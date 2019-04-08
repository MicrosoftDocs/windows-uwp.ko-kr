---
title: 텍스처 블록 압축
description: Direct3D 11에서 텍스처에 대한 블록 압축(BC) 지원 기능이 확장되면서 이제 BC6H 및 BC7 알고리즘도 지원합니다.
ms.assetid: 63506C46-BF14-464B-B20C-8B8F359E7AFE
keywords:
- 텍스처 블록 압축
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: dec33768eff90b9bd35a3ea60f3158fce663345e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640188"
---
# <a name="texture-block-compression"></a>텍스처 블록 압축


Direct3D 11에서 텍스처에 대한 블록 압축(BC) 지원 기능이 확장되면서 이제 BC6H 및 BC7 알고리즘도 지원합니다. BC6H는 High Dynamic Range 색 원본 데이터를 지원하고, BC7은 표준 RGB 원본 데이터에 대한 더 적은 아티팩트로 평균보다 나은 품질의 압축을 제공합니다.

BC1부터 BC5 형식에 대한 지원을 포함하여 Direct3D 11 이전의 블록 압축 알고리즘 지원에 대한 자세한 내용은 [블록 압축(Direct3D 10)](https://msdn.microsoft.com/library/windows/desktop/bb694531)을 참조하세요.

**파일 형식에 대 한 정보:  ** 압축 된 질감 데이터 저장을 위해 DDS 파일 형식을 사용 하는 BC6H 및 BC7 질감 압축 형식입니다. 자세한 내용은 [DDS 프로그래밍 가이드](https://msdn.microsoft.com/library/windows/desktop/bb943991)를 참조하세요.

## <a name="span-idblockcompressionformatssupportedindirect3d11spanspan-idblockcompressionformatssupportedindirect3d11spanspan-idblockcompressionformatssupportedindirect3d11spanblock-compression-formats-supported-in-direct3d-11"></a><span id="Block_Compression_Formats_Supported_in_Direct3D_11"></span><span id="block_compression_formats_supported_in_direct3d_11"></span><span id="BLOCK_COMPRESSION_FORMATS_SUPPORTED_IN_DIRECT3D_11"></span>Direct3D에서 지원 되는 압축 형식을 차단 11


| 원본 데이터                                  | 필요한 최소 데이터 압축 해상도                              | 권장 형식 | 지원되는 최소 특성 레벨 |
|----------------------------------------------|---------------------------------------------------------------------------|--------------------|---------------------------------|
| 3채널 색상(알파 채널 포함)       | 3개 색상 채널(5비트:6비트:5비트), 0 또는 1비트의 알파 포함  | BC1                | Direct3D 9.1                    |
| 3채널 색상(알파 채널 포함)       | 3개 색상 채널(5비트:6비트:5비트), 4비트의 알파 포함         | BC2                | Direct3D 9.1                    |
| 3채널 색상(알파 채널 포함)       | 3개 색상 채널(5비트:6비트:5비트), 8비트의 알파 포함          | BC3                | Direct3D 9.1                    |
| 1채널 색상                            | 1개 색상 채널(8비트)                                                | BC4                | Direct3D 10                     |
| 2채널 색상                            | 2개 색상 채널(8비트:8비트)                                        | BC5                | Direct3D 10                     |
| 3채널 HDR(High Dynamic Range) 색상 | "중간" 부동 소수점의 세 가지 색 채널 (16 비트: 16 비트: 16 비트)\* | BC6H               | Direct3D 11                     |
| 3채널 색상(알파 채널은 옵션)  | 3개 색상 채널(채널당 4~7비트), 0~8비트의 알파 포함  | BC7                | Direct3D 11                     |

 

\*"중간" 부동 소수점이 선택적 부호 비트를 구성 하는 16 비트 값, 지 수 및 10 또는 11 비트가 수는 5 비트 편향.
## <a name="span-idbc1bc2andb3formatsspanspan-idbc1bc2andb3formatsspanspan-idbc1bc2andb3formatsspanbc1-bc2-and-b3-formats"></a><span id="BC1__BC2__and_B3_Formats"></span><span id="bc1__bc2__and_b3_formats"></span><span id="BC1__BC2__AND_B3_FORMATS"></span>BC1, BC2 및 B3 형식


The BC1, BC2 및 BC3 형식은 Direct3D 9 DXTn 텍스처 압축 형식과 동등하며, 해당하는 Direct3D 10 BC1, BC2 및 BC3 형식과도 동일합니다. 이러한 세 가지 형식에 대 한 지원이 필요한 모든 기능 수준 (D3D\_기능\_수준\_9\_1, D3D\_기능\_수준\_9\_2, D3D\_ 기능\_수준\_9\_3, D3D\_기능\_수준\_10\_0 D3D\_기능\_수준\_10 \_1 및 D3D\_기능\_수준\_11\_0).

| 블록 압축 형식 | DXGI 형식                                                                           | Direct3D 9에 준하는 형식                               | 4x4 픽셀 블록당 바이트 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------------------------------------|---------------------------|
| BC1                      | DXGI\_형식\_BC1\_UNORM, DXGI\_형식\_BC1\_UNORM\_SRGB, DXGI\_형식\_BC1\_TYPELESS | D3DFMT\_DXT1, FourCC "DXT1" =                                | 8                         |
| BC2                      | DXGI\_형식\_BC2\_UNORM, DXGI\_형식\_BC2\_UNORM\_SRGB, DXGI\_형식\_BC2\_TYPELESS | D3DFMT\_DXT2\*, FourCC D3DFMT "DXT2" =\_DXT3, FourCC "DXT3" = | 16                        |
| BC3                      | DXGI\_형식\_BC3\_UNORM, DXGI\_형식\_BC3\_UNORM\_SRGB, DXGI\_형식\_BC3\_TYPELESS | D3DFMT\_DXT4\*, FourCC D3DFMT "DXT4" =\_DXT5, FourCC "DXT5" = | 16                        |

 

\*이러한 압축 체계 (DXT2 및 DXT4) Direct3D 9 미리 곱한 알파 형식 및 표준 알파 형식 구별 하지 확인합니다. 따라서 이러한 구분은 렌더링 시 프로그래밍이 가능한 셰이더를 이용해 처리해야 합니다.

## <a name="span-idbc4andbc5formatsspanspan-idbc4andbc5formatsspanspan-idbc4andbc5formatsspanbc4-and-bc5-formats"></a><span id="BC4_and_BC5_Formats"></span><span id="bc4_and_bc5_formats"></span><span id="BC4_AND_BC5_FORMATS"></span>BC4 및 BC5 형식


| 블록 압축 형식 | DXGI 형식                                                                     | Direct3D 9에 준하는 형식 | 4x4 픽셀 블록당 바이트 |
|--------------------------|---------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC4                      | DXGI\_형식\_BC4\_UNORM, DXGI\_형식\_BC4\_SNORM, DXGI\_형식\_BC4\_TYPELESS | FourCC="ATI1"                | 8                         |
| BC5                      | DXGI\_형식\_BC5\_UNORM, DXGI\_형식\_BC5\_SNORM, DXGI\_형식\_BC5\_TYPELESS | FourCC="ATI2"                | 16                        |

 

## <a name="span-idbc6hformatspanspan-idbc6hformatspanspan-idbc6hformatspanbc6h-format"></a><span id="BC6H_Format"></span><span id="bc6h_format"></span><span id="BC6H_FORMAT"></span>BC6H 형식


이 형식에 대한 자세한 내용은 [BC6H 형식](https://msdn.microsoft.com/library/windows/desktop/hh308952) 설명서를 참조하세요.

| 블록 압축 형식 | DXGI 형식                                                                      | Direct3D 9에 준하는 형식 | 4x4 픽셀 블록당 바이트 |
|--------------------------|----------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC6H                     | DXGI\_형식\_BC6H\_UF16, DXGI\_형식\_BC6H\_SF16, DXGI\_형식\_BC6H\_TYPELESS | 해당 없음                          | 16                        |

 

BC6H 형식은 4x4 픽셀 블록마다 다른 인코딩 모드를 선택할 수 있습니다. 총 14개의 인코딩 모드를 사용할 수 있으며, 각각 표시되는 텍스처의 시각적 품질이 약간씩 다릅니다. 이렇게 모드를 사용하면 원본 내용에 따라 품질 수준을 다르게 선택하거나 적용하여 하드웨어의 디코딩 속도를 빠르게 할 수 있지만 검색 공간의 복잡성이 매우 커지기도 합니다.

## <a name="span-idbc7formatspanspan-idbc7formatspanspan-idbc7formatspanbc7-format"></a><span id="BC7_Format"></span><span id="bc7_format"></span><span id="BC7_FORMAT"></span>BC7 형식


이 형식에 대한 자세한 내용은 [BC7 형식](https://msdn.microsoft.com/library/windows/desktop/hh308953) 설명서를 참조하세요.

| 블록 압축 형식 | DXGI 형식                                                                           | Direct3D 9에 준하는 형식 | 4x4 픽셀 블록당 바이트 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC7                      | DXGI\_형식\_BC7\_UNORM, DXGI\_형식\_BC7\_UNORM\_SRGB, DXGI\_형식\_BC7\_TYPELESS | 해당 없음                          | 16                        |

 

BC7 형식은 4x4 픽셀 블록마다 다른 인코딩 모드를 선택할 수 있습니다. 총 8개의 인코딩 모드를 사용할 수 있으며, 각각 표시되는 텍스처의 시각적 품질이 약간씩 다릅니다. 이렇게 모드를 사용하면 원본 내용에 따라 품질 수준을 다르게 선택하거나 적용하여 하드웨어의 디코딩 속도를 빠르게 할 수 있지만 검색 공간의 복잡성이 매우 커지기도 합니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련된 항목


[부록](appendix.md)

[질감](https://msdn.microsoft.com/library/windows/desktop/ff476902)

 

 




