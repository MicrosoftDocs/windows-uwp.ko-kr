---
title: 시작 화면을 더 오래 표시
description: 앱에 대 한 확장 된 시작 화면을 만들어 더 많은 시간 동안 시작 화면을 표시 합니다. 이 확장 화면은 앱이 시작 될 때 표시 되는 시작 화면을 모방한 사용자 지정할 수 있습니다.
ms.assetid: CD3053EB-7F86-4D74-9C5A-950303791AE3
ms.date: 02/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8eaede14fad920653e24506298c1d80b7d176a1f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164977"
---
# <a name="display-a-splash-screen-for-more-time"></a>시작 화면을 더 오래 표시

**중요 API**

-   [SplashScreen 클래스](/uwp/api/Windows.ApplicationModel.Activation.SplashScreen)
-   [SizeChanged 이벤트](/uwp/api/windows.ui.xaml.window.sizechanged)
-   [응용 프로그램. OnLaunched 메서드](/uwp/api/windows.ui.xaml.application.onlaunched)

앱에 대 한 확장 된 시작 화면을 만들어 더 많은 시간 동안 시작 화면을 표시 합니다. 이 확장 화면은 앱이 시작 될 때 표시 되는 시작 화면을 모방한 사용자 지정할 수 있습니다. 초기 UI를 준비 하기 위해 실시간 로드 정보를 표시 하거나 앱에 추가 시간을 지정 하는 것이 든, 시작 된 시작 화면을 사용 하 여 시작 환경을 정의할 수 있습니다.

> [!NOTE]
> 이 항목의 "확장 된 시작 화면"은 오랜 시간 동안 화면에 유지 되는 시작 화면을 참조 합니다. [SplashScreen](/uwp/api/Windows.ApplicationModel.Activation.SplashScreen) 클래스에서 파생 되는 하위 클래스를 의미 하는 것은 아닙니다.

다음 권장 사항에 따라 확장 된 시작 화면에서 기본 시작 화면을 정확 하 게 모방한 확인 합니다.

-   확장 된 시작 화면 페이지는 앱 매니페스트에서 시작 화면에 지정 된 이미지 (앱 시작 화면 이미지)와 일치 하는 620 x 300 픽셀 이미지를 사용 해야 합니다. Microsoft Visual Studio 2015에서 시작 화면 설정은 응용 프로그램 매니페스트 (appxmanifest.xml 파일)의 **시각적 자산** 탭의 **시작 화면** 섹션에 저장 됩니다.
-   확장 된 시작 화면에는 앱 매니페스트에서 시작 화면에 지정 된 배경색과 일치 하는 배경색 (앱 시작 화면 배경)을 사용 해야 합니다.
-   코드는 [SplashScreen](/uwp/api/Windows.ApplicationModel.Activation.SplashScreen) 클래스를 사용 하 여 응용 프로그램의 시작 화면 이미지를 기본 시작 화면과 동일한 화면 좌표에 배치 해야 합니다.
-   [SplashScreen](/uwp/api/Windows.ApplicationModel.Activation.SplashScreen) 클래스를 사용 하 여 확장 된 시작 화면에서 항목의 위치를 변경 하 여 사용자 코드에서 창 크기 조정 이벤트 (예: 화면이 회전 되거나 앱이 화면에서 다른 앱 옆으로 이동)에 응답 해야 합니다.

다음 단계를 사용 하 여 기본 시작 화면을 효과적으로 모방한 하는 확장 된 시작 화면을 만듭니다.

## <a name="add-a-blank-page-item-to-your-existing-app"></a>기존 앱에 **빈 페이지** 항목 추가


이 항목에서는 c #, Visual Basic 또는 c + +를 사용 하 여 기존 유니버설 Windows 플랫폼 (UWP) 앱 프로젝트에 확장 된 시작 화면을 추가 하려는 경우를 가정 합니다.

-   Visual Studio에서 앱을 엽니다.
-   메뉴 모음에서 **프로젝트** 를 누르거나 **새 항목 추가**를 클릭 합니다. **새 항목 추가** 대화 상자가 표시 됩니다.
-   이 대화 상자에서 새로운 **빈 페이지** 를 앱에 추가 합니다. 이 항목에서는 확장 시작 화면 페이지의 이름을 "ExtendedSplash"으로 합니다.

**빈 페이지** 항목을 추가하면 두 개의 파일이 생성됩니다. 하나는 태그 파일(ExtendedSplash.xaml)이고 다른 하나는 코드 파일(ExtendedSplash.xaml.cs)입니다.

## <a name="essential-xaml-for-an-extended-splash-screen"></a>확장 시작 화면에 대 한 필수 XAML


다음 단계를 따라 확장 시작 화면에 이미지 및 진행률 컨트롤을 추가 합니다.

ExtendedSplash .xaml 파일에서 다음을 수행 합니다.

-   응용 프로그램 매니페스트에서 앱 시작 화면에 대해 설정한 배경색 (appxmanifest.xml 파일의 **시각적 자산** 섹션)과 일치 하도록 기본 [그리드](/uwp/api/Windows.UI.Xaml.Controls.Grid) 요소의 [배경](/uwp/api/windows.ui.xaml.controls.control.backgroundproperty) 속성을 변경 합니다. 기본 시작 화면 색은 연한 회색 (16 진수 값 \# 464646)입니다. 이 **Grid** 요소는 새 **빈 페이지**를 만들 때 기본적으로 제공 됩니다. **그리드**를 사용할 필요가 없습니다. 확장 된 시작 화면을 빌드하기 위한 편리한 기반입니다.
-   [Canvas](/uwp/api/Windows.UI.Xaml.Controls.Canvas) 요소를 [모눈](/uwp/api/Windows.UI.Xaml.Controls.Grid)에 추가 합니다. 이 **캔버스** 를 사용 하 여 확장 된 시작 화면 이미지를 배치 합니다.
-   [Canvas](/uwp/api/Windows.UI.Xaml.Controls.Canvas)에 [Image](/uwp/api/Windows.UI.Xaml.Controls.Image) 요소를 추가 합니다. 기본 시작 화면에 대해 선택한 확장 시작 화면에 동일한 600 x 320 픽셀 이미지를 사용 합니다.
-   필드 응용 프로그램을 로드 하는 사용자를 표시 하는 진행률 컨트롤을 추가 합니다. 이 항목에서는 활성화 상태의 또는 확정 되지 않은 [ProgressBar](/uwp/api/Windows.UI.Xaml.Controls.ProgressBar)대신 [ProgressRing](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing)를 추가 합니다.

다음 예제에서는 이러한 추가 및 변경 내용이 포함 된 [표](/uwp/api/Windows.UI.Xaml.Controls.Grid) 를 보여 줍니다.

```xaml
    <Grid Background="#464646">
        <Canvas>
            <Image x:Name="extendedSplashImage" Source="Assets/SplashScreen.png"/>
            <ProgressRing Name="splashProgressRing" IsActive="True" Width="20" HorizontalAlignment="Center"></ProgressRing>
        </Canvas>
    </Grid>
```

> [!NOTE]
> 이 예에서는 [ProgressRing](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) 의 너비를 20 픽셀로 설정 합니다. 응용 프로그램에서 작동 하는 값으로 너비를 수동으로 설정할 수 있지만 컨트롤이 20 픽셀 미만이 면 렌더링 되지 않습니다.

## <a name="essential-code-for-an-extended-splash-screen-class"></a>확장 된 시작 화면 클래스의 필수 코드


확장 된 시작 화면은 창 크기 (Windows에만 해당) 또는 방향이 변경 될 때마다 응답 해야 합니다. 사용자가 사용 하는 이미지의 위치를 업데이트 하 여 확장 된 시작 화면이 창이 어떻게 변경 되는지에 관계 없이 제대로 표시 되도록 해야 합니다.

다음 단계를 사용 하 여 확장 된 시작 화면을 올바르게 표시 하는 메서드를 정의 합니다.

1.  **필수 네임 스페이스 추가**

    [SplashScreen](/uwp/api/Windows.ApplicationModel.Activation.SplashScreen) 클래스, [Rect](/uwp/api/windows.foundation.rect) 구조체 및 [SizeChanged](/uwp/api/windows.ui.xaml.window.sizechanged) 이벤트에 액세스 하려면 **ExtendedSplash.xaml.cs** 에 다음 네임 스페이스를 추가 해야 합니다.

    ```cs
    using Windows.ApplicationModel.Activation;
    using Windows.Foundation;
    using Windows.UI.Core;
    ```

2.  **Partial 클래스를 만들고 클래스 변수를 선언 합니다.**

    ExtendedSplash.xaml.cs에 다음 코드를 포함 하 여 확장 된 시작 화면을 나타내는 partial 클래스를 만듭니다.

    ```cs
    partial class ExtendedSplash : Page
    {
        internal Rect splashImageRect; // Rect to store splash screen image coordinates.
        private SplashScreen splash; // Variable to hold the splash screen object.
        internal bool dismissed = false; // Variable to track splash screen dismissal status.
        internal Frame rootFrame;

       // Define methods and constructor
    }
    ```

    이러한 클래스 변수는 여러 메서드에서 사용 됩니다. `splashImageRect`변수는 시스템이 앱에 대 한 시작 화면 이미지를 표시 하는 좌표를 저장 합니다. `splash`변수는 [SplashScreen](/uwp/api/Windows.ApplicationModel.Activation.SplashScreen) 개체를 저장 하 고, `dismissed` 변수는 시스템에서 표시 하는 시작 화면을 해제 했는지 여부를 추적 합니다.

3.  **이미지의 위치를 올바르게 지정 하는 클래스에 대 한 생성자를 정의 합니다.**

    다음 코드에서는 창 크기 조정 이벤트를 수신 하 고, 확장 시작 화면에 이미지 및 (선택 사항) 진행률 컨트롤을 배치 하 고, 탐색을 위한 프레임을 만들고, 저장 된 세션 상태를 복원 하는 비동기 메서드를 호출 하는 확장 시작 화면 클래스에 대 한 생성자를 정의 합니다.

    ```cs
    public ExtendedSplash(SplashScreen splashscreen, bool loadState)
    {
        InitializeComponent();

        // Listen for window resize events to reposition the extended splash screen image accordingly.
        // This ensures that the extended splash screen formats properly in response to window resizing.
        Window.Current.SizeChanged += new WindowSizeChangedEventHandler(ExtendedSplash_OnResize);

        splash = splashscreen;
        if (splash != null)
        {
            // Register an event handler to be executed when the splash screen has been dismissed.
            splash.Dismissed += new TypedEventHandler<SplashScreen, Object>(DismissedEventHandler);

            // Retrieve the window coordinates of the splash screen image.
            splashImageRect = splash.ImageLocation;
            PositionImage();

            // If applicable, include a method for positioning a progress control.
            PositionRing();
        }

        // Create a Frame to act as the navigation context
        rootFrame = new Frame();            
    }
    ```

    [Window.SizeChanged](/uwp/api/windows.ui.xaml.window.sizechanged) `ExtendedSplash_OnResize` 앱이 확장 된 시작 화면에서 이미지를 올바르게 배치 하도록 클래스 생성자에서 SizeChanged 처리기를 등록 해야 합니다 (예제에서는).

4.  **확장 시작 화면에 이미지를 배치할 클래스 메서드를 정의 합니다.**

    이 코드에서는 클래스 변수를 사용 하 여 확장 된 시작 화면 페이지에 이미지를 배치 하는 방법을 보여 줍니다 `splashImageRect` .

    ```cs
    void PositionImage()
    {
        extendedSplashImage.SetValue(Canvas.LeftProperty, splashImageRect.X);
        extendedSplashImage.SetValue(Canvas.TopProperty, splashImageRect.Y);
        extendedSplashImage.Height = splashImageRect.Height;
        extendedSplashImage.Width = splashImageRect.Width;
    }
    ```

5.  **필드 확장 시작 화면에서 진행률 컨트롤을 배치할 클래스 메서드를 정의 합니다.**

    확장 된 시작 화면에 [ProgressRing](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) 를 추가 하도록 선택한 경우 시작 화면 이미지를 기준으로 배치 합니다. ExtendedSplash.xaml.cs에 다음 코드를 추가 하 여 **ProgressRing** 32 픽셀을 이미지 아래로 가운데 맞춤 합니다.

    ```cs
    void PositionRing()
    {
        splashProgressRing.SetValue(Canvas.LeftProperty, splashImageRect.X + (splashImageRect.Width*0.5) - (splashProgressRing.Width*0.5));
        splashProgressRing.SetValue(Canvas.TopProperty, (splashImageRect.Y + splashImageRect.Height + splashImageRect.Height*0.1));
    }
    ```

6.  **클래스 내에서 해제 된 이벤트에 대 한 처리기를 정의 합니다.**

    ExtendedSplash.xaml.cs에서 클래스 변수를 true로 설정 하 여 [SplashScreen](/uwp/api/windows.applicationmodel.activation.splashscreen.dismissed) 이벤트가 발생할 때 응답 합니다 `dismissed` . 앱에 설치 작업이 있는 경우이 이벤트 처리기에 추가 합니다.

    ```cs
    // Include code to be executed when the system has transitioned from the splash screen to the extended splash screen (application's first view).
    void DismissedEventHandler(SplashScreen sender, object e)
    {
        dismissed = true;

        // Complete app setup operations here...
    }
    ```

    앱 설치가 완료 되 면 확장 된 시작 화면에서 밖으로 이동 합니다. 다음 코드는 `DismissExtendedSplash` `MainPage` 응용 프로그램의 mainpage .xaml 파일에 정의 된를 탐색 하는 라는 메서드를 정의 합니다.

    ```cs
    async void DismissExtendedSplash()
      {
         await Windows.ApplicationModel.Core.CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal,() =>            {
              rootFrame = new Frame();
              rootFrame.Content = new MainPage(); Window.Current.Content = rootFrame;
            });
      }
      ```

7.  **클래스 내에서 SizeChanged 이벤트에 대 한 처리기를 정의 합니다.**

    사용자가 창의 크기를 조정할 때 해당 요소의 위치를 변경 하기 위해 확장 된 시작 화면을 준비 합니다. 이 코드는 새 좌표를 캡처하고 이미지의 위치를 조정 하 여 [SizeChanged](/uwp/api/windows.ui.xaml.window.sizechanged) 이벤트가 발생할 때 응답 합니다. 진행률 컨트롤을 확장 된 시작 화면에 추가한 경우이 이벤트 처리기 내에서 해당 컨트롤의 위치를 변경 합니다.

    ```cs
    void ExtendedSplash_OnResize(Object sender, WindowSizeChangedEventArgs e)
    {
        // Safely update the extended splash screen image coordinates. This function will be executed when a user resizes the window.
        if (splash != null)
        {
            // Update the coordinates of the splash screen image.
            splashImageRect = splash.ImageLocation;
            PositionImage();

            // If applicable, include a method for positioning a progress control.
            // PositionRing();
        }
    }
    ```

    > [!NOTE]
    > 이미지 위치를 가져오기 전에 `splash` 예제에 표시 된 것 처럼 클래스 변수 ()에 유효한 [SplashScreen](/uwp/api/Windows.ApplicationModel.Activation.SplashScreen) 개체가 포함 되어 있는지 확인 합니다.

     

8.  **필드 저장 된 세션 상태를 복원 하는 클래스 메서드 추가**

    4 단계: [시작 활성화 처리기 수정](#modify-the-launch-activation-handler) 에서 [onlaunched](/uwp/api/windows.ui.xaml.application.onlaunched) 메서드에 추가한 코드는 시작 될 때 앱이 확장 된 시작 화면을 표시 하도록 합니다. 확장 된 시작 화면 클래스에서 앱 시작과 관련 된 모든 메서드를 통합 하려면 ExtendedSplash.xaml.cs 파일에 메서드를 추가 하 여 앱의 상태를 복원 하는 것이 좋습니다.

    ```cs
    void RestoreState(bool loadState)
    {
        if (loadState)
        {
             // code to load your app's state here
        }
    }
    ```

    App.xaml.cs에서 시작 활성화 처리기를 수정 하는 경우 `loadstate` 앱의 이전 [Applicationexecutionstate](/uwp/api/Windows.ApplicationModel.Activation.ApplicationExecutionState) 가 **종료**된 경우에도 true로 설정 됩니다. 이 경우 메서드는 `RestoreState` 앱을 이전 상태로 복원 합니다. 앱 시작, 일시 중단 및 종료에 대 한 개요는 [앱 수명 주기](app-lifecycle.md)를 참조 하세요.

## <a name="modify-the-launch-activation-handler"></a>시작 활성화 처리기 수정


앱이 시작 되 면 시스템은 시작 화면 정보를 앱의 시작 활성화 이벤트 처리기에 전달 합니다. 이 정보를 사용 하 여 확장 시작 화면 페이지에 이미지를 올바르게 배치할 수 있습니다. 앱의 [Onlaunched](/uwp/api/windows.ui.xaml.application.onlaunched) 된 처리기에 전달 된 활성화 이벤트 인수에서이 시작 화면 정보를 가져올 수 있습니다 ( `args` 다음 코드의 변수 참조).

앱에 대 한 [Onlaunched](/uwp/api/windows.ui.xaml.application.onlaunched) 된 처리기를 아직 재정의 하지 않은 경우 [응용 프로그램 수명 주기](app-lifecycle.md) 를 참조 하 여 활성화 이벤트를 처리 하는 방법을 알아보세요.

App.xaml.cs에서 다음 코드를 추가 하 여 확장 된 시작 화면을 만들고 표시 합니다.

```cs
protected override void OnLaunched(LaunchActivatedEventArgs args)
{
    if (args.PreviousExecutionState != ApplicationExecutionState.Running)
    {
        bool loadState = (args.PreviousExecutionState == ApplicationExecutionState.Terminated);
        ExtendedSplash extendedSplash = new ExtendedSplash(args.SplashScreen, loadState);
        Window.Current.Content = extendedSplash;
    }

    Window.Current.Activate();
}
```

## <a name="complete-code"></a>전체 코드

다음 코드는 이전 단계에 표시 된 코드 조각과 약간 다릅니다.
-   ExtendedSplash. xaml에는 단추가 포함 되어 있습니다. `DismissSplash` 이 단추를 클릭 하면 이벤트 처리기가 메서드를 `DismissSplashButton_Click` 호출 `DismissExtendedSplash` 합니다. 앱에서 `DismissExtendedSplash` 리소스를 로드 하거나 UI를 초기화 하는 작업이 완료 되 면를 호출 합니다.
-   또한이 앱은 [프레임](/uwp/api/Windows.UI.Xaml.Controls.Frame) 탐색을 사용 하는 UWP 앱 프로젝트 템플릿을 사용 합니다. 결과적으로, App.xaml.cs에서 실행 활성화 처리기 ([onlaunched](/uwp/api/windows.ui.xaml.application.onlaunched))는을 정의 하 `rootFrame` 고이를 사용 하 여 앱 창의 콘텐츠를 설정 합니다.

### <a name="extendedsplashxaml"></a>ExtendedSplash xaml

이 예제에는 `DismissSplash` 로드할 앱 리소스가 없으므로 단추가 포함 됩니다. 앱에서 리소스 로드 또는 초기 UI 준비를 완료 하면 확장 된 시작 화면을 자동으로 해제 합니다.

```xml
<Page
    x:Class="SplashScreenExample.ExtendedSplash"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:SplashScreenExample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="#464646">
        <Canvas>
            <Image x:Name="extendedSplashImage" Source="Assets/SplashScreen.png"/>
            <ProgressRing Name="splashProgressRing" IsActive="True" Width="20" HorizontalAlignment="Center"/>
        </Canvas>
        <StackPanel HorizontalAlignment="Center" VerticalAlignment="Bottom">
            <Button x:Name="DismissSplash" Content="Dismiss extended splash screen" HorizontalAlignment="Center" Click="DismissSplashButton_Click" />
        </StackPanel>
    </Grid>
</Page>
```

### <a name="extendedsplashxamlcs"></a>ExtendedSplash.xaml.cs

`DismissExtendedSplash`메서드는 단추의 click 이벤트 처리기에서 호출 됩니다 `DismissSplash` . 앱에서 단추가 필요 하지 않습니다 `DismissSplash` . 대신 응용 프로그램에서 `DismissExtendedSplash` 리소스 로딩을 완료 하 고 기본 페이지로 이동 하려는 경우를 호출 합니다.

```cs
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;

using Windows.ApplicationModel.Activation;
using SplashScreenExample.Common;
using Windows.UI.Core;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/p/?LinkID=234238

namespace SplashScreenExample
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    partial class ExtendedSplash : Page
    {
        internal Rect splashImageRect; // Rect to store splash screen image coordinates.
        private SplashScreen splash; // Variable to hold the splash screen object.
        internal bool dismissed = false; // Variable to track splash screen dismissal status.
        internal Frame rootFrame;

        public ExtendedSplash(SplashScreen splashscreen, bool loadState)
        {
            InitializeComponent();

            // Listen for window resize events to reposition the extended splash screen image accordingly.
            // This is important to ensure that the extended splash screen is formatted properly in response to snapping, unsnapping, rotation, etc...
            Window.Current.SizeChanged += new WindowSizeChangedEventHandler(ExtendedSplash_OnResize);

            splash = splashscreen;

            if (splash != null)
            {
                // Register an event handler to be executed when the splash screen has been dismissed.
                splash.Dismissed += new TypedEventHandler<SplashScreen, Object>(DismissedEventHandler);

                // Retrieve the window coordinates of the splash screen image.
                splashImageRect = splash.ImageLocation;
                PositionImage();

                // Optional: Add a progress ring to your splash screen to show users that content is loading
                PositionRing();
            }

            // Create a Frame to act as the navigation context
            rootFrame = new Frame();

            // Restore the saved session state if necessary
            RestoreState(loadState);
        }

        void RestoreState(bool loadState)
        {
            if (loadState)
            {
                // TODO: write code to load state
            }
        }

        // Position the extended splash screen image in the same location as the system splash screen image.
        void PositionImage()
        {
            extendedSplashImage.SetValue(Canvas.LeftProperty, splashImageRect.X);
            extendedSplashImage.SetValue(Canvas.TopProperty, splashImageRect.Y);
            extendedSplashImage.Height = splashImageRect.Height;
            extendedSplashImage.Width = splashImageRect.Width;

        }

        void PositionRing()
        {
            splashProgressRing.SetValue(Canvas.LeftProperty, splashImageRect.X + (splashImageRect.Width*0.5) - (splashProgressRing.Width*0.5));
            splashProgressRing.SetValue(Canvas.TopProperty, (splashImageRect.Y + splashImageRect.Height + splashImageRect.Height*0.1));
        }

        void ExtendedSplash_OnResize(Object sender, WindowSizeChangedEventArgs e)
        {
            // Safely update the extended splash screen image coordinates. This function will be fired in response to snapping, unsnapping, rotation, etc...
            if (splash != null)
            {
                // Update the coordinates of the splash screen image.
                splashImageRect = splash.ImageLocation;
                PositionImage();
                PositionRing();
            }
        }

        // Include code to be executed when the system has transitioned from the splash screen to the extended splash screen (application's first view).
        void DismissedEventHandler(SplashScreen sender, object e)
        {
            dismissed = true;

            // Complete app setup operations here...
        }

        void DismissExtendedSplash()
        {
            // Navigate to mainpage
            rootFrame.Navigate(typeof(MainPage));
            // Place the frame in the current Window
            Window.Current.Content = rootFrame;
        }

        void DismissSplashButton_Click(object sender, RoutedEventArgs e)
        {
            DismissExtendedSplash();
        }
    }
}
```

### <a name="appxamlcs"></a>App.xaml.cs

이 프로젝트는 Visual Studio에서 UWP 앱 **빈 앱 (XAML)** 프로젝트 템플릿을 사용 하 여 만들었습니다. `OnNavigationFailed`및 `OnSuspending` 이벤트 처리기가 모두 자동으로 생성 되며 확장 된 시작 화면을 구현 하기 위해 변경할 필요가 없습니다. 이 항목에서는만 수정 `OnLaunched` 합니다.

앱에 대 한 프로젝트 템플릿을 사용 하지 않은 경우에는 4 단계: 프레임 탐색을 사용 하지 않는 수정 된의 예제에 대해 4 단계: [시작 활성화 처리기 수정](#modify-the-launch-activation-handler) 을 참조 하세요 `OnLaunched` . [Frame](/uwp/api/Windows.UI.Xaml.Controls.Frame)

```cs
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
using Windows.ApplicationModel;
using Windows.ApplicationModel.Activation;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;

// The Blank Application template is documented at https://go.microsoft.com/fwlink/p/?LinkID=234227

namespace SplashScreenExample
{
    /// <summary>
    /// Provides application-specific behavior to supplement the default Application class.
    /// </summary>
    sealed partial class App : Application
    {
        /// <summary>
        /// Initializes the singleton application object.  This is the first line of authored code
        /// executed, and as such is the logical equivalent of main() or WinMain().
        /// </summary>
        public App()
        {
            Microsoft.ApplicationInsights.WindowsAppInitializer.InitializeAsync(
            Microsoft.ApplicationInsights.WindowsCollectors.Metadata |
            Microsoft.ApplicationInsights.WindowsCollectors.Session);
            this.InitializeComponent();
            this.Suspending += OnSuspending;
        }

        /// <summary>
        /// Invoked when the application is launched normally by the end user.  Other entry points
        /// will be used such as when the application is launched to open a specific file.
        /// </summary>
        /// <param name="e">Details about the launch request and process.</param>
        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
#if DEBUG
            if (System.Diagnostics.Debugger.IsAttached)
            {
                this.DebugSettings.EnableFrameRateCounter = true;
            }
#endif

            Frame rootFrame = Window.Current.Content as Frame;

            // Do not repeat app initialization when the Window already has content,
            // just ensure that the window is active
            if (rootFrame == null)
            {
                // Create a Frame to act as the navigation context and navigate to the first page
                rootFrame = new Frame();
                // Set the default language
                rootFrame.Language = Windows.Globalization.ApplicationLanguages.Languages[0];

                rootFrame.NavigationFailed += OnNavigationFailed;

                //  Display an extended splash screen if app was not previously running.
                if (e.PreviousExecutionState != ApplicationExecutionState.Running)
                {
                    bool loadState = (e.PreviousExecutionState == ApplicationExecutionState.Terminated);
                    ExtendedSplash extendedSplash = new ExtendedSplash(e.SplashScreen, loadState);
                    rootFrame.Content = extendedSplash;
                    Window.Current.Content = rootFrame;
                }
            }

            if (rootFrame.Content == null)
            {
                // When the navigation stack isn't restored navigate to the first page,
                // configuring the new page by passing required information as a navigation
                // parameter
                rootFrame.Navigate(typeof(MainPage), e.Arguments);
            }
            // Ensure the current window is active
            Window.Current.Activate();
        }

        /// <summary>
        /// Invoked when Navigation to a certain page fails
        /// </summary>
        /// <param name="sender">The Frame which failed navigation</param>
        /// <param name="e">Details about the navigation failure</param>
        void OnNavigationFailed(object sender, NavigationFailedEventArgs e)
        {
            throw new Exception("Failed to load Page " + e.SourcePageType.FullName);
        }

        /// <summary>
        /// Invoked when application execution is being suspended.  Application state is saved
        /// without knowing whether the application will be terminated or resumed with the contents
        /// of memory still intact.
        /// </summary>
        /// <param name="sender">The source of the suspend request.</param>
        /// <param name="e">Details about the suspend request.</param>
        private void OnSuspending(object sender, SuspendingEventArgs e)
        {
            var deferral = e.SuspendingOperation.GetDeferral();
            // TODO: Save applicaiton state and stop any background activity
            deferral.Complete();
        }
    }
}
```

## <a name="related-topics"></a>관련 항목


* [앱 수명 주기](app-lifecycle.md)

**참조**

* [Windows ApplicationModel. Activation 네임 스페이스](/uwp/api/Windows.ApplicationModel.Activation)
* [SplashScreen 클래스입니다.](/uwp/api/Windows.ApplicationModel.Activation.SplashScreen)
* [ImageLocation 속성을 SplashScreen.](/uwp/api/windows.applicationmodel.activation.splashscreen.imagelocation)
* [CoreApplicationView 이벤트가 발생 합니다.](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated)

 

 