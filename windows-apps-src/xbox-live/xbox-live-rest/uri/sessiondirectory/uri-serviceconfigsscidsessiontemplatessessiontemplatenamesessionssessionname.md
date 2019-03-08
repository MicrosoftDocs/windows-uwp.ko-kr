---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}
assetID: 55ce6459-1714-49bc-6231-b547ddf04143
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.html
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: da8d61634d0a849d51e2ab6ff143469ec05cc75c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657478"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}
PUT 및 GET 작업을 만들고 세션 검색을 지원 합니다.
<a id="ID4EO"></a>


## <a name="domain"></a>도메인
sessiondirectory.xboxlive.com  
<a id="ID4ET"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- |
| scid| GUID| (서비스 안내) 식별자를 구성 하는 서비스입니다. 1 부 세션 식별자입니다.|
| sessionTemplateName| 문자열| 세션 템플릿의 현재 인스턴스의 이름입니다. 2 부를 선택 하면 세션 식별자입니다.|
| sessionName| GUID| 세션의 고유 ID입니다. 3 부 세션 식별자입니다.| 

<a id="ID4EBC"></a>


## <a name="valid-methods"></a>올바른 메서드

[GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameget.md)

&nbsp;&nbsp;세션 개체를 가져옵니다.

[PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameput.md)

&nbsp;&nbsp;업데이트를 만들거나 세션에 참가 합니다.

<a id="ID4EOC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EQC"></a>


##### <a name="parent"></a>Parent

[세션 디렉터리 Uri](atoc-reference-sessiondirectory.md)
