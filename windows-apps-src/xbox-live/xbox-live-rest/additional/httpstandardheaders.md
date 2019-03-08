---
title: 표준 HTTP 요청 및 응답 헤더
assetID: a5f8fd96-9393-5234-04ad-837e5c117c92
permalink: en-us/docs/xboxlive/rest/httpstandardheaders.html
description: " 표준 HTTP 요청 및 응답 헤더"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ca97e82365eab40266b3ffdd84924f71289eede6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636518"
---
# <a name="standard-http-request-and-response-headers"></a>표준 HTTP 요청 및 응답 헤더
 
<a id="ID4ES"></a>

 
## <a name="request-headers"></a>요청 헤더
 
다음 표에서 Xbox Live 서비스 요청을 생성할 때 사용 되는 표준 HTTP 헤더를 나열 합니다.
 
| 헤더| 값| 설명| 
| --- | --- | --- | 
| x-xbl-contract-version| 1| API 계약 버전입니다. Xbox Live 서비스에 대 한 모든 요청에 필요합니다.| 
| 권한 부여| STSTokenString| STS 인증 토큰입니다. 이 헤더의 값에서 검색 되는 <b>GetTokenAndSignatureResult.Token</b> 속성입니다. | 
| Content-Type| application/xml, application/json, 다중 파트/폼 데이터 또는 응용 프로그램/x-www-형식-urlencoded| 요청과 함께 전송 되는 콘텐츠의 형식을 지정 합니다.| 
| Content-Length| 정수 값| POST 요청에 전송 되는 데이터의 길이 지정 합니다.| 
| Accept-Language | 문자열| 반환 된 모든 문자열을 지역화 하는 방법을 지정 합니다. 참조 <a href="https://msdn.microsoft.com/en-us/library/bb975829.aspx">Xbox 360 프로그래밍 고급</a> 유효한 언어/로캘에서 조합의 목록에 대 한 합니다.| 
  
<a id="ID4E6C"></a>

 
## <a name="response-headers"></a>응답 헤더
 
다음 표에서 Xbox Live 서비스 응답에 사용 되는 표준 HTTP 헤더를 나열 합니다.
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | --- | 
| Content-Type| application/xml, application/json| 반환 되는 콘텐츠의 형식을 지정 합니다.| 
| Content-Length| 정수 값| 반환 되는 데이터의 길이 지정 합니다.| 
  
<a id="ID4EEE"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EGE"></a>

 
##### <a name="parent"></a>Parent  

[추가 참조](atoc-xboxlivews-reference-additional.md)

   