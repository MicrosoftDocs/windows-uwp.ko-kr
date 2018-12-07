---
title: GameClipThumbnail(JSON)
assetID: 3ed87fc1-734c-d8b5-d908-0ae3359769ed
permalink: en-us/docs/xboxlive/rest/json-gameclipthumbnail.html
description: " GameClipThumbnail(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a491b70b8e34c1c736667b50271af7b970b6bb2a
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8792957"
---
# <a name="gameclipthumbnail-json"></a>GameClipThumbnail(JSON)
개별는 미리 보기와 관련 된 정보를 포함 합니다. 클립 당 여러 크기 있을 수 있으며 사용자가 디스플레이 위해 적절 한 선택 클라이언트입니다. 
<a id="ID4EN"></a>

 
## <a name="gameclipthumbnail"></a>GameClipThumbnail
 
GameClipThumbnail 개체에 다음과 같이 지정 합니다.
 
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

   