---
title: 응시 상호 작용
Description: Learn how to design and optimize your UWP apps to provide the best experience possible for users who rely on gaze input from eye and head trackers.
label: Gaze interactions
template: detail.hbs
keywords: 응시, 눈 추적, 머리 추적, 응시 지점, 입력, 사용자 상호 작용, 접근성, 유용성
ms.date: 05/01/2018
ms.topic: article
pm-contact: Jake Cohen
dev-contact: Austin Hodges
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5dc12e9b85b7274c9e1deb7d629917cbeaa981c0
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8758593"
---
# <a name="gaze-interactions-and-eye-tracking-in-uwp-apps"></a>UWP 앱의 응시 상호 작용 및 눈 추적

![눈 추적 영웅](images/gaze/eyecontrolbanner1.png)

눈의 위치와 움직임을 기반으로 사용자의 응시, 관심 및 현재 상태 추적을 지원합니다.

> [!NOTE]
> [Windows Mixed Reality](https://docs.microsoft.com/windows/mixed-reality/)의 응시 입력은 [응시](https://docs.microsoft.com/windows/mixed-reality/gaze)를 참조하세요.

**중요 API**: [Windows.Devices.Input.Preview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview), [GazeDevicePreview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicepreview), [GazePointPreview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazepointpreview), [GazeInputSourcePreview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview)

## <a name="overview"></a>개요

응시 입력은 특히 ALS와 같이 신경 근육 질병 및 근육 및 신경 기능이 손상된 기타 장애가 있는 사용자를 위한 보조 기술로 유용한 Windows 및 UWP 응용 프로그램을 사용하고 이와 상호 작용하는 강력한 방법입니다.

또한 응시 입력은 게임(대상 획득 및 추적 등) 및 기존 입력 장치(키보드, 마우스, 터치)를 사용할 수 없거나 쇼핑 백을 들어야 한다든지 하는 다른 작업을 위해 사용자의 손을 자유롭게 하는 것이 좋은 기존 생산성 응용 프로그램, 키오스크 및 기타 대화형 시나리오에 똑같이 매력적인 기회를 제공합니다.

> [!NOTE]
> **Windows 10 Fall Creators Update**에서 눈을 사용하여 화면 포인터를 제어하고, 화상 키보드로 입력하고, 텍스트 음성 변환을 사용하여 다른 사람과 소통하는 기본 제공 기능인 [아이 컨트롤](https://support.microsoft.com/en-us/help/4043921/windows-10-get-started-eye-control)과 함께 추적 하드웨어에 대한 지원이 도입되었습니다. 눈 추적 하드웨어와 상호 작용할 수 있는 응용 프로그램을 빌드하는 [UWP API]([Windows.Devices.Input.Preview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview)) 집합은 **Windows 10 2018년 4월 업데이트(버전 1803, 빌드 17134)** 이상에서 사용할 수 있습니다.

## <a name="privacy"></a>개인 정보

눈 추적에 의해 수집된 개인 데이터는 민감한 정보일 수 있기 때문에 UWP 응용 프로그램의 앱 매니페스트에 `gazeInput` 접근 권한 값을 선언해야 합니다(다음 **설정** 섹션 참조). 접근 권한 값을 선언하면 Windows는 자동으로 사용자에게 대화에 동의하라는 메시지를 표시합니다(앱을 처음 실행할 때). 앱이 추적 장치와 상호 작용하고 이 데이터에 액세스하려면 사용자는 권한을 부여해야 합니다.

또한, 앱이 눈 추적 데이터를 수집, 저장 또는 전송하는 경우 앱의 개인정보처리방침에서 이를 설명해야 하며 [앱 개발자 계약](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) 및 [Microsoft Store 정책](https://docs.microsoft.com/legal/windows/agreements/store-policies)의 **개인 정보**의 모든 기타 요구 사항을 따라야 합니다.

## <a name="setup"></a>설정

UWP 앱에서 응시 API를 사용하려면 다음을 수행해야 합니다. 

- 앱의 매니페스트에서 `gazeInput` 접근 권한 값을 지정합니다.

    Visual Studio 매니페스트 디자이너에서 **Package.appxmanifest** 파일을 열거나 **코드 보기**를 선택하고 다음 `DeviceCapability`를 `Capabilities` 노드에 삽입하여 수동으로 접근 권한 값을 추가합니다.

    ```xaml
    <Capabilities>
       <DeviceCapability Name="gazeInput" />
    </Capabilities>
    ```

- 시스템에 연결되어 켜져 있는 Windows와 호환되는 눈 추적 디바이스(내장형 또는 주변 장치).

    지원 되는 눈 추적 장치 목록은 [Windows 10의 아이 컨트롤 시작](https://support.microsoft.com/help/4043921/windows-10-get-started-eye-control#supported-devices )을 참조하세요.

## <a name="basic-eye-tracking"></a>기본 눈 추적

이 예에서 UWP 응용 프로그램 내에서 사용자의 응시를 추적하고 기본 적중 횟수 테스트를 통해 타이밍 기능을 사용하여 특정 요소에서 응시 초점을 얼마나 잘 유지할 수 있는지 나타내는 방법을 보여 줍니다.

응용 프로그램 뷰포트 내 응시 지점을 표시하는 데 작은 타원이 사용되며 [Windows 커뮤니티 도구 키트](https://docs.microsoft.com/en-us/windows/uwpcommunitytoolkit/)의 [RadialProgressBar](https://docs.microsoft.com/en-us/windows/uwpcommunitytoolkit/controls/radialprogressbar)는 캔버스에 임의로 배치됩니다. 응시 초점이 진행률 표시줄에 표시되면 타이머가 시작되고 진행률 표시줄이 100%에 도달하면 진행률 표시줄이 캔버스에 임의로 재배치됩니다.

![타이머 샘플로 응시 추적](images/gaze/gaze-input-timed2.gif)

*타이머 샘플로 응시 추적*

**[응시 입력 샘플(기본)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-gazeinput-basic.zip)에서 이 샘플 다운로드**

1. 먼저 UI(MainPage.xaml)를 설정합니다.

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

2. 그 다음, 앱을 초기화합니다.

    이 조각에서 전역 개체를 선언하고 [응시 장치 감시자](https://docs.microsoft.com/en-us/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview)를 시작하기 위해 [OnNavigatedTo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) 페이지 이벤트를, 그리고 [응시 장치 감시자](https://docs.microsoft.com/en-us/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview)를 중지하기 위해 [OnNavigatedFrom](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom) 페이지 이벤트를 재정의합니다.

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

3. 다음으로 응시 장치 감시자 메서드를 추가합니다. 
    
    `StartGazeDeviceWatcher`에서 [CreateWatcher](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.createwatcher)를 호출하고 눈 추적 장치 이벤트 수신기([DeviceAdded](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.added), [DeviceUpdated](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.updated), [DeviceRemoved](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.removed))를 선언합니다.

    `DeviceAdded`에서 눈 추적 디바이스의 상태를 확인합니다. 장치를 사용 가능한 경우 디바이스 수를 늘리고 응시 추적을 활성화합니다. 자세한 내용은 다음 단계를 참조하십시오.

    `DeviceUpdated`에서 장치를 재보정하는 경우 이 이벤트가 트리거되므로 응시 추적도 활성화합니다.

    `DeviceRemoved`에서, 장치 카운터를 줄이고 디바이스 이벤트 처리기를 제거합니다.

    `StopGazeDeviceWatcher`에서 응시 장치 감시자를 종료합니다. 

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

4. 여기에서 장치가 `IsSupportedDevice`에서 사용 가능한지 확인합니다. 그렇다면 `TryEnableGazeTrackingAsync`에서 응시 추적 활성화를 시도합니다.

    `TryEnableGazeTrackingAsync`에서, 응시 이벤트 처리기를 선언하고 [GazeInputSourcePreview.GetForCurrentView()](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.getforcurrentview)를 호출하여 입력 소스에 대한 참조를 가져옵니다(UI 스레드에서 호출해야 함. [UI 스레드 응답 유지](https://docs.microsoft.com/windows/uwp/debug-test-perf/keep-the-ui-thread-responsive) 참조).

    > [!NOTE]
    > 호환되는 눈 추적 디바이스가 연결되어 있고 응용 프로그램에서 요구할 때만 [GazeInputSourcePreview.GetForCurrentView()](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.getforcurrentview)를 호출해야 합니다. 그렇지 않으면 동의 대화는 필요 없습니다.

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

5. 그런 다음, 응시 이벤트 처리기를 설정합니다.

    각각 `GazeEntered` 및 `GazeExited`에서 응시 추적 타원을 표시 및 숨깁니다.

    `GazeMoved`에서 [GazeEnteredPreviewEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeenteredprevieweventargs)의 [CurrentPoint](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeenteredprevieweventargs.currentpoint)에서 제공되는 [EyeGazePosition](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazepointpreview.eyegazeposition)을 기반으로 응시 추적 타원을 이동합니다. 또한 진행률 표시줄의 위치 변경을 트리거하는 [RadialProgressBar](https://docs.microsoft.com/en-us/windows/uwpcommunitytoolkit/controls/radialprogressbar)에서 응시 초점 타이머를 관리합니다. 자세한 내용은 다음 단계를 참조하십시오.

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
6. 마지막으로, 다음은 이 앱에 대한 응시 초점 타이머를 관리하는 데 사용하는 방법입니다.

    `DoesElementContainPoint` 응시 포인터가 진행률 표시줄 위에 있는지 확인합니다. 그렇다면 응시 타이머가 시작되고 각 응시 타이머 눈금에서 진행률 표시줄이 증가합니다.

    `SetGazeTargetLocation` 진행률 표시줄의 초기 위치를 설정하고, (응시 초점 타이머에 따라) 진행률 표시줄이 완료되면 진행률 표시줄을 임의의 위치로 이동합니다.

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

## <a name="see-also"></a>기타 참조

### <a name="resources"></a>리소스

- [Windows 커뮤니티 도구 키트 응시 라이브러리](https://docs.microsoft.com/en-us/windows/uwpcommunitytoolkit/gaze/gazeinteractionlibrary)

### <a name="topic-samples"></a>항목 샘플

- [응시 샘플(기본)(C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-gazeinput-basic.zip)
