---
description: 이번 항목에서 증상 문제 및 해결 방법을 나타낸 표는 새로운 코드를 자르거나 기존 앱을 이식할지 결정하는 데 도움이 될 수 있습니다.
title: C++/WinRT 문제 해결
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 문제 해결, HRESULT, 오류
ms.localizationpriority: medium
ms.openlocfilehash: 563545e8a819ab6af5bbc0604c18b4833d76bebb
ms.sourcegitcommit: 1f39b67f2711b96c6b4e7ed7107a9a47127d4e8f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66721670"
---
# <a name="troubleshooting-cwinrt-issues"></a>C++/WinRT 문제 해결

> [!NOTE]
> 설치 및 사용에 대 한 정보를 [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) VSIX Visual Studio Extension () (지원을 제공 하는 프로젝트 템플릿)를 참조 하세요 [Visual Studio 지원에 대 한 C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

이번 항목은 사전적 정보이므로 당장 필요하지 않더라도 알고 있어야 합니다. 아래 증상 문제 및 해결 방법을 나타낸 표는 새로운 코드를 자르거나 기존 앱을 이식할지 결정하는 데 도움이 될 수 있습니다. 앱을 이식하여 프로젝트를 빌드 및 실행하는 단계까지 빠르게 진행하려는 경우에는 문제를 야기하는 일반(중요하지 않은) 코드를 주석 처리하거나 삭제한 후 해당 부채를 나중에 갚을 때 반환하는 방식으로 잠시 진행할 수는 있습니다.

자주 묻는 질문의 목록에 대해서 [질문과 대답](faq.md)합니다.

## <a name="tracking-down-xaml-issues"></a>XAML 문제 추적
예외 내에서 의미 있는 오류 메시지가 없는 경우에 특히 XAML 구문 분석 예외를 진단하는 것이 어려울 수 있습니다. 초기에 구문 분석 예외를 찾기 위해 첫째 예외를 catch하도록 디버거가 구성되어 있는지 확인하세요. 디버거에서 예외 변수를 검사하여 HRESULT 또는 메시지에 유용한 정보가 있는지 확인할 수 있습니다. XAML 파서로 오류 메시지 출력을 위한 Visual Studio 출력 창을 확인할 수도 있습니다.

앱이 종료된 후 XAML 태그 구문 분석 도중 처리되지 않은 예외가 발생하였다는 사실 외에 아무것도 모를 때는 누락된 리소스를 참조한(키 기준) 결과일 수 있습니다. 또는 UserControl, 사용자 지정 컨트롤 또는 사용자 지정 레이아웃 패널에서 예외가 발생했을 수 있습니다. 마지막으로 사용할 수 있는 방법은 이진 분할입니다. XAML 페이지에서 태그의 약 1/2을 제거하고 앱을 다시 실행합니다. 그런 다음 오류가 제거된 부분에서 발생했는지 제거되지 않은 부분에서 발생했는지 여부를 확인합니다. 문제가 더 이상 발생하지 않을 때까지 오류가 포함된 부분을 계속해서 분할하면서 프로세스를 반복합니다.

## <a name="symptoms-and-remedies"></a>증상 및 해결 방법
| 증상 | 해결 방법 |
|---------|--------|
| HRESULT 값이 REGDB_E_CLASSNOTREGISTERED일 때 런타임에서 예외가 발생하였습니다. | 이 오류가 발생하는 한 가지 원인은 Windows 런타임 구성 요소를 로드할 수 없기 때문입니다. 이때는 구성 요소의 Windows 런타임 메타데이터 파일(`.winmd`) 이름이 구성 요소 바이너리의 이름(`.dll`)과 동일한지 확인하세요. 구성 요소 바이너리는 프로젝트의 이름이자 루트 네임스페이스의 이름이기도 합니다. 또한 Windows 런타임 메타데이터와 바이너리가 빌드 프로세스를 통해 사용할 앱의 `Appx` 폴더로 정확히 복사되었는지도 확인하세요. 그 밖에 사용할 앱의 `AppxManifest.xml`(`Appx` 폴더에 위치함) 파일에 활성화할 수 있는 클래스를 정확히 선언한 **&lt;InProcessServer&gt;** 요소와 바이너리 이름이 포함되어 있는지도 확인해야 합니다. 이 오류는 로컬에 구현된 런타임 클래스를 프로젝션된 형식의 기본 생성자를 통해 인스턴스화하는 실수를 저지르는 경우에도 발생할 수 있습니다. 이러한 경우 프로젝션된 형식을 올바르게 사용하는 방법에 대한 자세한 내용은 [XAML 컨트롤, C++/WinRT 속성 바인딩](binding-property.md)을 참조하세요. |
| C++ 컴파일러가 오류 메시지로 " *'implements_type': is not a member of any direct or indirect base class of '&lt;projected type&gt;'* "를 생성합니다. | 이러한 오류는 구현체 유형의 네임스페이스-비정규화 이름(예: **MyRuntimeClass**, for example)으로 **make**를 호출하고서 해당 유형의 헤더를 추가하지 않았을 때 발생할 수 있습니다. 컴파일러는 **MyRuntimeClass**를 프로젝션된 형식으로 해석합니다. 이 문제를 해결하려면 구현체 유형의 헤더(예: `MyRuntimeClass.h`)를 추가해야 합니다. |
| C++ 컴파일러가 오류 메시지인 "*attempting to reference a deleted function*"을 생성합니다. | 이 오류는 **make**를 호출하면서 템플릿 매개 변수로 전달할 구현체 유형에 `= delete` 기본 생성자가 있을 때 발생할 수 있습니다. 이때는 구현체 유형의 헤더 파일을 편집하여 `= delete`를 `= default`로 변경하세요. 또한 런타임 클래스 IDL에 생성자를 추가해도 좋습니다. |
| [  **INotifyPropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged)를 실행하였지만 XAML 바인딩이 업데이트되지 않습니다(또한 UI가 [**PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)를 구독하지 않습니다). | XAML 태그의 바인딩 표현식에서 `Mode=OneWay`(또는 TwoWay)로 설정해야 합니다. [XAML 컨트롤, a C++/WinRT 속성 바인딩](binding-property.md)을 참조하세요. |
| XAML 항목 컨트롤을 관찰 가능한 컬렉션에 바인딩하고 있는 도중 런타임에서 "The parameter is incorrect" 메시지와 함께 예외가 발생하였습니다. | 이때는 IDL 및 구현체에서 관찰 가능한 컬렉션을 모두 **Windows.Foundation.Collections.IVector<IInspectable>** 유형으로 선언하세요. 단, **Windows.Foundation.Collections.IObservableVector<T>** 를 구현하는 개체는 반환해야 합니다. 여기에서 T는 요소 유형을 말합니다. [XAML 항목 컨트롤, C++/WinRT 컬렉션 바인딩](binding-collection.md)을 참조하세요.  |
| C++ 컴파일러가 " *'MyImplementationType_base&lt;MyImplementationType&gt;': no appropriate default constructor available*"이라는 오류 메시지를 생성합니다.|이 오류는 특수(non-trivial) 생성자를 가지고 있는 유형에서 파생되었을 때 발생할 수 있습니다. 이때는 파생된 유형의 생성자가 기본 유형의 생성자에게 필요한 매개 변수와 함께 전달되어야 합니다. 유효 예제는 [특수 생성자를 가지고 있는 유형에서 파생](author-apis.md#deriving-from-a-type-that-has-a-non-default-constructor)을 참조하세요.|
| C++ 컴파일러가 "*cannot convert from 'const std::vector&lt;std::wstring,std::allocator&lt;_Ty&gt;&gt;' to 'const winrt::param::async_iterable&lt;winrt::hstring&gt; &'* "이라는 오류 메시지를 생성합니다.|이 오류는 std::wstring에서 std::vector를 컬렉션이 필요한 Windows 런타임 API에게 전달할 때 발생할 수 있습니다. 자세한 내용은 [표준 C++ 데이터 형식 및 C++/WinRT](std-cpp-data-types.md)를 참조하세요.|
| C++ 컴파일러가 "*cannot convert from 'const std::vector&lt;winrt::hstring,std::allocator&lt;_Ty&gt;&gt;' to 'const winrt::param::async_iterable&lt;winrt::hstring&gt; &'* "이라는 오류 메시지를 생성합니다.|이 오류는 winrt::hstring에서 std::vector를 컬렉션이 필요한 비동기식 Windows 런타임 API로 전달하면서 벡터를 비동기 수신자로 복사하거나 이동시키지 않았을 때 발생할 수 있습니다. 자세한 내용은 [표준 C++ 데이터 형식 및 C++/WinRT](std-cpp-data-types.md)를 참조하세요.|
| 프로젝트를 열 때 Visual Studio에서 "*The application for the project is not installed*"라는 오류 메시지를 생성합니다.|아직 **C++ 개발용 Windows 유니버설 도구**를 설치하지 않았다면 Visual Studio의 **새 프로젝트** 대화 상자에서 설치해야 합니다. 문제를 해결 되지 않을 경우 프로젝트에 따라 달라질 수 있습니다는 C++WinRT Visual Studio 확장 (VSIX) (참조 [Visual Studio 지원에 대 한 C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)합니다.|
| 런타임 클래스 중 하나는 오류를 생성 하는 Windows 앱 인증 키트 테스트 "*Windows 기본 클래스에서 파생 되지 않습니다. 모든 구성 가능한 클래스 Windows 네임 스페이스의 형식에서 최종적으로 파생 되어야 합니다*"입니다.|기본 클래스에서 파생 되는 런타임 클래스 (선언 하는 응용 프로그램에서) 라고 한 *구성 가능한* 클래스입니다. 구성 가능한 클래스의 궁극적인 기본 클래스는 windows. * 네임 스페이스에서 발생 하는 형식 이어야 합니다. 예를 들어 [ **Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject)합니다. 참조 [XAML 컨트롤; 바인딩할를 C++/WinRT 속성](binding-property.md) 대 한 자세한 내용은 합니다.|
| C++ 컴파일러가 EventHandler 또는 TypedEventHandler 대리자 전문화에서 "*must be WinRT type*"이라는 오류 메시지를 생성합니다.|**winrt::delegate&lt;...T&gt;** 를 대신 사용하세요. [C++/WinRT의 작성자 이벤트](author-events.md)를 참조하세요.|
| C++ 컴파일러가 Windows 런타임 비동기 작업 전문화에서 "*must be WinRT type*"이라는 오류 메시지를 생성합니다.|병렬 패턴 라이브러리(PPL) [**작업**](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class)을 대신 반환하세요. [동시성 및 비동기 작업](concurrency.md)을 참조하세요.|
| C++ 컴파일러가 "*error C2220: warning treated as error - no 'object' file generated*"라는 오류 메시지를 생성합니다.|경고를 수정 하거나 설정 **C /C++**  > **일반** > **경고를 오류로 처리** 하 **없음 (/ WX-)** 합니다.|
| 개체 삭제 후 C++/WinRT 개체의 이벤트 처리기가 호출되어 앱이 충돌을 일으킵니다.|참조 [안전 하 게 액세스 합니다 *이* 포인터 이벤트 처리 대리자를 사용 하 여](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)입니다.|
| C++ 컴파일러 생성 "*C2338 오류: 이 약한 참조 지원에 대해서만*"입니다.|현재 **winrt::no_weak_ref** 마커 구조체를 템플릿 인수로 기본 클래스에게 전달한 유형에 대해 약한 참조를 요청하고 있습니다. 참조 [약한 참조 지원을 옵트아웃](weak-references.md#opting-out-of-weak-reference-support)합니다.|
| C++ 링커 생성 "*LNK2019 오류: 확인 되지 않은 외부 기호*"|참조 [이유 링커를 제공 된 "LNK2019: Unresolved external symbol"오류 인가요? ](faq.md#why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error).|
| LLVM 및 Clang 도구 체인에서 함께 사용할 경우 오류가 발생 하면 C++/WinRT 합니다.|C + LLVM 및 Clang 도구 체인을 지원 하지 않습니다 + 그런데 우리가 사용 하는 방법을 내부적으로 에뮬레이션 하는 경우 있지만 WinRT 있다면에서 설명 하는 것 같은 실험을 시도할 수 [LLVM/Clang C +를 사용 하 여 컴파일하는 데 사용할 수 있습니까 + WinRT?](faq.md#can-i-use-llvmclang-to-compile-with-cwinrt)합니다.|
| C++ 컴파일러 생성 "*적절 한 기본 생성자를 사용할 수 없습니다*" 프로젝션된 된 형식에 대 한 합니다. | 런타임 클래스 개체의 초기화를 지연 또는 사용 하 고 동일한 프로젝트에서 런타임 클래스를 구현 하려는 경우를 호출 해야 합니다 `nullptr_t` 생성자입니다. 자세한 정보는 [C++/WinRT를 통한 API 사용](consume-apis.md)을 참조하세요. |
| C++ 컴파일러 생성 "*오류 C3861: 'from_abi': 식별자를 찾을 수 없습니다*", 및에서 시작 된 기타 오류 *base.h*합니다. Visual Studio 2017을 사용 하는 경우이 오류가 나타날 수 있습니다 (15.8.0 버전 이상), 및 Windows SDK (Windows 10, 버전 1803) 10.0.17134.0 버전을 대상으로 지정 합니다. | 이후 (더 부합) 대상으로 하거나 버전의 Windows SDK 또는 프로젝트 속성 설정 **C /C++**  > **언어** > **준수 모드: 더** (또한 경우 **/ permissive-** 프로젝트 속성에 표시 됩니다 **C /C++**  > **언어**  >   **Command Line** 아래에서 **추가 옵션**, 삭제). |
| C++ 컴파일러 생성 "*오류 C2039: 'IUnknown':의 구성원이 아닌 '\`전역 네임 스페이스 '* "입니다. | 참조 [대상을 다시 지정 하는 방법에 C++Windows SDK의 최신 버전으로 /WinRT 프로젝트](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)합니다. |
| C++ 링커 생성 "*LNK2019 오류: 확인 되지 않은 외부 기호 _WINRT_CanUnloadNow@0 함수에서 참조 _VSDesignerCanUnloadNow@0* " | 참조 [대상을 다시 지정 하는 방법에 C++Windows SDK의 최신 버전으로 /WinRT 프로젝트](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)합니다. |
| 오류 메시지를 생성 하는 빌드 프로세스 *C + WinRT VSIX 프로젝트 빌드 지원을 제공 하지 않습니다.  Microsoft.Windows.CppWinRT Nuget 패키지에 대 한 프로젝트 참조를 추가 하십시오*합니다. | 설치 합니다 **Microsoft.Windows.CppWinRT** 프로젝트에 NuGet 패키지. 자세한 내용은 참조 하세요 [이전 버전의 VSIX 확장](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)합니다. |
| C++ 링커 생성 *LNK2019 오류: 확인 되지 않은 외부 기호*에 대 한 언급이 사용 하 여 *winrt::impl::consume_Windows_Foundation_Collections_IVector*합니다. | 일부로 [ C++WinRT 2.0](news.md#news-and-changes-in-cwinrt-20)범위 기반을 사용 하는 경우 `for` Windows 런타임 컬렉션에 대해 다음 이제 해야 `#include <winrt/Windows.Foundation.Collections.h>`합니다. |

> [!NOTE]
> 이 항목에서는 질문, 대답 하지 않은 경우 방문 하 여 도움말을 볼 수 있습니다 합니다 [Visual Studio C++ 개발자 커뮤니티](https://developercommunity.visualstudio.com/spaces/62/index.html), 또는 사용 하 여는 [ `c++-winrt` Stack Overflow에서 태그](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt).
