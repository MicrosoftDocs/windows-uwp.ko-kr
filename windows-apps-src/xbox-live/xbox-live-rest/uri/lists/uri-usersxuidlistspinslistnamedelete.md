---
title: DELETE (/users/xuid(xuid)/lists/PINS/{listname})
assetID: b43e3faa-7791-8bcb-3aec-7bdad8ffbebf
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnamedelete.html
author: KevinAsgari
description: " DELETE (/users/xuid(xuid)/lists/PINS/{listname})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0749229af844c7b71496351000cadece7544bfc6
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5469144"
---
# <a name="delete-usersxuidxuidlistspinslistname"></a>DELETE (/users/xuid(xuid)/lists/PINS/{listname})
목록에서 항목을 제거합니다. 이러한 Uri의 도메인은 `eplists.xboxlive.com`.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EIB)
  * [쿼리 문자열 매개 변수](#ID4ETB)
  * [권한 부여](#ID4ETC)
  * [필요한 요청 헤더](#ID4EAD)
  * [요청 본문](#ID4EWE)
  * [HTTP 상태 코드입니다.](#ID4EBF)
  * [응답 본문](#ID4E6BAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
제거할 항목 목록에서 인덱스를 통해 표시 하 고 쿼리 문자열 매개 변수 **인덱스**에 식별 됩니다. 인덱스 목록을 쉼표로 구분 된 및 단일 호출에 100 개의 인덱스를 전달할 수 있습니다. 그러나 인덱스가 (빈 쿼리 문자열 매개 변수)을 전달 하는 경우 전체 목록의 내용을 삭제 됩니다 (빈 목록을 유지 됩니다 및 버전 번호를 계속 증가 됩니다). 항목이 제거 되 면 목록을 "압축"를 빈 인덱스 순서에 남아 있습니다. 따라서이 호출은 idempotent 아닙니다.
 
이 호출 해야는 **경우-일치: versionNumber** 헤더를 **versionNumber** 는 파일의 현재 버전 번호는 요청에 포함 되어야 합니다. 포함 하지 않거나 현재 목록 버전 번호를는 HTTP 412 (전제 조건 실패)와 일치 하지 않는 경우에 상태 코드 반환 되 고 응답 본문의 현재 버전 번호를 포함 하는 목록 최신 메타 데이터를 포함 됩니다. 다른에서 무시 되는 다른 클라이언트에서 업데이트를 방지 하 수행 됩니다.
  
<a id="ID4EIB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid| string| Xbox 사용자 ID (XUID)입니다.| 
| listtype| string| 유형 (사용 하는 방법 및 작동 방식을) 목록입니다. 항상 이러한 "고정" 관련 메서드.| 
| listname| string| 목록 이름 (의 지정 된 listtype 어떤 목록). 항상 "XBLPins"에 대 한 Pin 항목입니다.| 
  
<a id="ID4ETB"></a>

 
## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | 
| 인덱스| string| 선택 사항입니다. 항목을 제거 하는 위치를 지정 합니다. 값을 지원: 0, 양의 정수 및 "종료". 기본값: 0입니다.| 
 
예: **인덱스 = 1, 8** 인덱스 1과 8에는 항목을 제거 합니다. 인덱스는 고유 해야 합니다. 인덱스가 제공 하는 경우 전체 목록을 지워집니다.
  
<a id="ID4ETC"></a>

 
## <a name="authorization"></a>권한 부여
 
이 호출은 **권한 부여** 헤더에서 XSTS SAML 토큰이 필요합니다. Xuid 클레임 호출자를 식별 하는 SAML 토큰 내에 있어야 합니다. 이 값은 호출자에 게 해당 목록 데이터에 대 한 액세스 권한을 있는지 확인 하려면 사용 됩니다. 목록 자체 것도으로 Xuid로 식별 하 고 목록에 대 한 URI에 포함 됩니다. 이 사용 하 여, 될 가능성은 나중에 목록에 대 한 지원 공유 액세스 하지만 하는 기능이 아니며 지금은. 현재 사용자가 액세스 하는 모든 목록 자신의 공유 액세스할 수 없습니다. 따라서 URI의 Xuid SAML 클레임 토큰에 Xuid 일치 해야 합니다. 

> [!NOTE] 
> 현재 XBL 인증 2.0 및 3.0 토큰을 모두 지원 됩니다. 


  
<a id="ID4EAD"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 인증 하 고 요청을 인증 하는 데 STS 토큰이 포함 되어 있습니다. XSTS 서비스에서 토큰 이어야 하 고 클레임 중 하나로 XUID를 포함 해야 합니다. | 
| XBL 계약 버전 X| 요청 된 (양의 정수) 되 고 API 버전을 지정 합니다. 버전 2의 핀 지원 합니다. 이 헤더 없거나 값 서비스 400-반환는 지원 되지 않습니다 "지원 되지 않는 없거나 계약 버전 헤더" 상태 설명에 잘못 된 요청 합니다.| 
| Content-Type| 요청/응답 본문의 콘텐츠를 json 또는 xml 수 있는지 여부를 지정 합니다. 지원 되는 값은 "application/json" 및 "application/xml"| 
| If-Match| 이 헤더 수정 요청을 만들 때 목록의 현재 버전 번호를 포함 해야 합니다. 요청 사용 PUT, POST, 수정 하거나 동사를 삭제할. 버전은 "" 헤더에서 현재 버전의 목록 일치 하지 않으면, HTTP 412 요청이 거부 됩니다 – 전제 조건 반환 코드에 실패 했습니다. (선택 사항) 사용할 수도 있습니다을 가져온에 대 한 필드에 현재 및 전달된 버전 일치 현재 목록 버전 다음 목록 데이터 없음 HTTP 304-수정 되지 코드가 반환 하는 경우. | 
  
<a id="ID4EWE"></a>

 
## <a name="request-body"></a>요청 본문
 
이 요청의 본문 개체가 전달 됩니다.
  
<a id="ID4EBF"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드입니다.
 
서비스는이 리소스에는이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스 사용 하는 표준 HTTP 상태 코드의 전체 목록은, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하십시오.
 
| Code| 원인 문구| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| 요청이 성공적으로 완료 합니다. 응답 본문 (GET)에 대 한 요청 된 리소스를 포함 해야 합니다. POST 및 PUT 요청 (예: 목록 버전, 개수) 최신 목록을 메타 데이터를 받게 됩니다.| 
| 201| 생성| 새 목록 작성 되었습니다. 목록에 초기 삽입 반환 됩니다. 응답 목록에 최신 메타 데이터를 포함 하 고 위치 헤더 목록에 대 한 URI를 포함 합니다.| 
| 304| 수정 되지 않음| 반환 된 가져옵니다. 클라이언트에 최신 버전의 목록 나타냅니다. 서비스 목록 버전 <b>일치 경우</b> 헤더에 있는 값을 비교합니다. 같으면는 304 데이터가 없는 반환 가져옵니다.| 
| 400| 잘못 된 요청| 요청이 잘못 되었습니다. 잘못 된 값 또는 URI 또는 쿼리 문자열 매개 변수의 형식 수 있습니다. 누락 된 필수 매개 변수의 결과 수 또는 데이터 값 이나 요청 누락 되거나 잘못 된 버전의 API 표시 합니다. <b>XBL 계약 버전 X</b> 헤더를 참조 하세요.| 
| 401| 권한 없음| 필요한 사용자 인증을 요청 합니다.| 
| 403| 사용할 수 없음| 사용자 또는 서비스에 대 한 요청을 허용 되지 않습니다.| 
| 404| 찾을 수 없습니다.| 호출자는 리소스에 액세스할 수 없습니다. 이러한 목록이 이미 만들었다고 나타냅니다.| 
| 412| 전제 조건 실패| 버전의 목록 변경 요청은 수정에 실패 한 되었음을 나타냅니다. 수정 요청 삽입, 업데이트 또는 제거 됩니다. 서비스 목록 버전에 대 한 <b>경우 일치</b> 헤더를 확인합니다. 현재 버전의 목록, 일치 하지 않으면 다음 작업이 실패 하 고 (현재 버전 포함)는 현재 목록 메타 데이터와 함께 반환 됩니다.| 
| 500| 내부 서버 오류| 서비스 요청 서버 쪽 오류로 인해 거부 됩니다.| 
| 501| 구현 되지 않음| 호출자에 게 요청 URI를 서버에 구현 되지 않았습니다. (현재 때 요청을 사용 하는 전용 대상인 허용 목록에 포함 되지 않은 목록 이름.)| 
| 503| 사용할 수 없는 서비스| 서버가 과도 한 부하도 인해 일반적으로 요청을 거부 됩니다. 된 지연 시간 후 (참조를 <b>다시 시도 후</b> 헤더), 요청을 다시 시도할 수 있습니다.| 
  
<a id="ID4E6BAC"></a>

 
## <a name="response-body"></a>응답 본문
 
<a id="ID4EFCAC"></a>

 
### <a name="sample-response"></a>샘플 응답입니다.
 

```cpp
{
  "ListVersion":  1,
  "ListCount":  1,
  "MaxListSize": 200,
  "AllowDuplicates": "true",
  "AccessSetting":  "OwnerOnly"
}        
         
```

   
<a id="ID4EPCAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ERCAC"></a>

 
##### <a name="parent"></a>부모 

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

   