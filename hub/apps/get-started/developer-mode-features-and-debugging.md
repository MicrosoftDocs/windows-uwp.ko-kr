---
title: 개발자 모드 기능 및 디버깅
description: Windows 10에서 개발자 모드의 기능에 대해 자세히 알아보고 설치 오류를 읽으십시오.
keywords: 개발자 라이선스 Visual Studio 시작, 개발자 라이선스 디바이스 활성화
ms.date: 10/13/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 85c64cd4baed7be9edde0dfc008ab90c1d537ca4
ms.sourcegitcommit: d0eef123b167dc63f482a9f4432a237c1c6212db
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077231"
---
# <a name="developer-mode-features-and-debugging"></a>개발자 모드 기능 및 디버깅

앱에 개발자 모드를 설치 하는 기본 사항에 관심이 있는 경우 [개발을 위해 장치를 사용 하도록 설정](enable-your-device-for-development.md) 에 설명 된 지침에 따라 시작할 수 있습니다. 이 문서에서는 개발자 모드의 고급 기능, 이전 버전의 Windows 10에 대 한 개발자 모드 및 개발자 모드 설치의 디버깅 오류에 대해 설명 합니다.

## <a name="additional-developer-mode-features"></a>추가 개발자 모드 기능

각 디바이스 패밀리의 경우 추가적인 개발자 기능을 사용할 수 있습니다. 이러한 기능은 디바이스에서 개발자 모드를 사용하는 경우에만 사용할 수 있고 OS 버전에 따라 달라질 수 있습니다.

이 이미지는 Windows 10에 대한 개발자 기능을 표시합니다.

![개발자 모드 옵션](images/devmode-mob-options.png)

### <a name="span-iddevice-discovery-and-pairingspandevice-portal"></a><span id="device-discovery-and-pairing"></span>디바이스 포털

디바이스 포털에 대한 자세한 내용은 [Windows Device Portal 개요](/windows/uwp/debug-test-perf/device-portal.md)를 참조하세요.


디바이스별 설치 지침은 다음을 참조하세요.
- [데스크톱 디바이스 포털](/windows/uwp/debug-test-perf/device-portal-desktop)
- [HoloLens용 디바이스 포털](/windows/mixed-reality/using-the-windows-device-portal)
- [IoT용 디바이스 포털](/windows/iot-core/manage-your-device/deviceportal)
- [모바일용 디바이스 포털](/windows/uwp/debug-test-perf/device-portal-mobile)
- [Xbox용 디바이스 포털](/windows/uwp/xbox-apps/device-portal-xbox)

개발자 모드 또는 디바이스 포털을 사용하도록 설정하는 데 문제가 있는 경우 [알려진 문제](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22) 포럼에서 이 문제에 대한 해결 방법을 찾아보거나 [개발자 모드 패키지 설치 실패](#failure-to-install-developer-mode-package)를 방문하여 추가 세부 정보를 살펴보고 개발자 모드 패키지의 차단을 해제하려면 어떤 WSUS KB를 허용해야 하는지 확인하세요.

### <a name="sideload-apps"></a>앱 테스트용 로드

> [!IMPORTANT]
> 최신 Windows 10 업데이트를 사용 하 여 기본적으로 테스트용 로드를 사용 하도록 설정 했으므로이 설정은 표시 되지 않습니다. 이전 버전의 Windows 10을 사용하는 경우 기본적으로 Microsoft Store의 앱만 실행할 수 있도록 설정되며, Microsoft가 아닌 소스의 앱을 설치하려면 테스트용 로드를 사용하도록 설정해야 합니다.

테스트용으로 앱 로드 설정은 일반적으로 Microsoft Store를 거치지 않고 관리 디바이스에 사용자 지정 앱을 설치해야 하는 회사나 학교에서 사용하거나 타사 소스에서 앱을 실행해야 하는 모든 사용자가 사용합니다. 이 경우 이전에 설정 페이지 이미지에 표시된 대로 조직에서 *UWP 앱* 설정을 사용하지 않는 정책을 적용하는 경우가 일반적입니다. 또한 조직에서는 필요한 인증서 및 테스트용 로드 앱의 설치 위치를 제공합니다. 자세한 내용은 TechNet 문서 [Windows 10에서 앱 사이드로드](/windows/deploy/sideload-apps-in-windows-10) 및 [Microsoft Intune 기본 사항](/mem/intune/fundamentals/)을 참조하세요.

디바이스 패밀리 관련 정보

-   데스크톱 디바이스 패밀리에서: 앱 패키지(.appx) 및 패키지("Add-AppDevPackage.ps1")로 만든 Windows PowerShell 스크립트를 실행하여 앱을 실행하는 데 필요한 모든 인증서를 설치할 수 있습니다. 자세한 내용은 [UWP 앱 패키징](/windows/msix/package/packaging-uwp-apps)을 참조하세요.

-   모바일 디바이스 패밀리에서: 필요한 인증서가 이미 설치되어 있는 경우 파일을 탭하여 이메일로 전송되거나 SD 카드에 있는 .appx를 설치할 수 있습니다.


**앱 테스트용 로드** 를 사용하면 신뢰할 수 있는 인증서가 없는 디바이스에 앱을 설치할 수 없으므로 개발자 모드보다 안전합니다.

> [!NOTE]
> 앱을 테스트용으로 로드하는 경우에도 여전히 신뢰할 수 있는 소스의 앱만을 설치해야 합니다. Microsoft Store에서 인증되지 않은 테스트용 로드 앱을 설치할 경우 이러한 앱을 테스트용으로 로드하는 데 필요한 모든 권한을 확보했으며 이러한 앱을 설치하고 실행하여 발생할 수 있는 피해에 대해 전적으로 책임을 진다는 점에 동의하는 것입니다. 이 [개인정보처리방침](https://privacy.microsoft.com/privacystatement)의 Windows &gt; Microsoft Store 섹션을 참조하세요.


### <a name="ssh"></a>SSH

디바이스에서 디바이스 검색을 사용하도록 설정할 경우에 SSH 서비스를 사용하도록 설정됩니다.  이 서비스는 디바이스가 UWP 애플리케이션용 원격 배포 대상인 경우에 사용됩니다.   서비스의 이름은 'SSH Server Broker' 및 'SSH Server Proxy'입니다.

> [!NOTE]
> SSH는 [GitHub](https://github.com/PowerShell/Win32-OpenSSH)에서 찾을 수 있는 Microsoft의 OpenSSH 구현이 아닙니다.  

SSH 서비스를 활용하기 위해 디바이스 검색을 사용하여 핀 페어링을 허용할 수 있습니다. 다른 SSH 서비스를 실행하려는 경우 다른 포트에서 설정하거나 개발자 모드 SSH 서비스를 끌 수 있습니다. SSH 서비스를 끄려면 디바이스 검색을 해제합니다.  

인증을 위한 암호를 수락하는 "DevToolsUser" 계정을 통해 SSH 로그인이 수행됩니다.  이 암호는 디바이스 검색 "페어링" 단추를 누른 후에 디바이스에 표시되는 PIN으로, PIN이 표시되는 동안에만 유효합니다.  SFTP 하위 시스템은 Visual Studio로부터 느슨한 파일 배포가 설치되는 DevelopmentFiles 폴더를 수동 관리하는 데도 사용됩니다.

#### <a name="caveats-for-ssh-usage"></a>SSH 사용에 대한 주의 사항
Windows에서 사용되는 기존 SSH 서버는 아직 프로토콜과 호환되지 않기 때문에 SFTP 또는 SSH 클라이언트를 사용하기 위해서는 특별 구성이 필요할 수 있습니다.  특히, SFTP 하위 시스템은 버전 3 이하에서 실행되므로 이전 서버를 예상할 수 있도록 모든 연결 클라이언트를 구성해야 합니다.  이전 버전의 디바이스의 SSH 서버는 `ssh-dss`OpenSSH에서 지원이 중단된 공개 키 인증을 사용합니다.  이러한 디바이스를 연결하려면 `ssh-dss`를 수락하도록 SSH 클라이언트를 수동으로 구성해야 합니다.  

### <a name="device-discovery"></a>디바이스 검색

디바이스 검색을 사용하는 경우 mDNS를 통해 디바이스가 네트워크의 다른 디바이스에 표시되도록 허용합니다.  이 기능을 사용하면 디바이스 검색을 사용하도록 설정할 때 나타나는 "페어링" 단추를 눌러서 이 디바이스에 페어링할 SSH PIN을 얻을 수 있습니다.  이 디바이스를 대상으로 하는 첫 번째 Visual Studio 배포를 완료하려면 이러한 PIN 프롬프트가 화면에 표시되어야 합니다.  

![핀 페어링](images/devmode-pc-pinpair.PNG)

디바이스를 배포 대상으로 할 경우에만 디바이스 검색을 사용해야 합니다. 예를 들어 Device Portal을 사용하여 테스트용으로 휴대폰에 앱을 배포할 경우 휴대폰에서 디바이스 검색을 사용해야 하지만 개발 PC에서는 사용하지 않습니다.

### <a name="optimizations-for-windows-explorer-remote-desktop-and-powershell-desktop-only"></a>Windows 탐색기, 원격 데스크톱 및 PowerShell에 최적화(데스크톱에만 해당)

 데스크톱 디바이스 패밀리의 **개발자용** 설정 페이지에는 개발 작업을 위해 PC를 최적화하는 데 사용할 수 있는 설정에 대한 바로 가기가 있습니다. 각 설정에서 확인란을 선택하고 **적용** 을 클릭하거나 **설정 표시** 링크를 클릭하여 해당 옵션의 설정 페이지를 열 수 있습니다.


## <a name="notes"></a>참고
Windows 10 Mobile의 초기 버전에서 크래시 덤프 옵션은 개발자 설정 메뉴에 있었습니다.  이 옵션이 [디바이스 포털](/windows/uwp/debug-test-perf/device-portal.md)로 이동되었으므로 USB를 통하기 보다는 원격으로 사용할 수 있습니다.  

Windows 10 PC에서 Windows 10 디바이스로 앱을 배포할 수 있도록 해주는 몇 가지 도구가 있습니다. 두 디바이스는 유선 또는 무선으로 연결하여 동일한 네트워크 서브넷에 연결되거나 USB로 연결되어야 합니다. 나열된 두 방법 모두 앱 패키지(.appx/.appxbundle)만 설치하고 인증서는 설치하지 않습니다.

-   Windows 10 응용 프로그램 배포(WinAppDeployCmd) 도구를 사용합니다. 자세한 내용은 [the WinAppDeployCmd 도구](/previous-versions/windows/apps/mt203806(v=vs.140))를 참조하세요.
-   [디바이스 포털](/windows/uwp/debug-test-perf/device-portal.md)을 사용하면 브라우저에서 Windows 10, 버전 1511 이상을 실행하는 모바일 디바이스로 배포할 수 있습니다. Device Portal의 **[앱](/windows/uwp/debug-test-perf/device-portal.md#apps-manager)** 페이지를 사용하여 앱 패키지(.appx)를 업로드하고 디바이스에 설치할 수 있습니다.

## <a name="failure-to-install-developer-mode-package"></a>개발자 모드 패키지 설치 실패
경우에 따라 네트워크 또는 관리 문제로 인해 개발자 모드가 제대로 설치되지 않습니다. 이 PC에 **원격** 으로 배포하려면, 다시 말해서 브라우저 또는 디바이스 검색에서 디바이스 포털을 사용하여 SSH를 설정하려면 개발자 모드 패키지가 필요하지만 로컬 배포에는 필요하지 않습니다.  이러한 문제가 발생하더라도 Visual Studio를 사용하여 앱을 로컬로 배포하거나 이 디바이스에서 다른 디바이스로 배포할 수 있습니다.

[알려진 문제](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22) 포럼을 참조하여 이러한 문제에 대한 해결 방법 등을 찾을 수 있습니다.

> [!NOTE]
> 개발자 모드가 제대로 설치되지 않으면 피드백 요청을 제출하는 것이 좋습니다. **피드백 허브** 앱에서 **새 피드백 추가** 를 선택하고 **개발자 플랫폼** 범주와 **개발자 모드** 하위 범주를 선택합니다. 피드백을 제출하면 발생한 문제를 Microsoft에서 해결하는 데 도움이 됩니다.

### <a name="failed-to-locate-the-package"></a>패키지 찾기 실패

"개발자 모드 패키지가 Windows 업데이트에서 없을 수 있습니다. 오류 코드 0x80004005 자세한 정보"   

이 오류는 네트워크 연결 문제, 엔터프라이즈 설정 또는 패키지 누락으로 인해 발생할 수 있습니다.

이 문제를 해결하려면

1. 컴퓨터가 인터넷에 연결되어 있는지 확인합니다.
2. 도메인에 가입된 컴퓨터를 사용하는 경우 네트워크 관리자에게 문의하세요. 모든 주문형 기능처럼 개발자 모드 패키지는 기본적으로 WSUS에서 차단됩니다.
2.1. 현재 및 이전 버전에서 개발자 모드 패키지의 잠금을 해제하려면 WSUS에서 KB 4016509, 3180030, 3197985를 허용해야 합니다.  
3. 설정 > 업데이트 및 보안 > Windows 업데이트에서 Windows 업데이트가 있는지 확인합니다.
4. 설정 > 시스템 > 앱 및 기능 > 선택적 기능 관리 > 기능 추가에서 Windows 개발자 모드 패키지가 있는지 확인합니다. 없는 경우 Windows는 컴퓨터에 적합한 패키지를 찾을 수 없습니다.

위의 단계를 수행한 후 개발자 모드를 사용하도록 설정했다가 다시 사용하지 않도록 설정하여 문제를 해결하세요.


### <a name="failed-to-install-the-package"></a>패키지 설치 실패

"개발자 모드 패키지를 설치하지 못했습니다. 오류 코드 0x80004005 자세한 정보"

이 오류는 사용 중인 Windows 빌드와 개발자 모드 패키지 간의 비호환성으로 인해 발생할 수 있습니다.

이 문제를 해결하려면

1. 설정 > 업데이트 및 보안 > Windows 업데이트에서 Windows 업데이트가 있는지 확인합니다.
2. 컴퓨터를 다시 부팅하여 모든 업데이트가 적용되도록 합니다.


## <a name="use-group-policies-or-registry-keys-to-enable-a-device"></a>그룹 정책 또는 레지스트리 키를 사용하여 디바이스를 사용하도록 설정

대부분의 개발자의 경우 디바이스를 디버깅에 사용할 수 있도록 설정 앱을 사용하려고 합니다. 자동화된 테스트와 같은 특정 시나리오에서 다른 방법으로 개발용 Windows 10 데스크톱 디바이스를 사용할 수 있습니다.  이러한 단계들은 SSH 서버를 사용하도록 설정하거나 원격 배포 및 디버깅을 위한 대상으로 허용하지 않는다는 점에 유의하세요.

Windows 10 Home을 설치한 경우가 아니라면 gpedit.msc를 사용하여 디바이스를 사용하도록 그룹 정책을 설정할 수 있습니다. Windows 10 Home을 설치한 경우 regedit 또는 PowerShell 명령을 사용하여 디바이스를 사용하도록 레지스트리 키를 직접 설정해야 합니다.

**gpedit를 사용하여 디바이스를 사용하도록 설정**

1.  **Gpedit.msc** 를 실행합니다.
2.  로컬 컴퓨터 정책 &gt; 컴퓨터 구성 &gt; 관리 템플릿 &gt; Windows 구성 요소 &gt; 앱 패키지 배포로 이동합니다.
3.  테스트용 로드를 사용하도록 설정하려면 다음을 사용하도록 정책을 편집합니다.

    -   **Allow all trusted apps to install**(모든 신뢰할 수 있는 앱을 설치할 수 있음)

    또는

    개발자 모드를 사용하도록 설정하려면 아래 두 항목을 모두 사용하도록 정책을 편집합니다.

    -   **Allow all trusted apps to install**(모든 신뢰할 수 있는 앱을 설치할 수 있음)
    -   **UWP 앱을 개발하고 IDE(통합 개발 환경)에서 이를 설치하도록 허용**

4.  컴퓨터를 다시 부팅합니다.

**regedit를 사용하여 디바이스를 사용하도록 설정**

1.  **regedit** 를 실행합니다.
2.  테스트용 로드를 사용하도록 설정하려면 이 DWORD 값을 1로 설정합니다.

    -   `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock\AllowAllTrustedApps`

    또는

    개발자 모드를 사용하도록 설정하려면 이 DWORD 값을 1로 설정합니다.

    -   `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock\AllowDevelopmentWithoutDevLicense`

**PowerShell을 사용하여 디바이스를 사용하도록 설정**

1.  관리자 권한으로 PowerShell을 실행합니다.
2.  테스트용 로드를 사용하도록 설정하려면 다음 명령을 실행합니다.

    ```powershell
    PS C:\WINDOWS\system32> reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock" /t REG_DWORD /f /v "AllowAllTrustedApps" /d "1"
    ```

    또는

    개발자 모드를 사용하도록 설정하려면 다음 명령을 실행합니다.

    ```powershell
    PS C:\WINDOWS\system32> reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock" /t REG_DWORD /f /v "AllowDevelopmentWithoutDevLicense" /d "1"
    ```

## <a name="upgrade-your-device-from-windows-81-to-windows-10"></a>Windows 8.1에서 Windows 10으로 디바이스 업그레이드

Windows 8.1 디바이스에서 앱을 만들거나 테스트용으로 로드할 때는 개발자 라이선스를 설치해야 합니다. Windows 8.1에서 Windows 10으로 디바이스를 업그레이드하는 경우에도 이 정보가 유지됩니다. 업그레이드된 Windows 10 디바이스에서 이 정보를 제거하려면 다음 명령을 실행합니다. Windows 8.1에서 Windows 10 버전 1511 이상으로 직접 업그레이드하는 경우에는 이 단계가 필요하지 않습니다.

**개발자 라이선스를 등록 취소하려면**

1.  관리자 권한으로 PowerShell을 실행합니다.
2.  `unregister-windowsdeveloperlicense` 명령을 실행합니다.

이후에도 이 디바이스에서 계속 개발하려면 이 항목에 설명된 대로 디바이스를 개발용으로 설정해야 합니다. 그러지 않으면 앱을 디버그하거나 앱의 패키지를 만들려고 할 때 오류가 발생할 수 있습니다. 다음은 이러한 오류의 예입니다.

오류: DEP0700 : 앱을 등록하지 못했습니다.