---
title: XBL Unity Prefabs 및 로그인
description: 소셜 prefabs 및 Xbox Live에서 소셜 서비스에 대 한 스크립트 예제
ms.date: 01/24/2018
ms.topic: get-started-article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, unity
ms.openlocfilehash: a893858dac11fa848c2601df2c1bd6292b72ac6d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660038"
---
# <a name="unity-prefabs-and-scripted-sign-in"></a>Unity Prefabs와 스크립팅된 로그인

이 문서는 안내 Xbox Live에 로그인 하 여 Unity 프로젝트에 추가 합니다. 두 가지 방법으로 다운로드 한 경우 로그인 얻을 수 있습니다 합니다 [Xbox Live Unity 플러그인](https://github.com/Microsoft/xbox-live-unity-plugin)합니다. 플러그 인 내에 포함 된 prefabs를 사용 하거나 사용자 고유의 사용자 지정 Gameobject를 Xbox Live 로그인 스크립트를 스크립트 및 포함 된 라이브러리를 사용 하 여 될 수 있습니다.

> [!IMPORTANT]
> 이 문서 (1804 릴리스)를 2018 년 5 월에 대 한 업데이트 하기 전에 플러그 인 버전에 적용 됩니다. 해당 시간 이후에 Xbox Live 플러그 인을 설치 하거나 아직 다운로드 하지 않은 경우에 로그인은 수행 하는 방법에 대 한 중요 한 차이점이 있는 최신 버전을 해야 합니다. 이 외에도이 플러그 인의 스크린샷에서 최신 버전의 정의와 일치 하지 않습니다를 찾을 수 있습니다. 대신 참조 하세요 합니다 [업데이트 된 로그인 prefab 문서](playerauthentication-prefab-sign-in.md) 뿐만 [로그인 스크립팅 하는 것에 대 한 업데이트 된 메서드를 자세히 보여 주는 문서](sign-in-manager.md)합니다.

## <a name="before-you-begin"></a>시작하기 전에

Unity 게임에 Xbox Live 소셜 서비스 추가 시작 하기 전에 계속 진행 하기 전에 완료 해야 하는 몇 가지 단계가 있습니다. 먼저 다운로드 하 고 통합을 확인 합니다 [Xbox Live Unity 플러그인](https://github.com/Microsoft/xbox-live-unity-plugin)합니다. 둘째,을 제목으로 예약 하 고이 통해 게시를 보유 하 려 합니다 [Microsoft 개발 센터](https://developer.microsoft.com/en-us/games/uwp)합니다. 읽기 [새 Xbox Live 크리에이터 스 프로그램 제목을](../get-started-with-creators/create-and-test-a-new-creators-title.md) 을 제목 게시에 대 한 지침은 합니다.
마지막으로, 읽 [구성 Xbox Live에서 Unity](../get-started-with-creators/configure-xbox-live-in-unity.md) Unity 환경을 제대로 설정 및 Xbox Live 서비스를 사용 하 여 제목을 구성 합니다. Unity 프로젝트에 제대로 설치 되 면 다음에 Unity에서 Xbox Live를 구현할 수는 두 가지 방법이 뿐만 아니라 Xbox Live를 사용 하도록 설정 제목으로 사용할 수 있습니다 도구에 대해 자세히 알아보려면: prefabs 및 스크립트.

## <a name="prefabs"></a>Prefabs

Unity 구성 요소 및 속성을 사용 하 여 완료를 GameObject를 저장 하도록 허용 하는 prefab 자산 형식을 있습니다. Prefab의 템플릿 역할을 올 Unity 장면에 새 개체 인스턴스를 만들 수 있습니다.
[Unity 웹 사이트에서 prefabs에 자세히 알아보려면](https://unity3d.com/learn/tutorials/topics/interface-essentials/prefabs-concept-usage)합니다.

Xbox Live Unity 플러그 인을 활용 하려면 프로젝트에서 사용할 수 있는 몇 가지 prefabs 제공 Xbox Live 기능입니다. 이 문서에 설명 된 prefabs를 사용 하면 로그인 할 수 [다중 사용자 지원 추가](../get-started-with-creators/add-multi-user-support.md) 프로필 제목 또는 Xbox Live에서 서명 된 friend 목록 표시 합니다. 이러한 및 다른 prefabs 아래에서 찾을 수 있습니다 합니다 **프로젝트** 경로 따라 탭 합니다. **자산 > Xbox Live > Prefabs**합니다.

### <a name="the-userprofile-prefab"></a>UserProfile prefab

가장 중요 한 첫 번째 소셜 prefab은 구성 된 **UserProfile** prefab 합니다. 합니다 **UserProfile** prefab에는 Xbox Live에 로그인을 허용 하는 데 필요한 모든 것입니다. 이 대로 해야 로그인 할 사용자 Xbox Live 서비스를 사용 하 여 이전에 매우 중요 합니다. 로그인 단추와 해당 게이머 태그, Gamerpic, 및 게임에서 플레이어에 로그인을 나타낼 GameObject를 prefab에 포함 되어 있습니다.

> [!NOTE]
> 다른 Xbox Live prefabs 중 하나를 사용 하려면 포함 해야 합니다는 **UserProfile** prefab 또는 수동으로 로그인 API를 호출 합니다.

![자산 및 계층 구조에서 UserProfile prefab](../images/unity/unity-userprofile-views.png)

확장 하는 경우는 **UserProfile** 의 prefab 합니다 **프로젝트** 패널 또는 **계층** 장면에 추가 된은 후 표시 하는 **UserProfile**  prefab 내부에 두 개의 Gameobject를 포함 합니다. 첫 번째 개체는 **SignInPanel** 로그인 단추 환경을 포함 하는 합니다. 두 번째 개체는 **ProfileInfo** 로그인 되 면 사용자에 대 한 정보가 포함 됩니다는 합니다. 합니다 **UserProfile** prefab은 로컬에 로그인을 제목 Xbox Live 사용자의 정보를 나타내는 데 사용 됩니다.

### <a name="the-xboxliveuser-prefab"></a>XboxLiveUser prefab

합니다 **UserProfile** prefab 호출 코드에서 두 번째 소셜 prefab을 사용 합니다 **XboxLiveUser**합니다. 코드에서 인스턴스화할 수 있습니다 단순히 대로 장면 계층을 추가할 필요 하지 않습니다이 prefab 사용 하이 여 즉시 명백해 아닙니다. 합니다 **XboxLiveUser** 시각적 표현이 없습니다 Xbox Live 사용자와 관련 된 세부 정보를 포함 하기만 하면 됩니다. 인스턴스를 해야 합니다 **XboxLiveUser** 의 모든 인스턴스에 대해는 **UserProfile**. 이 경우 중요 [다중 사용자 지원 추가](../get-started-with-creators/add-multi-user-support.md) 을 제목에 있습니다. 로그인 한 후 사용자에 대 한 정보를 보유 하는 것 외에도이 prefab은 또한 Xbox Live 사용자 로그인에 사용 되는 코드에 대 한 래퍼입니다.

## <a name="sign-in-with-the-userprofile-prefab"></a>UserProfile prefab으로 로그인

Xbox Live Unity 플러그 인 prefabs 훨씬 간편 하 게 특정 개발 작업 존재 합니다. Xbox Live에 로그인 하면는 Unity 프로젝트에 대해 사용 하도록 설정 합니다 **UserProfile** 및 **XboxLiveServices** Unity와 함께 prefabs **EventSystem**합니다.

먼저 끌어 합니다 **UserProfile** 는 장면을 prefab 합니다. 이상적으로 **UserProfile** 프로젝트의 초기 메뉴 화면에 배치 해야 합니다.

![사용자 프로필 계층으로 끌어 옵니다.](../images/unity/drag-userprofile.gif)

외에 **UserProfile** prefab 있는지 확인 해야 합니다 **XboxLiveServices** prefab은 프로젝트의 첫 번째 장면을 이상에 합니다.
합니다 **XboxLiveServices** prefab을 사용 하면 특정 prefabs 디버깅에 대 한 정보를 기록 하는 여부를 설정/해제할 수 있습니다. Prefab 동작에서 확인할 때 유용 합니다.

![prefab xboxliveservices 확인](../images/unity/check-for-xboxliveservices.gif)

마지막으로, 합니다 **UserProfile** 도 필요는 **EventSystem** 제대로 실행 합니다. 이 통해 클릭 한 다음 로그인 화면을 마우스 오른쪽 단추로 클릭 하 여 추가할 수 있습니다 **GameObject UI--> EventSystem-->** 합니다.

![이벤트 시스템 추가](../images/unity/add_event_system.gif)

재생 모드를 시작 하는 경우 서비스 사용자에 자동으로 로그인 됩니다. Unity에서 Xbox Live SDK Xbox Live 서비스에 대 한 호출을 시뮬레이션 하 고 작업을 할에 대 한 다시 모조 데이터를 보냅니다. 라이브 데이터를 보려면 UWP 응용 프로그램으로 프로젝트를 빌드하고 Visual Studio에서 실행 해야 합니다. 참조 [Unity에서 Xbox Live 구성](../get-started-with-creators/configure-xbox-live-in-unity.md) 자세한 내용은 합니다. 입력할 때 Unity에서 재생 모드, 게이머 태그, Gamerpic 플레이어의 게임 등의 정보를 시뮬레이션 하는 모조 데이터에는 prefab 채워집니다. 이 정보를 표시 해야 합니다 **UserProfile** prefab 합니다.

성공적인 로그인을 다음과 같이 표시 됩니다: ![성공적인 userprofile 재생](../images/unity/correct-user-profile-play.gif)

Xbox Live에 연결할 프로젝트를 구성 하지 않은 경우 재생 모드가 제대로 로그인 단추를 사용 하지 않도록 설정 되며 오류 메시지를 표시 합니다.

다음은 잘못 된 Xbox Live 응용 프로그램 구성으로 인해 실패 한 로그인의 예입니다.
![userprofile 재생을 하지 못했습니다.](../images/unity/flawed-user-profile-play.gif)

## <a name="scripting-sign-in"></a>로그인 스크립팅

사용 하는 방법을 배웠으므로 합니다 **UserProfile** prefab 것이 가장 좋습니다를 prefab의 기능을 제어 하는 기본 스크립트를 살펴보겠습니다. 보면를 **UserProfile** 에 **검사기** 에 표시 됩니다는 **UserProfile.cs** 스크립트 연결 된. 이 스크립트는 사용자를 로그인 및 로그인에 표시 하려는 프로필 정보를 로드 하는 데 필요한 모든 것입니다. 그러나 (시간이 지남에 따라 업데이트 될 수 있습니다)는 전체 prefab을 살펴보는 대신 하겠습니다 Xbox Live 사용자 로그인에 필요한 것을 이해 하려면 코드의 몇 가지 샘플 줄을 살펴보겠습니다.

### <a name="the-xboxliveuser-class"></a>XboxLiveUser 클래스

사용자 로그인 하는 데 필요한 호출에 래핑됩니다는 `XboxLiveUserInfo` 클래스입니다. 에 **UserProfile.cs** 스크립트의 인스턴스는 표시 됩니다는 `XboxLiveUserInfo` 라는 클래스 `XboxLiveUser`합니다. 예제 코드에 동일한 변수 이름을 사용 합니다. 합니다 `XboxLiveUserInfo` 클래스의 인스턴스를 포함 합니다 `XboxLiveUser` 라는 클래스 `User` 멤버 변수 중 하나로. 합니다 `XboxLiveUser` 에 로그인 하는 데 필요한 로그인 함수를 포함 하는 클래스는 `XboxLiveUser`합니다. 인스턴스를 사용 합니다 `XboxLiveUser` 클래스 `User` 로그인에 해당 게이머 태그, Gamerpic, 및 게임 등의 사용자도 이러한 기능을 설명 하는 정보를 얻는 대 한 합니다. 이 작업을 수행 하기 위해 인스턴스를 초기화 해야 합니다는 `XboxLiveUserInfo` 클래스 및 결과 사용 하 여 `XboxLiveUserInfo.User` 에 호출에 로그인 합니다.

### <a name="initialize-the-xboxliveuser"></a>XboxLiveUser 초기화

Xbox Live 사용자 초기화 실제로 이러한 Xbox Live에 로그인 하기 전에 첫 번째 단계입니다. 이렇게 하 여 코드에서 매우 간단 하 게 사용 하 여 `XboxLiveUserInfo.Initialize()` 함수.
예제 코드에서는 사용 합니다 `XboxLiveUser` 멤버 변수를이 `XboxLiveUserInfo` 인스턴스 및 로그인을 사용 하도록 초기화 합니다.

```csharp
    void ButtonClickTask()
    {
        this.StartCoroutine(this.InitializeXboxLiveUser());
    }

    public IEnumerator InitializeXboxLiveUserAndCallSignIn()
    {
        // Disable the sign-in button
        SignInButton.interactable = false;

        this.XboxLiveUser.Initialize();

        //Wait until the Xbox User has been initialized to call SignInAsync()
        yield return new WaitUntil(() => this.XboxLiveUser != null && this.XboxLiveUser.User != null);
        this.StartCoroutine(this.SignInAsync());
    }
```

이 샘플 코드에서 검색 했음을 알 수는 `XboxLiveUserInfo.Initialize()` 함수 단추 클릭에 대 한 응답으로 호출 됩니다. 전체 **UserProfile.cs** prefab 스크립트에 자동 로그인 허용 하는 코드는 `XboxLiveUserInfo.Initialize()` 상호 작용 없이 호출 됩니다.
합니다 `XboxLiveUserInfo.Initialize()` 함수는 새 만들지 `XboxLiveUserInfo.User` 에 포함 된 로그인 함수를 호출할 수 있는 `XboxLiveUserInfo.User` 클래스입니다.

### <a name="call-sign-in"></a>호출 로그인

XboxLiveUser 초기화 된 후에 호출 로그인 시간입니다. **UserProfile.cs** 로그인에서 호출 되는 `SignInAsync()` UserProfile.cs의 함수입니다. 앞의 예제 코드에서는 간단히 대기에 대 한 합니다 `XboxLiveUser` 를 호출 하기 전에 초기화할 수는 `SignInAsync()` 함수입니다.

> [!NOTE]
> 대기 해야 하는 합니다 `XboxLiveUser` 때문에 로그인을 호출 하기 전에 초기화할를 `XboxLiveUser` 포함는 `XboxLiveUser.User` 에 호출 로그인에 사용 되는 속성입니다.

**UserProfile.cs** 는 `SignInAsync()` 함수 두 로그인 하는 함수가 포함 사용자를 로그인 하는 데 사용 될 수 있습니다. `XboxLiveUser.User.SignInSilentlyAsync()` 및 `XboxLiveUser.User.SignInAsync()` 이것은 사용자를 로그인 하는 함수입니다. `SignInAsync()` 함수는 이러한 함수를 적절 하 게 사용 하는 방법의 좋은 예입니다. 다음 코드 샘플에는 두 개의 로그인 함수를 호출 하는 것이 적합 보여 줍니다.

```csharp
SignInStatus signInStatus;
TaskYieldInstruction<SignInResult> signInSilentlyTask = this.XboxLiveUser.User.SignInSilentlyAsync().AsCoroutine();
yield return signInSilentlyTask;

signInStatus = signInSilentlyTask.Result.Status;
if (signInSilentlyTask.Result.Status != SignInStatus.Success)
{
    TaskYieldInstruction<SignInResult> signInTask = this.XboxLiveUser.User.SignInAsync().AsCoroutine();
    yield return signInTask;

    signInStatus = signInTask.Result.Status;
}
```

이 예에서 로그인 호출의 결과 변수에 저장 된 `signInStatus`합니다. 이 속성을 사용 하면 여부 로그인에 성공 했는지를 적절 하 게 작동할 수 있습니다. 이 예제에서 함수는 먼저 자동으로 로그인을 만든 후에 시도 다음 자동 로그인 함수가 실패 하면 호출 일반 로그인 함수에서. 일단은 로그인 한 사용자 로그인 함수 중 하나를 성공적으로 호출 해야 합니다. 이제 사용할 수 있습니다는 `XboxLiveUser.User` 를 구해 하 여 로그인된 한 사용자에 대 한 세부 정보를 표시 합니다. 살펴봅니다를 `LoadProfileInfo()` 함수 **UserProfile.cs** 사용 하는 방법의 예는 `XboxLiveUser.User` 에서 로그인된 한 사용자 정보를 표시 합니다.

## <a name="build-and-test-sign-in"></a>빌드 및 로그인 테스트

제목, 편집기에서 실행할 때는 Xbox Live 기능을 사용 하려고 할 때 가짜 데이터가 나타납니다. 실제 프로필을 사용 하 여 로그인을 제목에서 Xbox Live 기능 테스트, UWP 솔루션을 구축 하 여 Visual Studio에서 실행 해야 합니다.  다음이 단계를 수행 하 여 Unity의 UWP 프로젝트를 빌드할 수 있습니다.

1. 엽니다는 **빌드 설정** 를 선택 하 여 창 **파일** > **빌드 설정**합니다.
2. 장면에서 빌드에 포함 하려는 모두 추가 합니다 **Scenes In Build** 섹션입니다.
3. 전환할 합니다 **유니버설 Windows 플랫폼** 를 선택 하 여 **유니버설 Windows 플랫폼** 아래 **플랫폼** 를 클릭 하 고 **플랫폼 전환**.
4. 설정할 **SDK** 하 **10.0.15063.0** 이상.
5. 스크립트 디버깅 검사를 사용 하도록 설정 하려면 **Unity C# 프로젝트**합니다.
6. 클릭 **빌드** 프로젝트의 위치를 지정 합니다.

빌드가 완료 되 면 Unity Visual Studio에서를 실행 해야 하는 새 UWP 솔루션 파일을 생성 합니다.

1. 지정 된 폴더를 엽니다  **&lt;ProjectName&gt;.sln** Visual Studio에서.
2. 맨 위에 있는 도구 모음에서 선택 **x64** 에 배포 하는 **로컬 컴퓨터**합니다.

사용 하도록 설정한 경우 **스크립트 디버깅** Unity에서 UWP 솔루션을 빌드할 때 다음 스크립트는 아래에 **어셈블리-CSharp (유니버설 Windows)** 프로젝트입니다.

> [!NOTE]
> 실제 데이터를 사용 하 여 게임을 테스트 하려면 Visual Studio 빌드를 사용 하기 전에 수행 [이 검사 목록을](test-visual-studio-build.md) 을 제목 Xbox Live 서비스에 액세스할 수 있도록 합니다.

## <a name="troubleshooting"></a>문제 해결

Xbox Live에 로그인 하는 문제 읽기를 시도 하면 [Xbox Live 문제 해결-로그인](../using-xbox-live/troubleshooting/troubleshooting-sign-in.md)합니다.
