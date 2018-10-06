---
title: Profile(JSON)
assetID: b92b1750-c2df-39b6-6c5c-f9e8068c8097
permalink: en-us/docs/xboxlive/rest/json-profile.html
author: KevinAsgari
description: " Profile(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0ae5e95befc6611c5905e6efe2bb01a396167626
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4392265"
---
# <a name="profile-json"></a>Profile(JSON)
사용자에 대 한 개인 프로필 설정 합니다. 
<a id="ID4EN"></a>

 
## <a name="profile"></a>프로필
 
프로필 개체에는 다음 사양을 있습니다.
 
| 멤버| 유형| 설명| 
| --- | --- | --- | 
| AppDisplayName| string| 앱의 표시 이름입니다. 사용자의 "실제 이름" 또는 개인 정보 보호에 따라 해당 게이머 수 있습니다. 이 설정은 앱에 표시 하기 위해 사용 해야 하는 사용자의 id 문자열을 나타냅니다.| 
| GameDisplayName| string| 게임에 표시 하기 위한 이름입니다. 사용자의 "실제 이름" 또는 개인 정보 보호에 따라 해당 게이머 수 있습니다. 이 설정은 게임에 표시 하기 위해 사용 해야 하는 사용자의 id 문자열을 나타냅니다.| 
| Gamertag| string| 사용자의 게이머 합니다.| 
| AppDisplayPicRaw| string| 원시 앱 디스플레이 pic URL (아래 참조).| 
| GameDisplayPicRaw| string| 원시 게임 디스플레이 pic URL (아래 참조).| 
| AccountTier| string| 사용자에 게 어떤 유형의 계정이 있습니까? 골드, 실버, 또는 FamilyGold?| 
| TenureLevel| 32 비트 부호 없는 정수| 몇 년 동안 사용자 된 Xbox live?| 
| 게이머 점수| 32 비트 부호 없는 정수| 사용자의 게이머 점수 합니다.| 
  


> [!NOTE] 
> 사용자의 '실제 그림' 또는 개인 정보 보호에 따라 해당 XboxOne gamerpic 사진 수 있습니다. 이러한 설정은 클라이언트에 표시 하기 위해 사용 해야 하는 사용자의 사진 url을 나타냅니다. 이 이미지 (사용자가 그림을 설정 되지 않은 나타냄) 비어 있을 수 있습니다. 


 
원시 URL은 크기 조정이 가능한 URL. 크기 및 형식을 추가 하 여 사용 하 여 다음 중 하나를 지정을 사용할 수 있습니다 `&format={format}&w={width}&h={height}` uri:
 
형식: png
 
크기: 64x64, 208 x 208, 424 x 424
 
<a id="ID4E2D"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E4D"></a>

 
##### <a name="parent"></a>부모 

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)

   