---
title: SPOP(Single Point of Presence)
description: 한 번에 제목을 단일 장치에서 재생 됩니다 되도록 Xbox Live Single Point of Presence (SPOP)를 사용 하는 방법을 알아봅니다.
ms.assetid: 40f19319-9ccc-4d35-85eb-4749c2cf5ef8
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, spop, 단일 지점의 현재 상태
ms.localizationpriority: medium
ms.openlocfilehash: f1503a6168a50175e861e17027e8b642d1b7834d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595008"
---
# <a name="single-point-of-presence-spop"></a>SPOP(Single Point of Presence)

## <a name="overview"></a>개요
단일 지점의 현재 상태 (SPOP)에 있는 제목을 재생할 수 있습니다만 하나의 장치 시간에는 Xbox Live 적용 조건입니다. 이 Xbox One XDK 및 모든 장치에서 UWP 타이틀에 대 한 적용 됩니다.
Xbox Live 제목, 장치에 관계 없이 사용자가 다른 Xbox One 또는 Windows 10 장치에서 제목에 로그인을 시작할 수 있습니다.

## <a name="how-to-handle-spop"></a>SPOP를 처리 하는 방법
SPOP는 다른 유형의 로그 아웃 이벤트와 마찬가지로 제목에서 처리 합니다. 인해 다른 장치의 연결을 끊을 수 제목 바란다고 있는지 확인 하는 SPOP 시작 하는 작업을 수행 하는 경우 사용자 UI를 통해 항상 알려지므로 됩니다.

* XDK 타이틀에 대 한는 `User::SignOutCompleted` 이런 때 트리거됩니다.
* UWP 타이틀에 대 한 알림이 표시 됩니다 통해 아웃 합니다 `sign_out_complete` 에서 처리기는 `xbox_live_user` 클래스입니다. 참조 [UWP 프로젝트에 대 한 인증](authentication-for-UWP-projects.md) 자세한
