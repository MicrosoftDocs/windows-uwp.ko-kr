---
title: 불투명 및 1비트 알파 텍스처
description: 텍스처 형식 BC1은 불투명한 텍스처 또는 투명한 단색 텍스처를 위한 것입니다.
ms.assetid: 8C53ACDD-72ED-4307-B4F3-2FCF9A9F53EC
keywords:
- 불투명 및 1비트 알파 텍스처
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4227a3ad77eadaa40e47420a5fdab6d65c875da5
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8480804"
---
# <a name="span-iddirect3dconceptsopaqueand1-bitalphatexturesspanopaque-and-1-bit-alpha-textures"></a><span id="direct3dconcepts.opaque_and_1-bit_alpha_textures"></span>불투명 및 1비트 알파 텍스처


텍스처 형식 BC1은 불투명한 텍스처 또는 투명한 단색 텍스처를 위한 것입니다.

각 불투명 또는 1 비트 알파 블록에 대해 16비트 값(RGB 5:6:5 형식) 2개와 픽셀당 2비트인 4x4 비트맵이 저장됩니다. 이를 모두 합치면 16개 텍셀에 대해 64비트 또는 텍셀당 네 비트입니다. 블록 비트맵에는 네 가지 색 중에서 선택할 수 있는 텍셀당 2비트가 있습니다. 네 가지 색 중 두 가지는 인코딩된 데이터에 저장됩니다. 나머지 두 색은 선형 보간을 통해 저장된 두 가지 색에서 파생됩니다. 이 레이아웃은 다음 그림에 표시되어 있습니다.

![비트맵 레이아웃 그림](images/colors1.png)

1비트 알파 형식은 블록에 저장된 두 가지 16비트 색 값과 비교하여 불투명 형식과 구별됩니다. 그 값들은 부호 없는 정수로 처리됩니다. 첫 번째 색이 두 번째 색보다 큰 경우, 이는 불투명 텍셀만 정의하였음을 뜻합니다. 즉, 네 가지 색을 사용하여 텍셀을 나타낸다는 것입니다. 4색 인코딩에는 파생된 색 두 가지가 있고 네 가지 색 모두는 RGB 색 공간에 동등하게 배포됩니다. 이 형식은 RGB 5:6:5 형식과 유사합니다. 그렇지 않은 경우 1비트 알파 투명도에 대해 세 가지 색을 사용하고 네 번째 색은 투명 텍셀을 표시하기 위해 남겨둡니다.

3색 인코딩에는 파생된 색이 하나 있고 네 번째 2비트 코드는 투명 텍셀(알파 정보)를 나타내기 위해 남겨둡니다. 이 형식은 알파 마스크 인코딩에 최종 비트가 사용되는 RGBA 5:5:5:1과 유사합니다.

다음 코드 예시에서는 3색 또는 4색 인코딩 선택 여부를 결정하는 알고리즘을 보여줍니다.

```
if (color_0 > color_1) 
{
    // Four-color block: derive the other two colors. 
    
    // 00 = color_0, 01 = color_1, 10 = color_2, 11 = color_3
    // These 2-bit codes correspond to the 2-bit fields 
    // stored in the 64-bit block.
    color_2 = (2 * color_0 + color_1 + 1) / 3;
    color_3 = (color_0 + 2 * color_1 + 1) / 3;
}    
else
{ 
    // Three-color block: derive the other color.
    // 00 = color_0,  01 = color_1,  10 = color_2,  
    // 11 = transparent.
    // These 2-bit codes correspond to the 2-bit fields 
    // stored in the 64-bit block. 
    color_2 = (color_0 + color_1) / 2;    
    color_3 = transparent;    

}
```

혼합하기 전에 투명도 픽셀의 RGBA 구성 요소를 0으로 설정하는 것이 좋습니다.

다음 표는 8바이트 블록의 메모리 레이아웃을 나타냅니다. 첫 번째 인덱스는 y 좌표에 해당하고 두 번째 인덱스는 x 좌표에 해당하는 것으로 가정합니다. 예를 들어 Texel\[1\]\[2\]는 (x y) = (2,1)의 텍스처 맵 픽셀을 가리킵니다.

다음은 8바이트(64비트) 블록의 메모리 레이아웃입니다.

| 단어 주소 | 16비트 단어    |
|--------------|----------------|
| 0            | 색\_0       |
| 1            | 색\_1       |
| 2            | 비트맵 단어\_0 |
| 3            | 비트맵 단어\_1 |

 

두 극단에 있는 색인 색\_0 및 색\_1은 다음과 같이 배치됩니다.

| 비트        | 색                 |
|-------------|-----------------------|
| 4:0 (LSB\ *) | 파란색 구성 요소  |
| 10:5        | 녹색 구성 요소 |
| 15:11       | 빨간색 구성 요소   |

 

\*최하위 비트

비트맵 Word\_0은 다음과 같이 배치됩니다.

| 비트          | 텍셀           |
|---------------|-----------------|
| 1:0 (LSB)     | 텍셀\[0\]\[0\] |
| 3:2           | 텍셀\[0\]\[1\] |
| 5:4           | 텍셀\[0\]\[2\] |
| 7:6           | 텍셀\[0\]\[3\] |
| 9:8           | 텍셀\[1\]\[0\] |
| 11:10         | 텍셀\[1\]\[1\] |
| 13:12         | 텍셀\[1\]\[2\] |
| 15:14 (MSB\*) | 텍셀\[1\]\[3\] |

 

\*최상위 비트(MSB)

비트맵 단어\_1은 다음과 같이 배치됩니다.

| 비트        | 텍셀           |
|-------------|-----------------|
| 1:0 (LSB)   | 텍셀\[2\]\[0\] |
| 3:2         | 텍셀\[2\]\[1\] |
| 5:4         | 텍셀\[2\]\[2\] |
| 7:6         | 텍셀\[2\]\[3\] |
| 9:8         | 텍셀\[3\]\[0\] |
| 11:10       | 텍셀\[3\]\[1\] |
| 13:12       | 텍셀\[3\]\[2\] |
| 15:14 (MSB) | 텍셀\[3\]\[3\] |

 

## <a name="span-idexampleofopaquecolorencodingspanspan-idexampleofopaquecolorencodingspanspan-idexampleofopaquecolorencodingspanexample-of-opaque-color-encoding"></a><span id="Example_of_Opaque_Color_Encoding"></span><span id="example_of_opaque_color_encoding"></span><span id="EXAMPLE_OF_OPAQUE_COLOR_ENCODING"></span>불투명 색 인코딩의 예


불투명 인코딩 예로, 빨간색과 검은색이 극단에 있다고 가정합시다. 빨간색은 color\_0이고 검은색은 color\_1입니다. 이들 사이에 균등하게 분산된 그라데이션을 이루는 보간된 색이 4가지입니다. 4x4 비트맵에 대한 값을 확인하려면 다음과 같이 계산하면 됩니다.

```
00 ? color_0
01 ? color_1
10 ? 2/3 color_0 + 1/3 color_1
11 ? 1/3 color_0 + 2/3 color_1
```

비트맵은 다음 그림과 같습니다.

![확장된 비트맵 레이아웃 그림](images/colors2.png)

이것은 다음과 같이 일련의 색으로 표시됩니다.

**참고**  이미지에서 픽셀 (0, 0) 왼쪽 위에 나타납니다.

 

![인코딩된 불투명 그라데이션 그림](images/redsquares.png)

## <a name="span-idexampleof1bitalphaencodingspanspan-idexampleof1bitalphaencodingspanspan-idexampleof1bitalphaencodingspanexample-of-1-bit-alpha-encoding"></a><span id="Example_of_1_Bit_Alpha_Encoding"></span><span id="example_of_1_bit_alpha_encoding"></span><span id="EXAMPLE_OF_1_BIT_ALPHA_ENCODING"></span>1비트 알파 인코딩의 예


부호 없는 16비트 정수 color\_0이 부호 없는 16비트 정수 color\_1보다 작은 경우, 이 형식을 선택합니다. 이 형식을 사용할 수 있는 예로 푸른 하늘을 배경으로 하여 표시된 나뭇잎을 들 수 있습니다. 일부 텍셀을 투명하게 표시할 수 있고, 아울러 녹색 음영 세 개를 여전히 나뭇잎에 사용할 수 있습니다. 두 가지 색은 극단을 수정하고 세 번째 색은 보간된 색입니다.

다음은 그러한 그림의 예입니다.

![1비트 알파 인코딩 그림](images/greenthing.png)

이미지가 흰색으로 표시되는 경우, 텍셀은 투명한 것으로 인코딩됩니다. 투명 텍셀의 RGBA 구성 요소는 혼합하기 전에 0으로 설정해야 합니다.

색 및 투명도의 비트맵 인코딩은 다음과 같이 계산하여 결정합니다.

```
00 ? color_0
01 ? color_1
10 ? 1/2 color_0 + 1/2 color_1
11   ?   Transparent
```

비트맵은 다음 그림과 같습니다.

![확장된 비트맵 레이아웃 그림](images/colors3.png)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[압축된 텍스처 리소스](compressed-texture-resources.md)

 

 




