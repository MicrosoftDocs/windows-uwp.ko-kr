---
title: UWP 앱에 대한 자동화된 빌드 설정
description: 자동화된 빌드를 구성하여 패키지를 테스트용으로 로드하거나 저장하는 방법입니다.
ms.date: 07/17/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: 32054a30e56102b9c0642392d78ac75b78fb99e9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158227"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>UWP 앱에 대한 자동화된 빌드 설정

Azure Pipelines를 사용하여 UWP 프로젝트에 사용할 자동화된 빌드를 만들 수 있습니다. 이 문서에서는 이 작업을 수행하는 여러 가지 방법을 살펴보겠습니다. 또한 다른 빌드 시스템과 통합할 수 있도록 명령줄을 사용하여 이러한 작업을 수행하는 방법도 보여 줍니다.

## <a name="create-a-new-azure-pipeline"></a>새 Azure Pipelines 만들기

아직 [Azure Pipelines에 가입](/azure/devops/pipelines/get-started/pipelines-sign-up)하지 않은 경우 먼저 이 작업을 시작합니다.

다음으로 소스 코드를 빌드하는 데 사용할 수 있는 파이프라인을 만듭니다. GitHub 리포지토리를 빌드하기 위한 파이프라인을 빌드하는 방법에 대한 자습서는 [첫 번째 파이프라인 만들기](/azure/devops/pipelines/get-started-yaml)를 참조하세요. Azure Pipelines는 [이 문서](/azure/devops/pipelines/repos)에 나열된 리포지토리 유형을 지원합니다.

## <a name="set-up-an-automated-build"></a>자동화된 빌드 설정

Azure Dev Ops에서 사용할 수 있는 기본 UWP 빌드 정의부터 살펴본 다음, 파이프라인을 구성하는 방법을 보여드리겠습니다.

빌드 정의 템플릿 목록에서 **유니버설 Windows 플랫폼** 템플릿을 선택합니다.

![UWP 템플릿 선택](images/select-yaml-template.png)

이 템플릿에는 UWP 프로젝트를 빌드하기 위한다음과 같은 기본 구성이 포함되어 있습니다.

```yml
trigger:
- master

pool:
  vmImage: 'windows-latest'

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

기본 템플릿은 .csproj 파일에 지정된 인증서를 사용하여 패키지에 서명하려고 시도합니다. 빌드 중에 패키지에 서명하려면 프라이빗 키에 대한 액세스 권한이 있어야 합니다. 서명하지 않으려면 YAML 파일의 `/p:AppxPackageSigningEnabled=false` 섹션에 `msbuildArgs` 매개 변수를 추가하여 서명을 사용하지 않도록 설정할 수 있습니다.

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>보안 파일 라이브러리에 프로젝트 인증서 추가

가능한 경우 인증서를 리포지토리에 제출하지 않도록 해야 하며, Git에서는 기본적으로 이를 무시합니다. 인증서와 같은 중요한 파일을 안전하게 처리하도록 관리하기 위해 Azure DevOps는 [보안 파일](/azure/devops/pipelines/library/secure-files?view=azure-devops) 기능을 지원합니다.

자동화된 빌드에 대한 인증서를 업로드하려면 다음을 수행합니다.

1. Azure Pipelines의 탐색 창에서 **파이프라인**을 펼치고 **라이브러리**를 클릭합니다.
2. **보안 파일** 탭을 클릭한 다음, **+ 보안 파일**을 클릭합니다.

    ![보안 파일을 업로드하는 방법](images/secure-file1.png)

3. 인증서 파일을 찾아서 **확인**을 클릭합니다.
4. 인증서가 업로드되면 이를 선택하여 해당 속성을 확인합니다. **파이프라인 권한** 아래에서 **모든 파이프라인에서 사용하도록 권한 부여** 설정/해제를 사용하도록 설정합니다.

    ![보안 파일을 업로드하는 방법](images/secure-file2.png)

5. 인증서의 프라이빗 키에 암호가 있는 경우 해당 암호를 [Azure Key Vault](/azure/key-vault/about-keys-secrets-and-certificates)에 저장한 다음, [변수 그룹](/azure/devops/pipelines/library/variable-groups)에 연결하는 것이 좋습니다. 그러면 이 변수를 사용하여 파이프라인에서 암호에 액세스할 수 있습니다. 암호는 프라이빗 키에 대해서만 지원되므로 자체적으로 암호로 보호된 인증서 파일을 사용하는 것은 현재 지원되지 않습니다.

> [!NOTE]
> Visual Studio 2019부터 임시 인증서는 더 이상 UWP 프로젝트에서 생성되지 않습니다. 인증서를 만들거나 내보내려면 [이 문서](/windows/msix/package/create-certificate-package-signing)에 설명된 PowerShell cmdlet을 사용하세요.

## <a name="configure-the-build-solution-build-task"></a>솔루션 빌드에 대한 빌드 작업 구성

이 작업은 작업 폴더에 있는 솔루션을 이진 파일로 컴파일하여 출력 앱 패키지 파일을 생성합니다. 이 작업에는 MSBuild 인수가 사용됩니다. 이러한 인수 값을 지정해야 합니다. 다음 표를 가이드로 따르세요.

|**MSBuild 인수**|**값**|**설명**|
|--------------------|---------|---------------|
| AppxPackageDir | $ (Build.ArtifactStagingDirectory)\AppxPackages | 생성된 아티팩트를 저장할 폴더를 정의합니다. |
| AppxBundlePlatforms | $(Build.BuildPlatform) | 번들에 포함할 플랫폼을 정의할 수 있습니다. |
| AppxBundle | 항상 | 지정된 플랫폼용 .msix/.appx 파일을 사용하여 .msixbundle/.appxbundle을 만듭니다. |
| UapAppxPackageBuildMode | StoreUpload | .msixupload/.appxupload 파일 및 사이드로드용 **_Test** 폴더를 생성합니다. |
| UapAppxPackageBuildMode | CI | .msixupload/.appxupload 파일만 생성합니다. |
| UapAppxPackageBuildMode | SideloadOnly | 사이드로드 전용 **_Test** 폴더를 생성합니다. |
| AppxPackageSigningEnabled | true | 패키지 서명을 사용하도록 설정합니다. |
| PackageCertificateThumbprint | 인증서 지문 | 이 값은 **서명 인증서의 지문과 일치하거나 빈 문자열이어야** 합니다. |
| PackageCertificateKeyFile | 경로 | 사용할 인증서의 경로입니다. 보안 파일 메타데이터에서 검색됩니다. |
| PackageCertificatePassword | 암호 | 인증서의 프라이빗 키용 암호입니다. 암호를 [Azure Key Vault](/azure/key-vault/about-keys-secrets-and-certificates)에 저장하고 이 암호를 [변수 그룹](/azure/devops/pipelines/library/variable-groups)에 연결하는 것이 좋습니다. 변수를 이 인수에 전달할 수 있습니다. |

### <a name="configure-the-build"></a>빌드 구성

명령줄을 사용하거나 다른 빌드 시스템을 사용하여 솔루션을 빌드하려는 경우 이 인수를 사용하여 MSBuild를 실행합니다.

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

### <a name="configure-package-signing"></a>패키지 서명 구성

MSIX(또는 .appx) 패키지에 서명하려면 파이프라인에서 서명 인증서를 검색해야 합니다. 이렇게 하려면 VSBuild 작업 전에 DownloadSecureFile 작업을 추가합니다.
그러면 ```signingCert```를 통해 서명 인증서에 액세스할 수 있습니다.

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

다음으로, 서명 인증서를 참조하도록 VSBuild 작업을 업데이트합니다.

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
> PackageCertificateThumbprint 인수는 예방 조치를 위해 의도적으로 빈 문자열로 설정됩니다. 지문이 프로젝트에 설정되었지만 서명 인증서와 일치하지 않으면 `Certificate does not match supplied signing thumbprint` 오류로 인해 빌드가 실패합니다.

### <a name="review-parameters"></a>매개 변수 검토

`$()` 구문으로 정의된 매개 변수는 빌드 정의에 정의된 변수이며, 다른 빌드 시스템에서 변경됩니다.

![기본 변수](images/building-screen5.png)

미리 정의된 변수를 모두 보려면 [미리 정의된 빌드 변수](/azure/devops/pipelines/build/variables)를 참조하세요.

## <a name="configure-the-publish-build-artifacts-task"></a>빌드 아티팩트 게시 작업 구성

기본 UWP 파이프라인은 생성된 아티팩트를 저장하지 않습니다. 게시 기능을 YAML 정의에 추가하려면 다음 작업을 추가합니다.

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

생성된 아티팩트는 빌드 결과 페이지의 **아티팩트** 옵션에서 확인할 수 있습니다.

![아티팩트](images/building-screen6.png)

`UapAppxPackageBuildMode` 인수를 `StoreUpload`로 설정했기 때문에, 아티팩트 폴더에는 Microsoft Store 제출용 패키지가 포함됩니다. 또한 Microsoft Store에 일반 앱 패키지(.msix/.appx)나 앱 번들(.msixbundle/.appxbundle/)도 제출할 수 있습니다. 이 문서의 목적을 위해 .appxupload 파일을 사용합니다.

## <a name="address-bundle-errors"></a>주소 번들 오류

솔루션에 UWP 프로젝트를 두 개 이상 추가하고 번들을 만들려는 경우 다음과 같은 오류가 나타날 수 있습니다.

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

이 오류는 솔루션 수준에서 앱이 번들로 나타나도 되는지 분명하지 않기 때문에 표시됩니다. 이 문제를 해결하려면 각 프로젝트 파일을 열고 첫 번째 `<PropertyGroup>` 요소 끝에 다음 속성을 추가합니다.

|**프로젝트**|**속성**|
|-------|----------|
|앱|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

그런 다음, 빌드 단계에서 `AppxBundle` MSBuild 인수를 제거합니다.

## <a name="related-topics"></a>관련 항목

- [Windows용 .NET 앱 빌드](/vsts/build-release/get-started/dot-net)
- [UWP 앱 패키징](/windows/msix/package/packaging-uwp-apps)
- [Windows 10에서 LOB 앱을 테스트용으로 로드 ](/windows/deploy/sideload-apps-in-windows-10)
- [패키지 서명용 인증서 만들기](/windows/msix/package/create-certificate-package-signing)