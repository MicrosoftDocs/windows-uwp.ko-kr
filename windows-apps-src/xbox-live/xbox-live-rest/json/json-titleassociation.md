---
title: TitleAssociation(JSON)
assetID: 05f4edbb-cc3d-c17d-0979-aac9e44a4988
permalink: en-us/docs/xboxlive/rest/json-titleassociation.html
author: KevinAsgari
description: " TitleAssociation(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c9d42f4285cd20785f5606d3e2ac6094a874acba
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4610753"
---
# <a name="titleassociation-json"></a>TitleAssociation(JSON)
도전 과제와 연결 된 제목입니다. 
<a id="ID4EN"></a>

 
## <a name="titleassociation"></a>TitleAssociation
 
TitleAssociation 개체에는 다음과 같이 지정 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| name| 문자열| 콘텐츠 지역화 된 이름입니다.| 
| id| string| TitleId (32 비트 부호 없는 정수, 10 진수에서 반환).| 
| 버전| 문자열| 연결 된 제목 (필요한 경우)의 특정 버전입니다.| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
  "name":"Microsoft Achievements Sample",
  "id":3051199919,
  "version":"abc"
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   