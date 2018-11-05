---
author: mijacobs
Description: Good icons harmonize with typography and with the rest of the design language. They don’t mix metaphors, and they communicate only what’s needed, as speedily and simply as possible.
title: 아이콘 단추
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.author: mijacobs
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 31bc1d33bccbf2d1948a119ea42bfef2599f3336
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6032200"
---
# <a name="icons-for-uwp-apps"></a>UWP 앱의 아이콘

![아이콘 헤더 이미지](images/icons/header-icons.png)

아이콘은 작업, 개념 또는 제품에 대한 시각적 줄임을 제공합니다. 기호 이미지에 의미를 압축하여 아이콘은 언어 장벽을 넘어 매우 중요한 리소스인 화면 공간을 절약할 수 있도록 합니다. 

아이콘은 앱의 내부와 외부에서 나타날 수 있습니다. 

:::row:::
    :::column:::
        **Icons inside the app**

        ![icons inside the app](images/icons/inside-icons.png)
        Inside your app, you use icons to represent an action, such as copying text or navigating to the settings page.
    :::column-end:::
    :::column:::
        **Icons outside the app**

        ![icons outside the app](images/icons/outside-icons.jpg)
         Outside your app, Windows uses an icon to represent your app in the start menu and in the taskbar. If the user chooses to pin your app to the start menu, your app's start tile can feature your app's icon. Your app's icon appears in the title bar and you can choose to create a splash screen with your app's logo.
    :::column-end:::
:::row-end:::

이 문서에서는 앱 내 아이콘을 설명합니다. 앱 외부 아이콘(앱 아이콘)에 대한 자세한 내용은 [앱 및 타일 아이콘 문서](/windows/uwp/design/shell/tiles-and-notifications/app-assets)를 참조합니다.

## <a name="when-to-use-icons"></a>아이콘을 사용하는 경우

아이콘은 공간을 저장할 수 있지만 언제 사용해야 하나요? 

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icons standard image](images/icons/icons-standard.svg)<br>

        Use an icon for actions, like cut, copy, paste, and save, or for navigation items in a navigation menu.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![icons concept image](images/icons/icons-concept.svg)<br>

        Use an icon if one already exists for the concept you want to represent. (To see whether an icon exists, check the Segoe icon list.)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icon shopping cart](images/icons/icon-shopping-cart.svg)<br>

        Use an icon if it's easy for the user to understand what the icon means and it's simple enough to be clear at small sizes.
    :::column-end:::
    :::column:::
        ![dont](images/dont.svg)
        ![icons concept image](images/icons/icon-bad-example.png)<br>

        Don't use an icon if its meaning isn't clear, or if making it clear requires a complex shape.
    :::column-end:::
:::row-end:::



## <a name="using-the-right-type-of-icon"></a>올바른 유형의 아이콘 사용

여러 가지 방법으로 아이콘을 만들 수 있습니다. Segoe MDL2 자산 같은 기호 글꼴을 사용할 수 있습니다. 자신의 벡터 기반 이미지를 만들 수 있습니다. 비트맵 이미지를 사용할 수도 있지만 권장하지 않습니다. 앱에 아이콘을 추가할 수 있는 다양한 방법에 대한 요약은 다음과 같습니다. 

### <a name="use-a-predefined-icon"></a>미리 정의된 아이콘을 사용합니다.
:::row:::
    :::column:::
        Microsoft provides over 1000 icons in the form of the Segoe MDL2 Assets font. It might not be intuitive to get an icon from a font, but our font display technology means these icons will look crisp and sharp on any display, at any resolution, and at any size. 
    :::column-end:::
    :::column:::
        ![pre-defined icon image](images/icons/predefined-icon.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-font"></a>글꼴을 사용합니다.
:::row:::
    :::column:::
        You don't have to use the Segoe MDL2 Assets font--you can use any font the user has installed on their system, such as Wingdings or Webdings.
    :::column-end:::
    :::column:::
        ![wingdings image](images/icons/wingdings.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>SVG(확장 가능한 벡터 그래픽) 파일을 사용합니다.
:::row:::
    :::column:::
        SVG resources are ideal for icons, because they always look sharp at any size or resolution. Most drawing applications can export to SVG. 
    :::column-end:::
    :::column:::
        ![SVG image](images/icons/icon-scale.gif)
    :::column-end:::
:::row-end:::

### <a name="use-geometry-objects"></a>기하 도형 개체를 사용합니다.
:::row:::
    :::column:::
        Like SVG files, geometries are a vector-based resource, so they always look sharp. However, creating a geometry is complicated because you have to individually specify each point and curve. It's really only a good choice if you need to modify the icon while your app is running (to animate it, for example). For instructions, see [Move and draw commands for geometries](../../xaml-platform/move-draw-commands-syntax.md). 
    :::column-end:::
    :::column:::
        ![Geometry objects image](images/icons/geometry-objects.png)
    :::column-end:::
:::row-end:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>PNG, GIF, 또는 JPEG와 같은 비트맵 이미지를 사용할 수도 있지만 권장하지 않습니다.
:::row:::
    :::column:::
        Bitmap images are created at a specific size, so they have to be scaled up or down depending on how large you want the icon to be and the resolution of the screen. When the image is scaled down (shrunk), it can appear blurry; when it's scaled up, it can appear blocky and pixelated. If you have to use a bitmap image we recommend using a PNG or GIF over a JPEG. 
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![Bitmap image](images/icons/bitmap-image.png)
    :::column-end:::
:::row-end:::

## <a name="make-the-icon-do-something"></a>아이콘으로 작업을 수행하도록 만들기

아이콘이 있는 경우 다음 단계는 명령이 나 탐색 작업을 연결 하 여 작업을 수행 하기 위해입니다. 이 작업을 수행 하는 가장 좋은 방법은 단추나 명령 모음에 아이콘을 추가 하는 것입니다. 

![명령 모음 이미지](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>아이콘 단추 만들기

표준 단추에 아이콘을 넣을 수 있습니다. 보다 여러 장소에서 단추를 사용할 수 있으므로 알림 아이콘이 표시되는 것에 좀 더 많은 유연성을 제공합니다. 

단추에 아이콘을 추가하는 방법은 몇 가지가 있습니다.

:::row:::
    :::column span="2":::
        <b>Step 1</b><br>
        Set the button's font family to `Segoe MDL2 Assets` and its content property to the unicode value of the glyph you want to use:
    :::column-end:::
    :::column:::
        ![Create an icon button step 1](images/icons/create-icon-step-1.svg)
    :::column-end:::
:::row-end:::

```xaml 
<Button FontFamily="Segoe MDL2 Assets" Content="&#xE102;" />
```

:::row:::
    :::column span="2":::
        <b>Step 2</b><br>
        You can use one of the icon element objects: [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon),
        [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon), 
        [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon), or
        [SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon). This gives you more types of icons to choose from, and enables you to combine icons and other types of content, such as text, if you want:
    :::column-end:::
    :::column:::
        ![Create an icon button step 2](images/icons/icon-text-step-2.svg)
    :::column-end:::
:::row-end:::

```xaml 
<Button>
    <StackPanel>
        <SymbolIcon Symbol="Play" />
        <TextBlock>Play the movie</TextBlock>
    </StackPanel>
</Button>
```

## <a name="create-a-series-of-icons-in-a-command-bar"></a>명령 모음에서 일련의 아이콘 만들기

:::row:::
    :::column span:::
        When you have a series of commands that go together, such as cut/copy/paste or a set of drawing commands for a photo-editing program, put them together in a [command bar](../controls-and-patterns/app-bars.md). A command bar takes one or more app bar buttons or app bar toggle buttons, each of which represents an action. Each button has an [Icon](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon) property you use to control which icon it displays. There are a variety of ways to specify the icon. 
    :::column-end:::
    :::column:::
        ![Example of a command bar with icons](images/icons/create-icon-command-bar.svg)
    :::column-end:::
:::row-end:::

가장 쉬운 방법은 미리 정의되어 제공된 아이콘 목록을 사용하는 것입니다. 아이콘 이름을 "뒤로" 또는 "중지"와 같이 지정하기만 하면 시스템이 이 아이콘을 그릴 것입니다. 

``` xaml
<CommandBar>
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle" Click="AppBarButton_Click" />
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat" Click="AppBarButton_Click"/>
    <AppBarSeparator/>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Forward" Label="Forward" Click="AppBarButton_Click"/>
</CommandBar>

```
아이콘 이름의 전체 목록은 [기호 열거](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbol)를 참조하세요. 

명령 모음에 있는 단추에 아이콘을 제공하는 다른 방법은 다음과 같습니다.

+ [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon) - 아이콘이 지정된 글꼴 패밀리의 문자 모양을 기반으로 합니다.
+ [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon) - 아이콘은 지정된 **Uri**로 비트맵 이미지 파일을 기반으로 합니다.
+ [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon) - 아이콘은 [경로](/uwp/api/windows.ui.xaml.shapes.path) 데이터를 기반으로 합니다.

명령 모음에 대한 자세한 내용은 [명령 모음 문서](../controls-and-patterns/app-bars.md)를 참조하세요. 



## <a name="related-articles"></a>관련 문서

* [타일 및 아이콘 자산에 대한 지침](../shell/tiles-and-notifications/app-assets.md)
