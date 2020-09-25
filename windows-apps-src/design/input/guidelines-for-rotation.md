---
Description: 이 항목에서는 새 Windows UI for rotation에 대해 설명 하 고 Windows 앱에서이 새로운 상호 작용 메커니즘을 사용할 때 고려해 야 하는 사용자 환경 지침을 제공 합니다.
title: 회전
ms.assetid: f098bc05-35b3-46b2-9e9b-9ff292d067ca
label: Rotation
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 135f7773a94491e1e6470c84ad428265273bc79d
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91217006"
---
# <a name="rotation"></a>회전


이 문서에서는 새 Windows UI for rotation에 대해 설명 하 고 Windows 앱에서이 새로운 상호 작용 메커니즘을 사용할 때 고려해 야 하는 사용자 환경 지침을 제공 합니다.

> **중요 한 api**: [**windows**](/uwp/api/Windows.UI.Input). Input, [**windows. .xaml**](/uwp/api/Windows.UI.Xaml.Input)

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항

-   사용자가 UI 요소를 직접 회전할 수 있도록 회전을 사용 합니다.

## <a name="additional-usage-guidance"></a>추가 사용법 지침


**회전 개요**

회전은 사용자가 개체를 원형 (시계 방향 또는 시계 반대 방향)으로 변환할 수 있도록 Windows 앱에서 사용 하는 터치 최적화 기술입니다.

입력 장치에 따라 회전 상호 작용은 다음을 사용 하 여 수행 됩니다.

-   선택한 개체의 회전 그리퍼를 이동 하는 마우스 또는 활성 펜/스타일러스입니다.
-   회전 제스처를 사용 하 여 개체를 원하는 방향으로 전환 하는 터치 또는 수동 펜/스타일러스입니다.

**회전을 사용 하는 경우**

사용자가 UI 요소를 직접 회전할 수 있도록 회전을 사용 합니다. 다음 다이어그램에서는 회전 상호 작용에 대해 지원 되는 몇 가지 손가락 위치를 보여 줍니다.

![회전에서 지 원하는 다양 한 손가락 postures을 보여 주는 다이어그램입니다.](images/ux-rotate-positions.png)

**참고**    사용자가 연결 지점 (예: 그리기 또는 레이아웃 응용 프로그램)과 관련 되지 않은 회전 지점을 지정 하지 않는 한, 대부분의 경우에는 대부분의 경우에 회전 지점은 두 터치 포인트 중 하나입니다. 다음 이미지는 회전 지점이 이런 방식으로 제한 되지 않는 경우 사용자 환경이 저하 될 수 있는 방법을 보여 줍니다.

첫 번째 그림은 초기 (엄지 단추) 및 보조 (인덱스 손가락) 터치 요소를 보여 줍니다. 인덱스 손가락은 트리를 터치 하 고 엄지 단추는 로그를 터치 합니다.

![회전 제스처에 대 한 두 개의 초기 터치 지점이 표시 된 이미지입니다.](images/ux-rotate-points1.png)
이 두 번째 그림에서는 초기 (엄지) 터치 지점을 중심으로 회전이 수행 됩니다. 회전 후에도 인덱스 손가락은 여전히 트리 트렁크에 접촉 하 고 있으며, 엄지 단추는 로그 (회전 지점)에 계속 접촉 하 고 있습니다.

![두 초기 터치 포인트 중 하나로 제한 된 회전 지점을 사용 하 여 회전 된 그림을 보여 주는 이미지입니다.](images/ux-rotate-points2.png)
이 세 번째 그림에서 회전 중심은 응용 프로그램 (또는 사용자가 설정한)에서 그림의 중심점으로 정의 되었습니다. 회전 후에는 사진이 손가락 중 하나를 중심으로 회전 하지 않으므로 사용자가이 설정을 선택 하지 않은 경우 직접 조작의 효과는 중단 됩니다.

![두 초기 터치 점이 아니라 그림의 중심으로 제한 된 회전 지점을 사용 하 여 회전 된 그림을 보여 주는 이미지입니다.](images/ux-rotate-points3.png)
이 마지막 그림에서 회전 중심은 응용 프로그램 (또는 사용자가 설정한)에 의해 그림의 왼쪽 가장자리 가운데에 있는 지점이 되도록 정의 되었습니다. 사용자가이 설정을 선택 하지 않은 경우에도이 경우 직접 조작의 효과는 중단 됩니다.

![두 초기 접촉 지점이 아닌 그림의 가장 왼쪽 중심으로 제한 되는 회전 지점을 표시 하는 이미지입니다.](images/ux-rotate-points4.png)

 

Windows 10은 사용 가능, 제한 및 결합의 세 가지 회전 유형을 지원 합니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Type</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">무료 회전</td>
<td align="left"><p>무료 회전을 사용 하면 사용자가 360 수준 원호의 어디에서 나 자유롭게 콘텐츠를 회전할 수 있습니다. 사용자가 개체를 해제할 때 개체는 선택한 위치에 유지 됩니다. 무료 회전은 Microsoft PowerPoint, Word, Visio 및 그림판과 같은 응용 프로그램의 그리기 및 레이아웃에 유용 합니다. 및 Adobe Photoshop, Illustrator 및 Flash가 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left">제한 된 회전</td>
<td align="left"><p>제한 된 회전은 조작 중에는 자유 회전을 지원 하지만 릴리스 시 90 수준 증분 (0, 90, 180 및 270)에는 맞춤 지점을 적용 합니다. 사용자가 개체를 놓으면 개체가 가장 가까운 맞춤 지점으로 자동으로 회전 합니다.</p>
<p>제한 된 회전은 가장 일반적인 회전 방법으로, 콘텐츠를 스크롤 하는 것과 비슷한 방식으로 작동 합니다. 맞춤선을 사용 하면 사용자가 정확 하지 않고 여전히 목표를 달성할 수 있습니다. 제한 된 회전은 웹 브라우저 및 사진 앨범 같은 응용 프로그램에 유용 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left">결합 된 회전</td>
<td align="left"><p>결합 된 회전은 제한 된 회전에 의해 적용 되는 각각의 90 방향 맞춤 지점에서 영역 (이동 <a href="guidelines-for-panning.md">에 대 한 지침</a>의 레일과 유사)으로 사용 가능한 회전을 지원 합니다. 사용자가 90도 영역 중 하나를 벗어난 개체를 해제 하는 경우 개체는 해당 위치에 유지 됩니다. 그렇지 않으면 개체가 자동으로 맞춤 지점으로 회전 합니다.</p>
<div class="alert">
<strong>참고</strong>    사용자 인터페이스 레일은 목표 영역에서 특정 값 이나 위치에 대 한 움직임을 제한 하 여 선택에 영향을 주는 기능입니다.
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

## <a name="related-topics"></a>관련 항목

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
- [입력: GestureRecognizer를 사용한 제스처 및 조작](/samples/browse/?redirectedfrom=MSDN-samples)
- [Input: 조작 및 제스처 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [DirectX touch 입력 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
