---
title: 새로운 기능-2017 년 8 월에 Xbox Live Api
author: KevinAsgari
description: 새로운 기능-2017 년 8 월에 Xbox Live Api
ms.assetid: ''
ms.author: kevinasg
ms.date: 08/16/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 2017 년 8 월, 새 이란 하나 xbox
ms.localizationpriority: medium
ms.openlocfilehash: 97d12d6a8f415144ff61e5df18757c6aa46397c7
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6191453"
---
# <a name="whats-new-for-the-xbox-live-apis---august-2017"></a>새로운 기능에 대 한 Xbox Live Api-2017 년 8 월

추가 된 항목에 대 한 [새로운-2017 년 7 월](1707-whats-new.md) 문서를 참조 하십시오 2017 년 7 월 릴리스에서 합니다.

또한 모든 Xbox Live Api에 최근 코드 변경 내용을 보려면 [Xbox Live API GitHub 커밋 기록](https://github.com/Microsoft/xbox-live-api/commits/master) 을 확인할 수 있습니다.

## <a name="xbox-live-features"></a>Xbox Live 기능

### <a name="in-game-clubs"></a>게임의 클럽

이제 개발자 "게임의 클럽"을 만들 수 있습니다. 게임의 클럽 개발자가 완전히 사용자 지정할 수 있으며 내부와 외부 게임에서 사용할 수 있다는 점에서 표준 Xbox 클럽에서 다릅니다. 게임 개발자가 신속 하 게 모든 종류의 고유한 요구 사항에 맞는 팀, 부족, 명, 길드 등과 같은 게임 내에서 영구 그룹 시나리오를 빌드하는 데 사용할 수 있습니다.

피드 LFG, 및 Mixer 자유롭게 Xbox 콘솔, PC, 또는 iOS/Android 장치를 채팅 같은 클럽 기능을 사용 하 여 게임을 서로 연결 하는 Xbox live 멤버를 유지 하는 모든 Xbox 환경에서 외부 게임에서 게임의 클럽 액세스할 수 있습니다.

Api는 작성 및 게임 내에서 직접 게임의 클럽을 관리할 수 있습니다. 이러한 Api는 xbox::services::clubs 네임 스페이스에 존재 합니다.
