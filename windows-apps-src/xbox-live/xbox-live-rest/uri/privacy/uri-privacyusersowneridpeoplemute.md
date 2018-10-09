---
title: /users/{ownerId}/people/mute
assetID: efb929d8-79a7-83f0-c348-c92ced42bc05
permalink: en-us/docs/xboxlive/rest/uri-privacyusersowneridpeoplemute.html
author: KevinAsgari
description: " /users/{ownerId}/people/mute"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a5de74be5e82fde007d6680eaf4c9e5a543afc64
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2018
ms.locfileid: "4461139"
---
# <a name="usersowneridpeoplemute"></a>/users/{ownerId}/people/mute
사용자에 대 한 음소거 목록에 액세스합니다.

  * [URI 매개 변수](#ID4EQ)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| ownerId| string| 필수. 해당 리소스를 액세스 하는 사용자의 식별자입니다. 가능한 값은 "me" <code>xuid({xuid})</code>, 또는 gt({gamertag}) 합니다. 인증된 된 사용자를 이어야 합니다. 예제 값: <code>xuid(2603643534573581)</code>, <code>gt(SomeGamertag)</code>. 최대 크기: 없음. |

<a id="ID4ETB"></a>


## <a name="valid-methods"></a>유효한 메서드

[GET (/users/{ownerId}/people/mute)](uri-privacyusersowneridpeoplemuteget.md)

&nbsp;&nbsp;사용자에 대 한 음소거 목록을 가져옵니다.

<a id="ID4E4B"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4E6B"></a>


##### <a name="parent"></a>부모

[개인 정보 URI](atoc-reference-privacyv2.md)