---
title: "데이터 형식 변환"
description: "다음 섹션 Direct3D가 데이터 형식 간 변환을 처리하는 방법을 설명합니다."
ms.assetid: B50AB8DE-CAED-465B-B18C-81F3A984B8AC
keywords: "데이터 형식 변환"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 6813f7b55957c185a85fe82b90297b6ba5a470eb
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="data-type-conversion"></a>데이터 형식 변환


다음 섹션은 Direct3D가 데이터 형식 간 변환을 처리하는 방법을 설명합니다.

## <a name="span-iddatatypeterminologyspanspan-iddatatypeterminologyspanspan-iddatatypeterminologyspandata-type-terminology"></a><span id="Data_Type_Terminology"></span><span id="data_type_terminology"></span><span id="DATA_TYPE_TERMINOLOGY"></span>데이터 형식 용어


다음 용어 집합은 이후 다양한 형식의 변환의 특징을 정의하는 데 사용됩니다.

| 용어  | 정의                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SNORM | 부호 있는 정규화된 정수. n비트 2의 보수 정수를 의미합니다. 최대값인 1.0f(예: 5비트 값 01111은 1.0f에 매핑)를 의미하며 최소값인 -1.0f(예: 5비트 값 10000은 -1.0f에 매핑)을 의미합니다. 또한 두 번째 최소 숫자는 -1.0f(예: 5비트 값 10001은 -1.0f에 매핑)에 매핑됩니다. 따라서-1.0f를 나타내는 정수 표현은 두 가지입니다. 0.0f에 대한 표현은 하나이며 1.0f에 대한 표현도 하나입니다. 이로 인해 (-1.0f...0.0f) 범위의 고른 간격을 가진 및 부동 소수점 값을 정수 표현 집합으로 나타낼 수 있고 (0.0f...1.0f) 범위의 숫자에 대한 보수 정수 표현 집합으로 나타낼 수 있습니다. |
| UNORM | 부호가 없는 정규화된 정수. n비트 숫자를 의미합니다. 모든 0은 0.0f를 의미하며 모든 1은 1.0f를 의미합니다. 0.0f에서 1.0f 범위의 고른 간격을 가진 부동 소수점 값이 순차적으로 표현됩니다. 예를 들어 2비트 UNORM은 0.0f, 1/3, 2/3 및 1.0f를 나타냅니다.                                                                                                                                                                                                                                                                                                                                                                                                                                |
| SINT  | 부호 있는 정수입니다. 2의 보수 정수입니다. 예를 들어 3비트 SINT는 적분 값인 -4, -3, -2, -1, 0, 1, 2, 3을 나타냅니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| UINT  | 부호 없는 정수입니다. 예를 들어 3비트 UINT은 적분 값인 0, 1, 2, 3, 4, 5, 6, 7을 나타냅니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| FLOAT | Direct3D에 정의된 부동 소수점 값의 모든 표현입니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| SRGB  | UNORM과 유사하게, n비트 숫자를 위한 것입니다. 모든 0은 0.0f이며 모든 1은 1.0f를 의미합니다. 그러나 UNORM과 다르게, SRGB를 통한 모든 0에서 모든 1까지의 부호 없는 정수의 순차적 인코딩은 0.0f에서 1.0f 사이의 부동 소수점 숫자 표현의 비선형 수열입니다. 대략적으로 말해, 이 비선형 수열인 SRGB를 순차적 색상으로 표시하면, 관찰자에게 광도 수준이 "평균"인 선형 램프로 표시될 것이며, 즉 "평균"적인 관찰 상태에서 "평균" 디스플레이로 표현됩니다. 완전한 정보는 IEC(국제 전자기술 위원회)의 SRGB 표준인 IEC 61996-2-1을 참조하세요.                |

 

위 용어는 "형식 이름 한정자"로도 자주 사용되며, 데이터가 메모리에 어떻게 배치되는지, 메모리나 셰이더와 같은 파이프라인에서 전송 경로(필터링 포함 가능)에서 어떤 전송이 수행되는지 설명합니다.

## <a name="span-idfloatingpointconversionspanspan-idfloatingpointconversionspanspan-idfloatingpointconversionspanfloating-point-conversion"></a><span id="Floating_Point_Conversion"></span><span id="floating_point_conversion"></span><span id="FLOATING_POINT_CONVERSION"></span>부동 소수점 변환


부동 소수점이 아닌 표현을 포함하여 서로 다른 표현의 부동 소수점을 변환할 때는 다음 규칙이 적용됩니다.

### <a name="span-idconververtingfromahigherrangerepresentationtoalowerrangerepresentationspanspan-idconververtingfromahigherrangerepresentationtoalowerrangerepresentationspanspan-idconververtingfromahigherrangerepresentationtoalowerrangerepresentationspanconververting-from-a-higher-range-representation-to-a-lower-range-representation"></a><span id="Conververting_from_a_higher_range_representation_to_a_lower_range_representation"></span><span id="conververting_from_a_higher_range_representation_to_a_lower_range_representation"></span><span id="CONVERVERTING_FROM_A_HIGHER_RANGE_REPRESENTATION_TO_A_LOWER_RANGE_REPRESENTATION"></span>높은 범위의 표현에서 낮은 범위의 표현으로 변환

-   다른 부동 소수점 형식으로 변환할 때는 0으로 반올림이 사용됩니다. 대상이 정수나 고정 소수점 형식인 경우, 가장 가까운 짝수로 반올림이 사용됩니다. FLOAT을 가장 가까운 SNORM으로, FLOAT을 UNORM, FLOAT을 SRGB로 반올림하라는 명확한 변환 지침이 있는 경우에는 예외입니다. 다른 예외로는 0으로 반올림하는 ftoi 및 ftou 셰이더 지침이 있습니다. 마지막으로, 부동 소수점에서 고정 소수점으로의 변환은 무한 정밀도 방식으로 마지막 위치 단위에서 측정된 지정된 허용 범위를 가진 텍스처 샘플러 및 래스터라이저에서 사용됩니다.
-   낮은 범위 대상 형식의 동적 범위보다 큰 원본 값(예: 큰 32비트 부동 소수점 값이 16비트 부동 소수점 RenderTarget으로 기록)의 경우, 최대값 표현(적절한 부호 사용) 값 결과가 사용됩니다. (위에서 설명한 0으로의 반올림 때문에) 부호 있는 무한의 값은 포함되지 않습니다.
-   더 높은 범위 형식의 NaN은 더 낮은 범위 형식의 NaN 표현이 있을 경우 더 낮은 범위 형식의 NaN 표현으로 변환됩니다. 더 낮은 형식의 NaN 표현이 없으면 결과는 0이 됩니다.
-   더 높은 범위 형식의 INF는 더 낮은 범위 형식을 사용할 수 있을 경우 이 INF로 변환됩니다. 더 낮은 형식에 INF 표현이 없으면 표현 가능한 최대값 표현으로 변환됩니다. 대상 형식에서 사용할 수 있다면 부호는 유지됩니다.
-   더 높은 범위 형식의 Denorm은 더 낮은 형식으로 변환이 가능할 경우 더 낮은 범위 형식으로 변환됩니다. 그렇지 않으면 결과는 0이 됩니다. 대상 형식에서 사용할 수 있다면 부호 비트는 유지됩니다.

### <a name="span-idconvertingfromalowerrangerepresentationtoahigherrangerepresentationspanspan-idconvertingfromalowerrangerepresentationtoahigherrangerepresentationspanspan-idconvertingfromalowerrangerepresentationtoahigherrangerepresentationspanconverting-from-a-lower-range-representation-to-a-higher-range-representation"></a><span id="Converting_from_a_lower_range_representation_to_a_higher_range_representation"></span><span id="converting_from_a_lower_range_representation_to_a_higher_range_representation"></span><span id="CONVERTING_FROM_A_LOWER_RANGE_REPRESENTATION_TO_A_HIGHER_RANGE_REPRESENTATION"></span>더 낮은 범위 표현에서 더 큰 범위 형식으로 변환

-   더 낮은 범위 형식의 NaN은 더 높은 범위 형식의 NaN 표현이 있을 경우 더 높은 범위 형식의 NaN 표현으로 변환됩니다. 더 높은 범위 형식에 NaN 표현이 없으면 0으로 변환됩니다.
-   더 낮은 범위 형식의 INF는 더 높은 범위 형식의 INF 표현이 있을 경우 더 높은 범위 형식의 INF 표현으로 변환됩니다. 더 높은 형식에 INF 표현이 없으면 표현할 수 있는 최대값(해당 형식의 MAX\_FLOAT)으로 변환됩니다. 대상 형식에서 사용할 수 있다면 부호는 유지됩니다.
-   낮은 범위 형식의 Denorm은 더 높은 범위 형식이 가능할 경우 정규화된 표현으로 변환됩니다. 또는 더 높은 범위 형식의 Denorm 표현이 있을 경우 이로 표현됩니다. 더 높은 범위 형식에 Denorm 표현이 없어 변환에 실패할 경우, 0으로 변환됩니다. 대상 형식에서 사용할 수 있다면 부호는 유지됩니다. 32비트 부동 소수점 숫자는 Denorm 표현 없는 형식으로 계산됩니다(32비트 부동 소수점에 대한 연산에서 Denorm은 부호 있는 0으로 플러시되기 때문).

## <a name="span-idintegerconversionspanspan-idintegerconversionspanspan-idintegerconversionspaninteger-conversion"></a><span id="Integer_Conversion"></span><span id="integer_conversion"></span><span id="INTEGER_CONVERSION"></span>정수 변환


다음 표는 위에서 설명한 다양한 표현을 다른 표현으로 변환하는 것에 대해 설명합니다. Direct3D에서만 실제 발생하는 변환은 다음과 같습니다.

달리 지정되지 않은 경우, 아래 설명된 정수에서 부동 소수점, 부동 소수점에서 정수로의 모든 변환은 정확히 수행됩니다.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">원본 데이터 형식</th>
<th align="left">대상 데이터 형식</th>
<th align="left">변환 규칙</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">SNORM</td>
<td align="left">FLOAT</td>
<td align="left"><p>부호가 있는 범위[-1.0f에서 1.0f]를 나타내는 n비트 정수 범위는 다음과 같이 부동 소수점으로 변환됩니다.</p>
<ul>
<li>가장 작은 음의 값은 -1.0f로 매핑됩니다. 예를 들어 있는 5비트 값 10000은 -1.0f로 매핑됩니다.</li>
<li>부동 소수점(c라고 함)으로 변환되는 다른 모든 값의 경우 결과는 c * (1.0f / (2⁽ⁿ⁻¹⁾-1)입니다. 예를 들어 5비트 값 10001은 -15.0f로 변환되고, 15.0f로 나뉘어 -1.0f가 됩니다.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">FLOAT</td>
<td align="left">SNORM</td>
<td align="left"><p>부동 소수점 숫자를 n비트 정수로 전환하면, 다음과 같이 부호가 있는 범위 [-1.0f에서 1.0f]로 표현됩니다.</p>
<ul>
<li>시작 값을 c라고 생각하겠습니다.</li>
<li>c가 NaN이면, 결과는 0입니다.</li>
<li>INF를 포함하여 c &gt; 1.0f인 경우, 1.0f로 고정됩니다.</li>
<li>-INF를 포함하여 c &lt; -1.0f인 경우, -1.0f로 고정됩니다.</li>
<li>부동 소수점 배율에서 정수 배율로 변환: c = c * (2ⁿ⁻¹-1).</li>
<li>정수로의 변환은 다음과 같습니다.
<ul>
<li>C &gt;= 0이면 c = c + 0.5f이고, 그렇지 않은 경우, c = c - 0.5f입니다.</li>
<li>소수점 부분은 삭제하고, 나머지 부동 소수점(적분) 값은 정수로 직접 변환됩니다.</li>
</ul></li>
</ul>
<p>이 변환은 D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_Unit-Last-Place 마지막 위치 단위(정수 쪽)의 허용 범위 내에서 허용됩니다. 이는 부동 소수점에서 정수 배율로 변환된 후, D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP 마지막 위치 단위 내의 표현 가능한 대상 형식 값은 해당 값으로 매핑 가능함을 의미합니다. 추가 데이터의 반전을 위해서는 변환이 범위 내에서 증가하지 않고 모든 출력 값이 달성 가능해야 합니다. (여기 표시된 상수 <em>xx</em>는 Direct3D 버전(예: 10, 11, 12)으로 대체되어야 합니다.)</p></td>
</tr>
<tr class="odd">
<td align="left">UNORM</td>
<td align="left">FLOAT</td>
<td align="left"><p>시작 n비트 값은 부동 소수점(0.0f, 1.0f, 2.0f 등)으로 변환된 다음 (2ⁿ-1)로 나눕니다.</p></td>
</tr>
<tr class="even">
<td align="left">FLOAT</td>
<td align="left">UNORM</td>
<td align="left"><p>시작 값을 c라고 생각하겠습니다.</p>
<ul>
<li>c가 NaN이면, 결과는 0입니다.</li>
<li>INF를 포함하여 c &gt; 1.0f인 경우, 1.0f로 고정됩니다.</li>
<li>INF를 포함하여 c &lt; 0.0f인 경우, 0.0f로 고정됩니다.</li>
<li>부동 소수점 배율에서 정수 배율로 변환: c = c * (2ⁿ-1).</li>
<li>정수로의 변환.
<ul>
<li>c = c + 0.5f.</li>
<li>소수점 부분은 삭제하고, 나머지 부동 소수점(적분) 값은 정수로 직접 변환됩니다.</li>
</ul></li>
</ul>
<p>이 변환은 D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP 마지막 위치 단위(정수 쪽)의 오차 범위로 허용됩니다. 이는 부동 소수점에서 정수 배율로 변환된 후, D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP 마지막 위치 단위 내의 표현 가능한 대상 형식 값은 해당 값으로 매핑 가능함을 의미합니다. 추가 데이터의 반전을 위해서는 변환이 범위 내에서 증가하지 않고 모든 출력 값이 달성 가능해야 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left">SRGB</td>
<td align="left">FLOAT</td>
<td align="left"><p>다음은 SRGB에서 FLOAT으로의 이상적인 변환입니다.</p>
<ul>
<li>시작 n비트 값을 부동 소수점(0.0f, 1.0f, 2.0f 등)으로 변환합니다. 이를 c라고 하겠습니다.</li>
<li>c = c * (1.0f / (2ⁿ-1))</li>
<li>(c &lt; = D3D<em>xx</em>_SRGB_TO_FLOAT_THRESHOLD)인 경우, 결과는 c / D3D<em>xx</em>_SRGB_TO_FLOAT_DENOMINATOR_1이며, 그렇지 않을 경우 결과는 ((c + D3D<em>xx</em>_SRGB_TO_FLOAT_OFFSET)/D3D<em>xx</em>_SRGB_TO_FLOAT_DENOMINATOR_2)D3D<em>xx</em>_SRGB_TO_FLOAT_EXPONENT입니다.</li>
</ul>
<p>이 변환은 D3D<em>xx</em>_SRGB_TO_FLOAT_TOLERANCE_IN_ULP 마지막 위치 단위(SRGB 쪽)의 오차 범위로 허용됩니다.</p></td>
</tr>
<tr class="even">
<td align="left">FLOAT</td>
<td align="left">SRGB</td>
<td align="left"><p>다음은 FLOAT -&gt; SRGB로의 이상적인 변환입니다.</p>
<p>대상 SRGB 색상 구성 요소에 n비트가 있다고 가정하겠습니다.</p>
<ul>
<li>시작 값이 c인 것으로 가정합니다.</li>
<li>c가 NaN이면, 결과는 0입니다.</li>
<li>INF를 포함하여 c &gt; 1.0f인 경우, 1.0f로 고정됩니다.</li>
<li>INF를 포함하여 c &lt; 0.0f인 경우, 0.0f로 고정됩니다.</li>
<li>만약 (c &lt;= D3D<em>xx</em>_FLOAT_TO_SRGB_THRESHOLD)인 경우 c = D3D<em>xx</em>_FLOAT_TO_SRGB_SCALE_1 * c이며, 그렇지 않을 경우 c = D3D<em>xx</em>_FLOAT_TO_SRGB_SCALE_2 * c(D3D<em>xx</em>_FLOAT_TO_SRGB_EXPONENT_NUMERATOR/D3D<em>xx</em>_FLOAT_TO_SRGB_EXPONENT_DENOMINATOR) - D3D<em>xx</em>_FLOAT_TO_SRGB_OFFSET입니다.</li>
<li>부동 소수점 배율에서 정수 배율로 변환: c = c * (2ⁿ-1).</li>
<li>정수로의 변환:
<ul>
<li>c = c + 0.5f.</li>
<li>소수점 부분은 삭제하고, 나머지 부동 소수점(적분) 값은 정수로 직접 변환됩니다.</li>
</ul></li>
</ul>
<p>이 변환은 D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP 마지막 위치 단위(정수 쪽)의 오차 범위로 허용됩니다. 이는 부동 소수점에서 정수 배율로 변환된 후, D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP 마지막 위치 단위 내의 표현 가능한 대상 형식 값은 해당 값으로 매핑 가능함을 의미합니다. 추가 데이터의 반전을 위해서는 변환이 범위 내에서 증가하지 않고 모든 출력 값이 달성 가능해야 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left">SINT</td>
<td align="left">더 많은 비트를 가진 SINT</td>
<td align="left"><p>SINT에서 더 많은 비트를 가진 SINT로 변환하려면, 시작 숫자의 최상위 비트(MSB)는 대상 형식에서 사용 가능한 &quot;부호 추가&quot; 추가 비트가 됩니다.</p></td>
</tr>
<tr class="even">
<td align="left">UINT</td>
<td align="left">더 많은 비트를 가진 SINT</td>
<td align="left"><p>UINT에서 더 많은 비트를 가진 SINT로 변환하려면, 숫자가 대상 형식의 최하위 비트(LSB)에 복사되고 추가 MSB는 0으로 채워집니다.</p></td>
</tr>
<tr class="odd">
<td align="left">SINT</td>
<td align="left">더 많은 비트를 가진 UINT</td>
<td align="left"><p>SINT를 더 많은 비트를 가진 UINT로 변환하려면, 음수인 경우 값은 0으로 고정됩니다. 그렇지 않으면 숫자가 대상 형식의 LSB로 복사되고 추가 MSB는 0으로 채워집니다.</p></td>
</tr>
<tr class="even">
<td align="left">UINT</td>
<td align="left">더 많은 비트를 가진 UINT</td>
<td align="left"><p>UNIT을 더 많은 비트를 가진 UINT로 변환하려면 숫자가 대상 형식의 LSB로 복사되고 추가 MSB는 0으로 채워집니다.</p></td>
</tr>
<tr class="odd">
<td align="left">SINT 또는 UINT</td>
<td align="left">SINT 또는 더 적거나 같은 비트를 가진 UINT</td>
<td align="left"><p>SINT 또는 UINT를 더 적거나 같은 비트를 가진 UNIT 또는 SINT로 변환(및/또는 부호가 없는 숫자로 변경)하려면 시작 값은 간단히 대상 범위의 범위로 고정됩니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idfixedpointintegerconversionspanspan-idfixedpointintegerconversionspanspan-idfixedpointintegerconversionspanfixed-point-integer-conversion"></a><span id="Fixed_Point_Integer_Conversion"></span><span id="fixed_point_integer_conversion"></span><span id="FIXED_POINT_INTEGER_CONVERSION"></span>고정 소수점 정수 변환


고정 소수점 정수는 간단하게 일부 비트 크기가 고정된 위치에 암시적 소수점이 있는 것입니다.

흔히 볼 수 있는 "정수" 데이터 형식은 숫자 끝에 소수점이 있는 고정 소수점 정수의 특별한 사례입니다.

고정된 소수점 숫자는 i.f로 표현됩니다. i는 정수 비트의 숫자이며 f는 소수 비트의 숫자입니다. 예를 들어 16.8은 16비트 정수에 8비트 소수점이 따르는 형태를 의미합니다. 정수 부분은 여기에 정의된 바에 따라 2의 보수에 저장됩니다(물론 동일하게 정의되거나 부호 없는 정수로 저장될 수도 있음). 소수점 부분은 부호가 없는 형식으로 저장됩니다. 소수점 부분은 항상 가장 음수 부분에서 시작하여 가장 가까운 두 적분 값 사이의 양수 부분을 나타냅니다.

고정 소수점에 대한 더하기 및 빼기 작업은 묵시적 소수점 위치를 고려하지 않고 표준 정수 산술을 사용하여 간단하게 수행됩니다. 1에 16.8 고정 소수점 숫자를 더하면 256을 더하는 것이 됩니다. 소수점 8이 숫자 끝의 최하위 비트에 위치하기 때문입니다. 곱하기와 같은 기타 연산도 정수 산술을 사용하여 간단하게 수행됩니다. 고정 소수점의 영향 때문입니다. 예를 들어, 정수 곱하기를 사용하여 두 16.8 정수를 곱하면 32.16의 결과가 나옵니다.

고정 소수점 정수 표현은 Direct3D에서 두 가지 방법으로 사용됩니다.

-   래스터라이저의 사전 잘림 꼭짓점 위치는 RenderTarget 영역 전체에서 일관되게 정밀도를 유지하기 위해 고정 소수점으로 스냅됩니다. 앞면 컬링의 예와 같은 많은 래스터라이저 작업은 고정 소수점 스냅 위치에서 발생하며, 특성 보정 설정과 같은 다른 작업은 고정 소수점 스냅 위치에서 부동 소수점으로 다시 변환된 위치를 사용합니다.
-   샘플링 작업을 위한 텍스처 위치는 고정 소수점으로 스냅(텍스처 크기에 의해 크기 조정된 후)되어 필터 탭 위치/가중치를 선택하여 텍스처 공간 내에서 정밀도를 고유하게 유지합니다. 실제 필터링 산술이 수행되기 전에 가중치 값은 고정 소수점 값으로 변환됩니다.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">원본 데이터 형식</th>
<th align="left">대상 데이터 형식</th>
<th align="left">변환 규칙</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">FLOAT</td>
<td align="left">고정 소수점 정수</td>
<td align="left"><p>다음은 부동 소수점 숫자 n을 고정 소수점 정수 i.f로 변환하는 일반적인 절차입니다. i는 부호가 있는 정수 비트의 숫자이며 f는 소수점 비트의 숫자입니다.</p>
<ul>
<li>FixedMin 계산 = -2⁽ⁱ⁻¹⁾</li>
<li>FixedMax 계산 = 2⁽ⁱ⁻¹⁾ - 2<sup>(-f)</sup></li>
<li>n이 NaN이면 결과는 0이며, n이 +Inf인 경우 결과는 FixedMax*2<sup>f</sup>, n이 -Inf인 경우 결과는 FixedMin*2<sup>f</sup>입니다.</li>
<li>n &gt;= FixedMax인 경우 결과는 Fixedmax*2<sup>f</sup>이며, n &lt;= FixedMin인 경우 결과는 FixedMin*2<sup>f</sup>입니다.</li>
<li>다른 경우 n*2<sup>f</sup>를 계산하고 정수로 변환합니다.</li>
</ul>
<p>위 마지막 단계 후 무한 정밀 값인 n*2<sup>f</sup> 대신, 정수 결과에서 D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP 마지막 위치 단위 오차 범위 내의 구현이 허용됩니다.</p></td>
</tr>
<tr class="even">
<td align="left">고정 소수점 정수</td>
<td align="left">FLOAT</td>
<td align="left"><p>특정 고정 소수점 표현이 부동 소수점으로 변환되면 총 24비트의 정보를 포함하지 않으며, 23비트를 넘는 소수 부분이 포함되지 않습니다. i.f 형식(i 비트 정수, f 비트 소수점)인 고정 소수점 숫자 fxp를 가정해 보겠습니다. 부동 소수점으로의 변환은 다음 가상 코드와 유사합니다.</p>
<p>부동 소수점 결과 = (float)(fxp &gt;&gt; f) + // 정수 추출</p>
((float)(fxp &amp; (2<sup>f</sup> - 1)) / (2<sup>f</sup>)); // 소수점 추출</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[부록](appendix.md)

 

 




