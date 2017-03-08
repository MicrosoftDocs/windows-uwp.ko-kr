---
author: awkoren
Description: "이 가이드에서는 Visual Studio 솔루션을 구성하여 변환된 데스크톱 브리지 앱을 편집, 디버그 및 패키징하는 방법을 설명합니다."
Search.Product: eADQiWindows 10XVcnh
title: "Visual Studio를 사용한 .NET 데스크톱 앱용 데스크톱 브리지 패키징 가이드"
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 8aa68312d6ce81c809c79ddcafe7732944a628be
ms.lasthandoff: 02/08/2017

---

# <a name="desktop-bridge-packaging-guide-for-net-desktop-apps-with-visual-studio"></a>Visual Studio를 사용한 .NET 데스크톱 앱용 데스크톱 브리지 패키징 가이드

Windows 10 1주년 업데이트를 통해 개발자는 데스크톱 브리지를 사용하여 기존 Win32 앱을 새 패키지 모델(.appx)로 패키징할 수 있게 되었습니다. 이는 스토어 게시나 편리한 테스트용 로드를 가능하게 합니다. 이 가이드는 Visual Studio 솔루션을 구성하여 앱을 편집, 디버그 및 패키징하는 방법을 설명합니다. 

시작하려면 [데스크톱 브리지를 사용하여 기존 앱 및 게임을 Windows 스토어로 가져오기](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge)에서 양식을 작성하세요. 그러면 온보딩 프로세스를 시작하라는 메시지가 표시됩니다. 데스크톱 브리지 앱을 제출하도록 계정이 승인되면 이 문서의 지침을 따라 업로드할 appxupload 패키지를 준비합니다. 

> 데스크톱 브리지를 사용하는 과정에서 겪은 문제에 대한 피드백이 있습니까? 기능을 제안할 수 있는 최고의 장소는 [Windows 개발자 UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)입니다. 질문이나 버그 보고서에 대해서는 [유니버설 Windows 앱 개발 포럼](https://social.msdn.microsoft.com/Forums/home?forum=wpdevelop)을 자세히 검토해 보세요.

## <a name="default-universal-windows-platform-packages"></a>기본 유니버설 Windows 플랫폼 패키지

Visual Studio를 사용하여 릴리스 패키지를 생성하고 디버그하여 Windows 스토어를 사용하여 배포하거나 테스트용으로 로드할 수 있습니다. 패키지 작성을 원활하게 하기 위해 Visual Studio가 appxupload 파일을 스토어에 제출할 준비를 합니다. 자세한 내용은 [UWP 앱 패키징](..\packaging\packaging-uwp-apps.md)을 참조하세요.

## <a name="desktop-bridge-packages"></a>데스크톱 브리지 패키지

[데스크톱 브리지](desktop-to-uwp-root.md)는 응용 프로그램 패키지(appx) 내에서 Win32 이진을 통합할 수 있는 서로 다른 구성을 허용합니다. 데스크톱 브리지 전체의 진행은 네 가지 주요 단계를 진행하는 것으로 생각할 수 있습니다. 

- **1단계 - 변환**: 코드를 최소한으로 또는 아예 변경하지 않고 기존 Win32 이진을 패키징합니다.
- **2단계 - 향상**: 기존 Win32 코드에서 Windows.winmd를 참조하여 기존 앱에 일부 기본 UWP 기능(예: live tile)을 포함합니다.
- **3단계 - 확장**: 기존 앱에 고급 UWP 기능(예: 백그라운드 작업)을 포함시킵니다. UWP 및 Win32 구성 요소가 관리되는 언어(예: C# 또는 VB.Net)를 사용하여 빌드된 경우, 결과 패키지는 혼합된 이진을 가지게 되므로 올바른 .NET 네이티브 처리를 위해 신중하게 처리되어야 합니다. 
- **4단계 - 마이그레이션**: UI를 최신 XAML 및 C#/VBNET으로 마이그레이션했으나 여전히 레거시 Win32 코드가 있습니다. 진입점은 이제 UWP .NET 실행 파일이지만, 일부 Win32 API를 사용하는 이진이 패키지에 아직 있습니다.

다음 표는 각 네 단계에서 각 앱에 대한 일부 차이점을 요약한 것입니다. 

| 단계 | 이진 | 진입점 | .NET 네이티브 | F5 디버그 |
|---|---|---|---|---|
| 1(변환) | Win32 | Win32 | 해당 없음 | VS 확장 |
| 2(향상) | WinMD 참조 | Win32 | 해당 없음 | VS 확장 |
| 3(확장) | Win32 + CoreCLR(*) | Win32 | 사용자에 의해(**) | VS 확장 |
| 4(마이그레이션)    | CoreCLR(*) + Win32 | UWP | 사용자에 의해(**) | VS |
| 5(UWP) | CoreCLR | UWP |스토어에 의해 | VS |

(*) [CoreCLR](https://github.com/dotnet/coreclr)은 관리되는 언어(C#/VB.NET)로 작성된 UWP 구성 요소가 참조하는 .NET Core 런타임을 말합니다. 이러한 구성 요소에는 .NET 네이티브 처리도 필요합니다.

(**) 3단계와 4단계에서, 사용자는 스토어에 게시하기 전에 .NET 네이티브 어셈블리 및 해당 기호를 생성하기 위해 CoreCLR 어셈블리를 처리합니다.

## <a name="configure-your-visual-studio-solution"></a>Visual Studio 솔루션 구성

Visual Studio에는 매니페스트 편집기와 패키지 만들기 마법사와 같이 응용 프로그램 패키지를 구성하는 데 필요한 도구가 포함되어 있습니다. 이러한 도구를 사용하려면 앱에 대한 appx 컨테이너로 작동하는 UWP 프로젝트가 필요합니다. C#, VB.NET, C++ 또는 JavaScript를 포함한 모든 UWP 프로젝트를 사용할 수 있지만, C#, VB.NET, C++ 프로젝트에는 일부 알려진 문제가 있으므로(이 문서 뒤에 나오는 [알려진 문제](#known-issues-anchor) 섹션 참조) 이 예제에서는 JavaScript를 사용하겠습니다. 

appx 응용 프로그램 모델의 컨텍스트에서 앱을 디버그하려는 경우, F5 appx 디버깅을 가능하게 하는 다른 프로젝트를 추가해야 합니다. 자세한 내용은 [데스크톱 브리지 앱 디버깅](#debugging-anchor)을 참조합니다.

1단계를 시작해 보겠습니다.

### <a name="step-1-convert"></a>1단계: 변환

이 단계는 기존 Win32 프로젝트에서 데스크톱 브리지 앱을 만드는 방법을 보여줍니다. 이 예제에서는 레지스트리에 읽기 및 쓰기 작업을 수행하는 기본 WinForms 프로젝트를 사용할 것입니다.

![](images/desktop-to-uwp/net-1.png)

#### <a name="add-the-uwp-project"></a>UWP 프로젝트 추가 

데스크톱 브리지 패키지를 만들려면 JavaScript UWP 프로젝트를 동일한 솔루션에 추가합니다.

> 참고: JavaScript UWP 템플릿을 사용하더라도 JavaScript 코드는 전혀 작성하지 않습니다. 프로젝트만 도구로 사용합니다.

![](images/desktop-to-uwp/net-2.png)

#### <a name="add-the-win32-binaries-to-the-win32-folder"></a>Win32 이진을 win32 폴더에 추가

모든 Win32 이진은 win32라는 폴더에 있는 UWP 프로젝트에 저장됩니다. 정확하게 win32라는 이름이 필요하지는 않습니다. 원하는 모든 이름을 사용할 수 있습니다.

Visual Studio를 사용하는 경우 각 빌드 후 파일을 복사하도록 프로젝트를 자동화하여 개발 워크플로를 향상시킬 수 있습니다. 다음과 같이 프로젝트 파일(이 예에서는 .csproj)을 편집하여 AfterBuild 대상을 포함시켜 모든 Win32 출력 파일을 UWP 프로젝트의 win32 폴더에 복사하도록 합니다. 

```xml
  <Target Name="AfterBuild">
    <PropertyGroup>
      <TargetUWP>..\MyDesktopApp.Package\win32\</TargetUWP>
    </PropertyGroup>
     <ItemGroup>
       <Win32Binaries Include="$(TargetDir)\*" />
     </ItemGroup>
    <Copy SourceFiles="@(Win32Binaries)" DestinationFolder="$(TargetUWP)" />
  </Target>
```

다른 도구를 사용하여 Win32 이진을 생성할 경우, 런타임에 필요한 모든 파일을 win32 폴더로 복사합니다. 

#### <a name="edit-the-app-manifest-to-enable-the-desktop-bridge-extensions"></a>앱 매니페스트를 편집하여 데스크톱 브리지 확장을 사용하도록 설정

이 템플릿에는 데스크톱 브리지 확장을 추가하는 데 사용할 수 있는 package.appxmanifest가 포함되어 있습니다. 이 파일을 편집하려면 마우스 오른쪽 단추로 클릭하고, "코드 보기"를 선택하고 이러한 항목을 추가 또는 수정합니다. 

- `<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10" xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10" xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities" IgnorableNamespaces="uap rescap">`

- `<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.0" MaxVersionTested="10.0.14393.0" />`

- `<rescap:Capability Name="runFullTrust" />`

- `<Application Id="MyDesktopAppStep1" Executable="win32\MyDesktopApp.exe" EntryPoint="Windows.FullTrustApplication">`

매니페스트 파일의 전체 예는 다음과 같습니다. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
        xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"

        xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
        xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
        IgnorableNamespaces="uap rescap mp">
  <Identity Name="MyDesktopAppStep1"
            ProcessorArchitecture="x64"
            Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US"
            Version="1.0.0.5" />
  <mp:PhoneIdentity PhoneProductId="6f6600a4-6da1-4d91-b493-35808d01f8de" PhonePublisherId="00000000-0000-0000-0000-000000000000" />
  <Properties>
    <DisplayName>MyDesktopAppStep1</DisplayName>
    <PublisherDisplayName>CN=Test</PublisherDisplayName>
    <Logo>Assets\SampleAppx.150x150.png</Logo>
  </Properties>
  <Resources>
    <Resource Language="en-us" />
  </Resources>
  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" 
                        MinVersion="10.0.14393.0" 
                        MaxVersionTested="10.0.14393.0" />
  </Dependencies>
  <Capabilities>
    <rescap:Capability Name="runFullTrust" />
  </Capabilities>
  <Applications>
    <Application Id="MyDesktopAppStep1" 
                 Executable="win32\MyDesktopApp.exe" 
                 EntryPoint="Windows.FullTrustApplication">
      <uap:VisualElements DisplayName="MyDesktopAppStep1" 
                          Description="MyDesktopAppStep1" 
                          BackgroundColor="#777777" 
                          Square150x150Logo="Assets\SampleAppx.150x150.png" 
                          Square44x44Logo="Assets\SampleAppx.44x44.png">
      </uap:VisualElements>
    </Application>
  </Applications>
</Package>
```

#### <a name="configure-the-win32-binaries"></a>Win32 이진 구성

앱에 필요한 이진을 출력 패키지에 포함하도록 하려면 Visual Studio에서 각 파일을 선택합니다. 속성을 "콘텐츠"로 설정하고 빌드 동작을 "새 버전이면 복사"로 설정합니다. 

![](images/desktop-to-uwp/net-3.png)

소스 코드 리포지토리로 이진 파일이 커밋되는 것을 피하기 위해서는 .gitignore 파일을 사용하여 win32 폴더의 모든 파일을 제외하도록 할 수 있습니다. 

#### <a name="optional-use-wildcards-to-specify-the-files-in-your-win32-folder"></a>선택 사항: 와일드카드를 사용하여 win32 폴더의 파일을 지정

Win32 앱에 여러 파일이 필요한 경우, 프로젝트 파일을 편집하여 와일드 카드 식 기반 "콘텐츠"로 표시될 파일을 지정하는 데 와일드카드를 지정할 수 있습니다. 텍스트 편집기에서 . jsproj 파일을 텍스트 편집기에서 열고 필요한 파일을 다음과 같이 포함시킵니다.

```xml
<Content Include="win32\*.dll">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
<Content Include="win32\*.exe">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
<Content Include="win32\*.config">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
<Content Include="win32\*.pdb">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
```

### <a name="step-2-enhance"></a>2단계: 향상

Win32 코드에서 사용할 수 있는 UWP API를 호출하고자 하려면 `\Program Files (x86)\Windows Kits\10\UnionMetadata\Windows.winmd`로의 참조를 추가합니다. 앱에서 사용할 수 있는 전체 목록은 [데스크톱 브리지로 변환된 앱을 위해 지원되는 UWP API](desktop-to-uwp-supported-api.md) 문서에 나열되어 있습니다.  

이 파일은 Windows 10에서 필요하지 않으므로 배포할 필요가 없습니다. 참조 속성에서 "로컬 복사" 속성을 false로 설정합니다.

![](images/desktop-to-uwp/net-4.png)

Win32 이진을 추가하려면 1단계와 동일한 지침을 따릅니다. 

### <a name="step-3-extend"></a>3단계: 확장 

이 예에서는 Win32 앱을 백그라운드 작업으로 확장합니다. 이렇게 하려면 백그라운드 작업을 UWP 앱의 package.appxmanifest에 등록하고 다음과 같이 백그라운드 작업을 구현하는 프로젝트에 참조를 추가해야 합니다.

```xml
<Extensions>
  <Extension Category="windows.backgroundTasks" 
              EntryPoint="BackgroundTasks.MyBackgroundTask">
    <BackgroundTasks>
      <Task Type="timer" />
    </BackgroundTasks>
  </Extension>
</Extensions>
```

백그라운드 작업이 C# 또는 VB.NET으로 구현되는 경우, 출력 결과에는 CoreCLR 이진이 포함되며 이는 3단계와 4단계에 설명한 대로 스토어에 제출하기 전에 .NET 네이티브 도구 체인에 의해 처리되어야 합니다. 혼합된 이진으로 appxupload를 만듭니다.

### <a name="step-4-migrate"></a>4단계: 마이그레이션

이 시나리오에는 이미 C# UWP 진입점이 있으므로, 추가 UWP 프로젝트를 추가할 필요가 없습니다. 그러나 1단계에 설명된 단계를 따라 Win32 이진을 포함시키고 구성해야 합니다.

Win32 프로세스를 실행하려면 [**FullTrustProcessLauncher**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.FullTrustProcessLauncher) API를 사용합니다. API를 사용하기 위해 앱의 매니페스트에 다음과 같이 데스크톱 확장과 *fullTrustProcess* 기능을 추가해야 합니다. 

```xml
..
xmlns:desktop=http://schemas.microsoft.com/appx/manifest/desktop/windows10
..
<desktop:Extension Category="windows.fullTrustProcess" 
                    Executable="win32\MyDesktopApp.exe" />
```

## <a name="generate-packages-for-your-desktop-bridge-app"></a>데스크톱 브리지 앱에 대한 패키지 생성

위 지침을 따랐다면 [UWP 앱 패키징](..\packaging\packaging-uwp-apps.md)에 설명된 대로 Visual Studio를 사용하여 패키지를 만들 준비가 된 것입니다. 

### <a name="steps-1-and-2-create-appxupload-with-win32-binaries"></a>1, 2단계: Win32 이진을 사용하여 appxupload 만들기

*fullTrust* 기능이 있는 패키지를 제출하려면 appxsym 파일을 생성하고, 이 파일에 각 플랫폼에 대한 기호를 포함시켜야 하며, appx 플랫폼 패키지를 포함하는 번들을 만들어야 합니다.

1, 2단계에서는 패키지에 어떤 CoreCLR 이진도 포함되지 않으므로 어떤 플랫폼을 선택할지 걱정할 필요가 없습니다. 아래 그림과 같이 "보통" 및 "릴리스(모든 CPU)"를 선택합니다.

![](images/desktop-to-uwp/net-5.png)

"스토어 패키지 생성" 옵션을 선택하면 마법사를 스토어에 제출 준비가 appxupload 파일을 생성합니다.

### <a name="step-3-and-4-create-appxupload-with-mixed-binaries"></a>3, 4단계: 혼합된 이진으로 appxupload 만들기

릴리스할 빌드도 만들어야 하며, 이 경우는 .NET 네이티브가 각 플랫폼에 대한 네이티브 이진을 생성하는 데 필요하므로 우리가 대상으로 하는 플랫폼을 반드시 지정해야 합니다.

![](images/desktop-to-uwp/net-6.png)

새 appxupload 파일을 만들려면 새 zip 아카이브를 만들어 _Test 폴더에서 생성된 appxsym 및 appxbundle 파일을 포함시킵니다.

appxsym 및 appxbundle 파일이 포함된 새 zip 파일을 만들고 appxupload로 확장명을 변경합니다.

![](images/desktop-to-uwp/net-7.png)

<span id="debugging-anchor" />
## <a name="debugging-your-desktop-bridge-app"></a>데스크톱 브리지 앱 디버깅

디버깅(Ctrl + F5) 없이도 Visual Studio에서 프로젝트를 시작할 수 있지만, Visual Studio가 실행 프로세스에 자동으로 연결할 수 없는 알려진 문제가 있습니다. 하지만 다음 연결 방법 중 하나를 사용하여 나중에 연결할 수 있습니다.

### <a name="attach-to-the-running-app"></a>실행 중인 앱에 연결

#### <a name="attach-to-an-existing-process"></a>기존 프로세스에 연결

Ctrl + F5를 사용하여 성공적으로 앱을 시작했다면, Win32 프로세스에 연결할 수 있습니다. 하지만 .NET 네이티브 모듈은 디버깅할 수 없습니다. 

![](images/desktop-to-uwp/net-8.png)

#### <a name="attach-to-an-installed-app"></a>설치된 앱에 연결

디버그 -> 기타 디버그 대상 -> 설치된 앱 패키지 디버그 옵션을 사용하여 모든 기존 Appx 패키지에 연결할 수 있습니다.

![](images/desktop-to-uwp/net-9.png)

로컬 컴퓨터를 선택하거나 원격 컴퓨터에도 연결할 수 있습니다.

![](images/desktop-to-uwp/net-10.png)

이 옵션을 사용하면 .NET 네이티브 코드를 디버깅할 수 있습니다.

### <a name="use-visual-studio-extension-to-debug-your-desktop-bridge-app"></a>Visual Studio 확장을 사용하여 데스크톱 브리지 앱 디버그 

F5를 사용한 앱 디버그 실행 방법을 선호한다면, Visual Studio 갤러리에서 Visual Studio 2017 확장 [데스크톱 브리지 디버깅 프로젝트](https://marketplace.visualstudio.com/items?itemName=VisualStudioProductTeam.DesktoptoUWPPackagingProject)을 설치해야 합니다.

이 프로젝트를 사용하면 Visual Studio를 사용하거나(이 문서에서 설명한 대로) Desktop App Converter를 사용하여 UWP로 마이그레이션된 모든 Win32 앱을 디버깅할 수 있습니다.

#### <a name="add-the-debugging-project-to-your-solution"></a>솔루션에 디버깅 프로젝트 추가

시작하려면, 새 데스크톱 브리지 디버깅 프로젝트를 솔루션에 추가합니다.

![](images/desktop-to-uwp/net-11.png)

이 프로젝트를 구성하려면 디버깅에 사용하려는 각 구성/플랫폼에 대해 속성 창에서 PackageLayout 속성을 정의해야 합니다.
Debug/x86의 구성을 위해 상대 경로 `..\MyDesktopApp.Package\bin\x86\Debug`를 사용하여 UWP 프로젝트의 bin\x86\debug 폴더에 패키지 레이아웃 속성을 설정합니다. 

![](images/desktop-to-uwp/net-12.png)

그리고 AppXFileLayout.xml 파일을 편집하여 진입점을 지정합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project ToolsVersion="14.0" 
         xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <MyProjectOutputPath>$(PackageLayout)</MyProjectOutputPath>
  </PropertyGroup>
  <ItemGroup>
    <LayoutFile Include="$(MyProjectOutputPath)\win32\MyDesktopApp.exe">
      <PackagePath>$(PackageLayout)\win32\MyDesktopApp.exe</PackagePath>
    </LayoutFile>
  </ItemGroup>
</Project>
```

마지막으로, 프로젝트가 적절한 순서로 빌드되었는지 확인하기 위해 솔루션 종속성을 구성해야 합니다. 

예를 들어 3단계에서 생성된 솔루션을 검토해 보겠습니다.

![](images/desktop-to-uwp/net-13.png)

빌드 순서를 구성하려면 프로젝트 종속성 구성을 사용할 수 있습니다. 솔루션을 마우스 오른쪽 단추로 클릭하고 프로젝트 종속성 옵션을 선택합니다. 올바른 종속성을 설정하면 아래와 같이(3 단계) 빌드 순서를 확인할 수 있습니다.

![](images/desktop-to-uwp/net-14.png)

<span id="known-issues-anchor" />
## <a name="known-issues-with-cvbnet-and-c-uwp-projects"></a>C#/VB.NET 및 C++ UWP 프로젝트에 대한 알려진 문제

앱 패키지를 위해 C# 프로젝트 사용을 선호하는 경우, 다음과 같은 알려진 문제를 고려해야 합니다. 

- **디버그에서 앱 빌드 시 오류 발생: Microsoft.Net.CoreRuntime.targets(235,5): 오류: 사용자 지정 진입점 실행 파일이 있는 응용 프로그램은 지원되지 않습니다. 패키지 메니페스트에서 응용 프로그램 요소의 실행 파일 특성을 확인합니다.** 해결 방법으로 릴리스 모드를 대신 사용합니다.

- **UWP 프로젝트의 루트 폴더에 저장된 Win32 이진은 릴리스에서 제거되어야 합니다**. Win32 이진을 저장하는 폴더를 사용하지 않을 경우, .NET 네이티브 컴파일러가 이를 최종 패키지에서 제거하므로 실행 파일 진입점을 찾을 수 없으면 매니페이스 검사 오류가 발생합니다.


