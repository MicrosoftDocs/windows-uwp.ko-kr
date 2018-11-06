---
title: POST (/users/me/scids/{scid}/clips)
assetID: 44535926-9fb8-5498-b1c8-514c0763e6c9
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipspost.html
author: KevinAsgari
description: " POST (/users/me/scids/{scid}/clips)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dd074b6ffc7b5367992c984e7e3c24b036f0f11d
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2018
ms.locfileid: "6026559"
---
# <a name="post-usersmescidsscidclips"></a>POST (/users/me/scids/{scid}/clips)
초기 업로드 요청을 확인 합니다. 이러한 Uri에 대 한 도메인은 `gameclipsmetadata.xboxlive.com` 및 `gameclipstransfer.xboxlive.com`해당 URI의 기능에 따라 합니다.
 
  * [설명](#ID4EX)
  * [URI 매개 변수](#ID4EFB)
  * [권한 부여](#ID4EQB)
  * [필요한 요청 헤더](#ID4EKC)
  * [선택적 요청 헤더](#ID4ENE)
  * [요청 본문](#ID4ENF)
  * [샘플 요청](#ID4E1F)
  * [HTTP 상태 코드](#ID4EDG)
  * [응답 본문](#ID4EVAAC)
  * [예제 응답](#ID4EFBAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>설명
 
이것이 GameClip 업로드 프로세스의 첫 번째 부분입니다. 비디오 캡처를 시 업로드 바로 시작 되도록 예약 되지 않은 경우에 비트, 업로드 ID 및 URI를 가져와야 하는 즉시 GameClips 서비스를 호출 된 것입니다. 이 호출은 사용자 할당량 검사 및 참조 클라이언트에 의해 업로드에 대 한 비디오 경우에 예약 해야 하는 등 개인 정보, 콘텐츠 격리를 통해 다른 검사를 수행 합니다. 이 호출을 통해 긍정적인 응답을 업로드할 비디오 클립을 감수할 서비스를 나타냅니다. 시스템에 적용 되도록 업로드 하는 모든 클립 (서비스 안내)를 통해 특정 제목 연결 되어야 합니다.
 
이 호출은 idempotent; 하지 않습니다. 다른 Id 및 Uri 발급 하는 후속 호출 하면 됩니다. 실패 시 재시도 표준 클라이언트 쪽 백오프 동작을 수행 해야 합니다.
  
<a id="ID4EFB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| 서비스 안내| string| 액세스 되는 리소스의 서비스 구성 ID입니다. 인증된 된 사용자의 서비스 안내 일치 해야 합니다.| 
  
<a id="ID4EQB"></a>

 
## <a name="authorization"></a>권한 부여
 
다음 클레임이이 메서드에 대 한 필요 합니다.
 
   * Xuid
   * DeviceType-업로드 하는 장치가 있어야 합니다.
   * DeviceId
   * TitleId
   * TitleSandboxId
   
<a id="ID4EKC"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증에 대 한 자격 증명을 인증 합니다. 예제 값: <b>Xauth =&lt;authtoken ></b>| 
| X RequestedServiceVersion| string| 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 인증 토큰 등의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우트됩니다 됩니다. 예: 1, vnext 합니다.| 
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>응용 프로그램/j</b>합니다.| 
| 수락| string| 콘텐츠 형식의 허용 되는 값입니다. 예: <b>응용 프로그램/j</b>합니다.| 
  
<a id="ID4ENE"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| string| Encodings 허용 압축 합니다. 예제 값: gzip만 줄이기 identity 합니다.| 
  
<a id="ID4ENF"></a>

 
## <a name="request-body"></a>요청 본문
 
요청 본문에는 [InitialUploadRequest](../../json/json-initialuploadrequest.md) 개체를 JSON 형식 이어야 합니다.
  
<a id="ID4E1F"></a>

 
## <a name="sample-request"></a>샘플 요청
 

```cpp
{
   "clipNameStringId": "1193045",
   "userCaption": "OMG Look at this!",
   "sessionRef": "4587552a-a5ad-4c4c-a787-5bc5af70e4c9",
   "dateRecorded": "2012-12-23T11:08:08Z",
   "durationInSeconds": 27,
   "expectedBlocks": 7,
   "fileSize": 1234567,
   "type": "MagicMoment, Achievement",
   "source": "Console",
   "visibility": "Default",
   "titleData": "{ 'Boss': 'The Invincible' }",
   "systemProperties": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }",
   "thumbnailSource": "Offset",
   "thumbnailOffsetMillseconds": 20000,
   "savedByUser": false
 }
      
```

  
<a id="ID4EDG"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스와 함께 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| 세션을 검색 했습니다.| 
| 400| 잘못 된 요청| 요청 본문에 오류가 발생 하거나 할당량을 통해 사용자가 합니다.| 
| 401| 권한 없음| 요청에 인증 토큰 형식으로 문제가 있습니다.| 
| 403| 금지| 일부 필요한 클레임 누락 되거나 DeviceType 않습니다.| 
| 503| 허용할 수 없음| 서비스 또는 일부 다운스트림 종속성 다운 되었습니다. 표준 백오프 동작을 사용 하 여 다시 시도 합니다.| 
  
<a id="ID4EVAAC"></a>

 
## <a name="response-body"></a>응답 본문
 
응답은 [InitialUploadResponse](../../json/json-initialuploadresponse.md) 또는 JSON 형식의 [ServiceErrorResponse](../../json/json-serviceerrorresponse.md) 개체 수 있습니다.
  
<a id="ID4EFBAC"></a>

 
## <a name="sample-response"></a>예제 응답
 

```cpp
{
   "clipName": "ClipName",
   "gameClipId": "6b364924-5650-480f-86a7-fc002a1ee752",  
   "titleName": "TitleName",
   "uploadUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container",
   "largeThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/large",
   "smallThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/small"
 }
         
```

  
<a id="ID4EOBAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EQBAC"></a>

 
##### <a name="parent"></a>부모 

[/users/me/scids/{scid}/clips](uri-usersmescidclips.md)

   