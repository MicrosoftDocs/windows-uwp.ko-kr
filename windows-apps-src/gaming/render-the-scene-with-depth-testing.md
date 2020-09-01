---
title: 깊이 테스트를 사용 하 여 장면 렌더링
description: 꼭 짓 점 (또는 기 하 도형) 셰이더 및 픽셀 셰이더에 깊이 테스트를 추가 하 여 그림자 효과를 만듭니다.
ms.assetid: bf496dfb-d7f5-af6b-d588-501164608560
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 렌더링, 장면, 깊이 테스트, direct3d, 그림자
ms.localizationpriority: medium
ms.openlocfilehash: fd38378e0a1f4cbdf4f9ded246b4b94ed3a705eb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159247"
---
# <a name="render-the-scene-with-depth-testing"></a>깊이 테스트를 사용 하 여 장면 렌더링




꼭 짓 점 (또는 기 하 도형) 셰이더 및 픽셀 셰이더에 깊이 테스트를 추가 하 여 그림자 효과를 만듭니다. [연습의 3부: Direct3D 11의 깊이 버퍼를 사용하여 그림자 볼륨 구현](implementing-depth-buffers-for-shadow-mapping.md).

## <a name="include-transformation-for-light-frustum"></a>밝은 부분에 대 한 변환 포함


꼭 짓 점 셰이더는 각 꼭 짓 점에 대해 변환 된 밝은 공간 위치를 계산 해야 합니다. 상수 버퍼를 사용 하 여 광원 모델, 뷰 및 프로젝션 행렬을 제공 합니다. 또한이 상수 버퍼를 사용 하 여 조명 계산에 대 한 밝은 위치 및 법선을 제공할 수 있습니다. 밝은 공간에서 변형 된 위치는 깊이 테스트 중에 사용 됩니다.

```cpp
PixelShaderInput main(VertexShaderInput input)
{
    PixelShaderInput output;
    float4 pos = float4(input.pos, 1.0f);

    // Transform the vertex position into projected space.
    float4 modelPos = mul(pos, model);
    pos = mul(modelPos, view);
    pos = mul(pos, projection);
    output.pos = pos;

    // Transform the vertex position into projected space from the POV of the light.
    float4 lightSpacePos = mul(modelPos, lView);
    lightSpacePos = mul(lightSpacePos, lProjection);
    output.lightSpacePos = lightSpacePos;

    // Light ray
    float3 lRay = lPos.xyz - modelPos.xyz;
    output.lRay = lRay;
    
    // Camera ray
    output.view = eyePos.xyz - modelPos.xyz;

    // Transform the vertex normal into world space.
    float4 norm = float4(input.norm, 1.0f);
    norm = mul(norm, model);
    output.norm = norm.xyz;
    
    // Pass through the color and texture coordinates without modification.
    output.color = input.color;
    output.tex = input.tex;

    return output;
}
```

그런 다음 픽셀 셰이더는 꼭 짓 점 셰이더에 제공 된 보간된 조명 공간 위치를 사용 하 여 픽셀이 그림자 인지 여부를 테스트 합니다.

## <a name="test-whether-the-position-is-in-the-light-frustum"></a>위치가 밝은 부분에 있는지 여부를 테스트 합니다.


먼저 X 및 Y 좌표를 표준화 하 여 픽셀이 조명의 시야에 있는지 확인 합니다. 둘 다 0, 1 범위 내에 있는 경우 \[ \] 픽셀이 그림자가 될 수 있습니다. 그렇지 않으면 깊이 테스트를 건너뛸 수 있습니다. 셰이더는 [포화](/windows/desktop/direct3dhlsl/saturate) 를 호출 하 고 결과를 원래 값과 비교 하 여이를 빠르게 테스트할 수 있습니다.

```cpp
// Compute texture coordinates for the current point's location on the shadow map.
float2 shadowTexCoords;
shadowTexCoords.x = 0.5f + (input.lightSpacePos.x / input.lightSpacePos.w * 0.5f);
shadowTexCoords.y = 0.5f - (input.lightSpacePos.y / input.lightSpacePos.w * 0.5f);
float pixelDepth = input.lightSpacePos.z / input.lightSpacePos.w;

float lighting = 1;

// Check if the pixel texture coordinate is in the view frustum of the 
// light before doing any shadow work.
if ((saturate(shadowTexCoords.x) == shadowTexCoords.x) &&
    (saturate(shadowTexCoords.y) == shadowTexCoords.y) &&
    (pixelDepth > 0))
{
```

## <a name="depth-test-against-the-shadow-map"></a>그림자 맵에 대 한 깊이 테스트


샘플 비교 함수 ( [Samplecmp](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-samplecmp) 또는 [SampleCmpLevelZero](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-samplecmplevelzero))를 사용 하 여 깊이 맵에 대 한 밝은 공간의 픽셀 깊이를 테스트 합니다. 정규화 된 밝은 공간 깊이 값을 계산 하 `z / w` 고, 값을 비교 함수에 전달 합니다. 샘플러에 대 한 LessOrEqual 비교 테스트를 사용 하기 때문에 비교 테스트가 통과할 때 내장 함수는 0을 반환 합니다. 픽셀이 그림자 임을 나타냅니다.

```cpp
// Use an offset value to mitigate shadow artifacts due to imprecise 
// floating-point values (shadow acne).
//
// This is an approximation of epsilon * tan(acos(saturate(NdotL))):
float margin = acos(saturate(NdotL));
#ifdef LINEAR
// The offset can be slightly smaller with smoother shadow edges.
float epsilon = 0.0005 / margin;
#else
float epsilon = 0.001 / margin;
#endif
// Clamp epsilon to a fixed range so it doesn't go overboard.
epsilon = clamp(epsilon, 0, 0.1);

// Use the SampleCmpLevelZero Texture2D method (or SampleCmp) to sample from 
// the shadow map, just as you would with Direct3D feature level 10_0 and
// higher.  Feature level 9_1 only supports LessOrEqual, which returns 0 if
// the pixel is in the shadow.
lighting = float(shadowMap.SampleCmpLevelZero(
    shadowSampler,
    shadowTexCoords,
    pixelDepth + epsilon
    )
    );
```

## <a name="compute-lighting-in-or-out-of-shadow"></a>광선 내 또는 섀도 외부 계산


픽셀이 그림자에 없으면 픽셀 셰이더는 직접 조명을 계산 하 고 픽셀 값에 추가 해야 합니다.

```cpp
return float4(input.color * (ambient + DplusS(N, L, NdotL, input.view)), 1.f);
```

```cpp
float3 DplusS(float3 N, float3 L, float NdotL, float3 view)
{
    const float3 Kdiffuse = float3(.5f, .5f, .4f);
    const float3 Kspecular = float3(.2f, .2f, .3f);
    const float exponent = 3.f;

    // Compute the diffuse coefficient.
    float diffuseConst = saturate(NdotL);

    // Compute the diffuse lighting value.
    float3 diffuse = Kdiffuse * diffuseConst;

    // Compute the specular highlight.
    float3 R = reflect(-L, N);
    float3 V = normalize(view);
    float3 RdotV = dot(R, V);
    float3 specular = Kspecular * pow(saturate(RdotV), exponent);

    return (diffuse + specular);
}
```

그렇지 않으면 픽셀 셰이더는 앰비언트 조명을 사용 하 여 픽셀 값을 계산 해야 합니다.

```cpp
return float4(input.color * ambient, 1.f);
```

이 연습의 다음 부분에서는 [하드웨어 범위에서 섀도 맵을 지 원하는](target-a-range-of-hardware.md)방법에 대해 알아봅니다.

 

 