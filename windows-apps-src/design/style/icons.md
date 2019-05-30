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
ms.openlocfilehash: 5e464251200812e79474d05d9d0a680b49167871
ms.sourcegitcommit: 7da28cf4f4e8390bc9a21a9488b03af39271cbbe
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64564540"
---
# <a name="icons-for-uwp-apps"></a>UWP 앱의 아이콘

![아이콘 헤더 이미지](images/icons/header-icons.png)

아이콘은 작업, 개념 또는 제품에 대한 시각적 줄임을 제공합니다. 기호 이미지에 의미를 압축하여 아이콘은 언어 장벽을 넘어 매우 중요한 리소스인 화면 공간을 절약할 수 있도록 합니다. 

아이콘은 앱의 내부와 외부에서 나타날 수 있습니다. 

:::row:::
    :::column:::
        **Icons inside the app**

        ![icons inside the app](images/icons/inside-icons.png)
앱 내에서 텍스트를 복사 하거나 설정 페이지로 이동 하는 등의 작업을 나타내는 아이콘을 사용 합니다.
    :::column-end:::
    :::column:::
**앱 외부 아이콘**

        ![icons outside the app](images/icons/outside-icons.jpg)
앱 외부 Windows 작업 표시줄 및 시작 메뉴에서 앱을 나타내는 아이콘을 사용 합니다. 사용자 시작 메뉴에 앱 고정을 선택 하면 앱의 시작 타일 앱의 아이콘을 기능 수 있습니다. 제목 표시줄에 표시 되는 앱의 아이콘 및 앱의 로고를 사용 하 여 시작 화면을 만들 수 있습니다.
    :::column-end:::
:::row-end:::

이 문서에서는 앱 내 아이콘을 설명합니다. 앱 외부 아이콘(앱 아이콘)에 대한 자세한 내용은 [앱 및 타일 아이콘 문서](/windows/uwp/design/shell/tiles-and-notifications/app-assets)를 참조합니다.

## <a name="when-to-use-icons"></a>아이콘을 사용하는 경우

아이콘은 공간을 저장할 수 있지만 언제 사용해야 하나요? 

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icons standard image](images/icons/icons-standard.svg)<br>

작업의 경우 같은 잘라내기, 복사, 붙여넣기 및 저장, 탐색 메뉴에 있는 탐색 항목에 대 한 아이콘을 사용 합니다.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![icons concept image](images/icons/icons-concept.svg)<br>

표시 하려는 개념에 대해 이미 있을 경우 아이콘을 사용 합니다. (아이콘 있는지를 확인 하려면 Segoe 아이콘 목록을 확인 합니다.)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icon shopping cart](images/icons/icon-shopping-cart.svg)<br>

아이콘의 의미를 이해 하기 쉽습니다 및 간단 충분히 작은 크기의 선택을 취소 되도록 하는 경우 아이콘을 사용 합니다.
    :::column-end:::
    :::column:::
        ![dont](images/dont.svg)
        ![icons concept image](images/icons/icon-bad-example.png)<br>

또는 복잡 한 도형이 필요한 명확 하 게 만드는 해당 의미를 명확 하지 않습니다 하는 경우에 아이콘을 사용 하지 마세요.
    :::column-end:::
:::row-end:::



## <a name="using-the-right-type-of-icon"></a>올바른 유형의 아이콘 사용

여러 가지 방법으로 아이콘을 만들 수 있습니다. Segoe MDL2 자산 같은 기호 글꼴을 사용할 수 있습니다. 사용자 고유의 벡터 기반 이미지를 만들 수 있습니다. 비트맵 이미지를 사용할 수도 있지만 권장하지 않습니다. 앱에 아이콘을 추가할 수 있는 다양한 방법에 대한 요약은 다음과 같습니다. 

### <a name="use-a-predefined-icon"></a>미리 정의된 아이콘을 사용합니다.
:::row:::
    :::column:::
Microsoft는 1천 개가 넘는 아이콘 Segoe MDL2 자산 글꼴의 형태로 제공합니다. 글꼴에서 아이콘을 가져오는 것이 직관적이지는 않을 수 있지만 Microsoft의 글꼴 표시 기술은 이러한 아이콘이 모든 디스플레이, 모든 해상도 및 크기에서 선명하고 날카롭게 보이게 합니다. 자세한 내용은 [Segoe MDL2 아이콘](segoe-ui-symbol-font.md)합니다.
    :::column-end:::
    :::column:::
        ![pre-defined icon image](images/icons/predefined-icon.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-font"></a>글꼴을 사용합니다.
:::row:::
    :::column:::
-Segoe MDL2 자산 글꼴을 사용 하지 않아도 Wingdings Webdings 등 해당 시스템에서 사용자가 설치한 모든 글꼴을 사용할 수 있습니다.
    :::column-end:::
    :::column:::
        ![wingdings image](images/icons/wingdings.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>SVG(확장 가능한 벡터 그래픽) 파일을 사용합니다.
:::row:::
    :::column:::
SVG 리소스 항상 모든 크기 또는 해상도 날카로운 모양이 때문에 아이콘, 가장 적합 합니다. 대부분의 그리기 응용 프로그램은 SVG로 내보낼 수 있습니다. 자세한 내용은 [SVGImageSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.svgimagesource)합니다.
    :::column-end:::
    :::column:::
        ![SVG image](images/icons/icon-scale.gif)
    :::column-end:::
:::row-end:::

### <a name="use-geometry-objects"></a>기하 도형 개체를 사용합니다.
:::row:::
    :::column:::
SVG 파일을 같은 기 하 도형 되므로 벡터 기반 리소스에 항상 선명 하 게 보입니다. 그러나 각 지점과 곡선을 개별적으로 지정해야 하므로 기하 도형 만들기는 복잡합니다. 앱을 실행(예: 애니메이션)하는 동안 아이콘을 수정하는 경우에만 좋은 선택입니다. 지침은 [기하 도형용 명령 이동 및 그리기](../../xaml-platform/move-draw-commands-syntax.md)를 참조하세요. 
    :::column-end:::
    :::column:::
        ![Geometry objects image](images/icons/geometry-objects.png)
    :::column-end:::
:::row-end:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>PNG, GIF, 또는 JPEG와 같은 비트맵 이미지를 사용할 수도 있지만 권장하지 않습니다.
:::row:::
    :::column:::
비트맵 이미지 위로 또는 아래로 아이콘을 표시할 크기 및 화면 해상도 따라 조정할 수 있어 특정 크기에 만들어집니다. 이미지가 크기가 축소되면 흐릿하게 나타날 수 있습니다. 크기가 확장되면 일그러지고 모자이크처럼 나타날 수 있습니다. 비트맵 이미지를 사용하는 경우 JPEG보다 PNG 또는 GIF를 이용하는 것이 좋습니다. 
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![Bitmap image](images/icons/bitmap-image.png)
    :::column-end:::
:::row-end:::

## <a name="make-the-icon-do-something"></a>아이콘으로 작업을 수행하도록 만들기

아이콘을 만든 후에 명령 또는 탐색 작업을 연결 하 여 작업을 수행 하도록 다음 단계가입니다. 이 작업을 수행 하는 가장 좋은 방법은 단추 또는 명령 모음 아이콘을 추가 하는 것입니다. 

![명령 모음 이미지](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>아이콘 단추 만들기

표준 단추에 아이콘을 넣을 수 있습니다. 보다 여러 장소에서 단추를 사용할 수 있으므로 알림 아이콘이 표시되는 것에 좀 더 많은 유연성을 제공합니다. 

단추에 아이콘을 추가하는 방법은 몇 가지가 있습니다.

:::row:::
    :::column span="2":::
        <b>Step 1</b><br>
단추의 글꼴 패밀리로 `Segoe MDL2 Assets` 및 해당 콘텐츠 속성을 사용 하려는 기호의 유니코드 값:
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
아이콘 요소 개체 중 하나를 사용할 수 있습니다. [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon)하십시오 [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon), [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon), 또는 [SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon)합니다. 더 다양 한 유형의 아이콘을 사용 하면 하 고 싶다면 아이콘 및 다른 형식의 텍스트와 같은 콘텐츠를 결합할 수 있습니다.
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
잘라내기/복사/붙여넣기 또는 그리기 사진 편집 프로그램에 대 한 명령 집합에 배치 합니다 같은 일련의 함께 이동 하는 명령이 있는 경우는 [명령 모음](../controls-and-patterns/app-bars.md)합니다. 명령 모음에는 각각 작업을 나타내는 하나 이상의 앱 바 단추 또는 앱 바 토글 단추가 있습니다. 각 단추에는 표시하는 아이콘을 제어하는 데 사용하는 [아이콘](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon) 속성이 있습니다. 아이콘의 색을 지정하는 다양한 방법이 있습니다. 
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

* [타일 및 아이콘 자산에 대 한 지침](../shell/tiles-and-notifications/app-assets.md)
