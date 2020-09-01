---
title: 입력 어셈블러 (IA) 단계
description: 입력 어셈블러 (IA) 단계는 아직 처리 되지 않은 기본 형식으로 처리를 줄임으로써 셰이더를 보다 효율적으로 만드는 데 도움이 되는 의미 체계 Id를 포함 하 여 파이프라인에 기본 및 인접 데이터를 제공 합니다.
ms.assetid: AF1DC611-C872-47F1-BF1A-92C68C8903E6
keywords:
- 입력 어셈블러 (IA) 단계
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8b1ba0205a837383e1c646664c0550e055227412
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173067"
---
# <a name="input-assembler-ia-stage"></a>입력 어셈블러 (IA) 단계


입력 어셈블러 (IA) 단계는 아직 처리 되지 않은 기본 형식으로 처리를 줄임으로써 셰이더를 보다 효율적으로 만드는 데 도움이 되는 의미 체계 Id를 포함 하 여 파이프라인에 기본 및 인접 데이터를 제공 합니다.

## <a name="span-idpurpose-and-usesspanspan-idpurpose-and-usesspanspan-idpurpose-and-usesspanpurpose-and-uses"></a><span id="Purpose-and-uses"></span><span id="purpose-and-uses"></span><span id="PURPOSE-AND-USES"></span>목적 및 사용


입력 어셈블러 (IA) 단계는 사용자가 채운 버퍼에서 기본 데이터 (요소, 선 및 삼각형)를 읽고 데이터를 다른 파이프라인 단계에서 사용할 기본 형식으로 결합 하 고, 셰이더를 보다 효율적으로 만드는 데 도움이 되는 [시스템 생성 값](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics) 을 연결 하는 것입니다. 시스템에서 생성 된 값은 의미 체계가 라고도 하는 텍스트 문자열입니다. 프로그래밍 가능한 셰이더 단계는 시스템에서 생성 되는 값 (예: 기본 ID, 인스턴스 ID 또는 꼭 짓 점 ID)을 사용 하는 공통 셰이더 코어에서 구성 되므로 셰이더 단계에서 아직 처리 되지 않은 기본 형식, 인스턴스 또는 꼭 짓 점에 대 한 처리를 줄일 수 있습니다.

IA 단계는 여러 가지 [기본 형식](primitive-topologies.md) (예: 선 목록, 삼각형 스트립 또는 인접 한 기본 형식)으로 꼭 짓 점을 어셈블할 수 있습니다. 인접도 있는 삼각형 목록과 같은 기본 형식 및 인접 한 줄 목록은 [기호 셰이더 (GS) 단계](geometry-shader-stage--gs-.md)를 지원 합니다.

인접 정보는 기 하 도형 셰이더의 응용 프로그램에만 표시 됩니다. 예를 들어 인접도를 포함 하는 삼각형을 사용 하 여 기 하 도형 셰이더를 호출 하는 경우 입력 데이터에는 각 삼각형에 대해 3 개의 꼭지점이 포함 되며 삼각형 당 인접 한 데이터의 경우 3 개의 꼭지점이

인접 한 데이터를 출력 하도록 IA 단계를 요청 하는 경우 입력 데이터는 인접 데이터를 포함 해야 합니다. 이 경우 꼭 짓 점 (중복 제거 삼각형)을 제공 하거나 꼭 짓 점이 있는지 여부에 관계 없이 꼭 짓 점 특성 중 하나에 플래그를 지정 해야 할 수 있습니다. 이는 래스터 라이저에서 검색 및 처리 해야 하는 것은 아니지만 래스터 라이저 고르기는 래스터 라이저에서 발생 합니다.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>입력


IA 단계는 사용자가 채운 버퍼에서 메모리의 데이터 (요소, 선 및/또는 삼각형)를 읽습니다.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>출력


IA 단계는 데이터를 기본 형식으로 어셈블한 후 시스템 생성 값을 연결 하 고, [꼭 짓 점 셰이더 (VS) 단계](vertex-shader-stage--vs-.md) 와 기타 파이프라인 단계에서 사용 되는 기본 형식으로 출력 합니다.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>섹션 항목


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">항목</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="primitive-topologies.md">기본 토폴로지</a></p></td>
<td align="left"><p>Direct3D는 점 목록, 선 목록 및 삼각형 스트립 등 파이프라인이 파이프라인에서 해석 되 고 렌더링 되는 방식을 정의 하는 몇 가지 기본 토폴로지를 지원 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="using-system-generated-values.md">시스템에서 생성된 값 사용</a></p></td>
<td align="left"><p>시스템에서 생성 된 값은 사용자 제공 입력 <a href="https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics">의미 체계</a>를 기반으로 하는 입력 어셈블러 (IA) 단계에서 생성 되어 셰이더 작업에서 특정 효율성을 허용 합니다. 인스턴스 id ( <a href="vertex-shader-stage--vs-.md">꼭 짓 점 셰이더 (vs) 단계</a>에 표시 됨), 꼭 짓 점 id (vs에 표시 됨) 또는 기본 ID ( <a href="geometry-shader-stage--gs-.md">기 하 도형 셰이더 (GS) 스테이지</a>픽셀 셰이더 (v) 단계에 표시 됨)와 같은 데이터를 연결 하 여 / <a href="pixel-shader-stage--ps-.md">Pixel Shader (PS) stage</a>후속 셰이더 단계에서 이러한 시스템 값을 검색 하 여 해당 단계의 처리를 최적화할 수 있습니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[그래픽 파이프라인](graphics-pipeline.md)

 

 