---
title: 탐색 시작
description: 탐색 시작
ms.assetid: F4DF5C5F-C886-4483-BBDA-498C4E2C1BAF
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: de9c5261afb7b76b2409599c9c1f88814d1dd6a1
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371787"
---
# <a name="getting-started-navigation"></a>시작: 탐색


## <a name="adding-navigation"></a>탐색 추가

iOS는 앱 내 탐색에 도움이 되는 **UINavigationController** 클래스를 제공합니다. 보기를 푸시하고 팝업하여 앱을 정의하는 **UIViewControllers** 계층을 만들 수 있습니다.

반면, 여러 뷰가 포함 된 Windows 10 앱을 많이 걸리는 웹 사이트 방식의 탐색 합니다. 사용자는 컨트롤을 클릭하여 앱 페이지를 탐색할 수 있습니다. 자세한 내용은 [탐색 디자인 기본 사항](https://docs.microsoft.com/windows/uwp/layout/navigation-basics)을 참조하세요.

Windows 10 앱에서이 탐색을 관리 하는 방법 중 하나를 사용 하는 것은 [ **프레임** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) 클래스입니다. 다음 연습에서는 이 작업을 수행하는 방법을 보여 줍니다.

이전에 시작한 솔루션으로 계속합니다. **MainPage.xaml** 파일을 열고 **디자인** 뷰에 단추를 추가합니다. 단추의 **Content** 속성을 "Button"에서 "Go To Page"로 변경합니다. 다음 그림과 같이 단추의 **Click** 이벤트에 대한 처리기를 만듭니다. 이렇게 하는 방법이 기억나지 않는 경우 이전 섹션의 연습을 검토하세요(힌트: **디자인** 뷰에서 단추를 두 번 클릭).

![Visual Studio에서 단추 및 해당 Click 이벤트 추가](images/ios-to-uwp/vs-go-to-page.png)

새 페이지를 추가해 보겠습니다. **솔루션** 뷰에서 **프로젝트** 메뉴를 탭한 다음 **새 항목 추가**를 탭합니다. 다음 그림과 같이 **빈 페이지**를 탭한 다음 **추가**를 탭합니다.

![Visual Studio에서 새 페이지 추가](images/ios-to-uwp/vs-add-new-page.png)

이제 BlankPage.xaml 파일에 단추를 추가합니다. AppBarButton 컨트롤을 사용하고 뒤로 화살표 이미지를 제공하겠습니다. **XAML** 보기에서 `<Grid> </Grid>` 요소 사이에 ` <AppBarButton Icon="Back"/>`를 추가합니다.

이제 단추에 이벤트 처리기를 추가 하겠습니다:에서 컨트롤을 두 번 클릭 합니다 **디자인** 보기 및 Microsoft Visual Studio 텍스트를 추가 합니다 "AppBarButton\_클릭"에 **클릭** 상자에 표시 된 대로 다음 그림에서는 로컬 폴더를 더하고 BlankPage.xaml.cs 파일의 해당 이벤트 처리기를 표시 합니다.

![Visual Studio에서 뒤로 단추 및 해당 Click 이벤트 추가](images/ios-to-uwp/vs-add-back-button.png)

BlankPage.xaml 파일의 **XAML** 보기로 돌아가면 `<AppBarButton>` 요소의 XAML(Extensible Application Markup Language) 코드가 다음과 같이 표시됩니다.

` <AppBarButton Icon="Back" Click="AppBarButton_Click"/>`

BlankPage.xaml.cs 파일로 돌아가서 사용자가 단추를 탭하면 이전 페이지로 돌아가도록 다음 코드를 추가합니다.

```csharp
private void AppBarButton_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.    
    Frame.GoBack();
}
```

마지막으로 MainPage.xaml.cs 파일을 열고 다음 코드를 추가합니다. 이제 사용자가 단추를 탭하면 BlankPage가 열립니다.

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.
    Frame.Navigate(typeof(BlankPage1));
}
```

이제 프로그램을 실행합니다. "Go To Page" 단추를 탭하여 다른 페이지로 이동한 다음 뒤로 화살표 단추를 탭하여 이전 페이지로 돌아갑니다.

페이지 탐색은 [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) 클래스에 의해 관리됩니다. 로 **UINavigationController** 에서 iOS에서 사용 하는 클래스 **pushViewController** 및 **popViewController** 메서드를 **프레임** 클래스 UWP 앱 제공 [ **Navigate** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate) 하 고 [ **GoBack** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goback) 메서드. 또한 **Frame** 클래스에는 [**GoForward**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goforward)라는 메서드가 있으며, 예상할 수 있는 작업을 수행합니다.

이 연습에서는 페이지를 탐색할 때마다 BlankPage의 새 인스턴스를 만듭니다. 이전 인스턴스는 자동으로 삭제되거나 *해제*됩니다. 매번 새 인스턴스가 만들어지지 않도록 하려면 BlankPage.xaml.cs 파일에서 BlankPage 클래스의 생성자에 다음 코드를 추가합니다. 그러면 [**NavigationCacheMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.navigationcachemode) 동작을 사용하도록 설정됩니다.

```csharp
public BlankPage()
{
    this.InitializeComponent();
    // Add the following line of code.
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```

**Frame** 클래스의 [**CacheSize**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.cachesize) 속성을 가져오거나 설정하여 탐색 기록에 캐시될 수 있는 페이지 수를 관리할 수도 있습니다.

탐색에 대한 자세한 내용은 [탐색](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) 및 [XAML 퍼스낼리티 애니메이션 샘플](https://go.microsoft.com/fwlink/p/?LinkID=242401)을 참조하세요.

**참고**  JavaScript 및 HTML을 사용 하 여 UWP 앱에 대 한 탐색에 대 한 정보를 참조 하세요. [빠른 시작: 단일 페이지 탐색을 사용 하 여](https://docs.microsoft.com/previous-versions/windows/apps/hh452768(v=win.10))입니다.
 
### <a name="next-step"></a>다음 단계

[시작 하기: 애니메이션](getting-started-animation.md)

