---
Description: 자연 스러운 필기 및 그리기 환경을 위한 디지털 잉크를 비롯 하 여 펜 및 스타일러스 장치에서 사용자 지정 상호 작용을 지 원하는 Windows 앱을 빌드 하세요.
title: Windows 앱의 펜 조작 및 Windows Ink
ms.assetid: 3DA4F2D2-5405-42A1-9ED9-3A87BCD84C43
label: Pen interactions and Windows Ink in Windows apps
template: detail.hbs
keywords: Windows Ink, Windows Ink, DirectInk, InkPresenter, InkCanvas, 필기 인식, 사용자 조작, 입력
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3733c98c81a23fbc5b369297b45f1c1e5183c198
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173387"
---
# <a name="pen-interactions-and-windows-ink-in-windows-apps"></a>Windows 앱의 펜 조작 및 Windows Ink

![Surface 펜](images/ink/hero-small.png)  
*Surface Pen* ( [Microsoft Store](https://www.microsoft.com/p/surface-pen/8zl5c82qmg6b)에서 구매할 수 있음)

## <a name="overview"></a>개요

Windows 앱에서 펜 입력을 최적화 하 여 표준 [**포인터 장치**](/uwp/api/Windows.Devices.Input.PointerDevice) 기능 및 사용자에 게 가장 적합 한 windows Ink 환경을 제공 합니다.

> [!NOTE]
> 이 항목에서는 Windows Ink 플랫폼에 대해 중점적으로 설명 합니다. 마우스, 터치 및 터치 패드와 유사한 일반적인 포인터 입력 처리의 경우 [포인터 입력 처리](handle-pointer-input.md)를 참조 하세요.

| 동영상 |   |
| --- | --- |
| <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Ink-in-Your-UWP-App/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe> | <iframe src="https://channel9.msdn.com/Events/Ignite/2016/BRK2060/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe> |
| *Windows 앱에서 잉크 사용* | *Windows 펜 및 잉크를 사용 하 여 더욱 매력적인 엔터프라이즈 앱 빌드* |

펜 디바이스와 더불어 Windows Ink 플랫폼을 사용하면 자연스럽게 디지털 필기 메모, 드로잉, 주석을 만들 수 있습니다. 플랫폼에서는 디지타이저 입력을 잉크 데이터로 캡처, 잉크 데이터 생성, 잉크 데이터 관리, 출력 디바이스에서 잉크 데이터를 스트로크로 렌더링 및 잉크를 필기 인식을 통해 텍스트로 변환 등을 지원합니다.

사용자가 쓰거나 그릴 때 펜의 기본 위치와 이동을 캡처하는 것 외에도 앱은 스트로크 전체에서 사용 되는 다양 한 압력을 추적 하 고 수집할 수 있습니다. 이 정보와 펜 팁 모양, 크기 및 회전에 대한 설정, 잉크 색 및 용도(일반 잉크, 지우기, 강조 표시 및 선택)를 사용하여 펜, 연필 또는 브러시를 사용하여 종이에 쓰거나 그리는 것과 매우 비슷한 사용자 환경을 제공할 수 있습니다.

> [!NOTE]
> 앱은 터치 디지타이저 및 마우스 장치를 비롯 한 다른 포인터 기반 장치의 잉크 입력도 지원할 수 있습니다. 

잉크 플랫폼은 매우 유연 합니다. 요구 사항에 따라 다양 한 수준의 기능을 지원 하도록 설계 되었습니다.

Windows Ink UX 지침은 [잉크 컨트롤](../controls-and-patterns/inking-controls.md)을 참조 하세요.

## <a name="components-of-the-windows-ink-platform"></a>Windows Ink 플랫폼의 구성 요소

| 구성 요소 | Description |
| --- | --- |
| [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) | 기본적으로 펜의 모든 입력을 받아 잉크 스트로크 또는 지우기 스트로크로 표시 하는 XAML UI 플랫폼 컨트롤입니다.<br/>InkCanvas를 사용 하는 방법에 대 한 자세한 내용은 [Windows ink 스트로크를 텍스트로 인식](convert-ink-to-text.md) 및 [windows Ink 스트로크 데이터 저장 및 검색](save-and-load-ink.md)을 참조 하세요. |
| [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) | [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 컨트롤([**InkCanvas.InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 속성을 통해 노출)과 함께 인스턴스화되는 코드 숨김 개체입니다. 이 개체는 추가 사용자 지정 및 개인 설정을 위한 포괄적인 Api 집합과 함께 **InkCanvas**에서 노출 하는 모든 기본 잉크 기능을 제공 합니다.<br/>InkPresenter를 사용 하는 방법에 대 한 자세한 내용은 [Windows ink 스트로크를 텍스트로 인식](convert-ink-to-text.md) 및 [windows Ink 스트로크 데이터 저장 및 검색](save-and-load-ink.md)을 참조 하세요. |
| [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) | 연결 된 [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas)의 잉크 관련 기능을 활성화 하는 사용자 지정 가능 하 고 확장 가능한 단추 컬렉션을 포함 하는 XAML UI 플랫폼 컨트롤입니다.<br/>InkToolbar를 사용 하는 방법에 대 한 자세한 내용은 [Windows 앱 잉크 앱에 InkToolbar 모음 추가](ink-toolbar.md)를 참조 하세요. |
| [**IInkD2DRenderer**](/windows/desktop/api/inkrenderer/nn-inkrenderer-iinkd2drenderer) | 기본 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 컨트롤이 아니라 유니버설 Windows 앱의 지정 된 Direct2D 장치 컨텍스트로 잉크 스트로크를 렌더링할 수 있습니다. 이렇게 하면 수동 작업을 완벽 하 게 사용자 지정할 수 있습니다.<br/>자세한 내용은 [복합 잉크 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)을 참조 하세요. |

## <a name="basic-inking-with-inkcanvas"></a>InkCanvas를 사용 하는 기본 잉크

기본 잉크 기능을 추가 하려면 앱의 해당 페이지에 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) UWP 플랫폼 컨트롤을 추가 하면 됩니다.

기본적으로 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 는 펜 에서만 잉크 입력을 지원 합니다. 입력은 색 및 두께 (두께 2 픽셀의 검정색 볼펜)에 대 한 기본 설정을 사용 하 여 잉크 스트로크로 렌더링 되거나, 입력이 지우개 팁에서 입력 된 경우 또는 지우기 단추로 펜 팁이 수정 된 경우 스트로크 지우개로 처리 됩니다.

> [!NOTE]
> 지우개 팁 또는 단추가 없는 경우 펜 팁에서 지우기 스트로크로 입력을 처리 하도록 InkCanvas를 구성할 수 있습니다.

이 예제에서 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 은 배경 이미지를 오버레이 합니다.

> [!NOTE]
> InkCanvas는 [StackPanel](/uwp/api/windows.ui.xaml.controls.stackpanel) 또는 [Grid](/uwp/api/windows.ui.xaml.controls.grid) 컨트롤과 같이 자식 요소의 크기를 자동으로 조정 하는 요소의 자식인 경우를 제외 하 고 기본 [**Height**](/uwp/api/windows.ui.xaml.frameworkelement.Height) 및 [**Width**](/uwp/api/windows.ui.xaml.frameworkelement.Width) 속성은 0입니다.

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

이 이미지 시리즈는이 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 컨트롤에서 펜 입력을 렌더링 하는 방법을 보여 줍니다.

| ![배경 이미지가 있는 빈 InkCanvas](images/ink_basic_1_small.png) | ![잉크 스트로크를 사용 하는 InkCanvas](images/ink_basic_2_small.png) | ![하나의 스트로크가 지워진 InkCanvas](images/ink_basic_3_small.png) |
| --- | --- | ---|
| 배경 이미지가 있는 빈 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) . | 잉크 스트로크를 사용 하는 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 입니다. | 하나의 스트로크가 지워진 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) (일부가 아니라 전체 스트로크에서 지우기가 작동 하는 방식에 유의 하세요.) |

[**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 컨트롤에서 지 원하는 잉크 기능은 [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter)라는 코드 숨겨진 개체에 의해 제공 됩니다.

기본 잉크를 사용 하는 경우에는 [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter)를 사용 하지 않아도 됩니다. 그러나 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)에서 잉크 동작을 사용자 지정 하 고 구성 하려면 해당 하는 **InkPresenter** 개체에 액세스 해야 합니다.

## <a name="basic-customization-with-inkpresenter"></a>InkPresenter를 사용한 기본 사용자 지정

각 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 컨트롤을 사용 하 여 [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) 개체가 인스턴스화됩니다.

> [!NOTE]
> [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) 는 직접 인스턴스화할 수 없습니다. 대신 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)의 [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 속성을 통해 액세스 됩니다. 

해당 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 컨트롤의 모든 기본 잉크 동작을 제공 하는 것과 함께, [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) 는 펜 입력 (표준 및 수정 됨)의 추가 스트로크 사용자 지정 및 세부적인 관리를 위한 포괄적인 api 집합을 제공 합니다. 여기에는 스트로크 속성, 지원 되는 입력 장치 형식, 입력이 개체에서 처리 되는지 또는 처리를 위해 앱에 전달 되는지 여부가 포함 됩니다.

> [!NOTE]
> 펜 팁 또는 지우개 팁/단추의 표준 잉크 입력은 펜 배럴 단추, 오른쪽 마우스 단추 또는 유사한 메커니즘과 같은 보조 하드웨어 affordance 수정 되지 않습니다. 

기본적으로 잉크는 펜 입력에 대해서만 지원 됩니다. 여기서는 펜과 마우스의 입력 데이터를 잉크 스트로크로 해석 하도록 [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 를 구성 합니다. 또한 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)에 스트로크를 렌더링 하는 데 사용 되는 일부 초기 잉크 스트로크 특성도 설정 합니다.

마우스 및 터치 잉크를 사용 하도록 설정 하려면 [**InkPresenter**](/uwp/api/windows.ui.input.inking.inkpresenter) 의 [**inputdevicetypes**](/uwp/api/windows.ui.input.inking.inkpresenter.InputDeviceTypes) 속성을 원하는 [**CoreInputDeviceTypes**](/uwp/api/windows.ui.core.coreinputdevicetypes) 값의 조합으로 설정 합니다.

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

여기서는 사용자가 잉크 색 목록에서 선택할 수 있습니다.

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

그런 다음 선택한 색의 변경을 처리 하 고 그에 따라 잉크 스트로크 특성을 업데이트 합니다.

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

이러한 이미지는 사용자가 [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)에서 펜 입력을 처리 하 고 사용자 지정 하는 방법을 보여 줍니다.

| ![기본 검정 잉크 스트로크를 사용 하는 inkcanvas](images/ink-basic-custom-1-small.png) | ![사용자가 빨간색 잉크 스트로크를 선택한 inkcanvas](images/ink-basic-custom-2-small.png) |
| --- | --- |
| 기본 검정 잉크 스트로크를 사용 하는 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 입니다. | [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 사용자가 빨간색 잉크 스트로크를 선택 했습니다. | 

잉크 입력 및 지우기 이상의 기능(예: 스트로크 선택)을 제공하려면 앱은 [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)에서 앱에서 처리하기 위해 처리되지 않은 상태로 통과시킬 특정 입력을 식별해야 합니다.

## <a name="pass-through-input-for-advanced-processing"></a>고급 처리를 위한 통과 입력

기본적으로, [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 는 펜 배럴 단추, 오른쪽 마우스 단추 또는 이와 같은 보조 하드웨어 affordance에서 수정한 입력을 포함 하 여 모든 입력을 잉크 스트로크 또는 지우기 스트로크로 처리 합니다. 그러나 일반적으로 사용자는 이러한 보조 affordances를 사용 하 여 몇 가지 추가 기능이 나 수정 된 동작을 필요로 합니다.

경우에 따라 보조 affordances (일반적으로 펜 팁과 연결 되지 않은 기능), 다른 입력 장치 형식 또는 앱 UI에서 사용자가 선택한 항목에 따라 수정 된 동작의 일부 형식의 펜에 대 한 추가 기능을 노출 해야 할 수도 있습니다.

이를 지원하기 위해 특정 입력을 처리되지 않은 상태로 두도록 [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)를 구성할 수 있습니다. 처리 되지 않은이 입력은 처리를 위해 앱으로 전달 됩니다.

### <a name="example---use-unprocessed-input-to-implement-stroke-selection"></a>예제-처리 되지 않은 입력을 사용 하 여 스트로크 선택 구현 

Windows 잉크 플랫폼에서는 스트로크 선택과 같이 수정 된 입력이 필요한 작업에 대 한 기본 제공 지원을 제공 하지 않습니다. 이와 같은 기능을 지원 하려면 앱에서 사용자 지정 솔루션을 제공 해야 합니다. 

다음 코드 예제 (모든 코드는 MainPage.xaml.cs 및 파일에 포함 되어 있습니다.)는 펜 배럴 단추 (또는 마우스 오른쪽 단추)를 사용 하 여 입력을 수정할 때 스트로크 선택을 사용 하도록 설정 하는 방법을 단계별로 안내 합니다.

1.  먼저 MainPage에서 UI를 설정 합니다.

    여기에서 캔버스 ( [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)아래)를 추가 하 여 선택 스트로크를 그립니다. 별도의 계층을 사용 하 여 선택 스트로크를 그리면 **InkCanvas** 및 해당 콘텐츠가 그대로 유지 됩니다.

    ![기본 선택 캔버스가 있는 빈 inkcanvas](images/ink-unprocessed-1-small.png)

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

2.  MainPage.xaml.cs에서는 선택 UI의 여러 측면에 대 한 참조를 유지 하기 위해 몇 가지 전역 변수를 선언 합니다. 특히 선택 된 올가미 스트로크와 선택 된 스트로크를 강조 표시 하는 경계 사각형입니다.

      ```csharp
        // Stroke selection tool.
        private Polyline lasso;
        // Stroke selection area.
        private Rect boundingRect;
      ```

3.  그런 다음 펜과 마우스 모두의 입력 데이터를 잉크 스트로크로 해석 하도록 [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 를 구성 하 고 스트로크 렌더링에 사용 되는 일부 초기 잉크 스트로크 특성을 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)로 설정 합니다.

    가장 중요 한 점은 [InkPresenter](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 의 [**InputProcessingConfiguration**](/uwp/api/windows.ui.input.inking.inkpresenter.inputprocessingconfiguration) 속성을 사용 하 여 수정 된 입력이 앱에서 처리 되어야 함을 나타내는 것입니다. 수정 된 입력은 **InputProcessingConfiguration** 의 [**InkInputRightDragAction**](/uwp/api/Windows.UI.Input.Inking.InkInputRightDragAction)값을 할당 하 여 지정 합니다. 이 값이 설정 되 면 [InkPresenter](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 는 처리할 포인터 이벤트 집합인 [Inkunprocessedinput](/uwp/api/windows.ui.input.inking.inkunprocessedinput) 클래스로 전달 됩니다.

    처리 되지 않은 [**Pointerpressed**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerpressed), [**Pointerpressed**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointermoved)및 [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)를 통해 전달 된 [**pointerpressed**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerreleased) 이벤트에 대해 수신기를 할당 합니다. 모든 선택 기능은 이러한 이벤트에 대 한 처리기에서 구현 됩니다.

    마지막으로, [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)의 [**StrokeStarted**](/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted) 및 [**StrokesErased**](/uwp/api/windows.ui.input.inking.inkpresenter.strokeserased) 이벤트에 대 한 수신기를 할당 합니다. 이 이벤트에 대 한 처리기를 사용 하 여 새 스트로크를 시작 하거나 기존 스트로크를 지울 경우 선택 UI를 정리 합니다.

    ![기본 검정 잉크 스트로크를 사용 하는 inkcanvas](images/ink-unprocessed-2-small.png)

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

4.  그런 다음, 해당 [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)를 통해 전달 되는 처리 되지 않은 [**pointerpressed**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerpressed), [**Pointerpressed**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointermoved)및 [**pointerpressed**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerreleased) 이벤트에 대 한 처리기를 정의 합니다.

    모든 선택 기능은 이러한 처리기에서 올가미 스트로크와 경계 사각형을 비롯 하 여 구현 됩니다.

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

5.  PointerReleased 이벤트 처리기를 종료 하려면 모든 콘텐츠의 선택 계층 (올가미 스트로크)을 지운 다음 올가미 영역에 있는 잉크 스트로크 주위에 단일 경계 사각형을 그립니다.

    ![선택 영역 경계 사각형](images/ink-unprocessed-4-small.png)

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

6.  마지막으로 [**StrokeStarted**](/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted) 및 [**StrokesErased**](/uwp/api/windows.ui.input.inking.inkpresenter.strokeserased) InkPresenter 이벤트에 대 한 처리기를 정의 합니다.

    둘 다 동일한 정리 함수를 호출 하 여 새 스트로크가 검색 될 때마다 현재 선택 영역을 지웁니다.

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

7.  다음은 새 스트로크를 시작 하거나 기존 스트로크를 지울 때 선택 캔버스에서 모든 선택 UI를 제거 하는 함수입니다.

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

기본적으로 잉크 입력은 대기 시간이 짧은 백그라운드 스레드에서 처리 되 고 진행 중에 렌더링 되며, 그릴 때 렌더링 됩니다. 스트로크를 완료 하는 경우 (펜 또는 손가락 리프트 또는 마우스 단추를 눌렀다 놓으면) 스트로크가 UI 스레드에서 처리 되 고 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 계층으로 "마른"로 렌더링 됩니다.

이 기본 동작을 재정의 하 고 잉크 스트로크를 "사용자 지정 drying" 하 여 잉크 환경을 완벽 하 게 제어할 수 있습니다. 일반적으로 대부분의 응용 프로그램에서는 기본 동작이 충분 하지만 사용자 지정 drying 필요할 수 있는 몇 가지 경우는 다음과 같습니다.
- 크고 복잡 한 잉크 스트로크 컬렉션을 보다 효율적으로 관리
- 대량 잉크 캔버스에서 보다 효율적인 패닝 및 확대/축소 지원
- Z 순서를 유지 하면서 잉크 및 기타 개체 (예: 도형 또는 텍스트)를 인터리빙 합니다. 
- 잉크를 DirectX 모양으로 동기적으로 Drying 하 고 변환 합니다 (예: 별도의 **InkCanvas** 레이어가 아닌 선 또는 응용 프로그램 콘텐츠에 통합 된 직선 또는 도형).

사용자 지정 drying에는 잉크 입력을 관리 하 고 기본 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 컨트롤 대신 유니버설 Windows 앱의 Direct2D 장치 컨텍스트로 렌더링 하기 위해 [**IInkD2DRenderer**](/windows/desktop/api/inkrenderer/nn-inkrenderer-iinkd2drenderer) 개체가 필요 합니다.

앱은 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 를 로드 하기 전에 [**ActivateCustomDrying**](/uwp/api/windows.ui.input.inking.inkpresenter.activatecustomdrying) 를 호출 하 여 잉크 스트로크를 [**SurfaceImageSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) 또는 [**VirtualSurfaceImageSource**](/uwp/api/windows.ui.xaml.media.imaging.virtualsurfaceimagesource)에 렌더링 하는 방법을 사용자 지정 하는 [**inksynchronizer 장치**](/uwp/api/Windows.UI.Input.Inking.InkSynchronizer) 개체를 만듭니다. 

[**SurfaceImageSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) 및 [**VirtualSurfaceImageSource**](/uwp/api/windows.ui.xaml.media.imaging.virtualsurfaceimagesource) 는 모두 응용 프로그램의 콘텐츠를 그리거나 구성 하기 위해 앱에 대 한 DirectX 공유 화면을 제공 합니다. 하지만 vsis는 성능 이동 및 확대/축소를 위해 화면 보다 큰 가상 표면을 제공 합니다. 이러한 화면에 대 한 시각적 업데이트는 XAML UI 스레드와 동기화 되기 때문에 잉크가 렌더링 되 면 InkCanvas에서 동시에가는 잉크를 제거할 수 있습니다. 

[SwapChainPanel](/uwp/api/windows.ui.xaml.controls.swapchainpanel)에 대 한 잉크를 사용자 지정할 수도 있지만 UI 스레드와의 동기화가 보장 되지 않으며 잉크가 SwapChainPanel에 렌더링 되는 시간과 InkCanvas에서 잉크가 제거 될 때 사이에 지연이 발생할 수 있습니다.

이 기능의 전체 예는 [복합 잉크 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)을 참조 하세요.

> [!NOTE]
> 사용자 지정 drying 및 [ **inktoolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar)  
> 앱이 사용자 지정 drying 구현을 사용 하 여 [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) 의 기본 잉크 렌더링 동작을 재정의 하는 경우에는 렌더링 된 잉크 스트로크를 InkToolbar 모음에서 더 이상 사용할 수 없으며, inktoolbar의 기본 제공 지우기 명령이 제대로 작동 하지 않습니다. 지우기 기능을 제공 하려면 모든 포인터 이벤트를 처리 하 고, 각 스트로크에 대해 적중 테스트를 수행 하 고, 기본 제공 되는 "모든 잉크 지우기" 명령을 재정의 해야 합니다.

## <a name="other-articles-in-this-section"></a>이 단원의 다른 문서

| 항목 | Description |
| --- | --- |
| [잉크 스트로크 인식](convert-ink-to-text.md) | 필기 인식을 사용 하 여 잉크 스트로크를 텍스트로 변환 하거나 사용자 지정 인식을 사용 하 여 도형으로 변환 합니다. |
| [잉크 스트로크 저장 및 검색](save-and-load-ink.md) | 포함 된 ISF (Ink Serialize 된 형식) 메타 데이터를 사용 하 여 GIF (그래픽 교환 형식) 파일에 잉크 스트로크 데이터를 저장 합니다. |
| [Windows 잉크 앱에 InkToolbar 추가](ink-toolbar.md) | Windows 앱 잉크 앱에 기본 InkToolbar를 추가 하 고, InkToolbar에 사용자 지정 펜 단추를 추가 하 고, 사용자 지정 펜 단추를 사용자 지정 펜 정의에 바인딩합니다. |

## <a name="related-articles"></a>관련된 문서

- [시작 하기: Windows 앱에서 잉크 지원](./ink-walkthrough.md)
- [포인터 입력 처리](handle-pointer-input.md)
- [입력 디바이스 식별](identify-input-devices.md)

### <a name="apis"></a>API

- [**Windows.Devices.Input**](/uwp/api/Windows.Devices.Input)
- [**Windows.UI.Input.Inking**](/uwp/api/Windows.UI.Input.Inking)
- [**Windows. i n s.**](/uwp/api/Windows.UI.Input.Inking.Core)

### <a name="samples"></a>샘플

- [자습서 시작: Windows 앱에서 잉크 지원](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
- [단순 잉크 샘플 (c #/C + +)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
- [복합 잉크 샘플 (c + +)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
- [Ink 샘플 (JavaScript)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BJavaScript%5D-Windows%208%20app%20samples/JavaScript/Windows%208%20app%20samples/Input%20Ink%20sample%20(Windows%208))
- [서적 샘플 강조](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [제품군 정보 샘플](https://github.com/Microsoft/Windows-appsample-familynotes)
- [기본 입력 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [짧은 대기 시간 입력 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [사용자 상호 작용 모드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [포커스 화면 효과 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>보관 샘플

- [입력: 장치 기능 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Input: XAML 사용자 입력 이벤트 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [XAML 스크롤, 패닝 및 확대/축소 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [입력: GestureRecognizer를 사용한 제스처 및 조작](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)