---
title: Profile(JSON)
assetID: b92b1750-c2df-39b6-6c5c-f9e8068c8097
permalink: en-us/docs/xboxlive/rest/json-profile.html
description: " Profile(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7299fcb4d375a3fc35ad67306b70f5fa4afde963
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607518"
---
# <a name="profile-json"></a>Profile(JSON)
사용자의 개인 프로필 설정입니다. 
<a id="ID4EN"></a>

 
## <a name="profile"></a>프로필
 
프로필 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| AppDisplayName| 문자열| 앱의 표시 이름입니다. 사용자의 "실제 name" 또는 개인 정보에 따라 해당 게이머 태그 수 있습니다. 이 설정은 앱에 표시 하기 위해 사용 해야 하는 사용자의 id 문자열을 나타냅니다.| 
| GameDisplayName| 문자열| 게임의 표시 이름입니다. 사용자의 "실제 name" 또는 개인 정보에 따라 해당 게이머 태그 수 있습니다. 이 설정은 게임에 표시 하기 위해 사용 해야 하는 사용자의 id 문자열을 나타냅니다.| 
| Gamertag| 문자열| 사용자의 게이머 태그입니다.| 
| AppDisplayPicRaw| 문자열| 원시 앱 표시 pic URL (아래 참조).| 
| GameDisplayPicRaw| 문자열| 원시 게임 표시 pic URL (아래 참조).| 
| AccountTier| 문자열| 사용자에 게 어떤 유형의 계정이 있습니까? Gold, Silver, 또는 FamilyGold?| 
| TenureLevel| 32 비트 부호 없는 정수| 수 년 동안 사용자는 왔습니다 Xbox Live?| 
| 게임| 32 비트 부호 없는 정수| 사용자의 게임입니다.| 
  


> [!NOTE] 
> 그림에는 사용자의 '실제 그림' 또는 개인 정보에 따라 해당 XboxOne gamerpic 수 있습니다. 이러한 설정은 클라이언트에 표시 하기 위해 사용 해야 하는 사용자의 그림 url을 나타냅니다. 이 이미지 (사용자가 사진을 설정 하지 않은 것을 나타냄) 비어 있을 수 있습니다. 


 
원시 URL은 크기 조정이 가능한 URL입니다. 크기 및 추가 하 여 사용 하 여 형식 중 하나를 지정 하려면 사용할 수 있습니다 `&format={format}&w={width}&h={height}` uri:
 
형식: png
 
크기: 64x64, 208x208, 424x424
 
<a id="ID4E2D"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E4D"></a>

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

   