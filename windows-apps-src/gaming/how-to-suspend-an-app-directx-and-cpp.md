---
title: 앱을 일시 중단 하는 방법 (DirectX 및 c + +)
description: 이 항목에서는 시스템에서 UWP (유니버설 Windows 플랫폼) DirectX 앱을 일시 중단 하는 경우 중요 한 시스템 상태 및 앱 데이터를 저장 하는 방법을 보여 줍니다.
ms.assetid: 5dd435e5-ec7e-9445-fed4-9c0d872a239e
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 일시 중단, directx
ms.localizationpriority: medium
ms.openlocfilehash: 28c93228f149b25ba129b632d23f9798712ee80d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163117"
---
# <a name="how-to-suspend-an-app-directx-and-c"></a>앱을 일시 중단 하는 방법 (DirectX 및 c + +)



이 항목에서는 시스템에서 UWP (유니버설 Windows 플랫폼) DirectX 앱을 일시 중단 하는 경우 중요 한 시스템 상태 및 앱 데이터를 저장 하는 방법을 보여 줍니다.

## <a name="register-the-suspending-event-handler"></a>일시 중단 이벤트 처리기 등록


먼저 사용자 또는 시스템 작업을 통해 앱이 일시 중단 된 상태로 이동할 때 발생 하는 [**CoreApplication::**](/uwp/api/windows.applicationmodel.core.coreapplication.suspending) suspended 이벤트를 처리 하도록 등록 합니다.

보기 공급자의 [**IFrameworkView:: Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize) 메서드 구현에 다음 코드를 추가 합니다.

```cpp
void App::Initialize(CoreApplicationView^ applicationView)
{
  //...
  
    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

  //...
}
```

## <a name="save-any-app-data-before-suspending"></a>일시 중단 하기 전에 모든 앱 데이터 저장


앱이 [**CoreApplication:: 일시 중단**](/uwp/api/windows.applicationmodel.core.coreapplication.suspending) 이벤트를 처리 하면 처리기 함수에 중요 한 응용 프로그램 데이터를 저장할 수 있습니다. 앱은 [**LocalSettings**](/uwp/api/windows.storage.applicationdata.localsettings) storage API를 사용 하 여 간단한 응용 프로그램 데이터를 동기식으로 저장 해야 합니다. 게임을 개발 하는 경우 중요 한 게임 상태 정보를 저장 합니다. 오디오 처리를 일시 중단 하는 것을 잊지 마세요.

이제 콜백을 구현 합니다. 이 메서드에 앱 데이터를 저장 합니다.

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

이 콜백은 5 초를 사용 하 여 완료 해야 합니다. 이 콜백 중에는 카운트다운을 시작 하는 [**SuspendingOperation:: GetDeferral**](/uwp/api/windows.applicationmodel.suspendingoperation.getdeferral)를 호출 하 여 지연을 요청 해야 합니다. 앱이 저장 작업을 완료할 때 [**SuspendingDeferral:: Complete**](/uwp/api/windows.applicationmodel.suspendingdeferral.complete) 를 호출 하 여 현재 앱을 일시 중단할 준비가 되었음을 시스템에 알립니다. 지연 시간을 요청 하지 않거나 앱이 데이터를 저장 하는 데 5 초 보다 오래 걸리면 앱이 자동으로 일시 중단 됩니다.

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

## <a name="call-trim"></a>Trim () 호출


Windows 8.1부터 모든 DirectX UWP 앱은 일시 중단할 때 [**IDXGIDevice3:: Trim**](/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgidevice3-trim) 을 호출 해야 합니다. 이 호출을 통해 앱에 할당 된 모든 임시 버퍼를 해제 하도록 그래픽 드라이버에 지시할 수 있습니다. 그러면 일시 중단 상태에서 메모리 리소스를 회수 하기 위해 앱이 종료 될 가능성이 줄어듭니다. Windows 8.1에 대 한 인증 요구 사항입니다.

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

## <a name="release-any-exclusive-resources-and-file-handles"></a>모든 배타 리소스 및 파일 핸들 해제


앱이 [**CoreApplication:: 일시 중단**](/uwp/api/windows.applicationmodel.core.coreapplication.suspending) 이벤트를 처리 하는 경우 배타 리소스 및 파일 핸들을 릴리스할 수 있습니다. 단독 리소스 및 파일 핸들을 명시적으로 해제 하면 앱에서 사용 하지 않는 동안 다른 앱에서 해당 리소스에 액세스할 수 있습니다. 종료 후에 앱이 활성화 되 면 단독 리소스 및 파일 핸들을 열어야 합니다.

## <a name="remarks"></a>설명


사용자가 다른 앱 이나 바탕 화면으로 전환할 때마다 시스템이 앱을 일시 중단 합니다. 사용자가 다시 전환할 때마다 시스템이 앱을 다시 시작 합니다. 시스템이 앱을 다시 시작할 때 변수 및 데이터 구조의 내용은 시스템이 앱을 일시 중단 하기 전과 동일 합니다. 시스템은 백그라운드에서 실행 중인 것 처럼 사용자에 게 표시 되도록, 중단 된 위치에서 앱을 복원 합니다.

시스템은 일시 중단 된 상태에서 앱과 해당 데이터를 메모리에 유지 하려고 합니다. 그러나 시스템에 앱을 메모리에 보관 하는 리소스가 없는 경우 시스템에서 앱이 종료 됩니다. 사용자가 종료 된 일시 중단 된 앱으로 다시 전환 되 면 시스템은 [**활성화**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) 된 이벤트를 보내고 **CoreApplicationView:: 활성화** 된 이벤트에 대 한 해당 처리기의 응용 프로그램 데이터를 복원 해야 합니다.

시스템은 종료 될 때 앱에 알리지 않으므로 응용 프로그램에서 응용 프로그램 데이터를 저장 하 고 일시 중단 된 경우 전용 리소스 및 파일 핸들을 해제 하 고 종료 후에 앱이 활성화 될 때 응용 프로그램을 복원 해야 합니다.

## <a name="related-topics"></a>관련 항목

* [앱을 다시 시작 하는 방법 (DirectX 및 c + +)](how-to-resume-an-app-directx-and-cpp.md)
* [앱을 활성화 하는 방법 (DirectX 및 c + +)](how-to-activate-an-app-directx-and-cpp.md)

 

 