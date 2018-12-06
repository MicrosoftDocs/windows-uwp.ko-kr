---
title: UWP(유니버설 Windows 플랫폼) 앱의 다중 샘플링
description: Direct3D를 사용하는 UWP(유니버설 Windows 플랫폼) 앱에서 다중 샘플링을 사용하는 방법을 알아봅니다.
ms.assetid: 1cd482b8-32ff-1eb0-4c91-83eb52f08484
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 게임, 다중 샘플링, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: 0c1634af8589a97f5070ff85909fe12ab16bf8d6
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8743357"
---
# <a name="span-iddevgamingmultisamplingmulti-sampleantialiasinginwindowsstoreappsspan-multisampling-in-universal-windows-platform-uwp-apps"></a><span id="dev_gaming.multisampling__multi-sample_anti_aliasing__in_windows_store_apps"></span>UWP(유니버설 Windows 플랫폼) 앱의 다중 샘플링



Direct3D를 사용하는 UWP(유니버설 Windows 플랫폼) 앱에서 다중 샘플링을 사용하는 방법을 알아봅니다. 다중 샘플 앤티앨리어싱이라고도 하는 다중 샘플링은 울퉁불퉁한 가장자리를 다듬기 위해 사용되는 그래픽 기법입니다. 작동 방식은, 최종 렌더링 대상에 실제로 있는 것보다 더 많은 픽셀을 그린 후 값을 평준화하여 특정 픽셀에서 "부분적인" 가장자리의 외양을 유지하는 것입니다. Direct3D에서 다중 샘플링이 실제로 작동하는 방식에 대해 자세히 알아보려면 [다중 샘플링 앤티앨리어싱 래스터화 규칙](https://msdn.microsoft.com/library/windows/desktop/cc627092#Multisample)을 참조하세요.

## <a name="multisampling-and-the-flip-model-swap-chain"></a>다중 샘플링 및 대칭 이동 모델 스왑 체인


DirectX를 사용하는 UWP 앱은 대칭 이동 모델 스왑 체인을 사용해야 합니다. 대칭 이동 스왑 체인은 다중 샘플링을 직접 지원하지 않지만, 다른 방식으로 다중 샘플링을 여전히 적용할 수 있습니다. 즉, 장면을 다중 샘플링된 렌더링 대상 보기로 렌더링한 후 다중 샘플링된 렌더링 대상을 표시 전에 백 버퍼로 해제하면 됩니다. 이 문서에서는 UWP 앱에 다중 샘플링을 추가하기 위해 필요한 단계에 대해 설명합니다.

### <a name="how-to-use-multisampling"></a>다중 샘플링을 사용하는 방법

Direct3D 접근 권한 값 수준은 특정 최소 샘플 수 기능에 대한 지원을 보장하며, 다중 샘플링을 지원하는 특정 버퍼 형식을 사용할 수 있도록 보장합니다. 그래픽 장치는 필수 최소 사양보다 종종 더 넓은 범위의 형식과 샘플 수를 지원합니다. 특정 DXGI 형식을 이용한 다중 샘플링에 대한 기능 지원을 점검한 후 지원되는 각 형식으로 사용할 수 있는 샘플 수를 점검하여 런타임 시 다중 샘플링의 지원 여부를 확인할 수 있습니다.

1.  다중 샘플링에 어떤 DXGI 형식을 사용할 수 있는지 알아보려면 [**ID3D11Device::CheckFeatureSupport**](https://msdn.microsoft.com/library/windows/desktop/ff476497)를 호출합니다. 게임에 사용할 수 있는 렌더링 대상 형식을 제공합니다. 렌더링 대상과 해제 대상은 모두 같은 형식을 사용해야 하므로 [**D3D11\_FORMAT\_SUPPORT\_MULTISAMPLE\_RENDERTARGET**](https://msdn.microsoft.com/library/windows/desktop/ff476134)과 **D3D11\_FORMAT\_SUPPORT\_MULTISAMPLE\_RESOLVE**를 모두 확인합니다.

    **기능 수준 9:** 기능 수준 9 디바이스 [다중 샘플링 된 렌더링 대상 형식에 대 한 지원을 보장](https://msdn.microsoft.com/library/windows/desktop/ff471324#MultiSample_RenderTarget), 하지만 지원은 다중 샘플 해제 대상에 대 한 지원을 보장 되지 않습니다. 따라서 이 항목에서 설명한 다중 샘플링 기법을 사용하기 전에 이러한 사항을 확인해야 합니다.

    다음 코드는 DXGI\_FORMAT 값에 대해 다중 샘플링 지원 여부를 확인합니다.

    ```cpp
    // Determine the format support for multisampling.
    for (UINT i = 1; i < DXGI_FORMAT_MAX; i++)
    {
        DXGI_FORMAT inFormat = safe_cast<DXGI_FORMAT>(i);
        UINT formatSupport = 0;
        HRESULT hr = m_d3dDevice->CheckFormatSupport(inFormat, &formatSupport);

        if ((formatSupport & D3D11_FORMAT_SUPPORT_MULTISAMPLE_RESOLVE) &&
            (formatSupport & D3D11_FORMAT_SUPPORT_MULTISAMPLE_RENDERTARGET)
            )
        {
            m_supportInfo->SetFormatSupport(i, true);
        }
        else
        {
            m_supportInfo->SetFormatSupport(i, false);
        }
    }
    ```

2.  지원되는 각 형식에서 [**ID3D11Device::CheckMultisampleQualityLevels**](https://msdn.microsoft.com/library/windows/desktop/ff476499)를 호출하여 샘플 수 지원을 쿼리합니다.

    다음 코드는 지원되는 DXGI 형식에 대해 샘플 크기 지원 여부를 확인합니다.

    ```cpp
    // Find available sample sizes for each supported format.
    for (unsigned int i = 0; i < DXGI_FORMAT_MAX; i++)
    {
        for (unsigned int j = 1; j < MAX_SAMPLES_CHECK; j++)
        {
            UINT numQualityFlags;

            HRESULT test = m_d3dDevice->CheckMultisampleQualityLevels(
                (DXGI_FORMAT) i,
                j,
                &numQualityFlags
                );

            if (SUCCEEDED(test) && (numQualityFlags > 0))
            {
                m_supportInfo->SetSampleSize(i, j, 1);
                m_supportInfo->SetQualityFlagsAt(i, j, numQualityFlags);
            }
        }
    }
    ```

    > **참고**  사용 하 여 [**id3d11device2:: checkmultisamplequalitylevels1**](https://msdn.microsoft.com/library/windows/desktop/dn280494) 대신에 대 한 다중 샘플 지원을 확인 하는 경우 타일 식 리소스 버퍼입니다.

     

3.  원하는 샘플 수로 버퍼 및 렌더링 대상 보기를 만듭니다. 스왑 체인과 동일한 DXGI\_FORMAT, 너비 및 높이를 사용하되, 1보다 큰 샘플 수를 지정하고 다중 샘플링된 텍스처 크기(예: **D3D11\_RTV\_DIMENSION\_TEXTURE2DMS**)를 사용합니다. 필요한 경우 다중 샘플링에 가장 적합한 새로운 설정으로 스왑 체인을 다시 만들 수 있습니다.

    다음 코드는 다중 샘플링된 렌더링 대상을 만듭니다.

    ```cpp
    float widthMulti = m_d3dRenderTargetSize.Width;
    float heightMulti = m_d3dRenderTargetSize.Height;

    D3D11_TEXTURE2D_DESC offScreenSurfaceDesc;
    ZeroMemory(&offScreenSurfaceDesc, sizeof(D3D11_TEXTURE2D_DESC));

    offScreenSurfaceDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;
    offScreenSurfaceDesc.Width = static_cast<UINT>(widthMulti);
    offScreenSurfaceDesc.Height = static_cast<UINT>(heightMulti);
    offScreenSurfaceDesc.BindFlags = D3D11_BIND_RENDER_TARGET;
    offScreenSurfaceDesc.MipLevels = 1;
    offScreenSurfaceDesc.ArraySize = 1;
    offScreenSurfaceDesc.SampleDesc.Count = m_sampleSize;
    offScreenSurfaceDesc.SampleDesc.Quality = m_qualityFlags;

    // Create a surface that's multisampled.
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(
        &offScreenSurfaceDesc,
        nullptr,
        &m_offScreenSurface)
        );

    // Create a render target view. 
    CD3D11_RENDER_TARGET_VIEW_DESC renderTargetViewDesc(D3D11_RTV_DIMENSION_TEXTURE2DMS);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateRenderTargetView(
        m_offScreenSurface.Get(),
        &renderTargetViewDesc,
        &m_d3dRenderTargetView
        )
        );
    ```

4.  다중 샘플링된 렌더링 대상과 일치하도록 깊이 버퍼의 너비, 높이, 샘플 수 및 텍스처 크기도 동일해야 합니다.

    다음 코드는 다중 샘플링된 깊이 버퍼를 만듭니다.

    ```cpp
    // Create a depth stencil view for use with 3D rendering if needed.
    CD3D11_TEXTURE2D_DESC depthStencilDesc(
        DXGI_FORMAT_D24_UNORM_S8_UINT,
        static_cast<UINT>(widthMulti),
        static_cast<UINT>(heightMulti),
        1, // This depth stencil view has only one texture.
        1, // Use a single mipmap level.
        D3D11_BIND_DEPTH_STENCIL,
        D3D11_USAGE_DEFAULT,
        0,
        m_sampleSize,
        m_qualityFlags
        );

    ComPtr<ID3D11Texture2D> depthStencil;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(
        &depthStencilDesc,
        nullptr,
        &depthStencil
        )
        );

    CD3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc(D3D11_DSV_DIMENSION_TEXTURE2DMS);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateDepthStencilView(
        depthStencil.Get(),
        &depthStencilViewDesc,
        &m_d3dDepthStencilView
        )
        );
    ```

5.  이제 뷰포트를 만들기에 적절한 시점입니다. 뷰포트 너비와 높이도 렌더링 대상과 일치해야 하기 때문입니다.

    다음은 뷰포트를 만드는 코드입니다.

    ```cpp
    // Set the 3D rendering viewport to target the entire window.
    m_screenViewport = CD3D11_VIEWPORT(
        0.0f,
        0.0f,
        widthMulti / m_scalingFactor,
        heightMulti / m_scalingFactor
        );

    m_d3dContext->RSSetViewports(1, &m_screenViewport);
    ```

6.  각 프레임을 다중 샘플링된 렌더링 대상으로 렌더링합니다. 렌더링이 완료되면 프레임을 표시하기 전에 [**ID3D11DeviceContext::ResolveSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476474)를 호출합니다. 그러면 Direct3D에서 다중 샘플링 작업을 수행하고, 표시를 위한 각 픽셀 값을 계산하고, 결과를 백 버퍼로 보냅니다. 백 버퍼에는 표시할 수 있는 최종 앤티앨리어싱 이미지가 포함됩니다.

    다음 코드는 프레임 표시 전에 하위 리소스를 해제합니다.

    ```cpp
    if (m_sampleSize > 1)
    {
        unsigned int sub = D3D11CalcSubresource(0, 0, 1);

        m_d3dContext->ResolveSubresource(
            m_backBuffer.Get(),
            sub,
            m_offScreenSurface.Get(),
            sub,
            DXGI_FORMAT_B8G8R8A8_UNORM
            );
    }

    // The first argument instructs DXGI to block until VSync, putting the application
    // to sleep until the next VSync. This ensures that we don't waste any cycles rendering
    // frames that will never be displayed to the screen.
    hr = m_swapChain->Present(1, 0);
    ```

 

 




