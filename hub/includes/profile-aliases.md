---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 2f7a57f1652ecab81a70c39faa1b70c42ed6a3de
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "72314907"
---
`sudo service mongodb start` 또는 `sudo service postgres start` 및 `sudo -u postgrest psql`을 입력하는 것은 지루할 수 있습니다.  그러나 WSL의 `.profile` 파일에 별칭을 설정하면 이러한 명령으로 더 빠르게 사용하고 더 쉽게 기억할 수 있습니다. 

이러한 명령 실행을 위한 사용자 지정 별칭 또는 바로 가기를 설정하려면 다음을 수행합니다.

1. WSL 터미널을 열고 루트 디렉터리에 있도록 `cd ~`를 입력합니다.
2. 터미널 텍스트 편집기인 Nano를 사용하여 터미널의 설정을 제어하는 `.profile` 파일을 엽니다. `sudo nano .profile`
3. 파일의 아래쪽에서(`# set PATH` 설정을 변경하지 않음) 다음을 추가합니다.

    ```bash
    # My Aliases
    alias start-pg='sudo service postgresql start'
    alias run-pg='sudo -u postgres psql'
    ```

이렇게 하면 `start-pg`를 입력하여 postgresql 서비스 실행을 시작하고 `run-pg`를 입력하여 psql 셸을 열 수 있습니다. `start-pg` 및 `run-pg`를 원하는 이름으로 변경할 수 있습니다. postgres가 이미 사용하는 명령을 덮어쓰지 않도록 주의하세요.

4. 새 별칭을 추가한 후에는 **Ctrl+X**를 사용하여 Nano 텍스트 편집기를 종료합니다. 저장 및 Enter에 대한 메시지가 표시되면 `Y`(예)를 선택합니다(파일 이름을 `.profile`로 그대로 둠).
5. WSL 터미널을 닫았다가 다시 연 다음, 새 별칭 명령을 시도합니다.
