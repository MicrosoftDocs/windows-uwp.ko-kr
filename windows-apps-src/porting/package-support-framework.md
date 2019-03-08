---
Description: MSIX 컨테이너에서 실행 하 여 데스크톱 응용 프로그램을 방해 하는 문제 해결
Search.Product: eADQiWindows 10XVcnh
title: MSIX 컨테이너에서 실행 하 여 데스크톱 응용 프로그램을 방해 하는 문제 해결
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 80f9c8bad9445bd9cfef9b09c00f99929fda37aa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590728"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>패키지 지원 프레임 워크를 사용 하 여 런타임 수정 MSIX 패키지에 적용

패키지 지원 프레임 워크 MSIX 컨테이너에서 실행할 수 있도록 소스 코드에 액세스할 수 없을 때 수정 기존 win32 응용 프로그램에 적용 하는 데 도움이 되는 오픈 소스 키트입니다. 패키지 지원 프레임 워크를 통해 최신 런타임 환경의 모범 사례에 따라 응용 프로그램을 수 있습니다.

자세한 내용은 참조 하세요 [패키지 지원 프레임 워크](https://docs.microsoft.com/windows/msix/package-support-framework-overview)합니다.

이 가이드에서는 응용 프로그램 호환성 문제를 식별 하 고 찾고, 적용, 런타임을 확장 하 고이 해결 하는 수정.

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>패키지에 포함 된 응용 프로그램 호환성 문제를 식별 합니다.

먼저 응용 프로그램에 대 한 패키지를 만듭니다. 다음을 설치 하 고 실행 한 다음 해당 동작을 관찰 합니다. 호환성 문제를 식별 하는 데 도움이 되는 오류 메시지가 나타날 수 있습니다. 사용할 수도 있습니다 [Process Monitor](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) 문제를 식별 합니다.  일반적인 문제 프로그램 경로 권한과 작업 디렉터리에 대 한 응용 프로그램 가정을 관련이 있습니다.

### <a name="using-process-monitor-to-identify-an-issue"></a>프로세스 모니터를 사용 하 여 문제를 식별 하려면

[처리할 모니터](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) 는 앱의 파일 및 레지스트리 작업 결과 관찰 하는 것에 대 한 강력한 유틸리티입니다.  이 응용 프로그램 호환성 문제를 이해 하면 도움이 됩니다.  프로세스 모니터를 연 후 추가 필터 (필터 > 필터...)은 응용 프로그램 실행 파일의 이벤트만 포함 하도록 합니다.

![ProcMon 앱 필터](images/desktop-to-uwp/procmon_app_filter.png)

이벤트의 목록이 표시 됩니다. 이러한 이벤트는 word의 많은 **성공** 에 표시 됩니다는 **결과** 열입니다.

![ProcMon 이벤트](images/desktop-to-uwp/procmon_events.png)

필요에 따라 실패만 표시 하도록 이벤트를 필터링 할 수 있습니다.

![ProcMon 제외 성공](images/desktop-to-uwp/procmon_exclude_success.png)

파일 시스템 액세스 실패를 생각 하는 경우 System32/SysWOW64 또는 패키지 파일 경로 아래에 있는 실패 한 이벤트를 검색 합니다. 너무 필터 여기서 도움이 수도 있습니다. 이 목록의 맨 아래에서 시작 하 고 위쪽으로 스크롤하십시오. 이 목록의 맨 아래에 나타나는 오류 가장 최근에 발생 합니다. "액세스 거부"와 같은 문자열을 포함 하는 오류와 "경로/이름 not found", 대부분의 주의 하 고 의심 스러운 보이지 않을 것을 무시 합니다. 합니다 [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/) 에 두 가지 문제가 있습니다. 다음 그림에 표시 된 목록에서 이러한 문제를 확인할 수 있습니다.

![ProcMon Config.txt](images/desktop-to-uwp/procmon_config_txt.png)

이 이미지에 표시 되는 첫 번째 문제에 응용 프로그램 "C:\Windows\SysWOW64" 경로에 있는 "Config.txt" 파일에서 읽기에 실패 했습니다. 그럴 가능성은 응용 프로그램은 해당 경로 직접 참조 하려고 합니다. 대부분의 경우 상대 경로 사용 하 여 해당 파일에서 읽기를 시도 하는 이며 기본적으로 "System32/SysWOW64" 응용 프로그램의 작업 디렉터리입니다. 이 응용 프로그램의 패키지의 위치 설정 해야 해당 현재 작업 디렉터리를 예상 하는 것을 제안 합니다. Appx를 내에서 찾고, 실행 파일과 동일한 디렉터리에 파일이 있는지 볼 수 있습니다.

![앱 Config.txt](images/desktop-to-uwp/psfsampleapp_config_txt.png)

두 번째 문제는 다음 이미지에 표시 됩니다.

![ProcMon 로그 파일](images/desktop-to-uwp/procmon_logfile.png)

이번이 호에서는 응용 프로그램 패키지 경로.log 파일을 작성 하지 못했습니다. 이 것이 좋습니다 파일 리디렉션 픽스업 도움이 될 수 있습니다.

<a id="find" />

## <a name="find-a-runtime-fix"></a>런타임 수정 사항 찾기

PSF 파일 리디렉션 픽스업 등 지금 바로 사용할 수 있는 런타임 수정 프로그램이 포함 되어 있습니다.

### <a name="file-redirection-fixup"></a>파일 리디렉션 수정

사용할 수는 [파일 리디렉션 픽스업](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup) 쓰거나 MSIX 컨테이너에서 실행 되는 응용 프로그램에서 액세스할 수 없는 디렉터리 데이터 읽기 시도 리디렉션할 수입니다.

예를 들어, 응용 프로그램 실행 파일 응용 프로그램과 동일한 디렉터리에 있는 로그 파일에 쓰는 사용할 수 있습니다 합니다 [파일 리디렉션 픽스업](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup) 로컬 앱 데이터 저장소 등의 다른 위치에서 해당 로그 파일을 만들 수 있습니다.

### <a name="runtime-fixes-from-the-community"></a>커뮤니티에서 런타임 수정

커뮤니티 기여를 검토 해야 우리의 [GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework) 페이지입니다. 다른 개발자가 자신과 유사한 문제를 해결 했으면을 런타임 수정 공유한 것 같습니다.

## <a name="apply-a-runtime-fix"></a>런타임 수정 적용

Windows SDK에서 다음이 단계를 수행 하 여 몇 가지 간단한 도구를 사용 하 여 기존 런타임 픽스를 적용할 수 있습니다.

> [!div class="checklist"]
> * 패키지 레이아웃 폴더 만들기
> * 패키지 지원 프레임 워크 파일 가져오기
> * 패키지에 추가
> * 패키지 매니페스트 수정
> * 구성 파일 만들기

각 태스크를 진행 해 보겠습니다.

### <a name="create-the-package-layout-folder"></a>패키지 레이아웃 폴더 만들기

.Msix (또는.appx) 파일이 이미 있으면 패키지에 대 한 준비 영역으로 사용할 레이아웃 폴더에 내용이 압축을 풀 수 있습니다. SDK의 설치 경로에 따라 MakeAppx 도구를 사용 하 여 명령 프롬프트에서이 수행할 수 있습니다,이 경우 Windows 10 PC에서 makeappx.exe 도구를 알게 됩니다: x86: C:\Program Files (x86)\Windows Kits\10\bin\x86\makeappx.exe x64: C:\Program Files (x86)\Windows Kits\10\bin\x64\makeappx.exe

```ps
makeappx unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.msix /d PackageContents

```

이렇게 하면 다음과 같은 화면이 표시 됩니다.

![패키지 레이아웃](images/desktop-to-uwp/package_contents.png)

가 없는.msix (또는.appx) 파일을 시작 하는 경우 처음부터 패키지 폴더 및 파일을 만들 수 있습니다.

### <a name="get-the-package-support-framework-files"></a>패키지 지원 프레임 워크 파일 가져오기

독립 실행형 Nuget 명령줄 도구를 사용 하 여 또는 Visual Studio를 통해 PSF Nuget 패키지를 가져올 수 있습니다.

#### <a name="get-the-package-by-using-the-command-line-tool"></a>명령줄 도구를 사용 하 여 패키지 가져오기

이 위치에서 Nuget 명령줄 도구를 설치 합니다. https://www.nuget.org/downloads합니다. 그런 다음 Nuget 명령을 명령줄에서이 명령을 실행 합니다.

```ps
nuget install Microsoft.PackageSupportFramework
```

#### <a name="get-the-package-by-using-visual-studio"></a>Visual Studio를 사용 하 여 패키지 가져오기

Visual Studio에서 솔루션 또는 프로젝트 노드를 마우스 오른쪽 단추로 클릭 하 고 Nuget 패키지 관리 명령 중 하나를 선택 합니다.  검색할 **Microsoft.PackageSupportFramework** 하거나 **PSF** Nuget.org에서 패키지를 찾으려고 합니다. 그런 다음 설치 합니다.

### <a name="add-the-package-support-framework-files-to-your-package"></a>패키지에 패키지 지원 프레임 워크 파일을 추가 합니다.

패키지 디렉터리에 필요한 32 비트 및 64 비트 PSF Dll 및 실행 파일을 추가 합니다. 다음 표를 가이드로 따르세요. 또한 해야 하는 모든 런타임 수정 사항을 포함 합니다. 이 예에서 파일 리디렉션 런타임 수정을 해야합니다.

| 응용 프로그램 실행 파일은 x64 | 응용 프로그램 실행 파일은 x86 |
|-------------------------------|-----------|
| [PSFLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |  [PSFLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |
| [PSFRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) | [PSFRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) |
| [PSFRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) | [PSFRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) |

패키지 콘텐츠 같이 같아집니다.

![패키지 이진 파일](images/desktop-to-uwp/package_binaries.png)

### <a name="modify-the-package-manifest"></a>패키지 매니페스트 수정

설정한 후에 패키지 매니페스트, 텍스트 편집기에서 엽니다는 `Executable` 특성을 `Application` PSF 시작 관리자 실행 파일의 이름으로 요소입니다.  대상 응용 프로그램의 아키텍처를 알고 있는 경우에 PSFLauncher32.exe 또는 PSFLauncher64.exe 적절 한 버전을 선택 합니다.  그러지 않으면 PSFLauncher32.exe 모든 경우에 작동 합니다.  예를 들면 다음과 같습니다.

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

파일 이름을 만들 ``config.json``, 패키지의 루트 폴더에 해당 파일을 저장 합니다. 방금 바꾼 실행 파일을 가리키도록 config.json 파일의 선언 된 앱 ID를 수정 합니다. 프로세스 모니터를 사용 하 여 얻은 정보를 사용 하 있습니다 수도 작업 디렉터리를 설정와 함께 사용 파일 리디렉션 픽스업의 패키지 상대 "PSFSampleApp" 디렉터리에서.log 파일에 읽기/쓰기를 리디렉션할 수 있습니다.

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

| 배열 | 키 | 값 |
|-------|-----------|-------|
| 응용 프로그램 | id |  값을 사용 합니다 `Id` 특성을 `Application` 패키지 매니페스트에 요소. |
| 응용 프로그램 | 실행 파일 | 파일의 패키지 상대 경로 시작 하려는 실행 파일입니다. 대부분의 경우에서 수정 하기 전에 패키지 매니페스트 파일에서이 값을 가져올 수 있습니다. 값인 합니다 `Executable` 특성을 `Application` 요소입니다. |
| 응용 프로그램 | workingDirectory | (선택 사항) 시작 하는 응용 프로그램의 작업 디렉터리로 사용 하 여 패키지 상대 경로입니다. 운영 체제에 사용 하 여이 값을 설정 하지 않으면 경우는 `System32` 응용 프로그램의 작업 디렉터리와 디렉터리입니다. |
| 프로세스 | 실행 파일 | 대부분의 경우, 이름의 됩니다는 `executable` 제거 경로 및 파일 확장명을 사용 하 여 위에 구성 합니다. |
| 수정 | dll | 수정, 로드.msix/.appx 패키지 상대 경로입니다. |
| 수정 | 구성 | (선택 사항) 픽스업 dl의 동작 방식을 제어 합니다. 이 값의 정확한 형식을 각 픽스업이 "blob" 해석할 수 하려고 하는 대로 수정 하 여 수정 별로 달라 집니다. |

합니다 `applications`, `processes`, 및 `fixups` 키는 배열입니다. 즉, 둘 이상의 응용 프로그램, 프로세스 및 픽스업 DLL 지정 하려면 config.json 파일을 사용할 수 있습니다.

### <a name="package-and-test-the-app"></a>패키지 및 응용 프로그램 테스트

다음으로 패키지를 만듭니다.

```ps
makeappx pack /d PackageContents /p PSFSamplePackageFixup.msix
```

그런 다음 서명 합니다.

```ps
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.msix
```

자세한 내용은 [패키지 서명 인증서를 만드는 방법](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate) 고 [signtool을 사용 하 여 패키지를 서명 하는 방법](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

PowerShell을 사용 하는 패키지를 설치 합니다.

>[!NOTE]
> 패키지를 먼저 제거 해야 합니다.

```ps
powershell Add-AppPackage .\PSFSamplePackageFixup.msix
```

응용 프로그램을 실행 하 고 적용 하는 런타임 수정 동작을 관찰 합니다.  진단 및 필요에 따라 패키징 단계를 반복 합니다.

### <a name="use-the-trace-fixup"></a>사용 추적 수정

패키지 응용 프로그램 호환성 문제를 진단 하는 대체 기술을 추적 픽스업을 사용 하는 것입니다. 이 DLL을 PSF 포함 하 고 프로세스 모니터와 유사한 앱의 동작에 대해 자세한 진단 보기를 제공 합니다.  응용 프로그램 호환성 문제를 표시 하기 위해 특별히 설계 되었습니다.  사용 추적 픽스업, DLL을 패키지에 추가 프로그램 config.json 다음 조각을 추가 및 다음 패키지 및 응용 프로그램을 설치 합니다.

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

기본적으로 추적 픽스업 "예상"으로 간주 될 수 있는 오류를 필터링 합니다.  예를 들어, 응용 프로그램 무조건 확인 하는 경우 이미 있는 결과 무시 하지 않고 파일을 삭제 하려고 할 수 있습니다. 이 파일 시스템 함수에서 모든 오류를 수신 하도록 선택할 것 위의 예에서 있으므로 부적절 한 결과 일부 예기치 않은 오류 필터링 가져올 수 있습니다. 그 전에 Config.txt 파일에서 읽지는 메시지 "파일 not found"와 함께 실패에서 알고 있으므로이 작업을 수행 합니다. 이 자주 관찰 하 고 일반적으로 예상 되지 것으로 간주 하는 경우 오류가 발생 합니다. 실제로 가능성이 가장 예기치 않은 오류에만 필터링 하 고 다음 대체 모든 오류에도 식별할 수 없는 문제가 발생 하는 경우 시작 하는 것입니다.

기본적으로 연결 된 디버거에 추적 픽스업의 출력을 전송 합니다. 예를 들어, 디버거에 연결 되지 않습니다 하 고를 대신 사용 합니다 [DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview) 프로그램 출력을 보려면 sysinternals에서 합니다. 앱을 실행 한 후 보면 동일한 오류 이전과 동일한 런타임 수정으로 우리 지점과 같은 방식입니다.

![TraceShim 파일을 찾을 수 없습니다.](images/desktop-to-uwp/traceshim_filenotfound.png)

![TraceShim 액세스 거부 됨](images/desktop-to-uwp/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>디버그, 확장 또는 런타임 수정 만들기

런타임 수정 프로그램을 디버그, 런타임 수정 프로그램을 확장 또는 만들기부터 Visual Studio를 사용할 수 있습니다. 찾기가 성공 하려면 다음이을 수행 해야 합니다.

> [!div class="checklist"]
> * 패키징 프로젝트를 추가 합니다.
> * 런타임 수정에 대 한 프로젝트를 추가 합니다.
> * 실행 PSF 시작 관리자를 시작 하는 프로젝트를 추가 합니다.
> * 패키징 프로젝트를 구성 합니다.

완료 되 면이 같이 솔루션이 표시 됩니다.

![완성 된 솔루션](images/desktop-to-uwp/runtime-fix-project-structure.png)

이 예제에서 각 프로젝트에 살펴보겠습니다.

| Project | 용도 |
|-------|-----------|
| DesktopApplicationPackage | 이 프로젝트를 기반으로 합니다 [Windows 응용 프로그램 패키징 프로젝트](desktop-to-uwp-packaging-dot-net.md) MSIX 패키지를 출력 합니다. |
| Runtimefix | 이 역할을 런타임 문제를 해결 하는 하나 이상의 대체 함수를 포함 하는 c + + Dynamic-Linked 라이브러리 프로젝트입니다. |
| PSFLauncher | C + + 빈 프로젝트입니다. 이 프로젝트는 패키지 지원 프레임 워크의 런타임 배포 가능 파일을 수집 하는 위치입니다. 실행 파일을 출력 합니다. 실행 파일을 먼저 솔루션을 시작할 때 실행 되는 경우 |
| WinFormsDesktopApplication | 이 프로젝트에는 데스크톱 응용 프로그램의 소스 코드를 포함 합니다. |

참조 모두 이러한 유형의 프로젝트를 포함 하는 전체 예제를 살펴보려면 [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/)합니다.

만들기 및 솔루션에서 이러한 각 프로젝트를 구성 하는 단계를 살펴보겠습니다.

### <a name="create-a-package-solution"></a>패키지 솔루션 만들기

데스크톱 응용 프로그램에 대 한 솔루션이 없는 경우 새로 만듭니다 **빈 솔루션** Visual Studio에서.

![빈 솔루션](images/desktop-to-uwp/blank-solution.png)

있는 모든 응용 프로그램 프로젝트를 추가할 수도 있습니다.

### <a name="add-a-packaging-project"></a>패키징 프로젝트를 추가 합니다.

아직 없는 경우는 **Windows 응용 프로그램 패키징 프로젝트**를 하나 만들고 솔루션에 추가 합니다.

![패키지 프로젝트 템플릿](images/desktop-to-uwp/package-project-template.png)

Windows 응용 프로그램 패키징 프로젝트에 대 한 자세한 내용은 참조 하세요. [Visual Studio를 사용 하 여 응용 프로그램 패키지](desktop-to-uwp-packaging-dot-net.md)합니다.

**솔루션 탐색기**패키징 프로젝트를 마우스 오른쪽 단추로 클릭, 선택 **편집**를 추가한 다음이 프로젝트 파일의 맨 아래에:

```xml
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

### <a name="add-project-for-the-runtime-fix"></a>런타임 수정에 대 한 프로젝트를 추가 합니다.

C + + 추가 **동적 연결 라이브러리 (DLL)** 프로젝트를 솔루션입니다.

![런타임 수정 라이브러리](images/desktop-to-uwp/runtime-fix-library.png)

이 프로젝트를 선택한 다음 마우스 오른쪽 단추로 클릭 **속성**합니다.

속성 페이지를 찾을 합니다 **c + + 언어 표준을** 필드를 선택한 후 해당 필드 옆의 드롭다운 목록에서를 **ISO C + + 17 표준을 (/ /std: c + + 17)** 옵션입니다.

![17 ISO 옵션](images/desktop-to-uwp/iso-option.png)

해당 프로젝트를 마우스 오른쪽 단추로 클릭 한 후 상황에 맞는 메뉴에서 선택 합니다 **Nuget 패키지 관리** 옵션입니다. 있는지 확인 합니다 **패키지 소스** 옵션을 설정 **모든** 또는 **nuget.org**합니다.

해당 필드 다음 설정 아이콘을 클릭 합니다.

검색 된 *PSF** Nuget 패키지를 찾은 다음이 프로젝트에 설치 합니다.

![nuget 패키지](images/desktop-to-uwp/psf-package.png)

디버그 또는 기존 런타임 수정 프로그램을 확장 하려는 경우에 설명 된 지침을 사용 하 여 얻은 런타임 수정 프로그램 파일을 추가 합니다 [런타임 수정 사항 찾기](#find) 이 가이드의 섹션입니다.

새로운 수정 프로그램을 만들려는 경우 추가 하지 마세요 아무 것도이 프로젝트에 아직. 이 가이드의 뒷부분에서이 프로젝트에 올바른 파일을 추가 하 게 도울 것입니다. 이제 솔루션을 구성 설정이 계속 됩니다.

### <a name="add-a-project-that-starts-the-psf-launcher-executable"></a>실행 PSF 시작 관리자를 시작 하는 프로젝트를 추가 합니다.

C + + 추가 **빈 프로젝트** 프로젝트를 솔루션입니다.

![빈 프로젝트](images/desktop-to-uwp/blank-app.png)

추가 된 **PSF** 이전 섹션에서 설명한 동일한 지침을 사용 하 여이 프로젝트에 Nuget 패키지.

및 프로젝트 속성 페이지를 엽니다는 **일반** 설정 페이지에서 설정 된 **대상 이름** 속성을 ``PSFLauncher32`` 또는 ``PSFLauncher64`` 응용 프로그램의 아키텍처에 따라 합니다.

![PSF 시작 관리자 참조](images/desktop-to-uwp/shim-exe-reference.png)

솔루션에서 런타임 수정 프로젝트를 프로젝트 참조를 추가 합니다.

![런타임 수정 참조](images/desktop-to-uwp/reference-fix.png)

참조를 마우스 오른쪽 단추로 클릭 한 다음 합니다 **속성** 창에서 이러한 값을 적용 합니다.

| 속성 | 값 |
|-------|-----------|
| 로컬 복사 | True |
| 로컬 위성 어셈블리 복사 | True |
| 참조 어셈블리 출력 | True |
| 라이브러리 종속성 링크 | False |
| 링크 라이브러리 종속성 입력 | False |

### <a name="configure-the-packaging-project"></a>패키징 프로젝트를 구성 합니다.

패키징 프로젝트에서 **응용 프로그램** 폴더를 마우스 오른쪽 단추로 클릭하고 **참조 추가**를 선택합니다.

![프로젝트 참조 추가](images/desktop-to-uwp/add-reference-packaging-project.png)

PSF launcher 프로젝트 및 데스크톱 응용 프로그램 프로젝트를 선택 하 고 선택 합니다 **확인** 단추입니다.

![데스크톱 프로젝트](images/desktop-to-uwp/package-project-references.png)

>[!NOTE]
> 응용 프로그램 소스 코드를 없다면 PSF launcher 프로젝트를 선택 합니다. 구성 파일을 만들 때 실행 파일을 참조 하는 방법을 살펴보겠습니다.

에 **응용 프로그램** 노드를 PSF 시작 관리자 응용 프로그램을 마우스 오른쪽 단추로 클릭 하 고 선택한 **진입점으로 설정**합니다.

![진입점 설정](images/desktop-to-uwp/set-startup-project.png)

이라는 파일을 추가 ``config.json`` 패키징 프로젝트에 다음을 복사 및 파일에 다음 json 텍스트를 붙여넣습니다. 설정 된 **패키지 작업** 속성을 **콘텐츠**합니다.

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

각 키에 대 한 값을 제공 합니다. 이 테이블을 가이드로 사용 합니다.

| 배열 | 키 | 값 |
|-------|-----------|-------|
| 응용 프로그램 | id |  값을 사용 합니다 `Id` 특성을 `Application` 패키지 매니페스트에 요소. |
| 응용 프로그램 | 실행 파일 | 파일의 패키지 상대 경로 시작 하려는 실행 파일입니다. 대부분의 경우에서 수정 하기 전에 패키지 매니페스트 파일에서이 값을 가져올 수 있습니다. 값인 합니다 `Executable` 특성을 `Application` 요소입니다. |
| 응용 프로그램 | workingDirectory | (선택 사항) 시작 하는 응용 프로그램의 작업 디렉터리로 사용 하 여 패키지 상대 경로입니다. 운영 체제에 사용 하 여이 값을 설정 하지 않으면 경우는 `System32` 응용 프로그램의 작업 디렉터리와 디렉터리입니다. |
| 프로세스 | 실행 파일 | 대부분의 경우, 이름의 됩니다는 `executable` 제거 경로 및 파일 확장명을 사용 하 여 위에 구성 합니다. |
| 수정 | dll | DLL이 로드 수정 패키지 상대 경로입니다. |
| 수정 | 구성 | (선택 사항) 픽스업 DLL의 동작 방식을 제어 합니다. 이 값의 정확한 형식을 각 픽스업이 "blob" 해석할 수 하려고 하는 대로 수정 하 여 수정 별로 달라 집니다. |

완료 되 면 프로그램 ``config.json`` 파일은 다음과 같아야 합니다.

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
> 합니다 `applications`, `processes`, 및 `fixups` 키는 배열입니다. 즉, 둘 이상의 응용 프로그램, 프로세스 및 픽스업 DLL 지정 하려면 config.json 파일을 사용할 수 있습니다.

### <a name="debug-a-runtime-fix"></a>디버그 런타임 수정

Visual Studio에서 f5 키를 눌러 디버거를 시작 합니다.  가장 먼저 시작 하는 됩니다 PSF 시작 관리자 응용 프로그램을 차례로 대상 데스크톱 응용 프로그램을 시작 합니다.  대상 데스크톱 응용 프로그램을 디버깅 하려면 수동으로 선택 하 여 데스크톱 응용 프로그램 프로세스에 연결 해야 **디버깅할**->**프로세스에 연결**를 선택한 다음 응용 프로그램 프로세스입니다. 네이티브 런타임 수정 DLL 사용 하 여.NET 응용 프로그램의 디버깅을 허용할 관리 및 네이티브 코드 형식 (혼합된 모드 디버깅)을 선택 합니다.  

했으므로이 설정한 후에 데스크톱 응용 프로그램 코드 및 런타임 수정 프로젝트에서 코드 줄 옆에 있는 중단점을 설정할 수 있습니다. 응용 프로그램 소스 코드 없으면 런타임 수정 프로젝트에서 코드 줄 옆에 중단점을 설정 수 있습니다.

>[!NOTE]
> Visual Studio에서는 가장 간단한 개발 및 디버깅 환경을, 몇 가지 제한 사항이, 하는 동안이 가이드의 뒷부분에서 설명 하겠습니다 적용할 수 있는 다른 디버깅 기술을입니다.

## <a name="create-a-runtime-fix"></a>런타임 수정 만들기

해결 하려는 대체 함수를 작성 하 고 모든 구성 데이터를 포함 하 여 새 런타임 수정 프로그램을 만들 수 있습니다 문제에 대 한 런타임 수정 아직 없는 경우는 합리적입니다. 각 부분을 살펴보겠습니다.

### <a name="replacement-functions"></a>대체 함수

먼저, MSIX 컨테이너에서 응용 프로그램을 실행 하는 경우 호출에 실패 하는 함수를 식별 합니다. 그런 다음 런타임 관리자 대신 호출 하 고 싶은 대체 함수를 만들 수 있습니다. 이렇게 하면 최신 런타임 환경의 규칙을 준수 하는 동작을 사용 하 여 함수의 구현을 바꿀 수 있습니다.

Visual Studio에서이 가이드의 앞부분에서 만든 런타임 수정 프로젝트를 엽니다.

선언 합니다 ``FIXUP_DEFINE_EXPORTS`` 매크로 다음 include 문을 추가 합니다 `fixup_framework.h` 각각의 맨 위에 있는 합니다. 런타임 수정 기능을 추가 하려는 CPP 파일입니다.

```c++
#define FIXUP_DEFINE_EXPORTS
#include <fixup_framework.h>
```

>[!IMPORTANT]
>확인을 `FIXUP_DEFINE_EXPORTS` 매크로가 포함 문 앞에 나타납니다.

함수의 동일한 서명을 가진 하는 함수 만들기는 수정 하려는 동작 합니다. 다음은 대체 하는 함수 예제는 `MessageBoxW` 함수입니다.

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

에 대 한 호출 `DECLARE_FIXUP` 매핑하는 `MessageBoxW` 새 대체 함수에는 함수입니다. 응용 프로그램를 호출 하려고 하는 경우는 `MessageBoxW` 함수 호출 대체 함수 대신 합니다.

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>런타임 수정 함수에 대 한 재귀 호출을 방지

필요에 따라 적용할 수 있습니다는 `reentrancy_guard` 형식 런타임 수정 함수에 대 한 재귀 호출을 방지 하는 함수입니다.

에 대 한 대체 함수를 생성할 수 있습니다 예를 들어를 `CreateFile` 함수입니다. 구현을 호출할 수 있습니다는 `CopyFile` 함수 하지만 구현의 합니다 `CopyFile` 함수를 호출할 수 있습니다는 `CreateFile` 함수입니다. 에 대 한 호출을 무한 재귀 주기 발생할 수 있습니다는 `CreateFile` 함수입니다.

에 대 한 자세한 `reentrancy_guard` 참조 [authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>구성 데이터

런타임 수정 하려면 구성 데이터를 추가 하려는 경우 추가할 것을 고려 하 여 ``config.json``입니다. 이런 방식으로 사용할 수는 `FixupQueryCurrentDllConfig` 쉽게 해당 데이터를 구문 분석 합니다. 이 예제에서는 해당 구성 파일에서 문자열 및 부울 값을 구문 분석합니다.

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

Visual Studio에는 가장 간단한 개발 및 디버깅 환경을 제공, 하는 동안에 몇 가지 제한이 있습니다.

먼저는.msix에서 설치 하는 대신 패키지 레이아웃 폴더 경로에서 느슨한 파일을 배포 하 여 응용 프로그램을 실행 F5 디버깅 /.appx 패키지 있습니다.  레이아웃 폴더 일반적으로 없는 동일한 보안 제한으로 설치 된 패키지 폴더를 합니다. 결과적으로, 런타임 수정을 적용 하기 전에 패키지 경로 액세스 거부 오류를 재현 하는 불가능할 수도 있습니다 것입니다.

이 문제를 해결 하기 위해 사용할.msix F5 대신.appx 패키지 배포 느슨한 파일 배포 /입니다.  .msix 만들려면 /.appx 패키지 파일을 사용 하 여는 [MakeAppx](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) 위에서 설명한 것 처럼 Windows SDK에서 유틸리티입니다. 또는에서 Visual Studio 내에서 응용 프로그램 프로젝트 노드를 마우스 오른쪽 단추로 클릭 하 고 선택 **스토어**->**앱 패키지 만들기**합니다.

Visual Studio를 사용 하 여 다른 문제는 디버거에서 시작 된 모든 자식 프로세스에 연결 하는 것에 대 한 기본 제공 지원 없기는입니다.   이렇게 하면 시작 후 Visual Studio에서 수동으로 연결할 수 있어야 하는 대상 응용 프로그램의 시작 경로에 논리를 디버깅 하기가 어렵습니다.

이 문제를 해결 하기 위해 자식 프로세스를 지 원하는 디버거를 사용 하 여 연결 합니다.  참고는 일반적으로 수 없는 대상 응용 프로그램 (JIT)-just-in-time 디버거를 연결할 수 있습니다.  이것이 대부분의 JIT 기술 ImageFileExecutionOptions 레지스트리 키를 통해 대상 앱을 대신 디버거 시작을 포함 하기 때문입니다.  이 무산 detouring 메커니즘 PSFLauncher.exe FixupRuntime.dll 대상 앱에 주입 하는 데 사용 합니다.  에 포함 된 WinDbg를 [도구에 대 한 Windows 디버깅](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)에서 가져오는 [Windows SDK](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk), 지원 자식 프로세스가 연결 합니다.  또한 이제 지원 직접 [시작 및 UWP 앱을 디버깅](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app)합니다.

대상 응용 프로그램 시작을 자식 프로세스를 디버깅 하려면 시작 ``WinDbg``합니다.

```ps
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

에 ``WinDbg`` 프롬프트 자식 디버깅을 사용 하도록 설정 하 고 적절 한 중단점을 설정 합니다.

```ps
.childdbg 1
g
```

(대상 응용 프로그램이 시작 되 고 디버거를 중단 될 때까지 실행)

```ps
sxe ld fixup.dll
g
```

(픽스업 DLL이 로드 될 때까지 실행)

```ps
bp ...
```

>[!NOTE]
> [PLMDebug](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/plmdebug) 시작 시 앱에 디버거를 연결할도 사용할 수 있으며에 포함 되어 합니다 [도구에 대 한 Windows 디버깅](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)합니다.  그러나 이제 WinDbg에서 제공 되는 직접 지원 보다 더 복잡 한 것입니다.

## <a name="support-and-feedback"></a>지원 및 피드백

**질문에 답변**

질문이 있으세요? Stack Overflow에서 질문해 주세요. 저희 팀은 이러한 [태그](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다. [여기](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)에서 Microsoft에 문의할 수도 있습니다.
