---
title: 주 게임 개체 정의
description: 이제 게임 샘플의 주 개체 및 해당 개체가 구현하는 규칙이 게임 월드와 조작하도록 변환되는 방식을 자세히 살펴봅니다.
ms.assetid: 6afeef84-39d0-cb78-aa2e-2e42aef936c9
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 주 개체
ms.localizationpriority: medium
ms.openlocfilehash: a3c47f3c22c41e7ca73c8a8b5d4e26dc27fab343
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367657"
---
# <a name="define-the-main-game-object"></a>주 게임 개체 정의

샘플 게임의 기본 프레임 워크를 배치 하 고 높은 수준의 사용자 및 시스템 동작을 처리 하는 상태 시스템을 구현 하면, 규칙 및 게임 샘플 게임을 설정 하는 메커니즘을 검사 하는 것이 좋습니다. 게임 샘플의 기본 개체의 세부 정보 및 게임 규칙 게임 세계를 상호 변환 하는 방법에 살펴보겠습니다.

>[!Note]
>이 샘플의 최신 게임 코드를 다운로드하지 않은 경우 [Direct3D 게임 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)로 이동합니다. 이 샘플은 UWP 기능 샘플의 큰 컬렉션의 일부입니다. 샘플을 다운로드하는 방법에 대한 지침은 [GitHub에서 UWP 샘플 가져오기](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)를 참조하세요.

## <a name="objective"></a>목표

게임 규칙 및 UWP DirectX 게임에 대 한 메커니즘을 구현 하기 위한 기본 개발 기술을 적용 하는 방법에 알아봅니다.

## <a name="main-game-object"></a>주 게임 개체

이 샘플 게임 __Simple3DGame__ 기본 게임 개체 클래스입니다. 인스턴스의 __Simple3DGame__ 개체에서 생성 되는 __App::Load__ 메서드.

합니다 __Simple3DGame__ 클래스 개체:
* 게임 플레이 논리의 구현을 지정합니다
* 통신 하는 메서드가 포함 되어 있습니다.
    * 응용 프로그램 프레임 워크에 정의 된 상태 시스템에 게임 상태에서 변경 내용입니다.
    * 게임 개체 자체에 앱에서 게임 상태 변경 내용입니다.
    * 게임의 UI (오버레이 및 헤드업 디스플레이), 애니메이션 및 물리학 (dynamics) 업데이트 하는 것에 대 한 세부 정보입니다.

    >[!Note]
    >그래픽의 업데이트를 처리 합니다 __GameRenderer__ 게임에서 사용 하는 그래픽 장치 리소스를 사용 하는 메서드를 포함 하는 클래스입니다. 자세한 내용은 참조 하세요. [렌더링 프레임 워크 i: 렌더링에 대 한 소개](tutorial--assembling-the-rendering-pipeline.md)합니다.

* 게임 세션 수준에서 정의 하는 데이터 또는 높은 수준에서 게임을 정의 하는 방법에 따라 수명에 대 한 컨테이너로 사용 됩니다. 이 예에서 게임 상태 데이터를 게임의 수명 동안 이며 한 번 초기화 되는 사용자가 게임을 시작 하는 경우.

메서드 및이 클래스 개체에 정의 된 데이터를 보려면로 이동 [Simple3DGame 개체](#simple3dgame-object)합니다.

## <a name="initialize-and-start-the-game"></a>초기화 하 고 게임 시작

플레이어가 게임을 시작하면 게임 오브젝트에서 해당 상태를 초기화하고 오버레이를 만들어 추가하며 플레이어의 성과를 추적하는 변수를 설정하고 레벨을 구성하는 데 사용할 개체를 인스턴스화해야 합니다. 이 샘플에서는 이렇게 경우 새 __GameMain__ 의 인스턴스가 만들어질 [ __App::Load__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L115-L123)합니다. 

게임 개체 __Simple3DGame__, 만들어집니다 합니다 __GameMain__ 생성자입니다. 사용 하 여 초기화 한 다음를 [ __Simple3DGame::Initialize__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L54-L250) 중 메서드를 [비동기 만들기 작업에는 __GameMain__ 생성자](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L65-L74).

### <a name="simple3dgameinitialize-method"></a>Simple3DGame::Initialize 메서드

게임 샘플 게임 개체의 다음 구성 요소를 설정 합니다.

* 새 오디오 재생 개체를 만듭니다.
* 레벨 기본 요소, 탄환 및 장애물 배열을 비롯한 게임의 그래픽 기본 요소에 대한 배열을 만듭니다.
* 게임 상태 데이터를 저장할 위치(*Game*)를 만들고 [**ApplicationData::Current**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.current)에 지정된 앱 데이터 설정 저장 위치에 배치합니다.
* 게임 타이머와 초기 게임 내 오버레이 비트맵을 만듭니다.
* 특정 보기 및 특정 투영 매개 변수 집합을 사용하여 새 카메라를 만듭니다.
* 입력 장치(컨트롤러)가 카메라와 동일한 시작 피치 및 요로 설정되어 플레이어의 시작 제어 위치와 카메라 위치 간에 1대1 대응이 되도록 합니다.
* 플레이어 개체를 만들고 활성 상태로 설정합니다. 플레이어의 근접성 벽 및 장애물 때문에 검색 하 고 집중 교육 중단 될 수 있는 위치에 배치 하기에서 카메라를 유지 하 구 개체를 사용 합니다.
* 게임 월드의 기본 요소를 만듭니다.
* 원통형 장애물을 만듭니다.
* 타겟(**Face** 개체)을 만들고 각각에 대해 번호를 지정합니다.
* 탄환을 만듭니다.
* 레벨을 만듭니다.
* 최고 점수를 로드합니다.
* 이전에 저장된 게임 상태를 로드합니다.

이제 게임에 월드, 플레이어, 장애물, 타겟 및 탄환 같은 모든 주요 구성 요소의 인스턴스가 있습니다. 또한 위의 모든 구성 요소 및 각 레벨별 해당 동작에 대한 구성을 나타내는 레벨 인스턴스도 있습니다. 게임에서 레벨을 구성하는 방법을 살펴보겠습니다.

## <a name="build-and-load-game-levels"></a>빌드 및 부하 게임 수준

대부분의 어려운 수준 생성 이루어집니다에 대 한는 __Level.h/.cpp__ 에서 파일을 찾을 합니다 __GameLevels__ 샘플 솔루션의 폴더입니다. 매우 구체적인 구현에 중점을 둡니다, 때문에에서는 없습니다 다루며, 이러한 여기 중요한 점은 각 레벨에 대한 코드가 별도의 __LevelN__ 개체로 실행된다는 점입니다. 게임을 확장 하려는 경우 만들 수 있습니다는 **수준** 개체를 매개 변수로 이동 하 고 임의로 할당된 된 번호를 사용 하는 장애물 및 대상에 배치 합니다. 또는 리소스 파일 또는 심지어 인터넷에서 수준 구성 데이터를 로드 하도록 할 수도 있습니다.

## <a name="define-the-game-play"></a>게임 플레이 정의 합니다.

이제 게임을 어셈블하는 데 필요한 모든 구성 요소가 있습니다. 수준을 기본 형식에서 메모리에 생성 된 및 상호 작용 하려면 플레이어에 대 한 준비가 되었습니다.

사항 최상의 게임 플레이어 입력에 즉시 대응할 및 즉각적인 피드백을 제공 합니다. 이것이 twitch-작업을 실시간 인칭 shooters 신중 하 게, 턴 기반 전략 게임에서 게임, 모든 형식에 대해 true입니다.

### <a name="simple3dgamerungame-method"></a>Simple3DGame::RunGame 메서드

게임에는 수준을 재생할 때 합니다 __Dynamics__ 상태입니다. 

[__GameMain::Update__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329) 은 아래와 같이 프레임당 한 번 응용 프로그램 상태를 업데이트 하는 주요 업데이트 루프입니다. 업데이트 루프에서 호출 합니다 [ __Simple3DGame::RunGame__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418) 게임이 되는 경우 작업을 처리 하는 메서드를 __Dynamics__ 상태.

```cpp
// Updates the application state once per frame.
void GameMain::Update()
{
    m_controller->Update(); //the controller instance has its own update loop.

    switch (m_updateState)
    {
        //...

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            //...
        }
        else
        {
            GameState runState = m_game->RunGame(); //when playing a level, the game is in the Dynamics state. Work is handled by Simple3DGame::RunGame method.
            switch (runState)
            {
                
      //...
```
          
[__Simple3DGame::RunGame__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418) 게임 루프의 현재 반복에 대 한 게임의 현재 상태를 정의 하는 데이터의 집합을 처리 합니다.

흐름 논리에서 게임 __RunGame__:
*  해당 수준이 완료될 때까지 메서드에서 시간(초)을 계산하는 타이머를 업데이트하고 해당 레벨의 시간이 만료되었는지 테스트하여 확인합니다. 이 게임의 규칙 중 하나입니다: 시간이 다 되 고 모든 대상 촬영 된 것을 때를 통해 게임.
*  시간이 만료되면 메서드에서 **TimeExpired** 게임 상태를 설정하고 이전 코드의 **Update** 메서드로 반환합니다.
*  시간이 남으면 카메라 위치 업데이트, 특히 카메라 평면(플레이어가 보고 있는 곳)에서 투영되는 보기 법선의 각도와 컨트롤러가 폴링된 이전 시간으로부터 각도가 이동한 거리에 대한 업데이트를 위해 이동-보기 컨트롤러가 폴링됩니다.
*  이동-보기 컨트롤러에서 새 데이터를 기반으로 카메라가 업데이트됩니다.
*  역학 또는 플레이어 제어와 독립적인 게임 월드에 있는 개체의 애니메이션 및 동작이 업데이트됩니다. 이 게임 샘플에서는 합니다 [ __UpdateDynamics()__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856) 메서드를 호출 하는 탄약 구 pillar 찾으면의 애니메이션 및 대상의 이동을 발생 되는 동작을 업데이트 합니다. 자세한 내용은 [게임 세계를 업데이트](#update-the-game-world)합니다.
*  메서드에서 성공적인 레벨 완료 조건을 충족했는지 확인합니다. 충족된 경우 레벨에 대한 점수를 마무리하고 6개 레벨 중 마지막 레벨인지 확인합니다. 마지막 레벨이면 메서드에서 **GameComplete** 게임 상태를 반환하고, 그렇지 않으면 __LevelComplete__ 게임 상태를 반환합니다.
*  레벨이 완료되지 않은 경우 메서드에서 게임 상태를 __Active__로 설정하고 반환합니다.

## <a name="update-the-game-world"></a>게임 세계를 업데이트 합니다.

이 샘플에서는 게임을 실행 하는 경우는 [ __Simple3DGame::UpdateDynamics()__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856) 에서 메서드를 호출 합니다 [ __Simple3DGame::RunGame__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418)메서드 (에서 이라고 [ __GameMain::Update__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329)) 게임 장면에서 렌더링 되는 개체를 업데이트 합니다.

에 __UpdateDynamics__ 루프, 진행, 플레이어가 입력할 무관 게임 세계를 설정 하는 데 사용 되는 메서드는 몰입 형 게임 환경을 만들고 제공 하는 수준 호출 *활성*합니다. 플레이어 입력 하지 않고 필요한 경우에 전 세계 숨을 쉬게 생활을 실현 하는 루프를 실행 중인 애니메이션 및 그래픽 렌더링 해야 하는 포함 됩니다. 예를 들어, 트리는 바람에 swaying 해안 선과 시스템 번호를 확장 하 고 이동 외계인 몬스터 따라 cresting 웨이브 합니다. 또한 플레이어 구와 월드 간의 충돌이나 탄약과 장애물 및 타겟과의 충돌을 포함한 개체 간의 조작도 포함합니다.

게임 루프는 항상 일반 인지 또는 게임 논리를 실제 알고리즘에 기반 하는지 여부를 게임 세계를 업데이트 게임 특히 일시 중지 되 면 제외 하 고 임의 유지 합니다. 

게임 샘플에서는 이 파이프라인을 *역학*이라고 하며 기둥 장애물의 올라가고 내려가는 동작 및 발사된 탄환의 모션 및 물리적 동작을 포함합니다. 

### <a name="simple3dgameupdatedynamics-method"></a>Simple3DGame::UpdateDynamics 메서드 

이 메서드는 네 가지 계산 집합을 처리합니다.

* 월드에서 발사한 탄약 구형의 위치
* 필터 장애물의 애니메이션
* 플레이어와 월드 경계의 교차
* 장애물, 타겟, 기타 탄약 구형 및 월드와 탄약 구형의 충돌

장애물의 애니메이션은 **Animate.h/.cpp**에 정의된 루프입니다. 탄약 및 모든 충돌의 동작 간단한 물리학 알고리즘에서 정의 되, 코드에서 제공 되 고 중력 및 재질 속성을 포함 하 여 게임 세계에 대 한 전역 상수 집합으로 매개 변수화 합니다. 이러한 요소는 모두 게임 월드 좌표 공간에서 계산됩니다.

### <a name="review-the-flow"></a>흐름을 검토 합니다.

장면에 있는 모든 개체를 업데이트 하 고 모든 충돌을 계산 했으므로, 이제는이 정보를 사용 하 여 해당 시각적 개체가 바뀌면서 그릴 해야 합니다. 

이후에 __GameMain::Update()__ 현재 반복을 완료 게임 루프의 샘플 즉시 호출 __render ()__ 업데이트 된 개체 데이터를 가져와 플레이어를 제공 하는 새 장면을 생성 여기에 표시 합니다. 다음으로 렌더링에 살펴보겠습니다.

```cpp
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
                // ...
                // otherwise fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update(); // GameMain::Update calls Simple3DGame::RunGame() if game is in Dynamics state, uses Simple3DGame::UpdateDynamics() to update game world.
                m_renderer->Render(); //Render() is called immediately after the Update() loop
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

## <a name="render-the-game-worlds-graphics"></a>게임 세계 그래픽 렌더링

게임의 그래픽은 가능하면 자주 즉, 최대 주 게임 루프가 반복될 때마다 업데이트되는 것이 좋습니다. 루프가 반복되면 플레이어 입력에 관계없이 계임이 업데이트됩니다. 따라서 계산되는 애니메이션 및 동작을 원활하게 표시할 수 있습니다. 플레이어가 단추를 누를 때만 물이 흐르는 간단한 장면을 상상해 보세요. 엄청 지루하게 보일 것입니다. 뛰어난 게임은 원활하고 유동적으로 보입니다.

위의 샘플 게임 루프를 회수 [ __GameMain::Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L143-L202)합니다. 게임의 주 창이 표시되고 스냅되거나 비활성화되지 않은 경우 게임이 계속 업데이트되어 해당 업데이트 결과를 렌더링합니다. 합니다 [ __렌더링__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameRenderer.cpp#L474-L624) 메서드를 검사 하는 것에 이제 해당 상태의으로 렌더링 합니다. 호출한 후 즉시 이루어집니다 **업데이트**를 포함 하는 **RunGame** 이전 섹션에서 설명 했던 업데이트 상태를 합니다.

이 메서드는 3D 월드의 투영을 그린 다음 Direct2D 오버레이를 그 위에 그립니다. 완료되면 결합된 버퍼와 함께 최종 스왑 체인을 제공하여 표시합니다.

>[!Note]
>샘플 게임의 Direct2D 오버레이 대 한 두 가지 상태는: 게임이 일시 중지 메뉴와 게임에 터치 스크린이 이동-모양에 대 한 사각형 함께 십자 표시 되는 위치에 대 한 비트맵을 포함 하는 게임 정보 오버레이 표시 하는 위치 하나 컨트롤러입니다. 점수 텍스트는 이 두 상태로 그립니다. 자세한 내용은 참조 하세요. [렌더링 프레임 워크 i: 렌더링에 대 한 소개](tutorial--assembling-the-rendering-pipeline.md)합니다.

### <a name="gamerendererrender-method"></a>GameRenderer::Render 메서드

```cpp
void GameRenderer::Render()
{
    bool stereoEnabled = m_deviceResources->GetStereoState();

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();
   
        // ...
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            //...
            auto objects = m_game->RenderObjects();
            for (auto object = objects.begin(); object != objects.end(); object++)
            {
                (*object)->Render(d3dContext, m_constantBufferChangesEveryPrim.Get()); // Renders the 3D objects
            }

        //...
        d3dContext->BeginEventInt(L"D2D BeginDraw", 1);
        d2dContext->BeginDraw(); //Start drawing the overlays
        
        // To handle the swapchain being pre-rotated, set the D2D transformation to include it.
        d2dContext->SetTransform(m_deviceResources->GetOrientationTransform2D());

        if (m_game != nullptr && m_gameResourcesLoaded)
        {
            // This is only used after the game state has been initialized.
            m_gameHud->Render(m_game); // Renders number of hits, shots, and time
        }

        if (m_gameInfoOverlay->Visible())
        {
            d2dContext->DrawBitmap(     // Renders the game overlay
                m_gameInfoOverlay->Bitmap(),
                m_gameInfoOverlayRect
                );
        }
        //...
    }
}
```

## <a name="simple3dgame-object"></a>Simple3DGame 개체

메서드 및에 정의 된 데이터를 __Simple3DGame__ 개체 클래스입니다.

### <a name="methods"></a>메서드

에 정의 된 내부 메서드 **Simple3DGame** 포함:

-   **초기화**: 전역 변수의 시작 값을 설정하고 게임 개체를 초기화합니다. 설명 합니다 [초기화 하 고 게임 시작](#initialize-and-start-the-game) 섹션입니다.
-   **LoadGame**: 새 레벨을 초기화하고 로드를 시작합니다.
-   **LoadLevelAsync**: 비동기 작업을 시작 (비동기 작업을 사용 하 여 잘 모르는 경우 [병렬 패턴 라이브러리](https://docs.microsoft.com/cpp/parallel/concrt/parallel-patterns-library-ppl)) 수준을 초기화 하 고 다음 장치 특정 수준 리소스를 로드 하는 렌더러의 비동기 작업을 호출 하 합니다. 이 메서드는 별도의 스레드에서 실행되므로 [**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) 메서드가 아니라 [**ID3D11Device**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11device) 메서드만 이 스레드에서 호출할 수 있습니다. 모든 디바이스 컨텍스트 메서드는 **FinalizeLoadLevel** 메서드에서 호출합니다.
-   **FinalizeLoadLevel**: 주 스레드에서 수행해야 하는 수준 로드를 위한 모든 작업을 완료합니다. 여기에는 Direct3D 11 디바이스 컨텍스트([**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)) 메서드 호출이 포함됩니다.
-   **StartLevel**: 새 레벨에 대한 게임 플레이를 시작합니다.
-   **PauseGame**: 게임을 일시 중지합니다.
-   **RunGame**: 게임 루프의 반복을 실행합니다. 이 메서드는 게임 상태가 **Active**인 경우 게임 루프가 반복될 때마다 한 번 **App::Update**에서 호출됩니다.
-   **OnSuspending** 하 고 **OnResuming**: 각각 게임의 오디오를 일시 중단하고 다시 시작합니다.

private 메서드:

-   **LoadSavedState** 하 고 **SaveState**: 각각 게임의 현재 상태를 로드하고 저장합니다.
-   **SaveHighScore** 하 고 **LoadHighScore**: 각각 게임에서 최고 점수를 저장하고 로드합니다.
-   **InitializeAmmo**: 탄약으로 사용되는 각 구형 개체의 상태를 각 라운드 시작의 원래 상태로 재설정합니다.
-   **UpdateDynamics**: 만든 애니메이션 루틴, 물리학 및 컨트롤 입력을 기준으로 모든 게임 개체를 업데이트하기 때문에 이것은 중요한 메서드입니다. 게임을 정의하는 대화형 작업의 핵심입니다. 설명 합니다 [게임 세계를 업데이트](#update-the-game-world) 섹션입니다.

다른 public 메서드는 게임 플레이 및 오버레이 관련 정보를 앱 프레임워크에 반환하여 표시하는 속성 getter입니다.

### <a name="data"></a>data

코드 예제의 맨 위에는 게임 루프가 실행될 때 인스턴스가 업데이트되는 4개 개체가 있습니다.

-   **MoveLookController** 개체: 플레이어 입력을 나타냅니다. 자세한 내용은 [컨트롤 추가](tutorial--adding-controls.md)를 참조하세요.
-   **GameRenderer** 개체: 파생 된 Direct3D 11 렌더러 나타냅니다 합니다 **DirectXBase** 모든 장치별 개체 및 해당 렌더링을 처리 하는 클래스입니다. 자세한 내용은 [렌더링 프레임 워크 있습니까](tutorial--assembling-the-rendering-pipeline.md)합니다.
-   **오디오** 개체: 게임에 대 한 오디오 재생을 제어합니다. 자세한 내용은 [소리를 추가](tutorial--adding-sound.md)합니다.

나머지 게임 변수에는 원형 목록과 해당 원형의 게임 내 양, 게임 플레이 관련 데이터, 제약 조건 등이 포함됩니다.

## <a name="next-steps"></a>다음 단계

이제 알아보겠습니다 실제 렌더링 엔진에 대 한: 하는 방법에 대 한 호출를 __렌더링__ 메서드는 업데이트 된 기본 형식에서 가져올 화면의 픽셀 전환 합니다. 두 부분으로 다룹니다 &mdash; [렌더링 프레임 워크 i: 렌더링에 대 한 소개](tutorial--assembling-the-rendering-pipeline.md) 고 [렌더링 프레임 워크 II: 게임 렌더링](tutorial-game-rendering.md)합니다. 플레이어 컨트롤에서 게임 상태를 업데이트하는 방법이 자세히 알고 싶으면 [컨트롤 추가](tutorial--adding-controls.md)를 확인하세요.
