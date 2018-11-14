---
author: Karl-Bridge-Microsoft
Description: This topic describes the use of contact geometry for touch targeting and provides best practices for targeting in Windows Runtime apps.
title: 타기팅
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bad800f3858cdfdf3def3a9a04854f078b3af399
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6154079"
---
# <a name="guidelines-for-targeting"></a>타기팅에 대한 지침


Windows의 터치 타기팅은 터치 디지타이저로 감지되는 각 손가락의 전체 접촉 영역을 사용합니다. 사용자가 의도했거나 대상이 될 가능성 높은 대상을 결정할 때 정확도를 높이기 위해 디지타이저가 보고하는 좀 더 크고 좀 더 복잡한 입력 데이터 집합이 사용됩니다.

> **중요 API**: [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383), [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084), [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994)

이 항목에서는 터치 타기팅을 위한 접촉 기하 사용에 대해 설명하고 UWP 앱에서 타기팅에 대한 모범 사례를 제공합니다.

## <a name="measurements-and-scaling"></a>측정값 및 배율


다양한 화면 크기 및 픽셀 밀도에서 일관성을 유지하기 위해서는 모든 대상 크기를 실제 단위(밀리미터)로 나타내야 합니다. 실제 단위는 다음 수식을 사용해서 픽셀로 변환할 수 있습니다.

픽셀 수 = 픽셀 밀도 × 측정값

다음 예제에서는 이 수식을 사용하여 135PPI(인치당 픽셀) 디스플레이 및 1x 배율 플라토에서 9mm 대상 크기에 대한 픽셀 크기를 계산합니다.

픽셀 수 = 135PPI × 9mm

픽셀 수 = 135PPI × (0.03937(mm당 인치 수 × 9mm)

픽셀 수 = 135PPI × 0.35433인치

픽셀 수 = 48픽셀

시스템 내에 정의된 배율 플라토에 따라 결과를 조정해야 합니다.

## <a name="thresholds"></a>임계값


거리 및 시간 임계값은 조작의 결과를 결정하는 데 사용될 수 있습니다.

예를 들어 터치다운이 감지될 경우 개체를 터치다운 지점에서 2.7mm 이내로 끈 다음 터치다운의 0.1초 이내에서 터치를 떼면 탭 동작으로 등록됩니다. 이러한 2.7mm 임계값을 벗어나 손가락을 움직이면 개체가 끌기되고 선택 또는 이동됩니다(자세한 내용은 [가로질러 밀기에 대한 지침](guidelines-for-cross-slide.md) 참조). 앱에 따라 0.1초보다 길게 손가락을 대고 있으면 시스템이 자체 노출 상호 작용을 수행하게 됩니다(자세한 내용은 [시각적 피드백에 대한 지침](guidelines-for-visualfeedback.md) 참조).

## <a name="target-sizes"></a>대상 크기


일반적으로 터치 대상 크기를 9mm 정사각형 이상(135 PPI 디스플레이에서 48x48픽셀, 1.0x 배율 수준)으로 설정합니다. 7mm 정사각형보다 작은 터치 대상을 사용하지 마세요.

다음 다이어그램은 시각적 대상, 실제 대상 크기, 실제 대상과 다른 잠재적 대상 사이의 여백이 조합되어 총 대상 크기가 결정되는 방식을 보여 줍니다.

![시각적 대상, 실제 대상 및 여백의 권장 크기를 보여 주는 다이어그램](images/targeting-size.png)

다음 표에는 터치 대상의 모든 구성 요소에 대한 최소 크기 및 권장 크기가 나와 있습니다.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">대상 구성 요소</th>
<th align="left">최소 크기</th>
<th align="left">권장 크기</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">여백</td>
<td align="left">2mm</td>
<td align="left">해당 없음</td>
</tr>
<tr class="even">
<td align="left">시각적 대상 크기</td>
<td align="left">&lt;실제 크기의 60%</td>
<td align="left">실제 크기의 90-100%
<p>시각적 대상이 4.2mm 정사각형(권장 최소 대상 크기 7mm의 60%)보다 작을 경우 대부분 사용자는 시각적 대상을 터치할 수 있다는 것을 인식하지 못합니다.</p></td>
</tr>
<tr class="odd">
<td align="left">실제 대상 크기</td>
<td align="left">7mm 정사각형</td>
<td align="left">9mm 정사각형(48 x 48px @ 1x)보다 크거나 같음</td>
</tr>
<tr class="even">
<td align="left">총 대상 크기</td>
<td align="left">11 x 11mm (약 60px: 3개의 20px 눈금 단위 @ 1x)</td>
<td align="left">13.5 x 13.5mm (72 x 72px @ 1x)
<p>이것은 실제 대상 및 여백이 조합된 크기가 최소 크기보다 커야 함을 의미합니다.</p></td>
</tr>
</tbody>
</table>

 

이러한 권장 대상 크기는 특정 시나리오에 따라 조정될 수 있습니다. 이 권장 크기에는 일부 고려해야 할 사항이 포함되어 있습니다.

-   터치 빈도: 반복되거나 자주 눌리는 대상은 최소 크기보다 크게 지정해 보세요.
-   오류 결과: 터치할 때 심각한 오류가 생기는 대상은 여백보다 커야 하며 콘텐츠 영역보다 커야 합니다. 이는 자주 터치되는 대상의 경우 더 합니다.
-   콘텐츠 영역의 위치
-   폼 팩터 및 화면 크기
-   손가락 모양
-   터치 시각화
-   하드웨어 및 터치 디지타이저

## <a name="targeting-assistance"></a>대상 지정 지원


Windows에서는 여기에 제공된 최소 크기 또는 여백 권장 지침이 적용될 수 없는 시나리오를 위한 대상 지정 지원을 제공합니다. 예를 들면 웹 페이지의 하이퍼링크, 일정 컨트롤, 드롭다운 목록과 콤보 상자, 텍스트 선택 등이 있습니다.

이러한 대상 지정 플랫폼 개선 기능과 사용자 인터페이스 동작은 시각적 피드백(명확성 UI)과 함께 작동하여 사용자 정확도와 신뢰도를 높여줍니다. 자세한 내용은 [시각적 피드백에 대한 지침](guidelines-for-visualfeedback.md)을 참조하세요.

터치 가능 요소가 최소 권장 대상 크기보다 작아야 하는 경우 다음 기술을 사용하여 대상 지정 문제를 최소화할 수 있습니다.

## <a name="tethering"></a>테더링


테더링은 입력 접촉 지점이 개체와 직접적으로 연결되어 있지 않더라도 개체에 연결하고 개체를 조작함을 사용자에게 알리는 데 사용되는 시각 신호(접촉 지점과 개체의 경계 직사각형을 연결하는 연결선)입니다. 다음과 같은 경우에 이러한 현상이 나타날 수 있습니다.

-   터치 접촉은 일부 개체에 근접한 임계값에서 우선 인식되며 이 개체는 접촉하려고 한 대상으로 식별됩니다.
-   터치 접촉이 개체에서 벗어났어도 접촉은 근접 임계값 내에 여전이 남습니다.

JavaScript를 사용하는 UWP 앱 개발자에게는 이 기능이 노출되지 않습니다.

## <a name="scrubbing"></a>스크러빙


스크러빙은 대상 필드 내의 아무 위치나 터치한 다음 원하는 대상에 도달할 때까지 손가락을 떼지 않고 미는 동작을 의미합니다. 이 동작을 "이륙 활성화"라고 합니다. 여기서 활성화된 개체는 화면에서 손가락을 뗄 때 마지막으로 터치한 개체입니다.

스크러빙 상호 작용을 설계하는 경우 다음 지침을 따르세요.

-   스크러빙은 명확성 UI와 함께 사용됩니다. 자세한 내용은 [시각적 피드백에 대한 지침](guidelines-for-visualfeedback.md)을 참조하세요.
-   스크러빙 터치 대상의 권장 최소 크기는 20px(3.75mm @ 1x 크기)입니다.
-   스크러빙은 이동할 수 있는 화면(예: 웹 페이지)에서 수행될 때 가장 우선합니다.
-   스크러빙 대상은 서로 가까이 있어야 합니다.
-   사용자가 스크러빙 대상에서 손가락을 떼면 작업이 취소됩니다.
-   대상이 수행하는 작업이 피해를 주지 않는 작업(예: 날짜 간의 전환)이면 스크러빙 대상에 테더링이 지정됩니다.
-   테더링은 가로, 세로 또는 한 방향으로 지정됩니다.

## <a name="related-articles"></a>관련 문서


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
 

 




