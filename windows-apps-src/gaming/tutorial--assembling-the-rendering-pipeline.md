---
author: joannaleecy
title: 렌더링 소개
description: 그래픽을 표시할 렌더링 파이프라인을 조립하는 방법을 알아봅니다. 렌더링 소개
ms.assetid: 1da3670b-2067-576f-da50-5eba2f88b3e6
ms.author: joanlee
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 렌더링
ms.localizationpriority: medium
ms.openlocfilehash: 7e8df200e8e989015834608d38cb8dfb0d36917b
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7567585"
---
# <a name="rendering-framework-i-intro-to-rendering"></a>렌더링 프레임워크 I: 렌더링 소개

지금까지 UWP(유니버설 Windows 플랫폼) 게임을 구조화하는 방법과 게임의 흐름을 처리하도록 상태 시스템을 정의하는 방법에 대해 살펴보았습니다. 이제, 렌더링 프레임워크를 어셈블하는 방법에 대해 알아볼 차례입니다. 샘플 게임 Direct3D11 (일반적으로 DirectX 11 라고 함)를 사용 하 여 게임 장면을 렌더링 하는 방법에 대해 살펴보겠습니다.

>[!Note]
>이 샘플의 최신 게임 코드를 다운로드하지 않은 경우 [Direct3D 게임 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)로 이동합니다. 이 샘플은 UWP 기능 샘플의 큰 컬렉션의 일부입니다. 샘플을 다운로드하는 방법에 대한 지침은 [GitHub에서 UWP 샘플 가져오기](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)를 참조하세요.

Direct3D 11에는 게임과 같이 그래픽 집약적인 응용 프로그램을 위한 3D 그래픽을 생성하는 데 사용할 수 있는 고성능 그래픽 하드웨어의 고급 기능에 액세스할 수 있도록 해주는 API 집합이 포함되어 있습니다.

화면상에서 게임 그래픽을 렌더링하는 것은 기본적으로 화면 상에서 프레임 시퀀스를 렌더링하는 것입니다. 각 프레임에서 보기를 기반으로 장면에 표시되는 개체를 렌더링해야 합니다. 

프레임을 렌더링하려면 화면에 표시될 수 있도록 하드웨어에 필요한 장면 정보를 전달해야 합니다. 화면에 뭔가를 표시하고 싶다면 게임 실행이 시작되는 즉시 렌더링을 시작해야 합니다.

## <a name="objective"></a>목표

UWP DirectX 게임에 그래픽 출력을 표시하도록 기본 렌더링 프레임워크를 설정하기 위해 이들을 크게 세 가지 단계로 묶을 수 있습니다.

 1. 그래픽 인터페이스에 대한 연결 설정
 2. 그래픽을 그리기 위해 필요한 리소스 생성
 3. 프레임을 렌더링하여 그래픽 표시

이 문서에서는 그래픽 렌더링 과정을 1 ~ 3단계로 설명합니다.

[렌더링 프레임 워크 II: 게임 렌더링](tutorial-game-rendering.md)은 2단계에 해당하며, 렌더링 프레임워크를 설정하는 방법과 렌더링에 앞서 데이터를 준비하는 방법을 보여줍니다.

## <a name="get-started"></a>시작

시작에 앞서 기본적인 그래픽 및 렌더링 개념을 숙지해야 합니다. Direct3D 및 렌더링 개념을 처음 접하셨다면 [용어 및 개념](#terms-and-concepts)을 참조하여 이 문서에서 사용되는 그래픽 및 렌더링 용어에 대해 간략히 숙지하세요.

이 게임에서는 __GameRenderer__ 클래스 개체가 이 샘플 게임에 대한 렌더러를 나타냅니다.  렌더러는 게임 시각 효과를 생성하는 데 사용되는 모든 Direct3D 11 및 Direct2D 개체를 만들고 유지 관리합니다.  또한 렌더링할 개체 목록을 검색하는 데 사용되는 __Simple3DGame__ 개체를 비롯해 헤드업 디스플레이(HUD)에서의 게임 상태에 대한 참조를 유지합니다. 

자습서의 이 부분에서는 게임에서의 3D 개체 렌더링에 초점을 맞추겠습니다.

## <a name="establish-a-connection-to-the-graphics-interface"></a>그래픽 인터페이스에 대한 연결 설정

렌더링을 위해 하드웨어에 액세스하는 방법은 [__App::Initialize__](tutorial--building-the-games-uwp-app-framework.md#appinitialize-method) 아래의 UWP 프레임워크 문서를 참조하세요.

[아래](#appinitialize-method)에 나와 있듯이 __make\_shared function__은 디바이스에 액세스할 수 있도록 해주는 [__DX::DeviceResources__](#dxdeviceresources)에 대한  __shared\_ptr__을 생성하는 데 사용됩니다. 

Direct3D 11에서는 [디바이스](#device)가 개체의 할당 및 삭제, 원형 렌더링, 그래픽 드라이버를 통한 그래픽 카드와의 통신에 사용됩니다.

### <a name="appinitialize-method"></a>App::Initialize 메서드

```cpp
void App::Initialize(
    CoreApplicationView^ applicationView
    )
{
    //...

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

## <a name="display-the-graphics-by-rendering-the-frame"></a>프레임을 렌더링하여 그래픽 표시

게임이 시작되면 게임 장면이 렌더링되어야 합니다. 아래와 같이 렌더링을 위한 지침은 [__GameMain::Run__](#gameamainrun-method) 메서드에서 시작됩니다.

간단한 흐름은 다음과 같습니다.
1. __업데이트__
2. __렌더링__
3. __표시__

### <a name="gamemainrun-method"></a>GameMain::Run 메서드

```cpp
// Comparing this to App::Run() in the DirectX 11 App template
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible) // if the window is visible
        {
            // Added: Switching different game states since this game sample is more complex
            switch (m_updateState) 
            {

                // ...
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                // 1. Update
                // Also exist in DirectX 11 App template: Update loop to get the latest game changes
                Update();
                
                // 2. Render
                // Present also in DirectX 11 App template: Prepare to render
                m_renderer->Render();
                
                // 3. Present
                // Present also in DirectX 11 App template: Present the 
                // contents of the swap chain to the screen.
                m_deviceResources->Present(); 
                
                // Added: resetting a variable we've added to indicate that 
                // render has been done and no longer needed to render
                m_renderNeeded = false; 
            }
        }
        else
        {   
            // Present also in DirectX 11 App template
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending); 
        }
    }
    m_game->OnSuspending();  // exiting due to window close. Make sure to save state.
}
```

### <a name="update"></a>업데이트

[__App::Update__ and __GameMain::Update__](tutorial-game-flow-management.md#appupdate-method) 메서드에서 게임 상태를 업데이트하는 방법에 대한 자세한 내용은 [게임 흐름 관리](tutorial-game-flow-management.md) 문서를 참조하세요.

### <a name="render"></a>렌더링

__GameMain::Run__에서 [__GameRenderer::Render__](#gamerendererrender-method) 메서드를 호출하면 렌더링이 실행됩니다.

[스테레오 렌더링](#stereo-rendering)이 활성화되어 있으면 두 가지 렌더링 패스가 존재하는데, 하나는 오른쪽 눈을 위한 것이고 다른 하나는 왼쪽 눈을 위한 것입니다. 각 렌더링 패스에서 렌더링 대상 및 [깊이 - 스텐실 보기](#depth-stencil-view)를 해당 디바이스에 바인딩합니다. 깊이 - 스텐실 보기는 이후에 삭제가 됩니다.

> [!Note]
> 꼭지점 인스턴스화나 기하 도형 셰이더를 사용하는 단일 패스 스테레오 같은 다른 메서드를 사용해 스테레오 렌더링을 수행할 수 있습니다. 두 개의 렌더링 패스 메서드를 사용하면 느리지만 보다 편리하게 스테레오 렌더링을 수행할 수 있습니다.

게임이 존재하고 리소소가 로드되면 렌더링 패스당 한 번씩 [프로젝트 매트릭스](#projection-transform-matrix)를 업데이트합니다. 개체는 보기마다 약간씩 다릅니다. 그런 다음, [그래픽 렌더링 파이프라인](#rendering-pipeline)을 설정합니다. 

> [!Note]
> 리소스 로드 방법에 대한 자세한 내용은 [DirectX 그래픽 리소스 생성 및 로드](tutorial-game-rendering.md#create-and-load-directx-graphic-resources)를 참조하세요.

이 게임 샘플에서는 모든 개체에서 표준 꼭지점 레이아웃을 사용하도록 렌더러가 설계되어 있습니다. 따라서 셰이더 디자인을 간소화하고 개체의 기하 도형에 관계 없이 셰이더 간의 변경을 손쉽게 수행할 수 있습니다.

#### <a name="gamerendererrender-method"></a>GameRenderer::Render 메서드

입력 꼭지점 레이아웃을 사용하여 Direct3D 컨텍스트를 설정합니다. 입력 - 레이아웃 개체는 꼭지점 버퍼 데이터가 [렌더링 파이프라인](#rendering-pipeline)에 어떻게 스트리밍되는지 설명합니다. 

다음으로 앞서 정의한 [상수 버퍼](#constant-buffers)를 사용하여 Direct3D 컨텍스트를 설정합니다. 상수 버퍼는 [꼭지점 렌더링](#vertex-shaders-and-pixel-shaders) 파이프라인 단계와 [픽셀 셰이더](#vertex-shaders-and-pixel-shaders) 파이프라인 단계에서 사용됩니다. 

> [!Note]
> 상수 버퍼의 정의에 대한 자세한 내용은 [렌더링 프레임워크 II: 게임 렌더링](tutorial-game-rendering.md)을 참조하세요.

파이프라인에 존재하는 모든 셰이더에서 동일한 입력 레이아웃과 상수 버퍼 집합이 사용되므로 프레임마다 한 번씩 설정됩니다.

```cpp
void GameRenderer::Render()
{
    bool stereoEnabled = m_deviceResources->GetStereoState();

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();

    int renderingPasses = 1;
    if (stereoEnabled)
    {
        renderingPasses = 2;
    }

    for (int i = 0; i < renderingPasses; i++)
    {
        // Iterate through the number of rendering passes to be completed.
        // 2 rendering passes if stereo is enabled
        if (i > 0)
        {
            // Doing the Right Eye View.
            ID3D11RenderTargetView *const targets[1] = { m_deviceResources->GetBackBufferRenderTargetViewRight() };

            // Resets render targets to the screen.
            // OMSetRenderTargets binds 2 things to the device.
            // 1. Binds one render target atomically to the device.
            // 2. Binds the depth-stencil view, as returned by the GetDepthStencilView method, to the device.
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476464.aspx

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
            
            // d2d -- Discussed later
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmapRight());
        }
        else
        {
            // Doing the Mono or Left Eye View.
            // As compared to the right eye:
            // m_deviceResources->GetBackBufferRenderTargetView instead of GetBackBufferRenderTargetViewRight
            ID3D11RenderTargetView *const targets[1] = { m_deviceResources->GetBackBufferRenderTargetView() }; 
            
            // Same as the Right Eye View.
            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());
            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);
            
            // d2d -- Discussed later under Adding UI
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmap()); 
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
                            m_game->GameCamera()->LeftEyeProjection() :
                            m_game->GameCamera()->RightEyeProjection()
                            ),
                        XMMatrixTranspose(XMLoadFloat4x4(&orientation))
                        )
                    );

                d3dContext->UpdateSubresource(
                    m_constantBufferChangeOnResize.Get(),
                    0,
                    nullptr,
                    &changesOnResize,
                    0,
                    0
                    );
            }

            // Update variables that change once per frame.
            ConstantBufferChangesEveryFrame constantBufferChangesEveryFrame;
            XMStoreFloat4x4(
                &constantBufferChangesEveryFrame.view,
                XMMatrixTranspose(m_game->GameCamera()->View())
                );
            d3dContext->UpdateSubresource(
                m_constantBufferChangesEveryFrame.Get(),
                0,
                nullptr,
                &constantBufferChangesEveryFrame,
                0,
                0
                );

            // Setup the graphics pipeline. This sample uses the same InputLayout and set of
            // constant buffers for all shaders, so they only need to be set once per frame.
            // For more info about the graphics or rendering pipeline, 
            // go to https://msdn.microsoft.com/library/windows/desktop/ff476882.aspx

            // IASetInputLayout binds an input-layout object to the input-assembler (IA) stage. 
            // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
            // Set up the Direct3D context to use this vertex layout.
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476454.aspx
            d3dContext->IASetInputLayout(m_vertexLayout.Get());

            // VSSetConstantBuffers sets the constant buffers used by the vertex shader pipeline stage.
            // Set up the Direct3D context to use these constant buffers.
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476491.aspx

            d3dContext->VSSetConstantBuffers(0, 1, m_constantBufferNeverChanges.GetAddressOf());
            d3dContext->VSSetConstantBuffers(1, 1, m_constantBufferChangeOnResize.GetAddressOf());
            d3dContext->VSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            d3dContext->VSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());

            // Sets the constant buffers used by the pixel shader pipeline stage. 
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476470.aspx

            d3dContext->PSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            d3dContext->PSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());
            d3dContext->PSSetSamplers(0, 1, m_samplerLinear.GetAddressOf());

            auto objects = m_game->RenderObjects();
            for (auto object = objects.begin(); object != objects.end(); object++)
            {
                // The 3D object render method handles the rendering.
                // For more info, see Primitive rendering below.
                (*object)->Render(d3dContext, m_constantBufferChangesEveryPrim.Get()); /
            }
        }
        else
        {
            const float ClearColor[4] = {0.5f, 0.5f, 0.8f, 1.0f};

            // Only need to clear the background when not rendering the full 3D scene since
            // the 3D world is a fully enclosed box and the dynamics prevents the camera from
            // moving outside this space.
            if (i > 0)
            {
                // Doing the Right Eye View.
                d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetViewRight(), ClearColor);
            }
            else
            {
                // Doing the Mono or Left Eye View.
                d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), ClearColor);
            }
        }

        // Start of 2D rendering
        d3dContext->BeginEventInt(L"D2D BeginDraw", 1);
        d2dContext->BeginDraw();

        // ...
    }
}
```

### <a name="primitive-rendering"></a>원형 렌더링

장면을 렌더링할 때 렌더링이 필요한 모든 개체를 반복합니다. 각 개체(원형)에 대해 아래 단계가 반복됩니다.

* 모델의 [월드 변환 매트릭스](#world-transform-matrix)와 재료 정보로 상수 버퍼(__m\_constantBufferChangesEveryPrim__)를 업데이트합니다.
* __m\_constantBufferChangesEveryPrim__에는 각 개체에 대한 매개 변수가 포함되어 있습니다.  여기에는 월드 변환 매트릭스에 대한 개체를 비롯하여 조명 계산을 위한 색과 반사 지수 같은 재질 속성이 포함되어 있습니다.
* 메시 개체 데이터가 [렌더링 파이프라인](#rendering-pipeline)의 입력 어셈블러(IA) 단계에 스트리밍될 수 있도록 입력 꼭지점 레이아웃을 사용하도록 Direct3D 컨텍스트를 설정합니다.
* IA 단계에서 [인덱스 버퍼](#index-buffer)를 사용하도록 Direct3D 컨텍스트를 설정합니다. 원형 정보(유형, 데이터 순서 등)를 제공합니다.
* 인덱싱된 비 인스턴스 원형을 그리기 위해 그리기 호출을 제출합니다. __GameObject::Render__ 메서드는 원형의 [상수 버퍼](#constant-buffer-or-shader-constant-buffer)를 지정된 원형 고유의 데이터로 업데이트합니다. 그러면 각 원형의 기하 도형을 그릴 수 있도록 컨텍스트에서 __DrawIndexed__가 호출됩니다. 구체적으로 설명하자면 이러한 그리기 호출은 상수 버퍼 데이터에 의해 매개 변수화되어 그래픽 처리 장치(GPU)의 대기열에 명령 및 데이터를 저장합니다. 그리기 호출을 할 때마다 꼭지점마다 한 번씩 [꼭지점 셰이더](#vertex-shaders-and-pixel-shaders)가 실행되고, 원형에 있는 각 삼각형의 모든 픽셀마다 한 번씩 [픽셀 셰이더](#vertex-shaders-and-pixel-shaders)가 실행됩니다. 텍스처는 픽셀 셰이더가 렌더링을 수행하는 데 사용하는 상태의 일부입니다.

상수 버퍼가 여러 개인 이유는 다음과 같습니다.
    * 게임에서는 여러 상수 버퍼를 사용하지만 이러한 버퍼는 원형당 한 번만 업데이트해야 합니다. 앞서 설명했듯이 상수 버퍼는 각 원형에서 실행되는 셰이더에 대한 입력과 같습니다. 일부 데이터는 정적(__m\_constantBufferNeverChanges__)이고, 어떤 데이터는 카메라 위치와 같이 프레임에서 일정(__m\_constantBufferChangesEveryFrame__)하며, 또 어떤 데이터는 색상 및 텍스처와 같이 원형에 고유합니다(__m\_constantBufferChangesEveryPrim__).
    * 게임 [렌더러](#renderer)는 이러한 입력을 서로 다른 상수 버퍼로 분리하여 CPU 및 GPU가 사용하는 메모리 대역폭을 최적화합니다. 이러한 접근 방식은 GPU에서 추적해야 하는 데이터의 양을 최소화하는 데도 도움이 됩니다. GPU에는 대형 명령 대기열이 있어서 게임에서 __Draw__를 호출할 때마다 해당 명령이 관련 데이터와 함께 대기열에 저장됩니다. 게임에서 원형 상수 버퍼를 업데이트하고 다음 __Draw__ 명령을 실행하면 그래픽 드라이버에서 이 다음 명령과 관련 데이터를 대기열에 추가합니다. 게임에서 100개의 원형을 그리면 대기열에 상수 버퍼 데이터 복사본 100개가 생성될 수 있습니다. 게임에서 GPU로 보내는 데이터의 양을 최소화하려고 하므로 게임에서는 각 원형에 대한 업데이트만 포함하는 별도의 원형 상수 버퍼를 사용합니다.

#### <a name="gameobjectrender-method"></a>GameObject::Render 메서드

```cpp
void GameObject::Render(
    _In_ ID3D11DeviceContext *context,
    _In_ ID3D11Buffer *primitiveConstantBuffer
    )
{
    if (!m\_active || (m\_mesh == nullptr) || (m_normalMaterial == nullptr))
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

#### MeshObject::Render method

void MeshObject::Render(\_In\_ ID3D11DeviceContext *context)
{
    // PNTVertex is a struct. stride provides us the size required for all the mesh data
    // struct PNTVertex
    //{
    //  DirectX::XMFLOAT3 position;
    //  DirectX::XMFLOAT3 normal;
    //  DirectX::XMFLOAT2 textureCoordinate;
    //};
    uint32 stride = sizeof(PNTVertex);
    uint32 offset = 0;

    // Similar to the main render loop.
    // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
    context->IASetVertexBuffers(0, 1, m_vertexBuffer.GetAddressOf(), &stride, &offset);

    // IASetIndexBuffer binds an index buffer to the input-assembler stage.
    // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476453.aspx
    context->IASetIndexBuffer(m_indexBuffer.Get(), DXGI_FORMAT_R16_UINT, 0);

    // Binds information about the primitive type, and data order that describes input data for the input assembler stage.
    // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476455.aspx
    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

    // Draw indexed, non-instanced primitives. A draw API submits work to the rendering pipeline.
    // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476409.aspx
    context->DrawIndexed(m_indexCount, 0, 0);
}
```

### <a name="present"></a>표시

__DX::DeviceResources::Present__ 메서드를 호출하여 배치한 콘텐츠를 버퍼에 저장하고 이를 표시합니다.

스왑 체인이라는 용어는 사용자에게 프레임을 표시하는 데 사용되는 버퍼의 컬렉션에 사용됩니다. 응용 프로그램이 표시할 새 프레임을 제공할 때마다, 스왑 체인의 첫 버퍼가 표시되는 버퍼의 자리를 차지합니다. 이 프로세스를 스와핑 또는 플리핑이라고 부릅니다. 자세한 내용은 [스왑 체인](../graphics-concepts/swap-chains.md)을 참조하세요.

* __IDXGISwapChain1__ 인터페이스의 __Present__ 메서드는 [DXGI](#dxgi)에게 세로 동기화(VSync)가 될 때까지 차단하고 다음 VSync 때까지 응용 프로그램을 절전 모드로 전환하라고 명령합니다. 이렇게 하면 화면에 표시되지 않는 모든 주기 렌더링 프레임을 낭비하지 않게 됩니다.
* __ID3D11DeviceContext3__ 인터페이스의 __DiscardView__ 메서드는 [렌더링 대상](#render-target)의 콘텐츠를 삭제합니다. 이는 기존 콘텐츠가 완전히 덮어쓰기 될 때만 유효한 작업입니다. 더티 또는 스크롤 rect가 사용되는 경우에는 이 호출을 제거해야 합니다.
* 동일한 __DiscardView__ 메서드를 사용하여 [깊이 - 스텐실](#depth-stencil)의 콘텐츠를 삭제합니다.
* __HandleDeviceLost__ 메서드는 [디바이스](#device)가 제거된 경우에 시나리오를 관리하는 데 사용됩니다. 연결 해제 또는 드라이버 업그레이드를 통해 디바이스가 제거되면 모든 디바이스 리소를 다시 만들어야 합니다. 자세한 내용은 [Direct3D 11에서 디바이스가 제거된 시나리오 처리](handling-device-lost-scenarios.md)를 참조하세요.

> [!Tip]
> 원활한 프레임 속도를 위해서는 프레임 렌더링을 위한 작업의 양이 VSync 간의 시간에 맞는지 확인해야 합니다.

```cpp
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
    m_d3dContext->DiscardView(m_d3dRenderTargetView.Get());

    // Discard the contents of the depth-stencil.
    m_d3dContext->DiscardView(m_d3dDepthStencilView.Get());

    // If the device was removed either by a disconnection or a driver upgrade, we 
    // must recreate all device resources.
    // For more info about how to handle a device lost scenario, go to:
    // https://docs.microsoft.com/windows/uwp/gaming/handling-device-lost-scenarios
    if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
    {
        HandleDeviceLost();
    }
    else
    {
        DX::ThrowIfFailed(hr);
    }
}
```

## <a name="next-steps"></a>다음 단계

이 문서에는 디스플레이에서 그래픽이 렌더링되는 방법이 설명되어 있고, 사용되고 있는 일부 렌더링 용어를 간략하게 설명되어 있습니다. [렌더링 프레임워크 II: 게임 렌더링](tutorial-game-rendering.md) 문서에서 렌더링에 대한 자세한 내용을 확인하고 렌더링 전에 필요한 데이터를 준비하는 방법을 알아보세요.

## <a name="terms-and-concepts"></a>용어 및 개념

### <a name="simple-game-scene"></a>간단한 게임 장면

간단한 게임 장면은 광원이 여러 개인 몇 가지 개체로 이루어져 있습니다.

개체의 모양은 공간에서 X, Y, Z 좌표 집합으로 정의됩니다. 게임 세계에서의 실제 렌더링 위치는 X, Y, Z 위치 좌표에 변환 매트릭스를 적용하여 결정할 수 있습니다. 여기에는 U와 V 같이 개체에 재료가 적용된 방법을 지정하는 텍스처 좌표 집합도 포함될 수 있습니다. 이는 개체의 표면 속성을 정의하고 개체의 표면이 테니스 볼 같은 거친지, 아니면 볼링 볼 같이 매끄럽고 윤이 나는지 확인할 수 있도록 해줍니다.

장면 및 개체 정보는 디스플레이 모니터에 구현이 되도록 렌더링 프레임워크가 프레임별로 장면을 다시 생성하는 데 사용됩니다.

### <a name="rendering-pipeline"></a>렌더링 파이프라인

렌더링 파이프라인은 3D 장면 정보가 화면에 표시되는 이미지로 변환되는 프로세스입니다. Direct3D 11에서 이 파이프라인은 프로그래밍이 가능합니다. 렌더링 요구를 지원하도록 단계를 조정할 수 있습니다. 공통 셰이더 코어를 사용하는 단계는 HLSL 프로그래밍 언어를 사용하여 프로그래밍할 수 있습니다. 이를 그래픽 렌더링 파이프라인 또는 간단하게 파이프라인이라고 합니다.

이러한 파이프라인을 손쉽게 만들기 위해서는 다음을 숙지해야 합니다.
* [HLSL](#HLSL). UWP DirectX 게임에서 HLSL 셰이더 모델 5.1 이상을 사용하는 것이 좋습니다.
* [셰이더](#Shaders)
* [꼭짓점 셰이더 및 픽셀 셰이더](#vertext-shaders-pixel-shaders)
* [셰이더 단계](#shader-stages)
* [다양한 셰이더 파일 형식](#various-shader-file-formats)

자세한 내용은 [Direct3D 11 렌더링 파이프라인 이해](https://msdn.microsoft.com/library/windows/desktop/dn643746.aspx) 및 [그래픽 파이프라인](https://msdn.microsoft.com/library/windows/desktop/ff476882.aspx)을 참조하세요.

#### <a name="hlsl"></a>HLSL

HLSL은 DirectX를 위한 상위 수준 셰이딩 언어입니다. HLSL를 사용하여 Direct3D 파이프라인을 위해 C 스타일의 프로그래밍 셰이더를 만들 수 있습니다. 자세한 내용은 [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561.aspx)를 참조하세요.

#### <a name="shaders"></a>셰이더

셰이더는 렌더링 시 개체의 표면이 어떻게 나타나는지를 판단하기 위한 지침의 집합으로 생각하면 됩니다. HLSL를 사용하여 프로그래밍되는 개체를 HLSL 셰이더라고 합니다. [HLSL])(#hlsl) 셰이더용 소스 코드 파일의 확장명은 .hlsl입니다. 이러한 셰이더는 작성 시점이나 실행 시에 컴파일이 되고, 실행 시에 해당되는 파이프라인 단계로 들어갑니다. 컴파일된 셰이더 개체의 파일 확장명은 .cso입니다.

Direct3D 9 셰이더는 셰이더 모델 1, 셰이더 모델 2 및 셰이더 모델 3을 사용하여 설계가 가능하지만, Direct3D 10 셰이더는 셰이더 모델 4를 토대로 해서만 설계가 가능합니다. Direct3D 11 셰이더는 셰이더 모델 5를 토대로 설계할 수 있습니다. Direct3D 11.3 및 Direct3D 12는 셰이더 모델 5.1을 토대로, Direct3D 12는 셰이더 모델 6를 토대로 설계가 가능합니다.

#### <a name="vertex-shaders-and-pixel-shaders"></a>꼭짓점 셰이더 및 픽셀 셰이더

데이터는 원형 스트림 형태로 그래픽 파이프라인으로 들어가고 꼭지점 셰이더나 픽셀 셰이더 같이 다양한 셰이더에 의해 처리됩니다. 

꼭지점 셰이더는 보통 변환, 스킨 지정, 조명 등의 작업을 수행하여 꼭지점을 처리합니다.  픽셀 셰이더는 픽셀별 조명과 후처리 같은 풍부한 음영 기술을 사용할 수 있도록 해줍니다. 또한 상수 변수, 텍스처 데이터, 보간된 꼭지점별 값 및 기타 데이터를 조합하여 픽셀별 출력을 산출합니다. 

#### <a name="shader-stages"></a>셰이더 단계

이러한 원형 스트림을 처리하도록 정의된 다양한 셰이더 시퀀스를 렌더링 파이프라인에서는 셰이더 단계라고 부릅니다. 실제 단계는 Direct3D의 버전에 따라 다르지만, 일반적으로 꼭지점, 기하 도형 및 픽셀 단계가 포함되어 있습니다. 공간 분할을 위한 헐(hull) 및 도메인 셰이더나 컴퓨팅 셰이더 등 다른 단계들도 있습니다. 이러한 모든 단계는 [HLSL])(#hlsl)을 사용해 완전히 프로그래밍이 가능합니다. 자세한 내용은 [그래픽 파이프라인](https://msdn.microsoft.com/library/windows/desktop/ff476882.aspx)을 참조하세요.

#### <a name="various-shader-file-formats"></a>다양한 셰이더 파일 형식

셰이더 코드 파일 확장면은 다음과 같습니다.
    * 확장명이 .hlsl인 파일은 [HLSL])(#hlsl) 소스 코드를 보유합니다.
    * 확장명이 .cso인 파일은 컴파일된 셰이더 개체를 보유합니다.
    * 확장명이 .h인 파일은 헤더 파일이지만, 셰이더 코드 맥락에서 보자면 이 헤더 파일은 셰이더 데이터를 보유하는 바이트 배열을 정의합니다.
    * 확장명이 .hlsli인 파일에는 상수 버퍼의 형식이 포함되어 있습니다. 게임 샘플에서 파일은 __Shaders__>__ConstantBuffers.hlsli__입니다.

> [!Note]
> 실행 시 .cso 파일을 로드하거나 실행 코드에 .h 파일을 추가하는 방법으로 셰이더를 기본 포함시킬 수 있습니다. 하지만 동일한 셰이더에 대해 둘을 모두 사용하지 않습니다.

### <a name="deeper-understanding-of-directx"></a>DirectX에 대한 깊은 이해

Direct3D 11은 게임 같이 그래픽 집약적이어서 집약적 연산을 처리할 수 있는 뛰어난 그래픽 카드가 요구되는 응용 프로그램에서 그래픽을 생성하도록 도와주는 API 집합입니다. 이 섹션에는 리소스, 하위 리소스, 디바이스, 디바이스 컨텍스트와 같은 Direct3D 11 그래픽 프로그램 개념에 대해 간략하게 설명되어 있습니다.

#### <a name="resource"></a>리소스

이 개념을 처음 접하는 사람들은 리소스(디바이스 리소스라고도 함)를 텍스처, 위치, 색상 같은 개체를 렌더링하는 방법에 대한 정보로 생각할 수 있습니다. 리소스는 파이프라인에 정보를 제공하고 장면에서 렌더링되는 내용을 정의합니다. 리소스는 게임 미디어에서 로드하거나 실행 시 동적으로 생성할 수 있습니다.

실제로 리소스는 Direct3D [파이프라인](#rendering-pipeline)에서 액세스할 수 있는 메모리의 영역을 뜻합니다. 파이프라인에서 메모리에 효율적으로 액세스하려면 파이프라인에 제공된 데이터(입력 기하 도형, 셰이더 리소스, 텍스처 등)를 리소스에 저장해야 합니다. 모든 Direct3D 리소스가 파생되는 리소스 종류는 버퍼와 텍스처, 두 가지입니다. 각 파이프라인 단계에 최대 128개 리소스를 활성화할 수 있습니다. 자세한 내용은 [리소스](../graphics-concepts/resources.md)를 참조하세요.

#### <a name="subresource"></a>하위 리소스

하위 리소스란 리소스의 하위 집합을 나타내는 용어입니다. Direct3D는 전체 리소스를 참조할 수도 있고 리소스의 하위 집합을 참조할 수도 있습니다. 자세한 내용은 [하위 리소스](../graphics-concepts/resource-types.md#subresources)를 참조하세요.

#### <a name="depth-stencil"></a>깊이 - 스텐실

깊이 - 스텐실 리소스에는 깊이와 스텐실 정보를 보유하는 형식 및 버퍼가 포함되어 있습니다. 이 리소스는 텍스터 리소스를 사용해 만들 수 있습니다. 깊이 - 스텐실 리소를 만드는 방법에 대한 자세한 내용은 [깊이 - 스텐실 기능 구성](https://msdn.microsoft.com/library/windows/desktop/bb205074.aspx)을 참조하세요. [ID3D11DepthStencilView](https://msdn.microsoft.com/library/windows/desktop/ff476377.aspx) 인터페이스를 사용해 구현된 깊이 - 스텐실 보기를 통해 깊이 - 스텐실 리소스에 액세스합니다.

깊이 정보는 다각형의 어떤 영역이 보기에서 숨겨지지 않고 렌더링되는지를 알려줍니다. 스텐실 정보는 어떤 픽셀이 마스킹되는지 알려줍니다. 이 정보는 픽셀이 그려져 있는지 여부를 판단하여 1 또는 0으로 설정하기 때문에 특수 효과를 내는 데 사용할 수 있습니다. 

자세한 내용은 [깊이 - 스텐실 보기](../graphics-concepts/depth-stencil-view--dsv-.md), [깊이 버퍼](../graphics-concepts/depth-buffers.md) 및 [스텐실 버퍼](../graphics-concepts/stencil-buffers.md)를 참조하세요.

#### <a name="render-target"></a>렌더링 대상

렌더링 대상은 렌더링 패스가 끝날 때 기록이 가능한 리소스입니다. 보통은 [ID3D11Device::CreateRenderTargetView](https://msdn.microsoft.com/library/windows/desktop/ff476517.aspx) 메서드에서 스왑 체인 백 버퍼(이 역시 리소스)를 입력 파라미터로 사용하여 생성됩니다. 

각 렌더링 대상은 해당되는 깊이 - 스텐실 보기를 가지고 있어야 합니다. 왜냐하면 사용에 앞서 [OMSetRenderTargets](https://msdn.microsoft.com/library/windows/desktop/ff476464.aspx)를 통해 렌더링 대상을 설정할 때 깊이 - 스텐실 보기가 필요할 수도 있기 때문입니다. [ID3D11RenderTargetView](https://msdn.microsoft.com/library/windows/desktop/ff476582.aspx) 인터페이스를 사용해 구현되는 렌더링 대상 보기를 통해 렌더링 대상 리소스에 액세스합니다. 

#### <a name="device"></a>디바이스

Direct3D 11이 처음인 사용자는 개체를 할당 및 삭제하고 원형을 렌더링하며 그래픽 드라이버를 통해 그래픽 카드와 통신하는 방법으로 디바이스를 생각할 수 있습니다. 

보다 정확하게 설명하자면 Direct3D 디바이스는 Direct3D의 렌더링 구성 요소입니다. 디바이스는 렌더링 상태를 캡슐화하여 저장하고, 변환 및 조명 작업을 수행하며, 이미지를 표면에 래스터화합니다. 자세한 내용은 [디바이스](../graphics-concepts/devices.md)를 참조하세요.

디바이스는 [ID3D11Device](https://msdn.microsoft.com/library/windows/desktop/ff476379.aspx) 인터페이스를 통해 표시됩니다. 즉, ID3D11Device 인터페이스는 가상 디스플레이 어댑터를 나타내고 디바이스가 소유하는 리소스를 생성하는 데 사용됩니다. 

다양한 버전의 ID3D11Device가 있으며, [ID3D11Device5](https://msdn.microsoft.com/library/windows/desktop/mt492478.aspx)는 ID3D11Device4에 새 메서드를 추가한 최신 버전입니다. Direct3D가 기반 하드웨어와 어떻게 통신하는지에 대한 자세한 내용은 [Windows 장치 드라이버 모델(WDDM) 아키텍처](https://docs.microsoft.com/windows-hardware/drivers/display/windows-vista-and-later-display-driver-model-architecture)를 참조하세요.

각 응용 프로그램에는 최소 하나의 디바이스가 있어야 하고, 대부분의 응용 프로그램은 오직 하나의 디바이스만 생성합니다. __D3D11CreateDevice__ 또는 __D3D11CreateDeviceAndSwapChain__을 호출하고 D3D\_DRIVER\_TYPE 플래그를 통해 드라이버 유형을 지정하여 시스템에 설치되는 하드웨어 드라이버 중 하나에 대한 디바이스를 생성합니다. 각 디바이스는 원하는 기능에 따라 디바이스 컨텍스트를 하나 이상 사용할 수 있습니다. 자세한 내용은 [D3D11CreateDevice 함수](https://msdn.microsoft.com/library/windows/desktop/ff476082.aspx)를 참조하세요.

#### <a name="device-context"></a>디바이스 컨텍스트

디바이스 컨텍스트는 [파이프라인](#rendering-pipeline) 상태를 설정하고 [디바이스](#device)가 소유하는 [리소스](#resource)를 사용해 렌더링 명령을 생성합니다. 

Direct3D 11은 두 가지 유형의 디바이스 컨텍스트를 구현하는데, 하나는 즉각적인 렌더링을 위한 것이고 다른 하나는 지연된 렌더링을 위한 것입니다. 두 컨텍스트 모두 [ID3D11DeviceContext](https://msdn.microsoft.com/library/windows/desktop/ff476385.aspx) 인터페이스를 통해 표시됩니다.  

__ID3D11DeviceContext__ 인터페이스는 다양한 버전이 있으며, __ID3D11DeviceContext4__는 __ID3D11DeviceContext3__에 새 메서드를 추가한 버전입니다.

참고: Windows 10 크리에이터스 업데이트에 도입된 __ID3D11DeviceContext4__는 __ID3D11DeviceContext__ 인터페이스의 최신 버전입니다. Windows 10 크리에이터스 업데이트를 대상으로 하는 응용 프로그램은 이전 버전이 아니라 이 버전의 인터페이스를 사용해야 합니다. 자세한 내용은 [ID3D11DeviceContext4](https://msdn.microsoft.com/library/windows/desktop/mt492481.aspx)를 참조하세요.

#### <a name="dxdeviceresources"></a>DX::DeviceResources

__DX::DeviceResources__ 클래스는 __DeviceResources.cpp__/__.h__ 파일에 존재하며 모든 DirectX 디바이스 리소스를 제어합니다. 샘플 게임 프로젝트와 DirectX 11 앱 템플릿 프로젝트에서는 이러한 파일들이 __Commons__ 폴더에 있습니다. Visual Studio 2015 이상에서 DirectX 11 앱 템플릿 프로젝트를 새로 만들 때 이러한 파일의 최신 버전을 얻을 수 있습니다.

### <a name="buffer"></a>버퍼

버퍼 리소스는 완벽하게 입력된 데이터를 수집한 것이며 요소로 그룹화됩니다. 버퍼를 사용하면 위치 벡터, 법선 벡터, 꼭지점 버퍼의 텍스처 좌표, 인덱스 버퍼의 인덱스, 디바이스 상태 같이 다양한 데이터를 저장할 수 있습니다. 버퍼 요소에는 압축된 데이터 값(R8G8B8A8 표면 값), 8비트 정수 하나 또는 32비트 부동 소수점 값 네 개가 포함될 수 있습니다.

버퍼의 유형은 꼭지점 버퍼, 인덱스 버퍼, 상수 버퍼 등 세 가지입니다.

#### <a name="vertex-buffer"></a>꼭지점 버퍼

기하 도형을 정의하는 데 사용되는 꼭지점 데이터가 포함되어 있습니다. 꼭지점 데이터에는 위치 좌표, 색상 데이터, 텍스처 좌표 데이터, 법선 데이터 등이 포함되어 있습니다. 

#### <a name="index-buffer"></a>인덱스 버퍼

꼭지점 버퍼에 대한 정수 오프셋을 포함하며, 원형을 효율적으로 렌더링하는 데 사용됩니다. 인덱스 버퍼는 16비트 또는 32비트 인덱스의 순차 집합을 포함하며, 각 인덱스는 꼭짓점 버퍼에서 꼭짓점을 식별하는 데 사용됩니다.

#### <a name="constant-buffer-or-shader-constant-buffer"></a>상수 버퍼 또는 셰이더 - 상수 버퍼

파이프라인에 셰이더 데이터를 효율적으로 제공할 수 있도록 해줍니다. 상수 버퍼를 각각의 원형에서 실행되고 렌더링 파이프라인의 스트림 - 출력 단계의 결과를 저장하는 셰이더에 대한 입력으로 사용할 수 있습니다. 개념적으로 상수 버퍼는 단일 요소 꼭지점 버퍼와 상당히 비슷합니다.

#### <a name="design-and-implementation-of-buffers"></a>버퍼 디자인 및 구현

데이터 형식에 따라 버퍼를 설계할 수 있습니다. 예를 들면, 게임 샘플에서 정적 데이터를 위한 버퍼와 프레임의 일정한 데이터를 위한 버퍼, 원형과 관련된 데이터를 위한 버퍼를 따로 설계할 수 있습니다.

__ID3D11Buffer__ 인터페이스에서 모든 버퍼 형식이 캡슐화되므로 __ID3D11Device::CreateBuffer__를 호출하여 버퍼 리소스를 생성할 수 있습니다. 하지만 파이프라인에 버퍼를 먼저 바운딩해야 액세스가 가능합니다. 읽기를 위해 버퍼를 동시에 여러 개의 파이프라인 단계에 바인딩할 수 있습니다. 쓰기의 경우 단일 파이프라인 단계에 버퍼를 바인딩할 수도 있지만, 동일한 버퍼를 읽기 및 쓰기를 위해 동시에 바인딩할 수 없습니다.

버퍼를 다음과 같이 바인딩합니다.
    * __ID3D11DeviceContext::IASetVertexBuffers__ 및 __ID3D11DeviceContext::IASetIndexBuffer__ 같은 __ID3D11DeviceContext__ 메서드를 호출하여 입력 - 어셈블러 단계에 바인딩
    * __ID3D11DeviceContext::SOSetTargets__를 호출하여 스트림 - 출력 단계에 바인딩
    * __ID3D11DeviceContext::VSSetConstantBuffers__ 같은 셰이더 메서드를 호출하여 셰이더 단계에 바인딩

자세한 내용은 [Direct3D 11의 버퍼 소개](https://msdn.microsoft.com/library/windows/desktop/ff476898.aspx)를 참조하세요.

### <a name="dxgi"></a>DXGI

Microsoft DirectX 그래픽 Infrastructure (DXGI)는 Direct3D 10 되는 하위 수준 작업 중 일부를 캡슐화 WindowsVista에 도입 된 새로운 하위 시스템 10.1, 11 및 11.1 합니다. 교착 상태가 발생하지 않도록 하려면 다중 스레딩된 응용 프로그램에서 DXGI를 사용할 때 특히 주의를 기울여야 합니다. 자세한 내용은 [DirectX Graphics Infrastructure(DXGI): 모범 사례 - 다중 스레딩](https://msdn.microsoft.com/library/windows/desktop/ee417025.aspx#multithreading_and_dxgi)을 참조하세요.

### <a name="feature-level"></a>기능 수준

기능 수준은 도입 신규 및 기존 컴퓨터에서 다양한 비디오 카드를 처리하기 위해 Direct3D 11에 도입된 개념입니다. 기능 수준은 체계적으로 정의된 그래픽 처리 장치(GPU) 기능 집합입니다. 

각 비디오 카드는 설치된 GPU에 따라 특정 수준의 DirectX 기능을 구현합니다. Microsoft Direct3D의 이전 버전에서는 비디오 카드에 구현된 Direct3D 버전을 찾아서 그에 따라 응용 프로그램을 프로그래밍할 수 있었습니다. 

기능 수준을 사용하면 디바이스를 만들 때 요청하고자 하는 기능 수준에 맞게 디바이스를 생성하려고 시도할 있습니다. 디바이스 생성이 진행되면 기능 수준이 지원되는 것이고, 그렇지 않으면 하드웨어에서 기능 수준이 지원되지 않는 것입니다. 더 낮은 기능 수준에서 디바이스를 다시 생성해 보거나 응용 프로그램을 종료하는 방법을 선택할 수 있습니다. 예를 들어 12\_0 기능 수준에는 Direct3D 11.3 또는 Direct3D 12와 셰이더 모델 l 5.1이 필요합니다. 자세한 내용은 [Direct3D 기능 수준: 각 기능 수준에 대한 개요](https://msdn.microsoft.com/library/windows/desktop/ff476876.aspx#Overview)를 참조하세요.

기능 수준을 사용 하면 Direct3D9, Microsoft Direct3D10 또는 Direct3D11, 응용 프로그램을 개발 하 고 9, 10 또는 11 하드웨어 (일부 예외)에서 실행할 수 있습니다. 자세한 내용은 [Direct3D 기능 수준](https://msdn.microsoft.com/library/windows/desktop/ff476876.aspx)을 참조하세요.

### <a name="stereo-rendering"></a>스테레오 렌더링

스테레오 렌더링은 깊이 효과를 높이는 데 사용됩니다. 디스플레이 화면에 장면을 표시하기 위해 두 개의 이미지가 사용되는데, 하나는 왼쪽 눈에서 나온 것이고 다른 하나는 오른쪽 눈에서 나온 것입니다. 

수학적으로 보면 깊이 효과를 위해 일반적인 모노 투영 매트릭스에 대해 왼쪽과 오른쪽에 대해 약간의 가로 오프셋이 있는 스테레오 투영 매트릭스를 적용하는 것입니다.

이 게임 샘플에서는 스테리오 렌더링을 실현하기 위해 두 개의 렌더링 패스를 사용했습니다.
* 오른쪽 렌더링 대상에 바인딩해서 오른쪽 투영을 적용한 다음, 원형 개체를 그립니다.
* 왼쪽 렌더링 대상에 바인딩해서 왼쪽 투영을 적용한 다음, 원형 개체를 그립니다.

### <a name="camera-and-coordinate-space"></a>카메라 및 좌표 공간

게임에는 고유한 좌표계로 월드(경우에 따라 월드 공간 또는 장면 공간이라고 함)를 업데이트하는 코드가 있습니다. 카메라를 포함한 모든 개체를 이 공간에 배치하고 방향을 지정합니다. 자세한 내용은 [좌표계](../graphics-concepts/coordinate-systems.md)를 참조하세요.

꼭지점 셰이더는 다음 알고리즘(여기에서 V는 벡터, M은 매트릭스)을 통해 모델 좌표를 디바이스 좌표로 대대적으로 변환합니다.

V(디바이스) = V(모델) x M(모델 - 월드) x M(월드 - 보기) x M(보기 - 디바이스)

각 항목은 다음을 의미합니다. 
* M(모델 - 월드)은 모델 좌표를 월드 좌표로 변환하는 매트릭스로, [월드 변환 매트릭스](#world-transform-matrix)라고도 합니다. 이 매트릭스는 원형에서 제공합니다.
* M(월드 - 보기)는 월드 좌표를 보기 좌표로 변환하는 매트릭스로, [보기 변환 매트릭스](#view-transform-matrix)라고도 합니다.
    * 이 매트릭스는 카메라의 보기 매트릭스에서 제공합니다. 보기 벡터(카메라에서 장면을 직접 가리키는 "정면 보기" 벡터 및 카메라와 수직으로 위쪽을 가리키는 "위 보기" 벡터)와 함께 카메라의 위치에 의해 정의됩니다.
    * 샘플 게임에서 __m\_viewMatrix__는 보기 변환 매트릭스로, __Camera::SetViewParams__를 사용해 계산됩니다. 
* M(보기 - 디바이스)은 보기 좌표를 디바이스 좌표로 변환하는 매트릭스로 [투영 변환 매트릭스](#projection-transform-matrix)라고도 합니다.
    * 이 매트릭스는 카메라의 투영에서 제공합니다. 해당 공간에서 어느 정도가 최종 장면에 실제로 표시될 것인지에 대한 정보를 제공합니다. 시야각(FoV), 가로 세로 비율 및 클리핑 평면은 투영 변환 매트릭스를 정의합니다.
    * 샘플 게임에서 __m\_projectionMatrix__는 __Camera::SetProjParams__를 사용해 계산된 투영 좌표에 대한 변환을 정의합니다(스테레오 투영의 경우, 각 눈에 하나씩 두 개의 투영 매트릭스를 사용). 

VertexShader.hlsl의 셰이더 코드는 이러한 벡터 및 매트릭스와 함께 상수 버퍼에서 로드되며, 모든 꼭지점에 대해 이 변환을 수행합니다.

### <a name="coordinate-transformation"></a>좌표 변환

Direct3D는 3D 모델 좌표를 픽셀 좌표(화면 공간)를 변경하기 위해 세 가지 변환을 사용하고 있습니다. 월드 변환, 보기 변환, 투영 변환이 바로 그것입니다. 자세한 내용은 [변환 개요](../graphics-concepts/transform-overview.md)를 참조하세요.

#### <a name="world-transform-matrix"></a>월드 변환 매트릭스

월드 변환은 모델 공간의 좌표(모델의 로컬 원점을 기준으로 꼭지점 정의)를 월드 공간의 좌표(장면의 모든 개체에 공통적인 원점을 기준으로 꼭지점 정의)로 변경합니다. 이름에서 알 수 있듯이 기본적으로 월드 변환은 모델을 월드에 배치합니다. 자세한 내용은 [월드 변환](../graphics-concepts/world-transform.md)을 참조하세요.

####  <a name="view-transform-matrix"></a>보기 변환 매트릭스

보기 변환은 월드 공간에 뷰어를 배치하여 꼭지점을 카메라 공간으로 변환합니다. 카메라 공간에서 카메라 또는 뷰어는 원점에 있으며 양의 Z축 방향을 바라봅니다. 자세한 내용은 [보기 변환](../graphics-concepts/view-transform.md)을 참조하세요.

####  <a name="projection-transform-matrix"></a>투영 변환 매트릭스

투영 변환은 절두체 보기를 직육면체 모양으로 변환합니다. 절두체 보기는 뷰포트의 카메라를 기준으로 배치되는 장면의 3D 볼륨입니다. 뷰포트는 3D 장면이 투영되는 2D 사각형입니다. 자세한 내용은 [뷰포트 및 클리핑](../graphics-concepts/viewports-and-clipping.md)을 참조하세요.

절두체 보기의 가까운 쪽 끝이 먼 쪽 끝보다 작기 때문에 카메라에 가까운 개체를 확대하는 효과가 있습니다. 이것이 장면에 원근이 적용되는 방법입니다. 따라서 플레이어에 보다 가까운 개체는 더 크게, 먼 개체는 더 작게 표시됩니다.

수학적으로 보면 투영 변환은 배율 및 원근감을 모두 투영하는 매트릭스입니다. 이는 마치 카메라 렌즈처럼 작동합니다. 자세한 내용은 [투영 변환](../graphics-concepts/projection-transform.md)을 참조하세요.

### <a name="sampler-state"></a>샘플러 상태

샘플러 상태는 텍스처 주소 지정 모드, 필터링 및 상세 수준을 사용하여 텍스처 데이터를 샘플링하는 방법을 결정합니다. 텍스처에서 텍스처 픽셀(텍셀)을 읽을 때마다 샘플링이 수행됩니다.

하나의 텍스처에는 여러 텍셀이 포함되어 있습니다. 각 텍셀의 위치는 (u,v)로 표시되는데, 여기에서 u는 너비를, v는 높이를 나타냅니다. 텍스처 너비 및 높이에 따라 0과 1 사이의 값으로 매핑됩니다. 그 결과로 나온 텍스처 좌표는 텍스처를 샘플링할 때 텍셀의 주소를 지정하는 데 사용됩니다.

텍스처 좌표가 0과 1 사이에 있을 때 텍스처 주소 모드는 텍스처 좌표가 텍셀 위치의 주소를 지정하는 방법을 정의합니다. 예를 들어 __TextureAddressMode.Clamp__를 사용할 때는 0 ~ 1의 범위를 벗어난 좌표는 샘플링에 앞서 최대 값 1, 최소 값 0으로 고정됩니다.

텍스처가 다각형에서 너무 크거나 작으면 공간에 맞게 텍스처가 필터링됩니다. 확대 필터는 텍스처를 확장하고 축소 필터는 작은 영역에 맞게 텍스처를 축소합니다. 텍스처 확대는 하나 이상의 주소에 대해 샘플 텍셀을 반복하기 때문에 이미지가 흐릿해질 수 있습니다. 텍스처 축소는 하나 이상의 텍셀 값을 단일 값으로 통합해야 한다는 점에서 좀더 복잡합니다. 이로 인해 텍스처 데이터에 따라 가장 자리가 들쭉날쭉 해지는 등 앨리어싱 현상이 발생할 수 있습니다. 가장 널리 사용되는 축소 접근 방식은 Mipmap을 사용하는 것입니다. Mipmap은 다단계 텍스처입니다. 각 단계의 크기는 이전 단계보다 2배 작습니다(최소 1x1 텍스처). 게임에서 축소 기능이 사용되면 렌더링 시 필요한 크기와 가장 가까운 Mipmap 수준이 선택됩니다. 

### <a name="basicloader"></a>BasicLoader

__BasicLoader__는 디스크 상의 파일에서 셰이더, 텍스처 및 메시를 로드할 수 있도록 지원하는 간단한 로더 클래스입니다. 동기 및 비동기 방법을 모두 제공합니다. 이 게임 샘플에서는 BasicLoader.h/.cpp 파일이 __Commons__ 폴더에 있습니다.

자세한 내용은 [기본 로더](complete-code-for-basicloader.md)를 참조하세요.