---
title: OM(출력 병합기) 단계
description: OM (출력 병합기) 단계에서는 다양 한 유형의 출력 데이터 (픽셀 셰이더 값, 깊이 및 스텐실 정보)를 렌더링 대상의 내용과 깊이/스텐실 버퍼의 내용과 결합 하 여 최종 파이프라인 결과를 생성 합니다.
ms.assetid: ED2DC4A0-2B92-47AF-884A-BFC2183C78B8
keywords:
- OM(출력 병합기) 단계
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6478eaa2dadac74c22e5959623f03547f18babfc
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156317"
---
# <a name="output-merger-om-stage"></a>OM(출력 병합기) 단계


OM (출력 병합기) 단계에서는 다양 한 유형의 출력 데이터 (픽셀 셰이더 값, 깊이 및 스텐실 정보)를 렌더링 대상의 내용과 깊이/스텐실 버퍼의 내용과 결합 하 여 최종 파이프라인 결과를 생성 합니다.

## <a name="span-idpurpose-and-usesspanspan-idpurpose-and-usesspanspan-idpurpose-and-usesspanpurpose-and-uses"></a><span id="Purpose-and-uses"></span><span id="purpose-and-uses"></span><span id="PURPOSE-AND-USES"></span>목적 및 사용


OM (출력 병합기) 단계는 표시 되는 픽셀 (깊이 스텐실 테스트 포함)을 확인 하 고 최종 픽셀 색을 혼합 하는 최종 단계입니다.

OM 스테이지는 다음의 조합을 사용 하 여 렌더링 된 최종 픽셀 색을 생성 합니다.

-   파이프라인 상태
-   픽셀 셰이더에 의해 생성 된 픽셀 데이터입니다.
-   렌더링 대상의 콘텐츠
-   깊이/스텐실 버퍼의 내용입니다.

### <a name="span-idblending-overviewspanspan-idblending-overviewspanspan-idblending-overviewspanblending-overview"></a><span id="Blending-overview"></span><span id="blending-overview"></span><span id="BLENDING-OVERVIEW"></span>혼합 개요

혼합은 하나 이상의 픽셀 값을 결합 하 여 최종 픽셀 색을 만듭니다. 다음 다이어그램에서는 픽셀 데이터를 혼합 하는 것과 관련 된 프로세스를 보여 줍니다.

![데이터 혼합 작동 방식을 보여 주는 다이어그램](images/d3d10-blend-state.png)

개념적으로이 흐름 차트는 출력 병합기 단계에서 두 번 구현 됩니다. 첫 번째는 RGB 데이터를 혼합 하 고 두 번째는 알파 데이터를 혼합 하는 것입니다. API를 사용 하 여 blend 상태를 만들고 설정 하는 방법을 보려면 [혼합 기능 구성](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-blend-state)을 참조 하세요.

고정 함수 blend는 각 렌더링 대상에 대해 독립적으로 사용 하도록 설정할 수 있습니다. 그러나 혼합 컨트롤 집합은 하나만 있으므로 혼합이 설정 된 모든 RenderTargets에 동일한 blend가 적용 됩니다. Blend 값 (BlendFactor 포함)은 항상 혼합 하기 전에 렌더링 대상 형식의 범위에 고정 됩니다. 고정은 렌더링 대상에 따라 렌더링 대상 유형을 준수 하 여 수행 됩니다. 유일한 예외는 고정 되지 않는 float16, float11 또는 float10 형식에 대 한 것입니다. 따라서 이러한 형식의 blend 연산을 출력 형식으로 최소 정밀도/범위를 사용 하 여 수행할 수 있습니다. NaN의 및 부호 있는 0은 모든 경우 (0.0 blend 가중치 포함)에 대해 전파 됩니다.

SRGB 렌더링 대상을 사용 하는 경우 런타임은 혼합을 수행 하기 전에 렌더링 대상 색을 선형 공간으로 변환 합니다. 런타임은 값을 렌더링 대상으로 다시 저장 하기 전에 최종 블렌드된 값을 다시 sRGB 공간으로 변환 합니다.

### <a name="span-iddual-source-color-blendingspanspan-iddual-source-color-blendingspanspan-iddual-source-color-blendingspandual-source-color-blending"></a><span id="Dual-source-color-blending"></span><span id="dual-source-color-blending"></span><span id="DUAL-SOURCE-COLOR-BLENDING"></span>이중 소스 색 혼합

이 기능을 사용 하면 출력 병합기 단계에서 픽셀 셰이더 출력 (o0 및 o1)을 모두 슬롯 0의 단일 렌더링 대상이 있는 혼합 작업에 대 한 입력으로 동시에 사용할 수 있습니다. 유효한 blend 작업에는 add, 빼기 및 revsubtract가 포함 됩니다. Blend 수식과 출력 쓰기 마스크는 픽셀 셰이더가 출력 하는 구성 요소를 지정 합니다. 추가 구성 요소는 무시 됩니다.

다른 픽셀 셰이더 출력 (o2, o3 등)에 대 한 쓰기는 정의 되지 않습니다. 슬롯 0에 바인딩되지 않은 경우 렌더링 대상에 쓸 수 없습니다. 이중 소스 색 혼합 중에는 oDepth 쓰기가 유효 합니다.

### <a name="span-iddepth-stencil-testspanspan-iddepth-stencil-testspanspan-iddepth-stencil-testspandepth-stencil-testing-overview"></a><span id="Depth-Stencil-Test"></span><span id="depth-stencil-test"></span><span id="DEPTH-STENCIL-TEST"></span>깊이 스텐실 테스트 개요

질감 리소스로 만들어지는 깊이 스텐실 버퍼에는 깊이 데이터와 스텐실 데이터가 모두 포함 될 수 있습니다. 깊이 데이터는 카메라와 가장 가까운 픽셀을 확인 하는 데 사용 되 고 스텐실 데이터는 업데이트할 수 있는 픽셀을 마스킹하는 데 사용 됩니다. 궁극적으로 깊이 및 스텐실 값 데이터는 모두 출력-병합기 단계에서 사용 되어 픽셀을 그려야 하는지 여부를 확인 합니다. 다음 다이어그램에서는 깊이 스텐실 테스트를 수행 하는 방법을 개념적으로 보여 줍니다.

![깊이 스텐실 테스트가 작동 하는 방식을 보여 주는 다이어그램](images/d3d10-depth-stencil-test.png)

깊이 스텐실 테스트를 구성 하려면 [깊이 스텐실 기능 구성](configuring-depth-stencil-functionality.md)을 참조 하세요. 깊이 스텐실 개체는 깊이 스텐실 상태를 캡슐화 합니다. 응용 프로그램에서 깊이 스텐실 상태를 지정할 수 있습니다. 또는 OM 단계에서 기본값을 사용 합니다. 다중 샘플링을 사용 하지 않도록 설정한 경우에는 픽셀 단위로 혼합 작업이 수행 됩니다. 다중 샘플링을 사용 하는 경우 혼합은 다중 샘플 기준으로 발생 합니다.

깊이 버퍼를 사용 하 여 그려야 하는 픽셀을 결정 하는 프로세스를 깊이 버퍼링 이라고 하며, 때로는 z 버퍼링이 라고도 합니다.

깊이 값이 출력-병합기 단계에 도달 하면 (보간에서 제공 되는지 픽셀 셰이더에 따라), 부동 소수점 규칙을 사용 하 여 깊이 버퍼의 형식/전체 자릿수에 따라 항상 고정: z = min (Viewport. MaxDepth, max (뷰포트))입니다. 고정 후에는 깊이 값이 기존 깊이 버퍼 값과 비교 됩니다 (DepthFunc 사용). 수준 버퍼가 바인딩되지 않은 경우에는 깊이 테스트가 항상 통과 합니다.

깊이 버퍼 형식의 스텐실 구성 요소가 없거나 깊이 버퍼가 바인딩되지 않은 경우 스텐실 테스트는 항상를 전달 합니다.

한 번에 하나의 깊이/스텐실 버퍼만 활성화 될 수 있습니다. 모든 바인딩된 리소스 뷰는 깊이/스텐실 보기와 일치 해야 합니다 (크기와 차원). 이는 리소스 크기가 일치 해야 한다는 것을 의미 하지 않습니다. 즉, 보기 크기가 일치 해야 합니다.

### <a name="span-idsample-maskspanspan-idsample-maskspanspan-idsample-maskspansample-mask-overview"></a><span id="Sample-Mask"></span><span id="sample-mask"></span><span id="SAMPLE-MASK"></span>샘플 마스크 개요

샘플 마스크는 활성 렌더링 대상에서 업데이트 되는 샘플을 결정 하는 32 비트 다중 샘플 검사 마스크입니다. 샘플 마스크는 하나만 허용 됩니다. 리소스의 샘플에 대 한 샘플 마스크의 비트 매핑은 사용자에 의해 정의 됩니다. N-샘플 렌더링의 경우 샘플 마스크의 처음 n 비트 (LSB)가 사용 됩니다 (최대 비트 수 32 비트).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>입력


OM (출력 병합기) 단계는 다음의 조합을 사용 하 여 마지막으로 렌더링 된 픽셀 색을 생성 합니다.

-   파이프라인 상태
-   픽셀 셰이더에 의해 생성 된 픽셀 데이터입니다.
-   렌더링 대상의 콘텐츠
-   깊이/스텐실 버퍼의 내용입니다.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>출력


### <a name="span-idoutput-write-mask-overviewspanspan-idoutput-write-mask-overviewspanspan-idoutput-write-mask-overviewspanoutput-write-mask-overview"></a><span id="Output-write-mask-overview"></span><span id="output-write-mask-overview"></span><span id="OUTPUT-WRITE-MASK-OVERVIEW"></span>출력-쓰기 마스크 개요

출력 쓰기 마스크를 사용 하 여 렌더링 대상에 쓸 수 있는 데이터를 제어 하는 (구성 요소별)

### <a name="span-idmultiple-render-targets-overviewspanspan-idmultiple-render-targets-overviewspanspan-idmultiple-render-targets-overviewspanmultiple-render-targets-overview"></a><span id="Multiple-render-targets-overview"></span><span id="multiple-render-targets-overview"></span><span id="MULTIPLE-RENDER-TARGETS-OVERVIEW"></span>여러 렌더링 대상 개요

픽셀 셰이더는 최소 8 개의 개별 렌더링 대상으로 렌더링 하는 데 사용할 수 있습니다 .이는 모두 동일한 형식 (buffer, Texture1D, Texture1DArray 등) 이어야 합니다. 또한 모든 렌더링 대상의 크기는 모든 차원 (너비, 높이, 깊이, 배열 크기, 샘플 개수)에서 동일 해야 합니다. 각 렌더링 대상의 데이터 형식이 다를 수 있습니다.

렌더링 대상 슬롯을 임의로 조합 하 여 사용할 수 있습니다 (최대 8 개). 그러나 리소스 뷰를 여러 렌더링 대상 슬롯에 동시에 바인딩할 수는 없습니다. 리소스를 동시에 사용 하지 않는 한 뷰를 다시 사용할 수 있습니다.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>섹션 내용


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
<td align="left"><p>이 섹션에서는 출력-병합기 단계에 대 한 깊이 스텐실 버퍼 및 깊이 스텐실 상태를 설정 하는 단계에 대해 설명 합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[그래픽 파이프라인](graphics-pipeline.md)

 

 