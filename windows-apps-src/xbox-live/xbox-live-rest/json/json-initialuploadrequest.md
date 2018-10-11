---
title: InitialUploadRequest(JSON)
assetID: 8b8bce98-cb5f-bbaf-5564-9be2f58d749b
permalink: en-us/docs/xboxlive/rest/json-initialuploadrequest.html
author: KevinAsgari
description: " InitialUploadRequest(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: eac2405b8668179fa60921ca45012a417e61b352
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4528673"
---
# <a name="initialuploadrequest-json"></a>InitialUploadRequest(JSON)
POST GameClip 본문 요청을 업로드 합니다. 
<a id="ID4EN"></a>

 
## <a name="initialuploadrequest"></a>InitialUploadRequest
 
InitialUploadRequest 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| <b>greatestMomentId</b>| string| 클립에 대 한 이름으로 사용 하 여 텍스트 문자열 ID입니다. 이 관리 및 제목 개발자가 제목에 대 한 구성 파일에 지역화 합니다.| 
| <b>userCaption</b>| string| 선택 사항입니다. 사용자가 입력 한의 대체 이름이 250 문자의 최대 길이 최대 게임 클립 합니다.| 
| <b>sessionRef</b>| string| 선택 사항입니다. 녹음/녹화 작업 수행 하는 동안 게임 세션 참조입니다.| 
| <b>dateRecorded</b>| DateTime| UTC에서 녹음/녹화 시작 된 시간입니다. ISO 8601에서 문자열 배열 형식 (자세한 내용은 <a href="http://www.w3.org/TR/NOTE-datetime">날짜 및 시간 형식</a> 참조).| 
| <b>durationInSeconds</b>| 32 비트 부호 없는 정수| 몇 초만에 클립의 길이입니다.| 
| <b>expectedBlocks</b>| 32 비트 부호 없는 정수| 선택 사항입니다. 파일은 나눈 블록 수입니다. 단일 요청에 파일 전송 될 경우 생략 됩니다.| 
| <b>파일 크기</b>| 32 비트 부호 없는 정수| 파일 업로드 되는 비디오의 바이트 크기입니다.| 
| <b>type</b>| [GameClipType 열거형](../enums/gvr-enum-gamecliptypes.md)| 쉼표로 구분 되는 열거형의 문자열 값으로 마샬링해야 클립의 형식입니다.| 
| <b>소스</b>| [GameClipSource 열거형](../enums/gvr-enum-gameclipsource.md)| 클립 였던 방법을 지정 열거형의 문자열 값으로 마샬링해야 합니다.| 
| <b>visibility</b>| [GameClipVisibility 열거형](../enums/gvr-enum-gameclipvisibility.md)| 시스템에 게시 되 면 게임 클립의 표시 여부를 지정 합니다.| 
| <b>titleData</b>| string| 선택 사항입니다. 이 클립와 관련 된 제목 특정 속성에 대 한 속성 모음입니다. 저장 하 고 반환-됩니다. 제목 개발자는이 필드를 사용 하 여 클립에 대 한 자체 메타 데이터를 유지할 수 있습니다.| 
| <b>titleData</b>| string| 선택 사항입니다. 이 클립와 관련 된 콘솔 특정 속성에 대 한 속성 모음입니다. 저장 하 고 반환-됩니다. 콘솔 플랫폼 클립에 대 한 자체 메타 데이터를 유지 하기 위해이 필드를 사용할 수 있습니다.| 
| <b>systemProperties</b>| string| 선택 사항입니다. 이 클립와 관련 된 콘솔 특정 속성에 대 한 속성 모음입니다. 저장 하 고 있는 그대로 반환 합니다. 콘솔 플랫폼 클립에 대 한 자체 메타 데이터를 유지 하기 위해이 필드를 사용할 수 있습니다.| 
| <b>usersInSession</b>| 문자열의 배열| 선택 사항입니다. 현재 세션에서 사용자의 목록입니다.| 
| <b>thumbnailSource</b>| [ThumbnailSource 열거형](../enums/gvr-enum-thumbnailsource.md)| 선택 사항입니다. 미리 보기의 소스입니다.| 
| <b>thumbnailOffsetMillseconds</b>| 32 비트 부호 있는 정수| 오프셋된 생성 된 미리 보기에 대 한 오프셋 (밀리초)을 지정합니다. Offset <b>thumbnailSource</b> 설정 된 경우에 지정 합니다.| 
| <b>savedByUser</b>| 부울 값| 선택 사항입니다. 사용자의 할당량 대신 FIFO 저장소에 저장 하려면 클립을 설정 합니다. 기본값은 false입니다.| 
  
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

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   