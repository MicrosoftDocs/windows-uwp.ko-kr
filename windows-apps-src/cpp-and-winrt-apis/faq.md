---
author: stevewhims
description: C++/WinRT를 통해 Windows 런타임 API를 작성하거나 사용하면서 가질 수 있는 질문에 대해 답변을 제공합니다.
title: C++/WinRT 질문과 대답
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 자주, 묻는, 질문, faq
ms.localizationpriority: medium
ms.openlocfilehash: 2a2ea3dddc592379199017408652cab0a2a68fbb
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5696478"
---
# <a name="frequently-asked-questions-about-cwinrt"></a>C++/WinRT 질문과 대답
작성 하 고 사용 하 여 Windows 런타임 Api를 사용 하는 방법에 대 한 될 수 있는 질문에 대답 [C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

> [!NOTE]
> 표시된 오류 메시지에 대한 질문이 있는 경우 [C++/WinRT 문제 해결](troubleshooting.md) 항목도 참조하세요.

## <a name="how-do-i-retarget-my-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>어떻게 대상을 수행 내 C + + Windows SDK의 최신 버전으로 WinRT 프로젝트?

참조 [방법을 대상을 C + + Windows SDK의 최신 버전으로 WinRT 프로젝트](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk).

## <a name="why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134"></a>새 프로젝트 컴파일되지 하는 이유 Visual Studio 2017을 사용 하 고 (15.8.0 버전 이상), 및 SDK 17134 버전

Visual Studio 2017을 사용 하는 경우 (15.8.0 버전 이상)를 대상으로 하는 Windows SDK 버전 10.0.17134.0 (Windows 10, 버전 1803) 한 다음 새로 생성 된 C + + WinRT 프로젝트 컴파일 오류가 있는 하지 못할 수도 있습니다 "C3861*오류: 'from_abi': 식별자 하지 발견*", 다른 오류 *base.h*에서 발생 합니다. 해결 방법은 대상 중 하나는 이후 (자세한 준수) 버전의 Windows SDK 또는 프로젝트 속성 집합 **C/c + +** > **언어** > **적합성 모드: 아니요** (또한 경우 **허용 /-** 프로젝트 속성 **에에서 표시 됩니다 C/C++** >  **추가 옵션****명령줄** 다음 삭제).

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsixhttpsakamscppwinrtvsix"></a>[C++/WinRT Visual Studio Extension(VSIX)](https://aka.ms/cppwinrt/vsix)의 요구 사항은 무엇입니까?
[VSIX](https://aka.ms/cppwinrt/vsix)는 최소 Windows SDK 대상 버전인 10.0.17134.0(Windows 10, 버전 1803)이 적용됩니다. Visual Studio 2017(버전 15.6 이상. 15.7 이상 권장) 또한 필요합니다. `.vcxproj` 파일에서 `<PropertyGroup Label="Globals">`이 `<CppWinRTEnabled>true</CppWinRTEnabled>`로 설정되어 있는지 확인하여 VSIX를 사용하는 프로젝트를 식별할 수 있습니다. 자세한 내용은 [C++/WinRT에 대한 Visual Studio 지원 및 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)를 참조하세요.

## <a name="whats-a-runtime-class"></a>*런타임 클래스*가 무엇입니까?
런타임 클래스란 일반적으로 실행 가능한 경계에서 최신 COM 인터페이스를 통해 활성화 및 사용되는 형식을 말합니다. 하지만 이 형식을 구현하는 컴파일 단위 내에서도 런타임 클래스를 사용할 수 있습니다. 런타임 클래스는 IDL로 선언하며, C++/WinRT를 사용하여 표준 C++에서 구현할 수 있습니다.

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>*프로젝션된 형식*과 *구현체 형식*이란 무엇을 의미합니까?
오직 Windows 런타임 클래스만 *사용하는* 경우에는 *프로젝션된 형식*만 배타적으로 처리합니다. C++/WinRT는 *언어 프로젝션*이기 때문에 프로젝션된 형식이란 C++/WinRT를 사용해 C++로 *프로젝션되는* Windows 런타임의 표면 중 일부를 말합니다. 자세한 내용은 [C++/WinRT를 통한 API 사용](consume-apis.md)을 참조하세요.

*구현체 형식*에는 런타임 클래스의 구현체가 포함됩니다. 따라서 런타임 클래스를 구현하는 프로젝트에서만 사용할 수 있습니다. 런타임 클래스를 구현하는 프로젝트(Windows 런타임 구성 요소 프로젝트, 또는 XAML UI를 사용하는 프로젝트)를 작업 중일 때는 런타임 클래스의 구현체 형식과 C++/WinRT로 표시되는 런타임 클래스를 나타내는, 프로젝션된 형식 사이를 쉽게 구분할 수 있도록 하는 것이 중요합니다. 자세한 내용은 [C++/WinRT를 통한 API 작성](author-apis.md)을 참조하세요.

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>내 런타임 클래스의 IDL로 생성자를 선언해야 합니까?
런타임 클래스가 구현하는 컴파일 단위의 외부에서 사용할 목적으로 설계된 경우에 한해 그렇습니다(여기에서 런타임 클래스는 Windows 런타임 클라이언트 앱에서 일반 용도로 사용하는 Windows 런타임 구성 요소임). 생성자를 IDL로 선언하는 목적과 그 중요성에 대한 자세한 내용은 [런타임 클래스 생성자](author-apis.md#runtime-class-constructors)를 참조하세요.

## <a name="why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error"></a>링커에서 "LNK2019: Unresolved external symbol" 오류를 표시하는 이유가 무엇인가요?
확인되지 않은 기호가 **winrt** 네임스페이스의 C++/WinRT 프로젝션에 대한 Windows 네임스페이스 헤더의 API인 경우 포함한 API는 전방 선언되지만 그 정의는 아직 포함하지 않은 헤더에 있습니다. API 네임스페이스에 따라 명명한 헤더를 추가하고 다시 빌드하세요. 자세한 정보는 [C++/WinRT 프로젝션 헤더](consume-apis.md#cwinrt-projection-headers)를 참조하세요.

기호가 [RoInitialize](https://msdn.microsoft.com/library/br224650)같은 Windows 런타임 무료 함수인 경우 프로젝트에서 [WindowsApp.lib](/uwp/win32-and-com/win32-apis) 상위 라이브러리를 명시적으로 연결 해야 합니다. C++/WinRT 프로젝션은 이러한 무료(비 구성원) 함수 및 진입점에 따라 달라집니다. 응용 프로그램에 [C++/WinRT Visual Studio Extension(VSIX)](https://aka.ms/cppwinrt/vsix) 프로젝트 템플릿 중 하나를 사용하는 경우 `WindowsApp.lib`가 자동으로 연결됩니다. 그렇지 않은 경우 프로젝트 연결 설정을 사용하거나 소스 코드에서 포함할 수 있습니다.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

**WindowsApp.lib**에 연결 하 여 수는 링커 오류를 해결 하는 않는 것이 좋습니다. 하지만 제출 (따라서 것 성공적으로 수행 되려면 응용 프로그램에 대 한 가능한 된다는 의미의 유효성을 검사 하려면 Microsoft Store 및 Visual Studio에서 사용 하는 [Windows 앱 인증 키트](../debug-test-perf/windows-app-certification-kit.md) 테스트를 통과 하려면 응용 프로그램 필요가 없는 경우 Microsoft Store 수집), 다음 대신 대체 된 동적 연결 라이브러리를 연결할 수 있습니다. 예를 들어 링커 오류가 **CoIncrementMTAUsage** (또는 **WINRT_CoIncrementMTAUsage**)를 참조 하는 경우 다음 해결할 수 있습니다 하는 경우 (예를 들어 버전 **WindowsApp.lib** 에 표시 되지 않으면 반드시 필요한 Ole32.lib를 연결 하 여 함수 내보내기).

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>내가 [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable)을 구현해야 합니까? 만약 그렇다면 어떻게 구현합니까?
소멸자에서 리소스 공간을 확보하는 런타임 클래스가 있다고 가정할 때, 이 런타임 클래스가 구현하는 컴파일 단위 외부에서 사용하도록 설계된 경우에는(여기에서 런타임 클래스는 Windows 런타임 클라이언트 앱에서 일반 용도로 사용하는 Windows 런타임 구성 요소임) 결정적 완료(deterministic finalization)가 부족한 언어를 기준으로 런타임 클래스의 사용을 지원할 수 있도록 **IClosable**을 구현하는 것이 바람직합니다. 소멸자가 호출되든, [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.Close)가 호출되든, 혹은 둘 다 호출되든 상관없이 리소스 공간이 확보되는지 확인하세요. **IClosable::Close**는 임의 횟수로 호출될 수 있습니다.

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosablewindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>사용할 런타임 클래스에 대해 [**IClosable::Close**](/uwp/api/windows.foundation.iclosable#Windows_Foundation_IClosable_Close_)를 호출해야 합니까?
**IClosable**은 결정적 완료가 부족한 언어를 지원하기 위해 존재합니다. 따라서 C++/WinRT에서는 **IClosable::Close**를 호출하지 않아도 됩니다. 단, 매우 드물지만 종료 경합 또는 프로세스 중단과 관련된 경우는 예외입니다. 한 예로 **Windows.UI.Composition** 형식을 사용한다면 C++/WinRT 래퍼의 소멸이 유효할 수 있는 대안으로서 개체를 설정된 시퀀스로 삭제해야 하는 경우가 발생할 수도 있습니다.

## <a name="can-i-use-llvmclang-to-compile-with-cwinrt"></a>C++/WinRT로 컴파일하기 위해 LLVM/Clang을 사용할 수 있나요?
C++/WinRT에 LLVM 및 Clang 도구 체인은 지원되지 않지만 C++/WinRT의 표준 성능을 검사하기 위해 내부적으로 이를 활용합니다. 예를 들어, Microsoft가 내부적으로 수행하는 작업을 에뮬레이션하려는 경우 아래에 설명된 것과 같은 실험을 시도할 수 있습니다.

[LLVM 다운로드 페이지](https://releases.llvm.org/download.html)로 이동하여 **LLVM 6.0.0 다운로드** > **사전 작성된 바이너리**를 찾고 **Windows용 Clang(64비트)** 을 다운로드합니다. 설치하는 동안 명령 프롬프트에서 호출할 수 있도록 LLVM을 PATH 시스템 변수에 추가할 수 있습니다. 이 실험의 목적을 위해 모든 "MSBuild 도구 디렉터리를 찾지 못했습니다." 또는 "MSVC 통합을 설치하지 못했습니다." 오류가 표시되는 경우 무시할 수 있습니다. LLVM/Clang을 호출하는 방법은 여러 가지가 있습니다. 아래의 예제는 이 중 한 가지 방법을 보여 줍니다.

```
C:\ExperimentWithLLVMClang>type main.cpp
// main.cpp
#pragma comment(lib, "windowsapp")
#pragma comment(lib, "ole32")

#include <winrt/Windows.Foundation.h>
#include <stdio.h>
#include <iostream>

using namespace winrt;

int main()
{
    winrt::init_apartment();
    Windows::Foundation::Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    std::wcout << rssFeedUri.Domain().c_str() << std::endl;
}

C:\ExperimentWithLLVMClang>clang-cl main.cpp /EHsc /I ..\.. -Xclang -std=c++17 -Xclang -Wno-delete-non-virtual-dtor -o app.exe

C:\ExperimentWithLLVMClang>app
windows.com
```

C++/WinRT가 C++17 표준의 기능을 사용하기 때문에 해당 지원을 얻기 위해 아무 컴파일러 플래그를 사용해야 합니다. 그러한 플래그는 컴파일러에 따라 다릅니다.

Visual Studio는 C++/WinRT를 지원하고 추천하는 개발 도구입니다. [C++/WinRT에 대한 Visual Studio 지원 및 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)를 참조하세요.

## <a name="why-doesnt-the-generated-implementation-function-for-a-read-only-property-have-the-const-qualifier"></a>읽기 전용 속성에 대해 생성 된 구현 함수 없는 이유는 `const` 한정자?

이미 예상 하시 [MIDL 3.0](/uwp/midl-3/)의 읽기 전용 속성을 선언 합니다 `cppwinrt.exe` 구현 함수를 생성할 수 있는 도구 `const`-자격을 갖춘 (const 함수 const를 *이* 포인터를 처리 하는 데 사용).

가능 하면 const를 사용 하 여 확실히 권장 하지만 `cppwinrt.exe` 도구 자체 어떤 구현에 대 한 기능 지역과 수 const 및 않았을 하는 이유를 시도 하지 않습니다. 이 예제와 같이 const 확인 구현 함수를 선택할 수 있습니다.

```cppwinrt
struct MyStringable : winrt::implements<MyStringable, winrt::Windows::Foundation::IStringable>
{
    winrt::hstring ToString() const
    {
        return L"MyStringable";
    }
};
```

제거할 수 있습니다 `const` **ToString** 한정자 사용 해야 할 결정 구현에서 일부 개체 상태를 변경 해야 합니다. 하지만 각각에 멤버의.pgd const 또는 비 const 함수를 만듭니다. 즉, 하지 오버 로드를 구현 하는 함수에서 `const`.

기타 다른 구현 기능 외에도 배치 const 위치 기능을 그림이 Windows 런타임 함수 프로젝션을 합니다. 이 코드를 것이 좋습니다.

```cppwinrt
int main()
{
    winrt::Windows::Foundation::IStringable s{ winrt::make<MyStringable>() };
    auto result{ s.ToString() };
}
```

**ToString** 위의에 대 한 호출을 Visual Studio에서 **선언 이동** 명령을 표시 하는 Windows 런타임 **IStringable::ToString** 의 프로젝션에 C + + WinRT 다음과 같이 표시 됩니다.

```
winrt::hstring ToString() const;
```

프로젝션 기능은 자격을 얻으려면 구현의 선택 하는 방법에 관계 없이 const 합니다. 내부적으로 프로젝션 응용 프로그램 이진 인터페이스 (ABI)는 시간과 COM 인터페이스 포인터를 통해 호출을 호출합니다. 프로젝션 된 **ToString** 상호 작용 하는 유일한 상태는 해당 COM 인터페이스 포인터; 있고 확실히 함수 const 이므로 해당 포인터를 수정할 필요가 없습니다. 이렇게 하면은 **IStringable**에 대 한 참조를 통해 호출할 **IStringable** 참조에 대 한 변경 되지 않습니다 및 const 있어도 **ToString** 을 호출할 수 있음을 보장 보증 있습니다.

이해 하는 이러한 예제 `const` 구현 세부 C + + WinRT 프로젝션 및 구현 합니다. 코드 관리의 편의 위한으로 구성 합니다. 문제는 `const` COM 및 Windows 런타임 ABI (멤버 함수)에 대 한에서.

## <a name="do-you-have-any-recommendations-for-decreasing-the-code-size-for-cwinrt-binaries"></a>C +에 대 한 코드 크기 감소에 대 한 권장 사항이 있습니까 + WinRT 바이너리?

Windows 런타임 개체를 사용 하 여 생성 하는 데 필요한 것 보다 더 많은 이진 코드 응용 프로그램에 부정적인 영향을 가질 수 있으므로 아래 표시 된 코딩 패턴 하면 안 됩니다.

```cppwinrt
anobject.b().c().d();
anobject.b().c().e();
anobject.b().c().f();
```

Windows 런타임 환경에서 컴파일러는의 값을 캐시할 수 `c()` 또는 전화를 간접 참조를 통해 호출 되는 각 메서드에 대 한 인터페이스 ('. '). 사용자가 개입 하지 않는 한는 더 많은 가상 호출 및 참조 카운트 오버 헤드가 발생 합니다. 위의 패턴 엄격 하 게 필요한 배의 코드를 쉽게 생성할 수 있습니다. 대신, 수 어디서 나 아래 표시 된 패턴을 선호 합니다. 훨씬 적은 코드를 생성 및 런타임 성능이 크게도 향상 시킬 수 있습니다.

```cppwinrt
auto a{ anobject.b().c() };
a.d();
a.e();
a.f();
```

위에 표시 된 권장된 패턴을 적용할 뿐만 아니라 C + + WinRT 있지만 모든 Windows 런타임 언어 프로젝션입니다.

> [!NOTE]
> 이 항목에는 질문에 응답 하지 않은 경우 [Visual Studio c + + 개발자 커뮤니티](https://developercommunity.visualstudio.com/spaces/62/index.html)방문 하 여 또는 사용 하 여 도움말을 검색할 수는 [ `c++-winrt` Stack Overflow에서 태그](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt)합니다.
