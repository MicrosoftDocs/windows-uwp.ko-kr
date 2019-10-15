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
ms.openlocfilehash: 42a257361cffec974d060a6518dfdf5254d62082
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314857"
---
# <a name="get-started-using-databases-with-python-on-windows"></a>Windows에서 Python을 사용 하 여 데이터베이스 사용 시작

Python 응용 프로그램은 파일, 로컬 저장소, 클라우드 서비스 또는 데이터베이스를 통해 발생할 수 있는 데이터를 유지 해야 하는 경우가 많습니다. 이 단계별 가이드는 Python 앱을 데이터베이스에 연결 하기 시작 하는 데 도움이 됩니다. 가장 널리 사용 되는 두 가지 옵션을 중점적으로 선택 했습니다. PostgreSQL 및 MongoDB.

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

자세히 알아보려면 VS Code 문서를 참조 하세요. [MongoDB 사용](https://code.visualstudio.com/docs/azure/mongodb).

## <a name="set-up-profile-aliases"></a>프로필 별칭 설정

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
