---
title: /handles/{handleId}/session
assetID: 4ed2dcf5-5d1f-91ce-4a3f-eb3ba68727bf
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidsession.html
description: " /handles/{handleId}/session"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e7b6990917437c22dd4d9282492e2a0eab37893b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628148"
---
# <a name="handleshandleidsession"></a>/handles/{handleId}/session
PUT 및 GET 작업 세션에 대 한 핸들 역참조를 사용 하 여 지원 합니다. 

> [!NOTE] 
> 이 URI 2015 멀티 플레이 게임에서 사용 되 고 이상 멀티 플레이 버전에만 적용 됩니다. 템플릿 계약 104/105 이상을 사용에 대 한 것입니다.  

 

> [!NOTE] 
> 이 URI는 현재 Xbox One 콘솔과 서버 서비스 식별자를 사용 하 여 외부에서 액세스할 수만 있습니다.  

 
<a id="ID4ES"></a>

 
## <a name="domain"></a>도메인
sessiondirectory.xboxlive.com  
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | --- | --- | 
| handleId| GUID| 세션에 대 한 핸들의 고유 ID입니다.| 
  
<a id="ID4ESB"></a>

 
## <a name="valid-methods"></a>올바른 메서드

[가져오기 (/handles/ {handleId} / 세션)](uri-handleshandleidsessionget.md)

&nbsp;&nbsp;지정 된 핸들 식별자에 대 한 세션 개체를 가져옵니다. 

[배치 (/handles/ {핸들-id} / 세션)](uri-handleshandleidsessionput.md)

&nbsp;&nbsp;만들거나 세션 핸들을 역참조 하 여 업데이트 합니다.
 
<a id="ID4E6B"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EBC"></a>

 
##### <a name="parent"></a>Parent 

[세션 디렉터리 Uri](atoc-reference-sessiondirectory.md)

   