---
title: /handles/{handleId}
assetID: 5b722d3e-fe80-fec5-a26b-8b3db6422004
permalink: en-us/docs/xboxlive/rest/uri-handleshandleid.html
description: " /handles/{handleId}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3a312d3744e96755a899d73307a47c01e3dc79fd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632638"
---
# <a name="handleshandleid"></a>/handles/{handleId}
식별자로 지정 하는 세션 핸들에 대 한 DELETE 및 GET 작업을 지원 합니다. 

> [!NOTE] 
> 이 URI 2015 멀티 플레이 게임에서 사용 되 고 이상 멀티 플레이 버전에만 적용 됩니다. 템플릿 계약 104/105 이상을 사용에 대 한 것입니다.  

 
<a id="ID4EQ"></a>

 
## <a name="domain"></a>도메인
sessiondirectory.xboxlive.com  
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | --- | 
| handleId| GUID| 세션에 대 한 핸들의 고유 ID입니다.| 
  
<a id="ID4ERB"></a>

 
## <a name="valid-methods"></a>올바른 메서드

[DELETE (/handles/{handleId})](uri-handleshandleiddelete.md)

&nbsp;&nbsp;핸들 ID로 지정 된 핸들을 삭제 합니다.

[GET (/handles/ {핸들-id})](uri-handleshandleidget.md)

&nbsp;&nbsp;핸들 ID로 지정한 검색 처리
 
<a id="ID4E4B"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E6B"></a>

 
##### <a name="parent"></a>Parent 

[세션 디렉터리 Uri](atoc-reference-sessiondirectory.md)

   