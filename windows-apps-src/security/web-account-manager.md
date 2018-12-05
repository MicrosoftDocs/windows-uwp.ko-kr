---
title: 웹 계정 관리자
description: '이 문서는 Windows10 웹 계정 관리자 API를 사용하여 AccountsSettingsPane에서 UWP(유니버설 Windows 플랫폼) 앱을 외부 ID 공급자(예: Microsoft, Facebook)에 연결하는 방법에 대해 설명합니다.'
ms.date: 12/6/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.assetid: ec9293a1-237d-47b4-bcde-18112586241a
ms.localizationpriority: medium
ms.openlocfilehash: b0a052d08c6b0816d977d3e86881540194075818
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8701422"
---
# <a name="web-account-manager"></a>웹 계정 관리자

이 문서는 Windows10 웹 계정 관리자 API를 사용하여 **[AccountsSettingsPane](https://docs.microsoft.com/uwp/api/Windows.UI.ApplicationSettings.AccountsSettingsPane)** 에서 UWP(유니버설 Windows 플랫폼) 앱을 외부 ID 공급자(예: Microsoft, Facebook)에 연결하는 방법에 대해 설명합니다. 사용자의 사용 권한에서 Microsoft 계정을 사용하도록 요청하고, 액세스 토큰을 받고, 이를 사용하여 기본 작업(프로필 데이터 가져오기, OneDrive 계정에 파일 업로드)을 수행하는 방법에 대해 살펴보겠습니다. 해당 단계는 웹 계정 관리자를 지원하는 ID 공급자를 사용하여 사용자 권한 및 액세스를 획득하는 과정과 비슷합니다.

> [!NOTE]
> 전체 코드 샘플을 보려면 [GitHub의 WebAccountManagement 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620621)을 참조하세요.

## <a name="get-set-up"></a>설정 방법

먼저 Visual Studio에서 비어 있는 새 솔루션을 만듭니다. 

둘째, ID 공급자에 연결하기 위해 앱을 스토어에 연결해야 합니다. 이렇게 하려면 프로젝트를 마우스 오른쪽 단추로 클릭하고 **스토어** > **스토어에 앱 연결**을 선택하고 마법사의 지침을 따릅니다. 

셋째로, 간단한 XAML 단추와 2개의 텍스트 상자로 구성된 매우 기본적인 UI를 만듭니다.

```XML
<StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
    <Button x:Name="LoginButton" Content="Log in" Click="LoginButton_Click" />
    <TextBlock x:Name="UserIdTextBlock"/>
    <TextBlock x:Name="UserNameTextBlock"/>
</StackPanel>
```

또한 이벤트 처리기가 코드 숨김 파일에서 단추에 연결됩니다.

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{   
}
```

마지막으로 나중에 참조 문제에 걱정하지 않아도 되도록 다음 네임스페이스를 추가합니다. 

```csharp
using System;
using Windows.Security.Authentication.Web.Core;
using Windows.System;
using Windows.UI.ApplicationSettings;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.Data.Json;
using Windows.UI.Xaml.Navigation;
using Windows.Web.Http;
```

## <a name="show-the-accounts-settings-pane"></a>계정 설정 창 표시

시스템은 ID 공급자 및 **AccountsSettingsPane**이라고 하는 웹 계정을 관리하기 위한 기본 제공 사용자 인터페이스를 제공합니다. 아래와 같이 표시할 수 있습니다.

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{
    AccountsSettingsPane.Show(); 
}
```

앱을 실행하고 "로그인" 단추를 클릭하면 빈 창이 표시됩니다. 

![계정 설정 창](images/tb-1.png)

시스템은 UI 셸만 제공하므로 창으 비어 있습니다. 프로그래밍 방식을 사용하여 ID 공급자로 창을 채우는 것은 개발자의 책임입니다. 

## <a name="register-for-accountcommandsrequested"></a>AccountCommandsRequested 등록

창에 명령을 추가하려면 먼저 AccountCommandsRequested 이벤트 처리기를 등록합니다. 이렇게 하면 사용자가 창을 표시하도록 요청할 때(예: XAML 단추 클릭) 시스템은 빌드 논리를 실행하라는 지시를 받게 됩니다. 

코드 숨김 파일에서 OnNavigatedTo 및 OnNavigatedFrom 이벤트를 재정의하고 다음 코드를 추가합니다. 

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    AccountsSettingsPane.GetForCurrentView().AccountCommandsRequested += BuildPaneAsync; 
}
```

```csharp
protected override void OnNavigatedFrom(NavigationEventArgs e)
{
    AccountsSettingsPane.GetForCurrentView().AccountCommandsRequested -= BuildPaneAsync; 
}
```

사용자가 계정을 자주 조작하지 않으므로 이러한 방식으로 이벤트 처리기를 등록 및 등록 취소하면 메모리 누수를 방지하는 데 도움이 됩니다. 이러한 방식으로 사용자 지정된 창은 사용자가 요청할 가능성이 높은 경우에만 메모리에 있습니다(예를 들어 “설정" 또는 “로그인” 페이지에 있음). 

## <a name="build-the-account-settings-pane"></a>계정 설정 창 빌드

**AccountsSettingsPane**이 표시될 때마다 BuildPaneAsync 메서드가 호출됩니다. 창에 표시되는 명령을 사용자 지정하기 위한 코드를 여기에 삽입합니다. 

먼저 지연을 획득합니다. 이렇게 하면 빌드가 완료될 때까지 **AccountsSettingsPane** 표시를 지연하라고 시스템에 지시됩니다.

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    deferral.Complete(); 
}
```

다음으로 WebAuthenticationCoreManager.FindAccountProviderAsync 메서드를 사용하여 공급자를 가져옵니다. 공급자의 URL은 공급자에 따라 다르며 공급자 설명서에서 찾을 수 있습니다. Microsoft 계정 및 Azure Active Directory의 경우 해당 URL은 "https://login.microsoft.com"입니다. 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    var msaProvider = await WebAuthenticationCoreManager.FindAccountProviderAsync(
        "https://login.microsoft.com", "consumers"); 
        
    deferral.Complete(); 
}
```

또한 선택적 *authority* 매개 변수에 문자열 "consumers"를 전달합니다. Microsoft는 두 가지 유형의 인증, 즉 “consumers"의 경우는 MSA(Microsoft 계정)를, “organizations"의 경우는 AAD(Azure Active Directory)를 제공하기 때문입니다. "consumers" 권한은 MSA 옵션을 원한다는 것을 나타냅니다. 엔터프라이즈 앱을 개발하는 경우 문자열 "organizations"를 대신 사용합니다.

마지막으로 다음과 같은 새 **[WebAccountProviderCommand](https://docs.microsoft.com/uwp/api/windows.ui.applicationsettings.webaccountprovidercommand)** 를 만들어 **AccountsSettingsPane**에 공급자를 추가합니다. 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();

    var msaProvider = await WebAuthenticationCoreManager.FindAccountProviderAsync(
        "https://login.microsoft.com", "consumers");

    var command = new WebAccountProviderCommand(msaProvider, GetMsaTokenAsync);  

    e.WebAccountProviderCommands.Add(command);

    deferral.Complete(); 
}
```

새 **WebAccountProviderCommand**에 전달할 GetMsaToken 메서드가 아직 없으므로(다음 단계에서 빌드) 일단 빈 메서드에 추가하면 됩니다.

위의 코드를 실행하면 창이 다음과 같이 표시됩니다. 

![계정 설정 창](images/tb-2.png)

### <a name="request-a-token"></a>토큰 요청

**AccountsSettingsPane**에 Microsoft 계정 옵션이 표시되면 사용자가 선택할 때 어떤 결과가 나타날지 처리해야 합니다. 사용자가 해당 Microsoft 계정으로 로그인하도록 선택할 때 GetMsaToken 메서드가 발생하도록 등록했으므로 해당 토큰을 가져올 것입니다. 

토큰을 획득하려면 다음과 같이 RequestTokenAsync 메서드를 사용합니다. 

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
}
```

이 예제에서는 문자열 "wl.basic"을 _scope_ 매개 변수에 전달합니다. 범위는 제공 서비스로부터 요청하는 정보 유형을 특정 사용자에 나타냅니다. 특정 범위에서는 이름 및 이메일 주소와 같은 사용자의 기본 정보에만 액세스할 수 있지만 다른 범위에서는 사용자의 사진이나 이메일 받은 편지함과 같은 중요한 정보에 대한 액세스를 허용할 수 있습니다. 일반적으로 앱은 해당 기능을 수행하는 데 필요한 최소 허용 범위를 사용해야 합니다. 서비스 공급자는 서비스에 사용할 토큰을 가져오는 데 필요한 범위에 대한 설명서를 제공합니다. 

* Office 365 및 Outlook.com 범위의 경우 [v2.0 인증 끝점을 사용하여 Office 365 및 Outlook.com API 인증](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)을 참조하세요. 
* OneDrive 범위의 경우 [OneDrive 인증 및 로그인](https://dev.onedrive.com/auth/msa_oauth.htm#authentication-scopes)을 참조하세요. 

> [!TIP]
> 필요에 따라 앱 (기본 전자 메일 주소를 사용 하 여 사용자 필드를 채우는)에 로그인 힌트 또는 로그인 환경에 관련 된 기타 특수 속성을 사용 하는 경우 **[WebTokenRequest.AppProperties](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core.webtokenrequest.appproperties#Windows_Security_Authentication_Web_Core_WebTokenRequest_AppProperties)** 속성에 나열 됩니다. 이렇게 하면 시스템에 캐시의 계정 불일치를 방지 하는 웹 계정을 캐싱의 경우 속성을 무시 합니다.

엔터프라이즈 앱을 개발하는 경우 AAD(Azure Active Directory) 인스턴스에 연결하고 일반 MSA 서비스 대신 Microsoft Graph API를 사용하려고 할 것입니다. 이 시나리오에서는 다음 코드를 대신 사용합니다. 

```csharp
private async void GetAadTokenAsync(WebAccountProviderCommand command)
{
    string clientId = "your_guid_here"; // Obtain your clientId from the Azure Portal
    WebTokenRequest request = new WebTokenRequest(provider, "User.Read", clientId);
    request.Properties.Add("resource", "https://graph.microsoft.com");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
}
```

이 문서의 나머지 부분에서는 MSA 시나리오를 계속 설명하지만 AAD에 대한 코드는 매우 유사합니다. GitHub의 전체 샘플을 포함하여 AAD/Graph에 대한 자세한 내용은 [Microsoft Graph 설명서](https://graph.microsoft.io/docs/platform/get-started)를 참조하세요.

## <a name="use-the-token"></a>토큰 사용

RequestTokenAsync 메서드는 요청의 결과를 포함하는 WebTokenRequestResult 개체를 반환합니다. 요청이 성공한 경우 토큰이 포함됩니다.  

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        string token = result.ResponseData[0].Token; 
    }
}
```

> [!NOTE]
> 토큰을 요청할 때 오류가 발생하면 1단계에서 설명한 대로 앱을 스토어에 연결했는지 확인합니다. 이 단계를 건너뛰면 앱은 토큰을 가져올 수 없습니다. 

토큰이 있으면 토큰을 사용하여 공급자의 API를 호출할 수 있습니다. 아래 코드에서는 [user info Microsoft Live API](https://msdn.microsoft.com/library/hh826533.aspx)를 호출하여 사용자에 대한 기본 정보를 얻고 UI에 표시합니다. 그러나 대부분의 경우 이렇게 얻은 토큰을 저장한 다음 이를 다른 메서드에 사용하는 것이 좋습니다.

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        string token = result.ResponseData[0].Token; 
        
        var restApi = new Uri(@"https://apis.live.net/v5.0/me?access_token=" + token);

        using (var client = new HttpClient())
        {
            var infoResult = await client.GetAsync(restApi);
            string content = await infoResult.Content.ReadAsStringAsync();

            var jsonObject = JsonObject.Parse(content);
            string id = jsonObject["id"].GetString();
            string name = jsonObject["name"].GetString();

            UserIdTextBlock.Text = "Id: " + id; 
            UserNameTextBlock.Text = "Name: " + name;
        }
    }
}
```

다양한 REST API를 호출하는 방법은 공급자마다 다릅니다. 토큰 사용 방법에 대한 자세한 내용은 공급자 API 설명서를 참조하세요. 

## <a name="store-the-account-for-future-use"></a>나중을 위해 계정 저장

토큰은 사용자에 대한 정보를 즉시 얻는 데 유용하지만 일반적으로 수명이 다양합니다. 예를 들어 MSA 토큰은 몇 시간 동안만 유효합니다. 다행히 토큰이 만료될 때마다 **AccountsSettingsPane**을 다시 표시할 필요가 없습니다. 사용자가 앱에 대한 권한을 부여하면 나중에 사용할 수 있게 사용자의 계정 정보를 저장할 수 있습니다. 

이렇게 하려면 **[WebAccount](https://docs.microsoft.com/uwp/api/windows.security.credentials.webaccount)** 클래스를 사용합니다. **WebAccount**는 토큰을 요청하는 데 사용한 동일한 메서드에 의해 반환됩니다.

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        WebAccount account = result.ResponseData[0].WebAccount; 
    }
}
```

**WebAccount** 인스턴스가 있으면 쉽게 저장할 수 있습니다. 다음 예제에서는 LocalSettings를 사용합니다. LocalSettings 및 사용자 데이터를 저장할 다른 메서드에 대한 자세한 내용은 [앱 설정 및 데이터 저장 및 검색](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)을 참조하세요.

```csharp
private async void StoreWebAccount(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values["CurrentUserProviderId"] = account.WebAccountProvider.Id;
    ApplicationData.Current.LocalSettings.Values["CurrentUserId"] = account.Id; 
}
```

그런 다음, 다음과 같은 비동기 메서드를 사용하여 저장된 **WebAccount**로 백그라운드에서 토큰을 얻으려고 시도할 수 있습니다.

```csharp
private async Task<string> GetTokenSilentlyAsync()
{
    string providerId = ApplicationData.Current.LocalSettings.Values["CurrentUserProviderId"]?.ToString();
    string accountId = ApplicationData.Current.LocalSettings.Values["CurrentUserId"]?.ToString();

    if (null == providerId || null == accountId)
    {
        return null; 
    }

    WebAccountProvider provider = await WebAuthenticationCoreManager.FindAccountProviderAsync(providerId);
    WebAccount account = await WebAuthenticationCoreManager.FindAccountAsync(provider, accountId);

    WebTokenRequest request = new WebTokenRequest(provider, "wl.basic");

    WebTokenRequestResult result = await WebAuthenticationCoreManager.GetTokenSilentlyAsync(request, account);
    if (result.ResponseStatus == WebTokenRequestStatus.UserInteractionRequired)
    {
        // Unable to get a token silently - you'll need to show the UI
        return null; 
    }
    else if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        // Success
        return result.ResponseData[0].Token;
    }
    else
    {
        // Other error 
        return null; 
    }
}
```

위의 메서드를 **AccountsSettingsPane**을 빌드하는 코드 앞에 두십시오. 토큰을 백그라운드에서 가져온 경우 창을 표시할 필요는 없습니다. 

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{
    string silentToken = await GetMsaTokenSilentlyAsync();

    if (silentToken != null)
    {
        // the token was obtained. store a reference to it or do something with it here.
    }
    else
    {
        // the token could not be obtained silently. Show the AccountsSettingsPane
        AccountsSettingsPane.Show();
    }
}
```

토큰을 자동으로 가져오는 것은 매우 간단하기 때문에 기존 토큰을 캐시하지 말고(언제든지 해당 토큰이 만료될 수 있음) 세션 간에 토큰을 새로 고쳐야 합니다.

> [!NOTE]
> 위 예제는 기본적인 성공 및 실패 사례만 제공합니다. 또한 앱은 비정상적인 시나리오(예: 사용자가 앱의 사용 권한 해지 또는 Windows에서 계정 제거)를 고려하고 적절히 처리해야 합니다.  

## <a name="remove-a-stored-account"></a>저장된 계정 제거

웹 계정을 유지하는 경우 사용자가 자신의 계정을 앱과 분리하는 것을 허용하려고 할 수 있습니다. 이렇게이 하면 이러한 수 효과적으로 "로그 아웃" 응용 프로그램의: 시작 시 사용자 계정 정보가 자동으로 로드 더 이상 됩니다. 이렇게 하려면 먼저 스토리지에서 저장된 계정 및 공급자 정보를 제거합니다. 그런 후 **[SignOutAsync](https://docs.microsoft.com/uwp/api/windows.security.credentials.webaccount.SignOutAsync)** 를 호출하여 캐시를 지우고 앱에 있을 수 있는 기존 토큰을 무효화합니다. 

```csharp
private async Task SignOutAccountAsync(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserProviderId");
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserId"); 
    account.SignOutAsync(); 
}
```

## <a name="add-providers-that-dont-support-webaccountmanager"></a>WebAccountManager를 지원하지 않는 공급자를 추가합니다.

서비스의 인증을 앱에 통합하려고 하지만 해당 서비스가 WebAccountManager(예: Google+ 또는 Twitter)를 지원하지 않을 경우 **AccountsSettingsPane**에 해당 공급자를 수동으로 추가할 수 있습니다. 이렇게 하려면 새 WebAccountProvider 개체를 만들고 고유한 이름 및 .png 아이콘을 제공한 다음 WebAccountProviderCommands 목록에 추가합니다. 일부 스텁 코드는 다음과 같습니다. 

 ```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 

    var twitterProvider = new WebAccountProvider("twitter", "Twitter", new Uri(@"ms-appx:///Assets/twitter-auth-icon.png")); 
    var twitterCmd = new WebAccountProviderCommand(twitterProvider, GetTwitterTokenAsync);
    e.WebAccountProviderCommands.Add(twitterCmd);   
    
    // other code here
}

private async void GetTwitterTokenAsync(WebAccountProviderCommand command)
{
    // Manually handle Twitter login here
}

```

> [!NOTE] 
> 이 코드를 사용하면 아이콘이 **AccountsSettingsPane**에 추가되기만 하며, 아이콘을 클릭해야만 지정한 메서드가 실행됩니다(이 경우 GetTwitterTokenAsync). 실제 인증을 처리하는 코드를 제공해야 합니다. 자세한 내용은 REST 서비스를 사용하여 인증하기 위한 도우미 메서드를 제공하는 (웹 인증 브로커)[web-authentication-broker]를 참조하세요. 

## <a name="add-a-custom-header"></a>사용자 지정 헤더 추가

아래와 같이 HeaderText 속성을 사용하여 계정 설정 창을 사용자 지정할 수 있습니다. 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 
    
    args.HeaderText = "MyAwesomeApp works best if you're signed in.";   
    
    // other code here
}
```

![계정 설정 창](images/tb-3.png)

헤더 텍스트를 복잡하게 만들면 느려지므로 간단하게 유지합니다. 로그인 프로세스가 복잡하고 자세한 정보를 표시해야 하는 경우 사용자 지정 링크를 사용하여 사용자를 별도 페이지로 연결합니다. 

## <a name="add-custom-links"></a>사용자 지정 링크 추가

지원되는 WebAccountProviders 아래에 나타나는 AccountsSettingsPane에 사용자 지정 명령을 추가할 수 있습니다. 사용자 지정 명령은 개인 정보 취급 방침 표시 또는 문제가 있는 사용자를 위한 지원 페이지 실행 등, 사용자 계정과 관련된 간단한 작업에 적합합니다. 

예를 들면 다음과 같습니다. 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 
    
    var settingsCmd = new SettingsCommand(
        "settings_privacy", 
        "Privacy policy", 
        async (x) => await Launcher.LaunchUriAsync(new Uri(@"https://privacy.microsoft.com/en-US/"))); 

    e.Commands.Add(settingsCmd); 
    
    // other code here
}
```

![계정 설정 창](images/tb-4.png)

이론적으로는 모든 경우에 설정 명령을 사용할 수 있습니다. 그러나 위에 설명된 것과 같은 계정과 관련된 직관적인 시나리오로 사용을 제한하는 것이 좋습니다. 

## <a name="see-also"></a>참고 항목

[Windows.Security.Authentication.Web.Core 네임스페이스](https://msdn.microsoft.com/library/windows/apps/windows.security.authentication.web.core.aspx)

[Windows.Security.Credentials 네임스페이스](https://msdn.microsoft.com/library/windows/apps/windows.security.credentials.aspx)

[AccountsSettingsPane 클래](https://msdn.microsoft.com/library/windows/apps/windows.ui.applicationsettings.accountssettingspane)

[웹 인증 브로커](web-authentication-broker.md)

[웹 계정 관리 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620621)

[Lunch Scheduler 앱](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
