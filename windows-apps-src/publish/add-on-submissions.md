---
description: 앱 내 제품이 라고도 하는 추가 기능을 고객이 구매할 수 있는 앱에 대 한 보조 항목으로 제출 하는 방법에 대해 알아봅니다.
title: 추가 기능 제출
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, iap, 앱 내 구매, 앱 내 제품, iap 제출
ms.localizationpriority: medium
ms.openlocfilehash: 25246d192f40bae096c33fae00feb1bfc2a4c530
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162150"
---
# <a name="add-on-submissions"></a>추가 기능 제출

추가 기능 (앱 내 제품이 라고도 함)은 고객이 구매할 수 있는 앱에 대 한 보충 항목입니다. 추가 기능은 흥미로운 새 기능, 새 게임 수준 또는 사용자가 계속 사용할 수 있는 것으로 생각 하는 것이 될 수 있습니다. 추가 기능을 사용 하 여 비용을 절감할 수 있을 뿐만 아니라 고객 상호 작용 및 참여를 추진 하는 데 도움이 됩니다.

추가 기능은 [파트너 센터](https://partner.microsoft.com/dashboard)를 통해 게시 되며 활성 [개발자 계정이](https://developer.microsoft.com/store/register)있어야 합니다. 또한 앱 코드에서 [추가 기능을 사용 하도록 설정](../monetize/in-app-purchases-and-trials.md) 해야 합니다.

추가 기능 제출 프로세스의 첫 번째 단계는 [제품 유형 및 제품 ID를 정의](set-your-add-on-product-id.md)하 여 파트너 센터에서 추가 기능을 만드는 것입니다. 그런 후에는 Microsoft Store를 통해 추가 기능을 구매할 수 있도록 제출을 만듭니다. [앱을 제출할](app-submissions.md)때 추가 기능을 제출 하거나 독립적으로 작업할 수 있습니다. 앱을 다시 다시 제출할 필요 없이 앱이 스토어에 있는 후에 추가 기능을 [업데이트할](#updating-an-add-on-after-publication) 수 있습니다.

> [!NOTE]
> 설명서의이 섹션에서는 파트너 센터에서 추가 기능을 제출 하는 방법에 대해 설명 합니다. 또는 [Microsoft Store 제출 API](../monetize/create-and-manage-submissions-using-windows-store-services.md) 를 사용 하 여 추가 기능 제출을 자동화할 수 있습니다.


## <a name="checklist-for-submitting-an-add-on"></a>추가 기능을 제출 하기 위한 검사 목록

다음은 추가 기능 제출을 만들 때 제공 하는 정보 목록입니다. 제공 해야 하는 항목은 아래에 나와 있습니다. 이러한 값 중 일부는 선택 사항이 며, 원하는 대로 변경할 수 있는 기본값이 이미 제공 되어 있습니다.


### <a name="create-a-new-add-on-page"></a>새 추가 기능 페이지 만들기

| 필드 이름                    | 참고                            |
|-------------------------------|----------------------------------|
| [**제품 유형**](set-your-add-on-product-id.md#product-type)      | 필수 |  
| [**Product ID**](set-your-add-on-product-id.md#product-id)          | 필수 |        


### <a name="properties-page"></a>속성 페이지

| 필드 이름                    | 참고                              |   
|-------------------------------|------------------------------------|
| [**제품 수명**](enter-add-on-properties.md#product-lifetime)  | 제품 형식이 **내구성이**있는 경우 필수입니다. 다른 제품 유형에는 적용 되지 않습니다. |
| [**수량**](enter-add-on-properties.md#quantity)  | 제품 형식이 **저장소 관리**의 사용할 수 있는 경우에 필요 합니다. 다른 제품 유형에는 적용 되지 않습니다. |
| [**구독 기간**](enter-add-on-properties.md#subscription-period)          | 제품 유형이 **Subscription**인 경우 필수입니다. 다른 제품 유형에는 적용 되지 않습니다.       |  
| [**무료 평가판**](enter-add-on-properties.md#free-trial)          | 제품 유형이 **Subscription**인 경우 필수입니다. 다른 제품 유형에는 적용 되지 않습니다.       |
| [**내용 유형**](enter-add-on-properties.md#content-type)          | 필수    |               
| [**키워드**](enter-add-on-properties.md#keywords)                  | 선택 사항 (최대 10 개 키워드, 각각 30 자 제한) |
| [**사용자 지정 개발자 데이터**](enter-add-on-properties.md#custom-developer-data)   | 선택 사항 (3000 문자 제한)            |


### <a name="pricing-and-availability-page"></a>가격 책정 및 가용성 페이지

| 필드 이름                    | 참고                                       |
|-------------------------------|---------------------------------------------|
| [**시장**](set-add-on-pricing-and-availability.md#markets)  | 기본값: 모든 가능한 시장 |
| [**가시 거리**](set-add-on-pricing-and-availability.md#visibility)   | 기본값: 구매할 수 있습니다. 앱 목록에 표시 될 수 있습니다. |
| [**예약**](set-add-on-pricing-and-availability.md#schedule)    | 기본값: 가능한 한 빨리 릴리스
| [**가격 책정**](set-add-on-pricing-and-availability.md#pricing)                | 필수                                    |
| [**판매 가격**](put-apps-and-add-ons-on-sale.md)               | 선택 사항                    |


### <a name="store-listings"></a>스토어 목록

하나의 저장소 목록이 필요 합니다. 앱에서 지 원하는 모든 [언어](create-add-on-store-listings.md#store-listing-languages) 에 대해 스토어 목록을 제공 하는 것이 좋습니다.

| 필드 이름                    | 참고                                       |
|-------------------------------|---------------------------------------------|
| [**제목과**](create-add-on-store-listings.md#title)                    | 필수 (100 문자 제한)           |
| [**설명**](create-add-on-store-listings.md#description)       | 선택 사항 (200 문자 제한)            |
| [**아이콘과**](create-add-on-store-listings.md#icon)                    | 선택 사항 (.png, 300x300 픽셀)            |


이 정보를 입력 했으면 **스토어에 제출**을 클릭 합니다. 대부분의 경우 인증 프로세스는 약 1 시간 정도 걸립니다. 그러면 추가 기능이 스토어에 게시 되 고 고객이 구매할 준비가 됩니다.

> [!NOTE]
> 또한 추가 기능을 앱 코드에서 구현 해야 합니다. 자세한 내용은 [앱에서 바로 구매 및 평가판](../monetize/in-app-purchases-and-trials.md)을 참조 하세요.


## <a name="updating-an-add-on-after-publication"></a>게시 후 추가 기능 업데이트

언제 든 지 게시 된 추가 기능을 변경할 수 있습니다. 추가 기능 변경 내용은 앱에 독립적으로 제출 및 게시 되므로 가격 또는 설명 업데이트 등의 추가 기능을 변경 하기 위해 일반적으로 전체 앱을 업데이트할 필요가 없습니다.

업데이트를 제출 하려면 파트너 센터의 추가 기능 페이지로 이동 하 고 **업데이트**를 클릭 합니다. 이렇게 하면 이전 제출의 정보를 시작 지점으로 사용 하 여 추가 기능에 대 한 새 제출을 만듭니다. 원하는 변경 작업을 수행한 다음 **스토어에 제출**을 클릭 합니다.

이전에 제공 된 추가 기능을 제거 하려는 경우 새 제출을 만들고 **획득 중지** 옵션을 사용 하 여 **저장소에서** [배포 및 표시 유형](set-add-on-pricing-and-availability.md) 옵션을 숨김으로 변경 하 여이 작업을 수행할 수 있습니다. 필요에 따라 응용 프로그램 코드를 업데이트 하 여 추가 기능에 대 한 참조도 제거 해야 합니다. 특히 이전에 게시 된 앱이 이전에 Windows 8.1를 지 원하는 경우에는 해당 고객에 게이 표시 유형 설정이 적용 되지 않습니다.

> [!IMPORTANT]
> 이전에 게시 된 앱을 Windows 8.x의 고객에 게 제공 하는 경우 해당 고객에 게 추가 기능 업데이트를 표시 하기 위해 새 앱 제출을 만들고 게시 해야 합니다. 마찬가지로, 앱이 게시 된 후에 Windows 8.x을 대상으로 하는 앱에 새 추가 기능을 추가 하는 경우 해당 추가 기능을 참조 하도록 앱 코드를 업데이트 한 다음 앱을 다시 제출 해야 합니다. 그렇지 않으면 Windows 8.x의 고객에 게 새 추가 기능이 표시 되지 않습니다.
