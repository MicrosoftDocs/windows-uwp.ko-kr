---
title: /serviceconfigs/{scid}/hoppers/{name}/stats
assetID: 56bb4398-445b-e8c5-a4ce-1651576ee7e7
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamestats.html
description: " /serviceconfigs/{scid}/hoppers/{name}/stats"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 04376505b76296e052ea431f2a4e5fcfeac7b9e4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613938"
---
# <a name="serviceconfigsscidhoppersnamestats"></a>/serviceconfigs/{scid}/hoppers/{name}/stats

hopper에 대 한 통계를 검색 하는 것에 대 한 가져오기 작업을 지원 합니다.

> [!IMPORTANT]
> 이 URI 103 이상 계약과 함께 사용 하기 위한 하며 헤더 요소를 X Xbl-계약 버전: 103 또는 나중에 모든 요청 합니다.

<a id="ID4ER"></a>


## <a name="domain"></a>도메인
momatch.xboxlive.com  
<a id="ID4EW"></a>


## <a name="remarks"></a>설명
이 URI는 대상 사용자의 구성에서 소유자 식별자에 대 한 값 xuid, gt, 및 me를 지원합니다. 티켓의 작성자만 티켓을 삭제 하거나 URI의 상태를 검색할 수 있습니다.  
<a id="ID4E6"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- | --- |
| scid| GUID| 서비스 구성 (서비스 안내) 세션 식별자입니다.|
| name| 문자열| hopper의 이름입니다.|

<a id="ID4EEC"></a>


## <a name="valid-methods"></a>올바른 메서드

[GET (/serviceconfigs/{scid}/hoppers/{name}/stats)](uri-serviceconfigsscidhoppershoppernamestatsget.md)

&nbsp;&nbsp;hopper에 대 한 통계를 가져옵니다.

<a id="ID4EQC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4ESC"></a>


##### <a name="parent"></a>Parent  

[결혼 정보 회사 연결 Uri](atoc-reference-matchtickets.md)
