---
title: 타일 풀에서 할 수 있는 작업
description: 타일 풀에서 할 수 있는 작업으로는 타일 풀 크기 조정, 리소스 제공(전체 타일 풀에 대한 시스템에 일시적으로 메모리 양보), 리소스 회수가 있습니다.
ms.assetid: 90347F7F-C991-4287-BD70-494533ECDC8A
keywords:
- 타일 풀에서 할 수 있는 작업
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6b8b0c6f4fa578e4ec483492b320dc9bc346ab66
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8473375"
---
# <a name="operations-available-on-tile-pools"></a>타일 풀에서 할 수 있는 작업


타일 풀에서 할 수 있는 작업으로는 타일 풀 크기 조정, 리소스 제공(전체 타일 풀에 대한 시스템에 일시적으로 메모리 양보), 리소스 회수가 있습니다.

-   타일 풀의 수명은 이 경우 스트리밍 리소스로부터의 매핑 추적을 포함해 참조 계산의 지원을 받아 다른 모든 Direct3D 리소스와 같이 작동합니다. 응용 프로그램이 더는 타일 풀을 참조하지 않고 메모리에 대한 모든 타일 매핑이 사라지고 GPU(그래픽 처리 장치) 액세스가 완료되면 운영 체제가 타일 풀 할당을 취소합니다.
-   타일 풀에 대한 표면 공유 및 동기화 작업(스트리밍 리소스에 직접 하는 작업은 아님) 관련 API. 제공한 타일 풀에 대한 동작과 마찬가지로, 타일 풀을 공유한 상태에서 현재 또 하나의 디바이스 및 프로세스에 따라 타일 풀을 획득한 경우, 타일 풀을 가리키는 스트리밍 리소스에 액세스하는 Direct3D 명령은 삭제됩니다.
-   타일 풀 크기 조정.
-   자원 제공 및 자원 회수 - 시스템에 일시적으로 메모리를 양보하기 위한 이 작업은 전체 타일 풀에서 작동합니다(개별 스트리밍 리소스에는 사용할 수 없음). 스트리밍 리소스가 제공된 타일 풀에 있는 타일 하나를 가리키는 경우, 스트리밍 리소스는 그 타일이 마치 제공된 것처럼 동작합니다(예를 들어 런타임은 그 타일을 참조하는 명령을 삭제함).

타일 풀 메모리에 대한 직접적인 데이터 복사 작업은 할 수 없습니다. 메모리에 대한 액세스는 스트리밍 리소스를 통해 언제든지 할 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스 만들기](creating-streaming-resources.md)

 

 




