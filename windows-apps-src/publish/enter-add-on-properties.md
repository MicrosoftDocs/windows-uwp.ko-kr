---
Description: 추가 기능을 제출할 때 속성 페이지의 옵션을 통해 고객에 게 제공 되는 추가 기능에 대 한 동작을 결정할 수 있습니다.
title: 추가 기능 속성 입력
ms.assetid: 26D2139F-66FD-479E-940B-7491238ADCAE
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 추가 기능, 속성, 구독 기간, 제품 수명, 콘텐츠 형식, iap, 앱 내 구매, 앱 내 제품
ms.localizationpriority: medium
ms.openlocfilehash: 9d092f443ab643b74cdd0221c96540fed0c7d474
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157998"
---
# <a name="enter-add-on-properties"></a>추가 기능 속성 입력

추가 기능을 제출할 때 **속성** 페이지의 옵션을 통해 고객에 게 제공 되는 추가 기능에 대 한 동작을 결정할 수 있습니다.

## <a name="product-type"></a>제품 유형

[추가 기능](set-your-add-on-product-id.md)을 처음 만들 때 제품 유형이 선택 됩니다. 선택한 제품 유형이 여기에 표시 되지만 변경할 수는 없습니다.

> [!TIP]
> 추가 기능을 게시 하지 않은 경우 다른 제품 유형을 선택 하려면 제출을 삭제 하 고 다시 시작할 수 있습니다.

이 페이지에 표시 되는 필드는 추가 기능 제품 유형에 따라 달라 집니다.


## <a name="product-lifetime"></a>제품 수명

제품 유형에 대해 지 **속성** 을 선택한 경우 **제품 수명은** 여기에 표시 됩니다. 내구성이 있는 추가 기능에 대 한 기본 **제품 수명은** **무기한**이며,이는 추가 기능이 만료 되지 않음을 의미 합니다. 원하는 경우 추가 기능이 설정 된 기간 (1-365 일의 옵션 사용) 후에 만료 되도록 **제품 수명을** 변경할 수 있습니다.


## <a name="quantity"></a>수량

제품 유형에 대해 **스토어 관리 기능** 을 선택한 경우 **수량이** 여기에 표시 됩니다. 1에서 100만 사이의 숫자를 입력 해야 합니다. 이 수량은 추가 기능을 획득할 때 고객에 게 부여 되며, 스토어는 앱이 고객의 추가 기능 소비를 보고할 때 잔액을 추적 합니다.


## <a name="subscription-period"></a>구독 기간

제품 유형에 대 한 **구독** 을 선택한 경우 **구독 기간이** 여기에 표시 됩니다. 고객에 게 구독에 대 한 요금이 부과 되는 빈도를 지정 하는 옵션을 선택 합니다. 기본 옵션은 **매월**이지만 **3 개월**, **6 개월**, **매년**또는 **24 개월**을 선택할 수도 있습니다.

> [!IMPORTANT]
> 추가 기능이 게시 된 후에는 **구독 기간** 선택을 변경할 수 없습니다.


## <a name="free-trial"></a>평가판

제품 유형에 대 한 **구독** 을 선택한 경우 **무료 평가판** 도 여기에 표시 됩니다. 기본 옵션은 **무료 평가판** 이 아닙니다. 선호 하는 경우 설정 된 시간 ( **1 주** 또는 **1 개월**) 동안 추가 기능을 무료로 사용할 수 있습니다. 

> [!IMPORTANT]
> 추가 기능이 게시 된 후에는 **무료 평가판** 선택 항목을 변경할 수 없습니다.


## <a name="content-type"></a>내용 유형

추가 기능의 제품 형식에 관계 없이, 제공 하는 콘텐츠 형식을 지정 해야 합니다. 대부분의 추가 기능에 대해 콘텐츠 형식은 **전자적 소프트웨어 다운로드**여야 합니다. 목록에서 다른 옵션을 통해 추가 기능을 더 잘 설명 하는 경우 (예: 음악 다운로드 나 전자를 제공 하는 경우) 대신 해당 옵션을 선택 합니다.

다음은 추가 기능의 콘텐츠 형식에 사용할 수 있는 옵션입니다.

-   전자 소프트웨어 다운로드
-   전자책
-   전자 잡지 단일 문제
-   Electronic 신문 단일 문제
-   음악 다운로드
-   음악 스트리밍
-   온라인 데이터 저장소/서비스
-   SaaS(Software as a Service)
-   비디오 다운로드
-   비디오 스트리밍


## <a name="additional-properties"></a>추가 속성

이러한 필드는 모든 유형의 추가 기능에 대 한 선택 사항입니다.

<span id="keywords" />

### <a name="keywords"></a>키워드

제출 하는 각 추가 기능에 대해 최대 30 자까지 최대 10 개의 키워드를 제공 하는 옵션이 있습니다. 그러면 앱이 이러한 단어와 일치 하는 추가 기능을 쿼리할 수 있습니다. 이 기능을 사용 하면 앱 코드에서 제품 ID를 직접 지정 하지 않고도 추가 기능을 로드할 수 있는 화면을 응용 프로그램에 빌드할 수 있습니다. 앱에서 코드를 변경 하거나 앱을 다시 제출 하지 않고도 언제 든 지 추가 기능의 키워드를 변경할 수 있습니다.

이 필드를 쿼리하려면 [Windows.. Store 네임 스페이스](/uwp/api/Windows.Services.Store) [의.](/uwp/api/windows.services.store.storeproduct.Keywords) 또는 [Windows](/uwp/api/Windows.ApplicationModel.Store)를 사용 하는 경우에는 [제품 목록 키워드인](/uwp/api/windows.applicationmodel.store.productlisting.Keywords) 속성을 사용 합니다.

> [!NOTE]
> Windows 8 및 Windows 8.1를 대상으로 하는 패키지에는 키워드를 사용할 수 없습니다.

<span id="custom-developer-data" />

### <a name="custom-developer-data"></a>사용자 지정 개발자 데이터

**사용자 지정 개발자 데이터** 필드 (이전에는 **태그**)에 최대 3000 개의 문자를 입력 하 여 앱 내 제품에 대 한 추가 컨텍스트를 제공할 수 있습니다. 대부분은 XML 문자열 형식 이지만이 필드에 원하는 모든 항목을 입력할 수 있습니다. 그러면 앱에서 해당 콘텐츠를 읽을 수 있도록이 필드를 쿼리할 수 있습니다. 단, 앱은 데이터를 편집할 수 없으며 변경 내용을 다시 전달할 수 없습니다.

예를 들어, 게임이 있고 고객이 추가 수준에 액세스할 수 있도록 하는 추가 기능을 판매 하 고 있다고 가정해 보겠습니다. **사용자 지정 개발자 데이터** 필드를 사용 하 여이 추가 기능을 소유 하는 고객이 사용할 수 있는 수준을 확인 하기 위해 앱에서 쿼리할 수 있습니다. 언제 든 지 (이 경우 포함 된 수준) 값을 조정 하 여 앱에서 코드를 변경 하거나 앱을 다시 제출할 필요 없이 추가 기능의 **사용자 지정 개발자 데이터** 필드에서 정보를 업데이트 하 고 추가 기능에 대 한 업데이트 된 제출을 게시할 수 있습니다.

이 필드를 쿼리하려면 StoreSku [네임 스페이스](/uwp/api/Windows.Services.Store)의 [CustomDeveloperData](/uwp/api/windows.services.store.storesku.customdeveloperdata#Windows_Services_Store_StoreSku_CustomDeveloperData) 속성을 사용 합니다. 또는 [Windows](/uwp/api/Windows.ApplicationModel.Store)를 사용 하는 경우에는 [제품 목록 태그](/uwp/api/windows.applicationmodel.store.productlisting.tag#Windows_ApplicationModel_Store_ProductListing_Tag) 속성을 사용 합니다.

> [!NOTE]
> **사용자 지정 개발자 데이터** 필드는 Windows 8 및 Windows 8.1를 대상으로 하는 패키지에서 사용할 수 없습니다.

 

 

 