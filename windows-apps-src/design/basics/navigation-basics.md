---
Description: UWP(유니버설 Windows 플랫폼) 앱의 탐색은 탐색 구조, 탐색 요소 및 시스템 수준 기능의 유연한 모델을 기반으로 합니다.
title: UWP 앱의 탐색 기본 사항
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 137dbfe6471ee4d42e2a34e24512bdb658e985d0
ms.sourcegitcommit: 6af7ce0e3c27f8e52922118deea1b7aad0ae026e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77463765"
---
# <a name="navigation-design-basics-for-uwp-apps"></a>UWP 앱의 탐색 디자인 기본 사항

![탐색 기본 사항 헤더](images/nav/navigation-basics-header.jpg)

앱을 페이지의 컬렉션으로 생각할 경우, ‘탐색’은 앱 내에서 페이지 사이를 이동하는 동작을 나타냅니다.  탐색은 사용자 환경의 시작 지점이고, 사용자가 관심 있는 콘텐츠와 기능을 찾는 방법입니다. 매우 중요한 것이며, 올바르게 만들기 어렵습니다.

탐색에 대해 선택할 수 있는 사항은 매우 많습니다. 다음이 가능합니다.

:::row:::
    :::column:::
        ![탐색 예제 1](images/nav/nav-1.svg)

사용자에 여러 페이지를 순서대로 진행할 것을 요구할 수 있습니다.
    :::column-end:::
    :::column:::
        ![탐색 예제 2](images/nav/nav-2.svg)

사용자가 어떤 페이지나 직접 점프할 수 있는 메뉴를 제공할 수 있습니다.
    :::column-end:::
    :::column:::
        ![탐색 예제 3](images/nav/nav-3.svg)

모든 내용을 한 페이지에 넣고 필터링 메커니즘을 제공하여 콘텐츠를 볼 수 있도록 할 수 있습니다.
    :::column-end:::
:::row-end:::

모든 앱에 맞는 하나의 탐색 디자인은 없지만, 앱에 적절한 디자인을 결정하는 데 도움이 되는 원칙과 지침이 있습니다.

## <a name="principles-of-good-navigation"></a>좋은 탐색의 원칙

좋은 탐색 디자인의 기본 원칙부터 시작하겠습니다.

- **일관성:** 사용자의 기대를 충족합니다.
- **단순함:** 필요한 것 외는 하지 않습니다.
- **명료함:** 명확한 경로와 옵션을 제공합니다.

### <a name="consistency"></a>Consistency

탐색은 사용자의 기대를 일관성 있게 충족해야 합니다. 사용자가 친숙한 [표준 컨트롤](#use-the-right-controls)을 사용하고, 아이콘과 위치 스타일링에 대한 표준 규칙을 사용하면 사용자의 탐색이 예측 가능하고 직관적이 됩니다.

![페이지 구성 요소 이미지](images/nav/page-components.svg)

> 사용자는 특정 UI 요소를 표준 위치에서 찾을 수 있을 것으로 기대합니다.

### <a name="simplicity"></a>단순함

탐색 항목이 적을수록 사용자의 의사 결정 과정이 단순해집니다. 중요한 대상에 쉽게 액세스하도록 하고 중요하지 않은 항목을 숨기면 사용자가 원하는 항목을 더 빨리 검색하는 데 도움이 됩니다.

:::row:::
    :::column:::
        ![권장 사항 예](images/nav/do.svg)

        ![navview 양호](images/nav/navview-good.svg)

친숙한 탐색 메뉴에 탐색 항목을 표시합니다.
    :::column-end:::
    :::column:::
        ![금지 사항 예시](images/nav/dont.svg)

        ![navview 불량](images/nav/navview-bad.svg)

탐색 옵션이 많으면 사용자가 당황하게 됩니다.
    :::column-end:::
:::row-end:::

### <a name="clarity"></a>명료함

명확한 경로는 사용자가 논리적으로 탐색을 할 수 있도록 해줍니다. 탐색 옵션을 명확히 만들고 페이지 간 관계를 명료하게 만들면 사용자가 헤매지 않고 탐색을 할 수 있습니다.

![권장 사항 예](images/nav/clarity-image.svg)

> 목적지를 명확히 레이블로 표시하면 사용자가 자신이 어디에 있는지 알 수 있습니다.

## <a name="general-recommendations"></a>일반 권장 사항

이제 일관성, 단순함, 명료함이라는 디자인 원칙에 따라 몇 가지 일반 권장 사항을 제안하겠습니다.

1. 사용자에 관해 생각하세요. 사용자가 앱에서 일반적으로 통과할 경로를 추적하고 각 페이지에서 사용자가 왜 머무는지 어디로 가려는지 생각하세요.

2. 탐색 계층 구조를 깊게 만들지 마세요. 탐색이 3개 수준을 넘어가면 사용자가 깊은 계층 구조에서 쉽게 빠져나오지 못할 수 있습니다.

3. “페이지 왕복”을 피합니다. 페이지 왕복은 관련된 콘텐츠가 있지만 탐색하려면 위 단계로 올라갔다가 다시 내려와야 하는 경우를 말합니다.

## <a name="use-the-right-structure"></a>적합한 구조 사용

일반적인 탐색 원칙을 알아봤습니다. 그렇다면 앱을 어떻게 구성해야 할까요? 플랫과 계층의 두 가지 일반 구조가 있습니다.

:::row:::
    :::column:::
        ![플랫 구조로 정렬된 페이지](images/nav/flat-lateral-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### <a name="flatlateral"></a>플랫/측면

플랫/측면 구조에서 페이지는 나란히 표시됩니다. 순서에 관계 없이 페이지 간을 이동할 수 있습니다.

다음과 같은 경우 플랫 구조를 사용하는 것이 좋습니다.

- 순서에 관계없이 페이지를 볼 수 있습니다.
- 페이지끼리 명확히 구분이 가며 명확한 부모/자식 관계가 없습니다.
- 그룹에 8페이지 미만이 포함되어 있습니다. <br>
(그룹에 8페이지 이상이 있는 경우 페이지가 고유한 페이지인지를 이해하거나 그룹 내에서의 현재 위치를 파악하기 어려울 수 있습니다. 이러한 것이 앱에 문제가 되지 않는다면 계속 진행하고 페이지를 피어로 만듭니다. 그러지 않은 경우 계층 구조를 사용하여 페이지를 둘 이상의 더 작은 그룹으로 구분하는 것이 좋습니다.

    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![계층으로 정렬된 페이지](images/nav/hierarchical-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### <a name="hierarchical"></a>계층형

계층 구조에서 페이지는 트리와 유사한 구조로 구성됩니다. 각 자식 페이지에는 부모 페이지가 하나만 있지만 부모 페이지에는 자식 페이지가 여러 개 있을 수 있습니다. 자식 페이지로 가려면 부모 페이지를 통해 이동합니다.

계층 구조는 많은 페이지로 확장되는 복잡한 콘텐츠 구성에 적합합니다. 단점은 탐색의 오버헤드입니다. 구조가 깊을 수록, 페이지에서 페이지로 이동하는 데 더 많은 클릭이 필요하게 됩니다.

다음과 같은 경우 계층 구조를 사용하는 것이 좋습니다.
        
- 페이지를 특정한 순서로 통과해야 합니다.
- 페이지 사이에 확실한 부모 자식 관계가 있습니다.
- 그룹에 8페이지 이상이 포함되어 있습니다.
        
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![하이브리드 구조를 사용하는 앱](images/nav/combining-structures.svg)
    :::column-end:::
    :::column span="2":::
        ### <a name="combining-structures"></a>결합 구조

굳이 하나의 구조를 선택할 필요는 없습니다. 잘 디자인된 많은 앱은 둘 다를 사용합니다. 앱은 최상위 수준 페이지를 어떤 순서로도 볼 수 있게 플랫 구조를 사용하고, 더 복잡한 관계에 있는 페이지에는 계층 구조를 사용합니다.

탐색 구조에 수준이 여러 개인 경우 피어 투 피어 탐색 요소를 현재 하위 트리 내 피어에만 연결하는 것이 좋습니다. 두 개의 수준이 있는 탐색 구조를 보여 주는 인접한 그림을 참조하세요.

- 수준 1에서 피어 투 피어 탐색 요소는 A, B, C 및 D 페이지에 대한 액세스를 제공해야 합니다.
- 수준 2에서 A2 페이지에 대한 피어 투 피어 탐색 요소는 다른 A2 페이지에만 연결되어야 하며 C 하위 트리의 수준 2 페이지에는 연결되지 않아야 합니다.
    :::column-end:::
:::row-end:::

## <a name="use-the-right-controls"></a>올바른 컨트롤 사용

페이지 구조를 결정하면, 사용자가 이 페이지를 탐색할 방법을 결정해야 합니다. UWP는 앱에 일관되고 신뢰할 수 있는 탐색 환경을 구현할 수 있도록 다양한 탐색 컨트롤을 제공합니다.

:::row:::
    :::column:::
        ![프레임 이미지](images/nav/thumbnail-frame.svg)
    :::column-end:::
    :::column span="2":::
        [**프레임**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)

몇 가지 예외를 제외하고, 여러 페이지가 있는 모든 앱은 프레임을 사용합니다. 일반적으로 앱에는 프레임과 탐색 보기 컨트롤과 같은 기본 탐색 요소가 있는 기본 페이지를 포함합니다. 사용자가 페이지를 선택하면, 프레임을 로드하여 표시합니다.
:::row-end:::

:::row:::
    :::column:::
        ![탭 및 피벗 이미지](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
    :::column span="2":::
        [**위쪽 탐색**](../controls-and-patterns/navigationview.md)

동일한 수준의 페이지에 대한 링크의 가로 목록을 표시합니다. [NavigationView](../controls-and-patterns/navigationview.md) 컨트롤은 최상위 탐색 패턴을 구현합니다.
        
다음과 같은 경우 최상위 탐색을 사용합니다.

- 화면에 모든 탐색 옵션을 표시하려는 경우
- 앱 콘텐츠에 대한 추가 공간을 원하는 경우
- 아이콘은 탐색 범주를 명확하게 설명할 수 없습니다.

:::row-end:::

:::row:::
    :::column:::
        ![탭 및 피벗 이미지](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
    :::column span="2":::
        [**탭**](../controls-and-patterns/tab-view.md)

가로 탭 세트 및 해당 콘텐츠를 표시합니다. [TabView](../controls-and-patterns/tab-view.md) 컨트롤은 여러 페이지(또는 문서)를 표시하는 데 유용하며 사용자에게 탭을 다시 정렬하고, 열고, 닫을 수 있는 기능을 제공합니다.
    
다음과 같은 경우 탭을 사용합니다.

- 사용자가 탭을 동적으로 열고, 닫고, 다시 정렬할 수 있도록 하려고 합니다.
- 한 번에 많은 수의 탭이 열려 있을 수 있습니다.
- 사용자는 Microsoft Edge와 같은 웹 브라우저와 마찬가지로 탭을 사용하는 애플리케이션의 창 간에 탭을 쉽게 이동할 수 있습니다.

:::row-end:::

:::row:::
    :::column:::
         ![탭 및 피벗 이미지](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
        :::column span="2":::
    [**Pivot**](../controls-and-patterns/pivot.md)
    
[탐색 보기](../controls-and-patterns/navigationview.md)와 비슷하지만 터치 및 약간 다른 탐색 동작을 추가로 지원합니다.
    
다음과 같은 경우 피벗을 사용합니다.
- 앱에서 범주 간에 터치로 살짝 밀기를 허용하려는 경우
- 탐색 옵션이 무한히 회전되게 하려는 경우
- 범주 간 탐색 동작에 대한 광범위한 제어가 필요하지 않은 경우

:::row-end:::

:::row:::
    :::column:::
        ![navview 이미지](images/nav/thumbnail-navview.svg)
    :::column-end:::
    :::column span="2":::
        [**왼쪽 탐색**](../controls-and-patterns/navigationview.md)

최상위 페이지에 세로 링크 목록을 표시합니다. 사용하는 경우:
        
- 페이지는 최상위 수준에 있습니다.
- 5개가 넘는 많은 탐색 항목이 있습니다.
- 사용자가 페이지 간을 자주 전환할 것으로 예상되지 않습니다.

:::row-end:::
        
:::row:::
    :::column:::
        ![마스터 세부 정보 이미지](images/nav/thumbnail-master-detail.svg)
    :::column-end:::
    :::column span="2":::
        [**마스터/세부 정보**](../controls-and-patterns/master-details.md)

항목의 목록(마스터 보기)을 표시합니다. 항목을 선택하면 세부 정보 섹션에 해당 항목 페이지가 표시됩니다. 사용하는 경우:
        
- 사용자가 자식 항목 간을 자주 전환할 것으로 예상됩니다.
- 개별 항목 또는 항목 그룹에 대해 삭제 또는 정렬과 같은 상위 수준 작업을 수행할 수 있도록 하거나 각 항목에 대한 세부 정보를 보거나 업데이트할 수 있도록 하려고 합니다.

마스터/세부 정보는 메일 받은 편지함, 연락처 목록 및 데이터 항목에 적합합니다.
:::row-end:::

:::row:::
    :::column:::
        ![하이퍼링크 및 단추 이미지](images/nav/thumbnail-hyperlinks-buttons.svg)
    :::column-end:::
    :::column span="2":::
        [**하이퍼링크**](../controls-and-patterns/hyperlinks.md)

포함된 탐색 요소는 페이지의 콘텐츠에 표시될 수 있습니다. 페이지에서 일관되어야 하는 다른 탐색 요소와 달리, 콘텐츠 포함 탐색 요소는 페이지 간에 고유합니다.
:::row-end:::

## <a name="next-add-navigation-code-to-your-app"></a>다음: 앱에 탐색 코드 추가

다음 문서인 [기본 탐색 구현](navigate-between-two-pages.md)에서는 프레임 컨트롤을 사용하여 앱의 두 페이지 간에 기본 탐색을 구현하는 데 필요한 코드를 소개합니다.
