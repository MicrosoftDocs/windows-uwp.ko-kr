---
title: /users/{userId}/profile/settings/people/{userList}?settings={settings}
assetID: 0ba20eba-f0ab-28ab-61d3-b4f9e4c07bc5
permalink: en-us/docs/xboxlive/rest/uri-usersuseridprofilesettingspeopleuserlist.html
description: " /users/{userId}/profile/settings/people/{userList}?settings={settings}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 24b58c817156a7c372a8e6acfab895e6b7c51207
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636968"
---
# <a name="usersuseridprofilesettingspeopleuserlistsettingssettings"></a>/users/{userId}/profile/settings/people/{userList}?settings={settings}
사용자 프로필에 액세스 하거나 사용자 모니커를 사용 하 여 사용자를 지원 합니다. 이러한 Uri에 대 한 도메인은 `profile.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| userId| 문자열| 'Xuid(12345)', 'gt(myGamertag)' 또는 'me' 수 있습니다.| 
| userList| 문자열| 명명 된 목록 설정을 가져올 사용자입니다. 현재 사용자는 지원 되는 유일한 목록입니다.| 
  
<a id="ID4E1B"></a>

 
## <a name="valid-methods"></a>올바른 메서드

[GET (/users/{userId}/profile/settings/people/{userList})](uri-usersuseridprofilesettingspeopleuserlistget.md)

&nbsp;&nbsp;사용자에 대 한 프로필을 가져오거나 사용자 모니커를 사용 하 여 사용자를 지원 합니다.
 
<a id="ID4EEC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EGC"></a>

 
##### <a name="parent"></a>Parent 

[프로필 Uri](atoc-reference-profiles.md)

 [프로필 (JSON)](../../json/json-profile.md)

   