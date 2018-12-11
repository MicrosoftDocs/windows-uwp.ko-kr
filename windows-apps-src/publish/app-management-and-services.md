---
Description: Manage and view details related to each of your apps in Partner Center, and configure services such as A/B testing and maps.
title: 앱 관리 및 서비스
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6a8f75fae686763f3d79cea2f02c3208993cb723
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8874774"
---
# <a name="app-management-and-services"></a>앱 관리 및 서비스

관리 하 고 각 앱과 관련 된 세부 정보를 볼 수 [파트너 센터, 알림, A와 같은 서비스를 구성 하 고 / B 테스트, 지도 합니다.

파트너 센터에서 앱을 사용할 때는 **서비스** 및 **앱 관리**섹션의 왼쪽된 탐색 메뉴에 표시 됩니다. 이들 섹션을 확장하여 아래 설명된 기능에 액세스할 수 있습니다.

## <a name="services"></a>서비스

**서비스** 섹션을 통해 앱에 대한 다양한 서비스를 관리할 수 있습니다.

## <a name="xbox-live"></a>Xbox Live

게임을 게시 하는 경우이 페이지에서 [Xbox Live 크리에이터 스 프로그램](http://xbox.com/developers/creators-program) 을 사용할 수 있습니다. 이렇게 하면 구성 및 테스트 Xbox Live 기능을 시작 하 고 결국 Xbox Live 크리에이터 스 프로그램 게임을 게시할 수 있습니다.

자세한 내용은 [Xbox Live 크리에이터 스 프로그램 시작](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md) 하 고 [새 Xbox Live 크리에이터 스 프로그램 타이틀 만들기 및 테스트 환경에 게시](../xbox-live/get-started-with-creators/create-and-test-a-new-creators-title.md)를 참조 하세요.

## <a name="experimentation"></a>실험

**실험** 페이지를 사용하여 A/B 테스트로 UWP(유니버설 Windows 플랫폼) 앱에 대한 실험을 만들어 실행합니다. A/B 테스트에서 모든 고객에 대해 변경하기 전에 일부 고객의 앱에서 기능 변경(또는 변형)의 효과를 측정합니다.

자세한 내용은 [A/B 테스트로 앱 실험 실행](../monetize/run-app-experiments-with-a-b-testing.md)을 참조하세요.

## <a name="maps"></a>지도

Windows10 또는 Windows8.x를 대상으로 하는 앱에서 지도 서비스를 사용하려면 [Bing 지도 개발자 센터](http://go.microsoft.com/fwlink/p/?LinkId=614880)를 방문하세요. Bing 지도 개발자 센터에서 지도 인증 키를 요청 하 고 앱에 추가 하는 방법에 대 한 정보를 [지도 인증 키 요청](../maps-and-location/authentication-key.md) 에 대 한 자세한 내용은 참조 하세요. 

Windows Phone 8.1 이하의 이전에 게시 된 앱에만 **지도** 페이지를 사용 합니다. 이러한 앱에서 지도 서비스를 사용 하려면 앱의 코드에 포함 하려면 지도 서비스 응용 프로그램 ID와 토큰을 요청 해야 합니다. **토큰을 가져올**을 클릭할 때 지도 서비스 응용 프로그램 ID (**응용 프로그램 Id**)를 생성 하 고 앱에 대 한 인증 토큰 (**AuthenticationToken**) 서비스를 매핑합니다. 하면 패키지 하기 전에 코드에 이러한 값을 추가 하 고 앱을 제출 해야 합니다. 자세한 내용은 [지도 컨트롤을 페이지에 추가하는 방법(Windows Phone 8.1)](http://go.microsoft.com/fwlink/p/?LinkId=614882)을 참조하세요.

## <a name="product-collections-and-purchases"></a>제품 컬렉션 및 구입

연결 된 입력 해야 앱과 추가 기능에 대 한 소유권 정보에 액세스 하는 Microsoft Store 컬렉션 API 및 Microsoft Store 구매 API를 사용 하려면 Azure AD 클라이언트 Id 다음과 같습니다. 변경 사항이 적용되려면 최대 16시간까지 걸릴 수도 있습니다.

자세한 내용은 [서비스에서 제품 권리 유형 관리](../monetize/view-and-grant-products-from-a-service.md)를 참조하세요.

## <a name="administrator-consent"></a>관리자 동의

f 제품 Azure AD와 통합 되 고 관리자 동의 해야 하는 [응용 프로그램 사용 권한이 나 위임 된 사용 권한을](https://developer.microsoft.com/graph/docs/concepts/permissions_reference) 요청 하는 Api를 호출 하면 Azure AD 클라이언트 ID를 입력 합니다. 이렇게 하면 관리자에 게 테 넌 트의 모든 사용자를 대신 하 여 제품의 조직 부여 동의 대 한 앱을 구입 하는 수 있습니다.

자세한 정보는 [전체 테 넌 트에 대 한 동의 요청](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes#requesting-consent-for-an-entire-tenant)을 참조 하세요.

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

**WNS/MPNS** 섹션을 만들고 앱의 고객에 게 알림을 보낼 수 있도록 옵션을 제공 합니다. 

> [!TIP]
> UWP 앱에 대 한 파트너 센터에서 **알림** 기능을 사용 하는 것이 좋습니다. [고객 세그먼트](create-customer-segments.md)에서 정의한 기준을 충족 하는 Windows 10 고객의 대상 일부 또는이 기능은 모든 앱의 고객에 게 알림을 보낼 수 있습니다. 자세한 내용은 [앱의 고객에게 알림 보내기](send-push-notifications-to-your-apps-customers.md)를 참조하세요.

앱의 패키지 유형 및 특정 요구 사항에 따라 다음 옵션 중 하나을 사용할 수 있습니다. 

-   **WNS(Windows 푸시 알림 서비스)** 를 통해 클라우드 서비스에서 알림, 타일, 배지 및 원시 업데이트를 보낼 수 있습니다. 자세한 내용은 [WNS(Windows 푸시 알림 서비스) 개요](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)를 참조하세요.

-   **Microsoft Azure 모바일 앱**를 통해 푸시 알림을 보내고, 앱 사용자를 인증 및 관리하고, 앱 데이터를 클라우드에 저장할 수 있습니다. 자세한 내용은 [모바일 앱 설명서](http://go.microsoft.com/fwlink/p/?LinkId=221116)를 참조하세요.

-   Windows Phone 이전에 게시 된.xap 패키지를 사용 하 여 **Microsoft 푸시 알림 서비스 (MPNS)를** 사용할 수 있습니다. 여기에서 구성을 하지 않고 제한적으로 인증되지 않은 알림을 보낼 수 있지만 대역폭 제한을 방지하려면 인증된 알림을 사용하는 것이 좋습니다. MPNS를 사용 하는 경우에 **WNS/MPNS** 페이지에 제공 된 필드에 인증서를 업로드 해야 합니다. 자세한 내용은 [Windows Phone 8에서 인증된 웹 서비스가 푸시 알림을 보내도록 설정](http://go.microsoft.com/fwlink/p/?LinkId=528736)을 참조하세요.
 

 
