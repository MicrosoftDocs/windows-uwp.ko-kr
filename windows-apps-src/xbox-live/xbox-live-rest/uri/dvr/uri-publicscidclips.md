---
title: /public/scids/{scid}/clips
assetID: 55a1f0ae-08bb-6d1e-a1da-cbeb481c42ee
permalink: en-us/docs/xboxlive/rest/uri-publicscidclips.html
description: " /public/scids/{scid}/clips"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: db279d546780ed40158894f73ecb84687ef35ba6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627978"
---
# <a name="publicscidsscidclips"></a>/public/scids/{scid}/clips
클립을 공개 하는 액세스 합니다. 이 URI 실제로 지정할 수 있습니다 두 가지 형태로 `/public/scids/{scid}/clips` 고 `/public/titles/{titleId}/clips`입니다. 자세한 내용은 아래를 참조하세요. 이 URI에 대 한 도메인은 `gameclipsmetadata.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4E1)
 
<a id="ID4E1"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| scid| 문자열| 공용 클립의 기본 서비스 구성 식별자입니다.| 
| titleid| 문자열| 공용 클립의 titleId 합니다. 서비스를 안내와 동일한 URI에 지정할 수 없습니다. 를 지정 하는 경우 기본 서비스 안내 조회에 사용 됩니다.| 
  
<a id="ID4E6B"></a>

 
## <a name="valid-methods"></a>올바른 메서드

[가져오기 (공용 / / scids / {서비스 안내} 자르는 /)](uri-publicscidclipsget.md)

&nbsp;&nbsp;공용 클립을 나열 합니다.
 
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>Parent 

[Marketplace Uri](../marketplace/atoc-reference-marketplace.md)

   