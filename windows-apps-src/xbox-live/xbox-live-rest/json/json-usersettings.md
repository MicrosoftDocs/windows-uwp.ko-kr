---
title: UserSettings(JSON)
assetID: 17c030cb-05e0-f78e-5ab1-cdbd8b801ceb
permalink: en-us/docs/xboxlive/rest/json-usersettings.html
author: KevinAsgari
description: " UserSettings(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 67b9edcb4ffd4c0da6929de8dfd47652cf7ab375
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5972954"
---
# <a name="usersettings-json"></a>UserSettings(JSON)
현재 인증 된 사용자에 대 한 설정을 반환합니다. 
<a id="ID4EN"></a>

 
## <a name="usersettings"></a>UserSettings
 
UserSettings 개체에는 다음과 같이 지정 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| id| 32 비트 부호 없는 정수| 설정의 식별자입니다.| 
| 원본| 32 비트 부호 없는 정수| 설정의 소스를 나타냅니다. | 
| titleId| 32 비트 부호 없는 정수| 식별자는 설정과 관련 된 제목입니다. | 
| value| 8 비트 부호 없는 정수의 배열| 설정의 값을 나타냅니다. 클라이언트 설정을 검색 데이터를 읽을 수 있어야 표시 형식을 이해 해야 합니다. | 
  
<a id="ID4EJC"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
   "id":268697600,
   "source":1,
   "titleId":1297287259,
   "value":"CAAAAA=="
}
    
```

  
<a id="ID4ESC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EUC"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   