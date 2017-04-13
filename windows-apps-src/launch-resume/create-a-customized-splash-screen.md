---
author: TylerMSFT
title: "시작 화면을 더 오래 표시"
description: "앱의 연장된 시작 화면을 만들어 시작 화면을 더 오랫동안 표시합니다. 이 연장된 화면은 앱을 시작할 때 표시되는 시작 화면을 모방하지만 사용자 지정할 수 있습니다."
ms.assetid: CD3053EB-7F86-4D74-9C5A-950303791AE3
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 0a7a51cdd330bc361b7d0cec3a002c5b1aee9de4
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="display-a-splash-screen-for-more-time"></a>시작 화면을 더 오래 표시


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**SplashScreen 클래스**](https://msdn.microsoft.com/library/windows/apps/br224763)
-   [**Window.SizeChanged 이벤트**](https://msdn.microsoft.com/library/windows/apps/br209055)
-   [**Application.OnLaunched 메서드**](https://msdn.microsoft.com/library/windows/apps/br242335)

앱의 연장된 시작 화면을 만들어 시작 화면을 더 오랫동안 표시합니다. 이 연장된 화면은 앱을 시작할 때 표시되는 시작 화면을 모방하지만 사용자 지정할 수 있습니다. 실시간 로드 정보를 표시할지 또는 앱에 초기 UI를 준비할 추가 시간을 제공할지에 관계없이 연장된 시작 화면을 사용하여 시작 환경을 정의할 수 있습니다.

> **참고**  이 항목의 "연장된 시작 화면" 구는 연장된 기간 동안 화면에 유지되는 초기 화면을 가리킵니다. [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763) 클래스에서 파생되는 하위 클래스를 의미하는 것은 아닙니다.

 

이러한 권장 사항을 따라 연장된 시작 화면이 기본 시작 화면을 정확하게 모방하도록 합니다.

-   연장된 시작 화면 페이지가 앱 매니페스트의 시작 화면에 대해 지정된 이미지(앱 시작 화면 이미지)와 일치하는 620 x 300 픽셀 이미지를 사용해야 합니다. Microsoft Visual Studio 2015에서는 시작 화면 설정이 앱 매니페스트(Package.appxmanifest 파일)에 있는 **시각적 자산** 탭의 **시작 화면** 섹션에 저장됩니다.
-   연장된 시작 화면은 앱 매니페스트에서 시작 화면에 대해 지정된 배경색(앱 시작 화면 배경)과 일치하는 배경색을 사용해야 합니다.
-   코드에서 [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763) 클래스를 사용하여 기본 시작 화면과 동일한 화면 좌표에 앱 시작 화면 이미지를 배치해야 합니다.
-   [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763) 클래스를 통해 연장된 시작 화면의 항목 위치를 변경하여 코드에서 화면이 회전되거나 앱이 화면의 다른 앱으로 이동되는 경우 등의 창 크기 조정 이벤트에 응답해야 합니다.

기본 시작 화면을 효과적으로 모방하는 연장된 시작 화면을 만들려면 다음 단계를 수행합니다.

## <a name="add-a-blank-page-item-to-your-existing-app"></a>기존 앱에 **빈 페이지** 추가


이 항목에서는 C#, Visual Basic 또는 C++로 작성한 기존 UWP(유니버설 Windows 플랫폼) 앱 프로젝트에 연장된 시작 화면을 추가하려 한다고 가정합니다.

-   Visual Studio 2015에서 앱을 엽니다.
-   누르거나 메뉴 모음에서 **프로젝트**를 열고 **새 항목 추가**를 클릭합니다. **새 항목 추가** 대화 상자가 나타납니다.
-   이 대화 상자에서 앱에 새로운 **빈 페이지**를 추가합니다. 이 항목에서는 연장된 시작 화면 페이지 이름을 "ExtendedSplash"로 지정합니다.

**빈 페이지** 항목을 추가하면 두 개의 파일이 생성됩니다. 하나는 태그 파일(ExtendedSplash.xaml)이고 다른 하나는 코드 파일(ExtendedSplash.xaml.cs)입니다.

## <a name="essential-xaml-for-an-extended-splash-screen"></a>연장된 시작 화면의 필수 XAML


연장된 시작 화면에 이미지 및 진행률 컨트롤을 추가하려면 다음 단계를 수행하세요.

ExtendedSplash.xaml 파일에서 다음을 수행합니다.

-   앱 매니페스트(Package.appxmanifest 파일의 **시각적 자산** 섹션)에서 앱 시작 화면에 대해 설정한 배경색과 일치하도록 기본 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 요소의 [**Background**](https://msdn.microsoft.com/library/windows/apps/br209396) 속성을 변경합니다. 기본 시작 화면 색은 연한 회색(16진수 값 \#464646)입니다. 이 **Grid** 요소는 기본적으로 새로운 **빈 페이지**를 만들 때 제공됩니다. **Grid**를 사용하지 않아도 됩니다. 연장된 시작 화면을 빌드하기 위해 편의상 사용된 것입니다.
-   [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)에 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 요소를 추가합니다. 이 **Canvas**를 사용하여 연장된 시작 화면 이미지를 배치합니다.
-   [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267)에 [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752) 요소를 추가합니다. 기본 시작 화면에 대해 선택한 것과 동일한 600x320 픽셀 이미지를 연장된 시작 화면에 사용합니다.
-   (옵션) 진행률 컨트롤을 추가하여 사용자에게 앱이 로드되고 있음을 표시합니다. 이 항목에서는 확정되었거나 확정되지 않은 [**ProgressBar**](https://msdn.microsoft.com/library/windows/apps/br227529) 대신 [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538)을 추가합니다.

ExtendedSplash.xaml에서 다음 코드를 추가하여 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 및 [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752) 요소와 [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) 컨트롤을 정의합니다.

```xml
    <Grid Background="#464646">
        <Canvas>
            <Image x:Name="extendedSplashImage" Source="Assets/SplashScreen.png"/>
            <ProgressRing Name="splashProgressRing" IsActive="True" Width="20" HorizontalAlignment="Center"></ProgressRing>
        </Canvas>
    </Grid>
```

**참고**  이 코드는 [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538)의 너비를 20픽셀로 설정합니다. 수동으로 너비를 앱에 적합한 값으로 설정할 수 있지만 20픽셀 미만의 너비에서는 컨트롤이 렌더링되지 않습니다.

 

## <a name="essential-code-for-an-extended-splash-screen-class"></a>연장된 시작 화면 클래스의 필수 코드


연장된 시작 화면은 창 크기(Windows만 해당) 또는 방향이 변경될 때마다 응답해야 합니다. 창 변경 방식과 관계없이 연장된 시작 화면이 적절하게 표시되도록 사용하는 이미지 위치를 업데이트해야 합니다.

연장된 시작 화면을 올바르게 표시할 메서드를 정의하려면 다음 단계를 수행하세요.

1.  **필수 네임스페이스 추가**

    [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763) 클래스, [**Window.SizeChanged**](https://msdn.microsoft.com/library/windows/apps/br209055) 이벤트에 액세스하려면 다음 네임스페이스를 ExtendedSplash.xaml.cs에 추가해야 합니다.

    ```cs
    using Windows.ApplicationModel.Activation;
    using Windows.UI.Core;
    ```

2.  **partial 클래스 만들기 및 클래스 변수 선언**

    ExtendedSplash.xaml.cs에 다음 코드를 포함하여 연장된 시작 화면을 나타낼 partial 클래스를 만듭니다.

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

    다음 클래스 변수는 여러 메서드에서 사용됩니다. `splashImageRect` 변수는 시스템이 앱의 시작 화면 이미지를 표시한 좌표를 저장합니다. `splash` 변수는 [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763) 개체를 저장하고 `dismissed` 변수는 시스템에 표시되는 시작 화면이 해제되었는지 여부를 추적합니다.

3.  **이미지를 올바르게 배치하는 클래스 생성자를 정의합니다.**

    다음 코드에서는 창 크기 조정 이벤트를 수신 대기하고, 연장된 시작 화면에 이미지 및 (옵션) 진행률 컨트롤을 배치하고, 탐색용 프레임을 만들고, 비동기 메서드를 호출하여 저장된 세션 상태를 복원하는 연장된 시작 화면 클래스의 생성자를 정의합니다.

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

    앱이 연장된 시작 화면에 이미지를 올바르게 배치하도록 클래스 생성자에 [**Window.SizeChanged**](https://msdn.microsoft.com/library/windows/apps/br209055) 처리기(예제의 `ExtendedSplash_OnResize`)를 록해야 합니다.

4.  **연장된 시작 화면에서 이미지를 배치하는 클래스 메서드 정의**

    이 코드에서는 `splashImageRect` 클래스 변수를 사용하여 연장된 시작 화면 페이지에 이미지를 배치하는 방법을 보여 줍니다.

    ```cs
    void PositionImage()
    {
        extendedSplashImage.SetValue(Canvas.LeftProperty, splashImageRect.X);
        extendedSplashImage.SetValue(Canvas.TopProperty, splashImageRect.Y);
        extendedSplashImage.Height = splashImageRect.Height;
        extendedSplashImage.Width = splashImageRect.Width;
    }
    ```

5.  **(옵션) 연장된 시작 화면에 진행률 표시줄을 배치하는 클래스 메서드 정의**

    연장된 시작 화면에 [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538)을 추가하는 경우 시작 화면 이미지를 기준으로 배치합니다. ExtendedSplash.xaml.cs에 다음 코드를 추가하여 **ProgressRing**의 중심을 이미지보다 32픽셀 아래로 지정합니다.

    ```cs
    void PositionRing()
    {
        splashProgressRing.SetValue(Canvas.LeftProperty, splashImageRect.X + (splashImageRect.Width*0.5) - (splashProgressRing.Width*0.5));
        splashProgressRing.SetValue(Canvas.TopProperty, (splashImageRect.Y + splashImageRect.Height + splashImageRect.Height*0.1));
    }
    ```

6.  **클래스 내에서 Dismissed 이벤트 처리기 정의**

    ExtendedSplash.xaml.cs에서 `dismissed` 클래스 변수를 true로 설정하여 [**SplashScreen.Dismissed**](https://msdn.microsoft.com/library/windows/apps/br224764) 이벤트가 발생할 때 응답합니다. 앱에 설치 작업이 있는 경우 이 이벤트 처리기에 추가합니다.

    ```cs
    // Include code to be executed when the system has transitioned from the splash screen to the extended splash screen (application's first view).
    void DismissedEventHandler(SplashScreen sender, object e)
    {
        dismissed = true;

        // Complete app setup operations here...
    }
    ```

    앱 설치가 완료되면 연장된 시작 화면을 닫습니다. 다음 코드에서는 앱의 MainPage.xaml 파일에 정의된 `MainPage`로 이동하는 `DismissExtendedSplash` 메서드를 정의합니다.

    ```cs
    void DismissExtendedSplash()
    {
        // Navigate to mainpage
        rootFrame.Navigate(typeof(MainPage));
        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }
    ```

7.  **클래스 내에서 Window.SizeChanged 이벤트 처리기 정의**

    사용자가 창 크기를 조정할 경우 해당 요소의 위치를 변경하도록 연장된 시작 화면을 준비합니다. 이 코드에서는 새로운 좌표를 캡처하고 이미지 위치를 변경하여 [**Window.SizeChanged**](https://msdn.microsoft.com/library/windows/apps/br209055) 이벤트가 발생할 때 응답합니다. 연장된 시작 화면에 진행률 컨트롤을 추가한 경우에도 이 이벤트 처리기 내에서 위치를 변경합니다.

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

    **참고**  이미지 위치를 가져오기 전에 예제에 표시된 것처럼 클래스 변수(`splash`)에 올바른 [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763) 개체가 있는지 확인합니다.

     

8.  **(선택 사항) 저장된 세션 상태를 복원하는 클래스 메서드 추가**

    4단계, [시작 활성화 처리기 수정](#modify-the-launch-activation-handler)에서 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 메서드에 추가한 코드로 인해 앱을 시작할 때 연장된 시작 화면이 표시됩니다. 연장된 시작 화면 클래스에 앱 실행과 관련된 모든 메서드를 통합하려면 비동기 메서드를 ExtendedSplash.xaml.cs 파일에 추가하여 앱 상태를 복원하는 것이 좋습니다.

    ```cs
    async void RestoreStateAsync(bool loadState)
    {
        if (loadState)
        {
             // code to load your app's state here
        }
    }
    ```

    App.xaml.cs에서 시작 활성화 처리기를 수정할 때 앱의 이전 [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694)가 **Terminated**인 경우에도 `loadstate`를 true로 설정합니다. 그러면 `RestoreStateAsync` 메서드가 앱을 이전 상태로 복원합니다. 앱 실행, 일시 중단 및 종료에 대한 개요는 [앱 수명 주기](app-lifecycle.md)를 참조하세요.

## <a name="modify-the-launch-activation-handler"></a>시작 활성화 처리기 수정


앱이 시작되면 시스템은 앱의 시작 활성화 이벤트 처리기에 시작 화면 정보를 전달합니다. 이 정보를 사용하여 연장된 시작 화면 페이지에 이미지를 정확히 배치할 수 있습니다. 이러한 시작 화면 정보를 앱의 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 처리기에 전달되는 활성화 이벤트 인수에서 가져올 수 있습니다(다음 코드의 `args` 변수 참조).

앱의 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 처리기를 아직 재정의하지 않은 경우 [앱 수명 주기](app-lifecycle.md)를 참조하여 활성화 이벤트를 처리하는 방법을 알아봅니다.

App.xaml.cs에서 다음 코드를 추가하여 연장된 시작 화면을 만들고 표시합니다.

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


> **참고**  다음 코드는 이전 단계에서 표시된 조각과 약간 다릅니다.
-   ExtendedSplash.xaml에는 `DismissSplash` 단추가 포함되어 있습니다. 이 단추를 클릭하면 이벤트 처리기 `DismissSplashButton_Click`에서 `DismissExtendedSplash` 메서드를 호출합니다. 앱이 리소스 로드 또는 UI 초기화를 완료하면 앱에서 `DismissExtendedSplash`를 호출합니다.
-   또한 이 앱은 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) 탐색이 사용되는 UWP 앱 프로젝트 템플릿을 사용합니다. 따라서 App.xaml.cs에서 시작 활성화 처리기([**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335))는 `rootFrame`을 정의하고 사용하여 앱 창의 콘텐츠를 설정합니다.

ExtendedSplash.xaml: 이 예제에는 로드할 앱 리소스가 없으므로 `DismissSplash` 단추가 포함되어 있습니다. 앱이 리소스 로드 또는 초기 UI 준비를 완료하면 앱에서 연장된 시작 화면을 자동으로 닫습니다.

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

ExtendedSplash.xaml.cs: `DismissExtendedSplash` 메서드는 `DismissSplash` 단추에 대한 click 이벤트 처리기에서 호출됩니다. 앱에는 `DismissSplash` 단추가 필요하지 않습니다. 대신, 앱이 리소스 로드를 완료하고 기본 페이지로 이동하려는 경우 `DismissExtendedSplash`를 호출합니다.

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

// The Blank Page item template is documented at http://go.microsoft.com/fwlink/p/?LinkID=234238

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
            await RestoreStateAsync(loadState);
        }

        async void RestoreStateAsync(bool loadState)
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

App.xaml.cs: 이 프로젝트는 Visual Studio 2015의 UWP 앱 **빈 앱(XAML)** 프로젝트 템플릿을 사용하여 만든 것입니다. `OnNavigationFailed` 및 `OnSuspending` 이벤트 처리기가 모두 자동으로 생성되며 연장된 시작 화면을 구현하기 위해 변경할 필요가 없습니다. 이 항목에서는 `OnLaunched`만 수정합니다.

앱에 프로젝트 템플릿을 사용하지 않은 경우 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) 탐색을 사용하지 않는 수정된 `OnLaunched`의 예제를 보려면 4단계: [시작 활성화 처리기 수정](#modify-the-launch-activation-handler)을 참조하세요.

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

// The Blank Application template is documented at http://go.microsoft.com/fwlink/p/?LinkID=234227

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

* [**Windows.ApplicationModel.Activation 네임스페이스**](https://msdn.microsoft.com/library/windows/apps/br224766)
* [**Windows.ApplicationModel.Activation.SplashScreen 클래스**](https://msdn.microsoft.com/library/windows/apps/br224763)
* [**Windows.ApplicationModel.Activation.SplashScreen.ImageLocation 속성**](https://msdn.microsoft.com/library/windows/apps/br224765)
* [**Windows.ApplicationModel.Core.CoreApplicationView.Activated 이벤트**](https://msdn.microsoft.com/library/windows/apps/br225018)

 

 
