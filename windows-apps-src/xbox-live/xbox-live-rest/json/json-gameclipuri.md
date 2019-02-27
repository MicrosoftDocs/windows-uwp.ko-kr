---
title: GameClipUri(JSON)
assetID: 03c097e8-7f29-1026-7a77-5c785b8511e9
permalink: en-us/docs/xboxlive/rest/json-gameclipuri.html
description: " GameClipUri(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1c74d0831c3b841ad2a1366bd2e03fb8a9b0448d
ms.sourcegitcommit: 079801609165bc7eb69670d771a05bffe236d483
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2019
ms.locfileid: "9115366"
---
# <a name="gameclipuri-json"></a>GameClipUri(JSON)
 
<a id="ID4EO"></a>

 
## <a name="gameclipuri"></a>GameClipUri
 
GameClipUri 개체에는 다음과 같이 지정 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| <b>uri</b>| string| 비디오 자산 위치의 URI입니다.| 
| <b>파일 크기</b>| 32 비트 부호 없는 정수| 미리 보기 이미지의 총 파일 크기입니다.| 
| <b>uriType</b>| GameClipUriType| URI의 형식입니다.| 
| <b>만료</b>| DateTime| 이 응답에 포함 된 URI의 만료 시간입니다. URL이 비어 있거나 재생 하기 전에 만료 된 것으로 간주, 호출자에 게 API를 호출 해야 RefreshUrl 합니다.| 
  
<a id="ID4EMC"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
         "uri": "https://gameclips.xbox.com/clips/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/clip.mp4",
         "fileSize": 1234565,
         "uriType": "Download",
         "expiration": "9999-12-31T23:59:59.9999999"
       }
    
```

  
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   