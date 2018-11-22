---
author: jnHs
Description: Add-ons (or in-app products) are published through Partner Center.
title: 추가 기능 제출
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, iap, 앱에서 바로 구매, 앱에서 바로 구매 제품, iap 제출
ms.localizationpriority: medium
ms.openlocfilehash: 28fd2e104de12cc297ce5d28ddd18b0ce550a5d0
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7563884"
---
# <a name="add-on-submissions"></a>추가 기능 제출

추가 기능(앱에서 바로 구매 제품이라고도 함)은 고객이 구매할 수 있는 앱에 대한 보충 항목입니다. 추가 기능 재미 있는 새로운 기능을 생각 하는 새 게임 레벨 또는 기타 모든 항목에 참여 하는 사용자가 유지 될 수 있습니다. 추가 기능은 수익을 창출하는 뛰어난 방법일 뿐 아니라 고객 조작 및 참여를 추진하는 데 도움이 됩니다.

추가 기능에는 [파트너 센터](https://partner.microsoft.com/dashboard)를 통해 게시 및 활성 [개발자 계정이](http://go.microsoft.com/fwlink/p/?LinkId=615100)있어야 합니다. 또한 앱 코드에서 [추가 기능을 사용](../monetize/in-app-purchases-and-trials.md)해야 합니다.

추가 기능 제출 프로세스의 첫 번째 단계 [제품 유형과 제품 ID를 정의](set-your-add-on-product-id.md)하 여 파트너 센터에서 추가 기능을 만드는 것입니다. 그 후 Microsoft Store를 통해 추가 기능을 구매할 수 있도록 제출을 만들 수 있습니다. [앱을 제출](app-submissions.md)할 때 추가 기능을 함께 제출하거나 개별적으로 작업할 수 있습니다. 또한 앱을 다시 제출할 필요 없이 앱이 스토어에 나열된 후에 추가 기능을 [업데이트](#updating-an-add-on-after-publication)할 수 있습니다.

> [!NOTE]
> 설명서의이 섹션에는 파트너 센터에서 추가 기능을 제출 하는 방법을 설명 합니다. 또는 [Microsoft Store 제출 API](../monetize/create-and-manage-submissions-using-windows-store-services.md)를 사용하여 추가 기능 제출을 자동화할 수 있습니다.


## <a name="checklist-for-submitting-an-add-on"></a>추가 기능 제출에 대한 검사 목록

추가 기능 제출을 만들 때 제공하는 정보 목록은 다음과 같습니다. 제공해야 하는 항목은 다음과 같습니다. 일부 항목은 선택 사항이거나 원하는 대로 변경할 수 있는 기본값이 이미 있습니다.


### <a name="create-a-new-add-on-page"></a>새 추가 기능 페이지 만들기

| 필드 이름                    | 참고                            |
|-------------------------------|----------------------------------|
| [**제품 유형**](set-your-add-on-product-id.md#product-type)      | 필수 |  
| [**제품 ID**](set-your-add-on-product-id.md#product-id)          | 필수 |        


### <a name="properties-page"></a>속성 페이지

| 필드 이름                    | 참고                              |   
|-------------------------------|------------------------------------|
| [**제품 수명**](enter-add-on-properties.md#product-lifetime)  | 제품 유형이 **지속형**인 경우 필수입니다. 기타 제품 형식에는 적용되지 않습니다. |
| [**수량**](enter-add-on-properties.md#quantity)  | 제품 형식이 **스토어 관리 소모성**인 경우 필요합니다. 기타 제품 형식에는 적용되지 않습니다. |
| [**구독 기간**](enter-add-on-properties.md#subscription-period)          | 제품 유형이 **구독**인 경우 필수입니다. 기타 제품 형식에는 적용되지 않습니다.       |  
| [**무료 평가판**](enter-add-on-properties.md#free-trial)          | 제품 유형이 **구독**인 경우 필수입니다. 기타 제품 형식에는 적용되지 않습니다.       |
| [**콘텐츠 형식**](enter-add-on-properties.md#content-type)          | 필수    |               
| [**키워드**](enter-add-on-properties.md#keywords)                  | 선택 사항(키워드 최대 10개, 각각 30자 제한) |
| [**사용자 지정 개발자 데이터**](enter-add-on-properties.md#custom-developer-data)   | 선택 사항(3,000자 제한)            |


### <a name="pricing-and-availability-page"></a>가격 책정 및 가용성 페이지

| 필드 이름                    | 참고                                       |
|-------------------------------|---------------------------------------------|
| [**시장**](set-add-on-pricing-and-availability.md#markets)  | 기본값: 모든 가능한 시장 |
| [**표시 여부**](set-add-on-pricing-and-availability.md#visibility)   | 기본값: 구매 가능 앱 목록에 표시될 수 있음 |
| [**일정**](set-add-on-pricing-and-availability.md#schedule)    | 기본값: 최대한 빨리 게시
| [**가격 책정**](set-add-on-pricing-and-availability.md#pricing)                | 필수                                    |
| [**할인 가격 책정**](put-apps-and-add-ons-on-sale.md)               | 선택 사항                    |


### <a name="store-listings"></a>Store 목록

하나의 Store 목록이 필요합니다. 앱에서 지원하는 모든 [언어](create-add-on-store-listings.md#store-listing-languages)에 대한 Store 목록을 제공하는 것이 좋습니다.

| 필드 이름                    | 참고                                       |
|-------------------------------|---------------------------------------------|
| [**제목**](create-add-on-store-listings.md#title)                    | 필수(100자 제한)           |
| [**설명**](create-add-on-store-listings.md#description)       | 선택 사항(200자 제한)            |
| [**아이콘**](create-add-on-store-listings.md#icon)                    | 선택 사항(.png, 300 x 300픽셀)            |


이 정보를 모두 입력했으면 **스토어에 제출**을 클릭합니다. 대부분은 인증 프로세스에 1시간 정도 걸립니다. 그 후에 추가 기능이 스토어에 게시되고 고객이 구매할 준비가 됩니다.

> [!NOTE]
> 또한 추가 기능이 앱 코드 내에서 구현되어야 합니다. 자세한 내용은 [앱에서 바로 구매 및 평가판](../monetize/in-app-purchases-and-trials.md)을 참조하세요.


## <a name="updating-an-add-on-after-publication"></a>게시 후 추가 기능 업데이트

언제든지 게시된 추가 기능을 변경할 수 있습니다. 추가 기능 변경 내용은 제출 되며 일반적으로 가격이 나 설명 업데이트 같은 추가 기능 변경을 위해 전체 앱을 업데이트할 필요가 없는 앱에 개별적으로 게시 됩니다.

업데이트를 제출 하려면 파트너 센터에 대 한 추가 기능 페이지로 이동 하 고 **업데이트**를 클릭 합니다. 이렇게 하면 이전 제출의 정보를 사용 하 여 시작 지점으로 추가 기능에 대 한 새 제출을 만들어집니다. 원하는 것을 누른 다음 **스토어에 제출**변경 내용을 확인 합니다.

이전에 제공한 추가 기능을 제거하려면 새 제출을 만들고 [배포 및 표시](set-add-on-pricing-and-availability.md) 옵션을 **Store에서 숨김** 및 **취득 중지** 옵션으로 변경하면 됩니다. 추가 기능에 대 한 참조도 제거 하려면 필요에 따라 앱의 코드를 업데이트 해야 (이전에 게시 된 앱이 이전 버전 Windows 8.1 지원 하는 경우에 특히이 표시 설정이 적용 되지 않음 해당 고객에 게).

> [!IMPORTANT]
> 이전에 게시 된 앱은 Windows에서 고객에 게 사용할 수 있는 경우 8.x를 만들고 추가 기능 업데이트가 해당 고객에 게 표시 되 게 하려면 새 앱 제출을 게시 해야 합니다. 마찬가지로 앱이 게시된 후 Windows8.x가 대상으로 지정된 앱에 새 추가 기능을 추가할 경우 해당 추가 기능을 참조하도록 앱 코드를 업데이트하고 앱을 다시 제출해야 합니다. 그러지 않으면 새 추가 기능이 Windows8.x의 고객에게 표시되지 않습니다.
