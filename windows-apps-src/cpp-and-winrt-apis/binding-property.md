---
description: XAML 컨트롤에 효과적으로 바인딩되는 속성은 *관찰 가능한* 속성으로 알려져 있습니다. 이 항목에서는 관찰 가능한 속성을 구현하여 사용하는 방법과 XAML 컨트롤에 바인딩하는 방법에 대해서 설명합니다.
title: XAML 컨트롤, C++/WinRT 속성 바인딩
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, XAML, 컨트롤, 바인딩, 속성
ms.localizationpriority: medium
ms.openlocfilehash: 2fe5c03eebd2b68e98ae908ea4624471fbd2b3d2
ms.sourcegitcommit: d23dab1533893b7fe0f01ca6eb273edfac4705e6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/15/2019
ms.locfileid: "65627664"
---
# <a name="xaml-controls-bind-to-a-cwinrt-property"></a>XAML 컨트롤, C++/WinRT 속성 바인딩
XAML 컨트롤에 효과적으로 바인딩되는 속성은 *관찰 가능한* 속성으로 알려져 있습니다. 이 아이디어는 *관찰자 패턴*이라고 알려진 소프트웨어 디자인 패턴에 바탕을 두고 있습니다. 이 항목에서는에서 관측 가능한 속성을 구현 하는 방법을 보여 줍니다 [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), 및 XAML 컨트롤에 바인딩하는 방법입니다.

> [!IMPORTANT]
> C++/WinRT를 사용해 런타임 클래스를 사용하거나 작성하는 방법을 더욱 쉽게 이해할 수 있는 필수 개념과 용어에 대해서는 [C++/WinRT를 통한 API 사용](consume-apis.md)과 [C++/WinRT를 통한 API 작성](author-apis.md)을 참조하세요.

## <a name="what-does-observable-mean-for-a-property"></a>속성을 얘기할 때 *관찰 가능하다는 것*은 무슨 뜻입니까?
**BookSku**라고 하는 런타임 클래스에 이름이 **Title**인 속성이 있다고 가정하겠습니다. **BookSku**에서 **Title** 값이 바뀔 때마다 [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) 이벤트가 발생하도록 선택한다면 **Title**은 관찰 가능한 속성이 됩니다. 이벤트가 발생하거나 발생하지 않는 **BookSku**의 동작에 따라서 관찰 가능한 속성인지 알 수 있습니다.

XAML 텍스트 요소, 즉 컨트롤은 업데이트된 값을 가져와 새로운 값을 표시하도록 스스로 업데이트함으로써 이러한 이벤트에 바인딩하여 처리할 수 있습니다.

> [!NOTE]
> 설치 및 사용에 대 한 정보는 C++WinRT Visual Studio 확장 (VSIX) 및 (있으며 함께 프로젝트 템플릿을 제공 지원도) NuGet 패키지를 참조 하세요 [Visual Studio 지원에 대 한 C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)합니다.

## <a name="create-a-blank-app-bookstore"></a>비어 있는 앱(Bookstore) 만들기
먼저 Microsoft Visual Studio에서 새 프로젝트를 만듭니다. 만들기는 **비어 있는 앱 (C++/WinRT)** 프로젝트를 만들고 이름을 *Bookstore*합니다.

지금부터 관찰 가능한 타이틀 속성을 갖는 동시에 책을 표현할 새로운 클래스를 작성하려고 합니다. 또한 동일한 컴파일 단위 내에서 클래스를 작성하고 사용할 계획입니다. 하지만 XAML에서 이 클래스에 바인딩할 수 있어야 하므로 결국 런타임 클래스가 될 것입니다. 그 밖에도 런타임 클래스를 작성하고 사용하는 데 모두 C++/WinRT를 사용합니다.

새로운 런타임 클래스를 작성하려면 먼저 새 **Midl 파일(.idl)** 항목을 프로젝트에 추가합니다. 이름을 `BookSku.idl`이라고 지정합니다. `BookSku.idl`의 기본 내용을 삭제한 후 아래 런타임 클래스 선언을 붙여 넣습니다.

```idl
// BookSku.idl
namespace Bookstore
{
    runtimeclass BookSku : Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        String Title;
    }
}
```

> [!NOTE]
> 보기 모델 클래스&mdash;사실 응용 프로그램에서 선언 하는 모든 런타임 클래스&mdash;기본 클래스에서 파생 되지 해야 합니다. 합니다 **BookSku** 위에 선언 된 클래스의 예로 합니다. 인터페이스를 구현 하지만 모든 기본 클래스에서 파생 되지 않습니다.
>
> 응용 프로그램에서 선언 하는 모든 런타임 클래스는 *않습니다* 기본에서 파생 클래스 라고는 *구성 가능* 클래스입니다. 및 관련 구성 가능 클래스 제약 조건이 있습니다. 전달할 응용 프로그램에 대 한 합니다 [Windows 앱 인증 키트](../debug-test-perf/windows-app-certification-kit.md) 제출의 유효성을 검사 하려면 Visual Studio 및 Microsoft Store 사용 하는 테스트 (및 따라서 성공적으로 Microsoft Store 수집 하려면 응용 프로그램에 대 한), 구성 가능한 클래스는 Windows 기본 클래스에서 최종적으로 파생 되어야 합니다. 상속 계층 구조의 매우 루트 클래스는 windows. * 네임 스페이스에서 시작 된 형식 이어야 합니다는 의미를 갖습니다. 기본 클래스에서 런타임 클래스를 파생 해야 하는 경우&mdash;예를 들어, 구현 하는 **BindableBase** 클래스를 파생할 보기 모델의 모든&mdash;에서 파생할 수 있습니다 다음 [ **Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject)합니다.
>
> 보기 모델은 뷰의 추상화 및 보기 (XAML 태그)에 직접 바인딩되는지 하므로 합니다. 데이터 모델을 데이터 추상화 이며 모델 보기 에서만에서 사용 되 고 XAML에 직접 바인딩되지 않습니다. 따라서 런타임 클래스가 아니라 아니라 데이터 모델 선언할 수 있습니다 C++ 구조체 또는 클래스. MIDL에서 선언할 필요가 없습니다 하 고 원하는 모든 상속 계층 구조를 사용 하도록 합니다.

파일을 저장하고 프로젝트를 빌드합니다. 빌드 과정에서 `midl.exe` 도구가 실행되어 런타임 클래스를 설명하는 Windows 런타임 메타데이터 파일(`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`)을 생성합니다. 그런 다음 `cppwinrt.exe` 도구가 실행되어 런타임 클래스를 작성하거나 사용하도록 지원하는 소스 코드 파일을 생성합니다. 소스 코드 파일에는 IDL로 선언한 **BookSku** 런타임 클래스의 구현을 시작할 수 있는 스텁이 포함됩니다. 이 스텁이 `\Bookstore\Bookstore\Generated Files\sources\BookSku.h`와 `BookSku.cpp`입니다.

프로젝트 노드를 마우스 오른쪽 단추로 클릭 하 고 클릭 **파일 탐색기에서 폴더 열기**합니다. 이 파일 탐색기에서 프로젝트 폴더를 엽니다. 여기서 스텁 파일을 복사 `BookSku.h` 및 `BookSku.cpp` 에서 합니다 `\Bookstore\Bookstore\Generated Files\sources\` 폴더 이며 프로젝트 폴더에는 `\Bookstore\Bookstore\`합니다. **솔루션 탐색기**, 프로젝트 노드를 선택 했는지 **모든 파일 표시** 에 설정/해제 합니다. 복사한 스텁 파일을 마우스 오른쪽 버튼으로 클릭하고 **프로젝트에 포함**을 클릭합니다.

## <a name="implement-booksku"></a>**BookSku** 구현
이제 `\Bookstore\Bookstore\BookSku.h`와 `BookSku.cpp`를 열고 런타임 클래스를 구현합니다. `BookSku.h`에서 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)을 가져올 생성자, 타이틀 문자열을 저장할 전용 멤버, 그리고 타이틀 변경 시 발생되는 이벤트에 사용할 전용 멤버를 추가합니다. 이러한 변경을 수행한 후에 `BookSku.h` 다음과 같이 표시 됩니다.

```cppwinrt
// BookSku.h
#pragma once
#include "BookSku.g.h"

namespace winrt::Bookstore::implementation
{
    struct BookSku : BookSkuT<BookSku>
    {
        BookSku() = delete;
        BookSku(winrt::hstring const& title);

        winrt::hstring Title();
        void Title(winrt::hstring const& value);
        winrt::event_token PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& value);
        void PropertyChanged(winrt::event_token const& token);
    
    private:
        winrt::hstring m_title;
        winrt::event<Windows::UI::Xaml::Data::PropertyChangedEventHandler> m_propertyChanged;
    };
}
```

`BookSku.cpp`에서 아래와 같이 함수를 구현합니다.

```cppwinrt
// BookSku.cpp
#include "pch.h"
#include "BookSku.h"
#include "BookSku.g.cpp"

namespace winrt::Bookstore::implementation
{
    BookSku::BookSku(winrt::hstring const& title) : m_title{ title }
    {
    }

    winrt::hstring BookSku::Title()
    {
        return m_title;
    }

    void BookSku::Title(winrt::hstring const& value)
    {
        if (m_title != value)
        {
            m_title = value;
            m_propertyChanged(*this, Windows::UI::Xaml::Data::PropertyChangedEventArgs{ L"Title" });
        }
    }

    winrt::event_token BookSku::PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler)
    {
        return m_propertyChanged.add(handler);
    }

    void BookSku::PropertyChanged(winrt::event_token const& token)
    {
        m_propertyChanged.remove(token);
    }
}
```

에 **Title** 변경자 (mutator) 함수에서는 값 설정 되어 있는지 확인 되는 현재 값을 다른 합니다. 따라서 제목을 업데이트 하 고도 발생 하는 경우는 [ **INotifyPropertyChanged::PropertyChanged** ](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) 변경 된 속성의 이름과 동일한 인수를 사용 하 여 이벤트입니다. 이에 따라 사용자 인터페이스(UI)가 다시 쿼리를 실행해야 하는 속성 값을 알 수 있습니다.

## <a name="declare-and-implement-bookstoreviewmodel"></a>**BookstoreViewModel** 선언 및 구현
이제 메인 XAML 페이지가 주요 보기 모델에 바인딩됩니다. 이 보기 모델은 **BookSku** 형식 중 하나를 포함해 여러 속성을 갖게 됩니다. 이번 단계에서는 주요 보기 모엘 런타임 클래스를 선언하고 구현합니다.

`BookstoreViewModel.idl`이라는 이름의 새 **Midl 파일(.idl)** 항목을 추가합니다.

```idl
// BookstoreViewModel.idl
import "BookSku.idl";

namespace Bookstore
{
    runtimeclass BookstoreViewModel
    {
        BookSku BookSku{ get; };
    }
}
```

저장 후 빌드합니다. `BookstoreViewModel.h`와 `BookstoreViewModel.cpp`를 `Generated Files\sources` 폴더에서 프로젝트 폴더로 복사한 후 프로젝트에 추가합니다. 해당 파일을 열고 아래와 같이 런타임 클래스를 구현 합니다. 참고 하는 방법, `BookstoreViewModel.h`, 포함 되 고 `BookSku.h`에 대 한 구현 유형을 선언 하는 **BookSku** (되 **winrt::Bookstore::implementation::BookSku**). 및에서는 제거 중인 `= default` 기본 생성자에서.

```cppwinrt
// BookstoreViewModel.h
#pragma once
#include "BookstoreViewModel.g.h"
#include "BookSku.h"

namespace winrt::Bookstore::implementation
{
    struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
    {
        BookstoreViewModel();

        Bookstore::BookSku BookSku();

    private:
        Bookstore::BookSku m_bookSku{ nullptr };
    };
}
```

```cppwinrt
// BookstoreViewModel.cpp
#include "pch.h"
#include "BookstoreViewModel.h"
#include "BookstoreViewModel.g.cpp"

namespace winrt::Bookstore::implementation
{
    BookstoreViewModel::BookstoreViewModel()
    {
        m_bookSku = winrt::make<Bookstore::implementation::BookSku>(L"Atticus");
    }

    Bookstore::BookSku BookstoreViewModel::BookSku()
    {
        return m_bookSku;
    }
}
```

> [!NOTE]
> 유형의 `m_bookSku` 는 프로젝션 된 형식 (**winrt::Bookstore::BookSku**), 및 사용 하는 템플릿 매개 변수 [ **winrt::make** ](/uwp/cpp-ref-for-winrt/make) 는 구현 형식 (**winrt::Bookstore::implementation::BookSku**). 그렇다 하더라도 **make**는 프로젝션된 형식 인스턴스를 반환합니다.

## <a name="add-a-property-of-type-bookstoreviewmodel-to-mainpage"></a>**BookstoreViewModel** 형식의 속성을 **MainPage**에 추가
`MainPage.idl`을 열고 메인 UI 페이지를 나타내는 런타임 클래스를 선언합니다. 가져오기 문을 추가하여 `BookstoreViewModel.idl`을 가져온 후 **BookstoreViewModel** 형식의 읽기 전용 속성(MainViewModel)을 추가합니다. 도 제거 합니다 **MyProperty** 속성입니다. 또한는 `import` 아래 목록에서 지시문입니다.

```idl
// MainPage.idl
import "BookstoreViewModel.idl";

namespace Bookstore
{
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

파일을 저장합니다. 지금은 완료 될 때까지 프로젝트를 빌드하지 않습니다 이지만 지금 빌드는 소스 코드 파일을 자동으로 다시 생성 하기 때문에 작업을 수행 하는 것은 **MainPage** 런타임 클래스에서 구현 됩니다 (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` 및 `MainPage.cpp`). 계속 해 서 지금 빌드 하므로. 이 단계에서 참조 될 수 있습니다 빌드 오류가 **'MainViewModel': 'winrt::Bookstore::implementation::MainPage'의 구성원이 아닌**합니다.

포함을 생략 하면 `BookstoreViewModel.idl` (목록을 참조 하세요 `MainPage.idl` 위에), 오류가 표시 됩니다 **예상 \< "MainViewModel" 근처**합니다. 동일한 네임 스페이스에 모든 형식을 그대로 둘 수 있는지 확인 하 여: 코드 샘플에 표시 되는 네임 스페이스입니다.

것으로 예상 하는 오류를 해결 하려면 이제 해야 한 접근자 스텁이 자동으로 복사 합니다 **MainViewModel** 생성 된 파일 속성 (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` 및 `MainPage.cpp`) 및 `\Bookstore\Bookstore\MainPage.h` 및 `MainPage.cpp`합니다. 작업을 수행 하는 단계는 다음과 같습니다.

`\Bookstore\Bookstore\MainPage.h`를 포함 `BookstoreViewModel.h`에 대 한 구현 유형을 선언 하는 **BookstoreViewModel** (되 **winrt::Bookstore::implementation::BookstoreViewModel**). 뷰 모델을 저장 하려면 private 멤버를 추가 합니다. 속성 접근자 함수 (및 멤버 m_mainViewModel)에 대 한 예상된 형식 측면에서 구현 됩니다 **BookstoreViewModel** (즉 **Bookstore::BookstoreViewModel**). 구현 형식 이므로 동일한 프로젝트 (컴파일 단위)에 응용 프로그램과 생성자 오버 로드를 통해 m_mainViewModel 생성 `nullptr_t`합니다. 도 제거 합니다 **MyProperty** 속성입니다.

```cppwinrt
// MainPage.h
...
#include "BookstoreViewModel.h"
...
namespace winrt::Bookstore::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        Bookstore::BookstoreViewModel MainViewModel();

        void ClickHandler(Windows::Foundation::IInspectable const&, Windows::UI::Xaml::RoutedEventArgs const&);

    private:
        Bookstore::BookstoreViewModel m_mainViewModel{ nullptr };
    };
}
...
```

`\Bookstore\Bookstore\MainPage.cpp`, 호출 [ **winrt::make** ](/uwp/cpp-ref-for-winrt/make) (구현 형식과) m_mainViewModel 프로젝션 된 형식의 새 인스턴스를 할당할 합니다. 책 타이틀에 대한 초기 값을 할당합니다. MainViewModel 속성 접근자를 구현합니다. 마지막으로 버튼의 이벤트 처리기에서 책 타이틀을 업데이트합니다. 도 제거 합니다 **MyProperty** 속성입니다.

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

using namespace winrt;
using namespace Windows::UI::Xaml;

namespace winrt::Bookstore::implementation
{
    MainPage::MainPage()
    {
        m_mainViewModel = winrt::make<Bookstore::implementation::BookstoreViewModel>();
        InitializeComponent();
    }

    void MainPage::ClickHandler(Windows::Foundation::IInspectable const& /* sender */, Windows::UI::Xaml::RoutedEventArgs const& /* args */)
    {
        MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
    }

    Bookstore::BookstoreViewModel MainPage::MainViewModel()
    {
        return m_mainViewModel;
    }
}
```

## <a name="bind-the-button-to-the-title-property"></a>버튼을 **Title** 속성에 바인딩
`MainPage.xaml`을 엽니다. 여기에는 메인 UI 페이지에 사용할 XAML 태그가 포함되어 있습니다. 아래 목록에 표시 된 것과 같이 단추에서 이름을 제거 하 고 변경 해당 **콘텐츠** 바인딩 식에서 리터럴 속성 값입니다. 바인딩 식에서 `Mode=OneWay` 속성을 확인하세요(보기 모델에서 UI로 단방향). 이 속성이 없으면 UI가 속성 변경 이벤트에 응답하지 않습니다.

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

이제 프로젝트를 빌드하고 실행합니다. 버튼을 클릭하여 **Click** 이벤트 처리기를 실행합니다. 처리기가 책의 타이틀 변경자 함수를 호출합니다. 그러면 변경자가 이벤트를 발생시켜 UI가 **Title** 속성이 변경되었다는 것을 알 수 있습니다. 마지막으로 버튼이 해당 속성 값에 대한 쿼리를 다시 실행하여 **Content** 값을 업데이트합니다.

## <a name="using-the-binding-markup-extension-with-cwinrt"></a>사용 하 여 {Binding} 태그 확장을 사용 하 여 C++/WinRT
현재 릴리스된 버전의 C++를 구현 해야 합니다. {Binding} 태그 확장을 사용할 수 있도록 /WinRT 합니다 [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) 하 고 [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) 인터페이스입니다.

## <a name="important-apis"></a>중요 API
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [winrt::make 함수 템플릿](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>관련 항목
* [C++/WinRT를 통한 API 사용](consume-apis.md)
* [C++/WinRT를 통한 API 작성](author-apis.md)
