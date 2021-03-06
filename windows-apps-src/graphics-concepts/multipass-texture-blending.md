---
title: 멀티패스 텍스처 혼합
description: Direct3D 응용 프로그램은 여러 렌더링 패스가 진행되는 동안 원형에 다양한 텍스처를 적용하여 수많은 특수 효과를 얻을 수 있습니다.
ms.assetid: FB4D6E3F-4EF5-4D20-BF7E-1008E790E30C
keywords:
- 멀티패스 텍스처 혼합
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d6b1e8958874ede50a18f2d2446c8f156361210e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589898"
---
# <a name="multipass-texture-blending"></a>멀티패스 텍스처 혼합


Direct3D 응용 프로그램은 여러 렌더링 패스가 진행되는 동안 원형에 다양한 텍스처를 적용하여 수많은 특수 효과를 얻을 수 있습니다. 여기서 사용하는 공통 용어는 *멀티패스 텍스처 혼합*입니다. 멀티패스 텍스처 혼합의 대표적인 용도는 몇 가지의 텍스처로부터 다수의 색을 적용하여 복잡한 조명 및 음영 모델의 효과를 에뮬레이트하는 것입니다. *조명 매핑*도 여기에 속합니다. [텍스처와 조명 매핑](light-mapping-with-textures.md)을 참조하세요.

**참고**    일부 장치는 단일 패스에서 기본 형식에 여러 질감을 적용할 수 있습니다. [텍스처 혼합](texture-blending.md)을 참조하세요.

 

사용자의 하드웨어에서 다중 텍스처 혼합을 지원하지 않는 경우, 응용 프로그램은 멀티패스 텍스처 혼합을 사용하여 동일한 시각 효과를 얻을 수 있습니다. 그러나 응용 프로그램은 다중 텍스처 혼합을 사용할 때는 가능한 프레임 속도를 유지할 수 없습니다.

C/C++ 응용 프로그램에서 멀티패스 텍스처 혼합을 수행하려면 다음과 같이 합니다.

1.  텍스처 단계 0에서 텍스처를 설정합니다.
2.  원하는 색과 알파 혼합 인수 및 작업을 선택합니다. 기본 설정은 멀티패스 텍스처 혼합에 적합합니다.
3.  장면에서 적절한 개체를 렌더링합니다.
4.  텍스처 단계 0에서 다음 텍스처를 설정합니다.
5.  필요에 따라 원본 및 대상 혼합 요인을 조정하기 위하여 렌더링 상태를 설정합니다. 시스템은 이 매개 변수에 따라 렌더링 대상 표면의 기존 픽셀을 새 텍스처와 혼합합니다.
6.  필요한 만큼의 텍스처로 3, 4, 5단계를 반복합니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련된 항목


[질감 혼합](texture-blending.md)

 

 




