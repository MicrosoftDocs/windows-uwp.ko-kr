---
author: eliotcowley
title: Marble Maze 응용 프로그램 구조
description: DirectX UWP(유니버설 Windows 플랫폼) 앱의 구조는 일반적인 데스크톱 응용 프로그램 구조와 다릅니다.
ms.assetid: 6080f0d3-478a-8bbe-d064-73fd3d432074
ms.author: elcowle
ms.date: 09/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 게임, 샘플, directx, 구조
ms.localizationpriority: medium
ms.openlocfilehash: c26b547d5cc94f3277d0c898804e65d75e6d17e2
ms.sourcegitcommit: cceaf2206ec53a3e9155f97f44e4795a7b6a1d78
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2018
ms.locfileid: "1700879"
---
# <a name="marble-maze-application-structure"></a>Marble Maze 응용 프로그램 구조




DirectX UWP(유니버설 Windows 플랫폼) 앱의 구조는 일반적인 데스크톱 응용 프로그램 구조와 다릅니다. [HWND](https://msdn.microsoft.com/library/windows/desktop/aa383751)와 같은 핸들 형식과 [CreateWindow](https://msdn.microsoft.com/library/windows/desktop/ms632679)와 같은 함수로 작업하는 대신 Windows 런타임은 보다 현대적이고 개체 지향적인 방식으로 UWP 앱을 개발할 수 있도록 [Windows::UI::Core::ICoreWindow](https://msdn.microsoft.com/library/windows/apps/br208296)와 같은 인터페이스를 제공합니다. 이 설명서 섹션에서는 Marble Maze 앱 코드가 구성된 방식을 보여 줍니다.

> [!NOTE]
> 이 문서에 해당하는 샘플 코드는 [DirectX Marble Maze 게임 샘플](http://go.microsoft.com/fwlink/?LinkId=624011)에 있습니다.

 
## 
이 문서에서 게임 코드를 구성하는 경우에 대해 논의하는 주요 사항은 다음과 같습니다.

-   초기화 단계에서 게임에 사용되는 런타임 및 라이브러리 구성 요소를 설정하며 게임 관련 리소스도 로드합니다.
-   UWP 앱은 시작 후 5초 내에 이벤트 처리를 시작해야 합니다. 따라서 앱을 로드할 때 중요한 리소스만 로드합니다. 게임은 큰 리소스를 백그라운드에서 로드하고 진행률 화면을 표시해야 합니다.
-   게임 루프에서 Windows 이벤트에 응답하고, 사용자 입력을 읽고, 장면 개체를 업데이트하고, 장면을 렌더링합니다.
-   이벤트 처리기를 사용하여 창 이벤트에 응답합니다. 데스크톱 Windows 응용 프로그램의 창 메시지가 이벤트 처리기로 대체됩니다.
-   상태 시스템을 사용하여 게임 논리의 흐름과 순서를 제어합니다.

##  <a name="file-organization"></a>파일 구성


Marble Maze의 일부 구성 요소를 거의 또는 전혀 수정하지 않고 모든 게임에 다시 사용할 수 있습니다. 이러한 파일이 제공하는 구성과 아이디어를 고유한 게임에 맞게 조정할 수 있습니다. 다음 표에서는 중요한 소스 코드 파일에 대해 간략하게 설명합니다.

| 파일                                      | 설명                                                                                                                                                                          |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| App.h, App.cpp               | 앱의 보기(창, 스레드 및 이벤트)를 캡슐화하는 **App** 및 **DirectXApplicationSource** 클래스를 정의합니다.                                                     |
| Audio.h, Audio.cpp                         | 오디오 리소스를 관리하는 **Audio** 클래스를 정의합니다.                                                                                                                          |
| BasicLoader.h, BasicLoader.cpp             | 텍스처, 메시 및 셰이더를 로드하는 데 도움이 되는 유틸리티 메서드를 제공하는 **BasicLoader** 클래스를 정의합니다.                                                                  |
| BasicMath.h                                | 벡터 및 행렬 데이터와 계산 작업에 도움이 되는 구조와 함수를 정의합니다. 이러한 함수는 대부분 HLSL 셰이더 형식과 호환됩니다.                     |
| BasicReaderWriter.h, BasicReaderWriter.cpp | Windows 런타임을 사용하여 UWP 앱의 파일 데이터를 읽고 쓰는 **BasicReaderWriter** 클래스를 정의합니다.                                                                    |
| BasicShapes.h, BasicShapes.cpp             | 큐브, 구 등의 기본 도형을 만들기 위한 유틸리티 메서드를 제공하는 **BasicShapes** 클래스를 정의합니다. 이러한 파일은 Marble Maze 구현에서 사용되지 않습니다. |                                                                                  |
| Camera.h, Camera.cpp                       | 카메라의 위치와 방향을 제공하는 **Camera** 클래스를 정의합니다.                                                                                               |
| Collision.h, Collision.cpp                 | 미로 등의 다른 개체와 구슬 간의 충돌 정보를 관리합니다.                                                                                                       |
| DDSTextureLoader.h, DDSTextureLoader.cpp   | 메모리 버퍼에서 .dds 형식의 텍스처를 로드하는 **CreateDDSTextureFromMemory** 함수를 정의합니다.                                                              |
| DirectXHelper.h             | 많은 DirectX UWP 앱에 유용한 DirectX 도우미 함수를 정의합니다.                                                                            |
| LoadScreen.h, LoadScreen.cpp               | 앱을 초기화하는 동안 대기 화면을 표시하는 **LoadScreen** 클래스를 정의합니다.                                                                                         |
| MarbleMazeMain.h, MarbleMazeMain.cpp               | 게임 관련 리소스를 관리하고 대부분의 게임 논리를 정의하는 **MarbleMazeMain** 클래스를 정의합니다.                                                                          |
| MediaStreamer.h, MediaStreamer.cpp         | 게임에서 오디오 리소스를 관리하는 데 도움이 되도록 미디어 파운데이션을 사용하는 **MediaStreamer** 클래스를 정의합니다.                                                                            |
| PersistentState.h, PersistentState.cpp     | 백업 저장소에서 기본 데이터 형식을 읽고 쓰는 **PersistentState** 클래스를 정의합니다.                                                                      |
| Physics.h, Physics.cpp                     | 구슬과 미로 간의 물리학 시뮬레이션을 구현하는 **Physics** 클래스를 정의합니다.                                                                              |
| Primitives.h                               | 게임에 사용되는 형상 유형을 정의합니다.                                                                                                                                   |
| SampleOverlay.h, SampleOverlay.cpp         | 공용 2D 및 사용자 인터페이스 데이터와 작업을 제공하는 **SampleOverlay** 클래스를 정의합니다.                                                                               |
| SDKMesh.h, SDKMesh.cpp                     | SDK 메시(.sdkmesh) 형식의 메시를 로드하고 렌더링하는 **SDKMesh** 클래스를 정의합니다.                                                                                |
| StepTimer.h               | 쉽게 총 시간과 경과 시간을 구할 수 있는 방법을 제공하는 **StepTimer** 클래스를 정의합니다.
| UserInterface.h, UserInterface.cpp         | 메뉴 시스템, 최고 점수 테이블 등의 사용자 인터페이스와 관련된 기능을 정의합니다.                                                                        |

 

##  <a name="design-time-versus-run-time-resource-formats"></a>디자인 타임 및 런타임 리소스 형식


가능한 경우 디자인 타임 형식 대신 런타임 형식을 사용하여 더 효율적으로 게임 리소스를 로드합니다.

*디자인 타임* 형식은 리소스를 디자인할 때 사용하는 형식입니다. 일반적으로 3D 디자이너는 디자인 타임 형식으로 작업합니다. 일부 디자인 타임 형식은 텍스트 기반이므로 모든 텍스트 편집기에서 수정할 수 있습니다. 디자인 타임 형식은 자세한 정보를 표시하며, 게임에 필요한 추가 정보를 포함할 수 있습니다. *런타임* 형식은 게임에서 읽는 이진 형식입니다. 일반적으로 런타임 형식은 해당 디자인 타임 형식보다 더 간단하며 더 효율적으로 로드됩니다. 대부분의 게임이 런타임에 런타임 자산을 사용하는 것은 이런 이유 때문입니다.

게임에서 디자인 타임 형식을 직접 읽을 수도 있지만 별도의 런타임 형식을 사용할 경우 여러 가지 이점이 있습니다. 대체로 런타임 형식은 더 간단하기 때문에 필요한 디스크 공간이 더 적고 네트워크를 통해 전송하는 데 필요한 시간이 더 짧습니다. 또한 런타임 형식은 대체로 메모리 매핑된 데이터 구조로 표시됩니다. 따라서 XML 기반 텍스트 파일 등보다 훨씬 더 빠르게 메모리에 로드할 수 있습니다. 마지막으로, 별도의 런타임 형식은 대개 이진 인코딩되기 때문에 최종 사용자가 수정하기 더 어렵습니다.

HLSL 셰이더는 다른 디자인 타임 및 런타임 형식을 사용하는 리소스의 한 예입니다. Marble Maze는 .hlsl을 디자인 타임 형식으로 사용하고 .cso를 런타임 형식으로 사용합니다. .hlsl 파일에는 셰이더 소스 코드가 포함되고 .cso 파일에는 해당 셰이더 바이트 코드가 포함됩니다. 오프라인에서 .hlsl 파일을 변환하고 게임과 함께 .cso 파일을 제공하는 경우 게임이 로드될 때 HLSL 소스 파일을 바이트 코드로 변환할 필요가 없도록 합니다.

지침을 위해 Marble Maze 프로젝트에는 다양한 리소스에 대한 디자인 타임 형식 및 런타임 형식이 둘 다 포함되지만, 필요할 때 런타임 형식으로 변환할 수 있으므로 고유한 게임에 대한 원본 프로젝트에는 디자인 타임 형식만 유지하면 됩니다. 이 설명서에서는 디자인 타임 형식을 런타임 형식으로 변환하는 방법을 보여 줍니다.

##  <a name="application-life-cycle"></a>응용 프로그램 수명 주기


Marble Maze는 일반적인 UWP 앱의 수명 주기를 따릅니다. UWP 앱의 수명 주기에 대한 자세한 내용은 [앱 수명 주기](https://msdn.microsoft.com/library/windows/apps/mt243287)를 참조하세요.

UWP 게임은 초기화될 때 일반적으로 Direct3D, Direct2D, 사용하는 모든 입력, 오디오 또는 물리학 라이브러리 등의 런타임 구성 요소를 초기화합니다. 또한 게임이 시작되기 전에 필요한 게임 관련 리소스를 로드합니다. 이 초기화는 게임 세션 중에 한 번 발생합니다.

초기화 후 게임은 일반적으로 *게임 루프*를 실행합니다. 이 루프에서 게임은 일반적으로 Windows 이벤트 처리, 입력 수집, 장면 개체 업데이트, 장면 렌더링의 4개 작업을 수행합니다. 게임은 장면을 업데이트할 때 장면 개체에 현재 입력 상태를 적용하고 개체 충돌 등의 물리적 이벤트를 시뮬레이션할 수 있습니다. 게임에서 소리 효과 재생, 네트워크를 통해 데이터 전송 등의 기타 작업을 수행할 수도 있습니다. 게임은 장면을 렌더링할 때 장면의 현재 상태를 캡처하고 디스플레이 장치에 그립니다. 다음 섹션에서는 이러한 작업에 대해 자세히 설명합니다.

##  <a name="adding-to-the-template"></a>템플릿에 추가


**DirectX 11 앱(유니버설 Windows)** 템플릿은 Direct3D로 렌더링할 수 있는 핵심 창을 만듭니다. 템플릿에는 UWP 앱에서 3D 콘텐츠 렌더링에 필요한 모든 Direct3D 장치 리소스를 만드는 **DeviceResources** 클래스도 포함되어 있습니다.

**App** 클래스는 **MarbleMazeMain** 클래스 개체를 만들고, 리소스의 로딩을 시작하고, 루프를 통해 타이머를 업데이트하고, 프레임별 **MarbleMazeMain::Render** 렌더 메서드를 호출합니다. **App::OnWindowSizeChanged**, **App::OnDpiChanged** 및 **App::OnOrientationChanged** 메서드는 각각 **MarbleMazeMain::CreateWindowSizeDependentResources** 메서드를 호출하고, **App::Run** 메서드는 **MarbleMazeMain::Update** 및 **MarbleMazeMain::Render** 메서드를 호출합니다.

다음 예제는 **App::SetWindow** 메서드가 **MarbleMazeMain** 클래스 개체를 만드는 곳을 보여 줍니다. 렌더링에 Direct3D 개체를 사용할 수 있도록 **DeviceResources** 클래스가 메서드로 전달됩니다.

```cpp
    m_main = std::unique_ptr<MarbleMazeMain>(new MarbleMazeMain(m_deviceResources));
```

**App** 클래스는 또한 지연된 게임용 리소스의 로딩을 시작합니다. 자세한 내용은 다음 섹션을 참조하세요.

또한 **App** 클래스는 [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow) 이벤트에 대한 이벤트 처리기를 설정합니다. 이러한 이벤트의 처리기가 호출되면 입력이 **MarbleMazeMain** 클래스로 전달됩니다.

## <a name="loading-game-assets-in-the-background"></a>백그라운드에서 게임 자산 로드


게임이 시작 후 5초 내에 창 이벤트에 응답할 수 있게 하려면 비동기적으로 또는 백그라운드에서 게임 자산을 로드하는 것이 좋습니다. 자산이 백그라운드에서 로드될 때 게임이 창 이벤트에 응답할 수 있습니다.

> [!NOTE]
> 준비된 경우 주 메뉴를 표시하고 나머지 자산이 백그라운드에서 계속 로드되도록 허용할 수도 있습니다. 모든 리소스가 로드되기 전에 사용자가 메뉴에서 옵션을 선택하는 경우 진행률 표시줄 등을 표시하여 장면 리소스가 계속 로드되고 있음을 나타낼 수 있습니다.

 

게임에 비교적 적은 게임 자산이 포함된 경우 두 가지 이유 때문에 게임 자산을 비동기적으로 로드하는 것이 좋습니다. 이유 중 하나는 모든 리소스가 모든 장치와 모든 구성에서 신속하게 로드된다는 보장이 없기 때문입니다. 또한 비동기 로드를 초기에 통합하면 기능을 추가할 때 코드가 쉽게 확장됩니다.

비동기 자산 로드는 **App::Load** 메서드로 시작됩니다. 이 메서드는 [task](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class) 클래스를 사용하여 백그라운드에서 게임 자산을 로드합니다.

```cpp
    task<void>([=]()
    {
        m_main->LoadDeferredResources(true, false);
    });
```

**MarbleMazeMain** 클래스는 비동기 로드가 완료되었음을 나타내는 *m\_deferredResourcesReady* 플래그를 정의합니다. **MarbleMazeMain::LoadDeferredResources** 메서드는 게임 리소스를 로드한 다음 이 플래그를 설정합니다. 앱의 업데이트(**MarbleMazeMain::Update**) 및 렌더링(**MarbleMazeMain::Render**) 단계에서 이 플래그를 검사합니다. 이 플래그가 설정된 경우 게임이 정상적으로 계속됩니다. 플래그가 설정되지 않은 경우 게임에 대기 화면이 표시됩니다.

UWP 앱용 비동기 프로그래밍에 대한 자세한 내용은 [C++의 비동기 프로그래밍](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)을 참조하세요.

> [!TIP]
> Windows 런타임 C++ 라이브러리(DLL)에 포함된 게임 코드를 작성하는 경우, [C++로 UWP 앱용 비동기 작업 만들기](https://docs.microsoft.com/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps)를 참조하여 앱 및 다른 라이브러리에서 사용할 수 있는 비동기 작업을 만드는 방법을 알아볼지 여부를 고려합니다.

 

## <a name="the-game-loop"></a>게임 루프


**App::Run** 메서드는 기본 게임 루프를 실행합니다(**MarbleMazeMain::Update**). 프레임마다 이 메서드가 호출됩니다.

게임 관련 코드에서 뷰 및 창 코드를 분리하기 위해 업데이트 및 렌더링 호출을 **MarbleMazeMain** 개체에 전달하는 **App::Run** 메서드를 구현했습니다.

다음 예제에서는 주 게임 루프가 포함된 **App::Run** 메서드를 보여 줍니다. 게임 루프는 총 시간 및 프레임 시간 변수를 업데이트하고, 장면을 업데이트 및 렌더링합니다. 따라서 콘텐츠는 창이 보일 때에만 렌더링됩니다.

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


일반적으로 게임에는 게임 논리의 흐름 및 순서를 제어하는 *상태 시스템*(*유한 상태 시스템* 또는 FSM이라고도 함)이 포함됩니다. 상태 시스템은 지정된 개수의 상태를 포함하며 상태 간에 전환할 수 있습니다. 일반적으로 상태 시스템은 *초기* 상태로 시작되고, 하나 이상의 *중간* 상태로 전환된 다음 *터미널* 상태로 끝납니다.

대체로 게임 루프는 현재 게임 상태와 관련된 논리를 수행할 수 있도록 상태 시스템을 사용합니다. Marble Maze는 게임의 가능한 상태를 각각 정의하는 **GameState** 열거형을 정의합니다.

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

예를 들어 **MainMenu** 상태는 주 메뉴가 표시되고 게임이 활성화되지 않은 상태를 정의합니다. 반대로, **InGameActive** 상태는 게임이 활성화되고 메뉴가 표시되지 않는 상태를 정의합니다. **MarbleMazeMain** 클래스는 활성 게임 상태를 저장할 **m\_gameState** 멤버 변수를 정의합니다.

**MarbleMazeMain::Update** 및 **MarbleMazeMain::Render** 메서드는 switch 문을 사용하여 현재 상태에 대한 논리를 수행합니다. 다음 예제에서는 이 switch 문이 **MarbleMazeMain::Update** 메서드에서 어떻게 표시되는지를 보여 줍니다(구조를 설명하기 위해 세부 정보는 제거됨).

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

게임 논리 또는 렌더링이 특정 게임 상태에 따라 달라지는 경우 이 설명서에서 강조됩니다.

## <a name="handling-app-and-window-events"></a>앱 및 창 이벤트 처리


Windows 런타임은 Windows 메시지를 더 쉽게 관리할 수 있도록 개체 지향적인 이벤트 처리 시스템을 제공합니다. 응용 프로그램에서 이벤트를 사용하려면 이벤트에 응답하는 이벤트 처리기 또는 이벤트 처리 메서드를 제공해야 합니다. 또한 이벤트 원본에 이벤트 처리기를 등록해야 합니다. 이 프로세스를 이벤트 연결이라고 합니다.

### <a name="supporting-suspend-resume-and-restart"></a>일시 중단, 계속 및 다시 시작 지원

사용자가 잠깐 조작을 멈추거나 Windows가 전원 부족 상태가 되면 Marble Maze가 일시 중단됩니다. 사용자가 게임을 포그라운드로 이동하거나 Windows가 전원 부족 상태에서 벗어나면 게임이 다시 시작됩니다. 일반적으로 앱을 닫지 않습니다. Windows에서 앱이 일시 중단 상태이고 앱에 사용 중인 리소스(예: 메모리)가 필요한 경우 앱을 종료할 수 있습니다. Windows는 일시 중단하거나 다시 시작할 때 앱에 알리지만 종료할 때는 앱에 알리지 않습니다. 따라서 Windows가 앱을 일시 중단한다고 알릴 때 앱을 다시 시작할 경우 현재 사용자 상태를 복원하는 데 필요한 모든 데이터를 저장할 수 있어야 합니다. 앱에 저장할 중요한 사용자 상태가 많은 경우 앱이 일시 중단 알림을 수신하기 전에 정기적으로 상태를 저장해야 할 수도 있습니다. Marble Maze는 다음 두 가지 이유로 일시 중단 및 다시 시작 알림에 응답합니다.

1.  앱이 일시 중단되면 게임이 현재 게임 상태를 저장하고 오디오 재생을 일시 중지합니다. 앱이 다시 시작되면 게임이 오디오 재생을 다시 시작합니다.
2.  앱을 닫고 나중에 다시 시작하면 게임이 이전 상태에서 다시 시작됩니다.

Marble Maze는 일시 중단 및 다시 시작을 지원하기 위해 다음 작업을 수행합니다.

-   사용자에 검사점에 도달하는 경우 등 게임의 주요 시점에서 상태를 영구적 저장소에 저장합니다.
-   상태를 영구적 저장소에 저장하여 일시 중단 알림에 응답합니다.
-   영구적 저장소에서 상태를 로드하여 다시 시작 알림에 응답합니다. 또한 시작 중에 이전 상태를 로드합니다.

일시 중단 및 다시 시작을 지원하기 위해 Marble Maze는 **PersistentState** 클래스를 정의합니다. **PersistentState.h** 및 **PersistentState.cpp**를 참조하세요. 이 클래스는 [Windows::Foundation::Collections::IPropertySet](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IPropertySet) 인터페이스를 사용하여 속성을 읽고 씁니다. **PersistentState** 클래스는 백업 저장소에서 기본 데이터 형식(**bool**, **int**, **float**, [XMFLOAT3](https://msdn.microsoft.com/library/windows/desktop/ee419475) 및 [Platform::String](https://docs.microsoft.com/cpp/cppcx/platform-string-class))을 읽고 쓰는 메서드를 제공합니다.

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

**MarbleMazeMain** 클래스는 **PersistentState** 개체를 저장합니다. **MarbleMazeMain** 생성자는 이 개체를 초기화하고 로컬 응용 프로그램 데이터 저장소를 백업 데이터 저장소로 제공합니다.

```cpp
m_persistentState = ref new PersistentState();

m_persistentState->Initialize(
    Windows::Storage::ApplicationData::Current->LocalSettings->Values,
    "MarbleMaze");
```

Marble Maze는 구슬이 검사점 또는 목표를 통과할 때(**MarbleMazeMain::Update** 메서드) 및 창이 포커스를 잃을 때(**MarbleMazeMain::OnFocusChange** 메서드) 상태를 저장합니다. 게임에 많은 상태 데이터가 있는 경우 몇 초 내에 일시 중단 알림에 응답해야 하므로 수시로 상태를 영구적 저장소에 유사한 방식으로 저장하는 것이 좋습니다. 이렇게 하면 앱이 일시 중단 알림을 수신할 때 변경된 상태 데이터만 저장하면 됩니다.

일시 중단 및 다시 시작 알림에 응답하기 위해 **MarbleMazeMain** 클래스는 일시 중단 및 다시 시작 시 호출되는 **SaveState** 및 **LoadState** 메서드를 정의합니다. **MarbleMazeMain::OnSuspending** 메서드는 일시 중단 이벤트를 처리하고 **MarbleMazeMain::OnResuming** 메서드는 다시 시작 이벤트를 처리합니다.

**MarbleMazeMain::OnSuspending** 메서드는 게임 상태를 저장하고 오디오를 일시 중단합니다.

```cpp
void MarbleMazeMain::OnSuspending()
{
    SaveState();
    m_audio.SuspendAudio();
}
```

**MarbleMazeMain::SaveState** 메서드는 구슬의 현재 위치 및 속도, 최근 검사점, 최고 점수 테이블 등의 게임 상태 값을 저장합니다.

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

게임이 다시 시작될 때 오디오만 다시 시작하면 됩니다. 상태가 메모리에 이미 로드되어 있으므로 영구적 저장소에서 상태를 로드할 필요는 없습니다.

게임이 오디오를 일시 중단하고 다시 시작하는 방법은 [Marble Maze 샘플에 오디오 추가](adding-audio-to-the-marble-maze-sample.md) 문서에서 설명합니다.

다시 시작을 지원하기 위해 시작 중에 호출되는 **MarbleMazeMain** 생성자는 **MarbleMazeMain::LoadState** 메서드를 호출합니다. **MarbleMazeMain::LoadState** 메서드는 상태를 읽고 게임 개체에 적용합니다. 또한 이 메서드는 게임이 일시 중지된 경우 현재 게임 상태를 일시 중지됨으로 설정하고, 게임이 일시 중단된 경우 활성으로 설정합니다. 사용자가 예기치 않은 작업에 놀라지 않도록 게임을 일시 중지합니다. 또한 게임을 일시 중단할 때 게임이 플레이 상태가 아닌 경우 주 메뉴로 이동됩니다.

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
> Marble Maze는 콜드 시작(이전 일시 중단 이벤트 없이 처음부터 시작)과 일시 중단 상태에서 다시 시작을 구분하지 않습니다. 이것이 모든 UWP 앱에 대해 권장되는 디자인입니다.

응용 프로그램 데이터에 대한 자세한 내용은 [설정 및 기타 앱 데이터 저장 및 검색](https://msdn.microsoft.com/library/windows/apps/mt299098)을 참조하세요.

##  <a name="next-steps"></a>다음 단계


시각적 리소스 작업을 할 때 고려할 몇 가지 주요 사항에 대한 자세한 내용은 [Marble Maze 샘플에 시각적 콘텐츠 추가](adding-visual-content-to-the-marble-maze-sample.md)를 참조하세요.

## <a name="related-topics"></a>관련 항목

* [Marble Maze 샘플에 시각적 콘텐츠 추가](adding-visual-content-to-the-marble-maze-sample.md)
* [Marble Maze 샘플 기본 사항](marble-maze-sample-fundamentals.md)
* [C++ 및 DirectX로 UWP 게임 Marble Maze 개발](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




