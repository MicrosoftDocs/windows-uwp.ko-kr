---
title: GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)
assetID: 890e3f93-4fdc-955f-d849-ba9579d5c1eb
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstatsgetvaluemetadata.html
author: KevinAsgari
description: " GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2c795dbd2b6193798b472e51526bdd9e2f6b4622
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4415264"
---
# <a name="get-usersxuidxuidscidsscidstatsincludevaluemetadata"></a>GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)
사용자 지정 된 서비스 구성에 대 한 통계 값을와 관련 된 메타 데이터를 포함 하 여 지정 된 통계 목록을 가져옵니다.
이러한 Uri에 대 한 도메인은 `userstats.xboxlive.com`.

  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EAB)
  * [쿼리 문자열 매개 변수](#ID4ELB)
  * [권한 부여](#ID4EWC)
  * [필요한 요청 헤더](#ID4ERD)
  * [선택적 요청 헤더](#ID4EDF)
  * [요청 본문](#ID4EHG)
  * [HTTP 상태 코드](#ID4ESG)
  * [응답 본문](#ID4EJCAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>설명

? 포함 = valuemetadata 쿼리 매개 변수는 응답 모델 및 경합 트랙에 시간을 달성 하는 데 사용 되는 자동차의 색 등의 사용자 통계 값와 관련 된 메타 데이터를 포함 하도록 합니다.

응답에 값 메타 데이터를 포함 하도록 요청 호출 3 X Xbl-계약 버전 헤더 값도 설정 해야 합니다.

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| xuid| GUID| Xbox 사용자 ID (XUID) 서비스 구성에 액세스 하려면 대신 사용자의 합니다.|
| 서비스 안내| GUID| 액세스 되는 리소스를 포함 하는 서비스 구성의 식별자입니다.|

<a id="ID4ELB"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- | --- | --- |
| statNames| string| 쉼표로 구분 사용자 통계 이름 목록. 예를 들어 다음 URI는 URI에 지정 된 사용자 id를 대신 하 여 4 개의 통계를 요청 하는 서비스 알립니다. {:: nomakrdown}<br/><br/>`https://userstats.xboxlive.com/users/xuid({xuid})/scids/{scid}/stats/wins,kills,kdratio,headshots?include=valuemetadata`| 
| 포함 = valuemetadata| string| 응답 uset 통계 값와 관련 된 모든 값 메타 데이터를 포함 하는 것을 나타냅니다.|

<a id="ID4EWC"></a>


## <a name="authorization"></a>권한 부여

권한 부여 논리 콘텐츠 격리 및 액세스 제어 시나리오에 대해 구현 있습니다.

   * 순위표와 사용자 통계 호출자가 요청을 사용 하 여 유효한 XSTS 토큰 제출 된 모든 플랫폼에서 클라이언트에서 읽을 수 있습니다. 쓰기는 데이터 플랫폼에서 지원 되는 클라이언트로 제한 됩니다.
   * 제목 개발자 열기 또는 XDP 또는 개발자 센터를 사용 하 여 제한 된 통계를 표시할 수 있습니다. 순위표 통계가 엽니다. 사용자가 해당 샌드박스에 인증으로 열기 통계 Smartglass, 뿐만 아니라 iOS, Android, Windows, Windows Phone 및 웹 응용 프로그램에서 액세스할 수 있습니다. 사용자 권한 부여 샌드박스에 XDP 또는 개발자 센터를 통해 관리 됩니다.

검사에 대 한 의사 코드는 다음과 같습니다.


```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.

```


<a id="ID4ERD"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 권한 부여| 문자열| HTTP 인증에 대 한 자격 증명을 인증 합니다. 예제 값: "XBL3.0 x =&lt;userhash >; &lt;토큰 > ".|
| Xbl 계약 버전 X| string| 사용 하는 API의 버전을 나타냅니다. 응답에 메타 데이터 값을 포함 하기 위해이 값을 "3" 설정 해야 합니다.|

<a id="ID4EDF"></a>


## <a name="optional-request-headers"></a>선택적 요청 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X RequestedServiceVersion|  | 이 요청은 전송 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 인증 토큰의 클레임 헤더의 유효성을 확인 한 후 서비스는 합니다. 기본값: 1입니다.|

<a id="ID4EHG"></a>


## <a name="request-body"></a>요청 본문

개체가이 요청 본문에 전송 됩니다.

<a id="ID4ESG"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드

서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스와 함께 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.

| Code| 이유 구문| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 확인| 세션을 검색 했습니다.|
| 304| 수정 되지 않음| 리소스 되지 요청 마지막으로 수정 합니다.|
| 400| 잘못 된 요청| 서비스 잘못 된 요청을 이해 하지 못했습니다. 일반적으로 잘못 된 매개 변수입니다.|
| 401| 권한 없음| 필요한 사용자 인증을 요청 합니다.|
| 403| 금지| 사용자 또는 서비스에 대 한 요청을 허용 되지 않습니다.|
| 404| 찾을 수 없음| 지정된 된 리소스를 찾을 수 없습니다.|
| 406| 허용할 수 없음| 리소스 버전은 지원 되지 않습니다.|
| 408| 요청 시간 제한| 리소스 버전이 지원 되지 않습니다. MVC 계층에 의해 거부 되어야 합니다.|

<a id="ID4EJCAC"></a>


## <a name="response-body"></a>응답 본문

<a id="ID4EPCAC"></a>


### <a name="sample-response"></a>예제 응답


```cpp
{
  "user": {
    "xuid": "123456789",
    "gamertag": "WarriorSaint",
    "stats": [
      {
        "statname": "Wins",
        "type": "Integer",
        "value": 40,
        "valuemetadata" : "{\"region\" : \"EU\", \"isRanked\" : true}"
      },
      {
        "statname": "Kills",
        "type": "Integer",
        "value": 700,
        "valuemetadata" : "{\"longestKillStreak" : 15, \"favoriteTarget\" : \"CrazyPigeon\"}"
      },
      {
        "statname": "KDRatio",
        "type": "Double",
        "value": 2.23,
        "valuemetadata" : "{\"totalKills\" : 700, \"totalDeaths\" : 314}"
      },
      {
        "statname": "Headshots",
        "type": "Integer",
        "value": 173,
        "valuemetadata" : ""
      }
    ],
  }
}

```


<a id="ID4EZCAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4E2CAC"></a>


##### <a name="parent"></a>부모

[/users/xuid({xuid})/scids/{scid}/stats](uri-usersxuidscidsscidstats.md)
