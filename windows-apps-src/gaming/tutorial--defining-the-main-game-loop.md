---
title: 주 게임 개체 정의
description: 이제 게임 샘플의 주 개체 및 해당 개체가 구현하는 규칙이 게임 월드와 조작하도록 변환되는 방식을 자세히 살펴봅니다.
ms.assetid: 6afeef84-39d0-cb78-aa2e-2e42aef936c9
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 주 개체
ms.localizationpriority: medium
ms.openlocfilehash: 96aefc8b053dd7490f47910ca5bb79989855e1a3
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8708971"
---
# <a name="define-the-main-game-object"></a>주 게임 개체 정의

샘플 게임의 기본 프레임 워크를 구축 하 고 상위 수준의 사용자 및 시스템 동작을 처리 하는 상태 시스템을 구현 했습니다, 규칙 및 기술과 게임 샘플 게임을 설정 하는 검사 하는 것이 좋습니다. 게임 샘플의 주 개체의 세부 정보 및 게임 월드와의 상호 작용을 게임 규칙을 번역 하는 방법에 대해 살펴보겠습니다.

>[!Note]
>이 샘플의 최신 게임 코드를 다운로드하지 않은 경우 [Direct3D 게임 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)로 이동합니다. 이 샘플은 UWP 기능 샘플의 큰 컬렉션의 일부입니다. 샘플을 다운로드하는 방법에 대한 지침은 [GitHub에서 UWP 샘플 가져오기](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)를 참조하세요.

## <a name="objective"></a>목표

게임 규칙 및 기술과 UWP DirectX 게임에 대 한 구현 하기 위한 기본 개발 기술을 적용 하는 방법을 알아봅니다.

## <a name="main-game-object"></a>주 게임 개체

이 샘플 게임에서 __Simple3DGame__ 주 게임 개체 클래스입니다. __App:: load__ 메서드에서 __Simple3DGame__ 개체의 인스턴스가 생성 됩니다.

__Simple3DGame__ 클래스 개체.
* 게임 플레이 논리의 구현을 지정합니다
* 통신 하는 메서드도를 포함 되어 있습니다.
    * 앱 프레임 워크에 정의 된 상태 시스템에 게임 상태의 변경 합니다.
    * 앱에서 게임 상태를 게임 개체 자체를 변경 합니다.
    * 게임의 UI (오버레이 및 주의 표시), 애니메이션 및 물리학 (역학) 업데이트에 대해 자세히 알아봅니다.

    >[!Note]
    >그래픽 업데이트 게임에서 사용 되는 그래픽 장치 리소스를 사용 하는 메서드가 포함 된 __GameRenderer__ 클래스에 의해 처리 됩니다. 자세한 내용은 [렌더링 프레임 워크 i: 렌더링 소개를](tutorial--assembling-the-rendering-pipeline.md)참조 하세요.

* 게임 세션, 레벨을 정의 하는 데이터 또는 상위 수준에서 게임을 정의 하는 방법에 따라 수명에 대 한 컨테이너 역할을 합니다. 이 경우 게임 상태 데이터는 게임의 수명 및 사용자가 게임을 시작할 때 한 번 초기화 됩니다.

메서드와이 클래스 개체에 정의 된 데이터를 보려면 [Simple3DGame 개체](#simple3dgame-object)를 이동 합니다.

## <a name="initialize-and-start-the-game"></a>초기화 하 고 게임을 시작 합니다.

플레이어가 게임을 시작하면 게임 오브젝트에서 해당 상태를 초기화하고 오버레이를 만들어 추가하며 플레이어의 성과를 추적하는 변수를 설정하고 레벨을 구성하는 데 사용할 개체를 인스턴스화해야 합니다. 이 샘플에서는 [__app:: load__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L115-L123)에 새 __GameMain__ 인스턴스를 만들 때 수행 됩니다. 

게임 개체 __Simple3DGame__ __GameMain__ 생성자에서 만들어집니다. 그런 다음 [비동기 만들기 __GameMain__ 생성자에서 작업](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L65-L74)하는 동안 [__simple3dgame:: initialize__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L54-L250) 메서드를 사용 하 여 초기화 됩니다.

### <a name="simple3dgameinitialize-method"></a>Simple3dgame:: initialize 메서드

게임 샘플 게임 개체에는 다음 구성 설정:

* 새 오디오 재생 개체를 만듭니다.
* 레벨 기본 요소, 탄환 및 장애물 배열을 비롯한 게임의 그래픽 기본 요소에 대한 배열을 만듭니다.
* 게임 상태 데이터를 저장할 위치(*Game*)를 만들고 [**ApplicationData::Current**](https://msdn.microsoft.com/library/windows/apps/br241619)에 지정된 앱 데이터 설정 저장 위치에 배치합니다.
* 게임 타이머와 초기 게임 내 오버레이 비트맵을 만듭니다.
* 특정 보기 및 특정 투영 매개 변수 집합을 사용하여 새 카메라를 만듭니다.
* 입력 장치(컨트롤러)가 카메라와 동일한 시작 피치 및 요로 설정되어 플레이어의 시작 제어 위치와 카메라 위치 간에 1대1 대응이 되도록 합니다.
* 플레이어 개체를 만들고 활성 상태로 설정합니다. 벽과 장애물 플레이어의 근접 정도 감지 하 고 몰입도 문제가 발생할 수 있는 위치에 배치 하기에서 카메라를 유지 하는 구형 개체를 사용 합니다.
* 게임 월드의 기본 요소를 만듭니다.
* 원통형 장애물을 만듭니다.
* 타겟(**Face** 개체)을 만들고 각각에 대해 번호를 지정합니다.
* 탄환을 만듭니다.
* 레벨을 만듭니다.
* 최고 점수를 로드합니다.
* 이전에 저장된 게임 상태를 로드합니다.

이제 게임에 월드, 플레이어, 장애물, 타겟 및 탄환 같은 모든 주요 구성 요소의 인스턴스가 있습니다. 또한 위의 모든 구성 요소 및 각 레벨별 해당 동작에 대한 구성을 나타내는 레벨 인스턴스도 있습니다. 게임에서 레벨을 구성하는 방법을 살펴보겠습니다.

## <a name="build-and-load-game-levels"></a>빌드 및 로드 게임 수준

대부분의 번거로운 작업 수준 구성에 대 한 샘플 솔루션의 __GameLevels__ 폴더에 있는 __Level.h/.cpp__ 파일에서 수행 됩니다. 매우 구체적인 구현에 집중 하므로에서는 없습니다 다루며 하 여기 중요한 점은 각 레벨에 대한 코드가 별도의 __LevelN__ 개체로 실행된다는 점입니다. 게임을 확장 하려는 경우 할당된 된 번호를 매개 변수로 사용 하 고 장애물을 임의로 배치 하는 **수준** 개체를 만들 수 있습니다. 또는 리소스 파일이 나 인터넷에서 레벨 구성 데이터를 로드 하도록 할 수도 있습니다.

## <a name="define-the-game-play"></a>게임 플레이 정의

이제 게임을 어셈블하는 데 필요한 모든 구성 요소가 있습니다. 레벨 원형에서 메모리에 구성 했으므로 되며 플레이어가 특정 조작을 시작할 수 있습니다.

구성 제품 최상의 게임은 플레이어 입력에 바로 반응 하 고 즉각적인 피드백을 제공 합니다. 실시간-1 인치 슈팅 기반의 전략 게임에 이르는-작업에서 모든 종류는 게임의 경우도 마찬가지입니다.

### <a name="simple3dgamerungame-method"></a>Simple3DGame::RunGame 메서드

레벨을 재생할 때 게임이 __Dynamics__ 상태입니다. 

[__GameMain::Update__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329) 아래와 같이 프레임 마다 한 번 응용 프로그램 상태를 업데이트 하는 주요 업데이트 루프입니다. 업데이트 루프에서 게임이 __Dynamics__ 상태에 있으면 작업을 처리 하도록 [__Simple3DGame::RunGame__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418) 메서드를 호출 합니다.

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
          
[__Simple3DGame::RunGame__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418) 게임 루프의 현재 반복에 대 한 게임 플레이의 현재 상태를 정의 하는 데이터 집합을 처리 합니다.

게임 흐름 논리를 __RunGame__:
*  해당 수준이 완료될 때까지 메서드에서 시간(초)을 계산하는 타이머를 업데이트하고 해당 레벨의 시간이 만료되었는지 테스트하여 확인합니다. 이것은 게임 규칙 중 하나: 게임 오버가 시간이 초과 하는 경우 모든 대상 촬영 되어 왔습니다.
*  시간이 만료되면 메서드에서 **TimeExpired** 게임 상태를 설정하고 이전 코드의 **Update** 메서드로 반환합니다.
*  시간이 남으면 카메라 위치 업데이트, 특히 카메라 평면(플레이어가 보고 있는 곳)에서 투영되는 보기 법선의 각도와 컨트롤러가 폴링된 이전 시간으로부터 각도가 이동한 거리에 대한 업데이트를 위해 이동-보기 컨트롤러가 폴링됩니다.
*  이동-보기 컨트롤러에서 새 데이터를 기반으로 카메라가 업데이트됩니다.
*  역학 또는 플레이어 제어와 독립적인 게임 월드에 있는 개체의 애니메이션 및 동작이 업데이트됩니다. 이 게임 샘플에서는 업데이트 된 탄환의 모션, 기둥 장애물의 애니메이션 및 타겟의 이동 하 [__UpdateDynamics()__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856) 메서드가 호출 됩니다. 자세한 내용은 [게임 월드 업데이트](#update-the-game-world)를 참조 하세요.
*  메서드에서 성공적인 레벨 완료 조건을 충족했는지 확인합니다. 충족된 경우 레벨에 대한 점수를 마무리하고 6개 레벨 중 마지막 레벨인지 확인합니다. 마지막 레벨이면 메서드에서 **GameComplete** 게임 상태를 반환하고, 그렇지 않으면 __LevelComplete__ 게임 상태를 반환합니다.
*  레벨이 완료되지 않은 경우 메서드에서 게임 상태를 __Active__로 설정하고 반환합니다.

## <a name="update-the-game-world"></a>게임 월드 업데이트

이 샘플에서는 게임이 실행 되는 경우는 [__Simple3DGame::UpdateDynamics()__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856) 메서드가 호출 됩니다 ( [__GameMain::Update__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329)에서 라고 함)는 [__Simple3DGame::RunGame__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418) 메서드에서 게임 장면에서 렌더링 되는 개체 업데이트.

__UpdateDynamics__ 루프에서 동작, 플레이어의 독립 게임 월드를 설정 하는 데 사용 되는 메서드를 호출 입력 몰입 형 게임 환경을 만들고 *구현이 되도록*수준을 확인 합니다. 그래픽 렌더링 해야 하는 포함 하 고 플레이어 입력이 없으면 필요한 경우에 월드 숨을 쉬게 생활에 대 한 상태로 실행 중인 애니메이션을 반복 합니다. 나무, 기계 스모 킹 확장 및 돌아다니는 외계인 몬스터 따라 cresting 물결 바람에 swaying 트리 예를 들어 있습니다. 또한 플레이어 구와 월드 간의 충돌이나 탄약과 장애물 및 타겟과의 충돌을 포함한 개체 간의 조작도 포함합니다.

게임 루프는 항상 기반 게임 논리를 물리적 알고리즘으로 있는지 또는 일반에 이르기까지 게임 월드 업데이트 임의 게임이 특별히 일시 중지 경우를 제외 하 고 유지 합니다. 

게임 샘플에서는 이 파이프라인을 *역학*이라고 하며 기둥 장애물의 올라가고 내려가는 동작 및 발사된 탄환의 모션 및 물리적 동작을 포함합니다. 

### <a name="simple3dgameupdatedynamics-method"></a>Simple3DGame::UpdateDynamics 메서드 

이 메서드는 네 가지 계산 집합을 처리합니다.

* 월드에서 발사한 탄약 구형의 위치
* 필터 장애물의 애니메이션
* 플레이어와 월드 경계의 교차
* 장애물, 타겟, 기타 탄약 구형 및 월드와 탄약 구형의 충돌

장애물의 애니메이션은 **Animate.h/.cpp**에 정의된 루프입니다. 탄약과 모든 충돌의 동작은 간단한 물리학 알고리즘으로 정의 되, 코드에서 제공 되 고 중력 및 재료 속성을 비롯 한 게임 월드에 대 한 전역 상수 집합으로 매개 변수화 합니다. 이러한 요소는 모두 게임 월드 좌표 공간에서 계산됩니다.

### <a name="review-the-flow"></a>흐름 검토

이제는 장면의 모든 개체를 업데이트 하 고 모든 충돌을 계산에이 정보를 사용 하 여 해당 시각적 변경 사항을 그립니다 해야 합니다. 

__GameMain::Update()__ 현재 게임 루프의 반복 완료 되 면 샘플 업데이트 된 개체 데이터를 가져오고 새 장면을 플레이어에 게 표시를 생성 하 여 다음과 같이 하 __render ()__ 즉시 호출 합니다. 다음으로 렌더링에 대해 살펴보겠습니다.

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

## <a name="render-the-game-worlds-graphics"></a>게임 월드의 그래픽 렌더링

게임의 그래픽은 가능하면 자주 즉, 최대 주 게임 루프가 반복될 때마다 업데이트되는 것이 좋습니다. 루프가 반복되면 플레이어 입력에 관계없이 계임이 업데이트됩니다. 따라서 계산되는 애니메이션 및 동작을 원활하게 표시할 수 있습니다. 플레이어가 단추를 누를 때만 물이 흐르는 간단한 장면을 상상해 보세요. 엄청 지루하게 보일 것입니다. 뛰어난 게임은 원활하고 유동적으로 보입니다.

[__Gamemain:: Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L143-L202)에 위에 표시 된 대로 샘플 게임의 루프를 회수 합니다. 게임의 주 창이 표시되고 스냅되거나 비활성화되지 않은 경우 게임이 계속 업데이트되어 해당 업데이트 결과를 렌더링합니다. [__Render__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameRenderer.cpp#L474-L624) 메서드를 검사 하는 것에 이제 해당 상태의 표현을 렌더링 합니다. **업데이트**, **RunGame** 상태를 업데이트 하려면 이전 섹션에서 설명한 포함 하는 호출 직후 수행 됩니다.

이 메서드는 3D 월드의 투영을 그린 다음 Direct2D 오버레이를 그 위에 그립니다. 완료되면 결합된 버퍼와 함께 최종 스왑 체인을 제공하여 표시합니다.

>[!Note]
>샘플 게임의 Direct2D 오버레이 대 한 두 가지 상태: 게임 일시 중지 메뉴와 하나는 게임 터치 스크린 이동-보기에 대 한 사각형과 함께 십자 모양을 표시에 대 한 비트맵을 포함 하는 게임 정보 오버레이 표시 하는 위치 중 하나 컨트롤러입니다. 점수 텍스트는 이 두 상태로 그립니다. 자세한 내용은 [렌더링 프레임워크 I: 렌더링 소개](tutorial--assembling-the-rendering-pipeline.md)를 참조하세요.

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

이들은 메서드와 __Simple3DGame__ 개체 클래스에 정의 된 데이터입니다.

### <a name="methods"></a>메서드

**Simple3DGame** 에 정의 된 내부 메서드는 다음과 같습니다.

-   **초기화**: 전역 변수의 시작 값을 설정 하 고 게임 개체를 초기화 합니다. 이 [초기화 하 고 게임을 시작](#initialize-and-start-the-game) 섹션에서 설명 합니다.
-   **LoadGame**: 새 레벨을 초기화 하 고 로드를 시작 합니다.
-   **LoadLevelAsync**: 비동기 작업을 시작 (비동기 작업으로 잘 모르는 경우 참조 [병렬 패턴 라이브러리](https://docs.microsoft.com/cpp/parallel/concrt/parallel-patterns-library-ppl)) 하 여 수준을 초기화 한 다음 렌더러에서 디바이스 관련 수준 리소스를 로드 하는 비동기 작업을 호출 합니다. 이 메서드는 별도의 스레드에서 실행되므로 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) 메서드가 아니라 [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) 메서드만 이 스레드에서 호출할 수 있습니다. 모든 디바이스 컨텍스트 메서드는 **FinalizeLoadLevel** 메서드에서 호출합니다.
-   **FinalizeLoadLevel**: 주 스레드에서 수행 해야 하는 수준 로드를 위한 모든 작업을 완료 합니다. 여기에는 Direct3D 11 디바이스 컨텍스트([**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)) 메서드 호출이 포함됩니다.
-   **StartLevel**: 새 레벨에 대 한 게임 플레이 시작 합니다.
-   **PauseGame**: 게임을 일시 중지 합니다.
-   **RunGame**: 게임 루프의 반복을 실행 합니다. 이 메서드는 게임 상태가 **Active**인 경우 게임 루프가 반복될 때마다 한 번 **App::Update**에서 호출됩니다.
-   **OnSuspending** 및 **OnResuming**: 일시 중단 하 고 각각 게임의 오디오를 다시 시작 합니다.

private 메서드:

-   **LoadSavedState** 및 **SaveState**: 로드 하 고 각각 게임의 현재 상태를 저장 합니다.
-   **SaveHighScore** 및 **LoadHighScore**: 저장 하 고 각각 게임에서 최고 점수를 로드 합니다.
-   **InitializeAmmo**: 각 라운드 시작의 원래 상태로 재설정으로 사용 되는 각 구형 개체의 상태를 초기화 합니다.
-   **UpdateDynamics**: 만든된 애니메이션 루틴, 물리학 및 컨트롤 입력에 따라 모든 게임 개체를 업데이트 하기 때문에이 중요 한 메서드입니다. 게임을 정의하는 대화형 작업의 핵심입니다. 이 [게임 월드 업데이트](#update-the-game-world) 섹션에서 설명 합니다.

다른 public 메서드는 게임 플레이 및 오버레이 관련 정보를 앱 프레임워크에 반환하여 표시하는 속성 getter입니다.

### <a name="data"></a>데이터

코드 예제의 맨 위에는 게임 루프가 실행될 때 인스턴스가 업데이트되는 4개 개체가 있습니다.

-   **MoveLookController** 개체: 플레이어 입력을 나타냅니다. 자세한 내용은 [컨트롤 추가](tutorial--adding-controls.md)를 참조하세요.
-   **GameRenderer** 개체: 모든 디바이스 관련 개체 및 렌더링을 처리 하는 **DirectXBase** 클래스에서 파생 된 Direct3D 11 렌더러를 나타냅니다. 자세한 내용은 참조 [렌더링 프레임 워크 I](tutorial--assembling-the-rendering-pipeline.md)합니다.
-   **오디오** 개체: 게임의 오디오 재생을 제어 합니다. 자세한 내용은 [추가 사운드](tutorial--adding-sound.md)를 참조 하세요.

나머지 게임 변수에는 원형 목록과 해당 원형의 게임 내 양, 게임 플레이 관련 데이터, 제약 조건 등이 포함됩니다.

## <a name="next-steps"></a>다음 단계

지금 알아보겠습니다 실제 렌더링 엔진에 대 한: 어떻게 업데이트 된 원형에 __렌더링__ 메서드 호출을 화면에 픽셀로 설정 가져오기. 이 두 부분으로 설명 &mdash; [렌더링 프레임 워크 i: 렌더링 소개](tutorial--assembling-the-rendering-pipeline.md) 하 고 [렌더링 프레임 워크 II: 게임 렌더링](tutorial-game-rendering.md)합니다. 플레이어 컨트롤에서 게임 상태를 업데이트하는 방법이 자세히 알고 싶으면 [컨트롤 추가](tutorial--adding-controls.md)를 확인하세요.
