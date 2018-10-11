---
title: 앱 간에 공유 인증서
description: 사용자 ID 및 암호 조합 이상의 보안 인증이 필요한 UWP(유니버설 Windows 플랫폼) 앱은 인증을 위해 인증서를 사용할 수 있습니다.
ms.assetid: 159BA284-9FD4-441A-BB45-A00E36A386F9
author: PatrickFarley
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: 863658438ce53f2c74faddb845a7d17c6ec3130c
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4507538"
---
# <a name="share-certificates-between-apps"></a>앱 간에 공유 인증서




사용자 ID 및 암호 조합 이상의 보안 인증이 필요한 UWP(유니버설 Windows 플랫폼) 앱은 인증을 위해 인증서를 사용할 수 있습니다. 인증서 인증은 사용자를 인증할 때 높은 수준의 신뢰를 제공합니다. 서비스 그룹에서 여러 개의 앱에 대해 사용자를 인증하려는 경우도 있습니다. 이 문서에서는 동일한 인증서를 사용하여 여러 개의 앱을 인증하는 방법 및 보안 웹 서비스 액세스를 위해 제공된 인증서를 사용자가 가져오는 데 편리한 코드를 제공하는 방법을 보여 줍니다.

앱은 인증서를 사용하여 웹 서비스에 인증할 수 있으며, 여러 개의 앱이 인증서 저장소의 단일 인증서를 사용하여 동일한 사용자를 인증할 수 있습니다. 저장소에 인증서가 없는 경우 앱에 코드를 추가하여 PFX 파일에서 인증서를 가져올 수 있습니다.

## <a name="enable-microsoft-internet-information-services-iis-and-client-certificate-mapping"></a>Microsoft IIS(인터넷 정보 서비스) 및 클라이언트 인증서 매핑 사용


이 문서에서는 Microsoft IIS(인터넷 정보 서비스)를 예제 목적으로 사용합니다. IIS는 기본적으로 사용되지 않습니다. 제어판을 통해 IIS를 사용하도록 설정할 수 있습니다.

1.  제어판을 열고 **프로그램**을 선택합니다.
2.  **Windows 기능 사용/사용 안 함**을 선택합니다.
3.  **인터넷 정보 서비스**를 확장한 다음 **World Wide Web 서비스**를 확장합니다. **응용 프로그램 개발 기능**을 확장하고 **ASP.NET 3.5** 및 **ASP.NET 4.5**를 선택합니다. 이러한 기능을 선택하면 **인터넷 정보 서비스**를 자동으로 사용할 수 있습니다.
4.  **확인**을 클릭하여 변경 내용을 적용합니다.

## <a name="create-and-publish-a-secured-web-service"></a>보안 웹 서비스 만들기 및 게시


1.  관리자 권한으로 Microsoft Visual Studio를 실행하고 시작 페이지에서 **새 프로젝트**를 선택합니다. IIS 서버에 웹 서비스를 게시하려면 관리자 액세스 권한이 필요합니다. 새 프로젝트 대화 상자에서 프레임워크를 **.NET Framework 3.5**로 변경합니다. **Visual C#** -&gt; **웹** -&gt; **Visual Studio** -&gt; **ASP.NET 웹 서비스 응용 프로그램**을 선택합니다. 응용 프로그램 이름을 "FirstContosoBank"로 지정합니다. **확인**을 클릭하여 프로젝트를 만듭니다.
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

3.  **Service1.asmx.cs** 파일을 저장합니다.
4.  **솔루션 탐색기**에서 "FirstContosoBank" 앱을 마우스 오른쪽 단추로 클릭하고 **게시**를 선택합니다.
5.  **웹 게시** 대화 상자에서 새 프로필을 만들고 이름을 "ContosoProfile"로 지정합니다. **다음**을 클릭합니다.
6.  다음 페이지에서 IIS 서버의 서버 이름을 입력하고 사이트 이름을 "Default Web Site/FirstContosoBank"로 지정합니다. **게시**를 클릭하여 웹 서비스를 게시합니다.

## <a name="configure-your-web-service-to-use-client-certificate-authentication"></a>클라이언트 인증서 인증을 사용하도록 웹 서비스 구성


1.  **IIS(인터넷 정보 서비스) 관리자**를 실행합니다.
2.  IIS 서버의 사이트를 확장합니다. **기본 웹 사이트** 아래에서 새 "FirstContosoBank" 웹 서비스를 선택합니다. **작업** 섹션에서 **고급 설정...** 을 선택합니다.
3.  **응용 프로그램 풀**을 **.NET v2.0**으로 설정하고 **확인**을 클릭합니다.
4.  **IIS(인터넷 정보 서비스) 관리자**에서 IIS 서버를 선택하고 **서버 인증서**를 두 번 클릭합니다. **작업** 섹션에서 **자체 서명된 인증서 만들기...** 를 선택하고 인증서의 식별 이름으로 "ContosoBank"를 입력한 후 **확인**을 클릭합니다. IIS 서버에서 사용할 새 인증서가 &lt;server-name&gt;.&lt;domain-name&gt; 형식으로 만들어집니다.
5.  **IIS(인터넷 정보 서비스) 관리자**에서 기본 웹 사이트를 선택합니다. **작업** 섹션에서 **바인딩**을 선택한 다음 **추가...** 를 클릭합니다. 유형으로 "https"를 선택하고 포트를 "443"으로 설정한 후 IIS 서버의 전체 호스트 이름("&lt;server-name&gt;.&lt;domain-name&gt;")을 입력합니다. SSL 인증서를 "ContosoBank"로 설정합니다. **확인**을 클릭합니다. **사이트 바인딩** 창에서 **닫기**를 클릭합니다.
6.  **IIS(인터넷 정보 서비스) 관리자**에서 "FirstContosoBank" 웹 서비스를 선택합니다. **SSL 설정**을 두 번 클릭합니다. **SSL 필요**를 선택합니다. **클라이언트 인증서**에서 **필요**를 선택합니다. **작업** 섹션에서 **적용**을 클릭합니다.
7.  웹 브라우저를 열고 다음 웹 주소를 입력하여 웹 서비스가 올바르게 구성되었는지 확인할 수 있습니다. "https://&lt;server-name&gt;.&lt;domain-name&gt;/FirstContosoBank/Service1.asmx" 예를 들면 "https://myserver.example.com/FirstContosoBank/Service1.asmx"입니다. 웹 서비스가 올바르게 구성된 경우 웹 서비스에 액세스하려면 클라이언트 인증서를 선택하라는 메시지가 표시됩니다.

이전 단계를 반복하여 동일한 클라이언트 인증서로 액세스할 수 있는 웹 서비스를 여러 개 만들 수 있습니다.

## <a name="create-a-uwp-app-that-uses-certificate-authentication"></a>인증서 인증을 사용하는 UWP 앱 만들기


이제 보안 웹 서비스가 하나 이상 있으므로 앱에서 인증서를 사용하여 해당 웹 서비스에 인증할 수 있습니다. [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 개체를 사용하여 인증된 웹 서비스를 요청하는 경우 초기 요청에는 클라이언트 인증서가 포함되지 않습니다. 인증된 웹 서비스에서 클라이언트 인증 요청으로 응답합니다. 그러면 Windows 클라이언트가 인증서 저장소에서 사용 가능한 클라이언트 인증서를 자동으로 쿼리합니다. 사용자는 이러한 인증서 중에서 웹 서비스에 인증할 인증서를 선택할 수 있습니다. 일부 인증서는 암호로 보호되어 있으므로 인증서 암호를 입력하는 방법을 사용자에게 제공해야 합니다.

사용 가능한 클라이언트 인증서가 없으면 사용자가 인증서 저장소에 인증서를 추가 해야 합니다. 사용자가 클라이언트 인증서를 포함하는 PFX 파일을 선택하고 해당 인증서를 클라이언트 인증서 저장소로 가져올 수 있도록 하는 코드를 앱에 포함할 수 있습니다.

**팁**  makecert.exe를 통해 이 빠른 시작에서 사용할 PFX 파일을 만들 수 있습니다. makecert.exe 사용에 대한 자세한 내용은 [MakeCert](https://msdn.microsoft.com/library/windows/desktop/aa386968)를 참조하세요.

 

1.  Visual Studio를 열고 시작 페이지에서 새 프로젝트를 만듭니다. 새 프로젝트의 이름을 "FirstContosoBankApp"으로 지정합니다. **확인**을 클릭하여 새 프로젝트를 만듭니다.
2.  MainPage.xaml 파일의 기본 **Grid** 요소에 다음 XAML을 추가합니다. 이 XAML에는 가져올 PFX 파일 찾아보기 단추, 암호로 보호된 PFX 파일의 암호를 입력할 입력란, 선택한 PFX 파일 가져오기 단추, 보안 웹 서비스에 로그인 단추 및 현재 작업의 상태를 표시할 텍스트 블록이 포함되어 있습니다.
    ```xml
    <Button x:Name="Import" Content="Import Certificate (PFX file)" HorizontalAlignment="Left" Margin="352,305,0,0" VerticalAlignment="Top" Height="77" Width="260" Click="Import_Click" FontSize="16"/>
    <Button x:Name="Login" Content="Login" HorizontalAlignment="Left" Margin="611,305,0,0" VerticalAlignment="Top" Height="75" Width="240" Click="Login_Click" FontSize="16"/>
    <TextBlock x:Name="Result" HorizontalAlignment="Left" Margin="355,398,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Height="153" Width="560"/>
    <PasswordBox x:Name="PfxPassword" HorizontalAlignment="Left" Margin="483,271,0,0" VerticalAlignment="Top" Width="229"/>
    <TextBlock HorizontalAlignment="Left" Margin="355,271,0,0" TextWrapping="Wrap" Text="PFX password" VerticalAlignment="Top" FontSize="18" Height="32" Width="123"/>
    <Button x:Name="Browse" Content="Browse for PFX file" HorizontalAlignment="Left" Margin="352,189,0,0" VerticalAlignment="Top" Click="Browse_Click" Width="499" Height="68" FontSize="16"/>
    <TextBlock HorizontalAlignment="Left" Margin="717,271,0,0" TextWrapping="Wrap" Text="(Optional)" VerticalAlignment="Top" Height="32" Width="83" FontSize="16"/>
    ```
    
3.  MainPage.xaml 파일을 저장합니다.
4.  MainPage.xaml.cs 파일에 다음 using 문을 추가합니다.
    ```cs
    using Windows.Web.Http;
    using System.Text;
    using Windows.Security.Cryptography.Certificates;
    using Windows.Storage.Pickers;
    using Windows.Storage;
    using Windows.Storage.Streams;
    ```

5.  MainPage.xaml.cs 파일의 **MainPage** 클래스에 다음 변수를 추가합니다. 이러한 변수는 "FirstContosoBank" 웹 서비스의 보안 "Login" 메서드에 대한 주소와 인증서 저장소로 가져올 PFX 인증서를 포함하는 전역 변수를 지정합니다. &lt;server-name&gt; 을 Microsoft Internet Information Server(IIS) 서버의 정규화된 서버 이름으로 업데이트합니다.
    ```cs
    private Uri requestUri = new Uri("https://<server-name>/FirstContosoBank/Service1.asmx?op=Login");
    private string pfxCert = null;
    ```

6.  MainPage.xaml.cs 파일에서 보안 웹 서비스 액세스에 필요한 메서드 및 로그인 단추에 대해 다음과 같은 클릭 처리기를 추가합니다.
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

7.  MainPage.xaml.cs 파일에서 PFX 파일 찾아보기 단추 및 인증서 저장소로 선택한 PFX 파일 가져오기 단추에 대해 다음과 같은 클릭 처리기를 추가합니다.
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

8.  앱을 실행하고 보안 웹 서비스에 로그인하거나 PFX 파일을 로컬 인증서 저장소로 가져올 수 있습니다.

이러한 단계를 따르면 동일한 사용자 인증서를 사용하여 같거나 서로 다른 보안 웹 서비스에 액세스하는 앱을 여러 개 만들 수 있습니다.