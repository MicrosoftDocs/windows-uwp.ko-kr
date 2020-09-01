---
title: BC7 형식
description: BC7 형식은 RGB 및 RGBA 데이터의 고화질 압축에 사용 되는 질감 압축 형식입니다.
ms.assetid: 788B6E8C-9A1F-45F9-BE49-742285E8D8A6
keywords:
- BC7 형식
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4ca22bc897acf0d4cbd924002de96d13f2ba8549
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159027"
---
# <a name="bc7-format"></a>BC7 형식


BC7 형식은 RGB 및 RGBA 데이터의 고화질 압축에 사용 되는 질감 압축 형식입니다.

BC7 형식의 블록 모드에 대 한 자세한 내용은 [BC7 형식 모드 참조](/windows/desktop/direct3d11/bc7-format-mode-reference)를 참조 하세요.

## <a name="span-idabout-bc7-dxgi-format-bc7spanspan-idabout-bc7-dxgi-format-bc7spanspan-idabout-bc7-dxgi-format-bc7spanabout-bc7dxgi_format_bc7"></a><span id="About-BC7-DXGI-FORMAT-BC7"></span><span id="about-bc7-dxgi-format-bc7"></span><span id="ABOUT-BC7-DXGI-FORMAT-BC7"></span>BC7/DXGI \_ 형식 정보 \_ BC7


BC7는 다음 DXGI \_ 형식 열거 값으로 지정 됩니다.

-   **DXGI \_ 형식 \_ 지정 \_ BC7**.
-   **DXGI \_ FORMAT \_ BC7 \_ unorm**.
-   **DXGI \_ \_BC7 \_ unorm \_ SRGB 형식**입니다.

BC7 형식은 [Texture2D](/windows/desktop/direct3d10/d3d10-graphics-reference-resource-structures) (배열 포함), Texture3D 또는 TextureCube (배열 포함) 질감 리소스에 사용할 수 있습니다. 마찬가지로이 형식은 이러한 리소스와 연결 된 모든 밉 맵 표면에 적용 됩니다.

BC7는 16 바이트 (128 비트)의 고정 된 블록 크기와 4x4 텍셀 고정 타일 크기를 사용 합니다. 이전 BC 형식과 마찬가지로, 지원 되는 타일 크기 (4x4) 보다 큰 질감 이미지는 여러 블록을 사용 하 여 압축 됩니다. 이 주소 지정 id는 3 차원 이미지 및 밉 맵, cubemaps 및 질감 배열에도 적용 됩니다. 모든 이미지 타일은 같은 형식 이어야 합니다.

BC7은 3 개의 채널 (RGB) 및 4-채널 (RGBA) 고정 소수점 데이터 이미지를 모두 압축 합니다. 일반적으로 형식은 색 구성 요소별 8 비트입니다. 단, 형식은 색 구성 요소별 상위 비트를 사용 하 여 원본 데이터를 인코딩할 수 있습니다. 모든 이미지 타일은 같은 형식 이어야 합니다.

BC7 디코더는 질감 필터링이 적용 되기 전에 압축을 수행 합니다.

BC7 압축 풀기 하드웨어는 약간 정확 해야 합니다. 즉, 하드웨어는이 문서에서 설명 하는 디코더가 반환 하는 결과와 동일한 결과를 반환 해야 합니다.

## <a name="span-idbc7-implementationspanspan-idbc7-implementationspanspan-idbc7-implementationspanbc7-implementation"></a><span id="BC7-Implementation"></span><span id="bc7-implementation"></span><span id="BC7-IMPLEMENTATION"></span>BC7 구현


BC7 구현은 16 바이트 (128 비트) 블록의 최하위 비트에서 지정 된 모드를 사용 하 여 8 개 모드 중 하나를 지정할 수 있습니다. 이 모드는 0 개 이상의 비트 (값 0, 1 순)로 인코딩됩니다.

BC7 블록에는 여러 개의 끝점 쌍이 포함 될 수 있습니다. 끝점 쌍에 해당 하는 인덱스 집합을 "하위 집합"으로 참조할 수 있습니다. 또한 일부 블록 모드에서 끝점 표현은 "RBGP"로 참조 될 수 있는 형식으로 인코딩됩니다. 여기서 "P" 비트는 끝점의 색 구성 요소에 대 한 공유 최하위 비트를 나타냅니다. 예를 들어 형식에 대 한 끝점 표현이 "RGB 5.5.5.1" 이면 끝점이 RGB 6.6.6 값으로 해석 됩니다. 여기서 P 비트의 상태는 각 구성 요소의 최하위 비트를 정의 합니다. 마찬가지로, 알파 채널이 있는 원본 데이터의 경우 형식에 대 한 표현이 "RGBAP 5.5.5.5.1" 이면 끝점은 RGBA 6.6.6.6로 해석 됩니다. 차단 모드에 따라 하위 집합의 두 끝점 모두에 대해 공유 최하위 비트를 개별적으로 지정 하거나 (하위 집합 당 2 P 비트) 하위 집합의 끝점 간에 공유 (하위 집합 당 1 P 비트) 할 수 있습니다.

알파 구성 요소를 명시적으로 인코딩하지 않는 BC7 블록의 경우 BC7 블록은 모드 비트, 파티션 비트, 압축 된 끝점, 압축 된 인덱스 및 선택적 P 비트로 구성 됩니다. 이러한 블록에서 끝점에는 RGB 전용 표현이 있으며 알파 구성 요소는 원본 데이터의 모든 텍셀에 대해 1.0로 디코딩됩니다.

색 및 알파 구성 요소가 결합 된 BC7 블록의 경우 블록은 모드 비트, 압축 된 끝점, 압축 된 인덱스 및 선택적 파티션 비트와 P 비트로 구성 됩니다. 이러한 블록에서 끝점 색은 RGBA 형식으로 표현 되 고 알파 구성 요소 값은 색 구성 요소 값과 함께 보간됩니다.

별도의 색 및 알파 구성 요소가 있는 BC7 블록의 경우 블록은 모드 비트, 회전 비트, 압축 된 끝점, 압축 된 인덱스 및 선택적 인덱스 선택기 비트로 구성 됩니다. 이러한 블록에는 효과적인 RGB 벡터 \[ R, G, B \] 및 스칼라 알파 채널 ( \[ 개별적으로 인코드)이 있습니다 \] .

다음 표에서는 각 블록 형식의 구성 요소를 나열 합니다.

| BC7 블록에 포함 된 내용 ...     | 모드 비트 | 회전 비트 | 인덱스 선택기 비트 | 파티션 비트 | 압축 된 끝점 | P 비트    | 압축 된 인덱스 |
|---------------------------|-----------|---------------|--------------------|----------------|----------------------|----------|--------------------|
| 색 구성 요소만     | 필수  | 해당 없음           | 해당 없음                | 필수       | 필수             | 선택 사항 | 필수           |
| 색 + 알파 조합    | 필수  | 해당 없음           | 해당 없음                | 선택 사항       | 필수             | 선택 사항 | 필수           |
| 색 및 알파 구분 | 필수  | 필수      | 선택 사항           | 해당 없음            | 필수             | 해당 없음      | 필수           |

 

BC7는 두 끝점 사이의 대략적인 선에 색의 색상표를 정의 합니다. Mode 값은 블록 당 보간 끝점 쌍의 수를 결정 합니다. BC7는 텍셀 당 하나의 색상표 인덱스를 저장 합니다.

인코더는 끝점 쌍에 해당 하는 인덱스의 각 하위 집합에 대해 해당 하위 집합에 대 한 압축 된 인덱스 데이터 중 한 비트의 상태를 수정 합니다. 이렇게 하려면 지정 된 "수정" 인덱스의 인덱스에서 가장 중요 한 비트를 0으로 설정 하 고,이를 삭제 하 여 하위 집합 당 1 비트를 절약할 수 있는 끝점 순서를 선택 합니다. 단일 하위 집합만 있는 블록 모드의 경우 수정 인덱스는 항상 인덱스 0입니다.

## <a name="span-iddecoding-the-bc7-formatspanspan-iddecoding-the-bc7-formatspanspan-iddecoding-the-bc7-formatspandecoding-the-bc7-format"></a><span id="Decoding-the-BC7-Format"></span><span id="decoding-the-bc7-format"></span><span id="DECODING-THE-BC7-FORMAT"></span>BC7 형식 디코딩


다음 의사 코드에서는 16 바이트 BC7 블록을 지정 하 여 (x, y)에서 픽셀의 압축을 푸는 단계를 간략하게 설명 합니다.

``` syntax
decompress_bc7(x, y, block)
{
    mode = extract_mode(block);
    
    //decode partition data from explicit partition bits
    subset_index = 0;
    num_subsets = 1;
    
    if (mode.type == 0 OR == 1 OR == 2 OR == 3 OR == 7)
    {
        num_subsets = get_num_subsets(mode.type);
        partition_set_id = extract_partition_set_id(mode, block);
        subset_index = get_partition_index(num_subsets, partition_set_id, x, y);
    }
    
    //extract raw, compressed endpoint bits
    UINT8 endpoint_array[num_subsets][4] = extract_endpoints(mode, block);
    
    //decode endpoint color and alpha for each subset
    fully_decode_endpoints(endpoint_array, mode, block);
    
    //endpoints are now complete.
    UINT8 endpoint_start[4] = endpoint_array[2 * subset_index];
    UINT8 endpoint_end[4]   = endpoint_array[2 * subset_index + 1];
        
    //Determine the palette index for this pixel
    alpha_index     = get_alpha_index(block, mode, x, y);
    alpha_bitcount  = get_alpha_bitcount(block, mode);
    color_index     = get_color_index(block, mode, x, y);
    color_bitcount  = get_color_bitcount(block, mode);

    //determine output
    UINT8 output[4];
    output.rgb = interpolate(endpoint_start.rgb, endpoint_end.rgb, color_index, color_bitcount);
    output.a   = interpolate(endpoint_start.a,   endpoint_end.a,   alpha_index, alpha_bitcount);
    
    if (mode.type == 4 OR == 5)
    {
        //Decode the 2 color rotation bits as follows:
        // 00 – Block format is Scalar(A) Vector(RGB) - no swapping
        // 01 – Block format is Scalar(R) Vector(AGB) - swap A and R
        // 10 – Block format is Scalar(G) Vector(RAB) - swap A and G
        // 11 - Block format is Scalar(B) Vector(RGA) - swap A and B
        rotation = extract_rot_bits(mode, block);
        output = swap_channels(output, rotation);
    }
    
}
```

이 코드는 16 바이트 BC7 블록에 지정 된 각 하위 집합에 대해 끝점 색 및 알파 구성 요소를 완전히 디코딩하는 단계를 간략하게 설명 합니다.

``` syntax
fully_decode_endpoints(endpoint_array, mode, block)
{
    //first handle modes that have P-bits
    if (mode.type == 0 OR == 1 OR == 3 OR == 6 OR == 7)
    {
        for each endpoint i
        {
            //component-wise left-shift
            endpoint_array[i].rgba = endpoint_array[i].rgba << 1;
        }
        
        //if P-bit is shared
        if (mode.type == 1) 
        {
            pbit_zero = extract_pbit_zero(mode, block);
            pbit_one = extract_pbit_one(mode, block);
            
            //rgb component-wise insert pbits
            endpoint_array[0].rgb |= pbit_zero;
            endpoint_array[1].rgb |= pbit_zero;
            endpoint_array[2].rgb |= pbit_one;
            endpoint_array[3].rgb |= pbit_one;  
        }
        else //unique P-bit per endpoint
        {  
            pbit_array = extract_pbit_array(mode, block);
            for each endpoint i
            {
                endpoint_array[i].rgba |= pbit_array[i];
            }
        }
    }

    for each endpoint i
    {
        // Color_component_precision & alpha_component_precision includes pbit
        // left shift endpoint components so that their MSB lies in bit 7
        endpoint_array[i].rgb = endpoint_array[i].rgb << (8 - color_component_precision(mode));
        endpoint_array[i].a = endpoint_array[i].a << (8 - alpha_component_precision(mode));

        // Replicate each component's MSB into the LSBs revealed by the left-shift operation above
        endpoint_array[i].rgb = endpoint_array[i].rgb | (endpoint_array[i].rgb >> color_component_precision(mode));
        endpoint_array[i].a = endpoint_array[i].a | (endpoint_array[i].a >> alpha_component_precision(mode));
    }
        
    //If this mode does not explicitly define the alpha component
    //set alpha equal to 1.0
    if (mode.type == 0 OR == 1 OR == 2 OR == 3)
    {
        for each endpoint i
        {
            endpoint_array[i].a = 255; //i.e. alpha = 1.0f
        }
    }
}
```

각 하위 집합에 대해 보간된 각 구성 요소를 생성 하려면 다음 알고리즘을 사용 합니다. "c"는 생성할 구성 요소입니다. "e0"가 하위 집합의 끝점 0의 구성 요소인 것을 허용 합니다. 그리고 "e1"은 하위 집합의 끝점 1의 구성 요소가 됩니다.

``` syntax
UINT16 aWeight2[] = {0, 21, 43, 64};
UINT16 aWeight3[] = {0, 9, 18, 27, 37, 46, 55, 64};
UINT16 aWeight4[] = {0, 4, 9, 13, 17, 21, 26, 30, 34, 38, 43, 47, 51, 55, 60, 64};

UINT8 interpolate(UINT8 e0, UINT8 e1, UINT8 index, UINT8 indexprecision)
{
    if(indexprecision == 2)
        return (UINT8) (((64 - aWeights2[index])*UINT16(e0) + aWeights2[index]*UINT16(e1) + 32) >> 6);
    else if(indexprecision == 3)
        return (UINT8) (((64 - aWeights3[index])*UINT16(e0) + aWeights3[index]*UINT16(e1) + 32) >> 6);
    else // indexprecision == 4
        return (UINT8) (((64 - aWeights4[index])*UINT16(e0) + aWeights4[index]*UINT16(e1) + 32) >> 6);
}
```

다음 의사 코드에서는 색 및 알파 구성 요소에 대 한 인덱스 및 비트 수를 추출 하는 방법을 보여 줍니다. 별도의 색과 알파를 사용 하는 블록에는 벡터 채널에 대 한 인덱스 데이터 집합과 스칼라 채널에 대 한 두 개의 인덱스 데이터 집합이 있습니다. 모드 4의 경우 이러한 인덱스는 서로 다른 너비 (2 또는 3 비트) 이며 벡터 또는 스칼라 데이터가 3 비트 인덱스를 사용 하는지 여부를 지정 하는 1 비트 선택 기가 있습니다. 알파 비트 수 추출은 색 비트 수를 추출 하는 것과 유사 하지만 **Idxmode** 비트를 기반으로 하는 역 동작을 포함 합니다.

``` syntax
bitcount get_color_bitcount(block, mode)
{
    if (mode.type == 0 OR == 1)
        return 3;
    
    if (mode.type == 2 OR == 3 OR == 5 OR == 7)
        return 2;
    
    if (mode.type == 6)
        return 4;
        
    //The only remaining case is Mode 4 with 1-bit index selector
    idxMode = extract_idxMode(block);
    if (idxMode == 0)
        return 2;
    else
        return 3;
}
```

## <a name="span-idbc7-format-mode-referencespanspan-idbc7-format-mode-referencespanspan-idbc7-format-mode-referencespanbc7-format-mode-reference"></a><span id="BC7-format-mode-reference"></span><span id="bc7-format-mode-reference"></span><span id="BC7-FORMAT-MODE-REFERENCE"></span>BC7 format 모드 참조


이 섹션에는 BC7 텍스처 압축 형식 블록에 대 한 8 개의 블록 모드 및 비트 할당 목록이 포함 되어 있습니다.

블록 내의 각 하위 집합에 대 한 색은 두 개의 명시적 끝점 색과 두 개의 보간된 색 집합으로 표시 됩니다. 블록의 인덱스 전체 자릿수에 따라 각 하위 집합에 4, 8 또는 16 색을 지정할 수 있습니다.

### <a name="span-idmode-0spanspan-idmode-0spanspan-idmode-0spanmode-0"></a><span id="Mode-0"></span><span id="mode-0"></span><span id="MODE-0"></span>모드 0

BC7 모드 0에는 다음과 같은 특징이 있습니다.

-   색 구성 요소만 (알파 없음)
-   블록 당 3 개 하위 집합
-   RGBP 4.4.4.1 끝점 마다 고유한 P 비트를 사용 합니다.
-   3 비트 인덱스
-   16 개 파티션

![모드 0 비트 레이아웃](images/bc7-mode0.png)

### <a name="span-idmode-1spanspan-idmode-1spanspan-idmode-1spanmode-1"></a><span id="Mode-1"></span><span id="mode-1"></span><span id="MODE-1"></span>모드 1

BC7 모드 1에는 다음과 같은 특징이 있습니다.

-   색 구성 요소만 (알파 없음)
-   블록 당 2 개의 하위 집합
-   RGBP 6.6.6.1 endpoints (하위 집합 당 공유 P 비트 포함)
-   3 비트 인덱스
-   64 파티션

![모드 1 비트 레이아웃](images/bc7-mode1.png)

### <a name="span-idmode-2spanspan-idmode-2spanspan-idmode-2spanmode-2"></a><span id="Mode-2"></span><span id="mode-2"></span><span id="MODE-2"></span>모드 2

BC7 모드 2에는 다음과 같은 특징이 있습니다.

-   색 구성 요소만 (알파 없음)
-   블록 당 3 개 하위 집합
-   RGB 5.5.5 끝점
-   2 비트 인덱스
-   64 파티션

![모드 2 비트 레이아웃](images/bc7-mode2.png)

### <a name="span-idmode-3spanspan-idmode-3spanspan-idmode-3spanmode-3"></a><span id="Mode-3"></span><span id="mode-3"></span><span id="MODE-3"></span>모드 3

BC7 모드 3에는 다음과 같은 특징이 있습니다.

-   색 구성 요소만 (알파 없음)
-   블록 당 2 개의 하위 집합
-   RGBP 7.7.7.1 endpoints (하위 집합 당 고유한 P 비트 포함)
-   2 비트 인덱스
-   64 파티션

![모드 3 비트 레이아웃](images/bc7-mode3.png)

### <a name="span-idmode-4spanspan-idmode-4spanspan-idmode-4spanmode-4"></a><span id="Mode-4"></span><span id="mode-4"></span><span id="MODE-4"></span>모드 4

BC7 모드 4에는 다음과 같은 특징이 있습니다.

-   별도의 알파 구성 요소가 있는 색 구성 요소
-   블록 당 1 개의 하위 집합
-   RGB 5.5.5 색 끝점
-   6 비트 알파 끝점
-   16 x 2 비트 인덱스
-   16 x 3 비트 인덱스
-   2 비트 구성 요소 회전
-   1 비트 인덱스 선택기 (2 비트 또는 3 비트 인덱스 사용 여부)

![모드 4 비트 레이아웃](images/bc7-mode4.png)

### <a name="span-idmode-5spanspan-idmode-5spanspan-idmode-5spanmode-5"></a><span id="Mode-5"></span><span id="mode-5"></span><span id="MODE-5"></span>모드 5

BC7 모드 5에는 다음과 같은 특징이 있습니다.

-   별도의 알파 구성 요소가 있는 색 구성 요소
-   블록 당 1 개의 하위 집합
-   RGB 7.7.7 색 끝점
-   6 비트 알파 끝점
-   16 x 2 비트 색 인덱스
-   16 x 2 비트 알파 인덱스
-   2 비트 구성 요소 회전

![모드 5 비트 레이아웃](images/bc7-mode5.png)

### <a name="span-idmode-6spanspan-idmode-6spanspan-idmode-6spanmode-6"></a><span id="Mode-6"></span><span id="mode-6"></span><span id="MODE-6"></span>모드 6

BC7 모드 6에는 다음과 같은 특징이 있습니다.

-   조합 된 색 및 알파 구성 요소
-   블록 당 하나의 하위 집합
-   RGBAP 7.7.7.7.1 color (및 알파) 끝점 (끝점 당 고유 P 비트)
-   16 x 4 비트 인덱스

![모드 6 비트 레이아웃](images/bc7-mode6.png)

### <a name="span-idmode-7spanspan-idmode-7spanspan-idmode-7spanmode-7"></a><span id="Mode-7"></span><span id="mode-7"></span><span id="MODE-7"></span>모드 7

BC7 모드 7에는 다음과 같은 특징이 있습니다.

-   조합 된 색 및 알파 구성 요소
-   블록 당 2 개의 하위 집합
-   RGBAP 5.5.5.5.1 color (및 알파) 끝점 (끝점 당 고유 P 비트)
-   2 비트 인덱스
-   64 파티션

![모드 7 비트 레이아웃](images/bc7-mode7.png)

### <a name="span-idremarksspanspan-idremarksspanspan-idremarksspanremarks"></a><span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>설명

모드 8 (가장 중요 하지 않은 바이트는 0x00으로 설정 됨)은 예약 되어 있습니다. 인코더에서 사용 하지 마세요. 이 모드를 하드웨어에 전달 하는 경우 0으로 초기화 된 블록이 모두 반환 됩니다.

BC7에서는 다음 방법 중 하나로 알파 구성 요소를 인코딩할 수 있습니다.

-   명시적 알파 구성 요소 인코딩이 없는 블록 형식입니다. 이러한 블록에서 색 끝점에는 RGB 전용 인코딩이 있습니다. 알파 구성 요소는 모든 텍셀에 대해 1.0로 디코딩됩니다.
-   색 및 알파 구성 요소가 결합 된 블록 형식 이러한 블록에서는 끝점 색 값이 RGBA 형식으로 지정 되 고 알파 구성 요소 값은 색 값과 함께 보간됩니다.
-   구분 된 색 및 알파 구성 요소가 있는 블록 형식입니다. 이러한 블록에서 색 및 알파 값은 개별적으로 지정 되며 각각 고유한 인덱스 집합이 사용 됩니다. 결과적으로, 벡터는 효과적인 벡터와 스칼라 채널을 개별적으로 인코드 합니다. 여기서 벡터는 일반적으로 색 채널 \[ R, G, B를 지정 \] 하 고 스칼라는 알파 채널 a를 지정 합니다 \[ \] . 이 방법을 지원 하기 위해 별도의 2 비트 필드가 인코딩에 제공 되므로 별도의 채널 인코딩을 스칼라 값으로 지정할 수 있습니다. 따라서 블록은이 알파 인코딩의 4 가지 다른 표현 (2 비트 필드로 표시 됨) 중 하나를 가질 수 있습니다.
    -   RGB | A: 알파 채널 분리
    -   AGB | R: "red" 색 채널 분리
    -   RAB | G: "녹색" 색 채널 분리
    -   RGA | B: "blue" 색 채널 분리

    디코더는 디코드 후 채널 순서를 RGBA로 다시 정렬 하므로 개발자에 게 내부 블록 형식이 표시 되지 않습니다. 별도의 색 및 알파 구성 요소를 포함 하는 검정에는 벡터화 된 채널 집합 및 스칼라 채널에 대 한 두 개의 인덱스 데이터 집합이 있습니다. (모드 4의 경우 이러한 인덱스는 서로 다른 너비 \[ 2 또는 3 비트입니다 \] . 모드 4에는 벡터 또는 스칼라 채널이 3 비트 인덱스를 사용 하는지 여부를 지정 하는 1 비트 선택기도 포함 되어 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[텍스처 블록 압축](texture-block-compression.md)

 

 