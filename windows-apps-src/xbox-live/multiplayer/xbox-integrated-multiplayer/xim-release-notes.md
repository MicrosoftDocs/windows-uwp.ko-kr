---
title: Xbox 통합 멀티 플레이어 릴리스 정보
author: KevinAsgari
description: Xbox 통합 멀티 플레이어에 대 한 릴리스 정보를 포함합니다.
ms.assetid: 38df7a49-71b9-4d86-9c49-683ffa7308d6
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, xbox 통합된 멀티 플레이어
ms.localizationpriority: medium
ms.openlocfilehash: 1f2416eec418fade2c7851c90af9e492ba8df537
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7154673"
---
# <a name="xbox-integrated-multiplayer-release-notes"></a>Xbox 통합 멀티 플레이어 릴리스 정보

2016 년 12 월 14 일 업데이트

다음 방법의 Xbox 통합 멀티 플레이어 (XIM)이 릴리스에서 사용할 수 없는:

-   `xim_authority::set_authority_reconciled_data()`
-   `xim_authority::set_authority_reconciliation_data()`
-   `xim_authority::send_data_to_players()`
-   `xim_authority::network_path_information()`
-   `xim_player::xim_local::send_data_to_authority()`

다음 상태 변경은이 릴리스의 XIM에서는 제공 되지 않습니다.

-   `xim_state_change_type::player_to_authority_data_received`
-   `xim_state_change_type::authority_to_player_data_received`
-   `xim_state_change_type::authority_reconciling`
-   `xim_state_change_type::authority_reconciled_local`
-   `xim_state_change_type::authority_reconciled_remote`
-   `xim_state_change_type::send_queue_alert_triggered`
