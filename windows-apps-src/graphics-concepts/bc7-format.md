---
title: BC7 형식
description: BC7 형식은 RGB 및 RGBA 데이터의 고품질 압축에 사용되는 텍스처 압축 형식입니다.
ms.assetid: 788B6E8C-9A1F-45F9-BE49-742285E8D8A6
keywords:
- BC7 형식
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 70380dd0bd07cfe0c81e8339f8606029663b47d4
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6052248"
---
# <a name="bc7-format"></a>BC7 형식


BC7 형식은 RGB 및 RGBA 데이터의 고품질 압축에 사용되는 텍스처 압축 형식입니다.

BC7 형식의 블록 모드에 대한 자세한 내용은 [BC7 Format Mode Reference(BC7 형식 모드 참조)](https://msdn.microsoft.com/library/windows/desktop/hh308954)를 참조하세요.

## <a name="span-idabout-bc7-dxgi-format-bc7spanspan-idabout-bc7-dxgi-format-bc7spanspan-idabout-bc7-dxgi-format-bc7spanabout-bc7dxgiformatbc7"></a><span id="About-BC7-DXGI-FORMAT-BC7"></span><span id="about-bc7-dxgi-format-bc7"></span><span id="ABOUT-BC7-DXGI-FORMAT-BC7"></span>BC7/DXGI\_FORMAT\_BC7 정보


BC7은 다음과 같은 DXGI\_FORMAT 열거 값으로 지정됩니다.

-   **DXGI\_FORMAT\_BC7\_TYPELESS**.
-   **DXGI\_FORMAT\_BC7\_UNORM**.
-   **DXGI\_FORMAT\_BC7\_UNORM\_SRGB**.

[Texture2D](https://msdn.microsoft.com/library/windows/desktop/bb205277)(배열 포함), Texture3D 또는 TextureCube(배열 포함) 텍스처 리소스에 BC7 형식을 사용할 수 있습니다. 마찬가지로 이 형식은 이러한 리소스와 연결된 모든 MIP 맵 표면에 적용됩니다.

BC7은 16바이트(128비트)의 고정 블록 크기와 4x4 텍셀의 고정 타일 크기를 사용합니다. 이전 BC 형식과 마찬가지로 지원되는 타일 크기(4x4)보다 큰 텍스처 이미지는 여러 블록을 사용하여 압축됩니다. 이 주소 ID는 3차원 이미지, MIP 맵, 큐브 맵 및 텍스처 배열에도 적용됩니다. 모든 이미지 타일이 동일한 형식이어야 합니다.

BC7은 3채널(RGB) 및 4채널(RGBA) 고정 소수점 데이터 이미지를 모두 압축합니다. 이 형식은 색 구성 요소당 비트 수가 8개 이상인 원본 데이터를 인코딩할 수 있지만 대개 원본 데이터는 색 구성 요소(채널)당 8비트입니다. 모든 이미지 타일이 동일한 형식이어야 합니다.

BC7 디코더는 텍스처 필터링이 적용되기 전에 압축 풀기를 수행합니다.

BC7 압축 하드웨어는 비트 단위로 정확해야 합니다. 즉, 하드웨어가 이 문서에 설명된 디코더에서 반환하는 결과와 동일한 결과를 반환해야 합니다.

## <a name="span-idbc7-implementationspanspan-idbc7-implementationspanspan-idbc7-implementationspanbc7-implementation"></a><span id="BC7-Implementation"></span><span id="bc7-implementation"></span><span id="BC7-IMPLEMENTATION"></span>BC7 구현


BC7 구현은 8개 모드 중 하나를 지정할 수 있습니다. 16바이트(128비트) 블록의 최하위 비트에 모드가 지정됩니다. 모드는 0과 1 값으로 0개 이상의 비트에 의해 인코딩됩니다.

BC7 블록에 여러 끝점 쌍이 포함될 수 있습니다. 끝점 쌍에 해당하는 인덱스 집합을 "하위 집합"이라고도 합니다. 또한 일부 블록 모드에서 끝점 표현은 "RBGP"라는 형태로 인코딩됩니다. 여기에서 "P" 비트는 끝점의 색 구성 요소에 대한 공유 최하위 비트를 나타냅니다. 예를 들어, 형식에 대한 끝점 표현이 "RGB 5.5.5.1"일 경우 끝점이 RGB 6.6.6 값으로 해석됩니다. 여기에서 P 비트의 상태는 각 구성 요소의 최하위 비트를 정의합니다. 마찬가지로 알파 채널이 있는 원본 데이터의 경우 형식에 대한 표현이 "RGBAP 5.5.5.5.1"이면 끝점이 RGBA 6.6.6.6으로 해석됩니다. 블록 모드에 따라 하위 집합의 두 끝점에 대한 공유 최하위 비트를 따로(하위 집합당 P 비트 2개) 또는 하위 집합의 끝점 간에 공유(하위 집합당 P 비트 1개)하도록 지정할 수 있습니다.

알파 구성 요소를 명시적으로 인코딩하지 않는 BC7 블록은 모드 비트, 파티션 비트, 압축된 끝점, 압축된 인덱스 및 옵션 P 비트로 구성됩니다. 이러한 블록에서는 끝점에 RGB 전용 표현이 있고 알파 구성 요소는 원본 데이터의 모든 텍셀에 대해 1.0으로 디코딩됩니다.

색 구성 요소와 알파 구성 요소를 결합한 BC7 블록은 모드 비트, 압축된 끝점, 압축된 인덱스 및 옵션 파티션 비트와 P 비트로 구성됩니다. 이러한 블록에서는 끝점 색이 RGBA 형식으로 표현되고 알파 구성 요소 값이 색 구성 요소 값과 함께 보간됩니다.

색 구성 요소와 알파 구성 요소가 분리된 BC7 블록은 모드 비트, 회전 비트, 압축된 끝점, 압축된 인덱스 및 옵션 인덱스 선택기 비트로 구성됩니다. 이러한 블록에서는 유효한 RGB 벡터 \[R, G, B\] 및 스칼라 알파 채널 \[A\]가 따로 인코딩됩니다.

다음 표에는 각 블록 유형의 구성 요소가 나열되어 있습니다.

| BC7 블록 내용     | 모드 비트 | 회전 비트 | 인덱스 선택기 비트 | 파티션 비트 | 압축된 끝점 | P 비트    | 압축된 인덱스 |
|---------------------------|-----------|---------------|--------------------|----------------|----------------------|----------|--------------------|
| 색 구성 요소만     | 필수  | 해당 없음           | 해당 없음                | 필수       | 필수             | 옵션 | 필수           |
| 색 및 알파 결합    | 필수  | 해당 없음           | 해당 없음                | 옵션       | 필수             | 옵션 | 필수           |
| 색 및 알파 분리 | 필수  | 필수      | 옵션           | 해당 없음            | 필수             | 해당 없음      | 필수           |

 

BC7은 두 끝점 사이의 근사 선에 색상표를 정의합니다. 모드 값으로 블록당 끝점 쌍 보간 횟수를 결정됩니다. BC7은 텍셀당 하나의 팔레트 인덱스를 저장합니다.

한 쌍의 끝점에 해당하는 인덱스의 각 하위 집합에 대해 인코더가 해당 하위 집합에 대한 압축된 인덱스 데이터의 1비트 상태를 수정합니다. 이를 위해, 지정된 "수정" 인덱스에 대한 인덱스에서 최상위 비트를 0으로 설정할 수 있게 하는 끝점 순서를 선택합니다. 그런 다음 최상위 비트가 삭제되어 하위 집합당 1비트만 남게 됩니다. 하위 집합이 하나만 있는 블록 모드의 경우 수정 인덱스가 항상 인덱스 0입니다.

## <a name="span-iddecoding-the-bc7-formatspanspan-iddecoding-the-bc7-formatspanspan-iddecoding-the-bc7-formatspandecoding-the-bc7-format"></a><span id="Decoding-the-BC7-Format"></span><span id="decoding-the-bc7-format"></span><span id="DECODING-THE-BC7-FORMAT"></span>BC7 형식 디코딩


다음 의사 코드는 16바이트 BC7 블록이 지정된 (x,y)의 픽셀 압축을 푸는 단계를 간략히 보여 줍니다.

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

다음 의사 코드는 16바이트 BC7 블록이 지정된 각 하위 집합에 대한 끝점 색 및 알파 구성 요소를 완전히 디코딩하는 단계를 간략히 보여 줍니다.

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

각 하위 집합에 대해 보간된 각 구성 요소를 생성하려면 다음 알고리즘을 사용합니다. 여기에서 "c"는 생성할 구성 요소이고, "e0"은 하위 집합의 끝점 0의 구성 요소이고, "e1"은 하위 집합의 끝점 1의 구성 요소입니다.

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

다음 의사 코드는 색 및 알파 구성 요소에 대한 인덱스 및 비트 개수를 추출하는 방법을 보여 줍니다. 색과 알파가 분리된 블록에도 2개의 인덱스 데이터 집합이 있습니다. 그 중 하나는 벡터 채널용이고, 다른 하나는 스칼라 채널용입니다. 모드 4의 경우 이러한 인덱스는 너비가 다르며(2비트 또는 3비트) 벡터 또는 스칼라 데이터가 3비트 인덱스를 사용하는지 여부를 지정하는 1비트 선택기가 있습니다. (알파 비트 개수 추출은 색 비트 개수 추출과 비슷하지만 역동작은 **idxMode** 비트를 기반으로 합니다.)

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

## <a name="span-idbc7-format-mode-referencespanspan-idbc7-format-mode-referencespanspan-idbc7-format-mode-referencespanbc7-format-mode-reference"></a><span id="BC7-format-mode-reference"></span><span id="bc7-format-mode-reference"></span><span id="BC7-FORMAT-MODE-REFERENCE"></span>BC7 형식 모드 참조


이 섹션에는 BC7 텍스처 압축 형식 블록에 대한 8개의 블록 모드 및 비트 할당의 목록이 포함되어 있습니다.

블록 내의 각 하위 집합의 색은 2개의 명시적 끝점 색과 그 사이의 보간된 색 집합으로 표현됩니다. 블록의 인덱스 정밀도에 따라 각 하위 집합에 4, 8 또는 16개의 색이 있을 수 있습니다.

### <a name="span-idmode-0spanspan-idmode-0spanspan-idmode-0spanmode-0"></a><span id="Mode-0"></span><span id="mode-0"></span><span id="MODE-0"></span>모드 0

BC7 모드 0에는 다음과 같은 특성이 있습니다.

-   색 구성 요소만(알파 없음)
-   블록당 3개의 하위 집합
-   끝점마다 고유한 P 비트가 있는 RGBP 4.4.4.1 끝점
-   3비트 인덱스
-   16개의 파티션

![모드 0 비트 레이아웃](images/bc7-mode0.png)

### <a name="span-idmode-1spanspan-idmode-1spanspan-idmode-1spanmode-1"></a><span id="Mode-1"></span><span id="mode-1"></span><span id="MODE-1"></span>모드 1

BC7 모드 1에는 다음과 같은 특성이 있습니다.

-   색 구성 요소만(알파 없음)
-   블록당 2개의 하위 집합
-   하위 집합당 1개의 공유 P 비트가 있는 RGBP 6.6.6.1 끝점
-   3비트 인덱스
-   64개의 파티션

![모드 1 비트 레이아웃](images/bc7-mode1.png)

### <a name="span-idmode-2spanspan-idmode-2spanspan-idmode-2spanmode-2"></a><span id="Mode-2"></span><span id="mode-2"></span><span id="MODE-2"></span>모드 2

BC7 모드 2에는 다음과 같은 특성이 있습니다.

-   색 구성 요소만(알파 없음)
-   블록당 3개의 하위 집합
-   RGB 5.5.5 끝점
-   2비트 인덱스
-   64개의 파티션

![모드 2 비트 레이아웃](images/bc7-mode2.png)

### <a name="span-idmode-3spanspan-idmode-3spanspan-idmode-3spanmode-3"></a><span id="Mode-3"></span><span id="mode-3"></span><span id="MODE-3"></span>모드 3

BC7 모드 3에는 다음과 같은 특성이 있습니다.

-   색 구성 요소만(알파 없음)
-   블록당 2개의 하위 집합
-   하위 집합당 1개의 고유한 P 비트가 있는 RGBP 7.7.7.1 끝점
-   2비트 인덱스
-   64개의 파티션

![모드 3비트 레이아웃](images/bc7-mode3.png)

### <a name="span-idmode-4spanspan-idmode-4spanspan-idmode-4spanmode-4"></a><span id="Mode-4"></span><span id="mode-4"></span><span id="MODE-4"></span>모드 4

BC7 모드 4에는 다음과 같은 특성이 있습니다.

-   별도의 알파 구성 요소를 포함한 색 구성 요소
-   블록당 1개의 하위 집합
-   RGB 5.5.5 색 끝점
-   6비트 알파 끝점
-   16개의 2비트 인덱스
-   16개의 3비트 인덱스
-   2비트 구성 요소 회전
-   1비트 인덱스 선택기(2비트 또는 3비트 인덱스 사용)

![모드 4 비트 레이아웃](images/bc7-mode4.png)

### <a name="span-idmode-5spanspan-idmode-5spanspan-idmode-5spanmode-5"></a><span id="Mode-5"></span><span id="mode-5"></span><span id="MODE-5"></span>모드 5

BC7 모드 5에는 다음과 같은 특성이 있습니다.

-   별도의 알파 구성 요소를 포함한 색 구성 요소
-   블록당 1개의 하위 집합
-   RGB 7.7.7 색 끝점
-   6비트 알파 끝점
-   16개의 2비트 색 인덱스
-   16개의 2비트 알파 인덱스
-   2비트 구성 요소 회전

![모드 5 비트 레이아웃](images/bc7-mode5.png)

### <a name="span-idmode-6spanspan-idmode-6spanspan-idmode-6spanmode-6"></a><span id="Mode-6"></span><span id="mode-6"></span><span id="MODE-6"></span>모드 6

BC7 모드 6에는 다음과 같은 특성이 있습니다.

-   결합된 색 및 알파 구성 요소
-   블록당 1개의 하위 집합
-   RGBAP 7.7.7.7.1 색 및 알파 끝점(끝점마다 고유한 P 비트)
-   16개의 4비트 인덱스

![모드 6비트 레이아웃](images/bc7-mode6.png)

### <a name="span-idmode-7spanspan-idmode-7spanspan-idmode-7spanmode-7"></a><span id="Mode-7"></span><span id="mode-7"></span><span id="MODE-7"></span>모드 7

BC7 모드 7에는 다음과 같은 특성이 있습니다.

-   결합된 색 및 알파 구성 요소
-   블록당 2개의 하위 집합
-   RGBAP 5.5.5.5.1 색 및 알파 끝점(끝점마다 고유한 P 비트)
-   2비트 인덱스
-   64개의 파티션

![모드 7비트 레이아웃](images/bc7-mode7.png)

### <a name="span-idremarksspanspan-idremarksspanspan-idremarksspanremarks"></a><span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>설명

모드 8(최하위 바이트가 0x00으로 설정됨)은 예약되어 있습니다. 인코더에 이 모드를 사용하지 마십시오. 하드웨어에 이 모드를 전달하면 모두 0으로 초기화된 블록이 반환됩니다.

BC7에서 다음 방법 중 하나로 알파 구성 요소를 인코딩할 수 있습니다.

-   명시적 알파 구성 요소 인코딩이 없는 블록 유형 이러한 블록에서는 색 끝점에 RGB 전용 인코딩이 있고 알파 구성 요소는 원본 데이터의 모든 텍셀에 대해 1.0으로 디코딩됩니다.
-   색 및 알파 구성 요소가 결합된 블록 유형 이러한 블록에서는 끝점 색 값이 RGBA 형식으로 지정되고 알파 구성 요소 값이 색 값과 함께 보간됩니다.
-   색 및 알파 구성 요소가 분리된 블록 유형 이러한 블록에서는 색 값과 알파 값이 따로 지정되고 각각 자체 인덱스 집합을 가집니다. 결과적으로 유효 벤더와 스칼라 채널이 따로 인코딩됩니다. 여기에서 벡터는 일반적으로 색 채널 \[R, G, B\]를 지정하고 스칼라는 알파 채널 \[A\]를 지정합니다. 이 접근 방식을 지원하기 위해 별도의 2비트 필드가 인코딩에서 제공됩니다. 이를 통해 별도의 채널 인코딩을 스칼라 값으로 지정할 수 있습니다. 결과적으로 블록에 이 알파 인코딩의 다음 4가지 표현 중 하나가 있을 수 있습니다(2비트 필드로 표시됨).
    -   RGB|A: 알파 채널 분리
    -   AGB|R: "빨강" 색 채널 분리
    -   RAB|G: "녹색" 색 채널 분리
    -   RGA|B: "파랑" 색 채널 분리

    디코더가 디코딩 후 채널 순서를 다시 RGBA로 재정렬하므로 내부 블록 형식이 개발자에게 표시되지 않습니다. 색 구성 요소와 알파 구성 요소가 분리된 블록에도 2개의 인덱스 데이터 집합이 있습니다. 그 중 하나는 벡터 채널 집합용이고, 다른 하나는 스칼라 채널용입니다. (모드 4의 경우 이러한 인덱스는 너비가 다릅니다\[2비트 또는 3비트\]. 또한 모드 4에는 벡터 또는 스칼라 채널에서 3비트 인덱스를 사용하는지 여부를 지정하는 1비트 선택기가 있습니다.)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[텍스처 블록 압축](texture-block-compression.md)

 

 




