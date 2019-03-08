---
title: Unity에서 SignInManager로 로그인
description: Unity 로그인 플러그 인 관리자 개요
ms.date: 05/08/2018
ms.topic: get-started-article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, unity
ms.openlocfilehash: e6d066fe7792912f8918cb139d45ff05d105feaa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641728"
---
# <a name="scripting-sign-in"></a>로그인 스크립팅

로그인 사용자 고유의 사용자 지정 게임 개체를 GameObject를 스크립트 해야 합니다 추가 하려면. PlayerAuthentication prefab 게임 맞지 및 고유한 로그인 패널이 하려는이 문서를 진행 하면 제목에 로그인 논리를 추가 하는 기본 단계를 한다고 가정해 보겠습니다.

## <a name="sign-in-with-the-signinmanager"></a>SignInManager를 사용 하 여 로그인

Xbox Live Unity 플러그 인에 대 한 스크립트를 포함 합니다 `SignInManager` 파일 경로의 **자산 >> XboxLive >> 스크립트 >> SignInManager.cs**합니다. 관리자가 호출 될 수 있는 폼 어디서 나 제목에 해당 타이틀의를 참조 하 여 단일 항목 클래스 *인스턴스* 의 `SignInManager`합니다. 이렇게 *인스턴스* 않습니다 초기화할 필요가 없고 있습니다 수 it 즉시 사용할 게임을 시작 합니다. 모든 액세스할 수 있습니다는 해당 public 속성과 참조 하 여 함수를 *인스턴스* 으로 `SignInManager.Instance`합니다.

`SignInManager` 을 제목에 대 한 인증을 관리, 여기에 로그인, 로그 아웃 하 고는 플레이어로 로그인 되어 있는 사용자에 대 한 정보를 가져오는 데 필요한 코드를 모두 포함 합니다.

### <a name="calls-and-results"></a>호출 및 결과

`SignInManager` 의 세 가지 비동기 공동 루틴 기능은 `SignInPlayer(int playerNumber)`, `SignOutPlayer(int playerNumber)`, 및 `SwitchUser(int playerNumber)`, 해당 트리거 이벤트 함수 호출의 결과 수집 하 고 적절 하 게 작동 합니다. 스크립트에 해당 하는 함수를 추가 하 고 할당 된 `SignInManager.Instance`의 콜백 목록입니다. 이벤트 기능은 `OnPlayerSignIn(int playerNumber, UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`, `OnPlayerSignOut(int playerNumber, UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`를 `OnAnyPlayerSignIn(UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`, 및 `OnAnyPlayerSignOut(UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`합니다. 이벤트 함수에서 각 이름에 설명 하는 이벤트를 수신 합니다. 제목에는 관리자의 콜백 목록에 고유한 함수를 추가할 수 있습니다 `Start()` 함수를 다음 코드로 바꿉니다.

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Client;

void Start () {
    try
    {
        SignInManager.Instance.OnPlayerSignOut(this.playerNumber, this.OnPlayerSignOut);
        SignInManager.Instance.OnPlayerSignIn(this.playerNumber, this.OnPlayerSignIn);
    }
    catch (Exception ex)
    {
        Debug.LogWarning(ex.Message);
    }

}
```

이 코드 조각은이 GameObject playerNumber 연관 플레이어에 대 한 로그인 및 로그 아웃 수신기를 추가 합니다. 이 GameObject `OnPlayerSignIn` 함수를 호출할 때 합니다 `SignInManager` 완료 된 로그인 시도 감지 및 해당 `OnPlayerSignOut` 는 SignInManager 감지를 로그 아웃 하면 함수가 호출 됩니다. 반환 형식 및 매개 변수는 SignInManager 호출한 함수 형식과 일치 하도록 프로그램 GameObject 이벤트 함수가 있어야 합니다. 모두를 `OnPlayerSignIn` 및 `OnPlayerSignOut` 필요한 void 함수는 `XboxLiveUser`, `XboxLiveAuthStatus`, 및 해당 매개 변수로 문자열입니다. 함수의 셸에서 다음과 같을 수 있습니다.

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Client;

private void OnPlayerSignIn(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
}

private void OnPlayerSignOut(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
}
```

두 함수 모두 확인 합니다 `XboxLiveAuthStatus` 있는지를 호출 하 여를 `SignInManager.Instance` 성공 했습니다. 성공적으로 호출할 합니다 `XboxLiveUser` 됩니다 합니다 `XboxLiveUser`에서 우리의 아웃이 로그인 `SignInManager`. 호출이 성공 없을 때를 `errorMessage` 문자열 원인 오류에 대 한 세부 정보에 포함 됩니다.

다음과 같은 코드에서 반환 호출이 성공 하면 확인 하는 코드 몇 줄을 추가 합니다.

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Client;

private void OnPlayerSignIn(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
    if(authStatus == XboxLiveAuthStatus.Succeeded)
    {
        this.xboxLiveUser = xboxLiveUserParam; //store the xboxLiveUser SignedIn
        this.signedIn = true;
    }
    else
    {
        if (XboxLiveServicesSettings.Instance.DebugLogsOn)
        {
            Debug.LogError(errorMessage); //Log the error message in case of unsuccessful call. 
        }
    }
}

private void OnPlayerSignOut(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
    if (authStatus == XboxLiveAuthStatus.Succeeded)
    {
        this.xboxLiveUser = null;
        this.signedIn = false;
    }
    else
    {
        if (XboxLiveServicesSettings.Instance.DebugLogsOn)
        {
            Debug.LogError(errorMessage);
        }
    }
}
```

호출 하 여 로그인 결과 대 한 결과 이벤트를 캡처, 제목에 대 한 로그인 및 로그 아웃을 처리할 수 있습니다.

## <a name="get-signed-in-player-information"></a>플레이어 정보에 로그온

플레이어 서비스에 로그인 하는 것 외에도 SignInManager는 모든 로그인된 사용자의 추적 합니다. PC에이 제한 됩니다는 단일 플레이어에 로그인 하 고 xbox 16으로 제한 됩니다. 어떻게 거의 한도 결과 비교 하 여 확인할 수 있습니다 `SignInManager.Instance.GetCurrentNumberOfPlayers()` 의 결과로 `SignInManager.Instance.GetMaximumNumberOfPlayers()`합니다. SignInManager에 로그인 플레이어는 플레이어의으로 인덱싱된 사전을 *playerNumber*합니다. 해당 연결에서 액세스할 수 있는 플레이어에 대 한 기본 정보를 검색 하는 데 사용할 수 있습니다 `XboxLiveUser`합니다.

```csharp
if (SignInManager.Instance.GetPlayer(this.playerNumber).IsSignedIn) // If there is a player signed in for this gameObjects player number
            {
                this.displayedGamertag = SignInManager.Instance.GetPlayer(this.playerNumber).Gamertag; // Set that users gamertag to the gamertag displayed
            }
```

이 약간의 코드에이 GameObject 플레이어 수 슬롯에 로그인 하는 플레이어 인지를 확인 하 고 로그인 되어 있는 경우 표시 되는 사용자가 게이머 태그를 저장 합니다. 하는 동안 합니다 `XboxLiveUser` 와 같은 사용자 게이머 태그 및 Xbox 사용자 ID (xuid) 다른 서비스를 호출 해야 합니다는 서명 포함는 `SocialManager` gamerpic 및 게임 같은 정보에 액세스할 수 합니다.

## <a name="destroying-your-sign-in-gameobject"></a>사용자 로그인 GameObject를 제거합니다.

Xbox Live 플러그 인 관리자 중 하나를 사용 하는 게임 개체를 제거 하는 경우와 같은 합니다 `SignInManager` 또는 `SocialManager`일반적으로 새 장면을 로드 때 관리자에 대 한 이벤트 수신기 목록에 추가 하는 모든 함수를 제거 해야 합니다. 이 문서에 대 한 예제 코드에서의 로그인 및 로그 아웃에 대 한 이벤트 수신기에 추가한 함수를 제거 해야 합니다. 이러한 함수를 제거할 것을 `SignInManager` 에 `OnDestroy()` 우리의 GameObject 함수.

```csharp
private void OnDestroy()
{
    if (SignInManager.Instance != null)
    {
        SignInManager.Instance.RemoveCallbackFromPlayer(this.PlayerNumber, this.OnPlayerSignOut);
        SignInManager.Instance.RemoveCallbackFromPlayer(this.PlayerNumber, this.OnPlayerSignIn);
    }
```

이 코드는이 GameObject 연관 플레이어에 대 한 로그인 및 로그 아웃 콜백 함수를 제거 합니다.

## <a name="testing-you-code-in-visual-studio"></a>Visual Studio에서 코드 테스트

이외에 [Visual Studio에서 게임을 빌드하는 데 필요한 단계](configure-xbox-live-in-unity.md#build-and-test-the-project)에 나열 된는 [Unity의 Xbox Live 제목을 구성](configure-xbox-live-in-unity.md) 문서를 제대로 게임을 테스트 하는 데 필요한 추가 단계를 Visual Studio입니다. Package.appxmanifest.xml 파일의 속성을 업데이트 해야 합니다. 이렇게 하려면 다음을 수행합니다.

1. Package.appxmanifest.xml 파일에 대 한 솔루션 탐색기 검색
2. 파일을 마우스 오른쪽 단추로 클릭 하 고 코드 보기를 선택 합니다.
3. 아래는 `<Properties><\/Properties>` 섹션에서 다음 줄을 추가 합니다. ' < uap:SupportedUsers > 여러 <\/uap:SupportedUsers >.
4. Visual Studio에서 원격 디버깅 빌드를 시작 하 여 Xbox로 게임을 배포 합니다. Xbox에 제목을 설정 하는 지침을 찾을 수 있습니다 합니다 [Xbox 개발 환경에서 프로그램 UWP 설정](../../xbox-apps/development-environment-setup.md) 문서.

> [!NOTE]
> 구성이 변경 되었습니다. 부분에는 여전히 단일 플레이어 시나리오에서 게임을 실행 해야 하는 멀티 플레이어를 사용 하는 것 처럼 보일 수 있습니다.

## <a name="policies-and-limitations"></a>정책 및 제한 사항

로그인 정책 및 제목의 로그인 환경을 개발할 때 고려할 수 있는 제한 사항이 있습니다.

- 타이틀의 초기 로그인 후 로그인 하는 하나 이상의 플레이어를 유지 해야 합니다. `SignInManager` 오류가 throw 되며 마지막으로 로그인 한 사용자 로그 아웃 하려는 경우 호출이 실패 합니다. 호출할 수는 없으며 알아야 이기도 `SignInManager.Instance.SwitchUser(int playerNumber)`의 플레이어 새 플레이어를 로그인 하기 전에 로그 아웃 하려고 하는 대로 마지막 플레이어에 로그인 합니다.

- PC 수 있습니다에 로그인 한 사용자를 한 번에 콘솔에 한 번에 최대 16 명의 플레이어에 서명할 수 있습니다.

- Title 실제로 OS에서 플레이어를 로그 아웃 하려면 권한 없는,이 SignOut으로 인해 예상과 다르게 작동할 수 있습니다. SignInManager 및 제목 관련이 있는 출력에 사용자를 서명할 수 있지만 수 없습니다. 로그 아웃 누구나 제목에 배포 된 컴퓨터에서.

- 여러 사용자 로그인은 Xbox 1 본체에서 사용할 수만 있습니다.

- 게스트 계정 Xbox Live 크리에이터 스 프로그램 제목에 사용할 수 없는 경우