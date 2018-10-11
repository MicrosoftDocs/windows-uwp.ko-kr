---
title: /users/{ownerId}/people/avoid
assetID: 13dc3a10-ed04-4600-3afb-aa95a4139a06
permalink: en-us/docs/xboxlive/rest/uri-privacyusersxuidpeopleavoid.html
author: KevinAsgari
description: " /users/{ownerId}/people/avoid"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 635f11677997523fe952de04b8398410efc503d2
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4528976"
---
# <a name="usersowneridpeopleavoid"></a>/users/{ownerId}/people/avoid
사용자에 대 한 문제 방지 목록에 액세스

  * [URI 매개 변수](#ID4EQ)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| ownerId| string| 필수. 해당 리소스를 액세스 하는 사용자의 식별자입니다. 가능한 값은 <code>xuid({xuid})</code>. 인증 된 사용자 여야 합니다. 예제 값: <code>xuid(2603643534573581)</code>. 최대 크기: 없음. |

<a id="ID4ERB"></a>


## <a name="valid-methods"></a>유효한 메서드

[GET (/users/{ownerId}/people/avoid)](uri-privacyusersxuidpeopleavoidget.md)

&nbsp;&nbsp;사용자에 대 한 문제 방지 목록을 가져옵니다.

<a id="ID4E2B"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4E4B"></a>


##### <a name="parent"></a>부모

[개인 정보 URI](atoc-reference-privacyv2.md)
