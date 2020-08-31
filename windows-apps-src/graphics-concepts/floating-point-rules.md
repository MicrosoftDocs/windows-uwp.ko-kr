---
title: 부동 소수점 규칙
description: Direct3D는 여러 부동 소수점 표현을 지원 합니다. 모든 부동 소수점 계산은 IEEE 754 32 비트 단일 정밀도 부동 소수점 규칙의 정의 된 하위 집합에서 작동 합니다.
ms.assetid: 3B0C95E2-1025-4F70-BF14-EBFF2BB53AFF
keywords:
- 부동 소수점 규칙
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7832d4ad344e425bc479d52e1516ae0c63dff624
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168067"
---
# <a name="span-iddirect3dconceptsfloating-point_rulesspanfloating-point-rules"></a><span id="direct3dconcepts.floating-point_rules"></span>부동 소수점 규칙


Direct3D는 여러 부동 소수점 표현을 지원 합니다. 모든 부동 소수점 계산은 IEEE 754 32 비트 단일 정밀도 부동 소수점 규칙의 정의 된 하위 집합에서 작동 합니다.

## <a name="span-idalpha_32_bitspanspan-idalpha_32_bitspan32-bit-floating-point-rules"></a><span id="alpha_32_bit"></span><span id="ALPHA_32_BIT"></span>32 비트 부동 소수점 규칙


규칙에는 IEEE-754을 준수 하는 규칙 집합과 표준에서 벗어난 규칙 집합이 있습니다.

### <a name="span-idalpha_754_rulesspanspan-idalpha_754_rulesspanspan-idalpha_754_rulesspanhonored-ieee-754-rules"></a><span id="alpha_754_Rules"></span><span id="alpha_754_rules"></span><span id="ALPHA_754_RULES"></span>IEEE-754 규칙 허용

이러한 규칙 중 일부는 IEEE-754에서 선택 항목을 제공 하는 단일 옵션입니다.

-   0으로 나누면 0/0이 발생 하 고 NaN이 생성 됩니다.
-   log of (+/-) 0은-INF를 생성 합니다.  

    음수 값 (-0이 아닌)의 로그는 NaN을 생성 합니다.
-   음수의 역 제곱근 (rsq) 또는 제곱근 (sqrt)은 NaN을 생성 합니다.  

    예외는-0입니다. sqrt (-0)는-0을 생성 하 고 rsq (-0)은-INF를 생성 합니다.
-   INF-INF = NaN
-   (+/-) INF/(+/-) INF = NaN
-   (+/-) INF \* 0 = NaN
-   NaN (any OP) any-value = NaN
-   두 피연산자 중 하나 또는 둘 모두가 NaN 인 경우 EQ, GT, GE, LT, LE의 비교는 **FALSE**를 반환 합니다.
-   비교는 0의 부호를 무시 합니다 (따라서 + 0은-0과 같음).
-   두 피연산자 중 하나 또는 둘 다가 NaN 일 때 비교는 n **을 반환 합니다**.
-   +/-INF에 대해 NaN이 아닌 값을 비교 하면 올바른 결과가 반환 됩니다.

### <a name="span-idalpha_754_deviationsspanspan-idalpha_754_deviationsspanspan-idalpha_754_deviationsspandeviations-or-additional-requirements-from-ieee-754-rules"></a><span id="alpha_754_Deviations"></span><span id="alpha_754_deviations"></span><span id="ALPHA_754_DEVIATIONS"></span>IEEE-754 규칙의 편차 또는 추가 요구 사항

-   IEEE-754을 사용 하려면 부동 소수점 연산에서 가장 가까운 표현 가능한 값인 무한 하 게 반올림 된 결과를 생성 합니다.

    Direct3D 11 및 up은 IEEE-754와 동일한 요구 사항을 정의 합니다. 32 비트 부동 소수점 연산에서는 무한 정확한 결과의 0.5 단위-끝 (ULP) 내에 있는 결과를 생성 합니다. 즉, 예를 들어 하드웨어는 최대 0.5 ULP의 오류를 초래 하는 것 처럼 라운드를 거의 사용 하지 않고 결과를 32 비트까지 자를 수 있습니다. 이 규칙은 더하기, 빼기 및 곱하기에만 적용 됩니다.

    이전 버전의 Direct3D는 IEEE-754 보다 완화 요구 사항을 정의 합니다. 32 비트 부동 소수점 연산에서는 무한 정확한 결과의 한 단위 (1-16) 내에 있는 결과를 생성 합니다. 즉, 예를 들어 하드웨어는 최대 하나의 ULP에 오류가 발생 하는 것 처럼 라운드를 거의 사용 하지 않고 결과를 32 비트까지 자를 수 있습니다.

-   부동 소수점 예외, 상태 비트 또는 트랩은 지원 되지 않습니다.
-   Denorms는 부동 소수점 수학적 작업의 입력 및 출력에서 부호 보존 0으로 플러시됩니다. 데이터를 조작 하지 않는 i/o 또는 데이터 이동 작업에 대 한 예외가 발생 합니다.
-   뷰포트 MinDepth/MaxDepth 또는 BorderColor values와 같이 부동 소수점 값을 포함 하는 상태는 denorm 값으로 제공 될 수 있으며 하드웨어에서 사용 하기 전에 플러시되지 않을 수도 있습니다.
-   비교에 대해 Min 또는 max 작업 플러시가 denorms 결과가 denorm 플러시되지 않을 수도 있습니다.
-   작업에 대 한 NaN 입력은 항상 출력 시 NaN을 생성 합니다. 그러나 NaN의 정확한 비트 패턴은 동일 하 게 유지 하는 데 필요 하지 않습니다 (작업은 데이터를 변경 하지 않는 원시 이동 명령인 경우는 제외).
-   하나의 피연산자만 NaN 인 Min 또는 max 연산은 다른 피연산자를 결과로 반환 합니다 (앞에서 살펴본 비교 규칙과 반대). IEEE 754R 규칙입니다.

    부동 소수점 min 및 max 연산의 IEEE-754R 사양에서는 min 또는 max에 대 한 입력 중 하나가 quiet QNaN 값인 경우 연산의 결과는 다른 매개 변수입니다. 예:

    ```ManagedCPlusPlus
    min(x,QNaN) == min(QNaN,x) == x (same for max)
    ```

    IEEE-754R 사양에 대 한 수정 사항은 한 입력이 "신호"로 값과 QNaN 값을 비교 하는 경우 min 및 max에 대해 다른 동작을 채택 했습니다.

    ```ManagedCPlusPlus
    min(x,SNaN) == min(SNaN,x) == QNaN (same for max)
     
    ```

    일반적으로 Direct3D는 IEEE-754 및 IEEE-754R의 표준을 따릅니다. 그러나이 경우에는 편차가 있습니다.

    Direct3D 10 이상에서 산술 규칙은 quiet 및 신호 NaN 값 (QNaN vs SNaN) 사이에서 차이가 없도록 합니다. 모든 NaN 값은 동일한 방식으로 처리 됩니다. Min 및 max의 경우 NaN 값에 대 한 Direct3D 동작은 QNaN이 IEEE-754R 정의에서 처리 되는 방식과 비슷합니다. 완전성을 위해-두 입력이 모두 NaN 이면 NaN 값이 반환 됩니다.

-   또 다른 IEEE 754R 규칙은 min (-0, + 0) = = min (+ 0,-0) = =-0, max (-0, + 0) = = max (+ 0,-0) = = + 0입니다 .이는 앞에서 살펴본 것 처럼 부호 있는 0에 대 한 비교 규칙과 달리 부호를 준수 합니다. Direct3D에서는 IEEE 754R 동작을 권장 하지만 적용 하지는 않습니다. 기호를 무시 하는 비교를 사용 하 여 0을 비교한 결과 매개 변수의 순서에 따라 달라질 수 있습니다.
-   x \* 1.0 f는 항상 x (denorm 플러시를 제외 하 고)를 생성 합니다.
-   x/1.0 f는 항상 x (denorm 플러시를 제외)를 생성 합니다.
-   x +/-0.0 f는 항상 x (denorm 플러시를 제외)로 생성 됩니다. -0 + 0 = + 0입니다.
-   퓨즈 작업 (예: 매드, dp3)은 작업의 unfused 확장 계산의 가장 낮은 가능한 순서 순서 보다 정확도가 낮은 결과를 생성 합니다. 허용 오차의 목적에 가장 최악의 정렬의 정의는 지정 된 퓨즈 작업에 대 한 고정 정의가 아닙니다. 입력의 특정 값에 따라 달라 집니다. Unfused 확장의 개별 단계는 각각 1 ULP 허용 오차를 허용 하 고, Direct3D 호출에는 1 ULP 보다 더 느슨한 허용 오차를 적용 하는 명령에 대해 보다 느슨한 허용 여부를 허용 합니다.
-   퓨즈 작업은 비 퓨즈 작업과 동일한 NaN 규칙을 따릅니다.
-   sqrt 및 rcp는 1 개의 ULP 허용 오차입니다. 셰이더 상호 및 상호 제곱 루트 명령 [**rcp**](/previous-versions/windows/desktop/legacy/hh447205(v=vs.85)) 및 [**rsq**](/windows/desktop/direct3dhlsl/rsq--sm4---asm-)에는 자체의 개별 완화 된 정밀도 요구 사항이 있습니다.
-   곱하기 및 나누기는 각각 32 비트 부동 소수점 전체 자릿수 수준으로 작동 합니다 (곱하기의 경우 정확도에서 0.5 ULP, 역의 경우 1.0 ULP). X/y가 직접 구현 되는 경우 결과는 2 단계 메서드 보다 크거나 같아야 합니다.

## <a name="span-iddouble_prec_64_bitspanspan-iddouble_prec_64_bitspan64-bit-double-precision-floating-point-rules"></a><span id="double_prec_64_bit"></span><span id="DOUBLE_PREC_64_BIT"></span>64 비트 (배정밀도) 부동 소수점 규칙


하드웨어 및 디스플레이 드라이버는 선택적으로 배정밀도 부동 소수점을 지원 합니다. 지원을 나타내려면 [**D3D11 \_ 기능 \_ **](/windows/desktop/api/d3d11/ne-d3d11-d3d11_feature)을 사용 하 여 [**ID3D11Device:: CheckFeatureSupport**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkfeaturesupport) 를 호출할 때 드라이버가 [**D3D11 \_ 기능 \_ 데이터 \_ **](/windows/desktop/api/d3d11/ns-d3d11-d3d11_feature_data_doubles) 의 **DoublePrecisionFloatShaderOps** 를 TRUE로 설정 합니다. 그런 다음 드라이버와 하드웨어는 모든 배정밀도 부동 소수점 명령을 지원 해야 합니다.

배정밀도 지침은 IEEE 754R 동작 요구 사항을 따릅니다.

이중 정밀도 데이터의 경우 비 정규화 된 값 생성에 대 한 지원이 필요 합니다 (플러시-제로 동작 없음). 마찬가지로,이 명령은 정규화 되지 않은 데이터를 부호 없는 데이터에 denorm 값을 적용 합니다.

## <a name="span-idalpha_16_bitspanspan-idalpha_16_bitspan16-bit-floating-point-rules"></a><span id="alpha_16_bit"></span><span id="ALPHA_16_BIT"></span>16 비트 부동 소수점 규칙


Direct3D에서는 부동 소수점 숫자의 16 비트 표현도 지원 합니다.

형식:

-   MSB 비트 위치에서 1 개의 부호 비트
-   바이어스 지 수의 5 비트 (e)
-   추가 숨겨진 비트가 있는 10 비트 (f)

Float16 값 (v)은 다음 규칙을 따릅니다.

-   e = = 31 및 f! = 0 이면 v는 s와 관계 없이 NaN입니다.
-   e = = 31 및 f = = 0 이면 v = (-1) s \* infinity (부호 있는 무한대)
-   e가 0에서 31 사이인 경우 v = (-1) s \* 2 (e-15) \* (1. f)
-   e = = 0 및 f! = 0 이면 v = (-1) s \* 2 (e-14) \* (0 .f) (비 정규화 된 숫자)
-   e = = 0 및 f = = 0 이면 v = (-1) s \* 0 (부호 있는 0)

32 비트 부동 소수점 규칙은 앞에서 설명한 비트 레이아웃에 맞게 조정 된 16 비트 부동 소수점 숫자에도 적용 됩니다. 이에 대 한 예외는 다음과 같습니다.

-   전체 자릿수: 16 비트 부동 소수점 숫자에 대 한 Unfused 연산은 무한 정밀도 결과에 가장 가까운 표현 가능한 값 (16 비트 값에 적용 되는 IEEE-754에 따라 가장 가까운 값으로 반올림)의 결과를 생성 합니다. 32 비트 부동 소수점 규칙은 1 ULP 허용 오차, 16 비트 부동 소수점 규칙은 unfused 작업을 위한 0.5 ULP 및 퓨즈 작업을 위한 0.6 ULP를 준수 합니다.
-   16 비트 부동 소수점 숫자는 denorms을 유지 합니다.

## <a name="span-idalpha_11_bitspanspan-idalpha_11_bitspan11-bit-and-10-bit-floating-point-rules"></a><span id="alpha_11_bit"></span><span id="ALPHA_11_BIT"></span>11 비트 및 10 비트 부동 소수점 규칙


Direct3D는 또한 11 비트 및 10 비트 부동 소수점 형식을 지원 합니다.

형식:

-   부호 비트 없음
-   바이어스 지 수의 5 비트 (e)
-   11 비트 형식에 대 한 6 비트 (f), 10 비트 형식의 경우 5 비트 (f), 두 경우 모두에 숨겨진 비트가 추가 됩니다.

Float11/float10 값 (v)은 다음 규칙을 따릅니다.

-   e = = 31 및 f! = 0 인 경우 v는 NaN입니다.
-   e = = 31 및 f = = 0 이면 v = + infinity
-   e가 0에서 31 사이인 경우 v = 2 (e-15) \* (1. f)
-   e = = 0 및 f! = 0 이면 v = \* 2 (e-14) ( \* 0. f) (비 정규화 숫자)
-   e = = 0 및 f = = 0 인 경우 v = 0 (0)

32 비트 부동 소수점 규칙은 앞에서 설명한 비트 레이아웃에 맞게 조정 된 11 비트 및 10 비트 부동 소수점 숫자에도 적용 됩니다. 다음과 같은 예외가 있습니다.

-   전체 자릿수: 32 비트 부동 소수점 규칙은 0.5 ULP를 준수 합니다.
-   10/11 비트 부동 소수점 숫자는 denorms을 유지 합니다.
-   0 보다 작은 숫자를 반환 하는 작업은 0으로 고정.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[부록](appendix.md)

[리소스](/windows/desktop/direct3d11/overviews-direct3d-11-resources)

[질감](/windows/desktop/direct3d11/overviews-direct3d-11-resources-textures)

 

 