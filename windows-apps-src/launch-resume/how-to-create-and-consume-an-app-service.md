---
title: 앱 서비스 만들기 및 사용
description: 다른 UWP 앱에 서비스를 제공할 수 있는 UWP(유니버설 Windows 플랫폼) 앱을 작성하는 방법과 해당 서비스를 사용하는 방법을 알아봅니다.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: 앱 간 통신, 프로세스 간 통신, IPC, 백그라운드 메시징, 백그라운드 통신, 앱, 앱 서비스
ms.date: 1/16/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9029c8ee3a930e66ebdbd0c4d0681d87486a8393
ms.sourcegitcommit: 6e2027f8ebc1d891d27ea6b2e4676d592871bcc7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/16/2019
ms.locfileid: "9011263"
---
# <a name="create-and-consume-an-app-service"></a>앱 서비스 만들기 및 사용

앱 서비스는 다른 UWP 앱에 서비스를 제공하는 UWP 앱입니다. 장치에서 앱 서비스는 웹 서비스와 비슷합니다. 앱 서비스는 호스트 앱에서 배경 작업으로 실행되고 다른 앱에 서비스를 제공할 수 있습니다. 예를 들어, 앱 서비스는 다른 앱에서 사용할 수 있는 바코드 스캐너 서비스를 제공할 수 있습니다. 또는 앱의 Enterprise Suite에 해당 제품군의 다른 앱에서 사용할 수 있는 일반 맞춤법 검사 앱 서비스가 있습니다.  Windows 10, 버전 1607부터 앱 서비스를 통해 앱이 동일한 장치에서 호출할 수 있는 UI 없는 서비스를 원격 장치에 만들 수 있습니다.

Windows10 버전 1607부터 호스트 앱과 같은 프로세스에서 실행되는 앱 서비스를 만들 수 있습니다. 이 문서는 별도 백그라운드 프로세스에서 실행되는 앱 서비스 만들기 및 사용에 중점을 둡니다. 공급자와 같은 프로세스에서 앱 서비스를 실행하는 방법에 대한 자세한 내용은 [앱 서비스가 호스트 앱과 동일한 프로세스에서 실행되도록 변환](convert-app-service-in-process.md)을 참조하세요.

앱 서비스 코드 샘플은 [유니버설 Windows 플랫폼(UWP) 앱 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)을 참조하세요.

## <a name="create-a-new-app-service-provider-project"></a>새 앱 서비스 공급자 프로젝트 만들기

이 방법에서는 편의상 모두를 한 솔루션으로 만듭니다.

1. Visual Studio 2015 이상 버전에서는 UWP 앱 프로젝트를 만들고 **AppServiceProvider**로 이름을 지정 합니다.
    1. **파일 gt_ 새 gt_ 프로젝트** 를 선택 합니다. 
    2. **새 프로젝트** 대화 상자의 **설치 된 gt_ Visual C# gt_ 빈 앱 (유니버설 Windows)을**선택 합니다. 이렇게 하면 다른 UWP 앱에서 앱 서비스 사용할 수 있게 됩니다.
    3. **AppServiceProvider**프로젝트 이름을, 위치를 선택 하 고 **확인**을 클릭 합니다.

2. 프로젝트에 대 한 **대상** 및 **최소 버전** 을 선택할를 묻는 메시지가 나타나면 하나 이상 선택 **10.0.14393**. Visual Studio 2017 및 대상 **10.0.15063** (**Windows 10 크리에이터 스 업데이트**)을 사용 해야 새 **SupportsMultipleInstances** 특성을 사용 하려는 경우 이상.

<span id="appxmanifest"/>

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>Package.appxmanifest에 앱 서비스 확장 추가

**AppServiceProvider** 프로젝트에서 텍스트 편집기에서 **Package.appxmanifest** 파일을 엽니다. 

1. **솔루션 탐색기**에서 마우스 합니다. 
2. **다른 프로그램으로 열기**를 선택 합니다. 
3. **XML (텍스트) 편집기**를 선택 합니다. 

다음 코드를 추가 `AppService` 확장 기능 합니다 `<Application>` 요소입니다. 이 예제에서는 `com.microsoft.inventory` 서비스를 광고하고 이 앱이 앱 서비스 공급자로 식별됩니다. 실제 서비스가 백그라운드 작업으로 구현됩니다. 앱 서비스 프로젝트에서 다른 앱에 서비스를 공개합니다. 서비스 이름에 역방향 도메인 이름 스타일을 사용하는 것이 좋습니다.

`xmlns:uap4` 네임스페이스 접두사 및 `uap4:SupportsMultipleInstances` 특성은 Windows SDK 버전 10.0.15063 이상을 대상으로 하는 경우에만 유효합니다. 이전 SDK 버전을 대상으로 하는 경우 안전하게 제거할 수 있습니다.

``` xml
<Package
    ...
    xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    ...
    <Applications>
        <Application Id="AppServiceProvider.App"
          Executable="$targetnametoken$.exe"
          EntryPoint="AppServiceProvider.App">
          ...
          <Extensions>
            <uap:Extension Category="windows.appService" EntryPoint="MyAppService.Inventory">
              <uap3:AppService Name="com.microsoft.inventory" uap4:SupportsMultipleInstances="true"/>
            </uap:Extension>
          </Extensions>
          ...
        </Application>
    </Applications>
```

`Category` 특성을 통해이 응용 프로그램을 앱 서비스 공급자로 식별 합니다.

`EntryPoint` 특성 다음 구현할 것은 서비스를 구현 하는 정규화 된 네임 스페이스 클래스를 식별 합니다.

`SupportsMultipleInstances` 는 앱 서비스는 호출 될 때마다 실행할지 새 프로세스에서 특성 나타냅니다. 필수는 아니지만 기능이 필요 하 고 10.0.15063 대상으로 하는 경우 사용할 수는이 SDK (**Windows 10 크리에이터 스 업데이트**) 이상. 마찬가지로 앞에 `uap4` 네임스페이스가 붙습니다.

## <a name="create-the-app-service"></a>앱 서비스 만들기

1.  앱 서비스를 백그라운드 작업으로 구현할 수 있습니다. 이렇게 하면 포그라운드 응용 프로그램이 다른 응용 프로그램에서 앱 서비스를 호출할 수 있습니다. 백그라운드 작업으로 앱 서비스를 만들려면 새 Windows 런타임 구성 요소 프로젝트를 솔루션에 추가 합니다 (**파일 &gt; 추가 &gt; 새 프로젝트**) **MyAppService**라는 합니다. **새 프로젝트 추가** 대화 상자에서 **설치 된 gt_ Visual C# gt_ Windows 런타임 구성 요소 (유니버설 Windows)을**선택 합니다.
2.  **AppServiceProvider** 프로젝트에서 새 **MyAppService** 프로젝트에 대 한 프로젝트-프로젝트 참조를 추가 합니다 ( **솔루션 탐색기**에서 마우스 오른쪽 단추로 클릭 **AppServiceProvider** 프로젝트 gt_ **추가**  >  ** 참조** > **프로젝트** > **솔루션**을 선택 **MyAppService** > **확인**). 참조를 추가하지 않으면 앱 서비스가 런타임 시 연결되지 않으므로 이 단계가 매우 중요합니다.
3.  **MyAppService** 프로젝트에 다음 **using** 문을 **Class1.cs**의 맨 위에 추가 합니다.
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  **Class1.cs **Inventory.cs**** 하 고 **Class1** 에 대 한 **인벤토리**라는 새 백그라운드 작업 클래스 스텁 코드로 바꿉니다.

    ```cs
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get a deferral so that the service isn't terminated.
            this.backgroundTaskDeferral = taskInstance.GetDeferral();

            // Associate a cancellation handler with the background task.
            taskInstance.Canceled += OnTaskCanceled;

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // This function is called when the app service receives a request.
        }

        private void OnTaskCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            if (this.backgroundTaskDeferral != null)
            {
                // Complete the service deferral.
                this.backgroundTaskDeferral.Complete();
            }
        }
    }
    ```

    이 클래스에서 앱 서비스가 작업을 수행합니다.

    **실행** 하면 백그라운드 작업이 만들어질 때 호출 됩니다. 백그라운드 작업은 **Run**이 완료되고 나면 종료되므로 백그라운드 작업이 요청을 계속 처리하도록 코드가 지연됩니다. 백그라운드 작업으로 구현된 앱 서비스는 호출을 받은 후 약 30초 동안 다시 호출되거나 지연을 사용하지 않는 한 활성 상태를 유지합니다. 앱 서비스가 호출자와 동일한 프로세스에 구현되면 앱 서비스의 수명은 호출자의 수명과 연결됩니다.

    앱 서비스의 수명은 호출자에 따라 달라집니다.

    * 호출자가 포그라운드 이면의 앱 서비스 수명은 호출자와 동일 합니다.
    * 호출자가 백그라운드 이면 앱 서비스는 30 초간 실행 합니다. 지연을 사용하면 일회에 한해 5초가 추가로 제공됩니다.

    **OnTaskCanceled** 작업이 취소 될 때 호출 됩니다. 클라이언트 앱 삭제 [AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/dn921704), 클라이언트 앱이 일시 중단, OS가 종료 또는 절전 상태 이거나 OS 리소스가 작업을 실행할 때 작업 취소 됩니다.

## <a name="write-the-code-for-the-app-service"></a>앱 서비스의 코드 작성

**OnRequestReceived** 앱 서비스의 코드가 이동 됩니다. **MyAppService**의 **Inventory.cs** 에서 **OnRequestReceived** 스텁을이 예의 코드로 바꿉니다. 이 코드에서는 인벤토리 항목의 인덱스를 가져온 다음 지정된 인벤토리 항목의 이름과 가격을 검색하기 위해 명령 문자열과 함께 서비스에 전달합니다. 여러분의 프로젝트에 대해 오류 처리 코드를 추가합니다.

```cs
private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below to respond to the message
    // and we don't want this call to get canceled while we are waiting.
    var messageDeferral = args.GetDeferral();

    ValueSet message = args.Request.Message;
    ValueSet returnData = new ValueSet();

    string command = message["Command"] as string;
    int? inventoryIndex = message["ID"] as int?;

    if (inventoryIndex.HasValue &&
        inventoryIndex.Value >= 0 &&
        inventoryIndex.Value < inventoryItems.GetLength(0))
    {
        switch (command)
        {
            case "Price":
            {
                returnData.Add("Result", inventoryPrices[inventoryIndex.Value]);
                returnData.Add("Status", "OK");
                break;
            }

            case "Item":
            {
                returnData.Add("Result", inventoryItems[inventoryIndex.Value]);
                returnData.Add("Status", "OK");
                break;
            }

            default:
            {
                returnData.Add("Status", "Fail: unknown command");
                break;
            }
        }
    }
    else
    {
        returnData.Add("Status", "Fail: Index out of range");
    }

    try
    {
        // Return the data to the caller.
        await args.Request.SendResponseAsync(returnData);
    }
    catch (Exception e)
    {
        // Your exception handling code here.
    }
    finally
    {
        // Complete the deferral so that the platform knows that we're done responding to the app service call.
        // Note for error handling: this must be called even if SendResponseAsync() throws an exception.
        messageDeferral.Complete();
    }
}
```

이 예에서 [SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722) 호출 awaitable 메서드 때문에 **OnRequestReceived** 가 **비동기식** note 합니다.

서비스가 **OnRequestReceived** 처리기에서 **비동기** 메서드를 사용할 수 있도록 지연을 가져옵니다. 메시지 처리를 완료할 때까지 **OnRequestReceived**에 대한 호출이 완료되지 않게 합니다.  [SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722)는 결과를 호출자에게 보냅니다. **SendResponseAsync**에서는 호출이 완료되어도 신호를 보내지 않습니다. 지연이 완료되어야 [SendMessageAsync](https://msdn.microsoft.com/library/windows/apps/dn921712)에 **OnRequestReceived**가 완료되었다는 신호를 보냅니다. **SendResponseAsync** 예외가 발생 하는 경우에 지연을 완료 해야 하므로 **SendResponseAsync** 에 대 한 호출을 try/finally 블록에서 래핑됩니다.

앱 서비스 [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) 개체를 사용 하 여 정보를 교환 합니다. 전달할 수 있는 데이터의 크기는 시스템 리소스를 통해서만 제한될 수 있습니다. **ValueSet**에서 사용할 사전 정의된 키가 없습니다. 앱 서비스의 프로토콜을 정의하는 데 사용할 키 값을 결정해야 합니다. 이 프로토콜을 염두에 두고 호출자를 작성해야 합니다. 이 예제에서는 `Command`라는 키를 선택했습니다. 이 키의 값을 통해 앱 서비스에서 인벤토리 항목의 이름을 제공할지 아니면 값을 제공할지를 나타냅니다. 인벤토리 이름의 색인은 `ID` 키에 저장됩니다. 반환 값은 `Result` 키에 저장됩니다.

앱 서비스에 대한 호출의 성공 여부를 표시하기 위해 [AppServiceClosedStatus](https://msdn.microsoft.com/library/windows/apps/dn921703) enum이 호출자에게 반환됩니다. 앱 서비스 호출이 실패할 수 있는 예로는 리소스가 초과되어 OS에서 서비스 끝점을 중단하는 경우를 들 수 있습니다. [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131)를 통해 추가 오류 정보를 반환할 수 있습니다. 이 예제에서는 `Status`라는 키를 사용하여 호출자에게 자세한 오류 정보를 반환합니다.

[SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722)에 대한 호출을 통해 호출자에게 [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131)를 반환합니다.

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>서비스 앱 배포 및 패키지 패밀리 이름 가져오기

앱 서비스 공급자를 배포 해야 클라이언트에서 호출할 수 있습니다. 또한이 메서드를 호출 하기 위해 앱 서비스의 패키지 패밀리 이름을 해야 합니다.

**AppServiceProvider** 프로젝트에서 [Windows.ApplicationModel.Package.Current.Id.FamilyName](https://msdn.microsoft.com/library/windows/apps/br224670) (예를 들어 **에서 **앱** 생성자에서에서 호출 하는 앱 서비스 응용 프로그램의 패키지 패밀리 이름을 가져오는 한 가지 방법은 App.xaml.cs**) 결과 기록해 둡니다. Visual Studio에서 **AppServiceProvider** 를 실행 하려면 **솔루션 탐색기** 창에서 시작 프로젝트로 설정 하 고 프로젝트를 실행 합니다.

패키지 패밀리 이름을 가져오는 또 다른 방법은 솔루션을 배포 하는 (**빌드 &gt; 솔루션 배포**) 고 **출력** 창에 전체 패키지 이름을 기록 (**보기 &gt; 출력**). 패키지 이름을 파생 시키려면 **출력** 창의 문자열에서 플랫폼 정보를 제거 해야 합니다. 예를 들어 전체 패키지 이름이 **출력** 창에 보고 하는 경우는 다음과 같습니다.

`Microsoft.SDKSamples.AppServicesProvider.CPP_1.0.0.0_x86__8wekyb3d8bbwe`

추출 다음 `1.0.0.0\_x86\_\_`, 패키지 패밀리 이름으로 다음 종료 합니다.

`Microsoft.SDKSamples.AppServicesProvider.CPP_8wekyb3d8bbwe`

## <a name="write-a-client-to-call-the-app-service"></a>앱 서비스를 호출하는 클라이언트 작성

1.  **파일 &gt; 추가 &gt; 새 프로젝트**를 선택하여 솔루션에 Windows 유니버설 앱 프로젝트를 추가합니다. **새 프로젝트 추가** 대화 상자에서 **설치 된 gt_ Visual C# gt_ 빈 앱 (유니버설 Windows)를** 선택 하 고 **ClientApp**이름을 지정 합니다.

2.  **ClientApp** 프로젝트에서 다음 **using** 문을 **MainPage.xaml.cs**의 맨 위에 추가 합니다.
    ```cs
    using Windows.ApplicationModel.AppService;
    ```

3.  **TextBox** 와 **MainPage.xaml**하는 단추를 호출 하는 텍스트 상자를 추가 합니다.

4.  단추 추가 **button_Click**이라는 단추에 대 한 처리기를 클릭 하 고 **비동기** 키워드 단추 처리기의 서명에 추가 합니다.

5. 단추 클릭 처리기의 스텁을 다음 코드로 바꿉니다. `inventoryService` 필드 선언을 포함해야 합니다.
    ```cs
   private AppServiceConnection inventoryService;

   private async void button_Click(object sender, RoutedEventArgs e)
   {
       // Add the connection.
       if (this.inventoryService == null)
       {
           this.inventoryService = new AppServiceConnection();

           // Here, we use the app service name defined in the app service 
           // provider's Package.appxmanifest file in the <Extension> section.
           this.inventoryService.AppServiceName = "com.microsoft.inventory";

           // Use Windows.ApplicationModel.Package.Current.Id.FamilyName 
           // within the app service provider to get this value.
           this.inventoryService.PackageFamilyName = "Replace with the package family name";

           var status = await this.inventoryService.OpenAsync();

           if (status != AppServiceConnectionStatus.Success)
           {
               textBox.Text= "Failed to connect";
               this.inventoryService = null;
               return;
           }
       }

       // Call the service.
       int idx = int.Parse(textBox.Text);
       var message = new ValueSet();
       message.Add("Command", "Item");
       message.Add("ID", idx);
       AppServiceResponse response = await this.inventoryService.SendMessageAsync(message);
       string result = "";

       if (response.Status == AppServiceResponseStatus.Success)
       {
           // Get the data  that the service sent to us.
           if (response.Message["Status"] as string == "OK")
           {
               result = response.Message["Result"] as string;
           }
       }

       message.Clear();
       message.Add("Command", "Price");
       message.Add("ID", idx);
       response = await this.inventoryService.SendMessageAsync(message);

       if (response.Status == AppServiceResponseStatus.Success)
       {
           // Get the data that the service sent to us.
           if (response.Message["Status"] as string == "OK")
           {
               result += " : Price = " + response.Message["Result"] as string;
           }
       }

       textBox.Text = result;
   }
   ```
    
    `this.inventoryService.PackageFamilyName = "Replace with the package family name";` 줄의 패키지 패밀리 이름을 위의 [서비스 앱 배포 및 패키지 패밀리 이름 가져오기](#deploy-the-service-app-and-get-the-package-family-name)에서 얻은 **AppServiceProvider** 프로젝트의 패키지 패밀리 이름으로 바꿉니다.

    > [!NOTE]
    > 변수에 배치 하는 것이 아니라 문자열 리터럴을에 붙여 있는지 확인 합니다. 변수를 사용 하는 경우에 작동 하지 않습니다.

    코드는 먼저 앱 서비스와 연결합니다. 연결은 `this.inventoryService`를 삭제할 때까지 열린 상태로 유지됩니다. 앱 서비스 이름과 일치 해야 합니다 `AppService` 요소의 `Name` 특성 **AppServiceProvider** 프로젝트의 **Package.appxmanifest** 파일에 추가 합니다. 이 예제에서는 `<uap3:AppService Name="com.microsoft.inventory"/>`입니다.

    라는 [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) `message` 우리는 앱 서비스에 보낼 명령을 지정할 수 만들어집니다. 예제 앱 서비스에서는 명령을 사용하여 수행할 두 가지 작업이 나타나게 됩니다. 클라이언트 앱의 텍스트 상자에서 인덱스를 가져온 및 서비스를 호출 하는 `Item` 명령 항목 설명을 가져오도록입니다. 그런 다음 항목 가격을 가져오도록 `Price` 명령을 사용하여 호출합니다. 단추 텍스트는 결과로 설정됩니다.

    [AppServiceResponseStatus](https://msdn.microsoft.com/library/windows/apps/dn921724)는 운영 체제에서 앱 서비스에 호출을 연결할 수 있었는지 여부만 나타내므로 앱 서비스에서 수신한 [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131)의 `Status` 키를 확인하여 요청을 수행할 수 있었는지 확인합니다.

6. **ClientApp** 프로젝트를 시작 프로젝트로 설정 ( **솔루션 탐색기**에서 마우스 오른쪽 단추로 클릭 > **시작 프로젝트로 설정**) 솔루션을 실행 합니다. 입력란에 숫자 1을 입력하고 단추를 클릭합니다. 서비스에서 “의자 : 가격 = 88.99”가 반환되어야 합니다.

    ![의자 가격=88.99를 표시하는 샘플 앱](images/appserviceclientapp.png)

앱 서비스 호출이 실패 하면 **ClientApp** 프로젝트에서 다음을 확인 합니다.

1.  인벤토리 서비스 연결에 할당 된 패키지 제품군 이름이 **AppServiceProvider** 앱의 패키지 패밀리 이름과 일치 하는지 확인 합니다. 사용 하 여 **button\_Click** 줄이 표시 `this.inventoryService.PackageFamilyName = "...";`.
2.  **Button\_Click**인벤토리 서비스 연결에 할당 된 앱 서비스 이름이 **AppServiceProvider**의 **Package.appxmanifest** 파일에서 앱 서비스 이름과 일치 하는지 확인 합니다. `this.inventoryService.AppServiceName = "com.microsoft.inventory";`를 참조하세요.
3.  **AppServiceProvider** 앱이 배포 되어 있는지 확인 합니다. ( **솔루션 탐색기**에서 솔루션을 마우스 오른쪽 단추로 클릭 하 고 **솔루션 배포**를 선택).

## <a name="debug-the-app-service"></a>앱 서비스 디버그

1.  앱 서비스 공급자 앱을 배포해야 서비스를 호출할 수 있으므로 디버깅 전에 솔루션이 배포되도록 합니다. (Visual Studio에서 **빌드 &gt; 솔루션 배포**).
2.  **솔루션 탐색기**에서 **AppServiceProvider** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다. **디버그** 탭에서 **시작 작업**을 **실행하지 않지만 시작되면 내 코드 디버그**로 변경합니다. (C++를 사용하여 앱 서비스 공급자를 구현하는 경우 **디버깅** 탭에서 **응용 프로그램 시작**을 **아니요**로 변경합니다).
3.  **MyAppService** 프로젝트에서 **Inventory.cs** 파일에서 **OnRequestReceived**에 중단점을 설정 합니다.
4.  **AppServiceProvider** 프로젝트 시작 프로젝트 여야 하 고 **f5 키**를 설정 합니다.
5.  (Visual Studio)가 아니라 시작 메뉴에서 **ClientApp** 을 시작 합니다.
6.  입력란에 숫자 1을 입력하고 단추를 누릅니다. 디버거가 앱 서비스의 중단점에서 앱 서비스 호출을 중단합니다.

## <a name="debug-the-client"></a>클라이언트 디버그

1.  이전 단계의 지침에 따라 앱 서비스를 호출하는 클라이언트를 디버그합니다.
2.  시작 메뉴에서 **ClientApp** 을 실행 합니다.
3.  **ClientApp.exe** 프로세스 ( **ApplicationFrameHost.exe** 프로세스가 아님)에 디버거를 연결 합니다. (Visual Studio에서 **디버그 &gt; 프로세스에 추가...** 선택)
4.  **ClientApp** 프로젝트에서 **button\_Click**에 중단점을 설정 합니다.
5.  **ClientApp** 의 텍스트 상자에 숫자 1을 입력 하 고 단추를 클릭 하면 클라이언트와 앱 서비스 둘 다의 중단점이 적중 됩니다.

## <a name="general-app-service-troubleshooting"></a>일반적인 앱 서비스 문제 해결

앱 서비스에 연결 하려고 **AppUnavailable** 상태를 발생 하는 경우 다음 사항을 확인 합니다.

- 앱 서비스 공급자 프로젝트 앱 및 서비스 프로젝트가 배포되었는지 확인합니다. 클라이언트를 실행하기 전에 둘 다 배포되어 있어야 하며, 그렇지 않으면 클라이언트가 연결할 대상이 없습니다. Visual Studio에서 **빌드** > **솔루션 배포**를 사용하여 배포할 수 있습니다.
- **솔루션 탐색기**에서 앱 서비스 공급자 프로젝트 앱 서비스를 구현 하는 프로젝트에 대 한 프로젝트 간 참조에 있는지 확인 합니다.
- 되어 있는지 확인 합니다 `<Extensions>` 항목 및 하위 요소에서 [Package.appxmanifest에 앱 서비스 확장 추가](#appxmanifest)위에 지정 된 앱 서비스 공급자 프로젝트에 속한 **Package.appxmanifest** 파일에 추가 되었습니다.
- 앱 서비스 공급자를 호출 하는 클라이언트에서 [AppServiceConnection.AppServiceName](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.appservicename) 문자열 일치 하는지 확인 합니다 `<uap3:AppService Name="..." />` 앱 서비스 공급자 프로젝트의 **Package.appxmanifest** 파일에 지정 합니다.
- [AppServiceConnection.PackageFamilyName](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.packagefamilyname) [Package.appxmanifest에 앱 서비스 확장 추가](#appxmanifest) 에 위에 지정 된 대로 앱 서비스 공급자 구성 요소의 패키지 패밀리 이름과 일치 하는지 확인 합니다.
- 이 예의 등의 프로세서 아웃 앱 서비스에 대 한 유효성을 검사 하는 `EntryPoint` 에 지정 된 합니다 `<uap:Extension ...>` 앱 서비스 공급자 프로젝트의 **Package.appxmanifest** 파일의 요소 일치 네임 스페이스 및 클래스의 공용 클래스 이름 앱 서비스 프로젝트에서 [IBackgroundTask](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask) 를 구현합니다.

### <a name="troubleshoot-debugging"></a>디버깅 문제 해결

디버거가 앱 서비스 공급자 앱 또는 서비스 프로젝트의 중단점에서 중지하지 않으면 다음 사항을 확인합니다.

- 앱 서비스 공급자 프로젝트 앱 및 서비스 프로젝트가 배포되었는지 확인합니다. 클라이언트를 실행하기 전에 둘 다 배포되어 있어야 합니다. Visual Studio에서 **빌드** > **솔루션 배포**를 사용하여 배포할 수 있습니다.
- 디버깅할 프로젝트가 시작 프로젝트로 설정 되어 있는지 그리고 해당 프로젝트의 디버깅 속성이 **f5 키** 를 누를 때 프로젝트를 실행 하지으로 설정 되어 있는지 확인 합니다. 프로젝트를 마우스 오른쪽 단추로 클릭한 다음 **속성**을 클릭하고 **디버그**(또는 C++에서는 **디버깅**)를 클릭합니다. C#에서 **시작 작업**을 **실행하지 않지만 시작되면 내 코드 디버그**로 변경합니다. C++에서 **응용 프로그램 시작**을 **아니요**로 설정합니다.

## <a name="remarks"></a>설명

이 예제에서는 백그라운드 작업으로 실행되는 앱 서비스를 만들고 다른 앱에서 이 앱을 호출하는 방법을 간략하게 소개합니다. 중요 한 사항에 주의 다음과 같습니다.

* 앱 서비스를 호스트 하는 백그라운드 작업을 만듭니다.
* 추가 합니다 `windows.appService` 앱 서비스 공급자의 **Package.appxmanifest** 파일을 확장 합니다.
* 클라이언트 앱에서에 연결할 수 있도록 앱 서비스 공급자의 패키지 패밀리 이름을 가져옵니다.
* 앱 서비스 프로젝트에 앱 서비스 공급자 프로젝트에서 프로젝트 간 참조를 추가 합니다.
* [Windows.ApplicationModel.AppService.AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/dn921704) 를 사용 하 여 서비스를 호출 합니다.

## <a name="full-code-for-myappservice"></a>MyAppService의 전체 코드

```cs
using System;
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
using Windows.Foundation.Collections;

namespace MyAppService
{
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get a deferral so that the service isn't terminated.
            this.backgroundTaskDeferral = taskInstance.GetDeferral();

            // Associate a cancellation handler with the background task.
            taskInstance.Canceled += OnTaskCanceled;

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // Get a deferral because we use an awaitable API below to respond to the message
            // and we don't want this call to get canceled while we are waiting.
            var messageDeferral = args.GetDeferral();

            ValueSet message = args.Request.Message;
            ValueSet returnData = new ValueSet();

            string command = message["Command"] as string;
            int? inventoryIndex = message["ID"] as int?;

            if (inventoryIndex.HasValue &&
                 inventoryIndex.Value >= 0 &&
                 inventoryIndex.Value < inventoryItems.GetLength(0))
            {
                switch (command)
                {
                    case "Price":
                        {
                            returnData.Add("Result", inventoryPrices[inventoryIndex.Value]);
                            returnData.Add("Status", "OK");
                            break;
                        }

                    case "Item":
                        {
                            returnData.Add("Result", inventoryItems[inventoryIndex.Value]);
                            returnData.Add("Status", "OK");
                            break;
                        }

                    default:
                        {
                            returnData.Add("Status", "Fail: unknown command");
                            break;
                        }
                }
            }
            else
            {
                returnData.Add("Status", "Fail: Index out of range");
            }

            // Return the data to the caller.
            await args.Request.SendResponseAsync(returnData);

            // Complete the deferral so that the platform knows that we're done responding to the app service call.
            // Note for error handling: this must be called even if SendResponseAsync() throws an exception.
            messageDeferral.Complete();
        }


        private void OnTaskCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            if (this.backgroundTaskDeferral != null)
            {
                // Complete the service deferral.
                this.backgroundTaskDeferral.Complete();
            }
        }
    }
}
```

## <a name="related-topics"></a>관련 항목

* [앱 서비스가 호스트 앱과 동일한 프로세스에서 실행되도록 변환](convert-app-service-in-process.md)
* [백그라운드 작업을 사용하여 앱 지원](support-your-app-with-background-tasks.md)
* [앱 서비스 코드 샘플(C#, C++ 및 VB)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)
