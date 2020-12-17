---
title: Windows 10 용 Powertoy 관리자 모드
description: Powertoy가 관리자 모드에서 실행 되는 앱과 함께 작동 하려면 관리자 모드 에서도 Powertoy를 실행 해야 합니다.
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2eb514c25c135f8d58641232523a32f5d35153d4
ms.sourcegitcommit: 46a7e9db64e17a645ee6e888f62a9b04632c56af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97618542"
---
# <a name="powertoys-running-with-administrator-elevated-permissions"></a>관리자 권한으로 실행 되는 Powertoy

관리자 권한 (상승 된 권한이 라고도 함)으로 응용 프로그램을 실행 하는 경우에는 상승 된 응용 프로그램이 포커스가 있거나 Fpowertoy Yzones와 같은 Powertoy 기능과 상호 작용 하려고 할 때 제대로 작동 하지 않을 수 있습니다. 이 문제는 관리자 권한으로 Powertoy를 실행 하 여 해결할 수 있습니다.

## <a name="options"></a>옵션

Powertoy는 관리자 권한으로 실행 되는 응용 프로그램을 지원 하기 위해 두 가지 옵션이 있습니다 (상승 된 권한 사용).

- **[권장]**: powertoy는 상승 된 프로세스가 감지 되 면 프롬프트를 표시 합니다. **Powertoy 설정** 을 엽니다. **일반** 탭 내에서 "관리자 권한으로 다시 시작"을 선택 합니다.

- **Powertoy 설정** 에서 "항상 관리자 권한으로 실행"을 사용 하도록 설정 합니다.

## <a name="run-as-administrator-elevated-processes-explained"></a>관리자 권한으로 실행 된 관리자 권한으로 실행

Windows 응용 프로그램은 기본적으로 **사용자 모드** 에서 실행 됩니다. **관리 모드** 에서 또는 *관리자 권한* 으로 응용 프로그램을 실행 하는 경우 운영 체제에 대 한 추가 액세스 권한으로 앱이 실행 됩니다.

관리 모드에서 응용 프로그램 또는 프로그램을 실행 하는 가장 간단한 방법은 프로그램을 마우스 오른쪽 단추로 클릭 하 고 **관리자 권한으로 실행** 을 선택 하는 것입니다. 현재 사용자가 관리자가 아닌 경우 Windows는 관리자 사용자 이름과 암호를 요청 합니다.

대부분의 앱은 상승 된 권한으로 실행할 필요가 없습니다. 그러나 일반적으로 관리자 권한이 필요한 경우에는 특정 PowerShell 명령을 실행 하거나 레지스트리를 편집 해야 합니다.

이 메시지가 표시 되 면 (사용자 Access Control 프롬프트) 응용 프로그램에서 관리자 수준 상승 된 권한을 요청 합니다.

![Windows 관리자 권한 프롬프트 스크린샷](../images/pt-admin-prompt.png)

관리자 권한 명령줄의 경우 일반적으로 제목 표시줄에 "관리자" 라는 제목이 추가 됩니다.

![Windows 관리 명령줄 스크린샷](../images/pt-admin-terminal.png)

## <a name="support-for-admin-mode-with-powertoys"></a>Powertoy를 사용 하 여 관리 모드 지원

Powertoy은 관리자 모드에서 실행 되는 다른 응용 프로그램과 상호 작용할 때 상승 된 관리자 권한만 필요 합니다. 이러한 응용 프로그램이 집중 되어 있는 경우에는 Powertoy가 상승 되지 않은 경우에도 작동 하지 않을 수 있습니다.

다음은에서 작동 하지 않는 두 가지 시나리오입니다.

- 특정 형식의 키보드 스트로크 가로채기
- 창 크기 조정/이동

### <a name="affected-powertoys-utilities"></a>영향을 받는 Powertoy 유틸리티

다음 시나리오에서는 관리자 모드 권한이 필요할 수 있습니다.

- FancyZones
  - 고급 영역에 관리자 권한 창 (예: 명령 프롬프트) 맞추기
  - 관리자 권한 창을 다른 영역으로 이동
- 바로 가기 가이드
  - 바로 가기 표시
- 키보드 다시 매퍼
  - 키 다시 매핑 키
  - 전역 수준 바로 가기 다시 매핑
  - 앱 대상 바로 가기 다시 매핑
- Powertoy 실행
  - 바로 가기 표시
