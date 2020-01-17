---
Description: 좋은 아이콘은 입력 체계 및 나머지 디자인 언어와 조화를 이룹니다. 상징을 혼합하지 않고 필요한 사항만 가능한 빠르고 단순하게 전달합니다.
title: 아이콘
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e30e9b2bed5cb4c0b7876ff1c597bb7d1243008a
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684158"
---
# <a name="icons-for-uwp-apps"></a>UWP 앱용 아이콘

![아이콘 헤더 이미지](images/icons/header-icons.png)

아이콘은 작업, 개념 또는 제품에 대한 시각적 줄임 표기를 제공합니다. 의미를 기호 이미지로 압축하는 방식으로 아이콘은 언어 장벽을 넘어 매우 중요한 리소스인 화면 공간을 절약할 수 있도록 합니다. 

아이콘은 앱의 내부와 외부에서 나타날 수 있습니다. 

:::row:::
    :::column:::
        **앱 내의 아이콘**

        ![앱 내의 아이콘](images/icons/inside-icons.png) 앱 내에서 아이콘을 사용하여 텍스트 복사 또는 설정 페이지 탐색과 같은 작업을 나타냅니다.
    :::column-end:::
    :::column:::
**앱 외부 아이콘**

        ![앱 외부 아이콘](images/icons/outside-icons.jpg) 앱 외부에서 Windows는 아이콘을 사용하여 시작 메뉴와 작업 표시줄에 앱을 나타냅니다. 사용자가 앱을 시작 메뉴에 고정하도록 선택하면 앱의 시작 타일에는 앱 아이콘이 포함될 수 있습니다. 앱 아이콘은 제목 표시줄에 표시되며 앱 로고를 사용하여 시작 화면을 만들 수도 있습니다.
    :::column-end:::
:::row-end:::

이 문서에서는 앱 내부 아이콘을 설명합니다. 앱 외부 아이콘(앱 아이콘)에 대한 자세한 내용은 [앱 및 타일 아이콘 문서](/windows/uwp/design/shell/tiles-and-notifications/app-assets)를 참조하세요.

## <a name="when-to-use-icons"></a>아이콘을 사용하는 경우

아이콘은 공간을 절약할 수 있지만 언제 사용해야 하나요? 

:::row:::
    :::column:::
        ![아이콘 표준 이미지](images/icons/icons-standard.svg) ![실행](images/do.svg)<br>

잘라내기, 복사, 붙여 넣기 및 저장 같은 작업이나 탐색 메뉴의 탐색 항목에 아이콘을 사용합니다.
    :::column-end:::
    :::column:::
        ![아이콘 개념 이미지](images/icons/icons-concept.svg) ![실행 안함](images/dont.svg)<br>

표시할 개념에 대한 아이콘이 이미 있는 경우에는 아이콘을 사용합니다. (아이콘이 있는지 확인하려면 Segoe 아이콘 목록을 확인합니다.)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![쇼핑 카트 열기](images/icons/icon-shopping-cart.svg) ![실행](images/do.svg)<br>

사용자가 아이콘의 의미를 쉽게 이해할 수 있고 작은 크기로 분명할 만큼 충분히 단순한 경우 아이콘을 사용합니다.
    :::column-end:::
    :::column:::
        ![아이콘 개념 이미지](images/icons/icon-bad-example.png) ![실행 안함](images/dont.svg)<br>

의미가 명확하지 않거나 분명히 표시하기 위해 복잡한 도형이 필요한 경우에는 아이콘을 사용하지 마세요.
    :::column-end:::
:::row-end:::



## <a name="using-the-right-type-of-icon"></a>올바른 유형의 아이콘 사용

여러 가지 방법으로 아이콘을 만들 수 있습니다. Segoe MDL2 자산 같은 기호 글꼴을 사용할 수 있습니다. 자신의 벡터 기반 이미지를 만들 수 있습니다. 비트맵 이미지를 사용할 수도 있지만 권장하지 않습니다. 앱에 아이콘을 추가할 수 있는 다양한 방법에 대한 요약은 다음과 같습니다. 

### <a name="use-a-predefined-icon"></a>미리 정의된 아이콘을 사용합니다.
:::row:::
    :::column:::
Microsoft는 Segoe MDL2 자산 글꼴의 형식으로 1000개 이상의 아이콘을 제공합니다. 글꼴에서 아이콘을 가져오는 것이 직관적이지는 않을 수 있지만 Microsoft의 글꼴 표시 기술은 이 아이콘이 모든 디스플레이, 모든 해상도 및 크기에서 선명하고 날카롭게 보이게 합니다. 자세한 내용은 [Segoe MDL2 아이콘](segoe-ui-symbol-font.md)을 참조하세요.
    :::column-end:::
    :::column:::
        ![미리 정의된 아이콘 이미지](images/icons/predefined-icon.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-font"></a>글꼴을 사용합니다.
:::row:::
    :::column:::
Segoe MDL2 자산 글꼴을 사용하지 않아도 됩니다. Wingdings 또는 Webdings 등 시스템에 사용자가 설치한 모든 글꼴을 사용할 수 있습니다.
    :::column-end:::
    :::column:::
        ![wingdings 이미지](images/icons/wingdings.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>SVG(확장 가능한 벡터 그래픽) 파일을 사용합니다.
:::row:::
    :::column:::
SVG 리소스는 모든 크기 또는 해상도에서 항상 선명하게 보이므로 아이콘에 적합합니다. 대부분의 그리기 애플리케이션은 SVG로 내보낼 수 있습니다. 자세한 내용은 [SVGImageSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.svgimagesource)를 참조하세요.
    :::column-end:::
    :::column:::
        ![SVG 이미지](images/icons/icon-scale.gif)
    :::column-end:::
:::row-end:::

### <a name="use-geometry-objects"></a>기하 도형 개체를 사용합니다.
:::row:::
    :::column:::
SVG 파일과 같이 기하 도형은 벡터 기반 리소스이므로 항상 선명하게 보입니다. 그러나 각 점과 곡선을 개별적으로 지정해야 하므로 기하 도형 만들기는 복잡합니다. 실제로는 앱을 실행(예: 애니메이션)하는 동안 아이콘을 수정해야 하는 경우에만 좋은 선택입니다. 지침은 [기하 도형용 이동 및 그리기 명령](../../xaml-platform/move-draw-commands-syntax.md)을 참조하세요. 
    :::column-end:::
    :::column:::
        ![기하 도형 개체 이미지](images/icons/geometry-objects.png)
    :::column-end:::
:::row-end:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>PNG, GIF 또는 JPEG와 같은 비트맵 이미지를 사용할 수도 있지만 권장하지 않습니다.
:::row:::
    :::column:::
비트맵 이미지는 원하는 아이콘의 크기와 화면의 해상도에 따라 확장할 수 있도록 특정 크기로 만들어집니다. 이미지가 크기가 축소되면 흐릿하게 나타날 수 있습니다. 크기가 확장되면 일그러지고 모자이크처럼 나타날 수 있습니다. 비트맵 이미지를 사용하는 경우 JPEG보다 PNG 또는 GIF를 이용하는 것이 좋습니다. 
    :::column-end:::
    :::column:::
        ![비트맵 이미지](images/icons/bitmap-image.png) ![실행 안 함](images/dont.svg)
    :::column-end:::
:::row-end:::

## <a name="make-the-icon-do-something"></a>아이콘으로 작업을 수행하도록 설정

아이콘이 있으면, 다음 단계는 명령이나 탐색 작업과 연결하여 작업을 수행하도록 설정하는 것입니다. 이 작업을 수행하는 가장 좋은 방법은 단추나 명령 모음에 아이콘을 추가하는 것입니다. 

![명령 모음 이미지](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>아이콘 단추 만들기

표준 단추에 아이콘을 넣을 수 있습니다. 더 다양한 장소에서 단추를 사용할 수 있으므로 작업 아이콘이 표시되는 위치에 좀 더 많은 유연성을 제공합니다. 

단추에 아이콘을 추가하는 방법은 몇 가지가 있습니다.

:::row:::
    :::column span="2":::
        <b>1단계</b><br>
단추의 글꼴 패밀리를 `Segoe MDL2 Assets`으로 설정하고 이 콘텐츠 속성을 사용할 문자 모양의 유니코드 값으로 설정합니다.
    :::column-end:::
    :::column:::
        ![아이콘 단추 만들기 1단계](images/icons/create-icon-step-1.svg)
    :::column-end:::
:::row-end:::

```xaml 
<Button FontFamily="Segoe MDL2 Assets" Content="&#xE102;" />
```

:::row:::
    :::column span="2":::
        <b>2단계</b><br>
아이콘 요소 개체 [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon), [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon), [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon) 또는 [SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon) 중 하나를 사용할 수 있습니다. 이를 통해 더 많은 유형의 아이콘을 선택할 수 있으며 원하는 경우 텍스트 등의 다른 유형의 콘텐츠와 아이콘을 결합할 수 있습니다.
    :::column-end:::
    :::column:::
        ![아이콘 단추 만들기 2단계](images/icons/icon-text-step-2.svg)
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
잘라내기/복사/붙여넣기 또는 사진 편집 프로그램에 대한 그리기 명령 세트와 같이 함께 사용하는 일련의 명령이 있을 때 [명령 모음](../controls-and-patterns/app-bars.md)에 함께 배치합니다. 명령 모음에는 각각 작업을 나타내는 하나 이상의 앱 바 단추 또는 앱 바 토글 단추가 있습니다. 각 단추에는 표시하는 아이콘을 제어하는 데 사용하는 [Icon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon) 속성이 있습니다. 아이콘을 지정하는 다양한 방법이 있습니다. 
    :::column-end:::
    :::column:::
        ![아이콘이 있는 명령 모음의 예](images/icons/create-icon-command-bar.svg)
    :::column-end:::
:::row-end:::

가장 쉬운 방법은 미리 정의되어 제공된 아이콘 목록을 사용하는 것입니다. 아이콘 이름을 “뒤로” 또는 “중지”와 같이 지정하기만 하면 시스템이 이 아이콘을 그릴 것입니다. 

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

명령 모음에 있는 단추의 아이콘을 제공하는 다른 방법은 다음과 같습니다.

+ [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon) - 아이콘은 지정된 글꼴 패밀리의 문자 모양을 기반으로 합니다.
+ [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon) - 아이콘은 지정된 **Uri**가 포함된 비트맵 이미지 파일을 기반으로 합니다.
+ [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon) - 아이콘은 [경로](/uwp/api/windows.ui.xaml.shapes.path) 데이터를 기반으로 합니다.

명령 모음에 대한 자세한 내용은 [명령 모음 문서](../controls-and-patterns/app-bars.md)를 참조하세요. 



## <a name="related-articles"></a>관련된 문서

* [타일 및 아이콘 자산에 대한 지침](../shell/tiles-and-notifications/app-assets.md)
