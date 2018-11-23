---
title: POST ({itemID})
assetID: 2c3c166b-e638-cfb9-d68e-9f8ab9a838d3
permalink: en-us/docs/xboxlive/rest/uri-inventoryconsumablesitemurlpost.html
author: KevinAsgari
description: " POST ({itemID})"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: da2a3fc507915f3ed20b718f5a40cca3d0ba5e0b
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/23/2018
ms.locfileid: "7559518"
---
# <a name="post-itemid"></a>POST ({itemID})
전체 또는 일부 소모 성 인벤토리 항목의 사용 되었는지 나타냅니다 점감 수량 요청 된 양만큼 소모 성입니다.
이러한 Uri에 대 한 도메인은 `inventory.xboxlive.com`.

  * [설명](#ID4EX)
  * [URI 매개 변수](#ID4EQB)
  * [요청 본문](#ID4E2B)
  * [응답 본문](#ID4ENC)

<a id="ID4EX"></a>


## <a name="remarks"></a>설명

   * 호출자가 사용 해야 수량 나머지 제공할 수 있는 항목을 초과 하면 호출이 거부 됩니다.
   * 호출자가 사용 해야 수량 양의 정수 0 이어야 합니다. 소비 값이 0 이하로 호출이 거부 됩니다.
   * 호출자는 빈 트랜잭션 ID를 제공 하는 경우 요청이 거부 됩니다.
   * 사용 가능한 경우 소비를 보고 하는 어떤 제목 결정을 수행할 수 있도록 제목 클레임 기록 됩니다.
   * 일정 기간에 대 한 동일한 transactionId 사용 하 여 추가 게시물 무시 됩니다.


> [!NOTE]
> 이 API에 대 한 <b>xbl 계약 버전 x 헤더</b> 는 "4"입니다.


<a id="ID4EQB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- |
| itemID| string| 단일 인벤토리 항목에 대 한 각 사용자에 게 고유한 ID|

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


제거 수량 필드에서 소모 성의 나머지 수량 제거 하고자 하는 소모 성 수량을 나타내기 위해 호출자를 허용 합니다. 트랜잭션 ID 필드는 소모 성 콘텐츠 작업을 사용 하 여 동일한 사용량을 두 번 계산의 위험을 제한 하는 동안 다시 시도 하는 수단을 사용 하 여 호출자를 제공 합니다.

<a id="ID4ENC"></a>


## <a name="response-body"></a>응답 본문

인증을 통과 하 고 적절 한 권한 부여 컨텍스트를 할당 게시물에 대 한 응답이 동일한 transactionId 사용 하 여 영수증의 acknolodgement POST 요청, 소모 성 항목의 URL 및 항목의 새 서비스에 전달 수량 값입니다.

<a id="ID4EVC"></a>


### <a name="sample-response"></a>예제 응답


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


##### <a name="parent"></a>부모

[/users/me/consumables/{itemID}](uri-inventoryconsumablesitemurl.md)


<a id="ID4ELD"></a>


##### <a name="further-information"></a>자세한 정보

[EDS 공통 헤더](../../additional/edscommonheaders.md)

 [EDS 매개 변수](../../additional/edsparameters.md)

 [EDS 쿼리 구체화](../../additional/edsqueryrefiners.md)

 [마켓플레이스 URI](atoc-reference-marketplace.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)
