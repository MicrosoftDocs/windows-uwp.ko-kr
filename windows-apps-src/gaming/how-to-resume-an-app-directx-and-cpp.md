---
title: 앱 다시 시작 방법(DirectX 및 C++)
description: 이 항목에서는 시스템이 UWP(유니버설 Windows 플랫폼) DirectX 앱을 다시 시작할 때 중요한 응용 프로그램 데이터를 복원하는 방법을 보여 줍니다.
ms.assetid: 5e6bb673-6874-ace5-05eb-f88c045f2178
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 다시 시작, directx
ms.localizationpriority: medium
ms.openlocfilehash: f0aa60061ae9fc14392bfe4beb0693ba50fda0df
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8469616"
---
# <a name="how-to-resume-an-app-directx-and-c"></a>앱 다시 시작 방법(DirectX 및 C++)



이 항목에서는 시스템이 UWP(유니버설 Windows 플랫폼) DirectX 앱을 다시 시작할 때 중요한 응용 프로그램 데이터를 복원하는 방법을 보여 줍니다.

## <a name="register-the-resuming-event-handler"></a>resuming 이벤트 처리기 등록


사용자가 앱에서 다른 곳으로 전환했다가 돌아왔음을 나타내는 [**CoreApplication::Resuming**](https://msdn.microsoft.com/library/windows/apps/br205859) 이벤트를 처리하도록 등록합니다.

뷰 공급자의 [**IFrameworkView::Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) 메서드 구현에 다음 코드를 추가합니다.

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

## <a name="refresh-displayed-content-after-suspension"></a>일시 중단 후 표시 콘텐츠 새로 고침


앱에서 Resuming 이벤트를 처리하면 표시 콘텐츠를 새로 고칠 기회가 생깁니다. [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860)에 대한 처리기를 사용하여 저장한 앱을 복원하고 처리를 다시 시작합니다. 게임 개발: 오디오 엔진을 일시 중단한 경우 이제 다시 시작합니다.

```cpp
void App::OnResuming(Platform::Object^ sender, Platform::Object^ args)
{
    // Restore any data or state that was unloaded on suspend. By default, data
    // and state are persisted when resuming from suspend. Note that this event
    // does not occur if the app was previously terminated.

    // Insert your code here.
}
```

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

## <a name="remarks"></a>설명


사용자가 다른 앱 또는 데스크톱으로 전환할 때마다 시스템에서 앱을 일시 중단합니다. 사용자가 다시 돌아올 때마다 시스템에서 앱을 다시 시작합니다. 시스템에서 앱을 다시 시작할 때, 변수와 데이터 구조의 콘텐츠는 시스템에서 앱을 일시 중단하기 전과 동일합니다. 앱은 중단되었던 곳에서 정확히 복원되므로, 사용자에게는 앱이 배경에서 실행되고 있었던 것처럼 보입니다. 그러나 앱은 상당히 오랜 기간 동안 일시 중단된 것일 수 있으므로, 일시 중단 기간 중에 변경되었을 수 있는 표시 콘텐츠를 새로 고쳐야 하며 렌더링 또는 오디오 처리 스레드를 다시 시작해야 합니다. 이전 일시 중단 이벤트 중에 게임 상태 데이터를 저장한 경우 지금 복원합니다.

## <a name="related-topics"></a>관련 항목

* [앱 일시 중단 방법(DirectX 및 C++)](how-to-suspend-an-app-directx-and-cpp.md)
* [앱 활성화 방법(DirectX 및 C++)](how-to-activate-an-app-directx-and-cpp.md)

 

 




