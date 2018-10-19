---
author: Xansky
ms.assetid: 9F0A59A1-FAD7-4AD5-B78B-C1280F215D23
description: Microsoft Store 대상 제품 API를 사용하여 앱의 현재 사용자가 사용할 수 있는 대상 제품을 가져옵니다.
title: 스토어 서비스를 사용하여 대상 제품 관리
ms.author: mhopkins
ms.date: 10/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 서비스, Microsoft Store 대상 제품 API, 대상 제품
ms.localizationpriority: medium
ms.openlocfilehash: cb3000e8d791f997267166505a190ddc7c9b4d34
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/19/2018
ms.locfileid: "4959230"
---
# <a name="manage-targeted-offers-using-store-services"></a>스토어 서비스를 사용하여 대상 제품 관리

Windows 개발자 센터 대시보드에서, 앱의 **참여 > 대상 제품** 페이지에서 *대상 제품*을 만들 때 앱 코드에서 *Microsoft Store 대상 제품 API*를 사용하여 대상 제품에 대한 앱 내 환경을 구현하는 데 도움이 되는 정보를 검색합니다. 대상 제품 및 대시보드에서 대상 제품을 만드는 방법에 대한 자세한 내용은 [대상 제품을 사용하여 참여 및 변환 최대화](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)를 참조하세요.

대상 제품 API는 사용자가 대상 제품의 고객 세그먼트에 포함되는지 여부에 따라 현재 사용자에게 제공되는 대상 제품을 가져오는 데 사용할 수 있는 간단한 REST API입니다. 이 API를 앱 코드에 사용하려면 다음 단계를 따릅니다.

1.  현재 앱에 로그인한 사용자의 [Microsoft 계정 토큰을 가져옵니다](#obtain-a-microsoft-account-token).
2.  [현재 사용자의 대상 제품을 가져옵니다](#get-targeted-offers).
3.  대상 제품 중 하나와 연결된 추가 기능에 대한 앱에서 바로 구매 환경을 구현합니다. 앱에서 바로 구매를 구현하는 데 대한 자세한 내용은 [이 문서](enable-in-app-purchases-of-apps-and-add-ons.md)를 참조하세요.

이러한 모든 단계를 보여 주는 완전한 코드 예제는 이 문서의 끝 부분에 있는 [코드 예제](#code-example)를 참조합니다. 다음 섹션에서는 이러한 각 단계에 대한 자세한 내용을 제공합니다.

<span id="obtain-a-microsoft-account-token" />

## <a name="get-a-microsoft-account-token-for-the-current-user"></a>현재 사용자의 Microsoft 계정 토큰 가져오기

앱의 코드에서 현재 로그인한 사용자의 MSA(Microsoft 계정) 토큰을 가져옵니다. Microsoft Store 대상 제품 API의 ```Authorization``` 요청 헤더에 이 토큰을 전달해야 합니다. 이 토큰은 Microsoft Store에서 현재 사용자에게 제공되는 대상 제품을 검색하는 데 사용됩니다.

MSA 토큰을 가져오려면 [WebAuthenticationCoreManager](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core.webauthenticationcoremanager) 클래스를 사용하여 ```devcenter_implicit.basic,wl.basic``` 범위를 사용하는 토큰을 요청합니다. 다음 예에서는 이 작업을 수행하는 방법을 보여 줍니다. [완전한 예제](#code-example)에서 발췌한 예제입니다. 완전한 예제에서 제공되는 **using** 문이 필요합니다.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetMSAToken)]

MSA 토큰을 가져오는 방법에 대한 자세한 내용은 [웹 계정 관리자](../security/web-account-manager.md)를 참조하세요.

<span id="get-targeted-offers" />

## <a name="get-the-targeted-offers-for-the-current-user"></a>현재 사용자의 대상 제품 가져오기

현재 사용자의 MSA 토큰을 확보한 후에는 ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` URI의 GET 메서드를 호출하여 현재 사용자에게 제공되는 대상 제품을 가져옵니다. 이 REST 메서드에 대한 자세한 내용은 [대상 제품 가져오기](get-targeted-offers.md)를 참조하세요.

이 메서드는 현재 사용자에게 제공되는 대상 제품과 연결된 추가 기능의 제품 ID를 반환합니다. 이 정보를 사용하여 사용자에게 하나 이상의 대상 제품을 앱에서 바로 구매로 제공할 수 있습니다.

다음 예제는 현재 사용자에게 대상 제품을 제시하는 방법을 보여 줍니다. 이 예제는 [완전한 예제](#code-example)에서 발췌했습니다. Newtonsoft의 [Json.NET](http://www.newtonsoft.com/json) 라이브러리와 추가 클래스, 완전한 예제에서 제공되는 **using** 문이 필요합니다.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffers)]

<span id="code-example" />

## <a name="complete-code-example"></a>전체 코드 예제

다음 코드 예제는 다음과 같은 작업을 보여 줍니다.

* 현재 사용자의 MSA 토큰을 가져옵니다.
* [Get targeted offers](get-targeted-offers.md) 메서드를 사용하여 현재 사용자에게 제공되는 대상 제품을 모두 가져옵니다.
* 대상 제품과 연결된 추가 기능을 구매합니다.

이 예제는 Newtonsoft의 [Json.NET](http://www.newtonsoft.com/json) 라이브러리가 필요합니다. 이 예에서는 이 라이브러리를 사용하여 JSON 형식의 데이터를 직렬화 및 역직렬화합니다.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffersSample)]

## <a name="related-topics"></a>관련 항목

* [대상 제품을 사용하여 참여 및 변환 최대화](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)
* [대상 제품 가져오기](get-targeted-offers.md)
