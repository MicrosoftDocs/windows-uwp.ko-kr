---
title: 데이터베이스를 사용하여 Windows에서 Python 사용 시작
description: Windows에서 Python으로 PostgreSQL 또는 MongoDB 사용을 시작하는 데 도움이 되는 가이드입니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, postgresql, mongodb, postgres, mongo, microsoft, windows의 python, windows에서 postgresql 설치, windows에서 mongodb 설치, python으로 postgresql 사용, python으로 mongodb 사용, WSL의 postgresql, WSL의 mongodb
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 9b1bdea86739f3d58b39cf7f0e6b8090474886f3
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "72517783"
---
# <a name="get-started-using-postgresql-or-mongodb-with-python-on-windows"></a>Windows에서 Python으로 PostgreSQL 또는 MongoDB 사용 시작

이 단계별 가이드는 Python 앱을 데이터베이스에 연결하기 시작하는 데 도움이 됩니다. 가장 널리 사용되는 두 가지 옵션인 PostgreSQL과 MongoDB를 중점적으로 살펴보겠습니다.

## <a name="differences-between-mongodb-and-postgresql"></a>MongoDB와 PostgreSQL의 차이점

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

> [!NOTE]
> 사용 중인 프레임워크 및 도구를 특정 데이터베이스 시스템에 통합하는 방법을 고려해야 할 수도 있습니다. [Django 웹 프레임워크](./web-frameworks.md#hello-world-tutorial-for-django)는 PostgreSQL와 더 잘 통합될 수 있습니다([Django 문서](https://docs.djangoproject.com/en/2.2/ref/contrib/postgres/) 및 [psycopg2](https://github.com/psycopg/psycopg2) 참조). [Flask 웹 프레임워크](./web-frameworks.md#hello-world-tutorial-for-flask)는 MongoDB와 더 잘 통합될 수 있습니다([MongoEngine](https://github.com/MongoEngine/flask-mongoengine) 및 [PyMongo](https://github.com/dcrosta/flask-pymongo) 참조).

## <a name="install-postgresql"></a>PostgreSQL 설치

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>PostgreSQL을 위한 VS Code 지원

VS Code는 [PostgreSQL 확장](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql)을 통해 PostgreSQL 데이터베이스 작업을 지원합니다. VS Code 내에서 PostgreSQL 데이터베이스를 만들고, 연결하고, 관리하고, 쿼리할 수 있습니다.

## <a name="install-mongodb"></a>MongoDB 설치

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>MongoDB를 위한 VS Code 지원

VS Code는 [Azure CosmosDB 확장](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)을 통해 MongoDB 데이터베이스 작업을 지원합니다. VS Code 내에서 MongoDB 데이터베이스를 만들고, 연결하고, 관리하고, 쿼리할 수 있습니다.

자세히 알아보려면 VS Code 설명서: [MongoDB 작업](https://code.visualstudio.com/docs/azure/mongodb)을 방문해보세요.

## <a name="set-up-profile-aliases"></a>프로필 별칭 설정

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
