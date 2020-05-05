---
description: 펜, Surface Dial 및 기타 입력 유형에 맞게 앱을 최적화합니다.
title: 입력 및 조작
keywords: 앱 입력, UWP 애플리케이션 사용자 지정
label: Input and interactions
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
ms.assetid: b771d452-c3ac-4d97-8482-eaf81bf34306
ms.localizationpriority: medium
ms.openlocfilehash: c2d7db47a0731323cbbb45c471428a2496f8d479
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80614938"
---
# <a name="input-and-interactions"></a>입력 및 조작

![입력 아이콘](../images/inputs-2x.png)

<!-- <div>
  <img src="images/keyboard/keyboard-hero.jpg" alt="" />
  <img src="images/input-interactions/icons-inputdevices03.png" />
</div> -->

UWP 앱은 다양한 입력을 자동으로 처리하고 다양한 디바이스에서 실행됩니다. 예를 들어 터치 입력을 사용하도록 설정하기 위한 작업을 수행해야 할 추가 작업이 없습니다. 그러나 특정 유형의 입력 또는 디바이스에 대해 앱을 최적화해야 하는 경우는 있습니다. 예를 들어 그리기 앱을 만드는 경우 펜 입력을 처리하는 방식을 사용자 지정할 수 있습니다.

이 섹션의 디자인 및 코딩 지침은 특정 유형의 입력에 대해 UWP 앱을 사용자 지정하는 데 도움이 됩니다.

:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="input-primer.md">입력 지침서</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">각 입력 디바이스의 유형 및 동작, 기능 및 특정 폼 팩터와 연결될 경우의 제한 사항을 알아보세요.</p>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="gaze-interactions.md">응시 입력</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">눈과 머리의 위치와 움직임에 따라 사용자의 응시를 추적합니다.</p>
    :::column-end:::
:::row-end:::

<!-- 
## Input primer

See our <b>[Input primer](index.md)</b> to familiarize yourself with each input device type and its behaviors, capabilities, and limitations when paired with certain form factors. -->

:::row:::
    :::column:::
        <h2 style="margin-top: 10px; margin-bottom: 0px">입력</h2>
        <a href="/windows/uwp/design/input/identify-input-devices">입력 디바이스 식별</a><br/>
        <a href="/windows/uwp/design/input/handle-pointer-input">포인터</a><br/>
        <a href="/windows/uwp/design/input/pen-and-stylus-interactions">펜 및 Windows Ink</a><br/>
        <a href="/windows/uwp/design/input/touch-interactions">터치</a><br/>
        <a href="/windows/uwp/design/input/mouse-interactions">마우스</a><br/>
        <a href="/windows/uwp/design/input/keyboard-interactions">키보드</a><br/>
        <a href="/windows/uwp/design/input/gamepad-and-remote-interactions">게임 패드 및 리모컨</a><br/>
        <a href="/windows/uwp/design/input/touchpad-interactions">터치패드</a><br/>
        <a href="/windows/uwp/design/input/windows-wheel-interactions">Surface Dial</a><br/>
        <a href="/windows/uwp/design/input/multiple-input-design-guidelines">여러 입력</a><br/>
        <a href="/windows/uwp/design/input/input-injection">입력 주입</a><br/>
        <a href="/windows/uwp/design/input/custom-text-input">사용자 지정 텍스트 입력</a><br/>
    :::column-end:::
    :::column:::
        <h2 style="margin-top: 10px; margin-bottom: 0px">상호 작용</h2>
        <a href="/windows/uwp/design/input/drag-and-drop">끌어서 놓기</a><br/>
        <a href="/windows/uwp/design/input/guidelines-for-panning">이동(좌우)</a><br/>
        <a href="/windows/uwp/design/input/guidelines-for-rotation">회전</a><br/>
        <a href="/windows/uwp/design/input/guidelines-for-textselection">텍스트 및 이미지 선택</a><br/>
        <a href="/windows/uwp/design/input/guidelines-for-targeting">대상 지정</a><br/>
        <a href="/windows/uwp/design/input/guidelines-for-visualfeedback">시각적 피드백</a><br/>
    :::column-end:::
    :::column:::
        <h2 style="margin-top: 10px; margin-bottom: 0px">음성 및 AI</h2>
        <a href="/windows/uwp/design/input/speech-interactions">음성</a><br/>
        <a href="/windows/uwp/design/input/cortana-interactions">Cortana</a><br/>
    :::column-end:::
:::row-end:::


<!-- <div class="side-by-side">
<div class="side-by-side-content">
<p>
<b>[Surface Dial](windows-wheel-interactions.md)</b><br/>
Learn how to integrate this brand new category of input device into your Windows apps.</br>
This device is intended as a secondary, multi-modal input device that complements or modifies input from a primary device.
</p>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[Cortana](cortana-interactions.md)</b><br/>
Extend the basic functionality of Cortana with voice commands that launch and execute a single action in an external application.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[Speech](speech-interactions.md)</b><br/>
Integrate speech recognition and text-to-speech (also known as TTS, or speech synthesis) directly into the user experience of your app.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[Pen](pen-and-stylus-interactions.md)</b><br/>
Optimize your UWP app for pen input to provide both standard pointer device functionality and the best Windows Ink experience for your users.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[Keyboard](keyboard-interactions.md)</b><br/>
Keyboard input is an important part of the overall user interaction experience for apps. The keyboard is indispensable to people with certain disabilities or users who just consider it a more efficient way to interact with an app.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[Touch](touch-interactions.md)</b><br/>
UWP includes a number of different mechanisms for handling touch input, all of which enable you to create an immersive experience that your users can explore with confidence.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[Touchpad](touchpad-interactions.md)</b><br/>
A touchpad combines both indirect multi-touch input with the precision input of a pointing device, such as a mouse. This combination makes the touchpad suited to both a touch-optimized UI and the smaller targets of productivity apps.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[Mouse](mouse-interactions.md)</b><br/>
Mouse input is best suited for user interactions that require precision when pointing and clicking. This inherent precision is naturally supported by the UI of Windows, which is optimized for the imprecise nature of touch.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[Gamepad and remote control](gamepad-and-remote-interactions.md)</b><br/>
UWP apps now support gamepad and remote control input. Gamepads and remote controls are the primary input devices for Xbox and TV experiences.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<p>
<b>[Multiple inputs](multiple-input-design-guidelines.md)</b><br/>
To accommodate as many users and devices as possible, we recommend that you design your apps to work with as many input types as possible (gesture, speech, touch, touchpad, mouse, and keyboard). Doing so will maximize flexibility, usability, and accessibility.
</p>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[Identify input devices](identify-input-devices.md)</b><br/>
Identify the input devices connected to a Universal Windows Platform (UWP) device and identify their capabilities and attributes.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[Handle pointer input](handle-pointer-input.md)</b><br/>
Receive, process, and manage input data from pointing devices, such as touch, mouse, pen/stylus, and touchpad, in Universal Windows Platform (UWP) apps.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p><b>[Custom text input](custom-text-input.md)</b><br/>
The core text APIs in the Windows.UI.Text.Core namespace enable a UWP app to receive text input from any text service supported on Windows devices. This enables the app to receive text in any language and from any input type, like keyboard, speech, or pen.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[Selecting text and images](guidelines-for-textselection.md)</b><br/>
This article describes selecting and manipulating text, images, and controls and provides user experience guidelines that should be considered when using these mechanisms in your apps.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<p>
<b>[Panning](guidelines-for-panning.md)</b><br/>
Panning or scrolling lets users navigate within a single view, to display the content of the view that does not fit within the viewport.
</p>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[Optical zoom and resizing](guidelines-for-optical-zoom.md)</b><br/>
This article describes Windows zooming and resizing elements and provides user experience guidelines for using these interaction mechanisms in your apps.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[Rotation](guidelines-for-rotation.md)</b><br/>
This article describes the new Windows UI for rotation and provides user experience guidelines that should be considered when using this new interaction mechanism in your UWP app.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p><b>[Targeting](guidelines-for-targeting.md)</b><br/>
Touch targeting in Windows uses the full contact area of each finger that is detected by a touch digitizer. The larger, more complex set of input data reported by the digitizer is used to increase precision when determining the user's intended (or most likely) target.
</p>
</div>
<div class="side-by-side-content-right">
<p><b>[Visual feedback](guidelines-for-visualfeedback.md)</b><br/>
Use visual feedback to show users when their interactions are detected, interpreted, and handled. Visual feedback can help users by encouraging interaction. It indicates the success of an interaction, which improves the user's sense of control. It also relays system status and reduces errors.
</p>
</div>
</div>
</div> -->


