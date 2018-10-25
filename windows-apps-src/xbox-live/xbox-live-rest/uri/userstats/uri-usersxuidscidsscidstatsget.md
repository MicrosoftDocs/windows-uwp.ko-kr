---
title: GET (/users/xuid({xuid})/scids/{scid}/stats)
assetID: af117e87-6f1d-6448-9adf-7cf890d1380f
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstatsget.html
author: KevinAsgari
description: " GET (/users/xuid({xuid})/scids/{scid}/stats)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ed96418141aec049a9577924597a07da4313b7e2
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5469003"
---
# <a name="get-usersxuidxuidscidsscidstats"></a>GET (/users/xuid({xuid})/scids/{scid}/stats)
지정된 된 사용자를 대신 하 여 사용자 통계 이름의 쉼표로 구분 된 목록에 의해 범위가 서비스 구성을 가져옵니다.
이러한 Uri의 도메인은 `userstats.xboxlive.com`.

  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EEB)
  * [쿼리 문자열 매개 변수](#ID4EPB)
  * [권한 부여](#ID4EUC)
  * [필요한 요청 헤더](#ID4EPD)
  * [선택적 요청 헤더](#ID4EYE)
  * [요청 본문](#ID4E3F)
  * [HTTP 상태 코드입니다.](#ID4EHG)
  * [응답 본문](#ID4E5BAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>설명

클라이언트는를 읽고 쓰는 제목 통계 플레이어를 대신 하 여 새 플레이어 통계 시스템 방법이 필요 합니다.

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| xuid| GUID| Xbox 사용자 ID (XUID) 서비스 구성에 액세스 하려면 대신 사용자입니다.|
| 서비스 안내| GUID| 액세스 되는 리소스를 포함 하는 서비스 구성의 식별자입니다.|

<a id="ID4EPB"></a>

 
## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- | --- | --- |
| statNames| string| 유일한 쿼리 문자열 매개 변수는 쉼표로 구분 된 사용자 통계 이름 URI 명사. 예를 들어 다음 URI는 URI에 지정 된 사용자 id를 대신 하 여 4 개의 통계를 요청 하는 서비스 알립니다. `https://userstats.xboxlive.com/users/xuid({xuid})/scids/{scid}/stats/wins,kills,kdratio,headshots`한 번 호출에서 요청할 수 있는 통계 개수에 제한 되 고 해당 제한은 "갖추었지만" URI 길이 실용성 비교 개발자 편의 위해 신중 하 게 고려 됩니다. 예를 들어 제한 통계 이름 텍스트 (쉼표 포함)의 가치가 600 문자 중 하나 또는 최대 10 통계에 의해 결정 됩니다. 다음과 같은 간단한 GET을 사용 하면 HTTP chatty 클라이언트에서 호출 볼륨은 감소 하는 일반적으로 요청된 통계에 대 한 캐싱의 활성화 합니다. |

<a id="ID4EUC"></a>


## <a name="authorization"></a>권한 부여

권한 부여 논리 콘텐츠 격리 및 액세스 제어 시나리오에 대해 구현 있습니다.

   * 순위표와 사용자 통계 호출자가 요청을 사용 하 여 유효한 XSTS 토큰 전송 된 모든 플랫폼에서 클라이언트에서 읽을 수 있습니다. 쓰기는 데이터 플랫폼에서 지 원하는 클라이언트에 분명히 제한 됩니다.
   * 제목 개발자 열기 또는 XDP 또는 개발자 센터를 사용 하 여 제한 된 통계를 표시할 수 있습니다. 순위표 통계가 엽니다. 사용자가 해당 샌드박스에 인증으로 열기 통계 Smartglass, 뿐만 아니라 iOS, Android, Windows, Windows Phone 및 웹 응용 프로그램에서 액세스할 수 있습니다. 사용자 권한 부여 샌드박스에 XDP 또는 개발자 센터를 통해 관리 됩니다.

검사에 대 한 의사 코드는 다음과 같습니다.


```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.

```


<a id="ID4EPD"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 권한 부여| 문자열| HTTP 인증에 대 한 자격 증명을 인증 합니다. 예제 값: "XBL3.0 x =&lt;userhash >; &lt;토큰 > ".|

<a id="ID4EYE"></a>


## <a name="optional-request-headers"></a>선택적 요청 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X RequestedServiceVersion|  | 이 요청은 전송 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 인증 토큰의 클레임 헤더의 유효성을 확인 한 후 서비스는 합니다. 기본값: 1.|

<a id="ID4E3F"></a>


## <a name="request-body"></a>요청 본문

이 요청의 본문 개체가 전달 됩니다.

<a id="ID4EHG"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드입니다.

서비스는이 리소스에는이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스 사용 하는 표준 HTTP 상태 코드의 전체 목록은, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하십시오.

| Code| 원인 문구| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 확인| 세션을 검색 했습니다.|
| 304| 수정 되지 않음| 리소스 되지 요청 된 마지막으로 수정 합니다.|
| 400| 잘못 된 요청| 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.|
| 401| 권한 없음| 필요한 사용자 인증을 요청 합니다.|
| 403| 사용할 수 없음| 사용자 또는 서비스에 대 한 요청을 허용 되지 않습니다.|
| 404| 찾을 수 없습니다.| 지정된 된 리소스를 찾을 수 없습니다.|
| 406| 허용할 수 없음| 리소스 버전이 지원 되지 않습니다.|
| 408| 요청 시간 제한| 리소스 버전이 지원 되지 않습니다. MVC 계층에 의해 거부 되어야 합니다.|

<a id="ID4E5BAC"></a>


## <a name="response-body"></a>응답 본문

<a id="ID4EECAC"></a>


### <a name="sample-response"></a>샘플 응답입니다.


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


##### <a name="parent"></a>부모

[/users/xuid({xuid})/scids/{scid}/stats](uri-usersxuidscidsscidstats.md)
