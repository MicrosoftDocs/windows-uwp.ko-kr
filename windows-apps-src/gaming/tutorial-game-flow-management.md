---
title: 게임 흐름 관리
description: 게임 상태를 초기화 하 고, 이벤트를 처리 하 고, 게임 업데이트 루프를 설정 하는 방법을 알아봅니다.
ms.assetid: 6c33bf09-b46a-4bb5-8a59-ca83ce257eb3
ms.date: 06/24/2020
ms.topic: article
keywords: Windows 10, UWP, 게임, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 181eca743a9ccdc76ebfc1302e8bb04d85a32269
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409632"
---
# <a name="game-flow-management"></a>게임 흐름 관리

> [!NOTE]
> 이 항목은 DirectX 자습서 시리즈 [를 사용 하 여 단순 유니버설 Windows 플랫폼 (UWP) 게임 만들기](tutorial--create-your-first-uwp-directx-game.md) 의 일부입니다. 해당 링크의 항목은 계열의 컨텍스트를 설정 합니다.

게임에는 이제 일부 이벤트 처리기를 등록 하 고 자산을 비동기식으로 로드 한 창이 있습니다. 이 항목에서는 게임 상태 사용, 특정 키 게임 상태를 관리 하는 방법 및 게임 엔진에 대 한 업데이트 루프를 만드는 방법을 설명 합니다. 그런 다음 사용자 인터페이스 흐름에 대해 알아보고, UWP 게임에 필요한 이벤트 처리기에 대 한 자세한 내용을 이해 합니다.

## <a name="game-states-used-to-manage-game-flow"></a>게임 흐름을 관리 하는 데 사용 되는 게임 상태

게임 상태를 사용 하 여 게임의 흐름을 관리 합니다.

**Simple3DGameDX** 샘플 게임은 컴퓨터에서 처음으로 실행 되는 경우 게임이 시작 되지 않은 상태에 있습니다. 다음 번에 게임을 실행 하면 이러한 상태 중 하나에 있을 수 있습니다.

- 게임이 시작 되지 않았거나 수준 사이에 게임이 있습니다 (높은 점수는 0).
- 게임 루프가 실행 중 이며 수준 중간에 있습니다.
- 게임이 완료 되었기 때문에 게임 루프가 실행 되 고 있지 않습니다 (높은 점수에 0이 아닌 값이 있음).

게임에는 필요한 만큼의 상태가 있을 수 있습니다. 하지만 언제 든 지 종료할 수 있습니다. 그리고 다시 시작 될 때 사용자는 종료 된 상태에서 다시 시작 될 것으로 예상 합니다.

## <a name="game-state-management"></a>게임 상태 관리

따라서 게임을 초기화 하는 동안 게임의 콜드 시작을 지원 하 고 비행을 중지 한 후 게임을 다시 시작 해야 합니다. **Simple3DGameDX** 샘플은 항상 중지 되지 않는다는 느낌을 주기 위해 게임 상태를 저장 합니다.

일시 중단 이벤트에 대 한 응답으로 플레이는 일시 중단 되지만 게임 리소스는 여전히 메모리에 있습니다. 마찬가지로, resume 이벤트는 샘플 게임이 일시 중단 되었거나 종료 된 상태에서 선택 되도록 하기 위해 처리 됩니다. 상태에 따라 플레이어에 게 다양 한 옵션이 표시 됩니다. 

- 게임이 중간 수준으로 다시 시작 되 면 일시 중지 된 것으로 표시 되 고 오버레이는 계속 하는 옵션을 제공 합니다.
- 게임이 완료 된 상태로 게임을 다시 시작 하는 경우 높은 점수와 새 게임을 재생 하는 옵션을 표시 합니다.
- 마지막으로, 수준이 시작 되기 전에 게임을 다시 시작 하는 경우 오버레이는 사용자에 게 시작 옵션을 제공 합니다.

샘플 게임은 게임의 콜드 시작, 일시 중단 이벤트 없이 처음으로 시작 또는 일시 중단 된 상태에서 재개를 구분 하지 않습니다. 이는 모든 UWP 앱을 위한 적절 한 디자인입니다.

이 샘플에서 게임 상태의 초기화는 [**GameMain:: InitializeGameState**](#the-gamemaininitializegamestate-method) 에서 발생 합니다 (해당 메서드의 개요는 다음 섹션에 표시 됨).

흐름을 시각화 하는 데 도움이 되는 순서도는 다음과 같습니다. 초기화와 업데이트 루프를 모두 다룹니다.

- 현재 게임 상태를 확인 하면 **시작** 노드에서 초기화가 시작 됩니다. 게임 코드는 다음 섹션에서 [**GameMain:: InitializeGameState**](#the-gamemaininitializegamestate-method) 를 참조 하세요.
* 업데이트 루프에 대 한 자세한 내용을 보려면 [게임 엔진 업데이트](#update-game-engine)로 이동 하세요. 게임 코드의 경우 [**GameMain:: Update**](#the-gamemainupdate-method)로 이동 합니다.

![게임의 주 상태 시스템입니다.](images/simple-dx-game-flow-statemachine.png)

### <a name="the-gamemaininitializegamestate-method"></a>GameMain:: InitializeGameState 메서드

**GameMain:: InitializeGameState** 메서드는 **GameMain** 클래스의 생성자를 통해 간접적으로 호출 됩니다 .이는 **App:: Load**내에서 **GameMain** 인스턴스를 만드는 결과입니다.

```cppwinrt
GameMain::GameMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) : ...
{
    m_deviceResources->RegisterDeviceNotify(this);
    ...
    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    ...
    m_renderer->FinalizeCreateGameDeviceResources();

    InitializeGameState();
    ...
}

void GameMain::InitializeGameState()
{
    // Set up the initial state machine for handling Game playing state.
    if (m_game->GameActive() && m_game->LevelActive())
    {
        // The last time the game terminated it was in the middle
        // of a level.
        // We are waiting for the user to continue the game.
        ...
    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        // The last time the game terminated the game had been completed.
        // Show the high score.
        // We are waiting for the user to acknowledge the high score and start a new game.
        // The level resources for the first level will be loaded later.
        ...
    }
    else
    {
        // This is either the first time the game has run or
        // the last time the game terminated the level was completed.
        // We are waiting for the user to begin the next level.
        ...
    }
    m_uiControl->ShowGameInfoOverlay();
}
```

## <a name="update-game-engine"></a>게임 엔진 업데이트

**App:: run** 메서드는 **GameMain:: run**을 호출 합니다. **GameMain:: Run** 내에서 사용자가 수행할 수 있는 모든 주요 작업을 처리 하기 위한 기본 상태 시스템입니다. 이 상태 시스템의 가장 높은 수준은 게임을 로드 하거나, 특정 수준을 재생 하거나, 게임이 일시 중지 된 후 (시스템 또는 사용자에 의해) 수준을 계속 하는 것을 다룹니다.

샘플 게임에는 4 가지 주요 상태 ( **UpdateEngineState** 열거형으로 표시 됨)가 있습니다.

1. **UpdateEngineState:: WaitingForResources**. 게임 루프가 순환 하 고 있으므로 리소스 (특히 그래픽 리소스)를 사용할 수 있을 때까지 전환할 수 없습니다. 비동기 리소스 로드 작업이 완료 되 면 상태를 **UpdateEngineState:: Sourceloaded**로 업데이트 합니다. 이는 일반적으로 수준이 디스크, 게임 서버 또는 클라우드 백 엔드에서 새 리소스를 로드 하는 경우에 발생 합니다. 샘플 게임에서는이 문제를 시뮬레이션 합니다 .이 문제는 해당 시점에 추가 수준 별 리소스가 필요 하지 않기 때문입니다.
2. **UpdateEngineState:: WaitingForPress를 누릅니다**. 게임 루프가 특정 사용자 입력을 기다리는 중입니다. 이 입력은 게임을 로드 하거나 수준을 시작 하거나 수준을 계속 하는 플레이어 작업입니다. 샘플 코드는 **보도 상태** 열거를 통해 이러한 하위 상태를 참조 합니다.
3. **UpdateEngineState::D ynamics**. 게임 루프가 사용자를 재생 하는 동안 실행 되 고 있습니다. 사용자가 게임을 진행 하는 동안 게임은 전환할 수 있는 3 개의 조건을 확인 합니다. 
 - **GameState:: TimeExpired 됨**. 수준에 대 한 시간 제한이 만료 되었습니다.
 - **GameState:: LevelComplete**. 플레이어의 수준 완료
 - **GameState:: GameComplete**. 플레이어의 모든 수준을 완료 합니다.

게임 이란 단순히 여러 개의 작은 상태 컴퓨터를 포함 하는 상태 시스템입니다. 각 특정 상태는 매우 특정 한 조건으로 정의 되어야 합니다. 한 상태에서 다른 상태로 전환 하는 것은 불연속 사용자 입력 또는 시스템 작업 (예: 그래픽 리소스 로딩)을 기반으로 해야 합니다.

게임을 계획 하는 동안 사용자 또는 시스템에서 수행할 수 있는 모든 작업을 해결 하기 위해 전체 게임 흐름을 그려 보세요. 게임은 매우 복잡할 수 있으므로 상태 시스템은 이러한 복잡성을 시각화 하 고 보다 쉽게 관리할 수 있도록 하는 강력한 도구입니다.

업데이트 루프의 코드를 살펴보겠습니다.

### <a name="the-gamemainupdate-method"></a>GameMain:: Update 메서드

게임 엔진을 업데이트 하는 데 사용 되는 상태 시스템의 구조입니다.

```cppwinrt
void GameMain::Update()
{
    // The controller object has its own update loop.
    m_controller->Update(); 

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        ...
        break;

    case UpdateEngineState::ResourcesLoaded:
        ...
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete())
        {
            ...
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            ...
        }
        else
        {
            // When the player is playing, work is done by Simple3DGame::RunGame.
            GameState runState = m_game->RunGame();
            switch (runState)
            {
            case GameState::TimeExpired:
                ...
                break;

            case GameState::LevelComplete:
                ...
                break;

            case GameState::GameComplete:
                ...
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // Transitioning state, so enable waiting for the press event.
            m_controller->WaitForPress(
                m_renderer->GameInfoOverlayUpperLeft(),
                m_renderer->GameInfoOverlayLowerRight());
        }
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // Transitioning state, so shut down the input controller
            // until resources are loaded.
            m_controller->Active(false);
        }
        break;
    }
}
```

## <a name="update-the-user-interface"></a>사용자 인터페이스 업데이트

플레이어의 상태를 시스템 계속 받을으로 유지 하 고 플레이어의 작업 및 게임을 정의 하는 규칙에 따라 게임 상태를 변경 하도록 해야 합니다. 이 샘플 게임을 비롯 한 많은 게임은 일반적으로 UI (사용자 인터페이스) 요소를 사용 하 여 플레이어에 게이 정보를 제공 합니다. UI에는 게임 상태와 점수, 탄약 또는 남은 확률의 수와 같은 기타 재생 관련 정보에 대 한 표현이 포함 되어 있습니다. UI는 주 그래픽 파이프라인과 별도로 렌더링 되 고 3D 투영 위에 배치 되기 때문에 오버레이 라고도 합니다.

일부 UI 정보도 HUD (준비 표시)로 제공 되므로 사용자가 주 게임 영역 전체에서 눈에 보이지 않고 해당 정보를 볼 수 있습니다. 샘플 게임에서는 Direct2D Api를 사용 하 여이 오버레이를 만듭니다. 또는 [샘플 게임 확장](tutorial-resources.md)에 설명 된 XAML을 사용 하 여이 오버레이를 만들 수 있습니다.

사용자 인터페이스에는 두 가지 구성 요소가 있습니다.

- 게임 플레이의 현재 상태에 대 한 점수 및 정보를 포함 하는 HUD입니다.
- 일시 중지 됨/일시 중단 됨 상태에서 텍스트가 겹친 검은색 사각형 인 일시 중지 비트맵입니다. 게임 오버레이입니다. [사용자 인터페이스를 추가 하는](tutorial--adding-a-user-interface.md)방법에 대해 자세히 설명 합니다.

오버레이에는 상태 시스템도 있습니다. 오버레이는 수준 시작 또는 게임을 통해 메시지를 표시할 수 있습니다. 기본적으로 게임을 일시 중지 하거나 일시 중단 하는 동안 플레이어에 게 표시 하려는 게임 상태에 대 한 정보를 출력할 수 있는 캔버스입니다.

렌더링 된 오버레이는 게임의 상태에 따라 이러한 6 개의 화면 중 하나일 수 있습니다.

1. 게임 시작 시 리소스 로드 진행률 화면입니다.
2. 게임 플레이 통계 화면.
3. 수준 시작 메시지 화면입니다.
4. 모든 단계가 완료 되 면 실행 되는 시간을 초과 하지 않고 완료 되는 게임 화면
5. 시간이 초과 되 면 게임을 통해 화면을 진행 합니다.
6. 메뉴 화면을 일시 중지 합니다.

게임의 그래픽 파이프라인에서 사용자 인터페이스를 분리 하면 게임의 그래픽 렌더링 엔진과 독립적으로 작업할 수 있으며 게임 코드의 복잡성을 크게 줄일 수 있습니다.

샘플 게임에서 오버레이 상태 시스템을 구조화 하는 방법은 다음과 같습니다.

```cppwinrt
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        ...
        break;

    case GameInfoOverlayState::LevelStart:
        ...
        break;

    case GameInfoOverlayState::GameOverCompleted:
        ...
        break;

    case GameInfoOverlayState::GameOverExpired:
        ...
        break;

    case GameInfoOverlayState::Pause:
        ...
        break;
    }
}
```

## <a name="event-handling"></a>이벤트 처리

[게임의 UWP 앱 프레임 워크 정의](tutorial--building-the-games-uwp-app-framework.md) 항목에서 보았듯이 **앱** 클래스의 많은 뷰 공급자 메서드는 이벤트 처리기를 등록 합니다. 이러한 메서드는 게임 메커니즘을 추가 하거나 그래픽 개발을 시작 하기 전에 이러한 중요 이벤트를 올바르게 처리 해야 합니다.

이러한 이벤트를 적절 하 게 처리 하는 것은 UWP 앱 환경의 기본 사항입니다. UWP 앱은 언제 든 지 활성화, 비활성화, 크기 조정, 맞추기가, 일시 중단 또는 다시 시작 될 수 있으므로 게임은 이러한 이벤트를 가능한 한 빨리 등록 하 고 플레이어에 대해 원활 하 고 예측 가능한 환경을 유지 하는 방식으로 처리 해야 합니다.

이러한 처리기는이 샘플에서 사용 되는 이벤트 처리기와 이러한 처리기가 처리 하는 이벤트입니다.

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
<td align="left"><a href="/uwp/api/windows.applicationmodel.core.coreapplicationview.activated"><strong>CoreApplicationView:: 활성화</strong></a>를 처리 합니다. 게임 앱을 포그라운드로 가져온 후 주 창이 활성화 됩니다.</td>
</tr>
<tr class="even">
<td align="left">OnDpiChanged</td>
<td align="left">핸들 <a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DpiChanged"><strong>그래픽::D isplay::D isplayInformation::D pichanged</strong></a>. 디스플레이의 DPI가 변경 되 고 게임에서 리소스를 적절 하 게 조정 합니다.
<div class="alert">
<strong>참고</strong> <a href="/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow"><strong>CoreWindow</strong></a> 좌표는 <a href="/windows/desktop/Direct2D/direct2d-overview">Direct2D</a>에 대 한 dip (장치 독립적 픽셀)입니다. 따라서 2D 자산이 나 기본 형식을 올바르게 표시 하려면 Direct2D의 변경 내용을 DPI에 알려야 합니다.
</div>
<div>
</div></td>
</tr>
<tr class="odd">
<td align="left">OnOrientationChanged</td>
<td align="left">처리 <a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_OrientationChanged"><strong>그래픽::D isplay::D isplayInformation:: OrientationChanged</strong></a>. 표시 방향이 변경 되 고 렌더링을 업데이트 해야 합니다.</td>
</tr>
<tr class="even">
<td align="left">OnDisplayContentsInvalidated 검사 됨</td>
<td align="left">핸들 <a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DisplayContentsInvalidated"><strong>그래픽::D isplay::D isplayInformation::D Isplaycontentsin유효성이 검사</strong></a>되었습니다. 디스플레이를 다시 그려야 하며 게임을 다시 렌더링 해야 합니다.</td>
</tr>
<tr class="odd">
<td align="left">OnResuming</td>
<td align="left"><a href="/uwp/api/windows.applicationmodel.core.coreapplication.resuming"><strong>CoreApplication:: 재개</strong></a>를 처리 합니다. 게임 앱은 일시 중단 된 상태에서 게임을 복원 합니다.</td>
</tr>
<tr class="even">
<td align="left">OnSuspending 중단</td>
<td align="left"><a href="/uwp/api/windows.applicationmodel.core.coreapplication.suspending"><strong>CoreApplication:: 일시 중단</strong></a>을 처리 합니다. 게임 앱은 상태를 디스크에 저장 합니다. 저장소에 상태를 저장 하는 데 5 초가 있습니다.</td>
</tr>
<tr class="odd">
<td align="left">OnVisibilityChanged</td>
<td align="left"><a href="/uwp/api/windows.ui.core.corewindow.visibilitychanged"><strong>CoreWindow:: VisibilityChanged</strong></a>를 처리 합니다. 게임 앱이 표시 유형으로 변경 되었으며, 다른 앱이 표시 될 때 표시 되거나 표시 되지 않았습니다.</td>
</tr>
<tr class="even">
<td align="left">OnWindowActivationChanged</td>
<td align="left"><a href="/uwp/api/windows.ui.core.corewindow.activated"><strong>CoreWindow:: 활성화</strong></a>를 처리 합니다. 게임 앱의 주 창이 비활성화 되거나 활성화 되었으므로 포커스를 제거 하 고 게임을 일시 중지 하거나 포커스를 다시 만들어야 합니다. 두 경우 모두 오버레이는 게임이 일시 중지 되었음을 나타냅니다.</td>
</tr>
<tr class="odd">
<td align="left">OnWindowClosed</td>
<td align="left"><a href="/uwp/api/windows.ui.core.corewindow.closed"><strong>CoreWindow:: Closed</strong></a>를 처리 합니다. 게임 앱이 주 창을 닫고 게임을 일시 중단 합니다.</td>
</tr>
<tr class="even">
<td align="left">OnWindowSizeChanged</td>
<td align="left"><a href="/uwp/api/windows.ui.core.corewindow.sizechanged"><strong>CoreWindow:: SizeChanged</strong></a>를 처리 합니다. 게임 앱은 크기 변경을 수용할 수 있도록 그래픽 리소스와 오버레이를 다시 할당 한 다음 렌더링 대상을 업데이트 합니다.</td>
</tr>
</tbody>
</table>

## <a name="next-steps"></a>다음 단계

이 항목에서는 게임 상태를 사용 하 여 전체 게임 흐름을 관리 하는 방법 및 여러 다른 상태 컴퓨터로 구성 된 게임을 살펴보았습니다. 또한 UI를 업데이트 하 고 키 앱 이벤트 처리기를 관리 하는 방법도 살펴보았습니다. 이제 렌더링 루프, 게임 및 해당 메커니즘을 살펴볼 준비가 되었습니다.
 
모든 순서로이 게임을 문서화 하는 나머지 항목을 진행할 수 있습니다.

- [주 게임 개체 정의](tutorial--defining-the-main-game-loop.md)
- [렌더링 프레임 워크 I: 렌더링 소개](tutorial--assembling-the-rendering-pipeline.md)
- [렌더링 프레임 워크 II: 게임 렌더링](tutorial-game-rendering.md)
- [사용자 인터페이스 추가](tutorial--adding-a-user-interface.md)
- [컨트롤 추가](tutorial--adding-controls.md)
- [소리 추가](tutorial--adding-sound.md)
