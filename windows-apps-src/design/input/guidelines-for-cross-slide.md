---
Description: Use cross-slide to support selection with the swipe gesture and drag (move) interactions with the slide gesture.
title: 교차 방향으로 밀기에 대한 지침
ms.assetid: 897555e2-c567-4bbe-b600-553daeb223d5
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b2d402bca61fc271b6d1e2e972cca280693f9ce3
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/04/2019
ms.locfileid: "9045196"
---
# <a name="guidelines-for-cross-slide"></a>교차 방향으로 밀기에 대한 지침




**중요 API**

-   [**CrossSliding**](https://msdn.microsoft.com/library/windows/apps/br241942)
-   [**CrossSlideThresholds**](https://msdn.microsoft.com/library/windows/apps/br241941)
-   [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)

교차 방향으로 밀기를 사용하면 살짝 밀기 제스처를 이용한 항목 선택과 밀기 제스처를 이용한 끌기(이동) 조작을 지원할 수 있습니다.

## <a name="span-iddosanddontsspanspan-iddosanddontsspanspan-iddosanddontsspandos-and-donts"></a><span id="Dos_and_don_ts"></span><span id="dos_and_don_ts"></span><span id="DOS_AND_DON_TS"></span>권장 사항 및 금지 사항


-   단일 방향으로 스크롤되는 목록이나 모음에는 교차 방향으로 밀기를 사용합니다.
-   탭 조작이 다른 용도로 사용되는 경우 항목 선택에 교차 방향으로 밀기를 사용합니다.
-   큐에 항목을 추가할 때는 교차 방향으로 밀기를 사용하지 마세요.

## <a name="span-idadditionalusageguidancespanspan-idadditionalusageguidancespanspan-idadditionalusageguidancespanadditional-usage-guidance"></a><span id="Additional_usage_guidance"></span><span id="additional_usage_guidance"></span><span id="ADDITIONAL_USAGE_GUIDANCE"></span>추가 사용법 지침


선택 및 끌기는 한 방향(세로 또는 가로)으로 이동 가능한 콘텐츠 영역 내에서만 사용할 수 있습니다. 다른 조작이 작동하려면 하나의 이동 방향을 잠그고 이동 방향에 수직으로 제스처를 수행해야 합니다.

여기서는 가로질러 밀기를 사용한 객체 선택 및 끌기를 보여 줍니다. 왼쪽의 이미지는 살짝 밀기 제스처가 거리 임계값에 도달하지 못한 상태에서 손가락을 떼고 개체를 놓을 경우 항목이 선택되는 방식을 보여 줍니다. 오른쪽의 이미지는 밀기 제스처가 거리 임계값에 도달하여 개체가 끌리는 방식을 보여 줍니다.

![선택 및 끌어서 놓기 프로세스를 보여 주는 다이어그램](images/crossslide-mechanism.png)

다음 다이어그램에는 가로질러 밀기 조작에 사용되는 거리 임계값이 나와 있습니다.

![선택 및 끌어서 놓기 프로세스를 보여 주는 스크린샷](images/crossslide-threshold.png)

이동 기능을 유지하려면 2.7mm(목표 해상도로 약 10픽셀)의 작은 임계값에 도달한 이후에 선택 또는 끌기 조작이 활성화되어야 합니다. 이와 같이 작은 임계값을 사용하면 시스템이 가로질러 밀기와 이동 간을 보다 잘 구분할 수 있으며 탭하기 동작이 가로질러 밀기 및 이동과 쉽게 구분될 수 있습니다.

이 이미지는 사용자가 UI의 요소를 터치하지만 접촉 시 손가락을 약간 아래로 이동하는 방법을 보여 줍니다. 임계값이 지정되지 않은 경우 이 조작은 초기 수직 이동 때문에 가로질러 밀기로 해석됩니다. 임계값이 지정되면 동작은 가로 이동으로 올바르게 해석됩니다.

![선택 또는 끌어서 놓기 명확성 임계값을 보여 주는 스크린샷](images/crossslide-threshold2.png)

다음은 가로질러 밀기 기능을 앱에 포함할 때 고려해야 하는 몇 가지 지침입니다.

단일 방향으로 스크롤되는 목록이나 모음에는 교차 방향으로 밀기를 사용합니다. 자세한 내용은 [ListView 컨트롤 추가](https://msdn.microsoft.com/library/windows/apps/hh465382)를 참조하세요.

**참고**콘텐츠 영역 웹 브라우저나 전자 판독기와 같이 두 방향으로 이동할 수 있는 경우에서 시간이 제한 된 길게와 상호 작용 이미지 및 하이퍼링크와 같은 개체에 대 한 상황에 맞는 메뉴를 호출 사용 해야 합니다.

 

|                                                                                         |                                                                                         |
|-----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| ![가로 이동 2차원 목록](images/groupedlistview1.png)                | ![세로 이동 1차원 목록](images/listviewlistlayout.png)                |
| 가로 이동 2차원 목록 세로로 끌어 항목을 선택하거나 이동합니다. | 세로 이동 1차원 목록 가로로 끌어 항목을 선택하거나 이동합니다. |

 

### <span id="selection"></span><span id="SELECTION"></span>

**선택**

선택이란 하나 이상의 개체를 시작하거나 활성화하지 않고 단순히 표시하는 것입니다. 이 동작은 하나 이상의 개체를 마우스로 한 번 클릭하거나 Shift 키를 누른 채로 클릭하는 것과 비슷합니다.

가로질러 밀기 선택은 요소를 터치하고 짧게 끈 다음 놓으면 됩니다. 이 선택 방법을 사용하면 전용 선택 모드와 다른 터치 인터페이스에 요구되는 시간이 제한된 누르고 있기 조작이 필요하지 않으며 활성화를 위한 탭하기 조작과도 충돌하지 않습니다.

가로질러 밀기 선택 방법은 다음 다이어그램과 같이 거리 임계값 외에 90° 임계 영역으로 제한됩니다. 개체를 이 영역 밖으로 끌면 선택되지 않습니다.

![선택 임계 영역을 보여 주는 다이어그램](images/crossslide-selection.png)

가로질러 밀기 조작은 "자체 노출" 조작이라고도 하는 시간이 제한된 누르고 있기 조작으로 보완됩니다. 이 보완적 조작은 개체에 대해 수행할 수 있는 동작을 나타내는 애니메이션을 활성화합니다. 명확성 UI에 대한 자세한 내용은 [시각적 피드백에 대한 지침](guidelines-for-visualfeedback.md)을 참조하세요.

다음 스크린샷은 자체 노출 애니메이션이 작동하는 방식을 보여 줍니다.

1.  자체 노출 조작 애니메이션을 시작하려면 길게 누릅니다. 항목의 선택된 상태는 애니메이션에 의해 노출되는 사항에 영향을 줍니다. 선택 취소된 경우 확인 표시가 노출되고 선택된 경우 확인 표시가 노출되지 않습니다.

    ![선택되지 않은 상태를 보여 주는 스크린샷](images/crossslide-selfreveal1.png)

2.  살짝 밀기 제스처(위쪽 또는 아래쪽)를 사용하여 항목을 선택합니다.

    ![선택 애니메이션을 보여 주는 스크린샷](images/crossslide-selfreveal2.png)

3.  이제 항목이 선택되었습니다. 밀기 제스처를 사용한 선택 동작을 재정의하여 항목을 이동합니다.

    ![끌어서 놓기 애니메이션을 보여 주는 스크린샷](images/crossslide-selfreveal3.png)

유일한 기본 동작인 경우 응용 프로그램에서 한 번 탭하여 선택을 수행합니다. 가로질러 밀기 자체 노출 애니메이션은 이러한 표준 탭하기 조작의 기능을 명확히 보여 줌으로써 활성화 및 탐색에 도움을 주기 위해 표시됩니다.

**선택 바스켓**

선택 바스켓은 응용 프로그램의 기본 목록이나 모음에서 선택된 항목에 대한 시각적으로 고유한 동적 표현입니다. 이 기능은 선택한 항목을 추적하는 데 유용하며, 다음이 가능한 응용 프로그램에서 사용해야 합니다.

-   여러 위치에서 항목을 선택할 수 있습니다.
-   많은 항목을 선택할 수 있습니다.
-   선택 목록에 따라 동작이나 명령이 달라집니다.

선택 바스켓의 콘텐츠는 다른 동작 및 명령을 수행해도 계속 유지됩니다. 예를 들어 갤러리에서 여러 장의 사진을 선택하고 각 사진의 색을 보정하고 사진을 공유해도 항목이 선택된 상태로 유지됩니다.

응용 프로그램에서 선택 바스켓이 사용되지 않으면 동작이나 명령이 수행된 후에 현재 선택 내용이 지워집니다. 예를 들어 재생 목록에서 음악을 선택하고 등급을 매기면 선택 내용이 지워집니다.

선택 바스켓이 사용되지 않고 목록이나 모음의 다른 항목이 활성화될 때도 현재 선택 내용이 지워집니다. 예를 들어 받은 편지함 메시지를 선택하면 미리 보기 창이 업데이트됩니다. 그런 후 또 다른 받은 편지함 메시지를 선택하면 이전 메시지 선택이 취소되고 미리 보기 창이 업데이트됩니다.

**큐**

큐는 선택 바스켓 목록과 다르므로 별도로 취급해야 합니다. 주요 차이점은 다음과 같습니다.

-   선택 바스켓의 항목 목록은 시각적인 표현일 뿐이지만 큐의 항목은 특정 동작을 염두에 두고 어셈블되어 있습니다.
-   선택 바스켓에서는 항목이 한 번만 표시되지만 큐에서는 여러 번 표시될 수 있습니다.
-   선택 바스켓의 항목 순서는 선택 순서를 나타냅니다. 큐의 항목 순서는 기능과 직접적으로 관련되어 있습니다.

이러한 이유로 큐에 항목을 추가하는 데는 가로질러 밀기 선택 조작을 사용하면 안 됩니다. 대신 항목을 큐에 추가할 때는 끌기 동작을 사용해야 합니다.

### <span id="draganddrop"></span><span id="DRAGANDDROP"></span>

**끌기**

끌기를 사용하여 하나 이상의 개체를 한 위치에서 다른 위치로 이동할 수 있습니다.

여러 개체를 이동해야 하는 경우 사용자가 여러 항목을 선택한 다음 모든 항목을 한 번에 끌 수 있게 합니다.

## <a name="span-idrelatedtopicsspanrelated-articles"></a><span id="related_topics"></span>관련 문서


**샘플**
* [기본 입력 샘플](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [짧은 대기 시간 입력 샘플](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [사용자 조작 모드 샘플](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [포커스 화면 효과 샘플](https://go.microsoft.com/fwlink/p/?LinkID=619895)
**보관 샘플**
* [입력: XAML 사용자 입력 이벤트 샘플](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [입력: 디바이스 기능 샘플](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [입력: 터치 적중 횟수 테스트 샘플](https://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML 스크롤, 이동 및 확대/축소 샘플](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [입력: 간단한 잉크 샘플](https://go.microsoft.com/fwlink/p/?linkid=246570)
* [입력: Windows 8 제스처 샘플](https://go.microsoft.com/fwlink/p/?LinkId=264995)
* [입력: 조작 및 제스처(C++) 샘플](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [DirectX 터치 입력 샘플](https://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 




