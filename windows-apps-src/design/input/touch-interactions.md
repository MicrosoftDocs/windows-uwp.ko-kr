---
Description: 터치에 최적화 되어 있지만 입력 장치에서 기능적으로 일관 된 직관적인 고유의 사용자 상호 작용 환경을 사용 하 여 Windows 앱을 만듭니다.
title: 터치 조작
ms.assetid: DA6EBC88-EB18-4418-A98A-457EA1DEA88A
label: Touch interactions
template: detail.hbs
keywords: 터치, 포인터, 입력, 사용자 상호 작용
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 537d5aa08f61471c43ca8a965369bdd9dcdee7d3
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165753"
---
# <a name="touch-interactions"></a>터치 조작


터치가 사용자의 기본 입력 방법이 될 것 이란 가정 하에 앱을 디자인 합니다. Uwp 컨트롤을 사용 하는 경우 UWP 앱에서 무료로 제공 하므로 터치 패드, 마우스 및 펜/스타일러스에 대 한 지원에 추가 프로그래밍이 필요 하지 않습니다.

그러나 터치에 최적화 된 UI는 기존 UI에 대해 항상 더 우수한 것은 아닙니다. 둘 다 기술 및 응용 프로그램에 고유한 장점과 단점을 제공 합니다. 터치 우선 UI로 이동에서 터치, 터치 패드, 펜/스타일러스, 마우스 및 키보드 입력 간의 핵심 차이점을 이해 하는 것이 중요 합니다.

> **중요 한 api**: [**windows. .xaml. input**](/uwp/api/Windows.UI.Xaml.Input), windows [**.**](/uwp/api/Windows.UI.Core) [**Devices**](/uwp/api/Windows.Devices.Input)


많은 장치에는 하나 이상의 손가락 (또는 터치 접점)을 입력으로 사용 하도록 지 원하는 다중 터치 스크린이 있습니다. 터치 접점 및 해당 움직임은 다양 한 사용자 상호 작용을 지 원하는 터치 제스처 및 조작으로 해석 됩니다.

Windows 앱에는 터치 입력 처리를 위한 다양 한 메커니즘이 포함 되어 있으므로 사용자가 자신 있게 탐색할 수 있는 몰입 형 환경을 만들 수 있습니다. 여기서는 Windows 앱에서 touch 입력을 사용 하는 기본 사항을 설명 합니다.

터치 상호 작용에는 다음 세 가지 작업이 필요 합니다.

-   터치를 구분 하는 디스플레이입니다.
-   해당 디스플레이에서 하나 이상의 손가락에 대 한 직접 접촉 (또는 디스플레이에 근접 센서가 있고 가리키기 검색을 지 원하는 경우에 근접)입니다.
-   터치 접점 이동 (또는 시간 임계값에 따라 부족).

터치 센서에서 제공 되는 입력 데이터는 다음과 같을 수 있습니다.

-   하나 이상의 UI 요소 (패닝, 회전, 크기 조정 또는 이동)의 직접 조작을 위한 물리적 제스처로 해석 됩니다. 반면에 속성 창, 대화 상자 또는 기타 UI 어포던스를 통해 요소를 조작하는 것은 간접 조작으로 간주됩니다.
-   마우스 또는 펜과 같은 대체 입력 방법으로 인식 됩니다.
-   펜으로 그린 잉크 스트로크를 smudging 같은 다른 입력 메서드의 요소를 보완 하거나 수정 하는 데 사용 됩니다.

터치 입력에는 일반적으로 화면에서 요소를 직접 조작 하는 작업이 포함 됩니다. 요소는 적중 테스트 영역 내의 모든 터치 접촉에 즉시 응답 하 고 제거를 포함 하 여 모든 후속 접촉 이동에 적절 하 게 반응 합니다.

사용자 지정 터치 제스처 및 상호 작용은 신중 하 게 디자인 해야 합니다. 직관적이 고 응답성이 뛰어나고 검색 가능 해야 하며 사용자가 자신 있게 앱을 탐색할 수 있어야 합니다.

지원 되는 모든 입력 장치 유형에 서 앱 기능이 일관 되 게 노출 되는지 확인 합니다. 필요한 경우 키보드 상호 작용에 대 한 텍스트 입력 또는 마우스 및 펜에 대 한 UI affordances 같은 형식의 간접 입력 모드를 사용 합니다.

일반적인 입력 장치 (예: 마우스 및 키보드)는 많은 사용자에 게 친숙 하 고 매력적인 것을 명심 하세요. 이를 통해 속도, 정확성 및 tactile 피드백을 제공할 수 있습니다.

모든 입력 장치에 대 한 고유 하 고 고유한 상호 작용 환경을 제공 하 여 가장 광범위 한 기능 및 기본 설정을 지원 하 고 최대한 광범위 한 대상 그룹에 적합 하며 더 많은 고객을 앱에 활용할 수 있습니다.

## <a name="compare-touch-interaction-requirements"></a>터치 상호 작용 요구 사항 비교

다음 표에서는 터치 최적화 Windows 앱을 디자인할 때 고려해 야 하는 입력 장치 간의 몇 가지 차이점을 보여 줍니다.

<table>
<tbody><tr><th>요인</th><th>터치 조작</th><th>마우스, 키보드, 펜/스타일러스 상호 작용</th><th>터치 패드</th></tr>
<tr><td rowspan="3">전체 자릿수</td><td>Fingertip의 연락처 영역이 단일 x-y 좌표 보다 크므로 의도 하지 않은 명령 활성화 가능성이 높아집니다.</td><td>마우스 및 펜/스타일러스가 정확한 x-y 좌표를 제공 합니다.</td><td>마우스와 동일 합니다.</td></tr>
<tr><td>접촉 영역의 모양은 이동 전체에서 변경 됩니다.  </td><td>마우스 움직임 및 펜/스타일러스 스트로크는 정확한 x-y 좌표를 제공 합니다. 키보드 포커스는 명시적입니다.</td><td>마우스와 동일 합니다.</td></tr>
<tr><td>대상 지정을 지 원하는 마우스 커서가 없습니다.</td><td>마우스 커서, 펜/스타일러스 커서 및 키보드 포커스는 모두 대상 지정을 지원 합니다.</td><td>마우스와 동일 합니다.</td></tr>
<tr><td rowspan="3">인간 분석</td><td>하나 이상의 손가락을 사용 하는 직선 동작이 어렵기 때문에 Fingertip 이동은 정확 하지 않습니다. 이는 직접 조인트의 곡률 및 동작과 관련 된 조인트 수가 원인입니다.</td><td>마우스 또는 펜/스타일러스를 사용 하 여 직선 동작을 수행 하는 것은 화면에서 커서 보다 실제 거리가 짧아 지도록 하는 것이 더 쉽습니다.</td><td>마우스와 동일 합니다.</td></tr>
<tr><td>디스플레이 장치의 터치 표면의 일부 영역은 장치에서 손가락 및 사용자의 그립 때문에 도달 하기 어려울 수 있습니다.</td><td>키보드에서 탭 순서를 통해 컨트롤에 액세스할 수 있는 동안 마우스 및 펜/스타일러스가 화면 부분에 도달할 수 있습니다. </td><td>핑거 및 그립은 문제가 될 수 있습니다.</td></tr>
<tr><td>개체는 하나 이상의 편리 하거나 사용자의 손을 통해 가려져 있을 수 있습니다. 이를 폐색 이라고 합니다.</td><td>간접 입력 장치는 폐색를 발생 시 키 지 않습니다.</td><td>마우스와 동일 합니다.</td></tr>
<tr><td>개체 상태</td><td>터치는 두 가지 상태 모델을 사용 합니다. 디스플레이 장치의 터치 표면은 작업 (켜기) 또는 그렇지 않음 (꺼짐)입니다. 추가 시각적 피드백을 트리거할 수 있는 가리키기 상태가 없습니다.</td><td>
<p>마우스, 펜/스타일러스 및 키보드는 모두 세 가지 상태 모델을 표시 합니다. up (off), down (on) 및 가리키기 (포커스)를 제공 합니다.</p>
<p>가리키기를 통해 사용자는 UI 요소와 관련 된 도구 설명을 탐색 하 고 학습할 수 있습니다. 가리키기 및 포커스 효과는 대화형으로 사용 되는 개체를 릴레이 하 고 대상 지정에도 도움을 줍니다. 
</p>
</td><td>마우스와 동일 합니다.</td></tr>
<tr><td rowspan="2">풍부한 상호 작용</td><td>터치 표면에서 다중 터치: 여러 입력 요소를 편리 하 게 지원 합니다.</td><td>단일 입력 지점을 지원 합니다.</td><td>터치와 동일 합니다.</td></tr>
<tr><td>는 누르기, 끌기, 슬라이딩, 집기 및 회전과 같은 제스처를 통해 개체의 직접 조작을 지원 합니다.</td><td>마우스, 펜/스타일러스 및 키보드로 직접 조작 하는 것은 간접 입력 장치에 대 한 지원이 없습니다.</td><td>마우스와 동일 합니다.</td></tr>
</tbody></table>

> [!NOTE]
> 간접 입력은 25 년간의 구체화를 통해 이점을 누릴 수 있습니다. 가리키기 트리거 도구 설명 등의 기능은 터치 패드, 마우스, 펜/스타일러스 및 키보드 입력 전용 UI 탐색을 해결 하도록 설계 되었습니다. 이와 같은 UI 기능은 이러한 다른 장치에 대 한 사용자 환경을 손상 시 키 지 않고 터치식 입력을 통해 제공 되는 풍부한 환경을 위해 다시 디자인 되었습니다.

 

## <a name="use-touch-feedback"></a>터치 피드백 사용

앱과 상호 작용 하는 동안 적절 한 시각적 피드백을 통해 사용자는 앱 및 Windows 플랫폼에서 상호 작용을 해석 하는 방법을 인식 하 고 배우고 조정 하는 데 도움이 됩니다. 시각적 피드백은 성공적인 상호 작용, 릴레이 시스템 상태, 제어의 의미를 개선 하 고, 오류를 줄이고, 사용자가 시스템 및 입력 장치를 이해 하 고 상호 작용 하는 데 도움을 줄 수 있습니다.

사용자가 위치에 따라 정확성 및 전체 자릿수가 필요한 활동에 대해 터치식 입력을 사용 하는 경우 시각적 피드백은 매우 중요 합니다. 터치 입력이 검색 될 때마다 피드백을 표시 하 여 사용자가 앱 및 해당 컨트롤에 정의 된 사용자 지정 대상 규칙을 이해할 수 있도록 합니다.


## <a name="targeting"></a>대상 지정

대상 지정은 다음을 통해 최적화 됩니다.

-   터치 대상 크기

    크기 지침 지우기는 응용 프로그램이 쉽고 안전 하 게 대상으로 지정할 수 있는 개체와 컨트롤을 포함 하는 친숙 한 UI를 제공 하도록 합니다.

-   접촉 기 하 도형

    핑거의 전체 연락처 영역에 따라 가장 가능성이 높은 대상 개체가 결정 됩니다.

-   Scrubbing

    그룹 내의 항목은 손가락 사이에서 손가락을 끌어서 쉽게 대상으로 다시 지정 됩니다 (예: 라디오 단추). 손가락을 떼면 현재 항목이 활성화됩니다.

-   Rocking

    조밀 압축 된 항목 (예: 하이퍼링크)은 손가락을 누르고, 슬라이딩 하지 않고 항목을 앞뒤로 rocking 쉽게 대상으로 지정 됩니다. 폐색로 인해 현재 항목은 도구 설명 또는 상태 표시줄을 통해 식별 되 고 터치가 릴리스되면 활성화 됩니다.

## <a name="accuracy"></a>정확도

다음을 사용 하 여 찾아낼 상호 작용을 설계 합니다.

-   사용자가 콘텐츠와 상호 작용할 때 원하는 위치에서 더 쉽게 중지할 수 있는 스냅 지점입니다.
-   약간의 호로 이동 하는 경우에도 세로 또는 가로 패닝에 도움이 될 수 있는 방향성 "레일"입니다. 자세한 내용은 [패닝에 대 한 지침](guidelines-for-panning.md)을 참조 하세요.

## <a name="occlusion"></a>폐색

Finger 및 핸드 폐색은 다음을 통해 방지할 수 있습니다.

-   UI 크기 조정 및 위치 지정

    Fingertip 연락처 영역에 완전히 포함 될 수 없도록 UI 요소를 충분히 크게 만듭니다.

    가능 하면 연락처 영역 위에 메뉴 및 팝업을 배치 합니다.

-   도구 설명

    사용자가 개체에 대 한 핑거 연락처를 유지 관리 하는 경우 도구 설명을 표시 합니다. 이는 개체 기능을 설명 하는 데 유용 합니다. 사용자는 도구 설명이 호출 되지 않도록 개체에서 fingertip를 끌 수 있습니다.

    작은 개체의 경우 fingertip 연락처 영역에 포함 되지 않도록 도구 설명을 오프셋 합니다. 이는 대상을 지정 하는 데 유용 합니다.

-   전체 자릿수에 대 한 핸들

    정밀도가 필요한 경우 (예: 텍스트 선택) 정확도를 높이기 위해 오프셋 되는 선택 핸들을 제공 합니다. 자세한 내용은 [텍스트 및 이미지를 선택 하기 위한 지침 (Windows 런타임 앱)](guidelines-for-textselection.md)을 참조 하세요.

## <a name="timing"></a>타이밍

직접 조작을 위해 시간 제한 모드를 변경 하지 않습니다. 직접 조작은 개체의 실시간 실제 실시간 처리를 시뮬레이션 합니다. 이 개체는 손가락이 이동 될 때 응답 합니다.

반면에 시간 지정 된 상호 작용은 터치 상호 작용 후에 발생 합니다. 시간이 지정 되는 상호 작용은 일반적으로 시간, 거리 또는 속도와 같은 보이지 않는 임계값에 따라 수행할 명령을 결정 합니다. 시간이 지정 된 상호 작용에는 시스템에서 작업을 수행할 때까지 시각적 피드백이 없습니다.

직접 조작은 시간 지정 된 상호 작용에 비해 다양 한 이점을 제공 합니다.

-   상호 작용 중에 즉각적인 시각적 피드백을 통해 사용자는 더 많은 작업을 수행 하 고 제어할 수 있습니다.
-   직접 조작은 시스템을 쉽게 탐색할 수 있도록 합니다. 즉, 사용자가 쉽게 논리적이 고 직관적인 방식으로 작업을 통해 다시 이동할 수 있습니다.
-   개체에 직접적으로 영향을 주는 상호 작용은 보다 직관적이 고 검색 가능 하며 기억 하기 쉬운 상호 작용입니다. 모호 하거나 추상적인 상호 작용에 의존 하지 않습니다.
-   사용자가 임의로 및 보이지 않는 임계값에 도달 해야 하므로 시간이 지정 된 상호 작용은 수행 하기 어려울 수 있습니다.

또한 다음 사항을 적극 권장 합니다.

-   조작은 사용 된 손가락 수로 구분 되어서는 안 됩니다.
-   상호 작용은 복합 조작을 지원 해야 합니다. 예를 들어 손가락을 끌어 이동 하는 동안 확대/축소 합니다.
-   상호 작용은 시간으로 구분 되어서는 안 됩니다. 동일한 상호 작용은이를 수행 하는 데 걸리는 시간에 관계 없이 동일한 결과를 가져야 합니다. 시간 기반 활성화는 사용자에 대 한 필수 지연 및 직접 조작의 몰입 형 특성과 시스템 응답성의 인식을 모두 야기 합니다.

   > [!NOTE]
   > 이에 대 한 예외는 특정 시간 제한 상호 작용을 사용 하 여 학습 및 탐색을 지 원하는 경우입니다 (예: 누르고 있는 경우).

     

-   적절 한 설명 및 시각적 신호는 고급 상호 작용 사용에 상당한 영향을 미칠 수 있습니다.


## <a name="app-views"></a>앱 보기


앱 보기의 이동/스크롤 및 확대/축소 설정을 통해 사용자 상호 작용 환경을 조정 합니다. 앱 뷰에서는 사용자가 앱과 해당 콘텐츠를 액세스 하 고 조작 하는 방법을 설명 합니다. 뷰는 관성, 콘텐츠 경계 바운스 및 끌기 지점과 같은 동작도 제공합니다.

[**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) 컨트롤의 이동 및 스크롤 설정은 뷰의 콘텐츠가 뷰포트 내에 들어가지 않는 경우 사용자가 단일 뷰 내에서 탐색 하는 방법을 지시 합니다. 단일 보기는 예를 들어 잡지 나 책의 페이지, 컴퓨터의 폴더 구조, 문서 라이브러리 또는 사진 앨범이 될 수 있습니다.

확대/축소 설정은 [**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) 컨트롤에서 지 원하는 광학 확대/축소 및 [**의미 체계 확대**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) /축소 컨트롤 모두에 적용 됩니다. 의미 체계 확대/축소는 단일 뷰 내에서 많은 관련 데이터 또는 콘텐츠를 표시 하 고 탐색 하기 위한 터치 최적화 기술입니다. 이 기능은 두 가지 고유한 분류 모드 또는 확대/축소 수준을 사용 하 여 작동 합니다. 이는 단일 뷰 내에서 패닝 및 스크롤 하는 것과 유사 합니다. 패닝 및 스크롤은 의미 체계 확대/축소와 함께 사용할 수 있습니다.

앱 보기 및 이벤트를 사용 하 여 팬/스크롤 및 확대/축소 동작을 수정 합니다. 이는 포인터 및 제스처 이벤트를 처리 하는 것 보다 원활한 상호 작용 환경을 제공할 수 있습니다.

앱 보기에 대 한 자세한 내용은 [컨트롤, 레이아웃 및 텍스트](../basics/index.md)를 참조 하세요.

## <a name="custom-touch-interactions"></a>사용자 지정 터치 상호 작용


사용자 고유의 상호 작용 지원을 구현 하는 경우 사용자가 앱의 UI 요소와 직접 상호 작용 하는 것과 관련 된 직관적인 환경이 필요 하다는 점을 명심 해야 합니다. 일관 되 고 검색 가능한 항목을 유지 하기 위해 플랫폼 컨트롤 라이브러리에서 사용자 지정 상호 작용을 모델링 하는 것이 좋습니다. 이러한 라이브러리의 컨트롤은 표준 상호 작용, 애니메이션 된 물리학 효과, 시각적 피드백 및 내게 필요한 옵션을 비롯 한 전체 사용자 상호 작용 환경을 제공 합니다. 명확 하 고 잘 정의 된 요구 사항이 있고 기본 상호 작용이 시나리오를 지원 하지 않는 경우에만 사용자 지정 상호 작용을 만듭니다.

사용자 지정 된 터치 지원을 제공 하기 위해 다양 한 [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) 이벤트를 처리할 수 있습니다. 이러한 이벤트는 세 가지 수준의 추상화로 그룹화 됩니다.

-   정적 제스처 이벤트는 상호 작용이 완료 된 후에 트리거됩니다. 제스처 이벤트에는 [**탭**](/uwp/api/windows.ui.xaml.uielement.tapped), [**DoubleTapped**](/uwp/api/windows.ui.xaml.uielement.doubletapped), [**righttapped**](/uwp/api/windows.ui.xaml.uielement.righttapped) [**상태 및 누르고**](/uwp/api/windows.ui.xaml.uielement.holding)있습니다.

    [**Istapenabled**](/uwp/api/windows.ui.xaml.uielement.istapenabled), [**IsDoubleTapEnabled**](/uwp/api/windows.ui.xaml.uielement.isdoubletapenabled), [**IsRightTapEnabled**](/uwp/api/windows.ui.xaml.uielement.isrighttapenabled)및 [**isholdingenabled**](/uwp/api/windows.ui.xaml.uielement.isholdingenabled) 를 **false**로 설정 하 여 특정 요소에서 제스처 이벤트를 사용 하지 않도록 설정할 수 있습니다.

-   [**Pointerpressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed) 및 [**pointerpressed**](/uwp/api/windows.ui.xaml.uielement.pointermoved) 과 같은 포인터 이벤트는 포인터 동작을 포함 하 여 각 터치 연락처에 대 한 하위 수준 세부 정보를 제공 하 고, 누르기 및 릴리스 이벤트를 구분 하는 기능을 제공 합니다.

    포인터는 통합 이벤트 메커니즘을 사용 하는 제네릭 입력 형식입니다. 터치, 터치 패드, 마우스 또는 펜으로 사용할 수 있는 활성 입력 소스에서 화면 위치와 같은 기본 정보를 노출 합니다.

-   [**ManipulationStarted**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted)와 같은 조작 제스처 이벤트는 진행 중인 조작을 나타냅니다. 사용자가 요소를 터치 하 고 사용자가 손가락을 뗄 때까지 또는 조작이 취소 될 때까지 계속 해 서 실행을 시작 합니다.

    조작 이벤트에는 확대/축소, 패닝 또는 회전과 같은 다중 터치 상호 작용이 포함 되며, 끌기와 같은 관성 및 속도 데이터를 사용 하는 상호 작용이 포함 됩니다. 조작 이벤트에서 제공하는 정보는 수행된 조작 형식을 확인하지 않고 오히려 위치, 변환 델타 및 속도와 같은 데이터를 포함합니다. 이 터치 데이터를 사용 하 여 수행 해야 하는 상호 작용의 유형을 결정할 수 있습니다.

UWP에서 지 원하는 기본 터치 제스처 집합은 다음과 같습니다.

| 이름           | 유형                 | Description                                                                            |
|----------------|----------------------|----------------------------------------------------------------------------------------|
| 탭            | 정적 제스처       | 한 손가락을 화면에 터치 하 고 리프트 합니다.                                            |
| 길게 누르기 | 정적 제스처       | 한 손가락은 화면에 접촉 하 여 제자리에 유지 됩니다.                                      |
| 슬라이드          | 조작 제스처 | 하나 이상의 손가락이 화면을 터치 하 고 동일한 방향으로 이동 합니다.                   |
| 살짝 밀기          | 조작 제스처 | 하나 이상의 손가락이 화면을 터치 하 고 짧은 거리를 동일한 방향으로 이동 합니다.  |
| 않을           | 조작 제스처 | 두 개 이상의 손가락이 화면을 터치 하 고 시계 방향 또는 반시계 방향 호에서 이동 합니다. |
| 손가락 모으기          | 조작 제스처 | 두 개 이상의 손가락이 화면을 터치 하 고 서로 가깝게 이동 합니다.                         |
| Stretch        | 조작 제스처 | 두 개 이상의 손가락이 화면을 터치 하 고 멀리 떨어져 이동 합니다.                           |

 

<!-- mijacobs: Removing for now. We don't have a real page to link to yet. 
For more info about gestures, manipulations, and interactions, see [Custom user interactions](custom-user-input-portal.md).
-->

## <a name="gesture-events"></a>제스처 이벤트


개별 컨트롤에 대 한 자세한 내용은 [controls list](../controls-and-patterns/index.md)를 참조 하세요.

## <a name="pointer-events"></a>포인터 이벤트


포인터 이벤트는 터치, 터치 패드, 펜 및 마우스를 비롯 하 여 다양 한 활성 입력 소스에 의해 발생 합니다 (기존 마우스 이벤트를 대체).

포인터 이벤트는 단일 입력 지점 (손가락, 펜 팁, 마우스 커서)을 기반으로 하며 속도 기반 상호 작용을 지원 하지 않습니다.

다음은 포인터 이벤트 목록과 관련 이벤트 인수입니다.

| 이벤트 또는 클래스                                                       | Description                                                   |
|----------------------------------------------------------------------|---------------------------------------------------------------|
| [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed)             | 단일 손가락이 화면에 닿을 때 발생 합니다.               |
| [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased)           | 동일한 터치 접점이 리프트 될 때 발생 합니다.                |
| [**PointerMoved 됨**](/uwp/api/windows.ui.xaml.uielement.pointermoved)                 | 포인터를 화면 위로 끌 때 발생 합니다.         |
| [**PointerEntered 됨**](/uwp/api/windows.ui.xaml.uielement.pointerentered)             | 포인터가 요소의 적중 테스트 영역에 들어가면 발생 합니다. |
| [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited)               | 포인터가 요소의 적중 테스트 영역을 벗어나면 발생 합니다.  |
| [**PointerCanceled**](/uwp/api/windows.ui.xaml.uielement.pointercanceled)           | 터치 접점이 비정상적으로 손실 될 때 발생 합니다.               |
| [**PointerCaptureLost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost)     | 포인터 캡처가 다른 요소에 의해 수행 될 때 발생 합니다.    |
| [**PointerWheelChanged**](/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)   | 마우스 휠의 델타 값이 변경 될 때와 터치 패드가 pinched 때 발생 합니다.         |
| [**PointerRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs) | 모든 포인터 이벤트에 대 한 데이터를 제공 합니다.                         |

 

다음 예제에서는 [**Pointerpressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed), [**pointerpressed**](/uwp/api/windows.ui.xaml.uielement.pointerreleased)및 [**pointerpressed**](/uwp/api/windows.ui.xaml.uielement.pointerexited) 이벤트를 사용 하 여 [**사각형**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 개체의 탭 상호 작용을 처리 하는 방법을 보여 줍니다.

먼저 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) `touchRectangle` Extensible Application Markup Language (XAML)에서 이라는 사각형을 만듭니다.

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
           Height="100" Width="200" Fill="Blue" />
</Grid>
```
다음으로 [**Pointerpressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed), [**pointerpressed**](/uwp/api/windows.ui.xaml.uielement.pointerreleased)및 [**pointerpressed**](/uwp/api/windows.ui.xaml.uielement.pointerexited) 이벤트에 대 한 수신기가 지정 됩니다.

```cpp
MainPage::MainPage()
{
    InitializeComponent();

    // Pointer event listeners.
    touchRectangle->PointerPressed += ref new PointerEventHandler(this, &MainPage::touchRectangle_PointerPressed);
    touchRectangle->PointerReleased += ref new PointerEventHandler(this, &MainPage::touchRectangle_PointerReleased);
    touchRectangle->PointerExited += ref new PointerEventHandler(this, &MainPage::touchRectangle_PointerExited);
}
```

```cs
public MainPage()
{
    this.InitializeComponent();

    // Pointer event listeners.
    touchRectangle.PointerPressed += touchRectangle_PointerPressed;
    touchRectangle.PointerReleased += touchRectangle_PointerReleased;
    touchRectangle.PointerExited += touchRectangle_PointerExited;
}
```

```vb
Public Sub New()

    ' This call is required by the designer.
    InitializeComponent()

    ' Pointer event listeners.
    AddHandler touchRectangle.PointerPressed, AddressOf touchRectangle_PointerPressed
    AddHandler touchRectangle.PointerReleased, AddressOf Me.touchRectangle_PointerReleased
    AddHandler touchRectangle.PointerExited, AddressOf touchRectangle_PointerExited

End Sub
```

마지막으로 [**Pointerpressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed) 이벤트 처리기는 [**사각형**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)의 [**높이**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 와 [**너비**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 를 늘리고, [**Pointerpressed**](/uwp/api/windows.ui.xaml.uielement.pointerreleased) 및 [**pointerpressed**](/uwp/api/windows.ui.xaml.uielement.pointerexited) 이벤트 처리기는 **높이** 와 **너비** 를 다시 시작 값으로 설정 합니다.

```cpp
// Handler for pointer exited event.
void MainPage::touchRectangle_PointerExited(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Pointer moved outside Rectangle hit test area.
    // Reset the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 200;
        rect->Height = 100;
    }
}

// Handler for pointer released event.
void MainPage::touchRectangle_PointerReleased(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Reset the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 200;
        rect->Height = 100;
    }
}

// Handler for pointer pressed event.
void MainPage::touchRectangle_PointerPressed(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Change the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 250;
        rect->Height = 150;
    }
}
```

```cs
// Handler for pointer exited event.
private void touchRectangle_PointerExited(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Pointer moved outside Rectangle hit test area.
    // Reset the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 200;
        rect.Height = 100;
    }
}
// Handler for pointer released event.
private void touchRectangle_PointerReleased(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Reset the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 200;
        rect.Height = 100;
    }
}

// Handler for pointer pressed event.
private void touchRectangle_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Change the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 250;
        rect.Height = 150;
    }
}
```

```vb
' Handler for pointer exited event.
Private Sub touchRectangle_PointerExited(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    ' Pointer moved outside Rectangle hit test area.
    ' Reset the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 200
        rect.Height = 100
    End If
End Sub

' Handler for pointer released event.
Private Sub touchRectangle_PointerReleased(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    ' Reset the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 200
        rect.Height = 100
    End If
End Sub

' Handler for pointer pressed event.
Private Sub touchRectangle_PointerPressed(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    ' Change the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 250
        rect.Height = 150
    End If
End Sub
```

## <a name="manipulation-events"></a>조작 이벤트


응용 프로그램에서 여러 손가락 상호 작용을 지원 하거나 속도 데이터를 요구 하는 상호 작용을 지원 해야 하는 경우 조작 이벤트를 사용 합니다.

조작 이벤트를 사용 하 여 끌기, 확대/축소 및 유지와 같은 상호 작용을 검색할 수 있습니다.

> [!NOTE]
> 터치 패드는 조작 이벤트를 발생 시 키 지 않습니다. 대신, 터치 패드 입력에 대해 포인터 이벤트가 발생 합니다.

다음은 조작 이벤트 및 관련 이벤트 인수의 목록입니다.

| 이벤트 또는 클래스                                                                                               | Description                                                                                                                               |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| [**System.windows.uielement.manipulationstarting> 이벤트**](/uwp/api/windows.ui.xaml.uielement.manipulationstarting)                                   | 조작 프로세서가 처음으로 만들어지면 발생합니다.                                                                                  |
| [**System.windows.uielement.manipulationstarted> 이벤트**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted)                                     | 입력 장치가 [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement)에서 조작을 시작할 때 발생 합니다.                                            |
| [**System.windows.uielement.manipulationdelta> 이벤트**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta)                                         | 입력 디바이스에서 조작 중에 위치를 변경하면 발생합니다.                                                                      |
| [**System.windows.uielement.manipulationinertiastarting> 이벤트**](/uwp/api/windows.ui.xaml.uielement.manipulationinertiastartingevent)                | 조작 하는 동안 입력 장치가 [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) 개체와의 연결을 잃고 관성이 시작 될 때 발생 합니다. |
| [**System.windows.uielement.manipulationcompleted> 이벤트**](/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)                                 | [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) 의 조작과 관성이 완료 되 면 발생 합니다.                                          |
| [**ManipulationStartingRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationStartingRoutedEventArgs)               | [**System.windows.uielement.manipulationstarting>**](/uwp/api/windows.ui.xaml.uielement.manipulationstarting) 이벤트에 대 한 데이터를 제공 합니다.                                         |
| [**ManipulationStartedRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationStartedRoutedEventArgs)                 | [**System.windows.uielement.manipulationstarted>**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted) 이벤트에 대 한 데이터를 제공 합니다.                                           |
| [**ManipulationDeltaRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationDeltaRoutedEventArgs)                     | [**System.windows.uielement.manipulationdelta>**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) 이벤트에 대 한 데이터를 제공 합니다.                                               |
| [**ManipulationInertiaStartingRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationInertiaStartingRoutedEventArgs) | [**System.windows.uielement.manipulationinertiastarting>**](/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting) 이벤트에 대 한 데이터를 제공 합니다.                           |
| [**ManipulationVelocities**](/uwp/api/Windows.UI.Input.ManipulationVelocities)                                              | 조작이 발생 하는 속도를 설명 합니다.                                                                                         |
| [**ManipulationCompletedRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationCompletedRoutedEventArgs)             | [**System.windows.uielement.manipulationcompleted>**](/uwp/api/windows.ui.xaml.uielement.manipulationcompleted) 이벤트에 대 한 데이터를 제공 합니다.                                       |

 

제스처는 일련의 조작 이벤트로 구성 됩니다. 각 제스처는 사용자가 화면에 접촉 하는 경우와 같은 [**system.windows.uielement.manipulationstarted>**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted) 이벤트로 시작 합니다.

그런 다음 [**system.windows.uielement.manipulationdelta>**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) 이벤트가 하나 이상 발생 합니다. 예를 들어 화면을 터치 한 다음 손가락을 화면에서 끌 수 있습니다. 마지막으로, 상호 작용이 완료 되 면 [**system.windows.uielement.manipulationcompleted>**](/uwp/api/windows.ui.xaml.uielement.manipulationcompleted) 이벤트가 발생 합니다.

> [!NOTE]
> 터치 스크린 모니터가 없는 경우 마우스 및 마우스 휠 인터페이스를 사용 하 여 시뮬레이터에서 조작 이벤트 코드를 테스트할 수 있습니다.

 

다음 예제에서는 [**system.windows.uielement.manipulationdelta>**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) 이벤트를 사용 하 여 [**사각형**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 의 슬라이드 상호 작용을 처리 하 고 화면에서 이동 하는 방법을 보여 줍니다.

첫째, 이라는 [**사각형**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 은 `touchRectangle` [**높이**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 와 [**너비가**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 200 인 XAML로 생성 됩니다.

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
               Width="200" Height="200" Fill="Blue" 
               ManipulationMode="All"/>
</Grid>
```

그런 다음 사각형을 변환 하기 위해 라는 전역 [**system.windows.media.translatetransform.x**](/uwp/api/Windows.UI.Xaml.Media.TranslateTransform) 을 `dragTranslation` 만듭니다. [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) [**System.windows.uielement.manipulationdelta>**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) 이벤트 수신기는 **사각형**에 지정 되며 `dragTranslation` **사각형**의 [**rendertransform**](/uwp/api/windows.ui.xaml.uielement.rendertransform) 에 추가 됩니다.

```cpp
// Global translation transform used for changing the position of 
// the Rectangle based on input data from the touch contact.
Windows::UI::Xaml::Media::TranslateTransform^ dragTranslation;
```

```cs
// Global translation transform used for changing the position of 
// the Rectangle based on input data from the touch contact.
private TranslateTransform dragTranslation;
```

```vb
' Global translation transform used for changing the position of 
' the Rectangle based on input data from the touch contact.
Private dragTranslation As TranslateTransform
```

```cpp
MainPage::MainPage()
{
    InitializeComponent();

    // Listener for the ManipulationDelta event.
    touchRectangle->ManipulationDelta += 
        ref new ManipulationDeltaEventHandler(
            this, 
            &MainPage::touchRectangle_ManipulationDelta);
    // New translation transform populated in 
    // the ManipulationDelta handler.
    dragTranslation = ref new TranslateTransform();
    // Apply the translation to the Rectangle.
    touchRectangle->RenderTransform = dragTranslation;
}
```

```cs
public MainPage()
{
    this.InitializeComponent();

    // Listener for the ManipulationDelta event.
    touchRectangle.ManipulationDelta += touchRectangle_ManipulationDelta;
    // New translation transform populated in 
    // the ManipulationDelta handler.
    dragTranslation = new TranslateTransform();
    // Apply the translation to the Rectangle.
    touchRectangle.RenderTransform = this.dragTranslation;
}
```

```vb
Public Sub New()

    ' This call is required by the designer.
    InitializeComponent()

    ' Listener for the ManipulationDelta event.
    AddHandler touchRectangle.ManipulationDelta,
        AddressOf testRectangle_ManipulationDelta
    ' New translation transform populated in 
    ' the ManipulationDelta handler.
    dragTranslation = New TranslateTransform()
    ' Apply the translation to the Rectangle.
    touchRectangle.RenderTransform = dragTranslation

End Sub
```

마지막으로 [**system.windows.uielement.manipulationdelta>**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) 이벤트 처리기에서 [**사각형**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 의 위치는 [**델타**](/uwp/api/windows.ui.xaml.input.manipulationdeltaroutedeventargs.delta) 속성의 [**system.windows.media.translatetransform.x**](/uwp/api/Windows.UI.Xaml.Media.TranslateTransform) 를 사용 하 여 업데이트 됩니다.

```cpp
// Handler for the ManipulationDelta event.
// ManipulationDelta data is loaded into the
// translation transform and applied to the Rectangle.
void MainPage::touchRectangle_ManipulationDelta(Object^ sender,
    ManipulationDeltaRoutedEventArgs^ e)
{
    // Move the rectangle.
    dragTranslation->X += e->Delta.Translation.X;
    dragTranslation->Y += e->Delta.Translation.Y;
    
}
```

```cs
// Handler for the ManipulationDelta event.
// ManipulationDelta data is loaded into the
// translation transform and applied to the Rectangle.
void touchRectangle_ManipulationDelta(object sender,
    ManipulationDeltaRoutedEventArgs e)
{
    // Move the rectangle.
    dragTranslation.X += e.Delta.Translation.X;
    dragTranslation.Y += e.Delta.Translation.Y;
}
```

```vb
' Handler for the ManipulationDelta event.
' ManipulationDelta data Is loaded into the
' translation transform And applied to the Rectangle.
Private Sub testRectangle_ManipulationDelta(
    sender As Object,
    e As ManipulationDeltaRoutedEventArgs)

    ' Move the rectangle.
    dragTranslation.X = (dragTranslation.X + e.Delta.Translation.X)
    dragTranslation.Y = (dragTranslation.Y + e.Delta.Translation.Y)

End Sub
```

## <a name="routed-events"></a>라우트된 이벤트


여기에 언급 된 모든 포인터 이벤트, 제스처 이벤트 및 조작 이벤트는 *라우트된 이벤트*로 구현 됩니다. 즉, 이벤트를 원래 이벤트가 발생 한 개체 이외의 개체에서 처리할 수 있습니다. [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) 의 부모 컨테이너 또는 앱의 루트 [**페이지**](/uwp/api/Windows.UI.Xaml.Controls.Page) 와 같은 개체 트리의 연속 부모는 원래 요소가 아닌 경우에도 이러한 이벤트를 처리 하도록 선택할 수 있습니다. 반대로 이벤트를 처리 하는 모든 개체는 더 이상 부모 요소에 도달 하지 않도록 이벤트를 처리 된 것으로 표시할 수 있습니다. 라우트된 이벤트 개념 및 라우트된 이벤트에 대 한 처리기를 작성 하는 방법에 대 한 자세한 내용은 [이벤트 및 라우트된 이벤트 개요](/previous-versions/windows/apps/hh758286(v=win.10))를 참조 하세요.

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항


-   터치 상호 작용을 기본 예상 입력 방법으로 사용 하 여 응용 프로그램을 디자인 합니다.
-   모든 형식 (터치, 펜, 스타일러스, 마우스 등)의 상호 작용에 대 한 시각적 피드백 제공
-   터치 대상 크기, 접촉 기 하 도형, 스크러빙 및 rocking을 조정 하 여 대상 최적화
-   맞추기 지점과 방향 "레일"을 사용 하 여 정확도를 최적화 합니다.
-   긴밀 하 게 압축 된 UI 항목의 터치 정확도를 향상 시키는 데 도움이 되는 도구 설명과 핸들을 제공 합니다.
-   가능 하면 시간 제한 상호 작용을 사용 하지 않습니다 (적절 한 사용 예: 터치 및 보류).
-   가능 하면 조작을 구분 하는 데 사용 되는 손가락 수를 사용 하지 마세요.

## <a name="related-articles"></a>관련된 문서

- [포인터 입력 처리](handle-pointer-input.md)
- [입력 디바이스 식별](identify-input-devices.md)

### <a name="samples"></a>샘플

- [기본 입력 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [짧은 대기 시간 입력 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [사용자 상호 작용 모드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [포커스 화면 효과 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>보관 샘플

- [입력: 장치 기능 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Input: XAML 사용자 입력 이벤트 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [XAML 스크롤, 패닝 및 확대/축소 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [입력: GestureRecognizer를 사용한 제스처 및 조작](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)