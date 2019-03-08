---
title: POST (/system/strings/validate)
assetID: 6a59bc0b-8edd-87bf-efaf-f16efa3bedf7
permalink: en-us/docs/xboxlive/rest/uri-systemstringsvalidatepost.html
description: " POST (/system/strings/validate)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 70e86567f449674c7a046e072437d9ee715dc6d6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595508"
---
# <a name="post-systemstringsvalidate"></a>POST (/system/strings/validate)
유효성 검사에 대 한 문자열의 배열을 수락 하 고 동일한 크기의 결과 배열을 반환 합니다. 이러한 Uri에 대 한 도메인은 `client-strings.xboxlive.com`합니다.
 
  * [설명](#ID4EV)
  * [필요한 요청 헤더](#ID4EIB)
  * [요청 본문](#ID4ELC)
  * [HTTP 상태 코드](#ID4E4C)
  * [응답 본문](#ID4ETF)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
각 결과는 해당 문자열의 Xbox LIVE에 적합 하 고 해당 하는 경우 잘못 된 문자열을 포함 하는지 여부를 나타냅니다.
 
항상 동일한 문자열 동일한 결과 제공 합니다. 성공 하지 못한 결과 표시 되 면 결과 분석 하 고 문자열을 적절 하 게 수정 합니다.
 
 

> [!NOTE] 
> 결과 <b>VerifyStringResult</b> 보고서만 문자열에서 첫 번째 잘못 된 단어가 됩니다. 문자열에서 단어를 잘못 추가 수 있습니다. 문자열을 사용할 수 있도록 하려면 잘못 된 단어를 대체 하려는 경우에 잘못 된 단어 또는 부분 문자열을 대체 하 고 추가 잘못 된 부분 문자열을 검색할 문자열을 다시 확인 해야 합니다.  

 
  
<a id="ID4EIB"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | 
| 권한 부여| 인증 토큰입니다. 예제: XBL3.0 x=[hash];[token].| 
| x-xbl-contract-version| 정수 API 계약 버전입니다. 이 API에 대 한 1 또는 2 여야 합니다.| 
  
<a id="ID4ELC"></a>

 
## <a name="request-body"></a>요청 본문
 
요청 본문은 배열의 크기에 제한이 없는 숫자와 문자열 당 512 자 문자열의 배열입니다.
 
<a id="ID4ETC"></a>

 
### <a name="sample-request"></a>샘플 요청
 

```cpp
{
    "stringstoVerify":
    [
        "Poppycock",
        "The quick brown fox jumped over the lazy dog.",
        "Hey, want to hang out?",
        "oh noes"
    ]
}
      
```

   
<a id="ID4E4C"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.
 
| 코드| 이유 구| 설명| 
| --- | --- | --- | --- | --- | --- | 
| 200| 확인| 모든 문자열은 성공적으로 처리 되었습니다. 이 반드시 모든 문자열 양의 HResults를 했습니다.| 
| 401| 권한 없음| 요청에 사용자 인증이 필요합니다.| 
| 403| 사용할 수 없음| 사용자 또는 서비스에 대 한 요청이 허용 되지 않습니다.| 
| 406| 허용 되지 않음| 누락 <b>콘텐츠 형식: application/json</b> 헤더입니다.| 
| 408| 요청 시간 초과| 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.| 
  
<a id="ID4ETF"></a>

 
## <a name="response-body"></a>응답 본문
 
배열을 반환 [VerifyStringResult (JSON)](../../json/json-verifystringresult.md), 요청 배열과 동일한 크기의 합니다.
  
<a id="ID4EAG"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ECG"></a>

 
##### <a name="parent"></a>Parent 

[/system/strings/validate](uri-systemstringsvalidate.md)

   