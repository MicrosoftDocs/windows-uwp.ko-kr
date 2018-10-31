---
author: jnHs
Description: In order to add and manage account users, you must first associate your Partner Center account with your organization's Azure Active Directory.
title: 파트너 센터 계정에 Azure Active Directory 연결
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, azure ad, azure 테넌트, aad 테넌트, azure ad 테넌트, 테넌트 관리, 테넌트
ms.localizationpriority: medium
ms.openlocfilehash: a76021f53417d30b91db282a194f6dc6ca268c1f
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5835479"
---
# <a name="associate-azure-active-directory-with-your-partner-center-account"></a>파트너 센터 계정에 Azure Active Directory 연결

[계정 사용자 추가 및 관리](add-users-groups-and-azure-ad-applications.md)하려면 먼저 조직의 Azure Active Directory를 파트너 센터 계정에를 연결 해야 합니다. 

[파트너 센터](https://partner.microsoft.com/dashboard) 는 다중 사용자 계정 액세스 및 관리를 위한 Azure AD를 활용합니다. 조직에서 이미 Office 365 또는 Microsoft의 다른 비즈니스 서비스를 사용하는 경우 이미 Azure AD가 있습니다. 그렇지 않으면 만들면 새 Azure AD 테 넌 트를 파트너 센터 내에서 추가 요금 없이 합니다.

> [!TIP]
> 테 넌 트 연결 및 사용자 관리는 Windows 데스크톱 응용 프로그램의 계정과 유사 하 게 작동 하지만이 항목은 [파트너 센터](https://partner.microsoft.com/dashboard)Windows 앱 개발자 프로그램 ( [Windows 데스크톱 응용 프로그램](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users) 에 대 한 참조 자세한 내용은) 및 Windows 하드웨어 개발자 프로그램 (여기서 **관리자** 역할에 대 한 참조 **관리자** 역할을 사용 하 여 하드웨어 계정에도 적용 됩니다; [대시보드 관리](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-administration) 에 대 한 자세한 내용은 참조).

단일 Azure AD 테 넌 트를 여러 파트너 센터 계정에 연결할 수 있습니다. 여러 계정 사용자를 추가 하려면 파트너 센터 계정과 연결 된 Azure AD 테 해야 하지만 단일 파트너 센터 계정에 여러 Azure AD 테 넌 트를 추가 하는 옵션을 수도 있습니다. 파트너 센터 계정에서 **관리자** 역할을 가진 모든 사용자를 추가 하 고 계정에서 Azure AD 테 넌 트를 제거 하는 옵션을 생깁니다.

> [!IMPORTANT]
> Azure AD 테 넌 트를 사용 하 여 파트너 센터 계정에 연결한 다음 추가 하 고 해당 테 넌 트의 계정 사용자를 관리 하려면 해야 **관리자** 역할이 있는 동일한 테 넌 트의 사용자로 파트너 센터에 로그인 합니다.


## <a name="associate-your-partner-center-account-with-your-organizations-existing-azure-ad-tenant"></a>파트너 센터 계정을 조직의 기존 Azure AD 테 넌 트와 연결

조직에 이미 Azure AD를 사용 하는 경우 파트너 센터 계정에 연결 하려면 다음이 단계를 따르세요.

1.  [파트너 센터](https://partner.microsoft.com/dashboard) 대시보드의 오른쪽 위 모서리) (근처 기어 아이콘을 선택 하 고 **개발자 설정**를 선택 합니다. **설정** 메뉴의 **테 넌 트**를 선택 합니다.
2.  **파트너 센터 계정과 Azure AD 연결**을 선택 합니다.
3.  연결하려는 테넌트에 대한 Azure AD 자격 증명을 입력합니다.
4.  Azure AD 테넌트에 대한 조직 및 도메인 이름을 검토합니다. 연결을 완료하려면 **확인**을 선택합니다.
5.  연결 되 면 추가 하 고 파트너 센터에서 **사용자가** 섹션에서 계정 사용자를 관리할 준비가 됩니다.

> [!IMPORTANT]
> 새 사용자를 만들거나 Azure AD에 기타 변경을 적용하려면 해당 테넌트에 대한 [전역 관리자 권한](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)이 있는 계정을 사용하여 해당 Azure AD 테넌트에 로그인해야 합니다. 그러나 파트너 센터 계정에 해당 테 넌 트에 이미 존재 하는 사용자를 추가 하거나 테 넌 트를 연결 하려면 전역 관리자 권한은 필요 하지 않습니다.


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account"></a>파트너 센터 계정과 연결할 새 Azure AD 만들기

파트너 센터 계정과 연결할 새 Azure AD를 설정 해야 할 경우 다음이 단계를 따르세요.

1.  [파트너 센터](https://partner.microsoft.com/dashboard)에서 대시보드의 오른쪽 위 모서리) (근처 기어 아이콘을 선택 하 고 **개발자 설정**를 선택 합니다. **설정** 메뉴의 **테 넌 트**를 선택 합니다.
2.  **새 Azure AD 만들기**를 선택합니다.
3.  새 Azure AD에 대한 디렉터리 정보를 입력합니다.
    - **도메인 이름**: Azure AD 도메인에 사용할 고유한 이름으로 ".onmicrosoft.com"과 함께 사용합니다. 예를 들어 "example"을 입력한 경우 Azure AD 도메인은 "example.onmicrosoft.com"이 됩니다.
    - **연락처 메일**: 필요한 경우 계정에 대해 문의할 수 있는 메일 주소입니다.
    - **전역 관리자 사용자 계정 정보**: 새 전역 관리자 계정에 사용할 성, 이름, 사용자 이름 및 암호입니다.
4.  **만들기**를 클릭하여 새 도메인 및 계정 정보를 확인합니다.
5.  새로운 Azure AD 전역 관리자 사용자 이름 및 암호로 로그인하여 [사용자 계정 추가 및 관리](add-users-groups-and-azure-ad-applications.md)를 시작합니다.


## <a name="manage-azure-ad-tenant-associations"></a>Azure AD 테넌트 연결 관리

파트너 센터 계정과 Azure AD 테 넌 트를 연결 하 고 나면 새 테 넌 트를 추가 하거나 **테 넌 트** 페이지에서 기존 테 넌 트를 제거할 수 있습니다.


### <a name="add-multiple-azure-ad-tenants-to-your-partner-center-account"></a>파트너 센터 계정에 여러 Azure AD 테 넌 트 추가

파트너 센터 계정에 대 한 **관리자** 역할을 가진 모든 사용자 계정을 사용 하 여 Azure AD 테 넌 트를 연결할 수 있습니다.

새 테넌트를 연결하려면 **다른 Azure AD 테넌트 연결**을 선택한 다음 위에 표시된 단계를 따릅니다. 연결하려는 Azure AD 테넌트에서 자격 증명에 대한 메시지가 표시됩니다.


### <a name="remove-an-azure-ad-tenant-from-your-partner-center-account"></a>파트너 센터 계정에서 Azure AD 테 넌 트를 제거

파트너 센터 계정에 대 한 **관리자** 역할을 가진 모든 사용자 계정에서 Azure AD 테 넌 트를 제거할 수 있습니다.

> [!IMPORTANT]
> 테 넌 트를 제거 하면 해당 테 넌 트를 파트너 센터 계정에 추가 된 모든 사용자가 더 이상 계정에 로그인 할 수 없게 됩니다. 

테 넌 트를 제거 하려면 ( **계정 설정**)에서 **테 넌 트** 페이지에서 이름을 찾아 다음 **제거**를 선택 합니다. 테넌트의 제거를 확인하라는 메시지가 표시됩니다. 이렇게 하면 해당 테 넌 트에 사용자가 파트너 센터 계정에 로그인 할 수 및 사용자에 대해 구성한 권한을 제거 됩니다.

> [!TIP]
> 로그인 한 경우 현재 파트너 센터에 동일한 테 넌 트의 계정을 사용 하 여 테 넌 트를 제거할 수 없습니다. 테 넌 트를 제거 하려면 로그인 해야 파트너 센터에는 **관리자** 계정과 사용 하 여 연결 된 다른 테 넌 트에 대 한 합니다. 계정에 연결된 테넌트가 하나뿐인 경우 해당 테넌트는 계정을 연 Microsoft 계정으로 로그인한 후 제거될 수 있습니다.


