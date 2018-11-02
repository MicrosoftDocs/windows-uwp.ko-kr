---
title: InitialUploadResponse(JSON)
assetID: 6abb7d37-2c35-2cc3-d9e5-eff695235262
permalink: en-us/docs/xboxlive/rest/json-initialuploadresponse.html
author: KevinAsgari
description: " InitialUploadResponse(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 24dc0b140991b7aab27472e291646da6f564a789
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5933193"
---
# <a name="initialuploadresponse-json"></a>InitialUploadResponse(JSON)
 
<a id="ID4EO"></a>

 
## <a name="initialuploadresponse"></a>InitialUploadResponse
 
InitialUploadResponse 개체에는 다음과 같이 지정 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| <b>gameClipId</b>| string| 업로드 데이터 요청에 대 한 할당 ID입니다.| 
| <b>uploadUri</b>| URI| 게임 클립을 업로드 해야 하는 위치입니다.| 
| <b>largeThumbnailUri</b>| URI| 선택 사항입니다. 위치는 큰 미리 보기 업로드 해야 합니다. 이 필드의 존재 여부 (업로드가 지정 되었을 때 존재 하 게 됩니다) <b>InitialUploadRequest</b> [ThumbnailSource 열거형](../enums/gvr-enum-thumbnailsource.md) 값으로 결정 됩니다.| 
| <b>smallThumbnailUri</b>| URI| 선택 사항입니다. 위치는 작은 미리 보기 업로드 해야 합니다. 이 필드의 존재 여부 (업로드가 지정 되었을 때 존재 하 게 됩니다) <b>InitialUploadRequest</b> [ThumbnailSource 열거형](../enums/gvr-enum-thumbnailsource.md) 값으로 결정 됩니다.| 
  
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

   