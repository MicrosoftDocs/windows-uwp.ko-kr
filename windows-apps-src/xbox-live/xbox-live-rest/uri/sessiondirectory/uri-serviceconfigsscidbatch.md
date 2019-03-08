---
title: /serviceconfigs/{scid}/batch
assetID: eb1b510f-d92e-ae9b-a3e6-0edf58b4f075
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidbatch.html
description: " /serviceconfigs/{scid}/batch"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 208ee92106563372dd4d92a8c800cc08f513e8c7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589718"
---
# <a name="serviceconfigsscidbatch"></a>/serviceconfigs/{scid}/batch
서비스 구성 식별자 수준에서 일괄 처리 쿼리에 대 한 POST 작업을 지원합니다.

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

<a id="ID4ESB"></a>


## <a name="valid-methods"></a>올바른 메서드

[POST (/serviceconfigs/{scid}/batch)](uri-serviceconfigsscidbatchpost.md)

&nbsp;&nbsp;서비스 구성에 대 한 여러 Xbox 사용자 Id에 일괄 처리 쿼리를 만듭니다.

<a id="ID4E3B"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4E5B"></a>


##### <a name="parent"></a>Parent

[세션 디렉터리 Uri](atoc-reference-sessiondirectory.md)
