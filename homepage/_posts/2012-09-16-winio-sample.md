---
layout: post
title: "WinIO Sample"
description: "The sample of use WinIO."
category: windows
tags: []
---

	// winio_sample.cpp

	#include <stdio.h>
	#include <tchar.h>

	#include <windows.h>

	typedef bool (*InitializeWinIo)();
	typedef void (*ShutdownWinIo)();
	typedef bool (*GetPortVal)(WORD wPortAddr, PDWORD pdwPortVal, BYTE bSize);
	typedef bool (*SetPortVal)(WORD wPortAddr, DWORD dwPortVal, BYTE bSize);
	typedef bool (*InstallWinIoDriver)(PWSTR pszWinIoDriverPath, bool IsDemandLoaded);

	InitializeWinIo		pInitializeWinIo	= NULL;
	ShutdownWinIo		pShutdownWinIo		= NULL;
	GetPortVal			pGetPortVal			= NULL;
	SetPortVal			pSetPortVal			= NULL;
	InstallWinIoDriver	pInstallWinIoDriver = NULL;

	HINSTANCE hinstLib = NULL;

	VOID SafeGetNativeSystemInfo(__out LPSYSTEM_INFO lpSystemInfo)
	{
		if (NULL==lpSystemInfo)	return;
		typedef VOID (WINAPI *LPFN_GetNativeSystemInfo)(LPSYSTEM_INFO lpSystemInfo);
		LPFN_GetNativeSystemInfo fnGetNativeSystemInfo = 
			(LPFN_GetNativeSystemInfo)GetProcAddress( GetModuleHandle(_T("kernel32")), "GetNativeSystemInfo");
		if (NULL != fnGetNativeSystemInfo)
		{
			fnGetNativeSystemInfo(lpSystemInfo);
		}
		else
		{
			GetSystemInfo(lpSystemInfo);
		}
	}

	int GetSystemBits()
	{
		SYSTEM_INFO si;
		SafeGetNativeSystemInfo(&si);
		if (si.wProcessorArchitecture == PROCESSOR_ARCHITECTURE_AMD64 ||
			si.wProcessorArchitecture == PROCESSOR_ARCHITECTURE_IA64 )
		{
			return 64;
		}
		return 32;
	}

	int LoadLib()
	{
		TCHAR *tcLib = NULL;
		tcLib = (32 == GetSystemBits()) ? TEXT("WinIo32.dll") : TEXT("WinIo64.dll");
		hinstLib = LoadLibrary(tcLib); 
		if (!hinstLib) 
		{
			wprintf(TEXT("Failed to load %s\n"), tcLib);
			return 1;
		}
		return 0;
	}

	void FreeLib()
	{
		if (hinstLib)
		{
			FreeLibrary(hinstLib);
			hinstLib = NULL;
		}
	}

	int GetFuncAddr()
	{
		if (hinstLib != NULL) 
		{ 
			pInitializeWinIo = (InitializeWinIo) GetProcAddress(hinstLib, "InitializeWinIo");
			if (!pInitializeWinIo) return 1;

			pShutdownWinIo = (ShutdownWinIo) GetProcAddress(hinstLib, "ShutdownWinIo");
			if (!pShutdownWinIo) return 1;

			pGetPortVal = (GetPortVal) GetProcAddress(hinstLib, "GetPortVal");
			if (!pGetPortVal) return 1;

			pSetPortVal = (SetPortVal) GetProcAddress(hinstLib, "SetPortVal");
			if (!pSetPortVal) return 1;

			pInstallWinIoDriver = (InstallWinIoDriver) GetProcAddress(hinstLib, "InstallWinIoDriver");
			if (!pInstallWinIoDriver) return 1;
		}

		return 0;
	}

	void MyGetPortVal(WORD wPortAddr, BYTE bSize)
	{
		DWORD dwPortVal = 0;
		pGetPortVal(wPortAddr, &dwPortVal, bSize);
		char tmp = (char)dwPortVal;
		printf("port:0x%x, value:0x%x value:0x%x \n", wPortAddr, dwPortVal, tmp);
	}

	int GetPortValLoop()
	{
		char *pBuf = NULL;
		char line[255] = {0};
		DWORD dwPortVal = 0;
		WORD wPortAddr = 0;
		BYTE bSize = 1;
		char bytePortVal = 0;

		printf("Enter get port value loop.\nType q can to quit application.\n");

		while(1)
		{
			printf("Please enter port address :");
			memset(line, 0, sizeof(line));
			pBuf = fgets(line, sizeof(line), stdin);

			// Type q and Enter to end loop
			if (!strcmp(pBuf,"q\n")) break;

			wPortAddr = atoi(pBuf);

			pGetPortVal(wPortAddr, &dwPortVal, bSize);

			bytePortVal = (char)dwPortVal;
			printf("port:0x%x, value:0x%x value:0x%x \n", wPortAddr, dwPortVal, bytePortVal);
		}

		getchar();
		return 0;
	}

	int _tmain(int argc, _TCHAR* argv[])
	{
		if (0 != LoadLib())
		{
			goto ExitApp;
		}

		if(0 != GetFuncAddr())
		{
			printf("Failed to get function address.\n");
			goto ExitApp;
		}

		if (!pInitializeWinIo())
		{
			printf("Failed to initialize winio.\n");
			goto ExitApp;
		}

		Sleep(1000);

		printf("start to get port value.\n");
		printf("press any key to continue.\n");
		getchar();
		
		GetPortValLoop();
			
		printf("Shutdown WinIo.\n");
		printf("press any key to continue.\n");
		getchar();
		pShutdownWinIo();

	ExitApp:
		FreeLib();
		printf("Press any key to continue.\n");
		getchar();
		return 0;
	}
