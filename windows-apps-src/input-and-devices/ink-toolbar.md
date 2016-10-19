---
author: Karl-Bridge-Microsoft
Description: "UWP(유니버설 Windows 플랫폼) 수동 입력 앱에 기본 InkToolbar를 추가하고, InkToolbar에 사용자 지정 펜 단추를 추가하고, 사용자 지정 펜 정의에 사용자 지정 펜 단추를 바인딩합니다."
title: "UWP(유니버설 Windows 플랫폼) 수동 입력 앱에 InkToolbar 추가"
label: Add an InkToolbar to a Universal Windows Platform (UWP) inking app
template: detail.hbs
keyword: Windows Ink, Windows Inking, DirectInk, InkPresenter, InkCanvas, InkToolbar, Universal Windows Platform, UWP
translationtype: Human Translation
ms.sourcegitcommit: 71b73605bab71dad36977d0506c090c34359a3e2
ms.openlocfilehash: c4a5b0ae2893fda7697457b9e7449a996707de4b

---

# UWP(유니버설 Windows 플랫폼) 수동 입력 앱에 InkToolbar 추가

UWP(유니버설 Windows 플랫폼) 앱에서 수동 입력을 간편하게 하는 두 가지 컨트롤은 [**InkCanvas**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) 및 [**InkToolbar**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)입니다.

[**InkCanvas**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) 컨트롤은 기본적인 Windows Ink 기능을 제공합니다. 이 컨트롤을 사용하여 펜 입력을 잉크 스트로크(색과 두께에 기본 설정 사용) 또는 지우기 스트로크로 렌더링할 수 있습니다.

> InkCanvas 구현에 대한 자세한 내용은 [UWP 앱에서 펜 및 스타일러스 조작](pen-and-stylus-interactions.md)을 참조하세요.

완전한 투명 오버레이인 InkCanvas는 잉크 스트로크 속성을 설정하기 위한 기본 제공 UI를 제공하지 않습니다. 기본 수동 입력 환경을 변경하고, 사용자가 잉크 스트로크 속성을 설정할 수 있게 하고, 다른 사용자 지정 수동 입력 기능을 지원하려는 경우 다음 두 가지 옵션이 있습니다.

- 코드 숨김에서 InkCanvas에 바인딩된 기본 [**InkPresenter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx) 개체를 사용합니다.

  InkPresenter API는 수동 입력 환경의 광범위한 사용자 지정을 지원합니다. 자세한 내용은 [UWP 앱에서 펜 및 스타일러스 조작](pen-and-stylus-interactions.md)을 참조하세요.

- [**InkToolbar**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)를 InkCanvas에 바인딩합니다. 기본적으로 InkToolbar는 잉크 기능을 활성화하고 스트로크 크기, 잉크 색, 펜 팁 모양 등의 잉크 속성을 설정하기 위한 기본 UI를 제공합니다.

  이 항목에서는 InkToolbar에 대해 설명합니다.

## 중요 API

  -   [**InkCanvas 클래스**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx)
  -   [**InkToolbar 클래스**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)
  -   [**InkPresenter 클래스**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx)
  -   [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)


## 기본 InkToolbar

기본적으로 InkToolbar에는 그리기, 지우기, 강조 표시 및 눈금자 표시 단추가 포함되어 있습니다. 기능에 따라 잉크 색, 스트로크 두께, 모든 잉크 지우기 등의 기타 설정 및 명령이 플라이아웃에 제공됩니다.

![InkToolbar](.\images\ink\ink-tools-invoked-toolbar-small.png)  
*기본 Windows Ink 도구 모음*

기본 InkToolbar를 추가하려면
1. MainPage.xaml에서 수동 입력 화면에 대한 컨테이너 개체(이 예제에서는 그리드 컨트롤 사용)를 선언합니다.
2. InkCanvas 개체를 컨테이너의 자식으로 선언합니다. InkCanvas 크기는 컨테이너에서 상속됩니다.
3. InkToolbar를 선언하고 TargetInkCanvas 특성을 사용하여 InkCanvas에 바인딩합니다.
  InkCanvas 뒤에 InkToolbar를 선언해야 합니다. 순서가 바뀌면 InkCanvas 오버레이로 인해 InkToolbar에 액세스할 수 없게 됩니다.

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
        <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
    </Grid>
</Grid>
```

## 기본 사용자 지정

이 섹션에서는 몇 가지 기본적인 Windows Ink 도구 모음 사용자 지정 시나리오에 대해 설명합니다.

### 선택되는 단추 지정  
![초기화 시 연필 단추가 선택됨](.\images\ink\ink-tools-default-toolbar.png)  
*초기화 시 연필 단추가 선택된 Windows Ink 도구 모음*

기본적으로 앱을 실행하고 도구 모음을 초기화하면 첫 번째(또는 맨 왼쪽) 단추가 선택됩니다. 기본 Windows Ink 도구 모음에서는 볼펜 단추입니다.

프레임워크에서 기본 제공 단추의 순서를 정의하기 때문에 첫 번째 단추가 기본적으로 활성화하려는 펜이나 도구가 아닐 수도 있습니다.

이 기본 동작을 재정의하고 도구 모음에서 선택되는 단추를 지정할 수 있습니다.

이 예제에서는 볼펜 대신 연필 단추를 선택하고 연필을 활성화하여 기본 도구 모음을 초기화합니다.

1. 이전 예제의 InkCanvas 및 InkToolbar에 대한 XAML 선언을 사용합니다.
2. 코드 숨김에서 [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) 개체의 [Loaded](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.loaded.aspx) 이벤트 처리기를 설정합니다.

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loaded += inkToolbar_Loaded;
  }
  ```

3. [Loaded](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.loaded.aspx) 이벤트 처리기에서 다음을 수행합니다.
  1. 기본 제공 [InkToolbarPencilButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx)에 대한 참조를 가져옵니다.

    [GetToolButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.gettoolbutton.aspx) 메서드에 [InkToolbarTool.Pencil](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbartool.aspx) 개체를 전달하면 [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx)에 대한 [InkToolbarToolButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbartoolbutton.aspx) 개체가 반환됩니다.

  2. [ActiveTool](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.activetool.aspx)을 이전 단계에서 반환된 개체로 설정합니다.

```CSharp
/// <summary>
/// Handle the Loaded event of the InkToolbar.
/// By default, the active tool is set to the first tool on the toolbar.
/// Here, we set the active tool to the pencil button.
/// </summary>
/// <param name="sender"></param>
/// <param name="e"></param>
private void inkToolbar_Loaded(object sender, RoutedEventArgs e)
{
    InkToolbarToolButton pencilButton = inkToolbar.GetToolButton(InkToolbarTool.Pencil);
    inkToolbar.ActiveTool = pencilButton;
}
```

### 기본 제공 단추 지정

![초기화 시 포함되는 특정 단추](.\images\ink\ink-tools-specific.png)  
*초기화 시 포함되는 특정 단추*

설명했듯이 Windows Ink 도구 모음에는 기본 제공 단추 컬렉션이 포함되어 있습니다. 이러한 단추는 다음 순서(왼쪽에서 오른쪽)로 표시됩니다.

- [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx)
- [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx)
- [InkToolbarHighlighterButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarhighlighterbutton.aspx)
- [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx)
- [InkToolbarRulerButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarrulerbutton.aspx)

이 예제에서는 기본 제공 볼펜, 연필 및 지우개 단추만 포함하여 도구 모음을 초기화합니다.

XAML 또는 코드 숨김을 사용하여 이 작업을 수행할 수 있습니다.

**XAML**

첫 번째 예제의 InkCanvas 및 InkToolbar에 대한 XAML 선언을 수정합니다.
- [InitialControls](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.initialcontrols.aspx) 특성을 추가하고 해당 값을 "[None](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarinitialcontrols.aspx)"으로 설정합니다. 그러면 기본 제공 단추 컬렉션이 지워집니다.
- 앱에 필요한 특정 InkToolbar 단추를 추가합니다. 여기서는 [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx), [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx), [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx)만 추가합니다.
> [!NOTE]
> 단추는 여기서 지정된 순서가 아니라 프레임워크에서 정의된 순서대로 도구 모음에 추가됩니다.

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
        <!-- Clear the default InkToolbar buttons by setting InitialControls to None. -->
        <!-- Set the active tool to the pencil button. -->
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar"
                    VerticalAlignment="Top"
                    TargetInkCanvas="{x:Bind inkCanvas}"
                    InitialControls="None">
            <!--
             Add only the ballpoint pen, pencil, and eraser.
             Note that the buttons are added to the toolbar in the order
             defined by the framework, not the order we specify here.
            -->
            <InkToolbarEraserButton />
            <InkToolbarBallpointPenButton />
            <InkToolbarPencilButton/>
        </InkToolbar>
    </Grid>
</Grid>
```

**코드 숨김**
1. 첫 번째 예제의 InkCanvas 및 InkToolbar에 대한 XAML 선언을 사용합니다.

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
          <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
      </Grid>
  </Grid>
  ```

2. 코드 숨김에서 [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) 개체의 [Loading](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.loading.aspx) 이벤트 처리기를 설정합니다.

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loading += inkToolbar_Loading;
  }
  ```

3. [InitialControls](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.initialcontrols.aspx)를 "[None](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarinitialcontrols.aspx)"으로 설정합니다.
4. 앱에 필요한 단추에 대한 개체 참조를 만듭니다. 여기서는 [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx), [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx), [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx)만 추가합니다.
  > [!NOTE]
  > 단추는 여기서 지정된 순서가 아니라 프레임워크에서 정의된 순서대로 도구 모음에 추가됩니다.

5. InkToolbar에 단추를 [추가](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.dependencyobjectcollection.add.aspx)합니다.

  ```csharp
  /// <summary>
  /// Handles the Loading event of the InkToolbar.
  /// Here, we identify the buttons to include on the InkToolbar.
  /// </summary>
  /// <param name="sender">The InkToolbar</param>
  /// <param name="args">The InkToolbar event data.
  /// If there is no event data, this parameter is null</param>
  private void inkToolbar_Loading(FrameworkElement sender, object args)
  {
      // Clear all built-in buttons from the InkToolbar.
      inkToolbar.InitialControls = InkToolbarInitialControls.None;

      // Add only the ballpoint pen, pencil, and eraser.
      // Note that the buttons are added to the toolbar in the order
      // defined by the framework, not the order we specify here.
      InkToolbarBallpointPenButton ballpoint = new InkToolbarBallpointPenButton();
      InkToolbarPencilButton pencil = new InkToolbarPencilButton();
      InkToolbarEraserButton eraser = new InkToolbarEraserButton();
      inkToolbar.Children.Add(eraser);
      inkToolbar.Children.Add(ballpoint);
      inkToolbar.Children.Add(pencil);
  }
  ```

<!--
### Support touch input
By default, the InkToolbar supports both pen and mouse input, you have to enable support for touch input.
-->

## 사용자 지정 단추 및 수동 입력 기능

InkToolbar를 통해 제공되는 단추 컬렉션(및 관련된 수동 입력 기능)을 사용자 지정하고 확장할 수 있습니다.

InkToolbar는 다음 두 가지 그룹의 단추 유형으로 이루어져 있습니다.

1. 기본 제공 그리기, 지우기 및 강조 표시 단추를 포함하는 "도구" 단추 그룹. 사용자 지정 펜과 도구가 여기에 추가됩니다.
> **참고**&nbsp;&nbsp;기능 선택은 함께 사용할 수 없습니다.

2. 기본 제공 눈금자 단추를 포함하는 "토글" 단추 그룹. 사용자 지정 토글이 여기에 추가됩니다.
> **참고**&nbsp;&nbsp;기능은 함께 사용할 수 있으며 다른 활성 도구와 동시에 사용할 수 있습니다.

응용 프로그램 및 필요한 수동 입력 기능에 따라 사용자 지정 잉크 기능에 바인딩된 다음 단추를 InkToolbar에 추가할 수 있습니다.

- 사용자 지정 펜 – 호스트 앱에서 잉크 색상표와 펜 팁 속성(예: 모양, 회전, 크기)이 정의된 펜입니다.
- 사용자 지정 도구 - 호스트 앱에서 정의된 펜 이외의 도구입니다.
- 사용자 지정 토글 - 앱에서 정의된 기능의 상태를 켜짐 또는 꺼짐으로 설정합니다. 켜진 경우 기능이 활성 도구와 함께 작동합니다.

> **참고**&nbsp;&nbsp;기본 제공 단추의 표시 순서는 변경할 수 없습니다. 기본 표시 순서는 볼펜, 연필, 형광펜, 지우개, 눈금자 순입니다. 사용자 지정 펜은 마지막 기본 펜 뒤에 추가되고, 사용자 지정 도구 단추는 마지막 펜 단추와 지우개 단추 사이에 추가되고, 사용자 지정 토글 단추는 눈금자 단추 뒤에 추가됩니다. 사용자 지정 단추는 지정된 순서대로 추가됩니다.

### 사용자 지정 펜

![사용자 지정 붓글씨 펜 단추](.\images\ink\ink-tools-custompen.png)  
*사용자 지정 붓글씨 펜 단추*

이 예제에서는 기본 붓글씨 잉크 스트로크를 사용하도록 설정하는 광범위한 팁을 가진 사용자 지정 펜을 정의합니다. 단추 플라이아웃에 표시되는 색상표의 브러시 컬렉션도 사용자 지정합니다.

**코드 숨김**

먼저, 사용자 지정 펜을 정의하고 코드 숨김에서 그리기 특성을 지정합니다. 나중에서 XAML에서 이 사용자 지정 펜을 참조합니다.

1. 솔루션 탐색기에서 프로젝트를 마우스 오른쪽 단추로 클릭하고 추가 -&gt; 새 항목을 선택합니다.
2. Visual C# -&gt; 코드에서 새 클래스 파일을 추가하고 CalligraphicPen.cs를 호출합니다.
3. Calligraphic.cs에서 기본 using 블록을 다음과 같이 바꿉니다.
```csharp
using System.Numerics;
using Windows.UI;
using Windows.UI.Input.Inking;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
```

4. CalligraphicPen 클래스가 [InkToolbarCustomPen](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompen.aspx)에서 파생되도록 지정합니다.
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
}
```

5. [CreateInkDrawingAttributesCore](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompen.createinkdrawingattributescore.aspx)를 재정의하여 고유한 브러시 및 스트로크 크기를 지정합니다.
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
    }
}
```

6. [InkDrawingAttributes](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.aspx) 개체를 만들고 [펜 팁 모양](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.pentip.aspx), [팁 회전](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.pentiptransform.aspx), [스트로크 크기](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.size.aspx) 및 [잉크 색](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.color.aspx)을 설정합니다.
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
        InkDrawingAttributes inkDrawingAttributes =
          new InkDrawingAttributes();
        inkDrawingAttributes.PenTip = PenTipShape.Circle;
        inkDrawingAttributes.Size =
          new Windows.Foundation.Size(strokeWidth, strokeWidth * 20);
        SolidColorBrush solidColorBrush = brush as SolidColorBrush;
        if (solidColorBrush != null)
        {
            inkDrawingAttributes.Color = solidColorBrush.Color;
        }
        else
        {
            inkDrawingAttributes.Color = Colors.Black;
        }

        Matrix3x2 matrix = Matrix3x2.CreateRotation(45);
        inkDrawingAttributes.PenTipTransform = matrix;

        return inkDrawingAttributes;
    }
}
```

**XAML**

MainPage.xaml에서 사용자 지정 펜에 필요한 참조를 추가합니다.

1. CalligraphicPen.cs에서 정의된 사용자 지정 펜(`CalligraphicPen`) 및 사용자 지정 펜에서 지원하는 [브러시 컬렉션](https://msdn.microsoft.com/en-us/library/windows/apps/Windows.UI.Xaml.Media.BrushCollection.aspx)(`CalligraphicPenPalette`)에 대한 참조를 만드는 로컬 페이지 리소스 사전을 선언합니다.
```xaml
<Page.Resources>
    <!-- Add the custom CalligraphicPen to the page resources. -->
    <local:CalligraphicPen x:Key="CalligraphicPen" />
    <!-- Specify the colors for the palette of the custom pen. -->
    <BrushCollection x:Key="CalligraphicPenPalette">
        <SolidColorBrush Color="Blue" />
        <SolidColorBrush Color="Red" />
    </BrushCollection>
</Page.Resources>
```

2. 그런 다음 자식 [InkToolbarCustomPenButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompenbutton.aspx) 요소가 포함된 InkToolbar를 추가합니다.

  사용자 지정 펜 단추에는 페이지 리소스에서 선언된 두 개의 고정 리소스 참조 `CalligraphicPen` 및 `CalligraphicPenPalette`가 포함되어 있습니다.

  스트로크 크기 슬라이더의 범위([MinStrokeWidth](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.minstrokewidth.aspx), [MaxStrokeWidth](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.maxstrokewidth.aspx), [SelectedStrokeWidth](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.selectedstrokewidthproperty.aspx)), 선택한 브러시([SelectedBrushIndex](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.selectedbrushindex.aspx)) 및 사용자 지정 펜 단추의 아이콘([SymbolIcon](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.symbolicon.aspx))도 지정합니다.
```xaml
<Grid Grid.Row="1">
    <InkCanvas x:Name="inkCanvas" />
    <InkToolbar x:Name="inkToolbar"
                VerticalAlignment="Top"
                TargetInkCanvas="{x:Bind inkCanvas}">
        <InkToolbarCustomPenButton
            CustomPen="{StaticResource CalligraphicPen}"
            Palette="{StaticResource CalligraphicPenPalette}"
            MinStrokeWidth="1" MaxStrokeWidth="3" SelectedStrokeWidth="2"
            SelectedBrushIndex ="1">
            <SymbolIcon Symbol="Favorite" />
            <InkToolbarCustomPenButton.ConfigurationContent>
                <InkToolbarPenConfigurationControl />
            </InkToolbarCustomPenButton.ConfigurationContent>
        </InkToolbarCustomPenButton>
    </InkToolbar>
</Grid>
```

<!--
### Custom toggle

Enable touch inking

>**Note**&nbsp;&nbsp;InkToolbar supports pen and mouse input and can be configured to recognize touch input.
-->


## 관련 문서

* [펜 및 스타일러스 조작](pen-and-stylus-interactions.md)

**샘플**
* [잉크 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [간단한 잉크 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [복잡한 잉크 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620314)



<!--HONumber=Sep16_HO2-->


