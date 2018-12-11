---
title: 게임 흐름 관리
description: 게임 상태를 초기화하고, 이벤트를 처리하고, 게임 업데이트 루프를 설정하는 방법을 알아보세요.
ms.assetid: 6c33bf09-b46a-4bb5-8a59-ca83ce257eb3
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, uwp, 게임, directx
ms.localizationpriority: medium
ms.openlocfilehash: c6d13b848a9e5d2dfc145431f732187c35c46ab6
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8895713"
---
# <a name="game-flow-management"></a>게임 흐름 관리

이제 게임에 몇 가지 이벤트 처리기가 등록된 창이 있으며 비동기적으로 자산을 로드합니다. 이 섹션은 게임 상태의 사용, 특정 키 게임 상태를 관리하는 방법 및 게임 엔진에 대한 업데이트 루프를 만드는 방법에 대해 설명합니다. 그 다음 사용자 인터페이스 흐름에 대해 알아보고, 마지막으로 이벤트 처리기 및 UWP 게임에 필요한 이벤트에 대해 자세히 살펴봅니다.

>[!Note]
>이 샘플의 최신 게임 코드를 다운로드하지 않은 경우 [Direct3D 게임 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)로 이동합니다. 이 샘플은 UWP 기능 샘플의 큰 컬렉션의 일부입니다. 샘플을 다운로드하는 방법에 대한 지침은 [GitHub에서 UWP 샘플 가져오기](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)를 참조하세요.

## <a name="game-states-used-to-manage-game-flow"></a>게임 흐름을 관리하는 데 사용되는 게임 상태

게임 상태를 사용하여 게임의 흐름을 관리할 수 있습니다. 사용자는 언제든지 일시 중단 상태에서 UWP 게임 앱을 다시 시작할 수 있으므로 앱의 가능한 상태 수에 제한이 없습니다.

이 게임 샘플은 시작할 때 다음 세 가지 상태 중 하나일 수 있습니다.
* 게임 루프가 실행되고 있으며 한 수준에 있습니다.
* 게임이 방금 완료되었으므로 게임 루프가 실행되고 있지 않습니다. (최고 점수가 설정됨)
* 시작된 게임이 없거나 게임이 수준 사이에 있습니다. (최고 점수는 0)

게임의 요구 사항에 따라 필요한 상태 수를 정의할 수 있습니다. 언제든지 UWP 게임을 종료할 수 있으며, 다시 시작할 때 플레이어는 게임이 재생 중지된 적이 없는 것처럼 동작하기를 기대한다는 것을 명심하세요.

## <a name="game-states-initialization"></a>게임 상태 초기화

게임 초기화에서 사용자는 앱 콜드 부팅만이 아니라 앱이 일시 중지되거나 종료된 후 다시 시작하는 것에 집중합니다. 샘플 게임은 항상 게임 상태를 저장하여 앱이 계속 실행되고 있는 것처럼 표시되게 합니다. 

일시 중단 상태는 게임 플레이가 일시 중단되었지만 게임 리소스가 여전히 메모리에 있음을 나타냅니다. 

마찬가지로, 다시 시작 이벤트는 샘플 게임이 마지막으로 일시 중단 또는 종료된 위치에서 다시 시작하도록 합니다. 샘플 게임을 종료한 후 다시 시작하면 게임이 정상적으로 시작된 다음 플레이어가 게임을 즉시 계속할 수 있도록 마지막으로 알려진 상태를 확인합니다.

상태에 따라 다른 옵션이 플레이어에게 제공됩니다. 

* 게임이 중간 수준에서 다시 시작되면 게임이 일시 중지된 것처럼 표시되며 오버레이에 계속 옵션이 제공됩니다.
* 게임을 완료한 상태에서 게임을 다시 시작하면 최고 점수와 새 게임을 플레이하는 옵션이 표시됩니다.
* 마지막으로, 한 수준이 시작되기 전에 게임이 다시 시작되면 오버레이에서 사용자에게 시작 옵션을 제공합니다.

게임 샘플은 일시 중단 이벤트 없이 처음 시작하는 콜드 부팅과 일시 중단 상태에서 다시 시작하는 시작을 구분하지 않습니다. 이 디자인은 모든 UWP 앱에 적합합니다.

이 샘플에서 게임 상태 초기화는 [__GameMain::InitializeGameState__](#gamemaininitializegamestate-method)에서 발생합니다.

이 순서도는 흐름을 시각화하는 데 도움이 되며, 초기화 및 업데이트 관련 루프를 모두 다룹니다.

* 초기화는 사용자가 게임의 현재 상태를 확인할 때 __시작__ 노드에서 시작합니다. 게임 코드에 대해 [__GameMain::InitializeGameState__](#gamemaininitializegamestate-method)로 이동합니다.
* 업데이트 루프에 대한 자세한 내용은 [게임 엔진 업데이트](#update-game-engine)를 참조하세요. 게임 코드에 대해 [__App::Update__](#appupdate-method)로 이동합니다.

![게임의 주 상태 시스템](images/simple-dx-game-flow-statemachine.png)

### <a name="gamemaininitializegamestate-method"></a>GameMain::InitializeGameState 메서드

__InitializeGameState__ 메서드는 __GameMain__ 클래스 개체가 [__App::Load__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L115-L123) 메서드에서 생성될 때 호출되는 [__GameMain__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L32-L131) 생성자 클래스에서 호출됩니다.

```cpp

GameMain::GameMain(...)
{
    m_deviceResources->RegisterDeviceNotify(this);
    ...

    create_task([this]()
    {
        ...

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // as the m_renderer object was created because the D3D device context
        // can ONLY be accessed on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState(); //Initialization of game states occurs here.
        
        ...
    
    }, task_continuation_context::use_current()).then([this]()
    {
        ...
        
    }, task_continuation_context::use_current());
}

```

```cpp

void GameMain::InitializeGameState()
{
    // Set up the initial state machine for handling Game playing state.
    if (m_game->GameActive() && m_game->LevelActive())
    {
        // The last time the game terminated it was in the middle of a level.
        // We are waiting for the user to continue the game.
        //...
    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        // The last time the game terminated the game had been completed.
        // Show the high score.
        // We are waiting for the user to acknowledge the high score and start a new game.
        // The level resources for the first level will be loaded later.
        //...
    }
    else
    {
        // This is either the first time the game has run or
        // the last time the game terminated the level was completed.
        // We are waiting for the user to begin the next level.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::PlayLevel;
        SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
        m_uiControl->SetAction(GameInfoOverlayCommand::PleaseWait);
    }
    m_uiControl->ShowGameInfoOverlay();
}

```

## <a name="update-game-engine"></a>게임 엔진 업데이트

[__App::Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L127-L130) 메서드에서 [__GameMain::Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L143-L202)을 호출합니다. 샘플에서는 플레이어가 수행할 수 있는 모든 주요 작업을 처리하는 기본 상태 시스템을 이 메서드 내에 구현했습니다. 이 상태 시스템의 최상위 수준에서는 게임 로드, 특정 레벨 플레이 또는 시스템 또는 플레이어에 의해 게임이 일시 중지된 후 레벨 진행을 처리합니다.

게임 샘플에는 게임의 가능한 세 가지 주요 상태(__UpdateEngineState__)가 있습니다.

1. __Waiting for resources__: 게임 루프가 순환되며 리소스(특히 그래픽 리소스)가 사용 가능할 때까지 전환할 수 없습니다. 리소스를 로드하는 비동기 작업이 완료되면 상태를 __ResourcesLoaded__로 업데이트합니다. 일반적으로 디스크, 게임 서버, 또는 클라우드 백 엔드에서 수준이 새 리소스를 로드할 때 발생합니다. 게임 샘플에서 이 동작은 해당 시점에 샘플에 수준별 추가 리소스가 필요 없기 때문에 시뮬레이트됩니다.
2. __Waiting for press__: 게임 루프가 순환되며 특정 사용자 입력을 기다립니다. 이 입력은 게임을 로드하거나, 수준을 시작하거나, 수준을 계속하는 플레이어 작업입니다. 이 샘플 코드에서는 이러한 하위 상태를 __PressResultState__ 열거형 값으로 나타냅니다.
3. __Dynamics__: 게임 루프가 사용자 플레이로 실행되고 있습니다. 사용자가 플레이하는 동안 게임은 전환할 수 있는 3가지 조건을 확인합니다. 
    * __TimeExpired__: 수준에 대해 설정된 시간 만료
    * __LevelComplete__: 플레이어에 의해 수준 완료 
    * __GameComplete__: 플레이어에 의해 모든 수준 완료

게임은 여러 작은 상태 시스템을 포함하는 상태 시스템입니다. 각 특정 상태는 매우 구체적인 특정 조건으로 정의되어야 합니다. 한 상태에서 다른 상태로 전환하는 방법은 개별 사용자 입력 또는 시스템 동작(예: 그래픽 리소스 로드)을 기반으로 해야 합니다. 게임을 계획할 때 사용자 또는 시스템이 수행할 수 있는 가능한 모든 작업을 다룰 수 있도록 전체 게임 흐름을 그리는 것을 고려하세요. 게임은 매우 복잡할 수 있으며 그렇기 때문에 상태 시스템은 이러한 복잡성을 시각화하여 보다 관리하기 쉽게 만드는 강력한 도구입니다.

아래에서 업데이트 루프에 대한 코드를 살펴보겠습니다.

### <a name="appupdate-method"></a>App::Update 메서드

게임 엔진을 업데이트하는 데 사용된 상태 시스템의 구조

```cpp
void GameMain::Update()
{
    m_controller->Update(); //the controller instance has its own update loop.

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        //...
        break;

    case UpdateEngineState::ResourcesLoaded:
        //...
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete())
        {
            //...
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            //...
        }
        else
        {
            GameState runState = m_game->RunGame(); //when the player is playing, the work is handled by this Simple3DGame::RunGame method.
            switch (runState)
            {
            case GameState::TimeExpired:
                //...
                break;

            case GameState::LevelComplete:
                //...
                break;

            case GameState::GameComplete:
                //...
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // Transitioning state, so enable waiting for the press event
            m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
        }
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // Transitioning state, so shut down the input controller until resources are loaded
            m_controller->Active(false);
        }
        break;
    }
}
```

## <a name="update-user-interface"></a>사용자 인터페이스 업데이트

플레이어가 시스템 상태에 대한 알림을 계속 받도록 하여 플레이어의 동작 및 게임을 정의하는 규칙에 따라 게임 상태를 변경할 수 있도록 해야 합니다. 이 게임 샘플을 포함하여 게임 대부분은 일반적으로 사용자 인터페이스(UI) 요소를 사용하여 플레이어에게 이 정보를 제공합니다. UI에는 게임 상태 및 점수 또는 탄약, 또는 남은 가능성의 수와 같은 기타 재생 관련 정보가 포함됩니다. UI는 주 그래픽 파이프라인과 별도로 렌더링되고 3D 투영 위에 배치되므로 오버레이라고도 합니다. 일부 UI 정보도 게임 플레이 주요 영역을 벗어나지 않고 사용자가 이러한 정보를 얻을 수 있도록 표시 HUD(헤드업 디스플레이)로 제공됩니다. 샘플 게임에서 이 오버레이는 Direct2D API를 사용하여 만듭니다. XAML을 사용하여 이 오버레이를 만들 수도 있으며, [게임 샘플 확장](tutorial-resources.md)에서 설명합니다.

사용자 인터페이스에 대한 구성 요소는 두 가지가 있습니다.

-   점수 및 현재 게임 플레이 상태에 대한 정보를 포함하는 HUD.
-   게임의 일시 중지/일시 중단 상태 중 오버레이된 텍스트가 있는 검정색 사각형의 일시 중지 비트맵. 게임 오버레이입니다. 게임 오버레이는 나중에 [사용자 인터페이스 추가](tutorial--adding-a-user-interface.md)에서 살펴봅니다.

당연히 오버레이에도 상태 시스템이 있습니다. 오버레이는 레벨 시작 또는 게임 종료 메시지를 표시할 수 있습니다. 기본적으로 게임이 일시 중지되거나 일시 중단되면 플레이어에게 표시할 게임 상태에 대한 정보를 출력하는 캔버스입니다.

렌더링된 오버레이는 게임의 상태에 따라 6 화면 중 하나가 될 수 있습니다. 
1. 게임 시작 시 화면을 로드하는 리소스
2. 게임 통계 플레이 화면
3. 수준 시작 메시지 화면
4. 시간이 부족하지 않고 모든 수준이 완료되었을 때 게임 종료 화면
5. 시간이 초과될 경우 게임 종료 화면
6. 일시 중지 메뉴 화면

게임의 그래픽 파이프라인에서 사용자 인터페이스를 분리하면 게임의 독립적인 그래픽 렌더링 엔진에서 작업을 수행하여 게임 코드의 복잡성을 현저하게 줄일 수 있습니다.

다음은 게임 샘플에서 오버레이의 상태 시스템을 구조화하는 방법입니다.

```cpp
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        //...
        break;

    case GameInfoOverlayState::LevelStart:
        //...
        break;

    case GameInfoOverlayState::GameOverCompleted:
        //...
        break;

    case GameInfoOverlayState::GameOverExpired:
        //...
        break;

    case GameInfoOverlayState::Pause:
        //...
        break;
    }
}
```

## <a name="events-handling"></a>이벤트 처리

샘플 코드에서는 App.cpp의 **Initialize**, **SetWindow** 및 **Load**의 특정 이벤트에 대해 많은 처리기를 등록했습니다. 이러한 이벤트는 게임 기술을 추가하거나 그래픽 개발을 시작하기 전에 작업해야 하는 중요한 이벤트입니다. 이러한 이벤트는 적절한 UWP 앱 환경에 기본적인 요소입니다. UWP 앱은 언제든지 활성화, 비활성화, 크기 조정, 스냅, 스냅 취소, 일시 중단 또는 다시 시작할 수 있으므로 게임에서 최대한 빨리 해당 이벤트를 등록하고 환경을 원활하며 플레이어가 예측할 수 있게 유지하는 방식으로 처리해야 합니다.

다음은 샘플에서 사용되는 이벤트 처리기와 해당 처리기에서 처리하는 이벤트입니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">이벤트 처리기</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">OnActivated</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/apps/br225018"><strong>CoreApplicationView::Activated</strong></a>를 처리합니다. 게임 앱이 전경으로 전환되었으므로 주 창이 활성화됩니다.</td>
</tr>
<tr class="even">
<td align="left">OnDpiChanged</td>
<td align="left"><a href="https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DpiChanged"><strong>Graphics::Display::DisplayInformation::DpiChanged</strong></a>를 처리합니다. 디스플레이의 DPI가 변경되었으며 게임이 해당 리소스를 적절하게 조정합니다.
<div class="alert">
<strong>참고</strong>[<strong>CoreWindow</strong>] (https://msdn.microsoft.com/library/windows/desktop/hh404559) 좌표는 [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370987)에 대 한 Dip (디바이스 독립적 픽셀). 따라서 2D 자산이나 원형을 올바르게 표시하려면 Direct2D에 DPI 변경을 알려야 합니다.
</div>
<div>
</div></td>
</tr>
<tr class="odd">
<td align="left">OnOrientationChanged</td>
<td align="left"><a href="https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_OrientationChanged"><strong>Graphics::Display::DisplayInformation::OrientationChanged</strong></a>를 처리합니다. 디스플레이 방향이 변경되며 렌더링을 업데이트해야 합니다.</td>
</tr>
<tr class="even">
<td align="left">OnDisplayContentsInvalidated</td>
<td align="left"><a href="https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DisplayContentsInvalidated"><strong>Graphics::Display::DisplayInformation::DisplayContentsInvalidated</strong></a>를 처리합니다. 디스플레이 다시 그리기가 필요하며 사용자의 게임을 다시 렌더링해야 합니다.</td>
</tr>
<tr class="odd">
<td align="left">OnResuming</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/apps/br205859"><strong>CoreApplication::Resuming</strong></a>을 처리합니다. 게임 앱이 일시 중단 상태에서 게임을 복원합니다.</td>
</tr>
<tr class="even">
<td align="left">OnSuspending</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/apps/br205860"><strong>CoreApplication::Suspending</strong></a>을 처리합니다. 게임 앱에서 해당 상태를 디스크에 저장합니다. 상태를 저장소에 저장하려면 5초가 필요합니다.</td>
</tr>
<tr class="odd">
<td align="left">OnVisibilityChanged</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/apps/hh701591"><strong>CoreWindow::VisibilityChanged</strong></a>를 처리합니다. 게임 앱의 가시성이 변경되어 표시되는 것으로 변환되거나 표시되지 않고 다른 앱 표시로 전환되었습니다.</td>
</tr>
<tr class="even">
<td align="left">OnWindowActivationChanged</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/apps/br208255"><strong>CoreWindow::Activated</strong></a>를 처리합니다. 게임 앱의 주 창이 비활성화되거나 활성화되어 포커스를 제거하고 게임을 일시 중지하거나 포커스를 다시 얻어야 합니다. 두 경우 모두 오버레이에는 게임이 일시 중지된 것으로 표시됩니다.</td>
</tr>
<tr class="odd">
<td align="left">OnWindowClosed</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/apps/br208261"><strong>CoreWindow::Closed</strong></a>를 처리합니다. 게임 앱에서 주 창을 닫고 게임을 일시 중단합니다.</td>
</tr>
<tr class="even">
<td align="left">OnWindowSizeChanged</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/apps/br208283"><strong>CoreWindow::SizeChanged</strong></a>를 처리합니다. 게임 앱에서 크기 변경을 수용하도록 그래픽 리소스 및 오버레이를 다시 할당한 다음 렌더링 대상을 업데이트합니다.</td>
</tr>
</tbody>
</table>

## <a name="next-steps"></a>다음 단계

이 항목에서는 게임 상태를 사용하여 전체 게임 흐름을 관리하는 방법과 여러 다른 상태 시스템으로 구성된 게임에 대해 알아보았습니다. 또한 UI를 업데이트하고 주요 앱 이벤트 처리기를 관리하는 방법에 대해 학습했습니다. 이제 루프, 게임 및 방법을 렌더링하는 것에 자세히 알아볼 준비가 되었습니다.
 
이 게임을 구성하는 다른 구성 요소를 원하는 순서대로 살펴볼 수 있습니다.
* [주 게임 개체 정의](tutorial--defining-the-main-game-loop.md)
* [렌더링 프레임워크 I: 렌더링 소개](tutorial--assembling-the-rendering-pipeline.md)
* [렌더링 프레임워크 II: 게임 렌더링](tutorial-game-rendering.md)
* [사용자 인터페이스 추가](tutorial--adding-a-user-interface.md)
* [컨트롤 추가](tutorial--adding-controls.md)
* [소리 추가](tutorial--adding-sound.md)