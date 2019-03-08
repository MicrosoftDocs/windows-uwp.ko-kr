---
title: 새로운 Xbox Live Api-2017 년 8 월
description: 새로운 Xbox Live Api-2017 년 8 월
ms.assetid: ''
ms.date: 08/16/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox 하나, 2017 년 8 월을 새로 추가 된 새로운 기능
ms.localizationpriority: medium
ms.openlocfilehash: db9ffd47e9ee383abb53bdede8de1854aae6b164
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618238"
---
# <a name="whats-new-for-the-xbox-live-apis---august-2017"></a>새로운 기능-2017 년 8 월 Xbox Live Api 용

참조 하세요 합니다 [새로운 기능-2017 년 7 월](1707-whats-new.md) 추가 된 기능에 대 한 문서는 2017 년 7 월 릴리스에서 합니다.

확인할 수도 있습니다는 [Xbox Live API GitHub 커밋 기록을](https://github.com/Microsoft/xbox-live-api/commits/master) 모든 Xbox Live Api의 최근 코드 변경 내용을 확인 합니다.

## <a name="xbox-live-features"></a>Xbox Live 기능

### <a name="in-game-clubs"></a>게임 내 클럽

개발자는 "게임 내 클럽"를 만들 수 있습니다. 개발자가 완전히 사용자 지정이 가능 하며 내부 및 외부 게임에서 사용할 수 있다는 점에서 표준 Xbox 클럽에서 게임 내 클럽 다릅니다. 게임 개발자는 모든 종류의 고유한 요구 사항과 일치 하는 팀, 부족, 명, 길드 등 게임 내 영구 그룹 시나리오를 신속 하 게 빌드를 사용할 수 있습니다.

피드를 LFG, 및 Mixer 자유롭게 Xbox 콘솔, PC 또는 iOS/Android 장치에서를 채팅 같은 club 기능을 사용 하 여 게임 하는 서로 연결 하는 Xbox 라이브 구성원 상태를 유지할 수 있는 Xbox 경험에서 외부 게임에서 게임 내 클럽을 액세스할 수 있습니다.

Api 만들기 및 게임 내에서 직접 게임 내 클럽 관리를 사용할 수 있습니다. 이러한 Api xbox::services::clubs 네임 스페이스에 존재합니다.
