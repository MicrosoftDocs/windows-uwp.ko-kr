---
author: joannaleecy
title: 게임 샘플 확장
description: UWP DirectX 게임에서 XAML 오버레이를 구현하는 방법을 자세히 알아보세요.
keywords: DirectX, XAML
ms.author: joanlee
ms.date: 10/24/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cb837965746eb1c2c7deab613eec239a83cac294
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7172335"
---
# <a name="extend-the-game-sample"></a>게임 샘플 확장

지금까지 기본적인 UWP(유니버설 Windows 플랫폼) DirectX 3D 게임의 주요 구성 요소에 대해 알아봤습니다. 뷰 공급자 및 렌더링 파이프라인을 포함한 게임의 프레임워크를 설정하고 기본 게임 루프를 구현할 수 있습니다. 또한 기본적인 사용자 인터페이스 오버레이를 만들고 소리를 통합하고 컨트롤을 구현할 수 있습니다. 지금은 자신만의 고유한 게임을 만들고 있는 중이지만, 더 많은 도움말 및 정보가 필요한 경우에는 다음 리소스를 확인합니다.

-   [DirectX 그래픽 및 게임](https://msdn.microsoft.com/library/windows/desktop/ee663274)
-   [Direct3D 11 개요](https://msdn.microsoft.com/library/windows/desktop/ff476345)
-   [Direct3D 11 참조](https://msdn.microsoft.com/library/windows/desktop/ff476147)

## <a name="using-xaml-for-the-overlay"></a>오버레이에서 XAML 사용


이 문서에서 자세히 다루지는 않았지만, 오버레이에서 [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990) 대신 XAML을 사용하는 것도 하나의 방법입니다. XAML은 사용자 인터페이스 요소를 작성하는 데 있어 Direct2D 보다 장점이 많습니다. 가장 중요 한 장점은 Windows10 모양과 느낌을 DirectX 게임에 보다 편리 하 게 통합할 수 있다는 것입니다. UWP 앱을 정의하는 대부분의 공통 요소, 스타일 및 동작이 XAML 모델로 긴밀하게 통합되어 게임 개발자가 훨씬 더 적은 작업으로 구현할 수 있습니다. 고유한 게임 디자인에 복잡한 사용자 인터페이스가 있는 경우 Direct2D 대신 XAML 사용을 고려해 보세요.

XAML에서는 Direct2D와 비슷한 모양의 게임 인터페이스를 훨씬 손쉽게 만들 수 있습니다.

### <a name="xaml"></a>XAML
![XAML 오버레이](./images/simple-dx-game-extend-xaml.PNG)

### <a name="direct2d"></a>Direct2D
![D2D 오버레이](./images/simple-dx-game-extend-d2d.PNG)

최종 결과는 비슷하지만, Direct2D 구현과 XAML 인터페이스 구현 간에 많은 차이점이 있습니다.

특징 | XAML| Direct2D
:----------|:----------- | :-----------
오버레이 정의 | XAML 파일 `\*.xaml`에 정의되어 있습니다. 일단 XAML을 이해하면 보다 정교한 오버레이를 생성 및 구성하는 것이 Direct2D에 비해 훨씬 쉽습니다.| Direct2D 대상 버퍼에 수동으로 배치되고 기록되는 Direct2D 원형 및 [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) 문자열 컬렉션으로 정의됩니다. 
사용자 인터페이스 요소 | XAML 사용자 인터페이스 요소는 [**Windows::UI::Xaml**](https://msdn.microsoft.com/library/windows/apps/br209045) 및 [**Windows::UI::Xaml::Controls**](https://msdn.microsoft.com/library/windows/apps/br227716) 같이 Windows 런타임 XAML API의 일부인 표준화된 요소로부터 나옵니다. XAML 사용자 인터페이스 요소의 동작을 처리하는 코드는 코드 숨김 파일인 Main.xaml.cpp에 정의되어 있습니다. | 사각형 및 줄임표처럼 간단한 셰이프를 그릴 수 있습니다.
창 크기 조정 | 핸들 크기를 자연스럽게 조정하고 상태 변경 이벤트를 확인하여 이에 맞게 오버레이 변환 | 오버레이의 구성 요소를 다시 그리는 방법을 수동으로 지정해야 합니다.


또 다른 큰 차이점은 [스왑 체인](https://docs.microsoft.com/windows/uwp/graphics-concepts/swap-chains)과 관련이 있습니다. 스왑 체인을 [**Windows::UI::Core::CoreWindow**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow) 개체에 연결할 필요가 없습니다. 대신에 [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.swapchainpanel) 개체가 새로 구성되면 XAML이 통합된 DirectX 앱을 스왑 체인을 연결합니다. 

다음 조각은 [**DirectXPage.xaml**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml) 파일에서 **SwapChainPanel**를 위한 XAML을 선언하는 방법을 보여줍니다.
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

**SwapChainPanel** 개체는 [시작 시](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp#L45-L51) 앱 단일 항목에서 생성된 현재 창 개체의 [**콘텐츠**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.Content) 속성으로 설정됩니다.

```cpp
void App::OnLaunched(_In_ LaunchActivatedEventArgs^ /* args */)
{
    m_mainPage = ref new DirectXPage();

    Window::Current->Content = m_mainPage;
    // Bring the application to the foreground so that it's visible
    Window::Current->Activate();
}
```


구성된 스왑 체인을 XAML에 정의된 [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) 인스턴스에 연결하려면 기본 네이티브 [**ISwapChainPanelNative**](https://msdn.microsoft.com/library/dn302143) 인터페이스 구현에 대한 포인터를 가져오고 여기에서 [**ISwapChainPanelNative::SetSwapChain**](https://msdn.microsoft.com/library/windows/desktop/dn302144)를 호출하여 구성된 스왑 체인에 전달해야 합니다. 

[**DX::DeviceResources::CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/Common/DeviceResources.cpp#L218-L521)에서 나온 다음 조각에는 DirectX/XAML 상호 운용성을 위한 이 기능이 자세히 설명되어 있습니다.

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

이 프로세스에 대한 자세한 내용은 [DirectX 및 XAML 상호 운용](directx-and-xaml-interop.md)을 참조하세요.

## <a name="sample"></a>샘플

오버레이에 XAML을 사용하는 이 게임 버전을 다운로드하려면 [Direct3D 슈팅 게임 샘플(XAML)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameXaml)로 이동하세요.


나머지 항목에서 논의한 게임 샘플 버전과 달리, XAML 버전은 [App.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/App.cpp) 및 [GameInfoOverlay.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp) 대신에 [App.xaml.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp) 및 [DirectXPage.xaml.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml.cpp) 파일에서 프레임워크를 정의합니다.