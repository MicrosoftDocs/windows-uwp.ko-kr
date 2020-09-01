---
title: 블록 압축
description: 블록 압축은 질감 크기 및 메모리 사용 공간을 줄이고 성능을 향상 시킬 수 있는 손실 질감 압축 기술입니다. 블록 압축 질감은 색 당 32 비트의 질감 보다 작을 수 있습니다.
ms.assetid: 2FAD6BE8-C6E4-4112-AF97-419CD27F7C73
keywords:
- 블록 압축
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 02b65357b52740e9b56e0e2dc11ff22723825f20
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156457"
---
# <a name="block-compression"></a>블록 압축

블록 압축은 질감 크기 및 메모리 사용 공간을 줄이고 성능을 향상 시킬 수 있는 손실 질감 압축 기술입니다. 블록 압축 질감은 색 당 32 비트의 질감 보다 작을 수 있습니다.

블록 압축은 질감 크기를 줄이기 위한 질감 압축 기술입니다. 색 당 32 비트와 비교할 때 블록 압축 질감은 최대 75% 더 작을 수 있습니다. 응용 프로그램은 일반적으로 메모리 공간이 더 작으므로 블록 압축을 사용 하는 경우 성능이 향상 됩니다.

손실 되는 동안 블록 압축은 제대로 작동 하며 파이프라인에 의해 변환 되 고 필터링 되는 모든 질감에 권장 됩니다. 화면에 직접 매핑되는 질감 (아이콘 및 텍스트와 같은 UI 요소)은 아티팩트가 더 눈에 띄는 압축을 선택 하는 데 적합 하지 않습니다.

블록 압축 질감은 모든 차원에서 크기가 4의 배수로 생성 되어야 하며 파이프라인의 출력으로 사용할 수 없습니다.

## <a name="span-idbasicsspanspan-idbasicsspanspan-idbasicsspanhow-block-compression-works"></a><span id="Basics"></span><span id="basics"></span><span id="BASICS"></span>블록 압축 작동 방법

블록 압축은 색 데이터를 저장 하는 데 필요한 메모리 양을 줄이는 기술입니다. 인코딩 체계를 사용 하 여 원래 크기 및 기타 색에 일부 색을 저장 하면 이미지를 저장 하는 데 필요한 메모리 양을 크게 줄일 수 있습니다. 하드웨어는 압축 된 데이터를 자동으로 디코딩 하므로 압축 된 질감 사용에 대 한 성능 저하가 없습니다.

압축의 작동 방식을 확인 하려면 다음 두 가지 예를 살펴보세요. 첫 번째 예제는 압축 되지 않은 데이터를 저장할 때 사용 되는 메모리의 양을 설명 합니다. 두 번째 예제에서는 압축 된 데이터를 저장할 때 사용 되는 메모리 양을 설명 합니다.

- [압축 되지 않은 데이터 저장](#storing-uncompressed-data)
- [압축 된 데이터 저장](#storing-compressed-data)

### <a name="span-idstoring_uncompressed_dataspanspan-idstoring_uncompressed_dataspanspan-idstoring_uncompressed_dataspanspan-idstoring-uncompressed-dataspanstoring-uncompressed-data"></a><span id="Storing_Uncompressed_Data"></span><span id="storing_uncompressed_data"></span><span id="STORING_UNCOMPRESSED_DATA"></span><span id="storing-uncompressed-data"></span>압축 되지 않은 데이터 저장

다음 그림은 압축 되지 않은 4 × 4 질감을 나타냅니다. 각 색에 단일 색 구성 요소가 포함 되어 있다고 가정 합니다 (예를 들어 빨간색).

![압축 되지 않은 4x4 질감](images/d3d10-block-compress-1.png)

압축 되지 않은 데이터는 순차적으로 메모리에 배치 되며 다음 그림에 표시 된 것 처럼 16 바이트가 필요 합니다.

![순차 메모리의 압축 되지 않은 데이터](images/d3d10-block-compress-2.png)

### <a name="span-idstoring_compressed_dataspanspan-idstoring_compressed_dataspanspan-idstoring_compressed_dataspanspan-idstoring-compressed-dataspanstoring-compressed-data"></a><span id="Storing_Compressed_Data"></span><span id="storing_compressed_data"></span><span id="STORING_COMPRESSED_DATA"></span><span id="storing-compressed-data"></span>압축 된 데이터 저장

압축 된 이미지에서 사용 하는 메모리의 양을 확인 했으므로 압축 된 이미지에서 저장 하는 메모리의 양을 확인 합니다. [BC1](#bc1) 압축 형식은 다음 그림과 같이 질감의 원래 색을 보간 하는 데 사용 되는 2 가지 색 (각각 1 바이트)과 16 3 비트 인덱스 (48 비트 또는 6 바이트)를 저장 합니다.

![bc1 압축 형식](images/d3d10-block-compress-3.png)

압축 된 데이터를 저장 하는 데 필요한 총 공간은 8 바이트 이며 압축 되지 않은 예제에 비해 50%의 메모리를 절약할 수 있습니다. 두 개 이상의 색 구성 요소가 사용 되는 경우에도 절약이 훨씬 더 커집니다.

블록 압축을 통해 제공 되는 상당한 메모리 절약으로 인해 성능이 향상 될 수 있습니다. 이 성능은 색 보간으로 인 한 이미지 품질의 비용으로 제공 됩니다. 그러나 낮은 품질이 종종 눈에 띄는 것은 아닙니다.

다음 섹션에서는 Direct3D에서 응용 프로그램의 블록 압축을 사용 하도록 설정 하는 방법을 보여 줍니다.

## <a name="span-idusing_block_compressionspanspan-idusing_block_compressionspanspan-idusing_block_compressionspanusing-block-compression"></a><span id="Using_Block_Compression"></span><span id="using_block_compression"></span><span id="USING_BLOCK_COMPRESSION"></span>블록 압축 사용

블록 압축 형식을 지정 하는 것을 제외 하 고 압축 되지 않은 질감 처럼 블록 압축 된 질감을 만듭니다.

다음으로, 블록 압축 된 질감이 셰이더 단계에 대 한 입력 으로만 사용 될 수 있으므로 셰이더 리소스 뷰를 만들려고 하기 때문에 질감을 파이프라인에 바인딩하는 뷰를 만듭니다.

압축 되지 않은 질감을 사용 하는 것과 동일한 방식으로 블록 압축 질감을 사용 합니다. 응용 프로그램에서 블록 압축 된 데이터에 대 한 메모리 포인터를 가져오는 경우에는 선언 된 크기와 실제 크기의 차이를 초래 하는 밉 맵의 메모리 안쪽 여백을 고려해 야 합니다.

- [가상 크기와 실제 크기 비교](#virtual-size-versus-physical-size)

### <a name="span-idvirtual_sizespanspan-idvirtual_sizespanspan-idvirtual_sizespanspan-idvirtual-size-versus-physical-sizespanvirtual-size-versus-physical-size"></a><span id="Virtual_Size"></span><span id="virtual_size"></span><span id="VIRTUAL_SIZE"></span><span id="virtual-size-versus-physical-size"></span>가상 크기와 실제 크기 비교

메모리 포인터를 사용 하 여 블록 압축 질감의 메모리를 탐색 하는 응용 프로그램 코드가 있는 경우 응용 프로그램 코드를 수정 해야 할 수 있는 중요 한 고려 사항이 하나 있습니다. 블록 압축 알고리즘이 4x4 텍셀 블록에서 작동 하기 때문에 블록 압축 질감은 모든 차원에서 4의 배수 여야 합니다. 이는 초기 차원이 4로 나눌 수 있는 밉 맵에 문제가 될 수 있지만,이 경우에는 세분화 수준이 아닙니다. 다음 다이어그램에서는 가상 (선언 된) 크기와 각 밉 맵 수준의 실제 (실제) 크기 사이의 영역 차이를 보여 줍니다.

![압축 되지 않은 압축 및 압축 된 밉 맵 수준](images/d3d10-block-compress-pad.png)

다이어그램의 왼쪽에는 압축 되지 않은 60 × 40 질감에 대해 생성 되는 밉 맵 수준 크기가 표시 됩니다. 최상위 수준 크기는 질감을 생성 하는 API 호출에서 가져옵니다. 각 후속 수준은 이전 수준 크기의 절반입니다. 압축 되지 않은 질감의 경우 가상 (선언 된) 크기와 실제 (실제) 크기 간에 차이가 없습니다.

다이어그램의 오른쪽에는 압축을 사용 하는 동일한 60 × 40 질감에 대해 생성 되는 밉 맵 수준 크기가 표시 됩니다. 두 번째와 세 번째 수준에는 모든 수준에서 크기 계수 4를 만드는 데 사용 되는 메모리 패딩이 있습니다. 이는 알고리즘이 4 × 4 텍셀 블록에서 작동할 수 있도록 하기 위해 필요 합니다. 이는 특히 4 × 4 보다 작은 밉 맵 수준을 고려 하는 경우 분명 합니다. 이러한 매우 작은 밉 맵 수준의 크기는 텍스처 메모리가 할당 될 때 가장 가까운 4 개의 요소로 반올림 됩니다.

샘플링 하드웨어는 가상 크기를 사용 합니다. 질감이 샘플링 되 면 메모리 패딩은 무시 됩니다. 4 × 4 보다 작은 밉 맵 수준의 경우 처음 4 개의 텍셀는 2 × 2 맵에 사용 되 고 첫 번째 텍셀는 1 × 1 블록에서 사용 됩니다. 그러나 실제 크기 (메모리 패딩 포함)를 노출 하는 API 구조는 없습니다.

요약 하자면 블록 압축 데이터가 포함 된 영역을 복사할 때 정렬 된 메모리 블록을 사용 해야 합니다. 메모리 포인터를 가져오는 응용 프로그램에서이 작업을 수행 하려면 포인터가 표면 피치를 사용 하 여 실제 메모리 크기를 고려 하는지 확인 합니다.

## <a name="span-idcompression_algorithmsspanspan-idcompression_algorithmsspanspan-idcompression_algorithmsspancompression-algorithms"></a><span id="Compression_Algorithms"></span><span id="compression_algorithms"></span><span id="COMPRESSION_ALGORITHMS"></span>압축 알고리즘

Direct3D의 블록 압축 기술은 압축 되지 않은 질감 데이터를 4 × 4 블록으로 분할 하 고 각 블록을 압축 한 다음 데이터를 저장 합니다. 이러한 이유로 질감이 압축 될 것으로 예상 되는 질감 차원은 4의 배수 여야 합니다.

![압축 차단](images/d3d10-compression-1.png)

위의 다이어그램에서는 텍셀 블록으로 분할 된 질감을 보여 줍니다. 첫 번째 블록은 a-p 라는 레이블이 지정 된 16 텍셀의 레이아웃을 표시 하지만 모든 블록에는 동일한 데이터 구성이 있습니다.

Direct3D는 여러 가지 압축 체계를 구현 하며, 각 압축 체계는 저장 된 구성 요소 수, 구성 요소별 비트 수 및 사용 된 메모리 양에 따라 서로 다른 균형을 구현 합니다. 이 표를 사용 하 여 데이터 형식 및 응용 프로그램에 가장 적합 한 데이터 확인에서 가장 잘 작동 하는 형식을 선택할 수 있습니다.

| 원본 데이터                     | 데이터 압축 확인 (비트) | 이 압축 형식 선택 |
|---------------------------------|---------------------------------------|--------------------------------|
| 3 구성 요소 색 및 알파 | 색 (5:6:5), 알파 (1) 또는 알파 없음  | [BC1](#bc1)                    |
| 3 구성 요소 색 및 알파 | 색 (5:6:5), 알파 (4)              | [BC2](#bc2)                    |
| 3 구성 요소 색 및 알파 | 색 (5:6:5), 알파 (8)              | [BC3](#bc3)                    |
| 단일 구성 요소 색             | 한 구성 요소 (8)                     | [BC4](#bc4)                    |
| 2 구성 요소 색             | 두 구성 요소 (8:8)                  | [BC5](#bc5)                    |

- [BC1](#bc1)
- [BC2](#bc2)
- [BC3](#bc3)
- [BC4](#bc4)
- [BC5](#bc5)

### <a name="span-idbc1spanspan-idbc1spanbc1"></a><span id="BC1"></span><span id="bc1"></span>BC1

\_ \_ \_ \_ \_ \_ \_ \_ \_ 5:6:5 색 (5 비트 빨강, 6 비트 녹색, 5 비트 파랑)을 사용 하 여 3 개 구성 요소 색 데이터를 저장 하려면 첫 번째 블록 압축 형식 (BC1) (dxgi 형식 BC1 형식 없음, dxgi 형식 BC1 unorm 또는 dxgi BC1 unorm SRGB)을 사용 합니다. 이는 데이터가 1 비트 알파도 포함 하는 경우에도 마찬가지입니다. 가능한 가장 큰 데이터 형식을 사용 하 여 4 × 4 개의 질감이 있다고 가정할 때 BC1 형식은 48 바이트 (16 개 색 x 3 구성 요소/색 × 1 바이트/구성 요소)에서 필요한 메모리를 8 바이트의 메모리로 줄입니다.

이 알고리즘은 텍셀 4 × 4 블록에서 작동 합니다. 16 색을 저장 하는 대신 다음 다이어그램에 표시 된 것 처럼 알고리즘은 2 가지 참조 색 (색 \_ 0 및 색 \_ 1)과 16 2 비트 색 인덱스 (– p 블록)를 저장 합니다.

![bc1 압축에 대 한 레이아웃](images/d3d10-compression-bc1.png)

색 인덱스 (a – p)는 색 테이블에서 원래 색을 조회 하는 데 사용 됩니다. 색 테이블에 4 가지 색이 있습니다. 처음 두 색 (색 \_ 0 및 색 \_ 1)은 최소 및 최대 색입니다. 색 2 및 색 3의 다른 두 색은 \_ \_ 선형 보간을 사용 하 여 계산 되는 중간 색입니다.

```cpp
color_2 = 2/3*color_0 + 1/3*color_1
color_3 = 1/3*color_0 + 2/3*color_1
```

4 개 색에는 – p 블록에 저장 되는 2 비트 인덱스 값이 할당 됩니다.

```cpp
color_0 = 00
color_1 = 01
color_2 = 10
color_3 = 11
```

마지막으로 블록의 각 색은 – p를 색 테이블의 4 색과 비교 하 고 가장 가까운 색의 인덱스는 2 비트 블록에 저장 됩니다.

이 알고리즘은 1 비트 알파도 포함 하는 데이터에 적합 합니다. 유일한 차이점은 색 \_ 3이 투명 색을 나타내는 0으로 설정 되 고 색 \_ 2는 색 \_ 0 및 색 1의 선형 blend입니다 \_ .

```cpp
color_2 = 1/2*color_0 + 1/2*color_1;
color_3 = 0;
```

### <a name="span-idbc2spanspan-idbc2spanbc2"></a><span id="BC2"></span><span id="bc2"></span>BC2

BC2 형식 (DXGI \_ 형식 \_ BC2 \_ , dxgi \_ 형식 \_ BC2 \_ unorm 또는 dxgi \_ BC2 \_ unorm \_ SRGB)을 사용 하 여 일관성이 낮은 색 및 알파 데이터를 포함 하는 데이터를 저장 합니다 (매우 일관 된 알파 데이터의 경우 [BC3](#bc3) 사용). BC2 형식은 RGB 데이터를 5:6:5 색 (5 비트 빨강, 6 비트 녹색, 5 비트 파랑)으로 저장 하 고 알파를 별도의 4 비트 값으로 저장 합니다. 가능한 가장 큰 데이터 형식을 사용 하는 4 × 4 개의 질감이 있다고 가정 하면이 압축 기술은 64 바이트 (16 개 색 × 4 개 구성 요소/색 × 1 바이트/구성 요소)에서 16 바이트의 메모리로 필요한 메모리를 줄입니다.

BC2 형식은 동일한 수의 비트 및 데이터 레이아웃을 사용 하 여 [BC1](#bc1) 형식으로 색을 저장 합니다. 그러나 BC2에는 다음 다이어그램에 표시 된 것 처럼 알파 데이터를 저장 하기 위한 추가 64-비트 메모리가 필요 합니다.

![bc2 압축에 대 한 레이아웃](images/d3d10-compression-bc2.png)

### <a name="span-idbc3spanspan-idbc3spanbc3"></a><span id="BC3"></span><span id="bc3"></span>BC3

BC3 형식 (DXGI \_ 형식 \_ BC3 \_ , dxgi \_ 형식 \_ BC3 \_ unorm 또는 dxgi \_ BC3 \_ unorm \_ SRGB)을 사용 하 여 매우 일관 된 색 데이터를 저장 합니다 (더 작은 알파 데이터로 [BC2](#bc2) 사용). BC3 형식은 1 바이트를 사용 하 여 5:6:5 색 (5 비트 빨강, 6 비트 녹색, 5 비트 파랑) 및 알파 데이터를 사용 하 여 색 데이터를 저장 합니다. 가능한 가장 큰 데이터 형식을 사용 하는 4 × 4 개의 질감이 있다고 가정 하면이 압축 기술은 64 바이트 (16 개 색 × 4 개 구성 요소/색 × 1 바이트/구성 요소)에서 16 바이트의 메모리로 필요한 메모리를 줄입니다.

BC3 형식은 동일한 수의 비트 및 데이터 레이아웃을 사용 하 여 [BC1](#bc1) 형식으로 색을 저장 합니다. 그러나 BC3에는 알파 데이터를 저장 하기 위한 추가 64 비트 메모리가 필요 합니다. BC3 형식은 두 개의 참조 값과 보간 사이에 두 개의 참조 값을 저장 하 여 알파를 처리 합니다 (BC1가 RGB 색을 저장 하는 방법과 유사).

이 알고리즘은 텍셀 4 × 4 블록에서 작동 합니다. 16 개의 알파 값을 저장 하는 대신 알고리즘은 \_ 다음 다이어그램에 표시 된 것 처럼 2 개의 참조 alphas (알파 0 및 알파 \_ 1) 및 16 3 비트 색 인덱스 (알파 a ~ p)를 저장 합니다.

![bc3 압축에 대 한 레이아웃](images/d3d10-compression-bc3.png)

BC3 형식은 알파 인덱스 (a – p)를 사용 하 여 8 개의 값이 포함 된 조회 테이블에서 원래 색을 조회 합니다. 첫 번째 두 값 (알파 \_ 0 및 알파 \_ 1)은 최소값과 최대값입니다. 다른 6 개의 중간 값은 선형 보간을 사용 하 여 계산 됩니다.

알고리즘은 두 참조 알파 값을 검사 하 여 보간된 알파 값의 수를 결정 합니다. 알파 \_ 0이 알파 1 보다 크면 \_ BC3는 6 개의 알파 값을 보간 하 고, 그렇지 않으면 4를 보간합니다. BC3가 4 개의 알파 값만 보간 하는 경우 두 개의 추가 알파 값을 설정 합니다 (완전히 투명 하 고 255 인 경우에는 완전히 불투명). BC3는 지정 된 텍셀의 원래 알파와 가장 가깝게 일치 하는 보간된 알파 값에 해당 하는 비트 코드를 저장 하 여 4 × 4 텍셀 영역의 알파 값을 압축 합니다.

```cpp
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

BC4 형식을 사용 하 여 각 색에 8 비트를 사용 하 여 단일 구성 요소 색 데이터를 저장 합니다. [BC1](#bc1)와 비교 하 여 정확도가 증가 함에 따라 BC4은 dxgi 형식 BC4 unorm 형식을 사용 하 여 0에서 1 사이의 부동 소수점 데이터를 저장 하는 데 이상적 이며, \[ \] \_ \_ \_ \[ \] dxgi \_ 형식 \_ BC4 snorm 형식을 사용 하 여-1에서 + 1까지 \_ 부동 소수점 데이터를 저장 하는 데 적합 합니다. 가능한 가장 큰 데이터 형식을 사용 하는 4 × 4 개의 질감이 있다고 가정 하면이 압축 기술은 16 바이트 (16 개 색 × 1 구성 요소/색 × 1 바이트/구성 요소)에서 8 바이트로 필요한 메모리를 줄입니다.

이 알고리즘은 텍셀 4 × 4 블록에서 작동 합니다. 16 색을 저장 하는 대신 다음 다이어그램에 표시 된 것 처럼 알고리즘은 2 가지 참조 색 (빨강 \_ 0 및 빨강 \_ 1)과 16 3 비트 색 인덱스 (빨강 a-빨강 p)를 저장 합니다.

![bc4 압축에 대 한 레이아웃](images/d3d10-compression-bc4.png)

알고리즘은 3 비트 인덱스를 사용 하 여 8 색을 포함 하는 색 테이블에서 색을 검색 합니다. 처음 두 색 (빨강 \_ 0 및 빨강 \_ 1)은 최소 및 최대 색입니다. 알고리즘은 선형 보간을 사용 하 여 나머지 색을 계산 합니다.

알고리즘은 두 참조 값을 검사 하 여 보간된 색 값의 수를 결정 합니다. 빨강 \_ 0이 빨강 1 보다 크면 \_ BC4는 6 개의 색 값을 보간 하 고, 그렇지 않으면 4를 보간합니다. BC4에서 4 개의 색 값만 보간 하는 경우 두 개의 추가 색 값 (완전히 투명 하 고 1.0 f에 대해 완전히 불투명 하 게 하려면 0.0 f)을 설정 합니다. BC4는 지정 된 텍셀의 원래 알파와 가장 가깝게 일치 하는 보간된 알파 값에 해당 하는 비트 코드를 저장 하 여 4 × 4 텍셀 영역의 알파 값을 압축 합니다.

- [BC4 \_ unorm](#bc4-unorm)
- [BC4 \_ snorm](#bc4-snorm)

### <a name="span-idbc4_unormspanspan-idbc4_unormspanspan-idbc4-unormspanbc4_unorm"></a><span id="BC4_UNORM"></span><span id="bc4_unorm"></span><span id="bc4-unorm"></span>BC4 \_ unorm

단일 구성 요소 데이터의 보간은 다음 코드 샘플과 같이 수행 됩니다.

```cpp
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

참조 색에는 3 비트 인덱스 (8 개의 값이 있으므로 000-111)가 할당 되 고 압축 중에 빨간색 a에서 빨간색 p까지 저장 됩니다.

### <a name="span-idbc4_snormspanspan-idbc4_snormspanspan-idbc4-snormspanbc4_snorm"></a><span id="BC4_SNORM"></span><span id="bc4_snorm"></span><span id="bc4-snorm"></span>BC4 \_ snorm

DXGI \_ 형식 \_ BC4 \_ snorm는 데이터를 serange로 인코딩하고 4 색 값이 보간된 경우를 제외 하 고는 정확히 동일 합니다. 단일 구성 요소 데이터의 보간은 다음 코드 샘플과 같이 수행 됩니다.

```cpp
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

참조 색에는 3 비트 인덱스 (8 개의 값이 있으므로 000-111)가 할당 되 고 압축 중에 빨간색 a에서 빨간색 p까지 저장 됩니다.

### <a name="span-idbc5spanspan-idbc5spanbc5"></a><span id="BC5"></span><span id="bc5"></span>BC5

BC5 형식을 사용 하 여 각 색에 8 비트를 사용 하 여 2 개의 구성 요소 색 데이터를 저장 합니다. [BC1](#bc1)와 비교 하 여 정확도가 증가 함에 따라 BC5은 dxgi 형식 BC5 unorm 형식을 사용 하 여 0에서 1 사이의 부동 소수점 데이터를 저장 하는 데 이상적 이며, \[ \] \_ \_ \_ \[ \] dxgi \_ 형식 \_ BC5 snorm 형식을 사용 하 여-1에서 + 1까지 \_ 부동 소수점 데이터를 저장 하는 데 적합 합니다. 가능한 가장 큰 데이터 형식을 사용 하는 4 × 4 개의 질감이 있다고 가정 하면이 압축 기술은 32 바이트 (16 개 색 x 2 구성 요소/색 × 1 바이트/구성 요소)에서 16 바이트로 필요한 메모리를 줄입니다.

- [BC5 \_ unorm](#bc5-unorm)
- [BC5 \_ snorm](#bc5-snorm)

이 알고리즘은 텍셀 4 × 4 블록에서 작동 합니다. 두 구성 요소에 대해 16 색을 저장 하는 대신 알고리즘은 각 구성 요소에 대 한 2 개의 참조 색 (빨강 \_ 0, 빨간색 \_ 1, 녹색 \_ 0 및 녹색 \_ 1)과 16 3 비트 색 인덱스 (각 구성 요소에 대해 빨강 a-빨강 p 및 녹색 a-녹색 p)를 저장 합니다. 여기에는 다음 다이어그램에 나와 있습니다.

![bc5 압축에 대 한 레이아웃](images/d3d10-compression-bc5.png)

알고리즘은 3 비트 인덱스를 사용 하 여 8 색을 포함 하는 색 테이블에서 색을 검색 합니다. 처음 두 색 (빨강 \_ 0 및 빨강 \_ 1 (또는 녹색 \_ 0 및 녹색 \_ 1))은 최소 및 최대 색입니다. 알고리즘은 선형 보간을 사용 하 여 나머지 색을 계산 합니다.

알고리즘은 두 참조 값을 검사 하 여 보간된 색 값의 수를 결정 합니다. 빨강 \_ 0이 빨강 1 보다 크면 \_ BC5는 6 개의 색 값을 보간 하 고, 그렇지 않으면 4를 보간합니다. BC5에서 4 개의 색 값만 보간 하면 나머지 두 색 값이 0.0 f와 1.0 f에 설정 됩니다.

### <a name="span-idbc5_unormspanspan-idbc5_unormspanspan-idbc5-unormspanbc5_unorm"></a><span id="BC5_UNORM"></span><span id="bc5_unorm"></span><span id="bc5-unorm"></span>BC5 \_ unorm

단일 구성 요소 데이터의 보간은 다음 코드 샘플과 같이 수행 됩니다. 녹색 구성 요소에 대 한 계산은 비슷합니다.

```cpp
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

참조 색에는 3 비트 인덱스 (8 개의 값이 있으므로 000-111)가 할당 되 고 압축 중에 빨간색 a에서 빨간색 p까지 저장 됩니다.

### <a name="span-idbc5_snormspanspan-idbc5_snormspanspan-idbc5-snormspanbc5_snorm"></a><span id="BC5_SNORM"></span><span id="bc5_snorm"></span><span id="bc5-snorm"></span>BC5 \_ snorm

\_ \_ \_ 데이터를 serange로 인코딩하고 4 개의 데이터 값이 보간된 경우 두 개의 추가 값이-1.0 f와 1.0 f 인 경우를 제외 하 고 DXGI FORMAT BC5 snorm는 정확히 동일 합니다. 단일 구성 요소 데이터의 보간은 다음 코드 샘플과 같이 수행 됩니다. 녹색 구성 요소에 대 한 계산은 비슷합니다.

```cpp
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

참조 색에는 3 비트 인덱스 (8 개의 값이 있으므로 000-111)가 할당 되 고 압축 중에 빨간색 a에서 빨간색 p까지 저장 됩니다.

## <a name="span-iddifferencesspanspan-iddifferencesspanspan-iddifferencesspanformat-conversion"></a><span id="Differences"></span><span id="differences"></span><span id="DIFFERENCES"></span>형식 변환

Direct3D는 동일한 비트 너비의 미리 구조화 된 질감 및 블록 압축 질감 간에 복사를 사용 하도록 설정 합니다.

몇 가지 형식 형식 간에 리소스를 복사할 수 있습니다. 이 유형의 복사 작업은 리소스 데이터를 다른 형식 유형으로 다시 해석 하는 형식 변환의 형식을 수행 합니다. Reinterpeting 데이터 간의 차이점을 보여 주는이 예제는 보다 일반적인 형식의 변환 동작을 보여 줍니다.

```cpp
FLOAT32 f = 1.0f;
UINT32 u;
```

' F '를 ' u '의 형식으로 다시 해석 하려면 [memcpy](/cpp/c-runtime-library/reference/memcpy-wmemcpy)을 사용 합니다.

```cpp
memcpy( &u, &f, sizeof( f ) ); // 'u' becomes equal to 0x3F800000.
```

앞의 재 해석에서 데이터의 기본 값이 변경 되지 않습니다. [memcpy](/cpp/c-runtime-library/reference/memcpy-wmemcpy) 은 float를 부호 없는 정수로 다시 해석 합니다.

보다 일반적인 변환 유형을 수행 하려면 할당을 사용 합니다.

```cpp
u = f; // 'u' becomes 1.
```

위의 변환에서 데이터의 기본 값이 변경 됩니다.

다음 표에서는이 재 해석 형식 변환에 사용할 수 있는 소스 및 대상 형식을 보여 줍니다. 다시 해석이 예상 대로 작동 하도록 값을 올바르게 인코딩해야 합니다.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">비트 너비</th>
<th align="left">압축 되지 않은 리소스</th>
<th align="left">블록 압축 리소스</th>
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
<td align="left"><p>DXGI_FORMAT_BC1_UNORM [_SRGB]</p>
<p>DXGI_FORMAT_BC4_UNORM</p>
<p>DXGI_FORMAT_BC4_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left">128</td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_UINT</p>
<p>DXGI_FORMAT_R32G32B32A32_SINT</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM [_SRGB]</p>
<p>DXGI_FORMAT_BC3_UNORM [_SRGB]</p>
<p>DXGI_FORMAT_BC5_UNORM</p>
<p>DXGI_FORMAT_BC5_SNORM</p></td>
</tr>
</tbody>
</table>

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목

[압축된 텍스처 리소스](compressed-texture-resources.md)