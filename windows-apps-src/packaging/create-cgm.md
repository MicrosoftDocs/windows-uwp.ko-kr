---
author: laurenhughes
ms.assetid: ff2523cb-8109-42be-9dfc-cb5d09002574
title: 소스 콘텐츠 그룹 맵 생성 및 변환
description: 유니버설 Windows 플랫폼(UWP) 앱을 UWP 앱 스트리밍 설치에 사용하도록 준비하려면 콘텐츠 그룹 맵을 생성해야 합니다. 이 문서는 콘텐츠 그룹 맵을 생성 및 변환하는 구체적인 방법을 알려주는 한편, 몇 가지 팁과 요령을 제공합니다.
ms.author: lahugh
ms.date: 9/30/2018
ms.topic: article
keywords: Windows 10, uwp, 콘텐츠 그룹 맵, 스트리밍 설치, uwp 앱 스트리밍 설치, 소스 콘텐츠 그룹 맵
ms.localizationpriority: medium
ms.openlocfilehash: 6a2922d6d3f54d693a9fe9c0982ea06cc5f2caae
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6037972"
---
# <a name="create-and-convert-a-source-content-group-map"></a>소스 콘텐츠 그룹 맵 생성 및 변환

유니버설 Windows 플랫폼(UWP) 앱을 UWP 앱 스트리밍 설치에 사용하도록 준비하려면 콘텐츠 그룹 맵을 생성해야 합니다. 이 문서는 콘텐츠 그룹 맵을 생성 및 변환하는 구체적인 방법을 알려주는 한편, 몇 가지 팁과 요령을 제공합니다.

## <a name="creating-the-source-content-group-map"></a>소스 콘텐츠 그룹 맵 만들기

`SourceAppxContentGroupMap.xml` 파일을 만든 다음, Visual Studio 또는 **MakeAppx.exe** 도구를 사용해 이 파일을 최종 버전인 `AppxContentGroupMap.xml`로 변환해야 합니다. 처음부터 `AppxContentGroupMap.xml`를 생성하여 단계를 건너뛸 수 있지만, `SourceAppxContentGroupMap.xml`을 생성하고 이를 변환하는 것이 더 손쉽고 권장할 만한 방법입니다. 왜냐하면 `AppxContentGroupMap.xml`에서는 와일드카드가 허용되지 않기 때문입니다(실제로는 도움이 되지만). 

UWP 앱 스트리밍 설치가 유용한 시나리오를 간단히 살펴보겠습니다. 

UWP 게임을 만들었는데, 최종 앱의 크기가 100GB를 넘었다고 해봅시다. 하는 편리 하 게 될 수 있는 Microsoft Store에서 다운로드 하는 데 시간이 오래 걸릴 것입니다. UWP 앱 스트리밍 설치를 사용하기로 한 경우에는 앱의 파일이 다운로드되는 순서를 지정할 수 있습니다. 사용자는 먼저 기본적인 파일을 다운로드하도록 스토어에 명령하여 백그라운드에서 필수가 아닌 파일이 다운로드되는 동안 더 빨리 앱과 상호 작용을 할 수 있습니다.

> [!NOTE]
> UWP 앱 스트리밍 설치의 사용은 앱의 파일 구성에 따라 크게 좌우됩니다. UWP 앱 스트리밍 설치와 관련된 앱의 콘텐츠 레이아웃을 가능한 먼저 고려하여 앱의 파일을 보다 간단하게 만드는 것이 좋습니다.

먼저, `SourceAppxContentGroupMap.xml`파일을 만들 것입니다.

세부 내용에 들어가기 앞서, 완전한 `SourceAppxContentGroupMap.xml` 파일을 간단히 예로 들어보겠습니다.

```xml
<?xml version="1.0" encoding="utf-8"?>  
<ContentGroupMap xmlns="http://schemas.microsoft.com/appx/2016/sourcecontentgroupmap" 
                 xmlns:s="http://schemas.microsoft.com/appx/2016/sourcecontentgroupmap"> 
    <Required>
        <ContentGroup Name="Required">
            <File Name="StreamingTestApp.exe"/>
        </ContentGroup>
    </Required>
    <Automatic>
        <ContentGroup Name="Level2">
            <File Name="Assets\Level2\*"/>
        </ContentGroup>
        <ContentGroup Name="Level3">
            <File Name="Assets\Level3\*"/>
        </ContentGroup>
    </Automatic>
</ContentGroupMap>
```

콘텐츠 그룹 맵에는 두 가지 주요 구성 요소가 있는데, 필수 콘텐츠 그룹을 포함하고 있는 **필수** 섹션과 여러 개의 **자동** 콘텐츠 그룹을 포함할 수 있는 자동 섹션이 바로 그것입니다.

### <a name="required-content-group"></a>필수 콘텐츠 그룹

필수 콘텐츠 그룹은 `SourceAppxContentGroupMap.xml`의 `<Required>`요소 내에 있는 단일 콘텐츠 그룹입니다. 필수 콘텐츠 그룹에는 최소한의 사용자 경험으로 앱을 실행하는 데 필요한 기본 파일들이 모두 포함되어 있어야 합니다. .NET 네이티브 컴파일로 인해 모든 코드 (응용 프로그램 실행 파일)를 필수 그룹에 반드시 포함시키고 자산과 기타 파일은 자동 그룹에 남겨 둬야 합니다.

예를 들어, 앱이 게임인 경우에는 필수 그룹에 주 메뉴나 게임 홈 화면에 사용되는 파일들이 포함될 수 있습니다.

원래 `SourceAppxContentGroupMap.xml` 파일에서 나온 조각은 다음과 같습니다. 
```xml
<Required>
    <ContentGroup Name="Required">
        <File Name="StreamingTestApp.exe"/>
    </ContentGroup>
</Required>
```

여기에서 주목해야 할 몇 가지 중요한 사항이 있습니다.

-  `<Required>` 요소 내의 `<ContentGroup>`는  **반드시** "필수(Required)"라고 명명되어 있어야 합니다. 이 이름은 필수 콘텐츠 그룹 전용으로 예약이 되었으며, 최종 콘텐츠 그룹 맵의 다른 `<ContentGroup>`에서 사용할 수 없습니다.
- `<ContentGroup>`는 오직 하나 뿐입니다. 이는 의도적인 것으로, 기본 파일 그룹이 오직 하나여야 하기 때문입니다.
- 이 예에서 파일은 `.exe`파일 하나입니다. 필수 콘텐츠 그룹은 하나의 파일로 제한되지 않으며 여러 개일 수 있습니다. 

좋아하는 텍스트 편집기에서 새 페이지를 열고 파일을 앱의 프로젝트 폴더에 "다른 이름으로 저장"한 다음, 새로 생성된 파일을 `SourceAppxContentGroupMap.xml`로 명명하는 방법으로 손쉽게 파일 작성을 시작할 수 있습니다.

> [!IMPORTANT]
> C++ UWP 앱을 개발하는 경우에는 `SourceAppxContentGroupMap.xml`의 파일 속성을 조정해야 합니다. `Content` 속성을  **참**으로, `File Type` 속성을 **XML 파일**로 설정합니다. 

`SourceAppxContentGroupMap.xml`를 만들 때는 파일 이름에 와일드카드를 활용하면 도움이 됩니다. 자세한 내용은 [와일드카드 사용 팁 및 요령](#wildcards) 섹션을 참조하십시오.

Visual Studio를 사용하여 앱을 개발한 경우에는 필수 콘텐츠 그룹에 이를 포함시키는 것이 좋습니다.

```xml
<File Name="*"/>
<File Name="WinMetadata\*"/>
<File Name="Properties\*"/>
<File Name="Assets\*Logo*"/>
<File Name="Assets\*SplashScreen*"/>
```

단일 와일드 카드 파일 이름을 추가하면 앱 실행 파일이나 DLL 같이 Visual Studio에서 나온 프로젝트 디렉터리에 파일이 추가됩니다. WinMetadata 및 속성 폴더에는 Visual Studio가 생성한 다른 폴더들이 포함됩니다. 자산 와일드 카드는 앱 설치에 필요한 로고 및 SplashScreen 이미지를 선택하기 위한 것입니다.

프로젝트에 모든 파일을 포함시키기 위해 파일 구조의 루트에서 더블 와일드카드 "**"를 사용할 수 없습니다. 왜냐하면 `SourceAppxContentGroupMap.xml`을 최종 `AppxContentGroupMap.xml`으로 변환하려고 하는 노력이 실패할 것이기 때문입니다.

또한, 풋프린트 파일(AppxManifest.xml, AppxSignature.p7x, resources.pri, etc.)을 콘텐츠 그룹 맵에 포함시켜서는 안 됩니다. 지정된 와일드카드 파일 이름 중 하나에 포함이 된 풋프린트 파일은 무시가 됩니다.

### <a name="automatic-content-groups"></a>자동 콘텐츠 그룹

자동 콘텐츠 그룹은 사용자가 이미 다운로드한 콘텐츠 그룹과 상호 작용하는 동안 백그라운드에서 다운로드되는 자산입니다. 여기에는 앱 실행에 필수적이지 않은 모든 추가 파일이 포함됩니다. 예를 들어, 자동 콘텐츠 그룹을 다른 수준으로 분할하여 각 수준을 별도의 콘텐츠 그룹으로 정의할 수 있습니다. 필수 콘텐츠 그룹 섹션에서 설명했듯이, .NET 네이티브 컴파일로 인해 모든 코드 (응용 프로그램 실행 파일)를 필수 그룹에 반드시 포함시키고 자산과 기타 파일은 자동 그룹에 남겨 둬야 합니다.

`SourceAppxContentGroupMap.xml` 예에서 자동 콘텐츠 그룹에 대해 자세히 살펴보겠습니다.
```xml
<Automatic>
    <ContentGroup Name="Level2">
        <File Name="Assets\Level2\*"/>
    </ContentGroup>
    <ContentGroup Name="Level3">
        <File Name="Assets\Level3\*"/>
    </ContentGroup>
</Automatic>
```

자동 그룹의 레이아웃은 몇 가지 예외를 제외하고 필수 그룹과 매우 유사합니다.

- 콘텐츠 그룹은 여러 개가 있습니다.
- 자동 콘텐츠 그룹은 필수 콘텐츠 그룹에 예약된 "필수"라는 이름을 제외하고 고유의 이름을 가질 수 있습니다.
- 자동 콘텐츠 그룹에는 필수 콘텐츠 그룹에서 나온 **어떤** 파일도 포함될 수 없습니다. 
- 자동 콘텐츠 그룹에는 다른 자동 콘텐츠 그룹에도 존재하는 파일이 포함될 수 있습니다. 파일은 단 한 번만 다운로드 되며, 파일을 포함한 첫 번째 자동 콘텐츠 그룹과 함께 다운로드가 됩니다.

#### 와일드카드 <a name="wildcards"></a> 사용 팁 및 요령

콘텐츠 그룹 맵에 대한 파일 레이아웃은 항상 프로젝트 루트 폴더를 기준으로 합니다.

이 예에서 와일드카드는 두 `<ContentGroup>`요소 모두에서 사용이 되며 "Assets\Level2" 또는 "Assets\Level3"의 단일 파일 수준 내에서 모든 파일을 검색합니다. 하위 폴더 구조를 사용하는 경우 더블 와일드카드를 사용할 수 있습니다.

```xml
<ContentGroup Name="Level2">
    <File Name="Assets\Level2\**"/>
</ContentGroup>
```

파일 이름에 대한 텍스트에서 와일드 카드를 사용할 수도 있습니다. 예를 들어, "Level2"가 포함된 파일 이름을 가진 "자산" 폴더의 모든 파일을 포함시키고 싶다면 다음과 같은 기능을 사용할 수 있습니다.

```xml
<ContentGroup Name="Level2">
    <File Name="Assets\*Level2*"/>
</ContentGroup>
```

## <a name="convert-sourceappxcontentgroupmapxml-to-appxcontentgroupmapxml"></a>SourceAppxContentGroupMap.xml을 AppxContentGroupMap.xml로 변환

`SourceAppxContentGroupMap.xml`을 최종 버전인 `AppxContentGroupMap.xml`으로 변환하려면 Visual Studio 2017 또는 **MakeAppx.exe** 명령줄 도구를 사용할 수 있습니다.

Visual Studio를 사용하여 콘텐츠 그룹 맵을 변환하는 방법:
1. `SourceAppxContentGroupMap.xml`을 프로젝트 폴더에 추가
2. 속성 창에서 빌드 작업을 `SourceAppxContentGroupMap.xml`에서 "AppxSourceContentGroupMap"으로 변경
2. 솔루션 탐색기에서 프로젝트를 마우스 오른쪽 단추로 클릭합니다.
3. 스토어 -> 콘텐츠 그룹 맵 파일 변환으로 이동

Visual Studio에서 앱을 개발하지 않았거나 명령줄 사용을 선호하는 경우에는 **MakeAppx.exe** 도구를 사용해 `SourceAppxContentGroupMap.xml`을 변환합니다. 

간단한 **MakeAppx.exe** 명령은 다음과 같이 표시될 수 있습니다.
```syntax
MakeAppx convertCGM /s MyApp\SourceAppxContentGroupMap.xml /f MyApp\AppxContentGroupMap.xml /d MyApp\
```

/s 옵션은 `SourceAppxContentGroupMap.xml`에 대한 경로를, /f는 `AppxContentGroupMap.xml`에 대한 경로를 지정합니다. 마지막 옵션인 /d는 파일 이름 와일드카드를 확장하는 데 사용할 디렉터리(이 예에서는 앱 프로젝트 디렉터리)를 지정합니다.

**MakeAppx.exe**에서 사용할 수 있는 옵션에 대한 자세한 내용을 보려면 명령 프롬프트를 열고 **MakeAppx.exe**로 이동해서 다음을 입력합니다.

```syntax
MakeAppx convertCGM /?
```

이렇게만 하면 최종 `AppxContentGroupMap.xml`를 앱에서 사용할 준비가 완료됩니다! 앱은 Microsoft Store에 대 한 완벽 한 준비 하기 위해 더 많은 있습니다. UWP 앱 스트리밍 설치를 앱에 추가하는 전체 프로세스에 대한 자세한 내용은 [이 블로그 게시물](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/)을 확인하세요.
