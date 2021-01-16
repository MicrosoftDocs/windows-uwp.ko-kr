---
title: Unity-버전 제어 UWP 프로젝트
description: 유니버설 Windows 플랫폼 (UWP)를 사용 하 여 Xbox 용 Unity 게임에서 버전 제어를 사용 하는 방법에 대해 알아봅니다.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 67c4ec927d83ecba1257eb73fec451e3281333b2
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254168"
---
# <a name="unity-version-control-your-uwp-project"></a>Unity: 버전 제어 UWP 프로젝트

유니버설 Windows 플랫폼 (UWP)를 사용 하 여 Xbox 용 Unity 게임을 아직 빌드하지 않았습니까?  먼저 [Xbox에서 UWP에 Unity 게임 가져오기를](development-lanes-unity.md)참조 하세요.

생성 된 UWP 디렉터리의 일부를 버전 제어에 추가 하는 이유는 몇 가지 이며, 그 중 하나는 종속성 (예: Xbox Live SDK)을 추가 하는 것입니다.  이 시나리오는이 자습서의 예제로 사용 되며, 프로젝트의 개별 요구를 해결 하는 데 도움이 될 것입니다.

***부인: 버전 제어 솔루션으로 Git를 사용할 예정입니다.  서로 다른 경우 개념은 여전히 변환 되어야 합니다.** _

메모리를 새로 고치려면 게임의 디렉터리 ( _*_ScrapyardPhoenix_*_)가 현재 처럼 보입니다.

![빌드 대상 폴더](images/build-destination.png)

UWP 디렉터리는 다음과 같습니다.

![UWP VS 솔루션](images/uwp-vs-solution.png)

이 디렉터리에는 _*_ScrapyardPhoenix_*_ (여기에 게임 이름 삽입) 폴더가 있는 폴더에만 관심이 있습니다.  다른 모든 항목은 버전 제어에서 무시 될 수 있습니다.

_*_.Gitignore 파일에 익숙하지 않나요?  [.Gitignore](https://git-scm.com/docs/gitignore)를 참조 하세요._*_

```console
##################################################################
# The original .gitignore file can be found at
# https://github.com/github/gitignore/blob/master/Unity.gitignore
##################################################################

# standard ignores for a Unity Project
...

# ignore the whole UWP directory
/UWP/_*

# except we want to keep... (this line will be modified and removed further down)
!/UWP/ScrapyardPhoenix/
```

**UWP/ScrapyardPhoenix** 폴더 내에서 몇 가지 다른 파일과 폴더를 선택 하 여 버전 제어에 추가 하려고 합니다.  먼저 전체 정보를 살펴보겠습니다.

![UWP 빌드 디렉터리](images/uwp-build-directory.png)  

## <a name="folders"></a>폴더  

| 폴더 이름 | 설정 | Description |
|-------------|---------|-------------|
| `Assets` | **_포함_* _ | Microsoft Store 이미지 포함 |
| `Data` | _*_무시_*_ | Unity에서 프로젝트를 컴파일하는 경우 (장면, 셰이더, 스크립트, Prefabs 등) |
| `Dependencies` | _*_되어야_*_ | 이 폴더는 모든 UWP 종속성 (예: XboxLiveSDK.dll)을 유지 하기 위해 만든 것입니다. |
| `Properties` | _*_되어야_*_ | 개발자가 수정할 수 있는 고급 설정을 포함 합니다. |
| `Unprocessed` | _*_무시_*_ | Unity `.dll` 및 `.pdb` 파일 포함 |

## <a name="files"></a>파일  

| 폴더 이름 | 설정 | Description |
|-------------|---------|-------------|
| `App.cs` | _*_포함_*_ | UWP 응용 프로그램에 대 한 진입점입니다. 다른 소스 파일을 사용 하 여 수정 하 고 확장할 수 있습니다. |
| `Package.appxmanifest` | _*_되어야_*_ | 응용 프로그램 패키지 매니페스트 소스 파일 (msix 또는 .appx 패키지) |
| `project.json` | _*_되어야_*_ | 에서 종속 된 NuGet 패키지에 대해 설명 합니다. `_.csproj` |
| `ScrapyardPhoenix.csproj` | ***포함** _ | UWP 빌드 대상을 설명 합니다. UWP 프로젝트에 추가 종속성을 추가 하는 경우이 `_.csproj` 파일에는 해당 정보가 포함 됩니다. |
| `ScrapyardPhoenix.csproj.user` | ***무시** _ | 이 파일에는 로컬 사용자 정보가 포함 되어 있습니다. |

## <a name="resulting-gitignore"></a>결과. .gitignore

```console
##################################################################
# The original .gitignore file can be found at
# https://github.com/github/gitignore/blob/master/Unity.gitignore
##################################################################

# standard ignores for a Unity Project
...

# ignore the whole UWP directory
/UWP/_*

# except we want to keep...
!/UWP/ScrapyardPhoenix/Assets/*
!/UWP/ScrapyardPhoenix/Dependencies/*
!/UWP/ScrapyardPhoenix/Properties/*
!/UWP/ScrapyardPhoenix/App.cs
!/UWP/ScrapyardPhoenix/Package.appxmanifest
!/UWP/ScrapyardPhoenix/project.json
!/UWP/ScrapyardPhoenix/ScrapyardPhoenix.csproj
```

이제 팀 동료가 생성 한 UWP 프로젝트와 동기화 됩니다. 이제는 더 자유롭게 UWP 프로젝트에 자산, 소스 및 종속성을 더 추가할 수 있습니다.

UWP 폴더의 버전 관리에 대 한 몇 가지 추가 예는 [다음 예제](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview)에서 찾을 수 있습니다.

## <a name="adding-dependencies-to-your-uwp-app"></a>UWP 앱에 종속성 추가

**플러그 인** 폴더 아래에 있는 **Unity 자산** 폴더에 배치 하 여 dll과 winmd에 종속성을 추가한 다음 선택 하 고 검사기에서 해당 대상 플랫폼 설정을 적절 하 게 설정 합니다.

![UWP 솔루션](images/uwp-solution.PNG)

**_ScrapyardPhoenix (유니버설 Windows)_** 는 XBOX Live SDK와 같은 참조를 추가 하는 프로젝트입니다.

## <a name="see-also"></a>참고 항목

- [기존 게임을 Xbox로 가져오기](development-lanes-landing.md)
- [Xbox One의 UWP](index.md)
