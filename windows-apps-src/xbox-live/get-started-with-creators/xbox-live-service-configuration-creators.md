---
title: Xbox Live 크리에이터 스 서비스 구성
description: 크리에이터 스 프로그램에 대 한 서비스 구성을 Xbox Live에 대해 알아봅니다.
ms.assetid: 22b8f893-abb3-426e-9840-f79de0753702
ms.date: 10/03/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 76d25a48caadb908e30e6e1897c19178e2b837e1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662238"
---
# <a name="xbox-live-service-configuration-for-the-creators-program"></a>크리에이터스 프로그램에 대한 Xbox Live 서비스 구성

## <a name="what-is-service-configuration"></a>서비스 구성 이란?

적이 있을 Xbox Live 기능 중 일부와 같은 [순위표](../leaderboards-and-stats-2017/leaderboards.md) 하 고 [연결 된 저장소](../storage-platform/connected-storage/connected-storage-technical-overview.md)합니다.

가 아닌 경우 예를 들어 순위표 간단 하 게 설명 됩니다. 순위표 수 다른 플레이어에 비해 실적을 나타내는 값을 참조 하세요. 예를 들어 고득점 레이싱 게임에서는 랩 시간 또는 1 인칭 슈팅에서 헤드샷, 아케이드 게임을 합니다. 이지만 해당 물리적 컴퓨터에서 재생 한 선수에서 최고 점수를 표시 하는 아케이드 컴퓨터를 달리 Xbox Live를 사용한 전 세계에서 최고 득점을 표시할 수 있습니다.

있지만 그러려면, Xbox Live에 순위표에 대 한 알 수 있도록 몇 가지 일회성 구성을 수행 해야 합니다. 예를 들어 오름차순 또는 내림차순 값에서 값이 정렬 되도록 해야 하는지 여부 및 데이터의 어느 부분이 해야 수 정렬 합니다.

이 구성에서 발생 [파트너 센터](https://partner.microsoft.com/dashboard) Xbox Live 크리에이터 스 프로그램 고 읽을 수에 대 한 [가져오기 시작 된 Xbox Live](get-started-with-xbox-live-creators.md) 를 설정 하는 방법을 알아봅니다.

## <a name="get-your-ids"></a>사용자 Id를 가져오려면

Xbox Live 서비스를 사용 하려면 개발 환경 및 title을 구성 하는 여러 개의 Id를 가져올 해야 합니다. 이러한 Xbox Live 서비스 구성을 업데이트 하 여 얻을 수 있습니다.

파트너 센터에서 제목의 현재 되어 있지 않으면 참조 [만들기 및 테스트 새 작성자 제목](create-and-test-a-new-creators-title.md) 지침에 대 한 합니다.

### <a name="critical-ids"></a>중요 한 Id

제목 및 Xbox One에 응용 프로그램의 개발을 위한 중요 한 세 가지 Id가: 샌드박스 ID, 제목 ID 및 서비스 구성 서비스 ID (안내).

샌드박스 id로 개발 환경을 설정 하는 데 필요한 상태인 제목 ID 및 서비스 안내 초기 개발에 대 한 필요는 없지만 Xbox Live 서비스 사용에 필요한 합니다. 따라서 세 가지 모두를 한 번에 가져오는 것이 좋습니다. "Xbox Live" 루트 구성 페이지에서 아래와 같이 모든 Id를 볼 수 있습니다.

![](../images/getting_started/devcenter_sandbox_id.png)

#### <a name="sandbox-ids"></a>샌드박스 Id

샌드박스 개발 및 테스트에 제목에 대 한 정리는 개발 하는 동안 사용자 환경에 대 한 콘텐츠 격리를 제공 합니다. 샌드박스 ID 샌드박스에 식별합니다. 콘솔을 하나의 샌드박스 여러 콘솔에서 액세스할 수 있지만 한 시점 하나 샌드박스를 액세스할만 수 있습니다.

샌드박스 Id 대/소문자를 구분 합니다.

#### <a name="service-configuration-id-scid"></a>서비스 구성 ID (서비스 안내)

개발의 일부로 통계, 순위표 및 기타 온라인 기능의 호스트를 만듭니다. 이러한 서비스 구성의 일부인 모든 및 액세스용 서비스를 안내 합니다. SCIDs 대/소문자를 구분 합니다.

#### <a name="title-id"></a>제목 ID

제목 ID Xbox Live 서비스에 제목을 고유 하 게 식별합니다. 타이틀의 라이브 콘텐츠, 사용자 통계, 성과, 및 등을 액세스 하 고 실시간 다중 접속 기능을 사용 하 여 사용자를 사용 하도록 설정 하려면 서비스 전체에서 사용 됩니다.

제목 Id 사용 방법 및 위치에 따라 대/소문자를 수 있습니다.

## <a name="publish-your-xbox-live-service-configuration"></a>Xbox Live 서비스 구성에 게시

게임에 대 한 변경 내용을 Xbox Live 구성 하는 경우에 Xbox Live의 나머지 부분에서 선택 하 고 게임에서 확인할 수 있습니다 하려면 먼저 변경 내용을 게시 해야 합니다. 게임에서 계속 작업 하는 경우 사용자 고유의 개발 샌드박스에 게시 합니다. 개발 샌드박스를 사용 하면 격리 된 환경에서 게임에 변경 내용에 작업할 수 있습니다. 게임은 공개적으로 릴리스되면 Xbox Live configuration 소매점 샌드박스에 자동으로 게시 됩니다.
기본적으로 하나의 콘솔 Xbox 및 Windows 10 Pc의에서 경우 소매 샌드박스

Xbox Live 구성 페이지에서 클릭 합니다 **테스트** 개발 샌드박스에 현재 Xbox Live 구성을 게시 하는 단추입니다.

![](../images/creators_udc/creators_udc_xboxlive_config_test.png)