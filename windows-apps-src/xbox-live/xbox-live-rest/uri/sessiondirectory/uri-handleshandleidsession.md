---
title: /handles/{handleId}/session
assetID: 4ed2dcf5-5d1f-91ce-4a3f-eb3ba68727bf
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidsession.html
author: KevinAsgari
description: " /handles/{handleId}/session"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 38fa1ad62b2e76dceda79744c59eb59ddc50ea90
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2018
ms.locfileid: "6050972"
---
# <a name="handleshandleidsession"></a>/handles/{handleId}/session
GET 및 PUT 작업 세션에 대 한 핸들을 해제를 사용 하 여 지원 합니다. 

> [!NOTE] 
> 이 URI 2015 멀티 플레이어에서 사용 되 고 및 나중 멀티 플레이 해당 버전에만 적용 됩니다. 용도가 템플릿 계약 104/105 이상와 함께 사용 합니다.  

 

> [!NOTE] 
> 이 URI는 현재 Xbox One 콘솔 및 서비스 식별자를 사용 하 여 서버에서 외부에서 액세스할 수만 있습니다.  

 
<a id="ID4ES"></a>

 
## <a name="domain"></a>도메인
sessiondirectory.xboxlive.com  
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | --- | 
| handleId| GUID| 세션에 대 한 핸들의 고유 ID입니다.| 
  
<a id="ID4ESB"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[GET (/handles/{handleId}/session)](uri-handleshandleidsessionget.md)

&nbsp;&nbsp;지정 된 핸들 식별자에 대 한 세션 개체를 가져옵니다. 

[PUT (/handles/{handle-id}/session)](uri-handleshandleidsessionput.md)

&nbsp;&nbsp;만들거나 핸들을 해제 하 여 세션을 업데이트 합니다.
 
<a id="ID4E6B"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EBC"></a>

 
##### <a name="parent"></a>부모 

[세션 디렉터리 URI](atoc-reference-sessiondirectory.md)

   