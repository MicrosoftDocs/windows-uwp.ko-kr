---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch
assetID: 4f8e1ece-2ba8-9ea4-e551-2a69c499d7b9
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigscidsessiontemplatessessiontemplatenamebatch.html
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 93ecaa95488493fa8779a44d76bf7d635d1751ad
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612598"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamebatch"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch
세션 템플릿 수준에서 일괄 처리 쿼리를 만드는 POST 작업을 지원 합니다.

> [!IMPORTANT]
> 이 메서드는 2015 멀티 플레이 게임에서 사용 되 고 이상 멀티 플레이 버전에만 적용 됩니다. 템플릿 계약 104/105 이상을 사용 하 여 사용 하기 위한 하 고 X-Xbl-계약-버전 헤더 요소를 필요 합니다. 104/105 또는 나중에 모든 요청 합니다.

<a id="ID4ER"></a>


## <a name="domain"></a>도메인
sessiondirectory.xboxlive.com  
<a id="ID4EW"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- | --- |
| scid| GUID| (서비스 안내) 식별자를 구성 하는 서비스입니다. 1 부 세션 식별자입니다.|
| sessionTemplateName| 문자열| 세션 템플릿의 현재 인스턴스의 이름입니다. 2 부를 선택 하면 세션 식별자입니다.|

<a id="ID4E2B"></a>


## <a name="valid-methods"></a>올바른 메서드

[POST (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch)](uri-serviceconfigscidsessiontemplatessessiontemplatenamebatchpost.md)

&nbsp;&nbsp;Xbox 사용자 Id가 여러 일괄 처리 쿼리를 만듭니다.

<a id="ID4EFC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EHC"></a>


##### <a name="parent"></a>Parent

[세션 디렉터리 Uri](atoc-reference-sessiondirectory.md)
