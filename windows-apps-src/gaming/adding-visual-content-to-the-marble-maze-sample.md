---
author: mtoepke
title: "Marble Maze 샘플에 시각적 콘텐츠 추가"
description: "이 문서에서는 패턴을 학습하고 고유한 게임 콘텐츠로 작업할 때 적절하게 조정할 수 있도록 Marble Maze 게임이 UWP(유니버설 Windows 플랫폼) 앱 환경에서 Direct3D 및 Direct2D를 사용하는 방법을 설명합니다."
ms.assetid: 6e43422e-e1a1-b79e-2c4b-7d5b4fa88647
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 70f35fe423b8ceb3e3e3e0c1c3c2563dc0d8cd61

---

# Marble Maze 샘플에 시각적 콘텐츠 추가


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


이 문서에서는 패턴을 학습하고 고유한 게임 콘텐츠로 작업할 때 적절하게 조정할 수 있도록 Marble Maze 게임이 UWP(유니버설 Windows 플랫폼) 앱 환경에서 Direct3D 및 Direct2D를 사용하는 방법을 설명합니다. 시각적 게임 구성 요소가 Marble Maze의 전체 응용 프로그램 구조에 맞게 조정되는 방식에 대한 자세한 내용은 [Marble Maze 응용 프로그램 구조](marble-maze-application-structure.md)를 참조하세요.

Marble Maze의 시각적 측면을 개발할 때 다음과 같은 기본 단계를 따랐습니다.

1.  Direct3D 및 Direct2D 환경을 초기화하는 기본 프레임워크를 만듭니다.
2.  이미지 및 모델 편집 프로그램을 사용하여 게임에 표시되는 2D 및 3D 자산을 디자인합니다.
3.  2D 및 3D 자산이 게임에 제대로 로드되고 표시되는지 확인합니다.
4.  게임 자산의 시각적 품질을 향상시키는 꼭짓점 및 픽셀 셰이더를 통합합니다.
5.  애니메이션, 사용자 입력 등의 게임 논리를 통합합니다.

또한 3D 자산 추가에 먼저 집중한 다음 2D 자산에 집중했습니다. 예를 들어 메뉴 시스템과 타이머를 추가하기 전에 핵심 게임 논리에 집중했습니다.

또한 개발 과정에서 이러한 단계 중 일부를 여러 번 반복해야 했습니다. 예를 들어 메시 및 구슬 모델을 변경할 때 해당 모델을 지원하는 셰이더 코드 중 일부도 변경해야 했습니다.

> **참고** 이 문서에 해당하는 샘플 코드는 [DirectX Marble Maze 게임 샘플](http://go.microsoft.com/fwlink/?LinkId=624011)에 있습니다.

 
DirectX 및 시각적 게임 콘텐츠 작업을 하는 경우, 즉 DirectX 그래픽 라이브러리를 초기화하고, 장면 리소스를 로드하고, 장면을 업데이트 및 렌더링하는 경우에 대해 이 문서에서 논의하는 주요 사항은 다음과 같습니다.

-   일반적으로 게임 콘텐츠 추가는 여러 단계로 이루어집니다. 이러한 단계를 반복해야 할 수도 있습니다. 대체로 게임 개발자는 먼저 3D 게임 콘텐츠 추가에 집중한 다음 2D 콘텐츠 추가에 집중합니다.
-   가능한 한 광범위한 그래픽 하드웨어를 지원하여 더 많은 고객에 도달하고 모든 고객에게 효율적인 환경을 제공합니다.
-   디자인 타임 형식과 런타임 형식을 명확하게 구분합니다. 유연성을 최대화하고 콘텐츠를 신속하게 반복할 수 있도록 디자인 타임 자산을 구성합니다. 런타임에 가능한 한 효율적으로 로드 및 렌더링되도록 자산 형식을 지정하고 압축합니다.
-   클래식 Windows 데스크톱 앱과 거의 유사한 방식으로 UWP 앱에서 Direct3D 및 Direct2D 장치를 만듭니다. 중요한 차이점 중 하나는 스왑 체인이 출력 창과 연결되는 방식입니다.
-   게임을 디자인할 때 선택하는 메시 형식이 주요 시나리오를 지원하는지 확인합니다. 예를 들어 게임에 충돌이 필요한 경우 메시에서 충돌 데이터를 가져올 수 있는지 확인합니다.
-   렌더링하기 전에 모든 장면 개체를 먼저 업데이트하여 렌더링 논리에서 게임 논리를 분리합니다.
-   일반적으로 3D 장면 개체를 그린 다음 장면 앞에 표시되는 2D 개체를 그립니다.
-   수직 소거에 그리기를 동기화하여 게임이 실제로 디스플레이에 표시되지 않는 프레임을 그리는 데 시간을 소비하지 않도록 합니다.

## DirectX 그래픽으로 시작


Marble Maze UWP(유니버설 Windows 플랫폼) 게임을 계획할 때 C++ 및 Direct3D 11.1을 선택했습니다. 최대 렌더링 제어 및 고성능이 필요한 3D 게임을 만들기 위한 최선의 선택이기 때문입니다. DirectX 11.1은 DirectX 9에서 DirectX 11 사이의 하드웨어를 지원하며 이전 DirectX 버전에 대해 각각 코드를 다시 작성하지 않아도 되므로 보다 효율적으로 더 많은 고객에 도달하는 데 도움이 됩니다.

Marble Maze는 Direct3D 11.1을 사용하여 3D 게임 자산, 즉 구슬과 미로를 렌더링합니다. 또한 Marble Maze는 Direct2D, DirectWrite 및 WIC(Windows Imaging Component)를 사용하여 메뉴, 타이머 등의 2D 게임 자산을 그립니다. 마지막으로, Marble Maze에서는 XAML을 사용하여 앱 바를 제공하고 XAML 컨트롤을 추가할 수 있도록 합니다.

게임 개발에는 계획이 필요합니다. DirectX 그래픽을 처음 접하는 경우 UWP DirectX 게임 만들기의 기본 개념을 이해하기 위해 DirectX 게임 만들기를 읽어보는 것이 좋습니다. 이 문서를 읽고 Marble Maze 소스 코드를 진행할 때 DirectX 그래픽에 대한 자세한 내용은 다음 리소스를 참조할 수 있습니다.

-   [Direct3D 11 그래픽](https://msdn.microsoft.com/library/windows/desktop/ff476080) Windows 플랫폼에서 3D 기하 도형을 렌더링하기 위한 강력한 하드웨어 가속 3D 그래픽 API인 Direct3D 11에 대해 설명합니다.
-   [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990) 2D 기하 도형, 비트맵 및 텍스트에 대한 고성능 및 고품질 렌더링을 제공하는 하드웨어 가속 2D 그래픽 API인 Direct2D에 대해 설명합니다.
-   [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) 고품질 텍스트 렌더링을 지원하는 DirectWrite에 대해 설명합니다.
-   [Windows Imaging Component](https://msdn.microsoft.com/library/windows/desktop/ee719902) 디지털 이미지용 하위 수준 API를 제공하는 확장 가능한 플랫폼인 WIC에 대해 설명합니다.

### 기능 수준

Direct3D 11에서는 기능 수준이라는 패러다임을 소개합니다. 기능 수준은 잘 정의된 GPU 기능 집합입니다. 기능 수준을 사용하여 이전 버전의 Direct3D 하드웨어에서 실행되도록 게임 대상을 지정합니다. Marble Maze는 상위 수준의 고급 기능이 필요하지 않으므로 기능 수준 9.1을 지원합니다. 가능한 최대 범위의 하드웨어를 지원하고 고급 또는 저급 컴퓨터를 가진 고객이 모두 효율적인 환경을 사용할 수 있도록 게임 콘텐츠를 확장하는 것이 좋습니다. 기능 수준에 대한 자세한 내용은 [하위 수준 하드웨어의 Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476872)을 참조하세요.

## Direct3D 및 Direct2D 초기화


장치는 디스플레이 어댑터를 나타냅니다. 클래식 Windows 데스크톱 앱과 거의 유사한 방식으로 UWP 앱에서 Direct3D 및 Direct2D 장치를 만듭니다. 주요 차이점은 Direct3D 스왑 체인을 창 시스템에 연결하는 방법입니다.

*DirectX 11 및 XAML 앱(유니버설 Windows)*은 게임 관련 기능에서 일반적인 운영 체제 및 3D 렌더링 기능을 제외합니다. **DeviceResources** 클래스는 Direct3D 및 Direct2D 관리의 토대가 됩니다. 이 클래스는 게임 관련 자산이 아니라 일반적인 인프라를 처리합니다. Marble Maze에서는 게임 관련 자산을 처리하기 위한 **MarbleMaze** 클래스를 정의합니다. 이 클래스에는 **DeviceResources** 개체에 대한 참조가 포함되어 있어 Marble Maze가 Direct3D 및 Direct2D에 액세스할 수 있습니다.

초기화 중에 **DeviceResources::Initialize** 메서드는 장치 독립적인 리소스와 Direct3D 및 Direct2D 장치를 만듭니다.

```cpp
// Initialize the Direct3D resources required to run. 
void DeviceResources::DeviceResources(CoreWindow^ window, float dpi)
{
    m_window = window;

    CreateDeviceIndependentResources();
    CreateDeviceResources();
    CreateWindowSizeDependentResources();
    SetDpi(dpi);
}
```

**DeviceResources** 클래스는 환경이 변경될 때 더 쉽게 응답할 수 있도록 이 기능을 분리합니다. 예를 들어 창 크기가 변경될 때 **CreateWindowSizeDependentResources** 메서드를 호출합니다.

###  Direct2D, DirectWrite 및 WIC 팩터리 초기화

**DeviceResources::CreateDeviceIndependentResources** 메서드는 Direct2D, DirectWrite 및 WIC용 팩터리를 만듭니다. DirectX 그래픽에서 팩터리는 그래픽 리소스를 만들기 위한 시작점입니다. Marble Maze는 주 스레드에서 모든 그리기를 수행하므로 **D2D1\_FACTORY\_TYPE\_SINGLE\_THREADED**를 지정합니다.

```cpp
// These are the resources required independent of hardware. 
void DeviceResources::CreateDeviceIndependentResources()
{
    D2D1_FACTORY_OPTIONS options;
    ZeroMemory(&options, sizeof(D2D1_FACTORY_OPTIONS));

#if defined(_DEBUG)
     // If the project is in a debug build, enable Direct2D debugging via SDK Layers.
    options.debugLevel = D2D1_DEBUG_LEVEL_INFORMATION;
#endif

    DX::ThrowIfFailed(
        D2D1CreateFactory(
            D2D1_FACTORY_TYPE_SINGLE_THREADED,
            __uuidof(ID2D1Factory1),
            &options,
            &m_d2dFactory
            )
        );

    DX::ThrowIfFailed(
        DWriteCreateFactory(
            DWRITE_FACTORY_TYPE_SHARED,
            __uuidof(IDWriteFactory),
            &m_dwriteFactory
            )
        );

    DX::ThrowIfFailed(
        CoCreateInstance(
            CLSID_WICImagingFactory,
            nullptr,
            CLSCTX_INPROC_SERVER,
            IID_PPV_ARGS(&m_wicFactory)
            )
        );
}
```

###  Direct3D 및 Direct2D 장치 만들기

**DeviceResources::CreateDeviceResources** 메서드는 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082)를 호출하여 Direct3D 디스플레이 어댑터를 나타내는 디바이스 개체를 만듭니다. Marble Maze가 기능 수준 9.1 이상을 지원하므로 **DeviceResources::CreateDeviceResources** 메서드는 **\\** 값 배열에 수준 9.1에서 11.1을 지정합니다. Direct3D는 목록을 순서대로 검색하고 사용 가능한 첫 번째 기능 수준을 앱에 제공합니다. 따라서 **D3D\_FEATURE\_LEVEL** 배열 항목은 앱에 사용 가능한 최고 기능 수준이 지정되도록 최고 수준에서 최저 수준으로 나열됩니다. **DeviceResources::CreateDeviceResources** 메서드는 **D3D11CreateDevice**에서 반환된 Direct3D 11 장치를 쿼리하여 Direct3D 11.1 장치를 가져옵니다.

```cpp
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

// Create the DX11 API device object, and get a corresponding context.
ComPtr<ID3D11Device> device;
ComPtr<ID3D11DeviceContext> context;
DX::ThrowIfFailed(
    D3D11CreateDevice(
        nullptr,                    // Specify null to use the default adapter.
        D3D_DRIVER_TYPE_HARDWARE,
        0,                          // Leave as 0 unless it is a software device.
        creationFlags,              // Optionally, set debug and Direct2D compatibility flags.
        featureLevels,              // A list of feature levels that this app can support.
        ARRAYSIZE(featureLevels),   // The number of entries in the above list.
        D3D11_SDK_VERSION,          // Always set this to D3D11_SDK_VERSION for modern.
        &device,                    // Returns the Direct3D device created.
        &m_featureLevel,            // Returns the feature level of the device created.
        &context                    // Returns the device immediate context.
        )
    );    

// Get the Direct3D 11.1 device by querying the Direct3D 11 device.
DX::ThrowIfFailed(
    device.As(&m_d3dDevice)
    );
```

그런 다음 **DeviceResources::CreateDeviceResources** 메서드는 Direct2D 장치를 만듭니다. Direct2D는 Microsoft DXGI(DirectX Graphics Infrastructure)를 사용하여 Direct3D와 상호 작용합니다. DXGI를 사용하면 그래픽 런타임 간에 비디오 메모리 표면을 공유할 수 있습니다. Marble Maze는 Direct3D 장치의 기본 DXGI 장치를 사용하여 Direct2D 팩터리에서 Direct2D 장치를 만듭니다.

```cpp
// Obtain the underlying DXGI device of the Direct3D 11.1 device.
DX::ThrowIfFailed(
    m_d3dDevice.As(&dxgiDevice)
    );

// Obtain the Direct2D device for 2-D rendering.
DX::ThrowIfFailed(
    m_d2dFactory->CreateDevice(dxgiDevice.Get(), &m_d2dDevice)
    );

// And get its corresponding device context object.
DX::ThrowIfFailed(
    m_d2dDevice->CreateDeviceContext(
        D2D1_DEVICE_CONTEXT_OPTIONS_NONE,
        &m_d2dContext
        )
    );
```

```cpp
// Obtain the underlying DXGI device of the Direct3D 11.1 device.
DX::ThrowIfFailed(
    m_d3dDevice.As(&dxgiDevice)
    );

// Obtain the Direct2D device for 2-D rendering.
DX::ThrowIfFailed(
    m_d2dFactory->CreateDevice(dxgiDevice.Get(), &m_d2dDevice)
    );

// And get its corresponding device context object.
DX::ThrowIfFailed(
    m_d2dDevice->CreateDeviceContext(
        D2D1_DEVICE_CONTEXT_OPTIONS_NONE,
        &m_d2dContext
        )
    );
```

DXGI 및 Direct2D와 Direct3D 간의 상호 운용성에 대한 자세한 내용은 [DXGI 개요](https://msdn.microsoft.com/library/windows/desktop/bb205075) 및 [Direct2D 및 Direct3D 상호 운용성 개요](https://msdn.microsoft.com/library/windows/desktop/dd370966)를 참조하세요.

### Direct3D를 뷰에 연결

**DeviceResources::CreateWindowSizeDependentResources** 메서드는 스왑 체인, Direct3D 및 Direct2D 렌더링 대상 등 지정된 창 크기에 종속된 그래픽 리소스를 만듭니다. DirectX UWP 앱과 데스크톱 앱의 중요한 차이점 중 하나는 스왑 체인이 출력 창에 연결되는 방식입니다. 스왑 체인은 장치가 렌더링되는 버퍼를 모니터에 표시합니다. Marble Maze 응용 프로그램 구조 문서에서는 UWP 앱용 창 시스템과 데스크톱 앱 간의 차이점에 대해 설명합니다. Windows 스토어 앱은 **HWND** 개체에서 작동하지 않으므로 Marble Maze는 [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559) 메서드를 사용하여 디바이스 출력을 뷰에 연결해야 합니다. 다음 예제에서는 스왑 체인을 만드는 **DeviceResources::CreateWindowSizeDependentResources** 메서드의 일부를 보여 줍니다.

```cpp
// Obtain the final swap chain for this window from the DXGI factory.
DX::ThrowIfFailed(
    dxgiFactory->CreateSwapChainForCoreWindow(
        m_d3dDevice.Get(),
        reinterpret_cast<IUnknown*>(m_window),
        &swapChainDesc,
        nullptr,    // Allow on all displays.
        &m_swapChain
        )
    );
```

랩톱, 태블릿 등 배터리 전원을 사용하는 디바이스에서 중요한 전력 소비를 최소화하기 위해 **DeviceResources::CreateWindowSizeDependentResources** 메서드는 [**IDXGIDevice1::SetMaximumFrameLatency**](https://msdn.microsoft.com/library/windows/desktop/ff471334) 메서드를 호출하여 게임이 수직 소거 후에만 렌더링되도록 합니다. 수직 소거와 동기화는 이 문서의 장면 표시 섹션에서 자세히 설명합니다.

```cpp
// Ensure that DXGI does not queue more than one frame at a time. This both reduces  
// latency and ensures that the application will only render after each VSync, minimizing  
// power consumption.
DX::ThrowIfFailed(
    dxgiDevice->SetMaximumFrameLatency(1)
    );
```

**DeviceResources::CreateWindowSizeDependentResources** 메서드는 대부분의 게임에서 작동하는 방식으로 그래픽 리소스를 초기화합니다.

> **참고** *뷰* 용어는 Direct3D와 Windows 런타임에서 서로 다른 의미를 갖습니다. Windows 런타임에서 뷰는 표시 영역, 입력 동작, 처리에 사용하는 스레드를 비롯하여 앱의 사용자 인터페이스 설정 컬렉션을 가리킵니다. 뷰를 만들 때 필요한 구성과 설정을 지정합니다. 앱 뷰를 설정하는 프로세스는 [Marble Maze 응용 프로그램 구조](marble-maze-application-structure.md)에서 설명합니다. Direct3D에서 뷰 용어에는 여러 가지 의미가 있습니다. 첫째, 리소스 뷰는 리소스가 액세스할 수 있는 하위 리소스를 정의합니다. 예를 들어 텍스처 개체가 셰이더 리소스 뷰와 연결된 경우 나중에 해당 셰이더가 텍스처에 액세스할 수 있습니다. 리소스 뷰의 장점 중 하나는 렌더링 파이프라인의 각 단계에서 서로 다른 방식으로 데이터를 해석할 수 있다는 것입니다. 리소스 뷰에 대한 자세한 내용은 [텍스처 뷰(Direct3D 10)](https://msdn.microsoft.com/library/windows/desktop/bb205128)를 참조하세요. 뷰 변형 또는 뷰 변형 행렬의 컨텍스트에서 사용되는 경우 뷰는 카메라의 위치와 방향을 가리킵니다. 뷰 변형은 카메라의 위치 및 방향을 중심으로 개체 위치를 옮깁니다. 뷰 변형에 대한 자세한 내용은 [뷰 변형(Direct3D 9)](https://msdn.microsoft.com/library/windows/desktop/bb206342)을 참조하세요. 이 항목에서는 Marble Maze가 리소스 및 행렬 뷰를 사용하는 방식에 대해 자세히 설명합니다.

 

## 장면 리소스 로드


Marble Maze는 BasicLoader.h에 선언되어 있는 **BasicLoader** 클래스를 사용하여 텍스처 및 셰이더를 로드합니다. Marble Maze는 **SDKMesh** 클래스를 사용하여 미로와 구슬에 대한 3D 메시를 로드합니다.

반응형 앱이 되기 위해 Marble Maze는 비동기적으로 또는 백그라운드에서 장면 리소스를 로드합니다. 자산이 백그라운드에서 로드될 때 게임이 창 이벤트에 응답할 수 있습니다. 이 프로세스는 이 가이드의 [백그라운드에서 게임 자산 로드](marble-maze-application-structure.md#loading_game_assets)에서 자세히 설명합니다.

###  2D 오버레이 및 사용자 인터페이스 로드

Marble Maze에서 오버레이는 화면 맨 위에 표시되는 이미지입니다. 오버레이는 항상 장면 앞에 표시됩니다. Marble Maze에서 오버레이에는 Windows 로고 및 텍스트 문자열 "DirectX Marble Maze 게임 샘플"이 포함됩니다. 오버레이 관리는 SampleOverlay.h에 정의되어 있는 **SampleOverlay** 클래스에서 수행됩니다. Direct3D 샘플에 포함된 오버레이를 사용하지만 이 코드를 조정하여 임의 이미지를 장면 앞에 표시할 수 있습니다.

오버레이의 중요한 측면 중 하나는 콘텐츠가 변경되지 않으므로 **SampleOverlay** 클래스가 초기화 중에 해당 콘텐츠를 [**ID2D1Bitmap1**](https://msdn.microsoft.com/library/windows/desktop/hh404349) 개체에 그리거나 캐시한다는 것입니다. 그릴 때 **SampleOverlay** 클래스는 비트맵만 화면에 그리면 됩니다. 이렇게 하면 텍스트 그리기 등의 값비싼 루틴을 모든 프레임에 대해 수행하지 않아도 됩니다.

UI(사용자 인터페이스)는 장면 앞에 표시되는 메뉴, HUD(주의 표시) 등의 2D 구성 요소로 이루어집니다. Marble Maze는 다음 UI 요소를 정의합니다.

-   사용자가 게임을 시작하거나 최고 점수를 볼 수 있는 메뉴 항목
-   플레이가 시작되기 전에 3초 동안 카운트 다운하는 타이머
-   경과된 플레이 시간을 추적하는 타이머
-   가장 빠른 완료 시간을 나열하는 테이블
-   게임이 일시 중지되면 "일시 중지됨"으로 표시되는 텍스트

Marble Maze는 UserInterface.h에서 게임 관련 UI 요소를 정의합니다. Marble Maze는 **ElementBase** 클래스를 모든 UI 요소의 기본 형식으로 정의합니다. **ElementBase** 클래스는 UI 요소의 크기, 위치, 맞춤, 표시 여부 등의 속성을 정의합니다. 또한 요소를 업데이트하고 렌더링하는 방법을 제어합니다.

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

> **참고** XAML을 사용하면 시뮬레이션 및 전략 게임에 사용되는 것과 같은 복잡한 사용자 인터페이스를 더 쉽게 만들 수 있으므로 XAML을 사용하여 UI를 정의할지 여부를 고려합니다. DirectX UWP 게임에서 XAML로 사용자 인터페이스를 개발하는 방법에 대한 자세한 내용은 [게임 샘플 확장(Windows)](tutorial-resources.md)을 참조하세요. 이 문서는 DirectX 3D 슈팅 게임 샘플을 참조합니다.

 

###  로딩 셰이더

Marble Maze는 **BasicLoader::LoadShader** 메서드를 사용하여 파일에서 셰이더를 로드합니다.

셰이더는 최신 게임에서 GPU 프로그래밍의 기본 단위입니다. 거의 모든 3D 그래픽 처리는 모델 변환 및 장면 조명인지 또는 더 복잡한 기하 도형 처리인지에 관계없이 문자 스킨에서 공간 분할까지 셰이더에 의해 구동됩니다. 셰이더 프로그래밍 모델에 대한 자세한 내용은 [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561)을 참조하세요.

Marble Maze는 꼭짓점 및 픽셀 셰이더를 사용합니다. 꼭짓점 셰이더는 항상 하나의 입력 꼭짓점에서 작동하며 출력으로 하나의 꼭짓점을 생성합니다. 픽셀 셰이더는 숫자 값, 텍스처 데이터, 보간된 꼭짓점별 값 및 기타 데이터를 사용하여 출력으로 픽셀 색상을 생성합니다. 셰이더는 한 번에 하나의 요소를 변형하기 때문에 여러 셰이더 파이프라인을 제공하는 그래픽 하드웨어는 요소 집합을 병렬로 처리할 수 있습니다. GPU에서 사용할 수 있는 병렬 파이프라인 수가 CPU에서 사용할 수 있는 개수보다 훨씬 많을 수 있습니다. 따라서 기본 셰이더도 처리량을 훨씬 향상시킬 수 있습니다.

**MarbleMaze::LoadDeferredResources** 메서드는 오버레이를 로드한 후 꼭짓점 셰이더 1개와 픽셀 셰이더 1개를 로드합니다. 두 셰이더의 디자인 타임 버전은 각각 BasicVertexShader.hlsl 및 BasicPixelShader.hlsl에 정의되어 있습니다. Marble Maze는 렌더링 단계에서 이러한 셰이더를 구슬과 미로에 적용합니다.

Marble Maze 프로젝트에는 .hlsl(디자인 타임 형식) 및 .cso(런타임 형식) 버전의 셰이더 파일이 둘 다 포함되어 있습니다. 빌드할 때 Visual Studio는 fxc.exe 효과 컴파일러를 사용하여 .hlsl 소스 파일을 .cso 이진 셰이더로 컴파일합니다. 효과 컴파일러 도구에 대한 자세한 내용은 [효과 컴파일러 도구](https://msdn.microsoft.com/library/windows/desktop/bb232919)를 참조하세요.

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

> **주의** 컴파일된 픽셀 셰이더에는 산술 명령 32개와 텍스처 명령 1개가 포함되어 있습니다. 이 셰이더는 데스크톱 컴퓨터 및 고급 태블릿에서도 제대로 작동해야 합니다. 그러나 이 셰이더를 처리할 수 없는 저급 컴퓨터도 대화형 프레임 속도를 제공할 수 있습니다. 대상 사용자의 일반적인 하드웨어를 고려하고 해당 하드웨어의 기능에 맞게 셰이더를 디자인합니다.

 

**MarbleMaze::LoadDeferredResources** 메서드는 **BasicLoader::LoadShader** 메서드를 사용하여 셰이더를 로드합니다. 다음 예제에서는 꼭짓점 셰이더를 로드합니다. 이 셰이더의 런타임 형식은 BasicVertexShader.cso입니다. **m\_vertexShader** 멤버 변수는 [**ID3D11VertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476641) 개체입니다.

```cpp
\loader->LoadShader(
    L"BasicVertexShader.cso",
    layoutDesc,
    ARRAYSIZE(layoutDesc),
    &m_vertexShader,
    &m_inputLayout
    );
```

**m\_inputLayout** 멤버 변수는 [**ID3D11InputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476575) 개체입니다. 입력 레이아웃 개체는 IA(입력 어셈블러) 단계의 입력 상태를 캡슐화합니다. IA 단계의 작업 중 하나는 시스템에서 생성된 값(*의미 체계*라고도 함)을 사용하여 아직 처리되지 않은 기본 요소 또는 꼭짓점만 처리하도록 하여 셰이더를 더 효율적으로 만드는 것입니다. [**ID3D11Device::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512) 메서드를 사용하여 입력 요소 설명 배열에서 입력 레이아웃을 만듭니다. 배열에는 하나 이상의 입력 요소가 포함됩니다. 각 입력 요소는 단일 꼭짓점 버퍼의 꼭짓점 데이터 요소 1개에 대해 설명합니다. 전체 입력 요소 설명 집합은 IA 단계에 바인딩할 모든 꼭짓점 버퍼의 꼭짓점 데이터 요소를 모두 설명합니다. 다음 예제에서는 Marble Maze에 사용되는 레이아웃 설명을 보여 줍니다. 레이아웃 설명은 꼭짓점 데이터 요소 4개가 포함된 꼭짓점 버퍼를 설명합니다. 배열에 포함된 각 항목의 중요한 부분은 의미 체계 이름, 데이터 형식 및 바이트 오프셋입니다. 예를 들어 **POSITION** 요소는 개체 공간의 꼭짓점 위치를 지정합니다. 바이트 오프셋 0에서 시작하고 부동 소수점 구성 요소 3개(총 12바이트)를 포함합니다. **NORMAL** 요소는 법선 벡터를 지정합니다. 레이아웃에서 **POSITION** 바로 뒤에 표시되며 12바이트가 필요하기 때문에 바이트 오프셋 12에서 시작됩니다. **NORMAL** 요소는 4개 구성 요소로 이루어진 32비트 부호 없는 정수를 포함합니다.

```cpp
D3D11_INPUT_ELEMENT_DESC layoutDesc[] = 
{
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "NORMAL",   0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "TEXCOORD",  0, DXGI_FORMAT_R32G32_FLOAT,   0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "TANGENT", 0, DXGI_FORMAT_R32G32B32_FLOAT,  0, 32, D3D11_INPUT_PER_VERTEX_DATA, 0 }, 
};
m_vertexStride = 44; // You must set this to match the size of layoutDesc above.
```

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

[의미 체계](https://msdn.microsoft.com/library/windows/desktop/bb509647) 문서에서는 사용 가능한 각 의미 체계를 자세히 설명합니다.

> **참고** 레이아웃에서 여러 셰이더가 동일한 레이아웃을 공유할 수 있도록 사용되지 않는 추가 구성 요소를 지정할 수 있습니다. 예를 들어 **TANGENT** 요소는 셰이더에서 사용되지 않습니다. 일반 매핑 등의 기술을 사용하려는 경우 **TANGENT** 요소를 사용할 수 있습니다. 일반 매핑(범프 매핑이라고도 함)을 사용하여 개체 표면에 범프 효과를 만들 수 있습니다. 범프 매핑에 대한 자세한 내용은 [범프 매핑(Direct3D 9)](https://msdn.microsoft.com/library/windows/desktop/bb172379)을 참조하세요.

 

입력 어셈블리 단계 상태에 대한 자세한 내용은 [입력 어셈블러 단계](https://msdn.microsoft.com/library/windows/desktop/bb205116) 및 [입력 어셈블러 단계 시작](https://msdn.microsoft.com/library/windows/desktop/bb205117)을 참조하세요.

꼭짓점 및 픽셀 셰이더를 사용하여 장면을 렌더링하는 프로세스는 이 문서의 뒷부분에 있는 [장면 렌더링](#rendering_the_scene) 섹션에서 설명합니다.

### 상수 버퍼 만들기

Direct3D 버퍼는 데이터 컬렉션을 그룹화합니다. 상수 버퍼는 데이터를 셰이더에 전달하는 데 사용할 수 있는 버퍼의 한 종류입니다. Marble Maze는 상수 버퍼를 사용하여 모델(또는 월드) 뷰와 활성 장면 개체에 대한 프로젝션 행렬을 저장합니다.

다음 예제에서는 **MarbleMaze::LoadDeferredResources** 메서드가 나중에 행렬 데이터를 저장할 상수 버퍼를 만드는 방법을 보여 줍니다. 이 예제는 **D3D11\_BIND\_CONSTANT\_BUFFER** 플래그를 사용하여 사용량을 상수 버퍼로 지정하는 **D3D11\_BUFFER\_DESC** 구조를 만듭니다. 그런 다음 구조를 [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) 메서드에 전달합니다. **m\_constantBuffer** 변수는 [**ID3D11Buffer**](https://msdn.microsoft.com/library/windows/desktop/ff476351) 개체입니다.

```cpp
// Create the constant buffer for updating model and camera data.
D3D11_BUFFER_DESC constantBufferDesc = {0};
constantBufferDesc.ByteWidth           = ((sizeof(ConstantBuffer) + 15) / 16) * 16; // Multiple of 16 bytes
constantBufferDesc.Usage               = D3D11_USAGE_DEFAULT;
constantBufferDesc.BindFlags           = D3D11_BIND_CONSTANT_BUFFER;
constantBufferDesc.CPUAccessFlags      = 0;
constantBufferDesc.MiscFlags           = 0;
// This will not be used as a structured buffer, so this parameter is ignored.
constantBufferDesc.StructureByteStride = 0;

DX::ThrowIfFailed(
    m_d3dDevice->CreateBuffer(
        &constantBufferDesc,
        nullptr,             // Leave the buffer uninitialized.
        &m_constantBuffer
        )
    );
```

**MarbleMaze::Update** 메서드는 나중에 미로와 구슬에 대해 하나씩, **ConstantBuffer** 개체를 업데이트합니다. 각 개체가 렌더링되기 전에 **MarbleMaze::Render** 메서드는 각 **ConstantBuffer** 개체를 상수 버퍼에 바인딩합니다. 다음 예제에서는 MarbleMaze.h에 있는 **ConstantBuffer** 구조를 보여 줍니다.

```cpp
// Describes the constant buffer that draws the meshes.
struct ConstantBuffer
{
    float4x4 model;
    float4x4 view;
    float4x4 projection;

    float3 marblePosition;
    float marbleRadius;
    float lightStrength;
};
```

상수 버퍼가 셰이더 코드에 매핑되는 방식을 더 쉽게 이해하려면 BasicVertexShader.hlsl에서 꼭짓점 셰이더로 정의된 **SimpleConstantBuffer** 상수 버퍼와 **ConstantBuffer** 구조를 비교합니다.

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

**ConstantBuffer** 구조의 레이아웃은 **cbuffer** 개체와 일치합니다. **cbuffer** 변수는 레지스터 b0을 지정하며, 이는 상수 버퍼 데이터가 레지스터 0에 저장됨을 의미합니다. **MarbleMaze::Render** 메서드는 상수 버퍼를 활성화할 때 레지스터 0을 지정합니다. 이 프로세스는 이 문서의 뒷부분에서 자세히 설명합니다.

상수 버퍼에 대한 자세한 내용은 [Direct3D 11의 버퍼 소개](https://msdn.microsoft.com/library/windows/desktop/ff476898)를 참조하세요. register 키워드에 대한 자세한 내용은 [**register**](https://msdn.microsoft.com/library/windows/desktop/dd607359)를 참조하세요.

###  메시 로드

Marble Maze는 SDK 메시를 런타임 형식으로 사용합니다. 이 형식은 샘플 응용 프로그램에 대한 메시 데이터를 로드하는 기본 방법을 제공하기 때문입니다. 프로덕션 사용의 경우 게임의 특정 요구 사항을 충족하는 메시 형식을 사용해야 합니다.

**MarbleMaze::LoadDeferredResources** 메서드는 꼭짓점 및 픽셀 셰이더를 로드한 후 메시 데이터를 로드합니다. 메시는 대체로 위치, 법선 데이터, 색상, 재질, 텍스처 좌표 등의 정보를 포함하는 꼭짓점 데이터 컬렉션입니다. 일반적으로 메시는 3D 제작 소프트웨어에서 생성되며 응용 프로그램 코드와 분리된 파일에서 유지 관리됩니다. 구슬과 미로는 게임에 사용되는 메시의 두 가지 예입니다.

Marble Maze는 **SDKMesh** 클래스를 사용하여 메시를 관리합니다. 이 클래스는 SDKMesh.h에 선언되어 있습니다. **SDKMesh**에서는 메시 데이터를 로드, 렌더링 및 삭제하는 메서드를 제공합니다.

> **중요** Marble Maze는 SDK 메시 형식을 사용하고 설명 용도로만 **SDKMesh** 클래스를 제공합니다. SDK 메시 형식은 프로토타입을 만들고 학습하는 데 유용하지만 매우 기본적인 형식이므로 대부분의 게임 개발에 필요한 요구 사항을 충족할 수 없습니다. 게임의 특정 요구 사항을 충족하는 메시 형식을 사용하는 것이 좋습니다.

 

다음 예제에서는 **MarbleMaze::LoadDeferredResources** 메서드가 **SDKMesh::Create** 메서드를 사용하여 미로와 구슬에 대한 메시 데이터를 로드하는 방법을 보여 줍니다.

```cpp
// Load the meshes.
DX::ThrowIfFailed(
    m_mazeMesh.Create(
        m_d3dDevice.Get(),
        L"Media\\Models\\maze1.sdkmesh",
        false
        )
    );

DX::ThrowIfFailed(
    m_marbleMesh.Create(
        m_d3dDevice.Get(),
        L"Media\\Models\\marble2.sdkmesh",
        false
        )
    );
```

###  충돌 데이터 로드

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

충돌 데이터를 로드하는 방법은 대체로 사용하는 런타임 형식에 따라 달라집니다. Marble Maze가 SDK 메시 파일에서 충돌 기하 도형을 로드하는 방법에 대한 자세한 내용은 소스 코드의 **MarbleMaze::ExtractTrianglesFromMesh** 메서드를 참조하세요.

## 게임 상태 업데이트


Marble Maze는 렌더링하기 전에 모든 장면 개체를 먼저 업데이트하여 렌더링 논리에서 게임 논리를 분리합니다.

Marble Maze 응용 프로그램 구조 문서에서는 주 게임 루프에 대해 설명합니다. 게임 루프에 포함된 장면 업데이트는 Windows 이벤트 및 입력이 처리된 후, 장면이 렌더링되기 전에 발생합니다. **MarbleMaze::Update** 메서드는 UI 및 게임 업데이트를 처리합니다.

### 사용자 인터페이스 업데이트

**MarbleMaze::Update** 메서드는 **UserInterface::Update** 메서드를 호출하여 UI 상태를 업데이트합니다.

```cpp
UserInterface::GetInstance().Update(timeTotal, timeDelta);
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

**ElementBase**에서 파생되는 클래스는 **Update** 메서드를 구현하여 특정 동작을 수행합니다. 예를 들어 **StopwatchTimer::Update** 메서드는 경과 시간을 제공된 양만큼 업데이트하고 나중에 표시되는 텍스트를 업데이트합니다.

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

###  장면 업데이트

**MarbleMaze::Update** 메서드는 현재 상태 시스템의 상태에 따라 게임을 업데이트합니다. 게임이 활성 상태이면 Marble Maze는 카메라를 업데이트하여 구슬을 따르고, 상수 버퍼의 뷰 행렬 부분을 업데이트하고, 물리학 시뮬레이션을 업데이트합니다.

다음 예제에서는 **MarbleMaze::Update** 메서드가 카메라의 위치를 업데이트하는 방법을 보여 줍니다. Marble Maze는 **m\_resetCamera** 변수를 사용하여 카메라가 구슬 바로 위에 오도록 다시 설정해야 한다는 플래그를 지정합니다. 게임이 시작되거나 구슬이 미로에서 떨어지면 카메라가 다시 설정됩니다. 주 메뉴 또는 최고 점수 표시 화면이 활성화된 경우 카메라가 일정한 위치에 설정됩니다. 그렇지 않으면 Marble Maze는 *timeDelta* 매개 변수를 사용하여 현재 위치와 대상 위치 사이에서 카메라 위치를 보간합니다. 대상 위치는 구슬 앞에서 약간 위에 있습니다. 경과된 프레임 시간을 사용하면 카메라가 구슬을 점점 따라가거나 추적할 수 있습니다.

```cpp
static float eyeDistance = 200.0f;
static float3 eyePosition = float3(0, 0, 0);

// Gradually move the camera above the marble.
float3 targetEyePosition = marblePosition - (eyeDistance * float3(g.x, g.y, g.z));
if (m_resetCamera)
{
    eyePosition = targetEyePosition;
    m_resetCamera = false;
}
else
{
    eyePosition = eyePosition + ((targetEyePosition - eyePosition) * min(1, timeDelta * 8));
}

// Look at the marble. 
if ((m_gameState == GameState::MainMenu) || (m_gameState == GameState::HighScoreDisplay))
{
    // Override camera position for menus.
    eyePosition = marblePosition + float3(75.0f, -150.0f, -75.0f);
    m_camera->SetViewParameters(eyePosition, marblePosition, float3(0.0f, 0.0f, -1.0f));
}
else
{
    m_camera->SetViewParameters(eyePosition, marblePosition, float3(0.0f, 1.0f, 0.0f));
}
```

다음 예제에서는 **MarbleMaze::Update** 메서드가 구슬과 미로에 대한 상수 버퍼를 업데이트하는 방법을 보여 줍니다. 미로의 모델 또는 월드 행렬은 항상 항등 행렬로 유지됩니다. 요소가 모두 1인 주 대각을 제외하고 항등 행렬은 0으로 구성된 정방 행렬입니다. 구슬의 모델 행렬은 위치 행렬에 회전 행렬을 곱한 값을 기반으로 합니다. **mul** 및 **translation** 함수는 BasicMath.h에 정의되어 있습니다.

```cpp
// Update the model matrices based on the simulation.
m_mazeConstantBufferData.model = identity();
m_marbleConstantBufferData.model = mul(
    translation(marblePosition.x, marblePosition.y, marblePosition.z),
    marbleRotationMatrix
    );

// Update the view matrix based on the camera.
float4x4 view;
m_camera->GetViewMatrix(&view);
m_mazeConstantBufferData.view = view;
m_marbleConstantBufferData.view = view;
```

**MarbleMaze::Update** 메서드가 사용자 입력을 읽고 구슬의 동작을 시뮬레이트하는 방법에 대한 자세한 내용은 [Marble Maze 샘플에 입력 및 대화형 작업 추가](adding-input-and-interactivity-to-the-marble-maze-sample.md)를 참조하세요.

## 장면 렌더링


장면을 렌더링하면 일반적으로 다음 단계가 포함됩니다.

1.  현재 렌더링 대상 깊이 스텐실 버퍼를 설정합니다.
2.  렌더링 및 스텐실 뷰의 선택을 취소합니다.
3.  그리기를 위해 꼭짓점 및 픽셀 셰이더를 준비합니다.
4.  장면에서 3D 개체를 렌더링합니다.
5.  장면 앞에 표시하려는 2D 개체를 렌더링합니다.
6.  렌더링된 이미지를 모니터에 표시합니다.

**MarbleMaze::Render** 메서드는 렌더링 대상 및 깊이 스텐실 뷰를 바인딩하고, 해당 뷰의 선택을 취소하고, 장면을 그리고, 오버레이를 그립니다.

###  렌더링 대상 준비

장면을 렌더링하기 전에 현재 렌더링 대상 깊이 스텐실 버퍼를 설정해야 합니다. 장면이 화면의 모든 픽셀 위에 그려진다고 보장할 수 없는 경우 렌더링 및 스텐실 뷰의 선택도 취소합니다. Marble Maze는 모든 프레임에서 렌더링 및 스텐실 뷰의 선택을 취소하여 이전 프레임의 아티팩트가 표시되지 않도록 합니다.

다음 예제에서는 **MarbleMaze::Render** 메서드가 [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464) 메서드를 호출하여 렌더링 대상과 깊이 스텐실 버퍼를 현재 항목으로 설정하는 방법을 보여 줍니다. **m\_renderTargetView** 멤버 변수인 [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) 개체와 **m\_depthStencilView** 멤버 변수인 [**ID3D11DepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476377) 개체는 **DirectXBase** 클래스에 의해 정의되고 초기화됩니다.

```cpp
// Bind the render targets.
m_d3dContext->OMSetRenderTargets(
    1,
    m_renderTargetView.GetAddressOf(),
    m_depthStencilView.Get()
    );

// Clear the render target and depth stencil to default values. 
const float clearColor[4] = { 0.0f, 0.0f, 0.0f, 1.0f };

m_d3dContext->ClearRenderTargetView(
    m_renderTargetView.Get(),
    clearColor
    );

m_d3dContext->ClearDepthStencilView(
    m_depthStencilView.Get(),
    D3D11_CLEAR_DEPTH,
    1.0f,
    0
    );
```

[**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) 및 [**ID3D11DepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476377) 인터페이스는 Direct3D 10 이상에서 제공되는 텍스처 뷰 메커니즘을 지원합니다. 텍스처 뷰에 대한 자세한 내용은 [텍스처 뷰(Direct3D 10)](https://msdn.microsoft.com/library/windows/desktop/bb205128)를 참조하세요. [**OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464) 메서드는 Direct3D 파이프라인의 출력 병합 단계를 준비합니다. 출력 병합 단계에 대한 자세한 내용은 [출력 병합 단계](https://msdn.microsoft.com/library/windows/desktop/bb205120)를 참조하세요.

### 꼭짓점 및 픽셀 셰이더 준비

장면 개체를 렌더링하기 전에 다음 단계를 수행하여 그리기를 위해 꼭짓점 및 픽셀 셰이더를 준비합니다.

1.  셰이더 입력 레이아웃을 현재 레이아웃으로 설정합니다.
2.  꼭짓점 및 픽셀 셰이더를 현재 셰이더로 설정합니다.
3.  상수 버퍼를 셰이더에 전달해야 하는 데이터로 업데이트합니다.

> **중요** Marble Maze는 한 쌍의 꼭짓점 및 픽셀 셰이더를 모든 3D 개체에 사용합니다. 게임에 두 쌍 이상의 셰이더가 사용되는 경우 다른 셰이더를 사용하는 개체를 그릴 때마다 다음 단계를 수행해야 합니다. 셰이더 상태 변경과 연결된 오버헤드를 줄이기 위해 동일한 셰이더를 사용하는 모든 개체에 대한 렌더링 호출을 그룹화하는 것이 좋습니다.

 

이 문서의 [셰이더 로드](#loading_shaders) 섹션에서는 꼭짓점 셰이더를 만들 때 입력 레이아웃이 생성되는 방식에 대해 설명합니다. 다음 예제에서는 **MarbleMaze::Render** 메서드가 [**ID3D11DeviceContext::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454) 메서드를 사용하여 이 레이아웃을 현재 레이아웃으로 설정하는 방법을 보여 줍니다.

```cpp
m_d3dContext->IASetInputLayout(m_inputLayout.Get());
```

다음 예제에서는 **MarbleMaze::Render** 메서드가 [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493) 및 [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) 메서드를 사용하여 각각 꼭짓점 및 픽셀 셰이더를 현재 셰이더로 설정하는 방법을 보여 줍니다.

```cpp
// Set the vertex shader stage state.
m_d3dContext->VSSetShader(
    m_vertexShader.Get(),   // Use this vertex shader. 
    nullptr,                // Don't use shader linkage.
    0                       // Don't use shader linkage.
    );

// Set the pixel shader stage state.
m_d3dContext->PSSetShader(
    m_pixelShader.Get(),    // Use this pixel shader. 
    nullptr,                // Don't use shader linkage.
    0                       // Don't use shader linkage.
    );

m_d3dContext->PSSetSamplers(
    0,                       // Starting at the first sampler slot
    1,                       // set one sampler binding
    m_sampler.GetAddressOf() // to use this sampler.
    );
```

**MarbleMaze::Render**는 셰이더 및 입력 레이아웃을 설정한 후 [**ID3D11DeviceContext::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476486) 메서드를 사용하여 미로에 대한 모델, 뷰 및 프로젝션 행렬로 상수 버퍼를 업데이트합니다. **UpdateSubresource** 메서드는 CPU 메모리에서 GPU 메모리로 행렬 데이터를 복사합니다. **ConstantBuffer** 구조의 모델 및 뷰 구성 요소는 **MarbleMaze::Update** 메서드에서 업데이트됩니다. 그런 다음 **MarbleMaze::Render** 메서드는 [**ID3D11DeviceContext::VSSetConstantBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476491) 및 [**ID3D11DeviceContext::PSSetConstantBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476470) 메서드를 호출하여 이 상수 버퍼를 현재 항목으로 설정합니다.

```cpp
// Update the constant buffer with the new data.
m_d3dContext->UpdateSubresource(
    m_constantBuffer.Get(),
    0,
    nullptr,
    &m_mazeConstantBufferData,
    0,
    0
    );

m_d3dContext->VSSetConstantBuffers(
    0,                // Starting at the first constant buffer slot
    1,                // set one constant buffer binding
    m_constantBuffer.GetAddressOf() // to use this buffer.
    );

m_d3dContext->PSSetConstantBuffers(
    0,                // Starting at the first constant buffer slot
    1,                // set one constant buffer binding
    m_constantBuffer.GetAddressOf() // to use this buffer.
    );
```

**MarbleMaze::Render** 메서드는 유사한 단계를 수행하여 렌더링할 구슬을 준비합니다.

### 미로 및 구슬 렌더링

현재 셰이더를 활성화한 후 장면 개체를 그릴 수 있습니다. **MarbleMaze::Render** 메서드는 **SDKMesh::Render** 메서드를 호출하여 미로 메시를 렌더링합니다.

```cpp
m_mazeMesh.Render(m_d3dContext.Get(), 0, INVALID_SAMPLER_SLOT, INVALID_SAMPLER_SLOT);
```

**MarbleMaze::Render** 메서드는 유사한 단계를 수행하여 구슬을 렌더링합니다.

이 문서의 앞부분에서 설명한 것처럼 **SDKMesh** 클래스는 데모 용도로만 제공되고 프로덕션 품질 게임에서 사용하지 않는 것이 좋습니다. 그러나 **SDKMesh::Render**가 호출하는 **SDKMesh::RenderMesh** 메서드는 [**ID3D11DeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456) 및 [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476453) 메서드를 사용하여 메시를 정의하는 현재 꼭짓점 및 인덱스 버퍼를 설정하고 [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476410) 메서드를 사용하여 버퍼를 그립니다. 꼭짓점 및 인덱스 버퍼로 작업하는 방법에 대한 자세한 내용은 [Direct3D 11의 버퍼 소개](https://msdn.microsoft.com/library/windows/desktop/ff476898)를 참조하세요.

### 사용자 인터페이스 및 오버레이 그리기

Marble Maze는 3D 장면 개체를 그린 후 장면 앞에 표시되는 2D UI 요소를 그립니다.

**MarbleMaze::Render** 메서드는 사용자 인터페이스와 오버레이를 그려서 종료합니다.

```cpp
// Draw the user interface and the overlay.
UserInterface::GetInstance().Render();

m_sampleOverlay->Render();
```

**UserInterface::Render** 메서드는 [**ID2D1DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/hh404479)개체를 사용하여 UI 요소를 그립니다. 이 메서드는 그리기 상태를 설정하고, 모든 활성 UI 요소를 그리고, 이전 그리기 상태를 복원합니다.

```cpp
void UserInterface::Render()
{
    m_d2dContext->SaveDrawingState(m_stateBlock.Get());
    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    m_d2dContext->SetTextAntialiasMode(D2D1_TEXT_ANTIALIAS_MODE_GRAYSCALE);

    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        if ((*iter)->IsVisible())
            (*iter)->Render();
    }

    m_d2dContext->EndDraw();
    m_d2dContext->RestoreDrawingState(m_stateBlock.Get());
}
```

**SampleOverlay::Render** 메서드는 유사한 기술을 사용하여 오버레이 비트맵을 그립니다.

###  장면 표시

Marble Maze는 모든 2D 및 3D 장면 개체를 그린 후 렌더링된 이미지를 모니터에 표시합니다. 수직 소거에 그리기를 동기화하여 실제로 디스플레이에 표시되지 않는 프레임을 그리는 데 시간을 소비하지 않도록 합니다. Marble Maze는 장면을 표시할 때 장치 변경도 처리합니다.

**MarbleMaze::Render** 메서드가 반환된 후 게임 루프는 **MarbleMaze::Present** 메서드를 호출하여 렌더링된 이미지를 모니터 또는 디스플레이에 보냅니다. **MarbleMaze** 클래스는 **DirectXBase::Present** 메서드를 재정의하지 않습니다. **DirectXBase::Present** 메서드는 다음 예제와 같이 [**IDXGISwapChain1::Present**](https://msdn.microsoft.com/library/windows/desktop/hh446797)를 호출하여 표시 작업을 수행합니다.

```cpp
// The application may optionally specify "dirty" or "scroll" rects 
// to improve efficiency in certain scenarios. 
// In this sample, however, we do not utilize those features.
DXGI_PRESENT_PARAMETERS parameters = {0};
parameters.DirtyRectsCount = 0;
parameters.pDirtyRects = nullptr;
parameters.pScrollRect = nullptr;
parameters.pScrollOffset = nullptr;

// The first argument instructs DXGI to block until VSync, putting the  
// application to sleep until the next VSync.  
// This ensures we don't waste any cycles rendering frames that will  
// never be displayed to the screen.
HRESULT hr = m_swapChain->Present1(1, 0, &parameters);
```

이 예제에서 **m\_swapChain**은 [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631) 개체입니다. 이 개체의 초기화는 이 문서의 [Direct3D 및 Direct2D 초기화](#initializing) 섹션에서 설명합니다.

[**IDXGISwapChain1::Present**](https://msdn.microsoft.com/library/windows/desktop/hh446797)의 첫 번째 매개 변수인 *SyncInterval*은 프레임을 표시하기 전에 기다릴 수직 소거 수를 지정합니다. Marble Maze는 다음 수직 소거까지 기다리도록 1을 지정합니다. 수직 소거는 한 프레임이 모니터에 그리기를 완료한 후 다음 프레임이 시작되기까지의 시간입니다.

[**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797) 메서드는 장치가 제거되었거나 다른 방식으로 실패했음을 나타내는 오류 코드를 반환합니다. 이 경우 Marble Maze는 장치를 다시 초기화합니다.

```cpp
// Reinitialize the renderer if the device was disconnected  
// or the driver was upgraded. 
if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
{
    Initialize(m_window, m_dpi);
}
else
{
    DX::ThrowIfFailed(hr);
}
```

## 다음 단계


입력 장치 작업을 할 때 고려할 몇 가지 주요 사항에 대한 자세한 내용은 [Marble Maze 샘플에 입력 및 대화형 작업 추가](adding-input-and-interactivity-to-the-marble-maze-sample.md)를 참조하세요. 이 문서에서는 Marble Maze가 터치, 가속도계, Xbox 360 컨트롤러 및 마우스 입력을 지원하는 방법을 설명합니다.

## 관련 항목


* [Marble Maze 샘플에 입력 및 대화형 작업 추가](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [Marble Maze 응용 프로그램 구조](marble-maze-application-structure.md)
* [C++ 및 DirectX로 UWP 게임 Marble Maze 개발](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 







<!--HONumber=Aug16_HO3-->


