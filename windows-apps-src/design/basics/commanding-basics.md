---
author: mijacobs
Description: In a Universal Windows Platform (UWP) app, command elements are the interactive UI elements that enable the user to perform actions, such as sending an email, deleting an item, or submitting a form.
title: UWP(유니버설 Windows 플랫폼) 앱용 명령 디자인 기본 사항
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 07b9ce7b5a57f6dc1ba202ed57e8b2d4d93e583f
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1654392"
---
#  <a name="command-design-basics-for-uwp-apps"></a>UWP 앱의 명령 디자인 기본 사항

UWP(유니버설 Windows 플랫폼) 앱에서 *명령 요소*는 사용자가 메일 보내기, 항목 삭제 또는 양식 제출과 같은 작업을 수행할 수 있게 해주는 대화형 UI 요소입니다. 

이 문서에서는 일반적인 명령 요소와 요소가 지원하는 상호 작용, 이를 호스팅하는 명령 화면에 대해 설명합니다.

![지도 앱의 명령 요소](images/maps.png)

위 지도 앱의 명령 요소를 참조하세요.

## <a name="provide-the-right-type-of-interactions"></a>올바른 유형의 조작 제공

명령 인터페이스를 디자인할 때 가장 중요한 결정은 작업을 수행할 수 있어야 하는 사용자를 선택하는 것입니다. 적절한 유형의 상호 작용을 계획하려면 앱에 집중하고, 사용하도록 설정하고 싶은 사용자 경험과 사용자가 수행할 단계를 고려하세요. 사용자가 수행하려는 작업을 파악한 후에, 이를 수행할 수 있는 도구를 제공할 수 있습니다.

다음은 앱에 제공하면 좋은 상호 작용 중 일부입니다.

- 정보를 보내거나 제출 
- 설정 선택 등 선택
- 콘텐츠 검색 및 필터링
- 파일 열기, 저장, 삭제
- 콘텐츠 편집이나 만들기

## <a name="use-the-right-command-element-for-the-interaction"></a>상호 작용에 적합한 명령 요소 사용

적합한 요소를 사용해 명령을 사용할 수 있도록 만들면 앱을 직관적으로 쉽게 사용할 수 있습니다. 그렇지 않으면 앱이 어렵고 혼동스러울 수 있습니다. UWP(유니버설 Windows 플랫폼)는 앱에 사용할 수 있는 큰 명령 요소 집합을 제공합니다. 가장 일반적인 컨트롤 몇 가지와 이러한 컨트롤을 통해 가능한 상호 작용에 대한 요약 목록은 다음과 같습니다.

<div class="mx-responsive-img">
<table>
<thead>
<tr class="header">
<th align="left">범주</th>
<th align="left">요소</th>
<th align="left">조작</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>단추</b><br/><br/>
    <img src="../controls-and-patterns/images/controls/button.png" alt="button" /></td>
<td align="left"><a href="../controls-and-patterns/buttons.md">단추</a></td>
<td align="left">즉시 동작을 트리거합니다. 예를 들어, 전자 메일을 보내고, 양식 데이터를 제출하고, 대화에서 작업을 확인합니다.</td>
</tr>
<tr class="even">
<td align="left">목록<br/><br/>
    <img src="../controls-and-patterns/images/controls/combo-box-open.png" alt="drop down list" /></td>
<td align="left"><a href="../controls-and-patterns/lists.md">드롭다운 목록, 목록 상자, 목록 보기 및 그리드 보기</a></td>
<td align="left">항목을 대화형 목록이나 그리드로 표시합니다. 일반적으로 많은 옵션이나 디스플레이 항목에 사용합니다.</td>
</tr>
<tr class="odd">
<td align="left">선택 컨트롤<br/><br/>
    <img src="../controls-and-patterns/images/controls/radio-button.png" alt="radio button" /></td>
<td align="left"><a href="../controls-and-patterns/checkbox.md">확인란</a>, <a href="../controls-and-patterns/radio-button.md">라디오 단추</a>, <a href="../controls-and-patterns/toggles.md">토글 스위치</a></td>
<td align="left">설문 조사를 작성하거나 앱 설정을 구성하는 등의 경우에 몇 가지 옵션 중에서 선택할 수 있습니다.</td>
</tr>
<tr class="even">
<td align="left">날짜 및 시간 선택<br/><br/>
    <img src="../controls-and-patterns/images/controls/calendar-date-picker-open.png" alt="date picker" /></td>
<td align="left"><a href="../controls-and-patterns/date-and-time.md">달력 날짜 선택, 달력 보기, 날짜 선택, 시간 선택</a></td>
<td align="left">이벤트를 만들거나 알람을 설정하는 등의 경우에 날짜 및 시간 정보를 보고 수정할 수 있습니다.</td>
</tr>
<tr class="odd">
<td align="left">텍스트 예측 입력<br/><br/>
    <img src="../controls-and-patterns/images/controls/auto-suggest-box.png" alt="autosuggest box" /></td>
<td align="left"><a href="../controls-and-patterns/auto-suggest-box.md">자동 제안 상자</a></td>
<td align="left">데이터를 입력하거나 쿼리를 수행할 때 등 사용자가 입력을 할 때 제안을 합니다.</td>
</tr>
</tbody>
</table>
</div>

전체 목록은 [컨트롤 및 UI 요소](https://dev.windows.com/design/controls-patterns)를 참조하세요.

##  <a name="place-commands-on-the-right-surface"></a>올바른 화면에 명령 배치
앱 캔버스(앱의 콘텐츠 영역) 또는 명령 모음, 메뉴, 대화 상자 및 플라이아웃과 같은 명령 컨테이너 역할을 할 수 있는 특수 명령 요소를 비롯하여 앱의 다양한 화면에 명령 요소를 배치할 수 있습니다.

가능한 경우 사용자가 콘텐츠에 실행되는 명령을 사용하기보다, 콘텐츠를 직접 조작할 수 있도록 해야 합니다. 예를 들어, 사용자가 위쪽 및 아래쪽 명령 단추를 사용하기 보다 목록을 끌어서 놓아 목록을 다시 정렬할 수 있도록 합니다.
  
그렇지 않고 사용자가 콘텐츠를 직접 조작할 수 없는 경우 앱의 명령 화면 중 하나에 명령 요소들을 배치합니다.

<div class="mx-responsive-img">
<table class="uwpd-top-aligned-table">

<tr class="header">
<th align="left">화면</th>
<th align="left">설명</th>
<th align="left">예</th>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top">앱 캔버스(콘텐츠 영역)
<p><img src="images/content-area.png" alt="The content area of an app" /></p></td>

<td align="left" style="vertical-align: top;">사용자가 핵심 시나리오를 완료하는 데 계속 필요한 명령이라면 캔버스에 배치하세요. 명령을 명령이 영향을 주는 개체 근처나 개체에 배치할 수 있으므로 캔버스에 명령을 배치하면 사용하기가 쉽고 명확합니다.
<p>그러나 캔버스에 배치할 명령을 신중하게 선택하세요. 앱 캔버스에 너무 많은 명령을 배치하면 중요한 화면 공간을 다 차지해버려서 사용자를 당혹스럽게 할 수 있습니다. 자주 사용하지 않는 명령이라면 다른 명령 화면에 배치하는 것을 고려합니다.</p> 
</td><td>
지도 앱 캔버스의 자동 제안 상자.
<br></br>
  <img src="images/maps-canvas.png" alt="autosuggest box on Maps app canvas"/>
</td>
</tr>

<tr class="even">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/app-bars.md">명령 모음</a>
<p><img src="../controls-and-patterns/images/controls_appbar_icons.png" alt="Example of a command bar with icons" /></p></td>
<td align="left" style="vertical-align: top;"> 명령 모음은 명령을 구성하고 쉽게 액세스 할 수 있도록 도움을 줍니다. 명령 모음은 화면의 위쪽, 화면의 아래쪽 또는 화면의 위쪽과 아래쪽 둘 다에 배치할 수 있습니다. 
</td>
<td>
지도 앱 위의 명령 모음.
<br></br>
<img src="images/maps-commandbar.png" alt="command bar in Maps app"/>
</td>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/menus.md">메뉴 및 상황에 맞는 메뉴</a>
<p><img src="images/controls-contextmenu-singlepane.png" alt="Example of a single-pane context menu" /></p></td>
<td align="left" style="vertical-align: top;">여려 명령을 하나의 명령으로 그룹화해서 공간을 절약하는 것이 더 효율적인 경우도 있습니다. 메뉴 및 상황에 맞는 메뉴는 사용자 요청에 따라 명령 또는 옵션 목록을 표시합니다.
<p>상황에 맞는 메뉴는 일반적으로 사용되는 작업에 대한 바로 가기를 제공하고 클립보드나 사용자 지정 명령 등 특정 상황에만 관련이 있는 보조 명령에 대한 액세스를 제공할 수 있습니다. 상황에 맞는 메뉴는 통상 사용자가 마우스 오른쪽 단추를 클릭하면 표시됩니다.</p>
</td><td>
지도 앱에서 사용자가 마우스 오른쪽 단추를 클릭하면 상황에 맞는 메뉴가 나타납니다.
<br></br>
  <img src="images/maps-contextmenu.png" alt="context menu in Maps app"/>
</td>
</tr>
</table>
</div>

## <a name="provide-feedback-for-interactions"></a>조작(방식)에 대한 피드백 제공

피드백은 명령에 대한 결과를 통신하고 사용자가 자신이 수행한 작업과 다음에 수행할 수 있는 작업을 이해할 수 있도록 해줍니다. 피드백을 UI에 자연스럽게 통합해서 사용자에게 방해가 없도록 만들거나 정말 필요한 경우를 제외하면 추가 작업을 할 필요가 없도록 만드는 것이 좋습니다. 

앱에서 피드백을 제공하는 방법 몇 가지를 소개합니다.

<div class="mx-responsive-img">
<table class="uwpd-top-aligned-table">

<tr class="header">
<th align="left">화면</th>
<th align="left">설명</th>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top;"> <a href="../controls-and-patterns/app-bars.md">명령 모음</a>
<p><img src="../controls-and-patterns/images/controls_appbar_icons.png" alt="Example of a command bar with icons" /></p>
</td>
<td align="left" style="vertical-align: top;"> 명령 모음의 콘텐츠 영역은 피드백을 확인하고 싶어하는 사용자에게 직관적으로 상태를 통신할 수 있는 장소입니다.
<p>
  <img src="images/commandbar_anatomy.png" alt="Command bar content area for feedback"/>
  </p>
</td>
</tr>

<tr class="even">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/dialogs.md">플라이아웃</a>
<p><img src="images/controls-flyout-default-200.png" alt="Image of default flyout" /></p></td>
<td align="left" style="vertical-align: top;">
플라이아웃 바깥쪽의 아무 곳이나 탭하거나 클릭하여 해제할 수 있는 경량의 상황에 맞는 팝업입니다.
<p>
  <img src="../controls-and-patterns/images/controls/flyout.png" alt="Flyout above button"/>
  </p>
</td>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/dialogs.md">대화 상자 컨트롤</a>
<p><img src="images/controls-dialog-twobutton-200.png" alt="Example of a simple two-button dialog" /></p></td>
<td align="left" style="vertical-align: top;">대화 상자는 상황에 맞는 앱 정보를 제공하는 모달 UI 오버레이입니다. 대부분의 경우 대화 상자는 명시적으로 해제할 때까지 앱 창의 조작을 차단하고 사용자 작업을 요청하기도 합니다.
<p>대화 상자는 불편을 줄 수 있어 특정 상황에서만 사용해야 합니다. 자세한 내용은 [작업을 확인하거나 실행 취소하는 경우](#when-to-confirm-or-undo-actions) 섹션을 참조하세요.</p>
<p>
  <img src="../controls-and-patterns/images/dialogs/dialog_RS2_delete_file.png" alt="dialog delete file"/></p>
</td>
</tr>

</table>
</div>

> [!TIP]
> 앱에서 확인 대화 상자를 사용하는 빈도에 유의하세요. 사용자가 실수하는 경우에는 확인 대화 상자가 매우 유용할 수 있지만 사용자가 의도적으로 작업을 수행하려고 할 때마다 확인 대화 상자를 표시하면 방해가 될 수 있습니다.

### <a name="when-to-confirm-or-undo-actions"></a>작업을 확인하거나 실행 취소하는 경우

사용자 인터페이스가 얼마나 잘 디자인되었는지, 사용자가 얼마나 신중한지와 관계없이 어느 시점에서는 모든 사용자가 수행하려고 하지 않았던 작업을 수행하게 됩니다. 앱은 사용자에게 작업을 확인하도록 요구하거나 최근 작업을 실행 취소하는 방법을 제공하여 이러한 상황에서 도움을 줄 수 있습니다.

-   실행 취소할 수 없으며 중대한 결과가 발생하는 작업의 경우 확인 대화 상자를 사용하는 것이 좋습니다. 이러한 동작의 예제는 다음과 같습니다.
    -   파일 덮어쓰기
    -   저장하지 않고 파일 닫기
    -   파일 또는 데이터의 영구 삭제 확인
    -   구매(사용자가 확인 요청을 옵트아웃(opt out)하지 않은 경우)
    -   양식 제출(예: 등록)
-   실행 취소할 수 있는 작업의 경우 간단히 실행 취소 명령을 제공하는 것만으로도 충분합니다. 이러한 동작의 예제는 다음과 같습니다.
    -   파일 삭제
    -   메일 삭제(영구적이지 않음)
    -   콘텐츠 수정 또는 텍스트 편집
    -   파일 이름 바꾸기

##  <a name="optimize-for-specific-input-types"></a>특정 입력 유형에 대한 최적화

특정 입력 유형 또는 디바이스에 맞게 사용자 환경을 최적화하는 방법에 대한 자세한 내용은 [상호 작용 입문서](../input/index.md)를 참조하세요.