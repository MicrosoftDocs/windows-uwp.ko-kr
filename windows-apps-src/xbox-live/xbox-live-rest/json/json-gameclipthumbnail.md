---
title: GameClipThumbnail(JSON)
assetID: 3ed87fc1-734c-d8b5-d908-0ae3359769ed
permalink: en-us/docs/xboxlive/rest/json-gameclipthumbnail.html
description: " GameClipThumbnail(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ad2d35431cb4c40690978f4f3920f2e47f2b9bc0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606568"
---
# <a name="gameclipthumbnail-json"></a>GameClipThumbnail(JSON)
개별 미리 보기에 관한 정보를 포함 합니다. 클립, 당 여러 크기 있을 수 있으며 클라이언트 표시에 대 한 적절 한 선택에 달려 있습니다. 
<a id="ID4EN"></a>

 
## <a name="gameclipthumbnail"></a>GameClipThumbnail
 
GameClipThumbnail 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| <b>uri</b>| 문자열| 미리 보기 이미지에 대 한 URI입니다.| 
| <b>fileSize</b>| 32 비트 부호 없는 정수| 축소판 그림 이미지의 총 파일 크기입니다.| 
| <b>thumbnailType</b>| ThumbnailType| 축소판 그림 이미지의 형식입니다.| 
  
<a id="ID4EAC"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

```json
{
         "uri": "https://gameclips.xbox.com/thumbnails/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/small.jpg",
         "fileSize": 123,
         "width": 120,
         "height": 250
       }
    
```

  
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

   