---
title: 텍스처 주소 지정 모드
description: Direct3D 응용 프로그램은 모든 기본 객체의 꼭짓점에 텍스처 좌표를 지정할 수 있습니다.
ms.assetid: 925E8F2E-43EC-404E-8870-03E39155F697
keywords:
- 텍스처 주소 지정 모드
- 텍스처 주소 지정 모드 래핑
- 미러 텍스처 주소 모드
- 클램프 텍스처 주소 모드
- 테두리 색 텍스처 주소 모드
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5e263876f414e5683ffc8a5645a12e5031b3d6fb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660188"
---
# <a name="texture-addressing-modes"></a>텍스처 주소 지정 모드


Direct3D 응용 프로그램은 모든 기본 객체의 꼭짓점에 텍스처 좌표를 지정할 수 있습니다. 일반적으로 꼭짓점에 할당되는 u 및 v-텍스처 좌표는 0.0~1.0(0.0과 1.0 포함)의 범위 내에 있습니다. 하지만 범위 외부에 텍스처 좌표를 지정할 경우 특수한 텍스처 효과를 생성할 수 있습니다. .

외부에 있는 질감 좌표를 사용 하 여 Direct3D 용도 제어 합니다 \[0.0, 1.0\] 질감 주소 지정 모드를 설정 하 여 범위입니다. 예를 들어 응용 프로그램에서 텍스처가 기본 객체에 타일링되도록 텍스처 주소 지정 모드를 설정할 수 있습니다.

Direct3D는 응용 프로그램에서 텍스처 래핑이 가능합니다. [텍스처 래핑](texture-wrapping.md)을 참조하세요.

벗어나는 텍스처 좌표를 사용 하면 효과적으로 래핑 질감을 사용 하도록 설정 합니다 \[0.0, 1.0\] , 잘못 된 범위 및 동작 래스터화에 대 한 이러한 연체 질감 좌표 정의 되지 않습니다 여기서 합니다. 텍스처 래핑을 사용하면 텍스처 주소 지정 모드는 사용하지 못합니다 따라서 텍스처 래핑을 사용할 때는 응용 프로그램이 텍스처 좌표를 0.0보다 낮게, 혹은 1.0보다 높게 지정하지 않도록 주의해야 합니다.

## <a name="span-idsummaryofthetextureaddressingmodesspanspan-idsummaryofthetextureaddressingmodesspanspan-idsummaryofthetextureaddressingmodesspansummary-of-the-texture-addressing-modes"></a><span id="Summary_of_the_texture_addressing_modes"></span><span id="summary_of_the_texture_addressing_modes"></span><span id="SUMMARY_OF_THE_TEXTURE_ADDRESSING_MODES"></span>주소 지정 모드 질감의 요약


| 텍스처 주소 지정 모드 | 설명                                                                                                                           |
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| 랩                    | 모든 정수 교차점에서 텍스처를 반복합니다.                                                                                        |
| Mirror                  | 모든 정수 테두리에서 텍스처를 미러링합니다.                                                                                        |
| 클램프                   | 질감 좌표를 clamps 합니다 \[0.0, 1.0\] 범위; 제한 모드 텍스처를 한 번 적용 후 묻어남 가장자리 픽셀의 색입니다. |
| 테두리 색            | 0.0~1.0(0.0과 1.0 포함)의 범위를 벗어나는 모든 텍스처 좌표에 임의의 *테두리 색*을 사용합니다.                         |

 

## <a name="span-idwraptextureaddressmodespanspan-idwraptextureaddressmodespanspan-idwraptextureaddressmodespanwrap-texture-address-mode"></a><span id="Wrap_texture_address_mode"></span><span id="wrap_texture_address_mode"></span><span id="WRAP_TEXTURE_ADDRESS_MODE"></span>랩 질감 주소 모드


랩 텍스처 주소 모드에서는 Direct3D가 모든 정수 교차점에서 텍스처를 반복합니다.

예를 들어 응용 프로그램이 기본 사각형을 생성한 다음 (0.0,0.0), (0.0,3.0), (3.0,3.0) 및 (3.0,0.0)의 텍스처 좌표를 지정한다고 가정하겠습니다. 이때 텍스처 주소 지정 모드를 "랩"으로 설정하면 다음 그림과 같이 텍스처가 u 및 v 방향으로 3회 적용됩니다.

![u 및 v 방향으로 래핑된 얼굴 텍스처 그림](images/wrap.png)

이것을 다음 **미러 텍스처 주소 모드**와 비교하세요.

## <a name="span-idmirrortextureaddressmodespanspan-idmirrortextureaddressmodespanspan-idmirrortextureaddressmodespanmirror-texture-address-mode"></a><span id="Mirror_texture_address_mode"></span><span id="mirror_texture_address_mode"></span><span id="MIRROR_TEXTURE_ADDRESS_MODE"></span>미러 질감 주소 모드


미러 텍스처 주소 모드에서는 Direct3D가 모든 정수 테두리에서 텍스처를 미러링합니다.

예를 들어 응용 프로그램이 기본 사각형을 생성한 다음 (0.0,0.0), (0.0,3.0), (3.0,3.0) 및 (3.0,0.0)의 텍스처 좌표를 지정한다고 가정하겠습니다. 이때 텍스처 주소 지정 모드를 "미러"로 설정하면 텍스처가 u 및 v 방향으로 3회 적용됩니다. 그 밖에 텍스처가 적용되는 모든 행과 열은 다음 그림과 같이 이전 행 또는 열의 미러 이미지입니다.

![3x3 그리드의 미러 이미지 그림](images/mirror.png)

이것을 이전 **랩 텍스처 주소 모드**와 비교하세요.

## <a name="span-idclamptextureaddressmodespanspan-idclamptextureaddressmodespanspan-idclamptextureaddressmodespanclamp-texture-address-mode"></a><span id="Clamp_texture_address_mode"></span><span id="clamp_texture_address_mode"></span><span id="CLAMP_TEXTURE_ADDRESS_MODE"></span>Clamp 질감 주소 모드


제한 질감 주소 모드에 따라 Direct3D 질감 좌표를 고정 합니다 \[0.0, 1.0\] 범위; 제한 모드 텍스처를 한 번 적용 후 묻어남 가장자리 픽셀의 색입니다.

예를 들어 응용 프로그램이 기본 사각형을 생성한 다음 텍스처 좌표 (0.0,0.0), (0.0,3.0), (3.0,3.0) 및 (3.0,0.0)을 기본 사각형의 꼭짓점에 지정한다고 가정하겠습니다. 이때 텍스처 주소 지정 모드를 "클램프"로 설정하면 텍스처가 한 번 적용됩니다. 열 상단과 행 끝의 픽셀 색상은 각각 기본 사각형의 상단과 오른쪽으로 확장됩니다.

다음은 클램프 텍스처를 나타낸 그림입니다.

![텍스처 및 클램프 텍스처 그림](images/clamp.png)

## <a name="span-idbordercolortextureaddressmodespanspan-idbordercolortextureaddressmodespanspan-idbordercolortextureaddressmodespanborder-color-texture-address-mode"></a><span id="Border_Color_texture_address_mode"></span><span id="border_color_texture_address_mode"></span><span id="BORDER_COLOR_TEXTURE_ADDRESS_MODE"></span>테두리 색 질감 주소 모드


테두리 색 텍스처 주소 모드에서는 Direct3D가 0.0~1.0(0.0과 1.0 포함)의 범위를 벗어나는 모든 텍스처 좌표에 *테두리 색*이라고 하는 임의의 색상을 사용합니다.

다음 그림에서는 응용 프로그램이 빨간색 테두리를 사용하여 텍스처가 기본 객체에 적용되도록 지정하고 있습니다.

![텍스처와 빨간색 테두리를 사용한 텍스처 그림](images/border.png)

## <a name="span-iddevicelimitationsspanspan-iddevicelimitationsspanspan-iddevicelimitationsspandevice-limitations"></a><span id="Device_Limitations"></span><span id="device_limitations"></span><span id="DEVICE_LIMITATIONS"></span>장치 제한 사항


일반적으로 시스템이 0.0~1.0(0.0과 1.0 포함)의 범위를 벗어나는 텍스처 좌표를 허용하기는 하지만 하드웨어 제한으로 인해 범위를 벗어날 수 있는 텍스처 좌표에도 한계가 있습니다. 렌더링 장치에서 여러 가지 기능을 살펴보면 허용되는 텍스트 좌표의 전체 범위 한계가 표시됩니다.

예를 들어 한계 값이 128이면 입력되는 텍스처 좌표가 -128.0~+128.0의 범위를 벗어나서는 안 됩니다. 이 범위를 벗어나는 텍스트 좌표의 꼭짓점을 전달하면 무효 처리됩니다. 자동 텍스처 좌표 생성 및 텍스처 좌표 변환에 따라 생성된 텍스처 좌표에도 동일한 제한이 따릅니다.

텍스처 반복 제한은 텍스처 좌표에서 인덱싱되는 텍스처 크기에 따라 다릅니다. 이 경우에서 텍스처 크기가 32이고, 텍스처 좌표의 허용 범위가 512라면 실제로 유효한 텍스처 좌표 범위는 512/32 = 16이 됩니다. 따라서 이 장치의 텍스처 범위는 -16.0~+16.0을 벗어나서는 안 됩니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련된 항목


[질감](textures.md)

 

 




