---
title: 샘플 게임 확장
description: UWP DirectX 게임에 대해 XAML 오버레이를 구현 하는 방법에 대해 알아봅니다.
keywords: DirectX, XAML
ms.date: 10/24/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 06b52e5b6fdba1db83c941e770cd49360085accf
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409552"
---
# <a name="extend-the-sample-game"></a>샘플 게임 확장

> [!NOTE]
> 이 항목은 DirectX 자습서 시리즈 [를 사용 하 여 단순 유니버설 Windows 플랫폼 (UWP) 게임 만들기](tutorial--create-your-first-uwp-directx-game.md) 의 일부입니다. 해당 링크의 항목은 계열의 컨텍스트를 설정 합니다.

이 시점에서 UWP (기본 유니버설 Windows 플랫폼) DirectX 3D 게임의 주요 구성 요소에 대해 살펴보았습니다. 보기 공급자 및 렌더링 파이프라인을 포함 하 여 게임의 프레임 워크를 설정 하 고 기본 게임 루프를 구현할 수 있습니다. 기본 사용자 인터페이스 오버레이를 만들고, 소리를 통합 하 고, 컨트롤을 구현할 수도 있습니다. 자신의 게임을 만드는 방법을 알고 있지만 추가 도움말과 정보가 필요한 경우 이러한 리소스를 확인 하세요.

-   [DirectX 그래픽 및 게임](/windows/desktop/directx)
-   [Direct3D 11 개요](/windows/desktop/direct3d11/dx-graphics-overviews)
-   [Direct3D 11 참조](/windows/desktop/direct3d11/d3d11-graphics-reference)

## <a name="using-xaml-for-the-overlay"></a>오버레이에 XAML 사용

자세히 설명 하지 않은 한 가지 대안은 오버레이의 [Direct2D](/windows/desktop/Direct2D/direct2d-portal) 대신 XAML을 사용 하는 것입니다. XAML은 사용자 인터페이스 요소를 그리기 위해 Direct2D에 비해 많은 이점을 제공 합니다. 가장 중요 한 혜택은 Windows 10 모양과 느낌을 DirectX 게임에 더 편리 하 게 통합 하는 것입니다. UWP 앱을 정의 하는 대부분의 공통 요소, 스타일 및 동작은 XAML 모델에 긴밀 하 게 통합 되어 게임 개발자가 구현 하는 데 더 많은 작업을 수행 합니다. 사용자 고유의 게임 디자인에 복잡 한 사용자 인터페이스가 있는 경우 Direct2D 대신 XAML을 사용 하는 것이 좋습니다.

XAML을 사용 하면 이전에 만든 Direct2D 유사 하 게 게임 인터페이스를 만들 수 있습니다.

### <a name="xaml"></a>XAML
![XAML 오버레이](./images/simple-dx-game-extend-xaml.PNG)

### <a name="direct2d"></a>Direct2D
![D2D 오버레이](./images/simple-dx-game-extend-d2d.PNG)

유사한 최종 결과가 있지만 Direct2D와 XAML 인터페이스를 구현 하는 것 사이에는 여러 가지 차이점이 있습니다.

기능 | XAML| Direct2D
:----------|:----------- | :-----------
오버레이 정의 | XAML 파일에 정의 `\*.xaml` 됩니다. XAML을 이해 하 고 나면 Direct2D에 비해 더 복잡 한 오버레이를 만들고 구성 하는 것이 simpiler 됩니다.| Direct2D 기본 형식의 컬렉션으로 정의 되 고 [DirectWrite](/windows/desktop/DirectWrite/direct-write-portal) 문자열을 수동으로 배치 하 여 Direct2D 대상 버퍼에 기록 합니다. 
사용자 인터페이스 요소 | XAML 사용자 인터페이스 요소는 [**windows:: ui:: xaml**](/uwp/api/Windows.UI.Xaml) 및 [**WINDOWS:: Ui:: Xaml:: 컨트롤**](/uwp/api/Windows.UI.Xaml.Controls)을 포함 하 여 Windows 런타임 xaml api의 일부인 표준화 된 요소에서 제공 됩니다. XAML 사용자 인터페이스 요소의 동작을 처리 하는 코드는 코드 숨김 파일인 .xaml에 정의 되어 있습니다. | 사각형 및 타원과 같은 간단한 셰이프를 그릴 수 있습니다.
창 크기 조정 | 기본적으로 크기 조정 및 보기 상태 변경 이벤트를 처리 하 고 적절 하 게 오버레이를 변형 합니다. | 오버레이 구성 요소를 다시 그리는 방법을 수동으로 지정 해야 합니다.

또 다른 큰 차이점에는 [스왑 체인이](/windows/uwp/graphics-concepts/swap-chains)포함 됩니다. 스왑 체인을 [**Windows:: UI:: Core:: CoreWindow**](/uwp/api/windows.ui.core.corewindow) 개체에 연결할 필요가 없습니다. 대신, XAML을 통합 하는 DirectX 앱은 새 [**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel) 개체가 생성 될 때 스왑 체인을 연결 합니다. 

다음 코드 조각에서는 [**Directxpage**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml) 파일에서 **SwapChainPanel** 에 대 한 xaml을 선언 하는 방법을 보여 줍니다.
```xml
<Page
    x:Class="Simple3DGameXaml.DirectXPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:Simple3DGameXaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <SwapChainPanel x:Name="DXSwapChainPanel">

    <!-- ... XAML user controls and elements -->

    </SwapChainPanel>
</Page>
```

**SwapChainPanel** 개체는 [시작 시](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp#L45-L51) 응용 프로그램 singleton에 의해 생성 된 현재 창 개체의 [**콘텐츠**](/uwp/api/Windows.UI.Xaml.Window.Content) 속성으로 설정 됩니다.

```cpp
void App::OnLaunched(_In_ LaunchActivatedEventArgs^ /* args */)
{
    m_mainPage = ref new DirectXPage();

    Window::Current->Content = m_mainPage;
    // Bring the application to the foreground so that it's visible
    Window::Current->Activate();
}
```

구성 된 스왑 체인을 XAML로 정의 된 [**SwapChainPanel**](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) 인스턴스에 연결 하려면 기본 네이티브 [**ISwapChainPanelNative**](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative) 인터페이스 구현에 대 한 포인터를 가져와서 구성 된 스왑 체인을 전달 하 여 [**ISwapChainPanelNative:: SetSwapChain**](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-iswapchainpanelnative-setswapchain) 를 호출 해야 합니다. 

[**DX::D eviceresources:: CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/Common/DeviceResources.cpp#L218-L521) 의 다음 코드 조각은 DIRECTX/XAML interop에 대해이 정보를 자세히 설명 합니다.

```cpp
        ComPtr<IDXGIDevice3> dxgiDevice;
        DX::ThrowIfFailed(
            m_d3dDevice.As(&dxgiDevice)
            );

        ComPtr<IDXGIAdapter> dxgiAdapter;
        DX::ThrowIfFailed(
            dxgiDevice->GetAdapter(&dxgiAdapter)
            );

        ComPtr<IDXGIFactory2> dxgiFactory;
        DX::ThrowIfFailed(
            dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
            );

        // When using XAML interop, the swap chain must be created for composition.
        DX::ThrowIfFailed(
            dxgiFactory->CreateSwapChainForComposition(
                m_d3dDevice.Get(),
                &swapChainDesc,
                nullptr,
                &m_swapChain
                )
            );

        // Associate swap chain with SwapChainPanel
        // UI changes will need to be dispatched back to the UI thread
        m_swapChainPanel->Dispatcher->RunAsync(CoreDispatcherPriority::High, ref new DispatchedHandler([=]()
        {
            // Get backing native interface for SwapChainPanel
            ComPtr<ISwapChainPanelNative> panelNative;
            DX::ThrowIfFailed(
                reinterpret_cast<IUnknown*>(m_swapChainPanel)->QueryInterface(IID_PPV_ARGS(&panelNative))
                );
            DX::ThrowIfFailed(
                panelNative->SetSwapChain(m_swapChain.Get())
                );
        }, CallbackContext::Any));

        // Ensure that DXGI does not queue more than one frame at a time. This both reduces latency and
        // ensures that the application will only render after each VSync, minimizing power consumption.
        DX::ThrowIfFailed(
            dxgiDevice->SetMaximumFrameLatency(1)
            );
    }
```

이 프로세스에 대 한 자세한 내용은 [DirectX 및 XAML interop](directx-and-xaml-interop.md)를 참조 하세요.

## <a name="sample"></a>예제

오버레이에 대해 XAML을 사용 하는이 게임의 버전을 다운로드 하려면 [Direct3D 해결 샘플 게임 (XAML)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameXaml)로 이동 합니다.

이러한 항목의 나머지 부분에서 설명 하는 샘플 게임의 버전과 달리 XAML 버전은 각각 [app.config](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/App.cpp) 및 [GameInfoOverlay](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp)대신 app.xaml [및](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp) [directxpage](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml.cpp) 파일에 프레임 워크를 정의 합니다.
