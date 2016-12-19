---
author: awkoren
Description: "데스크톱 변환기 앱을 실행하여 UWP(유니버설 Windows 플랫폼) 앱으로 Windows 데스크톱 응용 프로그램(예: Win32, WPF 및 Windows Forms)을 수동으로 변환합니다."
Search.Product: eADQiWindows 10XVcnh
title: Desktop App Converter
translationtype: Human Translation
ms.sourcegitcommit: a5ac8acdbb7480bb776cef6d1dffa303dab5a9e1
ms.openlocfilehash: 4095cfbe96f239afb5f3173e0a7f84e01d63452c

---

# <a name="desktop-app-converter"></a>Desktop App Converter

[Desktop App Converter 다운로드](https://aka.ms/converter)

DAC(Desktop App Converter)는 .NET 4.6.1 또는 Win32용으로 작성된 기존 데스크톱 앱을 UWP(유니버설 Windows 플랫폼)로 가져올 수 있는 도구입니다. 무인(자동) 모드에서 변환기를 통해 데스크톱 설치 관리자를 실행하고 개발 컴퓨터에서 Add-AppxPackage PowerShell cmdlet을 사용하여 설치할 수 있는 AppX 패키지를 가져올 수 있습니다.

이제 [Windows 스토어](https://aka.ms/converter)에서 Desktop App Converter를 사용할 수 있습니다.

변환기는 변환기 다운로드의 일부로 제공된 새로운 기본 이미지를 사용하여 격리된 Windows 환경에서 데스크톱 설치 관리자를 실행합니다. 또한 데스크톱 설치 관리자가 수행한 모든 레지스트리 및 파일 시스템 I/O를 캡처하고 출력의 일부로 패키징합니다. 변환기는 패키지 ID와 방대한 WinRT API를 호출하는 기능을 포함하여 AppX를 출력합니다.

## <a name="whats-new"></a>새로운 기능

이 섹션에서는 Desktop App Converter 버전 간의 변경 내용을 대략적으로 설명합니다. 

### <a name="1122016-v101"></a>11/2/2016(v1.0.1)

* 매니페스트 스키마 유효성 검사가 향상되었습니다. 
* 오류 메시지가 향상되었습니다. 
* 지원되는 Windows 버전의 유효성 검사가 추가되었습니다. 
* 레지스트리 필터 테스트의 버그가 수정되었습니다.

### <a name="9142016-v10"></a>2016/9/14(v1.0)

* Desktop App Converter는 이제 [Windows 스토어](https://aka.ms/converter)에서 다운로드할 수 있습니다. 
* [다운로드 센터](https://aka.ms/converterimages)에서 DAC에 사용할 최신 Windows 10 기본 이미지(.wim)를 가져옵니다.
* 이제 스토어 앱에서 새 진입점 *DesktopAppConverter.exe <arguments>*를 사용하여 관리자 권한 명령 프롬프트 또는 PowerShell 창의 어디에서든 변환기를 실행할 수 있습니다.  


### <a name="922016-v0125"></a>2016/9/2(v0.1.25)

* 최신 dotnet-computervirtualization NuGet 패키지가 통합되었습니다.
* 새로 도입된 common.dll에 대한 종속성이 추가되었습니다.
* 여러 버그가 수정되었습니다.

### <a name="842016-v0124"></a>2016/8/4(v0.1.24)

* 테스트 목적으로 DAC에서 생성한 변환된 앱을 자동 서명하는 지원이 추가되었습니다. 한 번 시도해 보려면 ```–Sign``` 플래그를 확인하세요. 
* 패키지에 포함된 AppX 내에서 가상 레지스트리 하이브의 COM 등록이 지원되지 않을 경우 경고가 추가되었습니다.  
* VC++ 라이브러리에 대한 앱 종속성을 자동 검색한 다음 AppX 매니페스트 종속성으로 변환하는 지원이 추가되었습니다. VC++ 런타임을 사용하여 앱을 테스트용으로 로드하여 테스트하려면 [Using Visual C++ Runtime in a Centennial project](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project)(Centennial 프로젝트에서 Visual C++ 런타임 사용) 블로그 게시물에 설명된 대로 VCLib 프레임워크 패키지를 다운로드해야 합니다. 컴퓨터의 ```Program Files (x86)\Microsoft SDKs\Windows Kits\10\ExtensionSDKs\Microsoft.VCLibs.Desktop``` 폴더 아래에서 패키지를 찾아 사용하는 버전(예: 11.0, 12.0, 14.0)으로 이동하고 적절한 아키텍처 패키지(x64, x86)를 두 번 클릭하여 설치합니다.
* Windows 10 1주년 업데이트(10.0.14393.0)에 맞게 매니페스트 스키마가 업데이트되었습니다. 
* 여러 버그가 수정되고 출력 레이아웃이 향상되었습니다. 

### <a name="772016-v0122"></a>2016/7/7(v0.1.22)

* 데스크톱 응용 프로그램에서 자동으로 셸 확장을 검색하고 UWP 패키지의 AppXManifest에서 해당 셸 확장을 선언하기 위한 지원이 추가되었습니다. 데스크톱 확장에 대한 자세한 내용은 [**변환된 데스크톱 앱 확장**](desktop-to-uwp-extensions.md) 항목을 참조하세요. 
* 대규모 앱 집합에 대한 AppExecutable 검색이 개선되었습니다. 

### <a name="6162016-v0120"></a>2016/6/16(v0.1.20)

* 최신 Windows 10 Insider Preview 빌드에서 성공적인 변환을 방해하는 문제가 해결되었습니다. 
* ```–CreateX86Package```가 ```–PackageArch```로 대체되어 생성된 패키지에 대한 아키텍처를 지정할 수 있습니다. 

### <a name="682016"></a>6/8/2016

* 변환기를 실행하는 AMD64 호스트 컴퓨터에 x86 appx 패키지를 생성하기 위한 지원이 추가되었습니다.
* 이전에 확장된 기본 이미지를 제거하여 디스크 공간 사용이 감소했습니다.
* 임시 파일과 불필요한 기본 이미지를 정리하기 위한 지원이 추가되었습니다.
* 파일 형식 및 프로토콜 연결을 검색하기 위한 지원이 개선되었습니다.
* 큰 앱 집합에 대한 AppExecutable 속성을 검색하는 논리가 개선되었습니다.
* MSI 기반 설치 관리자에 추가 –InstallerArguments를 제공하기 위한 지원이 추가되었습니다.
* 변환 프로세스 중에 PathTooLongException 오류에 대한 버그가 수정되었습니다.

### <a name="5122016"></a>5/12/2016

- Windows Pro 버전에 대한 지원이 복원되었습니다. 
- 변환기 ```-Setup``` 플래그는 이제 Windows 컨테이너 기능을 사용하도록 설정하고 기본 이미지 확장을 처리합니다. 관리자 권한의 PowerShell 프롬프트에서 다음을 실행하여 일회용 설치를 수행합니다. ```PS C:\> .\DesktopAppConverter.ps1 -Setup -BaseImage BaseImage-12345.wim -Verbose```
- 런타임에 불필요한 파일 시스템 리디렉션을 줄이기 위해 앱 설치 경로의 자동 검색과 VFS 외부로의 응용 프로그램 루트 이동 기능이 추가되었습니다.
- 변환 프로세스의 일부로 확장된 기본 이미지에 대한 자동 검색 기능이 추가되었습니다.
- 파일 형식 연결 및 프로토콜에 대한 자동 검색 기능이 추가되었습니다.
- 시작 메뉴 바로 가기를 감지하는 논리가 개선되었습니다.
- 앱 설치 MUI 파일을 유지하는 파일 시스템 필터링 기능이 개선되었습니다.
- 매니페스트에서 최소 지원 데스크톱 버전(10.0.14342.0)이 업데이트되었습니다.

## <a name="system-requirements"></a>시스템 요구 사항

### <a name="operating-system"></a>운영 체제

+ Windows 10 1주년 업데이트(10.0.14393.0 이상) Pro 또는 Enterprise 버전입니다.

### <a name="hardware-configuration"></a>하드웨어 구성

+ 64비트(x64) 프로세서
+ 하드웨어 지원 가상화
+ SLAT(Second Level Address Translation)

### <a name="required-resources"></a>필수 리소스

+ [Windows 10용 Windows SDK(소프트웨어 개발 키트)](https://go.microsoft.com/fwlink/?linkid=821375)

## <a name="set-up-the-desktop-app-converter"></a>Desktop App Converter 설정

Desktop App Converter는 최신 Windows 10 기능을 사용합니다. Windows 10 1주년 업데이트(14393.0) 또는 이후 빌드를 사용 중인지 확인하세요.

### <a name="store-download"></a>스토어 다운로드

1.  [Windows 스토어에서 DesktopAppConverter](https://aka.ms/converter) 및 [빌드에 맞는 기본 이미지.wim 파일](https://aka.ms/converterimages)을 다운로드합니다.  
2.  DesktopAppConverter를 관리자로 실행합니다. 이 작업은 시작 메뉴에서 타일을 마우스 오른쪽 단추로 클릭하고 *자세히*에서 *관리자 권한으로 실행*을 선택하거나, 작업 표시줄에서 타일을 마우스 오른쪽 단추로 클릭하여 나타나는 앱 이름을 마우스 오른쪽 단추로 짧게 클릭한 다음 *관리자 권한으로 실행*을 선택합니다.
3.  앱 콘솔 창에서 ```CMD PS C:\> Set-ExecutionPolicy bypass```를 실행합니다.
4.  앱 콘솔 창에서 ```CMD PS C:\> DesktopAppConverter.exe -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose```를 실행하여 변환기를 설정합니다.
5.  이전 명령을 실행할 경우 다시 부팅하라는 메시지가 표시되면 컴퓨터를 다시 시작하세요.

### <a name="zip-file"></a>파일 압축 

DAC는 [다운로드 센터](https://aka.ms/converterimages)에서 계속 zip 파일로 제공되어 오프라인 시나리오를 용이하게 합니다. 그러나 이후 업데이트는 모두 스토어 버전으로만 게시됩니다.

1.  DAC zip과, [빌드에 맞는 기본 이미지.wim 파일](https://aka.ms/converterimages)을 다운로드합니다.  
2. 로컬 폴더에 DesktopAppConverter.zip의 압축을 풉니다.
3. 관리자 PowerShell 창에서 ```CMD PS C:\> Set-ExecutionPolicy bypass```를 수행합니다.
4. 관리자 PowerShell 창에서 ```CMD PS C:\> .\DesktopAppConverter.ps1 -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose```를 실행하여 변환기를 설정합니다.
5. 이전 명령을 실행할 경우 다시 부팅하라는 메시지가 표시되면 컴퓨터를 다시 시작하세요.

## <a name="run-the-desktop-app-converter"></a>Desktop App Converter 실행

+ **스토어 다운로드**: ```DesktopAppConverter.exe```를 사용하여 변환기를 실행합니다.
+ **Zip 파일**: ```DesktopAppConverter.ps1```을 사용하여 변환기를 실행합니다. 

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

### <a name="example"></a>예제

다음 예제에서는 *&lt;publisher_name&gt;*에 의해 데스크톱 앱 *MyApp*을 UWP 패키지(AppX)로 변환하는 방법을 보여 줍니다.

```CMD
DesktopAppConverter.exe -Installer C:\Installer\MyApp.exe 
-InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" 
-Publisher "CN=<publisher_name>" -Version 0.0.0.1 -MakeAppx -Verbose
```

## <a name="deploy-your-converted-appx"></a>변환된 AppX 배포

PowerShell에서 [Add-AppxPackage](https://technet.microsoft.com/library/hh856048.aspx) cmdlet을 사용하여 서명된 앱 패키지(.appx)를 사용자 계정에 배포합니다. 

Desktop App Converter(v0.1.24)에서 ```-Sign``` 플래그를 사용하여 변환된 앱에 자동 서명할 수 있습니다. 또는 AppX 패키지에 자체 서명하는 방법은 [변환된 데스크톱 앱에 서명](desktop-to-uwp-signing.md)을 참조하세요.

Add-appxpackage PowerShell cmdlet의 ```-Register``` 매개 변수를 활용하여 개발 프로세스 동안 패키지에 포함되지 않은 파일의 폴더에서 설치할 수도 있습니다. 

변환된 앱 배포 및 디버깅에 대한 자세한 내용은 [변환된 UWP 앱 배포 및 디버그](desktop-to-uwp-deploy-and-debug.md)를 참조하세요. 

## <a name="sign-your-appx-package"></a>.AppX 패키지 서명

Add-appxpackage cmdlet에서는 배포 중인 응용 프로그램 패키지(.appx)가 서명되어야 합니다. .appx 패키지에 서명하려면 Microsoft Windows 10 SDK에서 제공하는 변환기 명령줄 또는 SignTool.exe의 일부인 ```-Sign``` 플래그를 사용합니다.

.appx 패키지에 서명하는 방법은 [변환된 데스크톱 앱에 서명](desktop-to-uwp-signing.md)을 참조하세요. 

## <a name="caveats"></a>주의 사항

1. 호스트 컴퓨터의 Windows 10 빌드는 Desktop App Converter 다운로드의 일부로 받은 기본 이미지와 일치해야 합니다.  
2. 변환기는 모든 디렉터리 콘텐츠를 격리된 Windows 환경으로 복사하기 때문에 데스크톱 설치 관리자는 독립된 디렉터리에 있습니다.  
3. 현재 Desktop App Converter는 64비트 운영 체제에서만 변환 프로세스의 실행을 지원합니다. 변환된 .appx 패키지를 64비트(x64) OS에만 배포할 수 있습니다.  
4. Desktop App Converter를 무인 모드에서 실행하려면 데스크톱 설치 관리자가 필요합니다. *-InstallerArguments* 매개 변수를 사용하여 설치 관리자에 대한 자동 플래그를 변환기에 제공해야 합니다.
5. 공용 SxS Fusion 어셈블리 게시는 작동하지 않습니다. 설치하는 동안 응용 프로그램이 다른 프로세스에서 액세스할 수 있는 공개 side-by-side Fusion 어셈블리를 게시할 수 있습니다. 프로세스 활성화 컨텍스트 생성 동안 이러한 어셈블리는 CSRSS.exe라는 시스템 프로세스에 의해 검색됩니다. 변환 프로세스에 대해 이 작업이 수행되면 이러한 어셈블리의 활성화 컨텍스트 생성 및 모듈 로드가 실패합니다. ComCtl과 같은 받은 편지함 어셈블리는 OS와 함께 제공되므로 해당 종속성을 유지하는 것이 안전합니다. SxS Fusion 어셈블리는 다음 위치에 등록됩니다.
  + 레지스트리: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + 파일 시스템: %windir%\\SideBySide

## <a name="known-issues"></a>알려진 문제

+ 이전에 Desktop App Converter를 설치한 개발자 컴퓨터에서 Windows 참가자 플라이트를 수신하는 경우 새로운 기본 이미지를 설정할 때 `New-ContainerNetwork: The object already exists` 오류가 발생할 수 있습니다. 이 문제를 해결하려면 관리자 권한 명령 프롬프트에서 `Netsh int ipv4 reset` 명령을 실행한 다음 컴퓨터를 다시 부팅합니다. 
+ 주 실행 파일 또는 종속성이 "Program Files" 또는 "Windows\System32" 아래에 배치된 경우 "AnyCPU" 빌드 옵션을 사용하여 컴파일된 .NET 앱이 설치되지 않습니다. 이 문제를 해결하려면 아키텍처 관련 데스크톱 설치 관리자(32비트 또는 64비트)를 사용하여 AppX 패키지를 성공적으로 생성하세요.

## <a name="telemetry-from-desktop-app-converter"></a>Desktop App Converter의 원격 분석  
Desktop App Converter가 사용자 및 사용자의 소프트웨어 사용에 대한 정보를 수집한 후 Microsoft로 보낼 수 있습니다. 제품 설명서 및 [Microsoft 개인 정보 취급 방침](http://go.microsoft.com/fwlink/?LinkId=521839)에서Microsoft의 데이터 수집 및 사용에 대해 알아볼 수 있습니다. Microsoft 개인 정보 취급 방침의 모든 규정을 준수한다는 데 동의합니다.

기본적으로 Desktop App Converter에 대한 원격 분석은 사용되도록 설정됩니다. 원격 분석을 원하는 설정으로 구성하려면 다음 레지스트리 키를 추가합니다.  
```CMD
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
|```-Installer <String>``` | 응용 프로그램의 설치 관리자 경로로, 무인/자동으로 실행할 수 있어야 합니다.|
|```-InstallerArguments <String>``` [옵션] | 강제로 설치 관리자를 무인/자동으로 실행할 수 있도록 하기 위한 쉼표로 구분된 인수 목록 또는 인수 문자열입니다. 설치 관리자가 msi인 경우 이 매개 변수는 선택적입니다. 설치 관리자에서 로그를 가져오려면 여기에 설치 관리자에 대한 로깅 인수를 제공하고, 변환기가 적절한 경로로 대체하는 토큰에 해당하는 경로 ```<log_folder>```를 사용합니다. <br><br>**참고: 무인/자동 플래그와 로그 인수는 설치 관리자 기술마다 다릅니다.** <br><br>이 매개 변수에 대한 사용 예제: ```-InstallerArguments "/silent /log <log_folder>\install.log"``` 로그 파일을 생성하지 않는 또 다른 예제는 다음과 같습니다. ```-InstallerArguments "/quiet", "/norestart"``` 변환기가 로그를 캡처한 후 최종 로그 폴더에 추가하게 하려면 토큰 경로 ```<log_folder>```로 모든 로그를 보내야 합니다.|
|```-InstallerValidExitCodes <Int32>``` [옵션] | 설치 관리자가 성공적으로 실행되었음을 나타내는 쉼표로 구분된 종료 코드 목록(예: 0, 1234, 5678).  기본적으로 msi가 아닌 경우는 0이고 msi인 경우는 0, 1641, 3010입니다.|

### <a name="appx-identity-parameters"></a>Appx ID 매개 변수  

|매개 변수|설명|
|---------|-----------|
|```-PackageName <String>``` | 유니버설 Windows 앱 패키지의 이름
|```-Publisher <String>``` | 유니버설 Windows 앱 패키지의 게시자
|```-Version <Version>``` | 유니버설 Windows 앱 패키지의 버전 번호

### <a name="optional-appx-manifest-parameters"></a>선택적 Appx 매니페스트 매개 변수  

|매개 변수|설명|
|---------|-----------|
|```-AppExecutable <String> [optional]``` [옵션] | 응용 프로그램의 주 실행 파일(예: "MyApp.exe")의 이름입니다. |
|```-AppFileTypes <String>``` [옵션] | 응용 프로그램과 연결될 쉼표로 구분된 파일 형식 목록(예: 큰따옴표를 제외한 ".txt, .doc")|
|```-AppId <String>``` [옵션] | 응용 프로그램 ID를 appx 매니페스트로 설정하기 위한 값을 지정합니다. 값을 지정하지 않으면 *PackageName*에 대해 제공한 값으로 설정됩니다.|
|```-AppDisplayName <String>``` [옵션] | 응용 프로그램 표시 이름을 appx 매니페스트로 설정하기 위한 값을 지정합니다. 값을 지정하지 않으면 *PackageName*에 대해 제공한 값으로 설정됩니다. |
|```-AppDescription <String>``` [옵션] | 응용 프로그램 설명을 appx 매니페스트로 설정하기 위한 값을 지정합니다. 값을 지정하지 않으면 *PackageName*에 대해 제공한 값으로 설정됩니다.|
|```-PackageDisplayName <String>``` [옵션] | 패키지 표시 이름을 appx 매니페스트로 설정하기 위한 값을 지정합니다. 값을 지정하지 않으면 *PackageName*에 대해 제공한 값으로 설정됩니다. |
|```-PackagePublisherDisplayName <String>``` [옵션] | 패키지 게시자 표시 이름을 appx 매니페스트로 설정하기 위한 값을 지정합니다. 값을 지정하지 않으면 *Publisher*에 대해 제공한 값으로 설정됩니다. |

### <a name="other-conversion-parameters"></a>기타 변환 매개 변수  

|매개 변수|설명|
|---------|-----------|
|```-ExpandedBaseImage <String>``` [옵션] | 이미 확장된 기본 이미지에 대한 전체 경로.|
|```-MakeAppx [<SwitchParameter>]``` [옵션] | 스위치(있는 경우)는 출력에 대해 MakeAppx를 호출하도록 이 스크립트에 지시합니다. |
|```-LogFile <String>``` [옵션] | 로그 파일을 지정합니다. 생략하면 로그 파일의 임시 위치가 생성됩니다. |
| ```Sign [<SwitchParameter>] [optional]``` | 이 스크립트를 통해 출력 appx에 서명합니다. 이 스위치는 ```-MakeAppx``` 스위치와 함께 있어야 합니다. 
|```<Common parameters>``` | 이 cmdlet은 *Verbose*, *Debug*, *ErrorAction*, *ErrorVariable*, *WarningAction*, *WarningVariable*, *OutBuffer*, *PipelineVariable*, *OutVariable* 등의 일반 매개 변수를 지원합니다. 자세한 내용은 [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216)를 참조하세요. |

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

변환 프로세스 동안 DesktopAppConverter는 모든 손상된 PE 헤더를 수정하기 위해 자동으로 PEHeaderCertFixTool을 실행합니다. 그러나 UWP appx, 느슨한 파일 또는 특정 이진에서 PEHeaderCertFixTool을 실행할 수도 있습니다. 

PEHeaderCertFixTool은 DesktopAppConverter.zip의 일부로 제공됩니다. 사용 예제: 

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


<!--HONumber=Dec16_HO1-->


