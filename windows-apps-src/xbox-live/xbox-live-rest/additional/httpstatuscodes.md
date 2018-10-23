---
title: 표준 HTTP 상태 코드
assetID: 7a19de56-7acd-ad2b-e8e6-53126991093b
permalink: en-us/docs/xboxlive/rest/httpstatuscodes.html
author: KevinAsgari
description: " 표준 HTTP 상태 코드"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 856b387825734fb7c6973293bc7004a79d05c207
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/22/2018
ms.locfileid: "5398705"
---
# <a name="standard-http-status-codes"></a>표준 HTTP 상태 코드
 
하이퍼텍스트 전송 프로토콜 (HTTP) 표준 다양 한 클라이언트 요청에 대 한 응답으로 서버에 의해 반환 되는 상태 코드를 설명 합니다. Xbox Live 서비스 메서드 요청 상태를 설명 하기 위해 HTTP 프로토콜 규격 상태 코드를 반환 합니다.
 
Xbox Live 서비스 및 일반적인 그 의미를 반환한 상태 코드의 목록은 다음과 같습니다.
 
<a id="ID4EAB"></a>

 
## <a name="xbox-live-services-status-codes"></a>Xbox Live 서비스 상태 코드
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | 
| 200| 확인| 요청이 성공 했습니다.| 
| 201| 생성| 엔터티를 만들었습니다.| 
| 202| Accepted| 요청이 수락 되었습니다 하지만 아직 완료 되지 않았습니다.| 
| 204| 콘텐츠| 요청 완료 되 면 있지만 콘텐츠를 반환할 수 없습니다.| 
| 301| 영구적으로 이동| 서비스는 다른 URI로 이동 합니다.| 
| 302| 발견| 요청 된 리소스가 다른 uri 일시적으로 합니다.| 
| 307| 임시 리디렉션| 이 리소스에 대 한 URI 일시적으로 바뀌었습니다.| 
| 400| 잘못 된 요청| 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음| 필요한 사용자 인증을 요청 합니다.| 
| 403| 사용할 수 없음| 사용자 또는 서비스에 대 한 요청을 허용 되지 않습니다.| 
| 404| 찾을 수 없습니다.| 지정된 된 리소스를 찾을 수 없습니다.| 
| 406| 허용할 수 없음| 리소스 버전이 지원 되지 않습니다.| 
| 408| 요청 시간 제한| 요청을 완료 하는 데 너무 오래 걸렸습니다.| 
| 409| 충돌| 리소스의 현재 상태를 사용 하 여 충돌 하는 요청이 완료 되지 않았습니다.| 
| 410| 최신 상태가 아닌| 요청 된 리소스는 더 이상 사용할 수 없습니다.| 
| 412| 전제 조건 실패| 서버 요청 자가 요청에 직면할 전제 조건 중 하나를 충족 하지 않습니다.| 
| 416| 요청한 범위가 충분 하지 않음| 요청 된 범위를 사용할 수 없는 경우| 
| 500| 내부 서버 오류| 서버에서 요청을 수행할 수 있는 예상치 못한 상황이 발생 합니다.| 
| 501| 구현 되지 않음| 서버에서 요청을 수행 하는 데 필요한 기능을 지원 하지 않습니다.| 
| 503| 사용할 수 없는 서비스| 요청을 제한, 클라이언트 재시도 값 초 (예: 5 초) 한 후 다시 시도 합니다.| 
 

> [!NOTE] 
> 일부 리소스 및 메서드는 해당 리소스 또는 메서드의 컨텍스트 내에서 특정 상태 코드의 의미에 대 한 특정 정보를 제공합니다. 자세한 내용은 리소스나 사용 하는 방법에 대 한 설명서를 참조 하세요. 

  
<a id="ID4E3BAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E5BAC"></a>

 
##### <a name="parent"></a>부모  

[추가 참조](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4EKCAC"></a>

 
##### <a name="reference--universal-resource-identifier-uri-referenceuriatoc-xboxlivews-reference-urismd"></a>참조 [유니버설 리소스 URI (Identifier) 참조](../uri/atoc-xboxlivews-reference-uris.md)

 [추가 참조](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4EZCAC"></a>

 
##### <a name="external-links--w3org-http11-status-code-definitionshttpwwww3orgprotocolsrfc2616rfc2616-sec10htmlsec10"></a>외부 링크 [W3.org: HTTP/1.1 상태 코드 정의](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10)

   