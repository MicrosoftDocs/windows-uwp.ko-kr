---
author: mcleanbyron
ms.assetid: c92c0ea8-f742-4fc1-a3d7-e90aac11953e
description: "Windows 스토어 리뷰 API를 사용하여 스토어의 앱 리뷰에 프로그래밍 방식으로 응답을 제출할 수 있습니다."
title: "Windows 스토어 서비스를 사용하여 리뷰에 응답"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, Windows 스토어 리뷰 API, 리뷰에 응답"
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 657149304048a88bf85f0dd6f205e7db0497e591
ms.lasthandoff: 02/08/2017

---

# <a name="respond-to-reviews-using-windows-store-services"></a>Windows 스토어 서비스를 사용하여 리뷰에 응답

*Windows 스토어 리뷰 API*를 사용하여 스토어의 앱 리뷰에 프로그래밍 방식으로 응답할 수 있습니다. 이 API는 Windows 개발자 센터 대시보드를 사용하지 않고 많은 리뷰에 대량으로 응답하려는 개발자에게 특히 유용합니다. 이 API는 Azure AD(Azure Active Directory)를 사용하여 앱 또는 서비스의 호출을 인증합니다.

다음 단계에서는 종단 간 프로세스를 설명합니다.

1.  [필수 조건](#prerequisites)을 모두 완료했는지 확인합니다.
2.  Windows 스토어 리뷰 API에서 메서드를 호출하기 전에 [Azure AD 액세스 토큰을 가져옵니다](#obtain-an-azure-ad-access-token). 토큰을 가져온 후 만료되기 전에 이 토큰을 Windows 스토어 리뷰 API에 대한 호출에 사용할 수 있는 시간은 60분입니다. 토큰이 만료된 후 새 토큰을 생성할 수 있습니다.
3.  [Windows 스토어 리뷰 API를 호출](#call-the-windows-store-reviews-api)합니다.

>**참고**&nbsp;&nbsp;Windows 스토어 리뷰 API를 사용하여 프로그래밍 방식으로 리뷰에 응답하는 것 외에도 [Windows 개발자 센터 대시보드를 사용하여](../publish/respond-to-customer-reviews.md) 리뷰에 응답할 수 있습니다.

<span id="prerequisites" />
## <a name="step-1-complete-prerequisites-for-using-the-windows-store-reviews-api"></a>단계 1: Windows 스토어 리뷰 API를 사용하기 위한 필수 조건 완료

Windows 스토어 리뷰 API를 호출하는 코드를 작성하기 전에 다음과 같은 필수 조건을 완료했는지 확인합니다.

* 사용자(또는 조직)에게 Azure AD 디렉터리와 해당 디렉터리에 대한 [전역 관리자](http://go.microsoft.com/fwlink/?LinkId=746654) 권한이 있어야 합니다. 이미 Office 365 또는 Microsoft의 다른 비즈니스 서비스를 사용하는 경우 이미 Azure AD 디렉터리가 있습니다. 아니면 추가 요금 없이 [개발자 센터 내에서 Azure AD를 새로 만들 수 있습니다](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users).

* Azure AD 응용 프로그램을 개발자 센터 계정과 연결하고 응용 프로그램에 대한 테넌트 ID 및 클라이언트 ID를 검색하고 키를 생성해야 합니다. Azure AD 응용 프로그램은 Windows 스토어 리뷰 API를 호출할 앱 또는 서비스입니다. API에 전달하는 Azure AD 액세스 토큰을 가져오려면 테넌트 ID, 클라이언트 ID 및 키가 필요합니다.

  >**참고**&nbsp;&nbsp;이 작업은 한 번만 수행하면 됩니다. 테넌트 ID, 클라이언트 ID 및 키는 Azure AD 액세스 토큰을 새로 만들 때마다 다시 사용할 수 있습니다.

Azure AD 응용 프로그램을 개발자 센터 계정에 연결하고 필요한 값을 검색하려면

1.  개발자 센터에서 **계정 설정**으로 이동하여 **사용자 관리**를 클릭하고 조직의 개발자 센터 계정을 조직의 Azure AD 디렉터리와 연결합니다. 자세한 내용은 [계정 사용자 관리](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users)를 참조하세요.

2.  **사용자 관리** 페이지에서 **Azure AD 응용 프로그램 추가**를 클릭하여 개발자 센터 계정에 대한 프로모션 캠페인을 관리하는 데 사용할 앱 또는 서비스를 나타내는 Azure AD 응용 프로그램을 추가하고 **관리자** 역할에 할당합니다. 이 응용 프로그램이 Azure AD 디렉터리에 이미 존재하는 경우 **Azure AD 응용 프로그램 추가** 페이지에서 선택하여 개발자 센터 계정에 추가할 수 있습니다. 그러지 않으면 **Azure AD 응용 프로그램 추가 페이지**에서 새 Azure AD 응용 프로그램을 만들 수 있습니다. 자세한 내용은 [Azure AD 응용 프로그램 추가 및 관리](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications)를 참조하세요.

3.  **사용자 관리** 페이지로 돌아가서 Azure AD 응용 프로그램의 이름을 클릭하여 응용 프로그램 설정으로 이동하고 **테넌트 ID** 및 **클라이언트 ID** 값을 복사합니다.

4. **새 키 추가**를 클릭합니다. 다음 화면에서 **키** 값을 복사합니다. 이 페이지를 벗어난 후에는 이 정보에 다시 액세스할 수 없습니다. 자세한 내용은 [Azure AD 응용 프로그램 추가 및 관리](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications)에서 키 관리에 대한 내용을 참조하세요.

<span id="obtain-an-azure-ad-access-token" />
## <a name="step-2-obtain-an-azure-ad-access-token"></a>단계 2: Azure AD 액세스 토큰 가져오기

Windows 스토어 리뷰 API에서 메서드를 호출하기 전에 먼저 API에 있는 각 메서드의 **Authorization** 헤더에 전달하는 Azure AD 액세스 토큰을 가져와야 합니다. 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 API에 대한 추가 호출에 계속 사용할 수 있도록 해당 토큰을 새로 고칠 수 있습니다.

액세스 토큰을 가져오려면 [클라이언트 자격 증명을 사용한 서비스 간 호출](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/)의 지침에 따라 HTTP POST를 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` 끝점에 보냅니다. 다음은 샘플 요청입니다.

```syntax
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

POST URI의 *tenant\_id* 값, *client\_id* 및 *client\_secret* 매개 변수에는 이전 섹션의 개발자 센터에서 검색한 응용 프로그램의 테넌트 ID, 클라이언트 ID 및 키를 지정합니다. *resource* 매개 변수에는 ```https://manage.devcenter.microsoft.com```을 지정해야 합니다.

만료된 액세스 토큰은 [여기](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)의 지침에 따라 새로 고칠 수 있습니다.

<span id="call-the-windows-store-reviews-api" />
## <a name="step-3-call-the-windows-store-reviews-api"></a>단계 3: Windows 스토어 리뷰 API 호출

Azure AD 액세스 토큰이 있으면 Windows 스토어 리뷰 API를 호출할 준비가 된 것입니다. 액세스 토큰을 각 메서드의 **Authorization** 헤더로 전달해야 합니다.

Windows 스토어 리뷰 API에는 주어진 리뷰에 대한 응답이 허용되는지 여부 및 하나 이상의 리뷰에 대한 응답 제출이 허용되는지 여부를 결정하는 데 사용할 수 있는 몇 가지 메서드가 포함되어 있습니다. 이 API를 사용하려면 다음 과정에 따릅니다.

1. 응답하려는 리뷰의 ID를 가져옵니다. 리뷰 ID는 Windows 스토어 분석 API의 [앱 리뷰 가져오기](get-app-reviews.md) 메서드의 응답 데이터와 [리뷰 보고서](../publish/reviews-report.md)의 [오프라인 다운로드](../publish/download-analytic-reports.md)에서 사용할 수 있습니다.
2. [앱 리뷰용 응답 정보 가져오기](get-response-info-for-app-reviews.md) 메서드를 호출해 리뷰에 응답할 수 있는지 여부를 결정합니다. 리뷰를 제출하는 고객은 해당 리뷰에 대한 응답을 받지 않기로 선택할 수 있습니다. 리뷰 응답을 받지 않기로 선택한 고객이 제출한 리뷰에는 응답할 수 없습니다.
3. [앱 리뷰에 대한 응답 제출](submit-responses-to-app-reviews.md) 메서드를 호출하여 프로그래밍 방식으로 리뷰에 응답합니다.


## <a name="related-topics"></a>관련 항목

* [앱 리뷰 가져오기](get-app-reviews.md)
* [앱 리뷰용 응답 정보 가져오기](get-response-info-for-app-reviews.md)
* [앱 리뷰에 대한 응답 제출](submit-responses-to-app-reviews.md)

 

