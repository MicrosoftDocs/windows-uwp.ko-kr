---
title: SPOP(Single Point of Presence)
description: Xbox Live 단일 지점 (of Presence)를 사용 하 여 한 번에는 제목만 하나의 장치에서 재생 될 수 있도록 하는 방법을 알아봅니다.
ms.assetid: 40f19319-9ccc-4d35-85eb-4749c2cf5ef8
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, spop, 단일 액세스 지점을 상태
ms.localizationpriority: medium
ms.openlocfilehash: f1503a6168a50175e861e17027e8b642d1b7834d
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8217899"
---
# <a name="single-point-of-presence-spop"></a>SPOP(Single Point of Presence)

## <a name="overview"></a>개요
단일 지점의 현재 상태 (SPOP)는 여기서 제목을 재생 됩니다 한 장치에서 한 번에는 Xbox Live 적용 조건입니다. 이 Xbox One XDK 및 UWP 타이틀이 모든 장치에 적용 됩니다.
장치에 관계 없이 Xbox Live 제목, Xbox One 또는 Windows 10의 다른 장치에는 제목에 서명한 사용자를 시작 수 있습니다.

## <a name="how-to-handle-spop"></a>SPOP 처리 하는 방법
SPOP는 다른 유형의 아웃 이벤트와 동일한 방식으로 제목에서 처리 합니다. 사용자가 작업을 다른 장치에 연결을 끊을 제목 될 것인지 확인 하는 SPOP을 시작 하는 경우 UI를 통해 알림이 항상 됩니다.

* XDK 제목에는 `User::SignOutCompleted` 이 동작이 발생 하면 이벤트 트리거합니다.
* UWP 제목 알려줍니다 통해 아웃의 합니다 `sign_out_complete` 에서 처리기는 `xbox_live_user` 클래스. 자세한 내용은 [UWP 프로젝트에 대 한 인증](authentication-for-UWP-projects.md) 을 참조 하세요.
