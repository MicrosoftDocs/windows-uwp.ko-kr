---
Description: You can add users, groups, and Azure AD applications to your Partner Center account.
title: Add users, groups, and Azure AD applications to your Partner Center account
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, azure ad application, aad, user, group, multiple users, multi-user
ms.localizationpriority: medium
ms.openlocfilehash: 41467f51e02f3cc700e3759f33d6fd6eea3ac7a6
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260075"
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-partner-center-account"></a>Add users, groups, and Azure AD applications to your Partner Center account

The **Users** section of [Partner Center](https://partner.microsoft.com/dashboard) (under **Account settings**) lets you use Azure Active Directory to add users to your Partner Center account. 각 사용자에게 계정에 대한 사용 권한을 정의하는 역할(또는 사용자 지정 권한 집합)이 할당됩니다. You can also add [groups of users](#groups) and [Azure AD applications](#azure-ad-applications) to grant them access to your Partner Center account.

사용자가 계정에 추가되고 나면 [계정 세부 정보 편집](#edit), [역할 및 사용 권한](set-custom-permissions-for-account-users.md), 변경 또는 [사용자 제거](#remove)가 가능합니다.

> [!IMPORTANT]
> In order to add users to your account, you must first [associate your Partner Center account with your organization's Azure Active Directory tenant](associate-azure-ad-with-partner-center.md). 

When adding users, you will need to specify their access to your Partner Center account by assigning them a [role or set of custom permissions](set-custom-permissions-for-account-users.md). 

Keep in mind that all Partner Center users (including groups and Azure AD applications) must have an active account in [an Azure AD tenant that is associated with your Partner Center account](associate-azure-ad-with-partner-center.md). 사용자 관리는 한 번에 한 테넌트에서 완료되며 사용자를 추가 또는 편집하려는 테넌트에 대한 관리자 계정으로 로그인해야 합니다. Creating a new user in Partner Center will also create an account for that user in the Azure AD tenant to which you are signed in, and making changes to a user's name in Partner Center will make the same changes in your organization's Azure AD tenant.

> [!NOTE]
> If your organization uses [directory integration](https://docs.microsoft.com/previous-versions/azure/azure-services/jj573653(v=azure.100)?redirectedfrom=MSDN) to sync the on-premises directory service with your Azure AD, you won't be able to create new users, groups, or Azure AD applications in Partner Center. You (or another admin in your on-premises directory) will need to create them directly in the on-premises directory before you'll be able to see and add them in Partner Center.


<span id="users" />

## <a name="add-users-to-your-partner-center-account"></a>Add users to your Partner Center account

To add users to your Partner Center account, go to the **Users** page in **Account settings** and select **Add users.** 작업하려는 Azure AD 테넌트에 대한 관리자 계정을 사용하여 로그인해야 합니다. 

### <a name="add-existing-users"></a>기존 사용자 추가 

You can select users who already exist in your organization's tenant and give them access to your Partner Center account. 

<span id="from-directory" />

1.  Select the gear icon (near the upper right corner of Partner Center) and then select **Developer settings**. In the **Settings** menu, select **Users**.
2.  **사용자** 페이지에서 **사용자 추가**를 선택합니다. 
3.  표시되는 목록에서 하나 이상의 사용자를 선택합니다. 검색 상자를 사용하여 특정 사용자를 검색할 수 있습니다.
    > [!TIP]
    > If you select more than one user to add to your Partner Center account, you must assign them the same role or set of custom permissions. 역할/권한이 서로 다른 사용자를 여러 명 추가하려면 각 역할 또는 사용자 지정 권한 집합에 대해 다음 단계를 반복합니다.
4.  사용자 선택이 완료되면 **선택된 항목 추가**를 클릭합니다.
5.  **역할** 섹션에서 선택한 사용자에 대한 [역할 또는 사용자 지정 권한 집합](set-custom-permissions-for-account-users.md) 을 지정합니다.
6.  **Save**을 클릭합니다.

### <a name="additional-methods-for-adding-users"></a>사용자를 추가하는 추가 방법

If you are signed in with a Manager account which also has [global administrator](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) permissions for the Azure AD tenant you're working in, you will have additional options to add users to your Partner Center account. 다음 중 하나를 선택해야 합니다.

-   **Add existing users**: Choose users who already exist in your organization's directory and give them access to your Partner Center account, using the method described above.
-   **Create new users**: Create brand new user accounts to add to both your organization's directory and your Partner Center account
-   **외부 사용자 초대**: 현재 조직의 디렉터리에 있지 않은 사용자에게 메일 초대장을 전송합니다. They will be invited to access your Partner Center account, and a new [guest user](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) account will be created for them in your Azure AD tenant.

<span id="new-user" />

### <a name="create-new-users"></a>새 사용자 만들기

> [!IMPORTANT]
> 새로운 사용자를 만들려면 반드시 Azure AD 테넌트에서 전역 관리자 계정으로 로그인을 해야 합니다.

1.  From the **Users** page (under **Account settings**), select **Add users**, then choose **Create new users**.
2.  새 사용자의 이름, 성 및 사용자 이름을 입력합니다.
3.  조직 디렉터리에서 새 사용자에게 [전역 관리자 계정](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)을 부여하려면 **Azure AD에서 이 사용자를 전역 관리자로 지정하여 모든 디렉터리 리소스에 대한 모든 권한을 부여** 상자를 선택합니다. 이렇게 하면 해당 사용자에게 회사 Azure AD의 모든 관리 기능에 대한 모든 권한이 부여됩니다. They'll be able to add and manage users in your organization's directory (though not in Partner Center, unless you grant the account the appropriate [role/permissions](set-custom-permissions-for-account-users.md)). 이 상자를 선택하면 사용자에게 **암호 복구 메일**을 보내야 합니다.
4.  **Azure AD에서 이 사용자를 전역 관리자로 지정** 확인란을 선택한 경우 사용자가 암호를 복구해야 하는 경우 사용할 수 있는 메일 주소를 입력합니다.
5.  **그룹 구성원** 섹션에서 새 사용자가 속하게 될 그룹을 선택합니다.
6.  **역할** 섹션에서 해당 사용자에 대한 [역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md) 을 지정합니다.
7.  **Save**을 클릭합니다.
8.  확인 페이지에 임시 암호를 비롯하여 새 사용자의 로그인 정보가 표시됩니다. 이 페이지를 나간 후에는 임시 암호에 액세스할 수 없으므로 이 정보를 메모하고 새 사용자에게 제공해야 합니다.


<span id="email" />

### <a name="invite-outside-users"></a>외부 사용자 초대

> [!IMPORTANT]
> 외부 사용자를 초대하려면 반드시 Azure AD 테넌트에서 전역 관리자 계정으로 로그인을 해야 합니다.

1.  From the **Users** page (under **Account settings**), select **Add users**, then choose **Invite users by email**.
1.  하나 이상의 메일 주소(최대 10개)를 쉼표나 세미콜론으로 구분하여 입력합니다.
2.  **역할** 섹션에서 해당 사용자에 대한 [역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md) 을 지정합니다.
3.  **Save**을 클릭합니다.

초대한 사용자가 계정에 가입하라는 메일 초대장을 받으면 Azure AD 테넌트에서 새로운 [게스트 사용자](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) 계정이 생성됩니다. 각 사용자는 계정에 액세스하려면 먼저 초대를 수락해야 합니다.

초대를 다시 보내려면 **사용자** 페이지에서 사용자를 찾아 메일 주소(또는 **보류 중인 초대**라고 표시되는 텍스트)를 선택합니다. 그런 다음 페이지의 맨 아래쪽에서 **초대 다시 보내기**를 클릭합니다.

> [!IMPORTANT]
> Outside users that you invite to join your Partner Center account can be assigned the same roles and permissions as other users. 그러나 외부 사용자는 Microsoft Store에 앱을 연결한다든지 Microsoft Store에 업로드할 패키지를 생성하는 등의 특정 작업을 Visual Studio에서 수행할 수 없습니다. 이러한 작업을 수행해야 하는 경우에는 **외부 사용자 초대** 대신 **새 사용자 만들기**를 선택합니다 (기존 Azure AD 테넌트에 이들 사용자를 추가하고 싶지 않은 경우에는 [새 테넌트 만들기](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account)를 선택하고 해당 테넌트에서 이들 사용자를 위한 새로운 계정을 만들 수 있음). 


### <a name="changing-a-users-directory-password"></a>사용자의 디렉터리 암호 변경

사용자 계정을 만들 때 귀하가 **암호 복구 메일**을 제공한 경우 사용자가 암호를 변경해야 하는 경우 직접 변경할 수 있습니다. 또한 사용자의 암호를 변경하기 위해 Azure AD 테넌트에서 전역 관리자 계정을 사용하여 로그인한 경우 아래 단계에 따라 사용자의 암호를 업데이트할 수 있습니다. Note that this will change the user's password in your Azure AD tenant, along with the password they use to access Partner Center. 

1.  From the **Users** page (under **Account settings**), select the name of the user account that you want to edit.
2.  Select the **Reset password** button at the bottom of the page.
3.  임시 암호를 비롯하여 사용자에 대한 로그인 정보를 표시하는 확인 페이지가 나타납니다.

    > [!IMPORTANT]
    >  Be sure to print or copy this info and provide it to the user, as you won't be able to access the temporary password after you leave this page.

<span id="groups" />

## <a name="add-groups-to-your-partner-center-account"></a>Add groups to your Partner Center account

You can add a group from your organization's directory to your Partner Center account. 이렇게 하면 그룹 구성원인 모든 사용자가 그룹에 할당된 역할과 관련해 권한을 가지고 액세스를 할 수 있게 됩니다.

### <a name="add-groups-from-your-organizations-directory"></a>조직 디렉터리의 그룹 추가

1.  Select the gear icon (near the upper right corner of Partner Center) and then select **Developer settings**. In the **Settings** menu, select **Users**.
2. From the **Users** page, select **Add groups**.
2.  표시되는 목록에서 하나 이상의 그룹을 선택합니다. 검색 상자를 사용하여 특정 그룹을 검색할 수 있습니다.
    > [!TIP]
    > If you select more than one group to add to your Partner Center account, you must assign them the same role or set of custom permissions. 역할/권한이 서로 다른 그룹을 여러 개 추가하려면 각 역할 또는 사용자 지정 권한 집합에 대해 다음 단계를 반복합니다.

3.  그룹 선택이 완료되면 **선택된 항목 추가**를 클릭합니다.
4.  **역할** 섹션에서 선택한 그룹(들)에 대한 [역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md)을 지정합니다. All members of the group will be able to access your Partner Center account with the permissions you apply to the group, regardless of the roles/permissions associated with their individual account.
5.  **Save**을 클릭합니다.


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Create a new group account in your organization's directory and add it to your Partner Center account

If you want to grant Partner Center access to a brand new group, you can create a new group in the **Users** section. Note that the new group will be created in your organization's directory, not just in your Partner Center account.

1.  From the **Users** page (under **Developer settings**), click **Add groups**.
2.  On the next page, select **New group**.
3.  새 그룹에 대한 표시 이름을 입력합니다.
4.  그룹에[역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md)을 지정합니다. All members of the group will be able to access your Partner Center account with the permissions you apply to the group, regardless of the roles/permissions associated with their individual account.
5.  목록이 나타나면 새 그룹에 할당할 사용자를 선택합니다. 검색 상자를 사용하여 특정 사용자를 검색할 수 있습니다.
6.  사용자 선택이 완료되면 **선택된 항목 추가**를 클릭하여 새 그룹에 사용자를 추가합니다.
7.  **Save**을 클릭합니다.


<span id="azure-ad-applications" />

## <a name="add-azure-ad-applications-to-your-partner-center-account"></a>Add Azure AD applications to your Partner Center account

You can allow applications or services that are part of your organization's Azure AD to access your Partner Center account. 이러한 Azure AD 응용 프로그램 사용자 계정은 [Microsoft Store 서비스](../monetize/using-windows-store-services.md)에서 제공되는 REST API를 호출하는 데 사용할 수 있습니다.


### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>조직 디렉터리의 Azure AD 응용 프로그램 추가

1.  1.  Select the gear icon (near the upper right corner of Partner Center) and then select **Developer settings**. In the **Settings** menu, select **Users**.
2. **사용자** 페이지에서 **Azure AD 응용 프로그램 추가**를 선택합니다.
3.  표시되는 목록에서 하나 이상의 Azure AD 응용 프로그램을 선택합니다. 검색 상자를 사용하여 특정 Azure AD 응용 프로그램을 검색할 수 있습니다.
    > [!TIP]
    > If you select more than one Azure AD application to add to your Partner Center account, you must assign them the same role or set of custom permissions. 역할/권한이 서로 다른 Azure AD 응용 프로그램을 여러 개 추가하려면 각 역할 또는 사용자 지정 권한 집합에 대해 다음 단계를 반복합니다.

4.  Azure AD 응용 프로그램 선택이 완료되면 **선택된 항목 추가**를 클릭합니다.
5.  **역할** 섹션에서 선택한 Azure AD 응용 프로그램에 대한 [역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md)을 지정합니다.
6.  **Save**을 클릭합니다.


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Create a new Azure AD application account in your organization's directory and add it to your Partner Center account

If you want to grant Partner Center access to a brand new Azure AD application account, you can create one in the **Users** section. Note that this will create a new account in your organization's directory, not just in your Partner Center account.

> [!TIP]
> If you are primarily using this Azure AD application for Partner Center authentication, and don't need users to access it directly, you can enter any valid address for the **Reply URL** and **App ID URI**, as long as those values are not used by any other Azure AD application in your directory.

1.  From the **Users** page (under **Account settings**), select **Add Azure AD applications**.
2.  On the next page, select **New Azure AD application**.
3.  새 Azure AD 응용 프로그램에 대한 **회신 URL**을 입력합니다. 이 URL를 통해 사용자가 Azure AD 응용 프로그램에 로그인하고 이를 사용할 수 있습니다(앱 URL 또는 로그온 URL이라고도 함). **회신 URL**은 256자 이내여야 하고 디렉터리 내에서 고유해야 합니다.
4.  새 Azure AD 응용 프로그램에 대한 **앱 ID URI**를 입력합니다. 이 URI는 Azure AD에 Single Sign-On 요청을 보낼 때 제공되는 Azure AD 응용 프로그램의 논리적 식별자입니다. 디렉터리에서 각 Azure AD 응용 프로그램에 대한 **앱 ID URI**는 고유해야 하고 256자 이내여야 합니다. **앱 ID URI**에 대한 자세한 내용은 [응용 프로그램과 Azure Active Directory 통합](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#changing-the-application-registration-to-support-multi-tenant)을 참조하세요.
5.  **역할** 섹션에서 Azure AD 응용 프로그램에 대한 [역할 또는 사용자 지정 권한](set-custom-permissions-for-account-users.md)을 지정합니다.
6.  **Save**을 클릭합니다.

Azure AD 응용 프로그램을 추가하거나 만든 후 **사용자** 섹션으로 돌아가 응용 프로그램 이름을 선택하면 테넌트 ID, 클라이언트 ID, 회신 URL 및 앱 ID URI를 비롯한 응용 프로그램의 설정을 검토할 수 있습니다.

> [!NOTE]
> [Microsoft Store 서비스](../monetize/using-windows-store-services.md)에서 제공하는 REST API를 사용하려는 경우, 서비스 호출을 인증하는 데 사용할 수 있는 Azure AD 액세스 토큰을 얻으려면 이 페이지에 표시된 테넌트 ID 및 클라이언트 ID 값이 필요합니다.   

<span id="manage-keys" />

### <a name="manage-keys-for-an-azure-ad-application"></a>Azure AD 응용 프로그램에 대한 키 관리

Azure AD 응용 프로그램이 Microsoft Azure AD에서 데이터를 읽고 쓸 경우 키가 필요합니다. You can create keys for an Azure AD application by editing its info in Partner Center. 더 이상 필요하지 않은 키를 제거할 수도 있습니다.

1.  From the **Users** page (under **Account settings**), select the name of the Azure AD application.
    > [!TIP]
    > When you click the name of the Azure AD application, you'll see all of the active keys for the Azure AD application, including the date on which the key was created and when it will expire. 더 이상 필요하지 않은 키를 제거하려면 **제거**를 클릭합니다.

2.  To add a new key, select **Add new key**.
3.  **클라이언트 ID** 및 **키** 값을 보여 주는 화면이 표시됩니다.
    > [!IMPORTANT]
    > Be sure to print or copy this info, as you won't be able to access it again after you leave this page.

4.  If you want to create more keys, select **Add another key**.

<span id="edit" />

## <a name="edit-a-user-group-or-azure-ad-application"></a>사용자, 그룹 또는 Azure AD 응용 프로그램 편집

After you've added users, groups, and/or Azure AD applications to your Partner Center account, you can make changes to their account info. 

> [!IMPORTANT]
> Changes made to [roles or permissions](set-custom-permissions-for-account-users.md) will only affect Partner Center access. All other changes (such as changing a user's name or group membership, or the Reply URL and App ID URI for an Azure AD application) will be reflected in your organization's Azure AD tenant as well as in your Partner Center account. 

1.  From the **Users** page (under **Account settings**), select the name of the user, group, or Azure AD application account that you want to edit.
2.  원하는 대로 변경 하면 됩니다. 편집할 수 있는 항목은 다음과 같습니다.
    -   **사용자**에서 사용자의 이름, 성 또는 사용자 이름을 편집할 수 있습니다. **그룹 구성원** 섹션에서 그룹을 선택 또는 선택 취소하여 그룹 구성원을 업데이트할 수도 있습니다.
    -   **그룹**에서 그룹의 이름을 편집할 수 있습니다. (그룹 구성원을 업데이트 하려면 그룹에서 추가 또는 제거할 사용자를 편집하고 **그룹 구성원** 섹션을 변경합니다.)
    -   **Azure AD 응용 프로그램**에서 **회신 URL** 또는 **앱 ID URI**에 대한 새로운 값을 입력할 수 있습니다.
    Remember that these changes will be made in your organization's directory as well as in your Partner Center account.
3.  For changes related to Partner Center access, select or deselect the role(s) that you want to apply, or select **Customize permissions** and make the desired changes. These changes only impact Partner Center access and will not change any permissions within your organization's Azure AD tenant.
3.  **Save**을 클릭합니다.


## <a name="view-history-for-account-users"></a>계정 사용자의 기록 보기

계정 소유자는 계정에 추가한 추가 사용자의 자세한 검색 기록을 확인할 수 있습니다.

On the **Users** page (under **Account settings**), select the link shown under **Last activity** for the user whose browsing history you’d like to review. 지난 30일 동안 사용자가 방문한 모든 페이지의 URL을 볼 수 있습니다.

<span id="remove" />

## <a name="remove-users-groups-and-azure-ad-applications"></a>사용자, 그룹 및 Azure AD 응용 프로그램 제거

To remove a user, group, or Azure AD application from your Partner Center account, select the **Remove** link that appears by their name on the **Users** page. After confirming that you want to remove it, that user, group, or Azure AD application will no longer be able to access to your Partner Center account (unless you add it again later).

> [!IMPORTANT]
> Removing a user, group, or Azure AD application means that it will no longer have access to your Partner Center account. 사용자, 그룹 또는 Azure AD 응용 프로그램을 조직의 디렉터리에서 삭제하지 **않습니다**.

 

