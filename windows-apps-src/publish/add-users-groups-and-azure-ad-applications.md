---
author: jnHs
Description: You can add users, groups, and Azure AD applications to your Partner Center account.
title: 사용자, 그룹, 파트너 센터 계정에 Azure AD 응용 프로그램 추가
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, azure ad 응용 프로그램, aad, 사용자, 그룹, 여러 사용자, 다중 사용자
ms.localizationpriority: medium
ms.openlocfilehash: 2821132944a20260d0005f8925c23ab48581a9e2
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6200832"
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-partner-center-account"></a>사용자, 그룹, 파트너 센터 계정에 Azure AD 응용 프로그램 추가

( **계정 설정**)에서 [파트너 센터](https://partner.microsoft.com/dashboard) **사용자** 섹션에는 Azure Active Directory를 사용 하 여 파트너 센터 계정에 사용자를 추가할 수 있습니다. 각 사용자에게 계정에 대한 사용 권한을 정의하는 역할(또는 사용자 지정 권한 집합)이 할당됩니다. 또한 [사용자 그룹](#groups) 및 파트너 센터 계정에 대 한 액세스 권한을 부여할 수 있는 [Azure AD 응용 프로그램](#azure-ad-applications) 을 추가할 수 있습니다.

사용자가 계정에 추가되고 나면 [계정 세부 정보 편집](#edit), [역할 및 사용 권한](set-custom-permissions-for-account-users.md), 변경 또는 [사용자 제거](#remove)가 가능합니다.

> [!IMPORTANT]
> 계정에 사용자를 추가 하려면 먼저 [파트너 센터 계정을 조직의 Azure Active Directory 테 넌 트와 연결](associate-azure-ad-with-dev-center.md)해야 합니다. 

사용자를 추가할 때 파트너 센터 계정에 대 한 액세스는 [역할 또는 사용자 지정 권한 집합에](set-custom-permissions-for-account-users.md)할당 하 여 지정 해야 합니다. 

모든 파트너 센터 사용자 (그룹 및 Azure AD 응용 프로그램 포함)는 [파트너 센터 계정과 연결 된 Azure AD 테 넌 트](associate-azure-ad-with-dev-center.md)에 활성 계정이 있어야을 염두에 두어야 합니다. 사용자 관리는 한 번에 한 테넌트에서 완료되며 사용자를 추가 또는 편집하려는 테넌트에 대한 관리자 계정으로 로그인해야 합니다. 파트너 센터에서 새 사용자 만들기, 로그인 및 조직의 Azure AD 테 넌 트에 동일한 변경을 수행할 됩니다 파트너 센터에서 사용자의 이름을 변경 하는 Azure AD 테 넌 트에서 해당 사용자에 대 한 계정이 함께 생성 됩니다.

> [!NOTE]
> 조직 [디렉터리 통합](http://go.microsoft.com/fwlink/p/?LinkID=724033) 을 사용 하 여 온-프레미스 디렉터리 서비스와 Azure AD 동기화를 파트너 센터에서 새 사용자, 그룹 또는 Azure AD 응용 프로그램을 만들 수 없습니다. 사용자 (또는 온-프레미스 디렉터리에 다른 관리자) 확인 하 고 파트너 센터에 추가할 수 있게 됩니다 전에 온-프레미스 디렉터리에서 직접 작성 해야 합니다.


<span id="users" />

## <a name="add-users-to-your-partner-center-account"></a>파트너 센터 계정에 사용자 추가

파트너 센터 계정에 사용자를 추가 하려면 **계정 설정** 의 **사용자** 페이지로 이동 하 고 선택 **사용자를 추가 합니다.** 작업하려는 Azure AD 테넌트에 대한 관리자 계정을 사용하여 로그인해야 합니다. 

### <a name="add-existing-users"></a>기존 사용자 추가 

이미 조직의 테 넌 트에 존재 하 고 파트너 센터 계정에 대 한 액세스 권한을 부여 하는 사용자를 선택할 수 있습니다. 

<span id="from-directory" />

1.  파트너 센터의 오른쪽 위 모서리) (근처 기어 아이콘을 선택 하 고 **개발자 설정**를 선택 합니다. **설정** 메뉴에서 **사용자**를 선택 합니다.
2.  **사용자** 페이지에서 **사용자 추가**를 선택합니다. 
3.  표시되는 목록에서 하나 이상의 사용자를 선택합니다. 검색 상자를 사용하여 특정 사용자를 검색할 수 있습니다.
    > [!TIP]
    > 파트너 센터 계정에 추가 하는 둘 이상의 사용자를 선택 하는 경우 동일한 역할 또는 사용자 지정 권한 집합을 할당 해야 합니다. 역할/권한이 서로 다른 사용자를 여러 명 추가하려면 각 역할 또는 사용자 지정 권한 집합에 대해 다음 단계를 반복합니다.
4.  사용자 선택이 완료되면 **선택된 항목 추가**를 클릭합니다.
5.  **역할** 섹션에서 선택한 사용자에 대한 [역할 또는 사용자 지정 권한 집합](set-custom-permissions-for-account-users.md) 을 지정합니다.
6.  **저장**을 클릭합니다.

### <a name="additional-methods-for-adding-users"></a>사용자를 추가하는 추가 방법

Azure AD 테 넌 트에 대 한 [전역 관리자](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) 권한이 있는 관리자 계정으로 로그인에서 작업 하 고, 추가 옵션을 파트너 센터 계정에 사용자를 추가 해야 합니다. 다음 중 하나를 선택해야 합니다.

-   **기존 사용자 추가**: 이미 조직의 디렉터리에 존재 하 고 액세스를 제공 하도록 파트너 센터 계정에 위에서 설명한 메서드를 사용 하는 사용자를 선택 합니다.
-   **새 사용자 만들기**: 모두 조직의 디렉터리에 추가 하려면 새로운 사용자 계정 및 파트너 센터 계정 만들기
-   **외부 사용자 초대**: 현재 조직의 디렉터리에 있지 않은 사용자에게 메일 초대장을 전송합니다. 파트너 센터 계정에 액세스 하 라는 초대를 하 고 새 [게스트 사용자](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) 계정에 Azure AD 테 넌 트에 생성 됩니다.

<span id="new-user" />

### <a name="create-new-users"></a>새 사용자 만들기

> [!IMPORTANT]
> 새로운 사용자를 만들려면 반드시 Azure AD 테넌트에서 전역 관리자 계정으로 로그인을 해야 합니다.

1.  ( **계정 설정**)에 따라 **사용자가** 페이지에서 **추가 사용자가**선택한 **새 사용자 만들기를**선택 합니다.
2.  새 사용자의 이름, 성 및 사용자 이름을 입력합니다.
3.  조직 디렉터리에서 새 사용자에게 [전역 관리자 계정](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)을 부여하려면 **Azure AD에서 이 사용자를 전역 관리자로 지정하여 모든 디렉터리 리소스에 대한 모든 권한을 부여** 상자를 선택합니다. 이렇게 하면 해당 사용자에게 회사 Azure AD의 모든 관리 기능에 대한 모든 권한이 부여됩니다. 추가 하 고 (아닌 파트너 센터에서는 계정에 적절 한 [역할/권한](set-custom-permissions-for-account-users.md)부여 하지 않는 한) 하지만 조직의 디렉터리에서 사용자를 관리할 수 있습니다. 이 상자를 선택하면 사용자에게 **암호 복구 메일**을 보내야 합니다.
4.  **Azure AD에서 이 사용자를 전역 관리자로 지정** 확인란을 선택한 경우 사용자가 암호를 복구해야 하는 경우 사용할 수 있는 메일 주소를 입력합니다.
5.  **그룹 구성원** 섹션에서 새 사용자가 속하게 될 그룹을 선택합니다.
6.  **역할** 섹션에서 해당 사용자에 대한 [역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md) 을 지정합니다.
7.  **저장**을 클릭합니다.
8.  확인 페이지에 임시 암호를 비롯하여 새 사용자의 로그인 정보가 표시됩니다. 이 페이지를 나간 후에는 임시 암호에 액세스할 수 없으므로 이 정보를 메모하고 새 사용자에게 제공해야 합니다.


<span id="email" />

### <a name="invite-outside-users"></a>외부 사용자 초대

> [!IMPORTANT]
> 외부 사용자를 초대하려면 반드시 Azure AD 테넌트에서 전역 관리자 계정으로 로그인을 해야 합니다.

1.  ( **계정 설정**)에 따라 **사용자가** 페이지에서 **사용자 추가**, 선택한 **메일로 사용자 초대**를 선택 합니다.
1.  하나 이상의 메일 주소(최대 10개)를 쉼표나 세미콜론으로 구분하여 입력합니다.
2.  **역할** 섹션에서 해당 사용자에 대한 [역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md) 을 지정합니다.
3.  **저장**을 클릭합니다.

초대한 사용자가 계정에 가입하라는 메일 초대장을 받으면 Azure AD 테넌트에서 새로운 [게스트 사용자](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) 계정이 생성됩니다. 각 사용자는 계정에 액세스하려면 먼저 초대를 수락해야 합니다.

초대를 다시 보내려면 **사용자** 페이지에서 사용자를 찾아 메일 주소(또는 **보류 중인 초대**라고 표시되는 텍스트)를 선택합니다. 그런 다음 페이지의 맨 아래쪽에서 **초대 다시 보내기**를 클릭합니다.

> [!IMPORTANT]
> 외부 사용자 참여 하도록 초대 하는 파트너 센터 계정에가 다른 사용자와 동일한 역할 및 권한을 할당할 수 있습니다. 그러나 외부 사용자는 Microsoft Store에 앱을 연결한다든지 Microsoft Store에 업로드할 패키지를 생성하는 등의 특정 작업을 Visual Studio에서 수행할 수 없습니다. 이러한 작업을 수행해야 하는 경우에는 **외부 사용자 초대** 대신 **새 사용자 만들기**를 선택합니다 (기존 Azure AD 테넌트에 이들 사용자를 추가하고 싶지 않은 경우에는 [새 테넌트 만들기](../publish/associate-azure-ad-with-dev-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account)를 선택하고 해당 테넌트에서 이들 사용자를 위한 새로운 계정을 만들 수 있음). 


### <a name="changing-a-users-directory-password"></a>사용자의 디렉터리 암호 변경

사용자 계정을 만들 때 귀하가 **암호 복구 메일**을 제공한 경우 사용자가 암호를 변경해야 하는 경우 직접 변경할 수 있습니다. 또한 사용자의 암호를 변경하기 위해 Azure AD 테넌트에서 전역 관리자 계정을 사용하여 로그인한 경우 아래 단계에 따라 사용자의 암호를 업데이트할 수 있습니다. Azure AD 테 넌 트에서 사용자의 암호도 변경 됩니다, 파트너 센터에 액세스 하는 데 사용 하는 암호와 함께 note 합니다. 

1.  ( **계정 설정**)에 따라 **사용자가** 페이지에서 편집 하려는 사용자 계정의 이름을 선택 합니다.
2.  **암호 다시 설정** 페이지의 맨 아래에 단추를 선택 합니다.
3.  임시 암호를 비롯하여 사용자에 대한 로그인 정보를 표시하는 확인 페이지가 나타납니다.

    > [!IMPORTANT]
    >  이 페이지를 나간 후에는 임시 암호에 액세스할 수 없으므로 이 정보를 인쇄하거나 복사하고 사용자에게 제공해야 합니다.

<span id="groups" />

## <a name="add-groups-to-your-partner-center-account"></a>파트너 센터 계정에 그룹 추가

파트너 센터 계정에 조직의 디렉터리에서 그룹을 추가할 수 있습니다. 이렇게 하면 그룹 구성원인 모든 사용자가 그룹에 할당된 역할과 관련해 권한을 가지고 액세스를 할 수 있게 됩니다.

### <a name="add-groups-from-your-organizations-directory"></a>조직 디렉터리의 그룹 추가

1.  파트너 센터의 오른쪽 위 모서리) (근처 기어 아이콘을 선택 하 고 **개발자 설정**를 선택 합니다. **설정** 메뉴에서 **사용자**를 선택 합니다.
2. **사용자가** 페이지에서 **추가 그룹**을 선택 합니다.
2.  표시되는 목록에서 하나 이상의 그룹을 선택합니다. 검색 상자를 사용하여 특정 그룹을 검색할 수 있습니다.
    > [!TIP]
    > 파트너 센터 계정에 추가할 그룹을 하나 이상 선택 하는 경우 동일한 역할 또는 사용자 지정 권한 집합을 할당 해야 합니다. 역할/권한이 서로 다른 그룹을 여러 개 추가하려면 각 역할 또는 사용자 지정 권한 집합에 대해 다음 단계를 반복합니다.

3.  그룹 선택이 완료되면 **선택된 항목 추가**를 클릭합니다.
4.  **역할** 섹션에서 선택한 그룹(들)에 대한 [역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md)을 지정합니다. 모든 구성원 그룹의 개별 계정과 관련 된 역할/권한에 관계 없이 그룹에 적용 권한 가진 파트너 센터 계정에 액세스할 수 없게 됩니다.
5.  **저장**을 클릭합니다.


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>조직의 디렉터리에서 새 그룹 계정 만들기 및 파트너 센터 계정에 추가

새 그룹을 파트너 센터 액세스 권한을 부여 하려는 경우 **사용자가** 섹션에서 새 그룹을 만들 수 있습니다. 참고 파트너 센터 계정 뿐만 아니라 조직의 디렉터리에서 새 그룹을 만들 수는 있습니다.

1.  ( **개발자 설정**)에 따라 **사용자가** 페이지에서 **그룹 추가**클릭 합니다.
2.  다음 페이지에서 **새 그룹**을 선택 합니다.
3.  새 그룹에 대한 표시 이름을 입력합니다.
4.  그룹에[역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md)을 지정합니다. 모든 구성원 그룹의 개별 계정과 관련 된 역할/권한에 관계 없이 그룹에 적용 권한 가진 파트너 센터 계정에 액세스할 수 없게 됩니다.
5.  목록이 나타나면 새 그룹에 할당할 사용자를 선택합니다. 검색 상자를 사용하여 특정 사용자를 검색할 수 있습니다.
6.  사용자 선택이 완료되면 **선택된 항목 추가**를 클릭하여 새 그룹에 사용자를 추가합니다.
7.  **저장**을 클릭합니다.


<span id="azure-ad-applications" />

## <a name="add-azure-ad-applications-to-your-partner-center-account"></a>파트너 센터 계정에 Azure AD 응용 프로그램 추가

응용 프로그램 또는 서비스 조직의 Azure의 일부인 허용할 수 광고 파트너 센터 계정에 액세스할 수 있습니다. 이러한 Azure AD 응용 프로그램 사용자 계정은 [Microsoft Store 서비스](../monetize/using-windows-store-services.md)에서 제공되는 REST API를 호출하는 데 사용할 수 있습니다.


### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>조직 디렉터리의 Azure AD 응용 프로그램 추가

1.  1.  파트너 센터의 오른쪽 위 모서리) (근처 기어 아이콘을 선택 하 고 **개발자 설정**를 선택 합니다. **설정** 메뉴에서 **사용자**를 선택 합니다.
2. **사용자** 페이지에서 **Azure AD 응용 프로그램 추가**를 선택합니다.
3.  표시되는 목록에서 하나 이상의 Azure AD 응용 프로그램을 선택합니다. 검색 상자를 사용하여 특정 Azure AD 응용 프로그램을 검색할 수 있습니다.
    > [!TIP]
    > 파트너 센터 계정에 추가 하려면 Azure AD 응용 프로그램을 하나 이상 선택 하는 경우 동일한 역할 또는 사용자 지정 권한 집합을 할당 해야 합니다. 역할/권한이 서로 다른 Azure AD 응용 프로그램을 여러 개 추가하려면 각 역할 또는 사용자 지정 권한 집합에 대해 다음 단계를 반복합니다.

4.  Azure AD 응용 프로그램 선택이 완료되면 **선택된 항목 추가**를 클릭합니다.
5.  **역할** 섹션에서 선택한 Azure AD 응용 프로그램에 대한 [역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md)을 지정합니다.
6.  **저장**을 클릭합니다.


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>새 Azure AD 응용 프로그램을 조직의 디렉터리에 계정 및 파트너 센터 계정에 추가

새 Azure AD 응용 프로그램 계정에 대 한 파트너 센터 액세스 권한을 부여 하려는 경우 **사용자가** 섹션에서 하나를 만들 수 있습니다. Note 파트너 센터 계정 뿐만 아니라 조직의 디렉터리에서 새 계정을 만들어집니다.

> [!TIP]
> 다른 Azure에서 해당 값을 사용 하지 않는으로 **회신 URL** 및 **앱 ID URI**대 한 유효한 주소를 입력할 수는 주로 파트너 센터 인증에이 Azure AD 응용 프로그램 사용 하 고 사용자가 직접 액세스할 필요가 경우 AD 응용 프로그램에서 디렉터리에 해당 합니다.

1.  ( **계정 설정**)에 따라 **사용자가** 페이지에서 **Azure AD 응용 프로그램**을 선택 합니다.
2.  다음 페이지에서 **새 Azure AD 응용 프로그램**을 선택 합니다.
3.  새 Azure AD 응용 프로그램에 대한 **회신 URL**을 입력합니다. 이 URL를 통해 사용자가 Azure AD 응용 프로그램에 로그인하고 이를 사용할 수 있습니다(앱 URL 또는 로그온 URL이라고도 함). **회신 URL**은 256자 이내여야 하고 디렉터리 내에서 고유해야 합니다.
4.  새 Azure AD 응용 프로그램에 대한 **앱 ID URI**를 입력합니다. 이 URI는 Azure AD에 Single Sign-On 요청을 보낼 때 제공되는 Azure AD 응용 프로그램의 논리적 식별자입니다. 디렉터리에서 각 Azure AD 응용 프로그램에 대한 **앱 ID URI**는 고유해야 하고 256자 이내여야 합니다. **앱 ID URI**에 대한 자세한 내용은 [응용 프로그램과 Azure Active Directory 통합](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#changing-the-application-registration-to-support-multi-tenant)을 참조하세요.
5.  **역할** 섹션에서 Azure AD 응용 프로그램에 대한 [역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md)을 지정합니다.
6.  **저장**을 클릭합니다.

Azure AD 응용 프로그램을 추가하거나 만든 후 **사용자** 섹션으로 돌아가 응용 프로그램 이름을 선택하면 테넌트 ID, 클라이언트 ID, 회신 URL 및 앱 ID URI를 비롯한 응용 프로그램의 설정을 검토할 수 있습니다.

> [!NOTE]
> [Microsoft Store 서비스](../monetize/using-windows-store-services.md)에서 제공하는 REST API를 사용하려는 경우, 서비스 호출을 인증하는 데 사용할 수 있는 Azure AD 액세스 토큰을 얻으려면 이 페이지에 표시된 테넌트 ID 및 클라이언트 ID 값이 필요합니다.   

<span id="manage-keys" />

### <a name="manage-keys-for-an-azure-ad-application"></a>Azure AD 응용 프로그램에 대한 키 관리

Azure AD 응용 프로그램이 Microsoft Azure AD에서 데이터를 읽고 쓸 경우 키가 필요합니다. 파트너 센터에서 정보를 편집 하 여 Azure AD 응용 프로그램에 대 한 키를 만들 수 있습니다. 더 이상 필요하지 않은 키를 제거할 수도 있습니다.

1.  ( **계정 설정**)에 따라 **사용자가** 페이지에서 Azure AD 응용 프로그램의 이름을 선택 합니다.
    > [!TIP]
    > Azure AD 응용 프로그램의 이름을 클릭하면 Azure AD 응용 프로그램에 대한 모든 활성 키가 키를 만든 날짜 및 키 만료 시기와 함께 표시됩니다. 더 이상 필요하지 않은 키를 제거하려면 **제거**를 클릭합니다.

2.  새 키를 추가 하려면 **새 키 추가**선택 합니다.
3.  **클라이언트 ID** 및 **키** 값을 보여 주는 화면이 표시됩니다.
    > [!IMPORTANT]
    > 이 페이지를 나간 후에는 정보를 액세스할 수 없으므로 이 정보를 인쇄하거나 복사해야 합니다.

4.  많은 수의 키를 만들려면 **다른 키 추가**선택 합니다.

<span id="edit" />

## <a name="edit-a-user-group-or-azure-ad-application"></a>사용자, 그룹 또는 Azure AD 응용 프로그램 편집

사용자, 그룹 및/또는 파트너 센터 계정에 Azure AD 응용 프로그램을 추가한 후 계정 정보를 변경할 수 있습니다. 

> [!IMPORTANT]
> [역할 또는 권한](set-custom-permissions-for-account-users.md) 변경에는 파트너 센터 액세스 영향을 미칩니다. 다른 모든 변경 (예: Azure AD 응용 프로그램에 대 한 사용자의 이름 또는 그룹 구성원 또는 회신 URL 및 앱 ID URI를 변경)은 조직의 Azure AD 테 넌 트에도 파트너 센터 계정에 반영 됩니다. 

1.  ( **계정 설정**)에 따라 **사용자가** 페이지에서 사용자, 그룹 또는 편집 하려는 Azure AD 응용 프로그램 계정의 이름을 선택 합니다.
2.  원하는 대로 변경 하면 됩니다. 편집할 수 있는 항목은 다음과 같습니다.
    -   **사용자**에서 사용자의 이름, 성 또는 사용자 이름을 편집할 수 있습니다. **그룹 구성원** 섹션에서 그룹을 선택 또는 선택 취소하여 그룹 구성원을 업데이트할 수도 있습니다.
    -   **그룹**에서 그룹의 이름을 편집할 수 있습니다. (그룹 구성원을 업데이트 하려면 그룹에서 추가 또는 제거할 사용자를 편집하고 **그룹 구성원** 섹션을 변경합니다.)
    -   **Azure AD 응용 프로그램**에서 **회신 URL** 또는 **앱 ID URI**에 대한 새로운 값을 입력할 수 있습니다.
    기억 조직의 디렉터리 파트너 센터 계정에도에 이러한 변경 사항을 적용 됩니다.
3.  파트너 센터 액세스와 관련 된 변경 내용에 대 한 선택 하거나 선택 취소를 적용 하 고, **권한 사용자 지정을** 선택 하 고 원하는 대로 변경 합니다. 이러한 변경 내용은 파트너 센터만 영향을 미치고 액세스 하 고 조직의 Azure AD 테 넌 트 내에서 어떤 권한도 변경 되지 것입니다.
3.  **저장**을 클릭합니다.


## <a name="view-history-for-account-users"></a>계정 사용자의 기록 보기

계정 소유자는 계정에 추가한 추가 사용자의 자세한 검색 기록을 확인할 수 있습니다.

( **계정 설정**)에 따라 **사용자가** 페이지에서 해당 검색 기록을 검토할 하려는 사용자에 대 한 **마지막 작업** 아래에 표시 된 링크를 선택 합니다. 지난 30일 동안 사용자가 방문한 모든 페이지의 URL을 볼 수 있습니다.

<span id="remove" />

## <a name="remove-users-groups-and-azure-ad-applications"></a>사용자, 그룹 및 Azure AD 응용 프로그램 제거

파트너 센터 계정에서 사용자, 그룹 또는 Azure AD 응용 프로그램을 제거 하려면 **사용자가** 페이지에서 해당 이름으로 표시 되는 **제거** 링크를 선택 합니다. 제거 하려는 경우를 확인 한 후 해당 사용자, 그룹 또는 Azure AD 응용 프로그램 더 이상 됩니다 (추가 하지 않으면이 나중에 다시) 파트너 센터 계정에 액세스할 수 없습니다.

> [!IMPORTANT]
> 사용자, 그룹 또는 Azure AD 응용 프로그램 제거는 더 이상 액세스 권한이 파트너 센터 계정에 의미 합니다. 사용자, 그룹 또는 Azure AD 응용 프로그램을 조직의 디렉터리에서 삭제하지 **않습니다**.

 

