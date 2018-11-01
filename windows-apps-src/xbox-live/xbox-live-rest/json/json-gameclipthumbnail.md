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
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5880670"
---
# <a name="gameclipthumbnail-json"></a>GameClipThumbnail(JSON)
개별는 미리 보기에 관한 정보를 포함 합니다. 클립 당 여러 크기 수 있으며 디스플레이 대 한 적절 한 하나를 선택 하는 클라이언트에 게 됩니다. 
<a id="ID4EN"></a>

 
## <a name="gameclipthumbnail"></a>GameClipThumbnail
 
GameClipThumbnail 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| <b>uri</b>| string| 미리 보기 이미지에 대 한 URI입니다.| 
| <b>파일 크기</b>| 32 비트 부호 없는 정수| 미리 보기 이미지의 총 파일 크기입니다.| 
| <b>thumbnailType</b>| ThumbnailType| 미리 보기 이미지의 형식입니다.| 
  
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

   