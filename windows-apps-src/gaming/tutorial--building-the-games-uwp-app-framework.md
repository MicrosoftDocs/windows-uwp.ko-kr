---
title: 게임의 UWP 앱 프레임워크 정의
description: UWP (유니버설 Windows 플랫폼) 게임을 코딩 하는 첫 번째 단계는 앱 개체가 Windows와 상호 작용할 수 있도록 하는 프레임 워크를 구축 하는 것입니다.
ms.assetid: 7beac1eb-ba3d-e15c-44a1-da2f5a79bb3b
ms.date: 06/24/2020
ms.topic: article
keywords: Windows 10, UWP, 게임, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 2d762aebeaa5c97c1b23a91f0765cb5c09a91b46
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163057"
---
#  <a name="define-the-games-uwp-app-framework"></a>게임의 UWP 앱 프레임워크 정의

> [!NOTE]
> 이 항목은 DirectX 자습서 시리즈 [를 사용 하 여 단순 유니버설 Windows 플랫폼 (UWP) 게임 만들기](tutorial--create-your-first-uwp-directx-game.md) 의 일부입니다. 해당 링크의 항목은 계열의 컨텍스트를 설정 합니다.

UWP (유니버설 Windows 플랫폼) 게임을 코딩 하는 첫 번째 단계는 응용 프로그램 개체가 Windows와 상호 작용할 수 있도록 하는 프레임 워크를 빌드하는 것입니다 .이 프레임 워크는 일시 중단 다시 시작 이벤트 처리, 창 포커스의 변경, 맞추기 등의 Windows 런타임 기능을 포함 합니다.

## <a name="objectives"></a>목표

* UWP (유니버설 Windows 플랫폼) DirectX 게임의 프레임 워크를 설정 하 고 전체 게임 흐름을 정의 하는 상태 시스템을 구현 합니다.

> [!NOTE]
> 이 항목의 작업을 수행 하려면 다운로드 한 [Simple3DGameDX](/samples/microsoft/windows-universal-samples/simple3dgamedx/) 샘플 게임의 소스 코드를 확인 합니다.

## <a name="introduction"></a>소개

[게임 프로젝트 설정](tutorial--setting-up-the-games-infrastructure.md) 항목에서 [**IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) 및 [**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) 인터페이스 뿐만 아니라 **wwinmain** 함수를 도입 했습니다. Simple3DGameDX 프로젝트의 소스 코드 파일에 정의 된 것 처럼 볼 수 있는 **App** 클래스는 `App.cpp` *뷰 공급자 팩터리* 및 *뷰 공급자로*사용 됩니다. **Simple3DGameDX**

이 항목에서 선택 하 고 게임의 **앱** 클래스에서 **IFrameworkView**의 메서드를 구현 하는 방법에 대 한 자세한 정보를 살펴보겠습니다.

## <a name="the-appinitialize-method"></a>App:: Initialize 메서드

응용 프로그램을 시작할 때 Windows에서 호출 하는 첫 번째 메서드는 [**IFrameworkView:: Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize)의 구현입니다.

구현은 해당 이벤트를 구독 하 여 일시 중단 (및 가능한 이후 다시 시작) 이벤트를 처리할 수 있도록 하는 것과 같이 UWP 게임의 가장 기본적인 동작을 처리 해야 합니다. 여기에서 디스플레이 어댑터 장치에 액세스할 수도 있으므로 장치에 종속 된 그래픽 리소스를 만들 수 있습니다.

```cppwinrt
void Initialize(CoreApplicationView const& applicationView)
{
    applicationView.Activated({ this, &App::OnActivated });

    CoreApplication::Suspending({ this, &App::OnSuspending });

    CoreApplication::Resuming({ this, &App::OnResuming });

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

가능 하면 원시 포인터를 사용 하지 않는 것이 좋습니다 (항상 가능한 경우).

- Windows 런타임 형식의 경우 포인터를 완전히 방지 하 고 스택에 값을 생성할 수 있습니다. 포인터가 필요한 경우 [**winrt:: com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) 을 사용 합니다 (곧 예제가 표시 됨).
- 고유 포인터의 경우 [**std:: unique_ptr**](/cpp/cpp/how-to-create-and-use-unique-ptr-instances) 및 **std:: make_unique**를 사용 합니다.
- 공유 포인터의 경우 [**std:: shared_ptr**](/cpp/cpp/how-to-create-and-use-shared-ptr-instances) 및 **std:: make_shared**를 사용 합니다.

## <a name="the-appsetwindow-method"></a>App:: SetWindow 메서드

**초기화**후 Windows에서는 [**IFrameworkView:: setwindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow)의 구현을 호출 하 여 게임의 주 창을 나타내는 [**CoreWindow**](/uwp/api/windows.ui.core.corewindow) 개체를 전달 합니다.

**앱:: SetWindow**에서 창 관련 이벤트를 구독 하 고 일부 창과 표시 동작을 구성 합니다. 예를 들어 마우스 포인터는 마우스 및 터치 컨트롤에서 사용할 수 있는 [**CoreCursor**](/uwp/api/windows.ui.core.corecursor) 클래스를 통해 생성 됩니다. 또한 window 개체를 장치 종속 리소스 개체에 전달 합니다.

[게임 흐름 관리](tutorial-game-flow-management.md#event-handling) 항목에서 이벤트 처리에 대해 자세히 설명 합니다.

```cppwinrt
void SetWindow(CoreWindow const& window)
{
    //CoreWindow window = CoreWindow::GetForCurrentThread();
    window.Activate();

    window.PointerCursor(CoreCursor(CoreCursorType::Arrow, 0));

    PointerVisualizationSettings visualizationSettings{ PointerVisualizationSettings::GetForCurrentView() };
    visualizationSettings.IsContactFeedbackEnabled(false);
    visualizationSettings.IsBarrelButtonFeedbackEnabled(false);

    m_deviceResources->SetWindow(window);

    window.Activated({ this, &App::OnWindowActivationChanged });

    window.SizeChanged({ this, &App::OnWindowSizeChanged });

    window.Closed({ this, &App::OnWindowClosed });

    window.VisibilityChanged({ this, &App::OnVisibilityChanged });

    DisplayInformation currentDisplayInformation{ DisplayInformation::GetForCurrentView() };

    currentDisplayInformation.DpiChanged({ this, &App::OnDpiChanged });

    currentDisplayInformation.OrientationChanged({ this, &App::OnOrientationChanged });

    currentDisplayInformation.StereoEnabledChanged({ this, &App::OnStereoEnabledChanged });

    DisplayInformation::DisplayContentsInvalidated({ this, &App::OnDisplayContentsInvalidated });
}
```

## <a name="the-appload-method"></a>App:: Load 메서드

이제 주 창이 설정 되었으므로 [**IFrameworkView:: Load**](/uwp/api/windows.applicationmodel.core.iframeworkview.load) 구현이 호출 됩니다. **로드** 는 **초기화** 및 **setwindow**보다 게임 데이터 또는 자산을 미리 인출 하는 더 좋은 장소입니다.

```cppwinrt
void Load(winrt::hstring const& /* entryPoint */)
{
    if (!m_main)
    {
        m_main = winrt::make_self<GameMain>(m_deviceResources);
    }
}
```

여기에서 볼 수 있듯이 실제 작업은 여기에서 만든 **GameMain** 개체의 생성자에 게 위임 됩니다. **GameMain** 클래스는 및에 정의 `GameMain.h` 되어 `GameMain.cpp` 있습니다.

### <a name="the-gamemaingamemain-constructor"></a>GameMain:: GameMain 생성자

**GameMain** 생성자와이 생성자가 호출 하는 다른 멤버 함수는 게임 개체를 만들고 그래픽 리소스를 로드 하 고 게임의 상태 시스템을 초기화 하는 일련의 비동기 로드 작업을 시작 합니다. 또한 시작 상태 또는 전역 값을 설정 하는 것과 같이 게임을 시작 하기 전에 필요한 준비 작업을 수행 합니다.

Windows에서는 게임에서 입력 처리를 시작 하기 전에 수행할 수 있는 시간을 제한 합니다. 따라서 여기서와 같이 asyc를 사용 하면 시작 하는 작업이 백그라운드에서 계속 진행 되는 동안 **부하가** 빠르게 반환 될 수 있음을 의미 합니다. 로드 시간이 오래 걸리거나 많은 리소스가 있는 경우 사용자에 게 자주 업데이트 되는 진행률 표시줄을 제공 하는 것이 좋습니다. 

비동기 프로그래밍을 처음 접하는 경우에는 [c + +/WinRT를 사용한 동시성 및 비동기 작업](../cpp-and-winrt-apis/concurrency.md)을 참조 하세요.

```cppwinrt
GameMain::GameMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) :
    m_deviceResources(deviceResources),
    m_windowClosed(false),
    m_haveFocus(false),
    m_gameInfoOverlayCommand(GameInfoOverlayCommand::None),
    m_visible(true),
    m_loadingCount(0),
    m_updateState(UpdateEngineState::WaitingForResources)
{
    m_deviceResources->RegisterDeviceNotify(this);

    m_renderer = std::make_shared<GameRenderer>(m_deviceResources);
    m_game = std::make_shared<Simple3DGame>();

    m_uiControl = m_renderer->GameUIControl();

    m_controller = std::make_shared<MoveLookController>(CoreWindow::GetForCurrentThread());

    auto bounds = m_deviceResources->GetLogicalSize();

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, bounds.Height - GameUIConstants::TouchRectangleSize),
        XMFLOAT2(GameUIConstants::TouchRectangleSize, bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(bounds.Width - GameUIConstants::TouchRectangleSize, bounds.Height - GameUIConstants::TouchRectangleSize),
        XMFLOAT2(bounds.Width, bounds.Height)
        );

    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    m_uiControl->SetAction(GameInfoOverlayCommand::None);
    m_uiControl->ShowGameInfoOverlay();

    // Asynchronously initialize the game class and load the renderer device resources.
    // By doing all this asynchronously, the game gets to its main loop more quickly
    // and in parallel all the necessary resources are loaded on other threads.
    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    auto lifetime = get_strong();

    m_game->Initialize(m_controller, m_renderer);

    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);

    // The finalize code needs to run in the same thread context
    // as the m_renderer object was created because the D3D device context
    // can ONLY be accessed on a single thread.
    // co_await of an IAsyncAction resumes in the same thread context.
    m_renderer->FinalizeCreateGameDeviceResources();

    InitializeGameState();

    if (m_updateState == UpdateEngineState::WaitingForResources)
    {
        // In the middle of a game so spin up the async task to load the level.
        co_await m_game->LoadLevelAsync();

        // The m_game object may need to deal with D3D device context work so
        // again the finalize code needs to run in the same thread
        // context as the m_renderer object was created because the D3D
        // device context can ONLY be accessed on a single thread.
        m_game->FinalizeLoadLevel();
        m_game->SetCurrentLevelToSavedState();
        m_updateState = UpdateEngineState::ResourcesLoaded;
    }
    else
    {
        // The game is not in the middle of a level so there aren't any level
        // resources to load.
    }

    // Since Game loading is an async task, the app visual state
    // may be too small or not have focus. Put the state machine
    // into the correct state to reflect these cases.

    if (m_deviceResources->GetLogicalSize().Width < GameUIConstants::MinPlayableWidth)
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
}

void GameMain::InitializeGameState()
{
    // Set up the initial state machine for handling Game playing state.
    ...
}
```

생성자에 의해 시작 되는 작업 시퀀스에 대 한 개요는 다음과 같습니다.

- **GameRenderer**형식의 개체를 만들고 초기화 합니다. 자세한 내용은 [렌더링 프레임 워크 I: 렌더링 소개](tutorial--assembling-the-rendering-pipeline.md)를 참조 하세요.
- **Simple3DGame**형식의 개체를 만들고 초기화 합니다. 자세한 내용은 [기본 게임 개체 정의](tutorial--defining-the-main-game-loop.md)를 참조 하세요.
- 게임 UI 컨트롤 개체를 만들고 게임 정보 오버레이를 표시 하 여 리소스 파일이 로드 될 때 진행률 표시줄을 표시 합니다. 자세한 내용은 [사용자 인터페이스 추가](tutorial--adding-a-user-interface.md)를 참조 하세요.
- 컨트롤러 개체를 만들어 컨트롤러 (터치, 마우스 또는 Xbox 무선 컨트롤러)에서 입력을 읽습니다. 자세한 내용은 [컨트롤 추가](tutorial--adding-controls.md)를 참조 하세요.
- 각각 이동 및 카메라 터치 컨트롤에 대 한 화면의 왼쪽 아래와 오른쪽 아래 모퉁이에 두 개의 사각형 영역을 정의 합니다. 플레이어는 카메라를 앞뒤로 이동 하는 데 사용 되는 가상 제어 패드로 왼쪽 아래 사각형 ( **SetMoveRect**호출에 정의 됨)을 사용 합니다. 오른쪽 아래 사각형 ( **SetFireRect** 메서드로 정의 됨)은 탄약을 발사 하는 가상 단추로 사용 됩니다.
- 코 루틴를 사용 하 여 리소스 로드를 별도의 단계로 나눌 수 있습니다. Direct3D 장치 컨텍스트에 대 한 액세스는 장치 컨텍스트가 만들어진 스레드로 제한 됩니다. 개체를 만들기 위해 Direct3D 장치에 액세스 하는 것은 무료 스레드입니다. 따라서 **GameRenderer:: CreateGameDeviceResourcesAsync** 코 루틴는 원래 스레드에서 실행 되는 완료 작업 (**GameRenderer:: FinalizeCreateGameDeviceResources**)에서 별도의 스레드에서 실행 될 수 있습니다.
- **Simple3DGame:: LoadLevelAsync** 및 **Simple3DGame:: FinalizeLoadLevel**를 사용 하 여 수준 리소스를 로드 하는 데 비슷한 패턴을 사용 합니다.

다음 항목 ([게임 흐름 관리](tutorial-game-flow-management.md))에서 **GameMain:: InitializeGameState** 에 대 한 추가 정보를 볼 수 있습니다.

## <a name="the-apponactivated-method"></a>App:: OnActivated 된 메서드

그런 다음 [**CoreApplicationView:: 활성화**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) 된 이벤트가 발생 합니다. 따라서 사용자가 보유 하 고 있는 **Onactivated** 된 이벤트 처리기 (예: **App:: onactivated** 된 메서드)가 호출 됩니다.

```cppwinrt
void OnActivated(CoreApplicationView const& /* applicationView */, IActivatedEventArgs const& /* args */)
{
    CoreWindow window = CoreWindow::GetForCurrentThread();
    window.Activate();
}
```

여기서 수행 하는 유일한 작업은 주 [**CoreWindow**](/uwp/api/windows.ui.core.corewindow)을 활성화 하는 것입니다. 또는 **앱:: SetWindow**에서 수행 하도록 선택할 수 있습니다.

## <a name="the-apprun-method"></a>App:: Run 메서드

**Initialize**, **Setwindow**및 **Load** 에서 단계를 설정 했습니다. 이제 게임이 실행 되 고 있으므로 [**IFrameworkView:: Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) 구현이 호출 됩니다.

```cppwinrt
void Run()
{
    m_main->Run();
}
```

다시 **GameMain**에 게 작업을 위임 합니다.

### <a name="the-gamemainrun-method"></a>GameMain:: Run 메서드

**GameMain:: Run** 은 게임의 주 루프입니다. 에서 찾을 수 있습니다 `GameMain.cpp` . 기본 논리는 게임 창이 열려 있는 상태에서 모든 이벤트를 디스패치 하 고 타이머를 업데이트 한 다음 그래픽 파이프라인의 결과를 렌더링 하 고 표시 하는 것입니다. 또한 여기에서 게임 상태를 전환 하는 데 사용 되는 이벤트를 발송 하 고 처리 합니다.

여기에서 코드는 게임 엔진 상태 시스템의 두 가지 상태에도 관심이 있습니다.

- **UpdateEngineState::D eactivated**되었습니다. 이는 게임 창이 비활성화 되거나 (포커스가 손실 됨) 또는 스냅 됨을 나타냅니다. 
- **UpdateEngineState:: TooSmall**. 이는 클라이언트 영역이 너무 작아 게임을 렌더링할 수 없음을 나타냅니다.

이러한 상태 중 하나에서 게임은 이벤트 처리를 일시 중단 하 고 창이 포커스를 unsnap 하거나 크기를 조정할 때까지 대기 합니다.

게임에 포커스가 있는 경우 메시지 큐가 도착할 때 메시지 큐의 모든 이벤트를 처리 해야 하므로 **ProcessAllIfPresent** 옵션을 사용 하 여 [**CoreWindowDispatch**](/uwp/api/windows.ui.core.coredispatcher.processevents) 를 호출 해야 합니다. 다른 옵션을 사용 하면 메시지 이벤트 처리에 지연이 발생 하 여 게임이 응답 하지 않거나 속도가 저하 되는 터치 동작이 발생할 수 있습니다.

게임을 볼 수 없거나, 일시 중단 되거나, 스냅 되지 않은 경우 도착 하지 않는 메시지를 발송 하는 모든 리소스를 사용 하는 것을 원치 않습니다. 이 경우 게임에서 **Processoneandallpending** 옵션을 사용 해야 합니다. 이 옵션은 이벤트를 받을 때까지 차단 된 다음 해당 이벤트를 처리 하 고 첫 번째를 처리 하는 동안 프로세스 큐에 도착 하는 다른 이벤트를 처리 합니다. 그러면 큐가 처리 된 후에 **CoreWindowDispatch** 가 즉시 반환 됩니다.

```cppwinrt
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
                    CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // otherwise fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close, so save state.
}
```

## <a name="the-appuninitialize-method"></a>App:: 초기화 취소 메서드

게임이 종료 되 면 [**IFrameworkView::**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize) 의 구현이 호출 됩니다. 이는 정리를 수행할 수 있는 기회입니다. 앱 창을 닫으면 앱 프로세스가 종료 되지 않습니다. 대신 응용 프로그램 singleton의 상태를 메모리에 기록 합니다. 시스템에서 리소스의 특별 한 정리를 포함 하 여이 메모리를 회수할 때 특별 한 작업이 발생 해야 하는 경우 해당 정리에 대 한 코드를 **초기화**하지 않도록 합니다.

이 경우 **App:: 초기화** 는 작동 하지 않습니다.

```cpp
void Uninitialize()
{
}
```

## <a name="tips"></a>팁

자신의 게임을 개발할 때이 항목에서 설명 하는 방법에 따라 시작 코드를 디자인 합니다. 다음은 각 방법에 대 한 간단한 기본 제안 목록입니다.

- **Initialize** 를 사용 하 여 기본 클래스를 할당 하 고 기본 이벤트 처리기를 연결 합니다.
- **Setwindow** 를 사용 하 여 창 특정 이벤트를 구독 하 고, 주 창을 장치 종속 리소스 개체에 전달 하 여 스왑 체인을 만들 때 해당 창을 사용할 수 있도록 합니다.
- **로드** 를 사용 하 여 나머지 설정을 처리 하 고 비동기 개체 생성과 리소스 로드를 시작 합니다. Procedurally 생성 된 자산과 같은 임시 파일이 나 데이터를 만들어야 하는 경우 여기에서 작업을 수행 해야 합니다.

## <a name="next-steps"></a>다음 단계

이 항목에서는 DirectX를 사용 하는 UWP 게임의 기본 구조 중 일부에 대해 설명 했습니다. 이후 항목에서 일부를 다시 참조할 예정 이기 때문에 이러한 메서드를 염두에 두는 것이 좋습니다.

다음 항목 &mdash; [게임 흐름 관리](tutorial-game-flow-management.md)에서 게임 &mdash; 상태와 이벤트 처리를 관리 하는 방법에 대해 자세히 살펴보고 게임을 계속 진행 합니다.