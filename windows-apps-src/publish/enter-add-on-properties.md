---
author: jnHs
Description: When submitting an add-on, the options on the Properties page help determine the behavior of your add-on when offered to customers.
title: 추가 기능 속성 입력
ms.assetid: 26D2139F-66FD-479E-940B-7491238ADCAE
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, 추가 기능, 속성, 구독 기간, 제품 수명, 콘텐츠 유형, iap, 앱에서 바로 구매, 앱에서 바로 구매 제품
ms.localizationpriority: medium
ms.openlocfilehash: fa0559c79b758373347427c0aa88b351c0fbddf0
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6157319"
---
# <a name="enter-add-on-properties"></a>추가 기능 속성 입력

추가 기능을 제출할 때 **속성** 페이지의 옵션을 통해 고객에게 제공되었을 때 나타나는 추가 기능의 동작을 결정할 수 있습니다.

## <a name="product-type"></a>제품 유형

제품 유형은 처음 [추가 기능을 만들 때](set-your-add-on-product-id.md) 선택됩니다. 선택한 제품 유형이 여기에 표시되지만 제품 유형을 변경할 수는 없습니다.

> [!TIP]
> 추가 기능을 게시하지 않은 경우, 다른 제품 유형을 선택하고 싶을 때는 해당 제출을 삭제하고 다시 시작할 수 있습니다.

이 페이지에 표시되는 필드는 추가 기능의 제품 유형에 따라 달라집니다.


## <a name="product-lifetime"></a>제품 수명

제품 유형으로 **지속형**을 선택한 경우 **제품 수명**이 여기에 표시됩니다. 지속형 추가 기능의 기본 **제품 수명**은 **계속**으로, 추가 기능이 만료되지 않음을 나타냅니다. 원하는 경우, 추가 기능 (1 ~ 365 일 옵션)으로 설정된 된 지속 시간 후 만료 되도록 **제품 수명** 변경할 수 있습니다.


## <a name="quantity"></a>수량

**스토어 관리 소모성** 제품 유형을 선택한 경우 **수량**이 여기에 표시됩니다. 1과 1000000 사이의 숫자를 입력해야 합니다. 이 수량은 추가 기능을 구입할 때 고객에게 부여되고 스토어는 고객의 추가 기능 소비를 보고하는 앱으로 잔액을 추적합니다.


## <a name="subscription-period"></a>구독 기간

제품 유형으로 **구독**을 선택한 경우 **구독 기간**이 여기에 표시됩니다. 구독에 대해 고객에게 얼마나 자주 요금을 청구할지 지정하는 옵션을 선택합니다. 기본 옵션은 **월별**, 하지만 **3 개월**, **6 개월**, **연간**또는 **24 개월**선택할 수도 있습니다.

> [!IMPORTANT]
> 추가 기능을 게시한 후에는 선택한 **구독 기간**을 변경할 수 없습니다.


## <a name="free-trial"></a>무료 평가판

제품 유형으로 **구독**을 선택한 경우 **무료 평가판** 또한 여기에 표시됩니다. 기본 옵션은 **무료 평가판 없음**입니다. 원하는 경우 설정된 시간 동안 고객이 무료로 추가 기능을 사용하도록 설정할 수 있습니다(**1주** 또는 **1개월**). 

> [!IMPORTANT]
> 추가 기능을 게시한 후에는 선택한 **무료 평가판**을 변경할 수 없습니다.


## <a name="content-type"></a>콘텐츠 유형

추가 기능의 제품 유형과 관계없이 제공할 콘텐츠 유형도 표시해야 합니다. 대부분의 추가 기능에 대한 콘텐츠 유형은 **전자 소프트웨어 다운로드**여야 합니다. 목록의 다른 옵션이 추가 기능을 더 잘 설명하는 경우(예를 들어 음악 다운로드 또는 전자책을 제공하는 경우) 해당 옵션을 대신 선택합니다.

추가 기능의 콘텐츠 형식에 대한 가능한 옵션은 다음과 같습니다.

-   전자 소프트웨어 다운로드
-   전자책
-   전자 잡지 단행본
-   전자 신문 단행본
-   음악 다운로드
-   음악 스트리밍
-   온라인 데이터 저장소/서비스
-   SaaS(Software as a Service)
-   동영상 다운로드
-   동영상 스트리밍


## <a name="additional-properties"></a>추가 속성

이러한 필드는 모든 유형의 추가 기능에서 옵션으로 제공됩니다.

<span id="keywords" />

### <a name="keywords"></a>키워드

제출하는 각 추가 기능에 대해 각각 30자 이내의 키워드를 10개까지 제공할 수 있습니다. 그러면 앱에서 이러한 단어와 일치하는 추가 기능을 쿼리할 수 있습니다. 이 기능을 사용하면 앱 코드에서 제품 ID를 직접 지정하지 않고도 추가 기능을 로드할 수 있는 앱 화면을 만들 수 있습니다. 그러면 앱에서 코드를 변경하거나 앱을 다시 제출하지 않고도 언제든지 추가 기능의 키워드를 변경할 수 있습니다.

이 필드를 쿼리하려면 [Windows.Services.Store 네임스페이스](https://docs.microsoft.com/uwp/api/Windows.Services.Store)의 [StoreProduct.Keywords](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.Keywords) 속성을 사용합니다. (또는 [Windows.ApplicationModel.Store 네임스페이스](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store)를 사용하는 경우 [ProductListing.Keywords](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting.Keywords) 속성을 사용합니다.)

> [!NOTE]
> 키워드는 Windows8 및 Windows8.1 대상으로 하는 패키지를 사용할 수 없습니다.

<span id="custom-developer-data" />

### <a name="custom-developer-data"></a>사용자 지정 개발자 데이터

**사용자 지정 개발자 데이터** 필드(이전의 **태그**)에 최대 3000자까지 입력하여 앱에서 바로 구매 제품에 대한 추가 컨텍스트를 제공할 수 있습니다. 대부분의 경우 이는 XML 문자열 형식이지만, 이 필드에 원하는 모든 내용을 입력할 수 있습니다. 앱이 이 필드를 쿼리하여 콘텐츠를 읽을 수 있습니다(단, 앱은 데이터를 편집하고 변경 사항을 적용할 수 없음).

예를 들어 게임을 보유하고 있고 고객이 추가 레벨에 액세스할 수 있도록 추가 기능을 판매하고 있다고 가정해 보겠습니다. 앱은 **사용자 정의 개발자 데이터** 필드를 사용하여 고객이 이러한 추가 기능을 소유하고 있을 때 쿼리를 통해 어떤 레벨을 사용할 수 있는지 확인할 수 있습니다. 앱에서 코드를 변경하거나 앱을 다시 제출하지 않고도 추가 기능의 **사용자 지정 개발자 데이터** 필드에서 정보를 업데이트한 후 업데이트된 제출을 추가 기능에 게시하여 언제든지 값을 조정할 수 있습니다(이 경우 레벨이 포함).

이 필드를 쿼리하려면 [Windows.Services.Store 네임스페이스](https://docs.microsoft.com/uwp/api/Windows.Services.Store)의 [StoreSku.CustomDeveloperData](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.customdeveloperdata#Windows_Services_Store_StoreSku_CustomDeveloperData) 속성을 사용합니다. (또는 [Windows.ApplicationModel.Store 네임스페이스](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store)를 사용하는 경우 [ProductListing.Tag](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting.tag#Windows_ApplicationModel_Store_ProductListing_Tag) 속성을 사용합니다.)

> [!NOTE]
> **사용자 지정 개발자 데이터** 필드가 Windows8 및 Windows8.1 대상으로 하는 패키지를 사용할 수 있습니다.

 

 

 
