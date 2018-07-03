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
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 077967c37f76c8f1d0942f365344de65db13b041
ms.sourcegitcommit: ce45a2bc5ca6794e97d188166172f58590e2e434
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/06/2018
ms.locfileid: "1983576"
---
# <a name="icons-for-uwp-apps"></a>UWP 앱의 아이콘

![아이콘 헤더 이미지](images/icons/header-icons.png)

아이콘은 작업, 개념 또는 제품에 대한 시각적 줄임을 제공합니다. 기호 이미지에 의미를 압축하여 아이콘은 언어 장벽을 넘어 매우 중요한 리소스인 화면 공간을 절약할 수 있도록 합니다. 

아이콘은 앱의 내부와 외부에서 나타날 수 있습니다. 

:::row::: :::column::: **앱 내 아이콘**

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

:::row::: :::column::: ![권장](images/do.svg) ![아이콘 표준 이미지](images/icons/icons-standard.svg)<br>

        Use an icon for actions, like cut, copy, paste, and save, or for navigation items in a navigation menu.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![icons concept image](images/icons/icons-concept.svg)<br>

        Use an icon if one already exists for the concept you want to represent. (To see whether an icon exists, check the Segoe icon list.)
    :::column-end:::
:::row-end:::

:::row::: :::column::: ![권장](images/do.svg) ![아이콘 쇼핑 카트](images/icons/icon-shopping-cart.svg)<br>

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
:::row::: :::column::: Microsoft는 Segoe MDL2 자산 글꼴의 형식으로 1000개 이상의 아이콘을 제공합니다. 글꼴에서 아이콘을 가져오는 것이 직관적이지는 않을 수 있지만 Microsoft의 글꼴 표시 기술은 이러한 아이콘이 모든 디스플레이, 모든 해상도 및 크기에서 선명하고 날카롭게 보이게 합니다. :::column-end::: :::column::: ![미리 정의된 아이콘 이미지](images/icons/predefined-icon.png) :::column-end::: :::row-end:::

### <a name="use-a-font"></a>글꼴을 사용합니다.
:::row::: :::column::: Segoe MDL2 자산 글꼴을 사용하지 않아도 됩니다. Wingdings 또는 Webdings 등 시스템에 사용자가 설치한 모든 글꼴을 사용할 수 있습니다.
:::column-end::: :::column::: ![wingdings 이미지](images/icons/wingdings.png) :::column-end::: :::row-end:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>SVG(확장 가능한 벡터 그래픽) 파일을 사용합니다.
:::row::: :::column::: SVG 리소스는 모든 크기 또는 해상도에서 항상 선명하게 보이므로 아이콘에 적합합니다. 대부분의 그리기 응용 프로그램은 SVG로 내보낼 수 있습니다. :::column-end::: :::column::: ![SVG 이미지](images/icons/icon-scale.gif) :::column-end::: :::row-end:::

### <a name="use-geometry-objects"></a>기하 도형 개체를 사용합니다.
:::row::: :::column::: SVG 파일과 같이 기하 도형은 벡터 기반 리소스이므로 항상 선명하게 보입니다. 그러나 각 지점과 곡선을 개별적으로 지정해야 하므로 기하 도형 만들기는 복잡합니다. 앱을 실행(예: 애니메이션)하는 동안 아이콘을 수정하는 경우에만 좋은 선택입니다. 지침은 [기하 도형용 명령 이동 및 그리기](../../xaml-platform/move-draw-commands-syntax.md)를 참조하세요. :::column-end::: :::column::: ![기하 도형 개체 이미지](images/icons/geometry-objects.png) :::column-end::: :::row-end:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>PNG, GIF, 또는 JPEG와 같은 비트맵 이미지를 사용할 수도 있지만 권장하지 않습니다.
:::row::: :::column::: 비트맵 이미지는 원하는 아이콘의 크기와 화면의 해상도에 따라 확장할 수 있도록 특정 크기로 만들어집니다. 이미지가 크기가 축소되면 흐릿하게 나타날 수 있습니다. 크기가 확장되면 일그러지고 모자이크처럼 나타날 수 있습니다. 비트맵 이미지를 사용하는 경우 JPEG보다 PNG 또는 GIF를 이용하는 것이 좋습니다. :::column-end::: :::column::: ![금지](images/dont.svg) ![비트맵 이미지](images/icons/bitmap-image.png) :::column-end::: :::row-end:::

## <a name="make-the-icon-do-something"></a>아이콘으로 작업을 수행하도록 만들기

아이콘이 있으면, 다음 단계는 명령이나 탐색 작업을 연결하여 작업을 수행할 수 있도록 만드는 것입니다. 이 작업을 수행하는 가장 좋은 방법은 단추나 명령 모음에 아이콘을 추가하는 것입니다. 

![명령 모음 이미지](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>아이콘 단추 만들기

표준 단추에 아이콘을 넣을 수 있습니다. 보다 여러 장소에서 단추를 사용할 수 있으므로 알림 아이콘이 표시되는 것에 좀 더 많은 유연성을 제공합니다. 

단추에 아이콘을 추가하는 방법은 몇 가지가 있습니다.

:::row::: :::column span="2"::: <b>1단계</b><br>
        단추의 글꼴 패밀리를 `Segoe MDL2 Assets`으로 설정하고 이 콘텐츠 속성을 사용할 문자 모양의 유니코드 값으로 설정합니다. :::column-end::: :::column::: ![아이콘 단추 만들기 1단계](images/icons/create-icon-step-1.svg) :::column-end::: :::row-end:::

```xaml 
<Button FontFamily="Segoe MDL2 Assets" Content="&#xE102;" />
```

:::row::: :::column span="2"::: <b>2단계</b><br>
        [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon), [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon), [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon), 또는 [SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon) 등 아이콘 요소 개체 중 하나를 사용할 수 있습니다. 이를 통해 더 많은 유형의 아이콘을 선택할 수 있으며 원하는 경우 텍스트 등의 다른 유형의 콘텐츠와 아이콘을 결합할 수 있습니다. :::column-end::: :::column::: ![아이콘 단추 만들기 2단계](images/icons/icon-text-step-2.svg) :::column-end::: :::row-end:::

```xaml 
<Button>
    <StackPanel>
        <SymbolIcon Symbol="Play" />
        <TextBlock>Play the movie</TextBlock>
    </StackPanel>
</Button>
```

## <a name="create-a-series-of-icons-in-a-command-bar"></a>명령 모음에서 일련의 아이콘 만들기

:::row::: :::column span::: 잘라내기/복사/붙여넣기 또는 사진 편집 프로그램에 대한 그리기 명령 집합과 같이 함께 사용하는 여러 명령이 있을 때 [명령 모음](../controls-and-patterns/app-bars.md)에 함께 배치합니다. 명령 모음에는 각각 작업을 나타내는 하나 이상의 앱 바 단추 또는 앱 바 토글 단추가 있습니다. 각 단추에는 표시하는 아이콘을 제어하는 데 사용하는 [아이콘](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon) 속성이 있습니다. 아이콘의 색을 지정하는 다양한 방법이 있습니다. :::column-end::: :::column::: ![아이콘의 명령 모음 예](images/icons/create-icon-command-bar.svg) :::column-end::: :::row-end:::

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
