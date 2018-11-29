---
Description: In a Universal Windows Platform (UWP) app, command elements are the interactive UI elements that enable the user to perform actions, such as sending an email, deleting an item, or submitting a form.
title: UWP(유니버설 Windows 플랫폼) 앱용 명령 디자인 기본 사항
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.date: 10/01/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 7ef7927af7acc8f437a323f374ae7dbf8a36d452
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7980215"
---
# <a name="command-design-basics-for-uwp-apps"></a>UWP 앱의 명령 디자인 기본 사항

유니버설 Windows 플랫폼 (UWP) 앱에서 *명령 요소* 는 사용자가 메일 보내기, 항목 삭제 또는 양식 제출과 같은 작업을 수행할 수 있게 해 주는 대화형 UI 요소는 합니다. *명령 인터페이스* 일반적인 명령 요소, 호스트 하는 명령 화면, 지 원하는 조작 및 제공 환경으로 구성 됩니다.

## <a name="provide-the-best-command-experience"></a>명령 최상의 환경을 제공 합니다.

명령 인터페이스의 가장 중요 한 측면은 어떤 사용자는 사용자가 수행할 수 있도록 합니다. 앱의 기능을 계획할 수 있게 할 사용자 환경 및 이러한 작업을 수행 하는 데 필요한 단계를 고려 합니다. 이러한 경험의 초안 완료 했으면를 구현 하는 도구와 상호 작용에 대 한 결정을 만들 수 있습니다.

다음은 몇 가지 일반적인 응용 프로그램 환경입니다.

- 정보를 보내거나 제출
- 설정 선택 등 선택
- 콘텐츠 검색 및 필터링
- 파일 열기, 저장, 삭제
- 콘텐츠 편집이나 만들기

명령 환경 디자인을 사용 하 여 발휘 합니다. 선택 하는 앱 입력 장치 지원 하 고 각 장치에 앱의 응답 방식입니다. 광범위 한 기본 설정과 기능을 지 원하는 앱으로 사용할 수 있는 노트북과 최대한 액세스할 수 있게 됩니다.



<!--
When designing a command interface, the most important decision is choosing what a user can do. To plan the right type of interactions, focus on your app - consider the user experiences you want to enable, and what steps users will need to take. Once you decide what you want users to accomplish, then you can provide them the tools to do so.
-->

## <a name="choose-the-right-command-elements"></a>올바른 명령 요소를 선택 합니다.

명령 인터페이스에서 올바른 요소를 사용 하 여 앱을 직관적으로 사용 하기 쉬운 혼란 스러운 앱이 어렵고 간의 차이 만들 수 있습니다. 다양 한 명령 요소는 유니버설 Windows 플랫폼 (UWP)에서 사용할 수 있습니다. 가장 일반적인 UWP 명령 요소 중 일부 목록은 다음과 같습니다.

:::row:::
    :::column:::
        ![button image](images/commanding/thumbnail-button.svg)
    :::column-end:::
    :::column span="2":::
        <b>Buttons</b>

        <a href="../controls-and-patterns/buttons.md" style="text-decoration:none">Buttons</a> trigger an immediate action. Examples include sending an email, submitting form data, or confirming an action in a dialog.
:::row-end:::

:::row:::
    :::column:::
        ![list image](images/commanding/thumbnail-list.svg)
    :::column-end:::
    :::column span="2":::
        <b>Lists</b>

        <a href="../controls-and-patterns/lists.md" style="text-decoration:none">Lists</a> present items in a interactive list or a grid. Usually used for many options or display items. Examples include drop-down list, list box, list view and grid view.
:::row-end:::

:::row:::
    :::column:::
        ![selection control image](images/commanding/thumbnail-selection.svg)
    :::column-end:::
    :::column span="2":::
        <b>Selection controls</b>

        Lets users choose from a few options, such as when completing a survey or configuring app settings. Examples include <a href="../controls-and-patterns/checkbox.md">check box</a>, <a href="../controls-and-patterns/radio-button.md">radio button</a>, and <a href="../controls-and-patterns/toggles.md">toggle switch</a>.
:::row-end:::

:::row:::
    :::column:::
        ![Calendar  image](images/commanding/thumbnail-calendar.svg)
    :::column-end:::
    :::column span="2":::
        <b>Calendar, date and time pickers</b>

        <a href="../controls-and-patterns/date-and-time.md">Calendar, date and time pickers</a> enable users to view and modify date and time info, such as when creating an event or setting an alarm. Examples include calendar date picker, calendar view, date picker, time picker.
:::row-end:::

:::row:::
    :::column:::
        ![Predictive text entry image](images/commanding/thumbnail-autosuggest.svg)
    :::column-end:::
    :::column span="2":::
        <b>Predictive text entry</b>

        Provides suggestions as users type, such as when entering data or performing queries. Examples include <a href="../controls-and-patterns/auto-suggest-box.md">auto-suggest box</a>.<br>
:::row-end:::

전체 목록은 [컨트롤 및 UI 요소](../controls-and-patterns/index.md)를 참조하세요.

## <a name="place-commands-on-the-right-surface"></a>올바른 화면에 명령 배치

앱 캔버스 또는 명령 모음, 명령 모음 플라이 아웃, 메뉴 모음 또는 대화 상자와 같은 명령 컨테이너를 포함 하 여 앱에서의 다양 한 화면에 명령 요소를 배치할 수 있습니다.

항상 사용자가 콘텐츠를 직접 조작할 수 있도록 하려고 하지 않고 통해 콘텐츠를 끌어서 놓기를 목록 항목을 다시 정렬 같은 아니라 위쪽 및 아래쪽 명령 단추에 대해 실행 되는 명령입니다. 

그러나이 하지 못할 수도 있습니다 특정 입력된 장치 또는 특정 사용자 기능 및 기본 설정을 제어 하는 경우. 이러한 경우 최대한 많은 명령 어포던스를 제공 하 고 앱의 명령 화면에 이러한 명령 요소를 배치 합니다.

가장 일반적인 명령 표면 중 일부 목록은 다음과 같습니다.

:::row:::
    :::column:::
        ![app canvas image](images/commanding/thumbnail-canvas.svg)
    :::column-end:::
    :::column span="2":::
        <b>App canvas (content area)</b>

        If a command is constantly needed for users to complete core scenarios, put it on the canvas. Because you can put commands near (or on) the objects they affect, putting commands on the canvas makes them easy and obvious to use. However, choose the commands you put on the canvas carefully. Too many commands on the app canvas take up valuable screen space and can overwhelm the user. If the command won't be frequently used, consider putting it in another command surface.
:::row-end:::

:::row:::
    :::column:::
        ![commandbar image](images/commanding/thumbnail-commandbar.svg)
    :::column-end:::
    :::column span="2":::
        <b>Command bars and menu bars</b>

        <a href="../controls-and-patterns/app-bars.md">Command bars</a> help organize commands and make them easy to access. Command bars can be placed at the top of the screen, at the bottom of the screen, or at both the top and bottom of the screen (a <a href="../controls-and-patterns/menus.md#create-a-menu-bar">MenuBar</a> can also be used when the functionality in your app is too complex for a command bar).
:::row-end:::

:::row:::
    :::column:::
        ![context menu image](images/commanding/thumbnail-contextmenu.svg)
    :::column-end:::
    :::column span="2":::
        <b>Menus and context menus</b>

        <p>Menus and context menus save space by organizing commands and hiding them until the user needs them. Users typically access a menu or context menu by clicking a button or right-clicking a control.</p> 

        <p>The <a href="../controls-and-patterns/command-bar-flyout.md">command bar flyout </a> is a type of contextual menu that combines the benefits of a command bar and a context menu into a single control. It can provide shortcuts to commonly-used actions and provide access to secondary commands that are only relevant in certain contexts, such as clipboard or custom commands.</p>

        <p>UWP also provides a set of traditional menus and context menus; for more info, see the <a href="../controls-and-patterns/menus.md">menus and context menus overview</a>.</p>
:::row-end:::

## <a name="provide-command-feedback"></a>명령 피드백을 제공 합니다. 

명령 피드백 사용자에 게는 감지 된 상호 작용이 나 명령, 해석 및 처리 된 방법 및 통신 여부 것은 성공 합니다. 이렇게 하면 사용자가 어떤 수행한 및 수행할 수 있는 다음 이해 합니다. 피드백을 UI에 자연스럽게 통합해서 사용자에게 방해가 없도록 만들거나 정말 필요한 경우를 제외하면 추가 작업을 할 필요가 없도록 만드는 것이 좋습니다.

> [!NOTE]
> 반드시 필요한 경우가 있고 피드백을 다른 곳에서 사용할 수 없는 경우가 아니면 피드백을 제공 하지 마십시오. 값을 추가 하는 경우가 아니면 응용 프로그램 UI를 깔끔하고 간결 하 게 유지 유지 합니다.

앱에서 피드백을 제공하는 방법 몇 가지를 소개합니다.

:::row:::
    :::column:::
        ![commandbar content area image](images/commanding/thumbnail-commandbar2.svg)
    :::column-end:::
    :::column span="2":::
        <b>Command bar</b>

        The content area of the <a href="../controls-and-patterns/app-bars.md">command bar</a> is an intuitive place to communicate status to users if they'd like to see feedback.
:::row-end:::

:::row:::
    :::column:::
        ![Flyout image](images/commanding/thumbnail-flyout.svg)
    :::column-end:::
    :::column span="2":::
        <b>Flyouts</b>

       <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">플라이 아웃</a> 은 플라이 아웃 바깥쪽 아무 곳 이나 클릭 또는 탭 하 여 해제할 수 있는 경량의 상황에 맞는 팝업입니다.
:::row-end:::

:::row:::
    :::column:::
        ![Dialog image](images/commanding/thumbnail-dialog.svg)
    :::column-end:::
    :::column span="2":::
        <b>Dialog controls</b>

        <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">Dialog controls</a> are modal UI overlays that provide contextual app information. In most cases, dialogs block interactions with the app window until being explicitly dismissed, and often request some kind of action from the user. Dialogs can be disruptive and should only be used in certain situations. For more info, see the [When to confirm or undo actions](#when-to-confirm-or-undo-actions) section.
    :::column-end:::
:::row-end:::

> [!TIP]
> 앱에서 확인 대화 상자를 사용하는 빈도에 유의하세요. 사용자가 실수하는 경우에는 확인 대화 상자가 매우 유용할 수 있지만 사용자가 의도적으로 작업을 수행하려고 할 때마다 확인 대화 상자를 표시하면 방해가 될 수 있습니다.

### <a name="when-to-confirm-or-undo-actions"></a>작업을 확인하거나 실행 취소하는 경우

응용 프로그램의 UI는, 얼마나 잘 디자인 신중한 작업을 수행 하는 모든 사용자가 있습니다. 앱의 동작을 확인을 요구 하거나 최근 작업을 취소 하는 방법을 제공 하 여 이러한 상황에서 유용 합니다.

:::row:::
    :::column:::
        ![do image](images/do.svg)

        For actions that can't be undone and have major consequences, we recommend using a confirmation dialog. Examples of such actions include:
        -   Overwriting a file
        -   Not saving a file before closing
        -   Confirming permanent deletion of a file or data
        -   Making a purchase (unless the user opts out of requiring a confirmation)
        -   Submitting a form, such as signing up for something
    :::column-end:::
    :::column:::
        ![do image](images/do.svg)

        For actions that can be undone, offering a simple undo command is usually enough. Examples of such actions include:
        -   Deleting a file
        -   Deleting an email (not permanently)
        -   Modifying content or editing text
        -   Renaming a file
:::row-end:::

##  <a name="optimize-for-specific-input-types"></a>특정 입력 유형에 대한 최적화

특정 입력 유형 또는 디바이스에 맞게 사용자 환경을 최적화하는 방법에 대한 자세한 내용은 [상호 작용 입문서](../input/index.md)를 참조하세요.

