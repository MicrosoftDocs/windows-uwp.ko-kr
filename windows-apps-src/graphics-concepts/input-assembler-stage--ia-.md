---
title: IA(입력 어셈블러) 단계
description: IA(입력 어셈블러) 단계는 의미 체계 ID를 포함해 삼각형, 직선과 점 같은 기본 및 인접 데이터를 파이프라인에 제공하여 이미 처리된 원형에 대한 처리를 줄임으로써 셰이더의 효율성을 높입니다.
ms.assetid: AF1DC611-C872-47F1-BF1A-92C68C8903E6
keywords:
- IA(입력 어셈블러) 단계
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: def755f868c7ea30679f19877cec84b20faa44f5
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5703868"
---
# <a name="input-assembler-ia-stage"></a>IA(입력 어셈블러) 단계


IA(입력 어셈블러) 단계는 의미 체계 ID를 포함해 삼각형, 직선과 점 같은 기본 및 인접 데이터를 파이프라인에 제공하여 이미 처리된 원형에 대한 처리를 줄임으로써 셰이더의 효율성을 높입니다.

## <a name="span-idpurpose-and-usesspanspan-idpurpose-and-usesspanspan-idpurpose-and-usesspanpurpose-and-uses"></a><span id="Purpose-and-uses"></span><span id="purpose-and-uses"></span><span id="PURPOSE-AND-USES"></span>목적 및 사용


IA(입력 어셈블러) 단계를 사용하는 이유는 사용자가 채운 버퍼에서 기본 데이터(점, 직선 및 삼각형)를 읽고 다른 파이프라인 단계에서 사용할 원형에 조합하거나 [시스템 생성 값 ](https://msdn.microsoft.com/library/windows/desktop/bb509647)을 연결하여 셰이더의 효율을 높이는 것입니다. 시스템 생성 값은 의미 체계라고도 하는 텍스트 문자열입니니다. 프로그래밍 가능한 셰이더 단계는 시스템 생성 값(기본 ID, 인스턴스 ID 또는 꼭짓점 ID 등)을 사용하는 공통 셰이더 핵심으로부터 구성되므로, 셰이더 단계는 아직 처리되지 않은 원형, 인스턴스 또는 꼭짓점만 처리할 수 있습니다.

IA 단계는 여러 [기본 형식](primitive-topologies.md)(선 목록, 삼각형 스트립 또는 인접한 원형 등)에 꼭짓점을 조합할 수 있습니다. 인접한 삼각형 목록, 인접한 선 목록 등의 기본 형식은 [GS(기하 도형 셰이더) 단계](geometry-shader-stage--gs-.md)를 지원합니다.

인접 정보는 기하 도형 셰이더의 응용 프로그램에서만 볼 수 있습니다. 예를 들어 인접성을 포함한 삼각형으로 기하 도형 셰이더가 호출된 경우 입력 데이터에는 삼각형 하나에 해당하는 3개의 꼭짓점, 삼각형의 인접 데이터에 해당하는 3개의 꼭짓점이 포함될 것입니다.

IA 단계에 인접 데이터를 출력하라는 요청이 접수된 경우, 입력 데이터에 인접 데이터가 포함되어야 합니다. 그러려면 더미 꼭짓점(중복 제거 삼각형 형성)을 제공하거나 경우에 따라 꼭짓점의 존재 여부를 나타내는 꼭짓점 속성 중 하나에 플래그를 지정해야 할 수도 있습니다. 래스터라이저 단계에 중복 제거 기하 도형의 컬링이 발생하더라도 이 역시 기하 도형 셰이더에서 발견하고 처리해야 합니다.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>입력


IA 단계는 메모리: 기본 데이터(점, 선 및/또는 삼각형), 사용자가 채운 버퍼에서 데이터를 읽어 들입니다.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>출력


IA 단계는 데이터를 원형으로 조합하고, 시스템 생성 값을 연결하며, [꼭짓점 셰이더(VS) 단계](vertex-shader-stage--vs-.md) 및 이후 다른 파이프라인 단계에서 사용할 원형으로 출력합니다.

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
<td align="left"><p><a href="primitive-topologies.md">기본 토폴로지</a></p></td>
<td align="left"><p>Direct3D는 여러 개의 기본 토폴로지를 지원하는데, 이 토폴로지는 점 목록, 선 목록, 삼각형 스트립 등 파이프라인이 꼭짓점을 해석 및 렌더링하는 방식을 정의합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="using-system-generated-values.md">시스템 생성 값 사용</a></p></td>
<td align="left"><p>시스템 생성 값은 셰이더 작업에서 어느 정도의 효율성을 허용하기 위해 IA(입력 어셈블러) 단계(사용자가 제공한 입력 <a href="https://msdn.microsoft.com/library/windows/desktop/bb509647">의미 체계</a> 기반)에서 생성됩니다. 후속 셰이더 단계에서는 인스턴스 ID(<a href="vertex-shader-stage--vs-.md">VS(꼭짓점 셰이더) 단계</a>에 표시), 꼭짓점 ID(VS에 표시) 또는 기본 ID(<a href="geometry-shader-stage--gs-.md">GS(기하 도형 셰이더) 단계</a>/<a href="pixel-shader-stage--ps-.md">PS(픽셀 셰이더) 단계</a>에 표시)와 같은 데이터를 연결하여 해당 단계에서 처리를 최적화하는 시스템 값을 찾을 수 있습니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[그래픽 파이프라인](graphics-pipeline.md)

 

 




