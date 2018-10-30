---
author: joannaleecy
title: 게임의 UWP 앱 프레임워크 정의
description: DirectX로 작성된 UWP(유니버설 Windows 플랫폼) 게임 코딩의 첫 번째 부분은 게임 개체가 Windows와 조작할 수 있도록 하는 프레임워크의 구축입니다.
ms.assetid: 7beac1eb-ba3d-e15c-44a1-da2f5a79bb3b
ms.author: joanlee
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, uwp, 게임, directx
ms.localizationpriority: medium
ms.openlocfilehash: 3444c71b4e4c610be0b7d92ac6d761340c5dd5c2
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5764949"
---
#  <a name="define-the-uwp-app-framework"></a>UWP 앱 프레임워크 정의

프레임 워크 창 포커스 대상에서 변경 처리 및 끌기 일시 중단 다시 시작 이벤트와 같은 Windows 런타임 속성 포함 하 여 Windows와 상호 작용 게임 개체를 작성 합니다.

이 프레임워크를 설정하려면 먼저 앱 단일 항목(실행 중인 앱의 인스턴스를 정의하는 Windows 런타임 개체)에서 필요한 그래픽 리소스에 액세스할 수 있도록 뷰 공급자를 가져와야 합니다. 또한 필요한 리소스와 리소스 처리 방법을 지정할 수 있도록 Windows 런타임을 통해 게임이 그래픽 인터페이스에 직접 연결됩니다.

뷰 공급자 개체는 __IFrameworkView__ 인터페이스를 구현하는데, 이 인터페이스는 이 게임 샘플을 생성하기 위해 구성해야 하는 일련의 메서드로 이루어져 있습니다.

앱 단일 항목이 호출하는 이러한 5가지 메서드를 반드시 구현해야 합니다.
* [__Initialize__](#initialize-the-view-provider)
* [__SetWindow__](#configure-the-window-and-display-behavior)
* [__Load__](#load-method-of-the-view-provider)
* [__Run__](#run-method-of-the-view-provider)
* [__Uninitialize__](#uninitialize-method-of-the-view-provider)

__Initialize__ 메서드는 응용 프로그램 시작 시 호출됩니다. __Initialize__에 이어 __SetWindow__ 메서드가 호출됩니다. 그런 다음 __Load__ 메서드가 호출됩니다. 게임이 실행 중일 때 __Run__ 메시지가 호출됩니다. 게임이 종료되면 __Uninitialize__ 메서드가 호출됩니다. 자세한 내용은 [__IFrameworkView__ API 참조](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview)를 참조하세요. 

>[!Note]
>이 샘플의 최신 게임 코드를 다운로드하지 않은 경우 [Direct3D 게임 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)로 이동합니다. 이 샘플은 UWP 기능 샘플의 큰 컬렉션의 일부입니다. 샘플을 다운로드하는 방법에 대한 지침은 [GitHub에서 UWP 샘플 가져오기](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)를 참조하세요.

## <a name="objective"></a>목표

UWP(유니버설 Windows 플랫폼) DirectX 게임을 위한 프레임워크를 설정하고 전체적인 게임 흐름을 정의하는 상태 시스템을 구현합니다.

## <a name="define-the-view-provider-factory-and-view-provider-object"></a>뷰 공급자 팩터리와 뷰 공급자 개체를 정의

__App.cpp__에서 __주__ 루프를 살펴보겠습니다. 

이 단계에서 뷰에 대한 팩터리를 생성하면(__IFrameworkViewSource__ 구현), 이 뷰를 정의하는 뷰 공급자 개체의 인스턴스가 생성됩니다(__IFrameworkView__ 구현).

### <a name="main-method"></a>주 메서드

GitHub 샘플 코드가 로드되면 __DirectXApplicationSource__를 새로 생성합니다 (원본 DirectX 템플릿을 사용하고 있는 경우에는 __Direct3DApplicationSource__ 사용). 이것은 __IFrameworkViewSource__를 구현하는 뷰 공급자 팩터리입니다. 뷰 공급자 팩터리의 __IFrameworkViewSource__ 인터페이스에는 단일 메서드인 __CreateView__가 정의되어 있습니다.

__CoreApplication::Run__에서 __Direct3DApplicationSource__ 또는 __DirectXApplicationSource__가 전달되면 앱 단일 항목에서 __CreateView__ 메서드가 호출됩니다.

__CreateView__는 __IFrameworkView__를 구현하는 앱 개체(이 샘플에서는 __앱__ 클래스 개체)의 새 인스턴스에 대한 참조를 반환합니다. __앱__ 클래스 개체는 뷰 공급자 개체입니다.

```cpp
// The main function is only used to initialize our IFrameworkView class.
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto directXApplicationSource = ref new DirectXApplicationSource();
    CoreApplication::Run(directXApplicationSource);
    return 0;
}

//--------------------------------------------------------------------------------------

IFrameworkView^ DirectXApplicationSource::CreateView()
{
    return ref new App();
}
```

## <a name="initialize-the-view-provider"></a>뷰 공급자 초기화

뷰 공급자 개체가 생성되고 나면 응용 프로그램 시작 시 앱 단일 항목이 [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) 메서드를 호출합니다. 따라서 주 창의 활성화를 처리하고 게임에서 갑작스러운 일시 중단(및 나중에 다시 시작 가능) 이벤트를 처리하도록 하는 등 UWP 게임의 기본 동작 대부분을 이 메서드에서 처리한다는 점은 매우 중요합니다.

이제 게임 앱은 일시 중지(또는 재개) 메시지를 처리할 수 있습니다. 그러나 여전히 작업할 창이 없고 게임이 초기화되지 않습니다. 따라서 몇 가지 작업을 더 수행해야 합니다.

### <a name="appinitialize-method"></a>App::Initialize 메서드

이 메서드에서 게임을 활성화, 일시 중지 및 재개할 수 있도록 다양한 이벤트 처리기를 생성합니다.

디바이스 리소스에 대한 액세스 권한을 얻습니다. __make_shared__ 함수는 메모리 리소스가 처음으로 생성될 때 __shared_ptr__을 생성하는 데 사용됩니다. __make_shared__를 사용하면 예외로부터 안전하다는 장점이 있습니다. 또한 동일한 호출을 사용하여 제어 블록 및 리소스를 위한 메모리를 할당하기 때문에 빌드 오버헤드를 줄일 수 있습니다.

```cpp
void App::Initialize(
    CoreApplicationView^ applicationView
    )
{
    // Register event handlers for app lifecycle. This example includes Activated, so that we
    // can make the CoreWindow active and start rendering on the window.
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);

    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

## <a name="configure-the-window-and-display-behaviors"></a>창과 디스플레이 동작 구성

이제 [__SetWindow__](https://msdn.microsoft.com/library/windows/apps/hh700509) 구현에 대해 살펴보겠습니다. __SetWindow__ 메서드에서 창과 디스플레이 동작을 구성합니다.

### <a name="appsetwindow-method"></a>App::SetWindow 메서드

앱 단일 항목은 게임의 주 창을 표시하고 리소스와 이벤트를 게임에서 사용할 수 있도록 해주는 [__CoreWindow__](https://msdn.microsoft.com/library/windows/apps/br208225) 개체를 제공합니다. 이제 작업할 수 있는 창이 생겼기 때문에 게임의 기본적인 UI 구성 요소 및 이벤트에서 추가를 시작할 수 있습니다.

그런 다음, 마우스 컨트롤과 터치 컨트롤 모두에서 사용할 수 있는 __CoreCursor__ 메서드를 사용하여 포인터를 생성합니다.

마지막으로 창 크기 조정, 닫기 및 DPI 변경(디스플레이 디바이스가 변경되는 경우)을 위한 기본 이벤트를 생성합니다. 이벤트에 대한 자세한 내용은 [이벤트 처리](tutorial-game-flow-management.md#events-handling)를 참조하세요.

```cpp
void App::SetWindow(
    CoreWindow^ window
    )
{
    // Codes added to modify the original DirectX template project
    window->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);

    PointerVisualizationSettings^ visualizationSettings = PointerVisualizationSettings::GetForCurrentView();
    visualizationSettings->IsContactFeedbackEnabled = false;
    visualizationSettings->IsBarrelButtonFeedbackEnabled = false;
    // --end of codes added
    
    m_deviceResources->SetWindow(window);

    window->Activated +=
        ref new TypedEventHandler<CoreWindow^, WindowActivatedEventArgs^>(this, &App::OnWindowActivationChanged);

    window->SizeChanged +=
        ref new TypedEventHandler<CoreWindow^, WindowSizeChangedEventArgs^>(this, &App::OnWindowSizeChanged);

    window->VisibilityChanged +=
        ref new TypedEventHandler<CoreWindow^, VisibilityChangedEventArgs^>(this, &App::OnVisibilityChanged);
        
    window->Closed +=
        ref new TypedEventHandler<CoreWindow^, CoreWindowEventArgs^>(this, &App::OnWindowClosed);

    DisplayInformation^ currentDisplayInformation = DisplayInformation::GetForCurrentView();

    currentDisplayInformation->DpiChanged +=
        ref new TypedEventHandler<DisplayInformation^, Platform::Object^>(this, &App::OnDpiChanged);

    currentDisplayInformation->OrientationChanged +=
        ref new TypedEventHandler<DisplayInformation^, Object^>(this, &App::OnOrientationChanged);
    
    // Codes added to modify the original DirectX template project
    currentDisplayInformation->StereoEnabledChanged +=
        ref new TypedEventHandler<DisplayInformation^, Platform::Object^>(this, &App::OnStereoEnabledChanged);
    // --end of codes added
    
    DisplayInformation::DisplayContentsInvalidated +=
        ref new TypedEventHandler<DisplayInformation^, Platform::Object^>(this, &App::OnDisplayContentsInvalidated);
}
```

## <a name="load-method-of-the-view-provider"></a>뷰 공급자의 Load 메서드

주 창이 설정되면 앱 단일 항목에서 [__Load__](https://msdn.microsoft.com/library/windows/apps/hh700501)를 호출합니다. 이 메서드에서는 일련의 비동기 작업을 사용하여 게임 개체를 생성하고 그래픽 리소스를 로드하며 게임의 상태 시스템을 초기화합니다. 게임 데이터나 자산을 미리 가져오려면 **SetWindow**나 **Initialize**가 아닌 이 메서드를 사용하는 것이 좋습니다. 

Windows는 비동기 작업 패턴을 사용하여 입력 처리를 시작하기 앞서 게임을 할 수 있는 시간에 제한을 두기 때문에 __Load__ 메서드가 신속하게 완료되어 입력 처리를 시작할 수 있도록 설계해야 합니다. 리소스가 많아서 로드에 많은 시간이 걸리는 경우, 정기적으로 업데이트되는 진행률 표시줄을 사용자에게 제공합니다. 이 메서드는 게임 시작에 앞서 시작 상태나 전역 값을 설정하는 등 필요한 준비를 수행하는 데도 사용됩니다.

비동기 프로그래밍 및 작업 병렬 처리 개념이 처음인 사용자는 [C++이 비동기 프로그래밍](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)으로 이동하세요.

### <a name="appload-method"></a>App::Load 메서드

로드 작업이 포함된 __GameMain__ 클래스를 생성합니다.

```cpp
void App::Load(
    Platform::String^ entryPoint
    )
{
        if (!m_main)
    {
        m_main = std::unique_ptr<GameMain>(new GameMain(m_deviceResources));
    }
}
````

### <a name="gamemain-constructor"></a>GameMain 생성자

* 게임 렌더러를 생성 및 초기화합니다. 자세한 내용은 [렌더링 프레임워크 I: 렌더링 소개](tutorial--assembling-the-rendering-pipeline.md)를 참조하세요.
* Simple3Dgame 개체를 생성 및 초기화합니다. 자세한 내용은 [주 게임 개체 정의](tutorial--defining-the-main-game-loop.md)를 참조하세요.    
* 게임 UI 컨트롤 개체를 생성하고 게임 정보 오버레이를 표시하여 리소스 파일이 로드됨에 따라 진행률 표시줄을 보여줍니다. 자세한 내용은 [사용자 인터페이스 추가](tutorial--adding-a-user-interface.md)를 참조하세요.
* 컨트롤러(터치, 마우스 또는 XBox 무선 컨트롤러)에서 입력을 읽을 수 있도록 컨트롤러를 생성합니다. 자세한 내용은 [컨트롤 추가](tutorial--adding-controls.md)를 참조하세요.
* 컨트롤러를 초기화한 후에는 화면의 왼쪽 아래와 오른쪽 아래에 각각 이동 및 카메라 터치 컨트롤을 위해 2개의 사각형 영역을 정의합니다. 플레이어는 **SetMoveRect** 호출로 정의된 왼쪽 아래 사각형을 카메라를 전후좌우로 이동하기 위한 가상 컨트롤 패드로 사용합니다. **SetFireRect** 메서드에서 정의된 오른쪽 아래 사각형은 탄약을 발사하는 가상 단추로 사용됩니다.
* __create_task__ 및 __create_task::then__을 사용하여 리소스 로드를 2개의 개별 단계로 분리합니다. Direct3D 11 디바이스에 대한 액세스는 스레드로 제한이 되기 때문에 개체 생성을 위한 Direct3D 11 디바이스 액세스가 자유 스레드 방식으로 수행되는 동안 디바이스 컨텍스트가 생성됩니다. 따라서 원래 스레드에서 실행되는 완성 작업(*FinalizeCreateGameDeviceResources*)과는 별도의 스레드에서 **CreateGameDeviceResourcesAsync** 작업을 실행할 수 있습니다. **LoadLevelAsync** 및 **FinalizeLoadLevel**을 사용하는 로드 수준 리소스에서는 비슷한 패턴을 사용합니다.

```cpp
GameMain::GameMain(const std::shared_ptr<DX::DeviceResources>& deviceResources) :
    m_deviceResources(deviceResources),
    m_windowClosed(false),
    m_haveFocus(false),
    m_gameInfoOverlayCommand(GameInfoOverlayCommand::None),
    m_visible(true),
    m_loadingCount(0),
    m_updateState(UpdateEngineState::WaitingForResources)
{
    m_deviceResources->RegisterDeviceNotify(this);

    m_renderer = ref new GameRenderer(m_deviceResources);
    m_game = ref new Simple3DGame();

    m_uiControl = m_renderer->GameUIControl();

    m_controller = ref new MoveLookController(CoreWindow::GetForCurrentThread());

    auto bounds = m_deviceResources->GetLogicalSize();

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(bounds.Width - GameConstants::TouchRectangleSize, bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(bounds.Width, bounds.Height)
        );

    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    m_uiControl->SetAction(GameInfoOverlayCommand::None);
    m_uiControl->ShowGameInfoOverlay();

    create_task([this]()
    {
        // Asynchronously initialize the game class and load the renderer device resources.
        // By doing all this asynchronously, the game gets to its main loop more quickly
        // and in parallel all the necessary resources are loaded on other threads.
        m_game->Initialize(m_controller, m_renderer);

        return m_renderer->CreateGameDeviceResourcesAsync(m_game);

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // as the m_renderer object was created because the D3D device context
        // can ONLY be accessed on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState();

        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // In the middle of a game so spin up the async task to load the level.
            return m_game->LoadLevelAsync().then([this]()
            {
                // The m_game object may need to deal with D3D device context work so
                // again the finalize code needs to run in the same thread
                // context as the m_renderer object was created because the D3D
                // device context can ONLY be accessed on a single thread.
                m_game->FinalizeLoadLevel();
                m_game->SetCurrentLevelToSavedState();
                m_updateState = UpdateEngineState::ResourcesLoaded;

            }, task_continuation_context::use_current());
        }
        else
        {
            // The game is not in the middle of a level so there aren't any level
            // resources to load.  Creating a no-op task so that in both cases
            // the same continuation logic is used.
            return create_task([]()
            {
            });
        }
    }, task_continuation_context::use_current()).then([this]()
    {
        // Since Game loading is an async task, the app visual state
        // may be too small or not have focus.  Put the state machine
        // into the correct state to reflect these cases.

        if (m_deviceResources->GetLogicalSize().Width < GameConstants::MinPlayableWidth)
        {
            m_updateStateNext = m_updateState;
            m_updateState = UpdateEngineState::TooSmall;
            m_controller->Active(false);
            m_uiControl->HideGameInfoOverlay();
            m_uiControl->ShowTooSmall();
            m_renderNeeded = true;
        }
        else if (!m_haveFocus)
        {
            m_updateStateNext = m_updateState;
            m_updateState = UpdateEngineState::Deactivated;
            m_controller->Active(false);
            m_uiControl->SetAction(GameInfoOverlayCommand::None);
            m_renderNeeded = true;
        }
    }, task_continuation_context::use_current());
}
```

## <a name="run-method-of-the-view-provider"></a>뷰 공급자의 Run 메서드

이전의 3가지 메서드 __Initialize__, __SetWindow__, __Load__는 스테이지를 설정했습니다. 이제 게임에서 **Run** 메서드를 실행하므로 즐기시면 됩니다! 게임 상태의 전환에 사용되는 이벤트는 디스패치 및 처리됩니다. 그래픽이 게임 루프 주기로 업데이트 됩니다.

### <a name="apprun"></a>App::Run

플레이어가 게임 창을 닫으면 종료되는 __while__ 루프를 시작합니다.

이 샘플 코드는 게임 엔진 상태 시스템에서 다음 두 상태 중 하나로 전환됩니다.
    * __Deactivated__: 게임 창이 비활성화(포커스를 잃음)되거나 사이드 창이 됩니다. 이 경우 게임이 이벤트 처리를 일시 중단하고 창이 포커스를 얻거나 비사이드될 때까지 기다립니다.
    * __TooSmall__: 게임이 고유한 상태를 업데이트하고 표시할 그래픽을 렌더링합니다.

게임에 포커스가 있으면 메시지가 도착할 때 메시지 큐의 모든 이벤트를 처리해야 하므로 **ProcessAllIfPresent** 옵션을 사용하여 [**CoreWindowDispatch.ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215)를 호출해야 합니다. 다른 옵션을 사용하면 메시지 이벤트 처리가 지연되어 게임이 응답하지 않는 것처럼 보이거나 터치 동작이 느리고 "고정"되지 않는 것처럼 느끼게 됩니다.

게임이 표시되지 않거나, 일시 중단되거나, 사이드 상태가 된 경우에는 도착하지 않는 메시지를 계속 디스패치하느라 리소스가 소비되는 일이 없어야 합니다. 이 경우에는 게임에서 **ProcessOneAndAllPending**을 사용하여 이벤트를 받을 때까지 차단하고, 해당 이벤트와 첫 번째 이벤트 처리 중 프로세스 큐에 도착하는 다른 이벤트를 처리합니다. 그러면 큐가 처리된 후 바로 [**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215)가 반환됩니다.

```cpp
void App::Run()
{
    m_main->Run();
}

void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            switch (m_updateState)
            {
            case UpdateEngineState::Deactivated:
            case UpdateEngineState::TooSmall:
                if (m_updateStateNext == UpdateEngineState::WaitingForResources)
                {
                    WaitingForResourceLoading();
                    m_renderNeeded = true;
                }
                else if (m_updateStateNext == UpdateEngineState::ResourcesLoaded)
                {
                    // In the device lost case, we transition to the final waiting state
                    // and make sure the display is updated.
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
                    m_updateStateNext = UpdateEngineState::WaitingForPress;
                    m_uiControl->ShowGameInfoOverlay();
                    m_renderNeeded = true;
                }

                if (!m_renderNeeded)
                {
                    // The App is not currently the active window and not in a transient state so just wait for events.
                    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // otherwise fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // exiting due to window close.  Make sure to save state.
}
```

## <a name="uninitialize-method-of-the-view-provider"></a>뷰 공급자의 Uninitialize 메서드

사용자가 결국 게임 세션을 종료하면 정리가 필요합니다. 이 때 **Uninitialize**가 필요하게 됩니다.

Windows10, 앱 창을 닫아도 앱의 프로세스가 하지만 대신 앱 단일 항목의 상태를 메모리에 기록 합니다. 시스템에서 이 메모리를 확보해야 할 때 리소스를 특별 정리하는 등 특별한 조치가 필요한 경우에는 이 메서드에 해당 정리를 위한 코드를 포함시킵니다.

### <a name="app-uninitialize"></a>App:: Uninitialize

```cpp
void App::Uninitialize()
{
}
```

## <a name="tips"></a>팁

게임을 직접 개발하는 경우 이러한 메서드를 기준으로 시작 코드를 디자인합니다. 다음에는 각 메서드에 대한 기본 제한 사항이 간략히 나와 있습니다.

-   **Initialize**를 사용하여 기본 클래스를 할당하고 기본 이벤트 처리기를 연결합니다.
-   **SetWindow**를 사용하여 주 창을 만들고 창 관련 이벤트를 연결합니다.
-   **Load**를 사용하여 나머지 설정을 처리하고 개체의 비동기 만들기 및 리소스 로드를 시작합니다. 단계적으로 생성되는 자산과 같은 임시 파일이나 데이터를 만들어야 하는 경우에도 이 메서드를 사용합니다.


## <a name="next-steps"></a>다음 단계

이제 DirectX와 UWP 게임의 기본 구조를 설명하겠습니다. 이 연습의 다른 부분에서 이를 참조하게 될 것으므로 이러한 5가지 메서드를 기억하세요. 그런 다음, [흐름 관리 게임](tutorial-game-flow-management.md)으로 가서 게임이 계속될 수 있도록 게임 상태와 이벤트 처리를 관리하는 방법을 심층적으로 살펴보겠습니다.



