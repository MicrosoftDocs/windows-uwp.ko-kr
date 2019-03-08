---
title: GET (/users/me/inventory)
assetID: 7b74dd08-2854-319d-3ed0-ddee75d922b9
permalink: en-us/docs/xboxlive/rest/uri-inventoryget.html
description: " GET (/users/me/inventory)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 31c787108fad84f06b02ded3958f9d2f99727cbe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632208"
---
# <a name="get-usersmeinventory"></a>GET (/users/me/inventory)
현재 호출자에 게 제공 된 사용자와 연결 된 인벤토리를 제공 합니다.
이러한 Uri에 대 한 도메인은 `inventory.xboxlive.com`합니다.

  * [설명](#ID4EV)
  * [쿼리 문자열 매개 변수](#ID4EHB)
  * [샘플 요청](#ID4EDE)
  * [응답 본문](#ID4ERE)

<a id="ID4EV"></a>


## <a name="remarks"></a>설명

정책이 없는 확인을 적용 하거나이 호출의 일부로 필터링이 발생 합니다. 호출자가 반환 된 결과의 범위를 좁힐 하려면 쿼리 매개 변수를 전달 하는 옵션이 있습니다.

이전 응답을 통해 제공 된 연속 토큰을 사용 하 여 결과 통해 호출자 페이지 수: **/users/me/inventory? continuationToken continuationTokenString =** 합니다.

호출자가 특정 항목에 대 한 정보를 보려면 세부 정보는 특정 항목 URL 사용 하 여 API에 대 한 호출을 만들 수 있습니다.

<a id="ID4EHB"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- |
| 가용성| 문자열| 현재 가용성 반환할 항목입니다. 기본값은 "사용 가능"이 고 현재 날짜가 포함 된 시작 날짜와 끝 사이 반환 항목 날짜 범위입니다. 다른 값으로는 "All"을 반환 하는 모든 항목 및 "사용할 수 없음" 현재 날짜는 항목을 반환 하는 시작 날짜 및 종료 날짜 범위 및 따라서 현재 사용할 수 없는 벗어납니다. |
| 컨테이너| 문자열| 선택 사항. 인벤토리에서 결과 게임에 관련 된 항목에만 포함 하는 다음 게임, 제품 id 값을 설정 합니다. 특정 게임의 제품으로 결과 필터링 하려면 서버에서 인벤토리를 호출 하는 경우 특히 유용 합니다.|
| expandSatisfyingEntitlements| 문자열| 응답은 사용자가 결과 내에서 반환 되는 모든 만족 자격을 포함 하는 경우를 나타내는 플래그입니다. 기본값은 "false"입니다. 이 매개 변수를 "true"를 통해 구독 혜택 Xbox one, Xbox 360 구매 마이그레이션할 자격 번들과 같은 항목에 만족 사용자에 게 부여 되는 모든 제품을 함께 사용 하면 등 결과에 추가 됩니다. 이 값은 "false" 하는 경우 결과에 포함 된 개별 항목이 아닌 번들의 ProductID와 같은 상위 항목만 반환 됩니다. **참고:** ItemType 매개 변수는 URI에 포함 되지 않습니다 하는 경우에 지원 됩니다 "true" 값을 사용 하 여이 매개 변수를 사용 하 여, 그렇지 않으면 HTTP 400 오류가 나타납니다. |  
  | productIds | 문자열 |  특히 사용자의 인벤토리에서 검색 하려는 Productid의 컬렉션인 구분 하 여 ','.  사용자 인벤토리 결과에 제공 된 ProductID 수 없는 경우 해당 항목이 나타나지 않습니다 결과에서 API 호출에서. ProductID expandSatisfyingEntitlements 매개 변수 집합을 함께 번들에서 true를 전달 하면 번들에 포함 된 모든 항목 (여부는 Productid 쿼리 문자열에서 지정한) 여부 호출 결과에 반환 됩니다.   |
  | 상태 | 문자열 | 반환할 항목의 상태입니다. 기본값은 "all", 하는 모든 항목을 반환 합니다. 다른 값은 "활성화"를 해당만 itemsthat 활성화할지를 나타내는 반환 되어야 합니다 "일시 중단" 일시 중지 된 항목만 반환 되어야 함을 만료 하는 항목만 반환 되어야 함을 나타냅니다는 "만료"를 나타내는 " 취소 되는 항목만 반환 하 고 갱신 되 었어야 하는 항목만 반환 되어야 함을 나타내는 "Renewed"는 나타냅니다 "취소 합니다.  |

이 외에도 리소스 표준 페이징 메커니즘을 지원합니다.

<a id="ID4EDE"></a>


## <a name="sample-request"></a>샘플 요청

이 URI 메서드에 대 한 정규화 된 도메인 이름이 `https://inventory.xboxlive.com/users/me/inventory.
         `

> [!NOTE] 
> 사용자를 사용 하는 것으로 간주에 따라 달라 집니다 제공 된, 토큰이 여러 사용자가 포함 될 수 있습니다. 단일 사용자의 인벤토리를 원한다 면 단독으로 고려해 야 할 특정 사용자에 대 한 사용자 해시를 제공 해야 합니다.

.

<a id="ID4ERE"></a>


## <a name="response-body"></a>응답 본문

호출이 성공 하면 서비스 인벤토리 항목의 배열을 반환 합니다. 참조 [inventoryItem (JSON)](../../json/json-inventoryitem.md)합니다.

<a id="ID4E4E"></a>


### <a name="sample-response"></a>샘플 응답


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


##### <a name="parent"></a>Parent

[/users/me/inventory](uri-inventory.md)


<a id="ID4ETF"></a>


##### <a name="further-information"></a>추가 정보

[EDS 일반적인 헤더](../../additional/edscommonheaders.md)

 [EDS 매개 변수](../../additional/edsparameters.md)

 [EDS 쿼리 구체화](../../additional/edsqueryrefiners.md)

 [Marketplace Uri](atoc-reference-marketplace.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)
