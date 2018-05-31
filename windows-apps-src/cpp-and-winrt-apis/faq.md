---
author: stevewhims
description: C++/WinRT를 통해 Windows 런타임 API를 작성하거나 사용하면서 가질 수 있는 질문에 대해 답변을 제공합니다.
title: C++/WinRT 질문과 대답
ms.author: stwhi
ms.date: 04/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 자주, 묻는, 질문, faq
ms.localizationpriority: medium
ms.openlocfilehash: aad5c5ed2123af39ebb6aff0c9098586ce958196
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832038"
---
# <a name="frequently-asked-questions-about-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 질문과 대답
> [!NOTE]
> **일부 정보는 상업용으로 출시되기 전에 상당 부분 수정될 수 있는 시험판 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.**

C++/WinRT를 통해 Windows 런타임 API를 작성하거나 사용하면서 가질 수 있는 질문에 대해 답변을 제공합니다.

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsixhttpsakamscppwinrtvsix"></a>C++/WinRT [Visual Studio Extension(VSIX)](https://aka.ms/cppwinrt/vsix)의 요구 사항은 무엇입니까?
[VSIX](https://aka.ms/cppwinrt/vsix)는 최소 Windows SDK 대상 버전인 10.0.17134.0(Windows 10, 버전 1803)이 적용됩니다. 또한 Visual Studio 2017 버전 15.6 이상이 필요합니다. `.vcxproj` 파일에서 `<PropertyGroup Label="Globals">`이 `<CppWinRTEnabled>true</CppWinRTEnabled>`로 설정되어 있는지 확인하여 VSIX를 사용하는 프로젝트를 식별할 수 있습니다. 자세한 내용은 [C++/WinRT에 대한 Visual Studio 지원 및 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)를 참조하세요.

## <a name="whats-a-runtime-class"></a>*런타임 클래스*가 무엇입니까?
런타임 클래스란 일반적으로 실행 가능한 경계에서 최신 COM 인터페이스를 통해 활성화 및 사용되는 형식을 말합니다. 하지만 이 형식을 구현하는 컴파일 단위 내에서도 런타임 클래스를 사용할 수 있습니다. 런타임 클래스는 IDL로 선언하며, C++/WinRT를 사용하여 표준 C++에서 구현할 수 있습니다.

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>*프로젝션된 형식*과 *구현체 형식*이란 무엇을 의미합니까?
오직 Windows 런타임 클래스만 *사용하는* 경우에는 *프로젝션된 형식*만 배타적으로 처리합니다. C++/WinRT는 *언어 프로젝션*이기 때문에 프로젝션된 형식이란 C++/WinRT를 사용해 C++로 *프로젝션되는* Windows 런타임의 표면 중 일부를 말합니다. 자세한 내용은 [C++/WinRT를 통한 API 사용](consume-apis.md)을 참조하세요.

*구현체 형식*에는 런타임 클래스의 구현체가 포함됩니다. 따라서 런타임 클래스를 구현하는 프로젝트에서만 사용할 수 있습니다. 런타임 클래스를 구현하는 프로젝트(Windows 런타임 구성 요소 프로젝트, 또는 XAML UI를 사용하는 프로젝트)를 작업 중일 때는 런타임 클래스의 구현체 형식과 C++/WinRT로 표시되는 런타임 클래스를 나타내는, 프로젝션된 형식 사이를 쉽게 구분할 수 있도록 하는 것이 중요합니다. 자세한 내용은 [C++/WinRT를 통한 API 작성](author-apis.md)을 참조하세요.

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>내가 [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable)을 구현해야 합니까? 만약 그렇다면 어떻게 구현합니까?
소멸자에서 리소스 공간을 확보하는 런타임 클래스가 있다고 가정할 때, 이 런타임 클래스가 구현하는 컴파일 단위 외부에서 사용하도록 설계된 경우에는(여기에서 런타임 클래스는 Windows 런타임 클라이언트 앱에서 일반 용도로 사용하는 Windows 런타임 구성 요소임) 결정적 완료(deterministic finalization)가 부족한 언어를 기준으로 런타임 클래스의 사용을 지원할 수 있도록 **IClosable**을 구현하는 것이 바람직합니다. 소멸자가 호출되든, [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.Close)가 호출되든, 혹은 둘 다 호출되든 상관없이 리소스 공간이 확보되는지 확인하세요. **IClosable::Close**는 임의 횟수로 호출될 수 있습니다.

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosablewindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>사용할 런타임 클래스에 대해 [**IClosable::Close**](/uwp/api/windows.foundation.iclosable#Windows_Foundation_IClosable_Close_)를 호출해야 합니까?
**IClosable**은 결정적 완료가 부족한 언어를 지원하기 위해 존재합니다. 따라서 C++/WinRT에서는 **IClosable::Close**를 호출하지 않아도 됩니다. 단, 매우 드물지만 종료 경합 또는 프로세스 중단과 관련된 경우는 예외입니다. 한 예로 **Windows.UI.Composition** 형식을 사용한다면 C++/WinRT 래퍼의 소멸이 유효할 수 있는 대안으로서 개체를 설정된 시퀀스로 삭제해야 하는 경우가 발생할 수도 있습니다.

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>내 런타임 클래스의 IDL로 생성자를 선언해야 합니까?
런타임 클래스가 구현하는 컴파일 단위의 외부에서 사용할 목적으로 설계된 경우에 한해 그렇습니다(여기에서 런타임 클래스는 Windows 런타임 클라이언트 앱에서 일반 용도로 사용하는 Windows 런타임 구성 요소임). 생성자를 IDL로 선언하는 목적과 그 중요성에 대한 자세한 내용은 [런타임 클래스 생성자](author-apis.md#runtime-class-constructors)를 참조하세요.