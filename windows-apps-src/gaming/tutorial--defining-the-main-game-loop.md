---
title: 주 게임 개체 정의
description: 이제 샘플 게임의 주 개체에 대 한 세부 정보 및 구현 하는 규칙이 게임 세계와의 상호 작용으로 변환 되는 방식을 살펴보겠습니다.
ms.assetid: 6afeef84-39d0-cb78-aa2e-2e42aef936c9
ms.date: 06/24/2020
ms.topic: article
keywords: windows 10, uwp, 게임, 주 개체
ms.localizationpriority: medium
ms.openlocfilehash: 497a1f0dc16308b4b9360aff958b94f04b6283ae
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162997"
---
# <a name="define-the-main-game-object"></a>주 게임 개체 정의

> [!NOTE]
> 이 항목은 DirectX 자습서 시리즈 [를 사용 하 여 단순 유니버설 Windows 플랫폼 (UWP) 게임 만들기](tutorial--create-your-first-uwp-directx-game.md) 의 일부입니다. 해당 링크의 항목은 계열의 컨텍스트를 설정 합니다.

샘플 게임의 기본 프레임 워크를 소개 하 고 높은 수준의 사용자 및 시스템 동작을 처리 하는 상태 시스템을 구현한 후에는 샘플 게임을 게임으로 전환 하는 규칙 및 메커니즘을 검토 하는 것이 좋습니다. 샘플 게임의 주 개체에 대 한 세부 정보를 살펴보고 게임 규칙을 게임과의 상호 작용으로 변환 하는 방법을 살펴보겠습니다.

## <a name="objectives"></a>목표

- UWP DirectX 게임의 게임 규칙 및 메커니즘을 구현 하는 기본 개발 기술을 적용 하는 방법을 알아봅니다.

## <a name="main-game-object"></a>주 게임 개체

**Simple3DGameDX** 샘플 게임에서 **Simple3DGame** 는 주 게임 개체 클래스입니다. **Simple3DGame** 인스턴스는 **App:: Load** 메서드를 통해 간접적으로 생성 됩니다.

**Simple3DGame** 클래스의 몇 가지 기능은 다음과 같습니다.

- 게임 플레이 논리의 구현을 포함 합니다.
- 이러한 세부 정보를 전달 하는 메서드를 포함 합니다.
  - 게임 상태를 앱 프레임 워크에 정의 된 상태 시스템으로 변경 합니다.
  - 게임 상태를 앱에서 게임 개체 자체에 변경 합니다.
  - 게임 UI (오버레이 및 준비 디스플레이), 애니메이션 및 물리학 (dynamics) 업데이트에 대 한 세부 정보입니다.
  > [!NOTE]
  > 그래픽의 업데이트는 게임에서 사용 되는 그래픽 장치 리소스를 가져오고 사용 하는 메서드가 포함 된 **GameRenderer** 클래스에 의해 처리 됩니다. 자세한 정보는 [렌더링 프레임 워크 I: 렌더링 소개](tutorial--assembling-the-rendering-pipeline.md)를 참조 하세요.
- 높은 수준에서 게임을 정의 하는 방법에 따라 게임 세션, 수준 또는 수명을 정의 하는 데이터에 대 한 컨테이너 역할을 합니다. 이 경우 게임 상태 데이터는 게임의 수명에 사용 되며 사용자가 게임을 시작할 때 한 번 초기화 됩니다.

이 클래스에 의해 정의 된 메서드 및 데이터를 보려면 아래 [Simple3DGame 클래스](#the-simple3dgame-class) 를 참조 하세요.

## <a name="initialize-and-start-the-game"></a>게임 초기화 및 시작

플레이어가 게임을 시작할 때 game 개체는 해당 상태를 초기화 하 고, 오버레이를 만들어 추가 하 고, 플레이어의 성능을 추적 하는 변수를 설정 하 고, 수준을 작성 하는 데 사용할 개체를 인스턴스화해야 합니다. 이 샘플에서는 **App:: Load**에서 **GameMain** 인스턴스를 만들 때이 작업을 수행 합니다.

**Simple3DGame**형식의 game 개체는 **GameMain:: GameMain** 생성자에 생성 됩니다. 그런 다음 **GameMain:: GameMain**에서 호출 되는 **GameMain:: ConstructInBackground** fire 및-잊어버린 코 루틴 중에 **Simple3DGame:: Initialize** 메서드를 사용 하 여 초기화 됩니다.

### <a name="the-simple3dgameinitialize-method"></a>Simple3DGame:: Initialize 메서드

샘플 게임은 게임 개체에서 이러한 구성 요소를 설정 합니다.

- 새 오디오 재생 개체가 만들어집니다.
- 게임의 그래픽 기본 형식의 배열은 수준 기본 형식, 탄약 및 장애물의 배열을 포함 하 여 만들어집니다.
- 게임 상태 데이터를 저장 하는 위치는 *game*이며 [**Applicationdata:: Current**](/uwp/api/windows.storage.applicationdata.current)로 지정 된 앱 데이터 설정 저장소 위치에 배치 됩니다.
- 게임 타이머와 초기 게임 내 오버레이 비트맵이 생성 됩니다.
- 특정 뷰 및 프로젝션 매개 변수 집합을 사용 하 여 새 카메라가 만들어집니다.
- 입력 장치 (컨트롤러)는 카메라와 동일한 시작 피치 및 요 설정 되므로 플레이어는 시작 컨트롤 위치와 카메라 위치 사이에 일대일 대응 관계가 있습니다.
- 플레이어 개체가 만들어지고 활성으로 설정 됩니다. 구 개체를 사용 하 여 벽 및 장애물에 대 한 플레이어의 근접성을 감지 하 고 집중 교육 중단 될 수 있는 위치에 카메라를 배치 하지 않도록 합니다.
- 게임 세계 기본 형식이 만들어집니다.
- 원통 장애가 생성 됩니다.
- 대상 (**Face** 개체)이 만들어지고 번호가 매겨집니다.
- 탄약 구를 만듭니다.
- 수준이 생성 됩니다.
- 높은 점수가 로드 됩니다.
- 이전에 저장 된 게임 상태가 모두 로드 됩니다.

이제 게임에는 &mdash; 세계, 플레이어, 장애물, 대상 및 탄약의 모든 주요 구성 요소 인스턴스가 있습니다. 또한 위의 모든 구성 요소에 대 한 구성 및 각 특정 수준에 대 한 동작을 나타내는 수준의 인스턴스가 있습니다. 이제 게임에서 수준을 빌드하는 방법을 알아보겠습니다.

## <a name="build-and-load-game-levels"></a>게임 수준 빌드 및 로드

수준 생성의 대부분은 `Level[N].h/.cpp` 샘플 솔루션의 **GameLevels** 폴더에 있는 파일에서 수행 됩니다. 매우 구체적인 구현에 중점을 두는 것은 여기에서 다루지 않습니다. 중요 한 점은 각 수준의 코드가 별도의 **수준 [N]** 개체로 실행 된다는 것입니다. 게임을 확장 하려는 경우 할당 된 숫자를 매개 변수로 사용 하 고 장애물 및 대상을 임의로 배치 하는 **수준 [N]** 개체를 만들 수 있습니다. 또는 리소스 파일 또는 인터넷 에서도이를 로드 수준 구성 데이터를 사용할 수 있습니다.

## <a name="define-the-gameplay"></a>게임 플레이 정의

이 시점에서 게임을 개발 하는 데 필요한 모든 구성 요소가 있습니다. 이 수준은 기본 형식에서 메모리로 생성 되었으며 플레이어와의 상호 작용을 시작할 준비가 되었습니다.

최상의 게임은 플레이어 입력에 즉시 반응 하 고 즉각적인 피드백을 제공 합니다. 이는 twitch, 실시간 최초 사용자 shooters에서 사전에 있는 전략 게임에 이르기까지 모든 유형의 게임에 적용 됩니다.

### <a name="the-simple3dgamerungame-method"></a>Simple3DGame:: RunGame 메서드

게임 수준이 진행 중인 동안에는 게임이 **Dynamics** 상태에 있습니다. 

**GameMain:: Update** 는 아래와 같이 프레임당 한 번 응용 프로그램 상태를 업데이트 하는 주 업데이트 루프입니다. 업데이트 루프는 **Simple3DGame:: rungame** 메서드를 호출 하 여 게임이 **Dynamics** 상태인 경우 작업을 처리 합니다.

```cppwinrt
// Updates the application state once per frame.
void GameMain::Update()
{
    // The controller object has its own update loop.
    m_controller->Update();

    switch (m_updateState)
    {
    ...
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
                ...
```
          
**Simple3DGame:: RunGame** 은 게임 루프의 현재 반복에 대 한 게임 플레이의 현재 상태를 정의 하는 데이터 집합을 처리 합니다.

**Simple3DGame:: RunGame**의 게임 흐름 논리는 다음과 같습니다.

- 메서드는 수준이 완료 될 때까지 초를 계산 하는 타이머를 업데이트 하 고 수준 시간이 만료 되었는지 테스트 합니다. 이는 시간이 초과 될 때 게임의 규칙 중 하나입니다 &mdash; . 모든 대상이 샷 되지 않은 경우 게임이 진행 됩니다.
- 시간이 초과 되 면 메서드는 **Timeexpired** 된 게임 상태를 설정 하 고 이전 코드에서 **Update** 메서드로 돌아갑니다.
- 시간이 남아 있으면 카메라 위치에 대 한 업데이트에 대 한 이동 확인 컨트롤러를 폴링합니다. 특히 카메라 평면에서 정상으로 표시 되는 보기의 각도 (플레이어를 찾고 있는 위치)에 대 한 업데이트 및 컨트롤러가 마지막으로 폴링 된 이후 각도가 이동 된 거리입니다.
- 카메라는 이동 보기 컨트롤러의 새 데이터를 기반으로 업데이트 됩니다.
- 플레이어 컨트롤과 독립적인 게임 세계의 개체에 대 한 dynamics 또는 애니메이션 및 동작이 업데이트 됩니다. 이 샘플 게임에서 **Simple3DGame:: UpdateDynamics** 메서드를 호출 하 여 발생 한 탄약 구, 기둥 장애물 애니메이션 및 대상 이동의 움직임을 업데이트 합니다. 자세한 내용은 [game 세계 업데이트](#update-the-game-world)를 참조 하세요.
- 메서드는 수준을 성공적으로 완료 하기 위한 조건이 충족 되었는지 확인 합니다. 이 경우 수준에 대 한 점수를 마무리 하 고 마지막 수준 (6) 인지 여부를 확인 합니다. 마지막 수준인 경우 메서드는 **GameState:: GameComplete** game 상태를 반환 합니다. 그렇지 않으면 **GameState:: LevelComplete** 게임 상태를 반환 합니다.
- 수준이 완전 하지 않은 경우 메서드는 게임 상태를 **GameState:: Active**로 설정 하 고를 반환 합니다.

## <a name="update-the-game-world"></a>게임 세계 업데이트

이 샘플에서 게임을 실행 하는 경우 **Simple3DGame:: UpdateDynamics** 메서드가 **Simple3DGame:: rungame** 메서드 ( **GameMain:: Update**에서 호출 됨)에서 호출 되어 게임 장면에서 렌더링 되는 개체를 업데이트 합니다.

**UpdateDynamics** 와 같은 루프는 플레이어 입력에 독립적으로 게임 세계를 설정 하는 데 사용 되는 모든 메서드를 호출 하 여 몰입 형 게임 환경을 만들고 수준을 유지 하도록 합니다. 여기에는 렌더링 해야 하는 그래픽이 포함 되며, 플레이어 입력이 없는 경우에도 동적 세계를 위해 애니메이션 루프를 실행 합니다. 게임에서 swaying 나무를 포함 하 여 물결이, cresting 흡연, 외계인 몬스터를 확장 하 고 이동할 수 있습니다. 또한 플레이어 구와 전 또는 탄약과 장애물 및 대상 간의 충돌을 포함 하 여 개체 간 상호 작용을 포함 합니다.

게임을 특별히 일시 중지 한 경우를 제외 하 고 게임 루프는 게임 세계를 계속 업데이트 해야 합니다. 게임 논리를 기반으로 하는지, 실제 알고리즘을 기반으로 하는지 아니면 단순한 지 여부를 결정 합니다.

샘플 게임에서는이 원칙을 dynamics 라고 하며,이 원칙을 *dynamics*장애물 및 동작에서 발생 하는 탄약의 동작 및 물리적 동작을 포함 합니다.

### <a name="the-simple3dgameupdatedynamics-method"></a>Simple3DGame:: UpdateDynamics 메서드 

이 메서드는 이러한 4 개의 계산 집합을 처리 합니다.

- 전 세계에서 발생 한 탄약 구 위치입니다.
- 기둥 장애물 애니메이션입니다.
- 플레이어와 세계 경계의 교집합입니다.
- 장애물, 대상, 기타 탄약 구 및 전 세계의 탄약에 대 한 충돌입니다.

장애물 애니메이션은에 대 한 애니메이션 **. h/.cpp** 소스 코드 파일에 정의 된 루프에서 발생 합니다. 탄약의 동작과 모든 충돌은 코드에서 제공 되 고 무게 및 자재 속성을 비롯 한 게임 세계의 전역 상수 집합으로 매개 변수화 되는 단순화 된 물리학 알고리즘에 의해 정의 됩니다. 이는 모두 게임 세계 좌표 공간에서 계산 됩니다.

### <a name="review-the-flow"></a>흐름 검토

이제 장면에서 모든 개체를 업데이트 하 고 충돌을 계산 했으므로이 정보를 사용 하 여 해당 하는 시각적 변경을 그려야 합니다. 

**GameMain:: Update** 가 게임 루프의 현재 반복을 완료 하 고 나면 샘플은 아래와 같이 업데이트 된 개체 데이터를 가져오고 플레이어에 게 제공할 새 장면을 생성 하는 즉시 **GameRenderer:: Render** 를 호출 합니다.

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
                ...
                // Otherwise, fall through and do normal processing to perform rendering.
            default:
                CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(
                    CoreProcessEventsOption::ProcessAllIfPresent);
                // GameMain::Update calls Simple3DGame::RunGame. If game is in Dynamics
                // state, uses Simple3DGame::UpdateDynamics to update game world.
                Update();
                // Render is called immediately after the Update loop.
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(
                CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close, so save state.
}
```

## <a name="render-the-game-worlds-graphics"></a>게임 세계의 그래픽을 렌더링 합니다.

게임 업데이트에서 그래픽을 자주 반복 하는 것이 좋습니다. 루프가 반복 되 면 게임 세계의 상태는 플레이어 입력을 사용 하거나 사용 하지 않고 업데이트 됩니다. 그러면 계산 된 애니메이션 및 동작이 부드럽게 표시 될 수 있습니다. 플레이어가 단추를 누른 경우에만 이동 하는 간단한 워터 장면을 생각해 보겠습니다. 현실적이 지 않습니다. 좋은 게임은 항상 부드러운 및 유체를 보입니다.

**GameMain:: Run**에서 위에 표시 된 것 처럼 샘플 게임의 루프를 회수 합니다. 게임의 주 창이 표시 되 고,이 창이 표시 되지 않거나 비활성화 되지 않은 경우 게임은 해당 업데이트의 결과를 계속 업데이트 하 고 렌더링 합니다. 다음을 검사 하는 **GameRenderer:: Render** 메서드는 해당 상태의 표현을 렌더링 합니다. 이전 섹션에서 설명한 대로 상태를 업데이트 하기 위해 **Simple3DGame:: RunGame** 을 포함 하는 **GameMain:: update**를 호출한 후 즉시 수행 됩니다.

**GameRenderer:: Render** 는 3d 세계의 프로젝션을 그린 다음 그 위에 Direct2D 오버레이를 그립니다. 완료 되 면 표시를 위해 결합 된 버퍼를 사용 하 여 최종 스왑 체인을 표시 합니다.

> [!NOTE]
> 샘플 게임의 Direct2D 오버레이에는 두 가지 상태가 있습니다 &mdash; . 즉, 게임은 일시 중지 메뉴에 대 한 비트맵이 포함 된 게임 정보 오버레이를 표시 하 고 다른 하나는 터치 스크린 보기 컨트롤러의 사각형과 함께 십자 기호를 표시 합니다. 점수 텍스트는 두 상태로 그려집니다. 자세한 내용은 [렌더링 프레임 워크 I: 렌더링 소개](tutorial--assembling-the-rendering-pipeline.md)를 참조 하세요.

### <a name="the-gamerendererrender-method"></a>GameRenderer:: Render 메서드

```cppwinrt
void GameRenderer::Render()
{
    bool stereoEnabled{ m_deviceResources->GetStereoState() };

    auto d3dContext{ m_deviceResources->GetD3DDeviceContext() };
    auto d2dContext{ m_deviceResources->GetD2DDeviceContext() };

    ...
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            ...
            for (auto&& object : m_game->RenderObjects())
            {
                object->Render(d3dContext, m_constantBufferChangesEveryPrim.get());
            }
        }

        d3dContext->BeginEventInt(L"D2D BeginDraw", 1);
        d2dContext->BeginDraw();

        // To handle the swapchain being pre-rotated, set the D2D transformation to include it.
        d2dContext->SetTransform(m_deviceResources->GetOrientationTransform2D());

        if (m_game != nullptr && m_gameResourcesLoaded)
        {
            // This is only used after the game state has been initialized.
            m_gameHud.Render(m_game);
        }

        if (m_gameInfoOverlay.Visible())
        {
            d2dContext->DrawBitmap(
                m_gameInfoOverlay.Bitmap(),
                m_gameInfoOverlayRect
                );
        }
        ...
    }
}
```

## <a name="the-simple3dgame-class"></a>Simple3DGame 클래스

이러한 메서드는 **Simple3DGame** 클래스에 의해 정의 되는 메서드 및 데이터 멤버입니다.

### <a name="member-functions"></a>멤버 함수

**Simple3DGame** 에 의해 정의 되는 공용 멤버 함수에는 아래와 같은 항목이 포함 됩니다.

- **초기화**. 전역 변수의 시작 값을 설정 하 고 게임 개체를 초기화 합니다. 이 내용은 [게임 초기화 및 시작](#initialize-and-start-the-game) 섹션에서 다룹니다.
- **Loadgame**. 새 수준을 초기화 하 고 로드를 시작 합니다.
- **Loadlevelasync**. 수준을 초기화 하 고 렌더러에 대해 다른 코 루틴를 호출 하 여 장치별 수준 리소스를 로드 하는 코 루틴입니다. 이 메서드는 별도의 스레드에서 실행 됩니다. 따라서 [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) 메서드와 달리 [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) 메서드만이 스레드에서 호출할 수 있습니다. 모든 장치 컨텍스트 메서드는 **FinalizeLoadLevel** 메서드에서 호출 됩니다. 비동기 프로그래밍을 처음 접하는 경우에는 [c + +/WinRT를 사용한 동시성 및 비동기 작업](../cpp-and-winrt-apis/concurrency.md)을 참조 하세요.
- **FinalizeLoadLevel**. 주 스레드에서 수행 해야 하는 수준 로드 작업을 완료 합니다. 여기에는 Direct3D 11[**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)(장치 컨텍스트) 메서드에 대 한 호출이 포함 됩니다.
- **Startlevel**. 새 수준의 게임 플레이를 시작 합니다.
- **Pausegame**. 게임을 일시 중지 합니다.
- **Rungame**. 게임 루프의 반복을 실행 합니다. 게임 상태가 **활성**인 경우 game 루프를 반복할 때마다 한 번 **App:: Update** 에서 호출 됩니다.
- **Onsuspending 중단** 및 **onsuspending**. 게임의 오디오를 각각 일시 중단/다시 시작 합니다.

Private 멤버 함수는 다음과 같습니다.

- **LoadSavedState** 및 **savestate**. 게임의 현재 상태를 각각 로드/저장 합니다.
- **Loadhighscore** 및 **savehighscore**를 계산 합니다. 게임에서 각각 높은 점수를 로드/저장 합니다.
- **Initializeammo**. 각 왕복이 시작 될 때 탄약로 사용 되는 각 구 개체의 상태를 원래 상태로 다시 설정 합니다.
- **UpdateDynamics**. 이 방법은 고정 애니메이션 루틴, 물리 및 컨트롤 입력을 기반으로 모든 게임 개체를 업데이트 하기 때문에 중요 합니다. 게임을 정의 하는 상호 작용의 핵심입니다. 이 내용은 [게임 세계 업데이트](#update-the-game-world) 섹션에서 다룹니다.

다른 공용 메서드는 디스플레이를 위해 게임 프레임 워크에 대 한 세부 정보 및 오버레이 정보를 반환 하는 속성 접근자입니다.

### <a name="data-members"></a>데이터 멤버

이러한 개체는 게임 루프가 실행 될 때 업데이트 됩니다.

- **MoveLookController** 개체입니다. 플레이어 입력을 나타냅니다. 자세한 내용은 [컨트롤 추가](tutorial--adding-controls.md)를 참조 하세요.
- **GameRenderer** 개체입니다. 모든 장치 관련 개체와 렌더링을 처리 하는 Direct3D 11 렌더러를 나타냅니다. 자세한 내용은 [프레임 워크 렌더링](tutorial--assembling-the-rendering-pipeline.md)을 참조 하세요.
- **Audio** 개체입니다. 게임의 오디오 재생을 제어 합니다. 자세한 내용은 [소리 추가](tutorial--adding-sound.md)를 참조 하세요.

게임 변수의 나머지 부분에는 기본 형식 목록, 각각의 게임 금액 및 게임 특정 데이터와 제약 조건이 포함 됩니다.

## <a name="next-steps"></a>다음 단계

&mdash;업데이트 된 기본 형식에서 **렌더링** 메서드를 호출 하는 것이 화면에서 픽셀로 설정 되는 방식을 실제 렌더링 엔진에 대해 아직 설명 하지 않았습니다. 이러한 측면은 &mdash; [렌더링 프레임 워크 I: 렌더링](tutorial--assembling-the-rendering-pipeline.md) 및 [렌더링 프레임 워크 II: 게임 렌더링](tutorial-game-rendering.md)의 두 부분에서 다룹니다. 플레이어 컨트롤이 게임 상태를 업데이트 하는 방법에 더 관심이 있는 경우 [컨트롤 추가](tutorial--adding-controls.md)를 참조 하세요.