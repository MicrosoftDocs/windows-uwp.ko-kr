---
title: 텍스처 블록 압축
description: BC6H 및 BC7 알고리즘을 포함 하도록 Direct3D 11에서 질감에 대 한 블록 압축 (BC) 지원이 확장 되었습니다.
ms.assetid: 63506C46-BF14-464B-B20C-8B8F359E7AFE
keywords:
- 텍스처 블록 압축
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 06c132f23d22dc688f82ff7976bcb93108f2ee1c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158817"
---
# <a name="texture-block-compression"></a>텍스처 블록 압축


BC6H 및 BC7 알고리즘을 포함 하도록 Direct3D 11에서 질감에 대 한 블록 압축 (BC) 지원이 확장 되었습니다. BC6H는 높은 수준의 동적 범위 색 원본 데이터를 지원 하 고, BC7는 표준 RGB 원본 데이터에 대해 더 작은 아티팩트를 사용 하 여 보다 우수한 평균 품질 압축을 제공 합니다.

BC1를 통한 BC5 형식 지원을 비롯 하 여 Direct3D 11 이전의 블록 압축 알고리즘 지원에 대 한 자세한 내용은 [차단 압축 (Direct3D 10)](/windows/desktop/direct3d10/d3d10-graphics-programming-guide-resources-block-compression)을 참조 하세요.

**파일 형식에 대 한 참고 사항:  ** BC6H 및 BC7 질감 압축 형식은 DDS 파일 형식을 사용 하 여 압축 된 질감 데이터를 저장 합니다. 자세한 내용은 [DDS의 프로그래밍 가이드](/windows/desktop/direct3ddds/dx-graphics-dds-pguide) 에서 자세한 내용을 참조 하세요.

## <a name="span-idblock_compression_formats_supported_in_direct3d_11spanspan-idblock_compression_formats_supported_in_direct3d_11spanspan-idblock_compression_formats_supported_in_direct3d_11spanblock-compression-formats-supported-in-direct3d-11"></a><span id="Block_Compression_Formats_Supported_in_Direct3D_11"></span><span id="block_compression_formats_supported_in_direct3d_11"></span><span id="BLOCK_COMPRESSION_FORMATS_SUPPORTED_IN_DIRECT3D_11"></span>Direct3D 11에서 지원 되는 압축 형식 차단


| 원본 데이터                                  | 필요한 최소 데이터 압축 확인                              | 권장 형식 | 지원 되는 최소 기능 수준 |
|----------------------------------------------|---------------------------------------------------------------------------|--------------------|---------------------------------|
| 알파 채널이 있는 3 채널 색       | 3 색 채널 (5 비트: 6 비트: 5 비트), 0 또는 1 비트 (알파)  | BC1                | Direct3D 9.1                    |
| 알파 채널이 있는 3 채널 색       | 4 비트 알파의 3 색 채널 (5 비트: 6 비트: 5 비트)         | BC2                | Direct3D 9.1                    |
| 알파 채널이 있는 3 채널 색       | 알파 8 비트의 3 색 채널 (5 비트: 6 비트: 5 비트)          | BC3                | Direct3D 9.1                    |
| 1 채널 색                            | 1 색 채널 (8 비트)                                                | BC4                | Direct3D 10                     |
| 2 채널 색                            | 두 색 채널 (8 비트: 8 비트)                                        | BC5                | Direct3D 10                     |
| 3 채널 HDR (high dynamic range) 색 | "절반" 부동 소수점의 3 색 채널 (16 비트: 16 비트: 16 비트)\* | BC6H               | Direct3D 11                     |
| 3 채널 색, 알파 채널 선택 사항  | 알파의 0 ~ 8 비트를 사용 하는 3 색 채널 (채널당 4 ~ 7 비트)  | BC7                | Direct3D 11                     |

 

\*"절반" 부동 소수점은 선택적 부호 비트, 5 비트 편향 지 수 및 10 또는 11 비트가 수로 구성 된 16 비트 값입니다.
## <a name="span-idbc1__bc2__and_b3_formatsspanspan-idbc1__bc2__and_b3_formatsspanspan-idbc1__bc2__and_b3_formatsspanbc1-bc2-and-b3-formats"></a><span id="BC1__BC2__and_B3_Formats"></span><span id="bc1__bc2__and_b3_formats"></span><span id="BC1__BC2__AND_B3_FORMATS"></span>BC1, BC2 및 B3 형식


BC1, BC2 및 BC3 형식은 Direct3D 9 DXTn 질감 압축 형식과 같으며 해당 Direct3D 10 BC1, BC2 및 BC3 형식과 동일 합니다. 모든 기능 수준 (D3D \_ 기능 \_ 수준 \_ 9 \_ 1, d3d 기능 수준 \_ \_ \_ 9 \_ 2, d3d \_ 기능 \_ 수준 \_ 9 \_ 3, D3D \_ 기능 \_ 수준 \_ 10 \_ 0, d3d \_ 기능 \_ 수준 \_ 10 \_ 1 및 d3d \_ 기능 \_ 수준 \_ 11 \_ 0)에는 이러한 세 가지 형식에 대 한 지원이 필요 합니다.

| 블록 압축 형식 | DXGI 형식                                                                           | Direct3D 9 해당 형식                               | 4x4 픽셀 블록 당 바이트 수 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------------------------------------|---------------------------|
| BC1                      | DXGI \_ format \_ BC1 \_ UNORM, dxgi \_ format \_ BC1 \_ unorm \_ SRGB, DXGI \_ 형식 \_ BC1 \_ 관대 한 형식 | D3DFMT \_ DXT1, FourCC = "DXT1"                                | 8                         |
| BC2                      | DXGI \_ format \_ BC2 \_ UNORM, dxgi \_ format \_ BC2 \_ unorm \_ SRGB, DXGI \_ 형식 \_ BC2 \_ 관대 한 형식 | D3DFMT \_ DXT2 \* , FourCC = "DXT2", D3DFMT \_ DXT3, FOURCC = "DXT3" | 16                        |
| BC3                      | DXGI \_ format \_ BC3 \_ UNORM, dxgi \_ format \_ BC3 \_ unorm \_ SRGB, DXGI \_ 형식 \_ BC3 \_ 관대 한 형식 | D3DFMT \_ DXT4 \* , FourCC = "DXT4", D3DFMT \_ DXT5, FOURCC = "DXT5" | 16                        |

 

\*이러한 압축 체계 (DXT2 및 DXT4)는 Direct3D 9 프리 곱하기 알파 형식과 표준 알파 형식을 구분 하지 않습니다. 이러한 구분은 렌더링 시 프로그래밍 가능한 셰이더를 통해 처리 되어야 합니다.

## <a name="span-idbc4_and_bc5_formatsspanspan-idbc4_and_bc5_formatsspanspan-idbc4_and_bc5_formatsspanbc4-and-bc5-formats"></a><span id="BC4_and_BC5_Formats"></span><span id="bc4_and_bc5_formats"></span><span id="BC4_AND_BC5_FORMATS"></span>BC4 및 BC5 형식


| 블록 압축 형식 | DXGI 형식                                                                     | Direct3D 9 해당 형식 | 4x4 픽셀 블록 당 바이트 수 |
|--------------------------|---------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC4                      | DXGI \_ format \_ BC4 \_ UNORM, dxgi \_ format \_ BC4 \_ snorm, dxgi \_ format \_ BC4 \_ 관대 | FourCC = "ATI1"                | 8                         |
| BC5                      | DXGI \_ format \_ BC5 \_ UNORM, dxgi \_ format \_ BC5 \_ snorm, dxgi \_ format \_ BC5 \_ 관대 | FourCC = "ATI2"                | 16                        |

 

## <a name="span-idbc6h_formatspanspan-idbc6h_formatspanspan-idbc6h_formatspanbc6h-format"></a><span id="BC6H_Format"></span><span id="bc6h_format"></span><span id="BC6H_FORMAT"></span>BC6H 형식


이 형식에 대 한 자세한 내용은 [BC6H format](/windows/desktop/direct3d11/bc6h-format) 설명서를 참조 하세요.

| 블록 압축 형식 | DXGI 형식                                                                      | Direct3D 9 해당 형식 | 4x4 픽셀 블록 당 바이트 수 |
|--------------------------|----------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC6H                     | DXGI \_ format \_ BC6H \_ UF16, dxgi \_ FORMAT \_ BC6H \_ SF16, dxgi \_ format \_ BC6H \_ 관대 한 형식 | 해당 없음                          | 16                        |

 

BC6H 형식은 각 4x4 픽셀 블록에 대해 서로 다른 인코딩 모드를 선택할 수 있습니다. 표시 되는 질감의 결과 시각적 품질에 약간 다른 장단점이 있는 총 14 개의 인코딩 모드를 사용할 수 있습니다. 모드를 선택 하면 원본 콘텐츠에 따라 품질 수준이 선택 되거나 적용 되는 하드웨어에의 한 빠른 디코딩에 가능 하지만 검색 공간의 복잡성이 크게 향상 됩니다.

## <a name="span-idbc7_formatspanspan-idbc7_formatspanspan-idbc7_formatspanbc7-format"></a><span id="BC7_Format"></span><span id="bc7_format"></span><span id="BC7_FORMAT"></span>BC7 형식


이 형식에 대 한 자세한 내용은 [BC7 format](/windows/desktop/direct3d11/bc7-format) 설명서를 참조 하세요.

| 블록 압축 형식 | DXGI 형식                                                                           | Direct3D 9 해당 형식 | 4x4 픽셀 블록 당 바이트 수 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC7                      | DXGI \_ format \_ BC7 \_ UNORM, dxgi \_ format \_ BC7 \_ unorm \_ SRGB, DXGI \_ 형식 \_ BC7 \_ 관대 한 형식 | 해당 없음                          | 16                        |

 

BC7 형식은 각 4x4 픽셀 블록에 대해 서로 다른 인코딩 모드를 선택할 수 있습니다. 표시 되는 질감의 결과 시각적 품질에 약간 다른 장단점이 있는 총 8 개의 인코딩 모드를 사용할 수 있습니다. 모드를 선택 하면 원본 콘텐츠에 따라 품질 수준이 선택 되거나 적용 되는 하드웨어에의 한 빠른 디코딩에 가능 하지만 검색 공간의 복잡성이 크게 향상 됩니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[부록](appendix.md)

[질감](/windows/desktop/direct3d11/overviews-direct3d-11-resources-textures)

 

 