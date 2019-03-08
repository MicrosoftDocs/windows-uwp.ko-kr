---
title: /users/{ownerId}/people/mute
assetID: efb929d8-79a7-83f0-c348-c92ced42bc05
permalink: en-us/docs/xboxlive/rest/uri-privacyusersowneridpeoplemute.html
description: " /users/{ownerId}/people/mute"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 86010d11ff0a7ebe188766f51bc3160a4f2befa4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641548"
---
# <a name="usersowneridpeoplemute"></a>/users/{ownerId}/people/mute
음소거 사용자 목록에 액세스합니다.

  * [URI 매개 변수](#ID4EQ)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- |
| ownerId| 문자열| 필수. 해당 리소스에 액세스 하는 사용자의 식별자입니다. 가능한 값은 "me" <code>xuid({xuid})</code>, 또는 gt({gamertag}) 합니다. 인증 된 사용자 여야 합니다. 예제 값: <code>xuid(2603643534573581)</code>, <code>gt(SomeGamertag)</code>합니다. 최대 크기: 없음. |

<a id="ID4ETB"></a>


## <a name="valid-methods"></a>올바른 메서드

[GET (/users/ {ownerId} / 사용자/음소거)](uri-privacyusersowneridpeoplemuteget.md)

&nbsp;&nbsp;사용자에 대 한 음소거 목록을 가져옵니다.

<a id="ID4E4B"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4E6B"></a>


##### <a name="parent"></a>Parent

[개인 정보 Uri](atoc-reference-privacyv2.md)
