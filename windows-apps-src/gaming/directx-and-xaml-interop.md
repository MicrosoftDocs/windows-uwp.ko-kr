---
title: DirectX 및 XAML interop
description: UWP(유니버설 Windows 플랫폼) 게임에서 XAML(Extensible Application Markup Language)과 Microsoft DirectX를 함께 사용할 수 있습니다.
ms.assetid: 0fb2819a-61ed-129d-6564-0b67debf5c6b
---

# DirectX 및 XAML interop


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

UWP(유니버설 Windows 플랫폼) 게임에서 XAML(Extensible Application Markup Language)과 Microsoft DirectX를 함께 사용할 수 있습니다. XAML과 DirectX를 함께 사용하면 DirectX로 렌더링된 콘텐츠와 상호 운용되는 유연한 사용자 인터페이스 프레임워크를 빌드할 수 있으며 이 방법은 그래픽을 많이 사용하는 앱에 특히 유용합니다. 이 항목에서는 DirectX를 사용하는 UWP 앱의 구조를 설명하고, DirectX와 함께 작동하도록 UWP 앱을 빌드할 때 사용할 중요 유형을 식별합니다.

> **참고** DirectX API는 Windows 런타임 형식으로 정의되지 않으므로 일반적으로 Visual C++ 확장 구성 요소(C++/CX)를 사용하여 DirectX와 상호 운용되는 XAMLUWP 구성 요소를 개발합니다. 또한 DirectX 호출을 개별 Windows 런타임 메타데이터 파일에 래핑할 경우 DirectX를 사용하는 C# 및 XAML로 UWP 앱을 만들 수 있습니다.

 

## XAML 및 DirectX


DirectX는 각각 2D와 3D 그래픽에 대한 강력한 라이브러리인 Direct2D 및 Microsoft Direct3D를 제공합니다. XAML이 기본적인 2D 기능 및 효과, 모델링 및 게임 등의 여러 앱을 제공하지만 보다 복잡한 그래픽 지원이 필요합니다. 따라서 Direct2D 및 Direct3D를 사용하여 그래픽 전체 또는 일부를 렌더링하고 XAML을 사용하여 그 외 모든 작업을 수행할 수 있습니다.

XAML 및 DirectX interop 시나리오에서는 다음의 두 개념에 대해 알고 있어야 합니다.

-   공유 표면은 XAML로 정의된 크기 조정되는 표시 영역입니다, 즉, DirectX를 사용하여 [**Windows::UI::Xaml::Media::Brush**](https://msdn.microsoft.com/library/windows/apps/br228076) 유형으로 간접적으로 그릴 수 있는 영역입니다. 공유 표면의 경우 스왑 체인을 표시하는 호출을 직접 제어하지 않습니다. 공유 표면에 대한 업데이트는 XAML 프레임워크의 업데이트와 동기화됩니다.
-   스왑 체인 자체. 렌더링 대상이 완료된 후 표시를 위해 제공되는 메모리 영역인 DirectX 렌더링 파이프라인의 백 버퍼를 제공합니다.

DirectX를 사용하여 어떤 작업을 하려는 것인지 고려해 봅니다. 표시 창 크기에 맞는 단일 컨트롤을 합성하거나 애니메이션 효과를 주기 위해 사용하나요? 합성된 표면을 다른 표면이나 화면 가장자리가 가릴 수 있나요? 게임에서처럼 실시간으로 렌더링 및 제어해야 하는 출력을 포함하나요?

DirectX를 사용할 방법을 결정했으면 다음 Windows 런타임 형식 중 하나를 사용하여 DirectX 렌더링을 Windows 스토어 앱에 통합합니다.

-   정적 이미지를 작성하거나 이벤트 구동 간격으로 복잡한 이미지를 그리려는 경우 [**Windows::UI::Xaml::Media::Imaging::SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041)를 사용하여 공유 표면에 그립니다. 이 유형은 크기가 조정되는 DirectX 그리기 표면을 처리합니다. 일반적으로 이미지나 텍스처를 문서 또는 UI 요소에 표시할 비트맵으로 작성할 때 이 유형을 사용하며, 고성능 게임 같은 실시간 대화형 작업에는 잘 맞지 않습니다. **SurfaceImageSource** 개체에 대한 업데이트는 XAML 사용자 인터페이스 업데이트와 동기화되므로 가변 프레임 속도 또는 실시간 입력에 대한 응답 사용자에게 제공하는 시각적 피드백에 지연이 발생할 수 있기 때문입니다. 그렇지만 동적 컨트롤이나 데이터 시뮬레이션을 하는 데는 무리 없는 정도로 빠른 업데이트 속도를 제공합니다.

    [
            **SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) 그래픽 개체는 다른 XAML UI 요소와 함께 작성할 수 있습니다. 이러한 개체는 변경하거나 투영할 수 있고, XAML 프레임워크는 불투명도 또는 z-인덱스 값을 따릅니다.

-   이미지가 제공된 화면 크기보다 크고 사용자가 이를 회전하거나 확대/축소할 수 있는 경우 [**Windows::UI::Xaml::Media::Imaging::VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050)를 사용합니다. 이 유형은 화면보다 큰 크기가 조정되는 DirectX 그리기 표면을 처리합니다. [
            **SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041)와 마찬가지로, 복잡한 이미지나 컨트롤을 동적으로 작성할 때 이 유형을 사용할 수 있습니다. 그리고 **SurfaceImageSource**처럼, 고성능 게임에는 적합하지 않습니다. **VirtualSurfaceImageSource**를 사용할 수 있는 XAML 요소의 예는 맵 컨트롤 또는 이미지가 많고 크기가 큰 문서 뷰어입니다.

-   정기적인 짧은 대기 시간 간격으로 업데이트가 수행되어야 하는 상황에서, 또는 업데이트된 그래픽을 실시간으로 나타내는 데 DirectX를 사용하는 경우 XAML 프레임워크 새로 고침 타이머와 동기화하지 않고 그래픽을 새로 고칠 수 있도록 [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) 클래스를 사용하세요. 이 유형을 사용하면 그래픽 디바이스의 스왑 체인([**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631))에 직접 액세스할 수 있고 렌더링 대상의 최상위 XAML 레이어에 액세스할 수 있습니다. 이 유형은 XAML 기반 사용자 인터페이스가 필요한 게임 및 기타 전체 화면 DirectX 앱에서 잘 작동합니다. 이 방법을 사용하려면 Microsoft DXGI(DirectX Graphics Infrastructure), Direct2D 및 Direct3D 기술을 포함하여 DirectX에 대해 잘 알고 있어야 합니다. 자세한 내용은 [Direct3D 11의 프로그래밍 지침](https://msdn.microsoft.com/library/windows/desktop/ff476345)을 참조하세요.

## SurfaceImageSource


[
            **SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041)는 그릴 DirectX 공유 표면을 제공하고 비트를 앱 콘텐츠로 작성합니다.

다음은 코드 숨김으로 [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) 개체를 만들고 업데이트하는 기본 프로세스입니다.

1.  [
            **SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) 생성자에 높이와 너비를 전달하여 공유 표면의 크기를 정의합니다. 표면에 알파(불투명도) 지원이 필요한지 여부도 지정할 수 있습니다.

    예제:

    `SurfaceImageSource^ surfaceImageSource = ref new SurfaceImageSource(400, 300);`

2.  포인터를 [**ISurfaceImageSourceNative**](https://msdn.microsoft.com/library/windows/desktop/hh848322)로 가져갑니다. [
            **SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) 개체를 [**IInspectable**](https://msdn.microsoft.com/library/windows/desktop/br205821)(또는 **IUnknown**)로 캐스트하고 그 위에서 **QueryInterface**를 호출하여 기본 **ISurfaceImageSourceNative** 구현을 가져옵니다. 이 구현에 정의된 메서드를 사용하여 디바이스를 설정하고 그리기 작업을 실행합니다.

    ```cpp
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNative> m_sisNative;
    // ...
    IInspectable* sisInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(surfaceImageSource);
    sisInspectable->QueryInterface(__uuidof(ISurfaceImageSourceNative), (void **)&m_sisNative);
    ```

3.  먼저 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082)를 호출한 다음 [**ISurfaceImageSourceNative::SetDevice**](https://msdn.microsoft.com/library/windows/desktop/hh848325)에 디바이스 및 컨텍스트를 전달하여 DXGI 디바이스를 설정합니다. 예제:

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device>              m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext>           m_d3dContext;
    // ...
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
            &m_d3dContext
            )
        );  
    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);
    // ...
    m_sisNative->SetDevice(dxgiDevice.Get());
    ```

4.  [
            **IDXGISurface**](https://msdn.microsoft.com/library/windows/desktop/bb174565) 개체에 대한 포인터를 [**ISurfaceImageSourceNative::BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323)에 전달하고 DirectX를 사용하여 해당 표면에 그립니다. *updateRect* 매개 변수에서 업데이트에 대해 지정한 영역에만 그려집니다.

    > **참고** [**IDXGIDevice**](https://msdn.microsoft.com/library/windows/desktop/bb174527)당 한 번에 하나의 미처리 [**BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323) 작업만 활성 상태로 있을 수 있습니다.

     

    This method returns the point (x,y) offset of the updated target rectangle in the *offset* parameter. You use this offset to determine where to draw into inside the [**IDXGISurface**](https://msdn.microsoft.com/library/windows/desktop/bb174565).

    ```cpp
    ComPtr<IDXGISurface> surface;

    HRESULT beginDrawHR = m_sisNative->BeginDraw(updateRect, &surface, &offset);
    if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED || beginDrawHR == DXGI_ERROR_DEVICE_RESET)
    {
              // Device changed
    }
    else
    {
        // Draw to IDXGISurface (the surface paramater)
    }
    ```

5.  [
            **ISurfaceImageSourceNative::EndDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848324)를 호출하여 비트맵을 완료합니다. 이 비트맵을 [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101)에 전달합니다.

    ```cpp
    m_sisNative->EndDraw();
    // ...
    // The SurfaceImageSource object's underlying ISurfaceImageSourceNative object contains the completed bitmap.
    ImageBrush^ brush = ref new ImageBrush();
    brush->ImageSource = surfaceImageSource;
    ```

6.  [
            **ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101)를 사용하여 비트맵을 그립니다.

> **참고** 현재 [**SurfaceImageSource::SetSource**](https://msdn.microsoft.com/library/windows/apps/br243255)(**IBitmapSource::SetSource**에서 상속됨)를 호출하면 예외가 발생합니다. [
            **SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) 개체에서 이 속성을 호출하지 마세요.

 

## VirtualSurfaceImageSource


[
            **VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050)는 콘텐츠가 화면에 맞출 수 있는 크기보다 잠재적으로 더 클 때 [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041)를 확장하며 최적으로 렌더링하려면 콘텐츠를 가상화해야 합니다.

[
            **VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050)는 표면의 영역이 화면에 보이게 되면 이를 업데이트하도록 구현하는 콜백 [**IVirtualSurfaceImageSourceCallbacksNative::UpdatesNeeded**](https://msdn.microsoft.com/library/windows/desktop/hh848337)을 사용한다는 점에서 [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041)와 다릅니다. XAML 프레임워크가 개발자 대신 수행해 주는 것처럼 숨겨진 영역을 지울 필요는 없습니다.

다음은 코드 숨김으로 [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) 개체를 만들고 업데이트하는 기본 프로세스입니다.

1.  원하는 크기로 [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050)의 인스턴스를 만듭니다. 예제:

    `VirtualSurfaceImageSource^ virtualSIS = ref new VirtualSurfaceImageSource(2000, 2000);`

2.  포인터를 [**IVirtualSurfaceImageSourceNative**](https://msdn.microsoft.com/library/windows/desktop/hh848328)로 가져갑니다. [
            **VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) 개체를 [**IInspectable**](https://msdn.microsoft.com/library/windows/desktop/br205821) 또는 [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)으로 캐스트하고 그 위에서 [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521)를 호출하여 기본 **IVirtualSurfaceImageSourceNative** 구현을 가져옵니다. 이 구현에 정의된 메서드를 사용하여 디바이스를 설정하고 그리기 작업을 실행합니다.

    ```cpp
    Microsoft::WRL::ComPtr<IVirtualSurfaceImageSourceNative>  m_vsisNative;
    // ...
    IInspectable* vsisInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(virtualSIS);
    vsisInspectable->QueryInterface(__uuidof(IVirtualSurfaceImageSourceNative), (void **)&m_vsisNative);
    ```

3.  [
            **IVirtualSurfaceImageSourceNative::SetDevice**](https://msdn.microsoft.com/library/windows/desktop/hh848325)를 호출하여 DXGI 디바이스를 설정합니다. 예제:

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device>              m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext>           m_d3dContext;
    // ...
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
            &m_d3dContext
            )
        );  
    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);
    // ...
    m_vsisNative->SetDevice(dxgiDevice.Get());
    ```

4.  [
            **IVirtualSurfaceUpdatesCallbackNative**](https://msdn.microsoft.com/library/windows/desktop/hh848336)의 구현에 대한 참조를 전달하여 [**IVirtualSurfaceImageSourceNative::RegisterForUpdatesNeeded**](https://msdn.microsoft.com/library/windows/desktop/hh848334)를 호출합니다.

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

    프레임워크는 [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050)의 영역이 업데이트될 때 [**IVirtualSurfaceUpdatesCallbackNative::UpdatesNeeded**](https://msdn.microsoft.com/library/windows/desktop/hh848334)의 구현을 호출합니다.

    이는 프레임워크가 영역을 그려야 할 필요가 있는지 결정하는 경우(예: 사용자가 표면의 보기를 회전하거나 확대/축소하는 경우), 또는 앱이 해당 영역에서 [**IVirtualSurfaceImageSourceNative::Invalidate**](https://msdn.microsoft.com/library/windows/desktop/hh848332)를 호출한 후에 발생할 수 있습니다.

5.  [
            **IVirtualSurfaceImageSourceNative::UpdatesNeeded**](https://msdn.microsoft.com/library/windows/desktop/hh848337)에서 [**IVirtualSurfaceImageSourceNative::GetUpdateRectCount**](https://msdn.microsoft.com/library/windows/desktop/hh848329) 및 [**IVirtualSurfaceImageSourceNative::GetUpdateRects**](https://msdn.microsoft.com/library/windows/desktop/hh848330) 메서드를 사용하여 표면의 어떤 영역을 그려야 할지 결정합니다.

    ```cpp
    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
        HRESULT hr = S_OK;

        try
        {
            ULONG drawingBoundsCount = 0;  

                  m_vsisNative->GetUpdateRectCount(&drawingBoundsCount);
            std::unique_ptr<RECT[]> drawingBounds(new RECT[drawingBoundsCount]);
            m_vsisNative->GetUpdateRects(drawingBounds.get(), drawingBoundsCount);
            
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

    1.  [
            **IDXGISurface**](https://msdn.microsoft.com/library/windows/desktop/bb174565) 개체에 대한 포인터를 [**IVirtualSurfaceImageSourceNative::BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323)에 전달하고 DirectX를 사용하여 해당 표면에 그립니다. *updateRect* 매개 변수에서 업데이트에 대해 지정한 영역에만 그려집니다.

        [
            **IlSurfaceImageSourceNative::BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323)와 마찬가지로, 이 메서드는 업데이트되는 대상 직사각형의 점(x,y) 오프셋을 *offset* 매개 변수에 반환합니다. 이 오프셋을 사용하여 [**IDXGISurface**](https://msdn.microsoft.com/library/windows/desktop/bb174565) 내에 그릴 위치를 결정합니다.

        > **참고** [**IDXGIDevice**](https://msdn.microsoft.com/library/windows/desktop/bb174527)당 한 번에 하나의 미처리 [**BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323) 작업만 활성 상태로 있을 수 있습니다.

         

        ```cpp
        ComPtr<IDXGISurface> bigSurface;

        HRESULT beginDrawHR = m_vsisNative->BeginDraw(updateRect, &bigSurface, &offset);
        if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED || beginDrawHR == DXGI_ERROR_DEVICE_RESET)
        {
                  // Device changed
        }
        else
        {
            // Draw to IDXGISurface
        }
        ```

    2.  Draw the specific content to that region, but constrain your drawing to the bounded regions for better performance.

    3.  Call [**IVirtualSurfaceImageSourceNative::EndDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848324). The result is a bitmap.

## SwapChainPanel 및 게임


[
            **SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834)은 개발자가 스왑 체인을 직접 관리할 수 있는 경우 고성능 그래픽 및 게임을 지원하기 위해 디자인된 Windows 런타임 유형입니다. 이때 자신만의 DirectX 스왑 체인을 만들고 렌더링된 콘텐츠의 표시를 관리합니다. 그런 다음 메뉴, 경고 표시 및 기타 UI 오버레이 등, XAML 요소를 **SwapChainPanel** 개체에 추가할 수 있습니다.

성능을 보장하기 위해, [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) 유형에는 특정 제한이 있습니다.

-   앱별 [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) 인스턴스는 4개 이하입니다.
-   [
            **SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834)에서 상속받은 **Opacity**, **RenderTransform**, **Projection** 및 **Clip** 속성은 지원되지 않습니다.
-   DirectX 스왑 체인의 높이 및 너비를 ([**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528)에서) 앱 창의 현재 크기로 설정해야 합니다. 그러지 않으면 (**DXGI\_SCALING\_STRETCH**를 사용하여) 디스플레이 콘텐츠의 크기가 조정됩니다.
-   DirectX 스왑 체인의 크기 조정 모드를 ([**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528)에서) **DXGI\_SCALING\_STRETCH**로 설정해야 합니다.
-   DirectX 스왑 체인의 알파 모드를 ([**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528)에서) **DXGI\_ALPHA\_MODE\_PREMULTIPLIED**로 설정하면 안 됩니다.
-   [
            **IDXGIFactory2::CreateSwapChainForComposition**](https://msdn.microsoft.com/library/windows/desktop/hh404558)을 호출하여 DirectX 스왑 체인을 만들어야 합니다.

앱의 요구 사항에 따라 [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834)을 업데이트합니다. 이때 XAML 프레임워크의 업데이트는 대상이 아닙니다. **SwapChainPanel**의 업데이트를 XAML 프레임워크의 해당 업데이트와 동기화해야 하면 [**Windows::UI::Xaml::Media::CompositionTarget::Rendering**](https://msdn.microsoft.com/library/windows/apps/br228127) 이벤트를 등록합니다. 그렇지 않을 경우 **SwapChainPanel** 패널을 업데이트하는 스레드가 아닌 스레드에서 XAML 요소를 업데이트하려면 스레드 교차 문제를 고려해야 합니다.

[
            **SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834)을 사용하여 앱을 디자인할 때 따라야 할 일반적인 모범 사례가 몇 가지 더 있습니다.

-   [
            **SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834)은 [**Windows::UI::Xaml::Controls::Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)에서 상속하며 유사한 레이아웃 동작을 지원합니다. 개발자는 **Grid** 유형과 그 속성을 능숙하게 다룰 수 있어야 합니다.

-   DirectX 스왑 체인이 설정된 후에는 [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834)에 대해 발생한 모든 입력 이벤트가 다른 XAML 요소에서와 동일하게 작동합니다. **SwapChainPanel**에 대한 배경 브러시를 설정하지 않으며, **SwapChainPanel**을 사용하지 않는 DirectX 앱에서처럼 앱의 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 개체에서 직접 입력 이벤트를 처리하지 않아도 됩니다.

-   • [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834)의 직계 자식 하위에 있는 시각적 XAML 요소 트리의 모든 콘텐츠는 **SwapChainPanel** 개체의 직계 자식의 레이아웃 크기로 잘립니다. 이러한 레이아웃 경계 외부에서 변형된 콘텐츠는 렌더링되지 않습니다. 따라서 XAML [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490)로 애니메이션을 실행하는 XAML 콘텐츠는 레이아웃 경계가 애니메이션의 전체 범위를 포함할 만큼 충분히 큰 요소 하위의 시각적 트리에 둡니다.

-   [
            **SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) 직속 하위의 시각적 XAML 요소의 수를 제한합니다. 가능하면 공통되는 부모 하위의 근접 연결에 있는 요소를 그룹화합니다. 직계 자식인 시각적 요소의 수와 크기 간에는 성능 절충이 필요합니다. 너무 많거나 불필요하게 큰 XAML 요소는 전체 성능을 저하시킬 수 있습니다. 마찬가지로, 앱의 **SwapChainPanel**에 대해 단일의 전체 화면 자식 XAML 요소를 만들지 마세요. 그럴 경우 앱에서 과잉 그리기가 늘어나고 성능은 저하됩니다. 일반적으로 앱의 **SwapChainPanel**에 대해 직계 자식 XAML 시각적 요소를 9개 이상 만들지 말고, 각 요소의 레이아웃 크기는 요소의 시각적 콘텐츠를 포함하는 데 필요한 정도로 제한해야 합니다. 그러나 성능을 현저하게 저하시키지 않고도 상당히 복잡한 **SwapChainPanel**의 자식 요소 하위에 요소의 시각적 트리를 만들 수 있습니다.

> **참고** 일반적으로 DirectX 앱은 스왑 체인을 표시 창 크기(일반적으로 대부분의 Windows 스토어 게임에서 기본 화면 해상도)와 동일하게 가로 방향으로 만들어야 합니다. 이렇게 해야 앱에서 표시되는 XAML 오버레이가 없을 때 최적의 스왑 체인 구현을 사용합니다. 앱이 세로 모드로 회전되는 경우 앱은 기존 스왑 체인에서 [**IDXGISwapChain1::SetRotation**](https://msdn.microsoft.com/library/windows/desktop/hh446801)을 호출하고 필요한 경우 콘텐츠에 변형을 적용한 다음 동일한 스왑 체인에서 [**SetSwapChain**](https://msdn.microsoft.com/library/windows/desktop/dn302144)을 다시 호출해야 합니다. 마찬가지로, **SetSwapChain** 호출을 통해 스왑 체인 크기를 변경할 때마다 앱이 동일한 스왑 체인에 대해 [**IDXGISwapChain::ResizeBuffers**](https://msdn.microsoft.com/library/windows/desktop/bb174577)를 다시 호출해야 합니다.

 

다음은 코드 숨김으로 [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) 개체를 만들고 업데이트하는 기본 프로세스입니다.

1.  앱에 대한 스왑 체인 패널의 인스턴스를 가져옵니다. XAML에서 인스턴스는 `<SwapChainPanel>` 태그로 표시됩니다.

    `Windows::UI::Xaml::Controls::SwapChainPanel^ swapChainPanel;`

    다음은 `<SwapChainPanel>` 태그의 예제입니다.

    ```xaml
    <SwapChainPanel x:Name=&quot;swapChainPanel&quot;>
        <SwapChainPanel.ColumnDefinitions>
            <ColumnDefinition Width=&quot;300*&quot;/>
            <ColumnDefinition Width=&quot;1069*&quot;/>
        </SwapChainPanel.ColumnDefinitions>
    …
    ```

2.  포인터를 [**ISwapChainPanelNative**](https://msdn.microsoft.com/library/windows/desktop/dn302143)로 가져갑니다. [
            **SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) 개체를 [**IInspectable**](https://msdn.microsoft.com/library/windows/desktop/br205821)(또는 **IUnknown**)로 캐스트하고 그 위에서 **QueryInterface**를 호출하여 기본 **ISwapChainPanelNative** 구현을 가져옵니다.

    ```cpp
    Microsoft::WRL::ComPtr<ISwapChainPanelNative> m_swapChainNative;
    // ...
    IInspectable* panelInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(swapChainPanel);
    panelInspectable->QueryInterface(__uuidof(ISwapChainPanelNative), (void **)&m_swapChainNative);
    ```

3.  DXGI 디바이스와 스왑 체인을 만들고 [**SetSwapChain**](https://msdn.microsoft.com/library/windows/desktop/dn302144)에 전달하여 스왑 체인을 [**ISwapChainPanelNative**](https://msdn.microsoft.com/library/windows/desktop/dn302143)로 설정합니다.

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

## 관련 항목


* [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041)
* [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050)
* [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834)
* [**ISwapChainPanelNative**](https://msdn.microsoft.com/library/windows/desktop/dn302143)
* [Direct3D 11의 프로그래밍 지침](https://msdn.microsoft.com/library/windows/desktop/ff476345)

 

 






<!--HONumber=Mar16_HO1-->


