---
author: jwmsft
title: xLoad 특성
description: xLoad 동적 생성 및 폐기 하는 요소 및 시작 시간 및 메모리 사용량을 줄이면 자식 함수를 사용 합니다.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8f34ea43b981de65c81e9cce2b8a896c3577e595
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2018
ms.locfileid: "610786"
---
# <a name="xload-attribute"></a>x:Load 특성

시작, 시각적 트리를 만들거나, 및 XAML 앱의 메모리 사용량을 최적화 하기 위해 **x: 부하** 를 사용할 수 있습니다. **X: 로드** 를 사용 하 여 비슷한 시각적 효과가 **표시 여부**는 요소 로드 되지 않은 경우 해당 메모리 해제 되 고 시각적 트리에서 해당 위치를 표시 하는 작은 자리 표시자를 사용 하는 내부적으로 한다는 점을 제외 합니다.

X: 부하와 특성을 사용 하는 UI 요소 로드 하 고를 통해 언로드된 코드 또는 [x: 바인딩](x-bind-markup-extension.md) 식을 사용 하 여 수 있습니다. 가끔 또는 조건부로 표시되는 요소의 비용을 줄이는 데 유용합니다. 예: 눈금 또는 StackPanel 컨테이너에서 x: 부하를 사용할 때 컨테이너와 모든 하위는 로드 하거나 언로드한 그룹

개체 틀에 맞게 x: 부하와 특성을 사용 하는 각 요소에 대 한 메모리 사용량을 약 600 바이트를 추가 하는 XAML 프레임 워크에 의해 지연 된 요소를 추적 합니다. 따라서를 성적 잔량이 실제로이 특성 과도 하 게 사용 하는 것이 같습니다. 만을 사용 하는 것을 숨길 수 하는 요소는 것이 좋습니다. 컨테이너에 x: 로드를 사용 하는 경우 오버 헤드가 x: 부하 특성을 가진 요소에 대해서만 지불 됩니다.

> [!IMPORTANT]
> X: 부하 특성은 Windows 10, 버전 1703 (작성자 업데이트)에서 1부터 제공 됩니다. Visual Studio 프로젝트가 대상으로 하는 최소 버전이 *Windows 10 크리에이터스 업데이트(10.0, Build 15063)* 는 되어야 x:Load를 사용할 수 있습니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object x:Load="True" .../>
<object x:Load="False" .../>
<object x:Load="{x:Bind Path.to.a.boolean, Mode=OneWay}" .../>
```

## <a name="loading-elements"></a>요소를 로드합니다.

여러 가지가 다른 요소를 로드할 수 있습니다:

- [X: 바인딩](x-bind-markup-extension.md) 식을 사용 하 여 부하 상태를 지정 합니다. 식이 **true** 를 로드이 고 **false 이면** 을 언로드하려면 다음 요소 반환 해야 합니다.
- 요소에 정의된 이름을 사용하여 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)을 호출합니다.
- 요소에 정의된 이름을 사용하여 [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416)를 호출합니다.
- [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007)x: 로드 요소를 대상으로 하는 [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) 또는 **스토리 보드** 애니메이션을 사용 합니다.
- 모든 **스토리 보드**에 언로드된 요소를 대상으로 합니다.

> 참고: 요소의 인스턴스화가 시작되면 UI 스레드에 요소가 만들어지므로 한 번에 너무 많이 만들 경우 UI 스터터가 발생할 수 있습니다.

위에 나열된 방법 중 하나에 의해 지연된 요소가 만들어진 경우 몇 가지 사항이 발생합니다.

- 요소의 [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) 이벤트가 발생합니다.
- X: 이름에 대 한 필드 설정 됩니다.
- 모든 x: 바인딩 바인딩 요소에 대해 평가 됩니다.
- 지연된 요소를 포함하는 특성에 대한 특성 변경 알림을 받도록 등록된 경우 알림이 발생합니다.

## <a name="unloading-elements"></a>언로드 요소

언로드하려면 다음 요소:

- X: 바인딩 식을 사용 하 여 부하 상태를 지정 합니다. 식이 **true** 를 로드이 고 **false 이면** 을 언로드하려면 다음 요소 반환 해야 합니다.
- 페이지 또는 UserControl에서 **UnloadObject** 를 호출 하 고 개체 참조에 전달
- **Windows.UI.Xaml.Markup.XamlMarkupHelper.UnloadObject** 를 호출 하 고 개체 참조에 전달

개체가 로드 된 경우 바뀝니다 트리에서 자리 표시자입니다. 개체 인스턴스에 대 한 모든 참조가 해제 될 때까지 메모리에 유지 됩니다. 페이지/UserControl에서 UnloadObject API는 개최 codegen x: 이름 및 x: 바인딩에 대 한 참조를 해제 하도록 설계 되었습니다. 응용 프로그램 코드에서 추가 참조 자료를 보유 하는 경우도 릴리스될 해야 합니다.

요소 로드 되 면 요소와 관련 된 모든 상태 삭제 됩니다, 그리고 하므로 표시 유형, 최적화 된 버전으로 x: 로드를 사용 하는 경우 다음 바인딩을 통해 적용 되는 상태 모두 확인 또는 로드 된 이벤트가 발생할 때 코드에 의해 다시 적용 합니다.

## <a name="restrictions"></a>제한 사항

**X 부하** 를 사용 하는 것에 대 한 제한 됩니다.:

- 나중에 요소를 찾아야 하는 경우가 발생하기 때문에 요소에 대한 [x:Name](x-name-attribute.md)을 정의해야 합니다.
- [**Ui 요소**](https://msdn.microsoft.com/library/windows/apps/br208911) 또는 [**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249)에서 파생 되는 형식에만 x: 부하를 사용할 수 있습니다.
- [**페이지**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page), [**UserControl**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol)또는 [**데이터 템플릿**](https://msdn.microsoft.com/library/windows/apps/br242348)루트 요소에 x: 부하를 사용할 수 없습니다.
- X: 부하 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)의 요소에 사용할 수 없습니다.
- X: 부하 [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048)와 함께 로드 느슨한 XAML에서 사용할 수 없습니다.
- 부모 요소를 이동 하는 로드 되지 않은 모든 요소를 지웁니다.

## <a name="remarks"></a>설명

하지만 가장 바깥쪽 요소에서 실현 해야 중첩 된 요소에 x: 부하를 사용할 수 있습니다.  부모 요소를 실현하기 전에 자식 요소를 실현하려고 하면 예외가 발생합니다.

일반적으로 첫 번째 프레임에서 볼 수 없는 요소를 지연시키는 것이 좋습니다. 지연할 후보 요소를 찾으려면 축소된 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992)로 만들려는 요소를 찾는 것이 좋습니다. 또한 사용자 조작에 의해 트리거되는 UI도 지연시킬 수 있는 요소를 찾기에 좋은 위치입니다.

[**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878)에서는 요소 지연에 조심해야 하는데, 시작 시간이 단축되지만 만드는 항목에 따라 이동 성능이 저하될 수 있기 때문입니다. 이동 성능을 향상시키려면 [{x:Bind} markup extension](x-bind-markup-extension.md) 및 [x:Phase attribute](x-phase-attribute.md) 설명서를 참조하세요.

[X: 단계 특성](x-phase-attribute.md) 사용 하는 경우 **x: 부하** 와 함께에서 다음 요소 또는 요소 트리 실현 하는 경우, 및 현재 단계까지 바인딩은 적용 됩니다. **X: 단계** 에 대해 지정 된 단계에는 영향을 않거나 요소는 로드 상태를 제어 합니다. 목록 항목을 팬의 일환으로 재활용 되 면 요소를 작동 하는 동일한 방식으로 다른 활성 요소 및 단계 설정 포함 되는 동일한 규칙을 사용 하 여 컴파일된 바인딩을 (**{x: 바인딩}** 바인딩)을 처리 하는 방식이 실현 합니다.

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

