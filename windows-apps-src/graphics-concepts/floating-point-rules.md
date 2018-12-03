---
title: 부동 소수점 규칙
description: Direct3D는 여러 부동 소수점 표현을 지원합니다. 모든 부동 소수점 계산은 IEEE 754 32비트 단정밀도 부동 소수점 규칙의 정의된 하위 집합 하에서 작동합니다.
ms.assetid: 3B0C95E2-1025-4F70-BF14-EBFF2BB53AFF
keywords:
- 부동 소수점 규칙
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4de5ba146c8241598527dd268d604fcc9bb97d6d
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8458388"
---
# <a name="span-iddirect3dconceptsfloating-pointrulesspanfloating-point-rules"></a><span id="direct3dconcepts.floating-point_rules"></span>부동 소수점 규칙


Direct3D는 여러 부동 소수점 표현을 지원합니다. 모든 부동 소수점 계산은 IEEE 754 32비트 단정밀도 부동 소수점 규칙의 정의된 하위 집합 하에서 작동합니다.

## <a name="span-idalpha32bitspanspan-idalpha32bitspan32-bit-floating-point-rules"></a><span id="alpha_32_bit"></span><span id="ALPHA_32_BIT"></span>32비트 부동 소수점 규칙


두 가지 규칙 집합이 있습니다. IEEE-754를 준수하는 것과 표준에서 벗어난 것입니다.

### <a name="span-idalpha754rulesspanspan-idalpha754rulesspanspan-idalpha754rulesspanhonored-ieee-754-rules"></a><span id="alpha_754_Rules"></span><span id="alpha_754_rules"></span><span id="ALPHA_754_RULES"></span>적용되는 IEEE-754 규칙

이러한 규칙의 일부는 IEEE-754에서 제공하는 유일한 선택 항목입니다.

-   0으로 나누면 +/-INF가 됩니다. NaN이 되는 0/0은 예외입니다.
-   (+/-)0의 로그 값은 -INF입니다.  

    음수 값(-0 제외)의 로그 값은 NaN입니다.
-   음수의 역수 제곱근(rsq) 또는 제곱근(sqrt)은 NaN이 됩니다.  

    예외는 -0입니다. sqrt(-0)은 -0이며 rsq(-0)은 -INF가 됩니다.
-   INF - INF = NaN
-   (+/-)INF / (+/-)INF = NaN
-   (+/-)INF \* 0 = NaN
-   NaN (모든 연산자) 모든 값 = NaN
-   한 피연산자 또는 두 피연산자가 NaN일 경우 EQ, GT, GE, LT, LE 비교는 **FALSE**를 반환합니다.
-   비교는 0의 부호를 무시합니다. 따라서 +0은 -0과 같습니다.
-   한 피연산자 또는 두 피연산자가 NaN일 경우 NE 비교는 **TRUE**를 반환합니다.
-   NaN이 아닌 모든 값을 +/-INF와 비교하면 올바른 결과를 반환합니다.

### <a name="span-idalpha754deviationsspanspan-idalpha754deviationsspanspan-idalpha754deviationsspandeviations-or-additional-requirements-from-ieee-754-rules"></a><span id="alpha_754_Deviations"></span><span id="alpha_754_deviations"></span><span id="ALPHA_754_DEVIATIONS"></span>IEEE-754 규칙에서 벗어나거나 추가된 요구 사항

-   IEEE-754에서는 부동 소수점 연산이 표현할 수 있는 가장 근접한 값을 무한한 정확도의 결과로 생성되도록 요구합니다. 이를 가장 가까운 짝수로 반올림이라고 합니다.

    Direct3D 11 이상은 IEEE-754와 동일한 요구 사항을 가지고 있습니다. 32비트 부동 소수점 연산은 무한한 정확도의 결과에서 0.5 마지막 위치 단위(ULP) 내의 결과를 생성합니다. 즉, 예를 들어 하드웨어가 가장 가까운 짝수로 반올림 대신 결과를 32비트로 자르도록 허용하며 오류가 최대 0.5 ULP의 범위 내에서만 발생하게 됩니다. 이 규칙은 더하기, 빼기, 곱하기에만 적용됩니다.

    이전 버전의 Direct3D는 IEEE-754보다 가벼운 요구 사항을 가지고 있습니다. 32비트 부동 소수점 연산이 무한한 정확도의 결과에서 1 마지막 위치 단위(ULP) 내의 결과를 생성합니다. 즉, 예를 들어 하드웨어가 가장 가까운 짝수로 반올림 대신 결과를 32비트로 자르도록 허용하면 오류가 최대 1 ULP의 범위 내에서만 발생하게 됩니다.

-   부동 소수점 예외, 상태 비트 또는 트랩은 지원되지 않습니다.
-   비정규화 값은 모든 부동 소수점 수학 연산의 입력과 출력에서 부호가 유지된 0으로 플러시됩니다. 데이터를 조작하지 않는 모든 I/O 또는 데이터 이동 작업에 대해서는 예외 사항이 있습니다.
-   뷰포트 MinDepth/MaxDepth 또는 BorderColor 값과 같은 부동 소수점 값을 포함하는 상태는 비정규화 값으로 제공될 수 있으며, 하드웨어가 이를 사용하기 전에 플러시될 수도, 그렇지 않을 수도 있습니다.
-   최소 또는 최대 연산은 비교 시 비정규화 값을 플러시하지만, 결과는 플러시될 수도, 그렇지 않을 수도 있습니다.
-   연산에 NaN이 입력되면 항상 NaN이 출력됩니다. 하지만 연산이 데이터를 변경하지 않는 순수 이동 명령인 경우 NaN의 정확한 비트 패턴은 동일하게 유지될 필요가 없습니다.
-   하나의 피연산자만 NaN인 최소 또는 최대 연산의 경우, 앞서 살펴본 비교 규칙과 반대로 다른 피연산자를 결과로 반환합니다. 이는 IEEE 754R 규칙입니다.

    부동 소수점 최소 또는 최대 연산에 대한 IEEE-754R 사양은 최소 또는 최대의 입력 중 하나가 QNaN 값인 경우, 연산의 결과는 다른 매개 변수가 된다고 정의하고 있습니다. 예:

    ```ManagedCPlusPlus
    min(x,QNaN) == min(QNaN,x) == x (same for max)
    ```

    IEEE 754R 사양의 개정안은 하나의 입력이 QNaN 값이 아닌 SNaN 값인 경우 최소 및 최대 연산에 대해 다른 동작을 채택했습니다.

    ```ManagedCPlusPlus
    min(x,SNaN) == min(SNaN,x) == QNaN (same for max)
     
    ```

    일반적으로 Direct3D는 산술에 대한 표준 IEEE-754 및 IEEE 754R을 따릅니다. 하지만 이 경우 다르게 적용되는 것이 있습니다.

    Direct3D 10 이상의 산술 규칙에서는 QNaN과 SNaN 값 사이에 구분이 없습니다. 모든 NaN 값은 같은 방식으로 처리됩니다. 최소 및 최대의 경우 모든 NaN 값에 대한 Direct3D의 동작은 QNaN이 IEEE 754R 정의에서 처리되는 것과 비슷합니다. (완벽한 설명을 위해 보충하면, 두 입력이 모두 NaN이면 둘 중 하나의 NaN이 반환됩니다.)

-   다른 IEEE 754R 규칙은 min(-0,+0) == min(+0,-0) == -0, max(-0,+0) == max(+0,-0) == +0으로 부호를 유지합니다. 앞서 본 부호가 있는 0에 대한 비교 규칙과는 반대입니다. Direct3D는 여기에 IEEE 754R의 동작을 권장하지만, 강요하지는 않습니다. 부호를 무시한 비교를 사용하여 매개 변수의 순서에 따라 0을 비교하는 것이 가능합니다.
-   x\*1.0f의 결과는 항상 x입니다(비정규화 값이 플러시되는 경우는 제외).
-   x/1.0f의 결과는 항상 x입니다(비정규화 값이 플러시되는 경우는 제외).
-   x +/- 0.0f의 결과는 항상 x입니다(비정규화 값이 플러시되는 경우는 제외). 하지만 -0 + 0 = +0입니다.
-   혼합 연산(예: mad, dp3)은 연산의 비혼합 확장이 가질 수 있는 가장 최악의 직렬 배열보다는 정확한 결과를 생성합니다. 최악의 배열의 정의는 허용 오차의 목적에서 지정된 혼합 연산을 위한 고정된 정의가 아닙니다. 이는 입력의 특정 값에 따라 달라집니다. 비혼합 확장의 개별 단계는 각각 1의 ULP 허용 오차가 허용됩니다. 또는 특정 명령에 Direct3D가 1 ULP보다 더 유연한 허용 오차를 사용하는 경우 더 유연한 허용 오차가 허용됩니다.
-   혼합 연산은 비혼합 연산과 동일한 NaN 규칙을 따릅니다.
-   sqrt와 rcp의 허용 오차는 1 ULP입니다. 셰이더 역수와 역수 제곱근 명령인 [**rcp**](https://msdn.microsoft.com/library/windows/desktop/hh447205)와 [**rsq**](https://msdn.microsoft.com/library/windows/desktop/hh447221)에는 자체적인 별도의 완화된 정밀도 요구 사항이 있습니다.
-   곱하기와 나누기는 각각 32비트 부동 소수점 정밀도 수준에서 계산됩니다(곱하기의 정밀도는 0.5 ULP, 역의 경우 1.0 ULP). x/y를 바로 구현하는 경우, 결과는 두 단계 방법보다 높거나 같은 정확도를 가져야 합니다.

## <a name="span-iddoubleprec64bitspanspan-iddoubleprec64bitspan64-bit-double-precision-floating-point-rules"></a><span id="double_prec_64_bit"></span><span id="DOUBLE_PREC_64_BIT"></span>64비트(배정밀도) 부동 소수점 규칙


하드웨어 및 디스플레이 드라이버는 선택적으로 배정밀도 부동 소수점을 지원합니다. 지원하도록 지정하기 위해 [**D3D11\_FEATURE\_DOUBLES**](https://msdn.microsoft.com/library/windows/desktop/ff476124#d3d11-feature-doubles)과 함께 [**ID3D11Device::CheckFeatureSupport**](https://msdn.microsoft.com/library/windows/desktop/ff476497)를 호출하면, 드라이버가 [**D3D11\_FEATURE\_DATA\_DOUBLES**](https://msdn.microsoft.com/library/windows/desktop/ff476127)의 **DoublePrecisionFloatShaderOps**를 TRUE로 설정합니다. 그러면 드라이버와 하드웨어는 모든 배정밀도 부동 소수점 명령을 지원해야 합니다.

배정밀도 명령은 IEEE 754R 동작 요구 사항을 따릅니다.

배정밀도 데이터(0으로 플러시 동작 없음)를 위해 비정규화 값의 생성 지원이 필요합니다. 마찬가지로, 명령은 비정규화 데이터를 부호가 있는 0으로 읽지 않으며 비정규화 값을 그대로 따릅니다.

## <a name="span-idalpha16bitspanspan-idalpha16bitspan16-bit-floating-point-rules"></a><span id="alpha_16_bit"></span><span id="ALPHA_16_BIT"></span>16비트 부동 소수점 규칙


Direct3D 부동 소수점의 16비트 표시도 지원합니다.

형식:

-   MSB 비트 위치에서 1개의 부호 비트(s)
-   편향 지수의 5비트(e)
-   숨겨진 추가 비트를 가진 분수의 10비트(f)

float16 값(v)은 이러한 규칙을 따릅니다.

-   e == 31이고 f != 0인 경우 s와 관계없이 v는 NaN입니다.
-   e == 31이고 f == 0이면 v = (-1)s\*무한(부호 있는 무한)입니다.
-   e가 0에서 31 사이에 있으면 v = (-1)s\*2(e-15)\*(1.f)입니다.
-   e == 0이고 f != 0이면 v = (-1)s\*2(e-14)\*(0.f)(비정규화 숫자)입니다.
-   e == 0이고 f == 0이면 v = (-1)s\*0(부호 있는 0)입니다.

32비트 부동 소수점 규칙은 앞에서 설명한 비트 레이아웃을 위해 조정되어 16비트 부동 소수점 숫자에도 적용됩니다. 다음과 같은 경우는 예외입니다.

-   정밀도: 16비트 부동 소수점 숫자에 대한 혼합 연산은 무한한 정확도 결과에 가장 가까운 표현 가능한 값(IEEE-754에 따른 가장 근사한 짝수로 반올림이 16비트 값에 적용)을 생성합니다. 32비트 부동 소수점 규칙은 1 ULP의 허용 오차를 따르며, 16비트 부동 소수점 규칙은 비혼합 연산의 경우 0.5 ULP, 혼합 연산의 경우 0.6 ULP 허용 오차를 따릅니다.
-   16비트 부동 소수점 숫자는 비정규화 값을 유지합니다.

## <a name="span-idalpha11bitspanspan-idalpha11bitspan11-bit-and-10-bit-floating-point-rules"></a><span id="alpha_11_bit"></span><span id="ALPHA_11_BIT"></span>11비트 및 10비트 부동 소수점 규칙


Direct3D는 11비트와 10비트 부동 소수점 형식도 지원합니다.

형식:

-   부호 비트 없음
-   편향 지수의 5비트(e)
-   11비트 형식의 경우 분수의 6비트, 10비트 형식의 경우 분수의 5비트(양쪽 모두 숨겨진 추가 비트 포함)

float11/float10 값(v)은 다음 규칙을 따릅니다.

-   e == 31이고 f != 0이면 v는 NaN입니다.
-   e == 31이고 f == 0이면 v = +무한입니다.
-   e가 0에서 31 사이에 있으면 v = 2(e-15)\*(1.f)입니다.
-   e == 0이고 f != 0이면 v = \*2(e-14)\*(0.f)(비정규화 숫자)입니다.
-   e == 0이고 f == 0이면 v = 0입니다.

32비트 부동 소수점 규칙은 앞에서 설명한 비트 레이아웃을 위해 조정되어 11비트 및 10비트 부동 소수점 숫자에도 적용됩니다. 예외에는 다음이 포함됩니다.

-   정밀도: 32비트 부동 소수점 규칙은 0.5 ULP를 따릅니다.
-   10/11비트 부동 소수점 숫자는 비정규화 값을 유지합니다.
-   결과가 0보다 작은 모든 연산은 0으로 고정됩니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[부록](appendix.md)

[리소스](https://msdn.microsoft.com/library/windows/desktop/ff476894)

[텍스처](https://msdn.microsoft.com/library/windows/desktop/ff476902)

 

 




