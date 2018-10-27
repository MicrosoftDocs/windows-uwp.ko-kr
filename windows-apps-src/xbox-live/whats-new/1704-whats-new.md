---
title: 새로운 기능에 대 한 Xbox Live Api-2017 년 4 월
author: KevinAsgari
description: 새로운 기능에 대 한 Xbox Live Api-2017 년 4 월
ms.assetid: ''
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 아레나, 토너먼트
ms.localizationpriority: medium
ms.openlocfilehash: e223c0c9bfca3b516d7eb623aba968cc8b547747
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5691233"
---
# <a name="whats-new-for-the-xbox-live-apis---april-2017"></a>새로운 기능에 대 한 Xbox Live Api-2017 년 4 월

추가 된 항목에 대 한 [새로운-2017 년 3 월](1703-whats-new.md) 문서를 참조 하십시오 2017 년 3 월 릴리스에서 합니다.

## <a name="xbox-services-apis"></a>Xbox 서비스 Api

### <a name="visual-studio-2017"></a>Visual Studio 2017

Xbox Live Api 유니버설 Windows 플랫폼 (UWP) 및 Xbox One 모두 제목에 대 한 Visual Studio 2017 지원 하도록 업데이트 되었습니다.

### <a name="tournaments"></a>토너먼트

새 Api가 토너먼트 지원 하기 위해 추가 되었습니다. 이제 사용할 수는 `xbox::services::tournaments::tournament_service` 타이틀에서 토너먼트 서비스에 액세스 하는 클래스입니다.

이러한 새 토너먼트 Api는 다음과 같은 시나리오를 사용 하도록 설정 합니다.

* 현재 타이틀에 대 한 모든 기존 토너먼트 찾으려면 서비스를 쿼리 합니다.
* 서비스에서는 토너먼트에 대 한 세부 정보를 검색 합니다.
* 팀은 토너먼트에 대 한 목록을 검색 하려면 서비스를 쿼리 합니다.
* 서비스에서는 토너먼트에 대 한 팀에 대 한 세부 정보를 검색 합니다.
* 실시간으로 활동 (RTA) 구독을 사용 하 여 토너먼트 및 팀에 변경 내용을 추적 합니다.
