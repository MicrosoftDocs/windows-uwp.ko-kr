---
title: 앱을 다시 시작 하는 방법 (DirectX 및 c + +)
description: 이 항목에서는 시스템에서 UWP (유니버설 Windows 플랫폼) DirectX 앱을 다시 시작할 때 중요 한 응용 프로그램 데이터를 복원 하는 방법을 보여 줍니다.
ms.assetid: 5e6bb673-6874-ace5-05eb-f88c045f2178
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 다시 시작, directx
ms.localizationpriority: medium
ms.openlocfilehash: 37bceafae39c314966a95f06a282fe5c91814738
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175287"
---
# <a name="how-to-resume-an-app-directx-and-c"></a>앱을 다시 시작 하는 방법 (DirectX 및 c + +)



이 항목에서는 시스템에서 UWP (유니버설 Windows 플랫폼) DirectX 앱을 다시 시작할 때 중요 한 응용 프로그램 데이터를 복원 하는 방법을 보여 줍니다.

## <a name="register-the-resuming-event-handler"></a>다시 시작 이벤트 처리기를 등록 합니다.


를 등록 하 여 [**CoreApplication:: 다시 시작**](/uwp/api/windows.applicationmodel.core.coreapplication.resuming) 이벤트를 처리 합니다 .이 이벤트는 사용자가 앱에서 다른 위치로 전환 된 후 다시 시작 됨을 나타냅니다.

보기 공급자의 [**IFrameworkView:: Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize) 메서드 구현에 다음 코드를 추가 합니다.

```cpp
// The first method is called when the IFrameworkView is being created.
void App::Initialize(CoreApplicationView^ applicationView)
{
  //...
  
    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);
    
  //...

}
```

## <a name="refresh-displayed-content-after-suspension"></a>일시 중단 후 표시 된 콘텐츠 새로 고침


앱이 다시 시작 이벤트를 처리 하는 경우 표시 된 콘텐츠를 새로 고칠 수 있습니다. [**CoreApplication:: 일시 중단**](/uwp/api/windows.applicationmodel.core.coreapplication.suspending)에 대 한 처리기를 사용 하 여 저장 한 앱을 복원 하 고 처리를 다시 시작 합니다. 게임 개발자: 오디오 엔진을 일시 중단 한 경우 다시 시작 하는 시간입니다.

```cpp
void App::OnResuming(Platform::Object^ sender, Platform::Object^ args)
{
    // Restore any data or state that was unloaded on suspend. By default, data
    // and state are persisted when resuming from suspend. Note that this event
    // does not occur if the app was previously terminated.

    // Insert your code here.
}
```

이 콜백은 앱 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)에 대해 [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) 에서 처리 한 이벤트 메시지로 발생 합니다. 응용 프로그램의 주 루프에서 [**CoreDispatcher::P rocessevents**](/uwp/api/windows.ui.core.coredispatcher.processevents) 를 호출 하지 않는 경우에는이 콜백이 호출 되지 않습니다 (뷰 공급자의 [**IFrameworkView:: Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) 메서드에서 구현 됨).

``` syntax
// This method is called after the window becomes active.
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

## <a name="remarks"></a>설명


사용자가 다른 앱 이나 바탕 화면으로 전환할 때마다 시스템이 앱을 일시 중단 합니다. 사용자가 다시 전환할 때마다 시스템이 앱을 다시 시작 합니다. 시스템이 앱을 다시 시작할 때 변수 및 데이터 구조의 내용은 시스템이 앱을 일시 중단 하기 전과 동일 합니다. 시스템은 백그라운드에서 실행 중인 것 처럼 사용자에 게 표시 되도록, 중단 된 위치에서 앱을 복원 합니다. 그러나 앱이 상당한 시간 동안 일시 중단 되었을 수 있으므로 앱이 일시 중단 된 동안 변경 되었을 수 있는 표시 된 콘텐츠를 새로 고치고 렌더링 또는 오디오 처리 스레드를 다시 시작 해야 합니다. 이전 일시 중단 이벤트 중에 게임 상태 데이터를 저장 한 경우 지금 복원 합니다.

## <a name="related-topics"></a>관련 항목

* [앱을 일시 중단 하는 방법 (DirectX 및 c + +)](how-to-suspend-an-app-directx-and-cpp.md)
* [앱을 활성화 하는 방법 (DirectX 및 c + +)](how-to-activate-an-app-directx-and-cpp.md)

 

 