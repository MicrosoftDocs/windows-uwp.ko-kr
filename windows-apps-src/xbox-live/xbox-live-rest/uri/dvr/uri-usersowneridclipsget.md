---
title: GET (/users/{ownerId}/clips)
assetID: da972b4e-bc38-66f5-2222-5e79d7c8a183
permalink: en-us/docs/xboxlive/rest/uri-usersowneridclipsget.html
author: KevinAsgari
description: " GET (/users/{ownerId}/clips)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a215e9e1abb6daad2c011f38d56c2e501bd16e40
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4689690"
---
# <a name="get-usersowneridclips"></a>GET (/users/{ownerId}/clips)
사용자의 클립의 목록을 검색 합니다.
이러한 Uri에 대 한 도메인은 `gameclipsmetadata.xboxlive.com` 및 `gameclipstransfer.xboxlive.com`해당 URI의 기능에 따라 합니다.

  * [설명](#ID4EX)
  * [URI 매개 변수](#ID4EEB)
  * [쿼리 문자열 매개 변수](#ID4EPB)
  * [요청 본문](#ID4EPE)
  * [필수 응답 헤더](#ID4E1E)
  * [선택적 응답 헤더](#ID4ENH)
  * [응답 본문](#ID4EOAAC)
  * [관련된 Uri](#ID4EABAC)

<a id="ID4EX"></a>


## <a name="remarks"></a>설명

이 API를 사용 하면 다양 한 방법으로 사용자의 자체 클립 물론 다른 사용자의 클립 서비스에 저장 된 목록입니다. 여러 진입점 수준에서 데이터를 반환 하 고 쿼리 매개 변수를 통해 필터링에 대 한 허용 합니다. 클레임에서 XUID URI에 지정 된 소유자와 일치 하는 경우 콘텐츠 격리 확인 한 후 사용자의 클립 반환 됩니다. URI의 소유자 클레임 XUID와 일치 하지 않는 경우 다음 지정 된 사용자의 클립에 따라 반환 됩니다 개인 정보 보호 검사 및 요청 XUID에 대 한 콘텐츠 격리 검사 합니다.

쿼리 사용자 서비스 구성 id (서비스 안내) 당 최적화 됩니다. 추가 필터 또는 지정 기본값 이외의 정렬 순서 아래에서 지정한 대로 상황에 따라 시간이 오래 걸릴 수를 반환 합니다. 이 사용자 당 동영상의 더 큰 집합에 대 한 더 분명 하 게 합니다.

동일한 API 호출 내에서 여러 사용자의 목록을 가져오기 위해 API 일괄 처리 안 함 있습니다. SLS 설계자에서 (현재) 권장된 패턴은 차례 대로 각 사용자에 대 한 쿼리를 합니다.

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| ownerId| string| 해당 리소스를 액세스 하는 사용자의 사용자 id입니다. 지원 되는 형식: "me" 또는 "xuid(123456789)". 최대 길이: 16입니다.|

<a id="ID4EPB"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- | --- | --- |
| skipItems| 32 비트 부호 있는 정수| 선택 사항입니다. 컬렉션 (즉, N 건너뛰기 항목)에서 N + 1에서 시작 하는 항목을 반환 합니다.|
| continuationToken| string| 선택 사항입니다. 지정 된 연속 토큰에서 시작 하는 항목을 반환 합니다. ContinuationToken 매개 변수 모두 제공 되는 경우 skipItems 보다 우선 합니다. 즉, continuationToken 매개 변수가 있으면 skipItems 매개 변수는 무시 됩니다. 최대 크기: 36 합니다.|
| maxItems| 32 비트 부호 있는 정수| 선택 사항입니다. 최대 (skipItems continuationToken 항목의 범위를 반환할 수와 결합할 수 있습니다) 컬렉션에서 반환할 항목 수입니다. 서비스는 maxItems 존재 하지 않으며 (경우에 결과의 마지막 페이지 아직 반환 되지 않은) maxItems 보다 적은 반환할 수 있습니다 경우 기본값을 제공할 수 있습니다.|
| 순서| 유니코드 문자| 선택 사항입니다. 지정 (D) escending의 목록을 반환 하는 경우 (처음 값 가장 높은) scending (A) 또는 (먼저 값 최저) 순서. 기본값: D.|
| type| GameClipTypes| 선택 사항입니다. 쉼표로 구분 된 집합 반환할 클립의 형식입니다. 기본값: 모든 합니다.|
| 이벤트 Id| string| 선택 사항입니다. 쉼표로 구분 된 집합으로 결과 필터링 하려면 eventIDs입니다. 기본값: Null입니다.|
| 한정자| string| 선택 사항입니다. 클립을 가져오는 데 필요한 사용할 순서 한정자를 지정 합니다. <ul><li>만든-시스템에 클립이 날짜의 순서로 반환 되도록 지정</li><li>-[최고 등급]-등급 클립에서 등급 값을 반환 지정</li><li>보기-[본 가장]-클립 보기의 수를 반환 되도록 지정</li></ul><br/> 최대 크기: 12. 기본값: "만든".| 

<a id="ID4EPE"></a>


## <a name="request-body"></a>요청 본문

이 요청에 대 한 필수 멤버인 없습니다.

<a id="ID4E1E"></a>


## <a name="required-response-headers"></a>필수 응답 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X RequestedServiceVersion| string| 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 인증 토큰 등의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우트된 됩니다. 예: 1, vnext 합니다.|
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>응용 프로그램/j</b>합니다.|
| 캐시 제어| string| 정중 요청 캐싱 동작을 지정 합니다.|
| 수락| string| 콘텐츠 형식의 허용 되는 값입니다. 예: <b>응용 프로그램/j</b>합니다.|
| 다시 시도 후| string| 클라이언트는 사용할 수 없는 서버의 경우 나중에 다시 시도 하도록 지시 합니다.|
| 다| string| 응답을 캐시 하는 방법을 다운스트림 프록시에 지시 합니다.|

<a id="ID4ENH"></a>


## <a name="optional-response-headers"></a>선택적 응답 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Etag| string| 캐시 최적화에 사용 됩니다. 예: "686897696a7c876b7e"입니다.|

<a id="ID4EOAAC"></a>


## <a name="response-body"></a>응답 본문

<a id="ID4EUAAC"></a>


### <a name="sample-response"></a>예제 응답


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

다음 URI는 기본 채널은 서비스 안내를 지정 하는 추가 경로 매개 변수를 사용 하면서이 문서의 동일 합니다. 해당 서비스 안내에 대 한 해당 사용자의 클립만 반환 됩니다. 요청 사용자에 게 요청 된 서비스 안내에 액세스할 수 있어야, 그렇지 않은 경우 HTTP 오류 403 반환 됩니다.

   * **{서비스 안내} {ownerId} /users/ /scids/ /clips**

<a id="ID4ENBAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EPBAC"></a>


##### <a name="parent"></a>부모

[/users/{ownerId}/clips](uri-usersowneridclips.md)
