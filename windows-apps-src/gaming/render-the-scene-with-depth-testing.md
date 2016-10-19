---
author: mtoepke
title: "깊이 테스트로 장면 렌더링"
description: "꼭짓점(또는 기하 도형) 셰이더와 픽셀 셰이더에 깊이 테스트를 추가하여 그림자 효과를 만듭니다."
ms.assetid: bf496dfb-d7f5-af6b-d588-501164608560
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 6351cc9f6efe0d4bffb54961624a35b4a9f4136a

---

# 깊이 테스트로 장면 렌더링


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


꼭짓점(또는 기하 도형) 셰이더와 픽셀 셰이더에 깊이 테스트를 추가하여 그림자 효과를 만듭니다. [연습의 3부: Direct3D 11의 깊이 버퍼를 사용하여 그림자 볼륨 구현](implementing-depth-buffers-for-shadow-mapping.md).

## 광원 절두체에 대한 변환 포함


꼭짓점 셰이더는 각 꼭짓점에 대한 변형된 광원 공간 위치를 계산해야 합니다. 상수 버퍼를 사용하는 광원 공간 모델, 보기 및 프로젝션 매트릭스를 제공합니다. 또한 이 상수 버퍼를 사용하여 조명 계산을 위해 광원 위치 및 법선을 제공할 수 있습니다. 깊이 테스트 중 광원 공간에서 변환된 위치를 사용합니다.

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

그 다음에 픽셀 셰이더는 픽셀이 그림자에 있는지 여부를 테스트하기 위해 꼭짓점 셰이더에서 제공하는 보간된 광원 공간 위치를 사용합니다.

## 위치가 광원 절두체에 있는지 여부 테스트


먼저 X 및 Y 좌표를 정규화하여 픽셀이 광원의 보기 절두체에 있는지 확인합니다. 둘 다 \[0, 1\] 범위 내에 있는 경우 픽셀이 그림자에 있을 가능성이 있습니다. 그렇지 않으면 깊이 테스트를 건너뛸 수 있습니다. 셰이더는 [Saturate](https://msdn.microsoft.com/library/windows/desktop/hh447231)를 호출하고 원래 값과 결과를 비교하여 빠르게 이를 테스트할 수 있습니다.

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

## 그림자 맵에 대한 깊이 테스트


샘플 비교 함수([SampleCmp](https://msdn.microsoft.com/library/windows/desktop/bb509696) 또는 [SampleCmpLevelZero](https://msdn.microsoft.com/library/windows/desktop/bb509697))를 사용하여 깊이 맵에 대해 광원 공간에서 픽셀의 깊이를 테스트합니다. `z / w`를 사용하여 정규화된 광원 공간 깊이 값을 계산하고 값을 비교 함수에 전달합니다. 여기서는 샘플러에 대해 LessOrEqual 비교 테스트를 사용하므로 비교 테스트에 통과하면 내장 함수에서 0을 반환하며, 이는 픽셀이 그림자에 있다는 것을 나타냅니다.

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

## 그림자 안이나 밖에서 조명 계산


픽셀이 그림자에 없는 경우 픽셀 셰이더가 직접 조명을 계산하고 픽셀 값에 이 값을 추가해야 합니다.

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

그렇지 않으면 픽셀 셰이더가 자연광을 사용하여 픽셀 값을 계산해야 합니다.

```cpp
return float4(input.color * ambient, 1.f);
```

이 연습의 다음 부분에서 [다양한 하드웨어의 그림자 맵 지원](target-a-range-of-hardware.md) 방법에 대해 알아보세요.

 

 







<!--HONumber=Aug16_HO3-->


