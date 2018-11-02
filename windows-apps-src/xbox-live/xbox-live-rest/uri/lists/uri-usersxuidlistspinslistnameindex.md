---
title: /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
assetID: edcb19bd-87a5-732b-0c45-6f7355fc2dd1
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameindex.html
author: KevinAsgari
description: " /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3d95d3f0f171fa0e529d57ab5deca8160ddc3c43
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5929267"
---
# <a name="usersxuidxuidlistspinslistnameindexindexinsertindexinsertindex"></a>/users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
목록 내의 항목으로 이동합니다. 이러한 Uri에 대 한 도메인은 `eplists.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수 
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| XUID| string| 사용자의 XUID 합니다.| 
| listname| string| 이름 조작 목록입니다.| 
| 인덱스| string| 이동할 수 있는 항목의 현재 인덱스를 지정 합니다. 인덱스 값이 0 또는 양의 정수 인 경우이 항목의 현재 인덱스를 참조 하 고 호출의 요청 본문의 빈 여야 합니다. 그러나 인덱스 값이 "-1" 인 경우 항목을 이동할 수 ItemId 또는 공급자/ProviderID 호출의 요청 본문에 의해 지정 되어야 합니다. | 
  
<a id="ID4EHC"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[POST](uri-usersxuidlistspinslistnameindexpost.md)

&nbsp;&nbsp;목록의 항목을 목록 내에서 다른 위치로 이동합니다.
 
<a id="ID4ERC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ETC"></a>

 
##### <a name="parent"></a>부모 

[URI(Universal Resource Identifier) 참조](../atoc-xboxlivews-reference-uris.md)

   