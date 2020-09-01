---
title: Marble Maze 샘플에 시각적 콘텐츠 추가
description: 이 문서에서는 Direct2D (UWP) 앱 환경 유니버설 Windows 플랫폼에서 대리석의 Direct3D 및를 사용 하는 방법에 대해 설명 합니다 .이를 통해 사용자의 게임 콘텐츠를 사용 하 여 패턴을 배우고 수정할 수 있습니다.
ms.assetid: 6e43422e-e1a1-b79e-2c4b-7d5b4fa88647
ms.date: 09/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 샘플, directx, 그래픽
ms.localizationpriority: medium
ms.openlocfilehash: ca7eb8e22c6fc90873715aa2177ea171e04f6339
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175387"
---
# <a name="adding-visual-content-to-the-marble-maze-sample"></a>Marble Maze 샘플에 시각적 콘텐츠 추가




이 문서에서는 Direct2D (UWP) 앱 환경 유니버설 Windows 플랫폼에서 대리석의 Direct3D 및를 사용 하는 방법에 대해 설명 합니다 .이를 통해 사용자의 게임 콘텐츠를 사용 하 여 패턴을 배우고 수정할 수 있습니다. Visual game 구성 요소가 대리석의 전체 응용 프로그램 구조에 어떻게 부합 하는지 알아보려면 대리석의 [응용 프로그램 구조](marble-maze-application-structure.md)를 참조 하세요.

다음은 대리석의 시각적 요소를 개발 하는 기본 단계입니다.

1.  Direct3D 및 Direct2D 환경을 초기화 하는 기본 프레임 워크를 만듭니다.
2.  이미지 및 모델 편집 프로그램을 사용 하 여 게임에 표시 되는 2D 및 3D 자산을 디자인 합니다.
3.  2D 및 3D 자산이 제대로 로드 되 고 게임에 표시 되는지 확인 합니다.
4.  게임 자산의 시각적 품질을 향상 시키는 꼭 짓 점 및 픽셀 셰이더를 통합 합니다.
5.  애니메이션 및 사용자 입력과 같은 게임 논리를 통합 합니다.

또한 3D 자산을 추가한 다음 2D 자산에 먼저 집중 했습니다. 예를 들어, 메뉴 시스템 및 타이머를 추가 하기 전에 핵심 게임 논리에 초점을 맞추고 있습니다.

개발 프로세스 중에 이러한 단계 중 일부를 여러 번 반복 해야 하는 경우도 있습니다. 예를 들어 메시 및 대리석 모델을 변경 했을 때 해당 모델을 지 원하는 셰이더 코드의 일부를 변경 해야 했습니다.

> [!NOTE]
> 이 문서에 해당 하는 샘플 코드는 [DirectX 대리석 미로 game 샘플](https://github.com/microsoft/Windows-appsample-marble-maze)에서 찾을 수 있습니다.

 
DirectX 및 시각적 게임 콘텐츠를 사용 하는 경우이 문서에서 설명 하는 핵심 사항 중 일부는 DirectX 그래픽 라이브러리를 초기화 하 고, 장면 리소스를 로드 하 고, 장면을 업데이트 및 렌더링 하는 경우입니다.

-   게임 콘텐츠를 추가 하는 작업에는 일반적으로 여러 단계가 포함 됩니다. 이러한 단계를 반복 해야 하는 경우도 있습니다. 게임 개발자는 3D 게임 콘텐츠를 추가 하 고 2D 콘텐츠를 추가 하는 것이 가장 먼저 집중 하는 경우가 많습니다.
-   더 많은 고객에 게 접근 하 고 가능한 한 광범위 한 그래픽 하드웨어를 지원 하 여 뛰어난 환경을 제공 합니다.
-   디자인 타임 및 런타임 형식을 명확 하 게 구분 합니다. 디자인 타임 자산을 구조화 하 여 유연성을 최대화 하 고 콘텐츠를 신속 하 게 반복할 수 있습니다. 런타임에 최대한 효율적으로 로드 및 렌더링 하기 위해 자산을 포맷 하 고 압축 합니다.
-   클래식 Windows 데스크톱 앱에서와 같이 UWP 앱에서 Direct3D 및 Direct2D 장치를 만듭니다. 한 가지 중요 한 차이점은 스왑 체인이 출력 창과 연결 되는 방법입니다.
-   게임을 디자인할 때 선택한 메시 형식이 주요 시나리오를 지원 하는지 확인 합니다. 예를 들어 게임에서 충돌이 필요한 경우 메시에서 충돌 데이터를 가져올 수 있는지 확인 합니다.
-   렌더링 하기 전에 먼저 모든 장면 개체를 업데이트 하 여 게임 논리를 렌더링 논리에서 분리 합니다.
-   일반적으로 3D 장면 개체와 장면 앞에 표시 되는 모든 2D 개체를 그립니다.
-   그림을 세로로 세로로 동기화 하 여 게임에서 실제로 디스플레이에 표시 되지 않는 프레임을 그리는 데 시간이 걸리지 않도록 합니다. *세로 공백은* 한 프레임이 모니터에 그리기를 완료 하 고 다음 프레임이 시작 될 때 까지의 시간입니다.

## <a name="getting-started-with-directx-graphics"></a>DirectX 그래픽 시작


UWP (대리석 메 이즈 유니버설 Windows 플랫폼) 게임을 계획할 때 렌더링 및 고성능을 최대한으로 제어 해야 하는 3D 게임을 만드는 데 탁월한 선택 사항이 기 때문에 c + + 및 Direct3D 11.1를 선택 했습니다. DirectX 11.1은 DirectX 9에서 DirectX 11 까지의 하드웨어를 지원 하므로 이전 DirectX 버전 각각에 대해 코드를 다시 작성할 필요가 없기 때문에 더 많은 고객에 게 더 효율적으로 연결 하는 데 도움이 될 수 있습니다.

대리석 미로는 Direct3D 11.1를 사용 하 여 3D 게임 자산, 즉 대리석 및 미로를 렌더링 합니다. 또한 대리석 미로는 Direct2D, DirectWrite 및 WIC (Windows Imaging Component)를 사용 하 여 메뉴 및 타이머와 같은 2D 게임 자산을 그립니다.

게임 개발에는 계획이 필요 합니다. DirectX 그래픽을 처음 접하는 경우에는 [directx: 시작](directx-getting-started.md) 하기를 참조 하 여 UWP directx 게임을 만드는 기본 개념을 숙지 하는 것이 좋습니다. 이 문서를 읽고, 대리석의 대리석 소스 코드를 사용 하는 경우 DirectX 그래픽에 대 한 자세한 정보는 다음 리소스를 참조할 수 있습니다.

-   [Direct3d 11 그래픽](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11): Windows 플랫폼에서 3d 기 하 도형을 렌더링 하기 위한 강력한 하드웨어 가속 3D 그래픽 API 인 direct3d 11에 대해 설명 합니다.
-   [Direct2D](/windows/desktop/Direct2D/direct2d-portal): 2d 기 하 도형, 비트맵 및 텍스트에 대 한 고성능 및 고품질 렌더링을 제공 하는 하드웨어 가속 2D 그래픽 API 인 Direct2D을 설명 합니다.
-   [DirectWrite](/windows/desktop/DirectWrite/direct-write-portal): 고품질 텍스트 렌더링을 지 원하는 DirectWrite을 설명 합니다.
-   [Windows 이미징 구성 요소](/windows/desktop/wic/-wic-lh): 디지털 이미지에 대 한 하위 수준 API를 제공 하는 확장 가능한 플랫폼인 WIC에 대해 설명 합니다.

### <a name="feature-levels"></a>기능 수준

Direct3D 11은 *기능 수준*이라는 패러다임을 도입 했습니다. 기능 수준은 잘 정의 된 GPU 기능 집합입니다. 기능 수준을 사용 하 여 이전 버전의 Direct3D 하드웨어에서 실행 되는 게임을 대상으로 합니다. 대리석은 상위 수준의 고급 기능이 필요 하지 않으므로 기능 수준 9.1을 지원 합니다. 하드웨어를 최대한 활용 하 고 게임 콘텐츠의 크기를 조정 하는 것이 좋습니다 .이 경우에는 고성능 컴퓨터를 포함 하는 고객이 모두 뛰어난 환경을 경험할 수 있습니다. 기능 수준에 대 한 자세한 내용은 [하위 하드웨어의 Direct3D 11](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel)을 참조 하십시오.

## <a name="initializing-direct3d-and-direct2d"></a>Direct3D 및 Direct2D 초기화


장치는 디스플레이 어댑터를 나타냅니다. 클래식 Windows 데스크톱 앱에서와 같이 UWP 앱에서 Direct3D 및 Direct2D 장치를 만듭니다. 주요 차이점은 Direct3D 스왑 체인을 창 시스템에 연결 하는 방법입니다.

**DeviceResources** 클래스는 Direct3D 및 Direct2D를 관리 하기 위한 기초입니다. 이 클래스는 게임 특정 자산이 아닌 일반 인프라를 처리 합니다. 대리석 미로는 **MarbleMazeMain** 클래스를 정의 하 여 **DeviceResources** 개체에 대 한 참조를 포함 하 여 Direct3D 및 Direct2D에 대 한 액세스를 제공 합니다.

초기화 하는 동안 **DeviceResources** 생성자는 장치 독립적인 리소스와 Direct3D 및 Direct2D 장치를 만듭니다.

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

**DeviceResources** 클래스는 환경이 변경 될 때 더 쉽게 대응할 수 있도록이 기능을 분리 합니다. 예를 들어 창 크기가 변경 될 때 **CreateWindowSizeDependentResources** 메서드를 호출 합니다.

###  <a name="initializing-the-direct2d-directwrite-and-wic-factories"></a>Direct2D, DirectWrite 및 WIC 팩터리 초기화

**DeviceResources:: CreateDeviceIndependentResources** 메서드는 Direct2D, DIRECTWRITE 및 WIC에 대 한 팩터리를 만듭니다. DirectX 그래픽에서 팩터리는 그래픽 리소스를 만들기 위한 시작점입니다. 대리석 미로는 주 스레드에서 모든 그리기를 수행 하기 때문에 **D2D1 \_ 팩터리 \_ 형식 \_ 단일 \_ 스레드** 를 지정 합니다.

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

**DeviceResources:: CreateDeviceResources** 메서드는 [D3D11CreateDevice](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 를 호출 하 여 Direct3D 표시 어댑터를 나타내는 장치 개체를 만듭니다. 대리석은 기능 수준 9.1 이상을 지원 하기 때문에 **DeviceResources:: CreateDeviceResources** 메서드는 **featurelevels** 배열에 9.1 ~ 11.1 수준을 지정 합니다. Direct3D는 목록을 순서 대로 탐색 하 고 앱에 사용할 수 있는 첫 번째 기능 수준을 제공 합니다. 따라서 **D3D \_ 기능 \_ 수준** 배열 항목은 앱이 가장 높은 기능 수준을 사용할 수 있도록 최고에서 최저 순으로 나열 됩니다. **DeviceResources:: CreateDeviceResources** 메서드는 **D3D11CreateDevice**에서 반환 된 direct3d 11 장치를 쿼리하여 direct3d 11.1 장치를 가져옵니다.

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

그런 다음 **DeviceResources:: CreateDeviceResources** 메서드는 Direct2D 장치를 만듭니다. Direct2D는 Microsoft DirectX Graphics Infrastructure (DXGI)를 사용 하 여 Direct3D와 상호 운용할 수 있습니다. DXGI를 사용 하면 비디오 메모리 표면을 그래픽 런타임 간에 공유할 수 있습니다. 대리석 메 이즈는 Direct3D 장치에서 기본 DXGI 장치를 사용 하 여 Direct2D 팩터리에서 Direct2D 장치를 만듭니다.

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

Direct2D와 Direct3D 간의 DXGI 및 상호 운용성에 대 한 자세한 내용은 [Dxgi 개요](/windows/desktop/direct3ddxgi/d3d10-graphics-programming-guide-dxgi) 및 [Direct2D 및 Direct3d 상호 운용성 개요](/windows/desktop/Direct2D/direct2d-and-direct3d-interoperation-overview)를 참조 하세요.

### <a name="associating-direct3d-with-the-view"></a>뷰와 Direct3D 연결

**DeviceResources:: CreateWindowSizeDependentResources** 메서드는 스왑 체인 및 Direct3D 및 Direct2D 렌더링 대상과 같은 지정 된 창 크기에 따라 달라 지는 그래픽 리소스를 만듭니다. DirectX UWP 앱이 데스크톱 앱과 다른 중요 한 한 가지 중요 한 방법은 스왑 체인이 출력 창과 연결 되는 방법입니다. 스왑 체인은 장치가 모니터에서 렌더링 하는 버퍼를 표시 합니다. [대리석 미로 응용 프로그램 구조](marble-maze-application-structure.md) UWP 앱에 대 한 창 이동 시스템이 데스크톱 앱과 어떻게 다른 지 설명 합니다. UWP 앱은 [HWND](/windows/desktop/WinProg/windows-data-types) 개체에서 작동 하지 않으므로, 대리석은 [IDXGIFactory2:: CreateSwapChainForCoreWindow](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow) 메서드를 사용 하 여 장치 출력을 뷰에 연결 해야 합니다. 다음 예제에서는 스왑 체인을 만드는 **DeviceResources:: CreateWindowSizeDependentResources** 메서드의 일부를 보여 줍니다.

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

노트북, 태블릿 등의 배터리 기반 장치에서 수행 해야 하는 전원 소비를 최소화 하기 위해 **DeviceResources:: CreateWindowSizeDependentResources** 메서드는 [IDXGIDevice1:: SetMaximumFrameLatency](/windows/desktop/api/dxgi/nf-dxgi-idxgidevice1-setmaximumframelatency) 메서드를 호출 하 여 게임이 세로 빈 후에만 렌더링 되도록 합니다. 세로 공백으로 동기화 하는 방법은이 문서의 [장면](#presenting-the-scene) 표시 섹션에 자세히 설명 되어 있습니다.

```cpp
// Ensure that DXGI does not queue more than one frame at a time. This both 
// reduces latency and ensures that the application will only render after each
// VSync, minimizing power consumption.
DX::ThrowIfFailed(
    dxgiDevice->SetMaximumFrameLatency(1)
    );
```

**DeviceResources:: CreateWindowSizeDependentResources** 메서드는 대부분의 게임에서 작동 하는 방식으로 그래픽 리소스를 초기화 합니다.

> [!NOTE]
> 용어 *보기* 는 Direct3D에서와 동일한 의미를 Windows 런타임 합니다. Windows 런타임 보기는 표시 영역, 입력 동작 및 처리에 사용 되는 스레드를 비롯 하 여 앱에 대 한 사용자 인터페이스 설정 컬렉션을 나타냅니다. 보기를 만들 때 필요한 구성 및 설정을 지정할 수 있습니다. 앱 보기를 설정 하는 프로세스는 [대리석-대리석 응용 프로그램 구조](marble-maze-application-structure.md)에 설명 되어 있습니다.
> Direct3D에서 용어 보기에는 여러 가지 의미가 있습니다. 리소스 뷰는 리소스가 액세스할 수 있는 하위 리소스를 정의 합니다. 예를 들어 텍스처 개체가 셰이더 리소스 뷰와 연결 된 경우 해당 셰이더는 나중에 질감에 액세스할 수 있습니다. 리소스 뷰의 이점 중 하나는 렌더링 파이프라인의 여러 단계에서 다양 한 방법으로 데이터를 해석할 수 있다는 점입니다. 리소스 뷰에 대 한 자세한 내용은 [리소스 뷰](/windows/desktop/direct3d11/overviews-direct3d-11-resources-intro)를 참조 하세요.
> 뷰 변환 또는 뷰 변환 매트릭스의 컨텍스트에서 사용 하는 경우 보기는 카메라의 위치와 방향을 나타냅니다. 뷰 변환은 카메라의 위치와 방향을 기준으로 세계의 개체를 재배치 합니다. 뷰 변환에 대 한 자세한 내용은 [변환 보기 (Direct3D 9)](/windows/desktop/direct3d9/view-transform)를 참조 하세요. 이 항목에서는 대리석 미로에서 리소스 및 행렬 뷰를 사용 하는 방법에 대해 자세히 설명 합니다.

 

## <a name="loading-scene-resources"></a>장면 리소스 로드


대리석 메 이즈는 **basicloader. h**에서 선언 된 **basicloader** 클래스를 사용 하 여 질감 및 셰이더를 로드 합니다. 대리석 메 이즈는 **SDKMesh** 클래스를 사용 하 여 미로 및 대리석의 3d 메시를 로드 합니다.

응답성이 뛰어난 앱을 보장 하기 위해 대리석은 장면 리소스를 비동기식으로 또는 백그라운드에서 로드 합니다. 자산이 백그라운드에서 로드 되 면 게임이 창 이벤트에 응답할 수 있습니다. 이 프로세스는이 가이드의 [배경에서 게임 자산 로드](marble-maze-application-structure.md#loading-game-assets-in-the-background) 에서 더 자세히 설명 합니다.

###  <a name="loading-the-2d-overlay-and-user-interface"></a>2D 오버레이 및 사용자 인터페이스 로드

대리석의 미로에서 오버레이는 화면 맨 위에 표시 되는 이미지입니다. 오버레이는 항상 장면 앞에 표시 됩니다. 대리석 x 메 이즈의 오버레이에는 Windows 로고 및 텍스트 문자열 **DirectX 대리석 미로 game 샘플이**포함 되어 있습니다. 오버레이 관리는 **SampleOverlay**에 정의 된 **SampleOverlay** 클래스에 의해 수행 됩니다. Direct3D 샘플에 포함된 오버레이를 사용하지만 이 코드를 조정하여 임의 이미지를 장면 앞에 표시할 수 있습니다.

오버레이에서 중요 한 한 가지 측면은 해당 내용이 변경 되지 않기 때문에 **SampleOverlay** 클래스는 초기화 중에 [ID2D1Bitmap1](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1bitmap1) 개체에 해당 콘텐츠를 그리기 또는 캐시 한다는 것입니다. 그리기 타임에 **SampleOverlay** 클래스는 화면에 비트맵을 그려야 합니다. 이러한 방식으로 텍스트 그리기와 같은 비용이 많이 드는 루틴은 모든 프레임에 대해 수행할 필요가 없습니다.

UI (사용자 인터페이스)는 장면 앞에 표시 되는 메뉴 및 헤드 표시 (HUDs)와 같은 2D 구성 요소로 구성 됩니다. 대리석은 다음 UI 요소를 정의 합니다.

-   사용자가 게임을 시작 하거나 높은 점수를 볼 수 있도록 하는 메뉴 항목입니다.
-   재생이 시작 되기까지 3 초 동안 계산 되는 타이머입니다.
-   경과 된 재생 시간을 추적 하는 타이머입니다.
-   가장 빠른 완료 시간을 나열 하는 테이블입니다.
-   게임을 일시 중지할 때 **일시 중지** 된 내용을 읽는 텍스트입니다.

대리석은 **Userinterface. h**의 게임 관련 UI 요소를 정의 합니다. 대리석은 모든 UI 요소에 대 한 기본 형식으로 **Elementbase** 클래스를 정의 합니다. **Elementbase** 클래스는 UI 요소의 크기, 위치, 맞춤 및 표시 유형 등의 특성을 정의 합니다. 또한 요소를 업데이트 하 고 렌더링 하는 방법을 제어 합니다.

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

UI 요소에 대 한 공통 기본 클래스를 제공 하 여 사용자 인터페이스를 관리 하는 **Userinterface** 클래스는 ui 관리를 간소화 하 고 재사용 가능한 사용자 인터페이스 관리자를 제공 하는 **elementbase** 개체의 컬렉션을 보유 하기만 하면 됩니다. 대리석 미로는 game 특정 동작을 구현 하는 **Elementbase** 에서 파생 되는 형식을 정의 합니다. 예를 들어 **HighScoreTable** 는 높은 점수 테이블의 동작을 정의 합니다. 이러한 형식에 대 한 자세한 내용은 소스 코드를 참조 하세요.

> [!NOTE]
> XAML을 사용 하면 시뮬레이션 및 전략 게임과 같이 복잡 한 사용자 인터페이스를 보다 쉽게 만들 수 있으므로 XAML을 사용 하 여 UI를 정의할 지 여부를 고려해 야 합니다. DirectX UWP 게임의 XAML에서 사용자 인터페이스를 개발 하는 방법에 대 한 자세한 내용은 DirectX 3D 촬영 게임 샘플을 참조 하는 [game 샘플 확장](tutorial-resources.md)을 참조 하세요.

 

###  <a name="loading-shaders"></a>셰이더 로드

대리석 미로는 **Basicloader:: LoadShader** 메서드를 사용 하 여 파일에서 셰이더를 로드 합니다.

셰이더는 현재 게임에서 GPU 프로그래밍의 기본 단위입니다. 거의 모든 3D 그래픽 처리는 문자 스키닝에서 공간 분할 (tessellation)에서 공간 분할 인지 여부에 관계 없이 셰이더를 통해 구동 됩니다. 셰이더 프로그래밍 모델에 대 한 자세한 내용은 [HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl)를 참조 하세요.

대리석 x는 꼭 짓 점 및 픽셀 셰이더를 사용 합니다. 꼭 짓 점 셰이더는 항상 하나의 입력 꼭지점에서 작동 하 고 하나의 꼭 짓 점을 출력으로 생성 합니다. 픽셀 셰이더는 숫자 값, 질감 데이터, 꼭지점 별 보간된 값 및 기타 데이터를 사용 하 여 픽셀 색을 출력으로 생성 합니다. 셰이더는 한 번에 하나의 요소를 변환 하기 때문에 여러 셰이더 파이프라인을 제공 하는 그래픽 하드웨어는 요소 집합을 병렬로 처리할 수 있습니다. GPU에서 사용할 수 있는 병렬 파이프라인 수는 CPU에서 사용할 수 있는 수보다 크게 클 수 있습니다. 따라서 기본 셰이더도 처리량을 크게 향상 시킬 수 있습니다.

**MarbleMazeMain:: LoadDeferredResources** 메서드는 오버레이를 로드 한 후 하나의 꼭 짓 점 셰이더 및 1 픽셀 셰이더를 로드 합니다. 이러한 셰이더의 디자인 타임 버전은 각각 **BasicVertexShader hlsl** 및 **BasicPixelShader**에 정의 되어 있습니다. 대리석은 렌더링 단계에서 이러한 셰이더를 공을 및 미로 모두에 적용 합니다.

대리석 미로 프로젝트에는 셰이더 파일의 hlsl (디자인 타임 형식)와 .cit (런타임 형식) 버전이 모두 포함 되어 있습니다. 빌드 시 Visual Studio는 fxc.exe 효과-컴파일러를 사용 하 여 hlsl 소스 파일을 .csbinary 셰이더에 컴파일합니다. 효과-컴파일러 도구에 대 한 자세한 내용은 [효과-컴파일러 도구](/windows/desktop/direct3dtools/fxc)를 참조 하세요.

꼭 짓 점 셰이더는 제공 된 모델, 뷰 및 프로젝션 매트릭스를 사용 하 여 입력 기 하 도형을 변환 합니다. 입력 기 하 도형의 위치 데이터는 변환 되 고 두 번 출력 됩니다. 화면 공간에서 렌더링 하는 데 필요 하 고, 영역에서 픽셀 셰이더를 사용 하 여 조명 계산을 수행할 수 있습니다. 표면 법선 벡터는 세계 공간으로 변환 되 고 광원의 픽셀 셰이더에도 사용 됩니다. 질감 좌표는 변경 되지 않은 상태로 픽셀 셰이더에 전달 됩니다.

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

픽셀 셰이더는 꼭 짓 점 셰이더의 출력을 입력으로 받습니다. 이 셰이더는 빛 계산을 수행 하 여 미로를 중심으로 하는 부드러운 가장자리 스포트라이트를 모방 하 고 대리석의 위치에 맞춥니다. 조명을 직접 가리키는 표면에서 조명이 가장 강력 합니다. 조명이 조명을 수직이 될 때 확산 구성 요소는 0으로 tapers, 앰비언트 용어는 빛에서 떨어진 법선 점으로 저하 됩니다. 대리석에 가까울수록 스포트라이트의 중심에 가까울수록 더 강력 하 게 켜 집니다. 그러나 빛은 대리석을 시뮬레이션 하는 대리석 무늬 아래 점에 맞게 변조 됩니다. 실제 환경에서는 흰색 대리석과 같은 개체가 장면의 다른 개체에 스포트라이트를 확산 합니다. 이는 대리석 무늬의 밝은 절반에 있는 표면에 대해 대략적으로 표시 됩니다. 추가 조명 요소는 상대 각도와 대리석 까지의 거리입니다. 결과 픽셀 색은 조명 계산 결과를 사용 하 여 샘플링 된 질감의 컴퍼지션입니다.

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
> 컴파일된 픽셀 셰이더에는 32 산술 명령 및 1 개의 질감 명령이 포함 되어 있습니다. 이 셰이더는 데스크톱 컴퓨터와 고성능 태블릿에서 잘 작동 합니다. 그러나 낮은 최종 컴퓨터는이 셰이더를 처리 하지 못할 수 있으며 대화형 프레임 속도로 계속 제공 됩니다. 대상 사용자의 일반적인 하드웨어를 고려 하 고 해당 하드웨어의 기능에 맞게 셰이더를 디자인 합니다.

 

**MarbleMazeMain:: LoadDeferredResources** 메서드는 **Basicloader:: loadshader** 메서드를 사용 하 여 셰이더를 로드 합니다. 다음 예제에서는 꼭 짓 점 셰이더를 로드 합니다. 이 셰이더의 런타임 형식은 **BasicVertexShader입니다.** **M \_ vertexShader** 멤버 변수는 [ID3D11VertexShader](/windows/desktop/api/d3d11/nn-d3d11-id3d11vertexshader) 개체입니다.

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

**M \_ inputlayout** 멤버 변수는 [ID3D11InputLayout](/windows/desktop/api/d3d11/nn-d3d11-id3d11inputlayout) 개체입니다. 입력 레이아웃 개체는 입력 어셈블러 (IA) 단계의 입력 상태를 캡슐화 합니다. IA 단계의 작업 중 하나는 시스템에서 생성된 값(*의미 체계*라고도 함)을 사용하여 아직 처리되지 않은 기본 요소 또는 꼭짓점만 처리하도록 하여 셰이더를 더 효율적으로 만드는 것입니다.

[ID3D11Device:: CreateInputLayout](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createinputlayout) 메서드를 사용 하 여 입력 요소 설명의 배열에서 입력 레이아웃을 만듭니다. 배열에 입력 요소가 하나 이상 포함 되어 있습니다. 각 입력 요소는 한 꼭 짓 점 버퍼의 단일 꼭 짓 점 데이터 요소를 설명 합니다. 전체 입력 요소 설명 집합에서는 IA 단계에 바인딩되는 모든 꼭 짓 점 버퍼의 모든 꼭 짓 점 데이터 요소에 대해 설명 합니다. 

위의 코드 조각에 있는 **Layoutdesc** 는 대리석에서 사용 하는 레이아웃 설명을 보여 줍니다. 레이아웃 설명에서는 네 개의 꼭 짓 점 데이터 요소를 포함 하는 꼭 짓 점 버퍼에 대해 설명 합니다. 배열의 각 항목에 대 한 중요 한 부분은 의미 체계 이름, 데이터 형식 및 바이트 오프셋입니다. 예를 들어 **position** 요소는 개체 공간에서 꼭 짓 점 위치를 지정 합니다. 바이트 오프셋 0에서 시작 하 고 세 개의 부동 소수점 구성 요소 (총 12 바이트)를 포함 합니다. **Normal** 요소는 법선 벡터를 지정 합니다. 이는 레이아웃의 **위치** 바로 뒤에 표시 되므로 12 바이트가 필요 하므로 바이트 오프셋 12에서 시작 합니다. **NORMAL** 요소는 네 개의 구성 요소 32 비트 부호 없는 정수를 포함 합니다.

다음 예제와 같이 입력 레이아웃을 꼭 짓 점 셰이더에 정의 된 **Svsinput** 구조와 비교 합니다. **Svsinput** 구조는 **POSITION**, **NORMAL**및 **TEXCOORD0** 요소를 정의 합니다. DirectX 런타임은 레이아웃의 각 요소를 셰이더에 의해 정의 된 입력 구조에 매핑합니다.

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

문서 [의미 체계](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics) 는 사용 가능한 각 의미 체계에 대해 자세히 설명 합니다.

> [!NOTE]
> 레이아웃에서는 여러 셰이더가 동일한 레이아웃을 공유할 수 있도록 하는 데 사용 되지 않는 추가 구성 요소를 지정할 수 있습니다. 예를 들어, 셰이더에는 **탄젠트** 요소가 사용 되지 않습니다. 표준 매핑과 같은 기술을 사용 하 여 실험 하려는 경우에는 **탄젠트** 요소를 사용할 수 있습니다. 범프 매핑이 라고도 하는 일반적인 매핑을 사용 하 여 개체의 표면에서 충격 효과를 만들 수 있습니다. 범프 매핑에 대 한 자세한 내용은 [범프 매핑 (Direct3D 9)](/windows/desktop/direct3d9/bump-mapping)을 참조 하세요.

 

입력 어셈블리 단계에 대 한 자세한 내용은 입력 [-어셈블러 단계](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage) 및 [입력-어셈블러 단계 시작](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage-getting-started)을 참조 하세요.

꼭 짓 점 및 픽셀 셰이더를 사용 하 여 장면을 렌더링 하는 프로세스는이 문서의 뒷부분에 나오는 [장면 렌더링](#rendering-the-scene) 섹션에 설명 되어 있습니다.

### <a name="creating-the-constant-buffer"></a>상수 버퍼 만들기

Direct3D 버퍼는 데이터 컬렉션을 그룹화 합니다. 상수 버퍼는 데이터를 셰이더에 전달 하는 데 사용할 수 있는 버퍼의 일종입니다. 대리석 미로는 상수 버퍼를 사용 하 여 모델 (또는 세계) 뷰와 활성 장면 개체의 프로젝션 행렬을 유지 합니다.

다음 예제에서는 **MarbleMazeMain:: LoadDeferredResources** 메서드가 나중에 행렬 데이터를 보관할 상수 버퍼를 만드는 방법을 보여 줍니다. 이 예제에서는 **D3D11 \_ BIND \_ 상수 \_ 버퍼** 플래그를 사용 하 여 사용량을 상수 버퍼로 지정 하는 **D3D11 \_ BUFFER \_ DESC** 구조체를 만듭니다. 그런 다음이 예제에서는 해당 구조를 [ID3D11Device:: CreateBuffer](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) 메서드에 전달 합니다. **M \_ constantBuffer** 변수는 [ID3D11Buffer](/windows/desktop/api/d3d11/nn-d3d11-id3d11buffer) 개체입니다.

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

**MarbleMazeMain:: Update** 메서드는 메 이즈 및 대리석에 대해 하나씩, **ConstantBuffer** 개체를 업데이트 합니다. 그런 다음 **MarbleMazeMain:: Render** 메서드는 각 개체를 렌더링 하기 전에 각 **ConstantBuffer** 개체를 상수 버퍼에 바인딩합니다. 다음 예제에서는 **MarbleMazeMain**에 있는 **ConstantBuffer** 구조체를 보여 줍니다.

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

상수 버퍼가 셰이더 코드에 매핑되는 방식을 더 잘 이해 하려면 **MarbleMazeMain** 의 **ConstantBuffer** 구조체를 **BasicVertexShader**의 꼭 짓 점 셰이더에 정의 된 **ConstantBuffer** 상수 버퍼와 비교 합니다.

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

**ConstantBuffer** 구조의 레이아웃이 **cbuffer** 개체와 일치 합니다. **Cbuffer** 변수는 레지스터 b0을 지정 합니다. 즉, 상수 버퍼 데이터가 레지스터 0에 저장 됩니다. **MarbleMazeMain:: Render** 메서드는 상수 버퍼를 활성화할 때 register 0을 지정 합니다. 이 프로세스는이 문서의 뒷부분에서 자세히 설명 합니다.

상수 버퍼에 대 한 자세한 내용은 [Direct3D 11의 버퍼 소개](/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro)를 참조 하세요. Register 키워드에 대 한 자세한 내용은 [register](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-variable-register)를 참조 하세요.

###  <a name="loading-meshes"></a>메시 로드

대리석은 샘플 응용 프로그램에 대 한 메시 데이터를 로드 하는 기본적인 방법을 제공 하므로 대리석 메 이즈는 런타임 형식으로 SDK 메시를 사용 합니다. 프로덕션 사용을 위해 게임의 특정 요구 사항을 충족 하는 메시 형식을 사용 해야 합니다.

**MarbleMazeMain:: LoadDeferredResources** 메서드는 꼭 짓 점 및 픽셀 셰이더를 로드 한 후 메시 데이터를 로드 합니다. 메시는 위치, 일반 데이터, 색, 재질, 질감 좌표 등의 정보를 포함 하는 꼭 짓 점 데이터의 컬렉션입니다. 메시는 일반적으로 3D 제작 소프트웨어에서 만들어지며 응용 프로그램 코드와 분리 된 파일에서 유지 관리 됩니다. 대리석 및 미로는 게임에서 사용 하는 메시의 두 가지 예입니다.

대리석- **SDKMesh** 클래스를 사용 하 여 메시를 관리 합니다. 이 클래스는 **SDKMesh**에서 선언 됩니다. **SDKMesh** 는 메시 데이터를 로드, 렌더링 및 제거 하는 메서드를 제공 합니다.

> [!IMPORTANT]
> 대리석은 SDK-메시 형식을 사용 하며 **SDKMesh** 클래스를 제공 합니다. SDK 메시 형식은 학습에 유용 하 고 프로토타입을 만드는 데 유용 하지만 대부분의 게임 개발 요구 사항을 충족 하지 못할 수 있는 매우 기본적인 형식입니다. 게임의 특정 요구 사항을 충족 하는 메시 형식을 사용 하는 것이 좋습니다.

 

다음 예제에서는 **MarbleMazeMain:: LoadDeferredResources** 메서드가 **SDKMesh:: Create** 메서드를 사용 하 여 메 이즈의 메시 데이터를 로드 하는 방법을 보여 줍니다.

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

이 섹션에서는 대리석에서 대리석 및 미로 사이의 물리 시뮬레이션을 구현 하는 방법에 중점을 두지 않지만, 메시를 로드할 때 물리 시스템의 메시 기 하 도형을 읽습니다.

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

충돌 데이터를 로드 하는 방법은 주로 사용 하는 런타임 형식에 따라 달라 집니다. 대리석이 SDK-메시 파일에서 충돌 기 하 도형을 로드 하는 방법에 대 한 자세한 내용은 소스 코드의 **MarbleMazeMain:: ExtractTrianglesFromMesh** 메서드를 참조 하세요.

## <a name="updating-game-state"></a>게임 상태 업데이트 중


대리석 x 메 이즈는 먼저 모든 장면 개체를 렌더링 하기 전에 업데이트 하 여 게임 논리를 렌더링 논리와 분리 합니다.

[대리석 미로 응용 프로그램 구조](marble-maze-application-structure.md) 주 게임 루프를 설명 합니다. 게임 루프의 일부인 장면을 업데이트 하는 것은 Windows 이벤트와 입력이 처리 된 후와 장면이 렌더링 되기 전에 발생 합니다. **MarbleMazeMain:: update** 메서드는 UI와 게임의 업데이트를 처리 합니다.

### <a name="updating-the-user-interface"></a>사용자 인터페이스 업데이트

**MarbleMazeMain:: update** 메서드는 **Userinterface:: update** 메서드를 호출 하 여 UI의 상태를 업데이트 합니다.

```cpp
UserInterface::GetInstance().Update(
    static_cast<float>(m_timer.GetTotalSeconds()), 
    static_cast<float>(m_timer.GetElapsedSeconds()));
```

**Userinterface:: Update** 메서드는 UI 컬렉션의 각 요소를 업데이트 합니다.

```cpp
void UserInterface::Update(float timeTotal, float timeDelta)
{
    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        (*iter)->Update(timeTotal, timeDelta);
    }
}
```

**Elementbase** 에서 파생 된 클래스 ( **userinterface. h**에서 정의 됨)는 **Update** 메서드를 구현 하 여 특정 동작을 수행 합니다. 예를 들어 **StopwatchTimer:: Update** 메서드는 제공 된 양만큼 경과 시간을 업데이트 하 고 나중에 표시 되는 텍스트를 업데이트 합니다.

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

**MarbleMazeMain:: Update** 메서드는 상태 시스템의 현재 상태 ( **m_gameState**에 저장 된 **GameState**)를 기반으로 게임을 업데이트 합니다. 게임이 활성 상태 (**GameState:: InGameActive**) 인 경우 대리석은 대리석을 따라 카메라를 업데이트 하 고, 상수 버퍼의 뷰 행렬 부분을 업데이트 하 고, 물리학 시뮬레이션을 업데이트 합니다.

다음 예제에서는 **MarbleMazeMain:: Update** 메서드가 카메라의 위치를 업데이트 하는 방법을 보여 줍니다. 대리석 미로는 **m \_ resetcamera** 변수를 사용 하 여 카메라를 대리석 무늬 바로 위에 배치 하도록 다시 설정 해야 함을 플래그 지정 합니다. 게임이 시작 되거나 대리석이 미로를 벗어날 때 카메라가 다시 설정 됩니다. 주 메뉴 또는 높은 점수 표시 화면이 활성 상태 이면 카메라는 일정 한 위치에 설정 됩니다. 그렇지 않으면 대리석은 *Timedelta* 매개 변수를 사용 하 여 현재 위치와 대상 위치 사이에서 카메라의 위치를 보간합니다. 대상 위치가 대리석 위 및 앞에 있습니다. 경과 된 프레임 시간을 사용 하면 카메라에서 대리석을 점차적으로 팔 로우 하거나 추적 하는 데 사용할 수 있습니다.

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

다음 예제에서는 **MarbleMazeMain:: Update** 메서드가 대리석 및 미로의 상수 버퍼를 업데이트 하는 방법을 보여 줍니다. 미로의 모델 또는 세계 행렬은 항상 항등 매트릭스로 유지 됩니다. 요소가 모두 있는 주 대각선을 제외 하 고 항등 행렬은 0으로 구성 된 사각형입니다. 대리석의 모델 행렬은 회전 행렬의 위치 매트릭스 시간을 기반으로 합니다.

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

**MarbleMazeMain:: Update** 메서드가 사용자 입력을 읽고 대리석의 동작을 시뮬레이트하는 방법에 대 한 자세한 내용은 [대리석에 입력 및 대화형 작업을 추가](adding-input-and-interactivity-to-the-marble-maze-sample.md)하는 방법 샘플을 참조 하세요.

## <a name="rendering-the-scene"></a>장면 렌더링


장면이 렌더링 되 면 일반적으로 이러한 단계가 포함 됩니다.

1.  현재 렌더링 대상 깊이 스텐실 버퍼를 설정 합니다.
2.  렌더링 및 스텐실 뷰를 지웁니다.
3.  그리기를 위해 꼭 짓 점 및 픽셀 셰이더를 준비 합니다.
4.  장면에서 3D 개체를 렌더링 합니다.
5.  장면 앞에 표시할 모든 2D 개체를 렌더링 합니다.
6.  렌더링 된 이미지를 모니터에 표시 합니다.

**MarbleMazeMain:: render** 메서드는 렌더링 대상 및 깊이 스텐실 뷰를 바인딩하고, 해당 뷰를 지우고, 장면을 그린 다음, 오버레이를 그립니다.

###  <a name="preparing-the-render-targets"></a>렌더링 대상 준비

장면을 렌더링 하기 전에 현재 렌더링 대상 깊이 스텐실 버퍼를 설정 해야 합니다. 장면이 화면에 있는 모든 픽셀에 대해 그려지지 않을 것으로 확신할 수 없는 경우 렌더링 및 스텐실 보기도 지웁니다. 대리석-모든 프레임에서 렌더링 및 스텐실 뷰를 지워 이전 프레임에서 표시 되는 아티팩트가 없도록 합니다.

다음 예제에서는 **MarbleMazeMain:: render** 메서드가 [ID3D11DeviceContext:: OMSetRenderTargets](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) 메서드를 호출 하 여 렌더링 대상과 깊이 스텐실 버퍼를 현재 상태로 설정 하는 방법을 보여 줍니다.

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

[ID3D11RenderTargetView](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) 및 [ID3D11DepthStencilView](/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilview) 인터페이스는 Direct3D 10 이상에서 제공 하는 질감 보기 메커니즘을 지원 합니다. 질감 보기에 대 한 자세한 내용은 [질감 뷰 (Direct3D 10)](/windows/desktop/direct3d10/d3d10-graphics-programming-guide-resources-access-views)를 참조 하세요. [OMSetRenderTargets](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) 메서드는 Direct3D 파이프라인의 출력 병합기 단계를 준비 합니다. 출력 병합기 단계에 대 한 자세한 내용은 [출력-병합기 단계](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage)를 참조 하세요.

### <a name="preparing-the-vertex-and-pixel-shaders"></a>꼭 짓 점 및 픽셀 셰이더 준비

장면 개체를 렌더링 하기 전에 다음 단계를 수행 하 여 그리기를 위해 꼭 짓 점 및 픽셀 셰이더를 준비 합니다.

1.  셰이더 입력 레이아웃을 현재 레이아웃으로 설정 합니다.
2.  꼭 짓 점 및 픽셀 셰이더를 현재 셰이더로 설정 합니다.
3.  셰이더에 전달 해야 하는 데이터로 상수 버퍼를 업데이트 합니다.

> [!IMPORTANT]
> 대리석 미로는 모든 3D 개체에 대해 한 쌍의 꼭 짓 점 및 픽셀 셰이더를 사용 합니다. 게임에서 여러 쌍의 셰이더를 사용 하는 경우 다른 셰이더를 사용 하는 개체를 그릴 때마다 이러한 단계를 수행 해야 합니다. 셰이더 상태 변경과 관련 된 오버 헤드를 줄이려면 동일한 셰이더를 사용 하는 모든 개체에 대해 렌더링 호출을 그룹화 하는 것이 좋습니다.

 

이 문서의 [셰이더 로드](#loading-shaders) 섹션에서는 꼭 짓 점 셰이더를 만들 때 입력 레이아웃을 만드는 방법에 대해 설명 합니다. 다음 예제에서는 **MarbleMazeMain:: Render** [메서드를 사용](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout) 하 여이 레이아웃을 현재 레이아웃으로 설정 하는 방법을 보여 줍니다.

```cpp
m_deviceResources->GetD3DDeviceContext()->IASetInputLayout(m_inputLayout.Get());
```

다음 예제에서는 **MarbleMazeMain:: Render** 메서드에서 [ID3D11DeviceContext:: Vssetshader](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader) 및 [ID3D11DeviceContext::P ssetshader](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader) 메서드를 사용 하 여 꼭 짓 점 및 픽셀 셰이더를 각각 현재 셰이더에 설정 하는 방법을 보여 줍니다.

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

**MarbleMazeMain:: Render** 는 셰이더와 해당 입력 레이아웃을 설정 하 고, [ID3D11DeviceContext:: UpdateSubresource](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-updatesubresource) 메서드를 사용 하 여 미로의 모델, 뷰 및 프로젝션 매트릭스를 사용 하 여 상수 버퍼를 업데이트 합니다. **UpdateSubresource** 메서드는 CPU 메모리에서 GPU 메모리로 행렬 데이터를 복사 합니다. **ConstantBuffer** 구조의 모델 및 뷰 구성 요소가 **MarbleMazeMain:: Update** 메서드에서 업데이트 되는 것을 기억 하세요. **MarbleMazeMain:: Render** 메서드는 [ID3D11DeviceContext:: VSSetConstantBuffers](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetconstantbuffers) 및 [ID3D11DeviceContext::P ssetconstantbuffers](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetconstantbuffers) 메서드를 호출 하 여이 상수 버퍼를 현재로 설정 합니다.

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

**MarbleMazeMain:: Render** 메서드는 유사한 단계를 수행 하 여 렌더링할 대리석을 준비 합니다.

### <a name="rendering-the-maze-and-the-marble"></a>미로 및 대리석 렌더링

현재 셰이더를 활성화 한 후에는 장면 개체를 그릴 수 있습니다. **MarbleMazeMain:: render** 메서드는 **SDKMesh:: render** 메서드를 호출 하 여 미로 메시를 렌더링 합니다.

```cpp
m_mazeMesh.Render(
    m_deviceResources->GetD3DDeviceContext(), 
    0, 
    INVALID_SAMPLER_SLOT, 
    INVALID_SAMPLER_SLOT);
```

**MarbleMazeMain:: render** 메서드는 대리석을 렌더링 하기 위해 유사한 단계를 수행 합니다.

이 문서 앞부분에서 언급 했 듯이 **SDKMesh** 클래스는 데모용으로 제공 되지만 프로덕션 품질 게임에서는 사용 하지 않는 것이 좋습니다. 그러나 **SDKMesh:: Render**에 의해 호출 되는 **SDKMesh:: Rendermesh** 메서드는 [ID3D11DeviceContext:: IASetVertexBuffers](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers) 및 [ID3D11DeviceContext:: IASetIndexBuffer](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer) 메서드를 사용 하 여 메시를 정의 하는 현재 꼭 짓 점 및 인덱스 버퍼를 설정 하 고 [ID3D11DeviceContext::D rawindexed](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexedinstanced) 메서드를 사용 하 여 버퍼를 그립니다. 꼭 짓 점 및 인덱스 버퍼로 작업 하는 방법에 대 한 자세한 내용은 [Direct3D 11의 버퍼 소개](/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro)를 참조 하세요.

### <a name="drawing-the-user-interface-and-overlay"></a>사용자 인터페이스 및 오버레이 그리기

3D 장면 개체를 그린 후 대리석은 장면 앞에 나타나는 2D UI 요소를 그립니다.

**MarbleMazeMain:: Render** 메서드는 사용자 인터페이스와 오버레이를 그려 종료 합니다.

```cpp
// Draw the user interface and the overlay.
UserInterface::GetInstance().Render(m_deviceResources->GetOrientationTransform2D());

m_deviceResources->GetD3DDeviceContext()->BeginEventInt(L"Render Overlay", 0);
m_sampleOverlay->Render();
m_deviceResources->GetD3DDeviceContext()->EndEvent();
```

**Userinterface:: Render** 메서드는 [ID2D1DeviceContext](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1devicecontext) 개체를 사용 하 여 UI 요소를 그립니다. 이 메서드는 그리기 상태를 설정 하 고, 모든 활성 UI 요소를 그린 다음, 이전 그리기 상태를 복원 합니다.

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

**SampleOverlay:: Render** 메서드는 비슷한 기술을 사용 하 여 오버레이 비트맵을 그립니다.

###  <a name="presenting-the-scene"></a>장면 프레젠테이션

모든 2D 및 3D 장면 개체를 그린 후 대리석 무늬 메 이즈는 렌더링 된 이미지를 모니터에 표시 합니다. 드로잉을 세로로 세로로 동기화 하 여 표시에 실제로 표시 되지 않는 시간을 그리기 위해 시간을 소요 하지 않도록 합니다. 또한 대리석은 장면이 표시 될 때 장치 변경 내용을 처리 합니다.

**MarbleMazeMain:: Render** 메서드가 반환 된 후 game Loop는 **DX::D eviceresources::P 재전송** 메서드를 호출 하 여 렌더링 된 이미지를 모니터 또는 표시로 보냅니다. 다음 예제와 같이 **DX::D eviceresources::P 다시 보낸** 메서드는 [Idxgiswapchain::P 다시 보낸](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) 것을 호출 하 여 현재 작업을 수행 합니다.

```cpp
// The first argument instructs DXGI to block until VSync, putting the application
// to sleep until the next VSync. This ensures we don't waste any cycles rendering
// frames that will never be displayed to the screen.
HRESULT hr = m_swapChain->Present(1, 0);
```

이 예제에서 **m \_ 이 swapchain present** 는 [IDXGISwapChain1](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) 개체입니다. 이 개체의 초기화에 대 한 자세한 내용은이 문서의 [Direct3D 및 Direct2D 초기화](#initializing-direct3d-and-direct2d) 섹션을 참조 하십시오.

[Idxgiswapchain::P 재전송](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1)됨, *syncinterval*에 대 한 첫 번째 매개 변수는 프레임을 표시 하기 전에 대기할 세로 공백 수를 지정 합니다. 대리석은 다음 세로 공백이 될 때까지 대기 하도록 1을 지정 합니다.

[Idxgiswapchain::P 재전송](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) 메서드는 장치가 제거 되었거나 실패 한 경우를 나타내는 오류 코드를 반환 합니다. 이 경우 대리석은 장치를 다시 초기화 합니다.

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


입력 장치를 사용할 때 유의 해야 하는 몇 가지 주요 방법에 대 한 자세한 내용은 [대리석에 입력 및 상호 작용 추가](adding-input-and-interactivity-to-the-marble-maze-sample.md) 를 참조 하세요. 이 문서에서는 대리석에서 touch,가 속도계, Xbox 컨트롤러 및 마우스 입력을 지 원하는 방법을 설명 합니다.

## <a name="related-topics"></a>관련 항목


* [Marble Maze 샘플에 입력 및 대화형 작업 추가](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [Marble Maze 응용 프로그램 구조](marble-maze-application-structure.md)
* [C + + 및 c + +의 UWP 게임, 대리석](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 