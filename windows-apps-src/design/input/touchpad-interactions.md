---
Description: 터치 패드에 최적화 되어 있지만 입력 장치에서 기능적으로 일관 된 직관적인 직관적인 사용자 조작 환경을 사용 하 여 Windows 앱을 만듭니다.
title: 터치 패드 조작
ms.assetid: CEDEA30A-FE94-4553-A7FB-6C1FA44F06AB
label: Touchpad interactions
template: detail.hbs
keywords: 터치 패드, PTP, 터치, 포인터, 입력, 사용자 상호 작용
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9e83cb1ceca96e5c7b51e71cb419b86b0395ea99
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165867"
---
# <a name="touchpad-design-guidelines"></a>터치 패드 디자인 지침


사용자가 터치 패드를 통해 상호 작용할 수 있도록 앱을 디자인 합니다. 터치 패드는 간접 다중 터치 입력을 마우스와 같은 포인팅 장치의 전체 자릿수 입력과 결합 합니다. 이 조합을 사용 하면 터치가 최적화 된 UI와 생산성 앱의 더 작은 대상에 모두 적합 합니다.

 

![패드](images/input-patterns/input-touchpad.jpg)


터치 패드 상호 작용에는 다음 세 가지 작업이 필요 합니다.

-   표준 터치 패드 또는 Windows Precision 터치 패드입니다.

    Precision 터치 패드은 Windows 앱 장치에 대해 최적화 되어 있습니다. 이를 통해 시스템은 장치에 대 한 보다 일관 된 환경을 제공 하기 위해 손가락 추적 및 팜 감지와 같이 기본적으로 터치 패드 환경의 특정 측면을 처리할 수 있습니다.

-   터치 패드에서 하나 이상의 손가락을 직접 연결 하는 것입니다.
-   터치 접점 이동 (또는 시간 임계값에 따라 부족).

터치 패드 센서에서 제공 되는 입력 데이터는 다음과 같을 수 있습니다.

-   하나 이상의 UI 요소 (패닝, 회전, 크기 조정 또는 이동)의 직접 조작을 위한 물리적 제스처로 해석 됩니다. 반면 속성 창이 나 기타 대화 상자를 통해 요소와 상호 작용 하는 것은 간접 조작으로 간주 됩니다.
-   마우스 또는 펜과 같은 대체 입력 방법으로 인식 됩니다.
-   펜으로 그린 잉크 스트로크를 smudging 같은 다른 입력 메서드의 요소를 보완 하거나 수정 하는 데 사용 됩니다.

터치 패드는 간접 다중 터치 입력을 마우스와 같은 포인팅 장치의 전체 자릿수 입력과 결합 합니다. 이 조합은 터치 최적화 된 UI와 일반적으로 더 작은 생산성 앱 및 데스크톱 환경 대상에 모두 적합 합니다. 터치 입력을 위한 Windows 앱 디자인을 최적화 하 고 기본적으로 터치 패드 지원을 받습니다.

터치 패드에서 지 원하는 상호 작용 환경의 수렴 때문에, [**Pointerentered**](/uwp/api/windows.ui.xaml.uielement.pointerentered) 이벤트를 사용 하 여 터치 입력에 대 한 기본 제공 지원과 함께 마우스 스타일의 UI 명령을 제공 하는 것이 좋습니다. 예를 들어 이전 및 다음 단추를 사용 하 여 사용자가 콘텐츠 페이지에서 대칭 이동 뿐만 아니라 콘텐츠를 이동할 수 있습니다.

이 항목에서 설명 하는 제스처와 지침은 앱이 터치 패드 입력을 원활 하 고 최소한의 코드로 지원 하는지 확인 하는 데 도움이 될 수 있습니다.

## <a name="the-touchpad-language"></a>터치 패드 언어


간결한 터치 패드 상호 작용 집합은 시스템 전체에서 일관 되 게 사용 됩니다. 터치 및 마우스 입력을 위해 앱을 최적화 하 고,이 언어를 사용 하면 앱이 사용자에 게 친숙 한 친숙 한 느낌을 높이고 응용 프로그램을 더 쉽게 배우고 사용할 수 있게 됩니다.

사용자는 표준 터치 패드에 사용할 수 있는 것 보다 훨씬 더 높은 정밀도 터치 패드 제스처 및 상호 작용 동작을 설정할 수 있습니다. 이러한 두 이미지는 &gt; &gt; 표준 터치 패드 및 전체 자릿수 터치 패드에 대 한 설정 장치 마우스 & 터치 패드의 서로 다른 터치 패드 설정 페이지를 표시 합니다.

![표준 터치 패드 설정](images/mouse-touchpad-settings-standard.png)

<sup>표준 \\ 터치 패드 \\ 설정</sup>

![windows precision 터치 패드 설정](images/mouse-touchpad-settings-ptp.png)

<sup>Windows \\ Precision \\ 터치 패드 \\ 설정</sup>

일반적인 작업을 수행 하기 위한 터치 패드 최적화 제스처의 몇 가지 예는 다음과 같습니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">용어</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>3 손가락 탭</p></td>
<td align="left"><p><strong>Cortana</strong> 를 사용 하 여 검색 하거나 <strong>동작 센터</strong>를 표시 하기 위한 사용자 기본 설정입니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>3 손가락 슬라이드</p></td>
<td align="left"><p>사용자 기본 설정 가상 데스크톱 작업 보기를 열거나 바탕 화면을 표시 하거나 열려 있는 앱 간을 전환 하는 데 사용 됩니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>기본 작업에 대 한 단일 손가락 탭</p></td>
<td align="left"><p>단일 손가락을 사용 하 여 요소를 탭 하 고 해당 기본 작업 (예: 앱 시작 또는 명령 실행)을 호출 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>마우스 오른쪽 단추로 클릭 하는 두 개의 손가락 탭</p></td>
<td align="left"><p>요소에서 두 손가락을 동시에 눌러 선택 하 고 상황별 명령을 표시 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>이동할 두 손가락 슬라이드</p></td>
<td align="left"><p>슬라이드는 주로 패닝 상호 작용에 사용 되지만 이동, 그리기 또는 쓰기에도 사용할 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>확대 하 고 확대/축소</p></td>
<td align="left"><p>손가락 및 늘이기 제스처는 크기 조정 및 의미 체계 확대/축소에 일반적으로 사용 됩니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>단일 손가락 누름 및 다시 정렬할 슬라이드</p></td>
<td align="left"><p>요소를 끌어 옵니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>단일 손가락으로 텍스트 선택 및 밀기</p></td>
<td align="left"><p>선택 가능한 텍스트 및 슬라이드 내에서 키를 눌러 선택 합니다. 두 번 탭 하 여 단어를 선택 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>왼쪽 및 오른쪽 클릭 영역</p></td>
<td align="left"><p>마우스 장치의 왼쪽 및 오른쪽 단추 기능을 에뮬레이트합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="hardware"></a>하드웨어


마우스 장치 기능 ([**MouseCapabilities**](/uwp/api/Windows.Devices.Input.MouseCapabilities))을 쿼리하여 터치 패드 하드웨어에서 직접 액세스할 수 있는 앱 UI의 측면을 파악 합니다. 터치와 마우스 입력 모두에 UI를 제공 하는 것이 좋습니다.

장치 기능을 쿼리 하는 방법에 대 한 자세한 내용은 [입력 장치 식별](identify-input-devices.md)을 참조 하세요.

## <a name="visual-feedback"></a>시각적 피드백


-   이동 또는 가리키기 이벤트를 통해 터치 패드 커서를 검색 하는 경우 요소에 의해 노출 되는 기능을 나타내는 마우스 특정 UI를 표시 합니다. 터치 패드 커서가 일정 시간 동안 이동 하지 않거나 사용자가 터치 상호 작용을 시작 하는 경우 터치 패드 UI를 점차적으로 페이드 아웃 합니다. 이렇게 하면 UI가 깨끗 하 고 깔끔하게 유지 됩니다.
-   가리키기 피드백에 커서를 사용 하지 마세요. 요소에서 제공 하는 피드백이 충분 합니다 (아래의 커서 섹션 참조).
-   요소가 상호 작용을 지원 하지 않는 경우 (예: 정적 텍스트) 시각적 피드백을 표시 하지 않습니다.
-   터치 패드 상호 작용에 포커스 사각형을 사용 하지 마세요. 키보드 상호 작용을 위해 예약 합니다.
-   동일한 입력 대상을 나타내는 모든 요소에 대해 시각적 피드백을 동시에 표시 합니다.

시각적 피드백에 대 한 보다 일반적인 지침은 [시각적 피드백에 대 한 지침](./guidelines-for-visualfeedback.md)을 참조 하세요.

## <a name="cursors"></a>커서


터치 패드 포인터에는 표준 커서 집합을 사용할 수 있습니다. 이러한 요소는 요소의 기본 동작을 나타내는 데 사용 됩니다.

각 표준 커서에는 해당 하는 기본 이미지가 연결 되어 있습니다. 사용자 또는 앱은 언제 든 지 표준 커서와 연결 된 기본 이미지를 바꿀 수 있습니다. UWP 앱은 [**Pointercursor**](/uwp/api/windows.ui.core.corewindow.pointercursor) 함수를 통해 커서 이미지를 지정 합니다.

마우스 커서를 사용자 지정 해야 하는 경우:

-   항상 화살표 커서를 사용 합니다.![화살표 커서](images/cursor-arrow.png))를 클릭 합니다. 가리키는 손 모양 커서를 사용 하지 않습니다.![손 모양 커서 가리키기](images/cursor-pointinghand.png))를 연결 합니다. 대신 가리키기 효과 (이전에 설명)를 사용 합니다.
-   텍스트 커서를 사용 합니다.![텍스트 커서](images/cursor-text.png))를 선택 합니다.
-   이동 커서를 사용 합니다.![커서 이동](images/cursor-move.png))를 이동 하는 것이 기본 동작입니다 (예: 끌기 또는 자르기). 기본 액션이 탐색 (예: 시작 타일) 인 요소에는 이동 커서를 사용 하지 마세요.
-   가로, 세로 및 대각선 크기 조정 커서를 사용 합니다 (![세로 크기 조정 커서](images/cursor-vertical.png), ![가로 크기 조정 커서](images/cursor-horizontal.png), ![대각선 크기 조정 커서 (왼쪽 아래, 오른쪽 위)](images/cursor-diagonal2.png), ![대각선 크기 조정 커서 (왼쪽 위, 오른쪽 아래)](images/cursor-diagonal1.png))를 사용할 수 있습니다.
-   잡은 커서를 사용 합니다.![손 모양 커서 (열림)](images/cursor-pan1.png), ![손 모양 커서 (닫힘)](images/cursor-pan2.png))로 고정 된 캔버스 내에서 콘텐츠를 이동 하는 경우 (예: 맵).

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