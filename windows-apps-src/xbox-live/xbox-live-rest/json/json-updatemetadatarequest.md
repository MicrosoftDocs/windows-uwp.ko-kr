---
title: UpdateMetadataRequest(JSON)
assetID: 0bc210e3-c1dc-9267-e322-aadb9f0a074a
permalink: en-us/docs/xboxlive/rest/json-updatemetadatarequest.html
author: KevinAsgari
description: " UpdateMetadataRequest(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 72b4a7b971647304e08fe52e7696be9fcd39e584
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5435095"
---
# <a name="updatemetadatarequest-json"></a>UpdateMetadataRequest(JSON)
클립을 업데이트 해야 하는 메타 데이터입니다. 
<a id="ID4EN"></a>

 
## <a name="updatemetadatarequest"></a>UpdateMetadataRequest
 
UpdateMetadataRequest 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| userCaption| string| 게임 클립에 대해 사용자가 입력 한 지역화 되지 않은 문자열을 변경합니다.| 
| visibility| [GameClipVisibility 열거형](../enums/gvr-enum-gameclipvisibility.md)| 시스템에 게시 될 때 게임 클립의 표시를 변경 합니다.| 
| titleData| string| 제목 관련 속성 모음입니다. 최대 크기: 10KB입니다.| 
  
<a id="ID4EBC"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 
클립 사용자 이름 및 표시 여부를 변경:
 

```json
{
  "userCaption": "I've changed this 100 Times!",
  "visibility": "Owner"
}

```

 
(이 예는 단지, 호출자에 게이 필드의 스키마 이므로) 방금 제목 속성 변경:
 

```json
{
  "titleData": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }"
}

```

  
<a id="ID4EQC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ESC"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E3C"></a>

 
##### <a name="reference"></a>참조 

[POST (/users/me/scids/{scid}/clips/{gameClipId})](../uri/dvr/uri-usersmescidclipsgameclipidpost.md)

   