---
title: 데이터베이스를 사용 하 여 Windows에서 Python 사용 시작
description: Windows에서 Python과 함께 PostgreSQL 또는 MongoDB 사용을 시작 하는 데 도움이 되는 가이드입니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, postgresql, mongodb, postgres, mongo, microsoft, windows의 python, windows에서 postgresql 설치, windows에 mongodb 설치, python과 함께 postgresql 사용, python과 mongodb 사용, postgresql에서 mongodb 사용, on wsl, on WSL
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 9b1bdea86739f3d58b39cf7f0e6b8090474886f3
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517783"
---
# <a name="get-started-using-postgresql-or-mongodb-with-python-on-windows"></a>Windows에서 Python을 사용 하 여 PostgreSQL 또는 MongoDB 사용 시작

이 단계별 가이드는 Python 앱을 데이터베이스에 연결 하기 시작 하는 데 도움이 됩니다. PostgreSQL 및 MongoDB의 두 가지 인기 있는 옵션에 집중 하도록 선택 했습니다.

## <a name="differences-between-mongodb-and-postgresql"></a>MongoDB와 PostgreSQL의 차이점

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

> [!NOTE]
> 사용 중인 프레임 워크 및 도구를 특정 데이터베이스 시스템에 통합 하는 방법을 고려해 야 할 수도 있습니다. [Django 웹 프레임 워크](./web-frameworks.md#hello-world-tutorial-for-django) 는 PostgreSQL와 더 잘 통합 된 것 같습니다 ( [Django 문서](https://docs.djangoproject.com/en/2.2/ref/contrib/postgres/) 및 [psycopg2](https://github.com/psycopg/psycopg2)참조). [Flask 웹 프레임 워크](./web-frameworks.md#hello-world-tutorial-for-flask) 는 MongoDB와 더 잘 통합 된 것 같습니다 ( [MongoEngine](https://github.com/MongoEngine/flask-mongoengine) 및 [PyMongo](https://github.com/dcrosta/flask-pymongo)참조).

## <a name="install-postgresql"></a>PostgreSQL 설치

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>PostgreSQL에 대 한 VS Code 지원

VS Code [PostgreSQL 확장](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql)을 통해 PostgreSQL 데이터베이스 작업을 지원 하므로 VS Code 내에서 PostgreSQL 데이터베이스를 만들고, 연결 하 고, 관리 하 고, 쿼리할 수 있습니다.

## <a name="install-mongodb"></a>MongoDB 설치

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>MongoDB에 대 한 VS Code 지원

[Azure CosmosDB 확장](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)을 통해 MongoDB 데이터베이스 작업을 지원 VS Code VS Code 내에서 MongoDB 데이터베이스를 만들고, 연결 하 고, 관리 하 고, 쿼리할 수 있습니다.

자세히 알아보려면 [MongoDB 작업](https://code.visualstudio.com/docs/azure/mongodb): 문서를 VS Code 참조 하세요.

## <a name="set-up-profile-aliases"></a>프로필 별칭 설정

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
