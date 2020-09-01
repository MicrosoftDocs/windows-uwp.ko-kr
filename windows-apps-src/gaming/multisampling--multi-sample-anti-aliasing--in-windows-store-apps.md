---
title: UWP 앱의 다중 샘플링
description: Direct3D로 빌드된 유니버설 Windows 플랫폼 (UWP) 앱에서 다중 샘플링을 사용 하는 방법에 대해 알아봅니다.
ms.assetid: 1cd482b8-32ff-1eb0-4c91-83eb52f08484
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 다중 샘플링, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: 0fccc549deb579a51cfedaad1fd0a1e1dd861fa8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165177"
---
# <a name="span-iddev_gamingmultisampling__multi-sample_anti_aliasing__in_windows_store_appsspan-multisampling-in-universal-windows-platform-uwp-apps"></a><span id="dev_gaming.multisampling__multi-sample_anti_aliasing__in_windows_store_apps"></span> UWP (유니버설 Windows 플랫폼) 앱의 다중 샘플링



Direct3D로 빌드된 유니버설 Windows 플랫폼 (UWP) 앱에서 다중 샘플링을 사용 하는 방법에 대해 알아봅니다. 다중 샘플링 앤티 앨리어싱이 라고도 하는 다중 샘플링은 별칭 가장자리의 모양을 줄이는 데 사용 되는 그래픽 기술입니다. 실제로 최종 렌더링 대상에 있는 것 보다 많은 픽셀을 그린 다음 값의 평균을 계산 하 여 특정 픽셀에 대 한 "부분" 가장자리의 모양을 유지 하는 방식으로 작동 합니다. 다중 샘플링이 Direct3D에서 실제로 작동 하는 방식에 대 한 자세한 내용은 [다중 샘플 앤티앨리어싱 규칙](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-rasterizer-stage-rules)을 참조 하세요.

## <a name="multisampling-and-the-flip-model-swap-chain"></a>다중 샘플링 및 대칭 전환 모델 교환 체인


DirectX를 사용 하는 UWP 앱은 플립 model 스왑 체인을 사용 해야 합니다. 모델 전환 스왑 체인은 다중 샘플링을 직접 지원 하지 않지만, 다중 샘플 렌더링 대상 뷰에 장면을 렌더링 한 다음, 표시 하기 전에 다중 샘플 렌더링 대상을 백 버퍼로 확인 하 여 다른 방식으로 다중 샘플링을 적용할 수 있습니다. 이 문서에서는 UWP 앱에 다중 샘플링을 추가 하는 데 필요한 단계를 설명 합니다.

### <a name="how-to-use-multisampling"></a>다중 샘플링을 사용 하는 방법

Direct3D 기능 수준은 특정, 최소 샘플 수 기능에 대 한 지원을 보장 하 고 다중 샘플링을 지 원하는 특정 버퍼 형식을 사용할 수 있도록 보장 합니다. 그래픽 장치는 필요한 최소값 보다 더 광범위 한 형식 및 샘플 개수를 지원 합니다. 런타임에 특정 DXGI 형식으로 다중 샘플링에 대 한 기능 지원을 확인 하 고 지원 되는 각 형식에 사용할 수 있는 샘플 수를 확인 하 여 런타임에 확인할 수 있습니다.

1.  [**ID3D11Device:: CheckFeatureSupport**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkfeaturesupport) 를 호출 하 여 다중 샘플링에 사용할 수 있는 DXGI 형식을 확인 합니다. 게임에서 사용할 수 있는 렌더링 대상 형식을 제공 합니다. 렌더링 대상과 확인 대상은 모두 동일한 형식을 사용 해야 하므로 [**D3D11 \_ format \_ 지원 \_ 다중 샘플 \_ rendertarget**](/windows/desktop/api/d3d11/ne-d3d11-d3d11_format_support) 및 **D3D11 \_ format \_ support \_ 다중 샘플 \_ resolve**를 모두 확인 하세요.

    **기능 수준 9:  ** 기능 수준 9 장치가 [다중 샘플 렌더링 대상 형식에 대 한 지원을 보장](/previous-versions/ff471324(v=vs.85))하지만 다중 샘플 resolution 대상에 대 한 지원은 보장 되지 않습니다. 따라서이 항목에서 설명 하는 다중 샘플링 기법을 사용 하기 전에 확인 해야 합니다.

    다음 코드는 모든 DXGI 형식 값에 대 한 다중 샘플링 지원을 확인 합니다 \_ .

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

2.  지원 되는 각 형식에 대해 [**ID3D11Device:: CheckMultisampleQualityLevels**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkmultisamplequalitylevels)를 호출 하 여 샘플 개수 지원에 대해 쿼리 합니다.

    다음 코드에서는 지원 되는 DXGI 형식에 대 한 샘플 크기 지원을 확인 합니다.

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

    > **참고**    바둑판식 리소스 버퍼에 대 한 다중 샘플 지원을 확인 해야 하는 경우 대신 [**ID3D11Device2:: CheckMultisampleQualityLevels1**](/windows/desktop/api/d3d11_2/nf-d3d11_2-id3d11device2-checkmultisamplequalitylevels1) 를 사용 합니다.

     

3.  원하는 샘플 수를 사용 하 여 버퍼 및 렌더링 대상 뷰를 만듭니다. 스왑 체인과 동일한 DXGI \_ 형식, 너비 및 높이를 사용 하지만 샘플 개수를 1 보다 크게 지정 하 고 다중 샘플 텍스처 차원 (예:**D3D11 \_ rtv \_ dimension \_ TEXTURE2DMS** )을 사용 합니다. 필요한 경우 다중 샘플링에 가장 적합 한 새 설정을 사용 하 여 스왑 체인을 다시 만들 수 있습니다.

    다음 코드는 다중 샘플 렌더링 대상을 만듭니다.

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

4.  깊이 버퍼의 너비, 높이, 샘플 수 및 텍스처 차원은 다중 샘플 렌더링 대상과 일치 해야 합니다.

    다음 코드는 다중 샘플 depth 버퍼를 만듭니다.

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

5.  뷰포트 너비와 높이가 렌더링 대상과도 일치 해야 하기 때문에 이제 뷰포트를 만드는 것이 좋습니다.

    다음 코드는 뷰포트를 만듭니다.

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

6.  각 프레임을 다중 샘플 렌더링 대상으로 렌더링 합니다. 렌더링이 완료 되 면 프레임을 표시 하기 전에 [**ID3D11DeviceContext:: ResolveSubresource**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-resolvesubresource) 를 호출 합니다. 그러면 Direct3D가 다중 샘플링 작업을 수행 하 여 표시를 위해 각 픽셀의 값을 계산 하 고 결과를 백 버퍼에 배치 하도록 지시 합니다. 그런 다음 백 버퍼는 최종 앤티앨리어싱 이미지를 포함 하 여 표시 될 수 있습니다.

    다음 코드는 프레임을 표시 하기 전에 subresource를 확인 합니다.

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

 

 