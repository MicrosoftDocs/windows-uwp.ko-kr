---
title: /serviceconfigs/{scid}/hoppers/{hoppername}
assetID: ba1e129d-b4c4-6535-46ce-fd184465c85f
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppername.html
author: KevinAsgari
description: " /serviceconfigs/{scid}/hoppers/{hoppername}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5e02a4ea5ba9c21337032daad44579f6001360cd
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4340546"
---
# <a name="serviceconfigsscidhoppershoppername"></a>/serviceconfigs/{scid}/hoppers/{hoppername}

일치 티켓 만들기를 게시 작업을 지원 합니다.

> [!IMPORTANT]
> 이 URI에 계약 103 이상을 사용 하 여 사용 하며 Xbl 계약 버전 X의 헤더 요소: 103 또는 나중에 모든 요청.

<a id="ID4ER"></a>


## <a name="domain"></a>도메인
momatch.xboxlive.com  
<a id="ID4EW"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- |
| 서비스 안내| GUID| 서비스 구성 (서비스 안내) 세션 식별자입니다.|
| hoppername | string | hopper의 이름입니다. |

<a id="ID4E2B"></a>


## <a name="valid-methods"></a>유효한 메서드

[POST (/serviceconfigs/{scid}/hoppers/{hoppername})](uri-serviceconfigsscidhoppershoppernamepost.md)

&nbsp;&nbsp;지정 된 일치 티켓을 만듭니다.

<a id="ID4EFC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EHC"></a>


##### <a name="parent"></a>부모  

[매치 메이킹 URI](atoc-reference-matchtickets.md)
