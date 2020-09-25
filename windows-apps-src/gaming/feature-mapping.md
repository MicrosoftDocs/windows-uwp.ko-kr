---
title: Directx 11 기능을 DirectX 11 Api에 매핑
description: Direct3D 9 게임에서 사용 하는 기능이 Direct3D 11 및 유니버설 Windows 플랫폼 (UWP)로 변환 되는 방식을 이해 합니다.
ms.assetid: 3aa8a114-4e47-ae0a-9447-88ba324377b8
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx 9, directx 11, 포팅
ms.localizationpriority: medium
ms.openlocfilehash: febfeb87f383855b0e92525fd4e2792159d0240f
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91217986"
---
# <a name="map-directx-9-features-to-directx-11-apis"></a>Directx 11 기능을 DirectX 11 Api에 매핑

Direct3D 9 게임에서 사용 하는 기능이 Direct3D 11 및 유니버설 Windows 플랫폼 (UWP)로 변환 되는 방식을 이해 합니다.

또한 [DirectX 포트 계획](plan-your-directx-port.md)및 [Direct3D 9에서 Direct3d 11로 중요 한 변경 사항](understand-direct3d-11-1-concepts.md)을 참조 하세요.

## <a name="mapping-direct3d-9-to-directx-11-apis"></a>Direct3D 9를 DirectX 11 Api에 매핑

[Direct3D](/windows/desktop/direct3d) 는 아직 directx 그래픽의 기반 이지만 directx 9 이후에는 API가 변경 되었습니다.

-   Microsoft DXGI (DirectX Graphics Infrastructure)는 그래픽 어댑터를 설정 하는 데 사용 됩니다. [DXGI](/windows/desktop/direct3ddxgi/dx-graphics-dxgi) 를 사용 하 여 버퍼 형식을 선택 하 고, 스왑 체인을 만들고, 프레임을 표시 하 고, 공유 리소스를 만듭니다. [DXGI 개요](/windows/desktop/direct3ddxgi/d3d10-graphics-programming-guide-dxgi)를 참조 하세요.
-   Direct3D 장치 컨텍스트는 파이프라인 상태를 설정 하 고 렌더링 명령을 생성 하는 데 사용 됩니다. 대부분의 샘플은 직접 컨텍스트를 사용 하 여 장치에 직접 렌더링 합니다. Direct3D 11은 지연 컨텍스트가 사용 되는 다중 스레드 렌더링도 지원 합니다. [Direct3D 11의 장치 소개를](/windows/desktop/direct3d11/overviews-direct3d-11-devices-intro)참조 하세요.
-   일부 기능은 더 이상 사용 되지 않습니다. 특히 고정 함수 파이프라인입니다. [사용 되지 않는 기능](/windows/desktop/direct3d10/d3d10-graphics-programming-guide-api-features-deprecated)을 참조 하세요.

Direct3D 11 기능의 전체 목록은 [direct3d 11 기능](/windows/desktop/direct3d11/direct3d-11-features) 및 [direct3d 11 기능](/windows/desktop/direct3d11/direct3d-11-1-features)을 참조 하세요.

## <a name="moving-from-direct2d-9-to-direct2d-11"></a>Direct2D 9에서 Direct2D 11로 이동

[Direct2D (Windows)](/windows/desktop/Direct2D/direct2d-portal) 는 여전히 DirectX 그래픽 및 windows의 중요 한 부분입니다. 여전히 Direct2D를 사용 하 여 2D 게임을 그리고 Direct3D 위에 오버레이 (HUDs)를 그릴 수 있습니다.

Direct2D는 Direct3D 위에서 실행 됩니다. 두 API를 사용 하 여 2D 게임을 구현할 수 있습니다. 예를 들어 Direct3D를 사용 하 여 구현 된 2D 게임에서는 직교 프로젝션을 사용 하 고, Z 값을 설정 하 여 기본 형식의 그리기 순서를 제어 하 고, 픽셀 셰이더를 사용 하 여 특수 효과를 추가할 수 있습니다.

Direct2D는 Direct3D를 기반으로 하므로 DXGI 및 장치 컨텍스트를 사용 하기도 합니다. [DIRECT2D API 개요](/windows/desktop/Direct2D/the-direct2d-api)를 참조 하세요.

[DirectWrite](/windows/desktop/DirectWrite/direct-write-portal) API는 Direct2D를 사용 하 여 서식 있는 텍스트에 대 한 지원을 추가 합니다. [DirectWrite 소개](/windows/desktop/DirectWrite/introducing-directwrite)를 참조 하세요.

## <a name="replace-deprecated-helper-libraries"></a>사용 되지 않는 도우미 라이브러리 바꾸기

D3DX 및 DXUT는 더 이상 사용 되지 않으며 UWP 게임에서 사용할 수 없습니다. 이러한 도우미 라이브러리는 질감 로드 및 메시 로드와 같은 작업에 대 한 리소스를 제공 합니다.

-   [Direct3d 9에서 UWP로의 단순 포트](walkthrough--simple-port-from-direct3d-9-to-11-1.md) 연습에서는 창을 설정 하 고, Direct3D를 초기화 하 고, 기본 3d 렌더링을 수행 하는 방법을 보여 줍니다.
-   [DirectX를 사용한 간단한 UWP 게임](tutorial--create-your-first-uwp-directx-game.md) 연습에서는 그래픽, 파일 로드, UI, 컨트롤 및 소리를 비롯 한 일반적인 게임 프로그래밍 작업을 보여 줍니다.
-   [DirectX Tool Kit](https://github.com/Microsoft/DirectXTK) community 프로젝트는 Direct3D 11 및 UWP 앱에서 사용할 도우미 클래스를 제공 합니다.

## <a name="move-shader-programs-from-fx-to-hlsl"></a>FX에서 HLSL로 셰이더 프로그램 이동

효과를 비롯 한 D3DX 유틸리티 라이브러리 (D3DX 9, D3DX 10 및 D3DX 11)는 UWP에서 사용 되지 않습니다. UWP 용 DirectX 게임은 모두 효과 없이 [HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl) 를 사용 하 여 그래픽 파이프라인을 구동 합니다.

Visual Studio는 여전히 FXC.EXE를 사용 하 여 셰이더 개체를 컴파일합니다. UWP 게임 셰이더가 미리 컴파일됩니다. 바이트 코드는 런타임에 로드 되며, 각 셰이더 리소스는 적절 한 렌더링 패스 중에 그래픽 파이프라인에 바인딩됩니다. 셰이더는 별개의 자체로 이동 해야 합니다. HLSL 파일 및 렌더링 기술은 c + + 코드에서 구현 해야 합니다.

셰이더 리소스를 로드 하는 방법에 대 한 간략 한 보기는 [Direct3D 9에서 UWP로의 단순 포트를](walkthrough--simple-port-from-direct3d-9-to-11-1.md)참조 하세요.

Direct3D 11에는 Direct3D 기능 수준 11 0 이상이 필요한 셰이더 모델 5가 도입 \_ 되었습니다. [Direct3D 11에 대 한 HLSL 셰이더 모델 5 기능을](/windows/desktop/direct3dhlsl/overviews-direct3d-11-hlsl)참조 하세요.

## <a name="replace-xnamath-and-d3dxmath"></a>XNAMath 및 D3DXMath 바꾸기

XNAMath (또는 D3DXMath)를 사용 하는 코드는 [Directxmath](/windows/desktop/dxmath/directxmath-portal)로 마이그레이션해야 합니다. DirectXMath에는 x86, x64 및 ARM에서 이식 가능한 형식이 포함 되어 있습니다. [XNA Math Library에서 코드 마이그레이션을](/windows/desktop/dxmath/pg-xnamath-migration)참조 하세요.

DirectXMath float 형식은 셰이더를 사용 하는 데 편리 합니다. 예를 들어 [**XMFLOAT4**](/windows/desktop/api/directxmath/ns-directxmath-xmfloat4) 및 [**XMFLOAT4X4**](/windows/desktop/api/directxmath/ns-directxmath-xmfloat4x4) 는 상수 버퍼에 대 한 데이터를 편리 하 게 맞춥니다.

## <a name="replace-directsound-with-xaudio2-and-background-audio"></a>DirectSound를 XAudio2 (및 배경 오디오)로 바꿉니다.

UWP에 대 한 DirectSound는 지원 되지 않습니다.

-   [XAudio2](/windows/desktop/xaudio2/xaudio2-apis-portal) 를 사용 하 여 게임에 음향 효과를 추가 합니다.

##  <a name="replace-directinput-with-xinput-and-windows-runtime-apis"></a>DirectInput을 XInput 및 Windows 런타임 Api로 바꾸기

DirectInput는 UWP에 대해 지원 되지 않습니다.

-   마우스, 키보드 및 터치 입력에 대해 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 입력 이벤트 콜백을 사용 합니다.
-   게임 컨트롤러 지원 (및 게임 컨트롤러 헤드셋 지원)에 [XInput](/windows/desktop/xinput/getting-started-with-xinput) 1.4를 사용 합니다. 데스크톱 및 UWP에 대 한 공유 코드 베이스를 사용 하는 경우 이전 버전과의 호환성에 대 한 자세한 내용은 [XInput 버전](/windows/desktop/xinput/xinput-versions) 을 참조 하세요.
-   게임에서 앱 바를 사용 해야 하는 경우 [**EdgeGesture**](/uwp/api/Windows.UI.Input.EdgeGesture) 이벤트를 등록 합니다.

## <a name="use-microsoft-media-foundation-instead-of-directshow"></a>DirectShow 대신 Microsoft 미디어 파운데이션 사용

DirectShow는 더 이상 DirectX API (또는 Windows API)의 일부가 아닙니다. [Microsoft 미디어 파운데이션](/windows/desktop/medfound/microsoft-media-foundation-sdk) 공유 화면을 사용 하 여 Direct3D에 비디오 콘텐츠를 제공 합니다. [Direct3D 11 Video api](/windows/desktop/medfound/direct3d-11-video-apis)를 참조 하세요.

## <a name="replace-directplay-with-networking-code"></a>DirectPlay를 네트워킹 코드로 바꾸기

Microsoft DirectPlay는 더 이상 사용 되지 않습니다. 게임에서 네트워크 서비스를 사용 하는 경우 UWP 요구 사항을 준수 하는 네트워킹 코드를 제공 해야 합니다. 다음 Api를 사용 합니다.

-   [UWP 앱 용 Win32 및 COM (네트워킹) (Windows)](/uwp/win32-and-com/win32-and-com-for-uwp-apps)
-   [**Windows. 네트워킹 네임 스페이스 (Windows)**](/uwp/api/Windows.Networking)
-   [**Windows. 네트워킹용 네임 스페이스 (Windows)**](/uwp/api/Windows.Networking.Sockets)
-   [**Windows. 연결 네임 스페이스 (Windows)**](/uwp/api/Windows.Networking.Connectivity)
-   [**Windows ApplicationModel. Background 네임 스페이스 (Windows)**](/uwp/api/Windows.ApplicationModel.Background)

다음 문서를 참조 하 여 네트워킹 기능을 추가 하 고, 앱의 패키지 매니페스트에 네트워킹 지원을 선언할 수 있습니다.

-   [소켓을 사용 하 여 연결 (c #/VB/C + + 및 XAML을 사용 하는 UWP 앱) (Windows)](/previous-versions/windows/apps/hh452976(v=win.10))
-   [Websocket을 사용 하 여 연결 (c #/VB/C + + 및 XAML을 사용 하는 UWP 앱) (Windows)](/previous-versions/windows/apps/hh994396(v=win.10))
-   [웹 서비스 (c #/VB/C + + 및 XAML을 사용 하는 UWP 앱)에 연결 (Windows)](/previous-versions/windows/apps/hh761504(v=win.10))
-   [네트워킹 기본 사항](../networking/networking-basics.md)

모든 UWP 앱 (게임 포함)은 특정 유형의 백그라운드 작업을 사용 하 여 앱이 일시 중단 된 동안 연결을 유지 합니다. 일시 중단 된 동안 게임에서 연결 상태를 유지 해야 하는 경우 [네트워킹 기본 사항](../networking/networking-basics.md)을 참조 하세요.

## <a name="function-mapping"></a>함수 매핑

다음 표를 사용 하 여 Direct3D 9에서 Direct3D 11로 코드를 변환할 수 있습니다. 또한 장치와 디바이스 컨텍스트를 구별하는 데에도 도움이 될 수 있습니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Direct3D9</th>
<th align="left">Direct3D 11 동급</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3ddevice9">IDirect3DDevice9</a></p></td>
<td align="left"><p><a href="/windows/desktop/api/d3d11_2/nn-d3d11_2-id3d11device2">ID3D11Device2</a></p>
<p><a href="/windows/desktop/api/d3d11_2/nn-d3d11_2-id3d11devicecontext2">ID3D11DeviceContext2</a></p>
<p>그래픽 파이프라인 단계는 <a href="/windows/desktop/direct3d11/overviews-direct3d-11-graphics-pipeline">그래픽 파이프라인</a>에 설명 되어 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3d9">IDirect3D9</a></p></td>
<td align="left"><p><a href="/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2">IDXGIFactory2</a></p>
<p><a href="/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiadapter2">IDXGIAdapter2</a></p>
<p><a href="/windows/desktop/api/dxgi1_3/nn-dxgi1_3-idxgidevice3">IDXGIDevice3</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-present">IDirect3DDevice9::P 다시 보낸</a></p></td>
<td align="left"><p><a href="/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1">IDXGISwapChain1::Present1</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-testcooperativelevel">IDirect3DDevice9::TestCooperativeLevel</a></p></td>
<td align="left"><p><a href="/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1">IDXGISwapChain1::P resent1</a> DXGI_PRESENT_TEST 플래그가 설정 된 상태에서 호출 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dbasetexture9">IDirect3DBaseTexture9</a></p>
<p><a href="/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dtexture9">IDirect3DTexture9</a></p>
<p><a href="/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dcubetexture9">IDirect3DCubeTexture9</a></p>
<p><a href="/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvolumetexture9">IDirect3DVolumeTexture9</a></p>
<p><a href="/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dindexbuffer9">IDirect3DIndexBuffer9</a></p>
<p><a href="/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvertexbuffer9">IDirect3DVertexBuffer9</a></p></td>
<td align="left"><p><a href="/windows/desktop/api/d3d11/nn-d3d11-id3d11buffer">ID3D11Buffer</a></p>
<p><a href="/windows/desktop/api/d3d11/nn-d3d11-id3d11texture1d">ID3D11Texture1D</a></p>
<p><a href="/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d">ID3D11Texture2D</a></p>
<p><a href="/windows/desktop/api/d3d11/nn-d3d11-id3d11texture3d">ID3D11Texture3D</a></p>
<p><a href="/windows/desktop/api/d3d11/nn-d3d11-id3d11shaderresourceview">ID3D11ShaderResourceView</a></p>
<p><a href="/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview">ID3D11RenderTargetView</a></p>
<p><a href="/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilview">ID3D11DepthStencilView</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvertexshader9">IDirect3DVertexShader9</a></p>
<p><a href="/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dpixelshader9">IDirect3DPixelShader9</a></p></td>
<td align="left"><p><a href="/windows/desktop/api/d3d11/nn-d3d11-id3d11vertexshader">ID3D11VertexShader</a></p>
<p><a href="/windows/desktop/api/d3d11/nn-d3d11-id3d11pixelshader">ID3D11PixelShader</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvertexdeclaration9">IDirect3DVertexDeclaration9</a></p></td>
<td align="left"><p><a href="/windows/desktop/api/d3d11/nn-d3d11-id3d11inputlayout">ID3D11InputLayout</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/direct3d9/id3dxeffectstatemanager--setrenderstate">IDirect3DDevice9::SetRenderState</a></p>
<p><a href="/windows/desktop/direct3d9/id3dxeffectstatemanager--setsamplerstate">IDirect3DDevice9:: SetSamplerState</a></p></td>
<td align="left"><p><a href="/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11blendstate1">ID3D11BlendState1</a></p>
<p><a href="/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilstate">ID3D11DepthStencilState</a></p>
<p><a href="/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11rasterizerstate1">ID3D11RasterizerState1</a></p>
<p><a href="/windows/desktop/api/d3d11/nn-d3d11-id3d11samplerstate">ID3D11SamplerState</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawindexedprimitive">IDirect3DDevice9::D rawIndexedPrimitive</a></p>
<p><a href="/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawprimitive">IDirect3DDevice9::D rawPrimitive</a></p></td>
<td align="left"><p><a href="/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw">ID3D11DeviceContext: 원시:D</a></p>
<p><a href="/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed">ID3D11DeviceContext::D rawIndexed</a></p>
<p><a href="/windows/desktop/api/d3d10/nf-d3d10-id3d10device-drawindexedinstanced">ID3D11DeviceContext::D rawIndexedInstanced</a></p>
<p><a href="/windows/desktop/api/d3d10/nf-d3d10-id3d10device-drawinstanced">ID3D11DeviceContext::D rawInstanced</a></p>
<p><a href="/windows/desktop/api/d3d10/nf-d3d10-id3d10device-iasetprimitivetopology">ID3D11DeviceContext:: IASetPrimitiveTopology</a></p>
<p><a href="/windows/desktop/api/d3d10/nf-d3d10-id3d10device-drawauto">ID3D11DeviceContext::D rawAuto</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-beginscene">IDirect3DDevice9:: BeginScene</a></p>
<p><a href="/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-endscene">IDirect3DDevice9:: EndScene</a></p>
<p><a href="/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawprimitiveup">IDirect3DDevice9::D rawPrimitiveUP</a></p>
<p><a href="/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawindexedprimitiveup">IDirect3DDevice9::D rawIndexedPrimitiveUP</a></p></td>
<td align="left"><p>직접적으로 해당하는 항목이 없습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-showcursor">IDirect3DDevice9::ShowCursor</a></p>
<p><a href="/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-setcursorposition">IDirect3DDevice9::SetCursorPosition</a></p>
<p><a href="/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-setcursorproperties">IDirect3DDevice9::SetCursorProperties</a></p></td>
<td align="left"><p>표준 커서 Api를 사용 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-reset">IDirect3DDevice9:: Reset</a></p></td>
<td align="left"><p>분실 한 장치 및 POOL_MANAGED 더 이상 존재 하지 않습니다. <a href="/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1">IDXGISwapChain1::P resent1</a> 는 <a href="/windows/desktop/direct3ddxgi/dxgi-error">DXGI_ERROR_DEVICE_REMOVED</a> 반환 값을 사용 하 여 실패할 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawrectpatch">IDirect3DDevice9:DrawRectPatch</a></p>
<p><a href="/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawtripatch">IDirect3DDevice9: DrawTriPatch</a></p>
<p><a href="/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-lightenable">IDirect3DDevice9:LightEnable</a></p>
<p><a href="/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-multiplytransform">IDirect3DDevice9:MultiplyTransform</a></p>
<p><a href="/windows/desktop/direct3d9/id3dxeffectstatemanager--setlight">IDirect3DDevice9: SetLight</a></p>
<p><a href="/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-setmaterial">IDirect3DDevice9: SetMaterial</a></p>
<p><a href="/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-setnpatchmode">IDirect3DDevice9:SetNPatchMode</a></p>
<p><a href="/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-settransform">IDirect3DDevice9: SetTransform</a></p>
<p><a href="/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-setfvf">IDirect3DDevice9:SetFVF</a></p>
<p><a href="/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-settexturestagestate">IDirect3DDevice9:SetTextureStageState</a></p></td>
<td align="left"><p>고정 함수 파이프라인은 더 이상 사용 되지 않습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-checkdepthstencilmatch">IDirect3DDevice9:CheckDepthStencilMatch</a></p>
<p><a href="/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-checkdeviceformat">IDirect3DDevice9: CheckDeviceFormat</a></p>
<p><a href="/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-getdevicecaps">IDirect3DDevice9:GetDeviceCaps</a></p>
<p><a href="/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-validatedevice">IDirect3DDevice9: ValidateDevice</a></p></td>
<td align="left"><p>기능 비트는 기능 수준으로 대체 됩니다. 지정 된 기능 수준에는 몇 가지 형식 및 기능 사용 사례만 선택할 수 있습니다. <a href="/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkfeaturesupport">ID3D11Device:: CheckFeatureSupport</a> 및 <a href="/windows/desktop/api/d3d10/nf-d3d10-id3d10device-checkformatsupport">ID3D11Device:: checkformatsupport</a>를 사용 하 여 확인할 수 있습니다.</p></td>
</tr>
</tbody>
</table>

## <a name="surface-format-mapping"></a>표면 형식 매핑

다음 표를 사용 하 여 Direct3D 9 형식을 DXGI 형식으로 변환할 수 있습니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Direct3D 9 형식</th>
<th align="left">Direct3D 11 형식</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>D3DFMT_UNKNOWN</p></td>
<td align="left"><p>DXGI_FORMAT_UNKNOWN</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R8G8B8</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8R8G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_B8G8R8A8_UNORM</p>
<p>DXGI_FORMAT_B8G8R8A8_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X8R8G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_B8G8R8X8_UNORM</p>
<p>DXGI_FORMAT_B8G8R8X8_UNORM_SRGB</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R5G6B5</p></td>
<td align="left"><p>DXGI_FORMAT_B5G6R5_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X1R5G5B5</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A1R5G5B5</p></td>
<td align="left"><p>DXGI_FORMAT_B5G5R5A1_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A4R4G4B4</p></td>
<td align="left"><p>DXGI_FORMAT_B4G4R4A4_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R3G3B2</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8</p></td>
<td align="left"><p>DXGI_FORMAT_A8_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8R3G3B2</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X4R4G4B4</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A2B10G10R10</p></td>
<td align="left"><p>DXGI_FORMAT_R10G10B10A2</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8B8G8R8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UNORM</p>
<p>DXGI_FORMAT_R8G8B8A8_UNORM_SRGB</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_X8B8G8R8</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G16R16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A2R10G10B10</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A16B16G16R16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8P8</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_P8</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L8</p></td>
<td align="left"><p>DXGI_FORMAT_R8_UNORM</p>
<div class="alert">
<strong>참고</strong>    셰이더에서. r swizzle를 사용 하 여 다른 구성 요소와 빨간색을 중복 하 여 Direct3D 9 동작을 가져옵니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8L8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_UNORM</p>
<div class="alert">
<strong>참고</strong>    Swizzle rrrg를 사용 하 여 red를 복제 하 고 녹색을 알파 구성 요소로 이동 하 여 Direct3D 9 동작을 가져옵니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A4L4</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_V8U8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L6V5U5</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X8L8V8U8</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_Q8W8V8U8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_SNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_V16U16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_W11V11U10</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A2W10V10U10</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_UYVY</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R8G8_B8G8</p></td>
<td align="left"><p>DXGI_FORMAT_G8R8_G8B8_UNORM</p>
<div class="alert">
<strong>참고</strong>    Direct3D 9에서 데이터는 255.0 f에 의해 확장 되었지만 셰이더에서 처리 될 수 있습니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_YUY2</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G8R8_G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_B8G8_UNORM</p>
<div class="alert">
<strong>참고</strong>    Direct3D 9에서 데이터는 255.0 f에 의해 확장 되었지만 셰이더에서 처리 될 수 있습니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT1</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM & DXGI_FORMAT_BC1_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_DXT2</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM & DXGI_FORMAT_BC1_UNORM_SRGB</p>
<div class="alert">
<strong>참고</strong>    DXT1 및 DXT2는 API/하드웨어 관점에서 동일 합니다. 유일한 차이점은 응용 프로그램에서 추적할 수 있고 별도의 형식이 필요 하지 않은 미리 증가 된 알파를 사용 하는지 여부입니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT3</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM & DXGI_FORMAT_BC2_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_DXT4</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM & DXGI_FORMAT_BC2_UNORM_SRGB</p>
<div class="alert">
<strong>참고</strong>    DXT3 및 DXT4는 API/하드웨어 관점에서 동일 합니다. 유일한 차이점은 응용 프로그램에서 추적할 수 있고 별도의 형식이 필요 하지 않은 미리 증가 된 알파를 사용 하는지 여부입니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT5</p></td>
<td align="left"><p>DXGI_FORMAT_BC3_UNORM & DXGI_FORMAT_BC3_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D16 & D3DFMT_D16_LOCKABLE</p></td>
<td align="left"><p>DXGI_FORMAT_D16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D32</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D15S1</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D24S8</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D24X8</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D24X4S4</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D16</p></td>
<td align="left"><p>DXGI_FORMAT_D16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D32F_LOCKABLE</p></td>
<td align="left"><p>DXGI_FORMAT_D32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D24FS8</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_S1D15</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_S8D24</p></td>
<td align="left"><p>DXGI_FORMAT_D24_UNORM_S8_UINT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_X8D24</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X4S4D24</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L16</p></td>
<td align="left"><p>DXGI_FORMAT_R16_UNORM</p>
<div class="alert">
<strong>참고</strong>    D3D9 동작을 얻으려면 셰이더에 swizzle을 사용 하 여 빨간색을 다른 구성 요소에 복제 합니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_INDEX16</p></td>
<td align="left"><p>DXGI_FORMAT_R16_UINT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_INDEX32</p></td>
<td align="left"><p>DXGI_FORMAT_R32_UINT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_Q16W16V16U16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_MULTI2_ARGB8</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_G16R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A16B16G16R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G32R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A32B32G32R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_CxV8U8</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT1</p></td>
<td align="left"><p>DXGI_FORMAT_R32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT2</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT3</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT4</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPED3DCOLOR</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_UBYTE4</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UINT</p>
<div class="alert">
<strong>참고</strong>    셰이더는 UINT 값을 가져오지만, Direct3D 9 스타일 정수 계열 float가 필요한 경우 (0.0 f, 1.0 f ... 255. f), UINT는 셰이더의 float32로만 변환할 수 있습니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_SHORT2</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SINT</p>
<div class="alert">
<strong>참고</strong>    셰이더는에 지 값을 가져오지만, Direct3D 9 스타일 정수 계열 float가 필요한 경우에는 셰이더에서 값을 float32로만 변환할 수 있습니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_SHORT4</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SINT</p>
<div class="alert">
<strong>참고</strong>    셰이더는에 지 값을 가져오지만, Direct3D 9 스타일 정수 계열 float가 필요한 경우에는 셰이더에서 값을 float32로만 변환할 수 있습니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_UBYTE4N</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_SHORT2N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_SHORT4N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_USHORT2N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_USHORT4N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_UDEC3</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_DEC3N</p></td>
<td align="left"><p>사용할 수 없음</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT16_2</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT16_4</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>FourCC ' ATI1 '</p></td>
<td align="left"><p>DXGI_FORMAT_BC4_UNORM</p>
<div class="alert">
<strong>참고</strong>    기능 수준 10.0 이상 필요
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>FourCC ' ATI2 '</p></td>
<td align="left"><p>DXGI_FORMAT_BC5_UNORM</p>
<div class="alert">
<strong>참고</strong>    기능 수준 10.0 이상 필요
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

## <a name="additional-mapping-info"></a>추가 매핑 정보

- **IDirect3DDevice9:: SetCursorPosition** 는 [**SetCursorPos**](/windows/desktop/api/winuser/nf-winuser-setcursorpos)로 바뀝니다.
- **IDirect3DDevice9:: SetCursorProperties** 는 [**setcursor**](/windows/desktop/api/winuser/nf-winuser-setcursor)로 대체 됩니다.
- **IDirect3DDevice9:: SetIndices** 는 [**ID3D11DeviceContext:: IASetIndexBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer)로 바뀝니다.
- **IDirect3DDevice9:: SetRenderTarget** 은 [**ID3D11DeviceContext:: OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets)로 바뀝니다.
- **IDirect3DDevice9:: SetScissorRect** 은 [**ID3D11DeviceContext:: RSSetScissorRects**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetscissorrects)로 바뀝니다.
- **IDirect3DDevice9:: SetStreamSource** 는 [**ID3D11DeviceContext:: IASetVertexBuffers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers)로 바뀝니다.
- **IDirect3DDevice9:: SetVertexDeclaration** 은 [**ID3D11DeviceContext:: IASetInputLayout**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout)로 바뀝니다.
- **IDirect3DDevice9:: SetViewport** 는 [**ID3D11DeviceContext:: RSSetViewports**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports)로 바뀝니다.
- **IDirect3DDevice9:: ShowCursor** 는 [**ShowCursor**](/windows/desktop/api/winuser/nf-winuser-showcursor)로 바뀝니다.

**IDirect3DDevice9:: SetGammaRamp** 를 통해 비디오 카드의 하드웨어 감마 램프를 제어할 **수 있습니다.** [감마 수정 사용](/windows/win32/direct3ddxgi/using-gamma-correction)을 참조 하세요.

**IDirect3DDevice9::P rocessvertices** 은 Geometry 셰이더의 스트림 출력 기능으로 대체 됩니다. [스트림 출력 단계 시작](/windows/win32/direct3d11/d3d10-graphics-programming-guide-output-stream-stage-getting-started)을 참조 하세요.

사용자 클립을 설정 하는 **IDirect3DDevice9:: SetClipPlane** 메서드는 VS_4_0 이상에서 사용할 수 있는 HLSL **SV_ClipDistance** 버텍스 셰이더 출력 의미 체계 ( [의미 체계](/windows/win32/direct3dhlsl/dx-graphics-hlsl-semantics)참조) 또는 새로운 HLSL clipplanes 함수 특성 ( [기능 수준 9 하드웨어의 사용자 클립 평면](/windows/win32/direct3dhlsl/user-clip-planes-on-10level9)참조)으로 대체 되었습니다.

**IDirect3DDevice9:: SetPaletteEntries** 및 **IDirect3DDevice9:: SetCurrentTexturePalette** 는 사용 되지 않습니다. 이를 대신 256x1 **R8G8B8A8** 질감에서 색을 조회 하는 픽셀 셰이더에 바꿉니다.

**DrawRectPatch**, **drawtripatch**, **SetNPatchMode**및 **DeletePatch** 와 같은 고정 함수 공간 분할 함수는 더 이상 사용 되지 않습니다. 이를 프로그래밍 가능 파이프라인 SM 5.0 공간 분할 셰이더로 대체 합니다 (하드웨어가 공간 분할 셰이더를 지 원하는 경우).

**IDirect3DDevice9:: SetFVF**및 FVF 코드는 더 이상 지원 되지 않습니다. D3D11 입력 레이아웃으로 이식 하기 전에 D3D8/D3D9 FVF 코드에서 D3D9 Vertex 선언으로 이식 해야 합니다.

VS_4_0 이상에서 꼭 짓 점 셰이더를 시작할 때 약간의 비트 연산을 사용 하 여 직접 지원 되지 않는 모든 **D3DDECLTYPE** 유형을 매우 효율적으로 에뮬레이션할 수 있습니다.