---
author: stevewhims
description: 이번 항목에서 증상 문제 및 해결 방법을 나타낸 표는 새로운 코드를 자르거나 기존 앱을 이식할지 결정하는 데 도움이 될 수 있습니다.
title: C++/WinRT 문제 해결
ms.author: stwhi
ms.date: 04/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 문제 해결, HRESULT, 오류
ms.localizationpriority: medium
ms.openlocfilehash: 21f5fc4773979b2d7940b85871264e27d56d29c4
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832267"
---
# <a name="troubleshooting-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-issues"></a>[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 문제 해결
> [!NOTE]
> **일부 정보는 상업용으로 출시되기 전에 상당 부분 수정될 수 있는 시험판 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.**

> [!NOTE]
> C++/WinRT Visual Studio Extension(VSIX)(프로젝트 템플릿 지원과 C++/WinRT MSBuild 속성 및 대상 제공)의 현재 가용성에 대한 자세한 내용은 [C++/WinRT에 대한 Visual Studio 지원 및 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)를 참조하세요.

이번 항목은 사전적 정보이므로 당장 필요하지 않더라도 알고 있어야 합니다. 아래 증상 문제 및 해결 방법을 나타낸 표는 새로운 코드를 자르거나 기존 앱을 이식할지 결정하는 데 도움이 될 수 있습니다. 앱을 이식하여 프로젝트를 빌드 및 실행하는 단계까지 빠르게 진행하려는 경우에는 문제를 야기하는 일반(중요하지 않은) 코드를 주석 처리하거나 삭제한 후 해당 부채를 나중에 갚을 때 반환하는 방식으로 잠시 진행할 수는 있습니다.

## <a name="tracking-down-xaml-issues"></a>XAML 문제 추적
예외 내에서 의미 있는 오류 메시지가 없는 경우에 특히 XAML 구문 분석 예외를 진단하는 것이 어려울 수 있습니다. 초기에 구문 분석 예외를 찾기 위해 첫째 예외를 catch하도록 디버거가 구성되어 있는지 확인하세요. 디버거에서 예외 변수를 검사하여 HRESULT 또는 메시지에 유용한 정보가 있는지 확인할 수 있습니다. XAML 파서로 오류 메시지 출력을 위한 Visual Studio 출력 창을 확인할 수도 있습니다.

앱이 종료된 후 XAML 태그 구문 분석 도중 처리되지 않은 예외가 발생하였다는 사실 외에 아무것도 모를 때는 누락된 리소스를 참조한(키 기준) 결과일 수 있습니다. 또는 UserControl, 사용자 지정 컨트롤 또는 사용자 지정 레이아웃 패널에서 예외가 발생했을 수 있습니다. 마지막으로 사용할 수 있는 방법은 이진 분할입니다. XAML 페이지에서 태그의 약 1/2을 제거하고 앱을 다시 실행합니다. 그런 다음 오류가 제거된 부분에서 발생했는지 제거되지 않은 부분에서 발생했는지 여부를 확인합니다. 문제가 더 이상 발생하지 않을 때까지 오류가 포함된 부분을 계속해서 분할하면서 프로세스를 반복합니다.

## <a name="symptoms-and-remedies"></a>증상 및 해결 방법
| 증상 | 해결 방법 |
|---------|--------|
| HRESULT 값이 REGDB_E_CLASSNOTREGISTERED일 때 런타임에서 예외가 발생하였습니다. | 이 오류가 발생하는 한 가지 원인은 Windows 런타임 구성 요소를 로드할 수 없기 때문입니다. 이때는 구성 요소의 Windows 런타임 메타데이터 파일(`.winmd`) 이름이 구성 요소 바이너리의 이름(`.dll`)과 동일한지 확인하세요. 구성 요소 바이너리는 프로젝트의 이름이자 루트 네임스페이스의 이름이기도 합니다. 또한 Windows 런타임 메타데이터와 바이너리가 빌드 프로세스를 통해 사용할 앱의 `Appx` 폴더로 정확히 복사되었는지도 확인하세요. 그 밖에 사용할 앱의 `AppxManifest.xml`(`Appx` 폴더에 위치함) 파일에 활성화할 수 있는 클래스를 정확히 선언한 **&lt;InProcessServer&gt;** 요소와 바이너리 이름이 포함되어 있는지도 확인해야 합니다. 이 오류는 로컬에 구현된 런타임 클래스를 프로젝션된 형식의 기본 생성자를 통해 인스턴스화하는 실수를 저지르는 경우에도 발생할 수 있습니다. 이러한 경우 프로젝션된 형식을 올바르게 사용하는 방법에 대한 자세한 내용은 [XAML 컨트롤, C++/WinRT 속성 바인딩](binding-property.md)을 참조하세요. |
| C++ 컴파일러가 오류 메시지로 "*'implements_type': is not a member of any direct or indirect base class of '&lt;projected type&gt;'*"를 생성합니다. | 이러한 오류는 구현체 유형의 네임스페이스-비정규화 이름(예: **MyRuntimeClass**, for example)으로 **make**를 호출하고서 해당 유형의 헤더를 추가하지 않았을 때 발생할 수 있습니다. 컴파일러는 **MyRuntimeClass**를 프로젝션된 형식으로 해석합니다. 이 문제를 해결하려면 구현체 유형의 헤더(예: `MyRuntimeClass.h`)를 추가해야 합니다. |
| C++ 컴파일러가 오류 메시지인 "*attempting to reference a deleted function*"을 생성합니다. | 이 오류는 **make**를 호출하면서 템플릿 매개 변수로 전달할 구현체 유형에 `= delete` 기본 생성자가 있을 때 발생할 수 있습니다. 이때는 구현체 유형의 헤더 파일을 편집하여 `= delete`를 `= default`로 변경하세요. 또한 런타임 클래스 IDL에 생성자를 추가해도 좋습니다. |
| [**INotifyPropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged)를 실행하였지만 XAML 바인딩이 업데이트되지 않습니다(또한 UI가 [**PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)를 구독하지 않습니다). | XAML 태그의 바인딩 표현식에서 `Mode=OneWay`(또는 TwoWay)로 설정해야 합니다. [XAML 컨트롤, a C++/WinRT 속성 바인딩](binding-property.md)을 참조하세요. |
| XAML 항목 컨트롤을 관찰 가능한 컬렉션에 바인딩하고 있는 도중 런타임에서 "The parameter is incorrect" 메시지와 함께 예외가 발생하였습니다. | 이때는 IDL 및 구현체에서 관찰 가능한 컬렉션을 모두 **Windows.Foundation.Collections.IVector<IInspectable>** 유형으로 선언하세요. 단, **Windows.Foundation.Collections.IObservableVector<T>** 를 구현하는 개체는 반환해야 합니다. 여기에서 T는 요소 유형을 말합니다. [XAML 항목 컨트롤, C++/WinRT 컬렉션 바인딩](binding-collection.md)을 참조하세요.  |
| C++ 컴파일러가 "*'MyImplementationType_base&lt;MyImplementationType&gt;': no appropriate default constructor available*"이라는 오류 메시지를 생성합니다.|이 오류는 특수(non-trivial) 생성자를 가지고 있는 유형에서 파생되었을 때 발생할 수 있습니다. 이때는 파생된 유형의 생성자가 기본 유형의 생성자에게 필요한 매개 변수와 함께 전달되어야 합니다. 유효 예제는 [특수 생성자를 가지고 있는 유형에서 파생](author-apis.md#deriving-from-a-type-that-has-a-non-trivial-constructor)을 참조하세요.|
| C++ 컴파일러가 "*cannot convert from 'const std::vector&lt;std::wstring,std::allocator&lt;_Ty&gt;&gt;' to 'const winrt::param::async_iterable&lt;winrt::hstring&gt; &'*"이라는 오류 메시지를 생성합니다.|이 오류는 std::wstring에서 std::vector를 컬렉션이 필요한 Windows 런타임 API에게 전달할 때 발생할 수 있습니다. 자세한 내용은 [표준 C++ 데이터 형식 및 C++/WinRT](std-cpp-data-types.md)를 참조하세요.|
| C++ 컴파일러가 "*cannot convert from 'const std::vector&lt;winrt::hstring,std::allocator&lt;_Ty&gt;&gt;' to 'const winrt::param::async_iterable&lt;winrt::hstring&gt; &'*"이라는 오류 메시지를 생성합니다.|이 오류는 winrt::hstring에서 std::vector를 컬렉션이 필요한 비동기식 Windows 런타임 API로 전달하면서 벡터를 비동기 수신자로 복사하거나 이동시키지 않았을 때 발생할 수 있습니다. 자세한 내용은 [표준 C++ 데이터 형식 및 C++/WinRT](std-cpp-data-types.md)를 참조하세요.|
| 프로젝트를 열 때 Visual Studio에서 "*The application for the project is not installed*"라는 오류 메시지를 생성합니다.|아직 **C++ 개발용 Windows 유니버설 도구**를 설치하지 않았다면 Visual Studio의 **새 프로젝트** 대화 상자에서 설치해야 합니다. 그래도 문제가 해결되지 않으면 프로젝트가 C++/WinRT Visual Studio Extension(VSIX)에 따라 달라질 수 있습니다([C++/WinRT에 대한 Visual Studio 지원 및 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix) 참조).|
| Windows 앱 인증 키트 테스트에서 런타임 클래스 중 하나가 "*does not derive from a Windows base class. All composable classes must ultimately derive from a type in the Windows namespace*"라는 오류 메시지가 생성됩니다.|*응용 프로그램에서 선언되는* 각 런타임 클래스의 최종 기본 클래스 Windows.* 네임스페이스로 시작되는 유형이어야 합니다. 이때는 [**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject)에서 보기 모델을 파생시킬 수 있습니다. 또는 **DependencyObject**에서 파생되고, 바인딩이 가능한 기본 클래스를 선언한 후 해당 클래스에서 보기 모델을 파생시키는 방법도 있습니다.|
| C++ 컴파일러가 EventHandler 또는 TypedEventHandler 대리자 전문화에서 "*must be WinRT type*"이라는 오류 메시지를 생성합니다.|**winrt::delegate&lt;...T&gt;** 를 대신 사용하세요. [C++/WinRT의 작성자 이벤트](author-events.md)를 참조하세요.|
| C++ 컴파일러가 Windows 런타임 비동기 작업 전문화에서 "*must be WinRT type*"이라는 오류 메시지를 생성합니다.|병렬 패턴 라이브러리(PPL) [**작업**](https://msdn.microsoft.com/library/hh750113)을 대신 반환하세요. [동시성 및 비동기 작업](concurrency.md)을 참조하세요.|
| C++ 컴파일러가 "*error C2220: warning treated as error - no 'object' file generated*"라는 오류 메시지를 생성합니다.|경고를 수정하거나, 혹은 **C/C++** > **일반** > **경고를 오류로 처리**를 **아니오(/WX-)** 로 설정하세요.|
| 개체 삭제 후 C++/WinRT 개체의 이벤트 처리기가 호출되어 앱이 충돌을 일으킵니다.|[이벤트 처리기에서 *이* 개체의 사용](handle-events.md#using-the-this-object-in-an-event-handler)을 참조하세요.|
| C++ 컴파일러가 "*error C2338: This is only for weak ref support*"라는 오류 메시지를 생성합니다.|현재 **winrt::no_weak_ref** 마커 구조체를 템플릿 인수로 기본 클래스에게 전달한 유형에 대해 약한 참조를 요청하고 있습니다. [약한 참조 지원을 사용하지 않도록 옵트아웃](weak-references.md#opting-out-of-weak-reference-support)을 참조하세요.|
| C++ 링커가 C++/WinRT 프로젝션(winrt 네임스페이스에서)의 Windows 네임스페이스 헤더의 API에 대해 "*error LNK2019: Unresolved external symbol*"이라는 오류 메시지를 생성합니다.|추가한 헤더에서 API가 전방 선언되었지만 정의는 아직 추가하지 않은 헤더에 있습니다. API 네임스페이스에 따라 명명한 헤더를 추가하고 다시 빌드하세요.|