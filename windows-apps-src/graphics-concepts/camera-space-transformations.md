---
title: 카메라 공간 변환
description: 카메라 공간의 꼭짓점은 세계 좌표 뷰 매트릭스로 개체 꼭짓점을 변환하여 계산됩니다.
ms.assetid: 86EDEB95-8348-4FAA-897F-25251B32B076
keywords:
- 카메라 공간 변환
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1b35fb71e51044ee6be6ed90001e3b5614c8cb45
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7985690"
---
# <a name="camera-space-transformations"></a>카메라 공간 변환


카메라 공간의 꼭짓점은 세계 좌표 뷰 매트릭스로 개체 꼭짓점을 변환하여 계산됩니다.

V = V \* wvMatrix

카메라 공간의 꼭짓점 법선은 세계 좌표 뷰 매트릭스의 역 바꿈으로 개체 법선을 변환하여 계산됩니다. 세계 좌표 뷰 매트릭스는 직교이거나 직교가 아닐 수 있습니다.

N = N \* (wvMatrix⁻¹)<sup>T</sup>

매트릭스 반전과 매트릭스 바꿈은 4x4 매트릭스에 사용할 수 있습니다. 곱하기가 결과 4x4 매트릭스의 3x3 부분과 법선을 결합합니다.

렌더링 상태가 법선을 정규화하도록 설정된 경우 다음과 같이 카메라 공간으로 변환 후 꼭짓점 법선 벡터가 정규화됩니다.

N = norm(N)

카메라 공간의 조명 위치는 뷰 매트릭스로 광원 위치를 변환하여 계산됩니다.

Lₚ = Lₚ \* vMatrix

방향성 광원에 대한 카메라 공간의 광원 방향은 광원 방향에 뷰 매트릭스를 곱하고, 정규화하고, 결과를 부정하여 계산됩니다.

L<sub>dir</sub> = -norm(L<sub>dir</sub> \* wvMatrix)

점 광원과 스포트라이트에 대해 조명 방향은 다음과 같이 계산됩니다.

L<sub>dir</sub> = norm(V \* Lₚ). 여기의 매개 변수는 다음 표에 정의되어 있습니다.

| 매개 변수       | 기본값 | 유형                                          | 설명                                               |
|-----------------|---------------|-----------------------------------------------|-----------------------------------------------------------|
| L<sub>dir</sub> | 해당 없음           | 3D 벡터(x, y 및 z 부동 소수점 값) | 개체 꼭짓점에서 조명으로의 방향 벡터          |
| V               | 해당 없음           | 3D 벡터(x, y 및 z 부동 소수점 값) | 카메라 공간의 꼭짓점 위치                           |
| wvMatrix        | ID      | 부동 소수점 값의 4x4 매트릭스           | 세계 좌표와 뷰 변환을 포함하는 합성 매트릭스 |
| N               | 해당 없음           | 3D 벡터(x, y 및 z 부동 소수점 값) | 꼭짓점 법선                                             |
| Lₚ              | 해당 없음           | 3D 벡터(x, y 및 z 부동 소수점 값) | 카메라 공간의 조명 위치                            |
| vMatrix         | ID      | 부동 소수점 값의 4x4 매트릭스           | 뷰 변환을 포함하는 매트릭스                      |

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[조명의 수학](mathematics-of-lighting.md)

 

 




