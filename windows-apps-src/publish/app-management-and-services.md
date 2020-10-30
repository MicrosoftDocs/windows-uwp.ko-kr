---
description: 파트너 센터에서 각 앱과 관련 된 세부 정보를 관리 하 고 보고 A/B 테스트 및 맵과 같은 서비스를 구성 합니다.
title: 앱 관리 및 서비스
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.date: 03/21/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e973e9b3a8f3c9ba63a091f4e542e36a84c26128
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032756"
---
# <a name="app-management-and-services"></a>앱 관리 및 서비스

[파트너 센터](https://partner.microsoft.com/dashboard)에서 각 앱과 관련 된 세부 정보를 관리 하 고 볼 수 있으며 알림, A/B 테스트, 지도 등의 서비스를 구성할 수 있습니다.

파트너 센터에서 앱을 사용 하는 경우 **서비스** 및 **앱 관리** 의 왼쪽 탐색 메뉴에 섹션이 표시 됩니다. 이러한 섹션을 확장 하 여 아래에 설명 된 기능에 액세스할 수 있습니다.

## <a name="services"></a>서비스

**서비스** 섹션을 사용 하 여 앱에 대 한 여러 가지 서비스를 관리할 수 있습니다.

## <a name="xbox-live"></a>Xbox Live

게임을 게시 하는 경우이 페이지에서 [Xbox Live 크리에이터 프로그램](https://www.xbox.com/developers/creators-program) 을 사용 하도록 설정할 수 있습니다. 그러면 Xbox Live 기능 구성 및 테스트를 시작 하 고, 궁극적으로 Xbox Live 크리에이터 프로그램 게임을 게시할 수 있습니다.

자세한 내용은 [Xbox Live 크리에이터 프로그램 시작](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators) 및 [새 Xbox live 크리에이터 프로그램 제목 만들기 및 테스트 환경에 게시](/gaming/xbox-live/get-started-with-creators/create-and-test-a-new-creators-title)를 참조 하세요.

## <a name="experimentation"></a>실험

**실험** 페이지를 사용 하 여 A/B 테스트를 통해 유니버설 WINDOWS 플랫폼 (UWP) 앱에 대 한 실험을 만들고 실행할 수 있습니다. A/B 테스트에서는 모든 사용자에 대 한 변경 내용을 사용 하도록 설정 하기 전에 일부 고객의 앱에서 기능 변경 (또는 변형)의 효율성을 측정 합니다.

자세한 내용은 [A/B 테스트로 앱 실험 실행](../monetize/run-app-experiments-with-a-b-testing.md)을 참조하세요.

## <a name="maps"></a>지도

Windows 10 또는 Windows 8.x을 대상으로 하는 앱에서 map services를 사용 하려면 [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)를 방문 하세요. Bing Maps 개발자 센터에서 지도 인증 키를 요청 하 고 앱에 추가 하는 방법에 대 한 자세한 내용은 [맵 인증 키 요청](../maps-and-location/authentication-key.md) 을 참조 하세요. 

이전에 게시 된 앱에 대해서만 **맵** 페이지를 사용 하 Windows Phone 8.1 이전 버전을 사용 합니다. 이러한 앱에서 map services를 사용 하려면 응용 프로그램 코드에 포함할 map service 응용 프로그램 ID와 토큰을 요청 해야 합니다. **토큰 가져오기** 를 클릭 하면 앱에 대 한 맵 서비스 응용 프로그램 ID ( **ApplicationID** ) 및 맵 서비스 인증 토큰 ( **authenticationtoken** )이 생성 됩니다. 앱을 패키지 하 고 제출 하기 전에 이러한 값을 코드에 추가 해야 합니다. 자세한 내용은 [페이지에 지도 컨트롤을 추가 하는 방법 (Windows Phone 8.1)](/previous-versions/windows/apps/jj207033(v=vs.105))을 참조 하세요.

## <a name="product-collections-and-purchases"></a>제품 컬렉션 및 구매

Microsoft Store 컬렉션 API 및 Microsoft Store 구매 API를 사용 하 여 앱 및 추가 기능에 대 한 소유권 정보에 액세스 하려면 여기에 연결 된 Azure AD 클라이언트 Id를 입력 해야 합니다. 이러한 변경 내용을 적용 하는 데 최대 16 시간이 걸릴 수 있습니다.

자세한 내용은 [서비스에서 제품 자격 관리](../monetize/view-and-grant-products-from-a-service.md)를 참조 하세요.

## <a name="administrator-consent"></a>관리자 동의

제품이 Azure AD와 통합 되 고 [응용 프로그램 사용 권한 또는](/graph/permissions-reference) 관리자 동의가 필요한 위임 된 권한을 요청 하는 api를 호출 하는 경우 여기에 Azure AD 클라이언트 ID를 입력 합니다. 이렇게 하면 조직의 앱을 확보 하는 관리자는 테 넌 트의 모든 사용자를 대신 하 여 제품에 대 한 동의를 부여할 수 있습니다.

자세한 내용은 [전체 테 넌 트에 대 한 동의 요청](/azure/active-directory/develop/v2-permissions-and-consent#requesting-consent-for-an-entire-tenant)을 참조 하세요.

## <a name="app-management"></a>앱 관리

**앱 관리** 섹션에서 id 및 패키지 세부 정보를 보고 앱의 이름을 관리할 수 있습니다.

## <a name="app-identity"></a>앱 id

이 페이지에는 앱 목록에 연결할 URL을 포함 하 여 스토어 내에서 앱의 고유한 id와 관련 된 세부 정보가 표시 됩니다.

자세한 내용은 [앱 id 세부 정보 보기](view-app-identity-details.md)를 참조 하세요.

## <a name="manage-app-names"></a>앱 이름 관리

여기에서 앱에 대해 예약한 모든 이름을 볼 수 있습니다. 여기에서 추가 이름을 예약 하거나 더 이상 사용 하지 않을 이름을 삭제할 수 있습니다.

자세한 내용은 [앱 이름 관리](manage-app-names.md)를 참조 하세요.

## <a name="current-packages"></a>현재 패키지

이 페이지에서는 모든 게시된 패키지에 관련된 세부 정보를 볼 수 있습니다.

> [!NOTE]
> 앱이 게시 될 때까지 여기에 정보가 표시 되지 않습니다.

각 패키지의 이름, 버전 및 아키텍처가 표시 됩니다. 지원 되는 언어, 앱 기능 및 파일 크기와 같은 추가 정보를 표시 하려면 **세부 정보** 를 클릭 합니다. 각 패키지에 대해 표시 되는 정보는 대상 운영 체제 및 기타 요인에 따라 달라질 수 있습니다. 

OEM 권한이 있는 개발자는 **현재 패키지** 페이지에서 [사전 설치 패키지를 생성할](generate-preinstall-packages-for-oems.md) 수도 있습니다.

## <a name="wnsmpns"></a>WNS/MPNS

**WNS/MPNS** 섹션에서는 앱의 고객에 게 알림을 만들고 보내는 데 사용할 수 있는 옵션을 제공 합니다. 

> [!TIP]
> UWP 앱의 경우 파트너 센터의 **알림** 기능을 사용 하는 것이 좋습니다. 이 기능을 사용 하면 모든 앱의 고객 또는 [고객 부문](create-customer-segments.md)에서 정의한 조건을 충족 하는 Windows 10 고객의 대상 하위 집합에 알림을 보낼 수 있습니다. 자세한 내용은 [앱 고객에 게 알림 보내기](send-push-notifications-to-your-apps-customers.md)를 참조 하세요.

앱의 패키지 형식 및 특정 요구 사항에 따라 다음 옵션 중 하나를 사용할 수도 있습니다. 

-   **WNS (Windows Push Notification Services)** 를 사용 하면 자신의 클라우드 서비스에서 알림, 타일, 배지 및 원시 업데이트를 보낼 수 있습니다. 자세한 내용은 [WNS (Windows 푸시 Notification Services) 개요](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)를 참조 하세요.

-   **Microsoft Azure Mobile Apps** 를 사용 하 여 푸시 알림을 보내고, 앱 사용자를 인증 하 고 관리 하며, 클라우드에 앱 데이터를 저장할 수 있습니다. 자세한 내용은 [Mobile Apps 설명서](/azure/app-service-mobile/)를 참조 하세요.

-   **MPNS (Microsoft 푸시 알림 서비스)** 는 Windows Phone에 대해 이전에 게시 된 xap 패키지와 함께 사용할 수 있습니다. 여기에서 구성을 수행 하지 않고 제한 된 수의 인증 되지 않은 알림을 보낼 수 있습니다. 단, 제한 제한을 방지 하려면 인증 된 알림을 사용 하는 것이 좋습니다. MPNS를 사용 하는 경우 **WNS/MPNS** 페이지에 제공 된 필드에 인증서를 업로드 해야 합니다. 자세한 내용은 [Windows Phone 8의 푸시 알림을 보내도록 인증 된 웹 서비스 설정](/previous-versions/windows/apps/ff941099(v=vs.105))을 참조 하십시오.
 

 
