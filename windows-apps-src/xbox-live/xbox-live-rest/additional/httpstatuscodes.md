---
title: 표준 HTTP 상태 코드
assetID: 7a19de56-7acd-ad2b-e8e6-53126991093b
permalink: en-us/docs/xboxlive/rest/httpstatuscodes.html
description: " 표준 HTTP 상태 코드"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c77972e88dbf4950f716594ee61220d1734a7fef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635558"
---
# <a name="standard-http-status-codes"></a>표준 HTTP 상태 코드
 
하이퍼텍스트 전송 프로토콜 (HTTP) 표준에 다양을 한 클라이언트 요청에 대 한 응답으로 서버에서 반환 되는 상태 코드를 설명 합니다. Xbox Live 서비스 메서드는 요청의 상태를 설명 하는 HTTP 프로토콜 규격 상태 코드를 반환 합니다.
 
Xbox Live 서비스와 일반적인 의미에서 반환 된 상태 코드 목록은 다음과 같습니다.
 
<a id="ID4EAB"></a>

 
## <a name="xbox-live-services-status-codes"></a>Xbox Live Services 상태 코드
 
| 코드| 이유 구| 설명| 
| --- | --- | --- | 
| 200| 확인| 요청에 성공 합니다.| 
| 201| 만든 날짜| 엔터티를 만들었습니다.| 
| 202| Accepted| 요청이 수락 되었음을 있지만 아직 완료 되지 않았습니다.| 
| 204| 내용 없음| 요청 완료 되었지만 반환 하는 내용을 포함 하지 않습니다.| 
| 301| 영구적으로 이동| 서비스는 다른 URI로 이동 되었습니다.| 
| 302| 찾을 수합니다| 요청된 된 리소스는 다른 URI 아래 일시적으로 상주합니다.| 
| 307| 임시 리디렉션| 이 리소스에 대 한 URI 일시적으로 변경 되었습니다.| 
| 400| 잘못된 요청| 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음| 요청에 사용자 인증이 필요합니다.| 
| 403| 사용할 수 없음| 사용자 또는 서비스에 대 한 요청이 허용 되지 않습니다.| 
| 404| 없음| 지정된 된 리소스를 찾을 수 없습니다.| 
| 406| 허용 되지 않음| 리소스 버전이 지원 되지 않습니다.| 
| 408| 요청 시간 초과| 요청이 너무 길어서 완료 합니다.| 
| 409| 충돌| 리소스의 현재 상태와 충돌로 인해 요청이 완료 되지 않았습니다.| 
| 410| 완료| 요청된 된 리소스를 더 이상 사용할 수 없습니다.| 
| 412| 전제 조건 실패| 서버 요청에 요청자에 게 추가 하는 사전 조건 중 하나를 충족 하지 않습니다.| 
| 416| 요청한 범위가 충분 하지 않음| 요청 된 범위를 사용할 수 없는 경우| 
| 500| 내부 서버 오류| 서버에는 요청을 처리 하지 못하게 하는 예기치 않은 상황이 발생 합니다.| 
| 501| 구현 되지 않음| 서버에서 요청을 처리 하는 데 필요한 기능을 지원 하지 않습니다.| 
| 503| 서비스 이용 불가| 요청이 제한 되었습니다, 그리고 요청 클라이언트 다시 시도 값 (초) (예: 5 초) 후에 다시 시도 하십시오.| 
 

> [!NOTE] 
> 일부 리소스 및 메서드는 해당 리소스 또는 메서드의 컨텍스트 내에서 특정 상태 코드의 의미에 대 한 특정 정보를 제공합니다. 자세한 내용은 사용 중인 리소스에 대 한 설명서를 참조 하십시오. 

  
<a id="ID4E3BAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E5BAC"></a>

 
##### <a name="parent"></a>Parent  

[추가 참조](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4EKCAC"></a>

 
##### <a name="reference--universal-resource-identifier-uri-referenceuriatoc-xboxlivews-reference-urismd"></a>참조 [유니버설 리소스 식별자 (URI) 참조](../uri/atoc-xboxlivews-reference-uris.md)

 [추가 참조](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4EZCAC"></a>

 
##### <a name="external-links--w3org-http11-status-code-definitionshttpswwww3orgprotocolsrfc2616rfc2616-sec10htmlsec10"></a>외부 링크 [W3.org: Http/1.1 상태 코드 정의](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10)

   