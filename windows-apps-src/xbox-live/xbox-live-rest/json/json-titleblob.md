---
title: TitleBlob(JSON)
assetID: fd1c904d-e8d0-f61f-e403-40b25bd4ac14
permalink: en-us/docs/xboxlive/rest/json-titleblob.html
author: KevinAsgari
description: " TitleBlob(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 91423df8367c275f40cd7f856a60070e1a46ad40
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4532408"
---
# <a name="titleblob-json"></a>TitleBlob(JSON)
저장소의 제목에 대 한 정보가 포함 되어 있습니다. 
<a id="ID4EP"></a>

 
## <a name="titleblob"></a>TitleBlob
 
TitleBlob 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| clientFileTime| DateTime| [옵션] 날짜 및 파일의 마지막 업로드 시간입니다.| 
| displayName| string| [옵션] 사용자에 게 표시 되는 파일 이름입니다.| 
| etag| string| 태그에서 사용 되는 파일을 다운로드 하 고 요청을 업로드 합니다.| 
| fileName| string| 파일의 이름입니다.| 
| size| 64 비트의 부호 있는 정수| 바이트에서 파일 크기입니다.| 
| smartBlobType| string| [옵션] 데이터 형식입니다. 가능한 값은: config json 이진 합니다.| 
  
<a id="ID4E6C"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

```json
{
    "fileName":"foo\bar\blob.txt,binary",
    "clientFileTime":"2012-01-01T01:02:03.1234567Z",
    "displayName":"Friendly Name",
    "size":12,
    "etag":"0x8CEB3E4F8F3A5BF",
    "smartBlobType":"binary"
}
      
```

  
<a id="ID4EID"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EKD"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   