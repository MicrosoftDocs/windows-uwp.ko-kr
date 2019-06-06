---
description: C++/WinRT를 통해 Windows 런타임 API를 작성하거나 사용하면서 가질 수 있는 질문에 대해 답변을 제공합니다.
title: C++/WinRT 질문과 대답
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 자주, 묻는, 질문, faq
ms.localizationpriority: medium
ms.openlocfilehash: 914cf884b97d14af523cc61b0fcce719104783ba
ms.sourcegitcommit: 1f39b67f2711b96c6b4e7ed7107a9a47127d4e8f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66721691"
---
# <a name="frequently-asked-questions-about-cwinrt"></a>C++/WinRT 질문과 대답
작성 하 고 사용 하 여 Windows 런타임 Api를 사용 하는 방법에 대 한 있을 수 있는 질문에 답변 [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)합니다.

> [!NOTE]
> 표시된 오류 메시지에 대한 질문이 있는 경우 [C++/WinRT 문제 해결](troubleshooting.md) 항목도 참조하세요.

## <a name="how-do-i-retarget-my-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>대상을 수행 하는 방법을 내 C++Windows SDK의 최신 버전으로 /WinRT 프로젝트?
참조 [대상을 다시 지정 하는 방법에 C++Windows SDK의 최신 버전으로 /WinRT 프로젝트](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)합니다.

## <a name="why-wont-my-new-project-compile-now-that-ive-moved-to-cwinrt-20"></a>왜 내 새 프로젝트 컴파일되지 이동할 했습니다 했으므로 C++WinRT 2.0?
변경 내용 (주요 변경 내용 포함)의 전체 집합을 참조 하세요 [뉴스 및 변경 내용에 C++WinRT 2.0](news.md#news-and-changes-in-cwinrt-20)합니다. 예를 들어, 범위 기반을 사용 하는 경우 `for` Windows 런타임 컬렉션에 대해 다음 이제 해야 `#include <winrt/Windows.Foundation.Collections.h>`합니다.

## <a name="why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134"></a>내 새 프로젝트 컴파일되지 이유는? Visual Studio 2017을 사용 하 고 (15.8.0 버전 이상), 및 SDK 버전 17134
Visual Studio 2017을 사용 하는 경우 (15.8.0 버전 이상), 한 다음 새로 만든된 Windows SDK (Windows 10, 버전 1803) 10.0.17134.0 버전을 대상으로 하 고 C++WinRT 프로젝트는 오류가 발생 하 여 컴파일하는 데 실패할 수 있습니다 / "*오류 C3861: 'from_abi': 식별자를 찾을 수 없습니다*", 및에서 발생 하는 다른 오류로 *base.h*합니다. 두 대상에는 솔루션을 이상 (더 부합) 버전의 Windows SDK 또는 프로젝트 속성 설정 **C /C++**  > **언어**  >   **준수 모드: 더** (또한 경우 **/ permissive-** 프로젝트 속성에 표시 됩니다 **C /C++**  > **명령줄** 아래 **추가 옵션**, 삭제).

## <a name="how-do-i-resolve-the-build-error-the-cwinrt-vsix-no-longer-provides-project-build-support--please-add-a-project-reference-to-the-microsoftwindowscppwinrt-nuget-package"></a>빌드 오류를 해결 하는 방법 "을 C++WinRT VSIX 프로젝트 빌드 지원을 제공 하지 않습니다.  Microsoft.Windows.CppWinRT Nuget 패키지에 프로젝트 참조를 추가 하십시오 "?
설치 합니다 **Microsoft.Windows.CppWinRT** 프로젝트에 NuGet 패키지. 자세한 내용은 참조 하세요 [이전 버전의 VSIX 확장](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)합니다.

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsix"></a>C +에 대 한 요구 사항은 무엇입니까 + WinRT Visual Studio 확장 (VSIX)?
버전에 대해서 1.0.190128.4 VSIX 확장의 이상 [Visual Studio 지원에 대 한 C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)합니다. 다른 버전 참조 [이전 버전의 VSIX 확장](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)합니다.

## <a name="whats-a-runtime-class"></a>*런타임 클래스*가 무엇입니까?
런타임 클래스란 일반적으로 실행 가능한 경계에서 최신 COM 인터페이스를 통해 활성화 및 사용되는 형식을 말합니다. 하지만 이 형식을 구현하는 컴파일 단위 내에서도 런타임 클래스를 사용할 수 있습니다. 런타임 클래스는 IDL로 선언하며, C++/WinRT를 사용하여 표준 C++에서 구현할 수 있습니다.

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>*프로젝션된 형식*과 *구현체 형식*이란 무엇을 의미합니까?
오직 Windows 런타임 클래스만 *사용하는* 경우에는 *프로젝션된 형식*만 배타적으로 처리합니다. C++/WinRT는 *언어 프로젝션*이기 때문에 프로젝션된 형식이란 C++/WinRT를 사용해 C++로 *프로젝션되는* Windows 런타임의 표면 중 일부를 말합니다. 자세한 내용은 참조 하세요. [사용 하 여 Api 사용 C++/WinRT](consume-apis.md)합니다.

*구현체 형식*에는 런타임 클래스의 구현체가 포함됩니다. 따라서 런타임 클래스를 구현하는 프로젝트에서만 사용할 수 있습니다. 런타임 클래스를 구현하는 프로젝트(Windows 런타임 구성 요소 프로젝트, 또는 XAML UI를 사용하는 프로젝트)를 작업 중일 때는 런타임 클래스의 구현체 형식과 C++/WinRT로 표시되는 런타임 클래스를 나타내는, 프로젝션된 형식 사이를 쉽게 구분할 수 있도록 하는 것이 중요합니다. 자세한 내용은 [C++/WinRT를 통한 API 작성](author-apis.md)을 참조하세요.

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>내 런타임 클래스의 IDL로 생성자를 선언해야 합니까?
런타임 클래스가 구현하는 컴파일 단위의 외부에서 사용할 목적으로 설계된 경우에 한해 그렇습니다(여기에서 런타임 클래스는 Windows 런타임 클라이언트 앱에서 일반 용도로 사용하는 Windows 런타임 구성 요소임). 생성자를 IDL로 선언하는 목적과 그 중요성에 대한 자세한 내용은 [런타임 클래스 생성자](author-apis.md#runtime-class-constructors)를 참조하세요.

## <a name="why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error"></a>이유는 링커 쓰면서를 "LNK2019: Unresolved external symbol"오류 인가요?
확인되지 않은 기호가 **winrt** 네임스페이스의 C++/WinRT 프로젝션에 대한 Windows 네임스페이스 헤더의 API인 경우 포함한 API는 전방 선언되지만 그 정의는 아직 포함하지 않은 헤더에 있습니다. API 네임스페이스에 따라 명명한 헤더를 추가하고 다시 빌드하세요. 자세한 정보는 [C++/WinRT 프로젝션 헤더](consume-apis.md#cwinrt-projection-headers)를 참조하세요.

확인 되지 않은 기호는 Windows 런타임 사용 가능한 함수와 같은 [RoInitialize](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roinitialize)를 명시적으로 링크 해야 합니다 [WindowsApp.lib](/uwp/win32-and-com/win32-apis) 포괄적인 라이브러리 프로젝트에서. C++/WinRT 프로젝션은 이러한 무료(비 구성원) 함수 및 진입점에 따라 달라집니다. 응용 프로그램에 [C++/WinRT Visual Studio Extension(VSIX)](https://aka.ms/cppwinrt/vsix) 프로젝트 템플릿 중 하나를 사용하는 경우 `WindowsApp.lib`가 자동으로 연결됩니다. 그렇지 않은 경우 프로젝트 연결 설정을 사용하거나 소스 코드에서 포함할 수 있습니다.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

연결 하 여 수행할 수 있는 링커 오류를 해결 하는 것 **WindowsApp.lib** 정적 링크 라이브러리를 대체 하는 대신 그렇지 않은 경우 응용 프로그램 전달 하지는 [Windows 앱 인증 키트](../debug-test-perf/windows-app-certification-kit.md) 제출 (따라서 수 없기 응용 프로그램이 성공적으로 Microsoft Store 수집 하는 의미) 유효성을 검사 하려면 Visual Studio 및 Microsoft Store 사용 하는 테스트 합니다.

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>내가 [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable)을 구현해야 합니까? 만약 그렇다면 어떻게 구현합니까?
소멸자에서 리소스 공간을 확보하는 런타임 클래스가 있다고 가정할 때, 이 런타임 클래스가 구현하는 컴파일 단위 외부에서 사용하도록 설계된 경우에는(여기에서 런타임 클래스는 Windows 런타임 클라이언트 앱에서 일반 용도로 사용하는 Windows 런타임 구성 요소임) 결정적 완료(deterministic finalization)가 부족한 언어를 기준으로 런타임 클래스의 사용을 지원할 수 있도록 **IClosable**을 구현하는 것이 바람직합니다. 소멸자가 호출되든, [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close)가 호출되든, 혹은 둘 다 호출되든 상관없이 리소스 공간이 확보되는지 확인하세요. **IClosable::Close**는 임의 횟수로 호출될 수 있습니다.

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>사용할 런타임 클래스에 대해 [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close)를 호출해야 합니까?
**IClosable**은 결정적 완료가 부족한 언어를 지원하기 위해 존재합니다. 따라서 C++/WinRT에서는 **IClosable::Close**를 호출하지 않아도 됩니다. 단, 매우 드물지만 종료 경합 또는 프로세스 중단과 관련된 경우는 예외입니다. 한 예로 **Windows.UI.Composition** 형식을 사용한다면 C++/WinRT 래퍼의 소멸이 유효할 수 있는 대안으로서 개체를 설정된 시퀀스로 삭제해야 하는 경우가 발생할 수도 있습니다.

## <a name="can-i-use-llvmclang-to-compile-with-cwinrt"></a>C++/WinRT로 컴파일하기 위해 LLVM/Clang을 사용할 수 있나요?
C++/WinRT에 LLVM 및 Clang 도구 체인은 지원되지 않지만 C++/WinRT의 표준 성능을 검사하기 위해 내부적으로 이를 활용합니다. 예를 들어, Microsoft가 내부적으로 수행하는 작업을 에뮬레이션하려는 경우 아래에 설명된 것과 같은 실험을 시도할 수 있습니다.

[LLVM 다운로드 페이지](https://releases.llvm.org/download.html)로 이동하여 **LLVM 6.0.0 다운로드** > **사전 작성된 바이너리**를 찾고 **Windows용 Clang(64비트)** 을 다운로드합니다. 설치하는 동안 명령 프롬프트에서 호출할 수 있도록 LLVM을 PATH 시스템 변수에 추가할 수 있습니다. 이 실험의 목적을 위해 모든 "MSBuild 도구 디렉터리를 찾지 못했습니다." 또는 "MSVC 통합을 설치하지 못했습니다." 오류가 표시되는 경우 무시할 수 있습니다. LLVM/Clang을 호출하는 방법은 여러 가지가 있습니다. 아래의 예제는 이 중 한 가지 방법을 보여 줍니다.

```cmd
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

Visual Studio는 C++/WinRT를 지원하고 추천하는 개발 도구입니다. 참조 [Visual Studio 지원에 대 한 C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)합니다.

## <a name="why-doesnt-the-generated-implementation-function-for-a-read-only-property-have-the-const-qualifier"></a>읽기 전용 속성에 대해 생성 된 구현을 함수 없는 이유는 `const` 한정자?
읽기 전용 속성을 선언 하는 경우 [MIDL 3.0](/uwp/midl-3/)를 예상할 수는 `cppwinrt.exe` 구현 하는 함수를 생성할 수 있는 도구 `const`-정규화 된 (const 함수 처리는 *이* const로 포인터).

물론 가능 하면 상수를 사용 하 여 권장 하지만 `cppwinrt.exe` 도구 자체는 구현에 대 한 함수 있다고 볼 수 있습니다, const 및 않을 하는 이유를 시도 하지 않습니다. 이 예제와 같이 const 확인 하는 구현 함수 중 하나를 선택할 수 있습니다.

```cppwinrt
struct MyStringable : winrt::implements<MyStringable, winrt::Windows::Foundation::IStringable>
{
    winrt::hstring ToString() const
    {
        return L"MyStringable";
    }
};
```

제거할 수 있습니다 `const` 한정자 **ToString** 구현에서 일부 개체 상태를 변경 해야 한다고 결정 해야 합니다. 하지만 각 멤버 둘 다 const 또는 비 const 함수를 확인 합니다. 즉, 오버 로드 되지 않도록 구현 함수를에서 `const`합니다.

기타 다른 구현 함수를 외에도 배치 const 위치에 그림은 Windows 런타임 함수 프로젝션의 합니다. 이 코드를 것이 좋습니다.

```cppwinrt
int main()
{
    winrt::Windows::Foundation::IStringable s{ winrt::make<MyStringable>() };
    auto result{ s.ToString() };
}
```

에 대 한 호출에 대 한 **ToString** 위의 합니다 **선언으로 이동** Visual Studio에서 명령에 따르면 Windows 런타임의 프로젝션 **IStringable::ToString** C++/WinRT 다음과 같이 표시 됩니다.

```cppwinrt
winrt::hstring ToString() const;
```

프로젝션에는 함수는 해당 구현 되려면 선택 하는 방법에 관계 없이 상수입니다. 내부적으로 프로젝션 응용 프로그램 이진 인터페이스 ABI ()에 COM 인터페이스 포인터를 통해 호출 하는 금액을 호출합니다. 유일한 상태를 **ToString** 상호 작용 하는 COM 인터페이스 포인터 이며 있고 확실히 함수 const 이므로 해당 포인터를 수정할 필요가 없습니다. 이렇게 하면 모든 항목에 대 한 변경 되지 않는 확인을 갖고 합니다 **IStringable** 참조를 통해 호출 하는 호출할 수 있음을 보장 **ToString** 는에대한const참조를사용하더라도 **IStringable**합니다.

이해 하는 이러한 예가 `const` 의 구현 세부 정보는 C++/WinRT 프로젝션 및 구현의 혜택에 대 한 코드 관리를 구성 합니다. 문제는 `const` COM 또는 Windows 런타임 ABI (예: 멤버 함수)에 있습니다.

## <a name="do-you-have-any-recommendations-for-decreasing-the-code-size-for-cwinrt-binaries"></a>C +에 대 한 코드 크기를 줄이기 위해 권장 사항을 해야 + WinRT 이진?
Windows 런타임 개체에서 작업할 때 응용 프로그램에 부정적인 영향을 생성 하는 데 필요한 것 보다 많은 이진 코드를 발생 시켜 가질 수 있으므로 아래 표시 된 코딩 패턴을 피해 야 합니다.

```cppwinrt
anobject.b().c().d();
anobject.b().c().e();
anobject.b().c().f();
```

Windows 런타임 환경에서 컴파일러는 값을 캐시할 수 `c()` 또는 간접 참조를 통해 호출 되는 각 메서드에 대 한 인터페이스 ('. '). 사용자가 개입할 경우가 아니면 결과는 더 많은 가상 호출 및 오버 헤드를 계산 하는 참조 합니다. 위의 패턴 엄격 하 게 필요한 두 배 많은 코드를 쉽게 생성할 수 없습니다. 대신 아무 곳에 나 있습니다 아래 표시 된 패턴을 선호 합니다. 훨씬 더 적은 코드를 생성 및 런타임 성능을 크게 향상 시킬 수 있습니다.

```cppwinrt
auto a{ anobject.b().c() };
a.d();
a.e();
a.f();
```

위에 표시 된 권장된 패턴 적용만 없습니다 C++/WinRT 모든 Windows 런타임 언어 프로젝션 하도록 합니다.

## <a name="how-do-i-turn-a-string-into-a-typemdashfor-navigation-for-example"></a>켜는 방법 문자열 형식으로&mdash;예제에 대 한 탐색을 위한?
끝에 [탐색 보기 코드 예제에서는](/windows/uwp/design/controls-and-patterns/navigationview#code-example) (대부분에 C#)는 C++이 작업을 수행 하는 방법을 보여주는 /WinRT 코드 조각입니다.

> [!NOTE]
> 이 항목에서는 질문, 대답 하지 않은 경우 방문 하 여 도움말을 볼 수 있습니다 합니다 [Visual Studio C++ 개발자 커뮤니티](https://developercommunity.visualstudio.com/spaces/62/index.html), 또는 사용 하 여는 [ `c++-winrt` Stack Overflow에서 태그](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt).
