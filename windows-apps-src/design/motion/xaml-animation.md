---
ms.assetid: 0C8DEE75-FB7B-4E59-81E3-55F8D65CD982
title: 애니메이션 개요
description: Windows 런타임 애니메이션 라이브러리의 애니메이션을 사용 하 여 Windows 모양과 느낌을 앱에 통합할 수 있습니다.
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 40752da17591c9eca16f46fbd244d4507a38b1fb
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220006"
---
# <a name="animations-in-xaml"></a>XAML의 애니메이션

애니메이션은 이동 및 대화형 작업을 추가 하 여 앱을 향상 시킬 수 있습니다. Windows 런타임 애니메이션 라이브러리의 애니메이션을 사용 하 여 Windows 모양과 느낌을 앱에 통합할 수 있습니다. 이 항목에서는 각각을 사용 하는 일반적인 시나리오의 애니메이션과 예제에 대해 간략하게 설명 합니다.

> [!TIP]
> XAML에 대 한 Windows 런타임 컨트롤은 애니메이션 라이브러리에서 제공 되는 기본 제공 동작으로 특정 형식의 애니메이션을 포함 합니다. 앱에서 이러한 컨트롤을 사용 하 여 직접 프로그래밍 하지 않고도 애니메이션 모양의 모양과 느낌을 얻을 수 있습니다.

Windows 런타임 애니메이션 라이브러리의 애니메이션은 다음과 같은 이점을 제공 합니다.

-   [애니메이션에 대 한 지침](./index.md) 에 맞는 동작
-   사용자에 게 알려 주지만 방해 하지 않는 UI 상태 간의 빠른 유체 전환
-   앱 내에서 사용자로의 전환을 나타내는 시각적 동작

예를 들어 사용자가 목록에 항목을 추가 하는 경우 새 항목이 목록에 즉시 표시 되는 대신 새 항목이 현재 위치로 표시 됩니다. 목록의 다른 항목은 짧은 시간 동안 새 위치에 애니메이션을 적용 하 여 추가 된 항목을 위한 공간을 만듭니다. 여기서 전환 동작은 사용자에 대 한 컨트롤 상호 작용을 보다 명확 하 게 합니다.

Windows 10, 버전 1607에서는 탐색 하는 동안 보기 사이에 애니메이션 효과를 주는 요소가 표시 되는 애니메이션을 구현 하기 위한 새로운 [**ConnectedAnimationService**](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice) API를 도입 했습니다. 이 API는 다른 애니메이션 라이브러리 API의 사용 패턴과 다릅니다. **ConnectedAnimationService** 사용에 대해서는 [참조 페이지](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)에서 설명 합니다.

애니메이션 라이브러리는 가능한 모든 시나리오에 대 한 애니메이션을 제공 하지 않습니다. XAML에서 사용자 지정 애니메이션을 만들려는 경우가 있습니다. 자세한 내용은 [Storyboarded 애니메이션](storyboarded-animations.md)(영문)을 참조 하세요.

또한 ScrollViewer의 스크롤 위치에 따라 항목에 애니메이션을 적용 하는 것과 같은 특정 고급 시나리오의 경우 개발자는 비주얼 계층 상호 운용을 사용 하 여 사용자 지정 애니메이션을 구현할 수 있습니다. 자세한 내용은 [시각적 계층](../../composition/visual-layer.md) 을 참조 하세요.

## <a name="types-of-animations"></a>애니메이션 형식

Windows 런타임 애니메이션 시스템 및 애니메이션 라이브러리는 컨트롤과 UI의 다른 부분에 애니메이션 효과를 주는 데 더 큰 목표를 제공 합니다. 애니메이션에는 여러 가지 다른 형식이 있습니다.

-   미리 정의 된 Windows 런타임 XAML UI 형식의 컨트롤이 나 요소를 포함 하 여 UI의 특정 조건이 변경 되 면 *테마 전환이* 자동으로 적용 됩니다. 애니메이션은 Windows 모양과 느낌을 지원 하 고 한 상호 작용 모드에서 다른 상호 작용 모드로 변경 될 때 특정 UI 시나리오에 대해 모든 앱이 수행 하는 작업을 정의 하므로 *테마 전환* 이라고 합니다. 테마 전환은 애니메이션 라이브러리의 일부입니다.
-   *테마 애니메이션* 은 미리 정의 된 WINDOWS 런타임 XAML UI 형식의 속성 하나 이상에 대 한 애니메이션입니다. 테마 애니메이션은 특정 요소 하나를 대상으로 하 고, 컨트롤 내의 특정 시각적 상태에 존재 하는 반면, 테마 전환은 시각적 상태 외부에 존재 하는 컨트롤의 속성에 할당 되 고 해당 상태 간의 전환에 영향을 주므로 테마 전환과는 다릅니다. 대부분의 Windows 런타임 XAML 컨트롤에는 시각적 상태에 의해 트리거되는 애니메이션을 포함 하는 컨트롤 템플릿의 일부인 storyboard 내 테마 애니메이션이 포함 되어 있습니다. 템플릿을 수정 하지 않는 한 UI의 컨트롤에 사용할 수 있는 기본 제공 테마 애니메이션을 사용할 수 있습니다. 그러나 템플릿을 바꾸는 경우 기본 제공 컨트롤 테마 애니메이션도 제거 됩니다. 이를 되돌리려면 컨트롤의 시각적 상태 집합 내에서 테마 애니메이션을 포함 하는 스토리 보드를 정의 해야 합니다. 시각적 상태에 있지 않은 storyboard에서 테마 애니메이션을 실행 하 여 [**Begin**](/uwp/api/windows.ui.xaml.media.animation.storyboard.begin) 메서드로 시작할 수도 있지만 일반적이 지 않습니다. 테마 애니메이션은 애니메이션 라이브러리의 일부입니다.
-   *시각적 전환은* 컨트롤이 정의 된 시각적 상태 중 하나에서 다른 상태로 전환 될 때 적용 됩니다. 이러한 사용자 지정 애니메이션은 사용자가 작성 하는 사용자 지정 애니메이션 이며 일반적으로 컨트롤에 대해 작성 하는 사용자 지정 템플릿과 해당 템플릿 내의 시각적 상태 정의와 관련이 있습니다. 애니메이션은 상태 간 시간 동안만 실행 되며 일반적으로 짧은 시간 (가장 짧은 시간)입니다. 자세한 내용은 [Storyboarded 애니메이션 (시각적 상태)의 "Visualtransition" 섹션](/previous-versions/windows/apps/jj819808(v=win.10))을 참조 하세요.
-   *Storyboarded 애니메이션* 은 시간이 지남에 따라 Windows 런타임 종속성 속성의 값에 애니메이션 효과를 적용 합니다. 스토리 보드를 시각적 전환의 일부로 정의 하거나 응용 프로그램에서 런타임에 트리거할 수 있습니다. 자세한 내용은 [Storyboarded 애니메이션](storyboarded-animations.md)(영문)을 참조 하세요. 종속성 속성 및 존재 하는 위치에 대 한 자세한 내용은 [종속성 속성 개요](../../xaml-platform/dependency-properties-overview.md)를 참조 하세요.
-   새 [**ConnectedAnimationService**](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice) API에서 제공 하는 *연결 된 애니메이션* 을 통해 개발자는 탐색 하는 동안 요소가 보기 사이에 애니메이션 효과를 주는 효과를 쉽게 만들 수 있습니다. 이 API는 Windows 10 버전 1607부터 사용할 수 있습니다. 자세한 내용은 [**ConnectedAnimationService**](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice) 를 참조 하세요.

## <a name="animations-available-in-the-library"></a>라이브러리에서 사용할 수 있는 애니메이션

애니메이션 라이브러리에서 제공 되는 애니메이션은 다음과 같습니다. 애니메이션의 이름을 클릭 하 여 기본 사용 시나리오, 정의 하는 방법 및 애니메이션 예제를 확인 합니다.

-   [페이지 전환](#page-transition): [**프레임**](/uwp/api/Windows.UI.Xaml.Controls.Frame)의 페이지 전환에 애니메이션 효과를 적용 합니다.
-   [콘텐츠 및 표시 전환](#content-transition-and-entrance-transition): 하나의 조각 또는 콘텐츠 집합을 보기에 애니메이션 효과를 적용 합니다.
-   [페이드 인/출력 및 페이드](#fade-in-out-and-crossfade)인: 일시적 요소나 컨트롤을 표시 하거나 콘텐츠 영역을 새로 고칩니다.
-   [Pointer up/down](#pointer-up-down): 타일을 탭 하거나 클릭 하 여 시각적 피드백을 제공 합니다.
-   [위치 변경](#reposition): 요소를 새 위치로 이동 합니다.
-   [표시/숨기기 popup](#show-hide-popup): 뷰 위에 상황별 UI를 표시 합니다.
-   [EDGE Ui 표시/숨기기](#show-hide-edge-ui): 패널과 같은 크게 ui를 포함 하는 슬라이드 가장자리 기반 ui를 표시 하거나 숨깁니다.
-   [목록 항목 변경](#list-item-changes): 목록에서 항목을 추가 또는 삭제 하거나 항목의 순서를 다시 정렬 합니다.
-   [끌어서 놓기](#drag-drop): 끌어서 놓기 작업을 수행 하는 동안 시각적 피드백을 제공 합니다.

### <a name="page-transition"></a>페이지 전환

페이지 전환을 사용 하 여 앱 내에서 탐색에 애니메이션 효과를 주는 경우 거의 모든 앱에서 일종의 탐색을 사용 하므로 페이지 전환 애니메이션은 앱에서 사용 하는 가장 일반적인 유형의 테마 애니메이션입니다. 페이지 전환 Api에 대 한 자세한 내용은 [**NavigationThemeTransition**](/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) 를 참조 하세요.



### <a name="content-transition-and-entrance-transition"></a>콘텐츠 전환 및 입구 전환

내용 전환 애니메이션 ([**ContentThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.ContentThemeTransition))을 사용 하 여 조각 또는 콘텐츠 집합을 현재 뷰에서 이동 하거나 외부로 이동 합니다. 예를 들어 콘텐츠 전환 애니메이션은 페이지가 처음 로드 될 때 또는 페이지의 섹션에서 콘텐츠가 변경 될 때 표시할 준비가 되지 않은 콘텐츠를 표시 합니다.

[**EntranceThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) 는 페이지 또는 많은 UI 섹션이 처음 로드 될 때 콘텐츠에 적용할 수 있는 동작을 나타냅니다. 따라서 콘텐츠의 첫 번째 형태는 내용에 대 한 변경 내용에 비해 다른 피드백을 제공할 수 있습니다. [**EntranceThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) 는 기본 매개 변수를 사용 하는 [**NavigationThemeTransition**](/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) 와 동일 하지만 [**프레임**](/uwp/api/Windows.UI.Xaml.Controls.Frame)외부에서 사용 될 수 있습니다.
 
 
<span id="fade-in-out-and-crossfade"/>

### <a name="fade-inout-and-crossfade"></a>페이드 인/아웃/페이드 아웃

애니메이션 페이드 인 및 페이드 아웃을 사용 하 여 임시 UI 나 컨트롤을 표시 하거나 숨깁니다. XAML에서이는 [**FadeInThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation) 및 [**FadeOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation)로 표시 됩니다. 한 가지 예는 사용자 상호 작용으로 인해 새 컨트롤이 표시 될 수 있는 앱 바에 있습니다. 또 다른 예는 일정 시간 동안 사용자 입력이 검색 되지 않은 후 페이드 아웃 되는 임시 스크롤 막대 또는 패닝 표시기입니다. 콘텐츠를 동적으로 로드할 때 응용 프로그램은 자리 표시자 항목에서 최종 항목으로 전환할 때 애니메이션에서 페이드를 사용 해야 합니다.

항목의 상태가 변경 될 때 전환을 매끄럽게 하려면 페이드 애니메이션을 사용 합니다. 예를 들어 앱이 뷰의 현재 콘텐츠를 새로 고치는 경우입니다. XAML 애니메이션 라이브러리는 전용 페이드 애니메이션 ( [**교차**](/previous-versions/windows/apps/br212661(v=win.10))일치에 해당 하지 않음)을 제공 하지 않지만 [**FadeInThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation) 및 [**FadeOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) 를 사용 하 여 동일한 결과를 얻을 수 있으며 겹치는 타이밍을 제공 합니다.

<span id="pointer-up-down"/>

### <a name="pointer-updown"></a>위쪽/아래쪽 포인터

[**PointerUpThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation) 및 [**PointerDownThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation) 애니메이션을 사용 하 여 사용자에 게 성공적으로 탭 하거나 타일을 클릭 하는 데 대 한 피드백을 제공 합니다. 예를 들어 사용자가 타일에서 마우스를 클릭 하거나 탭 하면 포인터 다운 애니메이션이 재생 됩니다. 클릭 또는 탭이 해제 되 면 포인터 위쪽 애니메이션이 재생 됩니다.

### <a name="reposition"></a>변경할

위치 변경 애니메이션 ([**RepositionThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeAnimation) 또는 [**RepositionThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeTransition))을 사용 하 여 요소를 새 위치로 이동 합니다. 예를 들어 항목 컨트롤에서 헤더를 이동 하면 위치 변경 애니메이션이 사용 됩니다.

<span id="show-hide-popup"/>

### <a name="showhide-popup"></a>팝업 표시/숨기기

[**PopInThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation) 및 [**PopOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopOutThemeAnimation) 를 사용 하 여 현재 보기 위에 [**POPUP**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup) 또는 이와 유사한 컨텍스트 UI를 표시 하거나 숨길 수 있습니다. [**PopupThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition) 팝업을 해제 하려는 경우 유용한 피드백 인 테마 전환입니다.

<span id="show-hide-edge-ui"/>

### <a name="showhide-edge-ui"></a>가장자리 UI 표시/숨기기

[**EdgeUIThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition) 애니메이션을 사용 하 여 작은 가장자리 기반 UI를 뷰로 이동 합니다. 예를 들어 화면 위쪽 또는 아래쪽에 사용자 지정 응용 프로그램 표시줄을 표시 하거나 화면 위쪽에 오류 및 경고에 대 한 UI 화면을 표시 하는 경우 이러한 애니메이션을 사용 합니다.

[**PaneThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.PaneThemeTransition) 애니메이션을 사용 하 여 창 또는 패널을 표시 하거나 숨깁니다. 이는 사용자 지정 키보드 또는 작업창과 같은 커다란 가장자리 기반 UI에 사용 됩니다.

### <a name="list-item-changes"></a>항목 변경 내용 목록

기존 목록에서 항목을 추가 하거나 삭제할 때 애니메이션 동작을 추가 하려면 [**AddDeleteThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.AddDeleteThemeTransition) 애니메이션을 사용 합니다. 추가의 경우, 먼저 목록에서 기존 항목의 위치를 변경 하 여 새 항목의 공간을 확보 한 다음 새 항목을 추가 합니다. 삭제의 경우 전환은 목록에서 항목을 제거 하 고 필요한 경우 삭제 된 항목이 제거 된 후 나머지 목록 항목의 위치를 재배치 합니다.

항목이 목록에서 위치를 변경 하는 경우에는 별도의 [**ReorderThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.ReorderThemeTransition) 적용 됩니다. 항목을 삭제 하 고 연결 된 delete/add 애니메이션을 사용 하 여 새 장소에 추가 하는 것과 다르게 애니메이션 효과가 적용 됩니다.

이러한 애니메이션은 이러한 컨트롤을 이미 사용 하 고 있는 경우 해당 애니메이션을 수동으로 추가할 필요가 없도록 기본 [**ListView**](/uwp/api/windows.ui.xaml.controls.listview) 및 [**GridView**](/uwp/api/windows.ui.xaml.controls.gridview) 템플릿에 포함 되어 있습니다.

<span id="drag-drop"/>

### <a name="dragdrop"></a>끌어서 놓기

끌기 애니메이션 ([**DragItemThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.DragItemThemeAnimation), [**DragOverThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.DragOverThemeAnimation)) 및 drop 애니메이션 ([**DropTargetItemThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.DropTargetItemThemeAnimation))을 사용 하 여 사용자가 항목을 끌거나 놓을 때 시각적 피드백을 제공 합니다.

활성 상태인 경우 애니메이션은 삭제 된 항목을 기준으로 목록을 다시 정렬할 수 있음을 사용자에 게 보여 줍니다. 항목이 현재 위치에서 삭제 되는 경우 목록에 항목이 배치 될 위치를 사용자에 게 알려 주는 것이 유용 합니다. 애니메이션은 끌고 있는 항목을 목록에 있는 다른 두 항목 사이에 놓을 수 있음을 시각적으로 표시 하 고 해당 항목은 그 밖으로 이동 합니다.

## <a name="using-animations-with-custom-controls"></a>사용자 지정 컨트롤에 애니메이션 사용

다음 표에는 이러한 Windows 런타임 컨트롤의 사용자 지정 버전을 만들 때 사용 해야 하는 애니메이션에 대 한 권장 사항이 요약 되어 있습니다.

| UI 유형 | 권장 애니메이션 |
|---------|-----------------------|
| 대화 상자 | [**FadeInThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation) 및 [ **FadeOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) |
| 플라이아웃 | [**PopInThemeAnimation**](/uwp/api/windows.ui.xaml.media.animation.popinthemeanimation.popinthemeanimation) 및 [ **PopOutThemeAnimation**](/uwp/api/windows.ui.xaml.media.animation.popoutthemeanimation.popoutthemeanimation) |
| 도구 설명 | [**FadeInThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation) 및 [ **FadeOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) |
| 상황에 맞는 메뉴 | [**PopInThemeAnimation**](/uwp/api/windows.ui.xaml.media.animation.popinthemeanimation.popinthemeanimation) 및 [ **PopOutThemeAnimation**](/uwp/api/windows.ui.xaml.media.animation.popoutthemeanimation.popoutthemeanimation) |
| 명령 모음 | [**EdgeUIThemeTransition**](/uwp/api/windows.ui.xaml.media.animation.edgeuithemetransition.edgeuithemetransition) |
| 작업창 또는 가장자리 기반 패널 | [**PaneThemeTransition**](/uwp/api/windows.ui.xaml.media.animation.panethemetransition.panethemetransition) |
| 모든 UI 컨테이너의 내용 | [**ContentThemeTransition**](/uwp/api/windows.ui.xaml.media.animation.contentthemetransition.contentthemetransition) |
| 컨트롤의 경우 또는 다른 애니메이션이 적용 되지 않는 경우 | [**FadeInThemeAnimation**](/uwp/api/windows.ui.xaml.media.animation.fadeinthemeanimation.fadeinthemeanimation) 및 [ **FadeOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) |

 

## <a name="transition-animation-examples"></a>전환 애니메이션 예제

응용 프로그램에서 애니메이션을 사용 하 여 사용자 인터페이스를 개선 하거나 사용자에 게 성가신 작업 없이 더 매력적으로 만드는 것이 가장 좋습니다. 이 작업을 수행 하는 한 가지 방법은 UI에 애니메이션이 적용 된 전환을 적용 하는 것입니다 .이 경우에는 어떤 항목이 화면에 들어가거나 변경 되거나 변경 될 때 애니메이션이 사용자의 변경에 주의를 기울여야 합니다. 예를 들어 단추를 표시 하거나 사라지게 하는 대신 뷰에서 신속 하 게 페이드 인 및 페이드 아웃할 수 있습니다. 일관 된 권장 또는 일반적인 애니메이션 전환을 만드는 데 사용할 수 있는 많은 Api를 만들었습니다. 예제에서는 단추에 애니메이션을 적용 하 여 슬라이드를 빠르게 보기에 적용 하는 방법을 보여 줍니다.

```xml
<Button Content="Transitioning Button">
     <Button.Transitions>
         <TransitionCollection> 
             <EntranceThemeTransition/>
         </TransitionCollection>
     </Button.Transitions>
 </Button>
 ```

이 코드에서는 단추의 전환 컬렉션에 [**EntranceThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) 개체를 추가 합니다. 이제 단추가 처음 렌더링 되 면 바로 표시 되는 것이 아니라 뷰로 빠르게 슬라이드를 표시 합니다. 애니메이션 개체에 대 한 몇 가지 속성을 설정 하 여 얼마나 멀리의 슬라이드와 그 방향을 조정할 수 있지만 실제로는 특정 시나리오에 대 한 간단한 API 인 것입니다. 즉, 눈에 띄는 입구를 만들 수 있습니다.

응용 프로그램의 스타일 리소스에서 전환 애니메이션 테마를 정의 하 여 효과를 균일 하 게 적용할 수 있습니다. 이 예제는 이전 예제와 동일 하며 [**스타일**](/uwp/api/Windows.UI.Xaml.Style)을 사용 하 여 적용 됩니다.

```xml
<UserControl.Resources>
     <Style x:Key="DefaultButtonStyle" TargetType="Button">
         <Setter Property="Transitions">
             <Setter.Value>
                 <TransitionCollection>
                     <EntranceThemeTransition/>
                 </TransitionCollection>
             </Setter.Value>
        </Setter>
    </Style>
</UserControl.Resources>
      
<StackPanel x:Name="LayoutRoot">
    <Button Style="{StaticResource DefaultButtonStyle}" Content="Transitioning Button"/>
</StackPanel>
```

이전 예제에서는 개별 컨트롤에 테마 전환을 적용 하지만, 테마 전환은 개체의 컨테이너에 적용 하는 경우 훨씬 더 흥미롭습니다. 이 작업을 수행 하면 컨테이너의 모든 자식 개체가 전환 작업에 포함 됩니다. 다음 예제에서는 [**EntranceThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) 사각형의 [**모눈**](/uwp/api/Windows.UI.Xaml.Controls.Grid) 에 적용 됩니다.

```xml
<!-- If you set an EntranceThemeTransition animation on a panel, the
     children of the panel will automatically offset when they animate
     into view to create a visually appealing entrance. -->        
<ItemsControl Grid.Row="1" x:Name="rectangleItems">
    <ItemsControl.ItemContainerTransitions>
        <TransitionCollection>
            <EntranceThemeTransition/>
        </TransitionCollection>
    </ItemsControl.ItemContainerTransitions>
    <ItemsControl.ItemsPanel>
        <ItemsPanelTemplate>
            <WrapGrid Height="400"/>
        </ItemsPanelTemplate>
    </ItemsControl.ItemsPanel>
            
    <!-- The sequence children appear depends on their order in 
         the panel's children, not necessarily on where they render
         on the screen. Be sure to arrange your child elements in
         the order you want them to transition into view. -->
    <ItemsControl.Items>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
    </ItemsControl.Items>
</ItemsControl>
```

이 애니메이션을 사각형에 개별적으로 적용 한 경우와 같이 [**눈금선**](/uwp/api/Windows.UI.Xaml.Controls.Grid) 의 자식 사각형은 한 번에 모두 표시 되는 것이 아니라 시각적으로 보기 편 된 방식으로 전환 됩니다.

이 애니메이션의 데모는 다음과 같습니다.

![자식 사각형을 뷰로 전환 하는 애니메이션](./images/animation-child-rectangles.gif)

해당 자식 중 하나 이상이 위치를 변경 하는 경우에도 컨테이너의 자식 개체가 다시 흐를 수 있습니다. 다음 예제에서는 사각형의 모눈에 [**RepositionThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeTransition) 를 적용 합니다. 사각형 중 하나를 제거 하면 다른 모든 사각형은 새 위치로 다시 흐릅니다.

```xml
<Button Content="Remove Rectangle" Click="RemoveButton_Click"/>
        
<ItemsControl Grid.Row="1" x:Name="rectangleItems">
    <ItemsControl.ItemContainerTransitions>
        <TransitionCollection>
                    
            <!-- Without this, there would be no animation when items 
                 are removed. -->
            <RepositionThemeTransition/>
        </TransitionCollection>
    </ItemsControl.ItemContainerTransitions>
    <ItemsControl.ItemsPanel>
        <ItemsPanelTemplate>
            <WrapGrid Height="400"/>
        </ItemsPanelTemplate>
    </ItemsControl.ItemsPanel>
            
    <!-- All these rectangles are just to demonstrate how the items
         in the grid re-flow into position when one of the child items
         are removed. -->
    <ItemsControl.Items>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
    </ItemsControl.Items>
</ItemsControl>
```

```cs
private void RemoveButton_Click(object sender, RoutedEventArgs e)
{
    if (rectangleItems.Items.Count > 0)
    {    
        rectangleItems.Items.RemoveAt(0);
    }                         
}
```

```cpp
// .h
private:
void RemoveButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e);

//.cpp
void BlankPage::RemoveButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
{
    if (rectangleItems->Items->Size > 0)
    {    
        rectangleItems->Items->RemoveAt(0);
    }
}
```

단일 개체 또는 개체 컨테이너에 여러 개의 전환 애니메이션을 적용할 수 있습니다. 예를 들어 사각형 목록을 보기에 애니메이션 효과를 적용 하 고 위치를 변경 하는 경우에도 애니메이션 효과를 주려면 다음과 같이 [**RepositionThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeTransition) 와 [**EntranceThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) 를 적용 하면 됩니다.

```xml
...
<ItemsControl.ItemContainerTransitions>
    <TransitionCollection>
        <EntranceThemeTransition/>                    
        <RepositionThemeTransition/>
    </TransitionCollection>
</ItemsControl.ItemContainerTransitions>
...      
```

UI 요소를 추가, 제거, 다시 정렬 하는 등의 방법으로 애니메이션을 만드는 여러 가지 전환 효과가 있습니다. 이러한 Api의 이름에는 모두 "ThemeTransition"가 포함 됩니다.

| API | 설명 |
|-----|-------------|
| [**NavigationThemeTransition**](/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) | [**프레임**](/uwp/api/Windows.UI.Xaml.Controls.Frame)의 페이지 탐색에 대 한 Windows 취향에 맞는 애니메이션을 제공 합니다. |
| [**AddDeleteThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.AddDeleteThemeTransition) | 컨트롤이 자식 또는 콘텐츠를 추가 하거나 삭제 하는 경우에 애니메이션 전환 동작을 제공 합니다. 일반적으로 컨트롤은 항목 컨테이너입니다. |
| [**ContentThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.ContentThemeTransition) | 컨트롤의 내용이 변경 되는 경우에 대 한 애니메이션 전환 동작을 제공 합니다. [**AddDeleteThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.AddDeleteThemeTransition)외에도이를 적용할 수 있습니다. |
| [**EdgeUIThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition) | (소형) 가장자리 UI 전환에 대 한 애니메이션 전환 동작을 제공 합니다. |
| [**EntranceThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) | 컨트롤이 처음으로 나타날 때 애니메이션 전환 동작을 제공 합니다. |
| [**PaneThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.PaneThemeTransition) | 패널 (large edge UI) UI 전환에 대 한 애니메이션 전환 동작을 제공 합니다. |
| [**PopupThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition) | 표시 되는 컨트롤의 팝업 구성 요소 (예: 개체의 도구 설명 처럼 UI)에 적용 되는 애니메이션 전환 동작을 제공 합니다. |
| [**ReorderThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.ReorderThemeTransition) | 목록 뷰 컨트롤 항목이 순서를 변경 하는 경우에 대 한 애니메이션 전환 동작을 제공 합니다. 일반적으로이는 끌어서 놓기 작업의 결과로 발생 합니다. 서로 다른 컨트롤과 테마는 애니메이션에 대 한 다양 한 특성을 가질 수 있습니다. |
| [**RepositionThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeTransition) | 컨트롤의 위치가 변경 될 때 애니메이션 전환 동작을 제공 합니다. |

 

## <a name="theme-animation-examples"></a>테마 애니메이션 예제

전환 애니메이션은 간단 하 게 적용할 수 있습니다. 하지만 애니메이션 효과의 타이밍과 순서를 좀 더 자세히 제어할 수 있습니다. 테마 애니메이션을 사용 하 여 애니메이션이 동작 하는 방식에 일관 된 테마를 계속 사용 하면서 더 많은 제어를 사용할 수 있습니다. 또한 테마 애니메이션에는 사용자 지정 애니메이션 보다 더 작은 태그만 필요 합니다. 여기서는 [**FadeOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) 를 사용 하 여 사각형을 뷰에서 페이드 아웃 합니다.

```xml
<StackPanel>    
    <StackPanel.Resources>
        <Storyboard x:Name="myStoryboard">
            <FadeOutThemeAnimation TargetName="myRectangle" />
        </Storyboard>
    </StackPanel.Resources>
    <Rectangle PointerPressed="Rectangle_Tapped" x:Name="myRectangle"  
              Fill="Blue" Width="200" Height="300"/>
</StackPanel>
```

```cs
// When the user taps the rectangle, the animation begins.
private void Rectangle_Tapped(object sender, PointerRoutedEventArgs e)
{
    myStoryboard.Begin();
}
```

```vb
' When the user taps the rectangle, the animation begins.
Private Sub Rectangle_Tapped(sender As Object, e As PointerRoutedEventArgs)
    myStoryboard.Begin()
End Sub
```

```cpp
//.h
void Rectangle_Tapped(Platform::Object^ sender, Windows::UI::Xaml::Input::PointerRoutedEventArgs^ e);

//.cpp
void BlankPage::Rectangle_Tapped(Object^ sender, PointerRoutedEventArgs^ e)
{
    myStoryboard->Begin();
}
```

전환 애니메이션과 달리 테마 애니메이션은 자동으로 실행 되는 기본 제공 트리거 (전환)를 포함 하지 않습니다. XAML에서 정의 하는 경우 테마 애니메이션을 포함 하려면 [**Storyboard**](/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) 를 사용 해야 합니다. 애니메이션의 기본 동작을 변경할 수도 있습니다. 예를 들어 [**FadeOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation)에서 [**Duration**](/uwp/api/windows.ui.xaml.media.animation.timeline.duration) 시간 값을 늘려 페이드 아웃의 속도를 늦출 수 있습니다.

**참고**    기본 애니메이션 기술을 보여 주기 위해 응용 프로그램 코드를 사용 하 여 [**Storyboard**](/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard)의 메서드를 호출 하 여 애니메이션을 시작 합니다. [**시작**](/uwp/api/windows.ui.xaml.media.animation.storyboard.begin), [**중지**](/uwp/api/windows.ui.xaml.media.animation.storyboard.stop), [**일시 중지**](/uwp/api/windows.ui.xaml.media.animation.storyboard.pause)및 [**다시 시작**](/uwp/api/windows.ui.xaml.media.animation.storyboard.resume) **storyboard** 메서드를 사용 하 여 **스토리 보드** 애니메이션을 실행 하는 방법을 제어할 수 있습니다. 그러나 일반적으로 앱에 라이브러리 애니메이션을 포함 하는 방법은 아닙니다. 대신 일반적으로 라이브러리 애니메이션을 컨트롤 또는 요소에 적용 되는 XAML 스타일 및 템플릿에 통합 합니다. 템플릿과 시각적 상태를 이해 하는 것은 약간 더 복잡 합니다. 그러나 시각적 상태에 [대 한 Storyboarded 애니메이션](/previous-versions/windows/apps/jj819808(v=win.10)) 항목의 일부로 시각적 상태에서 라이브러리 애니메이션을 사용 하는 방법에 대해서도 설명 합니다.

 

UI 요소에 다른 여러 테마 애니메이션을 적용 하 여 애니메이션 효과를 만들 수 있습니다. 이러한 API의 이름에는 모두 "ThemeAnimation"가 포함 됩니다.

| API | 설명 |
|-----|-------------|
| [**DragItemThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.DragItemThemeAnimation) | 끌고 있는 항목 요소에 적용 되는 미리 구성 된 애니메이션을 나타냅니다. |
| [**DragOverThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.DragOverThemeAnimation) | 끌고 있는 요소 아래에 있는 요소에 적용 되는 미리 구성 된 애니메이션을 나타냅니다. |
| [**DropTargetItemThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.DropTargetItemThemeAnimation) | 잠재적 놓기 대상 요소에 적용 되는 미리 구성 된 애니메이션입니다. |
| [**FadeInThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation) | 컨트롤을 처음 표시할 때 컨트롤에 적용 되는 미리 구성 된 불투명 애니메이션입니다. |
| [**FadeOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) | UI에서 제거 되거나 숨겨진 경우 컨트롤에 적용 되는 미리 구성 된 불투명 애니메이션입니다. |
| [**PointerDownThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation) | 항목 또는 요소를 탭 하거나 클릭 하는 사용자 작업에 대 한 미리 구성 된 애니메이션입니다. |
| [**PointerUpThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation) | 사용자가 항목 또는 요소를 탭 하 고 작업을 해제 한 후 실행 되는 사용자 동작에 대 한 미리 구성 된 애니메이션입니다. |
| [**PopInThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation) | 표시 되는 컨트롤의 pop 구성 요소에 적용 되는 미리 구성 된 애니메이션입니다. 이 애니메이션은 불투명도와 변환을 결합 합니다. |
| [**PopOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopOutThemeAnimation) | 컨트롤이 닫히거나 제거 될 때 컨트롤의 pop 구성 요소에 적용 되는 미리 구성 된 애니메이션입니다. 이 애니메이션은 불투명도와 변환을 결합 합니다. |
| [**RepositionThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeAnimation) | 위치가 변경 될 때 개체에 대 한 미리 구성 된 애니메이션입니다. |
| [**SplitCloseThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.SplitCloseThemeAnimation) | [**콤보 상자**](/uwp/api/windows.ui.xaml.controls.combobox) 를 열고 닫는 스타일의 애니메이션을 사용 하 여 대상 UI를 구성 하는 미리 구성 된 애니메이션입니다. |
| [**SplitOpenThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.SplitOpenThemeAnimation) | [**콤보 상자**](/uwp/api/windows.ui.xaml.controls.combobox) 를 열고 닫는 스타일에서 애니메이션을 사용 하 여 대상 UI를 표시 하는 미리 구성 된 애니메이션입니다. |
| [**DrillInThemeAnimation**](/uwp/api/windows.ui.xaml.media.animation.drillinthemeanimation) | 마스터 페이지에서 세부 정보 페이지로 이동 하는 것과 같이 사용자가 논리적 계층 구조에서 이동할 때 실행 되는 미리 구성 된 애니메이션을 나타냅니다. |
| [**DrillOutThemeAnimation**](/uwp/api/windows.ui.xaml.media.animation.drilloutthemeanimation) | 사용자가 세부 페이지에서 마스터 페이지로 이동 하는 것과 같은 논리 계층 구조에서 뒤로 이동할 때 실행 되는 미리 구성 된 애니메이션을 나타냅니다. |

 

## <a name="create-your-own-animations"></a>직접 애니메이션 만들기

테마 애니메이션이 사용자 요구에 충분 하지 않은 경우 고유한 애니메이션을 만들 수 있습니다. 하나 이상의 속성 값에 애니메이션을 적용 하 여 개체에 애니메이션 효과를 적용 합니다. 예를 들어 사각형의 너비, [**system.windows.media.rotatetransform.angle**](/uwp/api/Windows.UI.Xaml.Media.RotateTransform)의 각도 또는 단추의 색 값에 애니메이션 효과를 적용할 수 있습니다. 이 유형의 사용자 지정 애니메이션은 Windows 런타임 이미 미리 구성 된 애니메이션 형식으로 제공 되는 라이브러리 애니메이션과 구별 하기 위해 storyboarded 애니메이션입니다. Storyboarded 애니메이션의 경우 특정 형식의 값을 변경할 수 있는 애니메이션 (예: **Double**에 애니메이션 효과를 주는 [**system.windows.media.animation.doubleanimation.to**](/uwp/api/Windows.UI.Xaml.Media.Animation.DoubleAnimation) )을 사용 하 고 해당 애니메이션을 [**스토리 보드**](/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) 내에 배치 하 여 제어 합니다.

애니메이션을 적용 하려면 애니메이션 속성은 *종속성 속성*이어야 합니다. 종속성 속성에 대 한 자세한 내용은 [종속성 속성 개요](../../xaml-platform/dependency-properties-overview.md)를 참조 하세요. 대상 지정 및 제어 하는 방법을 비롯 하 여 사용자 지정 storyboarded 애니메이션을 만드는 방법에 대 한 자세한 내용은 [storyboarded 애니메이션](storyboarded-animations.md)을 참조 하세요.

Xaml에서 사용자 지정 storyboarded 애니메이션을 정의할 수 있는 가장 큰 영역은 xaml에서 컨트롤의 시각적 상태를 정의 하는 경우입니다. 새 컨트롤 클래스를 만들거나 컨트롤 템플릿에서 시각적 상태가 있는 기존 컨트롤의 템플릿을 다시 작성 하기 때문에이 작업을 수행할 수 있습니다. 자세한 내용은 [시각적 상태에 대한 스토리보드 애니메이션](/previous-versions/windows/apps/jj819808(v=win.10))을 참조하세요.

 

 
