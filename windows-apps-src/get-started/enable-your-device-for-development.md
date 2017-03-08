---
author: GrantMeStrength
ms.assetid: 54973C62-9669-4988-934E-9273FB0425FD
title: "디바이스를 개발에 사용하도록 설정"
description: "개발 및 디버깅을 위해 Windows 10 디바이스를 구성합니다."
keywords: "개발자 라이선스 Visual Studio 시작, 개발자 라이선스 디바이스 활성화"
ms.author: jken
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: dc1bf476c93ef9843c20244f24a199c7888eb4a5
ms.lasthandoff: 02/07/2017

---

# <a name="enable-your-device-for-development"></a>디바이스를 개발에 사용하도록 설정

앱을 작성하기 전에 개발 PC 및 코드를 테스트할 디바이스에서 모두 개발자 모드를 사용하도록 설정해야 합니다. 

![디바이스를 개발에 사용하도록 설정](images/developer-poster.png)

## <a name="use-developer-features"></a>개발자 기능 사용

### <a name="develop-your-app-with-microsoft-visual-studio"></a>Microsoft Visual Studio를 사용하여 앱 개발

Visual Studio에서 UWP 앱 프로젝트를 열기 전에 PC에서 개발자 모드를 사용하도록 설정해야 합니다. UWP 프로젝트를 열고 개발자 모드를 사용할 수 없는 경우 **개발자용** 설정 페이지가 자동으로 열립니다. 개발자 모드를 사용하도록 설정하려면 다음 섹션의 지침을 따릅니다.

Windows 10 버전 1511 이하에서 Visual Studio의 UWP 앱 프로젝트를 열면 Visual Studio에서 이 대화 상자가 표시됩니다. 

![Visual Studio에서 표시되는 개발자 모드 대화 상자를 사용하도록 설정](images/latestenabledialog.png)

이 대화 상자가 표시되면 **개발자용 설정**을 클릭하여 **개발자용** 설정 페이지를 열고 개발자 모드를 사용하도록 설정합니다.

> 언제든지 **개발자용** 페이지로 이동하여 개발자 모드 사용 여부를 설정할 수 있습니다. 작업 표시줄의 Cortana 검색 상자에 "개발자 설정"을 입력하기만 하면 합니다.

### <a name="enable-your-windows-10-devices"></a>Windows 10 장치를 사용하도록 설정

장치를 개발 또는 단순히 테스트용 로드에 사용할 수 있습니다.

-   *테스트용 로드*는 Windows 스토어에서 인증되지 않은 앱을 설치한 후 실행 또는 테스트하는 것입니다. 예를 들어 회사에서만 사용하는 내부용 앱입니다.
-   *개발자 모드*를 사용하면 앱을 테스트용으로 로드할 수 있을 뿐만 아니라 Visual Studio에서 디버그 모드로 앱을 실행할 수 있습니다. 

    개발자 모드를 사용하는 경우 다음이 포함된 옵션 패키지가 설치됩니다.
    - Windows Device Portal을 설치합니다. **디바이스 포털 사용** 옵션이 켜진 경우에만 Device Portal을 사용할 수 있고 방화벽 규칙이 구성됩니다.
    - 앱의 원격 설치를 허용하는 SSH 서비스에 대한 방화벽 규칙을 설치, 사용 및 구성합니다.
    - (데스크톱에만 해당) Linux용 Windows 하위 시스템의 사용을 허용합니다. 자세한 내용은 [Windows의 Ubuntu 기반 Bash 정보](https://msdn.microsoft.com/commandline/wsl/about)를 참조하세요.

옵션에 대한 자세한 내용은 [선택해야 하는 설정: 테스트용으로 앱 로드 또는 개발자 모드](https://msdn.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#which-settings-should-i-choose-sideload-apps-or-developer-mode)를 참조하세요.

**개발자 기능을 사용하려면**

1.  사용하도록 설정하려는 장치에서 **설정**으로 이동합니다. **업데이트 및 보안**을 선택한 다음 **개발자용**을 선택합니다.
2.  UWP 앱 개발에 필요한 액세스 수준을 선택하고 **개발자 모드**를 선택합니다. 
3.  선택한 설정에 대한 고지 사항을 읽은 다음 **예**를 클릭하여 변경 내용을 적용합니다.

> [!NOTE]
> 디바이스가 조직의 소유인 경우 다음과 같이 일부 옵션을 조직에서 사용하지 못할 수 있습니다.

다음은 데스크톱 디바이스 패밀리의 설정 페이지입니다.

![설정으로 이동하고 업데이트 및 보안을 선택한 후 개발자용을 선택하여 옵션 확인](images/devmode-pc-options.png)

다음은 모바일 디바이스 패밀리의 설정 페이지입니다.

![휴대폰의 설정에서 업데이트 및 보안 선택](images/devmode-mob.png)

## <a name="developer-mode-features"></a>개발자 모드 기능

각 디바이스 패밀리의 경우 추가적인 개발자 기능을 사용할 수 있습니다. 이러한 기능은 디바이스에서 개발자 모드를 사용하는 경우에만 사용할 수 있고 OS 버전에 따라 달라질 수 있습니다.

이 이미지는 Windows 10 버전 1511에서 모바일 디바이스 패밀리에 대한 개발자 기능을 보여 줍니다.

![모바일 디바이스에 대한 개발자 모드 옵션](images/devmode-mob-options.png) 

### <a name="span-iddevice-discovery-and-pairingspandevice-portal"></a><span id="device-discovery-and-pairing"></span>디바이스 포털

디바이스 검색 및 디바이스 포털에 대한 자세한 내용은 [Windows Device Portal 개요](../debug-test-perf/device-portal.md)를 참조하세요.

디바이스별 설치 지침은 다음을 참조하세요.
- [데스크톱 디바이스 포털](https://msdn.microsoft.com/windows/uwp/debug-test-perf/device-portal-desktop)
- [HoloLens용 디바이스 포털](https://developer.microsoft.com/windows/holographic/using_the_windows_device_portal)
- [IoT용 디바이스 포털](https://developer.microsoft.com/windows/iot/docs/DevicePortal)
- [모바일용 디바이스 포털](../debug-test-perf/device-portal-mobile.md)
- [Xbox용 디바이스 포털](../debug-test-perf/device-portal-xbox.md)

개발자 모드 또는 Device Portal 사용에 문제가 발생하면 [알려진 문제](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22) 포럼을 참조하여 이러한 문제에 대한 해결 방법을 찾을 수 있습니다. 

###<a name="ssh"></a>SSH

디바이스에서 개발자 모드를 사용할 경우에 SSH 서비스를 사용할 수 있습니다.  이 서비스는 디바이스가 UWP 응용 프로그램용 배포 대상인 경우에 사용됩니다.   서비스의 이름은 'SSH Server Broker' 및 'SSH Server Proxy'입니다.

> [!NOTE]
> SSH는 [GitHub](https://github.com/PowerShell/Win32-OpenSSH)에서 찾을 수 있는 Microsoft의 OpenSSH 구현이 아닙니다.

SSH 서비스를 활용하기 위해 디바이스 검색을 사용하여 핀 페어링을 허용할 수 있습니다. 다른 SSH 서비스를 실행하려는 경우 다른 포트에서 설정하거나 개발자 모드 SSH 서비스를 끌 수 있습니다. SSH 서비스를 끄려면 개발자 모드를 사용하지 않도록 설정하면 됩니다.  

### <a name="device-discovery"></a>디바이스 검색

디바이스 검색을 사용하는 경우 mDNS를 통해 디바이스가 네트워크의 다른 디바이스에 표시되도록 허용합니다.  또한 이 기능을 통해 이 디바이스에 페어링할 SSH 핀을 얻을 수 있습니다.  

![핀 페어링](images/devmode-pc-pinpair.PNG)

디바이스를 배포 대상으로 할 경우에만 디바이스 검색을 사용해야 합니다. 예를 들어 Device Portal을 사용하여 테스트용으로 휴대폰에 앱을 배포할 경우 휴대폰에서 디바이스 검색을 사용해야 하지만 개발 PC에서는 사용하지 않습니다.

### <a name="error-reporting-mobile-only"></a>오류 보고(모바일만 해당)

이 값을 설정하여 휴대폰에 저장되는 크래시 덤프 수를 지정할 수 있습니다.

휴대폰에서 크래시 덤프를 수집하면 크래시가 발생한 직후 중요한 크래시 정보에 즉시 액세스할 수 있습니다. 덤프는 개발자가 서명한 앱에 대해서만 수집됩니다. Documents\\Debug 폴더의 휴대폰 저장소에서 덤프를 찾을 수 있습니다. 덤프 파일에 대한 자세한 내용은 [덤프 파일 사용](https://msdn.microsoft.com/library/d5zhxt22.aspx)을 참조하세요.

### <a name="optimizations-for-windows-explorer-remote-desktop-and-powershell-desktop-only"></a>Windows 탐색기, 원격 데스크톱 및 PowerShell에 최적화(데스크톱에만 해당)

 데스크톱 디바이스 패밀리의 **개발자용** 설정 페이지에는 개발 작업을 위해 PC를 최적화하는 데 사용할 수 있는 설정에 대한 바로 가기가 있습니다. 각 설정에서 확인란을 선택하고 **적용**을 클릭하거나 **설정 표시** 링크를 클릭하여 해당 옵션의 설정 페이지를 열 수 있습니다. 

## <a name="which-settings-should-i-choose-sideload-apps-or-developer-mode"></a>선택해야 하는 설정: 테스트용으로 앱 로드 또는 개발자 모드

기본적으로 Windows 스토어의 UWP(유니버설 Windows 플랫폼) 앱만 설치할 수 있습니다. 개발자 기능을 사용하도록 이러한 설정을 변경하면 장치의 보안 수준을 변경할 수 있습니다. 확인되지 않은 원본에서 앱을 설치하지 않아야 합니다.

### <a name="sideload-apps"></a>앱 테스트용 로드

테스트용 로드 앱 설정은 일반적으로 Windows 스토어를 거치지 않고 관리되는 장치에 사용자 지정 앱을 설치해야 하는 회사 또는 학교에서 사용합니다. 이 경우 이전에 설정 페이지 이미지에 표시된 대로 조직에서 *Windows 스토어 앱* 설정을 사용하지 않는 정책을 적용하는 경우가 일반적입니다. 또한 조직에서는 필요한 인증서 및 테스트용 로드 앱의 설치 위치를 제공합니다. 자세한 내용은 TechNet 문서 [Windows 10에서 앱을 테스트용으로 로드](https://technet.microsoft.com/library/mt269549.aspx) 및 [Microsoft Intune에서 앱 배포 시작](https://technet.microsoft.com/library/dn646955.aspx)을 참조하세요.

디바이스 패밀리 관련 정보

-   데스크톱 디바이스 패밀리에서: 앱 패키지(.appx) 및 패키지("Add-AppDevPackage.ps1")로 만든 Windows PowerShell 스크립트를 실행하여 앱을 실행하는 데 필요한 모든 인증서를 설치할 수 있습니다. 자세한 내용은 [UWP 앱 패키징](../packaging/packaging-uwp-apps.md)을 참조하세요.

-   모바일 디바이스 패밀리에서: 필요한 인증서가 이미 설치되어 있는 경우 파일을 탭하여 메일로 전송되거나 SD 카드에 있는 .appx를 설치할 수 있습니다.

**앱 테스트용 로드**를 사용하면 신뢰할 수 있는 인증서가 없는 디바이스에 앱을 설치할 수 없으므로 개발자 모드보다 안전합니다.

> [!NOTE]
> 앱을 테스트용으로 로드하는 경우에도 여전히 신뢰할 수 있는 소스의 앱만을 설치해야 합니다. Windows 스토어에서 인증되지 않은 테스트용 로드 앱을 설치할 경우 이러한 앱을 테스트용으로 로드하는 데 필요한 모든 권한을 확보했으며 이러한 앱을 설치하고 실행하여 발생할 수 있는 피해에 대해 전적으로 책임을 진다는 점에 동의하는 것입니다. 이 [개인 정보 취급 방침](http://go.microsoft.com/fwlink/?LinkId=521839)의 Windows &gt; Windows 스토어 섹션을 참조하세요.

### <a name="developer-mode"></a>개발자 모드

개발자 모드는 개발자 라이선스에 대한 Windows 8.1 요구 사항을 대체합니다.  테스트용 로드 외에도 개발자 모드 설정을 사용하면 디버깅 및 추가 배포 옵션을 사용하도록 설정할 수 있습니다. 여기에는 이 디바이스에 배포할 수 있도록 허용하는 SSH 서비스 시작이 포함됩니다. 이 서비스를 중지하려면 개발자 모드를 사용하지 않도록 설정해야 합니다.

디바이스 패밀리 관련 정보

-   데스크톱 디바이스 패밀리에서:

    개발자 모드를 사용하여 Visual Studio에서 앱을 개발하고 디버그합니다. 앞에서 설명한대로 개발자 모드를 사용하도록 설정하지 않으면 Visual Studio에서 메시지가 표시됩니다.

    Linux용 Windows 하위 시스템의 사용을 허용합니다. 자세한 내용은 [Windows의 Ubuntu 기반 Bash 정보](https://msdn.microsoft.com/commandline/wsl/about)를 참조하세요.

-   모바일 디바이스 패밀리에서:

    개발자 모드를 사용하여 Visual Studio에서 앱을 배포하고 장치에서 디버그합니다.

    파일을 탭하여 메일로 전송되거나 SD 카드에 있는 .appx를 설치할 수 있습니다. 확인되지 않은 원본에서 앱을 설치하지 마세요.

**팁**  
여러 도구를 사용하여 Windows 10 PC에서 Windows 10 Mobile 디바이스로 앱을 배포할 수 있습니다. 두 장치는 유선 또는 무선으로 연결하여 동일한 네트워크 서브넷에 연결되거나 USB로 연결되어야 합니다. 나열된 방법은 앱 패키지(.appx)만 설치하고 인증서는 설치하지 않습니다.

-   Windows 10 응용 프로그램 배포(WinAppDeployCmd) 도구를 사용합니다. 자세한 내용은 [the WinAppDeployCmd 도구](http://msdn.microsoft.com/library/windows/apps/mt203806.aspx)를 참조하세요.
-   Windows 10 버전 1511부터 [장치 포털](#device_portal)을 사용하여 브라우저에서 Windows 10 버전 1511 이상을 실행하는 모바일 장치로 배포할 수 있습니다. Device Portal의 **[앱](../debug-test-perf/device-portal.md#apps)** 페이지를 사용하여 앱 패키지(.appx)를 업로드하고 디바이스에 설치할 수 있습니다.

## <a name="use-group-policies-or-registry-keys-to-enable-a-device"></a>그룹 정책 또는 레지스트리 키를 사용하여 디바이스를 사용하도록 설정

대부분의 개발자의 경우 디바이스를 디버깅에 사용할 수 있도록 설정 앱을 사용하려고 합니다. 자동화된 테스트와 같은 특정 시나리오에서 다른 방법으로 개발용 Windows 10 데스크톱 디바이스를 사용할 수 있습니다.

Windows 10 Home을 설치한 경우가 아니라면 gpedit.msc를 사용하여 디바이스를 사용하도록 그룹 정책을 설정할 수 있습니다. Windows 10 Home을 설치한 경우 regedit 또는 PowerShell 명령을 사용하여 장치를 사용하도록 레지스트리 키를 직접 설정해야 합니다.

**gpedit를 사용하여 장치를 사용하도록 설정**

1.  **Gpedit.msc**를 실행합니다.
2.  로컬 컴퓨터 정책 &gt; 컴퓨터 구성 &gt; 관리 템플릿 &gt; Windows 구성 요소 &gt; 앱 패키지 배포로 이동합니다.
3.  테스트용 로드를 사용하도록 설정하려면 다음을 사용하도록 정책을 편집합니다.

    -   **Allow all trusted apps to install(모든 신뢰할 수 있는 앱을 설치할 수 있음)**

    - 또는 -

    개발자 모드를 사용하도록 설정하려면 아래 두 항목을 모두 사용하도록 정책을 편집합니다.

    -   **Allow all trusted apps to install(모든 신뢰할 수 있는 앱을 설치할 수 있음)**
    -   **IDE(통합 개발 환경)에서 Windows 스토어 앱을 개발하고 설치할 수 있음**

4.  컴퓨터를 다시 부팅합니다.

**regedit를 사용하여 장치를 사용하도록 설정**

1.  **regedit**를 실행합니다.
2.  테스트용 로드를 사용하도록 설정하려면 이 DWORD 값을 1로 설정합니다.

    -   **HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock\\AllowAllTrustedApps**

    - 또는 -

    개발자 모드를 사용하도록 설정하려면 이 DWORD 값을 1로 설정합니다.

    -   **HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock\\AllowDevelopmentWithoutDevLicense**

**PowerShell을 사용하여 장치를 사용하도록 설정**

1.  관리자 권한으로 PowerShell을 실행합니다.
2.  테스트용 로드를 사용하도록 설정하려면 다음 명령을 실행합니다.

    -   **PS C:\\WINDOWS\\system32&gt; reg add "HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock" /t REG\_DWORD /f /v "AllowAllTrustedApps" /d "1"**

    - 또는 -

    개발자 모드를 사용하도록 설정하려면 다음 명령을 실행합니다.

    -   **PS C:\\WINDOWS\\system32&gt; reg add "HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock" /t REG\_DWORD /f /v "AllowDevelopmentWithoutDevLicense" /d "1"**

## <a name="upgrade-your-device-from-windows-81-to-windows-10"></a>Windows 8.1에서 Windows 10으로 디바이스 업그레이드

Windows 8.1 장치에서 앱을 만들거나 테스트용으로 로드할 때는 개발자 라이선스를 설치해야 합니다. Windows 8.1에서 Windows 10으로 장치를 업그레이드하는 경우에도 이 정보가 유지됩니다. 업그레이드된 Windows 10 장치에서 이 정보를 제거하려면 다음 명령을 실행합니다. Windows 8.1에서 Windows 10 버전 1511 이상으로 직접 업그레이드하는 경우에는 이 단계가 필요하지 않습니다.

**개발자 라이선스를 등록 취소하려면**

1.  관리자 권한으로 PowerShell을 실행합니다.
2.  다음 명령을 실행합니다. **unregister-windowsdeveloperlicense**.

이후에도 이 디바이스에서 계속 개발하려면 이 항목에 설명된 대로 디바이스를 개발용으로 설정해야 합니다. 그러지 않으면 앱을 디버그하거나 앱의 패키지를 만들려고 할 때 오류가 발생할 수 있습니다. 다음은 이러한 오류의 예입니다.

오류: DEP0700: 앱을 등록하지 못했습니다.

