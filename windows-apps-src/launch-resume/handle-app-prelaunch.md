---
title: 앱 사전 실행 처리
description: OnLaunched 메서드를 재정의하여 앱 사전 실행을 처리하는 방법을 알아봅니다.
ms.assetid: A4838AC2-22D7-46BA-9EB2-F3C248E22F52
---

# 앱 사전 실행 처리


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)

[
            **OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 메서드를 재정의하여 앱 사전 실행을 처리하는 방법을 알아봅니다.

## 소개


사용 가능한 시스템 리소스를 허용하면 사용자가 가장 자주 사용하는 앱을 백그라운드로 미리 시작하여 Windows 스토어 앱의 시작 성능을 향상시킵니다. 사전 실행된 앱은 시작된 후 일시 중단 상태가 됩니다. 사용자가 앱을 호출하면 앱이 일시 중단 상태에서 실행 상태로 전환되어 다시 시작합니다. 이는 앱 콜드를 시작하는 것보다 빠릅니다.

Windows 10 이전 버전에서는 앱이 자동으로 사전 실행을 활용하지 않았습니다. Windows 10부터 모든 UWP(유니버설 Windows 플랫폼) 앱이 자동으로 사전 실행을 활용합니다.

대부분의 앱은 다른 변경 없이 사전 실행이 작동합니다. 그러나 일부 유형의 앱은 사전 실행이 잘 작동하려면 시작 동작을 변경해야 할 수 있습니다. 예를 들어 시작하는 동안 사용자의 온라인 표시를 변경하는 메시지 앱 또는 앱이 시작할 때 사용자가 있다고 가정하여 정교한 시각 효과를 표시하는 게임입니다.

## 사전 실행 및 앱 수명 주기


앱이 사전 실행되면 곧 일시 중단 상태가 됩니다. ([앱 일시 중단 처리](suspend-an-app.md)를 참조하세요.)

## 사전 실행 감지 및 처리


앱이 활성화되는 동안 [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740) 플래그를 수신합니다. 이 플래그를 사용하여 다음 [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)의 발췌에 표시된 대로 사용자가 명시적으로 앱을 시작할 때에만 작업을 수행할지 여부를 결정합니다.

```cs
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content - rather just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();

        rootFrame.NavigationFailed += OnNavigationFailed;

        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }

        if (!e.PrelaunchActivated)
        {
            // TODO: This is not a prelaunch activation. Perform operations which
            // assume that the user explicitly launched the app such as updating
            // the online presence of the user on a social network, updating a 
            // what&#39;s new feed, etc.
        }

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }

    if (rootFrame.Content == null)
    {
        // When the navigation stack isn&#39;t restored navigate to the first page,
        // configuring the new page by passing required information as a navigation parameter
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
    }
    // Ensure the current window is active
    Window.Current.Activate();
}
```

**팁** 사전 실행을 옵트아웃(opt out)하려면 [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740) 플래그를 확인합니다. 이 플래그가 설정되어 있으면 프레임을 만들거나 창을 활성화하는 작업을 수행하기 전에 OnLaunched()에서 반환합니다.

 

## VisibilityChanged 이벤트 사용


사전 실행으로 활성화된 앱이 사용자에게 표시되지 않습니다. 사용자가 전환할 때 표시됩니다. 앱의 주 창이 표시될 때까지 특정 작업을 지연시킬 수 있습니다. 예를 들어 앱이 피드에서 새 항목 목록을 표시하면 앱이 사전 실행되었을 때 작성되어 사용자가 앱을 활성화하는 시점까지 상태가 오래될 수 있는 목록을 사용하는 대신 [**VisibilityChanged**](https://msdn.microsoft.com/library/windows/apps/hh702458) 이벤트가 발생하는 동안 목록을 업데이트할 수 있습니다. 다음 코드는 **MainPage**의 **VisibilityChanged** 이벤트를 처리합니다.

```cs
public sealed partial class MainPage : Page
{
    public MainPage()
    {
        this.InitializeComponent();

        Window.Current.VisibilityChanged += WindowVisibilityChangedEventHandler;
    }

    void WindowVisibilityChangedEventHandler(System.Object sender, Windows.UI.Core.VisibilityChangedEventArgs e)
    {
        // Perform operations that should take place when the application becomes visible rather than 
        // when it is prelaunched, such as building a what&#39;s new feed 
    }
}
```

## 지침


-   앱이 신속하게 일시 중단 상태가 되지 못하면 앱이 종료되므로 사전 실행 동안에는 장기 실행 작업을 수행하면 안 됩니다.
-   앱이 표시되지 않고 오디오 재생의 이유가 명확하지 않으므로 앱을 사전 실행할 때 앱이 [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)의 오디오 재생을 시작하면 안 됩니다.
-   사용자에게 표시되거나 명시적으로 사용자가 시작했다고 가정하는 앱은 시작하는 동안 어떤 작업도 수행하면 안 됩니다. 앱이 현재 명시적인 사용자 작업 없이 백그라운드에서 시작되었기 때문에 개발자는 개인 정보, 사용자 환경 및 성능 영향을 고려해야 합니다.
    -   개인 정보가 고려되어야 하는 예는 소셜 앱이 사용자의 상태를 온라인으로 변경해야 하는 경우입니다. 앱이 사전 실행되면 상태를 변경하는 대신 사용자가 앱으로 전환할 때까지 기다려야 합니다.
    -   사용자 환경이 고려되는 예는 시작할 때 소개 시퀀스를 표시하는 게임과 같은 앱의 경우 사용자가 앱으로 전환할 때까지 소개 시퀀스를 지연시킬 수 있는 경우입니다.
    -   성능 영향의 예는 앱이 사전 실행될 때 현재 날씨 정보를 로드하는 대신 사용자가 앱으로 전환하여 이 정보를 검색한 다음 앱이 표시되면 다시 로드하여 해당 정보가 최신 정보인지 확인해야 하는 경우입니다.
-   앱이 시작되었을 때 라이브 타일을 지우면 VisibilityChanged 이벤트가 발생할 때까지 이를 연기합니다.
-   앱에 대한 원격 분석으로 일반적인 타일 활성화와 사전 실행된 활성화를 구별하여 문제가 발생한 시나리오를 식별할 수 있습니다.
-   Microsoft Visual Studio 2015 Update 1 및 Windows 10, 버전 1511을 사용하는 경우 **디버그** &gt; **기타 디버그 대상** &gt; **Debug Windows Universal App PreLaunch(Windows 유니버설 앱 사전 실행 디버그)**를 선택하여 Visual Studio 2015에서 앱에 대한 사전 실행을 시뮬레이트할 수 있습니다.

## 관련 항목

* [앱 수명 주기](app-lifecycle.md)

 

 





<!--HONumber=Mar16_HO1-->


