---
author: normesta
Description: Run the Desktop Converter App to package a Windows desktop application (like Win32, WPF, and Windows Forms).
Search.Product: eADQiWindows 10XVcnh
title: Desktop App Converter를 사용해 앱을 패키지로 만들기(데스크톱 브리지)
ms.author: normesta
ms.date: 08/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 74c84eb6-4714-4e12-a658-09cb92b576e3
ms.localizationpriority: medium
ms.openlocfilehash: 8748b68bf4efbcc79d0bba475db32f3a2d7cc933
ms.sourcegitcommit: 5dda01da4702cbc49c799c750efe0e430b699502
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/21/2018
ms.locfileid: "4119665"
---
# <a name="package-an-app-using-the-desktop-app-converter-desktop-bridge"></a>데스크톱 앱 변화기를 사용해 앱을 패키지로 만들기(데스크톱 브리지)

[Desktop App Converter 다운로드](https://aka.ms/converter)

Desktop App Converter(DAC)를 사용해 UWP(유니버설 Windows 플랫폼)에 데스크톱 앱을 가져올 수 있습니다. Win32 앱 및 사용자가 .NET 4.6.1을 사용해 생성한 앱이 여기에 포함됩니다.

![DAC 아이콘](images/desktop-to-uwp/dac.png)

'변환기'가 이 도구의 이름으로 표시되지만, 실제 앱을 변환하지 않습니다. 앱은 그대로 유지됩니다. 그러나 이 도구는 패키지 ID와 방대한 WinRT API를 호출하는 기능을 포함하여 Windows 앱 패키지를 생성합니다.

개발 시스템에서 Add-AppxPackage PowerShell cmdlet를 사용하여 해당 패키지를 설치할 수 있습니다.

변환기는 변환기 다운로드의 일부로 제공된 새로운 기본 이미지를 사용하여 격리된 Windows 환경에서 데스크톱 설치 관리자를 실행합니다. 또한 데스크톱 설치 관리자가 수행한 모든 레지스트리 및 파일 시스템 I/O를 캡처하고 출력의 일부로 패키징합니다.

>[!IMPORTANT]
>데스크톱 브리지는 Windows 10 버전, 1607에 도입되었으며 Windows 10 1주년 업데이트(10.0, 빌드 14393) 또는 Visual Studio의 최신 릴리스를 대상으로 하는 프로젝트에만 사용할 수 있습니다.

> [!NOTE]
> Microsoft Virtual Academy가 게시한 짧은 동영상에서 <a href="https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373?l=oZG0B1WhD_8406218965/">이 시리즈</a>를 확인하세요. 이러한 동영상은 Desktop App Converter를 사용하는 몇 가지 일반적인 방법들을 보여 줍니다.

## <a name="the-dac-does-more-than-just-generate-a-package-for-you"></a>패키지 생성 이상을 제공하는 DAC

사용자를 위해 다음과 같이 몇 가지 추가 작업을 수행합니다.

**Windows 10 크리에이터스 업데이트**

:heavy_check_mark: 미리 보기 처리기, 미리 보기 이미지 처리기, 특성 처리기, 방화벽 규칙, URL 플래그를 자동으로 등록합니다.

:heavy_check_mark: 사용자가 파일 탐색기에서 **종류** 열을 사용해 파일을 그룹화할 수 있도록 파일 형식 매핑을 자동으로 등록합니다.

:heavy_check_mark: 공용 COM 서버를 등록합니다.

**Windows 10 1주년 업데이트 이상**

:heavy_check_mark: 앱을 테스트할 수 있도록 패키지를 자동으로 로그인합니다.

:heavy_check_mark: 데스크톱 브리지 및 Microsoft Store 요구 사항에 대해 앱의 유효성을 검사합니다.

옵션의 전체 목록은 이 가이드의 [매개 변수](#command-reference) 섹션을 참조하세요.

패키지 생성 준비가 되었으면 시작해 봅시다.

## <a name="first-prepare-your-application"></a>첫 번째, 응용 프로그램 준비

응용 프로그램에 대한 패키지를 만들기 전에 [앱 패키징을 위한 준비(데스크톱 브리지)](desktop-to-uwp-prepare.md) 가이드를 검토하세요.

## <a name="make-sure-that-your-system-can-run-the-converter"></a>시스템이 변환기를 실행할 수 있는지 확인

시스템이 다음 요구 사항을 충족하는지 확인합니다.

* Windows10 1주년 업데이트(10.0.14393.0 이상) Pro 또는 Enterprise 버전입니다.
* 64비트(x64) 프로세서
* 하드웨어 지원 가상화
* SLAT(Second Level Address Translation)
* [Windows 10용 Windows 소프트웨어 개발 키트(SDK)](https://go.microsoft.com/fwlink/?linkid=821375)를 참조하세요.

## <a name="start-the-desktop-app-converter"></a>Desktop App Converter 시작

1. [Desktop App Converter](https://aka.ms/converter)를 다운로드 및 설치합니다.

2. Desktop App Converter를 관리자 권한으로 실행합니다.

    ![DAC를 관리자 권한으로 실행](images/desktop-to-uwp/run-converter.png)

   콘솔 창이 나타납니다. 해당 콘솔 창을 사용해 명령을 실행하게 됩니다.

## <a name="set-a-few-things-up-apps-with-installers-only"></a>몇 가지 설정 수행(설치 관리자가 있는 앱만 해당)

앱에 설치 관리자가 없는 경우에는 다음 섹션으로 건너 뛸 수 있습니다.

1. 운영 체제의 버전 번호를 확인합니다.

   이를 위해 **실행** 대화 상자에 **winver**를 입력한 후 **확인** 버튼을 선택합니다.

   ![winver](images/desktop-to-uwp/winver.png)

   **Windows 정보** 대화 상자에 Windows 빌드 버전이 표시됩니다.

   ![Windows 10 버전](images/desktop-to-uwp/win-10-version.png)

2. 해당 [Desktop App Converter 기본 이미지](https://aka.ms/converterimages)를 다운로드합니다.

   파일 이름에 표시된 버전 번호가 Windows 빌드의 버전 번호와 일치하는지 확인합니다.

   >[!IMPORTANT]
   > 빌드 번호 **15063**을 사용 중이고 이 빌드의 부 버전이 **.483** 이상인 경우(예: **15063.540**), **BaseImage-15063-UPDATE.wim** 파일을 다운로드해야 합니다. 이 빌드의 부 버전이 **.483** 미만이면 **BaseImage-15063.wim** 파일을 다운로드합니다. 이 기본 파일의 호환되지 않는 버전을 이미 설정한 경우, 수정할 수 있습니다. 이 [블로그 게시물](https://blogs.msdn.microsoft.com/appconsult/2017/08/04/desktop-app-converter-fails-on-windows-10-15063-483-and-later-how-to-solve-it/)에 방법이 설명되어 있습니다.

3. 컴퓨터의 아무 곳에나 다운로드한 파일을 두어도 나중에 찾을 수 있습니다.

4. Desktop App Converter를 시작할 때 나타난 콘솔 창에서 ```Set-ExecutionPolicy bypass``` 명령을 실행합니다.
5. ```DesktopAppConverter.exe -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose``` 명령을 실행하여 변환기를 설정합니다.
6. 컴퓨터를 다시 시작하라는 메시지가 표시되면 작업을 수행합니다.

   변환기가 기본 이미지를 확장할 때 콘솔 창에 상태 메시지가 나타납니다. 상태 메시지가 나타나지 않으면 아무 키나 누릅니다. 이렇게 하면 콘솔 창의 내용이 새로 고침이 될 수 있습니다.

   ![콘솔 창의 상태 메시지](images/desktop-to-uwp/bas-image-setup.png)

   기본 이미지가 완벽하게 확장되면 다음 섹션으로 이동합니다.

## <a name="package-an-app"></a>앱 패키지 만들기

앱을 패키지로 만들려면 Desktop App Converter를 시작할 때 열었던 콘솔 창에서 ``DesktopAppConverter.exe``명령을 실행합니다.  

매개 변수를 사용하여 앱의 패키지 이름, 게시자 및 버전 번호를 지정합니다.

> [!NOTE]
> Microsoft Store에서 앱 이름을 예약한 경우에는 Windows 개발자 센터 대시보드를 사용하여 이름과 게시자를 알 수 있습니다. 다른 시스템에 앱을 사이드로드할 계획이라면 선택한 게시자 이름이 앱 로그인에 사용하는 인증서의 이름과 일치되는 경우에 한해 고유한 이름을 제공할 수 있습니다.

### <a name="a-quick-look-at-command-parameters"></a>명령 매개 변수를 빠르게 확인

다음은 필요한 매개 변수입니다.

```CMD
DesktopAppConverter.exe
-Installer <String>
-Destination <String>
-PackageName <String>
-Publisher <String>
-Version <Version>
```
각각에 대한 자세한 내용은 [여기](#command-reference)를 참조하세요.

### <a name="examples"></a>예

앱을 패키지로 만들 수 있는 몇 가지 일반적인 방법은 다음과 같습니다.

* [설치 관리자(.msi) 파일이 있는 앱을 패키징](#installer-conversion)
* [설정 실행 파일이 있는 앱을 패키징](#setup-conversion)
* [설치 관리자가 없는 앱을 패키징](#no-installer-conversion)
* [앱 패키징, 앱 서명, Microsoft Store 제출 준비](#optional-parameters)

<a id="installer-conversion" />

#### <a name="package-an-app-that-has-an-installer-msi-file"></a>설치 관리자(.msi) 파일이 있는 앱을 패키징

``Installer`` 매개 변수를 사용해 설치 관리자 파일을 가리킵니다.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.msi -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```

> [!IMPORTANT]
> 그러나 여기에서 염두에 두어야 할 중요한 두 가지 사항이 있습니다. 먼저 설치 관리자가 독립 폴더에 위치하고 해당 설치 관리자와 관련된 파일만 같은 폴더에 있는지 확인합니다. 변환기는 격리된 Windows 환경에 해당 폴더의 내용을 모두 복사합니다. <br> 그 다음으로 개발자 센터가 번호로 시작하는 패키지에 ID를 할당하는 경우 <i>-AppId</i> 매개 변수에서도 전달하는지 그리고 그 매개 변수의 값으로 문자열 접미사만 사용하는지 확인합니다.  

**비디오**

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Convert-an-Application-That-Has-an-MSI-Installer-Kh1UU2WhD_7106218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

설치 관리자에 종속 라이브러리 또는 프레임워크의 설치 관리자가 포함된 경우, 약간 다른 방식으로 구성해야 할 수 있습니다. [데스크톱 브리지를 사용하여 여러 설치 관리자 연결](https://blogs.msdn.microsoft.com/appconsult/2017/09/11/chaining-multiple-installers-with-the-desktop-app-converter/)을 참조하세요.

<a id="setup-conversion" />

#### <a name="package-an-app-that-has-a-setup-executable-file"></a>설정 실행 파일이 있는 앱을 패키징

``Installer`` 매개 변수를 사용해 설치 실행 파일을 가리킵니다.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.exe -InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```
>[!IMPORTANT]
>개발자 센터가 번호로 시작하는 패키지에 ID를 할당하는 경우 <i>-AppId</i> 매개 변수에서도 전달하는지 그리고 그 매개 변수의 값으로 문자열 접미사만 사용하는지 확인합니다.

``InstallerArguments`` 매개 변수는 선택적 매개 변수입니다. 그러나 Desktop App Converter를 무인 모드에서 실행하기 위해서는 설치 관리자가 필요하기 때문에 자동 실행을 위해 앱에서 자동 플래그가 필요한 경우에는 이를 사용해야 할 수도 있습니다. ``/S`` 플래그는 매우 일반적인 자동 플래그이지만, 설치 파일을 만드는 데 사용한 설치 관리자 기술에 따라 사용하는 플래그가 달라질 수 있습니다.

**비디오**

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Convert-an-Application-That-Has-a-Setup-exe-Installer-amWit2WhD_5306218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

<a id="no-installer-conversion" />

#### <a name="package-an-app-that-doesnt-have-an-installer"></a>설치 관리자가 없는 앱을 패키징

이 예에서는 앱 파일의 루트 폴더를 가리키기 위해 ``Installer`` 매개 변수를 사용합니다.

앱 실행 파일을 가리키려면 `AppExecutable` 매개 변수를 사용합니다.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyApp\ -AppExecutable MyApp.exe -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```

>[!IMPORTANT]
>개발자 센터가 번호로 시작하는 패키지에 ID를 할당하는 경우 <i>-AppId</i> 매개 변수에서도 전달하는지 그리고 그 매개 변수의 값으로 문자열 접미사만 사용하는지 확인합니다.

**비디오**

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Convert-a-No-Installer-Application-agAXF2WhD_3506218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

<a id="optional-parameters" />

#### <a name="package-an-app-sign-the-app-and-run-validation-checks-on-the-package"></a>앱 패키징, 앱 서명, 패키지에서 유효성 검사 실행

이 예는 로컬 테스트를 위해 앱에 로그인한 다음, 데스크톱 브리지 및 Microsoft Store 요구 사항에 대한 앱의 유효성을 검사할 수 있는 방법을 보여준다는 점을 제외하고 첫 번째 경우와 비슷합니다.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.exe -InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1 -MakeAppx -Sign -Verbose -Verify
```
>[!IMPORTANT]
>개발자 센터가 번호로 시작하는 패키지에 ID를 할당하는 경우 <i>-AppId</i> 매개 변수에서도 전달하는지 그리고 그 매개 변수의 값으로 문자열 접미사만 사용하는지 확인합니다.

``Sign`` 매개 변수는 인증서를 생성하고 이를 통해 앱에 로그인합니다. 앱을 실행하려면 생성된 인증서를 설치해야 합니다. 자세한 내용은 이 가이드의 [패키지 앱 실행](#run-app) 섹션을 참조하세요.

``Verify``매개 변수를 사용하여 앱의 유효성을 검사할 수 있습니다.

### <a name="a-quick-look-at-optional-parameters"></a>선택적 매개 변수를 빠르게 확인

``Sign``및 ``Verify`` 매개 변수는 선택적입니다. 이 외에도 많은 선택적 매개 변수가 있습니다.  자주 사용되는 선택적 매개 변수로는 다음과 같은 것들이 있습니다.

```CMD
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
전체 목록은 다음 섹션을 참조하세요.

<a id="command-reference" />

### <a name="parameter-reference"></a>매개 변수 참조

다음은 Desktop App Converter를 실행할 때 사용할 수 있는 매개 변수의 전체 목록(범주별로 구성)입니다.

* [설치](#setup-params)
* [변환](#conversion-params)
* [패키지 ID](#identity-params)
* [패키지 매니페스트](#manifest-params)
* [정리](#cleanup-params)
* [아키텍처](#architecture-params)
* [기타](#other-params)

앱 콘솔 창에서 ``Get-Help``명령을 실행하여 전체 목록을 볼 수도 있습니다.   

||||
|-------------|-----------|-------------|
|<a id="setup-params" /> <strong>설치 매개 변수</strong>  ||
|-설치 [&lt;SwitchParameter&gt;] |필수 |설치 모드에서 DesktopAppConverter를 실행합니다. 설치 모드는 제공된 기본 이미지 확장을 지원합니다.|
|-BaseImage &lt;String&gt; | 필수 |확장되지 않은 기본 이미지에 대한 전체 경로. 이 매개 변수는 -Setup을 지정한 경우에 필요합니다.|
| -LogFile &lt;String&gt; |선택 사항 |로그 파일을 지정합니다. 생략하면 로그 파일의 임시 위치가 생성됩니다.|
|-NatSubnetPrefix &lt;String&gt; |선택 사항 |Nat 인스턴스에 사용할 접두사 값입니다. 일반적으로 사용자는 호스트 컴퓨터가 변환기의 NetNat과 동일한 서브넷 범위에 연결되는 경우에만 이 값을 변경하려고 합니다. **Get NetNat** cmdlet을 사용하여 현재 변환기 NetNat 구성을 쿼리할 수 있습니다. |
|-NoRestart [&lt;SwitchParameter&gt;] |필수 |설치 프로그램을 실행할 때 다시 부팅하라는 메시지를 표시하지 않습니다(컨테이너 기능을 사용하도록 설정하려면 다시 부팅해야 함). |
|<a id="conversion-params" /> <strong>변환 매개 변수</strong>|||
|-AppInstallPath &lt;String&gt;  |선택 사항 |응용 프로그램이 설치된 경우 설치 파일에 대한 응용 프로그램 루트 폴더의 전체 경로입니다(예: "C:\Program Files (x86)\MyApp").|
|-Destination &lt;String&gt; |필수 |원하는 변환기 appx 출력 대상으로, DesktopAppConverter는 이 위치가 없으면 만들 수 있습니다.|
|-Installer &lt;String&gt; |필수 |응용 프로그램의 설치 관리자 경로로, 무인/자동으로 실행할 수 있어야 합니다. 설치 관리자 없이 변환할 경우 이는 앱 파일의 루트 디렉터리에 대한 경로입니다. |
|-InstallerArguments &lt;String&gt; |선택 사항 |강제로 설치 관리자를 무인/자동으로 실행할 수 있도록 하기 위한 쉼표로 구분된 인수 목록 또는 인수 문자열입니다. 설치 관리자가 msi인 경우 이 매개 변수는 선택적입니다. 설치 관리자에서 로그를 가져오려면 여기에 설치 관리자에 대한 로깅 인수를 제공하고, 변환기가 적절한 경로로 대체하는 토큰에 해당하는 &lt;log_folder&gt; 경로를 사용합니다. <br><br>**참고**: 무인/자동 플래그와 로그 인수는 설치 관리자 기술마다 다릅니다. <br><br>이 매개 변수의 사용 예제는 -InstallerArguments "/silent /log &lt;log_folder&gt;\install.log"를, 로그 파일을 생성하지 않는 또 다른 예제는 ```-InstallerArguments "/quiet", "/norestart"```를 참조하세요. 변환기가 로그를 캡처한 후 최종 로그 폴더에 추가하게 하려면 토큰 경로 &lt;log_folder&gt;로 모든 로그를 보내야 합니다.|
|-InstallerValidExitCodes &lt;Int32&gt; |선택 사항 |설치 관리자가 성공적으로 실행되었음을 나타내는 쉼표로 구분된 종료 코드 목록(예: 0, 1234, 5678).  기본적으로 msi가 아닌 경우는 0이고 msi인 경우는 0, 1641, 3010입니다.|
|-MakeAppx [&lt;SwitchParameter&gt;]  |선택 사항 |스위치(있는 경우)는 출력에 대해 MakeAppx를 호출하도록 이 스크립트에 지시합니다. |
|-MakeMSIX [&lt;SwitchParameter&gt;]  |선택 사항 |있는 경우에이 스크립트 MSIX 패키지로 출력 패키지를 통해 하는 스위치입니다. |
|<a id="identity-params" /><strong>패키지 ID 매개 변수</strong>||
|-PackageName &lt;String&gt; |필수 |유니버설 Windows 앱 패키지의 이름. 개발자 센터가 번호로 시작하는 패키지에 ID를 할당하는 경우 <i>-AppId</i> 매개 변수에서도 전달하는지 그리고 그 매개 변수의 값으로 문자열 접미사만 사용하는지 확인합니다. |
|-Publisher &lt;String&gt; |필수 |유니버설 Windows 앱 패키지의 게시자 |
|-Version &lt;Version&gt; |필수 |유니버설 Windows 앱 패키지의 버전 번호 |
|<a id="manifest-params" /><strong>패키지 매니페스트 매개 변수</strong>||
|-AppExecutable &lt;String&gt; |선택 사항 |응용 프로그램의 주 실행 파일(예: "MyApp.exe")의 이름입니다. 이 매개 변수는 설치 관리자 없이 변환할 경우 필요합니다. |
|-AppFileTypes &lt;String&gt;|선택 사항 |응용 프로그램과 연결될 쉼표로 구분된 파일 형식 목록 예제: -AppFileTypes "'.md', '.markdown'".|
|-AppId &lt;String&gt; |선택 사항 |응용 프로그램 ID를 Windows 앱 패키지 매니페스트로 설정하기 위한 값을 지정합니다. 값을 지정하지 않으면 *PackageName*에 대해 제공한 값으로 설정됩니다. 대부분의 경우에는 *PackageName*을 사용해도 괜찮습니다. 그러나 개발자 센터가 번호로 시작하는 패키지에 ID를 할당하는 경우 <i>-AppId</i> 매개 변수에서도 전달하는지 그리고 그 매개 변수의 값으로 문자열 접미사만 사용하는지 확인합니다. |
|-AppDisplayName &lt;String&gt;  |선택 사항 |응용 프로그램 표시 이름을 Windows 앱 패키지 매니페스트로 설정하기 위한 값을 지정합니다. 값을 지정하지 않으면 *PackageName*에 대해 제공한 값으로 설정됩니다. |
|-AppDescription &lt;String&gt; |선택 사항 |응용 프로그램 설명을 Windows 앱 패키지 매니페스트로 설정하기 위한 값을 지정합니다. 값을 지정하지 않으면 *PackageName*에 대해 제공한 값으로 설정됩니다.|
|-PackageDisplayName &lt;String&gt; |선택 사항 |패키지 표시 이름을 Windows 앱 패키지 매니페스트로 설정하기 위한 값을 지정합니다. 값을 지정하지 않으면 *PackageName*에 대해 제공한 값으로 설정됩니다. |
|-PackagePublisherDisplayName &lt;String&gt; |선택 사항 |패키지 게시자 표시 이름을 Windows 앱 패키지 매니페스트로 설정하기 위한 값을 지정합니다. 값을 지정하지 않으면 *Publisher*에 대해 제공한 값으로 설정됩니다. |
|<a id="cleanup-params" /><strong>정리 매개 변수</strong>|||
|-정리 [&lt;옵션&gt;] |필수 |DesktopAppConverter 아티팩트에 대한 정리를 실행합니다. 정리 모드에는 3가지 유효한 옵션이 있습니다. |
|-모두 정리 | |확장된 기본 이미지를 모두 삭제하고, 임시 변환기 파일을 제거하고, 컨테이너 네트워크를 제거하고, 선택적 Windows 기능인 컨테이너를 사용하지 않도록 설정합니다. |
|- 작업 디렉터리 정리 |필수 |모든 임시 변환기 파일을 제거합니다. |
|- ExpandedImage 정리 |필수 |호스트 컴퓨터에 설치된 확장된 기본 이미지를 모두 삭제합니다. |
|<a id="architecture-params" /><strong>패키지 아키텍처 매개 변수</strong>|||
|-PackageArch &lt;String&gt; |필수 |지정된 아키텍처를 사용하여 패키지를 생성합니다. 유효한 옵션은 'x86' 또는 'x64'입니다(예: -PackageArch x86). 이 매개 변수는 선택 사항입니다. 지정하지 않으면 DesktopAppConverter는 패키지 아키텍처를 자동으로 검색하려고 합니다. 자동 검색에 실패하면 기본적으로 x64 패키지로 설정합니다. |
|<a id="other-params" /><strong>기타 매개 변수</strong>|||
|-ExpandedBaseImage &lt;String&gt;  |선택 사항 |이미 확장된 기본 이미지에 대한 전체 경로.|
|-LogFile &lt;String&gt;  |선택 사항 |로그 파일을 지정합니다. 생략하면 로그 파일의 임시 위치가 생성됩니다. |
| -[&lt;SwitchParameter&gt;] 로그인 |선택 사항 |테스트용으로 생성된 인증서를 사용해 출력 Windows 앱 패키지를 로그인하라고 이 스크립트에 명령합니다. 이 스위치는 ```-MakeAppx``` 스위치와 함께 있어야 합니다. |
|&lt;일반 매개 변수&gt; |필수 |이 cmdlet은 *Verbose*, *Debug*, *ErrorAction*, *ErrorVariable*, *WarningAction*, *WarningVariable*, *OutBuffer*, *PipelineVariable*, *OutVariable* 등의 일반 매개 변수를 지원합니다. 자세한 내용은 [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216)를 참조하세요. |
| - [&lt;SwitchParameter&gt;] 확인 |선택 사항 |스위치가 있을 경우, DAC를 통해 앱 패키지가 데스크톱 브리지 및 Microsoft Store의 요구 사항에 맞는지 확인합니다. 결과는 "VerifyReport.xml"이라는 유효성 검사 보고서로 출력되며, 브라우저에서 가장 잘 보입니다. 이 스위치는 `-MakeAppx` 스위치와 함께 있어야 합니다. |
|-PublishComRegistrations| 선택 사항| 설치 관리자가 변경한 모든 공개 COM 등록을 검사하고 매니페스트에 유효한 것을 게시합니다. 이러한 등록을 다른 응용 프로그램에 대해 사용할 수 있도록 하고 싶은 경우에만 이 플래그를 사용합니다. 이러한 등록을 응용 프로그램에서만 사용할 경우에는 이 플래그를 사용할 필요가 없습니다. <br><br>앱 패키징 이후에 COM 등록이 예상대로 작동하도록 하는 방법은 [이 문서](https://blogs.windows.com/buildingapps/2017/04/13/com-server-ole-document-support-desktop-bridge/#lDg5gSFxJ2TDlpC6.97)를 참조하세요.

<a id="run-app" />

## <a name="run-the-packaged-app"></a>패키지로 만든 앱 실행

앱을 실행하는 방법은 두 가지입니다.

PowerShell 명령 프롬프트를 열어서 ```Add-AppxPackage –Register AppxManifest.xml``` 명령을 입력하는 것이 그 하나입니다. 로그인이 필요하지 않다는 점에서 앱을 실행하는 가장 쉬운 방법일 것입니다.

또 다른 방법은 인증서를 사용해 앱에 로그인하는 것입니다. ```sign```매개 변수를 사용하는 경우에는 Desktop App Converter가 사용자를 위한 인증서를 생성하고 이를 이용해 앱에 로그인을 합니다. 해당 파일의 이름은 **auto-generated.cer**이며, 패키징된 앱의 루트 폴더에서 찾을 수 있습니다.

몇 가지 단계에 따라 생성된 인증서를 설치하고 앱을 실행합니다.

1. **auto-generated.cer** 파일을 두 번 클릭하여 인증서를 설치합니다.

   ![생성된 인증서 파일](images/desktop-to-uwp/generated-cert-file.png)

   > [!NOTE]
   > 암호를 입력하라는 메시지가 표시되면 기본 암호 "123456"를 사용합니다.

2. **인증서** 대화 상자에서 **인증서 설치** 버튼을 선택합니다.
3. **인증서 가져오기 마법사**에서 **로컬 컴퓨터**에 인증서를 설치하고 이를 **신뢰할 수 있는 사용자** 인증서 저장소에 배치합니다.

   ![신뢰할 수 있는 사용자 저장소](images/desktop-to-uwp/trusted-people-store.png)

5. 패키지로 만든 앱의 루트 폴더에서 Windows 앱 패키지 파일을 두 번 클릭합니다.

   ![Windows 앱 패키지 파일](images/desktop-to-uwp/windows-app-package.png)

6. **설치** 단추를 선택하여 앱을 설치합니다.

   ![설치 단추](images/desktop-to-uwp/install.png)


## <a name="modify-the-packaged-app"></a>패키지 앱을 수정

패키지로 만든 앱을 변경하여 버그를 해결하거나, 시각적 자산을 추가하거나, 라이브 타일 같은 최신 기능을 통해 앱을 개선할 수 있습니다.

변경을 한 후에는 변환기를 다시 실행할 필요가 없습니다. 대부분의 경우 MakeAppx 도구와 DAC가 앱에 대해 생성한 appxmanifest.xml 파일을 사용해 앱을 쉽게 다시 패키징할 수 있습니다. [Windows 앱 패키지 생성](desktop-to-uwp-manual-conversion.md#make-appx)을 참조하세요.

* 앱의 시각적 자산을 수정하는 경우, 새 패키지 리소스 인덱스 파일을 생성한 다음 MakeAppx 도구를 실행하여 새 패키지를 생성합니다. [PRI(Package Resource Index) 파일 생성](desktop-to-uwp-manual-conversion.md#make-pri)을 참조하세요.

* Windows 작업 표시줄, 작업 보기, LT + TAB, 끌기 도우미 및 시작 타일의 오른쪽 아래 모서리에 표시되는 아이콘이나 타일을 추가하려면 [(선택 사항)대상 기반의 판이 없는 자산을 추가](desktop-to-uwp-manual-conversion.md#target-based-assets)를 참조하세요.

> [!NOTE]
> 설치 관리자가 만든 레지스트리 설정을 변경하는 경우에는 Desktop App Converter를 다시 실행하여 변경 내용을 반영해야 합니다.

**비디오**

|출력을 수정하고 다시 패키징 |데모: 출력을 수정하고 다시 패키징|
|---|---|
|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Video-Modifying-and-Repackaging-Output-from-Desktop-App-Converter-OwpAJ3WhD_6706218965" width="426" height="472" allowFullScreen frameBorder="0"></iframe>|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Modify-Output-from-Desktop-App-Converter-gEnsa3WhD_8606218965" width="426" height="472" allowFullScreen frameBorder="0"></iframe>|

다음 두 섹션에는 패키지로 만든 앱에 대해 고려해 볼 수 있는 몇 가지 "수정" 옵션이 나와 있습니다.

### <a name="delete-unnecessary-files-and-registry-keys"></a>불필요한 파일 및 레지스트리 키 삭제

Desktop App Converter는 컨테이너에서 파일과 시스템 노이즈를 걸러낼 때 매우 보수적으로 접근합니다.

원하는 경우에 VFS 폴더를 검토하고 설치 관리자에서 필요하지 않은 파일을 삭제할 수 있습니다.  또한 Reg.dat의 내용을 검토하고 앱에서 설치하지 않았거나 앱에 필요하지 않은 키가 있으면 삭제합니다.

### <a name="fix-corrupted-pe-headers"></a>손상된 PE 헤더 수정

변환 프로세스 동안 DesktopAppConverter는 모든 손상된 PE 헤더를 수정하기 위해 자동으로 PEHeaderCertFixTool을 실행합니다. 하지만 UWP Windows 앱 패키지, 느슨한 파일 또는 특정 이진에서 PEHeaderCertFixTool을 실행할 수도 있습니다. 예를 들면 다음과 같습니다.

```CMD
PEHeaderCertFixTool.exe <binary file>|<.appx package>|<folder> [/c] [/v]
 /c   -- check for corrupted certificate but do not fix (optional)
 /v   -- verbose (optional)
example1: PEHeaderCertFixTool app.exe
example2: PEHeaderCertFixTool c:\package.appx /c
example3: PEHeaderCertFixTool c:\myapp /c /v
```

## <a name="telemetry-from-desktop-app-converter"></a>Desktop App Converter의 원격 분석

Desktop App Converter가 사용자 및 사용자의 소프트웨어 사용에 대한 정보를 수집한 후 Microsoft로 보낼 수 있습니다. 제품 설명서 및 [Microsoft 개인 정보 취급 방침](http://go.microsoft.com/fwlink/?LinkId=521839)에서Microsoft의 데이터 수집 및 사용에 대해 알아볼 수 있습니다. Microsoft 개인 정보 취급 방침의 모든 규정을 준수한다는 데 동의합니다.

기본적으로 Desktop App Converter에 대한 원격 분석은 사용되도록 설정됩니다. 원격 분석을 원하는 설정으로 구성하려면 다음 레지스트리 키를 추가합니다.  

```cmd
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DesktopAppConverter
```
+ 1로 설정된 DWORD를 사용하여 *DisableTelemetry* 값을 추가 또는 편집합니다.
+ 원격 분석을 사용하려면 이 키를 제거하거나 값을 0으로 설정합니다.

### <a name="language-support"></a>언어 지원

Desktop App Converter는 유니코드를 지원하지 않으므로 중국어 문자나 ASCII가 아닌 문자는 도구에 사용할 수 없습니다.

## <a name="next-steps"></a>다음 단계

**질문에 대한 답변 찾기**

질문이 있으세요? Stack Overflow에서 질문해 주세요. 저희 팀은 이러한 [태그](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다. [여기](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)에서 Microsoft에 문의할 수도 있습니다.

알려진 문제의 [이](desktop-to-uwp-known-issues.md#app-converter) 목록을 참조할 수도 있습니다.

**피드백 제공 또는 기능 제안**

[UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)를 참조하세요.

**앱 실행 / 문제 찾기 및 해결**

[패키지 데스크톱 앱 실행, 디버그, 테스트(데스크톱 브리지)](desktop-to-uwp-debug.md)를 참조하세요.

**앱 배포**

[패키지 데스크톱 앱 배포(데스크톱 브리지)](desktop-to-uwp-distribute.md)를 참조하세요.
