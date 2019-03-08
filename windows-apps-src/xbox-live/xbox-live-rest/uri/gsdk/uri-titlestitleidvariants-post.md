---
title: POST (/titles/{titleId}/variants)
assetID: 84303448-5a11-d96f-907d-77f57f859741
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidvariants-post.html
description: " POST (/titles/{titleId}/variants)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 17974ddf7dec26abac18ccee9fda5249bc9d656f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618498"
---
# <a name="post-titlestitleidvariants"></a>POST (/titles/{titleId}/variants)
Id입니다. 지정 된 제목의 게임 변형의 목록을 검색 하는 클라이언트에서 호출 하는 URI 이러한 Uri에 대 한 도메인이 `gameserverds.xboxlive.com` 고 `gameserverms.xboxlive.com`입니다.
 
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
| titleid| ID는 요청에서 작동 해야 하는 제목입니다.| 
  
<a id="ID5EG"></a>

 
## <a name="host-name"></a>호스트 이름

gameserverds.xboxlive.com
 
<a id="ID4EIB"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
에 요청을 수행할 때 다음 표에 나와 있는 헤더는 필요 합니다.
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | 
| Content-Type| application/json| 전송 되는 데이터의 형식입니다.| 
| 호스트| gameserverds.xboxlive.com|  | 
| Content-Length|  | 요청 개체의 길이입니다.| 
| x-xbl-contract-version| 1| API 계약 버전입니다.| 
| 권한 부여| XBL3.0 x=[hash];[token]| 인증 토큰입니다.| 
  
<a id="ID4EED"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
에 요청을 수행할 때 다음 표에 나와 있는 헤더는 선택적입니다.
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| X-XblCorrelationId|  | 요청 본문의 mime 형식입니다.| 
  
<a id="ID4E3D"></a>

 
## <a name="authorization"></a>권한 부여

요청을 올바른 Xbox Live 권한 부여 헤더를 포함 해야 합니다. 호출자에 게가이 리소스에 액세스할 수 없습니다, 하는 경우 서비스 403 사용할 수 없음 응답에서 반환 합니다. 헤더가 잘못 되었거나 누락 된 경우 서비스 401 권한 없음 응답에 반환 합니다.
 
<a id="ID4EEE"></a>

 
## <a name="request-body"></a>요청 본문
 
요청에는 다음 멤버로 구성 된 JSON 개체를 포함 해야 합니다.
 
| 멤버| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| locale| 로컬 변형의 반환 합니다.| 
| maxVariants| 반환할 variant의 최대 수입니다.| 
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
 
응답에는 항상 다음 표에 나와 있는 헤더 포함 됩니다.
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Type| application/json| 응답 본문에는 데이터 형식입니다.| 
| Content-Length|  | 응답 본문의 길이입니다.| 
  
<a id="ID4EMG"></a>

 
## <a name="optional-response-headers"></a>선택적 응답 헤더
 
응답을 다음과 같이 헤더 포함이 될 수 있습니다.
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-XblCorrelationId|  | 응답 본문의 mime 형식입니다.| 
  
<a id="ID4EEH"></a>

 
## <a name="response-body"></a>응답 본문
 
호출이 성공 하면 서비스는 다음 멤버로 구성 된 JSON 개체를 반환 합니다.
 
| 멤버| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 변형| Variant의 배열입니다.| 
| variantId| Variant의 Id입니다.| 
| name| Variant의 이름입니다.| 
| isPublisher|  | 
| 순위|  | 
| gameVariantSchemaId|  | 
| variantSchemas| Variant 스키마의 배열입니다.| 
| variantSchemaId| 스키마의 Id입니다.| 
| schemaContent| 스키마 내용| 
| name| 스키마의 이름| 
| gsiSets| GSI 배열이 설정합니다.| 
| minRequiredPlayers| 변형에 대 한 플레이어의 최소 수입니다.| 
| maxAllowedPlayers| 변형에 대 한 플레이어의 최대 수입니다.| 
| gsiSetId| GSI 집합의 Id입니다.| 
| gsiSetName| GSI 집합의 이름입니다.| 
| selectionOrder|  | 
| variantSchemaId| GSI에 사용 된 varaint 스키마의 id를 설정 합니다.| 
 
<a id="ID4EYBAC"></a>

 
### <a name="sample-response"></a>샘플 응답
 

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

  