---
title: 설정
description: 렌더링 파이프라인을 조합 하 여 그래픽을 표시 하는 방법을 알아봅니다. 게임 렌더링, 데이터 설정 및 준비
ms.assetid: 7720ac98-9662-4cf3-89c5-7ff81896364a
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 렌더링
ms.localizationpriority: medium
ms.openlocfilehash: a87382aeffb0e0b7a8eaca1c4baec8561049e91e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168247"
---
# <a name="rendering-framework-ii-game-rendering"></a>렌더링 프레임 워크 II: 게임 렌더링

> [!NOTE]
> 이 항목은 DirectX 자습서 시리즈 [를 사용 하 여 단순 유니버설 Windows 플랫폼 (UWP) 게임 만들기](tutorial--create-your-first-uwp-directx-game.md) 의 일부입니다. 해당 링크의 항목은 계열의 컨텍스트를 설정 합니다.

[렌더링 프레임 워크](tutorial--assembling-the-rendering-pipeline.md)에서 장면 정보를 사용 하 여 표시 화면에 표시 하는 방법을 살펴보았습니다. 이제 이전 단계를 수행 하 고 렌더링을 위해 데이터를 준비 하는 방법을 알아봅니다.

>[!Note]
>이 샘플에 대 한 최신 게임 코드를 다운로드 하지 않은 경우 [Direct3D sample game](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)로 이동 합니다. 이 샘플은 여러 UWP 기능 샘플 컬렉션의 일부입니다. 샘플을 다운로드 하는 방법에 대 한 지침은 [GitHub에서 UWP 샘플 가져오기](../get-started/get-app-samples.md)를 참조 하세요.

## <a name="objective"></a>Objective

목표에 대 한 간략 한 요약입니다. UWP DirectX 게임의 그래픽 출력을 표시 하는 기본 렌더링 프레임 워크를 설정 하는 방법을 이해 하는 것입니다. 이러한 세 단계로 느슨하게 그룹화 할 수 있습니다.

 1. 그래픽 인터페이스에 대 한 연결 설정
 2. 준비: 그래픽을 그리는 데 필요한 리소스를 만듭니다.
 3. 그래픽 표시: 프레임을 렌더링 합니다.

[렌더링 프레임 워크 I: 렌더링 소개-](tutorial--assembling-the-rendering-pipeline.md) 1 단계 및 3 단계를 포함 하 여 그래픽이 렌더링 되는 방식을 설명 합니다. 

이 문서에서는 프로세스의 2 단계인 렌더링을 수행 하기 전에이 프레임 워크의 다른 부분을 설정 하 고 필요한 데이터를 준비 하는 방법을 설명 합니다.

## <a name="design-the-renderer"></a>렌더러 디자인

렌더러는 게임 시각적 개체를 생성 하는 데 사용 되는 모든 D3D11 및 D2D 개체를 만들고 유지 관리 하는 일을 담당 합니다. __GameRenderer__ 클래스는이 샘플 게임의 렌더러 이며 게임의 렌더링 요구를 충족 하도록 설계 되었습니다.

게임의 렌더러를 디자인 하는 데 사용할 수 있는 몇 가지 개념은 다음과 같습니다.
* Direct3D 11 Api는 [COM](/windows/desktop/com/the-component-object-model) api로 정의 되므로 이러한 api에 의해 정의 된 개체에 대 한 [ComPtr](/cpp/windows/comptr-class) 참조를 제공 해야 합니다. 이러한 개체는 앱이 종료 될 때 마지막 참조가 범위를 벗어나면 자동으로 해제 됩니다. 자세한 내용은 [ComPtr](https://github.com/Microsoft/DirectXTK/wiki/ComPtr)를 참조 하세요. 이러한 개체의 예로는 상수 버퍼, 셰이더 개체- [꼭 짓 점 셰이더](tutorial--assembling-the-rendering-pipeline.md#vertex-shaders-and-pixel-shaders), [픽셀 셰이더](tutorial--assembling-the-rendering-pipeline.md#vertex-shaders-and-pixel-shaders)및 셰이더 리소스 개체가 있습니다.
* 상수 버퍼는 렌더링에 필요한 다양 한 데이터를 저장 하기 위해이 클래스에 정의 됩니다.
    * 서로 다른 빈도로 여러 상수 버퍼를 사용 하 여 프레임당 GPU로 전송 해야 하는 데이터 양을 줄입니다. 이 샘플에서는 업데이트 해야 하는 빈도에 따라 상수를 서로 다른 버퍼로 분리 합니다. 이것은 Direct3D 프로그래밍에 대 한 모범 사례입니다. 
    * 이 샘플 게임에서는 4 개의 상수 버퍼가 정의 됩니다.
        1. __m \_ constantBufferNeverChanges__ 는 조명 매개 변수를 포함 합니다. __FinalizeCreateGameDeviceResources__ 메서드에서 한 번 설정 하 고 다시 변경 하지 않습니다.
        2. __m \_ constantBufferChangeOnResize__ 는 프로젝션 행렬을 포함 합니다. 프로젝션 매트릭스는 창의 크기 및 가로 세로 비율에 따라 달라 집니다. [__CreateWindowSizeDependentResources__](#createwindowsizedependentresource-method) 에 설정 된 다음 리소스가 [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) 메서드에 로드 된 후 업데이트 됩니다. 3D로 렌더링 하는 경우 프레임당 두 번 변경 됩니다.
        3. __m \_ constantBufferChangesEveryFrame__ 는 뷰 매트릭스를 포함 합니다. 이 매트릭스는 카메라 위치 및 모양 (프로젝션의 법선)에 따라 달라 지 며 __Render__ 메서드에서 프레임당 한 번 변경 됩니다. 이전에는 __렌더링 프레임 워크 I: 렌더링 소개__의 [ __GameRenderer:: Render__ 메서드](tutorial--assembling-the-rendering-pipeline.md#gamerendererrender-method)아래에 설명 되어 있습니다.
        4. __m \_ constantBufferChangesEveryPrim__ 각 기본 형식의 모델 행렬 및 재질 속성을 포함 합니다. 모델 매트릭스는 꼭 짓 점을 로컬 좌표에서 세계 좌표로 변환 합니다. 이러한 상수는 각 기본 형식에만 적용 되며 모든 그리기 호출에 대해 업데이트 됩니다. 이전에는 렌더링 __프레임 워크 I: 렌더링 소개__의 [기본 렌더링](tutorial--assembling-the-rendering-pipeline.md#primitive-rendering)아래에서 설명 했습니다.
* 기본 형식에 대 한 질감을 포함 하는 셰이더 리소스 개체는이 클래스에도 정의 되어 있습니다.
    * 일부 질감이 미리 정의 되어 있습니다.[DDS](/windows/desktop/direct3ddds/dx-graphics-dds-pguide) 는 압축 된 텍스처와 압축 되지 않은 질감을 저장 하는 데 사용할 수 있는 파일 형식입니다. DDS 질감은 전 세계의 옆면과 밑면에 사용 됩니다.
    * 이 샘플 게임에서 셰이더 리소스 개체는 __m \_ sphereTexture__, __m \_ cylinderTexture__, __m \_ ceilingTexture__, __m \_ floorTexture__, __m \_ wallsTexture__입니다.
* 셰이더 개체는이 클래스에 정의 되어 기본 형식 및 질감을 계산 합니다. 
    * 이 샘플 게임에서 셰이더 개체는 __m \_ vertexShader__, __m \_ vertexShaderFlat__및 __m \_ shadereffect__, __m \_ pixelShaderFlat__입니다.
    * 꼭 짓 점 셰이더는 기본 조명 및 기본 조명을 처리 하 고 픽셀 셰이더 (조각 셰이더 라고도 함)는 질감 및 픽셀 별 효과를 처리 합니다.
    * 서로 다른 기본 형식을 렌더링 하기 위한 두 가지 버전의 셰이더 (일반 및 플랫)가 있습니다. 서로 다른 버전의 이유는 플랫 버전이 훨씬 간단 하 고 반사 하이라이트 또는 픽셀 별 조명 효과를 수행 하지 않기 때문입니다. 이는 벽에 사용 되며, 더 낮은 전원 장치에서 렌더링을 더 빠르게 만듭니다.

## <a name="gamerendererh"></a>GameRenderer

이제 샘플 게임의 렌더러 클래스 개체에서 코드를 살펴보겠습니다.

```cppwinrt
// Class handling the rendering of the game
class GameRenderer : public std::enable_shared_from_this<GameRenderer>
{
public:
    GameRenderer(std::shared_ptr<DX::DeviceResources> const& deviceResources);

    void CreateDeviceDependentResources();
    void CreateWindowSizeDependentResources();
    void ReleaseDeviceDependentResources();
    void Render();
    // --- end of async related methods section

    winrt::Windows::Foundation::IAsyncAction CreateGameDeviceResourcesAsync(_In_ std::shared_ptr<Simple3DGame> game);
    void FinalizeCreateGameDeviceResources();
    winrt::Windows::Foundation::IAsyncAction LoadLevelResourcesAsync();
    void FinalizeLoadLevelResources();

    Simple3DGameDX::IGameUIControl* GameUIControl() { return &m_gameInfoOverlay; };

    DirectX::XMFLOAT2 GameInfoOverlayUpperLeft()
    {
        return DirectX::XMFLOAT2(m_gameInfoOverlayRect.left, m_gameInfoOverlayRect.top);
    };
    DirectX::XMFLOAT2 GameInfoOverlayLowerRight()
    {
        return DirectX::XMFLOAT2(m_gameInfoOverlayRect.right, m_gameInfoOverlayRect.bottom);
    };
    bool GameInfoOverlayVisible() { return m_gameInfoOverlay.Visible(); }
    // --- end of rendering overlay section
...
private:
    // Cached pointer to device resources.
    std::shared_ptr<DX::DeviceResources>        m_deviceResources;

    ...

    // Shader resource objects
    winrt::com_ptr<ID3D11ShaderResourceView>    m_sphereTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_cylinderTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_ceilingTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_floorTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_wallsTexture;

    // Constant buffers
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferNeverChanges;
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferChangeOnResize;
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferChangesEveryFrame;
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferChangesEveryPrim;

    // Texture sampler
    winrt::com_ptr<ID3D11SamplerState>          m_samplerLinear;

    // Shader objects: Vertex shaders and pixel shaders
    winrt::com_ptr<ID3D11VertexShader>          m_vertexShader;
    winrt::com_ptr<ID3D11VertexShader>          m_vertexShaderFlat;
    winrt::com_ptr<ID3D11PixelShader>           m_pixelShader;
    winrt::com_ptr<ID3D11PixelShader>           m_pixelShaderFlat;
    winrt::com_ptr<ID3D11InputLayout>           m_vertexLayout;
};
```

## <a name="constructor"></a>생성자

이제 샘플 게임의 __GameRenderer__ 생성자를 살펴보고 DirectX 11 앱 템플릿에서 제공 하는 __Sample3DSceneRenderer__ 생성자와 비교 하겠습니다.

```cppwinrt
// Constructor method of the main rendering class object
GameRenderer::GameRenderer(std::shared_ptr<DX::DeviceResources> const& deviceResources) : ...
    m_gameInfoOverlay(deviceResources),
    m_gameHud(deviceResources, L"Windows platform samples", L"DirectX first-person game sample")
{
    // m_gameInfoOverlay is a GameHud object to render text in the top left corner of the screen.
    // m_gameHud is Game info rendered as an overlay on the top-right corner of the screen,
    // for example hits, shots, and time.

    CreateDeviceDependentResources();
    CreateWindowSizeDependentResources();
}
```

## <a name="create-and-load-directx-graphic-resources"></a>DirectX 그래픽 리소스 만들기 및 로드

샘플 게임 (및 Visual Studio의 __DirectX 11 앱 (유니버설 Windows)__ 템플릿)에서는 __GameRenderer__ 생성자에서 호출 되는 다음 두 메서드를 사용 하 여 게임 리소스를 만들고 로드 합니다.

* [__CreateDeviceDependentResources__](#createdevicedependentresources-method)
* [__CreateWindowSizeDependentResources__](#createwindowsizedependentresource-method)

## <a name="createdevicedependentresources-method"></a>CreateDeviceDependentResources 메서드

DirectX 11 앱 템플릿에서는이 메서드를 사용 하 여 꼭 짓 점 및 픽셀 셰이더를 비동기적으로 로드 하 고, 셰이더 및 상수 버퍼를 만들고, 위치 및 색 정보를 포함 하는 꼭 짓 점을 사용 하 여 메시를 만듭니다. 

샘플 게임에서 장면 개체의 이러한 작업은 대신 [__CreateGameDeviceResourcesAsync__](#creategamedeviceresourcesasync-method) 및 [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) 메서드 간에 분할 됩니다. 

이 샘플 게임의 경우이 방법이 어떻게 되나요?

* 비동기적으로 로드 하는 중 이므로 렌더링으로 이동 하기 전에 리소스를 로드 했는지 여부를 나타내는 인스턴스화된 변수 (__m \_ gameResourcesLoaded__ = false 및 __m \_ levelresourcesloaded__ = false)입니다. 
* HUD 및 오버레이 렌더링은 별도의 클래스 개체에 있으므로 여기서 __GameHud:: CreateDeviceDependentResources__ 및 __GameInfoOverlay:: CreateDeviceDependentResources__ 메서드를 호출 합니다.

__GameRenderer:: CreateDeviceDependentResources__에 대 한 코드는 다음과 같습니다.

```cppwinrt
// This method is called in GameRenderer constructor when it's created in GameMain constructor.
void GameRenderer::CreateDeviceDependentResources()
{
    // instantiate variables that indicate whether resources were loaded.
    m_gameResourcesLoaded = false;
    m_levelResourcesLoaded = false;

    // game HUD and overlay are design as separate class objects.
    m_gameHud.CreateDeviceDependentResources();
    m_gameInfoOverlay.CreateDeviceDependentResources();
}
```

다음은 리소스를 만들고 로드 하는 데 사용 되는 방법의 목록입니다.

- CreateDeviceDependentResources
  - CreateGameDeviceResourcesAsync (추가 됨)
  - FinalizeCreateGameDeviceResources (추가 됨)
- CreateWindowSizeDependentResources

리소스를 만들고 로드 하는 데 사용 되는 다른 방법을 살펴보기 전에 먼저 렌더러를 만들어 게임 루프에 맞추는 방법을 알아보겠습니다.

## <a name="create-the-renderer"></a>렌더러 만들기

__GameRenderer__ 는 __GameMain__의 생성자에 생성 됩니다. 또한 리소스를 만들고 로드 하는 데 도움이 되도록 추가 된 [__CreateGameDeviceResourcesAsync__](#creategamedeviceresourcesasync-method) 및 [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) 라는 두 가지 메서드를 호출 합니다.

```cppwinrt
GameMain::GameMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) : ...
{
    m_deviceResources->RegisterDeviceNotify(this);

    // Creation of GameRenderer
    m_renderer = std::make_shared<GameRenderer>(m_deviceResources);

    ...

    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    ...

    // Asynchronously initialize the game class and load the renderer device resources.
    // By doing all this asynchronously, the game gets to its main loop more quickly
    // and in parallel all the necessary resources are loaded on other threads.
    m_game->Initialize(m_controller, m_renderer);

    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);

    // The finalize code needs to run in the same thread context
    // as the m_renderer object was created because the D3D device context
    // can ONLY be accessed on a single thread.
    // co_await of an IAsyncAction resumes in the same thread context.
    m_renderer->FinalizeCreateGameDeviceResources();

    InitializeGameState();

    ...
}
```

## <a name="creategamedeviceresourcesasync-method"></a>CreateGameDeviceResourcesAsync 메서드

__CreateGameDeviceResourcesAsync__ 는 게임 리소스를 비동기적으로 로드 하므로 __create \_ task__ 루프의 __GameMain__ constructor 메서드에서 호출 됩니다.
        
__CreateDeviceResourcesAsync__ 는 게임 리소스를 로드 하는 별도의 비동기 작업 집합으로 실행 되는 메서드입니다. 별도의 스레드에서 실행 될 것으로 예상 되기 때문에 __ID3D11Device__에 정의 된 Direct3D 11 장치 메서드 ( __ID3D11DeviceContext__에 정의 된 메서드)에만 액세스할 수 있으므로 렌더링을 수행 하지 않습니다.

__FinalizeCreateGameDeviceResources__ 메서드는 주 스레드에서 실행 되 고 Direct3D 11 장치 컨텍스트 메서드에 액세스할 수 있습니다.

원칙:
* __CreateGameDeviceResourcesAsync__ 에는 __ID3D11Device__ 메서드만 사용 합니다 .이 메서드는 자유 스레드 이므로 모든 스레드에서 실행할 수 있습니다. 또한 하나의 __GameRenderer__ 이 생성 된 스레드와 동일한 스레드에서 실행 되지 않습니다. 
* __ID3D11DeviceContext__ 에서 메서드를 사용 하지 마세요 .이 메서드는 단일 스레드와 __GameRenderer__와 동일한 스레드에서 실행 되어야 하기 때문입니다.
* 상수 버퍼를 만들려면이 메서드를 사용 합니다.
* 이 메서드를 사용 하 여 질감 (예: .dfiles) 및 셰이더 정보 (예: .csfilefiles)를 셰이더에 로드 [합니다.](tutorial--assembling-the-rendering-pipeline.md#shaders)

이 메서드는 다음을 수행 하는 데 사용 됩니다.
* 4 개의 [상수 버퍼](tutorial--assembling-the-rendering-pipeline.md#buffer)만들기: __m \_ constantBufferNeverChanges__, __m \_ constantBufferChangeOnResize__, __m \_ constantBufferChangesEveryFrame__, __m \_ constantBufferChangesEveryPrim__
* 질감에 대 한 샘플링 정보를 캡슐화 하는 [샘플러 상태](tutorial--assembling-the-rendering-pipeline.md#sampler-state) 개체 만들기
* 메서드로 만들어진 모든 비동기 작업을 포함 하는 작업 그룹을 만듭니다. 이러한 비동기 태스크가 모두 완료 될 때까지 대기한 다음 __FinalizeCreateGameDeviceResources__를 호출 합니다.
* [기본 로더](tutorial--assembling-the-rendering-pipeline.md#the-basicloader-class)를 사용 하 여 로더를 만듭니다. 이전에 만든 작업 그룹에 작업으로 로더 비동기 로드 작업을 추가 합니다.
* __Basicloader:: LoadShaderAsync__ 및 __Basicloader:: LoadTextureAsync__ 와 같은 메서드를 사용 하 여 로드 합니다.
    * 컴파일된 셰이더 개체 (VertextShader, VertexShaderFlat, Shadereffect, PixelShaderFlat 등)를 말합니다. 자세한 내용은 [다양 한 셰이더 파일 형식](tutorial--assembling-the-rendering-pipeline.md#various-shader-file-formats)을 참조 하세요.
    * game 특정 질감 ( \\ seafloor, metal_texture, 셀, 셀, 셀, 셀, 셀, 셀, 셀 및 셀).

```cppwinrt
IAsyncAction GameRenderer::CreateGameDeviceResourcesAsync(_In_ std::shared_ptr<Simple3DGame> game)
{
    auto lifetime = shared_from_this();

    // Create the device dependent game resources.
    // Only the d3dDevice is used in this method. It is expected
    // to not run on the same thread as the GameRenderer was created.
    // Create methods on the d3dDevice are free-threaded and are safe while any methods
    // in the d3dContext should only be used on a single thread and handled
    // in the FinalizeCreateGameDeviceResources method.
    m_game = game;

    auto d3dDevice = m_deviceResources->GetD3DDevice();

    // Define D3D11_BUFFER_DESC. See
    // https://docs.microsoft.com/windows/win32/api/d3d11/ns-d3d11-d3d11_buffer_desc
    D3D11_BUFFER_DESC bd;
    ZeroMemory(&bd, sizeof(bd));

    // Create the constant buffers.
    bd.Usage = D3D11_USAGE_DEFAULT;
    ...

    // Create the constant buffers: m_constantBufferNeverChanges, m_constantBufferChangeOnResize,
    // m_constantBufferChangesEveryFrame, m_constantBufferChangesEveryPrim
    // CreateBuffer is used to create one of these buffers: vertex buffer, index buffer, or 
    // shader-constant buffer. For CreateBuffer API ref info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11device-createbuffer.
    winrt::check_hresult(
        d3dDevice->CreateBuffer(&bd, nullptr, m_constantBufferNeverChanges.put())
        );

    ...

    // Define D3D11_SAMPLER_DESC. For API ref, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/ns-d3d11-d3d11_sampler_desc.
    D3D11_SAMPLER_DESC sampDesc;

    // ZeroMemory fills a block of memory with zeros. For API ref, see
    // https://docs.microsoft.com/previous-versions/windows/desktop/legacy/aa366920(v=vs.85).
    ZeroMemory(&sampDesc, sizeof(sampDesc));

    sampDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;
    sampDesc.AddressU = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.AddressV = D3D11_TEXTURE_ADDRESS_WRAP;
    ...

    // Create a sampler-state object that encapsulates sampling information for a texture.
    // The sampler-state interface holds a description for sampler state that you can bind to any 
    // shader stage of the pipeline for reference by texture sample operations.
    winrt::check_hresult(
        d3dDevice->CreateSamplerState(&sampDesc, m_samplerLinear.put())
        );

    // Start the async tasks to load the shaders and textures.

    // Load compiled shader objects (VertextShader.cso, VertexShaderFlat.cso, PixelShader.cso, and PixelShaderFlat.cso).
    // The BasicLoader class is used to convert and load common graphics resources, such as meshes, textures, 
    // and various shader objects into the constant buffers. For more info, see
    // https://docs.microsoft.com/windows/uwp/gaming/complete-code-for-basicloader.
    BasicLoader loader{ d3dDevice };

    std::vector<IAsyncAction> tasks;

    uint32_t numElements = ARRAYSIZE(PNTVertexLayout);

    // Load shaders asynchronously with the shader and pixel data using the
    // BasicLoader::LoadShaderAsync method. Push these method calls into a list of tasks.
    tasks.push_back(loader.LoadShaderAsync(L"VertexShader.cso", PNTVertexLayout, numElements, m_vertexShader.put(), m_vertexLayout.put()));
    tasks.push_back(loader.LoadShaderAsync(L"VertexShaderFlat.cso", nullptr, numElements, m_vertexShaderFlat.put(), nullptr));
    tasks.push_back(loader.LoadShaderAsync(L"PixelShader.cso", m_pixelShader.put()));
    tasks.push_back(loader.LoadShaderAsync(L"PixelShaderFlat.cso", m_pixelShaderFlat.put()));

    // Make sure the previous versions if any of the textures are released.
    m_sphereTexture = nullptr;
    ...

    // Load Game specific textures (Assets\\seafloor.dds, metal_texture.dds, cellceiling.dds,
    // cellfloor.dds, cellwall.dds).
    // Push these method calls also into a list of tasks.
    tasks.push_back(loader.LoadTextureAsync(L"Assets\\seafloor.dds", nullptr, m_sphereTexture.put()));
    ...

    // Simulate loading additional resources by introducing a delay.
    tasks.push_back([]() -> IAsyncAction { co_await winrt::resume_after(GameConstants::InitialLoadingDelay); }());

    // Returns when all the async tasks for loading the shader and texture assets have completed.
    for (auto&& task : tasks)
    {
        co_await task;
    }
}
```

## <a name="finalizecreategamedeviceresources-method"></a>FinalizeCreateGameDeviceResources 메서드

__FinalizeCreateGameDeviceResources__ 메서드는 __CreateGameDeviceResourcesAsync__ 메서드에 있는 모든 리소스 로드 태스크가 완료 된 후에 호출 됩니다. 

* 광원 위치 및 색을 사용 하 여 constantBufferNeverChanges를 초기화 합니다. __ID3D11DeviceContext:: UpdateSubresource__에 대 한 장치 컨텍스트 메서드 호출을 사용 하 여 초기 데이터를 상수 버퍼로 로드 합니다.
* 비동기적으로 로드 된 리소스는 로드를 완료 하므로 적절 한 게임 개체와 연결 해야 합니다.
* 각 게임 개체에 대해 로드 된 질감을 사용 하 여 메시와 재질을 만듭니다. 그런 다음 메시와 재질을 게임 개체에 연결 합니다.
* 대상 게임 개체의 경우, 위쪽에 숫자 값이 있는 동심 색 링으로 구성 된 질감이 질감 파일에서 로드 되지 않습니다. 대신 __targettexture__의 코드를 사용 하 여 생성 됩니다. __Targettexture__ 클래스는 초기화 시에 질감을 off 화면 리소스로 그리는 데 필요한 리소스를 만듭니다. 그러면 결과 질감이 적절 한 대상 게임 개체와 연결 됩니다.

__FinalizeCreateGameDeviceResources__ 및 [__CreateWindowSizeDependentResources__](#createwindowsizedependentresource-method) 는 다음과 같은 코드 부분을 공유 합니다.
* __SetProjParams__ 를 사용 하 여 카메라에 올바른 투영 매트릭스가 있는지 확인 합니다. 자세한 내용을 보려면 [카메라 및 좌표 공간](tutorial--assembling-the-rendering-pipeline.md#camera-and-coordinate-space)으로 이동 하세요.
* 3D 회전 매트릭스를 카메라의 프로젝션 매트릭스와 곱하여 화면 회전을 처리 합니다. 그런 다음 결과 프로젝션 매트릭스를 사용 하 여 __ConstantBufferChangeOnResize__ 상수 버퍼를 업데이트 합니다.
* 이제 리소스가 버퍼에 로드 되었음을 나타내도록 __m \_ gameResourcesLoaded__ __부울__ 전역 변수를 설정 하 여 다음 단계를 준비 합니다. __GameRenderer:: CreateDeviceDependentResources__ 메서드를 통해 __GameRenderer__의 생성자 메서드에서이 변수를 __FALSE__ 로 처음 초기화 했습니다. 
* 이 __m \_ GameResourcesLoaded__ __TRUE 이면__장면 개체의 렌더링이 수행 될 수 있습니다. 이에 대해서는 __렌더링 프레임 워크 I: 렌더링 소개__ 문서 [__GameRenderer:: Render 메서드__](tutorial--assembling-the-rendering-pipeline.md#gamerendererrender-method)아래에 설명 되어 있습니다.

```cppwinrt
// This method is called from the GameMain constructor.
// Make sure that 2D rendering is occurring on the same thread as the main rendering.
void GameRenderer::FinalizeCreateGameDeviceResources()
{
    // All asynchronously loaded resources have completed loading.
    // Now associate all the resources with the appropriate game objects.
    // This method is expected to run in the same thread as the GameRenderer
    // was created. All work will happen behind the "Loading ..." screen after the
    // main loop has been entered.

    // Initialize the Constant buffer with the light positions
    // These are handled here to ensure that the d3dContext is only
    // used in one thread.

    auto d3dDevice = m_deviceResources->GetD3DDevice();

    ConstantBufferNeverChanges constantBufferNeverChanges;
    constantBufferNeverChanges.lightPosition[0] = XMFLOAT4(3.5f, 2.5f, 5.5f, 1.0f);
    ...
    constantBufferNeverChanges.lightColor = XMFLOAT4(0.25f, 0.25f, 0.25f, 1.0f);

    // CPU copies data from memory (constantBufferNeverChanges) to a subresource 
    // created in non-mappable memory (m_constantBufferNeverChanges) which was created in the earlier 
    // CreateGameDeviceResourcesAsync method. For UpdateSubresource API ref info, 
    // go to: https://msdn.microsoft.com/library/windows/desktop/ff476486.aspx
    // To learn more about what a subresource is, go to:
    // https://msdn.microsoft.com/library/windows/desktop/ff476901.aspx

    m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
        m_constantBufferNeverChanges.get(),
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

    TargetTexture textureGenerator(
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

    auto cylinderMesh = std::make_shared<CylinderMesh>(d3dDevice, (uint16_t)26);
    ...

    // The Material class maintains the properties that represent how an object will
    // look when it is rendered.  This includes the color of the object, the
    // texture used to render the object, and the vertex and pixel shader that
    // should be used for rendering.

    auto cylinderMaterial = std::make_shared<Material>(
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(1.0f, 1.0f, 1.0f, 1.0f),
        15.0f,
        m_cylinderTexture.get(),
        m_vertexShader.get(),
        m_pixelShader.get()
        );

    ...

    // Attach the textures to the appropriate game objects.
    // We'll loop through all the objects that need to be rendered.
    for (auto&& object : m_game->RenderObjects())
    {
        if (object->TargetId() == GameConstants::WorldFloorId)
        {
            // Assign a normal material for the floor object.
            // This normal material uses the floor texture (cellfloor.dds) that was loaded asynchronously from
            // the Assets folder using BasicLoader::LoadTextureAsync method in the earlier 
            // CreateGameDeviceResourcesAsync loop

            object->NormalMaterial(
                std::make_shared<Material>(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_floorTexture.get(),
                    m_vertexShaderFlat.get(),
                    m_pixelShaderFlat.get()
                    )
                );
            // Creates a mesh object called WorldFloorMesh and assign it to the floor object.
            object->Mesh(std::make_shared<WorldFloorMesh>(d3dDevice));
        }
        ...
        else if (auto cylinder = dynamic_cast<Cylinder*>(object.get()))
        {
            cylinder->Mesh(cylinderMesh);
            cylinder->NormalMaterial(cylinderMaterial);
        }
        else if (auto target = dynamic_cast<Face*>(object.get()))
        {
            const int bufferLength = 16;
            wchar_t str[bufferLength];
            int len = swprintf_s(str, bufferLength, L"%d", target->TargetId());
            auto string{ winrt::hstring(str, len) };

            winrt::com_ptr<ID3D11ShaderResourceView> texture;
            textureGenerator.CreateTextureResourceView(string, texture.put());
            target->NormalMaterial(
                std::make_shared<Material>(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.get(),
                    m_vertexShader.get(),
                    m_pixelShader.get()
                    )
                );

            texture = nullptr;
            textureGenerator.CreateHitTextureResourceView(string, texture.put());
            target->HitMaterial(
                std::make_shared<Material>(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.get(),
                    m_vertexShader.get(),
                    m_pixelShader.get()
                    )
                );

            target->Mesh(targetMesh);
        }
        ...
    }

    // The SetProjParams method calculates the projection matrix based on input params and
    // ensures that the camera has been initialized with the right projection
    // matrix.  
    // The camera is not created at the time the first window resize event occurs.

    auto renderTargetSize = m_deviceResources->GetRenderTargetSize();
    m_game->GameCamera().SetProjParams(
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
            XMMatrixTranspose(m_game->GameCamera().Projection()),
            XMMatrixTranspose(XMLoadFloat4x4(&orientation))
            )
        );

    // UpdateSubresource method instructs CPU to copy data from memory (changesOnResize) to a subresource 
    // created in non-mappable memory (m_constantBufferChangeOnResize ) which was created in the earlier 
    // CreateGameDeviceResourcesAsync method.

    m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
        m_constantBufferChangeOnResize.get(),
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

CreateWindowSizeDependentResources 메서드는 창 크기, 방향, 스테레오 사용 렌더링 또는 해상도가 변경 될 때마다 호출 됩니다. 샘플 게임에서는 __ConstantBufferChangeOnResize__의 프로젝션 매트릭스를 업데이트 합니다.

창 크기 리소스는 다음과 같은 방식으로 업데이트 됩니다. 
* 앱 프레임 워크는 창 상태 변경을 나타내는 몇 가지 가능한 이벤트 중 하나를 가져옵니다. 
* 그런 다음 주 게임 루프는 이벤트에 대해 알리고__GameMain__(주 클래스) 인스턴스에서 __CreateWindowSizeDependentResources__ 를 호출 합니다. 그러면이 인스턴스는 game 렌더러 (__GameRenderer__) 클래스에서 __CreateWindowSizeDependentResources__ 구현을 호출 합니다.
* 이 메서드의 주요 작업은 창 속성의 변경으로 인해 시각적 개체가 혼동 되거나 잘못 되지 않도록 하는 것입니다.

이 샘플 게임의 경우 다양 한 메서드 호출이 [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) 메서드와 동일 합니다. 코드 연습을 보려면 이전 섹션으로 이동 하세요.

게임 HUD 및 오버레이 창 크기 렌더링 조정은 [사용자 인터페이스 추가](tutorial--adding-a-user-interface.md)에서 다룹니다.

```cppwinrt
// Initializes view parameters when the window size changes.
void GameRenderer::CreateWindowSizeDependentResources()
{
    // Game HUD and overlay window size rendering adjustments are done here
    // but they'll be covered in the UI section instead.

    m_gameHud.CreateWindowSizeDependentResources();

    ...

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    // In Sample3DSceneRenderer::CreateWindowSizeDependentResources, we had:
    // Size outputSize = m_deviceResources->GetOutputSize();

    auto renderTargetSize = m_deviceResources->GetRenderTargetSize();

    ...

    m_gameInfoOverlay.CreateWindowSizeDependentResources(m_gameInfoOverlaySize);

    if (m_game != nullptr)
    {
        // Similar operations as the last section of FinalizeCreateGameDeviceResources method
        m_game->GameCamera().SetProjParams(
            XM_PI / 2, renderTargetSize.Width / renderTargetSize.Height,
            0.01f,
            100.0f
            );

        XMFLOAT4X4 orientation = m_deviceResources->GetOrientationTransform3D();

        ConstantBufferChangeOnResize changesOnResize;
        XMStoreFloat4x4(
            &changesOnResize.projection,
            XMMatrixMultiply(
                XMMatrixTranspose(m_game->GameCamera().Projection()),
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
}
```

## <a name="next-steps"></a>다음 단계

게임의 그래픽 렌더링 프레임 워크를 구현 하는 기본 프로세스입니다. 게임 크기가 클수록 개체 형식 및 애니메이션 동작의 계층 구조를 처리 하는 데 더 많은 추상화를 적용 해야 합니다. 메시, 질감 등의 자산을 로드 하 고 관리 하는 더 복잡 한 메서드를 구현 해야 합니다. 다음으로 [사용자 인터페이스를 추가](tutorial--adding-a-user-interface.md)하는 방법에 대해 알아보겠습니다.