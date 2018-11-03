---
title: UserTitle(JSON)
assetID: 3e5767af-5704-8463-696b-42a7d2a1e231
permalink: en-us/docs/xboxlive/rest/json-usertitlev2.html
author: KevinAsgari
description: " UserTitle(JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1735b7fcedd358c290b289b97417e0ea3a6c090b
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5983877"
---
# <a name="usertitle-json"></a>UserTitle(JSON)
사용자가 제목 데이터가 들어 있습니다. 
<a id="ID4EN"></a>

 
## <a name="usertitle"></a>UserTitle
 
UserTitle 개체에는 다음과 같이 지정 합니다. 모든 속성은 필수입니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| lastUnlock| DateTime| 성과 달성 마지막 시간입니다.| 
| titleId| 32 비트 부호 없는 정수| 제목에 대 한 고유 식별자입니다.| 
| titleVersion| string| 버전 제목입니다.| 
| serviceConfigId| string| 제목와 관련 된 기본 서비스 구성 설정의 ID입니다.| 
| titleType| string| 제목 형식입니다.| 
| 플랫폼| string| 지원 되는 플랫폼입니다.| 
| name| 문자열| 이 제목 텍스트 이름입니다. 최대 길이 22입니다.| 
| earnedAchievements| 32 비트 부호 없는 정수| 도전 과제 수가 획득 한 잠금 해제 된 도전 과제를 포함 하 여 제목 및 문제를 성공적으로 완료 합니다.| 
| currentGamerscore| 32 비트 부호 없는 정수| 총 게이머가이 제목에서이 사용자를 얻었습니다.| 
| maxGamerscore| 32 비트 부호 없는 정수| 이 제목에 대 한 총 가능한 게이머 합니다.| 
  
<a id="ID4EFE"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EHE"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   