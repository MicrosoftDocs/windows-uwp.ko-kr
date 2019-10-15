---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 2f7a57f1652ecab81a70c39faa1b70c42ed6a3de
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314907"
---
@No__t-0 또는 `sudo service postgres start`, `sudo -u postgrest psql`를 입력 하는 것이 지루한 경우  그러나 WSL의 `.profile` 파일에 별칭을 설정 하 여 이러한 명령을 사용 하 고 쉽게 기억할 수 있도록 하는 것을 고려할 수 있습니다. 

이러한 명령을 실행 하기 위해 고유한 사용자 지정 별칭 또는 바로 가기를 설정 하려면 다음을 수행 합니다.

1. WSL 터미널을 열고 `cd ~`을 입력 하 여 루트 디렉터리에 있도록 합니다.
2. 터미널 텍스트 편집기 인 Nano: `sudo nano .profile`을 사용 하 여 터미널의 설정을 제어 하는 `.profile` 파일을 엽니다.
3. 파일의 아래쪽에서 (`# set PATH` 설정 변경 안 함) 다음을 추가 합니다.

    ```bash
    # My Aliases
    alias start-pg='sudo service postgresql start'
    alias run-pg='sudo -u postgres psql'
    ```

이렇게 하면 @no__t를 입력 하 여 postgresql 서비스 실행을 시작 하 @no__t 고-1을 사용 하 여 psql 셸을 열 수 있습니다. @No__t-0 및 `run-pg`을 원하는 이름으로 변경할 수 있습니다. postgres가 이미 사용 하는 명령을 덮어쓰지 않도록 주의 하세요.

4. 새 별칭을 추가 했으면 **Ctrl + X** 를 사용 하 여 Nano 텍스트 편집기를 종료 하 고, 저장 하 고 Enter 메시지가 표시 되 면 `Y` (예)를 선택 합니다 (파일 이름을 `.profile`로 유지).
5. WSL 터미널을 닫았다가 다시 연 다음 새 별칭 명령을 시도 합니다.
