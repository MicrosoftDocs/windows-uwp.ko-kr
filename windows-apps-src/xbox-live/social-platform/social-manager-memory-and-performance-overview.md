---
title: 소셜 관리자 메모리 및 성능
author: KevinAsgari
description: Xbox Live 소셜 관리자 API 관리자를 사용 하는 경우 메모리 및 성능 고려 사항에 설명 합니다.
ms.assetid: 2540145e-b8e2-4ab5-9390-65e2c9b19792
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 소셜 관리자, 사람
ms.localizationpriority: medium
ms.openlocfilehash: 711f04f29321736ae8adc887b54ce752963908c5
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5982313"
---
# <a name="social-manager-memory-and-performance-overview"></a>소셜 관리자 메모리 및 성능 개요

## <a name="memory"></a>메모리
소셜 관리자가 이제 허용 할당 된 영구 메모리 제목 공간에 저장할 수 있습니다. 소셜 관리자에 의해 사용 하기 위해 사용자 지정 메모리 할당을 호출 하 여 지정할 수 있습니다 `xbox_live_services_settings::set_memory_allocation_hooks()`. 이 메모리 할당 후크는도 사용할 수 Xbox Live API (XSAPI)를 사용 하는 향후 큰 메모리 할당 합니다.

기본적으로 소셜 관리자 메모리 할당에 대 한 최악의 경우 약 4.75 mb (1) 해야 합니다. 일부 추가 오버 헤드는 소셜 관리자 할당 RTA 및 HTTP 업데이트에 따라 합니다. 만드는 경우는 `xbox_social_user_group` 목록에서 추가 된 각 멤버는 추가 2.43 kb 차지 합니다. 사용자를 통해 추가 하는 경우는 `create_social_social_user_group_from_list`, `update_social_user_group`, 또는 제목 외부 친구를 추가 하면, 인접 한 메모리 공간을 찾지 realloc을 사용할 수는 발생할 수 있습니다.

(1) 4.75 mb에서 가져온 1000 = `xbox_social_user` 2.43 kb * 2. 2는 소셜 관리자 유지 double 메모리 버퍼입니다.

## <a name="additional-user-limits"></a>추가 사용자 제한
소셜 관리자는 현재 그래프에 100 추가 된 사용자의 제한이 되어 있습니다. 다음은 두 문제 때문입니다.

1. 사용자가 있는 RTA 구독의 최대 수는 1100 합니다. 로컬 사용자 1000 친구에 있는 경우 추가 구독에 대 한 100만 남았습니다.
2. PeopleHub를 보낼 수 있는 사용자의 일괄 처리 크기의 최대 크기는 약 100 현재입니다.

소셜 관리자 100 명이 넘는 사용자를 포함 하는 소셜 사용자 그룹 목록에서 허용 하지 않도록이 통신 합니다. 언제 든 지를 통해 시스템에 있을 수 있는 100 총 추가 사용자의 글로벌 제한이 `create_social_user_group_from_list`.

## <a name="processing-time"></a>처리 시간
소셜 관리자 최악의 경우 1100 사용자를 갖게 됩니다. Xbox One의 소셜 관리자의 성능 특성에 대 한 0.5 밀리초로 0.3 ms의 최악의 경우는 `do_work` 만들어진 소셜 그래프의 수에 따라 합니다. 일반적인 경우는 아님 1000 명의 사용자와 그룹에 대 한 그룹 생성 및 최대 0.2 ms 없는로 대 한 0.01 밀리초입니다.
