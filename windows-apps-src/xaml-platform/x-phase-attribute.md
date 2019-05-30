---
title: xPhase 특성
description: xBind 태그 확장과 함께 xPhase를 사용하여 ListView 및 GridView 항목을 증분적으로 렌더링하고 이동 환경을 개선할 수 있습니다.
ms.assetid: BD17780E-6A34-4A38-8D11-9703107E247E
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dc1ec762e5c6f69db608805ac58cfb9469114beb
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372276"
---
# <a name="xphase-attribute"></a>x:Phase 특성


[{x:Bind} 태그 확장](x-bind-markup-extension.md)과 함께 **x:Phase**를 사용하여 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 및 [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) 항목을 증분적으로 렌더링하고 이동 환경을 개선할 수 있습니다. **x:Phase**는 [**ContainerContentChanging**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.containercontentchanging) 이벤트를 사용하여 목록 항목의 렌더링을 수동으로 제어할 때와 동일한 효과를 얻을 수 있는 선언적 방법입니다. [ListView 및 GridView 항목의 증분 업데이트](../debug-test-perf/optimize-gridview-and-listview.md#update-items-incrementally)를 참조하세요.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용


``` syntax
<object x:Phase="PhaseValue".../>
```

## <a name="xaml-values"></a>XAML 값


| 용어 | 설명 |
|------|-------------|
| PhaseValue | 요소가 처리되는 단계를 나타내는 숫자입니다. 기본값은 0입니다. | 

## <a name="remarks"></a>설명

터치 또는 마우스 휠을 사용하여 목록을 빠르게 이동할 경우 데이터 템플릿의 복잡성에 따라 목록이 스크롤 속도만큼 빠르게 항목을 렌더링하지 못할 수도 있습니다. 휴대폰 또는 태블릿과 같은 전원 효율적인 CPU를 사용하는 휴대용 장치에서 특히 그렇습니다.

단계를 사용하면 콘텐츠의 우선 순위를 지정하여 가장 중요한 요소가 먼저 렌더링되도록 데이터 템플릿의 증분 렌더링이 가능합니다. 이렇게 하면 빠르게 이동할 경우 각 항목에 대한 일부 콘텐츠가 목록에 표시되고 시간이 허용되면 각 템플릿의 더 많은 요소가 렌더링됩니다.

## <a name="example"></a>예제

```xml
<DataTemplate x:Key="PhasedFileTemplate" x:DataType="model:FileItem">
    <Grid Width="200" Height="80">
        <Grid.ColumnDefinitions>
           <ColumnDefinition Width="75" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        <Image Grid.RowSpan="4" Source="{x:Bind ImageData}" MaxWidth="70" MaxHeight="70" x:Phase="3"/>
        <TextBlock Text="{x:Bind DisplayName}" Grid.Column="1" FontSize="12"/>
        <TextBlock Text="{x:Bind prettyDate}"  Grid.Column="1"  Grid.Row="1" FontSize="12" x:Phase="1"/>
        <TextBlock Text="{x:Bind prettyFileSize}"  Grid.Column="1"  Grid.Row="2" FontSize="12" x:Phase="2"/>
        <TextBlock Text="{x:Bind prettyImageSize}"  Grid.Column="1"  Grid.Row="3" FontSize="12" x:Phase="2"/>
    </Grid>
</DataTemplate>
```

데이터 템플릿에서는 다음 4단계를 설명합니다.

1.  DisplayName 텍스트 블록을 제공합니다. 지정된 단계가 없는 모든 컨트롤은 암시적으로 단계 0의 일부로 간주됩니다.
2.  prettyDate 텍스트 블록을 표시합니다.
3.  prettyFileSize 및 prettyImageSize 텍스트 블록을 표시합니다.
4.  이미지를 표시합니다.

단계는 [**ListViewBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewBase)에서 파생된 컨트롤과 함께 작동하고 데이터 바인딩에 대해 항목 템플릿을 증분적으로 처리하는 [{x:Bind}](x-bind-markup-extension.md)의 기능입니다. 목록 항목을 렌더링할 때 **ListViewBase**는 보기의 모든 항목에 대한 단일 단계를 렌더링한 후 다음 단계로 이동합니다. 렌더링 작업은 목록을 스크롤할 때 필요한 작업을 다시 평가할 수 있도록 시각 조각 일괄 처리로 수행되며, 더 이상 표시되지 않는 항목에 대해서는 수행되지 않습니다.

**x:Phase** 특성은 [{x:Bind}](x-bind-markup-extension.md)를 사용하는 데이터 템플릿의 모든 요소에 지정할 수 있습니다. 요소에 0이 아닌 단계가 있으면 해당 단계가 처리되고 바인딩이 업데이트될 때까지 보기에서 요소가 숨겨집니다(**Visibility**가 아니라 **Opacity**를 통해). [  **ListViewBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewBase)에서 파생된 컨트롤을 스크롤하면 더 이상 화면에 없는 목록에서 항목 템플릿을 재순환하여 새로 표시된 항목을 렌더링합니다. 템플릿 내의 UI 요소는 다시 데이터 바인딩될 때까지 이전 값을 유지합니다. 단계는 데이터 바인딩 단계를 지연시킬 수 있으므로 UI 요소가 부실한 경우 숨겨야 합니다.

각 UI 요소에 하나의 단계만 지정할 수 있습니다. 그러면 요소의 모든 바인딩에 적용됩니다. 단계를 지정하지 않으면 단계 0으로 간주됩니다.

단계 번호는 연속적이지 않아도 되며, [**ContainerContentChangingEventArgs.Phase**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.containercontentchangingeventargs.phase) 값과 같습니다. **x:Phase** 바인딩이 처리되기 전에 각 단계에 대해 [**ContainerContentChanging**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.containercontentchanging) 이벤트가 발생합니다.

단계는 [{x:Bind}](x-bind-markup-extension.md) 바인딩에만 영향을 주며, [{Binding}](binding-markup-extension.md) 바인딩에는 영향을 주지 않습니다.

단계를 인식하는 컨트롤을 사용하여 항목 템플릿을 렌더링하는 경우에만 단계가 적용됩니다. Windows 10에 대 한 의미 [ **ListView** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 하 고 [ **GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)합니다. 다른 항목 컨트롤에서 사용되는 데이터 템플릿이나 [**ContentTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.contenttemplate) 또는 [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) 섹션과 같은 다른 시나리오에는 단계가 적용되지 않습니다. 이러한 경우에는 모든 UI 요소가 한 번에 데이터 바인딩됩니다.

