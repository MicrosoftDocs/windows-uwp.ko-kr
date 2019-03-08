---
title: InitialUploadRequest(JSON)
assetID: 8b8bce98-cb5f-bbaf-5564-9be2f58d749b
permalink: en-us/docs/xboxlive/rest/json-initialuploadrequest.html
description: " InitialUploadRequest(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2fbb2417f743d97b487ad16abd241fcb5eea62bb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646638"
---
# <a name="initialuploadrequest-json"></a>InitialUploadRequest(JSON)
POST 클립의 본문 요청을 업로드 합니다. 
<a id="ID4EN"></a>

 
## <a name="initialuploadrequest"></a>InitialUploadRequest
 
InitialUploadRequest 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| <b>greatestMomentId</b>| 문자열| 클립에 대 한 이름으로 사용할 텍스트 문자열 ID입니다. 이 관리 되 고 제목의 개발자가 제목에 대 한 구성 파일에서 지역화 합니다.| 
| <b>userCaption</b>| 문자열| 선택 사항. 사용자 입력의 대체 이름을 250 자입니다. 최대 길이까지 게임 클립 합니다.| 
| <b>sessionRef</b>| 문자열| 선택 사항. 녹음/녹화 하는 동안 수행 된 게임 세션 참조입니다.| 
| <b>dateRecorded</b>| DateTime| 녹음/녹화 시작 된 utc에서 시간입니다. ISO 8601 형식에서 문자열로 마샬링 (참조 <a href="https://www.w3.org/TR/NOTE-datetime">날짜 및 시간 형식</a> 자세한).| 
| <b>durationInSeconds</b>| 32 비트 부호 없는 정수| 시간 (초)의 클립 길이입니다.| 
| <b>expectedBlocks</b>| 32 비트 부호 없는 정수| 선택 사항. 분할 하는 파일은 블록 수입니다. 파일을 단일 요청에서 전송 되는 경우를 생략 합니다.| 
| <b>fileSize</b>| 32 비트 부호 없는 정수| 파일 업로드 될 비디오의 바이트 크기입니다.| 
| <b>type</b>| [GameClipType 열거형](../enums/gvr-enum-gamecliptypes.md)| 문자열은 쉼표로 구분 하는 열거형 값으로 마샬링될 클립의 형식입니다.| 
| <b>source</b>| [GameClipSource 열거형](../enums/gvr-enum-gameclipsource.md)| 클립 원본이 지정 된 방법을 지정 열거형의 문자열 값으로 마샬링됩니다.| 
| <b>visibility</b>| [GameClipVisibility 열거형](../enums/gvr-enum-gameclipvisibility.md)| 시스템에 게시 된 후 게임 클립의 표시 여부를 지정 합니다.| 
| <b>titleData</b>| 문자열| 선택 사항. 이 클립 연관 된 title 별 속성에 대 한 속성 모음입니다. 저장 하 고 반환-됩니다. 제목 개발자 클립에 대 한 자체 메타 데이터를 유지 하려면이 필드를 사용할 수 있습니다.| 
| <b>titleData</b>| 문자열| 선택 사항. 이 클립 연관 된 콘솔-특정 속성에 대 한 속성 모음입니다. 저장 하 고 반환-됩니다. 콘솔 플랫폼이이 필드를 사용 하 여 클립에 대 한 자체 메타 데이터를 유지할 수 있습니다.| 
| <b>systemProperties</b>| 문자열| 선택 사항. 이 클립 연관 된 콘솔-특정 속성에 대 한 속성 모음입니다. 저장 하 고 그대로 반환 합니다. 콘솔 플랫폼이이 필드를 사용 하 여 클립에 대 한 자체 메타 데이터를 유지할 수 있습니다.| 
| <b>usersInSession</b>| 문자열의 배열| 선택 사항. 현재 세션에서 사용자의 목록입니다.| 
| <b>thumbnailSource</b>| [ThumbnailSource 열거형](../enums/gvr-enum-thumbnailsource.md)| 선택 사항. 미리 보기의 원본입니다.| 
| <b>thumbnailOffsetMillseconds</b>| 32 비트 부호 있는 정수| 오프셋된 생성 된 썸네일에 대 한 오프셋 (밀리초)을 지정합니다. 지정 된 경우에 <b>thumbnailSource</b> 오프셋으로 설정 됩니다.| 
| <b>savedByUser</b>| 부울 값| 선택 사항. FIFO storage 대신 사용자의 할당량에 저장 해야 하는 클립을 설정 합니다. 기본값은 false입니다.| 
  
<a id="ID4ERH"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

```json
{
   "greatestMomentId": "123abc",
   "userCaption": "OMG Look at this!",
   "sessionRef": "4587552a-a5ad-4c4c-a787-5bc5af70e4c9",
   "dateRecorded": "2012-12-23T11:08:08Z",
   "durationInSeconds": 27,
   "expectedBlocks": 7,
   "fileSize": 1234567,
   "type": "MagicMoment, Achievement",
   "source": "Console",
   "visibility": "Default",
   "titleData": "{ 'Boss': 'The Invincible' }",
   "systemProperties": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }",
   "thumbnailSource": "Offset",
   "thumbnailOffsetMillseconds": 20000,
   "savedByUser": false
 }
    
```

  
<a id="ID4E1H"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E3H"></a>

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

   