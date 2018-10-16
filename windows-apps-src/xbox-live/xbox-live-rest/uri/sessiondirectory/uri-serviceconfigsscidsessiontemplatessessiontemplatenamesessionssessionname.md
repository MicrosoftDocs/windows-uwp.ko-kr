---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}
assetID: 55ce6459-1714-49bc-6231-b547ddf04143
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.html
author: KevinAsgari
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 43054e909ce6e4a3d472a6a6480cd0812afa5ad4
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4684585"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}
GET 및 PUT 작업 만들기 및 검색 세션을 지원 합니다.
<a id="ID4EO"></a>


## <a name="domain"></a>도메인
sessiondirectory.xboxlive.com  
<a id="ID4ET"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| 서비스 안내| GUID| 서비스 구성 id (서비스 안내)입니다. 1 부 세션 식별자입니다.|
| sessionTemplateName| string| 세션 템플릿 현재 인스턴스의 이름입니다. 2 부 세션 식별자입니다.|
| 세션 이름| GUID| 세션의 고유 ID입니다. 3 부 세션 식별자입니다.| 

<a id="ID4EBC"></a>


## <a name="valid-methods"></a>유효한 메서드

[GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameget.md)

&nbsp;&nbsp;세션 개체를 가져옵니다.

[PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameput.md)

&nbsp;&nbsp;업데이트, 만들거나 세션에 참가 합니다.

<a id="ID4EOC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EQC"></a>


##### <a name="parent"></a>부모

[세션 디렉터리 URI](atoc-reference-sessiondirectory.md)
