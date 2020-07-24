---
title: xPhase 특성
description: Xphase 태그 확장과 함께 xPhase를 사용 하 여 ListView 및 GridView 항목을 증분 방식으로 렌더링 하 고 패닝 환경을 향상 시킵니다.
ms.assetid: BD17780E-6A34-4A38-8D11-9703107E247E
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bf1372289afcc8649fff6c2ed56ad85aad46b76c
ms.sourcegitcommit: e1104689fc1db5afb85701205c2580663522ee6d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997990"
---
# <a name="xphase-attribute"></a>x:Phase 특성


[{X:bind} 태그 확장과](x-bind-markup-extension.md) 함께 **x:phase** 를 사용 하 여 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 및 [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) 항목을 증분 방식으로 렌더링 하 고 패닝 환경을 향상 시킵니다. **X:phase** 는 [**ContainerContentChanging**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.containercontentchanging) 이벤트를 사용 하 여 목록 항목의 렌더링을 수동으로 제어 하는 것과 동일한 결과를 얻는 선언적 방법을 제공 합니다. 또한 [ListView 및 GridView 항목을 증분 업데이트를](../debug-test-perf/optimize-gridview-and-listview.md#update-items-incrementally)참조 하세요.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용


``` syntax
<object x:Phase="PhaseValue".../>
```

## <a name="xaml-values"></a>XAML 값


| 용어 | 설명 |
|------|-------------|
| PhaseValue | 요소가 처리 되는 단계를 나타내는 숫자입니다. 기본값은 0입니다. | 

## <a name="remarks"></a>설명

목록이 터치를 사용 하 여 빠르게 따라 이동 하거나 마우스 휠을 사용 하는 경우 데이터 템플릿의 복잡도에 따라 목록에서 항목을 빠르게 렌더링 하지 못할 수 있습니다. 태블릿 등의 전원 효율적인 CPU가 있는 휴대용 장치의 경우 특히 그렇습니다.

단계적으로 중단를 사용 하면 데이터의 우선 순위를 지정 하 고 가장 중요 한 요소를 먼저 렌더링할 수 있도록 데이터 템플릿 증분 렌더링을 사용할 수 있습니다. 이렇게 하면 목록에서 빠르게 이동 하는 경우 각 항목에 대 한 일부 콘텐츠를 표시할 수 있으며 시간이 허용 될 때 각 템플릿의 요소를 더 많이 렌더링 합니다.

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

데이터 템플릿은 다음 4 단계를 설명 합니다.

1.  DisplayName 텍스트 블록을 표시 합니다. 지정 된 단계가 없는 모든 컨트롤은 0 단계의 일부로 암시적으로 간주 됩니다.
2.  PrettyDate 텍스트 블록을 표시 합니다.
3.  PrettyFileSize 및 prettyImageSize 텍스트 블록을 표시 합니다.
4.  이미지를 표시 합니다.

단계적으로 중단는 [**ListViewBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewBase) 에서 파생 된 컨트롤을 사용 하 고 데이터 바인딩을 위해 항목 템플릿을 증분 처리 하는 [{x:bind}](x-bind-markup-extension.md) 의 기능입니다. 목록 항목을 렌더링 하는 경우 **ListViewBase** 는 다음 단계로 이동 하기 전에 뷰의 모든 항목에 대해 단일 단계를 렌더링 합니다. 렌더링 작업은 목록이 스크롤 될 때 필요한 작업을 다시 평가할 수 있고 더 이상 표시 되지 않는 항목에 대해 수행 되지 않도록 시간 분할 된 일괄 처리에서 수행 됩니다.

**X:phase** 특성은 [{x:bind}](x-bind-markup-extension.md)를 사용 하는 데이터 템플릿의 모든 요소에 지정할 수 있습니다. 요소에 0이 아닌 단계가 있는 경우 해당 단계를 처리 하 고 바인딩을 업데이트할 때까지 요소가 **표시**되지 않고 **불투명도**를 통해 숨겨집니다. [**ListViewBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewBase)파생 컨트롤을 스크롤하면 화면에 더 이상 표시 되지 않는 항목에서 항목 템플릿을 재활용 하 여 새로 표시 되는 항목을 렌더링 합니다. 템플릿 내의 UI 요소는 데이터 바인딩될 때까지 이전 값을 유지 합니다. 단계적으로 중단를 설정 하면 데이터 바인딩 단계가 지연 되므로 단계적으로 중단가 유효 하지 않은 경우 UI 요소를 숨겨야 합니다.

각 UI 요소에는 지정 된 단계가 하나만 있을 수 있습니다. 이 경우 요소의 모든 바인딩에 적용 됩니다. 단계가 지정 되지 않은 경우 0 단계를 가정 합니다.

단계 번호는 연속적이 지 않아도 되며 [**ContainerContentChangingEventArgs**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.containercontentchangingeventargs.phase)의 값과 동일 합니다. [**ContainerContentChanging**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.containercontentchanging) 이벤트는 **x:phase** 바인딩이 처리 되기 전에 각 단계에 대해 발생 합니다.

단계적으로 중단은 { [Binding}](binding-markup-extension.md) 바인딩이 아니라 [{x:bind}](x-bind-markup-extension.md) 바인딩에만 영향을 줍니다.

단계적으로 중단는 단계적으로 중단을 인식 하는 컨트롤을 사용 하 여 항목 템플릿이 렌더링 되는 경우에만 적용 됩니다. Windows 10의 경우 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 및 [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)를 의미 합니다. 단계적으로 중단는 다른 항목 컨트롤에 사용 되는 데이터 템플릿이나 [**system.windows.controls.contentcontrol.contenttemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.contenttemplate) 또는 [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) 섹션과 같은 다른 시나리오에는 적용 되지 않습니다. 이러한 경우 모든 UI 요소는 한 번에 데이터 바인딩됩니다.

