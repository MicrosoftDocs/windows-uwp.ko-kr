---
Description: Windows 앱의 키보드, 마우스, 터치, 펜 및 게임 패드와 같은 장치에서 입력을 시뮬레이션 하 고 자동화 합니다.
title: 입력 삽입을 통해 사용자 입력 시뮬레이트
label: Input injection
template: detail.hbs
keywords: 장치, 디지타이저, 입력, 상호 작용, 주입
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e4e1497ea30400c550cb0cbb2309801ff8145fd6
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219686"
---
# <a name="simulate-user-input-through-input-injection"></a>입력 삽입을 통해 사용자 입력 시뮬레이트

Windows 응용 프로그램의 키보드, 마우스, 터치, 펜 및 게임 패드와 같은 장치에서 사용자 입력을 시뮬레이션 하 고 자동화 합니다.

> **중요 한 api**: Windows. ui&gt. [ **삽입**](/uwp/api/windows.ui.input.preview.injection)

## <a name="overview"></a>개요

입력 주입을 통해 Windows 응용 프로그램은 다양 한 입력 장치에서 입력을 시뮬레이션 하 고 앱의 클라이언트 영역 외부 (레지스트리 편집기와 같은 관리자 권한으로 실행 되는 앱에도)를 비롯 한 모든 위치에서 해당 입력을 보낼 수 있습니다.

입력 주입은 내게 필요한 옵션, 테스트 (임시, 자동), 원격 액세스 및 지원 기능을 포함 하는 기능을 제공 해야 하는 Windows 앱 및 도구에 유용 합니다.

## <a name="setup"></a>설치

Windows 앱에서 입력 주입 Api를 사용 하려면 앱 매니페스트에 다음을 추가 해야 합니다.

1. **Appxmanifest.xml** 파일을 마우스 오른쪽 단추로 클릭 하 고 **코드 보기**를 선택 합니다.
1. 노드에 다음을 삽입 합니다 `Package` .
    - `xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"`
    - `IgnorableNamespaces="rescap"`
1. 노드에 다음을 삽입 합니다 `Capabilities` .
    - `<rescap:Capability Name="inputInjectionBrokered" />`

## <a name="duplicate-user-input"></a>중복 사용자 입력

| ![터치식 입력 주입 샘플](images/injection/touch-input-injection.gif) | 
|:--:|
| *터치식 입력 주입 샘플* |

이 예제에서는 입력 주입 Api ([Windows](/uwp/api/windows.ui.input.preview.injection))를 사용 하 여 앱의 한 지역에서 마우스 입력 이벤트를 수신 대기 하 고 다른 지역에서 해당 터치식 입력 이벤트를 시뮬레이트하는 방법을 보여 줍니다.

**[입력 주입 샘플 (마우스 터치)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-input-injection-mouse-to-touch.zip) 에서이 샘플 다운로드**

1. 먼저 UI (MainPage)를 설정 합니다.

    두 개의 그리드 영역 (마우스 입력 용으로 하나 및 삽입 된 터치식 입력 용)이 있고 각각 네 개의 단추가 있습니다.
      > [!NOTE] 
      > 그리드 배경에는 값 ( `Transparent` 이 경우)을 할당 해야 합니다. 그렇지 않으면 포인터 이벤트가 검색 되지 않습니다.

    입력 영역에서 마우스 클릭이 검색 되 면 해당 터치 이벤트가 입력 주입 영역에 삽입 됩니다. 삽입 입력의 단추 클릭이 제목 영역에 보고 됩니다.

    ```xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel Grid.Row="0"
                    Margin="10">
            <TextBlock Style="{ThemeResource TitleTextBlockStyle}" 
                       Name="titleText"
                       Text="Touch input injection"
                       HorizontalTextAlignment="Center" />
            <TextBlock Style="{ThemeResource BodyTextBlockStyle}"
                       Name="statusText"
                       HorizontalTextAlignment="Center" />
        </StackPanel>
        <Grid HorizontalAlignment="Center"
                        Grid.Row="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Column="0" 
                       Grid.Row="0" 
                       Style="{ThemeResource CaptionTextBlockStyle}"
                       Text="User mouse input area"/>
            <!-- Background must be set to something, otherwise pointer events are not detected. -->
            <Grid Name="ContainerInput" 
                  Grid.Column="0" 
                  Grid.Row="1"
                  HorizontalAlignment="Stretch" 
                  Background="Transparent" 
                  BorderBrush="Green" 
                  BorderThickness="2" 
                  MinHeight="100" MinWidth="300" 
                  Margin="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Button Name="B1" 
                        Grid.Column="0" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50"
                        Content="B1" />
                <Button Name="B2" 
                        Grid.Column="1" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50"
                        Content="B2" />
                <Button Name="B3" 
                        Grid.Column="2" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50"
                        Content="B3" />
                <Button Name="B4" 
                        Grid.Column="3" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50"
                        Content="B4" />
            </Grid>
            <TextBlock Grid.Column="1" 
                       Grid.Row="0"                         
                       Style="{ThemeResource CaptionTextBlockStyle}"
                       Text="Injected touch input area"/>
            <Grid Name="ContainerInject"
                  Grid.Column="1"  
                  Grid.Row="1"
                  HorizontalAlignment="Stretch"
                  BorderBrush="Red" 
                  BorderThickness="2" 
                  MinHeight="100" MinWidth="300" 
                  Margin="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Button Name="B1i" Click="Button_Click_Injected"
                        Content="B1i"
                        Grid.Column="0" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50" />
                <Button Name="B2i" Click="Button_Click_Injected"
                        Content="B2i"
                        Grid.Column="1" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50" />
                <Button Name="B3i" Click="Button_Click_Injected"
                        Content="B3i"
                        Grid.Column="2" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50" />
                <Button Name="B4i" Click="Button_Click_Injected"
                        Content="B4i"
                        Grid.Column="3" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50" />
            </Grid>
        </Grid>
    </Grid>
    ```

2. 다음으로 앱을 초기화 합니다.
    
    이 코드 조각에서는 전역 개체를 선언 하 고 마우스 입력 영역 내에서 단추 클릭 이벤트에 처리 된 것으로 표시 될 수 있는 포인터 이벤트 ([AddHandler](/uwp/api/windows.ui.xaml.uielement.addhandler))의 수신기를 선언 합니다.

    [InputInjector](/uwp/api/windows.ui.input.preview.injection.inputinjector) 개체는 입력 데이터를 전송 하기 위한 가상 입력 장치를 나타냅니다.

    처리기에서 `ContainerInput_PointerPressed` 터치 삽입 함수를 호출 합니다.

    처리기에서 `ContainerInput_PointerReleased` UninitializeTouchInjection을 호출 하 여 [InputInjector](/uwp/api/windows.ui.input.preview.injection.inputinjector) 개체를 종료 합니다.

    ```csharp
    public sealed partial class MainPage : Page
    {
        /// <summary>
        /// The virtual input device.
        /// </summary>
        InputInjector _inputInjector;

        /// <summary>
        /// Initialize the app, set the window size, 
        /// and add pointer input handlers for the container.
        /// </summary>
        public MainPage()
        {
            this.InitializeComponent();

            ApplicationView.PreferredLaunchViewSize =
                new Size(600, 200);
            ApplicationView.PreferredLaunchWindowingMode =
                ApplicationViewWindowingMode.PreferredLaunchViewSize;

            // Button handles PointerPressed/PointerReleased in 
            // the Tapped routed event, but we need the container Grid 
            // to handle them also. Add a handler for both 
            // PointerPressedEvent and PointerReleasedEvent on the input Grid 
            // and set handledEventsToo to true.
            ContainerInput.AddHandler(PointerPressedEvent,
                new PointerEventHandler(ContainerInput_PointerPressed), true);
            ContainerInput.AddHandler(PointerReleasedEvent,
                new PointerEventHandler(ContainerInput_PointerReleased), true);
        }

        /// <summary>
        /// PointerReleased handler for all pointer conclusion events.
        /// PointerPressed and PointerReleased events do not always occur 
        /// in pairs, so your app should listen for and handle any event that 
        /// might conclude a pointer down (such as PointerExited, PointerCanceled, 
        /// and PointerCaptureLost).  
        /// </summary>
        /// <param name="sender">Source of the click event</param>
        /// <param name="e">Event args for the button click routed event</param>
        private void ContainerInput_PointerReleased(
            object sender, PointerRoutedEventArgs e)
        {
            // Prevent most handlers along the event route from handling event again.
            e.Handled = true;

            // Shut down the virtual input device.
            _inputInjector.UninitializeTouchInjection();
        }

        /// <summary>
        /// PointerPressed handler.
        /// PointerPressed and PointerReleased events do not always occur 
        /// in pairs. Your app should listen for and handle any event that 
        /// might conclude a pointer down (such as PointerExited, 
        /// PointerCanceled, and PointerCaptureLost).  
        /// </summary>
        /// <param name="sender">Source of the click event</param>
        /// <param name="e">Event args for the button click routed event</param>
        private void ContainerInput_PointerPressed(
            object sender, PointerRoutedEventArgs e)
        {
            // Prevent most handlers along the event route from 
            // handling the same event again.
            e.Handled = true;

            InjectTouchForMouse(e.GetCurrentPoint(ContainerInput));

        }
        ...
    }
    ```
3. 터치 입력 삽입 함수는 다음과 같습니다.

    먼저, [InputInjector](/uwp/api/windows.ui.input.preview.injection.inputinjector) 개체를 인스턴스화하기 위해 [trcreate](/uwp/api/windows.ui.input.preview.injection.inputinjector.trycreate) 를 호출 합니다.

    그런 다음 [InjectedInputVisualizationMode](/uwp/api/windows.ui.input.preview.injection.injectedinputvisualizationmode) 를 사용 하 여 [InitializeTouchInjection](/uwp/api/windows.ui.input.preview.injection.inputinjector.initializetouchinjection) 를 호출 `Default` 합니다.

    삽입 지점을 계산한 후에는 [InjectedInputTouchInfo](/uwp/api/windows.ui.input.preview.injection.injectedinputtouchinfo) 를 호출 하 여 삽입할 터치 포인트 목록을 초기화 합니다 .이 예제에서는 마우스 입력 포인터에 해당 하는 터치 지점을 하나 만듭니다.

    마지막으로 [InjectTouchInput](/uwp/api/windows.ui.input.preview.injection.inputinjector.injecttouchinput) 를 두 번 호출 합니다. 첫 번째는 포인터 아래로, 두 번째는 포인터 위입니다.

    ```csharp
    /// <summary>
    /// Inject touch input on injection target corresponding 
    /// to mouse click on input target.
    /// </summary>
    /// <param name="pointerPoint">The mouse click pointer.</param>
    private void InjectTouchForMouse(PointerPoint pointerPoint)
    {
        // Create the touch injection object.
        _inputInjector = InputInjector.TryCreate();

        if (_inputInjector != null)
        {
            _inputInjector.InitializeTouchInjection(
                InjectedInputVisualizationMode.Default);

            // Create a unique pointer ID for the injected touch pointer.
            // Multiple input pointers would require more robust handling.
            uint pointerId = pointerPoint.PointerId + 1;

            // Get the bounding rectangle of the app window.
            Rect appBounds =
                Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().VisibleBounds;

            // Get the top left screen coordinates of the app window rect.
            Point appBoundsTopLeft = new Point(appBounds.Left, appBounds.Top);

            // Get a reference to the input injection area.
            GeneralTransform injectArea =
                ContainerInject.TransformToVisual(Window.Current.Content);

            // Get the top left screen coordinates of the input injection area.
            Point injectAreaTopLeft = injectArea.TransformPoint(new Point(0, 0));

            // Get the screen coordinates (relative to the input area) 
            // of the input pointer.
            int pointerPointX = (int)pointerPoint.Position.X;
            int pointerPointY = (int)pointerPoint.Position.Y;

            // Create the point for input injection and calculate its screen location.
            Point injectionPoint =
                new Point(
                    appBoundsTopLeft.X + injectAreaTopLeft.X + pointerPointX,
                    appBoundsTopLeft.Y + injectAreaTopLeft.Y + pointerPointY);

            // Create a touch data point for pointer down.
            // Each element in the touch data list represents a single touch contact. 
            // For this example, we're mirroring a single mouse pointer.
            List<InjectedInputTouchInfo> touchData =
                new List<InjectedInputTouchInfo>
                {
                    new InjectedInputTouchInfo
                    {
                        Contact = new InjectedInputRectangle
                        {
                            Left = 30, Top = 30, Bottom = 30, Right = 30
                        },
                        PointerInfo = new InjectedInputPointerInfo
                        {
                            PointerId = pointerId,
                            PointerOptions =
                            InjectedInputPointerOptions.PointerDown |
                            InjectedInputPointerOptions.InContact |
                            InjectedInputPointerOptions.New,
                            TimeOffsetInMilliseconds = 0,
                            PixelLocation = new InjectedInputPoint
                            {
                                PositionX = (int)injectionPoint.X ,
                                PositionY = (int)injectionPoint.Y
                            }
                    },
                    Pressure = 1.0,
                    TouchParameters =
                        InjectedInputTouchParameters.Pressure |
                        InjectedInputTouchParameters.Contact
                }
            };

            // Inject the touch input. 
            _inputInjector.InjectTouchInput(touchData);

            // Create a touch data point for pointer up.
            touchData = new List<InjectedInputTouchInfo>
            {
                new InjectedInputTouchInfo
                {
                    PointerInfo = new InjectedInputPointerInfo
                    {
                        PointerId = pointerId,
                        PointerOptions = InjectedInputPointerOptions.PointerUp
                    }
                }
            };

            // Inject the touch input. 
            _inputInjector.InjectTouchInput(touchData);
        }
    }
    ```

4. 마지막으로 입력 주입 영역에서 모든 단추 [클릭](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase) 라우트된 이벤트를 처리 하 고 클릭 한 단추 이름으로 UI를 업데이트 합니다.

## <a name="see-also"></a>참고 항목

### <a name="topic-samples"></a>토픽 샘플

- [입력 삽입 샘플 (마우스로 터치)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-input-injection-mouse-to-touch.zip)
