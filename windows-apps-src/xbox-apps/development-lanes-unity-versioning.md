---
title: Unity - UWP 프로젝트 버전 제어
description: Unity UWP 프로젝트 버전 관리.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 064eaf42fe7d664be273cd7e2222fa5d90be1a11
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608868"
---
# <a name="unity-version-control-your-uwp-project"></a>Unity: 버전 제어 UWP 프로젝트

UWP(유니버설 Windows 플랫폼)를 사용하여 Xbox용 Unity 게임을 아직 빌드하지 않았나요?  먼저 [Xbox의 UWP에 Unity 게임 가져오기](development-lanes-unity.md)를 참조하세요.

생성된 UWP 디렉터리 부분을 버전 제어에 추가하려는 몇 가지 다양한 이유가 있으며, 그중 하나가 종속성 추가입니다(예: Xbox Live SDK).  이 시나리오를 이 자습서의 예로 사용합니다. 이러한 예제가 프로젝트의 개별 요구를 해결하는 데 도움이 되기를 바랍니다.

***고 지 사항: Git 버전 제어 솔루션으로 사용할 것 것입니다.  다른 사용자의 경우 개념 여전히 변환 해야 합니다.***

다시 한 번 말하면 예제 게임, ***ScrapyardPhoenix***의 디렉터리는 현재 다음과 같습니다.

![빌드 대상 폴더](images/build-destination.png)

그리고 UWP 디렉터리는 다음과 같습니다.

![UWP VS 솔루션](images/uwp-vs-solution.png)

이 디렉터리에서 한 폴더 즉, ***ScrapyardPhoenix***(여기에 게임의 이름 삽입) 폴더만 살펴봅니다.  다른 모든 폴더는 다음과 같이 버전 제어에서 무시할 수 있습니다.

***.Gitignore 파일을 사용 하 여 알 수 없는?  참조 [gitignore](https://git-scm.com/docs/gitignore)합니다.***

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

**UWP/ScrapyardPhoenix** 폴더 내에서 몇 가지 다른 파일과 폴더를 선택하여 버전 제어에 추가하겠습니다.  먼저 전체적으로 자세히 살펴보겠습니다.

![UWP 빌드 디렉터리](images/uwp-build-directory.png)  

## <a name="folders"></a>폴더  

`Assets` | ***포함*** | Microsoft Store 이미지를 포함합니다.  
`Data`   | ***무시*** | Unity에서 (장면, 셰이더, 스크립트, Prefabs 등) 프로젝트를 컴파일하는  
`Dependencies` | ***포함*** | 이 폴더 (예를 들어 XboxLiveSDK.dll)에서 모든 UWP 종속성 유지를 위해 만든 것  
`Properties` | ***포함*** | 개발자가 수정할 수 있는 고급 설정이 포함 되어 있습니다.  
`Unprocessed` | ***무시*** | Unity를 포함 `.dll` 고 `.pdb` 파일  

## <a name="files"></a>파일  

`App.cs` | ***포함*** | UWP 응용 프로그램에 대 한 진입점 이 수정 되어 다른 소스 파일을 사용 하 여 확장  
`Package.appxmanifest` | ***포함*** | 프로그램의 AppX에 대 한 앱 패키지 매니페스트 소스 파일  
`project.json` | ***포함*** | NuGet 패키지를 설명 하 `*.csproj` 에 따라 달라 집니다  
`ScrapyardPhoenix.csproj` | ***포함*** | UWP 빌드 대상을;에 대해 설명합니다. 프로그램 uwp 추가 종속성을 추가 하는 경우 프로젝트를이 `*.csproj` 파일에는 해당 정보가 포함 됩니다  
`ScrapyardPhoenix.csproj.user` | ***무시*** | 로컬 사용자 정보를 포함 하는이 파일

## <a name="resulting-gitignore"></a>결과 .gitignore

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

작업을 수행하면 팀 동료는 사용자가 생성한 UWP 프로젝트와 동기화됩니다. 이제 UWP 프로젝트에 다른 자산, 소스 및 종속성을 언제든지 추가할 수 있습니다.

UWP 폴더 버전 관리의 일부 추가 예는 [이러한 예제](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview)에서 찾아볼 수 있습니다.

## <a name="adding-dependencies-to-your-uwp-app"></a>UWP 앱에 종속성 추가

**Plugins** 폴더 아래 **Unity Assets** 폴더에 종속성을 넣어 DLL 및 WINMD에 추가한 다음 종속성을 선택하여 검사기에서 해당 대상 플랫폼 설정을 적절하게 설정합니다.

![UWP 솔루션](images/uwp-solution.PNG)

***ScrapyardPhoenix(유니버설 Windows)*** 는 참조를 추가할 프로젝트(예: Xbox Live SDK)입니다.

## <a name="see-also"></a>참고 항목
- [기존 게임을 Xbox에 가져오기](development-lanes-landing.md)
- [Xbox One에서 UWP](index.md)
