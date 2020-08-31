---
title: Windows Hello 로그인 앱 만들기
description: 기존 사용자 이름 및 암호 인증 시스템 대신 Windows Hello를 사용 하는 Windows 10 UWP (유니버설 Windows 플랫폼) 앱을 만드는 방법에 대 한 전체 연습은 1 부입니다.
ms.assetid: A9E11694-A7F5-4E27-95EC-889307E0C0EF
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: dcfa3702f43907453da0769cf5c6430cfbcae6d5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157957"
---
# <a name="create-a-windows-hello-login-app"></a>Windows Hello 로그인 앱 만들기

기존 사용자 이름 및 암호 인증 시스템 대신 Windows Hello를 사용 하는 Windows 10 UWP (유니버설 Windows 플랫폼) 앱을 만드는 방법에 대 한 전체 연습은 1 부입니다. 앱은 로그인에 대 한 사용자 이름을 사용 하 고 각 계정에 대 한 Hello 키를 만듭니다. 이러한 계정은 Windows Hello 구성에서 Windows 설정에 설정 된 PIN으로 보호 됩니다.

이 연습은 앱 빌드 및 백 엔드 서비스 연결의 두 부분으로 나뉩니다. 이 문서를 완료 한 후에는 2 부: [Windows Hello 로그인 서비스](microsoft-passport-login-auth-service.md)를 계속 진행 하세요.

시작 하기 [전에 windows hello 개요를](microsoft-passport.md) 참조 하 여 windows hello가 작동 하는 방식을 일반적으로 이해 해야 합니다.

## <a name="get-started"></a>시작


이 프로젝트를 빌드하기 위해 c # 및 XAML에 대 한 몇 가지 환경이 필요 합니다. 또한 Windows 10 컴퓨터에서 Visual Studio 2015 (Community Edition 이상) 또는 이후 버전의 Visual Studio를 사용 해야 합니다. Visual Studio 2015는 필요한 최소 버전 이지만 최신 개발자 및 보안 업데이트를 위해 최신 버전의 Visual Studio를 사용 하는 것이 좋습니다.

-   Visual Studio를 열고 파일 > 새 > 프로젝트를 선택 합니다.
-   그러면 "새 프로젝트" 창이 열립니다. 템플릿 탐색 > Visual c #
-   비어 있는 앱 (유니버설 Windows)을 선택 하 고 응용 프로그램 이름을 "PassportLogin"로 입력 합니다.
-   새 응용 프로그램을 빌드 및 실행 (F5) 하면 화면에 빈 창이 표시 됩니다. 애플리케이션을 닫습니다.

![Windows Hello 새 프로젝트](images/passport-login-1.png)

## <a name="exercise-1-login-with-microsoft-passport"></a>연습 1: Microsoft Passport 로그인


이 연습에서는 컴퓨터에 Windows Hello가 설정 되어 있는지 확인 하는 방법 및 Windows Hello를 사용 하 여 계정에 로그인 하는 방법을 알아봅니다.

-   새 프로젝트에서 솔루션에 "Views" 라는 새 폴더를 만듭니다. 이 폴더에는이 샘플에서 탐색 되는 페이지가 포함 됩니다. 솔루션 탐색기에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 추가 > 새 폴더를 선택한 다음 폴더 이름을 뷰로 바꿉니다.

    ![Windows Hello 폴더 추가](images/passport-login-2.png)

-   새 보기 폴더를 마우스 오른쪽 단추로 클릭 하 고 추가 > 새 항목을 선택 하 고 빈 페이지를 선택 합니다. 이 페이지의 이름을 "Login. .xaml"으로 합니다.

    ![Windows Hello 빈 페이지 추가](images/passport-login-3.png)

-   새 로그인 페이지의 사용자 인터페이스를 정의 하려면 다음 XAML을 추가 합니다. 이 XAML은 다음 자식을 정렬 하는 StackPanel을 정의 합니다.

    -   제목이 포함 될 TextBlock입니다.
    -   오류 메시지에 대 한 TextBlock
    -   입력할 사용자 이름을 입력할 텍스트 상자입니다.
    -   단추를 클릭 하 여 등록 페이지로 이동 합니다.
    -   TextBlock-Windows Hello의 상태를 포함 합니다.
    -   백 엔드 또는 구성 된 사용자가 없기 때문에 로그인 페이지를 설명 하는 TextBlock

    ```xml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <StackPanel Orientation="Vertical">
        <TextBlock Text="Login" FontSize="36" Margin="4" TextAlignment="Center"/>
        <TextBlock x:Name="ErrorMessage" Text="" FontSize="20" Margin="4" Foreground="Red" TextAlignment="Center"/>
        <TextBlock Text="Enter your username below" Margin="0,0,0,20"
                   TextWrapping="Wrap" Width="300"
                   TextAlignment="Center" VerticalAlignment="Center" FontSize="16"/>
        <TextBox x:Name="UsernameTextBox" Margin="4" Width="250"/>
        <Button x:Name="PassportSignInButton" Content="Login" Background="DodgerBlue" Foreground="White"
            Click="PassportSignInButton_Click" Width="80" HorizontalAlignment="Center" Margin="0,20"/>
        <TextBlock Text="Don't have an account?"
                    TextAlignment="Center" VerticalAlignment="Center" FontSize="16"/>
        <TextBlock x:Name="RegisterButtonTextBlock" Text="Register now"
                   PointerPressed="RegisterButtonTextBlock_OnPointerPressed"
                   Foreground="DodgerBlue"
                   TextAlignment="Center" VerticalAlignment="Center" FontSize="16"/>
        <Border x:Name="PassportStatus" Background="#22B14C"
                   Margin="0,20" Height="100" >
          <TextBlock x:Name="PassportStatusText" Text="Microsoft Passport is ready to use!"
                 Margin="4" TextAlignment="Center" VerticalAlignment="Center" FontSize="20"/>
        </Border>
        <TextBlock x:Name="LoginExplaination" FontSize="24" TextAlignment="Center" TextWrapping="Wrap" 
            Text="Please Note: To demonstrate a login, validation will only occur using the default username 'sampleUsername'"/>
      </StackPanel>
    </Grid>
    ```

-   솔루션 빌드를 가져오기 위해 코드 숨김으로 몇 가지 메서드를 추가 해야 합니다. F7 키를 누르거나 솔루션 탐색기를 사용 하 여 Login.xaml.cs을 가져옵니다. 다음 두 이벤트 메서드를 추가 하 여 로그인을 처리 하 고 이벤트를 등록 합니다. 지금은 이러한 메서드는 ErrorMessage 텍스트를 빈 문자열로 설정 합니다.

    ```cs
    namespace PassportLogin.Views
    {
        public sealed partial class Login : Page
        {
            public Login()
            {
                this.InitializeComponent();
            }
     
            private void PassportSignInButton_Click(object sender, RoutedEventArgs e)
            {
                ErrorMessage.Text = "";
            }
            private void RegisterButtonTextBlock_OnPointerPressed(object sender, PointerRoutedEventArgs e)
            {
                ErrorMessage.Text = "";
            }
        }
    }
    ```

-   로그인 페이지를 렌더링 하려면 mainpage 코드를 편집 하 여 MainPage가 로드 될 때 로그인 페이지로 이동 합니다. MainPage.xaml.cs 파일을 엽니다. 솔루션 탐색기에서 MainPage.xaml.cs를 두 번 클릭 합니다. 이 위치를 찾을 수 없는 경우 MainPage 옆의 작은 화살표를 클릭 하 여 코드 숨김을 표시 합니다. 로그인 페이지로 이동 하는 로드 된 이벤트 처리기 메서드를 만듭니다. 뷰 네임 스페이스에 대 한 참조를 추가 해야 합니다.

    ```cs
    using PassportLogin.Views;
     
    namespace PassportLogin
    {
        public sealed partial class MainPage : Page
        {
            public MainPage()
            {
                this.InitializeComponent();
                Loaded += MainPage_Loaded;
            }
     
            private void MainPage_Loaded(object sender, RoutedEventArgs e)
            {
                Frame.Navigate(typeof(Login));
            }
        }
    }
    ```

-   로그인 페이지에서 OnNavigatedTo 이벤트를 처리 하 여이 컴퓨터에서 Windows Hello를 사용할 수 있는지 확인 해야 합니다. Login.xaml.cs에서 다음을 구현 합니다. MicrosoftPassportHelper 개체가 오류를 플래그로 표시 하는 것을 알 수 있습니다. 이것은 아직 구현 하지 않았기 때문입니다.

    ```cs
    public sealed partial class Login : Page
    {
        public Login()
        {
            this.InitializeComponent();
        }
     
        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            // Check Microsoft Passport is setup and available on this machine
            if (await MicrosoftPassportHelper.MicrosoftPassportAvailableCheckAsync())
            {
            }
            else
            {
                // Microsoft Passport is not setup so inform the user
                PassportStatus.Background = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 50, 170, 207));
                PassportStatusText.Text = "Microsoft Passport is not setup!\n" + 
                    "Please go to Windows Settings and set up a PIN to use it.";
                PassportSignInButton.IsEnabled = false;
            }
        }
    }
    ```

-   MicrosoftPassportHelper 클래스를 만들려면 솔루션 PassportLogin (유니버설 Windows)을 마우스 오른쪽 단추로 클릭 하 고 추가 > 새 폴더를 클릭 합니다. 이 폴더의 이름을 유틸리티로 합니다.

    ![passport 도우미 클래스 만들기](images/passport-login-5.png)

-   유틸리티 폴더를 마우스 오른쪽 단추로 클릭 하 고 > 클래스 추가를 클릭 합니다. 이 클래스의 이름을 "MicrosoftPassportHelper.cs"로 합니다.
-   MicrosoftPassportHelper의 클래스 정의를 public static으로 변경 하 고 Windows Hello를 사용할 준비가 되었는지 여부를 사용자에 게 알리는 다음 메서드를 추가 합니다. 필요한 네임 스페이스를 추가 해야 합니다.

    ```cs
    using System;
    using System.Diagnostics;
    using System.Threading.Tasks;
    using Windows.Security.Credentials;
     
    namespace PassportLogin.Utils
    {
        public static class MicrosoftPassportHelper
        {
            /// <summary>
            /// Checks to see if Passport is ready to be used.
            /// 
            /// Passport has dependencies on:
            ///     1. Having a connected Microsoft Account
            ///     2. Having a Windows PIN set up for that _account on the local machine
            /// </summary>
            public static async Task<bool> MicrosoftPassportAvailableCheckAsync()
            {
                bool keyCredentialAvailable = await KeyCredentialManager.IsSupportedAsync();
                if (keyCredentialAvailable == false)
                {
                    // Key credential is not enabled yet as user 
                    // needs to connect to a Microsoft Account and select a PIN in the connecting flow.
                    Debug.WriteLine("Microsoft Passport is not setup!\nPlease go to Windows Settings and set up a PIN to use it.");
                    return false;
                }
     
                return true;
            }
        }
    }
    ```

-   Login.xaml.cs에서 유틸리티 네임 스페이스에 대 한 참조를 추가 합니다. 이렇게 하면 OnNavigatedTo 메서드에서 오류가 해결 됩니다.

    ```cs
    using PassportLogin.Utils;
    ```

-   응용 프로그램을 빌드하고 실행 합니다 (F5). 로그인 페이지로 이동 되며 Hello를 사용할 준비가 되 면 Windows Hello 배너가 표시 됩니다. 컴퓨터의 Windows Hello 상태를 나타내는 녹색 또는 파란색 배너가 표시 됩니다.

    ![Windows Hello 로그인 화면이 준비 됨](images/passport-login-6.png)

    ![Windows Hello 로그인 화면이 설정 되지 않음](images/passport-login-7.png)

-   다음에 수행 해야 할 작업은 로그인에 대 한 논리를 작성 하는 것입니다. "모델" 이라는 새 폴더를 만듭니다.
-   모델 폴더에서 "Account.cs" 라는 새 클래스를 만듭니다. 이 클래스는 계정 모델 역할을 합니다. 이 샘플에는 사용자 이름만 포함 됩니다. 클래스 정의를 public으로 변경 하 고 Username 속성을 추가 합니다.
    
    ```cs
    namespace PassportLogin.Models
    {
        public class Account
        {
            public string Username { get; set; }
        }
    }
    ```

-   계정을 처리 하는 방법이 필요 합니다. 서버 또는 데이터베이스가 없기 때문에 랩에 대 한이 실습에서는 사용자 목록이 로컬로 저장 되 고 로드 됩니다. 유틸리티 폴더를 마우스 오른쪽 단추로 클릭 하 고 "AccountHelper.cs" 라는 새 클래스를 추가 합니다. 클래스 정의를 public static으로 변경 합니다. AccountHelper은 계정 목록을 로컬에 저장 하 고 로드 하는 데 필요한 모든 메서드를 포함 하는 정적 클래스입니다. XmlSerializer를 사용 하 여 저장 및 로드 작업을 수행할 수 있습니다. 또한 저장 한 파일과 저장 위치를 잊지 않아도 됩니다.
    
    ```cs
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Text;
    using System.Threading.Tasks;
    using System.Xml.Serialization;
    using Windows.Storage;
    using PassportLogin.Models;

    namespace PassportLogin.Utils
    {
        public static class AccountHelper
        {
            // In the real world this would not be needed as there would be a server implemented that would host a user account database.
            // For this tutorial we will just be storing accounts locally.
            private const string USER_ACCOUNT_LIST_FILE_NAME = "accountlist.txt";
            private static string _accountListPath = Path.Combine(ApplicationData.Current.LocalFolder.Path, USER_ACCOUNT_LIST_FILE_NAME);
            public static List<Account> AccountList = new List<Account>();
     
            /// <summary>
            /// Create and save a useraccount list file. (Updating the old one)
            /// </summary>
            private static async void SaveAccountListAsync()
            {
                string accountsXml = SerializeAccountListToXml();
     
                if (File.Exists(_accountListPath))
                {
                    StorageFile accountsFile = await StorageFile.GetFileFromPathAsync(_accountListPath);
                    await FileIO.WriteTextAsync(accountsFile, accountsXml);
                }
                else
                {
                    StorageFile accountsFile = await ApplicationData.Current.LocalFolder.CreateFileAsync(USER_ACCOUNT_LIST_FILE_NAME);
                    await FileIO.WriteTextAsync(accountsFile, accountsXml);
                }
            }
     
            /// <summary>
            /// Gets the useraccount list file and deserializes it from XML to a list of useraccount objects.
            /// </summary>
            /// <returns>List of useraccount objects</returns>
            public static async Task<List<Account>> LoadAccountListAsync()
            {
                if (File.Exists(_accountListPath))
                {
                    StorageFile accountsFile = await StorageFile.GetFileFromPathAsync(_accountListPath);
     
                    string accountsXml = await FileIO.ReadTextAsync(accountsFile);
                    DeserializeXmlToAccountList(accountsXml);
                }
     
                return AccountList;
            }
     
            /// <summary>
            /// Uses the local list of accounts and returns an XML formatted string representing the list
            /// </summary>
            /// <returns>XML formatted list of accounts</returns>
            public static string SerializeAccountListToXml()
            {
                XmlSerializer xmlizer = new XmlSerializer(typeof(List<Account>));
                StringWriter writer = new StringWriter();
                xmlizer.Serialize(writer, AccountList);
     
                return writer.ToString();
            }
     
            /// <summary>
            /// Takes an XML formatted string representing a list of accounts and returns a list object of accounts
            /// </summary>
            /// <param name="listAsXml">XML formatted list of accounts</param>
            /// <returns>List object of accounts</returns>
            public static List<Account> DeserializeXmlToAccountList(string listAsXml)
            {
                XmlSerializer xmlizer = new XmlSerializer(typeof(List<Account>));
                TextReader textreader = new StreamReader(new MemoryStream(Encoding.UTF8.GetBytes(listAsXml)));
     
                return AccountList = (xmlizer.Deserialize(textreader)) as List<Account>;
            }
        }
    }
    ```

-   그런 다음 계정 로컬 목록에서 계정을 추가 하 고 제거 하는 방법을 구현 합니다. 이러한 작업은 각각 목록을 저장 합니다. 이 실습에 필요한 최종 방법은 유효성 검사 방법입니다. 인증 서버 또는 사용자 데이터베이스는 없으므로 하드 코드 된 단일 사용자에 대해 유효성을 검사 합니다. 이러한 메서드는 AccountHelper 클래스에 추가 해야 합니다.
    
    ```cs
    public static Account AddAccount(string username)
            {
                // Create a new account with the username
                Account account = new Account() { Username = username };
                // Add it to the local list of accounts
                AccountList.Add(account);
                // SaveAccountList and return the account
                SaveAccountListAsync();
                return account;
            }
     
            public static void RemoveAccount(Account account)
            {
                // Remove the account from the accounts list
                AccountList.Remove(account);
                // Re save the updated list
                SaveAccountListAsync();
            }
     
            public static bool ValidateAccountCredentials(string username)
            {
                // In the real world, this method would call the server to authenticate that the account exists and is valid.
                // For this tutorial however we will just have a existing sample user that is just "sampleUsername"
                // If the username is null or does not match "sampleUsername" it will fail validation. In which case the user should register a new passport user
     
                if (string.IsNullOrEmpty(username))
                {
                    return false;
                }
     
                if (!string.Equals(username, "sampleUsername"))
                {
                    return false;
                }
     
                return true;
            }
    ```

-   다음으로 수행 해야 하는 작업은 사용자의 로그인 요청을 처리 하는 것입니다. Login.xaml.cs에서 현재 계정 로그인을 보유 하는 새 private 변수를 만듭니다. 그런 다음 새 메서드 호출 SignInPassport을 추가 합니다. 그러면 AccountHelper 자격 증명 메서드를 사용 하 여 계정 자격 증명의 유효성을 검사 합니다. 입력 한 사용자 이름이 이전 단계에서 설정한 하드 코딩 된 문자열 값과 같으면이 메서드는 부울 값을 반환 합니다. 이 샘플의 하드 코드 된 값은 "sampleUsername"입니다.

    ```cs
    using PassportLogin.Models;
    using PassportLogin.Utils;
    using System.Diagnostics;
     
    namespace PassportLogin.Views
    {
        public sealed partial class Login : Page
        {
            private Account _account;
     
            public Login()
            {
                this.InitializeComponent();
            }
     
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // Check Microsoft Passport is setup and available on this machine
                if (await MicrosoftPassportHelper.MicrosoftPassportAvailableCheckAsync())
                {
                }
                else
                {
                    // Microsoft Passport is not setup so inform the user
                    PassportStatus.Background = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 50, 170, 207));
                    PassportStatusText.Text = "Microsoft Passport is not setup!\nPlease go to Windows Settings and set up a PIN to use it.";
                    PassportSignInButton.IsEnabled = false;
                }
            }
     
            private void PassportSignInButton_Click(object sender, RoutedEventArgs e)
            {
                ErrorMessage.Text = "";
                SignInPassport();
            }
     
            private void RegisterButtonTextBlock_OnPointerPressed(object sender, PointerRoutedEventArgs e)
            {
                ErrorMessage.Text = "";
            }
     
            private async void SignInPassport()
            {
                if (AccountHelper.ValidateAccountCredentials(UsernameTextBox.Text))
                {
                    // Create and add a new local account
                    _account = AccountHelper.AddAccount(UsernameTextBox.Text);
                    Debug.WriteLine("Successfully signed in with traditional credentials and created local account instance!");
     
                    //if (await MicrosoftPassportHelper.CreatePassportKeyAsync(UsernameTextBox.Text))
                    //{
                    //    Debug.WriteLine("Successfully signed in with Microsoft Passport!");
                    //}
                }
                else
                {
                    ErrorMessage.Text = "Invalid Credentials";
                }
            }
        }
    }
    ```

-   MicrosoftPassportHelper에서 메서드를 참조 하는 주석 처리 된 코드를 발견 했을 수 있습니다. MicrosoftPassportHelper.cs에서 CreatePassportKeyAsync 라는 새 메서드를 추가 합니다. 이 메서드는 [**Keycredentialmanager**](/uwp/api/Windows.Security.Credentials.KeyCredentialManager)의 WINDOWS Hello API를 사용 합니다. [**Requestcreateasync**](/previous-versions/windows/dn973048(v=win.10)) 를 호출 하면 *accountId* 및 로컬 컴퓨터와 관련 된 Passport 키가 생성 됩니다. 실제 시나리오에서이를 구현 하는 데 관심이 있는 경우 switch 문의 주석을 메모해 두십시오.

    ```cs
    /// <summary>
    /// Creates a Passport key on the machine using the _account id passed.
    /// </summary>
    /// <param name="accountId">The _account id associated with the _account that we are enrolling into Passport</param>
    /// <returns>Boolean representing if creating the Passport key succeeded</returns>
    public static async Task<bool> CreatePassportKeyAsync(string accountId)
    {
        KeyCredentialRetrievalResult keyCreationResult = await KeyCredentialManager.RequestCreateAsync(accountId, KeyCredentialCreationOption.ReplaceExisting);

        switch (keyCreationResult.Status)
        {
            case KeyCredentialStatus.Success:
                Debug.WriteLine("Successfully made key");

                // In the real world authentication would take place on a server.
                // So every time a user migrates or creates a new Microsoft Passport account Passport details should be pushed to the server.
                // The details that would be pushed to the server include:
                // The public key, keyAttesation if available, 
                // certificate chain for attestation endorsement key if available,  
                // status code of key attestation result: keyAttestationIncluded or 
                // keyAttestationCanBeRetrievedLater and keyAttestationRetryType
                // As this sample has no concept of a server it will be skipped for now
                // for information on how to do this refer to the second Passport sample

                //For this sample just return true
                return true;
            case KeyCredentialStatus.UserCanceled:
                Debug.WriteLine("User cancelled sign-in process.");
                break;
            case KeyCredentialStatus.NotFound:
                // User needs to setup Microsoft Passport
                Debug.WriteLine("Microsoft Passport is not setup!\nPlease go to Windows Settings and set up a PIN to use it.");
                break;
            default:
                break;
        }

        return false;
    }
    ```

-   이제 CreatePassportKeyAsync 메서드를 만들었으므로 Login.xaml.cs 파일로 돌아가서 SignInPassport 메서드 내에서 코드의 주석 처리를 제거 합니다.

    ```cs
    private async void SignInPassport()
    {
        if (AccountHelper.ValidateAccountCredentials(UsernameTextBox.Text))
        {
            //Create and add a new local account
            _account = AccountHelper.AddAccount(UsernameTextBox.Text);
            Debug.WriteLine("Successfully signed in with traditional credentials and created local account instance!");

            if (await MicrosoftPassportHelper.CreatePassportKeyAsync(UsernameTextBox.Text))
            {
                Debug.WriteLine("Successfully signed in with Microsoft Passport!");
            }
        }
        else
        {
            ErrorMessage.Text = "Invalid Credentials";
        }
    }
    ```

-   애플리케이션을 빌드 및 실행합니다. 로그인 페이지가 표시 됩니다. "SampleUsername"를 입력 하 고 로그인을 클릭 합니다. PIN을 입력 하 라는 Windows Hello 프롬프트가 표시 됩니다. PIN을 올바르게 입력 하면 CreatePassportKeyAsync 메서드가 Windows Hello 키를 만들 수 있습니다. 출력 창을 모니터링 하 여 성공을 나타내는 메시지가 표시 되는지 확인 합니다.

    ![Windows Hello 로그인 pin 프롬프트](images/passport-login-8.png)

## <a name="exercise-2-welcome-and-user-selection-pages"></a>연습 2: 시작 및 사용자 선택 페이지


이 연습에서는 이전 연습을 계속 진행 합니다. 사용자가 성공적으로 로그인 하면 해당 사용자가 자신의 계정을 로그 아웃 하거나 삭제할 수 있는 시작 페이지로 이동 해야 합니다. Windows Hello에서 모든 컴퓨터에 대 한 키를 만들기 때문에 해당 컴퓨터에서 로그인 한 모든 사용자를 표시 하는 사용자 선택 화면을 만들 수 있습니다. 사용자는 이러한 계정 중 하나를 선택 하 고, 컴퓨터에 액세스 하기 위해 이미 인증 된 암호를 다시 입력 하지 않고도 시작 화면으로 바로 이동할 수 있습니다.

-   Views 폴더에 ".xaml" 이라는 새 빈 페이지를 추가 합니다. 다음 XAML을 추가 하 여 사용자 인터페이스를 완료 합니다. 그러면 제목, 로그인 한 사용자 이름 및 두 개의 단추가 표시 됩니다. 단추 중 하나는 나중에 만들 사용자 목록으로 다시 이동 하 고 다른 단추는이 사용자를 잊어버린 경우 처리 합니다.

    ```xml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <StackPanel Orientation="Vertical">
        <TextBlock x:Name="Title" Text="Welcome" FontSize="40" TextAlignment="Center"/>
        <TextBlock x:Name="UserNameText" FontSize="28" TextAlignment="Center" Foreground="Black"/>

        <Button x:Name="BackToUserListButton" Content="Back to User List" Click="Button_Restart_Click"
                HorizontalAlignment="Center" Margin="0,20" Foreground="White" Background="DodgerBlue"/>

        <Button x:Name="ForgetButton" Content="Forget Me" Click="Button_Forget_User_Click"
                Foreground="White"
                Background="Gray"
                HorizontalAlignment="Center"/>
      </StackPanel>
    </Grid>
    ```

-   Welcome.xaml.cs 코드 파일에서 로그인 한 계정을 보유할 새 private 변수를 추가 합니다. OnNavigateTo 이벤트를 재정의 하는 메서드를 구현 해야 합니다. 이렇게 하면 시작 페이지에 전달 되는 계정이 저장 됩니다. 또한 XAML에 정의 된 두 단추에 대해 click 이벤트를 구현 해야 합니다. 모델 및 유틸리티 폴더에 대 한 참조가 필요 합니다.

    ```cs
    using PassportLogin.Models;
    using PassportLogin.Utils;
    using System.Diagnostics;
     
    namespace PassportLogin.Views
    {
        public sealed partial class Welcome : Page
        {
            private Account _activeAccount;
     
            public Welcome()
            {
                InitializeComponent();
            }
     
            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
                _activeAccount = (Account)e.Parameter;
                if (_activeAccount != null)
                {
                    UserNameText.Text = _activeAccount.Username;
                }
            }
     
            private void Button_Restart_Click(object sender, RoutedEventArgs e)
            {
            }
     
            private void Button_Forget_User_Click(object sender, RoutedEventArgs e)
            {
                // Remove it from Microsoft Passport
                // MicrosoftPassportHelper.RemovePassportAccountAsync(_activeAccount);
     
                // Remove it from the local accounts list and resave the updated list
                AccountHelper.RemoveAccount(_activeAccount);
     
                Debug.WriteLine("User " + _activeAccount.Username + " deleted.");
            }
        }
    }
    ```

-   잊어버린 사용자 클릭 이벤트에서 주석으로 처리 된 줄을 발견할 수 있습니다. 계정이 로컬 목록에서 제거 되 고 있지만 현재 Windows Hello에서 제거할 수 있는 방법이 없습니다. Windows Hello 사용자 제거를 처리 하는 MicrosoftPassportHelper.cs에서 새 메서드를 구현 해야 합니다. 이 메서드는 다른 Windows Hello Api를 사용 하 여 계정을 열고 삭제 합니다. 실제로 계정을 삭제할 때 서버 또는 데이터베이스에 대 한 알림이 표시 되어야 사용자 데이터베이스가 유효한 상태로 유지 됩니다. 모델 폴더에 대 한 참조가 필요 합니다.

    ```cs
    using PassportLogin.Models;

    /// <summary>
    /// Function to be called when user requests deleting their account.
    /// Checks the KeyCredentialManager to see if there is a Passport for the current user
    /// Then deletes the local key associated with the Passport.
    /// </summary>
    public static async void RemovePassportAccountAsync(Account account)
    {
        // Open the account with Passport
        KeyCredentialRetrievalResult keyOpenResult = await KeyCredentialManager.OpenAsync(account.Username);

        if (keyOpenResult.Status == KeyCredentialStatus.Success)
        {
            // In the real world you would send key information to server to unregister
            //for example, RemovePassportAccountOnServer(account);
        }

        // Then delete the account from the machines list of Passport Accounts
        await KeyCredentialManager.DeleteAsync(account.Username);
    }
    ```

-   Welcome.xaml.cs로 돌아가서 RemovePassportAccountAsync를 호출 하는 줄의 주석 처리를 제거 합니다.

    ```cs
    private void Button_Forget_User_Click(object sender, RoutedEventArgs e)
    {
        // Remove it from Microsoft Passport
        MicrosoftPassportHelper.RemovePassportAccountAsync(_activeAccount);
     
        // Remove it from the local accounts list and resave the updated list
        AccountHelper.RemoveAccount(_activeAccount);
     
        Debug.WriteLine("User " + _activeAccount.Username + " deleted.");
    }
    ```

-   SignInPassport 메서드 (Login.xaml.cs)에서 CreatePassportKeyAsync가 성공적으로 완료 되 면 시작 화면으로 이동 하 여 계정을 전달 해야 합니다.

    ```cs
    private async void SignInPassport()
    {
        if (AccountHelper.ValidateAccountCredentials(UsernameTextBox.Text))
        {
            // Create and add a new local account
            _account = AccountHelper.AddAccount(UsernameTextBox.Text);
            Debug.WriteLine("Successfully signed in with traditional credentials and created local account instance!");

            if (await MicrosoftPassportHelper.CreatePassportKeyAsync(UsernameTextBox.Text))
            {
                Debug.WriteLine("Successfully signed in with Microsoft Passport!");
                Frame.Navigate(typeof(Welcome), _account);
            }
        }
        else
        {
            ErrorMessage.Text = "Invalid Credentials";
        }
    }
    ```

-   애플리케이션을 빌드 및 실행합니다. "SampleUsername"로 로그인 하 고 로그인을 클릭 합니다. PIN을 입력 하 고 성공 하면 시작 화면으로 이동 해야 합니다. 사용자 잊은 사용자를 클릭 하 고 출력 창을 모니터링 해 사용자가 삭제 되었는지 확인 합니다. 사용자를 삭제 하면 시작 페이지에 남아 있습니다. 앱이 탐색할 수 있는 사용자 선택 페이지를 만들어야 합니다.

    ![Windows Hello 시작 화면](images/passport-login-9.png)

-   Views 폴더에서 "UserSelection. .xaml" 이라는 새 빈 페이지를 만들고 다음 XAML을 추가 하 여 사용자 인터페이스를 정의 합니다. 이 페이지에는 로컬 계정 목록의 모든 사용자를 표시 하는 [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) 와 로그인 페이지로 이동 하 여 사용자가 다른 계정을 추가할 수 있는 단추가 포함 됩니다.

    ```xml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <StackPanel Orientation="Vertical">
        <TextBlock x:Name="Title" Text="Select a User" FontSize="36" Margin="4" TextAlignment="Center" HorizontalAlignment="Center"/>

        <ListView x:Name="UserListView" Margin="4" MaxHeight="200" MinWidth="250" Width="250" HorizontalAlignment="Center">
          <ListView.ItemTemplate>
            <DataTemplate>
              <Grid Background="DodgerBlue" Height="50" Width="250" HorizontalAlignment="Stretch" VerticalAlignment="Stretch">
                <TextBlock Text="{Binding Username}" HorizontalAlignment="Center" TextAlignment="Center" VerticalAlignment="Center" Foreground="White"/>
              </Grid>
            </DataTemplate>
          </ListView.ItemTemplate>
        </ListView>

        <Button x:Name="AddUserButton" Content="+" FontSize="36" Width="60" Click="AddUserButton_Click" HorizontalAlignment="Center"/>
      </StackPanel>
    </Grid>
    ```

-   UserSelection.xaml.cs에서 로컬 목록에 계정이 없는 경우 로그인 페이지로 이동 하는 로드 된 메서드를 구현 합니다. 또한 ListView의 SelectionChanged 이벤트와 단추에 대 한 클릭 이벤트를 구현 합니다.

    ```cs
    using System.Diagnostics;
    using PassportLogin.Models;
    using PassportLogin.Utils;

    namespace PassportLogin.Views
    {
        public sealed partial class UserSelection : Page
        {
            public UserSelection()
            {
                InitializeComponent();
                Loaded += UserSelection_Loaded;
            }

            private void UserSelection_Loaded(object sender, RoutedEventArgs e)
            {
                if (AccountHelper.AccountList.Count == 0)
                {
                    //If there are no accounts navigate to the LoginPage
                    Frame.Navigate(typeof(Login));
                }


                UserListView.ItemsSource = AccountHelper.AccountList;
                UserListView.SelectionChanged += UserSelectionChanged;
            }

            /// <summary>
            /// Function called when an account is selected in the list of accounts
            /// Navigates to the Login page and passes the chosen account
            /// </summary>
            private void UserSelectionChanged(object sender, RoutedEventArgs e)
            {
                if (((ListView)sender).SelectedValue != null)
                {
                    Account account = (Account)((ListView)sender).SelectedValue;
                    if (account != null)
                    {
                        Debug.WriteLine("Account " + account.Username + " selected!");
                    }
                    Frame.Navigate(typeof(Login), account);
                }
            }

            /// <summary>
            /// Function called when the "+" button is clicked to add a new user.
            /// Navigates to the Login page with nothing filled out
            /// </summary>
            private void AddUserButton_Click(object sender, RoutedEventArgs e)
            {
                Frame.Navigate(typeof(Login));
            }
        }
    }
    ```

<!-- -->

-   앱에는 UserSelection 페이지로 이동 하려는 몇 가지 위치가 있습니다. MainPage.xaml.cs에서 로그인 페이지 대신 UserSelection 페이지로 이동 해야 합니다. MainPage에서 로드 된 이벤트에 있는 동안 UserSelection 페이지에서 계정이 있는지 확인할 수 있도록 계정 목록을 로드 해야 합니다. 이렇게 하려면 로드 된 메서드를 async로 변경 하 고 유틸리티 폴더에 대 한 참조도 추가 해야 합니다.

    ```cs
    using PassportLogin.Utils;

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        // Load the local Accounts List before navigating to the UserSelection page
        await AccountHelper.LoadAccountListAsync();
        Frame.Navigate(typeof(UserSelection));
    }
    ```

-   다음에는 시작 페이지에서 UserSelection 페이지로 이동 합니다. 두 click 이벤트 모두에서 UserSelection 페이지로 다시 이동 해야 합니다.

    ```cs
    private void Button_Restart_Click(object sender, RoutedEventArgs e)
    {
        Frame.Navigate(typeof(UserSelection));
    }

    private void Button_Forget_User_Click(object sender, RoutedEventArgs e)
    {
        // Remove it from Microsoft Passport
        MicrosoftPassportHelper.RemovePassportAccountAsync(_activeAccount);

        // Remove it from the local accounts list and resave the updated list
        AccountHelper.RemoveAccount(_activeAccount);

        Debug.WriteLine("User " + _activeAccount.Username + " deleted.");

        // Navigate back to UserSelection page.
        Frame.Navigate(typeof(UserSelection));
    }
    ```

-   로그인 페이지에서 UserSelection 페이지의 목록에서 선택한 계정에 로그인 하는 데 필요한 코드가 필요 합니다. OnNavigatedTo 이벤트 저장소에서 탐색에 전달 된 계정을 검색 합니다. 계정이 기존 계정 인지 여부를 식별 하는 새 개인 변수를 추가 하 여 시작 합니다. 그런 다음 OnNavigatedTo 이벤트를 처리 합니다.

    ```cs
    namespace PassportLogin.Views
    {
        public sealed partial class Login : Page
        {
            private Account _account;
            private bool _isExistingAccount;

            public Login()
            {
                InitializeComponent();
            }

            /// <summary>
            /// Function called when this frame is navigated to.
            /// Checks to see if Microsoft Passport is available and if an account was passed in.
            /// If an account was passed in set the "_isExistingAccount" flag to true and set the _account
            /// </summary>
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // Check Microsoft Passport is setup and available on this machine
                if (await MicrosoftPassportHelper.MicrosoftPassportAvailableCheckAsync())
                {
                    if (e.Parameter != null)
                    {
                        _isExistingAccount = true;
                        // Set the account to the existing account being passed in
                        _account = (Account)e.Parameter;
                        UsernameTextBox.Text = _account.Username;
                        SignInPassport();
                    }
                }
                else
                {
                    // Microsoft Passport is not setup so inform the user
                    PassportStatus.Background = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 50, 170, 207));
                    PassportStatusText.Text = "Microsoft Passport is not setup!\n" + 
                        "Please go to Windows Settings and set up a PIN to use it.";
                    PassportSignInButton.IsEnabled = false;
                }
            }
        }
    }
    ```

-   SignInPassport 메서드를 업데이트 하 여 선택한 계정에 로그인 해야 합니다. 계정에 이미 Passport 키가 만들어져 있으므로 Passport를 사용 하 여 계정을 열 MicrosoftPassportHelper는 다른 방법이 필요 합니다. MicrosoftPassportHelper.cs에서 새 메서드를 구현 하 여 passport를 사용 하 여 기존 사용자에 로그인 합니다. 코드의 각 부분에 대 한 자세한 내용은 코드 주석을 참조 하세요.

    ```cs
    /// <summary>
    /// Attempts to sign a message using the Passport key on the system for the accountId passed.
    /// </summary>
    /// <returns>Boolean representing if creating the Passport authentication message succeeded</returns>
    public static async Task<bool> GetPassportAuthenticationMessageAsync(Account account)
    {
        KeyCredentialRetrievalResult openKeyResult = await KeyCredentialManager.OpenAsync(account.Username);
        // Calling OpenAsync will allow the user access to what is available in the app and will not require user credentials again.
        // If you wanted to force the user to sign in again you can use the following:
        // var consentResult = await Windows.Security.Credentials.UI.UserConsentVerifier.RequestVerificationAsync(account.Username);
        // This will ask for the either the password of the currently signed in Microsoft Account or the PIN used for Microsoft Passport.

        if (openKeyResult.Status == KeyCredentialStatus.Success)
        {
            // If OpenAsync has succeeded, the next thing to think about is whether the client application requires access to backend services.
            // If it does here you would Request a challenge from the Server. The client would sign this challenge and the server
            // would check the signed challenge. If it is correct it would allow the user access to the backend.
            // You would likely make a new method called RequestSignAsync to handle all this
            // for example, RequestSignAsync(openKeyResult);
            // Refer to the second Microsoft Passport sample for information on how to do this.

            // For this sample there is not concept of a server implemented so just return true.
            return true;
        }
        else if (openKeyResult.Status == KeyCredentialStatus.NotFound)
        {
            // If the _account is not found at this stage. It could be one of two errors. 
            // 1. Microsoft Passport has been disabled
            // 2. Microsoft Passport has been disabled and re-enabled cause the Microsoft Passport Key to change.
            // Calling CreatePassportKey and passing through the account will attempt to replace the existing Microsoft Passport Key for that account.
            // If the error really is that Microsoft Passport is disabled then the CreatePassportKey method will output that error.
            if (await CreatePassportKeyAsync(account.Username))
            {
                // If the Passport Key was again successfully created, Microsoft Passport has just been reset.
                // Now that the Passport Key has been reset for the _account retry sign in.
                return await GetPassportAuthenticationMessageAsync(account);
            }
        }

        // Can't use Passport right now, try again later
        return false;
    }
    ```

-   Login.xaml.cs의 SignInPassport 메서드를 업데이트 하 여 기존 계정을 처리 합니다. 그러면 MicrosoftPassportHelper.cs에서 새 메서드가 사용 됩니다. 성공 하면 계정이 로그인 되 고 사용자가 시작 화면으로 이동 됩니다.

    ```cs
    private async void SignInPassport()
    {
        if (_isExistingAccount)
        {
            if (await MicrosoftPassportHelper.GetPassportAuthenticationMessageAsync(_account))
            {
                Frame.Navigate(typeof(Welcome), _account);
            }
        }
        else if (AccountHelper.ValidateAccountCredentials(UsernameTextBox.Text))
        {
            //Create and add a new local account
            _account = AccountHelper.AddAccount(UsernameTextBox.Text);
            Debug.WriteLine("Successfully signed in with traditional credentials and created local account instance!");

            if (await MicrosoftPassportHelper.CreatePassportKeyAsync(UsernameTextBox.Text))
            {
                Debug.WriteLine("Successfully signed in with Microsoft Passport!");
                Frame.Navigate(typeof(Welcome), _account);
            }
        }
        else
        {
            ErrorMessage.Text = "Invalid Credentials";
        }
    }
    ```

-   애플리케이션을 빌드 및 실행합니다. "SampleUsername"를 사용 하 여 로그인 합니다. PIN을 입력 하 고 성공 하면 시작 화면으로 이동 됩니다. 사용자 목록으로 돌아가기를 클릭 합니다. 이제 목록에 사용자가 표시 됩니다. 이 Passport를 클릭 하면 암호 등을 다시 입력할 필요 없이 다시 로그인 할 수 있습니다.

    ![Windows Hello 사용자 목록 선택](images/passport-login-10.png)

## <a name="exercise-3-registering-a-new-windows-hello-user"></a>연습 3: 새 Windows Hello 사용자 등록


이 연습에서는 Windows Hello를 사용 하 여 새 계정을 만들 새 페이지를 만들게 됩니다. 이는 로그인 페이지의 작동 방식과 비슷하게 작동 합니다. 로그인 페이지는 Windows Hello를 사용 하도록 마이그레이션하는 기존 사용자에 대해 구현 됩니다. PassportRegister 페이지는 새 사용자에 대 한 Windows Hello 등록을 만듭니다.

-   Views 폴더에서 "PassportRegister" 라는 빈 페이지를 새로 만듭니다. XAML에서 다음을 추가 하 여 사용자 인터페이스를 설정 합니다. 여기에서 인터페이스는 로그인 페이지와 비슷합니다.

    ```xml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <StackPanel Orientation="Vertical">
        <TextBlock x:Name="Title" Text="Register New Passport User" FontSize="24" Margin="4" TextAlignment="Center"/>

        <TextBlock x:Name="ErrorMessage" Text="" FontSize="20" Margin="4" Foreground="Red" TextAlignment="Center"/>

        <TextBlock Text="Enter your new username below" Margin="0,0,0,20"
                   TextWrapping="Wrap" Width="300"
                   TextAlignment="Center" VerticalAlignment="Center" FontSize="16"/>

        <TextBox x:Name="UsernameTextBox" Margin="4" Width="250"/>

        <Button x:Name="PassportRegisterButton" Content="Register" Background="DodgerBlue" Foreground="White"
            Click="RegisterButton_Click_Async" Width="80" HorizontalAlignment="Center" Margin="0,20"/>

        <Border x:Name="PassportStatus" Background="#22B14C"
                   Margin="4" Height="100">
          <TextBlock x:Name="PassportStatusText" Text="Microsoft Passport is ready to use!" FontSize="20"
                 Margin="4" TextAlignment="Center" VerticalAlignment="Center"/>
        </Border>
      </StackPanel>
    </Grid>
    ```

-   PassportRegister.xaml.cs 코드 뒷부분에서 파일은 개인 계정 변수를 구현 하 고 등록 단추를 클릭 하 여 이벤트를 클릭 합니다. 그러면 새 로컬 계정이 추가 되 고 Passport 키가 생성 됩니다.

    ```cs
    using PassportLogin.Models;
    using PassportLogin.Utils;

    namespace PassportLogin.Views
    {
        public sealed partial class PassportRegister : Page
        {
            private Account _account;

            public PassportRegister()
            {
                InitializeComponent();
            }

            private async void RegisterButton_Click_Async(object sender, RoutedEventArgs e)
            {
                ErrorMessage.Text = "";

                //In the real world you would normally validate the entered credentials and information before 
                //allowing a user to register a new account. 
                //For this sample though we will skip that step and just register an account if username is not null.

                if (!string.IsNullOrEmpty(UsernameTextBox.Text))
                {
                    //Register a new account
                    _account = AccountHelper.AddAccount(UsernameTextBox.Text);
                    //Register new account with Microsoft Passport
                    await MicrosoftPassportHelper.CreatePassportKeyAsync(_account.Username);
                    //Navigate to the Welcome Screen. 
                    Frame.Navigate(typeof(Welcome), _account);
                }
                else
                {
                    ErrorMessage.Text = "Please enter a username";
                }
            }
        }
    }
    ```

-   등록을 클릭 하면 로그인 페이지에서이 페이지로 이동 해야 합니다.

    ```cs
    private void RegisterButtonTextBlock_OnPointerPressed(object sender, PointerRoutedEventArgs e)
    {
        ErrorMessage.Text = "";
        Frame.Navigate(typeof(PassportRegister));
    }
    ```

-   애플리케이션을 빌드 및 실행합니다. 새 사용자를 등록 하십시오. 그런 다음 사용자 목록으로 돌아가 해당 사용자 및 로그인을 선택할 수 있는지 확인 합니다.

    ![Windows Hello 새 사용자 등록](images/passport-login-11.png)

이 랩에서는 새 Windows Hello API를 사용 하 여 기존 사용자를 인증 하 고 새 사용자에 대 한 계정을 만드는 데 필요한 필수 기술을 배웠습니다. 이 새로운 정보를 통해 사용자가 응용 프로그램에 대 한 암호를 기억할 필요가 없도록 제거 하 고 응용 프로그램이 사용자 인증으로 보호 되는 상태를 유지할 수 있습니다. Windows 10은 Windows Hello의 새 인증 기술을 사용 하 여 해당 생체 인식 로그인 옵션을 지원 합니다.

## <a name="related-topics"></a>관련 항목

* [Windows Hello](microsoft-passport.md)
* [Windows Hello 로그인 서비스](microsoft-passport-login-auth-service.md)