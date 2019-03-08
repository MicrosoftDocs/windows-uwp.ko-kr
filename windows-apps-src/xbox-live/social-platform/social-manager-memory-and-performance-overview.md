---
title: 소셜 관리자 메모리 및 성능
description: Xbox Live 소셜 관리자 API 관리자를 사용 하는 경우 메모리 및 성능 고려 사항을 설명 합니다.
ms.assetid: 2540145e-b8e2-4ab5-9390-65e2c9b19792
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 소셜 관리자, 사용자
ms.localizationpriority: medium
ms.openlocfilehash: 7c6a0a31c8fe82faa644a59147060f9323d42eb0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618458"
---
# <a name="social-manager-memory-and-performance-overview"></a>소셜 관리자 메모리 및 성능 개요

## <a name="memory"></a>메모리
소셜 네트워크 관리자가 이제 허용 제목 공간에 보유 되도록 영구 메모리를 할당 합니다. 소셜 manager 사용에 대 한 사용자 지정 메모리 할당자를 호출 하 여 지정할 수 있습니다 `xbox_live_services_settings::set_memory_allocation_hooks()`합니다. 이 메모리 할당자 후크를 사용할 수도 있습니다 Xbox Live API (XSAPI)를 사용 하는 향후 대용량 메모리 할당에 대 한 합니다.

기본적으로 소셜 관리자가 메모리 할당에 대 한 최악의 경우 약 4.75 mb (1) 이어야 합니다. 일부 추가적인 오버 헤드가 발생 해당 소셜 관리자를 할당할 수 있습니다 RTA 및 HTTP 업데이트를 기준으로 합니다. 만드는 경우는 `xbox_social_user_group` 목록에서 추가 된 각 멤버를 추가 2.43 kb가 차지 합니다. 사용자를 통해 추가 하는 경우는 `create_social_social_user_group_from_list`, `update_social_user_group`, 또는 제목 외부에서 친구를 추가 하는 사용자, 인접 한 메모리 공간을 찾으려면 realloc 발생할 수 있습니다.

(에서 제공 됩니다 1) 4.75 mb = 1000 `xbox_social_user` 2.43 kb * 2. 2는 이중 메모리 버퍼를 보관 하는 해당 소셜 관리자입니다.

## <a name="additional-user-limits"></a>추가 사용자 제한
소셜 Manager에는 현재 그래프에 추가 100 명 사용자 제한에 있습니다. 다음은 두 가지 문제로 인해입니다.

1. 사용자를 가질 수 있는 RTA 구독의 최대 수는 1100 합니다. 로컬 사용자에 게 1000 친구에는 추가 구독에 대 한 100만 둡니다.
2. 일괄 처리 크기 PeopleHub에 보낼 수 있는 사용자의 최대 용량은 현재 약 100입니다.

주민 등록 관리자는 사용자 수가 100 개 이상의 목록에서 소셜 사용자 그룹을 허용 하지 않도록이 통신 합니다. 언제 든 지를 통해 시스템에 저장할 수 있는 100 총 추가 사용자의 전역 제한이 `create_social_user_group_from_list`합니다.

## <a name="processing-time"></a>Processing Time 수집
소셜 Manager 사례 1100 사용자 최악의 해야 합니다. Xbox One에 소셜 manager의 성능 특성에 대 한 0.5 ms를 0.3 ms의 최악의 사례가 `do_work` 생성 하는 소셜 그래프의 수에 따라 합니다. 일반적인 경우는 0.01 밀리초에서 1000 명의 사용자를 사용 하 여 그룹에 대 한 그룹 생성 되 고 최대 0.2 ms 없습니다를 사용 하 여 합니다.
