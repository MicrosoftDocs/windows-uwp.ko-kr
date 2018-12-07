---
title: 보기
description: '\ 0034;보기 \ 0034;라는 용어는 \ 0034;필요한 형식의 데이터 \ 0034;를 뜻하는 데 사용됩니다. 예를 들어 CBV(상수 버퍼 보기)는 올바른 형식의 상수 버퍼 데이터일 것입니다. 이 섹션에서는 가장 일반적이고 유용한 보기를 설명합니다.'
ms.assetid: 0C7FB99F-7391-472F-BA53-576888DFC171
keywords:
- 보기
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: df106a400a7ba8c8f94aa6dd35325aabacd36eca
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8796089"
---
# <a name="views"></a>보기


"보기"라는 용어는 "필요한 형식의 데이터"를 뜻하는 데 사용됩니다. 예를 들어 CBV(상수 버퍼 보기)는 올바른 형식의 상수 버퍼 데이터일 것입니다. 이 섹션에서는 가장 일반적이고 유용한 보기를 설명합니다.

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
<td align="left"><p><a href="constant-buffer-view--cbv-.md">CBV(상수 버퍼 보기)</a></p></td>
<td align="left"><p>상수 버퍼에는 셰이더 상수 데이터가 포함됩니다. 이러한 데이터의 가치는 데이터 변경이 필요할 때까지 데이터가 유지되고 모든 GPU 셰이더에서 데이터에 액세스할 수 있다는 점입니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="vertex-buffer-view--vbv-.md">VBV(꼭짓점 버퍼 보기) 및 IBV(인덱스 버퍼 보기)</a></p></td>
<td align="left"><p>꼭짓점 버퍼는 꼭짓점 목록에 대한 데이터를 저장합니다. 각 꼭짓점에 대한 데이터에는 위치, 색, 일반 벡터, 텍스처 좌표 등이 포함될 수 있습니다. 인덱스 버퍼는 꼭짓점 버퍼에 대한 정수 인덱스(오프셋)를 보관하며, 꼭짓점 전체 목록의 하위 집합으로 이루어진 개체를 정의하고 렌더링하는 데 사용됩니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="shader-resource-view--srv-.md">SRV(셰이더 리소스 보기) 및 UAV(정렬되지 않은 액세스 보기)</a></p></td>
<td align="left"><p>셰이더 리소스 뷰는 일반적으로 셰이더가 텍스처에 액세스할 수 있는 형식으로 텍스처를 래핑합니다. 순서가 지정되지 않은 액세스 보기는 유사한 기능을 제공하지만, 순서와 상관없이 텍스처(또는 다른 리소스)에 대한 읽기 및 쓰기가 가능합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="sampler.md">샘플러</a></p></td>
<td align="left"><p>샘플링은 텍스처 또는 기타 리소스로부터 입력 값을 읽는 프로세스입니다. &quot;샘플러&quot;는 리소스로부터 읽는 객체입니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="render-target-view--rtv-.md">RTV(렌더링 대상 보기)</a></p></td>
<td align="left"><p>렌더링 대상은 장면을 백 버퍼에 렌더링하지 않고 임시 중간 버퍼에 렌더링할 수 있습니다. 이 기능을 사용하면 그래픽 파이프라인 내에서 반사 텍스처 또는 다른 용도로 렌더링할 수 있는 복잡한 장면을 사용하거나 렌더링하기 전에 화면에 추가 픽셀 셰이더 효과를 추가할 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="depth-stencil-view--dsv-.md">DSV(깊이 스텐실 보기)</a></p></td>
<td align="left"><p>깊이 스텐실 보기는 깊이 및 스텐실 정보를 보관하는 형식 및 버퍼를 제공합니다. 깊이 버퍼는 더 가까운 개체에 의해 가려져 뷰어에서 픽셀의 그림이 보이지 않게 되면 이를 제거하는 데 사용됩니다. 스텐실 버퍼는 정의된 셰이프를 제외한 모든 그리기를 제거하는 데 사용할 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="stream-output-view--sov-.md">SOV(스트림 출력 보기)</a></p></td>
<td align="left"><p>스트림 출력 보기를 사용하면 꼭짓점, 공간 분할 및 기하 도형 셰이더가 제공하는 꼭짓점 정보를 나중에 사용할 수 있도록 응용 프로그램에 다시 스트리밍할 수 있습니다. 예를 들어, 이러한 셰이더에 의해 왜곡되는 개체는 응용 프로그램에 다시 작성되어 물리 또는 기타 엔진에 더 정확하게 입력할 수 있습니다. 하지만 실제로는 스트림 출력 보기는 그래픽 파이프라인에서 자주 사용되지 않는 기능입니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="rasterizer-ordered-view--rov-.md">ROV(정렬된 래스터라이저 보기)</a></p></td>
<td align="left"><p>정렬된 래스터라이저 보기를 사용하면 특히 투명도가 포함된 여러 텍스처를 모두 동일한 픽셀에 적용하는 경우 깊이 버퍼의 일부 제한을 해결할 수 있습니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[Direct3D 그래픽 학습 가이드](index.md)

 

 




