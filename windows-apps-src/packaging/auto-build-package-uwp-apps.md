---
title: UWP 앱에 대한 자동화된 빌드 설정
description: 자동화된 빌드를 구성하여 사이드로드/Microsoft Store 패키지를 생성하는 방법
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: 5837674f2cb20710a59eeac0af59498bf28b197e
ms.sourcegitcommit: a86d0bd1c2f67e5986cac88a98ad4f9e667cfec5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68229392"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>UWP 앱에 대한 자동화된 빌드 설정

UWP 프로젝트에 대 한 자동화 된 빌드를 만들려면 Azure 파이프라인을 사용할 수 있습니다. 이 문서에서는이 작업을 수행 하는 다양 한 방법에 살펴보겠습니다. 또한 보여드리겠습니다 다른 빌드 시스템을 통합할 수 있도록 명령줄을 사용 하 여 이러한 작업을 수행 하는 방법입니다.

## <a name="create-a-new-azure-pipeline"></a>새 Azure 파이프라인 만들기

먼저 [Azure 파이프라인 등록](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up) 이미 수행 하지 않은 경우.

다음으로 소스 코드를 작성 하는 데 사용할 수 있는 파이프라인을 만듭니다. GitHub 리포지토리에 빌드 파이프라인을 구축 하는 방법에 대 한 자습서를 참조 하세요 [첫 번째 파이프라인 만들기](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml)합니다. Azure 파이프라인 지원 저장소 유형 나열 [이 문서의](https://docs.microsoft.com/azure/devops/pipelines/repos)합니다.

## <a name="set-up-an-automated-build"></a>자동화된 빌드 설정

UWP Azure Devops에서 사용할 수 있는 작성 하 고 다음 파이프라인을 구성 하는 방법을 보여 기본값을 사용 하 여 시작 하겠습니다.

빌드 정의 템플릿 목록에서 **유니버설 Windows 플랫폼** 템플릿을 선택합니다.

![UWP 템플릿을 선택 합니다.](images/select-yaml-template.png)

이 템플릿에 UWP 프로젝트를 빌드할 수 있도록 기본 구성:

```yml
trigger:
- master

pool:
  vmImage: 'VS2017-Win2016'

variables:
  solution: '**/*.sln'
  buildPlatform: 'x86|x64|ARM'
  buildConfiguration: 'Release'
  appxPackageDir: '$(build.artifactStagingDirectory)\AppxPackages\\'

steps:
- task: NuGetToolInstaller@0

- task: NuGetCommand@2
  inputs:
    restoreSolution: '$(solution)'

- task: VSBuild@1
  inputs:
    platform: 'x86'
    solution: '$(solution)'
    configuration: '$(buildConfiguration)'
    msbuildArgs: '/p:AppxBundlePlatforms="$(buildPlatform)" /p:AppxPackageDir="$(appxPackageDir)" /p:AppxBundle=Always /p:UapAppxPackageBuildMode=StoreUpload'

```

기본 템플릿을.csproj 파일에 지정 된 인증서를 사용 하 여 패키지에 서명 하려고 시도 합니다. 빌드 중에 패키지를 서명 하려면 개인 키에 액세스할 수 있어야 합니다. 매개 변수를 추가 하 여 서명을 해제할 수이 고, 그렇지 `/p:AppxPackageSigningEnabled=false` 에 `msbuildArgs` YAML 파일의 섹션입니다.

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>보안 파일 라이브러리를 프로젝트 인증서 추가

리포지토리에 가능한, 인증서를 제출 하지 않아야 하 고 git은 기본적으로 무시 합니다. Azure DevOps는 인증서와 같은 중요 한 파일의 안전한 처리를 관리 하려면 다음을 지원 합니다. [파일을 보호](https://docs.microsoft.com/azure/devops/pipelines/library/secure-files?view=azure-devops)합니다.

자동화 된 빌드에 대 한 인증서를 업로드 합니다.

1. Azure 파이프라인 확장 **파이프라인** 탐색 창에서 클릭 **라이브러리**합니다.
2. 클릭 합니다 **파일을 보호** 탭을 클릭 한 다음 **+ 보안 파일**합니다.

    ![보안 파일을 업로드 하는 방법](images/secure-file1.png)

3. 인증서 파일을 찾아 클릭 **확인**합니다.
4. 인증서를 업로드 한 후에 해당 속성을 보려면 선택 합니다. 아래 **권한을 파이프라인**, 사용 하도록 설정 합니다 **모든 파이프라인에서 사용 하기 위해 권한 부여** 설정/해제 합니다.

    ![보안 파일을 업로드 하는 방법](images/secure-file2.png)

## <a name="configure-the-build-solution-build-task"></a>솔루션 빌드에 대한 빌드 작업 구성

이 작업은 작업 폴더에 이진 파일은 출력 앱 패키지 파일을 생성 하는 모든 솔루션을 컴파일합니다.
이 태스크는 MSBuild 인수를 사용합니다. 이러한 인수 값을 지정해야 합니다. 다음 표를 가이드로 따르세요.

|**MSBuild 인수**|**Value**|**설명**|
|--------------------|---------|---------------|
| AppxPackageDir | $ (Build.ArtifactStagingDirectory)\AppxPackages | 생성된 아티팩트를 저장할 폴더를 정의합니다. |
| AppxBundlePlatforms | $(Build.BuildPlatform) | 번들에 포함할 플랫폼을 정의할 수 있습니다. |
| AppxBundle | Always | 지정 된 플랫폼에 대 한.msix/.appx 파일을 사용 하 여는.msixbundle/.appxbundle를 만듭니다. |
| UapAppxPackageBuildMode | StoreUpload | .Msixupload/.appxupload 파일을 생성 하며 **테스트 (_t)** 테스트용 로드에 대 한 폴더입니다. |
| UapAppxPackageBuildMode | CI | .Msixupload/.appxupload 파일만을 생성합니다. |
| UapAppxPackageBuildMode | SideloadOnly | 생성 된 **테스트 (_t)** 만 테스트용 로드에 대 한 폴더입니다. |
| AppxPackageSigningEnabled | true | 패키지 서명이 사용 됩니다. |
| PackageCertificateThumbprint | 인증서 지문 | 이 값 **해야** 서명 인증서의 지문과 일치 또는 빈 문자열일 수 있습니다. |
| PackageCertificateKeyFile | Path | 인증서를 사용 하 여 경로입니다. 이 보안 파일 메타 데이터에서 검색 됩니다. |

### <a name="configure-the-build"></a>빌드 구성

명령줄을 사용 하 여 또는 다른 빌드 시스템을 사용 하 여 솔루션을 빌드 하려는 경우 이러한 인수를 사용 하 여 MSBuild를 실행 합니다.

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

### <a name="configure-package-signing"></a>패키지를 서명 구성

MSIX (또는 APPX) 패키지에 서명 하려면 파이프라인 서명 인증서를 검색 해야 합니다. 이 작업을 수행 하려면 VSBuild 작업 전에 DownloadSecureFile 작업을 추가 합니다.
통해 서명 인증서에 액세스할 수 그러면 ```signingCert```합니다.

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

다음으로 서명 인증서를 참조할 VSBuild 작업 업데이트:

```yml
- task: VSBuild@1
  inputs:
    platform: 'x86'
    solution: '$(solution)'
    configuration: '$(buildConfiguration)'
    msbuildArgs: '/p:AppxBundlePlatforms="$(buildPlatform)" 
                  /p:AppxPackageDir="$(appxPackageDir)" 
                  /p:AppxBundle=Always 
                  /p:UapAppxPackageBuildMode=StoreUpload 
                  /p:AppxPackageSigningEnabled=true
                  /p:PackageCertificateThumbprint="" 
                  /p:PackageCertificateKeyFile="$(signingCert.secureFilePath)"'
```

> [!NOTE]
> PackageCertificateThumbprint 인수 의도적으로 예방 조치로 서 빈 문자열로 설정 됩니다. 오류가 발생 하 여 빌드가 실패 지문을 프로젝트에 설정 되어 있지만 서명 인증서가 일치 하지 않습니다, 경우: `Certificate does not match supplied signing thumbprint`합니다.

### <a name="review-parameters"></a>매개 변수를 검토

정의 된 매개 변수는 `$()` 구문은 빌드 정의에 정의 된 변수 및 다른 변경 시스템 빌드됩니다.

![기본 변수](images/building-screen5.png)

모든 미리 정의 된 변수를 보려면 [미리 정의 된 빌드 변수](https://docs.microsoft.com/azure/devops/pipelines/build/variables)합니다.

## <a name="configure-the-publish-build-artifacts-task"></a>빌드 아티팩트 게시 작업 구성

기본 UWP 파이프라인에서 생성 된 아티팩트를 저장 하지 않습니다. YAML 정의에 게시 기능을 추가 하려면 다음 태스크를 추가 합니다.

```yml
- task: CopyFiles@2
  displayName: 'Copy Files to: $(build.artifactstagingdirectory)'
  inputs:
    SourceFolder: '$(system.defaultworkingdirectory)'
    Contents: '**\bin\$(BuildConfiguration)\**'
    TargetFolder: '$(build.artifactstagingdirectory)'

- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: drop'
  inputs:
    PathtoPublish: '$(build.artifactstagingdirectory)'
```

생성 된 아티팩트를 볼 수 있습니다 합니다 **아티팩트** 옵션 빌드 결과 페이지입니다.

![artifacts](images/building-screen6.png)

설정 했습니다 때문에 합니다 `UapAppxPackageBuildMode` 인수를 `StoreUpload`, 아티팩트 폴더 (.msixupload/.appxupload) 스토어에 제출할 패키지를 포함 합니다. 제출할 수도 있습니다는 일반 앱 패키지 (.msix/.appx) 또는 앱 번들 (.msixbundle/.appxbundle/) 저장소에 note 합니다. 이 문서의 목적을 위해 .appxupload 파일을 사용합니다.

## <a name="address-bundle-errors"></a>번들 오류를 해결

솔루션에 둘 이상의 UWP 프로젝트를 추가 하 고 번들 만들기를 시도 하는 경우 다음과 같은 오류가 발생할 수 있습니다.

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

이 오류는 솔루션 수준에서 앱이 번들로 나타나도 되는지 분명하지 않기 때문에 표시됩니다. 이 문제를 해결 하려면 각 프로젝트 파일을 열고 다음 속성을 첫 번째 끝에 추가 `<PropertyGroup>` 요소입니다.

|**프로젝트**|**Properties**|
|-------|----------|
|앱|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

그런 다음 제거를 `AppxBundle` 빌드 단계에서 MSBuild 인수입니다.

## <a name="related-topics"></a>관련 항목

- [Windows 용.NET 앱 빌드](https://docs.microsoft.com/vsts/build-release/get-started/dot-net)
- [UWP 앱 패키징](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)
- [Windows 10에서에서 테스트용으로 로드 LOB 앱](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10)
- [패키지 서명 인증서 만들기](https://docs.microsoft.com/windows/uwp/packaging/create-certificate-package-signing)
