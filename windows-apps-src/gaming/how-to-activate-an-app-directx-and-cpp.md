---
title: 앱 활성화 방법(DirectX 및 C++)
description: 이 항목에서는 UWP(유니버설 Windows 플랫폼) DirectX 앱에 대한 활성화 환경을 정의하는 방법을 보여 줍니다.
ms.assetid: b07c7da1-8a5e-5b57-6f77-6439bf653a53
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx, 활성화
ms.localizationpriority: medium
ms.openlocfilehash: 51c2435c8edeac2431198b7b5f3d9b1a307b5b78
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648848"
---
# <a name="how-to-activate-an-app-directx-and-c"></a>앱 활성화 방법(DirectX 및 C++)



이 항목에서는 UWP(유니버설 Windows 플랫폼) DirectX 앱에 대한 활성화 환경을 정의하는 방법을 보여 줍니다.

## <a name="register-the-app-activation-event-handler"></a>앱 활성화 이벤트 처리기 등록


먼저 운영 체제에 의해 앱이 시작되고 초기화될 때 발생하는 [**CoreApplicationView::Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) 이벤트를 처리하도록 등록합니다.

뷰 공급자(이 예에서는 **MyViewProvider**)의 [**IFrameworkView::Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) 메서드 구현에 다음 코드를 추가합니다.

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

## <a name="activate-the-corewindow-instance-for-the-app"></a>앱용 CoreWindow 인스턴스 활성화


앱이 시작되면 앱에 대한 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 참조를 획득해야 합니다. **CoreWindow**에는 앱이 창 이벤트를 처리하는 데 사용하는 창 이벤트 메시지 디스패처가 포함되어 있습니다. [  **CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589)를 호출하여 앱 활성화 이벤트에 대한 콜백에서 이 참조를 획득합니다. 이 참조를 획득한 후 [**CoreWindow::Activate**](https://msdn.microsoft.com/library/windows/apps/br208254)를 호출하여 메인 앱 창을 활성화합니다.

```cpp
void App::OnActivated(CoreApplicationView^ applicationView, IActivatedEventArgs^ args)
{
    // Run() won't start until the CoreWindow is activated.
    CoreWindow::GetForCurrentThread()->Activate();
}
```

## <a name="start-processing-event-message-for-the-main-app-window"></a>주 앱 창에 대한 이벤트 메시지 처리 시작


앱 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)의 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)가 이벤트 메시지를 처리할 때 콜백이 발생합니다. 앱의 주 루프(뷰 공급자의 [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 메서드에 구현)에서 [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215)를 호출해야만 이 콜백이 호출됩니다.

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


* [(DirectX 및 c + +) 앱을 중지 하는 방법](how-to-suspend-an-app-directx-and-cpp.md)
* [(DirectX 및 c + +)는 앱을 다시 시작 하는 방법](how-to-resume-an-app-directx-and-cpp.md)

 

 




