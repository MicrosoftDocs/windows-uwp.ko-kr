---
Description: 그리드 보기의 항목 템플릿
title: 그리드 보기의 항목 템플릿
template: detail.hbs
ms.date: 11/03/2017
ms.topic: article
keywords: windows 10, uwp, fluent
ms.openlocfilehash: f1cc576bbc7fe7fc326d86072e22b7a385cfeb8b
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63794544"
---
# <a name="item-templates-for-grid-view"></a>그리드 보기의 항목 템플릿

이 섹션에는 [**GridView**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.GridView) 컨트롤로 사용할 수 있는 항목 템플릿이 포함되어 있습니다. 이 템플릿을 사용하여 일반적인 앱 유형의 모양을 확인합니다.

데이터 바인딩을 보여주기 위해 이 템플릿은 [데이터 바인딩 개요](../../data-binding/data-binding-quickstart.md)의 예제 Recording 클래스에 **GridViewItems**를 바인딩합니다.

> [!NOTE] 
> 현재 **DataTemplate**에 여러 컨트롤(예: 둘 이상의 **TextBlock**)이 포함되어 있는 경우 화면 읽기 프로그램의 접근 가능한 기본 이름은 해당 항목의 .ToString()에서 가져옵니다. 편의상 **DataTemplate**의 루트 요소에 있는 [**AutomationProperties.Name**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.automation.automationproperties)을 대신 설정할 수 있습니다. 접근성에 대한 자세한 내용은 [접근성 개요](../accessibility/accessibility-overview.md)를 참조하세요.

## <a name="icon-and-text"></a>아이콘 및 텍스트
이 템플릿을 사용하여 아이콘과 텍스트가 있는 그리드에 앱 컬렉션을 표시합니다.

![작은 아이콘 및 텍스트 gridview 예제](images/listitems/icontext.png)
```xaml
<GridView ItemsSource="{x:Bind ViewModel.Recordings}">
    <GridView.ItemTemplate>
        <DataTemplate x:Name="IconTextTemplate" x:DataType="local:Recording">
            <StackPanel Width="264" Height="48" Padding="12" Orientation="Horizontal" AutomationProperties.Name="{x:Bind CompositionName}">
                <SymbolIcon Symbol="Audio" VerticalAlignment="Top"/>
                <TextBlock Margin="12,0,0,0" Width="204" Text="{x:Bind CompositionName}"/>
            </StackPanel>
        </DataTemplate>
    </GridView.ItemTemplate>
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid MaximumRowsOrColumns="4" Orientation="Horizontal" HorizontalAlignment="Center"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```

![아이콘 및 두 줄 텍스트 gridview 예제](images/listitems/icontext2.png)
```xaml
<GridView ItemsSource="{x:Bind ViewModel.Recordings}">
    <GridView.ItemTemplate>
        <DataTemplate x:Name="IconTextTemplate2" x:DataType="local:Recording">
            <StackPanel Width="264" Height="120" Padding="12" Orientation="Horizontal" AutomationProperties.Name="{x:Bind CompositionName}">
                <FontIcon Margin="0,6,0,0" FontSize="48" FontFamily="Segoe MDL2 Assets" FontWeight="Bold" Glyph="&#xE8D6;" VerticalAlignment="Top"/>
                <StackPanel Margin="16,1,0,0">
                    <TextBlock Width="176" Margin="0,0,0,2" TextWrapping="WrapWholeWords" TextTrimming="Clip" Text="{x:Bind CompositionName}"/>
                    <TextBlock Width="176" Height="48" Style="{ThemeResource CaptionTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseMediumBrush}" TextWrapping="WrapWholeWords" TextTrimming="Clip" Text="{x:Bind ArtistName}" />
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </GridView.ItemTemplate>
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid MaximumRowsOrColumns="4" Orientation="Horizontal" HorizontalAlignment="Center" Margin="40,0"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```

## <a name="image-gallery"></a>이미지 갤러리
이 템플릿을 사용하여 다중 선택 모드의 그리드에 이미지 컬렉션을 표시합니다.

![gridview 항목 레이아웃](images/listitems/gridviewitems.png)
```xaml
<GridView SelectionMode="Multiple">
    <GridView.ItemTemplate>
        <DataTemplate x:Name="ImageGalleryDataTemplate">
            <Image Source="Placeholder.png" Height="180" Width="180" Stretch="UniformToFill"/>
        </DataTemplate>
    </GridView.ItemTemplate>
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid MaximumRowsOrColumns="3" Orientation="Horizontal"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```
## <a name="image-and-text"></a>이미지 및 텍스트
이 템플릿을 사용하여 아래에 텍스트가 있는 미디어 컬렉션을 표시합니다.

![정사각형 이미지 및 텍스트 gridview 예제](images/listitems/imageandtext.png)
```xaml
<GridView ItemsSource="{x:Bind ViewModel.Recordings}">
    <GridView.ItemTemplate>
        <DataTemplate x:Name="ImageTextDataTemplate" x:DataType="local:Recording">
            <StackPanel Height="280" Width="180" Margin="12" AutomationProperties.Name="{x:Bind CompositionName}">
                <Image Source="Placeholder.png" Height="180" Width="180" Stretch="UniformToFill"/>
                <StackPanel Margin="0,12">
                    <TextBlock Text="{x:Bind CompositionName}"/>
                    <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource CaptionTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseMediumBrush}"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </GridView.ItemTemplate>
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid MaximumRowsOrColumns="10" Orientation="Horizontal"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```

![직사각형 이미지 및 텍스트 gridview 예제](images/listitems/imageandtext2.png)
```xaml
<GridView ItemsSource="{x:Bind ViewModel.Recordings}">
    <GridView.ItemTemplate>
        <DataTemplate x:Name="ImageTextDataTemplate2" x:DataType="local:Recording">
            <StackPanel Height="280" Width="320" Margin="12" AutomationProperties.Name="{x:Bind CompositionName}">
                <Image Source="Placeholder.png" Height="180" Width="320" Stretch="UniformToFill"/>
                <StackPanel Margin="0,12">
                    <TextBlock Text="{x:Bind CompositionName}"/>
                    <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource CaptionTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseMediumBrush}"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </GridView.ItemTemplate>
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid MaximumRowsOrColumns="4" Orientation="Horizontal"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```

## <a name="image-with-text-overlay"></a>텍스트 오버레이가 있는 이미지
이 템플릿을 사용하여 텍스트 오버레이가 있는 미디어 컬렉션을 표시합니다.

![이미지 및 텍스트 오버레이 gridview 예제](images/listitems/imageoverlay.png)
```xaml
<GridView ItemsSource="{x:Bind ViewModel.Recordings}">
    <GridView.ItemTemplate>
        <DataTemplate x:Name="ImageOverlayDataTemplate" x:DataType="local:Recording">
            <Grid Height="180" Width="320" AutomationProperties.Name="{x:Bind CompositionName}">
                <Image Source="Placeholder.png" Stretch="UniformToFill"/>
                <StackPanel Orientation="Vertical" Height="60" VerticalAlignment="Bottom" Background="{ThemeResource SystemBaseLowColor}" Padding="12">
                    <TextBlock Text="{x:Bind CompositionName}"/>
                    <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource CaptionTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseMediumBrush}"/>
                </StackPanel>
            </Grid>
        </DataTemplate>
    </GridView.ItemTemplate>
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid MaximumRowsOrColumns="4" Orientation="Horizontal"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```

## <a name="related-articles"></a>관련 문서
- [GridView 클래스](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.GridView)
- [데이터 바인딩 개요](../../data-binding/data-binding-quickstart.md)
- [접근성 개요](../accessibility/accessibility-overview.md)
- [ListView 및 GridView 샘플(Windows 10)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)
- [미리 보기 이미지](../../files/thumbnails.md)
