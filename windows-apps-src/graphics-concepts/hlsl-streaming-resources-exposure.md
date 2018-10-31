---
title: HLSL 스트리밍 리소스 노출
description: 셰이더 모델 5에서 스트리밍 리소스를 지원하려면 특정 Microsoft HLSL(High Level Shader Language) 구문이 필요합니다.
ms.assetid: 00A40D82-0565-43DC-82AB-0675B7E772E3
keywords:
- HLSL 스트리밍 리소스 노출
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a8523f4895c541ffb3b92ee00d5b62c57343ae00
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5834567"
---
# <a name="hlsl-streaming-resources-exposure"></a>HLSL 스트리밍 리소스 노출


[셰이더 모델 5](https://msdn.microsoft.com/library/windows/desktop/ff471356)에서 스트리밍 리소스를 지원하려면 특정 Microsoft HLSL(High Level Shader Language) 구문이 필요합니다.

셰이더 모델 5용 HLSL 구문은 스트리밍 리소스를 지원하는 디바이스에서만 허용됩니다. 다음 표의 스트리밍 리소스용 각 관련 HLSL 메서드는 하나(피드백) 또는 두 개(순서대로 클램프, 피드백)의 선택적 추가 매개 변수를 받습니다. 예를 들어 **Sample** 메서드는 다음과 같습니다.

**Sample(샘플러, 위치 \[, 오프셋 \[, 클램프 \[, 피드백\] \] \])**

**Sample** 메서드의 예는 [**Texture2D.Sample(S,float,int,float,uint)**](https://msdn.microsoft.com/library/windows/desktop/dn393787)입니다.

오프셋, 클램프 및 피드백 매개 변수는 선택 사항입니다. 하나만 필요하더라도 선택적 매개 변수를 모두 지정해야 하며, 기본 함수 인수에 대한 C++ 규칙을 지켜야 합니다. 예를 들어, 피드백 상태가 필요한 경우 논리적으로 필요하지 않더라도 오프셋 및 클램프 매개 변수도 **Sample**에 지정해야 합니다.

클램프 매개 변수는 스칼라 부동 소수점 값입니다. 클램프의 리터럴 값은 0.0f로, 클램프 작업이 수행되지 않음을 나타냅니다.

피드백 매개 변수는 메모리 액세스 쿼리 내장 함수 [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083)에 제공할 수 있는 **uint** 변수입니다. 피드백 매개 변수의 값을 수정하거나 해석해서는 안 되지만, 컴파일러는 값이 수정되었는지 감지할 수 있는 고급 분석 및 진단 기능을 제공하지는 않습니다.

[**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083)의 구문은 다음과 같습니다.

**bool CheckAccessFullyMapped(in uint FeedbackVar);**

[**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083)는 *FeedbackVar* 값을 해석하여 액세스하는 모든 데이터가 리소스에 매핑되었으면 true를 반환하며, 그렇지 않으면 **CheckAccessFullyMapped**는 false를 반환합니다.

클램프 또는 피드백 매개 변수가 있으면 컴파일러는 기본 명령을 변형하여 반환합니다. 예를 들어, 스트리밍 리소스의 샘플은 `sample_cl_s` 명령을 생성합니다.

클램프와 피드백을 모두 지정하지 않으면, 컴파일러는 기본 명령을 반환하므로 현재 동작이 변경되지 않습니다.

클램프 값이 0.0f이면 클램프가 수행되지 않는 것을 나타내므로, 드라이버 컴파일러도 대상 하드웨어에 맞게 명령을 변경할 수 있습니다. 피드백이 명령에서 NULL 레지스터이면 피드백이 사용되지 않습니다. 따라서 드라이버 컴파일러가 대상 아키텍처에 맞게 명령을 변경할 수 있습니다.

HLSL 컴파일러가 클램프를 0.0f으로, 피드백이 사용되지 않는 것으로 해석하면, 컴파일러는 해당 기본 명령을 반환(예를 들어, `sample_cl_s` 대신 `sample`)합니다.

스트리밍 리소스 액세스는 여러 구성 바이트 코드 명령으로 구성됩니다. 예를 들어, 구조적 리소스의 경우 컴파일러가 OR 연산을 통해 개별 피드백 값을 집계하여 최종 피드백 값을 생성합니다. 따라서 이러한 복잡한 액세스에 대해 하나의 피드백 값만 보게 됩니다.

이는 피드백 및/또는 클램프를 지원하도록 변경되는 HLSL 메서드의 요약 표입니다. 이는 모든 차원의 타일식 및 비 스트리밍 리소스에 모두 작동합니다. 비 스트리밍 리소스는 항상 완전히 매핑된 것으로 표시됩니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/ff471359">HLSL 개체</a> </th>
<th align="left">피드백 옵션이 있는 기본 메서드(*) - 클램프 옵션이 있음</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[RW]Texture2D</p>
<p>[RW]Texture2DArray</p>
<p>TextureCUBE</p>
<p>TextureCUBEArray</p></td>
<td align="left"><p>Gather</p>
<p>GatherRed</p>
<p>GatherGreen</p>
<p>GatherBlue</p>
<p>GatherAlpha</p>
<p>GatherCmp</p>
<p>GatherCmpRed</p>
<p>GatherCmpGreen</p>
<p>GatherCmpBlue</p>
<p>GatherCmpAlpha</p></td>
</tr>
<tr class="even">
<td align="left"><p>[RW]Texture1D</p>
<p>[RW]Texture1DArray</p>
<p>[RW]Texture2D</p>
<p>[RW]Texture2DArray</p>
<p>[RW]Texture3D</p>
<p>TextureCUBE</p>
<p>TextureCUBEArray</p></td>
<td align="left"><p>Sample*</p>
<p>SampleBias*</p>
<p>SampleCmp*</p>
<p>SampleCmpLevelZero</p>
<p>SampleGrad*</p>
<p>SampleLevel</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[RW]Texture1D</p>
<p>[RW]Texture1DArray</p>
<p>[RW]Texture2D</p>
<p>Texture2DMS</p>
<p>[RW]Texture2DArray</p>
<p>Texture2DArrayMS</p>
<p>[RW]Texture3D</p>
<p>[RW]Buffer</p>
<p>[RW]ByteAddressBuffer</p>
<p>[RW]StructuredBuffer</p></td>
<td align="left">로드</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스에 대한 파이프라인 액세스](pipeline-access-to-streaming-resources.md)

 

 




