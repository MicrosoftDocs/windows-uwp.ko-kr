---
title: 텍스처 리소스
description: Direct3D 텍스처 리소스를 사용 하 여 렌더링 하는 방법과 질감 단계를 사용 하 여 다중 질감 혼합을 지 원하는 방법을 알아봅니다.
ms.assetid: 016F6CDA-D361-4E6B-BA99-49E915A19364
keywords:
- 텍스처 리소스
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d82a5525601c98812d6aab97f5f5d4399ceddc91
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156187"
---
# <a name="texture-resources"></a>텍스처 리소스


질감은 렌더링에 사용 되는 리소스의 유형입니다.

## <a name="span-idrendering_with_texture_resourcesspanspan-idrendering_with_texture_resourcesspanspan-idrendering_with_texture_resourcesspanrendering-with-texture-resources"></a><span id="Rendering_with_Texture_Resources"></span><span id="rendering_with_texture_resources"></span><span id="RENDERING_WITH_TEXTURE_RESOURCES"></span>질감 리소스를 사용 하 여 렌더링


Direct3D에서는 질감 단계의 개념을 통해 다중 질감 혼합을 지원 합니다. 각 질감 단계에는 질감에 대해 수행할 수 있는 질감 및 작업이 포함 됩니다. 질감 단계의 질감이 현재 질감 집합을 형성 합니다. [질감 혼합](texture-blending.md)을 참조 하세요. 각 질감의 상태는 질감 단계에서 캡슐화 됩니다.

응용 프로그램에서 질감 큐브 뷰 및 질감 필터링 상태를 설정할 수도 있습니다. [질감 필터링](texture-filtering.md)을 참조 하세요.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[질감](textures.md)

 

 




