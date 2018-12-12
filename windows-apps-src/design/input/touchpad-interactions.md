---
Description: Create Universal Windows Platform (UWP) apps with intuitive and distinctive user interaction experiences that are optimized for touchpad but are functionally consistent across input devices.
title: 터치 패드 조작
ms.assetid: CEDEA30A-FE94-4553-A7FB-6C1FA44F06AB
label: Touchpad interactions
template: detail.hbs
keywords: 터치 패드, PTP, 터치, 포인터, 입력, 사용자 조작
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8dfc98127133790deba9274ddba7801bea34f9cd
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8935897"
---
# <a name="touchpad-design-guidelines"></a>터치 패드 디자인 지침


사용자가 터치 패드를 통해 조작할 수 있는 앱을 디자인하세요. 터치 패드는 간접 멀티 터치 입력을 마우스와 같은 포인팅 장치의 정밀도 입력과 결합합니다. 이러한 결합을 통해 터치 패드는 터치 최적화된 UI와 생산성 앱의 작은 대상에 모두 적합합니다.

 

![터치 패드](images/input-patterns/input-touchpad.jpg)


터치 패드 조작에는 다음 세 가지가 필요합니다.

-   표준 터치 패드 또는 Windows 정밀 터치 패드.

    정밀 터치 패드는 UWP(유니버설 Windows 플랫폼) 장치에 최적화되어 있습니다. 이러한 터치 패드를 사용하여 손가락을 사용한 추적 및 손바닥을 사용한 검색과 같은 터치 패드 환경의 특정 측면을 처리함으로써 여러 장치 간에 보다 일관된 환경을 제공할 수 있습니다.

-   터치 패드에 하나 이상의 손가락을 직접 접촉
-   터치 접촉 이동(또는 시간 임계값을 기반으로 하는 접촉 결핍)

터치 패드 센서에서 제공하는 입력 데이터에는 다음이 적용됩니다.

-   하나 이상의 UI 요소를 직접 조작하기 위한 물리적 제스처로 해석됩니다(예: 이동, 회전, 크기 조정). 반면에 속성 창이나 기타 대화 상자를 통해 요소를 조작하는 것은 간접 조작으로 간주됩니다.
-   마우스 또는 펜과 같은 대체 입력 메서드로 인식됩니다.
-   펜으로 그린 잉크 스트로크를 문지르는 등 다른 입력 측면을 수정하거나 보완하는 데 사용합니다.

터치 패드는 간접 멀티 터치 입력을 마우스와 같은 포인팅 디바이스의 정밀도 입력과 결합합니다. 이러한 결합을 통해 터치 패드는 터치 최적화된 UI와 생산성 앱 및 데스크톱 환경의 작은 대상에 모두 적합합니다. UWP 앱 디자인을 터치식 입력에 최적화하고 기본적으로 터치 패드 지원을 받으세요.

터치 패드가 지원하는 조작 환경의 수렴 때문에 터치식 입력에 대한 기본 제공 지원 외에 [**PointerEntered**](https://msdn.microsoft.com/library/windows/apps/br208968) 이벤트를 사용하여 마우스 스타일 UI 명령을 제공하는 것이 좋습니다. 예를 들어 이전 및 다음 단추를 사용하여 사용자가 콘텐츠 페이지를 전환하고 콘텐츠를 살펴볼 수 있게 합니다.

이 항목에서 설명하는 제스처 및 지침은 앱이 터치 패드 입력을 최소한의 코드로 매끄럽게 지원할 수 있게 합니다.

## <a name="the-touchpad-language"></a>터치 패드 언어


터치 패드 조작의 축약된 집합이 전체 시스템에서 일관되게 사용됩니다. 앱을 터치 및 마우스 입력에 최적화하면 이 언어를 통해 사용자가 앱에 즉시 친숙해져서 사용자의 신뢰가 증가하며 앱을 쉽게 배우고 사용할 수 있습니다.

사용자는 표준 터치 패드의 경우보다 훨씬 더 정밀한 터치 패드 제스처 및 조작 동작을 설정할 수 있습니다. 다음 두 이미지는 표준 터치 패드 및 정밀 터치 패드 각각에 대해 Settings &gt; Devices &gt; Mouse &amp; touchpad의 다른 터치 패드 설정 페이지를 보여 줍니다.

![표준 터치 패드 설정](images/mouse-touchpad-settings-standard.png)

<sup>표준\\ 터치 패드\\ 설정</sup>

![Windows 정밀 터치 패드 설정](images/mouse-touchpad-settings-ptp.png)

<sup>Windows\\ 정밀\\ 터치 패드\\ 설정</sup>

다음은 일반적인 작업을 수행하기 위한 터치 패드에 최적화된 몇 가지 제스처 예입니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">용어</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>세 손가락으로 탭하기</p></td>
<td align="left"><p>검색 <strong>Cortana</strong>를 사용하여 검색하거나 <strong>알림 센터</strong>를 표시하기 위한 사용자 기본 설정입니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>세 손가락으로 밀기</p></td>
<td align="left"><p>가상 데스크톱 작업 보기를 열거나, 바탕 화면을 표시하거나, 열려 있는 앱 간을 전환하기 위한 사용자 기본 설정입니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>한 손가락으로 탭하여 기본 동작 수행</p></td>
<td align="left"><p>한 손가락으로 요소를 탭하여 앱 시작이나 명령 실행과 같은 기본 동작을 호출합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>두 손가락으로 탭하여 마우스 오른쪽 단추 클릭</p></td>
<td align="left"><p>요소를 동시에 두 손가락으로 탭하여 선택하고 상황에 맞는 명령을 표시합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>두 손가락으로 밀어 이동</p></td>
<td align="left"><p>밀기는 주로 이동 조작에 사용하지만 이동, 그리기 또는 쓰기에 사용할 수도 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>손가락을 모으고 늘여 확대/축소</p></td>
<td align="left"><p>손가락을 모으고 늘이는 제스처는 크기 조정 및 시맨틱 줌에 주로 사용됩니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>한 손가락으로 누르고 밀어 다시 정렬</p></td>
<td align="left"><p>요소를 끕니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>한 손가락으로 누르고 밀어 텍스트 선택</p></td>
<td align="left"><p>선택 가능한 텍스트 안을 누르고 밀어 선택합니다. 두 번 탭하여 단어를 선택합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>왼쪽 및 오른쪽 클릭 영역</p></td>
<td align="left"><p>마우스 장치의 왼쪽 및 오른쪽 단추 기능을 에뮬레이트합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="hardware"></a>하드웨어


마우스 장치 기능([**MouseCapabilities**](https://msdn.microsoft.com/library/windows/apps/br225626))을 쿼리하여 터치 패드 하드웨어에서 직접 액세스할 수 있는 앱 UI 측면을 식별합니다. 터치와 마우스 입력 둘 다에 대한 UI를 제공하는 것이 좋습니다.

디바이스 기능 쿼리에 대한 자세한 내용은 [입력 디바이스 식별](identify-input-devices.md)을 참조하세요.

## <a name="visual-feedback"></a>시각적 피드백


-   이동 또는 가리키기 이벤트를 통해 터치 패드 커서가 검색되면 마우스 관련 UI를 표시하여 이벤트에 의해 노출되는 기능을 나타냅니다. 터치 패드 커서가 정해진 시간 동안 이동하지 않거나 사용자가 터치 조작을 시작하면 터치 패드 UI가 점점 사라지도록 합니다. 이렇게 하면 UI가 깔끔하고 간결하게 유지됩니다.
-   가리키기 피드백에 커서를 사용하지 마세요. 요소에서 제공하는 피드백만으로 충분합니다(아래 커서 참조).
-   요소가 조작을 지원하지 않는 경우(예: 정적 테스트) 시각적 피드백을 표시하지 마세요.
-   터치 패드 조작 시 포커스 사각형을 사용하지 마세요. 포커스 사각형은 키보드 조작에 예약합니다.
-   동일한 입력 대상을 나타내는 모든 요소에 대해 동시에 시각적 피드백을 표시합니다.

시각적 피드백에 대한 일반적인 내용은 [시각적 피드백에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh465342)을 참조하세요.

## <a name="cursors"></a>커서


터치 패드 포인터에 일련의 표준 커서를 사용할 수 있습니다. 이러한 커서는 요소의 기본 동작을 나타내는 데 사용됩니다.

각 표준 커서에는 해당 기본 이미지가 연결되어 있습니다. 사용자나 앱은 언제든지 표준 커서와 연결된 기본 이미지를 바꿀 수 있습니다. UWP 앱은 [**PointerCursor**](https://msdn.microsoft.com/library/windows/apps/br208273) 함수를 통해 커서 이미지를 지정합니다.

마우스 커서를 사용자 지정해야 하는 경우

-   클릭 가능한 요소에는 항상 화살표 커서(![화살표 커서](images/cursor-arrow.png))를 사용합니다. 링크 또는 다른 대화형 요소에 가리키는 손 모양 커서(![가리키는 손 모양 커서](images/cursor-pointinghand.png))를 사용하지 마세요. 대신 앞에서 설명한 가리키기 효과를 사용합니다.
-   선택 가능한 텍스트에는 텍스트 커서(![텍스트 커서](images/cursor-text.png))를 사용합니다.
-   이동이 기본 동작인 경우(예제: 끌기 또는 자르기) 이동 커서(![이동 커서](images/cursor-move.png))를 사용합니다. 기본 동작이 탐색인 요소(예제: 시작 타일)에는 이동 커서를 사용하지 마세요.
-   개체 크기를 조정할 수 있는 경우 가로, 세로 및 대각선 크기 조정 커서(![세로 크기 조정 커서](images/cursor-vertical.png), ![가로 크기 조정 커서](images/cursor-horizontal.png), ![대각선 크기 조정 커서(왼쪽 아래, 오른쪽 위)](images/cursor-diagonal2.png), ![대각선 크기 조정 커서(왼쪽 위, 오른쪽 아래)](images/cursor-diagonal1.png))를 사용합니다.
-   고정 캔버스 내에서 콘텐츠를 이동하는 경우(예제: 지도) 잡는 손 모양 커서(![잡는 손 모양(열림)](images/cursor-pan1.png), ![잡는 손 모양(닫힘)](images/cursor-pan2.png))를 사용합니다.

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
 



