---
title: 응시 조작
Description: Windows 앱을 디자인 하 고 최적화 하 여 눈동자 및 헤드 추적기의 입력을 사용 하는 사용자에 게 가능한 최상의 환경을 제공 하는 방법을 알아봅니다.
label: Gaze interactions
template: detail.hbs
keywords: 응시, 눈 추적, 헤드 추적, 응시 지점, 입력, 사용자 조작, 접근성, 유용성
ms.date: 05/01/2018
ms.topic: article
pm-contact: Jake Cohen
dev-contact: Austin Hodges
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4cfd84d54ecd1425b3b7e66c54c96fbd78c2dd46
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970128"
---
# <a name="gaze-interactions-and-eye-tracking-in-windows-apps"></a>Windows 앱에서 상호 작용 및 눈 추적 응시

![눈 추적 주인공](images/gaze/eyecontrolbanner1.png)

눈의 위치와 움직임을 기반으로 사용자의 응시, 주의 및 현재 상태를 추적 하기 위한 지원을 제공 합니다.

> [!NOTE]
> [Windows Mixed Reality](https://docs.microsoft.com/windows/mixed-reality/)에서의 응시 입력은 [응시](https://docs.microsoft.com/windows/mixed-reality/gaze)를 참조 하세요.

**중요 한 api**: [GazeDevicePreview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicepreview), [GazePointPreview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazepointpreview) [,](https://docs.microsoft.com/uwp/api/windows.devices.input.preview) [GazeInputSourcePreview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview)

## <a name="overview"></a>개요

응시 입력은 neuro-muscular 질병 (예: ALS) 및 장애가 있는 muscle 또는 nerve 함수와 관련 된 다른 장애가 있는 사용자를 위한 보조 기술로 특히 유용한 Windows 응용 프로그램을 조작 하 고 사용 하는 강력한 방법입니다.

또한, 응시 입력은 게임 (대상 취득 및 추적 포함), 기존 생산성 응용 프로그램, 키오스크 및 기존 입력 장치 (키보드, 마우스, 터치)를 사용할 수 없는 기타 대화형 시나리오에 대해 동일 하 게 뛰어난 기회를 제공 하 고, 다른 작업 (예: 시장 가방 보유)을 위해 사용자의 손을 확보 하는 데 유용 하거나 유용한 기타 대화형 시나리오를 제공 합니다.

> [!NOTE]
> 아이 추적 하드웨어 지원은 눈동자를 사용 하 여 화면에 있는 포인터를 제어 하 고, 화상 키보드를 사용 하 여 입력 하 고, 텍스트를 음성으로 변환 하 여 사용자와 통신할 수 있도록 하는 기본 제공 기능인 [아이 컨트롤과](https://support.microsoft.com/help/4043921/windows-10-get-started-eye-control)함께 Windows 10으로 구성 된 **작성자 업데이트** 에 도입 되었습니다. 아이 추적 하드웨어와 상호 작용할 수 있는 응용 프로그램을 빌드하기 위한 Windows 런타임 Api (2018) 집합은 **Windows 10 4 월 업데이트 (버전 1803, 빌드 17134)** 이상에서 사용할 수[있습니다.](https://docs.microsoft.com/uwp/api/windows.devices.input.preview)

## <a name="privacy"></a>개인 정보 취급 방침

눈 추적 장치에서 수집 하는 잠재적으로 중요 한 개인 데이터로 인해 `gazeInput` 응용 프로그램의 응용 프로그램 매니페스트에 기능을 선언 해야 합니다 (다음 **설정** 섹션 참조). 선언 된 경우 Windows는 응용 프로그램을 처음 실행할 때 동의 대화 상자를 자동으로 표시 합니다. 사용자는 앱이 눈 추적 장치와 통신 하 고이 데이터에 액세스할 수 있는 권한을 부여 해야 합니다.

또한 앱이 눈 추적 데이터를 수집, 저장 또는 전송 하는 경우 앱의 개인 정보 취급 방침에서이를 설명 하 고 [앱 개발자 계약](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) 및 [Microsoft Store 정책의](https://docs.microsoft.com/legal/windows/agreements/store-policies) **개인 정보** 에 대 한 기타 모든 요구 사항을 준수 해야 합니다.

## <a name="setup"></a>설치 프로그램

Windows 앱에서 응시 입력 Api를 사용 하려면 다음을 수행 해야 합니다. 

- `gazeInput`앱 매니페스트에서 기능을 지정 합니다.

    Visual Studio 매니페스트 디자이너를 사용 하 여 **appxmanifest.xml** 파일을 열거나, **코드 보기**를 선택 하 여 기능을 수동으로 추가 하 고, 다음을 노드에 삽입 합니다 `DeviceCapability` `Capabilities` .

    ```xaml
    <Capabilities>
       <DeviceCapability Name="gazeInput" />
    </Capabilities>
    ```

- 시스템 (기본 제공 또는 주변 장치)에 연결 되 고 설정 된 Windows 호환 되는 눈 추적 장치입니다.

    지원 되는 눈 추적 장치 목록은 [Windows 10의 눈동자 컨트롤 시작](https://support.microsoft.com/help/4043921/windows-10-get-started-eye-control#supported-devices ) 을 참조 하세요.

## <a name="basic-eye-tracking"></a>기본 눈 추적

이 예제에서는 Windows 앱 내에서 사용자의 응시를 추적 하 고 기본적인 적중 테스트와 함께 타이밍 함수를 사용 하 여 특정 요소에 대 한 사용자의 응시 포커스를 유지 관리할 수 있는지 여부를 나타내는 방법을 보여 줍니다.

작은 타원은 응시 지점이 응용 프로그램 뷰포트 내에 있는 위치를 표시 하는 데 사용 되 고, [Windows 커뮤니티 도구 키트](https://docs.microsoft.com/windows/communitytoolkit/) 의 [RadialProgressBar](https://docs.microsoft.com/windows/communitytoolkit/controls/radialprogressbar) 는 캔버스에 임의로 배치 됩니다. 진행률 표시줄에서 포커스 포커스가 검색 되 면 타이머가 시작 되 고 진행률 표시줄이 100%에 도달 하면 캔버스에서 진행률 표시줄이 무작위로 재배치 됩니다.

![Timer로 추적 응시 샘플](images/gaze/gaze-input-timed2.gif)

*Timer로 추적 응시 샘플*

**이 샘플은 [응시 입력 샘플 (기본)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-gazeinput-basic.zip) 에서 다운로드 합니다.**

1. 먼저 UI (MainPage)를 설정 합니다.

    ```xaml
    <Page
        x:Class="gazeinput.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:gazeinput"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:controls="using:Microsoft.Toolkit.Uwp.UI.Controls"    
        mc:Ignorable="d">
    
        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <Grid x:Name="containerGrid">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="*"/>
                </Grid.RowDefinitions>
                <StackPanel x:Name="HeaderPanel" 
                        Orientation="Horizontal" 
                        Grid.Row="0">
                    <StackPanel.Transitions>
                        <TransitionCollection>
                            <AddDeleteThemeTransition/>
                        </TransitionCollection>
                    </StackPanel.Transitions>
                    <TextBlock x:Name="Header" 
                           Text="Gaze tracking sample" 
                           Style="{ThemeResource HeaderTextBlockStyle}" 
                           Margin="10,0,0,0" />
                    <TextBlock x:Name="TrackerCounterLabel"
                           VerticalAlignment="Center"                 
                           Style="{ThemeResource BodyTextBlockStyle}"
                           Text="Number of trackers: " 
                           Margin="50,0,0,0"/>
                    <TextBlock x:Name="TrackerCounter"
                           VerticalAlignment="Center"                 
                           Style="{ThemeResource BodyTextBlockStyle}"
                           Text="0" 
                           Margin="10,0,0,0"/>
                    <TextBlock x:Name="TrackerStateLabel"
                           VerticalAlignment="Center"                 
                           Style="{ThemeResource BodyTextBlockStyle}"
                           Text="State: " 
                           Margin="50,0,0,0"/>
                    <TextBlock x:Name="TrackerState"
                           VerticalAlignment="Center"                 
                           Style="{ThemeResource BodyTextBlockStyle}"
                           Text="n/a" 
                           Margin="10,0,0,0"/>
                </StackPanel>
                <Canvas x:Name="gazePositionCanvas" Grid.Row="1">
                    <controls:RadialProgressBar
                        x:Name="GazeRadialProgressBar" 
                        Value="0"
                        Foreground="Blue" 
                        Background="White"
                        Thickness="4"
                        Minimum="0"
                        Maximum="100"
                        Width="100"
                        Height="100"
                        Outline="Gray"
                        Visibility="Collapsed"/>
                    <Ellipse 
                        x:Name="eyeGazePositionEllipse"
                        Width="20" Height="20"
                        Fill="Blue" 
                        Opacity="0.5" 
                        Visibility="Collapsed">
                    </Ellipse>
                </Canvas>
            </Grid>
        </Grid>
    </Page>
    ```

2. 다음으로 앱을 초기화 합니다.

    이 코드 조각에서는 전역 개체를 선언 하 고 [OnNavigatedTo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) page 이벤트를 재정의 하 여 [응시 장치 감시자](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview) 를 시작 하 고 [OnNavigatedFrom](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom) page 이벤트를 재정의 하 여 [응시 장치 감시자](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview)를 중지 합니다.

    ```csharp
    using System;
    using Windows.Devices.Input.Preview;
    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml;
    using Windows.Foundation;
    using System.Collections.Generic;
    using Windows.UI.Xaml.Media;
    using Windows.UI.Xaml.Navigation;
    
    namespace gazeinput
    {
        public sealed partial class MainPage : Page
        {
            /// <summary>
            /// Reference to the user's eyes and head as detected
            /// by the eye-tracking device.
            /// </summary>
            private GazeInputSourcePreview gazeInputSource;
    
            /// <summary>
            /// Dynamic store of eye-tracking devices.
            /// </summary>
            /// <remarks>
            /// Receives event notifications when a device is added, removed, 
            /// or updated after the initial enumeration.
            /// </remarks>
            private GazeDeviceWatcherPreview gazeDeviceWatcher;
    
            /// <summary>
            /// Eye-tracking device counter.
            /// </summary>
            private int deviceCounter = 0;
    
            /// <summary>
            /// Timer for gaze focus on RadialProgressBar.
            /// </summary>
            DispatcherTimer timerGaze = new DispatcherTimer();
    
            /// <summary>
            /// Tracker used to prevent gaze timer restarts.
            /// </summary>
            bool timerStarted = false;
    
            /// <summary>
            /// Initialize the app.
            /// </summary>
            public MainPage()
            {
                InitializeComponent();
            }

            /// <summary>
            /// Override of OnNavigatedTo page event starts GazeDeviceWatcher.
            /// </summary>
            /// <param name="e">Event args for the NavigatedTo event</param>
            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
                // Start listening for device events on navigation to eye-tracking page.
                StartGazeDeviceWatcher();
            }
    
            /// <summary>
            /// Override of OnNavigatedFrom page event stops GazeDeviceWatcher.
            /// </summary>
            /// <param name="e">Event args for the NavigatedFrom event</param>
            protected override void OnNavigatedFrom(NavigationEventArgs e)
            {
                // Stop listening for device events on navigation from eye-tracking page.
                StopGazeDeviceWatcher();
            }
        }
    }
    ```

3. 다음으로, 응시 장치 감시자 메서드를 추가 합니다. 
    
    에서는 `StartGazeDeviceWatcher` [createwatcher](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.createwatcher) 를 호출 하 고, 아이 추적 장치 이벤트 수신기 ([deviceadded](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.added), [deviceadded](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.updated)및 [DeviceRemoved](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.removed))를 선언 합니다.

    에서 `DeviceAdded` 눈 추적 장치의 상태를 확인 합니다. 실행 가능한 장치인 경우 장치 수를 증가 시키고 응시 추적을 사용 하도록 설정 합니다. 자세한 내용은 다음 단계를 참조 하세요.

    에서는 `DeviceUpdated` 장치가 recalibrated 때이 이벤트가 트리거되는 경우에도 응시 추적을 사용 하도록 설정 합니다.

    에서는 `DeviceRemoved` 장치 카운터를 감소 하 고 장치 이벤트 처리기를 제거 합니다.

    에서는 `StopGazeDeviceWatcher` 응시 장치 감시자를 종료 합니다. 

```csharp
    /// <summary>
    /// Start gaze watcher and declare watcher event handlers.
    /// </summary>
    private void StartGazeDeviceWatcher()
    {
        if (gazeDeviceWatcher == null)
        {
            gazeDeviceWatcher = GazeInputSourcePreview.CreateWatcher();
            gazeDeviceWatcher.Added += this.DeviceAdded;
            gazeDeviceWatcher.Updated += this.DeviceUpdated;
            gazeDeviceWatcher.Removed += this.DeviceRemoved;
            gazeDeviceWatcher.Start();
        }
    }

    /// <summary>
    /// Shut down gaze watcher and stop listening for events.
    /// </summary>
    private void StopGazeDeviceWatcher()
    {
        if (gazeDeviceWatcher != null)
        {
            gazeDeviceWatcher.Stop();
            gazeDeviceWatcher.Added -= this.DeviceAdded;
            gazeDeviceWatcher.Updated -= this.DeviceUpdated;
            gazeDeviceWatcher.Removed -= this.DeviceRemoved;
            gazeDeviceWatcher = null;
        }
    }

    /// <summary>
    /// Eye-tracking device connected (added, or available when watcher is initialized).
    /// </summary>
    /// <param name="sender">Source of the device added event</param>
    /// <param name="e">Event args for the device added event</param>
    private void DeviceAdded(GazeDeviceWatcherPreview source, 
        GazeDeviceWatcherAddedPreviewEventArgs args)
    {
        if (IsSupportedDevice(args.Device))
        {
            deviceCounter++;
            TrackerCounter.Text = deviceCounter.ToString();
        }
        // Set up gaze tracking.
        TryEnableGazeTrackingAsync(args.Device);
    }

    /// <summary>
    /// Initial device state might be uncalibrated, 
    /// but device was subsequently calibrated.
    /// </summary>
    /// <param name="sender">Source of the device updated event</param>
    /// <param name="e">Event args for the device updated event</param>
    private void DeviceUpdated(GazeDeviceWatcherPreview source,
        GazeDeviceWatcherUpdatedPreviewEventArgs args)
    {
        // Set up gaze tracking.
        TryEnableGazeTrackingAsync(args.Device);
    }

    /// <summary>
    /// Handles disconnection of eye-tracking devices.
    /// </summary>
    /// <param name="sender">Source of the device removed event</param>
    /// <param name="e">Event args for the device removed event</param>
    private void DeviceRemoved(GazeDeviceWatcherPreview source,
        GazeDeviceWatcherRemovedPreviewEventArgs args)
    {
        // Decrement gaze device counter and remove event handlers.
        if (IsSupportedDevice(args.Device))
        {
            deviceCounter--;
            TrackerCounter.Text = deviceCounter.ToString();

            if (deviceCounter == 0)
            {
                gazeInputSource.GazeEntered -= this.GazeEntered;
                gazeInputSource.GazeMoved -= this.GazeMoved;
                gazeInputSource.GazeExited -= this.GazeExited;
            }
        }
    }
```

4. 여기서는 장치가에서 실행 가능한 지 확인 하 `IsSupportedDevice` 고, 그럴 경우에서 응시 추적을 사용 하도록 설정 해 봅니다 `TryEnableGazeTrackingAsync` .

    에서는 `TryEnableGazeTrackingAsync` 응시 이벤트 처리기를 선언 하 고 [GazeInputSourcePreview ()](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.getforcurrentview) 를 호출 하 여 입력 소스에 대 한 참조를 가져옵니다 .이는 ui 스레드에서 호출 해야 합니다. [ui 스레드 응답성 유지](https://docs.microsoft.com/windows/uwp/debug-test-perf/keep-the-ui-thread-responsive)를 참조 하세요.

    > [!NOTE]
    > 응용 프로그램에 호환 되는 눈 추적 장치가 연결 되어 필요한 경우에만 [GazeInputSourcePreview ()](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.getforcurrentview) 를 호출 해야 합니다. 그렇지 않으면 동의 대화 상자가 필요 하지 않습니다.

```csharp
    /// <summary>
    /// Initialize gaze tracking.
    /// </summary>
    /// <param name="gazeDevice"></param>
    private async void TryEnableGazeTrackingAsync(GazeDevicePreview gazeDevice)
    {
        // If eye-tracking device is ready, declare event handlers and start tracking.
        if (IsSupportedDevice(gazeDevice))
        {
            timerGaze.Interval = new TimeSpan(0, 0, 0, 0, 20);
            timerGaze.Tick += TimerGaze_Tick;

            SetGazeTargetLocation();

            // This must be called from the UI thread.
            gazeInputSource = GazeInputSourcePreview.GetForCurrentView();

            gazeInputSource.GazeEntered += GazeEntered;
            gazeInputSource.GazeMoved += GazeMoved;
            gazeInputSource.GazeExited += GazeExited;
        }
        // Notify if device calibration required.
        else if (gazeDevice.ConfigurationState ==
                    GazeDeviceConfigurationStatePreview.UserCalibrationNeeded ||
                    gazeDevice.ConfigurationState ==
                    GazeDeviceConfigurationStatePreview.ScreenSetupNeeded)
        {
            // Device isn't calibrated, so invoke the calibration handler.
            System.Diagnostics.Debug.WriteLine(
                "Your device needs to calibrate. Please wait for it to finish.");
            await gazeDevice.RequestCalibrationAsync();
        }
        // Notify if device calibration underway.
        else if (gazeDevice.ConfigurationState == 
            GazeDeviceConfigurationStatePreview.Configuring)
        {
            // Device is currently undergoing calibration.  
            // A device update is sent when calibration complete.
            System.Diagnostics.Debug.WriteLine(
                "Your device is being configured. Please wait for it to finish"); 
        }
        // Device is not viable.
        else if (gazeDevice.ConfigurationState == GazeDeviceConfigurationStatePreview.Unknown)
        {
            // Notify if device is in unknown state.  
            // Reconfigure/recalbirate the device.  
            System.Diagnostics.Debug.WriteLine(
                "Your device is not ready. Please set up your device or reconfigure it."); 
        }
    }

    /// <summary>
    /// Check if eye-tracking device is viable.
    /// </summary>
    /// <param name="gazeDevice">Reference to eye-tracking device.</param>
    /// <returns>True, if device is viable; otherwise, false.</returns>
    private bool IsSupportedDevice(GazeDevicePreview gazeDevice)
    {
        TrackerState.Text = gazeDevice.ConfigurationState.ToString();
        return (gazeDevice.CanTrackEyes &&
                    gazeDevice.ConfigurationState == 
                    GazeDeviceConfigurationStatePreview.Ready);
    }
```

5. 다음으로, 응시 이벤트 처리기를 설정 합니다.

    및의 응시 추적 타원을 각각 표시 하 고 숨깁니다 `GazeEntered` `GazeExited` .

    에서는 `GazeMoved` [GazeEnteredPreviewEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeenteredprevieweventargs)의 [currentpoint](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeenteredprevieweventargs.currentpoint) 에서 제공 하는 [EyeGazePosition](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazepointpreview.eyegazeposition) 을 기반으로 하 여 응시 추적 타원이 이동 합니다. 또한 진행률 표시줄의 위치 변경을 트리거하는 [RadialProgressBar](https://docs.microsoft.com/windows/communitytoolkit/controls/radialprogressbar)에서 응시 포커스 타이머를 관리 합니다. 자세한 내용은 다음 단계를 참조 하세요.

    ```csharp
    /// <summary>
    /// GazeEntered handler.
    /// </summary>
    /// <param name="sender">Source of the gaze entered event</param>
    /// <param name="e">Event args for the gaze entered event</param>
    private void GazeEntered(
        GazeInputSourcePreview sender, 
        GazeEnteredPreviewEventArgs args)
    {
        // Show ellipse representing gaze point.
        eyeGazePositionEllipse.Visibility = Visibility.Visible;

        // Mark the event handled.
        args.Handled = true;
    }

    /// <summary>
    /// GazeExited handler.
    /// Call DisplayRequest.RequestRelease to conclude the 
    /// RequestActive called in GazeEntered.
    /// </summary>
    /// <param name="sender">Source of the gaze exited event</param>
    /// <param name="e">Event args for the gaze exited event</param>
    private void GazeExited(
        GazeInputSourcePreview sender, 
        GazeExitedPreviewEventArgs args)
    {
        // Hide gaze tracking ellipse.
        eyeGazePositionEllipse.Visibility = Visibility.Collapsed;

        // Mark the event handled.
        args.Handled = true;
    }

    /// <summary>
    /// GazeMoved handler translates the ellipse on the canvas to reflect gaze point.
    /// </summary>
    /// <param name="sender">Source of the gaze moved event</param>
    /// <param name="e">Event args for the gaze moved event</param>
    private void GazeMoved(GazeInputSourcePreview sender, GazeMovedPreviewEventArgs args)
    {
        // Update the position of the ellipse corresponding to gaze point.
        if (args.CurrentPoint.EyeGazePosition != null)
        {
            double gazePointX = args.CurrentPoint.EyeGazePosition.Value.X;
            double gazePointY = args.CurrentPoint.EyeGazePosition.Value.Y;

            double ellipseLeft = 
                gazePointX - 
                (eyeGazePositionEllipse.Width / 2.0f);
            double ellipseTop = 
                gazePointY - 
                (eyeGazePositionEllipse.Height / 2.0f) - 
                (int)Header.ActualHeight;

            // Translate transform for moving gaze ellipse.
            TranslateTransform translateEllipse = new TranslateTransform
            {
                X = ellipseLeft,
                Y = ellipseTop
            };

            eyeGazePositionEllipse.RenderTransform = translateEllipse;

            // The gaze point screen location.
            Point gazePoint = new Point(gazePointX, gazePointY);

            // Basic hit test to determine if gaze point is on progress bar.
            bool hitRadialProgressBar = 
                DoesElementContainPoint(
                    gazePoint, 
                    GazeRadialProgressBar.Name, 
                    GazeRadialProgressBar); 

            // Use progress bar thickness for visual feedback.
            if (hitRadialProgressBar)
            {
                GazeRadialProgressBar.Thickness = 10;
            }
            else
            {
                GazeRadialProgressBar.Thickness = 4;
            }

            // Mark the event handled.
            args.Handled = true;
        }
    }
    ```
6. 마지막으로,이 앱의 응시 포커스 타이머를 관리 하는 데 사용 되는 메서드는 다음과 같습니다.

    `DoesElementContainPoint`응시 포인터가 진행률 표시줄 위에 있는지 확인 합니다. 그럴 경우에는 응시 타이머를 시작 하 고 각 응시 timer tick에서 진행률 표시줄을 증가 시킵니다.

    `SetGazeTargetLocation`진행률 표시줄의 초기 위치를 설정 하 고 진행률 표시줄이 완료 되 면 (응시 포커스 타이머에 따라) 진행률 표시줄을 임의의 위치로 이동 합니다.

    ```csharp
    /// <summary>
    /// Return whether the gaze point is over the progress bar.
    /// </summary>
    /// <param name="gazePoint">The gaze point screen location</param>
    /// <param name="elementName">The progress bar name</param>
    /// <param name="uiElement">The progress bar UI element</param>
    /// <returns></returns>
    private bool DoesElementContainPoint(
        Point gazePoint, string elementName, UIElement uiElement)
    {
        // Use entire visual tree of progress bar.
        IEnumerable<UIElement> elementStack = 
            VisualTreeHelper.FindElementsInHostCoordinates(gazePoint, uiElement, true);
        foreach (UIElement item in elementStack)
        {
            //Cast to FrameworkElement and get element name.
            if (item is FrameworkElement feItem)
            {
                if (feItem.Name.Equals(elementName))
                {
                    if (!timerStarted)
                    {
                        // Start gaze timer if gaze over element.
                        timerGaze.Start();
                        timerStarted = true;
                    }
                    return true;
                }
            }
        }

        // Stop gaze timer and reset progress bar if gaze leaves element.
        timerGaze.Stop();
        GazeRadialProgressBar.Value = 0;
        timerStarted = false;
        return false;
    }

    /// <summary>
    /// Tick handler for gaze focus timer.
    /// </summary>
    /// <param name="sender">Source of the gaze entered event</param>
    /// <param name="e">Event args for the gaze entered event</param>
    private void TimerGaze_Tick(object sender, object e)
    {
        // Increment progress bar.
        GazeRadialProgressBar.Value += 1;

        // If progress bar reaches maximum value, reset and relocate.
        if (GazeRadialProgressBar.Value == 100)
        {
            SetGazeTargetLocation();
        }
    }

    /// <summary>
    /// Set/reset the screen location of the progress bar.
    /// </summary>
    private void SetGazeTargetLocation()
    {
        // Ensure the gaze timer restarts on new progress bar location.
        timerGaze.Stop();
        timerStarted = false;

        // Get the bounding rectangle of the app window.
        Rect appBounds = Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().VisibleBounds;

        // Translate transform for moving progress bar.
        TranslateTransform translateTarget = new TranslateTransform();

        // Calculate random location within gaze canvas.
            Random random = new Random();
            int randomX = 
                random.Next(
                    0, 
                    (int)appBounds.Width - (int)GazeRadialProgressBar.Width);
            int randomY = 
                random.Next(
                    0, 
                    (int)appBounds.Height - (int)GazeRadialProgressBar.Height - (int)Header.ActualHeight);

        translateTarget.X = randomX;
        translateTarget.Y = randomY;

        GazeRadialProgressBar.RenderTransform = translateTarget;

        // Show progress bar.
        GazeRadialProgressBar.Visibility = Visibility.Visible;
        GazeRadialProgressBar.Value = 0;
    }
    ```

## <a name="see-also"></a>참고 항목

### <a name="resources"></a>리소스

- [Windows 커뮤니티 도구 키트 응시 라이브러리](https://docs.microsoft.com/windows/communitytoolkit/gaze/gazeinteractionlibrary)

### <a name="topic-samples"></a>토픽 샘플

- [응시 샘플 (기본) (c #)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-gazeinput-basic.zip)
