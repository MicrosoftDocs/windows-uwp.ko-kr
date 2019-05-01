---
Description: 사용자, 그룹 및 파트너 센터 계정에 Azure AD 응용 프로그램을 추가할 수 있습니다.
title: 사용자, 그룹 및 파트너 센터 계정에 Azure AD 응용 프로그램 추가
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, azure ad 응용 프로그램, aad, 사용자, 그룹, 여러 사용자, 다중 사용자
ms.localizationpriority: medium
ms.openlocfilehash: ddbe47d94e17db0d272aedcff56df95fccf3434d
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63787285"
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-partner-center-account"></a>사용자, 그룹 및 파트너 센터 계정에 Azure AD 응용 프로그램 추가

합니다 **사용자** 부분 [파트너 센터](https://partner.microsoft.com/dashboard) (아래 **계정 설정**) Azure Active Directory를 사용 하 여 파트너 센터 계정에 사용자를 추가할 수 있습니다. 각 사용자에게 계정에 대한 사용 권한을 정의하는 역할(또는 사용자 지정 권한 집합)이 할당됩니다. 추가할 수도 있습니다 [사용자 그룹](#groups) 하 고 [Azure AD 응용 프로그램](#azure-ad-applications) 파트너 센터 계정에 대 한 액세스 권한을 부여 합니다.

사용자가 계정에 추가되고 나면 [계정 세부 정보 편집](#edit), [역할 및 사용 권한](set-custom-permissions-for-account-users.md), 변경 또는 [사용자 제거](#remove)가 가능합니다.

> [!IMPORTANT]
> 사용자 계정에 추가 하려면 먼저 [조직의 Azure Active Directory 테 넌 트를 사용 하 여 파트너 센터 계정을 연결](associate-azure-ad-with-partner-center.md)합니다. 

사용자를 추가할 때 지정 하 여 파트너 센터 계정에 대 한 액세스를 지정 해야 합니다는 [역할 또는 사용자 지정 사용 권한 집합이](set-custom-permissions-for-account-users.md)합니다. 

모든 파트너 센터 사용자 (그룹 및 Azure AD 응용 프로그램 포함)에 활성 계정이 있어야 하는 염두에서에 둡니다 [파트너 센터 계정과 사용 하 여 연결 된 Azure AD 테 넌](associate-azure-ad-with-partner-center.md)합니다. 사용자 관리는 한 번에 한 테넌트에서 완료되며 사용자를 추가 또는 편집하려는 테넌트에 대한 관리자 계정으로 로그인해야 합니다. 파트너 센터에서 새 사용자 만들기, 로그인 및 파트너 센터에서 사용자의 이름을 변경 하는 동일 하 게 변경 조직의 Azure AD 테 넌 트의 Azure AD 테 넌 트에서 해당 사용자에 대 한 계정을 만들 수도 됩니다.

> [!NOTE]
> 조직에서 사용 하는 경우 [디렉터리 통합](https://go.microsoft.com/fwlink/p/?LinkID=724033) Azure AD 사용 하 여 온-프레미스 디렉터리 서비스와 동기화 하려면 수 없게 파트너 센터에서 새 사용자, 그룹 또는 Azure AD 응용 프로그램을 만듭니다. 사용자 (또는 온-프레미스 디렉터리에서 다른 관리자)를 확인 하 여 파트너 센터에 추가할 수 전에 온-프레미스 디렉터리에서 직접 작성 해야 합니다.


<span id="users" />

## <a name="add-users-to-your-partner-center-account"></a>파트너 센터 계정에 사용자를 추가 합니다.

파트너 센터 계정에 사용자를 추가 하려면로 이동 합니다 **사용자** 페이지에서 **계정 설정** 선택한 **사용자를 추가 합니다.** 작업하려는 Azure AD 테넌트에 대한 관리자 계정을 사용하여 로그인해야 합니다. 

### <a name="add-existing-users"></a>기존 사용자 추가 

이미 조직의 테 넌 트에 존재 하 고 파트너 센터 계정에 액세스 권한을 부여할 사용자를 선택할 수 있습니다. 

<span id="from-directory" />

1.  파트너 센터의 오른쪽 위 모서리) (근처의 기어 아이콘을 선택 하 고 선택한 **개발자 설정을**합니다. 에 **설정을** 메뉴에서 **사용자**합니다.
2.  **사용자** 페이지에서 **사용자 추가**를 선택합니다. 
3.  표시되는 목록에서 하나 이상의 사용자를 선택합니다. 검색 상자를 사용하여 특정 사용자를 검색할 수 있습니다.
    > [!TIP]
    > 파트너 센터 계정에 추가 하려면 둘 이상의 사용자를 선택 하면 동일한 역할 또는 사용자 지정 사용 권한 집합을 할당 해야 합니다. 역할/권한이 서로 다른 사용자를 여러 명 추가하려면 각 역할 또는 사용자 지정 권한 집합에 대해 다음 단계를 반복합니다.
4.  사용자 선택이 완료되면 **선택된 항목 추가**를 클릭합니다.
5.  **역할** 섹션에서 선택한 사용자에 대한 [역할 또는 사용자 지정 권한 집합](set-custom-permissions-for-account-users.md) 을 지정합니다.
6.  **저장**을 클릭합니다.

### <a name="additional-methods-for-adding-users"></a>사용자를 추가하는 추가 방법

도 있는 관리자 계정으로 로그인 했으면 [전역 관리자](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) 에서 작업 하는 Azure AD 테 넌 트에 대 한 권한을, 사용자가 파트너 센터 계정에 추가 하는 추가 옵션을 사용 하도록 해야 합니다. 다음 중 하나를 선택해야 합니다.

-   **기존 사용자를 추가**: 이미 조직의 디렉터리에 존재 하며 액세스할 수 있도록 파트너 센터 계정에 위에서 설명한 방법을 사용 하 여 사용자를 선택 합니다.
-   **새 사용자를 만들**: 모두 조직의 디렉터리에 추가할 새 사용자 계정을 만들고 파트너 센터 계정
-   **외부 사용자 초대**: 현재 조직의 디렉터리에 없는 사용자에 대 한 전자 메일 초대를 보냅니다. 파트너 센터 계정에 및 새 액세스에 초대 될 수 있습니다 [게스트 사용자](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) 계정에 Azure AD 테 넌 트에서 생성 됩니다.

<span id="new-user" />

### <a name="create-new-users"></a>새 사용자 만들기

> [!IMPORTANT]
> 새로운 사용자를 만들려면 반드시 Azure AD 테넌트에서 전역 관리자 계정으로 로그인을 해야 합니다.

1.  **사용자** 페이지 (아래 **계정 설정**)을 선택 **사용자를 추가**를 선택한 **새 사용자를 만들**합니다.
2.  새 사용자의 이름, 성 및 사용자 이름을 입력합니다.
3.  조직 디렉터리에서 새 사용자에게 [전역 관리자 계정](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)을 부여하려면 **Azure AD에서 이 사용자를 전역 관리자로 지정하여 모든 디렉터리 리소스에 대한 모든 권한을 부여** 상자를 선택합니다. 이렇게 하면 해당 사용자에게 회사 Azure AD의 모든 관리 기능에 대한 모든 권한이 부여됩니다. 추가 하 고 조직의 디렉터리에서 사용자를 관리할 수 있습니다 (에 속하지 않은 파트너 센터 계정을 적절 한 부여 하지 않는 한, 하지만 [역할 및 사용 권한을](set-custom-permissions-for-account-users.md)). 이 상자를 선택하면 사용자의 **암호 복구 메일**을 제공해야 합니다.
4.  **Azure AD에서 이 사용자를 전역 관리자로 지정** 확인란을 선택한 경우 사용자가 암호를 복구해야 하는 경우 사용할 수 있는 메일 주소를 입력합니다.
5.  **그룹 구성원** 섹션에서 새 사용자가 속하게 될 그룹을 선택합니다.
6.  **역할** 섹션에서 해당 사용자에 대한 [역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md) 을 지정합니다.
7.  **저장**을 클릭합니다.
8.  확인 페이지에 임시 암호를 비롯하여 새 사용자의 로그인 정보가 표시됩니다. 이 페이지를 나간 후에는 임시 암호에 액세스할 수 없으므로 이 정보를 메모하고 새 사용자에게 제공해야 합니다.


<span id="email" />

### <a name="invite-outside-users"></a>외부 사용자 초대

> [!IMPORTANT]
> 외부 사용자를 초대하려면 반드시 Azure AD 테넌트에서 전역 관리자 계정으로 로그인을 해야 합니다.

1.  **사용자** 페이지 (아래 **계정 설정**)을 선택 **사용자를 추가**를 선택한 **전자 메일을 통해 사용자를 초대**합니다.
1.  하나 이상의 메일 주소(최대 10개)를 쉼표나 세미콜론으로 구분하여 입력합니다.
2.  **역할** 섹션에서 해당 사용자에 대한 [역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md) 을 지정합니다.
3.  **저장**을 클릭합니다.

초대한 사용자가 계정에 가입하라는 메일 초대장을 받으면 Azure AD 테넌트에서 새로운 [게스트 사용자](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) 계정이 생성됩니다. 각 사용자는 계정에 액세스하려면 먼저 초대를 수락해야 합니다.

초대를 다시 보내려면 **사용자** 페이지에서 사용자를 찾아 메일 주소(또는 **보류 중인 초대**라고 표시되는 텍스트)를 선택합니다. 그런 다음 페이지의 맨 아래쪽에서 **초대 다시 보내기**를 클릭합니다.

> [!IMPORTANT]
> 외부 사용자에 초대 하는 파트너 센터 계정이 다른 사용자로 동일한 역할과 권한을 할당할 수 있습니다. 그러나 외부 사용자는 Microsoft Store에 앱을 연결한다든지 Microsoft Store에 업로드할 패키지를 생성하는 등의 특정 작업을 Visual Studio에서 수행할 수 없습니다. 이러한 작업을 수행해야 하는 경우에는 **외부 사용자 초대** 대신 **새 사용자 만들기**를 선택합니다 (기존 Azure AD 테넌트에 이들 사용자를 추가하고 싶지 않은 경우에는 [새 테넌트 만들기](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account)를 선택하고 해당 테넌트에서 이들 사용자를 위한 새로운 계정을 만들 수 있음). 


### <a name="changing-a-users-directory-password"></a>사용자의 디렉터리 암호 변경

사용자 계정을 만들 때 귀하가 **암호 복구 메일**을 제공한 경우 사용자가 암호를 변경해야 하는 경우 직접 변경할 수 있습니다. 또한 사용자의 암호를 변경하기 위해 Azure AD 테넌트에서 전역 관리자 계정을 사용하여 로그인한 경우 아래 단계에 따라 사용자의 암호를 업데이트할 수 있습니다. 이 Azure AD 테 넌 트에서 사용자의 암호를 변경, 암호와 함께 사용 하 여 파트너 센터 액세스는 note 합니다. 

1.  **사용자** 페이지 (아래 **계정 설정**)을 편집 하려는 사용자 계정의 이름을 선택 합니다.
2.  선택 된 **암호 재설정** 페이지의 맨 위에 있는 단추입니다.
3.  임시 암호를 비롯하여 사용자에 대한 로그인 정보를 표시하는 확인 페이지가 나타납니다.

    > [!IMPORTANT]
    >  이 페이지를 나간 후 임시 암호에 액세스 할 수 없으므로 사용할 인쇄 하거나이 정보를 복사 하는 사용자에 게 제공 하는 일을 해야 합니다.

<span id="groups" />

## <a name="add-groups-to-your-partner-center-account"></a>파트너 센터 계정에 그룹 추가

파트너 센터 계정에 조직의 디렉터리에서 그룹을 추가할 수 있습니다. 이렇게 하면 그룹 구성원인 모든 사용자가 그룹에 할당된 역할과 관련해 권한을 가지고 액세스를 할 수 있게 됩니다.

### <a name="add-groups-from-your-organizations-directory"></a>조직 디렉터리의 그룹 추가

1.  파트너 센터의 오른쪽 위 모서리) (근처의 기어 아이콘을 선택 하 고 선택한 **개발자 설정을**합니다. 에 **설정을** 메뉴에서 **사용자**합니다.
2. **사용자가** 페이지에서 **그룹 추가**합니다.
2.  표시되는 목록에서 하나 이상의 그룹을 선택합니다. 검색 상자를 사용하여 특정 그룹을 검색할 수 있습니다.
    > [!TIP]
    > 파트너 센터 계정에 추가 하려면 둘 이상의 그룹을 선택 하면 동일한 역할 또는 사용자 지정 사용 권한 집합을 할당 해야 합니다. 역할/권한이 서로 다른 그룹을 여러 개 추가하려면 각 역할 또는 사용자 지정 권한 집합에 대해 다음 단계를 반복합니다.

3.  그룹 선택이 완료되면 **선택된 항목 추가**를 클릭합니다.
4.  **역할** 섹션에서 선택한 그룹(들)에 대한 [역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md)을 지정합니다. 그룹의 모든 멤버는 역할/연관 된 사용 권한을 개별 계정에 관계 없이 그룹에 적용할 권한 사용 하 여 파트너 센터 계정에 액세스할 수 없게 됩니다.
5.  **저장**을 클릭합니다.


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>조직의 디렉터리에서 새 그룹 계정을 만들고 파트너 센터 계정에 추가

새 그룹을 만들면 새 그룹에 파트너 센터 액세스 권한을 부여 하려는 경우는 **사용자** 섹션입니다. 새 그룹 뿐 아니라 파트너 센터 계정에서에서 조직의 디렉터리에 만들어졌는지 참고 합니다.

1.  **사용자** 페이지 (아래 **개발자 설정**)를 클릭 **그룹을 추가**.
2.  다음 페이지에서 선택 **새 그룹**합니다.
3.  새 그룹에 대한 표시 이름을 입력합니다.
4.  그룹에[역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md)을 지정합니다. 그룹의 모든 멤버는 역할/연관 된 사용 권한을 개별 계정에 관계 없이 그룹에 적용할 권한 사용 하 여 파트너 센터 계정에 액세스할 수 없게 됩니다.
5.  목록이 나타나면 새 그룹에 할당할 사용자를 선택합니다. 검색 상자를 사용하여 특정 사용자를 검색할 수 있습니다.
6.  사용자 선택이 완료되면 **선택된 항목 추가**를 클릭하여 새 그룹에 사용자를 추가합니다.
7.  **저장**을 클릭합니다.


<span id="azure-ad-applications" />

## <a name="add-azure-ad-applications-to-your-partner-center-account"></a>파트너 센터 계정에 Azure AD 응용 프로그램 추가

응용 프로그램 또는 조직의 Azure의 일부인 서비스를 허용할 수 있습니다 AD 파트너 센터 계정에 액세스할 수 있습니다. 이러한 Azure AD 응용 프로그램 사용자 계정은 [Microsoft Store 서비스](../monetize/using-windows-store-services.md)에서 제공되는 REST API를 호출하는 데 사용할 수 있습니다.


### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>조직 디렉터리의 Azure AD 응용 프로그램 추가

1.  1.  파트너 센터의 오른쪽 위 모서리) (근처의 기어 아이콘을 선택 하 고 선택한 **개발자 설정을**합니다. 에 **설정을** 메뉴에서 **사용자**합니다.
2. **사용자** 페이지에서 **Azure AD 응용 프로그램 추가**를 선택합니다.
3.  표시되는 목록에서 하나 이상의 Azure AD 응용 프로그램을 선택합니다. 검색 상자를 사용하여 특정 Azure AD 응용 프로그램을 검색할 수 있습니다.
    > [!TIP]
    > 파트너 센터 계정에 추가 하려면 둘 이상의 Azure AD 응용 프로그램을 선택 하면 동일한 역할 또는 사용자 지정 사용 권한 집합을 할당 해야 합니다. 역할/권한이 서로 다른 Azure AD 응용 프로그램을 여러 개 추가하려면 각 역할 또는 사용자 지정 권한 집합에 대해 다음 단계를 반복합니다.

4.  Azure AD 응용 프로그램 선택이 완료되면 **선택된 항목 추가**를 클릭합니다.
5.  **역할** 섹션에서 선택한 Azure AD 응용 프로그램에 대한 [역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md)을 지정합니다.
6.  **저장**을 클릭합니다.


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>새 Azure AD 응용 프로그램에서 조직 디렉터리의 계정 및 파트너 센터 계정에 추가

브랜드 새 Azure AD 응용 프로그램 계정에 파트너 센터 액세스 권한을 부여 하려는 경우에 하나를 만들 수 있습니다 합니다 **사용자** 섹션입니다. 파트너 센터 계정 뿐 아니라 조직의 디렉터리에 새 계정 만들기는이 note 합니다.

> [!TIP]
> 기본적으로 사용 하는이 Azure AD 응용 프로그램 파트너 센터 인증 하 고 사용자가 직접 액세스할 필요가 하는 경우에 대 한 유효한 주소를 입력할 수 있습니다 합니다 **회신 URL** 하 고 **앱 ID URI**있으면 해당 값으로는 디렉터리에서 다른 Azure AD 응용 프로그램에서 사용 되지 않습니다.

1.  **사용자** 페이지 (아래 **계정 설정**)을 선택 **Azure AD 응용 프로그램을 추가**합니다.
2.  다음 페이지에서 선택 **새 Azure AD 응용 프로그램**합니다.
3.  새 Azure AD 응용 프로그램에 대한 **회신 URL**을 입력합니다. 이 URL를 통해 사용자가 Azure AD 응용 프로그램에 로그인하고 이를 사용할 수 있습니다(앱 URL 또는 로그온 URL이라고도 함). **회신 URL**은 256자 이내여야 하고 디렉터리 내에서 고유해야 합니다.
4.  새 Azure AD 응용 프로그램에 대한 **앱 ID URI**를 입력합니다. 이 URI는 Azure AD에 Single Sign-On 요청을 보낼 때 제공되는 Azure AD 응용 프로그램의 논리적 식별자입니다. 디렉터리에서 각 Azure AD 응용 프로그램에 대한 **앱 ID URI**는 고유해야 하고 256자 이내여야 합니다. **앱 ID URI**에 대한 자세한 내용은 [응용 프로그램과 Azure Active Directory 통합](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#changing-the-application-registration-to-support-multi-tenant)을 참조하세요.
5.  **역할** 섹션에서 Azure AD 응용 프로그램에 대한 [역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md)을 지정합니다.
6.  **저장**을 클릭합니다.

Azure AD 응용 프로그램을 추가하거나 만든 후 **사용자** 섹션으로 돌아가 응용 프로그램 이름을 선택하면 테넌트 ID, 클라이언트 ID, 회신 URL 및 앱 ID URI를 비롯한 응용 프로그램의 설정을 검토할 수 있습니다.

> [!NOTE]
> [Microsoft Store 서비스](../monetize/using-windows-store-services.md)에서 제공하는 REST API를 사용하려는 경우, 서비스 호출을 인증하는 데 사용할 수 있는 Azure AD 액세스 토큰을 얻으려면 이 페이지에 표시된 테넌트 ID 및 클라이언트 ID 값이 필요합니다.   

<span id="manage-keys" />

### <a name="manage-keys-for-an-azure-ad-application"></a>Azure AD 응용 프로그램에 대한 키 관리

Azure AD 응용 프로그램이 Microsoft Azure AD에서 데이터를 읽고 쓸 경우 키가 필요합니다. 파트너 센터에서 해당 정보를 편집 하 여 Azure AD 응용 프로그램에 대 한 키를 만들 수 있습니다. 더 이상 필요하지 않은 키를 제거할 수도 있습니다.

1.  **사용자** 페이지 (아래 **계정 설정**), Azure AD 응용 프로그램의 이름을 선택 합니다.
    > [!TIP]
    > Azure AD 응용 프로그램의 이름을 클릭 하면, 만든 키에 만료 되는 날짜를 포함 하 여 Azure AD 응용 프로그램에 대 한 활성 키를 모두 표시 됩니다. 더 이상 필요하지 않은 키를 제거하려면 **제거**를 클릭합니다.

2.  새 키를 추가 하려면 선택 **새 키 추가**합니다.
3.  **클라이언트 ID** 및 **키** 값을 보여 주는 화면이 표시됩니다.
    > [!IMPORTANT]
    > 이 페이지를 나간 후에 다시 액세스할 수 없습니다 수를 인쇄 하거나이 정보를 복사 해야 합니다.

4.  많은 수의 키를 만들려는 경우 선택할 **다른 키를 추가**합니다.

<span id="edit" />

## <a name="edit-a-user-group-or-azure-ad-application"></a>사용자, 그룹 또는 Azure AD 응용 프로그램 편집

사용자, 그룹 및/또는 Azure AD 응용 프로그램 파트너 센터 계정에 추가한 후 해당 계정 정보를 변경할 수 있습니다. 

> [!IMPORTANT]
> 변경 내용을 [역할 또는 사용 권한을](set-custom-permissions-for-account-users.md) 파트너 센터 액세스에만 적용 됩니다. 조직의 Azure AD 테 넌 트에도 파트너 센터 계정의 경우 처럼 (예: Azure AD 응용 프로그램에 대 한 사용자의 이름 또는 그룹 멤버 자격 또는 회신 URL 및 앱 ID URI를 변경) 다른 모든 변경 내용이 반영 됩니다. 

1.  **사용자** 페이지 (아래 **계정 설정**), 사용자, 그룹 또는 편집 하려는 Azure AD 응용 프로그램 계정 이름을 선택 합니다.
2.  원하는 대로 변경 하면 됩니다. 편집할 수 있는 항목은 다음과 같습니다.
    -   **사용자**에서 사용자의 이름, 성 또는 사용자 이름을 편집할 수 있습니다. **그룹 구성원** 섹션에서 그룹을 선택 또는 선택 취소하여 그룹 구성원을 업데이트할 수도 있습니다.
    -   **그룹**에서 그룹의 이름을 편집할 수 있습니다. (그룹 구성원을 업데이트 하려면 그룹에서 추가 또는 제거할 사용자를 편집하고 **그룹 구성원** 섹션을 변경합니다.)
    -   **Azure AD 응용 프로그램**에서 **회신 URL** 또는 **앱 ID URI**에 대한 새로운 값을 입력할 수 있습니다.
    조직의 디렉터리에도 파트너 센터 계정에서 이러한 변경 된 내용이 기억 합니다.
3.  파트너 센터 액세스와 관련 된 변경 내용에 대 한 선택 하거나 역할을 적용 하거나 선택 취소 **사용 권한 사용자 지정** 원하는 변경 내용을 확인 합니다. 이러한 변경 내용은 파트너 센터에만 영향을 줄에 액세스 하 고 조직의 Azure AD 테 넌 트 내에서 모든 사용 권한을 변경 되지 것입니다.
3.  **저장**을 클릭합니다.


## <a name="view-history-for-account-users"></a>계정 사용자의 기록 보기

계정 소유자는 계정에 추가한 추가 사용자의 자세한 검색 기록을 확인할 수 있습니다.

에 **사용자** 페이지 (아래 **계정 설정**), 아래에 표시 된 링크를 선택 **마지막 활동** 검색 기록을 검토 하려는 사용자에 대 한 합니다. 지난 30일 동안 사용자가 방문한 모든 페이지의 URL을 볼 수 있습니다.

<span id="remove" />

## <a name="remove-users-groups-and-azure-ad-applications"></a>사용자, 그룹 및 Azure AD 응용 프로그램 제거

파트너 센터 계정에서 사용자, 그룹 또는 Azure AD 응용 프로그램을 제거 하려면 선택 합니다 **제거** 에 이름별으로 표시 되는 링크는 **사용자** 페이지입니다. 제거할 것인지를 확인 한 후 해당 사용자, 그룹 또는 Azure AD 응용 프로그램 더 이상 (추가 하지 않으면 나중에 다시) 파트너 센터 계정에 액세스할 수 없습니다.

> [!IMPORTANT]
> 파트너 센터 계정에 대 한 액세스를 더 이상 됩니다 의미 사용자, 그룹 또는 Azure AD 응용 프로그램을 제거 합니다. 사용자, 그룹 또는 Azure AD 응용 프로그램을 조직의 디렉터리에서 삭제하지 **않습니다**.

 

