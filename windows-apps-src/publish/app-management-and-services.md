---
Description: 관리 및 관련 된 각 파트너 센터에서 앱의 세부 정보 보기는 같은 서비스 구성 / B 테스트 및 매핑합니다.
title: 앱 관리 및 서비스
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d6261a7cce86c82b4865d7ca1d68c082cba9ccca
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610008"
---
# <a name="app-management-and-services"></a>앱 관리 및 서비스

관리 하 고 각 앱에 관련 된 세부 정보를 볼 수 있습니다 [파트너 센터](https://partner.microsoft.com/dashboard/), 알림, A와 같은 서비스 구성 및 / B 테스트 및 매핑합니다.

파트너 센터에서 앱으로 작업할 때의 왼쪽된 탐색 메뉴의 섹션을 볼 수 있습니다 **Services** 하 고 **앱 관리**합니다. 이들 섹션을 확장하여 아래 설명된 기능에 액세스할 수 있습니다.

## <a name="services"></a>서비스

**서비스** 섹션을 통해 앱에 대한 다양한 서비스를 관리할 수 있습니다.

## <a name="xbox-live"></a>Xbox Live

설정할 수 있습니다. 게임을 게시 하는 경우는 [Xbox Live 크리에이터 스 프로그램](https://xbox.com/developers/creators-program) 페이지입니다. 이렇게 하면 구성 및 테스트 Xbox Live 기능을 시작 하 고 최종적으로 게임에 Xbox Live 크리에이터 스 프로그램을 게시할 수 있습니다.

자세한 내용은 참조 하세요. [Xbox Live 크리에이터 스 프로그램 시작](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md) 하 고 [새 Xbox Live 크리에이터 스 프로그램 제목을 만들기 및 테스트 환경에 게시](../xbox-live/get-started-with-creators/create-and-test-a-new-creators-title.md)합니다.

## <a name="experimentation"></a>실험

**실험** 페이지를 사용하여 A/B 테스트로 UWP(유니버설 Windows 플랫폼) 앱에 대한 실험을 만들어 실행합니다. A/B 테스트에서 모든 고객에 대해 변경하기 전에 일부 고객의 앱에서 기능 변경(또는 변형)의 효과를 측정합니다.

자세한 내용은 [A/B 테스트로 앱 실험 실행](../monetize/run-app-experiments-with-a-b-testing.md)을 참조하세요.

## <a name="maps"></a>지도

Windows 10 또는 Windows 8.x를 대상으로 하는 앱에서 지도 서비스를 사용하려면 [Bing 지도 개발자 센터](https://go.microsoft.com/fwlink/p/?LinkId=614880)를 방문하세요. Bing 맵 개발자 센터에서 지도 인증 키를 요청 하 고 앱에 추가 하는 방법에 대 한 정보를 참조 하세요 [지도 인증 키를 요청](../maps-and-location/authentication-key.md) 자세한 정보에 대 한 합니다. 

사용 된 **Maps** 이전에 게시 된 앱 Windows Phone 8.1 및 이전 버전에 대해서만 페이지입니다. 이러한 앱에 지도 서비스를 사용 하려면 앱의 코드에 포함할 지도 서비스 응용 프로그램 ID 및 토큰을 요청 해야 합니다. 클릭 하면 **토큰 가져오기**, 맵 서비스 응용 프로그램 ID 생성 (**ApplicationID**) 서비스 인증 토큰을 매핑하고 (**AuthenticationToken**) 앱에 대 한 합니다. 패키지 있습니다 하기 전에 코드에 이러한 값을 추가 하 고 앱을 제출 해야 합니다. 자세한 내용은 [지도 컨트롤을 페이지에 추가하는 방법(Windows Phone 8.1)](https://go.microsoft.com/fwlink/p/?LinkId=614882)을 참조하세요.

## <a name="product-collections-and-purchases"></a>제품 컬렉션 및 구입

연결 된 입력 해야 하는 데 컬렉션 API 및 Microsoft Store 구매 API는 Microsoft Store 앱 및 추가 기능에 대 한 소유권 정보를 액세스, Azure AD 클라이언트 Id 여기 있습니다. 변경 사항이 적용되려면 최대 16시간까지 걸릴 수도 있습니다.

자세한 내용은 [서비스에서 제품 권리 유형 관리](../monetize/view-and-grant-products-from-a-service.md)를 참조하세요.

## <a name="administrator-consent"></a>관리자 동의

제품 Azure AD와 통합 되 고 하나를 요청 하는 Api 호출 [응용 프로그램 권한 또는 위임 된 권한](https://developer.microsoft.com/graph/docs/concepts/permissions_reference) 관리자 동의 필요로 하는 여기에 Azure AD 클라이언트 ID를 입력 합니다. 이렇게 하면 테 넌 트의 모든 사용자를 대신 하 여 제품에 대 한 조직 부여 동의 대 한 앱을 획득 하는 관리자 수 있습니다.

자세한 내용은 참조 하세요. [전체 테 넌 트에 대 한 동의 요청](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes#requesting-consent-for-an-entire-tenant)합니다.

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

합니다 **WNS/MPNS** 섹션을 만들고 앱의 고객에 게 알림을 보낼 수 있도록 옵션을 제공 합니다. 

> [!TIP]
> UWP 앱에 대 한 사용 하 여 제안 합니다 **알림을** 파트너 센터의 기능입니다. 이 기능을 사용 하 여 모든 앱의 고객에 게 알림을 보낼 수 또는에 정의 된 조건을 충족 하는 Windows 10 고객 대상된 하위 집합에는 [고객 세그먼트](create-customer-segments.md)합니다. 자세한 내용은 [앱의 고객에게 알림 보내기](send-push-notifications-to-your-apps-customers.md)를 참조하세요.

앱의 패키지 형식 및 특정 요구 사항에 따라 다음 옵션 중 하나을 사용할 수 있습니다. 

-   **WNS(Windows 푸시 알림 서비스)** 를 통해 클라우드 서비스에서 알림, 타일, 배지 및 원시 업데이트를 보낼 수 있습니다. 자세한 내용은 [WNS(Windows 푸시 알림 서비스) 개요](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)를 참조하세요.

-   **Microsoft Azure 모바일 앱**를 통해 푸시 알림을 보내고, 앱 사용자를 인증 및 관리하고, 앱 데이터를 클라우드에 저장할 수 있습니다. 자세한 내용은 [모바일 앱 설명서](https://go.microsoft.com/fwlink/p/?LinkId=221116)를 참조하세요.

-   **Microsoft 푸시 알림 서비스 (MPNS)** Windows Phone 이전에 게시 된.xap 패키지와 함께 사용할 수 있습니다. 여기에서 구성을 하지 않고 제한적으로 인증되지 않은 알림을 보낼 수 있지만 대역폭 제한을 방지하려면 인증된 알림을 사용하는 것이 좋습니다. 제공 된 필드에 인증서를 업로드 해야 MPNS를 사용 하는 경우는 **WNS/MPNS** 페이지입니다. 자세한 내용은 [Windows Phone 8에서 인증된 웹 서비스가 푸시 알림을 보내도록 설정](https://go.microsoft.com/fwlink/p/?LinkId=528736)을 참조하세요.
 

 
