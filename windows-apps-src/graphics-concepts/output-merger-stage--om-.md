---
title: OM(출력 병합기) 단계
description: OM(출력 병합기) 단계는 다양한 유형의 출력 데이터(픽셀 셰이더 값, 깊이 및 스텐실 정보)를 렌더링 대상 및 깊이/스텐실 버퍼의 내용과 결합하여 최종 파이프라인 결과를 생성합니다.
ms.assetid: ED2DC4A0-2B92-47AF-884A-BFC2183C78B8
keywords:
- OM(출력 병합기) 단계
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a4e2bbac12ddd3b60b2e4dd78f37b8934a8afea8
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5938032"
---
# <a name="output-merger-om-stage"></a>OM(출력 병합기) 단계


OM(출력 병합기) 단계는 다양한 유형의 출력 데이터(픽셀 셰이더 값, 깊이 및 스텐실 정보)를 렌더링 대상 및 깊이/스텐실 버퍼의 내용과 결합하여 최종 파이프라인 결과를 생성합니다.

## <a name="span-idpurpose-and-usesspanspan-idpurpose-and-usesspanspan-idpurpose-and-usesspanpurpose-and-uses"></a><span id="Purpose-and-uses"></span><span id="purpose-and-uses"></span><span id="PURPOSE-AND-USES"></span>목적 및 사용


OM(출력 병합기) 단계는 표시되는 픽셀을 결정하고(깊이 스텐실 테스트 사용) 최종 픽셀 색상을 혼합하기 위한 최종 단계입니다.

OM 단계는 다음과 같은 조합을 사용하여 최종 렌더링된 픽셀 색상을 생성합니다.

-   파이프라인 상태
-   픽셀 셰이더가 생성하는 픽셀 데이터
-   렌더링 대상의 콘텐츠
-   깊이/스텐실 버퍼의 콘텐츠.

### <a name="span-idblending-overviewspanspan-idblending-overviewspanspan-idblending-overviewspanblending-overview"></a><span id="Blending-overview"></span><span id="blending-overview"></span><span id="BLENDING-OVERVIEW"></span>혼합 개요

혼합은 하나 이상의 픽셀 값을 조합하여 최종 픽셀 색상을 만듭니다. 다음 다이어그램은 픽셀 데이터 혼합에 관련된 프로세스를 보여 줍니다.

![데이터 혼합 작동 방식 다이어그램](images/d3d10-blend-state.png)

개념상 이 순서도가 출력 병합기 단계에서 두 번 구현되는 것으로 시각화할 수 있는데, 첫 번째는 RGB 데이터를 혼합하며 이와 동시에 두 번째가 알파 데이터를 혼합합니다. API를 사용하여 혼합 상태를 만들고 설정하는 방법은 [혼합 기능 구성](https://msdn.microsoft.com/library/windows/desktop/bb205072)을 참조하세요.

고정 함수 blend는 각 렌더링 대상에 대해 독립적으로 활성화할 수 있습니다. 그러나 blend 컨트롤 세트가 하나뿐이므로 혼합이 활성화된 모든 RenderTarget에 동일한 blend가 적용됩니다. Blend 값(BlendFactor 포함)은 항상 혼합 전에 렌더링-대상 형식 범위에 고정됩니다. 고정은 렌더링 대상별로 이루어지며, 렌더링 대상 유형을 사용합니다. 유일한 예외는 고정되지 않는 float16, float11 또는 float10 형식으로서 이 형식들의 혼합 연산은 출력 형식과 적어도 동일한 정확도/범위로 수행될 수 있습니다. 모든 경우(0.0 blend weight 포함)에 NaN과 서명된 0이 전파됩니다.

sRGB 렌더링 대상을 사용하면 런타임은 혼합을 수행하기 전에 렌더링 대상 색상을 선형 공간으로 변환합니다. 런타임은 혼합된 최종 값을 다시 sRGB 공간으로 변환한 후 렌더링 대상에 값을 다시 저장합니다.

### <a name="span-iddual-source-color-blendingspanspan-iddual-source-color-blendingspanspan-iddual-source-color-blendingspandual-source-color-blending"></a><span id="Dual-source-color-blending"></span><span id="dual-source-color-blending"></span><span id="DUAL-SOURCE-COLOR-BLENDING"></span>듀얼 소스 색상 혼합

이 기능을 통해 출력 병합기 단계는 두 가지 픽셀 셰이더 출력(o0와 o1) 모두를 슬롯 0에서 단일 렌더링 대상이 포함된 혼합 연산에 대한 입력으로 동시에 사용할 수 있습니다. 유효한 blend 연산에는 add, subtract, revsubtract가 포함됩니다. Blend 수식과 출력 쓰기 마스크는 픽셀 셰이더가 출력하는 구성 요소를 지정합니다. 추가 구성 요소는 무시됩니다.

다른 픽셀 셰이더 출력(o2, o3 등)에 대한 쓰기는 정의되어 있지 않습니다. 슬롯 0에 바인딩되어 있지 않은 렌더링 대상에는 쓸 수 없습니다. oDepth 쓰기는 듀얼 소스 색 혼합 동안 유효합니다.

### <a name="span-iddepth-stencil-testspanspan-iddepth-stencil-testspanspan-iddepth-stencil-testspandepth-stencil-testing-overview"></a><span id="Depth-Stencil-Test"></span><span id="depth-stencil-test"></span><span id="DEPTH-STENCIL-TEST"></span>깊이 스텐실 테스트 개요

텍스처 리소스로 생성되는 깊이 스텐실 버퍼는 깊이 데이터와 스텐실 데이터를 모두 포함할 수 있습니다. 깊이 데이터는 카메라에 가장 가까운 픽셀을 결정하는 데 사용되고, 스텐실 데이터는 업데이트할 수 있는 픽셀을 마스크하는 데 사용됩니다. 결국 출력 병합기 단계는 깊이 값과 스텐실 값을 모두 사용하여 픽셀을 그려야 하는지 여부를 결정합니다. 다음 다이어그램은 깊이 스텐실 테스트를 수행하는 방법을 개념적으로 보여 줍니다.

![깊이 스텐실 테스트의 작동 방식 다이어그램](images/d3d10-depth-stencil-test.png)

깊이 스텐실 테스트를 구성하려면 [깊이 스텐실 기능 구성](configuring-depth-stencil-functionality.md)을 참조하세요. 깊이 스텐실 개체는 깊이 스텐실 상태를 캡슐화합니다. 응용 프로그램이 깊이 스텐실 상태를 지정할 수 있으며 그렇지 않을 경우, OM 단계는 기본값을 사용합니다. 다중 샘플링이 비활성화된 경우, 픽셀별 혼합 연산이 수행됩니다. 다중 샘플링이 활성화된 경우, 다중 샘플별 혼합이 이루어집니다.

깊이 버퍼를 사용하여 어떤 픽셀을 그릴지 결정하는 프로세스를 깊이 버퍼링이라고 하며, 경우에 따라 z 버퍼링이라고도 합니다.

깊이 값이 출력 병합기 단계에 도달하면(깊이 값이 보간에서 오건 픽셀 셰이더에서 오건 상관없이) 깊이 값은 깊이 버퍼의 형식/정확도에 따라 부동 소수점 규칙을 사용하여 항상 고정됩니다: z min(Viewport.MaxDepth,max(Viewport.MinDepth,z)) 고정 후 기본 깊이 버퍼 값을 기준으로 깊이 값을 비교합니다(DepthFunc 사용). 바인딩된 깊이 버퍼가 없으면 깊이 테스트는 항상 통과됩니다.

깊이 버퍼 형식에 스텐실 구성 요소가 없거나 바인딩된 깊이 버퍼가 없는 경우에는 스텐실 테스트가 항상 통과됩니다.

한 번에 하나의 깊이/스텐실 버퍼만 활성화될 수 있습니다. 바인딩된 리소스 보기는 깊이/스텐실 보기와 일치해야 합니다(같은 크기와 치수). 이것은 리소스 크기가 아니라 보기 크기가 일치해야 함을 뜻합니다.

### <a name="span-idsample-maskspanspan-idsample-maskspanspan-idsample-maskspansample-mask-overview"></a><span id="Sample-Mask"></span><span id="sample-mask"></span><span id="SAMPLE-MASK"></span>샘플 마스크 개요

샘플 마스크는 활성화된 렌더링 대상에서 업데이트되는 샘플을 결정하는 32비트 다중 샘플 커버리지 마스크입니다. 하나의 샘플 마스크만 허용됩니다. 샘플 마스크 내 비트의 리소스 내 샘플에 대한 매핑은 사용자가 정의합니다. n 샘플 렌더링의 경우, 샘플 마스크의 첫 번째 n비트(LSB의 비트)가 사용됩니다(최대 비트 수인 경우 32비트).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>입력


OM(출력 병합기) 단계는 다음과 같은 조합을 사용하여 최종 렌더링된 픽셀 색상을 생성합니다.

-   파이프라인 상태
-   픽셀 셰이더가 생성하는 픽셀 데이터
-   렌더링 대상의 콘텐츠
-   깊이/스텐실 버퍼의 콘텐츠.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>출력


### <a name="span-idoutput-write-mask-overviewspanspan-idoutput-write-mask-overviewspanspan-idoutput-write-mask-overviewspanoutput-write-mask-overview"></a><span id="Output-write-mask-overview"></span><span id="output-write-mask-overview"></span><span id="OUTPUT-WRITE-MASK-OVERVIEW"></span>출력 쓰기 마스크 개요

출력 쓰기 마스크를 사용하여 렌더링 대상에 쓸 수 있는 데이터를 (구성 요소별로) 제어합니다.

### <a name="span-idmultiple-render-targets-overviewspanspan-idmultiple-render-targets-overviewspanspan-idmultiple-render-targets-overviewspanmultiple-render-targets-overview"></a><span id="Multiple-render-targets-overview"></span><span id="multiple-render-targets-overview"></span><span id="MULTIPLE-RENDER-TARGETS-OVERVIEW"></span>다중 렌더링 대상 개요

픽셀 셰이더를 사용하여 8개 이상의 개별적 렌더링 대상을 렌더링할 수 있으며, 렌더링 대상은 모두 동일한 유형(버퍼, Texture1D, Texture1DArray 등)이어야 합니다. 또한 모든 렌더링 대상은 모든 치수(너비, 높이, 깊이, 배열 크기, 샘플 수)의 크기가 동일해야 합니다. 렌더링 대상마다 데이터 형식이 다를 수 있습니다.

렌더링 대상 슬롯의 어떤 조합도 사용할 수 있습니다(최대 8). 하지만 리소스 보기는 다중 렌더링 대상 슬롯에 동시에 바인딩할 수 없습니다. 리소스를 동시에 사용하지 않는 한 보기는 다시 사용할 수 있습니다.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>이 섹션의 내용


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">항목</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="configuring-depth-stencil-functionality.md">깊이 스텐실 기능 구성</a></p></td>
<td align="left"><p>이 섹션에서는 깊이 스텐실 버퍼와 출력 병합기 단계의 깊이 스텐실 상태를 설정하는 단계에 대해 설명합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[그래픽 파이프라인](graphics-pipeline.md)

 

 




