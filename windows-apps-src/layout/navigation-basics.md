---
author: mijacobs
Description: "UWP(유니버설 Windows 플랫폼) 앱의 탐색은 탐색 구조, 탐색 요소 및 시스템 수준 기능의 유연한 모델을 기반으로 합니다."
title: "UWP 앱을 위한 탐색의 기본"
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.openlocfilehash: a944e02ea116c0e054918397d9d46d366d43622a
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/22/2017
---
# <a name="navigation-design-basics-for-uwp-apps"></a>UWP 앱의 탐색 디자인 기본 사항

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

앱을 페이지의 컬렉션으로 생각할 경우, *탐색*이라는 용어는 앱 내에서 페이지 사이를 이동하는 행동을 나타냅니다. 이는 사용자 환경의 출발점입니다. 이는 사용자가 관심이 있는 콘텐츠와 기능을 찾는 방법입니다. 매우 중요한 것이며, 올바르게 만들기 어렵습니다. 

> **중요 API**: [Frame](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Frame), [Pivot 클래스](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Pivot), [NavigationView 클래스](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.NavigationView)

제대로 만들기 어려운 이유는, 앱 디자이너의 입장에서 선택해야 할 부분이 매우 많기 때문입니다. 책을 디자인한다면 선택할 내용은 간단합니다. 장의 순서를 정하기만 하면 됩니다. 앱의 경우 책과 비슷한 탐색 환경을 만들 수 있으며, 사용자가 일련의 페이지를 순서대로 지나갈 수 있도록 합니다. 또는 사용자가 원하는 모든 페이지에 직접 이동할 수 있는 메뉴를 제공할 수 있지만, 페이지가 너무 많다면 사용자의 선택 항목이 너무 많아집니다. 또는 모든 내용을 한 페이지에 넣고 필터링 메커니즘을 제공하여 콘텐츠를 볼 수 있도록 할 수 있습니다. 

모든 앱에 맞는 하나의 탐색 디자인은 없지만, 앱에 적절한 디자인을 알아내는 데 도움이 되는 원칙과 지침이 있습니다. 

## <a name="principles-of-good-design"></a>좋은 디자인의 원칙 
좋은 탐색 디자인의 기본이 되는 기본 원칙부터 시작하겠습니다. 

* 일관성 유지: 사용자 기대 충족.
* 간단하게 유지: 필요한 것 외에는 하지 말 것.
* 깔끔하게 유지: 탐색 기능이 사용자의 이동을 방해하지 않도록 유지.

### <a name="be-consistent"></a>일관성 유지 
탐색 기능은 사용자의 예측, 아이콘, 위치, 스타일에 대한 표준 규칙에 일관되어야 합니다. 

예를 들어 다음 그림에서 사용자가 일반적으로 기대하는 탐색 창 및 명령 표시줄의 위치를 볼 수 있습니다. 다른 디바이스 패밀리에는 탐색 요소에 대한 자체의 규칙이 있습니다. 예를 들어, 태블릿의 경우 탐색 창은 화면의 왼쪽에 표시되지만, 모바일 디바이스에서는 상단에 표시됩니다.

<figure class="wdg-figure">
  ![탐색 요소에 대한 선호 위치](images/nav/nav-component-layout.png)
  <figcaption>사용자는 특정 UI 요소를 표준 위치에서 찾을 수 있을 것으로 기대합니다.</figcaption>
</figure> 

### <a name="keep-it-simple"></a>단순하게 유지
탐색 디자인의 또 다른 중요 요인은 힉-하이먼 법칙으로, 탐색 옵션에 관련하여 자주 인용됩니다. 이 법칙은 메뉴에 더 적은 옵션을 추가하도록 장려합니다. 옵션이 더 많을 수록 사용자 상호 작용이 더 느려지며, 특히 사용자가 새 앱을 탐색할 때는 더 그렇습니다. 

<figure class="wdg-figure">
  ![단순한 메뉴와 복잡한 메뉴](images/nav/nav-simple-menus.png)
  <figcaption> 왼쪽의 경우 사용자가 선택할 수 있는 옵션이 적고, 오른쪽의 경우 옵션이 많습니다. 힉-하이먼 법칙에 따라 왼쪽의 메뉴가 사용자가 이해하고 활용하기 더 쉽습니다.
</figcaption>
</figure> 

### <a name="keep-it-clean"></a>깔끔하게 유지
탐색의 마지막 중요 특성은 깔끔한 상호 작용입니다. 이는 사용자가 다양한 컨텍스트와 상호 작용하는 물리적인 방식을 나타냅니다. 이는 개발자가 사용자의 입장에서 디자인에 대해 알 수 있는 하나의 영역입니다. 사용자와 그 동작을 이해하도록 시도하십시오. 예를 들어, 요리 앱을 디자인하는 경우 쇼핑 목록과 타이머에 쉽게 액세스할 수 있도록 고려할 수 있습니다. 

## <a name="three-general-rules"></a>세 가지 일반 규칙
이제 일관성, 단순함, 깔끔한 상호 작용의 디자인 원칙을 사용하여 몇 가지 일반 규칙을 도출해 보겠습니다. 경험에 따른 규칙으로, 이 규칙을 시작점으로 삼고 필요에 따라 조정하겠습니다. 

1. 깊은 탐색 계층을 피합니다. 사용자에게 가장 적합한 탐색 수준은 몇 개일까요? 대개 최상위 탐색과 한 단계 아래 메뉴만으로 충분합니다. 탐색 수준이 세 개 이상을 넘어가면, 간소함의 원칙을 깨는 것입니다. 더 나쁜 점은 사용자가 깊은 계층에서 빠져나오기 어려울 수 있습니다.

2. 너무 많은 탐색 옵션을 피합니다. 수준에 따라 3개에서 6개의 요소가 가장 일반적입니다. 탐색에 이보다 더 많은 요소가 필요한 경우, 특히 최상위 수준인 경우에는 앱을 여러 앱으로 분할하여 한 곳에서 너무 많은 작업을 하지 않도록 하는 것이 좋습니다. 앱에 너무 많은 탐색 요소가 있으면 대개 일관성을 저해하고 목표가 관련이 없게 됩니다.

3. "페이지 왕복"을 피합니다. 페이지 왕복은 관련된 콘텐츠가 있지만 탐색하려면 위 단계로 올라갔다가 다시 내려와야 하는 경우를 말합니다. 페이지 왕복은 분명한 목적을 달성하기 위해 불필요한 클릭이나 상호 작용을 요구하므로 깔끔한 상호 작용의 원칙에 위배됩니다. 이 경우의 목적은 관련 콘텐츠를 이어서 보는 것입니다. (이 규칙에 대한 예외는 검색과 찾아보기입니다. 페이지 왕복은 필요한 다양성과 깊이를 제공하는 유일한 방법이 될 수 있습니다.)
<figure class="wdg-figure">
  ![페이지 왕복의 예](images/nav/nav-pogo-sticking-1.png)
  <figcaption> 앱 탐색을 위한 페이지 왕복 - 사용자가 "프로젝트" 탭으로 탐색하기 위해 메인 페이지로 돌아갑니다(녹색 뒤로가기 화살표).
</figcaption>
</figure> 
<figure class="wdg-figure">
  ![살짝 밀기를 통한 측면 탐색으로 페이지 왕복 문제 해결](images/nav/nav-pogo-sticking-2.png)
  <figcaption>아이콘(녹색 살짝 밀기 제스처)으로 일부 페이지 왕복 문제를 해결할 수 있습니다.
</figcaption>
</figure> 


## <a name="use-the-right-structure"></a>적합한 구조 사용 
이제 일반 탐색 원칙과 규칙에 익숙할 것이므로, 앱의 구조를 어떻게 만들 것인가라는 가장 중요한 탐색 결정을 내려야 합니다. 플랫과 계층의 두 가지 일반 구조가 있습니다. 

### <a name="flatlateral"></a>플랫/측면
플랫/측면 구조에서 페이지는 나란히 표시됩니다. 순서에 관계 없이 페이지 간에 이동할 수 있습니다. 
<figure class="wdg-figure">
  <img src="images/nav/nav-pages-peer.png" alt="Pages arranged in a flat structure" />
<figcaption>플랫 구조로 정렬된 페이지</figcaption>
</figure> 

플랫 구조에는 여러 장점이 있습니다. 간단하고 이해하기 쉬우며, 사용자가 중간 페이지를 통하지 않고 특정 페이지로 바로 이동할 수 있습니다.  일반적으로 플랫 구조는 멋지지만, 모든 앱에서 사용할 수는 없습니다. 앱에 페이지가 아주 많다면 플랫 목록이 너무 커지게 됩니다. 페이지를 특정 순서대로 보아야 한다면, 플랫 구조가 적합하지 않습니다. 

다음과 같은 경우 플랫 구조를 사용하는 것이 좋습니다. 
<ul>
<li>순서에 관계없이 페이지를 볼 수 있습니다.</li>
<li>페이지끼리 명확히 구분이 가며 명확한 부모/자식 관계가 없습니다.</li>
<li>그룹의 페이지 수가 8개 미만입니다.<br/>
그룹의 7페이지 이상이 있는 경우 페이지가 고유한 페이지인지를 이해하거나 그룹 내에서의 현재 위치를 파악하기 어려울 수 있습니다. 이러한 것이 앱에 문제가 되지 않는다면 계속 진행하고 페이지를 피어로 만듭니다. 그러지 않은 경우 계층 구조를 사용하여 페이지를 둘 이상의 더 작은 그룹으로 구분하는 것이 좋습니다. (허브 컨트롤은 페이지를 범주로 그룹화하는 데 도움이 될 수 있습니다.)</li>
</ul>


### <a name="hierarchical"></a>계층

계층 구조에서 페이지는 트리와 유사한 구조로 구성됩니다. 각 자식 페이지에는 부모 페이지가 하나만 있지만 부모 페이지에는 자식 페이지가 여러 개 있을 수 있습니다. 자식 페이지로 가려면 부모 페이지를 통해 이동합니다.

<figure class="wdg-figure">
  <img src="images/nav/nav-pages-hiearchy.png" alt="Pages arranged in a hierarchy" />
<figcaption>계층으로 정렬된 페이지</figcaption>
</figure>

계층 구조는 콘텐츠가 많은 페이지로 구성되어 있거나 페이지를 특정 순서로 보아야 할 때 좋습니다. 계층적 페이지의 단점은 탐색의 과부하입니다. 구조가 깊을 수록, 사용자가 페이지에서 페이지로 이동하는 데 더 많은 클릭이 필요하게 됩니다. 

다음과 같은 경우 계층 구조를 사용하는 것이 좋습니다. 
<ul>
<li>사용자가 특정 순서로 페이지를 트래버스할 것으로 예상됩니다. 계층 구조를 정렬하여 해당 순서를 적용합니다.</li>
<li>그룹의 한 페이지와 다른 페이지 간에 명확한 부모-자식 관계가 있습니다.</li>
<li>그룹에 8페이지 이상이 포함되어 있습니다.<br/>
그룹에 8페이지 이상이 있는 경우 페이지가 얼마나 고유한지 이해하거나 그룹 내에서 페이지의 현재 위치를 파악하기 어려울 수 있습니다. 이러한 것이 앱에 문제가 되지 않는다면 계속 진행하고 페이지를 피어로 만듭니다. 그러지 않은 경우 계층 구조를 사용하여 페이지를 둘 이상의 더 작은 그룹으로 구분하는 것이 좋습니다. (허브 컨트롤은 페이지를 범주로 그룹화하는 데 도움이 될 수 있습니다.)</li>
</ul>

### <a name="combining-structures"></a>결합 구조
굳이 하나의 구조를 선택할 필요는 없습니다. 잘 디자인된 많은 앱은 플랫과 계층 구조 모두를 사용합니다.

![하이브리드 구조를 사용하는 앱](images/nav/nav-hybridstructure.png.png)

이러한 앱은 최상위 수준 페이지를 어떤 순서로도 볼 수 있게 플랫 구조를 사용하고, 더 복잡한 관계에 있는 페이지에는 계층 구조를 사용합니다. 

탐색 구조에 수준이 여러 개인 경우 피어 투 피어 탐색 요소를 현재 하위 트리 내 피어에만 연결하는 것이 좋습니다. 세 개의 수준이 있는 탐색 구조를 보여 주는 다음 그림을 참조하세요.

![두 개의 하위 트리가 있는 앱](images/nav/nav-subtrees.png)
-   수준 1의 경우 피어 투 피어 탐색 요소는 A, B, C 및 D 페이지에 대한 액세스를 제공해야 합니다.
-   수준 2에서 A2 페이지에 대한 피어 투 피어 탐색 요소는 다른 A2 페이지에만 연결되어야 하며 C 하위 트리의 수준 2 페이지에는 연결되지 않아야 합니다.

![두 개의 하위 트리가 있는 앱](images/nav/nav-subtrees2.png)
 

## <a name="use-the-right-controls"></a>올바른 컨트롤 사용

페이지 구조를 결정하면, 사용자가 이러한 페이지를 탐색할 방법을 결정해야 합니다. UWP는 이에 도움이 되는 다양한 탐색 컨트롤을 제공합니다. 이러한 컨트롤은 모든 UWP 앱에서 사용할 수 있으므로, 이를 통해 일관되고 안정적인 탐색 환경을 제공할 수 있습니다. 


<table>
<tr>
    <th>컨트롤</th>
    <th>설명</th>
</tr>
<tr>
    <td>[프레임](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Frame)</td>
    <td>몇 가지 예외를 제외하고, 여러 페이지가 있는 모든 앱은 프레임을 사용합니다. 일반적인 설정에서 앱에는 프레임과 탐색 보기 컨트롤과 같은 기본 탐색 요소가 있는 기본 페이지를 포함합니다. 사용자가 페이지를 선택하면, 프레임을 로드하여 표시합니다.</td>
</tr>
<tr>
    <td>[탭 및 피벗](../controls-and-patterns/tabs-pivot.md)<br/><br/>
    <img src="images/nav/nav-tabs-sm-300.png" alt="Tab-based navigation" /></td>
    <td>동일한 수준의 페이지에 대한 링크의 목록을 표시합니다.
<p>탭/피벗을 사용하는 경우:</p>
<ul>
<li><p>2-5페이지가 있습니다.</p>
<p>(5페이지를 초과하는 경우 탭/피벗을 사용할 수 있지만, 화면의 모든 탭/피벗에 맞추기는 어려울 수 있습니다.)</p></li>
<li>사용자가 페이지 사이에서 자주 전환할 것으로 예상됩니다.</li>
</ul></td>
</tr>
<tr>
    <td>[탐색 보기](../controls-and-patterns/navigationview.md)<br/><br/>
    <img src="images/nav/nav-navpane-4page-thumb.png" alt="A navigation pane" /></td>
    <td>최상위 수준 페이지에 대한 링크 목록을 표시합니다.
<p>탐색 창을 사용하는 경우:</p>
<ul>
<li>사용자가 페이지 간을 자주 전환할 것으로 예상되지 않습니다.</li>
<li>탐색 작업이 느려지더라도 공간을 절약하고 싶습니다.</li>
<li>페이지가 최상위 수준에 있습니다.</li>
</ul></td>
</tr>
<tr>
<td>[마스터/세부](../controls-and-patterns/master-details.md)<br/><br/>
<img src="images/higsecone-masterdetail-thumb.png" alt="Master/details" /></td>
<td>항목 요약 목록(마스터 보기)을 표시합니다. 항목을 선택하면 세부 정보 섹션에 해당 항목 페이지가 표시됩니다.
<p>마스터/세부 요소를 사용하는 경우:</p>
<ul>
<li>사용자가 자식 항목 간을 자주 전환할 것으로 예상됩니다.</li>
<li>개별 항목 또는 항목 그룹에 대해 삭제 또는 정렬과 같은 상위 수준 작업을 수행할 수 있도록 하거나 각 항목에 대한 세부 정보를 보거나 업데이트할 수 있도록 하려고 합니다.</li>
</ul>
<p>마스터/세부 요소는 메일 받은 편지함, 연락처 목록 및 데이터 항목에 적합합니다.</p>
</td>
</tr>
<tr>
<td s>[뒤로](navigation-history-and-backwards-navigation.md)</td>
<td style="vertical-align:top;">사용자가 앱 내에서 그리고 장치에 따라 앱 간에 탐색 기록을 트래버스할 수 있도록 하겠습니다. 자세한 내용은 [탐색 기록 및 뒤로 탐색 문서](navigation-history-and-backwards-navigation.md)를 참조하세요.</td>
</tr>
<tr class="odd">
<td>하이퍼링크 및 단추</td>
<td>콘텐츠가 포함 탐색 요소는 페이지의 콘텐츠에 나타납니다. 페이지 그룹이나 하위 트리에서 일관되어야 하는 다른 탐색 요소와 달리, 콘텐츠 포함 탐색 요소는 페이지 간에 고유합니다.</td>
</tr>
</table>

## <a name="next-add-navigation-code-to-your-app"></a>다음: 앱에 탐색 코드 추가
다음 기사인 [기본 탐색 구현](navigate-between-two-pages.md)에서는 프레임 컨트롤을 사용하여 앱에서 기본 탐색을 구현하는 데 필요한 XAML과 코드를 소개합니다. 


<!--
## History and the back button
UWP provides a back button and a system for traversing the user's navigation hsitory within an app. This system does most of the work for you, but there are a few APIs you need to call so that it works properly. For more info and instructions, see [History and backwards navigation](navigation-history-and-backwards-navigation.md). 
-->





