---
title: Direct3D 11에서 디바이스 제거 시나리오 처리
description: 이 항목에서는 그래픽 어댑터가 제거되거나 다시 초기화될 때 Direct3D 및 DXGI 디바이스 인터페이스 체인을 다시 만드는 방법에 대해 설명합니다.
ms.assetid: 8f905acd-08f3-ff6f-85a5-aaa99acb389a
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx 11, 장치 분실
ms.localizationpriority: medium
ms.openlocfilehash: c11bbf7657644fbf616590f50d75d93f62ed993e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646608"
---
# <a name="span-iddevgaminghandlingdevice-lostscenariosspanhandle-device-removed-scenarios-in-direct3d-11"></a><span id="dev_gaming.handling_device-lost_scenarios"></span>Direct3D 11에서 제거 하는 장치 시나리오를 처리 합니다.



이 항목에서는 그래픽 어댑터가 제거되거나 다시 초기화될 때 Direct3D 및 DXGI 디바이스 인터페이스 체인을 다시 만드는 방법에 대해 설명합니다.

DirectX 9에서는 D3D 장치가 비작동 상태에 들어서는 경우 응용 프로그램이 "[장치 손실](https://msdn.microsoft.com/library/windows/desktop/bb174714)" 상태에 처할 수 있습니다. 예를 들어 전체 화면 Direct3D 9 응용 프로그램이 초점을 잃으면 Direct3D 장치는 "손실" 상태가 되어, 손실된 장치로 그리려는 모든 시도가 자동으로 실패하게 됩니다. Direct3D 11은 가상 그래픽 인터페이스를 사용하므로, 여러 프로그램이 동일한 물리적 그래픽 장치를 공유할 수 있고 앱이 Direct3D 장치의 제어를 손실하는 상황이 발생하지 않습니다. 그러나 그래픽 어댑터 가용성이 변경될 가능성은 여전히 있습니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

-   그래픽 드라이버가 업그레이드됩니다.
-   절전 그래픽 어댑터에서 성능 그래픽 어댑터로 시스템이 변경됩니다.
-   그래픽 장치가 응답을 멈추고 다시 설정됩니다.
-   그래픽 어댑터가 물리적으로 연결되거나 제거됩니다.

그러한 상황이 발생하면 DXGI는 Direct3D 장치를 다시 초기화하고 장치 리소스를 다시 만들어야 한다고 알리는 오류 코드를 반환합니다. 이 연습에서는 Direct3D 11 앱과 게임이 그래픽 어댑터의 재설정, 제거 또는 변경 상황을 검색하고 이에 대응하는 방법에 대해 설명합니다. Microsoft Visual Studio 2015와 함께 제공 되는 DirectX 11 앱 (유니버설 Windows) 템플릿에서 코드 예제가 제공 됩니다.

## <a name="instructions"></a>지침

### <a name="spanspanstep-1"></a><span></span>1 단계:

디바이스 제거 오류 검사를 렌더링 루프에 포함합니다. [  **IDXGISwapChain::Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576)(또는 [**Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797) 등)를 호출하여 프레임을 표시합니다. 그런 다음 반환 하는지 여부를 확인 [ **DXGI\_오류\_장치\_제거 됨** ](https://msdn.microsoft.com/library/windows/desktop/bb509553) 하거나 **DXGI\_오류\_장치 \_재설정**합니다.

먼저 템플릿은 DXGI 스왑 체인이 반환한 HRESULT를 저장합니다.

```cpp
HRESULT hr = m_swapChain->Present(1, 0);
```

프레임을 표시하기 위한 다른 모든 작업을 처리한 후 템플릿은 장치 제거 오류를 검사합니다. 필요한 경우 장치 제거 상태를 처리하기 위한 메서드를 호출합니다.

```cpp
// If the device was removed either by a disconnection or a driver upgrade, we
// must recreate all device resources.
if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
{
    HandleDeviceLost();
}
else
{
    DX::ThrowIfFailed(hr);
}
```

### <a name="step-2"></a>2단계:

또한 창 크기 변경에 대해 응답할 때 장치 제거 오류에 대한 검사를 포함합니다. 이 확인 하는 것이 좋습니다 [ **DXGI\_오류\_장치\_제거** ](https://msdn.microsoft.com/library/windows/desktop/bb509553) 하거나 **DXGI\_오류\_장치\_ 재설정** 여러 가지 이유로 합니다.

-   스왑 체인의 크기를 변경하려면 기본 DXGI 어댑터에 대한 호출이 필요한데, 이 경우 장치 제거 오류가 반환될 수 있습니다.
-   앱이 다른 그래픽 장치에 연결된 모니터로 이동했을 수 있습니다.
-   그래픽 장치가 제거되거나 재설정되면 데스크톱 해상도가 종종 변경되어, 창 크기가 변경됩니다.

템플릿은 [**ResizeBuffers**](https://msdn.microsoft.com/library/windows/desktop/bb174577)에서 반환된 HRESULT를 확인합니다.

```cpp
// If the swap chain already exists, resize it.
HRESULT hr = m_swapChain->ResizeBuffers(
    2, // Double-buffered swap chain.
    static_cast<UINT>(m_d3dRenderTargetSize.Width),
    static_cast<UINT>(m_d3dRenderTargetSize.Height),
    DXGI_FORMAT_B8G8R8A8_UNORM,
    0
    );

if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
{
    // If the device was removed for any reason, a new device and swap chain will need to be created.
    HandleDeviceLost();

    // Everything is set up now. Do not continue execution of this method. HandleDeviceLost will reenter this method 
    // and correctly set up the new device.
    return;
}
else
{
    DX::ThrowIfFailed(hr);
}
```

### <a name="step-3"></a>3단계:

언제 든 지 앱이 수신 된 [ **DXGI\_오류\_장치\_제거 됨** ](https://msdn.microsoft.com/library/windows/desktop/bb509553) 오류 Direct3D 장치를 다시 초기화 하 고 모든 장치 종속적 다시 리소스입니다. 이전 Direct3D 디바이스로 만든 그래픽 장치 리소스에 대한 모든 참조를 해제합니다. 해당 리소스는 이제 유효하지 않으며, 새 참조를 만들기 전에 스왑 체인에 대한 모든 참조를 해제해야 합니다.

HandleDeviceLost 메서드는 스왑 체인을 해제하고 앱 구성 요소에 장치 리소스를 해제하도록 알립니다.

```cpp
m_swapChain = nullptr;

if (m_deviceNotify != nullptr)
{
    // Notify the renderers that device resources need to be released.
    // This ensures all references to the existing swap chain are released so that a new one can be created.
    m_deviceNotify->OnDeviceLost();
}
```

그런 다음 새 스왑 체인을 만들고, 장치 관리 클래스에서 제어하는 장치 종속 리소스를 다시 초기화합니다.

```cpp
// Create the new device and swap chain.
CreateDeviceResources();
m_d2dContext->SetDpi(m_dpi, m_dpi);
CreateWindowSizeDependentResources();
```

장치와 스왑 체인이 다시 설정되면, 앱 구성 요소에 장치 종속 리소스를 다시 초기화하도록 알립니다.

```cpp
// Create the new device and swap chain.
CreateDeviceResources();
m_d2dContext->SetDpi(m_dpi, m_dpi);
CreateWindowSizeDependentResources();

if (m_deviceNotify != nullptr)
{
    // Notify the renderers that resources can now be created again.
    m_deviceNotify->OnDeviceRestored();
}
```

HandleDeviceLost 메서드가 있으면 렌더링 루프로 컨트롤이 반환되는데, 이는 다음 프레임을 그릴 수 있도록 계속됩니다.

## <a name="remarks"></a>설명


### <a name="investigating-the-cause-of-device-removed-errors"></a>장치 제거 오류의 원인 조사

DXGI 장치 제거 오류가 반복해서 발생하면 그리기 루틴 중에 그래픽 코드가 잘못된 조건을 만들고 있음을 나타내는 것일 수 있습니다. 또한 그래픽 드라이버의 하드웨어 오류 또는 버그를 나타내는 것일 수도 있습니다. 장치 제거 오류의 원인을 조사하려면 Direct3D 장치를 해제하기 전에 [**ID3D11Device::GetDeviceRemovedReason**](https://msdn.microsoft.com/library/windows/desktop/ff476526)을 호출하세요. 이 메서드는 장치 제거 오류의 원인을 나타내는 여섯 가지 가능한 DXGI 오류 코드 중 하나를 반환합니다.

-   **DXGI\_오류\_장치\_여 닫**: 그래픽 드라이버가 잘못 된 조합이 앱에서 전송 된 그래픽 명령으로 인해 응답 하지 않습니다. 이 오류가 반복해서 발생하면 앱이 장치 중단을 일으켰음을 나타내는 것일 수 있으므로 디버깅이 필요합니다.
-   **DXGI\_오류\_장치\_제거**: 그래픽 장치를 물리적으로 제거 되었습니다, 해제 또는 드라이버 업그레이드 발생 했습니다. 이는 가끔 발생하는 정상적인 현상입니다. 이 항목에서 설명한 대로 앱 또는 게임이 장치 리소스를 다시 만듭니다.
-   **DXGI\_오류\_장치\_재설정**: 그래픽 장치를 잘못 구성 된 명령이 실패 했습니다. 이 오류가 반복해서 발생하면 코드가 잘못된 그리기 명령을 보내고 있는 것일 수 있습니다.
-   **DXGI\_오류\_드라이버\_내부\_오류**: 그래픽 드라이버에서 오류가 발생 하는 장치를 초기화 합니다.
-   **DXGI\_오류\_잘못 된\_호출**: 응용 프로그램이 잘못 된 매개 변수 데이터를 제공 합니다. 이 오류가 두 번 이상 발생하면 코드가 장치 제거 상태를 일으켰음을 나타내는 것이므로 디버깅이 필요합니다.
-   **S\_확인**: 그래픽 장치를 활성화, 비활성화 되었거나 현재 그래픽 장치를 무효화 하지 않고 다시 설정 하는 경우 반환 됩니다. 예를 들면 앱이 [WARP(Windows Advanced Rasterization Platform)](https://msdn.microsoft.com/library/windows/desktop/gg615082)를 사용 중이며 하드웨어 어댑터가 사용 가능해질 때 이 오류 코드가 반환될 수 있습니다.

다음 코드를 검색 합니다는 [ **DXGI\_오류\_장치\_제거 됨** ](https://msdn.microsoft.com/library/windows/desktop/bb509553) 오류 코드 및 디버그 콘솔에 인쇄 합니다. 이 코드를 HandleDeviceLost 메서드의 시작 부분에 추가합니다.

```cpp
    HRESULT reason = m_d3dDevice->GetDeviceRemovedReason();

#if defined(_DEBUG)
    wchar_t outString[100];
    size_t size = 100;
    swprintf_s(outString, size, L"Device removed! DXGI_ERROR code: 0x%X\n", reason);
    OutputDebugStringW(outString);
#endif
```

자세한 내용은 참조 하세요. [ **GetDeviceRemovedReason** ](https://msdn.microsoft.com/library/windows/desktop/ff476526) 하 고 [ **DXGI\_오류**](https://msdn.microsoft.com/library/windows/desktop/bb509553)합니다.

### <a name="testing-device-removed-handling"></a>제거된 디바이스 처리 테스트

Visual Studio 개발자의 명령 프롬프트에서 Visual Studio 그래픽 진단과 관련된 Direct3D 이벤트 캡처 및 재생에 대한 명령줄 도구 'dxcap'를 지원합니다. 명령줄 옵션을 사용할 수 있습니다 "-forcetdr" DXGI 트리거 하므로 앱은 GPU 제한 시간 탐지 및 복구 이벤트를 강제로 실행 되는 동안\_오류\_장치\_제거 하 고 오류를 테스트할 수 있습니다 코드를 처리 합니다.

> **참고** DXCap와 해당 지원 DLL이 Windows SDK를 통해 더 이상 배포되지 않는 Windows 10용 그래픽 도구의 일부로 system32/syswow64에 설치됩니다. 대신 선택적 OS 구성 요소인 주문형 그래픽 도구 기능을 통해 제공되며 Windows 10에서 그래픽 도구를 사용하기 위해 설치되어야 합니다. Windows 10 용 그래픽 도구를 설치 하는 방법에 대 한 자세한 내용은 여기에서 찾을 수 있습니다. <https://msdn.microsoft.com/library/mt125501.aspx#InstallGraphicsTools>
