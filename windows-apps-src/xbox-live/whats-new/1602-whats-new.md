---
title: 새로운 기능에 대 한 Xbox Live SDK-2016 년 2 월
author: KevinAsgari
description: 새로운 기능에 대 한 Xbox Live SDK-2016 년 2 월
ms.assetid: 7ff34ea4-f07d-4584-98e4-13977994ccca
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9180642d324146667425d6031143430dc499b5b9
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4415217"
---
# <a name="whats-new-for-the-xbox-live-sdk---february-2016"></a>새로운 기능에 대 한 Xbox Live SDK-2016 년 2 월

1510에 추가 된 항목에 대 한 [새로운 기능-2015 년 10 월](1510-whats-new.md) 문서를 참조 하세요

## <a name="os-and-tool-support"></a>운영 체제 및 도구 지원
Xbox Live SDK는 Windows 10 RTM [버전 10.0.10240] 및 Visual Studio 2015 RTM [버전 14.0.23107.0]를 지원합니다.

## <a name="throttling"></a>제한
- 세분화 된 조절 됩니다 곧 롤아웃 될 대부분의 Xbox Live 서비스.  Xbox 서비스 API (XSAPI)은 자동으로 다시 시도 처리 하 고의 호출을 개발 하는 동안 제한 되는 알립니다.  자세한 내용은 설명서의 [모범 사례 호출 Xbox Live](../using-xbox-live/best-practices/best-practices-for-calling-xbox-live.md) 문서에서 찾을 수 있습니다.

## <a name="leaderboards"></a>순위표
- 다중 열 순위표 GetLeaderboard API에 액세스할 수 있습니다. 이름을 추가 열 벡터를 제공 하는 경우 해당 하는 열 존재 하는 경우 결과에 열 벡터 입력 됩니다.

## <a name="documentation"></a>설명서
- [Application Insights](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/event-driven-data-platform/application-insights) 설명서는 다음과 같습니다.  Azure 무료 계정을 사용 하 여 거의 실시간 데이터 플랫폼 이벤트를 보려면 Application Insights를 사용할 수 있습니다.  이 기능은 현재만 바탕 화면에서 Windows 10에서 실행 되는 UWP 응용 프로그램에 사용할 수 있습니다.
- 데이터 플랫폼 이벤트를 전송 하는 것에 대 한 래퍼를 생성 하는 방법에 설명 하는 UWP 개발자를 위한 일반적인 이벤트 Xbox 도구에서 업데이트 된 설명서.  이것은 선택 사항 하 고 원하는 경우 WriteInGameEvent API를 사용 하 여 계속 수 note 하십시오.
- Fiddler를 사용 하 여 데이터 플랫폼 이벤트를 디버그 하 고 제대로 전송 되 고 있는지 확인 합니다.  UWP 이벤트에만 사용 됩니다.
- 라이브 추적 분석기 도구에 대 한 로그를 수집 하는 방법에 대 한 정보를 사용할 수 있습니다.  [Xbox Live 서비스에 대 한 분석 호출](../tools/analyze-service-calls.md) 문서를 참조 하세요.