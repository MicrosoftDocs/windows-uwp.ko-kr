---
title: xLoad 특성
description: xLoad를 사용 하면 요소와 자식 요소를 동적으로 만들고 소멸 하 여 시작 시간 및 메모리 사용량을 줄일 수 있습니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1b110091c1a1f208ad06ea52f5c84dc7daad56c0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161687"
---
# <a name="xload-attribute"></a>x:Load 특성

XAML 앱의 시작, 시각적 트리 생성 및 메모리 사용을 최적화 하기 위해 **X:load** 를 사용할 수 있습니다. **X:load** 를 사용 하면 **표시**와 유사한 시각적 효과가 있습니다. 단, 요소가 로드 되지 않은 경우에는 해당 메모리를 해제 하 고 내부적으로 작은 자리 표시자를 사용 하 여 시각적 트리에서 해당 자리를 표시 합니다.

X:Load로 특성이 지정 된 UI 요소는 코드를 통해 로드 및 언로드될 수 있습니다. 또는 [X:load](x-bind-markup-extension.md) 식을 사용할 수 있습니다. 자주 표시 되거나 조건부로 표시 되는 요소의 비용을 줄이는 데 유용 합니다. Grid 또는 StackPanel과 같은 컨테이너에서 x:Load를 사용 하는 경우 컨테이너와 모든 자식 항목이 그룹으로 로드 되거나 언로드됩니다.

XAML 프레임 워크에서 지연 된 요소를 추적 하면 x:Load를 사용 하 여 특성을 지정 하는 각 요소에 대 한 메모리 사용에 약 600 바이트를 추가 하 여 자리 표시자를 고려 합니다. 따라서 성능이 실제로 감소 하는 범위에 대해이 특성을 과도 하 게 사용할 수 있습니다. 숨겨야 하는 요소에만 사용 하는 것이 좋습니다. 컨테이너에서 x:Load를 사용 하는 경우에는 x:Load 특성이 있는 요소에 대해서만 오버 헤드가 지불 됩니다.

> [!IMPORTANT]
> X:Load 특성은 Windows 10 버전 1703 (크리에이터 업데이트)부터 사용할 수 있습니다. X:Load.를 사용 하려면 Visual Studio 프로젝트에서 대상으로 지정 된 최소 버전이 *Windows 10 크리에이터 업데이트 (10.0, 빌드 15063)* 여야 합니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object x:Load="True" .../>
<object x:Load="False" .../>
<object x:Load="{x:Bind Path.to.a.boolean, Mode=OneWay}" .../>
```

## <a name="loading-elements"></a>요소 로드

여러 가지 방법으로 요소를 로드할 수 있습니다.

- [X:bind](x-bind-markup-extension.md) 식을 사용 하 여 로드 상태를 지정 합니다. 식은 로드 하려면 **true** 를 반환 하 고, 요소를 언로드하려면 **false** 를 반환 해야 합니다.
- 요소에 대해 정의한 이름으로 [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) 을 호출 합니다.
- 요소에 대해 정의한 이름으로 [**system.windows.frameworkelement.gettemplatechild**](/uwp/api/windows.ui.xaml.controls.control.gettemplatechild) 를 호출 합니다.
- [**Visualstate**](/uwp/api/Windows.UI.Xaml.VisualState)에서 x:load 요소를 대상으로 하는 [**Setter**](/uwp/api/Windows.UI.Xaml.Setter) 또는 **Storyboard** 애니메이션을 사용 합니다.
- 모든 **Storyboard**에서 언로드된 요소를 대상으로 합니다.

> 참고: 요소 인스턴스화가 시작 되 면 UI 스레드에 생성 되므로 한 번에 너무 많이 생성 된 경우 UI가 끊길 될 수 있습니다.

이전에 나열 된 방법 중 하나에서 지연 된 요소를 만든 후에는 다음과 같은 몇 가지 작업을 수행 합니다.

- 요소에 대해 [**로드**](/uwp/api/windows.ui.xaml.frameworkelement.loaded) 된 이벤트가 발생 합니다.
- X:Name 필드는 설정 됩니다.
- 요소에 대 한 모든 x:Bind 바인딩이 평가 됩니다.
- 지연 된 요소를 포함 하는 속성에 대 한 속성 변경 알림을 수신 하도록 등록 한 경우 알림이 발생 합니다.

## <a name="unloading-elements"></a>요소 언로드

요소를 언로드하려면:

- X:Bind 식을 사용 하 여 로드 상태를 지정 합니다. 식은 로드 하려면 **true** 를 반환 하 고, 요소를 언로드하려면 **false** 를 반환 해야 합니다.
- 페이지 또는 UserControl에서 **UnloadObject** 를 호출 하 고 개체 참조를 전달 합니다.
- **XamlMarkupHelper UnloadObject** 를 호출 하 고 개체 참조를 전달 합니다.

개체가 언로드되면 해당 개체가 트리에서 자리 표시자로 바뀝니다. 개체 인스턴스는 모든 참조가 해제 될 때까지 메모리에 유지 됩니다. 페이지/UserControl의 UnloadObject API는 x:Name 및 x:Bind. 대해 codegen에서 보유 한 참조를 해제 하도록 디자인 되었습니다. 앱 코드에서 추가 참조를 보유 하는 경우에도 해제 해야 합니다.

요소가 언로드될 때 요소와 연결 된 모든 상태는 무시 되므로 x:Load를 최적화 된 버전의 표시 유형으로 사용 하는 경우 모든 상태가 바인딩을 통해 적용 되는지 확인 하거나 로드 된 이벤트가 발생할 때 코드에 의해 다시 적용 되는지 확인 합니다.

## <a name="restrictions"></a>제한 사항

**X:load** 사용에 대 한 제한 사항은 다음과 같습니다.

- 요소를 나중에 [x:Name](x-name-attribute.md)   찾을 수 있는 방법이 필요 하므로 요소에 대해 x:Name을 정의 해야 합니다.
- [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) 또는 [**FlyoutBase**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase)에서 파생 된 형식에 대해서만 x:load를 사용할 수 있습니다.
- [**페이지**](/uwp/api/windows.ui.xaml.controls.page), [**UserControl**](/uwp/api/windows.ui.xaml.controls.usercontrol)또는 [**DataTemplate**](/uwp/api/Windows.UI.Xaml.DataTemplate)의 루트 요소에는 x:load를 사용할 수 없습니다.
- [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)의 요소에는 x:load를 사용할 수 없습니다.
- [**XamlReader**](/uwp/api/windows.ui.xaml.markup.xamlreader.load)로 로드 된 느슨한 XAML에는 x:load를 사용할 수 없습니다.
- 부모 요소를 이동 하면 로드 되지 않은 모든 요소가 지워집니다.

## <a name="remarks"></a>설명

중첩 된 요소에 대해 x:Load를 사용할 수 있지만의 가장 바깥쪽 요소에서이를 인식 해야 합니다. 부모를 인식 하기 전에 자식 요소를 실현 하려고 하면 예외가 발생 합니다.

일반적으로 첫 번째 프레임에서는 볼 수 없는 요소를 지연 하는 것이 좋습니다.연기할 후보를 찾기 위한 좋은 지침은 축소 [**표시**](/uwp/api/windows.ui.xaml.uielement.visibility)를 사용 하 여 생성 되는 요소를 찾는 것입니다. 또한 사용자 상호 작용에 의해 트리거되는 UI는 연기할 수 있는 요소를 찾을 수 있는 좋은 위치입니다.

[**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView)에서 요소를 지연 하는 것은 시작 시간을 단축 하 고, 만드는 항목에 따라 패닝 성능을 낮출 수도 있으므로 주의 해야 합니다. 패닝 성능을 향상 시키려면 [{X:bind} 태그 확장](x-bind-markup-extension.md) 및 [x:phase 특성](x-phase-attribute.md) 설명서를 참조 하세요.

X:phase [특성이](x-phase-attribute.md) **x:phase** 와 함께 사용 되 면 요소 또는 요소 트리가 실현 될 때 바인딩이 현재 단계까지 적용 됩니다. **X:phase** 에 지정 된 단계는 요소의 로드 상태에 영향을 주거나이를 제어 합니다. 목록 항목이 이동의 일부로 재활용 되 면 인식 된 요소는 다른 활성 요소와 같은 방식으로 동작 하 고 컴파일된 바인딩 (**{X:bind}** 바인딩)은 단계적으로 중단를 비롯 한 동일한 규칙을 사용 하 여 처리 됩니다.

일반적인 지침은 원하는 성능을 얻을 수 있도록 하기 전후에 앱의 성능을 측정 하는 것입니다.

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