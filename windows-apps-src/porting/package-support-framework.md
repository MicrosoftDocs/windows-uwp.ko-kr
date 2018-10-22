---
author: normesta
Description: Fix issues that prevent your desktop application from running in an MSIX container
Search.Product: eADQiWindows 10XVcnh
title: MSIX 컨테이너에서 실행 되는 데스크톱 응용 프로그램을 방해 하는 문제 해결
ms.author: normesta
ms.date: 07/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1c8e4649fad0e467e0656415e2fd49a9fea05109
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/22/2018
ms.locfileid: "5396183"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>패키지 지원 프레임 워크를 사용 하 여 MSIX 패키지로 런타임 수정 적용

MSIX 컨테이너에서 실행 될 수 있도록 소스 코드에 액세스할 수 없는 경우 수정 기존 win32 응용 프로그램에 적용 하는 데 도움이 되는 오픈 소스 키트 있는 패키지 지원 프레임 워크입니다. 패키지 지원 프레임 워크를 사용 하면 응용 프로그램의 최신 런타임 환경 모범 사례를 따릅니다.

자세한 내용은 [지원 프레임 워크 패키지를](https://docs.microsoft.com/windows/msix/package-support-framework-overview)참조 하세요.

이 가이드를 응용 프로그램 호환성 문제를 식별 하는 데 도움이 됩니다 하 고 해결 하는 해결을 찾아를 적용 하 고 런타임 확장 합니다.

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>패키지 된 응용 프로그램 호환성 문제를 식별

먼저 응용 프로그램에 대 한 패키지를 만듭니다. 그런 다음 설치 하 고 실행 동작을 관찰 합니다. 호환성 문제를 식별 하는 데 도움이 되는 오류 메시지가 나타날 수 있습니다. 또한 문제를 식별 [프로세스 모니터](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) 를 사용할 수 있습니다.  일반적인 문제에 대 한 작업 디렉터리 및 프로그램 경로 사용 권한 응용 프로그램 가정 관련이 있습니다.

### <a name="using-process-monitor-to-identify-an-issue"></a>프로세스 모니터를 사용 하 여 문제를 식별 하기

[프로세스 모니터](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) 는 앱의 파일 및 레지스트리 작업의 결과 관찰 하기 위한 강력한 유틸리티입니다.  응용 프로그램 호환성 문제를 이해 하는 데 도움이 수 있습니다.  프로세스 모니터를 연 다음 추가 필터 (필터 >... 필터) 응용 프로그램 실행 파일에서 이벤트만 포함 하도록 합니다.

![ProcMon 앱 필터](images/desktop-to-uwp/procmon_app_filter.png)

이벤트의 목록이 표시 됩니다. 이러한 이벤트의 많은 단어 **성공** 나타납니다 **결과** 열.

![ProcMon 이벤트](images/desktop-to-uwp/procmon_events.png)

필요에 따라 오류만 표시 하도록 이벤트를 필터링 할 수 있습니다.

![ProcMon 제외 성공](images/desktop-to-uwp/procmon_exclude_success.png)

파일 시스템 액세스 오류가 것 같으면, System32/SysWOW64 또는 패키지 파일 경로 아래에 있는 실패 한 이벤트를 검색 합니다. 필터 수도 여기서도 도움이 됩니다. 이 목록의 맨 아래에서 시작 하 고 위쪽으로 스크롤하십시오. 이 목록의 맨 아래에 표시 되는 오류 발생 한 가장 최근에 합니다. "액세스 거부" 같은 문자열을 포함 하는 오류 및 "경로/이름 찾을 수 없음", 대부분의 주의 기울이는 및 의심 스러운 보이지 않을 것을 무시 합니다. [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/) 에 두 가지 문제가 있습니다. 다음 그림에 표시 되는 목록에 이러한 문제를 확인할 수 있습니다.

![ProcMon Config.txt](images/desktop-to-uwp/procmon_config_txt.png)

이 이미지에 표시 되는 첫 번째 문제를 "C:\Windows\SysWOW64" 경로에 있는 "Config.txt" 파일에서 읽는 응용 프로그램이 실패 했습니다. 응용 프로그램은 해당 경로 직접 참조 하려고 가능성이 아닙니다. 상대 경로 사용 하 여 해당 파일에서 읽은 하려고 하는 대부분의 경우, 기본적으로 "System32/SysWOW64"은 응용 프로그램의 작업 디렉터리 및 합니다. 이 응용 프로그램 패키지의 다른 곳으로 설정 해야 현재 작업 디렉터리의 예상는 것이 좋습니다. Appx 내에서 찾고, 실행 파일과 동일한 디렉터리에 파일이 있는지 볼 수 있습니다.

![앱 Config.txt](images/desktop-to-uwp/psfsampleapp_config_txt.png)

두 번째 문제는 다음 그림에 표시 됩니다.

![ProcMon 로그 파일](images/desktop-to-uwp/procmon_logfile.png)

이 문제에 응용 프로그램 파일을 작성할.log 해당 패키지 경로에 실패 했습니다. 이 것이 좋습니다 파일 리디렉션 수정 도움이 될 수 있습니다.

<a id="find" />

## <a name="find-a-runtime-fix"></a>런타임 수정 사항 찾기

PSF 파일 리디렉션 수정을 같은 지금 바로 사용할 수 있는 런타임 수정 사항이 포함 되어 있습니다.

### <a name="file-redirection-fixup"></a>파일 리디렉션 수정

MSIX 컨테이너에서 실행 되는 응용 프로그램에서 액세스할 수 없는 디렉터리에 데이터를 읽거나 쓰기 하려고 리디렉션할 수 있으며 [파일 리디렉션 수정 하기 위해](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim) 사용할 수 있습니다.

예를 들어 응용 프로그램 실행 파일 응용 프로그램와 동일한 디렉터리에 있는 로그 파일에 쓰는, 경우 [파일 리디렉션 수정](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim) 를 사용 하 여 로컬 앱 데이터 저장소 등의 다른 위치에 해당 로그 파일을 만들 수 있습니다.

### <a name="runtime-fixes-from-the-community"></a>커뮤니티에서 런타임 수정

커뮤니티 기여 하려면 [GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop) 페이지를 검토 해야 합니다. 다른 개발자가 유사한 문제 해결 하 고 런타임 수정 공유한 가능 합니다.

## <a name="apply-a-runtime-fix"></a>런타임 수정 적용

Windows SDK에서 및 다음 단계를 수행 하 여 몇 가지 간단한 도구를 사용 하 여 기존 런타임 수정 프로그램을 적용할 수 있습니다.

> [!div class="checklist"]
> * 패키지 레이아웃 폴더 만들기
> * 지원 프레임 워크 패키지 파일을 가져오려면
> * 패키지에 추가
> * 패키지 매니페스트 수정
> * 구성 파일 만들기

각 작업 진행 하겠습니다.

### <a name="create-the-package-layout-folder"></a>패키지 레이아웃 폴더 만들기

.Msix (또는.appx) 파일이 이미 있는 경우 패키지에 대 한 준비 영역으로 지원할 수 있는 레이아웃 폴더에 해당 콘텐츠 압축을 푸는 수 있습니다.  이렇게 하려면 프로그램 **x64 VS 2017 용 네이티브 도구 명령 프롬프트**, 또는 실행 검색 경로에 SDK bin 경로 사용 하 여 수동으로 합니다.

```
makemsix unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.msix /d PackageContents

```

이렇게 하면 다음과 같은 것입니다.

![패키지 레이아웃](images/desktop-to-uwp/package_contents.png)

가 없는.msix (또는.appx) 파일을 시작 하는 경우 처음부터 패키지 폴더 및 파일을 만들 수 있습니다.

### <a name="get-the-package-support-framework-files"></a>지원 프레임 워크 패키지 파일을 가져오려면

Visual Studio를 사용 하 여 PSF Nuget 패키지를 가져올 수 있습니다. 독립 실행형 Nuget 명령줄 도구를 사용 하 여 얻을 수 있습니다.

#### <a name="get-the-package-by-using-visual-studio"></a>Visual Studio를 사용 하 여 패키지 가져오기

Visual Studio에서 프로젝트 또는 솔루션에 노드를 마우스 오른쪽 단추로 클릭 하 고 Nuget 패키지 관리 명령 중 하나를 선택 합니다.  **Microsoft.PackageSupportFramework** 또는 **PSF** Nuget.org에서 패키지를 찾을 수를 검색 합니다. 그런 다음 설치 합니다.

#### <a name="get-the-package-by-using-the-command-line-tool"></a>명령줄 도구를 사용 하 여 패키지 가져오기

이 위치에서 Nuget 명령줄 도구를 설치: https://www.nuget.org/downloads. 그런 다음 Nuget 명령줄에서이 명령을 실행 합니다.

```
nuget install Microsoft.PackageSupportFramework
```

### <a name="add-the-package-support-framework-files-to-your-package"></a>패키지에 지원 프레임 워크 패키지 파일 추가

패키지 디렉터리에 필요한 32 비트 및 64 비트 PSF Dll 및 실행 파일을 추가 합니다. 다음 표를 가이드로 따르세요. 또한 필요한 런타임 수정 사항이 포함 합니다. 이 예제에서는 파일 리디렉션 런타임 수정을 해야합니다.

| 응용 프로그램 실행 파일은 x64 | 응용 프로그램 실행 파일은 x86 |
|-------------------------------|-----------|
| [PSFLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |  [PSFLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |
| [PSFRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) | [PSFRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) |
| [PSFRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) | [PSFRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) |

패키지 콘텐츠는 이제 모양은 다음과 같습니다.

![패키지 바이너리](images/desktop-to-uwp/package_binaries.png)

### <a name="modify-the-package-manifest"></a>패키지 매니페스트 수정

텍스트 편집기에서 패키지 매니페스트를 열고 다음을 설정 합니다 `Executable` 의 특성은 `Application` PSF 시작 관리자 실행 파일의 이름 지정할 요소를 합니다.  대상 응용 프로그램의 아키텍처를 알고 있는 경우 PSFLauncher32.exe 또는 PSFLauncher64.exe 적절 한 버전을 선택 합니다.  그렇지 않으면 PSFLauncher32.exe 모든 경우에 적합 합니다.  예를 들면 다음과 같습니다.

```xml
<Package ...>
  ...
  <Applications>
    <Application Id="PSFSample"
                 Executable="PSFLauncher32.exe"
                 EntryPoint="Windows.FullTrustApplication">
      ...
    </Application>
  </Applications>
</Package>
```

### <a name="create-a-configuration-file"></a>구성 파일 만들기

파일 이름을 만들기 ``config.json``, 해당 파일을 패키지의 루트 폴더에 저장 합니다. 방금 대체 하는 실행 파일을 가리키도록 config.json 파일의 선언 된 응용 프로그램 ID를 수정 합니다. 프로세스 모니터를 사용 하 여에서 얻은 정보를 사용 하도 작업 디렉터리를 설정 뿐 아니라 수 파일 리디렉션 수정을 사용 하 여 패키지 상대 "PSFSampleApp" 디렉터리 아래.log 파일을 읽기/쓰기를 리디렉션하 합니다.

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
            "fixups": [
                {
                    "dll": "FileRedirectionFixup.dll",
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
| 응용 프로그램 | id |  값을 사용 하는 `Id` 의 특성의 `Application` 패키지 매니페스트에 요소. |
| 응용 프로그램 | 실행 | 시작 하려는 실행 파일의 패키지 상대 경로입니다. 대부분의 경우, 수정 하기 전에 패키지 매니페스트 파일에서이 값을 얻을 수 있습니다. 값은 `Executable` 의 특성은 `Application` 요소. |
| 응용 프로그램 | workingDirectory | (선택 사항) 시작 하는 응용 프로그램의 작업 디렉터리를 사용 하 여 패키지 상대 경로입니다. 이 값을 설정 하지 않으면, 운영 체제에서 사용 합니다 `System32` 응용 프로그램의 작업 디렉터리로 디렉터리입니다. |
| 프로세스 | 실행 | 대부분에서의 이름이 됩니다 합니다 `executable` 위에 제거 경로 및 파일 확장명으로 구성 합니다. |
| 수정 | dll | .Msix/.appx 로드를 수정 하기 위해 패키지 상대 경로입니다. |
| 수정 | 구성 | (선택 사항) 수정 메일 그룹 동작 하는 방법을 제어 합니다. 이 값의 정확한 형식을 각 수정이 "blob" 해석할 수 원하는 대로 수정 하 여 수정 별로 달라 집니다. |

`applications`, `processes`, 및 `fixups` 키는 배열입니다. 즉,는 둘 이상의 응용 프로그램, 프로세스 및 수정을 DLL 지정 하려면 config.json 파일을 사용할 수 있습니다.


### <a name="package-and-test-the-app"></a>패키지 및 앱 테스트

그런 다음 패키지를 만듭니다.

```
makeappx pack /d PackageContents /p PSFSamplePackageFixup.msix
```

그런 다음 서명 합니다.

```
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.msix
```

자세한 내용은 참조 [패키지 서명 인증서를 만드는](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate) 방법과 [signtool을 사용 하 여 패키지에 서명 하는 방법](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

PowerShell을 사용 하 여 패키지를 설치 합니다.

>[!NOTE]
> 먼저 패키지를 제거 해야 합니다.

```
powershell Add-MSIXPackage .\PSFSamplePackageFixup.msix
```

응용 프로그램을 실행 하 고 런타임 수정 적용 된 동작을 확인 합니다.  진단 및 필요에 따라 패키징 단계를 반복 합니다.

### <a name="use-the-trace-fixup"></a>추적 수정을 사용합니다

패키지 된 응용 프로그램 호환성 문제를 진단 하는 대체 기술을 추적 수정을 사용 하는 것입니다. 이 DLL의 PSF 포함 이며 앱의 동작을 유사한 프로세스 모니터의 진단 상세 보기를 제공 합니다.  응용 프로그램 호환성 문제를 표시 하도록 특별히 설계 되었습니다.  추적 수정을 사용 하를 추가 하는 DLL 패키지에 다음 조각에 config.json를 추가 및 다음 패키지 응용 프로그램을 설치 합니다.

```json
{
    "dll": "TraceFixup.dll",
    "config": {
        "traceLevels": {
            "filesystem": "allFailures"
        }
    }
}
```

기본적으로 수정 추적 하기 위해 "예상"으로 간주할 수도 있는 오류를 필터링 합니다.  예를 들어, 응용 프로그램 무조건적으로 확인 하는 경우 이미 있는 결과 무시 하지 않고 파일을 삭제 하려고 할 수 있습니다. 이 파일 시스템 기능에서 모든 오류를 수신 하도록 옵트인 하는 위의 예제에서는 있으므로 맬웨어 그 결과 로그 아웃 일부 예기치 않은 오류를 필터링 가져올 수 있습니다. 그 전에 Config.txt 파일에서 읽는 시도 실패 하는 메시지 "파일이 없습니다"에서 알고 있으므로이 수행 합니다. 이 자주 관찰 되 고 하지 일반적으로 예상 된 것으로 간주 되는 실패 합니다. 실제로 것 가능성이 가장 예기치 않은 오류를 에게만 필터링 및 다음 폴백 모든 오류를 여전히 식별할 수 없는 문제가 있으면 시작 합니다.

기본적으로 연결 된 디버거에 추적 수정을에서 출력 전송 가져옵니다. 이 예제에서는 디버거를 첨부할 생각 하 고 출력을 보려면 SysInternals에서 [DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview) 프로그램을 대신 사용 됩니다. 앱을 실행 한 후 알 수 있습니다 동일한 오류 이전과는 가리킵니다 우리는 동일한 런타임 수정 합니다.

![TraceShim 파일을 찾을 수 없습니다.](images/desktop-to-uwp/traceshim_filenotfound.png)

![TraceShim 액세스 거부 됨](images/desktop-to-uwp/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>디버그, 연장 하거나, 런타임 수정 만들기

Visual Studio를 사용 하 여 런타임 수정 디버그, 런타임 수정, 확장 하거나 처음부터 새로 만들 수 있습니다. 성공 하려면 다음이 작업을 수행 해야 합니다.

> [!div class="checklist"]
> * 패키징 프로젝트 추가
> * 런타임 수정에 대 한 프로젝트 추가
> * 실행 PSF 시작 관리자를 시작 하는 프로젝트 추가
> * 패키징 프로젝트를 구성

를 완료 한 경우 솔루션은 다음과 같이 보입니다.

![완료 된 솔루션](images/desktop-to-uwp/runtime-fix-project-structure.png)

이 예제에서 각 프로젝트에 대해 살펴보겠습니다.

| Project | 목적 |
|-------|-----------|
| DesktopApplicationPackage | 이 프로젝트는 [Windows 응용 프로그램 패키징 프로젝트](desktop-to-uwp-packaging-dot-net.md) 를 기반으로 하며 MSIX 패키지 출력 합니다. |
| Runtimefix | 런타임 수정으로 사용 되는 하나 이상의 대체 함수를 포함 하는 c + + Dynamic-Linked 라이브러리 프로젝트입니다. |
| PSFLauncher | C + + 빈 프로젝트입니다. 이 프로젝트는 패키지 지원 프레임 워크의 런타임 배포 파일을 수집 하는 위치입니다. 실행 파일을 출력 합니다. 해당 실행 파일은 솔루션을 시작할 때 실행 되는 먼저 합니다. |
| WinFormsDesktopApplication | 이 프로젝트는 데스크톱 응용 프로그램의 소스 코드를 포함 합니다. |

이러한 유형의 프로젝트의 모든 포함 된 전체 샘플을 보면 [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/)를 참조 하세요.

솔루션의 구성 이들 각이 프로젝트를 만드는 단계를 살펴보겠습니다.


### <a name="create-a-package-solution"></a>패키지 솔루션 만들기

데스크톱 응용 프로그램에 대 한 솔루션 없는 경우 Visual Studio에서 새 **빈 솔루션** 을 만듭니다.

![빈 솔루션](images/desktop-to-uwp/blank-solution.png)

수 있는 모든 응용 프로그램 프로젝트에 추가할 수도 있습니다.

### <a name="add-a-packaging-project"></a>패키징 프로젝트 추가

**Windows 응용 프로그램 패키징 프로젝트**를 아직 없는 경우 하나 만들고 솔루션에 추가 합니다.

![패키지 프로젝트 템플릿](images/desktop-to-uwp/package-project-template.png)

Windows 응용 프로그램 패키징 프로젝트에 대 한 자세한 내용은 [Visual Studio를 사용 하 여 응용 프로그램 패키지](desktop-to-uwp-packaging-dot-net.md)를 참조 하세요.

**솔루션 탐색기**패키징 프로젝트를 마우스 오른쪽 단추로 클릭, **편집**, 선택 하 고이 프로젝트 파일의 맨 아래에 추가:

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

### <a name="add-project-for-the-runtime-fix"></a>런타임 수정에 대 한 프로젝트 추가

C + + **동적 연결 라이브러리 (DLL)** 프로젝트를 솔루션에 추가 합니다.

![런타임 수정 라이브러리](images/desktop-to-uwp/runtime-fix-library.png)

마우스 오른쪽 단추로 클릭은 프로젝트를 만들어서 다음 **속성**을 선택 합니다.

속성 페이지에서 **c + + 언어 표준** 필드를 찾은 다음 해당 필드 옆에 있는 드롭다운 목록에서 선택 합니다 **ISO c++17 표준 (/ (/std:c++17 + + 17)** 옵션입니다.

![17 ISO 옵션](images/desktop-to-uwp/iso-option.png)

해당 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 상황에 맞는 메뉴에서 **Nuget 패키지 관리** 옵션을 선택 합니다. **패키지 원본** 옵션 **모든** 또는 **nuget.org**로 설정 되어 있는지 확인 합니다.

해당 필드 옆 설정 아이콘을 클릭 합니다.

*PSF*검색 * Nuget 패키지를 선택한 다음이 프로젝트에 대 한 설치 합니다.

![nuget 패키지](images/desktop-to-uwp/psf-package.png)

디버그 하거나 기존 런타임 수정 프로그램을 확장 하려는 경우이 가이드의 [런타임 수정 사항 찾기](#find) 섹션에 설명 된 지침을 사용 하 여 얻은 런타임 수정 파일을 추가 합니다.

새로운 수정 만들려는 경우 추가 하지 마세요 아무 것도이 프로젝트에 아직 합니다. 이 가이드의 뒷부분에서이 프로젝트에 올바른 파일을 추가 도와 드립니다. 지금은 솔루션 설정을 계속 진행 됩니다.

### <a name="add-a-project-that-starts-the-psf-launcher-executable"></a>실행 PSF 시작 관리자를 시작 하는 프로젝트 추가

C + + **프로젝트 빈** 프로젝트를 솔루션에 추가 합니다.

![빈 프로젝트](images/desktop-to-uwp/blank-app.png)

이전 섹션에 설명 된 동일한 지침을 사용 하 여이 프로젝트를 **PSF** Nuget 패키지를 추가 합니다.

Open **일반** 설정 페이지에서 프로젝트에 대 한 속성 페이지의 **대상 이름** 속성을 설정 ``PSFLauncher32`` 또는 ``PSFLauncher64`` 응용 프로그램의 아키텍처에 따라 합니다.

![PSF 시작 관리자 참조](images/desktop-to-uwp/shim-exe-reference.png)

솔루션에서 런타임 수정 프로젝트에 대 한 프로젝트 참조를 추가 합니다.

![런타임 수정 참조](images/desktop-to-uwp/reference-fix.png)

참조를 마우스 오른쪽 단추로 클릭 하 고 **속성** 창에 이러한 값을 적용 합니다.

| 속성 | 값 |
|-------|-----------|
| 로컬 복사 | True |
| 로컬 위성 어셈블리를 복사 합니다. | True |
| 참조 어셈블리 출력 | True |
| 라이브러리 종속성 링크 | False |
| 링크 라이브러리 종속성 입력 | False |

### <a name="configure-the-packaging-project"></a>패키징 프로젝트를 구성

패키징 프로젝트에서 **응용 프로그램** 폴더를 마우스 오른쪽 단추로 클릭하고 **참조 추가**를 선택합니다.

![프로젝트 참조 추가](images/desktop-to-uwp/add-reference-packaging-project.png)

PSF 시작 관리자 프로젝트 및 데스크톱 응용 프로그램 프로젝트를 선택 하 고 **확인** 단추를 선택 합니다.

![데스크톱 프로젝트](images/desktop-to-uwp/package-project-references.png)

>[!NOTE]
> 응용 프로그램에 소스 코드가 없는, PSF 시작 관리자 프로젝트를 선택 하기만 합니다. 구성 파일을 만들 때 실행 파일을 참조 하는 방법을 살펴보겠습니다.

**응용 프로그램** 노드에서 PSF 시작 관리자 응용 프로그램을 마우스 오른쪽 단추로 클릭 하 고 **진입점으로 설정**를 선택 합니다.

![진입점 설정](images/desktop-to-uwp/set-startup-project.png)

라는 파일 추가 ``config.json`` 패키징 프로젝트에 다음, 복사 및 파일에 다음 json 텍스트를 붙여넣습니다. **콘텐츠**를 **패키지 동작** 속성을 설정 합니다.

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
            "fixups": [
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
각 키에 대 한 값을 제공 합니다. 이 표를 가이드로 사용 합니다.

| 배열 | key | 값 |
|-------|-----------|-------|
| 응용 프로그램 | id |  값을 사용 하는 `Id` 의 특성의 `Application` 패키지 매니페스트에 요소. |
| 응용 프로그램 | 실행 | 시작 하려는 실행 파일의 패키지 상대 경로입니다. 대부분의 경우, 수정 하기 전에 패키지 매니페스트 파일에서이 값을 얻을 수 있습니다. 값은 `Executable` 의 특성은 `Application` 요소. |
| 응용 프로그램 | workingDirectory | (선택 사항) 시작 하는 응용 프로그램의 작업 디렉터리를 사용 하 여 패키지 상대 경로입니다. 이 값을 설정 하지 않으면, 운영 체제에서 사용 합니다 `System32` 응용 프로그램의 작업 디렉터리로 디렉터리입니다. |
| 프로세스 | 실행 | 대부분에서의 이름이 됩니다 합니다 `executable` 위에 제거 경로 및 파일 확장명으로 구성 합니다. |
| 수정 | dll | 로드 DLL 수정 패키지 상대 경로입니다. |
| 수정 | 구성 | (선택 사항) DLL 수정을 동작 하는 방법을 제어 합니다. 이 값의 정확한 형식을 각 수정이 "blob" 해석할 수 원하는 대로 수정 하 여 수정 별로 달라 집니다. |

확인을 완료 하는 경우에 ``config.json`` 파일은 다음과 같이 보입니다.

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
      "fixups": [ { "dll": "RuntimeFix.dll" } ]
    }
  ]
}

```

>[!NOTE]
> `applications`, `processes`, 및 `fixups` 키는 배열입니다. 즉,는 둘 이상의 응용 프로그램, 프로세스 및 수정을 DLL 지정 하려면 config.json 파일을 사용할 수 있습니다.

### <a name="debug-a-runtime-fix"></a>런타임 수정 디버그

Visual Studio에서 f5 키를 눌러 디버거를 시작 합니다.  먼저 시작 하는 차례로 대상 데스크톱 응용 프로그램을 시작 하는 PSF 시작 관리자 응용 프로그램입니다.  대상 데스크톱 응용 프로그램을 디버깅 하려면 수동으로 **디버그**선택 하 여 데스크톱 응용 프로그램 프로세스에 연결 해야 합니다->**프로세스에 연결**하 고 다음 응용 프로그램 프로세스를 선택 합니다. 기본 런타임 수정을 DLL.NET 응용 프로그램의 디버깅을 허용 하려면 관리 코드와 네이티브 코드 형식 (혼합된 모드 디버깅)를 선택 합니다.  

했으므로이 설정한 후 데스크톱 응용 프로그램 코드 및 런타임 수정 프로젝트에서 줄의 코드로 옆에 중단점을 설정할 수 있습니다. 응용 프로그램에 소스 코드를 없다면 런타임 수정 프로젝트에서 줄의 코드로 옆에 중단점을 설정 하 여 됩니다.

>[!NOTE]
> Visual Studio는 가장 간단한 개발 제공 되며, 디버깅, 일부의 제한이,이 가이드의 뒷부분에서 적용할 수 있는 디버깅 기술 다른 설명 하겠습니다.

## <a name="create-a-runtime-fix"></a>런타임 수정 만들기

런타임을 해결 하려면, 대체 함수를 작성 하 고 모든 구성 데이터를 포함 하 여 새 런타임 수정을 만들 수 있습니다 문제에 대 한 해결 아직 없는 경우는 타당 합니다. 각 부분에 대해 살펴보겠습니다.

### <a name="replacement-functions"></a>대체 기능

먼저, 응용 프로그램 MSIX 컨테이너에서 실행 되 면 호출이 실패 하는 함수를 식별 합니다. 그런 다음 런타임 관리자를 대신 호출 하 고 싶은 대체 함수를 만들 수 있습니다. 이렇게 하면 최신 런타임 환경 규칙에 맞는 동작 함수의 구현 바꿉니다 수 있습니다.

Visual Studio에서이 가이드 앞부분에서 만든 런타임 수정 프로젝트를 엽니다.

선언 합니다 ``FIXUP_DEFINE_EXPORTS`` 매크로 대 한 include 문을 추가 합니다 `fixup_framework.h` 각각의 맨 합니다. 런타임 수정 기능을 추가 하려는 CPP 파일입니다.

```c++
#define FIXUP_DEFINE_EXPORTS
#include <fixup_framework.h>
```
>[!IMPORTANT]
>있는지 확인 합니다 `FIXUP_DEFINE_EXPORTS` 매크로 include 문의 앞에 나타납니다.

함수의 동일한 서명을 포함 된 함수를 만드는 사용자가 수정 하려는 동작 합니다. 대체 하는 예제 함수는 다음과 같습니다 합니다 `MessageBoxW` 함수입니다.

```c++
auto MessageBoxWImpl = &::MessageBoxW;
int WINAPI MessageBoxWFixup(
    _In_opt_ HWND hwnd,
    _In_opt_ LPCWSTR,
    _In_opt_ LPCWSTR caption,
    _In_ UINT type)
{
    return MessageBoxWImpl(hwnd, L"SUCCESS: This worked", caption, type);
}

DECLARE_FIXUP(MessageBoxWImpl, MessageBoxWFixup);
```

호출 `DECLARE_FIXUP` 지도 `MessageBoxW` 함수에 새 대체 함수를 합니다. 응용 프로그램 호출 하려고 할 때의 `MessageBoxW` 함수를 호출 대체 함수 대신 합니다.

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>런타임 수정 함수를 재귀적 호출 으로부터 보호

선택적으로 적용할 수 있습니다 합니다 `reentrancy_guard` 형식 재귀적에에서 함수 호출 런타임 수정 으로부터 보호 하는 기능입니다.

에 대 한 대체 함수를 생성할 수 있습니다 예를 들어 합니다 `CreateFile` 함수입니다. 구현을 호출할 수도 있습니다 합니다 `CopyFile` 함수를 하지만 구현의 합니다 `CopyFile` 함수를 호출할 수는 `CreateFile` 함수. 이에 대 한 호출에서 무한 반복 주기 시킬 수는 `CreateFile` 함수.

대 한 자세한 내용은 `reentrancy_guard` [authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md) 를 참조 하세요.

### <a name="configuration-data"></a>구성 데이터

런타임 수정 구성 데이터를 추가 하려는 경우 추가할 것을 고려 하는 ``config.json``. 사용할 수 이런 식으로는 `FixupQueryCurrentDllConfig` 를 쉽게 해당 데이터를 구문 분석 합니다. 이 예제에서는 해당 구성 파일에서 문자열 및 부울 값을 구문 분석 합니다.

```c++
if (auto configRoot = ::FixupQueryCurrentDllConfig())
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

Visual Studio는 가장 간단한 개발 및 디버깅 경험을 제공 하는 동안 일부 제한이 있습니다.

먼저는.msix에서 설치 하는 것이 아니라 패키지 레이아웃 폴더 경로에서 느슨한 파일 배포 하 여 응용 프로그램을 실행 F5 디버깅 /.appx 패키지입니다.  일반적으로 레이아웃 폴더가 설치 된 패키지 폴더와 동일한 보안 제한을가지고 있지 않습니다. 따라서 런타임 수정 적용 하기 전에 패키지 경로 액세스 거부 오류를 재현 불가능할 수도 있습니다 것입니다.

이 문제를 해결 하기 위해.msix를 사용 하 여 F5 것이 아니라.appx 패키지 배포 느슨한 파일 배포 / 합니다.  .msix 만들려면.appx 패키지 파일을 / 위에서 설명한 대로 Windows SDK에서 [MakeMSIX](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) 유틸리티를 사용 합니다. 또는에서 Visual Studio 내에서 응용 프로그램 프로젝트 노드를 마우스 오른쪽 단추로 클릭 하 고 **스토어**선택->**앱 패키지 만들기**.

Visual Studio를 사용 하 여 다른 문제는 디버거를 통해 시작 된 모든 자식 프로세스에 연결에 대 한 기본 제공 지원이 필요가 없습니다.   이렇게 하면 시작 된 후 Visual Studio에서 수동으로 연결 되어 있어야 합니다 대상 응용 프로그램의 시작 경로에서 논리를 디버그 하기가 어렵습니다.

이 문제를 해결 하기 위해 하위 프로세스를 지 원하는 디버거를 사용 하 여 연결 합니다.  참고 아닌지 일반적으로 대상 응용 프로그램에 적시에 (JIT) 디버거를 첨부할 수 있습니다.  대부분의 JIT 기술 관련 ImageFileExecutionOptions 레지스트리 키를 통해 대상 앱을 대신 디버거 실행 때문입니다.  이 방법을 detouring 메커니즘 PSFLauncher.exe FixupRuntime.dll 대상 앱에 삽입 하는 데 사용 합니다.  WinDbg, [Windows 용 디버깅 도구](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)포함 되어 있으며 [Windows SDK](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk)에서 가져온 지원 하위 프로세스를 연결 합니다.  또한 지원 직접 [시작 하 고 UWP 앱을 디버깅](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app)합니다.

대상 응용 프로그램 시작 하위 프로세스를 디버깅 하려면 시작 ``WinDbg``.

```
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

에 ``WinDbg`` , 묻는 메시지를 표시 하 고, 디버깅 자식 활성화 하 고, 적절 한 중단점을 설정 합니다.

```
.childdbg 1
g
```
(대상 응용 프로그램 시작 하 고 디버거를 중단 될 때까지 실행)

```
sxe ld fixup.dll
g
```
(수정을 DLL이 로드 될 때까지 실행)

```
bp ...
```

>[!NOTE]
> [PLMDebug](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/plmdebug) 앱 시작 시에 디버거를 추가할도 사용할 수 있으며 [Windows 용 디버깅 도구에](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)에서 포함 되어 있습니다.  하지만 이제 WinDbg에서 제공 하는 직접 지원 보다 사용 하기 더 복잡 한 것입니다.

## <a name="support-and-feedback"></a>지원 및 피드백

**질문에 대한 답변 찾기**

질문이 있으세요? Stack Overflow에서 질문해 주세요. 저희 팀은 이러한 [태그](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다. [여기](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)에서 Microsoft에 문의할 수도 있습니다.

