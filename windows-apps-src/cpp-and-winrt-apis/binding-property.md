---
description: XAML 컨트롤에 효과적으로 바인딩할 수 있는 속성은 *식별할 수 있는*(observable) 속성이라고 합니다. 이 항목에서는 식별할 수 있는 속성을 구현하고 사용하는 방법과 XAML 컨트롤에 바인딩하는 방법을 보여 줍니다.
title: XAML 컨트롤, C++/WinRT 속성에 바인딩
ms.date: 09/25/2020
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, XAML, 컨트롤, 바인딩, 속성
ms.localizationpriority: medium
ms.openlocfilehash: 77155b92c126f2aae7f798c8ecd67cb255182445
ms.sourcegitcommit: bcf60b6d460dc4855f207ba21da2e42644651ef6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91376241"
---
# <a name="xaml-controls-bind-to-a-cwinrt-property"></a>XAML 컨트롤, C++/WinRT 속성에 바인딩

XAML 컨트롤에 효과적으로 바인딩할 수 있는 속성은 *식별할 수 있는*(observable) 속성이라고 합니다. 이 아이디어는 ‘관찰자 패턴’이라고 알려진 소프트웨어 디자인 패턴에 바탕을 두고 있습니다. 이 항목에서는 [C++/WinRT](./intro-to-using-cpp-with-winrt.md)에서 관찰 가능한 속성을 구현하는 방법과 XAML 컨트롤을 이 속성에 바인딩하는 방법을 보여줍니다(배경 정보는 [데이터 바인딩](../data-binding/index.md) 참조).

> [!IMPORTANT]
> C++/WinRT를 사용해 런타임 클래스를 사용하거나 작성하는 방법을 더욱 쉽게 이해할 수 있는 필수 개념과 용어에 대해서는 [C++/WinRT를 통한 API 사용](consume-apis.md)과 [C++/WinRT를 통한 API 작성](author-apis.md)을 참조하세요.

## <a name="what-does-observable-mean-for-a-property"></a>속성을 얘기할 때 *관찰 가능하다는 것*은 무슨 뜻인가요?

**BookSku**라고 하는 런타임 클래스에 이름이 **Title**인 속성이 있다고 가정하겠습니다. **제목** 값이 변경될 때마다 **BookSku**에서 [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) 이벤트를 발생시키는 경우 **제목**이 관찰 가능 속성이라는 뜻입니다. 이벤트가 발생하거나 발생하지 않는 **BookSku**의 동작에 따라서 관찰 가능한 속성인지 알 수 있습니다.

XAML 텍스트 요소 또는 컨트롤은 이러한 이벤트에 바인딩하고 이러한 이벤트를 처리할 수 있습니다. 이러한 요소나 컨트롤은 업데이트된 값을 검색한 다음, 새 값을 표시하도록 자체를 업데이트하여 이벤트를 처리합니다.

> [!NOTE]
> 프로젝트 템플릿 및 빌드 지원을 함께 제공하는 C++/WinRT Visual Studio 확장(VSIX) 및 NuGet 패키지를 설치하고 사용하는 방법에 대한 자세한 내용은 [Visual Studio의 C++/WinRT 지원](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)을 참조하세요.

## <a name="create-a-blank-app-bookstore"></a>비어 있는 앱(Bookstore) 만들기

먼저 Microsoft Visual Studio에서 새 프로젝트를 만듭니다. **비어 있는 앱(C++/WinRT)** 프로젝트를 만들고 이름을 *Bookstore*라고 지정합니다. **솔루션 및 프로젝트를 같은 디렉터리에 배치**를 선택하지 않아야 합니다. 일반적으로 사용 가능한 최신(미리 보기 아님) 버전의 Windows SDK를 대상으로 합니다.

지금부터 관찰 가능한 제목 속성을 갖는 동시에 책을 표현할 새로운 클래스를 작성하려고 합니다. 동일한 컴파일 단위 내에서 클래스를 작성하고 사용합니다. 하지만 XAML에서 이 클래스에 바인딩할 수 있어야 하므로 결국 런타임 클래스가 될 것입니다. 그 밖에도 런타임 클래스를 작성하고 사용하는 데 모두 C++/WinRT를 사용합니다.

새로운 런타임 클래스를 작성하려면 먼저 새 **Midl 파일(.idl)** 항목을 프로젝트에 추가합니다. 새 항목의 이름을 `BookSku.idl`로 지정합니다. `BookSku.idl`의 기본 콘텐츠를 삭제한 후 아래 런타임 클래스 선언에 붙여넣습니다.

```idl
// BookSku.idl
namespace Bookstore
{
    runtimeclass BookSku : Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        BookSku(String title);
        String Title;
    }
}
```

> [!NOTE]
> 보기 모델 클래스(실제로는 애플리케이션에서 선언하는 런타임 클래스)는 기본 클래스에서 파생될 필요가 없습니다. 위에 선언된 **BookSku** 클래스가 해당하는 예입니다. 이 클래스는 인터페이스를 구현하지만 기본 클래스에서 파생되지 않습니다.
>
> 기본 클래스에서 ‘파생된’ 애플리케이션에서 선언하는 런타임 클래스를 ‘구성 가능’ 클래스라고 합니다. 또한 구성 가능 클래스에 관련된 제약 조건이 있습니다. 애플리케이션이 Visual Studio 및 Microsoft Store에서 제출의 유효성 검사에 사용되는 [Windows 앱 인증 키트](../debug-test-perf/windows-app-certification-kit.md) 테스트를 통과하여 Microsoft Store로 성공적으로 수집되려면 구성 가능 클래스는 기본적으로 Windows 기본 클래스에서 파생되어야 합니다. 즉, 상속 계층 구조의 루트에 있는 클래스는 Windows.* 네임스페이스에서 시작되는 형식이어야 합니다. 기본 클래스에서 런타임 클래스를 파생시켜야 하는 경우(예를 들어 파생시킬 모든 보기 모델에 대한 **BindableBase** 클래스를 구현하려면) [**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject)에서 파생시킬 수 있습니다.
>
> 보기 모델은 보기의 추상화이므로 보기(XAML 태그)에 직접 바인딩됩니다. 데이터 모델은 데이터의 추상화이며 보기 모델에서만 사용되고 XAML에 직접 바인딩되지 않습니다. 따라서 데이터 모델을 런타임 클래스가 아니라 C++ 구조체 또는 클래스로 선언할 수 있습니다. MIDL로 선언할 필요가 없으며 원하는 모든 상속 계층 구조를 자유롭게 사용할 수 있습니다.

파일을 저장하고 프로젝트를 빌드합니다. 아직은 빌드가 완전히 성공하지는 않지만, 우리에게 필요한 기능은 작동합니다. 특히 빌드 프로세스 중에 `midl.exe` 도구가 실행되어 런타임 클래스를 설명하는 Windows 런타임 메타데이터 파일(`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`)을 만듭니다. 그런 다음, `cppwinrt.exe` 도구가 실행되어 런타임 클래스를 작성하거나 사용하도록 지원하는 원본 코드 파일을 생성합니다. 원본 코드 파일에는 IDL로 선언한 **BookSku** 런타임 클래스의 구현을 시작할 수 있는 스텁이 포함됩니다. 이 스텁이 `\Bookstore\Bookstore\Generated Files\sources\BookSku.h`와 `BookSku.cpp`입니다.

프로젝트 노드를 마우스 오른쪽 단추로 클릭하고 **파일 탐색기에서 폴더 열기**를 클릭합니다. 파일 탐색기에서 프로젝트 폴더가 열립니다. 여기서 스텁 파일인 `BookSku.h` 및 `BookSku.cpp`를 `\Bookstore\Bookstore\Generated Files\sources\` 폴더에서 프로젝트 폴더인 `\Bookstore\Bookstore\`로 복사합니다. **솔루션 탐색기**에서 프로젝트 노드를 선택하고 **모든 파일 표시**가 켜져 있는지 확인합니다. 복사한 스텁 파일을 마우스 오른쪽 단추로 클릭하고 **프로젝트에 포함**을 클릭합니다.

## <a name="implement-booksku"></a>**BookSku** 구현
이제 `\Bookstore\Bookstore\BookSku.h`와 `BookSku.cpp`를 열고 런타임 클래스를 구현합니다. 먼저 `BookSku.h` 및 `BookSku.cpp`의 위쪽에 `static_assert`가 보이는데, 이것을 제거해야 합니다.

다음으로 `BookSku.h`에서 다음과 같이 변경합니다.

- 기본 생성자에서 `= default`를 `= delete`로 변경합니다. 기본 생성자가 필요 없기 때문입니다.
- 제목 문자열을 저장할 프라이빗 멤버를 추가합니다. [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 값을 사용하는 생성자가 생겼습니다. 이 값은 제목 문자열입니다.
- 제목이 변경될 때 발생시킬 이벤트에 대한 또 다른 프라이빗 멤버를 추가합니다.

이 변경을 수행한 후 `BookSku.h`가 다음과 같이 표시됩니다.

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
namespace winrt::Bookstore::factory_implementation
{
    struct BookSku : BookSkuT<BookSku, implementation::BookSku>
    {
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

**Title** 변경자(mutator) 함수에서 값이 현재 값과 다르게 설정되어 있는지 확인합니다. 다르게 설정되어 있으면 제목을 업데이트하고 변경된 속성의 이름과 같은 인수를 사용하여 [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) 이벤트를 발생시킵니다. 이렇게 하면 UI(사용자 인터페이스)가 다시 쿼리할 속성 값을 인식할 수 있습니다.

프로젝트가 지금 다시 빌드됩니다. 원한다면 확인할 수 있습니다.

## <a name="declare-and-implement-bookstoreviewmodel"></a>**BookstoreViewModel** 선언 및 구현
이제 기본 XAML 페이지가 주요 보기 모델에 바인딩됩니다. 이 보기 모델은 **BookSku** 형식 중 하나를 포함해 여러 속성을 갖게 됩니다. 이번 단계에서는 주요 보기 모델 런타임 클래스를 선언하고 구현합니다.

`BookstoreViewModel.idl`이라는 이름의 새 **Midl 파일(.idl)** 항목을 추가합니다. 또한 [런타임 클래스를 Midl 파일(.idl)로 팩터링](./author-apis.md#factoring-runtime-classes-into-midl-files-idl)도 참조하세요.

```idl
// BookstoreViewModel.idl
import "BookSku.idl";

namespace Bookstore
{
    runtimeclass BookstoreViewModel
    {
        BookstoreViewModel();
        BookSku BookSku{ get; };
    }
}
```

저장 및 빌드합니다. 아직은 빌드가 완전히 성공하지 않지만, 빌드하는 이유는 스텁 파일을 다시 생성하는 것입니다.

`BookstoreViewModel.h`와 `BookstoreViewModel.cpp`를 `Generated Files\sources` 폴더에서 프로젝트 폴더로 복사한 후 프로젝트에 추가합니다. 이 파일을 열고(`static_assert`를 다시 제거) 아래와 같이 런타임 클래스를 구현합니다. **BookSku**의 구현 형식(**winrt::Bookstore::implementation::BookSku**)을 선언하는 `BookstoreViewModel.h`에 `BookSku.h`를 포함하는 방법을 확인합니다. 또한 기본 생성자에서 `= default`를 제거합니다.

> [!NOTE]
> `BookstoreViewModel.h` 및 `BookstoreViewModel.cpp`에 대한 아래 목록에서 이 코드는 데이터 멤버 *m_bookSku*를 생성하는 기본 방법을 보여줍니다. 이 메서드는 C++/WinRT의 첫 번째 릴리스에서 제공하는 것이며, 이 패턴을 숙지하는 것이 좋습니다. C++/WinRT 버전 2.0 이상에서는 *균일한 생성*으로 알려진 최적화된 생성 형식을 사용할 수 있습니다([C++/WinRT 2.0의 새로운 기능 및 변경 내용](./news.md#news-and-changes-in-cwinrt-20) 참조). 이 토픽의 뒷부분에서 균일한 생성의 예를 보여드리겠습니다.

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
namespace winrt::Bookstore::factory_implementation
{
    struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel, implementation::BookstoreViewModel>
    {
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
> `m_bookSku` 형식은 프로젝션된 형식(**winrt::Bookstore::BookSku**)이며, [**winrt::make**](/uwp/cpp-ref-for-winrt/make)와 함께 사용하는 템플릿 매개 변수는 구현 형식(**winrt::Bookstore::implementation::BookSku**)입니다. 그렇다 하더라도 **make**는 프로젝션된 형식 인스턴스를 반환합니다.

프로젝트가 다시 빌드됩니다.

## <a name="add-a-property-of-type-bookstoreviewmodel-to-mainpage"></a>**BookstoreViewModel** 형식의 속성을 **MainPage**에 추가
`MainPage.idl`을 열고 기본 UI 페이지를 나타내는 런타임 클래스를 선언합니다.

- `BookstoreViewModel.idl`을 가져오도록 `import` 지시문을 추가합니다.
- **BookstoreViewModel** 형식의 읽기 전용 속성 **MainViewModel**을 추가합니다.
- **MyProperty** 속성을 제거합니다.

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

파일을 저장합니다. 아직은 프로젝트가 완전히 빌드되지 않지만, 지금 빌드하면 **MainPage** 런타임 클래스가 구현되는 원본 코드 파일(`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` 및 `MainPage.cpp`)이 생성되므로 유용합니다. 따라서 지금 바로 빌드합니다. 이 스테이지에서 예상할 수 있는 빌드 오류는 **‘MainViewModel’: ‘winrt::Bookstore::implementation::MainPage’의 멤버가 아님**입니다.

`BookstoreViewModel.idl`의 include를 생략하면(위의 `MainPage.idl` 목록 참조) 오류 **“MainViewModel” 근처에 \< 필요**가 표시됩니다. 또 다른 팁은 모든 형식을 코드 목록에 표시된 동일한 네임스페이스에 남겨 두어야 한다는 것입니다.

표시될 것으로 예상하는 오류를 해결하려면 **MainViewModel** 속성의 접근자 스텁을 생성된 파일(`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` 및 `MainPage.cpp`)에서 복사하여 `\Bookstore\Bookstore\MainPage.h` 및 `MainPage.cpp`에 붙여넣습니다. 이 작업을 수행하는 단계는 다음과 같이 설명됩니다.

`\Bookstore\Bookstore\MainPage.h`에서 다음 단계를 수행합니다.

- **BookstoreViewModel**의 구현 형식(**winrt::Bookstore::implementation::BookstoreViewModel**)을 선언하는 `BookstoreViewModel.h`를 포함합니다.
- 보기 모델을 저장할 프라이빗 멤버를 추가합니다. 단, 속성 접근자 함수(및 *m_mainViewModel* 멤버)는 **BookstoreViewModel**의 프로젝션된 형식(**Bookstore::BookstoreViewModel**)을 기반으로 구현됩니다.
- 구현 형식이 애플리케이션과 동일한 프로젝트(컴파일 단위)에 있으므로 **std::nullptr_t**를 가져오는 생성자 오버로드를 통해 *m_mainViewModel*을 생성합니다.
- **MyProperty** 속성을 제거합니다.

> [!NOTE]
> `MainPage.h` 및 `MainPage.cpp`에 대한 아래 목록 쌍에서 이 코드는 데이터 멤버 *m_mainViewModel*을 생성하는 기본 방법을 보여줍니다. 이어지는 섹션에서는 균일한 생성을 사용하는 버전을 보여드리겠습니다.

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

아래 목록에 표시된 것처럼 `\Bookstore\Bookstore\MainPage.cpp`에서 다음과 같이 변경합니다.

- [**winrt::make**](/uwp/cpp-ref-for-winrt/make)(**BookstoreViewModel** 구현 형식 포함)를 호출하여 프로젝션된 **BookstoreViewModel** 형식의 새 인스턴스를 *m_mainViewModel*에 할당합니다. 위에서 살펴본 것처럼, **BookstoreViewModel** 생성자는 새 **BookSku** 개체를 프라이빗 데이터 멤버로 만들고, 처음에는 제목을 `L"Atticus"`로 설정합니다.
- 단추의 이벤트 처리기(**ClickHandler**)에서 책의 제목을 게시된 제목으로 업데이트합니다.
- **MainViewModel** 속성의 접근자를 구현합니다.
- **MyProperty** 속성을 제거합니다.

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

### <a name="uniform-construction"></a>균일한 생성
[**winrt::make**](/uwp/cpp-ref-for-winrt/make) 대신 균일한 생성을 사용하려면 아래와 같이 `MainPage.h`에서 한 번에 *m_mainViewModel*을 선언하고 초기화합니다.

```cppwinrt
// MainPage.h
...
#include "BookstoreViewModel.h"
...
struct MainPage : MainPageT<MainPage>
{
    ...
private:
    Bookstore::BookstoreViewModel m_mainViewModel;
};
...
```

그러면 `MainPage.cpp`의 **MainPage** 생성자에서 `m_mainViewModel = winrt::make<Bookstore::implementation::BookstoreViewModel>();`가 필요 없습니다.

균일한 생성에 대한 자세한 내용과 코드 예제는 [균일한 생성 및 직접 구현 액세스 옵트인](./author-apis.md#opt-in-to-uniform-construction-and-direct-implementation-access)을 참조하세요.

## <a name="bind-the-button-to-the-title-property"></a>단추를 **Title** 속성에 바인딩
`MainPage.xaml`을 엽니다. 여기에는 기본 UI 페이지에 사용할 XAML 태그가 포함되어 있습니다. 아래 목록과 같이, 단추에서 이름을 제거하고 해당 **Content** 속성 값을 리터럴에서 바인딩 식으로 변경합니다. 바인딩 식에서 `Mode=OneWay` 속성을 확인합니다(보기 모델에서 UI로 단방향). 이 속성이 없으면 UI가 속성 변경 이벤트에 응답하지 않습니다.

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

이제 프로젝트를 빌드하고 실행합니다. 단추를 클릭하여 **Click** 이벤트 처리기를 실행합니다. 처리기가 책의 제목 변경자 함수를 호출합니다. 그러면 변경자가 이벤트를 발생시켜 UI가 **Title** 속성이 변경되었다는 것을 알 수 있습니다. 마지막으로 단추가 해당 속성 값에 대한 쿼리를 다시 실행하여 **Content** 값을 업데이트합니다.

## <a name="using-the-binding-markup-extension-with-cwinrt"></a>C++/WinRT와 함께 {Binding} 태그 확장 사용
현재 릴리스된 C++/WinRT 버전의 경우 {Binding} 태그 확장을 사용하려면 [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) 및 [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) 인터페이스를 구현해야 합니다.

## <a name="element-to-element-binding"></a>요소 간 바인딩

한 XAML 요소의 속성을 다른 XAML 요소의 속성에 바인딩할 수 있습니다. 태그에서는 다음과 같이 표시됩니다.

```xaml
<TextBox x:Name="myTextBox" />
<TextBlock Text="{x:Bind myTextBox.Text, Mode=OneWay}" />
```

명명된 XAML 엔터티 `myTextBox`를 Midl 파일(.idl)에서 읽기 전용 속성으로 선언해야 합니다.

```idl
// MainPage.idl
runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
{
    MainPage();
    Windows.UI.Xaml.Controls.TextBox myTextBox{ get; };
}
```

이렇게 해야 하는 이유는 다음과 같습니다. XAML 컴파일러에서 유효성을 검사해야 하는 모든 형식([{x:Bind}](../xaml-platform/x-bind-markup-extension.md)에서 사용되는 형식 포함)을 Windows 메타 데이터(WinMD)에서 읽어옵니다. 사용자는 Midl 파일에 읽기 전용 속성을 추가하기만 하면 됩니다. 구현하지 않도록 합니다. 자동 생성된 XAML 코드 숨김에서 자동으로 구현을 제공하기 때문입니다.

## <a name="consuming-objects-from-xaml-markup"></a>XAML 태그에서 개체 사용

XAML [ **{x:Bind} 태그 확장**](../xaml-platform/x-bind-markup-extension.md)을 통해 사용되는 모든 엔터티는 IDL에 공개적으로 노출되어야 합니다. 또한 XAML 태그에 또 다른 요소에 대한 참조도 포함되어 있는 경우 해당 태그에 대한 getter가 IDL에 있어야 합니다.

```xaml
<Page x:Name="MyPage">
    <StackPanel>
        <CheckBox x:Name="UseCustomColorCheckBox" Content="Use custom color"
             Click="UseCustomColorCheckBox_Click" />
        <Button x:Name="ChangeColorButton" Content="Change color"
            Click="{x:Bind ChangeColorButton_OnClick}"
            IsEnabled="{x:Bind UseCustomColorCheckBox.IsChecked.Value, Mode=OneWay}"/>
    </StackPanel>
</Page>
```

*ChangeColorButton* 요소는 바인딩을 통해 *UseCustomColorCheckBox* 요소를 참조합니다. 따라서 바인딩에 액세스할 수 있도록 이 페이지의 IDL에서 *UseCustomColorCheckBox*라는 읽기 전용 속성을 선언해야 합니다.

*UseCustomColorCheckBox*에 대한 클릭 이벤트 처리기 대리자는 클래식 XAML 대리자 구문을 사용하므로 IDL의 항목이 필요하지 않으며 단지 구현 클래스에서 public으로만 선언하면 됩니다. 반면, *ChangeColorButton*에는 IDL에도 포함되어야 하는 `{x:Bind}` 클릭 이벤트 처리기도 있습니다.

```idl
runtimeclass MyPage : Windows.UI.Xaml.Controls.Page
{
    MyPage();

    // These members are consumed by binding.
    void ChangeColorButton_OnClick();
    Windows.UI.Xaml.Controls.CheckBox UseCustomColorCheckBox{ get; };
}
```

**UseCustomColorCheckBox** 속성에 대한 구현은 제공할 필요가 없습니다. XAML 코드 생성기에서 이를 수행합니다.

### <a name="binding-to-boolean"></a>부울에 바인딩

이 작업은 진단 모드에서 수행할 수 있습니다.

<syntaxhighlight lang="xml">
<TextBlock Text="{Binding CanPair}"/>
</syntaxhighlight>

이 경우 C++/CX에서는 `true` 또는 `false`가 표시되지만, C++/WinRT에서는 **Windows.Foundation.IReference`1<Boolean>** 이 표시됩니다.

부울에 바인딩하는 경우 `x:Bind`를 사용합니다.

```xaml
<TextBlock Text="{x:Bind CanPair}"/>
```

## <a name="important-apis"></a>중요 API
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [winrt::make 함수 템플릿](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>관련 항목
* [C++/WinRT를 통한 API 사용](consume-apis.md)
* [C++/WinRT를 통한 API 작성](author-apis.md)