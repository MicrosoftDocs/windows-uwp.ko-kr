---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: aa1da2a65f95d92e895533ed37426b5e454c255b
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "72314897"
---
PostgreSQL을 설치하려면 다음을 수행합니다.

1. WSL 터미널을 엽니다(예: Ubuntu 18.04).
2. Ubuntu 패키지를 업데이트합니다. `sudo apt update`
3. 패키지가 업데이트된 후에는 PostgreSQL(몇 가지 유용한 유틸리티가 포함된 -contrib 패키지)을 설치합니다. `sudo apt install postgresql postgresql-contrib`
4. 설치를 확인하고 버전 번호(`psql --version`)를 가져옵니다.

PostgreSQL이 설치되면 다음 세 가지 명령을 알고 있어야 합니다.

1. `sudo service postgresql status`: 데이터베이스의 상태 확인
2. `sudo service postgresql start`: 데이터베이스 실행 시작
3. `sudo service postgresql stop`: 데이터베이스 실행 중지

### <a name="postgresql-user-setup"></a>PostgreSQL 사용자 설정

기본 관리자 사용자 `postgres`에는 데이터베이스에 연결하기 위해 할당된 암호가 필요합니다. 암호를 설정하려면 다음을 수행합니다.

1. `sudo passwd postgres` 명령을 입력합니다.
2. 새 암호를 입력하라는 메시지가 표시됩니다.
3. 터미널을 닫았다가 다시 엽니다.

### <a name="run-postgresql-with-psql-shell"></a>psql 셸을 사용하여 PostgreSQL 실행

[psql](https://www.postgresql.org/docs/10/app-psql.html)은 PostgreSQL에 대한 터미널 기반 프런트 엔드입니다. 이 기능을 사용하면 쿼리를 대화형으로 입력하고, PostgreSQL에 발급하고, 쿼리 결과를 볼 수 있습니다. 또는 파일에서 입력할 수 있습니다. 또한 스크립트를 작성하고 다양한 작업을 자동화하는 데 도움이 되는 여러 메타 명령과 다양한 셸 형태의 기능을 제공합니다.

psql 셸을 시작하려면 다음을 수행합니다.

1. Postgres 서비스를 시작합니다. `sudo service postgresql start`
2. Postgres 서비스에 연결하고 psql 셸을 엽니다. `sudo -u postgres psql`

psql 셸을 성공적으로 입력하면 명령줄 변경 내용이 다음과 같이 표시됩니다. `postgres=#`

> [!NOTE]
> 또는 `su - postgres`를 사용하여 postgres 사용자로 전환한 다음, `psql` 명령을 입력하여 psql 셸을 열 수 있습니다.

postgres를 종료하려면 `\q`를 입력하거나 바로 가기 키를 사용합니다. Ctrl+D

PostgreSQL 설치에 생성된 사용자 계정을 확인하려면 WSL 터미널에서 `psql -c "\du"`를 사용하거나 psql 셸이 열려 있는 경우 `\du`를 사용합니다. 이 명령은 계정 사용자 이름, 역할 특성 목록 및 역할 그룹의 구성원 열을 표시합니다. 명령줄로 돌아가려면 `q`를 입력합니다.
