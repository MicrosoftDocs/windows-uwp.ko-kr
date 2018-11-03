---
title: /public/scids/{scid}/clips
assetID: 55a1f0ae-08bb-6d1e-a1da-cbeb481c42ee
permalink: en-us/docs/xboxlive/rest/uri-publicscidclips.html
author: KevinAsgari
description: " /public/scids/{scid}/clips"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 131a1eb67d4a33c3fbd2f5f818499ffeea851f3d
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5993723"
---
# <a name="publicscidsscidclips"></a>/public/scids/{scid}/clips
공개 클립 액세스 합니다. 이 URI 실제로 지정할 수 있으며 두 가지 형식 `/public/scids/{scid}/clips` 및 `/public/titles/{titleId}/clips`. 자세한 내용은 아래를 참조하세요. 이 URI에 대 한 도메인은 `gameclipsmetadata.xboxlive.com`.
 
  * [URI 매개 변수](#ID4E1)
 
<a id="ID4E1"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| 서비스 안내| string| 기본 서비스 구성 공개 클립의 식별자입니다.| 
| titleid| string| 공개 클립의 titleId 합니다. 동일한 uri는 서비스 안내도 지정할 수 없습니다. 기본 서비스 안내를 조회할 수를 지정 하는 경우 사용 됩니다.| 
  
<a id="ID4E6B"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[GET (/public/scids/{scid}/clips)](uri-publicscidclipsget.md)

&nbsp;&nbsp;공개 클립 나열 되어 있습니다.
 
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>부모 

[마켓플레이스 URI](../marketplace/atoc-reference-marketplace.md)

   