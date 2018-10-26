---
title: InitialUploadResponse(JSON)
assetID: 6abb7d37-2c35-2cc3-d9e5-eff695235262
permalink: en-us/docs/xboxlive/rest/json-initialuploadresponse.html
author: KevinAsgari
description: " InitialUploadResponse(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8f4e82f3c38f880e230df4381a10a11f51a65ee1
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5555456"
---
# <a name="initialuploadresponse-json"></a>InitialUploadResponse(JSON)
 
<a id="ID4EO"></a>

 
## <a name="initialuploadresponse"></a>InitialUploadResponse
 
InitialUploadResponse 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| <b>gameClipId</b>| string| 할당 업로드 데이터 요청에 대 한 ID입니다.| 
| <b>uploadUri</b>| URI| 위치 게임 클립 업로드할 수 있습니다.| 
| <b>largeThumbnailUri</b>| URI| 선택 사항입니다. 위치 큰 미리 보기 업로드할 수 있습니다. 이 필드의 존재 여부 <b>InitialUploadRequest</b> (업로드가 지정 되었을 때 존재 하 게 됩니다)에서 [ThumbnailSource 열거형](../enums/gvr-enum-thumbnailsource.md) 값에 의해 결정 됩니다.| 
| <b>smallThumbnailUri</b>| URI| 선택 사항입니다. 위치 작은 미리 보기 업로드할 수 있습니다. 이 필드의 존재 여부 <b>InitialUploadRequest</b> (업로드가 지정 되었을 때 존재 하 게 됩니다)에서 [ThumbnailSource 열거형](../enums/gvr-enum-thumbnailsource.md) 값에 의해 결정 됩니다.| 
  
<a id="ID4EYC"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

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

   