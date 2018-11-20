---
author: mtoepke
title: 앱 일시 중단 방법(DirectX 및 C++)
description: 이 항목에서는 시스템이 UWP(유니버설 Windows 플랫폼) DirectX 앱을 일시 중단할 때 중요한 시스템 상태 및 앱 데이터를 저장하는 방법을 보여 줍니다.
ms.assetid: 5dd435e5-ec7e-9445-fed4-9c0d872a239e
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 일시 중지, directx
ms.localizationpriority: medium
ms.openlocfilehash: 204d61430f59c820e9ef9ef36832cd1c24ee7f9c
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7282415"
---
# <a name="how-to-suspend-an-app-directx-and-c"></a>앱 일시 중단 방법(DirectX 및 C++)



이 항목에서는 시스템이 UWP(유니버설 Windows 플랫폼) DirectX 앱을 일시 중단할 때 중요한 시스템 상태 및 앱 데이터를 저장하는 방법을 보여 줍니다.

## <a name="register-the-suspending-event-handler"></a>suspending 이벤트 처리기 등록


먼저 사용자 또는 시스템 동작에 의해 앱이 일시 중단 상태로 전환될 때 발생하는 [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) 이벤트를 처리하도록 등록합니다.

뷰 공급자의 [**IFrameworkView::Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) 메서드 구현에 다음 코드를 추가합니다.

```cpp
void App::Initialize(CoreApplicationView^ applicationView)
{
  //...
  
    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

  //...
}
```

## <a name="save-any-app-data-before-suspending"></a>일시 중단 전에 앱 데이터 저장


앱에서 [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) 이벤트를 처리하면, 처리기 함수에서 중요한 응용 프로그램 데이터를 저장할 기회가 생깁니다. 간단한 응용 프로그램 데이터를 동기적으로 저장하기 위해 앱에서 [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) 저장소 API를 사용해야 합니다. 게임을 개발하는 경우 중요한 게임 상태 정보를 저장합니다. 오디오 처리를 반드시 일시 중단해야 합니다.

이제 콜백을 구현합니다. 이 메서드에 앱 데이터를 저장합니다.

```cpp
void App::OnSuspending(Platform::Object^ sender, SuspendingEventArgs^ args)
{
    // Save app state asynchronously after requesting a deferral. Holding a deferral
    // indicates that the application is busy performing suspending operations. Be
    // aware that a deferral may not be held indefinitely. After about five seconds,
    // the app will be forced to exit.
    SuspendingDeferral^ deferral = args->SuspendingOperation->GetDeferral();

    create_task([this, deferral]()
    {
        m_deviceResources->Trim();

        // Insert your code here.

        deferral->Complete();
    });
}
```

이 콜백은 5초 내에 완료되어야 합니다. 이 콜백 동안 카운트다운을 시작하는 [**SuspendingOperation::GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690)를 호출하여 지연을 요청해야 합니다. 앱이 저장 작업을 완료하면 [**SuspendingDeferral::Complete**](https://msdn.microsoft.com/library/windows/apps/br224685)를 호출하여 앱이 일시 중단될 준비가 되었음을 시스템에 알립니다. 지연을 요청하지 않거나 앱이 데이터를 저장하는 데 5초보다 오래 소요될 경우 앱이 자동으로 일시 중단됩니다.

앱 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)의 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)가 이벤트 메시지를 처리할 때 이 콜백이 발생합니다. 앱의 주 루프(뷰 공급자의 [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 메서드에 구현)에서 [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215)를 호출해야만 이 콜백이 호출됩니다.

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

## <a name="call-trim"></a>Trim() 호출


Windows8.1부터 모든 DirectX UWP 앱 호출 해야 [**idxgidevice3:: Trim**](https://msdn.microsoft.com/library/windows/desktop/dn280346) 일시 중단 시 합니다. 이 호출은 앱에 할당된 모든 임시 버퍼를 해제하도록 그래픽 드라이버에 알리는데, 그러면 일시 중단된 상태인 동안 메모리 리소스를 회수하기 위해 앱이 종료되는 가능성이 줄어듭니다. Windows8.1에 대 한 인증 요구 사항입니다.

```cpp
void App::OnSuspending(Platform::Object^ sender, SuspendingEventArgs^ args)
{
    // Save app state asynchronously after requesting a deferral. Holding a deferral
    // indicates that the application is busy performing suspending operations. Be
    // aware that a deferral may not be held indefinitely. After about five seconds,
    // the app will be forced to exit.
    SuspendingDeferral^ deferral = args->SuspendingOperation->GetDeferral();

    create_task([this, deferral]()
    {
        m_deviceResources->Trim();

        // Insert your code here.

        deferral->Complete();
    });
}

// Call this method when the app suspends. It provides a hint to the driver that the app 
// is entering an idle state and that temporary buffers can be reclaimed for use by other apps.
void DX::DeviceResources::Trim()
{
    ComPtr<IDXGIDevice3> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    dxgiDevice->Trim();
}
```

## <a name="release-any-exclusive-resources-and-file-handles"></a>단독 리소스 및 파일 핸들 해제


앱은 [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) 이벤트를 처리할 때 단독 리소스와 파일 핸들을 해제할 수도 있습니다. 단독 리소스와 파일 핸들을 명시적으로 해제하면 앱이 해당 리소스와 파일 핸들을 사용하지 않는 동안 다른 앱이 액세스할 수 있습니다. 앱이 종료 후 활성화되면 단독 리소스와 파일 핸들을 열어야 합니다.

## <a name="remarks"></a>설명


사용자가 다른 앱 또는 데스크톱으로 전환할 때마다 시스템에서 앱을 일시 중단합니다. 사용자가 다시 돌아올 때마다 시스템에서 앱을 다시 시작합니다. 시스템에서 앱을 다시 시작할 때, 변수와 데이터 구조의 콘텐츠는 시스템에서 앱을 일시 중단하기 전과 동일합니다. 앱은 중단되었던 곳에서 정확히 복원되므로, 사용자에게는 앱이 배경에서 실행되고 있었던 것처럼 보입니다.

앱이 일시 중단된 동안 시스템은 앱과 데이터를 메모리에 유지하려고 합니다. 그러나 앱을 메모리에 유지할 리소스가 없으면 시스템은 앱을 종료합니다. 사용자가 일시 중단 후 일시 중단된 앱으로 다시 돌아오면, 시스템은 [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) 이벤트를 전송하며 **CoreApplicationView::Activated** 이벤트에 대한 처리기에서 응용 프로그램 데이터를 복원해야 합니다.

시스템은 앱이 종료되었을 때 앱에게 알리지 않으므로, 앱은 일시 중단될 때 응용 프로그램 데이터를 저장하고 단독 리소스와 파일 핸들을 해제하며 앱이 종료 후 활성화될 때 이 리소스와 파일 핸들을 복원해야 합니다.

## <a name="related-topics"></a>관련 항목

* [앱 다시 시작 방법(DirectX 및 C++)](how-to-resume-an-app-directx-and-cpp.md)
* [앱 활성화 방법(DirectX 및 C++)](how-to-activate-an-app-directx-and-cpp.md)

 

 




