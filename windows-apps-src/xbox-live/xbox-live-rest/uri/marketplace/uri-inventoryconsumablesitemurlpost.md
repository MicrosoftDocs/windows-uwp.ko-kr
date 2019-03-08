---
title: POST ({itemID})
assetID: 2c3c166b-e638-cfb9-d68e-9f8ab9a838d3
permalink: en-us/docs/xboxlive/rest/uri-inventoryconsumablesitemurlpost.html
description: " POST ({itemID})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 877986ce9d48269295a68dbfd644f14785916b88
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640858"
---
# <a name="post-itemid"></a>POST ({itemID})
일부 또는 전부를 사용할 수 있도록 인벤토리 항목의 사용 되었음을 나타냅니다 롤백하고 요청 된 크기 만큼 사용할 수 있는 양입니다.
이러한 Uri에 대 한 도메인은 `inventory.xboxlive.com`합니다.

  * [설명](#ID4EX)
  * [URI 매개 변수](#ID4EQB)
  * [요청 본문](#ID4E2B)
  * [응답 본문](#ID4ENC)

<a id="ID4EX"></a>


## <a name="remarks"></a>설명

   * 호출자에 게 사용 하도록 요청 수량 항목의 나머지 공급을 초과 하면 호출이 거부 됩니다.
   * 호출자에 게 사용 하 라는 메시지가 표시 된 수량에는 0 이상의 양의 정수 여야 합니다. 0 이하로 사용량 값을 사용 하 여 호출 거부 됩니다.
   * 호출자에 게는 빈 트랜잭션 ID를 제공 하는 경우 요청이 거부 됩니다.
   * 사용 가능한 경우에 제목 사용량 보고 결정할 수 있도록 제목 클레임이 로깅됩니다.
   * 일정 기간 동안 동일한 트랜잭션 Id 사용 하 여 추가 게시물은 무시 됩니다.


> [!NOTE]
> 합니다 <b>x xbl-계약 버전 헤더가</b> 이 API는 "4"에 대 한 합니다.


<a id="ID4EQB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- | --- |
| itemID| 문자열| 단일 인벤토리 항목에 대 한 각 사용자에 게 고유 ID|

<a id="ID4E2B"></a>


## <a name="request-body"></a>요청 본문

<a id="ID4EBC"></a>


### <a name="sample-request"></a>샘플 요청


```cpp
{
  "transactionId": String
  "removeQuantity": Int
}

```


제거 수량 필드를 사용할 수 있는 남은 수량에서 제거 하려는 사용할 수 있는 수량을 표시 하기 위해 호출자를 허용 합니다. 트랜잭션 ID 필드에 동일한 사용을 두 번 계산의 위험을 제한 하는 동안 사용할 수 있는 콘텐츠 작업을 사용 하 여 다시 시도 하는 수단을 사용 하 여 호출자에 게를 제공 합니다.

<a id="ID4ENC"></a>


## <a name="response-body"></a>응답 본문

인증을 전달 하 고 적절 한 권한 부여 컨텍스트를 할당 하는 가정 하 고 게시물에 대 한 응답이 동일한 트랜잭션 Id 사용 하 여 수신 된 acknolodgement POST 요청, 사용할 수 있는 항목의 URL 및 항목의 새 서비스에 전달 수량 값입니다.

<a id="ID4EVC"></a>


### <a name="sample-response"></a>샘플 응답


```cpp
{
  "transactionId": String
  "url": String
  "newQuantity": Int
}

```


<a id="ID4E6C"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EBD"></a>


##### <a name="parent"></a>Parent

[/users/me/consumables/{itemID}](uri-inventoryconsumablesitemurl.md)


<a id="ID4ELD"></a>


##### <a name="further-information"></a>추가 정보

[EDS 일반적인 헤더](../../additional/edscommonheaders.md)

 [EDS 매개 변수](../../additional/edsparameters.md)

 [EDS 쿼리 구체화](../../additional/edsqueryrefiners.md)

 [Marketplace Uri](atoc-reference-marketplace.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)
