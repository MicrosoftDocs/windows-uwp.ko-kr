---
title: UserTitle(JSON)
assetID: 3e5767af-5704-8463-696b-42a7d2a1e231
permalink: en-us/docs/xboxlive/rest/json-usertitlev2.html
description: " UserTitle(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 33901a5bde25fd17072c2b45d587a33209424378
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623028"
---
# <a name="usertitle-json"></a>UserTitle(JSON)
사용자 제목 데이터를 포함합니다. 
<a id="ID4EN"></a>

 
## <a name="usertitle"></a>UserTitle
 
UserTitle 개체에 다음과 같이 지정 합니다. 모든 속성은 필수입니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| lastUnlock| DateTime| 마지막으로 성과 달성 하는 시간입니다.| 
| titleId| 32 비트 부호 없는 정수| 제목에 대 한 고유 식별자입니다.| 
| titleVersion| 문자열| 타이틀의 버전입니다.| 
| serviceConfigId| 문자열| 제목과 연결 된 기본 서비스 구성 집합의 ID입니다.| 
| titleType| 문자열| 제목 유형입니다.| 
| 플랫폼| 문자열| 지원 되는 플랫폼입니다.| 
| name| 문자열| 이 제목은의 텍스트 이름입니다. 최대 길이 22입니다.| 
| earnedAchievements| 32 비트 부호 없는 정수| 잠금이 해제 된 성과 포함 하 여 제목에 획득 하 고 과제를 성공적으로 완료 하는 성과의 수입니다.| 
| currentGamerscore| 32 비트 부호 없는 정수| 총 게임이이 제목에이 사용자를 얻었습니다.| 
| maxGamerscore| 32 비트 부호 없는 정수| 이 제목에 대 한 총 가능한 게임입니다.| 
  
<a id="ID4EFE"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EHE"></a>

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

   