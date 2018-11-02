---
author: Karl-Bridge-Microsoft
Description: Build Universal Windows Platform (UWP) apps that support custom interactions from pen and stylus devices, including digital ink for natural writing and drawing experiences.
title: UWP 앱의 펜 조작 및 Windows Ink
ms.assetid: 3DA4F2D2-5405-42A1-9ED9-3A87BCD84C43
label: Pen interactions and Windows Ink in UWP apps
template: detail.hbs
keywords: Windows Ink, Windows 수동 입력, DirectInk, InkPresenter, InkCanvas, 필기 인식, 사용자 조작, 입력
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3d04ea3506fc909b115ba9aab397ded9e4464479
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5946956"
---
# <a name="pen-interactions-and-windows-ink-in-uwp-apps"></a>UWP 앱의 펜 조작 및 Windows Ink

![Surface 펜](images/ink/hero-small.png)  
*Surface 펜*([Microsoft 스토어](https://aka.ms/purchasesurfacepen)에서 구매 가능)

## <a name="overview"></a>개요

UWP(유니버설 Windows 플랫폼) 앱을 펜 입력에 최적화하여 사용자에게 표준 [**포인터 장치**](https://msdn.microsoft.com/library/windows/apps/br225633) 기능과 최상의 Windows Ink 환경을 제공합니다.

> [!NOTE]
> 이 항목은 Windows Ink 플랫폼을 중점적으로 다룹니다. 일반적인 포인터 입력 처리(마우스, 터치 및 터치 패드와 유사)는 [포인터 입력 처리](handle-pointer-input.md)를 참조하세요.

| 비디오 |   |
| --- | --- |
| <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Ink-in-Your-UWP-App/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe> | <iframe src="https://channel9.msdn.com/Events/Ignite/2016/BRK2060/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe> |
| *UWP 앱에서 잉크 사용* | *Windows 펜 및 잉크 매력적인 enterpriseapps 빌드를 사용 하 여* |

펜 장치와 더불어 Windows Ink 플랫폼을 사용하면 자연스럽게 디지털 필기 메모, 드로잉, 주석을 만들 수 있습니다. 플랫폼에서는 디지타이저 입력을 잉크 데이터로 캡처, 잉크 데이터 생성, 잉크 데이터 관리, 출력 장치에서 잉크 데이터를 스트로크로 렌더링 및 잉크를 필기 인식을 통해 텍스트로 변환 등을 지원합니다.

앱은 사용자가 필기를 하거나 그릴 때 펜의 기본 위치 및 움직임을 캡처하는 것 외에, 스트로크에 작용하는 다양한 압력 크기를 추적하고 수집할 수 있습니다. 이 정보와 펜 팁 모양, 크기 및 회전에 대한 설정, 잉크 색 및 용도(일반 잉크, 지우기, 강조 표시 및 선택)를 사용하여 펜, 연필 또는 브러시를 사용하여 종이에 쓰거나 그리는 것과 매우 비슷한 사용자 환경을 제공할 수 있습니다.

> [!NOTE]
> 앱은 터치 디지타이저 및 마우스 장치를 비롯한 기타 포인터 기반 장치의 잉크 입력을 지원합니다. 

잉크 플랫폼은 매우 유연합니다. 사용자의 요구 수준에 따라 다양한 수준의 기능을 지원하도록 디자인되어 있습니다.

Windows Ink UX 지침은 [수동 입력 컨트롤](../controls-and-patterns/inking-controls.md)을 참조하세요.

## <a name="components-of-the-windows-ink-platform"></a>Windows Ink 플랫폼의 구성 요소

| 구성 요소 | 설명 |
| --- | --- |
| [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) | 기본적으로 받아 펜의 모든 입력을 잉크 스트로크 또는 지우기 스트로크로 표시는 XAMLUI 플랫폼 컨트롤입니다.<br/>InkCanvas를 사용하는 방법은 [Windows Ink 스트로크를 텍스트로 인식](convert-ink-to-text.md) 및 [Windows Ink 스트로크 데이터 저장 및 검색](save-and-load-ink.md)을 참조하세요. |
| [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) | [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 컨트롤([**InkCanvas.InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 속성을 통해 노출)과 함께 인스턴스화되는 코드 숨김 개체입니다. 이 개체는 **InkCanvas**에서 노출하는 모든 기본 수동 입력 기능과 추가 사용자 지정 및 개인 설정을 위한 포괄적인 API 집합을 제공합니다.<br/>InkPresenter를 사용하는 방법은 [Windows Ink 스트로크를 텍스트로 인식](convert-ink-to-text.md) 및 [Windows Ink 스트로크 데이터 저장 및 검색](save-and-load-ink.md)을 참조하세요. |
| [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) | 관련된 된 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)에서 잉크 관련 기능을 활성화 하는 단추, 사용자 지정 및 확장이 가능한 컬렉션을 포함 하는 XAMLUI 플랫폼 컨트롤입니다.<br/>InkToolbar를 사용하는 방법은 [UWP(유니버설 Windows 플랫폼) 수동 입력 앱에 InkToolbar 추가](ink-toolbar.md)를 참조하세요. |
| [**IInkD2DRenderer**](https://msdn.microsoft.com/library/mt147263) | 기본 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 컨트롤 대신 유니버설 Windows 앱의 지정된 Direct2D 장치 컨텍스트 위에 잉크 스트로크를 렌더링할 수 있도록 합니다. 이렇게 하면 수동 입력 환경을 완전히 사용자 지정할 수 있습니다.<br/>자세한 내용은 [복잡한 잉크 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620314)을 참조하세요. |

## <a name="basic-inking-with-inkcanvas"></a>InkCanvas를 사용하는 기본 수동 입력

기본 수동 입력 기능을 추가하려면 앱의 적절한 페이지에 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) UWP 플랫폼 컨트롤을 배치합니다.

기본적으로 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)는 펜의 잉크 입력만 지원합니다. 입력은 색 및 두께(두께가 2픽셀인 검은색 볼펜)의 기본 설정을 사용하는 잉크 스트로크로 렌더링되거나 스트로크 지우개로 처리됩니다(지우기 끝에서 입력이 나오거나 지우기 단추를 사용하여 펜 팁을 수정한 경우).

> [!NOTE]
> 지우개 팁 또는 단추가 없는 경우 InkCanvas는 펜 팁에서 입력을 지우기 스트로크로 처리하도록 구성할 수 있습니다.

이 예제에서 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)는 배경 이미지를 오버레이합니다.

> [!NOTE]
> InkCanvas의 기본 [**높이**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Height)와 [**너비**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Width)는 자녀 요소의 크기를 자동으로 설정하는 요소의 자식이 아닌 경우 0입니다. 

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink sample"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />            
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

여기에 나오는 일련의 이미지는 펜 입력이 이 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 컨트롤에 의해 렌더링되는 방식을 보여 줍니다.

| ![배경 이미지가 있는 비운 InkCanvas](images/ink_basic_1_small.png) | ![잉크 스트로크를 사용하는 InkCanvas](images/ink_basic_2_small.png) | ![하나의 스트로크가 지워진 InkCanvas](images/ink_basic_3_small.png) |
| --- | --- | ---|
| 배경 이미지를 사용하여 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)를 비웁니다. | 잉크 스트로크를 사용하는 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)입니다. | 하나의 스트로크가 지워진 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)입니다(지우기 기능이 일부가 아닌 전체 스트로크에 작동하는 방식을 확인합니다). |

[**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 컨트롤에서 지원되는 잉크 기능은 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011)라는 코드 숨김 개체에서 제공합니다.

기본 수동 입력을 위해 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011)를 사용할 필요는 없습니다. 그러나 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)에 대해 수동 입력 동작을 사용자 지정하고 구성하려면 해당하는 **InkPresenter** 개체에 액세스해야 합니다.

## <a name="basic-customization-with-inkpresenter"></a>InkPresenter를 사용한 기본 사용자 지정

[**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) 개체는 각 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 컨트롤을 사용하여 인스턴스화됩니다.

> [!NOTE]
> [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011)는 직접 인스턴스화될 수 없습니다. 대신, [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn899081)의 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn858535) 속성을 통해 액세스됩니다. 

해당 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 컨트롤의 모든 기본 잉크 입력 동작을 제공할 뿐만 아니라, [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011)는 추가 스트로크 사용자 지정 및 세분화된 펜 입력 관리(표준 및 수정된)를 위한 포괄적인 API 집합을 제공합니다. 여기에는 스트로크 속성, 지원되는 입력 장치 유형, 입력을 개체에서 처리하는지 아니면 앱으로 보내서 처리하는지 여부가 포함됩니다.

> [!NOTE]
> 표준 잉크 입력(펜 팁 또는 지우개 팁/단추의)은 펜 단추, 마우스 오른쪽 단추 또는 유사한 메커니즘과 같은 보조 하드웨어 기능을 사용하여 수정되지 않습니다. 

기본적으로 잉크는 펜 입력에만 지원됩니다. 여기서 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)는 펜과 마우스의 입력 데이터를 모두 잉크 스트로크로 해석하도록 구성됩니다. 또한 스트로크를 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)로 렌더링하는 데 사용되는 일부 초기 잉크 스트로크 특성도 설정합니다.

마우스와 터치 수동 입력을 사용하려면, [**InkPresenter**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.inkpresenter)의 [**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.InputDeviceTypes) 속성을 원하는 [**CoreInputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.core.coreinputdevicetypes) 값의 조합으로 설정합니다.

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Set supported inking device types.
    inkCanvas.InkPresenter.InputDeviceTypes =
        Windows.UI.Core.CoreInputDeviceTypes.Mouse |
        Windows.UI.Core.CoreInputDeviceTypes.Pen;

    // Set initial ink stroke attributes.
    InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
    drawingAttributes.Color = Windows.UI.Colors.Black;
    drawingAttributes.IgnorePressure = false;
    drawingAttributes.FitToCurve = true;
    inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);
}
```

잉크 스트로크 특성은 사용자 기본 설정 또는 앱 요구 사항에 맞게 동적으로 설정할 수 있습니다.

여기서는 잉크 색상 목록에서 선택할 수 있습니다.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink customization sample"
                   VerticalAlignment="Center"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />
        <TextBlock Text="Color:"
                   Style="{StaticResource SubheaderTextBlockStyle}"
                   VerticalAlignment="Center"
                   Margin="50,0,10,0"/>
        <ComboBox x:Name="PenColor"
                  VerticalAlignment="Center"
                  SelectedIndex="0"
                  SelectionChanged="OnPenColorChanged">
            <ComboBoxItem Content="Black"/>
            <ComboBoxItem Content="Red"/>
        </ComboBox>
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

그런 다음 선택한 색상의 변경 내용을 처리하고 그에 따라 잉크 스트로크 특성을 업데이트합니다.

```csharp
// Update ink stroke color for new strokes.
private void OnPenColorChanged(object sender, SelectionChangedEventArgs e)
{
    if (inkCanvas != null)
    {
        InkDrawingAttributes drawingAttributes =
            inkCanvas.InkPresenter.CopyDefaultDrawingAttributes();

        string value = ((ComboBoxItem)PenColor.SelectedItem).Content.ToString();

        switch (value)
        {
            case "Black":
                drawingAttributes.Color = Windows.UI.Colors.Black;
                break;
            case "Red":
                drawingAttributes.Color = Windows.UI.Colors.Red;
                break;
            default:
                drawingAttributes.Color = Windows.UI.Colors.Black;
                break;
        };

        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);
    }
}
```

다음 이미지는 펜 입력이 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)에 의해 처리 및 사용자 지정되는 방식을 보여 줍니다.

| ![기본 검은색 잉크 스트로크를 사용하는 InkCanvas](images/ink-basic-custom-1-small.png) | ![사용자가 선택한 빨간색 잉크 스트로크를 사용하는 InkCanvas](images/ink-basic-custom-2-small.png) |
| --- | --- |
| 기본 검은색 잉크 스트로크를 사용하는 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) | 사용자가 선택한 빨간색 잉크 스트로크를 사용하는 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) | 

잉크 입력 및 지우기 이상의 기능(예: 스트로크 선택)을 제공하려면 앱은 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)에서 앱에서 처리하기 위해 처리되지 않은 상태로 통과시킬 특정 입력을 식별해야 합니다.

## <a name="pass-through-input-for-advanced-processing"></a>고급 처리를 위한 통과 입력

기본적으로 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)는 펜 단추, 오른쪽 마우스 단추 등의 보조 하드웨어 기능에 의해 수정되는 입력을 포함하여 모든 입력을 잉크 스트로크 또는 지우기 스트로크로 처리합니다. 그러나, 사용자는 일반적으로 이러한 보조 기능의 사용으로 일부 추가 기능이나 수정된 동작을 기대합니다.

일부 경우 사용자의 앱 UI 선택에 따라 보조 기능이 없는 펜을 위한 추가 기능(보통 펜 팁과 관련되지 않은 기능), 다른 입력 장치 유형 또는 일부 수정된 동작 유형을 지원해야 할 수 있습니다.

이를 지원하기 위해 특정 입력을 처리되지 않은 상태로 두도록 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)를 구성할 수 있습니다. 이 처리되지 않은 입력은 처리를 위해 앱으로 전달됩니다.

### <a name="example---use-unprocessed-input-to-implement-stroke-selection"></a>예제 - 처리되지 않은 입력을 사용하여 스트로크 선택 구현 

Windows Ink 플랫폼에서는 스트로크 선택과 같은 수정된 입력을 요구하는 기본 지원 작업을 제공하자 않습니다. 이러한 기능을 지원하려면 앱에 사용자 지정 솔루션을 제공해야 합니다. 

다음은 펜 단추(또는 마우스 오른쪽 단추)를 사용하여 입력을 수정할 때 스트로크 선택을 활성화하는 방법을 보여주는 코드 예입니다(전체 코드는 MainPage.xaml 및 MainPage.xaml.cs 파일에 있음).

1.  먼저 MainPage.xaml에서 UI를 설정합니다.

    여기서는 선택 스트로크를 그릴 캔버스를 추가합니다([**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 아래). 별도의 계층을 사용하여 선택 스트로크를 그리면 **InkCanvas**를 빠져 나가고 콘텐츠는 원래 상태가 됩니다.

    ![기본 선택 캔버스가 있는 빈 InkCanvas](images/ink-unprocessed-1-small.png)

      ```xaml
        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
          <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
          </Grid.RowDefinitions>
          <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="Header"
              Text="Advanced ink customization sample"
              VerticalAlignment="Center"
              Style="{ThemeResource HeaderTextBlockStyle}"
              Margin="10,0,0,0" />
          </StackPanel>
          <Grid Grid.Row="1">
            <!-- Canvas for displaying selection UI. -->
            <Canvas x:Name="selectionCanvas"/>
            <!-- Inking area -->
            <InkCanvas x:Name="inkCanvas"/>
          </Grid>
        </Grid>
      ```

2.  MainPage.xaml.cs에서는 선택 UI의 다양한 측면에 대한 참조를 유지하기 위해 몇 개의 전역 변수를 선언합니다. 특히, 선택한 스트로크를 강조 표시하는 경계 직사각형과 선택 올가미 스트로크가 여기에 해당됩니다.

      ```csharp
        // Stroke selection tool.
        private Polyline lasso;
        // Stroke selection area.
        private Rect boundingRect;
      ```

3.  다음에는 펜 및 마우스의 입력 데이터를 모두 잉크 스트로크로 해석하도록 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)를 구성하고, 스트로크 렌더링에 사용되는 일부 초기 잉크 스트로크 특성을 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)로 설정합니다.

    가장 중요한 것은, [InkPresenter](https://msdn.microsoft.com/library/windows/apps/dn899081)의 [**InputProcessingConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn948764) 속성을 사용하여 수정된 입력이 앱을 통해 처리되어야 함을 나타내는 것입니다. 수정된 입력은 **InputProcessingConfiguration.RightDragAction**에 [**InkInputRightDragAction.LeaveUnprocessed**](https://msdn.microsoft.com/library/windows/apps/dn948760) 값을 할당하여 지정합니다. 이 값이 설정되면 [InkPresenter](https://msdn.microsoft.com/library/windows/apps/dn899081)가 [InkUnprocessedInput](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput) 클래스(개발자가 처리할 수 있는 포인터 이벤트 집합)를 통과합니다.

    [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)에 의해 통과되어 처리되지 않은 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/dn914712), [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/dn914711) 및 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/dn914713) 이벤트를 위해 수신기를 할당합니다. 이러한 이벤트의 처리기에서 모든 선택 기능이 구현됩니다.

    마지막으로 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn914702)의 [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn948767) 및 [**StrokesErased**](https://msdn.microsoft.com/library/windows/apps/dn899081) 이벤트에 대한 수신기를 할당합니다. 새 스트로크가 시작되거나 기존 스트로크가 지워진 경우 이러한 이벤트의 처리기를 선택 UI를 정리합니다.

    ![기본 검은색 잉크 스트로크를 사용하는 InkCanvas](images/ink-unprocessed-2-small.png)

      ```csharp
        public MainPage()
        {
          this.InitializeComponent();

          // Set supported inking device types.
          inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

          // Set initial ink stroke attributes.
          InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
          drawingAttributes.Color = Windows.UI.Colors.Black;
          drawingAttributes.IgnorePressure = false;
          drawingAttributes.FitToCurve = true;
          inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

          // By default, the InkPresenter processes input modified by
          // a secondary affordance (pen barrel button, right mouse
          // button, or similar) as ink.
          // To pass through modified input to the app for custom processing
          // on the app UI thread instead of the background ink thread, set
          // InputProcessingConfiguration.RightDragAction to LeaveUnprocessed.
          inkCanvas.InkPresenter.InputProcessingConfiguration.RightDragAction =
              InkInputRightDragAction.LeaveUnprocessed;

          // Listen for unprocessed pointer events from modified input.
          // The input is used to provide selection functionality.
          inkCanvas.InkPresenter.UnprocessedInput.PointerPressed +=
              UnprocessedInput_PointerPressed;
          inkCanvas.InkPresenter.UnprocessedInput.PointerMoved +=
              UnprocessedInput_PointerMoved;
          inkCanvas.InkPresenter.UnprocessedInput.PointerReleased +=
              UnprocessedInput_PointerReleased;

          // Listen for new ink or erase strokes to clean up selection UI.
          inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
              StrokeInput_StrokeStarted;
          inkCanvas.InkPresenter.StrokesErased +=
              InkPresenter_StrokesErased;
        }
      ```

4.  그런 후에는 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn914712)에 의해 전달된 처리되지 않은 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/dn914711), [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/dn914713) 및 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/dn899081) 이벤트에 대한 처리기를 정의합니다.

    올가미 스트로크 및 경계 사각형을 비롯한 모든 선택 기능이 이러한 처리기에서 구현됩니다.

    ![선택 올가미](images/ink-unprocessed-3-small.png)

      ```csharp
        // Handle unprocessed pointer events from modified input.
        // The input is used to provide selection functionality.
        // Selection UI is drawn on a canvas under the InkCanvas.
        private void UnprocessedInput_PointerPressed(
          InkUnprocessedInput sender, PointerEventArgs args)
        {
          // Initialize a selection lasso.
          lasso = new Polyline()
          {
            Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
              StrokeThickness = 1,
              StrokeDashArray = new DoubleCollection() { 5, 2 },
              };

              lasso.Points.Add(args.CurrentPoint.RawPosition);

              selectionCanvas.Children.Add(lasso);
          }

          private void UnprocessedInput_PointerMoved(
            InkUnprocessedInput sender, PointerEventArgs args)
          {
            // Add a point to the lasso Polyline object.
            lasso.Points.Add(args.CurrentPoint.RawPosition);
          }

          private void UnprocessedInput_PointerReleased(
            InkUnprocessedInput sender, PointerEventArgs args)
          {
            // Add the final point to the Polyline object and
            // select strokes within the lasso area.
            // Draw a bounding box on the selection canvas
            // around the selected ink strokes.
            lasso.Points.Add(args.CurrentPoint.RawPosition);

            boundingRect =
              inkCanvas.InkPresenter.StrokeContainer.SelectWithPolyLine(
                lasso.Points);

            DrawBoundingRect();
          }
      ```

5.  PointerReleased 이벤트 처리기를 완료하려면 모든 콘텐츠의 선택 계층(올가미 스트로크)을 지운 다음, 올가미 영역으로 둘러싸인 잉크 스트로크 주위에 단일 경계 사각형을 그립니다.

    ![선택 경계 직사각형](images/ink-unprocessed-4-small.png)

      ```csharp
        // Draw a bounding rectangle, on the selection canvas, encompassing
        // all ink strokes within the lasso area.
        private void DrawBoundingRect()
        {
          // Clear all existing content from the selection canvas.
          selectionCanvas.Children.Clear();

          // Draw a bounding rectangle only if there are ink strokes
          // within the lasso area.
          if (!((boundingRect.Width == 0) ||
            (boundingRect.Height == 0) ||
            boundingRect.IsEmpty))
            {
              var rectangle = new Rectangle()
              {
                Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
                  StrokeThickness = 1,
                  StrokeDashArray = new DoubleCollection() { 5, 2 },
                  Width = boundingRect.Width,
                  Height = boundingRect.Height
              };

              Canvas.SetLeft(rectangle, boundingRect.X);
              Canvas.SetTop(rectangle, boundingRect.Y);

              selectionCanvas.Children.Add(rectangle);
            }
          }
      ```

6.  마지막으로 [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702) 및 [**StrokesErased**](https://msdn.microsoft.com/library/windows/apps/dn948767) InkPresenter 이벤트에 대한 처리기를 정의합니다.

    이러한 두 항목은 동일한 정리 함수를 호출하여 새 스트로크가 검색될 때마다 현재 선택을 지웁니다.

      ```csharp
        // Handle new ink or erase strokes to clean up selection UI.
        private void StrokeInput_StrokeStarted(
          InkStrokeInput sender, Windows.UI.Core.PointerEventArgs args)
        {
          ClearSelection();
        }

        private void InkPresenter_StrokesErased(
          InkPresenter sender, InkStrokesErasedEventArgs args)
        {
          ClearSelection();
        }
      ```

7.  새 스트로크를 시작하거나 기존 스트로크를 지울 때 선택 캔버스에서 모든 선택 UI를 제거하는 함수는 다음과 같습니다.

      ```csharp
        // Clean up selection UI.
        private void ClearSelection()
        {
          var strokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
          foreach (var stroke in strokes)
          {
            stroke.Selected = false;
          }
          ClearDrawnBoundingRect();
         }

        private void ClearDrawnBoundingRect()
        {
          if (selectionCanvas.Children.Any())
          {
            selectionCanvas.Children.Clear();
            boundingRect = Rect.Empty;
          }
        }
      ```

## <a name="custom-ink-rendering"></a>사용자 지정 잉크 렌더링

기본적으로 잉크 입력은 짧은 대기 시간의 백그라운드 스레드에서 처리되고 그릴 때 진행 중 또는 "젖은" 상태로 렌더링됩니다. 스트로크가 완료되면(펜 또는 손가락을 들거나 마우스 단추를 뗄 때) 스트로크는 UI 스레드에서 처리되고 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 계층(응용 프로그램 콘텐츠 위 계층으로, 젖은 잉크를 대체함)에 대해 "건조" 상태로 렌더링됩니다.

젖은 잉크 스트로크를 "사용자 지정 건조"함으로써 이러한 기본 동작을 재정의하고 잉크 입력 환경을 완전히 제어할 수 있습니다. 일반적으로 대부분의 응용 프로그램에서는 기본 동작으로도 충분하지만, 다음과 같이 사용자 지정 건조가 필요할 수 있는 경우가 몇 가지 있습니다.
- 대용량이거나 복잡한 잉크 스트로크 모음의 보다 효율적인 관리
- 넓은 잉크 캔버스에서의 보다 효율적인 이동 및 확대/축소 지원
- z-순서를 유지하면서 셰이프나 텍스트와 같은 잉크 및 기타 개체 상호 배치 
- DirectX 셰이프로 잉크를 동기적으로 건조하고 변환(예: 별도의 **InkCanvas** 레이어 형식이 아닌 응용 프로그램 콘텐츠로 직선이나 셰이프를 레스터화하고 통합).

건조 상태를 사용자 지정하려면 기본 [**InkCanvas**](https://msdn.microsoft.com/library/mt147263) 컨트롤 대신, 잉크 입력을 관리한 후 유니버설 Windows 앱의 Direct2D 장치 컨텍스트로 렌더링하기 위해 [**IInkD2DRenderer**](https://msdn.microsoft.com/library/windows/apps/dn858535) 개체가 필요합니다.

[**ActivateCustomDrying**](https://msdn.microsoft.com/library/windows/apps/dn922012)([**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)가 로드되기 전에)를 호출하면 앱은 [**InkSynchronizer**](https://msdn.microsoft.com/library/windows/apps/dn903979) 개체를 만들어 잉크 스트로크가 [**SurfaceImageSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) 또는 [**VirtualSurfaceImageSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.virtualsurfaceimagesource)에 대해 건조 상태로 렌더링되는 방식을 사용자 지정합니다. 

VSIS는 고성능의 이동 및 확대/축소를 위해 화면보다 큰 가상 표면을 제공하는 반면, [**SurfaceImageSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) 및 [**VirtualSurfaceImageSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.virtualsurfaceimagesource)는 모두 앱에서 응용 프로그램의 콘텐츠에 그림을 그리고 글을 작성하도록 DirectX 공유 표면을 제공합니다. 이러한 표면에 대한 시작적 업데이트는 XAML UI 스레드와 동기화되므로 잉크가 둘 중 하나로 렌더링되면 젖은 잉크가 InkCanvas에서 동시에 제거됩니다. 

건조 잉크를 [SwapChainPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.swapchainpanel)로 사용자 지정할 수도 있지만, UI 스레드와의 동기화는 보장되지 않으며 잉크가 SwapChainPanel에 렌더링되는 시기와 잉크가 InkCanvas에서 제거되는 시기 간에 지연이 발생할 수 있습니다.

이 기능의 전체 예제를 보려면 [복잡한 잉크 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620314)을 참조하세요.

> [!NOTE]
> 사용자 지정 건조 및 [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)  
> 앱이 사용자 지정 건조를 구현하여 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011)의 기본 잉크 렌더링 동작을 재정의하는 경우 렌더링된 잉크 스트로크를 InkToolbar에서 더 이상 사용할 수 없고 InkToolbar의 기본 제공 지우기 명령이 예상대로 작동하지 않습니다. 지우기 기능을 제공하려면 모든 포인터 이벤트를 처리하고, 각 스트로크에 대해 적중 횟수 테스트를 수행하고, 기본 제공 "모든 잉크 지우기" 명령을 재정의해야 합니다.

## <a name="other-articles-in-this-section"></a>이 섹션의 다른 문서

| 항목 | 설명 |
| --- | --- |
| [잉크 스트로크 인식](convert-ink-to-text.md) | 잉크 스트로크를 필기 인식을 사용하여 텍스트로 변환하거나 사용자 지정 인식을 사용하여 모양으로 변환합니다. |
| [잉크 스트로크 저장 및 검색](save-and-load-ink.md) | 포함된 ISF(Ink Serialized Format) 메타데이터를 사용하여 GIF(Graphics Interchange Format) 파일에 잉크 스트로크 데이터를 저장합니다. |
| [UWP 수동 입력 앱에 InkToolbar 추가](ink-toolbar.md) | UWP(유니버설 Windows 플랫폼) 수동 입력 앱에 기본 InkToolbar를 추가하고, InkToolbar에 사용자 지정 펜 단추를 추가하고, 사용자 지정 펜 정의에 사용자 지정 펜 단추를 바인딩합니다. |

## <a name="related-articles"></a>관련 문서

* [시작: UWP 앱에서 잉크 지원](../../get-started/ink-walkthrough.md)
* [포인터 입력 처리](handle-pointer-input.md)
* [입력 장치 식별](identify-input-devices.md)

**API**

* [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)
* [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)
* [**Windows.UI.Input.Inking.Core**](https://msdn.microsoft.com/library/windows/apps/dn958452)

**샘플**
* [시작 자습서: UWP 앱에서 잉크 지원](https://aka.ms/appsample-ink)
* [간단한 잉크 샘플(C#/C++)](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [복잡한 잉크 샘플(C++)](http://go.microsoft.com/fwlink/p/?LinkID=620314)
* [잉크 샘플(JavaScript)](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [색칠하기 책 샘플](https://aka.ms/cpubsample-coloringbook)
* [가족 메모 샘플](https://aka.ms/cpubsample-familynotessample)
* [기본 입력 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [짧은 대기 시간 입력 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [사용자 조작 모드 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [포커스 화면 효과 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**보관 샘플**
* [입력: 디바이스 기능 샘플](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [입력: XAML 사용자 입력 이벤트 샘플](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [XAML 스크롤, 이동 및 확대/축소 샘플](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [입력: GestureRecognizer를 사용한 조작 및 제스처](http://go.microsoft.com/fwlink/p/?LinkID=231605)
