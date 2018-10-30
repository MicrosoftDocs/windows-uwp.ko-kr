---
title: 인덱스 버퍼
description: 인덱스 버퍼는 인덱스 데이터를 포함한 메모리 버퍼입니다. 이 데이터는 원형 렌더링에 사용되는 꼭짓점 버퍼에 대한 정수 오프셋입니다.
ms.assetid: 14D3DEC5-CF74-488B-BE41-16BF5E3201BE
keywords:
- 인덱스 버퍼
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0df56ebeefdbdabe5904547d77e90077549422c2
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5766635"
---
# <a name="index-buffers"></a>인덱스 버퍼


*인덱스 버퍼*는 인덱스 데이터를 포함하는 메모리 버퍼로써 기본 요소를 렌더링하는 데 사용되는 버텍스 버퍼에 대한 정수 오프셋입니다.

인덱스 버퍼는 인덱스 데이터를 포함하는 메모리 버퍼입니다. 인덱스 데이터 또는 인덱스는 꼭짓점 버퍼에 대한 정수 오프셋이며 원형 렌더링에 사용됩니다.

꼭짓점 버퍼에는 꼭짓점이 포함되므로 인덱싱된 원형을 이용하거나 이용하지 않고 꼭짓점 버퍼를 그릴 수 있습니다. 하지만 인덱스 버퍼에는 인덱스가 포함되므로 해당하는 꼭짓점 버퍼 없이 인덱스 버퍼를 사용할 수 없습니다.

## <a name="span-idindexbufferdescriptionspanspan-idindexbufferdescriptionspanspan-idindexbufferdescriptionspanindex-buffer-description"></a><span id="Index_Buffer_Description"></span><span id="index_buffer_description"></span><span id="INDEX_BUFFER_DESCRIPTION"></span>인덱스 버퍼 설명


인덱스 버퍼는 메모리 내에서의 위치, 읽기 및 쓰기 지원 여부, 버퍼가 포함할 수 있는 인덱스의 유형과 개수 등 기능의 관점에서 설명됩니다.

인덱스 버퍼 설명은 기존 버퍼의 생성 과정에 대한 정보를 응용 프로그램에 제공합니다. 이전에 생성한 인덱스 버퍼의 기능으로 채우려면 시스템에 빈 설명 구조를 제공합니다.

## <a name="span-idindexprocessingrequirementsspanspan-idindexprocessingrequirementsspanspan-idindexprocessingrequirementsspanindex-processing-requirements"></a><span id="Index_Processing_Requirements"></span><span id="index_processing_requirements"></span><span id="INDEX_PROCESSING_REQUIREMENTS"></span>인덱스 처리 요구 사항


인덱스 처리 작업의 성능은 인덱스 버퍼가 메모리의 어느 위치에 있는지, 그리고 어떤 종류의 렌더링 장치가 사용되는지에 크게 좌우됩니다. 응용 프로그램은 인덱스 버퍼가 만들어질 때 이에 대한 메모리 할당을 제어합니다.

응용 프로그램은 드라이버 최적 메모리에 할당된 인덱스 버퍼에 직접 인덱스를 작성할 수 있습니다. 이 기술은 나중에 중복 사본 작업이 발생하는 것을 방지합니다. 응용 프로그램이 인덱스 버퍼에서 데이터를 다시 읽는 경우, 드라이버 최적 메모리의 호스트에서 수행하는 읽기 작업이 매우 느릴 수 있기 때문에 이 기술은 제대로 작동하지 않습니다. 따라서 응용 프로그램이 처리 중에 데이터를 읽거나 버퍼에 이상하게 데이터를 써야 하는 경우, 시스템 메모리 인덱스 버퍼가 더 낫습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[꼭짓점 및 인덱스 버퍼](vertex-and-index-buffers.md)

 

 




