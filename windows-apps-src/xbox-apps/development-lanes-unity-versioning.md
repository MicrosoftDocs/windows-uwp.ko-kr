---
title&#58; Unity&#58; UWP 프로젝트 버전 제어 작성자&#58; JordanEllis6809
---

# Unity&#58; UWP 프로젝트 버전 제어

UWP를 사용하여 Xbox용 Unity 게임을 아직 빌드하지 않았나요?  먼저 [여기](development-lanes-unity.md)를 읽어 보세요.

생성된 UWP 디렉터리 부분을 버전 제어에 추가하려는 몇 가지 다양한 이유가 있으며, 그중 하나가 종속성 추가입니다(예: Xbox Live SDK).  이 시나리오를 이 자습서의 예로 사용합니다. 이러한 예제가 프로젝트의 개별 요구를 해결하는 데 도움이 되기를 바랍니다.

***고지 사항: 버전 제어 솔루션으로 Git를 사용합니다.  다른 솔루션을 사용하는 경우 개념을 이해해야 합니다.***

다시 한 번 말하면 예제 게임, ***ScrapyardPhoenix***의 디렉터리는 현재 다음과 같습니다.

![빌드 대상 폴더](images/build-destination.png)

그리고 UWP 디렉터리는 다음과 같습니다.

![UWP VS 솔루션](images/uwp-vs-solution.png)

이 디렉터리에서 한 폴더 즉, ***ScrapyardPhoenix***(여기에 게임의 이름 삽입) 폴더만 살펴봅니다.  다른 모든 폴더는 다음과 같이 버전 제어에서 무시할 수 있습니다.

***.gitignore 파일에 익숙하지 않으세요?  
            [여기에서](https://git-scm.com/docs/gitignore) 자세히 알아보세요.***

    ##################################################################
    # The original .gitignore file can be found at
    # https://github.com/github/gitignore/blob/master/Unity.gitignore
    ##################################################################

    # standard ignores for a Unity Project
    ...

    # ignore the whole UWP directory
    /UWP/**

    # except we want to keep... (this line will be modified and removed further down)
    !/UWP/ScrapyardPhoenix/


            `UWP/ScrapyardPhoenix` 폴더 내에서 몇 가지 다른 파일과 폴더를 선택하여 버전 제어에 추가하겠습니다.  먼저 전체적으로 자세히 살펴보겠습니다.

![UWP 빌드 디렉터리](images/uwp-build-directory.png)  

## 폴더  

`Assets` | 
            ***포함*** | Windows 스토어 이미지가 포함되어 있습니다.  
`Data`   | 
            ***무시*** | Unity에서 프로젝트를 컴파일하는 폴더입니다(장면, 셰이더, 스크립트, Prefabs 등).  
`Dependencies` | 
            ***포함*** | 이 폴더는 모든 UWP 종속성을 유지하기 위해 만든 폴더입니다(예: XboxLiveSDK.dll).  
`Properties` | 
            ***포함*** | 개발자가 수정할 수 있는 추가 고급 설정이 포함되어 있습니다.  
`Unprocessed` | 
            ***무시*** | Unity `.dll` 및 `.pdb` 파일이 포함되어 있습니다.  

## 파일  

`App.cs` | 
            ***포함*** | UWP 응용 프로그램에 대한 진입점입니다.  이 파일은 다른 소스 파일을 사용하여 수정하고 확장할 수 있습니다.  
`Package.appxmanifest` | 
            ***포함*** | AppX의 패키지 매니페스트입니다.  
`project.json` | 
            ***포함*** | `*.csproj`가 종속된 NuGet 패키지를 설명합니다.  
`ScrapyardPhoenix.csproj` | 
            ***포함*** | UWP 빌드 대상을 설명합니다.  UWP 프로젝트에 다른 종속성을 추가하는 경우 이 `*.csproj` 파일에 해당 정보가 포함됩니다.  
`ScrapyardPhoenix.csproj.user` | 
            ***무시*** | 이 파일에 로컬 사용자 정보가 포함되어 있습니다.

## 결과 .gitignore

    ##################################################################
    # The original .gitignore file can be found at
    # https://github.com/github/gitignore/blob/master/Unity.gitignore
    ##################################################################

    # standard ignores for a Unity Project
    ...

    # ignore the whole UWP directory
    /UWP/**

    # except we want to keep...
    !/UWP/ScrapyardPhoenix/Assets/*
    !/UWP/ScrapyardPhoenix/Dependencies/*
    !/UWP/ScrapyardPhoenix/Properties/*
    !/UWP/ScrapyardPhoenix/App.cs
    !/UWP/ScrapyardPhoenix/Package.appxmanifest
    !/UWP/ScrapyardPhoenix/project.json
    !/UWP/ScrapyardPhoenix/ScrapyardPhoenix.csproj

작업을 수행하면 팀 동료는 사용자가 생성한 UWP 프로젝트와 동기화됩니다.  이제 UWP 프로젝트에 다른 자산, 소스 및 종속성을 언제든지 추가할 수 있습니다.

UWP 폴더 버전 관리의 일부 추가 예는 [이러한 예제](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview)에서 찾아볼 수 있습니다.

## UWP 앱에 종속성 추가

UWP 응용 프로그램에 종속성을 추가하는 작업은 다른 Visual Studio 프로젝트에서와 마찬가지로 수행됩니다.  여기에서는 솔루션에서 종속성을 추가할 프로젝트를 좀 더 간단하게 파악할 수 있습니다.  다음은 해당 프로젝트가 어느 것인지를 보여 주는 이미지입니다(강조 표시되어 있음).

![UWP 솔루션](images/uwp-solution.png)


            ***ScrapyardPhoenix(유니버설 Windows)***는 참조를 추가할 프로젝트(예: Xbox Live SDK)입니다.



<!--HONumber=Jul16_HO1-->


