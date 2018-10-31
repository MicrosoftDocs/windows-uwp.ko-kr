---
author: Karl-Bridge-Microsoft
Description: This topic describes Windows zooming and resizing elements and provides user experience guidelines for using these interaction mechanisms in your apps.
title: 광학 줌 및 크기 조정에 대한 지침
ms.assetid: 51a0007c-8a5d-4c44-ac9f-bbbf092b8a00
label: Optical zoom and resizing
template: detail.hbs
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f1643638eaf7eb625defe1f25b44cae20faf0a5c
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5869131"
---
# <a name="optical-zoom-and-resizing"></a>광학 줌 및 크기 조정



이 문서에서는 Windows 확대/축소 및 크기 조정 요소에 대해 설명하고, 앱에서 이러한 조작 메커니즘 사용을 위한 사용자 환경 지침을 제공합니다.

> **중요 API**: [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084), [**Input (XAML)**](https://msdn.microsoft.com/library/windows/apps/br227994)

광학 줌을 사용하면 콘텐츠 영역 내에서 콘텐츠 보기를 확대할 수 있지만(콘텐츠 영역 자체에 대해 수행) 크기 조정을 사용하면 콘텐츠 영역 보기는 바뀌지 않으면서 콘텐츠 영역 내에서 하나 이상 개체의 상대 크기를 변경할 수 있습니다(콘텐츠 영역 내 개체에 대해 수행).

광학 줌 및 크기 조정 조작은 손가락 모으기 및 확대 제스처(손가락을 멀리 벌려 확대하고 가깝게 모아 축소)를 통해 또는 Ctrl 키를 누른 채로 마우스 스크롤 휠을 스크롤하거나 Ctrl 키(숫자 키패드를 사용할 수 없는 경우 Shift 키)를 누르고 더하기(+) 또는 빼기(-) 키를 눌러 수행합니다.

다음 다이어그램은 크기 조정과 광학 줌 간의 차이점을 보여 줍니다.

**광학 줌**: 사용자가 영역을 선택한 다음 전체 영역으로 확대/축소합니다.

![콘텐츠 영역에서 손가락을 모아서 확대 및 손가락을 벌려서 축소](images/areazoom.png)

**크기 조정**: 영역 내에게 개체를 선택하고 해당 개체의 크기를 조정합니다.

![손가락을 모아서 개체를 축소하고 벌려서 확대](images/objectresize.png)

**참고**  광학 줌 [시맨틱 줌](../controls-and-patterns/semantic-zoom.md)과 혼동 하지 않아야 합니다. 두 조작에 모두 동일한 제스처가 사용되지만 시맨틱 줌은 단일 보기(예: 컴퓨터의 폴더 구조, 문서 라이브러리 또는 사진 앨범) 내에 구성된 콘텐츠의 표시와 탐색을 나타냅니다.

 

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항


크기 조정 및 광학 줌을 지원하는 앱에 대해 다음 지침을 사용하세요.

-   최대 및 최소 크기 제약 조건이나 경계가 정의된 경우 사용자가 해당 경계에 도달하면 표시할 시각적 피드백을 사용합니다.
-   끌기 지점을 사용하여 조작을 중지할 논리 지점을 제공함으로써 확대/축소 및 크기 조정 동작에 영향을 주고 특정 콘텐츠 부분이 뷰포트에 표시되도록 합니다. 사용자가 확대/축소 수준 또는 논리적 보기를 보다 쉽게 선택할 수 있도록 해당 수준에 대해 끌기 지점을 제공합니다. 예를 들어 사진 앱은 크기 조정 끌기 지점을 100%로 제공할 수 있고, 지도 앱의 경우 시/군/구, 시/도 및 국가 보기에서 끌기 지점이 유용할 수 있습니다.

    끌기 지점은 부정확한 측면이 있지만 목표에 도달할 수 있도록 합니다. XAML을 사용하는 경우 [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527)의 끌기 지점 속성을 참조하세요. JavaScript 및 HTML의 경우 [**-ms-content-zoom-snap-points**](https://msdn.microsoft.com/library/hh771895)를 참조하세요.

    다음 두 가지 끌기 지점 유형이 있습니다.

    -   근접 - 접촉을 뗀 후 끌기 지점의 거리 임계값 내에서 관성이 멈추면 끌기 지점이 선택됩니다. 근접 끌기 지점을 사용할 경우 끌기 지점 사이에서 확대/축소 또는 크기 조정을 끝낼 수 있습니다.
    -   필수 - 제스처의 방향과 속도에 따라 접촉을 떼기 전에 마지막으로 교차한 끌기 지점 바로 앞이나 뒤에 있는 끌기 지점이 선택됩니다. 필수 끌기 지점에서 조작을 끝내야 합니다.
-   관성 물리학을 사용합니다. 여기에는 다음과 같은 동작이 포함됩니다.
    -   감속: 사용자가 손가락 모으기나 확대를 중지할 때 발생합니다. 이 동작은 매끄러운 표면을 미끄러지다가 멈추는 것과 비슷합니다.
    -   바운스: 크기 제약 조건이나 경계를 지날 때 약간 뒤로 바운스되는 효과가 발생합니다.
-   [타기팅에 대한 지침](guidelines-for-targeting.md)에 따라 컨트롤 간격을 조정합니다.
-   제한된 크기 조정을 위해 크기 조정 핸들을 제공합니다. 핸들이 지정되지 않은 경우 등각 또는 비례식 크기 조정이 기본 방식입니다.
-   확대/축소를 사용하여 UI를 탐색하거나 앱 내에 추가 컨트롤을 노출하지 마세요. 대신 이동 영역을 사용합니다. 이동에 대한 자세한 내용은 [이동에 대한 지침](guidelines-for-panning.md)을 참조하세요.
-   크기 조정 가능한 콘텐츠 영역 내에 크기 조정 가능한 개체를 넣지 마세요. 다음과 같은 경우는 예외입니다.
    -   크기 조정이 가능한 항목이 크기 조정이 가능한 캔버스나 아트 보드에 표시될 수 있는 그리기 응용 프로그램
    -   맵과 같은 포함 개체가 있는 웹 페이지

    **참고**  콘텐츠 영역의 모든 터치 지점이 크기 조정이 가능한 개체 안에 있는 경우가 아니면 항상에서 조정 합니다.

     

## <a name="related-articles"></a>관련 문서


**샘플**
* [기본 입력 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [짧은 대기 시간 입력 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [사용자 조작 모드 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [포커스 화면 효과 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**보관 샘플**
* [입력: XAML 사용자 입력 이벤트 샘플](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [입력: 디바이스 기능 샘플](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [입력: 터치 적중 횟수 테스트 샘플](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML 스크롤, 이동 및 확대/축소 샘플](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [입력: 간단한 잉크 샘플](http://go.microsoft.com/fwlink/p/?linkid=246570)
* [입력: Windows 8 제스처 샘플](http://go.microsoft.com/fwlink/p/?LinkId=264995)
* [입력: 조작 및 제스처(C++) 샘플](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [DirectX 터치 입력 샘플](http://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 




