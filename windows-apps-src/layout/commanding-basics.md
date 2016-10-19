---
author: mijacobs
Description: "UWP(유니버설 Windows 플랫폼) 앱에서 명령 요소는 사용자가 메일 보내기, 항목 삭제 또는 양식 제출과 같은 작업을 수행할 수 있게 해주는 대화형 UI 요소입니다."
title: "UWP(유니버설 Windows 플랫폼) 앱용 명령 디자인 기본 사항"
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 736ab8ebe74a293efd48ffd7dcd9d2026089147a

---

#  UWP 앱의 명령 디자인 기본 사항

UWP(유니버설 Windows 플랫폼) 앱에서 *명령 요소*는 사용자가 메일 보내기, 항목 삭제 또는 양식 제출과 같은 작업을 수행할 수 있게 해주는 대화형 UI 요소입니다. 이 문서에서는 단추 및 확인란과 같은 명령 요소, 이러한 명령 요소에서 지원하는 조작 및 명령 요소를 호스트하기 위한 명령 화면(예: 명령 모음 및 상황에 맞는 메뉴)에 대해 설명합니다.

## <span id="Provide_the_right_type_of_interactions"></span><span id="provide_the_right_type_of_interactions"></span><span id="PROVIDE_THE_RIGHT_TYPE_OF_INTERACTIONS"></span>올바른 유형의 조작 제공


명령 인터페이스를 디자인할 때 가장 중요한 결정은 작업을 수행할 수 있어야 하는 사용자를 선택하는 것입니다. 예를 들어 사진 앱을 만드는 경우 사용자에게는 사진을 편집하는 도구가 필요합니다. 그러나 사진을 표시하는 소셜 미디어 앱을 만드는 경우 이미지 편집이 우선순위가 아닐 수 있으므로 공간 절약을 위해 편집 도구를 생략할 수 있습니다. 사용자가 수행하려는 작업을 파악하고 도움이 되는 도구를 제공합니다.

앱에 대한 올바른 조작을 계획하는 방법에 대한 권장 사항은 [앱 계획](https://msdn.microsoft.com/library/windows/apps/hh465427.aspx)을 참조하세요.

## <span id="Use_the_right_command_element_for_the_interaction"></span><span id="use_the_right_command_element_for_the_interaction"></span><span id="USE_THE_RIGHT_COMMAND_ELEMENT_FOR_THE_INTERACTION"></span>조작에 적합한 명령 요소 사용


올바른 조작에 올바른 요소를 사용한다는 것은 직관적으로 사용할 수 있는 앱과 사용하기 어렵거나 혼란스러운 앱 간의 차이를 의미할 수 있습니다. UWP(유니버설 Windows 플랫폼)는 앱에 사용할 수 있는 컨트롤 형식의 대규모 명령 요소 집합을 제공합니다. 가장 일반적인 컨트롤 몇 가지와 이러한 컨트롤을 통해 가능한 조작에 대한 요약 목록은 다음과 같습니다.

| 범주              | 요소                                                                                                                                                                                                            | 조작                                                                                                                                        |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| 단추               | [단추](https://msdn.microsoft.com/library/windows/apps/hh465470)                                                                                                                                                     | 메일 보내기, 대화 상자의 작업 확인, 양식 데이터 제출과 같은 즉각적인 작업을 트리거합니다.                                    |
| 날짜 및 시간 선택 | [달력 날짜 선택, 달력 보기, 날짜 선택, 시간 선택](https://msdn.microsoft.com/library/windows/apps/hh465466)                                                                                                                 | 신용 카드 만료 날짜를 입력하거나 알람을 설정하는 등의 경우에 날짜 및 시간 정보를 보고 수정할 수 있습니다.                   |
| 목록                 | [드롭다운 목록, 목록 상자, 목록 보기 및 그리드 보기](https://msdn.microsoft.com/library/windows/apps/mt186889)                                                                                                                                              | 항목을 대화형 목록이나 그리드로 표시합니다. 사용자가 새 개봉작 목록에서 영화를 선택하거나 인벤토리를 관리할 수 있도록 하려면 이러한 요소를 선택합니다. |
| 자동 완성 텍스트 입력 | [자동 제안 상자](https://msdn.microsoft.com/library/windows/apps/dn997762)                                                                                                                                                                    | 사용자가 입력함에 따라 제안을 제공하여 데이터 입력 또는 쿼리 수행 시 사용자의 시간을 절약해 줍니다.                                                   |
| 선택 컨트롤    | [확인란](https://msdn.microsoft.com/library/windows/apps/hh700393), [라디오 단추](https://msdn.microsoft.com/library/windows/apps/hh700395), [토글 스위치](https://msdn.microsoft.com/library/windows/apps/hh465475) | 설문 조사를 작성하거나 앱 설정을 구성하는 등의 경우에 다양한 옵션 중에서 선택할 수 있습니다.                                      |

 

전체 목록은 [컨트롤 및 UI 요소](https://dev.windows.com/design/controls-patterns)를 참조하세요.

## <span id="_________Place_commands_on_the_right_surface_______"></span><span id="_________place_commands_on_the_right_surface_______"></span><span id="_________PLACE_COMMANDS_ON_THE_RIGHT_SURFACE_______"></span> 올바른 화면에 명령 배치


앱 캔버스(앱의 콘텐츠 영역) 또는 명령 모음, 메뉴, 대화 상자 및 플라이아웃과 같은 명령 컨테이너 역할을 할 수 있는 특수 명령 요소를 비롯하여 앱의 다양한 화면에 명령 요소를 배치할 수 있습니다. 다음은 앱을 배치하기 위한 몇 가지 일반적인 권장 사항입니다.

-   가능한 경우 항상 사용자가 콘텐츠에 실행되는 명령을 추가하기보다 앱의 캔버스에서 콘텐츠를 직접 조작할 수 있도록 합니다. 예를 들어 사용자가 여행 앱에서 활동을 선택하고 위쪽 또는 아래쪽 명령 단추를 사용하기보다 캔버스의 목록에 활동을 끌어다 놓아 여행 일정표를 다시 짤 수 있도록 합니다.
-   그러지 않으면 사용자가 콘텐츠를 직접 조작할 수 없는 경우 이러한 UI 화면 중 하나에 명령을 배치합니다.

    -   [명령 모음](https://msdn.microsoft.com/library/windows/apps/hh465302): 명령을 구성하고 명령에 쉽게 액세스할 수 있도록 명령 모음에 대부분의 명령을 배치해야 합니다.
    -   앱 캔버스: 사용자가 단일 목적을 갖는 페이지나 뷰를 표시하는 경우 해당 목적의 명령을 캔버스에 직접 제공할 수 있습니다. 이러한 명령은 일부에 불과합니다.
    -   [상황에 맞는 메뉴](https://msdn.microsoft.com/library/windows/apps/hh465308): 클립보드 동작(예: 잘라내기, 복사, 붙여넣기) 또는 선택할 수 없는 콘텐츠에 적용되는 명령(예: 지도의 특정 위치에 고정핀 추가)에는 상황에 맞는 메뉴를 사용할 수 있습니다.

다음은 Windows에서 제공하는 명령 화면과 해당 화면을 사용하는 경우에 대한 권장 사항의 목록입니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Surface</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">앱 캔버스(콘텐츠 영역)
<p><img src="images/content-area.png" alt="The content area of an app" /></p></td>
<td align="left"><p>명령이 중요하고 사용자가 핵심 시나리오를 완료하는 데 지속적으로 필요한 경우 캔버스(앱의 콘텐츠 영역)에 배치합니다. 명령을 명령이 영향을 주는 개체 근처나 개체에 배치할 수 있으므로 캔버스에 명령을 배치하면 사용하기가 쉽고 명확합니다.</p>
<p>그러나 캔버스에 배치할 명령을 신중하게 선택하세요. 앱 캔버스에 너무 많은 명령을 배치하면 중요한 화면 공간을 다 차지해버려서 사용자를 당혹스럽게 할 수 있습니다. 명령을 자주 사용하지 않는 경우 메뉴나 명령 모음의 &quot;자세히&quot; 영역 등 다른 명령 화면에 배치하는 것을 고려하세요.</p></td>
</tr>
<tr class="even">
<td align="left">[명령 모음](https://msdn.microsoft.com/library/windows/apps/hh465302)
<p><img src="images/controls-appbar-icons-200.png" alt="Example of a command bar with icons" /></p></td>
<td align="left"><p>명령 모음은 사용자가 작업에 쉽게 액세스할 수 있게 해줍니다. 명령 모음을 사용하여 사진 선택이나 그리기 모드와 같이 사용자의 상황에 맞는 명령이나 옵션을 표시할 수 있습니다.</p>
<p>명령 모음은 화면의 위쪽, 화면의 아래쪽 또는 화면의 위쪽과 아래쪽 둘 다에 배치할 수 있습니다. 이 사진 편집 앱의 디자인에서는 콘텐츠 영역 및 명령 모음을 보여 줍니다.</p>
<p><img src="images/commands-appcanvas-example.png" alt="A photo app" /></p>
<p>명령 모음에 대한 자세한 내용은 [명령 모음에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh465302) 문서를 참조하세요.</p></td>
</tr>
<tr class="odd">
<td align="left">[메뉴 및 상황에 맞는 메뉴](../controls-and-patterns/dialogs-popups-menus.md)
<p><img src="images/controls-contextmenu-singlepane.png" alt="Example of a single-pane context menu" /></p></td>
<td align="left"><p>여러 명령을 명령 메뉴로 그룹화하는 것이 효율적인 경우도 있습니다. 메뉴를 사용하면 더 적은 공간으로 더 많은 옵션을 제공할 수 있습니다. 메뉴에는 대화형 컨트롤이 포함될 수 있습니다.</p>
<p>상황에 맞는 메뉴는 일반적으로 사용되는 작업에 대한 바로 가기를 제공하고 특정 상황에만 관련이 있는 보조 명령에 대한 액세스를 제공할 수 있습니다.</p>
<p>상황에 맞는 메뉴는 다음과 같은 유형의 명령 및 명령 시나리오를 위한 것입니다.</p>
<ul>
<li>복사, 잘라내기, 붙여넣기, 맞춤법 검사 등과 같은 텍스트 선택에 대한 상황별 작업</li>
<li>작업이 수행되어야 하지만 선택할 수 없거나 달리 표시할 수 없는 개체에 대한 명령</li>
<li>클립보드 명령 표시</li>
<li>사용자 지정 명령</li>
</ul>
<p>이 예제에서는 노선 수정, 노선 책갈피 지정 또는 다른 기차 선택을 위한 상황에 맞는 메뉴를 사용하는 지하철 앱의 디자인을 보여 줍니다.</p>
<p><img src="images/subway/uap-subway-ak-8in-dashboard-200.png" alt="A context menu in an subway app" /></p>
<p>상황에 맞는 메뉴에 대한 자세한 내용은 [상황에 맞는 메뉴에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh465308) 문서를 참조하세요.</p></td>
</tr>
<tr class="even">
<td align="left">[대화 상자 컨트롤](../controls-and-patterns/dialogs-popups-menus.md)
<p><img src="images/controls-dialog-twobutton-200.png" alt="Example of a simple two-button dialog" /></p></td>
<td align="left"><p>대화 상자는 상황에 맞는 앱 정보를 제공하는 모달 UI 오버레이입니다. 대부분의 경우 대화 상자는 명시적으로 해제할 때까지 앱 창의 조작을 차단하고 사용자 작업을 요청하기도 합니다.</p>
<p>대화 상자는 불편을 줄 수 있어 특정 상황에서만 사용해야 합니다. 자세한 내용은 [작업을 확인하거나 실행 취소하는 경우](#whentoconfirm) 섹션을 참조하세요.</p></td>
</tr>
<tr class="odd">
<td align="left">[플라이아웃](../controls-and-patterns/dialogs-popups-menus.md)
<p><img src="images/controls-flyout-default-200.png" alt="Image of default flyout" /></p></td>
<td align="left"><p>사용자가 수행하는 작업과 관련된 UI를 표시하는 경량의 상황에 맞는 팝업입니다. 플라이아웃은 다음과 같은 작업에 사용됩니다.</p>
<p></p>
<ul>
<li>메뉴를 표시합니다.</li>
<li>항목에 대한 세부 정보를 표시합니다.</li>
<li>앱 조작을 차단하지 않고 사용자에게 작업 확인을 요청합니다.</li>
</ul>
<p>플라이아웃은 플라이아웃 바깥쪽의 아무 곳이나 탭하거나 클릭하여 해제할 수 있습니다. 플라이아웃 컨트롤에 대한 자세한 내용은 [대화 상자, 메뉴 및 팝업](../controls-and-patterns/dialogs-popups-menus.md) 문서를 참조하세요.</p></td>
</tr>
</tbody>
</table>

 

## <span id="whentoconfirm"></span><span id="WHENTOCONFIRM"></span>작업을 확인하거나 실행 취소하는 경우


사용자 인터페이스가 얼마나 잘 디자인되었는지 및 사용자가 얼마나 신중한지와 관계없이 어느 시점에서는 모든 사용자가 수행하려고 하지 않았던 작업을 수행하게 됩니다. 앱은 사용자에게 작업을 확인하도록 요구하거나 최근 작업을 실행 취소하는 방법을 제공하여 이러한 상황에서 도움을 줄 수 있습니다.

-   실행 취소할 수 없으며 중대한 결과가 발생하는 작업의 경우 확인 대화 상자를 사용하는 것이 좋습니다. 이러한 동작의 예제는 다음과 같습니다.
    -   파일 덮어쓰기
    -   저장하지 않고 파일 닫기
    -   파일 또는 데이터의 영구 삭제 확인
    -   구매(사용자가 확인 요청을 옵트아웃(opt out)하지 않은 경우)
    -   양식 제출(예: 등록)
-   실행 취소할 수 있는 작업의 경우 간단히 실행 취소 명령을 제공하는 것만으로도 충분합니다. 이러한 동작의 예제는 다음과 같습니다.
    -   파일 삭제
    -   메일 삭제(영구적이지 않음)
    -   콘텐츠를 수정 또는 텍스트 편집
    -   파일 이름 바꾸기

**팁** 앱에서 확인 대화 상자를 사용하는 정도와 관련하여 주의하세요. 사용자가 실수하는 경우에는 확인 대화 상자가 매우 유용할 수 있지만 사용자가 의도적으로 작업을 수행하려고 할 때마다 확인 대화 상자를 표시하면 방해가 될 수 있습니다.

 

## <span id="_________Optimize_for_specific_input_types_______"></span><span id="_________optimize_for_specific_input_types_______"></span><span id="_________OPTIMIZE_FOR_SPECIFIC_INPUT_TYPES_______"></span> 특정 입력 유형에 대한 최적화


특정 입력 유형 또는 디바이스에 맞게 사용자 환경을 최적화하는 방법에 대한 자세한 내용은 [상호 작용 입문서](../input-and-devices/input-primer.md)를 참조하세요.




 

 







<!--HONumber=Aug16_HO3-->


