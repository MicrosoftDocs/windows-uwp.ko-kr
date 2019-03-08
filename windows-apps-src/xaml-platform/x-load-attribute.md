---
title: xLoad 특성
description: xLoad는 요소 및 자식 요소의 동적인 생성 및 소멸을 지원하여 시작 시간과 메모리 사용을 줄일 수 있도록 합니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1fa0f12779ad56d57c92f667443644851dc3d5e5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629368"
---
# <a name="xload-attribute"></a>x:Load 특성

**x:Load**를 사용하여 XAML 앱의 시작, 시각적 트리 생성 및 메모리 사용을 최적화할 수 있습니다. **x:Load**를 사용하면 **Visibility**와 유사한 시각적 효과를 갖습니다. 단, 요소가 로드되지 않으면 메모리가 해제되고 내부적으로 작은 자리 표시자를 사용해 시각적 트리에 위치를 표시된다는 점이 다릅니다.

x:Load 특성이 지정된 UI 요소는 코드를 통해, 또는 [x:Bind](x-bind-markup-extension.md) 식을 사용해 로드 및 언로드가 가능합니다. 가끔 또는 조건부로 표시되는 요소의 비용을 줄이는 데 유용합니다. 그리드 또는 StackPanel 같은 컨테이너에서 x:Load를 사용하면 컨테이너와 그 자식 요소들이 하나의 그룹으로 로드 또는 언로드됩니다.

XAML 프레임워크에서 지연된 요소를 추적하면 자리 표시자를 고려하여 x:Load 특성이 지정된 각 요소에서 메모리 사용이 약 600바이트 추가됩니다. 그러므로 실제 성능이 저하될 정도로 이 특성을 지나치게 많이 사용할 수 있습니다. 숨겨야 하는 요소에서만 이 특성을 사용하는 것이 좋습니다. 컨테이너에서 x:Load를 사용하면 x:Load 특성을 가진 요소에만 오버헤드 비용이 지불됩니다.

> [!IMPORTANT]
> X: 로드 특성은 Windows 10 버전 1703 (크리에이터 업데이트)부터 사용할 수 있습니다. Visual Studio 프로젝트가 대상으로 하는 최소 버전이 *Windows 10 크리에이터스 업데이트(10.0, Build 15063)* 는 되어야 x:Load를 사용할 수 있습니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object x:Load="True" .../>
<object x:Load="False" .../>
<object x:Load="{x:Bind Path.to.a.boolean, Mode=OneWay}" .../>
```

## <a name="loading-elements"></a>요소 로드

요소를 로드하는 여러 가지 방법이 있습니다.

- [x:Bind](x-bind-markup-extension.md) 식을 사용해 로드 상태를 지정합니다. 식은 요소를 로드하려면 **true**를, 언로드하려면 **false**를 반환해야 합니다.
- 요소에 정의된 이름을 사용하여 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)을 호출합니다.
- 요소에 정의된 이름을 사용하여 [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416)를 호출합니다.
- [  **VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007)에서 x:Load 요소를 대상으로 하는 [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) 또는 **Storyboard** 애니메이션을 사용합니다.
- 모든 **Storyboard**에서 언로드된 요소를 대상으로 합니다.

> 참고: 요소의 인스턴스화 시작 된 후 UI 끊길 경우 너무 많은 동시에 생성 됩니다 될 수 있도록 UI 스레드에서 만들어집니다.

위에 나열된 방법 중 하나에 의해 지연된 요소가 만들어진 경우 몇 가지 사항이 발생합니다.

- 요소의 [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) 이벤트가 발생합니다.
- x:Name에 대한 필드가 설정됩니다.
- 요소에 대한 모든 x:Bind 바인딩이 평가됩니다.
- 지연된 요소를 포함하는 특성에 대한 특성 변경 알림을 받도록 등록된 경우 알림이 발생합니다.

## <a name="unloading-elements"></a>요소 언로드

요소를 언로드하는 방법:

- x:Bind 식을 사용해 로드 상태를 지정합니다. 식은 요소를 로드하려면 **true**를, 언로드하려면 **false**를 반환해야 합니다.
- Page 또는 UserControl에서 **UnloadObject**를 호출하고 개체 참조에 전달합니다.
- **Windows.UI.Xaml.Markup.XamlMarkupHelper.UnloadObject**를 호출하고 개체 참조에 전달합니다.

언로드된 개체는 트리에서 자리 표시자로 대체가 됩니다. 모든 참조가 해제될 때까지 개체 인스턴스는 메모리에 그대로 유지됩니다. Page/UserControl의 UnloadObject API는 x:Name 및 x:Bind에서 codegen이 보유한 참조를 해제하도록 설계되었습니다. 앱 코드에 추가 참조를 보유하고 있는 경우에도 이들을 해제해야 합니다.

요소가 언로드되면 요소와 관련된 모든 상태가 삭제되기 때문에 x:Load를 최적화된 버전의 Visibility로 사용하는 경우에는 모든 상태가 바인딩을 통해 적용되었는지, 아니면 Loaded 이벤트가 발생할 때 코드에 의해 다시 적용되었는지 확인합니다.

## <a name="restrictions"></a>제한 사항

**x:Load** 사용에 대한 제한 사항은 다음과 같습니다.

- 정의 해야 합니다는 [X:name](x-name-attribute.md) 요소 만큼 해야 나중에 요소를 찾을 수 있습니다.
- [  **UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 또는 [**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249)에서 파생된 형식에서만 x:Load를 사용할 수 있습니다.
- [  **Page**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page), [**UserControl**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol) 또는 [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348)의 루트 요소에서는 x:Load를 사용할 수 없습니다.
- [  **ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)의 요소에서는 x:Load를 사용할 수 없습니다.
- [  **XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048)를 통해 로드된 느슨한 XAML에서는 x:Load를 사용할 수 없습니다.
- 부모 요소를 이동하면 로드되지 않은 모든 요소가 지워집니다.

## <a name="remarks"></a>설명

중첩된 요소에서 x:Load를 사용할 수 있지만, 가장 바깥쪽 요소에서 실현해야 합니다.  부모 요소를 실현하기 전에 자식 요소를 실현하려고 하면 예외가 발생합니다.

일반적으로 첫 번째 프레임에서 볼 수 없는 요소를 지연시키는 것이 좋습니다. 지연할 후보를 찾으려면 축소된 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992)로 만들려는 요소를 찾는 것이 좋습니다. 또한 사용자 조작에 의해 트리거되는 UI도 지연시킬 수 있는 요소를 찾기에 좋은 위치입니다.

[  **ListView**](https://msdn.microsoft.com/library/windows/apps/br242878)에서는 요소 지연에 조심해야 하는데, 시작 시간이 단축되지만 만드는 항목에 따라 이동 성능이 저하될 수 있기 때문입니다. 이동 성능을 향상시키려면 [{x:Bind} markup extension](x-bind-markup-extension.md) 및 [x:Phase attribute](x-phase-attribute.md) 설명서를 참조하세요.

[x:Phase attribute](x-phase-attribute.md)가 **x:Load**와 함께 사용되는 경우, 요소 또는 요소 트리가 실현되면 바인딩이 현재 단계까지 적용됩니다. **x:Phase**에 지정된 단계는 요소의 로드 상태에 영향을 미치거나 제어하지 않습니다. 목록 항목이 이동의 한 부분으로 재활용되면 실현된 요소는 다른 활성 요소와 같은 방식으로 동작하고, 컴파일된 바인딩(**{x:Bind}** 바인딩)은 단계를 포함하여 동일한 규칙을 사용해 처리됩니다.

일반적인 지침은 이전과 이후에 앱의 성능을 측정하여 원하는 성능을 얻었는지 확인하기 위한 것입니다.

## <a name="example"></a>예

```xml
<StackPanel>
    <Grid x:Name="DeferredGrid" x:Load="False">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>

        <Rectangle Height="100" Width="100" Fill="Orange" Margin="0,0,4,4"/>
        <Rectangle Height="100" Width="100" Fill="Green" Grid.Column="1" Margin="4,0,0,4"/>
        <Rectangle Height="100" Width="100" Fill="Blue" Grid.Row="1" Margin="0,4,4,0"/>
        <Rectangle Height="100" Width="100" Fill="Gold" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0"
                   x:Name="one" x:Load="{x:Bind (x:Boolean)CheckBox1.IsChecked, Mode=OneWay}"/>
        <Rectangle Height="100" Width="100" Fill="Silver" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0"
                   x:Name="two" x:Load="{x:Bind Not(CheckBox1.IsChecked), Mode=OneWay}"/>
    </Grid>

    <Button Content="Load elements" Click="LoadElements_Click"/>
    <Button Content="Unload elements" Click="UnloadElements_Click"/>
    <CheckBox x:Name="CheckBox1" Content="Swap Elements" />
</StackPanel>
```

```csharp
// This is used by the bindings between the rectangles and check box.
private bool Not(bool? value) { return !(value==true); }

private void LoadElements_Click(object sender, RoutedEventArgs e)
{
    // This will load the deferred grid, but not the nested
    // rectangles that have x:Load attributes.
    this.FindName("DeferredGrid"); 
}

private void UnloadElements_Click(object sender, RoutedEventArgs e)
{
     // This will unload the grid and all its child elements.
     this.UnloadObject(DeferredGrid);
}
```

