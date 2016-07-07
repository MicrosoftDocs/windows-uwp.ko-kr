---
author: awkoren
Description: "데스크톱 변환기 앱을 실행하여 UWP(유니버설 Windows 플랫폼) 앱으로 Windows 데스크톱 응용 프로그램(예&#58; Win32, WPF 및 Windows Forms)을 수동으로 변환합니다."
Search.Product: eADQiWindows 10XVcnh
title: "데스크톱 앱 변환기 미리 보기(Project Centennial)"
ms.sourcegitcommit: 07016fabb8b49e57dd0ae4ef68447451d31aa2dc
ms.openlocfilehash: bc28197cccc0559f57abc8cb81e23bf241ca3716

---

# 데스크톱 앱 변환기 미리 보기(Project Centennial)

\[일부 정보는 상업용으로 출시되기 전에 상당 부분 수정될 수 있는 시험판 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.\]

[데스크톱 앱 변환기를 다운로드합니다.](http://go.microsoft.com/fwlink/?LinkId=785437)

데스크톱 앱 변환기는 .NET 4.6.1 또는 Win32용으로 작성된 기존 데스크톱 앱을 UWP(유니버설 Windows 플랫폼)으로 가져올 수 있는 시험판 도구입니다. 무인(자동) 모드에서 변환기를 통해 데스크톱 설치 관리자를 실행하고 개발 컴퓨터에서 Add-AppxPackage PowerShell cmdlet을 사용하여 설치할 수 있는 AppX 패키지를 가져올 수 있습니다.

변환기는 변환기 다운로드의 일부로 제공된 새로운 기본 이미지를 사용하여 격리된 Windows 환경에서 데스크톱 설치 관리자를 실행합니다. 또한 데스크톱 설치 관리자가 수행한 모든 레지스트리 및 파일 시스템 I/O를 캡처하고 출력의 일부로 패키징합니다. 변환기는 패키지 ID와 방대한 WinRT API를 호출하는 기능을 포함하여 AppX를 출력합니다.

## 새로운 기능

이 섹션에서는 데스크톱 앱 변환기의 버전 간 변경 내용을 대략적으로 설명합니다. 

### 6/16/2016

* 데스크톱 앱 변환기(v0.1.20)는 최신 Windows 10 Insider Preview 빌드에서 성공적인 변환을 방해하는 문제를 해결합니다. 
* ```–CreateX86Package```가 ```–PackageArch```로 대체되어 생성된 패키지에 대한 아키텍처를 지정할 수 있습니다. 

### 6/8/2016

* 변환기를 실행하는 AMD64 호스트 컴퓨터에 x86 appx 패키지를 생성하기 위한 지원이 추가되었습니다.
* 이전에 확장된 기본 이미지를 제거하여 디스크 공간 사용이 감소했습니다.
* 임시 파일과 불필요한 기본 이미지를 정리하기 위한 지원이 추가되었습니다.
* 파일 형식 및 프로토콜 연결을 검색하기 위한 지원이 개선되었습니다.
* 큰 앱 집합에 대한 AppExecutable 속성을 검색하는 논리가 개선되었습니다.
* MSI 기반 설치 관리자에 추가 –InstallerArguments를 제공하기 위한 지원이 추가되었습니다.
* 변환 프로세스 중에 PathTooLongException 오류에 대한 버그가 수정되었습니다.

### 5/12/2016

- Windows Pro 버전에 대한 지원이 복원되었습니다. 
- 변환기 ```-Setup``` 플래그는 이제 Windows 컨테이너 기능을 사용하도록 설정하고 기본 이미지 확장을 처리합니다. 관리자 권한의 PowerShell 프롬프트에서 다음을 실행하여 일회용 설치를 수행합니다. ```PS C:\> .\DesktopAppConverter.ps1 -Setup -BaseImage BaseImage-12345.wim -Verbose```
- 런타임에 불필요한 파일 시스템 리디렉션을 줄이기 위해 앱 설치 경로의 자동 검색과 VFS 외부로의 응용 프로그램 루트 이동 기능이 추가되었습니다.
- 변환 프로세스의 일부로 확장된 기본 이미지에 대한 자동 검색 기능이 추가되었습니다.
- 파일 형식 연결 및 프로토콜에 대한 자동 검색 기능이 추가되었습니다.
- 시작 메뉴 바로 가기를 감지하는 논리가 개선되었습니다.
- 앱 설치 MUI 파일을 유지하는 파일 시스템 필터링 기능이 개선되었습니다.
- 매니페스트에서 Project Centennial에 대한 최소 지원 데스크톱 버전(10.0.14342.0)이 업데이트되었습니다.

## 시스템 요구 사항

### 지원되는 운영 체제
+ Windows 10 1주년 업데이트 Enterprise Edition 미리 보기(빌드 10.0.14342.0 이상)

### 필수 하드웨어 구성

컴퓨터에는 다음 최소 기능이 있어야 합니다.
+ 64비트(x64) 프로세서
+ 하드웨어 지원 가상화
+ SLAT(Second Level Address Translation)

### 권장 리소스
+ [Windows 10용 Windows SDK(소프트웨어 개발 키트)](https://developer.microsoft.com/en-us/windows/downloads/windows-10-sdk)

## 데스크톱 앱 변환기 설정   
데스크톱 앱 변환기는 Windows Insider Preview 빌드의 일부로 플라이트된 Windows 10 기능에 의존합니다. 이 변환기를 사용하려면 최신 빌드를 사용하고 있는지 확인합니다.

1. 최신 Windows 10 Insider Preview OS - Enterprise 또는 Pro Edition(http://insider.windows.com)이 있는지 확인합니다. 
2. DesktopAppConverter.zip 및 Insider Preview 빌드(http://aka.ms/converter)와 일치하는 기본 이미지 .wim 파일을 다운로드합니다. 
3. 로컬 폴더에 DesktopAppConverter.zip의 압축을 풉니다.
4. 관리자 PowerShell 창에서 다음을 수행합니다.  
```CMD
PS C:\> Set-ExecutionPolicy bypass
```
5. 관리자 PowerShell 창에서 다음 명령을 실행하여 변환기를 설정합니다.
```CMD
PS C:\> .\DesktopAppConverter.ps1 -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose
```
6. 이전 명령을 실행할 경우 다시 부팅하라는 메시지가 표시되면 컴퓨터를 다시 시작하고 명령을 다시 실행합니다.

## 데스크톱 앱 변환기 실행
데스크톱 앱 변환기는 PowerShell 및 명령 셸의 두 진입점을 사용하여 시작할 수 있습니다. 이러한 진입점 중 하나를 사용하여 변환 프로세스를 시작할 수 있습니다.

### 사용법
```CMD
DesktopAppConverter.ps1
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

### 예제
다음 예제에서는 *&lt;publisher_name&gt;*에 의해 데스크톱 앱 *MyApp*을 UWP 패키지(AppX)로 변환하는 방법을 보여 줍니다.

+ 관리자 PowerShell 창에서 다음 명령을 실행합니다.
```CMD
PS C:\>.\DesktopAppConverter.ps1 -Installer C:\Installer\MyApp.exe 
-InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" 
-Publisher "CN=<publisher_name>" -Version 0.0.0.1 -MakeAppx -Verbose
```

## 변환된 AppX 배포
PowerShell에서 [Add-AppxPackage](https://technet.microsoft.com/en-us/library/hh856048.aspx) cmdlet을 사용하여 서명된 앱 패키지(.appx)를 사용자 계정에 배포합니다. .appx 패키지에 서명하려면 “.Appx 패키지 서명" 섹션을 참조하세요. 또한 개발 프로세스 동안 cmdlet의 *Register* 매개 변수를 포함하여 패키지되지 않은 파일의 폴더에서 설치할 수 있습니다. 자세한 내용은 [변환된 UWP 앱 배포 및 디버그](desktop-to-uwp-deploy-and-debug.md)를 참조하세요.

## .AppX 패키지 서명

Add-appxpackage cmdlet에서는 배포 중인 응용 프로그램 패키지(.appx)가 서명되어야 합니다. .appx 패키지를 서명하려면 Microsoft Windows 10 SDK에서 제공된 SignTool.exe를 사용합니다.

### 예제
```CMD
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
```
**참고:** MakeCert.exe를 실행하고 암호를 입력하라는 메시지가 표시되면 **없음**을 선택합니다.

인증서와 서명에 대한 자세한 내용은 다음을 참조하세요.

+ [개발 중 사용할 임시 인증서를 만드는 방법](https://msdn.microsoft.com/library/ms733813.aspx)
+ [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx)
+ [SignTool.exe(서명 도구)](https://msdn.microsoft.com/library/8s9b9yaz.aspx)

### 주의 사항
1. 호스트 컴퓨터의 Windows 10 빌드는 데스크톱 앱 변환기 다운로드의 일부로 받은 기본 이미지와 일치해야 합니다.  
2. 변환기는 모든 디렉터리 콘텐츠를 격리된 Windows 환경으로 복사하기 때문에 데스크톱 설치 관리자는 독립된 디렉터리에 있습니다.  
3. 현재 데스크톱 앱 변환기는 64비트 운영 체제에서만 변환 프로세스의 실행을 지원합니다. 변환된 .appx 패키지를 64비트(x64) OS에만 배포할 수 있습니다.  
4. 데스크톱 앱 변환기를 무인 모드에서 실행하려면 데스크톱 설치 관리자가 필요합니다. *-InstallerArguments* 매개 변수를 사용하여 설치 관리자에 대한 자동 플래그를 변환기에 제공해야 합니다.
5. 공용 SxS Fusion 어셈블리 게시는 작동하지 않습니다. 설치하는 동안 응용 프로그램이 다른 프로세스에서 액세스할 수 있는 공개 side-by-side Fusion 어셈블리를 게시할 수 있습니다. 프로세스 활성화 컨텍스트 생성 동안 이러한 어셈블리는 CSRSS.exe라는 시스템 프로세스에 의해 검색됩니다. Centennial 프로세스에 대해 이 작업이 수행되면 이러한 어셈블리의 활성화 컨텍스트 생성 및 모듈 로드가 실패합니다. ComCtl과 같은 받은 편지함 어셈블리는 OS와 함께 제공되므로 Centennial 프로세스에서 해당 종속성을 유지하는 것이 안전합니다. SxS Fusion 어셈블리는 다음 위치에 등록됩니다.
  + 레지스트리: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + 파일 시스템: %windir%\\SideBySide

## 알려진 문제

+ 이전에 데스크톱 앱 변환기 Preview를 설치한 개발자 컴퓨터에서 Windows Insider 플라이트를 수신하는 경우 새로운 기본 이미지를 설정할 때 `New-ContainerNetwork: The object already exists` 오류가 발생할 수 있습니다. 이 문제를 해결하려면 관리자 권한 명령 프롬프트에서 `Netsh int ipv4 reset` 명령을 실행한 다음 컴퓨터를 다시 부팅합니다. 
+ 주 실행 파일 또는 종속성이 "Program Files" 또는 "Windows\System32" 아래에 배치된 경우 "AnyCPU" 빌드 옵션을 사용하여 컴파일된 .NET 앱이 설치되지 않습니다. 이 문제를 해결하려면 아키텍처 관련 데스크톱 설치 관리자(32비트 또는 64비트)를 사용하여 AppX 패키지를 성공적으로 생성하세요.

## 데스크톱 앱 변환기의 원격 분석  
데스크톱 앱 변환기가 사용자 및 사용자의 소프트웨어 사용에 대한 정보를 수집한 후 Microsoft로 보낼 수 있습니다. 제품 설명서 및 [Microsoft 개인 정보 취급 방침](http://go.microsoft.com/fwlink/?LinkId=521839)에서Microsoft의 데이터 수집 및 사용에 대해 알아볼 수 있습니다. Microsoft 개인 정보 취급 방침의 모든 규정을 준수한다는 데 동의합니다.

기본적으로 데스크톱 앱 변환기에 대한 원격 분석은 사용되도록 설정됩니다. 원격 분석을 원하는 설정으로 구성하려면 다음 레지스트리 키를 추가합니다.  
```CMD
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DesktopAppConverter
```
+ 1로 설정된 DWORD를 사용하여 *DisableTelemetry* 값을 추가 또는 편집합니다.
+ 원격 분석을 사용하려면 이 키를 제거하거나 값을 0으로 설정합니다.

## 데스크톱 앱 변환기 사용법
데스크톱 앱 변환기에 대한 매개 변수 목록은 다음과 같습니다. 다음 명령을 실행하여 Windows Powershell 창에서 이 목록을 볼 수도 있습니다.  
```CMD
get-help .\DesktopAppConverter.ps1 -detailed
```

### 설정 매개 변수  
|매개 변수|설명|
|---------|-----------|
|```-Setup [<SwitchParameter>]``` | 설치 모드에서 DesktopAppConverter를 실행합니다. 설치 모드는 제공된 기본 이미지 확장을 지원합니다.|
|```-BaseImage <String>``` | 확장되지 않은 기본 이미지에 대한 전체 경로. 이 매개 변수는 -Setup을 지정한 경우에 필요합니다.|
|```-LogFile <String>``` [옵션] | 로그 파일을 지정합니다. 생략하면 로그 파일의 임시 위치가 생성됩니다.|
|```-NatSubnetPrefix <String>``` [옵션] | Nat 인스턴스에 사용할 접두사 값입니다. 일반적으로 사용자는 호스트 컴퓨터가 변환기의 NetNat과 동일한 서브넷 범위에 연결되는 경우에만 이 값을 변경하려고 합니다. **Get NetNat** cmdlet을 사용하여 현재 변환기 NetNat 구성을 쿼리할 수 있습니다. |
|```-NoRestart [<SwitchParameter>]``` | 설치 프로그램을 실행할 때 다시 부팅하라는 메시지를 표시하지 않습니다(컨테이너 기능을 사용하도록 설정하려면 다시 부팅해야 함). |

### 변환 매개 변수  
|매개 변수|설명|
|---------|-----------|
|```-Installer <String>``` | 응용 프로그램의 설치 관리자 경로로, 무인/자동으로 실행할 수 있어야 합니다.|
|```-InstallerArguments <String>``` [옵션] | 강제로 설치 관리자를 무인/자동으로 실행할 수 있도록 하기 위한 쉼표로 구분된 인수 목록 또는 인수 문자열입니다. 설치 관리자가 msi인 경우 이 매개 변수는 선택적입니다. 설치 관리자에서 로그를 가져오려면 여기에 설치 관리자에 대한 로깅 인수를 제공하고, 변환기가 적절한 경로로 대체하는 토큰에 해당하는 경로 ```<log_folder>```를 사용합니다. <br><br>**참고: 무인/자동 플래그와 로그 인수는 설치 관리자 기술마다 다릅니다.** <br><br>이 매개 변수에 대한 사용 예제: ```-InstallerArguments "/silent /log <log_folder>\install.log"``` 로그 파일을 생성하지 않는 또 다른 예제는 다음과 같습니다. ```-InstallerArguments "/quiet", "/norestart"``` 변환기가 로그를 캡처한 후 최종 로그 폴더에 추가하게 하려면 토큰 경로 ```<log_folder>```로 모든 로그를 보내야 합니다.|
|```-InstallerValidExitCodes <Int32>``` [옵션] | 설치 관리자가 성공적으로 실행되었음을 나타내는 쉼표로 구분된 종료 코드 목록(예: 0, 1234, 5678).  기본적으로 msi가 아닌 경우는 0이고 msi인 경우는 0, 1641, 3010입니다.|
|```-Destination <String>``` | 원하는 변환기 appx 출력 대상으로, DesktopAppConverter는 이 위치가 없으면 만들 수 있습니다.|

### Appx ID 매개 변수  
|매개 변수|설명|
|---------|-----------|
|```-PackageName <String>``` | 유니버설 Windows 앱 패키지의 이름
|```-Publisher <String>``` | 유니버설 Windows 앱 패키지의 게시자
|```-Version <Version>``` | 유니버설 Windows 앱 패키지의 버전 번호

### 선택적 Appx 매니페스트 매개 변수  
|매개 변수|설명|
|---------|-----------|
|```-AppExecutable <String>``` [옵션] | 설치할 수 있는(반드시 설치할 필요는 없음) 응용 프로그램 주 실행 파일의 전체 경로. 예: "C:\Program Files (x86)\MyApp\MyApp.exe"|
|```-AppFileTypes <String>``` [옵션] | 응용 프로그램과 연결될 쉼표로 구분된 파일 형식 목록(예: 큰따옴표를 제외한 ".txt, .doc")|
|```-AppId <String>``` [옵션] | 응용 프로그램 ID를 appx 매니페스트로 설정하기 위한 값을 지정합니다. 값을 지정하지 않으면 *PackageName*에 대해 제공한 값으로 설정됩니다.|
|```-AppDisplayName <String>``` [옵션] | 응용 프로그램 표시 이름을 appx 매니페스트로 설정하기 위한 값을 지정합니다. 값을 지정하지 않으면 *PackageName*에 대해 제공한 값으로 설정됩니다. |
|```-AppDescription <String>``` [옵션] | 응용 프로그램 설명을 appx 매니페스트로 설정하기 위한 값을 지정합니다. 값을 지정하지 않으면 *PackageName*에 대해 제공한 값으로 설정됩니다.|
|```-PackageDisplayName <String>``` [옵션] | 패키지 표시 이름을 appx 매니페스트로 설정하기 위한 값을 지정합니다. 값을 지정하지 않으면 *PackageName*에 대해 제공한 값으로 설정됩니다. |
|```-PackagePublisherDisplayName <String>``` [옵션] | 패키지 게시자 표시 이름을 appx 매니페스트로 설정하기 위한 값을 지정합니다. 값을 지정하지 않으면 *Publisher*에 대해 제공한 값으로 설정됩니다. |

### 기타 변환 매개 변수  
|매개 변수|설명|
|---------|-----------|
|```-ExpandedBaseImage <String>``` [옵션] | 이미 확장된 기본 이미지에 대한 전체 경로.|
|```-MakeAppx [<SwitchParameter>]``` [옵션] | 스위치(있는 경우)는 출력에 대해 MakeAppx를 호출하도록 이 스크립트에 지시합니다. |
|```-LogFile <String>``` [옵션] | 로그 파일을 지정합니다. 생략하면 로그 파일의 임시 위치가 생성됩니다. |
|```<Common parameters>``` | 이 cmdlet은 *Verbose*, *Debug*, *ErrorAction*, *ErrorVariable*, *WarningAction*, *WarningVariable*, *OutBuffer*, *PipelineVariable*, *OutVariable* 등의 일반 매개 변수를 지원합니다. 자세한 내용은 [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216)를 참조하세요. |

### 정리 매개 변수
|매개 변수|설명|
|---------|-----------|
|```Cleanup [<Option>]``` | DesktopAppConverter 아티팩트에 대한 정리를 실행합니다. 정리 모드에는 3가지 유효한 옵션이 있습니다. |
|```Cleanup All``` | 확장된 기본 이미지를 모두 삭제하고, 임시 변환기 파일을 제거하고, 컨테이너 네트워크를 제거하고, 선택적 Windows 기능인 컨테이너를 사용하지 않도록 설정합니다. |
|```Cleanup WorkDirectory``` | 모든 임시 변환기 파일을 제거합니다. |
|```Cleanup ExpandedImages``` | 호스트 컴퓨터에 설치된 확장된 기본 이미지를 모두 삭제합니다. |

### 패키지 아키텍처
데스크톱 앱 변환기 Preview는 이제 x86 및 amd64 컴퓨터에서 설치하고 실행할 수 있는 x86 및 x64 앱 패키지를 만드는 기능을 지원합니다. 하지만 성공적인 변환을 수행하려면 데스크톱 앱 변환기가 AMD64 컴퓨터에서 실행되어야 합니다.

|매개 변수|설명|
|---------|-----------|
|```-PackageArch <String>``` | 지정된 아키텍처를 사용하여 패키지를 생성합니다. 유효한 옵션은 'x86' 또는 'x64'입니다(예: -PackageArch x86). 이 매개 변수는 선택 사항입니다. 지정하지 않으면 DesktopAppConverter는 패키지 아키텍처를 자동으로 검색하려고 합니다. 자동 검색에 실패하면 기본적으로 x64 패키지로 설정합니다. |

## 참고 항목
+ [데스크톱 앱 변환기 다운로드](http://go.microsoft.com/fwlink/?LinkId=785437)
+ [데스크톱 앱을 유니버설 Windows 플랫폼으로 가져오기](https://developer.microsoft.com/en-us/windows/bridges/desktop)
+ [데스크톱 앱 변환기를 사용하여 데스크톱 앱을 UWP로 가져오기](https://channel9.msdn.com/events/Build/2016/P504)
+ [Project Centennial: 기존 데스크톱 응용 프로그램을 유니버설 Windows 플랫폼으로 가져오기](https://channel9.msdn.com/events/Build/2016/B829)  
+ [데스크톱 브리지용 UserVoice(Project Centennial)](http://aka.ms/UserVoiceDesktopToUwp)
+ [GitHub의 UWP에 대한 데스크톱 앱 브리지 코드 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)



<!--HONumber=Jun16_HO4-->


