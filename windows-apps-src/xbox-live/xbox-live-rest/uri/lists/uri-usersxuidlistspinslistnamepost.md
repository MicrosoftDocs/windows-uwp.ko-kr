---
title: POST (/users/xuid(xuid)/lists/PINS/{listname})
assetID: 813c0bd2-aca9-a1f6-9e81-a84a4c897b1e
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnamepost.html
author: KevinAsgari
description: " POST (/users/xuid(xuid)/lists/PINS/{listname})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 77789014fcd242aad70e7302b01907bc10b8bbab
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4748302"
---
# <a name="post-usersxuidxuidlistspinslistname"></a>POST (/users/xuid(xuid)/lists/PINS/{listname})
쿼리 문자열 매개 변수 **insertIndex**에 따라 인덱스 목록에 항목을 삽입 합니다. 이러한 Uri에 대 한 도메인은 `eplists.xboxlive.com`.
 
  * [설명](#ID4EY)
  * [URI 매개 변수](#ID4ETB)
  * [쿼리 문자열 매개 변수](#ID4E5B)
  * [권한 부여](#ID4EZC)
  * [HTTP 상태 코드](#ID4EGD)
  * [필요한 요청 헤더](#ID4EEAAC)
  * [샘플 요청](#ID4E1BAC)
  * [응답 본문](#ID4EPCAC)
 
<a id="ID4EY"></a>

 
## <a name="remarks"></a>설명
 
이 호출 된 인덱스는 쿼리 문자열 매개 변수 **insertIndex** (기본값은 0 또는 목록 부분)에 따라 목록에 항목을 삽입 됩니다. 요청 본문에 있는 모든 항목 목록에서 해당 지점에 삽입 됩니다. **InsertIndex** 기존 목록에 항목 개수 보다 큰 경우 새 항목의 끝에 삽입 됩니다.
 
항목을 삽입 하는 기능 사양;에 표시 된 필수 필드 있어야 합니다. 그렇지 않은 경우 HTTP 400 반환 됩니다. 마찬가지로, 목록 (목록 종류에 따라 정의 됨)의 최대 크기를 초과 하는 삽입의 결과 다음 HTTP 400 반환 되 고 아무것도 삽입 됩니다.
 
항목이 *하지* 시작 또는 목록의 끝에 삽입 되 면 **경우-일치: versionNumber** 헤더는 요청에 포함 해야 합니다. 헤더 시작 또는 종료를 삽입 하는 경우에 선택 사항입니다. 있는 경우 헤더 삽입 위치에 관계 없이 유효성이 검사 됩니다. 헤더 **VersionNumber** 목록의 현재 버전 번호입니다. 하지 포함 되어 있으며, 필요한 경우 현재 목록 버전 번호를 다음는 HTTP 412 (전제 조건 실패)와 일치 하지 않는 또는 상태 코드가 반환 하 고 응답 본문의 현재 버전 번호를 포함 하는 목록 최신 메타 데이터를 포함 됩니다. 서로에서 무시 되는 다른 클라이언트에서 업데이트를 방지 하기 위해서입니다.
 
이 호출 되지 않도록 idempotent; 참고 동일한 데이터를 사용 하 여 반복 해 서 호출 여러 삽입 될 수 있습니다. 그러나 목록이 현재 지원 하므로 중복, 반복 해 서 호출 HTTP 400 될 가능성이 반환 코드 됩니다.
  
<a id="ID4ETB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid| string| Xbox 사용자 ID (XUID)입니다.| 
| listtype| string| 유형 (사용 하는 방법 및 작동 방식을) 목록입니다. 항상 이러한 "핀" 관련 메서드.| 
| listname| string| 목록 이름 (의 지정 된 listtype 어떤 목록). 항상 "XBLPins"에 대 한 Pin 항목입니다.| 
  
<a id="ID4E5B"></a>

 
## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | 
| insertIndex| string| 선택 사항입니다. 항목을 삽입 위치를 정의 합니다. 지원 되는 값: 0, 양의 정수 및 "종료". 목록 항목 개수 보다 큰 인덱스 값 목록 맨 아래에 새 항목 추가 하 고 "공백" 목록에 삽입 되지 않습니다. 기본값: 0입니다.| 
  
<a id="ID4EZC"></a>

 
## <a name="authorization"></a>권한 부여
 
이 호출은 **권한 부여** 헤더에서 XSTS SAML 토큰이 필요합니다. Xuid 클레임 호출자를 식별 하는 SAML 토큰 내에 있어야 합니다. 이 값은 호출자에 해당 목록 데이터에 대 한 액세스 권한을 있는지 확인 하려면 사용 합니다. 목록 자체 것도 Xuid로 식별 하 고 목록에 대 한 URI에 포함 됩니다. 이 사용 하 여, 될 가능성은 나중에 목록에 대 한 지원 공유 액세스 하지만 하는 기능이 아니며이 시간에 합니다. 현재 사용자가 액세스 하는 모든 목록 자신의 공유 액세스할 수 없습니다. 따라서 URI의 Xuid SAML 클레임 토큰에 Xuid 일치 해야 합니다. 

> [!NOTE] 
> 현재 XBL 인증 2.0 및 3.0 토큰을 모두 지원 됩니다. 


  
<a id="ID4EGD"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스 사용 되는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| 요청이 성공적으로 완료 합니다. 응답 본문 (GET)에 대 한 요청 된 리소스를 포함 해야 합니다. POST 및 PUT 요청 (예: 목록 버전, 개수) 최신 목록 메타 데이터를 받게 됩니다.| 
| 201| 생성| 새 목록 작성 되었습니다. 이 목록에 초기 삽입에 반환 됩니다. 응답 목록에 최신 메타 데이터를 포함 하 고 위치 헤더 목록에 대 한 URI를 포함 합니다.| 
| 304| 수정 되지 않음| 반환 된 가져옵니다. 클라이언트에 최신 버전의 목록의 있음을 나타냅니다. 서비스는 <b>헤더 목록 버전</b> 값과 비교 합니다. 같으면는 304 데이터가 없는 반환 가져옵니다.| 
| 400| 잘못 된 요청| 요청이 잘못 되었습니다. 잘못 된 값 또는 URI 또는 쿼리 문자열 매개 변수의 형식을 사용할 수 있습니다. 누락 된 필수 매개 변수의 결과 수 또는 데이터 값 또는 요청 누락 되거나 잘못 된 버전의 API 표시 합니다. <b>X XBL-계약 버전</b> 헤더를 참조 하세요.| 
| 401| 권한 없음| 요청은 사용자 인증이 필요합니다.| 
| 403| 금지| 사용자 또는 서비스에 대 한 요청을 허용 되지 않습니다.| 
| 404| 찾을 수 없음| 호출자는 리소스에 액세스할 수 없습니다. 이러한 목록이 이미 만들었다고 나타냅니다.| 
| 412| 전제 조건 실패| 버전의 목록 변경 요청은 수정에 실패 한 되었음을 나타냅니다. 요청은 수정 삽입, 업데이트 또는 제거 됩니다. 서비스는 <b>헤더 목록 버전</b> 을 확인합니다. 현재 버전의 목록 일치 하지 않으면, 다음 작업이 실패 하 고 (현재 버전 포함)는 현재 목록 메타 데이터와 함께이 반환 됩니다.| 
| 500| 내부 서버 오류| 서비스 서버 쪽 오류로 인해 요청이 거부 됩니다.| 
| 501| 구현 되지 않음| 호출자 요청 URI를 서버에 구현 되지 않았습니다. (현재 때 요청 사용만 대상인 허용 목록에 포함 되지 않은 목록 이름.)| 
| 503| 사용할 수 없는 서비스| 서버가 과도 한 부하 인해 일반적으로 요청을 거부 됩니다. 된 지연 시간 후 (참조를 <b>다시 시도 후</b> 헤더), 요청을 다시 시도할 수 있습니다.| 
  
<a id="ID4EEAAC"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 인증 하 고 요청을 인증 하는 데 STS 토큰이 포함 되어 있습니다. XSTS 서비스에서 토큰 이어야 하 고는 XUID 클레임 중 하나로 포함 해야 합니다. | 
| X XBL-계약 버전| 요청 된 (양의 정수) 되 고 API 버전을 지정 합니다. 버전 2의 핀 지원 합니다. 이 헤더 없거나 값 서비스 400 – 반환는 지원 되지 않습니다 헤더로"지원 되지 않는 없거나 계약 버전" 상태 설명에 잘못 된 요청 합니다.| 
| Content-Type| 요청/응답 본문의 콘텐츠 json 또는 xml에서 수 있는지 여부를 지정 합니다. 지원 되는 값은 "application/json" 및 "application/xml"| 
| If-Match| 이 헤더 수정 요청을 만들 때 목록의 현재 버전 번호를 포함 해야 합니다. 요청 사용 PUT, POST를 수정 하거나 동사를 삭제 합니다. 버전은 "" 헤더에서 현재 버전의 목록 일치 하지 않으면, HTTP 412 요청이 거부 됩니다 – 전제 조건 반환 코드에 실패 했습니다. (선택 사항) 또한 경우 사용할 수 있습니다을 가져온에 대 한 현재 목록 버전 다음 목록 데이터 없음 및 HTTP 304 있고 전달된 버전 일치 – 코드 수정 되지 반환 됩니다. | 
  
<a id="ID4E1BAC"></a>

 
## <a name="sample-request"></a>샘플 요청
 
**ContentType**, **ItemId** 또는 **ProviderId**, **공급자** 및 **로캘** 는 필수입니다.
 

```cpp
{"Items":
  [{
    "ContentType": "Movie",
    "ItemId": "3a5095a5-eac3-4215-944d-27bc051faa47",
    "ProviderId": "",
    "Provider": "",
    "ImageUrl": "http://www.bing.com/thumb/get?bid=Gw%2fsjCGSS4kAAQ584x800&bn=SANGAM&fbid=7wIR63+Clmj+0A&fbn=CC", 
    "AltImageUrl": null, 
    "Title": "The Dark Knight", 
    "SubTitle": null, 
    "Locale": "en-us",
    "DeviceType": "XboxOne"
  }]}
      
```

  
<a id="ID4EPCAC"></a>

 
## <a name="response-body"></a>응답 본문
 
<a id="ID4EVCAC"></a>

 
### <a name="sample-response"></a>예제 응답
 

```cpp
{
  "ListVersion":  1,
  "ListCount":  1,
  "MaxListSize": 200,
  "AllowDuplicates": "true",
  "AccessSetting":  "OwnerOnly"
}        
         
```

   
<a id="ID4E6CAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EBDAC"></a>

 
##### <a name="parent"></a>부모 

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

   