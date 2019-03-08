---
title: POST (/users/me/scids/{scid}/clips)
assetID: 44535926-9fb8-5498-b1c8-514c0763e6c9
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipspost.html
description: " POST (/users/me/scids/{scid}/clips)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7a8973390ccbf5dd9980410f60f03a7edd78c134
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608788"
---
# <a name="post-usersmescidsscidclips"></a>POST (/users/me/scids/{scid}/clips)
초기 업로드 요청을 확인 합니다. 이러한 Uri에 대 한 도메인이 `gameclipsmetadata.xboxlive.com` 고 `gameclipstransfer.xboxlive.com`해당 URI의 기능에 따라 합니다.
 
  * [설명](#ID4EX)
  * [URI 매개 변수](#ID4EFB)
  * [권한 부여](#ID4EQB)
  * [필요한 요청 헤더](#ID4EKC)
  * [선택적 요청 헤더](#ID4ENE)
  * [요청 본문](#ID4ENF)
  * [샘플 요청](#ID4E1F)
  * [HTTP 상태 코드](#ID4EDG)
  * [응답 본문](#ID4EVAAC)
  * [샘플 응답](#ID4EFBAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>설명
 
클립 업로드 프로세스의 첫 번째 부분입니다. 비디오 캡처 시는 것이 좋습니다 업로드 즉시 시작 되도록 예약 되지 않은 경우에 비트를 업로드 하기 위한 ID 및 URI를 가져오는 즉시 GameClips 서비스를 호출 합니다. 이 호출은 사용자 할당량 검사 및 개인 정보 보호 등에 보려는 경우 비디오도 예약 되도록 업로드에 대 한 클라이언트에서 콘텐츠 격리를 통해 다른 검사를 수행 합니다. 이 호출을 긍정적으로 응답할 서비스 업로드에 대 한 비디오 클립을 수신 하려고를 나타냅니다. 업로드 하는 모든 클립을 시스템에서 허용할 (한 서비스 안내)를 통해 특정 제목을 사용 하 여 연결 되어야 합니다.
 
이 호출은 idempotent; 하지 않습니다. 다양 한 Id 및 Uri 발급할 후속 호출 하면 발생 합니다. 실패 시 다시 시도 동작 백오프 표준 클라이언트 쪽을 따라야 합니다.
  
<a id="ID4EFB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| scid| 문자열| 서비스 액세스 되는 리소스의 ID를 구성 합니다. 인증된 된 사용자의 서비스 안내 일치 해야 합니다.| 
  
<a id="ID4EQB"></a>

 
## <a name="authorization"></a>권한 부여
 
다음 클레임을이이 메서드에 대 한 필요 합니다.
 
   * xuid
   * DeviceType-업로드 하는 장치가 있어야 합니다.
   * DeviceId
   * TitleId
   * TitleSandboxId
   
<a id="ID4EKC"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증을 위해 자격 증명을 인증 합니다. 예제 값: <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| 문자열| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 등 인증 토큰의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우팅됩니다 됩니다. 예제: 1, vnext 합니다.| 
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>application/json</b>합니다.| 
| 수락| 문자열| Content-type의 허용 되는 값입니다. 예: <b>application/json</b>합니다.| 
  
<a id="ID4ENE"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| 문자열| 허용 가능한 압축 인코딩입니다. 예제 값: gzip, deflate, identity입니다.| 
  
<a id="ID4ENF"></a>

 
## <a name="request-body"></a>요청 본문
 
요청 본문 이어야 합니다는 [InitialUploadRequest](../../json/json-initialuploadrequest.md) 개체를 JSON 형식으로 합니다.
  
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
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.
 
| 코드| 이유 구| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| 세션을 검색 했습니다.| 
| 400| 잘못된 요청| 요청 본문에 오류가 발생 했습니다 또는 사용자의 할당량이 초과 되었습니다.| 
| 401| 권한 없음| 요청에 인증 토큰 형식 사용 하 여 문제가 있습니다.| 
| 403| 사용할 수 없음| 일부 필수 클레임이 누락 또는 DeviceType 아닙니다.| 
| 503| 허용 되지 않음| 서비스 또는 일부 다운스트림 종속성 중지 되었습니다. 표준 백오프 동작을 사용 하 여 다시 시도 하세요.| 
  
<a id="ID4EVAAC"></a>

 
## <a name="response-body"></a>응답 본문
 
응답 수를 [InitialUploadResponse](../../json/json-initialuploadresponse.md) 개체 또는 [ServiceErrorResponse](../../json/json-serviceerrorresponse.md) 개체를 JSON 형식으로 합니다.
  
<a id="ID4EFBAC"></a>

 
## <a name="sample-response"></a>샘플 응답
 

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

 
##### <a name="parent"></a>Parent 

[/users/me/scids/{scid}/clips](uri-usersmescidclips.md)

   