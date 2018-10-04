---
title: /serviceconfigs/{scid}/batch
assetID: eb1b510f-d92e-ae9b-a3e6-0edf58b4f075
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidbatch.html
author: KevinAsgari
description: " /serviceconfigs/{scid}/batch"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a48bf9c30370238adc83376a524e9c61abb4af13
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4352665"
---
# <a name="serviceconfigsscidbatch"></a>/serviceconfigs/{scid}/batch
서비스 구성 식별자 수준에서 일괄 처리 쿼리에 대 한 POST 작업을 지원합니다.

> [!IMPORTANT]
> 이 메서드는 2015 멀티 플레이어에서 사용 하 고 나중 및 멀티 플레이 해당 버전에만 적용 됩니다. 템플릿 계약 104/105 이상을 사용 하 여 사용 하기 위한 하 고 X Xbl-계약 버전의 헤더 요소가: 104/105 또는 나중에 모든 요청.

<a id="ID4ER"></a>


## <a name="domain"></a>도메인
sessiondirectory.xboxlive.com  
<a id="ID4EW"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- |
| 서비스 안내| GUID| 서비스 구성 id (서비스 안내)입니다. 파트 1 세션 식별자입니다.|

<a id="ID4ESB"></a>


## <a name="valid-methods"></a>유효한 메서드

[POST (/serviceconfigs/{scid}/batch)](uri-serviceconfigsscidbatchpost.md)

&nbsp;&nbsp;서비스 구성에 대 한 여러 Xbox 사용자 Id에서 일괄 처리 쿼리를 만듭니다.

<a id="ID4E3B"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4E5B"></a>


##### <a name="parent"></a>부모

[세션 디렉터리 URI](atoc-reference-sessiondirectory.md)
