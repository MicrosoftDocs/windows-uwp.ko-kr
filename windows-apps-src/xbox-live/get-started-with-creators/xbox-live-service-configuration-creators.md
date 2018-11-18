---
title: Xbox Live 크리에이터 스 서비스 구성
author: KevinAsgari
description: 크리에이터 스 프로그램에 대 한 Xbox Live 서비스 구성에 알아봅니다.
ms.assetid: 22b8f893-abb3-426e-9840-f79de0753702
ms.author: kevinasg
ms.date: 10/03/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b8136aa4d07204d2c97fec393439a17d3660f934
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7146448"
---
# <a name="xbox-live-service-configuration-for-the-creators-program"></a>크리에이터스 프로그램에 대한 Xbox Live 서비스 구성

## <a name="what-is-service-configuration"></a>서비스 구성 란 무엇 인가요?

[연결 된 저장소](../storage-platform/connected-storage/connected-storage-technical-overview.md)및 [순위표](../leaderboards-and-stats-2017/leaderboards.md) 와 같은 Xbox Live 기능 중 일부에 익숙한 수도 있습니다.

하지 않을 경우를 예로 순위표 간략하게 설명 하겠습니다. 순위표 허용 다른 플레이어와 비교는 성과 나타내는 값을 참조 하세요. 아케이드 게임에서 레이싱 게임에서 랩 시간 또는 1 인칭 슈팅에서 헤드샷 예를 들어 최고 점수 합니다. 하지만 해당 물리적 컴퓨터에서 플레이 한 플레이어에서 최고 점수 표시 하는 아케이드 컴퓨터, Xbox Live와 달리 전 세계에서 최고 점수를 표시할 수 있습니다.

하지만 이런 상황은 대 한 Xbox Live에 순위표에 대 한 알 수 있도록 몇 가지 일회성 구성을 수행 해야 합니다. 예를 들어를 오름차순 또는 내림차순 값, 값 정렬할 수는 여부 및 데이터의 어떤 부분은 것 해야 수 정렬 합니다.

Xbox Live 크리에이터 스 프로그램에 대 한 [파트너 센터](https://partner.microsoft.com/dashboard) 에서 발생 하는이 구성 및 설정 하는 방법을 알아보려면 [하기 시작 된 Xbox Live](get-started-with-xbox-live-creators.md) 를 읽을 수 있습니다.

## <a name="get-your-ids"></a>사용자 id

Xbox Live 서비스를 사용 하려면 제목 및 개발 환경을 구성 하는 몇 가지 Id를 가져오는 해야 합니다. Xbox Live 서비스 구성을 업데이트 하 여이 얻을 수 있습니다.

파트너 센터에서 타이틀을 현재 되어 있지 않으면 [만들기 및 테스트 새 크리에이터 스 타이틀](create-and-test-a-new-creators-title.md) 지침에 대 한 참조입니다.

### <a name="critical-ids"></a>중요 한 Id

제목 및 Xbox One 용 응용 프로그램 개발에 대 한 중요 한 세 가지 Id는: 샌드박스 ID, 제목 ID 및 서비스 구성 ID (서비스 안내).

개발 환경 설정에 샌드박스 ID를 설정할 필요가 이지만, 제목 ID 및 서비스 안내 초기 개발을 위해 필요는 없지만 모든 Xbox Live 서비스 사용에 필요 합니다. 따라서 세 가지 모두를 한 번에 얻을 것이 좋습니다. "Xbox Live" 루트 구성 페이지에서 아래와 같이 모든 Id를 볼 수 있습니다.

![](../images/getting_started/devcenter_sandbox_id.png)

#### <a name="sandbox-ids"></a>샌드박스 Id

샌드박스 개발 및 테스트 타이틀에 대 한 정리 지 확인 하는 개발 하는 동안 사용자 환경에 대 한 콘텐츠 격리를 제공 합니다. 샌드박스 ID에 샌드박스를 식별합니다. 콘솔 하나의 샌드박스 여러 콘솔에 액세스할 수 있지만, 한 번에 하나의 샌드박스를 액세스할만 수 있습니다.

샌드박스 Id 대/소문자를 구분 합니다.

#### <a name="service-configuration-id-scid"></a>서비스 구성 ID (서비스 안내)

개발의 일부로, 통계, 순위표 및 기타 온라인 기능의 호스트를 만듭니다. 이들 모든 부분에 서비스 구성 되며 액세스에 대 한 서비스는 안내 필요 합니다. SCIDs 대/소문자를 구분 합니다.

#### <a name="title-id"></a>제목 ID

제목 ID는 고유 하 게 타이틀에 Xbox Live 서비스를 식별합니다. 사용자가 타이틀의 라이브 콘텐츠, 사용자 통계, 도전 과제 및 등을 액세스 하 고 실시간 멀티 플레이 기능을 사용 하는 데 사용 하도록 서비스 전체에서 사용 됩니다.

제목 Id 사용 방법 및 위치에 따라 대/소문자를 수 있습니다.

## <a name="publish-your-xbox-live-service-configuration"></a>Xbox Live 서비스 구성에 게시

레코드를 게임에 대 한 Xbox Live 구성과 변경 하는 경우 Xbox Live의 나머지 부분에서 획득 게임에서 볼 수 전에 변경 내용을 게시 해야 합니다. 게임에서 작업 하는 여전히 자체 개발 샌드박스를 게시 합니다. 개발 샌드박스를 사용 하면 격리 된 환경에서 게임에 대 한 변경 내용에 작업할 수 있습니다. 공개적으로 게임 릴리스된 Xbox Live 구성과 소매 샌드박스에 자동으로 게시 됩니다.
기본적으로 Windows 10 Pc 및 Xbox One 본체 소매 샌드박스에 속합니다.

Xbox Live 구성 페이지에서 개발 샌드박스를 현재 Xbox Live 구성을 게시 하려면 **테스트** 단추를 클릭 합니다.

![](../images/creators_udc/creators_udc_xboxlive_config_test.png)