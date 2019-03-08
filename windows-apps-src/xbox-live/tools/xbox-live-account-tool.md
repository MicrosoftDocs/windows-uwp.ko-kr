---
title: Xbox Live 계정 도구
description: Xbox Live 계정을 도구를 Xbox Live를 사용 하도록 설정 제목 테스트용 테스트 계정을 빠르게 만들기를 사용 하는 방법을 알아봅니다.
ms.assetid: ec5959f9-1c60-4aa4-94a6-5d8bdcf77a96
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox, 테스트, 테스트 계정
ms.localizationpriority: medium
ms.openlocfilehash: dead4e62e41b7b597ba9a578ee8f174386529937
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632828"
---
# <a name="xbox-live-account-tool"></a>Xbox Live 계정 도구

## <a name="what-is-xbox-live-account-tool"></a>Xbox Live 계정을 도구는 무엇입니까?
Xbox Live 계정을 도구는 title 개발자가 게임 시나리오를 테스트 하는 것에 대 한 기존 개발자 계정을 설정할 수 있도록 설계 된 도구. 예를 들어, Xbox Live 계정을 도구를 사용 하 여 개발 계정의 게이머 태그를 변경 하 하거나 신속 하 게 계정의 친구 목록에 1000 팔로 워를 추가할 수 있습니다.

## <a name="what-can-i-do-with-xbox-live-account-tool"></a>Xbox Live 계정을 도구를 사용 하 여 어떻게 해야 합니까?
다음을 수행할 수 있습니다.
  1. 사용자의 프로필 설정, XUID, 및 현재 권한 보기
  2. 팔로 워 목록을 사용자의 소셜 그래프에는 Xbox 개발자 플랫폼 csv 또는 텍스트 파일에 추가
  3. 사용자의 친구 목록을 관리: 즐겨찾기를 즐겨찾기에서 제거할, 차단, 사용자 및 차단 해제를 수행 하 고 하는 경우를 따릅니까 있습니다 다시 참조 하세요.
  4. 개발 사용자의 평판을 변경 (및 원시 평판 상태 값이 즉시 표시)
  5. 사용자의 게이머 태그를 변경 합니다.

## <a name="where-can-i-find-xbox-live-account-tool"></a>Xbox Live 계정을 도구 찾기
Xbox Live 계정을 도구에서 Xbox Live 도구 패키지의 일부로 찾을 수 있습니다 [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools)합니다.

## <a name="how-do-i-log-in"></a>기록 하려면 어떻게 해야 하나요?
관리 하 고 올바른 샌드박스를 지정 하려는 사용자의 자격 증명이 필요 합니다. 개발자 계정에는 샌드박스에 대 한 액세스 권한이 그렇지 않으면 로그인 실패 수 있는지 확인 합니다. 이 도구는 염두에서 샌드박스를 사용 하 여 개발자 계정으로 설계 되었습니다.

## <a name="can-i-use-a-retail-account-or-does-it-have-to-be-a-sandboxed-account"></a>샌드박스 계정으로 있습니까 또는 소매 계정에 사용할 수 있습니까?
Xbox Live 계정을 도구 소매 계정 관리를 사용할 수 있습니다 있지만 일부 기능이 지원 됩니다. 예를 들어 소매 사용자의 평판을 변경할 수 없습니다.

## <a name="how-do-i-change-a-dev-users-gamertag"></a>개발 사용자의 게이머 태그를 변경 하려면 어떻게 해야 합니까?
"게이머 태그" 탭으로 이동한 후 게이머 태그를 입력 합니다. 게이머 태그는 숫자, 문자 및 공백을 포함 해야 하 고 15 자까지 될 수 있습니다. 개발자 계정 게이머 태그 2로 시작 해야 합니다. 현재 하나의 변경만 지원 됩니다.

## <a name="how-do-i-see-my-block-list"></a>블록 목록을 보려면 어떻게 해야 합니까?
"People" 탭으로 이동은 현재 차단 하는 사용자가 정렬 하기 위해 "차단 됨" 열 머리글을 선택 합니다.

## <a name="how-do-i-follow-a-large-group-of-users"></a>대규모 사용자 그룹을 지키는 수행 하는 방법
XUIDs 수행 하려는 목록에 있으면 텍스트 파일로 복사 합니다. "팔 로우" 탭으로 이동한 "Import list"를 선택 합니다. 파일을 선택 합니다. 목록 뷰에서 XUIDs 채워야 합니다. "변경 내용 커밋"를 클릭 합니다. 사용자를 따라야 합니다.

## <a name="how-do-i-block-someone"></a>어떻게 차단 누군가가 하나요?
"People" 탭으로 이동한 다음 사용자를 차단 하려는 사용자를 선택 합니다. "블록" 단추를 누르고 일괄 처리에서 차단 될 수 있습니다. 오류를 발견 하는 경우 단순히 나중에 다시 시도 합니다.

## <a name="how-do-i-change-my-dev-accounts-repuation"></a>내 개발 계정의 신뢰도 변경 하려면 어떻게 해야 합니까?
"평판" 탭으로 이동 합니다. 하는 요청을 제출 하려면 "변경 내용 커밋" 단추를 눌러 기본값을 선택 합니다. 값이 변경 요청에 성공한 경우 평판 stat을 볼 수 있습니다.

## <a name="what-do-the-values-in-the-reputation-tab-mean"></a>평판 탭의 값은 무엇을 의미 하나요?
전체 평판 세 하위 있도록에서 계산 됩니다. Fairplay (멀티 플레이 수행), 비디오 클립과 같은 사용자 생성 콘텐츠 및 통신 (메시지 및 음성). 각 범주에 대 한 원시 값은 향상 된 사용자의 신뢰도 높은 의미 있는 0에서 75 범위 수 있습니다. OverallStatIsBad 사용자에 게 "방지 Me" 평판 있는지를 알려 줍니다.

## <a name="whats-the-black-area-at-the-bottom"></a>아래쪽에 검은색 영역 이란?
을 유지 하기 위해 새로운 내용을 알려 드리고는 유용 디버그 출력 UI에 인쇄 하는 것이 생각 합니다. 영역을 기준으로 삼을 어떤 도구를를 실행 하는 모든 오류를 인쇄 합니다.

## <a name="wheres-my-gamerpic"></a>내 gamerpic는 어디 입니까?
일부 개발자 계정에 계정 생성 시 자동으로 생성 된 gamerpic를 받지 못한 버그 인식 하 합니다.

## <a name="why-are-things-happening-so-slowly"></a>왜 작업 발생 하는 매우 천천히?
일괄 처리 작업 (예: 팔로 워를 추가)에 대 한 도구를 자동으로 큰 요청 크기를 방지 하기 위해 일괄 처리를 수행 합니다.

## <a name="how-do-i-run-xbox-live-account-tool"></a>Xbox Live 계정을 도구를 실행 하려면 어떻게 해야 하나요?
Xbox Live SDK 폴더에 추출 및 실행 하려면 Tools\XboxLiveAccountTool.exe 파일을 두 번 클릭 합니다.

## <a name="i-have-a-feature-request-how-do-i-get-my-feature-incorporated"></a>기능 요청이 있습니다. 통합 기능을 어떻게 받나요?
기능 요청을 사용 하 여 Microsoft 담당자에 문의 하 고 수행할 수 있는 살펴보겠습니다.
