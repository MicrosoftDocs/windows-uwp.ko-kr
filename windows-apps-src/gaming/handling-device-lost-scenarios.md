---
title: Direct3D 11에서 장치 제거 시나리오 처리
description: 이 항목에서는 그래픽 어댑터를 제거 하거나 다시 초기화할 때 Direct3D 및 DXGI 장치 인터페이스 체인을 다시 만드는 방법에 대해 설명 합니다.
ms.assetid: 8f905acd-08f3-ff6f-85a5-aaa99acb389a
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx 11, 장치 손실
ms.localizationpriority: medium
ms.openlocfilehash: 49356b910879f96d607a8bfeb2586b8da0ea3c69
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173157"
---
# <a name="span-iddev_gaminghandling_device-lost_scenariosspanhandle-device-removed-scenarios-in-direct3d-11"></a><span id="dev_gaming.handling_device-lost_scenarios"></span>Direct3D 11에서 장치 제거 시나리오 처리



이 항목에서는 그래픽 어댑터를 제거 하거나 다시 초기화할 때 Direct3D 및 DXGI 장치 인터페이스 체인을 다시 만드는 방법에 대해 설명 합니다.

DirectX 9에서 응용 프로그램은 D3D 장치가 작동 하지 않는 상태로 전환 되는 "[장치 손실](/windows/desktop/direct3d9/lost-devices)" 조건을 발생할 수 있습니다. 예를 들어 전체 화면 Direct3D 9 응용 프로그램이 포커스를 잃으면 Direct3D 장치는 "손실" 됩니다. 그러면 분실 된 장치로 그리기를 시도 해도 자동으로 실패 합니다. Direct3D 11은 가상 그래픽 장치 인터페이스를 사용 하 여 여러 프로그램이 동일한 실제 그래픽 장치를 공유할 수 있도록 하 고 앱이 Direct3D 장치를 제어할 수 없는 상태를 제거 합니다. 그러나 그래픽 어댑터 가용성이 변경 될 수도 있습니다. 예:

-   그래픽 드라이버가 업그레이드 됩니다.
-   시스템이 절전 그래픽 어댑터에서 성능 그래픽 어댑터로 변경 됩니다.
-   그래픽 장치가 응답을 중지 하 고 다시 설정 됩니다.
-   그래픽 어댑터는 실제로 연결 되거나 제거 됩니다.

이러한 상황이 발생 하는 경우 DXGI는 Direct3D 장치를 다시 초기화 해야 하 고 장치 리소스를 다시 만들어야 함을 나타내는 오류 코드를 반환 합니다. 이 연습에서는 Direct3D 11 앱 및 게임이 그래픽 어댑터를 다시 설정, 제거 또는 변경 하는 모든 상황을 감지 하 고 대응 하는 방법을 설명 합니다. Microsoft Visual Studio 2015에 제공 된 DirectX 11 앱 (유니버설 Windows) 템플릿에서 코드 예제가 제공 됩니다.

## <a name="instructions"></a>Instructions

### <a name="spanspanstep-1"></a><span></span>1단계:

렌더링 루프에서 장치 제거 오류에 대 한 검사를 포함 합니다. [**Idxgiswapchain::P 재전송**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) (또는 [**Present1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1)등)을 호출 하 여 프레임을 표시 합니다. 그런 다음 [**dxgi \_ 오류 \_ 장치 \_ 제거 됨**](/windows/desktop/direct3ddxgi/dxgi-error) 또는 **dxgi \_ 오류 \_ 장치 \_ 다시 설정**이 반환 되었는지 확인 합니다.

먼저 템플릿은 DXGI 스왑 체인이 반환한 HRESULT를 저장합니다.

```cpp
HRESULT hr = m_swapChain->Present(1, 0);
```

프레임을 표시 하는 다른 모든 작업을 처리 한 후에 템플릿에서 장치 제거 오류를 확인 합니다. 필요한 경우 메서드를 호출 하 여 장치 제거 조건을 처리 합니다.

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

또한 창 크기 변경에 응답할 때 장치 제거 오류에 대 한 확인을 포함 합니다. 이는 다음과 같은 여러 가지 이유로 [**dxgi \_ 오류 \_ 장치 \_ 제거**](/windows/desktop/direct3ddxgi/dxgi-error) 또는 **dxgi \_ 오류 \_ 장치 \_ 재설정** 을 확인 하는 데 적합 한 위치입니다.

-   스왑 체인의 크기를 조정 하려면 장치 제거 오류를 반환할 수 있는 기본 DXGI 어댑터를 호출 해야 합니다.
-   앱이 다른 그래픽 장치에 연결 된 모니터로 이동 했을 수 있습니다.
-   그래픽 장치를 제거 하거나 다시 설정 하면 데스크톱 해상도가 변경 되어 창 크기가 변경 되는 경우가 많습니다.

이 템플릿은 [**ResizeBuffers**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers)에서 반환 된 HRESULT를 확인 합니다.

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

앱이 [**DXGI \_ 오류 \_ 장치 \_ 제거**](/windows/desktop/direct3ddxgi/dxgi-error) 오류를 수신 하는 경우 언제 든 지 Direct3D 장치를 다시 초기화 하 고 장치 종속 리소스를 다시 만들어야 합니다. 이전 Direct3D 장치를 사용 하 여 만든 그래픽 장치 리소스에 대 한 참조를 해제 합니다. 이러한 리소스는 이제 유효 하지 않으므로 새 항목을 만들려면 먼저 스왑 체인에 대 한 모든 참조를 해제 해야 합니다.

HandleDeviceLost 메서드는 스왑 체인을 해제 하 고 앱 구성 요소에 장치 리소스를 해제 하도록 알립니다.

```cpp
m_swapChain = nullptr;

if (m_deviceNotify != nullptr)
{
    // Notify the renderers that device resources need to be released.
    // This ensures all references to the existing swap chain are released so that a new one can be created.
    m_deviceNotify->OnDeviceLost();
}
```

그런 다음 새 스왑 체인을 만들고 장치 관리 클래스에서 제어 하는 장치 종속 리소스를 다시 초기화 합니다.

```cpp
// Create the new device and swap chain.
CreateDeviceResources();
m_d2dContext->SetDpi(m_dpi, m_dpi);
CreateWindowSizeDependentResources();
```

장치 및 스왑 체인을 다시 설정한 후에는 장치 종속 리소스를 다시 초기화 하도록 앱 구성 요소에 알립니다.

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

HandleDeviceLost 메서드가 종료 되 면 컨트롤은 렌더링 루프로 반환 되어 다음 프레임을 그리기 위해 계속 됩니다.

## <a name="remarks"></a>설명


### <a name="investigating-the-cause-of-device-removed-errors"></a>장치 제거 오류의 원인을 조사 하는 중

DXGI 장치 제거 오류로 인 한 문제 반복은 그래픽 코드가 그리기 루틴 중에 잘못 된 조건을 생성 하 고 있음을 나타낼 수 있습니다. 그래픽 드라이버의 하드웨어 오류 또는 버그를 나타낼 수도 있습니다. 장치 제거 오류의 원인을 조사 하려면 Direct3D 장치를 릴리스하기 전에 [**ID3D11Device:: GetDeviceRemovedReason**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-getdeviceremovedreason) 를 호출 합니다. 이 메서드는 장치 제거 오류에 대 한 이유를 나타내는 6 가지 가능한 DXGI 오류 코드 중 하나를 반환 합니다.

-   **DXGI \_ 오류 \_ 장치가 \_ 정지**됨: 앱에서 보낸 그래픽 명령의 조합이 잘못 되어 그래픽 드라이버가 응답 하지 않습니다. 이 오류가 반복적으로 발생 하는 경우 앱에서 장치를 중지 하 고 디버깅 해야 하는 것일 수 있습니다.
-   **DXGI \_ \_ \_ 제거 된 오류 장치**: 그래픽 장치가 물리적으로 제거 되거나 꺼져 있거나 드라이버 업그레이드가 발생 했습니다. 이는 경우에 따라 정상적으로 발생 합니다. 앱 또는 게임은이 항목에 설명 된 대로 장치 리소스를 다시 만들어야 합니다.
-   **DXGI \_ \_장치 \_ 다시 설정 오류**: 잘못 구성 된 명령으로 인해 그래픽 장치에 오류가 발생 했습니다. 이 오류가 반복적으로 발생 하는 경우 코드에서 잘못 된 그리기 명령을 보내는 것일 수 있습니다.
-   **DXGI \_ 오류 \_ 드라이버 \_ 내부 \_ 오류**: 그래픽 드라이버에서 오류가 발생 하 여 장치를 다시 설정 했습니다.
-   **DXGI \_ 오류 \_ 잘못 된 \_ 호출**: 응용 프로그램에서 잘못 된 매개 변수 데이터를 제공 했습니다. 이 오류가 한 번 이라도 발생 하면 코드에서 장치 제거 조건이 발생 하 여 디버깅 해야 함을 의미 합니다.
-   **S \_ OK**: 그래픽 장치를 사용 하도록 설정 하거나, 사용 하지 않도록 설정 하거나, 현재 그래픽 장치를 무효화 하지 않고 다시 설정 했을 때 반환 됩니다. 예를 들어 응용 프로그램에서 [뒤틀기 (Windows Advanced 래스터화 Platform)](/windows/desktop/direct3darticles/directx-warp) 를 사용 하 고 있고 하드웨어 어댑터를 사용할 수 있게 되 면이 오류 코드가 반환 될 수 있습니다.

다음 코드에서는 [**DXGI \_ 오류 \_ 장치 \_ 제거**](/windows/desktop/direct3ddxgi/dxgi-error) 오류 코드를 검색 하 여 디버그 콘솔에 출력 합니다. HandleDeviceLost 메서드의 시작 부분에 다음 코드를 삽입 합니다.

```cpp
    HRESULT reason = m_d3dDevice->GetDeviceRemovedReason();

#if defined(_DEBUG)
    wchar_t outString[100];
    size_t size = 100;
    swprintf_s(outString, size, L"Device removed! DXGI_ERROR code: 0x%X\n", reason);
    OutputDebugStringW(outString);
#endif
```

자세한 내용은 [**GetDeviceRemovedReason**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-getdeviceremovedreason) 및 [**DXGI \_ ERROR**](/windows/desktop/direct3ddxgi/dxgi-error)를 참조 하세요.

### <a name="testing-device-removed-handling"></a>장치 제거 처리 테스트

Visual Studio의 개발자 명령 프롬프트는 Visual Studio 그래픽 진단와 관련 된 Direct3D 이벤트 캡처 및 재생에 대 한 명령줄 도구 ' dxcap.exe '를 지원 합니다. 앱이 실행 되는 동안 "-forcetdr" 명령줄 옵션을 사용 하 여 GPU 시간 제한 검색 및 복구 이벤트를 강제로 수행 하 여 DXGI \_ 오류 \_ 장치를 \_ 제거 하 고 오류 처리 코드를 테스트할 수 있습니다.

> **참고** Dxcap.exe 및 해당 지원 Dll은 더 이상 Windows SDK를 통해 배포 되지 않는 Windows 10 용 그래픽 도구의 일부로 system32/syswow64에 설치 됩니다. 대신, 선택적 OS 구성 요소인 주문형 그래픽 도구 기능을 통해 제공 되며, Windows 10에서 그래픽 도구를 사용 하도록 설정 하 고 사용 하기 위해 설치 되어야 합니다. Windows 10 용 그래픽 도구를 설치 하는 방법에 대 한 자세한 내용은 다음을 참조 하세요. <https://msdn.microsoft.com/library/mt125501.aspx#InstallGraphicsTools>