---
title: 컨트롤 추가
description: 이제 샘플 게임에서 3 차원 게임의 이동 모양 컨트롤을 구현 하는 방법과 기본적인 터치, 마우스 및 게임 컨트롤러 컨트롤을 개발 하는 방법을 살펴보겠습니다.
ms.assetid: f9666abb-151a-74b4-ae0b-ef88f1f252f8
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 컨트롤, 입력
ms.localizationpriority: medium
ms.openlocfilehash: 87a56c9213aabce23801f305d8a100f536f0889b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175237"
---
# <a name="add-controls"></a>컨트롤 추가

> [!NOTE]
> 이 항목은 DirectX 자습서 시리즈 [를 사용 하 여 단순 유니버설 Windows 플랫폼 (UWP) 게임 만들기](tutorial--create-your-first-uwp-directx-game.md) 의 일부입니다. 해당 링크의 항목은 계열의 컨텍스트를 설정 합니다.

\[ Windows 10의 UWP 앱 용으로 업데이트 되었습니다. Windows 8.x 문서는 [보관 파일](/previous-versions/windows/apps/mt244353(v=win.10)?redirectedfrom=MSDN) 을 참조 하세요. \]

UWP (좋은 유니버설 Windows 플랫폼) 게임은 다양 한 인터페이스를 지원 합니다. 잠재적 플레이어는 물리적 단추가 없는 태블릿에 Windows 10을 포함 하거나, Xbox 컨트롤러가 연결 된 PC 또는 고성능 마우스 및 게임 키보드를 사용 하는 최신 데스크톱 게임 rig를 사용할 수 있습니다. 게임에서 컨트롤은 [**MoveLookController**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp) 클래스에서 구현 됩니다. 이 클래스는 세 가지 유형의 입력 (마우스 및 키보드, 터치 및 게임 패드)을 모두 단일 컨트롤러에 집계 합니다. 최종 결과는 여러 장치에서 작동 하는 장르 표준 이동 모양 컨트롤을 사용 하는 첫 번째 사람의 슈팅입니다.

> [!NOTE]
> 컨트롤에 대 한 자세한 내용은 게임의 게임 및 [터치 컨트롤용](tutorial--adding-touch-controls-to-your-directx-game.md)컨트롤 [이동](tutorial--adding-move-look-controls-to-your-directx-game.md) 을 참조 하세요.


## <a name="objective"></a>Objective

이 시점에서 렌더링 하는 게임을 하지만 플레이어를 이동 하거나 대상을 이동할 수 없습니다. 다음은 UWP DirectX 게임에서 다음과 같은 유형의 입력에 대 한 슈팅 컨트롤을 처음 사용 하는 게임의 구현 방식을 살펴보겠습니다.
- 마우스 및 키보드
- 터치
- 게임 패드

>[!Note]
>이 샘플에 대 한 최신 게임 코드를 다운로드 하지 않은 경우 [Direct3D sample game](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)로 이동 합니다. 이 샘플은 여러 UWP 기능 샘플 컬렉션의 일부입니다. 샘플을 다운로드 하는 방법에 대 한 지침은 [GitHub에서 UWP 샘플 가져오기](../get-started/get-app-samples.md)를 참조 하세요.

## <a name="common-control-behaviors"></a>일반적인 제어 동작


터치 컨트롤 및 마우스/키보드 컨트롤의 핵심 구현은 매우 유사 합니다. UWP 앱에서 포인터는 단순히 화면에 있는 지점입니다. 터치 스크린에서 마우스를 움직이거나 손가락을 이동 하 여 이동할 수 있습니다. 결과적으로 단일 이벤트 집합에 등록 하 고 플레이어에서 마우스를 사용 하는지 또는 터치 스크린을 사용 하 여 포인터를 이동할지 걱정 하지 않을 수 있습니다.

샘플 게임의 **MoveLookController** 클래스는 초기화 되 면 다음과 같은 4 개의 포인터 관련 이벤트와 한 개의 마우스 특정 이벤트를 등록 합니다.

이벤트 | Description
:------ | :-------
[**CoreWindow::P ointerPressed 있습니다.**](/uwp/api/windows.ui.core.corewindow.pointerpressed) | 왼쪽 또는 오른쪽 마우스 단추를 눌 었는 경우 또는 터치 서피스가 수행 되었습니다.
[**CoreWindow::P ointerMoved 됨**](/uwp/api/windows.ui.core.corewindow.pointermoved) |터치 화면에서 마우스를 이동 했거나 끌기 작업을 수행 했습니다.
[**CoreWindow::P ointerReleased**](/uwp/api/windows.ui.core.corewindow.pointerreleased) |마우스 왼쪽 단추를 놓았음을 터치 표면에 연결 하는 개체가 리프트 되었습니다.
[**CoreWindow::P ointerExited**](/uwp/api/windows.ui.core.corewindow.pointerexited) |포인터가 주 창 밖으로 이동 했습니다.
[**Windows::D evices:: Input:: MouseMoved**](/uwp/api/windows.devices.input.mousedevice.mousemoved) | 마우스가 특정 거리 만큼 이동 했습니다. 현재 X-y 위치가 아니라 마우스 이동 델타 값에만 관심이 있습니다.


이러한 이벤트 처리기는 응용 프로그램 창에서 **MoveLookController** 가 초기화 되는 즉시 사용자 입력에 대 한 수신 대기를 시작 하도록 설정 됩니다.
```cppwinrt
void MoveLookController::InitWindow(_In_ CoreWindow const& window)
{
    ResetState();

    window.PointerPressed({ this, &MoveLookController::OnPointerPressed });

    window.PointerMoved({ this, &MoveLookController::OnPointerMoved });

    window.PointerReleased({ this, &MoveLookController::OnPointerReleased });

    window.PointerExited({ this, &MoveLookController::OnPointerExited });

    ...

    // There is a separate handler for mouse-only relative mouse movement events.
    MouseDevice::GetForCurrentView().MouseMoved({ this, &MoveLookController::OnMouseMoved });

    ...
}
```

[**Initwindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L68-L92) 에 대 한 전체 코드는 GitHub에서 볼 수 있습니다.


게임에서 특정 입력을 수신 해야 하는 시기를 확인 하기 위해 **MoveLookController** 클래스에는 컨트롤러 유형에 관계 없이 세 가지 컨트롤러 관련 상태가 있습니다.

시스템 상태 | Description
:----- | :-------
**없음** | 컨트롤러에 대해 초기화 된 상태입니다. 게임에서 컨트롤러 입력을 예측 하지 않으므로 모든 입력은 무시 됩니다.
**WaitForInput** | 컨트롤러는 게임 화면에서 마우스 왼쪽 단추 클릭, 터치 이벤트를 사용 하 여 플레이어가 게임의 메시지를 승인할 때까지 기다리고 있습니다.
**활성** | 컨트롤러가 활성 게임 재생 모드입니다.



### <a name="waitforinput-state-and-pausing-the-game"></a>WaitForInput 상태 및 게임 일시 중지

게임을 일시 중지 하면 **Waitforinput** 상태가 됩니다. 플레이어에서 포인터를 게임의 주 창 외부로 이동 하거나 일시 중지 단추 (P 키 또는 게임 패드 **시작** 단추)를 누를 때 발생 합니다. **MoveLookController** 은 press를 등록 하 고 [**IsPauseRequested**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L107-L127) 메서드를 호출할 때 게임 루프에 알립니다. **IsPauseRequested** 가 **true**를 반환 하는 경우 Game 루프가 **MoveLookController** 에서 **Waitforpress** 를 호출 하 여 컨트롤러를 **waitforpress** 상태로 전환 합니다. 


**Waitforinput** 상태에서 게임은 **활성** 상태로 반환 될 때까지 거의 모든 게임 플레이 입력 이벤트의 처리를 중지 합니다. 예외는 일시 중지 단추 이며이로 인해 게임이 활성 상태로 돌아갑니다. 게임을 **활성** 상태로 되돌리기 위해 플레이어에서 메뉴 항목을 선택 해야 하는 일시 중지 단추를 제외 합니다. 



### <a name="the-active-state"></a>활성 상태입니다.

**활성** 상태에서 **MoveLookController** 인스턴스는 사용 하도록 설정 된 모든 입력 장치의 이벤트를 처리 하 고 플레이어의 작업을 해석 합니다. 그 결과, 플레이어 보기의 속도와 모양을 업데이트 하 고 게임 루프에서 **업데이트** 를 호출한 후 업데이트 된 데이터를 게임과 공유 합니다.


모든 포인터 입력은 다른 포인터 작업에 해당 하는 다른 포인터 Id를 사용 하 여 **활성** 상태에서 추적 됩니다.
[**Pointerpressed**](/uwp/api/windows.ui.core.corewindow.pointerpressed) 이벤트를 수신 하면 **MoveLookController** 는 창에서 만든 포인터 ID 값을 가져옵니다. 포인터 ID는 특정 형식의 입력을 나타냅니다. 예를 들어 다중 터치 장치에서는 여러 다른 활성 입력이 동시에 있을 수 있습니다. Id는 플레이어에서 사용 중인 입력을 추적 하는 데 사용 됩니다. 터치 화면의 이동 사각형에 하나의 이벤트가 있는 경우 포인터 ID가 할당 되어 이동 사각형의 포인터 이벤트를 추적 합니다. 화재 사각형의 다른 포인터 이벤트는 별도의 포인터 ID를 사용 하 여 개별적으로 추적 됩니다.


> [!NOTE]
> 게임 패드의 마우스 및 오른쪽 엄지 스틱 입력에는 별도로 처리 되는 Id도 있습니다.

포인터 이벤트를 특정 게임 작업에 매핑한 후에는 **MoveLookController** 개체가 주 게임 루프와 공유 하는 데이터를 업데이트 해야 합니다.

샘플 게임의 [**Update**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1005-L1096) 메서드는 호출 되 면 입력을 처리 하 고 속도 및 모양 변수 (**m \_ 속도** 및 **m \_ lookdirection**)를 업데이트 합니다. 그러면 게임 루프가 public [**속도**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L906-L909) 및 [**lookdirection**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L913-L923) 메서드를 호출 하 여 검색 합니다.

> [!NOTE]
> [**업데이트**](#the-update-method) 방법에 대 한 자세한 내용은이 페이지의 뒷부분에서 볼 수 있습니다.




게임 루프는 **MoveLookController** 인스턴스에서 **isfiring** 메서드를 호출 하 여 플레이어가 발생 하는지 테스트할 수 있습니다. **MoveLookController** 는 플레이어가 세 입력 유형 중 하나에서 발사 단추를 눌렀는지 확인 합니다.

```cppwinrt
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

이제 세 가지 컨트롤 형식의 각 구현에 대해 좀 더 자세히 살펴보겠습니다.

## <a name="adding-relative-mouse-controls"></a>상대 마우스 컨트롤 추가


마우스 이동이 감지 되 면 해당 이동을 사용 하 여 카메라의 새 피치와 요 결정 합니다. 이 작업은 동작의 절대 x-y 픽셀 좌표를 기록 하는 것과는 반대로 마우스가 이동한 거리 (이동 시작과 정지 사이의 델타)를 처리 하는 상대 마우스 컨트롤을 구현 하 여 수행 합니다.

이렇게 하려면 [**mousedelta**](/uwp/api/windows.devices.input.mousedevice.mousemoved) 이벤트에서 반환 된 [**Windows::D Evice:: Input:: MouseEventArgs:: mousedelta**](/uwp/api/windows.devices.input.mouseeventargs.mousedelta) 인수 개체에서 [**mousedelta:: x**](/uwp/api/Windows.Devices.Input.MouseDelta) 및 **mousedelta:: y** 필드를 검사 하 여 X (가로 동작) 및 y (세로 동작) 좌표에 대 한 변경 내용을 가져옵니다.

```cppwinrt
void MoveLookController::OnMouseMoved(
    _In_ MouseDevice const& /* mouseDevice */,
    _In_ MouseEventArgs const& args
    )
{
    // Handle Mouse Input via dedicated relative movement handler.

    switch (m_state)
    {
    case MoveLookControllerState::Active:
        XMFLOAT2 mouseDelta;
        mouseDelta.x = static_cast<float>(args.MouseDelta().X);
        mouseDelta.y = static_cast<float>(args.MouseDelta().Y);

        XMFLOAT2 rotationDelta;
        // Scale for control sensitivity.
        rotationDelta.x = mouseDelta.x * MoveLookConstants::RotationGain;
        rotationDelta.y = mouseDelta.y * MoveLookConstants::RotationGain;

        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;
        m_yaw += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        float limit = XM_PI / 2.0f - 0.01f;
        m_pitch = __max(-limit, m_pitch);
        m_pitch = __min(+limit, m_pitch);

        // Keep longitude in sane range by wrapping.
        if (m_yaw > XM_PI)
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

터치 컨트롤은 태블릿 사용자를 지 원하는 데 유용 합니다. 이 게임은 특정 게임 내 작업에 맞추어 화면에서 특정 영역의 영역을 지정 하 여 터치식 입력을 수집 합니다.
이 게임의 터치식 입력은 3 개의 영역을 사용 합니다.

![터치 모양 이동 레이아웃](images/simple-dx-game-controls-touchzones.png)

다음 명령은 터치식 제어 동작을 요약 합니다.
사용자 입력 | 작업
:------- | :--------
사각형 이동 | 터치 입력은 세로 동작이 앞으로/뒤로 이동 하 고 가로 동작이 왼쪽/오른쪽 위치 동작으로 변환 되는 가상 조이스틱으로 변환 됩니다.
사각형 발생 | 구를 발사 합니다.
이동 및 발생 사각형 외부에서 터치 | 카메라 보기의 회전 (피치 및 요)을 변경 합니다.

**MoveLookController** 는 포인터 ID를 확인 하 여 이벤트가 발생 한 위치를 확인 하 고 다음 작업 중 하나를 수행 합니다.

-   [**Pointermoved**](/uwp/api/windows.ui.core.corewindow.pointermoved) 이벤트가 이동 또는 발생 영역에서 발생 한 경우 컨트롤러의 포인터 위치를 업데이트 합니다.
-   화면 (모양 컨트롤로 정의 됨)의 나머지 부분에서 [**Pointermoved**](/uwp/api/windows.ui.core.corewindow.pointermoved) 이벤트가 발생 한 경우에는 모양 벡터의 피치 및 요 변화를 계산 합니다.


터치 컨트롤을 구현한 후에는 Direct2D를 사용 하 여 앞에서 그린 사각형은 이동, 화재 및 보이는 영역이 있는 플레이어를 표시 합니다.


![터치 컨트롤](images/simple-dx-game-controls-showzones.png)


이제 각 컨트롤을 구현 하는 방법을 살펴보겠습니다.


### <a name="move-and-fire-controller"></a>컨트롤러 이동 및 실행
화면 왼쪽 아래에 있는 컨트롤러 이동 사각형은 방향 패드로 사용 됩니다. 이 공간 내에서 왼쪽 및 오른쪽으로 이동 하 여 플레이어를 왼쪽 및 오른쪽으로 이동 하 고 위쪽 및 아래쪽으로 이동 하 여 카메라를 앞뒤로 이동 합니다.
이렇게 설정 하면 화면의 오른쪽 아래 사분면에서 화재 컨트롤러를 누르면 구가 발생 합니다.

[**SetMoveRect**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L843-L853) 및 [**SetFireRect**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L857-L867) 메서드는 두 개의 2d 벡터를 가져와 화면에서 각 사각형의 왼쪽 위와 오른쪽 아래 모퉁이 위치를 지정 하는 입력 사각형을 만듭니다. 


그러면 사용자가 사각형의 내에서 접촉 하 고 있는지 여부를 확인 하는 데 도움이 되는 매개 변수가 **m \_ fireUpperLeft** 및 **m \_ fireLowerRight** 에 할당 됩니다. 
```cppwinrt
m_fireUpperLeft = upperLeft;
m_fireLowerRight = lowerRight;
```

화면 크기를 조정 하면 이러한 사각형이 approperiate 크기로 다시 그려집니다.

이제 컨트롤을 배열로 영역 설정 사용자가 실제로이를 사용 하는 시기를 확인할 수 있습니다.
이렇게 하려면 사용자가 포인터를 눌렀다가 이동 하거나 해제할 때 **MoveLookController:: InitWindow** 메서드에 일부 이벤트 처리기를 설정 합니다.

```cppwinrt
window.PointerPressed({ this, &MoveLookController::OnPointerPressed });

window.PointerMoved({ this, &MoveLookController::OnPointerMoved });

window.PointerReleased({ this, &MoveLookController::OnPointerReleased });
```

먼저 사용자가 [**Onpointerpressed**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L179-L313) 메서드를 사용 하 여 이동 또는 발생 사각형 내에서 처음 누를 때 발생 하는 상황을 확인 합니다.
여기서는 컨트롤이 접촉 하는 위치와 포인터가 해당 컨트롤러에 이미 있는지 확인 합니다. 특정 컨트롤을 터치 하는 첫 번째 손가락이 면 다음을 수행 합니다.
- System.windows.uielement.touchdown>의 위치를 **m \_ movefirstdown** 또는 **m \_ fireFirstDown** 에 2d 벡터로 저장 합니다.
- 포인터 ID를 **m \_ movepointerid** 또는 **m \_ firePointerID**에 할당 합니다.
- 이제 해당 컨트롤에 대 한 활성 포인터가 있으므로 적절 한 **여유 주소** 플래그 (**m \_ moveInUse** 또는 **m \_ fireInUse**)를로 설정 합니다 `true` .

```cppwinrt
PointerPoint point = args.CurrentPoint();
uint32_t pointerID = point.PointerId();
Point pointerPosition = point.Position();
PointerPointProperties pointProperties = point.Properties();
auto pointerDevice = point.PointerDevice();
auto pointerDeviceType = pointerDevice.PointerDeviceType();

XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);

...
case MoveLookControllerState::Active:
    switch (pointerDeviceType)
    {
    case winrt::Windows::Devices::Input::PointerDeviceType::Touch:
        // Check to see if this pointer is in the move control.
        if (position.x > m_moveUpperLeft.x &&
            position.x < m_moveLowerRight.x &&
            position.y > m_moveUpperLeft.y &&
            position.y < m_moveLowerRight.y)
        {
            // If no pointer is in this control yet.
            if (!m_moveInUse)
            {
                // Process a DPad touch down event.
                // Save the location of the initial contact
                m_moveFirstDown = position;
                // Store the pointer using this control
                m_movePointerID = pointerID;
                // Set InUse flag to signal there is an active move pointer
                m_moveInUse = true;
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
                // Save the location of the initial contact
                m_fireLastPoint = position;
                // Store the pointer using this control
                m_firePointerID = pointerID;
                // Set InUse flag to signal there is an active fire pointer
                m_fireInUse = true;
                ...
            }
        }
        ...
```

사용자가 이동 또는 화재 컨트롤을 터치 하 고 있는지 여부를 확인 했으므로 플레이어에서 눌린 손가락으로 이동 하 고 있는지 확인 합니다.
[**MoveLookController:: OnPointerMoved**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L317-L395) 메서드를 사용 하 여 이동 된 포인터를 확인 한 다음 새 위치를 2d 벡터로 저장 합니다.  

```cppwinrt
PointerPoint point = args.CurrentPoint();
uint32_t pointerID = point.PointerId();
Point pointerPosition = point.Position();
PointerPointProperties pointProperties = point.Properties();
auto pointerDevice = point.PointerDevice();

// convert to allow math
XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);

switch (m_state)
{
case MoveLookControllerState::Active:
    // Decide which control this pointer is operating.

    // Move control
    if (pointerID == m_movePointerID)
    {
        // Save the current position.
        m_movePointerPosition = position;
    }
    // Look control
    else if (pointerID == m_lookPointerID)
    {
        ...
    }
    // Fire control
    else if (pointerID == m_firePointerID)
    {
        m_fireLastPoint = position;
    }
    ...
```

사용자가 컨트롤 내에서 제스처를 만든 후에는 포인터를 해제 합니다. [**MoveLookController:: OnPointerReleased**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) 메서드를 사용 하 여 어떤 포인터가 해제 되었으며 일련의 재설정을 수행 하는지 확인 합니다.


이동 컨트롤이 릴리스되면 다음 작업을 수행 합니다.
- 플레이어의 속도를 모든 방향으로 설정 하 여 `0` 게임에서 이동 하지 않도록 합니다.
- 사용자 **가 \_ ** `false` 더 이상 이동 컨트롤러에 접촉 하지 않으므로 m moveInUse을로 전환 합니다.
- 이동 `0` 컨트롤러에 포인터가 더 이상 없으므로 이동 포인터 ID를로 설정 합니다.

```cppwinrt
if (pointerID == m_movePointerID)
{
    // Stop on release.
    m_velocity = XMFLOAT3(0, 0, 0);
    m_moveInUse = false;
    m_movePointerID = 0;
}
```

Fire 컨트롤의 경우 모두 해제 된 경우에는 **m_fireInUse** 플래그를로 전환 하 `false` 고, 화재 포인터 ID를로 전환 합니다 .이는 `0` 더 이상 화재 컨트롤에 포인터가 없기 때문입니다.
```cppwinrt
else if (pointerID == m_firePointerID)
{
    m_fireInUse = false;
    m_firePointerID = 0;
}
```

### <a name="look-controller"></a>컨트롤러 보기
화면에서 사용 하지 않는 영역에 대 한 터치 장치 포인터 이벤트를 모양 컨트롤러로 처리 합니다. 이 영역 주위에서 손가락을 이동 하면 플레이어 카메라의 피치 및 요 (회전)이 변경 됩니다.

이 지역의 터치 장치에서 **MoveLookController:: OnPointerPressed** 이벤트가 발생 하 고 게임 상태가 **활성**으로 설정 된 경우 포인터 ID가 할당 됩니다.

```cppwinrt
// If no pointer is in this control yet.
if (!m_lookInUse)
{
    // Save point for later move.
    m_lookLastPoint = position;
    // Store the pointer using this control.
    m_lookPointerID = pointerID;
    // These are for smoothing.
    m_lookLastDelta.x = m_lookLastDelta.y = 0;
    m_lookInUse = true;
}
```

여기서 **MoveLookController** 는 이벤트를 발생 시킨 포인터에 대 한 포인터 ID를 찾는 영역에 해당 하는 특정 변수에 할당 합니다. 모양 영역에서 터치가 발생 하는 경우 **m \_ lookPointerID** 변수는 이벤트를 발생 시킨 포인터 ID로 설정 됩니다. 또한 부울 변수인 **m \_ lookInUse**는 컨트롤이 아직 해제 되지 않았음을 나타내기 위해 설정 됩니다.

이제 샘플 게임에서 [**Pointermoved**](/uwp/api/windows.ui.core.corewindow.pointermoved) screen 이벤트를 처리 하는 방법을 살펴보겠습니다.

**MoveLookController:: OnPointerMoved** 메서드 내에서 이벤트에 할당 된 포인터 ID의 종류를 확인 합니다. **M_lookPointerID**경우 포인터의 위치에 대 한 변경 내용을 계산 합니다.
그런 다음이 델타를 사용 하 여 회전의 변경 정도를 계산 합니다. 마지막으로 플레이어 회전을 변경 하는 데 사용 되는 **m \_ 피치** 및 **m \_ 요** 를 업데이트할 수 있는 지점입니다. 

```cppwinrt
// This is the look pointer.
else if (pointerID == m_lookPointerID)
{
    // Look control.
    XMFLOAT2 pointerDelta;
    // How far did the pointer move?
    pointerDelta.x = position.x - m_lookLastPoint.x;
    pointerDelta.y = position.y - m_lookLastPoint.y;

    XMFLOAT2 rotationDelta;
    // Scale for control sensitivity.
    rotationDelta.x = pointerDelta.x * MoveLookConstants::RotationGain;
    rotationDelta.y = pointerDelta.y * MoveLookConstants::RotationGain;
    // Save for next time through.
    m_lookLastPoint = position;

    // Update our orientation based on the command.
    m_pitch -= rotationDelta.y;
    m_yaw += rotationDelta.x;

    // Limit pitch to straight up or straight down.
    float limit = XM_PI / 2.0f - 0.01f;
    m_pitch = __max(-limit, m_pitch);
    m_pitch = __min(+limit, m_pitch);
    ...
}
```

마지막으로 샘플 게임에서 [**Pointerreleased**](/uwp/api/windows.ui.core.corewindow.pointerreleased) 터치 스크린 이벤트를 처리 하는 방법을 살펴보겠습니다.
사용자가 터치 제스처를 완료 하 고 화면에서 손가락을 제거 하면 [**MoveLookController:: OnPointerReleased**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) 가 시작 됩니다.
[**Pointerreleased**](/uwp/api/windows.ui.core.corewindow.pointerreleased) 이벤트를 발생 시킨 포인터의 id가 이전에 기록 된 이동 포인터의 id 인 경우, **MoveLookController** 는 `0` 플레이어가 모양 영역에 대 한 접촉을 중지 했기 때문에 속도를로 설정 합니다.

```cppwinrt
else if (pointerID == m_lookPointerID)
{
    m_lookInUse = false;
    m_lookPointerID = 0;
}
```

## <a name="adding-mouse-and-keyboard-support"></a>마우스 및 키보드 지원 추가

이 게임에는 키보드 및 마우스에 대 한 다음과 같은 컨트롤 레이아웃이 있습니다.

사용자 입력 | 작업
:------- | :--------
W | 플레이어를 앞으로 이동
A | 플레이어를 왼쪽으로 이동
S | 플레이어 뒤로 이동
D | 플레이어 오른쪽으로 이동
X | 뷰 위로 이동
스페이스바 | 뷰 아래로 이동
P | 게임 일시 중지
마우스 이동 | 카메라 보기의 회전 (피치 및 요) 변경
마우스 왼쪽 단추 | 구 발사


키보드를 사용 하기 위해 샘플 게임은 [**MoveLookController:: InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L84-L88) 메서드 내에 두 개의 새 이벤트 [**CoreWindow:: KeyUp**](/uwp/api/windows.ui.core.corewindow.keyup) 및 [**CoreWindow:: KeyDown**](/uwp/api/windows.ui.core.corewindow.keydown)을 등록 합니다. 이러한 이벤트는 키의 눌렀다가 릴리스를 처리 합니다.

```cppwinrt
window.KeyDown({ this, &MoveLookController::OnKeyDown });

window.KeyUp({ this, &MoveLookController::OnKeyUp });
```

마우스는 포인터를 사용 하는 경우에도 터치 컨트롤과 약간 다르게 처리 됩니다. 컨트롤 레이아웃을 맞추기 위해 **MoveLookController** 는 마우스를 이동할 때마다 카메라를 회전 하 고 마우스 왼쪽 단추를 누르면 발생 합니다.

이는 **MoveLookController**의 [**Onpointerpressed**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L179-L313) 메서드에서 처리 됩니다.

이 메서드에서는 열거와 함께 사용 되는 포인터 장치 유형을 확인 [`Windows::Devices::Input::PointerDeviceType`](/uwp/api/Windows.Devices.Input.PointerDeviceType) 합니다. 게임이 **활성** 상태이 고 **PointerDeviceType** 가 **닿지**않으면 마우스 입력 이라고 가정 합니다.

```cppwinrt
case MoveLookControllerState::Active:
    switch (pointerDeviceType)
    {
    case winrt::Windows::Devices::Input::PointerDeviceType::Touch:
        // Behavior for touch controls
        ...

    default:
        // Behavior for mouse controls
        bool rightButton = pointProperties.IsRightButtonPressed();
        bool leftButton = pointProperties.IsLeftButtonPressed();

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
            // These are for smoothing.
            m_lookLastDelta.x = m_lookLastDelta.y = 0;
        }
        break;
    }
    break;
```

플레이어에서 마우스 단추 중 하나를 누르면 [CoreWindow::P ointerreleased](/uwp/api/Windows.UI.Core.CoreWindow.PointerReleased) 마우스 이벤트가 발생 하 고 [MoveLookController:: onpointerreleased](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) 메서드를 호출 하 고 입력이 완료 됩니다. 이 시점에서 왼쪽 마우스 단추를 누른 상태에서 현재 릴리스된 경우에는 구 발생이 중지 됩니다. 찾기는 항상 사용 하도록 설정 되어 있으므로 게임은 계속 해 서 동일한 마우스 포인터를 사용 하 여 진행 중인 모양 이벤트를 추적 합니다.

```cppwinrt
case MoveLookControllerState::Active:
    // Touch points
    if (pointerID == m_movePointerID)
    {
        // Stop movement
        ...
    }
    else if (pointerID == m_lookPointerID)
    {
        // Stop look rotation
        ...
    }
    // Fire button has been released
    else if (pointerID == m_firePointerID)
    {
        // Stop firing
        ...
    }
    // Mouse point
    else if (pointerID == m_mousePointerID)
    {
        bool rightButton = pointProperties.IsRightButtonPressed();
        bool leftButton = pointProperties.IsLeftButtonPressed();

        // Mouse no longer in use so stop firing
        m_mouseInUse = false;

        // Don't clear the mouse pointer ID so that Move events still result in Look changes.
        // m_mousePointerID = 0;
        m_mouseLeftInUse = leftButton;
        m_mouseRightInUse = rightButton;
    }
    break;
```

이제 지원 되는 마지막 컨트롤 형식 (gamepads)을 살펴보겠습니다. Gamepads는 포인터 개체를 사용 하지 않으므로 터치 및 마우스 컨트롤에서 별도로 처리 됩니다. 이로 인해 몇 가지 새로운 이벤트 처리기 및 메서드를 추가 해야 합니다.

## <a name="adding-gamepad-support"></a>게임 패드 지원 추가

이 게임에서 게임에 대 한 지원은 [Windows. 게이밍](/uwp/api/windows.gaming.input) api를 호출 하 여 추가 됩니다. 이 Api 집합은 레이스 휠 및 비행 스틱 같은 게임 컨트롤러 입력에 대 한 액세스를 제공 합니다. 

다음은 게임 패드 컨트롤입니다.

사용자 입력 | 작업
:------- | :--------
왼쪽 아날로그 스틱 | 플레이어 이동
오른쪽 아날로그 스틱 | 카메라 보기의 회전 (피치 및 요) 변경
오른쪽 트리거 | 구 발사
시작/메뉴 단추 | 게임 일시 중지 또는 다시 시작

[**Initwindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L68-L103) 메서드에서는 새 이벤트 두 개를 추가 하 여 게임 창이 [추가](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1100-L1105) 또는 [제거](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1109-L1114)되었는지 확인 합니다. 이러한 이벤트는 **m_gamepadsChanged** 속성을 업데이트 합니다. **UpdatePollingDevices** 메서드에서는 알려진 gamepads 목록이 변경 되었는지 여부를 확인 하는 데 사용 됩니다. 

```cppwinrt
// Detect gamepad connection and disconnection events.
Gamepad::GamepadAdded({ this, &MoveLookController::OnGamepadAdded });

Gamepad::GamepadRemoved({ this, &MoveLookController::OnGamepadRemoved });
```

> [!NOTE]
> 앱이 포커스를 두지 않는 동안 UWP 앱은 Xbox One 컨트롤러에서 입력을 받을 수 없습니다.

### <a name="the-updatepollingdevices-method"></a>UpdatePollingDevices 메서드

**MoveLookController** 인스턴스의 [**UpdatePollingDevices**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L654-L782) 메서드는 즉시 게임 패드를 연결 했는지 확인 합니다. [**GetCurrentReading**](/uwp/api/windows.gaming.input.gamepad.GetCurrentReading)으로 상태를 읽기 시작 합니다. 그러면 [**GamepadReading**](/uwp/api/Windows.Gaming.Input.GamepadReading) 구조체가 반환 되어 클릭 하거나 thumbsticks 이동 된 단추를 확인할 수 있습니다.

게임의 상태가 **Waitforinput**인 경우 게임을 다시 시작할 수 있도록 컨트롤러의 시작/메뉴 단추만 수신 대기 합니다.

**활성 상태인**경우 사용자의 입력을 확인 하 고 수행 해야 하는 작업을 결정 합니다.
예를 들어 사용자가 왼쪽 아날로그 스틱을 특정 방향으로 이동한 경우이를 통해 플레이어가 이동 하는 방향으로 플레이어를 이동 해야 한다는 것을 알 수 있습니다. 특정 방향으로 스틱을 이동 하는 것은 **데드 영역의**반경 보다 크게 등록 해야 합니다. 그렇지 않으면 아무 작업도 수행 되지 않습니다. "유동"을 방지 하기 위해이 비활성 영역 반지름이 필요 합니다 .이는 컨트롤러가 스틱 위에 있는 플레이어의 엄지 단추에서 작은 움직임을 선택 하는 경우입니다. 비활성 영역이 없으면 컨트롤이 너무 중요 한 사용자에 게 표시 될 수 있습니다.

엄지 스틱 입력은 x 축과 y 축 모두에 대해-1에서 1 사이입니다. 다음 consant는 엄지 데드 영역에 대 한 반지름을 지정 합니다.

```cppwinrt
#define THUMBSTICK_DEADZONE 0.25f
```

그런 다음이 변수를 사용 하 여 실행 가능한 엄지 스틱 입력 처리를 시작 합니다. 이동은 두 축에 [-1,-.26] 또는 [. 26, 1]의 값으로 발생 합니다.

![thumbsticks에 대 한 데드 영역](images/simple-dx-game-controls-deadzone.png)

이 **UpdatePollingDevices** 메서드는 왼쪽과 오른쪽 thumbsticks를 처리 합니다.
각 스틱 X 및 Y 값은 데드 영역 외부에 있는지 확인 합니다. 하나 또는 둘 다 인 경우 해당 구성 요소를 업데이트 합니다.
예를 들어 왼쪽 엄지 스틱을 X 축을 따라 왼쪽으로 이동 하는 경우 **m_moveCommand** 벡터의 **x** 구성 요소에-1을 추가 합니다. 이 벡터는 모든 장치에서 모든 움직임을 집계 하는 데 사용 되며 나중에 플레이어의 이동 위치를 계산 하는 데 사용 됩니다. 

```cppwinrt
// Use the left thumbstick to control the eye point position
// (position of the player).

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

왼쪽 스틱에서 움직임을 제어 하는 방법과 마찬가지로 오른쪽 스틱을 카메라 회전을 제어 합니다.

오른쪽 엄지 단추는 마우스 및 키보드 컨트롤 설정에서 마우스 움직임의 동작에 맞춰집니다.
스틱가 데드 영역 외부에 있는 경우 현재 포인터 위치와 사용자가 현재 확인 하려고 하는 위치 간의 차이를 계산 합니다.
그런 다음 포인터 위치 (**Pointerdelta**)의이 변경 내용을 사용 하 여 나중에 **업데이트** 메서드에서 적용 되는 카메라 회전의 피치와 요을 업데이트 합니다.
**Pointerdelta** 벡터는 마우스 및 터치 입력에 대 한 포인터 위치의 변경 내용을 추적 하기 위해 [MoveLookController:: OnPointerMoved](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L318-L395) 방법에도 사용 되므로 익숙할 수 있습니다.

```cppwinrt
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
// Scale for control sensitivity.
rotationDelta.x = pointerDelta.x * 0.08f;
rotationDelta.y = pointerDelta.y * 0.08f;

// Update our orientation based on the command.
m_pitch += rotationDelta.y;
m_yaw += rotationDelta.x;

// Limit pitch to straight up or straight down.
m_pitch = __max(-XM_PI / 2.0f, m_pitch);
m_pitch = __min(+XM_PI / 2.0f, m_pitch);
```

게임의 컨트롤은 구를 발생 시 키 지 않고 완료 되지 않습니다.

이 **UpdatePollingDevices** 메서드는 오른쪽 트리거가 눌러져 있는지도 확인 합니다. 이 경우에는 **m_firePressed** 속성이 true로 전환 되 고,이를 통해 구를 시작 해야 하는 게임에 신호를 보내는 것입니다.
```cppwinrt
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

항목을 래핑하려면 [**업데이트**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1005-L1096) 메서드를 자세히 살펴보겠습니다.
이 메서드는 지원 되는 입력을 사용 하 여 플레이어가 만든 움직임 또는 회전을 병합 하 여 속도 벡터를 생성 하 고 게임 루프에서 액세스 하기 위한 피치 및 요 값을 업데이트 합니다.

**Update** 메서드는 [**UpdatePollingDevices**](#the-updatepollingdevices-method) 를 호출 하 여 컨트롤러의 상태를 업데이트 하는 작업을 수행 합니다. 또한이 메서드는 게임 패드에서 입력을 수집 하 고 **m_moveCommand** 벡터에 움직임을 추가 합니다. 

**업데이트** 방법에서 다음 입력 검사를 수행 합니다.
- 플레이어에서 컨트롤러 이동 사각형을 사용 하는 경우 포인터 위치의 변경 내용을 확인 하 고이를 사용 하 여 사용자가 포인터를 컨트롤러의 비활성 영역 밖으로 이동 했는지 여부를 계산 합니다. 데드 영역 외부에서 **m_moveCommand** vector 속성은 가상 조이스틱 값으로 업데이트 됩니다.
- 이동 키보드 입력을 누르면 또는에 대 한 값 `1.0f` `-1.0f` 이 **m_moveCommand** 벡터의 해당 구성 요소에 추가 되 &mdash; `1.0f` 고 앞으로에 추가 됩니다 `-1.0f` .

모든 이동 입력을 고려 하면 몇 가지 계산을 통해 **m_moveCommand** 벡터를 실행 하 여 게임 세계와 관련 하 여 플레이어의 방향을 나타내는 새 벡터를 생성 합니다.
그런 다음 전 세계와 관련 하 여 이동 하 고 해당 방향으로 플레이어에 게이를 적용 합니다.
마지막으로 **m_moveCommand** `(0.0f, 0.0f, 0.0f)` 다음 게임 프레임에 대 한 모든 것을 준비 하도록 m_moveCommand vector를로 다시 설정 합니다.

```cppwinrt
void MoveLookController::Update()
{
    // Get any gamepad input and update state
    UpdatePollingDevices();

    if (m_moveInUse)
    {
        // Move control.
        XMFLOAT2 pointerDelta;

        pointerDelta.x = m_movePointerPosition.x - m_moveFirstDown.x;
        pointerDelta.y = m_movePointerPosition.y - m_moveFirstDown.y;

        // Figure out the command from the virtual joystick.
        XMFLOAT3 commandDirection = XMFLOAT3(0.0f, 0.0f, 0.0f);
        // Leave 32 pixel-wide dead spot for being still.
        if (fabsf(pointerDelta.x) > 16.0f)
            m_moveCommand.x -= pointerDelta.x / fabsf(pointerDelta.x);

        if (fabsf(pointerDelta.y) > 16.0f)
            m_moveCommand.y -= pointerDelta.y / fabsf(pointerDelta.y);
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
    wCommand.x = m_moveCommand.x * cosf(m_yaw) - m_moveCommand.y * sinf(m_yaw);
    wCommand.y = m_moveCommand.x * sinf(m_yaw) + m_moveCommand.y * cosf(m_yaw);
    wCommand.z = m_moveCommand.z;

    // Scale for sensitivity adjustment.
    // Our velocity is based on the command. Y is up.
    m_velocity.x = -wCommand.x * MoveLookConstants::MovementGain;
    m_velocity.z = wCommand.y * MoveLookConstants::MovementGain;
    m_velocity.y = wCommand.z * MoveLookConstants::MovementGain;

    // Clear movement input accumulator for use during next frame.
    m_moveCommand = XMFLOAT3(0.0f, 0.0f, 0.0f);
}
```

## <a name="next-steps"></a>다음 단계

이제 컨트롤을 추가 했으므로 몰입 형 게임을 만들기 위해 추가 해야 하는 또 다른 기능이 있습니다.
음악과 음향 효과는 게임에 중요 하므로 다음에 [소리를 추가](tutorial--adding-sound.md) 하는 방법을 살펴보겠습니다.