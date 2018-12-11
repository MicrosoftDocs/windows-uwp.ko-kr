---
title: 새로운 기능-2015 년 8 월 Xbox Live SDK에 대 한
description: 새로운 기능-2015 년 8 월 Xbox Live SDK에 대 한
ms.assetid: a034867b-7cc0-4b97-89d5-3486e95a80b4
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a454b7339035475416934c2f921dae65283c340c
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8887742"
---
# <a name="whats-new-for-the-xbox-live-sdk---august-2015"></a>새로운 기능-2015 년 8 월 Xbox Live SDK에 대 한

추가 된 항목에 대 한 [새로운 기능-2015 년 6 월](1506-whats-new.md) 문서를 참조 하십시오 2015 년 6 월 릴리스에서 합니다.

Xbox Live SDK의 8 월 릴리스는 다음과 같은 업데이트가 포함 됩니다.

## <a name="os-and-tool-support"></a>운영 체제 및 도구 지원
Xbox Live SDK는 이제 Windows 10 RTM [버전 10.0.10240] 및 Visual Studio 2015 RTM [버전 14.0.23107.0]를 지원합니다.

## <a name="multiplayer-manager-winrt-apis"></a>멀티 플레이어 관리자 WinRT Api
멀티 플레이어 관리자 (실험적 네임 스페이스)에서 지원 됩니다 (c + + Api) 외에도 WinRT Api

## <a name="submit-batch-feedback-from-a-title"></a>타이틀에서 일괄 처리 피드백 제출
다양 한 타이틀에서 한 번에 피드백 항목을 제출합니다.

## <a name="newupdated-documentation"></a>새/업데이트 된 설명서
이제 Xbox Live SDK 패키지 "문서" 폴더를 포함, 업데이트 된 API 참조와 새 "Xbox Live 프로그래밍 가이드"를 포함 합니다.

버그 수정 합니다.

* 실시간으로 활동 서비스에서 구독을 제거 하는 동안 크래시
* 게스트 계정으로 로그인 할 때 충돌
* 액세스 위반이 네트워크 케이블을 분리 하는 경우
* 이제 터널 오류 c + + Api에서 오류 코드를 제공
* ETag 문제가 TitleStorageService::DownloadBlobAsync
* 샘플 앱의 다양 한 버그가 수정 되었습니다.
