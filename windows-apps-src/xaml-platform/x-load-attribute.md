---
author: jwmsft
title: xLoad 특성
description: xLoad 요소와 하위 시작 시간 및 메모리 사용량을 감소의 동적 생성 및 제거가 수 있습니다. 
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d9659d183c020c579aa0a21fe179a69c1d9997c5
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6051918"
---
# <a name="xload-attribute"></a>x:Load 특성

시작, 시각적 트리 생성 및 XAML 앱의 메모리 사용량을 최적화 하기 위해 **X:load** 를 사용할 수 있습니다. **X:load** 를 사용 하 여 요소가 로드 되지 않을 때 메모리를 해제 되 고 시각적 트리에서 받으므로 장소를 표시 하는 작은 자리 표시자를 사용 하는 내부적으로 점을 제외 하 고 유사한 시각 효과를 **표시 여부**있습니다.

X:load를 사용 하 여 특성을 사용 하는 UI 요소는 로드 및 언로드를 통해 코드 또는 [X:bind](x-bind-markup-extension.md) 식을 사용 하 여 수 있습니다. 가끔 또는 조건부로 표시되는 요소의 비용을 줄이는 데 유용합니다. 그리드 등 StackPanel 컨테이너에서 X:load를 사용 하면 컨테이너 및 모든 하위는 로드 또는 그룹으로 언로드 합니다.

자리 표시자에 맞게 새롭게 X:load를 사용 하 여 특성이 지정 된 각 요소에 대 한 메모리 사용량에 약 600 바이트를 추가 하는 XAML 프레임 워크에서 지연 된 요소를 추적 합니다. 따라서 될 정도로이 특성을 지나치게 실제로 그러므로 성능이 수 있습니다. 만을 사용 하는 것 요소를 숨겨야 하는 것이 좋습니다. 컨테이너에서 X:load를 사용 하는 경우 X:load 특성을 사용 하 여 요소에 대해서만 오버 헤드가 지급 됩니다.

> [!IMPORTANT]
> X:load 특성은 Windows 10 버전 1703 (크리에이터 스 업데이트)부터 사용할 수 있습니다. Visual Studio 프로젝트가 대상으로 하는 최소 버전이 *Windows 10 크리에이터스 업데이트(10.0, Build 15063)* 는 되어야 x:Load를 사용할 수 있습니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object x:Load="True" .../>
<object x:Load="False" .../>
<object x:Load="{x:Bind Path.to.a.boolean, Mode=OneWay}" .../>
```

## <a name="loading-elements"></a>요소를 로드합니다.

여러 가지 다른 방법으로 요소를 로드 하려면:

- [X: Bind](x-bind-markup-extension.md) 식을 사용 하 여 로드 상태를 지정 합니다. 식 **true** 로드 및 **false** 언로드 요소를 반환 해야 합니다.
- 요소에 정의된 이름을 사용하여 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)을 호출합니다.
- 요소에 정의된 이름을 사용하여 [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416)를 호출합니다.
- [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007)에서 X:load 요소를 대상으로 하는 [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) 또는 **스토리 보드** 애니메이션을 사용 합니다.
- 모든 **스토리 보드**에서 언로드할된 요소를 대상으로 합니다.

> 참고: 요소의 인스턴스화가 시작되면 UI 스레드에 요소가 만들어지므로 한 번에 너무 많이 만들 경우 UI 스터터가 발생할 수 있습니다.

위에 나열된 방법 중 하나에 의해 지연된 요소가 만들어진 경우 몇 가지 사항이 발생합니다.

- 요소의 [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) 이벤트가 발생합니다.
- X:name 필드 설정 됩니다.
- 요소의 모든 X:bind 바인딩이 평가 됩니다.
- 지연된 요소를 포함하는 특성에 대한 특성 변경 알림을 받도록 등록된 경우 알림이 발생합니다.

## <a name="unloading-elements"></a>언로드 요소

요소를 언로드할:

- X: Bind 식을 사용 하 여 로드 상태를 지정 합니다. 식 **true** 로드 및 **false** 언로드 요소를 반환 해야 합니다.
- 페이지 또는 UserControl, **UnloadObject** 를 호출 하 고 개체 참조에 전달 합니다.
- **Windows.UI.Xaml.Markup.XamlMarkupHelper.UnloadObject** 를 호출 하 고 개체 참조에 전달 합니다.

개체 언로드될 때 트리의 자리 표시자를 사용 하 여 대체 될 예정입니다. 개체 인스턴스는 릴리스된 모든 참조 될 때까지 메모리에 남아 있습니다. 페이지/UserControl에서 UnloadObject API X:name 및 X:bind 떨어뜨리지 소유한 참조를 해제 하도록 설계 되었습니다. 앱 코드에서 추가 대 한 참조를 보유 하는 경우 릴리스될 할 수도 있습니다.

요소를 로드할 때 요소와 관련 된 모든 상태 삭제 됩니다, 그리고 따라서 표시 여부를 최적화 된 버전으로 X:load를 사용 하는 경우 다음 바인딩을 통해 적용 된 모든 상태 확인 또는 Loaded 이벤트 발생 시 코드에 의해 다시 적용 됩니다.

## <a name="restrictions"></a>제한

**X:load** 를 사용 하기 위한 제한 됩니다.

- [X:name](x-name-attribute.md)을 정의 해야요소에 대 한 경우가 해야 나중에 요소를 찾을 수 있습니다.
- X:load [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 또는 [**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249)에서 파생 된 유형을 에서만 사용할 수 있습니다.
- X:load [**페이지**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page), [**UserControl**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol)또는 [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348)에서 루트 요소에 사용할 수 없습니다.
- [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)요소에 X:load를 사용할 수 없습니다.
- [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048)로 로드 된 느슨한 XAML에서 X:load를 사용할 수 없습니다.
- 부모 요소를 이동 로드 되지 않은 모든 요소가 지워집니다 됩니다.

## <a name="remarks"></a>설명

X:load 있지만 가장 바깥쪽 요소에서 실현 해야 중첩 된 요소에 사용할 수 있습니다. 부모 요소를 실현하기 전에 자식 요소를 실현하려고 하면 예외가 발생합니다.

일반적으로 첫 번째 프레임에서 볼 수 없는 요소를 지연시키는 것이 좋습니다.지연할 후보 요소를 찾으려면 축소된 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992)로 만들려는 요소를 찾는 것이 좋습니다. 또한 사용자 조작에 의해 트리거되는 UI도 지연시킬 수 있는 요소를 찾기에 좋은 위치입니다.

[**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878)에서는 요소 지연에 조심해야 하는데, 시작 시간이 단축되지만 만드는 항목에 따라 이동 성능이 저하될 수 있기 때문입니다. 이동 성능을 향상시키려면 [{x:Bind} markup extension](x-bind-markup-extension.md) 및 [x:Phase attribute](x-phase-attribute.md) 설명서를 참조하세요.

그런 다음 [X:phase 특성](x-phase-attribute.md) 은 **X:load** 와 함께에서 사용 요소 또는 요소 트리가 실현 되 면, 바인딩이 및 현재 단계까지 적용 됩니다. **X:phase** 에 대해 지정 된 단계에 영향을 않거나 요소의 로드 상태를 제어 합니다. 목록 항목이 이동의 부분으로 재활용 되 면 실현 요소는 다른 활성 요소, 동일한 방식으로 동작 하 고 컴파일된 바인딩 (**{x: Bind}** 바인딩) 단계가 포함 하 여 동일한 규칙을 사용 하 여 처리 됩니다.

일반적인 지침은 이전과 이후에 앱의 성능을 측정하여 원하는 성능을 얻었는지 확인하기 위한 것입니다.

## <a name="example"></a>예제

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

