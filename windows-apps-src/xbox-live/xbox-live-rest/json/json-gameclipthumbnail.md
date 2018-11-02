---
title: GameClipThumbnail(JSON)
assetID: 3ed87fc1-734c-d8b5-d908-0ae3359769ed
permalink: en-us/docs/xboxlive/rest/json-gameclipthumbnail.html
author: KevinAsgari
description: " GameClipThumbnail(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4fae93f76d9c8647b2d4264463b434d86897e2a5
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5933613"
---
# <a name="gameclipthumbnail-json"></a>GameClipThumbnail(JSON)
개별는 미리 보기와 관련 된 정보를 포함 합니다. 클립 당 여러 크기 있을 수 있으며 적절 한 디스플레이 대 한 선택 클라이언트 그것은 개발자가 있습니다. 
<a id="ID4EN"></a>

 
## <a name="gameclipthumbnail"></a>GameClipThumbnail
 
GameClipThumbnail 개체에는 다음과 같이 지정 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| <b>uri</b>| string| 미리 보기 이미지에 대 한 URI입니다.| 
| <b>fileSize</b>| 32 비트 부호 없는 정수| 미리 보기 이미지의 총 파일 크기입니다.| 
| <b>thumbnailType</b>| ThumbnailType| 미리 보기 이미지의 유형입니다.| 
  
<a id="ID4EAC"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
         "uri": "http://gameclips.xbox.com/thumbnails/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/small.jpg",
         "fileSize": 123,
         "width": 120,
         "height": 250
       }
    
```

  
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   