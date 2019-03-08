---
title: 새로운 기능-2016 년 2 월 Xbox Live SDK 용
description: 새로운 기능-2016 년 2 월 Xbox Live SDK 용
ms.assetid: 7ff34ea4-f07d-4584-98e4-13977994ccca
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c13b2893397a0d84e8919146435ece338bc1afde
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594738"
---
# <a name="whats-new-for-the-xbox-live-sdk---february-2016"></a>새로운 기능-2016 년 2 월 Xbox Live SDK 용

하십시오 합니다 [새로운 기능-2015 년 10 월](1510-whats-new.md) 1510에 추가 된 기능에 대 한 문서

## <a name="os-and-tool-support"></a>OS 및 도구 지원
Xbox Live SDK는 Windows 10 RTM [Version 10.0.10240] 및 Visual Studio 2015 RTM [Version 14.0.23107.0]를 지원합니다.

## <a name="throttling"></a>제한
- 세분화 된 제한 곧 출시 될 예정 대부분의 Xbox Live 서비스에 있습니다.  Xbox 서비스 API (XSAPI)는 자동으로 재시도 처리 하 고 개발 하는 동안 제한 되는 호출 하는 경우 알립니다.  자세한 세부 정보를 찾을 수 있습니다 합니다 [모범 사례 호출 Xbox Live](../using-xbox-live/best-practices/best-practices-for-calling-xbox-live.md) 문서의 문서.

## <a name="leaderboards"></a>순위표
- 열이 여러 개인 순위표 GetLeaderboard API에서 액세스할 수 있습니다. 추가 열 이름의 벡터를 제공 하는 경우 해당 열이 없으면 결과에 있는 열의 벡터 입력 됩니다.

## <a name="documentation"></a>설명서
- [Application Insights](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/event-driven-data-platform/application-insights) 설명서는 여기 있습니다.  무료 Azure 계정을 사용 하 여 거의 실시간 데이터 플랫폼 이벤트를 보려면 Application Insights를 사용할 수 있습니다.  이 기능은 현재만 바탕 화면에서 Windows 10에서 실행 하는 UWP 응용 프로그램에 사용할 수 있습니다.
- 데이터 플랫폼 이벤트를 전송 하는 것에 대 한 래퍼를 생성 하는 방법을 설명 하는 UWP 개발자를 위한 Xbox 공용 이벤트 도구에서 업데이트 된 설명서입니다.  이 선택 사항임을 계속 하려는 경우 WriteInGameEvent API를 사용할 수 있습니다 note 하십시오.
- Fiddler를 사용 하 여 데이터 플랫폼 이벤트를 디버그 하 여 올바르게 전송 되는 것이 있는지 확인 합니다.  UWP 이벤트에만 해당 됩니다.
- 라이브 추적 분석기 도구에 대 한 로그를 수집 하는 방법에 대 한 정보가 제공 됩니다.  참조 된 [Xbox Live 서비스에 대 한 호출을 분석](../tools/analyze-service-calls.md) 문서.
