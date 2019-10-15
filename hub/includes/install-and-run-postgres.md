---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: aa1da2a65f95d92e895533ed37426b5e454c255b
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314897"
---
PostgreSQL를 설치 하려면:

1. WSL 터미널을 엽니다 (ie. Ubuntu 18.04).
2. Ubuntu 패키지 업데이트: `sudo apt update`
3. 패키지가 업데이트 되 면 PostgreSQL를 설치 하 고, 다음과 @no__t 같은 몇 가지 유용한 유틸리티를 포함 하는-를 설치 합니다.
4. 설치를 확인 하 고 버전 번호를 가져옵니다. `psql --version`

PostgreSQL가 설치 되 면 다음 세 가지 명령을 알고 있어야 합니다.

1. 데이터베이스의 상태를 확인 하려면-0을 @no__t 합니다.
2. `sudo service postgresql start`을 실행 하 여 데이터베이스 실행을 시작 합니다.
3. -0을 @no__t 하 여 데이터베이스 실행을 중지 합니다.

### <a name="postgresql-user-setup"></a>PostgreSQL 사용자 설정

기본 admin 사용자 `postgres`에는 데이터베이스에 연결 하기 위해 할당 된 암호가 필요 합니다. 암호를 설정 하려면:

1. @No__t-0 명령을 입력 합니다.
2. 새 암호를 입력 하 라는 메시지가 표시 됩니다.
3. 터미널을 닫았다가 다시 엽니다.

### <a name="run-postgresql-with-psql-shell"></a>Psql shell을 사용 하 여 PostgreSQL 실행

[psql](https://www.postgresql.org/docs/10/app-psql.html) 은 PostgreSQL에 대 한 터미널 기반 프런트 엔드입니다. 이 기능을 사용 하면 쿼리를 대화형으로 입력 하 고, PostgreSQL에 발급 하 고, 쿼리 결과를 볼 수 있습니다. 또는 파일에서 입력할 수 있습니다. 또한 스크립트를 작성 하 고 다양 한 작업을 자동화 하는 데 도움이 되는 다양 한 메타 명령과 다양 한 셸 형태의 기능을 제공 합니다.

Psql 셸을 시작 하려면:

1. Postgres service 시작: `sudo service postgresql start`
2. Postgres service에 연결 하 고 psql shell을 엽니다. `sudo -u postgres psql`

Psql 셸을 성공적으로 입력 하면 명령줄 변경 내용이 다음과 같이 표시 됩니다. `postgres=#`

> [!NOTE]
> 또는 다음을 사용 하 여 postgres 사용자로 전환한 다음: `su - postgres`을 입력 하 고 `psql` 명령을 입력 하 여 psql 셸을 열 수 있습니다.

Postgres = # enter: `\q`을 종료 하거나 바로 가기 키를 사용 합니다. Ctrl + D

PostgreSQL 설치에 생성 된 사용자 계정을 확인 하려면 WSL 터미널에서를 사용 합니다. `psql -c "\du"` ... 또는 psql shell이 열려 있는 경우에만 @no__t 합니다. 이 명령은 열을 표시 합니다. 계정 사용자 이름, 역할 특성 목록 및 역할 그룹의 구성원입니다. 명령줄로 돌아가려면 `q`을 입력 합니다.
