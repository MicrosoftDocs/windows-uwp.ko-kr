---
title: xDeferLoadStrategy 특성
description: xDeferLoadStrategy는 요소와 해당 자식의 생성을 지연 하 여 시작 시간을 단축 하지만 메모리 사용을 약간 늘립니다.영향을 받는 각 요소는 약 600 바이트를 메모리 사용에 추가 합니다.
ms.assetid: E763898E-13FF-4412-B502-B54DBFE2D4E4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f37c04af132e742a54df8e88e5c875a32fa94beb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173777"
---
# <a name="xdeferloadstrategy-attribute"></a>x:DeferLoadStrategy 특성

> [!IMPORTANT]
> Windows 10부터 1703 (크리에이터 업데이트) 버전부터 **x:DeferLoadStrategy** 는 [**x:load 특성**](x-load-attribute.md)으로 대체 됩니다. 를 사용 하 `x:Load="False"` 는 것은와 동일 `x:DeferLoadStrategy="Lazy"` 하지만 필요한 경우 UI를 언로드하는 기능을 제공 합니다. 자세한 정보는 [X:load 특성](x-load-attribute.md) 을 참조 하세요.

**X:DeferLoadStrategy = "Lazy"** 를 사용 하 여 XAML 앱의 시작 또는 트리 생성 성능을 최적화할 수 있습니다. **X:DeferLoadStrategy = "Lazy"** 를 사용 하는 경우 요소와 해당 자식의 생성이 지연 되어 시작 시간 및 메모리 비용이 줄어듭니다. 자주 표시 되거나 조건부로 표시 되는 요소의 비용을 줄이는 데 유용 합니다. 요소는 코드 또는 r에서 참조 될 때 인식 됩니다.

그러나 XAML 프레임 워크에서 지연 된 요소를 추적 하면 영향을 받는 각 요소의 메모리 사용에 약 600 바이트가 추가 됩니다. 지연 되는 요소 트리가 클수록 더 많은 시작 시간이 소요 되지만 메모리 사용 공간이 더 커집니다. 따라서 성능이 저하 되는 범위에 대해이 특성을 과도 하 게 사용할 수 있습니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object x:DeferLoadStrategy="Lazy" .../>
```

## <a name="remarks"></a>설명

**X:DeferLoadStrategy** 사용에 대 한 제한 사항은 다음과 같습니다.

- 요소를 나중에 [x:Name](x-name-attribute.md)   찾을 수 있는 방법이 필요 하므로 요소에 대해 x:Name을 정의 해야 합니다.
- [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) 또는 [**FlyoutBase**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase)에서 파생 된 형식만 연기할 수 있습니다.
- [**페이지**](/uwp/api/windows.ui.xaml.controls.page), [**UserControl**](/uwp/api/windows.ui.xaml.controls.usercontrol)또는 [**DataTemplate**](/uwp/api/Windows.UI.Xaml.DataTemplate)의 루트 요소는 연기할 수 없습니다.
- [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)의 요소는 연기할 수 없습니다.
- [**XamlReader**](/uwp/api/windows.ui.xaml.markup.xamlreader.load)로 로드 된 느슨한 XAML은 연기할 수 없습니다.
- 부모 요소를 이동 하면 인식 되지 않은 모든 요소가 지워집니다.

지연 된 요소를 실현 하는 여러 가지 방법이 있습니다.

- 요소에 대해 정의한 이름으로 [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) 을 호출 합니다.
- 요소에 대해 정의한 이름으로 [**system.windows.frameworkelement.gettemplatechild**](/uwp/api/windows.ui.xaml.controls.control.gettemplatechild) 를 호출 합니다.
- [**Visualstate**](/uwp/api/Windows.UI.Xaml.VisualState)에서 지연 된 요소를 대상으로 하는 [**Setter**](/uwp/api/Windows.UI.Xaml.Setter) 또는 **Storyboard** 애니메이션을 사용 합니다.
- 모든 **Storyboard**에서 지연 된 요소를 대상으로 합니다.
- 지연 된 요소를 대상으로 하는 바인딩을 사용 합니다.

> 참고: 요소 인스턴스화가 시작 되 면 UI 스레드에 생성 되므로 한 번에 너무 많이 생성 된 경우 UI가 끊길 될 수 있습니다.

이전에 나열 된 방법 중 하나에서 지연 된 요소를 만든 후에는 다음과 같은 몇 가지 작업을 수행 합니다.

- 요소에 대해 [**로드**](/uwp/api/windows.ui.xaml.frameworkelement.loaded) 된 이벤트가 발생 합니다.
- 요소에 대 한 모든 바인딩이 평가 됩니다.
- 지연 된 요소를 포함 하는 속성에 대 한 속성 변경 알림을 수신 하도록 등록 한 경우 알림이 발생 합니다.

지연 된 요소는 중첩할 수 있지만의 가장 바깥쪽 요소에서 실현 되어야 합니다. 부모를 인식 하기 전에 자식 요소를 실현 하려고 하면 예외가 발생 합니다.

일반적으로 첫 번째 프레임에서는 볼 수 없는 요소를 지연 하는 것이 좋습니다.연기할 후보를 찾기 위한 좋은 지침은 축소 [**표시**](/uwp/api/windows.ui.xaml.uielement.visibility)를 사용 하 여 생성 되는 요소를 찾는 것입니다. 또한 사용자 상호 작용에 의해 트리거되는 UI는 연기할 수 있는 요소를 찾을 수 있는 좋은 위치입니다.

[**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView)에서 요소를 지연 하는 것은 시작 시간을 단축 하 고, 만드는 항목에 따라 패닝 성능을 낮출 수도 있으므로 주의 해야 합니다. 패닝 성능을 향상 시키려면 [{X:bind} 태그 확장](x-bind-markup-extension.md) 및 [x:phase 특성](x-phase-attribute.md) 설명서를 참조 하세요.

[X:phase 특성](x-phase-attribute.md) 을 **x:DeferLoadStrategy** 와 함께 사용 하는 경우 요소 또는 요소 트리가 실현 될 때 바인딩이 현재 단계까지 적용 됩니다. **X:phase** 에 지정 된 단계는 요소의 지연에 영향을 주거나 제어 하지 않습니다. 목록 항목이 이동의 일부로 재활용 되 면 인식 된 요소는 다른 활성 요소와 같은 방식으로 동작 하 고 컴파일된 바인딩 (**{X:bind}** 바인딩)은 단계적으로 중단를 비롯 한 동일한 규칙을 사용 하 여 처리 됩니다.

일반적인 지침은 원하는 성능을 얻을 수 있도록 하기 전후에 앱의 성능을 측정 하는 것입니다.

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