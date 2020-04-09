---
title: 명명 된 개체 공유
description: 이 항목에서는 유니버설 Windows 플랫폼 (UWP) 응용 프로그램 및 Win32 응용 프로그램 간에 명명 된 개체를 공유 하는 방법에 대해 설명 합니다.
ms.date: 04/06/2020
ms.topic: article
keywords: windows 10, uwp
ms.openlocfilehash: 95bbecd85b1dfa6f6e12766c082f3338de549677
ms.sourcegitcommit: 2d375e1c34473158134475af401532cc55fc50f4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80888575"
---
# <a name="sharing-named-objects"></a>명명 된 개체 공유

이 항목에서는 유니버설 Windows 플랫폼 (UWP) 응용 프로그램 및 Win32 응용 프로그램 간에 명명 된 개체를 공유 하는 방법에 대해 설명 합니다.

## <a name="named-objects-in-packaged-applications"></a>패키지 된 응용 프로그램의 명명 된 개체

[명명 된 개체](/windows/win32/sync/object-names) 는 프로세스가 개체 핸들을 공유 하는 쉬운 방법을 제공 합니다. 프로세스에서 명명 된 개체를 만든 후에는이 이름을 사용 하 여 해당 함수를 호출 하 여 개체에 대 한 핸들을 열 수 있습니다. 명명 된 개체는 일반적으로 [스레드 동기화](/windows/win32/sync/interprocess-synchronization) 및 [프로세스 간 통신](/windows/uwp/communication/interprocess-communication)에 사용 됩니다.

기본적으로 패키지 응용 프로그램은 자신이 만든 명명 된 개체에만 액세스할 수 있습니다. 명명 된 개체를 패키지 된 응용 프로그램과 공유 하려면 개체를 만들 때 사용 권한을 설정 해야 하 고, 개체를 열 때 이름을 정규화 해야 합니다.

## <a name="creating-named-objects"></a>명명 된 개체 만들기

명명 된 개체는 해당 `Create` API를 사용 하 여 생성 됩니다.

* [CreateEvent](/windows/win32/api/synchapi/nf-synchapi-createeventexw)
* [Createfilemapping에서](/windows/win32/api/memoryapi/nf-memoryapi-createfilemappingw)
* [Createmutex가](/windows/win32/api/synchapi/nf-synchapi-createmutexexw)
* [CreateSemaphore](/windows/win32/api/synchapi/nf-synchapi-createsemaphoreexw)
* [CreateWaitableTimer](/windows/win32/api/synchapi/nf-synchapi-createwaitabletimerexw)

이러한 모든 Api는 호출자가 개체에 액세스할 수 있는 프로세스를 제어 하는 [acl (액세스 제어 목록)](/previous-versions/windows/desktop/legacy/aa379560(v=vs.85)) 을 지정할 수 있도록 하는 `LPSECURITY_ATTRIBUTES` 매개 변수를 공유 합니다. 명명 된 개체를 패키지 된 응용 프로그램과 공유 하려면 명명 된 개체를 만들 때 Acl 내에서 사용 권한을 부여 해야 합니다.

Sid (보안 식별자)는 Acl 내의 id를 나타냅니다. 패키지 된 모든 응용 프로그램에는 해당 패키지 제품군 이름을 기반으로 하는 자체 SID가 있습니다. 패키지 제품군 이름을 [DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername)에 전달 하 여 패키지 된 응용 프로그램에 대 한 SID를 생성할 수 있습니다.

> [!NOTE]
> 패키지 패밀리 이름은 개발 시간 동안 Visual Studio의 패키지 매니페스트 편집기를 통해, Microsoft Store을 통해 게시 된 응용 프로그램의 경우 [파트너 센터](/windows/uwp/publish/view-app-identity-details) 를 통해, 이미 설치 된 응용 프로그램의 경우 [add-appxpackage](/powershell/module/appx/get-appxpackage?view=win10-ps) PowerShell 명령을 통해 찾을 수 있습니다.

[이 샘플](/windows/win32/api/securityappcontainer/nf-securityappcontainer-getappcontainernamedobjectpath#examples) 에서는 명명 된 개체의 ACL에 필요한 기본 패턴을 보여 줍니다. 명명 된 개체를 패키지 된 응용 프로그램과 공유 하려면 각 응용 프로그램에 대 한 [EXPLICIT_ACCESS](/windows/win32/api/accctrl/ns-accctrl-explicit_access_w) 구조를 작성 합니다.

* `grfAccessMode = GRANT_ACCESS`
* 개체 및 용도에 따라 적절 한 사용 권한 `grfAccessPermissions =`
    * [일반 액세스 권한](/windows/win32/secauthz/generic-access-rights)
    * [동기화 개체 보안 및 액세스 권한](/windows/win32/sync/synchronization-object-security-and-access-rights)
    * [파일 매핑 보안 및 액세스 권한](/windows/win32/memory/file-mapping-security-and-access-rights)
* `grfInheritance = NO_INHERITANCE`
* `Trustee.TrusteeForm = TRUSTEE_IS_SID`
* `Trustee.TrusteeType = TRUSTEE_IS_USER`
* [DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername) 에서 가져온 SID를 `Trustee.ptstrName =` 합니다.

`Create` 호출의 `LPSECURITY_ATTRIBUTES` 매개 변수를 패키지 된 응용 프로그램에 대 한 `EXPLICIT_ACCESS` 규칙으로 채우면 이러한 응용 프로그램에 대 한 액세스 권한을 부여 하 여 명명 된 개체를 열 수 있습니다.

> [!NOTE]
> Win32 응용 프로그램은 [열](#opening-named-objects)때 개체 이름을 한정 하는 한 패키지 된 응용 프로그램에서 만든 모든 명명 된 개체에 액세스할 수 있습니다. 액세스 권한을 부여 하지 않아도 됩니다.

## <a name="opening-named-objects"></a>명명 된 개체 열기

명명 된 개체는 해당 `Open` API에 이름을 전달 하 여 엽니다.

* [OpenEvent](/windows/win32/api/synchapi/nf-synchapi-openeventw)
* [Openfilemapping이](/windows/win32/api/memoryapi/nf-memoryapi-openfilemappingw)
* [OpenMutex](/windows/win32/api/synchapi/nf-synchapi-openmutexw)
* [OpenSemaphore](/windows/win32/api/synchapi/nf-synchapi-opensemaphorew)
* [OpenWaitableTimer](/windows/win32/api/synchapi/nf-synchapi-openwaitabletimerw)

패키지 된 응용 프로그램에서 생성 된 명명 된 개체는 응용 프로그램의 네임 스페이스 내에 생성 되며, 그렇지 않은 경우에는 명명 된 개체 경로 라고 합니다. 패키지 된 응용 프로그램에서 생성 된 명명 된 개체를 열 때 개체 이름 앞에 응용 프로그램의 명명 된 개체 경로를 붙여야 합니다.

[GetAppContainerNamedObjectPath](/windows/win32/api/securityappcontainer/nf-securityappcontainer-getappcontainernamedobjectpath) 는 해당 SID에 기반 하 여 패키지 된 응용 프로그램에 대 한 명명 된 개체 경로를 반환 합니다. 패키지 제품군 이름을 [DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername)에 전달 하 여 패키지 된 응용 프로그램에 대 한 SID를 생성할 수 있습니다.

> [!NOTE]
> 패키지 패밀리 이름은 개발 시간 동안 Visual Studio의 패키지 매니페스트 편집기를 통해, Microsoft Store을 통해 게시 된 응용 프로그램의 경우 [파트너 센터](/windows/uwp/publish/view-app-identity-details) 를 통해, 이미 설치 된 응용 프로그램의 경우 [add-appxpackage](/powershell/module/appx/get-appxpackage?view=win10-ps) PowerShell 명령을 통해 찾을 수 있습니다.

패키지 된 응용 프로그램에서 생성 된 명명 된 개체를 열 때 `<PATH>\<NAME>`형식을 사용 합니다.

* `<PATH>`를 만들기 응용 프로그램의 명명 된 개체 경로로 바꿉니다.
* `<NAME>`을 개체 이름으로 바꿉니다.

> [!NOTE]
> 패키지 된 응용 프로그램에서 개체를 만든 경우에만 `<PATH>`를 사용 하 여 개체 이름을 접두사로 사용 해야 합니다. 개체를 [만들](#creating-named-objects)때 액세스 권한을 부여 해야 하지만 Win32 응용 프로그램에서 생성 된 명명 된 개체를 한정할 필요는 없습니다.

## <a name="remarks"></a>설명

패키지 응용 프로그램의 명명 된 개체는 보안을 유지 하 고 일시 중단 및 종료와 같은 응용 프로그램 수명 주기 이벤트를 지원 하기 위해 기본적으로 격리 됩니다. 응용 프로그램 간에 명명 된 개체를 공유 하면 엄격한 바인딩 및 버전 관리 제약 조건이 적용 되며, 각 응용 프로그램이 다른 응용 프로그램의 수명 주기 동안 복원 될 수 있어야 합니다. 이러한 이유로 동일한 게시자의 응용 프로그램 간에 명명 된 개체만 공유 하는 것이 좋습니다.
