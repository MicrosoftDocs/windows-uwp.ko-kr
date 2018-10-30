---
title: POST (/titles/{titleId}/variants)
assetID: 84303448-5a11-d96f-907d-77f57f859741
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidvariants-post.html
author: KevinAsgari
description: " POST (/titles/{titleId}/variants)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7cc67b61d9130838802774460cd38ec08e2e349b
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5743650"
---
# <a name="post-titlestitleidvariants"></a>POST (/titles/{titleId}/variants)
URI의 id입니다. 지정 된 제목에 대 한 게임 변형의 목록을 검색 하는 클라이언트에 의해 호출 이러한 Uri에 대 한 도메인은 `gameserverds.xboxlive.com` 및 `gameserverms.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EZ)
  * [필요한 요청 헤더](#ID4EIB)
  * [선택적 요청 헤더](#ID4EED)
  * [권한 부여](#ID4E3D)
  * [요청 본문](#ID4EEE)
  * [필요한 응답 헤더](#ID4ELF)
  * [선택적 응답 헤더](#ID4EMG)
  * [응답 본문](#ID4EEH)
 
<a id="ID4EZ"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 설명| 
| --- | --- | 
| titleid| 요청에서 작동 해야 하는 타이틀의 ID입니다.| 
  
<a id="ID5EG"></a>

 
## <a name="host-name"></a>호스트 이름

gameserverds.xboxlive.com
 
<a id="ID4EIB"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
요청을 만들 때 다음 표에 표시 된 헤더는 필요 합니다.
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | 
| 콘텐츠 유형| application/json| 전송 되는 데이터 형식입니다.| 
| 호스트| gameserverds.xboxlive.com|  | 
| Content-Length|  | 요청 개체의 길이입니다.| 
| xbl 계약 버전 x| 1| API 계약 버전입니다.| 
| 권한 부여| XBL3.0 x = [해시]. [토큰]| 인증 토큰입니다.| 
  
<a id="ID4EED"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
요청을 만들 때 다음 표에 표시 된 헤더는 선택적입니다.
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| X XblCorrelationId|  | 요청 본문의 mime 형식입니다.| 
  
<a id="ID4E3D"></a>

 
## <a name="authorization"></a>권한 부여

요청이 유효한 Xbox Live 권한 부여 헤더를 포함 해야 합니다. 호출자에 게가이 리소스에 액세스 하는 허용 되지 않으면, 서비스 응답 403 반환 합니다. 헤더 잘못 되었거나 누락 된 경우 서비스 401 권한 없음 응답에 반환 합니다.
 
<a id="ID4EEE"></a>

 
## <a name="request-body"></a>요청 본문
 
요청에는 다음 멤버가 포함 된 JSON 개체를 포함 해야 합니다.
 
| 멤버| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| locale| 반환 변형의의 로컬 합니다.| 
| maxVariants| 반환할 변형의의 최대 수입니다.| 
| publisherOnly|  | 
| 제한|  | 
 
<a id="ID4EDF"></a>

 
### <a name="sample-request"></a>샘플 요청
 

```cpp
{
  "locale": "en-us",
  "maxVariants": "100",
  "publisherOnly": "false",
  "restriction": null
}

```

   
<a id="ID4ELF"></a>

 
## <a name="required-response-headers"></a>필요한 응답 헤더
 
응답에는 다음 표에 표시 된 헤더 항상 포함 됩니다.
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 콘텐츠 유형| application/json| 응답 본문에는 데이터 형식입니다.| 
| Content-Length|  | 응답 본문의 길이입니다.| 
  
<a id="ID4EMG"></a>

 
## <a name="optional-response-headers"></a>선택적 응답 헤더
 
응답 inlcude를 다음에 표시 된 헤더를 수 있습니다.
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X XblCorrelationId|  | 응답 본문의 mime 형식입니다.| 
  
<a id="ID4EEH"></a>

 
## <a name="response-body"></a>응답 본문
 
호출 되 면 서비스는 다음 멤버가 포함 된 JSON 개체를 반환 합니다.
 
| 멤버| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 변형| 변형의의 배열입니다.| 
| variantId| Variant의 Id입니다.| 
| name| Variant의 이름입니다.| 
| isPublisher|  | 
| 순위|  | 
| gameVariantSchemaId|  | 
| variantSchemas| 변형 스키마의 배열입니다.| 
| variantSchemaId| 스키마의 Id입니다.| 
| schemaContent| 스키마 내용| 
| name| 스키마 이름| 
| gsiSets| GSI 집합의 배열입니다.| 
| minRequiredPlayers| 변형에 대 한 플레이어의 최소 수입니다.| 
| maxAllowedPlayers| 변형에 대 한 플레이어의 최대 수입니다.| 
| gsiSetId| GSI 집합의 Id입니다.| 
| gsiSetName| GSI 집합의 이름입니다.| 
| selectionOrder|  | 
| variantSchemaId| 집합은 GSI에서 사용 되는 varaint 스키마의 id입니다.| 
 
<a id="ID4EYBAC"></a>

 
### <a name="sample-response"></a>예제 응답
 

```cpp
{
 "variants": [
     { 
       "variantId": "8B6EF8A0-7807-42C4-9CB0-1D9B8B8CE742", 
       "name": "tankWarsV2.0",
       "isPublisher": "true",
       "rank": "1",
       "gameVariantSchemaId": "9742DBA5-23FD-4760-9D74-6CFA211B9CFB"
     }],
  "variantSchemas": [
     {
        "variantSchemaId": "9742DBA5-23FD-4760-9D74-6CFA211B9CFB",
        "schemaContent": "&lt;?xml version=\"1.0\" encoding=\"UTF-8\" ?>&lt;xs:schema xmlns:xs=\"http://www.w3.org/2001/XMLSchema\">&lt;xs:element name=\"root\">&lt;/xs:element>&lt;/xs:schema>"
        "name": "tanksSchema"
     }],
     "gsiSets":
     [{ 
          "minRequiredPlayers": "5", 
          "maxAllowedPlayers": "10", 
          "gsiSetId": "B28047F5-B52F-477E-97C2-4C1C39E31D42",
          "gsiSetName": "TanksGSISet",
          "selectionOrder": "1",
          "variantSchemaId": "9742DBA5-23FD-4760-9D74-6CFA211B9CFB"
     }]
 }

  

```

   
<a id="ID4ERCAC"></a>

 
## <a name="see-also"></a>참고 항목
 [/titles/{titleId}/variants](uri-titlestitleidvariants.md)

  