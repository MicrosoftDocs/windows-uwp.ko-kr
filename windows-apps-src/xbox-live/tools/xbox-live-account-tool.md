---
title: Xbox Live 계정 도구
author: KevinAsgari
description: Xbox Live 계정 도구를 사용 하 여 신속 하 게 Xbox Live가 지원 타이틀을 테스트 하기 위한 테스트 계정을 만드는 방법을 알아봅니다.
ms.assetid: ec5959f9-1c60-4aa4-94a6-5d8bdcf77a96
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox, 테스트, 테스트 계정
ms.localizationpriority: medium
ms.openlocfilehash: 55e2d46f59a8ecd2d8bac77e8ce61834a4249a88
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4682565"
---
# <a name="xbox-live-account-tool"></a>Xbox Live 계정 도구

## <a name="what-is-xbox-live-account-tool"></a>Xbox Live 계정 도구란 무엇 인가요?
Xbox Live 계정 도구는 제목 개발자가 게임 시나리오를 테스트 하는 것에 대 한 기존 개발자 계정을 설정할 수 있도록 설계 되었습니다. 예를 들어, Xbox Live 계정 도구를 사용 하 여 개발자 계정의 게이머 태그를 변경 하거나 신속 하 게 1000 팔로 워 계정의 친구 목록에 추가 수 있습니다.

## <a name="what-can-i-do-with-xbox-live-account-tool"></a>Xbox Live 계정 도구를 사용 하 여 어떻게 해야 합니까?
다음을 수행할 수 있습니다.
  1. 사용자의 프로필 설정, XUID, 및 활성 권한 보기
  2. 사용자의 소셜 그래프를 텍스트 파일 또는 Xbox 개발자 플랫폼 csv에서 팔로 워의 목록을 추가합니다
  3. 사용자의 친구 목록 관리: unfavorite, 즐겨찾기를 잠그고 사용자에 따라, 하 게 수행 하면 다시 참조
  4. 개발자 사용자의 평판 변경 (및 통계 원시 신뢰도 값이 즉시 표시)
  5. 사용자의 게이머 변경

## <a name="where-can-i-find-xbox-live-account-tool"></a>Xbox Live 계정 도구는 어디서 찾을 수 있나요?
Xbox Live 계정 도구에서 Xbox Live 도구 패키지의 일부로 찾을 수 있습니다 [https://aka.ms/xboxliveuwptools](https://aka.ms/xboxliveuwptools).

## <a name="how-do-i-log-in"></a>어떻게에 로그인 있나요?
관리 하 고 올바른 샌드박스를 지정 하려는 사용자의 자격 증명을 해야 합니다. 개발자 계정에 대 한 액세스 샌드박스를, 그렇지 않은 경우 로그인 실패할 수 있는지 확인 합니다. 도구는 개발자 계정 샌드박스를 사용 하 여 염두에 두고 설계 되었습니다.

## <a name="can-i-use-a-retail-account-or-does-it-have-to-be-a-sandboxed-account"></a>소매 계정에 사용할 수 있나요 아니면 샌드박스가 적용 된 계정으로 있습니까?
물론 Xbox Live 계정 도구는 소매 계정을 관리 하는 데 사용할 수 있지만 일부 기능이 지원 됩니다. 예를 들어 소매 사용자의 평판을 변경할 수 없습니다.

## <a name="how-do-i-change-a-dev-users-gamertag"></a>개발자 사용자의 게이머 태그를 변경 하려면 어떻게 해야 하나요?
"게이머 태그" 탭으로 이동한 게이머 태그를 입력 합니다. 게이머 태그 숫자, 문자 및 공백을 포함 해야 하 고 15 자 될 수 있습니다. 개발자 계정 게이머 태그는 2로 시작 해야 합니다. 한 번만 변경은 현재 지원 됩니다.

## <a name="how-do-i-see-my-block-list"></a>내 차단 목록 확인 어떻게 해야 합니까?
"사람" 탭으로 이동 하 여 사용자에 게는 현재 차단 된 정렬 "차단" 열 머리글을 선택 합니다.

## <a name="how-do-i-follow-a-large-group-of-users"></a>큰 그룹에 사용자가 수행 하는 방법
XUIDs 목록을 하려는 경우에 따라, 텍스트 파일에 복사 합니다. "수행" 탭을 찾아서 선택 "목록 가져오기" 파일을 선택 합니다. 목록 보기에는 XUIDs 채워야 합니다. "변경 내용 커밋" 클릭 사용자를 따라야 합니다.

## <a name="how-do-i-block-someone"></a>누군가 차단지 않습니다는 방법
"사람" 탭으로 이동 하는 사용자 또는 차단 하려는 사용자를 선택 합니다. "차단" 단추를 누르고 일괄 처리에서 차단 될 수 있습니다. 오류를 발생 하면 단순히 나중에 다시 시도 합니다.

## <a name="how-do-i-change-my-dev-accounts-repuation"></a>내 개발자 계정의 신뢰도 변경 하려면 어떻게 해야 하나요?
이동 "평판" 탭 기본값을 하는 요청을 제출 하려면 "변경 내용 커밋" 단추를 눌러 선택 합니다. 값이 변경 요청이 성공한 경우 평판 상태를 볼 수 있습니다.

## <a name="what-do-the-values-in-the-reputation-tab-mean"></a>평판 탭의 값은 무엇을 의미 합니까?
평판 세 개의 하위 평판에서 계산 되는 전체: Fairplay (멀티 플레이 사항), 사용자가 생성 (비디오 클립 등), 콘텐츠 및 통신 (메시지 및 음성). 각 범주에 대 한 원시 값 까지입니다 0에서 75, 사용자의 신뢰도 높은 것을 의미 하는 더 나은. OverallStatIsBad는 사용자에 게 "방지 Me" 신뢰도 알려 줍니다.

## <a name="whats-the-black-area-at-the-bottom"></a>맨 아래에 검은색 영역 란?
일을 유지 하기 위해 것 유용한 디버그 출력 UI에서 인쇄 생각 합니다. 영역 도구 란 최대 알려 되며 모든 오류를 인쇄에 실행 됩니다.

## <a name="wheres-my-gamerpic"></a>Gamerpic 내 어디 입니까?
우리 인식 버그를 일부 개발자 계정 계정 생성 시 자동으로 생성 된 gamerpic을 받을 수 없습니다.

## <a name="why-are-things-happening-so-slowly"></a>왜 사항이 발생 하므로 천천히?
(예: 팔로 워 추가)를 일괄 처리 작업 도구 거 대 한 요청 크기를 방지 하기 위해 일괄 처리를 자동으로 수행 합니다.

## <a name="how-do-i-run-xbox-live-account-tool"></a>Xbox Live 계정 도구를 실행 하려면 어떻게 해야 하나요?
Xbox Live SDK 폴더로 추출 하 고 Tools\XboxLiveAccountTool.exe 파일 실행을 두 번 클릭 합니다.

## <a name="i-have-a-feature-request-how-do-i-get-my-feature-incorporated"></a>특정 기능을 요청을 했습니다! 통합 my 기능 다운로드 어떻게 해야 하나요?
모든 기능 요청을 사용 하 여 Microsoft 담당자에 게 문의 하 고 수행할 수 있는 것으로 표시 됩니다.
