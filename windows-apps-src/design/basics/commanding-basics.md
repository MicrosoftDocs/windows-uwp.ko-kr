---
Description: UWP(유니버설 Windows 플랫폼) 앱에서 명령 요소는 사용자가 메일 보내기, 항목 삭제 또는 양식 제출과 같은 작업을 수행할 수 있게 해주는 대화형 UI 요소입니다.
title: UWP(유니버설 Windows 플랫폼) 앱용 명령 디자인 기본 사항
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.date: 11/01/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6be51c274078d3b8db5ae50033bbf714ec4aa12a
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081400"
---
# <a name="command-design-basics-for-uwp-apps"></a>UWP 앱의 명령 디자인 기본 사항

UWP(유니버설 Windows 플랫폼) 앱에서 *명령 요소*는 사용자가 이메일 보내기, 항목 삭제 또는 양식 제출 같은 작업을 수행할 수 있게 해주는 대화형 UI 요소입니다. *명령 인터페이스*는 일반적인 명령 요소, 명령 요소를 호스트하는 명령 화면, 명령 요소가 지원하는 상호 작용 및 제공하는 환경으로 구성됩니다.

## <a name="provide-the-best-command-experience"></a>최고의 명령 환경 제공

명령 인터페이스에서 가장 중요한 것은 사용자가 수행할 수 있도록 허용할 작업입니다. 앱의 기능을 계획할 때 이러한 작업을 수행하는 데 필요한 단계와 구현할 사용자 환경을 고려해야 합니다. 이러한 환경의 초안을 작성한 후에는 구현에 사용할 도구와 상호 작용을 결정할 수 있습니다.

다음은 몇 가지 일반적인 명령 환경입니다.

- 정보 전송 또는 제출
- 설정 및 옵션 선택
- 콘텐츠 검색 및 필터링
- 파일 열기, 저장, 삭제
- 콘텐츠 편집 또는 만들기

독창적인 명령 환경을 디자인하세요. 앱에서 지원할 입력 디바이스와 앱이 각 디바이스에 응답하는 방식을 선택합니다. 광범위한 기능과 기본 설정을 지원하여 앱의 사용 편의성, 휴대성 및 접근성을 최대한 높여야 합니다(자세한 내용은 [UWP(유니버설 Windows 플랫폼) 앱의 명령 디자인](../controls-and-patterns/commanding.md) 참조).



<!--
When designing a command interface, the most important decision is choosing what a user can do. To plan the right type of interactions, focus on your app - consider the user experiences you want to enable, and what steps users will need to take. Once you decide what you want users to accomplish, then you can provide them the tools to do so.
-->

## <a name="choose-the-right-command-elements"></a>올바른 명령 요소 선택

명령 인터페이스에 적합한 요소를 사용하면 직관적이고 쉽게 사용할 수 있는 앱을 만들 수 있고, 그렇지 않으면 앱이 어렵고 사용하기 까다롭게 됩니다. 포괄적인 명령 요소는 UWP(유니버설 Windows 플랫폼)에서 사용할 수 있습니다. 다음은 가장 일반적인 UWP 명령 요소 목록입니다.

:::row:::
    :::column:::
![단추 이미지](images/commanding/thumbnail-button.svg)
    :::column-end:::
    :::column span="2":::
<b>단추</b>

<a href="../controls-and-patterns/buttons.md" style="text-decoration:none">Button</a>은 즉각적인 작업을 트리거합니다. 예를 들어 이메일 보내기, 양식 데이터 제출 또는 대화 상자에서 작업 확인이 있습니다.
:::row-end:::

:::row:::
    :::column:::
![목록 이미지](images/commanding/thumbnail-list.svg)
    :::column-end:::
    :::column span="2":::
<b>목록</b>

<a href="../controls-and-patterns/lists.md" style="text-decoration:none">List</a>는 항목을 대화형 목록 또는 그리드로 표시합니다. 일반적으로 많은 옵션 또는 표시 항목에 사용합니다. 예를 들어 드롭다운 목록, 목록 상자, 목록 보기 및 그리드 보기가 있습니다.
:::row-end:::

:::row:::
    :::column:::
![선택 컨트롤 이미지](images/commanding/thumbnail-selection.svg)
    :::column-end:::
    :::column span="2":::
<b>선택 컨트롤</b>

설문 조사를 작성하거나 앱 설정을 구성하는 경우와 같이 사용자가 몇 가지 옵션 중에서 선택할 수 있습니다. 예를 들어 <a href="../controls-and-patterns/checkbox.md">확인란</a>, <a href="../controls-and-patterns/radio-button.md">라디오 단추</a> 및 <a href="../controls-and-patterns/toggles.md">토글 스위치</a>가 있습니다.
:::row-end:::

:::row:::
    :::column:::
![달력 이미지](images/commanding/thumbnail-calendar.svg)
    :::column-end:::
    :::column span="2":::
<b>달력, 날짜 및 시간 선택기</b>

<a href="../controls-and-patterns/date-and-time.md">달력, 날짜 및 시간 선택기</a>를 사용하면 사용자가 이벤트를 만들거나 경보를 설정하는 경우와 같이 날짜 및 시간 정보를 보고 수정할 수 있습니다. 예를 들어 달력 날짜 선택기, 달력 보기, 날짜 선택기, 시간 선택기가 있습니다.
:::row-end:::

:::row:::
    :::column:::
![예측 텍스트 입력 이미지](images/commanding/thumbnail-autosuggest.svg)
    :::column-end:::
    :::column span="2":::
<b>예측 텍스트 항목</b>

데이터를 입력하거나 쿼리를 수행하는 경우와 같이 사용자가 입력함에 따라 제안을 제공합니다. 예를 들어 <a href="../controls-and-patterns/auto-suggest-box.md">자동 제안 상자</a>가 있습니다.<br>
:::row-end:::

전체 목록은 [컨트롤 및 UI 요소](../controls-and-patterns/index.md)를 참조하세요.

## <a name="place-commands-on-the-right-surface"></a>올바른 화면에 명령 배치

명령 모음, 명령 모음 플라이아웃, 메뉴 모음, 대화 상자 같은 앱 캔버스 또는 특수 명령 컨테이너를 포함하여 다양한 앱 화면에 명령 요소를 배치할 수 있습니다.

위쪽 및 아래쪽 명령 단추보다는 끌어서 놓아 목록 항목을 다시 정렬하는 것처럼, 항상 콘텐츠에서 작동하는 명령보다는 사용자가 콘텐츠를 직접 조작하는 방법을 사용합니다. 

하지만 특정 입력 디바이스에서, 또는 특정 사용자 기능 및 기본 설정을 수용하는 경우에는 이렇게 하는 것이 불가능할 수 있습니다. 이 경우 최대한 많은 명령 어포던스를 제공하고, 이러한 명령 요소를 앱의 명령 화면에 배치합니다.

다음은 가장 일반적인 명령 표면 목록입니다.

:::row:::
    :::column:::
![앱 캔버스 이미지](images/commanding/thumbnail-canvas.svg)
    :::column-end:::
    :::column span="2":::
<b>앱 캔버스(콘텐츠 영역)</b>

사용자가 핵심 시나리오를 완료하기 위한 명령이 지속적으로 필요한 경우 해당 명령을 캔버스에 배치합니다. 명령을 명령이 영향을 주는 개체 근처나 개체에 배치할 수 있으므로 캔버스에 명령을 배치하면 사용하기가 쉽고 명확합니다. 그러나 캔버스에 배치할 명령을 신중하게 선택하세요. 앱 캔버스에 너무 많은 명령이 있으면 중요한 화면 공간을 차지하여 사용자를 당혹스럽게 할 수 있습니다. 자주 사용되지 않는 명령인 경우 다른 명령 화면에 배치하는 것이 좋습니다.
:::row-end:::

:::row:::
    :::column:::
![명령 모음 이미지](images/commanding/thumbnail-commandbar.svg)
    :::column-end:::
    :::column span="2":::
<b>명령 모음 및 메뉴 모음</b>

<a href="../controls-and-patterns/app-bars.md">명령 모음</a>을 사용하면 명령을 구성하고 쉽게 액세스 할 수 있습니다. 명령 모음은 화면 위쪽, 화면 아래쪽 또는 양쪽 모두에 배치할 수 있습니다(앱의 기능이 명령 모음에 비해 너무 복잡한 경우 <a href="../controls-and-patterns/menus.md#create-a-menu-bar">MenuBar</a>를 사용할 수도 있음).
:::row-end:::

:::row:::
    :::column:::
![상황에 맞는 메뉴 이미지](images/commanding/thumbnail-contextmenu.svg)
    :::column-end:::
    :::column span="2":::
<b>메뉴 및 상황에 맞는 메뉴</b>

<p>메뉴 및 상황에 맞는 메뉴는 명령을 구성하고 사용자가 요청할 때까지 이를 숨겨 공간을 절약합니다. 사용자는 일반적으로 단추를 클릭하거나 마우스 오른쪽 단추로 컨트롤을 클릭하여 메뉴 또는 상황에 맞는 메뉴에 액세스합니다.</p> 

<p><a href="../controls-and-patterns/command-bar-flyout.md">명령 모음 플라이아웃</a>은 명령 모음과 상황에 맞는 메뉴의 이점을 단일 컨트롤로 결합한 상황에 맞는 메뉴의 한 유형입니다. 일반적으로 사용되는 작업에 대한 바로 가기를 제공하고 특정 상황에만 관련된 보조 명령(예: 클립보드 또는 사용자 지정 명령)에 대한 액세스를 제공할 수 있습니다.</p>

<p>또한 UWP는 기존 메뉴 및 상황에 맞는 메뉴 세트도 제공합니다. 자세한 내용은 <a href="../controls-and-patterns/menus.md">메뉴 및 상황에 맞는 메뉴 개요</a>를 참조하세요.</p>
:::row-end:::

## <a name="provide-command-feedback"></a>명령 피드백 제공 

명령 피드백은 상호 작용 또는 명령이 발견되었다는 사실과 명령을 해석하고 처리한 방법 및 명령의 성공 여부를 사용자에게 전달합니다. 이렇게 하면 사용자는 무엇을 했는지, 앞으로 무엇을 할 수 있는지 이해할 수 있습니다. 꼭 필요한 경우를 제외하고 사용자가 개입하거나 추가 작업을 할 필요가 없도록 피드백을 UI에 자연스럽게 통합하는 것이 가장 좋습니다.

> [!NOTE]
> 꼭 필요하고 다른 곳에서 제공할 수 없는 경우에만 피드백을 제공합니다. 값을 추가하지 않는 이상 애플리케이션 UI를 깨끗하고 깔끔하게 유지합니다.

다음은 앱에서 피드백을 제공하는 방법입니다.

:::row:::
    :::column:::
![명령 모음 콘텐츠 영역 이미지](images/commanding/thumbnail-commandbar2.svg)
    :::column-end:::
    :::column span="2":::
<b>명령 모음</b>

<a href="../controls-and-patterns/app-bars.md">명령 모음</a>의 콘텐츠 영역은 피드백을 확인하려는 사용자에게 상태를 전달하는 직관적인 장소입니다.
:::row-end:::

:::row:::
    :::column:::
![플라이아웃 이미지](images/commanding/thumbnail-flyout.svg)
    :::column-end:::
    :::column span="2":::
<b>플라이아웃</b>

       <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">플라이아웃</a> - 플라이아웃 바깥쪽의 아무 곳이나 탭하거나 클릭하여 해제할 수 있는 경량의 상황에 맞는 팝업입니다.
:::row-end:::

:::row:::
    :::column:::
![대화 상자 이미지](images/commanding/thumbnail-dialog.svg)
    :::column-end:::
    :::column span="2":::
<b>Dialog 컨트롤</b>

<a href="../controls-and-patterns/dialogs-and-flyouts/index.md">Dialog 컨트롤</a>은 상황에 맞는 앱 정보를 제공하는 모달 UI 오버레이입니다. 대부분의 경우 대화 상자는 명시적으로 해제할 때까지 앱 창의 조작을 차단하고 사용자 작업을 요청하기도 합니다. 대화 상자는 불편을 줄 수 있어 특정 상황에서만 사용해야 합니다. 자세한 내용은 [작업을 확인하거나 실행 취소하는 경우](#when-to-confirm-or-undo-actions) 섹션을 참조하세요.
    :::column-end:::
:::row-end:::

> [!TIP]
> 앱에서 확인 대화 상자를 사용하는 빈도에 유의하세요. 사용자가 실수하는 경우에는 확인 대화 상자가 매우 유용할 수 있지만 사용자가 의도적으로 작업을 수행하려고 할 때마다 확인 대화 상자를 표시하면 방해가 될 수 있습니다.

### <a name="when-to-confirm-or-undo-actions"></a>작업을 확인하거나 실행 취소하는 경우

앱의 UI가 아무리 잘 설계되었더라도 모든 사용자는 원치 않는 작업을 수행합니다. 앱에서 사용자에게 작업을 확인하도록 요구하거나 최근 작업을 실행 취소하는 방법을 제공하여 이러한 상황을 도와줄 수 있습니다.

:::row:::
    :::column:::
![허용 이미지](images/do.svg)

실행 취소할 수 없으며 중대한 결과가 발생하는 작업의 경우 확인 대화 상자를 사용하는 것이 좋습니다. 이러한 동작의 예제는 다음과 같습니다.
-   파일 덮어쓰기
-   저장하지 않고 파일 닫기
-   파일 또는 데이터의 영구 삭제 확인
-   구매(사용자가 확인 요청을 옵트아웃(opt out)하지 않은 경우)
-   양식 제출(예: 등록)
    :::column-end:::
    :::column:::
![허용 이미지](images/do.svg)

실행 취소할 수 있는 작업의 경우 간단히 실행 취소 명령을 제공하는 것만으로도 충분합니다. 이러한 동작의 예제는 다음과 같습니다.
-   파일 삭제
-   메일 삭제(영구적이지 않음)
-   콘텐츠 수정 또는 텍스트 편집
-   파일 이름 바꾸기
:::row-end:::

##  <a name="optimize-for-specific-input-types"></a>특정 입력 유형에 대한 최적화

특정 입력 유형 또는 디바이스에 맞게 사용자 환경을 최적화하는 방법에 대한 자세한 내용은 [상호 작용 입문서](../input/index.md)를 참조하세요.

