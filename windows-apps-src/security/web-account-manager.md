---
title: 웹 계정 관리자
description: 이 문서에서는 AccountsSettingsPane를 사용 하 여 Windows 10 웹 계정 관리자 Api를 사용 하는 Microsoft 또는 Facebook과 같은 외부 id 공급자에 유니버설 Windows 플랫폼 (UWP) 앱을 연결 하는 방법을 설명 합니다.
ms.date: 12/06/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.assetid: ec9293a1-237d-47b4-bcde-18112586241a
ms.localizationpriority: medium
ms.openlocfilehash: 7cf4cfa4b87842cd7113b36220cdfdff69449a3a
ms.sourcegitcommit: 720413d2053c8d5c5b34d6873740be6e913a4857
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88846793"
---
# <a name="web-account-manager"></a>웹 계정 관리자

이 문서에서는 **[AccountsSettingsPane](https://docs.microsoft.com/uwp/api/Windows.UI.ApplicationSettings.AccountsSettingsPane)** 를 사용 하 여 Windows 10 웹 계정 관리자 api를 사용 하는 Microsoft 또는 Facebook과 같은 외부 id 공급자에 유니버설 WINDOWS 플랫폼 (UWP) 앱을 연결 하는 방법을 설명 합니다. 사용자의 권한을 요청 하 여 Microsoft 계정 사용 권한을 요청 하 고 액세스 토큰을 가져온 다음이를 사용 하 여 기본 작업 (예: 프로필 데이터 가져오기 또는 OneDrive 계정에 파일 업로드)을 수행 하는 방법을 알아봅니다. 이러한 단계는 웹 계정 관리자를 지 원하는 모든 id 공급자를 사용 하 여 사용자 권한 및 액세스를 가져오는 것과 비슷합니다.

> [!NOTE]
> 전체 코드 샘플은 [GitHub의 WebAccountManagement 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAccountManagement)을 참조 하세요.

## <a name="get-set-up"></a>설정

먼저 Visual Studio에서 비어 있는 새 솔루션을 만듭니다. 

둘째, id 공급자에 연결 하기 위해 앱을 스토어에 연결 해야 합니다. 이렇게 하려면 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **스토어**앱을 스토어에  >  **연결**을 선택 하 고 마법사의 지침을 따릅니다. 

셋째, 간단한 XAML 단추와 두 개의 텍스트 상자로 구성 된 매우 기본적인 UI를 만듭니다.

```XML
<StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
    <Button x:Name="LoginButton" Content="Log in" Click="LoginButton_Click" />
    <TextBlock x:Name="UserIdTextBlock"/>
    <TextBlock x:Name="UserNameTextBlock"/>
</StackPanel>
```

그리고 코드 숨김으로 단추에 연결 된 이벤트 처리기는 다음과 같습니다.

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{   
}
```

마지막으로 다음 네임 스페이스를 추가 하 여 나중에 참조 문제를 걱정 하지 않아도 됩니다. 

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

시스템은 id 공급자와 **AccountsSettingsPane**이라는 웹 계정을 관리 하기 위한 기본 제공 사용자 인터페이스를 제공 합니다. 다음과 같이 표시할 수 있습니다.

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{
    AccountsSettingsPane.Show(); 
}
```

앱을 실행 하 고 "로그인" 단추를 클릭 하면 빈 창이 표시 됩니다. 

![계정 설정 창](images/tb-1.png)

시스템은 UI 셸만 제공 하므로 창이 비어 있습니다. 개발자는 프로그래밍 방식으로 id 공급자를 사용 하 여 창을 채울 수 있습니다. 

> [!TIP]
> 필요에 따라 **[Show](https://docs.microsoft.com/uwp/api/windows.ui.applicationsettings.accountssettingspane.show#Windows_UI_ApplicationSettings_AccountsSettingsPane_Show)** 대신 **[ShowAddAccountAsync](https://docs.microsoft.com/uwp/api/windows.ui.applicationsettings.accountssettingspane.showaddaccountasync)** 를 사용 하 여 **[iasyncaction](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncAction)** 을 반환 하는 작업의 상태를 쿼리할 수 있습니다. 

## <a name="register-for-accountcommandsrequested"></a>AccountCommandsRequested에 등록

창에 명령을 추가 하려면 AccountCommandsRequested 이벤트 처리기를 등록 하는 것부터 시작 합니다. 그러면 사용자가 창을 볼 때 (예: XAML 단추 클릭) 빌드 논리를 실행 하 라는 메시지가 시스템에 표시 됩니다. 

코드 뒤에서 OnNavigatedTo 및 OnNavigatedFrom 이벤트를 재정의 하 고 다음 코드를 추가 합니다. 

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

사용자는 계정을 자주 조작 하지 않으므로 이러한 방식으로 이벤트 처리기를 등록 하 고 있음이 하면 메모리 누수가 방지 됩니다. 이러한 방식으로 사용자 지정 된 창은 "설정" 또는 "로그인" 페이지와 같이 사용자가 요청 하는 경우에만 메모리에 있습니다. 

## <a name="build-the-account-settings-pane"></a>계정 설정 창 만들기

BuildPaneAsync 메서드는 **AccountsSettingsPane** 가 표시 될 때마다 호출 됩니다. 여기서는 창에 표시 된 명령을 사용자 지정 하는 코드를 저장 합니다. 

지연을 가져와 시작 합니다. 이를 통해 시스템 빌드를 완료할 때까지 **AccountsSettingsPane** 표시를 지연 시킬 수 있습니다.

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    deferral.Complete(); 
}
```

그런 다음 WebAuthenticationCoreManager. FindAccountProviderAsync 메서드를 사용 하 여 공급자를 가져옵니다. 공급자의 URL은 공급자에 따라 다르며 공급자 설명서에서 확인할 수 있습니다. Microsoft 계정과 Azure Active Directory의 경우 "https \: //login.microsoft.com"입니다. 

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

또한 "소비자" 문자열을 선택적 *authority* 매개 변수에 전달 합니다. Microsoft에서 "소비자"에 대 한 두 가지 인증 유형인 MSA (Microsoft 계정)를 제공 하 고 "조직"에 대해 AAD (Azure Active Directory)를 제공 하기 때문입니다. "소비자" 기관은 MSA 옵션을 원하는 것으로 표시 합니다. 엔터프라이즈 앱을 개발 하는 경우 "조직" 문자열을 대신 사용 합니다.

마지막으로 다음과 같은 새 **[WebAccountProviderCommand](https://docs.microsoft.com/uwp/api/windows.ui.applicationsettings.webaccountprovidercommand)** 를 만들어 **AccountsSettingsPane** 에 공급자를 추가 합니다. 

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

새 **WebAccountProviderCommand** 에 전달 된 GetMsaToken 메서드는 아직 존재 하지 않습니다 (다음 단계에서 빌드). 지금은 빈 메서드로 자유롭게 추가할 수 있습니다.

위의 코드를 실행 하면 창이 다음과 같이 표시 됩니다. 

![계정 설정 창](images/tb-2.png)

### <a name="request-a-token"></a>토큰 요청

**AccountsSettingsPane**에 Microsoft 계정 옵션이 표시 되 면 사용자가이를 선택할 때 발생 하는 상황을 처리 해야 합니다. 사용자가 Microsoft 계정으로 로그인 할 때 발생 하는 GetMsaToken 메서드를 등록 했으므로 여기에서 토큰을 가져옵니다. 

토큰을 가져오려면 다음과 같이 RequestTokenAsync 메서드를 사용 합니다. 

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
}
```

이 예에서는 "wl" 문자열을 _범위_ 매개 변수에 전달 합니다. 범위는 특정 사용자에 게 제공 하는 서비스에서 요청 하는 정보의 유형을 나타냅니다. 특정 범위는 사용자의 기본 정보 (예: 이름 및 전자 메일 주소)에 대 한 액세스만 제공 하는 반면 다른 범위는 사용자의 사진이 나 전자 메일 받은 편지함과 같은 중요 한 정보에 대 한 액세스 권한을 부여할 수 있습니다. 일반적으로 앱은 함수를 구현 하는 데 필요한 최소 허용 범위를 사용 해야 합니다. 서비스 공급자는 서비스에 사용할 토큰을 가져오는 데 필요한 범위에 대 한 설명서를 제공 합니다. 

* Microsoft 365 및 Outlook.com 범위는 [Outlook REST API (버전 2.0) 사용](/previous-versions/office/office-365-api/api/version-2.0/use-outlook-rest-api)을 참조 하세요. 
* OneDrive 범위는 [onedrive 인증 및 로그인](https://dev.onedrive.com/auth/msa_oauth.htm#authentication-scopes)을 참조 하세요. 

> [!TIP]
> 필요에 따라 앱에서 로그인 힌트를 사용 하는 경우 (사용자 필드를 기본 전자 메일 주소로 채우기 위해) 또는 로그인 환경과 관련 된 다른 특별 한 속성을 사용 하는 경우 **[Webtokenrequest. AppProperties](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core.webtokenrequest.appproperties#Windows_Security_Authentication_Web_Core_WebTokenRequest_AppProperties)** 속성에 나열 합니다. 이렇게 하면 웹 계정을 캐시할 때 시스템이 속성을 무시 하므로 캐시의 계정 불일치를 방지할 수 있습니다.

엔터프라이즈 앱을 개발 하는 경우 AAD (Azure Active Directory) 인스턴스에 연결 하 고 일반 MSA 서비스 대신 Microsoft Graph API를 사용 하는 것이 좋습니다. 이 시나리오에서는 다음 코드를 대신 사용 합니다. 

```csharp
private async void GetAadTokenAsync(WebAccountProviderCommand command)
{
    string clientId = "your_guid_here"; // Obtain your clientId from the Azure Portal
    WebTokenRequest request = new WebTokenRequest(provider, "User.Read", clientId);
    request.Properties.Add("resource", "https://graph.microsoft.com");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
}
```

이 문서의 나머지 부분에서는 MSA 시나리오를 계속 설명 하지만 AAD에 대 한 코드는 매우 유사 합니다. GitHub에 대 한 전체 샘플을 포함 하 여 AAD/Graph에 대 한 자세한 내용은 [Microsoft Graph 설명서](https://developer.microsoft.com/graph)를 참조 하세요.

## <a name="use-the-token"></a>토큰 사용

RequestTokenAsync 메서드는 요청 결과를 포함 하는 WebTokenRequestResult 개체를 반환 합니다. 요청이 성공적으로 완료 되 면 토큰을 포함 합니다.  

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
> 토큰을 요청할 때 오류가 발생 하는 경우 1 단계에 설명 된 대로 스토어와 앱을 연결 했는지 확인 합니다. 이 단계를 건너뛴 경우 앱에서 토큰을 가져올 수 없습니다. 

토큰이 있으면 공급자의 API를 호출 하는 데 사용할 수 있습니다. 아래 코드에서는 사용자 [정보 Microsoft LIVE API](https://docs.microsoft.com/office/) 를 호출 하 여 사용자에 대 한 기본 정보를 얻고 UI에 표시 합니다. 그러나 대부분의 경우 토큰을 가져온 후 별도의 메서드에서 사용 하는 것이 좋습니다.

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

여러 REST Api를 호출 하는 방법은 공급자 마다 다릅니다. 토큰을 사용 하는 방법에 대 한 자세한 내용은 공급자의 API 설명서를 참조 하세요. 

## <a name="store-the-account-for-future-use"></a>나중에 사용 하기 위해 계정 저장

토큰은 사용자에 대 한 정보를 즉시 가져오는 데 유용 하지만 일반적으로 다양 한 lifespans 토큰을 갖고 있습니다. 예를 들어 몇 시간 동안만 유효 합니다. 다행히 토큰이 만료 될 때마다 **AccountsSettingsPane** 를 다시 표시할 필요가 없습니다. 사용자가 앱을 한 번 인증 한 후에는 나중에 사용할 수 있도록 사용자 계정 정보를 저장할 수 있습니다. 

이렇게 하려면 **[WebAccount](https://docs.microsoft.com/uwp/api/windows.security.credentials.webaccount)** 클래스를 사용 합니다. **WebAccount** 는 토큰을 요청 하는 데 사용한 것과 동일한 메서드에서 반환 됩니다.

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

**WebAccount** 인스턴스를 만든 후에는 쉽게 저장할 수 있습니다. 다음 예제에서는 LocalSettings를 사용 합니다. LocalSettings 및 기타 메서드를 사용 하 여 사용자 데이터를 저장 하는 방법에 대 한 자세한 내용은 [앱 설정 및 데이터 저장 및 검색](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)을 참조 하세요.

```csharp
private async void StoreWebAccount(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values["CurrentUserProviderId"] = account.WebAccountProvider.Id;
    ApplicationData.Current.LocalSettings.Values["CurrentUserId"] = account.Id; 
}
```

그런 다음, 다음과 같은 비동기 메서드를 사용 하 여 저장 된 **WebAccount**를 사용 하 여 백그라운드에서 토큰을 가져오려고 시도할 수 있습니다.

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

위의 메서드를 **AccountsSettingsPane**을 빌드하는 코드 바로 앞에 놓습니다. 토큰을 백그라운드로 가져오는 경우 창을 표시할 필요가 없습니다. 

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

토큰을 자동으로 가져오는 것은 매우 간단 하기 때문에이 프로세스를 사용 하 여 기존 토큰을 캐시 하지 않고 세션 간에 토큰을 새로 고쳐야 합니다 (해당 토큰은 언제 든 지 만료 될 수 있으므로).

> [!NOTE]
> 위의 예제에서는 기본 성공 및 실패 사례에 대해서만 다룹니다. 앱은 사용자가 앱의 권한을 취소 하거나 Windows에서 계정을 제거 하는 등의 특수 시나리오를 고려해 야 합니다 (예:).  

## <a name="remove-a-stored-account"></a>저장 된 계정 제거

웹 계정을 유지 하는 경우 사용자에 게 앱과의 계정을 분리 하는 기능을 제공 하는 것이 좋습니다. 이러한 방식으로 앱의 "로그 아웃"을 효과적으로 "로그 아웃" 할 수 있습니다. 해당 계정 정보는 시작 시 자동으로 로드 되지 않습니다. 이렇게 하려면 먼저 저장 된 계정 및 공급자 정보를 저장소에서 제거 합니다. 그런 다음 **[SignOutAsync](https://docs.microsoft.com/uwp/api/windows.security.credentials.webaccount.SignOutAsync)** 를 호출 하 여 캐시를 지우고 앱에 포함 될 수 있는 기존 토큰을 무효화 합니다. 

```csharp
private async Task SignOutAccountAsync(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserProviderId");
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserId"); 
    account.SignOutAsync(); 
}
```

## <a name="add-providers-that-dont-support-webaccountmanager"></a>WebAccountManager을 지원 하지 않는 공급자 추가

서비스의 인증을 앱에 통합 하려고 하지만 해당 서비스가 WebAccountManager-Google + 또는 Twitter를 지원 하지 않는 경우 (예:) 해당 공급자를 **AccountsSettingsPane**에 수동으로 추가할 수 있습니다. 이렇게 하려면 새 WebAccountProvider 개체를 만들고 고유한 이름과 .png 아이콘을 입력 한 다음 WebAccountProviderCommands 목록에 추가 합니다. 다음은 몇 가지 스텁 코드입니다. 

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
> 이는 **AccountsSettingsPane** 에 아이콘만 추가 하 고 아이콘을 클릭할 때 지정한 메서드를 실행 합니다 (이 경우 GetTwitterTokenAsync). 실제 인증을 처리 하는 코드를 제공 해야 합니다. 자세한 내용은 REST 서비스를 사용 하 여 인증 하기 위한 도우미 메서드를 제공 하는 [웹 인증 브로커](web-authentication-broker.md)를 참조 하세요. 

## <a name="add-a-custom-header"></a>사용자 지정 헤더 추가

다음과 같이 HeaderText 속성을 사용 하 여 계정 설정 창을 사용자 지정할 수 있습니다. 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 
    
    args.HeaderText = "MyAwesomeApp works best if you're signed in.";   
    
    // other code here
}
```

![계정 설정 창](images/tb-3.png)

머리글 텍스트를 사용 하 여 과도 하 게 이동 하지 않습니다. 짧고 간단 하 게 유지 합니다. 로그인 프로세스가 복잡 하 고 자세한 정보를 표시 해야 하는 경우 사용자 지정 링크를 사용 하 여 별도의 페이지에 사용자를 연결 합니다. 

## <a name="add-custom-links"></a>사용자 지정 링크 추가

지원 되는 WebAccountProviders 아래 링크로 표시 되는 AccountsSettingsPane에 사용자 지정 명령을 추가할 수 있습니다. 사용자 지정 명령은 개인 정보 취급 방침을 표시 하거나 사용자에 게 문제를 발생 시킬 수 있는 지원 페이지를 시작 하는 등의 사용자 계정과 관련 된 간단한 작업에 유용 합니다. 

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

이론적으로는 모든 항목에 대해 설정 명령을 사용할 수 있습니다. 그러나 위에서 설명한 것과 같은 직관적인 계정 관련 시나리오로 사용을 제한 하는 것이 좋습니다. 

## <a name="see-also"></a>참고 항목

[Windows... i a.](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core)

[Windows. 보안 자격 증명 네임 스페이스](https://docs.microsoft.com/uwp/api/windows.security.credentials)

[AccountsSettingsPane 클래스](https://docs.microsoft.com/uwp/api/windows.ui.applicationsettings.accountssettingspane)

[웹 인증 브로커](web-authentication-broker.md)

[웹 계정 관리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAccountManagement)

[점심 스케줄러 앱](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
