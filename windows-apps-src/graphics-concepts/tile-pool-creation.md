---
title: 타일 풀 만들기
description: 응용 프로그램은 Direct3D 장치당 하나 이상의 타일 풀을 만들 수 있습니다. 각 타일 풀의 총 크기는 약 1/4 그래픽 처리 장치 (GPU) RAM의 Direct3D11의 리소스 크기 제한, 제한 됩니다.
ms.assetid: BD51EDD3-4AD3-4733-B014-DD77B9D743BB
keywords:
- 타일 풀 만들기
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cbb8b61c8eeef1a842a7c6b61d09670f056bb409
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6030205"
---
# <a name="tile-pool-creation"></a>타일 풀 만들기


응용 프로그램은 Direct3D 장치당 하나 이상의 타일 풀을 만들 수 있습니다. 각 타일 풀의 총 크기는 약 1/4 그래픽 처리 장치 (GPU) RAM의 Direct3D11의 리소스 크기 제한, 제한 됩니다.

타일 풀은 64KB 타일로 구성되지만 운영 체제(디스플레이 드라이버)가 백그라운드에서 전체 풀을 하나 이상의 할당으로 관리합니다. 따라서 세분화 수준은 응용 프로그램에 표시되지 않습니다. 스트리밍 리소스는 타일 풀을 구성하는 타일을 가리키는 콘텐츠를 정의합니다. 타일을 스트리밍 리소스에서 매핑 해제하려면 타일을 **NULL**로 지정하면 됩니다. 이렇게 매핑 해제된 타일은 읽기 또는 쓰기 동작에 대한 규칙이 있스니다. [위험 추적 및 타일 풀 리소스](hazard-tracking-versus-tile-pool-resources.md)를 참조하세요.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[타일 풀에 대한 매핑](mappings-are-into-a-tile-pool.md)

 

 




