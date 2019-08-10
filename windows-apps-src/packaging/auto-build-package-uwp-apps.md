---
title: UWP 앱에 대한 자동화된 빌드 설정
description: 자동화된 빌드를 구성하여 사이드로드/Microsoft Store 패키지를 생성하는 방법
ms.date: 07/17/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: 08ad21d3ddc73499bb2b97b300e635fe0a6c148d
ms.sourcegitcommit: 698a86640b365dc1ca772fb6f53ca556dc284ed6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68935773"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>UWP 앱에 대한 자동화된 빌드 설정

Azure Pipelines를 사용 하 여 UWP 프로젝트에 대해 자동화 된 빌드를 만들 수 있습니다. 이 문서에서는이 작업을 수행 하는 다양 한 방법을 살펴보겠습니다. 또한 다른 빌드 시스템과 통합할 수 있도록 명령줄을 사용 하 여 이러한 작업을 수행 하는 방법을 보여 줍니다.

## <a name="create-a-new-azure-pipeline"></a>새 Azure 파이프라인 만들기

아직 수행 하지 않은 경우 [Azure Pipelines에 등록](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up) 하 여 시작 합니다.

다음으로 소스 코드를 빌드하는 데 사용할 수 있는 파이프라인을 만듭니다. GitHub 리포지토리를 빌드하기 위한 파이프라인 빌드에 대 한 자습서는 [첫 번째 파이프라인 만들기](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml)를 참조 하세요. Azure Pipelines은 [이 문서에](https://docs.microsoft.com/azure/devops/pipelines/repos)나열 된 리포지토리 유형을 지원 합니다.

## <a name="set-up-an-automated-build"></a>자동화된 빌드 설정

Azure Dev Ops에서 사용할 수 있는 기본 UWP 빌드 정의로 시작 하 고 파이프라인을 구성 하는 방법을 보여 줍니다.

빌드 정의 템플릿 목록에서 **유니버설 Windows 플랫폼** 템플릿을 선택합니다.

![UWP 템플릿 선택](images/select-yaml-template.png)

이 템플릿에는 UWP 프로젝트를 빌드하기 위한 기본 구성이 포함 되어 있습니다.

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

기본 템플릿은 .csproj 파일에 지정 된 인증서를 사용 하 여 패키지에 서명 하려고 합니다. 빌드 중에 패키지에 서명 하려면 개인 키에 대 한 액세스 권한이 있어야 합니다. 그렇지 않은 경우에는 yaml 파일의 `/p:AppxPackageSigningEnabled=false` `msbuildArgs` 섹션에 매개 변수를 추가 하 여 서명을 사용 하지 않도록 설정할 수 있습니다.

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>보안 파일 라이브러리에 프로젝트 인증서 추가

가능 하면 리포지토리에 인증서를 제출 하지 말고, git은 기본적으로이를 무시 해야 합니다. Azure DevOps는 인증서와 같은 중요 한 파일에 대 한 안전한 처리를 관리 하기 위해 [보안 파일](https://docs.microsoft.com/azure/devops/pipelines/library/secure-files?view=azure-devops) 기능을 지원 합니다.

자동화 된 빌드에 대 한 인증서를 업로드 하려면:

1. Azure Pipelines의 탐색 창에서 **파이프라인** 을 확장 하 고 **라이브러리**를 클릭 합니다.
2. **보안 파일** 탭을 클릭 한 다음 **+ 보안 파일**을 클릭 합니다.

    ![보안 파일을 업로드 하는 방법](images/secure-file1.png)

3. 인증서 파일을 찾은 다음 **확인**을 클릭 합니다.
4. 인증서를 업로드 한 후에는 인증서를 선택 하 여 해당 속성을 확인 합니다. **파이프라인 권한**에서 **모든 파이프라인에서 사용할** 수 있는 권한 부여를 설정 합니다.

    ![보안 파일을 업로드 하는 방법](images/secure-file2.png)

5. 인증서의 개인 키에 암호가 있는 경우 [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) 에 암호를 저장 하 고 [변수 그룹](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups)에 암호를 연결 하는 것이 좋습니다. 변수를 사용 하 여 파이프라인에서 암호에 액세스할 수 있습니다. 암호는 개인 키에 대해서만 지원 됩니다. 암호로 보호 되는 인증서 파일을 사용 하는 것은 현재 지원 되지 않습니다.

> [!NOTE]
> Visual Studio 2019 부터는 UWP 프로젝트에서 임시 인증서가 더 이상 생성 되지 않습니다. 인증서를 만들거나 내보내려면 [이 문서](/windows/msix/package/create-certificate-package-signing)에 설명 된 PowerShell cmdlet을 사용 합니다.

## <a name="configure-the-build-solution-build-task"></a>솔루션 빌드에 대한 빌드 작업 구성

이 작업은 작업 폴더에 있는 모든 솔루션을 이진 파일로 컴파일하고 출력 앱 패키지 파일을 생성 합니다. 이 태스크에서는 MSBuild 인수를 사용 합니다. 이러한 인수 값을 지정해야 합니다. 다음 표를 가이드로 따르세요.

|**MSBuild 인수**|**Value**|**설명**|
|--------------------|---------|---------------|
| AppxPackageDir | $ (Build.ArtifactStagingDirectory)\AppxPackages | 생성된 아티팩트를 저장할 폴더를 정의합니다. |
| AppxBundlePlatforms | $(Build.BuildPlatform) | 번들에 포함할 플랫폼을 정의할 수 있습니다. |
| AppxBundle | Always | 지정 된 플랫폼에 대 한. m 6/.appx 파일을 사용 하 여. a s n x 번들/.appxbundle를 만듭니다. |
| UapAppxPackageBuildMode | StoreUpload | 는 .appxupload 파일 및 테스트용 로드에 대 한 **_Test** 폴더를 생성 합니다. |
| UapAppxPackageBuildMode | CI | 는. msixupload/.appxupload 파일만 생성 합니다. |
| UapAppxPackageBuildMode | 함께 Loadonly | 테스트용 로드만 **_Test** 폴더를 생성 합니다. |
| AppxPackageSigningEnabled | true | 패키지 서명을 사용 하도록 설정 합니다. |
| PackageCertificateThumbprint | 인증서 지문 | 이 값은 서명 인증서의 지문과 일치 **해야** 합니다. 그렇지 않으면 빈 문자열 이어야 합니다. |
| PackageCertificateKeyFile | Path | 사용할 인증서의 경로입니다. 이는 보안 파일 메타 데이터에서 검색 됩니다. |
| PackageCertificatePassword | 암호 | 인증서의 개인 키에 대 한 암호입니다. [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) 에 암호를 저장 하 고 [변수 그룹](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups)에 암호를 연결 하는 것이 좋습니다. 이 인수에 변수를 전달할 수 있습니다. |

### <a name="configure-the-build"></a>빌드 구성

명령줄을 사용 하거나 다른 빌드 시스템을 사용 하 여 솔루션을 빌드 하려는 경우 이러한 인수로 MSBuild를 실행 합니다.

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

### <a name="configure-package-signing"></a>패키지 서명 구성

MSIX (또는 APPX) 패키지에 서명 하려면 파이프라인에서 서명 인증서를 검색 해야 합니다. 이렇게 하려면 VSBuild 작업 전에 DownloadSecureFile 작업을 추가 합니다.
그러면를 통해 ```signingCert```서명 인증서에 액세스할 수 있습니다.

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

다음으로, 서명 인증서를 참조 하도록 VSBuild 작업을 업데이트 합니다.

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
> PackageCertificateThumbprint 인수는 의도적으로 빈 문자열로 설정 됩니다. 지문이 프로젝트에 설정 되어 있지만 서명 인증서와 일치 하지 않으면 빌드에 실패 하 고 오류가 발생 `Certificate does not match supplied signing thumbprint`합니다.

### <a name="review-parameters"></a>매개 변수 검토

`$()` 구문을 사용 하 여 정의 된 매개 변수는 빌드 정의에 정의 된 변수 이며 다른 빌드 시스템에서 변경 됩니다.

![기본 변수](images/building-screen5.png)

미리 정의 된 변수를 모두 보려면 [미리 정의 된 빌드 변수](https://docs.microsoft.com/azure/devops/pipelines/build/variables)를 참조 하세요.

## <a name="configure-the-publish-build-artifacts-task"></a>빌드 아티팩트 게시 작업 구성

기본 UWP 파이프라인은 생성 된 아티팩트를 저장 하지 않습니다. YAML 정의에 게시 기능을 추가 하려면 다음 작업을 추가 합니다.

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

빌드 결과 페이지의 **아티팩트** 옵션에서 생성 된 아티팩트를 볼 수 있습니다.

![artifacts](images/building-screen6.png)

`UapAppxPackageBuildMode` 인수를로 `StoreUpload`설정 했으므로 아티팩트 폴더에는 저장소 (. msixupload/. .appxupload)에 제출할 패키지를 포함 합니다. 또한 일반 앱 패키지 (. msix/.appx) 또는 앱 번들 (. msixbundle/.appxbundle/)을 스토어에 제출할 수 있습니다. 이 문서의 목적을 위해 .appxupload 파일을 사용합니다.

## <a name="address-bundle-errors"></a>주소 번들 오류

솔루션에 둘 이상의 UWP 프로젝트를 추가한 다음 번들을 만들려고 하면 다음과 같은 오류가 표시 될 수 있습니다.

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

이 오류는 솔루션 수준에서 앱이 번들로 나타나도 되는지 분명하지 않기 때문에 표시됩니다. 이 문제를 해결 하려면 각 프로젝트 파일을 열고 첫 번째 `<PropertyGroup>` 요소의 끝에 다음 속성을 추가 합니다.

|**프로젝트**|**Properties**|
|-------|----------|
|앱|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

그런 다음 빌드 단계 `AppxBundle` 에서 MSBuild 인수를 제거 합니다.

## <a name="related-topics"></a>관련 항목

- [Windows 용 .NET 앱 빌드](https://docs.microsoft.com/vsts/build-release/get-started/dot-net)
- [UWP 앱 패키징](/windows/msix/package/packaging-uwp-apps)
- [Windows 10에서 LOB 앱 테스트용으로 로드](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10)
- [패키지 서명에 대 한 인증서 만들기](/windows/msix/package/create-certificate-package-signing)
