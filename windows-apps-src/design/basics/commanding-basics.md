---
author: mijacobs
Description: In a Universal Windows Platform (UWP) app, command elements are the interactive UI elements that enable the user to perform actions, such as sending an email, deleting an item, or submitting a form.
title: UWP(유니버설 Windows 플랫폼) 앱용 명령 디자인 기본 사항
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 05/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 09f775ad0ba596379b6d3ddf158285849520111f
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842570"
---
#  <a name="command-design-basics-for-uwp-apps"></a>UWP 앱의 명령 디자인 기본 사항

UWP(유니버설 Windows 플랫폼) 앱에서 *명령 요소*는 사용자가 메일 보내기, 항목 삭제 또는 양식 제출과 같은 작업을 수행할 수 있게 해주는 대화형 UI 요소입니다. 이 문서에서는 일반적인 명령 요소와 요소가 지원하는 상호 작용, 이를 호스팅하는 명령 화면에 대해 설명합니다.

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

:::row::: :::column::: ![버튼 이미지](images/commanding/thumbnail-button.svg) :::column-end::: :::column span="2"::: <b>버튼</b>

        <a href="../controls-and-patterns/buttons.md" style="text-decoration:none">Buttons</a> trigger an immediate action. Examples include sending an email, submitting form data, or confirming an action in a dialog.
:::row-end:::

:::row::: :::column::: ![목록 이미지](images/commanding/thumbnail-list.svg) :::column-end::: :::column span="2"::: <b>목록</b>

        <a href="../controls-and-patterns/lists.md" style="text-decoration:none">Lists</a> present items in a interactive list or a grid. Usually used for many options or display items. Examples include drop-down list, list box, list view and grid view.
:::row-end:::

:::row::: :::column::: ![선택 컨트롤 이미지](images/commanding/thumbnail-selection.svg) :::column-end::: :::column span="2"::: <b>선택 컨트롤</b>

        Lets users choose from a few options, such as when completing a survey or configuring app settings. Examples include <a href="../controls-and-patterns/checkbox.md">check box</a>, <a href="../controls-and-patterns/radio-button.md">radio button</a>, and <a href="../controls-and-patterns/toggles.md">toggle switch</a>.
:::row-end:::

:::row::: :::column::: ![일정 이미지](images/commanding/thumbnail-calendar.svg) :::column-end::: :::column span="2"::: <b>일정, 날짜 및 시간 선택기</b>

        <a href="../controls-and-patterns/date-and-time.md">Calendar, date and time pickers</a> enable users to view and modify date and time info, such as when creating an event or setting an alarm. Examples include calendar date picker, calendar view, date picker, time picker.
:::row-end:::

:::row::: :::column::: ![자동 완성 텍스트 입력 이미지](images/commanding/thumbnail-autosuggest.svg) :::column-end::: :::column span="2"::: <b>자동 완성 텍스트 입력</b>

        Provides suggestions as users type, such as when entering data or performing queries. Examples include <a href="../controls-and-patterns/auto-suggest-box.md">auto-suggest box</a>.<br>
:::row-end:::

전체 목록은 [컨트롤 및 UI 요소](../controls-and-patterns/index.md)를 참조하세요.

##  <a name="place-commands-on-the-right-surface"></a>올바른 화면에 명령 배치
앱 캔버스(앱의 콘텐츠 영역) 또는 명령 모음, 메뉴, 대화 상자 및 플라이아웃과 같은 명령 컨테이너 역할을 할 수 있는 특수 명령 요소를 비롯하여 앱의 다양한 화면에 명령 요소를 배치할 수 있습니다.

가능한 경우 사용자가 콘텐츠에 실행되는 명령을 사용하기보다, 콘텐츠를 직접 조작할 수 있도록 해야 합니다. 예를 들어, 사용자가 위쪽 및 아래쪽 명령 단추를 사용하기 보다 목록을 끌어서 놓아 목록을 다시 정렬할 수 있도록 합니다.

그렇지 않고 사용자가 콘텐츠를 직접 조작할 수 없는 경우 앱의 명령 화면 중 하나에 명령 요소들을 배치합니다. 가장 일반적인 명령 표면 중 일부 목록은 다음과 같습니다.

:::row::: :::column::: ![앱 캔버스 이미지](images/commanding/thumbnail-canvas.svg) :::column-end::: :::column span="2"::: <b>앱 캔버스(콘텐츠 영역)</b>

        If a command is constantly needed for users to complete core scenarios, put it on the canvas. Because you can put commands near (or on) the objects they affect, putting commands on the canvas makes them easy and obvious to use. However, choose the commands you put on the canvas carefully. Too many commands on the app canvas take up valuable screen space and can overwhelm the user. If the command won't be frequently used, consider putting it in another command surface.
:::row-end:::

:::행::: :::열::: ![명령 모음 이미지](images/commanding/thumbnail-commandbar.svg):::마지막 열::: :::열 범위 ="2"::: <b>대비</b>

        <a href="../controls-and-patterns/app-bars.md">Command bars</a> help organize commands and make them easy to access. Command bars can be placed at the top of the screen, at the bottom of the screen, or at both the top and bottom of the screen.
:::row-end:::

:::row::: :::column::: ![상황에 맞는 메뉴 이미지](images/commanding/thumbnail-contextmenu.svg) :::column-end::: :::column span="2"::: <b>메뉴 및 상황에 맞는 메뉴</b>

        Sometimes it is more efficient to group multiple commands into a command menu to save space. <a href="../controls-and-patterns/menus.md">Menus and context menus</a> display a list of commands or options when the user requests them. Context menus can provide shortcuts to commonly-used actions and provide access to secondary commands that are only relevant in certain contexts, such as clipboard or custom commands. Context menus are usually prompted by a user right-clicking.
:::row-end:::

## <a name="provide-feedback-for-interactions"></a>조작(방식)에 대한 피드백 제공

피드백은 명령에 대한 결과를 통신하고 사용자가 자신이 수행한 작업과 다음에 수행할 수 있는 작업을 이해할 수 있도록 해줍니다. 피드백을 UI에 자연스럽게 통합해서 사용자에게 방해가 없도록 만들거나 정말 필요한 경우를 제외하면 추가 작업을 할 필요가 없도록 만드는 것이 좋습니다. 

앱에서 피드백을 제공하는 방법 몇 가지를 소개합니다.

:::행::: :::열::: ![명령 모음 콘텐츠 영역 이미지](images/commanding/thumbnail-commandbar2.svg):::마지막 열::: :::열 범위 ="2"::: <b>명령 모음</b>

        The content area of the <a href="../controls-and-patterns/app-bars.md">command bar</a> is an intuitive place to communicate status to users if they'd like to see feedback.
:::row-end:::

:::row::: :::column::: ![플라이아웃 이미지](images/commanding/thumbnail-flyout.svg) :::column-end::: :::column span="2"::: <b>플라이아웃</b>

       <a href="../controls-and-patterns/dialogs.md">Flyouts</a> are lightweight contextual popups that can be dismissed by tapping or clicking somewhere outside the flyout.
:::row-end:::

:::row::: :::column::: ![대화 이미지](images/commanding/thumbnail-dialog.svg) :::column-end::: :::column span="2"::: <b>대화 컨트롤</b>

        <a href="../controls-and-patterns/dialogs.md">Dialog controls</a> are modal UI overlays that provide contextual app information. In most cases, dialogs block interactions with the app window until being explicitly dismissed, and often request some kind of action from the user. Dialogs can be disruptive and should only be used in certain situations. For more info, see the [When to confirm or undo actions](#when-to-confirm-or-undo-actions) section.
    :::column-end:::
:::row-end:::

> [!TIP]
> 앱에서 확인 대화 상자를 사용하는 빈도에 유의하세요. 사용자가 실수하는 경우에는 확인 대화 상자가 매우 유용할 수 있지만 사용자가 의도적으로 작업을 수행하려고 할 때마다 확인 대화 상자를 표시하면 방해가 될 수 있습니다.

### <a name="when-to-confirm-or-undo-actions"></a>작업을 확인하거나 실행 취소하는 경우

사용자 인터페이스가 얼마나 잘 디자인되었는지, 사용자가 얼마나 신중한지와 관계없이 어느 시점에서는 모든 사용자가 수행하려고 하지 않았던 작업을 수행하게 됩니다. 앱은 사용자에게 작업을 확인하도록 요구하거나 최근 작업을 실행 취소하는 방법을 제공하여 이러한 상황에서 도움을 줄 수 있습니다.

:::row::: :::column::: ![권장 이미지](images/do.svg)

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
