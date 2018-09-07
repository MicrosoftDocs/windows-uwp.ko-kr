---
author: QuinnRadich
Description: Learn how to use page transitions in your UWP apps.
title: UWP 앱의 페이지 전환
template: detail.hbs
ms.author: quradic
ms.date: 04/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
ms.localizationpriority: medium
ms.openlocfilehash: 0afc2c55ab0d0bdd2bee0206f986b2724d331eaf
ms.sourcegitcommit: 00d27738325d6db5b5e481911ae7fac0711b05eb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/07/2018
ms.locfileid: "3660908"
---
# <a name="page-transitions"></a>페이지 전환

페이지 전환은 앱 페이지 사이에서 사용자를 탐색하면서 페이지 사이의 관계로 피드백을 제공합니다. 페이지 전환은 사용자가 탐색 계층의 최상단에 있는지, 형제 페이지 사이를 이동하고 있는지, 혹은 페이지 계층 속으로 더욱 깊숙이 탐색하고 있는지 이해하는 데 효과적입니다.

앱 페이지 사이를 탐색할 때는 두 가지 다른 애니메이션(*페이지 새로 고침*과 *드릴*)이 제공되고, [**NavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo)의 하위 클래스로 표시됩니다.

## <a name="page-refresh"></a>페이지 새로 고침

페이지 새로 고침은 다음 콘텐츠를 표시하기 위한 위로 밀기 애니메이션과 페이드 인 애니메이션의 조합입니다. 사용자가 탭 또는 왼쪽 탐색 항목을 탐색하는 등 탐색 스택의 최상단으로 이동할 때는 페이지 새로 고침을 사용하세요.

사용자가 다시 시작하는 듯한 느낌을 받도록 하는 것이 목적입니다.

![페이지 새로 고침 애니메이션](images/page-refresh.gif)

페이지 새로 고침 애니메이션은 [**EntranceNavigationTransitionInfoClass**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo)로 표시됩니다.

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());

```

**참고**: [**프레임**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame)은 자동으로 [**NavigationThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition)을 사용하여 두 페이지 간에 탐색을 애니메이션화합니다. 기본적으로 애니메이션은 페이지 새로 고침입니다.

## <a name="drill"></a>드릴

사용자가 항목 검색 후 더 많은 정보를 표시하는 등 앱을 더욱 깊숙이 탐색할 때는 드릴을 사용하세요.

사용자가 앱 속으로 더욱 깊숙이 이동한다는 느낌을 받도록 하는 것이 목적입니다.

![드릴 애니메이션](images/drill.gif)

드릴 애니메이션은 [**DrillInNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo) 클래스로 표시됩니다.

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
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

- [두 페이지 간 이동](../basics/navigate-between-two-pages.md)
- [UWP 앱의 동작](index.md)
