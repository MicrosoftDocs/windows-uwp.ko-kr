---
title: 게임 터치 컨트롤
description: DirectX를 사용 하 여 UWP (유니버설 Windows 플랫폼) c + + 게임에 기본 터치 컨트롤을 추가 하는 방법에 대해 알아봅니다.
ms.assetid: 9d40e6e4-46a9-97e9-b848-522d61e8e109
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 터치, 컨트롤, directx, 입력
ms.localizationpriority: medium
ms.openlocfilehash: 546d36e26489563720f5aad8a0c9f81f85649e5a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168287"
---
# <a name="touch-controls-for-games"></a>게임 터치 컨트롤



DirectX를 사용 하 여 UWP (유니버설 Windows 플랫폼) c + + 게임에 기본 터치 컨트롤을 추가 하는 방법에 대해 알아봅니다. 터치 기반 컨트롤을 추가 하 여 Direct3D 환경에서 고정 평면 카메라를 이동 하는 방법을 보여 줍니다 .이 경우 손가락 또는 스타일러스를 사용 하 여 끌면 카메라 큐브 뷰가 이동 합니다.

플레이어에서 지도 또는 playfield와 같은 3D 환경에서 스크롤하거나 이동 하려는 게임에 이러한 컨트롤을 통합할 수 있습니다. 예를 들어 전략 또는 퍼즐 게임에서는 이러한 컨트롤을 사용 하 여 왼쪽 또는 오른쪽으로 이동 하 여 플레이어에서 화면 보다 큰 게임 환경을 볼 수 있도록 합니다.

> **참고**    코드는 마우스 기반 패닝 컨트롤 에서도 작동 합니다. 포인터 관련 이벤트는 Windows 런타임 Api에 의해 추상화 되므로 터치 또는 마우스 기반 포인터 이벤트를 처리할 수 있습니다.

 

## <a name="objectives"></a>목표


-   DirectX 게임에서 고정 평면 카메라를 패닝 하기 위한 간단한 터치 끌기 컨트롤을 만듭니다.

## <a name="set-up-the-basic-touch-event-infrastructure"></a>기본 터치 이벤트 인프라 설정


먼저 기본 컨트롤러 유형인이 경우 **CameraPanController**를 정의 합니다. 여기서는 사용자가 수행할 수 있는 동작 집합인 컨트롤러를 추상 개념으로 정의 합니다.

**CameraPanController** 클래스는 카메라 컨트롤러 상태에 대 한 정보를 정기적으로 새로 고치고 앱이 업데이트 루프에서 해당 정보를 가져올 수 있는 방법을 제공 합니다.

```cpp
using namespace Windows::UI::Core;
using namespace Windows::System;
using namespace Windows::Foundation;
using namespace Windows::Devices::Input;
#include <directxmath.h>

// Methods to get input from the UI pointers
ref class CameraPanController
{
}
```

이제 카메라 컨트롤러의 상태와 카메라 컨트롤러 상호 작용을 구현 하는 기본 메서드 및 이벤트 처리기를 정의 하는 헤더를 만들어 보겠습니다.

```cpp
ref class CameraPanController
{
private:
    // Properties of the controller object
    DirectX::XMFLOAT3 m_position;               // the position of the camera

    // Properties of the camera pan control
    bool m_panInUse;                
    uint32 m_panPointerID;          
    DirectX::XMFLOAT2 m_panFirstDown;           
    DirectX::XMFLOAT2 m_panPointerPosition;   
    DirectX::XMFLOAT3 m_panCommand;         
    
internal:
    // Accessor to set the position of the controller
    void SetPosition( _In_ DirectX::XMFLOAT3 pos );

       // Accessor to set the fixed "look point" of the controller
       DirectX::XMFLOAT3 get_FixedLookPoint();

    // Returns the position of the controller object
    DirectX::XMFLOAT3 get_Position();

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

    // Set up the Controls supported by this controller
    void Initialize( _In_ Windows::UI::Core::CoreWindow^ window );

    void Update( Windows::UI::Core::CoreWindow ^window );

};  // Class CameraPanController
```

전용 필드에는 카메라 컨트롤러의 현재 상태가 포함 됩니다. 이를 검토해 보겠습니다.

-   **m \_ position** 은 장면 공간에서 카메라의 위치입니다. 이 예제에서 z 좌표 값은 0에서 고정 됩니다. DirectX:: XMFLOAT2를 사용 하 여이 값을 나타낼 수 있지만이 샘플 및 이후 확장성의 목적에는 DirectX:: XMFLOAT3를 사용 합니다. 이 값을 적절 하 게 업데이트 하려면 **get \_ Position** 속성을 통해 앱 자체에이 값을 전달 합니다.
-   **m \_ panInUse** 은 이동 작업이 활성 상태 인지, 아니면 플레이어에서 화면을 터치 하 고 카메라를 이동 하는지 여부를 나타내는 부울 값입니다.
-   **m \_ ** 은 포인터에 대 한 고유 id입니다. 샘플에서는이 기능을 사용 하지 않지만 컨트롤러 상태 클래스를 특정 포인터와 연결 하는 것이 좋습니다.
-   **m \_ ** 확대/축소는 플레이어가 화면에 처음으로 작업을 하거나 카메라 이동 작업 중 마우스를 클릭 한 지점입니다. 나중에이 값을 사용 하 여 화면이 작업 될 때 지터를 방지 하도록 데드 영역을 설정 하거나 마우스 shakes 작은 경우를 사용 합니다.
-   **m \_ 위치** 는 플레이어가 현재 포인터를 이동한 화면에 있는 지점입니다. 이를 사용 하 여 플레이어가 이동 하는 방향을 확인 하 고 **m \_ **을 기준으로 하 여 이동 합니다.
-   **m \_ ** 단추는 카메라 컨트롤러의 최종 계산 된 명령 (위쪽, 아래쪽, 왼쪽 또는 오른쪽)입니다. X-y 평면으로 고정 된 카메라로 작업 하 고 있으므로이는 DirectX:: XMFLOAT2 값이 될 수 있습니다.

이러한 3 개의 이벤트 처리기를 사용 하 여 카메라 컨트롤러 상태 정보를 업데이트 합니다.

-   **Onpointerpressed** 는 플레이어가 터치 표면에 손가락을 누를 때 앱이 호출 하는 이벤트 처리기입니다. 포인터가 누름의 좌표로 이동 합니다.
-   **Onpointermoved** 은 플레이어가 터치 표면에서 손가락을 swipes 때 앱이 호출 하는 이벤트 처리기입니다. 끌기 경로의 새 좌표를 사용 하 여 업데이트 합니다.
-   **Onpointerreleased** 은 플레이어에서 누름 손가락을 터치 화면에서 제거할 때 앱이 호출 하는 이벤트 처리기입니다.

마지막으로, 이러한 메서드 및 속성을 사용 하 여 카메라 컨트롤러 상태 정보를 초기화, 액세스 및 업데이트 합니다.

-   **Initialize** 는 컨트롤을 초기화 하 고 표시 창을 설명 하는 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 개체에 연결 하기 위해 앱이 호출 하는 이벤트 처리기입니다.
-   **Setposition** 은 앱이 장면 공간에서 컨트롤의 (x, y, z) 좌표를 설정 하기 위해 호출 하는 메서드입니다. 이 자습서 전체에서 z 좌표는 0입니다.
-   **get \_ Position** 은 앱이 장면 공간에서 카메라의 현재 위치를 가져오기 위해 액세스 하는 속성입니다. 현재 카메라 위치를 앱에 전달 하는 방법으로이 속성을 사용 합니다.
-   **get \_ FixedLookPoint** 는 컨트롤러 카메라가 향하는 현재 지점을 얻기 위해 앱이 액세스 하는 속성입니다. 이 예제에서는 x-y 평면에 대해 normal로 잠깁니다.
-   **업데이트** 는 컨트롤러 상태를 읽고 카메라 위치를 업데이트 하는 메서드입니다. &lt; &gt; 앱의 주 루프에서이 항목을 지속적으로 호출 하 여 카메라 컨트롤러 데이터와 장면 공간에서 카메라 위치를 새로 고칩니다.

이제 터치 컨트롤을 구현 하는 데 필요한 모든 구성 요소가 여기에 있습니다. 터치 또는 마우스 포인터 이벤트가 발생 한 시기와 위치 및 동작을 검색할 수 있습니다. 장면 공간을 기준으로 카메라의 위치와 방향을 설정 하 고 변경 내용을 추적할 수 있습니다. 마지막으로 새 카메라 위치를 호출 하는 앱에 전달할 수 있습니다.

이제 이러한 조각을 함께 연결 해 보겠습니다.

## <a name="create-the-basic-touch-events"></a>기본 터치 이벤트 만들기


Windows 런타임 이벤트 발송자는 앱에서 처리할 3 개의 이벤트를 제공 합니다.

-   [**PointerPressed**](/uwp/api/windows.ui.core.corewindow.pointerpressed)
-   [**PointerMoved 됨**](/uwp/api/windows.ui.core.corewindow.pointermoved)
-   [**PointerReleased**](/uwp/api/windows.ui.core.corewindow.pointerreleased)

이러한 이벤트는 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 형식에 대해 구현 됩니다. 사용할 **CoreWindow** 개체가 있다고 가정 합니다. 자세한 내용은 [UWP c + + 앱을 설정 하 여 DirectX 보기를 표시 하는 방법](/previous-versions/windows/apps/hh465077(v=win.10))을 참조 하세요.

앱이 실행 되는 동안 이러한 이벤트가 발생 하면 처리기는 개인 필드에 정의 된 카메라 컨트롤러 상태 정보를 업데이트 합니다.

먼저 터치 포인터 이벤트 처리기를 채웁니다. 첫 번째 이벤트 처리기에서 **Onpointerpressed**있으면 사용자가 화면에 접촉 하거나 마우스를 클릭할 때 디스플레이를 관리 하는 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 에서 포인터의 x-y 좌표를 가져옵니다.

**OnPointerPressed**

```cpp
void CameraPanController::OnPointerPressed(
                                           _In_ CoreWindow^ sender,
                                           _In_ PointerEventArgs^ args)
{
    // Get the current pointer position.
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    auto device = args->CurrentPoint->PointerDevice;
    auto deviceType = device->PointerDeviceType;
    

       if ( !m_panInUse )   // If no pointer is in this control yet.
    {
       m_panFirstDown = position;                   // Save the location of the initial contact.
       m_panPointerPosition = position;
       m_panPointerID = pointerID;              // Store the id of the pointer using this control.
       m_panInUse = TRUE;
    }
    
}
```

이 처리기를 사용 하 여 현재 **CameraPanController** 인스턴스는 **m \_ panInUse** 를 TRUE로 설정 하 여 카메라 컨트롤러를 활성으로 처리 해야 함을 알 수 있습니다. 이렇게 하면 앱이 **업데이트** 를 호출할 때 현재 위치 데이터를 사용 하 여 뷰포트를 업데이트 합니다.

사용자가 화면에 접촉 하거나 화면을 클릭할 때 카메라 이동에 대 한 기준 값을 설정 했으므로 이제 사용자가 화면을 끌거나 단추를 누른 상태로 마우스를 이동할 때 수행할 작업을 결정 해야 합니다.

**Onpointermoved** 이벤트 처리기는 포인터가 이동 될 때마다 (플레이어에서 화면으로 끄는 모든 틱) 발생 합니다. 응용 프로그램에서 포인터의 현재 위치를 인식 하 고이를 수행 하는 방법에 대해 설명 해야 합니다.

**OnPointerMoved 됨**

```cpp
void CameraPanController::OnPointerMoved(
                                        _In_ CoreWindow ^sender,
                                        _In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    m_panPointerPosition = position;
}
```

마지막으로 플레이어에서 화면 터치를 중지할 때 카메라 팬 동작을 비활성화 해야 합니다. [**Pointerreleased**](/uwp/api/windows.ui.core.corewindow.pointerreleased) 가 실행 될 때 호출 되는 **onpointerreleased**를 사용 하 고, **m \_ panInUse** 을 FALSE로 설정 하 고, 카메라 팬 이동을 해제 하 고, 포인터 ID를 0으로 설정 합니다.

**OnPointerReleased**

```cpp
void CameraPanController::OnPointerReleased(
                                             _In_ CoreWindow ^sender,
                                             _In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    m_panInUse = FALSE;
    m_panPointerID = 0;
}
```

## <a name="initialize-the-touch-controls-and-the-controller-state"></a>터치 컨트롤 및 컨트롤러 상태를 초기화 합니다.


이벤트를 후크하여 카메라 컨트롤러의 모든 기본 상태 필드를 초기화 해 보겠습니다.

**초기**

```cpp
void CameraPanController::Initialize( _In_ CoreWindow^ window )
{

    // Start recieving touch/mouse events.
    window->PointerPressed += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &CameraPanController::OnPointerPressed);

    window->PointerMoved += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &CameraPanController::OnPointerMoved);

    window->PointerReleased += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &CameraPanController::OnPointerReleased);


    // Initialize the state of the controller.
    m_panInUse = FALSE;             
    m_panPointerID = 0;

    //  Initialize this as it is reset on every frame.
    m_panCommand = DirectX::XMFLOAT3( 0.0f, 0.0f, 0.0f );

}
```

**Initialize** 는 응용 프로그램의 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 인스턴스에 대 한 참조를 매개 변수로 사용 하 고, 개발한 이벤트 처리기를 해당 **CoreWindow**의 적절 한 이벤트에 등록 합니다.

## <a name="getting-and-setting-the-position-of-the-camera-controller"></a>카메라 컨트롤러의 위치 가져오기 및 설정


장면 공간에서 카메라 컨트롤러의 위치를 가져오고 설정 하는 몇 가지 메서드를 정의 해 보겠습니다.

```cpp
void CameraPanController::SetPosition( _In_ DirectX::XMFLOAT3 pos )
{
    m_position = pos;
}

// Returns the position of the controller object
DirectX::XMFLOAT3 CameraPanController::get_Position()
{
    return m_position;
}

DirectX::XMFLOAT3 CameraPanController::get_FixedLookPoint()
{
    // For this sample, we don't need to use the trig functions because our
    // look point is fixed. 
    DirectX::XMFLOAT3 result= m_position;
    result.z += 1.0f;
    return result;    

}
```

**Setposition** 은 카메라 컨트롤러 위치를 특정 지점으로 설정 해야 하는 경우 앱에서 호출할 수 있는 공용 메서드입니다.

**get \_ Position** 은 가장 중요 한 공용 속성입니다 .이는 앱이 장면 공간에서 카메라 컨트롤러의 현재 위치를 가져오는 방식입니다. 따라서이에 따라 뷰포트를 업데이트할 수 있습니다.

**get \_ FixedLookPoint** 는이 예제에서 x-y 평면에 대 한 일반적인 연결점을 가져오는 공용 속성입니다. 고정 카메라에 대해 더 많은 오블리크 각도를 만들려는 경우 x, y 및 z 좌표 값을 계산할 때 삼각 함수 sin 및 cos를 사용 하도록이 메서드를 변경할 수 있습니다.

## <a name="updating-the-camera-controller-state-information"></a>카메라 컨트롤러 상태 정보를 업데이트 하는 중


이제 m에서 추적 되는 포인터 좌표 정보를 3D 장면 공간의 새 **좌표 \_ ** 정보로 변환 하는 계산을 수행 합니다. 앱은 기본 앱 루프를 새로 고칠 때마다이 메서드를 호출 합니다. 여기서는 뷰포트에 프로젝션 하기 전에 뷰 매트릭스를 업데이트 하는 데 사용 되는 앱에 전달 하려는 새 위치 정보를 계산 합니다.

```cpp

void CameraPanController::Update( CoreWindow ^window )
{
    if ( m_panInUse )
    {
        pointerDelta.x = m_panPointerPosition.x - m_panFirstDown.x;
        pointerDelta.y = m_panPointerPosition.y - m_panFirstDown.y;

        if ( pointerDelta.x > 16.0f )        // Leave 32 pixel-wide dead spot for being still.
            m_panCommand.x += 1.0f;
        else
            if ( pointerDelta.x < -16.0f )
                m_panCommand.x += -1.0f;

        if ( pointerDelta.y > 16.0f )        
            m_panCommand.y += 1.0f;
        else
            if (pointerDelta.y < -16.0f )
                m_panCommand.y += -1.0f;
    }

       DirectX::XMFLOAT3 command = m_panCommand;
   
    // Our velocity is based on the command.
    DirectX::XMFLOAT3 Velocity;
    Velocity.x =  command.x;
    Velocity.y =  command.y;
    Velocity.z =  0.0f;

    // Integrate
    m_position.x = m_position.x + Velocity.x;
    m_position.y = m_position.y + Velocity.y;
    m_position.z = m_position.z + Velocity.z;

    // Clear the movement input accumulator for use during the next frame.
    m_panCommand = DirectX::XMFLOAT3( 0.0f, 0.0f, 0.0f );

}
```

카메라를 이동 하는 데 터치 또는 마우스 지터를 사용 하 고 싶지 않으므로 jerky가 32 픽셀의 포인터로 비활성 영역을 설정 합니다. 또한 작동 중단 영역을 지난 포인터의 픽셀 순회와 함께 1:1 인 속도 값이 있습니다. 이 동작을 조정 하 여 속도를 늦추거나 이동 속도를 높일 수 있습니다.

## <a name="updating-the-view-matrix-with-the-new-camera-position"></a>새 카메라 위치를 사용 하 여 뷰 매트릭스 업데이트


이제 카메라에 포커스가 있는 장면 공간 좌표를 얻을 수 있으며, 앱에 지시할 때마다 업데이트 됩니다 (예: 기본 앱 루프에서 60 초 마다). 이 의사 코드에서는 구현할 수 있는 호출 동작을 제안 합니다.

```cpp
 myCameraPanController->Update( m_window ); 

 // Update the view matrix based on the camera position.
 myCamera->MyMethodToComputeViewMatrix(
        myController->get_Position(),        // The position in the 3D scene space.
        myController->get_FixedLookPoint(),      // The point in the space we are looking at.
        DirectX::XMFLOAT3( 0, 1, 0 )                    // The axis that is "up" in our space.
        );  
```

축하합니다! 게임에서 터치 컨트롤을 이동 하는 간단한 카메라 집합을 구현 했습니다.


 

 

 