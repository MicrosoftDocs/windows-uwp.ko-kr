---
title: UWP 앱에서 MongoDB 데이터베이스 사용
description: UWP(유니버설 Windows 플랫폼) 앱을 MongoDB 데이터베이스에 직접 연결하고 프로그래밍 방식으로 연결을 테스트하는 방법을 알아봅니다.
ms.date: 03/28/2019
ms.topic: article
keywords: windows 10, uwp, MongoDB, 데이터베이스
ms.localizationpriority: medium
ms.openlocfilehash: aaa035393e49d6bdad49faa806485cc51d21bb84
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043515"
---
# <a name="use-a-mongodb-database"></a>MongoDB 데이터베이스 사용
이 문서는 UWP 앱에서 MongoDB 데이터베이스를 사용하도록 설정하는 데 필요한 단계를 포함하고 있습니다. 코드로 데이터베이스와 상호 작용하는 방법을 보여주는 작은 코드 조각도 포함되어 있습니다.

## <a name="set-up-your-solution"></a>솔루션 설정

앱을 MongoDB 데이터베이스에 직접 연결하려면 프로젝트 최소 버전의 대상이 Fall Creators Update(빌드 16299)여야 합니다.  UWP 프로젝트의 속성 페이지에서 이에 대한 정보를 찾을 수 있습니다.

![Fall Creators Update에 설정된 대상 및 최소 버전을 보여주는 Visual Studio의 대상 속성 창 이미지](images/min-version-fall-creators.png)

**패키지 관리자 콘솔**을 엽니다(보기 -> 다른 Windows -> 패키지 관리자 콘솔). **Install-Package MongoDB.Driver** 명령을 사용하여 MongoDB용 드라이버를 설치합니다. 이렇게 하면 MongoDB 데이터베이스에 프로그래밍 방식으로 액세스할 수 있습니다.

## <a name="test-your-connection-using-sample-code"></a>샘플 코드를 사용하여 연결 테스트
다음 샘플 코드는 원격 MongoDB 클라이언트에서 컬렉션을 가져온 다음, 해당 컬렉션에 새 문서를 추가합니다. 그리고 MongoDB API를 사용하여 컬렉션의 새 크기와 삽입된 문서를 검색하여 출력합니다. IP 주소 및 데이터베이스 이름을 사용자 지정해야 합니다.

```csharp
var client = new MongoClient("mongodb://10.xxx.xx.xxx:xxx");
IMongoDatabase database = client.GetDatabase("foo");
IMongoCollection<BsonDocument> collection = database.GetCollection<BsonDocument>("bar");
BsonDocument document = new BsonDocument
{
     { "name","MongoDB"},
     { "type","Database"},
     { "count",1},
     { "info",new BsonDocument { { "x", 203 }, { "y", 102 } }}
};
collection.InsertOne(document);
long count = collection.CountDocuments(document);
Debug.WriteLine(count);
IFindFluent<BsonDocument, BsonDocument> document1 = collection.Find(document);
Debug.WriteLine(document1.ToString());
```
