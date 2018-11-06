---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name}
assetID: aed0764f-4e3d-e0b3-1ea0-543c32f3f822
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersservername.html
author: KevinAsgari
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name}"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d799db37aef00aaaa7a992b2bda5cb535a12b569
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2018
ms.locfileid: "6051592"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameserversserver-name"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name}
세션의 지정 된 서버를 제거 하려면 삭제 작업을 지원 합니다.
<a id="ID4EO"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| 서비스 안내| GUID| 서비스 구성 id (서비스 안내)입니다. 파트 1의 세션 식별자입니다.|
| sessionTemplateName| string| 현재 인스턴스의 세션 서식 파일의 이름입니다. 파트 2 세션 식별자입니다.|
| 세션| GUID| 세션의 고유 ID입니다. 3 부 세션 식별자입니다.| 

<a id="ID4E3B"></a>


## <a name="valid-methods"></a>유효한 메서드

[DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name})](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersservernamedelete.md)

&nbsp;&nbsp;세션에서 지정된 된 서버를 제거합니다.

<a id="ID4EGC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EIC"></a>


##### <a name="parent"></a>부모

[세션 디렉터리 URI](atoc-reference-sessiondirectory.md)
