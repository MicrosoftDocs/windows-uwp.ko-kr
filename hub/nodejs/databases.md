---
title: Node.js 앱을 데이터베이스에 연결 하기 시작
description: Windows의 데이터베이스에 node.js 앱을 연결 하는 작업을 시작 합니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, node.js, windows 10, microsoft, learning NodeJS, windows의 노드, windows의 노드, windows에서 노드 설치, windows에서 노드 설치, windows에서 노드를 사용 하 여 개발, windows에서 NodeJS를 사용 하 여 개발, windows에서 노드 설치, windows의 NodeJS Linux 용 하위 시스템
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: bdc3e3c944c4aeb25f5cf880fc4d31df1019da5a
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72315117"
---
# <a name="get-started-connecting-nodejs-apps-to-a-database"></a>Node.js 앱을 데이터베이스에 연결 하기 시작

Node.js 응용 프로그램은 종종 파일, 로컬 저장소, 클라우드 서비스 또는 데이터베이스를 통해 발생할 수 있는 데이터를 유지 해야 합니다. 이 단계별 가이드는 node.js 앱을 데이터베이스에 연결 하기 시작 하는 데 도움이 됩니다. 가장 널리 사용 되는 두 가지 옵션을 중점적으로 선택 했습니다. MongoDB 및 PostgreSQL.

## <a name="prerequisites"></a>사전 요구 사항

이 가이드에서는 다음을 포함 하 여 [WSL 2를 사용 하 여 node.js 개발 환경을 설정](./setup-on-wsl2.md)하는 단계를 이미 완료 했다고 가정 합니다.

- Windows 10 Insider Preview 빌드 18932 이상 버전을 설치 합니다.
- Windows에서 WSL 2 기능을 사용 하도록 설정 합니다.
- Linux 배포 (Ubuntu 18.04)를 설치 합니다. @No__t-0으로 확인할 수 있습니다.
- Ubuntu 18.04 배포가 WSL 2 모드로 실행 되 고 있는지 확인 합니다. WSL은 v1 또는 v2 모드에서 배포를 실행할 수 있습니다. PowerShell을 열고 다음을 입력 하 여이를 확인할 수 있습니다. `wsl -l -v`
- PowerShell을 사용 하 여 `wsl -s ubuntu 18.04`을 사용 하 여 Ubuntu 18.04을 기본 배포로 설정 합니다.

## <a name="differences-between-mongodb-and-postgresql"></a>MongoDB와 PostgreSQL의 차이점

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

## <a name="install-mongodb"></a>MongoDB 설치

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>MongoDB에 대 한 VS Code 지원

[Azure CosmosDB 확장](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)을 통해 MongoDB 데이터베이스 작업을 지원 VS Code VS Code 내에서 MongoDB 데이터베이스를 만들고 관리 하 고 쿼리할 수 있습니다.

자세히 알아보려면 VS Code 문서를 참조 하세요. [MongoDB 사용](https://code.visualstudio.com/docs/azure/mongodb).

MongoDB 문서에서 자세히 알아보세요.

- [MongoDB 사용 소개](https://docs.mongodb.com/manual/introduction/)
- [사용자 만들기](https://docs.mongodb.com/manual/tutorial/create-users/)
- [원격 호스트에서 MongoDB 인스턴스에 연결](https://docs.mongodb.com/manual/mongo/#mongodb-instance-on-a-remote-host)
- [CRUD: Create, Read, Update, Delete @ no__t-0
- [참조 문서](https://docs.mongodb.com/manual/reference/)

## <a name="install-postgresql"></a>PostgreSQL 설치

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>PostgreSQL에 대 한 VS Code 지원

VS Code [PostgreSQL 확장](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql)을 통해 PostgreSQL 데이터베이스 작업을 지원 하므로 VS Code 내에서 PostgreSQL 데이터베이스를 만들고, 연결 하 고, 관리 하 고, 쿼리할 수 있습니다.

## <a name="set-up-profile-aliases"></a>프로필 별칭 설정

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
