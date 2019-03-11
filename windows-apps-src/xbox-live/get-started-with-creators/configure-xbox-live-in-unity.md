---
title: 구성 Xbox Live Unity에서
description: Xbox Live Unity 플러그 인을 사용 하 여 Unity 게임에 Xbox Live를 구성 하는 방법에 알아봅니다.
ms.assetid: 55147c41-cc49-47f3-829b-fa7e1a46b2dd
ms.date: 01/25/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, Unity, xbox 구성
localizationpriority: medium
ms.openlocfilehash: d464fc54d322db9da91870bd3ca7cbc29957b379
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596728"
---
# <a name="configure-xbox-live-in-unity"></a>구성 Xbox Live Unity에서

> [!NOTE]
> Xbox Live Unity 플러그 인에만 권장 됩니다 [Xbox Live 크리에이터 스 프로그램](../developer-program-overview.md) 멤버를 현재 이후 성과 멀티 플레이 게임에 대 한 지원은 없습니다.

사용 하 여 합니다 [Xbox Live Unity 플러그인](https://github.com/Microsoft/xbox-live-unity-plugin)Unity 게임에 Xbox Live 지원 쉽습니다 추가, Xbox Live를 사용 하 여 가장을 제목에 맞게는 방식에서에 집중 하는 데 시간이 더 제공 합니다.

이 항목에서는 Unity에서 Xbox Live 플러그 인을 설정 하는 프로세스를 거치게 됩니다.

## <a name="prerequisites"></a>필수 구성 요소

Unity에서 Xbox Live를 사용 하기 전에 다음이 필요 합니다.

1.  **[Xbox Live 계정을](https://support.xbox.com/browse/my-account/manage-account/Create%20account)** 합니다.
1. 등록 합니다  **[파트너 센터 개발자 프로그램](https://developer.microsoft.com/store/register)** 합니다.
2. **[Windows 10 1 주년 업데이트](https://microsoft.com/windows)**  이상
3. **[Unity](https://store.unity.com/)**  버전 **5.5.4p5** (이상)에서 **2017.1p5** (이상)에서 또는 **2017.2.0f3** (이상) 사용 하 여 **[Microsoft Visual Studio Tools for Unity](https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity)** 하 고 **Windows Store.NET 백 엔드 스크립팅**합니다.
4. **[Visual Studio 2015](https://www.visualstudio.com/)**  하거나 **[Visual Studio 2017 15.3.3](https://www.visualstudio.com/)** (이상) 사용 하 여 합니다 **유니버설 Windows 앱 개발 도구**합니다.
5. **[Xbox Live 플랫폼 확장 SDK](https://aka.ms/xblextsdk)** 합니다.


> [!NOTE]
> Unity 2017.2.0p2 해야 Xbox Live를 사용한 스크립팅 IL2CPP 백 엔드를 사용 하려는 경우 이상 하 고 "1802 미리 보기 릴리스"는 Xbox Live Unity 플러그 인 버전 이상.


## <a name="import-the-unity-plugin"></a>Unity 플러그 인 가져오기

기존 또는 새 Unity 프로젝트에 플러그 인을 가져오려면 다음이 단계를 수행 합니다.

1. Xbox Live Unity 플러그 인 릴리스 탭으로 이동 [ https://github.com/Microsoft/xbox-live-unity-plugin/releases ](https://github.com/Microsoft/xbox-live-unity-plugin/releases)합니다.
2. 다운로드 **XboxLive.unitypackage**합니다.
3. Unity에서 클릭 **자산** > **패키지 가져오기** > **Custom Package** 이동한 **XboxLive.unitypackage**.

![성공적으로 가져오기](../images/unity/get-started-with-creators/importXBL_Small.gif)

### <a name="optional-configure-the-plugin-to-work-in-the-unity-editor-net-46-or-il2cpp-only"></a>(선택 사항) Unity 편집기의 (.NET 4.6 또는 IL2CPP만)을 작동 하려면 플러그 인 구성

> [!NOTE]
> Xbox Live Unity 플러그 인 버전 "1711 릴리스" 필요 Unity의 스크립팅 런타임 버전 변경에 대 한 지원이.NET 4.6 및 버전 "1802 미리 보기 릴리스"에 대 한 이상 IL2CPP에 대 한 이상.

코드 컴파일 방식을 정의 하는 Unity에서 구성할 수 있는 세 가지 설정이 있습니다.

1. **백 엔드를 스크립팅** 사용 되는 컴파일러입니다. Unity는 두 개의 다른 스크립팅 백 엔드 유니버설 Windows 플랫폼에 대 한 지원:.NET 및 IL2CPP 합니다.
2. 합니다 **스크립팅 런타임 버전** Unity 편집기를 실행 하는 스크립팅 런타임 버전입니다.
3. 합니다 **API 호환성 수준** 는 API 화면에 대 한 게임을 빌드합니다.

다음 표에서 Xbox Live Unity 플러그 인에 대 한 현재 스크립팅 지원 매트릭스를 보여 줍니다.

| 백 엔드를 스크립팅합니다.     | 스크립팅 런타임 버전 | 지원함     | 필요한 최소 Unity 버전 |
|-------------------    |-------------------        |-----------    |------------------------------- |
| IL2CPP                | .NET 3.5와 동일       | 아니오            | 해당 없음                            |
| Il2CPP                | .NET 4.6 해당       | 예           | 2017.2.0p2                     |
| .NET                  | .NET 3.5와 동일       | 예           | 필수 구성 요소와 동일          |
| .NET                  | .NET 4.6 해당       | 예           | 필수 구성 요소와 동일          |

Xbox Live Unity 플러그 인을 버전 "1711 릴리스"부터 추가 스크립팅 런타임 지원이 추가 되었습니다. 플러그 인은 기본적으로.NET 백 엔드 스크립팅 및.NET 3.5의 런타임 버전 스크립팅을 사용 하 여 Unity 편집기에서 실행 되도록 구성 됩니다. 프로젝트가.NET 4.6의 스크립팅 런타임 버전을 사용 하는 경우 편집기에서 제대로 작동 하려면 플러그 인을 구성 해야 합니다.

1. Unity 프로젝트 탐색기에서로 이동 **Xbox Live\Libs\UnityEditor\NET46** 폴더에서 모든 Dll을 선택 합니다.
2. 검사기 창에서 확인 **편집기** 아래에서 **플랫폼 포함**합니다.
3. Unity 프로젝트 탐색기에서로 이동 **Xbox Live\Libs\UnityEditor\NET35** 폴더에서 모든 Dll을 선택 합니다.
4. 검사기 창에서 선택 취소 **편집기** 아래에서 **플랫폼 포함**합니다.

![스크립팅 런타임을 변경합니다](../images/unity/get-started-with-creators/changeScriptingRuntime.gif)

> [!IMPORTANT]
> 다음이 단계를 스크립팅 런타임 버전을 프로젝트에 3.5로 다시 변경 하면 되돌릴 수 해야 합니다.

## <a name="set-visual-studio-as-the-ide-in-unity"></a>Visual Studio를 Unity에서 IDE로

빌드하는 데 필요한 visual Studio는 [유니버설 Windows 플랫폼 (UWP)](https://docs.microsoft.com/windows/uwp/get-started/whats-a-uwp) 게임입니다. 로 이동 하 여 Visual Studio로 Unity에서 IDE를 설정할 수 있습니다 **편집할** > **기본 설정** > **외부 도구** 설정과 합니다 **외부 스크립트 편집기** Visual Studio로 합니다.

![VS 외부 도구를 설정 합니다.](../images/unity/get-started-with-creators/setVSExternalTool_Small.gif)

## <a name="unity-plugin-file-structure"></a>Unity 플러그 인 파일 구조

Unity 플러그 인의 파일 구조는 다음 부분으로 나뉩니다.

* __Xbox Live__ 포함 된 실제 플러그 인 자산이 포함 된에서 게시 된 **.unitypackage**합니다.
    * __편집기__ 기본 Unity 구성 UI를 제공 하는 스크립트를 포함 하 고 빌드하는 동안 프로젝트를 처리 합니다.
    * __예제__ 다양 한 prefabs를 사용 하 여 서로 연결 하는 방법을 보여 주는 간단한 장면 파일 집합이 포함 되어 있습니다.
    * __이미지__ 를 prefabs에서 사용 되는 이미지의 작은 집합이 있습니다.
    * __Libs__ 는 Xbox Live 라이브러리 저장 됩니다.
    * __Prefabs__ 다양 한 포함 [Unity prefab](https://docs.unity3d.com/Manual/Prefabs.html) Xbox Live 기능을 구현 하는 개체입니다.
    * __스크립트__ 를 prefabs에서 Xbox Live Api를 호출 하는 모든 코드 파일을 포함 합니다. 이 올바로 Xbox Live Api를 호출 하는 방법에 대 한 예제를 확인 하기에 적합 합니다.
    * __Tools\AssociationWizard__ Xbox Live 연결 마법사에서 응용 프로그램 구성을 풀다운합니다 하는 데 포함 [파트너 센터](https://developer.microsoft.com/windows) Unity 내에서 사용 합니다.

## <a name="enable-xbox-live"></a>Xbox Live를 사용 하도록 설정

Xbox Live와 상호 작용 하 여 제목에 대 한 초기 Xbox Live 구성을 설정 해야 합니다. 수행할 수 있습니다이 쉽고 Unity 내에서 Xbox Live 연결 마법사를 사용 하 여.

1. 에 **Xbox Live** 메뉴에서 **구성**합니다.
2. 에 **Xbox Live** 창에서 **실행 Xbox Live 연결 마법사**합니다.
3. 에 **제목을 Windows 스토어에 연결** 대화 상자에서 클릭 **다음**, 파트너 센터 계정에 로그인 합니다.
4. 이 프로젝트와 연결 하 고 클릭 하려는 앱을 선택 **선택**합니다. 있습니다 표시 되지 않으면를 클릭 해 보십시오 **새로 고침**합니다. 또는 새 앱 이름을 예약 하 고 클릭 하 여 만들 수 있습니다 **예약**합니다.
5. 아직 없는 경우 Xbox Live를 사용 하도록 설정 하 라는 메시지가 표시 됩니다. 클릭 **사용** 제목에 Xbox Live를 사용 하도록 설정 합니다.
6. 클릭 **완료** 여 구성을 저장 합니다.

Xbox Live 서비스를 호출 하려면 데스크톱 개발자 모드 여야 하 고 동일한 샌드박스에서 제목은 Xbox Live 구성에서으로 설정 해야 합니다. 모두 확인 하 여 확인할 수 있습니다 합니다 **Xbox Live 구성** Unity에서 창:

1. **개발자 모드 구성을** 나타나야 **Enabled**합니다. 이 표시 되 면 **사용 안 함**, 클릭 **개발자 모드를 전환할**합니다.
2. **구성 제목** > **샌드박스** ID와 동일 해야 **개발자 모드 구성을** > **개발자 모드**합니다.

![XBL 사용 하도록 설정](../images/unity/unity-xbl-enabled.png)

참조 [Xbox Live 샌드박스](../xbox-live-sandboxes.md) 샌드박스에 대 한 정보에 대 한 합니다.

## <a name="build-and-test-the-project"></a>빌드 및 테스트 프로젝트

제목, 편집기에서 실행할 때는 Xbox Live 기능을 사용 하려고 할 때 가짜 데이터가 나타납니다. 예를 들어 경우 있습니다 [기능에서 추가](unity-prefabs-and-sign-in.md) 장면 및 로그인 시도, 나타납니다 **모조 사용자** 프로필 이름의 자리 표시자 아이콘으로 표시 합니다. 실제 프로필을 사용 하 여 로그인을 제목에서 Xbox Live 기능 테스트, UWP 솔루션을 구축 하 여 Visual Studio에서 실행 해야 합니다.  다음이 단계를 수행 하 여 Unity의 UWP 프로젝트를 빌드할 수 있습니다.

1. 엽니다는 **빌드 설정** 를 선택 하 여 창 **파일** > **빌드 설정**합니다.
2. 장면에서 빌드에 포함 하려는 모두 추가 합니다 **Scenes In Build** 섹션입니다.
3. 전환할 합니다 **유니버설 Windows 플랫폼** 를 선택 하 여 **유니버설 Windows 플랫폼** 아래 **플랫폼** 를 클릭 하 고 **플랫폼 전환**.
4. 설정할 **SDK** 하 **10.0.15063.0** 이상.
5. 스크립트 디버깅 검사를 사용 하도록 설정 하려면 **Unity C# 프로젝트**합니다.
6. 클릭 **빌드** 프로젝트의 위치를 지정 합니다.

![빌드 설정](../images/unity/build_settings.JPG)

빌드가 완료 되 면 Unity Visual Studio에서를 실행 해야 하는 새 UWP 솔루션 파일을 생성 합니다.

1. 지정 된 폴더를 엽니다  **&lt;ProjectName&gt;.sln** Visual Studio에서.
2. 맨 위에 있는 도구 모음에서 선택 **x64** 에 배포 하는 **로컬 컴퓨터**합니다.

사용 하도록 설정한 경우 **스크립트 디버깅** Unity에서 UWP 솔루션을 빌드할 때 다음 스크립트는 아래에 **어셈블리-CSharp (유니버설 Windows)** 프로젝트입니다.

![가짜 사용자: 123456789](../images/unity/get-started-with-creators/visualStudio.PNG)

> [!NOTE]
> 실제 데이터를 사용 하 여 게임을 테스트 하려면 Visual Studio 빌드를 사용 하기 전에 수행 [이 검사 목록을](test-visual-studio-build.md) 을 제목 Xbox Live 서비스에 액세스할 수 있도록 합니다.

> [!IMPORTANT]
> 부터는 이제 2018 필요할 Visual Studio에서 UWP 제목을 올바르게 테스트 하려면 package.appxmanifest.xml 파일에 대 한 업데이트를 확인 합니다. 이렇게 하려면 다음을 수행합니다.
>
> 1. Package.appxmanifest.xml 파일에 대 한 솔루션 탐색기 검색
> 2. 파일을 마우스 오른쪽 단추로 클릭 하 고 코드 보기를 선택 합니다.  
    코드 보기 옵션을 사용할 수 없는 경우 package.appxmanifest 파일을 확장명이 없는 합니다. Xml 파일을 열고 나머지 단계를 진행 해야 합니다.
> 3. 아래는 `<Properties></Properties>` 섹션에서 다음 줄 추가: `<uap:SupportedUsers>multiple</uap:SupportedUsers>`합니다.
> 4. Visual Studio에서 원격 디버깅 빌드를 시작 하 여 Xbox로 게임을 배포 합니다. Xbox에 제목을 설정 하는 지침을 찾을 수 있습니다 합니다 [Xbox 개발 환경에서 프로그램 UWP 설정](../../xbox-apps/development-environment-setup.md) 문서.
>
> 구성이 변경 되었습니다. 부분에는 여전히 단일 플레이어 시나리오에서 게임을 실행 해야 하는 멀티 플레이어를 사용 하는 것 처럼 보일 수 있습니다.

## <a name="try-out-the-examples"></a>예를 사용

모든 준비가 끝났습니다 Xbox Live를 사용 하 여 Unity 프로젝트에서 시작! 장면에 열어 보세요 합니다 **Xbox Live/Examples** 플러그 인 작업에 직접 기능을 사용 하는 방법에 대 한 예제를 보려는 폴더입니다. 편집기의 예제를 실행 하면 모조 데이터를 Visual Studio에서 프로젝트를 빌드할 경우 및 [샌드박스를 사용 하 여 Xbox Live 계정을 연결](authorize-xbox-live-accounts.md), 게이머 태그에 로그인 할 수 있습니다.

시도 합니다 **SignInAndProfile** 장면에 대 한 Microsoft 계정에 로그인 합니다 **순위표** 순위표를 만들기 위한 장면 및 **UpdateStat** 장면에 대 한 표시 하 고 통계를 업데이트 합니다.

## <a name="see-also"></a>참고 항목

* [Unity에 살고 있는 Xbox에 로그인](unity-prefabs-and-sign-in.md)
* [Xbox Live 계정에 권한 부여](authorize-xbox-live-accounts.md)
