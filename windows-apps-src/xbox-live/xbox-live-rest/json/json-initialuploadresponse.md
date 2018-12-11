---
title: InitialUploadResponse(JSON)
assetID: 6abb7d37-2c35-2cc3-d9e5-eff695235262
permalink: en-us/docs/xboxlive/rest/json-initialuploadresponse.html
description: " InitialUploadResponse(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dab59fefb389cf550a1bc4fc6429f6b0970f50ab
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8888096"
---
# <a name="initialuploadresponse-json"></a>InitialUploadResponse(JSON)
 
<a id="ID4EO"></a>

 
## <a name="initialuploadresponse"></a>InitialUploadResponse
 
InitialUploadResponse 개체에 다음과 같이 지정 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| <b>gameClipId</b>| string| 업로드 데이터 요청에 대 한 할당 ID입니다.| 
| <b>uploadUri</b>| URI| 위치는 게임 클립을 업로드 해야 합니다.| 
| <b>largeThumbnailUri</b>| URI| 선택 사항입니다. 위치 큰 미리 보기 업로드할 수 있습니다. 이 필드의 현재 상태 (업로드가 지정 되었을 때 존재 하 게 됩니다) <b>InitialUploadRequest</b> [ThumbnailSource 열거형](../enums/gvr-enum-thumbnailsource.md) 값으로 결정 됩니다.| 
| <b>smallThumbnailUri</b>| URI| 선택 사항입니다. 위치 작은 미리 보기 업로드할 수 있습니다. 이 필드의 현재 상태 (업로드가 지정 되었을 때 존재 하 게 됩니다) <b>InitialUploadRequest</b> [ThumbnailSource 열거형](../enums/gvr-enum-thumbnailsource.md) 값으로 결정 됩니다.| 
  
<a id="ID4EYC"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

```json
{
   "gameClipId": "6b364924-5650-480f-86a7-fc002a1ee752"  ,  
   "uploadUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container",
   "largeThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/large",
   "smallThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/small"
 }
    
```

  
<a id="ID4EBD"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EDD"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   