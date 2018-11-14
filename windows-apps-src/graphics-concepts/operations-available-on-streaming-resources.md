---
title: 스트리밍 리소스에서 할 수 있는 작업
description: 이 섹션에서는 스트리밍 리소스에서 할 수 있는 작업이 무엇인지 설명합니다.
ms.assetid: 700D8C54-0E20-4B2B-BEA3-20F6F72B8E24
keywords:
- 스트리밍 리소스에서 할 수 있는 작업
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1289b01e7ffb780c7e3faa52585eb5f002cf519c
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6163282"
---
# <a name="operations-available-on-streaming-resources"></a>스트리밍 리소스에서 할 수 있는 작업


이 섹션에서는 스트리밍 리소스에서 할 수 있는 작업이 무엇인지 설명합니다.

-   void를 반환하는 타일 매핑 업데이트 작업과 void를 반환하는 타일 매핑 복사 작업 - 이 작업들은 스트리밍 리소스의 타일 위치를 타일 풀의 위치나 NULL 또는 둘 다로 가리킵니다. 이러한 작업은 공통 원소를 갖지 않는 타일 포인터 하위 집합을 업데이트할 수 있습니다.
-   복사 및 업데이트 작업 - 스트리밍 리소스에 대한 기본 풀 표면 작업에 대해 데이터 복사 작업을 할 수 있는 모든 API. 매핑되지 않은 타일에서 읽기는 0을 산출하고 매핑되지 않은 타일에 쓰기는 삭제됩니다.
-   타일 복사 및 타일 업데이트 작업 - 이 작업들은 모든 스트리밍 리소스와 정식 메모리 레이아웃의 버퍼 리소스에 대해 64KB 세분성으로 타일을 복사하기 위한 것입니다. 디스플레이 드라이버와 하드웨어는 스트리밍 리소스에 필요한 모든 메모리 "스위즐링"을 수행합니다.
-   Direct3D 파이프라인 바인딩 및 보기 생성 / 비스트리밍 리소스에서 작동하는 바인딩은 스트리밍 리소스에서도 작동합니다.

타일 컨트롤은 즉시 또는 지연 컨텍스트에서 사용할 수 있으며(일반적인 리소스로 업데이트하는 것과 마찬가지임) 실행 시 타일에 대한 후속 액세스에 영향을 미칩니다(이전에 제출한 작업).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스 만들기](creating-streaming-resources.md)

 

 




