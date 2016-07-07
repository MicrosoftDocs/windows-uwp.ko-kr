---
author: mcleblanc
ms.assetid: 569E8C27-FA01-41D8-80B9-1E3E637D5B99
title: "XAML 태그 최적화"
description: "메모리에서 개체를 생성하기 위해 XAML 태그를 구문 분석하는 작업은 복잡한 UI의 경우 시간이 많이 걸립니다. 다음은 XAML 태그 구문 분석 및 로드 시간과 앱의 메모리 효율성을 개선하기 위해 수행할 수 있는 몇 가지 작업입니다."
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: c2131b084d8bb989f1f7767f54db697e1cdd8dcf

---
# XAML 태그 최적화

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

메모리에서 개체를 생성하기 위해 XAML 태그를 구문 분석하는 작업은 복잡한 UI의 경우 시간이 많이 걸립니다. 다음은 XAML 태그 구문 분석 및 로드 시간과 앱의 메모리 효율성을 개선하기 위해 수행할 수 있는 몇 가지 작업입니다.

앱 시작 시 로드되는 XAML 태그를 초기 UI에 필요한 태그로만 제한합니다. 초기 페이지에서 태그를 검사하고 필요 없는 태그가 포함되어 있지 않은지 확인합니다. 페이지가 다른 파일에 정의된 사용자 컨트롤이나 리소스를 참조하는 경우 프레임워크는 해당 파일도 구문 분석합니다.

이 예제에서는 InitialPage.xaml에서 ExampleResourceDictionary.xaml의 리소스 하나를 사용하므로 시작 시 전체 ExampleResourceDictionary.xaml을 구문 분석해야 합니다.

**InitialPage.xaml.**

```xml
<Page x:Class="ExampleNamespace.InitialPage" ...> 
    <UserControl.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="ExampleResourceDictionary.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </UserControl.Resources>

    <Grid>
        <TextBox Foreground="{StaticResource TextBrush}"/>
    </Grid>
</Page>
```

**ExampleResourceDictionary.xaml.**

```xml
<ResourceDictionary>
    <SolidColorBrush x:Key="TextBrush" Color="#FF3F42CC"/>

    <!--This ResourceDictionary contains many other resources that
    used in the app, but not during startup.-->
</ResourceDictionary>
```

앱 전반에 걸쳐 여러 페이지의 리소스를 사용하는 경우 App.xaml에 저장하는 것이 가장 좋으며, 중복을 방지합니다. 그러나 App.xaml은 앱 시작 시 구문 분석되므로 하나의 페이지(초기 페이지가 아닌 경우)에서만 사용되는 리소스는 해당 페이지의 로컬 리소스에 두어야 합니다. 이와 반대되는 예제에서는 하나의 페이지(초기 페이지가 아닌)에서 사용되는 리소스가 포함된 App.xaml을 보여 줍니다. 이 경우 앱 시작 시간이 불필요하게 증가합니다.

**InitialPage.xaml.**

```xml
<Page ...>  <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->   
    <StackPanel>  
        <TextBox Foreground="{StaticResource InitialPageTextBrush}"/> 
    </StackPanel> 
</Page> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**SecondPage.xaml.**

```xml
<Page ...>  <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
    <StackPanel> 
        <Button Content="Submit" Foreground="{StaticResource SecondPageTextBrush}" /> 
    </StackPanel> 
</Page> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**App.xaml**

```xml
<Application ...> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
     <Application.Resources>  
        <SolidColorBrush x:Key="DefaultAppTextBrush" Color="#FF3F42CC"/> 
        <SolidColorBrush x:Key="InitialPageTextBrush" Color="#FF3F42CC"/> 
        <SolidColorBrush x:Key="SecondPageTextBrush" Color="#FF3F42CC"/> 
        <SolidColorBrush x:Key="ThirdPageTextBrush" Color="#FF3F42CC"/> 
    </Application.Resources> 
</Application> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

위의 반대 예제를 보다 효율적으로 만들려면 `SecondPageTextBrush`를 SecondPage.xaml로 이동하고 `ThirdPageTextBrush`를 ThirdPage.xaml로 이동하면 됩니다. `InitialPageTextBrush` (은)는 항상 앱 시작 시 응용 프로그램 리소스를 구문 분석해야 하므로 App.xaml에서 유지할 수 있습니다.

## 요소 수 최소화

XAML 플랫폼은 많은 요소를 표시할 수 있지만 원하는 시각 효과를 달성하려면 최소한의 요소를 사용하여 앱 배치 및 렌더링을 보다 빠르게 만들어야 합니다.

-   레이아웃 패널에는 [**Background**](https://msdn.microsoft.com/library/windows/apps/BR227512) 속성이 있으므로 색을 지정하기 위해 패널 앞에 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371)을 둘 필요가 없습니다.

**비효율적인 경우**

```xml
<Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
        <Rectangle Fill="Black"/> 
    </Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**효율적인 경우**

```xml
<Grid Background="Black"/>
```

-   동일한 벡터 기반 요소를 다시 사용할 시간이 충분한 경우 [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) 요소를 대신 사용하는 것이 보다 효율적입니다. 벡터 기반 요소는 CPU에서 각 개별 요소를 별도로 만들어야 하므로 비용이 더 많이 소요될 수 있습니다. 이미지 파일을 한 번만 디코딩해야 합니다.

## 같은 모양의 여러 브러시를 하나의 리소스에 통합

XAML 플랫폼은 공통적으로 사용되는 개체를 가능한 자주 다시 사용할 수 있도록 캐시하려고 합니다. 그러나 XAML은 하나의 태그 조각에 선언된 브러시가 다른 태그 조각에 선언된 브러시와 동일한지 쉽게 구별할 수 없습니다. 다음 예제에서는 보여 주기 위해 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)를 사용하지만 [**GradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210068)를 사용하는 것이 더 중요하고 더 일반적입니다.

**비효율적인 경우**

```xml
<Page ... > <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
    <StackPanel>  
        <TextBlock>  
            <TextBlock.Foreground>  
                <SolidColorBrush Color="#FFFFA500"/> 
            </TextBlock.Foreground> 
        </TextBox> 
        <Button Content="Submit"> 
            <Button.Foreground> 
                <SolidColorBrush Color="#FFFFA500"/> 
            </Button.Foreground> 
        </Button> 
    </StackPanel> 
</Page> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

미리 정의된 색을 사용하는 브러시를 확인합니다. `"Orange"`와 `"#FFFFA500"`은 같은 색입니다. 중복을 해결하려면 브러시를 리소스로 정의합니다. 다른 페이지의 컨트롤에서 동일한 브러시를 사용하는 경우 브러시를 App.xaml로 이동합니다.

**효율적인 경우**

```xml
<Page ... >
    <Page.Resources>
        <SolidColorBrush x:Key="BrandBrush" Color="#FFFFA500"/>
    </Page.Resources>
    
    <StackPanel>
        <TextBlock Foreground="{StaticResource BrandBrush}" />
        <Button Content="Submit" Foreground="{StaticResource BrandBrush}" />
    </StackPanel>
</Page>
```

## 과도한 그리기 최소화

과도한 그리기는 둘 이상의 개체가 동일한 화면 픽셀에 그려지는 경우입니다. 이 지침과 요소 수 최소화 간에는 간혹 상충 관계가 있습니다.

-   요소가 투명하거나 다른 요소 뒤에 숨겨져 있어 보이지 않고 레이아웃에 참여하지 않는 경우 해당 요소를 삭제합니다. 요소가 초기 시각적 상태에서는 보이지 않지만 다른 시각적 상태에서는 보이는 경우 요소 자체에 대한 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992)를 **Collapsed**로 설정하고 적절한 상태에서 값을 **Visible**로 변경합니다. 이 추론에 대한 예외가 있습니다. 일반적으로 대부분의 시각적 상태에서는 속성 값을 요소에서 로컬로 설정하는 것이 가장 좋습니다.
-   여러 요소를 계층화하는 대신 복합 요소를 사용하여 효과를 만듭니다. 이 예제에서는 위쪽 절반은 검은색([**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)의 배경에서)이고 아래쪽 절반은 회색(**Grid**의 검은색 배경 위에 알파 혼합된 반투명 흰색의 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371)에서)인 두 가지 색조의 모양이 만들어집니다. 여기에서는 결과를 달성하는 데 필요한 픽셀의 150%가 채워집니다.

**비효율적인 경우**
    
```xml
    <Grid Background="Black"> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
        <Grid.RowDefinitions> 
            <RowDefinition Height="*"/> 
            <RowDefinition Height="*"/> 
        </Grid.RowDefinitions> 
        <Rectangle Grid.Row="1" Fill="White" Opacity=".5"/> 
    </Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**효율적인 경우**

```xml
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <Rectangle Fill="Black"/>
        <Rectangle Grid.Row="1" Fill="#FF7F7F7F"/>
    </Grid>
```

-   레이아웃 패널은 영역의 색을 지정하고 자식 요소를 배치하는 두 가지 용도로 사용됩니다. 다시 z 순서로 지정된 요소가 영역의 색을 이미 지정하는 경우 앞쪽의 레이아웃 패널에서는 해당 영역을 다시 그릴 필요가 없으며 대신 해당 자식을 배치하는 데 중점을 둘 수 있습니다. 예를 들면 다음과 같습니다.

**비효율적인 경우**

```xml
    <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
    <GridView Background="Blue">  
        <GridView.ItemTemplate> 
            <DataTemplate> 
                <Grid Background="Blue"/> 
            </DataTemplate> 
        </GridView.ItemTemplate> 
    </GridView> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**효율적인 경우**

```xml
    <GridView Background="Blue">  
        <GridView.ItemTemplate> 
            <DataTemplate> 
                <Grid/> 
            </DataTemplate>
        </GridView.ItemTemplate>
    </GridView> 
```

[**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)가 적중 횟수를 테스트할 수 있어야 하는 경우 그 위에 투명한 배경색을 설정합니다.

-   [**Border**](https://msdn.microsoft.com/library/windows/apps/BR209253) 요소를 사용하여 개체 주위의 테두리를 그립니다. 이 예제에서는 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)를 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 주위의 임시 테두리로 사용합니다. 그러나 가운데 셀에 있는 모든 픽셀은 과도하게 그려집니다.

**비효율적인 경우**

```xml
    <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
    <Grid Background="Blue" Width="300" Height="45"> 
        <Grid.RowDefinitions> 
            <RowDefinition Height="5"/> 
            <RowDefinition/> 
            <RowDefinition Height="5"/> 
        </Grid.RowDefinitions> 
        <Grid.ColumnDefinitions> 
            <ColumnDefinition Width="5"/> 
            <ColumnDefinition/> 
            <ColumnDefinition Width="5"/> 
        </Grid.ColumnDefinitions> 
        <TextBox Grid.Row="1" Grid.Column="1"></TextBox> 
    </Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**효율적인 경우**

```xml
    <Border BorderBrush="Blue" BorderThickness="5" Width="300" Height="45">
        <TextBox/>
    </Border>
```

-   여백에 주의합니다. 음수 여백이 다른 요소의 렌더링 범위로 확장되어 과도한 그리기가 발생하는 경우 인접한 두 요소가 실수로 겹쳐질 수 있습니다.

[**DebugSettings.IsOverdrawHeatMapEnabled**](https://msdn.microsoft.com/library/windows/apps/Hh701823)를 시각적 진단으로 사용합니다. 장면에서 인식하지 못한 개체가 그려지는 경우가 있을 수 있습니다.

## 정적 콘텐츠 캐시

과도한 그리기의 또 다른 원인은 하나의 모양이 겹쳐진 여러 요소에서 만들어지는 경우입니다. 복합 모양이 포함된 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911)에서 [**CacheMode**](https://msdn.microsoft.com/library/windows/apps/BR228084)를 **BitmapCache**로 설정한 경우 플랫폼은 요소를 비트맵으로 렌더링한 다음 각 프레임에서 과도한 그리기 대신 해당 비트맵을 사용합니다.

**비효율적인 경우**

```xml
<Canvas Background="White">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

![단색 원 세 개가 있는 벤 다이어그램](images/solidvenn.png)

위 이미지가 결과이지만 과도하게 그려진 영역의 맵은 다음과 같습니다. 빨간색이 진할수록 과도하게 그려진 정도가 높습니다.

![겹친 영역을 보여 주는 벤 다이어그램](images/translucentvenn.png)

**효율적인 경우**

```xml
<Canvas Background="White" CacheMode="BitmapCache">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

[**CacheMode**](https://msdn.microsoft.com/library/windows/apps/BR228084)의 사용에 주의합니다. 하위 모양을 애니메이션하는 경우 모든 프레임에서 비트맵 캐시를 다시 생성해야 할 수 있으므로 이 기술을 사용해서는 안 됩니다.

## ResourceDictionaries

ResourceDictionaries는 일반적으로 전역 수준에서 리소스를 저장하는 데 사용됩니다. 앱에서 여러 위치에서 참조하려는 리소스로서 예를 들어 스타일, 브러시, 템플릿 등이 포함됩니다. 일반적으로 요청되지 않은 경우 리소스를 인스턴스화하지 않도록 ResourceDictionaries를 최적화했습니다. 그러나 약간 주의가 필요한 몇몇 위치가 있습니다.

**x:Name이 포함된 리소스**. x:Name이 포함된 리소스는 플랫폼 최적화의 이점이 적용되지 않는 대신 ResourceDictionary가 만들어지면 곧바로 인스턴스화됩니다. 이렇게 되는 이유는 x:Name에서 앱에 이 리소스에 대한 필드 액세스가 필요함을 플랫폼에 알리므로 플랫폼이 참조를 생성할 관련 항목을 만들어야 하기 때문입니다.

**UserControl의 ResourceDictionaries**. UserControl 내부에서 정의된 ResourceDictionaries에는 페널티가 있습니다. 플랫폼에서는 UserControl의 모든 인스턴스에 대해 이러한 ResourceDictionary의 복사본을 만듭니다. 많이 사용되는 UserControl이 있는 경우에는 ResourceDictionary를 UserControl 외부로 이동하여 페이지 수준에 배치합니다.

## XBF2 사용

XBF2는 런타임 시 모든 텍스트 구문 분석을 방지하는 XAML 태그의 이진 표현입니다. 또한 부하 및 트리 생성을 위한 이진 파일을 최적화하고 XAML 유형에 대해 "빠른 경로"를 허용하여 VSM, ResourceDictionary, 스타일 등과 같은 힙 및 개체 생성 비용을 개선합니다. 메모리가 완전히 매핑되었으므로 XAML 페이지 로드 및 읽기를 위한 힙 공간이 없습니다. 뿐만 아니라 appx에서 저장된 XAML 페이지의 디스크 공간이 줄어듭니다. XBF2는 보다 압축된 표현이며 비교되는 XAML/XBF1 파일의 디스크 공간을 최대 50%까지 줄일 수 있습니다. 예를 들어 ~1mb 정도의 XBF1 자산에서 ~400kb의 XBF2 자산으로 XBF2 삭제 변환 이후 기본 제공 사진 앱에서 디스크 공간이 60% 줄었습니다. 앱의 CPU 공간이 15~20%, Win32 힙이 10~15% 정도 개선되었습니다.

XAML 기본 제공 컨트롤 및 프레임워크에서 제공되는 사전은 이미 XBF2가 전적으로 지원됩니다. 고유한 앱의 경우 프로젝트 파일에서 TargetPlatformVersion 8.2 이상을 선언해야 합니다.

XBF2가 있는지를 확인하려면 바이너리 편집기에서 앱을 엽니다. XBF2가 있는 경우 12번째와 13번째 바이트가 00 02입니다.




<!--HONumber=Jun16_HO4-->


