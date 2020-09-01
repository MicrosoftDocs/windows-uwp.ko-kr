---
title: UWP DirectX 게임의 입력 대기 시간 최적화
description: 입력 대기 시간은 게임 환경에 크게 영향을 줄 수 있으며 최적화를 사용 하면 게임 느낌을 더욱 향상 시킬 수 있습니다.
ms.assetid: e18cd1a8-860f-95fb-098d-29bf424de0c0
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx, 입력 대기 시간
ms.localizationpriority: medium
ms.openlocfilehash: f0f95e7bdc523751e0d9eea5ffdd1ef5b889ddfc
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175267"
---
#  <a name="optimize-input-latency-for-universal-windows-platform-uwp-directx-games"></a>UWP (유니버설 Windows 플랫폼) DirectX 게임의 입력 대기 시간 최적화



입력 대기 시간은 게임 환경에 크게 영향을 줄 수 있으며 최적화를 사용 하면 게임 느낌을 더욱 향상 시킬 수 있습니다. 또한 적절 한 입력 이벤트 최적화는 배터리 수명을 향상 시킬 수 있습니다. 적절 한 CoreDispatcher 입력 이벤트 처리 옵션을 선택 하 여 게임에서 입력을 최대한 원활 하 게 처리 하는지 확인 하는 방법을 알아봅니다.

## <a name="input-latency"></a>입력 대기 시간


입력 대기 시간은 시스템이 사용자 입력에 응답 하는 데 걸리는 시간입니다. 응답은 화면에 표시 되는 내용이 변경 되거나 오디오 피드백을 통해 들릴 수 있는 경우가 많습니다.

터치 포인터, 마우스 포인터 또는 키보드에서 가져온 모든 입력 이벤트는 이벤트 처리기에서 처리할 메시지를 생성 합니다. 최신 터치 디지타이저 및 게임 주변 장치는 포인터 당 최소 100 Hz의 입력 이벤트를 보고 합니다. 즉, 앱에서 포인터 또는 키 입력 당 초당 100 이벤트를 받을 수 있습니다. 여러 포인터가 동시에 발생 하거나 더 높은 정밀도 입력 장치 (예: 게임 마우스)를 사용 하는 경우이 업데이트 속도가 증폭 됩니다. 이벤트 메시지 큐가 매우 빠르게 채워질 수 있습니다.

시나리오에 가장 적합 한 방식으로 이벤트를 처리 하도록 게임의 입력 대기 시간 요구를 이해 하는 것이 중요 합니다. 모든 게임에 대해 하나의 솔루션이 없습니다.

## <a name="power-efficiency"></a>전원 효율성


입력 대기 시간의 맥락에서 "전원 효율성"은 게임에서 GPU를 사용 하는 정도를 나타냅니다. 더 작은 GPU 리소스를 사용 하는 게임은 더 강력한 기능을 사용 하 여 더 긴 배터리 수명을 허용할 수 있습니다. CPU의 경우에도 마찬가지입니다.

사용자 경험을 저하 시 키 지 않고 게임에서 초당 60 프레임 미만의 전체 화면을 그릴 수 있는 경우 (현재 대부분의 렌더링 속도) 사용자의 경험을 저하 시 키 지 않고 더 효율적으로 그리는 것이 좋습니다. 일부 게임에서는 사용자 입력에 대 한 응답 으로만 화면을 업데이트 하므로 이러한 게임에서는 초당 60 프레임에서 동일한 콘텐츠를 반복 해 서 그리지 않아야 합니다.

## <a name="choosing-what-to-optimize-for"></a>최적화할 항목 선택


DirectX 앱을 디자인할 때 몇 가지 항목을 선택 해야 합니다. 앱에서 부드러운 애니메이션을 제공 하기 위해 초당 60 프레임을 렌더링 하거나 입력에 대 한 응답 으로만 렌더링 해야 하나요? 가능한 최저 입력 대기 시간이 필요 하거나 약간의 지연을 허용할 수 있나요? 사용자의 앱이 배터리 사용에 적절 한 것으로 간주 하나요?

이러한 질문에 대 한 대답은 다음 시나리오 중 하나를 사용 하 여 앱을 정렬할 수 있습니다.

1.  주문형으로 렌더링 합니다. 이 범주의 게임은 특정 형식의 입력에 대 한 응답으로 화면을 업데이트 하기만 하면 됩니다. 앱이 동일한 프레임을 반복 해 서 렌더링 하지 않으며, 앱에서 입력 대기 시간 대부분을 소비 하기 때문에 입력 대기 시간이 부족 하기 때문에 전원 효율성이 우수 합니다. 이 범주에 속하는 응용 프로그램의 예로는 보드 게임과 뉴스 읽기 권한자가 있습니다.
2.  임시 애니메이션으로 요청 시 렌더링 합니다. 이 시나리오는 특정 유형의 입력이 사용자의 후속 입력에 종속 되지 않는 애니메이션을 시작 한다는 점만 제외 하 고 첫 번째 시나리오와 비슷합니다. 게임에서 동일한 프레임을 반복 해 서 렌더링 하지 않고 입력 대기 시간이 부족 하 여 게임에 애니메이션을 적용 하지 않기 때문에 전원 효율성이 양호 합니다. 각 이동에 애니메이션 효과를 주는 대화형 어린이 게임 및 보드 게임은이 범주에 포함 될 수 있는 앱의 예입니다.
3.  초당 60 프레임을 렌더링 합니다. 이 시나리오에서 게임은 지속적으로 화면을 업데이트 합니다. 디스플레이가 제공할 수 있는 최대 프레임 수를 렌더링 하므로 전원 효율성이 저하 됩니다. 콘텐츠를 표시 하는 동안 DirectX가 스레드를 차단 하기 때문에 입력 대기 시간이 높습니다. 이렇게 하면 스레드가 사용자에 게 표시할 수 있는 것 보다 더 많은 프레임을 표시 하는 것을 방지 합니다. 이 범주에 속하는 응용 프로그램의 예로는 shooters, 실시간 전략 게임 및 물리학 기반 게임이 있습니다.
4.  초당 60 프레임을 렌더링 하 고 가능한 최저 입력 대기 시간을 확보 합니다. 시나리오 3과 마찬가지로 앱은 지속적으로 화면을 업데이트 하므로 전원 효율성이 저하 됩니다. 차이점은 게임에서 별도 스레드의 입력에 응답 하 여 그래픽을 표시에 표시 하 여 입력 처리가 차단 되지 않는다는 것입니다. 온라인 멀티 플레이 게임, 전투 게임 또는 리듬/타이밍 게임은 매우 긴밀 한 이벤트 창 내에서 입력 이동을 지원 하기 때문에이 범주에 속합니다.

## <a name="implementation"></a>구현


대부분의 DirectX 게임은 게임 루프로 알려진 항목을 기반으로 합니다. 기본 알고리즘은 사용자가 게임 또는 앱을 종료할 때까지 다음 단계를 수행 하는 것입니다.

1.  입력 처리
2.  게임 상태 업데이트
3.  게임 콘텐츠 그리기

DirectX 게임의 콘텐츠가 렌더링 되 고 화면에 표시 될 준비가 되 면 게임 루프는 입력을 다시 처리 하기 위해 절전 모드를 해제 하기 전에 GPU가 새 프레임을 받을 준비가 될 때까지 기다립니다.

앞에서 설명한 각 시나리오에 대 한 게임 루프의 구현은 간단한 jigsaw 퍼즐 게임을 반복 하 여 살펴보겠습니다. 각 구현에 대해 설명 하는 의사 결정 사항, 이점 및 장단점은 짧은 대기 시간 입력 및 전원 효율성을 위해 앱을 최적화 하는 데 도움이 되는 가이드 역할을 할 수 있습니다.

## <a name="scenario-1-render-on-demand"></a>시나리오 1: 주문형 렌더링


Jigsaw 퍼즐 게임의 첫 번째 반복은 사용자가 퍼즐 조각을 이동할 때만 화면을 업데이트 합니다. 사용자는 퍼즐 조각을 선택 하거나 선택한 후 올바른 대상에 접촉 하 여 제자리에 놓을 수 있습니다. 두 번째 경우에는 퍼즐 조각이 애니메이션이 나 효과 없이 대상으로 이동 합니다.

이 코드에는 **CoreProcessEventsOption::P rocessoneandallpending**를 사용 하는 [**IFrameworkView:: Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) 메서드 내에 단일 스레드 게임 루프가 있습니다. 이 옵션을 사용 하면 큐에서 현재 사용할 수 있는 모든 이벤트를 디스패치합니다. 보류 중인 이벤트가 없으면 게임 루프가 나타날 때까지 기다립니다.

``` syntax
void App::Run()
{
    
    while (!m_windowClosed)
    {
        // Wait for system events or input from the user.
        // ProcessOneAndAllPending will block the thread until events appear and are processed.
        CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);

        // If any of the events processed resulted in a need to redraw the window contents, then we will re-render the
        // scene and present it to the display.
        if (m_updateWindow || m_state->StateChanged())
        {
            m_main->Render();
            m_deviceResources->Present();

            m_updateWindow = false;
            m_state->Validate();
        }
    }
}
```

## <a name="scenario-2-render-on-demand-with-transient-animations"></a>시나리오 2: 임시 애니메이션을 사용 하 여 요청 시 렌더링


두 번째 반복에서는 사용자가 퍼즐 조각을 선택 하 고 해당 부분에 대해 올바른 대상에 접촉 하는 경우 대상에 도달할 때까지 화면 전체에 애니메이션 효과를 적용 하도록 게임을 수정 합니다.

이전과 마찬가지로이 코드에는 **Processoneandallpending** 을 사용 하 여 큐에 입력 이벤트를 디스패치 하는 단일 스레드 게임 루프가 있습니다. 이제 애니메이션을 실행 하는 동안 루프가 새 입력 이벤트를 기다리지 않도록 **CoreProcessEventsOption::P rocessallifpresent** 를 사용 하도록 변경 된다는 점이 다릅니다. 보류 중인 이벤트가 없으면 [**Processevents**](/uwp/api/windows.ui.core.coredispatcher.processevents) 는 즉시 반환 되며 앱에서 애니메이션의 다음 프레임을 제공할 수 있습니다. 애니메이션이 완료 되 면 루프에서 **Processoneandallpending** 으로 다시 전환 하 여 화면 업데이트를 제한 합니다.

``` syntax
void App::Run()
{

    while (!m_windowClosed)
    {
        // 2. Switch to a continuous rendering loop during the animation.
        if (m_state->Animating())
        {
            // Process any system events or input from the user that is currently queued.
            // ProcessAllIfPresent will not block the thread to wait for events. This is the desired behavior when
            // you are trying to present a smooth animation to the user.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_state->Update();
            m_main->Render();
            m_deviceResources->Present();
        }
        else
        {
            // Wait for system events or input from the user.
            // ProcessOneAndAllPending will block the thread until events appear and are processed.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);

            // If any of the events processed resulted in a need to redraw the window contents, then we will re-render the
            // scene and present it to the display.
            if (m_updateWindow || m_state->StateChanged())
            {
                m_main->Render();
                m_deviceResources->Present();

                m_updateWindow = false;
                m_state->Validate();
            }
        }
    }
}
```

**Processoneandallpending** 및 **ProcessAllIfPresent**간의 전환을 지원 하려면 앱에서 상태를 추적 하 여 애니메이션이 적용 되는지 확인 해야 합니다. Jigsaw 퍼즐 앱에서 GameState 클래스의 게임 루프 중에 호출할 수 있는 새 메서드를 추가 하 여이 작업을 수행 합니다. 게임 루프의 애니메이션 분기는 GameState의 새 업데이트 메서드를 호출 하 여 애니메이션 상태의 업데이트를 구동 합니다.

## <a name="scenario-3-render-60-frames-per-second"></a>시나리오 3: 초당 60 프레임 렌더링


세 번째 반복에서 앱은 사용자에 게 퍼즐에서 작업 한 시간을 표시 하는 타이머를 표시 합니다. 경과 된 시간을 밀리초 단위로 표시 하기 때문에 표시를 최신 상태로 유지 하기 위해 초당 60 프레임을 렌더링 해야 합니다.

시나리오 1 및 2와 마찬가지로 앱에는 단일 스레드 게임 루프가 있습니다. 이 시나리오의 차이점은 항상 렌더링 되기 때문에 처음 두 시나리오에서 수행한 것 처럼 게임 상태의 변경 내용을 더 이상 추적할 필요가 없다는 것입니다. 따라서 기본값은 **ProcessAllIfPresent** 를 사용 하 여 이벤트를 처리할 수 있습니다. 보류 중인 이벤트가 없으면 **Processevents** 는 즉시 반환 되 고 다음 프레임을 렌더링 합니다.

``` syntax
void App::Run()
{

    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            // 3. Continuously render frames and process system events and input as they appear in the queue.
            // ProcessAllIfPresent will not block the thread to wait for events. This is the desired behavior when
            // trying to present smooth animations to the user.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_state->Update();
            m_main->Render();
            m_deviceResources->Present();
        }
        else
        {
            // 3. If the window isn't visible, there is no need to continuously render.
            // Process events as they appear until the window becomes visible again.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

이 접근 방식은 렌더링 시기를 결정 하기 위해 추가 상태를 추적할 필요가 없기 때문에 게임을 작성 하는 가장 쉬운 방법입니다. 타이머 간격에서 적절 한 입력 응답성과 함께 가능한 가장 빠른 렌더링을 달성 합니다.

그러나 이러한 개발 용이성은 가격이 제공 됩니다. 초당 60 프레임에서 렌더링 하면 주문형 렌더링 보다 더 많은 전력이 사용 됩니다. 게임에서 모든 프레임에 표시 되는 항목을 변경 하는 경우 **ProcessAllIfPresent** 를 사용 하는 것이 가장 좋습니다. 앱이 이제 **Processevents**대신 디스플레이의 동기화 간격에서 게임 루프를 차단 하기 때문에 16.7 밀리초 만큼 입력 대기 시간이 증가 합니다. 큐가 프레임당 한 번만 처리 되므로 (60 Hz) 일부 입력 이벤트가 삭제 될 수 있습니다.

## <a name="scenario-4-render-60-frames-per-second-and-achieve-the-lowest-possible-input-latency"></a>시나리오 4: 초당 60 프레임을 렌더링 하 고 가능한 가장 낮은 입력 대기 시간 획득


일부 게임은 시나리오 3에 표시 된 입력 대기 시간의 증가를 무시 하거나 보정할 수 있습니다. 그러나 게임 환경 및 플레이어 피드백의 의미에 대 한 낮은 입력 대기 시간이 중요 한 경우 초당 60 프레임을 렌더링 하는 게임은 별도의 스레드에서 입력을 처리 해야 합니다.

Jigsaw 퍼즐 게임의 네 번째 반복은 게임 루프에서 별도의 스레드로 입력 처리 및 그래픽 렌더링을 분할 하 여 시나리오 3을 기반으로 합니다. 각각에 대해 별도의 스레드를 사용 하면 입력이 그래픽 출력에 의해 지연 되지 않습니다. 그러나이 코드는 결과로 더 복잡해 집니다. 시나리오 4에서 입력 스레드는 [**CoreProcessEventsOption::P rocessuntilquit**](/uwp/api/Windows.UI.Core.CoreProcessEventsOption)를 사용 하 여 [**processevents**](/uwp/api/windows.ui.core.coredispatcher.processevents) 를 호출 합니다 .이는 새 이벤트를 기다리고 사용 가능한 모든 이벤트를 디스패치합니다. 창이 닫히거나 게임이 [**CoreWindow:: Close**](/uwp/api/windows.ui.core.corewindow.close)를 호출할 때까지이 동작을 계속 합니다.

``` syntax
void App::Run()
{
    // 4. Start a thread dedicated to rendering and dedicate the UI thread to input processing.
    m_main->StartRenderThread();

    // ProcessUntilQuit will block the thread and process events as they appear until the App terminates.
    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
}

void JigsawPuzzleMain::StartRenderThread()
{
    // If the render thread is already running, then do not start another one.
    if (IsRendering())
    {
        return;
    }

    // Create a task that will be run on a background thread.
    auto workItemHandler = ref new WorkItemHandler([this](IAsyncAction^ action)
    {
        // Notify the swap chain that this app intends to render each frame faster
        // than the display's vertical refresh rate (typically 60 Hz). Apps that cannot
        // deliver frames this quickly should set this to 2.
        m_deviceResources->SetMaximumFrameLatency(1);

        // Calculate the updated frame and render once per vertical blanking interval.
        while (action->Status == AsyncStatus::Started)
        {
            // Execute any work items that have been queued by the input thread.
            ProcessPendingWork();

            // Take a snapshot of the current game state. This allows the renderers to work with a
            // set of values that won't be changed while the input thread continues to process events.
            m_state->SnapState();

            m_sceneRenderer->Render();
            m_deviceResources->Present();
        }

        // Ensure that all pending work items have been processed before terminating the thread.
        ProcessPendingWork();
    });

    // Run the task on a dedicated high priority background thread.
    m_renderLoopWorker = ThreadPool::RunAsync(workItemHandler, WorkItemPriority::High, WorkItemOptions::TimeSliced);
}
```

Microsoft Visual Studio 2015의 **DirectX 11 및 XAML 앱 (유니버설 Windows)** 템플릿은 게임 루프를 비슷한 방식으로 여러 스레드로 분할 합니다. [**Windows:: UI:: Core:: CoreIndependentInputSource**](/uwp/api/Windows.UI.Core.CoreIndependentInputSource) 개체를 사용 하 여 입력 처리 전용 스레드를 시작 하 고 XAML UI 스레드를 독립적으로 렌더링 스레드를 만듭니다. 이러한 템플릿에 대 한 자세한 내용은 [템플릿에서 유니버설 Windows 플랫폼 및 DirectX 게임 프로젝트 만들기](user-interface.md)를 참조 하세요.

## <a name="additional-ways-to-reduce-input-latency"></a>입력 대기 시간을 줄이는 추가 방법


### <a name="use-waitable-swap-chains"></a>대기 가능 스왑 체인 사용

DirectX 게임은 사용자가 화면에 표시 되는 내용을 업데이트 하 여 사용자 입력에 응답 합니다. 60 Hz 디스플레이에서 화면은 16.7 밀리초 (1 초/60 프레임) 마다 새로 고쳐집니다. 그림 1에서는 초당 60 프레임을 렌더링 하는 앱에 대 한 16.7 ms refresh 신호 (VBlank)를 기준으로 하는 입력 이벤트에 대 한 대략적인 수명 주기와 응답을 보여 줍니다.

그림 1

![그림 1 directx의 입력 대기 시간 ](images/input-latency1.png)

Windows 8.1에서 DXGI는 스왑 체인에 대해 **dxgi \_ 스왑 \_ 체인 \_ 플래그 \_ 프레임 \_ 대기 시간 \_ 대기 가능 \_ 개체** 플래그를 도입 했습니다 .이를 통해 앱에서 추론을 구현 하 여 현재 큐를 빈 상태로 유지 하도록 요구 하지 않고 대기 시간을 쉽게 줄일 수 있습니다. 이 플래그를 사용 하 여 만든 스왑 체인을 대기 가능 스왑 체인 이라고 합니다. 그림 2에서는 대기 가능 스왑 체인을 사용할 때 입력 이벤트에 대 한 대략적인 수명 주기와 응답을 보여 줍니다.

그림 2

![directx 대기 가능의 그림 2 입력 대기 시간](images/input-latency2.png)

이러한 다이어그램에서 볼 수 있는 것은 게임에서 디스플레이의 새로 고침 속도로 정의한 16.7 ms 예산 내에서 각 프레임을 렌더링 하 고 제공할 수 있는 경우 두 개의 전체 프레임으로 입력 대기 시간을 줄일 수 있다는 것입니다. Jigsaw 퍼즐 샘플에서는 대기 가능 스왑 체인을 사용 하 고 다음을 호출 하 여 현재 큐 제한을 제어 합니다.` m_deviceResources->SetMaximumFrameLatency(1);`

 

 