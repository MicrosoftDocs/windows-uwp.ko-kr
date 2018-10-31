---
title: XDK 프로젝트에 대 한 인증
author: KevinAsgari
description: Xbox 개발 키트 (XDK) 타이틀에 Xbox Live 사용자가 로그인 하는 방법을 알아봅니다.
ms.assetid: 713bb2e3-80c5-4ac9-8697-257525f243d3
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3d11a9ce213e8ba1cb51d633cd11364285a64154
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5838755"
---
# <a name="authentication-for-xdk-projects"></a>XDK 프로젝트에 대 한 인증

게임에 Xbox Live 기능을 활용 되려면 사용자를 Xbox Live 커뮤니티에서 자신을 식별 하는 Xbox Live 프로필을 생성 해야 합니다.  Xbox Live 서비스 게임 관련 한 추적 사용자의 게이머 태그 및 사용자의 게임 친구 들 게이머 사진 등의 Xbox Live 프로필을 사용 하 여 활동 사용자 어떻게 게임 플레이, 사용자가 잠금 사용자 독립적 여기서 어떤 도전 과제는 특정 게임 등에 대 한 순위표 합니다.

상위 수준에서 다음 단계를 수행 하 여 Xbox Live Api를 사용 합니다.
1. 상호 작용 사용자를 식별 합니다.
2. 사용자를 기준으로 하는 Xbox Live 컨텍스트 만들기
3. Xbox Live 컨텍스트를 사용 하 여 Xbox Live 서비스에 액세스
4. 때 게임가 종료 되거나 사용자가 기호 아웃, null로 설정 하 여 XboxLiveContext 개체를 해제 합니다.

### <a name="creating-an-xboxliveuser-object"></a>XboxLiveUser 개체 만들기
대부분의 Xbox Live 작업 Xbox Live 사용자와 관련이 있습니다.  게임 개발자, 로컬 사용자를 나타내는 XboxLiveUser 개체를 만들려면 먼저 필요 합니다.

C++:
```
Windows::Xbox::System::User^ user; // the interacting user.  From User::Users, etc
std::shared_ptr<xbox::services::xbox_live_context> xboxLiveContext = std::make_shared<xbox::services::xbox_live_context>( user );
```

WinRT:
```
Windows::Xbox::System::User^ user; // the interacting user.  From User::Users, etc
Microsoft::Xbox::Services::XboxLiveContext^ xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext( user );
```
