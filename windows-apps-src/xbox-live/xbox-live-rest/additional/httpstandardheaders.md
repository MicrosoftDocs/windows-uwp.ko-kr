---
title: 표준 HTTP 요청 및 응답 헤더
assetID: a5f8fd96-9393-5234-04ad-837e5c117c92
permalink: en-us/docs/xboxlive/rest/httpstandardheaders.html
description: " 표준 HTTP 요청 및 응답 헤더"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5dea70403b2a6a7c61be2761d1f5d1ff3d75ccb6
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8351893"
---
# <a name="standard-http-request-and-response-headers"></a>표준 HTTP 요청 및 응답 헤더
 
<a id="ID4ES"></a>

 
## <a name="request-headers"></a>요청 헤더
 
다음 표에서 Xbox Live 서비스 요청을 만들 때 사용 하 여 표준 HTTP 헤더를 보여 줍니다.
 
| 헤더| 값| 설명| 
| --- | --- | --- | 
| xbl 계약 버전 x| 1| API 계약 버전입니다. 모든 Xbox Live 서비스 요청에 필요합니다.| 
| 권한 부여| STSTokenString| STS 인증 토큰입니다. 이 헤더에 대 한 값은 <b>GetTokenAndSignatureResult.Token</b> 속성에서 검색 됩니다. | 
| Content-Type| 응용 프로그램/xml, 응용 프로그램/j, multipart/양식 데이터 또는 응용 프로그램/x-으로-양식-urlencoded| 요청으로 전송 되는 콘텐츠 유형을 지정 합니다.| 
| Content-Length| 정수 값| POST 요청에 전송 되는 데이터의 길이 지정 합니다.| 
| Accept Language | 문자열| 반환 되는 모든 문자열을 지역화 하는 방법을 지정 합니다. 유효한 언어/로캘 조합 목록은 <a href="http://msdn.microsoft.com/en-us/library/bb975829.aspx">고급 Xbox 360 프로그래밍</a> 을 참조 하세요.| 
  
<a id="ID4E6C"></a>

 
## <a name="response-headers"></a>응답 헤더
 
다음 표에서 Xbox Live 서비스 응답에 사용 되는 표준 HTTP 헤더를 보여 줍니다.
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | --- | 
| Content-Type| 응용 프로그램/xml, 응용 프로그램/j| 반환 되는 콘텐츠 유형을 지정 합니다.| 
| Content-Length| 정수 값| 반환할 데이터의 길이 지정 합니다.| 
  
<a id="ID4EEE"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EGE"></a>

 
##### <a name="parent"></a>부모  

[추가 참조](atoc-xboxlivews-reference-additional.md)

   