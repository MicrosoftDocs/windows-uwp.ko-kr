---
title: /users/me/consumables/{itemID}
assetID: 45724827-5e35-326f-3f17-f49e606d9e08
permalink: en-us/docs/xboxlive/rest/uri-inventoryconsumablesitemurl.html
author: KevinAsgari
description: Xbox 사용자에 대해 소모 성 항목에 대 한 rESTful 끝점입니다.
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: bbdf869cffae575f53555b31d9ed66647d3d09b2
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2018
ms.locfileid: "6046584"
---
# <a name="usersmeconsumablesitemid"></a>/users/me/consumables/{itemID}
특정 소모 성 인벤토리 항목에 대 한 세부 정보의 전체 집합을 액세스 합니다.
이러한 Uri에 대 한 도메인은 `inventory.xboxlive.com`.

  * [URI 매개 변수](#ID4EV)

<a id="ID4EV"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| itemID| string| 단일 인벤토리 항목에 대 한 각 사용자에 게 고유한 ID|

<a id="ID4ERB"></a>


## <a name="valid-methods"></a>유효한 메서드

[POST ({itemID})](uri-inventoryconsumablesitemurlpost.md)

&nbsp;&nbsp;전체 또는 일부 소모 성 인벤토리 항목의 사용 되었는지 나타냅니다 점감 수량 요청 된 양만큼 소모 성입니다.

<a id="ID4E4B"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4E6B"></a>


##### <a name="parent"></a>부모

[마켓플레이스 URI](atoc-reference-marketplace.md)


<a id="ID4EJC"></a>


##### <a name="further-information"></a>자세한 정보

[EDS 공통 헤더](../../additional/edscommonheaders.md)

 [EDS 매개 변수](../../additional/edsparameters.md)

 [EDS 쿼리 구체화](../../additional/edsqueryrefiners.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)
