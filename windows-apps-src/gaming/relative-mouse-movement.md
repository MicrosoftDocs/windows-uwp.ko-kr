---
title: 상대 마우스 이동
description: 시스템 커서를 사용하지 않고 절대 화면 좌표를 반환하지 않는 상대 마우스 컨트롤을 사용하여 게임에서 마우스 이동 사이의 픽셀 델타를 추적합니다.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 게임, 마우스, 입력
ms.assetid: 08c35e05-2822-4a01-85b8-44edb9b6898f
ms.localizationpriority: medium
ms.openlocfilehash: 71985841e6c0fa764201c179fb12408581823e5e
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8749556"
---
# <a name="relative-mouse-movement-and-corewindow"></a>상대 마우스 이동 및 CoreWindow

게임에서 마우스는 많은 플레이어에게 익숙한 공용 컨트롤 옵션이며 첫 번째 및 세 번째 슈터와 실시간 전략 게임을 포함하여 많은 게임 장르에 필요합니다. 여기서는 시스템 커서를 사용하지 않고 절대 화면 좌표를 반환하지 않는 상대 마우스 컨트롤 구현에 대해 설명합니다. 대신 이 컨트롤은 마우스 이동 사이의 픽셀 델타를 추적합니다.

게임 등의 일부 앱은 마우스를 보다 일반적인 입력 장치로 사용합니다. 예를 들어 3D 모델러는 마우스 입력을 통해 가상 트랙볼을 시뮬레이션하여 3D 개체의 방향을 지정할 수 있습니다. 또는 게임에서 마우스를 사용하여 마우스 모양 컨트롤을 통해 보기 카메라의 방향을 변경할 수 있습니다. 

이 시나리오에서는 앱에 상대 마우스 데이터가 필요합니다. 상대 마우스 값은 창 또는 화면 내의 절대 x-y 좌표 값 대신 마지막 프레임 이후 마우스가 이동한 거리를 나타냅니다. 또한 3D 개체나 장면을 조작하는 경우 화면 좌표 기반의 커서 위치는 관련이 없기 때문에 대체로 마우스 커서를 숨깁니다. 

사용자가 앱을 상대 3D 개체/장면 조작 모드로 이동하는 조치를 취하는 경우 앱에서 다음 작업을 수행해야 합니다. 
- 기본 마우스 처리를 무시합니다.
- 상대 마우스 처리를 사용하도록 설정합니다.
- 마우스 커서를 null 포인터(nullptr)로 설정하여 숨깁니다. 

사용자가 앱을 상대 3D 개체/장면 조작 모드에서 이동하는 조치를 취하는 경우 앱에서 다음 작업을 수행해야 합니다. 
- 기본/절대 마우스 처리를 사용하도록 설정합니다.
- 상대 마우스 처리를 끕니다. 
- 마우스 커서를 null이 아닌 값으로 설정하여 표시합니다.

> **참고**  
이 패턴에서는 커서 없는 상대 모드로 전환할 때 절대 마우스 커서의 위치가 유지됩니다. 커서가 이전과 동일한 화면 좌표 위치에 다시 표시되어 상대 마우스 이동 모드를 사용하도록 설정합니다.

 

## <a name="handling-relative-mouse-movement"></a>상대 마우스 이동 처리


상대 마우스 델타 값에 액세스하려면 여기에 표시된 대로 [MouseDevice::MouseMoved](https://msdn.microsoft.com/library/windows/apps/xaml/windows.devices.input.mousedevice.mousemoved.aspx) 이벤트를 등록합니다.


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

이 코드 예제의 이벤트 처리기인 **OnMouseMoved**는 마우스 이동을 기준으로 뷰를 렌더링합니다. 마우스 포인터의 위치가 처리기에 [MouseEventArgs](https://msdn.microsoft.com/library/windows/apps/xaml/windows.devices.input.mouseeventargs.aspx) 개체로 전달됩니다. 

앱이 상대 마우스 이동 값 처리로 변경되면 [CoreWindow::PointerMoved](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.core.corewindow.pointermoved.aspx) 이벤트에서 절대 마우스 데이터 처리를 건너뜁니다. 하지만 터치 입력과 반대로 마우스 입력의 결과로 **CoreWindow::PointerMoved** 이벤트가 발생한 경우에만 이 입력을 건너뜁니다. [CoreWindow::PointerCursor](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.core.corewindow.pointercursor.aspx)를 **nullptr**로 설정하면 커서가 숨겨집니다. 

## <a name="returning-to-absolute-mouse-movement"></a>절대 마우스 이동으로 되돌리기

앱이 3D 개체 또는 장면 조작 모드를 끝내고 더 이상 상대 마우스 이동을 사용하지 않는 경우(메뉴 화면으로 돌아가는 경우 등) 일반적인 절대 마우스 이동 처리로 돌아갑니다. 이때 상대 마우스 데이터 읽기를 중지하고, 표준 마우스(및 포인터) 이벤트 처리를 다시 시작한 다음 [CoreWindow::PointerCursor](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.core.corewindow.pointercursor.aspx)를 null이 아닌 값으로 설정합니다. 

> **참고**  
앱이 3D 개체/장면 조작 모드(커서를 끄고 상대 마우스 이동 처리)에 있는 경우 마우스에서 참 메뉴, 백 스택 또는 앱 바 같은 에지 UI를 호출할 수 없습니다. 따라서 자주 사용하는 **Esc** 키 등 이 특정 모드를 끝내는 메커니즘을 제공하는 것이 중요합니다.

## <a name="related-topics"></a>관련 항목

* [게임용 이동-보기 컨트롤](tutorial--adding-move-look-controls-to-your-directx-game.md) 
* [게임용 터치 컨트롤](tutorial--adding-touch-controls-to-your-directx-game.md)