---
title: 새로운 기능-2015 년 8 월 Xbox Live SDK 용
description: 새로운 기능-2015 년 8 월 Xbox Live SDK 용
ms.assetid: a034867b-7cc0-4b97-89d5-3486e95a80b4
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a454b7339035475416934c2f921dae65283c340c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639818"
---
# <a name="whats-new-for-the-xbox-live-sdk---august-2015"></a>새로운 기능-2015 년 8 월 Xbox Live SDK 용

참조 하세요 합니다 [새로운 기능-2015 년 6 월](1506-whats-new.md) 추가 된 기능에 대 한 문서는 2015 년 6 월 릴리스에서 합니다.

8 월 릴리스는 Xbox Live SDK의 다음 업데이트가 포함 됩니다.

## <a name="os-and-tool-support"></a>OS 및 도구 지원
Xbox Live SDK는 이제 Windows 10 RTM [Version 10.0.10240] 및 Visual Studio 2015 RTM [Version 14.0.23107.0]를 지원합니다.

## <a name="multiplayer-manager-winrt-apis"></a>다중 접속 Manager WinRT Api
(실험적 네임 스페이스)에서 멀티 플레이 게임 관리자 지원 (c + + Api) 외에도 WinRT Api

## <a name="submit-batch-feedback-from-a-title"></a>제목에서 일괄 처리 피드백 제출
제목에서 한 번에 피드백 항목의 수를 제출합니다.

## <a name="newupdated-documentation"></a>새/업데이트 된 설명서
이제 Xbox Live SDK 패키지에는 업데이트 된 API 참조 및 새 "Xbox Live 프로그래밍 가이드"를 포함, "Docs" 폴더를 포함 합니다.

버그 수정:

* 실시간 작업 서비스에서 구독을 제거 하는 동안 크래시
* 게스트 계정이 로그인 할 때 충돌
* 네트워크 케이블을 분리 하는 경우 액세스 위반
* 터널 오류 이제 c + + Api에서 오류 코드를 제공
* TitleStorageService::DownloadBlobAsync ETag 문제
* 샘플 앱에 대 한 다양 한 버그가 수정 되었습니다.
