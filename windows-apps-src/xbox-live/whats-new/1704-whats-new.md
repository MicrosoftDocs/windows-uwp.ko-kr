---
title: 새로운 기능 Xbox Live Api에 대 한-2017 년 4 월
author: KevinAsgari
description: 새로운 기능 Xbox Live Api에 대 한-2017 년 4 월
ms.assetid: ''
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 아레나, 토너먼트
ms.localizationpriority: medium
ms.openlocfilehash: 5084b0badf29bdf5e4adc3b45ee1961d6a4e7097
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4747464"
---
# <a name="whats-new-for-the-xbox-live-apis---april-2017"></a>새로운 기능 Xbox Live Api에 대 한-2017 년 4 월

추가 된 항목에 대 한 [새로운 기능-2017 년 3 월](1703-whats-new.md) 문서를 참조 하십시오 2017 년 3 월 릴리스에서 합니다.

## <a name="xbox-services-apis"></a>Xbox 서비스 Api

### <a name="visual-studio-2017"></a>Visual Studio 2017

Xbox Live Api 유니버설 Windows 플랫폼 (UWP)와 Xbox One에 모두 제목에 대 한 Visual Studio 2017을 지원 하도록 업데이트 되었습니다.

### <a name="tournaments"></a>토너먼트

토너먼트 지원 하기 위해 새로운 Api가 추가 되었습니다. 이제 사용할 수는 `xbox::services::tournaments::tournament_service` 타이틀에서 토너먼트 서비스에 액세스 하는 클래스.

이러한 새 토너먼트 Api는 다음과 같은 시나리오를 사용 하도록 설정:

* 현재 제목에 대 한 모든 기존 토너먼트 찾으려면 서비스를 쿼리 합니다.
* 서비스에서는 토너먼트에 대 한 세부 정보를 검색 합니다.
* 토너먼트에 대 한 팀의 목록을 검색 서비스를 쿼리 합니다.
* 서비스에서는 토너먼트에 대 한 팀에 대 한 세부 정보를 검색 합니다.
* 변경 내용을 추적 토너먼트 및 팀 실시간으로 활동 (RTA) 구독을 사용 하 여 합니다.
