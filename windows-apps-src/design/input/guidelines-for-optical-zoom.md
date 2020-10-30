---
description: 이 항목에서는 창 확대/축소 및 크기 조정에 대해 설명 하 고 앱에서 이러한 상호 작용 메커니즘을 사용 하기 위한 사용자 환경 지침을 제공 합니다.
title: 광학 확대/축소 및 크기 조정 지침
ms.assetid: 51a0007c-8a5d-4c44-ac9f-bbbf092b8a00
label: Optical zoom and resizing
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1cf62546efd95c3a4d26ad3ca6f16990b832611c
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93035116"
---
# <a name="optical-zoom-and-resizing"></a>광학 줌 및 크기 조정



이 문서에서는 Windows 확대/축소 및 크기 조정 요소를 설명 하 고 앱에서 이러한 상호 작용 메커니즘을 사용 하기 위한 사용자 환경 지침을 제공 합니다.

> **중요 한 api** : [**Windows. input**](/uwp/api/Windows.UI.Input), [**input (XAML)**](/uwp/api/Windows.UI.Xaml.Input)

사용자는 광학 확대/축소를 사용 하 여 콘텐츠 영역 내에서 콘텐츠 보기를 확대할 수 있습니다 (콘텐츠 영역 자체에서 수행 됨). 반면 크기 조정을 사용 하면 콘텐츠 영역의 뷰를 변경 하지 않고 하나 이상의 개체에 대 한 상대 크기를 변경할 수 있습니다 (콘텐츠 영역 내의 개체에서 수행 됨).

확대/축소 및 크기 조정 상호 작용은 모두 바깥쪽 및 늘이기 제스처를 통해 수행 됩니다. 즉, 이동 하는 손가락을 확대 하 고 확대 하 여 더 가깝게 이동 하거나 ctrl 키를 누른 상태에서 마우스 스크롤 휠을 스크롤하거나 Ctrl 키를 누른 상태에서 Shift 키를 누른 채 (숫자 키패드를 사용할 수 없는 경우) 더하기 (+) 또는 빼기 (-) 키를 누릅니다.

다음 다이어그램에서는 크기 조정 및 광학 확대/축소의 차이점을 보여 줍니다.

**광학 확대/축소** : 사용자는 영역을 선택 하 고 전체 영역을 확대 합니다.

![손가락을 가깝게 이동 하면 콘텐츠 영역을 확대 하 고 별도로 이동 하면 축소 됩니다.](images/areazoom.png)

**크기 조정** : 사용자가 영역 내에서 개체를 선택 하 고 해당 개체의 크기를 조정 합니다.

![손가락을 가깝게 이동 하면 개체를 축소 하 고 서로를 분리 하 여 이동 합니다.](images/objectresize.png)

**참고**  
시각적 확대/축소는 [의미 체계 확대/](../controls-and-patterns/semantic-zoom.md)축소와 혼동 해서는 안 됩니다. 동일 제스처는 두 상호 작용에 모두 사용 되지만, 의미 체계 확대/축소는 단일 뷰 내에서 구성 된 콘텐츠 (예: 컴퓨터의 폴더 구조, 문서 라이브러리 또는 사진 앨범)의 표현과 탐색을 나타냅니다.

 

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항


크기 조정 또는 광학 확대/축소를 지 원하는 앱에 대해 다음 지침을 사용 합니다.

-   최대 및 최소 크기 제약 조건 또는 경계가 정의 된 경우 시각적 피드백을 사용 하 여 사용자가 해당 경계에 도달 하거나이를 초과 하는 경우를 보여 줍니다.
-   맞추기 요소를 사용 하 여 조작을 중지 하 고 특정 콘텐츠의 하위 집합이 뷰포트에 표시 되도록 논리 요소를 제공 하 여 확대/축소 및 크기 조정 동작에 영향을 줍니다. 일반 확대/축소 수준이 나 논리적 보기에 대 한 맞춤 위치를 제공 하 여 사용자가 해당 수준을 더 쉽게 선택할 수 있도록 합니다. 예를 들어 사진 앱은 100%에 크기 조정 맞춤 지점을 제공할 수 있으며, 응용 프로그램을 매핑하는 경우 맞춤 지점은 도시, 시/도 및 국가 보기에서 유용할 수 있습니다.

    맞춤선을 사용 하면 사용자가 정확 하 게 유지 되 고 목표를 달성할 수 있습니다. XAML을 사용 하는 경우 [**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)의 snap points 속성을 참조 하세요. JavaScript 및 HTML의 경우 [**-ms-콘텐츠-확대/축소-맞춤-요소**](/previous-versions/hh771895(v=vs.85))를 사용 합니다.

    다음과 같은 두 가지 유형의 스냅 지점이 있습니다.

    -   근접-연락처가 리프트 된 후에 관성이 맞춤 지점의 거리 임계값 내에서 중지 되 면 맞춤 지점이 선택 됩니다. 근접 한 맞춤 지점은 여전히 확대/축소 또는 크기 조정을 사용 하 여 맞춤 요소 사이를 종료 합니다.
    -   필수-제스처의 방향 및 속도에 따라, 선택 된 스냅 지점은 마지막 스냅 지점을 바로 앞 이나 뒤에 오도록 하는 것입니다. 조작은 반드시 필수 맞춤 지점에서 끝나야 합니다.
-   관성 물리를 사용 합니다. 여기에는 다음이 포함됩니다.
    -   감속: 사용자가 집기를 중지 하거나 확장 하는 경우 발생 합니다. 이는 slippery 표면의 중지를 슬라이딩 하는 것과 비슷합니다.
    -   바운스: 크기 제약 조건 또는 경계가 전달 될 때 약간의 바운스 효과가 발생 합니다.
-   [대상 지정에 대 한 지침](guidelines-for-targeting.md)에 따라 공간 제어.
-   제한 된 크기 조정에 대 한 크기 조정 핸들을 제공 합니다. 핸들을 지정 하지 않으면 기본적으로 크기가 조정 됩니다.
-   확대/축소를 사용 하 여 UI를 탐색 하거나 응용 프로그램 내에서 추가 컨트롤을 노출 하지 마세요. 대신 패닝 영역을 사용 하세요. 이동에 대 한 자세한 내용은 [패닝에 대 한 지침](guidelines-for-panning.md)을 참조 하세요.
-   크기 조정 가능한 개체는 크기 조정 가능한 콘텐츠 영역 내에 배치 하지 마세요. 이에 대 한 예외는 다음과 같습니다.
    -   크기 조정 가능한 항목이 크기 조정 가능한 캔버스 또는 아트 보드에 나타날 수 있는 응용 프로그램을 그립니다.
    -   맵과 같은 포함 된 개체를 포함 하는 웹 페이지입니다.

    **참고**  
    모든 경우에, 모든 터치 포인트가 크기 조정 가능한 개체 내에 있지 않으면 콘텐츠 영역의 크기가 조정 됩니다.

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
