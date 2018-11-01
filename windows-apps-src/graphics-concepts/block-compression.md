---
title: 블록 압축
description: 블록 압축은 성능 향상을 위해 텍스처 크기와 메모리 공간을 줄이는 손실 허용 텍스처 압축 기술입니다. 블록이 압축된 텍스처는 색당 32비트인 텍스처보다 작을 수 있습니다.
ms.assetid: 2FAD6BE8-C6E4-4112-AF97-419CD27F7C73
keywords:
- 블록 압축
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4c959ced5ada9145ca494dd023c9aa802d7dccc2
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5886417"
---
# <a name="block-compression"></a>블록 압축


블록 압축은 성능 향상을 위해 텍스처 크기와 메모리 공간을 줄이는 손실 허용 텍스처 압축 기술입니다. 블록이 압축된 텍스처는 색당 32비트인 텍스처보다 작을 수 있습니다.

블록 압축은 텍스처 크기를 줄이는 텍스처 압축 기술입니다. 블록이 압축된 텍스처는 색당 32비트인 텍스처보다 최대 75% 작을 수 있습니다. 메모리 공간이 더 작기 때문에 블록 압축을 사용하면 대개 응용 프로그램 성능이 향상됩니다.

블록 압축은 손실이 있지만 잘 작동하며 파이프라인에 의해 변환 및 필터링되는 모든 텍스처에 권장됩니다. 화면에 직접 매핑되는 텍스처(아이콘 및 텍스트와 같은 UI 요소)는 아티팩트가 눈에 더 잘 띄기 때문에 압축에 적합하지 않습니다.

블록이 압축된 텍스처는 모든 차원에서 크기 4의 배수로 만들어야 하며 파이프라인의 출력으로 사용할 수 없습니다.

## <a name="span-idbasicsspanspan-idbasicsspanspan-idbasicsspanhow-block-compression-works"></a><span id="Basics"></span><span id="basics"></span><span id="BASICS"></span>블록 압축의 작동 방식


블록 압축은 색 데이터를 저장하는 데 필요한 메모리 양을 줄이는 기술입니다. 일부 색을 원래 크기로 저장하고 다른 색은 인코딩 체계를 사용하여 저장하여 이미지 저장에 필요한 메모리 양을 크게 줄일 수 있습니다. 압축된 데이터를 하드웨어에서 자동으로 디코딩하므로 압축된 텍스처를 사용해도 성능에는 영향이 없습니다.

다음 두 가지 예제를 통해 압축이 어떻게 작동하는지 알 수 있습니다. 첫 번째 예제에서는 압축되지 않은 데이터 저장 시 사용되는 메모리 양을 설명하고, 두 번째 예제에서는 압축된 데이터 저장 시 사용되는 메모리 양을 설명합니다.

-   [압축되지 않은 데이터 저장](#storing-uncompressed-data)
-   [압축된 데이터 저장](#storing-compressed-data)

### <a name="span-idstoringuncompresseddataspanspan-idstoringuncompresseddataspanspan-idstoringuncompresseddataspanspan-idstoring-uncompressed-dataspanstoring-uncompressed-data"></a><span id="Storing_Uncompressed_Data"></span><span id="storing_uncompressed_data"></span><span id="STORING_UNCOMPRESSED_DATA"></span><span id="storing-uncompressed-data"></span>압축되지 않은 데이터 저장

다음 그림에는 압축되지 않은 4×4 텍스처가 표시되어 있습니다. 각 색이 단일 색 구성 요소(예: 빨강)를 포함하며 1바이트의 메모리에 저장된다고 가정합니다.

![압축되지 않은 4x4 텍스처](images/d3d10-block-compress-1.png)

다음 그림과 같이 압축되지 않은 데이터가 메모리에 순서대로 배치되고 16바이트가 필요합니다.

![순차적 메모리의 압축되지 않은 데이터](images/d3d10-block-compress-2.png)

### <a name="span-idstoringcompresseddataspanspan-idstoringcompresseddataspanspan-idstoringcompresseddataspanspan-idstoring-compressed-dataspanstoring-compressed-data"></a><span id="Storing_Compressed_Data"></span><span id="storing_compressed_data"></span><span id="STORING_COMPRESSED_DATA"></span><span id="storing-compressed-data"></span>압축된 데이터 저장

지금까지 압축되지 않은 이미지에 사용되는 메모리 양을 알아보았으므로 이번에는 압축된 이미지로 절약되는 메모리 양을 살펴보겠습니다. [BC1](#bc1) 압축 형식은 다음 그림과 같이 2개의 색(각각 1바이트)과 16개의 3비트 인덱스(48비트 또는 6바이트)를 저장합니다. 이들은 텍스처의 원래 색을 보간하는 데 사용됩니다.

![bc1 압축 형식](images/d3d10-block-compress-3.png)

압축된 데이터를 저장하는 데 필요한 총 공간은 8바이트이며 이는 압축되지 않은 데이터를 저장하는 데 필요한 공간의 50%에 불과합니다. 여러 구성 요소가 사용될 경우 더 많은 공간이 절약됩니다.

블록 압축 사용 시 메모리가 많이 절약되어 성능이 향상됩니다. 단, 색 보간 때문에 이미지 품질이 저하되지만 대개 이러한 품질 저하는 그렇게 두드러지지 않습니다.

다음 섹션에는 어떻게 Direct3D로 응용 프로그램에서 블록 압축을 사용할 수 있는지를 보여 줍니다.

## <a name="span-idusingblockcompressionspanspan-idusingblockcompressionspanspan-idusingblockcompressionspanusing-block-compression"></a><span id="Using_Block_Compression"></span><span id="using_block_compression"></span><span id="USING_BLOCK_COMPRESSION"></span>블록 압축 사용


블록이 압축된 형식을 지정하는 것을 제외하고는, 압축되지 않은 텍스처와 같은 방식으로 블록이 압축된 텍스처를 만듭니다.

그런 다음 뷰를 만들어 파이프라인에 텍스처를 바인딩합니다. 블록이 압축된 텍스처는 셰이더 단계에 대한 입력으로만 사용할 수 있으므로 셰이더 리소스 뷰를 만듭니다.

압축되지 않은 텍스처를 사용할 때와 같은 방식으로 블록이 압축된 텍스처를 사용합니다. 응용 프로그램에서 블록이 압축된 데이터에 대한 메모리 포인터를 가져올 경우 MIP 맵의 메모리 패딩을 고려해야 합니다. MIP 맵의 메모리 패딩으로 인해 선언된 크기가 실제 크기와 달라지기 때문입니다.

-   [가상 크기와 물리적 크기](#virtual-size-versus-physical-size)

### <a name="span-idvirtualsizespanspan-idvirtualsizespanspan-idvirtualsizespanspan-idvirtual-size-versus-physical-sizespanvirtual-size-versus-physical-size"></a><span id="Virtual_Size"></span><span id="virtual_size"></span><span id="VIRTUAL_SIZE"></span><span id="virtual-size-versus-physical-size"></span>가상 크기와 물리적 크기

메모리 포인터를 사용하여 블록이 압축된 텍스처의 메모리를 탐색하는 응용 프로그램 코드가 있는 경우 응용 프로그램 코드 수정이 필요할 수 있는 중요한 고려 사항이 하나 있습니다. 블록 압축 알고리즘은 4x4 텍셀 블록에 작용하기 때문에 블록이 압축된 텍스처는 모든 차원에서 크기 4의 배수여야 합니다. 초기 차원을 4로 나눌 수 있지만 나눈 수준은 4로 나눌 수 없는 MIP 맵의 경우 문제가 생깁니다. 다음 다이어그램에서는 각 MIP 맵 수준의 가상(선언된) 크기와 물리적(실제) 크기 사이의 영역 차이를 보여 줍니다.

![압축되지 않은 MIP 맵 수준 및 압축된 MIP 맵 수준](images/d3d10-block-compress-pad.png)

다이어그램의 왼쪽은 압축되지 않은 60×40 텍스처에 대해 생성되는 MIP 맵 수준 크기를 보여 줍니다. 최상위 수준의 크기는 텍스처를 생성하는 API 호출에서 가져옵니다. 각 하위 수준은 이전 수준 크기의 절반입니다. 압축되지 않은 텍스처의 경우 가상(선언된) 크기와 물리적(실제) 크기 사이에 차이가 없습니다.

다이어그램의 오른쪽은 압축된 동일한 60×40 텍스처에 대해 생성되는 MIP 맵 수준 크기를 보여 줍니다. 두 번째 수준과 세 번째 수준에는 모든 수준에서 크기를 4의 계수로 만드는 메모리 패딩이 있습니다. 이는 알고리즘이 4×4 텍셀 블록에 작용하는 데 필요합니다. 4×4보다 작은 MIP 맵 수준을 고려하는 경우 특히 그렇습니다. 텍스처 메모리가 할당될 때 이렇게 작은 MIP 맵 수준의 크기는 가장 가까운 4의 계수로 반올림됩니다.

샘플링 하드웨어는 가상 크기를 사용합니다. 텍스처가 샘플링될 때 메모리 패딩은 무시됩니다. 4×4보다 작은 MIP 맵 수준의 경우 처음 네 개의 텍셀만 2×2 맵에 사용되며 첫 번째 텍셀만 1×1 블록에 사용됩니다. 그러나 메모리 패딩을 포함한 실제 크기를 노출하는 API 구조는 없습니다.

따라서 블록이 압축된 데이터가 포함된 영역을 복사할 때 정렬된 메모리 블록을 주의해서 사용해야 합니다. 메모리 포인터를 가져오는 응용 프로그램에서 이를 수행하려면 포인터가 표면 피치를 사용하여 물리적 메모리 크기를 고려하는지 확인하세요.

## <a name="span-idcompressionalgorithmsspanspan-idcompressionalgorithmsspanspan-idcompressionalgorithmsspancompression-algorithms"></a><span id="Compression_Algorithms"></span><span id="compression_algorithms"></span><span id="COMPRESSION_ALGORITHMS"></span>압축 알고리즘


Direct3D의 블록 압축 기술은 압축되지 않은 텍스처 데이터를 4×4 블록으로 나누고 각 블록을 압축한 다음 데이터를 저장합니다. 이러한 이유로 압축할 텍스처의 차원은 4의 배수여야 합니다.

![블록 압축](images/d3d10-compression-1.png)

위의 다이어그램에서는 텍셀 블록으로 분할된 텍스처를 보여 줍니다. 첫 번째 블록은 a부터 p까지의 16개 텍셀의 레이아웃을 보여 주지만 모든 블록은 동일한 데이터 구성을 가집니다.

Direct3D는 여러 압축 체계를 구현하며 각 압축 체계는 저장된 구성 요소 수, 구성 요소당 비트 수 및 사용된 메모리 양에 대한 장단점이 서로 다릅니다. 다음 표를 사용하여 데이터 형식에 가장 적합한 형식과 응용 프로그램에 가장 잘 맞는 데이터 해상도를 고를 수 있습니다.

| 원본 데이터                     | 데이터 압축 해상도(비트 단위) | 적합한 압축 형식 |
|---------------------------------|---------------------------------------|--------------------------------|
| 3분 색 및 알파 | 색(5:6:5), 알파(1) 또는 알파 없음  | [BC1](#bc1)                    |
| 3분 색 및 알파 | 색(5:6:5), 알파(4)              | [BC2](#bc2)                    |
| 3분 색 및 알파 | 색(5:6:5), 알파(8)              | [BC3](#bc3)                    |
| 1분 색             | 1분(8)                     | [BC4](#bc4)                    |
| 2분 색             | 2분(8:8)                  | [BC5](#bc5)                    |

 

-   [BC1](#bc1)
-   [BC2](#bc2)
-   [BC3](#bc3)
-   [BC4](#bc4)
-   [BC5](#bc5)

### <a name="span-idbc1spanspan-idbc1spanbc1"></a><span id="BC1"></span><span id="bc1"></span>BC1

첫 번째 블록 압축 형식(BC1)(DXGI\_FORMAT\_BC1\_TYPELESS, DXGI\_FORMAT\_BC1\_UNORM 또는 DXGI\_BC1\_UNORM\_SRGB)을 사용하여 5:6:5 색(5비트 빨강, 6비트 녹색, 5비트 파랑)으로 3분 색 데이터를 저장합니다. 데이터에 1비트 알파도 들어 있는 경우에도 마찬가지입니다. 가능한 최대 데이터 형식을 사용하는 4×4 텍스처의 경우 BC1 형식은 필요한 메모리를 48바이트(16가지 색 × 3분/색 × 1바이트/분)에서 8바이트로 줄입니다.

알고리즘은 4×4 블록의 텍셀에 작용합니다. 알고리즘은 다음 다이어그램과 같이 16가지 색을 저장하는 대신 2개의 참조 색(color\_0 및 color\_1)과 16개의 2비트 색 인덱스(블록 a–p)를 저장합니다.

![bc1 압축의 레이아웃](images/d3d10-compression-bc1.png)

색 인덱스(a–p)는 색상표에서 원래 색을 조회하는 데 사용됩니다. 색상표에는 4가지 색이 있습니다. 처음 2개의 색인 color\_0과 color\_1은 최소 색과 최대 색입니다. 다른 2개의 색인 color\_2와 color\_3은 선형 보간으로 계산된 중간 색입니다.

```
color_2 = 2/3*color_0 + 1/3*color_1
color_3 = 1/3*color_0 + 2/3*color_1
```

4가지 색에는 블록 a–p에 저장되는 2비트 인덱스 값이 할당됩니다.

```
color_0 = 00
color_1 = 01
color_2 = 10
color_3 = 11
```

마지막으로 블록 a–p의 각 색이 색상표의 4가지 색과 비교되고 가장 가까운 색에 대한 인덱스가 2비트 블록에 저장됩니다.

이 알고리즘은 1비트 알파도 들어 있는 데이터에도 적용됩니다. 유일한 차이점은 color\_3은 투명 색을 나타내는 0으로 설정되고 color\_2는 color\_0과 color\_1의 선형 혼합이라는 점입니다.

```
color_2 = 1/2*color_0 + 1/2*color_1;
color_3 = 0;
```

### <a name="span-idbc2spanspan-idbc2spanbc2"></a><span id="BC2"></span><span id="bc2"></span>BC2

BC2 형식(DXGI\_FORMAT\_BC2\_TYPELESS, DXGI\_FORMAT\_BC2\_UNORM 또는 DXGI\_BC2\_UNORM\_SRGB)을 사용하여 색과 일관성이 낮은 알파 데이터가 포함된 데이터를 저장합니다(일관성이 높은 알파 데이터의 경우 [BC3](#bc3) 사용). BC2 형식은 RGB 데이터를 5:6:5 색(5비트 빨강, 6비트 녹색, 5비트 파랑)으로 저장하고 알파를 별도의 4비트 값으로 저장합니다. 가능한 최대 데이터 형식을 사용하는 4×4 텍스처의 경우 이 압축 기술은 필요한 메모리를 64바이트(16가지 색 × 4분/색 × 1바이트/분)에서 16바이트로 줄입니다.

BC2 형식은 동일한 비트 수와 데이터 레이아웃의 색을 [BC1](#bc1) 형식으로 저장합니다. 그러나 BC2에는 다음 다이어그램과 같이 알파 데이터를 저장하기 위한 64비트의 메모리가 추가로 필요합니다.

![bc2 압축의 레이아웃](images/d3d10-compression-bc2.png)

### <a name="span-idbc3spanspan-idbc3spanbc3"></a><span id="BC3"></span><span id="bc3"></span>BC3

BC3 형식(DXGI\_FORMAT\_BC3\_TYPELESS, DXGI\_FORMAT\_BC3\_UNORM 또는 DXGI\_BC3\_UNORM\_SRGB)을 사용하여 일관성이 높은 색 데이터를 저장합니다(일관성이 낮은 알파 데이터의 경우 [BC2](#bc2) 사용). BC3 형식은 5:6:5 색(5비트 빨강, 6비트 녹색, 5비트 파랑)을 사용하여 색 데이터를 저장하고 1바이트를 사용하여 알파 데이터를 저장합니다. 가능한 최대 데이터 형식을 사용하는 4×4 텍스처의 경우 이 압축 기술은 필요한 메모리를 64바이트(16가지 색 × 4분/색 × 1바이트/분)에서 16바이트로 줄입니다.

BC3 형식은 동일한 비트 수와 데이터 레이아웃의 색을 [BC1](#bc1) 형식으로 저장합니다. 그러나 BC3에는 알파 데이터를 저장하기 위한 64비트의 메모리가 추가로 필요합니다. BC3 형식은 2개의 참조 값을 저장하고 이 둘을 보간하여 알파를 처리합니다(BC1이 RGB 색을 저장하는 방식과 비슷).

알고리즘은 4×4 블록의 텍셀에 작용합니다. 알고리즘은 다음 다이어그램과 같이 16개의 알파 값을 저장하는 대신 2개의 참조 알파(alpha\_0 및 alpha\_1)과 16개의 3비트 색 인덱스(알파 a–p)를 저장합니다.

![bc3 압축의 레이아웃](images/d3d10-compression-bc3.png)

BC3 형식은 알파 인덱스(a–p)를 사용하여 8개의 값이 들어 있는 조회 테이블에서 원래 색을 조회합니다. 처음 2개의 값인 alpha\_0과 alpha\_1은 최소 색과 최대 색입니다. 다른 6개의 중간 값은 선형 보간을 사용하여 계산됩니다.

알고리즘은 2개의 참조 알파 값을 검사하여 보간된 알파 값의 수를 결정합니다. alpha\_0이 alpha\_1보다 크면 BC3이 6개의 알파 값을 보간하고, 그렇지 않으면 4개의 알파 값을 보간합니다. BC3이 4개의 알파 값만 보간하는 경우 2개의 추가 알파 값을 설정합니다(완전 투명을 나타내는 0과 완전 불투명을 나타내는 255). BC3은 지정된 텍셀에 대한 원래 알파와 가장 일치하는 보간된 알파 값에 해당하는 비트 코드를 저장하여 4×4 텍셀 영역의 알파 값을 압축합니다.

```
if( alpha_0 > alpha_1 )
{
  // 6 interpolated alpha values.
  alpha_2 = 6/7*alpha_0 + 1/7*alpha_1; // bit code 010
  alpha_3 = 5/7*alpha_0 + 2/7*alpha_1; // bit code 011
  alpha_4 = 4/7*alpha_0 + 3/7*alpha_1; // bit code 100
  alpha_5 = 3/7*alpha_0 + 4/7*alpha_1; // bit code 101
  alpha_6 = 2/7*alpha_0 + 5/7*alpha_1; // bit code 110
  alpha_7 = 1/7*alpha_0 + 6/7*alpha_1; // bit code 111
}
else
{
  // 4 interpolated alpha values.
  alpha_2 = 4/5*alpha_0 + 1/5*alpha_1; // bit code 010
  alpha_3 = 3/5*alpha_0 + 2/5*alpha_1; // bit code 011
  alpha_4 = 2/5*alpha_0 + 3/5*alpha_1; // bit code 100
  alpha_5 = 1/5*alpha_0 + 4/5*alpha_1; // bit code 101
  alpha_6 = 0;                         // bit code 110
  alpha_7 = 255;                       // bit code 111
}
```

### <a name="span-idbc4spanspan-idbc4spanbc4"></a><span id="BC4"></span><span id="bc4"></span>BC4

BC4 형식으로 각 색에 대해 8비트를 사용하여 1분 색 데이터를 저장합니다. BC4는 [BC1](#bc1)보다 정확하므로 DXGI\_FORMAT\_BC4\_UNORM 형식을 사용하여 \[0 ~ 1\] 범위에 부동 소수점 데이터를 저장하고 DXGI\_FORMAT\_BC4\_SNORM 형식을 사용하여 \[-1 ~ +1\] 범위에 부동 소수점 데이터를 저장하는 데 이상적입니다. 가능한 최대 데이터 형식을 사용하는 4×4 텍스처의 경우 이 압축 기술은 필요한 메모리를 16바이트(16가지 색 × 1분/색 × 1바이트/분)에서 8바이트로 줄입니다.

알고리즘은 4×4 블록의 텍셀에 작용합니다. 알고리즘은 다음 다이어그램과 같이 16가지 색을 저장하는 대신 2개의 참조 색(red\_0 및 red\_1)과 16개의 3비트 색 인덱스(빨강 a–p)를 저장합니다.

![bc4 압축의 레이아웃](images/d3d10-compression-bc4.png)

알고리즘에서 3비트 인덱스를 사용하여 8가지 색이 들어 있는 색상표에서 색을 조회합니다. 처음 2개의 색인 red\_0과 red\_1은 최소 색과 최대 색입니다. 알고리즘에서 선형 보간을 사용하여 나머지 색을 계산합니다.

알고리즘에서 2개의 참조 값을 검사하여 보간된 색 값의 수를 결정합니다. red\_0이 red\_1보다 크면 BC4가 6개의 색 값을 보간하고, 그렇지 않으면 4개의 색 값을 보간합니다. BC4가 4개의 색 값만 보간하는 경우 2개의 추가 색 값을 설정합니다(완전 투명을 나타내는 0.0f와 완전 불투명을 나타내는 1.0f). BC4는 지정된 텍셀에 대한 원래 알파와 가장 일치하는 보간된 알파 값에 해당하는 비트 코드를 저장하여 4×4 텍셀 영역의 알파 값을 압축합니다.

-   [BC4\_UNORM](#bc4-unorm)
-   [BC4\_SNORM](#bc4-snorm)

### <a name="span-idbc4unormspanspan-idbc4unormspanspan-idbc4-unormspanbc4unorm"></a><span id="BC4_UNORM"></span><span id="bc4_unorm"></span><span id="bc4-unorm"></span>BC4\_UNORM

다음 코드 샘플과 같이 1분 데이터의 보간이 수행됩니다.

```
unsigned word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = 0.0f;                     // bit code 110
  red_7 = 1.0f;                     // bit code 111
}
```

참조 색에 3비트 인덱스(8개의 값이 있으므로 000–111)가 할당되며 이들은 압축 중 빨강 a-p 블록에 저장됩니다.

### <a name="span-idbc4snormspanspan-idbc4snormspanspan-idbc4-snormspanbc4snorm"></a><span id="BC4_SNORM"></span><span id="bc4_snorm"></span><span id="bc4-snorm"></span>BC4\_SNORM

DXGI\_FORMAT\_BC4\_SNORM은 똑같습니다. 단, 데이터가 SNORM 범위에서 인코딩되고 4개의 색 값이 보간된다는 점이 다릅니다. 다음 코드 샘플과 같이 1분 데이터의 보간이 수행됩니다.

```
signed word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = -1.0f;                     // bit code 110
  red_7 =  1.0f;                     // bit code 111
}
```

참조 색에 3비트 인덱스(8개의 값이 있으므로 000–111)가 할당되며 이들은 압축 중 빨강 a-p 블록에 저장됩니다.

### <a name="span-idbc5spanspan-idbc5spanbc5"></a><span id="BC5"></span><span id="bc5"></span>BC5

BC5 형식으로 각 색에 대해 8비트를 사용하여 2분 색 데이터를 저장합니다. BC5는 [BC1](#bc1)보다 정확하므로 DXGI\_FORMAT\_BC5\_UNORM 형식을 사용하여 \[0 ~ 1\] 범위에 부동 소수점 데이터를 저장하고 DXGI\_FORMAT\_BC5\_SNORM 형식을 사용하여 \[-1 ~ +1\] 범위에 부동 소수점 데이터를 저장하는 데 이상적입니다. 가능한 최대 데이터 형식을 사용하는 4×4 텍스처의 경우 이 압축 기술은 필요한 메모리를 32바이트(16가지 색 × 2분/색 × 1바이트/분)에서 16바이트로 줄입니다.

-   [BC5\_UNORM](#bc5-unorm)
-   [BC5\_SNORM](#bc5-snorm)

알고리즘은 4×4 블록의 텍셀에 작용합니다. 다음 다이어그램과 같이 알고리즘은 2분 모두에 대해 16개의 색을 저장하는 각 분(red\_0, red\_1, green\_0 및 green\_1)에 대해 2개의 참조 색을 저장하고 각 분(빨강 a-p 및 녹색 a-p)에 대해 16개의 3비트 색 인덱스를 저장합니다.

![bc5 압축의 레이아웃](images/d3d10-compression-bc5.png)

알고리즘에서 3비트 인덱스를 사용하여 8가지 색이 들어 있는 색상표에서 색을 조회합니다. 처음 2개의 색인 red\_0과 red\_1(또는 green\_0과 green\_1)은 최소 색과 최대 색입니다. 알고리즘에서 선형 보간을 사용하여 나머지 색을 계산합니다.

알고리즘에서 2개의 참조 값을 검사하여 보간된 색 값의 수를 결정합니다. red\_0이 red\_1보다 크면 BC5가 6개의 색 값을 보간하고, 그렇지 않으면 4개의 색 값을 보간합니다. BC5가 4개의 색 값만 보간하는 경우 0.0f와 1.0f에 나머지 2개의 색 값을 설정합니다.

### <a name="span-idbc5unormspanspan-idbc5unormspanspan-idbc5-unormspanbc5unorm"></a><span id="BC5_UNORM"></span><span id="bc5_unorm"></span><span id="bc5-unorm"></span>BC5\_UNORM

다음 코드 샘플과 같이 1분 데이터의 보간이 수행됩니다. 녹색 분에 대한 계산은 비슷합니다.

```
unsigned word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = 0.0f;                     // bit code 110
  red_7 = 1.0f;                     // bit code 111
}
```

참조 색에 3비트 인덱스(8개의 값이 있으므로 000–111)가 할당되며 이들은 압축 중 빨강 a-p 블록에 저장됩니다.

### <a name="span-idbc5snormspanspan-idbc5snormspanspan-idbc5-snormspanbc5snorm"></a><span id="BC5_SNORM"></span><span id="bc5_snorm"></span><span id="bc5-snorm"></span>BC5\_SNORM

DXGI\_FORMAT\_BC5\_SNORM은 똑같습니다. 단, 데이터가 SNORM 범위에서 인코딩되며 4개의 데이터 값이 보간될 때 2개의 추가 값이 -1.0f와 1.0f라는 점이 다릅니다. 다음 코드 샘플과 같이 1분 데이터의 보간이 수행됩니다. 녹색 분에 대한 계산은 비슷합니다.

```
signed word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = -1.0f;                    // bit code 110
  red_7 =  1.0f;                    // bit code 111
}
```

참조 색에 3비트 인덱스(8개의 값이 있으므로 000–111)가 할당되며 이들은 압축 중 빨강 a-p 블록에 저장됩니다.

## <a name="span-iddifferencesspanspan-iddifferencesspanspan-iddifferencesspanformat-conversion"></a><span id="Differences"></span><span id="differences"></span><span id="DIFFERENCES"></span>형식 변환


Direct3D는 동일한 비트 너비의 선구조화된 형식의 텍스처와 블록이 압축된 텍스처 간의 복사를 가능하게 합니다.

몇 가지 형식 유형 간에 리소스를 복사할 수 있습니다. 이러한 유형의 복사 작업은 리소스 데이터를 다른 형식 유형으로 재해석하는 일종의 형식 변환을 수행합니다. 데이터 재해석과 더 일반적인 유형의 변환 작동 방식 간의 차이를 보여 주는 이 예제를 살펴보세요.

```
FLOAT32 f = 1.0f;
UINT32 u;
```

'f'을 'u' 유형으로 재해석하려면 [memcpy](http://msdn.microsoft.com/library/dswaw1wk.aspx)를 사용합니다.

```
memcpy( &u, &f, sizeof( f ) ); // 'u' becomes equal to 0x3F800000.
```

위의 재해석에서 데이터의 기본 값은 변경되지 않습니다. [memcpy](http://msdn.microsoft.com/library/dswaw1wk.aspx)는 부동 소수점을 부호 없는 정수로 재해석합니다.

더 일반적인 유형의 변환을 수행하려면 할당을 사용합니다.

```
u = f; // 'u' becomes 1.
```

위의 변환에서는 데이터의 기본 값이 변경됩니다.

다음 표에는 이러한 재해석 유형의 형식 변환에 사용할 수 있는 소스 및 대상 형식이 나와 있습니다. 재해석이 예상대로 작동하려면 값을 적절히 인코딩해야 합니다.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">비트 너비</th>
<th align="left">압축되지 않은 리소스</th>
<th align="left">블록이 압축된 리소스</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">32</td>
<td align="left"><p>DXGI_FORMAT_R32_UINT</p>
<p>DXGI_FORMAT_R32_SINT</p></td>
<td align="left">DXGI_FORMAT_R9G9B9E5_SHAREDEXP</td>
</tr>
<tr class="even">
<td align="left">64</td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UINT</p>
<p>DXGI_FORMAT_R16G16B16A16_SINT</p>
<p>DXGI_FORMAT_R32G32_UINT</p>
<p>DXGI_FORMAT_R32G32_SINT</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM[_SRGB]</p>
<p>DXGI_FORMAT_BC4_UNORM</p>
<p>DXGI_FORMAT_BC4_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left">128</td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_UINT</p>
<p>DXGI_FORMAT_R32G32B32A32_SINT</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM[_SRGB]</p>
<p>DXGI_FORMAT_BC3_UNORM[_SRGB]</p>
<p>DXGI_FORMAT_BC5_UNORM</p>
<p>DXGI_FORMAT_BC5_SNORM</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[압축된 텍스처 리소스](compressed-texture-resources.md)
