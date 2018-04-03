---
author: jnHs
Description: In order to add and manage account users, you must first associate your Dev Center account with your organization's Azure Active Directory.
title: 개발자 센터 계정에 Azure Active Directory 연결
ms.author: wdg-dev-content
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, azure ad, azure 테넌트, aad 테넌트, azure ad 테넌트, 테넌트 관리, 테넌트
ms.localizationpriority: high
ms.openlocfilehash: c430bb279d0b9da6126212a8af7400df8cd1693e
ms.sourcegitcommit: ef5a1e1807313a2caa9c9b35ea20b129ff7155d0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2018
---
# <a name="associate-azure-active-directory-with-your-dev-center-account"></a>개발자 센터 계정에 Azure Active Directory 연결

[계정 사용자를 추가하고 관리하려면](add-users-groups-and-azure-ad-applications.md) 먼저 개발자 센터 계정을 조직의 Azure Active Directory와 연결해야 합니다. 

Windows 개발자 센터는 다중 사용자 계정 액세스 및 관리를 위해 Azure AD를 활용합니다. 조직에서 이미 Office 365 또는 Microsoft의 다른 비즈니스 서비스를 사용하는 경우 이미 Azure AD가 있습니다. 아니면 추가 요금 없이 개발자 센터 내에서 Azure AD 테넌트를 새로 만들 수 있습니다.

> [!TIP]
> 이 항목은 Windows 앱 개발자 프로그램에 한정되지만 테넌트 연결 및 사용자 관리는 Windows 데스크톱 응용 프로그램 프로그램(자세한 내용은 [Windows 데스크톱 응용 프로그램 프로그램](https://msdn.microsoft.com/library/windows/desktop/mt826504#users) 참조) 및 Windows 하드웨어 개발자 프로그램(여기에서 **관리자** 역할에 대한 참조는 **시스템 관리자** 역할의 하드웨어 계정에도 적용됨. 자세한 내용은 [대시보드 관리](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-administration) 참조)의 계정과 유사하게 작동합니다.

단일 Azure AD 테넌트는 여러 개발자 센터 계정과 연결할 수 있습니다. 여러 계정 사용자를 추가하려면 개발자 센터 계정과 연결된 단일 Azure AD 테넌트만 필요하지만 단일 개발자 센터 계정에 여러 Azure AD 테넌트를 추가할 수도 있습니다. 개발자 센터 계정에서 **관리자** 역할을 가진 모든 사용자는 Azure AD 테넌트를 추가하고 제거할 수 있습니다.

> [!IMPORTANT]
> 개발자 센터 계정을 Azure AD 테넌트에 연결하고 나면 해당 테넌트의 계정 사용자를 추가하고 관리하려면 **관리자** 역할이 있는 동일한 테넌트의 사용자로 개발자 센터에 로그인해야 합니다.


## <a name="associate-your-dev-center-account-with-your-organizations-existing-azure-ad-tenant"></a>개발자 센터 계정을 조직의 기존 Azure AD 테넌트와 연결

조직에서 이미 Azure AD를 사용하는 경우 개발자 센터 계정에 연결하려면 다음 단계를 따릅니다.

1.  **계정 설정**으로 이동하여 **테넌트**를 클릭합니다.
2.  **Azure AD와 개발자 센터 계정 연결**을 선택합니다.
3.  연결하려는 테넌트에 대한 Azure AD 자격 증명을 입력합니다.
4.  Azure AD 테넌트에 대한 조직 및 도메인 이름을 검토합니다. 연결을 완료하려면 **확인**을 선택합니다.
5.  연결이 되면 개발자 센터의 **사용자** 섹션에서 사용자를 추가하고 관리할 준비가 됩니다.

> [!IMPORTANT]
> 새 사용자를 만들거나 Azure AD에 기타 변경을 적용하려면 해당 테넌트에 대한 [전역 관리자 권한](http://go.microsoft.com/fwlink/?LinkId=746654)이 있는 계정을 사용하여 해당 Azure AD 테넌트에 로그인해야 합니다. 그러나 테넌트를 연결하고나 개발자 센터 계정에 해당 테넌트에 이미 있는 사용자를 추가하기 위해 전역 관리자 권한은 필요하지 않습니다.


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-dev-center-account"></a>개발자 센터 계정에 연결할 새로운 Azure AD 만들기

개발자 센터 계정과 연결할 새 Azure AD를 설정해야 하는 경우 다음 단계를 따릅니다.

1.  **계정 설정**으로 이동하여 **테넌트**를 클릭합니다.
2.  **새 Azure AD 만들기**를 선택합니다.
3.  새 Azure AD에 대한 디렉터리 정보를 입력합니다.
 - **도메인 이름**: Azure AD 도메인에 사용할 고유한 이름으로 ".onmicrosoft.com"과 함께 사용합니다. 예를 들어 "example"을 입력한 경우 Azure AD 도메인은 "example.onmicrosoft.com"이 됩니다.
 - **연락처 메일**: 필요한 경우 계정에 대해 문의할 수 있는 메일 주소입니다.
 - **전역 관리자 사용자 계정 정보**: 새 전역 관리자 계정에 사용할 성, 이름, 사용자 이름 및 암호입니다.
4.  **만들기**를 클릭하여 새 도메인 및 계정 정보를 확인합니다.
5.  새로운 Azure AD 전역 관리자 사용자 이름 및 암호로 로그인하여 [사용자 계정 추가 및 관리](add-users-groups-and-azure-ad-applications.md)를 시작합니다.


## <a name="manage-azure-ad-tenant-associations"></a>Azure AD 테넌트 연결 관리

개발자 센터 계정의 Azure AD 테넌트를 연결하고 나면 **테넌트** 페이지에서 새 테넌트를 추가하거나 기존 테넌트를 제거할 수 있습니다.


### <a name="add-multiple-azure-ad-tenants-to-your-dev-center-account"></a>개발자 센터 계정에 여러 Azure AD 테넌트 추가

개발자 센터 계정에 대해 **관리자** 역할이 있는 모든 사용자는 Azure AD 테넌트를 계정에 연결할 수 있습니다.

새 테넌트를 연결하려면 **다른 Azure AD 테넌트 연결**을 선택한 다음 위에 표시된 단계를 따릅니다. 연결하려는 Azure AD 테넌트에서 자격 증명에 대한 메시지가 표시됩니다.


### <a name="remove-an-azure-ad-tenant-from-your-dev-center-account"></a>개발자 센터 계정에서 Azure AD 테넌트 제거

개발자 센터 계정에 대해 **관리자** 역할이 있는 모든 사용자는 계정에서 Azure AD 테넌트를 제거할 수 있습니다.

> [!IMPORTANT]
> 테넌트를 제거하면 해당 테넌트에서 개발자 센터 계정에 추가한 모든 사용자가 더 이상 계정에 로그인할 수 없게 됩니다. 

테넌트를 제거하려면 **테넌트** 페이지에서 이름을 찾아 **제거**를 선택합니다. 테넌트의 제거를 확인하라는 메시지가 표시됩니다. 이렇게 하면 해당 테넌트의 개발자 센터 사용자가 개발자 센터 계정에 로그인할 수 없게 되고 사용자에 대해 구성한 모든 권한이 제거됩니다.

> [!TIP]
> 현재 동일한 테넌트에서 계정을 사용하여 개발자 센터에 로그인한 경우 테넌트를 제거할 수 없습니다. 테넌트를 제거하려면 계정과 연결된 다른 테넌트에 대한 **관리자**로 개발자 센터에 로그인해야 합니다. 계정에 연결된 테넌트가 하나뿐인 경우 해당 테넌트는 계정을 연 Microsoft 계정으로 로그인한 후 제거될 수 있습니다.


