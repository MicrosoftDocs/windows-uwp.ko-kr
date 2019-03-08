---
title: Xbox Live 제목 저장소
description: Xbox Live 제목 저장소를 사용 하 여 클라우드에서 타이틀 게임 정보를 저장 하는 방법을 알아봅니다.
ms.assetid: a4182bc8-d232-4e77-93ae-97fe17ac71b1
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 472a2131c113cdde5d7383e169e4fbe638e4e555
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623928"
---
# <a name="xbox-live-title-storage"></a>Xbox Live 제목 저장소

제목 저장소 Xbox Live 서비스에는 클라우드에서 게임 제목 정보를 저장 하는 방법을 제공 합니다. 모든 플랫폼에서 실행 되는 게임이이 서비스를 사용할 수 있습니다.

<a name="ID4EW"></a>

## <a name="features-of-xbox-live-title-storage"></a>Xbox Live 제목 storage의 기능

Xbox Live 제목 저장소의 고급 기능 중 일부 포함 되지만에 제한 되지 않습니다.

-   사용자, 제목 및 다양 한 플랫폼에서 공유할 수 있습니다.
-   JSON, 이진, 지원 및 구성 파일

Xbox Live 제목 저장소의 주요 기능을 다음 섹션에서 자세히 설명 합니다.

-   [저장소 유형](#ID4ETB)
-   [데이터 형식](#ID4ECF)
-   [저장소 Uri를 제목](#ID4EBEAC)
-   [스로틀 한계](#ID4ETEAC)

<a name="ID4ETB"></a>

관리 되는 파트너 및 ID@Xbox 멤버:

| 저장소 유형       | 할당량 (관리 되는 Partners/ID@Xbox) | 할당량 (Xbox Live 크리에이터 스 프로그램) |  용도                                                                                                                                                      | 플랫폼                                                                                           | 사용자                                       |
|--------------------|--------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|---------------------------------------------|
| 신뢰할 수 있는 플랫폼   | 사용자 당 256mb 이상 | 사용자 당 64MB    | 사용자별 데이터 저장과 같은 게임 또는 재생/일시 중지/다시 시작에 대 한 게임 상태입니다. 플랫폼 제한 하지만 더 안전 합니다. | 모든 플랫폼을 읽을 수 있습니다, 있지만 Xbox One, Xbox 360, 또는 Windows Phone 작성할 수 있습니다.  | 공용 또는 소유자만 하도록 구성할 수 있습니다.       |
| 유니버설 플랫폼 | 사용자 당 64MB | 사용자 당 64MB    | 사용자별 데이터 저장과 같은 게임 또는 재생/일시 중지/다시 시작에 대 한 게임 상태입니다. | 모든 플랫폼 작성할 수 있지만 Xbox One, Xbox 360 또는 Windows Phone 이외의 플랫폼만 읽을 수 있습니다. | 공용 또는 소유자만 하도록 구성할 수 있습니다.       |
| 전역             | 256MB 이상 | 256MB 이상            | 명단, 맵, 문제, 또는 최신 리소스와 같은 모든 사용자가 읽을 수 있는 데이터입니다. | Xbox 개발자 포털 또는 파트너 센터를 통해 쓰기 가능한 지만 모든 플랫폼 읽을 수 있습니다.                                | 모든 사용자가 읽을 수 있습니다.

### <a name="deprecated-storage-types"></a>사용 되지 않는 저장소 유형

다음 유형의 저장소를 사용 하는 사용 되지 않습니다. 현재 사용 되는 제목에 대해서만 지원 됩니다. 새 제목에 사용할 수 없는 합니다.

| 저장소 유형       | 할당량  |   용도                                                                                                                                                      | 플랫폼                                                                                           | 사용자                                       |
|--------------------|--------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|---------------------------------------------|
| JSON               | 사용자 당 64MB     | 사용자별 데이터 저장과 같은 게임 또는 재생/일시 중지/다시 시작에 대 한 게임 상태입니다. 보안 강화 플랫폼 제한이 하지만 데이터 형식 제한 사항 (JSON에만 해당). | 모든 플랫폼 읽기 또는 쓰기 수 있습니다.                                                                     | 공용 또는 소유자만 하도록 구성할 수 있습니다.       |
| 장치             | 장치당 64MB   | 특정 장치 기본 설정 설정 등 장치에 사용 되는 데이터입니다.                                                                                            | 만 Xbox One, Xbox 360, 또는 Windows Phone 작성할 수 있습니다. 데이터를 작성 하는 장치에만 읽을 수 있습니다.  | 모든 사용자가 읽을 수 있습니다.                         |
| 세션 저장소    | 세션당 256mb 이상 | 모든 사용자에 대 한 데이터 특정 멀티 플레이 게임 세션에 가입 합니다.                                                                                             | 세션에 참여할 수 있는 모든 플랫폼.                                                             | 세션의 모든 사용자는 읽기 또는 쓰기 수 있습니다. |


<a name="ID4ECF"></a>

## <a name="types-of-data"></a>데이터 형식

게임에서 사용 하는 데이터의 형식을 지정 합니다 **{type}** GET 또는 PUT 메서드의 매개 변수입니다. 다음 섹션에는 세 가지 지원 되는 형식을 설명합니다.

-   [이진 정보](#ID4ENF)
-   [JSON 정보](#ID4EUF)
-   [구성 정보](#ID4ECAAC)

<a name="ID4ENF"></a>

#### <a name="binary-information"></a>이진 정보

이미지, 소리 및 사용자 지정 데이터 이진 형식을 사용 합니다. HTTP를 통해 데이터를 전송 해야, 하므로 이진 데이터 HTTP 허용 하는 문자를 인코딩해야 합니다. 예를 들어, 데이터를 16 진수 문자열로 변환 하거나 base64 인코딩을 사용 수 있습니다. 게임 인코딩 및 디코딩 스트림에서 읽고 제목 저장소에 쓸 때 데이터에 대 한 동일한 구성표를 사용 해야 하므로 제목 저장소 시스템에 인코딩된 데이터를 처리 하지 않습니다.

<a name="ID4EUF"></a>

#### <a name="json-information"></a>JSON 정보

구조화 된 데이터는 JSON 형식을 사용할 수 있습니다. JSON 개체, JavaScript와 같은 언어를 지 원하는에서 직접 사용할 수 있습니다. 게임에 JSON 파일에서 데이터를 검색할 때를 제공할 수는 *선택* 매개 변수 구조 내에서 특정 항목을 반환 합니다. 예를 들어, 다음 정보를 포함 하는 JSON 형식의 파일을 사용 합니다.

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
| 보안을 위해 JSON 데이터의 첫 번째 요소 배열 수 없습니다. 루트에서 배열을 사용 하 여 제출 하는 JSON 데이터 서비스에서 거부 됩니다. |

게임 부분 다음과 같은 쿼리를 사용 하 여이 구조를 선택할 수 있습니다.

             GET https://titlestorage.xboxlive.com/users/xuid(1234)/storage/titlestorage/titlegroups/
             faa29d21-2b49-4908-96bf-b953157ac4fe/data/save1.dat,json?select=weapon.name
             Content-Type: application/octet-stream
             x-xbl-contract-version: 1
             Authorization: XBL3.0 x=<userHash>;<STSTokenString>
             Connection: Keep-Alive

이 쿼리에 대 한 응답 본문은:

    {
        "name" : "poison"
    }

배열 다음과 같은 쿼리를 사용 하 여 액세스할 수 있습니다.

      GET https://titlestorage.xboxlive.com//users/xuid(1234)/storage/titlestorage/titlegroups/
      faa29d21-2b49-4908-96bf-b953157ac4fe/data/save1.dat,json?select=levels[3].quest
      Content-Type: application/octet-stream
      x-xbl-contract-version: 1
      Authorization: XBL3.0 x=<userHash>;<STSTokenString>
      Connection: Keep-Alive

이 쿼리에 대 한 응답 본문은:

    {
        "quest" : "queen"
    }

JSON 데이터에 대 한 다음과 같은 길이 제한 사항이 적용 됩니다.

-   숫자 값이 고, 최대 길이 = 32
-   문자열 값을 최대 길이 = 1024
-   속성 이름, 최대 길이 = 64
-   계층, 최대 깊이 = 16
-   배열, 최대 크기 = 1024
-   자식 속성을 개체에 최대 1024 =

<a name="ID4ECAAC"></a>

#### <a name="configuration-information"></a>구성 정보

합니다 **{type}** 될 수 있습니다 **구성** 구성 blob 데이터 임을 나타냅니다. 구성 blob은 전역 제목 저장소에 저장 된 데이터 구조입니다. Blob의 형식을 JSON 개체와 비슷합니다.

구성 blob 설정 가능한 항목 목록에서 반환 하는 가상 노드를 포함할 수 있습니다. 가상 노드에 제목 또는 로캘와 같은 특정 한 상황에 대 한 설정을 제공 하는 데 유용 합니다. 가상 노드 값 및 값에서 선택 하기 위한 조건을 함께 몇 가지 가능한 설정을 포함 합니다. 다음 예제에서는 **defaultCardDesign** 설정을 가상 노드에서 값 중 하나일 수 있습니다.

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

시스템에서 값 중 하나를 선택 게임에서이 파일을 읽는 경우 합니다  **\_sourceNodes** 배열입니다. 이 경우에 항목이 게임의 제목 ID에 따라 선택 됩니다. 사용자가 게임을 플레이 **12456799** 참조 하세요.

    {
      "defaultCardDesign":"RobotUnicornCard.png,binary",
      "_sourceNodes":["defaultCardDesign:titleID:1234567899"]
    }

사용자의 나머지 부분을 참조 하세요.

    {
      "defaultCardDesign":"StandardCard.png,binary",
      "_sourceNodes":["defaultCardDesign:titleID:default"]
    }

게임 요청에서 매개 변수와 일치 하는 사용자 지정 선택기를 정의할 수 있습니다. 이 구성 blob의 예를 들어:

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

문자열을 전달 하는 게임 합니다 **customSelector** 매개 변수를 반환 하는 항목을 선택 합니다. 예를 들어, 두 번째 옵션을 가져오려면 게임을 요청 합니다.

      GET https://titlestorage.xboxlive.com/media/titlegroups/faa29d21-2b49-4908-96bf-b953157ac4fe
      /storage/data/config.json,config?customSelector=gameMode.serious
      Content-Type: application/octet-stream
      x-xbl-contract-version: 1
      Authorization: XBL3.0 x=<userHash>;<STSTokenString>
      Connection: Keep-Alive

합니다  **\_selectBy** 값에는 작업을 수행 하는 선택의 유형을 나타냅니다 하며  **\_선택기** 값 선택 영역에 사용할 데이터를 나타냅니다. 가능한 값은:

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
<td ><p>합니다 <strong>_selector</strong> 제공 된 클레임의 제목 ID와 일치 합니다.</p></td>
</tr>
<tr>
<td >locale</td>
<td ><p>합니다 <strong>_selector</strong> 수용-언어 헤더에서 로캘 문자열을 찾습니다.</p></td>
</tr>
<tr>
<td >사용자 지정</td>
<td ><p>합니다 <strong>_selector</strong> 전달 된 사용자 지정 문자열과 일치 합니다 <strong>customSelector</strong> 쿼리 매개 변수입니다. 합니다 <strong>customSelector</strong> 쉼표로 구분 된 하나 이상의 쿼리를 포함 합니다. 각 쿼리는 이름 합니다 <strong>selectBy</strong> 요소와 값을 <strong>_selector</strong> 요소.</p></td>
</tr>
</tbody>
</table>

<a name="ID4EBEAC"></a>

## <a name="title-storage-uris"></a>저장소 Uri를 제목

제목 저장소 Uri의 형식은 다음과 같습니다.

    https://titlestorage.xboxlive.com/{path}

합니다 **{path}** URI의 부분 만들어지는 요청 유형의 및 245 자 여야 합니다.

<a name="ID4ETEAC"></a>

## <a name="throttle-limit"></a>스로틀 한계

얼마나 많은 읽기 또는 쓰기 제목을 분당 가능 고정 한도가 있습니다 하지만 일반적으로 사용할 수 없는 둘 이상의 분당 평균 1 시간 세션에서. 예를 들어, 제목을 확인 시간의 나머지 하지만 더 이상 세션의 시작 부분에 60 읽기 또는 쓰기 수 있습니다. 제목 보안을 강화 해야 더 많은 호출에 대해 나중 경우 Xbox LIVE 서비스 요청을 제한 해야 합니다.

제목에는 추가 읽기 또는 쓰기와 같은 특수 한 분할 요구 사항이 Microsoft에 문의 합니다.

<a name="ID4E5EAC"></a>

## <a name="using-title-storage"></a>제목 저장소를 사용 하 여

제목 저장소 사용을 시작 하려면 먼저 저장 하려는 데이터 종류에 따라 확인 합니다. 저장 된 게임, 게임 상태, 일상적인 과제, 게임 맵 및 최신 리소스를 포함 하는 몇 가지 예입니다.

그런 다음 제목 및 플랫폼은 필요한 데이터에 액세스할 수를 결정 합니다. 제목 저장소 지원 클라우드 데이터 액세스 단일 플랫폼에서 단일 제목에서 여러 플랫폼에서 여러 제목입니다.

마지막으로,이 단원의 항목을 사용 하 여 저장소 구성, 데이터를 업로드 및 선택에 따라 적절 하 게 액세스 권한을 설정 하 합니다.

<a name="ID4EJFAC"></a>

## <a name="in-this-section"></a>이 섹션의 내용

[Xbox Live 제목 저장소의 구성 Blob을 읽는 중](reading-configuration-blobs.md)  
읽는 방법을 보여 줍니다 Xbox Live에서 구성 blob 저장소를 제목입니다.

[Xbox Live 제목 저장소에 이진 Blob을 저장합니다.](storing-binary-blobs.md)  
Xbox Live 제목 저장소에 이진 blob을 저장 하는 방법을 보여 줍니다.

[Xbox Live 제목 저장소에 이진 Blob을 읽는 중](reading-binary-blobs.md)  
Xbox Live 제목 저장소에서 이진 blob을 읽는 방법을 보여 줍니다.

[Xbox Live 제목 저장소에서 JSON Blob을 저장합니다.](storing-jsonblobs.md)  
Xbox Live 제목 저장소에서 JSON blob을 저장 방법을 보여 줍니다.

[Xbox Live 제목 저장소에서 JSON Blob을 읽는 중](reading-jsonblobs.md)  
읽는 방법을 보여 줍니다 Xbox Live에서 JSON blob 저장소를 제목입니다.

<a name="ID4E4FAC"></a>
