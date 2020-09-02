---
ms.assetid: 9F0A59A1-FAD7-4AD5-B78B-C1280F215D23
description: Microsoft Store 대상 제공 API를 사용 하 여 앱의 현재 사용자에 게 제공 되는 대상 제공 서비스를 가져옵니다.
title: 매장 서비스를 사용 하 여 대상 제품 관리
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 스토어 서비스 Microsoft Store 대상 제공 API, 대상 제안
ms.localizationpriority: medium
ms.openlocfilehash: 836ef99f827eba52699663d4a24ea58598fe3400
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363846"
---
# <a name="manage-targeted-offers-using-store-services"></a>매장 서비스를 사용 하 여 대상 제품 관리

파트너 센터에서 앱에 대 한 **> 대상 제공 서비스** 페이지에서 *대상 제품* 을 만드는 경우 앱 코드의 *Microsoft Store 대상 제품 API* 를 사용 하 여 대상 제품의 앱 내 환경을 구현 하는 데 도움이 되는 정보를 검색 합니다. 대상 제공 서비스 및 대시보드에서 이러한 항목을 만드는 방법에 대 한 자세한 내용은 [대상 제공 서비스를 사용 하 여 참여 및 변환 최대화](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)를 참조 하세요.

대상 제공 API는 사용자가 대상 제품에 대 한 고객 세그먼트의 일부 인지 여부에 따라 현재 사용자에 게 제공 되는 대상 제공 서비스를 가져오는 데 사용할 수 있는 간단한 REST API입니다. 앱의 코드에서이 API를 사용 하려면 다음 단계를 수행 합니다.

1.  앱의 현재 로그인 한 사용자에 대 한 [Microsoft 계정 토큰을 가져옵니다](#obtain-a-microsoft-account-token) .
2.  [현재 사용자의 대상 제공 서비스를 가져옵니다](#get-targeted-offers).
3.  대상 제품 중 하 나와 연결 된 추가 기능에 대 한 앱 내 구매 환경을 구현 합니다. 앱에서 바로 구매를 구현 하는 방법에 대 한 자세한 내용은 [이 문서](enable-in-app-purchases-of-apps-and-add-ons.md)를 참조 하세요.

이러한 모든 단계를 보여 주는 전체 코드 예제는이 문서의 끝에 있는 [코드 예제](#code-example) 를 참조 하세요. 다음 섹션에서는 각 단계에 대 한 자세한 정보를 제공 합니다.

<span id="obtain-a-microsoft-account-token" />

## <a name="get-a-microsoft-account-token-for-the-current-user"></a>현재 사용자에 대 한 Microsoft 계정 토큰을 가져옵니다.

앱의 코드에서 현재 로그인 한 사용자에 대 한 MSA (Microsoft 계정) 토큰을 가져옵니다. ```Authorization```Microsoft Store 대상 제공 API에 대 한 요청 헤더에이 토큰을 전달 해야 합니다. 이 토큰은 저장소에서 현재 사용자가 사용할 수 있는 대상 제공 서비스를 검색 하는 데 사용 됩니다.

MSA 토큰을 가져오려면 [Webauthenticationcoremanager](/uwp/api/windows.security.authentication.web.core.webauthenticationcoremanager) 클래스를 사용 하 여 범위를 사용 하 여 토큰을 요청 ```devcenter_implicit.basic,wl.basic``` 합니다. 다음 예제에서는 이 작업을 수행하는 방법을 보여 줍니다. 이 예는 [전체 예제](#code-example)에서 발췌 한 것 이며 전체 예제에서 제공 되는 문을 **사용** 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs" id="GetMSAToken":::

MSA 토큰을 가져오는 방법에 대 한 자세한 내용은 [웹 계정 관리자](../security/web-account-manager.md)를 참조 하세요.

<span id="get-targeted-offers" />

## <a name="get-the-targeted-offers-for-the-current-user"></a>현재 사용자에 대 한 대상 제안 가져오기

현재 사용자에 대 한 MSA 토큰을 가져온 후에는 URI의 GET 메서드를 호출 ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` 하 여 현재 사용자에 대해 사용 가능한 대상 제공 서비스를 가져옵니다. 이 REST 방법에 대 한 자세한 내용은 [대상 제안 가져오기](get-targeted-offers.md)를 참조 하세요.

이 메서드는 현재 사용자에 게 제공 되는 대상 제공 서비스와 관련 된 추가 기능의 제품 Id를 반환 합니다. 이 정보를 사용 하 여 사용자에 대 한 앱 내 구매로 하나 이상의 대상 제품을 제공할 수 있습니다.

다음 예에서는 현재 사용자의 대상 제공 서비스를 가져오는 방법을 보여 줍니다. 이 예제는 [전체 예제](#code-example)에서 발췌 한 것입니다. Newtonsoft.json의 [Json.NET](https://www.newtonsoft.com/json) 라이브러리와 추가 클래스 및 전체 예제에서 제공 되는 문을 **사용** 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs" id="GetTargetedOffers":::

<span id="code-example" />

## <a name="complete-code-example"></a>전체 코드 예제

다음 코드 예제에서는 다음 작업을 보여 줍니다.

* 현재 사용자에 대 한 MSA 토큰을 가져옵니다.
* [Get 대상 제품](get-targeted-offers.md) 메서드를 사용 하 여 현재 사용자에 대 한 대상 제품을 모두 가져옵니다.
* 대상 제품에 연결 된 추가 기능을 구입 합니다.

이 예제에는 Newtonsoft.json의 [Json.NET](https://www.newtonsoft.com/json) 라이브러리가 필요 합니다. 이 예제에서는이 라이브러리를 사용 하 여 JSON 형식 데이터를 serialize 및 deserialize 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs" id="GetTargetedOffersSample":::

## <a name="related-topics"></a>관련 항목

* [대상 제공 서비스를 사용 하 여 참여 및 변환 최대화](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)
* [대상 제품 가져오기](get-targeted-offers.md)
