---
author: Karl-Bridge-Microsoft
Description: Build UWP apps and custom/templated controls that support platform text scaling.
title: 텍스트 크기 조정
label: Text scaling
template: detail.hbs
keywords: UWP, 텍스트, 크기 조정, 접근성, "접근성", 표시, "만들기 text 큰", 사용자 조작, 입력
ms.author: kbridge
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ce3ec15a45f812162c7aab0cb9683183d7196ae3
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5931688"
---
# <a name="text-scaling"></a>텍스트 크기 조정

![225%로 100% 배율 텍스트의 예](images/coretext/text-scaling-news-hero-small.png)  
*Windows 10 (225%로 100%)의 크기 조정 하는 텍스트의 예*

## <a name="overview"></a>개요

많은 사람들이 (Surface Hub의 거 대 한 화면에 데스크톱 모니터에 노트북에 모바일 장치)에서 컴퓨터 화면에서 텍스트 읽기 쉽지 않을 수 있습니다. 반대로, 일부 사용자는 앱과 웹 사이트에서는 데 필요한 것 보다 클 글꼴 크기를 찾습니다.

텍스트는 광범위 한 사용자에 대 한 가능한 읽힙니다 위해, Windows 운영 체제와 개별 응용 프로그램 간에 상대 글꼴 크기를 변경 하는 사용자 기능을 제공 합니다. (있으며 일반적으로 방금 화면 영역 내에서 모든 확대 고유한 유용성 문제가) 돋보기 앱을 사용 하 여, 디스플레이 해상도 변경 하거나 (디스플레이 및 일반적인 가시 기반으로 모든 항목의 크기를 조정 된 DPI 배율에 의존 하는 대신 거리), 사용자는 100% (기본값)에서 사이의 텍스트만 크기를 조정 하는 설정을 빠르게 액세스할 수 최대 225%입니다.

## <a name="support"></a>지원

유니버설 Windows 응용 프로그램 (표준 및 PWA), 기본적으로 크기 조정 하는 텍스트를 지원 합니다.

UWP 응용 프로그램 사용자 지정 컨트롤, 사용자 지정 텍스트 표면, 하드 코딩 된 컨트롤 높이, 이전 프레임 워크 또는 타사 프레임 워크가 있으면 가능성이 사용자에 게 유용 하 게 일관 된 환경을 제공 하기 위해 일부 업데이트 해야 합니다.  

DirectWrite, GDI 및 XAML SwapChainPanels 지원 하지 않습니다 기본적으로 텍스트 크기 조정, Win32 지원은 메뉴, 아이콘 및 도구 모음을 제한 하는 동안.  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>사용자 환경

사용자가 텍스트 비율을 조정할 수 접근성 설정에 더 큰 슬라이더-> 만들기 텍스트로 비전/디스플레이 화면-> 합니다.

![225%로 100% 배율 텍스트의 예](images/coretext/text-scaling-settings-100-small.png)  
*설정에서 설정 하는 텍스트 배율 접근성-> 비전/디스플레이 화면->*

## <a name="ux-guidance"></a>UX 지침

텍스트 크기를 조정할 때 컨트롤 및 컨테이너 해야도 크기를 조정 하 고 텍스트 및 새 레이아웃에 맞게 재배치 합니다. 앱, 프레임 워크 및 플랫폼에 따라 이전에 설명한 것 처럼이 작업의 대부분을 수행 됩니다. 다음 UX 지침 없는 이러한 경우를 다룹니다.

### <a name="use-the-platform-controls"></a>플랫폼 컨트롤 사용

않은 것 들이 이미? 반복 이므로: 가능한 경우 항상를 사용 하 여 다양 한 Windows 앱 프레임 워크에서 제공 하는 기본 제공 컨트롤 최소한의 노력에 대해 가능한 가장 포괄적인 사용자 경험을 가져옵니다.

예를 들어 모든 UWP 텍스트 컨트롤 템플릿 이나 사용자 지정 없이도 환경을 배율 전체 텍스트를 지원 합니다.

표준 텍스트 컨트롤의 몇 가지를 포함 하는 기본 UWP 앱에서 코드 조각을 다음과 같습니다.

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

![애니메이션 효과 준된 텍스트 225%로 100% 배율](images/coretext/text-scaling.gif)  
*애니메이션 효과 준된 텍스트 크기 조정*

### <a name="use-auto-sizing"></a>자동 크기 조정을 사용합니다

컨트롤에 대 한 절대 크기를 지정 하지 마십시오. 가능한 경우 사용자 및 디바이스 설정에 따라 자동으로 컨트롤의 크기를 조정 하는 플랫폼 수 있도록 합니다.  

이전 예제에서 다음이 코드를 사용 하 여는 `Auto` 및 `*` 집합 그리드 열과 수 있도록 플랫폼의 너비 값 그리드 내에 포함 된 요소 크기에 따라 앱 레이아웃을 조정 합니다.

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>텍스트 줄 바꿈을 사용합니다

앱의 레이아웃은 다음과 같이 유연 하 고 최대한 조정할 수 있도록 되도록 (많은 컨트롤 지원 하지 않는 텍스트 줄 바꿈을 기본적으로) 텍스트를 포함 하는 모든 컨트롤에 텍스트 줄 바꿈을 사용 합니다.

플랫폼 클리핑 포함 하 여 레이아웃을 조정 하려면 다른 메서드를 사용 하 텍스트 배치를 지정 하지 않으면, (이전 예제 참조).

여기서 사용 하는 `AcceptsReturn` 및 `TextWrapping` 레이아웃 맞춰지도록 되도록 TextBox 속성은 최대한으로 유연 합니다.

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![텍스트 배치를 사용 하 여 225%로 100% 배율 텍스트 애니메이션](images/coretext/text-scaling-textwrap.gif)  
*텍스트 배치를 사용 하 여 배율 애니메이션된 텍스트*

### <a name="specify-text-trimming-behavior"></a>텍스트 자르기 동작 지정

텍스트 줄 바꿈을 기본 동작 없는 경우 대부분의 텍스트 컨트롤에 텍스트를 클리핑 또는 텍스트 자르기 동작에 대 한 줄임표를 지정할 수 있습니다. 클리핑은 줄임표 공간을 차지 자체는 줄임표 하는 것이 좋습니다.

> [!NOTE]
> 텍스트를 클리핑 해야 하는 경우 시작이 아닌 문자열의 끝을 클립입니다.

이 예제에서는 보여 줍니다 TextBlock의 텍스트를 클리핑 하는 방법을 [TextTrimming](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.texttrimming) 속성을 사용 하 여.

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![텍스트 클리핑이 있는 100% ~ 225% 배율 텍스트](images/coretext/text-scaling-clipping-small.png)  
*텍스트 텍스트 클리핑이 있는 크기 조정*

### <a name="use-a-tooltip"></a>도구 설명을 사용합니다

텍스트를 클리핑 도구 설명을 사용 하 여 사용자에 게 전체 텍스트를 제공 합니다.

여기서 도구 설명 텍스트 배치를 지원 하지 않는 TextBlock에 추가:

``` xaml
<TextBlock TextTrimming="Clip">
    <ToolTipService.ToolTip>
        <ToolTip Content="The quick brown fox jumped over the lazy dog."/>
    </ToolTipService.ToolTip>
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

### <a name="dont-scale-font-based-icons-or-symbols"></a>글꼴 기반 아이콘 또는 기호 조정

강조 또는 장식 글꼴 기반 아이콘을 사용할 때 이러한 문자에 크기 조정을 사용 하지 않도록 설정 합니다.

[IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled) 속성을 설정 `false` 대부분의 XAML을 제어 합니다.

### <a name="support-text-scaling-natively"></a>기본적으로 크기 조정 지원 텍스트

사용자 지정 프레임 워크 및 컨트롤에서 [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) UISettings 시스템 이벤트를 처리 합니다. 이 이벤트는 사용자가 시스템에 텍스트 배율 설정 될 때마다 발생 합니다.

## <a name="summary"></a>요약

이 항목에서는 배율 Windows에서 지원 되는 텍스트의 개요를 제공 하 고 사용자 환경을 사용자 지정 하는 방법에 대 한 UX 및 개발자 지침을 포함 합니다.

## <a name="related-articles"></a>관련 문서

### <a name="api-reference"></a>API 참조

- [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)
