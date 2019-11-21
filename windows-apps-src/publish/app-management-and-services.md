---
Description: Manage and view details related to each of your apps in Partner Center, and configure services such as A/B testing and maps.
title: 앱 관리 및 서비스
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.date: 03/21/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 30610cdacbd9d2be10205958688376371f0387f6
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260035"
---
# <a name="app-management-and-services"></a>앱 관리 및 서비스

You can manage and view details related to each of your apps in [Partner Center](https://partner.microsoft.com/dashboard), and configure services such as notifications, A/B testing, and maps.

When working with an app in Partner Center, you'll see sections in the left navigation menu for **Services** and **App management**. 이들 섹션을 확장하여 아래 설명된 기능에 액세스할 수 있습니다.

## <a name="services"></a>서비스

**서비스** 섹션을 통해 앱에 대한 다양한 서비스를 관리할 수 있습니다.

## <a name="xbox-live"></a>Xbox Live

If you are publishing a game, you can enable the [Xbox Live Creators Program](https://www.xbox.com/developers/creators-program) on this page. This lets you start configuring and testing Xbox Live features, and eventually publish your Xbox Live Creators Program game.

For more info, see [Get started with the Xbox Live Creators Program](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators) and [Create a new Xbox Live Creators Program title and publish to the test environment](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/create-and-test-a-new-creators-title).

## <a name="experimentation"></a>실험

**실험** 페이지를 사용하여 A/B 테스트로 UWP(유니버설 Windows 플랫폼) 앱에 대한 실험을 만들어 실행합니다. A/B 테스트에서 모든 고객에 대해 변경하기 전에 일부 고객의 앱에서 기능 변경(또는 변형)의 효과를 측정합니다.

자세한 내용은 [A/B 테스트로 앱 실험 실행](../monetize/run-app-experiments-with-a-b-testing.md)을 참조하세요.

## <a name="maps"></a>맵

Windows 10 또는 Windows 8.x를 대상으로 하는 앱에서 지도 서비스를 사용하려면 [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)를 방문하세요. For info about how to request a maps authentication key from the Bing Maps Developer Center and add it to your app, see [Request a maps authentication key](../maps-and-location/authentication-key.md) for more info. 

Use the **Maps** page only for previously-published apps for Windows Phone 8.1 and earlier. To use map services in these apps, you'll need to request a map service application ID and a token to include in your app's code. When you click **Get token**, we'll generate a Map service Application ID (**ApplicationID**) and Map service Authentication Token (**AuthenticationToken**) for your app. Be sure to add these values to your code before you package and submit your app. 자세한 내용은 [지도 컨트롤을 페이지에 추가하는 방법(Windows Phone 8.1)](https://docs.microsoft.com/previous-versions/windows/apps/jj207033(v=vs.105)?redirectedfrom=MSDN)을 참조하세요.

## <a name="product-collections-and-purchases"></a>제품 컬렉션 및 구입

To use the Microsoft Store collection API and the Microsoft Store purchase API to access ownership information for apps and add-ons, you need to enter the associated Azure AD client IDs here. 변경 사항이 적용되려면 최대 16시간까지 걸릴 수도 있습니다.

자세한 내용은 [서비스에서 제품 권리 유형 관리](../monetize/view-and-grant-products-from-a-service.md)를 참조하세요.

## <a name="administrator-consent"></a>Administrator consent

If your product integrates with Azure AD and calls APIs that request either [application permissions or delegated permissions](https://developer.microsoft.com/graph/docs/concepts/permissions_reference) that require administrator consent, enter your Azure AD Client ID here. This lets administrators who acquire the app for their organization grant consent for your product to act on behalf of all users in the tenant.

For more info, see [Requesting consent for an entire tenant](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#requesting-consent-for-an-entire-tenant).

## <a name="app-management"></a>앱 관리

**앱 관리** 섹션을 통해 ID 및 패키지 세부 정보를 보고 앱 이름을 관리할 수 있습니다.

## <a name="app-identity"></a>앱 ID

이 페이지에서는 앱 목록 링크의 URL을 포함하여 앱의 고유 ID에 관련된 세부 정보를 보여 줍니다.

자세한 내용은 [앱 ID 세부 정보 보기](view-app-identity-details.md)를 참조하세요.

## <a name="manage-app-names"></a>앱 이름 관리

여기에서는 앱용으로 예약한 모든 이름을 볼 수 있습니다. 여기에서 추가 이름을 예약하거나 더 사용하지 않는 이름을 삭제할 수 있습니다.

자세한 내용은 [앱 이름 관리](manage-app-names.md)를 참조하세요.

## <a name="current-packages"></a>현재 패키지

이 페이지에서는 모든 게시된 패키지에 관련된 세부 정보를 볼 수 있습니다.

> [!NOTE]
> 앱이 게시되고 나서 여기에 모든 정보가 표시됩니다.

각 패키지의 이름, 버전 및 아키텍처가 표시됩니다. **세부 정보**를 클릭하여 지원되는 언어, 앱 기능 및 파일 크기와 같은 추가 정보를 표시합니다. 각 패키지에 대해 표시되는 정보는 대상 운영 체제 및 기타 요소에 따라 다를 수 있습니다. 

OEM 권한이 있는 개발자는 **현재 패키지** 페이지에서 [사전 설치 패키지를 생성](generate-preinstall-packages-for-oems.md)할 수도 있습니다.

## <a name="wnsmpns"></a>WNS/MPNS

The **WNS/MPNS** section provides options to help you create and send notifications to your app's customers. 

> [!TIP]
> For UWP apps, we suggest using the **Notifications** feature in Partner Center. This feature lets you send notifications to all of your app's customers, or to a targeted subset of your Windows 10 customers who meet the criteria you’ve defined in a [customer segment](create-customer-segments.md). 자세한 내용은 [앱의 고객에게 알림 보내기](send-push-notifications-to-your-apps-customers.md)를 참조하세요.

Depending on your app's package type and its specific requirements, you can also use one of the following options: 

-   **WNS(Windows 푸시 알림 서비스)** 를 통해 클라우드 서비스에서 알림, 타일, 배지 및 원시 업데이트를 보낼 수 있습니다. 자세한 내용은 [WNS(Windows 푸시 알림 서비스) 개요](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)를 참조하세요.

-   **Microsoft Azure 모바일 앱**를 통해 푸시 알림을 보내고, 앱 사용자를 인증 및 관리하고, 앱 데이터를 클라우드에 저장할 수 있습니다. 자세한 내용은 [모바일 앱 설명서](https://docs.microsoft.com/azure/app-service-mobile/)를 참조하세요.

-   **Microsoft Push Notifications Service (MPNS)** can be used with previously published .xap packages for Windows Phone. 여기에서 구성을 하지 않고 제한적으로 인증되지 않은 알림을 보낼 수 있지만 대역폭 제한을 방지하려면 인증된 알림을 사용하는 것이 좋습니다. If you're using MPNS, you'll need to upload a certificate to the field provided on the **WNS/MPNS** page. 자세한 내용은 [Windows Phone 8에서 인증된 웹 서비스가 푸시 알림을 보내도록 설정](https://docs.microsoft.com/previous-versions/windows/apps/ff941099(v=vs.105)?redirectedfrom=MSDN)을 참조하세요.
 

 
