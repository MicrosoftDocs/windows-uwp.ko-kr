---
title: BC6H 형식
description: BC6H 형식은 원본 데이터에서 HDR (high dynamic range) 색 공간을 지원 하도록 디자인 된 질감 압축 형식입니다.
ms.assetid: 6781D967-9262-4EE7-B354-7A6D0EA0498E
keywords:
- BC6H 형식
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5f53ebf6a7326bd9e6a99272c01d9eeb5c03f580
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165077"
---
# <a name="bc6h-format"></a>BC6H 형식


BC6H 형식은 원본 데이터에서 HDR (high dynamic range) 색 공간을 지원 하도록 디자인 된 질감 압축 형식입니다.

## <a name="span-idabout-bc6h-dxgi-format-bc6hspanspan-idabout-bc6h-dxgi-format-bc6hspanspan-idabout-bc6h-dxgi-format-bc6hspanabout-bc6hdxgi_format_bc6h"></a><span id="About-BC6H-DXGI-FORMAT-BC6H"></span><span id="about-bc6h-dxgi-format-bc6h"></span><span id="ABOUT-BC6H-DXGI-FORMAT-BC6H"></span>BC6H/DXGI \_ 형식 정보 \_ BC6H


BC6H 형식은 세 가지 HDR 색 채널을 사용 하 고 값 (16:16:16)의 각 색 채널에 대해 16 비트 값을 사용 하는 이미지에 대 한 고품질 압축을 제공 합니다. 알파 채널에 대 한 지원은 없습니다.

BC6H는 다음 DXGI \_ 형식 열거 값으로 지정 됩니다.

-   **DXGI \_ 형식 \_ 지정 \_ BC6H**.
-   **DXGI \_ \_BC6H \_ UF16 형식으로 지정**합니다. 이 BC6H 형식은 16 비트 부동 소수점 색 채널 값에 부호 비트를 사용 하지 않습니다.
-   **DXGI \_ \_BC6H \_ SF16 형식으로 지정**합니다. 이 BC6H 형식은 16 비트 부동 소수점 색 채널 값에 부호 비트를 사용 합니다.

**참고**    색 채널에 대 한 16 비트 부동 소수점 형식을 종종 "절반" 부동 소수점 형식 이라고 합니다. 이 형식의 비트 레이아웃은 다음과 같습니다.
|                       |                                                 |
|-----------------------|-------------------------------------------------|
| UF16 (부호 없는 float) | 5 지 수 비트 + 11가 수 비트              |
| SF16 (부호 있는 float)   | 1 부호 비트 + 5 지 수 비트 + 10 개의가 수 비트 |

 

 

BC6H 형식은 [Texture2D](/windows/desktop/direct3d10/d3d10-graphics-reference-resource-structures) (배열 포함), Texture3D 또는 TextureCube (배열 포함) 질감 리소스에 사용할 수 있습니다. 마찬가지로이 형식은 이러한 리소스와 연결 된 모든 밉 맵 표면에 적용 됩니다.

BC6H는 16 바이트 (128 비트)의 고정 된 블록 크기와 4x4 텍셀 고정 타일 크기를 사용 합니다. 이전 BC 형식과 마찬가지로, 지원 되는 타일 크기 (4x4) 보다 큰 질감 이미지는 여러 블록을 사용 하 여 압축 됩니다. 이 주소 지정 id는 3 차원 이미지, 밉 맵, 큐브 맵 및 질감 배열에도 적용 됩니다. 모든 이미지 타일은 같은 형식 이어야 합니다.

BC6H 형식의 주의 사항:

-   BC6H는 부동 소수점 비 정규화를 지원 하지만 INF (infinity) 및 NaN (숫자가 아님)을 지원 하지 않습니다. 예외는 \_ \_ \_ -INF (음의 무한대)를 지 원하는 BC6H (DXGI FORMAT BC6H SF16)의 서명 된 모드입니다. -INF에 대 한이 지원은 단순히 형식 자체의 아티팩트 이며이 형식의 인코더에서 특별히 지원 되지 않습니다. 일반적으로 인코더에서 INF (긍정 또는 부정) 또는 NaN 입력 데이터가 발견 되 면 인코더는 해당 데이터를 허용 되는 최대 비 INF 표현 값으로 변환 하 고, 압축 하기 전에 NaN을 0으로 매핑합니다.
-   BC6H는 알파 채널을 지원 하지 않습니다.
-   BC6H 디코더는 질감 필터링을 수행 하기 전에 압축 풀기를 수행 합니다.
-   BC6H 압축 풀기는 정확 해야 합니다. 즉, 하드웨어는이 설명서에 설명 된 디코더와 동일한 결과를 반환 해야 합니다.

## <a name="span-idbc6h-implementationspanspan-idbc6h-implementationspanspan-idbc6h-implementationspanbc6h-implementation"></a><span id="BC6H-implementation"></span><span id="bc6h-implementation"></span><span id="BC6H-IMPLEMENTATION"></span>BC6H 구현


BC6H 블록은 모드 비트, 압축 된 끝점, 압축 된 인덱스 및 선택적 파티션 인덱스로 구성 됩니다. 이 형식은 14 가지 다른 모드를 지정 합니다.

끝점 색은 RGB 세 개로 저장 됩니다. BC6H은 정의 된 여러 색 끝점에서 대략적인 줄에 색의 색상표를 정의 합니다. 또한 모드에 따라 타일을 두 영역으로 구분 하거나 단일 지역으로 취급할 수 있습니다. 여기서 두 지역 타일에는 각 지역에 대 한 별도의 색 끝점 집합이 있습니다. BC6H는 텍셀 당 하나의 색상표 인덱스를 저장 합니다.

두 지역에 있는 경우 32 개의 파티션이 있습니다.

## <a name="span-iddecoding-the-bc6h-formatspanspan-iddecoding-the-bc6h-formatspanspan-iddecoding-the-bc6h-formatspandecoding-the-bc6h-format"></a><span id="Decoding-the-BC6H-format"></span><span id="decoding-the-bc6h-format"></span><span id="DECODING-THE-BC6H-FORMAT"></span>BC6H 형식 디코딩


아래 의사 코드에서는 16 바이트 BC6H 블록을 지정 하 여 (x, y)에 픽셀의 압축을 푸는 단계를 보여 줍니다.

``` syntax
decompress_bc6h(x, y, block)
{
    mode = extract_mode(block);
    endpoints;
    index;
    
    if(mode.type == ONE)
    {
        endpoints = extract_compressed_endpoints(mode, block);
        index = extract_index_ONE(x, y, block);
    }
    else //mode.type == TWO
    {
        partition = extract_partition(block);
        region = get_region(partition, x, y);
        endpoints = extract_compressed_endpoints(mode, region, block);
        index = extract_index_TWO(x, y, partition, block);
    }
    
    unquantize(endpoints);
    color = interpolate(index, endpoints);
    finish_unquantize(color);
}
```

다음 표에는 BC6H 블록에 사용할 수 있는 14 가지 형식 각각에 대 한 비트 수 및 값이 나와 있습니다.

| Mode | 파티션 인덱스 | 파티션 | 색 끝점                  | 모드 비트      |
|------|-------------------|-----------|----------------------------------|----------------|
| 1    | 46 비트           | 5 비트    | 75 비트 (10.555, 10.555, 10.555) | 2 비트 (00)    |
| 2    | 46 비트           | 5 비트    | 75 비트 (7666, 7666, 7666)       | 2 비트 (01)    |
| 3    | 46 비트           | 5 비트    | 72 비트 (11.555, 11.444, 11.444) | 5 비트 (00010) |
| 4    | 46 비트           | 5 비트    | 72 비트 (11.444, 11.555, 11.444) | 5 비트 (00110) |
| 5    | 46 비트           | 5 비트    | 72 비트 (11.444, 11.444, 11.555) | 5 비트 (01010) |
| 6    | 46 비트           | 5 비트    | 72 비트 (9555, 9555, 9555)       | 5 비트 (01110) |
| 7    | 46 비트           | 5 비트    | 72 비트 (8666, 8555, 8555)       | 5 비트 (10010) |
| 8    | 46 비트           | 5 비트    | 72 비트 (8555, 8666, 8555)       | 5 비트 (10110) |
| 9    | 46 비트           | 5 비트    | 72 비트 (8555, 8555, 8666)       | 5 비트 (11010) |
| 10   | 46 비트           | 5 비트    | 72 비트 (6666, 6666, 6666)       | 5 비트 (11110) |
| 11   | 63 비트           | 0 비트    | 60 비트 (10.10, 10.10, 10.10)    | 5 비트 (00011) |
| 12   | 63 비트           | 0 비트    | 60 비트 (11.9, 11.9, 11.9)       | 5 비트 (00111) |
| 13   | 63 비트           | 0 비트    | 60 비트 (12.8, 12.8, 12.8)       | 5 비트 (01011) |
| 14   | 63 비트           | 0 비트    | 60 비트 (16.4, 16.4, 16.4)       | 5 비트 (01111) |

 

이 테이블의 각 형식은 모드 비트로 고유 하 게 식별할 수 있습니다. 처음 10 개 모드는 2 개 지역 타일에 사용 되며 모드 비트 필드는 2 비트 또는 5 비트 중 하나일 수 있습니다. 이러한 블록에는 압축 된 색 끝점 (72 또는 75 비트), 파티션 (5 비트) 및 파티션 인덱스 (46 비트)에 대 한 필드도 포함 됩니다.

압축 된 색 끝점의 경우 앞의 표에 있는 값은 저장 된 RGB 끝점의 전체 자릿수와 각 색 값에 사용 되는 비트 수를 확인 합니다. 예를 들어 모드 3은 색 엔드포인트 전체 자릿수 수준을 11로 지정 하 고, 빨강, 파랑 및 녹색 색 (각각 5, 4 및 4)에 대해 변환 된 끝점의 델타 값을 저장 하는 데 사용 되는 비트 수를 지정 합니다. 모드 10은 델타 압축을 사용 하지 않고 대신 4 가지 색 끝점을 모두 명시적으로 저장 합니다.

마지막 4 개 블록 모드는 단일 지역 타일에 사용 되며, 여기서 mode 필드는 5 비트입니다. 이러한 블록에는 끝점에 대 한 필드 (60 비트)와 압축 된 인덱스 (63 비트)가 있습니다. 모드 11 (예: 모드 10)은 델타 압축을 사용 하지 않고 대신 두 색 끝점을 모두 명시적으로 저장 합니다.

모드 10011, 10111, 11011 및 11111 (표시 되지 않음)이 예약 되어 있습니다. 인코더에서 사용 하지 마십시오. 이러한 모드 중 하나를 지정 하 여 하드웨어가 전달 되는 경우 압축을 푼 블록은 알파 채널을 제외 하 고 모든 채널에서 0을 모두 포함 해야 합니다.

BC6H의 경우 알파 채널은 모드에 관계 없이 항상 1.0을 반환 해야 합니다.

### <a name="span-idbc6h-partition-setspanspan-idbc6h-partition-setspanspan-idbc6h-partition-setspanbc6h-partition-set"></a><span id="BC6H-partition-set"></span><span id="bc6h-partition-set"></span><span id="BC6H-PARTITION-SET"></span>BC6H 파티션 집합

두 지역 타일에 대 한 32 가능한 파티션 집합이 있으며 아래 표에 정의 되어 있습니다. 각 4x4 블록은 단일 셰이프를 나타냅니다.

![bc6h 파티션 집합의 테이블](images/bc6h-partition-sets.png)

이 파티션 집합 테이블에서 굵게 표시 된 항목은 하위 집합 1에 대 한 수정 인덱스의 위치입니다 (1 개 이하의 비트로 지정). 인덱스 0이 항상 하위 집합 0에 있도록 분할이 항상 정렬 되므로 하위 집합 0에 대 한 수정 인덱스는 항상 인덱스 0입니다. 파티션 순서는 왼쪽 위에서 오른쪽 아래로 이동 하 여 왼쪽에서 오른쪽으로 이동한 다음 위쪽에서 아래쪽으로 이동 합니다.

## <a name="span-idbc6h-compressed-endpoint-formatspanspan-idbc6h-compressed-endpoint-formatspanspan-idbc6h-compressed-endpoint-formatspanbc6h-compressed-endpoint-format"></a><span id="BC6H-compressed-endpoint-format"></span><span id="bc6h-compressed-endpoint-format"></span><span id="BC6H-COMPRESSED-ENDPOINT-FORMAT"></span>BC6H 압축 된 끝점 형식


![bc6h 압축 끝점 형식에 대 한 비트 필드](images/bc6h-headers-med.png)

다음 표에서는 끝점 형식의 함수로 압축 된 끝점의 비트 필드를 보여 줍니다. 각 열에는 인코딩과 비트 필드를 지정 하는 각 행이 있습니다. 이 접근 방식은 한 지역 타일에 대 한 2 개 지역 타일 및 65 비트에 82 비트를 사용 합니다. 예를 들어 위의 한 지역 16 4 인코딩에 대 한 처음 5 비트 \[ \] (특히 가장 오른쪽 열)는 bits m 4:0이 고 \[ \] , 다음 10 비트는 비트 rw 9:0 이며, \[ \] bw 10:15을 포함 하는 마지막 6 비트를 사용 \[ \] 하는 경우에 한 합니다.

위의 표에 있는 필드 이름은 다음과 같이 정의 됩니다.

| 필드 | 변수          |
|-------|-------------------|
| 분     | mode              |
| d     | 셰이프 인덱스       |
| rw    | endpt \[ 0 \] . 0입니다. \[\] |
| x    | endpt \[ 0 \] . B \[ 0\] |
| r)    | endpt \[ 1 \] . 0입니다. \[\] |
| rz    | endpt \[ 1 \] . B \[ 0\] |
| gw    | endpt \[ 0 \] . \[1\] |
| x    | endpt \[ 0 \] . B \[ 1\] |
| gy    | endpt \[ 1 \] . \[1\] |
| release.tar.gz    | endpt \[ 1 \] . B \[ 1\] |
| bw    | endpt \[ 0 \] . A \[ 2\] |
| bx    | endpt \[ 0 \] . B \[ 2\] |
| by    | endpt \[ 1 \] . A \[ 2\] |
| bz    | endpt \[ 1 \] . B \[ 2\] |

 

Endpt \[ i는 \] 0 또는 1 인 경우 각각 0 번째 또는 1 개의 끝점 집합을 참조 합니다.
## <a name="span-idsign-extension-for-endpoint-valuesspanspan-idsign-extension-for-endpoint-valuesspanspan-idsign-extension-for-endpoint-valuesspansign-extension-for-endpoint-values"></a><span id="Sign-extension-for-endpoint-values"></span><span id="sign-extension-for-endpoint-values"></span><span id="SIGN-EXTENSION-FOR-ENDPOINT-VALUES"></span>끝점 값에 대 한 서명 확장


2 개 지역 타일의 경우 부호 확장 될 수 있는 4 개의 끝점 값이 있습니다. Endpt \[ 0 \] . 형식이 서명 된 형식인 경우에만가 서명 됩니다. 다른 끝점은 끝점이 변환 되었거나 형식이 부호 있는 형식인 경우에만 서명 됩니다. 아래 코드에서는 두 지역 끝점 값의 부호를 확장 하는 알고리즘을 보여 줍니다.

``` syntax
static void sign_extend_two_region(Pattern &p, IntEndpts endpts[NREGIONS_TWO])
{
    for (int i=0; i<NCHANNELS; ++i)
    {
      if (BC6H::FORMAT == SIGNED_F16)
        endpts[0].A[i] = SIGN_EXTEND(endpts[0].A[i], p.chan[i].prec);
      if (p.transformed || BC6H::FORMAT == SIGNED_F16)
      {
        endpts[0].B[i] = SIGN_EXTEND(endpts[0].B[i], p.chan[i].delta[0]);
        endpts[1].A[i] = SIGN_EXTEND(endpts[1].A[i], p.chan[i].delta[1]);
        endpts[1].B[i] = SIGN_EXTEND(endpts[1].B[i], p.chan[i].delta[2]);
      }
    }
}
```

1 개 지역 타일의 경우 동작은 동일 하며, endpt 1이 제거 된 경우에만 해당 됩니다 \[ \] .

``` syntax
static void sign_extend_one_region(Pattern &p, IntEndpts endpts[NREGIONS_ONE])
{
    for (int i=0; i<NCHANNELS; ++i)
    {
    if (BC6H::FORMAT == SIGNED_F16)
        endpts[0].A[i] = SIGN_EXTEND(endpts[0].A[i], p.chan[i].prec);
    if (p.transformed || BC6H::FORMAT == SIGNED_F16) 
        endpts[0].B[i] = SIGN_EXTEND(endpts[0].B[i], p.chan[i].delta[0]);
    }
}
```

## <a name="span-idtransform-inversion-for-endpoint-valuesspanspan-idtransform-inversion-for-endpoint-valuesspanspan-idtransform-inversion-for-endpoint-valuesspantransform-inversion-for-endpoint-values"></a><span id="Transform-inversion-for-endpoint-values"></span><span id="transform-inversion-for-endpoint-values"></span><span id="TRANSFORM-INVERSION-FOR-ENDPOINT-VALUES"></span>끝점 값의 변환 반전


2 개 지역 타일의 경우 변환에서는 차이 인코딩의 역함수를 적용 하 여 endpt 0에 기준 값을 추가 \[ 합니다 \] . 총 9 개의 추가 작업에 대해 3 개의 다른 항목에 대 한입니다. 아래 이미지에서 기준 값은 "A0"로 표시 되 고 부동 소수점이 가장 높은 전체 자릿수가 있습니다. "A1", "B0" 및 "B1"은 앵커 값에서 계산 되는 모든 델타 이며 이러한 델타 값은 더 낮은 정밀도로 표시 됩니다. (A0은 endpt 0에 해당 \[ \] 합니다. A, B0은 endpt 0에 해당 \[ \] 합니다. B, A1은 endpt 1에 해당 \[ \] 합니다. A 및 B1은 endpt \[ 1 \] . B에 해당 합니다.)

![변환 반전 끝점 값의 계산](images/bc6h-transform-inverse.png)

단일 지역 타일의 경우 델타 오프셋이 하나만 있으므로 추가 작업은 3 개만 있습니다.

압축 풀기는 역 변환의 결과가 endpt \[ 0. a의 전체 자릿수를 오버플로 하지 않도록 합니다. \] 오버플로가 발생 하는 경우 역 변환에서 생성 되는 값은 동일한 비트 수 내에 래핑해야 합니다. A0의 전체 자릿수가 "p" 비트 이면 변환 알고리즘은 다음과 같습니다.

`B0 = (B0 + A0) & ((1 << p) - 1)`

서명 된 형식의 경우 델타 계산 결과도 부호 확장 이어야 합니다. 부호 확장 작업에서 두 기호를 모두 확장 하는 것으로 간주 하는 경우 (0은 양수이 고 1은 음수) 부호 확장 0은 위의 클램프를 처리 합니다. 위의 클램프 후에는 값 1 (음수)만 부호 확장 해야 합니다.

## <a name="span-idunquantization-of-color-endpointsspanspan-idunquantization-of-color-endpointsspanspan-idunquantization-of-color-endpointsspanunquantization-of-color-endpoints"></a><span id="Unquantization-of-color-endpoints"></span><span id="unquantization-of-color-endpoints"></span><span id="UNQUANTIZATION-OF-COLOR-ENDPOINTS"></span>색 끝점의 Unquantization


압축 되지 않은 끝점이 지정 된 경우 다음 단계는 색 끝점의 초기 unquantization를 수행 하는 것입니다. 이는 세 가지 단계를 포함합니다.

-   색상표의 unquantization
-   색상표 보간
-   Unquantization 종료

Unquantization 프로세스를 두 부분으로 분리 (보간 후에는 색상표 unquantization, 보간 후 최종 unquantization)는 색상표 보간 전 전체 unquantization 프로세스와 비교할 때 필요한 곱하기 작업의 수를 줄입니다.

아래 코드에서는 원래 16 비트 색 값의 추정치를 검색 한 다음 제공 된 가중치 값을 사용 하 여 6 개의 추가 색 값을 색상표에 추가 하는 프로세스를 보여 줍니다. 각 채널에 대해 동일한 작업이 수행 됩니다.

``` syntax
int aWeight3[] = {0, 9, 18, 27, 37, 46, 55, 64};
int aWeight4[] = {0, 4, 9, 13, 17, 21, 26, 30, 34, 38, 43, 47, 51, 55, 60, 64};

// c1, c2: endpoints of a component
void generate_palette_unquantized(UINT8 uNumIndices, int c1, int c2, int prec, UINT16 palette[NINDICES])
{
    int* aWeights;
    if(uNumIndices == 8)
        aWeights = aWeight3;
    else  // uNumIndices == 16
        aWeights = aWeight4;

    int a = unquantize(c1, prec); 
    int b = unquantize(c2, prec);

    // interpolate
    for(int i = 0; i < uNumIndices; ++i)
        palette[i] = finish_unquantize((a * (64 - aWeights[i]) + b * aWeights[i] + 32) >> 6);
}
```

다음 코드 샘플에서는 다음과 같은 관찰을 통해 보간 프로세스를 보여 줍니다.

-   **Unquantize** 함수 (아래)의 전체 색 값 범위는-32768에서 65535 사이 이므로 보간 기는 17 비트 부호 있는 산술 연산을 사용 하 여 구현 됩니다.
-   보간 후에는 값이 최종 크기 조정에 적용 되는 **finish \_ unquantize** 함수 (이 단원의 세 번째 샘플에서 설명)로 전달 됩니다.
-   이러한 함수를 사용 하 여 비트 정밀 결과를 반환 하려면 모든 하드웨어 decompressors 필요 합니다.

``` syntax
int unquantize(int comp, int uBitsPerComp)
{
    int unq, s = 0;
    switch(BC6H::FORMAT)
    {
    case UNSIGNED_F16:
        if(uBitsPerComp >= 15)
            unq = comp;
        else if(comp == 0)
            unq = 0;
        else if(comp == ((1 << uBitsPerComp) - 1))
            unq = 0xFFFF;
        else
            unq = ((comp << 16) + 0x8000) >> uBitsPerComp;
        break;

    case SIGNED_F16:
        if(uBitsPerComp >= 16)
            unq = comp;
        else
        {
            if(comp < 0)
            {
                s = 1;
                comp = -comp;
            }

            if(comp == 0)
                unq = 0;
            else if(comp >= ((1 << (uBitsPerComp - 1)) - 1))
                unq = 0x7FFF;
            else
                unq = ((comp << 15) + 0x4000) >> (uBitsPerComp-1);

            if(s)
                unq = -unq;
        }
        break;
    }
    return unq;
}
```

**마침 \_ unquantize** 는 색상표 보간 후에 호출 됩니다. **Unquantize** 함수는 부호 있는 31/32에 대해의 비율 31/64을 연기 합니다. 이 동작은 필요한 multiplications 수를 줄이기 위해 색상표 보간을 완료 한 후 최종 값을 유효한 절반 범위 (-0x7BFF ~ 0x7BFF)로 가져오는 데 필요 합니다. **finish \_ unquantize** 는 최종 크기 조정을 적용 하 고 **절반**으로 다시 해석 하는 **부호 없는 short** 값을 반환 합니다.

``` syntax
unsigned short finish_unquantize(int comp)
{
    if(BC6H::FORMAT == UNSIGNED_F16)
    {
        comp = (comp * 31) >> 6;                                         // scale the magnitude by 31/64
        return (unsigned short) comp;
    }
    else // (BC6H::FORMAT == SIGNED_F16)
    {
        comp = (comp < 0) ? -(((-comp) * 31) >> 5) : (comp * 31) >> 5;   // scale the magnitude by 31/32
        int s = 0;
        if(comp < 0)
        {
            s = 0x8000;
            comp = -comp;
        }
        return (unsigned short) (s | comp);
    }
}
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[텍스처 블록 압축](texture-block-compression.md)

 

 