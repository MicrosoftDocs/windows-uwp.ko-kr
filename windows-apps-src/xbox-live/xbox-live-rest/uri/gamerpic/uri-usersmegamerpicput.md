---
title: PUT (/users/me/gamerpic)
assetID: ddf71c62-197d-a81d-35a7-47c6dc9e1b0c
permalink: en-us/docs/xboxlive/rest/uri-usersmegamerpicput.html
author: KevinAsgari
description: " PUT (/users/me/gamerpic)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8c8c8f4297bb671f8e90c233ccf98dc2cf0730ad
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4355398"
---
# <a name="put-usersmegamerpic"></a>PUT (/users/me/gamerpic)
1080 x 1080 gamerpic에 업로드합니다. 
  * [요청 본문](#ID4EQ)
  * [HTTP 상태 코드](#ID4EZ)
  * [응답 본문](#ID4EXC)
 
<a id="ID4EQ"></a>

 
## <a name="request-body"></a>요청 본문
 
요청 본문 gamerpic (1080 x 1080 PNG 파일)입니다.
  
<a id="ID4EZ"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스와 함께 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | 
| 200| 확인| 성공적으로 다운로드 합니다.| 
| 201| 만들어집니다.| 업로드 성공 했습니다.| 
| 403| 금지| 권한 세션이 해지 됩니다.| 
| 500| 오류| 문제가 발생했습니다.| 
  
<a id="ID4EXC"></a>

 
## <a name="response-body"></a>응답 본문
 
개체가 응답 본문에 전송 됩니다.
  
<a id="ID4ECD"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EED"></a>

 
##### <a name="parent"></a>부모 

[/users/me/gamerpic](uri-usersmegamerpic.md)

   