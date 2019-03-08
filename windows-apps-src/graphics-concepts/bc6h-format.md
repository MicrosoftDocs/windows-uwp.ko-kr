---
title: BC6H 형식
description: BC6H 형식은 원본 데이터의 HDR(High Dynamic Range) 색 공간을 지원하도록 설계된 텍스처 압축 형식입니다.
ms.assetid: 6781D967-9262-4EE7-B354-7A6D0EA0498E
keywords:
- BC6H 형식
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f147f4c30d2a662806df5928fc79178522b9b6a6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633098"
---
# <a name="bc6h-format"></a>BC6H 형식


BC6H 형식은 원본 데이터의 HDR(High Dynamic Range) 색 공간을 지원하도록 설계된 텍스처 압축 형식입니다.

## <a name="span-idabout-bc6h-dxgi-format-bc6hspanspan-idabout-bc6h-dxgi-format-bc6hspanspan-idabout-bc6h-dxgi-format-bc6hspanabout-bc6hdxgiformatbc6h"></a><span id="About-BC6H-DXGI-FORMAT-BC6H"></span><span id="about-bc6h-dxgi-format-bc6h"></span><span id="ABOUT-BC6H-DXGI-FORMAT-BC6H"></span>About BC6H/DXGI\_FORMAT\_BC6H


BC6H 형식은 값의 각 색 채널에 대한 16비트 값(16:16:16)과 함께 3개의 HDR 색 채널을 사용하는 이미지에 대한 고품질 압축을 제공합니다. 알파 채널은 지원되지 않습니다.

다음 DXGI BC6H 된\_열거형 값의 서식 지정 합니다.

-   **DXGI\_FORMAT\_BC6H\_TYPELESS**.
-   **DXGI\_FORMAT\_BC6H\_UF16**. 이 BC6H 형식은 16비트 부동 소수점 색 채널 값에 부호 비트를 사용하지 않습니다.
-   **DXGI\_FORMAT\_BC6H\_SF16**. 이 BC6H 형식은 16비트 부동 소수점 색 채널 값에 부호 비트를 사용합니다.

**참고**    "중간" 부동 소수점 형식으로 16 비트 부동 소수점 형식 색 채널에 대 한 하 라고 합니다. 이 형식의 비트 레이아웃은 다음과 같습니다.
|                       |                                                 |
|-----------------------|-------------------------------------------------|
| UF16(부호 없는 부동 소수점) | 5개의 지수 비트 + 11개의 가수 비트              |
| SF16(부호 있는 부동 소수점)   | 1개의 부호 비트 + 5개의 지수 비트 + 10개의 가수 비트 |

 

 

[Texture2D](https://msdn.microsoft.com/library/windows/desktop/bb205277)(배열 포함), Texture3D 또는 TextureCube(배열 포함) 텍스처 리소스에 BC6H 형식을 사용할 수 있습니다. 마찬가지로 이 형식은 이러한 리소스와 연결된 모든 MIP 맵 표면에 적용됩니다.

BC6H는 16바이트(128비트)의 고정 블록 크기와 4x4 텍셀의 고정 타일 크기를 사용합니다. 이전 BC 형식과 마찬가지로 지원되는 타일 크기(4x4)보다 큰 텍스처 이미지는 여러 블록을 사용하여 압축됩니다. 이 주소 지정 ID는 3차원 이미지, MIP 맵, 큐브 맵 및 텍스처 배열에도 적용됩니다. 모든 이미지 타일이 동일한 형식이어야 합니다.

BC6H 형식 관련 주의 사항:

-   BC6H는 부동 소수점 비정규화를 지원하지만 INF(무한대)와 NaN(숫자가 아님)은 지원하지 않습니다. 예외는 BC6H의 서명 된 모드 (DXGI\_형식\_BC6H\_SF16),-INF (음의 무한대)를 지 합니다. -INF에 대한 이러한 지원은 형식 자체의 아티팩트일 뿐이며 이 형식에 대한 인코더에서는 이를 지원하지 않습니다. 일반적으로 인코더가 INF(양수 또는 음수)나 NaN 입력 데이터를 발견하면 해당 데이터를 허용되는 최대 비INF 표현 값으로 변환하고 압축 전에 NaN을 0에 매핑해야 합니다.
-   BC6H는 알파 채널을 지원하지 않습니다.
-   BC6H 디코더는 텍스처 필터링을 수행하기 전에 압축 풀기를 수행합니다.
-   BC6H 압축 풀기는 비트 단위로 정확해야 합니다. 즉, 하드웨어가 이 문서에 설명된 디코더와 동일한 결과를 반환해야 합니다.

## <a name="span-idbc6h-implementationspanspan-idbc6h-implementationspanspan-idbc6h-implementationspanbc6h-implementation"></a><span id="BC6H-implementation"></span><span id="bc6h-implementation"></span><span id="BC6H-IMPLEMENTATION"></span>BC6H 구현


BC6H 블록은 모드 비트, 압축된 끝점, 압축된 인덱스 및 옵션 파티션 인덱스로 구성됩니다. 이 형식의 14개의 모드를 지정합니다.

끝점 색은 RGB 3색으로 저장됩니다. BC6H는 다수의 정의된 색 끝점을 가로지르는 근사 선에 색상표를 정의합니다. 또한 모드에 따라 타일을 두 영역으로 나누거나 한 영역으로 처리할 수 있습니다. 한 영역으로 처리할 경우 두 영역 타일에 각 영역에 대한 별도의 색 끝점 집합이 있습니다. BC6H는 텍셀당 하나의 색상표 인덱스를 저장합니다.

두 영역의 경우 32개의 가능한 파티션이 있습니다.

## <a name="span-iddecoding-the-bc6h-formatspanspan-iddecoding-the-bc6h-formatspanspan-iddecoding-the-bc6h-formatspandecoding-the-bc6h-format"></a><span id="Decoding-the-BC6H-format"></span><span id="decoding-the-bc6h-format"></span><span id="DECODING-THE-BC6H-FORMAT"></span>BC6H 형식 디코딩


아래 의사 코드는 16바이트 BC6H 블록이 지정된 (x,y)의 픽셀 압축을 푸는 단계를 보여 줍니다.

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

다음 표에는 BC6H 블록에 대한 14개의 가능한 형식별 비트 개수와 값이 나와 있습니다.

| 모드 | 파티션 인덱스 | Partition | 색 끝점                  | 모드 비트      |
|------|-------------------|-----------|----------------------------------|----------------|
| 1    | 46비트           | 5비트    | 75비트(10.555, 10.555, 10.555) | 2비트(00)    |
| 2    | 46비트           | 5비트    | 75비트(7666, 7666, 7666)       | 2비트(01)    |
| 3    | 46비트           | 5비트    | 72비트(11.555, 11.444, 11.444) | 5비트(00010) |
| 4    | 46비트           | 5비트    | 72비트(11.444, 11.555, 11.444) | 5비트(00110) |
| 5    | 46비트           | 5비트    | 72비트(11.444, 11.444, 11.555) | 5비트(01010) |
| 6    | 46비트           | 5비트    | 72비트(9555, 9555, 9555)       | 5비트(01110) |
| 7    | 46비트           | 5비트    | 72비트(8666, 8555, 8555)       | 5비트(10010) |
| 8    | 46비트           | 5비트    | 72비트(8555, 8666, 8555)       | 5비트(10110) |
| 9    | 46비트           | 5비트    | 72비트(8555, 8555, 8666)       | 5비트(11010) |
| 10   | 46비트           | 5비트    | 72비트(6666, 6666, 6666)       | 5비트(11110) |
| 11   | 63비트           | 0비트    | 60비트(10.10, 10.10, 10.10)    | 5비트(00011) |
| 12   | 63비트           | 0비트    | 60비트(11.9, 11.9, 11.9)       | 5비트(00111) |
| 13   | 63비트           | 0비트    | 60비트(12.8, 12.8, 12.8)       | 5비트(01011) |
| 14   | 63비트           | 0비트    | 60비트(16.4, 16.4, 16.4)       | 5비트(01111) |

 

이 표의 각 형식은 모드 비트로 고유하게 식별 가능합니다. 처음 10개의 모드가 두 영역 타일에 사용되며 모드 비트 필드 길이는 2비트 또는 5비트일 수 있습니다. 이러한 블록에는 압축된 색 끝점(72 또는 75비트), 파티션(5비트) 및 파티션 인덱스(46비트)에 대한 필드도 있습니다.

압축된 색 끝점의 경우 위 표의 값은 저장된 RGB 끝점의 정밀도와 각 색 값에 사용된 비트 수를 나타냅니다. 예를 들어, 모드 3은 색 끝점 정밀도 11과 빨강, 파랑 및 녹색에 대한 변환된 끝점의 델타 값을 저장하는 데 사용되는 비트 수(각각 5, 4 및 4)를 지정합니다. 모드 10은 델타 압축을 사용하지 않고 대신 4개의 색 끝점을 모두 명시적으로 저장합니다.

마지막 4개의 블록 모드는 모드 필드가 5비트인 한 영역 타일에 사용됩니다. 이러한 블록에는 끝점(60비트)과 압축된 인덱스(63비트)에 대한 필드도 있습니다. 모드 11은 모드 10과 같이 델타 압축을 사용하지 않고 대신 2개의 색 끝점을 모두 명시적으로 저장합니다.

모드 10011, 10111, 11011 및 11111(표시되지 않음)은 예약되어 있습니다. 인코더에 이들 모드를 사용하지 마십시오. 이러한 모드 중 하나가 지정된 블록이 하드웨어에 전달될 경우 압축을 푼 결과 블록의 모든 채널(알파 채널 제외)에 모두 0만 포함되어야 합니다.

BC6H의 경우 모드에 관계없이 알파 채널이 항상 1.0을 반환합니다.

### <a name="span-idbc6h-partition-setspanspan-idbc6h-partition-setspanspan-idbc6h-partition-setspanbc6h-partition-set"></a><span id="BC6H-partition-set"></span><span id="bc6h-partition-set"></span><span id="BC6H-PARTITION-SET"></span>BC6H 파티션 집합

두 영역 타일에 대한 32개의 가능한 파티션 집합이 있습니다. 아래 표에 이들 파티션 집합이 정의되어 있습니다. 각 4x4 블록은 단일 셰이프를 나타냅니다.

![bc6h 파티션 집합 표](images/bc6h-partition-sets.png)

이 파티션 집합 표에서 굵게 표시되고 밑줄이 그어진 항목은 하위 집합 1(1비트 적게 지정됨)에 대한 수정 인덱스의 위치입니다. 항상 인덱스 0이 하위 집합 0에 있도록 파티션이 정렬되므로 하위 집합 0에 대한 수정 인덱스는 항상 인덱스 0입니다. 파티션 순서는 왼쪽 위에서 오른쪽 아래입니다. 즉, 왼쪽에서 오른쪽으로 이동한 다음 위쪽에서 아래쪽으로 이동합니다.

## <a name="span-idbc6h-compressed-endpoint-formatspanspan-idbc6h-compressed-endpoint-formatspanspan-idbc6h-compressed-endpoint-formatspanbc6h-compressed-endpoint-format"></a><span id="BC6H-compressed-endpoint-format"></span><span id="bc6h-compressed-endpoint-format"></span><span id="BC6H-COMPRESSED-ENDPOINT-FORMAT"></span>BC6H 압축된 끝점 형식


![bc6h 압축된 끝점 형식에 대한 비트 필드](images/bc6h-headers-med.png)

이 표에는 끝점 형식의 기능으로 압축된 끝점에 대한 비트 필드가 나와 있습니다. 열과 행마다 각각 인코딩과 비트 필드가 지정되어 있습니다. 이 접근 방식은 두 영역 타일의 경우 82비트를 차지하고 한 영역 타일의 경우 65비트를 차지합니다. 예를 들어 한 지역에 대 한 첫 번째 5 비트가 \[16 4\] 비트 m 됩니다 (특히 맨 오른쪽 열) 위의 인코딩\[4:0\]에 다음 10 비트는 비트 rw\[9:0\]등 bw를 포함 하 고 마지막 6 비트\[10시 15분\]합니다.

위 표의 필드 이름은 다음과 같이 정의됩니다.

| 필드 | 변수          |
|-------|-------------------|
| m     | mode              |
| d     | 셰이프 인덱스       |
| rw    | endpt\[0\].A\[0\] |
| rx    | endpt\[0\].B\[0\] |
| ry    | endpt\[1\].A\[0\] |
| rz    | endpt\[1\].B\[0\] |
| gw    | endpt\[0\].A\[1\] |
| gx    | endpt\[0\].B\[1\] |
| gy    | endpt\[1\].A\[1\] |
| gz    | endpt\[1\].B\[1\] |
| bw    | endpt\[0\].A\[2\] |
| bx    | endpt\[0\].B\[2\] |
| 수단    | endpt\[1\].A\[2\] |
| bz    | endpt\[1\].B\[2\] |

 

Endpt\[있습니까\]이며, 여기서 i은 0 또는 1 0 번째 또는 첫 번째 일련의 끝점을 각각 가리킵니다.
## <a name="span-idsign-extension-for-endpoint-valuesspanspan-idsign-extension-for-endpoint-valuesspanspan-idsign-extension-for-endpoint-valuesspansign-extension-for-endpoint-values"></a><span id="Sign-extension-for-endpoint-values"></span><span id="sign-extension-for-endpoint-values"></span><span id="SIGN-EXTENSION-FOR-ENDPOINT-VALUES"></span>끝점 값에 대 한 부호 확장


두 영역 타일의 경우 부호를 확장할 수 있는 4개의 끝점 값이 있습니다. Endpt\[0\]합니다. A 형식은 부호 있는 형식으로 하는 경우에 서명 끝점 변환 된 경우에 또는 형식 서명 된 형식인 경우 다른 끝점 서명 됩니다. 아래 코드는 두 영역 끝점 값의 부호를 확장하는 알고리즘을 보여 줍니다.

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

한 지역 타일에 대 한 동작은 동일만 endpt\[1\] 제거 합니다.

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

## <a name="span-idtransform-inversion-for-endpoint-valuesspanspan-idtransform-inversion-for-endpoint-valuesspanspan-idtransform-inversion-for-endpoint-valuesspantransform-inversion-for-endpoint-values"></a><span id="Transform-inversion-for-endpoint-values"></span><span id="transform-inversion-for-endpoint-values"></span><span id="TRANSFORM-INVERSION-FOR-ENDPOINT-VALUES"></span>끝점 값에 대 한 반전 변환


두 지역 타일에 대 한 변환을 적용 인코딩, 기준 값 endpt에 추가 차이의 역\[0\]합니다. 세 가지 다른 항목에는 총 9에 대 한 작업을 추가 합니다. 아래 이미지에서는 기준 값이 "A0"으로 표현되고 부동 소수점 정밀도가 가장 높습니다. "A1," "B0" 및 "B1"은 모두 앵커 값에서 계산되는 델타이며 이러한 델타 값은 더 낮은 정밀도로 표현됩니다. (Endpt에 따라 A0\[0\]합니다. Endpt에 해당 하는, B0\[0\]합니다. B, A1 해당 endpt\[1\]합니다. A, B1에 해당 endpt\[1\]합니다. 2.)

![역변환 끝점 값의 계산](images/bc6h-transform-inverse.png)

한 영역 타일의 경우 델타 오프셋이 하나만 있으므로 더하기 연산이 3개만 있습니다.

압축 풀기 프로그램 결과를 역 변환할 수 있는 endpt 자릿수 오버플로되지 않습니다는 확인 해야 합니다\[0\].을 합니다. 오버플로되는 경우 역변환의 결과 값이 같은 수의 비트 내에 래핑되어야 합니다. A0의 정밀도가 "p" 비트인 경우 변환 알고리즘은 다음과 같습니다.

`B0 = (B0 + A0) & ((1 << p) - 1)`

부호 있는 형식의 경우 델타 계산의 결과도 부호가 확장되어야 합니다. 부호 확장 작업에서 두 부호 모두 확장을 고려할 경우(0은 양수이고 1은 음수) 0의 부호 확장이 위의 Clamp를 처리합니다. 마찬가지로 위의 Clamp 다음에 1(음수)의 값만 부호가 확장되어야 합니다.

## <a name="span-idunquantization-of-color-endpointsspanspan-idunquantization-of-color-endpointsspanspan-idunquantization-of-color-endpointsspanunquantization-of-color-endpoints"></a><span id="Unquantization-of-color-endpoints"></span><span id="unquantization-of-color-endpoints"></span><span id="UNQUANTIZATION-OF-COLOR-ENDPOINTS"></span>색 끝점 unquantization


압축되지 않은 끝점이 지정된 경우 다음 단계는 색 끝점의 초기 비양자화를 수행하는 것입니다. 이 작업은 다음 세 단계로 이루어져 있습니다.

-   색상표의 비양자화
-   색상표의 보간
-   비양자화 종료

비양자화 프로세스를 두 부분(보간 전 색상표 비양자화와 보간 후 최종 비양자화)으로 나누면 색상표 보간 전 전체 비양자화 프로세스와 비교 시 필요한 곱하기 연산 수가 줄어듭니다.

아래 코드는 원본 16비트 색 값에 대한 예상치를 검색한 다음 제공된 가중치 값을 사용하여 색상표에 6개의 추가 색 값을 더하는 프로세스를 보여 줍니다. 동일한 작업이 각 채널에서 수행됩니다.

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

그 다음 코드 샘플은 다음과 같은 관측과 함께 보간 프로세스를 보여 줍니다.

-   **unquantize** 함수(아래)에 대한 색 값의 전체 범위가 -32768 ~ 65535이므로 17비트의 부호 있는 산술을 사용하여 보간이 구현됩니다.
-   보간을 후 값에 전달 되는 **완료\_unquantize** 마지막 크기 조정 적용 되는 함수 (이 섹션의 세 번째 샘플에서 설명)과 합니다.
-   모든 하드웨어 압축 풀기 프로그램에서 이러한 함수를 사용하여 비트 단위로 정확한 결과를 반환해야 합니다.

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

**마침\_unquantize** 색상표 보간 이후에 호출 됩니다. **unquantize** 함수는 부호가 있는 경우 31/32, 부호가 없는 경우 31/64로 조정을 연기합니다. 이 동작은 색상표 보간 완료 후 필요한 곱하기 수를 줄이기 위해 유효한 절반 범위(-0x7BFF ~ 0x7BFF)로 최종 값을 가져오는 데 필요합니다. **마침\_unquantize** 마지막 크기 조정을 적용 하 고 반환을 **unsigned short** 가져옵니다를 해석 하는 값 **절반**합니다.

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

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련된 항목


[질감 블록 압축](texture-block-compression.md)

 

 




