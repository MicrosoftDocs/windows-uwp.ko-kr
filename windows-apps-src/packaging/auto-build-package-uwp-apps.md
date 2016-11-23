---
author: rmpablos
title: "UWP 앱에 대한 자동화된 빌드 설정"
description: "자동화된 빌드를 구성하여 패키지를 테스트용으로 로드하거나 저장하는 방법입니다."
translationtype: Human Translation
ms.sourcegitcommit: 1e30c20b38b9e6b00cd4f56bf9ffb3752ceed6a9
ms.openlocfilehash: 681019d708b859d117992cde6d2ad3d37fa4833a

---
# UWP 앱에 대한 자동화된 빌드 설정

VSTS(Visual Studio Team Services)를 사용하여 UWP 프로젝트에 대해 자동화된 빌드를 만들 수 있습니다. 이 문서에서는 이 작업을 수행하는 여러 방법에 대해 살펴보겠습니다.  또한 AppVeyor 같은 다른 빌드 시스템과 통합할 수 있도록 이러한 작업을 명령줄을 사용하여 수행하는 방법도 알아보겠습니다. 

## 올바른 빌드 에이전트 유형 선택

빌드 프로세스를 실행할 때 VSTS에서 사용할 빌드 에이전트 유형을 선택합니다. 호스트된 빌드 에이전트는 가장 일반적인 도구 및 sdk를 사용하여 배포되며, 대부분의 시나리오에 적용됩니다. 자세한 내용은 [Software on the hosted build server](https://www.visualstudio.com/en-us/docs/build/admin/agents/hosted-pool#software)(호스트된 빌드 서버의 소프트웨어) 문서를 참조하세요. 그러나 빌드 단계를 더 제어 해야 할 경우에는 사용자 지정 빌드 에이전트를 만들 수 있습니다. 다음 테이블을 사용하면 이러한 결정을 내리는 데 도움이 됩니다.

|**시나리오**|**사용자 지정 에이전트**|**호스트된 빌드 에이전트**|
-------------|----------------|----------------------|
|기본 UWP 빌드(.NET 네이티브 포함)|: white_check_mark:|: white_check_mark:|
|테스트용 로드를 위한 패키지 생성|: white_check_mark:|: white_check_mark:|
|저장소 제출을 위한 패키지 생성|: white_check_mark:|: white_check_mark:|
|사용자 지정 인증서 사용|: white_check_mark:||
|사용자 지정 Windows SDK를 대상으로 하는 빌드|: white_check_mark:||
|단위 테스트 실행|: white_check_mark:||
|증분 빌드 사용|: white_check_mark:||

>참고: Windows 1주년 업데이트 SDK(빌드 14393)를 대상으로 지정하려는 경우에는 호스트 빌드 풀에서 SDK 10586 및 10240만 지원하므로 사용자 지정 빌드 에이전트를 설정해야 합니다. [UWP 버전 선택](https://msdn.microsoft.com/en-us/windows/uwp/updates-and-versions/choose-a-uwp-version)에 대한 자세한 정보

#### 사용자 지정 빌드 에이전트 만들기(옵션)

사용자 지정 빌드 에이전트를 만들려는 경우 유니버설 Windows 플랫폼 도구가 필요합니다. 이러한 도구는 Visual Studio에 포함되어 제공됩니다. Visual Studio의 커뮤니티 버전을 사용할 수 있습니다.

자세한 내용은 [Deploy an agent on Windows](https://www.visualstudio.com/en-us/docs/build/admin/agents/v2-windows)(Windows에 에이전트 배포)를 참조하세요. 

UWP 단위 테스트를 실행하려면 다음을 수행해야 합니다. • 앱을 배포하고 시작합니다. • VSTS 에이전트를 대화형 모드로 실행합니다. • 에이전트를 다시 부팅 한 후 자동 로그온되도록 구성합니다.

이제 자동화된 빌드를 설정하는 방법에 대해 알아보겠습니다.

## 자동화된 빌드 설정
먼저 VSTS에서 사용할 수 있는 기본 UWP 빌드 정의부터 살펴본 다음 해당 정의를 구성하는 방법을 알아 보겠습니다. 이렇게 하면 고급 빌드 작업을 더 많이 수행할 수 있습니다.

**소스 코드 리포지토리에 프로젝트 인증서 추가**

VSTS는 TFS 및 GIT 기반 코드 리포지토리 모두와 작동합니다.  
Git 리포지토리를 사용하는 경우 빌드 에이전트에서 appx 패키지에 서명할 수 있도록 프로젝트의 인증서 파일을 리포지토리에 추가합니다. 이렇게 하지 않으면 Git 리포지토리에서는 인증서 파일을 무시합니다. 인증서 파일을 저장소에 추가하려면 솔루션 탐색기에서 인증서 파일을 마우스 오른쪽 단추로 클릭하고 바로 가기 메뉴에서 소스 제어에 무시된 파일 추가 명령을 선택합니다. 

![인증서를 포함하는 방법](images/building-screen1.png)

[고급 인증서 관리](#certificates-best-practices)는 이 가이드의 뒷부분에서 살펴보겠습니다. 

VSTS에서 첫 번째 빌드 정의를 만들려면 빌드 탭으로 이동한 다음 + 단추를 선택합니다.

![빌드 정의 만들기](images/building-screen2.png)

빌드 정의 템플릿 목록에서 *유니버설 Windows 플랫폼* 템플릿을 선택합니다.

![uwp 빌드 선택](images/building-screen3.png)

이 빌드 정의에 포함된 빌드 작업은 다음과 같습니다.

- NuGet 복원 **\*.sln
- 솔루션 빌드 **\*.sln
- 기호 게시
- 아티팩트 게시: 놓기

#### NuGet 복원 빌드 작업 구성

이 작업에서는 프로젝트에 정의된 NuGet 패키지를 복원합니다. 일부 패키지에는 NuGet.exe의 사용자 지정 버전이 필요합니다. 이 버전이 필요한 패키지를 사용하는 경우에는 리포지토리에서 해당 버전의 NuGet.exe를 참조한 다음 *NuGet.exe 경로*의 고급 속성에서 참조하세요.

![기본 빌드 정의](images/building-screen4.png)

#### 솔루션 빌드에 대한 빌드 작업 구성

이 작업은 작업 폴더에 있는 솔루션을 이진 파일로 컴파일하여 출력 AppX 파일을 생성합니다. 이 작업에서는 MSbuild 인수가 사용됩니다.  이러한 인수 값을 지정해야 합니다. 다음 표를 가이드로 따르세요. 

|**MSBuild 인수**|**값**|**설명**|
|--------------------|---------|---------------|
|AppxPackageDir|$ (Build.ArtifactStagingDirectory)\AppxPackages|생성된 아티팩트를 저장할 폴더를 정의합니다.|
|AppxBundlePlatforms|$(Build.BuildPlatform)|번들에 포함할 플랫폼을 정의할 수 있습니다.|
|AppxBundle|항상|지정된 플랫폼용 appx 파일과 함께 appxbundle을 만듭니다.|
|**UapAppxPackageBuildMode**|StoreUpload|생성할 appx 패키지 종류를 정의합니다. (기본적으로 포함 안 됨)|


명령줄을 사용하거나 다른 빌드 시스템을 사용하여 솔루션을 빌드하려는 경우 이 인수를 사용하여 msbuild를 실행합니다.

```
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"  
/p:UapAppxPackageBuildMode=StoreUpload 
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

$() 구문을 사용하여 정의된 매개 변수는 해당 빌드 정의에 정의된 변수이며, 다른 빌드 시스템에서는 변경됩니다.

![기본 변수](images/building-screen5.png)

미리 정의된 변수를 모두 보려면 [빌드 변수 사용](https://www.visualstudio.com/docs/build/define/variables)을 참조하세요.

#### 아티팩트 게시 빌드 작업 구성 
이 작업에서는 생성된 아티팩트를 VSTS에 저장합니다. 저장된 아티팩트는 빌드 결과 페이지의 아티팩트 탭에서 볼 수 있습니다. VSTS에서는 이전에 정의한 `$Build.ArtifactStagingDirectory)\AppxPackages` 폴더를 사용합니다.

![아티팩트](images/building-screen6.png)

`UapAppxPackageBuildMode` 속성을 `StoreUpload`로 설정했으므로 아티팩트 폴더에는 스토어에 업로드한 패키지(appxupload)와 테스트용 로드를 사용하도록 설정한 패키지(appxbundle)가 포함됩니다.


>참고: 기본적으로 VSTS 에이전트는 최신 appx 생성 패키지를 유지 관리합니다. 현재 빌드의 아티팩트만 저장하려면 이진 디렉터리가 정리되도록 빌드를 구성하세요. 이렇게 하려면 이름이 `Build.Clean`인 변수를 추가한 다음 변수 값을 `all`로 설정합니다. 자세한 내용은 [Specify the repository](https://www.visualstudio.com/en-us/docs/build/define/repository#how-can-i-clean-the-repository-in-a-different-way)(리포지토리 지정)를 참조하세요.

#### 자동화된 빌드 형식
다음으로 빌드 정의를 사용하여 자동화된 빌드를 만들어 보겠습니다. 다음 표에서는 만들 수 있는 자동화된 빌드 형식 각각에 대해 설명합니다. 

|**빌드 형식**|**아티팩트**|**권장 빈도**|**설명**|
|-----------------|------------|-------------------------|---------------|
|연속 통합|빌드 로그, 테스트 결과|커밋할 때마다|이 빌드 형식은 빠르며 하루에도 여러 번 실행됩니다.|
|테스트용 로드를 위한 연속 배포 빌드|배포 패키지|매일 |이 빌드 형식에는 단위 테스트가 포함될 수 있지만 약간 더 오래 걸립니다. 수동 테스트가 가능하며, HockeyApp 등 다른 도구와 통합할 수 있습니다.|
|저장소에 패키지를 제출하는 연속 배포 빌드|패키지 게시|주문형|이 빌드 형식은 저장소에 게시할 수 있는 패키지를 만듭니다.|

각각 구성하는 방법에 대해 살펴보겠습니다.


## CI(연속 통합) 빌드 설정 
이 빌드 형식을 사용하면 코드 관련 문제를 신속하게 진단할 수 있습니다. 일반적으로 하나의 플랫폼에 대해서만 실행되며, .NET 네이티브 도구 체인에서 처리할 필요가 없습니다. 또한 CI 빌드를 사용하여 테스트 결과 보고서를 생성하는 단위 테스트를 실행할 수 있습니다.  

UWP 단위 테스트를 CI 빌드의 일부로 실행하려면 호스트된 빌드 에이전트 대신 사용자 지정 빌드 에이전트를 사용해야 합니다.

>참고: 같은 솔루션에서 둘 이상의 앱을 번들로 만들면 오류가 표시될 수도 있습니다. 해당 오류를 해결하려면 [같은 솔루션에서 둘 이상의 앱을 번들로 만들 때 나타나는 오류 해결](#bundle-errors) 항목을 참조하세요. 


### CI 빌드 정의 구성
빌드 정의를 만들려면 기본 UWP 템플릿을 사용합니다. 그런 다음 각 체크 인에서 실행하도록 트리거를 구성합니다.  

![ci 트리거](images/building-screen7.png)

CI 빌드는 사용자에게 배포되지 않으므로 CD 빌드와의 혼동을 피하기 위해 다른 버전 관리 번호를 유지하는 것이 좋습니다. 예를 들면 `$(BuildDefinitionName)_0.0.$(DayOfYear)$(Rev:.r)`와 같습니다.


#### 단위 테스트를 위한 사용자 지정 빌드 에이전트 구성

1. 먼저 PC에서 개발자 모드를 사용하도록 설정합니다. 디바이스를 개발에 사용하도록 설정을 참조하세요. 2. 서비스가 대화형 프로세스로 실행되도록 설정합니다. Deploy an agent on Windows(Windows에 에이전트 배포)를 참조하세요. 3. 에이전트에 서명 인증서를 배포합니다.

이렇게 하려면.cer 파일을 두 번 클릭하고 로컬 컴퓨터, 신뢰할 수 있는 사용자 저장소를 차례로 선택합니다.

<span id="uwp-unit-tests" />
### 빌드 정의에서 UWP 단위 테스트를 실행하도록 구성
단위 테스트를 실행하려면 Visual Studio 테스트 빌드 단계를 사용합니다.


![단위 테스트 추가](images/building-screen8.png)

UWP 단위 테스트는 생성된 번들을 사용할 수 없으므로 지정된 appx 파일 컨텍스트에서 실행됩니다. 또한 구체적인 플랫폼 appx 파일에 대한 경로를 지정해야 합니다. 예를 들면 다음과 같습니다.

```
$(Build.ArtifactStagingDirectory)\AppxPackages\MyUWPApp.UnitTest\x86\MyUWPApp.UnitTest_$(AppxVersion)_x86.appx
```

>참고: 명령줄에서 단위 테스트를 로컬로 실행하려면 다음 명령을 사용하세요.
`"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\CommonExtensions\Microsoft\TestWindow\vstest.console.exe"`

#### 테스트 결과 액세스
VSTS의 빌드 요약 페이지에서는 단위 테스트를 실행하는 각 빌드에 대한 테스트 결과를 보여줍니다.  여기에서 테스트 결과 페이지를 열어 테스트 결과에 대한 자세한 정보를 볼 수 있습니다. 

![테스트 결과](images/building-screen9.png)

#### CI 빌드 속도 향상
CI 빌드 체크 인을 체크 인의 품질을 모니터링하는 데에만 사용하면 빌드 시간을 줄일 수 있습니다.

#### CI 빌드 속도를 개선하려면
1.  플랫폼 하나에 대해서만 빌드합니다.
2.  x86만 사용하여 BuildPlatform 변수를 편집합니다. ![구성 ci](images/building-screen10.png) 
3.  빌드 단계에서 MSBuild 인수 속성에 /p:AppxBundle=Never를 추가한 다음 플랫폼 속성을 설정합니다. ![플랫폼 구성](images/building-screen11.png)
4.  단위 테스트 프로젝트에서.NET 네이티브를 사용하지 않도록 설정합니다. 

이렇게 하려면 프로젝트 파일을 열고 프로젝트 속성에서 `UseDotNetNativeToolchain` 속성을 `false`로 설정합니다.

>참고. .NET 네이티브 도구 체인은 릴리스 빌드를 테스트하는 데 계속 사용해야 하므로 이 도구 체인을 사용하는 것은 워크플로에 중요한 부분입니다. 

<span id="bundle-errors" />
#### 같은 솔루션에서 둘 이상의 앱을 번들로 만들 때 나타나는 오류 해결 
솔루션에 UWP 프로젝트를 두 개 이상 추가하고 번들을 만들려는 경우 다음과 같은 오류가 나타날 수 있습니다. 

```
MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle
```

이 오류는 솔루션 수준에서 앱이 번들로 나타나도 되는지 분명하지 않기 때문에 표시됩니다. 이 문제를 해결하려면 각 프로젝트 파일을 열고 첫 번째 `<PropertyGroup>` 요소 끝에 다음 속성을 추가합니다.

|**Project**|**속성**|
|-------|----------|
|앱|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

그런 다음 빌드 단계에서 `AppxBundle` msbuild 인수를 제거합니다.

## 테스트용 로드를 위한 연속 배포 빌드 설정
이 빌드 형식이 완료되면 사용자는 빌드 결과 페이지의 아티팩트 섹션에서 appxbundle 파일을 다운로드할 수 있습니다. 좀 더 완벽한 배포를 만들어 앱을 베타 테스트하려는 경우라면 HockeyApp 서비스를 사용할 수 있습니다. 이 서비스는 베타 테스트, 사용자 분석 및 크래시 진단에 대한 고급 기능을 제공합니다.


### 빌드에 버전 번호 적용

매니페스트 파일에는 앱 버전 번호가 포함되어 있습니다.  버전 번호를 변경하려면 소스 제어 리포지토리에 매니페스트 파일을 업데이트합니다. 앱의 버전 번호 업데이트하는 다른 방법은 VSTS에서 생성한 빌드 번호를 사용하여 앱을 컴파일하기 바로 전에 앱 매니페스트를 수정하는 것입니다. 이러한 변경 내용은 소스 코드 리포지토리로 커밋하지 마세요.

빌드 정의에서 버전 관리 빌드 번호 형식을 정의한 다음 결과로 생성되는 버전 번호를 사용하여 컴파일하기 전에 AppxManifest 및 AssemblyInfo.cs(옵션) 파일을 업데이트해야 합니다.

빌드 정의의 *일반* 탭에서 빌드 번호 형식을 정의합니다.

![빌드 버전](images/building-screen12.png) 

예를 들어 빌드 번호 형식을 다음 값으로 설정하려면  
``` 
$(BuildDefinitionName)_1.1.$(DayOfYear)$(Rev:r).0 
```

VSTS에서 다음과 같은 버전 번호를 생성합니다.
```
CI_MyUWPApp_1.1.2501.0
```

>참고: 스토어에서는 버전의 마지막 숫자가 0이어야 합니다.

버전 번호를 추출하여 매니페스트 또는 `AssemblyInfo` 파일에 적용할 수 있도록 사용자 지정 PowerShell 스크립트를 사용합니다([여기](https://go.microsoft.com/fwlink/?prd=12560&pver=14&plcid=0x409&clcid=0x9&ar=DevCenter&sar=docs)에서 사용 가능). 이 스크립트는 환경 변수에서 버전 번호 `BUILD_BUILDNUMBER`를 읽고 AssemblyInfo 및 AppxManifest 파일을 수정합니다. 소스 리포지토리에 이 스크립트를 추가한 후 다음과 같이 PowerShell 빌드 작업을 구성해야 합니다.


![버전 업데이트](images/building-screen13.png) 

`$(AppxVersion)` 변수에는 버전 번호가 포함되어 있습니다. 이 번호는 다른 빌드 단계에서도 사용할 수 있습니다. 


#### 옵션: HockeyApp과 통합
먼저 [HockeyApp](https://marketplace.visualstudio.com/items?itemName=ms.hockeyapp) Visual Studio 확장을 설치합니다. 

>참고: 이 확장은 VSTS 관리자로 설치해야 합니다. 


![hockey 앱](images/building-screen14.png) 

다음으로 [HockeyApp을 VSTS(Visual Studio Team Services) 또는 TFS(Team Foundation Server)와 함께 사용하는 방법](https://support.hockeyapp.net/kb/third-party-bug-trackers-services-and-webhooks/how-to-use-hockeyapp-with-visual-studio-team-services-vsts-or-team-foundation-server-tfs) 가이드를 사용하여 HockeyApp 연결을 구성합니다. Microsoft 계정, 소셜 미디어 계정 또는 메일 주소만 사용해도 HockeyApp 계정을 설정할 수 있습니다. 무료 요금제에서는 앱 2개, 소유자 한 명, 데이터 무제한을 제공합니다.

그런 다음 HockeyApp 앱을 수동으로 만들거나, 기존 appx 패키지 파일을 업로드하여 만들 수 있습니다. 자세한 내용은 [How to create a new app](https://support.hockeyapp.net/kb/app-management-2/how-to-create-a-new-app)(새 앱을 만드는 방법)을 참조하세요.  

기존 appx 패키지 파일을 사용하려면 빌드 단계를 추가하고 빌드 단계의 이진 파일 경로 매개 변수를 설정합니다. 

![hockey 앱 구성](images/building-screen15.png) 

이 매개 변수를 설정하려면 앱 이름, AppxVersion 변수 및 지원되는 플랫폼을 함께 다음과 같이 한 개의 문자열로 결합합니다.

``` 
$(Build.ArtifactStagingDirectory)\AppxPackages\MyUWPApp_$(AppxVersion)_Test\MyUWPApp_$(AppxVersion)_x86_x64_ARM.appxbundle
```

>참고: HockeyApp 작업을 사용하면 기호 파일에 대한 경로를 지정할 수 있지만 번들을 사용하여 기호(appxsym 파일)를 포함하는 것이 좋습니다.

테스트용으로 로드된 패키지를 설치하고 실행하는 작업은 이 가이드에서 [나중](#sideloading-best-practices)에 살펴보겠습니다. 

## 저장소에 패키지를 제출하는 연속 배포 빌드 설정 

저장소 제출 패키지를 생성하려면 Visual Studio에서 스토어 연결 마법사를 사용하여 스토어에 앱을 연결합니다.

![스토어에 연결](images/building-screen16.png) 

>참고: 이 마법사에서는 스토어 연결 정보를 포함하는 Package.StoreAssociation.xml이라는 파일이 생성됩니다. GitHub와 같은 공용 리포지토리에 소스 코드를 저장하는 경우 이 파일에는 해당 계정에 대한 예약된 앱 이름이 모두 포함됩니다. 이 파일을 공개하기 전에 제외하거나 삭제할 수 있습니다.

앱을 게시하는 데 사용된 개발자 센터 계정에 액세스할 수 없으면 [타사 앱을 빌드 중인 경우 스토어 앱을 패키징하는 방법은 무엇인가요?](https://blogs.windows.com/buildingapps/2015/12/15/building-an-app-for-a-3rd-party-how-to-package-their-store-app/#e35YzR5aRG6uaBqK.97) 문서의 지침에 따라 수행하면 됩니다. 

그런 다음 빌드 단계에서 다음 매개 변수를 포함하고 있는지 확인해야 합니다.

```
/p:UapAppxPackageBuildMode=StoreUpload 
```

이렇게 하면 스토어에 전송할 수 있는 appxupload 파일이 생성됩니다.


#### 자동 저장소 제출 구성

저장소 API와 통합하는 Windows 스토어용 Visual Studio Team Services 확장을 사용하여 저장소에 appxupload 패키지를 보냅니다.

Azure AD(Active Directory)를 사용하여 개발자 센터 계정에 연결한 다음 AD에 요청을 인증할 수 있는 앱을 만들어야 합니다. 작업을 수행하는 확장 페이지의 지침에 따라 수행할 수 있습니다. 

확장을 구성한 후에 빌드 작업을 추가하고 앱 ID와 appxupload 파일의 위치를 사용하여 구성할 수 있습니다.

![개발자 센터 구성](images/building-screen17.png) 

여기서 `Package File` 매개 변수 값은 다음과 같습니다.

```
$(Build.ArtifactStagingDirectory)\
AppxPackages\MyUWPApp__$(AppxVersion)_x86_x64_ARM_bundle.appxupload
```

>참고. 이 빌드는 수동으로 활성화해야 합니다. 기존 앱 업데이트에 사용할 수 있지만 스토어에 대한 첫 번째 제출에는 사용할 수 없습니다. 자세한 내용은 [Windows 스토어 서비스를 사용하여 제출 만들기 및 관리](https://msdn.microsoft.com/windows/uwp/monetize/create-and-manage-submissions-using-windows-store-services)를 참조하세요.

## 모범 사례

<span id="sideloading-best-practices"/>
### 앱의 테스트용 로드에 대한 모범 사례

스토어에 게시하지 않고 앱을 배포하려는 경우 디바이스에서 앱 패키지에 서명하는 데 사용된 인증서를 신뢰하는 한 디바이스에 앱을 직접 테스트용으로 로드할 수 있습니다. 

앱을 설치하려면 `Add-AppDevPackage.ps1` PowerShell 스크립트를 사용하세요. 이 스크립트는 로컬 컴퓨터에 대해 신뢰할 수 있는 루트 인증 섹션에 인증서를 추가한 다음 appx 파일을 설치하거나 업데이트합니다.

#### Windows10 1주년 업데이트를 사용하여 앱을 테스트용으로 로드
Windows10 1주년 업데이트에서 appxbundle 파일을 두 번 클릭하고 대화 상자에서 설치 단추를 선택하여 앱을 설치할 수 있습니다. 


![rs1에 테스트용으로 로드](images/building-screen18.png) 

>참고: 이 메서드는 인증서 또는 관련된 종속성을 설치하지 않습니다.

VSTS 또는 HockeyApp 등 웹 사이트에서 appx 패키지를 배포하려는 경우 브라우저에서 신뢰할 수 있는 사이트 목록에 해당 사이트를 추가해야 합니다. 그렇지 않으면 파일이 잠금 상태로 표시됩니다. 

<span id="certificates-best-practices"/>
### 인증서 서명에 대한 모범 사례 
Visual Studio는 각 프로젝트에 대한 인증서를 생성합니다. 이렇게 하면 유효한 인증서의 조정된 목록을 유지 관리하기가 어렵습니다. 앱을 여러 개 만드는 경우에는 모든 앱에 서명하는 단일 인증서를 만들 수 있습니다. 그런 다음 인증서를 신뢰하는 각 디바이스에 다른 인증서를 설치하지 않고 임의의 앱을 테스트용으로 로드할 수 있게 됩니다. 자세한 내용은 [앱 패키지 서명 인증서를 만드는 방법](https://msdn.microsoft.com/en-us/library/windows/desktop/jj835832(v=vs.85).aspx)을 참조하세요.


#### 서명 인증서 만들기
인증서를 만들려면 [MakeCert.exe](https://msdn.microsoft.com/en-us/library/windows/desktop/ff548309(%09v=vs.85).aspx) 도구를 사용합니다. 다음 예제에서는 MakeCert.exe 도구를 사용하여 인증서를 만듭니다.

```
MakeCert /n publisherName /r /h 0 /eku "1.3.6.1.5.5.7.3.3,1.3.6.1.4.1.311.10.3.13" /e expirationDate /sv MyKey.pvk MyKey.cer
```

암호로 보호된 개인 키를 포함하는 PFX 파일을 생성하려면 Pvk2Pfx 도구를 사용할 수 있습니다.

이러한 인증서를 각 컴퓨터 역할에 제공합니다.

|**컴퓨터**|**사용**|**Certificate**|**인증서 저장소**|
|-----------|---------|---------------|---------------------|
|개발자/빌드 컴퓨터|빌드 서명|MyCert.PFX|현재 사용자/개인|
|개발자/빌드 컴퓨터|Run|MyCert.cer|로컬 컴퓨터/신뢰할 수 있는 사용자|
|사용자|Run|MyCert.cer|로컬 컴퓨터/신뢰할 수 있는 사용자|

>참고: 사용자가 이미 신뢰할 수 있는 엔터프라이즈 인증서를 사용할 수도 있습니다.

#### UWP 앱에 서명
Visual Studio 및 MSBuild는 앱에 서명하는 데 사용하는 인증서를 관리하는 다양한 옵션을 제공합니다.

한 가지 옵션으로 솔루션에 개인 키와 인증서를 포함하고(일반적으로 .PFX 파일 형식) 프로젝트 파일에서 pfx를 참조하는 것입니다. 매니페스트 편집기의 패키지 탭을 사용하여 관리할 수 있습니다.


![인증서 만들기](images/building-screen19.png) 

또 다른 옵션은 빌드 컴퓨터(현재 사용자/개인)에 인증서를 설치한 다음 인증서 저장소 옵션에서 선택을 사용하는 것입니다. 프로젝트를 빌드하는 데 사용할 모든 컴퓨터에서 인증서를 설치해야 하므로 인증서의 지문을 프로젝트 파일에서 지정합니다.

#### 대상 디바이스에서 서명 인증서 신뢰
앱에서 인증서를 설치하기 전에 먼저 인증서를 신뢰해야 합니다. 

로컬 컴퓨터 인증서 저장소에 신뢰할 수 있는 사용자 또는 신뢰 루트 위치에 인증서의 공개 키를 등록합니다.

인증서를 등록하는 가장 쉬운 방법은 .cer 파일을 두 번 클릭한 다음 마법사의 단계에 따라 로컬 컴퓨터와 신뢰할 수 있는 사용자 저장소에서 인증서를 저장하는 것입니다.

## 관련 항목
* [Windows용 .NET 앱 빌드](https://www.visualstudio.com/en-us/docs/build/get-started/dot-net) 
* [UWP 앱 패키징](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)
* [Windows 10에서 LOB 앱을 테스트용으로 로드](https://technet.microsoft.com/itpro/windows/deploy/sideload-apps-in-windows-10)
* [앱 패키지 서명 인증서를 만드는 방법](https://msdn.microsoft.com/en-us/library/windows/desktop/jj835832(v=vs.85).aspx)



<!--HONumber=Nov16_HO1-->

