---
title: 불투명 및 1비트 알파 텍스처
description: 질감 형식 BC1는 불투명 하거나 투명 한 색이 하나인 질감에 대 한 것입니다.
ms.assetid: 8C53ACDD-72ED-4307-B4F3-2FCF9A9F53EC
keywords:
- 불투명 및 1비트 알파 텍스처
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c08489055bfd4e867310c5bdec6fea655ddc6d41
ms.sourcegitcommit: 53c00939b20d4b0a294936df3d395adb0c13e231
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91933014"
---
# <a name="span-iddirect3dconceptsopaque_and_1-bit_alpha_texturesspanopaque-and-1-bit-alpha-textures"></a><span id="direct3dconcepts.opaque_and_1-bit_alpha_textures"></span>불투명 및 1비트 알파 텍스처

질감 형식 BC1는 불투명 하거나 투명 한 색이 하나인 질감에 대 한 것입니다.

각 불투명 또는 1 비트 알파 블록에 대해 2 16 비트 값 (RGB 5:6:5 형식) 및 픽셀당 2 비트의 4x4 비트맵이 저장 됩니다. 이 합계는 16 텍셀의 경우 64 비트이 고, 텍셀 당 4 비트입니다. 블록 비트맵에서 네 가지 색 중에서 선택할 수 있는 텍셀 당 2 비트가 있습니다. 두 색은 인코딩된 데이터에 저장 됩니다. 다른 두 색은 선형 보간을 통해 이러한 저장 된 색에서 파생 됩니다. 이 레이아웃은 다음 다이어그램에 나와 있습니다.

![비트맵 레이아웃 다이어그램](images/colors1.png)

1 비트 알파 형식은 블록에 저장 된 2 16 비트 색 값을 비교 하 여 불투명 형식과 구분 됩니다. 이러한 숫자는 부호 없는 정수로 처리 됩니다. 첫 번째 색이 두 번째 색 보다 크면 불투명 텍셀 정의 되어 있음을 의미 합니다. 즉, 텍셀를 나타내는 데 사용 되는 4 가지 색이 있습니다. 4 색 인코딩에서는 두 개의 파생 된 색이 있으며 네 색은 모두 RGB 색 공간에 균등 하 게 분포 됩니다. 이 형식은 RGB 5:6:5 형식과 유사 합니다. 그렇지 않으면 1 비트 알파 투명도의 경우 세 가지 색이 사용 되 고 네 번째는 투명 텍셀을 나타내기 위해 예약 됩니다.

3 색 인코딩에서 하나의 파생 된 색과 네 번째 2 비트 코드는 투명 텍셀 (알파 정보)를 나타내기 위해 예약 됩니다. 이 형식은 최종 비트가 알파 마스크 인코딩에 사용 되는 RGBA 5:5:5:1와 유사 합니다.

다음 코드 예제에서는 3 또는 4 색 인코딩을 선택 했는지 여부를 결정 하는 알고리즘을 보여 줍니다.

```cpp
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

혼합 하기 전에 투명도 픽셀의 RGBA 구성 요소를 0으로 설정 하는 것이 좋습니다.

다음 표에서는 8 바이트 블록의 메모리 레이아웃을 보여 줍니다. 첫 번째 인덱스가 y 좌표에 해당 하 고 두 번째 인덱스가 x 좌표에 해당 하는 것으로 가정 합니다. 예를 들어 텍셀 \[ 1 \] \[ 2는 \] (x, y) = (2, 1)에서 질감 맵 픽셀을 참조 합니다.

다음은 8 바이트 (64 비트) 블록의 메모리 레이아웃입니다.

| 단어 주소 | 16 비트 단어    |
|--------------|----------------|
| 0            | 색 \_ 0       |
| 1            | 색 \_ 1       |
| 2            | Bitmap Word \_ 0 |
| 3            | 비트맵 단어 \_ 1 |

 

색 \_ 0 및 색 \_ 1, 두 극단 색은 다음과 같이 레이아웃 됩니다.

| 비트        | 색                 |
|-------------|-----------------------|
| 4:0 (LSB \* ) | 파랑 색 구성 요소  |
| 10:5        | 녹색 색 구성 요소 |
| 15:11       | 빨강 색 구성 요소   |

 

\*최하위 비트

비트맵 단어 \_ 0은 다음과 같이 레이아웃 됩니다.

| 비트          | 텍셀           |
|---------------|-----------------|
| 1:0 (LSB)     | 텍셀 \[ 0 \] \[ 0\] |
| 3:2           | 텍셀 \[ 0 \] \[ 1\] |
| 5:4           | 텍셀 \[ 0 \] \[ 2\] |
| 7:6           | 텍셀 \[ 0 \] \[ 3\] |
| 9:8           | 텍셀 \[ 1 \] \[ 0\] |
| 11:10         | 텍셀 \[ 1 \] \[ 1\] |
| 13:12         | 텍셀 \[ 1 \] \[ 2\] |
| 15:14 (MSB \* ) | 텍셀 \[ 1 \] \[ 3\] |

 

\*가장 중요 한 비트 (MSB)

비트맵 단어 \_ 1은 다음과 같이 레이아웃 됩니다.

| 비트        | 텍셀           |
|-------------|-----------------|
| 1:0 (LSB)   | 텍셀 \[ 2 \] \[ 0\] |
| 3:2         | 텍셀 \[ 2 \] \[ 1\] |
| 5:4         | 텍셀 \[ 2 \] \[ 2\] |
| 7:6         | 텍셀 \[ 2 \] \[ 3\] |
| 9:8         | 텍셀 \[ 3 \] \[ 0\] |
| 11:10       | 텍셀 \[ 3 \] \[ 1\] |
| 13:12       | 텍셀 \[ 3 \] \[ 2\] |
| 15:14 (MSB) | 텍셀 \[ 3 \] \[ 3\] |

 

## <a name="span-idexample_of_opaque_color_encodingspanspan-idexample_of_opaque_color_encodingspanspan-idexample_of_opaque_color_encodingspanexample-of-opaque-color-encoding"></a><span id="Example_of_Opaque_Color_Encoding"></span><span id="example_of_opaque_color_encoding"></span><span id="EXAMPLE_OF_OPAQUE_COLOR_ENCODING"></span>불투명 색 인코딩의 예


불투명 인코딩의 예로 red 및 black 색이 극단에 있다고 가정 합니다. 빨간색은 색 \_ 0이 고 검정은 색 \_ 1입니다. 두 색 사이에 균일 하 게 분산 된 그라데이션을 형성 하는 네 가지 보간된 색이 있습니다. 4x4 비트맵의 값을 확인 하기 위해 다음 계산을 사용 합니다.

```cpp
00 ? color_0
01 ? color_1
10 ? 2/3 color_0 + 1/3 color_1
11 ? 1/3 color_0 + 2/3 color_1
```

비트맵은 다음 다이어그램과 같습니다.

![빨강 및 검정의 확장 된 비트맵 레이아웃 다이어그램](images/colors2.png)

다음과 같이 표시 되는 일련의 색과 같습니다.

**참고**    이미지에서 픽셀 (0, 0)은 왼쪽 위에 표시 됩니다.

 

![불투명 하 게 인코딩된 그라데이션 그림](images/redsquares.png)

## <a name="span-idexample_of_1_bit_alpha_encodingspanspan-idexample_of_1_bit_alpha_encodingspanspan-idexample_of_1_bit_alpha_encodingspanexample-of-1-bit-alpha-encoding"></a><span id="Example_of_1_Bit_Alpha_Encoding"></span><span id="example_of_1_bit_alpha_encoding"></span><span id="EXAMPLE_OF_1_BIT_ALPHA_ENCODING"></span>1 비트 알파 인코딩의 예


부호 없는 16 비트 정수 색 \_ 0이 부호 없는 16 비트 정수 색 1 보다 작은 경우이 형식이 선택 됩니다 \_ . 이 형식을 사용할 수 있는 위치의 예는 파란색 하늘에 대해 표시 된 트리에 그대로 둡니다. 일부 텍셀는 투명 한 것으로 표시 될 수 있지만, 3 개의 녹색 음영은 여전히 리프에 사용할 수 있습니다. 두 색은 극단를 수정 하 고, 세 번째 색은 보간된 색입니다.

다음 그림은 이러한 그림의 예입니다.

![1 비트 알파 인코딩 그림](images/greenthing.png)

이미지가 흰색으로 표시 되는 경우 텍셀는 투명 하 게 인코딩됩니다. 투명 한 텍셀의 RGBA 구성 요소는 혼합 전에 0으로 설정 해야 합니다.

색 및 투명도에 대 한 비트맵 인코딩은 다음 계산을 사용 하 여 결정 됩니다.

```cpp
00 ? color_0
01 ? color_1
10 ? 1/2 color_0 + 1/2 color_1
11   ?   Transparent
```

비트맵은 다음 다이어그램과 같습니다.

![밝은 녹색 및 짙은 녹색의 확장 된 비트맵 레이아웃 다이어그램](images/colors3.png)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[압축된 텍스처 리소스](compressed-texture-resources.md)

 

 




