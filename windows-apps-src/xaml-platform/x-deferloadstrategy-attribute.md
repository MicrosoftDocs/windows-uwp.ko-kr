---
title: xDeferLoadStrategy 특성
description: xDeferLoadStrategy는 요소 및 해당 자식의 생성을 지연하므로 시작 시간은 감소하고 메모리 사용량은 약간 늘어납니다.영향을 받는 각 요소는 메모리 사용량에 약 600바이트를 추가합니다.
ms.assetid: E763898E-13FF-4412-B502-B54DBFE2D4E4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4432362db74f830774a2c4f74401c472c128a120
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8937701"
---
# <a name="xdeferloadstrategy-attribute"></a>x:DeferLoadStrategy 특성

> [!IMPORTANT]
> Windows 10, 버전 1703(크리에이터스 업데이트)부터는 **x:DeferLoadStrategy**가 [**x:Load 특성**](x-load-attribute.md)으로 대체됩니다. `x:Load="False"` 사용은 `x:DeferLoadStrategy="Lazy"`와 동일한 기능을 하지만, 필요할 경우 UI를 언로드할 수 있습니다. 자세한 내용은 [x:Load attribute](x-load-attribute.md)을 참조하세요.

**x: DeferLoadStrategy = "Lazy"** 를 사용해 XAML 앱의 시작 또는 트리 생성 성능을 최적화할 수 있습니다. **x: DeferLoadStrategy = "Lazy"** 를 사용하면 요소 및 그 자식 요소의 생성이 지연되면서 시작 시간 및 메모리 비용이 증가합니다. 가끔 또는 조건부로 표시되는 요소의 비용을 줄이는 데 유용합니다. 이 요소는 코드나 VisualStateManager에서 참조될 때 실현됩니다.

그러나 XAML 프레임워크에서 지연된 요소를 추적하면 영향을 받는 각 요소에서의 메모리 사용이 약 600바이트 추가됩니다. 지연되는 요소 트리가 클수록 시작 시간이 더 많이 단축되지만 메모리 공간이 증가합니다. 그러므로 성능이 저하될 정도로 이 특성을 지나치게 많이 사용할 수 있습니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object x:DeferLoadStrategy="Lazy" .../>
```

## <a name="remarks"></a>설명

**x:DeferLoadStrategy** 사용에 대한 제한 사항은 다음과 같습니다.

- [X:name](x-name-attribute.md)을 정의 해야요소에 대 한 경우가 되어야 할 나중에 요소를 찾을 수 있습니다.
- [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 또는 [**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249)에서 파생된 형식만 지연시킬 수 있습니다.
- [**Page**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page), [**UserControl**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol) 또는 [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348)에서 루트 요소를 지연시킬 수 없습니다.
- [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)에서 요소를 지연시킬 수 없습니다.
- [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048)에서 로드된 느슨한 XAML을 지연시킬 수 없습니다.
- 부모 요소를 이동하면 실현되지 않은 모든 요소가 지워집니다.

지연된 요소를 실현하는 여러 가지 방법이 있습니다.

- 요소에 정의된 이름을 사용하여 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)을 호출합니다.
- 요소에 정의된 이름을 사용하여 [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416)를 호출합니다.
- [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007)에서 지연된 요소를 대상으로 하는 [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) 또는 **Storyboard** 애니메이션을 사용합니다.
- 모든 **Storyboard**에서 지연된 요소를 대상으로 합니다.
- 지연된 요소를 대상으로 하는 바인딩을 사용합니다.

> 참고: 요소의 인스턴스화가 시작되면 UI 스레드에 요소가 만들어지므로 한 번에 너무 많이 만들 경우 UI 스터터가 발생할 수 있습니다.

위에 나열된 방법 중 하나에 의해 지연된 요소가 만들어진 경우 몇 가지 사항이 발생합니다.

- 요소의 [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) 이벤트가 발생합니다.
- 요소의 모든 바인딩이 평가됩니다.
- 지연된 요소를 포함하는 특성에 대한 특성 변경 알림을 받도록 등록된 경우 알림이 발생합니다.

지연된 요소를 중첩할 수 있지만 가장 바깥쪽 요소에서 실현해야 합니다. 부모 요소를 실현하기 전에 자식 요소를 실현하려고 하면 예외가 발생합니다.

일반적으로 첫 번째 프레임에서 볼 수 없는 요소를 지연시키는 것이 좋습니다.지연할 후보 요소를 찾으려면 축소된 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992)로 만들려는 요소를 찾는 것이 좋습니다. 또한 사용자 조작에 의해 트리거되는 UI도 지연시킬 수 있는 요소를 찾기에 좋은 위치입니다.

[**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878)에서는 요소 지연에 조심해야 하는데, 시작 시간이 단축되지만 만드는 항목에 따라 이동 성능이 저하될 수 있기 때문입니다. 이동 성능을 향상시키려면 [{x:Bind} markup extension](x-bind-markup-extension.md) 및 [x:Phase attribute](x-phase-attribute.md) 설명서를 참조하세요.

[x:Phase attribute](x-phase-attribute.md)이 **x:DeferLoadStrategy**와 함께 사용되는 경우, 요소 또는 요소 트리가 실현되면 바인딩이 현재 단계까지 적용됩니다. **x:Phase**에 지정된 단계는 요소의 지연에 영향을 미치거나 제어하지 않습니다. 목록 항목이 이동의 한 부분으로 재활용되면 실현된 요소는 다른 활성 요소와 같은 방식으로 동작하고, 컴파일된 바인딩(**{x:Bind}** bindings)은 단계를 포함하여 동일한 규칙을 사용해 처리됩니다.

일반적인 지침은 이전과 이후에 앱의 성능을 측정하여 원하는 성능을 얻었는지 확인하기 위한 것입니다.

## <a name="example"></a>예제

```xml
<Grid x:Name="DeferredGrid" x:DeferLoadStrategy="Lazy">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition Width="Auto" />
    </Grid.ColumnDefinitions>

    <Rectangle Height="100" Width="100" Fill="#F65314" Margin="0,0,4,4" />
    <Rectangle Height="100" Width="100" Fill="#7CBB00" Grid.Column="1" Margin="4,0,0,4" />
    <Rectangle Height="100" Width="100" Fill="#00A1F1" Grid.Row="1" Margin="0,4,4,0" />
    <Rectangle Height="100" Width="100" Fill="#FFBB00" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0" />
</Grid>
<Button x:Name="RealizeElements" Content="Realize Elements" Click="RealizeElements_Click"/>
```

```csharp
private void RealizeElements_Click(object sender, RoutedEventArgs e)
{
    // This will realize the deferred grid.
    this.FindName("DeferredGrid");
}
```
