---
Description: 목록 보기의 항목 템플릿
title: 목록 보기의 항목 템플릿
template: detail.hbs
ms.date: 11/03/2017
ms.topic: article
keywords: windows 10, uwp, fluent
ms.openlocfilehash: 9328c3f156acd13fd8947e01e924bf0d6849c0a6
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75684400"
---
# <a name="item-templates-for-list-view"></a>목록 보기의 항목 템플릿

이 섹션에는 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 컨트롤로 사용할 수 있는 항목 템플릿이 포함되어 있습니다. 이 템플릿을 사용하여 일반적인 앱 유형의 모양을 확인합니다. 

데이터 바인딩을 보여 주기 위해 이 템플릿은 [데이터 바인딩 개요](../../data-binding/data-binding-quickstart.md)의 예제 Recording 클래스에 **ListViewItems**를 바인딩합니다.

> [!NOTE] 
> 현재 **DataTemplate**에 여러 컨트롤(예: 둘 이상의 단일 **TextBlock**)이 포함되어 있는 경우, 화면 판독기의 기본 액세스 가능 이름은 항목의 .ToString()에서 제공됩니다. 편의상 **DataTemplate**의 루트 요소에 있는 [**AutomationProperties.Name**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties)을 대신 설정할 수 있습니다. 접근성에 대한 자세한 내용은 [접근성 개요](../accessibility/accessibility-overview.md)를 참조하세요.

## <a name="single-line-list-item"></a>한 줄 목록 항목
이 템플릿을 사용하여 이미지와 한 줄 텍스트가 있는 항목 목록을 표시합니다.

![한 줄 목록 항목 예제](images/listitems/singlelineexample.png)
![한 줄 목록 항목](images/listitems/singlelineicon.png)
```xaml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}">
    <ListView.ItemTemplate>
        <DataTemplate x:Name="SingleLineDataTemplate" x:DataType="local:Recording">
            <StackPanel Orientation="Horizontal" Height="44" Padding="12" AutomationProperties.Name="{x:Bind CompositionName}">
                <Image Source="Placeholder.png" Height="16" Width="16" VerticalAlignment="Center"/>
                <TextBlock Text="{x:Bind CompositionName}" VerticalAlignment="Center" Style="{ThemeResource BaseTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseHighBrush}" Margin="12,0,0,0"/>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

## <a name="double-line-list-item"></a>두 줄 목록 항목 
이 템플릿을 사용하여 이미지와 두 줄 텍스트가 있는 항목 목록을 표시합니다.

![아이콘이 있는 두 줄 목록 항목 예제](images/listitems/doublelineexample.png) 
![아이콘이 있는 두 줄 목록 항목](images/listitems/doublelineicon.png)

```xaml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}">
    <ListView.ItemTemplate>
        <DataTemplate x:Name="DoubleLineDataTemplate" x:DataType="local:Recording">
            <StackPanel Orientation="Horizontal" Height="64" AutomationProperties.Name="{x:Bind CompositionName}">
                <Ellipse Height="48" Width="48" VerticalAlignment="Center">
                    <Ellipse.Fill>
                        <ImageBrush ImageSource="Placeholder.png"/>
                    </Ellipse.Fill>
                </Ellipse>
                <StackPanel Orientation="Vertical" VerticalAlignment="Center" Margin="12,0,0,0">
                    <TextBlock Text="{x:Bind CompositionName}"  Style="{ThemeResource BaseTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseHighBrush}" />
                    <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource BodyTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseMediumBrush}"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

## <a name="triple-line-list-item"></a>세 줄 목록 항목
이 템플릿을 사용하여 세 줄 텍스트가 있는 항목 목록을 표시합니다.

![세 줄 목록 항목 예제](images/listitems/triplelineexample.png)
![세 줄 목록 항목](images/listitems/tripleline.png)

```xaml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}">
    <ListView.ItemTemplate>
        <DataTemplate x:Name="TripleLineDataTemplate" x:DataType="local:Recording">
            <StackPanel Height="84" Padding="20" AutomationProperties.Name="{x:Bind CompositionName}">
                <TextBlock Text="{x:Bind CompositionName}"  Style="{ThemeResource BaseTextBlockStyle}" Margin="0,4,0,0"/>
                <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource CaptionTextBlockStyle}" Opacity=".8" Margin="0,4,0,0"/>
                <TextBlock Text="{x:Bind ReleaseDateTime}" Style="{ThemeResource CaptionTextBlockStyle}" Opacity=".6" Margin="0,4,0,0"/>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

## <a name="table-list-item"></a>테이블 목록 항목
이 템플릿을 사용하여 정의된 열에 텍스트가 있는 항목 목록을 표시합니다.

![테이블 목록 항목 예제](images/listitems/tablelist.png)
```xaml
<ListView  ItemsSource="{x:Bind ViewModel.Recordings}">
    <ListView.HeaderTemplate>
        <DataTemplate>
            <Grid Padding="12" Background="{ThemeResource SystemBaseLowColor}">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="408"/>
                    <ColumnDefinition Width="360"/>
                    <ColumnDefinition Width="360"/>
                </Grid.ColumnDefinitions>
                <TextBlock Text="Composition" Style="{ThemeResource CaptionTextBlockStyle}"/>
                <TextBlock Grid.Column="1" Text="Artist" Style="{ThemeResource CaptionTextBlockStyle}"/>
                <TextBlock Grid.Column="2" Text="Release Date" Style="{ThemeResource CaptionTextBlockStyle}"/>
            </Grid>
        </DataTemplate>
    </ListView.HeaderTemplate>
    <ListView.ItemTemplate>
        <DataTemplate x:Name="TableDataTemplate" x:DataType="local:Recording">
            <Grid Height="48" AutomationProperties.Name="{x:Bind CompositionName}">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="48"/>
                    <ColumnDefinition Width="360"/>
                    <ColumnDefinition Width="360"/>
                    <ColumnDefinition Width="360"/>
                </Grid.ColumnDefinitions>
                <Ellipse Height="32" Width="32" VerticalAlignment="Center">
                    <Ellipse.Fill>
                        <ImageBrush ImageSource="Placeholder.png"/>
                    </Ellipse.Fill>
                </Ellipse>
                <TextBlock Grid.Column="1" VerticalAlignment="Center" Style="{ThemeResource BaseTextBlockStyle}" Text="{x:Bind CompositionName}" />
                <TextBlock Grid.Column="2" VerticalAlignment="Center" Text="{x:Bind ArtistName}"/>
                <TextBlock Grid.Column="3" VerticalAlignment="Center" Text="{x:Bind ReleaseDateTime}"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

## <a name="related-articles"></a>관련된 문서
- [ListView 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)
- [데이터 바인딩 개요](../../data-binding/data-binding-quickstart.md)
- [접근성 개요](../accessibility/accessibility-overview.md)
- [ListView 및 GridView 샘플(Windows 10)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)
- [미리 보기 이미지](../../files/thumbnails.md)
