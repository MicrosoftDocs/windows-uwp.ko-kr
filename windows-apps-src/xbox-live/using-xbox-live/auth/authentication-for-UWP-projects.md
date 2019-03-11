---
title: UWP 프로젝트에 대 한 인증
author: aablackm
description: 유니버설 Windows 플랫폼 (UWP) 제목에 사용자가 Xbox Live에 로그인 하는 방법에 알아봅니다.
ms.assetid: e54c98ce-e049-4189-a50d-bb1cb319697c
ms.author: aablackm
ms.date: 03/19/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 인증, 로그인
ms.localizationpriority: medium
ms.openlocfilehash: 5473b7ede7731d7d07b7e5bfd72857fdb64f1c89
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628388"
---
# <a name="authentication-for-uwp-projects"></a>UWP 프로젝트에 대 한 인증

게임에 Xbox Live 기능을 사용 하려면 사용자는 Xbox Live 커뮤니티에서 자신을 식별 하는 Xbox Live 프로필 만들기 해야 합니다.  Xbox Live 서비스와 관련 된 게임 한 추적 사용자 게이머 태그 등 사용자의 게임 친구는 게이머 그림 Xbox Live 프로필을 사용 하 여 활동 새로운 게임 사용자가 재생 사용자 잠금이 사용자는 여기서 어떤 성과 순위표를 특정, 게임 등에 대 한 합니다.

사용자가 특정 장치에 특정 게임에 Xbox Live 서비스에 액세스 하려고 하는 경우 사용자는 먼저 인증 해야 합니다.  게임 인증 프로세스를 시작 하려면 Xbox Live Api를 호출할 수 있습니다.  일부 경우에는 사용자가 표시 됩니다 username 및 password를 사용 하려면 Microsoft 계정을 입력 하는 등의 추가 정보를 제공 하는 인터페이스를 사용 하 여 게임에 대 한 사용 권한 동의 계정 문제 해결, 새로운 사용 약관 수락 등

인증 되 면 사용자가 명시적으로 로그인 할 Xbox Live에서 Xbox 앱에서 될 때까지 해당 장치를 사용 하 여 연결 합니다.  콘솔 내 장치에서 모든 Xbox Live 게임); (한 번에 인증할 수만 단일 플레이어  콘솔 내 장치에서 인증할 새 플레이어를 기존 인증된 플레이어 첫 번째 로그 아웃 해야 합니다.

## <a name="steps-to-sign-in"></a>로그인 하는 단계

높은 수준에서 다음이 단계를 수행 하 여 Xbox Live Api를 사용 합니다.

1. 사용자를 나타내는 XboxLiveUser 개체 만들기
2. 에 로그인 자동으로 Xbox Live 시작
3. 로그인에 UX를 사용 하 여 필요한 경우 시도
4. 상호 작용 하는 사용자를 기반으로 하는 Xbox Live 컨텍스트 만들기
5. Xbox Live 컨텍스트를 사용 하 여 Xbox Live 서비스 액세스
6. 경우 게임 종료 또는 사용자 기호 스케일 아웃, null로 설정 하 여 릴리스를 XboxLiveUser 개체 및 XboxLiveContext 개체

### <a name="creating-an-xboxliveuser-object"></a>XboxLiveUser 개체 만들기

대부분의 Xbox Live 활동 Xbox Live 사용자와 관련이 있습니다.  게임 개발자는 먼저 로컬 사용자를 나타내는 XboxLiveUser 개체를 만들 해야 합니다.

C++:

```cpp
auto xboxUser = std::make_shared<xbox_live_user>(Windows::System::User^ windowsSystemUser);
```

C + + /cli CX (WinRT):

```cpp
XboxLiveUser xboxUser = ref new XboxLiveUser(Windows::System::User^ windowsSystemUser);
```

C#(WinRT):

```csharp
XboxLiveUser xboxUser = new XboxLiveUser(Windows.System.User windowsSystemUser);
```

* **windowsSystemUser** xbox live 사용자와 연결 하는 데 사용할 windows 시스템 사용자 개체입니다. 앱은 단일 사용자 application(SUA) 경우 nullptr을 수 있습니다.
  * 단일 사용자 Application(SUA) 및 다중 사용자 Application(MUA)에 대 한 자세한 내용은 하십시오 [다중 사용자 응용 프로그램 소개](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/multi-user-applications#single-user-applications)
  * Windows::System::User를 가져오는 방법에 대 한 자세한 내용은 ^, Windows에서 확인 하십시오 [UWP의 windows 시스템 사용자를 검색 합니다.](retrieving-windows-system-user-on-UWP.md)

### <a name="sign-in-silently-to-xbox-live-at-startup"></a>에 로그인 자동으로 Xbox Live 시작 ###

게임을 Xbox Live 서비스에서 데이터를 사전 인출 사용자 인터페이스를 제공 하기 전에 실행 한 후 가능한 한 빨리 Xbox Live에 사용자 인증을 위해 시작 해야 합니다.

로컬 사용자를 자동으로 인증 호출

C++:

```cpp
pplx::task<xbox_live_result<sign_in_result>> xbox_live_user::signin_silently(Platform::Object^ coreDispatcher)
```

C + + /cli CX (WinRT):

```cpp
Windows::Foundation::IAsyncOperation<SignInResult^>^ XboxLiveUser::SignInSilentlyAsync(Platform::Object^ coreDispatcher)
```

C#(WinRT):

```csharp
Microsoft.Xbox.Services.System.SignInResult XboxLiveUser.SignInSilentlyAsync(Windows.UI.Core.CoreDispatcher coreDispatcher);
```

* **coreDispatcher**

  스레드 디스패처는 스레드 간의 통신에 사용 됩니다. 자동 로그인 API UI를 표시 하지 않습니다, 하지만 XSAPI appx의 로캘에 대 한 정보를 가져오기 위한 UI 스레드 디스패처를 여전히 필요 합니다. 정적을 가져올 수 있습니다 Windows::UI::Core::CoreWindow::GetForCurrentThread()를 호출 하 여 UI 스레드 디스패처 UI 스레드에서 디스패처를-> 합니다. 또는 (예: JS UWA)에서 nullptr에 전달할 수 인 경우이 API가 UI 스레드에서 호출 되는 특정 합니다.


자동 로그인 시도에서 가능한 결과 3 가지

* **성공**

  장치 온라인 상태 이면 즉, 사용자가 Xbox Live에 성공적으로 인증 하 고 유효한 토큰을 가져올 수 있었습니다.

  장치가 오프 라인 상태인 경우 즉 사용자가 성공적으로 인증 Xbox Live에 이전에 명시적으로 서명 된 스케일 아웃이이 제목에서 되지 않았습니다.  참고가 예에서 title을 보장 하지는 유효한 토큰에 대 한 액세스만 반드시 사용자의 id가 알려져 있고 확인 된는 있습니다.    사용자의 id와 사용자 id (xuid) xbox 게이머 태그를 통해 제목으로 알려져 있습니다.

* **UserInteractionRequired**

  즉, 런타임은에 로그인 하면 자동으로 수 없습니다.  게임 호출 해야 `xbox_live_user::sign_in` 등록-등록/로그인 사용자에 대 한 필요한 UX 흐름을 나타내기 위해 Xbox Id 공급자를 호출 합니다.  일반적인 문제는:

  * 사용자에 게 Microsoft 계정
  * 사용자가 게임에 대 한 기본 Microsoft 계정을 설정 하지
  * 선택한 Microsoft 계정에는 Xbox Live 프로필이 없는 합니다.
  * 사용자를 Microsoft 계정 동의 수락 해야 합니다.

* **기타 오류**

  런타임이 다른 이유로 인해 한 로그인 수 없습니다.  일반적으로 이러한 문제는 가능한 게임 또는 사용자가 없습니다. Xbox_live_result <>.err();를 선택 하 여 오류를 확인 해야 c + + API를 사용 하는 경우 WinRT에서 platform:: exception을 catch 해야 ^ 합니다.

### <a name="attempt-to-sign-in-with-ux-if-required"></a>로그인에 UX를 사용 하 여 필요한 경우 시도 ###

게임에 Xbox Live 자동 로그인에 성공 하지 고에서 사용자 인터페이스를 표시할 준비가 되었습니다. 사용 하도록 설정 하는 UX를 사용 하 여 사용자를 인증 해야 합니다.

UX 사용 하 여 로컬 사용자를 인증 하려면 호출

C++:

```cpp
pplx::task<xbox_live_result<sign_in_result>> xbox_live_user::signin(Platform::Object^ coreDispatcher)
```


C + + /cli CX (WinRT):

```cpp
Windows::Foundation::IAsyncOperation<SignInResult^>^ XboxLiveUser::SignInAsync(Platform::Object^ coreDispatcher)
```

C#(WinRT):

```csharp
Microsoft.Xbox.Services.System.SignInResult  XboxLiveUser.SignInAsync(Windows.UI.Core.CoreDispatcher coreDispatcher);
```

* **coreDispatcher**

  스레드 디스패처는 스레드 간의 통신에 사용 됩니다. API 로그인 UI 발송자는 필요 하므로 UI에서 기호를 표시 하 고 appx의 로캘에 대 한 정보를 가져올 수 있습니다. 정적을 가져올 수 있습니다 Windows::UI::Core::CoreWindow::GetForCurrentThread()를 호출 하 여 UI 스레드 디스패처 UI 스레드에서 디스패처를-> 합니다. 또는 (예: JS UWA)에서 nullptr에 전달할 수 인 경우이 API가 UI 스레드에서 호출 되는 특정 합니다.

UX 사용 하 여 로그인 시도에서 가능한 결과 3 가지가 있습니다.

* **성공**

  장치 온라인 상태 이면 즉, 사용자가 Xbox Live에 성공적으로 인증 하 고 유효한 토큰을 가져올 수 있었습니다.

  장치가 오프 라인 상태인 경우 즉 사용자가 성공적으로 인증 Xbox Live에 이전에 명시적으로 서명 된 스케일 아웃이이 제목에서 되지 않았습니다.  참고가 예에서 title을 보장 하지는 유효한 토큰에 대 한 액세스만 반드시 사용자의 id가 알려져 있고 확인 된는 있습니다.    사용자의 id 게이머 태그를 제목 xbox 사용자 id (xuid) 이라고 합니다.

* **UserCancel**

  이 사용자가 완료 되기 전에 로그인 작업을 취소 하는 것을 의미 합니다.  이 경우 게임은 자동으로 재시도 하지 사용자 환경으로 로그인  대신, 사용자 로그인 작업을 다시 시도를 허용 하는 게임 내 UX 제공 되어야 합니다.  (예: 로그인 단추)

* **기타 오류**

  런타임이 다른 이유로 인해 한 로그인 수 없습니다.  일반적으로 이러한 문제는 가능한 게임 또는 사용자가 없습니다. Xbox_live_result <>.err();를 선택 하 여 오류를 확인 해야 c + + API를 사용 하는 경우 WinRT에서 platform:: exception을 catch 해야 ^ 합니다.

## <a name="sign-in-code-examples"></a>로그인 코드 예제

### <a name="c"></a>C++

```cpp

#include "xsapi\services.h" // contains the xbox::services::system namespace

using namespace xbox::services::system; // contains definitions necessary for sign-in

void SignInSample::SignIn()
{
    //1. Create an xbox_live_user object
    m_user = std::make_shared<xbox::services::system::xbox_live_user>(); // m_user declared in header file

    //2. Sign-In silently to Xbox Live at startup
    m_user->signin_silently()
        .then([this](xbox::services::xbox_live_result<xbox::services::system::sign_in_result> result)
    {
        if (!result.err())
        {
            auto rPayload = result.payload();
            switch (rPayload.status())
            {
            case xbox::services::system::sign_in_status::success:
                // sign-in successful
                signIn = true;
                break;
            case xbox::services::system::sign_in_status::user_interaction_required:
                // 3. Attempt to sign-in with UX if required
                m_user->signin(Windows::UI::Core::CoreWindow::GetForCurrentThread()->Dispatcher)
                    .then([this](xbox::services::xbox_live_result<xbox::services::system::sign_in_result> loudResult) // use task_continuation_context::use_current() to make the continuation task running in current apartment 
                {
                    if (!loudResult.err())
                    {
                        auto resPayload = loudResult.payload();
                        switch (resPayload.status())
                        {
                        case xbox::services::system::sign_in_status::success:
                            // sign-in successful
                            signIn = true;
                            break;
                        case xbox::services::system::sign_in_status::user_cancel:
                            // user cancelled sign in 
                            // present in-game UX that allows the user to retry the sign-in operation. (For example, a sign-in button)
                            break;
                        }
                    }
                    else
                    {
                        //login has failed at this point
                    }
                }, concurrency::task_continuation_context::use_current());
                break;
            }
        }
    });
    if (signIn)
    {
        // 4. Create an Xbox Live context based on the interacting user
        m_xboxLiveContext = std::make_shared<xbox::services::xbox_live_context>(m_user); // m_xboxLiveContext declared in header file

        // add sign out event handler
        AddSignOut();
    }
}

void SignInSample::AddSignOut()
{
    xbox::services::system::xbox_live_user::add_sign_out_completed_handler(
        [this](const xbox::services::system::sign_out_completed_event_args&)

    {
        // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
        m_user = NULL;
        m_xboxLiveContext = NULL;
    });
}

```

### <a name="c-winrt"></a>C#(WinRT)

```csharp

using System.Diagnostics;
using Microsoft.Xbox.Services.System;
using Microsoft.Xbox.Services;

public async Task SignIn()
{
    bool signedIn = false;

    // Get a list of the active Windows users.
    IReadOnlyList<Windows.System.User> users = await Windows.System.User.FindAllAsync();

    // Acquire the CoreDispatcher which will be required for SignInSilentlyAsync and SignInAsync.
    Windows.UI.Core.CoreDispatcher UIDispatcher = Windows.UI.Xaml.Window.Current.CoreWindow.Dispatcher; 

    try
    {
        // 1. Create an XboxLiveUser object to represent the user
        XboxLiveUser primaryUser = new XboxLiveUser(users[0]);

        // 2. Sign-in silently to Xbox Live
        SignInResult signInSilentResult = await primaryUser.SignInSilentlyAsync(UIDispatcher);
        switch (signInSilentResult.Status)
        {
            case SignInStatus.Success:
                signedIn = true;
                break;
            case SignInStatus.UserInteractionRequired:
                //3. Attempt to sign-in with UX if required
                SignInResult signInLoud = await primaryUser.SignInAsync(UIDispatcher);
                switch(signInLoud.Status)
                {
                    case SignInStatus.Success:
                        signedIn = true;
                        break;
                    case SignInStatus.UserCancel:
                        // present in-game UX that allows the user to retry the sign-in operation. (For example, a sign-in button)
                        break;
                    default:
                        break;
                }
                break;
            default:
                break;
        }
        if(signedIn)
        {
            // 4. Create an Xbox Live context based on the interacting user
            Microsoft.Xbox.Services.XboxLiveContext m_xboxLiveContext = new Microsoft.Xbox.Services.XboxLiveContext(user);

            //add the sign out event handler
            XboxLiveUser.SignOutCompleted += OnSignOut;
        }
    }
    catch (Exception e)
    {
        Debug.WriteLine(e.Message);
    }

}

public void OnSignOut(object sender, SignOutCompletedEventArgs e)
    {
        // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
        primaryUser = null;
        xboxLiveContext = null;
    }
```

## <a name="sign-out"></a>로그 아웃

### <a name="handling-user-sign-out-completed-event"></a>처리 사용자 로그 아웃 완료 이벤트

사용자에 게 다음 중 하나가 발생 하는 경우 제목을에서 로그 아웃 합니다.

1. 사용자 로그인 스케일 아웃 Xbox 앱 (Windows 10) 또는 콘솔 셸 (Xbox One). 로그 아웃 하면 모든 Xbox Live를 사용 하도록 설정 앱이이 사용자에 대해 설치 영향을 줍니다.
2. 다른 Microsoft 계정으로 전환 하는 사용자
3. 다른 장치에서 동일한 제목에 로그인 한 사용자

이러한 모든 경우 제목에서 이벤트를 받게 됩니다 합니다 `xbox_live_user::add_sign_out_completed_handler` 또는 `XboxLiveUser::SignOutCompleted` 처리기입니다.  게임 로그 아웃 완료 이벤트를 적절 하 게 처리 해야 합니다.

1. 게임 라는 서명 된 스케일 아웃 Xbox Live에서 하는 사용자에 게 명확한 시각적 표시가 표시 됩니다.
2. 게임은 사용자가 이미 서명 된 스케일 아웃 하 고 사용할 수 있는 권한 부여 토큰 없이 있기 때문에 이벤트 처리기에서 Xbox Live 서비스 Api를 호출할 수 없습니다.

## <a name="sign-out-handler-code-samples"></a>처리기 코드 샘플 로그 아웃

### <a name="c"></a>C++

```cpp

xbox::services::system::xbox_live_user::add_sign_out_completed_handler(
        [this](const xbox::services::system::sign_out_completed_event_args&)

    {
        // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
        m_user = NULL;
        m_xboxLiveContext = NULL;
    });

```

### <a name="c-winrt"></a>C#(WinRT)

```csharp
XboxLiveUser.SignOutCompleted += OnUserSignOut;

public void OnSignOut(object sender, SignOutCompletedEventArgs e)
        {
            // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
            primaryUser = null;
            xboxLiveContext = null;
        }
```

## <a name="determining-if-the-device-is-offline"></a>장치가 오프 라인 상태 인지 확인 합니다.

Api 로그인 여전히 성공적으로 수행 됩니다 때 오프 라인으로 사용자가 한 번 로그인 하 고 계정에 로그인 하는 마지막 반환 됩니다.  

사용자가 없는 경우 이전에 오프 라인 로그인 달성할 수 없다는 것 서명 되었습니다.

제목을 오프 라인으로 재생할 수 있습니다 하는 경우 (캠페인 모드 등) WriteInGameEvent API 및 연결 된 저장소 API를 통해 게임 진행률 레코드를 둘 다 제대로 작동 하지만 장치가 오프 라인인 및 제목 사용자를 플레이할 수 있습니다.

제목을 오프 라인으로 재생할 수 없는 경우 제목을 장치가 오프 라인 상태 및 상태 및 가능한 솔루션에 대 한 사용자에 게 알릴 경우 알아보려면 GetNetworkConnectivityLevel API를 호출 해야 합니다 (멀티 플레이어 게임 또는 서버 기반 게임, 등) (예를 들어, ' 해야 계속 하려면 인터넷에 연결 ').

## <a name="online-status-code-samples"></a>온라인 상태 코드 샘플

### <a name="c"></a>C++

```cpp

using namespace Windows::Networking::Connectivity;

//Retrieve the ConnectionProfile
ConnectionProfile^ InternetConnectionProfile = NetworkInformation::GetInternetConnectionProfile();

NetworkConnectivityLevel connectionLevel = InternetConnectionProfile->GetNetworkConnectivityLevel();

switch (connectionLevel)
{
case NetworkConnectivityLevel::InternetAccess:
    // User is connected to the internet.
    break;
case NetworkConnectivityLevel::ConstrainedInternetAccess: //Limited Internet Access Possible Authentication Required
     // display error message for user.
    LogConnectivityLine("Game Offline: Limited internet access, browser authentication may be required. "); //function writes to UI
    break;
default:
    LogConnectivityLine("Game Offline: No internet access.");
    break;
}

```

### <a name="c-winrt"></a>C#(WinRT)

```csharp
using Windows.Networking.Connectivity;

//Retrieve the ConnectionProfile
string connectionProfileInfo = string.Empty;
ConnectionProfile InternetConnectionProfile = NetworkInformation.GetInternetConnectionProfile();

NetworkConnectivityLevel connectionLevel = InternetConnectionProfile.GetNetworkConnectivityLevel();

switch(connectionLevel)
    {
        case NetworkConnectivityLevel.InternetAccess:
            // User is connected to the internet.
            break;
        case NetworkConnectivityLevel.ConstrainedInternetAccess: //Limited Internet Access Possible Authentication Required
            // display error message for user.
            LogConnectivityLine("Game Offline: Limited internet access, browser authentication may be required. "); //function writes to UI
            break;
        default:
            LogConnectivityLine("Game Offline: No internet access.");
            break;
    }
```