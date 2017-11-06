---
author: jnHs
Description: "개발자 센터 계정에 사용자, 그룹, Azure AD 응용 프로그램을 추가할 수 있습니다."
title: "개발자 센터 계정에 사용자, 그룹, Azure AD 응용 프로그램을 추가"
ms.author: wdg-dev-content
ms.date: 07/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 32f62d6022d075e71ce6bcc15e1603a2e11a68a5
ms.sourcegitcommit: 23cda44f10059bcaef38ae73fd1d7c8b8330c95e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/19/2017
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-dev-center-account"></a>개발자 센터 계정에 사용자, 그룹, Azure AD 응용 프로그램을 추가

개발자 센터의 [사용자 관리](manage-account-users.md) 섹션에서 Azure Active Directory를 사용하여 개발자 센터 계정에 사용자를 추가할 수 있습니다. 각 사용자에게 계정에 대한 사용 권한을 정의하는 역할(또는 사용자 지정 권한 집합)이 할당됩니다. [사용자 그룹](#groups) 및 [Azure AD 응용 프로그램](#azure-ad-applications)을 추가하여 개발자 센터 계정에 대한 액세스 권한을 부여할 수도 있습니다.

사용자가 계정에 추가되고 나면 [계정 세부 정보 편집](#edit), [역할 및 사용 권한](set-custom-permissions-for-account-users.md), 변경 또는 [사용자 제거](#remove)가 가능합니다.

> [!IMPORTANT]
> 계정에 사용자를 추가하려면 먼저 [개발자 센터 계정을 조직의 Azure Active Directory 테넌트와 연결](associate-azure-ad-with-dev-center.md)해야 합니다. 

사용자를 추가할 때 다음 사항을 고려해야 합니다. (이들은 그룹 및 Azure AD 응용 프로그램을 비롯하여 개별 사용자에게 적용됩니다.)

-   모든 개발자 센터 사용자는 조직의 디렉터리에 활성 계정이 있어야 합니다. 개발자 센터에서 새 사용자를 생성하면 조직의 Azure AD 테넌트에서 해당 사용자에 대한 계정이 함께 생성됩니다.
-   개발자 센터의 사용자 이름을 [변경](#edit)하면 조직의 Azure AD 테넌트도 똑같이 변경됩니다.
-   사용자를 추가할 때는 [역할 또는 사용자 지정 권한 집합](set-custom-permissions-for-account-users.md)을 할당하여 개발자 센터 계정에 대한 액세스를 지정해야 합니다. 

> [!NOTE]
> 조직에서 [디렉터리 통합](http://go.microsoft.com/fwlink/p/?LinkID=724033)을 사용하여 온-프레미스 디렉터리 서비스와 Azure AD를 동기화하면 개발자 센터에서 새 사용자, 그룹 또는 Azure AD 응용 프로그램을 만들 수 없습니다. 개발자 센터에서 새 사용자, 그룹 또는 Azure AD 응용 프로그램을 보고 추가할 수 있으려면 귀하 또는 온-프레미스 디렉터리의 다른 관리자가 온-프레미스 디렉터리에서 직접 만들어야 합니다.


<span id="users" />
## <a name="add-users-to-your-dev-center-account"></a>개발자 센터 계정에 사용자 추가

다음 방법 중 하나로 개발자 센터 계정에 개별 사용자를 추가할 수 있습니다.
-   조직 디렉터리에 이미 존재하는 사용자 추가
-   조직 디렉터리와 개발자 센터 계정 모두에 추가할 새로운 사용자 계정 만들기
-   현재 조직의 디렉터리에 없는 기존 사용자 추가


<span id="from-directory" />
### <a name="add-users-from-your-organizations-directory"></a>조직 디렉터리의 사용자 추가

1.  **사용자 관리** 페이지에서 **사용자 추가**를 클릭합니다.
2.  표시되는 목록에서 하나 이상의 사용자를 선택합니다. 검색 상자를 사용하여 특정 사용자를 검색할 수 있습니다.
    > [!TIP]
    > 개발자 센터 계정에 추가할 사용자를 한 명 이상 선택하는 경우에는 동일한 역할 또는 사용자 지정 권한 집합을 할당해야 합니다. 역할/권한이 서로 다른 사용자를 여러 명 추가하려면 각 역할 또는 사용자 지정 권한 집합에 대해 다음 단계를 반복합니다.

3.  사용자 선택이 완료되면 **선택된 항목 추가**를 클릭합니다.
4.  **역할** 섹션에서 선택한 사용자에 대한 [역할 또는 사용자 지정 권한 집합](set-custom-permissions-for-account-users.md) 을 지정합니다.
5.  **저장**을 클릭합니다.


<span id="new-user" />
### <a name="create-a-new-user-account-in-your-organizations-directory-and-add-them-to-your-dev-center-account"></a>조직 디렉터리에 새로운 사용자 계정을 만들고 이를 개발자 센터 계정에 추가

Azure AD 테넌트에서 새 계정에 대한 개발자 센터 액세스 권한을 부여하려는 경우, **새 사용자**를 클릭하여 **사용자 관리** 섹션에서 새 사용자 계정을 만들 수 있습니다. 

> [!IMPORTANT]
> 디렉터리에 새로운 사용자를 추가하려면 반드시 Azure AD 테넌트에서 전역 관리자 계정으로 로그인을 해야 합니다.

조직 디렉터리에서 새 사용자에게 [전역 관리자 계정](http://go.microsoft.com/fwlink/p/?LinkId=746654)을 부여하려면 **Azure AD에서 이 사용자를 전역 관리자로 지정하여 모든 디렉터리 리소스에 대한 모든 권한을 부여** 상자를 선택합니다. 이렇게 하면 해당 사용자에게 회사 Azure AD의 모든 관리 기능에 대한 모든 권한이 부여됩니다. 조직의 디렉터리에서 사용자를 추가하고 관리할 수 있지만, 개발자 센터에서는 적절한 [역할/권한](set-custom-permissions-for-account-users.md)을 계정에 부여하지 않는 한 해당되지 않습니다. 이 상자를 선택하면 사용자에게 **암호 복구 메일**을 보내야 합니다.

1.  **사용자 관리** 페이지에서 **사용자 추가**를 클릭합니다.
2.  다음 페이지에서 **새 사용자**를 클릭합니다.
3.  조직 디렉터리에서 새 계정을 생성하고 개발자 센터 계정에 해당 사용자를 추가할 수 있도록 **Azure AD에 추가** 라디오 단추를 선택했는지 확인합니다. 
4.  새 사용자의 이름, 성 및 사용자 이름을 입력합니다.
5.  사용자가 암호를 복구해야 할 경우 사용자가 사용할 수 있는 메일을 입력합니다. 이 옵션은 **Azure AD에서 이 사용자를 전역 관리자로 지정** 상자를 선택한 경우에만 필수입니다.
6.  **그룹 구성원** 섹션에서 새 사용자가 속하게 될 그룹을 선택합니다.
7.  **역할** 섹션에서 해당 사용자에 대한 [역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md) 을 지정합니다.
8.  **저장**을 클릭합니다.
9.  확인 페이지에 임시 암호를 비롯하여 새 사용자의 로그인 정보가 표시됩니다. 이 페이지를 나간 후에는 임시 암호에 액세스할 수 없으므로 이 정보를 메모하고 새 사용자에게 제공해야 합니다.


<span id="email" />
### <a name="add-a-user-from-outside-of-your-azure-ad-tenant-to-your-dev-center-account-and-your-directory"></a>Azure AD 테넌트 외부의 사용자를 개발자 센터 계정 및 디렉터리에 추가

현재 Azure AD 테넌트에 포함되어 있지 않은 사용자를 초대하려면 다음 단계를 수행 합니다. 사용자가 메일로 계정을 가입하라는 초대를 받으면 Azure AD 테넌트에서 새로운 [게스트 사용자](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) 계정이 생성됩니다. 

1.  **사용자 관리** 페이지에서 **사용자 추가**를 클릭합니다.
2.  다음 페이지에서 **새 사용자**를 클릭합니다.
3.  **메일로 사용자 초대** 라디오 단추를 선택합니다.
3.  하나 이상의 메일 주소(최대 10개)를 쉼표나 세미콜론으로 구분하여 입력합니다.
4.  **역할** 섹션에서 해당 사용자에 대한 [역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md) 을 지정합니다.
6.  **저장**을 클릭합니다.

초대 받은 사용자는 개발자 센터 계정에 액세스하라는 초대가 포함된 메일을 받게 됩니다. 각 사용자는 계정에 액세스하려면 먼저 초대를 수락해야 합니다.

초대를 다시 보내려면 **사용자 관리** 페이지에서 사용자를 찾아 메일 주소(또는 **보류 중인 초대**라고 표시되는 텍스트)를 클릭하여 계정을 편집합니다. 그런 다음 페이지의 맨 아래쪽에서 **초대 다시 보내기**를 클릭합니다.


### <a name="changing-a-users-directory-password"></a>사용자의 디렉터리 암호 변경

개발자 센터 계정에 추가한 사용자 계정용 암호를 변경해야 하는 경우 **사용자 관리** 섹션에서 수행할 수 있습니다. 이렇게 하면 개발자 센터에 액세스하기 위해 사용하는 암호화 함께 Azure AD 테넌트의 사용자 암호가 바뀌게 됩니다. 

사용자 계정을 만들 때 **암호 복구 메일**을 입력한 경우 고유의 암호로 재설정될 수 있습니다. 다음 단계에 따라 사용자 암호를 업데이트할 수도 있습니다.

1.  **사용자 관리** 페이지에서 편집하려는 사용자 계정의 이름을 클릭합니다.
2.  페이지 맨 아래에서 **암호 재설정** 단추를 클릭합니다.
3.  임시 암호를 비롯하여 사용자에 대한 로그인 정보를 표시하는 확인 페이지가 나타납니다.
    > [!IMPORTANT]
    >  이 페이지를 나간 후에는 임시 암호에 액세스할 수 없으므로 이 정보를 인쇄하거나 복사하고 사용자에게 제공해야 합니다.

<span id="groups" />
## <a name="add-groups-to-your-dev-center-account"></a>개발자 센터 계정에 그룹 추가

조직 디렉터리의 그룹을 개발자 센터 계정으로 추가할 수 있습니다. 이렇게 하면 그룹 구성원인 모든 사용자가 그룹에 할당된 역할과 관련해 권한을 가지고 액세스를 할 수 있게 됩니다.

### <a name="add-groups-from-your-organizations-directory"></a>조직 디렉터리의 그룹 추가

1.  **사용자 관리** 페이지에서 **그룹 추가**를 클릭합니다.
2.  표시되는 목록에서 하나 이상의 그룹을 선택합니다. 검색 상자를 사용하여 특정 그룹을 검색할 수 있습니다.
    > [!TIP]
    > 개발자 센터 계정에 추가할 그룹을 하나 이상 선택하는 경우에는 동일한 역할 또는 사용자 지정 권한 집합을 할당해야 합니다. 역할/권한이 서로 다른 그룹을 여러 개 추가하려면 각 역할 또는 사용자 지정 권한 집합에 대해 다음 단계를 반복합니다.

3.  그룹 선택이 완료되면 **선택된 항목 추가**를 클릭합니다.
4.  **역할** 섹션에서 선택한 그룹(들)에 대한 [역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md)을 지정합니다. 개별 계정과 관련된 역할/권한에 관계 없이 그룹의 모든 구성원이 그룹에 적용되는 권한으로 개발자 센터 계정에 액세스할 수 있습니다.
5.  **저장**을 클릭합니다.


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-dev-center-account"></a>조직 디렉터리에 새로운 그룹 계정을 만들고 이를 개발자 센터 계정에 추가

새 그룹에 개발자 센터 액세스 권한을 부여하려는 경우 **사용자 관리** 섹션에서 새 그룹을 만들 수 있습니다. 새 그룹이 개발자 센터 계정뿐만 아니라 조직의 디렉터리에도 만들어집니다.

1.  **사용자 관리** 페이지에서 **그룹 추가**를 클릭합니다.
2.  다음 페이지에서 **새 그룹**을 클릭합니다.
3.  새 그룹에 대한 표시 이름을 입력합니다.
4.  그룹에[역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md)을 지정합니다. 개별 계정과 관련된 역할/권한에 관계 없이 그룹의 모든 구성원이 그룹에 적용되는 권한으로 개발자 센터 계정에 액세스할 수 있습니다.
5.  목록이 나타나면 새 그룹에 할당할 사용자를 선택합니다. 검색 상자를 사용하여 특정 사용자를 검색할 수 있습니다.
6.  사용자 선택이 완료되면 **선택된 항목 추가**를 클릭하여 새 그룹에 사용자를 추가합니다.
7.  **저장**을 클릭합니다.


<span id="azure-ad-applications" />
## <a name="add-azure-ad-applications-to-your-dev-center-account"></a>개발자 센터 계정에 Azure AD 응용 프로그램을 추가

조직의 Azure AD에 포함된 응용 프로그램이나 서비스에서 개발자 센터 계정에 액세스하도록 허용할 수 있습니다.

### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>조직 디렉터리의 Azure AD 응용 프로그램 추가

1.  **사용자 관리** 페이지에서 **Azure AD 응용 프로그램 추가**를 클릭합니다.
2.  표시되는 목록에서 하나 이상의 Azure AD 응용 프로그램을 선택합니다. 검색 상자를 사용하여 특정 Azure AD 응용 프로그램을 검색할 수 있습니다.
    > [!TIP]
    > 개발자 센터 계정에 추가할 Azure AD 응용 프로그램을 하나 이상 선택하는 경우에는 동일한 역할 또는 사용자 지정 권한 집합을 할당해야 합니다. 역할/권한이 서로 다른 Azure AD 응용 프로그램을 여러 개 추가하려면 각 역할 또는 사용자 지정 권한 집합에 대해 다음 단계를 반복합니다.

3.  Azure AD 응용 프로그램 선택이 완료되면 **선택된 항목 추가**를 클릭합니다.
4.  **역할** 섹션에서 선택한 Azure AD 응용 프로그램에 대한 [역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md)을 지정합니다.
5.  **저장**을 클릭합니다.


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-dev-center-account"></a>조직 디렉터리에 새로운 Azure AD 응용 프로그램 계정을 만들고 이를 개발자 센터 계정에 추가

새 Azure AD 응용 프로그램 계정에 개발자 센터 액세스 권한을 부여하려는 경우 **사용자 관리** 섹션에서 새 사용자 계정을 만들 수 있습니다. 개발자 센터 계정뿐만 아니라 조직의 디렉터리에도 새 계정이 만들어집니다.

> [!TIP]
> 개발자 센터 인증에 이 Azure AD 응용 프로그램을 주로 사용하고 사용자가 직접 액세스할 필요가 없는 경우, 해당 값을 디렉터리의 다른 Azure AD 응용 프로그램에서 사용하지 않으면 **회신 URL** 및 **앱 ID URI**에 유효한 주소를 입력할 수 있습니다.

1.  **사용자 관리** 페이지에서 **Azure AD 응용 프로그램 추가**를 클릭합니다.
2.  다음 페이지에서 **새 Azure AD 응용 프로그램**을 클릭합니다.
3.  새 Azure AD 응용 프로그램에 대한 **회신 URL**을 입력합니다. 이 URL를 통해 사용자가 Azure AD 응용 프로그램에 로그인하고 이를 사용할 수 있습니다(앱 URL 또는 로그온 URL이라고도 함). **회신 URL**은 256자 이내여야 합니다.
4.  새 Azure AD 응용 프로그램에 대한 **앱 ID URI**를 입력합니다. 이 URI는 Azure AD에 Single Sign-On 요청을 보낼 때 제공되는 Azure AD 응용 프로그램의 논리적 식별자입니다. 디렉터리에서 각 Azure AD 응용 프로그램에 대한 **앱 ID URI**는 고유해야 하고 256자 이내여야 합니다.
5.  **역할** 섹션에서 Azure AD 응용 프로그램에 대한 [역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md)을 지정합니다.
6.  **저장**을 클릭합니다.

Azure AD 응용 프로그램을 추가하거나 만든 후 **사용자 관리** 섹션으로 돌아가 응용 프로그램 이름을 클릭하면 테넌트 ID, 클라이언트 ID, 회신 URL 및 앱 ID URI를 비롯한 응용 프로그램의 설정을 검토할 수 있습니다.

> [!NOTE]
> [Windows 스토어 서비스](../monetize/using-windows-store-services.md)에서 제공하는 REST API를 사용하려는 경우, 서비스 호출을 인증하는 데 사용할 수 있는 Azure AD 액세스 토큰을 얻으려면 이 페이지에 표시된 테넌트 ID 및 클라이언트 ID 값이 필요합니다.   

<span id="manage-keys" />
### <a name="manage-keys-for-an-azure-ad-application"></a>Azure AD 응용 프로그램에 대한 키 관리

Azure AD 응용 프로그램이 Microsoft Azure AD에서 데이터를 읽고 쓸 경우 키가 필요합니다. 개발자 센터에서 정보를 편집하여 Azure AD 응용 프로그램에 대한 키를 만들 수 있습니다. 더 이상 필요하지 않은 키를 제거할 수도 있습니다.

1.  **사용자 관리** 페이지에서 Azure AD 응용 프로그램의 이름을 클릭합니다.
    > [!TIP]
    > Azure AD 응용 프로그램의 이름을 클릭하면 Azure AD 응용 프로그램에 대한 모든 활성 키가 키를 만든 날짜 및 키 만료 시기와 함께 표시됩니다. 더 이상 필요하지 않은 키를 제거하려면 **제거**를 클릭합니다.

2.  새 키를 추가하려면 **새 키 추가**를 클릭합니다.
3.  **클라이언트 ID** 및 **키** 값을 보여 주는 화면이 표시됩니다.
    > [!IMPORTANT]
    > 이 페이지를 나간 후에는 정보를 액세스할 수 없으므로 이 정보를 인쇄하거나 복사해야 합니다.

4.  키를 추가로 만들려면 **다른 키 추가**를 클릭합니다.

<span id="edit" />
## <a name="edit-a-user-group-or-azure-ad-application"></a>사용자, 그룹 또는 Azure AD 응용 프로그램 편집

개발자 센터 계정에 사용자 그룹 및/또는 Azure AD 응용 프로그램을 추가하고 난 후 계정 정보를 변경할 수 있습니다. 

> [!IMPORTANT]
> 역할 또는 권한에 대한 변경 사항은 개발자 센터 액세스에만 영향을 줍니다. 기타 모든 변경 내용(사용자의 이름이나 그룹 구성원, Azure AD 응용 프로그램에 대한 회신 URL 및 앱 ID URI 등)은 조직의 Azure AD 테넌트를 비롯하여 개발자 센터 계정에 반영됩니다. 

1.  **사용자 관리** 페이지에서 편집하려는 사용자, 그룹 또는 Azure AD 응용 프로그램 계정의 이름을 클릭합니다.
2.  원하는 대로 변경 하면 됩니다. 편집할 수 있는 항목은 다음과 같습니다.
    -   **사용자**에서 사용자의 이름, 성 또는 사용자 이름을 편집할 수 있습니다. **그룹 구성원** 섹션에서 그룹을 선택 또는 선택 취소하여 그룹 구성원을 업데이트할 수도 있습니다.
    -   **그룹**에서 그룹의 이름을 편집할 수 있습니다. (그룹 구성원을 업데이트 하려면 그룹에서 추가 또는 제거할 사용자를 편집하고 **그룹 구성원** 섹션을 변경합니다.)
    -   **Azure AD 응용 프로그램**에서 **회신 URL** 또는 **앱 ID URI**에 대한 새로운 값을 입력할 수 있습니다.
    이러한 변경 내용은 개발자 센터 계정뿐만 아니라 조직의 디렉터리에도 적용됩니다.
3.  개발자 센터 액세스와 관련된 변경의 경우, 적용하려는 역할(들)을 선택 또는 선택 취소하거나 **사용 권한 사용자 지정**을 선택하여 원하는 대로 변경합니다. 이러한 변경 내용은 개발자 센터 액세스에만 영향을 미치고, 조직의 Azure AD 테넌트 내에서는 어떤 권한도 변경되지 않습니다.
3.  **저장**을 클릭합니다.


## <a name="view-history-for-account-users"></a>계정 사용자의 기록 보기

계정 소유자는 계정에 추가한 추가 사용자의 자세한 검색 기록을 확인할 수 있습니다.

**사용자 관리** 페이지에서 검색 기록을 검토할 사용자의 **마지막 작업** 아래에 표시된 링크를 클릭합니다. 지난 30일 동안 사용자가 방문한 모든 페이지의 URL을 볼 수 있습니다.

<span id="remove" />
## <a name="remove-users-groups-and-azure-ad-applications"></a>사용자, 그룹 및 Azure AD 응용 프로그램 제거

개발자 센터 계정에서 사용자, 그룹 또는 Azure AD 응용 프로그램을 제거하려면 **사용자 관리** 페이지에 해당 이름으로 표시되는 **제거** 링크를 클릭합니다. 제거하기를 원한다고 확인한 후에는 나중에 다시 추가하지 않는 한 그 사용자, 그룹 또는 Azure AD 응용 프로그램이 더 이상 개발자 센터 계정에 액세스할 수 없게 됩니다.

> [!IMPORTANT] 
> 사용자, 그룹 또는 Azure AD 응용 프로그램 제거는 개발자 센터 계정에 더 이상 액세스 권한이 없음을 의미합니다. 사용자, 그룹 또는 Azure AD 응용 프로그램을 조직의 디렉터리에서 삭제하지 **않습니다**.

 
