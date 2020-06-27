---
title: 렌더링 소개
description: 렌더링 파이프라인을 개발 하 여 그래픽을 표시 하는 방법을 알아봅니다. 렌더링을 소개 합니다.
ms.assetid: 1da3670b-2067-576f-da50-5eba2f88b3e6
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 렌더링
ms.localizationpriority: medium
ms.openlocfilehash: d1abb324c5e9e16babbbf8d3650adc39cb995137
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409642"
---
# <a name="rendering-framework-i-intro-to-rendering"></a>렌더링 프레임 워크 I: 렌더링 소개

> [!NOTE]
> 이 항목은 DirectX 자습서 시리즈 [를 사용 하 여 단순 유니버설 Windows 플랫폼 (UWP) 게임 만들기](tutorial--create-your-first-uwp-directx-game.md) 의 일부입니다. 해당 링크의 항목은 계열의 컨텍스트를 설정 합니다.

지금까지 UWP (유니버설 Windows 플랫폼) 게임을 구성 하는 방법과 게임 흐름을 처리 하는 상태 시스템을 정의 하는 방법을 살펴보았습니다. 이제 렌더링 프레임 워크를 개발 하는 방법을 배울 수 있습니다. 샘플 게임에서 Direct3D 11을 사용 하 여 게임 장면을 렌더링 하는 방법을 살펴보겠습니다.

Direct3D 11에는 게임과 같은 그래픽 집약적 응용 프로그램용 3D 그래픽을 만드는 데 사용할 수 있는 고성능 그래픽 하드웨어의 고급 기능에 대 한 액세스를 제공 하는 Api 집합이 포함 되어 있습니다.

게임 그래픽을 화면에 렌더링 하는 것은 기본적으로 화면에 프레임 시퀀스를 렌더링 한다는 의미입니다. 각 프레임에서 뷰에 따라 장면에 표시 되는 개체를 렌더링 해야 합니다.

프레임을 렌더링 하려면 화면에 표시 될 수 있도록 필요한 장면 정보를 하드웨어에 전달 해야 합니다. 화면에 아무것도 표시 하려면 게임 실행이 시작 되는 즉시 렌더링을 시작 해야 합니다.

## <a name="objectives"></a>목표

UWP DirectX 게임의 그래픽 출력을 표시 하는 기본 렌더링 프레임 워크를 설정 합니다. 이 세 단계로 느슨하게 나눌 수 있습니다.

1. 그래픽 인터페이스에 대 한 연결을 설정 합니다.
2. 그래픽을 그리는 데 필요한 리소스를 만듭니다.
3. 프레임을 렌더링 하 여 그래픽을 표시 합니다.

이 항목에서는 1 단계 및 3 단계를 포함 하 여 그래픽이 렌더링 되는 방식에 대해 설명 합니다.

[렌더링 프레임 워크 II: 게임 렌더링](tutorial-game-rendering.md) 은 2 단계 렌더링 &mdash; 프레임 워크를 설정 하는 방법 및 렌더링을 수행 하기 전에 데이터를 준비 하는 방법에 대해 설명 합니다.

## <a name="get-started"></a>시작하기

기본적인 그래픽 및 렌더링 개념을 이해 하는 것이 좋습니다. Direct3D 및 렌더링을 처음 접하는 경우이 항목에서 사용 되는 그래픽 및 렌더링 용어에 대 한 간략 한 설명은 [용어 및 개념](#terms-and-concepts) 을 참조 하세요.

이 게임에서 **GameRenderer** 클래스는이 샘플 게임의 렌더러를 나타냅니다. 게임 시각적 개체를 생성 하는 데 사용 되는 모든 Direct3D 11 및 Direct2D 개체를 만들고 유지 관리 하는 일을 담당 합니다. 또한 렌더링할 개체 목록을 검색 하는 데 사용 되는 **Simple3DGame** 개체에 대 한 참조 뿐만 아니라 HUD (준비 디스플레이)의 게임 상태를 유지 관리 합니다.

자습서의이 부분에서는 게임에서 3D 개체를 렌더링 하는 데 초점을 둡니다.

## <a name="establish-a-connection-to-the-graphics-interface"></a>그래픽 인터페이스에 대 한 연결 설정

렌더링을 위한 하드웨어에 액세스 하는 방법에 대 한 정보는 [게임의 UWP 앱 프레임 워크 정의](tutorial--building-the-games-uwp-app-framework.md#the-appinitialize-method) 항목을 참조 하세요.

### <a name="the-appinitialize-method"></a>App:: Initialize 메서드

아래와 같이 **std:: make_shared** 함수를 사용 하 여 장치에 대 한 액세스를 제공 하는 **DX::D eviceresources**에 대 한 **shared_ptr** 를 만듭니다.

Direct3D 11에서 [장치](#device) 를 사용 하 여 개체를 할당 및 제거 하 고, 기본 형식을 렌더링 하 고, 그래픽 드라이버를 통해 그래픽 카드와 통신 합니다.

```cppwinrt
void Initialize(CoreApplicationView const& applicationView)
{
    ...

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

## <a name="display-the-graphics-by-rendering-the-frame"></a>프레임을 렌더링 하 여 그래픽 표시

게임을 시작할 때 게임 장면을 렌더링 해야 합니다. 렌더링에 대 한 지침은 아래와 같이 [**GameMain:: Run**](#gamemainrun-method) 메서드에서 시작 합니다.

간단한 흐름은 다음과 같습니다.

1. **Update**
2. **Render**
3. **있음**

### <a name="gamemainrun-method"></a>GameMain:: Run 메서드

```cppwinrt
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible) // if the window is visible
        {
            switch (m_updateState)
            {
            ...
            default:
                CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close, so save state.
}
```

### <a name="update"></a>업데이트

[**GameMain:: Update**](tutorial-game-flow-management.md#the-gamemainupdate-method) 메서드에서 게임 상태를 업데이트 하는 방법에 대 한 자세한 내용은 [game flow 관리](tutorial-game-flow-management.md) 항목을 참조 하세요.

### <a name="render"></a>렌더링

렌더링은 **GameMain:: Run**에서 [**GameRenderer:: Render**](#gamerendererrender-method) 메서드를 호출 하 여 구현 합니다.

[스테레오 렌더링이](#stereo-rendering) 사용 하도록 설정 된 경우 왼쪽에는 두 개의 렌더링이 전달 되 고, 하나는 오른쪽으로 전달 됩니다 &mdash; . 각 렌더링 패스에서 렌더링 대상과 깊이 스텐실 뷰를 장치에 바인딩합니다. 또한 나중에 깊이 스텐실 뷰를 지웁니다.

> [!NOTE]
> 꼭 짓 점 인스턴스화 또는 기 하 도형 셰이더를 사용 하 여 단일 패스 스테레오 같은 다른 방법을 사용 하 여 스테레오 렌더링을 수행할 수 있습니다. 두 렌더링-전달 메서드는 속도가 느리고 더 편리한 방식으로 스테레오 렌더링을 구현 합니다.

게임을 실행 하 고 리소스를 로드 한 후에는 렌더링 패스 마다 한 번씩 [프로젝션 매트릭스](#projection-transform-matrix)를 업데이트 합니다. 개체는 각 뷰와 약간 다릅니다. 다음으로, [그래픽 렌더링 파이프라인](#rendering-pipeline)을 설정 합니다. 

> [!NOTE]
> 리소스를 로드 하는 방법에 대 한 자세한 내용은 [DirectX 그래픽 리소스 만들기 및 로드](tutorial-game-rendering.md#create-and-load-directx-graphic-resources) 를 참조 하세요.

이 샘플 게임에서 렌더러는 모든 개체에서 표준 꼭 짓 점 레이아웃을 사용 하도록 디자인 되었습니다. 이렇게 하면 셰이더 디자인이 간소화 되 고 개체의 기 하 도형에 독립적으로 셰이더를 쉽게 변경할 수 있습니다.

#### <a name="gamerendererrender-method"></a>GameRenderer:: Render 메서드

입력 꼭 짓 점 레이아웃을 사용 하도록 Direct3D 컨텍스트를 설정 합니다. 입력 레이아웃 개체는 꼭 짓 점 버퍼 데이터가 [렌더링 파이프라인](#rendering-pipeline)으로 스트리밍되는 방식을 설명 합니다. 

다음으로, 이전에 정의 된 상수 버퍼를 사용 하도록 Direct3D 컨텍스트를 설정 합니다 .이는 [꼭 짓 점 셰이더](#vertex-shaders-and-pixel-shaders) 파이프라인 단계와 [픽셀 셰이더](#vertex-shaders-and-pixel-shaders) 파이프라인 단계에서 사용 됩니다. 

> [!NOTE]
> 상수 버퍼의 정의에 대 한 자세한 내용은 [렌더링 프레임 워크 II: 게임 렌더링](tutorial-game-rendering.md) 을 참조 하세요.

파이프라인에 있는 모든 셰이더에 동일한 입력 레이아웃 및 상수 버퍼 집합이 사용 되므로 프레임당 한 번 설정 됩니다.

```cppwinrt
void GameRenderer::Render()
{
    bool stereoEnabled{ m_deviceResources->GetStereoState() };

    auto d3dContext{ m_deviceResources->GetD3DDeviceContext() };
    auto d2dContext{ m_deviceResources->GetD2DDeviceContext() };

    int renderingPasses = 1;
    if (stereoEnabled)
    {
        renderingPasses = 2;
    }

    for (int i = 0; i < renderingPasses; i++)
    {
        // Iterate through the number of rendering passes to be completed.
        // 2 rendering passes if stereo is enabled.
        if (i > 0)
        {
            // Doing the Right Eye View.
            ID3D11RenderTargetView* const targets[1] = { m_deviceResources->GetBackBufferRenderTargetViewRight() };

            // Resets render targets to the screen.
            // OMSetRenderTargets binds 2 things to the device.
            // 1. Binds one render target atomically to the device.
            // 2. Binds the depth-stencil view, as returned by the GetDepthStencilView method, to the device.
            // For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets

            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());

            // Clears the depth stencil view.
            // A depth stencil view contains the format and buffer to hold depth and stencil info.
            // For more info about depth stencil view, go to: 
            // https://docs.microsoft.com/windows/uwp/graphics-concepts/depth-stencil-view--dsv-
            // A depth buffer is used to store depth information to control which areas of 
            // polygons are rendered rather than hidden from view. To learn more about a depth buffer,
            // go to: https://docs.microsoft.com/windows/uwp/graphics-concepts/depth-buffers
            // A stencil buffer is used to mask pixels in an image, to produce special effects. 
            // The mask determines whether a pixel is drawn or not,
            // by setting the bit to a 1 or 0. To learn more about a stencil buffer,
            // go to: https://docs.microsoft.com/windows/uwp/graphics-concepts/stencil-buffers

            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);

            // Direct2D -- discussed later
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmapRight());
        }
        else
        {
            // Doing the Mono or Left Eye View.
            // As compared to the right eye:
            // m_deviceResources->GetBackBufferRenderTargetView instead of GetBackBufferRenderTargetViewRight
            ID3D11RenderTargetView* const targets[1] = { m_deviceResources->GetBackBufferRenderTargetView() };

            // Same as the Right Eye View.
            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());
            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);

            // d2d -- Discussed later under Adding UI
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmap());
        }

        const float clearColor[4] = { 0.5f, 0.5f, 0.8f, 1.0f };

        // Only need to clear the background when not rendering the full 3D scene since
        // the 3D world is a fully enclosed box and the dynamics prevents the camera from
        // moving outside this space.
        if (i > 0)
        {
            // Doing the Right Eye View.
            d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetViewRight(), clearColor);
        }
        else
        {
            // Doing the Mono or Left Eye View.
            d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), clearColor);
        }

        // Render the scene objects
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            if (stereoEnabled)
            {
                // When doing stereo, it is necessary to update the projection matrix once per rendering pass.

                auto orientation = m_deviceResources->GetOrientationTransform3D();

                ConstantBufferChangeOnResize changesOnResize;
                // Apply either a left or right eye projection, which is an offset from the middle
                XMStoreFloat4x4(
                    &changesOnResize.projection,
                    XMMatrixMultiply(
                        XMMatrixTranspose(
                            i == 0 ?
                            m_game->GameCamera().LeftEyeProjection() :
                            m_game->GameCamera().RightEyeProjection()
                            ),
                        XMMatrixTranspose(XMLoadFloat4x4(&orientation))
                        )
                    );

                d3dContext->UpdateSubresource(
                    m_constantBufferChangeOnResize.get(),
                    0,
                    nullptr,
                    &changesOnResize,
                    0,
                    0
                    );
            }

            // Update variables that change once per frame.
            ConstantBufferChangesEveryFrame constantBufferChangesEveryFrameValue;
            XMStoreFloat4x4(
                &constantBufferChangesEveryFrameValue.view,
                XMMatrixTranspose(m_game->GameCamera().View())
                );
            d3dContext->UpdateSubresource(
                m_constantBufferChangesEveryFrame.get(),
                0,
                nullptr,
                &constantBufferChangesEveryFrameValue,
                0,
                0
                );

            // Set up the graphics pipeline. This sample uses the same InputLayout and set of
            // constant buffers for all shaders, so they only need to be set once per frame.
            // For more info about the graphics or rendering pipeline, see
            // https://docs.microsoft.com/windows/win32/direct3d11/overviews-direct3d-11-graphics-pipeline

            // IASetInputLayout binds an input-layout object to the input-assembler (IA) stage. 
            // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
            // Set up the Direct3D context to use this vertex layout. For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout
            d3dContext->IASetInputLayout(m_vertexLayout.get());

            // VSSetConstantBuffers sets the constant buffers used by the vertex shader pipeline stage.
            // Set up the Direct3D context to use these constant buffers. For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-vssetconstantbuffers

            ID3D11Buffer* constantBufferNeverChanges{ m_constantBufferNeverChanges.get() };
            d3dContext->VSSetConstantBuffers(0, 1, &constantBufferNeverChanges);
            ID3D11Buffer* constantBufferChangeOnResize{ m_constantBufferChangeOnResize.get() };
            d3dContext->VSSetConstantBuffers(1, 1, &constantBufferChangeOnResize);
            ID3D11Buffer* constantBufferChangesEveryFrame{ m_constantBufferChangesEveryFrame.get() };
            d3dContext->VSSetConstantBuffers(2, 1, &constantBufferChangesEveryFrame);
            ID3D11Buffer* constantBufferChangesEveryPrim{ m_constantBufferChangesEveryPrim.get() };
            d3dContext->VSSetConstantBuffers(3, 1, &constantBufferChangesEveryPrim);

            // Sets the constant buffers used by the pixel shader pipeline stage. 
            // For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-pssetconstantbuffers

            d3dContext->PSSetConstantBuffers(2, 1, &constantBufferChangesEveryFrame);
            d3dContext->PSSetConstantBuffers(3, 1, &constantBufferChangesEveryPrim);
            ID3D11SamplerState* samplerLinear{ m_samplerLinear.get() };
            d3dContext->PSSetSamplers(0, 1, &samplerLinear);

            for (auto&& object : m_game->RenderObjects())
            {
                // The 3D object render method handles the rendering.
                // For more info, see Primitive rendering below.
                object->Render(d3dContext, m_constantBufferChangesEveryPrim.get());
            }
        }

        // Start of 2D rendering
        ...
    }
}
```

### <a name="primitive-rendering"></a>기본 렌더링

장면을 렌더링할 때 렌더링 해야 하는 모든 개체를 반복 합니다. 아래 단계는 각 개체 (기본 형식)에 대해 반복 됩니다.

- 모델의 [세계 변환 매트릭스](#world-transform-matrix) 및 재질 정보를 사용 하 여 상수 버퍼 (**m_constantBufferChangesEveryPrim**)를 업데이트 합니다.
- **M_constantBufferChangesEveryPrim** 는 각 개체에 대 한 매개 변수를 포함 합니다. 여기에는 조명 계산에 대 한 색 및 반사 지 수와 같은 재질 속성 뿐만 아니라 개체 간 변환 행렬이 포함 됩니다.
- [렌더링 파이프라인](#rendering-pipeline)의 IA (입력-어셈블러) 단계로 스트리밍되는 메시 개체 데이터에 대 한 입력 꼭 짓 점 레이아웃을 사용 하도록 Direct3D 컨텍스트를 설정 합니다.
- IA 단계에서 [인덱스 버퍼](#index-buffer) 를 사용 하도록 Direct3D 컨텍스트를 설정 합니다. 기본 정보: 형식, 데이터 순서를 제공 합니다.
- 그리기 호출을 전송 하 여 인스턴스화된 인덱싱되지 않은 기본 형식을 그립니다. **GameObject:: Render** 메서드는 지정 된 기본 형식에 해당 하는 데이터로 기본 [상수 버퍼](#constant-buffer-or-shader-constant-buffer) 를 업데이트 합니다. 그러면 컨텍스트에 대해 **DrawIndexed** 를 호출 하 여 각 기본 형식의 기 하 도형을 그립니다. 특히이 그리기 호출은 상수 버퍼 데이터로 매개 변수화 된 명령 및 데이터를 GPU (그래픽 처리 장치)에 큐에 대기 시킵니다. 각 그리기 호출에서 꼭 짓 점 마다 한 번씩 꼭 짓 점 셰이더를 실행 한 다음 기본 형식에서 각 삼각형의 픽셀 마다 한 번씩 [픽셀 셰이더](#vertex-shaders-and-pixel-shaders) 를 실행 합니다. 질감은 픽셀 셰이더가 렌더링을 수행 하는 데 사용 하는 상태의 일부입니다.

여러 상수 버퍼를 사용 하는 이유는 다음과 같습니다.

- 게임은 여러 상수 버퍼를 사용 하지만 이러한 버퍼는 기본 형식 마다 한 번만 업데이트 하면 됩니다. 앞에서 설명한 것 처럼 상수 버퍼는 각 기본 형식에 대해 실행 되는 셰이더에 대 한 입력과 유사 합니다. 일부 데이터가 정적 (**m_constantBufferNeverChanges**)입니다. 일부 데이터는 카메라의 위치와 같은 프레임 (**m_constantBufferChangesEveryFrame**)을 통해 일정 합니다. 일부 데이터는 색 및 질감 (**m_constantBufferChangesEveryPrim**)과 같은 기본 형식에만 적용 됩니다.
- 게임 렌더러는 이러한 입력을 다른 상수 버퍼로 분리 하 여 CPU와 GPU에서 사용 하는 메모리 대역폭을 최적화 합니다. 이 방법은 GPU가 추적 해야 하는 데이터의 양을 최소화 하는 데도 도움이 됩니다. GPU에는 큰 명령 큐가 있으며, 게임에서 **그리기**를 호출할 때마다 해당 명령은 연결 된 데이터와 함께 큐에 들어갑니다. 게임에서 기본 상수 버퍼를 업데이트 하 고 다음 **그리기** 명령을 실행 하면 그래픽 드라이버는이 다음 명령과 연결 된 데이터를 큐에 추가 합니다. 게임에서 100 기본 형식을 그린 경우 큐에 있는 상수 버퍼 데이터의 복사본을 100 개까지 포함할 수 있습니다. 게임에서 GPU로 전송 되는 데이터의 양을 최소화 하기 위해 게임은 각 기본 형식에 대 한 업데이트만 포함 하는 별도의 기본 상수 버퍼를 사용 합니다.

#### <a name="gameobjectrender-method"></a>GameObject:: Render 메서드

```cppwinrt
void GameObject::Render(
    _In_ ID3D11DeviceContext* context,
    _In_ ID3D11Buffer* primitiveConstantBuffer
    )
{
    if (!m_active || (m_mesh == nullptr) || (m_normalMaterial == nullptr))
    {
        return;
    }

    ConstantBufferChangesEveryPrim constantBuffer;

    // Put the model matrix info into a constant buffer, in world matrix.
    XMStoreFloat4x4(
        &constantBuffer.worldMatrix,
        XMMatrixTranspose(ModelMatrix())
        );

    // Check to see which material to use on the object.
    // If a collision (a hit) is detected, GameObject::Render checks the current context, which 
    // indicates whether the target has been hit by an ammo sphere. If the target has been hit, 
    // this method applies a hit material, which reverses the colors of the rings of the target to 
    // indicate a successful hit to the player. Otherwise, it applies the default material 
    // with the same method. In both cases, it sets the material by calling Material::RenderSetup, 
    // which sets the appropriate constants into the constant buffer. Then, it calls 
    // ID3D11DeviceContext::PSSetShaderResources to set the corresponding texture resource for the 
    // pixel shader, and ID3D11DeviceContext::VSSetShader and ID3D11DeviceContext::PSSetShader 
    // to set the vertex shader and pixel shader objects themselves, respectively.

    if (m_hit && m_hitMaterial != nullptr)
    {
        m_hitMaterial->RenderSetup(context, &constantBuffer);
    }
    else
    {
        m_normalMaterial->RenderSetup(context, &constantBuffer);
    }

    // Update the primitive constant buffer with the object model's info.
    context->UpdateSubresource(primitiveConstantBuffer, 0, nullptr, &constantBuffer, 0, 0);

    // Render the mesh.
    // See MeshObject::Render method below.
    m_mesh->Render(context);
}
```

#### <a name="meshobjectrender-method"></a>MeshObject:: Render 메서드

```cppwinrt
void MeshObject::Render(_In_ ID3D11DeviceContext* context)
{
    // PNTVertex is a struct. stride provides us the size required for all the mesh data
    // struct PNTVertex
    //{
    //  DirectX::XMFLOAT3 position;
    //  DirectX::XMFLOAT3 normal;
    //  DirectX::XMFLOAT2 textureCoordinate;
    //};
    uint32_t stride{ sizeof(PNTVertex) };
    uint32_t offset{ 0 };

    // Similar to the main render loop.
    // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
    ID3D11Buffer* vertexBuffer{ m_vertexBuffer.get() };
    context->IASetVertexBuffers(0, 1, &vertexBuffer, &stride, &offset);

    // IASetIndexBuffer binds an index buffer to the input-assembler stage.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer.
    context->IASetIndexBuffer(m_indexBuffer.get(), DXGI_FORMAT_R16_UINT, 0);

    // Binds information about the primitive type, and data order that describes input data for the input assembler stage.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology.
    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

    // Draw indexed, non-instanced primitives. A draw API submits work to the rendering pipeline.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed.
    context->DrawIndexed(m_indexCount, 0, 0);
}
```

### <a name="deviceresourcespresent-method"></a>DeviceResources::P 다시 보낸 메서드

**DeviceResources::P 다시 보낸** 메서드를 호출 하 여 버퍼에 배치한 내용을 표시 합니다.

사용자에 게 프레임을 표시 하는 데 사용 되는 버퍼 컬렉션에 대해 바꾸기 체인 이라는 용어를 사용 합니다. 응용 프로그램이 표시를 위해 새 프레임을 제공할 때마다 스왑 체인의 첫 번째 버퍼가 표시 된 버퍼의 자리를 차지 합니다. 이 프로세스를 스와핑 또는 대칭 이동 이라고 합니다. 자세한 내용은 [교환 체인](../graphics-concepts/swap-chains.md)을 참조 하세요.

- **IDXGISwapChain1** 인터페이스의 **Present** 메서드는 VSync (수직 동기화)가 수행 될 때까지 [DXGI](#dxgi) 가 차단 되도록 지시 하 여 응용 프로그램이 다음 VSync 될 때까지 절전 모드로 전환 되도록 합니다. 이렇게 하면 화면에 표시 되지 않는 모든 주기 렌더링 프레임을 낭비 하지 않습니다.
- **ID3D11DeviceContext3** 인터페이스의 **DiscardView** 메서드는 [렌더링 대상](#render-target)의 콘텐츠를 삭제 합니다. 이 작업은 기존 콘텐츠를 완전히 덮어쓰는 경우에만 유효한 작업입니다. 더티 또는 스크롤을 사용 하는 경우이 호출을 제거 해야 합니다.
* 동일한 **DiscardView** 메서드를 사용 하 여 [깊이 스텐실](#depth-stencil)의 콘텐츠를 삭제 합니다.
* **HandleDeviceLost** 메서드는 제거 되는 [장치의](#device) 시나리오를 관리 하는 데 사용 됩니다. 연결 끊기 또는 드라이버 업그레이드로 인해 장치가 제거 된 경우 모든 장치 리소스를 다시 만들어야 합니다. 자세한 내용은 [Direct3D 11에서 장치 제거 시나리오 처리](handling-device-lost-scenarios.md)를 참조 하세요.

> [!TIP]
> 부드러운 프레임 속도로 프레임을 렌더링 하는 작업 양이 VSyncs 사이의 시간에 맞는지 확인 해야 합니다.

```cppwinrt
// Present the contents of the swap chain to the screen.
void DX::DeviceResources::Present()
{
    // The first argument instructs DXGI to block until VSync, putting the application
    // to sleep until the next VSync. This ensures we don't waste any cycles rendering
    // frames that will never be displayed to the screen.
    HRESULT hr = m_swapChain->Present(1, 0);

    // Discard the contents of the render target.
    // This is a valid operation only when the existing contents will be entirely
    // overwritten. If dirty or scroll rects are used, this call should be removed.
    m_d3dContext->DiscardView(m_d3dRenderTargetView.get());

    // Discard the contents of the depth stencil.
    m_d3dContext->DiscardView(m_d3dDepthStencilView.get());

    // If the device was removed either by a disconnection or a driver upgrade, we 
    // must recreate all device resources.
    if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
    {
        HandleDeviceLost();
    }
    else
    {
        winrt::check_hresult(hr);
    }
}
```

## <a name="next-steps"></a>다음 단계

이 항목에서는 디스플레이에서 그래픽을 렌더링 하는 방법에 대해 설명 하 고 사용 된 일부 렌더링 용어에 대 한 간략 한 설명을 제공 합니다 (아래 참조). 렌더링 [프레임 워크 II: 게임 렌더링](tutorial-game-rendering.md) 항목에서 렌더링 하는 방법에 대해 자세히 알아보고 렌더링 하기 전에 필요한 데이터를 준비 하는 방법을 알아보세요.

## <a name="terms-and-concepts"></a>용어 및 개념

### <a name="simple-game-scene"></a>간단한 게임 장면

간단한 게임 장면을 몇 개의 광원으로 구성 된 몇 개의 개체로 구성 됩니다.

개체의 도형은 공간에서 X, Y, Z 좌표 집합으로 정의 됩니다. Game 세계의 실제 렌더링 위치는 X, Y, Z 좌표 위치에 변형 행렬을 적용 하 여 확인할 수 있습니다. 또한 &mdash; &mdash; 개체에 재질을 적용 하는 방법을 지정 하는 질감 좌표 집합 및 V가 있을 수 있습니다. 이는 개체의 표면 속성을 정의 하 고, 개체에 거친 화면 (예: 테니스 구슬) 또는 부드러운 광택 표면 (예: 볼링 구슬)이 있는지를 확인 하는 기능을 제공 합니다.

장면 및 개체 정보는 렌더링 프레임 워크에서 프레임을 사용 하 여 장면 프레임을 다시 만들고 표시 모니터에서 활성 상태로 전환 하는 데 사용 됩니다.

### <a name="rendering-pipeline"></a>렌더링 파이프라인

렌더링 파이프라인은 3D 장면 정보를 화면에 표시 되는 이미지로 변환 하는 프로세스입니다. Direct3D 11에서이 파이프라인은 프로그래밍 가능 합니다. 렌더링 요구를 지원 하도록 단계를 조정할 수 있습니다. 일반적인 셰이더 코어 기능을 사용 하는 단계는 HLSL 프로그래밍 언어를 사용 하 여 프로그래밍할 수 있습니다. 이를 *그래픽 렌더링 파이프라인*또는 단순한 *파이프라인*이라고 합니다.

이 파이프라인을 만드는 데 도움이 되도록 이러한 세부 정보에 대해 잘 알고 있어야 합니다.

- [HLSL](#hlsl). UWP DirectX 게임의 경우 HLSL 셰이더 모델 5.1 이상을 사용 하는 것이 좋습니다.
- [셰이더](#shaders).
- [꼭 짓 점 셰이더 및 픽셀 셰이더](#vertex-shaders-and-pixel-shaders).
- [셰이더 단계](#shader-stages).
- [다양 한 셰이더 파일 형식](#various-shader-file-formats).

자세한 내용은 [Direct3D 11 렌더링 파이프라인](/windows/desktop/direct3dgetstarted/understand-the-directx-11-2-graphics-pipeline) 및 [그래픽 파이프라인](/windows/desktop/direct3d11/overviews-direct3d-11-graphics-pipeline)이해를 참조 하세요.

#### <a name="hlsl"></a>HLSL

HLSL는 DirectX에 대 한 상위 수준 셰이더 언어입니다. HLSL를 사용 하 여 Direct3D 파이프라인에 대 한 C #와 같은 프로그래밍 가능한 셰이더를 만들 수 있습니다. 자세한 내용은 [HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl)를 참조 하세요.

#### <a name="shaders"></a>셰이더

셰이더는 렌더링 될 때 개체의 표면이 표시 되는 방법을 결정 하는 일련의 지침으로 간주할 수 있습니다. HLSL를 사용 하 여 프로그래밍 된 것을 HLSL 셰이더 라고 합니다. [HLSL]) (#hlsl) 셰이더에 대 한 소스 코드 파일에는 `.hlsl` 파일 확장명이 있습니다. 이러한 셰이더는 빌드 타임 또는 런타임에 컴파일될 수 있으며 런타임에 적절 한 파이프라인 단계로 설정 됩니다. 컴파일된 셰이더 개체에는 `.cso` 파일 확장명이 있습니다.

Direct3D 9 셰이더는 셰이더 모델 1, 셰이더 모델 2 및 셰이더 모델 3을 사용 하 여 디자인할 수 있습니다. Direct3D 10 셰이더는 셰이더 모델 4 에서만 디자인할 수 있습니다. Direct3D 11 셰이더는 셰이더 모델 5에서 디자인할 수 있습니다. Direct3D 11.3 및 Direct3D 12는 셰이더 모델 5.1에서 디자인할 수 있으며, Direct3D 12는 셰이더 모델 6에서 디자인할 수도 있습니다.

#### <a name="vertex-shaders-and-pixel-shaders"></a>꼭 짓 점 셰이더 및 픽셀 셰이더

데이터는 그래픽 파이프라인을 기본 형식 스트림으로 입력 하 고 꼭 짓 점 셰이더 및 픽셀 셰이더와 같은 다양 한 셰이더에 의해 처리 됩니다. 

꼭 짓 점 셰이더는 일반적으로 변환, 스킨, 조명 등의 작업을 수행 하는 꼭 짓 점을 처리 합니다. 픽셀 셰이더를 사용 하면 픽셀 별 조명 및 사후 처리와 같은 풍부한 음영 기술을 사용할 수 있습니다. 상수 변수, 질감 데이터, 보간된 꼭지점 별 값 및 기타 데이터를 결합 하 여 픽셀 당 출력을 생성 합니다. 

#### <a name="shader-stages"></a>셰이더 단계

이 기본 스트림을 처리 하도록 정의 된 이러한 여러 셰이더 시퀀스는 렌더링 파이프라인에서 셰이더 단계 라고 합니다. 실제 단계는 Direct3D 버전에 따라 다르지만 일반적으로 꼭 짓 점, 기 하 도형 및 픽셀 단계를 포함 합니다. 또한 공간 분할에 대 한 선체 및 도메인 셰이더와 계산 셰이더가 같은 다른 단계가 있습니다. 이러한 모든 단계는 [HLSL](#hlsl)을 사용 하 여 완벽 하 게 프로그래밍할 수 있습니다. 자세한 내용은 [그래픽 파이프라인](/windows/desktop/direct3d11/overviews-direct3d-11-graphics-pipeline)을 참조 하세요.

#### <a name="various-shader-file-formats"></a>다양 한 셰이더 파일 형식

다음은 셰이더 코드 파일 확장명입니다.

- `.hlsl`확장명이 [HLSL] #hlsl) 인 파일은 소스 코드를 포함 합니다.
- 확장명이 인 파일은 `.cso` 컴파일된 셰이더 개체를 보유 합니다.
- 확장명이 인 파일 `.h` 은 헤더 파일 이지만 셰이더 코드 컨텍스트에서이 헤더 파일은 셰이더 데이터를 포함 하는 바이트 배열을 정의 합니다.
- 확장명이 있는 파일에는 `.hlsli` 상수 버퍼의 형식이 포함 됩니다. 샘플 게임에서 파일은 **셰이더**  >  **ConstantBuffers. .hlsli**입니다.

> [!NOTE]
> `.cso`런타임에 파일을 로드 하거나 실행 코드에 파일을 추가 하 여 셰이더를 포함 `.h` 합니다. 하지만 동일한 셰이더에 사용 하지 않는 것이 좋습니다.

### <a name="deeper-understanding-of-directx"></a>DirectX의 심층적 이해

Direct3D 11은 게임과 같은 그래픽 집약적 응용 프로그램을 위한 그래픽을 만드는 데 도움이 되는 Api 집합입니다. 여기에는 집약적 계산을 처리 하는 데 적합 한 그래픽 카드가 필요 합니다. 이 섹션에서는 Direct3D 11 그래픽 프로그래밍 개념 (리소스, 하위 리소스, 장치 및 장치 컨텍스트)에 대해 간략하게 설명 합니다.

#### <a name="resource"></a>리소스

리소스 (장치 리소스 라고도 함)는 개체를 렌더링 하는 방법에 대 한 정보 (예: 질감, 위치 또는 색)로 간주할 수 있습니다. 리소스는 파이프라인에 데이터를 제공 하 고 장면 중 렌더링 되는 항목을 정의 합니다. 게임 미디어에서 리소스를 로드 하거나 런타임에 동적으로 만들 수 있습니다.

실제로 리소스는 Direct3D [파이프라인](#rendering-pipeline)에서 액세스할 수 있는 메모리 영역입니다. 파이프라인에서 메모리에 효율적으로 액세스하려면 파이프라인에 제공되는 데이터(입력 기하 도형, 셰이더 리소스, 질감 등)를 리소스에 저장해야 합니다. 모든 Direct3D 리소스가 파생되는 두 가지 리소스 종류는 버퍼와 질감입니다. 각 파이프라인 단계마다 최대 128개의 리소스를 활성화할 수 있습니다. 자세한 내용은 [리소스](../graphics-concepts/resources.md)를 참조하세요.

#### <a name="subresource"></a>하위 리소스

하위 리소스 라는 용어는 리소스의 하위 집합을 나타냅니다. Direct3D는 전체 리소스를 참조 하거나 리소스의 하위 집합을 참조할 수 있습니다. 자세한 내용은 [Subresource](../graphics-concepts/resource-types.md#subresources)를 참조 하세요.

#### <a name="depth-stencil"></a>깊이 스텐실

깊이 스텐실 리소스는 깊이와 스텐실 정보를 저장할 형식 및 버퍼를 포함 합니다. 텍스처 리소스를 사용 하 여 생성 됩니다. 깊이 스텐실 리소스를 만드는 방법에 대 한 자세한 내용은 [깊이 스텐실 기능 구성](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-depth-stencil)을 참조 하세요. [ID3D11DepthStencilView](/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilview) 인터페이스를 사용 하 여 구현 된 깊이 스텐실 뷰를 통해 깊이 스텐실 리소스에 액세스 합니다.

깊이 정보는 어떤 다각형이 어떤 영역을 다른 지에 대 한 정보를 제공 하 여 어떤 다각형이 숨겨져 있는지 확인할 수 있도록 합니다. 스텐실 정보는 마스크 된 픽셀을 알려 줍니다. 픽셀을 그릴지 여부를 결정 하므로 특수 효과를 생성 하는 데 사용할 수 있습니다. 비트를 1 또는 0으로 설정 합니다. 

자세한 내용은 [깊이 스텐실 뷰](../graphics-concepts/depth-stencil-view--dsv-.md), [깊이 버퍼](../graphics-concepts/depth-buffers.md)및 [스텐실 버퍼](../graphics-concepts/stencil-buffers.md)를 참조 하세요.

#### <a name="render-target"></a>렌더링 대상

렌더링 대상은 렌더링 패스의 끝에서 쓸 수 있는 리소스입니다. 일반적으로이 메서드는 [ID3D11Device:: CreateRenderTargetView](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview) 메서드를 사용 하 여 입력 매개 변수로 스왑 체인 백 버퍼 (이기도 한 리소스)를 사용 하 여 생성 됩니다. 

[OMSetRenderTargets](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) 를 사용 하 여 렌더링 대상을 설정 하는 경우에도 깊이 스텐실 뷰가 필요 하므로 각 렌더링 대상에는 해당 하는 깊이 스텐실 보기가 있어야 합니다. [ID3D11RenderTargetView](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) 인터페이스를 사용 하 여 구현 된 렌더링 대상 뷰를 통해 렌더링 대상 리소스에 액세스 합니다. 

#### <a name="device"></a>디바이스

개체를 할당 및 제거 하 고, 기본 형식을 렌더링 하 고, 그래픽 드라이버를 통해 그래픽 카드와 통신 하는 방법으로 장치를 생각해 볼 수 있습니다. 

보다 정확한 설명을 위해 Direct3D 장치는 Direct3D의 렌더링 구성 요소입니다. 디바이스는 렌더링 상태를 캡슐화하여 저장하고, 변환 및 조명 작업을 수행하며, 이미지를 표면에 래스터화합니다. 자세한 내용은 [장치](../graphics-concepts/devices.md) 를 참조 하세요.

장치는 [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) 인터페이스로 표시 됩니다. 즉, **ID3D11Device** 인터페이스는 가상 표시 어댑터를 나타내며 장치에서 소유 하는 리소스를 만드는 데 사용 됩니다. 

ID3D11Device의 서로 다른 버전이 있습니다. [**ID3D11Device5**](/windows/desktop/api/d3d11_4/nn-d3d11_4-id3d11device5) 은 최신 버전 이며 **ID3D11Device4**의 해당 메서드에 새 메서드를 추가 합니다. Direct3D가 기본 하드웨어와 통신 하는 방법에 대 한 자세한 내용은 [WDDM (Windows 장치 드라이버 모델) 아키텍처](/windows-hardware/drivers/display/windows-vista-and-later-display-driver-model-architecture)를 참조 하세요.

각 응용 프로그램에는 하나 이상의 장치가 있어야 합니다. 대부분의 응용 프로그램은 하나만 만듭니다. **D3D11CreateDevice** 또는 **D3D11CreateDeviceAndSwapChain** 를 호출 하 고 **D3D_DRIVER_TYPE** 플래그를 사용 하 여 드라이버 유형을 지정 하 여 컴퓨터에 설치 된 하드웨어 드라이버 중 하나에 대 한 장치를 만듭니다. 각 장치는 원하는 기능에 따라 하나 이상의 장치 컨텍스트를 사용할 수 있습니다. 자세한 내용은 [D3D11CreateDevice 함수](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice)를 참조 하세요.

#### <a name="device-context"></a>장치 컨텍스트

장치 컨텍스트는 [파이프라인](#rendering-pipeline) 상태를 설정 하 고 [장치가](#device)소유한 [리소스](#resource) 를 사용 하 여 렌더링 명령을 생성 하는 데 사용 됩니다. 

Direct3D 11은 두 가지 유형의 장치 컨텍스트를 구현 합니다. 하나는 즉시 렌더링을 위한 것이 고 다른 하나는 지연 렌더링의 경우입니다. 두 컨텍스트는 모두 [ID3D11DeviceContext](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) 인터페이스로 표시 됩니다. 

**ID3D11DeviceContext** 인터페이스의 버전이 다릅니다. **ID3D11DeviceContext4** 는 **ID3D11DeviceContext3**의 해당 메서드에 새 메서드를 추가 합니다.

**ID3D11DeviceContext4** 는 Windows 10 크리에이터 스 업데이트에서 도입 되었으며 **ID3D11DeviceContext** 인터페이스의 최신 버전입니다. Windows 10 크리에이터 업데이트 이상을 대상으로 하는 응용 프로그램은 이전 버전 대신이 인터페이스를 사용 해야 합니다. 자세한 내용은 [ID3D11DeviceContext4](/windows/desktop/api/d3d11_3/nn-d3d11_3-id3d11devicecontext4)를 참조 하세요.

#### <a name="dxdeviceresources"></a>DX::D eviceResources

**DX::D eviceresources** 클래스는 **DeviceResources** / 파일에 있으며 모든 DirectX 장치 리소스를 제어**합니다.**

### <a name="buffer"></a>Buffer

버퍼 리소스는 요소로 그룹화 된 완전히 형식화 된 데이터의 컬렉션입니다. 버퍼를 사용 하 여 위치 벡터, 법선 벡터, 꼭 짓 점 버퍼의 질감 좌표, 인덱스 버퍼의 인덱스 또는 장치 상태를 비롯 한 다양 한 데이터를 저장할 수 있습니다. 버퍼 요소에는 압축 된 데이터 값 (예: **R8G8B8A8** surface 값), 단일 8 비트 정수 또는 4 32 비트 부동 소수점 값이 포함 될 수 있습니다.

사용할 수 있는 버퍼에는 꼭 짓 점 버퍼, 인덱스 버퍼 및 상수 버퍼의 세 가지 유형이 있습니다.

#### <a name="vertex-buffer"></a>꼭 짓 점 버퍼

기 하 도형을 정의 하는 데 사용 되는 꼭 짓 점 데이터를 포함 합니다. 꼭 짓 점 데이터에는 위치 좌표, 색 데이터, 질감 좌표 데이터, 일반 데이터 등이 포함 됩니다. 

#### <a name="index-buffer"></a>인덱스 버퍼

꼭 짓 점 버퍼에 정수 오프셋을 포함 하며 기본 형식을 보다 효율적으로 렌더링 하는 데 사용 됩니다. 인덱스 버퍼에는 16 비트 또는 32 비트 인덱스의 순차 집합이 포함 되어 있습니다. 각 인덱스는 꼭 짓 점 버퍼에서 꼭 짓 점을 식별 하는 데 사용 됩니다.

#### <a name="constant-buffer-or-shader-constant-buffer"></a>상수 버퍼 또는 셰이더 상수 버퍼

파이프라인에 셰이더 데이터를 효율적으로 제공할 수 있습니다. 상수 버퍼를 각 기본 형식에 대해 실행 되는 셰이더의 입력으로 사용 하 고 렌더링 파이프라인의 스트림 출력 단계 결과를 저장할 수 있습니다. 개념적으로 상수 버퍼는 단일 요소 꼭 짓 점 버퍼와 동일 하 게 보입니다.

#### <a name="design-and-implementation-of-buffers"></a>버퍼 디자인 및 구현

데이터 형식을 기반으로 버퍼를 디자인할 수 있습니다. 예를 들어 샘플 게임과 같이 정적 데이터에 대 한 버퍼 한 개, 프레임에서 일정 한 데이터 및 기본 형식에 해당 하는 데이터에 대해 다른 버퍼를 만듭니다.

모든 버퍼 형식은 **ID3D11Buffer** 인터페이스로 캡슐화 되며 **ID3D11Device:: createbuffer**를 호출 하 여 버퍼 리소스를 만들 수 있습니다. 그러나 버퍼는 파이프라인에 바인딩되어야 액세스할 수 있습니다. 버퍼는 여러 파이프라인 단계에 동시에 바인딩하여 읽을 수 있습니다. 쓰기 위해 버퍼를 단일 파이프라인 단계에 바인딩할 수도 있습니다. 그러나 동일한 버퍼를 읽고 쓰기 위해 동시에 바인딩할 수 없습니다.

이러한 방법으로 버퍼를 바인딩할 수 있습니다.

- **ID3D11DeviceContext** 메서드 (예: **ID3D11DeviceContext:: IASetVertexBuffers** 및 **ID3D11DeviceContext:: IASetIndexBuffer**)를 호출 하 여 입력 어셈블러 단계로 이동할 수 있습니다.
- **ID3D11DeviceContext:: SOSetTargets**를 호출 하 여 스트림 출력 단계로 이동할 수 있습니다.
- 셰이더 단계는 **ID3D11DeviceContext:: VSSetConstantBuffers**와 같은 셰이더 메서드를 호출 하 여 수행 합니다.

자세한 내용은 [Direct3D 11의 버퍼 소개](/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro)를 참조 하세요.

### <a name="dxgi"></a>DXGI

Microsoft DXGI (DirectX Graphics Infrastructure)는 Direct3D에 필요한 하위 수준 작업 중 일부를 캡슐화 하는 하위 시스템입니다. 다중 스레드 응용 프로그램에서 DXGI를 사용 하 여 교착 상태가 발생 하지 않도록 하려면 특히 주의를 기울여야 합니다. 자세한 내용은 [다중 스레딩 및 DXGI](/windows/win32/direct3darticles/dxgi-best-practices#multithreading-and-dxgi) 를 참조 하세요.

### <a name="feature-level"></a>기능 수준

기능 수준은 새로운 및 기존 컴퓨터의 다양 한 비디오 카드를 처리 하기 위해 Direct3D 11에 도입 된 개념입니다. 기능 수준은 잘 정의 된 GPU (그래픽 처리 장치) 기능 집합입니다. 

각 비디오 카드는 설치 된 Gpu에 따라 특정 수준의 DirectX 기능을 구현 합니다. 이전 버전의 Microsoft Direct3D에서는 비디오 카드가 구현 된 Direct3D 버전을 확인 한 다음 응용 프로그램을 적절 하 게 프로그래밍할 수 있습니다. 

기능 수준에서 장치를 만들 때 요청 하려는 기능 수준에 대 한 장치를 만들 수 있습니다. 디바이스 생성이 진행되면 기능 수준이 지원되는 것이고, 그렇지 않으면 하드웨어에서 기능 수준이 지원되지 않는 것입니다. 더 낮은 기능 수준에서 장치를 다시 만들려고 하거나 응용 프로그램을 종료 하도록 선택할 수 있습니다. 예를 들어, 12_0 기능 수준에는 Direct3D 11.3 또는 Direct3D 12와 셰이더 모델 5.1이 필요 합니다. 자세한 내용은 [Direct3D 기능 수준: 각 기능 수준에 대 한 개요](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro)를 참조 하세요.

기능 수준을 사용 하 여 Direct3D 9, Microsoft Direct3D 10 또는 Direct3D 11 용 응용 프로그램을 개발 하 고 9, 10 또는 11 하드웨어 (몇 가지 예외 포함)에서 실행할 수 있습니다. 자세한 내용은 [Direct3D 기능 수준](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro)을 참조 하세요.

### <a name="stereo-rendering"></a>스테레오 렌더링

스테레오 렌더링은 깊이의 효과를 향상 시키는 데 사용 됩니다. 왼쪽에 있는 이미지 두 개와 오른쪽에 있는 이미지를 사용 하 여 표시 화면에 장면을 표시 합니다. 

수학적으로이를 위해 일반 모노 프로젝션 행렬의 오른쪽 및 왼쪽에 있는 약간의 가로 오프셋 인 스테레오 프로젝션 행렬을 적용 합니다.

이 샘플 게임에서 스테레오 렌더링을 수행 하기 위해 두 개의 렌더링 패스가 수행 되었습니다.

- 오른쪽 렌더링 대상에 바인딩하고, 오른쪽 프로젝션을 적용 한 다음, 기본 개체를 그립니다.
- 왼쪽 렌더링 대상에 바인딩하고 왼쪽 프로젝션을 적용 한 다음 기본 개체를 그립니다.

### <a name="camera-and-coordinate-space"></a>카메라 및 좌표 공간

게임에는 전 세계의 좌표계에서 세계를 업데이트 하기 위한 코드가 있습니다 (전 세계 공간 또는 장면 공간 이라고 함). 카메라를 포함 하 여 모든 개체를 배치 하 고이 공간에 지향 합니다. 자세한 내용은 [좌표계](../graphics-concepts/coordinate-systems.md)를 참조 하세요.

꼭 짓 점 셰이더는 다음 알고리즘을 사용 하 여 모델 좌표에서 장치 좌표로 변환 하는 데 많은 부담을 합니다. 여기서 V는 벡터이 고 M은 행렬입니다.

`V(device) = V(model) x M(model-to-world) x M(world-to-view) x M(view-to-device)`

- `M(model-to-world)`는 전 세계 좌표에 대 한 모델 좌표에 대 한 변환 행렬입니다 .이 행렬은 [전 세계 변형 행렬](#world-transform-matrix)이 라고도 합니다. 이는 기본 형식에서 제공 됩니다.
- `M(world-to-view)`[뷰 변형 매트릭스가](#view-transform-matrix)라고도 하는 좌표를 보기 위한 세계 좌표에 대 한 변환 행렬입니다.
  - 이는 카메라의 뷰 매트릭스에서 제공 됩니다. 카메라의 위치에 따라 모양 벡터 (카메라에서 장면으로 직접 가리키는 벡터와 *위에* 표시 되는 *벡터 조회)* 를 사용 하 여 정의 됩니다.
  - 샘플 게임에서 **m_viewMatrix** 는 뷰 변환 매트릭스 이며 **Camera:: setviewparams**를 사용 하 여 계산 됩니다.
- `M(view-to-device)`는 [프로젝션 변환 매트릭스가](#projection-transform-matrix)라고도 하는 장치 좌표의 뷰 좌표에 대 한 변환 행렬입니다.
  - 이는 카메라의 프로젝션에서 제공 됩니다. 마지막 장면에 실제로 표시 되는 공간의 양에 대 한 정보를 제공 합니다. 뷰 (FoV), 가로 세로 비율 및 클리핑 평면의 필드는 투영 변환 매트릭스를 정의 합니다.
  - 샘플 게임에서 **m_projectionMatrix** 는 **카메라:: SetProjParams** 를 사용 하 여 계산 된 프로젝션 좌표에 대 한 변환을 정의 합니다. 스테레오 프로젝션의 경우 &mdash; 각 눈의 보기에 대해 두 개의 프로젝션 매트릭스를 사용 합니다.

의 셰이더 코드는 `VertexShader.hlsl` 상수 버퍼에서 이러한 벡터 및 매트릭스를 사용 하 여 로드 되 고 모든 꼭 짓 점에 대해이 변환을 수행 합니다.

### <a name="coordinate-transformation"></a>좌표 변환

Direct3D에서는 세 가지 변환을 사용 하 여 3D 모델 좌표를 픽셀 좌표 (화면 공간)로 변경 합니다. 이러한 변환은 세계 변형, 뷰 변환 및 프로젝션 변환입니다. 자세한 내용은 [변환 개요](../graphics-concepts/transform-overview.md)를 참조 하세요.

#### <a name="world-transform-matrix"></a>세계 변형 행렬

전 세계 변환은 모델 공간에서의 좌표를 변경 합니다. 여기서 꼭 짓 점은 장면의 모든 개체에 공통 된 원본에 상대적으로 정의 된 모델의 로컬 원점에 상대적으로 정의 됩니다. 기본적으로 세계 변환은 모델을 전 세계에 배치 합니다. 이름입니다. 자세한 내용은 [세계 transform](../graphics-concepts/world-transform.md)을 참조 하세요.

#### <a name="view-transform-matrix"></a>변환 행렬 보기

뷰 변환은 세계 공간에서 뷰어를 찾고, 꼭 짓 점을 카메라 공간으로 변환 합니다. 카메라 공간에서 카메라 또는 뷰어는 원점에 있으며 양의 z 방향을 찾습니다. 자세한 내용을 [보려면 변환 보기](../graphics-concepts/view-transform.md)로 이동 하세요.

####  <a name="projection-transform-matrix"></a>프로젝션 변환 매트릭스

투영 변환은 시야를 볼 수 있습니다. 보기와 같은 시야는 뷰포트의 카메라를 기준으로 하는 장면의 3D 볼륨입니다. 뷰포트는 3D 장면이 투영 되는 2D 사각형입니다. 자세한 내용은 [뷰포트 및 클리핑](../graphics-concepts/viewports-and-clipping.md) (영문)을 참조 하세요.

시야의 가까운 끝은 끝 보다 작으므로 카메라에 가까운 개체를 확장 하는 효과가 있습니다. 이는 원근감이 장면에 적용 되는 방식입니다. 따라서 플레이어와 더 가까운 개체가 더 크게 표시 됩니다. 그 밖의 개체는 더 작게 표시 됩니다.

수학적으로 프로젝션 변환은 일반적으로 규모와 큐브 뷰 투영 모두에 해당 하는 행렬입니다. 카메라의 렌즈 처럼 작동 합니다. 자세한 내용은 [프로젝션 변환](../graphics-concepts/projection-transform.md)을 참조 하세요.

### <a name="sampler-state"></a>샘플러 상태

샘플러 상태 질감 주소 지정 모드, 필터링 및 세부 수준을 사용 하 여 텍스처 데이터를 샘플링 하는 방법을 결정 합니다. 질감에서 질감 픽셀 (또는 텍셀)을 읽을 때마다 샘플링을 수행 합니다.

질감에는 텍셀의 배열이 포함 됩니다. 각 텍셀의 위치는로 표시 됩니다 `(u,v)` . 여기서 `u` 은 너비이 고 `v` 은 높이가 며 질감 너비와 높이에 따라 0과 1 사이에 매핑됩니다. 결과 질감 좌표는 질감을 샘플링할 때 텍셀를 해결 하는 데 사용 됩니다.

질감 좌표가 0 이상 1 미만이 면 질감 주소 모드에서 질감 좌표가 텍셀 위치를 처리 하는 방법을 정의 합니다. 예를 들어 **TextureAddressMode**를 사용 하는 경우 0-1 범위를 벗어난 모든 좌표는 고정의 최대값은 1이 고, 샘플링 전에는 최소값 0입니다.

질감이 너무 크거나 너무 작은 경우 다각형에 맞게 질감이 필터링 됩니다. 확대 필터는 질감을 확대 하 고 축소 필터는 작은 영역에 맞게 텍스처를 축소 합니다. 텍스처 확대는 blurrier 이미지를 생성 하는 하나 이상의 주소에 대해 샘플 텍셀을 반복 합니다. 질감 축소는 둘 이상의 텍셀 값을 단일 값으로 결합 해야 하기 때문에 더 복잡 합니다. 이 경우 텍스처 데이터에 따라 앨리어싱 또는 들쭉날쭉한 가장자리가 발생할 수 있습니다. 가장 많이 사용 되는 축소 방법은 밉 맵을 사용 하는 것입니다. 밉 맵은 다중 수준 질감입니다. 각 수준의 크기는 이전 수준에서 1x1 질감까지 2의 거듭제곱입니다. 축소를 사용 하는 경우 게임은 렌더링 시 필요한 크기에 가장 가까운 밉 맵 수준을 선택 합니다. 

### <a name="the-basicloader-class"></a>BasicLoader 클래스

**Basicloader** 는 디스크에 있는 파일에서 셰이더, 질감 및 메시를 로드 하기 위한 지원을 제공 하는 간단한 로더 클래스입니다. 동기 메서드와 비동기 메서드를 모두 제공 합니다. 이 샘플 게임에서 파일은 `BasicLoader.h/.cpp` **유틸리티** 폴더에 있습니다.

자세한 내용은 [기본 로더](complete-code-for-basicloader.md)를 참조 하세요.