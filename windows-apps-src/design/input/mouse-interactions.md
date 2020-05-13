---
Description: 터치 및 펜 입력에 사용 하는 것과 동일한 기본 포인터 이벤트를 처리 하 여 앱의 마우스 입력에 응답 합니다.
title: 마우스 상호 작용
ms.assetid: C8A158EF-70A9-4BA2-A270-7D08125700AC
label: Mouse
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2d6ddf03541e94f89d0950a4f4c03eebaa0e396e
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234474"
---
# <a name="mouse-interactions"></a>마우스 상호 작용

터치 입력을 위한 Windows 앱 디자인을 최적화 하 고 기본적으로 기본적인 마우스 지원을 받으세요. 

![마우스](images/input-patterns/input-mouse.jpg)

마우스 입력은를 가리키고 클릭할 때 전체 자릿수가 필요한 사용자 상호 작용에 가장 적합 합니다. 이러한 고유 전체 자릿수는 터치의 부정확 한 특성에 대해 최적화 된 Windows의 UI에서 자연스럽 게 지원 됩니다.

마우스 및 터치 입력 분기는 터치가 해당 개체에서 직접 수행 하는 실제 제스처를 통해 UI 요소의 직접 조작을 보다 긴밀 하 게 에뮬레이트할 수 있는 기능입니다 (예: 살짝 밀기, 슬라이딩, 끌기, 회전 등). 일반적으로 마우스로 조작 하려면 핸들을 사용 하 여 개체의 크기를 조정 하거나 회전 하는 등의 다른 UI affordance 필요 합니다.

이 항목에서는 마우스 상호 작용에 대 한 디자인 고려 사항에 대해 설명 합니다.

## <a name="the-uwp-app-mouse-language"></a>UWP 앱 마우스 언어

간결한 일련의 마우스 상호 작용은 시스템 전체에서 일관 되 게 사용 됩니다.

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
<td align="left"><p>자세히 가리키기</p></td>
<td align="left"><p>요소 위로 마우스를 이동 하면 작업에 대 한 약정 없이 더 자세한 정보 또는 교육 시각적 개체 (예: 도구 설명)가 표시 됩니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>기본 작업을 마우스 왼쪽 단추를 클릭 합니다.</p></td>
<td align="left"><p>요소를 마우스 왼쪽 단추를 클릭 하 여 해당 기본 동작을 호출 합니다 (예: 앱 시작 또는 명령 실행).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>변경 보기로 스크롤</p></td>
<td align="left"><p>콘텐츠 영역 내에서 위쪽, 아래쪽, 왼쪽 및 오른쪽으로 이동 하려면 스크롤 막대를 표시 합니다. 사용자는 스크롤 막대를 클릭 하거나 마우스 휠을 돌려 스크롤할 수 있습니다. 스크롤 막대는 콘텐츠 영역 내에서 현재 보기의 위치를 나타낼 수 있습니다. 터치를 사용 하 여 이동 하면 비슷한 UI가 표시 됩니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>마우스 오른쪽 단추를 클릭 하 여 선택 하 고 명령</p></td>
<td align="left"><p>마우스 오른쪽 단추를 클릭 하 여 탐색 모음 (있는 경우)을 표시 하 고 전역 명령이 포함 된 앱 모음을 표시 합니다. 요소를 마우스 오른쪽 단추로 클릭 하 여 선택 하 고 선택한 요소에 대 한 상황별 명령이 포함 된 앱 표시줄을 표시 합니다.</p>
<div class="alert">
<strong>참고</strong>    선택 또는 앱 바 명령이 적절 한 UI 동작이 아니면 마우스 오른쪽 단추를 클릭 하 여 상황에 맞는 메뉴를 표시 합니다. 그러나 모든 명령 동작에 대해 앱 바를 사용 하는 것이 좋습니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>확대/축소 하는 UI 명령</p></td>
<td align="left"><p>응용 프로그램 표시줄에 UI 명령을 표시 하거나 (예: + 및-) Ctrl 키를 누르고 마우스 휠을 회전 하 여 확대/축소 제스처를 에뮬레이션 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>회전할 UI 명령</p></td>
<td align="left"><p>응용 프로그램 표시줄에 UI 명령을 표시 하거나 Ctrl + Shift를 누르고 마우스 휠을 회전 하 여 회전을 위한 회전 제스처를 에뮬레이션 합니다. 장치 자체를 회전 하 여 전체 화면을 회전 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>마우스 왼쪽 단추를 클릭 하 고 끌어서 다시 정렬</p></td>
<td align="left"><p>요소를 마우스 왼쪽 단추를 클릭 하 고 끌어 이동 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>마우스 왼쪽 단추를 클릭 하 고 끌어서 텍스트 선택</p></td>
<td align="left"><p>선택 가능한 텍스트 내에서 마우스 왼쪽 단추를 클릭 하 고 끌어서 선택 합니다. 단어를 두 번 클릭 하 여 선택 합니다.</p></td>
</tr>
</tbody>
</table>

## <a name="mouse-input-events"></a>마우스 입력 이벤트

대부분의 마우스 입력은 모든 [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) 개체에서 지 원하는 일반적인 라우트된 입력 이벤트를 통해 처리할 수 있습니다. 이러한 개체는 다음과 같습니다.

- [**BringIntoViewRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)
- [**CharacterReceived**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.characterreceived)
- [**컨텍스트 취소 됨**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextcanceled)
- [**ContextRequested 됨**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextrequested)
- [**DoubleTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.doubletapped)
- [**System.windows.dragdrop.dragenter>**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragenter)
- [**System.windows.dragdrop.dragleave>**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragleave)
- [**System.windows.uielement.dragover>**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragover)
- [**DragStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragstarting)
- [**그림자**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.drop)
- [**DropCompleted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dropcompleted)
- [**Get이상 포커스**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gettingfocus)
- [**System.windows.uielement.gotfocus>**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus)
- [**Ctrl**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.holding)
- [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown)
- [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup)
- [**LosingFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.losingfocus)
- [**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus)
- [**System.windows.uielement.manipulationcompleted>**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)
- [**System.windows.uielement.manipulationdelta>**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationdelta)
- [**System.windows.uielement.manipulationinertiastarting>**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting)
- [**System.windows.uielement.manipulationstarted>**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarted)
- [**System.windows.uielement.manipulationstarting>**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarting)
- [**NoFocusCandidateFound**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.nofocuscandidatefound)
- [**PointerCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercanceled)
- [**PointerCaptureLost**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost)
- [**PointerEntered 됨**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered)
- [**PointerExited**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited)
- [**PointerMoved 됨**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved)
- [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed)
- [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased)
- [**PointerWheelChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**System.windows.forms.control.previewkeydown>**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeydown)
- [**PreviewKeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeyup)
- [**오른쪽 탭**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.righttapped)
- [**탭**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.tapped)

그러나 [Windows](https://docs.microsoft.com/uwp/api/windows.ui.input)의 포인터, 제스처 및 조작 이벤트를 사용 하 여 각 장치 (예: 마우스 휠 이벤트)의 특정 기능을 활용할 수 있습니다.

**샘플:** 의 [Basicinput 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)을 참조 하세요.

## <a name="guidelines-for-visual-feedback"></a>시각적 피드백에 대한 지침

- 이동 또는 가리키기 이벤트를 통해 마우스가 검색 되 면 요소에 의해 노출 되는 기능을 나타내는 마우스 특정 UI를 표시 합니다. 마우스가 특정 시간 동안 이동 하지 않거나 사용자가 터치 상호 작용을 시작 하는 경우 마우스 UI를 점차적으로 페이드 아웃 합니다. 이렇게 하면 UI가 깨끗 하 고 깔끔하게 유지 됩니다.
- 가리키기 피드백에 커서를 사용 하지 마세요. 요소에서 제공 하는 피드백이 충분 합니다 (아래 커서 참조).
- 요소가 상호 작용을 지원 하지 않는 경우 (예: 정적 텍스트) 시각적 피드백을 표시 하지 않습니다.
- 마우스 상호 작용에 포커스 사각형을 사용 하지 마세요. 키보드 상호 작용을 위해 예약 합니다.
- 동일한 입력 대상을 나타내는 모든 요소에 대해 시각적 피드백을 동시에 표시 합니다.
- 이동, 회전, 확대/축소 등의 터치 기반 조작을 에뮬레이트 하는 단추 (예: + 및-)를 제공 합니다.

시각적 피드백에 대 한 보다 일반적인 지침은 [시각적 피드백에 대 한 지침](guidelines-for-visualfeedback.md)을 참조 하세요.

## <a name="cursors"></a>커서

마우스 포인터에는 표준 커서 집합을 사용할 수 있습니다. 이러한 요소는 요소의 기본 동작을 나타내는 데 사용 됩니다.

각 표준 커서에는 해당 하는 기본 이미지가 연결 되어 있습니다. 사용자 또는 앱은 언제 든 지 표준 커서와 연결 된 기본 이미지를 바꿀 수 있습니다. [**Pointercursor**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointercursor) 함수를 통해 커서 이미지를 지정 합니다.

마우스 커서를 사용자 지정 해야 하는 경우:

- 항상 화살표 커서를 사용 합니다.![화살표 커서](images/cursor-arrow.png))를 클릭 합니다. 가리키는 손 모양 커서를 사용 하지 않습니다.![손 모양 커서 가리키기](images/cursor-pointinghand.png))를 연결 합니다. 대신 가리키기 효과 (이전에 설명)를 사용 합니다.
- 텍스트 커서를 사용 합니다.![텍스트 커서](images/cursor-text.png))를 선택 합니다.
- 이동 커서를 사용 합니다.![커서 이동](images/cursor-move.png))를 이동 하는 것이 기본 동작입니다 (예: 끌기 또는 자르기). 기본 액션이 탐색 (예: 시작 타일) 인 요소에는 이동 커서를 사용 하지 마세요.
- 가로, 세로 및 대각선 크기 조정 커서를 사용 합니다 (![세로 크기 조정 커서](images/cursor-vertical.png), ![가로 크기 조정 커서](images/cursor-horizontal.png), ![대각선 크기 조정 커서 (왼쪽 아래, 오른쪽 위)](images/cursor-diagonal2.png), ![대각선 크기 조정 커서 (왼쪽 위, 오른쪽 아래)](images/cursor-diagonal1.png))를 사용할 수 있습니다.
- 잡은 커서를 사용 합니다.![손 모양 커서 (열림)](images/cursor-pan1.png), ![손 모양 커서 (닫힘)](images/cursor-pan2.png))로 고정 된 캔버스 내에서 콘텐츠를 이동 하는 경우 (예: 맵).

## <a name="related-articles"></a>관련된 문서

- [포인터 입력 처리](handle-pointer-input.md)
- [입력 디바이스 식별](identify-input-devices.md)
- [이벤트 및 라우트된 이벤트 개요](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview)

### <a name="samples"></a>샘플

- [기본 입력 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [짧은 대기 시간 입력 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [사용자 상호 작용 모드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [포커스 화면 효과 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)
