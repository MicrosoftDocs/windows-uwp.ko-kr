---
author: msatranjr
title: "Windows 런타임 구성 요소"
description: "Windows 런타임 구성 요소는 C#, Visual Basic, JavaScript, C++ 등 모든 언어에서 인스턴스화하고 사용할 수 있는 자체 포함된 개체입니다."
ms.assetid: 55887622-828b-4318-87f2-25592268f7c1
translationtype: Human Translation
ms.sourcegitcommit: 4e9f3de68c44cf545ceee2efd99d9db8cab08676
ms.openlocfilehash: ddbfc30f765bdb1dacbf5c6b72255233fe134073

---

# Windows 런타임 구성 요소


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

Windows 런타임 구성 요소는 C#, Visual Basic, JavaScript, C++ 등 모든 언어에서 인스턴스화하고 사용할 수 있는 자체 포함된 개체입니다.

Visual Studio 및 C#, Visual Basic 또는 C++를 통해 UWP(유니버설 Windows 플랫폼) 앱에서 사용할 수 있는 Windows 런타임 구성 요소를 만들 수 있습니다.

| 항목 | 설명 |
|-------|-------------|
| [C++로 Windows 런타임 구성 요소 만들기](creating-windows-runtime-components-in-cpp.md) | 이 문서에서는 C++를 사용하여 JavaScript(또는 C#, Visual Basic, C++)를 사용해 작성된 유니버설 Windows 앱에서 호출할 수 있는 DLL인 Windows 런타임 구성 요소를 만드는 방법을 보여 줍니다. |
| [연습: C++로 기본적인 Windows 런타임 구성 요소를 만들고 JavaScript 또는 C에서 호출#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md) | 이 연습에서는 JavaScript, C# 또는 Visual Basic에서 호출할 수 있는 기본 Windows 런타임 구성 요소 DLL을 만드는 방법을 보여 줍니다. 이 연습을 시작하기 전에 쉽게 ref 클래스를 사용할 수 있게 해주는 ABI(추상 이진 인터페이스), ref 클래스, Visual C++ 구성 요소 확장 등의 개념을 이해해야 합니다. 자세한 내용은 [C++로 Windows 런타임 구성 요소 만들기](creating-windows-runtime-components-in-cpp.md) 및 [Visual C++ 언어 참조(C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx)를 참조하세요. |
| [C# 및 Visual Basic에서 Windows 런타임 구성 요소 만들기](creating-windows-runtime-components-in-csharp-and-visual-basic.md) | .NET Framework 4.5부터 관리 코드를 사용하여 Windows 런타임 구성 요소에 패키지된 Windows 런타임 형식을 직접 만들 수 있습니다. UWP(유니버설 Windows 플랫폼) 앱의 구성 요소를 C++, JavaScript, Visual Basic 또는 C#과 함께 사용할 수 있습니다. 이 문서에서는 구성 요소를 만들기 위한 규칙을 간략히 설명하고 Windows 런타임에 대한 .NET Framework 지원의 일부 측면을 설명합니다. 일반적으로 이 지원은 .NET Framework 프로그래머에게 투명하게 디자인되었습니다. 그러나 JavaScript 또는 C++를 사용하는 구성 요소를 만들 때는 이러한 언어와 Windows 런타임을 지원하는 방식의 차이점을 알아야 합니다. |
| [연습: 단순한 Windows 런타임 구성 요소를 만들고 JavaScript에서 이를 호출](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) | 이 연습에서는 Visual Basic 또는 C#과 함께 .NET Framework를 사용하여 Windows 런타임 구성 요소로 패키지된 고유한 Windows 런타임 형식을 만드는 방법 및 JavaScript를 사용하여 Windows용으로 빌드된 유니버설 Windows 앱에서 구성 요소를 호출하는 방법을 보여 줍니다. |
| [Windows 런타임 구성 요소에서 이벤트 발생](raising-events-in-windows-runtime-components.md) | Windows 런타임 구성 요소가 백그라운드 스레드(작업자 스레드)에서 사용자 정의 대리자 형식의 이벤트를 발생시키며 JavaScript가 이벤트를 받을 수 있게 하려는 경우 다음 방법 중 하나로 구현 및/또는 발생시킬 수 있습니다. | 
| [테스트용으로 로드하는 Windows 스토어 앱의 조정된 Windows 런타임 구성 요소](brokered-windows-runtime-components-for-side-loaded-windows-store-apps.md) | 이 항목에서는 Windows 10 업데이트 이상에서 지원되며, 쉽게 터치할 수 있는 .NET 앱에서 업무에 중요한 핵심 작업을 담당하는 기존 코드를 사용할 수 있도록 하는 기업 대상 기능에 대해 설명합니다.
 

 

 






<!--HONumber=Jul16_HO1-->


