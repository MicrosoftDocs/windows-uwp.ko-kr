---
title: /handles/{handleId}
assetID: 5b722d3e-fe80-fec5-a26b-8b3db6422004
permalink: en-us/docs/xboxlive/rest/uri-handleshandleid.html
author: KevinAsgari
description: " /handles/{handleId}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 47dda291a9a86ccbee69e1e51ca71be373f5dc1d
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4417513"
---
# <a name="handleshandleid"></a>/handles/{handleId}
세션 핸들 식별자에서 지정 하 고 삭제 작업을 지원 합니다. 

> [!NOTE] 
> 이 URI 2015 멀티 플레이어에서 사용 되 고 및 나중 멀티 플레이 해당 버전에만 적용 됩니다. 용도가 템플릿 계약 104/105 이상와 함께 사용 합니다.  

 
<a id="ID4EQ"></a>

 
## <a name="domain"></a>도메인
sessiondirectory.xboxlive.com  
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | 
| handleId| GUID| 세션에 대 한 핸들의 고유 ID입니다.| 
  
<a id="ID4ERB"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[DELETE (/handles/{handleId})](uri-handleshandleiddelete.md)

&nbsp;&nbsp;핸들 id 지정 핸들을 삭제 합니다.

[GET (/handles/{handle-id})](uri-handleshandleidget.md)

&nbsp;&nbsp;핸들 id 지정 핸들을 검색 합니다.
 
<a id="ID4E4B"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E6B"></a>

 
##### <a name="parent"></a>부모 

[세션 디렉터리 URI](atoc-reference-sessiondirectory.md)

   