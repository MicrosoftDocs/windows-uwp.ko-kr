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
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589838"
---
# <a name="initialuploadresponse-json"></a>InitialUploadResponse(JSON)
 
<a id="ID4EO"></a>

 
## <a name="initialuploadresponse"></a>InitialUploadResponse
 
InitialUploadResponse 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| <b>gameClipId</b>| 문자열| 업로드 데이터 요청에 대 한 할당 ID입니다.| 
| <b>uploadUri</b>| URI| 게임 클립이 업로드 될 위치입니다.| 
| <b>largeThumbnailUri</b>| URI| 선택 사항. 위치는 큰 썸네일 업로드 해야 합니다. 이 필드의 존재 여부에 따라 결정 됩니다 합니다 [ThumbnailSource 열거형](../enums/gvr-enum-thumbnailsource.md) 값을 <b>InitialUploadRequest</b> (에 업로드 된 경우 표시 됩니다).| 
| <b>smallThumbnailUri</b>| URI| 선택 사항. 위치는 작은 축소판 그림을 업로드 합니다. 이 필드의 존재 여부에 따라 결정 됩니다 합니다 [ThumbnailSource 열거형](../enums/gvr-enum-thumbnailsource.md) 값을 <b>InitialUploadRequest</b> (에 업로드 된 경우 표시 됩니다).| 
  
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

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

   