---
title: GET (/users/xuid({xuid})/scids/{scid}/stats)
assetID: af117e87-6f1d-6448-9adf-7cf890d1380f
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstatsget.html
description: " GET (/users/xuid({xuid})/scids/{scid}/stats)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: baf965dcbd23bf00d7d0953726f9f20852324e5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662388"
---
# <a name="get-usersxuidxuidscidsscidstats"></a>GET (/users/xuid({xuid})/scids/{scid}/stats)
지정된 된 사용자를 대신 하 여 사용자 통계 이름의 쉼표로 구분 된 목록으로 범위가 지정 된 서비스 구성을 가져옵니다.
이러한 Uri에 대 한 도메인은 `userstats.xboxlive.com`합니다.

  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EEB)
  * [쿼리 문자열 매개 변수](#ID4EPB)
  * [권한 부여](#ID4EUC)
  * [필요한 요청 헤더](#ID4EPD)
  * [선택적 요청 헤더](#ID4EYE)
  * [요청 본문](#ID4E3F)
  * [HTTP 상태 코드](#ID4EHG)
  * [응답 본문](#ID4E5BAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>설명

클라이언트에서에 읽기 및 쓰기 제목 통계 플레이어를 대신 하 여 새 플레이어 통계 시스템에 있어야 합니다.

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- |
| xuid| GUID| Xbox 사용자 ID (XUID) 서비스 구성에 액세스 하는 사용자입니다.|
| scid| GUID| 액세스 되는 리소스를 포함 하는 서비스 구성의 식별자입니다.|

<a id="ID4EPB"></a>

 
## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- | --- | --- | --- |
| statNames| 문자열| 유일한 쿼리 문자열 매개 변수는 쉼표로 구분 된 사용자 통계 이름 URI 명사. 예를 들어, 다음 URI는 URI에 지정 된 사용자 id를 대신 하 여 요청 된 4 개의 통계가 알립니다. `https://userstats.xboxlive.com/users/xuid({xuid})/scids/{scid}/stats/wins,kills,kdratio,headshots`단일 호출에서 요청할 수 있는 통계의 수에 제한이 및 한도 신중 하 게 고려해 "최적" 개발자를 위한 편의 vs. URI 길이 실용성을 살펴보겠습니다. 예를 들어, 한계 통계 이름 텍스트 (쉼표 포함)의 가치가 600 두 문자 또는 최대 10 개의 통계에 의해 결정 합니다. 다음과 같은 간단한 GET을 사용 하면 대화 량이 많은 클라이언트에서의 통화 량을 줄일 수는 일반적으로 요청 된 통계에 대 한 HTTP 캐싱을 활성화 합니다. |

<a id="ID4EUC"></a>


## <a name="authorization"></a>권한 부여

콘텐츠 격리 및 액세스 제어 시나리오에 대해 구현 하는 권한 부여 논리가 있습니다.

   * 순위표 및 사용자 통계는 호출자에 게 유효한 XSTS 토큰 요청과 함께 전송 된 모든 플랫폼에서 클라이언트에서 읽을 수 있습니다. 쓰기는 분명히 데이터 플랫폼에서 지 원하는 클라이언트 제한 됩니다.
   * 제목 개발자에 따라 개방 또는 제한 XDP 또는 파트너 센터를 사용 하 여 통계를 표시할 수 있습니다. 순위표 통계가 엽니다. 사용자는 샌드박스에 허용으로 열기 통계 Smartglass, 뿐만 아니라 iOS, Android, Windows, Windows Phone 및 웹 응용 프로그램에서 액세스할 수 있습니다. 사용자 권한 부여는 샌드박스에 XDP 또는 파트너 센터를 통해 관리 됩니다.

확인에 대 한 의사 (pseudo) 코드는 다음과 같습니다.


```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.

```


<a id="ID4EPD"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 권한 부여| 문자열| HTTP 인증을 위해 자격 증명을 인증 합니다. 예를 들어 값: "XBL3.0 x=&lt;userhash>;&lt;token>".|

<a id="ID4EYE"></a>


## <a name="optional-request-headers"></a>선택적 요청 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | 빌드는이 요청을 보내야 하는 서비스의 이름/번호입니다. 요청만으로 라우팅되는 헤더, 인증 토큰의 클레임의 유효성을 확인 한 후 서비스는 합니다. 기본값: 1.|

<a id="ID4E3F"></a>


## <a name="request-body"></a>요청 본문

개체가이 요청의 본문에 전송 됩니다.

<a id="ID4EHG"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드

서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.

| 코드| 이유 구| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 확인| 세션을 검색 했습니다.|
| 304| 수정 안 됨| 리소스 되지 마지막 요청 이후 수정 합니다.|
| 400| 잘못된 요청| 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.|
| 401| 권한 없음| 요청에 사용자 인증이 필요합니다.|
| 403| 사용할 수 없음| 사용자 또는 서비스에 대 한 요청이 허용 되지 않습니다.|
| 404| 없음| 지정된 된 리소스를 찾을 수 없습니다.|
| 406| 허용 되지 않음| 리소스 버전이 지원 되지 않습니다.|
| 408| 요청 시간 초과| 리소스 버전이 지원 되지 않습니다. MVC 계층에서 거부 해야 합니다.|

<a id="ID4E5BAC"></a>


## <a name="response-body"></a>응답 본문

<a id="ID4EECAC"></a>


### <a name="sample-response"></a>샘플 응답


```cpp
{
    "user": {
    "xuid": "123456789",
        "gamertag": "WarriorSaint",
        "stats": [
            {
                "statname": "Wins",
                "type": "Integer",
                "value": 40
            },
            {
                "statname": "Kills",
                "type": "Integer",
                "value": 700
            },
            {
                "statname": "KDRatio",
                "type": "Double",
                "value": 2.23
            },
            {
                "statname": "Headshots",
                "type": "Integer",
                "value": 173
            }
        ],
    }
}

```


<a id="ID4EOCAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EQCAC"></a>


##### <a name="parent"></a>Parent

[/users/xuid({xuid})/scids/{scid}/stats](uri-usersxuidscidsscidstats.md)
