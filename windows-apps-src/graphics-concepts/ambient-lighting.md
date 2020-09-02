---
title: 주변 조명
description: 주변 조명에서 장면의 고정 조명을 제공 하 고 c + +를 사용 하 여 Direct3D에서 주변 광원을 설정 하는 방법을 알아봅니다.
ms.assetid: C34FA65A-3634-4A4B-B183-4CDA89F4DC95
keywords:
- 주변 조명
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c21a674b0961836752c879bcea681b568f31053c
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304465"
---
# <a name="ambient-lighting"></a>주변 조명

주변 광원은 장면에 일정 한 조명을 제공 합니다. 꼭 짓 점 법선, 광원, 광원 위치, 범위 또는 감쇠와 같은 다른 조명 요소에 종속 되지 않기 때문에 모든 개체 꼭지점이 동일 하 게 조명 합니다. 주변 조명은 모든 방향에서 상수 이며 개체의 모든 픽셀을 동일한 색으로 색으로 합니다. 계산 하는 것은 빠르지만 개체를 평평 하 고 비현실적으로 유지 합니다.

주변 조명은 가장 빠른 조명 유형 이지만 가장 실제적인 결과를 생성 합니다. Direct3D에는 광원을 생성 하지 않고 사용할 수 있는 단일 전역 주변 광원 속성이 포함 되어 있습니다. 또는 조명 개체를 설정 하 여 주변 조명을 제공할 수 있습니다.

장면의 앰비언트 조명은 다음 수식에 설명 되어 있습니다.

주변 조명 = C ₐ \* \[ g ₐ + 합계 (atten<sub>i</sub> \* <sub>i</sub> \* L<sub>ai</sub>)\]

위치:

| 매개 변수         | 기본값 | 유형          | 설명                                                                                                       |
|-------------------|---------------|---------------|-------------------------------------------------------------------------------------------------------------------|
| C ₐ                | (0, 0, 0, 0)     | D3DCOLORVALUE | 재질 주변 색                                                                                            |
| G ₐ                | (0, 0, 0, 0)     | D3DCOLORVALUE | 전역 앰비언트 색                                                                                              |
| Atten<sub>i</sub> | (0, 0, 0, 0)     | D3DCOLORVALUE | 저 빛의 빛 감쇠. [감쇠 및 스포트라이트 요소](attenuation-and-spotlight-factor.md)를 참조 하세요. |
| 스폿<sub>i</sub>  | (0, 0, 0, 0)     | D3DVECTOR     | 저 광원의 스포트라이트 요소입니다. [감쇠 및 스포트라이트 요소](attenuation-and-spotlight-factor.md)를 참조 하세요.  |
| sum               | 해당 없음           | 해당 없음           | 앰비언트 조명의 합계입니다.                                                                                          |
| L<sub>ai</sub>    | (0, 0, 0, 0)     | D3DVECTOR     | 해당 하는 조명의 밝은 주변 색                                                                              |

 

C ₐ의 값은 다음 중 하나입니다.

-   꼭 짓 점 color1, AMBIENTMATERIALSOURCE = D3DMCS \_ color1, 첫 번째 꼭 짓 점 색이 꼭 짓 점 선언에 제공 됩니다.
-   꼭 짓 점 color2, AMBIENTMATERIALSOURCE = D3DMCS \_ color2 및 두 번째 꼭 짓 점 색이 꼭 짓 점 선언에 제공 됩니다.
-   재질 주변 색입니다.

**참고**    AMBIENTMATERIALSOURCE 옵션 중 하나를 사용 하 고 꼭 짓 점 색을 지정 하지 않으면 재질 주변 색이 사용 됩니다.

 

재질 주변 색을 사용 하려면 아래 예제 코드에 표시 된 것 처럼 SetMaterial을 사용 합니다.

G ₐ는 전역 앰비언트 색입니다. SetRenderState (D3DRS 앰비언트)를 사용 하 여 설정 됩니다 \_ . Direct3D 장면에는 하나의 전역 앰비언트 색이 있습니다. 이 매개 변수는 Direct3D light 개체와 연결 되어 있지 않습니다.

L<sub>ai</sub> 는 장면에서 해당 하는 조명의 앰비언트 색입니다. 각 Direct3D light에는 속성 집합이 있으며,이 중 하나는 앰비언트 색입니다. 용어 sum (L<sub>ai</sub>)은 장면의 모든 앰비언트 색의 합계입니다.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>예 들어


이 예제에서 개체는 장면 주변 광원 및 재질 주변 색을 사용 하 여 색이 지정 됩니다.

```cpp
#define GRAY_COLOR  0x00bfbfbf

Ambient.r = 0.75f;
Ambient.g = 0.0f;
Ambient.b = 0.0f;
Ambient.a = 0.0f;
```

수식에 따라 개체 꼭 짓 점에 대 한 결과 색은 재질 색과 밝은 색의 조합입니다.

다음 두 그림은 재질 색, 회색, 밝은 빨강의 밝은 색을 보여 줍니다.

![회색 구 그림](images/amb1.jpg)![빨간색 구의 그림](images/lightred.jpg)

다음 그림에서는 결과 장면을 보여 줍니다. 장면의 유일한 개체는 구입니다. 주변 광원은 동일한 색의 모든 개체 꼭지점을 조명 합니다. 꼭 짓 점 정상 또는 조명 방향에 종속 되지 않습니다. 따라서 개체의 표면 주위 음영에 차이가 없으므로 구는 2D 원 처럼 보입니다.

![주변 조명이 있는 구의 그림](images/lighta.jpg)

개체를 보다 사실적 하 게 보이게 하려면 주변 조명 뿐만 아니라 확산 또는 반사 조명을 적용 합니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[조명의 수학](mathematics-of-lighting.md)

 

 




