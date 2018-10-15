---
title: 새로운 기능-2015 년 8 월에 대 한 Xbox Live SDK
author: KevinAsgari
description: 새로운 기능-2015 년 8 월에 대 한 Xbox Live SDK
ms.assetid: a034867b-7cc0-4b97-89d5-3486e95a80b4
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a8dedfe2111621215f0cc0741f7209c37f816bd3
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4614616"
---
# <a name="whats-new-for-the-xbox-live-sdk---august-2015"></a>새로운 기능-2015 년 8 월에 대 한 Xbox Live SDK

추가 된 항목에 대 한 [새로운 기능-2015 년 6 월](1506-whats-new.md) 문서를 참조 하십시오 2015 년 6 월 릴리스에서 합니다.

Xbox Live SDK의 8 월 릴리스는 다음과 같은 업데이트가 포함 됩니다.

## <a name="os-and-tool-support"></a>운영 체제 및 도구 지원
Xbox Live SDK는 이제 Windows 10 RTM [버전 10.0.10240] 및 Visual Studio 2015 RTM [버전 14.0.23107.0]를 지원합니다.

## <a name="multiplayer-manager-winrt-apis"></a>멀티 플레이어 관리자 WinRT Api
멀티 플레이어 관리자 (실험적 네임 스페이스)에서 지 원하는 (c + + Api) 외에도 WinRT Api

## <a name="submit-batch-feedback-from-a-title"></a>타이틀에서 일괄 처리 피드백 제출
다양 한 타이틀에서 한 번에 피드백 항목을 제출합니다.

## <a name="newupdated-documentation"></a>새/업데이트 된 설명서
이제 Xbox Live SDK 패키지 "문서" 폴더를 포함, 업데이트 된 API 참조와 새 "Xbox Live 프로그래밍 가이드"를 포함 합니다.

버그 수정 합니다.

* 실시간으로 활동 서비스에서 구독을 제거 하는 동안 크래시
* 게스트 계정으로 로그인 할 때 충돌
* 액세스 위반이 네트워크 케이블을 분리 하는 경우
* 터널 오류 이제을 제공 하는 오류 코드는 c + + Api
* ETag 문제가 TitleStorageService::DownloadBlobAsync
* 샘플 앱의 다양 한 버그가 수정 되었습니다.
