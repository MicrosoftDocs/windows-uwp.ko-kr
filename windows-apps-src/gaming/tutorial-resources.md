---
게임 샘플 확장
축하합니다! 이제 기본 UWP(유니버설 Windows 플랫폼) DirectX 3D 게임의 주요 구성 요소에 대해 이해하셨습니다.
ms.assetid: a1432c45-569e-7ecd-4098-f5ad6da9327e
키워드: ["DirectX, XAML"]
---

# 게임 샘플 확장


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

축하합니다! 이제 기본 UWP(유니버설 Windows 플랫폼) DirectX 3D 게임의 주요 구성 요소에 대해 이해하셨습니다. 뷰 공급자 및 렌더링 파이프라인을 포함한 게임의 프레임워크를 설정하고 기본 게임 루프를 구현할 수 있습니다. 또한 기본 사용자 인터페이스 오버레이를 만들고 사운드 및 컨트롤을 통합할 수 있습니다. 지금은 자신만의 고유한 게임을 만들고 있는 중이며, 다음은 DirectX 게임 개발에 대한 지식을 발전시킬 수 있는 몇 가지 리소스입니다.

-   [DirectX 그래픽 및 게임](https://msdn.microsoft.com/library/windows/desktop/ee663274)
-   [Direct3D 11 개요](https://msdn.microsoft.com/library/windows/desktop/ff476345)
-   [Direct3D 11 참조](https://msdn.microsoft.com/library/windows/desktop/ff476147)

## 게임 샘플 확장: 오버레이에 XAML 사용


자세히 살펴보지 않은 한 가지 다른 방법은 오버레이에 Direct2D 대신 XAML을 사용하는 것입니다. XAML은 사용자 인터페이스 요소를 그릴 때 Direct2D에 비해 많은 장점이 있으며, 가장 중요한 점은 Windows 10 모양과 느낌을 DirectX 게임에 보다 편리하게 통합할 수 있다는 것입니다. UWP 앱을 정의하는 대부분의 공통 요소, 스타일 및 동작이 XAML 모델로 긴밀하게 통합되어 게임 개발자가 훨씬 더 적은 작업으로 구현할 수 있습니다. 고유한 게임 디자인에 복잡한 사용자 인터페이스가 있는 경우 Direct2D 대신 XAML 사용을 고려해 보세요.

그렇다면 Direct2D를 사용한 사용자 인터페이스 구현과 XAML을 사용하는 동일한 인터페이스 구현의 차이점은 무엇일까요?

-   오버레이는 수동으로 Direct2D 대상 버퍼에 배치되고 기록된 Direct2D 원형 및 DirectWrite 문자열이 아닌 XAML 파일 \*.xaml에 정의합니다. XAML을 이해하면 보다 복잡한 오버레이를 훨씬 더 쉽게 만들고 구성할 수 있다는 사실을 확인할 수 있습니다. 특히 Visual Studio의 XAML 편집 도구를 사용하는 경우는 더욱 그렇습니다.
-   사용자 인터페이스 요소는 [**Windows::UI::Xaml**](https://msdn.microsoft.com/library/windows/apps/br209045) 및 [**Windows::UI::Xaml::Controls**](https://msdn.microsoft.com/library/windows/apps/br227716)를 포함하여 Windows 런타임 XAML API의 일부인 표준화된 요소에서 가져옵니다. XAML 사용자 인터페이스 요소의 동작을 처리하는 코드는 코드 숨김 파일인 Main.xaml.cpp에 정의되어 있습니다.
-   긴밀하게 통합된 Windows 런타임 구성 요소인 XAML은 기본적으로 크기 조정 및 보기 상태 변경 이벤트를 처리하고 그에 따라 오버레이를 변환하므로 오버레이의 구성 요소를 다시 그리는 방법을 수동으로 지정할 필요가 없습니다.
-   스왑 체인은 [**Windows::UI::Core::CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 개체에 직접 연결되지 않거나 최소한 직접 연결하지 않아도 됩니다. 대신 새 [**SwapChainBackgroundPanel**](https://msdn.microsoft.com/library/windows/apps/hh702626) 개체가 구성되면 XAML을 통합하는 DirectX 앱에서 스왑 체인을 연결합니다. **SwapChainBackgroundPanel** 개체는 앱 단일 항목에 의한 시작 시 만들어지는 현재 창 개체의 [**Content**](https://msdn.microsoft.com/library/windows/apps/br209051) 속성으로 설정되며 창은 **CoreWindow** 개체로 **Simple3DGame::Initialize**에 전달됩니다.

다음과 같이 **Main.app.xaml** 파일에서 **SwapChainBackgroundPanel**에 대한 XAML을 선언합니다.

```xaml
<Page
    x:Name="DXMainPage"
    x:Class="Simple3DGameXaml.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignWidth="1366"
    d:DesignHeight="768">

    <SwapChainBackgroundPanel x:Name="DXSwapChainPanel">

    <!-- ... XAML user controls and elements -->

    </SwapChainBackgroundPanel>
</Page>
```

```cpp
void App::OnLaunched(LaunchActivatedEventArgs^ /* args */)
{
    Suspending += ref new SuspendingEventHandler(this, &App::OnSuspending);
    Resuming += ref new EventHandler<Object^>(this, &App::OnResuming);

    m_mainPage = ref new MainPage();
    m_mainPage->SetApp(this);

    Window::Current->Content = m_mainPage;
    Window::Current->Activated += ref new WindowActivatedEventHandler(this, &App::OnWindowActivationChanged);
    Window::Current->Activate();

    m_controller = ref new MoveLookController();
    m_renderer = ref new GameRenderer();
    m_game = ref new Simple3DGame();

    auto window = Window::Current->CoreWindow;

    window->SizeChanged +=
        ref new TypedEventHandler<CoreWindow^, WindowSizeChangedEventArgs^>(this, &App::OnWindowSizeChanged);

    window->VisibilityChanged +=
        ref new TypedEventHandler<CoreWindow^, VisibilityChangedEventArgs^>(this, &App::OnVisibilityChanged);

    DisplayProperties::LogicalDpiChanged +=
        ref new DisplayPropertiesEventHandler(this, &App::OnLogicalDpiChanged);

    m_controller->Initialize(window);
    m_controller->AutoFire(false);

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, window->Bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(window->Bounds.Width - GameConstants::TouchRectangleSize, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(window->Bounds.Width, window->Bounds.Height)
        );

    m_renderer->Initialize(window, m_mainPage->GetSwapChainBackgroundPanel(), DisplayProperties::LogicalDpi);
    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    SetAction(GameInfoOverlayCommand::None);
    ShowGameInfoOverlay();

    m_onRenderingEventToken = CompositionTarget::Rendering::add(ref new EventHandler<Object^>(this, &App::OnRendering));
    m_renderNeeded = true;

    create_task([this]()
    {
        // Asynchronously initialize the game class and load the renderer device resources.
        // This way, the game gets to its main loop faster 
        // and in parallel all the necessary resources are loaded on other threads.
        m_game->Initialize(m_controller, m_renderer);

        return m_renderer->CreateGameDeviceResourcesAsync(m_game);

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // as the m_renderer object was created because the D3D device context
        // can be accessed only on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeLicense();
        InitializeGameState();

        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // In the middle of a game, so spin up the async task to load the level.
            create_task([this]()
            {
                return m_game->LoadLevelAsync();

            }).then([this]()
            {
                // The m_game object may need to deal with D3D device context work so
                // again the finalize code needs to run in the same thread
                // context as the m_renderer object was created because the D3D 
                // device context can  be accessed only on a single thread.
                m_game->FinalizeLoadLevel();
                m_updateState = UpdateEngineState::ResourcesLoaded;

            }, task_continuation_context::use_current());
        }
    }, task_continuation_context::use_current());
}
```

구성된 스왑 체인을 XAML에서 정의한 [**SwapChainBackgroundPanel**](https://msdn.microsoft.com/library/windows/apps/hh702626) 패널 인스턴스에 연결하려면 기본 네이티브 [**ISwapChainBackgroundPanel**](https://msdn.microsoft.com/library/windows/desktop/hh848326) 인터페이스 구현에 대한 포인터를 가져오고 [**SetSwapChain**](https://msdn.microsoft.com/library/windows/desktop/hh848327)을 호출하여 구성된 스왑 체인에 전달해야 합니다. 특히 DirectX/XAML interop에서 파생된 **DirectXBase::CreateWindowSizeDependentResources** 메서드에서:

```cpp
        ComPtr<IDXGIDevice1> dxgiDevice;
        DX::ThrowIfFailed(
            m_d3dDevice.As(&dxgiDevice)
            );

        // Next, get the associated adapter from the DXGI Device.
        ComPtr<IDXGIAdapter> dxgiAdapter;
        DX::ThrowIfFailed(
            dxgiDevice->GetAdapter(&dxgiAdapter)
            );

        // Next, get the parent factory from the DXGI adapter.
        ComPtr<IDXGIFactory2> dxgiFactory;
        DX::ThrowIfFailed(
            dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
            );

        // Create the swap chain and then associate it with the SwapChainBackgroundPanel.
        DX::ThrowIfFailed(
            dxgiFactory->CreateSwapChainForComposition(
                m_d3dDevice.Get(),
                &swapChainDesc,
                nullptr,
                &m_swapChain
                )
            );

        ComPtr<ISwapChainBackgroundPanelNative> dxRootPanelAsNative;

        // Set the swap chain on the SwapChainBackgroundPanel.
        reinterpret_cast<IUnknown*>(m_swapChainPanel)->QueryInterface(__uuidof(ISwapChainBackgroundPanelNative), (void**)&dxRootPanelAsNative);

        DX::ThrowIfFailed(
            dxRootPanelAsNative->SetSwapChain(m_swapChain.Get())
            );

        DX::ThrowIfFailed(
            dxgiDevice->SetMaximumFrameLatency(1)
            );
```

이 프로세스에 대한 자세한 내용은 [DirectX 및 XAML 상호 운용](https://msdn.microsoft.com/library/windows/apps/hh825871)을 참조하세요.

## XAML 게임 샘플 XAML 코드 숨김에 대한 전체 코드


다음은 Direct3D 11.1 슈팅 게임 샘플의 XAML 버전에 있는 코드 숨김에 대한 전체 코드입니다.

나머지 항목에서 검토한 게임 샘플의 버전과 달리 XAML 버전은 각각 **DirectXApp.cpp** 및 **GameInfoOverlay.cpp**가 아닌 **App.xaml.cpp** 및 **MainPage.xaml.cpp** 파일에서 프레임워크를 정의합니다.

App.xaml.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include "MainPage.xaml.h"
#include "Simple3DGame.h"
#include "App.g.h"

namespace Simple3DGameXaml
{
    private enum class UpdateEngineState
    {
        Uninitialized,
        WaitingForResources,
        WaitingForPress,
        Dynamics,
        Snapped,
        Suspended,
        Deactivated,
    };

    private enum class PressResultState
    {
        LoadGame,
        PlayLevel,
        ContinueLevel,
    };

    private enum class GameInfoOverlayState
    {
        GameStats,
        GameOverExpired,
        GameOverCompleted,
        LevelStart,
        Pause,
        Snapped,
    };

    public ref class App sealed
    {
    public:
        App();

        virtual void OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ pArgs);

        void PauseRequested() { if (m_updateState == UpdateEngineState::Dynamics) m_pauseRequested = true; };
        void PressComplete()  { if (m_updateState == UpdateEngineState::WaitingForPress) m_pressComplete = true; };
        void ResetGame();

    private:
        ~App();

        void OnSuspending(
            _In_ Platform::Object^ sender, 
            _In_ Windows::ApplicationModel::SuspendingEventArgs^ args
            );
        void OnResuming(
            _In_ Platform::Object^ sender,
            _In_ Platform::Object^ args
            );

        void OnViewStateChanged(
            _In_ Windows::UI::ViewManagement::ApplicationView^ view,
            _In_ Windows::UI::ViewManagement::ApplicationViewStateChangedEventArgs^ args
            );

        void OnWindowActivationChanged(
            _In_ Platform::Object^ sender,
            _In_ Windows::UI::Core::WindowActivatedEventArgs^ args
            );

        void OnWindowSizeChanged(
            _In_ Windows::UI::Core::CoreWindow^ sender,
            _In_ Windows::UI::Core::WindowSizeChangedEventArgs^ args
            );

        void OnLogicalDpiChanged(
            _In_ Platform::Object^ sender
            );

        void OnRendering(
            _In_ Object^ sender, 
            _In_ Object^ args
            );

        void InitializeGameState();
        void Update();
        void SetGameInfoOverlay(GameInfoOverlayState state);
        void SetAction (GameInfoOverlayCommand command);
        void ShowGameInfoOverlay();
        void HideGameInfoOverlay();
        void SetSnapped();
        void HideSnapped();

        Windows::Foundation::EventRegistrationToken         m_eventToken;
        bool                                                m_pauseRequested;
        bool                                                m_pressComplete;
        bool                                                m_renderNeeded;
        bool                                                m_haveFocus;

        MainPage^                                           m_mainPage;
        Simple3DGame^                                       m_game;
        MoveLookController^                                 m_controller;           // Controller to handle user input

        UpdateEngineState                                   m_updateState;
        UpdateEngineState                                   m_updateStateNext;
        PressResultState                                    m_pressResult;
        GameInfoOverlayState                                m_gameInfoOverlayState;
    };
}
```

App.xaml.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "App.xaml.h"

using namespace Simple3DGameXaml;

using namespace Platform;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::ApplicationModel;
using namespace Windows::ApplicationModel::Core;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::UI::Core;
using namespace Windows::UI::ViewManagement;
using namespace Windows::Foundation;
using namespace Windows::Foundation::Collections;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::UI::Xaml::Controls::Primitives;
using namespace Windows::UI::Xaml::Data;
using namespace Windows::UI::Xaml::Input;
using namespace Windows::UI::Xaml::Media;
using namespace Windows::UI::Xaml::Navigation;
using namespace Windows::UI::Xaml::Media::Animation;
using namespace Windows::Graphics::Display;

//----------------------------------------------------------------------
App::App():
    m_pauseRequested(false),
    m_pressComplete(false),
    m_renderNeeded(false),
    m_haveFocus(false)
{
    InitializeComponent();
}
//----------------------------------------------------------------------
App::~App()
{
    CompositionTarget::Rendering::remove(m_eventToken);
}
//----------------------------------------------------------------------
void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ args)
{
    m_mainPage = ref new MainPage(this);

    Window::Current->Content = m_mainPage;
    Window::Current->Activated += ref new WindowActivatedEventHandler(this, &App::OnWindowActivationChanged);
    Window::Current->Activate();

    // Create the game and pass to window and root panel for swap chain setup.
    m_controller = ref new MoveLookController();
    m_controller->Initialize(Window::Current->CoreWindow);
    
    m_game = ref new Simple3DGame();
    m_game->Initialize(Window::Current->CoreWindow, m_mainPage, DisplayProperties::LogicalDpi, m_controller);

    m_eventToken = CompositionTarget::Rendering::add(ref new EventHandler<Object^>(this, &App::OnRendering));

    ApplicationView::GetForCurrentView()->ViewStateChanged +=
        ref new TypedEventHandler<ApplicationView^, ApplicationViewStateChangedEventArgs^>(
            this,
            &App::OnViewStateChanged
            );

    CoreApplication::Suspending += ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);
    CoreApplication::Resuming += ref new EventHandler<Object^>(this, &App::OnResuming);

    Window::Current->CoreWindow->SizeChanged += 
        ref new TypedEventHandler<CoreWindow^, WindowSizeChangedEventArgs^>(this, &App::OnWindowSizeChanged);

    DisplayProperties::LogicalDpiChanged +=
        ref new DisplayPropertiesEventHandler(this, &App::OnLogicalDpiChanged);

    InitializeGameState();
}
//----------------------------------------------------------------------
void App::OnRendering(
    _In_ Object^ sender, 
    _In_ Object^ args
    )
{
    Update();
    if (m_updateState == UpdateEngineState::Dynamics  || m_renderNeeded)
    {
        m_game->Render();
        m_renderNeeded = false;
    }
}
//--------------------------------------------------------------------------------------
void App::OnWindowSizeChanged(
    _In_ CoreWindow^ sender,
    _In_ WindowSizeChangedEventArgs^ args
    )
{
    m_renderNeeded = true;
    m_game->UpdateForWindowSizeChange();
}
//--------------------------------------------------------------------------------------
void App::OnLogicalDpiChanged(
    _In_ Object^ sender
    )
{
    m_game->SetDpi(DisplayProperties::LogicalDpi);
}
//--------------------------------------------------------------------------------------
void App::InitializeGameState()
{
    //
    // Set up the initial state machine for handling the Game playing state.
    //
    if (m_game->GameActive() && m_game->LevelActive())
    {
        // The last time the game terminated it was in the middle 
        // of a level. 
        // We are waiting for the user to continue the game.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::ContinueLevel;
        SetGameInfoOverlay(GameInfoOverlayState::Pause);
    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        // The last time the game terminated the game had been completed.
        // Show the high score.
        // We are waiting for the user to start a new game.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::LoadGame;
        SetGameInfoOverlay(GameInfoOverlayState::GameStats);
    }
    else
    {
        // This is either the first time the game has run or
        // the last time the game terminated the level was completed.
        // We are waiting for the user to begin the next level.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::PlayLevel;
        SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
    }
    SetAction(GameInfoOverlayCommand::PleaseWait);
    ShowGameInfoOverlay();
}
//--------------------------------------------------------------------------------------
void App::Update()
{
    m_controller->Update();

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        if (m_game->IsResourceLoadingComplete())
        {
            switch (m_pressResult)
            {
            case PressResultState::LoadGame:
                SetGameInfoOverlay(GameInfoOverlayState::GameStats);
                break;

            case PressResultState::PlayLevel:
                SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
                break;

            case PressResultState::ContinueLevel:
                SetGameInfoOverlay(GameInfoOverlayState::Pause);
                break;
            }
            m_updateState = UpdateEngineState::WaitingForPress;
            SetAction(GameInfoOverlayCommand::TapToContinue);
            m_controller->WaitForPress();
            ShowGameInfoOverlay();
            m_renderNeeded = true;
        }
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete() || m_pressComplete)
        {
            m_pressComplete = false;

            switch (m_pressResult)
            {
            case PressResultState::LoadGame:
                m_updateState = UpdateEngineState::WaitingForResources;
                m_pressResult = PressResultState::PlayLevel;
                m_controller->Active(false);
                m_game->LoadGame();
                SetAction(GameInfoOverlayCommand::PleaseWait);
                SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
                ShowGameInfoOverlay();
                break;

            case PressResultState::PlayLevel:
                m_updateState = UpdateEngineState::Dynamics;
                HideGameInfoOverlay();
                m_controller->Active(true);
                m_game->StartLevel();
                break;

            case PressResultState::ContinueLevel:
                m_updateState = UpdateEngineState::Dynamics;
                HideGameInfoOverlay();
                m_controller->Active(true);
                m_game->ContinueGame();
                break;
            }
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested() || m_pauseRequested)
        {
            m_pauseRequested = false;

            m_game->PauseGame();
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            SetAction(GameInfoOverlayCommand::TapToContinue);
            m_updateState = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            ShowGameInfoOverlay();
        }
        else 
        {
            GameState runState = m_game->RunGame();
            switch (runState)
            {
            case GameState::TimeExpired:
                SetAction(GameInfoOverlayCommand::TapToContinue);
                SetGameInfoOverlay(GameInfoOverlayState::GameOverExpired);
                ShowGameInfoOverlay();
                m_updateState = UpdateEngineState::WaitingForPress;
                m_pressResult = PressResultState::LoadGame;
                break;

            case GameState::LevelComplete:
                SetAction(GameInfoOverlayCommand::PleaseWait);
                SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
                ShowGameInfoOverlay();
                m_updateState = UpdateEngineState::WaitingForResources;
                m_pressResult = PressResultState::PlayLevel;
                break;

            case GameState::GameComplete:
                SetAction(GameInfoOverlayCommand::TapToContinue);
                SetGameInfoOverlay(GameInfoOverlayState::GameOverCompleted);
                ShowGameInfoOverlay();
                m_updateState  = UpdateEngineState::WaitingForPress;
                m_pressResult = PressResultState::LoadGame;
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // Transitioning state, so enable waiting for the press event
            m_controller->WaitForPress();
        }
        break;
    }
}
//--------------------------------------------------------------------------------------
void App::OnWindowActivationChanged(
    _In_ Platform::Object^ sender,
    _In_ Windows::UI::Core::WindowActivatedEventArgs^ args
    )
{
    if (args->WindowActivationState == CoreWindowActivationState::Deactivated)
    {
        m_haveFocus = false;

        switch (m_updateState)
        {
        case UpdateEngineState::Dynamics:
            // From Dynamic mode, when coming out of Deactivated rather than going directly back into game play
            // go to the paused state waiting for user input to continue.
            m_updateStateNext = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            ShowGameInfoOverlay();
            m_game->PauseGame();
            m_updateState = UpdateEngineState::Deactivated;
            SetAction(GameInfoOverlayCommand::None);
            m_renderNeeded = true;
            break;

        case UpdateEngineState::WaitingForResources:
        case UpdateEngineState::WaitingForPress:
            m_updateStateNext = m_updateState;
            m_updateState = UpdateEngineState::Deactivated;
            SetAction(GameInfoOverlayCommand::None);
            ShowGameInfoOverlay();
            m_renderNeeded = true;
            break;
        }
    }
    else if (args->WindowActivationState == CoreWindowActivationState::CodeActivated 
        || args->WindowActivationState == CoreWindowActivationState::PointerActivated)
    {
        m_haveFocus = true;

        if (m_updateState == UpdateEngineState::Deactivated)
        {
            m_updateState = m_updateStateNext;

            if (m_updateState == UpdateEngineState::WaitingForPress)
            {
                SetAction(GameInfoOverlayCommand::TapToContinue);
                m_controller->WaitForPress();
            }
            else if (m_updateStateNext == UpdateEngineState::WaitingForResources)
            {
                SetAction(GameInfoOverlayCommand::PleaseWait);
            }
        }
    }
}
//--------------------------------------------------------------------------------------
void App::OnSuspending(
    _In_ Platform::Object^ sender, 
    _In_ SuspendingEventArgs^ args
    )
{
    // Save application state.
    // If your application needs time to complete a lengthy operation, it can request a deferral.
    // The SuspendingOperation has a deadline time. Make sure all your operations are complete by that time!
    // If the app doesn't return from this handler within five seconds, it will be terminated.
    SuspendingOperation^ op = args->SuspendingOperation;
    SuspendingDeferral^ deferral = op->GetDeferral();

    switch (m_updateState)
    {
    case UpdateEngineState::Dynamics:
       // Game is in the active game play state, Stop Game Timer and Pause play and save state.
        SetAction(GameInfoOverlayCommand::None);
        SetGameInfoOverlay(GameInfoOverlayState::Pause);
        m_updateStateNext = UpdateEngineState::WaitingForPress;
        m_pressResult = PressResultState::ContinueLevel;
        m_game->PauseGame();
        break;

    case UpdateEngineState::WaitingForResources:
    case UpdateEngineState::WaitingForPress:
        m_updateStateNext = m_updateState;
        break;

    default:
        // Any other state don't save as next state as they are transient states and have already set m_updateStateNext
        break;
    }
    m_updateState = UpdateEngineState::Suspended;

    m_controller->Active(false);
    m_game->OnSuspending();

    deferral->Complete();
}
//--------------------------------------------------------------------------------------
void App::OnResuming(
    _In_ Platform::Object^ sender,
    _In_ Platform::Object^ args
    )
{
    if (m_haveFocus)
    {
        m_updateState = m_updateStateNext;
    }
    else
    {
        m_updateState = UpdateEngineState::Deactivated;
    }

    if (m_updateState == UpdateEngineState::WaitingForPress)
    {
        SetAction(GameInfoOverlayCommand::TapToContinue);
        m_controller->WaitForPress();
    }
    m_game->OnResuming();
    ShowGameInfoOverlay();
    m_renderNeeded = true;
}
//--------------------------------------------------------------------------------------
void App::OnViewStateChanged(
     _In_ ApplicationView^ view, 
     _In_ ApplicationViewStateChangedEventArgs^ args
     )
{
    m_renderNeeded = true;

    if (args->ViewState == ApplicationViewState::Snapped)
    {
        switch (m_updateState)
        {
        case UpdateEngineState::Dynamics:
            // From Dynamic mode, when coming out of SNAPPED layout rather than going directly back into game play
            // go to the paused state waiting for user input to continue.
            m_updateStateNext = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            SetAction(GameInfoOverlayCommand::TapToContinue);
            m_game->PauseGame();
            break;

        case UpdateEngineState::WaitingForResources:
        case UpdateEngineState::WaitingForPress:
            // Avoid corrupting the m_updateStateNext on a transition from Snapped -> Snapped.
            // Otherwise, just cache the current state and return to it when leaving SNAPPED layout.

            m_updateStateNext = m_updateState;
            break;

        default:
            break;
        }

        m_updateState = UpdateEngineState::Snapped;
        m_controller->Active(false);
        HideGameInfoOverlay();
        SetSnapped();
    }
    else if (args->ViewState == ApplicationViewState::Filled ||
        args->ViewState == ApplicationViewState::FullScreenLandscape ||
        args->ViewState == ApplicationViewState::FullScreenPortrait)
    {
        if (m_updateState == UpdateEngineState::Snapped)
        {

            HideSnapped();
            ShowGameInfoOverlay();
            m_renderNeeded = true;

            if (m_haveFocus)
            {
                if (m_updateStateNext == UpdateEngineState::WaitingForPress)
                {
                    SetAction(GameInfoOverlayCommand::TapToContinue);
                    m_controller->WaitForPress();
                }
                else if (m_updateStateNext == UpdateEngineState::WaitingForResources)
                {
                    SetAction(GameInfoOverlayCommand::PleaseWait);
                }

                m_updateState = m_updateStateNext;
            }
            else
            {
                m_updateState = UpdateEngineState::Deactivated;
                SetAction(GameInfoOverlayCommand::None);
            }
        }
    }
}
//--------------------------------------------------------------------------------------
void App::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::GameStats:
        m_mainPage->SetGameStats(
            m_game->HighScore().levelCompleted + 1,
            m_game->HighScore().totalHits,
            m_game->HighScore().totalShots
            );
        break;

    case GameInfoOverlayState::LevelStart:
        m_mainPage->SetLevelStart(
            m_game->LevelCompleted() + 1,
            m_game->CurrentLevel()->Objective(),
            m_game->CurrentLevel()->TimeLimit(),
            m_game->BonusTime()
            );
        break;

    case GameInfoOverlayState::GameOverCompleted:
        m_mainPage->SetGameOver(
            true, 
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::GameOverExpired:
        m_mainPage->SetGameOver(
            false, 
            m_game->LevelCompleted(),
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::Pause:
        m_mainPage->SetPause(
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->TimeRemaining()
            );
        break;
    }
}
//--------------------------------------------------------------------------------------
void App::SetAction(GameInfoOverlayCommand command)
{
    m_mainPage->SetAction(command);
}
//--------------------------------------------------------------------------------------
void App::ShowGameInfoOverlay()
{
    m_mainPage->ShowGameInfoOverlay();
}
//--------------------------------------------------------------------------------------
void App::HideGameInfoOverlay()
{
    m_mainPage->HideGameInfoOverlay();
}
//--------------------------------------------------------------------------------------
void App::SetSnapped()
{
    m_mainPage->SetSnapped();
}
//--------------------------------------------------------------------------------------
void App::HideSnapped()
{
    m_mainPage->HideSnapped();
}
//--------------------------------------------------------------------------------------
void App::ResetGame()
{
    m_updateState = UpdateEngineState::WaitingForResources;
    m_pressResult = PressResultState::PlayLevel;
    m_controller->Active(false);
    m_game->LoadGame();
    SetAction(GameInfoOverlayCommand::PleaseWait);
    SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
    ShowGameInfoOverlay();
    m_renderNeeded = true;
}
```

MainPage.xaml

```xaml
<SwapChainBackgroundPanel
    x:Name="DXSwapChainPanel"
    x:Class="Simple3DGameXaml.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignWidth="1366"
    d:DesignHeight="768">
  <UserControl x:Name="LayoutControl" Background="Transparent">
    <Grid x:Name="LayoutRoot">
      <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="GameInfoOverlayStates">
          <VisualState x:Name="NormalState">
            <Storyboard>
              <DoubleAnimation Storyboard.TargetName="GameInfoOverlay" Storyboard.TargetProperty="(UIElement.Opacity)" Duration="00:00:00.25" To="0">
                <DoubleAnimation.EasingFunction>
                  <CubicEase EasingMode="EaseIn" />
                </DoubleAnimation.EasingFunction>
              </DoubleAnimation>

              <ObjectAnimationUsingKeyFrames Storyboard.TargetProperty="Visibility" Storyboard.TargetName="GameInfoOverlay">
                <DiscreteObjectKeyFrame KeyTime="0:0:1" Value="Collapsed" />
              </ObjectAnimationUsingKeyFrames>
            </Storyboard>
          </VisualState>

          <VisualState x:Name="GameInfoOverlayState">
            <Storyboard>
              <DoubleAnimation Storyboard.TargetName="GameInfoOverlay" Storyboard.TargetProperty="(UIElement.Opacity)" Duration="00:00:00.25" To="1">
                <DoubleAnimation.EasingFunction>
                  <CubicEase EasingMode="EaseIn" />
                </DoubleAnimation.EasingFunction>
              </DoubleAnimation>

              <ObjectAnimationUsingKeyFrames Storyboard.TargetProperty="Visibility" Storyboard.TargetName="GameInfoOverlay">
                <DiscreteObjectKeyFrame KeyTime="0:0:0" Value="Visible" />
              </ObjectAnimationUsingKeyFrames>
            </Storyboard>
          </VisualState>

          <VisualState x:Name="SnappedState">
            <Storyboard>
              <DoubleAnimation Storyboard.TargetName="SDKHeader" Storyboard.TargetProperty="(UIElement.Opacity)" Duration="00:00:00.25" To="0">
                <DoubleAnimation.EasingFunction>
                  <CubicEase EasingMode="EaseIn" />
                </DoubleAnimation.EasingFunction>
              </DoubleAnimation>
              <DoubleAnimation Storyboard.TargetName="FullscreenView" Storyboard.TargetProperty="(UIElement.Opacity)" Duration="00:00:00.25" To="0">
                <DoubleAnimation.EasingFunction>
                  <CubicEase EasingMode="EaseIn" />
                </DoubleAnimation.EasingFunction>
              </DoubleAnimation>
              <DoubleAnimation Storyboard.TargetName="SnappedView" Storyboard.TargetProperty="(UIElement.Opacity)" Duration="00:00:00.25" To="1">
                <DoubleAnimation.EasingFunction>
                  <CubicEase EasingMode="EaseIn" />
                </DoubleAnimation.EasingFunction>
              </DoubleAnimation>

              <ObjectAnimationUsingKeyFrames Storyboard.TargetProperty="Visibility" Storyboard.TargetName="SDKHeader">
                <DiscreteObjectKeyFrame KeyTime="0:0:1" Value="Collapsed" />
              </ObjectAnimationUsingKeyFrames>
              <ObjectAnimationUsingKeyFrames Storyboard.TargetProperty="Visibility" Storyboard.TargetName="FullscreenView">
                <DiscreteObjectKeyFrame KeyTime="0:0:1" Value="Collapsed" />
              </ObjectAnimationUsingKeyFrames>
              <ObjectAnimationUsingKeyFrames Storyboard.TargetProperty="Visibility" Storyboard.TargetName="SnappedView">
                <DiscreteObjectKeyFrame KeyTime="0:0:0" Value="Visible" />
              </ObjectAnimationUsingKeyFrames>
            </Storyboard>
          </VisualState>

          <VisualState x:Name="UnsnappedState">
            <Storyboard>
              <DoubleAnimation Storyboard.TargetName="SDKHeader" Storyboard.TargetProperty="(UIElement.Opacity)" Duration="00:00:00.25" To="1">
                <DoubleAnimation.EasingFunction>
                  <CubicEase EasingMode="EaseIn" />
                </DoubleAnimation.EasingFunction>
              </DoubleAnimation>
              <DoubleAnimation Storyboard.TargetName="FullscreenView" Storyboard.TargetProperty="(UIElement.Opacity)" Duration="00:00:00.25" To="1">
                <DoubleAnimation.EasingFunction>
                  <CubicEase EasingMode="EaseIn" />
                </DoubleAnimation.EasingFunction>
              </DoubleAnimation>
              <DoubleAnimation Storyboard.TargetName="SnappedView" Storyboard.TargetProperty="(UIElement.Opacity)" Duration="00:00:00.25" To="0">
                <DoubleAnimation.EasingFunction>
                  <CubicEase EasingMode="EaseIn" />
                </DoubleAnimation.EasingFunction>
              </DoubleAnimation>

              <ObjectAnimationUsingKeyFrames Storyboard.TargetProperty="Visibility" Storyboard.TargetName="SDKHeader">
                <DiscreteObjectKeyFrame KeyTime="0:0:0" Value="Visible" />
              </ObjectAnimationUsingKeyFrames>
              <ObjectAnimationUsingKeyFrames Storyboard.TargetProperty="Visibility" Storyboard.TargetName="FullscreenView">
                <DiscreteObjectKeyFrame KeyTime="0:0:0" Value="Visible" />
              </ObjectAnimationUsingKeyFrames>
              <ObjectAnimationUsingKeyFrames Storyboard.TargetProperty="Visibility" Storyboard.TargetName="SnappedView">
                <DiscreteObjectKeyFrame KeyTime="0:0:1" Value="Collapsed" />
              </ObjectAnimationUsingKeyFrames>
            </Storyboard>
          </VisualState>
        </VisualStateGroup>
      </VisualStateManager.VisualStateGroups>
      <Grid x:Name="ContentRoot">
        <Grid.RowDefinitions>
          <RowDefinition Height="1*"/>
          <RowDefinition Height="2*"/>
          <RowDefinition Height="1*"/>
        </Grid.RowDefinitions>

        <!-- Sample Overlay Title -->
        <StackPanel x:Name="SDKHeader" Grid.Row="0">
          <StackPanel Orientation="Horizontal">
            <Image Source="windows-sdk.png"/>
            <TextBlock Text="Windows 8 SDK Samples" VerticalAlignment="Bottom" Style="{StaticResource OverlayTitleStyle}" TextWrapping="Wrap"/>
          </StackPanel>
          <TextBlock x:Name="FeatureName" Text="UWP DirectX/XAML first-person game sample" Style="{StaticResource OverlayH1Style}" TextWrapping="Wrap"/>
        </StackPanel>
        <!-- End Sample Overlay Title -->

        <Grid Grid.Row="1" x:Name="SnappedView" Background="{StaticResource PageBackgroundBrush}" Visibility="Collapsed">
          <Grid.RowDefinitions>
            <RowDefinition Height="2*"/>
            <RowDefinition Height="5*"/>
            <RowDefinition Height="1*"/>
          </Grid.RowDefinitions>

          <!-- Title of the Game Info Overlay -->
          <StackPanel Grid.Row="1" Orientation="Horizontal" HorizontalAlignment="Center">
            <TextBlock
              Text="Game Paused"
              Style="{StaticResource TitleStyle}"/>
          </StackPanel>
        </Grid>

        <Grid Grid.Row="1" x:Name="FullscreenView">
          <Grid.ColumnDefinitions>
            <ColumnDefinition Width="1*"/>
            <ColumnDefinition Width="2*"/>
            <ColumnDefinition Width="1*"/>
          </Grid.ColumnDefinitions>

          <!-- Center of the outer Grid.  This is the Center 50% of the screen -->
          <Grid x:Name="GameInfoOverlay" Grid.Column="1" Background="{StaticResource PageBackgroundBrush}" Visibility="Collapsed"
                Tapped="OnGameInfoOverlayTapped">
            <Grid.RowDefinitions>
              <RowDefinition Height="2*"/>
              <RowDefinition Height="5*"/>
              <RowDefinition Height="1*"/>
            </Grid.RowDefinitions>

            <!-- Title of the Game Info Overlay -->
            <StackPanel Grid.Row="0" Orientation="Horizontal" HorizontalAlignment="Center">
              <TextBlock x:Name="GameInfoOverlayTitle"
                Text="Title"
                Style="{StaticResource TitleStyle}"/>
            </StackPanel>

            <!-- Body1: Game Statistics -->
            <Grid x:Name="Stats" Grid.Row="1" Visibility="Collapsed">
              <Grid.ColumnDefinitions>
                <ColumnDefinition Width="Auto"/>
                <ColumnDefinition Width="*"/>
              </Grid.ColumnDefinitions>
              <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
              </Grid.RowDefinitions>

              <StackPanel Grid.Column="0" Grid.Row="0" Orientation="Horizontal">
                <TextBlock
                  Text="Levels Completed"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="0" Grid.Row="1" Orientation="Horizontal">
                <TextBlock
                  Text="Total Points"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="0" Grid.Row="2" Orientation="Horizontal">
                <TextBlock
                  Text="Total Shots"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel x:Name="HighScoreTitle" Grid.Column="0" Grid.Row="3" Orientation="Horizontal" Visibility="Visible">
                <TextBlock
                  Text="High Score"
                  Style="{StaticResource H1StyleSpace}"/>
              </StackPanel>
              <StackPanel Grid.Column="1" Grid.Row="0" Orientation="Horizontal">
                <TextBlock x:Name="LevelsCompleted"
                  Text="1"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="1" Grid.Row="1" Orientation="Horizontal">
                <TextBlock x:Name="TotalPoints"
                  Text="9"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="1" Grid.Row="2" Orientation="Horizontal">
                <TextBlock x:Name="TotalShots"
                  Text="25"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel x:Name="HighScoreData" Grid.Column="1" Grid.Row="3" Orientation="Horizontal">
                <TextBlock x:Name="HighScore"
                  Text="120"
                  Style="{StaticResource H1StyleSpace}"/>
              </StackPanel>
            </Grid>

            <!-- Body2: Level Start -->
            <Grid x:Name="LevelStart" Grid.Row="1" Visibility="Visible">
              <Grid.ColumnDefinitions>
                <ColumnDefinition Width="Auto"/>
                <ColumnDefinition Width="*"/>
              </Grid.ColumnDefinitions>
              <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
              </Grid.RowDefinitions>

              <StackPanel Grid.Column="0" Grid.Row="0" Orientation="Horizontal">
                <TextBlock
                  Text="Objective"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="0" Grid.Row="1" Orientation="Horizontal">
                <TextBlock
                  Text="Time Limit"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel x:Name="BonusTimeTitle" Grid.Column="0" Grid.Row="2" Orientation="Horizontal">
                <TextBlock
                  Text="Bonus Time"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <Grid Grid.Column="1" Grid.Row="0">
                <TextBlock x:Name="Objective"
                  Text="Objective Text - replaced before it is displayed"
                  TextWrapping="Wrap"
                  Style="{StaticResource H1Style}"/>
              </Grid>
              <StackPanel Grid.Column="1" Grid.Row="1" Orientation="Horizontal">
                <TextBlock x:Name="TimeLimit"
                  Text="30 sec"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel x:Name="BonusTimeData" Grid.Column="1" Grid.Row="2" Orientation="Horizontal">
                <TextBlock x:Name="BonusTime"
                  Text="20 sec"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
            </Grid>

            <!-- Body3: Pause -->
            <Grid x:Name="PauseData" Grid.Row="1" Visibility="Visible">
              <Grid.ColumnDefinitions>
                <ColumnDefinition Width="Auto"/>
                <ColumnDefinition Width="*"/>
              </Grid.ColumnDefinitions>
              <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
              </Grid.RowDefinitions>

              <StackPanel Grid.Column="0" Grid.Row="0" Orientation="Horizontal">
                <TextBlock
                  Text="Level"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="0" Grid.Row="1" Orientation="Horizontal">
                <TextBlock
                  Text="Hits"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="0" Grid.Row="2" Orientation="Horizontal">
                <TextBlock
                  Text="Shots"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="0" Grid.Row="3" Orientation="Horizontal">
                <TextBlock
                  Text="Time"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="1" Grid.Row="0" Orientation="Horizontal">
                <TextBlock x:Name="PauseLevel"
                  Text="1"
                  TextWrapping="Wrap"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="1" Grid.Row="1" Orientation="Horizontal">
                <TextBlock x:Name="PauseHits"
                  Text="0"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="1" Grid.Row="2" Orientation="Horizontal">
                <TextBlock x:Name="PauseShots"
                  Text="0"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="1" Grid.Row="3" Orientation="Horizontal">
                <TextBlock x:Name="PauseTimeRemaining"
                  Text="20.0 sec"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
            </Grid>

            <!-- Footer of Game Info Overlay.  There are several options -->
            <StackPanel x:Name="TapToContinue" Grid.Row="2" Orientation="Horizontal" Visibility="Collapsed">
              <TextBlock
                Text="Tap to continue ..."
                Style="{StaticResource H3Style}"
                TextWrapping="Wrap"/>
            </StackPanel>
            <StackPanel x:Name="PleaseWait" Grid.Row="2" Orientation="Horizontal" Visibility="Collapsed">
              <TextBlock
                Text="Level Loading Please Wait ..."
                Style="{StaticResource H3Style}"
                TextWrapping="Wrap"/>
            </StackPanel>
            <StackPanel x:Name="PlayAgain" Grid.Row="2" Orientation="Horizontal" Visibility="Collapsed">
              <TextBlock
                Text="Tap to play again ..."
                Style="{StaticResource H3Style}"
                TextWrapping="Wrap"/>
            </StackPanel>
          </Grid>
        </Grid>
      </Grid>
      <AppBar x:Name="GameAppBar"  Height="88" VerticalAlignment="Bottom">
        <Grid>
          <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="Auto"/>
          </Grid.ColumnDefinitions>
          <StackPanel Grid.Column="0" Orientation="Horizontal" HorizontalAlignment="Left" VerticalAlignment="Top">
            <Button x:Name="Reset" Tag="Reset" Style="{StaticResource ResetButtonStyle}" Click="OnResetButtonClicked"/>
          </StackPanel>
          <StackPanel Grid.Column="1" Orientation="Horizontal" HorizontalAlignment="Right" VerticalAlignment="Top">
            <Button x:Name="Pause" Tag="Pause" Style="{StaticResource PauseButtonStyle}" Click="OnPauseButtonClicked"/>
            <Button x:Name="Play"  Tag="Play"  Style="{StaticResource PlayButtonStyle}"  Click="OnPlayButtonClicked"/>
          </StackPanel>
        </Grid>
      </AppBar>
    </Grid>
  </UserControl>
</SwapChainBackgroundPanel>
```

MainPage.xaml.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include "MainPage.g.h"

namespace Simple3DGameXaml
{
    ref class App;

    public enum class GameInfoOverlayCommand
    {
        None,
        TapToContinue,
        PleaseWait,
        PlayAgain,
    };

    public ref class MainPage sealed
    {
    public:
        MainPage(App^ app);

        void SetGameStats(int maxLevel, int hitCount, int shotCount);
        void SetGameOver(bool win, int maxLevel, int hitCount, int shotCount, int highScore);
        void SetLevelStart(int level, Platform::String^ objective, float timeLimit, float bonusTime);
        void SetPause(int level, int hitCount, int shotCount, float timeRemaining);
        void SetSnapped();
        void HideSnapped();
        void SetAction(GameInfoOverlayCommand action);
        void HideGameInfoOverlay();
        void ShowGameInfoOverlay();

    protected:
        void OnPauseButtonClicked(Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e);
        void OnPlayButtonClicked(Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e);
        void OnResetButtonClicked(Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e);
        void OnGameInfoOverlayTapped(Object^ sender, Windows::UI::Xaml::Input::TappedRoutedEventArgs^ args);

    private:
        App^ m_app;
    };
}
```

Main.xaml.cpp 코드 숨김

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "App.xaml.h"
#include "MainPage.xaml.h"

using namespace Simple3DGameXaml;

using namespace Platform;
using namespace Windows::Foundation;
using namespace Windows::Foundation::Collections;
using namespace Windows::Graphics::Display;
using namespace Windows::UI::ViewManagement;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::UI::Xaml::Controls::Primitives;
using namespace Windows::UI::Xaml::Data;
using namespace Windows::UI::Xaml::Input;
using namespace Windows::UI::Xaml::Media;
using namespace Windows::UI::Xaml::Navigation;

//----------------------------------------------------------------------
MainPage::MainPage(App^ app)
{
    InitializeComponent();

    m_app = app;
}
//----------------------------------------------------------------------
void MainPage::HideGameInfoOverlay()
{
    VisualStateManager::GoToState(this->LayoutControl, ref new String(L"NormalState"), true);
}
//----------------------------------------------------------------------
void MainPage::ShowGameInfoOverlay()
{
    VisualStateManager::GoToState(this->LayoutControl, ref new String(L"GameInfoOverlayState"), true);
}
//----------------------------------------------------------------------
void MainPage::SetAction(GameInfoOverlayCommand action)
{
    // Enable only one of the four possible commands at the bottom of the
    // Game Info Overlay.

    PlayAgain->Visibility = ::Visibility::Collapsed;
    PleaseWait->Visibility = ::Visibility::Collapsed;
    TapToContinue->Visibility = ::Visibility::Collapsed;

    switch (action)
    {
    case GameInfoOverlayCommand::PlayAgain:
        PlayAgain->Visibility = ::Visibility::Visible;
        break;
    case GameInfoOverlayCommand::PleaseWait:
        PleaseWait->Visibility = ::Visibility::Visible;
        break;
    case GameInfoOverlayCommand::TapToContinue:
        TapToContinue->Visibility = ::Visibility::Visible;
        break;
    case GameInfoOverlayCommand::None:
        break;
    }
}
//----------------------------------------------------------------------
void MainPage::SetGameStats(
    int maxLevel, 
    int hitCount, 
    int shotCount
    )
{
    GameInfoOverlayTitle->Text = "Game Statistics";
    Stats->Visibility = ::Visibility::Visible;
    LevelStart->Visibility = ::Visibility::Collapsed;
    PauseData->Visibility = ::Visibility::Collapsed;

    static const int bufferLength = 20;
    static char16 wsbuffer[bufferLength];

    int length = swprintf_s(wsbuffer, bufferLength, L"%d", maxLevel);
    LevelsCompleted->Text = ref new Platform::String(wsbuffer, length);

    length = swprintf_s(wsbuffer, bufferLength, L"%d", hitCount);
    TotalPoints->Text = ref new Platform::String(wsbuffer, length);

    length = swprintf_s(wsbuffer, bufferLength, L"%d", shotCount);
    TotalShots->Text = ref new Platform::String(wsbuffer, length);

    // High Score is not used for showing Game Statistics
    HighScoreTitle->Visibility = ::Visibility::Collapsed;
    HighScoreData->Visibility  = ::Visibility::Collapsed;
}
//----------------------------------------------------------------------
void MainPage::SetGameOver(
    bool win, 
    int maxLevel, 
    int hitCount, 
    int shotCount, 
    int highScore
    )
{
    if (win)
    {
        GameInfoOverlayTitle->Text = "You Won!";
    }
    else
    {
        GameInfoOverlayTitle->Text = "Game Over";
    }
    Stats->Visibility = ::Visibility::Visible;
    LevelStart->Visibility = ::Visibility::Collapsed;
    PauseData->Visibility = ::Visibility::Collapsed;

    static const int bufferLength = 20;
    static char16 wsbuffer[bufferLength];

    int length = swprintf_s(wsbuffer, bufferLength, L"%d", maxLevel);
    LevelsCompleted->Text = ref new Platform::String(wsbuffer, length);

    length = swprintf_s(wsbuffer, bufferLength, L"%d", hitCount);
    TotalPoints->Text = ref new Platform::String(wsbuffer, length);

    length = swprintf_s(wsbuffer, bufferLength, L"%d", shotCount);
    TotalShots->Text = ref new Platform::String(wsbuffer, length);

    // Show High Score
    HighScoreTitle->Visibility = ::Visibility::Visible;
    HighScoreData->Visibility  = ::Visibility::Visible;
    length = swprintf_s(wsbuffer, bufferLength, L"%d", highScore);
    HighScore->Text = ref new Platform::String(wsbuffer, length);
}
//----------------------------------------------------------------------
void MainPage::SetLevelStart(
    int level, 
    Platform::String^ objective, 
    float timeLimit, 
    float bonusTime
    )
{
    static const int bufferLength = 20;
    static char16 wsbuffer[bufferLength];

    int length = swprintf_s(wsbuffer, bufferLength, L"Level %d", level);
    GameInfoOverlayTitle->Text = ref new Platform::String(wsbuffer, length);

    Stats->Visibility = ::Visibility::Collapsed;
    LevelStart->Visibility = ::Visibility::Visible;
    PauseData->Visibility = ::Visibility::Collapsed;

    Objective->Text = objective;

    length = swprintf_s(wsbuffer, bufferLength, L"%6.1f sec", timeLimit);
    TimeLimit->Text = ref new Platform::String(wsbuffer, length);

    if (bonusTime > 0.0)
    {
        BonusTimeTitle->Visibility = ::Visibility::Visible;
        BonusTimeData->Visibility  = ::Visibility::Visible;
        length = swprintf_s(wsbuffer, bufferLength, L"%6.1f sec", bonusTime);
        BonusTime->Text = ref new Platform::String(wsbuffer, length);
    }
    else
    {
        BonusTimeTitle->Visibility = ::Visibility::Collapsed;
        BonusTimeData->Visibility  = ::Visibility::Collapsed;  
    }
}
//----------------------------------------------------------------------
void MainPage::SetPause(int level, int hitCount, int shotCount, float timeRemaining)
{
    GameInfoOverlayTitle->Text = "Paused";
    Stats->Visibility = ::Visibility::Collapsed;
    LevelStart->Visibility = ::Visibility::Collapsed;
    PauseData->Visibility = ::Visibility::Visible;

    static const int bufferLength = 20;
    static char16 wsbuffer[bufferLength];

    int length = swprintf_s(wsbuffer, bufferLength, L"%d", level);
    PauseLevel->Text = ref new Platform::String(wsbuffer, length);

    length = swprintf_s(wsbuffer, bufferLength, L"%d", hitCount);
    PauseHits->Text = ref new Platform::String(wsbuffer, length);

    length = swprintf_s(wsbuffer, bufferLength, L"%d", shotCount);
    PauseShots->Text = ref new Platform::String(wsbuffer, length);

    length = swprintf_s(wsbuffer, bufferLength, L"%6.1f sec", timeRemaining);
    PauseTimeRemaining->Text = ref new Platform::String(wsbuffer, length);
}
//----------------------------------------------------------------------
void MainPage::SetSnapped()
{
    VisualStateManager::GoToState(this->LayoutControl, ref new String(L"SnappedState"), true);
}
//----------------------------------------------------------------------
void MainPage::HideSnapped()
{
    VisualStateManager::GoToState(this->LayoutControl, ref new String(L"UnsnappedState"), true);
}
//----------------------------------------------------------------------
void MainPage::OnGameInfoOverlayTapped(Object^ sender, TappedRoutedEventArgs^ args)
{
    m_app->PressComplete();
}
//----------------------------------------------------------------------
void MainPage::OnPauseButtonClicked(Object^ sender, RoutedEventArgs^ args)
{
    m_app->PauseRequested();
}
//----------------------------------------------------------------------
void MainPage::OnPlayButtonClicked(Object^ sender, RoutedEventArgs^ args)
{
    m_app->PressComplete();
}
//----------------------------------------------------------------------
void MainPage::OnResetButtonClicked(Object^ sender, RoutedEventArgs^ args)
{
    m_app->ResetGame();
}
//----------------------------------------------------------------------
```

오베레이에 XAML을 사용하는 샘플 게임 버전을 다운로드하려면 [Direct3D 슈팅 게임 샘플(XAML)](http://go.microsoft.com/fwlink/p/?linkid=241418)(영문)으로 이동하세요.

 

 






<!--HONumber=Mar16_HO1-->


