---
description&#58; 작성자&#58; martinekuan ms.assetid&#58; 54973C62-9669-4988-934E-9273FB0425FD 제목&#58; 개발을 위해 디바이스 사용. Windows 10 디바이스 개발에 대한 다양한 접근 방식이 있습니다.
키워드&#58; 키워드 시작&#58; 개발자 라이선스 키워드&#58; Visual Studio, 개발자 라이선스 키워드&#58; 디바이스 사용
---
# 디바이스를 개발에 사용하도록 설정

Windows 10 장치 개발에 대한 다양한 접근 방식이 있습니다. 앱을 개발, 설치 또는 테스트하는 데 사용하려는 각 장치에 대해 개발자 라이선스가 더 이상 필요하지 않습니다. 장치의 설정에서 이러한 작업에 대해 한 번만 장치를 사용하도록 설정합니다. 정말 간단하죠? 개발자 라이선스를 더 이상 30일 또는 90일 마다 갱신하지 않아도 됩니다.

Microsoft Visual Studio 2013 또는 Microsoft Visual Studio 2015에서 앱을 개발 또는 테스트하는 데 Windows 8.1 장치를 계속 사용할 경우에는 여전히 [개발자 라이선스를 얻거나](https://msdn.microsoft.com/library/windows/apps/Hh974578)[Windows Phone을 등록](https://msdn.microsoft.com/library/windows/apps/Dn614128)해야 합니다.

## 개발자 기능 사용

### Microsoft Visual Studio를 사용하여 앱 개발

Windows 10 디바이스에서 Microsoft Visual Studio를 사용하고 Windows 8.1 또는 Windows 10 앱용 솔루션을 여는 경우 이 대화 상자를 통해 디바이스를 사용하도록 설정하라는 메시지가 표시됩니다. 디자이너를 사용하고 앱을 디버그하는 데 디바이스를 사용할 수 있도록 설정해야 합니다.

![Visual Studio에서 표시되는 개발자 모드 대화 상자를 사용하도록 설정](images/latestenabledialog.png)

이 대화 상자가 나타나면 **개발자용 설정**을 클릭하여 아래 표시된 대로 **업데이트 및 보안** 페이지로 바로 이동합니다. 또는 **확인**을 클릭하고 아래 단계에 따라 Windows 10 장치를 개발에 사용하도록 설정합니다.

### Windows 10 장치를 사용하도록 설정

Windows 10의 경우 장치에 사용하도록 설정할 개발자 기능을 선택합니다. 여기에는 Windows 10 데스크톱, 태블릿 및 휴대폰과 같은 장치가 포함됩니다. 장치를 개발 또는 단순히 테스트용 로드에 사용할 수 있습니다.

-   *테스트용 로드*는 Windows 스토어에서 인증되지 않은 앱을 설치한 후 실행 또는 테스트하는 것입니다. 예를 들어 회사에서만 사용하는 내부용 앱입니다.
-   *개발자 모드*를 사용하면 앱을 테스트용으로 로드할 수 있을 뿐만 아니라 Visual Studio에서 디버그 모드로 앱을 실행할 수 있습니다.

**참고** 앱을 테스트용으로 로드하는 경우에도 여전히 신뢰할 수 있는 소스의 앱만을 설치해야 합니다. Windows 스토어에서 인증되지 않은 테스트용 로드 앱을 설치할 경우 이러한 앱을 테스트용으로 로드하는 데 필요한 모든 권한을 확보했으며 이러한 앱을 설치하고 실행하여 발생할 수 있는 피해에 대해 전적으로 책임을 진다는 점에 동의하는 것입니다. 이 [개인 정보 취급 방침](http://go.microsoft.com/fwlink/?LinkId=521839)의 Windows &gt; Windows 스토어 섹션을 참조하세요.

**개발자 기능을 사용하려면**

1.  사용하도록 설정하려는 장치에서 **설정**으로 이동합니다. **업데이트 및 보안**을 선택한 다음 **개발자용**을 선택합니다.
2.  필요한 액세스 수준을 선택합니다. 옵션에 대한 자세한 내용은 [선택해야 하는 설정: 테스트용으로 앱 로드 또는 개발자 모드](#WhichSettings)를 참조하세요.
3.  선택한 설정에 대한 고지 사항을 읽은 다음 **예**를 클릭하여 변경 내용을 적용합니다.

다음은 데스크톱 디바이스 패밀리의 설정 페이지입니다.

![설정으로 이동하고 업데이트 및 보안을 선택한 후 개발자용을 선택하여 옵션 확인](images/devmode-pc-options.png)

다음은 모바일 디바이스 패밀리의 설정 페이지입니다.

![휴대폰의 설정에서 업데이트 및 보안 선택](images/devmode-mob.png)

### 선택해야 하는 설정: 테스트용으로 앱 로드 또는 개발자 모드

기본적으로 Windows 스토어의 UWP(유니버설 Windows 플랫폼) 앱만 설치할 수 있습니다. 개발자 기능을 사용하도록 이러한 설정을 변경하면 장치의 보안 수준을 변경할 수 있습니다. 확인되지 않은 원본에서 앱을 설치하지 않아야 합니다.

**앱 테스트용 로드**

테스트용 로드 앱 설정은 일반적으로 Windows 스토어를 거치지 않고 관리되는 장치에 사용자 지정 앱을 설치해야 하는 회사 또는 학교에서 사용합니다. 이 경우 이전에 전화 설정 페이지 이미지에 표시된 대로 조직에서 *Windows 스토어 앱* 설정을 사용하지 않는 정책을 적용하는 경우가 일반적입니다. 또한 조직에서는 필요한 인증서 및 테스트용 로드 앱의 설치 위치를 제공합니다. 자세한 내용은 TechNet 문서 [Windows 10에서 앱을 테스트용으로 로드](https://technet.microsoft.com/library/mt269549.aspx) 및 [Microsoft Intune에서 앱 배포 시작](https://technet.microsoft.com/library/dn646955.aspx)을 참조하세요.

디바이스 패밀리 관련 정보

-   데스크톱 디바이스 패밀리에서: 앱 패키지(.appx) 및 패키지("Add-AppDevPackage.ps1")로 만든 Windows PowerShell 스크립트를 실행하여 앱을 실행하는 데 필요한 모든 인증서를 설치할 수 있습니다.

-   모바일 디바이스 패밀리에서: 필요한 인증서가 이미 설치되어 있는 경우 파일을 탭하여 메일로 전송되거나 SD 카드에 있는 .appx를 설치할 수 있습니다.

**앱 테스트용 로드**를 사용하면 신뢰할 수 있는 인증서가 없는 장치에 앱을 설치할 수 없으므로 개발자 모드보다 안전합니다.

**개발자 모드**

테스트용 로드 외에도 개발자 모드 설정을 사용하면 디버깅 및 추가 배포 옵션을 사용하도록 설정할 수 있습니다. 이는 개발자 라이선스에 대한 Windows 8.1 요구 사항을 대체합니다.

디바이스 패밀리 관련 정보

-   데스크톱 디바이스 패밀리에서:

    개발자 모드를 사용하여 Visual Studio에서 앱을 개발하고 디버그합니다. 앞에서 설명한대로 개발자 모드를 사용하도록 설정하지 않으면 Visual Studio에서 메시지가 표시됩니다.

-   모바일 디바이스 패밀리에서:

    개발자 모드를 사용하여 Visual Studio에서 앱을 배포하고 장치에서 디버그합니다.

    파일을 탭하여 메일로 전송되거나 SD 카드에 있는 .appx를 설치할 수 있습니다. 확인되지 않은 원본에서 앱을 설치하지 마세요.

**팁**  
여러 도구를 사용하여 Windows 10 PC에서 Windows 10 Mobile 디바이스로 앱을 배포할 수 있습니다. 두 장치는 유선 또는 무선으로 연결하여 동일한 네트워크 서브넷에 연결되거나 USB로 연결되어야 합니다. 나열된 방법은 앱 패키지(.appx)만 설치하고 인증서는 설치하지 않습니다.

-   Windows 10 응용 프로그램 배포(WinAppDeployCmd) 도구를 사용합니다. 자세한 내용은 [the WinAppDeployCmd 도구](http://msdn.microsoft.com/library/windows/apps/mt203806.aspx)를 참조하세요.
-   Windows 10 버전 1511부터 [장치 포털](#device_portal)을 사용하여 브라우저에서 Windows 10 버전 1511 이상을 실행하는 모바일 장치로 배포할 수 있습니다. 디바이스 포털(&lt;IP&gt;/appmanager.md)의 **앱** 페이지를 사용하여 앱 패키지(.appx)를 업로드하고 디바이스에 설치할 수 있습니다.

 

### 그룹 정책 또는 레지스트리 키 설정

또한 그룹 정책이나 레지스트리 키를 대체 방법으로 사용하여 Windows 10 데스크톱 장치를 개발에 사용하도록 설정할 수도 있습니다.

**데스크톱 디바이스 패밀리에서**

Windows 10 Home을 설치한 경우가 아니라면 gpedit.msc를 사용하여 장치를 사용하도록 그룹 정책을 설정합니다. Windows 10 Home을 설치한 경우 regedit 또는 PowerShell 명령을 사용하여 장치를 사용하도록 레지스트리 키를 직접 설정해야 합니다.

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

## 개발자 모드 기능

각 디바이스 패밀리의 경우 추가적인 개발자 기능을 사용할 수 있습니다. 이러한 기능은 디바이스에서 **개발자 모드**를 사용하는 경우에만 사용할 수 있고 OS 버전에 따라 달라질 수 있습니다.

이 이미지는 Windows 10 버전 1511에서 모바일 디바이스 패밀리에 대한 개발자 기능을 보여 줍니다.

![모바일 디바이스에 대한 개발자 모드 옵션](images/devmode-mob-options.png)

### <span id="device-discovery-and-pairing"></span>디바이스 검색 및 디바이스 포털

디바이스 검색 및 디바이스 포털에 대한 자세한 정보는 [Windows Device Portal 개요](../debug-test-perf/device-portal.md)를 참조하세요.

디바이스별 설치 지침은 다음을 참조하세요.
- [HoloLens용 디바이스 포털](https://dev.windows.com/holographic/using_the_windows_device_portal)
- [IoT용 디바이스 포털](http://ms-iot.github.io/content/en-US/win10/tools/DevicePortal.htm)
- [모바일용 디바이스 포털](../debug-test-perf/device-portal-mobile.md)
- [Xbox용 디바이스 포털](../debug-test-perf/device-portal-xbox.md)

### 오류 보고

이 값을 설정하여 휴대폰에 저장되는 크래시 덤프 수를 지정할 수 있습니다.

휴대폰에서 크래시 덤프를 수집하면 크래시가 발생한 직후 중요한 크래시 정보에 즉시 액세스할 수 있습니다. 덤프는 개발자가 서명한 앱에 대해서만 수집됩니다. Documents\\Debug 폴더의 휴대폰 저장소에서 덤프를 찾을 수 있습니다. 덤프 파일에 대한 자세한 내용은 [덤프 파일 사용](https://msdn.microsoft.com/library/d5zhxt22.aspx)을 참조하세요.

## Windows 8.1에서 Windows 10으로 장치 업그레이드

Windows 8.1 장치에서 앱을 만들거나 테스트용으로 로드할 때는 개발자 라이선스를 설치해야 합니다. Windows 8.1에서 Windows 10으로 장치를 업그레이드하는 경우에도 이 정보가 유지됩니다. 업그레이드된 Windows 10 장치에서 이 정보를 제거하려면 다음 명령을 실행합니다. Windows 8.1에서 Windows 10 버전 1511 이상으로 직접 업그레이드하는 경우에는 이 단계가 필요하지 않습니다.

**개발자 라이선스를 등록 취소하려면**

1.  관리자 권한으로 PowerShell을 실행합니다.
2.  다음 명령을 실행합니다. **unregister-windowsdeveloperlicense**.

이후에도 이 디바이스에서 계속 개발하려면 이 항목에 설명된 대로 디바이스를 개발용으로 설정해야 합니다. 그러지 않으면 앱을 디버그하거나 앱의 패키지를 만들려고 할 때 오류가 발생할 수 있습니다. 다음은 이러한 오류의 예입니다.

오류: DEP0700: 앱을 등록하지 못했습니다.




<!--HONumber=May16_HO2-->


