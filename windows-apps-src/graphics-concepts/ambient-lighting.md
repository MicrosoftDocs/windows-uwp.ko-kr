---
title: 주변 조명
description: 주변 조명은 장면에 일정한 조명을 제공합니다.
ms.assetid: C34FA65A-3634-4A4B-B183-4CDA89F4DC95
keywords:
- 주변 조명
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 558d7e655a54b22f1fc74591a718a7180d90366f
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8463974"
---
# <a name="ambient-lighting"></a>주변 조명


주변 조명은 장면에 일정한 조명을 비춥니다. 또한 꼭짓점 법선, 조명 방향, 조명 위치 또는 감쇠를 비롯한 다른 조명 요소의 영향을 받지 않기 때문에 모든 개체 꼭짓점을 동일하게 비춥니다. 주변 조명은 모든 방향에서 일정하며 개체의 모든 픽셀에 동일한 색을 지정합니다. 따라서 계산이 빠르지만 개체가 편평해 보이고 사실성이 떨어집니다.

주변 조명은 가장 빠른 조명 유형이지만 결과의 사실성이 가장 떨어집니다. Direct3D에는 조명을 만들지 않고 사용할 수 있는 단일 전역 주변 조명 속성이 있습니다. 또는 주변 조명을 제공하도록 조명 개체를 설정할 수 있습니다.

다음 수식으로 장면의 주변 조명을 설정합니다.

주변 조명 = Cₐ\*\[Gₐ + sum(Atten<sub>i</sub>\*Spot<sub>i</sub>\*L<sub>ai</sub>)\]

다음은 항목에 대한 설명입니다.

| 매개 변수         | 기본값 | 유형          | 설명                                                                                                       |
|-------------------|---------------|---------------|-------------------------------------------------------------------------------------------------------------------|
| Cₐ                | (0,0,0,0)     | D3DCOLORVALUE | 재질 주변 색                                                                                            |
| Gₐ                | (0,0,0,0)     | D3DCOLORVALUE | 전역 주변 색                                                                                              |
| Atten<sub>i</sub> | (0,0,0,0)     | D3DCOLORVALUE | i번째 조명의 조명 감쇠. [감쇠 및 스포트라이트 계수](attenuation-and-spotlight-factor.md)를 참조하세요. |
| Spot<sub>i</sub>  | (0,0,0,0)     | D3DVECTOR     | i번째 조명의 스포트라이트 계수. [감쇠 및 스포트라이트 계수](attenuation-and-spotlight-factor.md)를 참조하세요.  |
| sum               | 해당 없음           | 해당 없음           | 주변 조명의 합                                                                                          |
| L<sub>ai</sub>    | (0,0,0,0)     | D3DVECTOR     | i번째 조명의 조명 주변 색                                                                              |

 

Cₐ의 값은 다음 중 하나입니다.

-   vertex color1(AMBIENTMATERIALSOURCE = D3DMCS\_COLOR1이고 꼭짓점 선언에 첫 번째 꼭짓점 색을 제공할 경우)
-   vertex color2(AMBIENTMATERIALSOURCE = D3DMCS\_COLOR2이고 꼭짓점 선언에 두 번째 꼭짓점 색을 제공할 경우)
-   재질 주변 색

**참고**  AMBIENTMATERIALSOURCE 옵션을 사용 하 고 꼭 짓 점 색을 제공 하지 않을 경우 재질 주변 색이 사용 됩니다.

 

재질 주변 색을 사용하려면 아래 예제 코드처럼 SetMaterial을 사용합니다.

Gₐ는 전역 주변 색입니다. SetRenderState(D3DRS\_AMBIENT)를 사용하여 설정됩니다. Direct3D 장면에 1개의 전역 주변 색이 있습니다. 이 매개 변수는 Direct3D 조명 개체와 연결되어 있지 않습니다.

L<sub>ai</sub>는 장면에 있는 i번째 조명의 주변 색입니다. 각 Direct3D 조명에는 속성 집합이 있으며 집합의 속성 중 하나가 주변 색입니다. sum(L<sub>ai</sub>)이라는 용어는 장면에 있는 모든 주변 색의 합입니다.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>예제


이 예제에서는 장면 주변 조명과 재질 주변 색이 개체에 지정됩니다.

```
#define GRAY_COLOR  0x00bfbfbf

Ambient.r = 0.75f;
Ambient.g = 0.0f;
Ambient.b = 0.0f;
Ambient.a = 0.0f;
```

수식에 따라 재질 색과 조명 색을 합친 색이 개체 꼭짓점의 색이 됩니다.

다음 두 그림은 재질 색(회색)과 조명 색(밝은 빨강)을 보여 줍니다.

![회색 구 그림](images/amb1.jpg)![빨강 구 그림](images/lightred.jpg)

결과 장면은 다음 그림에 나와 있습니다. 장면의 개체만 구입니다. 주변 조명은 모든 개체 꼭짓점을 같은 색으로 비춥니다. 꼭짓점 법선이나 조명 방향의 영향을 받지 않습니다. 따라서 객체 표면 주변에 음영 차이가 없기 때문에 구가 2D 원처럼 보입니다.

![주변 조명이 적용된 구 그림](images/lighta.jpg)

개체가 더 사실적으로 보이게 하려면 주변 조명과 함께 확산이나 반사 조명을 적용합니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[조명의 수학](mathematics-of-lighting.md)

 

 




