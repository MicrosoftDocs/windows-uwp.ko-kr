---
ms.assetid: c92c0ea8-f742-4fc1-a3d7-e90aac11953e
description: Microsoft Store 리뷰 API를 사용 하 여 스토어의 앱 검토에 대 한 응답을 프로그래밍 방식으로 제출할 수 있습니다.
title: 스토어 서비스를 사용 하 여 리뷰에 응답
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 리뷰 API, 리뷰에 응답
ms.localizationpriority: medium
ms.openlocfilehash: 3a88f55555245ac64982b01920e538295c2ffbd2
ms.sourcegitcommit: 720413d2053c8d5c5b34d6873740be6e913a4857
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88846853"
---
# <a name="respond-to-reviews-using-store-services"></a>스토어 서비스를 사용 하 여 리뷰에 응답

*Microsoft Store 리뷰 API* 를 사용 하 여 스토어의 앱 검토에 프로그래밍 방식으로 응답 합니다. 이 API는 파트너 센터를 사용 하지 않고 많은 검토에 대량으로 응답 하려는 개발자에 게 특히 유용 합니다. 이 API는 Azure Active Directory (Azure AD)를 사용 하 여 앱 또는 서비스에서 호출을 인증 합니다.

다음 단계는 종단 간 프로세스를 설명 합니다.

1.  모든 [필수 구성 요소](#prerequisites)를 완료 했는지 확인 합니다.
2.  Microsoft Store 리뷰 API에서 메서드를 호출 하기 전에 [AZURE AD 액세스 토큰을 가져옵니다](#obtain-an-azure-ad-access-token). 토큰을 가져온 후에는 토큰이 만료 되기 전에 Microsoft Store 검토 API에 대 한 호출에서이 토큰을 사용 하는 데 60 분이 소요 됩니다. 토큰이 만료 된 후 새 토큰을 생성할 수 있습니다.
3.  [Microsoft Store 리뷰 API를 호출](#call-the-windows-store-reviews-api)합니다.

> [!NOTE]
> Microsoft Store 리뷰 API를 사용 하 여 리뷰에 프로그래밍 방식으로 응답 하는 것 외에도 [파트너 센터를 사용 하 여](../publish/respond-to-customer-reviews.md)리뷰에 응답할 수 있습니다.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-reviews-api"></a>1 단계: Microsoft Store 검토 API를 사용 하기 위한 필수 구성 요소 완료

Microsoft Store 리뷰 API를 호출 하는 코드 작성을 시작 하기 전에 다음 필수 구성 요소를 완료 했는지 확인 합니다.

* 사용자(또는 조직)는 Azure AD 디렉터리가 있어야 하고 디렉터리에 대한 [전역 관리자](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) 권한이 있어야 합니다. Microsoft에서 이미 Microsoft 365 또는 다른 비즈니스 서비스를 사용 하는 경우 Azure AD 디렉터리가 이미 있습니다. 그렇지 않으면 추가 비용 없이 [파트너 센터에서 새 AZURE AD를 만들](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) 수 있습니다.

* Azure AD 응용 프로그램을 파트너 센터 계정에 연결 하 고, 응용 프로그램에 대 한 테 넌 트 ID와 클라이언트 ID를 검색 하 고, 키를 생성 해야 합니다. Azure AD 응용 프로그램은 Microsoft Store 검토 API를 호출 하려는 응용 프로그램 또는 서비스를 나타냅니다. API에 전달하는 Azure AD 액세스 토큰을 얻으려면 테넌트 ID, 클라이언트 ID 및 키가 필요합니다.
    > [!NOTE]
    > 이 작업은 한 번만 수행 하면 됩니다. 테넌트 ID, 클라이언트 ID 및 키가 있으면 새 Azure AD 액세스 토큰을 만들어야 할 때마다 다시 사용할 수 있습니다.

Azure AD 응용 프로그램을 파트너 센터 계정에 연결 하 고 필요한 값을 검색 하려면 다음을 수행 합니다.

1.  파트너 센터에서 [조직의 파트너 센터 계정을 조직의 Azure AD 디렉터리에 연결합니다](../publish/associate-azure-ad-with-partner-center.md).

2.  다음으로, 파트너 센터의 **계정 설정** 섹션에 있는 **사용자** 페이지에서 리뷰에 응답 하는 데 사용할 앱 또는 서비스를 나타내는 [Azure AD 응용 프로그램을 추가](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) 합니다. 이 애플리케이션을 **관리자** 역할에 할당해야 합니다. 애플리케이션이 아직 Azure AD 디렉터리에 존재하지 않는 경우 [파트너 센터에서 새 Azure AD 애플리케이션 만들 수 있습니다](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account). 

3.  **사용자** 페이지로 돌아가 Azure AD 애플리케이션의 이름을 클릭하여 애플리케이션 설정으로 이동한 다음 **테넌트 ID**와 **클라이언트 ID** 값을 복사합니다.

4. **새 키 추가**를 클릭합니다. 다음 화면에서 **키** 값을 복사합니다. 이 페이지를 나가면 이 정보에 다시 액세스할 수 없습니다. 자세한 내용은 [Azure AD 애플리케이션 키 관리](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys)를 참조하세요.

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>2단계: Azure AD 액세스 토큰 가져오기

Microsoft Store 리뷰 API에서 메서드를 호출 하기 전에 먼저 API의 각 메서드에 대 한 **인증** 헤더에 전달 하는 Azure AD 액세스 토큰을 가져와야 합니다. 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후에는 API에 대 한 추가 호출에서 토큰을 계속 사용할 수 있도록 토큰을 새로 고칠 수 있습니다.

액세스 토큰을 가져오려면 [클라이언트 자격 증명을 사용 하 여 서비스 호출](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) 에 대 한 지침에 따라 HTTP POST를 끝점으로 보냅니다 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` . 샘플 요청은 다음과 같습니다.

```syntax
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

POST URI와 클라이언트 * \_ id* 및 *클라이언트 \_ 암호* 매개 변수의 *테 넌 트 \_ Id* 값에 대해 이전 섹션의 파트너 센터에서 검색 한 응용 프로그램의 테 넌 트 id, 클라이언트 id 및 키를 지정 합니다. *리소스* 매개 변수에는 ```https://manage.devcenter.microsoft.com```을 지정해야 합니다.

액세스 토큰이 만료 되 면 [여기](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)에 설명 된 지침에 따라 새로 고칠 수 있습니다.

<span id="call-the-windows-store-reviews-api" />

## <a name="step-3-call-the-microsoft-store-reviews-api"></a>3 단계: Microsoft Store 검토 API 호출

Azure AD 액세스 토큰을 만든 후에는 Microsoft Store 리뷰 API를 호출할 준비가 된 것입니다. 각 메서드의 **인증** 헤더에 액세스 토큰을 전달 해야 합니다.

Microsoft Store 검토 API에는 지정 된 검토에 응답할 수 있는지 여부를 확인 하는 데 사용할 수 있는 몇 가지 방법과 하나 이상의 검토에 대 한 응답을 전송 하는 데 사용할 수 있는 몇 가지 방법이 있습니다. 이 API를 사용 하려면 다음 절차를 따르세요.

1. 응답 하려는 검토의 Id를 가져옵니다. 검토 Id는 Microsoft Store 분석 API 및 [검토 보고서](../publish/reviews-report.md)의 [오프 라인 다운로드](../publish/download-analytic-reports.md) 에서 [앱 검토 가져오기](get-app-reviews.md) 메서드의 응답 데이터에서 사용할 수 있습니다.
2. [앱 검토에 대 한 응답 정보 가져오기](get-response-info-for-app-reviews.md) 메서드를 호출 하 여 검토에 응답할 수 있는지 여부를 확인 합니다. 고객은 리뷰를 제출할 때 해당 검토에 대 한 응답을 수신 하지 않도록 선택할 수 있습니다. 검토 응답을 수신 하지 않기로 선택한 고객이 제출한 검토에 응답할 수 없습니다.
3. [앱 리뷰에 대 한 응답 제출](submit-responses-to-app-reviews.md) 방법을 호출 하 여 프로그래밍 방식으로 검토에 응답 합니다.


## <a name="related-topics"></a>관련 항목

* [앱 리뷰 가져오기](get-app-reviews.md)
* [앱 리뷰에 대 한 응답 정보 가져오기](get-response-info-for-app-reviews.md)
* [앱 리뷰에 응답 제출](submit-responses-to-app-reviews.md)

 
