---
title: Marble Maze 응용 프로그램 구조
description: UWP (DirectX 유니버설 Windows 플랫폼) 앱의 구조는 기존 데스크톱 응용 프로그램의 구조와 다릅니다.
ms.assetid: 6080f0d3-478a-8bbe-d064-73fd3d432074
ms.date: 09/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 샘플, directx, 구조
ms.localizationpriority: medium
ms.openlocfilehash: e4dd33bb40b84db79e3ac2ea43a4252b6e20d441
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165247"
---
# <a name="marble-maze-application-structure"></a>Marble Maze 응용 프로그램 구조




UWP (DirectX 유니버설 Windows 플랫폼) 앱의 구조는 기존 데스크톱 응용 프로그램의 구조와 다릅니다. [HWND](/windows/desktop/WinProg/windows-data-types) 와 같은 핸들 형식으로 작업 하는 [대신, Windows 런타임](/windows/desktop/api/winuser/nf-winuser-createwindowa)은 [Windows:: UI:: Core:: ICoreWindow](/uwp/api/Windows.UI.Core.ICoreWindow) 와 같은 인터페이스를 제공 하므로, 더 현대적인 개체 지향 방식으로 UWP 앱을 개발할 수 있습니다. 설명서의이 섹션에서는 대리석의 응용 프로그램 코드를 구성 하는 방법을 보여 줍니다.

> [!NOTE]
> 이 문서에 해당 하는 샘플 코드는 [DirectX 대리석 미로 game 샘플](https://github.com/microsoft/Windows-appsample-marble-maze)에서 찾을 수 있습니다.

 
## 
게임 코드를 구성할 때이 문서에서 설명 하는 주요 사항은 다음과 같습니다.

-   초기화 단계에서 게임에 사용 되는 런타임 및 라이브러리 구성 요소를 설정 하 고 게임 특정 리소스를 로드 합니다.
-   UWP 앱은 시작 후 5 초 이내에 이벤트 처리를 시작 해야 합니다. 따라서 앱을 로드할 때 필수 리소스만 로드 합니다. 게임은 백그라운드에서 많은 리소스를 로드 하 고 진행률 화면을 표시 합니다.
-   게임 루프에서 Windows 이벤트에 응답 하 고, 사용자 입력을 읽고, 장면 개체를 업데이트 하 고, 장면을 렌더링 합니다.
-   이벤트 처리기를 사용 하 여 창 이벤트에 응답 합니다. 이는 데스크톱 Windows 응용 프로그램에서 창 메시지를 대체 합니다.
-   상태 시스템을 사용하여 게임 논리의 흐름과 순서를 제어합니다.

##  <a name="file-organization"></a>파일 조직


대리석의 일부 구성 요소는 약간의 수정 없이 모든 게임과 재사용할 수 있습니다. 자신의 게임을 위해 이러한 파일에서 제공 하는 조직 및 아이디어를 수정할 수 있습니다. 다음 표에서는 중요 한 소스 코드 파일에 대해 간략하게 설명 합니다.

| Files                                      | Description                                                                                                                                                                          |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| App-v, App-v               | 앱의 뷰 (창, 스레드 및 이벤트)를 캡슐화 하는 **앱** 및 **Directxapplicationsource** 클래스를 정의 합니다.                                                     |
| 오디오만, Audio .cpp                         | 오디오 리소스를 관리 하는 **오디오** 클래스를 정의 합니다.                                                                                                                          |
| BasicLoader .h, BasicLoader .cpp             | 질감, 메시 및 셰이더를 로드 하는 데 도움이 되는 유틸리티 메서드를 제공 하는 **Basicloader** 클래스를 정의 합니다.                                                                  |
| BasicMath .h                                | 벡터 및 행렬 데이터와 계산을 사용 하는 데 도움이 되는 구조체 및 함수를 정의 합니다. 이러한 함수 중 상당수는 HLSL 셰이더 형식과 호환 됩니다.                     |
| BasicReaderWriter .h, BasicReaderWriter .cpp | Windows 런타임를 사용 하 여 UWP 앱에서 파일 데이터를 읽고 쓰는 **Basicreaderwriter** 클래스를 정의 합니다.                                                                    |
| BasicShapes .h, BasicShapes .cpp             | 큐브와 구 등의 기본 셰이프를 만드는 유틸리티 메서드를 제공 하는 **Basicshapes** 클래스를 정의 합니다. 이러한 파일은 대리석 미로 구현에서 사용 되지 않습니다. |                                                                                  |
| 카메라 .h, node.js                       | 카메라의 위치와 방향을 제공 하는 **카메라** 클래스를 정의 합니다.                                                                                               |
| 충돌. h, 충돌 .cpp                 | 는 다른 개체 (예: 미로)와 대리석 사이의 충돌 정보를 관리 합니다.                                                                                                       |
| DDSTextureLoader, DDSTextureLoader   | 메모리 버퍼에서 dds 형식의 질감을 로드 하는 **CreateDDSTextureFromMemory** 함수를 정의 합니다.                                                              |
| DirectXHelper .h             | 많은 DirectX UWP 앱에 유용한 DirectX 도우미 함수를 정의 합니다.                                                                            |
| LoadScreen, LoadScreen               | 앱 초기화 중 로드 화면을 표시 하는 **loadscreen** 클래스를 정의 합니다.                                                                                         |
| MarbleMazeMain, MarbleMazeMain               | 게임 특정 리소스를 관리 하 고 많은 게임 논리를 정의 하는 **MarbleMazeMain** 클래스를 정의 합니다.                                                                          |
| MediaStreamer, MediaStreamer         | 게임에서 오디오 리소스를 관리할 수 있도록 미디어 파운데이션를 사용 하는 **MediaStreamer** 클래스를 정의 합니다.                                                                            |
| 만들면 persistentstate, 만들면 persistentstate     | 백업 저장소에서 기본 데이터 형식을 읽고 쓰는 **만들면 persistentstate** 클래스를 정의 합니다.                                                                      |
| 물리학, 물리학                     | 대리석과 메 이즈 사이의 물리 시뮬레이션을 구현 하는 **물리학** 클래스를 정의 합니다.                                                                              |
| 기본 .h                               | 게임에서 사용 되는 기하학적 형식을 정의 합니다.                                                                                                                                   |
| SampleOverlay, SampleOverlay         | 공용 2D 및 사용자 인터페이스 데이터 및 작업을 제공 하는 **SampleOverlay** 클래스를 정의 합니다.                                                                               |
| SDKMesh, SDKMesh                     | SDK 메시 (. SDKMesh) 형식의 메시를 로드 하 고 렌더링 하는 **SDKMesh** 클래스를 정의 합니다.                                                                                |
| Sttimer .h               | 전체 및 경과 시간을 쉽게 가져오는 방법을 제공 하는 **Sttimer** 클래스를 정의 합니다.
| UserInterface .h, UserInterface .cpp         | 메뉴 시스템 및 높은 점수 표 같은 사용자 인터페이스와 관련 된 기능을 정의 합니다.                                                                        |

 

##  <a name="design-time-versus-run-time-resource-formats"></a>디자인 타임 및 런타임 리소스 형식 비교


가능 하면 디자인 타임 형식 대신 런타임 형식을 사용 하 여 게임 리소스를 보다 효율적으로 로드 합니다.

*디자인 타임* 형식은 리소스를 디자인할 때 사용 하는 형식입니다. 일반적으로 3D 디자이너는 디자인 타임 형식으로 작동 합니다. 일부 디자인 타임 형식은 텍스트 기반 편집기에서 수정할 수 있도록 텍스트 기반 이기도 합니다. 디자인 타임 형식은 자세한 정보를 표시 하 고 게임에 필요한 것 보다 많은 정보를 포함할 수 있습니다. *런타임* 형식은 게임에서 읽는 이진 형식입니다. 런타임 형식은 일반적으로 해당 디자인 타임 형식 보다 더 간결 하 고 더 효율적입니다. 이로 인해 대부분의 게임이 런타임에 런타임 자산을 사용 하 게 됩니다.

게임에서 디자인 타임 형식을 직접 읽을 수는 있지만 별도의 런타임 형식을 사용 하는 경우 몇 가지 이점이 있습니다. 런타임 형식이 더 간결 하기 때문에 디스크 공간이 절약 되 고 네트워크를 통해 전송 하는 데 시간이 더 적게 필요 합니다. 또한 런타임 형식은 종종 메모리 매핑된 데이터 구조로 표시 됩니다. 따라서 이러한 파일은 XML 기반 텍스트 파일 등의 메모리에 훨씬 더 빠르게 로드할 수 있습니다. 마지막으로 별도의 런타임 형식이 이진 인코딩 되기 때문에 최종 사용자가 수정 하기가 더 어려워집니다.

HLSL 셰이더는 다양 한 디자인 타임 및 런타임 형식을 사용 하는 리소스의 한 예입니다. 대리석 메 이즈는 hlsl을 디자인 타임 형식으로 사용 하 고 cso를 런타임 형식으로 사용 합니다. Hlsl 파일은 셰이더에 대 한 소스 코드를 포함 합니다. 이 파일에는 해당 셰이더 바이트 코드가 포함 됩니다. Hlsl 파일을 오프 라인으로 변환 하 고 게임을 통해 .csfile 파일을 제공 하는 경우 게임 로드 시 HLSL 원본 파일을 바이트 코드로 변환 하지 않아도 됩니다.

참고로,이에 대 한 설명을 위해 대리석 무늬 메 이즈 프로젝트에는 여러 리소스에 대 한 디자인 타임 형식과 런타임 형식이 모두 포함 되어 있지만 필요할 때 런타임 형식으로 변환할 수 있기 때문에 원본 프로젝트에서 디자인 타임 형식만 유지 관리 하면 됩니다. 이 설명서에서는 디자인 타임 형식을 런타임 형식으로 변환 하는 방법을 보여 줍니다.

##  <a name="application-life-cycle"></a>응용 프로그램 수명 주기


대리석은 일반적인 UWP 앱의 수명 주기를 따릅니다. UWP 앱의 수명 주기에 대 한 자세한 내용은 [앱 수명 주기](../launch-resume/app-lifecycle.md)를 참조 하세요.

UWP 게임은 초기화 될 때 일반적으로 Direct3D, Direct2D 등의 런타임 구성 요소와이를 사용 하는 입력, 오디오 또는 물리학 라이브러리를 초기화 합니다. 또한 게임을 시작 하기 전에 필요한 게임 특정 리소스를 로드 합니다. 이 초기화는 게임 세션 중에 한 번 발생 합니다.

초기화 후 게임은 일반적으로 *게임 루프*를 실행 합니다. 이 루프에서 게임은 일반적으로 Windows 이벤트 처리, 입력 수집, 장면 개체 업데이트 및 장면 렌더링의 네 가지 작업을 수행 합니다. 게임에서 장면을 업데이트할 때 현재 입력 상태를 장면 개체에 적용 하 고 개체 충돌과 같은 실제 이벤트를 시뮬레이션할 수 있습니다. 게임에서 음향 효과를 재생 하거나 네트워크를 통해 데이터를 전송 하는 등의 다른 작업을 수행할 수도 있습니다. 게임에서 장면을 렌더링 하면 장면의 현재 상태를 캡처하여 디스플레이 장치에 그립니다. 다음 섹션에서는 이러한 작업에 대해 더 자세히 설명 합니다.

##  <a name="adding-to-the-template"></a>템플릿에 추가


**DirectX 11 앱 (유니버설 Windows)** 템플릿은 Direct3D로 렌더링할 수 있는 코어 창을 만듭니다. 템플릿에는 UWP 앱에서 3D 콘텐츠를 렌더링 하는 데 필요한 모든 Direct3D 장치 리소스를 만드는 **DeviceResources** 클래스도 포함 되어 있습니다.

**App** 클래스는 **MarbleMazeMain** 클래스 개체를 만들고, 리소스 로드를 시작 하 고, 타이머를 업데이트 하는 루프를 시작 하 고, 각 프레임의 **MarbleMazeMain:: Render** 메서드를 호출 합니다. **App:: OnWindowSizeChanged**, **App:: OnDpiChanged**및 **app:: OnOrientationChanged** 메서드는 각각 **MarbleMazeMain:: CreateWindowSizeDependentResources** 메서드를 호출 하 고 **App:: Run** 메서드는 **MarbleMazeMain:: Update** 및 **MarbleMazeMain:: Render** 메서드를 호출 합니다.

다음 예제에서는 **App:: SetWindow** 메서드가 **MarbleMazeMain** 클래스 개체를 만드는 위치를 보여 줍니다. **DeviceResources** 클래스는 렌더링을 위해 Direct3D 개체를 사용할 수 있도록 메서드에 전달 됩니다.

```cpp
    m_main = std::unique_ptr<MarbleMazeMain>(new MarbleMazeMain(m_deviceResources));
```

또한 **App** 클래스는 게임에 대해 지연 된 리소스를 로드 하기 시작 합니다. 자세한 내용은 다음 섹션을 참조 하세요.

또한 **App** 클래스는 [CoreWindow](/uwp/api/windows.ui.core.corewindow) 이벤트에 대 한 이벤트 처리기를 설정 합니다. 이러한 이벤트에 대 한 처리기가 호출 되 면 입력을 **MarbleMazeMain** 클래스에 전달 합니다.

## <a name="loading-game-assets-in-the-background"></a>백그라운드에서 게임 자산 로드


게임이 시작 된 후 5 초 이내에 창 이벤트에 응답할 수 있도록 하려면 게임 자산을 비동기식으로 또는 백그라운드에서 로드 하는 것이 좋습니다. 자산이 백그라운드에서 로드 되 면 게임이 창 이벤트에 응답할 수 있습니다.

> [!NOTE]
> 주 메뉴가 준비 되 면 표시 하 고 나머지 자산이 백그라운드에서 계속 로드 되도록 허용할 수도 있습니다. 모든 리소스를 로드 하기 전에 사용자가 메뉴에서 옵션을 선택 하는 경우 진행률 표시줄을 표시 하 여 장면 리소스를 계속 로드 하도록 지정할 수 있습니다 (예:).

 

게임에 비교적 적은 게임 자산이 포함 된 경우에도 두 가지 이유로 비동기적으로 로드 하는 것이 좋습니다. 한 가지 이유는 모든 리소스가 모든 장치 및 모든 구성에서 신속 하 게 로드 되도록 보장 하기 어렵기 때문입니다. 또한 비동기 로드를 초기에 통합 하면 기능을 추가할 때 코드를 확장할 준비가 된 것입니다.

비동기 자산 로드는 **App:: Load** 메서드로 시작 합니다. 이 메서드는 [작업](/cpp/parallel/concrt/reference/task-class) 클래스를 사용 하 여 백그라운드에서 게임 자산을 로드 합니다.

```cpp
    task<void>([=]()
    {
        m_main->LoadDeferredResources(true, false);
    });
```

**MarbleMazeMain** 클래스는 비동기 로드가 완료 되었음을 나타내는 *m \_ deferredResourcesReady* 플래그를 정의 합니다. **MarbleMazeMain:: LoadDeferredResources** 메서드는 게임 리소스를 로드 한 다음이 플래그를 설정 합니다. 앱의 업데이트 (**MarbleMazeMain:: update**) 및 렌더링 (**MarbleMazeMain:: render**) 단계는이 플래그를 확인 합니다. 이 플래그가 설정 되 면 게임이 정상적으로 계속 됩니다. 플래그가 아직 설정 되지 않은 경우 게임에서 로드 화면을 표시 합니다.

UWP 앱에 대 한 비동기 프로그래밍에 대 한 자세한 내용은 [c + +의 비동기 프로그래밍](../threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps.md)을 참조 하세요.

> [!TIP]
> Windows 런타임 c + + 라이브러리 (즉, DLL)의 일부인 게임 코드를 작성 하는 경우 [UWP 앱에 대 한 c + +에서 비동기 작업 만들기](/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps) 를 읽을 지 여부를 고려 하 여 앱 및 기타 라이브러리에서 사용할 수 있는 비동기 작업을 만드는 방법을 알아보세요.

 

## <a name="the-game-loop"></a>게임 루프


**App:: Run** 메서드는 주 게임 루프 (**MarbleMazeMain:: Update**)를 실행 합니다. 이 메서드는 모든 프레임에서 호출 됩니다.

게임 관련 코드에서 뷰와 창 코드를 분리 하기 위해 **App:: Run** 메서드를 구현 하 여 **MarbleMazeMain** 개체에 대 한 업데이트 및 렌더링 호출을 전달 했습니다.

다음 예제에서는 주 게임 루프를 포함 하는 **App:: Run** 메서드를 보여 줍니다. 게임 루프는 총 시간 및 프레임 시간 변수를 업데이트 한 다음 장면을 업데이트 하 고 렌더링 합니다. 또한 창이 표시 될 때만 콘텐츠가 렌더링 되도록 합니다.

```cpp
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->
                ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->
                ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }

    // The app is exiting so do the same thing as if the app were being suspended.
    m_main->OnSuspending();

#ifdef _DEBUG
    // Dump debug info when exiting.
    DumpD3DDebug();
#endif //_DEGBUG
}
```

## <a name="the-state-machine"></a>상태 시스템


게임은 일반적으로 게임 논리의 흐름과 순서를 제어 하는 *상태 시스템* ( *유한 상태 시스템*또는 fsm이 라고도 함)을 포함 합니다. 상태 시스템에는 지정 된 수의 상태와 이러한 상태를 전환 하는 기능이 포함 되어 있습니다. 일반적으로 상태 시스템은 *초기* 상태에서 시작 하 고 하나 이상의 *중간* 상태로 전환 되며 *터미널* 상태에서 종료 될 수 있습니다.

게임 루프는 현재 게임 상태와 관련 된 논리를 수행할 수 있도록 상태 시스템을 사용 하는 경우가 많습니다. 대리석 미로는 게임의 가능한 각 상태를 정의 하는 **GameState** 열거형을 정의 합니다.

```cpp
enum class GameState
{
    Initial,
    MainMenu,
    HighScoreDisplay,
    PreGameCountdown,
    InGameActive,
    InGamePaused,
    PostGameResults,
};
```

예를 들어 **MainMenu** 상태는 주 메뉴가 나타나고 게임이 활성화 되지 않은 것을 정의 합니다. 반대로 **Ingameactive** 상태는 게임이 활성 상태이 고 메뉴가 나타나지 않는다는 것을 정의 합니다. **MarbleMazeMain** 클래스는 활성 게임 상태를 유지 하는 **m \_ gameState** 멤버 변수를 정의 합니다.

**MarbleMazeMain:: Update** 및 **MarbleMazeMain:: Render** 메서드는 switch 문을 사용 하 여 현재 상태에 대 한 논리를 수행 합니다. 다음 예제에서는 **MarbleMazeMain:: Update** 메서드 (구조를 설명 하기 위해 세부 정보는 제거 됨)에 대 한 switch 문이 표시 되는 모양을 보여 줍니다.

```cpp
switch (m_gameState)
{
case GameState::MainMenu:
    // Do something with the main menu. 
    break;

case GameState::HighScoreDisplay:
    // Do something with the high-score table. 
    break;

case GameState::PostGameResults:
    // Do something with the game results. 
    break;

case GameState::InGamePaused:
    // Handle the paused state. 
    break;
}
```

게임 논리 또는 렌더링이 특정 게임 상태에 따라 달라 지는 경우이 문서에서 강조 합니다.

## <a name="handling-app-and-window-events"></a>앱 및 창 이벤트 처리


Windows 런타임는 Windows 메시지를 보다 쉽게 관리할 수 있도록 개체 지향 이벤트 처리 시스템을 제공 합니다. 응용 프로그램에서 이벤트를 사용 하려면 이벤트에 응답 하는 이벤트 처리기 또는 이벤트 처리 메서드를 제공 해야 합니다. 또한 이벤트 처리기를 이벤트 소스에 등록 해야 합니다. 이 프로세스를 종종 이벤트 와이어링 이라고 합니다.

### <a name="supporting-suspend-resume-and-restart"></a>일시 중단, 다시 시작 및 다시 시작 지원

대리석은 사용자가 멀리 떨어져 있거나 Windows에서 절전 상태가 될 때 일시 중단 됩니다. 사용자가 전경으로 이동 하거나 Windows가 저전력 상태에서 벗어나면 게임이 다시 시작 됩니다. 일반적으로 앱을 닫지 않습니다. Windows는 일시 중단 된 상태에 있을 때 앱을 종료할 수 있으며, Windows에는 앱에서 사용 하는 것과 같은 리소스 (예: 메모리)가 필요 합니다. Windows는 응용 프로그램이 일시 중단 되거나 다시 시작 될 때 응용 프로그램에 알림을 표시 하지만 종료 될 때 앱에 알리지 않습니다. 따라서 앱이 일시 중단 되 고 있음을 응용 프로그램에 알리는 시점 (앱이 다시 시작 될 때 현재 사용자 상태를 복원 하는 데 필요한 모든 데이터)을 저장할 수 있어야 합니다. 앱이 저장 하는 데 비용이 많이 드는 많은 사용자 상태를 사용 하는 경우 앱이 일시 중단 알림을 받기 전에도 정기적으로 상태를 저장 해야 할 수도 있습니다. 대리석 미로는 다음과 같은 두 가지 이유로 일시 중단 및 다시 시작 알림에 응답 합니다.

1.  앱이 일시 중단 되 면 게임은 현재 게임 상태를 저장 하 고 오디오 재생을 일시 중지 합니다. 앱이 다시 시작 되 면 게임에서 오디오 재생을 다시 시작 합니다.
2.  앱을 닫고 나중에 다시 시작 하면 게임이 이전 상태에서 다시 시작 됩니다.

대리석은 일시 중단 및 다시 시작을 지원 하기 위해 다음 작업을 수행 합니다.

-   사용자가 검사점에 도달 했을 때와 같이 게임의 핵심 지점에서 상태를 영구 저장소에 저장 합니다.
-   영구 저장소에 상태를 저장 하 여 알림을 일시 중단 하는 것에 응답 합니다.
-   영구 저장소에서 해당 상태를 로드 하 여 다시 시작 알림에 응답 합니다. 또한 시작 하는 동안 이전 상태를 로드 합니다.

일시 중단 및 다시 시작을 지원 하기 위해 대리석은 **만들면 persistentstate** 클래스를 정의 합니다. **만들면 persistentstate** 및 **만들면 persistentstate**를 참조 하세요. 이 클래스는 [Windows:: Foundation:: Collections:: IPropertySet](/uwp/api/Windows.Foundation.Collections.IPropertySet) 인터페이스를 사용 하 여 속성을 읽고 씁니다. **만들면 persistentstate** 클래스는 및에서 백업 저장소에 대 한 기본 데이터 형식 (예: **bool**, **int**, **Float**, [XMFLOAT3](/windows/desktop/api/directxmath/ns-directxmath-xmfloat3)및 [Platform:: String](/cpp/cppcx/platform-string-class))을 읽고 쓰는 메서드를 제공 합니다.

```cpp
ref class PersistentState
{
internal:
    void Initialize(
        _In_ Windows::Foundation::Collections::IPropertySet^ settingsValues,
        _In_ Platform::String^ key
        );

    void SaveBool(Platform::String^ key, bool value);
    void SaveInt32(Platform::String^ key, int value);
    void SaveSingle(Platform::String^ key, float value);
    void SaveXMFLOAT3(Platform::String^ key, DirectX::XMFLOAT3 value);
    void SaveString(Platform::String^ key, Platform::String^ string);

    bool LoadBool(Platform::String^ key, bool defaultValue);
    int  LoadInt32(Platform::String^ key, int defaultValue);
    float LoadSingle(Platform::String^ key, float defaultValue);

    DirectX::XMFLOAT3 LoadXMFLOAT3(
        Platform::String^ key, 
        DirectX::XMFLOAT3 defaultValue);

    Platform::String^ LoadString(
        Platform::String^ key, 
        Platform::String^ defaultValue);

private:
    Platform::String^ m_keyName;
    Windows::Foundation::Collections::IPropertySet^ m_settingsValues;
};
```

**MarbleMazeMain** 클래스에는 **만들면 persistentstate** 개체가 포함 되어 있습니다. **MarbleMazeMain** 생성자는이 개체를 초기화 하 고 로컬 응용 프로그램 데이터 저장소를 지원 데이터 저장소로 제공 합니다.

```cpp
m_persistentState = ref new PersistentState();

m_persistentState->Initialize(
    Windows::Storage::ApplicationData::Current->LocalSettings->Values,
    "MarbleMaze");
```

대리석은 대리석이 검사점 또는 목표 ( **MarbleMazeMain:: Update** 메서드에서)를 통과할 때와 창이 포커스를 잃으면 ( **MarbleMazeMain:: OnFocusChange** 메서드에서) 해당 상태를 저장 합니다. 게임에서 많은 양의 상태 데이터를 보유 하는 경우 일시 중단 알림에 응답 하는 데 몇 초 밖에 안 되기 때문에 비슷한 방식으로 영구 저장소에 상태를 저장 하는 것이 좋습니다. 따라서 앱이 일시 중단 알림을 받으면 변경 된 상태 데이터만 저장 하면 됩니다.

일시 중단 및 다시 시작 알림에 응답 하려면 **MarbleMazeMain** 클래스는 일시 중단 및 다시 시작 시 호출 되는 **Savestate** 및 **loadstate** 메서드를 정의 합니다. **MarbleMazeMain:: onsuspending** 메서드는 suspend 이벤트를 처리 하 고 **MarbleMazeMain:: onsuspending** 메서드는 resume 이벤트를 처리 합니다.

**MarbleMazeMain:: OnSuspending 중단** 메서드는 게임 상태를 저장 하 고 오디오를 일시 중단 합니다.

```cpp
void MarbleMazeMain::OnSuspending()
{
    SaveState();
    m_audio.SuspendAudio();
}
```

**MarbleMazeMain:: SaveState** 메서드는 현재 위치 및 대리석 (예: 대리석의 현재 위치 및 속도), 최신 검사점 및 고득점 테이블 등의 게임 상태 값을 저장 합니다.

```cpp
void MarbleMazeMain::SaveState()
{
    m_persistentState->SaveXMFLOAT3(":Position", m_physics.GetPosition());
    m_persistentState->SaveXMFLOAT3(":Velocity", m_physics.GetVelocity());

    m_persistentState->SaveSingle(
        ":ElapsedTime", 
        m_inGameStopwatchTimer.GetElapsedTime());

    m_persistentState->SaveInt32(":GameState", static_cast<int>(m_gameState));
    m_persistentState->SaveInt32(":Checkpoint", static_cast<int>(m_currentCheckpoint));

    int i = 0;
    HighScoreEntries entries = m_highScoreTable.GetEntries();
    const int bufferLength = 16;
    char16 str[bufferLength];

    m_persistentState->SaveInt32(":ScoreCount", static_cast<int>(entries.size()));

    for (auto iter = entries.begin(); iter != entries.end(); ++iter)
    {
        int len = swprintf_s(str, bufferLength, L"%d", i++);
        Platform::String^ string = ref new Platform::String(str, len);

        m_persistentState->SaveSingle(
            Platform::String::Concat(":ScoreTime", string), 
            iter->elapsedTime);

        m_persistentState->SaveString(
            Platform::String::Concat(":ScoreTag", string), 
            iter->tag);
    }
}
```

게임을 다시 시작 하는 경우 오디오를 다시 시작 해야 합니다. 상태는 메모리에 이미 로드 되어 있으므로 영구 저장소에서 상태를 로드할 필요가 없습니다.

게임에서 오디오를 일시 중단 하 고 다시 시작 하는 방법은 [대리석 미로 샘플에 오디오 추가](adding-audio-to-the-marble-maze-sample.md)문서에 설명 되어 있습니다.

다시 시작을 지원 하기 위해 시작 하는 동안 호출 되는 **MarbleMazeMain** 생성자는 **MarbleMazeMain:: loadstate** 메서드를 호출 합니다. **MarbleMazeMain:: loadstate** 메서드는 게임 개체에 상태를 읽고 적용 합니다. 또한이 메서드는 일시 중지 되거나 일시 중지 되었을 때 현재 게임 상태를 일시 중지 됨으로 설정 합니다. 사용자가 예기치 않은 작업을 하지 않도록 게임을 일시 중지 합니다. 일시 중단 된 게임 플레이 상태가 아닌 경우에도 주 메뉴로 이동 합니다.

```cpp
void MarbleMazeMain::LoadState()
{
    XMFLOAT3 position = m_persistentState->LoadXMFLOAT3(
        ":Position", 
        m_physics.GetPosition());

    XMFLOAT3 velocity = m_persistentState->LoadXMFLOAT3(
        ":Velocity", 
        m_physics.GetVelocity());

    float elapsedTime = m_persistentState->LoadSingle(":ElapsedTime", 0.0f);

    int gameState = m_persistentState->LoadInt32(
        ":GameState", 
        static_cast<int>(m_gameState));

    int currentCheckpoint = m_persistentState->LoadInt32(
        ":Checkpoint", 
        static_cast<int>(m_currentCheckpoint));

    switch (static_cast<GameState>(gameState))
    {
    case GameState::Initial:
        break;

    case GameState::MainMenu:
    case GameState::HighScoreDisplay:
    case GameState::PreGameCountdown:
    case GameState::PostGameResults:
        SetGameState(GameState::MainMenu);
        break;

    case GameState::InGameActive:
    case GameState::InGamePaused:
        m_inGameStopwatchTimer.SetVisible(true);
        m_inGameStopwatchTimer.SetElapsedTime(elapsedTime);
        m_physics.SetPosition(position);
        m_physics.SetVelocity(velocity);
        m_currentCheckpoint = currentCheckpoint;
        SetGameState(GameState::InGamePaused);
        break;
    }

    int count = m_persistentState->LoadInt32(":ScoreCount", 0);

    const int bufferLength = 16;
    char16 str[bufferLength];

    for (int i = 0; i < count; i++)
    {
        HighScoreEntry entry;
        int len = swprintf_s(str, bufferLength, L"%d", i);
        Platform::String^ string = ref new Platform::String(str, len);

        entry.elapsedTime = m_persistentState->LoadSingle(
            Platform::String::Concat(":ScoreTime", string), 
            0.0f);

        entry.tag = m_persistentState->LoadString(
            Platform::String::Concat(":ScoreTag", string), 
            L"");

        m_highScoreTable.AddScoreToTable(entry);
    }
}
```

> [!IMPORTANT]
> 대리석 미로는 콜드 시작 (즉, 이전 일시 중단 이벤트 없이 처음부터 시작)을 구분 하지 않으며 일시 중단 된 상태에서 다시 시작 합니다. 모든 UWP 앱에 권장 되는 디자인입니다.

응용 프로그램 데이터에 대 한 자세한 내용은 [설정 및 기타 앱 데이터 저장 및 검색](../design/app-settings/store-and-retrieve-app-data.md)을 참조 하세요.

##  <a name="next-steps"></a>다음 단계


시각적 리소스로 작업할 때 염두에 두어야 하는 몇 가지 주요 방법에 대 한 정보를 보려면 [대리석 미로 샘플에 시각적 콘텐츠 추가](adding-visual-content-to-the-marble-maze-sample.md) 를 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [Marble Maze 샘플에 시각적 콘텐츠 추가](adding-visual-content-to-the-marble-maze-sample.md)
* [Marble Maze 샘플 기본 사항](marble-maze-sample-fundamentals.md)
* [C + + 및 c + +의 UWP 게임, 대리석](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 