---
author: QuinnRadich
Description: Navigation in Universal Windows Platform (UWP) apps is based on a flexible model of navigation structures, navigation elements, and system-level features.
title: UWP 앱을 위한 탐색의 기본
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.author: quradic
ms.date: 7/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b731910f53a6152554b74e946374234b827f4a86
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2018
ms.locfileid: "4148288"
---
# <a name="navigation-design-basics-for-uwp-apps"></a>UWP 앱의 탐색 디자인 기본 사항

![탐색 기본 사항 헤더](images/nav/navigation-basics-header.jpg)

앱을 페이지의 컬렉션으로 생각할 경우, *탐색*은 앱 내에서 페이지 사이를 이동하는 동작을 나타냅니다. 사용자 경험의 시작 지점이고, 사용자가 관심 있는 콘텐츠와 기능을 찾는 방법입니다. 매우 중요한 것이며, 올바르게 만들기 어렵습니다.

탐색에 대해 선택할 수 있는 사항은 매우 많습니다. 다음이 가능합니다.

:::row:::
    :::column:::
        ![탐색 예제 1](images/nav/nav-1.svg)

        Require users to go through a series of pages in order.
    :::column-end:::
    :::column:::
        ![navigation example 2](images/nav/nav-2.svg)

        Provide a menu that allows users to jump directly to any page.
    :::column-end:::
    :::column:::
        ![navigation example 3](images/nav/nav-3.svg)

        Place everything on a single page and provide filtering mechanisms for viewing content.
    :::column-end:::
:::row-end:::

모든 앱에 맞는 하나의 탐색 디자인은 없지만, 앱에 적절한 디자인을 결정하는 데 도움이 되는 원칙과 지침이 있습니다.

## <a name="principles-of-good-navigation"></a>좋은 탐색의 원칙

좋은 탐색 디자인의 기본 원칙부터 시작하겠습니다.

- **일관성**: 사용자 기대 충족
- **단순함**: 필요한 것 외는 하지 않음
- **명료함**: 명확한 경로와 옵션을 제공

### <a name="consistency"></a>일관성

탐색은 사용자 경험과 일관되어야 합니다. 사용자는 익숙한 및 아이콘에 대 한 표준 규칙을 다음 [표준 컨트롤](#use-the-right-controls) 을 사용 하 여, 위치 및 스타일 됩니다 탐색 예측 가능 하 고 직관적인 사용자를 위해로 합니다.

![페이지 구성 요소 이미지](images/nav/page-components.svg)

> 사용자는 특정 UI 요소를 표준 위치에서 찾을 수 있을 것으로 기대합니다.

### <a name="simplicity"></a>단순함

탐색 항목이 적을 수록 사용자의 결정이 단순해집니다. 중요한 목적지에 쉽게 액세스 할 수 있도록 만들고 덜 중요한 항목을 숨기면 사용자가 원하는 장소에 더 빨리 도착하는 데 도움을 줄 수 있습니다.

:::row:::
    :::column:::
        ![권장 사항 예](images/nav/do.svg)

        ![navview good](images/nav/navview-good.svg)

        Present navigation items in a familiar navigation menu.
    :::column-end:::
    :::column:::
        ![don't example](images/nav/dont.svg)

        ![navview bad](images/nav/navview-bad.svg)

        Overwhelm users with many navigation options.
    :::column-end:::
:::row-end:::

### <a name="clarity"></a>명료함

명확한 경로는 사용자가 논리적으로 탐색을 할 수 있도록 해줍니다. 탐색 옵션을 명확히 만들고 페이지 간 관계를 명료하게 만들면 사용자가 헤매지 않고 탐색을 할 수 있습니다.

![권장 사항 예](images/nav/clarity-image.svg)

> 목적지를 명확히 레이블로 표시하면 사용자가 자신이 어디에 있는지 알 수 있습니다.

## <a name="general-recommendations"></a>일반 권장 사항

이제 일관성, 단순함, 명료함이라는 디자인 원칙을 사용하여 몇 가지 일반 권장 사항을 도출해 보겠습니다.

1. 사용자에 대해 생각하세요. 앱과 각 페이지에서 일반적으로 통과할 경로를 추적하고, 사용자가 이를 통과하는 이유와 사용자가 가고 싶어하는 장소에 대해 생각하세요.

2. 깊은 탐색 계층을 방지 합니다. 탐색이 3개 수준을 넘어가면 사용자가 깊은 계층에서 빠져나오기 어려울 수 있습니다.

3. "페이지 왕복"을 피합니다. 페이지 왕복은 관련된 콘텐츠가 있지만 탐색하려면 위 단계로 올라갔다가 다시 내려와야 하는 경우를 말합니다.

## <a name="use-the-right-structure"></a>적합한 구조 사용

일반적인 탐색 원칙을 알아봤습니다. 그렇다면 앱을 어떻게 구성해야 할까요? 플랫과 계층의 두 가지 일반 구조가 있습니다.

:::row:::
    :::column:::
        ![플랫 구조로 정렬된 페이지](images/nav/flat-lateral-structure.svg)
    :::column-end:::
    ::: 열 범위 = "2":::
        ### Flat/lateral

        In a flat/lateral structure, pages exist side-by-side. You can go from one page to another in any order.

        We recommend using a flat structure when:

        - The pages can be viewed in any order.
        - The pages are clearly distinct from each other and don't have an obvious parent/child relationship.
        - There are less than 8 pages in the group. <br>
        (When there are more pages, it might be difficult for users to understand how the pages are unique or to understand their current location within the group. If you don't think that's an issue for your app, go ahead and make the pages peers. Otherwise, consider using a hierarchical structure to break the pages into two or more smaller groups.)

    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![계층으로 정렬된 페이지](images/nav/hierarchical-structure.svg)
    :::column-end:::
    ::: 열 범위 = "2":::
        ### Hierarchical

        In a hierarchical structure, pages are organized into a tree-like structure. Each child page has one parent, but a parent can have one or more child pages. To reach a child page, you travel through the parent.

        Hierarchical structures are good for organizing complex content that spans lots of pages. The downside is some navigation overhead: the deeper the structure, the more clicks it takes to get from page to page.

        We recommend a hierarchical structure when:
        
        - Pages should be traversed in a specific order.
        - There is a clear parent-child relationship between pages.
        - There are more than 7 pages in the group.
        
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![하이브리드 구조를 사용하는 앱](images/nav/combining-structures.svg)
    :::column-end:::
    ::: 열 범위 = "2":::
        ### Combining structures

        You don't have choose to one structure or the other; many well-design apps use both. An app can use flat structures for top-level pages that can be viewed in any order, and hierarchical structures for pages that have more complex relationships.

        If your navigation structure has multiple levels, we recommend that peer-to-peer navigation elements only link to the peers within their current subtree. Consider the adjacent illustration, which shows a navigation structure that has two levels:

        - At level 1, the peer-to-peer navigation element should provide access to pages A, B, C, and D.
        - At level 2, the peer-to-peer navigation elements for the A2 pages should only link to the other A2 pages. They should not link to level 2 pages in the C subtree.
    :::column-end:::
:::row-end:::

## <a name="use-the-right-controls"></a>올바른 컨트롤 사용

페이지 구조를 결정하면, 사용자가 이러한 페이지를 탐색할 방법을 결정해야 합니다. UWP는 앱에 일관되고 신뢰할 수 있는 탐색 경험을 구현할 수 있도록 다양한 탐색 컨트롤을 제공합니다.

:::row:::
    :::column:::
        ![프레임 이미지](images/nav/thumbnail-frame.svg)
    :::column-end:::
    ::: 열 범위 = "2"::: **프레임** [](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)

        With few exceptions, any app that has multiple pages uses a frame. Typically, an app has a main page that contains the frame and a primary navigation element, such as a navigation view control. When the user selects a page, the frame loads and displays it.
:::row-end:::

:::row:::
    :::column:::
        ![탭 및 피벗 이미지](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
    ::: 열 범위 = "2"::: **상단 탐색 및 탭** [](../controls-and-patterns/navigationview.md)

        Displays a horizontal list of links to pages at the same level. The [NavigationView](../controls-and-patterns/navigationview.md) control implements the top navigation and tabs patterns.
        
        Use top navigation when:

        - You want to show all navigation options on the screen.
        - You desire more space for your app's content.
        - Icons cannot clearly describe your navigation categories.
        
        Use tabs when:

        - You want to preserve navigation history and page state.
        - You expect users to switch between tabs frequently.

:::row-end:::

:::row:::
    :::column:::
        ![navview 이미지](images/nav/thumbnail-navview.svg)
    :::column-end:::
    ::: 열 범위 = "2"::: [ **왼쪽된 탐색**](../controls-and-patterns/navigationview.md)

        Displays a vertical list of links to top-level pages. Use when:
        
        - The pages exist at the top level.
        - There are many navigation items (more than 5)
        - You don't expect users to switch between pages frequently.
        
:::row-end:::

:::row:::
    :::column:::
        ![마스터 세부 정보 이미지](images/nav/thumbnail-master-detail.svg)
    :::column-end:::
    ::: 열 범위 = "2"::: [ **마스터/세부 정보**](../controls-and-patterns/master-details.md)

        Displays a list (master view) of items. Selecting an item displays its corresponding page in the details section. Use when:
        
        - You expect users to switch between child items frequently.
        - You want to enable the user to perform high-level operations, such as deleting or sorting, on individual items or groups of items, and also want to enable the user to view or update the details for each item.

        Master/details is well suited for email inboxes, contact lists, and data entry.
:::row-end:::

:::row:::
    :::column:::
        ![하이퍼링크 및 단추 이미지](images/nav/thumbnail-hyperlinks-buttons.svg)
    :::column-end:::
    ::: 열 범위 = "2"::: **하이퍼링크** [](../controls-and-patterns/hyperlinks.md)

        Embedded navigation elements can appear in a page's content. Unlike other navigation elements, which should be consistent across the pages, content-embedded navigation elements are unique from page to page.
:::row-end:::

## <a name="next-add-navigation-code-to-your-app"></a>다음: 앱에 탐색 코드 추가

다음 문서인 [기본 탐색 구현](navigate-between-two-pages.md)에서는 프레임 컨트롤을 사용하여 앱에서 기본 탐색을 구현하는 데 필요한 코드를 소개합니다.