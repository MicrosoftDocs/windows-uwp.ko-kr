---
xDeferLoadStrategy 특성
xDeferLoadStrategy는 요소 및 해당 자식의 생성을 지연하므로 시작 시간은 감소하고 메모리 사용량은 약간 늘어납니다. 영향을 받는 각 요소는 메모리 사용량에 약 600바이트를 추가합니다.
ms.assetid: E763898E-13FF-4412-B502-B54DBFE2D4E4
---

# x:DeferLoadStrategy 특성

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

**x:DeferLoadStrategy="Lazy"**는 요소 및 해당 자식의 생성을 지연하므로 시작 시간은 감소하고 메모리 사용량은 약간 늘어납니다. 영향을 받는 각 요소는 메모리 사용량에 약 600바이트를 추가합니다. 지연되는 요소 트리가 클수록 시작 시간이 더 많이 단축되지만 메모리 공간이 증가합니다. 그러므로 성능이 저하될 정도로 이 특성을 지나치게 많이 사용할 수 있습니다.

## XAML 특성 사용

``` syntax
<object x:DeferLoadStrategy="Lazy" .../>
```

## 설명

**x:DeferLoadStrategy** 사용에 대한 제한 사항은 다음과 같습니다.

-   나중에 요소를 찾을 방법이 필요하므로 [x:Name](x-name-attribute.md)을 정의해야 합니다.
-   [
            **FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249)에서 파생된 형식을 제외하고 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)만 지연되는 것으로 표시할 수 있습니다.
-   루트 요소는 [**Page**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.page), [**UserControls**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.usercontrol) 또는 [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348)에서 지연될 수 없습니다.
-   [
            **ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)의 요소는 지연될 수 없습니다.
-   [
            **XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048)를 통해 로드된 느슨한 XAML과는 작동하지 않습니다.
-   부모 요소를 이동하면 실현되지 않은 모든 요소가 지워집니다.

지연된 요소를 실현하는 여러 가지 방법이 있습니다.

-   요소에 정의된 이름을 사용하여 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)을 호출합니다.
-   요소에 정의된 이름을 사용하여 [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416)를 호출합니다.
-   [
            **VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007)에서 지연된 요소를 대상으로 하는 [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) 또는 **Storyboard** 애니메이션을 사용합니다.
-   모든 **Storyboard**에서 지연된 요소를 대상으로 합니다.
-   지연된 요소를 대상으로 하는 바인딩을 사용합니다.
-   참고: 요소의 인스턴스화가 시작되면 UI 스레드에 요소가 만들어지므로 한 번에 너무 많이 만들 경우 UI 스터터가 발생할 수 있습니다.

위에 나열된 방법 중 하나에 의해 지연된 요소가 만들어진 경우 몇 가지 사항이 발생합니다.

-   요소의 [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) 이벤트가 발생합니다.
-   요소의 모든 바인딩이 평가됩니다.
-   응용 프로그램이 지연된 요소를 포함하는 속성에 대한 속성 변경 알림을 받도록 등록된 경우 알림이 발생합니다.

지연된 요소를 중첩할 수 있지만 가장 바깥쪽 요소에서 실현해야 합니다.  부모를 실현하기 전에 자식 요소를 실현하려고 하면 예외가 발생합니다.

일반적으로 첫 번째 프레임에서 볼 수 없는 작업을 지연하는 것이 좋습니다.  지연할 후보를 찾으려면 축소된 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992)로 만들려는 요소를 찾는 것이 좋습니다.  또한 부수적 UI(즉, 사용자 조작에 의해 트리거되는 UI)도 지연 요소를 찾기에 적절한 위치입니다.  

[
            **ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 시나리오에서는 지연 요소에 주의해야 합니다. 시작 시간이 단축되지만 만드는 항목에 따라 이동 성능도 저하될 수 있습니다.  이동 성능을 향상시키려면 [{x:Bind} 태그 확장](x-bind-markup-extension.md) 및 [x:Phase 특성](x-phase-attribute.md) 설명서를 참조하세요.

[x:Phase 특성](x-phase-attribute.md)이 **x:DeferLoadStrategy**와 함께 사용되는 경우 요소 또는 요소 트리가 실현되면 바인딩이 현재 단계까지 적용됩니다. **x:Phase**에 대해 지정된 단계는 요소의 지연에 영향을 미치거나 제어하지 않습니다. 목록 항목이 이동의 한 부분으로 재활용되면 실현된 요소는 다른 활성 요소와 같은 방식으로 동작하고, 컴파일된 바인딩(**{x:Bind}** 바인딩)은 단계를 포함하여 동일한 규칙을 사용하여 처리됩니다.

일반적인 지침은 이전과 이후에 응용 프로그램을 측정하여 원하는 성능을 얻었는지 확인하는 것입니다.

## 예제

```xaml
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
    this.FindName("DeferredGrid"); // This will realize the deferred grid
}
```



<!--HONumber=Mar16_HO1-->


