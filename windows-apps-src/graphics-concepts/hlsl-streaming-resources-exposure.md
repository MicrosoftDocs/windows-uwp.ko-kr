---
title: HLSL 스트리밍 리소스 노출
description: 셰이더 모델 5에서 스트리밍 리소스를 지원 하려면 특정 Microsoft HLSL (High Level Shader Language) 구문이 필요 합니다.
ms.assetid: 00A40D82-0565-43DC-82AB-0675B7E772E3
keywords:
- HLSL 스트리밍 리소스 노출
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: decb0ff2d791417326a70b06c0fdd6a25ea2d119
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173087"
---
# <a name="hlsl-streaming-resources-exposure"></a>HLSL 스트리밍 리소스 노출


[셰이더 모델 5](/windows/desktop/direct3dhlsl/d3d11-graphics-reference-sm5)에서 스트리밍 리소스를 지원 하려면 특정 Microsoft HLSL (High Level Shader Language) 구문이 필요 합니다.

셰이더 모델 5에 대 한 HLSL 구문은 스트리밍 리소스를 지 원하는 장치 에서만 사용할 수 있습니다. 다음 표에서 리소스를 스트리밍하는 데 관련 된 각 HLSL 메서드는 하나 (피드백) 또는 2 (이 순서 대로 클램프 및 피드백) 추가 선택적 매개 변수를 허용 합니다. 예를 들어 **샘플** 메서드는 다음과 같습니다.

**샘플 (샘플러, 위치 \[ , 오프셋 \[ , 클램프 \[ , 피드백 \] \] \] )**

**샘플** 메서드의 예는 [**Texture2D (S, float, int, float, uint)입니다.**](/windows/desktop/direct3dhlsl/t2darray-sample-s-float-int-float-uint-)

Offset, 클램프 및 피드백 매개 변수는 선택 사항입니다. 필요한 모든 선택적 매개 변수를 지정 해야 합니다 .이 매개 변수는 기본 함수 인수에 대 한 c + + 규칙과 일치 합니다. 예를 들어 피드백 상태가 필요한 경우 논리적으로 필요 하지 않은 경우에도 offset 및 클램프 매개 변수를 **Sample**에 명시적으로 제공 해야 합니다.

클램프 매개 변수는 스칼라 float 값입니다. 클램프 = 0.0 f의 리터럴 값은 클램프 작업이 수행 되지 않음을 나타냅니다.

피드백 매개 변수는 메모리 액세스 쿼리 내장 [**CheckAccessFullyMapped**](/windows/desktop/direct3dhlsl/checkaccessfullymapped) 함수에 제공할 수 있는 **uint** 변수입니다. 피드백 매개 변수의 값을 수정 하거나 해석 해서는 안 됩니다. 그러나 컴파일러는 값을 수정 했는지 여부를 검색 하는 고급 분석과 진단을 제공 하지 않습니다.

[**CheckAccessFullyMapped**](/windows/desktop/direct3dhlsl/checkaccessfullymapped)의 구문은 다음과 같습니다.

**bool CheckAccessFullyMapped (uint FeedbackVar);**

[**CheckAccessFullyMapped**](/windows/desktop/direct3dhlsl/checkaccessfullymapped) 는 *FeedbackVar* 값을 해석 하 고 액세스 하는 모든 데이터가 리소스에서 매핑된 경우 true를 반환 합니다. 그렇지 않으면 **CheckAccessFullyMapped** 는 false를 반환 합니다.

클램프 또는 피드백 매개 변수가 있으면 컴파일러가 기본 명령의 변형을 내보냅니다. 예를 들어 스트리밍 리소스의 샘플은 명령을 생성 `sample_cl_s` 합니다.

클램프 나 피드백이 모두 지정 되지 않은 경우 컴파일러는 기본 명령을 내보내 현재 동작이 변경 되지 않도록 합니다.

0.0 f의 클램프 값은 클램프를 수행 하지 않음을 나타냅니다. 따라서 드라이버 컴파일러는 대상 하드웨어에 대 한 지침을 추가로 조정할 수 있습니다. 사용자 피드백이 명령에서 NULL 인 경우 피드백은 사용 되지 않습니다. 따라서 드라이버 컴파일러는 대상 아키텍처에 대 한 지침을 추가로 조정할 수 있습니다.

HLSL 컴파일러에서 클램프가 0.0 f이 고 피드백이 사용 되지 않는 것으로 유추 되는 경우 컴파일러는 해당 하는 기본 명령 (예: 대신)을 내보냅니다 `sample` `sample_cl_s` .

스트리밍 리소스 액세스가 구조화 된 리소스의 경우와 같이 몇 가지 구성 바이트 코드 명령으로 구성 된 경우 컴파일러는 또는 작업을 통해 개별 피드백 값을 집계 하 여 최종 피드백 값을 생성 합니다. 따라서 이러한 복잡 한 액세스를 위한 단일 피드백 값이 표시 됩니다.

사용자 의견 및/또는 클램프를 지원 하도록 변경 된 HLSL 메서드의 요약 테이블입니다. 이러한 모든 작업은 모든 차원의 타일 및 비 스트리밍 리소스에서 작동 합니다. 비 스트리밍 리소스는 항상 완전히 매핑되도록 표시 됩니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><a href="/windows/desktop/direct3dhlsl/d3d11-graphics-reference-sm5-objects">HLSL 개체</a> </th>
<th align="left">사용자 의견 옵션 (*)을 사용 하는 내장 메서드에는 클램프 옵션도 있습니다.</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>RW Texture2D</p>
<p>RW Texture2DArray</p>
<p>TextureCUBE</p>
<p>TextureCUBEArray</p></td>
<td align="left"><p>수집</p>
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
<td align="left"><p>RW Texture1D</p>
<p>RW Texture1DArray</p>
<p>RW Texture2D</p>
<p>RW Texture2DArray</p>
<p>RW Texture3D</p>
<p>TextureCUBE</p>
<p>TextureCUBEArray</p></td>
<td align="left"><p>샘플이</p>
<p>SampleBias*</p>
<p>SampleCmp *</p>
<p>SampleCmpLevelZero</p>
<p>SampleGrad*</p>
<p>SampleLevel</p></td>
</tr>
<tr class="odd">
<td align="left"><p>RW Texture1D</p>
<p>RW Texture1DArray</p>
<p>RW Texture2D</p>
<p>Texture2DMS</p>
<p>RW Texture2DArray</p>
<p>Texture2DArrayMS</p>
<p>RW Texture3D</p>
<p>RW 버퍼</p>
<p>RW ByteAddressBuffer</p>
<p>RW StructuredBuffer</p></td>
<td align="left">로드</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스에 대한 파이프라인 액세스](pipeline-access-to-streaming-resources.md)

 

 