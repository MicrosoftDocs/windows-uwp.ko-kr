---
author: normesta
Description: This article contains known issues with the Desktop Bridge.
Search.Product: eADQiWindows 10XVcnh
title: 알려진 문제(데스크톱 브리지)
ms.author: normesta
ms.date: 06/20/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 71f8ffcb-8a99-4214-ae83-2d4b718a750e
ms.localizationpriority: medium
ms.openlocfilehash: 731d449d779806fbd4104787b692d3b7d0408036
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/23/2018
ms.locfileid: "5431803"
---
# <a name="known-issues-with-packaged-desktop-applications"></a>패키지로 만든된 데스크톱 응용 프로그램의 알려진된 문제

이 문서에서는 데스크톱 응용 프로그램용 Windows 앱 패키지를 만들 때 발생할 수 있는 알려진된 문제를 포함 합니다.

<a id="app-converter" />

## <a name="known-issues-with-the-desktop-app-converter"></a>Desktop App Converter에 대해 알려진 문제

### <a name="ecreatingisolatedenvfailed-an-estartingisolatedenvfailed-errors"></a>E_CREATING_ISOLATED_ENV_FAILED an E_STARTING_ISOLATED_ENV_FAILED 오류    

이러한 오류 중 하나를 수신하면 [다운로드 센터](https://aka.ms/converterimages)의에서 유효한 기본 이미지를 사용하고 있는지 확인하세요.
유효한 기본 이미지를 사용하고 있다면 명령에서 ``-Cleanup All``를 사용해 보세요.
이 명령이 작동하지 않을 경우 converter@microsoft.com에 로그를 보내주시면 조사하는 데 도움이 될 것입니다.

### <a name="new-containernetwork-the-object-already-exists-error"></a>New-ContainerNetwork: "개체가 이미 존재" 오류

새로운 기본 이미지를 설치할 때 이러한 오류가 발생할 수 있습니다. 이전에 Desktop App Converter를 설치한 개발자 컴퓨터에서 Windows 참가자 플라이트를 수신하는 경우에 이러한 오류가 발생할 수 있습니다.

이 문제를 해결하려면 관리자 권한 명령 프롬프트에서 `Netsh int ipv4 reset` 명령을 실행한 다음, 컴퓨터를 다시 시작합니다.

### <a name="your-net-application-is-compiled-with-the-anycpu-build-option-and-fails-to-install"></a>.NET 응용 프로그램 "AnyCPU" 빌드 옵션을 사용 하 여 컴파일된 및 설치에 실패

주 실행 파일 또는 종속성이 **Program Files** 또는 **Windows\System32** 아래에 배치된 경우 이 오류가 발생할 수 있습니다.

이 문제를 해결하려면 아키텍처별 데스크톱 설치 관리자(32 비트 또는 64 비트)를 사용해 Windows 앱 패키지를 생성해 보십시오.

### <a name="publishing-public-side-by-side-fusion-assemblies-wont-work"></a>공용 SxS Fusion 어셈블리 게시는 작동하지 않습니다.

 설치하는 동안 응용 프로그램이 다른 프로세스에서 액세스할 수 있는 공용 SxS Fusion 어셈블리를 게시할 수 있습니다. 프로세스 활성화 컨텍스트 생성 동안 이러한 어셈블리는 CSRSS.exe라는 시스템 프로세스에 의해 검색됩니다. 변환 프로세스에 대해 이 작업이 수행되면 이러한 어셈블리의 활성화 컨텍스트 생성 및 모듈 로드가 실패합니다. SxS Fusion 어셈블리는 다음 위치에 등록됩니다.
  + 레지스트리: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + 파일 시스템: %windir%\\SideBySide

이것은 알려진 제한 사항이며, 어떤 해결책도 현재 존재하지 않습니다. 즉, ComCtl과 같은 받은 편지함 어셈블리는 OS와 함께 제공되므로 해당 종속성을 유지하는 것이 안전합니다.

### <a name="error-found-in-xml-the-executable-attribute-is-invalid---the-value-myappexe-is-invalid-according-to-its-datatype"></a>XML에서 오류 발견 '실행 파일' 특성이 유효 - 데이터 형식으로 볼 때 'MyApp.EXE' 값은 잘못된 것입니다.

애플리케이션의 실행 파일에 대문자 **.EXE** 확장명이 있는 경우 이런 오류가 발생할 수 있습니다. 하지만이 확장명의 대/소문자 표기 응용 프로그램의 실행 여부에 영향을 DAC에서이 오류가 발생할 수 있습니다.

이 문제를 해결하려면 패키징 시 **-AppExecutable** 플래그를 지정하고 주 실행 파일의 확장명으로 소문자 ".exe"를 사용해 보십시오(예: MYAPP.exe).    또는 모든 실행 파일에서 소문자를 소문자로 응용 프로그램에서 변경할 수 있습니다 (예:에서. .Exe EXE)입니다.

### <a name="corrupted-or-malformed-authenticode-signatures"></a>손상되거나 잘못된 형식의 Authenticode 서명

이 섹션에서는 손상되거나 잘못된 형식의 Authenticode 서명이 포함되어 있을 수 있는 Windows 앱 패키지의 PE(이식 가능 파일) 파일과 관련된 문제를 식별하는 방법에 대해 설명합니다. .exe, .dll, .chm 등의 이진 형식일 수 있는 PE 파일의 Authenticode 서명이 잘못된 경우 패키지가 올바르게 서명되지 않으므로 Windows 앱 패키지를 배포할 수 없게 됩니다.

PE 파일의 Authenticode 서명 위치는 선택적 헤더 데이터 디렉터리의 인증서 표 항목 및 관련된 특성 인증서 표에 의해 지정됩니다. 서명 확인 중에 이러한 구조에 지정된 정보는 PE 파일에서 서명을 찾는 데 사용됩니다. 이러한 값이 손상되면 파일이 잘못 서명된 것처럼 보일 수 있습니다.

Authenticode 서명이 올바르려면 Authenticode 서명에 대해 다음 조건을 충족해야 합니다.

- PE 파일에 있는 **WIN_CERTIFICATE** 항목의 시작을 실행 파일의 끝을 지나도록 확장할 수 없습니다.
- **WIN_CERTIFCATE** 항목이 이미지의 끝에 위치해야 합니다.
- **WIN_CERTIFICATE** 항목의 크기는 양수여야 합니다.
- **WIN_CERTIFICATE** 항목이 32비트 실행 파일의 경우 **IMAGE_NT_HEADERS32** 구조체 뒤에서 시작해야 하고 64비트 실행 파일의 경우 IMAGE_NT_HEADERS64 구조체 뒤에서 시작해야 합니다.

자세한 내용은 [Authenticode 포털 실행 파일 사양](http://download.microsoft.com/download/9/c/5/9c5b2167-8017-4bae-9fde-d599bac8184a/Authenticode_PE.docx) 및 [PE 파일 형식 사양](https://msdn.microsoft.com/windows/hardware/gg463119.aspx)을 참조하세요.

SignTool.exe는 Windows 앱 패키지에 서명하려고 할 때 손상되거나 잘못된 형식의 이진 파일 목록을 출력할 수 있습니다. 이렇게 하려면 APPXSIP_LOG 환경 변수를 1로 설정하여(예: ```set APPXSIP_LOG=1```) 자세한 정보 로깅을 사용하도록 설정하고 SignTool.exe를 다시 실행합니다.

이러한 잘못된 형식의 이진 파일을 수정하려면 해당 이진 파일이 위의 요구 사항을 따르는지 확인합니다.

## <a name="you-receive-the-error----msb4018-the-generateresource-task-failed-unexpectedly"></a>'MSB4018 예기치 않게 "GenerateResource" 작업이 실패했습니다.'는 오류가 발생

위성 어셈블리를 PRI(Package Resource Index) 파일로 변환하려 시도할 때 발생할 수 있는 오류입니다.

저희는 이 문제를 인식하고, 장기적인 솔루션을 찾고 있습니다. 임시 해결 방법으로 호스팅 프로젝트 파일의 첫 PropertyGroup 요소에 이 XML 줄을 추가, 리소스 생성기를 끌 수 있습니다.

``<AppxGeneratePrisForPortableLibrariesEnabled>false</AppxGeneratePrisForPortableLibrariesEnabled>``

## <a name="blue-screen-with-error-code-0x139-kernelsecuritycheckfailure"></a>블루 스크린, 오류 코드 0x139 (KERNEL_SECURITY_CHECK_FAILURE)

Microsoft Store에서 특정 앱을 설치하거나 실행한 후 컴퓨터가 **0x139 (KERNEL\_SECURITY\_CHECK\_ FAILURE)** 오류로 인해 예기치 않게 다시 부팅될 수 있습니다.

영향을 받는 것으로 알려진 앱에는 Kodi, JT2Go, Ear Trumpet, Teslagrad 등이 포함됩니다.

2016년 10월 27일 릴리스된 [Windows 업데이트(버전 14393.351-KB3197954)](https://support.microsoft.com/kb/3197954)에는 이 문제를 해결하는 중요한 수정이 포함되어 있습니다. 이 문제가 발생하면 컴퓨터를 업데이트합니다. 로그인하기 전에 컴퓨터를 다시 시작해야 하기 때문에 PC를 업데이트할 수 없는 경우 시스템 복원을 사용하여 영향을 받는 앱 중 하나를 설치하기 전의 지점으로 시스템을 복구해야 합니다. 시스템 복원을 사용하는 방법에 대한 자세한 내용은 [Windows 10의 복구 옵션](https://support.microsoft.com/help/12415/windows-10-recovery-options)을 참조하세요.

업데이트해도 문제가 해결되지 않거나 PC를 복구하는 방법을 잘 모를 경우 [Microsoft 지원](https://support.microsoft.com/contactus/)에 문의하세요.

개발자인 경우에는 이 업데이트를 포함하지 않는 Windows 버전에서 패키지된 응용 프로그램 설치를 방지할 수 있습니다. 이때 응용 프로그램이이 작업을 수행 하 여 업데이트를 아직 설치 하지 않은 사용자에 게 사용할 수 없습니다. 이 업데이트를 설치한 사용자에 게 응용 프로그램의 가용성을 제한 하려면 AppxManifest.xml 파일을 다음과 같이 수정 합니다.

```<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.351" MaxVersionTested="10.0.14393.351"/>```

Windows 업데이트에 대한 세부 정보는 다음 페이지에서 확인할 수 있습니다.
* https://support.microsoft.com/kb/3197954
* https://support.microsoft.com/help/12387/windows-10-update-history

## <a name="common-errors-that-can-appear-when-you-sign-your-app"></a>앱에 로그인할 때 나타날 수 있는 일반적인 오류

### <a name="publisher-and-cert-mismatch-causes-signtool-error-error-signersign-failed--21470248850x8007000b"></a>게시자와 인증서 불일치로 인해 Signtool 오류 "Error: SignerSign() 실패" (-2147024885/0x8007000b) 발생

Windows 앱 패키지 매니페스트의 게시자 항목이 서명한 인증서의 주체와 일치해야 합니다.  인증서의 주체를 보려면 다음 방법 중 하나를 사용할 수 있습니다.

**옵션 1: Powershell**

다음 PowerShell 명령을 실행합니다. .cer 또는 .pfx에는 동일한 게시자 정보가 있으므로 인증서 파일로 사용할 수 있습니다.

```ps
(Get-PfxCertificate <cert_file>).Subject
```

**옵션 2: 파일 탐색기**

파일 탐색기에서 인증서를 두 번 클릭하여 *세부 정보* 탭을 선택한 다음 목록에서 *Subject* 필드를 선택합니다. 그런 다음 내용을 복사할 수 있습니다.

**옵션 3: CertUtil**

PFX 파일의 명령줄에서 **certutil** 을 실행 하 고 출력에서 *제목* 필드를 복사 합니다.

```cmd
certutil -dump <cert_file.pfx>
```

<a id="bad-pe-cert" />

### <a name="bad-pe-certificate-0x800700c1"></a>잘못 된 PE 인증서 (0x800700C1)

이 패키지에 손상 된 인증서가 있는 이진 파일을 포함 하는 경우 발생할 수 있습니다. 다음은 일부의 이유는 이유이 발생할 수 있습니다.

* 인증서의 시작 화면 이미지의 끝에 되지 않습니다.  

* 인증서의 크기는 양수 하지 않습니다.

* 후 인증서 시작 되지 합니다 `IMAGE_NT_HEADERS32` 후 또는 32 비트 실행 파일에 대 한 구조는 `IMAGE_NT_HEADERS64` 64 비트 실행 파일에 대 한 구조입니다.

* 인증서 포인터 WIN_CERTIFICATE 구조에 대 한 제대로 정렬 되지 않습니다.

잘못 된 PE 인증서를 포함 하는 파일을 찾으려면 **명령 프롬프트**를 열고 라는 환경 변수를 설정 `APPXSIP_LOG` 값이 1 합니다.

```
set APPXSIP_LOG=1
```

그런 다음 **명령 프롬프트**응용 프로그램을 다시 서명 합니다. 예:

```
signtool.exe sign /a /v /fd SHA256 /f APPX_TEST_0.pfx C:\Users\Contoso\Desktop\pe\VLC.appx
```

잘못 된 PE 인증서를 포함 하는 파일에 대 한 정보는 **콘솔 창**에 표시 됩니다. 예:

```
...

ERROR: [AppxSipCustomLoggerCallback] File has malformed certificate: uninstall.exe

...   
```
## <a name="next-steps"></a>다음 단계

**질문에 대한 답변 찾기**

질문이 있으세요? Stack Overflow에서 질문해 주세요. 저희 팀은 이러한 [태그](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다. [여기](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)에서 Microsoft에 문의할 수도 있습니다.

**피드백 제공 또는 기능 제안**

[UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)를 참조하세요.
