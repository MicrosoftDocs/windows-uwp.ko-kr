---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch
assetID: 4f8e1ece-2ba8-9ea4-e551-2a69c499d7b9
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigscidsessiontemplatessessiontemplatenamebatch.html
author: KevinAsgari
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c1f1add0104ad166620dbdff79936e6a99379df3
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5986238"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamebatch"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch
세션 템플릿 수준에서 일괄 처리 쿼리를 만들고 게시 작업을 지원 합니다.

> [!IMPORTANT]
> 이 메서드는 2015 멀티 플레이어에서 사용 되 고 및 나중 멀티 플레이 해당 버전에만 적용 됩니다. 템플릿 계약 104/105 이상을 사용 하 여 사용 하기 위한 하 고 X Xbl-계약 버전의 헤더 요소가: 104/105 또는 나중에 모든 요청.

<a id="ID4ER"></a>


## <a name="domain"></a>도메인
sessiondirectory.xboxlive.com  
<a id="ID4EW"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- |
| 서비스 안내| GUID| 서비스 구성 id (서비스 안내)입니다. 파트 1 세션 식별자입니다.|
| sessionTemplateName| string| 현재 세션 템플릿 인스턴스의의 이름입니다. 2 부 세션 식별자입니다.|

<a id="ID4E2B"></a>


## <a name="valid-methods"></a>유효한 메서드

[POST (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch)](uri-serviceconfigscidsessiontemplatessessiontemplatenamebatchpost.md)

&nbsp;&nbsp;여러 Xbox 사용자 Id에서 일괄 처리 쿼리를 만듭니다.

<a id="ID4EFC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EHC"></a>


##### <a name="parent"></a>부모

[세션 디렉터리 URI](atoc-reference-sessiondirectory.md)
