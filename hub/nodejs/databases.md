---
title: Node.js 앱을 데이터베이스에 연결
description: Windows에서 Node.js 앱을 데이터베이스에 연결합니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, nodejs 학습, windows의 노드, wsl의 노드, windows 기반 linux의 노드, windows에 노드 설치, vs code를 사용하는 nodejs, windows에서 노드를 사용하여 개발, windows에서 nodejs를 사용하여 개발, WSL에 노드 설치, Linux용 Windows 하위 시스템의 NodeJS
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 63c47107538d8744201f83ea1be24cfaf3193f4f
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517821"
---
# <a name="get-started-using-mongodb-or-postgresql-with-nodejs-on-windows"></a>Windows의 Node.js에서 MongoDB 또는 PostgreSQL 사용

Node.js 애플리케이션은 종종 파일, 로컬 스토리지, 클라우드 서비스 또는 데이터베이스를 통해 발생할 수 있는 데이터를 유지해야 합니다. 이 단계별 가이드는 Node.js 앱을 데이터베이스에 연결하기 시작하는 데 도움이 됩니다. 가장 널리 사용되는 두 가지 옵션인 MongoDB 및 PostgreSQL을 중점적으로 살펴보겠습니다.

## <a name="prerequisites"></a>필수 구성 요소

이 가이드에서는 다음을 포함하여 [WSL 2를 사용하여 Node.js 개발 환경 설치](./setup-on-wsl2.md) 단계를 이미 완료했다고 가정합니다.

- Windows 10 Insider Preview 빌드 18932 이상 설치
- Windows에서 WSL 2 기능을 사용하도록 설정
- Linux 배포판(이 예제에서는 Ubuntu 18.04) 설치. `wsl lsb_release -a`를 사용하여 확인할 수 있습니다.
- Ubuntu 18.04 배포판이 WSL 2 모드로 실행되는지 확인. (WSL은 배포판을 v1 또는 v2 모드로 실행할 수 있습니다.) PowerShell을 열고 `wsl -l -v`를 입력하여 확인할 수 있습니다.
- PowerShell에서 `wsl -s ubuntu 18.04`를 사용하여 Ubuntu 18.04를 기본 배포로 설정

## <a name="differences-between-mongodb-and-postgresql"></a>MongoDB와 PostgreSQL의 차이점

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

## <a name="install-mongodb"></a>MongoDB 설치

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>MongoDB를 위한 VS Code 지원

VS Code는 [Azure CosmosDB 확장](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)을 통해 MongoDB 데이터베이스 작업을 지원합니다. VS Code 내에서 MongoDB 데이터베이스를 만들고, 관리하고, 쿼리할 수 있습니다.

자세히 알아보려면 VS Code 설명서: [MongoDB 작업](https://code.visualstudio.com/docs/azure/mongodb)을 방문해보세요.

다음 MongoDB 문서에서 자세히 알아보세요.

- [MongoDB 사용 소개](https://docs.mongodb.com/manual/introduction/)
- [사용자 만들기](https://docs.mongodb.com/manual/tutorial/create-users/)
- [원격 호스트에서 MongoDB 인스턴스에 연결](https://docs.mongodb.com/manual/mongo/#mongodb-instance-on-a-remote-host)
- [CRUD: 만들기, 읽기, 업데이트, 삭제](https://docs.mongodb.com/manual/crud/)
- [참조 문서](https://docs.mongodb.com/manual/reference/)

## <a name="install-postgresql"></a>PostgreSQL 설치

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>PostgreSQL을 위한 VS Code 지원

VS Code는 [PostgreSQL 확장](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql)을 통해 PostgreSQL 데이터베이스 작업을 지원합니다. VS Code 내에서 PostgreSQL 데이터베이스를 만들고, 연결하고, 관리하고, 쿼리할 수 있습니다.

## <a name="set-up-profile-aliases"></a>프로필 별칭 설정

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
