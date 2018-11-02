---
author: joannaleecy
title: 설정
description: 그래픽을 표시할 렌더링 파이프라인을 조립하는 방법을 알아봅니다. 게임 렌더링, 설정 및 데이터 준비.
ms.assetid: 7720ac98-9662-4cf3-89c5-7ff81896364a
ms.author: joanlee
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 렌더링
ms.localizationpriority: medium
ms.openlocfilehash: 134c9005a796f52fb61ba628c0a85c8dbd875442
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5938966"
---
# <a name="rendering-framework-ii-game-rendering"></a>렌더링 프레임워크 II: 게임 렌더링

[렌더링 프레임워크 I](tutorial--assembling-the-rendering-pipeline.md)에서 장면 정보를 가지고 표시 화면에 제시하는 방법을 다루었습니다. 이제 한 단계 돌아가 렌더링하기 위해 데이터를 준비하는 방법을 알아보겠습니다.

>[!Note]
>이 샘플의 최신 게임 코드를 다운로드하지 않은 경우 [Direct3D 게임 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)로 이동합니다. 이 샘플은 UWP 기능 샘플의 큰 컬렉션의 일부입니다. 샘플을 다운로드하는 방법에 대한 지침은 [GitHub에서 UWP 샘플 가져오기](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)를 참조하세요.

## <a name="objective"></a>목표

목표에 대한 빠른 요약입니다. UWP DirectX 게임의 그래픽 출력을 표시하도록 기본 렌더링 프레임워크를 설정하는 방법을 이해하기 위해 고안되었습니다. 이를 느슨하게 이러한 3단계로 그룹화할 수 있습니다.

 1. 그래픽 인터페이스에 연결 구축
 2. 준비: 그래픽 그리는 데 필요한 리소스 만들기
 3. 그래픽 표시: 프레임 렌더링

[렌더링 프레임워크 I: 렌더링 소개](tutorial--assembling-the-rendering-pipeline.md)에서는 1단계와 3단계에서 다루는 그래픽이 렌더링되는 방식을 설명했습니다. 

이 문서에서는 이 프레임워크의 다른 부분을 설정하고 렌더링하기 전에 필요한 데이터를 준비하는 프로세스의 2단계를 설명합니다.

## <a name="design-the-renderer"></a>렌더러 디자인

렌더러는 게임 시각 효과를 생성하는 데 사용되는 모든 D3D11 및 D2D 개체를 만들고 관리합니다. __GameRenderer__ 클래스는 이 샘플 게임의 렌더러이며 게임의 렌더링 요구를 충족하도록 설계되었습니다.

다음은 게임에 대한 렌더러를 디자인하는 데 도움이 되는 몇 가지 개념입니다.
* Direct3D 11 API는 [COM](https://msdn.microsoft.com/library/windows/desktop/ms694363.aspx) API로 정의되므로 이러한 API가 정의하는 개체에 대한 [ComPtr](https://docs.microsoft.com/cpp/windows/comptr-class) 참조를 제공해야 합니다. 앱이 종료될 때 마지막 참조가 범위를 벗어나면 이러한 개체가 자동으로 해제됩니다. 자세한 내용은 [ComPtr](https://github.com/Microsoft/DirectXTK/wiki/ComPtr)를 참조하세요. 이러한 개체의 예로는 상수 버퍼, 셰이더 개체- [꼭짓점 셰이더](tutorial--assembling-the-rendering-pipeline.md#vertex-shaders-and-pixel-shaders), [픽셀 셰이더](tutorial--assembling-the-rendering-pipeline.md#vertex-shaders-and-pixel-shaders), 셰이더 리소스 개체가 있습니다.
* 렌더링을 위해 필요한 다양한 데이터를 저장하기 위해 상수 버퍼가 이 클래스에서 정의됩니다.
    * 각각 빈도가 다른 여러 상수 버퍼를 사용하면 프레임당 GPU로 전송해야 하는 데이터 양이 감소합니다. 이 샘플에서는 업데이트해야 하는 빈도를 기준으로 상수를 각기 다른 버퍼로 구분합니다. 이 방법은 Direct3D 프로그래밍의 모범 사례입니다. 
    * 이 게임 샘플에서 4 상수 버퍼가 정의됩니다.
        1. __m\_constantBufferNeverChanges__에는 조명 매개 변수가 들어 있습니다. 이는 __FinalizeCreateGameDeviceResources__ 메서드에서 한 번 설정되고 다시 변경되지 않습니다.
        2. __m\_constantBufferChangeOnResize__에는 투영 행렬이 들어 있습니다. 투영 행렬은 창의 크기와 가로 세로 비율에 따라 달라집니다. 이는 [__CreateWindowSizeDependentResources__](#createwindowsizedependentresources-method)에서 설정된 다음 리소스가 [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) 메서드에 로드된 후 업데이트됩니다. 3D로 렌더링하면 이 또한 프레임별로 두 번 변경됩니다.
        3. __m\_constantBufferChangesEveryFrame__에는 보기 행렬이 들어 있습니다. 이 행렬은 카메라 위치와 보기 방향(프로젝션에 수직)에 따라 달라지며 __Render__ 메서드에서 프레임당 한 번 변경됩니다. 이는 이전에 [__GameRenderer::Render__ 메서드](tutorial--assembling-the-rendering-pipeline.md#gamerendererrender-method) 아래의 __렌더링 프레임워크 I: 렌더링 소개__에서 다루었습니다.
        4. __m\_constantBufferChangesEveryPrim__에는 각 원형의 모델 행렬과 재질 속성이 들어 있습니다. 모델 행렬은 로컬 좌표에서 월드 좌표로 꼭짓점을 변환합니다. 이러한 상수는 각 원형과 관련이 있으며 그리기 호출 시마다 업데이트됩니다. 이는 이전에 [원형 렌더링](tutorial--assembling-the-rendering-pipeline.md#primitive-rendering) 아래의 __렌더링 프레임워크 I: 렌더링 소개__에서 다루었습니다.
* 이 클래스에서 원형의 텍스처를 포함하는 셰이더 리소스 개체도 정의됩니다.
    * 일부 텍스처는 미리 정의됩니다.([DDS](https://msdn.microsoft.com/library/windows/desktop/bb943991.aspx)는 압축되거나 압축되지 않은 텍스처를 저장하기 위해 사용할 수 있는 파일 형식입니다. DDS 텍스처는 월드의 벽과 마루, 탄약 구형에 사용됩니다.
    * 이 게임 샘플에서 셰이더 리소스 개체는 __m\_sphereTexture__, __m\_cylinderTexture__, __m\_ceilingTexture__, __m\_floorTexture__, __m\_wallsTexture__입니다.
* 이 클래스의 셰이더 개체가 원형 및 텍스처를 계산하기 위해 정의됩니다. 
    * 이 게임 샘플에서 셰이더 개체는 __m\_vertexShader__, __m\_vertexShaderFlat__, 및 __m\_pixelShader__, __m\_pixelShaderFlat__입니다.
    * 꼭짓점 셰이더는 원형과 기본 조명을 처리하고, 픽셀 셰이더(조각 셰이더라고 함)는 텍스처와 픽셀당 효과를 처리합니다.
    * 이러한 셰이더는 다른 원형을 렌더링하기 위한 두 가지 버전(일반 및 평면)이 있습니다. 다른 버전이 있는 이유는 평면 버전이 더 단순하며 반사 하이라이트나 픽셀당 조명 효과가 없기 때문입니다. 이 버전은 벽에 사용되며 저전력 디바이스에서 렌더링 속도를 늘립니다.

## <a name="gamerendererh"></a>GameRenderer.h

이제 게임 샘플의 렌더러 클래스 개체를 살펴보고 이를 DirectX 11 앱 템플릿에서 제공되는 __Sample3DSceneRenderer.h__와 비교해 보겠습니다.

```cpp
// Class object handling the rendering of the game
ref class GameRenderer
{
internal:
    GameRenderer(const std::shared_ptr<DX::DeviceResources>& deviceResources);
    
    // Compared with Sample3DSceneRenderer.h in the DirectX 11 App template sample. 
    
    // These methods are present.
    void CreateDeviceDependentResources();
    void CreateWindowSizeDependentResources();
    void ReleaseDeviceDependentResources();
    void Render();

    // Added: Async related methods to prepare 3D game objects for rendering
    concurrency::task<void> CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game);
    void FinalizeCreateGameDeviceResources();
    concurrency::task<void> LoadLevelResourcesAsync();
    void FinalizeLoadLevelResources();
    // --- end of async related methods section

    // Added: Methods for rendering overlay
    Simple3DGameDX::IGameUIControl^ GameUIControl()  { return m_gameInfoOverlay; };

    DirectX::XMFLOAT2 GameInfoOverlayUpperLeft()
    {
        return DirectX::XMFLOAT2(m_gameInfoOverlayRect.left, m_gameInfoOverlayRect.top);
    };
    DirectX::XMFLOAT2 GameInfoOverlayLowerRight()
    {
        return DirectX::XMFLOAT2(m_gameInfoOverlayRect.right, m_gameInfoOverlayRect.bottom);
    };
    bool GameInfoOverlayVisible() { return m_gameInfoOverlay->Visible(); }
    // --- end of rendering overlay section

    //...
    protected private:
    // Cached pointer to device resources.
    std::shared_ptr<DX::DeviceResources>                m_deviceResources;

    // ...

    // Shader resource objects
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_sphereTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_cylinderTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_ceilingTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_floorTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_wallsTexture;

    // Constant Buffers
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferNeverChanges;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangeOnResize;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangesEveryFrame;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangesEveryPrim;

    // Texture sampler
    Microsoft::WRL::ComPtr<ID3D11SamplerState>          m_samplerLinear;

    // Shader objects: Vertex shaders and pixel shaders
    Microsoft::WRL::ComPtr<ID3D11VertexShader>          m_vertexShader;
    Microsoft::WRL::ComPtr<ID3D11VertexShader>          m_vertexShaderFlat;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>           m_pixelShader;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>           m_pixelShaderFlat;
    Microsoft::WRL::ComPtr<ID3D11InputLayout>           m_vertexLayout;
};
```

## <a name="constructor"></a>생성자

다음으로 게임 샘플의 __GameRenderer__ 생성자를 살펴보고 이를 DirectX 11 앱 템플릿에서 제공되는 __Sample3DSceneRenderer__ 생성자와 비교해 보겠습니다.

```cpp
// Constructor method of the main rendering class object
GameRenderer::GameRenderer(const std::shared_ptr<DX::DeviceResources>& deviceResources) : //...
{
    // Compared with Sample3DSceneRenderer::Sample3DSceneRenderer in the DirectX 11 App template sample. 
    
    // Added: Create a new GameHud object to rendered text on the top left corner of the screen
    m_gameHud = ref new GameHud(
        deviceResources,
        "Windows platform samples",
        "DirectX first-person game sample"
        );
    //--- end of new GameHud object section
        
    // Added: Game info rendered as an overlay on the top right corner of the screen (eg. Hits, Shots, Time)    
    m_gameInfoOverlay = ref new GameInfoOverlay(deviceResources);
    //--- end of game info rendered as overlay section

    // These methods are present.
    CreateDeviceDependentResources();
    CreateWindowSizeDependentResources();
}
```

## <a name="create-and-load-directx-graphic-resources"></a>DirectX 그래픽 리소스 만들기 및 로드

게임 샘플(그리고 Visual Studio의 __DirectX 11 앱(유니버설 Windows)__ 템플릿)에서 게임 리소스 만들기 및 로딩은 __GameRenderer__ 생성자에서 호출되는 이 두 메서드를 사용하여 구현됩니다.

* [__CreateDeviceDependentResources__](#createdevicedependentresources-method)
* [__CreateWindowSizeDependentResources__](#createwindowsizedependentresources-method)

## <a name="createdevicedependentresources-method"></a>CreateDeviceDependentResources 메서드

DirectX 11 앱 템플릿에서 이 메서드는 꼭짓점 및 픽셀 셰이더를 비동기적으로 로드하고, 셰이더 및 상수 버퍼를 만들고, 위치 및 색 정보를 포함하는 꼭짓점이 있는 메시를 만드는 데 사용됩니다. 

샘플 게임에서 장면 개체의 이러한 작업은 대신 [__CreateGameDeviceResourcesAsync__](#creategamedeviceresourcesasync-method) 및 [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) 메서드 간에 분할됩니다. 

이 게임 샘플의 경우 이 메서드에 무엇이 분할됩니까?

* 리소스가 비동기적으로 로드되기 때문에 렌더링하기 전에 리소스가 로드되었는지 나타내는 인스턴스화된 변수(__m\_gameResourcesLoaded__ = false and __m\_levelResourcesLoaded__ = false). 
* HUD 및 오버레이 렌더링은 별도의 클래스 개체이지 때문에 여기에서 __GameHud::CreateDeviceDependentResources__ 및 __GameInfoOverlay::CreateDeviceDependentResources__ 메서드를 호출합니다.

다음은 __GameRenderer::CreateDeviceDependentResources__에 대한 코드입니다.

```cpp
// This method is called in GameRenderer constructor when it's created in GameMain constructor.
void GameRenderer::CreateDeviceDependentResources()
{
    // instantiate variables that indicate if resources were loaded.
    m_gameResourcesLoaded = false;
    m_levelResourcesLoaded = false;

    // game HUD and overlay are design as separate class objects.
    m_gameHud->CreateDeviceDependentResources();
    m_gameInfoOverlay->CreateDeviceDependentResources();
}
```
아래 표에서 리소스를 만들고 로드하는 데 사용되는 메서드를 나열합니다. 리소스를 비동기적으로 로드할 수 있도록 __CreateGameDeviceResourcesAsync__ 및 __FinalizeCreateGameDeviceResources__가 샘플 게임에 추가됩니다.

|원본 DirectX 11 앱 템플릿           |샘플 게임                                                                |
|-------------------------------------------|---------------------------------------------------------------------------|
|CreateDeviceDependentResources             |CreateDeviceDependentResources                                             |
|                                           | - CreateGameDeviceResourcesAsync(추가됨)                                  |
|                                           | - FinalizeCreateGameDeviceResources(추가됨)                               |
|CreateWindowSizeDependentResources         |CreateWindowSizeDependentResources                                         |

리소스를 만들고 로드하는 데 사용하는 다른 메서드에 대해 자세히 알아보기 전에 먼저 렌더러를 만들고 해당 렌더러가 어떻게 게임 루프에 적용되는지 확인해 보겠습니다.

## <a name="create-the-renderer"></a>렌더러 만들기

__GameRenderer__는 __GameMain__의 생성자에서 생성됩니다. 이는 리소스를 만들고 로드하는 것을 돕기 위해 추가되는 두 가지 다른 메서드 [__CreateGameDeviceResourcesAsync__](#creategamedeviceresourcesasync-method) 및 [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method)를 호출합니다.

```cpp

GameMain::GameMain(const std::shared_ptr<DX::DeviceResources>& deviceResources) : // ...
{
    m_deviceResources->RegisterDeviceNotify(this);

    // These methods are used in the DirectX 11 App template to create the class objects used for rendering. 
    // But are replaced in this game sample with GameRenderer as shown below.
    // m_sceneRenderer = std::unique_ptr<Sample3DSceneRenderer>(new Sample3DSceneRenderer(m_deviceResources));
    // m_fpsTextRenderer = std::unique_ptr<SampleFpsTextRenderer>(new SampleFpsTextRenderer(m_deviceResources));
    
    // Creation of GameRenderer
    m_renderer = ref new GameRenderer(m_deviceResources);
    
    //...

     create_task([this]()
    {
        // Asynchronously initialize the game class and load the renderer device resources.
        // By doing all this asynchronously, the game gets to its main loop more quickly
        // and in parallel all the necessary resources are loaded on other threads.
        m_game->Initialize(m_controller, m_renderer);

        return m_renderer->CreateGameDeviceResourcesAsync(m_game);

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // as the m_renderer object was created because the D3D device context
        // can ONLY be accessed on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState();
    
    //...
}
```

## <a name="creategamedeviceresourcesasync-method"></a>CreateGameDeviceResourcesAsync 메서드

비동기적으로 게임 리소스를 로드하므로 __CreateGameDeviceResourcesAsync__는 __create\_task__의 __GameMain__ 생성자 메서드에서 호출됩니다.
        
__CreateDeviceResourcesAsync__는 게임 리소스를 로드하기 위해 별도의 비동기 작업 집합으로 실행되는 메서드입니다. 이 메서드는 별도의 스레드에서 실행되어야 하므로 Direct3D 11 디바이스 메서드(__ID3D11Device__에 정의되어 있음)에만 액세스할 수 있고 디바이스 컨텍스트 메서드(__ID3D11DeviceContext__에 정의된 메서드)에는 액세스할 수 없습니다. 따라서 렌더링을 수행하지 않습니다.

__FinalizeCreateGameDeviceResources__ 메서드는 주 스레드에서 실행되고 Direct3D 11 디바이스 컨텍스트 메서드에 액세스할 수 있습니다.

원칙:
* __CreateGameDeviceResourcesAsync__의 __ID3D11Device__ 메서드만 사용합니다. 자유 스레드 방식, 즉 모든 스레드에서 실행할 수 있기 때문입니다. 또한 __GameRenderer__가 생성된 것과 동일한 스레드에서 실행하지 않는 것이 좋습니다. 
* 이 메서드는 단일 스레드, 그리고 __GameRenderer__와 동일한 스레드에서 실행해야 하므로 여기에서 __ID3D11DeviceContext__에 메서드를 사용하지 마십시오.
* 이 메서드를 사용하여 상수 버퍼를 만들 수 있습니다.
* 이 메서드를 사용하여 텍스처(예: .dds 파일) 및 셰이더 정보(예: .cso 파일)를 [셰이더](tutorial--assembling-the-rendering-pipeline.md#shaders)에 로드할 수 있습니다.

이 메서드는 다음을 위해 사용됩니다.
* __m\_constantBufferNeverChanges__, __m\_constantBufferChangeOnResize__, __m\_constantBufferChangesEveryFrame__, __m\_constantBufferChangesEveryPrim__ 등 4개의 [상수 버퍼](tutorial--assembling-the-rendering-pipeline.md#buffer) 만들기
* 텍스처에 대한 샘플 정보를 캡슐화하는 [sampler-state](tutorial--assembling-the-rendering-pipeline.md#sampler-state) 개체 만들기
* 메서드에서 만든 모든 비동기 작업이 포함된 작업 그룹을 만듭니다. 이러한 모든 비동기 작업이 완료될 때까지 기다린 다음 __FinalizeCreateGameDeviceResources__를 호출합니다.
* [기본 로더](tutorial--assembling-the-rendering-pipeline.md#basicloader)를 사용하여 로더를 만듭니다. 로더의 비동기 로딩 작업을 이전에 만든 작업 그룹에 추가합니다.
* __BasicLoader::LoadShaderAsync__ 및 __BasicLoader::LoadTextureAsync__ 등의 메서드가 다음을 로드하는 데 사용됩니다.
    * 컴파일된 셰이더 개체(VertextShader.cso, VertexShaderFlat.cso, PixelShader.cso, PixelShaderFlat.cso). 자세한 내용은 [다양한 셰이더 파일 형식](tutorial--assembling-the-rendering-pipeline.md#various-shader-file-formats)으로 이동합니다.
    * 게임 관련 텍스처(Assets\\seafloor.dds, metal_texture.dds, cellceiling.dds, cellfloor.dds, cellwall.dds).

```cpp
task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    // Create the device dependent game resources.
    // Only the d3dDevice is used in this method.  It is expected
    // to not run on the same thread as the GameRenderer was created.
    // Create methods on the d3dDevice are free-threaded and are safe while any methods
    // in the d3dContext should only be used on a single thread and handled
    // in the FinalizeCreateGameDeviceResources method.
    m_game = game;

    auto d3dDevice = m_deviceResources->GetD3DDevice();

    // Define D3D11_BUFFER_DESC.
    // For API ref, go to: https://msdn.microsoft.com/library/windows/desktop/ff476092.aspx
    D3D11_BUFFER_DESC bd;
    ZeroMemory(&bd, sizeof(bd));
    
    bd.Usage = D3D11_USAGE_DEFAULT;
    // ...
    
    // Create the constant buffers: m_constantBufferNeverChanges, m_constantBufferChangeOnResize,
    // m_constantBufferChangesEveryFrame, m_constantBufferChangesEveryPrim
    // CreateBuffer is used to create one of these buffers: vertex buffer, index buffer, or 
    // shader-constant buffer. For CreateBuffer API ref info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476501.aspx
    
    DX::ThrowIfFailed(
        d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferNeverChanges) 
        );
    // ...
    
    // Define D3D11_SAMPLER_DESC. For API ref, go to: https://msdn.microsoft.com/library/windows/desktop/ff476207.aspx
    D3D11_SAMPLER_DESC sampDesc;

    // ZeroMemory fills a block of memory with zeros. 
    // For API ref, go to: https://msdn.microsoft.com/en-us/library/windows/desktop/aa366920(v=vs.85).aspx
    ZeroMemory(&sampDesc, sizeof(sampDesc));

    sampDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;
    sampDesc.AddressU = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.AddressV = D3D11_TEXTURE_ADDRESS_WRAP;
    // ...
    
    // Create a sampler-state object that encapsulates sampling information for a texture.
    // The sampler-state interface holds a description for sampler state that you can bind to any 
    // shader stage of the pipeline for reference by texture sample operations.
    DX::ThrowIfFailed(
        d3dDevice->CreateSamplerState(&sampDesc, &m_samplerLinear)
        );

    // Start the async tasks to load the shaders and textures (resources).
    
    // Load compiled shader objects (VertextShader.cso, VertexShaderFlat.cso, PixelShader.cso, and PixelShaderFlat.cso).
    // The BasicLoader class is used to convert and load common graphics resources, such as meshes, textures, 
    // and various shader objects into the constant buffers. 
    // For more info, go to: https://docs.microsoft.com/windows/uwp/gaming/complete-code-for-basicloader
    BasicLoader^ loader = ref new BasicLoader(d3dDevice);

    std::vector<task<void>> tasks;

    uint32 numElements = ARRAYSIZE(PNTVertexLayout);

    // Load shaders asynchronously with the shader and pixel data using the BasicLoader::LoadShaderAsync method
    // Push these method calls into a list of tasks
    tasks.push_back(loader->LoadShaderAsync("VertexShader.cso", PNTVertexLayout, numElements, &m_vertexShader, &m_vertexLayout));
    tasks.push_back(loader->LoadShaderAsync("VertexShaderFlat.cso", nullptr, numElements, &m_vertexShaderFlat, nullptr));
    tasks.push_back(loader->LoadShaderAsync("PixelShader.cso", &m_pixelShader));
    tasks.push_back(loader->LoadShaderAsync("PixelShaderFlat.cso", &m_pixelShaderFlat));

    // Make sure the previous versions are set to NULL before any of the textures are loaded.
    m_sphereTexture = nullptr;
    // ...

    // Load Game specific textures (Assets\\seafloor.dds, metal_texture.dds, cellceiling.dds, cellfloor.dds, cellwall.dds).
    // Push these method calls also into a list of tasks
    tasks.push_back(loader->LoadTextureAsync("Assets\\seafloor.dds", nullptr, &m_sphereTexture));
    // ...
    
    tasks.push_back(create_task([]()
    {
        // Simulate loading additional resources by introducing a delay.
        wait(GameConstants::InitialLoadingDelay);
    }));

    // Returns when all the async tasks for loading the shader and texture assets have completed.
    return when_all(tasks.begin(), tasks.end());
}
```

## <a name="finalizecreategamedeviceresources-method"></a>FinalizeCreateGameDeviceResources 메서드

__FinalizeCreateGameDeviceResources__ 메서드는 __CreateGameDeviceResourcesAsync__ 메서드의 모든 리소스 로드 작업이 완료된 후에 호출됩니다. 

* 위치와 색의 constantBufferNeverChanges를 초기화합니다. __ID3D11DeviceContext::UpdateSubresource__에 대한 디바이스 컨텍스트 메서드 호출을 사용하여 초기 데이터를 상수 버퍼에 로드합니다. .
* 비동기적으로 로드된 리소스 로드가 완료되었으므로 이제 이를 적절한 게임 개체와 연결해야 합니다.
* 각 게임 개체에 대해 로드된 텍스처를 사용하여 메시 및 재질을 만듭니다. 그런 다음 메시 및 재질을 게임 개체에 연결합니다.
* 대상 게임 개체에 대해 맨 위에 숫자 값이 있고 동심원으로 색상이 지정된 고리로 구성된 텍스처는 텍스처 파일에서 로드되지 않습니다. 대신 절차에 따라 __TargetTexture.cpp__의 코드를 사용하여 생성됩니다. __TargetTexture__ 클래스는 초기화 시 화면 외부 리소스에 텍스처를 그리는 데 필요한 리소스를 만듭니다. 결과 텍스처는 적절한 대상 게임 개체와 연결됩니다.

__FinalizeCreateGameDeviceResources__ 및 [__CreateWindowSizeDependentResources__](#createwindowsizedependentresource-method)는 다음과 코드의 유사한 부분을 공유합니다.
* __SetProjParams__를 사용하여 카메라에 오른쪽 투영 행렬이 있는지 확인합니다. 자세한 내용은 [카메라 및 좌표 공간](tutorial--assembling-the-rendering-pipeline.md#camera-and-coordinate-space)을 참조하세요.
* 3D 회전 행렬을 카메라의 투영 행렬에 곱하는 포스트로 화면 회전을 처리합니다. 그런 다음 __ConstantBufferChangeOnResize__ 상수 버퍼를 결과 투영 행렬로 업데이트합니다.
* __m\_gameResourcesLoaded__ __부울__ 전역 변수를 리소스가 이제 버퍼에 로드되어 다음 단계에 대한 준비가 되었음을 나타내도록 설정합니다. 처음에 __GameRenderer::CreateDeviceDependentResources__ 메서드를 통해 __GameRenderer__의 생성자 메서드에서 이 변수를 __FALSE__로 초기화했습니다. 
* 이 __m\_gameResourcesLoaded__가 __TRUE__이면 장면 개체 렌더링이 발생할 수 있습니다. 이는 이전에 [__GameRenderer::Render 메서드__](tutorial--assembling-the-rendering-pipeline.md#gamerendererrender-method) 아래의 __렌더링 프레임워크 I: 렌더링 소개__ 문서에서 다루었습니다.

```cpp
// When creating this sample game using the DirectX 11 App template, this method needs to be created.
// This new method is called from GameMain constructor in the .then loop.
// Make sure the 2D rendering is occurring on the same thread as the main rendering.
// Note: Helper class .h and .cpp files used in this method are located in the SharedContent/cpp/GameContent folder
void GameRenderer::FinalizeCreateGameDeviceResources()
{
    // All asynchronously loaded resources have completed loading.
    // Now associate all the resources with the appropriate game objects.
    // This method is expected to run in the same thread as the GameRenderer
    // was created. All work will happen behind the "Loading ..." screen after the
    // main loop has been entered.

    // Initialize constantBufferNeverChanges with the light positions and color.
    // These are handled here to ensure that the d3dContext is only
    // used in one thread.

    auto d3dDevice = m_deviceResources->GetD3DDevice();
    ConstantBufferNeverChanges constantBufferNeverChanges;
    constantBufferNeverChanges.lightPosition[0] = XMFLOAT4( 3.5f, 2.5f,  5.5f, 1.0f);
    // ...
    constantBufferNeverChanges.lightColor = XMFLOAT4(0.25f, 0.25f, 0.25f, 1.0f);

    // CPU copies data from memory (constantBufferNeverChanges) to a subresource 
    // created in non-mappable memory (m_constantBufferNeverChanges) which was created in the earlier 
    // CreateGameDeviceResourcesAsync method. For UpdateSubresource API ref info, 
    // go to: https://msdn.microsoft.com/library/windows/desktop/ff476486.aspx
    // To learn more about what a subresource is, go to:
    // https://msdn.microsoft.com/library/windows/desktop/ff476901.aspx

    m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
        m_constantBufferNeverChanges.Get(),
        0,
        nullptr,
        &constantBufferNeverChanges,
        0,
        0
        );

    // For the objects that function as targets, they have two unique generated textures.
    // One version is used to show that they have never been hit and the other is 
    // used to show that they have been hit.
    // TargetTexture is a helper class to procedurally generate textures for game
    // targets. The class creates the necessary resources to draw the texture into 
    // an off screen resource at initialization time.

    TargetTexture^ textureGenerator = ref new TargetTexture(
        d3dDevice,
        m_deviceResources->GetD2DFactory(),
        m_deviceResources->GetDWriteFactory(),
        m_deviceResources->GetD2DDeviceContext()
        );

    // CylinderMesh is a class derived from MeshObject and creates a ID3D11Buffer of
    // vertices and indices to represent a canonical cylinder (capped at
    // both ends) that is positioned at the origin with a radius of 1.0,
    // a height of 1.0 and with its axis in the +Z direction.
    // In the game sample, there are various types of mesh types:
    // CylinderMesh (vertical rods), SphereMesh (balls that the player shoots), 
    // FaceMesh (target objects), and WorldMesh (Floors and ceilings that define the enclosed area)

    MeshObject^ cylinderMesh = ref new CylinderMesh(d3dDevice, 26);
    // ...

    // The Material class maintains the properties that represent how an object will
    // look when it is rendered.  This includes the color of the object, the
    // texture used to render the object, and the vertex and pixel shader that
    // should be used for rendering.

    Material^ cylinderMaterial = ref new Material(
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(1.0f, 1.0f, 1.0f, 1.0f),
        15.0f,
        m_cylinderTexture.Get(),
        m_vertexShader.Get(),
        m_pixelShader.Get()
        );

    // ...
    auto objects = m_game->RenderObjects();

    // Attach the textures to the appropriate game objects.
    // We'll loop through all the objects that need to be rendered.
    for (auto object = objects.begin(); object != objects.end(); object++)
    {

        if ((*object)->TargetId() == GameConstants::WorldFloorId)
        {
            // Assign a normal material for the floor object.
            // This normal material uses the floor texture (cellfloor.dds) that was loaded asynchronously from
            // the Assets folder using BasicLoader::LoadTextureAsync method in the earlier 
            // CreateGameDeviceResourcesAsync loop

            (*object)->NormalMaterial(
                ref new Material(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_floorTexture.Get(),
                    m_vertexShaderFlat.Get(),
                    m_pixelShaderFlat.Get()
                    )
                );
            // Creates a mesh object called WorldFloorMesh and assign it to the floor object.
            (*object)->Mesh(ref new WorldFloorMesh(d3dDevice));
        }
        // ...
        else if (Cylinder^ cylinder = dynamic_cast<Cylinder^>(*object))
        {
            cylinder->Mesh(cylinderMesh);
            cylinder->NormalMaterial(cylinderMaterial);
        }
        else if (Face^ target = dynamic_cast<Face^>(*object))
        {
            const int bufferLength = 16;
            char16 str[bufferLength];
            int len = swprintf_s(str, bufferLength, L"%d", target->TargetId());
            Platform::String^ string = ref new Platform::String(str, len);

            ComPtr<ID3D11ShaderResourceView> texture;
            textureGenerator->CreateTextureResourceView(string, &texture);
            target->NormalMaterial(
                ref new Material(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.Get(),
                    m_vertexShader.Get(),
                    m_pixelShader.Get()
                    )
                );

            textureGenerator->CreateHitTextureResourceView(string, &texture);
            target->HitMaterial(
                ref new Material(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.Get(),
                    m_vertexShader.Get(),
                    m_pixelShader.Get()
                    )
                );

            target->Mesh(targetMesh);
        }
        // ...
    }


    // The SetProjParams method calculates the projection matrix based on input params and
    // ensures that the camera has been initialized with the right projection
    // matrix.  
    // The camera is not created at the time the first window resize event occurs.

    auto renderTargetSize = m_deviceResources->GetRenderTargetSize();
    m_game->GameCamera()->SetProjParams(
        XM_PI / 2,
        renderTargetSize.Width / renderTargetSize.Height,
        0.01f,
        100.0f
        );

    // Make sure that the correct projection matrix is set in the ConstantBufferChangeOnResize buffer.

    // Get the 3D rotation transform matrix. We are handling screen rotations directly to eliminate an unaligned 
    // fullscreen copy. So it is necessary to post multiply the 3D rotation matrix to the camera's projection matrix
    // to get the projection matrix that we need.

    auto orientation = m_deviceResources->GetOrientationTransform3D();

    ConstantBufferChangeOnResize changesOnResize;

    // The matrices are transposed due to the shader code expecting the matrices in the opposite
    // row/column order from the DirectX math library.

    // XMStoreFloat4x4 takes a matrix and writes the components out to sixteen single-precision floating-point values at the given address. 
    // The most significant component of the first row vector is written to the first four bytes of the address, 
    // followed by the second most significant component of the first row, and so on. The second row is then written out in a 
    // like manner to memory beginning at byte 16, followed by the third row to memory beginning at byte 32, and finally 
    // the fourth row to memory beginning at byte 48. For more API ref info, go to: 
    // https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.storing.xmstorefloat4x4.aspx

    XMStoreFloat4x4(
        &changesOnResize.projection,
        XMMatrixMultiply(
            XMMatrixTranspose(m_game->GameCamera()->Projection()),
            XMMatrixTranspose(XMLoadFloat4x4(&orientation))
            )
        );

    // UpdateSubresource method instructs CPU to copy data from memory (changesOnResize) to a subresource 
    // created in non-mappable memory (m_constantBufferChangeOnResize ) which was created in the earlier 
    // CreateGameDeviceResourcesAsync method.

    m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
        m_constantBufferChangeOnResize.Get(),
        0,
        nullptr,
        &changesOnResize,
        0,
        0
        );

    // Finally we set the m_gameResourcesLoaded as TRUE, so we can start rendering.
    m_gameResourcesLoaded = true;
}
```

## <a name="createwindowsizedependentresource-method"></a>CreateWindowSizeDependentResource 메서드

CreateWindowSizeDependentResources 메서드는 창 크기, 방향, 스테레오 활성화 렌더링, 또는 해상도 변경 시마다 호출됩니다. 샘플 게임에서 __ConstantBufferChangeOnResize__투영 행렬을 업데이트 됩니다.

창 크기 리소스는 이러한 방식으로 업데이트됩니다. 
* 앱 프레임워크는 창 상태의 변경을 나타내는 몇 가지 가능한 이벤트 중 하나를 가져옵니다. 
* 그러면 기본 게임 루프에 이벤트에 대해 알리며 기본 클래스(__GameMain__) 인스턴스에 __CreateWindowSizeDependentResources__가 호출되고, 이어 게임 렌더러(__GameRenderer__) 클래스에 __CreateWindowSizeDependentResources__ 구현이 호출됩니다.
* 이 메서드의 주요 작업은 창 속성의 변경으로 인해 시각 효과가 혼동되거나 잘못되지 않는지 확인하는 것입니다.

이 게임 샘플에서는 여러 메서드 호출은 [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) 메서드와 동일합니다. 코드 연습의 경우 이전 섹션으로 이동합니다.

게임 HUD 및 오버레이 창 크기 렌더링 조정은 [사용자 인터페이스 추가](#tutorial--adding-a-user-interface)에서 다룹니다.

```cpp
// Initializes view parameters when the window size changes.
void GameRenderer::CreateWindowSizeDependentResources()
{

    // Game HUD and overlay window size rendering adjustments are done here
    // but they'll be covered in the UI section instead.

    m_gameHud->CreateWindowSizeDependentResources();

    // ...

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    // In Sample3DSceneRenderer::CreateWindowSizeDependentResources, we had:
    // Size outputSize = m_deviceResources->GetOutputSize();

    auto renderTargetSize = m_deviceResources->GetRenderTargetSize();

    // ...

    m_gameInfoOverlay->CreateWindowSizeDependentResources(m_gameInfoOverlaySize);

    if (m_game != nullptr)
    {
        // Similar operations as the last section of FinalizeCreateGameDeviceResources method
        m_game->GameCamera()->SetProjParams(
            XM_PI / 2, renderTargetSize.Width / renderTargetSize.Height,
            0.01f,
            100.0f
            );

        XMFLOAT4X4 orientation = m_deviceResources->GetOrientationTransform3D();

        ConstantBufferChangeOnResize changesOnResize;
        XMStoreFloat4x4(
            &changesOnResize.projection,
            XMMatrixMultiply(
                XMMatrixTranspose(m_game->GameCamera()->Projection()),
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
}
```

## <a name="next-steps"></a>다음 단계

이것이 게임의 프레임워크를 렌더링하는 그래픽을 구현하는 기본 프로세스입니다. 게임의 규모가 클수록 개체 유형 및 애니메이션 동작의 계층 구조를 처리하기 위해 더 많은 추상화를 배치해야 할 것입니다. 메시 및 텍스처와 같은 자산을 로드 및 관리하기 위한 더 복잡한 메서드를 구현해야 합니다. 다음으로 [사용자 인터페이스 추가](tutorial--adding-a-user-interface.md)에 대해 알아봅니다.