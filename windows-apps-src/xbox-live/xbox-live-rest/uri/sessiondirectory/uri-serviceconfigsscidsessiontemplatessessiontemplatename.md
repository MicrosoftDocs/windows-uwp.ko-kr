---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}
assetID: cc306ca0-57f8-f676-6d4b-f9ddd716dcc0
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatename.html
author: KevinAsgari
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d1bf7735fabbc08f723dbaf77a020b205f66584d
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4531016"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatename"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}
세션 템플릿 이름으로 이루어진 집합을 검색할 GET 작업을 지원 합니다. 
<a id="ID4EO"></a>

 
## <a name="domain"></a>도메인
sessiondirectory.xboxlive.com  
<a id="ID4ET"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| 서비스 안내| GUID| 서비스 구성 식별자 (서비스 안내)입니다. 파트 1 세션의 id.| 
| sessionTemplateName| string| 현재 인스턴스 세션 서식 파일의 이름입니다. 파트 2 세션의 id. | 
  
<a id="ID4EYB"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName})](uri-serviceconfigsscidsessiontemplatessessiontemplatenameget.md)

&nbsp;&nbsp;세션 템플릿 이름 집합을 검색합니다.
 
<a id="ID4ECC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EEC"></a>

 
##### <a name="parent"></a>부모 

[세션 디렉터리 URI](atoc-reference-sessiondirectory.md)

   