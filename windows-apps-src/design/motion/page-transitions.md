---
Description: UWP 앱에서 페이지 전환을 사용 하는 방법에 알아봅니다.
title: UWP 앱의 페이지 전환
template: detail.hbs
ms.date: 04/08/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 9b3244c24ff4fa8e3c85ee9970536b1b35d8efd5
ms.sourcegitcommit: cc0ef75f314658b14376eb60ef8e5bb4d7726e04
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65444197"
---
# <a name="page-transitions"></a>페이지 전환

페이지 전환은 앱 페이지 사이에서 사용자를 탐색하면서 페이지 사이의 관계로 피드백을 제공합니다. 페이지 전환은 사용자가 탐색 계층의 최상단에 있는지, 형제 페이지 사이를 이동하고 있는지, 혹은 페이지 계층 속으로 더욱 깊숙이 탐색하고 있는지 이해하는 데 효과적입니다.

앱 페이지 사이를 탐색할 때는 두 가지 다른 애니메이션(*페이지 새로 고침*과 *드릴*)이 제공되고, [**NavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo)의 하위 클래스로 표시됩니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>있는 경우는 <strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱을 설치 하려면 여기를 클릭 <a href="xamlcontrolsgallery:/item/PageTransition">앱을 열고 페이지 전환 중인 참조</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="page-refresh"></a>페이지 새로 고침

페이지 새로 고침은 다음 콘텐츠를 표시하기 위한 위로 밀기 애니메이션과 페이드 인 애니메이션의 조합입니다. 사용자가 탭 또는 왼쪽 탐색 항목을 탐색하는 등 탐색 스택의 최상단으로 이동할 때는 페이지 새로 고침을 사용하세요.

사용자가 다시 시작하는 듯한 느낌을 받도록 하는 것이 목적입니다.

![페이지 새로 고침 애니메이션](images/page-refresh.gif)

페이지 새로 고침 애니메이션은 [**EntranceNavigationTransitionInfoClass**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo)로 표시됩니다.

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());

```

**참고**: A [ **프레임** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame) 자동으로 사용 하 여 [ **NavigationThemeTransition** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) 두 페이지 간의 탐색에 애니메이션 효과를 합니다. 기본적으로 애니메이션은 페이지 새로 고침입니다.

## <a name="drill"></a>드릴

사용자가 항목 검색 후 더 많은 정보를 표시하는 등 앱을 더욱 깊숙이 탐색할 때는 드릴을 사용하세요.

사용자가 앱 속으로 더욱 깊숙이 이동한다는 느낌을 받도록 하는 것이 목적입니다.

![드릴 애니메이션](images/drill.gif)

드릴 애니메이션은 [**DrillInNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo) 클래스로 표시됩니다.

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
```

## <a name="horizontal-slide"></a>가로 슬라이드

가로 슬라이드를 사용 하 여 서로 옆에 있는 형제 페이지 표시 되는지 보여 줍니다. 합니다 [NavigationView](../controls-and-patterns/navigationview.md) 컨트롤 위쪽 탐색 창에 대 한이 애니메이션을 자동으로 사용 하지만 사용자 고유의 가로 탐색 경험을 작성 하는 경우 SlideNavigationTransitionInfo 사용 하 여 가로 슬라이드를 구현할 수 있습니다.

원하는 느낌 서로 옆에 있는 페이지 사이 사용자가 탐색 하는입니다. 

```csharp
// Navigate to the right, ie. from LeftPage to RightPage
myFrame.Navigate(typeof(RightPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromRight } );

// Navigate to the left, ie. from RightPage to LeftPage
myFrame.Navigate(typeof(LeftPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromLeft } );
```

## <a name="suppress"></a>숨기기

탐색 도중 애니메이션을 재생하지 않으려면 다른 **NavigationTransitionInfo** 하위 유형 대신에 [**SuppressNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo)를 사용하세요.

```csharp
// Suppress the default animation
myFrame.Navigate(typeof(Page2), null, new SuppressNavigationTransitionInfo());
```

애니메이션 숨기기는 [연결된 애니메이션](connected-animation.md) 또는 묵시적인 표시하기/숨기기 애니메이션을 사용하여 자체 전환을 빌드하는 경우에 유용합니다.

## <a name="backwards-navigation"></a>뒤로 탐색

`Frame.GoBack(NavigationTransitionInfo)`를 사용하여 뒤로 탐색할 때 특정 전환을 재생할 수 있습니다.

예를 들어, 반응형 마스터/디테일 시나리오처럼 탐색 동작을 화면 크기에 따라 동적으로 변경할 때는 이 클래스가 유용할 수 있습니다.

## <a name="related-topics"></a>관련 항목

- [두 페이지 사이 이동](../basics/navigate-between-two-pages.md)
- [UWP 앱에서 동작](index.md)
