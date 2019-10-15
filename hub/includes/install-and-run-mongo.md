---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: f594600991f08a7dfda784ae127be2e6438dacbd
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314887"
---
MongoDB를 설치 하려면:

1. WSL 터미널을 엽니다 (ie. Ubuntu 18.04).
2. Ubuntu 패키지 업데이트: `sudo apt update`
3. 패키지가 업데이트 되 면 MongoDB를 사용 하 여 설치 합니다. `sudo apt-get install mongodb`
4. 설치를 확인 하 고 버전 번호를 가져옵니다. `mongod --version`

MongoDB가 설치 되 면 다음 세 가지 명령을 알고 있어야 합니다.

1. 데이터베이스의 상태를 확인 하려면-0을 @no__t 합니다.
2. `sudo service mongodb start`을 실행 하 여 데이터베이스 실행을 시작 합니다.
3. -0을 @no__t 하 여 데이터베이스 실행을 중지 합니다.

> [!NOTE]
> 자습서 또는 문서에 사용 되는 `sudo systemctl status mongodb` 명령이 표시 될 수 있습니다. 경량으로 유지 하기 위해 WSL에는 @no__t (Linux의 서비스 관리 시스템)가 포함 되지 않습니다. 대신 SysVinit를 사용 하 여 컴퓨터에서 서비스를 시작 합니다. 차이점은 볼 수 없지만 자습서에서 `sudo systemctl`을 사용 하는 것이 권장 되는 경우 `sudo /etc/init.d/`을 대신 사용 합니다. 예를 들어 WSL에 대 한 `sudo systemctl status mongodb`은 `sudo /etc/inid.d/mongodb status` ... 또는 `sudo service mongodb status`를 사용할 수도 있습니다.

### <a name="run-your-mongo-database-in-a-local-server"></a>로컬 서버에서 Mongo 데이터베이스 실행

1. 데이터베이스의 상태를 확인 합니다. @no__t 데이터베이스를 아직 시작 하지 않은 경우 [Fail] 응답이 표시 되어야 합니다.

2. 데이터베이스를 시작 합니다. `sudo service mongodb start` 이제 [OK] 응답이 표시 됩니다.

3. 데이터베이스 서버에 연결 하 고 진단 명령을 실행 하 여 확인 합니다. `mongo --eval 'db.runCommand({ connectionStatus: 1 })'`은 현재 데이터베이스 버전, 서버 주소 및 포트, 상태 명령의 출력을 출력 합니다. 응답의 "확인" 필드 값 `1`은 서버가 작동 하 고 있음을 나타냅니다.

4. MongoDB 서비스가 실행 되는 것을 중지 하려면 다음을 입력 합니다. `sudo service mongodb stop`

> [!NOTE]
> MongoDB에는 데이터를/data/db에 저장 하 고 포트 27017에서 실행 하는 등의 몇 가지 기본 매개 변수가 있습니다. 또한 `mongod`은 디먼 (데이터베이스에 대 한 호스트 프로세스)이 고 `mongo`은 `mongod`의 특정 인스턴스에 연결 하는 명령줄 셸입니다.
