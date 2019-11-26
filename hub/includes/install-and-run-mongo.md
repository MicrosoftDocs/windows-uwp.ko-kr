---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: f594600991f08a7dfda784ae127be2e6438dacbd
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314887"
---
MongoDB를 설치하려면 다음을 수행합니다.

1. WSL 터미널을 엽니다(예: Ubuntu 18.04).
2. Ubuntu 패키지를 업데이트합니다. `sudo apt update`
3. 패키지가 업데이트되면 `sudo apt-get install mongodb`를 사용하여 MongoDB를 설치합니다.
4. 설치를 확인하고 버전 번호(`mongod --version`)를 가져옵니다.

MongoDB가 설치되면 다음 세 가지 명령을 알고 있어야 합니다.

1. `sudo service mongodb status`: 데이터베이스의 상태 확인
2. `sudo service mongodb start`: 데이터베이스 실행 시작
3. `sudo service mongodb stop`: 데이터베이스 실행 중지

> [!NOTE]
> 자습서 또는 문서에 사용된 `sudo systemctl status mongodb` 명령이 표시될 수 있습니다. WSL은 경량 상태를 유지하기 위해 `systemd`(Linux의 서비스 관리 시스템)를 포함하지 않습니다. 대신 SysVinit를 사용하여 머신에서 서비스를 시작합니다. 차이점은 볼 수 없지만 자습서에서 `sudo systemctl` 사용을 권장하는 경우 대신 `sudo /etc/init.d/`를 사용합니다. 예를 들어 WSL에 대한 `sudo systemctl status mongodb`는 `sudo /etc/inid.d/mongodb status`가 될 수 있습니다. 또는 `sudo service mongodb status`를 사용할 수도 있습니다.

### <a name="run-your-mongo-database-in-a-local-server"></a>로컬 서버에서 Mongo 데이터베이스 실행

1. 데이터베이스의 상태 확인: `sudo service mongodb status` 데이터베이스를 아직 시작하지 않은 경우 [Fail] 응답이 표시되어야 합니다.

2. 데이터베이스 시작: `sudo service mongodb start` 이제 [OK] 응답이 표시되어야 합니다.

3. 데이터베이스 서버에 연결하고 진단 명령을 실행하여 확인합니다. `mongo --eval 'db.runCommand({ connectionStatus: 1 })'` 그러면 현재 데이터베이스 버전, 서버 주소 및 포트, 상태 명령의 출력이 출력됩니다. 응답의 “확인” 필드에 대한 `1` 값은 서버가 작동하고 있음을 나타냅니다.

4. MongoDB 서비스가 실행되는 것을 중지하려면 `sudo service mongodb stop`을 입력합니다.

> [!NOTE]
> MongoDB에는 데이터를 /data/db에 저장하고 포트 27017에서 실행하는 등의 몇 가지 기본 매개 변수가 있습니다. 또한 `mongod`는 디먼(데이터베이스에 대한 호스트 프로세스)이고 `mongo`는 `mongod`의 특정 인스턴스에 연결하는 명령줄 셸입니다.
