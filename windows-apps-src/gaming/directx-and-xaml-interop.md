---
title: DirectX 및 XAML interop
description: UWP(유니버설 Windows 플랫폼) 게임에서 XAML(Extensible Application Markup Language)과 Microsoft DirectX를 함께 사용할 수 있습니다.
ms.assetid: 0fb2819a-61ed-129d-6564-0b67debf5c6b
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx, xaml 상호 운용성
ms.localizationpriority: medium
ms.openlocfilehash: ad03a86ba18f11d8d63c2c98649e7f159f3d4f52
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321295"
---
# <a name="directx-and-xaml-interop"></a>DirectX 및 XAML interop



UWP(유니버설 Windows 플랫폼) 게임 또는 앱에서 XAML(Extensible Application Markup Language)과 Microsoft DirectX를 함께 사용할 수 있습니다. XAML과 DirectX를 함께 사용하면 DirectX로 렌더링된 콘텐츠와 상호 운용되는 유연한 사용자 인터페이스 프레임워크를 빌드할 수 있으며 이 방법은 그래픽을 많이 사용하는 앱에 특히 유용합니다. 이 항목에서는 DirectX를 사용하는 UWP 앱의 구조를 설명하고, DirectX와 함께 작동하도록 UWP 앱을 빌드할 때 사용할 중요 유형을 식별합니다.

앱이 주로 2D 렌더링에 중점을 두는 경우 [Win2D](https://github.com/microsoft/win2d) Windows 런타임 라이브러리를 사용하려고 할 수 있습니다. 이 라이브러리는 Microsoft에서 유지 관리하며, 핵심 Direct2D 기술을 기반으로 합니다. 이 라이브러리는 2D 그래픽을 구현하는 사용 패턴을 크게 간소화하며 이 문서에 설명된 기법 중 일부에 대한 유용한 추상화를 포함하고 있습니다. 자세한 내용은 프로젝트 페이지를 참조하세요. 이 문서에서는 Win2D를 사용*하지* 않도록 선택한 앱 개발자를 위한 지침을 다룹니다.

> **참고**  DirectX Api 정의 되어 있지 않은 Windows 런타임 형식으로 시각적 개체를 일반적으로 사용 되므로 C++ 구성 요소 확장 (C++/CX) DirectX를 사용 하 여 상호 운용 되는 XAML UWP 구성 요소를 개발할 수 있습니다. 또한 DirectX 호출을 개별 Windows 런타임 메타데이터 파일에 래핑할 경우 DirectX를 사용하는 C# 및 XAML로 UWP 앱을 만들 수 있습니다.

 

## <a name="xaml-and-directx"></a>XAML 및 DirectX

DirectX는 2D 및 3D 그래픽에 대 한 두 가지 강력한 라이브러리를 제공합니다. Direct2D 및 Microsoft Direct3D 합니다. XAML이 기본적인 2D 기능 및 효과, 모델링 및 게임 등의 여러 앱을 제공하지만 보다 복잡한 그래픽 지원이 필요합니다. 따라서 Direct2D 및 Direct3D를 사용하여 그래픽 전체 또는 일부를 렌더링하고 XAML을 사용하여 그 외 모든 작업을 수행할 수 있습니다.

사용자 지정 XAML 및 DirectX interop을 구현하는 경우 다음의 두 개념에 대해 알고 있어야 합니다.

-   공유 표면은 XAML로 정의된 크기 조정되는 표시 영역입니다. 즉, DirectX를 사용하여 [Windows::UI::Xaml::Media::ImageSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imagesource) 유형으로 간접적으로 그릴 수 있는 영역입니다. 공유 표면의 경우 새 콘텐츠가 화면에 표시될 때에 대한 정확한 타이밍을 제어하지 못합니다. 대신, 공유 표면에 대한 업데이트는 XAML 프레임워크의 업데이트와 동기화됩니다.
-   [스왑 체인](https://docs.microsoft.com/windows/desktop/direct3d9/what-is-a-swap-chain-)은 최소 대기 시간에 그래픽을 표시하는 데 사용되는 버퍼의 컬렉션을 나타냅니다. 일반적으로 스왑 체인은 UI 스레드에서 별도로 초당 60프레임으로 업데이트됩니다. 그러나 스왑 체인은 빠른 업데이트를 지원하기 위해 더 많은 메모리와 CPU 리소스를 사용하며, 사용자는 여러 스레드를 관리해야 하므로 사용하기가 더욱 어렵습니다.

DirectX를 사용하여 어떤 작업을 하려는 것인지 고려해 봅니다. 표시 창 크기에 맞는 단일 컨트롤을 합성하거나 애니메이션 효과를 주기 위해 사용하나요? 게임에서처럼 실시간으로 렌더링 및 제어해야 하는 출력을 포함하나요? 그런 경우 스왑 체인을 구현해야 할 수 있습니다. 그러지 않으면 공유 표면을 세밀하게 사용해야 합니다.

DirectX를 사용할 방법을 결정했으면 다음 Windows 런타임 형식 중 하나를 사용하여 DirectX 렌더링을 UWP 앱에 통합합니다.

-   정적 이미지를 작성하거나 이벤트 구동 간격으로 복잡한 이미지를 그리려는 경우 [Windows::UI::Xaml::Media::Imaging::SurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource)를 사용하여 공유 표면에 그립니다. 이 유형은 크기가 조정되는 DirectX 그리기 표면을 처리합니다. 일반적으로 이미지나 텍스처를 문서 또는 UI 요소에 표시할 비트맵으로 작성할 때 이 유형을 사용하며, 고성능 게임 같은 실시간 대화형 작업에는 잘 맞지 않습니다. **SurfaceImageSource** 개체에 대한 업데이트는 XAML 사용자 인터페이스 업데이트와 동기화되므로 가변 프레임 속도 또는 실시간 입력에 대한 응답 사용자에게 제공하는 시각적 피드백에 지연이 발생할 수 있기 때문입니다. 그렇지만 동적 컨트롤이나 데이터 시뮬레이션을 하는 데는 무리 없는 정도로 빠른 업데이트 속도를 제공합니다.

-   이미지가 제공된 화면 크기보다 크고 사용자가 이를 회전하거나 확대/축소할 수 있는 경우 [Windows::UI::Xaml::Media::Imaging::VirtualSurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource)를 사용합니다. 이 유형은 화면보다 큰 크기가 조정되는 DirectX 그리기 표면을 처리합니다. [SurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource)와 마찬가지로, 복잡한 이미지나 컨트롤을 동적으로 작성할 때 이 유형을 사용할 수 있습니다. 그리고 **SurfaceImageSource**처럼, 고성능 게임에는 적합하지 않습니다. **VirtualSurfaceImageSource**를 사용할 수 있는 XAML 요소의 예는 맵 컨트롤 또는 이미지가 많고 크기가 큰 문서 뷰어입니다.

-   정기적인 짧은 대기 시간 간격으로 업데이트가 수행되어야 하는 상황에서, 또는 업데이트된 그래픽을 실시간으로 나타내는 데 DirectX를 사용하는 경우 XAML 프레임워크 새로 고침 타이머와 동기화하지 않고 그래픽을 새로 고칠 수 있도록 [SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) 클래스를 사용하세요. 이 유형을 사용하면 그래픽 디바이스의 스왑 체인([IDXGISwapChain1](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1))에 직접 액세스할 수 있고 렌더링 대상의 최상위 XAML 레이어에 액세스할 수 있습니다. 이 유형은 XAML 기반 사용자 인터페이스가 필요한 게임 및 전체 화면 DirectX 앱에서 잘 작동합니다. 이 방법을 사용하려면 Microsoft DXGI(DirectX Graphics Infrastructure), Direct2D 및 Direct3D 기술을 포함하여 DirectX에 대해 잘 알고 있어야 합니다. 자세한 내용은 [Direct3D 11의 프로그래밍 지침](https://docs.microsoft.com/windows/desktop/direct3d11/dx-graphics-overviews)을 참조하세요.

## <a name="surfaceimagesource"></a>SurfaceImageSource


[SurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource)는 그릴 DirectX 공유 표면을 제공하고 비트를 앱 콘텐츠로 작성합니다.

다음은 코드 숨김으로 [SurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) 개체를 만들고 업데이트하는 기본 프로세스입니다.

1.  [SurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) 생성자에 높이와 너비를 전달하여 공유 표면의 크기를 정의합니다. 표면에 알파(불투명도) 지원이 필요한지 여부도 지정할 수 있습니다.

    예를 들어 다음과 같은 가치를 제공해야 합니다.

    `SurfaceImageSource^ surfaceImageSource = ref new SurfaceImageSource(400, 300);`

2.  [ISurfaceImageSourceNativeWithD2D](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d)에 대한 포인터를 가져옵니다. [SurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) 개체를 [IInspectable](https://docs.microsoft.com/windows/desktop/api/inspectable/nn-inspectable-iinspectable)(또는 **IUnknown**)로 캐스트하고 그 위에서 **QueryInterface**를 호출하여 기본 **ISurfaceImageSourceNativeWithD2D** 구현을 가져옵니다. 이 구현에 정의된 메서드를 사용하여 디바이스를 설정하고 그리기 작업을 실행합니다.

    ```cpp
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNativeWithD2D> m_sisNativeWithD2D;

    // ...

    IInspectable* sisInspectable = 
        (IInspectable*) reinterpret_cast<IInspectable*>(surfaceImageSource);

    sisInspectable->QueryInterface(
        __uuidof(ISurfaceImageSourceNativeWithD2D), 
        (void **)&m_sisNativeWithD2D);
    ```

3.  먼저 [D3D11CreateDevice](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 및 [D2D1CreateDevice](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nf-d2d1_1-d2d1createdevice)를 호출한 후 [ISurfaceImageSourceNativeWithD2D::SetDevice](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d-setdevice)로 디바이스 및 컨텍스트를 전달하여 DXGI 및 D2D 디바이스를 만듭니다. 

    > [!NOTE]
    > 백그라운드 스레드에서 **SurfaceImageSource**에 그릴 경우 DXGI 디바이스에 다중 스레드 액세스가 활성화되어 있는지도 확인해야 합니다. 이는 백그라운드 스레드에서 그릴 경우에만 성능을 위해 취해야 하는 조치입니다.

    예를 들어 다음과 같은 가치를 제공해야 합니다.

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

4.  [ISurfaceImageSourceNativeWithD2D::BeginDraw](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d-begindraw)에 [ID2D1DeviceContext](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgisurface) 개체에 대한 포인터를 제공하고 반환된 그리기 컨텍스트를 사용하여 **SurfaceImageSource** 내에 원하는 사각형의 콘텐츠를 그립니다. **ISurfaceImageSourceNativeWithD2D::BeginDraw**와 그리기 명령은 백그라운드 스레드에서 호출할 수 있습니다. *updateRect* 매개 변수에서 업데이트에 대해 지정한 영역에만 그려집니다.

    이 메서드는 *offset* 매개 변수에서 업데이트된 대상 직사각형의 점(x,y) 오프셋을 반환합니다. **ID2D1DeviceContext**에서 이 오프셋을 사용하여 업데이트된 콘텐츠를 그릴 위치를 결정할 수 있습니다.

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

5. [ISurfaceImageSourceNativeWithD2D::EndDraw](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d-enddraw)를 호출하여 비트맵을 완료합니다. 비트맵은 XAML [이미지](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) 또는 [ImageBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush)의 원본으로 사용될 수 있습니다. **ISurfaceImageSourceNativeWithD2D::EndDraw**는 UI 스레드에서만 호출해야 합니다.

    ```cpp
    m_sisNative->EndDraw();

    // ...
    // The SurfaceImageSource object's underlying 
    // ISurfaceImageSourceNativeWithD2D object contains the completed bitmap.

    ImageBrush^ brush = ref new ImageBrush();
    brush->ImageSource = surfaceImageSource;
    ```

    > [!NOTE]
    > 현재 [SurfaceImageSource::SetSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsource)(**IBitmapSource::SetSource**에서 상속됨)를 호출하면 예외가 발생합니다. [SurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) 개체에서 이 속성을 호출하지 마세요.

    > [!NOTE]
    > 연결된 [창](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window)이 숨겨져 있을 때는 응용 프로그램이 **SurfaceImageSource**에 그리지 말아야 합니다. 그러지 않으면 **ISurfaceImageSourceNativeWithD2D** API가 실패합니다. 이 작업을 수행하려면 가시성 변경 사항을 추적하는 [Window.VisibilityChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) 이벤트에 대한 이벤트 수신기로 등록합니다.

## <a name="virtualsurfaceimagesource"></a>VirtualSurfaceImageSource

[VirtualSurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource)는 콘텐츠가 화면에 맞출 수 있는 크기보다 잠재적으로 더 클 때 [SurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource)를 확장하며 최적으로 렌더링하려면 콘텐츠를 가상화해야 합니다.

[VirtualSurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource)는 표면의 영역이 화면에 보이게 되면 이를 업데이트하도록 구현하는 콜백 [IVirtualSurfaceImageSourceCallbacksNative::UpdatesNeeded](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceupdatescallbacknative-updatesneeded)을 사용한다는 점에서 [SurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource)와 다릅니다. XAML 프레임워크가 개발자 대신 수행해 주는 것처럼 숨겨진 영역을 지울 필요는 없습니다.

다음은 코드 숨김으로 [VirtualSurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) 개체를 만들고 업데이트하는 기본 프로세스입니다.

1.  원하는 크기로 [VirtualSurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource)의 인스턴스를 만듭니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

    ```cpp
    VirtualSurfaceImageSource^ virtualSIS = 
        ref new VirtualSurfaceImageSource(2000, 2000);
    ```

2.  [IVirtualSurfaceImageSourceNative](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative) 및 [ISurfaceImageSourceNativeWithD2D](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d)에 대한 포인터를 가져옵니다. [VirtualSurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) 개체를 [IInspectable](https://docs.microsoft.com/windows/desktop/api/inspectable/nn-inspectable-iinspectable) 또는 [IUnknown](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)으로 캐스트하고 그 위에서 [QueryInterface](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))를 호출하여 기본 **IVirtualSurfaceImageSourceNative** 및 **ISurfaceImageSourceNativeWithD2D** 구현을 가져옵니다. 이 구현에 정의된 메서드를 사용하여 디바이스를 설정하고 그리기 작업을 실행합니다.

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

3.  먼저 **D3D11CreateDevice** 및 **D2D1CreateDevice**를 호출한 후 **ISurfaceImageSourceNativeWithD2D::SetDevice**에 D2D 디바이스를 전달하여 DXGI 및 D2D 디바이스를 만듭니다.

    > [!NOTE]
    > 백그라운드 스레드에서 **VirtualSurfaceImageSource**에 그릴 경우 DXGI 디바이스에 다중 스레드 액세스가 활성화되어 있는지도 확인해야 합니다. 이는 백그라운드 스레드에서 그릴 경우에만 성능을 위해 취해야 하는 조치입니다.

    예를 들어 다음과 같은 가치를 제공해야 합니다.

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

4.  [IVirtualSurfaceUpdatesCallbackNative](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-ivirtualsurfaceupdatescallbacknative)의 구현에 대한 참조를 전달하여 [IVirtualSurfaceImageSourceNative::RegisterForUpdatesNeeded](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-registerforupdatesneeded)를 호출합니다.

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

    프레임워크는 [VirtualSurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource)의 영역이 업데이트될 때 [IVirtualSurfaceUpdatesCallbackNative::UpdatesNeeded](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-registerforupdatesneeded)의 구현을 호출합니다.

    이는 프레임워크가 영역을 그려야 할 필요가 있는지 결정하는 경우(예: 사용자가 표면의 보기를 회전하거나 확대/축소하는 경우), 또는 앱이 해당 영역에서 [IVirtualSurfaceImageSourceNative::Invalidate](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-invalidate)를 호출한 후에 발생할 수 있습니다.

5.  [IVirtualSurfaceImageSourceNative::UpdatesNeeded](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceupdatescallbacknative-updatesneeded)에서 [IVirtualSurfaceImageSourceNative::GetUpdateRectCount](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-getupdaterectcount) 및 [IVirtualSurfaceImageSourceNative::GetUpdateRects](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-getupdaterects) 메서드를 사용하여 표면의 어떤 영역을 그려야 할지 결정합니다.

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

6.  마지막으로, 업데이트해야 할 각 영역에 대해 다음 작업을 수행합니다.

    1.  **ISurfaceImageSourceNativeWithD2D::BeginDraw**에 **ID2D1DeviceContext** 개체에 대한 포인터를 제공하고 반환된 그리기 컨텍스트를 사용하여 **SurfaceImageSource** 내에 원하는 사각형의 콘텐츠를 그립니다. **ISurfaceImageSourceNativeWithD2D::BeginDraw**와 그리기 명령은 백그라운드 스레드에서 호출할 수 있습니다. *updateRect* 매개 변수에서 업데이트에 대해 지정한 영역에만 그려집니다.

        이 메서드는 *offset* 매개 변수에서 업데이트된 대상 직사각형의 점(x,y) 오프셋을 반환합니다. **ID2D1DeviceContext**에서 이 오프셋을 사용하여 업데이트된 콘텐츠를 그릴 위치를 결정할 수 있습니다.

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

    2.  특정 콘텐츠를 해당 영역에 그리되, 더 나은 성능을 위해 경계 영역으로 그리기를 제한합니다.

    3.  **ISurfaceImageSourceNativeWithD2D::EndDraw**를 호출합니다. 결과로 비트맵이 생성됩니다.

> [!NOTE]
> 연결된 [창](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window)이 숨겨져 있을 때는 응용 프로그램이 **SurfaceImageSource**에 그리지 말아야 합니다. 그러지 않으면 **ISurfaceImageSourceNativeWithD2D** API가 실패합니다. 이 작업을 수행하려면 가시성 변경 사항을 추적하는 [Window.VisibilityChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) 이벤트에 대한 이벤트 수신기로 등록합니다.

## <a name="swapchainpanel-and-gaming"></a>SwapChainPanel 및 게임


[SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)은 개발자가 스왑 체인을 직접 관리할 수 있는 경우 고성능 그래픽 및 게임을 지원하기 위해 디자인된 Windows 런타임 유형입니다. 이때 자신만의 DirectX 스왑 체인을 만들고 렌더링된 콘텐츠의 표시를 관리합니다.

성능을 보장하기 위해, [SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) 유형에는 특정 제한이 있습니다.

-   앱별 [SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) 인스턴스는 4개 이하입니다.
-   DirectX 스왑 체인의 높이 너비를 설정 해야 합니다 (에서 [DXGI\_스왑\_체인\_DESC1](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1)) 스왑 체인 요소의 현재 차원에 있습니다. 콘텐츠 표시는 크기가 조정 하지 않으면 (사용 하 여 **DXGI\_크기 조정\_STRETCH**) 맞지 않습니다.
-   DirectX 스왑 체인의 크기 조정 모드를 설정 해야 합니다 (에서 [DXGI\_스왑\_체인\_DESC1](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1))를 **DXGI\_배율\_STRETCH**합니다.
-   DirectX 스왑 체인의 알파 모드를 설정할 수 없습니다 (에서 [DXGI\_스왑\_체인\_DESC1](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1))를 **DXGI\_알파\_모드\_ 미리 곱한**합니다.
-   [IDXGIFactory2::CreateSwapChainForComposition](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcomposition)을 호출하여 DirectX 스왑 체인을 만들어야 합니다.

앱의 요구 사항에 따라 [SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)을 업데이트합니다. 이때 XAML 프레임워크의 업데이트는 대상이 아닙니다. **SwapChainPanel**의 업데이트를 XAML 프레임워크의 해당 업데이트와 동기화해야 하면 [Windows::UI::Xaml::Media::CompositionTarget::Rendering](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.compositiontarget.rendering) 이벤트를 등록합니다. 그렇지 않을 경우 **SwapChainPanel** 패널을 업데이트하는 스레드가 아닌 스레드에서 XAML 요소를 업데이트하려면 스레드 교차 문제를 고려해야 합니다.

**SwapChainPanel**에 짧은 대기 시간의 포인터 입력을 수신해야 하는 경우 [SwapChainPanel::CreateCoreIndependentInputSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.swapchainpanel.createcoreindependentinputsource)를 사용합니다. 이 메서드는 백그라운드 스레드에서 최소 대기 시간에 입력 이벤트를 수신하는 데 사용할 수 있는 [CoreIndependentInputSource](https://docs.microsoft.com/uwp/api/windows.ui.core.coreindependentinputsource) 개체를 반환합니다. 이 메서드가 호출되면 **SwapChainPanel**에 대해 일반 XAML 포인터 입력 이벤트가 발생되지 않습니다. 모든 입력이 백그라운드 스레드로 리디렉션되기 때문입니다.


> **참고** 일반적으로 DirectX 앱은 스왑 체인을 표시 창 크기(일반적으로 대부분의 Microsoft Store 게임에서 기본 화면 해상도)와 동일하게 가로 방향으로 만들어야 합니다. 이렇게 해야 앱에서 표시되는 XAML 오버레이가 없을 때 최적의 스왑 체인 구현을 사용합니다. 앱이 세로 모드로 회전되는 경우 앱은 기존 스왑 체인에서 [IDXGISwapChain1::SetRotation](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-setrotation)을 호출하고 필요한 경우 콘텐츠에 변형을 적용한 다음 동일한 스왑 체인에서 [SetSwapChain](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-iswapchainpanelnative-setswapchain)을 다시 호출해야 합니다. 마찬가지로, **SetSwapChain** 호출을 통해 스왑 체인 크기를 변경할 때마다 앱이 동일한 스왑 체인에 대해 [IDXGISwapChain::ResizeBuffers](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers)를 다시 호출해야 합니다.


 

다음은 코드 숨김으로 [SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) 개체를 만들고 업데이트하는 기본 프로세스입니다.

1.  앱에 대한 스왑 체인 패널의 인스턴스를 가져옵니다. XAML에서 인스턴스는 `<SwapChainPanel>` 태그로 표시됩니다.

    `Windows::UI::Xaml::Controls::SwapChainPanel^ swapChainPanel;`

    다음은 `<SwapChainPanel>` 태그의 예제입니다.

    ```xml
    <SwapChainPanel x:Name="swapChainPanel">
        <SwapChainPanel.ColumnDefinitions>
            <ColumnDefinition Width="300*"/>
            <ColumnDefinition Width="1069*"/>
        </SwapChainPanel.ColumnDefinitions>
    …
    ```

2.  포인터를 [ISwapChainPanelNative](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative)로 가져갑니다. [SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) 개체를 [IInspectable](https://docs.microsoft.com/windows/desktop/api/inspectable/nn-inspectable-iinspectable)(또는 **IUnknown**)로 캐스트하고 그 위에서 **QueryInterface**를 호출하여 기본 **ISwapChainPanelNative** 구현을 가져옵니다.

    ```cpp
    Microsoft::WRL::ComPtr<ISwapChainPanelNative> m_swapChainNative;
    // ...
    IInspectable* panelInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(swapChainPanel);
    panelInspectable->QueryInterface(__uuidof(ISwapChainPanelNative), (void **)&m_swapChainNative);
    ```

3.  DXGI 디바이스와 스왑 체인을 만들고 [SetSwapChain](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-iswapchainpanelnative-setswapchain)에 전달하여 스왑 체인을 [ISwapChainPanelNative](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative)로 설정합니다.

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

4.  DirectX 스왑 체인에 그리고, 이를 나타내어 콘텐츠를 표시합니다.

    ```cpp
    HRESULT hr = m_swapChain->Present(1, 0);
    ```

    XAML 요소는 Windows 런타임 레이아웃/렌더링 논리가 업데이트를 알리면 새로 고쳐집니다.

## <a name="related-topics"></a>관련 항목

* [Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm)
* [SurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource)
* [VirtualSurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource)
* [SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)
* [ISwapChainPanelNative](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative)
* [Direct3D 용 프로그래밍 가이드 11](https://docs.microsoft.com/windows/desktop/direct3d11/dx-graphics-overviews)

 

 




