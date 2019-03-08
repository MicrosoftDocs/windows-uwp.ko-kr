---
title: /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
assetID: edcb19bd-87a5-732b-0c45-6f7355fc2dd1
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameindex.html
description: " /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b56b563c72c206340aa2c1ce9f73aa8dfe50809d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641228"
---
# <a name="usersxuidxuidlistspinslistnameindexindexinsertindexinsertindex"></a>/users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
목록 내의 항목을 이동합니다. 이러한 Uri에 대 한 도메인은 `eplists.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수 
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| XUID| 문자열| 사용자의 XUID 합니다.| 
| listname| 문자열| 조작 목록의 이름입니다.| 
| 인덱스| 문자열| 이동할 항목의 현재 인덱스를 지정 합니다. 인덱스 값을 0 또는 양의 정수 이면이 항목의 현재 인덱스를 참조 하며 호출의 요청 본문은 비어 있어야 합니다. 그러나 인덱스 값을 "-1" 인 경우 이동할 항목 ItemId 또는 공급자/ProviderID 호출의 요청 본문에 지정 해야 합니다. | 
  
<a id="ID4EHC"></a>

 
## <a name="valid-methods"></a>올바른 메서드

[POST](uri-usersxuidlistspinslistnameindexpost.md)

&nbsp;&nbsp;목록의 항목을 목록 내에서 다른 위치로 이동합니다.
 
<a id="ID4ERC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ETC"></a>

 
##### <a name="parent"></a>Parent 

[유니버설 리소스 식별자 (URI) 참조](../atoc-xboxlivews-reference-uris.md)

   