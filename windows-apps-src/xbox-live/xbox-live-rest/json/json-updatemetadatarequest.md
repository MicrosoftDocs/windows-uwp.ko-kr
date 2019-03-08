---
title: UpdateMetadataRequest(JSON)
assetID: 0bc210e3-c1dc-9267-e322-aadb9f0a074a
permalink: en-us/docs/xboxlive/rest/json-updatemetadatarequest.html
description: " UpdateMetadataRequest(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a76e4b12e0ffadb112913775b500ac0d39d413d5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597768"
---
# <a name="updatemetadatarequest-json"></a>UpdateMetadataRequest(JSON)
클립에 대 한 업데이트 되어야 하는 메타 데이터입니다. 
<a id="ID4EN"></a>

 
## <a name="updatemetadatarequest"></a>UpdateMetadataRequest
 
UpdateMetadataRequest 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| userCaption| 문자열| 게임 클립에 대 한 사용자가 입력 한 지역화 되지 않은 문자열을 변경합니다.| 
| visibility| [GameClipVisibility 열거형](../enums/gvr-enum-gameclipvisibility.md)| 시스템에 게시 될 때 게임 클립의 표시 여부를 변경 합니다.| 
| titleData| 문자열| Title 별 속성 모음입니다. 최대 크기: 10KB입니다.| 
  
<a id="ID4EBC"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 
사용자 클립 이름 및 표시 유형을 변경 합니다.
 

```json
{
  "userCaption": "I've changed this 100 Times!",
  "visibility": "Owner"
}

```

 
제목 속성 (이 예 일뿐입니다, 호출자가이 필드의 스키마 이므로)를 변경 합니다.
 

```json
{
  "titleData": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }"
}

```

  
<a id="ID4EQC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ESC"></a>

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E3C"></a>

 
##### <a name="reference"></a>참고자료 

[POST (/ 사용자/me scids / {서비스 안내} /clips/ {gameClipId})](../uri/dvr/uri-usersmescidclipsgameclipidpost.md)

   