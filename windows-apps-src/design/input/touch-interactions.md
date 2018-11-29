---
Description: Create Universal Windows Platform (UWP) apps with intuitive and distinctive user interaction experiences that are optimized for touch but are functionally consistent across input devices.
title: 터치 조작
ms.assetid: DA6EBC88-EB18-4418-A98A-457EA1DEA88A
label: Touch interactions
template: detail.hbs
keywords: 터치, 포인터, 입력, 사용자 조작
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b662a7689f0b0b24fc3f70a9fbc143d4268d2cb8
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8193391"
---
# <a name="touch-interactions"></a>터치 조작


터치가 사용자의 기본적인 입력 방법이 될 것이라는 가정하에 앱을 디자인하세요. UWP 컨트롤을 사용하는 경우 터치 패드, 마우스 및 펜/스타일러스 지원은 UWP 앱에서 무료로 제공되기 때문에 추가 프로그래밍이 필요하지 않습니다.

그러나 터치에 최적화된 UI가 기존 UI보다 항상 나은 것은 아닙니다. 둘 다 기술 및 응용 방식에 고유한 장점과 단점이 있습니다. 터치 우선식 UI로 전환할 경우 터치(터치 패드 포함), 펜/스타일러스, 마우스 및 키보드 입력 간의 주요 차이점을 이해하는 것이 중요합니다.

> **중요 API**: [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994), [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383), [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)


여러 장치에는 입력으로 하나 이상의 손가락을 사용(또는 터치 접촉)하여 지원하는 멀티 터치 화면이 있습니다. 터치 접촉과 해당 움직임은 터치 제스처 및 조작으로 해석되어 다양한 사용자 조작을 지원합니다.

UWP(유니버설 Windows 플랫폼)에는 터치식 입력을 처리하는 다양한 메커니즘이 많이 포함되어 있으므로, 최종 사용자가 자신 있게 이용할 수 있는 몰입형 환경을 만들 수 있습니다. 본 문서는 UWP 앱에서 터치식 입력을 사용하는 기본 사항을 설명합니다.

터치 조작에는 다음 세 가지가 필요합니다.

-   터치 지원 디스플레이.
-   해당 디스플레이에 하나 이상의 손가락을 직접 접촉(또는 디스플레이에 근접 센서가 있으며 가리키기 감지를 지원하는 경우 근접).
-   터치 접촉 이동(또는 시간 임계값을 기반으로 하는 접촉 결핍).

터치 센서에서 제공하는 입력 데이터에는 다음이 적용됩니다.

-   하나 이상의 UI 요소를 직접 조작하기 위한 물리적 제스처로 해석됩니다(예: 이동, 회전, 크기 조정). 반면에 속성 창, 대화 상자 또는 기타 UI 어포던스를 통해 요소를 조작하는 것은 간접 조작으로 간주됩니다.
-   마우스 또는 펜과 같은 대체 입력 메서드로 인식됩니다.
-   펜으로 그린 잉크 스트로크를 문지르는 등 다른 입력 측면을 수정하거나 보완하는 데 사용합니다.

일반적으로 터치식 입력은 화면의 요소를 직접 조작하는 것과 관련이 있습니다. 요소는 해당 적중 횟수 테스트 범위 내에서 모든 터치 접촉에 즉시 응답하며 제거를 포함하여 터치 접촉의 후속 움직임에 적절하게 반응합니다.

사용자 지정 터치 제스처 및 조작은 신중하게 디자인해야 합니다. 또한 직관적이고 반응성이 뛰어나며 검색 가능해야 합니다. 그리고 사용자가 자신 있게 앱을 탐색할 수 있도록 해야 합니다.

지원되는 모든 입력 장치 유형 간에 앱 기능이 일관되게 표시되도록 해야 합니다. 필요한 경우 키보드 조작을 위한 텍스트 입력 또는 마우스 및 펜용 UI 어포던스와 같이 몇 가지 간접 입력 모드 형식을 사용합니다.

일반적인 입력 장치(예: 마우스와 키보드)는 많은 사용자에게 친숙하며 매력적으로 다가옵니다. 터치와 달리 속도, 정확도 및 촉각 피드백을 제공할 수 있습니다.

모든 입력 디바이스에 고유하고 독특한 조작 환경을 제공하면 가장 포괄적인 기능 및 기본 설정을 지원하므로 최대한 많은 사용자에게 환영받게 되어 앱으로 더 많은 고객을 이끌 수 있습니다.

## <a name="compare-touch-interaction-requirements"></a>터치 조작 요구 사항 비교

다음 표에는 터치 최적화된 UWP 앱을 디자인할 때 고려해야 할 입력 디바이스 간의 몇 가지 차이점이 나와 있습니다.

<table>
<tbody><tr><th>요인</th><th>터치 조작</th><th>마우스, 키보드, 펜/스타일러스 조작</th><th>터치 패드</th></tr>
<tr><td rowspan="3">정밀도</td><td>손가락 끝의 접촉 영역이 단일 x-y 좌표보다 크므로 의도하지 않은 명령을 활성화할 가능성이 높아집니다.</td><td>마우스 및 펜/스타일러스는 정확한 x-y 좌표를 제공합니다.</td><td>마우스와 동일합니다.</td></tr>
<tr><td>접촉 영역의 모양이 움직이면서 달라집니다.  </td><td>마우스 움직임 및 펜/스타일러스 스트로크는 정확한 x-y 좌표를 제공합니다. 키보드 포커스가 명확합니다.</td><td>마우스와 동일합니다.</td></tr>
<tr><td>대상 지정을 지원하는 마우스 커서가 없습니다.</td><td>마우스 커서, 펜/스타일러스 커서, 키보드 포커스가 모두 대상 지정을 지원합니다.</td><td>마우스와 동일합니다.</td></tr>
<tr><td rowspan="3">인체 구조</td><td>하나 이상의 손가락을 사용하여 직선으로 이동하는 것은 어렵기 때문에 손끝을 이용한 이동은 정확하지 않습니다. 이것은 손 마디의 곡률과 동작에 관련된 마디 수 때문입니다.</td><td>손으로 마우스나 펜/스타일러스를 조정할 경우에는 화면의 커서보다 실제로 더 짧은 거리를 이동하게 되므로 이러한 도구로 직선 동작을 수행하는 것이 더 쉽습니다.</td><td>마우스와 동일합니다.</td></tr>
<tr><td>디스플레이 장치에서 터치 화면의 일부 영역은 손가락 포스처 때문에, 그리고 사용자가 장치를 잡아야 하기 때문에 닿기 어려울 수 있습니다.</td><td>마우스 및 펜/스타일러스는 탭 순서를 통해 키보드로 컨트롤에 액세스하면서 화면의 어디에든 닿을 수 있습니다. </td><td>손가락 위치와 잡는 방법이 문제가 될 수 있습니다.</td></tr>
<tr><td>하나 이상의 손가락이나 사용자의 손으로 물체를 가릴 수 있습니다. 이것을 폐색이라고 합니다.</td><td>간접 입력 장치는 폐색을 야기하지 않습니다.</td><td>마우스와 동일합니다.</td></tr>
<tr><td>개체 상태</td><td>터치는 두 개의 상태로 존재하는 모델을 사용합니다. 디스플레이 장치의 터치 표면은 터치됨(켜짐) 또는 터치되지 않음(꺼짐)의 상태로 존재합니다. 추가적인 시각적 피드백을 발생할 수 있는 가리키기 상태는 없습니다.</td><td>
<p>마우스, 펜/스타일러스 및 키보드는 모두 위쪽(꺼짐), 아래쪽(켜짐), 및 가리키기(포커스)의 세 가지 상태로 존재합니다.</p>
<p>가리키기를 사용하면 UI 요소와 관련된 도구 설명을 보고 정보를 얻을 수 있습니다. 가리키기 및 포커스 효과는 조작하는 개체에 연결되며 타기팅에도 도움이 될 수 있습니다. 
</p>
</td><td>마우스와 동일합니다.</td></tr>
<tr><td rowspan="2">풍부한 조작 방식</td><td>멀티 터치, 즉 터치 화면의 다중 입력 지점(손가락 끝)이 지원됩니다.</td><td>단일 입력 지점이 지원됩니다.</td><td>터치와 동일합니다.</td></tr>
<tr><td>탭하기, 끌기, 밀기, 손가락 모으기 및 회전과 같은 동작을 통해 개체를 직접적으로 조작할 수 있습니다.</td><td>마우스, 펜/스타일러스 및 키보드는 간접 입력 장치이므로 직접 조작은 지원되지 않습니다.</td><td>마우스와 동일합니다.</td></tr>
</tbody></table>



**참고**  간접 입력 25 년 이상 미세 혜택 했습니다. 가리키기로 트리거되는 도구 설명과 같은 기능은 터치 패드, 마우스, 펜/스타일러스 및 키보드 입력을 비롯한 UI 탐색 문제를 해결하도록 디자인되었습니다. 이러한 UI 기능은 다른 장치의 사용자 환경을 손상시키지 않고 터치 입력에서 제공하는 풍부한 환경에 맞게 다시 디자인되었습니다.

 

## <a name="use-touch-feedback"></a>터치 피드백 사용

앱과의 조작 중에 적절 한 시각적 피드백이 사용자 익히고, 앱과는 Windowsplatform 모두에서 조작이 해석 되는 방식이에 적응할 수 있습니다. 시각적 피드백은 성공적인 조작을 알리고, 시스템 상태를 전달하고, 제어 기능을 향상시키고, 오류를 줄이고, 사용자가 시스템 및 입력 장치를 이해하는 데 도움을 주고, 조작을 권장할 수 있습니다.

위치에 따라 정확도와 정밀도를 요구하는 활동에 터치식 입력을 사용할 경우 시각적 피드백이 중요합니다. 터치식 입력이 감지될 때마다 피드백을 표시하면 사용자가 앱 및 컨트롤이 정의하는 사용자 지정 대상 지정 규칙을 이해하는 데 도움이 됩니다.


## <a name="targeting"></a>대상 지정

다음을 통해 대상을 최적으로 지정할 수 있습니다.

-   터치 대상 크기

    크기 지우기 지침을 통해 응용 프로그램에서 대상에 쉽고 안전한 개체 및 컨트롤이 포함된 익숙한 UI를 제공할 수 있습니다.

-   접촉 기하

    손가락의 전체 접촉 영역은 가장 가능성이 큰 대상 개체를 결정합니다.

-   스크러빙

    항목(예제: 라디오 단추) 간을 손가락으로 끌어서 그룹 내의 항목 대상을 쉽게 다시 지정합니다. 손가락을 떼면 현재 항목이 활성화됩니다.

-   로킹

    손가락을 아래로 누르고 밀지 않고 앞뒤로 흔들어 고밀도로 압축된 항목(예제: 하이퍼링크) 대상을 쉽게 다시 지정합니다. 폐색으로 인해 현재 항목이 도구 설명 또는 상태바를 통해 식별되며 손가락을 떼면 활성화됩니다.

## <a name="accuracy"></a>정확도

정확하지 않은 조작에 대해 다음을 사용하여 디자인합니다.

-   사용자가 콘텐츠를 조작할 때 원하는 위치에서 쉽게 멈출 수 있는 끌기 지점
-   손이 약간 호를 그리며 움직일 경우에도 가로 또는 세로로 이동하도록 도와주는 방향 "길"입니다. 자세한 내용은 [이동에 대한 지침](guidelines-for-panning.md)을 참조하세요.

## <a name="occlusion"></a>폐색

다음을 통해 손가락 및 손의 폐색을 피합니다.

-   UI 크기 및 위치 지정

    손가락 끝 접촉 영역으로는 완전히 커버할 수 없도록 UI 요소를 크게 만듭니다.

    가능할 때마다 메뉴와 팝업을 접촉 영역에 놓습니다.

-   도구 설명

    손가락을 개체에 접촉한 상태로 유지하면 도구 설명을 표시합니다. 도구 설명은 개체 기능을 설명하는 데 유용합니다. 사용자는 도구 설명이 호출되지 않도록 개체에서 손가락을 뗄 수 있습니다.

    작은 개체의 경우 도구 설명을 상쇄시켜 손가락 끝 접촉 영역으로 커버되지 않게 합니다. 이는 대상을 지정하는 데 도움이 됩니다.

-   정확도용 핸들

    텍스트 선택과 같이 정확해야 하는 경우 정확도를 향상시키도록 선택 핸들을 제공합니다. 자세한 내용은 [텍스트 및 이미지 선택에 대한 지침(Windows 런타임 앱)](guidelines-for-textselection.md)을 참조하세요.

## <a name="timing"></a>타이밍

직접 조작을 위해 시간 제한 모드를 변경하지 마세요. 직접 조작은 개체의 직접적인 실시간 실제 처리를 시뮬레이트합니다. 개체는 손가락이 이동할 때 반응합니다.

반면에 시간이 제한된 조작은 터치 조작 이후에 발생합니다. 시간이 제한된 조작은 일반적으로 시간, 거리 또는 속도와 같은 보이지 않는 임계값에 의존하여 수행할 명령을 결정합니다. 시간이 제한된 조작은 시스템이 동작을 수행할 때까지 시각적 피드백을 제공하지 않습니다.

직접적인 조작은 시간이 제한된 조작에 많은 이점을 제공합니다.

-   조작하는 동안의 즉각적인 시각적 피드백으로 인해 사용자는 제대로 작업 중이라고 좀 더 확신하게 됩니다.
-   직접적인 조작은 복구가 가능하기 때문에 시스템을 좀 더 안전하게 탐색할 수 있습니다. 사용자는 작업하는 동안 논리적 및 직관적인 방법으로 쉽게 이전 단계로 갈 수 있습니다.
-   개체에 직접적인 영향을 주는 조작 및 실제와 비슷하게 모방한 조작은 좀더 직관적이고 검색 및 기억하기 쉽습니다. 모호하거나 추상적인 조작을 사용하지 않습니다.
-   시간이 제한된 조작은 사용자가 보이지 않는 임의의 임계값에 도달해야 하므로 수행하기 어려울 수 있습니다.

그 외에도 다음과 같이 하는 것이 좋습니다.

-   사용한 손가락 수로 조작을 구별해서는 안 됩니다.
-   조작 방식은 복합적인 조작을 지원해야 합니다. 예를 들어, 손가락을 끌어서 이동하는 동안 손가락을 모아 확대/축소할 경우
-   조작은 시간으로 구분되어서는 안 됩니다. 같은 조작은 수행하는 데 걸린 시간과 상관 없이 결과가 같아야 합니다. 시간 기반 활성화는 사용자를 강제로 지연시키며 직접 조작의 몰입성과 시스템 응답 성능의 인식 능력을 모두 저하시킵니다.

    **참고**학습 및 탐색 (예: 길게 누르기)에 도움이 되도록 특정 시간이 제한 된 조작은 사용 하는 예외입니다.

     

-   적절한 설명 및 시각 신호는 앞으로의 조작에 큰 영향을 미칩니다.


## <a name="app-views"></a>앱 보기


앱 뷰의 이동/스크롤 및 확대/축소 설정을 통해 사용자 조작 환경을 조정할 수 있습니다. 앱 뷰는 사용자가 앱과 해당 콘텐츠를 액세스하고 조작하는 방법을 제어합니다. 뷰는 관성, 콘텐츠 경계 바운스 및 끌기 지점과 같은 동작도 제공합니다.

[**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527) 컨트롤의 이동 및 스크롤 설정은 보기의 콘텐츠가 뷰포트 내에 맞지 않을 때 사용자가 단일 보기 내에서 탐색하는 방법을 지정합니다. 예를 들어 단일 보기는 잡지 또는 책의 페이지, 컴퓨터의 폴더 구조, 문서 라이브러리 또는 사진 앨범일 수 있습니다.

확대/축소 설정은 광학 줌([**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527) 컨트롤에서 지원) 및 [**Semantic Zoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) 컨트롤에 모두 적용됩니다. 시맨틱 줌은 단일 뷰 내에서 대규모의 관련 데이터 또는 콘텐츠 집합을 제공하고 탐색하기 위한 터치 최적화된 기법입니다. 두 가지 분류 모드 또는 확대/축소 수준을 사용하여 작동합니다. 단일 뷰 내에서 이동 및 스크롤하는 것과 비슷합니다. 이동 및 스크롤을 시맨틱 줌과 함께 사용할 수 있습니다.

앱 보기 및 이벤트를 사용하여 이동/스크롤 및 확대/축소 동작을 수정합니다. 그러면 포인터 및 제스처 이벤트 처리를 통해 가능한 것보다 매끄러운 조작 환경을 제공할 수 있습니다.

앱 보기에 대한 자세한 내용은 [컨트롤, 레이아웃 및 텍스트](https://msdn.microsoft.com/library/windows/apps/mt228348)를 참조하세요.

## <a name="custom-touch-interactions"></a>사용자 지정 터치 조작


조작 지원을 직접 구현할 경우 사용자들은 앱의 UI 요소와의 직접적 조작이 이루어지는 직관적인 환경을 기대한다는 점에 유의하세요. 플랫폼 컨트롤 라이브러리에서 항목이 일관되고 검색 가능하도록 사용자 지정 조작을 모델링하는 것이 좋습니다. 이러한 라이브러리의 컨트롤은 표준 조작, 애니메이션 효과를 준 물리적 효과, 시각적 피드백 및 접근성을 비롯하여 사용자 조작 환경 전체를 제공합니다. 요구 사항이 명확하게 잘 정의되어 있으며 기본 제스처가 시나리오를 지원하지 않는 경우에만 사용자 지정 조작을 만드세요.

사용자 지정 터치 지원을 제공하기 위해 다양한 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 이벤트를 처리할 수 있습니다. 이러한 이벤트는 세 가지의 추상화 수준으로 그룹화됩니다.

-   정적 제스처 이벤트는 조작이 완료된 후 트리거됩니다. 제스처 이벤트에는 [**Tapped**](https://msdn.microsoft.com/library/windows/apps/br208985), [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/br208922), [**RightTapped**](https://msdn.microsoft.com/library/windows/apps/br208984) 및 [**Holding**](https://msdn.microsoft.com/library/windows/apps/br208928)이 포함됩니다.

    [**IsTapEnabled**](https://msdn.microsoft.com/library/windows/apps/br208939), [**IsDoubleTapEnabled**](https://msdn.microsoft.com/library/windows/apps/br208931), [**IsRightTapEnabled**](https://msdn.microsoft.com/library/windows/apps/br208937) 및 [**IsHoldingEnabled**](https://msdn.microsoft.com/library/windows/apps/br208935)를 **false**로 설정하여 특정 요소에서 제스처 이벤트를 사용하지 않도록 설정할 수 있습니다.

-   [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) 및 [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970)와 같은 포인터 이벤트는 포인터 동작 및 누르기와 놓기 이벤트를 구별하는 기능을 비롯하여 각 터치 접촉에 대한 낮은 수준의 세부 정보를 제공합니다.

    포인터는 통합 이벤트 메커니즘이 있는 일반 입력 유형입니다. 포인터는 터치, 터치 패드, 마우스 또는 펜 등의 활성 입력 원본에서 화면 위치와 같은 기본 정보를 표시합니다.

-   [**ManipulationStarted**](https://msdn.microsoft.com/library/windows/apps/br208950)와 같은 조작 제스처 이벤트는 진행 중인 조작을 나타냅니다. 조작 제스처 이벤트는 사용자가 요소를 터치할 때 발생하기 시작하고, 손가락을 들거나 조작이 취소될 때까지 계속됩니다.

    조작 이벤트에는 확대/축소, 이동 또는 회전과 같은 멀티 터치 조작과 끌기처럼 관성과 속도 데이터를 사용하는 조작이 포함됩니다. 조작 이벤트에서 제공하는 정보는 수행된 조작 형식을 확인하지 않고 오히려 위치, 변환 델타 및 속도와 같은 데이터를 포함합니다. 이 터치 데이터를 사용하여 수행되어야 하는 조작 유형을 파악할 수 있습니다.

다음은 UWP에서 지원하는 터치 제스처의 기본 집합입니다.

| 이름           | 유형                 | 설명                                                                            |
|----------------|----------------------|----------------------------------------------------------------------------------------|
| 탭하기            | 정적 제스처       | 손가락 하나로 화면을 터치하고 뗍니다.                                            |
| 길게 누르기 | 정적 제스처       | 손가락 하나로 화면을 터치한 다음 누르고 있습니다.                                      |
| 밀기          | 조작 제스처 | 하나 이상의 손가락으로 화면을 터치하고 같은 방향으로 움직입니다.                   |
| 살짝 밀기          | 조작 제스처 | 하나 이상의 손가락으로 화면을 터치하고 같은 방향으로 짧게 움직입니다.  |
| 회전           | 조작 제스처 | 둘 이상의 손가락으로 화면을 터치하고 시계 방향으로 또는 시계 반대 방향으로 호를 그리며 움직입니다. |
| 손가락 모으기          | 조작 제스처 | 둘 이상의 손가락으로 화면을 터치하고 모으면서 움직입니다.                         |
| 늘이기        | 조작 제스처 | 둘 이상의 손가락으로 화면을 터치하고 늘리면서 움직입니다.                           |

 

<!-- mijacobs: Removing for now. We don't have a real page to link to yet. 
For more info about gestures, manipulations, and interactions, see [Custom user interactions](custom-user-input-portal.md).
-->

## <a name="gesture-events"></a>제스처 이벤트


개별 컨트롤에 대한 자세한 내용은 [컨트롤 목록](https://msdn.microsoft.com/library/windows/apps/mt185406)을 참조하세요.

## <a name="pointer-events"></a>포인터 이벤트


포인터 이벤트는 터치, 터치 패드, 펜 및 마우스를 비롯한 다양한 활성 입력 소스에 의해 발생합니다(기존 마우스 이벤트를 대체함).

포인터 이벤트는 단일 입력 지점(손가락, 펜 팁, 마우스 커서)을 기반으로 하며 속도 기반 조작을 지원하지 않습니다.

다음은 포인터 이벤트 및 해당 관련 이벤트 인수 목록입니다.

| 이벤트 또는 클래스                                                       | 설명                                                   |
|----------------------------------------------------------------------|---------------------------------------------------------------|
| [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971)             | 한 손가락으로 화면을 터치하는 경우 발생합니다.               |
| [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972)           | 해당하는 동일 터치 접촉을 떼면 발생합니다.                |
| [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970)                 | 포인터를 화면에서 끌 때 발생합니다.         |
| [**PointerEntered**](https://msdn.microsoft.com/library/windows/apps/br208968)             | 포인터가 요소의 적중 횟수 테스트 범위에 들어갈 때 발생합니다. |
| [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969)               | 포인터가 요소의 적중 횟수 테스트 범위를 벗어날 때 발생합니다.  |
| [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964)           | 터치 접촉이 비정상적으로 손실될 때 발생합니다.               |
| [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965)     | 다른 요소에 의해 포인터 캡처가 수행될 때 발생합니다.    |
| [**PointerWheelChanged**](https://msdn.microsoft.com/library/windows/apps/br208973)   | 마우스 휠의 델타 값이 변경될 때 발생합니다.         |
| [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076) | 모든 포인터 이벤트에 대한 데이터를 제공합니다.                         |

 

다음 예제에서는 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971), [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) 및 [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969) 이벤트를 사용하여 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 개체에 대한 탭 조작을 처리하는 방법을 보여 줍니다.

먼저, XAML(Extensible Application Markup Language)에서 `touchRectangle`이라는 이름의 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)을 만듭니다.

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
           Height="100" Width="200" Fill="Blue" />
</Grid>
```
다음으로, [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971), [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) 및 [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969) 이벤트에 대한 수신기를 지정합니다.

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

마지막으로, [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) 이벤트 처리기에서 [**Rectangle**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height)의 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 및 [**Width**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)를 증가시키며, [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) 및 [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969) 이벤트 처리기에서는 **Height** 및 **Width**를 다시 해당 시작 값으로 설정합니다.

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


앱에서 여러 손가락 조작 또는 속도 데이터가 필요한 조작을 지원해야 할 경우 조작 이벤트를 사용합니다.

조작 이벤트를 사용하여 끌기, 확대/축소 및 길게 누르기와 같은 조작을 감지할 수 있습니다.

다음은 조작 이벤트 및 관련 이벤트 인수 목록입니다.

| 이벤트 또는 클래스                                                                                               | 설명                                                                                                                               |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| [**ManipulationStarting 이벤트**](https://msdn.microsoft.com/library/windows/apps/br208951)                                   | 조작 프로세서가 처음 만들어질 때 발생합니다.                                                                                  |
| [**ManipulationStarted 이벤트**](https://msdn.microsoft.com/library/windows/apps/br208950)                                     | 입력 디바이스가 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)에 대한 조작을 시작할 때 발생합니다.                                            |
| [**ManipulationDelta 이벤트**](https://msdn.microsoft.com/library/windows/apps/br208946)                                         | 입력 디바이스가 조작 중에 위치를 바꿀 때 발생합니다.                                                                      |
| [**ManipulationInertiaStarting 이벤트**](https://msdn.microsoft.com/library/windows/apps/hh702425)                | 조작하는 동안 입력 디바이스와 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 개체의 연결이 끊어지고 관성이 시작될 때 발생합니다. |
| [**ManipulationCompleted 이벤트**](https://msdn.microsoft.com/library/windows/apps/br208945)                                 | [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)에 대한 조작 및 관성이 완료될 때 발생합니다.                                          |
| [**ManipulationStartingRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702132)               | [**ManipulationStarting**](https://msdn.microsoft.com/library/windows/apps/br208951) 이벤트에 대한 데이터를 제공합니다.                                         |
| [**ManipulationStartedRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702101)                 | [**ManipulationStarted**](https://msdn.microsoft.com/library/windows/apps/br208950) 이벤트에 대한 데이터를 제공합니다.                                           |
| [**ManipulationDeltaRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702051)                     | [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946) 이벤트에 대한 데이터를 제공합니다.                                               |
| [**ManipulationInertiaStartingRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702074) | [**ManipulationInertiaStarting**](https://msdn.microsoft.com/library/windows/apps/br208947) 이벤트에 대한 데이터를 제공합니다.                           |
| [**ManipulationVelocities**](https://msdn.microsoft.com/library/windows/apps/br242032)                                              | 조작이 발생하는 속도를 설명합니다.                                                                                         |
| [**ManipulationCompletedRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702035)             | [**ManipulationCompleted**](https://msdn.microsoft.com/library/windows/apps/br208945) 이벤트에 대한 데이터를 제공합니다.                                       |

 

제스처는 일련의 조작 이벤트로 구성됩니다. 각 제스처는 사용자가 화면을 터치할 때와 같은 [**ManipulationStarted**](https://msdn.microsoft.com/library/windows/apps/br208950) 이벤트로 시작됩니다.

다음에는 하나 이상의 [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946) 이벤트가 발생합니다. 예를 들어 화면을 터치한 후 손가락을 화면에서 끌면 이벤트가 발생합니다. 마지막으로 조작이 완료되면 [**ManipulationCompleted**](https://msdn.microsoft.com/library/windows/apps/br208945) 이벤트가 발생합니다.

**참고**터치 스크린 모니터가 없는 경우 마우스 및 마우스 휠 인터페이스를 사용 하 여 시뮬레이터에서 조작 이벤트 코드를 테스트할 수 있습니다.

 

다음 예제에서는 [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946) 이벤트를 사용하여 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 개체에 대한 슬라이드 조작을 처리하고 화면에서 이동하는 방법을 보여 줍니다.

먼저 이름이 `touchRectangle`이며 [**Height**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 및 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height)가 200인 [**Rectangle**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)이 XAML에 만들어집니다.

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
               Width="200" Height="200" Fill="Blue" 
               ManipulationMode="All"/>
</Grid>
```

다음에는 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243027)을 변환하기 위해 이름이 `dragTranslation`인 전역 [**TranslateTransform**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)이 만들어집니다. **Rectangle**에서 [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946) 이벤트 수신기를 지정하고, **Rectangle**의 [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/br208980)에 `dragTranslation`을 추가합니다.

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

마지막으로 [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946) 이벤트 처리기에서 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)의 위치가 [**Delta**](https://msdn.microsoft.com/library/windows/apps/br243027) 속성의 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/hh702058)을 사용하여 업데이트됩니다.

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


여기서 언급한 모든 포인터 이벤트, 제스처 이벤트 및 조작 이벤트는 *라우트된 이벤트*로 구현됩니다. 즉, 원래 이벤트를 발생시킨 개체가 아닌 개체에서 이벤트를 처리할 수 있습니다. [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)의 부모 컨테이너 또는 앱의 루트 [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503)와 같은 개체 트리의 연속된 부모는 원래 요소가 처리하지 않는 경우에도 이러한 이벤트를 처리하도록 선택할 수 있습니다. 반대로, 이벤트를 처리하는 개체는 더 이상 부모 요소에 도달하지 않도록 이벤트를 처리된 것으로 표시할 수 있습니다. 라우트된 이벤트 개념 및 라우트된 이벤트의 처리기를 작성하는 방법에 미치는 영향에 대한 자세한 내용은 [이벤트 및 라우트된 이벤트 개요](https://msdn.microsoft.com/library/windows/apps/hh758286)를 참조하세요.

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항


-   터치 조작이 있는 응용 프로그램을 기본 입력 방법으로 디자인합니다.
-   모든 형식의 조작(터치, 펜, 스타일러스, 마우스 등)에 대한 시각적 피드백을 제공합니다.
-   터치 대상 크기, 접촉 기하, 스크러빙 및 로킹을 조정하여 대상을 최적화합니다.
-   끌기 지점 및 방향 "레일"을 사용하여 정확도를 최적화합니다.
-   밀접하게 압축된 UI 항목의 터치 정확도를 향상시키기 위해 도구 설명 및 핸들을 제공합니다.
-   가능하면 기간이 정해진 조작을 사용하지 마세요(적절한 사용의 예제: 길게 누르기).
-   가능하면 조작을 구분하기 위해 손가락 수를 사용하지 마세요.


## <a name="related-articles"></a>관련 문서

* [포인터 입력 처리](handle-pointer-input.md)
* [입력 디바이스 식별](identify-input-devices.md)

**샘플**

* [기본 입력 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [짧은 대기 시간 입력 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [사용자 조작 모드 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [포커스 화면 효과 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**보관 샘플**

* [입력: 디바이스 기능 샘플](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [입력: XAML 사용자 입력 이벤트 샘플](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [XAML 스크롤, 이동 및 확대/축소 샘플](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [입력: GestureRecognizer를 사용한 조작 및 제스처](http://go.microsoft.com/fwlink/p/?LinkID=231605)
 

 




