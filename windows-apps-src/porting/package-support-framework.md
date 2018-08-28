---
author: normesta
Description: Fix issues that prevent your desktop application from running in an MSIX container
Search.Product: eADQiWindows 10XVcnh
title: MSIX 컨테이너에서 실행 중인에서 데스크톱 응용 프로그램을 방해 하는 문제 수정
ms.author: normesta
ms.date: 07/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 46d5705233af9e8254b9ac89a2d6e9891e90701f
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2018
ms.locfileid: "2888222"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>MSIX 패키지에 패키지 지원 프레임 워크를 사용 하 여 런타임 픽스를 적용

패키지 지원 프레임 워크는 오픈 소스 키트 (영문) MSIX 컨테이너에서 실행할 수 있도록 소스 코드에 액세스할 수 없을 때 픽스가 기존의 win32 응용 프로그램에 적용 하는데 도움이 되는 경우 패키지 지원 프레임 워크 현대 런타임 환경의 모범 사례에 따라 응용 프로그램 도움이 됩니다.

패키지 지원 프레임 워크를 만들려면 우리는 Microsoft Research (MSR) 하 여 개발 된 오픈소스 프레임 워크 및 API 리디렉션 및 연결을 사용할 때 도움이 되는 [자유롭게](https://www.microsoft.com/en-us/research/project/detours) 기술을 활용 했습니다.

이 프레임 워크는 경량, 오픈 소스 하 고 사용 하는 응용 프로그램 문제를 해결 하기 신속 하 게 합니다. 또한, 세계의 커뮤니티와 상의 하 고 다른 사용자의 투자를 기반으로 구축 기회를 제공 합니다.

## <a name="a-quick-look-inside-of-the-package-support-framework"></a>패키지 지원 프레임 워크 내의 빠른 보기

패키지 지원 프레임 워크는 실행 파일, 런타임 관리자 DLL 및 런타임 픽스 집합이 들어 있습니다.

![패키지 지원 프레임 워크](images/desktop-to-uwp/package-support-framework.png)

작동 방식은 다음과 같습니다. 응용 프로그램에 적용 하려는 fix(s)를 지정 하는 구성 파일을 만들 수 있습니다. 그런 다음 shim 시작 관리자 실행 파일을 가리키도록 패키지를 수정할 수 있습니다.

사용자가 응용 프로그램을 시작 하는 경우 shim 시작 관리자는을 실행 하는 첫번째 실행 파일입니다. 구성 파일을 읽고 응용 프로그램 프로세스에 런타임 fix(s) 및 런타임 관리자 DLL 삽입 합니다.

![패키지 지원 프레임 워크 DLL 주입](images/desktop-to-uwp/package-support-framework-2.png)

런타임 관리자 응용 프로그램이 MSIX 컨테이너 환경에서 실행 되기 위해 필요할 때 수정 프로그램을 적용 합니다.

이 가이드는 응용 프로그램 호환성 문제를 식별 하는데 도움이 됩니다 하 고, 적용을 찾아 런타임 확장을 해결 하는 수정 합니다.

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>패키지 된 응용 프로그램 호환성 문제를 식별 합니다.

먼저 응용 프로그램에 대 한 패키지를 만듭니다. 다음을 설치 하 고 실행 한 다음 해당 동작을 확인 합니다. 호환성 문제를 식별 하는데 도움이 되는 오류 메시지를 받을 수 있습니다. [프로세스 모니터](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) 를 사용 하 여 문제를 식별 수도 있습니다.  일반적인 문제 프로그램 경로 권한과 작업 디렉터리에 대 한 응용 프로그램 가정와 관련이 있습니다.

### <a name="using-process-monitor-to-identify-an-issue"></a>프로세스 모니터를 사용 하 여 문제를 식별 하려면

[프로세스 모니터](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) 는 응용 프로그램의 파일 및 레지스트리 작업과 결과로 관찰 하기 위한 강력한 유틸리티입니다.  이 응용 프로그램 호환성 문제를 이해 하는데 도움이 됩니다.  프로세스 모니터를 연 후 필터 추가 (필터 > 필터...) 응용 프로그램 실행 파일에서 이벤트에만 포함 하도록 합니다.

![ProcMon App 필터](images/desktop-to-uwp/procmon_app_filter.png)

이벤트의 목록이 표시 됩니다. 여러 이러한 이벤트에 대 한 단어 **성공** 에 나타나는 **결과** 열.

![ProcMon 이벤트](images/desktop-to-uwp/procmon_events.png)

필요에 따라만 오류를 표시 하는 이벤트를 필터링 할 수 있습니다.

![ProcMon 제외 성공](images/desktop-to-uwp/procmon_exclude_success.png)

파일 시스템 액세스 오류, 의심 되는 경우 System32/SysWOW64 또는 패키지 파일 경로 아래에 있는 오류가 발생 한 이벤트를 검색 합니다. 필터 수도 여기서도 도움이 됩니다. 이 목록의 맨 아래에 시작 하 고 위쪽으로 스크롤하십시오. 이 목록의 맨 아래에 표시 되는 오류는 가장 최근에 발생 했습니다. "경로/이름을 찾을 수 없음", 및 "액세스 거부"와 같은 문자열을 포함 하는 오류에 대 한 대부분 주의 하 고 의심 스러운 보이지 않을 것을 무시 합니다. [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/) 에 두 문제가 있습니다. 다음 그림에 표시 되는 목록에서 해당 문제를 볼 수 있습니다.

![ProcMon Config.txt](images/desktop-to-uwp/procmon_config_txt.png)

이 이미지에 표시 되는 첫번째 문제에 응용 프로그램은 실패 하 여 "C:\Windows\SysWOW64" 경로에 있는 "Config.txt" 파일을 읽을 수 있습니다. 응용 프로그램은 해당 경로 직접 참조 하려고 할 경우 대부분의 경우, 상대 경로 사용 하 여 해당 파일에서 읽는 하려는 이며 기본적으로 "System32/SysWOW64" 응용 프로그램의 작업 디렉터리입니다. 이 응용 프로그램 패키지에 포함으로 설정 해야 해당 현재 작업 디렉터리를 예상는 것이 좋습니다. appx 내의 찾고, 했 파일이 실행 파일와 동일한 디렉터리에 있는지를 확인할 수 있습니다.

![응용 프로그램 Config.txt](images/desktop-to-uwp/psfsampleapp_config_txt.png)

두번째 문제는 다음 이미지에 나타납니다.

![ProcMon 로그 파일](images/desktop-to-uwp/procmon_logfile.png)

이 문제에 응용 프로그램은 실패 하 여 해당 패키지 경로를.log 파일을 쓸 수 있습니다. 이 것이 좋습니다 파일 리디렉션 shim 도움이 될 수 있습니다.

<a id="find" />

## <a name="find-a-runtime-fix"></a>런타임 수정 사항 찾기

PSF 파일 리디렉션 shim 등 지금 바로 사용할 수 있는 런타임 픽스를 포함 합니다.

### <a name="file-redirection-shim"></a>리디렉션 Shim 파일

메모를 작성 하거나 MSIX 컨테이너에서 실행 되는 응용 프로그램에서 액세스할 수 없는 디렉터리에 데이터를 읽는 시도 리디렉션하려면 [리디렉션을 Shim 파일을](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim) 사용할 수 있습니다.

예, 응용 프로그램을 실행 가능한 응용 프로그램와 동일한 디렉터리에 있는 로그 파일에 기록 하는 경우 [파일 리디렉션 Shim](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim) 를 사용 하 여 로컬 응용 프로그램 데이터 저장소와 같이 다른 위치에 해당 로그 파일을 만들 수 있습니다.

### <a name="runtime-fixes-from-the-community"></a>커뮤니티에서 런타임 수정 사항

커뮤니티 기여도 사용해 [GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop) 페이지를 검토 했는지 확인 합니다. 다른 개발자가 유사한 문제를 해결 한 및 런타임 수정 프로그램을 공유 하는 것 같습니다.

## <a name="apply-a-runtime-fix"></a>런타임 수정 프로그램을 적용 합니다.

Windows SDK에서 하 고 다음이 단계를 수행 하 여 몇가지 간단한 도구와 기존 런타임 수정 프로그램을 적용할 수 있습니다.

> [!div class="checklist"]
> * 패키지 레이아웃 폴더 만들기
> * 패키지 지원 프레임 워크 파일 가져오기
> * 패키지에 추가합니다
> * 패키지 매니페스트 수정
> * 구성 파일 만들기

각 작업을 통해 보겠습니다.

### <a name="create-the-package-layout-folder"></a>패키지 레이아웃 폴더 만들기

이미.appx 파일을 설치한 경우 나중에 프로그램 패키지에 대 한 준비 영역으로 역할을 레이아웃 폴더에 해당 내용을 압축을 풉니다 수 있습니다.  이 수행할 수는 **x64 VS 2017에 대 한 기본 도구 명령 프롬프트**, 또는 실행 파일 검색 경로에서 SDK bin 경로 사용 하 여 수동으로 합니다.

```
makeappx unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.appx /d PackageContents

```

이렇게 하면 된 항목은 다음과 같습니다.

![패키지 레이아웃](images/desktop-to-uwp/package_contents.png)

.Appx 파일을 시작 하 여 설치 하지 않은 경우 처음부터 패키지 폴더 및 파일을 만들 수 있습니다.

### <a name="get-the-package-support-framework-files"></a>패키지 지원 프레임 워크 파일 가져오기

Visual Studio를 사용 하 여 PSF Nuget 패키지를 얻을 수 있습니다. 독립 실행형 Nuget 명령줄 도구를 사용 하 여 얻을 수 있습니다.

#### <a name="get-the-package-by-using-visual-studio"></a>Visual Studio를 사용 하 여 패키지 가져오기

Visual Studio에서 사용자 솔루션 또는 프로젝트 노드를 마우스 오른쪽 단추로 클릭 하 고 Nuget 패키지 관리 명령 중 하나를 선택 합니다.  검색할 **Microsoft.PackageSupportFramework** 또는 **PSF** Nuget.org에서 패키지를 찾을 수 있습니다. 다음을 설치 합니다.

#### <a name="get-the-package-by-using-the-command-line-tool"></a>명령줄 도구를 사용 하 여 패키지 가져오기

이 위치의 Nuget 명령줄 도구를 설치: https://www.nuget.org/downloads합니다. 그런 다음, Nuget 명령줄에서이 명령을 실행 합니다.

```
nuget install Microsoft.PackageSupportFramework
```

### <a name="add-the-package-support-framework-files-to-your-package"></a>패키지에 패키지 지원 프레임 워크 파일 추가

패키지 디렉터리에 필요한 32 비트 및 64 비트 PSF Dll 및 실행 파일을 추가 합니다. 다음 표를 가이드로 따르세요. 또한 필요한 모든 런타임 수정 프로그램을 포함 합니다. 이 예제에서는 파일 리디렉션 런타임 수정을 해야합니다.

| 응용 프로그램 실행 파일은 x64 | 응용 프로그램 실행 파일은 x86 |
|-------------------------------|-----------|
| [ShimLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |  [ShimLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |
| [ShimRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) | [ShimRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) |
| [ShimRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) | [ShimRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) |

패키지 콘텐츠 형식은 다음과 같습니다.

![패키지 이진 파일](images/desktop-to-uwp/package_binaries.png)

### <a name="modify-the-package-manifest"></a>패키지 매니페스트 수정

텍스트 편집기에서 패키지 매니페스트를 열고 다음 설정의 `Executable` 의 특성은 `Application` 요소 shim 시작 관리자 실행 파일의 이름입니다.  대상 응용 프로그램의 아키텍처를 알고 있는 경우 적절 한 버전 ShimLauncher32.exe 또는 ShimLauncher64.exe를 선택 합니다.  그렇지 않은 경우에 ShimLauncher32.exe 경우에서 모두 작동 합니다.  예를 들면 다음과 같습니다.

```xml
<Package ...>
  ...
  <Applications>
    <Application Id="PSFSample"
                 Executable="ShimLauncher32.exe"
                 EntryPoint="Windows.FullTrustApplication">
      ...
    </Application>
  </Applications>
</Package>
```

### <a name="create-a-configuration-file"></a>구성 파일 만들기

파일 이름 만들기 ``config.json``, 패키지의 루트 폴더에 해당 파일을 저장 합니다. 방금 대체 하는 실행 파일을 가리키도록 config.json 파일의 선언 된 응용 프로그램 ID를 수정 합니다. 프로세스 모니터를 사용 하 여에서 얻은 정보를 사용 하 여 있습니다 수도 작업 디렉터리를 설정으로 패키지 상대 "PSFSampleApp" 디렉터리 아래에 있는.log 파일을 읽기/쓰기를 리디렉션하려면 파일 리디렉션 shim을 사용 합니다.

```json
{
    "applications": [
        {
            "id": "PSFSample",
            "executable": "PSFSampleApp/PSFSample.exe",
            "workingDirectory": "PSFSampleApp/"
        }
    ],
    "processes": [
        {
            "executable": "PSFSample",
            "shims": [
                {
                    "dll": "FileRedirectionShim.dll",
                    "config": {
                        "redirectedPaths": {
                            "packageRelative": [
                                {
                                    "base": "PSFSampleApp/",
                                    "patterns": [
                                        ".*\\.log"
                                    ]
                                }
                            ]
                        }
                    }
                }
            ]
        }
    ]
}
```
다음은 config.json 스키마에 대 한 가이드입니다.

| 배열 | key | 값 |
|-------|-----------|-------|
| 응용 프로그램 | id |  값을 사용 하는 `Id` 의 특성은 `Application` 패키지 매니페스트에서 요소. |
| 응용 프로그램 | 실행 파일 | 패키지 상대 경로 시작 하려면 실행 파일입니다. 대부분의 경우 수정 하기 전에이 값 패키지 매니페스트 파일에서 얻을 수 있습니다. 값이는 `Executable` 의 특성은 `Application` 요소입니다. |
| 응용 프로그램 | workingDirectory | (선택 사항) 시작 하는 응용 프로그램의 작업 디렉터리도 사용 하 여 패키지 상대 경로입니다. 운영 체제를 사용 하 여이 값을 설정 하지 않으면 하는 경우는 `System32` 응용 프로그램의 작업 디렉터리와 디렉터리입니다. |
| 프로세스 | 실행 파일 | 대부분의 경우에서이 속성은 이름을 `executable` 제거 경로 및 파일 확장명을 가진 위에 구성 합니다. |
| shim | dll | 패키지 상대 경로를 로드할 shim.appx입니다. |
| shim | 구성 | (선택 사항) Shim dl 작동 하는 방식을 제어 합니다. 이 값의 정확한 형식으로 각 shim 수로 해석 하 여이 "blob" 브로드캐스트하며 shim 하 여 shim 별로 달라 집니다. |

`applications`, `processes`, 및 `shims` 키는 배열 합니다. 즉, 둘 이상의 응용 프로그램, 프로세스 및 shim DLL 지정 하려면 config.json 파일을 사용할 수 있습니다.


### <a name="package-and-test-the-app"></a>패키지 및 응용 프로그램 테스트

그런 다음 패키지를 만듭니다.

```
makeappx pack /d PackageContents /p PSFSamplePackageFixup.appx
```

그런 다음, 로그인 합니다.

```
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.appx
```

자세한 내용은 [서명 인증서 패키지를 만드는 방법](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate) 및 [signtool를 사용 하 여 패키지에 서명 하는 방법](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool) 을 참조 하십시오.

PowerShell을 사용 하 여 패키지를 설치 합니다.

>[!NOTE]
> 패키지를 먼저 제거 해야 합니다.

```
powershell Add-AppxPackage .\PSFSamplePackageFixup.appx
```

응용 프로그램을 실행 하 고 적용 하는 런타임 수정 프로그램의 동작을 확인 합니다.  진단 하 고 필요에 따라 패키지 단계를 반복 합니다.

### <a name="use-the-trace-shim"></a>추적 Shim을 사용 하 여

패키지 된 응용 프로그램 호환성 문제를 진단 하는 대체 기술을 추적 Shim을 사용 하는 것입니다. 이 DLL은 PSF에 포함 된 하 고 응용 프로그램의 동작을 프로세스 모니터 비슷합니다의 상세 진단 보기를 제공 합니다.  응용 프로그램 호환성 문제를 표시 하도록 특수 하 게 합니다.  해제 하려면 추적 Shim을 사용, DLL 패키지를 추가, 다음 조각을 대화 config.json에 추가 하 고 다음 패키지 응용 프로그램을 설치 합니다.

```json
{
    "dll": "TraceShim.dll",
    "config": {
        "traceLevels": {
            "filesystem": "allFailures"
        }
    }
}
```

기본적으로 추적 Shim "예상" 것으로 간주 될 수 있는 오류를 필터링 합니다.  예, 응용 프로그램 조건에 관계 없이 이미 존재 하는지, 결과 무시 하 고 확인 하지 않고 파일을 삭제 하려고 할 수 있습니다. 이 설정은 위의 예제에서는 파일 시스템 함수에서 모든 오류를 수신 거부는 하므로 일부 예기치 않은 오류 필터링 얻을 수 있는 맬웨어 결과. 그 전에 Config.txt 파일에서 읽기를 시도 실패 하 고는 메시지 "파일을 찾지"에서 때문에이 작업을 수행 합니다. 자주 관찰 되 고 일반적으로 하지 예상 없는 것으로 간주 되는 오류입니다. 실제로 하는 것 가능성이 가장 예기치 않은 오류에만 필터링 하 고 다음으로 대체 하기 모든 오류는 문제가 여전히 식별할 수 없는 경우에서 시작 합니다.

기본적으로 추적 Shim의 출력에 연결 된 디버거 전송을 가져옵니다. 이 예제에서는 우리는 디버거 연결 하려고 하지 및 출력을 보려면 SysInternals에서 [DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview) 프로그램이 대신 사용 됩니다. 응용 프로그램을 실행 한 후 볼 수 있습니다 동일한 오류 이전과 같이 하는 것 가리킵니다 우리 같은 런타임 수정 사항.

![TraceShim 파일을 찾을 수 없음](images/desktop-to-uwp/traceshim_filenotfound.png)

![TraceShim 액세스 거부](images/desktop-to-uwp/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>디버그, 확장 또는 런타임 수정 프로그램을 만들려면

Visual Studio를 사용 하 여 런타임 수정 프로그램을 디버깅, 런타임 수정 프로그램을 확장 하거나 처음부터 새로 만들 수 있습니다. 성공 하기 위해서는 이러한 작업을 수행 해야 합니다.

> [!div class="checklist"]
> * 패키징 프로젝트를 추가 합니다.
> * 런타임 수정 프로그램에 대 한 프로젝트를 추가 합니다.
> * 실행 파일 Shim 시작 관리자를 시작 하는 프로젝트를 추가 합니다.
> * 패키징 프로젝트를 구성 합니다.

완료 했으면이 솔루션은 다음과 같이 찾습니다.

![완성 된 솔루션](images/desktop-to-uwp/runtime-fix-project-structure.png)

이 예제에서는 각 프로젝트에 살펴보겠습니다.

| Project | 목적 |
|-------|-----------|
| DesktopApplicationPackage | 이 프로젝트는 [Windows 응용 프로그램 패키지 프로젝트](desktop-to-uwp-packaging-dot-net.md) 를 기반으로 하며 출력의 MSIX 패키지 합니다. |
| Runtimefix | 런타임 수정 프로그램으로 역할을 하는 하나 이상의 대체 함수가 포함 된 c + + Dynamic-Linked 라이브러리 프로젝트입니다. |
| ShimLauncher | C + + 빈 프로젝트입니다. 이 프로젝트가 패키지 지원 프레임 워크의 런타임 배포 가능 파일을 수집할 수 있는 위치에 설명 합니다. 실행 파일을 출력 합니다. 해당 실행 파일은 시작한 경우 사용자는 솔루션을 실행 하는 작업이 가장 먼저입니다. |
| WinFormsDesktopApplication | 이 프로젝트 데스크톱 응용 프로그램의 소스 코드를 포함합니다. |

이러한 유형의 프로젝트의 모든 포함 된 전체 예제를 살펴보는 [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/)를 참조 하십시오.

만들고 이러한 프로젝트의 각 솔루션의 구성 하는 단계를 살펴보겠습니다.


### <a name="create-a-package-solution"></a>패키지 솔루션 만들기

데스크톱 응용 프로그램에 대 한 솔루션을 없는 경우 Visual Studio에서 새 **빈 솔루션** 을 만듭니다.

![빈 솔루션](images/desktop-to-uwp/blank-solution.png)

해야 하는 응용 프로그램 프로젝트를 추가 하려면 원하는 메시지가 표시 될 수 있습니다.

### <a name="add-a-packaging-project"></a>패키징 프로젝트를 추가 합니다.

**Windows 응용 프로그램 패키지 프로젝트**를 아직 설치 하지 않은 경우 하나 만들고 솔루션에 추가 합니다.

![패키지 프로젝트 서식 파일](images/desktop-to-uwp/package-project-template.png)

Windows 응용 프로그램 패키지 프로젝트에 대 한 자세한 내용은 [Visual Studio를 사용 하 여 응용 프로그램 패키지](desktop-to-uwp-packaging-dot-net.md)를 참조 하십시오.

**솔루션 탐색기**패키징 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **편집**을 선택 다음 프로젝트 파일의 아래쪽에 추가 하는이:

```
<Target Name="PSFRemoveSourceProject" AfterTargets="ExpandProjectReferences" BeforeTargets="_ConvertItems">
<ItemGroup>
  <FilteredNonWapProjProjectOutput Include="@(_FilteredNonWapProjProjectOutput)">
  <SourceProject Condition="'%(_FilteredNonWapProjProjectOutput.SourceProject)'=='<your runtime fix project name goes here>'" />
  </FilteredNonWapProjProjectOutput>
  <_FilteredNonWapProjProjectOutput Remove="@(_FilteredNonWapProjProjectOutput)" />
  <_FilteredNonWapProjProjectOutput Include="@(FilteredNonWapProjProjectOutput)" />
</ItemGroup>
</Target>
```

### <a name="add-project-for-the-runtime-fix"></a>런타임 수정 프로그램에 대 한 프로젝트를 추가 합니다.

솔루션에 c + + **동적 연결 라이브러리 (DLL)** 프로젝트를 추가 합니다.

![런타임 수정 프로그램 라이브러리](images/desktop-to-uwp/runtime-fix-library.png)

마우스 오른쪽 단추로 클릭 한 다음 **속성**을 선택 하 고 프로젝트는 합니다.

속성 페이지에서 **c + + 언어 표준** 필드를 찾은 다음 해당 필드 옆에 있는 드롭다운 목록에서 선택 된 **ISO C + + 17 표준 (/ std:c + + 17)** 옵션입니다.

![ISO 17 옵션](images/desktop-to-uwp/iso-option.png)

해당 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 상황에 맞는 메뉴에서 **Nuget 패키지 관리** 옵션을 선택 합니다. **패키지 원본** 옵션은 **모든** 또는 **nuget.org**으로 설정 되어 있는지 확인 합니다.

해당 필드 다음 설정 아이콘을 클릭 합니다.

*PSF*에 대 한 검색 * Nuget 패키지를 선택한 다음이 프로젝트에 대 한 설치 합니다.

![nuget 패키지](images/desktop-to-uwp/psf-package.png)

디버깅 하거나 기존 런타임 수정 프로그램을 확장 하려는 경우이 가이드의 [런타임 수정 사항 찾기](#find) 섹션에 설명 된 지침을 사용 하 여 얻은 런타임 수정 프로그램 파일을 추가 합니다.

새로운 수정 프로그램을 만들려는 경우 추가 하지 마십시오 아무것도이 프로젝트에 아직. 이 가이드의 뒷부분에서이 프로젝트에 올바른 파일을 추가 하면 알아봅니다. 이제 솔루션 설정을 계속 표시 됩니다.

### <a name="add-a-project-that-starts-the-shim-launcher-executable"></a>실행 파일 Shim 시작 관리자를 시작 하는 프로젝트를 추가 합니다.

솔루션에 c + + **빈 프로젝트** 프로젝트를 추가 합니다.

![빈 프로젝트](images/desktop-to-uwp/blank-app.png)

이전 섹션에 설명 된 동일한 지침을 사용 하 여이 프로젝트에 **PSF** Nuget 패키지를 추가 합니다.

**Open 속성 페이지의 프로젝트에 대 한 및 **일반** 설정 페이지에서 대상 Name** 속성을 설정 ``ShimLauncher32`` 또는 ``ShimLauncher64`` 응용 프로그램의 아키텍처에 따라 합니다.

![shim 시작 관리자 참조 (영문)](images/desktop-to-uwp/shim-exe-reference.png)

솔루션에서 런타임 수정 프로그램 프로젝트에 대 한 프로젝트 참조를 추가 합니다.

![런타임 수정 프로그램 참조 (영문)](images/desktop-to-uwp/reference-fix.png)

참조를 마우스 오른쪽 단추로 클릭 하 고 **속성** 창에서이 값을 적용 합니다.

| 속성 | 값 |
|-------|-----------|-------|
| 로컬 복사 | True |
| 로컬 위성 어셈블리를 복사 합니다. | True |
| 참조 어셈블리 출력 | True |
| 라이브러리 종속성 링크 | False |
| 링크 라이브러리 종속성 입력 | False |

### <a name="configure-the-packaging-project"></a>패키징 프로젝트를 구성 합니다.

패키징 프로젝트에서 **응용 프로그램** 폴더를 마우스 오른쪽 단추로 클릭하고 **참조 추가**를 선택합니다.

![프로젝트 참조 추가](images/desktop-to-uwp/add-reference-packaging-project.png)

Shim 시작 관리자 프로젝트 및 데스크톱 응용 프로그램 프로젝트를 선택한 다음 **확인** 단추를 선택 합니다.

![데스크톱 프로젝트](images/desktop-to-uwp/package-project-references.png)

>[!NOTE]
> 소스 코드를 응용 프로그램을 설치 하지 않은 경우에 방금 shim 시작 관리자 프로젝트를 선택 합니다. 구성 파일을 만들 때 실행 파일을 참조 하는 방법을 살펴보겠습니다.

**응용 프로그램** 노드를에서 shim 시작 관리자 응용 프로그램을 마우스 오른쪽 단추로 클릭 하 고 **진입점으로 설정 합니다.** 를 선택 합니다.

![진입점 설정](images/desktop-to-uwp/set-startup-project.png)

명명 된 파일을 추가 ``config.json`` 패키징 프로젝트에 다음 복사 하 여 파일에 다음 json 텍스트를 붙여넣습니다. **콘텐츠**를 **패키지 Action** 속성을 설정 합니다.

```json
{
    "applications": [
        {
            "id": "",
            "executable": "",
            "workingDirectory": ""
        }
    ],
    "processes": [
        {
            "executable": "",
            "shims": [
                {
                    "dll": "",
                    "config": {
                    }
                }
            ]
        }
    ]
}
```
각 키에 대 한 값을 제공 합니다. 이 표를 사용 하 여을 참조 합니다.

| 배열 | key | 값 |
|-------|-----------|-------|
| 응용 프로그램 | id |  값을 사용 하는 `Id` 의 특성은 `Application` 패키지 매니페스트에서 요소. |
| 응용 프로그램 | 실행 파일 | 패키지 상대 경로 시작 하려면 실행 파일입니다. 대부분의 경우 수정 하기 전에이 값 패키지 매니페스트 파일에서 얻을 수 있습니다. 값이는 `Executable` 의 특성은 `Application` 요소입니다. |
| 응용 프로그램 | workingDirectory | (선택 사항) 시작 하는 응용 프로그램의 작업 디렉터리도 사용 하 여 패키지 상대 경로입니다. 운영 체제를 사용 하 여이 값을 설정 하지 않으면 하는 경우는 `System32` 응용 프로그램의 작업 디렉터리와 디렉터리입니다. |
| 프로세스 | 실행 파일 | 대부분의 경우에서이 속성은 이름을 `executable` 제거 경로 및 파일 확장명을 가진 위에 구성 합니다. |
| shim | dll | 패키지 상대 경로를 로드할 DLL shim입니다. |
| shim | 구성 | (선택 사항) Shim dl 작동 하는 방식을 제어 합니다. 이 값의 정확한 형식으로 각 shim 수로 해석 하 여이 "blob" 브로드캐스트하며 shim 하 여 shim 별로 달라 집니다. |

완료 했으면 하면 ``config.json`` 파일은 다음과 같이 찾습니다.

```json
{
  "applications": [
    {
      "id": "DesktopApplication",
      "executable": "DesktopApplication/WinFormsDesktopApplication.exe",
      "workingDirectory": "WinFormsDesktopApplication"
    }
  ],
  "processes": [
    {
      "executable": ".*App.*",
      "shims": [ { "dll": "RuntimeFix.dll" } ]
    }
  ]
}

```

>[!NOTE]
> `applications`, `processes`, 및 `shims` 키는 배열 합니다. 즉, 둘 이상의 응용 프로그램, 프로세스 및 shim DLL 지정 하려면 config.json 파일을 사용할 수 있습니다.

### <a name="debug-a-runtime-fix"></a>런타임 수정 프로그램을 디버깅

Visual Studio에서 f5 키를 눌러 디버거를 시작 합니다.  시작 하는 작업이 가장 먼저 shim 시작 관리자 응용 프로그램을 차례로 대상 데스크톱 응용 프로그램을 시작 하는 경우  대상 데스크톱 응용 프로그램을 디버깅 하려면 **디버그**를 선택 하 여 데스크톱 응용 프로그램 프로세스를 수동으로 연결 해야하는->**프로세스에 연결**하 고 다음 응용 프로그램 프로세스를 선택 합니다. 네이티브 런타임 수정 프로그램 DLL 사용 하는.NET 응용 프로그램의 디버깅을 허용 하려면 관리 코드와 네이티브 코드 형식 (혼합된 모드 디버깅)을 선택 합니다.  

했을 때이 설정한 후에 데스크톱 응용 프로그램 코드와 런타임 수정 프로그램 프로젝트에 코드 줄 옆에 있는 나누기 포인트를 설정할 수 있습니다. 소스 코드를 응용 프로그램을 설치 하지 않은 경우 런타임 수정 프로그램 프로젝트의 코드 한 줄만 옆에 있는 나누기 포인트를 설정할 수 있습니다.

>[!NOTE]
> Visual Studio에서는 가장 간단한 개발 제한 사항은 가지 경험, 디버깅 하는 동안이 가이드의 뒷부분에서 설명할 것 적용할 수 있는 다른 디버깅 기술입니다.

## <a name="create-a-runtime-fix"></a>런타임 수정 프로그램을 만들기

없는 아직를 해결 하려면을 대체 함수를 작성 하 고 모든 구성 데이터를 포함 하 여 새 런타임 수정 프로그램을 만들 수 있습니다는 런타임 문제에 대 한 수정 하는 경우 하는 것이 좋습니다. 각 부분에 살펴보겠습니다.

### <a name="replacement-functions"></a>대체 함수

먼저, MSIX 컨테이너에서 응용 프로그램을 실행 하는 경우 통화가 실패 하는 함수를 식별 합니다. 그런 다음, 런타임 관리자 대신 전화를 선택 하는 대체 함수를 만들 수 있습니다. 이렇게 하면를 구현 하는 함수 현대 런타임 환경에 적용 되는 규칙을 준수 하는 동작으로 바꿉니다.

Visual Studio에서이 가이드의 앞부분에서 만든 런타임 수정 프로그램 프로젝트를 엽니다.

선언 된 ``SHIM_DEFINE_EXPORTS`` 매크로 다음에 대 한 include 문을 추가 하 고는 `shim_framework.h` 각각의 맨 합니다. 런타임 수정 함수를 추가 하려는 CPP 파일입니다.

```c++
#define SHIM_DEFINE_EXPORTS
#include <shim_framework.h>
```
>[!IMPORTANT]
>있는지 확인은 `SHIM_DEFINE_EXPORTS` 매크로 include 문을 앞에 나타납니다.

함수의 동일한 서명이 하는 함수 만들기 사용자가 수정할 동작 합니다. 다음은 대체 하는 예제 함수는 `MessageBoxW` 함수입니다.

```c++
auto MessageBoxWImpl = &::MessageBoxW;
int WINAPI MessageBoxWShim(
    _In_opt_ HWND hwnd,
    _In_opt_ LPCWSTR,
    _In_opt_ LPCWSTR caption,
    _In_ UINT type)
{
    return MessageBoxWImpl(hwnd, L"SUCCESS: This worked", caption, type);
}

DECLARE_SHIM(MessageBoxWImpl, MessageBoxWShim);
```

에 대 한 호출 `DECLARE_SHIM` 매핑되는 `MessageBoxW` 함수를 새 대체 함수입니다. 응용 프로그램 호출 하려고 할 때의 `MessageBoxW` 함수를 호출 하는 것 대체 함수 대신 합니다.

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>런타임 수정 사항에 대 한 함수에 대 한 재귀 호출 보호

선택적으로 적용할 수는 `reentrancy_guard` 형식 재귀에에서 함수 호출 런타임 수정 사항에 대해 보호 하는 함수입니다.

에 대 한 대체 함수를 생성할 수 있습니다 예는 `CreateFile` 함수입니다. 구현을 호출할 수 있습니다는 `CopyFile` 함수, 하지만 구현 하는 `CopyFile` 함수를 호출할 수는 `CreateFile` 함수입니다. 이에 대 한 통화는 무한 재귀 주기로 인해 발생할 수 있습니다는 `CreateFile` 함수입니다.

에 대 한 자세한 내용은 `reentrancy_guard` [authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md) 를 참조 하십시오.

### <a name="configuration-data"></a>구성 데이터

런타임 수정에 구성 데이터를 추가 하려면 추가할 것을 고려 하 여 ``config.json``합니다. 사용할 수 있는 이러한 방법으로는 `ShimQueryCurrentDllConfig` 쉽게 하 여 데이터를 구문 분석할 수 있습니다. 이 예제에서는 해당 구성 파일에서 boolean 및 문자열 값을 구문 분석합니다.

```c++
if (auto configRoot = ::ShimQueryCurrentDllConfig())
{
    auto& config = configRoot->as_object();

    if (auto enabledValue = config.try_get("enabled"))
    {
        g_enabled = enabledValue->as_boolean().get();
    }

    if (auto logPathValue = config.try_get("logPath"))
    {
        g_logPath = logPathValue->as_string().wstring();
    }
}
```

## <a name="other-debugging-techniques"></a>다른 디버깅 기술

Visual Studio를 사용 하면 가장 간단한 개발 및 디버깅 경험, 하는 동안에 일부 제한이 있습니다.

첫째, F5 디버깅.appx 패키지에서 설치 하는 것이 아니라 패키지 레이아웃 폴더 경로에서 느슨한 파일을 배포 하 여 응용 프로그램을 실행 합니다.  일반적으로 레이아웃 폴더가 설치 된 패키지 폴더와 동일한 보안 제한을가지고 있지 않습니다. 결과적으로, 해당 되지 않을 수 있습니다 런타임 수정 프로그램을 적용 하기 전에 패키지 경로 액세스 거부 오류를 재현할 수 있습니다.

이 문제를 해결 하기 위해 F5 느슨한 파일로 배포 하지 않고.appx 패키지 배포를 사용 합니다.  위에서 설명한 것 처럼.appx 패키지 파일을 만들려면 Windows SDK에서 [MakeAppx](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) 유틸리티를 사용 합니다. 또는에서 Visual Studio 내에서 응용 프로그램 프로젝트 노드를 마우스 오른쪽 단추로 클릭 하 고 **저장소**선택->**응용 프로그램 패키지 만들기**.

Visual Studio를 사용 하는 다른 문제는 디버거 사용 하 여 시작 하는 모든 자식 프로세스에 연결 하는 것에 대 한 기본 제공 지원 되어있지 않습니다.   이 실행 한 후 Visual Studio에서 수동으로 첨부할 수 있어야 하는 대상 응용 프로그램의 시작 경로에서 논리를 디버깅 하 어려워집니다.

이 문제를 해결 하기 위해 사용 하 여 하위 프로세스를 지 원하는 디버거 연결 합니다.  참고 아닌지 일반적으로 적시에 (JIT) 디버거 대상 응용 프로그램에 연결할 수 있습니다.  이 대부분 JIT 기술을 ImageFileExecutionOptions 레지스트리 키를 통해 대상 응용 프로그램 대신 디버거 실행을 포함 합니다.  이 방법을 ShimLauncher.exe 대상 응용 프로그램에 ShimRuntime.dll 주입를 사용 하는 detouring 메커니즘입니다.  WinDbg, [Windows 용 디버깅 도구](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)포함 하 고 [Windows sdk (영문)](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk)가져온 지원 하위 프로세스에 연결 합니다.  또한 이제 지원 직접 [시작 하 고 UWP 응용 프로그램을 디버깅](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app)합니다.

대상 응용 프로그램 시작 하위 프로세스를 디버깅 하려면 시작 ``WinDbg``합니다.

```
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

``WinDbg`` , 메시지를 표시 하 고, 자식 디버깅을 설정 하 고, 적절 한 중단점을 설정 합니다.

```
.childdbg 1
g
```
(대상 응용 프로그램을 시작 하 고 디버거도 나누기 때까지 실행)

```
sxe ld fixup.dll
g
```
(DLL을 로드 하는 수정까지 실행)

```
bp ...
```

>[!NOTE]
> [PLMDebug](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/plmdebug) 응용 프로그램을 시작 시에 디버거를 연결 하려면도 사용할 수 있으며 [Windows 용 디버깅 도구에](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)에서도 포함 됩니다.  그러나 이제 WinDbg 제공한 직접 지원 보다 사용 하 여 작업이 복잡해 집니다.

## <a name="support-and-feedback"></a>지원 및 피드백

**질문에 대한 답변 찾기**

질문이 있으세요? Stack Overflow에서 질문해 주세요. 저희 팀은 이러한 [태그](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다. [여기](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)에서 Microsoft에 문의할 수도 있습니다.
