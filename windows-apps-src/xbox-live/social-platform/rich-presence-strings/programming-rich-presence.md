---
title: 다양 한 상태 프로그래밍
author: KevinAsgari
description: Xbox Live 멤버의 온라인 상태 상태 설정에 코드 예제를 제공 합니다.
ms.assetid: 7e6e7b69-d7c3-42fa-bcc4-6d68947f6fdb
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 풍부한 상태, xbox
ms.localizationpriority: medium
ms.openlocfilehash: bd39074e67c58e20154083be66211dbefc9f782d
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4383875"
---
# <a name="programming-rich-presence"></a>다양 한 상태 프로그래밍

다양 한 상태는 다른 플레이어에 플레이어의 현재 동작을 광고에 대 한 기능을 제공 합니다. 자세한 내용은 참조 [다양 한 상태 문자열: 개요](rich-presence-strings-overview.md).

```cpp

XboxLiveContext^ xboxLiveContext = NULL;

// Set the XboxLiveContext for a user *once* otherwise you may encounter unpredictable behavior.
void OneTimeInit()
{
  // Get the XboxLiveContext.  This should only be done once per user after signing in.
  xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));
}

void Example_PresenceService_SetPresenceAsync()
{
    // The config ID below is an example.
    // The real ID to use is defined when you set up the game on Xbox Development Portal.
    String^ aServiceConfigurationId = "C1BA92A9-0000-0000-0000-000000000000";

    PresenceData^ presenceData = ref new PresenceData(
        aServiceConfigurationId,
        "mainMenuPresence"
        );

    IAsyncAction^ setPresenceAction = xboxLiveContext->PresenceService->SetPresenceAsync(
        xboxLiveContext->User->XboxLiveId,
        true, // isUserActive
        presenceData    // richPresenceData is optional
        );

    create_task( setPresenceAction )
    .then([] ()
    {
        LogComment( L"SetPresenceAsync Done" );
    })
    .wait();
}
```
