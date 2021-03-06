---
title: 계산 파이프라인
description: Direct3D 컴퓨팅 파이프라인은 그래픽 파이프라인과 거의 동시에 수행할 수 있는 계산을 처리하도록 설계되었습니다.
ms.assetid: 355B66C6-C0DF-47BA-A9C9-7AFA50B5B614
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 36d1bd524d6e71b0a1aa9477d7a2b7a5f27544aa
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750049"
---
# <a name="compute-pipeline"></a>계산 파이프라인


\[일부 정보는 상업적으로 출시 되기 전에 대폭 수정 될 수 있는 미리 릴리스된 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 어떠한 명시적 또는 묵시적 보증도 하지 않습니다.\]


Direct3D 컴퓨팅 파이프라인은 그래픽 파이프라인과 거의 동시에 수행할 수 있는 계산을 처리하도록 설계되었습니다. 계산 파이프라인에는 몇 가지 단계만 있으며,이는 프로그래밍 가능한 계산 셰이더 단계를 통해 입력에서 출력으로 흐르는 데이터를 포함 합니다.

## <a name="purpose"></a>목적

다른 프로그래밍 가능한 셰이더와 마찬가지로 [CS (계산 셰이더) 단계](compute-shader-stage--cs-.md) 는 HLSL를 사용 하 여 디자인 및 구현 됩니다. 계산 셰이더는 고속 범용 컴퓨팅을 제공 하며 GPU (그래픽 처리 장치)에서 많은 수의 병렬 프로세서를 활용 합니다. 계산 셰이더는 메모리 공유 및 스레드 동기화 기능을 제공 하 여 보다 효과적인 병렬 프로그래밍 메서드를 허용 합니다. |

## <a name="input"></a>입력

다른 프로그래밍 가능한 셰이더와 달리 입력의 정의는 추상입니다. 입력은 1, 2 또는 3 차원 일 수 있으며, 실행할 계산 셰이더의 호출 수를 결정 합니다. 한 호출 집합에 대 한 공유 데이터를 읽을 수 있습니다. |

## <a name="output"></a>출력

계산 된 데이터가 필요할 때 매우 다를 수 있는 계산 셰이더의 출력 데이터를 그래픽 렌더링 파이프라인과 동기화 할 수 있습니다.

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

 

 
