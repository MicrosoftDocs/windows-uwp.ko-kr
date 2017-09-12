---
author: jnHs
Description: "추가 기능을 제출할 때 속성 페이지의 옵션을 통해 고객에게 제공되었을 때 나타나는 추가 기능의 동작을 결정할 수 있습니다."
title: "추가 기능 속성 입력"
ms.assetid: 26D2139F-66FD-479E-940B-7491238ADCAE
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 253e008d3622094dcfe765531d71e5f37b7777b0
ms.sourcegitcommit: de6bc8acec2cd5ebc36bb21b2ce1a9980c3e78b2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/17/2017
---
# <a name="enter-add-on-properties"></a>추가 기능 속성 입력


추가 기능을 제출할 때 **속성** 페이지의 옵션을 통해 고객에게 제공되었을 때 나타나는 추가 기능의 동작을 결정할 수 있습니다.

## <a name="product-type"></a>제품 유형

제품 유형은 처음 [추가 기능을 만들 때](set-your-add-on-product-id.md) 선택됩니다. 선택한 제품 유형이 여기에 표시되지만 제품 유형을 변경할 수는 없습니다.

> [!TIP]
> 추가 기능을 게시하지 않은 경우, 다른 제품 유형을 선택하고 싶을 때는 해당 제출을 삭제하고 다시 시작할 수 있습니다.

이 페이지에 표시되는 필드는 추가 기능의 제품 유형에 따라 달라집니다.

## <a name="product-lifetime"></a>제품 수명


제품 유형으로 **지속형**을 선택한 경우 **제품 수명**이 여기에 표시됩니다. 지속형 추가 기능의 기본 **제품 수명**은 **계속**으로, 추가 기능이 만료되지 않음을 나타냅니다. 원하는 경우 1~365일 옵션으로 정해진 기간 후에 추가 기능이 만료되도록 **제품 수명**을 설정할 수 있습니다.

## <a name="quantity"></a>수량


**스토어 관리 소모성** 제품 유형을 선택한 경우 **수량**이 여기에 표시됩니다. 1과 1000000 사이의 숫자를 입력해야 합니다. 이 수량은 추가 기능을 구입할 때 고객에게 부여되고 스토어는 고객의 추가 기능 소비를 보고하는 앱으로 잔액을 추적합니다.


## <a name="subscription-period"></a>구독 기간

제품 유형으로 **구독**을 선택한 경우 **구독 기간**이 여기에 표시됩니다. 사용 가능한 옵션 중 하나(**월별**, **3개월**, **6개월**, **매년**, **24개월**)를 선택하여 고객에게 구독 요금이 얼마나 자주 청구되는지 표시해야 합니다. 추가 기능을 게시한 후에는 선택한 **구독 기간**을 변경할 수 없습니다.

> [!NOTE]
> 구독 추가 기능은 현재 초기 채택 프로그램에 참여하고 있는 개발자 계정에서만 사용할 수 있습니다. 향후 모든 개발자 계정에서 구독 추가 기능을 사용할 수 있도록 만들 계획입니다. 우리는 개발자들에게 기능 미리 보기를 제공하기 위해 이 예비 자료를 제공하고 있습니다. 자세한 정보는 [앱에 구독 추가 기능을 사용하도록 설정](../monetize/enable-subscription-add-ons-for-your-app.md)을 참조하세요.


## <a name="free-trial"></a>무료 평가판

구독 추가 기능에 대한 **무료 평가판**이 여기 표시됩니다. 설정된 기간(**1주** 또는 **1개월**) 동안 고객이 추가 기능을 무료로 사용할 수 있도록 할 것인지, 또는 **무료 평가판 없음**을 제안할 것인지 선택해야 합니다. 추가 기능을 게시한 후에는 선택한 **무료 평가판**을 변경할 수 없습니다.


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

이 필드를 쿼리하려면 [Windows.Services.Store 네임스페이스](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.aspx)의 [StoreProduct.Keywords](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_Keywords) 속성을 사용합니다. (또는 [Windows.ApplicationModel.Store 네임스페이스](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.aspx)를 사용하는 경우 [ProductListing.Keywords](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting#Windows_ApplicationModel_Store_ProductListing_Keywords) 속성을 사용합니다.)

> [!NOTE]
> Windows 8 및 Windows 8.1을 대상으로 하는 패키지에서는 키워드를 사용할 수 없습니다.

<span id="custom-developer-data" />
### <a name="custom-developer-data"></a>사용자 지정 개발자 데이터

**사용자 지정 개발자 데이터** 필드(이전의 **태그**)에 최대 3000자까지 입력하여 앱에서 바로 구매 제품에 대한 추가 컨텍스트를 제공할 수 있습니다. 대부분의 경우 이는 XML 문자열 형식이지만, 이 필드에 원하는 모든 내용을 입력할 수 있습니다. 앱이 이 필드를 쿼리하여 콘텐츠를 읽을 수 있습니다(단, 앱은 데이터를 편집하고 변경 사항을 적용할 수 없음).

예를 들어 게임을 보유하고 있고 고객이 추가 레벨에 액세스할 수 있도록 추가 기능을 판매하고 있다고 가정해 보겠습니다. 앱은 **사용자 정의 개발자 데이터** 필드를 사용하여 고객이 이러한 추가 기능을 소유하고 있을 때 쿼리를 통해 어떤 레벨을 사용할 수 있는지 확인할 수 있습니다. 앱에서 코드를 변경하거나 앱을 다시 제출하지 않고도 추가 기능의 **사용자 지정 개발자 데이터** 필드에서 정보를 업데이트한 후 업데이트된 제출을 추가 기능에 게시하여 언제든지 값을 조정할 수 있습니다(이 경우 레벨이 포함).

이 필드를 쿼리하려면 [Windows.Services.Store 네임스페이스](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.aspx)의 [StoreSku.CustomDeveloperData](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.storesku.customdeveloperdata.aspx) 속성을 사용합니다. (또는 [Windows.ApplicationModel.Store 네임스페이스](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.aspx)를 사용하는 경우 [ProductListing.Tag](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.productlisting.tag.aspx) 속성을 사용합니다.)

> [!NOTE]
> Windows 8 및 Windows 8.1을 대상으로 하는 패키지에서는 **사용자 지정 개발자 데이터** 필드를 사용할 수 없습니다.

 

 

 
