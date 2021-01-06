---
title: 페이지 전환
description: UWP (유니버설 Windows 플랫폼) 페이지 전환을 사용 하 여 사용자에 게 앱의 페이지 간 관계에 대 한 피드백을 제공 하는 방법을 알아봅니다.
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b676cf274121ae991066f908b18f1be705d1c580
ms.sourcegitcommit: 77903451ae8ab1ba854494488f79184e674707df
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/06/2021
ms.locfileid: "97911013"
---
# <a name="page-transitions"></a>페이지 전환

페이지 전환은 앱의 페이지 간 사용자를 탐색 하 여 페이지 간 관계로 피드백을 제공 합니다. 페이지 전환은 사용자가 탐색 계층 구조의 맨 위에 있는지, 형제 페이지 간에 이동 하는지, 아니면 페이지 계층 구조를 심층적으로 탐색 하는지 이해 하는 데 도움이 됩니다.

앱에서 페이지를 탐색 하 고, *페이지를 새로 고치고* , *드릴* 하 고, [**NavigationTransitionInfo**](/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo)의 서브 클래스로 표시 되는 두 가지 애니메이션이 제공 됩니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치 되어 있는 경우 여기를 클릭 하 여 <a href="xamlcontrolsgallery:/item/PageTransition">앱을 열고 동작의 페이지 전환을 참조</a>하세요.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="page-refresh"></a>페이지 새로 고침

페이지 새로 고침은 들어오는 콘텐츠에 대 한 애니메이션의 페이드와 슬라이드 위로 애니메이션의 조합입니다. 탭 또는 왼쪽 탐색 항목 사이에서 이동 하는 것과 같이 사용자가 탐색 스택의 맨 위에 있는 경우 페이지 새로 고침을 사용 합니다.

사용자가 시작 하는 것은 원하는 느낌입니다.

![페이지 새로 고침 애니메이션](images/page-refresh.gif)

페이지 새로 고침 애니메이션은 [**EntranceNavigationTransitionInfoClass**](/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo)로 표시 됩니다.

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());

```

**참고**: [**프레임**](/uwp/api/windows.ui.xaml.controls.frame) 은 자동으로 [**NavigationThemeTransition**](/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) 를 사용 하 여 두 페이지 간의 탐색에 애니메이션 효과를 적용 합니다. 기본적으로 애니메이션은 페이지 새로 고침입니다.

## <a name="drill"></a>Drill

사용자가 항목을 선택한 후 추가 정보를 표시 하는 것과 같이 사용자가 더 심층적으로 탐색할 때 드릴을 사용 합니다.

원하는 느낌은 사용자가 앱을 더 심층적으로 파악 하는 것입니다.

![드릴 애니메이션](images/drill.gif)

드릴 애니메이션은 [**DrillInNavigationTransitionInfo**](/uwp/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo) 클래스로 표현 됩니다.

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
```

## <a name="horizontal-slide"></a>가로 슬라이드

가로 슬라이드를 사용 하 여 형제 페이지가 서로 옆에 표시 되도록 합니다. [Navigationview](../controls-and-patterns/navigationview.md) 컨트롤은 위쪽 탐색에 대해이 애니메이션을 자동으로 사용 하지만 사용자 고유의 가로 탐색 환경을 빌드하는 경우 SlideNavigationTransitionInfo를 사용 하 여 가로 슬라이드를 구현할 수 있습니다.

원하는 느낌은 사용자가 서로 옆에 있는 페이지 간을 탐색 하는 것입니다. 

```csharp
// Navigate to the right, ie. from LeftPage to RightPage
myFrame.Navigate(typeof(RightPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromRight } );

// Navigate to the left, ie. from RightPage to LeftPage
myFrame.Navigate(typeof(LeftPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromLeft } );
```

## <a name="suppress"></a>표시 안 함

탐색 하는 동안 애니메이션 재생을 방지 하려면 다른 **NavigationTransitionInfo** 하위 형식 대신 [**SuppressNavigationTransitionInfo**](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) 를 사용 합니다.

```csharp
// Suppress the default animation
myFrame.Navigate(typeof(Page2), null, new SuppressNavigationTransitionInfo());
```

애니메이션을 표시 하지 않으면 [연결 된 애니메이션이](connected-animation.md) 나 암시적 표시/숨기기 애니메이션을 사용 하 여 자체 전환을 빌드하는 경우에 유용 합니다.

## <a name="backwards-navigation"></a>뒤로 탐색

`Frame.GoBack(NavigationTransitionInfo)`를 사용 하 여 역방향으로 탐색할 때 특정 전환을 재생할 수 있습니다.

이는 화면 크기에 따라 탐색 동작을 동적으로 수정할 경우에 유용 합니다. 예를 들어 반응 형 마스터/세부 시나리오에서입니다.

## <a name="related-topics"></a>관련 항목

- [두 페이지 간 이동](../basics/navigate-between-two-pages.md)
- [UWP 앱에서의 동작](index.md)
