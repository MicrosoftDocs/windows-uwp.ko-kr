---
title: Xbox Live 타이틀 저장소
author: KevinAsgari
description: Xbox Live 타이틀 저장소를 사용 하 여 클라우드에서 제목에 대 한 게임 정보를 저장 하는 방법을 알아봅니다.
ms.assetid: a4182bc8-d232-4e77-93ae-97fe17ac71b1
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 49f0f88a7e64ce57462b3ee7b07676280d91fb41
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4428656"
---
# <a name="xbox-live-title-storage"></a>Xbox Live 타이틀 저장소

Xbox Live 타이틀 저장소 서비스는 클라우드에서 제목에 대 한 게임 정보를 저장 하는 방법을 제공 합니다. 모든 플랫폼에서 실행 되는 게임이이 서비스를 사용할 수 있습니다.

<a name="ID4EW"></a>

## <a name="features-of-xbox-live-title-storage"></a>Xbox Live 타이틀 저장소의 기능

Xbox Live 타이틀 저장소의 고급 기능을 포함 하지만 국한 되지 않습니다.

-   사용자, 제목 및 다양 한 플랫폼 간에 공유할 수 있습니다.
-   구성 파일 및 지 원하는 이진, JSON

Xbox Live 타이틀 저장소의 주요 기능 다음 섹션에서 자세히 설명 되어 있습니다.

-   [저장소 유형](#ID4ETB)
-   [데이터 유형](#ID4ECF)
-   [타이틀 저장소 Uri](#ID4EBEAC)
-   [스로틀 제한](#ID4ETEAC)

<a name="ID4ETB"></a>

관리 파트너 및 ID@Xbox 멤버:

| 저장소 유형       | 할당량 (관리 Partners/ID@Xbox) | 할당량 (Xbox Live 크리에이터 스 프로그램) |  목적                                                                                                                                                      | 플랫폼                                                                                           | 사용자                                       |
|--------------------|--------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|---------------------------------------------|
| 신뢰할 수 있는 플랫폼   | 사용자 당 256MB | 사용자 당 64 MB    | 사용자 당 데이터와 같은 저장 된 게임이 나 재생/일시 중지/다시 시작에 대 한 게임 상태. 플랫폼 제한 사항과 함께 있지만 더 안전 합니다. | 모든 플랫폼 읽을 수는 있지만 Xbox One, Xbox 360 또는 Windows Phone 쓸 수 있습니다.  | 공용 또는 소유자만 구성할 수 있습니다.       |
| 유니버설 플랫폼 | 사용자 당 64 MB | 사용자 당 64 MB    | 사용자 당 데이터와 같은 저장 된 게임이 나 재생/일시 중지/다시 시작에 대 한 게임 상태. | 모든 플랫폼 쓸 수 있지만 Xbox One, Xbox 360 또는 Windows Phone 이외의 플랫폼만 읽을 수 있습니다. | 공용 또는 소유자만 구성할 수 있습니다.       |
| 글로벌             | 256MB | 256MB            | 명단, 지도, 문제, 또는 아트 리소스 등의 모든 사용자가 읽을 수 있는 데이터입니다. | Xbox 개발자 포털 또는 Windows 개발자 센터를 통해 쓰기만 모든 플랫폼 읽을 수 있습니다.                                | 모든 사용자가 읽을 수 있습니다.

### <a name="deprecated-storage-types"></a>사용 되지 않는 저장소 유형

다음 저장소 유형 되지 않습니다. 현재 사용 중인 제목에 대해서만 지원 됩니다. 새 제목에 사용할 수 있는 하지 않습니다.

| 저장소 유형       | 할당량  |   목적                                                                                                                                                      | 플랫폼                                                                                           | 사용자                                       |
|--------------------|--------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|---------------------------------------------|
| JSON               | 사용자 당 64 MB     | 사용자 당 데이터와 같은 저장 된 게임이 나 재생/일시 중지/다시 시작에 대 한 게임 상태. 더 안전 하 고, 플랫폼 제한이 비슷하지만 데이터 형식을 제한 (JSON에만 해당). | 모든 플랫폼 읽거나 쓸 수 있습니다.                                                                     | 공용 또는 소유자만 구성할 수 있습니다.       |
| 장치             | 장치별 64 MB   | 장치 설정 또는 장치 기본 설정 등의 데이터입니다.                                                                                            | 만 Xbox One, Xbox 360 또는 Windows Phone 쓸 수 있습니다. 데이터를 작성 하는 장치에만 읽을 수 있습니다.  | 모든 사용자가 읽을 수 있습니다.                         |
| 세션 저장소    | 세션별 256MB | 모든 사용자에 대 한 데이터는 특정 멀티 플레이 게임 세션에 가입 합니다.                                                                                             | 세션에 참가할 수 있는 모든 플랫폼입니다.                                                             | 세션의 모든 사용자가 읽거나 쓸 수 있습니다. |


<a name="ID4ECF"></a>

## <a name="types-of-data"></a>데이터 유형

게임에서 GET 또는 PUT 메서드에의 **{형식}** 매개 변수를 사용 하는 데이터의 종류를 지정 합니다. 다음 섹션에서 지원 되는 세 가지 종류를 설명합니다.

-   [이진 정보](#ID4ENF)
-   [JSON 정보](#ID4EUF)
-   [구성 정보](#ID4ECAAC)

<a name="ID4ENF"></a>

#### <a name="binary-information"></a>이진 정보

이미지, 소리 및 사용자 지정 데이터 이진 형식을 사용 합니다. HTTP를 통해 데이터를 전송 해야, 때문에 문자로 HTTP 수락 하는 이진 데이터 인코딩해야 합니다. 예를 들어 데이터를 16 진수 문자열 변환 또는 base64 인코딩을 사용 해야 합니다. 타이틀 저장소 시스템 게임 인코드 및 디코드 데이터에서 읽고 타이틀 저장소에 쓸 때 같은 체계를 사용 해야 하므로 인코딩된 데이터를 처리 하지 않습니다.

<a name="ID4EUF"></a>

#### <a name="json-information"></a>JSON 정보

구조화 된 데이터를 JSON 형식에 사용할 수 있습니다. 같은 언어에서 JavaScript, 지 원하는 JSON 개체를 직접 사용할 수 있습니다. JSON 파일에서 데이터를 검색할 때 게임 구조 내에서 특정 항목을 반환 하는 *선택* 매개 변수를 제공할 수 있습니다. 예를 들어 다음 정보를 포함 하는 JSON 형식의 파일을 사용 합니다.

    {
    "difficulty" : 1,
    "level" :
        [
            { "number" : "1", "quest" : "swords" },
            { "number" : "2", "quest" : "iron" },
            { "number" : "3", "quest" : "gold" },
            { "number" : "4", "quest" : "queen" }
         ],
    "weapon" :
        {
             "name" : "poison",
             "timeleft" : "2mins"
        }
    }


| 참고                                                                                                                                              |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 보안을 위해 JSON 데이터의 첫 번째 요소 배열 수 없습니다. 루트 배열을 사용 하 여 제출 하는 JSON 데이터 서비스에서 거부 됩니다. |

게임 같이 쿼리를 사용 하 여이 구조 부분을 선택할 수 있습니다.

             GET https://titlestorage.xboxlive.com/users/xuid(1234)/storage/titlestorage/titlegroups/
             faa29d21-2b49-4908-96bf-b953157ac4fe/data/save1.dat,json?select=weapon.name
             Content-Type: application/octet-stream
             x-xbl-contract-version: 1
             Authorization: XBL3.0 x=<userHash>;<STSTokenString>
             Connection: Keep-Alive

이 쿼리에 대 한 응답 본문은입니다.

    {
        "name" : "poison"
    }

배열 같이 쿼리를 사용 하 여 액세스할 수 있습니다.

      GET https://titlestorage.xboxlive.com//users/xuid(1234)/storage/titlestorage/titlegroups/
      faa29d21-2b49-4908-96bf-b953157ac4fe/data/save1.dat,json?select=levels[3].quest
      Content-Type: application/octet-stream
      x-xbl-contract-version: 1
      Authorization: XBL3.0 x=<userHash>;<STSTokenString>
      Connection: Keep-Alive

이 쿼리에 대 한 응답 본문은입니다.

    {
        "quest" : "queen"
    }

JSON 데이터에 대 한 다음 길이 제한은 적용 됩니다.

-   숫자 값, 최대 길이 32 =
-   문자열 값을 최대 길이 1024 =
-   속성 이름, 최대 길이 64 =
-   계층 구조, 최대 깊이 = 16
-   배열, 최대 크기 1024 =
-   자식 속성을 개체에서 최대 1024 =

<a name="ID4ECAAC"></a>

#### <a name="configuration-information"></a>구성 정보

**{유형}** 데이터 구성 blob 임을 나타냅니다 **구성** 될 수 있습니다. 구성 blob에는 전역 타이틀 저장소에 저장 된 데이터 구조는 합니다. Blob 형식의 JSON 개체와 비슷합니다.

구성 blob 설정을 가능성의 목록에서 반환 하는 가상 노드를 포함할 수 있습니다. 가상 노드는 제목 또는 지역에 대 한와 같은 특정 상황에 대 한 설정을 제공 하는 데 유용 합니다. 가상 노드 값 및 값에서 선택에 대 한 조건 함께 가능한 몇 가지 설정이 포함 됩니다. 다음 예제에서는 **defaultCardDesign** 설정에에서 있을 수 값 중 하나로 가상 노드.

    {
      "defaultCardDesign":
      {
        "_virtualNode":
       {
          "_selectBy":"titleId",
          "_sourceNodes":
          [
            {"_selector":"123456799", "_data":"RobotUnicornCard.png,binary"},
            {"_selector":"default", "_data":"StandardCard.png,binary"}
          ]
        }
      },
    }

게임을이 파일을 읽고, 시스템 **\_sourceNodes** 배열에서 값 중 하나를 선택 합니다. 이 경우 항목이 게임의 제목 ID에 따라 선택 합니다. 게임 **12456799** 재생 하는 사용자를 참조 하세요.

    {
      "defaultCardDesign":"RobotUnicornCard.png,binary",
      "_sourceNodes":["defaultCardDesign:titleID:1234567899"]
    }

사용자의 나머지 부분을 참조 하세요.

    {
      "defaultCardDesign":"StandardCard.png,binary",
      "_sourceNodes":["defaultCardDesign:titleID:default"]
    }

게임 요청에 대 한 매개 변수를 일치 하는 사용자 지정 선택기를 정의할 수 있습니다. 예를 들어,이 구성 blob에서:

    {
        "defaultCardDesign":
        {
            "_virtualNode":
            {
                "_selectBy":"custom:gameMode",
                "_sourceNodes":
                [
                    {"_selector":"silly", "_data":"RobotUnicornCard.png,binary"},
                    {"_selector":"serious", "_data":"SeriousCard.png,binary"},
                    {"_selector":"default", "_data":"StandardCard.png,binary"}
                 ]
            }
        },
        "backgroundColor":"green",
        "dealerHitsOnSoft17":true
    }

게임은 **customSelector** 매개 변수를 반환 되는 항목을 선택 하는 문자열을 전달 합니다. 예를 들어, 두 번째 옵션을 가져오려면 게임을 요청 합니다.

      GET https://titlestorage.xboxlive.com/media/titlegroups/faa29d21-2b49-4908-96bf-b953157ac4fe
      /storage/data/config.json,config?customSelector=gameMode.serious
      Content-Type: application/octet-stream
      x-xbl-contract-version: 1
      Authorization: XBL3.0 x=<userHash>;<STSTokenString>
      Connection: Keep-Alive

**\_SelectBy** 값 선택 작업을 수행 하 고 **\_selector** 값의 유형을 선택에 사용 하 여 데이터를 나타냅니다. 가능한 값은 다음과 같습니다.

<table>
<thead>
<tr>
<th>_selectBy</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td >titleId</td>
<td ><p><strong>_Selector</strong> 제공 된 클레임에 제목 ID와 일치합니다.</p></td>
</tr>
<tr>
<td >locale</td>
<td ><p><strong>_Selector</strong> Accept-language 헤더에서 로캘 문자열와 일치합니다.</p></td>
</tr>
<tr>
<td >사용자 지정</td>
<td ><p><strong>_Selector</strong> <strong>customSelector</strong> 쿼리 매개 변수에서 전달 된 사용자 지정 문자열을 찾습니다. <strong>CustomSelector</strong> 쉼표로 구분 된 하나 이상의 쿼리를 포함 합니다. 각 쿼리는 <strong>selectBy</strong> 요소에서 이름과 <strong>_selector</strong> 요소에서 값.</p></td>
</tr>
</tbody>
</table>

<a name="ID4EBEAC"></a>

## <a name="title-storage-uris"></a>타이틀 저장소 Uri

타이틀 저장소 Uri 형식은 다음과 같습니다.

    https://titlestorage.xboxlive.com/{path}

URI의 **{경로}** 부분 요청 되는 유형의 이며 245 자 이하로 이어야 합니다.

<a name="ID4ETEAC"></a>

## <a name="throttle-limit"></a>스로틀 제한

읽기 나 쓰기 제목을 분당 만들 수는 고정된 제한이 하지만 일반적으로 사용할 수 없는 분당 하나 이상 평균 1 시간 세션에서. 예를 들어 제목을 남아 있는 시간 동안 더 이상 하지만 세션의 시작 부분에서 60 읽기 또는 쓰기 만들 수 있습니다. 타이틀 해야 강화 호출에 대해 나중에 Xbox LIVE 서비스 요청을 제한 해야 하는 경우.

타이틀에 추가 읽기 또는 쓰기와 같은 특수 분할 요구 사항이 있는 경우 Microsoft에 문의 합니다.

<a name="ID4E5EAC"></a>

## <a name="using-title-storage"></a>타이틀 저장소를 사용 하 여

타이틀 저장소 시작 하려면 먼저 어떤 종류의 데이터를 저장 하려는 확인 합니다. 몇 가지 예로 저장 된 게임, 게임 상태, 매일 문제, 게임 지도 및 아트 리소스입니다.

그런 다음 제목 및 플랫폼은 필요한 데이터에 액세스할 수를 결정 합니다. 타이틀 저장소 지원 클라우드 데이터 액세스 단일 플랫폼에 단일 제목 및 여러 플랫폼에서 여러 제목에서.

마지막으로, 저장소 구성 데이터를 업로드 하 고 옵션에 따른 적절 하 게 액세스 권한을 설정 하려면이 섹션의 항목을를 사용 합니다.

<a name="ID4EJFAC"></a>

## <a name="in-this-section"></a>이 섹션 내용

[Xbox Live 타이틀 저장소에서 구성 Blob 읽기](reading-configuration-blobs.md)  
읽는 방법을 보여 줍니다 구성 blob에서 Xbox Live 타이틀 저장소.

[Xbox Live 타이틀 저장소의 이진 Blob 저장](storing-binary-blobs.md)  
Xbox Live 타이틀 저장소의 이진 blob 저장 하는 방법을 보여 줍니다.

[Xbox Live 타이틀 저장소의 이진 Blob 읽기](reading-binary-blobs.md)  
Xbox Live 타이틀 저장소에서 이진 blob 읽기를 보여 줍니다.

[Xbox Live 타이틀 저장소에 JSON Blob 저장](storing-jsonblobs.md)  
Xbox Live 타이틀 저장소에 저장 JSON blob을 보여 줍니다.

[Xbox Live 타이틀 저장소에 JSON Blob 읽기](reading-jsonblobs.md)  
읽는 방법을 보여 줍니다 JSON blob에서 Xbox Live 타이틀 저장소.

<a name="ID4E4FAC"></a>
