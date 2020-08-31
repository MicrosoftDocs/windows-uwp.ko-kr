---
title: 탐색 시작
description: UWP (유니버설 Windows 플랫폼) 프레임 클래스를 사용 하 여 여러 뷰가 있는 Windows 10 앱에 페이지 탐색을 추가 하는 방법에 대해 알아봅니다.
ms.assetid: F4DF5C5F-C886-4483-BBDA-498C4E2C1BAF
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3d26bf6e63c61207142b8945c48f9925a7459844
ms.sourcegitcommit: 45dec3dc0f14934b8ecf1ee276070b553f48074d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89094690"
---
# <a name="getting-started-navigation"></a>시작: 탐색


## <a name="adding-navigation"></a>탐색 추가

iOS는 앱 내 탐색에 도움이 되는 **UINavigationController** 클래스를 제공 합니다. 보기를 푸시 및 팝 하 여 앱을 정의 하는 **uiviewcontrollers** 의 계층 구조를 만들 수 있습니다.

반면 여러 뷰가 포함 된 Windows 10 앱은 탐색에 대 한 웹 사이트 접근 방식을 더 많이 사용 합니다. 사용자가 컨트롤을 클릭 하면 응용 프로그램을 통해 작업을 수행 하 게 될 때 페이지에서 도약 수 있습니다. 자세한 내용은 [탐색 디자인 기본 사항](https://docs.microsoft.com/windows/uwp/layout/navigation-basics)을 참조 하세요.

Windows 10 앱에서이 탐색을 관리 하는 방법 중 하나는 [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) 클래스를 사용 하는 것입니다. 다음 연습에서는이를 수행 하는 방법을 보여 줍니다.

앞서 시작한 솔루션을 계속 진행 하 여 **mainpage** 파일을 열고 **디자인** 뷰에서 단추를 추가 합니다. 단추의 **콘텐츠** 속성을 "단추"에서 "페이지로 이동"으로 변경 합니다. 그런 다음, 다음 그림에 표시 된 것 처럼 단추의 **Click** 이벤트에 대 한 처리기를 만듭니다. 이 작업을 수행 하는 방법을 기억할 수 없는 경우 이전 섹션의 연습 (힌트: **디자인** 뷰에서 단추를 두 번 클릭)을 검토 합니다.

![visual studio에서 단추 및 단추 클릭 이벤트 추가](images/ios-to-uwp/vs-go-to-page.png)

새 페이지를 추가 해 보겠습니다. **솔루션** 보기에서 **프로젝트** 메뉴를 탭 하 고 **새 항목 추가**를 탭 합니다. 다음 그림에 표시 된 것 처럼 **빈 페이지** 를 탭 한 다음 **추가**를 누릅니다.

![visual studio에서 새 페이지 추가](images/ios-to-uwp/vs-add-new-page.png)

그런 다음 BlankPage 파일에 단추를 추가 합니다. App 단추 컨트롤을 사용 하 여 뒤로 화살표 이미지를 제공 해 보겠습니다. **XAML** 뷰에서 요소 사이에를 추가 합니다. ` <AppBarButton Icon="Back"/>` `<Grid> </Grid>`

이제 단추에 이벤트 처리기를 추가 해 보겠습니다. 다음 그림과 같이 **디자인** 뷰에서 컨트롤을 두 번 클릭 하 고 Microsoft Visual Studio 클릭 상자에 "App바 단추 \_ 클릭" 텍스트를 **Click** 추가한 다음 BlankPage.xaml.cs 파일에 해당 이벤트 처리기를 추가 하 고 표시 합니다.

![visual studio에서 뒤로 단추 및 클릭 이벤트 추가](images/ios-to-uwp/vs-add-back-button.png)

BlankPage 파일의 **xaml** 뷰로 돌아가면 `<AppBarButton>` 이제 요소의 xaml (Extensible Application Markup Language) 코드가 다음과 같이 표시 됩니다.

` <AppBarButton Icon="Back" Click="AppBarButton_Click"/>`

BlankPage.xaml.cs 파일로 돌아가이 코드를 추가 하 여 사용자가 단추를 탭 한 후 이전 페이지로 이동 합니다.

```csharp
private void AppBarButton_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.    
    Frame.GoBack();
}
```

마지막으로 MainPage.xaml.cs 파일을 열고이 코드를 추가 합니다. 사용자가 단추를 탭 하면 BlankPage 열립니다.

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.
    Frame.Navigate(typeof(BlankPage1));
}
```

이제 프로그램을 실행 합니다. "페이지로 이동" 단추를 탭 하 여 다른 페이지로 이동 하 고 뒤로 화살표 단추를 탭 하 여 이전 페이지로 돌아갑니다.

페이지 탐색은 [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) 클래스를 통해 관리 됩니다. IOS의 **UINavigationController** 클래스는 **Pushviewcontroller** 및 **POPVIEWCONTROLLER** 메서드를 사용 하므로 UWP 앱에 대 한 **Frame** 클래스는 [**Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate) 및 [**GoBack**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goback) 메서드를 제공 합니다. 또한 **Frame** 클래스에는 기대 하는 것으로 간주 되는 [**goforward**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goforward)라는 메서드가 있습니다.

이 연습에서는 탐색할 때마다 BlankPage의 새 인스턴스를 만듭니다. 이전 인스턴스는 자동으로 해제 되거나 *해제*됩니다. 매번 새 인스턴스를 만들지 않으려면 BlankPage.xaml.cs 파일의 BlankPage 클래스의 생성자에 다음 코드를 추가 합니다. 그러면 [**Navigationcachemode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.navigationcachemode) 동작을 사용할 수 있습니다.

```csharp
public BlankPage()
{
    this.InitializeComponent();
    // Add the following line of code.
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```

**프레임** 클래스의 [**CacheSize**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.cachesize) 속성을 가져오거나 설정 하 여 탐색 기록에서 캐시할 수 있는 페이지 수를 관리할 수도 있습니다.

탐색에 대 한 자세한 내용은 [탐색](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) 및 [XAML 개성 애니메이션 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20personality%20animations%20sample%20(Windows%208))을 참조 하세요.

**참고**    JavaScript 및 HTML을 사용 하 여 UWP 앱을 탐색 하는 방법에 대 한 자세한 내용은 [빠른 시작: 단일 페이지 탐색 사용](https://docs.microsoft.com/previous-versions/windows/apps/hh452768(v=win.10))을 참조 하세요.
 
### <a name="next-step"></a>다음 단계

[시작: 애니메이션](getting-started-animation.md)

