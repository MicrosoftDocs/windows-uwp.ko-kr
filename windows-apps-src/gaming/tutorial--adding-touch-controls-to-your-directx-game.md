---
author: mtoepke
title: 게임용 터치 컨트롤
description: DirectX를 사용하는 UWP(유니버설 Windows 플랫폼) C++ 게임에 기본 터치 컨트롤을 추가하는 방법을 알아봅니다.
ms.assetid: 9d40e6e4-46a9-97e9-b848-522d61e8e109
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 터치, 컨트롤, directx, 입력
ms.localizationpriority: medium
ms.openlocfilehash: 53c4a91f3ef20c11783796c3ca362f74b3f39adb
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5689068"
---
# <a name="touch-controls-for-games"></a>게임용 터치 컨트롤



DirectX를 사용하는 UWP(유니버설 Windows 플랫폼) C++ 게임에 기본 터치 컨트롤을 추가하는 방법을 알아봅니다. 터치 기반 컨트롤을 추가하여 Direct3D 환경에서 고정 평면 카메라를 이동하는 방법을 보여줍니다. 이러한 Direct3D 환경에서는 손가락이나 스타일러스로 끌면 카메라 관점이 이동합니다.

이러한 컨트롤은 플레이어가 끌어 3D 환경(예: 맵 또는 플레이필드)을 스크롤하거나 해당 환경에서 이동하도록 하는 게임에서 통합할 수 있습니다. 예를 들어 전략 게임이나 퍼즐 게임에서는 플레이어가 왼쪽이나 오른쪽으로 이동하여 화면보다 더 큰 게임 환경을 볼 수 있도록 하는 데 이러한 컨트롤을 사용할 수 있습니다.

> **참고**이 코드는 마우스 기반 이동 컨트롤 에서도 작동 합니다. 포인터 관련 이벤트는 Windows 런타임 API에서 추상화되어 터치 기반이나 마우스 기반 포인터 이벤트를 처리할 수 있습니다.

 

## <a name="objectives"></a>목표


-   DirectX 게임에서 고정 평면 카메라를 이동하는 간단한 터치 끌기 컨트롤을 만듭니다.

## <a name="set-up-the-basic-touch-event-infrastructure"></a>기본 터치 이벤트 인프라를 설정합니다.


이 경우 먼저 기본 컨트롤러 유형인 **CameraPanController**를 정의합니다. 여기서는 컨트롤러를 추상적 개념 및 사용자가 수행할 수 있는 동작 집합으로 정의합니다.

**CameraPanController** 클래스는 카메라 컨트롤러 상태에 대해 정기적으로 새로 고쳐지는 정보 모음이며 앱이 업데이트 루프에서 해당 정보를 가져오는 방법을 제공합니다.

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

이제 카메라 컨트롤러의 상태를 정의하는 헤더와 카메라 컨트롤러 상호 작용을 구현하는 기본 메서드 및 이벤트 처리기를 만들어 보겠습니다.

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

private 필드에는 카메라 컨트롤러의 현재 상태가 포함되어 있습니다. 해당 필드를 검토해 보겠습니다.

-   **m\_position**은 장면 공간에서 카메라의 위치입니다. 이 예에서는 z-좌표 값이 0에 고정되어 있습니다. DirectX::XMFLOAT2를 사용하여 이 값을 나타낼 수 있지만 이 샘플 및 향후 확장성을 위해 DirectX::XMFLOAT3을 사용합니다. **get\_Position** 속성을 통해 이 값을 앱 자체로 전달하면 그에 따라 뷰포트를 업데이트할 수 있습니다.
-   **m\_panInUse**는 이동 작업이 활성 상태인지 여부, 보다 구체적으로 플레이어가 화면을 터치하고 카메라를 이동하고 있는지 여부를 나타내는 부울 값입니다.
-   **m\_panPointerID**는 포인터에 고유한 ID입니다. 샘플에서는 사용되지 않지만 컨트롤러 상태 클래스를 특정 포인터에 연결하는 것이 좋습니다.
-   **m\_panFirstDown**은 카메라 이동 작업 중 플레이어가 처음으로 화면을 터치하거나 마우스를 클릭한 화면상의 점입니다. 나중에 이 값을 사용하여 화면을 터치할 때의 떨림이나 마우스가 약간 흔들리는 경우를 방지하는 데드존을 설정합니다.
-   **m\_panPointerPosition**은 플레이어가 포인터를 현재 이동한 화면상의 위치입니다. 이 필드는 **m\_panFirstDown**을 기준으로 검사하여 플레이어가 이동하고자 하는 방향을 확인하는 데 사용됩니다.
-   **m\_panCommand**는 카메라 컨트롤러에 대해 계산된 최종 명령으로 위쪽, 아래쪽, 왼쪽 또는 오른쪽입니다. x-y 평면에 고정된 카메라로 작업을 하고 있으므로 대신 DirectX::XMFLOAT2 값을 사용할 수 있습니다.

다음과 같은 3가지 이벤트 처리기를 사용하여 카메라 컨트롤러 상태 정보를 업데이트합니다.

-   **OnPointerPressed**는 플레이어가 터치 표면을 손가락으로 누르고 포인터가 해당 누르기 동작의 좌표로 이동할 때 앱에서 호출하는 이벤트 처리기입니다.
-   **OnPointerMoved**는 플레이어가 터치 표면을 손가락으로 살짝 밀 때 앱에서 호출하는 이벤트 처리기입니다. 이 이벤트 처리기는 끌기 경로의 새 좌표로 업데이트됩니다.
-   **OnPointerReleased**는 플레이어가 터치 표면에서 손가락을 뗄 때 앱에서 호출하는 이벤트 처리기입니다.

마지막으로 다음과 같은 메서드 및 속성을 사용하여 카메라 컨트롤러 상태 정보를 초기화하고 액세스하며 업데이트합니다.

-   **Initialize**는 컨트롤을 초기화하고 디스플레이 창을 설명하는 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 개체에 연결하기 위해 앱에서 호출하는 이벤트 처리기입니다.
-   **SetPosition**은 장면 공간에서 컨트롤의 좌표(x, y 및 z)를 설정하기 위해 앱에서 호출하는 메서드입니다. z-좌표는 이 자습서 전체에서 0입니다.
-   **get\_Position**은 장면 공간에서 카메라의 현재 위치를 가져오기 위해 앱에서 액세스하는 속성입니다. 이 속성은 현재 카메라 위치를 앱에 전달하는 방법으로 사용합니다.
-   **get\_FixedLookPoint**는 컨트롤러 카메라가 향하고 있는 현재 지점을 가져오기 위해 앱에서 액세스하는 속성입니다. 이 예에서는 x-y 평면에 직각으로 잠겨 있습니다.
-   **Update**는 컨트롤러 상태를 읽고 카메라 위치를 업데이트하는 메서드입니다. 앱의 주 루프에서 이 &lt;something&gt;을 계속 호출하여 장면 공간에서 카메라 컨트롤러 데이터 및 카메라 위치를 새로 고칩니다.

이제 터치 컨트롤을 구현하는 데 필요한 모든 구성 요소가 있습니다. 터치 또는 마우스 포인터 이벤트가 발생한 시기 및 위치와 수행하는 작업을 파악할 수 있습니다. 장면 공간을 기준으로 카메라의 위치 및 방향을 설정하고 변경 사항을 추적할 수 있습니다. 마지막으로 새 카메라 위치를 호출 앱에 전달할 수 있습니다.

이제 이러한 조각을 함께 연결해 보겠습니다.

## <a name="create-the-basic-touch-events"></a>기본 터치 이벤트 만들기


Windows 런타임 이벤트 디스패처에서는 다음과 같이 앱에서 처리할 3개의 이벤트를 제공합니다.

-   [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278)
-   [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276)
-   [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279)

이러한 이벤트는 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 유형에 구현됩니다. 이때 사용할 **CoreWindow** 개체가 있다고 가정합니다. 자세한 내용은 [DirectX 보기를 표시하도록 UWP C++ 앱을 설정하는 방법](https://msdn.microsoft.com/library/windows/apps/hh465077)을 참조하세요.

이러한 이벤트는 앱이 실행되는 동안 발생하므로 처리기에서 private 필드에 정의된 카메라 컨트롤러 상태 정보를 업데이트합니다.

먼저 터치 포인터 이벤트 처리기를 채워보겠습니다. 첫 번째 이벤트 처리기 **OnPointerPressed**에서는 사용자가 화면을 터치하거나 마우스를 클릭할 때 디스플레이를 관리하는 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)에서 포인터의 x-y 좌표를 가져옵니다.

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

이 처리기를 사용하여 **m\_panInUse**를 TRUE로 설정함으로써 카메라 컨트롤러가 활성 상태로 처리되어야 함을 현재 **CameraPanController** 인스턴스에 알립니다. 이런 식으로 앱에서 **Update**를 호출하면 현재 위치 데이터를 사용하여 뷰포트를 업데이트합니다.

사용자가 화면을 터치하거나 디스플레이 창을 클릭하거나 누를 때의 카메라 이동에 대한 기본 값을 설정했으므로 사용자가 누른 화면을 끌거나 단추를 누른 채로 마우스를 이동할 때 수행할 동작을 결정해야 합니다.

**OnPointerMoved** 이벤트 처리기는 플레이어가 화면에서 끄는 모든 틱에서 포인터가 이동할 때마다 발생합니다. 앱에서 포인터의 현재 위치를 계속 인식하도록 해야 하며 이런 식으로 해당 작업을 수행합니다.

**OnPointerMoved**

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

마지막으로 플레이어가 화면 터치를 중지하면 카메라 이동 동작을 비활성화해야 합니다. [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279)가 발생할 때 호출되는 **OnPointerReleased**를 사용하여 **m\_panInUse**를 FALSE로 설정하고 카메라 이동 동작을 해제한 다음 포인터 ID를 0으로 설정합니다.

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

## <a name="initialize-the-touch-controls-and-the-controller-state"></a>터치 컨트롤 및 컨트롤러 상태 초기화


이벤트를 연결하고 카메라 컨트롤러의 모든 기본 상태 필드를 초기화해 보겠습니다.

**Initialize**

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

**Initialize**에서는 앱의 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 인스턴스에 대한 참조를 매개 변수로 사용하고 개발한 인벤트 처리기를 해당 **CoreWindow**의 적합한 이벤트에 등록합니다.

## <a name="getting-and-setting-the-position-of-the-camera-controller"></a>카메라 컨트롤러의 위치 가져오기 및 설정


장면 공간에서 카메라 컨트롤러의 위치를 가져오고 설정하는 몇 가지 메서드를 정의해 보겠습니다.

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

**SetPosition**은 카메라 컨트롤러 위치를 특정 점으로 설정해야 하는 경우 앱에서 호출할 수 있는 public 메서드입니다.

**get\_Position**은 가장 중요한 public 속성이며, 앱이 장면 공간에서 카메라 컨트롤러의 현재 위치를 가져와 그에 따라 뷰포트를 업데이트할 수 방법입니다.

**get\_FixedLookPoint**는 이 예제에서 x-y 평면에 직각인 보기 지점을 가져오는 public 속성입니다. 고정된 카메라에 대해 더 큰 기울기 각도를 만들려는 경우 x, y 및 z 좌표 값을 계산하려면 이 메서드를 변경하여 삼각 함수 사인과 코사인을 사용할 수 있습니다.

## <a name="updating-the-camera-controller-state-information"></a>카메라 컨트롤러 상태 정보 업데이트


이제 **m\_panPointerPosition**에서 추적한 포인터 좌표 정보를 3D 장면 공간과 관련된 새 좌표 정보로 변환하는 계산을 수행합니다. 앱에서는 메인 앱 루프를 새로 고칠 때마다 이 메서드를 호출합니다. 따라서 뷰포트로 투영하기 전에 보기 매트릭스를 업데이트하는 데 사용되는 앱에 전달할 새로운 위치 정보를 여기에서 계산해야 합니다.

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

터치나 마우스 떨림으로 인해 카메라가 갑자기 이동하지 않도록 설정하려고 하므로 포인터 주위에 32픽셀 지름으로 데드존을 설정합니다. 속도 값도 있으며, 이 경우는 데드존을 지나는 포인터의 픽셀 통과를 사용하는 1:1입니다. 이 동작을 조정하여 이동 속도를 낮추거나 높일 수 있습니다.

## <a name="updating-the-view-matrix-with-the-new-camera-position"></a>새로운 카메라 위치로 보기 매트릭스 업데이트


이제 카메라가 초점을 맞춘 장면 공간 좌표를 가져올 수 있으며 이 좌표는 가져오도록 앱에 알릴 때마다(예: 주 앱 루프에서 60초마다) 업데이트됩니다. 다음 의사 코드는 구현할 수 있는 호출 동작을 제안합니다.

```cpp
 myCameraPanController->Update( m_window ); 

 // Update the view matrix based on the camera position.
 myCamera->MyMethodToComputeViewMatrix(
        myController->get_Position(),        // The position in the 3D scene space.
        myController->get_FixedLookPoint(),      // The point in the space we are looking at.
        DirectX::XMFLOAT3( 0, 1, 0 )                    // The axis that is "up" in our space.
        );  
```

축하합니다! 게임에서 간단한 카메라 이동 터치 컨트롤 집합을 구현했습니다.


 

 

 




