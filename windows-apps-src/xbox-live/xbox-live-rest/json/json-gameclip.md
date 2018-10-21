---
title: GameClip(JSON)
assetID: 204cb702-4ce4-85a8-f231-3b4fb243405f
permalink: en-us/docs/xboxlive/rest/json-gameclip.html
author: KevinAsgari
description: " GameClip(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 047f7287578f52591c48ee059e72efb559b41c87
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2018
ms.locfileid: "5160353"
---
# <a name="gameclip-json"></a>GameClip(JSON)
 
<a id="ID4EO"></a>

 
## <a name="gameclip"></a>GameClip
 
GameClip 개체에는 다음과 같이 지정 합니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| <b>gameClipId</b>| string| 게임 클립에 할당 된 ID입니다.| 
| <b>상태</b>| GameClipState| 시스템에서 게임 클립의 상태입니다.| 
| <b>dateRecorded</b>| DateTime| 날짜 및 UTC (ISO 8601 형식)에서 녹음/녹화 시작 된 시간입니다.| 
| <b>lastModified</b>| DateTime| 마지막으로 수정한 시간 게임 클립 또는 UTC (ISO 8601 형식)에 메타 데이터입니다.| 
| <b>userCaption</b>| string| 사용자가 입력 한 지역화 되지 않은 문자열 게임 클립에 대해입니다.| 
| <b>type</b>| GameClipTypes| 클립의 형식입니다. 여러 값 및 그렇다면 쉼표로 구분 됩니다.| 
| <b>원본</b>| GameClipSource| 어떻게 클립 였던 합니다.| 
| <b>visibility</b>| GameClipVisibility| 시스템에서 게시 되 면 게임 클립의 표시 여부입니다.| 
| <b>durationInSeconds</b>| 32 비트 부호 없는 정수| 몇 초만에 게임 클립의 지속 됩니다.| 
| <b>서비스 안내</b>| string| 게임 클립 연결 되어 있는 서비스 안내 합니다.| 
| <b>rating</b>| 배정밀도 부동 소수점 숫자| 관련 된 게임을 0.0에서 5.0 범위에 대 한 평점.| 
| <b>ratingCount</b>| 32 비트 부호 없는 정수| 이 클립 등급이 횟수입니다.| 
| <b>레이아웃</b>| 32 비트 부호 없는 정수| 게임 클립와 관련 된 보기의 수입니다.| 
| <b>titleData</b>| string| 제목 관련 속성 모음입니다.| 
| <b>titleData</b>| string| 콘솔 관련 속성 모음입니다.| 
| <b>미리 보기</b>| GameClipThumbnail의 배열| GameClipThumbnail 개체의 배열입니다.| 
| <b>gameClipUris</b>| GameClipUri의 배열| GameClipUri 개체의 배열입니다.| 
| <b>xuid</b>| string| 마샬링하 문자열로 게임 클립의 소유자의 XUID 합니다.| 
| <b>clipName</b>| string| 클립의 이름, 제목 관리 시스템에서 조회 하는 대로 요청의 입력된 로캘에 따라의 지역화 된 버전입니다.| 
  
<a id="ID4ERH"></a>

 
## <a name="sample-json-syntax"></a>샘플 JSON 구문
 

```json
{
     "id": "7ce5c1a7-1255-46d3-a90e-34a0e2dfab06",
     "xuid": "2716903703773872",
     "state": "Published", 
     "dateRecorded": "2012-12-23T12:00:00Z",
     "lastModified": "2012-10-31T10:45:00Z",
     "clipName": "Forza 4",
     "userCaption": "My awesome car flip",
     "type": "DeveloperInitiated, Achievement",
     "source": "TitleDirect",
     "visibility": "Default",
     "durationInSeconds": 30,
     "scid": "ecb5497e-76d4-4a8a-870d-e76a26306b7d",
     "rating": 1.0,
     "views": 5,
     "thumbnails": [
       {
         "uri": "http://gameclips.xbox.com/thumbnails/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/small.jpg",
         "fileSize": 123,
         "width": 120,
         "height": 250
       }
     ],
     "gameClipUris": [
       {
         "uri": "http://gameclips.xbox.com/clips/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/clip.mp4",
         "fileSize": 1234565,
         "uriType": "Download",
         "expiration": "9999-12-31T23:59:59.9999999"
       }
     ]
   }
    
```

  
<a id="ID4E1H"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E3H"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   