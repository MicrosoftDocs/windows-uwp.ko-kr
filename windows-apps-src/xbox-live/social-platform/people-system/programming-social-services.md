---
title: 소셜 서비스 프로그래밍
author: KevinAsgari
description: Xbox Live 소셜 관리자 API를 사용 하는 방법의 코드 예제를 제공 합니다.
ms.assetid: 101d059a-e03f-472c-8300-800aa5730ee2
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 소셜 관리자, 예제
ms.localizationpriority: medium
ms.openlocfilehash: e3d65b55a351073ded99a2ec8d2d1e03f4ab3c34
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7154266"
---
# <a name="programming-social-services"></a>소셜 서비스 프로그래밍

> [!NOTE]
> 이 문서에서는 고급 API 사용 방법을 보여 줍니다.  시작 지점으로 개발을 크게 간소화 [소셜 관리자 API 소개](../intro-to-social-manager.md) 살펴볼을 보십시오.  찾을 경우 지원 되지 않는 시나리오 소셜 관리자에서 알고 있는 사용자 댐을 알려 주시기 바랍니다.

다음 코드 예제에서는 Xbox live 소셜 관계를 검색 하는 방법을 보여 줍니다. 시스템의 모든 사용자의 목록을 생성 하 고 첫 번째를 검색 합니다. 그런 다음, 해당 사용자의 소셜 관계를 모두 검색합니다. 마지막으로, 이러한 관계의 각 공용 속성이 표시 됩니다.

```cpp
XboxLiveContext^ xboxLiveContext = NULL;

// An XboxLiveContext for a user should be created only once, or you may encounter unpredictable behavior.
void OneTimeInit()
{
  // Get the XboxLiveContext.  This should only be done once per user after signing in.
  xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));
}

void Example_SocialService_GetSocialRelationshipsAsync()
{
    // Generate a list of users on the system.
    // Create an XboxLiveContext from user 0 (the first one returned).
    // This user's authentication token will be used for service requests.
    XboxLiveContext^ xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));

    // Asynchronously retrieve all social relationships from that context.
    auto asyncOp = xboxLiveContext->SocialService->GetSocialRelationshipsAsync();

    create_task( asyncOp )
    .then([](XboxSocialRelationshipResult^ result)
    {
        // For each social relationship of the specified user...
        for( XboxSocialRelationship^ xboxSocialRelationship : result->Items )
        {
            // ...display the public properties of the relationship.
            LogComment( xboxSocialRelationship->XboxUserId );
            LogComment( xboxSocialRelationship->IsFavorite.ToString() );
        }

    })
    .wait();
}
```
