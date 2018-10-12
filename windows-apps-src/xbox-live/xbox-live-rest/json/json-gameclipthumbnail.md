---
title: GameClipThumbnail(JSON)
assetID: 3ed87fc1-734c-d8b5-d908-0ae3359769ed
permalink: en-us/docs/xboxlive/rest/json-gameclipthumbnail.html
author: KevinAsgari
description: " GameClipThumbnail(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 681a269cd861f741e2bbde3554acc1b25104d90d
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4536900"
---
# <a name="gameclipthumbnail-json"></a>GameClipThumbnail(JSON)
개별 미리 보기와 관련 된 정보를 포함 합니다. 클립 당 여러 크기 수 있으며 디스플레이 대 한 적절 한 하나를 선택 하는 클라이언트에 게 됩니다. 
<a id="ID4EN"></a>

 
## <a name="gameclipthumbnail"></a>GameClipThumbnail
 
GameClipThumbnail 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| <b>uri</b>| string| 미리 보기 이미지에 대 한 URI입니다.| 
| <b>파일 크기</b>| 32 비트 부호 없는 정수| 미리 보기 이미지의 총 파일 크기입니다.| 
| <b>thumbnailType</b>| ThumbnailType| 미리 보기 이미지의 형식입니다.| 
  
<a id="ID4EAC"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

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

   