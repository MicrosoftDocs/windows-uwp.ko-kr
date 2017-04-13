---
author: normesta
Description: "데스크톱 변환기 앱을 실행하여 UWP(유니버설 Windows 플랫폼) 앱으로 Windows 데스크톱 응용 프로그램(예: Win32, WPF 및 Windows Forms)을 수동으로 변환합니다."
Search.Product: eADQiWindows 10XVcnh
title: "데스크톱-UWP 브리지 Desktop App Converter"
ms.author: normesta
ms.date: 03/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 74c84eb6-4714-4e12-a658-09cb92b576e3
ms.openlocfilehash: 4daf45a4637ac846c550430cbc7518238ebd5bd8
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="desktop-to-uwp-bridge-desktop-app-converter"></a>데스크톱-UWP 브리지: Desktop App Converter

[Desktop App Converter 다운로드](https://aka.ms/converter)

DAC(Desktop App Converter)는 .NET 4.6.1 또는 Win32용으로 작성된 기존 데스크톱 앱을 UWP(유니버설 Windows 플랫폼)로 가져올 수 있는 도구입니다. 무인(자동) 모드에서 변환기를 통해 데스크톱 설치 관리자를 실행하고 개발 컴퓨터에서 Add-AppxPackage PowerShell cmdlet을 사용하여 설치할 수 있는 Windows 앱 패키지를 가져올 수 있습니다.

이제 [Windows 스토어](https://aka.ms/converter)에서 Desktop App Converter를 사용할 수 있습니다.

변환기는 변환기 다운로드의 일부로 제공된 새로운 기본 이미지를 사용하여 격리된 Windows 환경에서 데스크톱 설치 관리자를 실행합니다. 또한 데스크톱 설치 관리자가 수행한 모든 레지스트리 및 파일 시스템 I/O를 캡처하고 출력의 일부로 패키징합니다. 변환기는 패키지 ID와 방대한 WinRT API를 호출하는 기능을 포함하여 Windows 앱 패키지를 출력합니다.

## <a name="whats-new"></a>새로운 기능

DAC의 최신 버전은 v1.0.9.0입니다. 이 업데이트의 새로운 기능:

* 설치 관리자 없이 변환: 앱이 xcopy를 사용하여 설치되었거나 앱의 설치 관리자가 시스템을 변경하는 부분을 잘 알고 있다면, -Installer 매개 변수를 앱 파일의 루트로 설정하여 설치 관리자 없이 변환을 실행할 수 있습니다.
* 앱 패키지 유효성 검사: 새로운 `-Verify` 플래그를 사용하여 변환된 앱 패키지가 데스크톱 브리지와 스토어 요구 사항에 맞는지 검사할 수 있습니다.


## <a name="system-requirements"></a>시스템 요구 사항

### <a name="operating-system"></a>운영 체제

+ Windows10 1주년 업데이트(10.0.14393.0 이상) Pro 또는 Enterprise 버전입니다.

### <a name="hardware-configuration"></a>하드웨어 구성

+ 64비트(x64) 프로세서
+ 하드웨어 지원 가상화
+ SLAT(Second Level Address Translation)

### <a name="required-resources"></a>필수 리소스

+ [Windows 10용 Windows SDK(소프트웨어 개발 키트)](https://go.microsoft.com/fwlink/?linkid=821375)

## <a name="set-up-the-desktop-app-converter"></a>Desktop App Converter 설정

*(이러한 단계는 설치 관리자 없이 변환할 경우 필요하지 않습니다.)*

Desktop App Converter는 최신 Windows 10 기능을 사용합니다. Windows 10 1주년 업데이트(14393.0) 또는 이후 빌드를 사용 중인지 확인하세요.

1.    [Windows 스토어의 DesktopAppConverter](https://aka.ms/converter) 및 [빌드에 맞는 기본 이미지 .wim 파일](https://aka.ms/converterimages)을 다운로드합니다.  
2.    DesktopAppConverter를 관리자로 실행합니다. 이 작업은 시작 메뉴에서 타일을 마우스 오른쪽 단추로 클릭하고 *자세히*에서 *관리자 권한으로 실행*을 선택하거나, 작업 표시줄에서 타일을 마우스 오른쪽 단추로 클릭하여 나타나는 앱 이름을 마우스 오른쪽 단추로 짧게 클릭한 다음 *관리자 권한으로 실행*을 선택합니다.
3.    앱 콘솔 창에서 ```Set-ExecutionPolicy bypass```를 실행합니다.
4.    앱 콘솔 창에서 ```DesktopAppConverter.exe -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose```를 실행하여 변환기를 설정합니다.
5.    이전 명령을 실행할 경우 다시 부팅하라는 메시지가 표시되면 컴퓨터를 다시 시작하세요.

## <a name="run-the-desktop-app-converter"></a>Desktop App Converter 실행

+ **스토어 다운로드**: ```DesktopAppConverter.exe```를 사용하여 변환기를 실행합니다.

### <a name="usage"></a>사용

```CMD
DesktopAppConverter.exe
-Installer <String> [-InstallerArguments <String>] [-InstallerValidExitCodes <Int32>]
-Destination <String>
-PackageName <String>
-Publisher <String>
-Version <Version>
[-ExpandedBaseImage <String>]
[-AppExecutable <String>]
[-AppFileTypes <String>]
[-AppId <String>]
[-AppDisplayName <String>]
[-AppDescription <String>]
[-PackageDisplayName <String>]
[-PackagePublisherDisplayName <String>]
[-MakeAppx]
[-LogFile <String>]
[<CommonParameters>]  
```

### <a name="examples"></a>예

다음 예제에서는 *MyApp*이라는 이름의 데스크톱 앱을 Windows 앱 패키지인 *MyPublisher*로 변환하는 방법을 보여 줍니다.

#### <a name="no-installer-conversion"></a>설치 관리자 없이 변환

설치 관리자 없이 변환할 경우 `-Installer` 매개 변수는 앱 파일의 루트 디렉터리 가리키며 `-AppExecutable` 매개 변수가 필요합니다.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyApp\ -AppExecutable MyApp.exe -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1 -MakeAppx -Sign -Verbose
```

#### <a name="installer-based-conversion"></a>설치 관리자 기반 변환

설치 관리자 기반 변환의 경우 `-Installer`는 앱의 설치 관리자를 가리킵니다.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.exe -InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1 -MakeAppx -Sign -Verbose
```

## <a name="deploy-your-converted-windows-app-package"></a>변환된 Windows 앱 패키지 배포

PowerShell에서 [Add-AppxPackage](https://technet.microsoft.com/library/hh856048.aspx) cmdlet을 사용하여 서명된 앱 패키지(.appx)를 사용자 계정에 배포합니다.

Desktop App Converter(v0.1.24)에서 ```-Sign``` 플래그를 사용하여 변환된 앱에 자동 서명할 수 있습니다. 또는 AppX 패키지에 자체 서명하는 방법은 [변환된 데스크톱 앱에 서명](desktop-to-uwp-signing.md)을 참조하세요.

Add-appxpackage PowerShell cmdlet의 ```-Register``` 매개 변수를 활용하여 개발 프로세스 동안 패키지에 포함되지 않은 파일의 폴더에서 설치할 수도 있습니다.

변환된 앱 배포 및 디버깅에 대한 자세한 내용은 [변환된 UWP 앱 배포 및 디버그](desktop-to-uwp-deploy-and-debug.md)를 참조하세요.

## <a name="sign-your-windows-app-package"></a>Windows 앱 패키지 서명

Add-AppxPackage cmdlet을 사용하려면 배포하는 Windows 앱 패키지(.appx)에 서명해야 합니다. Windows 앱 패키지에 서명하려면 변환기 명령줄의 일부로 ```-Sign``` 플래그를 사용하거나 Microsoft Windows10 SDK에서 제공하는 SignTool.exe를 사용합니다.

Windows 앱 패키지에 서명하는 방법은 [변환된 데스크톱 앱에 서명](desktop-to-uwp-signing.md)을 참조하세요.

참고: 자동 생성 인증서를 사용하여 패키지에 서명하려는 경우에는 기본 암호 "123456"을 사용해야 합니다.

## <a name="modify-vfs-folder-and-registry-hive-optional"></a>VFS 폴더 및 레지스트리 하이브 수정(선택 사항)

Desktop App Converter는 컨테이너에서 파일과 시스템 노이즈를 걸러낼 때 매우 보수적으로 접근합니다.  필수는 아니지만 변환 후에는 다음과 같은 작업을 수행하는 것이 좋습니다.

1. VFS 폴더를 검토하고 설치 관리자에 필요하지 않은 파일이 있으면 삭제합니다.
2. Reg.dat의 내용을 검토하고 앱에서 설치하지 않았거나 앱에 필요하지 않은 키가 있으면 삭제합니다.

변환된 앱에 변경 작업을 수행한 경우(위의 작업 포함) 컨버터를 다시 실행할 필요 없이 DAC에서 앱을 위해 생성한 appxmanifest.xml 파일과 MakeAppx 도구를 사용하여 앱을 수동으로 다시 패키징할 수 있습니다. 도움이 필요하면 [데스크톱 브리지를 사용하여 수동으로 앱을 UWP로 변환](desktop-to-uwp-manual-conversion.md)을 참조하세요.

## <a name="caveats"></a>주의 사항

1. 호스트 컴퓨터의 Windows 10 빌드는 Desktop App Converter 다운로드의 일부로 받은 기본 이미지와 일치해야 합니다.  
2. 변환기는 모든 디렉터리 콘텐츠를 격리된 Windows 환경으로 복사하기 때문에 데스크톱 설치 관리자는 독립된 디렉터리에 있습니다.  
3. 현재 Desktop App Converter는 64비트 운영 체제에서만 변환 프로세스의 실행을 지원합니다. 변환된 Windows 앱 패키지는 64비트(x64) OS에만 배포할 수 있습니다.  
4. Desktop App Converter를 무인 모드에서 실행하려면 데스크톱 설치 관리자가 필요합니다. *-InstallerArguments* 매개 변수를 사용하여 설치 관리자에 대한 자동 플래그를 변환기에 제공해야 합니다.
5. 공용 SxS Fusion 어셈블리 게시는 작동하지 않습니다. 설치하는 동안 응용 프로그램이 다른 프로세스에서 액세스할 수 있는 공개 side-by-side Fusion 어셈블리를 게시할 수 있습니다. 프로세스 활성화 컨텍스트 생성 동안 이러한 어셈블리는 CSRSS.exe라는 시스템 프로세스에 의해 검색됩니다. 변환 프로세스에 대해 이 작업이 수행되면 이러한 어셈블리의 활성화 컨텍스트 생성 및 모듈 로드가 실패합니다. ComCtl과 같은 받은 편지함 어셈블리는 OS와 함께 제공되므로 해당 종속성을 유지하는 것이 안전합니다. SxS Fusion 어셈블리는 다음 위치에 등록됩니다.
  + 레지스트리: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + 파일 시스템: %windir%\\SideBySide

## <a name="known-issues"></a>알려진 문제

* 일부 OS 빌드에서 발생하는 다음 오류는 현재 조사 중입니다.

    * ```E_CREATTING_ISOLATED_ENV_FAILED```
    * ```E_STARTING_ISOLATED_ENV_FAILED```

    이러한 오류를 발견할 경우 [다운로드 센터](https://aka.ms/converterimages)의 유효한 기본 이미지를 사용하고 있는지 확인하세요. 유효한 .wim을 사용 중인 경우 converter@microsoft.com으로 로그를 보내 주시면 조사에 많은 도움이 될 것입니다.

* 이전에 Desktop App Converter를 설치한 개발자 컴퓨터에서 Windows 참가자 플라이트를 수신하는 경우 새로운 기본 이미지를 설정할 때 `New-ContainerNetwork: The object already exists` 오류가 발생할 수 있습니다. 이 문제를 해결하려면 관리자 권한 명령 프롬프트에서 `Netsh int ipv4 reset` 명령을 실행한 다음 컴퓨터를 다시 부팅합니다.

* 주 실행 파일 또는 종속성이 "Program Files" 또는 "Windows\System32" 아래에 배치된 경우 "AnyCPU" 빌드 옵션을 사용하여 컴파일된 .NET 앱이 설치되지 않습니다. 이 문제를 해결하려면 아키텍처 관련 데스크톱 설치 관리자(32비트 또는 64비트)를 사용하여 Windows 앱 패키지를 성공적으로 생성하세요.

## <a name="telemetry-from-desktop-app-converter"></a>Desktop App Converter의 원격 분석

Desktop App Converter가 사용자 및 사용자의 소프트웨어 사용에 대한 정보를 수집한 후 Microsoft로 보낼 수 있습니다. 제품 설명서 및 [Microsoft 개인 정보 취급 방침](http://go.microsoft.com/fwlink/?LinkId=521839)에서Microsoft의 데이터 수집 및 사용에 대해 알아볼 수 있습니다. Microsoft 개인 정보 취급 방침의 모든 규정을 준수한다는 데 동의합니다.

기본적으로 Desktop App Converter에 대한 원격 분석은 사용되도록 설정됩니다. 원격 분석을 원하는 설정으로 구성하려면 다음 레지스트리 키를 추가합니다.  

```cmd
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DesktopAppConverter
```
+ 1로 설정된 DWORD를 사용하여 *DisableTelemetry* 값을 추가 또는 편집합니다.
+ 원격 분석을 사용하려면 이 키를 제거하거나 값을 0으로 설정합니다.

## <a name="desktop-app-converter-usage"></a>Desktop App Converter 사용법

Desktop App Converter에 대한 매개 변수 목록은 다음과 같습니다. 또한 다음을 실행하여 이 목록을 볼 수 있습니다.   

```CMD
Get-Help DesktopAppConverter.exe -detailed
```

### <a name="setup-parameters"></a>설정 매개 변수  

|매개 변수|설명|
|---------|-----------|
|```-Setup [<SwitchParameter>]``` | 설치 모드에서 DesktopAppConverter를 실행합니다. 설치 모드는 제공된 기본 이미지 확장을 지원합니다.|
|```-BaseImage <String>``` | 확장되지 않은 기본 이미지에 대한 전체 경로. 이 매개 변수는 -Setup을 지정한 경우에 필요합니다.|
|```-LogFile <String>``` [옵션] | 로그 파일을 지정합니다. 생략하면 로그 파일의 임시 위치가 생성됩니다.|
|```-NatSubnetPrefix <String>``` [옵션] | Nat 인스턴스에 사용할 접두사 값입니다. 일반적으로 사용자는 호스트 컴퓨터가 변환기의 NetNat과 동일한 서브넷 범위에 연결되는 경우에만 이 값을 변경하려고 합니다. **Get NetNat** cmdlet을 사용하여 현재 변환기 NetNat 구성을 쿼리할 수 있습니다. |
|```-NoRestart [<SwitchParameter>]``` | 설치 프로그램을 실행할 때 다시 부팅하라는 메시지를 표시하지 않습니다(컨테이너 기능을 사용하도록 설정하려면 다시 부팅해야 함). |

### <a name="conversion-parameters"></a>변환 매개 변수  

|매개 변수|설명|
|---------|-----------|
|```-AppInstallPath <String> [optional]``` | 응용 프로그램이 설치된 경우 설치 파일에 대한 응용 프로그램 루트 폴더의 전체 경로입니다(예: "C:\Program Files (x86)\MyApp").|
|```-Destination <String>``` | 원하는 변환기 appx 출력 대상으로, DesktopAppConverter는 이 위치가 없으면 만들 수 있습니다.|
|```-Installer <String>``` | 응용 프로그램의 설치 관리자 경로로, 무인/자동으로 실행할 수 있어야 합니다. 설치 관리자 없이 변환할 경우 이는 앱 파일의 루트 디렉터리에 대한 경로입니다. |
|```-InstallerArguments <String>``` [옵션] | 강제로 설치 관리자를 무인/자동으로 실행할 수 있도록 하기 위한 쉼표로 구분된 인수 목록 또는 인수 문자열입니다. 설치 관리자가 msi인 경우 이 매개 변수는 선택적입니다. 설치 관리자에서 로그를 가져오려면 여기에 설치 관리자에 대한 로깅 인수를 제공하고, 변환기가 적절한 경로로 대체하는 토큰에 해당하는 경로 ```<log_folder>```를 사용합니다. <br><br>**참고: 무인/자동 플래그와 로그 인수는 설치 관리자 기술마다 다릅니다.** <br><br>이 매개 변수에 대한 사용 예제: ```-InstallerArguments "/silent /log <log_folder>\install.log"``` 로그 파일을 생성하지 않는 또 다른 예제는 다음과 같습니다. ```-InstallerArguments "/quiet", "/norestart"``` 변환기가 로그를 캡처한 후 최종 로그 폴더에 추가하게 하려면 토큰 경로 ```<log_folder>```로 모든 로그를 보내야 합니다.|
|```-InstallerValidExitCodes <Int32>``` [옵션] | 설치 관리자가 성공적으로 실행되었음을 나타내는 쉼표로 구분된 종료 코드 목록(예: 0, 1234, 5678).  기본적으로 msi가 아닌 경우는 0이고 msi인 경우는 0, 1641, 3010입니다.|

### <a name="windows-app-package-identity-parameters"></a>Windows 앱 패키지 ID 매개 변수  

|매개 변수|설명|
|---------|-----------|
|```-PackageName <String>``` | 유니버설 Windows 앱 패키지의 이름
|```-Publisher <String>``` | 유니버설 Windows 앱 패키지의 게시자
|```-Version <Version>``` | 유니버설 Windows 앱 패키지의 버전 번호

### <a name="optional-windows-app-package-manifest-parameters"></a>선택적인 Windows 앱 패키지 매니페스트 매개 변수  

|매개 변수|설명|
|---------|-----------|
|```-AppExecutable <String> [optional]``` [옵션] | 응용 프로그램의 주 실행 파일(예: "MyApp.exe")의 이름입니다. 이 매개 변수는 설치 관리자 없이 변환할 경우 필요합니다. |
|```-AppFileTypes <String>``` [옵션] | 응용 프로그램과 연결될 쉼표로 구분된 파일 형식 목록(예: 큰따옴표를 제외한 ".txt, .doc")|
|```-AppId <String>``` [옵션] | 응용 프로그램 ID를 Windows 앱 패키지 매니페스트로 설정하기 위한 값을 지정합니다. 값을 지정하지 않으면 *PackageName*에 대해 제공한 값으로 설정됩니다.|
|```-AppDisplayName <String>``` [옵션] | 응용 프로그램 표시 이름을 Windows 앱 패키지 매니페스트로 설정하기 위한 값을 지정합니다. 값을 지정하지 않으면 *PackageName*에 대해 제공한 값으로 설정됩니다. |
|```-AppDescription <String>``` [옵션] | 응용 프로그램 설명을 Windows 앱 패키지 매니페스트로 설정하기 위한 값을 지정합니다. 값을 지정하지 않으면 *PackageName*에 대해 제공한 값으로 설정됩니다.|
|```-PackageDisplayName <String>``` [옵션] | 패키지 표시 이름을 Windows 앱 패키지 매니페스트로 설정하기 위한 값을 지정합니다. 값을 지정하지 않으면 *PackageName*에 대해 제공한 값으로 설정됩니다. |
|```-PackagePublisherDisplayName <String>``` [옵션] | 패키지 게시자 표시 이름을 Windows 앱 패키지 매니페스트로 설정하기 위한 값을 지정합니다. 값을 지정하지 않으면 *Publisher*에 대해 제공한 값으로 설정됩니다. |

### <a name="other-conversion-parameters"></a>기타 변환 매개 변수  

|매개 변수|설명|
|---------|-----------|
|```-ExpandedBaseImage <String>``` [옵션] | 이미 확장된 기본 이미지에 대한 전체 경로.|
|```-MakeAppx [<SwitchParameter>]``` [옵션] | 스위치(있는 경우)는 출력에 대해 MakeAppx를 호출하도록 이 스크립트에 지시합니다. |
|```-LogFile <String>``` [옵션] | 로그 파일을 지정합니다. 생략하면 로그 파일의 임시 위치가 생성됩니다. |
| ```-Sign [<SwitchParameter>] [optional]``` | 이 스크립트를 통해 출력 Windows 앱 패키지에 서명합니다. 이 스위치는 ```-MakeAppx``` 스위치와 함께 있어야 합니다.
|```<Common parameters>``` | 이 cmdlet은 *Verbose*, *Debug*, *ErrorAction*, *ErrorVariable*, *WarningAction*, *WarningVariable*, *OutBuffer*, *PipelineVariable*, *OutVariable* 등의 일반 매개 변수를 지원합니다. 자세한 내용은 [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216)를 참조하세요. |
| ```-Verify [<SwitchParameter>] [optional]``` | 스위치가 있을 경우, DAC를 통해 변환된 앱 패키지가 데스크톱 브리지 및 Windows 스토어의 요구 사항에 맞는지 확인합니다. 결과는 "VerifyReport.xml"이라는 유효성 검사 보고서로 출력되며, 브라우저에서 가장 잘 보입니다. 이 스위치는 `-MakeAppx` 스위치와 함께 있어야 합니다.

### <a name="cleanup-parameters"></a>정리 매개 변수

|매개 변수|설명|
|---------|-----------|
|```Cleanup [<Option>]``` | DesktopAppConverter 아티팩트에 대한 정리를 실행합니다. 정리 모드에는 3가지 유효한 옵션이 있습니다. |
|```Cleanup All``` | 확장된 기본 이미지를 모두 삭제하고, 임시 변환기 파일을 제거하고, 컨테이너 네트워크를 제거하고, 선택적 Windows 기능인 컨테이너를 사용하지 않도록 설정합니다. |
|```Cleanup WorkDirectory``` | 모든 임시 변환기 파일을 제거합니다. |
|```Cleanup ExpandedImage``` | 호스트 컴퓨터에 설치된 확장된 기본 이미지를 모두 삭제합니다. |

### <a name="package-architecture"></a>패키지 아키텍처

Desktop App Converter는 이제 x86 및 amd64 컴퓨터에서 설치하고 실행할 수 있는 x86 및 x64 앱 패키지를 만드는 기능을 지원합니다. 하지만 성공적인 변환을 수행하려면Desktop App Converter가 AMD64 컴퓨터에서 실행되어야 합니다.

|매개 변수|설명|
|---------|-----------|
|```-PackageArch <String>``` | 지정된 아키텍처를 사용하여 패키지를 생성합니다. 유효한 옵션은 'x86' 또는 'x64'입니다(예: -PackageArch x86). 이 매개 변수는 선택 사항입니다. 지정하지 않으면 DesktopAppConverter는 패키지 아키텍처를 자동으로 검색하려고 합니다. 자동 검색에 실패하면 기본적으로 x64 패키지로 설정합니다.

### <a name="running-the-peheadercertfixtool"></a>PEHeaderCertFixTool 실행

변환 프로세스 동안 DesktopAppConverter는 모든 손상된 PE 헤더를 수정하기 위해 자동으로 PEHeaderCertFixTool을 실행합니다. 하지만 UWP Windows 앱 패키지, 느슨한 파일 또는 특정 이진에서 PEHeaderCertFixTool을 실행할 수도 있습니다. 사용 예제:

```CMD
PEHeaderCertFixTool.exe <binary file>|<.appx package>|<folder> [/c] [/v]
 /c   -- check for corrupted certificate but do not fix (optional)
 /v   -- verbose (optional)
example1: PEHeaderCertFixTool app.exe
example2: PEHeaderCertFixTool c:\package.appx /c
example3: PEHeaderCertFixTool c:\myapp /c /v
```

## <a name="language-support"></a>언어 지원

Desktop App Converter는 유니코드를 지원하지 않으므로 중국어 문자나 ASCII가 아닌 문자는 도구에 사용할 수 없습니다.

## <a name="see-also"></a>참고 항목

+ [Desktop App Converter를 사용하여 데스크톱 앱을 UWP로 가져오기](https://channel9.msdn.com/events/Build/2016/P504)
+ [Project Centennial: 기존 데스크톱 응용 프로그램을 유니버설 Windows 플랫폼으로 가져오기](https://channel9.msdn.com/events/Build/2016/B829)  
