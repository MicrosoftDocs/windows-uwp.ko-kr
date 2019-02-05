---
title: GET (/inventory/{itemID})
assetID: d3ca14a5-0214-ef42-091e-3f05f2a3482d
permalink: en-us/docs/xboxlive/rest/uri-inventoryitemurlget.html
description: " GET (/inventory/{itemID})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 446197eb20820304088ddac4a6379fa3b2510873
ms.sourcegitcommit: b975c8fc8cf0770dd73d8749733ae5636f2ee296
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/05/2019
ms.locfileid: "9058484"
---
# <a name="get-inventoryitemid"></a>GET (/inventory/{itemID})
특정 인벤토리 항목에 대 한 세부 정보의 전체 집합을 제공합니다. 이러한 Uri에 대 한 도메인은 `inventory.xboxlive.com`.
 
  * [설명](#ID4EX)
  * [URI 매개 변수](#ID4EAB)
  * [응답 본문](#ID4ELB)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>설명
 
정책이 없는 검사를 적용 하는 또는 필터링이 호출의 일환으로이 발생 합니다.
  
<a id="ID4EAB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| itemID| string| 각 사용자에 게 단일 인벤토리 항목에 대 한 고유 ID| 
  
<a id="ID4ELB"></a>

 
## <a name="response-body"></a>응답 본문
 
<a id="ID4ERB"></a>

 
### <a name="sample-response"></a>예제 응답
 
인증을 통과 하 고 적절 한 권한을 컨텍스트를 할당 된 것으로 가정 GET 요청에 대 한 응답 항목 속성의 전체 집합을 사용 하 여 단일 인벤토리 항목입니다.
 

```cpp
{inventoryItem}
         
```

   
<a id="ID4E4B"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E6B"></a>

 
##### <a name="parent"></a>부모 

[GET (/inventory/{itemID})](uri-inventoryget.md)

  
<a id="ID4EJC"></a>

 
##### <a name="further-information"></a>자세한 정보 

[EDS 공통 헤더](../../additional/edscommonheaders.md)

 [EDS 매개 변수](../../additional/edsparameters.md)

 [EDS 쿼리 구체화](../../additional/edsqueryrefiners.md)

 [마켓플레이스 URI](atoc-reference-marketplace.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)

   