---
title: UserSettings(JSON)
assetID: 17c030cb-05e0-f78e-5ab1-cdbd8b801ceb
permalink: en-us/docs/xboxlive/rest/json-usersettings.html
description: " UserSettings(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5451c59ab608105677a657ade41154bd2b622f5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655048"
---
# <a name="usersettings-json"></a>UserSettings(JSON)
현재 인증 된 사용자에 대 한 설정을 반환합니다. 
<a id="ID4EN"></a>

 
## <a name="usersettings"></a>UserSettings
 
UserSettings 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| id| 32 비트 부호 없는 정수| 설정의 식별자입니다.| 
| 소스| 32 비트 부호 없는 정수| 설정의 원본을 나타냅니다. | 
| titleId| 32 비트 부호 없는 정수| 설정과 연결 된 제목의 식별자입니다. | 
| value| 8 비트 부호 없는 정수 배열| 설정의 값을 나타냅니다. 클라이언트 설정을 가져오는 데이터를 읽을 수 있으려면 표현 형식을 이해 해야 합니다. | 
  
<a id="ID4EJC"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

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

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

   