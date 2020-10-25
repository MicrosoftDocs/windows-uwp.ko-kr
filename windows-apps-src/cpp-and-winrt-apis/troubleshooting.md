---
description: 이 항목에 나와 있는 문제 증상 및 해결 방법에 대한 표는 새 코드를 자르거나 기존 앱을 이식할지를 결정하는 데 도움이 될 수 있습니다.
title: C++/WinRT 문제 해결
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 문제 해결, HRESULT, 오류
ms.localizationpriority: medium
ms.openlocfilehash: 268792dfe8053feca8c1e6fcb486bede4b26ee6a
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297721"
---
# <a name="troubleshooting-cwinrt-issues"></a>C++/WinRT 문제 해결

> [!NOTE]
> 프로젝트 템플릿 지원을 제공하는 [C++/WinRT](./intro-to-using-cpp-with-winrt.md) VSIX(Visual Studio Extension)를 설치 및 사용하는 방법에 대한 자세한 내용은 [Visual Studio의 C++/WinRT 지원](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)을 참조하세요.

이 항목은 당장 필요하지 않더라도 바로 파악할 수 있도록 사전 정보로 제공되었습니다. 아래 문제 해결 표의 증상과 해결 방법은 새 코드를 잘라내든, 기존 앱을 포팅하든 간에 유용할 수 있습니다. 포팅 중이며 프로젝트를 빌드 및 실행하는 단계까지 빠르게 도달하려는 경우 문제가 발생하는, 중요하지 않은 코드를 주석으로 처리하거나 스텁한 후 나중에 돌아가서 문제를 해결하면 일시적으로 진행할 수 있습니다.

자주 묻는 질문 목록은 [자주 묻는 질문](faq.md)을 참조하세요.

## <a name="tracking-down-xaml-issues"></a>XAML 문제 추적
예외 내에 의미 있는 오류 메시지가 없는 경우 특히, XAML 구문 분석 예외를 진단하기 어려울 수 있습니다. 초기에 구문 분석 예외를 찾기 위해 첫째 예외를 catch하도록 디버거가 구성되어 있는지 확인하세요. 디버거에서 예외 변수를 검사하여 HRESULT 또는 메시지에 유용한 정보가 있는지 확인할 수 있습니다. XAML 파서로 오류 메시지 출력을 위한 Visual Studio 출력 창을 확인할 수도 있습니다.

앱이 종료되고 XAML 태그 구문 분석 중 처리되지 않은 예외가 throw되었다는 것만 알 수 있는 경우 누락된 리소스를 키로 참조한 결과일 수 있습니다. 또는 UserControl, 사용자 지정 컨트롤 또는 사용자 지정 레이아웃 패널 내에서 예외가 throw되었을 수 있습니다. 마지막으로 사용할 수 있는 방법은 이진 분할입니다. XAML 페이지에서 태그의 절반 정도를 제거하고 앱을 다시 실행합니다. 그러면 오류가 제거한 절반(어떤 경우든지 지금 복원해야 함)에서 발생했는지, 제거하지 않은 절반에서 발생했는지를 알 수 있습니다. 문제가 더 이상 발생하지 않을 때까지 오류가 포함된 부분을 계속해서 분할하면서 프로세스를 반복합니다.

## <a name="symptoms-and-remedies"></a>증상 및 해결 방법
| 증상 | 해결책 |
|---------|--------|
| HRESULT 값이 REGDB_E_CLASSNOTREGISTERED일 때 런타임에서 예외가 throw됩니다. | ["클래스가 등록되지 않음" 예외가 발생하는 이유는 무엇인가요?](faq.md#why-am-i-getting-a-class-not-registered-exception)를 참조하세요. |
| C++ 컴파일러에서 “’implements_type’: ‘&lt;projected type&gt;’의 직접 또는 간접 기본 클래스에 속하는 멤버가 아닙니다.” 오류를 생성합니다.** | 이 오류는 네임스페이스로 한정되지 않은 구현 형식 이름(예: **MyRuntimeClass**)으로 **make**를 호출하고 해당 형식의 헤더를 포함하지 않은 경우에 발생할 수 있습니다. 컴파일러가 **MyRuntimeClass**를 프로젝션된 형식으로 해석합니다. 해결 방법은 구현 형식의 헤더(예: `MyRuntimeClass.h`)를 포함하는 것입니다. |
| C++ 컴파일러에서 “삭제된 함수를 참조하려고 합니다.” 오류를 생성합니다.** | 이 오류는 **make**를 호출하고 템플릿 매개 변수로 전달하는 구현 형식에 `= delete` 기본 생성자가 있는 경우에 발생할 수 있습니다. 구현 형식의 헤더 파일을 편집하여 `= delete`를 `= default`로 변경합니다. 런타임 클래스의 IDL에 생성자를 추가할 수도 있습니다. |
| [**INotifyPropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged)를 구현했지만 XAML 바인딩이 업데이트되지 않고 UI가 [**PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)를 구독하지 않습니다. | XAML 태그의 바인딩 식에서 `Mode=OneWay`(또는 TwoWay)를 설정해야 합니다. [XAML 컨트롤, C++/WinRT 속성에 바인딩](binding-property.md)을 참조하세요. |
| 식별할 수 있는 컬렉션에 XAML 항목 컨트롤을 바인딩하고 있으며, 런타임에 예외가 throw되고 “매개 변수가 잘못되었습니다.” 메시지가 표시됩니다. | IDL 및 구현에서 식별할 수 있는 모든 컬렉션을 **Windows.Foundation.Collections.IVector<IInspectable>** 형식으로 선언합니다. 단, **Windows.Foundation.Collections.IObservableVector<T>** 를 구현하는 개체를 반환합니다. 여기서 T는 요소 형식입니다. [XAML 항목 컨트롤, C++/WinRT 컬렉션에 바인딩](binding-collection.md)을 참조하세요.  |
| C++ 컴파일러에서 “‘MyImplementationType_base&lt;MyImplementationType&gt;’: 사용할 수 있는 적절한 기본 생성자가 없습니다.” 형식의 오류를 생성합니다.**|이 오류는 특수한 생성자가 있는 형식에서 파생한 경우에 발생할 수 있습니다. 파생 형식의 생성자가 기본 형식의 생성자에 필요한 매개 변수를 전달해야 합니다. 처리된 예제는 [특수 생성자가 있는 형식에서 파생](author-apis.md#deriving-from-a-type-that-has-a-non-default-constructor)을 참조하세요.|
| C++ 컴파일러에서 “‘const std::vector&lt;std::wstring,std::allocator&lt;_Ty&gt;&gt;’에서 ‘const winrt::param::async_iterable&lt;winrt::hstring&gt; &’로 변환할 수 없습니다.” 오류를 생성합니다.|이 오류는 std::wstring의 std::vector를 컬렉션이 필요한 Windows 런타임 API에 전달하는 경우에 발생할 수 있습니다. 자세한 내용은 [표준 C++ 데이터 형식 및 C++/WinRT](std-cpp-data-types.md)를 참조하세요.|
| C++ 컴파일러에서 “‘const std::vector&lt;winrt::hstring,std::allocator&lt;_Ty&gt;&gt;’에서 ‘const winrt::param::async_iterable&lt;winrt::hstring&gt; &’로 변환할 수 없습니다.” 오류를 생성합니다.|이 오류는 winrt::hstring의 std::vector를 컬렉션이 필요한 비동기 Windows 런타임 API에 전달하고, 벡터를 비동기 호출 수신자로 복사하거나 이동하지 않은 경우에 발생할 수 있습니다. 자세한 내용은 [표준 C++ 데이터 형식 및 C++/WinRT](std-cpp-data-types.md)를 참조하세요.|
| 프로젝트를 열 때 Visual Studio에서 “프로젝트에 대한 애플리케이션이 설치되어 있지 않습니다.” 오류를 생성합니다.**|**C++ 개발용 Windows 유니버설 도구**를 아직 설치하지 않은 경우 Visual Studio의 **새 프로젝트** 대화 상자에서 설치해야 합니다. 그래도 문제가 해결되지 않으면 프로젝트에서 C++/WinRT VSIX(Visual Studio Extension)를 사용할 수 있습니다([Visual Studio의 C++/WinRT 지원](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) 참조).|
| Windows 앱 인증 키트 테스트에서 런타임 클래스 중 하나가 “Windows 기본 클래스에서 파생되지 않았습니다. 구성 가능한 모든 클래스는 궁극적으로 Windows 네임스페이스의 형식에서 파생되어야 합니다.” 오류를 생성합니다.|기본 클래스에서 파생된 런타임 클래스(애플리케이션에서 선언)를 ‘구성 가능’ 클래스라고 합니다.** 구성 가능 클래스의 최종 기본 클래스는 Windows.* 네임스페이스에서 시작되는 형식(예: [**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject))이어야 합니다. 자세한 내용은 [XAML 컨트롤, C++/WinRT 속성에 바인딩](binding-property.md)을 참조하세요.|
| C++ 컴파일러가 EventHandler 또는 TypedEventHandler 대리자 전문화에 대해 "*T는 WinRT 형식이어야 합니다.* "라는 오류 메시지를 생성합니다.|**winrt::delegate&lt;...T&gt;** 를 대신 사용하세요. [C++/WinRT의 이벤트 작성](author-events.md)을 참조하세요.|
| C++ 컴파일러가 Windows 런타임 비동기 작업 전문화에 대해 "*T는 WinRT 형식이어야 합니다.* "라는 오류를 생성합니다.|PPL(병렬 패턴 라이브러리) [**작업**](/cpp/parallel/concrt/reference/task-class)을 대신 반환하세요. [동시성 및 비동기 작업](concurrency.md)을 참조하세요.|
| C++ 컴파일러가 [**winrt::xaml_typename**](/uwp/cpp-ref-for-winrt/xaml-typename)을 호출할 때 "*T는 WinRT 형식이어야 합니다.* "라는 오류를 생성합니다.|**winrt::xaml_typename**의 프로젝션 형식(예: **BgLabelControlApp::BgLabelControl** 사용)을 사용하고, 구현 형식(**BgLabelControlApp::implementation::BgLabelControl**을 사용하지 않음)을 사용하지 마세요. [XAML 사용자 지정(템플릿) 컨트롤](xaml-cust-ctrl.md)을 참조하세요.|
| C++ 컴파일러에서 “오류 C2220: 경고가 오류로 처리되어 생성된 ‘object’ 파일이 없습니다.” 오류를 생성합니다.**|경고를 수정하거나, **C/C++**  > **일반** > **경고를 오류로 처리**를 **아니요(/WX-)** 로 설정합니다.|
| 개체 삭제 후 C++/WinRT 개체의 이벤트 처리기가 호출되어 앱 작동이 중단됩니다.|[이벤트 처리 대리자를 사용하여 안전하게 *this* 포인터 액세스](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)를 참조하세요.|
| C++ 컴파일러에서 “오류 C2338: 약한 참조 지원에만 사용됩니다.”를 생성합니다.|현재 **winrt::no_weak_ref** 마커 구조체를 템플릿 인수로 기본 클래스에 전달한 형식에 대해 약한 참조를 요청하고 있습니다. [약한 참조 지원 옵트아웃](weak-references.md#opting-out-of-weak-reference-support)을 참조하세요.|
| C++ 링커에서 “오류 LNK2019: 확인되지 않은 외부 기호”를 생성합니다.|[링커에서 “LNK2019: 확인되지 않은 외부 기호” 오류가 발생하는 이유는 무엇인가요?](faq.md#why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error)를 참조하세요.|
| C++/WinRT에서 사용할 경우 LLVM 및 Clang 도구 체인에서 오류를 생성합니다.|LLVM 및 Clang 도구 체인은 C++/WinRT에서 지원되지 않지만 내부적으로 사용하는 방법을 에뮬레이트하려는 경우 [C++/WinRT로 컴파일하기 위해 LLVM/Clang을 사용할 수 있나요?](faq.md#can-i-use-llvmclang-to-compile-with-cwinrt)에 설명된 것처럼 실험해 볼 수 있습니다.|
| C++ 컴파일러에서 프로젝션된 형식에 대해 “사용할 수 있는 적절한 기본 생성자가 없습니다.”를 생성합니다.** | 런타임 클래스 개체의 초기화를 지연하거나 동일한 프로젝트에서 런타임 클래스를 사용 및 구현하려는 경우, **std::nullptr_t** 생성자를 호출해야 합니다. 자세한 내용은 [C++/WinRT를 통한 API 사용](consume-apis.md)을 참조하세요. |
| C++ 컴파일러에서 “오류 C3861: ‘from_abi’: 식별자를 찾을 수 없습니다.” 및 *base.h*에서 시작되는 기타 오류를 생성합니다.** 이 오류는 Visual Studio 2017(버전 15.8.0 이상)을 사용 중이며 Windows SDK 버전 10.0.17134.0(Windows 10, 버전 1803)을 대상으로 지정하는 경우에 표시될 수 있습니다. | 보다 규칙에 맞는 Windows SDK 최신 버전을 대상으로 지정하거나, 프로젝트 속성 **C/C++**  > **언어** > **적합성 모드: 아니요**를 설정합니다. 또는 **추가 옵션** 아래의 프로젝트 속성 **C/C++**  > **언어** > **명령줄**에 **/permissive-** 가 표시되는 경우 삭제합니다. |
| C++ 컴파일러에서 “오류 C2039: ‘IUnknown’: ‘\`global namespace’의 멤버가 아닙니다.”를 생성합니다. | [C++/WinRT 프로젝트의 대상을 Windows SDK 최신 버전으로 변경하는 방법](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)을 참조하세요. |
| C++ 링커에서 “오류 LNK2019: 확인되지 않은 외부 기호 _WINRT_CanUnloadNow@0이 _VSDesignerCanUnloadNow@0 함수에서 참조되었습니다.”를 생성합니다.** | [C++/WinRT 프로젝트의 대상을 Windows SDK 최신 버전으로 변경하는 방법](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)을 참조하세요. |
| 빌드 프로세스에서 오류 메시지 “C++/WinRT VSIX에서 더 이상 프로젝트 빌드 지원을 제공하지 않습니다.  Microsoft.Windows.CppWinRT Nuget 패키지에 프로젝트 참조를 추가하세요.”를 생성합니다. | **Microsoft.Windows.CppWinRT** NuGet 패키지를 프로젝트에 설치합니다. 자세한 내용은 [이전 버전의 VSIX 확장](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)을 참조하세요. |
| C++ 링커에서 *winrt::impl::consume_Windows_Foundation_Collections_IVector*에 대한 멘션과 함께 “오류 LNK2019: 확인되지 않은 외부 기호”를 생성합니다.** | [C++/WinRT 2.0](news.md#news-and-changes-in-cwinrt-20)에서는 Windows 런타임 컬렉션에서 범위 기반의 `for`를 사용하는 경우 이제 `#include <winrt/Windows.Foundation.Collections.h>`가 필요합니다. |
| C++ 컴파일러에서 "*오류 C4002: 함수 형식 매크로 호출 GetCurrentTime에 대한 인수가 너무 많습니다.* "를 생성합니다. | [GetCurrentTime 및/또는 TRY를 사용하여 모호성을 해결하려면 어떻게 하나요?](faq.md#how-do-i-resolve-ambiguities-with-getcurrenttime-andor-try)를 참조하세요. |
| C++ 컴파일러에서 "*오류 C2334: '{' 앞에 예기치 않은 토큰이 있습니다. 명백한 함수 본문을 건너뜁니다.*"를 생성합니다. | [GetCurrentTime 및/또는 TRY를 사용하여 모호성을 해결하려면 어떻게 하나요?](faq.md#how-do-i-resolve-ambiguities-with-getcurrenttime-andor-try)를 참조하세요. |
| C++ 컴파일러에서 "GetBindingConnector가 없으므로 *winrt::impl::produce&lt;D,I&gt;에서 추상 클래스를 인스턴스화할 수 없습니다.*"를 생성합니다. | `#include <winrt/Windows.UI.Xaml.Markup.h>`를 수행해야 합니다. |
| C++ 컴파일러에서 "*오류 C2039: 'promise_type':은 'std::experimental::coroutine_traits<void>'의 멤버가 아닙니다.*"를 생성합니다. | 사용자 코루틴에서 비동기 작업 개체 또는 **winrt::fire_and_forget** 중 하나를 반환해야 합니다. [동시성 및 비동기 작업](concurrency.md)을 참조하세요. |
| 사용자 프로젝트에서 "*'PopulatePropertyInfoOverride' 액세스가 모호합니다.*"를 생성합니다. | 이 오류는 IDL에서 하나의 기본 클래스를 선언하고 XAML 태그에서 다른 기본 클래스를 선언하는 경우에 발생할 수 있습니다. |
| 처음으로 C++/WinRT 솔루션을 로드하면 "*프로젝트 'MyProject.vcxproj' 구성 'Debug\|x86'에 대한 Designtime 빌드에 실패했습니다. IntelliSense를 사용하지 못할 수 있습니다.* "를 생성합니다. | 이 IntelliSense 문제는 처음 빌드한 후 해결됩니다. |
| 대리자 등록 시 [**winrt::auto_revoke**](/uwp/cpp-ref-for-winrt/auto-revoke-t)를 지정하려고 하면 [**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) 예외를 생성합니다. | [자동 취소 대리자를 등록하지 못하는 경우](handle-events.md#if-your-auto-revoke-delegate-fails-to-register)를 참조하세요. |
|C++/WinRT 앱에서 XAML을 사용하는 [C# Windows 런타임 구성 요소](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md)를 사용할 때 컴파일러는 " *'MyNamespace_XamlTypeInfo': is not a member of 'winrt::MyNamespace'* " 형식의 오류를 발생합니다. &mdash;여기서 *MyNamespace*는 Windows 런타임 구성 요소의 네임스페이스 이름입니다. | 사용하는 C++/WinRT 앱의 `pch.h`에서 *MyNamespace*를 적절하게 대체하는 `#include <winrt/MyNamespace.MyNamespace_XamlTypeInfo.h>`&mdash;를 추가합니다. |

> [!NOTE]
> 이 항목에서 질문에 대한 답변을 찾지 못한 경우, [Visual Studio C++ 개발자 커뮤니티](https://developercommunity.visualstudio.com/spaces/62/index.html)를 방문하거나 [Stack Overflow의 `c++-winrt` 태그](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt)를 사용하여 도움말을 찾을 수 있습니다.