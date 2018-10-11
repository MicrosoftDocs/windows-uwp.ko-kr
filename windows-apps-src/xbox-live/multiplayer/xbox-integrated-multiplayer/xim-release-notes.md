---
title: Xbox 통합 멀티 플레이어 릴리스 정보
author: KevinAsgari
description: Xbox 통합 멀티 플레이어에 대 한 릴리스 정보를 포함합니다.
ms.assetid: 38df7a49-71b9-4d86-9c49-683ffa7308d6
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, xbox 통합된 멀티 플레이어
ms.localizationpriority: medium
ms.openlocfilehash: 07c3094b5308bb0062d046440f83b45b842eb820
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4507917"
---
# <a name="xbox-integrated-multiplayer-release-notes"></a>Xbox 통합 멀티 플레이어 릴리스 정보

2016 년 12 월 14 일 업데이트

다음 방법의 Xbox 통합 멀티 플레이어 (XIM)이 릴리스에서 사용할 수 없는:

-   `xim_authority::set_authority_reconciled_data()`
-   `xim_authority::set_authority_reconciliation_data()`
-   `xim_authority::send_data_to_players()`
-   `xim_authority::network_path_information()`
-   `xim_player::xim_local::send_data_to_authority()`

다음 상태 변화는 XIM의이 릴리스에서 제공 되지 않습니다.

-   `xim_state_change_type::player_to_authority_data_received`
-   `xim_state_change_type::authority_to_player_data_received`
-   `xim_state_change_type::authority_reconciling`
-   `xim_state_change_type::authority_reconciled_local`
-   `xim_state_change_type::authority_reconciled_remote`
-   `xim_state_change_type::send_queue_alert_triggered`
