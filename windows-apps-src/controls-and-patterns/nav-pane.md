---
author: Jwmsft
Description: "화면 공간을 절약하는 동시에 최상위 수준 탐색을 제공합니다."
title: "탐색 창에 대한 지침"
ms.assetid: 8FB52F5E-8E72-4604-9222-0B0EC6A97541
label: Nav pane
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: a5f15129c424c92ac537116458c8433f6c96fa87
ms.lasthandoff: 02/07/2017

---
# <a name="nav-panes"></a>탐색 창

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

탐색 창은 실제 화면 공간을 절약함과 동시에 많은 수의 최상위 수준 탐색 항목을 허용하는 패턴입니다. 탐색 창은 모바일 앱에서 널리 사용되지만, 더욱 큰 화면에서도 잘 작동합니다. 창이 오버레이로 사용될 경우 사용자가 단추를 누를 때까지 축소된 상태를 유지하고 표시되지 않으므로 더 작은 화면에 편리합니다. 창이 고정 모드로 사용될 경우 열린 상태를 유지하므로 실제 화면 공간이 충분한 경우 더욱 실용적입니다.

![탐색 창의 예](images/navHero.png)

<div class="important-apis" >
<b>중요 API</b><br/>
<ul>
<li>[**SplitView 클래스**](https://msdn.microsoft.com/library/windows/apps/dn864360)</li>
<li> </li>
<li> </li>
<li> </li>
<li> </li>
<li> </li>
</ul>
</div>


## <a name="is-this-the-right-pattern"></a>올바른 패턴인가요?

탐색 창은 다음에 대해 제대로 작동합니다.

-   유형이 비슷한 최상위 수준 탐색 항목이 여러 개인 앱. 예를 들면 미식축구, 야구, 농구, 축구 등과 같은 범주가 있는 스포츠 앱입니다.
-   앱 전체에서 일관된 탐색 환경 제공. 탐색 창에는 작업이 아닌 탐색 요소만 포함해야 합니다.
-   최상위 수준 탐색 범주의 중간에서 높은 쪽(5-10+)
-   화면 공간 유지(오버레이로)
-   덜 자주 액세스하는 탐색 항목 (오버레이로)

## <a name="building-a-nav-pane"></a>탐색 창 만들기

탐색 창 패턴은 탐색 범주 창, 콘텐츠 영역 및 창을 열고 닫는 선택적 단추로 구성됩니다. 탐색 창을 만드는 가장 쉬운 방법은 [분할 보기 컨트롤](split-view.md)을 사용하는 것입니다. 이 컨트롤은 빈 창과 항상 표시되는 콘텐츠 영역이 포함되어 제공됩니다.

이 패턴을 구현하는 코드를 사용해 보려면 GitHub에서 [XAML 탐색 솔루션](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlNavigation)을 다운로드하세요.

<div class="microsoft-internal-note">
탐색 창과 햄버거 단추에 대한 검토는 [UNI](http://uni/DesignDepot.FrontEnd/#/Search?c=t&t=Windows%2BRS1%2BControls&f=NavPane_Hamburger)에서 수행할 수 있습니다.
</div>

### <a name="pane"></a>창

탐색 범주의 헤더는 창에 있습니다. 해당하는 경우 앱 설정 및 계정 관리에 대한 진입점도 창에 있습니다. 탐색 헤더는 주로 사용자가 선택하는 항목 목록입니다.

![탐색 창의 예](images/nav_pane_expanded.png)

### <a name="content-area"></a>콘텐츠 영역

콘텐츠 영역은 선택한 탐색 위치에 대한 정보가 표시되는 곳입니다. 개별 요소 또는 다른 하위 수준 탐색을 포함할 수 있습니다.

### <a name="button"></a>Button

있을 경우 사용자는 단추를 사용하여 창을 열고 닫을 수 있습니다. 단추는 고정된 위치에서 표시되며 창과 함께 이동하지 않습니다. 이 단추는 앱의 왼쪽 위 모서리에 배치하는 것이 좋습니다. 탐색 창 단추는 세 개의 누적된 가로줄 형태로 시각화되며 일반적으로 "햄버거" 단추라고 합니다.

![탐색 창 단추의 예](images/nav_button.png)

이 단추는 일반적으로 텍스트 문자열과 연결되어 있습니다. 앱의 최상위 수준에서 단추 옆에 앱 제목이 표시될 수 있습니다. 앱의 하위 수준에서는 텍스트 문자열이 사용자가 현재 있는 페이지의 제목일 수도 있습니다.

## <a name="nav-pane-variations"></a>탐색 창 변형

탐색 창에는 오버레이, 컴팩트, 인라인 등 세 가지 모드가 있습니다. 오버레이는 필요에 따라 축소 및 확장됩니다. 컴팩트 모드인 경우 창은 항상 확장 가능한 좁은 조각으로 나타납니다. 인라인 창은 기본적으로 열린 상태를 유지합니다.

### <a name="overlay"></a>오버레이

-   오버레이는 모든 화면 크기에서 그리고 세로 또는 가로 방향으로 사용할 수 있습니다. 오버레이는 기본(축소된) 상태에서 공간을 차지하지 않으며 단추만 표시됩니다.
-   화면 공간을 절약하는 요청 시 탐색을 제공합니다. 휴대폰 및 패블릿에서 실행하는 앱에 적합합니다.
-   창은 기본적으로 숨겨져 있으며 단추 하나만 표시됩니다.
-   탐색 창 단추를 누르면 오버레이를 열고 닫을 수 있습니다.
-   확장된 상태는 일시적이며, 선택이 수행되거나 뒤로 단추가 사용되거나 사용자가 창 외부를 탭할 경우 해제됩니다.
-   오버레이는 콘텐츠 위로 이동하며 콘텐츠를 재배치하지 않습니다.

### <a name="compact"></a>컴팩트

-   컴팩트 모드는 열 때 콘텐츠를 오버레이하는 `CompactOverlay` 또는 콘텐츠를 보이지 않게 하는 `CompactInline`으로 지정할 수 있습니다. CompactOverlay를 사용하는 것이 좋습니다.
-   컴팩트 창은 화면 공간을 작게 사용하는 동안 선택한 위치를 일부 보여 줍니다.
-   이 모드는 태블릿과 같은 중간 화면에 더욱 적합합니다.
-   창은 기본적으로 탐색 아이콘과 버튼이 표시된 상태로 닫힙니다.
-   탐색 창 단추를 누르면 창이 열리고 닫힙니다. 즉 지정된 디스플레이 모드에 따라 오버레이 또는 인라인처럼 동작합니다.
-   목록 아이콘에서 선택 영역은 사용자가 탐색 트리 내에 있는 위치를 강조 표시하도록 표시되어야 합니다.

### <a name="inline"></a>인라인

-   탐색 창이 열린 상태를 유지합니다. 이 모드는 큰 화면에 더욱 적합합니다.
-   창 안팎으로의 끌어서 놓기 시나리오를 지원합니다.
-   이 상태에는 탐색 창 단추가 필요하지 않습니다. 단추를 사용하면 콘텐츠 영역이 밀려 나가고 해당 영역 내 콘텐츠가 재배치됩니다.
-   목록 항목에서 선택 영역은 사용자가 탐색 트리 내에 있는 위치를 강조 표시하도록 표시되어야 합니다.

## <a name="adaptability"></a>적응성

다양한 장치에서 사용성을 최대화하려면 [중단점](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)을 활용하고 앱 창 너비에 따라 탐색 창 모드를 조정하는 것이 좋습니다.
-   작은 창
   -   640px보다 작거나 같습니다.
   -   탐색 창은 오버레이 모드여야 하며 기본적으로 닫혀 있습니다.
-   중간 창
   -   640px보다 크고 1007px보다 작거나 같습니다.
   -   탐색 창은 조각 모드여야 하며 기본적으로 닫혀 있습니다.
-   큰 창
   -   1007px보다 큽니다.
   -   탐색 창은 고정 모드여야 하며 기본적으로 열려 있습니다.

## <a name="tailoring"></a>조정

앱의 [10ft 경험](http://go.microsoft.com/fwlink/?LinkId=760736)을 최적화하려면 탐색 요소의 모양을 변경하여 탐색 창을 조정하는 것이 좋습니다. 조작 컨텍스트에 따라 사용자가 선택한 탐색 항목이나 중요 탐색 항목에 집중하도록 유도하는 것이 더 중요할 수 있습니다. 게임 패드가 가장 일반적인 입력 장치인 10ft 환경의 경우 특히 사용자가 화면에서 현재 중요한 항목의 위치를 쉽게 추적할 수 있게 하는 것이 중요합니다.

![조정된 탐색 창 항목의 예](images/nav_item_states.png)

## <a name="related-topics"></a>관련 항목

* [분할 보기 컨트롤](split-view.md)
* [마스터/세부](master-details.md)
* [탐색 기본 사항](https://msdn.microsoft.com/library/windows/apps/dn958438)
 

 

