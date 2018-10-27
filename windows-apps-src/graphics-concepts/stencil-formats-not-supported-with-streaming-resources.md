---
title: 스트리밍 리소스에서 지원되지 않는 스텐실 형식
description: 스텐실을 포함하는 형식은 스트리밍 리소스에서 지원되지 않습니다.
ms.assetid: 90A572A4-3C76-4795-BAE9-FCC72B5F07AD
keywords:
- 스트리밍 리소스에서 지원되지 않는 스텐실 형식
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d10478d76be88138d1382793bd8b5e0d5b3ef41a
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5710159"
---
# <a name="stencil-formats-not-supported-with-streaming-resources"></a>스트리밍 리소스에서 지원되지 않는 스텐실 형식


스텐실을 포함하는 형식은 스트리밍 리소스에서 지원되지 않습니다.

스텐실을 포함하는 형식으로는 DXGI\_FORMAT\_D24\_UNORM\_S8\_UINT(및 R24G8 패밀리의 관련 형식)과 DXGI\_FORMAT\_D32\_FLOAT\_S8X24\_UINT(및 R32G8X24 패밀리의 관련 형식)이 포함됩니다.

일부 구현에서는 깊이 및 스텐실을 개별 할당에 저장하고 다른 구현에서는 이들을 함께 저장합니다. 두 방식의 타일 관리는 달라야 하며, 그 차이를 추상화 또는 합리화할 수 있는 단일 API는 없습니다. Microsoft에서는 향후 하드웨어가 각각 독립적으로 타일링된 개별 깊이 및 스텐실 표면을 지원할 것을 권장합니다.

32비트 깊이는 128x128개의 타일을 가지며 8비트 스텐실은 256 x 256개 타일을 가질 것입니다. 따라서 응용 프로그램이 깊이와 스텐실 간 타일 형태 불일치를 수용해야 합니다. 하지만 이미 다른 렌더링 대상 표면 형식에 동일한 문제가 존재합니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스 프로세스 간 및 장치 공유](streaming-resource-cross-process-and-device-sharing.md)

 

 




