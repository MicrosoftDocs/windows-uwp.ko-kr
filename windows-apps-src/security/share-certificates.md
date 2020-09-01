---
title: 앱 간에 인증서 공유
description: 사용자 Id 및 암호 조합 이외의 보안 인증을 필요로 하는 UWP (유니버설 Windows 플랫폼) 앱은 인증을 위해 인증서를 사용할 수 있습니다.
ms.assetid: 159BA284-9FD4-441A-BB45-A00E36A386F9
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: cbe4f1e2a9b3e5290cd26edae10af6ac9bb0dbfc
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155297"
---
# <a name="share-certificates-between-apps"></a>앱 간에 인증서 공유




사용자 Id 및 암호 조합 이외의 보안 인증을 필요로 하는 UWP (유니버설 Windows 플랫폼) 앱은 인증을 위해 인증서를 사용할 수 있습니다. 인증서 인증은 사용자를 인증할 때 높은 수준의 신뢰를 제공합니다. 서비스 그룹에서 여러 개의 앱에 대해 사용자를 인증하려는 경우도 있습니다. 이 문서에서는 동일한 인증서를 사용하여 여러 개의 앱을 인증하는 방법 및 보안 웹 서비스 액세스를 위해 제공된 인증서를 사용자가 가져오는 데 편리한 코드를 제공하는 방법을 보여 줍니다.

앱은 인증서를 사용 하 여 웹 서비스에 인증할 수 있으며, 여러 앱은 인증서 저장소에서 단일 인증서를 사용 하 여 동일한 사용자를 인증할 수 있습니다. 인증서가 저장소에 없는 경우 응용 프로그램에 코드를 추가 하 여 PFX 파일에서 인증서를 가져올 수 있습니다.

## <a name="enable-microsoft-internet-information-services-iis-and-client-certificate-mapping"></a>Microsoft 인터넷 정보 서비스 (IIS) 및 클라이언트 인증서 매핑 사용


이 문서에서는 예를 들어 Microsoft 인터넷 정보 서비스 (IIS)를 사용 합니다. IIS는 기본적으로 사용 되지 않습니다. 제어판을 사용 하 여 IIS를 사용 하도록 설정할 수 있습니다.

1.  제어판을 열고 **프로그램**을 선택 합니다.
2.  **Windows 기능 사용/사용 안 함을**선택 합니다.
3.  **인터넷 정보 서비스** 확장 한 다음 **World Wide Web Services**를 확장 합니다. **응용 프로그램 개발 기능** 을 확장 하 고 **ASP.NET 3.5** 및 **ASP.NET 4.5**를 선택 합니다. 이러한 항목을 선택 하면 **인터넷 정보 서비스**자동으로 사용 하도록 설정 됩니다.
4.  **확인**을 클릭하여 변경 내용을 적용합니다.

## <a name="create-and-publish-a-secured-web-service"></a>보안 웹 서비스 만들기 및 게시


1.  관리자 권한으로 Microsoft Visual Studio를 실행 하 고 시작 페이지에서 **새 프로젝트** 를 선택 합니다. IIS 서버에 웹 서비스를 게시 하려면 관리자 권한이 필요 합니다. 새 프로젝트 대화 상자에서 프레임 워크를 **.NET Framework 3.5**로 변경 합니다. **Visual c #**  - &gt; **웹**  - &gt; **visual Studio**  - &gt; **ASP.NET 웹 서비스 응용 프로그램**을 선택 합니다. 응용 프로그램 이름을 "FirstContosoBank"로 합니다. **확인**을 클릭하여 프로젝트를 만듭니다.
2.  **Service1.asmx.cs** 파일에서 기본 **HelloWorld** 웹 메서드를 다음 "Login" 메서드로 바꿉니다.
    ```cs
            [WebMethod]
            public string Login()
            {
                // Verify certificate with CA
                var cert = new System.Security.Cryptography.X509Certificates.X509Certificate2(
                    this.Context.Request.ClientCertificate.Certificate);
                bool test = cert.Verify();
                return test.ToString();
            }
    ```

3.  **Service1.asmx.cs** 파일을 저장 합니다.
4.  **솔루션 탐색기**에서 "FirstContosoBank" 앱을 마우스 오른쪽 단추로 클릭 하 고 **게시**를 선택 합니다.
5.  **웹 게시** 대화 상자에서 새 프로필을 만들고 이름을 "ContosoProfile"로 합니다. **다음**을 클릭합니다.
6.  다음 페이지에서 IIS 서버에 대 한 서버 이름을 입력 하 고 "Default Web Site/FirstContosoBank"의 사이트 이름을 지정 합니다. **게시** 를 클릭 하 여 웹 서비스를 게시 합니다.

## <a name="configure-your-web-service-to-use-client-certificate-authentication"></a>클라이언트 인증서 인증을 사용 하도록 웹 서비스 구성


1.  **인터넷 정보 서비스 (IIS) 관리자**를 실행 합니다.
2.  IIS 서버에 대 한 사이트를 확장 합니다. **기본 웹 사이트**에서 새 "FirstContosoBank" 웹 서비스를 선택 합니다. **작업** 섹션에서 **고급 설정**...을 선택 합니다.
3.  **응용 프로그램 풀** 을 **.net v 2.0** 으로 설정 하 고 **확인**을 클릭 합니다.
4.  **인터넷 정보 서비스 (iis) 관리자**에서 iis 서버를 선택 하 고 **서버 인증서**를 두 번 클릭 합니다. **작업** 섹션에서 **자체 서명 된 인증서 만들기**...를 선택 합니다. 인증서의 이름으로 "ContosoBank"를 입력 하 고 **확인**을 클릭 합니다. 이렇게 하면 IIS 서버에서 "서버 이름" 형식으로 사용할 새 인증서를 만듭니다 &lt; &gt; . &lt; 도메인 이름 &gt; ".
5.  **인터넷 정보 서비스 (IIS) 관리자**에서 기본 웹 사이트를 선택 합니다. **작업** 섹션에서 **바인딩** 을 선택 하 고 **추가**...를 클릭 합니다. 유형으로 "https"를 선택 하 고, 포트를 "443"로 설정 하 고, IIS 서버의 전체 호스트 이름 ("서버 이름")을 입력 합니다 &lt; &gt; . &lt; 도메인 이름 &gt; "). SSL 인증서를 "ContosoBank"로 설정 합니다. **확인**을 클릭합니다. **사이트 바인딩** 창에서 **닫기** 를 클릭 합니다.
6.  **인터넷 정보 서비스 (IIS) 관리자**에서 "FirstContosoBank" 웹 서비스를 선택 합니다. **SSL 설정**을 두 번 클릭 합니다. **SSL 필요**를 선택 합니다. **클라이언트 인증서**아래에서 **요구**를 선택 합니다. **작업** 섹션에서 **적용**을 클릭 합니다.
7.  웹 브라우저를 열고 다음 웹 주소를 입력 하 여 웹 서비스가 올바르게 구성 되어 있는지 확인할 수 있습니다. "https:// &lt; &gt; . &lt; 도메인 이름 &gt; /FirstContosoBank/Service1.asmx ". 예: "https://myserver.example.com/FirstContosoBank/Service1.asmx". 웹 서비스가 올바르게 구성 된 경우 웹 서비스에 액세스 하기 위해 클라이언트 인증서를 선택 하 라는 메시지가 표시 됩니다.

이전 단계를 반복 하 여 동일한 클라이언트 인증서를 사용 하 여 액세스할 수 있는 웹 서비스를 여러 개 만들 수 있습니다.

## <a name="create-a-uwp-app-that-uses-certificate-authentication"></a>인증서 인증을 사용 하는 UWP 앱 만들기


이제 하나 이상의 보안 웹 서비스가 있으므로 앱에서 인증서를 사용 하 여 해당 웹 서비스에 인증할 수 있습니다. [**Httpclient**](/uwp/api/Windows.Web.Http.HttpClient) 개체를 사용 하 여 인증 된 웹 서비스에 요청을 수행 하면 초기 요청에 클라이언트 인증서가 포함 되지 않습니다. 인증 된 웹 서비스는 클라이언트 인증 요청을 사용 하 여 응답 합니다. 이 경우 Windows 클라이언트는 인증서 저장소에서 사용 가능한 클라이언트 인증서를 자동으로 쿼리 합니다. 사용자는 이러한 인증서를 선택 하 여 웹 서비스에 인증할 수 있습니다. 일부 인증서는 암호로 보호 되므로 사용자에 게 인증서 암호를 입력 하는 방법을 제공 해야 합니다.

사용할 수 있는 클라이언트 인증서가 없는 경우 인증서 저장소에 인증서를 추가 해야 합니다. 사용자가 클라이언트 인증서가 포함 된 PFX 파일을 선택 하 고 해당 인증서를 클라이언트 인증서 저장소로 가져올 수 있도록 하는 코드를 앱에 포함할 수 있습니다.

**팁**    makecert.exe를 사용 하 여이 빠른 시작에 사용할 PFX 파일을 만들 수 있습니다. makecert.exe 사용에 대 한 자세한 내용은 [makecert.exe](/windows/desktop/SecCrypto/makecert) 를 참조 하세요.

 

1.  Visual Studio를 열고 시작 페이지에서 새 프로젝트를 만듭니다. 새 프로젝트의 이름을 "FirstContosoBankApp"로 합니다. **확인**을 클릭하여 새 프로젝트를 만듭니다.
2.  MainPage .xaml 파일에서 다음 XAML을 기본 **Grid** 요소에 추가 합니다. 이 XAML에는 가져올 PFX 파일을 검색 하는 단추, 암호로 보호 된 PFX 파일에 대 한 암호를 입력 하는 입력란, 선택한 PFX 파일을 가져오는 단추, 보안 웹 서비스에 로그인 하는 데 사용할 수 있는 단추 및 현재 동작의 상태를 표시 하는 텍스트 블록이 포함 되어 있습니다.
    ```xml
    <Button x:Name="Import" Content="Import Certificate (PFX file)" HorizontalAlignment="Left" Margin="352,305,0,0" VerticalAlignment="Top" Height="77" Width="260" Click="Import_Click" FontSize="16"/>
    <Button x:Name="Login" Content="Login" HorizontalAlignment="Left" Margin="611,305,0,0" VerticalAlignment="Top" Height="75" Width="240" Click="Login_Click" FontSize="16"/>
    <TextBlock x:Name="Result" HorizontalAlignment="Left" Margin="355,398,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Height="153" Width="560"/>
    <PasswordBox x:Name="PfxPassword" HorizontalAlignment="Left" Margin="483,271,0,0" VerticalAlignment="Top" Width="229"/>
    <TextBlock HorizontalAlignment="Left" Margin="355,271,0,0" TextWrapping="Wrap" Text="PFX password" VerticalAlignment="Top" FontSize="18" Height="32" Width="123"/>
    <Button x:Name="Browse" Content="Browse for PFX file" HorizontalAlignment="Left" Margin="352,189,0,0" VerticalAlignment="Top" Click="Browse_Click" Width="499" Height="68" FontSize="16"/>
    <TextBlock HorizontalAlignment="Left" Margin="717,271,0,0" TextWrapping="Wrap" Text="(Optional)" VerticalAlignment="Top" Height="32" Width="83" FontSize="16"/>
    ```
    
3.  MainPage .xaml 파일을 저장 합니다.
4.  MainPage.xaml.cs 파일에서 다음 using 문을 추가 합니다.
    ```cs
    using Windows.Web.Http;
    using System.Text;
    using Windows.Security.Cryptography.Certificates;
    using Windows.Storage.Pickers;
    using Windows.Storage;
    using Windows.Storage.Streams;
    ```

5.  MainPage.xaml.cs 파일에서 다음 변수를 **Mainpage** 클래스에 추가 합니다. "FirstContosoBank" 웹 서비스에 대 한 보안 "로그인" 방법의 주소와 인증서 저장소로 가져올 PFX 인증서를 포함 하는 전역 변수를 지정 합니다. &lt;서버 이름을 &gt; Microsoft Iis (인터넷 정보 서버) 서버의 정규화 된 서버 이름으로 업데이트 합니다.
    ```cs
    private Uri requestUri = new Uri("https://<server-name>/FirstContosoBank/Service1.asmx?op=Login");
    private string pfxCert = null;
    ```

6.  MainPage.xaml.cs 파일에서 로그인 단추 및 메서드에 대해 다음 클릭 처리기를 추가 하 여 보안 웹 서비스에 액세스 합니다.
    ```cs
    private void Login_Click(object sender, RoutedEventArgs e)
    {
        MakeHttpsCall();
    }

    private async void MakeHttpsCall()
    {

        StringBuilder result = new StringBuilder("Login ");
        HttpResponseMessage response;
        try
        {
            Windows.Web.Http.HttpClient httpClient = new Windows.Web.Http.HttpClient();
            response = await httpClient.GetAsync(requestUri);
            if (response.StatusCode == HttpStatusCode.Ok)
            {
                result.Append("successful");
            }
            else
            {
                result = result.Append("failed with ");
                result = result.Append(response.StatusCode);
            }
        }
        catch (Exception ex)
        {
            result = result.Append("failed with ");
            result = result.Append(ex.Message);
        }

        Result.Text = result.ToString();
    }
    ```

7.  MainPage.xaml.cs 파일에서 단추에 대 한 다음 클릭 처리기를 추가 하 여 PFX 파일을 찾고, 단추를 선택 하 여 선택한 PFX 파일을 인증서 저장소로 가져옵니다.
    ```cs
    private async void Import_Click(object sender, RoutedEventArgs e)
    {
        try
        {
            Result.Text = "Importing selected certificate into user certificate store....";
            await CertificateEnrollmentManager.UserCertificateEnrollmentManager.ImportPfxDataAsync(
                pfxCert,
                PfxPassword.Password,
                ExportOption.Exportable,
                KeyProtectionLevel.NoConsent,
                InstallOptions.DeleteExpired,
                "Import Pfx");

            Result.Text = "Certificate import succeded";
        }
        catch (Exception ex)
        {
            Result.Text = "Certificate import failed with " + ex.Message;
        }
    }

    private async void Browse_Click(object sender, RoutedEventArgs e)
    {

        StringBuilder result = new StringBuilder("Pfx file selection ");
        FileOpenPicker pfxFilePicker = new FileOpenPicker();
        pfxFilePicker.FileTypeFilter.Add(".pfx");
        pfxFilePicker.CommitButtonText = "Open";
        try
        {
            StorageFile pfxFile = await pfxFilePicker.PickSingleFileAsync();
            if (pfxFile != null)
            {
                IBuffer buffer = await FileIO.ReadBufferAsync(pfxFile);
                using (DataReader dataReader = DataReader.FromBuffer(buffer))
                {
                    byte[] bytes = new byte[buffer.Length];
                    dataReader.ReadBytes(bytes);
                    pfxCert = System.Convert.ToBase64String(bytes);
                    PfxPassword.Password = string.Empty;
                    result.Append("succeeded");
                }
            }
            else
            {
                result.Append("failed");
            }
        }
        catch (Exception ex)
        {
            result.Append("failed with ");
            result.Append(ex.Message); ;
        }

        Result.Text = result.ToString();
    }
    ```

8.  앱을 실행 하 고 보안 웹 서비스에 로그인 하 고 PFX 파일을 로컬 인증서 저장소로 가져옵니다.

이러한 단계를 사용 하 여 동일한 사용자 인증서를 사용 하는 여러 앱을 만들어 동일 하거나 다른 보안 웹 서비스에 액세스할 수 있습니다.