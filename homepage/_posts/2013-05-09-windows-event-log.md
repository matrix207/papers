---
layout: post
title: "windows event log"
description: ""
category: windows
tags: [windows]
---


## 1. Introduce

Application can use the Event logging API to report and view events.
For details on reporting events, see Reporting Events.
To view events that have been written to log files, see Querying for Event Source Messages
and Receiving Event Notification. 

You can find widows event log files(binary file) on C:\WINDOWS\system32\config\, list as below:

	C:\WINDOWS\system32\config\AppEvent.Evt
	C:\WINDOWS\system32\config\SecEvent.Evt
	C:\WINDOWS\system32\config\SysEvent.Evt

## 2. Program
### 2.1 Prepare work
#### 2.1.1 Write a message text file, name provider.mc, the follow show it

	; // MyEventProvider.mc 

	; // This is the header section.


	SeverityNames=(Success=0x0:STATUS_SEVERITY_SUCCESS
				   Informational=0x1:STATUS_SEVERITY_INFORMATIONAL
				   Warning=0x2:STATUS_SEVERITY_WARNING
				   Error=0x3:STATUS_SEVERITY_ERROR
				  )


	FacilityNames=(System=0x0:FACILITY_SYSTEM
				   Runtime=0x2:FACILITY_RUNTIME
				   Stubs=0x3:FACILITY_STUBS
				   Io=0x4:FACILITY_IO_ERROR_CODE
				  )

	LanguageNames=(English=0x409:MSG00409)


	; // The following are the categories of events.

	MessageIdTypedef=WORD

	MessageId=0x1
	SymbolicName=NETWORK_CATEGORY
	Language=English
	Network Events
	.

	MessageId=0x2
	SymbolicName=DATABASE_CATEGORY
	Language=English
	Database Events
	.

	MessageId=0x3
	SymbolicName=UI_CATEGORY
	Language=English
	UI Events
	.


	; // The following are the message definitions.

	MessageIdTypedef=DWORD

	MessageId=0x100
	Severity=Error
	Facility=Runtime
	SymbolicName=MSG_INVALID_COMMAND
	Language=English
	The command is not valid.
	.


	MessageId=0x101
	Severity=Error
	Facility=System
	SymbolicName=MSG_BAD_FILE_CONTENTS
	Language=English
	File %1 contains content that is not valid.
	.

	MessageId=0x102
	Severity=Warning
	Facility=System
	SymbolicName=MSG_RETRIES
	Language=English
	There have been %1 retries with %2 success! Disconnect from
	the server and try again later.
	.

	MessageId=0x103
	Severity=Informational
	Facility=System
	SymbolicName=MSG_COMPUTE_CONVERSION
	Language=English
	%1 %%4096 = %2 %%4097. 
	.


	; // The following are the parameter strings */


	MessageId=0x1000
	Severity=Success
	Facility=System
	SymbolicName=QUARTS_UNITS
	Language=English
	quarts%0
	.

	MessageId=0x1001
	Severity=Success
	Facility=System
	SymbolicName=GALLONS_UNITS
	Language=English
	gallons%0
	.

Use the following command to compile the message text file:

	mc -U provider.mc

It will generate 3 files: (the report event project use provider.h)

	provider.h
	provider.rc
	MSG0409.bin

Compile the resources file:

	rc provider.rc

Generate only one file, provider.res

Last use link to create resource-dll file:

	link -dll -noentry provider.res

Output file, provider.dll, with these library, event log viewer can show the 
the detail description by ID, Type ... etc.(at the follow, Receiving event 
notification use thid library)

### 2.2 Report event log

An example show how to report event.
Create a win32 console project, with empty file.
Then, add a reportevent.cpp to it, and add the follow code.

	#ifndef UNICODE
	#define UNICODE
	#endif
	#include <windows.h>
	#include <stdio.h>
	#include "provider.h"

	#pragma comment(lib, "advapi32.lib")

	#define PROVIDER_NAME L"MyEventProvider"

	// Hardcoded insert string for the event messages.
	CONST LPWSTR pBadCommand = L"The command that was not valid";
	CONST LPWSTR pFilename = L"c:\\folder\\file.ext";
	CONST LPWSTR pNumberOfRetries = L"3";
	CONST LPWSTR pSuccessfulRetries = L"0";
	CONST LPWSTR pQuarts = L"8";
	CONST LPWSTR pGallons = L"2";

	void wmain(void)
	{
		HANDLE hEventLog = NULL;
		LPWSTR pInsertStrings[2] = {NULL, NULL};
		DWORD dwEventDataSize = 0;

		// The source name (provider) must exist as a subkey of Application.
		hEventLog = RegisterEventSource(NULL, PROVIDER_NAME);
		if (NULL == hEventLog)
		{
			wprintf(L"RegisterEventSource failed with 0x%x.\n", GetLastError());
			goto cleanup;
		}

		// This event includes user-defined data as part of the event. The event message
		// does not use insert strings.
		dwEventDataSize = ((DWORD)wcslen(pBadCommand) + 1) * sizeof(WCHAR);
		if (!ReportEvent(hEventLog, EVENTLOG_ERROR_TYPE, UI_CATEGORY, MSG_INVALID_COMMAND, NULL, 0, dwEventDataSize, NULL, pBadCommand))
		{
			wprintf(L"ReportEvent failed with 0x%x for event 0x%x.\n", GetLastError(), MSG_INVALID_COMMAND);
			goto cleanup;
		}

		// This event uses insert strings.
		pInsertStrings[0] = pFilename;
		if (!ReportEvent(hEventLog, EVENTLOG_ERROR_TYPE, DATABASE_CATEGORY, MSG_BAD_FILE_CONTENTS, NULL, 1, 0, (LPCWSTR*)pInsertStrings, NULL))
		{
			wprintf(L"ReportEvent failed with 0x%x for event 0x%x.\n", GetLastError(), MSG_BAD_FILE_CONTENTS);
			goto cleanup;
		}

		// This event uses insert strings.
		pInsertStrings[0] = pNumberOfRetries;
		pInsertStrings[1] = pSuccessfulRetries;
		if (!ReportEvent(hEventLog, EVENTLOG_WARNING_TYPE, NETWORK_CATEGORY, MSG_RETRIES, NULL, 2, 0, (LPCWSTR*)pInsertStrings, NULL))
		{
			wprintf(L"ReportEvent failed with 0x%x for event 0x%x.\n", GetLastError(), MSG_RETRIES);
			goto cleanup;
		}

		// This event uses insert strings.
		pInsertStrings[0] = pQuarts;
		pInsertStrings[1] = pGallons;
		if (!ReportEvent(hEventLog, EVENTLOG_INFORMATION_TYPE, UI_CATEGORY, MSG_COMPUTE_CONVERSION, NULL, 2, 0, (LPCWSTR*)pInsertStrings, NULL))
		{
			wprintf(L"ReportEvent failed with 0x%x for event 0x%x.\n", GetLastError(), MSG_COMPUTE_CONVERSION);
			goto cleanup;
		}

		wprintf(L"All events successfully reported.\n");

	cleanup:

		if (hEventLog)
			DeregisterEventSource(hEventLog);
	}

### 2.3 View event log

go to <http://msdn.microsoft.com/en-us/library/aa363677(v=vs.85).aspx> 
get the source code of "Receiving Event Notification".

Notice that, change these two lines for your apply:  

	#define PROVIDER_NAME L"MyEventProvider"
	#define RESOURCE_DLL  L"<path>\\Provider.dll"


## 3. Questions
How to monitor multi event logs? And make it to be a component.  
I want to implement it, on this site you can find a first version project 
[Click here](https://github.com/matrix207/VC/tree/master/eventlog/EventLogMonitor)

But I think, it was a pool method to check each event name. Is there exist any 
good method to do it? I meet the same problem with this guy, 
[Unexpected behavior of WaitForMultipleObjects for events used in NotifyChangeEventLog](http://social.msdn.microsoft.com/Forums/en-US/windowssdk/thread/2c0d8708-4d07-47f9-87f4-c3f7141b55fd) 
I'm sad, why he don't told the detail info.

## 4. Reference
* [Using Event Logging](http://msdn.microsoft.com/en-us/library/aa363681%28v=vs.85%29.aspx)
* Reporting Events: <http://msdn.microsoft.com/en-us/library/aa363680(v=vs.85).aspx>
* Receiving Event Notification: <http://msdn.microsoft.com/en-us/library/aa363677(v=vs.85).aspx>
* [resource](http://angusj.com/resourcehacker/) a freeware utility to view, modify, rename, 
add delete and extract resources in 32bit & 64bit windows executable and resource files
* [Unexpected behavior of WaitForMultipleObjects for events used in NotifyChangeEventLog](http://social.msdn.microsoft.com/Forums/en-US/windowssdk/thread/2c0d8708-4d07-47f9-87f4-c3f7141b55fd)
* SNARE Auditing and EventLog Management: <http://sourceforge.net/projects/snare/files/Snare for Windows/>
