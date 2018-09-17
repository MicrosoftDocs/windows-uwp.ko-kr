---
author: normesta
Description: Fix issues that prevent your desktop application from running in an MSIX container
Search.Product: eADQiWindows 10XVcnh
title: MSIX 컨테이너에서 실행 되는 데스크톱 응용 프로그램을 방해 하는 문제를 해결 합니다.
ms.author: normesta
ms.date: 07/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 46d5705233af9e8254b9ac89a2d6e9891e90701f
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/17/2018
ms.locfileid: "3986370"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>MSIX 패키지에 패키지 지원 프레임 워크를 사용 하 여 런타임 수정 프로그램을 적용

패키지 지원 프레임 워크 MSIX 컨테이너에서 실행 될 수 있도록 소스 코드에 액세스할 수 없는 경우 수정 기존 win32 응용 프로그램에 적용 하는 데 도움이 되는 오픈 소스 키트입니다. 패키지 지원 프레임 워크 최신 런타임 환경에 최선의 방법에 따라 응용 프로그램을 수 있습니다.

패키지 지원 프레임 워크를 만들려면 우리는 Microsoft Research (MSR)에 의해 개발 된 오픈 소스 프레임 워크와 API 리디렉션 및 연결 하는 데 도움이 되는 [우회](https://www.microsoft.com/en-us/research/project/detours) 기술을 활용 했습니다.

프레임이 워크는 오픈 소스를 간단 하 고 사용할 수 있습니다 응용 프로그램 문제 해결을 신속 하 게. 또한, 세계의 커뮤니티와 협의 하 고 다른 사람들의 투자 기반을 활용 하는 기회를 제공 합니다.

## <a name="a-quick-look-inside-of-the-package-support-framework"></a>내부 패키지 지원 프레임 워크 개요

패키지 지원 프레임 워크는 실행 파일, DLL 런타임 관리자와 런타임 수정 집합이 포함 되어 있습니다.

![패키지 지원 프레임 워크](images/desktop-to-uwp/package-support-framework.png)

작동 방식은 다음과 같습니다. 응용 프로그램에 적용 하려면 fix(s)를 지정 하는 구성 파일을 만듭니다. 그런 다음 shim 실행 프로그램 실행 파일을 가리키도록 패키지 수정.

사용자가 응용 프로그램을 시작 심 (shim) 실행 프로그램 실행 되는 실행 파일입니다. 구성 파일을 읽고 응용 프로그램 프로세스 런타임 fix(s) 및 런타임 관리자 DLL 삽입 합니다.

![패키지 지원 프레임 워크 DLL 주입](images/desktop-to-uwp/package-support-framework-2.png)

런타임 관리자 MSIX 컨테이너를 사용 하는 환경에서 실행 하는 응용 프로그램에서 필요할 때 수정 프로그램을 적용 합니다.

이 가이드를 사용 하면 응용 프로그램 호환성 문제를 식별 하 고 찾을 적용 하 고 런타임 확장을 해결 하는 수정.

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>패키지 된 응용 프로그램 호환성 문제를 식별 합니다.

먼저 응용 프로그램에 대 한 패키지를 만듭니다. 다음, 설치 하 고 실행의 동작을 관찰 합니다. 호환성 문제를 식별 하는 데 도움이 되는 오류 메시지가 나타날 수 있습니다. [프로세스 모니터](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) 문제를 식별 하는 데 사용할 수도 있습니다.  일반적인 문제 관련 프로그램 경로 권한과 작업 디렉터리에 대 한 응용 프로그램 가정 됩니다.

### <a name="using-process-monitor-to-identify-an-issue"></a>프로세스 모니터를 사용 하 여 문제를 식별 합니다.

[프로세스 모니터](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) 는 응용 프로그램의 파일 및 레지스트리 작업 및 테스트 결과 관찰 하기 위한 강력한 유틸리티입니다.  응용 프로그램 호환성 문제를 이해 하는 데 도움이 수 있습니다.  프로세스 모니터를 연 다음 추가 필터 (필터 > 필터...) 응용 프로그램 실행 파일의 이벤트만 포함.

![ProcMon 응용 프로그램 필터](images/desktop-to-uwp/procmon_app_filter.png)

이벤트 목록이 표시 됩니다. 이러한 이벤트 중 많은 단어 **성공** 나타납니다 **결과** 열.

![ProcMon 이벤트](images/desktop-to-uwp/procmon_events.png)

필요에 따라 이벤트 오류만 표시 하도록 필터링 할 수 있습니다.

![성공 ProcMon 제외](images/desktop-to-uwp/procmon_exclude_success.png)

파일 시스템 액세스 오류를 의심 되 면 System32/SysWOW64 또는 패키지 파일 경로 중 하나 아래에 있는 실패 한 이벤트를 검색 합니다. 필터 수 있습니다, 너무. 이 목록의 맨 아래에 시작한 위쪽으로 스크롤하십시오. 가장 최근에이 목록 맨 아래에 나타나는 오류 발생. "경로/이름을 찾을 수 없음", "액세스가 거부 되었습니다"와 같은 문자열을 포함 하는 오류를 대부분 주의 하 고 보이지 않는 의심 하는 것을 무시. [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/) 에 두 가지 문제가 있습니다. 다음 그림에 표시 된 목록에서 문제를 볼 수 있습니다.

![ProcMon Config.txt](images/desktop-to-uwp/procmon_config_txt.png)

이 이미지에 표시 되는 첫 번째 문제에서 "C:\Windows\SysWOW64" 경로에 있는 "Config.txt" 파일을 읽는 응용 프로그램이 실패 했습니다. 응용 프로그램은 해당 경로 직접 참조 하려고 가능성이 아닙니다. 대부분의 경우 상대 경로 사용 하 여 해당 파일에서 읽을 하려고 하 고 기본적으로 "System32/SysWOW64"는 응용 프로그램의 작업 디렉터리. 이 응용 프로그램 패키지의 위치 설정할 수 현재 작업 디렉터리를 기대는 것이 좋습니다. Appx의 내부를 찾는 파일이 실행 파일과 같은 디렉터리에 있는지 볼 수 있습니다.

![응용 프로그램 Config.txt](images/desktop-to-uwp/psfsampleapp_config_txt.png)

두 번째 문제는 다음 그림에 표시 됩니다.

![ProcMon 로그 파일](images/desktop-to-uwp/procmon_logfile.png)

이번이 호에서 응용 프로그램 패키지 경로에.log 파일을 쓰는 데 실패 했습니다. 이 것이 좋습니다 파일 리디렉션 심은 도움이 될 수 있습니다.

<a id="find" />

## <a name="find-a-runtime-fix"></a>런타임 수정 사항 찾기

해당 PSF 파일 리디렉션 심은 등 지금 당장 사용할 수 있는 런타임 수정 프로그램이 포함 되어 있습니다.

### <a name="file-redirection-shim"></a>파일 리디렉션 심은

쓰거나 읽을 MSIX 컨테이너에서 실행 되는 응용 프로그램에서 액세스할 수 없는 디렉터리에 데이터를 리디렉션할 [파일 리디렉션 Shim](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim) 을 사용할 수 있습니다.

예를 들어, 응용 프로그램 실행 프로그램과 같은 디렉터리에 있는 로그 파일에 쓰는, 사용할 수 있습니다 [파일 리디렉션 심](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim) 은 로컬 응용 프로그램 데이터 저장소와 같은 다른 위치에 해당 로그 파일을 만들 수 있습니다.

### <a name="runtime-fixes-from-the-community"></a>커뮤니티에서 런타임 수정

커뮤니티 기여도 [GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop) 페이지를 검토할 수 있는지 확인 하십시오. 다른 개발자가 유사한 문제를 해결 하 고 런타임 수정 공유 가능 합니다.

## <a name="apply-a-runtime-fix"></a>런타임 수정 프로그램을 적용

Windows SDK를 다음이 단계를 수행 하 여 몇 가지 간단한 도구를 사용 하 여 기존 런타임 수정 프로그램을 적용할 수 있습니다.

> [!div class="checklist"]
> * 패키지 레이아웃 폴더 만들기
> * 패키지 지원 프레임 워크 파일 가져오기
> * 패키지 추가
> * 패키지 매니페스트 수정
> * 구성 파일 만들기

각 작업을 통해 가자.

### <a name="create-the-package-layout-folder"></a>패키지 레이아웃 폴더 만들기

.Appx 파일이 이미 있는 경우 패키지에 대 한 준비 영역으로 사용할 수 있는 레이아웃 폴더로 내용을 풀 수 있습니다.  이 작업을 수행할 수 있는 **x64 VS 2017에 대 한 기본 도구 명령 프롬프트**, 또는 실행 파일 검색 경로에 SDK bin 경로 사용 하 여 수동으로.

```
makeappx unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.appx /d PackageContents

```

이렇게 하면 다음과 같은 구문이 표시 됩니다.

![패키지 레이아웃](images/desktop-to-uwp/package_contents.png)

.Appx 파일을 시작 하 여 없다면 처음부터 패키지 폴더와 파일을 만들 수 있습니다.

### <a name="get-the-package-support-framework-files"></a>패키지 지원 프레임 워크 파일 가져오기

PSF Nuget 패키지는 Visual Studio 사용 하 여 얻을 수 있습니다. 독립 실행형 Nuget 명령줄 도구를 사용 하 여 얻을 수 있습니다.

#### <a name="get-the-package-by-using-visual-studio"></a>Visual Studio 사용 하 여 패키지를 얻을

Visual Studio 솔루션이 나 프로젝트 노드를 마우스 오른쪽 단추로 클릭 하 고 Nuget 패키지 관리 명령 중 하나를 선택 하십시오.  **Microsoft.PackageSupportFramework** 또는 **PSF** Nuget.org에서 패키지를 찾을 수 검색 합니다. 설치 합니다.

#### <a name="get-the-package-by-using-the-command-line-tool"></a>명령줄 도구를 사용 하 여 패키지를 얻을

이 위치의 Nuget 명령줄 도구 설치: https://www.nuget.org/downloads. 그런 다음 Nuget 명령줄에서이 명령을 실행 합니다.

```
nuget install Microsoft.PackageSupportFramework
```

### <a name="add-the-package-support-framework-files-to-your-package"></a>패키지 지원 프레임 워크 파일에 패키지 추가

패키지 디렉터리에 필요한 32 비트 및 64 비트 PSF Dll 및 실행 파일을 추가 합니다. 다음 표를 가이드로 따르세요. 또한 필요한 모든 런타임 수정 프로그램을 포함 합니다. 이 예제에서는 리디렉션 런타임 파일 수정을 해야합니다.

| 응용 프로그램 실행 파일은 x64 | 응용 프로그램 실행 파일은 x86 |
|-------------------------------|-----------|
| [ShimLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |  [ShimLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |
| [ShimRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) | [ShimRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) |
| [ShimRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) | [ShimRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) |

패키지 내용을 다음과 같이 모양은.

![바이너리 패키지](images/desktop-to-uwp/package_binaries.png)

### <a name="modify-the-package-manifest"></a>패키지 매니페스트 수정

패키지 매니페스트를 텍스트 편집기에서 연 다음 설정에서 `Executable` 의 특성은 `Application` shim 실행 프로그램 실행 파일의 이름으로 요소.  대상 응용 프로그램의 아키텍처를 알고 있으면 해당 버전 ShimLauncher32.exe 또는 ShimLauncher64.exe을 선택 합니다.  그렇지 않은 경우 ShimLauncher32.exe는 항상에서 작동 합니다.  예를 들면 다음과 같습니다.

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

파일 이름을 ``config.json``, 패키지의 루트 폴더에 해당 파일을 저장 합니다. 방금 교체 하는 실행 파일을 가리키도록 config.json 파일의 선언 된 응용 프로그램 ID를 수정 합니다. 프로세스 모니터를 사용 하 여 얻은 정보를 사용 하 여 있습니다 수 또한 작업 디렉토리를 설정으로 파일 리디렉션 shim을 사용 하 여 패키지 상대 "PSFSampleApp" 디렉터리 아래에 있는.log 파일을 읽기/쓰기를 리디렉션할.

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
다음은 config.json 스키마:

| 배열 | key | 값 |
|-------|-----------|-------|
| 응용 프로그램 | id |  값을 사용 하는 `Id` 특성은 `Application` 패키지 매니페스트의 요소. |
| 응용 프로그램 | 실행 파일 | 시작 하려는 실행 파일에 패키지 상대 경로입니다. 대부분의 경우 수정 하기 전에이 값 패키지 매니페스트 파일에서 얻을 수 있습니다. 값은 `Executable` 특성은 `Application` 요소. |
| 응용 프로그램 | workingDirectory | (선택 사항) 로 시작 하는 응용 프로그램의 작업 디렉터리를 사용 하 여 패키지 상대 경로. 운영 체제를 사용 하 여이 값을 설정 하지 않으면의 `System32` 디렉터리와 응용 프로그램의 작업 디렉터리입니다. |
| 프로세스 | 실행 파일 | 대부분의 경우에서 이름이 됩니다는 `executable` 위 구성 경로 및 파일 확장명을 제거 합니다. |
| shim | dll | 로드할 수 심은.appx 패키지 상대 경로입니다. |
| shim | 구성 | (선택 사항) 심은 dl의 동작 방식을 제어 합니다. 이 값의 정확한 형식은 각 shim이 "blob" 해석할 수 원하는 대로 shim으로 심은 별로 다릅니다. |

`applications`, `processes`, 및 `shims` 키가 배열입니다. 즉, 응용 프로그램, 프로세스 및 shim DLL 여러 개 지정 하려면 config.json 파일을 사용할 수 있습니다.


### <a name="package-and-test-the-app"></a>패키지 및 응용 프로그램 테스트

다음에 패키지를 만듭니다.

```
makeappx pack /d PackageContents /p PSFSamplePackageFixup.appx
```

그런 다음 서명.

```
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.appx
```

자세한 내용은 [서명 인증서는 패키지를 만드는](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate) 방법과 [signtool을 사용 하 여 패키지에 서명 하는 방법](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool) 을 참조 하십시오.

PowerShell을 사용 하 여 패키지를 설치 합니다.

>[!NOTE]
> 먼저 패키지를 제거 해야 합니다.

```
powershell Add-AppxPackage .\PSFSamplePackageFixup.appx
```

응용 프로그램을 실행 하 고 런타임 수정 프로그램이 적용 된 동작을 관찰 합니다.  진단 및 필요에 따라 포장 단계를 반복 합니다.

### <a name="use-the-trace-shim"></a>추적 Shim을 사용 하 여

패키지 된 응용 프로그램 호환성 문제를 진단 하는 대체 기술을 추적 Shim을 사용 하는 것입니다. 이 DLL의 PSF에 포함 된 응용 프로그램의 동작을 비슷한 프로세스 모니터의 상세 진단 보기를 제공 하며  응용 프로그램 호환성 문제를 표시 하도록 특별히 설계 되었습니다.  추적 Shim을 사용 하 여, 패키지에 DLL을 추가 프로그램 config.json에 다음을 추가 하 고 다음 패키지 하 고 설치 응용 프로그램입니다.

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

기본적으로 추적 Shim "예상 된"으로 간주할 수도 있는 오류를 필터링 합니다.  예를 들어, 응용 프로그램이 이미 존재 하는지, 결과 무시 하 고 참조를 확인 하지 않고 무조건 파일을 삭제 시도할 수 있습니다. 이것은 파일 시스템 함수에서 모든 오류를 받을 수 선택 우리는 위의 예제 이므로 일부 예기치 않은 오류 필터링 얻을 수 있는 불행 한 결과가 있습니다. 그 전에 Config.txt 파일에서 읽을 수 실패 하는 메시지 "파일이 없습니다"를 알고 있기 때문에이 수행 됩니다. 이것은 자주 관찰 하 고 필요 없는 것으로 보이며 일반적으로 하지 하는 오류입니다. 실제로 가능성이 가장 예기치 않은 오류에만 필터링 한 다음 대체 하기 모든 오류 문제가 여전히 식별할 수 없는 경우 시작 하는 것.

기본적으로 출력 추적 Shim을 연결 된 디버거에 보내집니다. 이 예제에서는 디버거를 연결할 생각 우리 고 출력을 보려면 SysInternals에서 [DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview) 프로그램 대신 사용 됩니다. 응용 프로그램을 실행 한 후 볼 수 있습니다 동일한 오류 예전 처럼가 나타나는데 우리 같은 런타임 수정 쪽으로.

![TraceShim 파일을 찾을 수 없습니다.](images/desktop-to-uwp/traceshim_filenotfound.png)

![TraceShim 액세스가 거부 되었습니다.](images/desktop-to-uwp/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>디버그, 확장 또는 런타임 수정 프로그램을 만듭니다.

Visual Studio 디버그 런타임 수정 확장 런타임 수정 프로그램을 만들거나, 처음부터 사용할 수 있습니다. 성공 하기 위해서는 이러한 작업을 수행 해야 합니다.

> [!div class="checklist"]
> * 패키징 프로젝트 추가
> * 런타임 수정 프로그램에 대 한 프로젝트를 추가 합니다.
> * 심 (shim) 실행 프로그램 실행을 시작 하는 프로젝트에 추가
> * 패키징 프로젝트 구성

완료 되 면 다음과 같이 솔루션에 보입니다.

![완성 된 솔루션](images/desktop-to-uwp/runtime-fix-project-structure.png)

이 예제에서 각 프로젝트에 살펴보겠습니다.

| Project | 목적 |
|-------|-----------|
| DesktopApplicationPackage | 이 프로젝트는 [Windows 응용 프로그램 패키징 프로젝트](desktop-to-uwp-packaging-dot-net.md) 기반 고 출력의 MSIX 패키지. |
| Runtimefix | 런타임 수정 프로그램으로 사용 되는 하나 이상의 대체 함수를 포함 하는 c + + Dynamic-Linked 라이브러리 프로젝트입니다. |
| ShimLauncher | C + + 빈 프로젝트입니다. 이 프로젝트는 패키지 지원 프레임 워크의 런타임 배포 가능한 파일을 수집할 수 있습니다. 실행 파일을 출력 합니다. 해당 실행 파일은 먼저 솔루션을 시작할 때 실행 합니다. |
| WinFormsDesktopApplication | 이 프로젝트에 데스크톱 응용 프로그램의 소스 코드가 포함 되어 있습니다. |

이러한 유형의 프로젝트를 모두 포함 하는 전체 샘플을 볼 수 [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/)를 참조 하십시오.

만들고 솔루션의 각이 프로젝트를 구성 하는 단계를 살펴보겠습니다.


### <a name="create-a-package-solution"></a>솔루션 패키지 만들기

데스크톱 응용 프로그램에 대 한 솔루션이 없는 경우 Visual Studio 새 **빈 솔루션** 을 만듭니다.

![빈 솔루션](images/desktop-to-uwp/blank-solution.png)

있는 모든 응용 프로그램 프로젝트를 추가 하려면.

### <a name="add-a-packaging-project"></a>패키징 프로젝트 추가

**Windows 응용 프로그램 패키징 프로젝트**를 이미 없는 경우 하나 만들고 솔루션에 추가 합니다.

![프로젝트 템플릿 패키지](images/desktop-to-uwp/package-project-template.png)

자세한 내용은 Windows 응용 프로그램 패키징 프로젝트에서 [Visual Studio 사용 하 여 응용 프로그램 패키지](desktop-to-uwp-packaging-dot-net.md)를 참조 하십시오.

**솔루션 탐색기**포장 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **편집**을 선택 합니다. 프로젝트 파일의 맨 아래에이 추가 합니다.

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

C + + **동적 연결 라이브러리 (DLL)** 프로젝트를 솔루션에 추가 합니다.

![런타임 수정 프로그램 라이브러리](images/desktop-to-uwp/runtime-fix-library.png)

다음 **속성**을 선택 하 고 프로젝트를 마우스 오른쪽 단추로.

속성 페이지에서 **c + + 언어 표준** 필드를 찾은 다음 해당 필드 옆에 있는 드롭 다운 목록에서 선택 된 **ISO C + + 17 표준 (/ std:c + + 17)** 옵션.

![17 ISO 옵션](images/desktop-to-uwp/iso-option.png)

해당 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 상황에 맞는 메뉴에서 **Nuget 패키지 관리** 옵션을 선택 합니다. **패키지 소스** 옵션 **전체** 또는 **nuget.org**로 설정 되어 있는지 확인 하십시오.

필드는 다음 설정 아이콘을 클릭 합니다.

*PSF*검색 * Nuget 패키지를 선택한 다음이 프로젝트에 대해 설치 합니다.

![nuget의 패키지](images/desktop-to-uwp/psf-package.png)

디버깅 하거나 기존 런타임 수정 프로그램을 확장 하려는 경우이 가이드의 [런타임 수정 사항 찾기](#find) 섹션에 설명 된 지침을 사용 하 여 가져온 런타임 수정 프로그램 파일을 추가 합니다.

새로운 수정 프로그램을 작성 하려는 경우 추가 하지 마십시오 것이 프로젝트를 아직. 우리가 도와줄이 가이드의 뒷부분에서이 프로젝트에 올바른 파일을 추가할 수 있습니다. 당분간 솔루션 설정을 계속 합니다.

### <a name="add-a-project-that-starts-the-shim-launcher-executable"></a>심 (shim) 실행 프로그램 실행을 시작 하는 프로젝트에 추가

C + + **빈 프로젝트** 프로젝트를 솔루션에 추가 합니다.

![빈 프로젝트](images/desktop-to-uwp/blank-app.png)

이전 섹션에 설명 된 동일한 지침을 사용 하 여이 프로젝트에 **PSF** Nuget 패키지를 추가 합니다.

**열기 및 **일반** 설정 페이지에서 프로젝트의 속성 페이지에서 대상 이름** 속성을 설정 ``ShimLauncher32`` 또는 ``ShimLauncher64`` 응용 프로그램의 아키텍처에 따라.

![심 (shim) 실행 프로그램 참조](images/desktop-to-uwp/shim-exe-reference.png)

런타임 수정 프로젝트에 대 한 프로젝트를 솔루션에 추가 합니다.

![런타임 수정 참조](images/desktop-to-uwp/reference-fix.png)

참조를 마우스 오른쪽 단추로 클릭 하 고 **속성** 창에서 이러한 값을 적용 합니다.

| 속성 | 값 |
|-------|-----------|-------|
| 로컬 복사 | True |
| 위성 어셈블리 로컬 복사 | True |
| 참조 어셈블리 출력 | True |
| 라이브러리 종속성 링크 | False |
| 라이브러리 종속성 입력 연결 | False |

### <a name="configure-the-packaging-project"></a>패키징 프로젝트 구성

패키징 프로젝트에서 **응용 프로그램** 폴더를 마우스 오른쪽 단추로 클릭하고 **참조 추가**를 선택합니다.

![프로젝트 참조 추가](images/desktop-to-uwp/add-reference-packaging-project.png)

심 (shim) 실행 프로그램 프로젝트 및 데스크톱 응용 프로그램 프로젝트를 선택한 다음 **확인** 단추를 선택 합니다.

![데스크톱 프로젝트](images/desktop-to-uwp/package-project-references.png)

>[!NOTE]
> 응용 프로그램 소스 코드가 없다면 단지 심은 시작 관리자 프로젝트를 선택 합니다. 실행 파일의 구성 파일을 만들 때 참조 하는 방법을 알아봅니다.

**응용 프로그램** 노드에서 심은 시작 관리자 응용 프로그램을 마우스 오른쪽 단추로 클릭 하 고 **시작 지점으로 설정**을 선택 합니다.

![진입점 설정](images/desktop-to-uwp/set-startup-project.png)

라는 추가 ``config.json`` 포장 프로젝트에 다음을 복사한 다음 json 텍스트 파일에 붙여넣습니다. **콘텐츠**를 **패키지 작업** 속성을 설정 합니다.

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
각 키에 값을 입력 합니다. 이 테이블을 참조 하 여.

| 배열 | key | 값 |
|-------|-----------|-------|
| 응용 프로그램 | id |  값을 사용 하는 `Id` 특성은 `Application` 패키지 매니페스트의 요소. |
| 응용 프로그램 | 실행 파일 | 시작 하려는 실행 파일에 패키지 상대 경로입니다. 대부분의 경우 수정 하기 전에이 값 패키지 매니페스트 파일에서 얻을 수 있습니다. 값은 `Executable` 특성은 `Application` 요소. |
| 응용 프로그램 | workingDirectory | (선택 사항) 로 시작 하는 응용 프로그램의 작업 디렉터리를 사용 하 여 패키지 상대 경로. 운영 체제를 사용 하 여이 값을 설정 하지 않으면의 `System32` 디렉터리와 응용 프로그램의 작업 디렉터리입니다. |
| 프로세스 | 실행 파일 | 대부분의 경우에서 이름이 됩니다는 `executable` 위 구성 경로 및 파일 확장명을 제거 합니다. |
| shim | dll | Shim DLL 로드할 패키지 상대 경로입니다. |
| shim | 구성 | (선택 사항) 심은 dl의 동작 방식을 제어 합니다. 이 값의 정확한 형식은 각 shim이 "blob" 해석할 수 원하는 대로 shim으로 심은 별로 다릅니다. |

완료 되 면 프로그램 ``config.json`` 파일의 모습은 다음과 같습니다.

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
> `applications`, `processes`, 및 `shims` 키가 배열입니다. 즉, 응용 프로그램, 프로세스 및 shim DLL 여러 개 지정 하려면 config.json 파일을 사용할 수 있습니다.

### <a name="debug-a-runtime-fix"></a>디버그 런타임 수정

Visual Studio f5 키를 눌러 디버거를 시작 합니다.  가장 먼저 시작 하는 대상 데스크톱 응용 프로그램을 시작 하는 shim 시작 관리자 응용 프로그램입니다.  대상 데스크톱 응용 프로그램을 디버깅 하려면 **디버깅**을 선택 하 여 데스크톱 응용 프로그램 프로세스에 수동으로 연결 해야->**프로세스에 연결**선택한 다음 응용 프로그램 프로세스입니다. 네이티브 런타임 DLL 수정 프로그램을 사용 하 여.NET 응용 프로그램의 디버깅을 허용 하도록 관리 코드와 네이티브 코드 형식 (혼합된 모드 디버깅)을 선택 합니다.  

설정이 끝나면이, 데스크톱 응용 프로그램 코드와 런타임 수정 프로젝트에서 코드 줄 옆에 중단점을 설정할 수 있습니다. 응용 프로그램 소스 코드가 없다면 런타임 수정 프로젝트에만 코드 줄 옆에 중단점을 설정할 수 있습니다.

>[!NOTE]
> Visual Studio에서는 간단한 개발을 디버깅, 일부의 제한이 있지만이 가이드의 뒷부분에서 설명할 것 다른 디버깅 기법을 적용할 수 있습니다.

## <a name="create-a-runtime-fix"></a>런타임 수정 만들기

없는 경우 런타임 수정 문제를 해결 하려면 대체 함수를 작성 하 고 구성 데이터를 포함 하 여 새로운 런타임 수정 프로그램을 만들 수 있습니다 하는 의미가 있습니다. 각 부분에 살펴보겠습니다.

### <a name="replacement-functions"></a>대체 기능

첫째, MSIX 컨테이너에서 응용 프로그램을 실행 하는 경우 호출이 실패 하는 함수를 식별 합니다. 그런 다음 대체 함수를 대신 호출 하는 런타임 관리자를 만들 수 있습니다. 이 함수의 구현을 대체 규칙을 최신 런타임 환경에 맞는 동작을 제공 합니다.

Visual Studio이 가이드의 앞부분에서 만든 런타임 수정 프로젝트를 엽니다.

선언 된 ``SHIM_DEFINE_EXPORTS`` 매크로 다음 include 문을 추가 `shim_framework.h` 각각 맨. CPP 파일에 런타임 수정 프로그램의 기능을 추가 하려는.

```c++
#define SHIM_DEFINE_EXPORTS
#include <shim_framework.h>
```
>[!IMPORTANT]
>있는지 확인은 `SHIM_DEFINE_EXPORTS` 매크로 include 문의 앞에 나타납니다.

함수 같은 서명이 있는 함수를 만들어가 동작을 수정 하려면. 다음은 대체 하는 예제 함수는 `MessageBoxW` 함수.

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

호출을 `DECLARE_SHIM` 매핑하는 `MessageBoxW` 새 대체 함수에는 함수입니다. 응용 프로그램 호출 하려고 할 때의 `MessageBoxW` 함수를 호출 합니다 대체 함수 대신.

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>런타임 수정 프로그램의 기능에 대 한 재귀 호출을 방지

선택적으로 적용할 수는 `reentrancy_guard` 런타임 수정 프로그램의 기능에 대 한 재귀 호출을 방지 하 여 함수에는 유형.

에 대 한 대체 함수를 발생할 수 있습니다 예를 들어, 해당 `CreateFile` 함수입니다. 구현을 호출 하는 `CopyFile` 함수, 하지만 구현은 `CopyFile` 함수를 호출할 수 있습니다는 `CreateFile` 함수. 에 대 한 호출에는 무한 재귀 주기가 발생할 수 있습니다는 `CreateFile` 함수입니다.

대 한 자세한 내용은 `reentrancy_guard` [authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md) 를 참조 하십시오.

### <a name="configuration-data"></a>구성 데이터

런타임 수정 프로그램 구성 데이터를 추가 하려면 추가할 것을 고려 하 여 ``config.json``. 사용할 수 있습니다 이러한 방법으로는 `ShimQueryCurrentDllConfig` 쉽게 데이터를 구문 분석할 수 있습니다. 이 예제에서는 해당 구성 파일에서 부울 형식과 문자열 값을 구문 분석합니다.

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

Visual Studio 사용 하면 간단한 개발 및 디버깅, 하지만 몇 가지 제한이 있습니다.

먼저 F5 디버깅.appx 패키지를 설치 하는 것이 아니라 패키지 레이아웃 폴더 경로에서 느슨한 파일을 배포 하 여 응용 프로그램을 실행 합니다.  일반적으로 레이아웃 폴더가 설치 된 패키지 폴더와 동일한 보안 제한을가지고 있지 않습니다. 따라서 런타임 수정 프로그램을 적용 하기 전에 패키지 경로 액세스 거부 오류를 재현 하는 불가능할 수도 있습니다 것입니다.

이 문제를 해결 하기 위해 F5 느슨한 파일로 배포 하는 대신.appx 패키지 배포를 사용 합니다.  위에서 설명한 대로.appx 패키지 파일을 만들려면 Windows SDK에서 [MakeAppx](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) 유틸리티를 사용 합니다. 또는에서 Visual Studio 내에서 응용 프로그램 프로젝트 노드를 마우스 오른쪽 단추로 클릭 하 고 **저장소**선택->**응용 프로그램 패키지를 만들**.

Visual Studio 사용 하 여 또 다른 문제는 기본 제공 되는 디버거를 통해 시작 된 모든 자식 프로세스에 연결할 필요가 없습니다.   따라서 수동으로 연결 해야 Visual Studio 출시 후 대상 응용 프로그램의 시작 경로에서 논리를 디버깅 하기 어렵습니다.

이 문제를 해결 하기 위해 자식 프로세스를 지 원하는 디버거를 사용 하 여 연결 합니다.  참고 아닌지 일반적으로 적시에 디버거 (JIT) 대상 응용 프로그램에 연결할 수 있습니다.  즉, 대부분 JIT 기법 대신 ImageFileExecutionOptions 레지스트리 키를 통해 대상 응용 프로그램에 디버거를 실행 포함.  이 방법을 detouring 메커니즘 ShimLauncher.exe 대상 응용 프로그램에 ShimRuntime.dll를 삽입 하는 데 사용 됩니다.  WinDbg, [Windows 용 디버깅 도구](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)에 포함 되어 있으며 [Windows SDK](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk)얻은 지원 하위 프로세스를 연결 합니다.  [이제 직접 실행 및](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app)지원 UWP app를 디버깅 합니다.

대상 응용 프로그램 시작 하위 프로세스를 디버깅 하려면 먼저 ``WinDbg``.

```
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

에 있는 ``WinDbg`` 표시 자식 디버깅을 사용 하도록 설정 하 고 적절 한 중단점을 설정 합니다.

```
.childdbg 1
g
```
(대상 응용 프로그램이 시작 되 고 디버거에서 실행을 중단 될 때까지 실행)

```
sxe ld fixup.dll
g
```
(DLL이 로드 되 고 수정 될 때까지 실행)

```
bp ...
```

>[!NOTE]
> [PLMDebug](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/plmdebug) 시작 시 응용 프로그램에 디버거를 연결 하도 사용할 수 있으며 [Windows 용 디버깅 도구에](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)에서 포함 되어 있습니다.  그러나 이제 WinDbg에서 제공 하는 직접 지원 보다 사용 하는 것이 복잡해 집니다.

## <a name="support-and-feedback"></a>지원 및 피드백

**질문에 대한 답변 찾기**

질문이 있으세요? Stack Overflow에서 질문해 주세요. 저희 팀은 이러한 [태그](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다. [여기](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)에서 Microsoft에 문의할 수도 있습니다.
