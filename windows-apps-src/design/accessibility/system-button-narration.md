---
Description: ''
title: 화면 판독기 및 하드웨어 단추 이벤트
label: Screen readers and hardware button events
template: detail.hbs
ms.date: 02/20/2020
ms.topic: article
keywords: windows 10, uwp, 접근성, 내레이터, 화면 판독기
ms.localizationpriority: medium
ms.openlocfilehash: 41c6ed531f21a922b407ff3ba5aae93afb8d917e
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234936"
---
# <a name="screen-readers-and-hardware-system-buttons"></a>화면 판독기 및 하드웨어 시스템 단추

[내레이터](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator)와 같은 화면 판독기는 하드웨어 시스템 단추 이벤트를 인식 하 고 처리 하 고 사용자에 게 상태를 전달할 수 있어야 합니다. 경우에 따라 화면 판독기는 이러한 하드웨어 단추 이벤트를 단독으로 처리 해야 하 고 다른 처리기로 버블링 하지 않을 수 있습니다.

Windows 10 버전 2004부터 UWP 응용 프로그램은 다른 하드웨어 단추와 동일한 방법으로 **Fn** 하드웨어 시스템 단추 이벤트를 수신 하 고 처리할 수 있습니다. 이전에는이 시스템 단추가 다른 하드웨어 단추가 이벤트와 상태를 보고 하는 방법에 대 한 한정자로만 작동 했습니다.

> [!NOTE]
> Fn 단추 지원은 OEM에만 해당 되며, 해당 잠금 표시기 (블라인드 또는 시각 장애가 있는 사용자에 게 유용 하지 않을 수 있음)와 함께 설정/해제/잠금 설정/해제 하는 기능 (예: 누르고 있는 키 조합)과 같은 기능을 포함할 수 있습니다.

Fn 단추 이벤트는 새 [Systembuttoneventcontroller 클래스](/uwp/api/windows.ui.input.systembuttoneventcontroller) 를 통해 표시 [됩니다.](/uwp/api/windows.ui.input) SystemButtonEventController 개체는 다음 이벤트를 지원 합니다.

- [SystemFunctionButtonPressed](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionbuttonpressed)
- [SystemFunctionButtonReleased](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionbuttonreleased)
- [SystemFunctionLockChanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockchanged)
- [SystemFunctionLockIndicatorChanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockindicatorchanged)

> [!Important]
> SystemButtonEventController는 더 높은 우선 순위 처리기에서 이미 처리 된 이벤트를 수신할 수 없습니다.

## <a name="examples"></a>예

다음 예제에서는 DispatcherQueue를 기반으로 [Systembuttoneventcontroller](/uwp/api/windows.ui.input.systembuttoneventcontroller) 를 만들고이 개체에서 지 원하는 4 개의 이벤트를 처리 하는 방법을 보여 줍니다.

Fn 단추를 누르면 일반적으로 지원 되는 이벤트 중 두 개 이상이 발생 합니다. 예를 들어 Surface 키보드의 Fn 단추를 누르면 SystemFunctionButtonPressed, Systemfunctionbuttonpressed 및 SystemFunctionLockIndicatorChanged가 동시에 실행 됩니다.

1. 이 첫 번째 코드 조각에서는 필수 네임 스페이스를 포함 하 고, [Systembuttoneventcontroller](/uwp/api/windows.ui.input.systembuttoneventcontroller) 스레드를 관리 하기 위한 [Dispatcherqueue](/uwp/api/windows.system.dispatcherqueue) 및 [DispatcherQueueController](/uwp/api/windows.system.dispatcherqueuecontroller) 개체를 비롯 한 일부 전역 개체를 지정 합니다.

   그런 다음 [Systembuttoneventcontroller](/uwp/api/windows.ui.input.systembuttoneventcontroller) 이벤트 처리 대리자를 등록할 때 반환 되는 [이벤트 토큰](/uwp/cpp-ref-for-winrt/event-token) 을 지정 합니다.

    ```cppwinrt
    namespace winrt
    {
        using namespace Windows::System;
        using namespace Windows::UI::Input;
    }

    ...

    // Declare related members
    winrt::DispatcherQueueController _queueController;
    winrt::DispatcherQueue _queue;
    winrt::SystemButtonEventController _controller;
    winrt::event_token _fnKeyDownToken;
    winrt::event_token _fnKeyUpToken;
    winrt::event_token _fnLockToken;
    ```

2. 또한 bool과 함께 [SystemFunctionLockIndicatorChanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockindicatorchanged) 이벤트에 대 한 이벤트 토큰을 지정 하 여 응용 프로그램이 "학습 모드" 인지 여부를 나타냅니다. 여기서 사용자는 함수를 수행 하지 않고 단순히 키보드를 탐색 하려고 합니다.

    ```cppwinrt
    winrt::event_token _fnLockIndicatorToken;
    bool _isLearningMode = false;
    ```

3. 이 세 번째 코드 조각은 [Systembuttoneventcontroller](/uwp/api/windows.ui.input.systembuttoneventcontroller) 개체에서 지 원하는 각 이벤트에 대 한 해당 이벤트 처리기 대리자를 포함 합니다.

   각 이벤트 처리기는 발생 한 이벤트를 알립니다. 또한 FunctionLockIndicatorChanged 처리기는 앱이 "학습" 모드 (= true) 인지 여부를 제어 하 여 `_isLearningMode` 이벤트를 다른 처리기로 버블링 하 고 사용자가 실제로 작업을 수행 하지 않고 키보드 기능을 탐색할 수 있도록 합니다.

    ```cppwinrt
    void SetupSystemButtonEventController()
    {
        // Create dispatcher queue controller and dispatcher queue
        _queueController = winrt::DispatcherQueueController::CreateOnDedicatedThread();
        _queue = _queueController.DispatcherQueue();

        // Create controller based on new created dispatcher queue
        _controller = winrt::SystemButtonEventController::CreateForDispatcherQueue(_queue);

        // Add Event Handler for each different event
        _fnKeyDownToken = _controller->FunctionButtonPressed(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionButtonEventArgs& args)
            {
                // Mock function to read the sentence "Fn button is pressed"
                PronounceFunctionButtonPressedMock();
                // Set Handled as true means this event is consumed by this controller
                // no more targets will receive this event
                args.Handled(true);
            });

            _fnKeyUpToken = _controller->FunctionButtonReleased(
                [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionButtonEventArgs& args)
                {
                    // Mock function to read the sentence "Fn button is up"
                    PronounceFunctionButtonReleasedMock();
                    // Set Handled as true means this event is consumed by this controller
                    // no more targets will receive this event
                    args.Handled(true);
                });

        _fnLockToken = _controller->FunctionLockChanged(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionLockChangedEventArgs& args)
            {
                // Mock function to read the sentence "Fn shift is locked/unlocked"
                PronounceFunctionLockMock(args.IsLocked());
                // Set Handled as true means this event is consumed by this controller
                // no more targets will receive this event
                args.Handled(true);
            });

        _fnLockIndicatorToken = _controller->FunctionLockIndicatorChanged(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionLockIndicatorChangedEventArgs& args)
            {
                // Mock function to read the sentence "Fn lock indicator is on/off"
                PronounceFunctionLockIndicatorMock(args.IsIndicatorOn());
                // In learning mode, the user is exploring the keyboard. They expect the program
                // to announce what the key they just pressed WOULD HAVE DONE, without actually
                // doing it. Therefore, handle the event when in learning mode so the key is ignored
                // by the system.
                args.Handled(_isLearningMode);
            });
    }
    ```

## <a name="see-also"></a>참고 항목

[SystemButtonEventController 클래스](/uwp/api/windows.ui.input.systembuttoneventcontroller)
