---
title: 하드웨어 범위에서 섀도 맵 지원
description: 더 빠른 장치에서 더 높은 성능의 그림자를 렌더링 하 고, 더 낮은 수준의 장치에서 더 빠른 그림자를 렌더링 합니다.
ms.assetid: d97c0544-44f2-4e29-5e02-54c45e0dff4e
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 섀도 맵, directx
ms.localizationpriority: medium
ms.openlocfilehash: 2c21b1c77b15435458d75a1772a914aa95048559
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168297"
---
# <a name="support-shadow-maps-on-a-range-of-hardware"></a>하드웨어 범위에서 섀도 맵 지원




더 빠른 장치에서 더 높은 성능의 그림자를 렌더링 하 고, 더 낮은 수준의 장치에서 더 빠른 그림자를 렌더링 합니다. 연습 4 부 [: Direct3D 11에서 깊이 버퍼를 사용 하 여 섀도 볼륨 구현](implementing-depth-buffers-for-shadow-mapping.md).

## <a name="comparison-filter-types"></a>비교 필터 유형


장치에서 성능 저하를 감당할 수 있는 경우에만 선형 필터링을 사용 합니다. 일반적으로 Direct3D 기능 수준 9 \_ 1 장치에는 그림자에 대 한 선형 필터링을 위한 충분 한 전원이 없습니다. 이러한 장치에는 대신 지점 필터링을 사용 합니다. 선형 필터링을 사용 하는 경우 그림자 가장자리가 혼합 되도록 픽셀 셰이더를 조정 합니다.

포인트 필터링을 위한 비교 샘플러를 만듭니다.

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

그런 다음 선형 필터링을 위한 샘플러를 만듭니다.

```cpp
comparisonSamplerDesc.Filter = D3D11_FILTER_COMPARISON_MIN_MAG_MIP_LINEAR;
DX::ThrowIfFailed(
    pD3DDevice->CreateSamplerState(
        &comparisonSamplerDesc,
        &m_comparisonSampler_linear
        )
    );
```

샘플러 선택:

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

선형 필터링을 사용 하 여 그림자 가장자리 Blend:

```cpp
// Blends the shadow area into the lit area.
float3 light = lighting * (ambient + DplusS(N, L, NdotL, input.view));
float3 shadow = (1.0f - lighting) * ambient;
return float4(input.color * (light + shadow), 1.f);
```

## <a name="shadow-buffer-size"></a>섀도 버퍼 크기


더 큰 그림자 맵은 blocky로 표시 되지 않지만 그래픽 메모리에서 더 많은 공간을 차지 합니다. 게임에서 다른 그림자 맵 크기를 실험 하 고 다양 한 유형의 장치 및 다른 디스플레이 크기의 결과를 관찰 합니다. 더 작은 그래픽 메모리를 사용 하 여 더 나은 결과를 얻을 수 있도록 종속 된 그림자 맵과 같은 최적화를 고려 합니다. [그림자 깊이 맵을 개선 하는 일반적인 방법을](/windows/desktop/DxTechArts/common-techniques-to-improve-shadow-depth-maps)참조 하세요.

## <a name="shadow-buffer-depth"></a>섀도 버퍼 깊이


섀도 버퍼의 전체 자릿수를 더 정확 하 게 지정 하면 z 버퍼와 같은 문제를 방지할 수 있습니다. 하지만 더 큰 그림자 맵과 마찬가지로 정밀도가 클수록 더 많은 메모리가 사용 됩니다. 게임에서 다양 한 깊이 전체 자릿수를 사용 하 여 실험-DXGI \_ 형식 \_ R24G8 형식 \_ 및 \_ \_ R16 \_ 유형 없음-다른 기능 수준의 속도와 품질을 관찰 합니다.

## <a name="optimizing-precompiled-shaders"></a>미리 컴파일된 셰이더 최적화


UWP (유니버설 Windows 플랫폼) 앱은 동적 셰이더 컴파일을 사용할 수 있지만 동적 셰이더 링크를 사용 하는 것이 더 빠릅니다. 컴파일러 지시문 및 `#ifdef` 블록을 사용 하 여 다른 버전의 셰이더를 만들 수도 있습니다. 이렇게 하려면 텍스트 편집기에서 Visual Studio 프로젝트 파일을 열고 `<FxcCompiler>` HLSL에 대 한 여러 항목을 추가 합니다 (각각 적절 한 전처리기 정의가 포함 됨). 이 경우 다른 파일 이름이 있습니다. 이 경우 Visual Studio는 \_ point와 linear를 \_ 다른 버전의 셰이더에 추가 합니다.

셰이더의 선형 필터링 버전에 대 한 프로젝트 파일 항목은 선형을 정의 합니다.

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

셰이더의 선형 필터링 버전에 대 한 프로젝트 파일 항목에는 전처리기 정의가 포함 되어 있지 않습니다.

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

 

 