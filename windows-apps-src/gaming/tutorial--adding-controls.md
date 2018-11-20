---
author: abbycar
title: 컨트롤 추가
description: 이제 게임 샘플이 3D 게임에서 이동-보기 컨트롤을 구현하는 방법 및 기본 터치, 마우스 및 게임 컨트롤러 컨트롤을 개발하는 방법을 살펴보겠습니다.
ms.assetid: f9666abb-151a-74b4-ae0b-ef88f1f252f8
ms.author: abigailc
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 컨트롤, 입력
ms.localizationpriority: medium
ms.openlocfilehash: bc5873486bdd9c4adf4ea74b10a240617143ad23
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7301632"
---
# <a name="add-controls"></a>컨트롤 추가


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

좋은 UWP(유니버설 Windows 플랫폼) 게임은 다양한 인터페이스를 지원합니다. 플레이어는 Xbox 컨트롤러가 연결 된 PC는 실제 단추가 없는 태블릿에 Windows10 있을 또는 고성능 마우스 및 게임용 키보드가 있는 최신 데스크톱 게임 리그 합니다. 게임에서 컨트롤은 [**MoveLookController**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp) 클래스에 구현되어 있습니다. 이 클래스는 세 가지 유형의 입력(마우스 및 키보드, 터치, 게임 패드) 모두를 단일 컨트롤러에 집계합니다. 그 결과로 1인칭 슈팅 게임에서 여러 디바이스에서 작동하는 장르 표준 이동 - 보기 컨트롤러를 사용할 수 있게 되었습니다.

> [!NOTE]
> 컨트롤에 대한 자세한 내용은 [게임용 이동 - 보기 컨트롤](tutorial--adding-move-look-controls-to-your-directx-game.md) 및 [게임용 터치 컨트롤](tutorial--adding-touch-controls-to-your-directx-game.md)을 참조하세요.


## <a name="objective"></a>목표

이제 게임이 렌더링되고 있지만, 플레이어를 이동시키거나 대상에게 슈팅할 수는 없습니다. UWP DirectX 게임에서 다음과 같은 유형의 입력에 대해 1인칭 슈팅 게임 이동 - 보기 컨트롤을 구현하는 방법에 대해 살펴보겠습니다.
- 마우스 및 키보드
- 터치
- 게임 패드

>[!Note]
>이 샘플의 최신 게임 코드를 다운로드하지 않은 경우 [Direct3D 게임 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)로 이동합니다. 이 샘플은 UWP 기능 샘플의 큰 컬렉션의 일부입니다. 샘플을 다운로드하는 방법에 대한 지침은 [GitHub에서 UWP 샘플 가져오기](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)를 참조하세요.

## <a name="common-control-behaviors"></a>공용 컨트롤 동작


터치 컨트롤 및 마우스/키보드 컨트롤의 핵심 구현은 매우 유사합니다. UWP 앱에서 포인터는 화면에 표시되는 점일 뿐입니다. 마우스를 밀거나 터치 스크린에서 손가락을 밀어 이동할 수 있습니다. 따라서 단일 이벤트 집합을 등록할 수 있으며 플레이어가 포인터를 이동하고 누르는 데 마우스를 사용하는지 터치 스크린을 사용하는지에 대해 걱정하지 않아도 됩니다.

게임 샘플의 **MoveLookController** 클래스를 초기화하면 4개의 포인터 관련 이벤트와 1개의 마우스 관련 이벤트가 등록됩니다.

이벤트 | 설명
:------ | :-------
[**CoreWindow::PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278) | 마우스 왼쪽 또는 오른쪽 단추를 누르고 있거나 터치 표면을 터치했습니다.
[**CoreWindow::PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) |마우스를 이동하거나 터치 표면에서 끌기 작업을 수행했습니다.
[**CoreWindow::PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) |마우스 왼쪽 단추를 놓았거나 터치 표면에 닿는 개체를 들어 올렸습니다.
[**CoreWindow::PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208275) |포인터를 주 창 밖으로 이동했습니다.
[**Windows::Devices::Input::MouseMoved**](https://msdn.microsoft.com/library/windows/apps/hh758356) | 마우스를 특정 거리만큼 이동했습니다. 마우스 이동 델타 값에만 관심이 있고 현재 X-Y 위치는 표시하지 않습니다.


이러한 이벤트 처리기는 응용 프로그램 창에서 **MoveLookController**가 초기화되는 즉시 사용자 입력을 수신하기 시작하도록 설정되어 있습니다.
```cpp
void MoveLookController::InitWindow(_In_ CoreWindow^ window)
{
    ResetState();

    window->PointerPressed +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

    window->PointerMoved +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

    window->PointerReleased +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);

    window->PointerExited +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerExited);

    // There is a separate handler for mouse only relative mouse movement events.
    MouseDevice::GetForCurrentView()->MouseMoved +=
        ref new TypedEventHandler<MouseDevice^, MouseEventArgs^>(this, &MoveLookController::OnMouseMoved);
    //
    // ...
    //
}
```

[**InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L68-L92)에 대한 전체 코드는 GitHub에서 확인할 수 있습니다.


게임이 특정 입력을 반드시 수신해야 하는 경우를 판단할 수 있도록 **MoveLookController** 클래스는 컨트롤러 유형에 관계 없이 3개의 컨트롤러 관련 상태를 가집니다.

시 | 설명
:----- | :-------
**없음** | 컨트롤러의 초기화된 상태입니다. 게임에서 어떤 컨트롤러 입력도 예상되지 않기 때문에 모든 입력이 무시됩니다.
**WaitForInput** | 컨트롤러는 왼쪽 마우스 클릭, 터치 이벤트 또는 게임 패드의 메뉴 단추를 사용하여 플레이어가 게임에서 메시지를 승인하는 동안 기다립니다.
**활성** | 컨트롤러는 활성 게임 플레이 모드에 있습니다.



### <a name="waitforinput-state-and-pausing-the-game"></a>WaitForInput 상태와 게임 일시 중지

일시 중지되면 게임은 **WaitForInput** 상태가 됩니다. 플레이어가 게임의 주 창 밖에서 포인터를 이동시키거나 일시 중지 단추(P 키나 게임 패드의 **시작** 단추)를 누를 때 이러한 일시 중지가 발생합니다. **MoveLookController**는 이러한 단추 누름을 등록하고 [**IsPauseRequested**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L107-L127) 메서드가 호출될 때 게임 루프에 알립니다. 이때 **IsPauseRequested**에서 **true**를 반환하면 게임 루프에서 **MoveLookController**의 **WaitForPress**를 호출하여 컨트롤러를 **WaitForInput** 상태로 전환합니다. 


**WaitForInput** 상태가 되면 **활성** 상태로 되돌아갈 때까지 게임에서 거의 모든 게임 플레이 입력 이벤트의 처리가 중지됩니다. 예외는 일시 중지 단추인데, 이 단추를 누르면 게임이 활성 상태로 되돌아갈 수 있습니다. 일시 중지 단추 이외의 **활성** 상태 돌아가려면 게임 플레이어가 컴파일하려면 메뉴 항목을 선택 합니다. 



### <a name="the-active-state"></a>활성 상태

**활성** 상태 동안에는 **MoveLookController** 인스턴스가 활성화된 모든 입력 장치에서 이벤트를 처리하고 플레이어의 의도를 해석합니다. 그 결과로, 플레이어 보기에서 속도 및 보기 방향을 업데이트하고 게임 루프에서 **업데이트**가 호출된 후 업데이트된 데이터를 게임과 공유합니다.


모든 포인터 입력은 **활성** 상태에서 추적되며 포인터 작업에 따라 서로 다른 포인터 ID를 사용합니다.
[**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278) 이벤트를 받으면 **MoveLookController**는 창에서 만든 포인터 ID 값을 가져옵니다. 포인터 ID는 특정 유형의 입력을 나타냅니다. 예를 들어 멀티 터치 장치에는 여러 가지 다른 활성 입력이 동시에 있을 수 있습니다. 이 ID는 플레이어가 사용하는 입력을 추적하는 데 사용됩니다. 한 이벤트가 터치 스크린의 이동 사각형에 있으면 포인터 ID가 할당되어 이동 사각형의 모든 포인터 이벤트를 추적합니다. 실행 사각형의 다른 포인터 이벤트는 별도의 포인터 ID를 사용하여 별도로 추적됩니다.


> [!NOTE]
> 마우스에서의 입력과 게임 패드의 오른쪽 섬스틱에서의 입력은 별도로 처리되는 ID를 갖습니다.

포인터 이벤트를 특정 게임 작업에 매핑했으면 이제 **MoveLookController** 개체가 주 게임 루프와 공유하는 데이터를 업데이트합니다.

게임 샘플에서 [**Update**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1005-L1096) 메서드를 호출하면 입력이 처리되고 속도 및 보기 방향 변수(**m\_velocity** 및 **m\_lookdirection**)가 업데이트됩니다. 그런 다음, 게임 루프가 공개된 [**Velocity**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L906-L909) 및 [**LookDirection**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L913-L923) 메서드를 호출하여 검색을 합니다.

> [!NOTE]
> [**Update**](#the-update-method) 메서드에 대한 자세한 내용은 이 페이지 뒷부분에서 확인할 수 있습니다.




게임 루프는 **MoveLookController** 인스턴스에서 **IsFiring** 메서드를 호출하여 플레이어가 실행 중인지 여부를 알아보기 위한 테스트를 할 수 있습니다. **MoveLookController**는 플레이어가 세 개의 입력 유형 중 하나에서 실행 단추를 눌렀는지 확인합니다.

```cpp
bool MoveLookController::IsFiring()
{
    if (m_state == MoveLookControllerState::Active)
    {
        if (m_autoFire)
        {
            return (m_fireInUse || (m_mouseInUse && m_mouseLeftInUse) || PollingFireInUse());
        }
        else
        {
            if (m_firePressed)
            {
                m_firePressed = false;
                return true;
            }
        }
    }
    return false;
}
```









이제 세 가지 컨트롤 유형의 구현에 대해 좀더 자세히 살펴보겠습니다.

## <a name="adding-relative-mouse-controls"></a>상대 마우스 컨트롤 추가


마우스 이동이 감지되면 해당 이동을 사용하여 카메라의 새 피치와 요를 결정할 수 있습니다. 이를 위해 상대 마우스 컨트롤을 구현합니다. 여기서는 이동의 절대 x-y 픽셀 좌표 기록과 반대로 마우스가 이동한 상태 거리(이동의 시작과 정지 사이의 델타)를 처리합니다.

이를 구현하기 위해 [**MouseMoved**](https://msdn.microsoft.com/library/windows/apps/hh758356) 이벤트에서 반환된 [**Windows::Device::Input::MouseEventArgs::MouseDelta**](https://msdn.microsoft.com/library/windows/apps/hh758358) 인수 개체의 [**MouseDelta::X**](https://msdn.microsoft.com/library/windows/apps/hh758353) 및 **MouseDelta::Y** 필드를 검토하여 X(가로 이동) 및 Y(세로 이동) 좌표의 변경을 확인합니다.

```cpp
void MoveLookController::OnMouseMoved(
    _In_ MouseDevice^ /* mouseDevice */,
    _In_ MouseEventArgs^ args
    )
{
    // Handle Mouse Input via dedicated relative movement handler.

    switch (m_state)
    {
    case MoveLookControllerState::Active:
        XMFLOAT2 mouseDelta;
        mouseDelta.x = static_cast<float>(args->MouseDelta.X);
        mouseDelta.y = static_cast<float>(args->MouseDelta.Y);

        XMFLOAT2 rotationDelta;
        rotationDelta.x  = mouseDelta.x * MoveLookConstants::RotationGain;   // scale for control sensitivity
        rotationDelta.y  = mouseDelta.y * MoveLookConstants::RotationGain;

        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;
        m_yaw   += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        float limit = XM_PI / 2.0f - 0.01f;
        m_pitch = __max(-limit, m_pitch);
        m_pitch = __min(+limit, m_pitch);

        // Keep longitude in same range by wrapping.
        if (m_yaw >  XM_PI)
        {
            m_yaw -= XM_PI * 2.0f;
        }
        else if (m_yaw < -XM_PI)
        {
            m_yaw += XM_PI * 2.0f;
        }
        break;
    }
}
```

## <a name="adding-touch-support"></a>터치 지원 추가

터치 컨트롤은 태블릿에서 사용자를 지원하기에 적합합니다. 이 게임은 특정한 게임 내 작업에 맞게 각각을 정렬하면서 화면의 특정 영역에 대한 영역 지정을 해제하여 터치 입력을 수집합니다.
이 게임의 터치 입력은 세 가지 영역을 사용합니다.

![이동 - 보기 터치 레이아웃](images/simple-dx-game-controls-touchzones.png)

다음 명령에는 터치 컨트롤 동작이 요약되어 있습니다.
사용자 입력 | 작업
:------- | :--------
이동 사각형 | 터치 입력은 가상 조이스틱으로 변환되는데, 세로 이동은 전방/후방 위치 동작으로, 가로 이동은 왼쪽/오른쪽 위치 동작으로 해석됩니다.
실행 사각형 | 구를 실행합니다.
이동 및 실행 사각형 밖을 터치 | 카메라 보기의 회전(피치 및 요)을 변경

**MoveLookController**는 포인터 ID를 검사하여 이벤트가 발생한 위치를 확인하고 다음 작업 중 하나를 수행합니다.

-   [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) 이벤트가이동 또는 발생 사각형에서 발생한 경우 컨트롤러의 포인터 위치를 업데이트합니다.
-   [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) 이벤트가 화면의 나머지 영역에서 발생한 경우(보기 컨트롤로 정의됨) 보기 방향 벡터의 피치와 요 변화를 계산합니다.


일단 터치 스크롤이 구현되면 이전에 Direct2D를 사용해 그렸던 사각형이 이동, 실행 및 모양 영역이 어디인지를 플레이어에게 나타냅니다.


![터치 컨트롤](images/simple-dx-game-controls-showzones.png)


이제 각 컨트롤을 구현하는 방법을 살펴보겠습니다.


### <a name="move-and-fire-controller"></a>이동 및 실행 컨트롤러
화면의 왼쪽 아래 사분면에 있는 이동 컨트롤러 사각형은 방향 패드로 사용됩니다. 이 공간의 왼쪽과 오른쪽으로 엄지 손가락을 밀면 플레이어가 왼쪽과 오른쪽으로 이동하고 위쪽과 아래쪽으로 밀면 카메라가 앞뒤로 이동합니다.
이 설정 후 화면의 왼쪽 아래 사분면에 있는 컨트롤러를 터치하면 구가 실행됩니다.

[**SetMoveRect**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L843-L853) 및 [**SetFireRect**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L857-L867) 메서드는 2개의 2D 벡터를 가져와서 화면에서 각 사각형의 왼쪽 위와 오른쪽 아래 모서리 위치를 지정하여 입력 사각형을 만듭니다. 


이제 **m\_fireUpperLeft** 및 **m\_fireLowerRight**에 매개 변수가 할당되는데, 이들은 사용자가 사각형 내부를 터치하고 있는지 판단하는 데 도움이 됩니다. 
```cpp
m_fireUpperLeft  = upperLeft;
m_fireLowerRight = lowerRight;
```

화면 크기를 조정하는 경우 이러한 사각형은 적정 크기로 다시 그려집니다.


컨트롤에 대한 영역 지정이 해제되면 사용자가 실제로 이를 사용하는 시기를 결정합니다.
이를 위해 사용자가 포인터를 누르거나 이동하거나 해제하는 경우에 대해 **MoveLookController::InitWindow** 메서드에서 몇 가지 이벤트 처리기를 설정합니다.

```cpp
window->PointerPressed +=
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

window->PointerMoved +=
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

window->PointerReleased +=
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);
```


[**OnPointerPressed**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L179-L313) 메서드를 사용하여 이동 또는 실행 사각형 안을 사용자가 처음으로 누르면 어떻게 되는지를 가장 먼저 확인합니다.
여기에서 컨트롤 터치 영역과 포인터가 해당 컨트롤러에 이미 있는지 여부를 확인합니다. 손가락으로 특정 컨트롤을 터치하는 것이 처음이라면 다음과 같이 합니다.
- **m\_moveFirstDown** 또는 **m\_fireFirstDown**에 터치한 위치를 2D 벡터로 저장합니다.
- **m\_movePointerID** 또는 **m\_firePointerID**에 포인터 ID를 할당합니다.
- 해당 컨트롤에 대해 포인터가 활성 상태가 되었기 때문에 해당되는 **InUse** 플래그(**m\_moveInUse** 또는 **m\_fireInUse**)를 `true`로 설정합니다.


```cpp
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;
    auto pointerDeviceType = pointerDevice->PointerDeviceType;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);

    case MoveLookControllerState::Active:
        switch (pointerDeviceType)
        {
        case Windows::Devices::Input::PointerDeviceType::Touch:
            // Check to see if this pointer is in the move control.
            if (position.x > m_moveUpperLeft.x &&
                position.x < m_moveLowerRight.x &&
                position.y > m_moveUpperLeft.y &&
                position.y < m_moveLowerRight.y)
            {
                if (!m_moveInUse)         // If no pointer is in this control yet:
                {
                    // Process a DPad touch down event.
                    m_moveFirstDown = position;                 // Save the location of the initial contact
                    m_movePointerID = pointerID;                // Store the pointer using this control
                    m_moveInUse = true;                         // Set InUse flag to signal there is an active move pointer
                }
            }
            // Check to see if this pointer is in the fire control.
            else if (position.x > m_fireUpperLeft.x &&
                position.x < m_fireLowerRight.x &&
                position.y > m_fireUpperLeft.y &&
                position.y < m_fireLowerRight.y)
            {
                if (!m_fireInUse)
                {
                    m_fireLastPoint = position;     // Save the location of the initial contact
                    m_firePointerID = pointerID;    // Store the pointer using this control
                    m_fireInUse = true;             // Set InUse flag to signal there is an active fire pointer
                }
            }
            ...
```


사용자가 이동 컨트롤을 터치하고 있는지 실행 컨트롤을 터치하고 있는지 판단했다면 이제는 플레이어가 손가락을 눌러 이동을 하고 있는지 확인합니다.
[**MoveLookController::OnPointerMoved**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L317-L395) 메서드를 사용하여 어떤 포인터가 이동되었는지 확인하고 새 위치를 2D 벡터로 저장합니다.  


```cpp
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // convert to allow math
    
    switch (m_state)
    {
    case MoveLookControllerState::Active:
        // Decide which control this pointer is operating.
        
        // Move control
        if (pointerID == m_movePointerID)
        {
            m_movePointerPosition = position;       // Save the current position.
        }
        // Look control
        else if (pointerID == m_lookPointerID)
        {
            // ...
        }
        // Fire control
        else if (pointerID == m_firePointerID)
        {
            m_fireLastPoint = position;
        }
```


사용자가 컨트롤 내에서 제스처를 하면 포인터가 해제됩니다. [**MoveLookController::OnPointerReleased**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) 메서드를 사용하여 어떤 포인터가 해제되었는지 확인하고 일련의 재설정을 수행합니다.


이동 컨트롤이 해제된 경우에는 다음을 수행합니다.
- 게임 시 이동되는 일이 없도록 모든 방향에서 플레이어의 속도를 `0`으로 설정합니다.
- 사용자가 더 이상 이동 컨트롤러를 터치하지 않기 때문에 **m\_moveInUse**를 `false`로 전환합니다.
- 이동 컨트롤러에 더 이상 포인터가 없기 때문에 이동 포인터 ID를 `0`으로 설정합니다.

```cpp
       if (pointerID == m_movePointerID)
        {
            m_velocity = XMFLOAT3(0, 0, 0);      // Stop on release.
            m_moveInUse = false;
            m_movePointerID = 0;
        }
```


실행 컨트롤이 해제된 경우에는 실행 컨트롤에 더 이상 포인터가 없기 때문에 **m_fireInUse** 플래그를 `false`로, 실행 포인터 ID를 `0`으로 전환하기만 하면 됩니다.
```cpp
        else if (pointerID == m_firePointerID)
        {
            m_fireInUse = false;
            m_firePointerID = 0;
        }
```

### <a name="look-controller"></a>보기 컨트롤러
화면에서 사용되지 않은 영역에 대한 터치 디바이스 포인터 이벤트는 보기 컨트롤러로 처리됩니다. 이 영역 주위를 손가락을 밀어 플레이어 카메라의 피치와 요(회전)를 변경합니다.


**MoveLookController::OnPointerPressed** 이벤트가 이 영역의 터치 디바이스에서 발생하고 게임 상태가 **활성**으로 설정이 되어 있으면 포인터 ID가 할당됩니다.

```cpp
    if (!m_lookInUse)   // If no pointer is in this control yet:
    {
        m_lookLastPoint = position;                   // Save the pointer for a later move.
        m_lookPointerID = pointerID;                  // Store the pointer using this control.
        m_lookLastDelta.x = m_lookLastDelta.y = 0;    // These are for smoothing.
        m_lookInUse = true;
    }
```
[GitHub](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L252-L259)에서 **MoveLookController::OnPointerPressed** 메서드에 대한 전체 코드를 확인할 수 있습니다.




여기에서 **MoveLookController**는 이벤트를 실행한 포인터의 포인터 ID를 보기 영역에 해당되는 특정 변수에 할당합니다. 보기 영역에서 발생 하는 터치의 경우 **m\_lookPointerID** 변수가 이벤트를 발생 시킨 포인터 ID로 설정 됩니다. 부울 변수 **m\_lookInUse**는 아직 컨트롤이 해제되지 않았음을 나타내도록 설정됩니다.

이제 게임 샘플이 [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) 터치 스크린 이벤트를 처리하는 방법에 대해 살펴보겠습니다.


**MoveLookController::OnPointerMoved** 메서드 내에서 어떤 종류의 포인터 ID가 이벤트에 할당되었는지 확인합니다. **m_lookPointerID**의 경우에는 포인터의 위치에서 변경을 계산합니다.
그런 다음, 이 델타 값을 사용하여 회전을 어느 정도 변경해야 하는지를 계산합니다. 마지막으로 게임에 사용할 **m\_pitch** 및 **m\_yaw**를 업데이트하여 플레이어 회전을 변경할 수 있습니다. 

```cpp
    else if (pointerID == m_lookPointerID)     // This is the look pointer.
    {
        // Look control
        XMFLOAT2 pointerDelta;
        pointerDelta.x = position.x - m_lookLastPoint.x;        // How far did the pointer move.
        pointerDelta.y = position.y - m_lookLastPoint.y;

        XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x * MoveLookConstants::RotationGain;       // Scale for control sensitivity.
        rotationDelta.y = pointerDelta.y * MoveLookConstants::RotationGain;
        m_lookLastPoint = position;                             // Save for the next time through.

        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;
        m_yaw   += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        float limit = XM_PI / 2.0f - 0.01f;
        m_pitch = __max(-limit, m_pitch);
        m_pitch = __min(+limit, m_pitch);M_PI / 2.0f, m_pitch);
        }
```





마지막으로 게임 샘플이 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) 터치 스크린 이벤트를 어떻게 처리하는지 살펴보겠습니다.
사용자가 터치 제스처를 완료하고 화면에서 손을 떼면 [**MoveLookController::OnPointerReleased**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500)가 시작됩니다.
[**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) 이벤트를 실행한 포인터의 ID가 이전에 기록된 이동 포인터의 ID이면 플레이어가 보기 영역에 대한 터치를 중지한 것이기 때문에 **MoveLookController**에서 속도가 `0`으로 설정됩니다.

```cpp
    else if (pointerID == m_lookPointerID)
    {
        m_lookInUse = false;
        m_lookPointerID = 0;
    }
```





## <a name="adding-mouse-and-keyboard-support"></a>마우스 및 키보드 지원 추가


이 게임은 키보드 및 마우스에 대해 다음과 같은 컨트롤 레이아웃을 가지고 있습니다.

사용자 입력 | 작업
:------- | :--------
W | 플레이어를 앞으로 이동
A | 플레이어를 왼쪽으로 이동
S | 플레이어를 뒤로 이동
D | 플레이어를 오른쪽으로 이동
X | 보기를 위로 이동
스페이스바 | 보기를 아래로 이동
P | 게임을 일시 중지
마우스 이동 | 카메라 보기의 회전(피치 및 요) 변경
왼쪽 마우스 단추 | 구 실행


키보드를 사용하기 위해 게임 샘플은 [**MoveLookController::InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L84-L88) 메서드 내에 2개의 새로운 이벤트([**CoreWindow::KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208271) 및 [**CoreWindow::KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208270))를 등록합니다. 이러한 이벤트는 키 누름과 해제를 처리합니다.

```cpp
window->KeyDown +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyDown);

window->KeyUp +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyUp);
```

마우스는 포인터를 사용하기는 하지만 터치 스크롤과 약간 다르게 처리됩니다. 컨트롤 레이아웃에 맞춰지도록 **MoveLookController**는 마우스가 이동될 때마다 카메라를 회전시키고 왼쪽 마우스 단추를 누를 때 이벤트를 실행합니다.


이는 **MoveLookController**의 [**OnPointerPressed**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L179-L313) 메서드에서 처리됩니다.

이 메서드에서 [`Windows::Devices::Input::PointerDeviceType`](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Input.PointerDeviceType) 열거형 값에서 어떤 종류의 포인터 디바이스가 사용되고 있는지 확인합니다. 게임이 **활성** 상태이고 **PointerDeviceType**가 **터치**가 아니면 이를 마우스 입력으로 가정합니다.

```cpp
    case MoveLookControllerState::Active:
        switch (pointerDeviceType)
        {
        case Windows::Devices::Input::PointerDeviceType::Touch:
            // Behavior for touch controls
        
        default:
        // Behavior for mouse controls
            bool rightButton = pointProperties->IsRightButtonPressed;
            bool leftButton = pointProperties->IsLeftButtonPressed;

            if (!m_autoFire && (!m_mouseLeftInUse && leftButton))
            {
                m_firePressed = true;
            }

            if (!m_mouseInUse)
            {
                m_mouseInUse = true;
                m_mouseLastPoint = position;
                m_mousePointerID = pointerID;
                m_mouseLeftInUse = leftButton;
                m_mouseRightInUse = rightButton;
                m_lookLastDelta.x = m_lookLastDelta.y = 0;          // These are for smoothing.
            }
            break;
        }
```

플레이어가 마우스 단추 중 하나를 누르는 것을 중지하면 [CoreWindow::PointerReleased](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow.PointerReleased) 마우스 이벤트가 발생하면서 [MoveLookController::OnPointerReleased](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) 메서드가 호출되고 입력이 완료됩니다. 이 때 왼쪽 마우스 단추를 누르고 있다가 놓으면 구의 실행이 중지됩니다. 보기 이벤트는 항상 활성화되어 있기 때문에 게임에서 동일한 마우스 포인터를 사용하여 진행 중인 보기 이벤트를 추적할 수 있습니다.

```cpp
    case MoveLookControllerState::Active:
        // Touch points
        if (pointerID == m_movePointerID)
        {
            // Stop movement
        }
        else if (pointerID == m_lookPointerID)
        {
            // Stop look rotation
        }
        // Fire button has been released
        else if (pointerID == m_firePointerID)
        {
            // Stop firing
        }
        // Mouse point
        else if (pointerID == m_mousePointerID)
        {
            bool rightButton = pointProperties->IsRightButtonPressed;
            bool leftButton = pointProperties->IsLeftButtonPressed;

            // Mouse no longer in use so stop firing
            m_mouseInUse = false;

            // Don't clear the mouse pointer ID so that Move events still result in Look changes.
            // m_mousePointerID = 0;
            m_mouseLeftInUse = leftButton;
            m_mouseRightInUse = rightButton;
        }
        break;
    }
}
```



이제 마지막 지원 컨트롤 유형인 게임 패드에 대해 살펴보겠습니다. 게임 패드는 포인터 개체를 사용하지 않기 때문에 터치 및 마우스 컨트롤과 별개로 처리됩니다. 이 때문에 몇 가지 이벤트 처리기와 메서드를 새로 추가해야 합니다.

## <a name="adding-gamepad-support"></a>게임 패드 지원 추가


이 게임에서는 [Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input) API 호출을 통해 게임 패드 지원이 추가됩니다. 이 API 집합은 레이싱 휠, 플라이트 스틱 같은 게임 컨트롤러 입력에 액세스할 수 있도록 해줍니다. 


게임 패드 컨트롤은 다음과 같습니다.

사용자 입력 | 작업
:------- | :--------
왼쪽 아날로그 스틱 | 플레이어를 이동
오른쪽 아날로그 스틱 | 카메라 보기의 회전(피치 및 요) 변경
오른쪽 트리거 | 구 실행
시작/메뉴 단추 | 게임 일시 중지/재개




[**InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L68-L103) 메서드에서 게임 패드가 [추가](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1100-L1105) 상태인지 [제거](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1109-L1114) 상태인지 판단하기 위해 2개의 이벤트가 새로 추가되었습니다. 이러한 이벤트는 **m_gamepadsChanged** 속성을 업데이트합니다. **UpdatePollingDevices** 메서드에서 알려진된 게임 패드의 목록이 변경 되었는지 여부를 확인 하려면 사용 됩니다. 

```cpp
    // Detect gamepad connection and disconnection events.
    Gamepad::GamepadAdded +=
        ref new EventHandler<Gamepad^>(this, &MoveLookController::OnGamepadAdded);

    Gamepad::GamepadRemoved +=
        ref new EventHandler<Gamepad^>(this, &MoveLookController::OnGamepadRemoved);
```

### <a name="the-updatepollingdevices-method"></a>UpdatePollingDevices 메서드

**MoveLookController** 인스턴스의 [**UpdatePollingDevices**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L654-L782) 메서드는 즉시 게임 패드의 연결 여부를 확인합니다. 연결이 되어 있으면 [**Gamepad.GetCurrentReading**](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.GetCurrentReading)로 상태 판독을 시작합니다. 이렇게 하면 [**GamepadReading**](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.GamepadReading) 구조체가 반환되기 때문에 어떤 단추가 클릭되었는지, 어떤 섬스틱이 이동되었는지 확인할 수 있습니다.


게임의 상태가 **WaitForInput**이면 게임이 재개될 수 있도록 컨트롤러의 시작/메뉴 단추에 대해서만 수신 대기를 합니다 .


상태가 **활성**이면 사용자 입력을 확인하고 어떤 게임 내 작업이 필요한지 판단합니다.
예를 들어, 사용자가 특정 방향으로 왼쪽 아날로그 스틱을 이동한다는 것은 스틱이 이동되는 방향으로 플레이어를 이동시켜야 한다는 것을 의미합니다. 이 **데드존** 반경은 플레이어의 엄지 손가락이 스틱 위에 있을 때 컨트롤러가 이 손가락으로부터 순간 이동을 선택하는 경우를 나타내는 "드리프트"를 제공하는 데 필요합니다. 이 데드존 반경은 "드리프팅"을 막기 위해 반드시 필요합니다. 스틱 위에 놓여 있는 플레이어의 엄지 손가락의 작은 이동까지 컨트롤러가 잡아낼 때 이러한 드리프팅이 발생합니다. 데드존이 없으면 사용자의 움직임에 너무 민감하게 컨트롤이 이루어질 수 있습니다.


섬스틱 입력은 X축과 Y축 모두에서 -1에서 1 사이의 값을 갖습니다. 다음 상수는 섬스틱 데드존의 반경을 지정합니다.

```cpp
#define THUMBSTICK_DEADZONE 0.25f
```

이 변수를 사용하여 실행 가능한 섬스틱 입력을 처리하게 됩니다. 한쪽 축에서 [-1, -.26] 또는 [.26, 1]의 값으로 이동이 이루어집니다.

![섬스틱에 대한 데드존](images/simple-dx-game-controls-deadzone.png)

**UpdatePollingDevices** 메서드의 이 부분은 왼쪽 및 오른쪽 섬스틱을 처리합니다.
각 스틱의 X 값과 Y 값을 확인하여 데드존을 벗어났는지 여부를 알아봅니다. 하나 또는 둘 모두가 있는 경우, 해당되는 구성 요소를 업데이트합니다.
예를 들어 왼쪽 섬스틱이 X축을 따라 왼쪽으로 이동 중인 경우에는 **m_moveCommand** 벡터의 **x** 구성 요소에 -1을 추가합니다. 이 vector는 모든 디바이스에서이 모든 이동을 집계하는 데 사용되고, 추후 플레이어의 이동 범위를 계산하는 데 사용됩니다. 


```cpp
        // Use the left thumbstick to control the eye point position (position of the player)

        // Check if left thumbstick is outside of dead zone on x axis
        if (reading.LeftThumbstickX > THUMBSTICK_DEADZONE ||
            reading.LeftThumbstickX < -THUMBSTICK_DEADZONE)
        {
            // Get value of left thumbstick's position on x axis
            float x = static_cast<float>(reading.LeftThumbstickX);
            // Set the x of the move vector to 1 if the stick is being moved right.
            // Set to -1 if moved left. 
            m_moveCommand.x -= (x > 0) ? 1 : -1;
        }

        // Check if left thumbstick is outside of dead zone on y axis
        if (reading.LeftThumbstickY > THUMBSTICK_DEADZONE ||
            reading.LeftThumbstickY < -THUMBSTICK_DEADZONE)
        {
            // Get value of left thumbstick's position on y axis
            float y = static_cast<float>(reading.LeftThumbstickY);
            // Set the y of the move vector to 1 if the stick is being moved forward.
            // Set to -1 if moved backwards. 
            m_moveCommand.y += (y > 0) ? 1 : -1;
        }

```

왼쪽 스틱이 이동을 컨트롤하는 방법과 유사하게 오른쪽 스틱도 카메라 회전을 컨트롤합니다.


오른쪽 섬스틱 동작은 마우스 및 키보드 제어 설정에 나와 있는 마우스 이동 동작에 맞춰집니다.
스틱이 데드존을 벗어나면 현재 포인터 위치와 사용자가 확인하려는 위치 간의 차이를 계산합니다.
포인터 위치의 이러한 변경 값(**pointerDelta**)은 추후 **Update** 메서드에 적용되도록 카메라 회전의 피치 및 요를 업데이트하는 데 사용됩니다.
**pointerDelta** 벡터는 마우스 및 터치 입력에서 포인터 위치의 변경을 추적하기 위해 [MoveLookController::OnPointerMoved](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L318-L395) 메서드에서도 사용되기 때문에 비슷하게 보일 수 있습니다.


```cpp
        // Use the right thumbstick to control the look at position

        XMFLOAT2 pointerDelta;

        // Check if right thumbstick is outside of deadzone on x axis
        if (reading.RightThumbstickX > THUMBSTICK_DEADZONE ||
            reading.RightThumbstickX < -THUMBSTICK_DEADZONE)
        {
            float x = static_cast<float>(reading.RightThumbstickX);
            // Register the change in the pointer along the x axis
            pointerDelta.x = x * x * x; 
        }
        // No actionable thumbstick movement. Register no change in pointer.
        else
        {
            pointerDelta.x = 0.0f;
        }
        // Check if right thumbstick is outside of deadzone on y axis
        if (reading.RightThumbstickY > THUMBSTICK_DEADZONE ||
            reading.RightThumbstickY < -THUMBSTICK_DEADZONE)
        {
            float y = static_cast<float>(reading.RightThumbstickY);
            // Register the change in the pointer along the y axis
            pointerDelta.y = y * y * y;
        }
        else
        {
            pointerDelta.y = 0.0f;
        }

        XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x *  0.08f;       // Scale for control sensitivity.
        rotationDelta.y = pointerDelta.y *  0.08f;

        // Update our orientation based on the command.
        m_pitch += rotationDelta.y;
        m_yaw += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        m_pitch = __max(-XM_PI / 2.0f, m_pitch);
        m_pitch = __min(+XM_PI / 2.0f, m_pitch);
```

게임의 컨트롤은 구 실행 기능이 없으면 완전할 수 없습니다!


이 **UpdatePollingDevices** 메서드는 오른쪽 트리거의 누름 여부를 확인합니다. 누름인 경우, **m_firePressed** 속성이 true로 전환되면서 구 실행을 시작하도록 게임에 알립니다.
```cpp
    if (reading.RightTrigger > TRIGGER_DEADZONE)
    {
        if (!m_autoFire && !m_gamepadTriggerInUse)
        {
            m_firePressed = true;
        }

        m_gamepadTriggerInUse = true;
    }
    else
    {
        m_gamepadTriggerInUse = false;
    }
```


## <a name="the-update-method"></a>Update 메서드

마무리를 위해 [**Update**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1005-L1096) 메서드에 대해 자세히 알아보겠습니다.
이 메서드는 지원되는 모든 입력에서 플레이어가 수행한 모든 이동 또는 회전을 병합하여 속도 벡터를 생성하고 게임 루프가 액세스할 수 있도록 피치 및 요 값을 업데이트합니다.


**Update** 메서드는 [**UpdatePollingDevices**](#the-updatepollingdevices-method)를 호출하여 컨트롤러 상태를 업데이트함으로써 처리를 시작합니다. 이 메서드는 게임 패드에서 입력을 수집하고 **m_moveCommand** 벡터에 이동 이벤트를 추가합니다. 

**Update** 메서드에서 다음과 같이 입력을 확인합니다.
- 플레이어가 이동 컨트롤러 사각형을 사용하는 경우에는 포인터 위치의 변경 여부를 판단하고, 사용자가 컨트롤러의 데드존 밖으로 포인터를 이동시킨 경우에는 이를 토대로 계산을 합니다. 데드존을 벗어난 경우에는 **m_moveCommand** 벡터 속성이 가상 조이스틱 값으로 업데이트됩니다.
- 이동 키보드 입력 중 하나를 누르면 `1.0f` 또는 `-1.0f`의 값이 **m_moveCommand** 벡터의 해당 구성 요소에 추가됩니다. &mdash;예를 들면 앞으로 이동은 `1.0f`이고 뒤로 이동은 `-1.0f`입니다.


모든 이동 입력을 고려했다면 이제는 **m_moveCommand** 벡터를 실행하여 몇 가지 계산을 통해 게임 환경과 관련해 플레이어의 방향을 보여주는 새 벡터를 생성합니다.
환경과 관련된 이동을 계산하고 이를 해당 방향의 속도로 플레이어에게 적용합니다.
마지막으로 **m_moveCommand** 벡터를 `(0.0f, 0.0f, 0.0f)`로 재설정하여 다음 게임 프레임에 대한 만반의 준비를 합니다.


```cpp
void MoveLookController::Update()
{
    // Get any gamepad input and update state
    UpdatePollingDevices();

    // Get change in 
    if (m_moveInUse)
    {
        XMFLOAT2 pointerDelta;

        pointerDelta.x = m_movePointerPosition.x - m_moveFirstDown.x;
        pointerDelta.y = m_movePointerPosition.y - m_moveFirstDown.y;

        // Figure out the command from the virtual joystick.
        XMFLOAT3 commandDirection = XMFLOAT3(0.0f, 0.0f, 0.0f);
        if (fabsf(pointerDelta.x) > 16.0f)         // Leave 32 pixel-wide dead spot for being still.
            m_moveCommand.x -= pointerDelta.x/fabsf(pointerDelta.x);

        if (fabsf(pointerDelta.y) > 16.0f)
            m_moveCommand.y -= pointerDelta.y/fabsf(pointerDelta.y);
    }

    // Poll our state bits set by the keyboard input events.
    if (m_forward)
    {
        m_moveCommand.y += 1.0f;
    }
    if (m_back)
    {
        m_moveCommand.y -= 1.0f;
    }
    if (m_left)
    {
        m_moveCommand.x += 1.0f;
    }
    if (m_right)
    {
        m_moveCommand.x -= 1.0f;
    }
    if (m_up)
    {
        m_moveCommand.z += 1.0f;
    }
    if (m_down)
    {
        m_moveCommand.z -= 1.0f;
    }

    // Make sure that 45deg cases are not faster.
    if (fabsf(m_moveCommand.x) > 0.1f ||
        fabsf(m_moveCommand.y) > 0.1f ||
        fabsf(m_moveCommand.z) > 0.1f)
    {
        XMStoreFloat3(&m_moveCommand, XMVector3Normalize(XMLoadFloat3(&m_moveCommand)));
    }

    // Rotate command to align with our direction (world coordinates).
    XMFLOAT3 wCommand;
    wCommand.x =  m_moveCommand.x * cosf(m_yaw) - m_moveCommand.y * sinf(m_yaw);
    wCommand.y =  m_moveCommand.x * sinf(m_yaw) + m_moveCommand.y * cosf(m_yaw);
    wCommand.z =  m_moveCommand.z;

    // Scale for sensitivity adjustment.
    // Our velocity is based on the command. Y is up.
    m_velocity.x = -wCommand.x * MoveLookConstants::MovementGain;
    m_velocity.z =  wCommand.y * MoveLookConstants::MovementGain;
    m_velocity.y =  wCommand.z * MoveLookConstants::MovementGain;

    // Clear movement input accumulator for use during next frame.
    m_moveCommand = XMFLOAT3(0.0f, 0.0f, 0.0f);
}
```


## <a name="next-steps"></a>다음 단계

컨트롤이 추가되었다면 이제 몰입감 높은 게임을 위해 추가해야 할 또 다른 기능이 있습니다. 사운드가 바로 그것입니다!
음악과 사운드 효과는 모든 게임에 있어 중요합니다. [사운드 추가](tutorial--adding-sound.md)에 대해서는 다음에 살펴보겠습니다.
 

 

 




