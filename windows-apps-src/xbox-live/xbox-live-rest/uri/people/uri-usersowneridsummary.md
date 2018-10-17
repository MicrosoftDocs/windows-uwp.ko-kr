---
title: /users/{ownerId}/summary
assetID: 63f8ed09-532d-381e-59e6-2849893df5bf
permalink: en-us/docs/xboxlive/rest/uri-usersowneridsummary.html
author: KevinAsgari
description: " /users/{ownerId}/summary"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1cf5fc70d2f4b149f7a5c6dd20c5aaf22cafe2a7
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4749427"
---
# <a name="usersowneridsummary"></a>/users/{ownerId}/summary
호출자의 관점에서 소유자에 대 한 요약 데이터에 액세스 합니다.

  * [URI 매개 변수](#ID4EQ)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| ownerId| string| 해당 리소스를 액세스 하는 사용자의 식별자입니다. 가능한 값은 "me", xuid({xuid}), 또는 gt({gamertag}) 합니다. 예제 값: <code>me</code>, <code>xuid(2603643534573581)</code>, <code>gt(SomeGamertag)</code>|

<a id="ID4ESB"></a>


## <a name="valid-methods"></a>유효한 메서드

[GET (/users/{ownerId}/summary)](uri-usersowneridsummaryget.md)

&nbsp;&nbsp;호출자의 관점에서 소유자에 대 한 요약 데이터를 가져옵니다.

<a id="ID4E3B"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4E5B"></a>


##### <a name="parent"></a>부모

[/users/{ownerId}/summary]()
