---
title: 게임의 이동 컨트롤
description: 기존 마우스 및 키보드 이동 모양 컨트롤 (mouselook 컨트롤이 라고도 함)을 DirectX 게임에 추가 하는 방법에 대해 알아봅니다.
ms.assetid: 4b4d967c-3de9-8a97-ae68-0327f00cc933
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 이동 모양, 컨트롤
ms.localizationpriority: medium
ms.openlocfilehash: ed17d38bfaa90956b22265d1dfc320bfcc13ede7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165127"
---
# <a name="span-iddev_gamingtutorial__adding_move-look_controls_to_your_directx_gamespanmove-look-controls-for-games"></a><span id="dev_gaming.tutorial__adding_move-look_controls_to_your_directx_game"></span>게임의 이동 컨트롤



기존 마우스 및 키보드 이동 모양 컨트롤 (mouselook 컨트롤이 라고도 함)을 DirectX 게임에 추가 하는 방법에 대해 알아봅니다.

또한 방향 입력 처럼 동작 하는 화면의 왼쪽 아래 섹션으로 정의 된 이동 컨트롤러, 화면 나머지에 대해 정의 된 모양 컨트롤러 및 화면 나머지에 대해 정의 된 모양 컨트롤러를 사용 하 여 터치 장치에 대 한 이동에 대 한 지원에 대해 설명 합니다.

이 방법이 익숙하지 않은 사용자에 게 친숙 한 경우에는이를 고려해 야 합니다. 키보드 (또는 터치 기반 방향 입력 상자)는이 3D 공간에서 다리를 제어 하 고, 다리를 앞 이나 뒤로 이동할 수 있는 것 처럼 동작 하 고 왼쪽 및 오른쪽으로 strafing 합니다. 마우스 (또는 터치 포인터)는 헤드를 제어 합니다. 머리를 사용 하 여 왼쪽 또는 오른쪽, 위쪽 또는 아래쪽, 해당 평면의 위치를 확인 합니다. 보기에 대상이 있는 경우 마우스를 사용 하 여 해당 대상에 대 한 카메라 보기를 가운데에 배치 하 고 전방 키를 눌러 이동 하거나 뒤로 이동 합니다. 대상에 원을 표시 하려면 카메라 보기를 대상 중심으로 유지 하 고 왼쪽 또는 오른쪽으로 이동 합니다. 이 방법이 3D 환경을 탐색 하는 매우 효과적인 제어 방법 인지 확인할 수 있습니다.

이러한 컨트롤은 일반적으로 게임에서 WASD 컨트롤 이라고 합니다. 여기서 W, A, S 및 D 키는 x z 평면 고정 카메라 움직임에 사용 되 고 마우스는 x 축과 y 축을 중심으로 카메라 회전을 제어 하는 데 사용 됩니다.

## <a name="objectives"></a>목표


-   마우스와 키보드 및 터치 스크린 모두에 대 한 기본 이동 컨트롤을 DirectX 게임에 추가 합니다.
-   3D 환경을 탐색 하는 데 사용 되는 첫 번째 사용자 카메라를 구현 합니다.

## <a name="a-note-on-touch-control-implementations"></a>터치 제어 구현에 대 한 참고 사항


터치 컨트롤의 경우 두 개의 컨트롤러를 구현 합니다. 이동 컨트롤러는 카메라의 모양에 상대적인 x z 평면에서 이동을 처리 합니다. 카메라의 모양을 목표로 하는 보기 컨트롤러 이동 컨트롤러가 키보드 WASD 단추로 매핑되고, 모양 컨트롤러는 마우스로 매핑됩니다. 그러나 터치 컨트롤의 경우에는 방향 입력 또는 가상 WASD 단추 역할을 하는 화면의 영역을 정의 해야 하며,이는 화면의 나머지 부분이 모양 컨트롤의 입력 공간으로 사용 됩니다.

화면은 다음과 같습니다.

![이동 보기 컨트롤러 레이아웃](images/movelook-touch.png)

화면 왼쪽 아래에서 터치 포인터를 이동 하는 경우 (마우스를 표시 하지 않음) 위쪽으로 이동 하면 카메라를 앞으로 이동 합니다. 아래로 이동 하면 카메라가 뒤로 이동 합니다. 이동 컨트롤러의 포인터 공간 내에서 왼쪽 및 오른쪽 이동의 경우에도 마찬가지입니다. 해당 공간 외에도 모양의 컨트롤러가 됩니다. 카메라를 얼굴을 원하는 위치로 이동 하거나 끕니다.

## <a name="set-up-the-basic-input-event-infrastructure"></a>기본 입력 이벤트 인프라 설정


먼저 마우스 및 키보드에서 입력 이벤트를 처리 하는 데 사용 하는 컨트롤 클래스를 만들고 해당 입력에 따라 카메라 큐브 뷰를 업데이트 해야 합니다. 이동 모양 컨트롤을 구현 하기 때문에 **MoveLookController**라고 합니다.

```cpp
using namespace Windows::UI::Core;
using namespace Windows::System;
using namespace Windows::Foundation;
using namespace Windows::Devices::Input;
#include <DirectXMath.h>

// Methods to get input from the UI pointers
ref class MoveLookController
{
};  // class MoveLookController
```

이제 이동 하는 컨트롤러 및 해당 자사 카메라의 상태를 정의 하는 헤더와 컨트롤을 구현 하 고 카메라의 상태를 업데이트 하는 기본 메서드 및 이벤트 처리기를 만들어 보겠습니다.

```cpp
#define ROTATION_GAIN 0.004f    // Sensitivity adjustment for the look controller
#define MOVEMENT_GAIN 0.1f      // Sensitivity adjustment for the move controller

ref class MoveLookController
{
private:
    // Properties of the controller object
    DirectX::XMFLOAT3 m_position;               // The position of the controller
    float m_pitch, m_yaw;           // Orientation euler angles in radians

    // Properties of the Move control
    bool m_moveInUse;               // Specifies whether the move control is in use
    uint32 m_movePointerID;         // Id of the pointer in this control
    DirectX::XMFLOAT2 m_moveFirstDown;          // Point where initial contact occurred
    DirectX::XMFLOAT2 m_movePointerPosition;   // Point where the move pointer is currently located
    DirectX::XMFLOAT3 m_moveCommand;            // The net command from the move control

    // Properties of the Look control
    bool m_lookInUse;               // Specifies whether the look control is in use
    uint32 m_lookPointerID;         // Id of the pointer in this control
    DirectX::XMFLOAT2 m_lookLastPoint;          // Last point (from last frame)
    DirectX::XMFLOAT2 m_lookLastDelta;          // For smoothing

    bool m_forward, m_back;         // States for movement
    bool m_left, m_right;
    bool m_up, m_down;


public:

    // Methods to get input from the UI pointers
    void OnPointerPressed(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );

    void OnPointerMoved(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );

    void OnPointerReleased(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );

    void OnKeyDown(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::KeyEventArgs^ args
        );

    void OnKeyUp(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::KeyEventArgs^ args
        );

    // Set up the Controls that this controller supports
    void Initialize( _In_ Windows::UI::Core::CoreWindow^ window );

    void Update( Windows::UI::Core::CoreWindow ^window );
    
internal:
    // Accessor to set position of controller
    void SetPosition( _In_ DirectX::XMFLOAT3 pos );

    // Accessor to set position of controller
    void SetOrientation( _In_ float pitch, _In_ float yaw );

    // Returns the position of the controller object
    DirectX::XMFLOAT3 get_Position();

    // Returns the point  which the controller is facing
    DirectX::XMFLOAT3 get_LookPoint();


};  // class MoveLookController
```

코드에는 네 개의 전용 필드 그룹이 포함 되어 있습니다. 각 항목의 용도를 검토해 보겠습니다.

먼저, 카메라 보기에 대 한 업데이트 된 정보를 포함 하는 몇 가지 유용한 필드를 정의 합니다.

-   **m \_ position** 은 장면 좌표를 사용 하 여 3d 장면에서 카메라의 위치 (따라서 viewplane)입니다.
-   **m \_ 피치** 는 카메라의 피치 또는 viewplane x 축 주위에 있는 up-down 회전 (라디안)입니다.
-   **m \_ 요** 는 카메라의 요 또는 viewplane y 축을 중심으로 왼쪽에서 오른쪽으로 회전 하는 각도 (라디안)입니다.

이제 컨트롤러의 상태 및 위치에 대 한 정보를 저장 하는 데 사용 하는 필드를 정의 해 보겠습니다. 먼저 touch 기반 이동 컨트롤러에 필요한 필드를 정의 합니다. 이동 컨트롤러의 키보드 구현에는 특별 한 작업이 필요 하지 않습니다. 특정 처리기를 사용 하 여 키보드 이벤트를 읽기만 합니다.

-   **m \_ moveInUse** 이동 컨트롤러를 사용 중인지 여부를 나타냅니다.
-   **m \_ movepointerid** 는 현재 이동 포인터에 대 한 고유 id입니다. 포인터 ID 값을 확인할 때이를 사용 하 여 모양 포인터와 이동 포인터를 구분 합니다.
-   **m \_ movefirstdown** 은 플레이어가 컨트롤러 이동 포인터 영역을 먼저 연결한 화면에 있는 지점입니다. 나중에이 값을 사용 하 여 jittering에서 가장 작은 움직임을 유지 하기 위해 데드 영역을 설정 합니다.
-   **m \_ movepointerposition** 은 플레이어에서 현재 포인터를 이동 하는 화면에 있는 지점입니다. 이를 통해 **m \_ movefirstdown**을 기준으로 하 여 플레이어의 이동 방향을 결정 합니다.
-   **m \_ moveCommand** 은 이동 컨트롤러에 대 한 최종 계산 된 명령입니다. up (forward), down (back), left 또는 right입니다.

이제는 모양 컨트롤러에 사용 하는 필드와 마우스 및 터치 구현을 정의 합니다.

-   **m \_ lookInUse** 는 모양 컨트롤이 사용 중인지 여부를 나타냅니다.
-   **m \_ lookPointerID** 는 현재 모양 포인터에 대 한 고유 ID입니다. 포인터 ID 값을 확인할 때이를 사용 하 여 모양 포인터와 이동 포인터를 구분 합니다.
-   **m \_ lookLastPoint** 는 이전 프레임에서 캡처된 마지막 지점 (장면 좌표)입니다.
-   **m \_ lookLastDelta** 은 현재 **m \_ 위치** 와 **m \_ lookLastPoint**간의 계산 된 차이입니다.

마지막으로, 각 방향 이동 작업 (on 또는 off)의 현재 상태를 나타내는 데 사용 되는 6도의 이동에 대해 6 개의 부울 값을 정의 합니다.

-   **m \_ 앞**으로 **, m \_ 뒤로**, **m \_ 왼쪽**, **m \_ 오른쪽**, **m \_ 위쪽** 및 **m \_ **

6 개의 이벤트 처리기를 사용 하 여 컨트롤러의 상태를 업데이트 하는 데 사용 하는 입력 데이터를 캡처합니다.

-   **Onpointerpressed**있습니다. 플레이어가 게임 화면에 포인터를 놓고 왼쪽 마우스 단추를 누르거나 화면을 작업 했습니다.
-   **Onpointermoved**했습니다. 플레이어는 게임 화면의 포인터로 마우스를 이동 하거나 화면에서 터치 포인터를 끕니다.
-   **Onpointerreleased**. 플레이어가 게임 화면에 포인터를 놓고 왼쪽 마우스 단추를 눌렀다 놓은 상태에서 화면 터치를 중지 했습니다.
-   **OnKeyDown**. 플레이어에서 키를 눌렀습니다.
-   **OnKeyUp**. 플레이어에서 키를 해제 했습니다.

마지막으로, 이러한 메서드 및 속성을 사용 하 여 컨트롤러의 상태 정보를 초기화, 액세스 및 업데이트 합니다.

-   **초기화**. 앱은이 이벤트 처리기를 호출 하 여 컨트롤을 초기화 하 고 표시 창을 설명 하는 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 개체에 연결 합니다.
-   **Setposition**. 앱은이 메서드를 호출 하 여 장면 공간에서 컨트롤의 (x, y, z) 좌표를 설정 합니다.
-   **Setorientation**. 앱은이 메서드를 호출 하 여 카메라의 피치와 요를 설정 합니다.
-   **get \_ 위치**. 앱이이 속성에 액세스 하 여 장면 공간에서 카메라의 현재 위치를 가져옵니다. 현재 카메라 위치를 앱에 전달 하는 방법으로이 속성을 사용 합니다.
-   **get \_ LookPoint**. 앱이이 속성에 액세스 하 여 컨트롤러 카메라가 연결 된 현재 지점을 가져옵니다.
-   **업데이트**. 이동 및 보기 컨트롤러의 상태를 읽고 카메라 위치를 업데이트 합니다. 앱의 주 루프에서이 메서드를 지속적으로 호출 하 여 카메라 컨트롤러 데이터와 장면 공간에서 카메라 위치를 새로 고칩니다.

이제 이동 모양 컨트롤을 구현 하는 데 필요한 모든 구성 요소가 있습니다. 따라서 이러한 조각을 함께 연결 해 보겠습니다.

## <a name="create-the-basic-input-events"></a>기본 입력 이벤트 만들기


Windows 런타임 이벤트 디스패처는 **MoveLookController** 클래스의 인스턴스가 처리할 5 개의 이벤트를 제공 합니다.

-   [**PointerPressed**](/uwp/api/windows.ui.core.corewindow.pointerpressed)
-   [**PointerMoved 됨**](/uwp/api/windows.ui.core.corewindow.pointermoved)
-   [**PointerReleased**](/uwp/api/windows.ui.core.corewindow.pointerreleased)
-   [**KeyUp**](/uwp/api/windows.ui.core.corewindow.keyup)
-   [**KeyDown**](/uwp/api/windows.ui.core.corewindow.keydown)

이러한 이벤트는 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 형식에 대해 구현 됩니다. 사용할 **CoreWindow** 개체가 있다고 가정 합니다. 이를 가져오는 방법을 모를 경우에 [는 유니버설 Windows 플랫폼 (UWP) c + + 앱을 설정 하 여 DirectX 보기를 표시 하는 방법](/previous-versions/windows/apps/hh465077(v=win.10))을 참조 하세요.

앱이 실행 되는 동안 이러한 이벤트가 발생 하면 처리기는 전용 필드에 정의 된 컨트롤러의 상태 정보를 업데이트 합니다.

먼저 마우스 및 터치 포인터 이벤트 처리기를 채웁니다. 첫 번째 이벤트 처리기 인 **Onpointerpressed ()** 에서는 사용자가 마우스를 클릭 하거나 모양 컨트롤러 영역에서 화면을 닿을 때 디스플레이를 관리 하는 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 포인터의 x-y 좌표를 가져옵니다.

**OnPointerPressed**

```cpp
void MoveLookController::OnPointerPressed(
_In_ CoreWindow^ sender,
_In_ PointerEventArgs^ args)
{
    // Get the current pointer position.
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    auto device = args->CurrentPoint->PointerDevice;
    auto deviceType = device->PointerDeviceType;
    if ( deviceType == PointerDeviceType::Mouse )
    {
        // Action, Jump, or Fire
    }

    // Check  if this pointer is in the move control.
    // Change the values  to percentages of the preferred screen resolution.
    // You can set the x value to <preferred resolution> * <percentage of width>
    // for example, ( position.x < (screenResolution.x * 0.15) ).

    if (( position.x < 300 && position.y > 380 ) && ( deviceType != PointerDeviceType::Mouse ))
    {
        if ( !m_moveInUse ) // if no pointer is in this control yet
        {
            // Process a DPad touch down event.
            m_moveFirstDown = position;                 // Save the location of the initial contact.
            m_movePointerPosition = position;
            m_movePointerID = pointerID;                // Store the id of the pointer using this control.
            m_moveInUse = TRUE;
        }
    }
    else // This pointer must be in the look control.
    {
        if ( !m_lookInUse ) // If no pointer is in this control yet...
        {
            m_lookLastPoint = position;                         // save the point for later move
            m_lookPointerID = args->CurrentPoint->PointerId;  // store the id of pointer using this control
            m_lookLastDelta.x = m_lookLastDelta.y = 0;          // these are for smoothing
            m_lookInUse = TRUE;
        }
    }
}
```

이 이벤트 처리기는 포인터가 마우스가 아닌지 (마우스 및 터치를 모두 지 원하는이 샘플의 목적), 컨트롤러 이동 영역에 있는지 여부를 확인 합니다. 두 조건이 모두 true 인 경우에는 포인터를 방금 눌렀는지 여부를 확인 합니다. 특히,이 클릭이 이전 이동 또는 검색 입력과 관련이 없는지를 확인 하 여 **m \_ moveInUse** 이 false 인지 테스트 합니다. 이 경우 처리기는 키가 발생 한 이동 컨트롤러 영역에서 지점을 캡처하고 **m \_ moveInUse** 을 true로 설정 하 여이 처리기가 다시 호출 될 때 이동 컨트롤러 입력 상호 작용의 시작 위치를 덮어쓰지 않습니다. 또한 move controller 포인터 ID를 현재 포인터의 ID로 업데이트 합니다.

포인터가 마우스 이거나 터치 포인터가 컨트롤러 이동 영역에 없으면 컨트롤러 보기 영역에 있어야 합니다. **M \_ lookLastPoint** 을 사용자가 마우스 단추를 누른 상태에서 마우스 단추를 누른 상태에서 현재 위치로 설정 하 고, 델타를 다시 설정 하 고, 모양 컨트롤러의 포인터 id를 현재 포인터 id로 업데이트 합니다. 또한 보기 컨트롤러의 상태를 활성으로 설정 합니다.

**OnPointerMoved 됨**

```cpp
void MoveLookController::OnPointerMoved(
    _In_ CoreWindow ^sender,
    _In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2(args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y);

    // Decide which control this pointer is operating.
    if (pointerID == m_movePointerID)           // This is the move pointer.
    {
        // Move control
        m_movePointerPosition = position;       // Save the current position.

    }
    else if (pointerID == m_lookPointerID)      // This is the look pointer.
    {
        // Look control

        DirectX::XMFLOAT2 pointerDelta;
        pointerDelta.x = position.x - m_lookLastPoint.x;        // How far did pointer move
        pointerDelta.y = position.y - m_lookLastPoint.y;

        DirectX::XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x * ROTATION_GAIN;   // Scale for control sensitivity.
        rotationDelta.y = pointerDelta.y * ROTATION_GAIN;

        m_lookLastPoint = position;                     // Save for the next time through.

                                                        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;                     // Mouse y increases down, but pitch increases up.
        m_yaw -= rotationDelta.x;                       // Yaw is defined as CCW around the y-axis.

                                                        // Limit the pitch to straight up or straight down.
        m_pitch = (float)__max(-DirectX::XM_PI / 2.0f, m_pitch);
        m_pitch = (float)__min(+DirectX::XM_PI / 2.0f, m_pitch);
    }
}
```

**Onpointermoved** 이벤트 처리기는 포인터가 이동할 때마다 (이 경우 터치 스크린 포인터를 끌고 있거나 왼쪽 단추를 누른 상태에서 마우스 포인터를 이동 하는 경우) 발생 합니다. 포인터 ID가 이동 컨트롤러 포인터의 ID와 같으면 포인터는 이동 포인터입니다. 그렇지 않은 경우 활성 포인터인 표시 컨트롤러 인지 확인 합니다.

이동 컨트롤러인 경우 포인터 위치를 업데이트 합니다. **Onpointerpressed** 이벤트 처리기를 사용 하 여 마지막 위치를 캡처된 첫 번째 위치와 비교 하려고 하기 때문에 [**pointermoved**](/uwp/api/windows.ui.core.corewindow.pointermoved) 이벤트가 계속 발생 하는 동안 계속 해 서 업데이트 합니다.

보기 컨트롤러인 경우에는 약간 더 복잡 합니다. 새 모양을 계산 하 고 카메라를 가운데에 배치 해야 합니다. 그러면 마지막 모양과 현재 화면 위치 사이의 델타를 계산 하 고 배율 인수를 곱합니다. 그러면 화면 이동 거리를 기준으로 하 여 더 작거나 더 큰 모양으로 이동 합니다. 이 값을 사용 하 여 피치와 요 계산 합니다.

마지막으로 플레이어에서 마우스 이동을 중지 하거나 화면을 터치 하는 경우 이동 또는 보기 컨트롤러 동작을 비활성화 해야 합니다. [**Pointerreleased**](/uwp/api/windows.ui.core.corewindow.pointerreleased) 가 실행 될 때 호출 되는 **onpointerreleased**을 사용 하 고, **m \_ moveInUse** 또는 **m \_ lookInUse** 를 FALSE로 설정 하 고, 카메라 팬 이동을 끄고, 포인터 ID를 0으로 설정 합니다.

**OnPointerReleased**

```cpp
void MoveLookController::OnPointerReleased(
_In_ CoreWindow ^sender,
_In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );


    if ( pointerID == m_movePointerID )    // This was the move pointer.
    {
        m_moveInUse = FALSE;
        m_movePointerID = 0;
    }
    else if (pointerID == m_lookPointerID ) // This was the look pointer.
    {
        m_lookInUse = FALSE;
        m_lookPointerID = 0;
    }
}
```

지금까지 모든 터치 스크린 이벤트를 처리 했습니다. 이제 키보드 기반 이동 컨트롤러에 대 한 키 입력 이벤트를 처리 해 보겠습니다.

**OnKeyDown**

```cpp
void MoveLookController::OnKeyDown(
                                   __in CoreWindow^ sender,
                                   __in KeyEventArgs^ args )
{
    Windows::System::VirtualKey Key;
    Key = args->VirtualKey;

    // Figure out the command from the keyboard.
    if ( Key == VirtualKey::W )     // Forward
        m_forward = true;
    if ( Key == VirtualKey::S )     // Back
        m_back = true;
    if ( Key == VirtualKey::A )     // Left
        m_left = true;
    if ( Key == VirtualKey::D )     // Right
        m_right = true;
}
```

이러한 키 중 하나를 누르면이 이벤트 처리기는 해당 방향성 move 상태를 true로 설정 합니다.

**OnKeyUp**

```cpp
void MoveLookController::OnKeyUp(
                                 __in CoreWindow^ sender,
                                 __in KeyEventArgs^ args)
{
    Windows::System::VirtualKey Key;
    Key = args->VirtualKey;

    // Figure out the command from the keyboard.
    if ( Key == VirtualKey::W )     // forward
        m_forward = false;
    if ( Key == VirtualKey::S )     // back
        m_back = false;
    if ( Key == VirtualKey::A )     // left
        m_left = false;
    if ( Key == VirtualKey::D )     // right
        m_right = false;
}
```

키가 릴리스되면이 이벤트 처리기는 키를 다시 false로 설정 합니다. **업데이트**를 호출 하면 이러한 방향 이동 상태를 확인 하 고 카메라를 적절 하 게 이동 합니다. 이는 터치 구현 보다 약간 더 간단 합니다.

## <a name="initialize-the-touch-controls-and-the-controller-state"></a>터치 컨트롤 및 컨트롤러 상태를 초기화 합니다.


지금 이벤트를 연결 하 고 모든 컨트롤러 상태 필드를 초기화 해 보겠습니다.

**초기**

```cpp
void MoveLookController::Initialize( _In_ CoreWindow^ window )
{

    // Opt in to receive touch/mouse events.
    window->PointerPressed += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

    window->PointerMoved += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

    window->PointerReleased += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);

    window->CharacterReceived +=
    ref new TypedEventHandler<CoreWindow^, CharacterReceivedEventArgs^>(this, &MoveLookController::OnCharacterReceived);

    window->KeyDown += 
    ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyDown);

    window->KeyUp += 
    ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyUp);

    // Initialize the state of the controller.
    m_moveInUse = FALSE;                // No pointer is in the Move control.
    m_movePointerID = 0;

    m_lookInUse = FALSE;                // No pointer is in the Look control.
    m_lookPointerID = 0;

    //  Need to init this as it is reset every frame.
    m_moveCommand = DirectX::XMFLOAT3( 0.0f, 0.0f, 0.0f );

    SetOrientation( 0, 0 );             // Look straight ahead when the app starts.

}
```

**Initialize** 는 응용 프로그램의 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 인스턴스에 대 한 참조를 매개 변수로 사용 하 고, 개발한 이벤트 처리기를 해당 **CoreWindow**의 적절 한 이벤트에 등록 합니다. 이동 및 보기 포인터의 Id를 초기화 하 고, 터치 스크린 이동 컨트롤러 구현에 대 한 명령 벡터를 0으로 설정 하 고, 앱이 시작 될 때 바로 카메라를 표시 하도록 설정 합니다.

## <a name="getting-and-setting-the-position-and-orientation-of-the-camera"></a>카메라의 위치 및 방향 가져오기 및 설정


뷰포트를 기준으로 카메라의 위치를 가져오고 설정 하는 몇 가지 메서드를 정의 해 보겠습니다.

```cpp
void MoveLookController::SetPosition( _In_ DirectX::XMFLOAT3 pos )
{
    m_position = pos;
}

// Accessor to set the position of the controller.
void MoveLookController::SetOrientation( _In_ float pitch, _In_ float yaw )
{
    m_pitch = pitch;
    m_yaw = yaw;
}

// Returns the position of the controller object.
DirectX::XMFLOAT3 MoveLookController::get_Position()
{
    return m_position;
}

// Returns the point at which the camera controller is facing.
DirectX::XMFLOAT3 MoveLookController::get_LookPoint()
{
    float y = sinf(m_pitch);        // Vertical
    float r = cosf(m_pitch);        // In the plane
    float z = r*cosf(m_yaw);        // Fwd-back
    float x = r*sinf(m_yaw);        // Left-right
    DirectX::XMFLOAT3 result(x,y,z);
    result.x += m_position.x;
    result.y += m_position.y;
    result.z += m_position.z;

    // Return m_position + DirectX::XMFLOAT3(x, y, z);
    return result;
}
```

## <a name="updating-the-controller-state-info"></a>컨트롤러 상태 정보를 업데이트 하는 중


이제 **m \_ movepointerposition** 에서 추적 되는 포인터 좌표 정보를 전 세계 좌표계의 새 좌표 정보로 변환 하는 계산을 수행 합니다. 앱은 기본 앱 루프를 새로 고칠 때마다이 메서드를 호출 합니다. 따라서 여기에서 뷰포트에 프로젝션 하기 전에 뷰 매트릭스를 업데이트 하기 위해 앱에 전달 하려는 새 모양 위치 정보를 계산 합니다.

```cpp
void MoveLookController::Update(CoreWindow ^window)
{
    // Check for input from the Move control.
    if (m_moveInUse)
    {
        DirectX::XMFLOAT2 pointerDelta(m_movePointerPosition);
        pointerDelta.x -= m_moveFirstDown.x;
        pointerDelta.y -= m_moveFirstDown.y;

        // Figure out the command from the touch-based virtual joystick.
        if (pointerDelta.x > 16.0f)      // Leave 32 pixel-wide dead spot for being still.
            m_moveCommand.x = 1.0f;
        else
            if (pointerDelta.x < -16.0f)
            m_moveCommand.x = -1.0f;

        if (pointerDelta.y > 16.0f)      // Joystick y is up, so change sign.
            m_moveCommand.y = -1.0f;
        else
            if (pointerDelta.y < -16.0f)
            m_moveCommand.y = 1.0f;
    }

    // Poll our state bits that are set by the keyboard input events.
    if (m_forward)
        m_moveCommand.y += 1.0f;
    if (m_back)
        m_moveCommand.y -= 1.0f;

    if (m_left)
        m_moveCommand.x -= 1.0f;
    if (m_right)
        m_moveCommand.x += 1.0f;

    if (m_up)
        m_moveCommand.z += 1.0f;
    if (m_down)
        m_moveCommand.z -= 1.0f;

    // Make sure that 45 degree cases are not faster.
    DirectX::XMFLOAT3 command = m_moveCommand;
    DirectX::XMVECTOR vector;
    vector = DirectX::XMLoadFloat3(&command);

    if (fabsf(command.x) > 0.1f || fabsf(command.y) > 0.1f || fabsf(command.z) > 0.1f)
    {
        vector = DirectX::XMVector3Normalize(vector);
        DirectX::XMStoreFloat3(&command, vector);
    }
    

    // Rotate command to align with our direction (world coordinates).
    DirectX::XMFLOAT3 wCommand;
    wCommand.x = command.x*cosf(m_yaw) - command.y*sinf(m_yaw);
    wCommand.y = command.x*sinf(m_yaw) + command.y*cosf(m_yaw);
    wCommand.z = command.z;

    // Scale for sensitivity adjustment.
    wCommand.x = wCommand.x * MOVEMENT_GAIN;
    wCommand.y = wCommand.y * MOVEMENT_GAIN;
    wCommand.z = wCommand.z * MOVEMENT_GAIN;

    // Our velocity is based on the command.
    // Also note that y is the up-down axis. 
    DirectX::XMFLOAT3 Velocity;
    Velocity.x = -wCommand.x;
    Velocity.z = wCommand.y;
    Velocity.y = wCommand.z;

    // Integrate
    m_position.x += Velocity.x;
    m_position.y += Velocity.y;
    m_position.z += Velocity.z;

    // Clear movement input accumulator for use during the next frame.
    m_moveCommand = DirectX::XMFLOAT3(0.0f, 0.0f, 0.0f);

}
```

플레이어에서 touch 기반 이동 컨트롤러를 사용 하는 경우에는 떨림 이동이 필요 하지 않기 때문에 32 픽셀의 직경을 사용 하 여 포인터 주위에 가상 데드 영역을 설정 합니다. 또한 명령 값에 이동 게인 속도를 더한 속도를 추가 합니다. (컨트롤러 이동 영역에서 포인터가 이동 하는 거리를 기준으로이 동작을 원하는 대로 조정 하 여 이동 속도를 높이는 속도를 높일 수 있습니다.)

속도를 계산할 때 이동 및 보기 컨트롤러에서 받은 좌표를 장면에 대 한 뷰 매트릭스를 계산 하는 메서드로 보내는 실제 모양 점의 이동으로 변환 합니다. 첫 번째는 x 좌표를 반전 합니다. 즉, 모양 컨트롤러와 함께 왼쪽 또는 오른쪽을 클릭 하거나 이동 하는 경우 카메라의 중앙 축이 회전 될 수 있으므로 모양 점에서 반대 방향으로 회전 합니다. 그런 다음 이동 컨트롤러에서 위쪽/아래쪽 키 누르기 또는 터치 끌기 동작 (y 축 동작으로 읽음)이 화면에서 (z 축) 모양 또는 화면 밖으로 이동 하는 카메라 동작으로 변환 되기 때문에 y 축과 z 축을 바꿔 보겠습니다.

플레이어의 모양에 대 한 최종 위치는 마지막 위치에 계산 된 속도를 더한 것 이며,이는 **get \_ position** 메서드 (각 프레임에 대 한 설치 중)를 호출할 때 렌더러에서 읽습니다. 그런 다음 move 명령을 0으로 다시 설정 합니다.

## <a name="updating-the-view-matrix-with-the-new-camera-position"></a>새 카메라 위치를 사용 하 여 뷰 매트릭스 업데이트


카메라에 포커스가 있는 장면 공간 좌표를 얻을 수 있으며, 앱에 지시할 때마다 업데이트 됩니다 (예: 기본 앱 루프에서 60 초 마다). 이 의사 코드에서는 구현할 수 있는 호출 동작을 제안 합니다.

```cpp
myMoveLookController->Update( m_window );   

// Update the view matrix based on the camera position.
myFirstPersonCamera->SetViewParameters(
                 myMoveLookController->get_Position(),       // Point we are at
                 myMoveLookController->get_LookPoint(),      // Point to look towards
                 DirectX::XMFLOAT3( 0, 1, 0 )                   // Up-vector
                 ); 
```

축하합니다! 게임에서 터치 스크린과 키보드/마우스 입력 터치 컨트롤에 대 한 기본 이동 모양 컨트롤을 구현 했습니다.



 

 