---
author: serenaz
Description: Navigation in Universal Windows Platform (UWP) apps is based on a flexible model of navigation structures, navigation elements, and system-level features.
title: UWP 앱을 위한 탐색의 기본
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.author: sezhen
ms.date: 11/27/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3edb7dc28561b5d316a908df951e3052eafc995b
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1654272"
---
# <a name="navigation-design-basics-for-uwp-apps"></a>UWP 앱의 탐색 디자인 기본 사항

앱을 페이지의 컬렉션으로 생각할 경우, *탐색*은 앱 내에서 페이지 사이를 이동하는 동작을 나타냅니다. 사용자 경험의 시작 지점이고, 사용자가 관심 있는 콘텐츠와 기능을 찾는 방법입니다. 매우 중요한 것이며, 올바르게 만들기 어렵습니다.

어려운 이유는, 앱 디자이너의 입장에서 선택해야 할 부분이 매우 많기 때문입니다. 사용자에 여러 페이지를 순서대로 진행할 것을 요구할 수 있습니다. 또는 사용자가 어떤 페이지나 직접 점프할 수 있는 메뉴를 제공할 수 있습니다. 또는 모든 내용을 한 페이지에 넣고 필터링 메커니즘을 제공하여 콘텐츠를 볼 수 있도록 할 수 있습니다.
 
모든 앱에 맞는 하나의 탐색 디자인은 없지만, 앱에 적절한 디자인을 결정하는 데 도움이 되는 원칙과 지침이 있습니다. 

<figure class="wdg-figure">
  <img alt="Navigation diagram for an app" src="images/navigation_diagram.png" />
  <figcaption>앱 탐색 다이어그램.</figcaption>
</figure> 

## <a name="principles-of-good-navigation"></a>좋은 탐색의 원칙
좋은 탐색 디자인의 기본 원칙부터 시작하겠습니다. 

* 일관성: 사용자 기대 충족
* 단순함: 필요한 것 외는 하지 않음.
* 명료함: 명확한 경로와 옵션을 제공.

### <a name="consistency"></a>일관성
탐색은 사용자 경험과 일관되어야 합니다. 사용자가 친숙한 [표준 컨트롤](#use-the-right-controls)을 사용하고, 아이콘과 위치 스타일링에 대한 표준 규칙을 사용하면 사용자의 탐색이 예측 가능하고 직관적이 됩니다.

<figure class="wdg-figure">
<img src="images/nav/nav-component-layout.png" alt="Preferred location for navigation elements"/>
  <figcaption>사용자는 특정 UI 요소를 표준 위치에서 찾을 수 있을 것으로 기대합니다.</figcaption>
</figure> 

### <a name="simplicity"></a>단순함
탐색 항목이 적을 수록 사용자의 결정이 단순해집니다. 중요한 목적지에 쉽게 액세스 할 수 있도록 만들고 덜 중요한 항목을 숨기면 사용자가 원하는 장소에 더 빨리 도착하는 데 도움을 줄 수 있습니다.

<figure class="wdg-figure">
<img src="images/nav/nav-simple-menus.png" alt="A simple versus a complex menu"/>
  <figcaption> 왼쪽의 메뉴를 사용자가 더 쉽게 이해할 것입니다. 항목이 더 적기 때문입니다.
</figcaption>
</figure> 

### <a name="clarity"></a>명료함
명확한 경로는 사용자가 논리적으로 탐색을 할 수 있도록 해줍니다. 탐색 옵션을 명확히 만들고 페이지 간 관계를 명료하게 만들면 사용자가 헤매지 않고 탐색을 할 수 있습니다.

<figure class="wdg-figure">
<img src="images/nav/nav-pages.png" alt="Clear paths and destinations"/>
  <figcaption> 목적지를 명확히 레이블로 표시하면 사용자가 자신이 어디에 있는지 알 수 있습니다.
</figcaption>
</figure> 

## <a name="general-recommendations"></a>일반 권장 사항
이제 일관성, 단순함, 명료함이라는 디자인 원칙을 사용하여 몇 가지 일반 권장 사항을 도출해 보겠습니다.

1. 사용자에 대해 생각하세요. 앱과 각 페이지에서 일반적으로 통과할 경로를 추적하고, 사용자가 이를 통과하는 이유와 사용자가 가고 싶어하는 장소에 대해 생각하세요. 

2. 깊은 탐색 계층을 피합니다. 탐색이 3개 수준을 넘어가면 사용자가 깊은 계층에서 빠져나오기 어려울 수 있습니다.

3. "페이지 왕복"을 피합니다. 페이지 왕복은 관련된 콘텐츠가 있지만 탐색하려면 위 단계로 올라갔다가 다시 내려와야 하는 경우를 말합니다.

## <a name="use-the-right-structure"></a>적합한 구조 사용 
일반적인 탐색 원칙을 알아봤습니다. 그렇다면 앱을 어떻게 구성해야 할까요? 플랫과 계층의 두 가지 일반 구조가 있습니다. 

### <a name="flatlateral"></a>플랫/측면
![플랫 구조로 정렬된 페이지](images/nav/nav-pages-peer.png)

플랫/측면 구조에서 페이지는 나란히 표시됩니다. 순서에 관계 없이 페이지 간에 이동할 수 있습니다. 

다음과 같은 경우 플랫 구조를 사용하는 것이 좋습니다. 
<ul>
<li>순서에 관계없이 페이지를 볼 수 있습니다.</li>
<li>페이지끼리 명확히 구분이 가며 명확한 부모/자식 관계가 없습니다.</li>
<li>그룹의 페이지 수가 8개 미만입니다.<br/>
(그룹의 7페이지 이상이 있는 경우 페이지가 고유한 페이지인지를 이해하거나 그룹 내에서의 현재 위치를 파악하기 어려울 수 있습니다. 이러한 것이 앱에 문제가 되지 않는다면 계속 진행하고 페이지를 피어로 만듭니다. 그러지 않은 경우 계층 구조를 사용하여 페이지를 둘 이상의 더 작은 그룹으로 구분하는 것이 좋습니다.)</li>
</ul>

### <a name="hierarchical"></a>계층
![계층으로 정렬된 페이지](images/nav/nav-pages-hiearchy.png)

계층 구조에서 페이지는 트리와 유사한 구조로 구성됩니다. 각 자식 페이지에는 부모 페이지가 하나만 있지만 부모 페이지에는 자식 페이지가 여러 개 있을 수 있습니다. 자식 페이지로 가려면 부모 페이지를 통해 이동합니다.

계층 구조는 많은 페이지로 확장되는 복잡한 콘텐츠 구성에 적합합니다. 단점은 탐색의 과부하입니다. 구조가 깊을 수록, 페이지에서 페이지로 이동하는 데 더 많은 클릭이 필요하게 됩니다. 

다음과 같은 경우 계층 구조를 사용하는 것이 좋습니다. 
<ul>
<li>페이지를 특정한 순서로 통과할 수 있어야 합니다.</li>
<li>페이지 사이에는 확실한 부모 자식 관계가 있습니다.</li>
<li>그룹에 8페이지 이상이 포함되어 있습니다.</li>
</ul>

### <a name="combining-structures"></a>결합 구조
![하이브리드 구조를 사용하는 앱](images/nav/nav-hybridstructure.png.png)

굳이 하나의 구조를 선택할 필요는 없습니다. 잘 디자인된 많은 앱은 둘 모두를 사용합니다. 앱은 최상위 수준 페이지를 어떤 순서로도 볼 수 있게 플랫 구조를 사용하고, 더 복잡한 관계에 있는 페이지에는 계층 구조를 사용합니다. 

탐색 구조에 수준이 여러 개인 경우 피어 투 피어 탐색 요소를 현재 하위 트리 내 피어에만 연결하는 것이 좋습니다. 세 개의 수준이 있는 탐색 구조를 보여 주는 다음 그림을 참조하세요.

![두 개의 하위 트리가 있는 앱](images/nav/nav-subtrees.png)
- 수준 1의 경우 피어 투 피어 탐색 요소는 A, B, C 및 D 페이지에 대한 액세스를 제공해야 합니다.
- 수준 2에서 A2 페이지에 대한 피어 투 피어 탐색 요소는 다른 A2 페이지에만 연결되어야 하며 C 하위 트리의 수준 2 페이지에는 연결되지 않아야 합니다.

![두 개의 하위 트리가 있는 앱](images/nav/nav-subtrees2.png)

## <a name="use-the-right-controls"></a>올바른 컨트롤 사용
페이지 구조를 결정하면, 사용자가 이러한 페이지를 탐색할 방법을 결정해야 합니다. UWP는 앱에 일관되고 신뢰할 수 있는 탐색 경험을 구현할 수 있도록 다양한 탐색 컨트롤을 제공합니다. 

앱의 탐색 요소 수에 따라 탐색 컨트롤을 선택하는 것이 좋습니다. 탐색 항목이 5개 미만이라면 [탭 및 피벗](../controls-and-patterns/tabs-pivot.md)같은 최상위 탐색을 사용합니다. 탐색 항목이 6개 이상이라면 [탐색 보기](../controls-and-patterns/navigationview.md) 또는 [마스터/세부 정보](../controls-and-patterns/master-details.md) 같은 왼쪽 탐색을 사용합니다.

<div class="mx-responsive-img">

<table>
<tr>
    <th>컨트롤</th>
    <th>설명</th>
</tr>
<tr>
    <td><a href="https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Frame">프레임</a><br/><br/>
    <img src="images/frame.png" alt="Frame" /></td>
    <td>페이지 표시 <p>몇 가지 예외를 제외하고, 여러 페이지가 있는 모든 앱은 프레임을 사용합니다. 일반적으로 앱에는 프레임과 탐색 보기 컨트롤과 같은 기본 탐색 요소가 있는 기본 페이지를 포함합니다. 사용자가 페이지를 선택하면, 프레임을 로드하여 표시합니다.</p></td>
</tr>
<tr>
    <td><a href="../controls-and-patterns/tabs-pivot.md">탭 및 피벗</a><br/><br/>
    <img src="images/nav/nav-tabs-sm-300.png" alt="Tab-based navigation" /></td>
    <td>동일한 수준의 페이지에 대한 링크의 가로 목록을 표시합니다.
<p>사용하는 경우:</p>
<ul>
<li>2-5페이지가 있습니다. (5페이지를 초과하는 경우 탭/피벗을 사용할 수 있지만, 화면의 모든 탭/피벗에 맞추기는 어려울 수 있습니다.)</li>
<li>사용자가 페이지 사이에서 자주 전환할 것으로 예상됩니다.</li>
</ul></td>
</tr>
<tr>
    <td><a href="../controls-and-patterns/navigationview.md">탐색 보기</a><br/><br/>
    <img src="images/nav/nav-navpane-4page-thumb.png" alt="A navigation pane" /></td>
    <td>최상위 수준 페이지에 대한 세로 목록을 표시합니다.
<p>사용하는 경우:</p>
<ul>
<li>페이지가 최상위 수준에 있습니다.</li>
<li>(5개 이상의)많은 탐색 항목이 있습니다.</li>
<li>사용자가 페이지 간을 자주 전환할 것으로 예상되지 않습니다.</li>

</ul></td>
</tr>
<tr>
<td><a href="../controls-and-patterns/master-details.md">마스터/세부 정보</a><br/><br/>
<img src="images/higsecone-masterdetail-thumb.png" alt="Master/details" /></td>
<td>항목의 목록(마스터 보기)을 표시합니다. 항목을 선택하면 세부 정보 섹션에 해당 항목 페이지가 표시됩니다.
<p>사용하는 경우:</p>
<ul>
<li>사용자가 자식 항목 간을 자주 전환할 것으로 예상됩니다.</li>
<li>개별 항목 또는 항목 그룹에 대해 삭제 또는 정렬과 같은 상위 수준 작업을 수행할 수 있도록 하거나 각 항목에 대한 세부 정보를 보거나 업데이트할 수 있도록 하려고 합니다.</li>
</ul>
<p>마스터/세부 요소는 전자 메일 받은 편지함, 연락처 목록 및 데이터 항목에 적합합니다.</p>
</td>
<tr>
<td><a href="../controls-and-patterns/hub.md">허브</a><br/><br/>
<img src="images/hub.png" alt="Hub" /></td>
<td> 순서로 된 항목 섹션을 그리드로 표시합니다. 
<p>사용하는 경우:</p>
<ul>
<li>히어로 이미지로 시각적 탐색을 만들기 원합니다.</li>
</ul>
<p>허브는 검색과 보기, 구매 경험에 적합합니다.</p>
</td>
</tr>
<tr>
<td><a href="../controls-and-patterns/hyperlinks.md">파이퍼링크</a> 및 <a href="../controls-and-patterns/buttons.md">단추</a></td>
<td> 포함된 탐색 요소는 페이지의 콘텐츠에 표시될 수 있습니다. 페이지에서 일관되어야 하는 다른 탐색 요소와 달리, 콘텐츠 포함 탐색 요소는 페이지 간에 고유합니다.</td>
</tr>
</table>
</div>

## <a name="next-add-navigation-code-to-your-app"></a>다음: 앱에 탐색 코드 추가
다음 기사인 [기본 탐색 구현](navigate-between-two-pages.md)에서는 프레임 컨트롤을 사용하여 앱에서 기본 탐색을 구현하는 데 필요한 코드를 소개합니다. 