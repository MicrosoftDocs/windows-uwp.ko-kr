---
title: DirectX 및 XAML interop
description: UWP (유니버설 Windows 플랫폼) 게임에서 Extensible Application Markup Language (XAML) 및 Microsoft DirectX를 함께 사용할 수 있습니다.
ms.assetid: 0fb2819a-61ed-129d-6564-0b67debf5c6b
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx, xaml interop
ms.localizationpriority: medium
ms.openlocfilehash: fc5e5323f509759754822849bd7dc93e7a45eaee
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156477"
---
# <a name="directx-and-xaml-interop"></a>DirectX 및 XAML interop

Extensible Application Markup Language (XAML) 및 Microsoft DirectX를 유니버설 Windows 플랫폼 (UWP) 게임 또는 앱에서 함께 사용할 수 있습니다. XAML과 DirectX를 함께 사용 하면 DirectX에서 렌더링 된 콘텐츠와 상호 작용 하는 유연한 사용자 인터페이스 프레임 워크를 빌드할 수 있으며, 특히 그래픽을 많이 사용 하는 앱에 유용 합니다. 이 항목에서는 DirectX를 사용 하는 UWP 앱의 구조를 설명 하 고, DirectX를 사용 하기 위해 UWP 앱을 빌드할 때 사용할 중요 한 유형을 식별 합니다.

앱이 주로 2D 렌더링을 중심으로 하는 경우 [Win2D](https://github.com/microsoft/win2d) Windows 런타임 라이브러리를 사용 하는 것이 좋습니다. 이 라이브러리는 Microsoft에서 유지 관리 하며 핵심 Direct2D 기술을 기반으로 구축 되었습니다. 이를 통해 2D 그래픽을 구현 하는 데 사용 패턴이 크게 간소화 되 고이 문서에서 설명 하는 기술 중 일부에 대 한 유용한 추상화가 포함 됩니다. 자세한 내용은 프로젝트 페이지를 참조 하세요. 이 문서에서는 Win2D를 사용 *하지 않도록* 선택 하는 앱 개발자를 위한 지침을 다룹니다.

> [!NOTE]
> DirectX Api는 Windows 런타임 형식으로 정의 되지 않지만 일반적으로 [c + +/Winrt](../cpp-and-winrt-apis/index.md) 를 사용 하 여 directx와 상호 작용 하는 XAML UWP 구성 요소를 개발할 수 있습니다. 또한 별도의 Windows 런타임 메타 데이터 파일에서 DirectX 호출을 래핑하는 경우 DirectX를 사용 하는 c # 및 XAML을 사용 하 여 UWP 앱을 만들 수 있습니다.

## <a name="xaml-and-directx"></a>XAML 및 DirectX

DirectX는 각각 2D와 3D 그래픽에 대한 강력한 라이브러리인 Direct2D 및 Microsoft Direct3D를 제공합니다. XAML은 기본 2D 기본 및 효과에 대 한 지원을 제공 하지만 모델링 및 게임과 같은 많은 앱에는 더 복잡 한 그래픽 지원이 필요 합니다. 이러한 경우 Direct2D 및 Direct3D를 사용 하 여 그래픽의 일부 또는 전체를 렌더링 하 고 다른 모든 항목에 대해 XAML을 사용할 수 있습니다.

사용자 지정 XAML 및 DirectX interop를 구현 하는 경우 다음과 같은 두 가지 개념을 알고 있어야 합니다.

-   공유 화면은 XAML로 정의 된 디스플레이의 크기가 조정 된 영역으로, [Windows:: UI:: XAML:: Media:: ImageSource](/uwp/api/windows.ui.xaml.media.imagesource) types를 사용 하 여 DirectX를 간접적으로 그릴 수 있습니다. 공유 표면의 경우 새 콘텐츠가 화면에 표시 되는 정확한 타이밍을 제어 하지 않습니다. 대신 공유 화면에 대 한 업데이트는 XAML 프레임 워크의 업데이트와 동기화 됩니다.
-   [스왑 체인](/windows/desktop/direct3d9/what-is-a-swap-chain-) 은 최소 대기 시간에 그래픽을 표시 하는 데 사용 되는 버퍼의 컬렉션을 나타냅니다. 일반적으로 스왑 체인은 UI 스레드와 별도로 60 프레임에서 초당 업데이트 됩니다. 그러나 스왑 체인은 신속한 업데이트를 지원 하기 위해 더 많은 메모리와 CPU 리소스를 사용 하므로 여러 스레드를 관리 해야 하므로 사용 하기가 더 어려워집니다.

에서 DirectX를 사용 하는 것을 고려 합니다. 표시 창의 크기에 맞는 단일 컨트롤을 합성 하거나 애니메이션 효과를 주는 데 사용 됩니까? 게임에서처럼 실시간으로 렌더링 및 제어해야 하는 출력을 포함하나요? 그럴 경우 스왑 체인을 구현 해야 할 것입니다. 그렇지 않으면 공유 표면을 사용 해야 합니다.

DirectX를 사용 하려는 방법을 결정 한 후에는 다음 Windows 런타임 형식 중 하나를 사용 하 여 DirectX 렌더링을 UWP 앱에 통합 합니다.

-   정적 이미지를 작성 하거나 이벤트 기반 간격에 복잡 한 이미지를 그리려면 [Windows:: UI:: Xaml:: Media:: Imaging:: SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource)를 사용 하 여 공유 화면에 그립니다. 이 형식은 크기가 지정 된 DirectX 그리기 화면을 처리 합니다. 일반적으로 이미지나 질감을 문서 또는 UI 요소에 표시 하기 위해 비트맵으로 작성할 때이 형식을 사용 합니다. 고성능 게임과 같은 실시간 상호 작용에는 제대로 작동 하지 않습니다. 이는 **SurfaceImageSource** 개체에 대 한 업데이트가 XAML 사용자 인터페이스 업데이트와 동기화 되기 때문 이며, 변동 프레임 속도나 실시간 입력에 대 한 잘못 된 응답과 같이 사용자에 게 제공 하는 시각적 피드백에 대기 시간이 발생할 수 있습니다. 그러나 동적 컨트롤 또는 데이터 시뮬레이션에 대 한 업데이트는 여전히 빠르게 진행 됩니다.

-   이미지가 제공 된 화면 부동산 보다 크고 사용자가 따라 이동 하거나 확대할 수 있는 경우 [Windows:: UI:: Xaml:: Media:: Imaging:: VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource)를 사용 합니다. 이 형식은 화면 보다 큰 크기가 지정 된 DirectX 그리기 화면을 처리 합니다. [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource)와 마찬가지로 복잡 한 이미지 또는 컨트롤을 동적으로 작성 하는 경우이를 사용 합니다. 또한 **SurfaceImageSource**와 마찬가지로 고성능 게임에서는 제대로 작동 하지 않습니다. **VirtualSurfaceImageSource** 를 사용할 수 있는 XAML 요소의 몇 가지 예는 지도 컨트롤 또는 이미지 조밀한 많은 문서 뷰어입니다.

-   DirectX를 사용 하 여 실시간으로 그래픽을 업데이트 하거나 업데이트를 정기적인 대기 시간 간격으로 제공 해야 하는 경우에는 [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) 클래스를 사용 하 여 XAML 프레임 워크 새로 고침 타이머와 동기화 하지 않고 그래픽을 새로 고칠 수 있습니다. 이 형식을 사용 하면 그래픽 장치의[IDXGISwapChain1](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1)(스왑 체인)에 직접 액세스 하 여 렌더링 대상의 맨 위에 있는 레이어 XAML에 액세스할 수 있습니다. 이 형식은 XAML 기반 사용자 인터페이스가 필요한 게임과 전체 화면 DirectX 앱에 적합 합니다. Microsoft의 DXGI (DirectX Graphics Infrastructure), Direct2D 및 Direct3D 기술을 포함 하 여이 접근 방식을 사용 하려면 DirectX 웰을 알아야 합니다. 자세한 내용은 [Direct3D 11에 대 한 프로그래밍 가이드](/windows/desktop/direct3d11/dx-graphics-overviews)를 참조 하세요.

## <a name="surfaceimagesource"></a>SurfaceImageSource

[SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) 는 응용 프로그램 콘텐츠로 비트를 그린 후 작성 하기 위한 DirectX 공유 표면을 제공 합니다.

다음은 코드 숨김으로 [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) 개체를 만들고 업데이트 하는 기본 프로세스입니다.

1.  [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) 생성자에 높이와 너비를 전달 하 여 공유 표면의 크기를 정의 합니다. 표면에 알파 (불투명도) 지원이 필요한 지 여부를 나타낼 수도 있습니다.

    예:

    `SurfaceImageSource^ surfaceImageSource = ref new SurfaceImageSource(400, 300);`

2.  [ISurfaceImageSourceNativeWithD2D](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d)에 대 한 포인터를 가져옵니다. [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) 개체를 [IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (또는 **IUnknown**)로 캐스팅 하 고 해당 개체에 대해 **QueryInterface** 를 호출 하 여 기본 **ISurfaceImageSourceNativeWithD2D** 구현을 가져옵니다. 이 구현에 정의 된 메서드를 사용 하 여 장치를 설정 하 고 그리기 작업을 실행 합니다.

    ```cpp
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNativeWithD2D> m_sisNativeWithD2D;

    // ...

    IInspectable* sisInspectable = 
        (IInspectable*) reinterpret_cast<IInspectable*>(surfaceImageSource);

    sisInspectable->QueryInterface(
        __uuidof(ISurfaceImageSourceNativeWithD2D), 
        (void **)&m_sisNativeWithD2D);
    ```

3.  먼저 [D3D11CreateDevice](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 및 [D2D1CreateDevice](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nf-d2d1_1-d2d1createdevice) 를 호출 하 고 장치 및 컨텍스트를 [ISurfaceImageSourceNativeWithD2D:: SETDEVICE](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d-setdevice)에 전달 하 여 DXGI 및 D2D 장치를 만듭니다. 

    > [!NOTE]
    > 백그라운드 스레드에서 **SurfaceImageSource** 으로 그리면 DXGI 장치에서 다중 스레드 액세스를 사용 하도록 설정 했는지 확인 해야 합니다. 성능상의 이유로 백그라운드 스레드에서 그릴 경우에만이 작업을 수행 해야 합니다.

    예:

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device> m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext> m_d3dContext;
    Microsoft::WRL::ComPtr<ID2D1Device> m_d2dDevice;

    // Create the DXGI device
    D3D11CreateDevice(
            NULL,
            D3D_DRIVER_TYPE_HARDWARE,
            NULL,
            flags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &m_d3dDevice,
            NULL,
            &m_d3dContext);

    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    // To enable multi-threaded access (optional)
    Microsoft::WRL::ComPtr<ID3D10Multithread> d3dMultiThread;

    m_d3dDevice->QueryInterface(
        __uuidof(ID3D10Multithread), 
        (void **) &d3dMultiThread);

    d3dMultiThread->SetMultithreadProtected(TRUE);

    // Create the D2D device
    D2D1CreateDevice(m_dxgiDevice.Get(), NULL, &m_d2dDevice);

    // Set the D2D device
    m_sisNativeWithD2D->SetDevice(m_d2dDevice.Get());
    ```

4.  [ID2D1DeviceContext](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgisurface) 개체에 대 한 포인터를 [ISurfaceImageSourceNativeWithD2D:: begindraw](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d-begindraw)에 제공 하 고, 반환 된 그리기 컨텍스트를 사용 하 여 **SurfaceImageSource**내에서 원하는 사각형의 콘텐츠를 그립니다. **ISurfaceImageSourceNativeWithD2D:: BeginDraw** 및 그리기 명령을 백그라운드 스레드에서 호출할 수 있습니다. *UpdateRect* 매개 변수에서 update에 지정 된 영역만 그려집니다.

    이 메서드는 *offset* 매개 변수에 업데이트 된 대상 사각형의 점 (x, y) 오프셋을 반환 합니다. 이 오프셋을 사용 하 여 **ID2D1DeviceContext**으로 업데이트 된 콘텐츠를 그릴 위치를 결정 합니다.

    ```cpp
    Microsoft::WRL::ComPtr<ID2D1DeviceContext> drawingContext;

    HRESULT beginDrawHR = m_sisNative->BeginDraw(
        updateRect, 
        &drawingContext, 
        &offset);

    if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED 
        || beginDrawHR == DXGI_ERROR_DEVICE_RESET
        || beginDrawHR == D2DERR_RECREATE_TARGET)
    {
        // The D3D and D2D device was lost and need to be re-created.
        // Recovery steps are:
        // 1) Re-create the D3D and D2D devices
        // 2) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the new D2D
        //    device
        // 3) Redraw the contents of the SurfaceImageSource
    }
    else if (beginDrawHR == E_SURFACE_CONTENTS_LOST)
    {
        // The devices were not lost but the entire contents of the surface
        // were. Recovery steps are:
        // 1) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the D2D 
        //    device again
        // 2) Redraw the entire contents of the SurfaceImageSource
    }
    else 
    {
        // Draw your updated rectangle with the drawingContext
    }
    ```

5. [ISurfaceImageSourceNativeWithD2D:: EndDraw](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d-enddraw) 를 호출 하 여 비트맵을 완성 합니다. 비트맵을 XAML [이미지](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) 또는 [ImageBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush)의 소스로 사용할 수 있습니다. **ISurfaceImageSourceNativeWithD2D:: EndDraw** 는 UI 스레드에서만 호출 해야 합니다.

    ```cpp
    m_sisNative->EndDraw();

    // ...
    // The SurfaceImageSource object's underlying 
    // ISurfaceImageSourceNativeWithD2D object contains the completed bitmap.

    ImageBrush^ brush = ref new ImageBrush();
    brush->ImageSource = surfaceImageSource;
    ```

    > [!NOTE]
    > [SurfaceImageSource:: SetSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsource) ( **IBitmapSource:: SetSource**에서 상속)를 호출 하면 현재 예외가 throw 됩니다. [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) 개체에서 호출 하지 마세요.

    > [!NOTE]
    > 응용 프로그램은 연결 된 [창이](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) 숨겨진 상태에서 **SurfaceImageSource** 로의 그리기를 방지 해야 합니다. 그렇지 않으면 **ISurfaceImageSourceNativeWithD2D** api가 실패 합니다. 이를 수행 하려면 [VisibilityChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) 이벤트에 대 한 이벤트 수신기로 등록 하 여 표시 여부 변경을 추적 합니다.

## <a name="virtualsurfaceimagesource"></a>VirtualSurfaceImageSource

[VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) 은 콘텐츠가 화면에 맞게 조정 될 수 있는 것 보다 클 수 있는 경우 [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) 를 확장 하므로 최적의 상태로 렌더링 하려면 콘텐츠를 가상화 해야 합니다.

[VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) 는 화면에 표시 될 때 표면의 영역을 업데이트 하기 위해 구현 하는 [IVirtualSurfaceImageSourceCallbacksNative:: UpdatesNeeded](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceupdatescallbacknative-updatesneeded)콜백을 사용 한다는 점에서 [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) 와 다릅니다. XAML 프레임 워크에서이를 처리 하므로 숨겨진 영역을 지울 필요가 없습니다.

다음은 코드 숨김으로 [VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) 개체를 만들고 업데이트 하는 기본 프로세스입니다.

1.  원하는 크기를 사용 하 여 [VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) 의 인스턴스를 만듭니다. 예:

    ```cpp
    VirtualSurfaceImageSource^ virtualSIS = 
        ref new VirtualSurfaceImageSource(2000, 2000);
    ```

2.  [IVirtualSurfaceImageSourceNative](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative) 및 [ISurfaceImageSourceNativeWithD2D](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d)에 대 한 포인터를 가져옵니다. [VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) 개체를 [IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) 또는 [IUnknown](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)으로 캐스팅 하 고 해당 개체에 대해 [QueryInterface](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) 를 호출 하 여 기본 **IVirtualSurfaceImageSourceNative** 및 **ISurfaceImageSourceNativeWithD2D** 구현을 가져옵니다. 이러한 구현에 정의 된 메서드를 사용 하 여 장치를 설정 하 고 그리기 작업을 실행 합니다.

    ```cpp
    Microsoft::WRL::ComPtr<IVirtualSurfaceImageSourceNative>  m_vsisNative;
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNativeWithD2D> m_sisNativeWithD2D;

    // ...

    IInspectable* vsisInspectable = 
        (IInspectable*) reinterpret_cast<IInspectable*>(virtualSIS);

    vsisInspectable->QueryInterface(
        __uuidof(IVirtualSurfaceImageSourceNative), 
        (void **) &m_vsisNative);

    vsisInspectable->QueryInterface(
        __uuidof(ISurfaceImageSourceNativeWithD2D), 
        (void **) &m_sisNativeWithD2D);
    ```

3.  먼저 **D3D11CreateDevice** 및 **D2D1CreateDevice**를 호출 하 여 DXGI 및 d2d 장치를 만든 다음, D2D 장치를 **ISurfaceImageSourceNativeWithD2D:: setdevice**에 전달 합니다.

    > [!NOTE]
    > 백그라운드 스레드에서 **VirtualSurfaceImageSource** 으로 그리면 DXGI 장치에서 다중 스레드 액세스를 사용 하도록 설정 했는지 확인 해야 합니다. 성능상의 이유로 백그라운드 스레드에서 그릴 경우에만이 작업을 수행 해야 합니다.

    예:

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device> m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext> m_d3dContext;
    Microsoft::WRL::ComPtr<ID2D1Device> m_d2dDevice;

    // Create the DXGI device
    D3D11CreateDevice(
            NULL,
            D3D_DRIVER_TYPE_HARDWARE,
            NULL,
            flags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &m_d3dDevice,
            NULL,
            &m_d3dContext);  

    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);
    
    // To enable multi-threaded access (optional)
    Microsoft::WRL::ComPtr<ID3D10Multithread> d3dMultiThread;

    m_d3dDevice->QueryInterface(
        __uuidof(ID3D10Multithread), 
        (void **) &d3dMultiThread);

    d3dMultiThread->SetMultithreadProtected(TRUE);

    // Create the D2D device
    D2D1CreateDevice(m_dxgiDevice.Get(), NULL, &m_d2dDevice);

    // Set the D2D device
    m_vsisNativeWithD2D->SetDevice(m_d2dDevice.Get());

    m_vsisNative->SetDevice(dxgiDevice.Get());
    ```

4.  [IVirtualSurfaceUpdatesCallbackNative](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-ivirtualsurfaceupdatescallbacknative)의 구현에 대 한 참조를 전달 하 여 [IVirtualSurfaceImageSourceNative:: RegisterForUpdatesNeeded](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-registerforupdatesneeded)를 호출 합니다.

    ```cpp
    class MyContentImageSource : public IVirtualSurfaceUpdatesCallbackNative
    {
    // ...
      private:
         virtual HRESULT STDMETHODCALLTYPE UpdatesNeeded() override;
    }

    // ...

    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
      // .. Perform drawing here ...
    }

    void MyContentImageSource::Initialize()
    {
      // ...
      m_vsisNative->RegisterForUpdatesNeeded(this);
      // ...
    }
    ```

    이 프레임 워크는 [VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) 의 영역을 업데이트 해야 할 때 [IVirtualSurfaceUpdatesCallbackNative:: UpdatesNeeded](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-registerforupdatesneeded) 의 구현을 호출 합니다.

    이는 프레임 워크에서 그려야 하는 영역을 결정 하는 경우 (예: 사용자가 화면 보기를 계획 하거나 확대/축소 하는 경우) 또는 앱이 해당 지역에서 [IVirtualSurfaceImageSourceNative:: 무효화할](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-invalidate) 를 호출한 경우에 발생할 수 있습니다.

5.  [IVirtualSurfaceImageSourceNative:: UpdatesNeeded](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceupdatescallbacknative-updatesneeded)에서 [IVirtualSurfaceImageSourceNative:: GetUpdateRectCount](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-getupdaterectcount) 및 [IVirtualSurfaceImageSourceNative:: GetUpdateRects](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-getupdaterects) 메서드를 사용 하 여 표면의 영역을 그려야 하는지 확인 합니다.

    ```cpp
    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
        HRESULT hr = S_OK;

        try
        {
            ULONG drawingBoundsCount = 0;  
            m_vsisNative->GetUpdateRectCount(&drawingBoundsCount);

            std::unique_ptr<RECT[]> drawingBounds(
                new RECT[drawingBoundsCount]);

            m_vsisNative->GetUpdateRects(
                drawingBounds.get(), 
                drawingBoundsCount);
            
            for (ULONG i = 0; i < drawingBoundsCount; ++i)
            {
                // Drawing code here ...
            }
        }
        catch (Platform::Exception^ exception)
        {
            hr = exception->HResult;
        }

        return hr;
    }
    ```

6.  마지막으로 업데이트 해야 하는 각 지역에 대해 다음을 수행 합니다.

    1.  **ID2D1DeviceContext** 개체에 대 한 포인터를 **ISurfaceImageSourceNativeWithD2D:: begindraw**에 제공 하 고, 반환 된 그리기 컨텍스트를 사용 하 여 **SurfaceImageSource**내에서 원하는 사각형의 콘텐츠를 그립니다. **ISurfaceImageSourceNativeWithD2D:: BeginDraw** 및 그리기 명령을 백그라운드 스레드에서 호출할 수 있습니다. *UpdateRect* 매개 변수에서 update에 지정 된 영역만 그려집니다.

        이 메서드는 *offset* 매개 변수에 업데이트 된 대상 사각형의 점 (x, y) 오프셋을 반환 합니다. 이 오프셋을 사용 하 여 **ID2D1DeviceContext**으로 업데이트 된 콘텐츠를 그릴 위치를 결정 합니다.

        ```cpp
        Microsoft::WRL::ComPtr<ID2D1DeviceContext> drawingContext;

        HRESULT beginDrawHR = m_sisNative->BeginDraw(
            updateRect, 
            &drawingContext, 
            &offset);

        if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED 
            || beginDrawHR == DXGI_ERROR_DEVICE_RESET 
            || beginDrawHR == D2DERR_RECREATE_TARGET)
        {
            // The D3D and D2D devices were lost and need to be re-created.
            // Recovery steps are:
            // 1) Re-create the D3D and D2D devices
            // 2) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the 
            //    new D2D device
            // 3) Redraw the contents of the VirtualSurfaceImageSource
        }
        else if (beginDrawHR == E_SURFACE_CONTENTS_LOST)
        {
            // The devices were not lost but the entire contents of the 
            // surface were lost. Recovery steps are:
            // 1) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the 
            //    D2D device again
            // 2) Redraw the entire contents of the VirtualSurfaceImageSource
        }
        else
        {
            // Draw your updated rectangle with the drawingContext
        }
        ```

    2.  특정 콘텐츠를 해당 영역에 그리면 더 나은 성능을 위해 드로잉을 제한 된 영역으로 제한 합니다.

    3.  **ISurfaceImageSourceNativeWithD2D:: EndDraw**를 호출 합니다. 결과는 비트맵입니다.

> [!NOTE]
> 응용 프로그램은 연결 된 [창이](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) 숨겨진 상태에서 **SurfaceImageSource** 로의 그리기를 방지 해야 합니다. 그렇지 않으면 **ISurfaceImageSourceNativeWithD2D** api가 실패 합니다. 이를 수행 하려면 [VisibilityChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) 이벤트에 대 한 이벤트 수신기로 등록 하 여 표시 여부 변경을 추적 합니다.

## <a name="swapchainpanel-and-gaming"></a>SwapChainPanel 및 게임


[SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) 는 스왑 체인을 직접 관리 하는 고성능 그래픽과 게임을 지원 하도록 설계 된 Windows 런타임 유형입니다. 이 경우 고유한 DirectX 스왑 체인을 만들고 렌더링 된 콘텐츠의 프레젠테이션을 관리 합니다.

성능 향상을 위해 [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) 유형에 대 한 특정 제한 사항이 있습니다.

-   앱 당 [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) 인스턴스가 4 개 미만입니다.
-   DirectX 스왑 체인의 높이 및 너비 ( [DXGI \_ 스왑 \_ 체인 \_ DESC1](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1))를 스왑 체인 요소의 현재 차원으로 설정 해야 합니다. 그렇지 않은 경우 디스플레이 콘텐츠의 크기가 조정 됩니다 ( **DXGI \_ 크기 \_ 스트레치**사용).
-   DirectX 스왑 체인의 크기 조정 모드 ( [dxgi \_ 스왑 \_ 체인 \_ DESC1](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1))를 **dxgi \_ 크기 조정 \_ 스트레치**로 설정 해야 합니다.
-   [IDXGIFactory2:: CreateSwapChainForComposition](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcomposition)를 호출 하 여 DirectX 스왑 체인을 만들어야 합니다.

XAML 프레임 워크의 업데이트는 아니라 앱의 요구 사항에 따라 [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) 를 업데이트 합니다. **SwapChainPanel** 의 업데이트를 XAML 프레임 워크의 업데이트와 동기화 해야 하는 경우 [WINDOWS:: UI:: XAML:: Media:: CompositionTarget:: 렌더링](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.compositiontarget.rendering) 이벤트에 등록 합니다. 그렇지 않으면 **SwapChainPanel**를 업데이트 하는 것과 다른 스레드에서 XAML 요소를 업데이트 하려고 할 때 크로스 스레드 문제를 고려해 야 합니다.

**SwapChainPanel**에 대 한 대기 시간이 짧은 포인터 입력을 수신 해야 하는 경우 [SwapChainPanel:: CreateCoreIndependentInputSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.swapchainpanel.createcoreindependentinputsource)를 사용 합니다. 이 메서드는 백그라운드 스레드에서 최소 대기 시간으로 입력 이벤트를 수신 하는 데 사용할 수 있는 [CoreIndependentInputSource](https://docs.microsoft.com/uwp/api/windows.ui.core.coreindependentinputsource) 개체를 반환 합니다. 이 메서드가 호출 되 면 모든 입력이 백그라운드 스레드로 리디렉션되도록 일반적인 XAML 포인터 입력 이벤트가 **SwapChainPanel**에 대해 발생 하지 않습니다.


> **참고**   일반적으로 DirectX 앱은 가로 방향으로 스왑 체인을 만들고 표시 창 크기 (일반적으로 대부분의 Microsoft Store 게임에서 기본 화면 해상도)와 동일 해야 합니다. 이렇게 하면 응용 프로그램에 XAML 오버레이가 표시 되지 않는 경우 최적의 스왑 체인 구현을 사용 합니다. 앱이 세로 모드로 회전 된 경우 앱은 기존 스왑 체인에서 [IDXGISwapChain1:: SetRotation](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-setrotation) 을 호출 하 고 필요한 경우 콘텐츠에 변환을 적용 한 다음 동일한 스왑 체인에서 [SetSwapChain](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-iswapchainpanelnative-setswapchain) 를 다시 호출 해야 합니다. 마찬가지로 [Idxgiswapchain:: ResizeBuffers](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers)를 호출 하 여 스왑 체인의 크기를 조정할 때마다 앱이 동일한 스왑 체인에서 **SetSwapChain** 를 다시 호출 해야 합니다.


 

다음은 코드 숨김으로 [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) 개체를 만들고 업데이트 하는 기본 프로세스입니다.

1.  앱에 대 한 스왑 체인 패널의 인스턴스를 가져옵니다. 인스턴스는 태그를 사용 하 여 XAML에 표시 됩니다 `<SwapChainPanel>` .

    `Windows::UI::Xaml::Controls::SwapChainPanel^ swapChainPanel;`

    예제 태그는 다음과 같습니다 `<SwapChainPanel>` .

    ```xml
    <SwapChainPanel x:Name="swapChainPanel">
        <SwapChainPanel.ColumnDefinitions>
            <ColumnDefinition Width="300*"/>
            <ColumnDefinition Width="1069*"/>
        </SwapChainPanel.ColumnDefinitions>
    …
    ```

2.  [ISwapChainPanelNative](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative)에 대 한 포인터를 가져옵니다. [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) 개체를 [IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (또는 **IUnknown**)로 캐스팅 하 고 해당 개체에 대해 **QueryInterface** 를 호출 하 여 기본 **ISwapChainPanelNative** 구현을 가져옵니다.

    ```cpp
    Microsoft::WRL::ComPtr<ISwapChainPanelNative> m_swapChainNative;
    // ...
    IInspectable* panelInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(swapChainPanel);
    panelInspectable->QueryInterface(__uuidof(ISwapChainPanelNative), (void **)&m_swapChainNative);
    ```

3.  DXGI 장치와 스왑 체인을 만들고 스왑 체인을 [SetSwapChain](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-iswapchainpanelnative-setswapchain)에 전달 하 여 [ISwapChainPanelNative](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative) 로 설정 합니다.

    ```cpp
    Microsoft::WRL::ComPtr<IDXGISwapChain1>               m_swapChain;    
    // ...
    DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};
            swapChainDesc.Width = m_bounds.Width;
            swapChainDesc.Height = m_bounds.Height;
            swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;           // This is the most common swapchain format.
            swapChainDesc.Stereo = false; 
            swapChainDesc.SampleDesc.Count = 1;                          // Don't use multi-sampling.
            swapChainDesc.SampleDesc.Quality = 0;
            swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
            swapChainDesc.BufferCount = 2;
            swapChainDesc.Scaling = DXGI_SCALING_STRETCH;
            swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // We recommend using this swap effect for all. applications
            swapChainDesc.Flags = 0;
                    
    // QI for DXGI device
    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    // Get the DXGI adapter.
    Microsoft::WRL::ComPtr<IDXGIAdapter> dxgiAdapter;
    dxgiDevice->GetAdapter(&dxgiAdapter);

    // Get the DXGI factory.
    Microsoft::WRL::ComPtr<IDXGIFactory2> dxgiFactory;
    dxgiAdapter->GetParent(__uuidof(IDXGIFactory2), &dxgiFactory);
    // Create a swap chain by calling CreateSwapChainForComposition.
    dxgiFactory->CreateSwapChainForComposition(
                m_d3dDevice.Get(),
                &swapChainDesc,
                nullptr,        // Allow on any display. 
                &m_swapChain
                );
            
    m_swapChainNative->SetSwapChain(m_swapChain.Get());
    ```

4.  DirectX 스왑 체인에 그리고 내용을 표시 하기 위해 표시 합니다.

    ```cpp
    HRESULT hr = m_swapChain->Present(1, 0);
    ```

    Windows 런타임 레이아웃/렌더링 논리가 업데이트를 신호로 만들면 XAML 요소가 새로 고쳐집니다.

## <a name="related-topics"></a>관련 항목

* [Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm)
* [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource)
* [VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource)
* [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)
* [ISwapChainPanelNative](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative)
* [Direct3D 11 프로그래밍 가이드](/windows/desktop/direct3d11/dx-graphics-overviews)