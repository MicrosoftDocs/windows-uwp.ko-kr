---
author: Karl-Bridge-Microsoft
Description: "UWP(유니버설 Windows 플랫폼) 수동 입력 앱에 기본 InkToolbar를 추가하고, InkToolbar에 사용자 지정 펜 단추를 추가하고, 사용자 지정 펜 정의에 사용자 지정 펜 단추를 바인딩합니다."
title: "UWP(유니버설 Windows 플랫폼) 수동 입력 앱에 InkToolbar 추가"
label: Add an InkToolbar to a Universal Windows Platform (UWP) inking app
template: detail.hbs
keyword: Windows Ink, Windows Inking, DirectInk, InkPresenter, InkCanvas, InkToolbar, Universal Windows Platform, UWP
translationtype: Human Translation
ms.sourcegitcommit: 9e971104a7f7de9425787f32edcb7c376fb0c934
ms.openlocfilehash: f5c8f7f8e60317a3ef30ff1900d99f9f6d63d391

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

사용자 지정 펜(사용자 지정 펜 단추를 통해 활성화됨)을 만들어 잉크 색상표와 펜 팁 속성(예: 모양, 회전, 크기)을 정의합니다.

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

### 사용자 지정 토글

사용자 지정 토글(사용자 지정 토글 단추를 통해 활성화됨)을 만들어 앱에서 정의된 기능의 상태를 켜짐 또는 꺼짐으로 설정합니다. 켜진 경우 기능이 활성 도구와 함께 작동합니다.

이 예제에서는 터치식 입력을 사용한 수동 입력을 설정하는 사용자 지정 토글 단추를 정의합니다(기본적으로 터치 수동 입력은 사용할 수 없음).

> [!NOTE]  
> 터치하여 수동 입력을 지원해야 하는 경우 이 예에서 지정한 아이콘과 도구 설명과 함께 CustomToggleButton을 사용하여 지원하는 것이 좋습니다.

일반적으로 터치식 입력은 개체나 앱 UI의 직접 조작에 사용됩니다. 터치식 수동 입력을 사용하도록 설정한 경우 동작의 차이를 보여 주기 위해 InkCanvas를 ScrollViewer 컨테이너 내에 배치하고 ScrollViewer 치수를 InkCanvas보다 작게 설정해보겠습니다. 

앱이 시작 될 때 펜 수동 입력만 지원되며 터치는 수동 입력 화면을 확대/축소하거나 이동하는 데 사용됩니다. 터치식 수동 입력을 사용하도록 설정하면 수동 입력 화면은 터치 입력을 통해 이동하거나 확대/축소되지 않습니다.

> [!NOTE]
> [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkCanvas) 및 [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbar) UX 지침 관련 내용은 [수동 입력 컨트롤](..\controls-and-patterns\inking-controls.md)을 참조하세요. 다음 권장 사항이 이 예제와 관련이 있습니다.
> - [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbar) 및 일반적인 수동 입력은 활성 펜을 통해 가장 잘 작동합니다. 그러나 앱에 필요한 경우 마우스와 터치를 사용한 수동 입력을 지원할 수 있습니다. 
> - 터치식 입력을 사용한 수동 입력을 지원하는 경우 “터치 쓰기” 도구 설명과 함께 "Segoe MLD2 자산" 글꼴의 "ED5F" 아이콘을 토글 단추에 사용하는 것이 좋습니다. 

**XAML**

1. 먼저, 이벤트 처리기(Toggle_Custom)를 지정하는 클릭 이벤트 수신기를 사용하여 [**InkToolbarCustomToggleButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbarCustomToggleButton) 요소(toggleButton)를 선언합니다.

```xaml 
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <StackPanel Grid.Row="0" 
                x:Name="HeaderPanel" 
                Orientation="Horizontal">
        <TextBlock x:Name="Header" 
                   Text="Basic ink sample" 
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10" />
    </StackPanel>

    <ScrollViewer Grid.Row="1" 
                  HorizontalScrollBarVisibility="Auto" 
                  VerticalScrollBarVisibility="Auto">
        
        <Grid HorizontalAlignment="Left" VerticalAlignment="Top">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            
            <InkToolbar Grid.Row="0" 
                        Margin="10"
                        x:Name="inkToolbar" 
                        VerticalAlignment="Top"
                        TargetInkCanvas="{x:Bind inkCanvas}">
                <InkToolbarCustomToggleButton 
                x:Name="toggleButton" 
                Click="CustomToggle_Click" 
                ToolTipService.ToolTip="Touch Writing">
                    <SymbolIcon Symbol="{x:Bind TouchWritingIcon}"/>
                </InkToolbarCustomToggleButton>
            </InkToolbar>
            
            <ScrollViewer Grid.Row="1" 
                          Height="500"
                          Width="500"
                          x:Name="scrollViewer" 
                          ZoomMode="Enabled" 
                          MinZoomFactor=".1" 
                          VerticalScrollMode="Enabled" 
                          VerticalScrollBarVisibility="Auto" 
                          HorizontalScrollMode="Enabled" 
                          HorizontalScrollBarVisibility="Auto">
                
                <Grid x:Name="outputGrid" 
                      Height="1000"
                      Width="1000"
                      Background="{ThemeResource SystemControlBackgroundChromeWhiteBrush}">
                    <InkCanvas x:Name="inkCanvas"/>
                </Grid>
                
            </ScrollViewer>
        </Grid>
    </ScrollViewer>
</Grid>
```

**코드 숨김**

2. 이전 코드 조각에서터치식 수동 입력을 위한 사용자 지정 토글 단추(toggleButton)에 클릭 이벤트 수신기와 처리기(Toggle_Custom)를 선언했습니다. 이 처리기는 InkPresenter의 InputDeviceTypes 속성을 통해 CoreInputDeviceTypes.Touch에 대한 지원을 토글합니다.

   또한 SymbolIcon 요소 및 코드 숨김 파일 (TouchWritingIcon)에 정의된 필드로 바인딩하는 {x: Bind} 태그 확장을 사용하여 단추에 아이콘도 지정했습니다.

   다음 코드 조각에는 클릭 이벤트 처리기와 TouchWritingIcon 정의가 둘 다 포함됩니다.

```csharp 
namespace Ink_Basic_InkToolbar
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage_AddCustomToggle : Page
    {
        Symbol TouchWritingIcon = (Symbol)0xED5F;

        public MainPage_AddCustomToggle()
        {
            this.InitializeComponent();
        }

        // Handler for the custom toggle button that enables touch inking.
        private void CustomToggle_Click(object sender, RoutedEventArgs e)
        {
            if (toggleButton.IsChecked == true)
            {
                inkCanvas.InkPresenter.InputDeviceTypes |= CoreInputDeviceTypes.Touch;
            }
            else
            {
                inkCanvas.InkPresenter.InputDeviceTypes &= ~CoreInputDeviceTypes.Touch;
            }
        }
    }
}
```

### 사용자 지정 도구

사용자 지정 도구 단추를 만들어 앱에서 정의한 펜 이외의 도구를 호출할 수 있습니다.

기본적으로 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkPresenter)는 모든 입력을 잉크 스트로크 또는 지우기 스트로크로 처리합니다. 여기에는 펜 단추, 마우스 오른쪽 단추 등과 같은 보조 하드웨어 기능에 의해 수정되는 입력이 포함됩니다. 하지만 특정 입력을 처리되지 않은 상태로 두도록 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkPresenter)를 구성할 수 있습니다. 이렇게 하면 사용자 지정 처리를 위해 처리되지 않은 입력이 앱에 전달될 수 있습니다.

이 예제에서는 선택할 경우 후속 스트로크가 처리되어 잉크 대신 선택 올가미(파선)로 렌더링되게 하는 사용자 지정 도구 단추를 정의하겠습니다. 선택 영역 경계 내에서 모든 잉크 스트로크는 [**선택됨**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkStroke.Selected)으로 설정됩니다.

> [!NOTE]
> InkCanvas 및 InkToolbar UX 지침 관련 내용은 수동 입력 컨트롤을 참조하세요. 다음 권장 사항은 이 예제와 관련이 있습니다.
> - 스트로크 선택을 입력할 때 “선택 도구” 도구 설명과 함께 “Segoe MLD2 자산 글꼴”의 EF20 아이콘을 도구 단추에 사용하는 것이 좋습니다. 
 
**XAML**

1. 먼저, 스트로크 선택이 구성된 이벤트 처리기(customToolButton_Click)를 지정하는 클릭 이벤트 수신기를 사용하여 [**InkToolbarCustomToolButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton) 요소(customToolButton)를 선언합니다. 스트로크 선택을 복사하고 잘라내고 붙여넣는 단추 집합도 추가했습니다.

2. 또한 선택 스트로크를 그릴 수 있도록 캔버스 요소도 추가되었습니다. 별도의 계층을 사용하여 선택 스트로크를 그리면 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkCanvas)와 콘텐츠는 원래 상태로 유지됩니다. 

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header" 
                   Text="Basic ink sample" 
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10,0,0,0" />
    </StackPanel>
    <StackPanel x:Name="ToolPanel" Orientation="Horizontal" Grid.Row="1">
        <InkToolbar x:Name="inkToolbar" 
                    VerticalAlignment="Top" 
                    TargetInkCanvas="{x:Bind inkCanvas}">
            <InkToolbarCustomToolButton 
                x:Name="customToolButton" 
                Click="customToolButton_Click" 
                ToolTipService.ToolTip="Selection tool">
                <SymbolIcon Symbol="{x:Bind SelectIcon}"/>
            </InkToolbarCustomToolButton>
        </InkToolbar>
        <Button x:Name="cutButton" 
                Content="Cut" 
                Click="cutButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
        <Button x:Name="copyButton" 
                Content="Copy"  
                Click="copyButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
        <Button x:Name="pasteButton" 
                Content="Paste"  
                Click="pasteButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
    </StackPanel>
    <Grid Grid.Row="2" x:Name="outputGrid" 
              Background="{ThemeResource SystemControlBackgroundChromeWhiteBrush}" 
              Height="Auto">
        <!-- Canvas for displaying selection UI. -->
        <Canvas x:Name="selectionCanvas"/>
        <!-- Canvas for displaying ink. -->
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

**코드 숨김**

2. 그런 다음 MainPage.xaml.cs 코드 숨김 파일에서 [**InkToolbarCustomToolButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton)에 대한 클릭 이벤트를 처리합니다.

   이 처리기는 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkPresenter)에서 처리되지 않은 입력을 앱으로 전달하도록 구성합니다. 

   이 코드를 통해 자세한 단계는 [UWP 앱에서 펜 조작 및 Windows Ink](pen-and-stylus-interactions.md)의 고급 처리를 위한 통과 입력 섹션을 참조하세요.

   또한 SymbolIcon 요소 및 코드 숨김 파일 (SelectIcon)에 정의된 필드로 바인딩하는 {x: Bind} 태그 확장을 사용하여 단추에 아이콘도 지정했습니다.

   다음 코드 조각에는 클릭 이벤트 처리기와 SelectIcon 정의가 둘 다 포함됩니다.

```csharp
namespace Ink_Basic_InkToolbar
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage_AddCustomTool : Page
    {
        // Icon for custom selection tool button.
        Symbol SelectIcon = (Symbol)0xEF20;

        // Stroke selection tool.
        private Polyline lasso;
        // Stroke selection area.
        private Rect boundingRect;

        public MainPage_AddCustomTool()
        {
            this.InitializeComponent();

            // Listen for new ink or erase strokes to clean up selection UI.
            inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
                StrokeInput_StrokeStarted;
            inkCanvas.InkPresenter.StrokesErased +=
                InkPresenter_StrokesErased;
        }

        private void customToolButton_Click(object sender, RoutedEventArgs e)
        {
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
        }

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

        private void cutButton_Click(object sender, RoutedEventArgs e)
        {
            inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
            inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            ClearSelection();
        }

        private void copyButton_Click(object sender, RoutedEventArgs e)
        {
            inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
        }

        private void pasteButton_Click(object sender, RoutedEventArgs e)
        {
            if (inkCanvas.InkPresenter.StrokeContainer.CanPasteFromClipboard())
            {
                inkCanvas.InkPresenter.StrokeContainer.PasteFromClipboard(
                    new Point(0, 0));
            }
            else
            {
                // Cannot paste from clipboard.
            }
        }

        // Clean up selection UI.
        private void ClearSelection()
        {
            var strokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
            foreach (var stroke in strokes)
            {
                stroke.Selected = false;
            }
            ClearBoundingRect();
        }

        private void ClearBoundingRect()
        {
            if (selectionCanvas.Children.Any())
            {
                selectionCanvas.Children.Clear();
                boundingRect = Rect.Empty;
            }
        }

        // Handle unprocessed pointer events from modifed input.
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
    }
}
```



### 사용자 지정 잉크 렌더링

기본적으로 잉크 입력은 짧은 대기 시간의 백그라운드 스레드에서 처리되고 그릴 때 "젖은" 상태로 렌더링됩니다. 스트로크가 완료되면(펜 또는 손가락을 들거나 마우스 단추를 뗄 때) 스트로크는 UI 스레드에서 처리되고 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 계층(응용 프로그램 콘텐츠 위 계층으로, 젖은 잉크를 대체함)에 대해 "건조" 상태로 렌더링됩니다.

잉크 플랫폼을 사용하여 이 동작을 재정의하고 잉크 입력의 사용자 지정 건조를 수행하여 수동 입력 환경을 완전히 사용자 지정할 수 있습니다.

사용자 지정 건조 상태에 대한 자세한 내용은 [UWP 앱에서 펜 조작 및 Windows Ink](https://msdn.microsoft.com/en-us/windows/uwp/input-and-devices/pen-and-stylus-interactions#custom-ink-rendering)을 참조하세요.

> [!NOTE]
> 사용자 지정 건조 및 [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)  
> 앱이 사용자 지정 건조를 구현하여 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011)의 기본 잉크 렌더링 동작을 재정의하는 경우 렌더링된 잉크 스트로크를 InkToolbar에서 더 이상 사용할 수 없고 InkToolbar의 기본 제공 지우기 명령이 예상대로 작동하지 않습니다. 지우기 기능을 제공하려면 모든 포인터 이벤트를 처리하고, 각 스트로크에 대해 적중 횟수 테스트를 수행하고, 기본 제공 "모든 잉크 지우기" 명령을 재정의해야 합니다.

## 관련 문서

* [펜 및 스타일러스 조작](pen-and-stylus-interactions.md)

**샘플**
* [잉크 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [간단한 잉크 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [복잡한 잉크 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620314)



<!--HONumber=Nov16_HO1-->


