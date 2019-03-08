---
title: GET (/users/{ownerId}/clips)
assetID: da972b4e-bc38-66f5-2222-5e79d7c8a183
permalink: en-us/docs/xboxlive/rest/uri-usersowneridclipsget.html
description: " GET (/users/{ownerId}/clips)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7c52daf4a07914c34f1aadc84a7238771669d65f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623208"
---
# <a name="get-usersowneridclips"></a>GET (/users/{ownerId}/clips)
사용자의 클립 목록을 검색 합니다.
이러한 Uri에 대 한 도메인이 `gameclipsmetadata.xboxlive.com` 고 `gameclipstransfer.xboxlive.com`해당 URI의 기능에 따라 합니다.

  * [설명](#ID4EX)
  * [URI 매개 변수](#ID4EEB)
  * [쿼리 문자열 매개 변수](#ID4EPB)
  * [요청 본문](#ID4EPE)
  * [필요한 응답 헤더](#ID4E1E)
  * [선택적 응답 헤더](#ID4ENH)
  * [응답 본문](#ID4EOAAC)
  * [관련된 Uri](#ID4EABAC)

<a id="ID4EX"></a>


## <a name="remarks"></a>설명

이 API를 사용 하면 다양 한 사용자의 자체 뿐 아니라 다른 사용자의 클립 서비스에 저장 되는 클립 목록입니다. 여러 진입점을 여러 수준에서 데이터를 반환 하 고 쿼리 매개 변수를 통해 필터링을 허용 합니다. 클레임의 XUID URI에 지정 된 소유자와 일치 하는 경우 콘텐츠 격리 확인 한 후 사용자의 클립 반환 됩니다. URI에서 소유자 XUID, 클레임 일치 하지 않으면 지정된 된 사용자의 클립 개인 정보 보호 검사 및 요청 XUID에 대 한 콘텐츠 격리 검사에 따라 반환 됩니다.

쿼리는 서비스 구성 id (서비스 안내) 사용자별 최적화 됩니다. 지정 추가 필터 또는 정렬 순서 기본값 이외의 아래 지정 된 대로 일부 상황에서 오래 걸릴 수 있습니다를 반환 합니다. 이 옵션은 사용자 당 비디오의 큰 집합에 대 한 더 분명 합니다.

동일한 API 호출에서 여러 사용자 목록 가져오기에 대 한 일괄 처리 안 함 API 있습니다. SLS 설계자에서 현재 권장 되는 패턴에 각 사용자에 대해 쿼리 하는 것입니다.

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- |
| ownerId| 문자열| 해당 리소스가 액세스 되는 사용자의 사용자 id입니다. 지원 되는 형식: "me" 또는 "xuid(123456789)"입니다. 최대 길이: 16.|

<a id="ID4EPB"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- | --- | --- | --- |
| skipItems| 32 비트 부호 있는 정수| 선택 사항. 컬렉션 (즉, skip N 항목)에서 N + 1부터 시작 하는 항목을 반환 합니다.|
| continuationToken| 문자열| 선택 사항. 지정 된 연속 토큰에서 시작 하는 항목을 반환 합니다. 연속 토큰 매개 변수가 둘 다 제공 하는 경우 skipItems 보다 우선 합니다. 즉, 연속 토큰 매개 변수가 있으면 skipItems 매개 변수가 무시 됩니다. 최대 크기: 36.|
| maxItems| 32 비트 부호 있는 정수| 선택 사항. (항목의 범위를 반환 하는 continuationToken skipItems와 결합할 수 있습니다) 컬렉션에서 반환할 항목의 최대 수입니다. MaxItems 없는 있습니다 maxItems 미만 (경우에 반환할 결과의 마지막 페이지 아직 반환 되지 않은) 경우 서비스는 기본값을 제공할 수 있습니다.|
| 순서| 유니코드 문자| 선택 사항. 목록 (D) escending 반환 됩니다 지정 합니다 (가장 높은 값) (A) 오름차순 또는 (가장 낮은 값) 순서입니다. Default: D.|
| 유형| GameClipTypes| 선택 사항. 반환할 클립 형식의 쉼표로 구분 된 집합입니다. Default: 모든 합니다.|
| eventId| 문자열| 선택 사항. 쉼표로 구분 된 집합에서 결과 필터링 하는 Eventid입니다. Default: null입니다.|
| 한정자| 문자열| 선택 사항. 클립을 가져오기에 사용 되는 순서 한정자를 지정 합니다. <ul><li>만든-클립 체제로 날짜 순서로 반환 되도록 지정</li><li>클립 해당 등급 값으로 반환 되도록 지정-[최고 등급]-등급</li><li>보기-[볼 가장]-클립 뷰의 번호로 반환 되도록 지정</li></ul><br/> 최대 크기: 12. 기본값: "created"입니다.| 

<a id="ID4EPE"></a>


## <a name="request-body"></a>요청 본문

이 요청에 대 한 필수 멤버가 없습니다.

<a id="ID4E1E"></a>


## <a name="required-response-headers"></a>필요한 응답 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion| 문자열| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 등 인증 토큰의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우팅됩니다 됩니다. 예제: 1, vnext 합니다.|
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>application/json</b>합니다.|
| Cache-Control| 문자열| 캐싱 동작을 지정 하는 처리 완료 후 요청입니다.|
| 수락| 문자열| Content-type의 허용 되는 값입니다. 예: <b>application/json</b>합니다.|
| Retry-after| 문자열| 서버를 사용할 수 없는 경우 나중에 다시 시도 하도록 클라이언트를 지시 합니다.|
| 다| 문자열| 다운스트림 프록시 응답을 캐시 하는 방법을 지시 합니다.|

<a id="ID4ENH"></a>


## <a name="optional-response-headers"></a>선택적 응답 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Etag| 문자열| 캐시 최적화를 위해 사용 합니다. 예제: "686897696a7c876b7e".|

<a id="ID4EOAAC"></a>


## <a name="response-body"></a>응답 본문

<a id="ID4EUAAC"></a>


### <a name="sample-response"></a>샘플 응답


```cpp
{
       "gameClips":
       [
           {
               "xuid": "2716903703773872",
               "clipName": "[en-US] Localized Greatest Moment Name",
               "titleName": "[en-US] Localized Title Name",
               "gameClipLocale": "en-US",
               "gameClipId": "6873f6cf-af12-48a5-8be6-f3dfc3f61538-000",
               "state": "Published",
               "dateRecorded": "2013-06-14T01:02:55.4918465Z",
               "lastModified": "2013-06-14T01:05:41.3652693Z",
               "userCaption": "Set by user!",
               "type": "DeveloperInitiated",
               "source": "Console",
               "visibility": "Public",
               "durationInSeconds": 30,
               "scid": "00000000-0000-0012-0023-000000000070",
               "titleId": 354975,
               "rating": 3.75,
               "ratingCount": 245,
               "views": 7453,
               "titleData": "",
               "systemProperties": "",
               "savedByUser": false,
               "achievementId": "AchievementId",
               "greatestMomentId": "GreatestMomentId",
               "thumbnails": [
                   {
                       "uri": "http://localhost/users/xuid(2716903703773872)/scids/00000000-0000-0012-0023-000000000070/clips/6873f6cf-af12-48a5-8be6-f3dfc3f61538-000/thumbnails/large",
                       "fileSize": 637293,
                       "thumbnailType": "Large"
                   },
                   {
                       "uri": "http://localhost/users/xuid(2716903703773872)/scids/00000000-0000-0012-0023-000000000070/clips/6873f6cf-af12-48a5-8be6-f3dfc3f61538-000/thumbnails/small",
                       "fileSize": 163998,
                       "thumbnailType": "Small"
                   }
               ],
               "gameClipUris": [
                   {
                       "uri": "http://localhost/897f65a9-63f0-45a0-926f-05a3155c04fc/GameClip-Original_4000.ism/manifest",
                       "uriType": "SmoothStreaming",
                       "expiration": "2013-06-14T01:10:08.73652Z"
                   },
                   {
                       "uri": "http://localhost/897f65a9-63f0-45a0-926f-05a3155c04fc/GameClip-Original_4000.ism/manifest(format=m3u8-aapl)",
                       "uriType": "Ahls",
                       "expiration": "2013-06-14T01:10:08.73652Z"
                   },
                   {
                       "uri": "http://localhost/users/xuid(2716903703773872)/scids/00000000-0000-0012-0023-000000000070/clips/6873f6cf-af12-48a5-8be6-f3dfc3f61538-000",
                       "fileSize": 88820,
                       "uriType": "Download",
                       "expiration": "2999-12-31T11:59:40Z"
                   }
               ]
           }
       ],
   "pagingInfo":
       {
           "continuationToken": null,
           "totalItems": 1
       }
   }

```


<a id="ID4EABAC"></a>


## <a name="related-uris"></a>관련된 Uri

이 문서에서는 있지만 서비스는 안내를 지정 하는 추가 경로 매개 변수를 사용 하 여 기본 페이지로 다음 URI는 동일 합니다. 해당 서비스 안내에 대 한 해당 사용자의 클립만 반환 됩니다. 요청 하는 사용자는 요청 된 서비스 안내에 액세스할 수 있어야, 그렇지 않으면 HTTP 오류 403 반환 됩니다.

   * **/users/{ownerId}/scids/{scid}/clips**

<a id="ID4ENBAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EPBAC"></a>


##### <a name="parent"></a>Parent

[/users/{ownerId}/clips](uri-usersowneridclips.md)
