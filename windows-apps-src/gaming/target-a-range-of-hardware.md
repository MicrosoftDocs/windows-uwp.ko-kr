---
title: 다양한 하드웨어에서 그림자 맵 지원
description: 더 빠른 장치에서 충실도가 더 높은 그림자를 렌더링하고 덜 강력한 장치에서 보다 빠른 그림자를 렌더링합니다.
ms.assetid: d97c0544-44f2-4e29-5e02-54c45e0dff4e
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 게임, 그림자 맵, directx
ms.localizationpriority: medium
ms.openlocfilehash: d0e661065f86ac173a6ce323281c80fc964d0a4c
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8190244"
---
# <a name="support-shadow-maps-on-a-range-of-hardware"></a>다양한 하드웨어에서 그림자 맵 지원




더 빠른 디바이스에서 충실도가 더 높은 그림자를 렌더링하고 덜 강력한 디바이스에서 보다 빠른 그림자를 렌더링합니다. [연습의 4부: Direct3D 11의 깊이 버퍼를 사용하여 그림자 볼륨 구현](implementing-depth-buffers-for-shadow-mapping.md).

## <a name="comparison-filter-types"></a>비교 필터 형식


디바이스가 성능 저하를 감당할 수 있는 경우에만 선형 필터링을 사용합니다. 일반적으로 Direct3D 기능 수준 9\_1 디바이스에는 그림자의 선형 필터링을 위한 전원이 부족합니다. 이러한 디바이스에서는 대신 포인트 필터링을 사용합니다. 선형 필터링을 사용하는 경우 그림자 가장자리를 혼합하도록 픽셀 셰이더를 조정합니다.

포인트 필터링에 대한 비교 샘플러를 만듭니다.

```cpp
D3D11_SAMPLER_DESC comparisonSamplerDesc;
ZeroMemory(&comparisonSamplerDesc, sizeof(D3D11_SAMPLER_DESC));
comparisonSamplerDesc.AddressU = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.AddressV = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.AddressW = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.BorderColor[0] = 1.0f;
comparisonSamplerDesc.BorderColor[1] = 1.0f;
comparisonSamplerDesc.BorderColor[2] = 1.0f;
comparisonSamplerDesc.BorderColor[3] = 1.0f;
comparisonSamplerDesc.MinLOD = 0.f;
comparisonSamplerDesc.MaxLOD = D3D11_FLOAT32_MAX;
comparisonSamplerDesc.MipLODBias = 0.f;
comparisonSamplerDesc.MaxAnisotropy = 0;
comparisonSamplerDesc.ComparisonFunc = D3D11_COMPARISON_LESS_EQUAL;
comparisonSamplerDesc.Filter = D3D11_FILTER_COMPARISON_MIN_MAG_MIP_POINT;

// Point filtered shadows can be faster, and may be a good choice when
// rendering on hardware with lower feature levels. This sample has a
// UI option to enable/disable filtering so you can see the difference
// in quality and speed.

DX::ThrowIfFailed(
    pD3DDevice->CreateSamplerState(
        &comparisonSamplerDesc,
        &m_comparisonSampler_point
        )
    );
```

그런 다음 선형 필터링에 대한 샘플러를 만듭니다.

```cpp
comparisonSamplerDesc.Filter = D3D11_FILTER_COMPARISON_MIN_MAG_MIP_LINEAR;
DX::ThrowIfFailed(
    pD3DDevice->CreateSamplerState(
        &comparisonSamplerDesc,
        &m_comparisonSampler_linear
        )
    );
```

샘플러를 선택합니다.

```cpp
ID3D11PixelShader* pixelShader;
ID3D11SamplerState** comparisonSampler;
if (m_useLinear)
{
    pixelShader = m_shadowPixelShader_linear.Get();
    comparisonSampler = m_comparisonSampler_linear.GetAddressOf();
}
else
{
    pixelShader = m_shadowPixelShader_point.Get();
    comparisonSampler = m_comparisonSampler_point.GetAddressOf();
}

// Attach our pixel shader.
context->PSSetShader(
    pixelShader,
    nullptr,
    0
    );

context->PSSetSamplers(0, 1, comparisonSampler);
context->PSSetShaderResources(0, 1, m_shadowResourceView.GetAddressOf());
```

선형 필터링으로 그림자 가장자리를 혼합합니다.

```cpp
// Blends the shadow area into the lit area.
float3 light = lighting * (ambient + DplusS(N, L, NdotL, input.view));
float3 shadow = (1.0f - lighting) * ambient;
return float4(input.color * (light + shadow), 1.f);
```

## <a name="shadow-buffer-size"></a>그림자 버퍼 크기


더 큰 그림자 맵은 농담이 고르지 않게 보이지 않지만 그래픽 메모리에서 공간을 더 차지합니다. 게임에서 여러 그림자 맵 크기로 실험하고 여러 형식의 장치 및 여러 표시 크기의 결과를 확인합니다. 그래픽 메모리를 더 적게 사용하면서 더 나은 결과를 내는 중첩된 그림자 맵과 같은 최적화를 고려하세요. [그림자 깊이 맵을 향상시키기 위한 일반적인 기법](https://msdn.microsoft.com/library/windows/desktop/ee416324)을 참조하세요.

## <a name="shadow-buffer-depth"></a>그림자 버퍼 깊이


그림자 버퍼에서 정밀도가 높을수록 깊이 테스트 결과가 더욱 정확하므로 Z-버퍼 충돌과 같은 문제를 방지하는 데 도움이 됩니다. 하지만 더 큰 그림자 맵처럼 정밀도가 더 높으면 메모리를 더 많이 차지합니다. 게임에서 여러 깊이 정밀도 형식을 실험하고(DXGI\_FORMAT\_R24G8\_TYPELESS 대 DXGI\_FORMAT\_R16\_TYPELESS) 여러 기능 수준에서 속도와 품질을 확인합니다.

## <a name="optimizing-precompiled-shaders"></a>미리 컴파일된 셰이더 최적화


Windows 스토어 앱은 동적 셰이더 컴파일을 사용할 수 있지만 동적 셰이더 연결을 사용하는 것이 더 빠릅니다. 컴파일러 지시문과 `#ifdef` 블록을 사용하여 서로 다른 버전의 셰이더를 만들 수도 있습니다. 텍스트 편집기에서 Visual Studio 프로젝트 파일을 열고 HLSL에 대한 여러 `<FxcCompiler>` 항목(각각에 적절한 전처리기 정의가 포함된)을 추가하여 이 작업을 수행합니다. 이 작업에는 서로 다른 파일 이름이 필요하며, 이 경우 Visual Studio가 \_point 및 \_linear를 서로 다른 버전의 셰이더에 추가합니다.

선형 필터링된 버전의 셰이더에 대한 프로젝트 파일 항목은 LINEAR를 정의합니다.

```xml
<FxCompile Include="Content\ShadowPixelShader.hlsl">
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|x64'">Pixel</ShaderType>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">false</DisableOptimizations>
  <EnableDebuggingInformation Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">true</EnableDebuggingInformation>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">false</DisableOptimizations>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">false</DisableOptimizations>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|x64'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Release|x64'">LINEAR</PreprocessorDefinitions>
</FxCompile>
```

선형 필터링된 버전의 셰이더에 대한 프로젝트 파일 항목은 전처리기 정의를 포함하지 않습니다.

```xml
<FxCompile Include="Content\ShadowPixelShader.hlsl">
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|x64'">Pixel</ShaderType>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">false</DisableOptimizations>
  <EnableDebuggingInformation Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">true</EnableDebuggingInformation>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">false</DisableOptimizations>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">false</DisableOptimizations>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|x64'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
</FxCompile>
```

 

 




