---
title: CBV(상수 버퍼 보기)
description: 상수 버퍼는 셰이더 상수 데이터를 포함 합니다. 값은 데이터가 지속 되는 것 이며 데이터를 변경 해야 할 때까지 GPU 셰이더에 의해 액세스 될 수 있습니다.
ms.assetid: 99AEC6B0-A43B-4B61-8C3A-ECC8DE1B69A7
keywords:
- CBV(상수 버퍼 보기)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 50b590ab931f60b67ecd527629b681c9ffe63f71
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162747"
---
# <a name="constant-buffer-view-cbv"></a>CBV(상수 버퍼 보기)


상수 버퍼는 셰이더 상수 데이터를 포함 합니다. 값은 데이터가 지속 되는 것 이며 데이터를 변경 해야 할 때까지 GPU 셰이더에 의해 액세스 될 수 있습니다.

상수 버퍼에 대 한 일반적인 데이터는 세계, 프로젝션 및 뷰 매트릭스가 며 한 프레임의 그리기 전체에서 일정 하 게 유지 됩니다.

상수 버퍼 레이아웃은 HLSL 레이아웃과 일치 해야 합니다 ( [상수 변수에 대 한 압축 규칙](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-packing-rules)참조).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[Views](views.md)

 

 