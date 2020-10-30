---
description: 계정 사용자를 추가 하 고 관리 하려면 먼저 파트너 센터 계정을 조직의 Azure Active Directory 연결 해야 합니다.
title: 파트너 센터 계정에 Azure Active Directory 연결
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, azure ad, azure 테 넌 트, aad 테 넌 트, azure ad 테 넌 트, 테 넌 트 관리, 테 넌 트
ms.localizationpriority: medium
ms.openlocfilehash: ccb8a4fde9f1dc54d1de085dbcf0929bc6c762ea
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031286"
---
# <a name="associate-azure-active-directory-with-your-partner-center-account"></a>파트너 센터 계정에 Azure Active Directory 연결

[계정 사용자를 추가 하 고 관리](add-users-groups-and-azure-ad-applications.md)하려면 먼저 파트너 센터 계정을 조직의 Azure Active Directory 연결 해야 합니다. 

[파트너 센터](https://partner.microsoft.com/dashboard) 는 다중 사용자 계정 액세스 및 관리를 위해 Azure AD를 활용 합니다. 조직에서 이미 Microsoft 365 또는 Microsoft의 다른 비즈니스 서비스를 사용 하는 경우 Azure AD가 이미 있는 것입니다. 그렇지 않으면 추가 비용 없이 파트너 센터 내에서 새 Azure AD 테 넌 트를 만들 수 있습니다.

> [!TIP]
> 이 항목은 [파트너 센터](https://partner.microsoft.com/dashboard)의 windows apps 개발자 프로그램에만 적용 되지만 테 넌 트를 연결 하 고 사용자를 관리 하는 것은 Windows 데스크톱 응용 프로그램 (자세한 내용은 [windows 데스크톱 응용 프로그램 프로그램](/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users) 참조) 및 windows 하드웨어 개발자 프로그램에서 유사 하 게 작동 합니다 **. 관리자** 역할에 대 한 참조는 **관리자** 역할이 있는 하드웨어 계정에도 적용 됩니다. 자세한 내용은 [대시보드 관리](/windows-hardware/drivers/dashboard/dashboard-administration) 를 참조 하세요.

단일 Azure AD 테 넌 트를 여러 파트너 센터 계정에 연결할 수 있습니다. 여러 계정 사용자를 추가 하려면 파트너 센터 계정과 연결 된 하나의 Azure AD 테 넌 트가 있어야 하지만 단일 파트너 센터 계정에 여러 Azure AD 테 넌 트를 추가 하는 옵션도 있습니다. 파트너 센터 계정의 **관리자** 역할을 보유한 사용자에게는 계정에서 Azure AD 테넌트를 추가 및 제거할 수 있는 옵션이 주어집니다.

> [!IMPORTANT]
> 파트너 센터 계정을 Azure AD 테 넌 트에 연결한 후에는 해당 테 넌 트의 계정 사용자를 추가 하 고 관리 하기 위해 **관리자** 역할을 하는 동일한 테 넌 트의 사용자로 파트너 센터에 로그인 해야 합니다.


## <a name="associate-your-partner-center-account-with-your-organizations-existing-azure-ad-tenant"></a>파트너 센터 계정을 조직의 기존 Azure AD 테 넌 트와 연결

조직에서 이미 Azure AD를 사용 하는 경우 다음 단계에 따라 파트너 센터 계정을 연결 합니다.

1.  [파트너 센터](https://partner.microsoft.com/dashboard)에서 기어 아이콘 (대시보드의 오른쪽 위 모퉁이 근처)을 선택한 다음 **개발자 설정** 을 선택 합니다. **설정** 메뉴에서 **테 넌 트** 를 선택 합니다.
2.  **파트너 센터 계정에 AZURE AD 연결을** 선택 합니다.
3.  연결하려는 테넌트에 대한 Azure AD 자격 증명을 입력합니다.
4.  Azure AD 테넌트에 대한 조직 및 도메인 이름을 검토합니다. 연결을 완료하려면 **확인** 을 선택합니다.
5.  연결에 성공하면 파트너 센터의 **사용자** 섹션에서 계정 사용자를 추가하고 관리할 준비가 된 것입니다.

> [!IMPORTANT]
> 새 사용자를 만들거나 Azure AD에 대 한 다른 변경 작업을 수행 하려면 해당 테 넌 트에 대 한 [전역 관리자 권한이](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) 있는 계정을 사용 하 여 해당 azure ad 테 넌 트에 로그인 해야 합니다. 그러나 테 넌 트를 연결 하거나 해당 테 넌 트에 이미 있는 사용자를 파트너 센터 계정에 추가 하기 위해 전역 관리자 권한이 필요 하지 않습니다.


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account"></a>파트너 센터 계정과 연결할 새 Azure AD를 만드세요.

파트너 센터 계정과 연결 하기 위해 새 Azure AD를 설정 해야 하는 경우 다음 단계를 수행 합니다.

1.  [파트너 센터](https://partner.microsoft.com/dashboard)에서 기어 아이콘 (대시보드의 오른쪽 위 모퉁이 근처)을 선택한 다음 **개발자 설정** 을 선택 합니다. **설정** 메뉴에서 **테 넌 트** 를 선택 합니다.
2.  **새 AZURE AD 만들기** 를 선택 합니다.
3.  새 Azure AD에 대한 디렉터리 정보를 입력합니다.
    - **도메인 이름** : Azure AD 도메인에 사용할 고유 이름으로, "onmicrosoft.com"와 함께 사용 합니다. 예를 들어 "example"을 입력 한 경우 Azure AD 도메인은 "example.onmicrosoft.com"가 됩니다.
    - **연락처 전자 메일** : 필요한 경우 계정에 대해 문의할 수 있는 전자 메일 주소입니다.
    - **전역 관리자 사용자 계정 정보** : 새 전역 관리자 계정에 사용할 이름, 성, 사용자 이름 및 암호입니다.
4.  **만들기** 를 클릭 하 여 새 도메인 및 계정 정보를 확인 합니다.
5.  새 Azure AD 전역 관리자 사용자 이름 및 암호로 로그인 하 여 [추가 계정 사용자 추가 및 관리](add-users-groups-and-azure-ad-applications.md)를 시작 합니다.


## <a name="manage-azure-ad-tenant-associations"></a>Azure AD 테 넌 트 연결 관리

Azure AD 테 넌 트를 파트너 센터 계정에 연결한 후에는 **테** 넌 트 페이지에서 새 테 넌 트를 추가 하거나 기존 테 넌 트를 제거할 수 있습니다.


### <a name="add-multiple-azure-ad-tenants-to-your-partner-center-account"></a>파트너 센터 계정에 여러 Azure AD 테 넌 트 추가

파트너 센터 계정에 대 한 **관리자** 역할을 가진 사용자는 Azure AD 테 넌 트를 계정에 연결할 수 있습니다.

새 테 넌 트를 연결 하려면 **다른 AZURE AD 테 넌 트 연결** 을 선택한 후 위에 나와 있는 단계를 따릅니다. 연결 하려는 Azure AD 테 넌 트에서 자격 증명을 입력 하 라는 메시지가 표시 됩니다.


### <a name="remove-an-azure-ad-tenant-from-your-partner-center-account"></a>파트너 센터 계정에서 Azure AD 테 넌 트 제거

파트너 센터 계정에 대 한 **관리자** 역할이 있는 사용자는 계정에서 Azure AD 테 넌 트를 제거할 수 있습니다.

> [!IMPORTANT]
> 테넌트를 제거하는 경우 해당 테넌트에서 파트너 센터 계정에 추가된 모든 사용자가 더 이상 계정에 로그인할 수 없습니다. 

테 넌 트를 제거 **하려면 테 넌 트 페이지 (** **계정 설정** )에서 해당 이름을 찾은 다음 **제거** 를 선택 합니다. 테 넌 트를 제거할지를 확인 하는 메시지가 표시 됩니다. 테넌트를 제거하면 해당 테넌트의 사용자가 파트너 센터 계정에 로그인할 수 없으며, 해당 사용자에 대해 구성한 모든 권한이 제거됩니다.

> [!TIP]
> 현재 동일한 테 넌 트의 계정을 사용 하 여 파트너 센터에 로그인 한 경우에는 테 넌 트를 제거할 수 없습니다. 테넌트를 제거하려면 계정과 연결된 다른 테넌트에 대한 **관리자** 로서 파트너 센터에 로그인해야 합니다. 계정과 연결된 테넌트가 하나뿐인 경우 해당 테넌트는 계정을 연 Microsoft 계정으로 로그인해야만 제거할 수 있습니다.
