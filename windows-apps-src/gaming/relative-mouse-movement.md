---
title: 상대 마우스 이동
description: 시스템 커서를 사용 하지 않고 절대 화면 좌표를 반환 하지 않는 상대 마우스 컨트롤을 사용 하 여 게임의 마우스 움직임 사이의 픽셀 델타를 추적 합니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 마우스, 입력
ms.assetid: 08c35e05-2822-4a01-85b8-44edb9b6898f
ms.localizationpriority: medium
ms.openlocfilehash: 155fed8e4bc1cb12196fcaaca4b7232b84c19032
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159257"
---
# <a name="relative-mouse-movement-and-corewindow"></a>상대 마우스 이동 및 CoreWindow

게임에서 마우스는 많은 플레이어에 게 익숙한 공용 컨트롤 옵션이 며, 첫 번째 및 세 번째 사용자 shooters 및 실시간 전략 게임을 비롯 한 많은 게임 장르에도 중요 합니다. 여기서는 시스템 커서를 사용 하지 않고 절대 화면 좌표를 반환 하지 않는 상대 마우스 컨트롤의 구현에 대해 설명 합니다. 대신 마우스 움직임 사이의 픽셀 델타를 추적 합니다.

게임과 같은 일부 앱은 보다 일반적인 입력 장치로 마우스를 사용 합니다. 예를 들어 3 차원 모델러는 가상 트랙볼을 시뮬레이션 하 여 3 차원 개체의 방향을 마우스 입력으로 사용할 수 있습니다. 또는 게임에서 마우스를 사용 하 여 마우스 모양 컨트롤을 통해 보기 카메라의 방향을 변경할 수 있습니다. 

이러한 시나리오에서 앱은 상대 마우스 데이터를 필요로 합니다. 상대 마우스 값은 창이 나 화면 내에서 절대 x-y 좌표 값이 아니라 마지막 프레임 이후 마우스를 이동한 거리를 나타냅니다. 또한 3 차원 개체 또는 장면을 조작할 때 화면 좌표에 대 한 커서의 위치는 관련이 없으므로 앱은 종종 마우스 커서를 숨깁니다. 

사용자가 앱을 상대 3 차원 개체/장면 조작 모드로 이동 하는 작업을 수행 하는 경우 앱은 다음을 수행 해야 합니다. 
- 기본 마우스 처리를 무시 합니다.
- 상대 마우스 처리를 사용 합니다.
- Null 포인터 (nullptr)로 설정 하 여 마우스 커서를 숨깁니다. 

사용자가 상대 3 차원 개체/장면 조작 모드에서 앱을 이동 하는 작업을 수행 하는 경우 앱은 다음 작업을 수행 해야 합니다. 
- 기본/절대 마우스 처리를 사용 합니다.
- 상대 마우스 처리를 해제 합니다. 
- 마우스 커서를 null이 아닌 값으로 설정 하면 표시 됩니다.

> **참고**  
이 패턴을 사용 하면 cursorless 상대 모드를 시작할 때 절대 마우스 커서의 위치를 유지할 수 있습니다. 상대 마우스 이동 모드를 사용 하기 위해 이전 처럼 동일한 화면 좌표 위치에 커서가 다시 표시 됩니다.

 

## <a name="handling-relative-mouse-movement"></a>상대 마우스 이동 처리


상대 마우스 델타 값에 액세스 하려면 여기에 표시 된 대로 [Mousedevice:: Mousedevice](/uwp/api/windows.devices.input.mousedevice.mousemoved) 이벤트를 등록 합니다.


```cpp


// register handler for relative mouse movement events
Windows::Devices::Input::MouseDevice::GetForCurrentView()->MouseMoved +=
        ref new TypedEventHandler<MouseDevice^, MouseEventArgs^>(this, &MoveLookController::OnMouseMoved);


```

```cpp


void MoveLookController::OnMouseMoved(
    _In_ Windows::Devices::Input::MouseDevice^ mouseDevice,
    _In_ Windows::Devices::Input::MouseEventArgs^ args
    )
{
    float2 pointerDelta;
    pointerDelta.x = static_cast<float>(args->MouseDelta.X);
    pointerDelta.y = static_cast<float>(args->MouseDelta.Y);

    float2 rotationDelta;
    rotationDelta = pointerDelta * ROTATION_GAIN;   // scale for control sensitivity

    // update our orientation based on the command
    m_pitch -= rotationDelta.y;                     // mouse y increases down, but pitch increases up
    m_yaw   -= rotationDelta.x;                     // yaw defined as CCW around y-axis

    // limit pitch to straight up or straight down
    float limit = (float)(M_PI/2) - 0.01f;
    m_pitch = (float) __max( -limit, m_pitch );
    m_pitch = (float) __min( +limit, m_pitch );

    // keep longitude in useful range by wrapping
    if ( m_yaw >  M_PI )
        m_yaw -= (float)M_PI*2;
    else if ( m_yaw < -M_PI )
        m_yaw += (float)M_PI*2;
}

```

이 코드 예제의 이벤트 처리기 인 **Onmousemoved**는 마우스 움직임을 기반으로 뷰를 렌더링 합니다. 마우스 포인터의 위치는 처리기에 [MouseEventArgs](/uwp/api/Windows.Devices.Input.MouseEventArgs) 개체로 전달 됩니다. 

앱이 상대 마우스 이동 값을 처리 하도록 변경 될 때 [CoreWindow::P ointerMoved](/uwp/api/windows.ui.core.corewindow.pointermoved) 이벤트에서 절대 마우스 데이터의 처리를 건너뜁니다. 그러나 **CoreWindow::P ointerMoved** 이벤트가 마우스 입력의 결과로 발생 하는 경우에만이 입력을 건너뜁니다 (터치 입력과 반대 됨). [CoreWindow::P ointerCursor](/uwp/api/windows.ui.core.corewindow.pointercursor) 를 **nullptr**로 설정 하 여 커서를 숨깁니다. 

## <a name="returning-to-absolute-mouse-movement"></a>절대 마우스 이동으로 돌아가기

앱이 3 차원 개체 또는 장면 조작 모드를 종료 하 고 더 이상 상대 마우스 이동을 사용 하지 않는 경우 (예: 메뉴 화면으로 반환 되는 경우) 절대 마우스 이동의 정상적인 처리로 돌아갑니다. 이번에는 상대 마우스 데이터 읽기를 중지 하 고 표준 마우스 (및 포인터) 이벤트의 처리를 다시 시작 하 고 [CoreWindow::P ointerCursor](/uwp/api/windows.ui.core.corewindow.pointercursor) 를 null이 아닌 값으로 설정 합니다. 

> **참고**  
앱이 3 차원 개체/장면 조작 모드에 있을 때 (커서를 사용 하 여 상대 마우스 움직임 처리) 마우스는 참 메뉴, back stack 또는 앱 표시줄과 같은 가장자리 UI를 호출할 수 없습니다. 따라서 일반적으로 사용 되는 **Esc** 키와 같은 특정 모드를 종료 하는 메커니즘을 제공 하는 것이 중요 합니다.

## <a name="related-topics"></a>관련 항목

* [게임의 이동 컨트롤](tutorial--adding-move-look-controls-to-your-directx-game.md) 
* [게임 터치 컨트롤](tutorial--adding-touch-controls-to-your-directx-game.md)