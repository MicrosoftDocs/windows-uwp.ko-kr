---
author: stevewhims
description: XAML 컨트롤에 효과적으로 바인딩되는 속성은 *관찰 가능한* 속성으로 알려져 있습니다. 이번 항목에서는 관찰 가능한 속성을 구현하여 사용하는 방법과 XAML 컨트롤에 바인딩하는 방법에 대해서 설명합니다.
title: XAML 컨트롤, C++/WinRT 속성 바인딩
ms.author: stwhi
ms.date: 08/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, XAML, 컨트롤, 바인딩, 속성
ms.localizationpriority: medium
ms.openlocfilehash: 31913ae162bfe541d04f304db87b4dff962a8af4
ms.sourcegitcommit: 914b38559852aaefe7e9468f6f53a7465bf36e30
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2018
ms.locfileid: "3388807"
---
# <a name="xaml-controls-bind-to-a-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-property"></a>XAML 컨트롤, [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 속성 바인딩
XAML 컨트롤에 효과적으로 바인딩되는 속성은 *관찰 가능한* 속성으로 알려져 있습니다. 이 아이디어는 *관찰자 패턴*이라고 알려진 소프트웨어 디자인 패턴에 바탕을 두고 있습니다. 이번 항목에서는 C++/WinRT에서 관찰 가능한 속성을 구현하는 방법과 XAML 컨트롤을 이 속성에 바인딩하는 방법에 대해서 설명합니다.

> [!IMPORTANT]
> C++/WinRT를 사용해 런타임 클래스를 사용하거나 작성하는 방법을 더욱 쉽게 이해할 수 있는 필수 개념과 용어에 대해서는 [C++/WinRT를 통한 API 사용](consume-apis.md)과 [C++/WinRT를 통한 API 작성](author-apis.md)을 참조하세요.

## <a name="what-does-observable-mean-for-a-property"></a>속성을 얘기할 때 *관찰 가능하다는 것*은 무슨 뜻입니까?
**BookSku**라고 하는 런타임 클래스에 이름이 **Title**인 속성이 있다고 가정하겠습니다. **BookSku**에서 **Title** 값이 바뀔 때마다 [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) 이벤트가 발생하도록 선택한다면 **Title**은 관찰 가능한 속성이 됩니다. 이벤트가 발생하거나 발생하지 않는 **BookSku**의 동작에 따라서 관찰 가능한 속성인지 알 수 있습니다.

XAML 텍스트 요소, 즉 컨트롤은 업데이트된 값을 가져와 새로운 값을 표시하도록 스스로 업데이트함으로써 이러한 이벤트에 바인딩하여 처리할 수 있습니다.

> [!NOTE]
> C++/WinRT Visual Studio Extension(VSIX)(프로젝트 템플릿 지원과 C++/WinRT MSBuild 속성 및 대상 제공)의 설치 및 사용에 대한 자세한 내용은 [C++/WinRT에 대한 Visual Studio 지원 및 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)를 참조하세요.

## <a name="create-a-blank-app-bookstore"></a>비어 있는 앱(Bookstore) 만들기
먼저 Microsoft Visual Studio에서 새 프로젝트를 만듭니다. **Visual C++ Blank App (C++/WinRT)** 프로젝트를 만들어서 이름을 *Bookstore*라고 지정합니다.

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
> 뷰 모델 클래스&mdash;사실, 응용 프로그램에서 선언 하는 모든 런타임 클래스&mdash;기본 클래스에서 파생 되지 필요 합니다. 위에서 선언한 **BookSku** 클래스의 예로 나와 있습니다. 인터페이스를 구현 하지만 모든 기본 클래스에서 파생 하지 않습니다.
>
> 응용 프로그램에서 선언 하는 모든 런타임 클래스는 ** 해당 기본에서 파생 클래스 라고는 *구성 가능한* 클래스입니다. 졌으 composable 클래스 제약 조건입니다. 제출의 유효성을 검사 하려면 Microsoft Store 및 Visual Studio에서 사용 하는 [Windows 앱 인증 키트](../debug-test-perf/windows-app-certification-kit.md) 테스트를 통과 하려면 응용 프로그램에 대 한 (및 따라서를 전달한 성공적으로 사용할 Microsoft Store 응용 프로그램에 대 한), composable 클래스 해야 합니다. 궁극적으로 Windows 기본 클래스에서 파생 됩니다. 의미 상속 계층의 루트에 매우 클래스 된 네임 스페이스로 시작 형식 이어야 합니다. 기본 클래스에서 런타임 클래스를 파생 해야 하는 경우&mdash;에서 보기 모델의 모든 **있음** 클래스를 구현 하는 등에 대 한&mdash;다음 [**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject)에서 파생 시킬 수 있습니다.
>
> 보기 모델은 보기의 추상화 및 보기 (XAML 태그)에 직접 바인딩되어 하므로 합니다. 데이터 모델 데이터의 추상화 이며 보기 모델 에서만에서 사용 있으며 XAML에 직접 연결 되지 않습니다. 따라서 런타임 클래스 아니라 하지만 c + + 구조체 또는 클래스 데이터 모델을 선언할 수 있습니다. 있지만 MIDL를 선언할 필요가 없습니다 하 고 원하는 모든 상속 계층 구조를 사용 하도록 합니다.

파일을 저장하고 프로젝트를 빌드합니다. 빌드 과정에서 `midl.exe` 도구가 실행되어 런타임 클래스를 설명하는 Windows 런타임 메타데이터 파일(`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`)을 생성합니다. 그런 다음 `cppwinrt.exe` 도구가 실행되어 런타임 클래스를 작성하거나 사용하도록 지원하는 소스 코드 파일을 생성합니다. 소스 코드 파일에는 IDL로 선언한 **BookSku** 런타임 클래스의 구현을 시작할 수 있는 스텁이 포함됩니다. 이 스텁이 `\Bookstore\Bookstore\Generated Files\sources\BookSku.h`와 `BookSku.cpp`입니다.

스텁 파일인 `BookSku.h`와 `BookSku.cpp`를 `\Bookstore\Bookstore\Generated Files\sources\`에서 프로젝트 폴더인 `\Bookstore\Bookstore\`로 복사합니다. **솔루션 탐색기**에서 **모든 파일 표시**가 설정되어 있는지 확인합니다. 복사한 스텁 파일을 마우스 오른쪽 버튼으로 클릭하고 **프로젝트에 포함**을 클릭합니다.

## <a name="implement-booksku"></a>**BookSku** 구현
이제 `\Bookstore\Bookstore\BookSku.h`와 `BookSku.cpp`를 열고 런타임 클래스를 구현합니다. `BookSku.h`에서 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)을 가져올 생성자, 타이틀 문자열을 저장할 전용 멤버, 그리고 타이틀 변경 시 발생되는 이벤트에 사용할 전용 멤버를 추가합니다. 이러한 변경한 후에 `BookSku.h` 은 다음과 같이 표시 됩니다.

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

**타이틀** 변경자 함수에서 여부는 값이 설정 되는 현재 값에서 다른를 확인 합니다. 하 고, 만약 그렇다면에서는 타이틀을 업데이트에 변경 된 속성 이름에는 인수와 함께 [**inotifypropertychanged:: Propertychanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) 이벤트를 발생 시킵니다. 이에 따라 사용자 인터페이스(UI)가 다시 쿼리를 실행해야 하는 속성 값을 알 수 있습니다.

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

저장 후 빌드합니다. `BookstoreViewModel.h`와 `BookstoreViewModel.cpp`를 `Generated Files` 폴더에서 프로젝트 폴더로 복사한 후 프로젝트에 추가합니다. 해당 파일을 열고 아래와 같이 런타임 클래스를 구현 합니다. 참고 어떻게에서 `BookstoreViewModel.h`를 포함 시킬 것 `BookSku.h`, 구현 체 형식 (**winrt::Bookstore::implementation::BookSku**)를 선언 합니다. 제거 하 여 기본 생성자를 복원 하는에서는 `= delete`.

```cppwinrt
// BookstoreViewModel.h
#pragma once

#include "BookstoreViewModel.g.h"
#include "BookSku.h"

namespace winrt::Bookstore::implementation
{
    struct BookstoreViewModel final : BookstoreViewModelT<BookstoreViewModel>
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

namespace winrt::Bookstore::implementation
{
    BookstoreViewModel::BookstoreViewModel()
    {
        m_bookSku = make<Bookstore::implementation::BookSku>(L"Atticus");
    }

    Bookstore::BookSku BookstoreViewModel::BookSku()
    {
        return m_bookSku;
    }
}
```

> [!NOTE]
> `m_bookSku` 형식은 프로젝션된 형식(**winrt::Bookstore::BookSku**)이며, **make**와 함께 사용하는 테플릿 매개 변수는 구현체 형식(**winrt::Bookstore::implementation::BookSku**)입니다. 그렇다 하더라도 **make**는 프로젝션된 형식 인스턴스를 반환합니다.

## <a name="add-a-property-of-type-bookstoreviewmodel-to-mainpage"></a>**BookstoreViewModel** 형식의 속성을 **MainPage**에 추가
`MainPage.idl`을 열고 메인 UI 페이지를 나타내는 런타임 클래스를 선언합니다. 가져오기 문을 추가하여 `BookstoreViewModel.idl`을 가져온 후 **BookstoreViewModel** 형식의 읽기 전용 속성(MainViewModel)을 추가합니다. 또한 **MyProperty** 속성을 제거 합니다. 또한는 `import` 지시문 아래 목록에 있습니다.

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

파일을 저장합니다. 프로젝트는 순간 완료 빌드 없습니다 수 있지만 **MainPage** 런타임 클래스가 구현 되는 소스 코드 파일을 다시 생성 때문에 작업을 수행 하는 것은 이제 빌드 (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` 및 `MainPage.cpp`). 따라서 중이면 하 고 이제 빌드하십시오. 이 단계에서 표시 되는 수 빌드 오류는 **'MainViewModel': 'winrt::Bookstore::implementation::MainPage'의 구성원이 아닙니다**.

포함을 생략 하면 `BookstoreViewModel.idl` (의 목록을 볼 `MainPage.idl` 위에), 다음 오류가 표시 됩니다 **예상 \ < "MainViewModel" 근처**. 다른 팁이 동일한 네임 스페이스의 모든 종류를 유지 하도록: 코드 목록에 표시 되는 네임 스페이스입니다.

것으로 예상 된 오류를 해결 하려면 이제 해야 생성 된 파일에서 **MainViewModel** 속성 접근자 스텁을 복사 (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` 및 `MainPage.cpp`)로 `\Bookstore\Bookstore\MainPage.h` 및 `MainPage.cpp`.

`\Bookstore\Bookstore\MainPage.h`를 포함 `BookstoreViewModel.h`, 구현 체 형식 (**winrt::Bookstore::implementation::BookstoreViewModel**)를 선언 합니다. 보기 모델을 저장할 전용 멤버를 추가 합니다. 단, 속성 접근자 함수(및 m_mainViewModel 멤버)는 프로젝션된 형식인 **Bookstore::BookstoreViewModel**과 관련하여 구현됩니다. 구현 체 형식이 동일한 프로젝트 (컴파일 단위)에 응용 프로그램으로 반환 하는 생성자 오버 로드를 통해 m_mainViewModel을 생성 하도록 `nullptr_t`. 또한 **MyProperty** 속성을 제거 합니다.

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

`\Bookstore\Bookstore\MainPage.cpp`를 프로젝션 된 형식의 새로운 인스턴스를 m_mainViewModel에 할당 (구현 체 형식 포함) [**winrt:: make**](/uwp/cpp-ref-for-winrt/make) 를 호출 합니다. 책 타이틀에 대한 초기 값을 할당합니다. MainViewModel 속성 접근자를 구현합니다. 마지막으로 버튼의 이벤트 처리기에서 책 타이틀을 업데이트합니다. 또한 **MyProperty** 속성을 제거 합니다.

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"

using namespace winrt;
using namespace Windows::UI::Xaml;

namespace winrt::Bookstore::implementation
{
    MainPage::MainPage()
    {
        m_mainViewModel = make<Bookstore::implementation::BookstoreViewModel>();
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
`MainPage.xaml`을 엽니다. 여기에는 메인 UI 페이지에 사용할 XAML 태그가 포함되어 있습니다. 아래 목록에 표시 된 것과 같이 버튼에서 이름을 제거 하 고 해당 **콘텐츠** 속성 값을 리터럴에서 바인딩 식 변경. 바인딩 식에서 `Mode=OneWay` 속성을 확인하세요(보기 모델에서 UI로 단방향). 이 속성이 없으면 UI가 속성 변경 이벤트에 응답하지 않습니다.

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

이제 프로젝트를 빌드하고 실행합니다. 버튼을 클릭하여 **Click** 이벤트 처리기를 실행합니다. 처리기가 책의 타이틀 변경자 함수를 호출합니다. 그러면 변경자가 이벤트를 발생시켜 UI가 **Title** 속성이 변경되었다는 것을 알 수 있습니다. 마지막으로 버튼이 해당 속성 값에 대한 쿼리를 다시 실행하여 **Content** 값을 업데이트합니다.

## <a name="using-the-binding-markup-extension-with-cwinrt"></a>{Binding} 태그 확장을 사용 하 여 C + + WinRT
현재 릴리스된 버전의 C + + /winrt 하려면 [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) 및 [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) 인터페이스를 구현 해야 {Binding} 태그 확장을 사용할 수 있습니다.

## <a name="important-apis"></a>중요 API
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [winrt::make 함수 템플릿](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>관련 항목
* [C++/WinRT를 통한 API 사용](consume-apis.md)
* [C++/WinRT를 통한 API 작성](author-apis.md)
