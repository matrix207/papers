---
layout: post
title: "Dll write by C# use on VC++"
description: ""
category: windows
tags: []
---

##C#工程
1. __修改项目属性__  
>_属性_ -> _应用程序_ -> _输出类型_ -> _类库_  
2. __编译工程,生成类库文件__  
>_projectName.dll_

##VC++工程
1. __修改项目属性__  
>_配置属性_ -> _常规_ -> _项目默认值_ -> _公共语言运行时支持_ -> _公共语言运行时支持(/clr)_  
2. __加入代码__  

* _类库路径_  `#using "./bin/CSharp.dll"`
* _设定使用的命名空间_  `using namespace CSharpNamespaceSample;`
* _使用类成员和方法_  

_方法一:_

	void TestFunc()
	{
		CSharpClassSample ^cs = gcnew CSharpClassSample();

		cs->member_sample = 1;
		cs->func_sample();
	}

_方法二:_

	ref class A{
	public:
		static CSharpClassSample obj;
	};

	void TestFunc_1(void)
	{
		A::obj.member_sample = 1;
	}

	void TestFunc_2(void)
	{
		A::obj.func_sample();
	}
