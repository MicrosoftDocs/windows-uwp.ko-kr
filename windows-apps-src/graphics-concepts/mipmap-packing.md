---
title: Mipmap 압축
description: mips 중 일부(배열 슬라이스당)는 스트리밍 리소스의 크기, 형식, mipmap의 개수 및 배열 슬라이스에 따라 일부 타일로 압축할 수 있습니다.
ms.assetid: 906C3CAC-4E84-4947-B508-06788551BE85
keywords:
- Mipmap 압축
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ec3d091d7cc5aca82aeef9a3e7f29a8d363705a3
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6266010"
---
# <a name="mipmap-packing"></a>Mipmap 압축


mips 중 일부(배열 슬라이스당)는 스트리밍 리소스의 크기, 형식, mipmap의 개수 및 배열 슬라이스에 따라 일부 타일로 압축할 수 있습니다.

특정 크기의 mipmap은 스트리밍 리소스 지원의 [계층](streaming-resources-features-tiers.md)에 따라 표준 타일 모양을 따르지 않고 응용 프로그램에 불투명한 방식으로 모두 함께 상호 압축된 것으로 간주합니다. 더 높은 계층의 지원은 어떤 유형의 표면 크기가 표준 타일 모양에 맞는지(따라서 응용 프로그램에서 개별적으로 매핑할 수 있는지)에 대해 더 폭넓은 보증을 받습니다.

구현 간의 차이는 스트리밍 리소스의 크기, 형식, mipmap의 개수 및 배열 슬라이스가 정해진 조건에서 M개의 mips(배열 슬라이스당)를 N개의 타일로 압축할 수 있다는 것입니다. 디바이스의 리소스 타일링 정보를 가져오면 드라이버는 응용 프로그램에게 M과 N이 무엇인지(표준이어서 하드웨어 공급업체에 따라 달라지지 않는 표면의 기타 세부 정보 중에서) 보고합니다. 압축된 mips의 타일 집합은 여전히 64KB이고 타일 풀의 서로 다른 여러 위치에 개별적으로 매핑할 수 있습니다.

하지만 타일의 픽셀 모양과 타일 집합 전반에 걸쳐 mipmap이 맞아들어가는 방식은 하드웨어 공급업체에 따라 다르고 너무 복잡해 노출할 수 없습니다. 따라서 응용 프로그램은 압축된 것으로 지정된 모든 타일을 매핑하거나 그 중 아무것도 매핑하지 않아야 합니다. 그렇게 하지 않으면 스트리밍 리소스에 액세스하기 위한 동작을 정의할 수 없습니다.

배열형 표면의 경우, 압축된 mips의 집합과 그러한 mips(앞서 설명한 M 및 N)를 저장하고 있는 압축된 타일의 개수가 각 배열 슬라이스에 개별적으로 적용됩니다.

타일 복사 전용 API는 압축된 mips에 액세스할 수 없습니다. 압축된 mips에 대해 데이터 복사 작업을 할 응용 프로그램은 표면으로 복사 및 렌더링하기 위한 모든 비스트리밍 리소스별 API를 사용하여 해당 작업을 할 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스 영역의 타일링 방법](how-a-streaming-resource-s-area-is-tiled.md)

 

 




