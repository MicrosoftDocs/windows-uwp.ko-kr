---
Description: 밀기 제스처를 사용 하 여 선택 영역을 지원 하 고 슬라이드 제스처를 사용 하 여 상호 작용을 끄는 (이동) 할 수 있습니다.
title: 슬라이드 간 지침
ms.assetid: 897555e2-c567-4bbe-b600-553daeb223d5
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e5f86da29900e0ef83fb0bf41d2c8d9fe59727f2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172517"
---
# <a name="guidelines-for-cross-slide"></a>슬라이드 간 지침




**중요 API**

-   [**상대 슬라이딩**](/uwp/api/windows.ui.input.gesturerecognizer.crosssliding)
-   [**CrossSlideThresholds**](/uwp/api/windows.ui.input.gesturerecognizer.crossslidethresholds)
-   [**Windows.UI.Input**](/uwp/api/Windows.UI.Input)

밀기 제스처를 사용 하 여 선택 영역을 지원 하 고 슬라이드 제스처를 사용 하 여 상호 작용을 끄는 (이동) 할 수 있습니다.

## <a name="span-iddos_and_don_tsspanspan-iddos_and_don_tsspanspan-iddos_and_don_tsspandos-and-donts"></a><span id="Dos_and_don_ts"></span><span id="dos_and_don_ts"></span><span id="DOS_AND_DON_TS"></span>Dos 및 일과


-   단일 방향으로 스크롤 하는 목록 또는 컬렉션에 대해 교차 슬라이드를 사용 합니다.
-   탭 상호 작용을 다른 용도로 사용 하는 경우 항목을 선택할 때 교차 슬라이드를 사용 합니다.
-   큐에 항목을 추가 하는 데는 슬라이드 간을 사용 하지 마세요.

## <a name="span-idadditional_usage_guidancespanspan-idadditional_usage_guidancespanspan-idadditional_usage_guidancespanadditional-usage-guidance"></a><span id="Additional_usage_guidance"></span><span id="additional_usage_guidance"></span><span id="ADDITIONAL_USAGE_GUIDANCE"></span>추가 사용 지침


선택 및 끌기는 한 방향 (세로 또는 가로)으로 pannable 콘텐츠 영역 내 에서만 가능 합니다. 상호 작용이 작동 하려면 하나의 패닝 방향이 잠기고 제스처를 패닝 방향에 수직인 방향으로 수행 해야 합니다.

여기서는 크로스 슬라이드를 사용 하 여 개체를 선택 하 고 끄는 방법을 보여 줍니다. 왼쪽 이미지는 대화가 리프트 되 고 개체가 해제 되기 전에 살짝 밀기 제스처가 거리 임계값을 초과 하지 않는 경우 항목이 선택 되는 방법을 보여 줍니다. 오른쪽의 이미지는 거리 임계값을 교차 하는 슬라이딩 제스처를 표시 하 고 개체를 끌고 있습니다.

![선택 및 끌어서 놓기 프로세스를 보여 주는 다이어그램입니다.](images/crossslide-mechanism.png)

슬라이드 간 상호 작용에 사용 되는 임계값 거리가 다음 다이어그램에 표시 됩니다.

![선택 및 끌어서 놓기 프로세스를 보여 주는 스크린샷](images/crossslide-threshold.png)

패닝 기능을 유지 하려면 선택 또는 끌기 상호 작용이 활성화 되기 전에 작은 임계값 2.7 mm (대상 해상도의 약 10 픽셀)을 초과 해야 합니다. 이 작은 임계값은 시스템에서 교차 슬라이딩를 구분 하는 데 도움이 되 고 상호 슬라이딩 및 패닝 모두에서 탭 제스처를 구분 하는 데도 도움이 됩니다.

이 이미지는 사용자가 UI의 요소에 연결 하는 방법을 보여 주지만 연락처에서 손가락을 약간 아래로 이동 합니다. 임계값을 사용 하지 않으면 초기 세로 이동으로 인해 상호 작용으로 해석 됩니다. 임계값을 사용 하면 이동이 수평 패닝으로 올바르게 해석 됩니다.

![선택 또는 끌어서 놓기 명확성 임계값을 보여 주는 스크린샷](images/crossslide-threshold2.png)

앱에 슬라이드 간 기능을 포함 하는 경우 고려해 야 할 몇 가지 지침은 다음과 같습니다.

단일 방향으로 스크롤 하는 목록 또는 컬렉션에 대해 교차 슬라이드를 사용 합니다. 자세한 내용은 [ListView 컨트롤 추가](/previous-versions/windows/apps/hh465382(v=win.10))를 참조 하세요.

**참고**    콘텐츠 영역을 두 방향 (예: 웹 브라우저 또는 e-learning)으로 따라 이동 수 있는 경우에는 이미지 및 하이퍼링크와 같은 개체에 대 한 상황에 맞는 메뉴를 호출 하기 위해 누르고 있는 시간 제한의 상호 작용을 사용 해야 합니다.

 

|                                                                                         |                                                                                         |
|-----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| ![가로 패닝, 2 차원 목록](images/groupedlistview1.png)                | ![수직 패닝, 1 차원 목록](images/listviewlistlayout.png)                |
| 2 차원 가로로 패닝 된 목록입니다. 항목을 선택 하거나 이동 하려면 세로를 끕니다. | 1 차원 목록을 세로로 패닝 합니다. 항목을 선택 하거나 이동 하려면 가로를 끕니다. |

 

### <span id="selection"></span><span id="SELECTION"></span>

**선택**

선택은 하나 이상의 개체를 시작 하거나 활성화 하지 않고 표시 하는 것입니다. 이 작업은 하나 이상의 개체에서 마우스를 한 번 클릭 하거나 Shift 키와 마우스를 클릭 하는 것과 유사 합니다.

슬라이드 간 선택은 요소를 터치 하 고 짧은 끌기 상호 작용 후 해제 하 여 수행 됩니다. 이 선택 방법은 전용 선택 모드와 다른 터치 인터페이스에서 요구 하는 누르고 유지 시간 제한 상호 작용을 모두 분배 활성화를 위한 탭 상호 작용과 충돌 하지 않습니다.

다음 다이어그램에 표시 된 것 처럼 거리 임계값 외에도 교차 슬라이드 선택은 90 ° 임계값 영역으로 제한 됩니다. 개체를이 영역 밖으로 끌면 선택 되지 않습니다.

![선택 임계값 영역을 보여 주는 다이어그램입니다.](images/crossslide-selection.png)

크로스 슬라이드 상호 작용은 "자체 노출" 상호 작용이 라고도 하는 누르고 유지 시간 제한 상호 작용을 통해 보완 됩니다. 이 추가 상호 작용은 개체에 대해 수행할 수 있는 작업을 나타내는 애니메이션을 활성화 합니다. 명확성 UI에 대 한 자세한 내용은 [시각적 피드백에 대 한 지침](guidelines-for-visualfeedback.md)을 참조 하세요.

다음 스크린샷은 자체 노출 애니메이션의 작동 방식을 보여 줍니다.

1.  누르고 있으면 자체를 드러내는 상호 작용에 대 한 애니메이션을 시작 합니다. 항목의 선택 된 상태는 애니메이션에 의해 표시 되는 내용에 영향을 줍니다. 선택 하지 않은 경우 확인 표시가 선택 되 고 선택 하면 확인 표시가 표시 되지 않습니다.

    ![선택 되지 않은 상태를 보여 주는 스크린샷](images/crossslide-selfreveal1.png)

2.  살짝 밀기 제스처(위쪽 또는 아래쪽)를 사용하여 항목을 선택합니다.

    ![선택 항목에 대 한 애니메이션을 보여 주는 스크린샷](images/crossslide-selfreveal2.png)

3.  이제 항목이 선택 됩니다. 항목을 이동 하려면 슬라이드 제스처를 사용 하 여 선택 동작을 재정의 합니다.

    ![끌어서 놓기를 위한 애니메이션을 보여 주는 스크린샷](images/crossslide-selfreveal3.png)

유일한 기본 작업 인 응용 프로그램에서 한 번 탭을 사용 하 여 선택 합니다. 활성화 및 탐색을 위한 표준 탭 상호 작용에서이 기능을 명확 하 게 구분 하기 위해 슬라이드 간 자체 노출 애니메이션이 표시 됩니다.

**선택 바구니**

선택 바구니는 응용 프로그램의 기본 목록 또는 컬렉션에서 선택 된 항목을 시각적으로 구분 하 고 동적으로 표현한 것입니다. 이 기능은 선택한 항목을 추적 하는 데 유용 하며, 다음과 같은 응용 프로그램에서 사용 해야 합니다.

-   여러 위치에서 항목을 선택할 수 있습니다.
-   여러 항목을 선택할 수 있습니다.
-   작업 또는 명령은 선택 목록에 의존 합니다.

선택 바구니의 콘텐츠는 작업 및 명령에서 지속 됩니다. 예를 들어 갤러리에서 일련의 사진을 선택 하 고 각 사진에 색 교정을 적용 하 고 사진을 공유 하는 경우 항목은 선택 된 상태로 유지 됩니다.

응용 프로그램에서 선택 바구니를 사용 하지 않는 경우 작업 또는 명령 후에 현재 선택 항목을 지워야 합니다. 예를 들어 재생 목록에서 노래를 선택 하 고 등급을 지정 하는 경우 선택을 취소 해야 합니다.

선택 바구니를 사용 하지 않고 목록 또는 컬렉션의 다른 항목을 활성화 한 경우에도 현재 선택이 지워집니다. 예를 들어 받은 편지함 메시지를 선택 하면 미리 보기 창이 업데이트 됩니다. 그런 다음 두 번째 수신함 메시지를 선택 하면 이전 메시지의 선택이 취소 되 고 미리 보기 창이 업데이트 됩니다.

**큐**

큐는 선택 바구니 목록과 동일 하지 않으므로 처리 하지 않아야 합니다. 주요 차이점은 다음과 같습니다.

-   선택 바구니의 항목 목록은 시각적 표현입니다. 큐의 항목은 특정 동작을 염두에 두어야 합니다.
-   항목은 선택 바구니에서 한 번만 표시 될 수 있지만 큐에서 여러 번 표시 될 수 있습니다.
-   선택 바구니에 있는 항목의 순서는 선택 순서를 나타냅니다. 큐에 있는 항목의 순서는 기능과 직접적으로 관련이 있습니다.

이러한 이유로, 항목을 큐에 추가 하는 데는 슬라이드 간 선택 상호 작용을 사용 하면 안 됩니다. 대신 끌기 작업을 통해 큐에 항목을 추가 해야 합니다.

### <span id="draganddrop"></span><span id="DRAGANDDROP"></span>

**끌기**

끌기를 사용 하 여 하나 이상의 개체를 한 위치에서 다른 위치로 이동 합니다.

둘 이상의 개체를 이동 해야 하는 경우에는 사용자가 여러 항목을 선택한 다음 모든 항목을 한 번에 끌어 놓습니다.

## <a name="related-articles"></a>관련된 문서

### <a name="samples"></a>샘플

- [기본 입력 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [짧은 대기 시간 입력 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [사용자 상호 작용 모드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [포커스 화면 효과 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>보관 샘플

- [Input: XAML 사용자 입력 이벤트 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [입력: 장치 기능 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [입력: 터치 적중 테스트 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [XAML 스크롤, 패닝 및 확대/축소 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [입력: 간소화 된 잉크 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [입력: Windows 8 제스처 샘플](/samples/browse/?redirectedfrom=MSDN-samples)
- [Input: 조작 및 제스처 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [DirectX touch 입력 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
 

 