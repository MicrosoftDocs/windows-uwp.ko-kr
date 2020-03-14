---
description: C++/WinRT를 통해 Windows 런타임 API를 작성하거나 사용하는 것과 관련하여 발생할 수 있는 질문에 대한 대답입니다.
title: C++/WinRT에 대해 자주 묻는 질문
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 자주, 묻는, 질문, faq
ms.localizationpriority: medium
ms.openlocfilehash: b0ec2c5a05e7c4e9309311fa22ad863d06597a53
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79209178"
---
# <a name="frequently-asked-questions-about-cwinrt"></a>C++/WinRT에 대해 자주 묻는 질문
[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)를 통해 Windows 런타임 API를 작성하거나 사용하는 방법과 관련된 질문과 대답입니다.

> [!NOTE]
> 표시된 오류 메시지에 대한 질문이 있는 경우 [C++/WinRT 문제 해결](troubleshooting.md) 항목도 참조하세요.

## <a name="how-do-i-retarget-my-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>C++/WinRT 프로젝트의 대상을 Windows SDK 최신 버전으로 변경하려면 어떻게 하나요?
[C++/WinRT 프로젝트의 대상을 Windows SDK 최신 버전으로 변경하는 방법](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)을 참조하세요.

## <a name="why-wont-my-new-project-compile-now-that-ive-moved-to-cwinrt-20"></a>이제 C++/WinRT 2.0으로 이동했는데도 새 프로젝트가 컴파일되지 않는 이유는 무엇인가요?
호환성이 손상되는 변경 내용을 비롯한 전체 변경 내용 세트는 [C++/WinRT 2.0의 뉴스 및 변경 내용](news.md#news-and-changes-in-cwinrt-20)을 참조하세요. 예를 들어 Windows 런타임 컬렉션에서 범위 기반의 `for`를 사용하는 경우 이제 `#include <winrt/Windows.Foundation.Collections.h>`가 필요합니다.

## <a name="why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134"></a>새 프로젝트가 컴파일되지 않는 이유는 무엇인가요? Visual Studio 2017(버전 15.8.0 이상) 및 SDK 버전 17134를 사용하고 있습니다.
Visual Studio 2017(버전 15.8.0 이상)을 사용 중이고 Windows SDK 버전 10.0.17134.0(Windows 10 버전, 1803)을 대상으로 하는 경우, 새로 만든 C++/WinRT 프로젝트가 컴파일되지 않고 “오류 C3861: ‘from_abi’: 식별자를 찾을 수 없음” 오류와 *base.h*에서 발생하는 기타 오류가 표시될 수 있습니다.  해결 방법으로, 보다 규칙에 맞는 Windows SDK 최신 버전을 대상으로 지정하거나, 프로젝트 속성 **C/C++**  > **언어** > **적합성 모드: 아니요**를 설정합니다. 또는 **추가 옵션** 아래의 프로젝트 속성 **C/C++**  > **명령줄**에 **/permissive-** 가 표시되는 경우 삭제합니다.

## <a name="how-do-i-resolve-the-build-error-the-cwinrt-vsix-no-longer-provides-project-build-support--please-add-a-project-reference-to-the-microsoftwindowscppwinrt-nuget-package"></a>빌드 오류 “C++/WinRT VSIX에서 더 이상 프로젝트 빌드 지원을 제공하지 않습니다.  Microsoft.Windows.CppWinRT Nuget 패키지에 프로젝트 참조를 추가하세요.”를 해결하려면 어떻게 하나요?
**Microsoft.Windows.CppWinRT** NuGet 패키지를 프로젝트에 설치합니다. 자세한 내용은 [이전 버전의 VSIX 확장](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)을 참조하세요.

## <a name="how-do-i-customize-the-build-support-in-the-nuget-package"></a>NuGet 패키지에서 빌드 지원을 사용자 지정하려면 어떻게 할까요??

C++/WinRT 빌드 지원(속성/대상)은 Microsoft.Windows.CppWinRT NuGet 패키지 [readme](https://github.com/microsoft/xlang/tree/master/src/package/cppwinrt/nuget/readme.md#customizing)에 문서화되어 있습니다.

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsix"></a>C++/WinRT VSIX(Visual Studio Extension)의 요구 사항은 무엇인가요?
VSIX 확장 버전 1.0.190128.4 이상의 경우 [Visual Studio의 C++/WinRT 지원](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)을 참조하세요. 다른 버전의 경우 [이전 버전의 VSIX 확장](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)을 참조하세요.

## <a name="whats-a-runtime-class"></a>*런타임 클래스*란 무엇인가요?
런타임 클래스는 일반적으로 실행 가능한 경계를 넘어서 최신 COM 인터페이스를 통해 활성화 및 사용할 수 있는 형식입니다. 하지만 런타임 클래스를 구현하는 컴파일 단위 내에서 사용할 수도 있습니다. 런타임 클래스는 IDL(Interface Definition Language)로 선언하며, C++/WinRT를 사용하여 표준 C++로 구현할 수 있습니다.

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>‘프로젝션된 형식’과 ‘구현 형식’은 무엇을 의미하나요?  
Windows 런타임 클래스(런타임 클래스)를 ‘사용’하기만 하는 경우에는 ‘프로젝션된 형식’만 처리하게 됩니다.   C++/WinRT는 ‘언어 프로젝션’이므로, 프로젝션된 형식이란 C++/WinRT를 사용하여 C++로 ‘프로젝션된’ Windows 런타임의 표면 중 일부를 말합니다.   자세한 내용은 [C++/WinRT를 통한 API 사용](consume-apis.md)을 참조하세요.

‘구현 형식’에는 런타임 클래스 구현이 포함되므로, 런타임 클래스를 구현하는 프로젝트에서만 사용할 수 있습니다.  런타임 클래스를 구현하는 프로젝트(Windows 런타임 구성 요소 프로젝트 또는 XAML UI를 사용하는 프로젝트)에서 작업 중인 경우 런타임 클래스의 구현 형식과 C++/WinRT에 프로젝션된 런타임 클래스를 나타내는 프로젝션된 형식 간의 차이점을 잘 아는 것이 중요합니다. 자세한 내용은 [C++/WinRT를 통한 API 작성](author-apis.md)을 참조하세요.

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>런타임 클래스의 IDL로 생성자를 선언해야 하나요?
런타임 클래스가 구현하는 컴파일 단위의 외부에서 사용하도록 설계된 경우에 한합니다(여기에서 런타임 클래스는 Windows 런타임 클라이언트 앱에서 일반 용도로 사용하는 Windows 런타임 구성 요소임). 생성자를 IDL로 선언하는 목적과 결과에 대한 자세한 내용은 [런타임 클래스 생성자](author-apis.md#runtime-class-constructors)를 참조하세요.

## <a name="why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error"></a>링커에서 “LNK2019: 확인되지 않은 외부 기호” 오류가 발생하는 이유는 무엇인가요?
확인되지 않은 기호가 **winrt** 네임스페이스에 있는 C++/WinRT 프로젝션의 Windows 네임스페이스 헤더 API인 경우 API는 포함한 헤더에서 정방향으로 선언되지만 해당 정의는 아직 포함하지 않은 헤더에 있습니다. API 네임스페이스를 따라 명명된 헤더를 추가하고 다시 빌드합니다. 자세한 내용은 [C++/WinRT 프로젝션 헤더](consume-apis.md#cwinrt-projection-headers)를 참조하세요.

확인되지 않은 기호가 [RoInitialize](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roinitialize)와 같은 Windows 런타임 프리 함수인 경우 프로젝트에서 [WindowsApp.lib](/uwp/win32-and-com/win32-apis) 상위 라이브러리를 명시적으로 연결해야 합니다. C++/WinRT 프로젝션은 이러한 프리(비멤버) 함수 및 진입점에 따라 달라집니다. 애플리케이션에 [C++/WinRT VSIX(Visual Studio Extension)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) 프로젝트 템플릿 중 하나를 사용하는 경우 `WindowsApp.lib`가 자동으로 연결됩니다. 그러지 않으면 프로젝트 연결 설정을 사용하여 포함하거나, 소스 코드에서 포함할 수 있습니다.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

대체 정적 연결 라이브러리 대신 **WindowsApp.lib**를 연결하여 해결할 수 있는 링커 오류를 모두 해결하는 것이 중요합니다. 그러지 않으면 애플리케이션이 Visual Studio 및 Microsoft Store에서 제출의 유효성 검사에 사용하는 [Windows 앱 인증 키트](../debug-test-perf/windows-app-certification-kit.md) 테스트를 통과하지 못하기 때문에 애플리케이션을 Microsoft Store로 수집할 수 없습니다.

## <a name="why-am-i-getting-a-class-not-registered-exception"></a>"클래스가 등록되지 않음" 예외가 발생하는 이유는 무엇인가요?

이런 경우 해당 증상은 (런타임 클래스를 생성하거나 정적 멤버에 액세스할 때) REGDB_E_CLASSNOTREGISTERED의 HRESULT 값을 사용하여 런타임에서 예외가 발생한 것입니다.

한 가지 원인은 Windows 런타임 구성 요소를 로드할 수 없기 때문일 수 있습니다. 구성 요소의 Windows 런타임 메타데이터 파일(`.winmd`) 이름이 구성 요소 이진(`.dll`) 이름과 같은지 확인합니다. 프로젝트 이름이자 루트 네임스페이스 이름이기도 합니다. Windows 런타임 메타데이터와 이진이 빌드 프로세스를 통해 사용하는 앱의 `Appx` 폴더로 정확히 복사되었는지도 확인합니다. 또한 사용하는 앱의 `AppxManifest.xml`(`Appx` 폴더에 있음)에 활성화 가능한 클래스와 이진 이름을 올바르게 선언하는 **&lt;InProcessServer&gt;** 요소가 포함되어 있는지 확인합니다.

### <a name="uniform-construction"></a>균일한 생성

프로젝션된 형식의 생성자 중 하나(**std::nullptr_t** 생성자 제외)를 통해 로컬로 구현한 런타임 클래스를 인스턴스화하려는 경우에도 이 오류가 발생할 수 있습니다. 이 작업을 수행하려면 균일한 생성이라고 부르는 C++/WinRT 2.0 기능이 필요합니다. 해당 기능을 옵트인하려고 하고 자세한 내용과 코드 예제를 알아보려면 [균일한 생성 및 직접 구현 액세스 옵트인](/windows/uwp/cpp-and-winrt-apis/author-apis#opt-in-to-uniform-construction-and-direct-implementation-access)을 참조하세요.

균일한 생성이 필요하지 *않은* 로컬로 구현한 런타임 클래스를 인스턴스화하는 방법은 [XAML 컨트롤, C++/WinRT 속성에 바인딩](binding-property.md)을 참조하세요.

## <a name="should-i-implement-windowsfoundationiclosable-and-if-so-how"></a>[**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable)을 구현해야 하나요? 구현해야 한다면 어떻게 구현하나요?
소멸자에서 리소스를 해제하는 런타임 클래스가 있고, 이 런타임 클래스가 구현하는 컴파일 단위 외부에서 사용하도록 설계된 경우(여기에서 런타임 클래스는 Windows 런타임 클라이언트 앱에서 일반 용도로 사용하는 Windows 런타임 구성 요소임), 결정적 종료가 없는 언어에서 런타임 클래스를 사용할 수 있도록 지원하기 위해 **IClosable**도 구현하는 것이 좋습니다. 소멸자, [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close) 또는 둘 다 호출하든 관계없이 리소스가 해제되는지 확인합니다. **IClosable::Close**는 원하는 횟수만큼 임의로 호출할 수 있습니다.

## <a name="do-i-need-to-call-iclosableclose-on-runtime-classes-that-i-consume"></a>사용하는 런타임 클래스에서 [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close)를 호출해야 하나요?
**IClosable**은 결정적 종료가 없는 언어를 지원하기 위한 것입니다. 따라서 C++/WinRT에서는 **IClosable::Close**를 호출하지 않아도 됩니다. 단, 매우 드물지만 종료 경합 또는 반 교착 상태와 관련된 경우는 예외입니다. 예를 들어 **Windows.UI.Composition** 형식을 사용한다면, C++/WinRT 래퍼의 소멸을 통해 개체를 삭제하는 대신 설정된 시퀀스에서 개체를 삭제하려는 경우가 발생할 수 있습니다.

## <a name="can-i-use-llvmclang-to-compile-with-cwinrt"></a>C++/WinRT로 컴파일하기 위해 LLVM/Clang을 사용할 수 있나요?
LLVM 및 Clang 도구 체인은 C++/WinRT에서 지원되지 않지만 C++/WinRT의 표준 적합성의 유효성을 검사하기 위해 내부적으로 사용됩니다. 예를 들어 Microsoft가 내부적으로 수행하는 작업을 에뮬레이트하려는 경우 아래에 설명된 대로 실험해 볼 수 있습니다.

[LLVM Download Page](https://releases.llvm.org/download.html)(LLVM 다운로드 페이지)로 이동하여 **Download LLVM 6.0.0**(LLVM 6.0.0 다운로드) > **Pre-Built Binaries**(미리 빌드된 이진)를 찾은 다음, **Clang for Windows (64-bit)** (Windows용 Clang(64비트))를 다운로드합니다. 설치 중에 명령 프롬프트에서 호출할 수 있도록 LLVM을 PATH 시스템 변수에 추가할 수 있습니다. 이 실험의 목적을 위해 “MSBuild 도구 세트 디렉터리를 찾지 못했습니다.” 또는 “MSVC 통합을 설치하지 못했습니다.” 오류가 표시되는 경우 모두 무시해도 됩니다. LLVM/Clang을 호출하는 방법은 여러 가지가 있습니다. 아래 예제는 이러한 방법 중 하나일 뿐입니다.

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

C++/WinRT는 C++17 표준의 기능을 사용하기 때문에 해당 지원을 얻는 데 필요한 컴파일러 플래그를 모두 사용해야 합니다. 이러한 플래그는 컴파일러마다 다릅니다.

Visual Studio는 C++/WinRT용으로 지원 및 추천되는 개발 도구입니다. [Visual Studio의 C++/WinRT 지원](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)을 참조하세요.

## <a name="why-doesnt-the-generated-implementation-function-for-a-read-only-property-have-the-const-qualifier"></a>읽기 전용 속성에 대해 생성된 구현 함수에 `const` 한정자가 없는 이유는 무엇인가요?
[MIDL 3.0](/uwp/midl-3/)에서 읽기 전용 속성을 선언하는 경우 `cppwinrt.exe` 도구에서 `const`로 한정된 구현 함수를 생성할 것을 예상할 수 있습니다(const 함수는 *this* 포인터를 const로 처리함).

가능한 경우 항상 const를 사용하는 것이 좋지만, `cppwinrt.exe` 도구 자체는 const가 될 수 있는 구현 함수와 const가 될 수 없는 구현 함수를 추론하지 않습니다. 이 예제와 같이 모든 구현 함수를 const를 설정할 수 있습니다.

```cppwinrt
struct MyStringable : winrt::implements<MyStringable, winrt::Windows::Foundation::IStringable>
{
    winrt::hstring ToString() const
    {
        return L"MyStringable";
    }
};
```

구현에서 일부 개체 상태를 변경해야 하는 경우 **ToString**의 `const` 한정자를 제거할 수 있습니다. 그러나 각 멤버 함수를 const 또는 비const로 설정해야 하며, 둘 다로 설정하면 안 됩니다. 즉, `const`에 구현 함수를 오버로드하지 않습니다.

구현 함수 외에도 const는 Windows 런타임 함수 프로젝션에서 중요합니다. 다음 코드를 살펴보세요.

```cppwinrt
int main()
{
    winrt::Windows::Foundation::IStringable s{ winrt::make<MyStringable>() };
    auto result{ s.ToString() };
}
```

위의 **ToString** 호출에서 Visual Studio의 **선언으로 이동** 명령을 실행하면 Windows 런타임 **IStringable::ToString**을 C++/WinRT로 프로젝션하는 작업이 다음과 같이 표시됩니다.

```cppwinrt
winrt::hstring ToString() const;
```

프로젝션의 함수는 선택한 구현 한정 방법에 관계없이 const입니다. 백그라운드에서 프로젝션은 COM 인터페이스 포인터를 통한 호출인 ABI(애플리케이션 이진 인터페이스) 호출을 수행합니다. 프로젝션된 **ToString**이 상호 작용하는 상태는 COM 인터페이스 포인터뿐이고 해당 포인터를 수정할 필요가 없으므로 함수는 const입니다. 따라서 호출 시 사용하는 **IStringable** 참조가 변경되지 않으며, **IStringable**에 대한 const 참조를 통해서도 **ToString**을 호출할 수 있습니다.

이러한 `const` 예제는 C++/WinRT 프로젝션 및 구현의 구현 정보이며, 사용자에게 유용한 코드 예방 조치입니다. COM 또는 Windows 런타임 ABI(멤버 함수의 경우)에는 `const`에 해당하는 요소가 없습니다.

## <a name="do-you-have-any-recommendations-for-decreasing-the-code-size-for-cwinrt-binaries"></a>C++/WinRT 이진의 코드 크기를 줄이는 방법에 대한 권장 사항이 있나요?
Windows 런타임 개체로 작업하는 경우 아래 표시된 코딩 패턴을 사용하면 필요한 코드보다 많은 이진 코드가 생성되어 애플리케이션에 부정적인 영향을 미칠 수 있으므로 사용하지 않도록 해야 합니다.

```cppwinrt
anobject.b().c().d();
anobject.b().c().e();
anobject.b().c().f();
```

Windows 런타임 환경에서는 컴파일러가 `c()`의 값 또는 간접 참조(‘.’)를 통해 호출된 각 메서드의 인터페이스를 캐시할 수 없습니다. 조치를 취하지 않으면 더 많은 가상 호출과 참조 계산 오버헤드가 발생합니다. 위의 패턴은 엄격하게 필요한 코드보다 두 배 많은 코드를 쉽게 생성할 수 있습니다. 가능한 경우 항상 아래에 표시된 패턴을 대신 사용하는 것이 좋습니다. 이 패턴은 훨씬 적은 코드를 생성하며, 런타임 성능도 크게 개선할 수 있습니다.

```cppwinrt
auto a{ anobject.b().c() };
a.d();
a.e();
a.f();
```

위에 표시된 권장 패턴은 C++/WinRT뿐만 아니라 모든 Windows 런타임 언어 프로젝션에 적용됩니다.

## <a name="how-do-i-turn-a-string-into-a-typemdashfor-navigation-for-example"></a>탐색 등을 위해 문자열을 형식으로 변환하려면 어떻게 하나요?
[탐색 보기 코드 예제](/windows/uwp/design/controls-and-patterns/navigationview#code-example)(대부분 C#으로 작성됨)의 끝에 이 작업을 수행하는 방법을 보여 주는 C++/WinRT 코드 조각이 있습니다.

## <a name="how-do-i-resolve-ambiguities-with-getcurrenttime-andor-try"></a>GetCurrentTime 및/또는 TRY를 사용하여 모호성을 해결하려면 어떻게 하나요?

`winrt/Windows.UI.Xaml.Media.Animation.h` 헤더 파일에서 이름이 **GetCurrentTime**인 메서드를 선언하며 `windows.h`(`winbase.h`를 통해)에서는 **GetCurrentTime**이라는 매크로를 정의합니다. 두 개가 충돌하는 경우 C++ 컴파일러에서 "*오류 C4002: 함수 형식 매크로 호출 GetCurrentTime에 대한 인수가 너무 많습니다.* "를 생성합니다.

마찬가지로 `winrt/Windows.Globalization.h`에서 **TRY**라는 이름의 메서드를 선언하며 `afx.h`에서는 **GetCurrentTime**이라는 매크로를 정의합니다. 이들이 충돌하면 C++ 컴파일러에서 "*오류 C2334: '{' 앞에 예기치 않은 토큰이 있습니다. 명백한 함수 본문을 건너뜁니다.* "를 생성합니다.

두 문제 중 하나 또는 둘 모두 해결하려면 다음을 수행할 수 있습니다.

```cppwinrt
#pragma push_macro("GetCurrentTime")
#pragma push_macro("TRY")
#undef GetCurrentTime
#undef TRY
#include <winrt/include_your_cppwinrt_headers_here.h>
#include <winrt/include_your_cppwinrt_headers_here.h>
#pragma pop_macro("TRY")
#pragma pop_macro("GetCurrentTime")
```

> [!NOTE]
> 이 항목에서 질문에 대한 답변을 찾지 못한 경우, [Visual Studio C++ 개발자 커뮤니티](https://developercommunity.visualstudio.com/spaces/62/index.html)를 방문하거나 [Stack Overflow의 `c++-winrt` 태그](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt)를 사용하여 도움말을 찾을 수 있습니다.
