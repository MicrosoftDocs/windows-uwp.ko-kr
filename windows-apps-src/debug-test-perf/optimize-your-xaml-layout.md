---
author: jwmsft
ms.assetid: 79CF3927-25DE-43DD-B41A-87E6768D5C35
title: XAML 레이아웃 최적화
description: 레이아웃은 CPU 사용과 메모리 오버헤드 모두에서 비용이 많이 드는 XAML 앱 요소입니다. 다음은 XAML 앱의 레이아웃 성능 향상을 위해 수행할 수 있는 몇 가지 간단한 절차입니다.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.openlocfilehash: 40afd15da7e225ea82814ab2fa680a3c95e00488
ms.sourcegitcommit: ec18e10f750f3f59fbca2f6a41bf1892072c3692
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/14/2017
ms.locfileid: "894747"
---
# <a name="optimize-your-xaml-layout"></a>XAML 레이아웃 최적화

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

**중요 API**

-   [**패널**](https://msdn.microsoft.com/library/windows/apps/BR227511)

레이아웃은 UI의 시각적 구조를 정의하는 프로세스입니다. XAML의 레이아웃을 설명하는 기본 메커니즘은 컨테이너 개체인 패널을 통해서 가능하며 이 패널 내에 UI 요소를 배치 및 정렬할 수 있습니다. 레이아웃은 CPU 사용과 메모리 오버헤드 모두에서 비용이 많이 드는 XAML 앱 요소입니다. 다음은 XAML 앱의 레이아웃 성능 향상을 위해 수행할 수 있는 몇 가지 간단한 절차입니다.

## <a name="reduce-layout-structure"></a>레이아웃 구조 줄이기

UI 요소의 트리 계층 구조를 간소화하면 레이아웃 성능을 가장 크게 개선할 수 있습니다. 패널은 시각적 트리로 존재하지만, [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) 또는 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371)과 같은 *픽셀 생성 요소*가 아닌 구조적 요소입니다. 일반적으로 비픽셀 생성 요소 수를 줄여 트리를 단순화하면 성능을 상당히 개선할 수 있습니다.

여러 UI가 중첩 패널로 구현되므로 결과적으로 패널 및 요소의 깊고 복잡한 트리가 생성됩니다. 패널을 중첩하면 편리하지만 많은 경우에 다소 복잡한 하나의 패널에 동일한 UI를 구현할 수 있습니다. 하나의 패널을 사용하면 성능이 더 개선됩니다.

### <a name="when-to-reduce-layout-structure"></a>레이아웃 구조를 줄일 시기

간단한 방식으로 레이아웃 구조를 줄이면(예: 상위 수준 페이지에서 중첩된 패널 하나 줄이기) 뚜렷한 효과를 거둘 수 없습니다.

[**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 또는 [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705)와 같은 UI에서 반복되는 레이아웃 구조를 줄이면 성능을 가장 크게 개선할 수 있습니다. 이러한 [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/BR242803) 요소는 여러 번 인스턴스화되는 UI 요소의 하위 트리를 정의하는 [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348)을 사용합니다. 동일한 하위 트리가 앱에서 여러 번 중복되는 경우 해당 하위 트리의 성능을 개선하면 앱의 전반적인 성능에 미치는 효과가 배가됩니다.

### <a name="examples"></a>예제

다음 UI를 생각해 보세요.

![양식 레이아웃 예제](images/layout-perf-ex1.png)

이 예제에서는 동일한 UI를 구현하는 3가지 방법을 보여 줍니다. 각 구현 선택 항목이 화면에서 거의 동일한 픽셀로 표시되지만, 구현 세부 정보에서 보면 크게 다릅니다.

옵션 1: 중첩된 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/BR209635) 요소

가장 간단한 모델이지만, 5개의 패널 요소를 사용하므로 상당한 오버헤드가 발생합니다.

```xml
  <StackPanel>
  <TextBlock Text="Options:" />
  <StackPanel Orientation="Horizontal">
      <CheckBox Content="Power User" />
      <CheckBox Content="Admin" Margin="20,0,0,0" />
  </StackPanel>
  <TextBlock Text="Basic information:" />
  <StackPanel Orientation="Horizontal">
      <TextBlock Text="Name:" Width="75" />
      <TextBox Width="200" />
  </StackPanel>
  <StackPanel Orientation="Horizontal">
      <TextBlock Text="Email:" Width="75" />
      <TextBox Width="200" />
  </StackPanel>
  <StackPanel Orientation="Horizontal">
      <TextBlock Text="Password:" Width="75" />
      <TextBox Width="200" />
  </StackPanel>
  <Button Content="Save" />
</StackPanel>
```

옵션 2: 단일 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)

[**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)는 약간 복잡하지만, 단일 패널 요소만 사용합니다.

```xml
  <Grid>
  <Grid.RowDefinitions>
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
      <ColumnDefinition Width="Auto" />
      <ColumnDefinition Width="Auto" />
  </Grid.ColumnDefinitions>
  <TextBlock Text="Options:" Grid.ColumnSpan="2" />
  <CheckBox Content="Power User" Grid.Row="1" Grid.ColumnSpan="2" />
  <CheckBox Content="Admin" Margin="150,0,0,0" Grid.Row="1" Grid.ColumnSpan="2" />
  <TextBlock Text="Basic information:" Grid.Row="2" Grid.ColumnSpan="2" />
  <TextBlock Text="Name:" Width="75" Grid.Row="3" />
  <TextBox Width="200" Grid.Row="3" Grid.Column="1" />
  <TextBlock Text="Email:" Width="75" Grid.Row="4" />
  <TextBox Width="200" Grid.Row="4" Grid.Column="1" />
  <TextBlock Text="Password:" Width="75" Grid.Row="5" />
  <TextBox Width="200" Grid.Row="5" Grid.Column="1" />
  <Button Content="Save" Grid.Row="6" />
</Grid>
```

옵션 3: 단일 [**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/Dn879546):

이 단일 패널은 중첩된 패널을 사용하는 것보다 약간 더 복잡하기도 하지만, [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)보다는 이해하고 유지 관리하기가 더 쉬울 수 있습니다.

```xml
<RelativePanel>
  <TextBlock Text="Options:" x:Name="Options" />
  <CheckBox Content="Power User" x:Name="PowerUser" RelativePanel.Below="Options" />
  <CheckBox Content="Admin" Margin="20,0,0,0" 
            RelativePanel.RightOf="PowerUser" RelativePanel.Below="Options" />
  <TextBlock Text="Basic information:" x:Name="BasicInformation"
           RelativePanel.Below="PowerUser" />
  <TextBlock Text="Name:" RelativePanel.AlignVerticalCenterWith="NameBox" />
  <TextBox Width="200" Margin="75,0,0,0" x:Name="NameBox"               
           RelativePanel.Below="BasicInformation" />
  <TextBlock Text="Email:"  RelativePanel.AlignVerticalCenterWith="EmailBox" />
  <TextBox Width="200" Margin="75,0,0,0" x:Name="EmailBox"
           RelativePanel.Below="NameBox" />
  <TextBlock Text="Password:" RelativePanel.AlignVerticalCenterWith="PasswordBox" />
  <TextBox Width="200" Margin="75,0,0,0" x:Name="PasswordBox"
           RelativePanel.Below="EmailBox" />
  <Button Content="Save" RelativePanel.Below="PasswordBox" />
</RelativePanel>
```

이러한 예제에서 알 수 있듯이 여러 방법으로 동일한 UI를 구현할 수 있습니다. 성능, 가독성 및 유지 관리를 비롯하여 모든 장단점을 신중하게 고려하여 선택해야 합니다.

## <a name="use-single-cell-grids-for-overlapping-ui"></a>UI 겹치기에 단일 셀 그리드 사용

레이아웃에서 요소가 서로 겹칠 수 있어야 하는 것이 일반적인 UI 요구 사항입니다. 요소를 겹쳐서 배치하는 데 일반적으로 안쪽 여백, 여백, 맞춤 및 변형이 사용됩니다. XAML [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) 컨트롤은 겹치는 요소에 대한 레이아웃 성능을 향상하는 데 최적화되어 있습니다.

**중요**  기능 향상 효과를 보려면 단일 셀 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)를 사용하세요. [**RowDefinitions**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.grid.rowdefinitions) 또는 [**ColumnDefinitions**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.grid.columndefinitions)를 정의하지 마세요.

### <a name="examples"></a>예제

```xml
<Grid>
    <Ellipse Fill="Red" Width="200" Height="200" />
    <TextBlock Text="Test" 
               HorizontalAlignment="Center" 
               VerticalAlignment="Center" />
</Grid>
```

![원에서 겹쳐진 텍스트](images/layout-perf-ex2.png)

```xml
<Grid Width="200" BorderBrush="Black" BorderThickness="1">
    <TextBlock Text="Test1" HorizontalAlignment="Left" />
    <TextBlock Text="Test2" HorizontalAlignment="Right" />
</Grid>
```

![그리드의 두 텍스트 블록](images/layout-perf-ex3.png)

## <a name="use-a-panels-built-in-border-properties"></a>패널의 기본 제공 테두리 속성 사용

[**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704), [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/BR209635), [**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/Dn879546) 및 [**ContentPresenter**](https://msdn.microsoft.com/library/windows/apps/BR209378) 컨트롤은 기본 제공 테두리 속성이 있으므로 이러한 속성을 사용하면 XAML에 [**Border**](https://msdn.microsoft.com/library/windows/apps/BR209250) 요소를 추가하지 않고 주위에 테두리를 그릴 수 있습니다. 기본 제공 테두리를 지원하는 새로운 속성은 **BorderBrush**, **BorderThickness**, **CornerRadius** 및 **Padding**입니다. 이러한 각 속성은 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR242362)이므로 바인딩 및 애니메이션과 함께 사용할 수 있습니다. 또한 별도의 **Border** 요소를 완벽히 대체할 수 있도록 설계되었습니다.

UI에서 [**Border**](https://msdn.microsoft.com/library/windows/apps/BR209250) 요소가 이러한 패널 주위에 있는 경우 기본 제공 테두리를 대신 사용합니다. 그러면 앱의 레이아웃 구조에 추가 요소가 저장됩니다. 앞에서 언급했듯이 이를 통해 성능을 크게 개선하고 오버헤드를 줄일 수 있습니다(반복된 UI의 경우 특히).

### <a name="examples"></a>예제

```xml
<RelativePanel BorderBrush="Red" BorderThickness="2" CornerRadius="10" Padding="12">
    <TextBox x:Name="textBox1" RelativePanel.AlignLeftWithPanel="True"/>
    <Button Content="Submit" RelativePanel.Below="textBox1"/>
</RelativePanel>
```

## <a name="use-sizechanged-events-to-respond-to-layout-changes"></a>**SizeChanged** 이벤트를 사용하여 레이아웃 변경에 응답

[**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) 클래스는 레이아웃 변경에 응답하는 두 가지의 유사한 이벤트 즉, [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.layoutupdated) 및 [**SizeChanged**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.sizechanged)를 공개합니다. 이러한 이벤트 중 하나를 사용하면 레이아웃 수행 중 요소 크기가 조정될 때 알림을 받을 수 있습니다. 두 이벤트의 의미 체계가 다르므로 둘 중 하나를 선택하는 데 중요한 성능 고려 사항이 있습니다.

최상의 성능을 위해서는 [**SizeChanged**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.sizechanged)를 선택하는 것이 거의 항상 좋습니다. **SizeChanged**는 직관적인 시맨틱입니다. [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706)의 크기가 업데이트된 경우 레이아웃 중 발생합니다.

[**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.layoutupdated)도 레이아웃 중에 발생하지만, 전역 시맨틱입니다. 즉, 요소가 업데이트될 때마다 모든 요소에서 발생합니다. 이벤트 처리기에서 로컬 처리만 수행되는 것이 일반적이며, 이 경우 코드가 필요한 것보다 더 자주 실행됩니다. 흔하지는 않지만 요소가 크기 변경 없이 재배치되는 때를 알아야 하는 경우에만 **LayoutUpdated**를 사용합니다.

## <a name="choosing-between-panels"></a>패널 중 선택

개별 패널 중에서 선택할 경우 성능은 일반적인 고려 사항이 아닙니다. 일반적으로 구현할 UI에 가장 근접한 레이아웃 동작을 제공하는 패널을 고려하여 선택합니다. 예를 들어 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704), [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/BR209635), [**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/Dn879546) 중에서 선택하는 경우 구현의 멘탈 모델에 가장 근접한 매핑을 제공하는 패널을 선택해야 합니다.

모든 XAML 패널은 우수한 성능에 최적화되어 있으며, 모든 패널은 유사한 UI에 비슷한 성능을 제공합니다.

