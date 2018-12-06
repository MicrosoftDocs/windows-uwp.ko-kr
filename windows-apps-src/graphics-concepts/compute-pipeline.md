---
title: 계산 파이프라인
description: Direct3D 계산 파이프라인은 대개 그래픽 파이프라인과 함께 병렬로 수행할 수 있는 계산을 처리하도록 설계되었습니다.
ms.assetid: 355B66C6-C0DF-47BA-A9C9-7AFA50B5B614
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 911546f1c2973a79aea4b597a47352149a4e4210
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8753371"
---
# <a name="compute-pipeline"></a>계산 파이프라인


\[일부 정보는 상업용으로 출시되기 전에 상당 부분 수정될 수 있는 시험판 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.\]


Direct3D 계산 파이프라인은 대개 그래픽 파이프라인과 함께 병렬로 수행할 수 있는 계산을 처리하도록 설계되었습니다. 계산 파이프라인에는 프로그래밍 가능한 계산 셰이더 단계를 통해 입력에서 출력으로 데이터가 흐르는 몇 가지 단계만 있습니다.

| | |
|-|-|
|용도|다른 프로그래밍 가능한 셰이더와 같이 [CS(계산 셰이더) 단계](compute-shader-stage--cs-.md)는 HLSL로 설계되고 구현됩니다. 계산 셰이더는 일반적인 용도의 고속 계산을 제공하고 GPU(그래픽 처리 장치)의 많은 병렬 프로세서를 활용합니다. 계산 셰이더는 더 효과적인 병렬 프로그래밍 방법을 지원하기 위해 메모리 공유 및 스레드 동기화 기능을 제공합니다.|
|입력|다른 프로그래밍 가능한 셰이더와 달리 입력의 정의는 추상적입니다. 입력은 본질적으로 1차원, 2차원 또는 3차원일 수 있습니다. 이에 따라 실행할 계산 셰이더의 호출 수가 결정됩니다. 읽을 하나의 호출 집합에 대한 공유 데이터를 정의할 수 있습니다.|
|출력|계산된 데이터가 필요할 때 다양한 계산 셰이더의 출력 데이터를 그래픽 렌더링 파이프라인과 동기화할 수 있습니다.|
| | |




<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">Like other programmable shaders, <a href="#compute-shader-stage--cs-.md">Compute Shader (CS) stage</a> is designed and implemented with HLSL. A compute shader provides high-speed general purpose computing and takes advantage of the large numbers of parallel processors on the graphics processing unit (GPU). The compute shader provides memory sharing and thread synchronization features to allow more effective parallel programming methods.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Unlike other programmable shaders, the definition of input is abstract. The input can be one, two or three-dimensional in nature, determining the number of invocations of the compute shader to execute. It is possible to define shared data for one set of invocations to read.</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">Output data from the compute shader, which can be highly varied, can be synchronized with the graphics rendering pipeline when the computed data is required.</td>
</tr>
</tbody>
</table>
-->

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[Direct3D 그래픽 학습 가이드](index.md)

 

 
