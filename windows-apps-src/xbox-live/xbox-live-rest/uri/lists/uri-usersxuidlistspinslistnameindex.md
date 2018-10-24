---
title: /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
assetID: edcb19bd-87a5-732b-0c45-6f7355fc2dd1
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameindex.html
author: KevinAsgari
description: " /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d4b1be4ab591a5bea8d7bc70fb7f7dcb29e4f548
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5435113"
---
# <a name="usersxuidxuidlistspinslistnameindexindexinsertindexinsertindex"></a>/users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
목록 내의 항목으로 이동합니다. 이러한 Uri에 대 한 도메인은 `eplists.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수 
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| XUID| string| 사용자의 XUID 합니다.| 
| listname| string| 조작 목록의 이름입니다.| 
| 인덱스| string| 이동할 수 있는 항목의 현재 인덱스를 지정 합니다. 인덱스 값이 0 또는 양의 정수 인 경우이 항목의 현재 인덱스를 나타냅니다 및 호출의 요청 본문 비어 있어야 합니다. 그러나 인덱스 값은 "-1"을 이동할 항목 ItemId 또는 공급자/ProviderID 호출의 요청 본문에 의해 지정 되어야 합니다. | 
  
<a id="ID4EHC"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[POST](uri-usersxuidlistspinslistnameindexpost.md)

&nbsp;&nbsp;목록에서 항목을 목록 내에서 다른 위치로 이동합니다.
 
<a id="ID4ERC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ETC"></a>

 
##### <a name="parent"></a>부모 

[URI(Universal Resource Identifier) 참조](../atoc-xboxlivews-reference-uris.md)

   