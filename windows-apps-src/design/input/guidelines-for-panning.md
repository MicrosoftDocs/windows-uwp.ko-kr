---
Description: 이동 또는 스크롤을 통해 사용자는 단일 뷰 내에서 이동 하 여 뷰포트 내에 맞지 않는 뷰의 콘텐츠를 표시할 수 있습니다. 보기의 예로는 컴퓨터의 폴더 구조, 문서 라이브러리 또는 사진 앨범이 있습니다.
title: 이동
ms.assetid: b419f538-c7fb-4e7c-9547-5fb2494c0b71
label: Panning
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 596a9f2f3f234ba90b799eae982523c3a9de9732
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91749939"
---
# <a name="guidelines-for-panning"></a>패닝에 대 한 지침


이동 또는 스크롤을 통해 사용자는 단일 뷰 내에서 이동 하 여 뷰포트 내에 맞지 않는 뷰의 콘텐츠를 표시할 수 있습니다. 보기의 예로는 컴퓨터의 폴더 구조, 문서 라이브러리 또는 사진 앨범이 있습니다.

> **중요 한 api**: [**windows**](/uwp/api/Windows.UI.Input). Input, [**windows. .xaml**](/uwp/api/Windows.UI.Xaml.Input)


## <a name="dos-and-donts"></a>권장 사항 및 금지 사항


**표시기 및 스크롤 막대 패닝**

-   앱에 콘텐츠를 로드 하기 전에 패닝/스크롤이 가능한 지 확인 합니다.

-   위치 및 크기 큐를 제공 하기 위해 패닝 표시기 및 스크롤 막대를 표시 합니다. 사용자 지정 탐색 기능을 제공 하는 경우 숨깁니다.

    **참고**    표준 스크롤 막대와 달리 패닝 표시기는 전적으로 정보를 제공 합니다. 이러한 장치는 입력 장치에 노출 되지 않으며 어떤 방식으로든 조작할 수 없습니다.

     

**단일 축 패닝 (1 차원 오버플로)**

-   하나의 뷰포트 경계 (세로 또는 가로) 이상으로 확장 되는 콘텐츠 영역에 대해 1 축 이동을 사용 합니다.

    -   항목의 1 차원 목록에 대해 세로 방향으로 이동 합니다.
    -   항목 그리드를 위한 가로 패닝
-   사용자가 스냅 지점과 함께 팬 하 고 중지할 수 있어야 하는 경우 단일 축 패닝에서 필수 스냅 지점도 사용 하지 마세요. 필수 스냅 지점은 사용자가 맞춤 지점에서 중지 되도록 보장 합니다. 대신 근접 한 맞춤 위치를 사용 합니다.

**자유형 패닝 (2 차원 오버플로)**

-   뷰포트 경계 (세로 및 가로) 이상으로 확장 되는 콘텐츠 영역에 대해 2 축 이동을 사용 합니다.

    -   기본 레일 동작을 재정의 하 고 사용자가 여러 방향으로 이동할 가능성이 높은 구조화 되지 않은 콘텐츠에 대해 자유형 이동을 사용 합니다.
-   자유형 패닝은 일반적으로 이미지나 맵 내에서 탐색 하는 데 적합 합니다.

**페이지 보기**

-   콘텐츠를 불연속 요소로 구성 하거나 전체 요소를 표시 하려는 경우에는 필수 맞춤 위치를 사용 합니다. 여기에는 책 또는 잡지의 페이지, 항목의 열 또는 개별 이미지가 포함 될 수 있습니다.

    -   각 논리적 경계에 스냅 지점을 배치 해야 합니다.
    -   각 요소의 크기를 조정 하거나 뷰에 맞게 크기를 조정 해야 합니다.

**논리적 및 핵심 사항**

-   콘텐츠 내에서 사용자가 중지할 수 있는 핵심 지점이 나 논리적 위치가 있으면 근접 한 스냅 지점을 사용 합니다. 예를 들어 섹션 헤더입니다.

-   최대 및 최소 크기 제약 조건 또는 경계가 정의 된 경우 시각적 피드백을 사용 하 여 사용자가 해당 경계에 도달 하거나이를 초과 하는 경우를 보여 줍니다.

**포함 되거나 중첩 된 콘텐츠 연결**

-   텍스트 및 표 기반 콘텐츠에는 단일 축 패닝 (일반적으로 가로) 및 열 레이아웃을 사용 합니다. 이러한 경우, 콘텐츠는 일반적으로 열에서 열로 자연스럽 게 래핑 및 전달 되며 사용자 환경을 일관 되 게 유지 하 고 Windows 앱에서 검색할 수 있도록 합니다.

-   포함 된 pannable 영역을 사용 하 여 텍스트 또는 항목 목록을 표시 하지 않습니다. 영역 내에서 입력 연락처가 검색 된 경우에만 패닝 표시기 및 스크롤 막대가 표시 되므로 직관적 이거나 검색 가능한 사용자 환경이 아닙니다.

-   여기에 표시 된 것 처럼 동일한 방향으로 이동 하는 경우 다른 pannable 지역 내에 하나의 pannable 지역을 연결 하거나 위치를 두지 마십시오. 이로 인해 자식 영역에 대 한 경계에 도달 하면 부모 영역이 실수로 따라 이동 될 수 있습니다. 패닝 축을 수직으로 만드는 것이 좋습니다.

    ![컨테이너와 동일한 방향으로 스크롤 하는 포함 된 pannable 영역을 보여 주는 이미지입니다.](images/scrolling-embedded3.png)

## <a name="additional-usage-guidance"></a>추가 사용법 지침

터치를 사용 하 여 이동 하는 경우 하나 이상의 손가락으로 살짝 밀기 또는 밀기 제스처를 사용 하는 것은 마우스로 스크롤 하는 것과 같습니다. 패닝 상호 작용은 스크롤 막대를 클릭 하는 대신 마우스 휠을 회전 하거나 스크롤 상자를 슬라이딩 하는 것과 비슷합니다. API에서 구분 하거나 특정 장치별 Windows UI에 필요한 경우를 제외 하 고는 두 가지 상호 작용을 패닝 이라고 지칭 합니다.

> <div id="main">
> <strong>Windows 10의 작성자 업데이트-동작 변경</strong> 기본적으로 텍스트를 선택 하는 대신 활성 펜은 터치, 터치 패드, 수동 펜 등의 Windows 앱에서 스크롤/계획 됩니다.  
> 앱이 이전 동작을 사용하는 경우 펜 스크롤을 재정의하고 이전 동작으로 되돌릴 수 있습니다. 자세한 내용은 <a href="/uwp/api/windows.ui.xaml.controls.scrollviewer">ScrollViewer 클래스</a>의 API 참조 항목을 참조하세요.
> </div>

입력 장치에 따라 사용자는 다음 중 하나를 사용 하 여 pannable 지역 내에서 계획 합니다.

-   스크롤 화살표를 클릭 하거나 스크롤 상자를 끌거나 스크롤 막대 내부를 클릭 하는 마우스, 터치 패드 또는 활성 펜/스타일러스입니다.
-   스크롤 상자 끌기를 에뮬레이트하기 위한 마우스의 휠 단추입니다.
-   마우스에서 지 원하는 경우 확장 된 단추 (XBUTTON1 및 있는)입니다.
-   스크롤 상자를 에뮬레이트할 스크롤 상자 또는 페이지 키를 에뮬레이트할 키보드 화살표 키입니다.
-   원하는 방향으로 손가락을 이동 하거나 살짝 밀기 위한 터치, 터치 패드 또는 수동 펜/스타일러스입니다.

슬라이딩에는 손가락을 패닝 방향으로 천천히 이동 하는 작업이 포함 됩니다. 그러면 일 대 일 관계가 생성 됩니다. 여기서 콘텐츠는 손가락의 속도와 거리가 계획. 손가락을 신속 하 게 이동 하 고 리프트 하는 살짝 밀기는 다음과 같은 물리가 패닝 애니메이션에 적용 됩니다.

-   감속 (관성): 손가락을 떼는 경우 팬이 감속 시작 됩니다. 이는 slippery 표면의 중지를 슬라이딩 하는 것과 비슷합니다.
-   Absorption: 감속 중에 모멘텀를 패닝 하면 맞춤 지점이 나 콘텐츠 영역 경계에 도달할 경우 약간의 바운스 효과가 발생 합니다.

**패닝 유형**

Windows 8은 세 가지 유형의 이동을 지원 합니다.

-   단일 축-패닝은 한 방향 으로만 지원 됩니다 (가로 또는 세로).
-   레일 패닝은 모든 방향에서 지원 됩니다. 그러나 사용자가 특정 방향으로 거리 임계값을 초과 하면 패닝은 해당 축으로 제한 됩니다.
-   자유형 패닝은 모든 방향에서 지원 됩니다.

**패닝 UI**

이동에 대 한 상호 작용 환경은 입력 장치에 대해 고유 하며 여전히 유사한 기능을 제공 합니다.

**Pannable 지역** Pannable 영역 동작은 디자인 타임에 CSS (CSS 스타일시트)를 통해 JavaScript 개발자를 사용 하 여 Windows 앱에 노출 됩니다.

검색 된 입력 장치를 기반으로 하는 두 개의 패닝 디스플레이 모드가 있습니다.

-   터치를 위한 패닝 표시기입니다.
-   마우스, 터치 패드, 키보드 및 스타일러스를 비롯 한 다른 입력 장치의 스크롤 막대.

**참고**    패닝 표시기는 터치 접점이 pannable 지역 내에 있는 경우에만 표시 됩니다. 마찬가지로 스크롤 막대는 마우스 커서, 펜/스타일러스 커서 또는 키보드 포커스가 스크롤 가능 영역 내에 있는 경우에만 표시 됩니다.

 

**패닝 표시기** 패닝 표시기는 스크롤 막대의 스크롤 상자와 비슷합니다. 이는 pannable 영역에 표시 되는 콘텐츠의 비율과 pannable 영역에 표시 되는 콘텐츠의 상대적 위치를 표시 합니다.

다음 다이어그램에서는 길이가 다른 두 개의 pannable 영역 및 패닝 표시기를 보여 줍니다.

![길이가 다른 두 개의 pannable 영역 및 패닝 표시기를 보여 주는 이미지입니다.](images/scrolling-indicators.png)

**패닝 동작** 
 **맞춤 요소** 터치 접점이 리프트 될 때 살짝 밀기 제스처를 사용 하 여 이동 하면 관성 동작이 상호 작용에 도입 됩니다. 관성을 사용 하면 사용자의 직접 입력 없이도 특정 거리 임계값에 도달할 때까지 콘텐츠가 계속 이동 합니다. 맞추기 요소를 사용 하 여이 관성 동작을 수정 합니다.

맞춤 지점은 앱 콘텐츠의 논리적 중지를 지정 합니다. Cognitively, 스냅 지점은 사용자에 대 한 페이징 메커니즘 역할을 하며 과도 한 슬라이딩 또는 피로 (large pannable) 지역에서의 최소화 합니다. 이를 통해 부정확 한 사용자 입력을 처리 하 고 특정 콘텐츠나 키 정보의 하위 집합이 뷰포트에 표시 되도록 할 수 있습니다.

다음과 같은 두 가지 유형의 스냅 지점이 있습니다.

-   근접-연락처가 리프트 된 후에 관성이 맞춤 지점의 거리 임계값 내에서 중지 되 면 맞춤 지점이 선택 됩니다. 근접 한 맞춤 지점의 경우에도 이동이 중지 될 수 있습니다.
-   필수-제스처의 방향 및 속도에 따라, 선택 된 스냅 지점은 마지막 스냅 지점을 바로 앞 이나 뒤에 오도록 하는 것입니다. 패닝은 필수 맞춤 지점에서 중지 되어야 합니다.

이동 중심점은 페이지가 매겨진 콘텐츠를 에뮬레이트하는 웹 브라우저 및 사진 앨범이 나 뷰포트 또는 디스플레이에 맞게 동적으로 regrouped 수 있는 항목의 논리적 그룹화를 포함 하는 응용 프로그램에 유용 합니다.

다음 다이어그램에서는 특정 지점으로 이동 하 고 해제 하는 방법을 보여 줍니다. 그러면 콘텐츠가 논리적 위치로 자동으로 이동 합니다.

:::row:::
   :::column:::
      ![pannable 영역을 표시 하는 이미지입니다.](images/ux-panning-snap1.png)

      이동 하 여 이동 합니다.
   :::column-end:::
   :::column:::
      ![왼쪽으로 따라 이동 되는 pannable 영역을 보여 주는 이미지입니다.](images/ux-panning-snap2.png)

      터치 접점을 올립니다.
   :::column-end:::
   :::column:::
      ![논리적 스냅 지점에서의 이동이 중지 된 pannable 영역을 보여 주는 이미지입니다.](images/ux-panning-snap3.png)

      Pannable 지역은 접촉이 리프트 된 위치가 아니라 맞춤 지점에서 중지 됩니다.
   :::column-end:::
:::row-end:::

**레일** 콘텐츠는 디스플레이 장치의 크기 및 해상도 보다 넓거나 클 수 있습니다. 이러한 이유로 2 차원 패닝 (가로 및 세로)은 종종 필요 합니다. 레일 동작 축의 움직임 (세로 또는 가로)을 강조 하 여 이러한 경우 사용자 환경을 개선 합니다.

다음 다이어그램에서는 레일의 개념을 보여 줍니다.

![이동을 제한 하는 레일의 화면 다이어그램](images/ux-panning-rails.png)

**포함 되거나 중첩 된 콘텐츠 연결**

사용자가 다른 확대/축소 가능 또는 스크롤할 수 있는 요소 내에 중첩 된 요소에 대 한 확대/축소 또는 스크롤 제한을 적중 한 후에는 부모 요소가 해당 자식 요소에서 확대/축소 또는 스크롤 작업을 시작 해야 하는지 여부를 지정할 수 있습니다. 이를 확대/축소 또는 스크롤 체인 이라고 합니다.

연결은 하나 이상의 단일 축 또는 자유형 이동 지역이 포함 된 단일 축 콘텐츠 영역 내에서 이동 하는 데 사용 됩니다 (터치 contact가 이러한 자식 영역 중 하나에 있는 경우). 특정 방향으로 자식 영역의 패닝 경계에 도달 하면 부모 지역에서 동일한 방향으로 이동이 활성화 됩니다.

Pannable 지역이 다른 pannable 지역 안에 중첩 된 경우 컨테이너와 포함 된 콘텐츠 사이에 충분 한 공간을 지정 하는 것이 중요 합니다. 다음 다이어그램에서는 하나의 pannable 지역이 서로 수직 방향으로 이동 하는 다른 pannable 지역 내에 배치 됩니다. 사용자가 각 지역에서 이동할 수 있는 공간이 많습니다.

![포함 된 pannable 영역을 보여 주는 이미지입니다.](images/scrolling-embedded.png)

다음 다이어그램과 같이 공간이 충분 하지 않은 경우 포함 된 pannable 지역은 컨테이너의 이동을 방해할 수 있으며 하나 이상의 pannable 지역에서 의도 하지 않은 이동이 발생 합니다.

![포함 된 pannable 영역에 대 한 패딩 부족을 보여 주는 이미지입니다.](images/ux-panning-embedded-wrong.png)

이 지침은 앨범 (이전 또는 다음 이미지) 또는 세부 정보 영역에서 단일 축 이동을 지 원하는 한편 개별 이미지 또는 지도 내에서 제한 되지 않은 이동을 지 원하는 사진 앨범 또는 앱 매핑과 같은 앱에도 유용 합니다. 자유형 패닝 이미지 또는 맵에 해당 하는 세부 정보 또는 옵션 영역을 제공 하는 앱에서는 이미지 또는 맵의 제한 되지 않은 이동 영역이 세부 정보 영역으로 이동 하는 것을 방해할 수 있으므로 페이지 레이아웃을 세부 정보 및 옵션 영역으로 시작 하는 것이 좋습니다.

## <a name="related-articles"></a>관련된 문서

- [사용자 지정 사용자 조작](../layout/index.md)
- [ListView 및 GridView 최적화](../../debug-test-perf/optimize-gridview-and-listview.md)
- [키보드 접근성](../accessibility/keyboard-accessibility.md)

**샘플**
- [기본 입력 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [짧은 대기 시간 입력 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [사용자 상호 작용 모드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [포커스 화면 효과 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

**보관 샘플**
- [Input: XAML 사용자 입력 이벤트 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [입력: 장치 기능 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [입력: 터치 적중 테스트 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [XAML 스크롤, 패닝 및 확대/축소 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [입력: 간소화 된 잉크 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [입력: Windows 8 제스처 샘플](/samples/browse/?redirectedfrom=MSDN-samples)
- [Input: 조작 및 제스처 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [DirectX touch 입력 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
