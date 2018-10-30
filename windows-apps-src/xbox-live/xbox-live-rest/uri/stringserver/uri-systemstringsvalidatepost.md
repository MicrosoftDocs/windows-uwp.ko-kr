---
title: POST (/system/strings/validate)
assetID: 6a59bc0b-8edd-87bf-efaf-f16efa3bedf7
permalink: en-us/docs/xboxlive/rest/uri-systemstringsvalidatepost.html
author: KevinAsgari
description: " POST (/system/strings/validate)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1c3364d3020627aa0d0826a390bf5c4f0b633af8
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5753092"
---
# <a name="post-systemstringsvalidate"></a>POST (/system/strings/validate)
유효성 검사에 대 한 문자열의 배열 받아들이고 같은 크기의 결과 배열을 반환 합니다. 이러한 Uri에 대 한 도메인은 `client-strings.xboxlive.com`.
 
  * [설명](#ID4EV)
  * [필요한 요청 헤더](#ID4EIB)
  * [요청 본문](#ID4ELC)
  * [HTTP 상태 코드](#ID4E4C)
  * [응답 본문](#ID4ETF)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
각 결과 해당 문자열 Xbox LIVE에 허용 되 고 해당 하는 경우 문제가 되는 문자열을 포함 하는지 여부를 나타냅니다.
 
동일한 문자열은 항상 동일한 결과 제공 됩니다. 성공적인 아닌 결과 받을 경우 결과 분석 하 고 그에 따라 문자열을 수정 합니다.
 
 

> [!NOTE] 
> <b>VerifyStringResult</b> 결과만 문자열에서 잘못 된 첫 번째 단어를 보고 합니다. 문자열에서 단어를 잘못 된 추가 수 있습니다. 문자열을 사용할 수 있도록 하려면 잘못 된 단어를 교체 하려는 경우에 잘못 된 단어 또는 하위 문자열을 대체 하 고 잘못 된 문자열을 추가 하는 문자열을 다시 확인 해야 합니다.  

 
  
<a id="ID4EIB"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | 
| 권한 부여| 인증 토큰입니다. 예: XBL3.0 x = [해시]. [토큰]입니다.| 
| xbl 계약 버전 x| 정수 API 계약 버전입니다. 이 API에 대 한 1 또는 2 이어야 합니다.| 
  
<a id="ID4ELC"></a>

 
## <a name="request-body"></a>요청 본문
 
요청 본문은 배열 크기에 제한이 없는 문자열 당 512 문자를 사용 하 여 문자열의 배열입니다.
 
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
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스에 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | 
| 200| 확인| 모든 문자열 성공적으로 처리 합니다. 이 반드시 모든 문자열 양수 Hresult 했습니다.| 
| 401| 권한 없음| 필요한 사용자 인증을 요청 합니다.| 
| 403| 사용할 수 없음| 사용자 또는 서비스에 대 한 요청을 허용 되지 않습니다.| 
| 406| 허용할 수 없음| 누락 <b>콘텐츠 형식: 응용 프로그램/j</b> 헤더.| 
| 408| 요청 시간 제한| 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.| 
  
<a id="ID4ETF"></a>

 
## <a name="response-body"></a>응답 본문
 
[VerifyStringResult (JSON)](../../json/json-verifystringresult.md), 요청 배열와 같은 크기의 배열을 반환합니다.
  
<a id="ID4EAG"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ECG"></a>

 
##### <a name="parent"></a>부모 

[/system/strings/validate](uri-systemstringsvalidate.md)

   