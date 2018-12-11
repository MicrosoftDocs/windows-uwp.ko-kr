---
title: CBV(상수 버퍼 뷰)
description: 상수 버퍼에는 셰이더 상수 데이터가 포함됩니다. 이러한 데이터의 가치는 데이터 변경이 필요할 때까지 데이터가 유지되고 모든 GPU 셰이더에서 데이터에 액세스할 수 있다는 점입니다.
ms.assetid: 99AEC6B0-A43B-4B61-8C3A-ECC8DE1B69A7
keywords:
- CBV(상수 버퍼 뷰)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 33e850ba16be7a8d2621f061015d39c8b334cab2
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8888200"
---
# <a name="constant-buffer-view-cbv"></a>CBV(상수 버퍼 뷰)


상수 버퍼에는 셰이더 상수 데이터가 포함됩니다. 이러한 데이터의 가치는 데이터 변경이 필요할 때까지 데이터가 유지되고 모든 GPU 셰이더에서 데이터에 액세스할 수 있다는 점입니다.

상수 버퍼의 일반적인 데이터는 한 프레임의 그리기 전체에서 일정하게 유지되는 세계 좌표, 프로젝션 및 뷰 매트릭스입니다.

상수 버퍼 레이아웃은 HLSL 레이아웃과 일치해야 합니다([Packing Rules for Constant Variables(상수 변수에 대한 압축 규칙)](https://msdn.microsoft.com/library/windows/desktop/bb509632.aspx) 참조).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[보기](views.md)

 

 




