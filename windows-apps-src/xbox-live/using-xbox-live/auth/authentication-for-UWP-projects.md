---
title: UWP 프로젝트에 대 한 인증
author: aablackm
description: 유니버설 Windows 플랫폼 (UWP) 타이틀에 Xbox Live 사용자가 로그인 하는 방법을 알아봅니다.
ms.assetid: e54c98ce-e049-4189-a50d-bb1cb319697c
ms.author: aablackm
ms.date: 03/14/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 인증에 로그인
ms.localizationpriority: medium
ms.openlocfilehash: e6e833601bc02052a841c78bc4f140cb9f45e74f
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2018
ms.locfileid: "5165509"
---
# <a name="authentication-for-uwp-projects"></a>UWP 프로젝트에 대 한 인증

게임에 Xbox Live 기능을 활용 되려면 사용자를 Xbox Live 커뮤니티에서 자신을 식별 하는 Xbox Live 프로필을 생성 해야 합니다.  Xbox Live 서비스 게임 관련 한 추적 해당 사용자의 게이머 태그 및 사용자의 게임 친구 들 게이머 사진 등의 Xbox Live 프로필을 사용 하 여 활동 어떻게 게임 사용자가 재생 사용자가 잠금 해제, 사용자 의미 있는 어떤 도전 과제는 특정 게임 등에 대 한 순위표 합니다.

사용자를 특정 장치에서 특정 게임에서 Xbox Live 서비스에 액세스 하려는 경우 사용자는 먼저 인증 해야 합니다.  게임에서 인증 프로세스를 시작할 Xbox Live Api를 호출할 수 있습니다.  경우에 따라 사용자가 나타납니다 이름과 사용 하려면 Microsoft 계정의 암호를 입력 하는 등 추가 정보를 제공 하는 인터페이스를 사용 하 여 게임을 권한에 동의 제공, 계정 문제 해결, 새 서비스 계약을 수락 등입니다.

인증 되 면 사용자가 명시적으로 로그 아웃 Xbox Live Xbox 앱에서 때까지 해당 장치에서 연관 됩니다.  하나의 플레이어 중이면 (게임에 대 한 모든 Xbox Live); 한 번에 장치에서 인증을  장치에 인증 하는 새 플레이어에 대 한 기존 인증된 플레이어가 아웃 서명 해야 합니다.

## <a name="steps-to-sign-in"></a>로그인 하는 단계

높은 수준에서 다음 단계를 수행 하 여 Xbox Live Api를 사용 합니다.

1. 사용자를 나타내는 XboxLiveUser 개체 만들기
2. 로그인에 자동으로 Xbox Live 시작 시
3. 로그인 UX를 사용 하 여 필요한 경우 하려고 시도
4. 상호 작용 사용자를 기준으로 하는 Xbox Live 컨텍스트 만들기
5. Xbox Live 컨텍스트를 사용 하 여 Xbox Live 서비스에 액세스 하려면
6. 때 게임 종료 또는 사용자가 기호 아웃, XboxLiveUser 개체와 XboxLiveContext 개체를 null로 설정 하 여 해제

### <a name="creating-an-xboxliveuser-object"></a>XboxLiveUser 개체 만들기

대부분의 Xbox Live 작업은 Xbox Live 사용자에 게 관련 되어 합니다.  게임 개발자, 로컬 사용자를 나타내는 XboxLiveUser 개체를 만들려면 먼저 필요 합니다.

C++:

```cpp
auto xboxUser = std::make_shared<xbox_live_user>(Windows::System::User^ windowsSystemUser);
```

C + + CX (WinRT):

```cpp
XboxLiveUser xboxUser = ref new XboxLiveUser(Windows::System::User^ windowsSystemUser);
```

C# (WinRT):

```csharp
XboxLiveUser xboxUser = new XboxLiveUser(Windows.System.User windowsSystemUser);
```

* **windowsSystemUser** Windows 시스템 사용자 개체 xbox와 연결 하는 데 사용할 사용자를 live 합니다. 앱은 단일 사용자 application(SUA) 경우 nullptr 수 있습니다.
  * 단일 사용자 Application(SUA) 및 다중 사용자 Application(MUA)에 대 한 자세한 내용은 [다중 사용자 응용 프로그램 소개](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/multi-user-applications#single-user-applications) 를 확인 하세요
  * Windows::System::User를 얻는 방법에 대 한 자세한 내용은 ^ 창에서 확인 하세요 [UWP에서 windows 시스템 사용자 검색](retrieving-windows-system-user-on-UWP.md)

### <a name="sign-in-silently-to-xbox-live-at-startup"></a>로그인에 자동으로 Xbox Live 시작 시 ###

게임에서 사용자를 인증 Xbox Live를 최대한 빨리를 시작한 후 Xbox Live 서비스에서 데이터를 미리 사용자 인터페이스를 제공 하기 전에도 시작 해야 합니다.

로컬 사용자를 자동으로 인증.

C++:

```cpp
pplx::task<xbox_live_result<sign_in_result>> xbox_live_user::signin_silently(Platform::Object^ coreDispatcher)
```

C + + CX (WinRT):

```cpp
Windows::Foundation::IAsyncOperation<SignInResult^>^ XboxLiveUser::SignInSilentlyAsync(Platform::Object^ coreDispatcher)
```

C# (WinRT):

```csharp
Microsoft.Xbox.Services.System.SignInResult XboxLiveUser.SignInSilentlyAsync(Windows.UI.Core.CoreDispatcher coreDispatcher);
```

* **coreDispatcher**

  스레드 디스패처는 스레드 간 통신에 사용 됩니다. 자동 로그인 API UI를 표시 하는 것은, XSAPI에도 여전히 UI 스레드 디스패처 appx의 로캘에 대 한 정보를 가져오는 데 필요한 해야 합니다. 정적 가져올 수 있습니다 Windows::UI::Core::CoreWindow::GetForCurrentThread()를 호출 하 여 UI 스레드 디스패처 UI 스레드의 디스패처-> 합니다. 또는 인 경우이 API는 UI 스레드에서 호출 되는 특정 전달할 수 nullptr (예를 들어 JS UWA).


자동 로그인 시도에서 가능한 결과 3 가지

* **성공**

  장치가 온라인 상태인, 즉, 사용자가 Xbox Live를 성공적으로 인증 하 고 유효한 토큰을 가져올 수 있었습니다.

  디바이스가 오프 라인 이면이 사용자가 성공적으로 인증 Xbox Live에 이전에 하 고 명시적으로 서명 아웃이이 제목에서 되지 않았음을 의미 합니다.  참고가 경우 제목 보장 하지는 올바른 토큰이에 대 한 액세스를 사용자의 id 라고 하 고 판명 된만 보장 됩니다.    사용자의 id는 통해 자신의 xbox 사용자 id (xuid) 및 게이머 제목으로 알려져 있습니다.

* **UserInteractionRequired**

  즉, 런타임은 로그인 사용자가 자동으로 수 없습니다.  게임 호출 해야 `xbox_live_user::sign_in` sign-up/에 로그인 하 고 사용자에 대 한 필요한 UX 흐름을 표시 하는 Xbox Id 공급자를 호출 합니다.  일반적인 사항은 다음과 같습니다.

  * 사용자가 Microsoft 계정이 없습니다
  * 사용자가 게임에 대 한 기본 Microsoft 계정을 설정 하지
  * 선택한 Microsoft 계정에 Xbox Live 프로필을 없는 합니다.
  * 사용자가 Microsoft 계정 동의 수락 하는 데 필요한

* **기타 오류**

  런타임은 로그인 다른 이유로 인해 수 없습니다.  일반적으로 이러한 문제는 게임 또는 사용자가 실행 가능한 되지 않습니다. Xbox_live_result <>.err();을 확인 하 여 오류를 확인 해야 c + + API를 사용 하는 경우 platform:: exception catch 해야 WinRT에서 ^ 합니다.

### <a name="attempt-to-sign-in-with-ux-if-required"></a>로그인 UX를 사용 하 여 필요한 경우 하려고 시도 ###

게임 UX 자동 로그인 성공 하지, 및 사용자 인터페이스를 표시할 준비가 경우 활성화를 사용 하 여 Xbox Live에 사용자를 인증 해야 합니다.

UX 사용 하 여 로컬 사용자를 인증.

C++:

```cpp
pplx::task<xbox_live_result<sign_in_result>> xbox_live_user::signin(Platform::Object^ coreDispatcher)
```


C + + CX (WinRT):

```cpp
Windows::Foundation::IAsyncOperation<SignInResult^>^ XboxLiveUser::SignInAsync(Platform::Object^ coreDispatcher)
```

C# (WinRT):

```csharp
Microsoft.Xbox.Services.System.SignInResult  XboxLiveUser.SignInAsync(Windows.UI.Core.CoreDispatcher coreDispatcher);
```

* **coreDispatcher**

  스레드 디스패처는 스레드 간 통신에 사용 됩니다. API 로그인 UI의 부호를 표시 하 고 appx의 로캘에 대 한 정보를 가져올 수 있도록 UI 디스패처가 필요 합니다. 정적 가져올 수 있습니다 Windows::UI::Core::CoreWindow::GetForCurrentThread()를 호출 하 여 UI 스레드 디스패처 UI 스레드의 디스패처-> 합니다. 또는 인 경우이 API는 UI 스레드에서 호출 되는 특정 전달할 수 nullptr (예를 들어 JS UWA).

UX 사용 하 여 로그인 시도에서 가능한 결과 3 가지 있습니다.

* **성공**

  장치가 온라인 상태인, 즉, 사용자가 Xbox Live를 성공적으로 인증 하 고 유효한 토큰을 가져올 수 있었습니다.

  디바이스가 오프 라인 이면이 사용자가 성공적으로 인증 Xbox Live에 이전에 하 고 명시적으로 서명 아웃이이 제목에서 되지 않았음을 의미 합니다.  참고가 경우 제목 보장 하지는 올바른 토큰이에 대 한 액세스를 사용자의 id 라고 하 고 판명 된만 보장 됩니다.    사용자의 id 제목 xbox 사용자 id (xuid) 및 게이머도 알려져 있습니다.

* **UserCancel**

  즉,는 사용자가 완료 되기 전에 로그인 작업을 취소 합니다.  이 경우 게임 해야 자동으로 다시 로그인 된 사용자 환경  대신, 사용자가 로그인 작업을 다시 시도를 허용 하는 게임 UX 제공 되어야 합니다.  (예를 들어 로그인 단추)

* **기타 오류**

  런타임은 로그인 다른 이유로 인해 수 없습니다.  일반적으로 이러한 문제는 게임 또는 사용자가 실행 가능한 되지 않습니다. Xbox_live_result <>.err();을 확인 하 여 오류를 확인 해야 c + + API를 사용 하는 경우 platform:: exception catch 해야 WinRT에서 ^ 합니다.

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

### <a name="c-winrt"></a>C# (WinRT)

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

## <a name="sign-out"></a>로그아웃

### <a name="handling-user-sign-out-completed-event"></a>처리 사용자 로그 아웃 완료 이벤트

다음 중 하나가 발생 하는 경우 사용자 타이틀에서 로그 아웃 됩니다.

1. 사용자가 서명 아웃 Xbox 앱 (Windows 10) 또는 콘솔 셸 (Xbox One)에서. 로그 아웃 하면 모든 Xbox Live가 지원 앱이이 사용자에 대해 설치 된 영향을 미칩니다.
2. 사용자가 다른 Microsoft 계정으로 전환
3. 사용자가 다른 장치에서 동일한 제목에 로그인

이러한 모든 경우에 제목에서 이벤트를 받게 됩니다 합니다 `xbox_live_user::add_sign_out_completed_handler` 또는 `XboxLiveUser::SignOutCompleted` 처리기.  게임 로그 아웃 완료 이벤트를 적절 하 게 처리 해야 합니다.

1. 게임 명확한 시각적 표시는 she/he가 서명 아웃 Xbox Live에서 사용자에 게 표시 됩니다.
2. 사용자가 이미 서명 아웃 하 고 사용할 수 있는 인증 토큰을 없는 때문에 게임에 이벤트 처리기에서 Api 모든 Xbox Live 서비스를 호출할 수 없습니다.

## <a name="sign-out-handler-code-samples"></a>로그 아웃 처리기 코드 샘플

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

### <a name="c-winrt"></a>C# (WinRT)

```csharp
XboxLiveUser.SignOutCompleted += OnUserSignOut;

public void OnSignOut(object sender, SignOutCompletedEventArgs e)
        {
            // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
            primaryUser = null;
            xboxLiveContext = null;
        }
```

## <a name="determining-if-the-device-is-offline"></a>디바이스가 오프 라인 상태인 경우 결정

Api 로그인은 계속 오프 라인 상태 성공 경우 사용자가 한 번 로그인 및 계정 로그인 마지막 반환 됩니다.

제목 오프 라인 재생할 수 (캠페인 모드, 등) 관계 없이 장치가 온라인 또는 오프 라인으로 제목 사용자가 재생 하도록 허용할 수 있고 WriteInGameEvent API와 연결 된 저장소 API를 통해 게임 진행 상황 기록, 둘 다 제대로 작동 장치는 오프 라인 합니다.

제목 오프 라인 재생할 수 없는 경우 (멀티 플레이어 게임 또는 서버 기반 게임 등.) 제목 해야는 오프 라인으로 하는지 알아보려면 GetNetworkConnectivityLevel API를 호출 하 고 상태 및 가능한 솔루션에 대 한 사용자에 게 알립니다 (예를 들어 ' 필요한 계속 하려면 인터넷에 연결할')

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

### <a name="c-winrt"></a>C# (WinRT)

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