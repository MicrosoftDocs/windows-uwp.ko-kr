---
author: normesta
Description: "이 문서에는 데스크톱 브리지의 알려진 문제가 포함되어 있습니다."
Search.Product: eADQiWindows 10XVcnh
title: "알려진 문제(데스크톱 브리지)"
ms.author: normesta
ms.date: 07/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 71f8ffcb-8a99-4214-ae83-2d4b718a750e
ms.openlocfilehash: bf0e81d4a6ff7bd091f25963b1cf26cdecc93636
ms.sourcegitcommit: de6bc8acec2cd5ebc36bb21b2ce1a9980c3e78b2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/17/2017
---
# <a name="known-issues-desktop-bridge"></a>알려진 문제(데스크톱 브리지)

이 문서에는 데스크톱 브리지의 알려진 문제가 포함되어 있습니다.

## <a name="blue-screen-with-error-code-0x139-kernelsecuritycheckfailure"></a>블루 스크린, 오류 코드 0x139 (KERNEL_SECURITY_CHECK_FAILURE)

Windows 스토어에서 특정 앱을 설치하거나 실행한 후 컴퓨터가 **0x139 (KERNEL\_SECURITY\_CHECK\_ FAILURE)** 오류로 인해 예기치 않게 다시 부팅될 수 있습니다.

영향을 받는 것으로 알려진 앱에는 Kodi, JT2Go, Ear Trumpet, Teslagrad 등이 포함됩니다.

2016년 10월 27일 릴리스된 [Windows 업데이트(버전 14393.351-KB3197954)](https://support.microsoft.com/kb/3197954)에는 이 문제를 해결하는 중요한 수정이 포함되어 있습니다. 이 문제가 발생하면 컴퓨터를 업데이트합니다. 로그인하기 전에 컴퓨터를 다시 시작해야 하기 때문에 PC를 업데이트할 수 없는 경우 시스템 복원을 사용하여 영향을 받는 앱 중 하나를 설치하기 전의 지점으로 시스템을 복구해야 합니다. 시스템 복원을 사용하는 방법에 대한 자세한 내용은 [Windows 10의 복구 옵션](https://support.microsoft.com/help/12415/windows-10-recovery-options)을 참조하세요.

업데이트해도 문제가 해결되지 않거나 PC를 복구하는 방법을 잘 모를 경우 [Microsoft 지원](https://support.microsoft.com/contactus/)에 문의하세요.

개발자인 경우에는 이 업데이트를 포함하지 않는 Windows 버전에서 데스크톱 브리지 앱 설치를 방지할 수 있습니다. 이렇게 하면 업데이트를 설치하지 않은 사용자는 앱을 사용할 수 없게 됩니다. 이 업데이트를 설치한 사용자에게 앱의 가용성을 제한하려면 AppxManifest.xml 파일을 다음과 같이 수정합니다.

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

PFX 파일의 명령줄에서 **certutil**을 실행하고 출력에서 *Subject* 필드를 복사합니다.

```cmd
certutil -dump <cert_file.pfx>
```

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

<span id="known-issues-anchor" />
## <a name="known-issues-with-cvbnet-and-c-uwp-projects"></a>C#/VB.NET 및 C++ UWP 프로젝트에 대한 알려진 문제

앱 패키지를 위해 C# 프로젝트 사용을 선호하는 경우, 다음과 같은 알려진 문제를 고려해야 합니다.

- 디버그 모드에서 앱을 빌드하면 오류가 발생합니다(_Microsoft.Net.CoreRuntime.targets(235,5): 오류: 사용자 지정 진입점 실행 파일 응용 프로그램을 지원하지 않습니다. 패키지 매니페스트의 응용 프로그램 요소의 실행 속성을 확인하십시오.)_

  이 문제를 해결하려면 릴리스 모드를 대신 사용합니다.

- UWP 프로젝트의 루트 폴더에 저장된 Win32 이진은 릴리스에서 제거되어야 합니다. .NET 네이티브 컴파일러가 이를 최종 패키지에서 제거하므로 실행 파일 진입점을 찾을 수 없으면 매니페스트 검사 오류가 발생합니다.

  이 문제를 해결하려면 프로젝트에 하위 폴더를 만들어 win32 바이너리를 저장합니다.


## <a name="you-receive-the-error----msb4018-the-generateresource-task-failed-unexpectedly"></a>'MSB4018 예기치 않게 "GenerateResource" 작업이 실패했습니다.'는 오류가 발생

위성 어셈블리를 PRI(Package Resource Index) 파일로 변환하려 시도할 때 발생할 수 있는 오류입니다.

저희는 이 문제를 인식하고, 장기적인 솔루션을 찾고 있습니다. 임시 해결 방법으로 호스팅 프로젝트 파일의 첫 PropertyGroup 요소에 이 XML 줄을 추가, 리소스 생성기를 끌 수 있습니다.

``<AppxGeneratePrisForPortableLibrariesEnabled>false</AppxGeneratePrisForPortableLibrariesEnabled>``
