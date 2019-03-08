---
title: PUT (/users/me/gamerpic)
assetID: ddf71c62-197d-a81d-35a7-47c6dc9e1b0c
permalink: en-us/docs/xboxlive/rest/uri-usersmegamerpicput.html
description: " PUT (/users/me/gamerpic)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7aedc7cbd8366c9cb8d3a60e2cb1f5e843b24a8a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661658"
---
# <a name="put-usersmegamerpic"></a>PUT (/users/me/gamerpic)
1080 x 1080 gamerpic를 업로드합니다. 
  * [요청 본문](#ID4EQ)
  * [HTTP 상태 코드](#ID4EZ)
  * [응답 본문](#ID4EXC)
 
<a id="ID4EQ"></a>

 
## <a name="request-body"></a>요청 본문
 
요청 본문은 한 gamerpic (1080 x 1080 PNG 파일).
  
<a id="ID4EZ"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.
 
| 코드| 이유 구| 설명| 
| --- | --- | --- | 
| 200| 확인| 성공적으로 받습니다.| 
| 201| 만들어집니다.| 업로드가 완료 되었습니다.| 
| 403| 사용할 수 없음| 권한 해지 됩니다.| 
| 500| 오류| 문제가 발생했습니다.| 
  
<a id="ID4EXC"></a>

 
## <a name="response-body"></a>응답 본문
 
개체가 응답 본문에 전송 됩니다.
  
<a id="ID4ECD"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EED"></a>

 
##### <a name="parent"></a>Parent 

[/users/me/gamerpic](uri-usersmegamerpic.md)

   