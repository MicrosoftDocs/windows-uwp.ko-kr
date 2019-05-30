---
title: Marble Maze 샘플에 시각적 콘텐츠 추가
description: 이 문서에서는 패턴을 학습하고 고유한 게임 콘텐츠로 작업할 때 적절하게 조정할 수 있도록 Marble Maze 게임이 UWP(유니버설 Windows 플랫폼) 앱 환경에서 Direct3D 및 Direct2D를 사용하는 방법을 설명합니다.
ms.assetid: 6e43422e-e1a1-b79e-2c4b-7d5b4fa88647
ms.date: 09/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 샘플, directx, 그래픽
ms.localizationpriority: medium
ms.openlocfilehash: ce62e065170349523062fbd42d867edfed63f47c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369080"
---
# <a name="adding-visual-content-to-the-marble-maze-sample"></a>Marble Maze 샘플에 시각적 콘텐츠 추가




이 문서에서는 패턴을 학습하고 고유한 게임 콘텐츠로 작업할 때 적절하게 조정할 수 있도록 Marble Maze 게임이 UWP(유니버설 Windows 플랫폼) 앱 환경에서 Direct3D 및 Direct2D를 사용하는 방법을 설명합니다. 시각적 게임 구성 요소가 Marble Maze의 전체 응용 프로그램 구조에 맞게 조정되는 방식에 대한 자세한 내용은 [Marble Maze 응용 프로그램 구조](marble-maze-application-structure.md)를 참조하세요.

Marble Maze의 시각적 측면을 개발할 때 다음과 같은 기본 단계를 따랐습니다.

1.  Direct3D 및 Direct2D 환경을 초기화하는 기본 프레임워크를 만듭니다.
2.  게임에 표시 되는 2D 및 3D 자산을 디자인 하려면 이미지 및 프로그램을 편집 하는 모델을 사용 합니다.
3.  2D 및 3D 자산 올바르게 로드 하는 게임에 표시를 확인 합니다.
4.  게임 자산의 시각적 품질을 향상시키는 꼭짓점 및 픽셀 셰이더를 통합합니다.
5.  애니메이션, 사용자 입력 등의 게임 논리를 통합합니다.

또한 집중 했습니다 먼저 3D 자산을 추가 하는 방법 및 2D 자산. 예를 들어 메뉴 시스템과 타이머를 추가하기 전에 핵심 게임 논리에 집중했습니다.

또한 개발 과정에서 이러한 단계 중 일부를 여러 번 반복해야 했습니다. 예를 들어, marble 모델과 메시를 변경한 것으로 이러한 모델을 지 원하는 셰이더 코드의 일부를 변경 했습니다.

> [!NOTE]
> 이 문서에 해당하는 샘플 코드는 [DirectX Marble Maze 게임 샘플](https://go.microsoft.com/fwlink/?LinkId=624011)에 있습니다.

 
DirectX 및 시각적 게임 콘텐츠 작업을 하는 경우, 즉 DirectX 그래픽 라이브러리를 초기화하고, 장면 리소스를 로드하고, 장면을 업데이트 및 렌더링하는 경우에 대해 이 문서에서 논의하는 주요 사항은 다음과 같습니다.

-   일반적으로 게임 콘텐츠 추가는 여러 단계로 이루어집니다. 이러한 단계를 반복해야 할 수도 있습니다. 게임 개발자에 게 종종 집중할 먼저 3D 게임 콘텐츠를 추가 하는 방법에 다음 2D 콘텐츠를 추가 합니다.
-   가능한 한 광범위한 그래픽 하드웨어를 지원하여 더 많은 고객에 도달하고 모든 고객에게 효율적인 환경을 제공합니다.
-   디자인 타임 형식과 런타임 형식을 명확하게 구분합니다. 유연성을 최대화하고 콘텐츠를 신속하게 반복할 수 있도록 디자인 타임 자산을 구성합니다. 런타임에 가능한 한 효율적으로 로드 및 렌더링되도록 자산 형식을 지정하고 압축합니다.
-   클래식 Windows 데스크톱 앱과 거의 유사한 방식으로 UWP 앱에서 Direct3D 및 Direct2D 장치를 만듭니다. 중요한 차이점 중 하나는 스왑 체인이 출력 창과 연결되는 방식입니다.
-   게임을 디자인할 때 선택하는 메시 형식이 주요 시나리오를 지원하는지 확인합니다. 예를 들어 게임에 충돌이 필요한 경우 메시에서 충돌 데이터를 가져올 수 있는지 확인합니다.
-   렌더링하기 전에 모든 장면 개체를 먼저 업데이트하여 렌더링 논리에서 게임 논리를 분리합니다.
-   일반적으로 3D 장면의 개체를 그리고 이면 모든 2D 장면 앞에 표시 되는 개체입니다.
-   수직 소거에 그리기를 동기화하여 게임이 실제로 디스플레이에 표시되지 않는 프레임을 그리는 데 시간을 소비하지 않도록 합니다. A *세로 빈* 한 프레임 모니터에 대 한 그리기를 완료 하는 때 사이의 시간 이며 다음 프레임을 시작 합니다.

## <a name="getting-started-with-directx-graphics"></a>DirectX 그래픽으로 시작


당사가 하려고 계획할 지 Marble Maze 유니버설 Windows 플랫폼 (UWP) 게임을 하는 경우 C++ 및 Direct3D 11.1는 3D 게임을 만들기 위한 선택 항목 렌더링 및 높은 성능 최대 제어 해야 뛰어난 때문입니다. DirectX 11.1은 DirectX 9에서 DirectX 11 사이의 하드웨어를 지원하며 이전 DirectX 버전에 대해 각각 코드를 다시 작성하지 않아도 되므로 보다 효율적으로 더 많은 고객에 도달하는 데 도움이 됩니다.

Marble Maze 게임 3D 자산을 namely를 marble 및 미로 렌더링할 Direct3D 11.1을 사용 합니다. Marble Maze는 또한 메뉴 등 타이머 2D 게임 자산을 그릴 Direct2D, DirectWrite, 및 Windows Imaging 구성 요소 (WIC)를 사용 합니다.

게임 개발에는 계획이 필요합니다. DirectX 그래픽을 처음 접하는 경우 참조 하는 것이 좋습니다 [DirectX: 시작](directx-getting-started.md) UWP DirectX 게임을 만들기의 기본 개념을 잘 알고 있습니다. 이 문서를 읽고 Marble Maze 소스 코드를 통해 작동 하는 대로 DirectX 그래픽에 대 한 자세한 대 한 자세한 내용은 다음 리소스를 참조할 수 있습니다.

-   [Direct3D 11 그래픽](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11): Direct3D 11, 강력 하 고 하드웨어 가속 3D 그래픽 API Windows 플랫폼에서 기 하 도형 3D 렌더링에 대해 설명 합니다.
-   [Direct2D](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-portal): Direct2D를 설명 하는 하드웨어 가속, 2D 그래픽 API 및 제공 하는 고성능 고품질 렌더링 2D 기 하 도형, 비트맵 및 텍스트에 대 한 합니다.
-   [DirectWrite](https://docs.microsoft.com/windows/desktop/DirectWrite/direct-write-portal): DirectWrite를 고품질 텍스트 렌더링이 지를 설명 합니다.
-   [Windows Imaging Component](https://docs.microsoft.com/windows/desktop/wic/-wic-lh): WIC를 디지털 이미지에 대 한 하위 수준 API를 제공 하는 확장 가능한 플랫폼을 설명 합니다.

### <a name="feature-levels"></a>기능 수준

Direct3D 11 라는 패러다임을 소개 *기능 수준*합니다. 기능 수준은 잘 정의된 GPU 기능 집합입니다. 기능 수준을 사용하여 이전 버전의 Direct3D 하드웨어에서 실행되도록 게임 대상을 지정합니다. Marble Maze는 상위 수준의 고급 기능이 필요하지 않으므로 기능 수준 9.1을 지원합니다. 가능한 최대 범위의 하드웨어를 지원하고 고급 또는 저급 컴퓨터를 가진 고객이 모두 효율적인 환경을 사용할 수 있도록 게임 콘텐츠를 확장하는 것이 좋습니다. 기능 수준에 대한 자세한 내용은 [하위 수준 하드웨어의 Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel)을 참조하세요.

## <a name="initializing-direct3d-and-direct2d"></a>Direct3D 및 Direct2D 초기화


장치는 디스플레이 어댑터를 나타냅니다. 클래식 Windows 데스크톱 앱과 거의 유사한 방식으로 UWP 앱에서 Direct3D 및 Direct2D 장치를 만듭니다. 주요 차이점은 Direct3D 스왑 체인을 창 시스템에 연결하는 방법입니다.

**DeviceResources** 클래스는 Direct3D 및 Direct2D 관리의 토대가 됩니다. 이 클래스는 일반 인프라, 게임와 관련이 없을 자산을 처리합니다. Marble Maze 정의 **MarbleMazeMain** 클래스 게임 관련 자산을 처리 하는 대 한 참조가에 **DeviceResources** Direct3D 및 Direct2D에 액세스할 수 있는 개체입니다.

초기화 하는 동안 합니다 **DeviceResources** 생성자 장치 독립적인 리소스 및 Direct3D 및 Direct2D 장치를 만듭니다.

```cpp
// Initialize the Direct3D resources required to run. 
DX::DeviceResources::DeviceResources() :
    m_screenViewport(),
    m_d3dFeatureLevel(D3D_FEATURE_LEVEL_9_1),
    m_d3dRenderTargetSize(),
    m_outputSize(),
    m_logicalSize(),
    m_nativeOrientation(DisplayOrientations::None),
    m_currentOrientation(DisplayOrientations::None),
    m_dpi(-1.0f),
    m_deviceNotify(nullptr)
{
    CreateDeviceIndependentResources();
    CreateDeviceResources();
}
```

**DeviceResources** 클래스는 환경이 변경될 때 더 쉽게 응답할 수 있도록 이 기능을 분리합니다. 예를 들어 창 크기가 변경될 때 **CreateWindowSizeDependentResources** 메서드를 호출합니다.

###  <a name="initializing-the-direct2d-directwrite-and-wic-factories"></a>Direct2D, DirectWrite 및 WIC 팩터리 초기화

**DeviceResources::CreateDeviceIndependentResources** 메서드는 Direct2D, DirectWrite 및 WIC용 팩터리를 만듭니다. DirectX 그래픽에서 팩터리는 그래픽 리소스를 만들기 위한 시작점입니다. Marble Maze 지정 **D2D1\_팩터리\_형식\_SINGLE\_계층 구조로** 주 스레드에서 모든 드로잉을 수행 합니다.

```cpp
// These are the resources required independent of hardware. 
void DX::DeviceResources::CreateDeviceIndependentResources()
{
    // Initialize Direct2D resources.
    D2D1_FACTORY_OPTIONS options;
    ZeroMemory(&options, sizeof(D2D1_FACTORY_OPTIONS));

#if defined(_DEBUG)
    // If the project is in a debug build, enable Direct2D debugging via SDK Layers.
    options.debugLevel = D2D1_DEBUG_LEVEL_INFORMATION;
#endif

    // Initialize the Direct2D Factory.
    DX::ThrowIfFailed(
        D2D1CreateFactory(
            D2D1_FACTORY_TYPE_SINGLE_THREADED,
            __uuidof(ID2D1Factory2),
            &options,
            &m_d2dFactory
            )
        );

    // Initialize the DirectWrite Factory.
    DX::ThrowIfFailed(
        DWriteCreateFactory(
            DWRITE_FACTORY_TYPE_SHARED,
            __uuidof(IDWriteFactory2),
            &m_dwriteFactory
            )
        );

    // Initialize the Windows Imaging Component (WIC) Factory.
    DX::ThrowIfFailed(
        CoCreateInstance(
            CLSID_WICImagingFactory2,
            nullptr,
            CLSCTX_INPROC_SERVER,
            IID_PPV_ARGS(&m_wicFactory)
            )
        );
}
```

###  <a name="creating-the-direct3d-and-direct2d-devices"></a>Direct3D 및 Direct2D 장치 만들기

합니다 **DeviceResources::CreateDeviceResources** 메서드 호출 [D3D11CreateDevice](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) Direct3D 디스플레이 어댑터를 나타내는 장치 개체를 만들려고 합니다. Marble Maze 기능 수준 9.1을 지원 하므로 이상 합니다 **DeviceResources::CreateDeviceResources** 메서드 수준 9.1 11.1 통해 지정 합니다 **featureLevels** 배열입니다. Direct3D는 목록을 순서대로 검색하고 사용 가능한 첫 번째 기능 수준을 앱에 제공합니다. 따라서 합니다 **D3D\_기능\_수준** 배열 항목 나열 되어 높은 것부터 가장 낮은 앱에서 사용할 수 있는 가장 높은 기능 수준을 받게 됩니다. **DeviceResources::CreateDeviceResources** 메서드는 **D3D11CreateDevice**에서 반환된 Direct3D 11 장치를 쿼리하여 Direct3D 11.1 장치를 가져옵니다.

```cpp
// This flag adds support for surfaces with a different color channel ordering
// than the API default. It is required for compatibility with Direct2D.
UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
    if (DX::SdkLayersAvailable())
    {
        // If the project is in a debug build, enable debugging via SDK Layers 
        // with this flag.
        creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
    }
#endif

// This array defines the set of DirectX hardware feature levels this app will support.
// Note the ordering should be preserved.
// Don't forget to declare your application's minimum required feature level in its
// description.  All applications are assumed to support 9.1 unless otherwise stated.
D3D_FEATURE_LEVEL featureLevels[] =
{
    D3D_FEATURE_LEVEL_11_1,
    D3D_FEATURE_LEVEL_11_0,
    D3D_FEATURE_LEVEL_10_1,
    D3D_FEATURE_LEVEL_10_0,
    D3D_FEATURE_LEVEL_9_3,
    D3D_FEATURE_LEVEL_9_2,
    D3D_FEATURE_LEVEL_9_1
};

// Create the Direct3D 11 API device object and a corresponding context.
ComPtr<ID3D11Device> device;
ComPtr<ID3D11DeviceContext> context;

HRESULT hr = D3D11CreateDevice(
    nullptr,                    // Specify nullptr to use the default adapter.
    D3D_DRIVER_TYPE_HARDWARE,   // Create a device using the hardware graphics driver.
    0,                          // Should be 0 unless the driver is D3D_DRIVER_TYPE_SOFTWARE.
    creationFlags,              // Set debug and Direct2D compatibility flags.
    featureLevels,              // List of feature levels this app can support.
    ARRAYSIZE(featureLevels),   // Size of the list above.
    D3D11_SDK_VERSION,          // Always set this to D3D11_SDK_VERSION for UWP apps.
    &device,                    // Returns the Direct3D device created.
    &m_d3dFeatureLevel,         // Returns feature level of device created.
    &context                    // Returns the device immediate context.
    );

if (FAILED(hr))
{
    // If the initialization fails, fall back to the WARP device.
    // For more information on WARP, see:
    // https://go.microsoft.com/fwlink/?LinkId=286690
    DX::ThrowIfFailed(
        D3D11CreateDevice(
            nullptr,
            D3D_DRIVER_TYPE_WARP, // Create a WARP device instead of a hardware device.
            0,
            creationFlags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &device,
            &m_d3dFeatureLevel,
            &context
            )
        );
}

// Store pointers to the Direct3D 11.1 API device and immediate context.
DX::ThrowIfFailed(
    device.As(&m_d3dDevice)
    );

DX::ThrowIfFailed(
    context.As(&m_d3dContext)
    );
```

그런 다음 **DeviceResources::CreateDeviceResources** 메서드는 Direct2D 장치를 만듭니다. Direct2D는 Microsoft DXGI(DirectX Graphics Infrastructure)를 사용하여 Direct3D와 상호 작용합니다. DXGI를 사용하면 그래픽 런타임 간에 비디오 메모리 표면을 공유할 수 있습니다. Marble Maze는 Direct3D 장치의 기본 DXGI 장치를 사용하여 Direct2D 팩터리에서 Direct2D 장치를 만듭니다.

```cpp
// Create the Direct2D device object and a corresponding context.
ComPtr<IDXGIDevice3> dxgiDevice;
DX::ThrowIfFailed(
    m_d3dDevice.As(&dxgiDevice)
    );

DX::ThrowIfFailed(
    m_d2dFactory->CreateDevice(dxgiDevice.Get(), &m_d2dDevice)
    );

DX::ThrowIfFailed(
    m_d2dDevice->CreateDeviceContext(
        D2D1_DEVICE_CONTEXT_OPTIONS_NONE,
        &m_d2dContext
        )
    );
```

DXGI 및 Direct2D와 Direct3D 간의 상호 운용성에 대한 자세한 내용은 [DXGI 개요](https://docs.microsoft.com/windows/desktop/direct3ddxgi/d3d10-graphics-programming-guide-dxgi) 및 [Direct2D 및 Direct3D 상호 운용성 개요](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-and-direct3d-interoperation-overview)를 참조하세요.

### <a name="associating-direct3d-with-the-view"></a>Direct3D를 뷰에 연결

**DeviceResources::CreateWindowSizeDependentResources** 메서드는 스왑 체인, Direct3D 및 Direct2D 렌더링 대상 등 지정된 창 크기에 종속된 그래픽 리소스를 만듭니다. DirectX UWP 앱과 데스크톱 앱의 중요한 차이점 중 하나는 스왑 체인이 출력 창에 연결되는 방식입니다. 스왑 체인은 장치가 렌더링되는 버퍼를 모니터에 표시합니다. [Marble Maze 응용 프로그램 구조](marble-maze-application-structure.md) windowing 시스템 UWP 앱 용 데스크톱 앱에서 어떻게 다른 지에 대해 설명 합니다. UWP 앱을 사용 하 여 작동 하지 않으므로 [HWND](https://docs.microsoft.com/windows/desktop/WinProg/windows-data-types) 개체, Marble Maze를 사용 해야 합니다 [IDXGIFactory2::CreateSwapChainForCoreWindow](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow) 보기로 장치 출력을 연결 하는 방법. 다음 예제에서는 스왑 체인을 만드는 **DeviceResources::CreateWindowSizeDependentResources** 메서드의 일부를 보여 줍니다.

```cpp
// Obtain the final swap chain for this window from the DXGI factory.
DX::ThrowIfFailed(
    dxgiFactory->CreateSwapChainForCoreWindow(
        m_d3dDevice.Get(),
        reinterpret_cast<IUnknown*>(m_window.Get()),
        &swapChainDesc,
        nullptr,
        &m_swapChain
        )
    );
```

랩톱, 태블릿 등 배터리를 사용 하는 장치에서 작업을 수행 하는 중요 한 전원 소비를 최소화 하는 **DeviceResources::CreateWindowSizeDependentResources** 메서드 호출을 [IDXGIDevice1:: SetMaximumFrameLatency](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgidevice1-setmaximumframelatency) 세로 빈 해야만 게임이 렌더링 됨을 확인 하는 방법입니다. 섹션에 자세히 설명 되어 세로 빈 값을 사용 하 여 동기화 [장면 제공](#presenting-the-scene) 이 문서의.

```cpp
// Ensure that DXGI does not queue more than one frame at a time. This both 
// reduces latency and ensures that the application will only render after each
// VSync, minimizing power consumption.
DX::ThrowIfFailed(
    dxgiDevice->SetMaximumFrameLatency(1)
    );
```

**DeviceResources::CreateWindowSizeDependentResources** 메서드는 대부분의 게임에서 작동하는 방식으로 그래픽 리소스를 초기화합니다.

> [!NOTE]
> 용어 *보기* Direct3D에 보다 Windows 런타임에서 다른 의미를 갖습니다. Windows 런타임에서 뷰는 표시 영역, 입력 동작, 처리에 사용하는 스레드를 비롯하여 앱의 사용자 인터페이스 설정 컬렉션을 가리킵니다. 뷰를 만들 때 필요한 구성과 설정을 지정합니다. 앱 뷰를 설정하는 프로세스는 [Marble Maze 응용 프로그램 구조](marble-maze-application-structure.md)에서 설명합니다.
> Direct3D에서 뷰 용어에는 여러 가지 의미가 있습니다. 리소스 보기 리소스에 액세스할 수 있는 하위를 정의 합니다. 예를 들어 텍스처 개체가 셰이더 리소스 뷰와 연결된 경우 나중에 해당 셰이더가 텍스처에 액세스할 수 있습니다. 리소스 뷰의 장점 중 하나는 렌더링 파이프라인의 각 단계에서 서로 다른 방식으로 데이터를 해석할 수 있다는 것입니다. 리소스 보기에 대 한 자세한 내용은 참조 하세요. [리소스 뷰](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-intro)합니다.
> 뷰 변형 또는 뷰 변형 행렬의 컨텍스트에서 사용되는 경우 뷰는 카메라의 위치와 방향을 가리킵니다. 뷰 변형은 카메라의 위치 및 방향을 중심으로 개체 위치를 옮깁니다. 뷰 변형에 대한 자세한 내용은 [뷰 변형(Direct3D 9)](https://docs.microsoft.com/windows/desktop/direct3d9/view-transform)을 참조하세요. 이 항목에서는 Marble Maze가 리소스 및 행렬 뷰를 사용하는 방식에 대해 자세히 설명합니다.

 

## <a name="loading-scene-resources"></a>장면 리소스 로드


Marble Maze 사용 합니다 **BasicLoader** 에서 선언 된 클래스 **BasicLoader.h**, 질감 및 셰이더를 로드 하려면. Marble Maze 사용 합니다 **SDKMesh** 미로 marble 한 메시 3D를 로드 하는 클래스입니다.

반응형 앱이 되기 위해 Marble Maze는 비동기적으로 또는 백그라운드에서 장면 리소스를 로드합니다. 자산이 백그라운드에서 로드될 때 게임이 창 이벤트에 응답할 수 있습니다. 이 프로세스는 이 가이드의 [백그라운드에서 게임 자산 로드](marble-maze-application-structure.md#loading-game-assets-in-the-background)에서 자세히 설명합니다.

###  <a name="loading-the-2d-overlay-and-user-interface"></a>2D 오버레이 및 사용자 인터페이스를 로드합니다.

Marble Maze에서 오버레이는 화면 맨 위에 표시되는 이미지입니다. 오버레이는 항상 장면 앞에 표시됩니다. Marble Maze에 오버레이 Windows 로고 및 텍스트 문자열을 포함 **DirectX Marble Maze 게임 샘플**합니다. 오버레이 관리에서 수행 되는 **SampleOverlay** 클래스에 정의 되어 있는 **SampleOverlay.h**합니다. Direct3D 샘플에 포함된 오버레이를 사용하지만 이 코드를 조정하여 임의 이미지를 장면 앞에 표시할 수 있습니다.

중요 한 측면을 오버레이의 이므로, 해당 콘텐츠를 변경 하지 않는 합니다 **SampleOverlay** 그립니다, 또는 캐시, 해당 내용을 클래스는 [ID2D1Bitmap1](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1bitmap1) 초기화 중에 개체를 합니다. 그릴 때 **SampleOverlay** 클래스는 비트맵만 화면에 그리면 됩니다. 이렇게 하면 텍스트 그리기 등의 값비싼 루틴을 모든 프레임에 대해 수행하지 않아도 됩니다.

사용자 인터페이스 (UI) 메뉴 등 장면 앞에 나타나는 헤드업 표시 (HUDs) 2D 구성 요소로 이루어져 있습니다. Marble Maze는 다음 UI 요소를 정의합니다.

-   사용자가 게임을 시작하거나 최고 점수를 볼 수 있는 메뉴 항목
-   플레이가 시작되기 전에 3초 동안 카운트 다운하는 타이머
-   경과된 플레이 시간을 추적하는 타이머
-   가장 빠른 완료 시간을 나열하는 테이블
-   텍스트를 읽는 **일시 중지 됨** 게임이 일시 중지 되었습니다.

Marble Maze 게임-특정 UI 요소를 정의 **UserInterface.h**합니다. Marble Maze는 **ElementBase** 클래스를 모든 UI 요소의 기본 형식으로 정의합니다. **ElementBase** 클래스는 UI 요소의 크기, 위치, 맞춤, 표시 여부 등의 속성을 정의합니다. 또한 요소를 업데이트하고 렌더링하는 방법을 제어합니다.

```cpp
class ElementBase
{
public:
    virtual void Initialize() { }
    virtual void Update(float timeTotal, float timeDelta) { }
    virtual void Render() { }

    void SetAlignment(AlignType horizontal, AlignType vertical);
    virtual void SetContainer(const D2D1_RECT_F& container);
    void SetVisible(bool visible);

    D2D1_RECT_F GetBounds();

    bool IsVisible() const { return m_visible; }

protected:
    ElementBase();

    virtual void CalculateSize() { }

    Alignment       m_alignment;
    D2D1_RECT_F     m_container;
    D2D1_SIZE_F     m_size;
    bool            m_visible;
};
```

UI 요소에 대한 공용 기본 클래스를 제공할 경우 사용자 인터페이스를 관리하는 **UserInterface** 클래스가 **ElementBase** 개체 컬렉션만 저장하면 되기 때문에 UI 관리가 간소화되고 재사용 가능한 사용자 인터페이스 관리자가 제공됩니다. Marble Maze는 **ElementBase**에서 파생되고 게임 관련 동작을 구현하는 형식을 정의합니다. 예를 들어 **HighScoreTable**은 최소 점수 테이블의 동작을 정의합니다. 이러한 형식에 대한 자세한 내용은 소스 코드를 참조하세요.

> [!NOTE]
> XAML을 사용 하면 시뮬레이션 및 전략 게임에서 복잡 한 사용자 인터페이스를 쉽게 작성할 수 때문에 XAML을 사용 하 여 UI를 정의할 것인지 고려 합니다. DirectX UWP 게임에서 XAML의 사용자 인터페이스를 개발 하는 방법에 대 한 정보를 참조 하세요 [게임 샘플 확장](tutorial-resources.md), DirectX 3D 게임 샘플 해결를 참조 하는 합니다.

 

###  <a name="loading-shaders"></a>로딩 셰이더

Marble Maze는 **BasicLoader::LoadShader** 메서드를 사용하여 파일에서 셰이더를 로드합니다.

셰이더는 최신 게임에서 GPU 프로그래밍의 기본 단위입니다. 거의 모든 3D 그래픽 처리는 셰이더, 모델 변환 및 조명, 장면 인지 또는 더 복잡 한 기 하 도형 공간 분할에 문자 스킨에서 처리를 통해 구동 됩니다. 셰이더 프로그래밍 모델에 대한 자세한 내용은 [HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl)을 참조하세요.

Marble Maze는 꼭짓점 및 픽셀 셰이더를 사용합니다. 꼭짓점 셰이더는 항상 하나의 입력 꼭짓점에서 작동하며 출력으로 하나의 꼭짓점을 생성합니다. 픽셀 셰이더는 숫자 값, 텍스처 데이터, 보간된 꼭짓점별 값 및 기타 데이터를 사용하여 출력으로 픽셀 색상을 생성합니다. 셰이더는 한 번에 하나의 요소를 변형하기 때문에 여러 셰이더 파이프라인을 제공하는 그래픽 하드웨어는 요소 집합을 병렬로 처리할 수 있습니다. GPU에서 사용할 수 있는 병렬 파이프라인 수가 CPU에서 사용할 수 있는 개수보다 훨씬 많을 수 있습니다. 따라서 기본 셰이더도 처리량을 훨씬 향상시킬 수 있습니다.

합니다 **MarbleMazeMain::LoadDeferredResources** 오버레이 로드 된 후 한 꼭 짓 점 셰이더 및 하나의 픽셀 셰이더만 메서드 로드 합니다. 이 셰이더 디자인 타임 버전에 정의 된 **BasicVertexShader.hlsl** 하 고 **BasicPixelShader.hlsl**, 각각. Marble Maze는 렌더링 단계에서 이러한 셰이더를 구슬과 미로에 적용합니다.

Marble Maze 프로젝트에는 .hlsl(디자인 타임 형식) 및 .cso(런타임 형식) 버전의 셰이더 파일이 둘 다 포함되어 있습니다. 빌드할 때 Visual Studio는 fxc.exe 효과 컴파일러를 사용하여 .hlsl 소스 파일을 .cso 이진 셰이더로 컴파일합니다. 효과 컴파일러 도구에 대한 자세한 내용은 [효과 컴파일러 도구](https://docs.microsoft.com/windows/desktop/direct3dtools/fxc)를 참조하세요.

꼭짓점 셰이더는 제공된 모델, 뷰 및 프로젝션 행렬을 사용하여 입력 기하 도형을 변형합니다. 입력 기하 도형의 위치 데이터가 변형되고 두 번 출력됩니다. 한 번은 화면 공간에서 출력되며 렌더링에 필요하고, 다른 한 번은 월드 공간에서 출력되어 픽셀 셰이더가 조명 계산을 수행할 수 있게 합니다. 표면 법선 벡터도 월드 공간으로 변형되어 픽셀 셰이더에 의해 조명에 사용됩니다. 텍스처 좌표는 변경되지 않고 픽셀 셰이더에 전달됩니다.

```hlsl
sPSInput main(sVSInput input)
{
    sPSInput output;
    float4 temp = float4(input.pos, 1.0f);
    temp = mul(temp, model);
    output.worldPos = temp.xyz / temp.w;
    temp = mul(temp, view);
    temp = mul(temp, projection);
    output.pos = temp;
    output.tex = input.tex;
    output.norm = mul(float4(input.norm, 0.0f), model).xyz;
    return output;
}
```

픽셀 셰이더는 꼭짓점 셰이더의 출력을 입력으로 받습니다. 이 셰이더는 조명 계산을 수행하여 미로를 비추고 구슬 위치에 맞춰지는 가장자리가 부드러운 스포트라이트를 모방합니다. 광원을 정면으로 향하는 표면에 대한 조명이 가장 강합니다. 표면 법선이 광원에 수직이 될수록 확산 구성 요소가 0으로 가늘어지고, 법선이 광원에서 멀어질수록 주변 항이 줄어듭니다. 지점이 구슬에 가까울수록(따라서 스포트라이트 중심에 가까울수록) 조명이 더 강해집니다. 그러나 부드러운 그림자를 시뮬레이트하기 위해 구슬 아래 지점에 대한 조명은 조절됩니다. 실제 환경에서 흰색 구슬 등의 개체는 장면의 다른 개체에 스포트라이트를 반사합니다. 구슬의 밝은 절반 뷰에 있는 표면에 대해 근사값이 계산됩니다. 추가 조명 요소는 구슬에 대한 상대 각도와 거리에 있습니다. 결과 픽셀 색상은 조명 계산의 결과를 사용하여 샘플링된 텍스처의 컴퍼지션입니다.

```hlsl
float4 main(sPSInput input) : SV_TARGET
{
    float3 lightDirection = float3(0, 0, -1);
    float3 ambientColor = float3(0.43, 0.31, 0.24);
    float3 lightColor = 1 - ambientColor;
    float spotRadius = 50;

    // Basic ambient (Ka) and diffuse (Kd) lighting from above.
    float3 N = normalize(input.norm);
    float NdotL = dot(N, lightDirection);
    float Ka = saturate(NdotL + 1);
    float Kd = saturate(NdotL);

    // Spotlight.
    float3 vec = input.worldPos - marblePosition;
    float dist2D = sqrt(dot(vec.xy, vec.xy));
    Kd = Kd * saturate(spotRadius / dist2D);

    // Shadowing from ball.
    if (input.worldPos.z > marblePosition.z)
        Kd = Kd * saturate(dist2D / (marbleRadius * 1.5));

    // Diffuse reflection of light off ball.
    float dist3D = sqrt(dot(vec, vec));
    float3 V = normalize(vec);
    Kd += saturate(dot(-V, N)) * saturate(dot(V, lightDirection))
        * saturate(marbleRadius / dist3D);

    // Final composite.
    float4 diffuseTexture = Texture.Sample(Sampler, input.tex);
    float3 color = diffuseTexture.rgb * ((ambientColor * Ka) + (lightColor * Kd));
    return float4(color * lightStrength, diffuseTexture.a);
}
```

> [!WARNING]
> 컴파일된 픽셀 셰이더 32 산술 지침 및 1 개 질감 명령을 포함합니다. 이 셰이더는 데스크톱 컴퓨터 및 고급 태블릿에서도 제대로 작동해야 합니다. 그러나 이 셰이더를 처리할 수 없는 저급 컴퓨터도 대화형 프레임 속도를 제공할 수 있습니다. 대상 사용자의 일반적인 하드웨어를 고려하고 해당 하드웨어의 기능에 맞게 셰이더를 디자인합니다.

 

합니다 **MarbleMazeMain::LoadDeferredResources** 메서드는 **BasicLoader::LoadShader** 셰이더를 로드 하는 방법입니다. 다음 예제에서는 꼭짓점 셰이더를 로드합니다. 이 셰이더에 대 한 런타임 형식은 **BasicVertexShader.cso**합니다. 합니다 **m\_vertexShader** 멤버 변수가 [ID3D11VertexShader](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11vertexshader) 개체입니다.

```cpp
BasicLoader^ loader = ref new BasicLoader(m_deviceResources->GetD3DDevice());

D3D11_INPUT_ELEMENT_DESC layoutDesc [] =
{
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "NORMAL", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT, 0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "TANGENT", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 32, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};
m_vertexStride = 44; // must set this to match the size of layoutDesc above

Platform::String^ vertexShaderName = L"BasicVertexShader.cso";
loader->LoadShader(
    vertexShaderName,
    layoutDesc,
    ARRAYSIZE(layoutDesc),
    &m_vertexShader,
    &m_inputLayout
    );
```

합니다 **m\_inputLayout** 멤버 변수가 [ID3D11InputLayout](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11inputlayout) 개체입니다. 입력 레이아웃 개체는 IA(입력 어셈블러) 단계의 입력 상태를 캡슐화합니다. IA 단계의 작업 중 하나는 시스템에서 생성된 값(*의미 체계*라고도 함)을 사용하여 아직 처리되지 않은 기본 요소 또는 꼭짓점만 처리하도록 하여 셰이더를 더 효율적으로 만드는 것입니다.

사용 된 [ID3D11Device::CreateInputLayout](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createinputlayout) 입력 요소 설명의 배열에서 입력 레이아웃을 만드는 방법. 배열에는 하나 이상의 입력 요소가 포함됩니다. 각 입력 요소는 단일 꼭짓점 버퍼의 꼭짓점 데이터 요소 1개에 대해 설명합니다. 전체 입력 요소 설명 집합은 IA 단계에 바인딩할 모든 꼭짓점 버퍼의 꼭짓점 데이터 요소를 모두 설명합니다. 

**layoutDesc** 위의 코드 조각 레이아웃 설명을 사용 한다는 사실을 보여줍니다 Marble Maze 합니다. 레이아웃 설명은 꼭짓점 데이터 요소 4개가 포함된 꼭짓점 버퍼를 설명합니다. 배열에 포함된 각 항목의 중요한 부분은 의미 체계 이름, 데이터 형식 및 바이트 오프셋입니다. 예를 들어 **POSITION** 요소는 개체 공간의 꼭짓점 위치를 지정합니다. 바이트 오프셋 0에서 시작하고 부동 소수점 구성 요소 3개(총 12바이트)를 포함합니다. **NORMAL** 요소는 법선 벡터를 지정합니다. 레이아웃에서 **POSITION** 바로 뒤에 표시되며 12바이트가 필요하기 때문에 바이트 오프셋 12에서 시작됩니다. **NORMAL** 요소는 4개 구성 요소로 이루어진 32비트 부호 없는 정수를 포함합니다.

다음 예제와 같이 꼭짓점 셰이더에서 정의된 **sVSInput** 구조와 입력 레이아웃을 비교합니다. **sVSInput** 구조는 **POSITION**, **NORMAL** 및 **TEXCOORD0** 요소를 정의합니다. DirectX 런타임은 레이아웃의 각 요소를 셰이더에서 정의된 입력 구조에 매핑합니다.

```hlsl
struct sVSInput
{
    float3 pos : POSITION;
    float3 norm : NORMAL;
    float2 tex : TEXCOORD0;
};

struct sPSInput
{
    float4 pos : SV_POSITION;
    float3 norm : NORMAL;
    float2 tex : TEXCOORD0;
    float3 worldPos : TEXCOORD1;
};

sPSInput main(sVSInput input)
{
    sPSInput output;
    float4 temp = float4(input.pos, 1.0f);
    temp = mul(temp, model);
    output.worldPos = temp.xyz / temp.w;
    temp = mul(temp, view);
    temp = mul(temp, projection);
    output.pos = temp;
    output.tex = input.tex;
    output.norm = mul(float4(input.norm, 0.0f), model).xyz;
    return output;
}
```

[의미 체계](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics) 문서에서는 사용 가능한 각 의미 체계를 자세히 설명합니다.

> [!NOTE]
> 레이아웃으로 동일한 레이아웃을 공유 하는 여러 셰이더를 사용 하도록 설정 하려면 사용 되지 않는 추가 구성 요소를 지정할 수 있습니다. 예를 들어 **TANGENT** 요소는 셰이더에서 사용되지 않습니다. 일반 매핑 등의 기술을 사용하려는 경우 **TANGENT** 요소를 사용할 수 있습니다. 일반 매핑(범프 매핑이라고도 함)을 사용하여 개체 표면에 범프 효과를 만들 수 있습니다. 범프 매핑에 대한 자세한 내용은 [범프 매핑(Direct3D 9)](https://docs.microsoft.com/windows/desktop/direct3d9/bump-mapping)을 참조하세요.

 

입력된 어셈블리 단계에 대 한 자세한 내용은 참조 하세요. [입력 어셈블러](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage) 하 고 [Getting Started with 입력 어셈블러 단계](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage-getting-started)합니다.

꼭짓점 및 픽셀 셰이더를 사용하여 장면을 렌더링하는 프로세스는 이 문서의 뒷부분에 있는 [장면 렌더링](#rendering-the-scene) 섹션에서 설명합니다.

### <a name="creating-the-constant-buffer"></a>상수 버퍼 만들기

Direct3D 버퍼는 데이터 컬렉션을 그룹화합니다. 상수 버퍼는 데이터를 셰이더에 전달하는 데 사용할 수 있는 버퍼의 한 종류입니다. Marble Maze는 상수 버퍼를 사용하여 모델(또는 월드) 뷰와 활성 장면 개체에 대한 프로젝션 행렬을 저장합니다.

다음 예제와 방법을 **MarbleMazeMain::LoadDeferredResources** 메서드는 나중에 행렬 데이터를 보유 하는 상수 버퍼를 만듭니다. 만듭니다는 **D3D11\_버퍼\_DESC** 사용 하는 구조는 **D3D11\_바인딩\_상수\_버퍼** 플래그입니다. 상수 버퍼 사용을 지정 합니다. 그런 다음 구조를 [ID3D11Device::CreateBuffer](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) 메서드에 전달합니다. 합니다 **m\_constantBuffer** 변수가 [ID3D11Buffer](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11buffer) 개체입니다.

```cpp
// Create the constant buffer for updating model and camera data.
D3D11_BUFFER_DESC constantBufferDesc = {0};

// Multiple of 16 bytes
constantBufferDesc.ByteWidth = ((sizeof(ConstantBuffer) + 15) / 16) * 16;

constantBufferDesc.Usage               = D3D11_USAGE_DEFAULT;
constantBufferDesc.BindFlags           = D3D11_BIND_CONSTANT_BUFFER;
constantBufferDesc.CPUAccessFlags      = 0;
constantBufferDesc.MiscFlags           = 0;

// This will not be used as a structured buffer, so this parameter is ignored.
constantBufferDesc.StructureByteStride = 0;

DX::ThrowIfFailed(
    m_deviceResources->GetD3DDevice()->CreateBuffer(
        &constantBufferDesc,
        nullptr,    // leave the buffer uninitialized
        &m_constantBuffer
        )
    );
```

합니다 **MarbleMazeMain::Update** 메서드는 나중에 업데이트 **ConstantBuffer** 미로 및는 marble에 대 한 개체입니다. 합니다 **MarbleMazeMain::Render** 메서드는 다음 각 바인딩합니다 **ConstantBuffer** 각 개체를 렌더링 하기 전에 상수 버퍼 개체입니다. 다음 예제는 **ConstantBuffer** 구조에 있는 **MarbleMazeMain.h**합니다.

```cpp
// Describes the constant buffer that draws the meshes.
struct ConstantBuffer
{
    XMFLOAT4X4 model;
    XMFLOAT4X4 view;
    XMFLOAT4X4 projection;

    XMFLOAT3 marblePosition;
    float marbleRadius;
    float lightStrength;
};
```

상수 지도 버퍼링 하는 방법을 더 잘 이해 하려면 셰이더 코드를 비교 합니다 **ConstantBuffer** 구조 **MarbleMazeMain.h** 에 **ConstantBuffer** 상수 버퍼 꼭 짓 점 셰이더는 정의한 **BasicVertexShader.hlsl**:

```hlsl
cbuffer ConstantBuffer : register(b0)
{
    matrix model;
    matrix view;
    matrix projection;
    float3 marblePosition;
    float marbleRadius;
    float lightStrength;
};
```

**ConstantBuffer** 구조의 레이아웃은 **cbuffer** 개체와 일치합니다. **cbuffer** 변수는 레지스터 b0을 지정하며, 이는 상수 버퍼 데이터가 레지스터 0에 저장됨을 의미합니다. 합니다 **MarbleMazeMain::Render** 메서드 지정 상수 버퍼를 활성화 하는 경우 0을 등록 합니다. 이 프로세스는 이 문서의 뒷부분에서 자세히 설명합니다.

상수 버퍼에 대한 자세한 내용은 [Direct3D 11의 버퍼 소개](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro)를 참조하세요. register 키워드에 대한 자세한 내용은 [register](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-variable-register)를 참조하세요.

###  <a name="loading-meshes"></a>로딩 메시

Marble Maze는 SDK 메시를 런타임 형식으로 사용합니다. 이 형식은 샘플 응용 프로그램에 대한 메시 데이터를 로드하는 기본 방법을 제공하기 때문입니다. 프로덕션 사용의 경우 게임의 특정 요구 사항을 충족하는 메시 형식을 사용해야 합니다.

합니다 **MarbleMazeMain::LoadDeferredResources** 메서드 로드 꼭 짓 점 및 픽셀 셰이더를 로드 한 후 데이터를 메시입니다. 메시는 대체로 위치, 법선 데이터, 색상, 재질, 텍스처 좌표 등의 정보를 포함하는 꼭짓점 데이터 컬렉션입니다. 메시는 일반적으로 소프트웨어를 제작 하는 3d에서 생성 되 고 응용 프로그램 코드에서 분리 된 파일에서 유지 관리 합니다. 구슬과 미로는 게임에 사용되는 메시의 두 가지 예입니다.

Marble Maze는 **SDKMesh** 클래스를 사용하여 메시를 관리합니다. 이 클래스에서 선언 된 **SDKMesh.h**합니다. **SDKMesh**에서는 메시 데이터를 로드, 렌더링 및 삭제하는 메서드를 제공합니다.

> [!IMPORTANT]
> Marble Maze SDK 메시 형식을 사용 하 고 제공 된 **SDKMesh** 목적 으로만 클래스입니다. SDK 메시 형식은 프로토타입을 만들고 학습하는 데 유용하지만 매우 기본적인 형식이므로 대부분의 게임 개발에 필요한 요구 사항을 충족할 수 없습니다. 게임의 특정 요구 사항을 충족하는 메시 형식을 사용하는 것이 좋습니다.

 

다음 예제와 방법을 **MarbleMazeMain::LoadDeferredResources** 메서드는 **SDKMesh::Create** 공을 미로 대 한 데이터를 메시 로드 하는 방법입니다.

```cpp
// Load the meshes.
DX::ThrowIfFailed(
    m_mazeMesh.Create(
        m_deviceResources->GetD3DDevice(),
        L"Media\\Models\\maze1.sdkmesh",
        false
        )
    );

DX::ThrowIfFailed(
    m_marbleMesh.Create(
        m_deviceResources->GetD3DDevice(),
        L"Media\\Models\\marble2.sdkmesh",
        false
        )
    );
```

###  <a name="loading-collision-data"></a>충돌 데이터 로드

이 섹션에서는 Marble Maze가 구슬과 미로 간의 물리학 시뮬레이션을 구현하는 방식에 집중하지 않지만 메시를 로드할 때 물리학 시스템에 대한 메시 기하 도형을 읽습니다.

```cpp
// Extract mesh geometry for the physics system.
DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_walls",
        m_collision.m_wallTriList
        )
    );

DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_Floor",
        m_collision.m_groundTriList
        )
    );

DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_floorSides",
        m_collision.m_floorTriList
        )
    );

m_physics.SetCollision(&m_collision);
float radius = m_marbleMesh.GetMeshBoundingBoxExtents(0).x / 2;
m_physics.SetRadius(radius);
```

충돌 데이터는 주로 로드 하는 방식으로 사용 하는 런타임 형식에 따라 달라 집니다. Marble Maze SDK 메시 파일에서 충돌 기 하 도형에 로드 하는 방법에 대 한 자세한 내용은 참조는 **MarbleMazeMain::ExtractTrianglesFromMesh** 소스 코드에서 메서드.

## <a name="updating-game-state"></a>게임 상태 업데이트


Marble Maze는 렌더링하기 전에 모든 장면 개체를 먼저 업데이트하여 렌더링 논리에서 게임 논리를 분리합니다.

[Marble Maze 응용 프로그램 구조](marble-maze-application-structure.md) 기본 게임 루프에 설명 합니다. 게임 루프에 포함된 장면 업데이트는 Windows 이벤트 및 입력이 처리된 후, 장면이 렌더링되기 전에 발생합니다. 합니다 **MarbleMazeMain::Update** UI 및 게임의 업데이트를 처리 합니다.

### <a name="updating-the-user-interface"></a>사용자 인터페이스 업데이트

합니다 **MarbleMazeMain::Update** 메서드 호출을 **UserInterface::Update** UI의 상태를 업데이트 하는 방법입니다.

```cpp
UserInterface::GetInstance().Update(
    static_cast<float>(m_timer.GetTotalSeconds()), 
    static_cast<float>(m_timer.GetElapsedSeconds()));
```

**UserInterface::Update** 메서드는 UI 컬렉션의 각 요소를 업데이트합니다.

```cpp
void UserInterface::Update(float timeTotal, float timeDelta)
{
    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        (*iter)->Update(timeTotal, timeDelta);
    }
}
```

파생 된 클래스 **ElementBase** (에 정의 된 **UserInterface.h**)를 구현 합니다 **업데이트** 특정 동작을 수행 하는 방법입니다. 예를 들어 **StopwatchTimer::Update** 메서드는 경과 시간을 제공된 양만큼 업데이트하고 나중에 표시되는 텍스트를 업데이트합니다.

```cpp
void StopwatchTimer::Update(float timeTotal, float timeDelta)
{
    if (m_active)
    {
        m_elapsedTime += timeDelta;

        WCHAR buffer[16];
        GetFormattedTime(buffer);
        SetText(buffer);
    }

    TextElement::Update(timeTotal, timeDelta);
}
```

###  <a name="updating-the-scene"></a>장면 업데이트

**MarbleMazeMain::Update** 메서드는 상태 시스템의 현재 상태에 따라 게임 업데이트 (합니다 **GameState**저장 된 **m_gameState**). 게임 활성 상태인 경우 (**GameState::InGameActive**), Marble Maze는 marble 수행 하려면 카메라 업데이트 상수 버퍼 보기 행렬 부분을 업데이트 하 고에서 물리학 시뮬레이션을 업데이트 합니다.

다음 예제와 방법을 **MarbleMazeMain::Update** 메서드 카메라의 위치를 업데이트 합니다. Marble Maze 사용 합니다 **m\_resetCamera** 변수 카메라는 marble 바로 위에 있는 수를 다시 설정 해야 하는 플래그를 합니다. 게임이 시작되거나 구슬이 미로에서 떨어지면 카메라가 다시 설정됩니다. 주 메뉴 또는 최고 점수 표시 화면이 활성화된 경우 카메라가 일정한 위치에 설정됩니다. 그렇지 않으면 Marble Maze는 *timeDelta* 매개 변수를 사용하여 현재 위치와 대상 위치 사이에서 카메라 위치를 보간합니다. 대상 위치는 구슬 앞에서 약간 위에 있습니다. 경과된 프레임 시간을 사용하면 카메라가 구슬을 점점 따라가거나 추적할 수 있습니다.

```cpp
static float eyeDistance = 200.0f;
static XMFLOAT3A eyePosition = XMFLOAT3A(0, 0, 0);

// Gradually move the camera above the marble.
XMFLOAT3A targetEyePosition;
XMStoreFloat3A(
    &targetEyePosition, 
    XMLoadFloat3A(&marblePosition) - (XMLoadFloat3A(&g) * eyeDistance));

if (m_resetCamera)
{
    eyePosition = targetEyePosition;
    m_resetCamera = false;
}
else
{
    XMStoreFloat3A(
        &eyePosition, 
        XMLoadFloat3A(&eyePosition) 
            + ((XMLoadFloat3A(&targetEyePosition) - XMLoadFloat3A(&eyePosition)) 
                * min(1, static_cast<float>(m_timer.GetElapsedSeconds()) * 8)
            )
    );
}

// Look at the marble. 
if ((m_gameState == GameState::MainMenu) || (m_gameState == GameState::HighScoreDisplay))
{
    // Override camera position for menus.
    XMStoreFloat3A(
        &eyePosition, 
        XMLoadFloat3A(&marblePosition) + XMVectorSet(75.0f, -150.0f, -75.0f, 0.0f));

    m_camera->SetViewParameters(
        eyePosition, 
        marblePosition, 
        XMFLOAT3(0.0f, 0.0f, -1.0f));
}
else
{
    m_camera->SetViewParameters(eyePosition, marblePosition, XMFLOAT3(0.0f, 1.0f, 0.0f));
}
```

다음 예제와 방법을 **MarbleMazeMain::Update** 메서드는 marble 및 미로 상수 버퍼를 업데이트 합니다. 미로의 모델 또는 월드 행렬은 항상 항등 행렬로 유지됩니다. 요소가 모두 1인 주 대각을 제외하고 항등 행렬은 0으로 구성된 정방 행렬입니다. 구슬의 모델 행렬은 위치 행렬에 회전 행렬을 곱한 값을 기반으로 합니다.

```cpp
// Update the model matrices based on the simulation.
XMStoreFloat4x4(&m_mazeConstantBufferData.model, XMMatrixIdentity());

XMStoreFloat4x4(
    &m_marbleConstantBufferData.model, 
    XMMatrixTranspose(
        XMMatrixMultiply(
            marbleRotationMatrix, 
            XMMatrixTranslationFromVector(XMLoadFloat3A(&marblePosition))
        )
    )
);

// Update the view matrix based on the camera.
XMFLOAT4X4 view;
m_camera->GetViewMatrix(&view);
m_mazeConstantBufferData.view = view;
m_marbleConstantBufferData.view = view;
```

하는 방법에 대 한 내용은 **MarbleMazeMain::Update** 참조 메서드 사용자 입력을 읽고 여 marble의 동작을 시뮬레이션 하세요 [추가 입력 및 상호 작용 Marble Maze 샘플에](adding-input-and-interactivity-to-the-marble-maze-sample.md)입니다.

## <a name="rendering-the-scene"></a>장면 렌더링


장면을 렌더링하면 일반적으로 다음 단계가 포함됩니다.

1.  현재 렌더링 대상 깊이 스텐실 버퍼를 설정합니다.
2.  렌더링 및 스텐실 뷰의 선택을 취소합니다.
3.  그리기를 위해 꼭짓점 및 픽셀 셰이더를 준비합니다.
4.  3D 장면의 개체를 렌더링 합니다.
5.  장면 앞에 표시 하려는 모든 2D 개체를 렌더링 합니다.
6.  렌더링된 이미지를 모니터에 표시합니다.

합니다 **MarbleMazeMain::Render** 메서드 렌더링 대상에 바인딩하고 깊이 스텐실 뷰, 해당 뷰를 지우고, 장면 그립니다 및 오버레이 그립니다.

###  <a name="preparing-the-render-targets"></a>렌더링 대상 준비

장면을 렌더링하기 전에 현재 렌더링 대상 깊이 스텐실 버퍼를 설정해야 합니다. 장면이 화면의 모든 픽셀 위에 그려진다고 보장할 수 없는 경우 렌더링 및 스텐실 뷰의 선택도 취소합니다. Marble Maze는 모든 프레임에서 렌더링 및 스텐실 뷰의 선택을 취소하여 이전 프레임의 아티팩트가 표시되지 않도록 합니다.

다음 예제와 방법을 **MarbleMazeMain::Render** 메서드 호출을 [ID3D11DeviceContext::OMSetRenderTargets](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) 현재 렌더링 대상과 깊이 스텐실 버퍼를 설정 하는 방법 것입니다.

```cpp
auto context = m_deviceResources->GetD3DDeviceContext();

// Reset the viewport to target the whole screen.
auto viewport = m_deviceResources->GetScreenViewport();
context->RSSetViewports(1, &viewport);

// Reset render targets to the screen.
ID3D11RenderTargetView *const targets[1] = 
    { m_deviceResources->GetBackBufferRenderTargetView() };

context->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());

// Clear the back buffer and depth stencil view.
context->ClearRenderTargetView(
    m_deviceResources->GetBackBufferRenderTargetView(), 
    DirectX::Colors::Black);

context->ClearDepthStencilView(
    m_deviceResources->GetDepthStencilView(), 
    D3D11_CLEAR_DEPTH | D3D11_CLEAR_STENCIL, 
    1.0f, 
    0);
```

합니다 [ID3D11RenderTargetView](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) 하 고 [ID3D11DepthStencilView](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilview) 인터페이스 Direct3D 10 이상에 제공 되는 텍스처 보기 메커니즘을 지원 합니다. 텍스처 뷰에 대한 자세한 내용은 [텍스처 뷰(Direct3D 10)](https://docs.microsoft.com/windows/desktop/direct3d10/d3d10-graphics-programming-guide-resources-access-views)를 참조하세요. 합니다 [OMSetRenderTargets](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) 메서드는 Direct3D 파이프라인의 출력 병합기 단계를 준비 합니다. 출력 병합 단계에 대한 자세한 내용은 [출력 병합 단계](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage)를 참조하세요.

### <a name="preparing-the-vertex-and-pixel-shaders"></a>꼭짓점 및 픽셀 셰이더 준비

장면 개체를 렌더링하기 전에 다음 단계를 수행하여 그리기를 위해 꼭짓점 및 픽셀 셰이더를 준비합니다.

1.  셰이더 입력 레이아웃을 현재 레이아웃으로 설정합니다.
2.  꼭짓점 및 픽셀 셰이더를 현재 셰이더로 설정합니다.
3.  상수 버퍼를 셰이더에 전달해야 하는 데이터로 업데이트합니다.

> [!IMPORTANT]
> Marble Maze 모든 3D 개체의 꼭 짓 점 및 픽셀 셰이더 한 쌍을 사용합니다. 게임에 두 쌍 이상의 셰이더가 사용되는 경우 다른 셰이더를 사용하는 개체를 그릴 때마다 다음 단계를 수행해야 합니다. 셰이더 상태 변경과 연결된 오버헤드를 줄이기 위해 동일한 셰이더를 사용하는 모든 개체에 대한 렌더링 호출을 그룹화하는 것이 좋습니다.

 

이 문서의 [셰이더 로드](#loading-shaders) 섹션에서는 꼭짓점 셰이더를 만들 때 입력 레이아웃이 생성되는 방식에 대해 설명합니다. 다음 예제와 방법을 **MarbleMazeMain::Render** 메서드는 [ID3D11DeviceContext::IASetInputLayout](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout) 이 레이아웃을 현재 레이아웃으로 설정 하는 방법입니다.

```cpp
m_deviceResources->GetD3DDeviceContext()->IASetInputLayout(m_inputLayout.Get());
```

다음 예제와 방법을 **MarbleMazeMain::Render** 메서드는 [ID3D11DeviceContext::VSSetShader](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader) 하 고 [ID3D11DeviceContext::PSSetShader](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader) 꼭 짓 점 및 픽셀 셰이더를 현재 셰이더를 각각 설정 하는 메서드.

```cpp
// Set the vertex shader stage state.
m_deviceResources->GetD3DDeviceContext()->VSSetShader(
    m_vertexShader.Get(),   // use this vertex shader
    nullptr,                // don't use shader linkage
    0);                     // don't use shader linkage

m_deviceResources->GetD3DDeviceContext()->PSSetShader(
    m_pixelShader.Get(),    // use this pixel shader
    nullptr,                // don't use shader linkage
    0);                     // don't use shader linkage

m_deviceResources->GetD3DDeviceContext()->PSSetSamplers(
    0,                          // starting at the first sampler slot
    1,                          // set one sampler binding
    m_sampler.GetAddressOf());  // to use this sampler
```

후 **MarbleMazeMain::Render** 설정 하는 셰이더 및 입력된 레이아웃을 사용 하 여 합니다 [ID3D11DeviceContext::UpdateSubresource](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-updatesubresource) 모델, 뷰를 사용 하 여 상수 버퍼를 업데이트 하는 방법 및 미로 대 한 투영 행렬입니다. **UpdateSubresource** 메서드는 CPU 메모리에서 GPU 메모리로 행렬 데이터를 복사합니다. 이전에 설명한 대로 모델과 뷰 구성 요소를 **ConstantBuffer** 구조에서 업데이트 되는 **MarbleMazeMain::Update** 메서드. 합니다 **MarbleMazeMain::Render** 메서드를 호출 합니다 [ID3D11DeviceContext::VSSetConstantBuffers](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetconstantbuffers) 하 고 [ID3D11DeviceContext::PSSetConstantBuffers](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetconstantbuffers) 방법 현재이 상수 버퍼를 설정 합니다.

```cpp
// Update the constant buffer with the new data.
m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
    m_constantBuffer.Get(),
    0,
    nullptr,
    &m_mazeConstantBufferData,
    0,
    0);

m_deviceResources->GetD3DDeviceContext()->VSSetConstantBuffers(
    0,                                  // starting at the first constant buffer slot
    1,                                  // set one constant buffer binding
    m_constantBuffer.GetAddressOf());   // to use this buffer

m_deviceResources->GetD3DDeviceContext()->PSSetConstantBuffers(
    0,                                  // starting at the first constant buffer slot
    1,                                  // set one constant buffer binding
    m_constantBuffer.GetAddressOf());   // to use this buffer
```

합니다 **MarbleMazeMain::Render** 메서드 렌더링할 marble를 준비 하는 데 비슷한 단계를 수행 합니다.

### <a name="rendering-the-maze-and-the-marble"></a>미로 및 구슬 렌더링

현재 셰이더를 활성화한 후 장면 개체를 그릴 수 있습니다. 합니다 **MarbleMazeMain::Render** 메서드 호출을 **SDKMesh::Render** maze 메시 렌더링 하는 방법입니다.

```cpp
m_mazeMesh.Render(
    m_deviceResources->GetD3DDeviceContext(), 
    0, 
    INVALID_SAMPLER_SLOT, 
    INVALID_SAMPLER_SLOT);
```

합니다 **MarbleMazeMain::Render** 메서드는 marble를 렌더링 하는 데 비슷한 단계를 수행 합니다.

이 문서의 앞부분에서 설명한 것처럼 **SDKMesh** 클래스는 데모 용도로만 제공되고 프로덕션 품질 게임에서 사용하지 않는 것이 좋습니다. 그러나 합니다 **SDKMesh::RenderMesh** 메서드를 호출 하는 **SDKMesh::Render**를 사용 하는 [ID3D11DeviceContext::IASetVertexBuffers](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers) 및 [ID3D11DeviceContext::IASetIndexBuffer](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer) 현재 꼭 짓 점 및 메시를 정의 하는 인덱스 버퍼를 설정 하는 방법 및 [ID3D11DeviceContext::DrawIndexed](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexedinstanced) 버퍼를 그리는 방법입니다. 꼭짓점 및 인덱스 버퍼로 작업하는 방법에 대한 자세한 내용은 [Direct3D 11의 버퍼 소개](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro)를 참조하세요.

### <a name="drawing-the-user-interface-and-overlay"></a>사용자 인터페이스 및 오버레이 그리기

3D 장면의 개체를 그린 후 Marble Maze 장면 앞에 표시 되는 2D UI 요소를 그립니다.

합니다 **MarbleMazeMain::Render** 메서드가 사용자 인터페이스 및 오버레이 그려 종료 합니다.

```cpp
// Draw the user interface and the overlay.
UserInterface::GetInstance().Render(m_deviceResources->GetOrientationTransform2D());

m_deviceResources->GetD3DDeviceContext()->BeginEventInt(L"Render Overlay", 0);
m_sampleOverlay->Render();
m_deviceResources->GetD3DDeviceContext()->EndEvent();
```

합니다 **UserInterface::Render** 메서드는 [ID2D1DeviceContext](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1devicecontext) UI 요소를 그릴 개체입니다. 이 메서드는 그리기 상태를 설정하고, 모든 활성 UI 요소를 그리고, 이전 그리기 상태를 복원합니다.

```cpp
void UserInterface::Render(D2D1::Matrix3x2F orientation2D)
{
    m_d2dContext->SaveDrawingState(m_stateBlock.Get());
    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(orientation2D);

    m_d2dContext->SetTextAntialiasMode(D2D1_TEXT_ANTIALIAS_MODE_GRAYSCALE);

    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        if ((*iter)->IsVisible())
            (*iter)->Render();
    }

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = m_d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        DX::ThrowIfFailed(hr);
    }

    m_d2dContext->RestoreDrawingState(m_stateBlock.Get());
}
```

**SampleOverlay::Render** 메서드는 유사한 기술을 사용하여 오버레이 비트맵을 그립니다.

###  <a name="presenting-the-scene"></a>장면 표시

모든 2D 및 3D 장면의 개체를 그린 후 Marble Maze는 모니터에 렌더링 되는 이미지를 표시 합니다. 수직 소거에 그리기를 동기화하여 실제로 디스플레이에 표시되지 않는 프레임을 그리는 데 시간을 소비하지 않도록 합니다. Marble Maze는 장면을 표시할 때 장치 변경도 처리합니다.

후는 **MarbleMazeMain::Render** 게임 루프 호출 메서드가 반환 된 **DX::DeviceResources::Present** 모니터에 렌더링 되는 이미지를 보내거나 표시 방법입니다. 합니다 **DX::DeviceResources::Present** 메서드 호출 [IDXGISwapChain::Present](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) 다음 예와에서 같이 있는 작업을 수행 하려면:

```cpp
// The first argument instructs DXGI to block until VSync, putting the application
// to sleep until the next VSync. This ensures we don't waste any cycles rendering
// frames that will never be displayed to the screen.
HRESULT hr = m_swapChain->Present(1, 0);
```

이 예에서 **m\_swapChain** 되는 [IDXGISwapChain1](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) 개체입니다. 이 개체의 초기화는 이 문서의 [Direct3D 및 Direct2D 초기화](#initializing-direct3d-and-direct2d) 섹션에서 설명합니다.

첫 번째 매개 변수를 [IDXGISwapChain::Present](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1)를 *SyncInterval*, 프레임을 표시 하기 전에 대기할 세로 공백의 수를 지정 합니다. Marble Maze는 다음 수직 소거까지 기다리도록 1을 지정합니다.

합니다 [IDXGISwapChain::Present](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) 메서드 장치 제거 되었거나 그렇지 않으면 실패를 나타내는 오류 코드를 반환 합니다. 이 경우 Marble Maze는 장치를 다시 초기화합니다.

```cpp
// If the device was removed either by a disconnection or a driver upgrade, we
// must recreate all device resources.
if (hr == DXGI_ERROR_DEVICE_REMOVED)
{
    HandleDeviceLost();
}
else
{
    DX::ThrowIfFailed(hr);
}
```

## <a name="next-steps"></a>다음 단계


입력 장치 작업을 할 때 고려할 몇 가지 주요 사항에 대한 자세한 내용은 [Marble Maze 샘플에 입력 및 대화형 작업 추가](adding-input-and-interactivity-to-the-marble-maze-sample.md)를 참조하세요. 이 문서에서는 Marble Maze는 터치가 속도계, Xbox 컨트롤러 및 마우스 입력을 지원 하는 방법을 설명 합니다.

## <a name="related-topics"></a>관련 항목


* [입력 및 Marble Maze 샘플에 대화형 작업 추가](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [Marble Maze 응용 프로그램 구조](marble-maze-application-structure.md)
* [UWP 게임 Marble Maze 개발 C++ 와 DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




