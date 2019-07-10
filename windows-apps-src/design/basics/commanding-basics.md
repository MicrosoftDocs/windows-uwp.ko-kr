---
Description: UWP(유니버설 Windows 플랫폼) 앱에서 명령 요소는 사용자가 메일 보내기, 항목 삭제 또는 양식 제출과 같은 작업을 수행할 수 있게 해주는 대화형 UI 요소입니다.
title: UWP(유니버설 Windows 플랫폼) 앱용 명령 디자인 기본 사항
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 11/01/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ac2bd55d1cea25359c3c609148c7098532d76c46
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63796470"
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

명령 모음, 명령 모음 플라이아웃, 메뉴 모음, 대화 상자 같은 앱 캔버스 또는 특수 명령 컨테이너를 포함하여 다양한 앱 화면에 명령 요소를 배치할 수 있습니다.

위쪽 및 아래쪽 명령 단추보다는 끌어서 놓아 목록 항목을 다시 정렬하는 것처럼, 항상 콘텐츠에서 작동하는 명령보다는 사용자가 콘텐츠를 직접 조작하는 방법을 사용합니다. 

하지만 특정 입력 디바이스에서, 또는 특정 사용자 기능 및 기본 설정을 수용하는 경우에는 이렇게 하는 것이 불가능할 수 있습니다. 이 경우 최대한 많은 명령 어포던스를 제공하고, 이러한 명령 요소를 앱의 명령 화면에 배치합니다.

다음은 가장 일반적인 명령 표면 목록입니다.

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

## <a name="provide-command-feedback"></a>명령 피드백 제공 

명령 피드백은 상호 작용 또는 명령이 발견되었다는 사실과 명령을 해석하고 처리한 방법 및 명령의 성공 여부를 사용자에게 전달합니다. 이렇게 하면 사용자는 무엇을 했는지, 앞으로 무엇을 할 수 있는지 이해할 수 있습니다. 꼭 필요한 경우를 제외하고 사용자가 개입하거나 추가 작업을 할 필요가 없도록 피드백을 UI에 자연스럽게 통합하는 것이 가장 좋습니다.

> [!NOTE]
> 꼭 필요하고 다른 곳에서 제공할 수 없는 경우에만 피드백을 제공합니다. 값을 추가하지 않는 이상 애플리케이션 UI를 깨끗하고 깔끔하게 유지합니다.

다음은 앱에서 피드백을 제공하는 방법입니다.

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

       <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">플라이아웃</a> - 플라이아웃 바깥쪽의 아무 곳이나 탭하거나 클릭하여 해제할 수 있는 경량의 상황에 맞는 팝업입니다.
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

앱의 UI가 아무리 잘 설계되었더라도 모든 사용자는 원치 않는 작업을 수행합니다. 앱에서 사용자에게 작업을 확인하도록 요구하거나 최근 작업을 실행 취소하는 방법을 제공하여 이러한 상황을 도와줄 수 있습니다.

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

