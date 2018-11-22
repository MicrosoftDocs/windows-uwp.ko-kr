---
title: 알파 채널을 사용하는 텍스처
description: 투명도 복잡성이 더욱 큰 텍스처 맵을 인코딩하는 방법은 두 가지입니다.
ms.assetid: 768A774A-4F21-4DDE-B863-14211DA92926
keywords:
- 알파 채널을 사용하는 텍스처
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: eef41642d371f3a8be451c2687eee007608c3b2e
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/22/2018
ms.locfileid: "7580258"
---
# <a name="textures-with-alpha-channels"></a>알파 채널을 사용하는 텍스처


두 가지 방법으로 더 복잡한 투명성을 표시하는 텍스처 맵을 인코딩할 수 있습니다. 각 경우에서 투명성을 설명하는 블록이 이미 설명된 64비트 블록 앞에 옵니다. 투명도는 픽셀당 4비트(명시적 인코딩)이거나, 혹은 비트 수가 더 적을 뿐만 아니라 선형 보간이 색상 인코딩에 사용되는 것과 유사한 4x4 비트맵으로 표현됩니다.

투명도 블록과 색상 블록은 다음 표와 같이 정렬됩니다.

| 워드 주소 | 64비트 블록                      |
|--------------|-----------------------------------|
| 3:0          | 투명도 블록                |
| 7:4          | 앞에서 설명한 64비트 블록 |

 

## <a name="span-idexplicit-texture-encodingspanspan-idexplicit-texture-encodingspanspan-idexplicit-texture-encodingspanexplicit-texture-encoding"></a><span id="Explicit-Texture-Encoding"></span><span id="explicit-texture-encoding"></span><span id="EXPLICIT-TEXTURE-ENCODING"></span>명시적 텍스처 인코딩


명시적 텍스처 인코딩(BC2 형식)에서는 투명도를 나타내는 텍셀의 알파 성분이 픽셀당 4비트인 4x4 비트맵으로 인코딩됩니다. 이 4개 비트는 디더링을 통하거나, 혹은 알파 데이터에서 최상위 비트(MSB) 4개를 사용하는 등 다양한 방법으로 얻을 수 있습니다. 이렇게 얻을 수는 있지만 보간 형태 없이 있는 상태 그대로 사용됩니다.

다음은 64비트 투명도 블록을 나타낸 다이어그램입니다.

![64비트 투명도 블록 다이어그램](images/colors4.png)

**참고**  Direct3D의 압축 방법에서는 최상위 비트 4 개를 사용 합니다.

 

다음은 각 16비트 워드마다 알파 정보가 메모리에 배치되는 방식을 나타낸 표입니다.

워드 0에 대한 레이아웃:

| 비트          | 알파      |
|---------------|------------|
| 3:0(LSB\*)   | \[0\]\[0\] |
| 7:4           | \[0\]\[1\] |
| 11:8          | \[0\]\[2\] |
| 15:12(MSB\*) | \[0\]\[3\] |

 

\*최하위 비트, 최상위 비트(MSB)

워드 1에 대한 레이아웃:

| 비트        | 알파      |
|-------------|------------|
| 3:0(LSB)   | \[1\]\[0\] |
| 7:4         | \[1\]\[1\] |
| 11:8        | \[1\]\[2\] |
| 15:12(MSB) | \[1\]\[3\] |

 

워드 2에 대한 레이아웃:

| 비트        | 알파      |
|-------------|------------|
| 3:0(LSB)   | \[2\]\[0\] |
| 7:4         | \[2\]\[1\] |
| 11:8        | \[2\]\[2\] |
| 15:12(MSB) | \[2\]\[3\] |

 

워드 3에 대한 레이아웃:

| 비트        | 알파      |
|-------------|------------|
| 3:0(LSB)   | \[3\]\[0\] |
| 7:4         | \[3\]\[1\] |
| 11:8        | \[3\]\[2\] |
| 15:12(MSB) | \[3\]\[3\] |

 

텍셀의 투명 여부를 알기 위해 BC1에서 사용되는 색상 비교는 이 형식에서 사용되지 않습니다. 따라서 색상 비교 없이 색상 데이터가 항상 4색상 모드인 것처럼 처리됩니다.

## <a name="span-idthree-bit-linear-alpha-interpolationspanspan-idthree-bit-linear-alpha-interpolationspanspan-idthree-bit-linear-alpha-interpolationspanthree-bit-linear-alpha-interpolation"></a><span id="Three-Bit-Linear-Alpha-Interpolation"></span><span id="three-bit-linear-alpha-interpolation"></span><span id="THREE-BIT-LINEAR-ALPHA-INTERPOLATION"></span>3비트 선형 알파 보간


BC3 형식의 투명도 인코딩은 색상에 사용되는 선형 인코딩과 기본 개념이 유사합니다. 8비트 알파 값 2개와 픽셀당 3비트인 4x4 비트맵이 블록에서 첫 번째 8바이트에 저장됩니다. 이 대표 알파 값이 중간 알파 값을 보간하는 데 사용됩니다. 추가 정보는 두 알파 값이 저장되는 방식으로 사용할 수 있습니다. alpha\_0이 alpha\_1보다 크면 보간을 통해 6개의 중간 알파 값이 생성됩니다. 그렇지 않으면 지정된 양 끝단의 알파 값 사이에 중간 알파 값 4개가 보간됩니다. 묵시적으로 추가되는 알파 값 2개는 0(완전 투명)과 255(완전 불투명)입니다.

다음은 이 알고리즘을 나타내는 코드 예제입니다.

```
// 8-alpha or 6-alpha block?    
if (alpha_0 > alpha_1) {    
    // 8-alpha block:  derive the other six alphas.    
    // Bit code 000 = alpha_0, 001 = alpha_1, others are interpolated.
    alpha_2 = (6 * alpha_0 + 1 * alpha_1 + 3) / 7;    // bit code 010
    alpha_3 = (5 * alpha_0 + 2 * alpha_1 + 3) / 7;    // bit code 011
    alpha_4 = (4 * alpha_0 + 3 * alpha_1 + 3) / 7;    // bit code 100
    alpha_5 = (3 * alpha_0 + 4 * alpha_1 + 3) / 7;    // bit code 101
    alpha_6 = (2 * alpha_0 + 5 * alpha_1 + 3) / 7;    // bit code 110
    alpha_7 = (1 * alpha_0 + 6 * alpha_1 + 3) / 7;    // bit code 111  
}    
else {  
    // 6-alpha block.    
    // Bit code 000 = alpha_0, 001 = alpha_1, others are interpolated.
    alpha_2 = (4 * alpha_0 + 1 * alpha_1 + 2) / 5;    // Bit code 010
    alpha_3 = (3 * alpha_0 + 2 * alpha_1 + 2) / 5;    // Bit code 011
    alpha_4 = (2 * alpha_0 + 3 * alpha_1 + 2) / 5;    // Bit code 100
    alpha_5 = (1 * alpha_0 + 4 * alpha_1 + 2) / 5;    // Bit code 101
    alpha_6 = 0;                                      // Bit code 110
    alpha_7 = 255;                                    // Bit code 111
}
```

알파 블록의 메모리 레이아웃은 다음과 같습니다.

| 바이트 | 알파                                                          |
|------|----------------------------------------------------------------|
| 0    | Alpha\_0                                                       |
| 1    | Alpha\_1                                                       |
| 2    | \[0\]\[2\] (2 MSBs), \[0\]\[1\], \[0\]\[0\]                    |
| 3    | \[1\]\[1\] (1 MSB), \[1\]\[0\], \[0\]\[3\], \[0\]\[2\] (1 LSB) |
| 4    | \[1\]\[3\], \[1\]\[2\], \[1\]\[1\] (2 LSB)                    |
| 5    | \[2\]\[2\] (2 MSBs), \[2\]\[1\], \[2\]\[0\]                    |
| 6    | \[3\]\[1\] (1 MSB), \[3\]\[0\], \[2\]\[3\], \[2\]\[2\] (1 LSB) |
| 7    | \[3\]\[3\], \[3\]\[2\], \[3\]\[1\] (2 LSB)                    |

 

텍셀의 투명 여부를 알기 위해 BC1에서 사용되는 색상 비교는 이 형식에서 사용되지 않습니다. 따라서 색상 비교 없이 색상 데이터가 항상 4색상 모드인 것처럼 처리됩니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[압축된 텍스처 리소스](compressed-texture-resources.md)

 

 




