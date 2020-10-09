---
description: 플랫폼 텍스트 크기 조정을 지 원하는 사용자 지정/템플릿 컨트롤 및 Windows 앱을 빌드합니다.
title: 텍스트 크기 조정
label: Text scaling
template: detail.hbs
keywords: UWP, 텍스트, 크기 조정, 접근성, "접근성", 표시, "텍스트 크게 만들기", 사용자 조작, 입력
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0d47523ca69f8088d5e13ab944c5dd2be2d1d8ba
ms.sourcegitcommit: d786d084dafee5da0268ebb51cead1d8acb9b13e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91860163"
---
# <a name="text-scaling"></a>텍스트 크기 조정

![100%에서 225% 까지의 텍스트 크기 조정 예제를 보여 주는 주인공 이미지입니다.](images/coretext/text-scaling-news-hero-small.png)  
*Windows 10의 텍스트 크기 조정 예 (100% ~ 225%)*

## <a name="overview"></a>개요

여러 사용자가 컴퓨터 화면에서 텍스트를 읽는 경우 (모바일 장치에서 노트북에서 데스크톱 모니터로 Surface Hub) 반대로, 일부 사용자는 앱 및 웹 사이트에 사용 되는 글꼴 크기를 필요한 것 보다 더 많이 찾을 수 있습니다.

가장 광범위 한 사용자가 텍스트를 최대한 이해 하기 위해 Windows는 사용자가 OS와 개별 응용 프로그램 모두에서 상대적 글꼴 크기를 변경할 수 있는 기능을 제공 합니다. 일반적으로 화면 영역 내의 모든 항목을 확대하고 자체의 유용성 문제를 제기하는 돋보기 앱을 사용하거나, 디스플레이 해상도를 변경하거나, 디스플레이 및 일반 시청 거리에 따라 모든 항목의 크기를 조정하는 DPI 배율을 사용하는 대신, 사용자가 설정에 빠르게 액세스하여 텍스트 크기만 100%(기본 크기)에서 최대 225%까지 조정할 수 있습니다.

## <a name="support"></a>Support(지원)

유니버설 Windows 응용 프로그램 (표준 및 PWA 모두)은 기본적으로 텍스트 크기 조정을 지원 합니다.

Windows 응용 프로그램에 사용자 지정 컨트롤, 사용자 지정 텍스트 표면, 하드 코드 된 컨트롤 높이, 이전 프레임 워크 또는 타사 프레임 워크가 포함 된 경우 사용자에 게 일관 되 고 유용한 환경을 유지 하기 위해 일부 업데이트를 수행 해야 할 수 있습니다.  

DirectWrite, GDI 및 XAML SwapChainPanels는 기본적으로 텍스트 크기 조정을 지원 하지 않지만 Win32 지원은 메뉴, 아이콘 및 도구 모음으로 제한 됩니다.  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>사용자 환경

사용자는 설정-내게 필요한 옵션 > 비전/디스플레이 화면 > 설정에서 텍스트 크게 만들기 슬라이더를 사용 하 여 텍스트 크기 조정을 조정할 수 있습니다.

![텍스트 크게 만들기 슬라이더를 표시 하는 액세스 편의성 비전/표시 설정 페이지의 스크린샷](images/coretext/text-scaling-settings-100-small.png)  
*설정에서 텍스트 크기 조정 설정-> 접근성-> 비전/디스플레이 화면*

## <a name="ux-guidance"></a>UX 지침

텍스트 크기를 조정할 때 컨트롤과 컨테이너는 텍스트와 새 레이아웃을 수용할 수 있도록 크기를 조정 하 고 다시 흐르게 해야 합니다. 앞에서 설명한 것 처럼 앱, 프레임 워크 및 플랫폼에 따라이 작업은 대부분 수행 됩니다. 다음 UX 지침에서는 이러한 경우를 설명 합니다.

### <a name="use-the-platform-controls"></a>플랫폼 컨트롤 사용

이미 생각 하시 나요? 반복 가능: 가능한 경우 항상 다양 한 Windows 앱 프레임 워크와 함께 제공 되는 기본 제공 컨트롤을 사용 하 여 최소한의 노력으로 가장 포괄적인 사용자 환경을 제공 합니다.

예를 들어 모든 UWP 텍스트 컨트롤은 사용자 지정 또는 템플릿 없이 전체 텍스트 크기 조정 환경을 지원 합니다.

다음은 몇 가지 표준 텍스트 컨트롤을 포함 하는 기본 UWP 앱의 코드 조각입니다.

``` xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    <TextBlock Grid.Row="0" 
                Style="{ThemeResource TitleTextBlockStyle}"
                Text="Text scaling test" 
                HorizontalTextAlignment="Center" />
    <Grid Grid.Row="1">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto"/>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
        <Image Grid.Column="0" 
                Source="Assets/StoreLogo.png" 
                Width="450" Height="450"/>
        <StackPanel Grid.Column="1" 
                    HorizontalAlignment="Center">
            <TextBlock TextWrapping="WrapWholeWords">
                The quick brown fox jumped over the lazy dog.
            </TextBlock>
            <TextBox PlaceholderText="Type something here" />
        </StackPanel>
        <Image Grid.Column="2" 
                Source="Assets/StoreLogo.png" 
                Width="450" Height="450"/>
    </Grid>
    <TextBlock Grid.Row="2" 
                Style="{ThemeResource TitleTextBlockStyle}"
                Text="Text scaling test footer" 
                HorizontalTextAlignment="Center" />
</Grid>
```

![100% ~ 225%의 텍스트 크기 조정 애니메이션입니다.](images/coretext/text-scaling.gif)  
*애니메이션 텍스트 크기 조정*

### <a name="use-auto-sizing"></a>자동 크기 조정 사용

컨트롤의 절대 크기를 지정 하지 마세요. 가능 하면 플랫폼에서 사용자 및 장치 설정에 따라 컨트롤의 크기를 자동으로 조정 하도록 합니다.  

이전 예제의이 코드 조각에서는 `Auto` `*` 그리드 열 집합에 대 한 및 너비 값을 사용 하 고 플랫폼이 그리드 내에 포함 된 요소의 크기에 따라 앱 레이아웃을 조정 하도록 합니다.

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>텍스트 줄 바꿈 사용

응용 프로그램의 레이아웃을 최대한 유연 하 고 유연 하 게 유지 하기 위해 텍스트가 포함 된 모든 컨트롤에서 텍스트 잘림을 가능 하 게 합니다. 대부분의 컨트롤은 기본적으로 텍스트 잘림을 지원 하지 않습니다.

텍스트 줄 바꿈을 지정 하지 않으면 플랫폼은 다른 메서드를 사용 하 여 클리핑 (이전 예제 참조)을 포함 하 여 레이아웃을 조정 합니다.

여기서는 `AcceptsReturn` 및 `TextWrapping` 텍스트 상자 속성을 사용 하 여 레이아웃이 최대한 유연 하 게 작동 하는지 확인 합니다.

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![텍스트 잘림 방지 100% ~ 225%의 텍스트 크기 조정 애니메이션입니다.](images/coretext/text-scaling-textwrap.gif)  
*텍스트 줄 바꿈이 있는 애니메이션 텍스트 크기 조정*

### <a name="specify-text-trimming-behavior"></a>텍스트 트리밍 동작 지정

텍스트 줄 바꿈이 기본 동작이 아닌 경우 대부분의 텍스트 컨트롤은 텍스트를 클리핑 하거나 텍스트 트리밍 동작에 대 한 줄임표를 지정할 수 있습니다. 타원이 공간 자체를 차지 하므로 자르기는 줄임표로 기본 설정 됩니다.

> [!NOTE]
> 텍스트를 클리핑 해야 하는 경우 시작 부분이 아니라 문자열의 끝을 자릅니다.

이 예제에서는 [Texttrimming](/uwp/api/windows.ui.xaml.controls.textblock.texttrimming) 속성을 사용 하 여 TextBlock의 텍스트를 자르는 방법을 보여 줍니다.

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![텍스트를 오려낸 텍스트 크기 조정 100% ~ 225%의 스크린샷](images/coretext/text-scaling-clipping-small.png)  
*텍스트를 오려낸 텍스트 크기 조정*

### <a name="use-a-tooltip"></a>도구 설명 사용

텍스트를 자르는 경우 도구 설명을 사용 하 여 전체 텍스트를 사용자에 게 제공 합니다.

여기서는 텍스트 잘림을 지원 하지 않는 TextBlock에 도구 설명을 추가 합니다.

``` xaml
<TextBlock TextTrimming="Clip">
    <ToolTipService.ToolTip>
        <ToolTip Content="The quick brown fox jumped over the lazy dog."/>
    </ToolTipService.ToolTip>
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

### <a name="dont-scale-font-based-icons-or-symbols"></a>글꼴 기반 아이콘 또는 기호의 크기를 조정 하지 않음

강조 또는 장식을 위한 글꼴 기반 아이콘을 사용 하는 경우 이러한 문자에 대 한 크기 조정을 사용 하지 않도록 설정 합니다.

대부분의 XAML 컨트롤에 대해 [IsTextScaleFactorEnabled](/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled) 속성을로 설정 `false` 합니다.

### <a name="support-text-scaling-natively"></a>기본적으로 텍스트 크기 조정 지원

사용자 지정 프레임 워크 및 컨트롤에서 [TextScaleFactorChanged](/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) uisettings 시스템 이벤트를 처리 합니다. 이 이벤트는 사용자가 시스템에서 텍스트 배율 인수를 설정 하는 때마다 발생 합니다.

## <a name="summary"></a>요약

이 항목에서는 Windows의 텍스트 크기 조정 지원에 대 한 개요를 제공 하 고 사용자 환경을 사용자 지정 하는 방법에 대 한 UX 및 개발자 지침을 제공 합니다.

## <a name="related-articles"></a>관련된 문서

### <a name="api-reference"></a>API 참조

- [IsTextScaleFactorEnabled](/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [TextScaleFactorChanged](/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)
