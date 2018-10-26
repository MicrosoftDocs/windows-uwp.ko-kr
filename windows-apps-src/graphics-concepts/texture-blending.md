---
title: 텍스처 혼합
description: Direct3D는 싱글패스에 최대 8개까지 텍스처를 기본 객체로 혼합할 수 있습니다.
ms.assetid: 9AD388FA-B2B9-44A9-B73E-EDBD7357ACFB
keywords:
- 텍스처 혼합
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d4121bd402b048ee6102ed3be30b94a66e274273
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5562201"
---
# <a name="texture-blending"></a>텍스처 혼합


Direct3D는 싱글패스에 최대 8개까지 텍스처를 기본 객체로 혼합할 수 있습니다. 다중 텍스처 혼합을 사용하면 Direct3D 응용 프로그램의 프레임 속도를 크게 높일 수 있습니다. 응용 프로그램은 다중 텍스처 혼합을 사용하여 싱글패스에 텍스처, 그림자, 정반사 조명, 확산 조명 및 기타 특수 효과를 적용합니다.

텍스처 혼합을 사용하려면 먼저 애플리케이션이 사용자의 하드웨어가 지원하는지 확인해야 합니다.

## <a name="span-idtexture-stages-and-the-texture-blending-cascadespanspan-idtexture-stages-and-the-texture-blending-cascadespanspan-idtexture-stages-and-the-texture-blending-cascadespantexture-stages-and-the-texture-blending-cascade"></a><span id="Texture-Stages-and-the-Texture-Blending-Cascade"></span><span id="texture-stages-and-the-texture-blending-cascade"></span><span id="TEXTURE-STAGES-AND-THE-TEXTURE-BLENDING-CASCADE"></span>텍스처 스테이지 및 텍스처 혼합 전파(cascade)


Direct3D는 텍스처 스테이지를 사용하여 싱글패스 방식의 다중 텍스처 혼합을 지원합니다. 텍스처 스테이지는 2개의 인수를 사용하여 혼합 연산을 실행한 다음 그 결과를 나중에 처리하거나 래스터화할 수 있도록 전달합니다. 텍스처 스테이지는 다음 다이어그램과 같이 시각화할 수 있습니다.

![텍스처 스테이지 다이어그램](images/texstg.png)

위 다이어그램에서도 알 수 있듯이 텍스처 스테이지는 지정 연산자를 사용하여 2개의 인수를 혼합합니다. 공통 연산으로는 인수의 색상 또는 알파 성분을 단순히 곱하거나(modulation) 더하는(addition) 것도 포함되지만 지원되는 연산 수는 24개가 넘습니다. 스테이지 인수는 관련 텍스처, 반복되는 색상 또는 알파(Gouraud 셰이딩 단계에서 반복), 임의 색상 및 알파, 또는 이전 텍스처 스테이지의 결과가 될 수도 있습니다.

**참고**  Direct3D는 색상 혼합과 알파 혼합을 구분 합니다. 응용 프로그램은 색상 및 알파에 대한 혼합 연산과 인수를 개별적으로 설정하며, 이러한 설정 결과는 서로 독립적입니다.

 

다중 혼합 스테이지에서 사용되는 인수와 연산의 조합은 간단한 흐름 기반 혼합 언어를 정의합니다. 한 스테이지의 결과가 다른 스테이지로, 그리고 또 다른 스테이지로 흘러가는 방식입니다. 결과가 이렇게 스테이지에서 스테이지로 흐르면서 결국 폴리곤에서 래스터화되는 개념을 텍스트 혼합 전파라고 합니다. 다음 다이어그램은 각 텍스처 스테이지가 텍스처 혼합 전파를 구성하는 방식을 나타낸 것입니다.

![텍스처 혼합 전파를 구성하는 텍스처 스테이지 다이어그램](images/tcascade.png)

장치의 각 스테이지는 0 기반 인덱스를 갖습니다. 항상 장치 기능을 살펴보면서 현재 하드웨어가 스테이지를 몇 개까지 지원하는지 확인해야겠지만 Direct3D는 혼합 스테이지를 최대 8개까지 지원합니다. 첫 번째 혼합 스테이지는 인덱스 0에, 두 번째는 1에 위치하는 식으로 인덱스 7까지 이어집니다. 시스템은 인덱스 오름차순으로 스테이지를 혼합합니다.

스테이지 번호는 필요한 만큼만 사용합니다. 사용하지 않는 혼합 스테이지는 기본적으로 사용 안 함 상태로 설정됩니다. 따라서 응용 프로그램이 첫 번째 스테이지 2개만 사용하는 경우에는 0번과 1번 스테이지의 연산 및 인수만 설정해야 합니다. 시스템은 이 스테이지 2개만 사용하고 사용 안 함으로 설정되는 스테이지는 모두 무시합니다.

응용 프로그램이 상황에 따라 사용하는 스테이지 번호가 다르다고 해서(예: 일부 객체에 4개 스테이지를, 그리고 다른 객체에 2개 스테이지 사용) 이전에 사용했던 스테이지를 모두 명시적으로 사용 안 함으로 설정할 필요는 없습니다. 한 가지 옵션으로 첫 번째 사용하지 않는 스테이지의 색상 연산을 사용 안 함으로 설정하는 방법 있습니다. 그러면 인덱스가 더 높은 스테이지는 적용되지 않습니다. 또 다른 옵션으로 첫 번째 텍스처 스테이지(0번 스테이지)의 색상 연산을 사용 안 함 상태로 설정하여 텍스처 매핑까지 함께 사용하지 않는 방법도 있습니다.

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
<td align="left"><p><a href="blending-stages.md">혼합 스테이지</a></p></td>
<td align="left"><p>혼합 스테이지는 텍스처의 혼합 방식을 정의하는 텍스처 연산 및 인수로 구성된 집합입니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="multipass-texture-blending.md">멀티패스 텍스처 혼합</a></p></td>
<td align="left"><p>Direct3D 응용 프로그램은 다중 렌더링 패스를 통해 여러 가지 텍스처를 기본 객체에 적용함으로써 다양한 특수 효과를 낼 수 있습니다. 여기서 사용하는 공통 용어는 <em>멀티패스 텍스처 혼합</em>입니다. 일반적인 의미의 멀티패스 텍스처 혼합이란 몇 가지 다른 텍스처에서 여러 색상을 적용하여 복잡한 조명 및 음영 모델의 효과를 에뮬레이션하는 것을 말합니다. <em>조명 매핑</em>도 여기에 속합니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[텍스처](textures.md)

 

 




