---
title: DirectX 11 API에 DirectX 9 기능 매핑
description: Direct3D 9 게임에서 사용하는 기능이 Direct3D 11 및 UWP(유니버설 Windows 플랫폼)로 변환되는 방법을 이해합니다.
ms.assetid: 3aa8a114-4e47-ae0a-9447-88ba324377b8
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx 9, directx 11, 포팅
ms.localizationpriority: medium
ms.openlocfilehash: 0cfaa071ea0182ef5fac264e85d919be5744d15d
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/05/2019
ms.locfileid: "9050676"
---
# <a name="map-directx-9-features-to-directx-11-apis"></a>DirectX 11 API에 DirectX 9 기능 매핑



**요약**

-   [DirectX 포트 계획](plan-your-directx-port.md)
-   [Direct3D 9에서 Direct3D 11로의 중요 변경 사항](understand-direct3d-11-1-concepts.md)
-   기능 매핑


Direct3D 9 게임에서 사용하는 기능이 Direct3D 11 및 UWP(유니버설 Windows 플랫폼)로 변환되는 방법을 이해합니다.

## <a name="mapping-direct3d-9-to-directx-11-apis"></a>DirectX 11 API에 Direct3D 9 매핑


[Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466)는 여전히 DirectX 그래픽의 기반이지만 API는 DirectX 9 이후로 변경되었습니다.

-   Microsoft DXGI(DirectX Graphics Infrastructure)는 그래픽 어댑터를 설정하는 데 사용됩니다. [DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534)를 사용하여 버퍼 형식을 선택하고, 스왑 체인을 만들고, 프레임을 표시하고, 공유 리소스를 만들 수 있습니다. [DXGI 개요](https://msdn.microsoft.com/library/windows/desktop/bb205075)를 참조하세요.
-   Direct3D 디바이스 컨텍스트는 파이프라인 상태를 설정하고 렌더링 명령을 생성하는 데 사용됩니다. 대부분의 샘플에서는 즉각적인 컨텍스트를 사용하여 장치에 직접 렌더링합니다. 또한 Direct3D 11은 다중 스레딩 렌더링을 지원하며, 이 경우 지연된 컨텍스트가 사용됩니다. [Direct3D 11에서의 장치 소개](https://msdn.microsoft.com/library/windows/desktop/ff476880)를 참조하세요.
-   일부 기능은 사용되지 않는데, 특히 고정 함수 파이프라인은 사용되지 않습니다. [사용되지 않는 기능](https://msdn.microsoft.com/library/windows/desktop/cc308047)을 참조하세요.

Direct3D 11의 전체 기능 목록은 [Direct3D 11 기능](https://msdn.microsoft.com/library/windows/desktop/ff476342) 및 [Direct3D 11 기능](https://msdn.microsoft.com/library/windows/desktop/hh404562)을 참조하세요.

## <a name="moving-from-direct2d-9-to-direct2d-11"></a>Direct2D 9에서 Direct2D 11로 이동


[Direct2D(Windows)](https://msdn.microsoft.com/library/windows/desktop/dd370990)는 여전히 DirectX 그래픽 및 Windows의 중요한 부분입니다. 여전히 Direct2D를 사용하여 2D 게임을 그리고 Direct3D 위에 오버레이(HUD)를 그릴 수 있습니다.

Direct2D는 Direct3D 위에서 실행됩니다. 2D 게임은 두 API 중 어느 쪽을 사용해서도 구현할 수 있습니다. 예를 들어, Direct3D를 사용하여 구현된 2D 게임은 직교 투영을 사용하고, 기본 요소의 그리기 순서를 제어하기 위한 Z 값을 설정하고, 픽셀 셰이더를 사용하여 특수 효과를 추가할 수 있습니다.

Direct2D는 Direct3D를 기반으로 하므로 역시 DXGI 및 디바이스 컨텍스트를 사용합니다. [Direct2D API 개요](https://msdn.microsoft.com/library/windows/desktop/dd317121)를 참조하세요.

[DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) API는 Direct2D를 사용하여 서식 있는 텍스트에 대한 지원을 추가합니다. [DirectWrite 소개](https://msdn.microsoft.com/library/windows/desktop/dd371554)를 참조하세요.

## <a name="replace-deprecated-helper-libraries"></a>사용되지 않는 도우미 라이브러리 바꾸기


D3DX 및 DXUT는 더 이상 사용되지 않으며 UWP 게임에서 사용할 수 없습니다. 이러한 도우미 라이브러리는 텍스처 로드 및 메시 로드와 같은 작업에 대한 리소스를 제공했습니다.

-   [Direct3D 9에서 UWP로의 간단한 포팅](walkthrough--simple-port-from-direct3d-9-to-11-1.md) 연습에서는 창을 설정하고, Direct3D를 초기화하고, 기본 3D 렌더링을 수행하는 방법을 보여 줍니다.
-   [DirectX로 작성한 간단한 UWP 게임](tutorial--create-your-first-uwp-directx-game.md) 연습에서는 그래픽, 파일 로드, UI, 컨트롤 및 소리를 비롯한 일반적인 게임 프로그래밍 작업을 보여 줍니다.
-   [DirectX 도구 키트](https://go.microsoft.com/fwlink/p/?LinkID=248929) 커뮤니티 프로젝트는 Direct3D 11 및 UWP 앱에 사용할 수 있는 도우미 클래스를 제공합니다.

## <a name="move-shader-programs-from-fx-to-hlsl"></a>FX에서 HLSL로 셰이더 프로그램 이동


효과를 포함한 D3DX 유틸리티 라이브러리(D3DX 9, D3DX 10 및 D3DX 11)는 UWP에서는 사용되지 않습니다. 모든 UWP용 DirectX 게임은 효과 없이 [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561)을 사용하여 그래픽 파이프라인을 구동합니다.

Visual Studio에서는 여전히 내부의 FXC를 사용하여 셰이더 개체를 컴파일합니다. UWP 게임 셰이더는 미리 컴파일되어 있습니다. 바이트코드가 런타임에 로드된 다음 적절한 렌더링 단계 동안 각 셰이더 리소스가 그래픽 파이프라인에 바인딩됩니다. 셰이더는 고유한 별도의 .HLSL 파일로 이동되어야 하며, 렌더링 기술은 C++ 코드로 구현되어야 합니다.

셰이더 리소스 로드를 간단히 살펴보려면 [Direct3D 9에서 UWP로의 간단한 포팅](walkthrough--simple-port-from-direct3d-9-to-11-1.md)을 참조하세요.

Direct3D 11은 셰이더 모델 5에 도입되었으며, 이 모델에는 Direct3D 기능 수준 11\_0 이상이 필요합니다. [Direct3D 11용 HLSL 셰이더 모델 5 기능](https://msdn.microsoft.com/library/windows/desktop/ff471419)을 참조하세요.

## <a name="replace-xnamath-and-d3dxmath"></a>XNAMath 및 D3DXMath 바꾸기


XNAMath(또는 D3DXMath)를 사용하여 코드를 [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833)로 마이그레이션해야 합니다. DirectXMath에는 x86, x64 및 ARM에서 포팅 가능한 형식이 포함되어 있습니다. [XNA 수학 라이브러리에서 코드 마이그레이션](https://msdn.microsoft.com/library/windows/desktop/ee418730)을 참조하세요.

DirectXMath float 형식은 셰이더에 사용하기 편리합니다. 예를 들어, [**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608) 및 [**XMFLOAT4X4**](https://msdn.microsoft.com/library/windows/desktop/ee419621)는 편리하게 상수 버퍼에 맞게 데이터를 정렬합니다.

## <a name="replace-directsound-with-xaudio2-and-background-audio"></a>DirectSound를 XAudio2(및 백그라운드 오디오)로 바꾸기


DirectSound는 UWP에 지원되지 않음

-   [XAudio2](https://msdn.microsoft.com/library/windows/desktop/hh405049)를 사용하여 게임에 소리 효과를 추가합니다.

##  <a name="replace-directinput-with-xinput-and-uwp-apis"></a>DirectInput을 XInput 및 UWP API로 바꾸기


DirectInput은 UWP에 지원되지 않음

-   마우스, 키보드 및 터치식 입력에 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 입력 이벤트 콜백을 사용합니다.
-   게임 컨트롤러 지원(및 게임 컨트롤러 헤드셋 지원)에 [XInput](https://msdn.microsoft.com/library/windows/desktop/ee417001) 1.4를 사용합니다. 데스크톱 및 UWP에 공유 코드베이스를 사용하는 경우 이전 버전과의 호환성에 대한 자세한 내용은 [XInput 버전](https://msdn.microsoft.com/library/windows/desktop/hh405051)을 참조하세요.
-   게임에 앱 바를 사용해야 하는 경우 [**EdgeGesture**](https://msdn.microsoft.com/library/windows/apps/hh701600) 이벤트를 등록합니다.

## <a name="use-microsoft-media-foundation-instead-of-directshow"></a>DirectShow 대신 Microsoft 미디어 파운데이션 사용


DirectShow는 더 이상 DirectX API(또는 Windows API)의 일부가 아닙니다. [Microsoft 미디어 파운데이션](https://msdn.microsoft.com/library/windows/desktop/ms694197)은 공유 화면을 사용하여 Direct3D에 비디오 콘텐츠를 제공합니다. [Direct3D 11 비디오 API](https://msdn.microsoft.com/library/windows/desktop/hh447677)를 참조하세요.

## <a name="replace-directplay-with-networking-code"></a>DirectPlay를 네트워킹 코드로 바꾸기


Microsoft DirectPlay는 사용되지 않습니다. 게임에서 네트워크 서비스를 사용하는 경우 UWP 요구 사항을 준수하는 네트워킹 코드를 제공해야 합니다. 다음 API를 사용합니다.

-   [UWP 앱(네트워킹)(Windows)용 Win32 및 COM](https://msdn.microsoft.com/library/windows/apps/br205759)
-   [**Windows.Networking 네임스페이스(Windows)**](https://msdn.microsoft.com/library/windows/apps/br207124)
-   [**Windows.Networking.Sockets 네임스페이스(Windows)**](https://msdn.microsoft.com/library/windows/apps/br226960)
-   [**Windows.Networking.Connectivity 네임스페이스(Windows)**](https://msdn.microsoft.com/library/windows/apps/br207308)
-   [**Windows.ApplicationModel.Background 네임스페이스(Windows)**](https://msdn.microsoft.com/library/windows/apps/br224847)

다음 문서는 네트워킹 기능을 추가하고 앱의 패키지 매니페스트에 네트워킹에 대한 지원을 선언하는 데 도움이 됩니다.

-   [소켓을 사용하여 연결(C#/VB/C++ 및 XAML로 작성한 UWP 앱)(Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh452976)
-   [WebSocket을 사용하여 연결(C#/VB/C++ 및 XAML로 작성한 UWP 앱)(Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh994396)
-   [웹 서비스에 연결(C#/VB/C++ 및 XAML로 작성한 UWP 앱)(Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh761504)
-   [네트워킹 기본 사항](https://msdn.microsoft.com/library/windows/apps/mt280233)

모든 UWP 앱(게임 포함)은 특정 유형의 백그라운드 작업을 사용하여, 앱이 일시 중단된 동안에도 연결을 유지합니다. 게임이 일시 중단된 동안 연결 상태를 유지해야 하는 경우 [네트워킹 기본 사항](https://msdn.microsoft.com/library/windows/apps/mt280233)을 참조하세요.

## <a name="function-mapping"></a>함수 매핑


다음 표는 Direct3D 9에서 Direct3D 11로 코드를 변환하는 데 도움이 됩니다. 또한 장치와 디바이스 컨텍스트를 구별하는 데에도 도움이 될 수 있습니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Direct3D9</th>
<th align="left">Direct3D 11 해당 함수</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174336">IDirect3DDevice9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/dn280493">ID3D11Device2</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/dn280498">ID3D11DeviceContext2</a></p>
<p>그래픽 파이프라인 단계는 <a href="https://msdn.microsoft.com/library/windows/desktop/ff476882">그래픽 파이프라인</a>에 설명되어 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174300">IDirect3D9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/hh404556">IDXGIFactory2</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/hh404537">IDXGIAdapter2</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/dn280345">IDXGIDevice3</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174423">IDirect3DDevice9::Present</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/hh446797">IDXGISwapChain1::Present1</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174472">IDirect3DDevice9::TestCooperativeLevel</a></p></td>
<td align="left"><p>DXGI_PRESENT_TEST 플래그 설정을 통해<a href="https://msdn.microsoft.com/library/windows/desktop/hh446797">IDXGISwapChain1::Present1</a>을 호출합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174322">IDirect3DBaseTexture9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205909">IDirect3DTexture9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174329">IDirect3DCubeTexture9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205941">IDirect3DVolumeTexture9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205865">IDirect3DIndexBuffer9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205915">IDirect3DVertexBuffer9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476351">ID3D11Buffer</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476633">ID3D11Texture1D</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476635">ID3D11Texture2D</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476637">ID3D11Texture3D</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476628">ID3D11ShaderResourceView</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476582">ID3D11RenderTargetView</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476377">ID3D11DepthStencilView</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205922">IDirect3DVertexShader9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205869">IDirect3DPixelShader9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476641">ID3D11VertexShader</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476576">ID3D11PixelShader</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205919">IDirect3DVertexDeclaration9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476575">ID3D11InputLayout</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205805">IDirect3DDevice9::SetRenderState</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205806">IDirect3DDevice9::SetSamplerState</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/hh404571">ID3D11BlendState1</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476375">ID3D11DepthStencilState</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/hh446828">ID3D11RasterizerState1</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476588">ID3D11SamplerState</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174369">IDirect3DDevice9::DrawIndexedPrimitive</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174371">IDirect3DDevice9::DrawPrimitive</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476407">ID3D11DeviceContext::Draw</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476409">ID3D11DeviceContext::DrawIndexed</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb173566">ID3D11DeviceContext::DrawIndexedInstanced</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb173567">ID3D11DeviceContext::DrawInstanced</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb173590">ID3D11DeviceContext::IASetPrimitiveTopology</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb173564">ID3D11DeviceContext::DrawAuto</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174350">IDirect3DDevice9::BeginScene</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174375">IDirect3DDevice9::EndScene</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174372">IDirect3DDevice9::DrawPrimitiveUP</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174370">IDirect3DDevice9::DrawIndexedPrimitiveUP</a></p></td>
<td align="left"><p>직접적으로 해당하는 항목이 없습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174470">IDirect3DDevice9::ShowCursor</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174429">IDirect3DDevice9::SetCursorPosition</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174430">IDirect3DDevice9::SetCursorProperties</a></p></td>
<td align="left"><p>표준 커서 API를 사용합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174425">IDirect3DDevice9::Reset</a></p></td>
<td align="left"><p>LOST 장치 및 POOL_MANAGED는 더 이상 존재하지 않습니다. <a href="https://msdn.microsoft.com/library/windows/desktop/hh446797">IDXGISwapChain1::Present1</a>은 <a href="https://msdn.microsoft.com/library/windows/desktop/bb509553">DXGI_ERROR_DEVICE_REMOVED</a> 반환 값에서 실패할 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174373">IDirect3DDevice9:DrawRectPatch</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174374">IDirect3DDevice9:DrawTriPatch</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174421">IDirect3DDevice9: LightEnable</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174422">IDirect3DDevice9:MultiplyTransform</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205798">IDirect3DDevice9:SetLight</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174437">IDirect3DDevice9:SetMaterial</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174438">IDirect3DDevice9:SetNPatchMode</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174463">IDirect3DDevice9:SetTransform</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174433">IDirect3DDevice9:SetFVF</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174462">IDirect3DDevice9:SetTextureStageState</a></p></td>
<td align="left"><p>고정 함수 파이프라인은 사용되지 않습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174308">IDirect3DDevice9:CheckDepthStencilMatch</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174309">IDirect3DDevice9:CheckDeviceFormat</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174320">IDirect3DDevice9:GetDeviceCaps</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205859">IDirect3DDevice9:ValidateDevice</a></p></td>
<td align="left"><p>기능 비트는 기능 수준으로 바뀌었습니다. 일부 형식 및 기능 사용 사례만 지정된 기능 수준에 대해 선택적입니다. <a href="https://msdn.microsoft.com/library/windows/desktop/ff476497">ID3D11Device::CheckFeatureSupport</a> 및 <a href="https://msdn.microsoft.com/library/windows/desktop/bb173536">ID3D11Device::CheckFormatSupport</a>를 통해 이들을 확인할 수 있습니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="surface-format-mapping"></a>화면 형식 매핑


다음 표를 사용하여 Direct3D 9 형식을 DXGI 형식으로 변환합니다.

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
<strong>참고</strong>  빨간색을 다른 구성 요소 Direct3D 9 동작을 얻으려면 셰이더에서.r swizzle을 사용 합니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8L8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_UNORM</p>
<div class="alert">
<strong>참고</strong>  셰이더에서 swizzle.rrrg를 사용 하 여 빨간색을 복제 하 고 녹색을 알파 구성 Direct3D 9 동작을 얻으려면 이동 합니다.
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
<strong>참고</strong>  Direct3D 9 데이터, 255.0 f 씩 되었지만이 셰이더에서 처리할 수 있습니다.
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
<strong>참고</strong>  Direct3D 9 데이터, 255.0 f 씩 되었지만이 셰이더에서 처리할 수 있습니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT1</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM 및 DXGI_FORMAT_BC1_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_DXT2</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM 및 DXGI_FORMAT_BC1_UNORM_SRGB</p>
<div class="alert">
<strong>참고</strong>  DXT1 및 DXT2는 / 하드웨어 관점에서 동일 합니다. 유일한 차이점은 응용 프로그램에서 추적할 수 있고 별도의 형식이 필요하지 않은 프리멀티플라이된 알파가 사용되는지 여부입니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT3</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM 및 DXGI_FORMAT_BC2_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_DXT4</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM 및 DXGI_FORMAT_BC2_UNORM_SRGB</p>
<div class="alert">
<strong>참고</strong>  DXT3 및 DXT4는 / 하드웨어 관점에서 동일 합니다. 유일한 차이점은 응용 프로그램에서 추적할 수 있고 별도의 형식이 필요하지 않은 프리멀티플라이된 알파가 사용되는지 여부입니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT5</p></td>
<td align="left"><p>DXGI_FORMAT_BC3_UNORM 및 DXGI_FORMAT_BC3_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D16 및 D3DFMT_D16_LOCKABLE</p></td>
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
<strong>참고</strong>  빨간색을 복제 하 여 D3D9 동작을 얻으려면 다른 구성 요소를 셰이더에서.r swizzle을 사용 합니다.
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
<strong>참고</strong>  UINT 값을 가져오지만 경우 Direct3D 9 스타일의 정수 float가 필요한 (0.0 f, 1.0 f... 255.f) 셰이더에서 float32를 UINT를 변환할 수 있습니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_SHORT2</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SINT</p>
<div class="alert">
<strong>참고</strong>  SINT 값을 가져오지만 Direct3D 9 스타일의 정수 float가 필요한 경우 셰이더에서 float32로 SINT 변환 방금 수 있습니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_SHORT4</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SINT</p>
<div class="alert">
<strong>참고</strong>  SINT 값을 가져오지만 Direct3D 9 스타일의 정수 float가 필요한 경우 셰이더에서 float32로 SINT 변환 방금 수 있습니다.
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
<td align="left"><p>FourCC 'ATI1'</p></td>
<td align="left"><p>DXGI_FORMAT_BC4_UNORM</p>
<div class="alert">
<strong>참고</strong>  기능 수준 10.0 이상 필요
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>FourCC 'ATI2'</p></td>
<td align="left"><p>DXGI_FORMAT_BC5_UNORM</p>
<div class="alert">
<strong>참고</strong>  기능 수준 10.0 이상 필요
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

 

 




