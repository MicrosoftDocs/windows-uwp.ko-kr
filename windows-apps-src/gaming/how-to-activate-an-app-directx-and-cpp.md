---
title: 앱을 활성화 하는 방법 (DirectX 및 c + +)
description: 이 항목에서는 UWP (유니버설 Windows 플랫폼) DirectX 앱에 대 한 정품 인증 환경을 정의 하는 방법을 보여 줍니다.
ms.assetid: b07c7da1-8a5e-5b57-6f77-6439bf653a53
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx, 정품 인증
ms.localizationpriority: medium
ms.openlocfilehash: 839cfc718e6225beb14df535bc48f9ba6f3f6dc5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173147"
---
# <a name="how-to-activate-an-app-directx-and-c"></a>앱을 활성화 하는 방법 (DirectX 및 c + +)



이 항목에서는 UWP (유니버설 Windows 플랫폼) DirectX 앱에 대 한 정품 인증 환경을 정의 하는 방법을 보여 줍니다.

## <a name="register-the-app-activation-event-handler"></a>앱 활성화 이벤트 처리기 등록


먼저 운영 체제에서 앱을 시작 하 고 초기화할 때 발생 하는 [**CoreApplicationView:: 활성화**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) 된 이벤트를 처리 하도록 등록 합니다.

보기 공급자의 [**IFrameworkView:: Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize) 메서드 구현에 다음 코드를 추가 합니다 (이 예제에서는 **myviewprovider** 라는 이름 지정).

```cpp
void App::Initialize(CoreApplicationView^ applicationView)
{
    // Register event handlers for the app lifecycle. This example includes Activated, so that we
    // can make the CoreWindow active and start rendering on the window.
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);
  
  //...

}
```

## <a name="activate-the-corewindow-instance-for-the-app"></a>앱에 대 한 CoreWindow 인스턴스 활성화


앱이 시작 되 면 앱에 대 한 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 에 대 한 참조를 가져와야 합니다. **CoreWindow** 에는 앱에서 창 이벤트를 처리 하는 데 사용 하는 window 이벤트 메시지 디스패처가 포함 되어 있습니다. [**CoreWindow:: GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread)를 호출 하 여 앱 활성화 이벤트의 콜백에서이 참조를 가져옵니다. 이 참조를 가져온 후에는 [**CoreWindow:: activate**](/uwp/api/windows.ui.core.corewindow.activate)를 호출 하 여 주 앱 창을 활성화 합니다.

```cpp
void App::OnActivated(CoreApplicationView^ applicationView, IActivatedEventArgs^ args)
{
    // Run() won't start until the CoreWindow is activated.
    CoreWindow::GetForCurrentThread()->Activate();
}
```

## <a name="start-processing-event-message-for-the-main-app-window"></a>주 응용 프로그램 창에 대 한 이벤트 메시지 처리 시작


응용 프로그램의 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)에 대 한 [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) 에서 이벤트 메시지를 처리할 때 콜백이 발생 합니다. 응용 프로그램의 주 루프에서 [**CoreDispatcher::P rocessevents**](/uwp/api/windows.ui.core.coredispatcher.processevents) 를 호출 하지 않는 경우에는이 콜백이 호출 되지 않습니다 (뷰 공급자의 [**IFrameworkView:: Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) 메서드에서 구현 됨).

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

## <a name="related-topics"></a>관련 항목


* [앱을 일시 중단 하는 방법 (DirectX 및 c + +)](how-to-suspend-an-app-directx-and-cpp.md)
* [앱을 다시 시작 하는 방법 (DirectX 및 c + +)](how-to-resume-an-app-directx-and-cpp.md)

 

 