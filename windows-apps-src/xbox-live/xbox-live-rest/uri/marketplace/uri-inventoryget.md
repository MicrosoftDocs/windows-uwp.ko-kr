---
title: GET (/users/me/inventory)
assetID: 7b74dd08-2854-319d-3ed0-ddee75d922b9
permalink: en-us/docs/xboxlive/rest/uri-inventoryget.html
author: KevinAsgari
description: " GET (/users/me/inventory)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 270f36d354ee4561d12f78644436ad51e286768c
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5436950"
---
# <a name="get-usersmeinventory"></a>GET (/users/me/inventory)
현재 호출자에 게 제공 되는 사용자와 관련 된 인벤토리 집합을 제공 합니다.
이러한 Uri에 대 한 도메인은 `inventory.xboxlive.com`.

  * [설명](#ID4EV)
  * [쿼리 문자열 매개 변수](#ID4EHB)
  * [샘플 요청](#ID4EDE)
  * [응답 본문](#ID4ERE)

<a id="ID4EV"></a>


## <a name="remarks"></a>설명

정책이 없는 검사를 적용 하는 또는 필터링이 호출의 일부로 발생 합니다. 호출자는 쿼리 매개 변수를 전달 하 여 반환 된 결과의 범위를 좁히려면의 옵션이 있습니다.

이전 응답을 통해 제공 된 호출자 연속 토큰을 사용 하 여 결과를 페이지 수: **/users/me/inventory?continuationToken continuationTokenString =**.

호출자에 게 특정 항목에 대 한 정보를 표시 하려면 특정 항목 URL 사용 하 여 API 세부 정보에 대 한 호출을 만들 수 있습니다.

<a id="ID4EHB"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| 가용성| string| 반환할 항목의 현재 가용성입니다. 기본값은 "가능" 반환 항목 현재 날짜가 포함의 시작 날짜와 종료 날짜 범위입니다. 다른 값 "모든"를 반환 하는 모든 항목 및 "를 사용할 수 없음" 현재 날짜는 항목을 반환 하는 시작 날짜와 종료 날짜 범위 및 따라서 현재 사용할 수 없는 것 벗어나는 포함 됩니다. |
| 컨테이너| string| 선택 사항입니다. 인벤토리의 결과만 해당 게임에 관련 된 항목을 포함 하는 게임의 제품 ID 값을 설정 합니다. 특정 게임의 제품으로 결과 필터링 하려면 서버에서 인벤토리를 호출할 때 특히 유용 합니다.|
| expandSatisfyingEntitlements| string| 응답에 사용자가 결과 내에서 반환 되는 모든 만족 권리 유형을 포함 하는 경우를 나타내는 플래그입니다. 기본값은 "false"입니다. 이 매개 변수 값이 "true"를 통해 구독 혜택 Xbox one, Xbox 360 구매 마이그레이션할 권리 유형 같은 번들에 항목을 만족 사용자에 게 부여 된 제품 사용 되는 경우 등 결과에 추가 됩니다. 이 값은 "false" 하는 경우 결과에 포함 된 개별 항목이 아니라 번들의 ProductID 등의 상위 항목에만 반환 됩니다. **참고:** ItemType 매개 변수는 URI에 포함 되지 않은 경우에이 매개 변수를 사용 하 여 "true" 값만 지원, 그렇지 않으면 HTTP 400 오류가 나타납니다. |  
  | productIds | string |  특히 사용자의 인벤토리에서 검색 하려는 ProductIds 컬렉션이 구분 하 여 ','.  사용자가 인벤토리 결과에 제공 된 ProductID 수 없는 경우 해당 항목에에서 나타나지 않습니다 결과 API 호출에서. True로 expandSatisfyingEntitlements 매개 변수 집합 함께 번들의 productID을 전달 하는 경우 (또는 자신의 productIds 쿼리 문자열에 지정) 여부 번들에 포함 된 모든 항목 호출 결과에 반환 됩니다.   |
  | 상태 | string | 반환할 항목의 상태입니다. 기본값은 "all" 하는 모든 항목을 반환 합니다. 다른 값은 "사용"만 해당 itemsthat의 사용을 지정 하는 반환할지, "중단" 하는 일시 중단 시 항목만 반환할지, "만료" 만료는 항목만 반환할지를 나타냅니다 나타냅니다 " 취소는 항목만 반환할지, 및 갱신 되었는지를 항목만 반환할지 나타냅니다 "Renewed"는 나타냅니다 "을 취소 합니다.  |

뿐만 아니라 리소스 표준 페이징 기술을 지원합니다.

<a id="ID4EDE"></a>


## <a name="sample-request"></a>샘플 요청

이 URI 메서드에 대 한 정규화 된 도메인 이름이 `https://inventory.xboxlive.com/users/me/inventory.
         `

> [!NOTE] 
> 어떤 사용자 것으로 간주 됩니다에 따라 다름 제공 토큰 여러 사용자를 포함할 수 있음 단일 사용자의 인벤토리를 원하는 경우 단독으로 고려해 야 할 특정 사용자에 대 한 사용자 해시를 제공 해야 합니다.

.

<a id="ID4ERE"></a>


## <a name="response-body"></a>응답 본문

호출 되 면 서비스 인벤토리 항목의 배열을 반환 합니다. [InventoryItem (JSON)를](../../json/json-inventoryitem.md)참조 하세요.

<a id="ID4E4E"></a>


### <a name="sample-response"></a>예제 응답


```cpp
{
  "pagingInfo": {
    "continuationToken": string,
    "totalItems": int
  },
  "items":
  {
    "url": string,
    "itemType": "Music",
    "titleId": string,
    "containers": string,
    "obtained": DateTime,
    "startDate": DateTime,
    "endDate": DateTime,
    "state": "Enabled"  
}

```


<a id="ID4EHF"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EJF"></a>


##### <a name="parent"></a>부모

[/users/me/inventory](uri-inventory.md)


<a id="ID4ETF"></a>


##### <a name="further-information"></a>자세한 정보

[EDS 공통 헤더](../../additional/edscommonheaders.md)

 [EDS 매개 변수](../../additional/edsparameters.md)

 [EDS 쿼리 구체화](../../additional/edsqueryrefiners.md)

 [마켓플레이스 URI](atoc-reference-marketplace.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)
