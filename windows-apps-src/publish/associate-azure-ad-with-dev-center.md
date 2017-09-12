---
author: jnHs
Description: "계정 사용자를 추가하고 관리하려면 먼저 개발자 센터 계정을 조직의 Azure Active Directory와 연결해야 합니다."
title: "개발자 센터 계정에 Azure Active Directory 연결"
ms.author: wdg-dev-content
ms.date: 07/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: ace9470cc707206461baa8c3dd72828ea68a8eb4
ms.sourcegitcommit: eaacc472317eef343b764d17e57ef24389dd1cc3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/17/2017
---
# <a name="associate-azure-active-directory-with-your-dev-center-account"></a>개발자 센터 계정에 Azure Active Directory 연결

[계정 사용자를 추가하고 관리하려면](add-users-groups-and-azure-ad-applications.md) 먼저 개발자 센터 계정을 조직의 Azure Active Directory와 연결해야 합니다. 

Windows 개발자 센터는 다중 사용자 관리 및 역할 할당을 위해 Azure AD를 활용합니다. 조직에서 이미 Office 365 또는 Microsoft의 다른 비즈니스 서비스를 사용하는 경우 이미 Azure AD가 있습니다. 아니면 추가 요금 없이 개발자 센터 내에서 Azure AD 테넌트를 새로 만들 수 있습니다.

> [!IMPORTANT]
> 개발자 센터 계정에 Azure AD를 연결하려면 [전역 관리자](http://go.microsoft.com/fwlink/?LinkId=746654) 계정으로 Azure AD 테넌트에 로그인해야 합니다.
> 
> Azure AD와 개발자 센터 계정을 연결하고 나면 계정 사용자를 추가하고 관리하기 위해 Azure AD 전역 관리자 계정을 사용하여(개인 Microsoft 계정은 안 됨) 개발자 센터에 항상 로그인해야 합니다.

Azure AD 테넌트에는 단 하나의 개발자 센터 계정만 연결할 수 있습니다. 마찬가지로 개발자 센터 계정은 단 하나의 Azure AD 테넌트만 연결할 수 있습니다. 이 연결을 설정하면 제거할 때 고객 지원에 반드시 문의해야 합니다.


## <a name="associate-your-dev-center-account-with-your-organizations-existing-azure-ad-tenant"></a>개발자 센터 계정을 조직의 기존 Azure AD 테넌트와 연결

조직에서 이미 Azure AD를 사용하는 경우 개발자 센터 계정에 연결하려면 다음 단계를 따릅니다.

1.  **계정 설정**으로 이동하여 **사용자 관리**를 클릭합니다.
2.  **Azure AD와 개발자 센터 계정 연결** 단추를 클릭합니다.
3.  Azure AD 계정에 로그인합니다. 연결을 설정하려면 이 계정에 [전역 관리자](http://go.microsoft.com/fwlink/?LinkId=746654) 권한이 있어야 합니다.
4.  Azure AD 테넌트에 대한 조직 및 도메인 이름을 검토합니다. 연결을 완료하려면 **확인**을 클릭합니다.
5.  연결이 되면 개발자 센터의 **사용자 관리** 섹션에서 사용자를 추가하고 관리할 준비가 됩니다.


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-dev-center-account"></a>개발자 센터 계정에 연결할 새로운 Azure AD 만들기

개발자 센터 계정과 연결할 새 Azure AD를 설정해야 하는 경우 다음 단계를 따릅니다.

1.  **계정 설정**으로 이동하여 **사용자 관리**를 클릭합니다.
2.  **Create new Azure AD**(새 Azure AD 만들기) 단추를 클릭합니다.
3.  새 Azure AD에 대한 디렉터리 정보를 입력합니다.
 - **도메인 이름**: Azure AD 도메인에 사용할 고유한 이름으로 ".onmicrosoft.com"과 함께 사용합니다. 예를 들어 "example"을 입력한 경우 Azure AD 도메인은 "example.onmicrosoft.com"이 됩니다.
 - **연락처 메일**: 필요한 경우 계정에 대해 문의할 수 있는 메일 주소입니다.
 - **전역 관리자 사용자 계정 정보**: 새 전역 관리자 계정에 사용할 성, 이름, 사용자 이름 및 암호입니다.
4.  **만들기**를 클릭하여 새 도메인 및 계정 정보를 확인합니다.
5.  새로운 Azure AD 전역 관리자 사용자 이름 및 암호로 로그인하여 [사용자 계정 추가 및 관리](add-users-groups-and-azure-ad-applications.md)를 시작합니다.



