---
author: TylerMSFT
title: 앱 서비스 만들기 및 사용
description: 다른 UWP 앱에 서비스를 제공할 수 있는 UWP(유니버설 Windows 플랫폼) 앱을 작성하는 방법과 해당 서비스를 사용하는 방법을 알아봅니다.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: 앱 간 통신, 프로세스 간 통신, IPC, 백그라운드 메시징, 백그라운드 통신, 앱, 앱 서비스
ms.author: twhitney
ms.date: 09/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7475ae8db964b23de89488d883c135158ea20e74
ms.sourcegitcommit: 5dda01da4702cbc49c799c750efe0e430b699502
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/21/2018
ms.locfileid: "4110677"
---
# <a name="create-and-consume-an-app-service"></a>앱 서비스 만들기 및 사용

앱 서비스는 다른 UWP 앱에 서비스를 제공하는 UWP 앱입니다. 장치에서 앱 서비스는 웹 서비스와 비슷합니다. 앱 서비스는 호스트 앱에서 배경 작업으로 실행되고 다른 앱에 서비스를 제공할 수 있습니다. 예를 들어, 앱 서비스는 다른 앱에서 사용할 수 있는 바코드 스캐너 서비스를 제공할 수 있습니다. 또는 앱의 Enterprise Suite에 해당 제품군의 다른 앱에서 사용할 수 있는 일반 맞춤법 검사 앱 서비스가 있습니다.  Windows 10, 버전 1607부터 앱 서비스를 통해 앱이 동일한 장치에서 호출할 수 있는 UI 없는 서비스를 원격 장치에 만들 수 있습니다. 

Windows10 버전 1607부터 호스트 앱과 같은 프로세스에서 실행되는 앱 서비스를 만들 수 있습니다. 이 문서는 별도 백그라운드 프로세스에서 실행되는 앱 서비스 만들기 및 사용에 중점을 둡니다. 공급자와 같은 프로세스에서 앱 서비스를 실행하는 방법에 대한 자세한 내용은 [앱 서비스가 호스트 앱과 동일한 프로세스에서 실행되도록 변환](convert-app-service-in-process.md)을 참조하세요.

앱 서비스 코드 샘플은 [유니버설 Windows 플랫폼(UWP) 앱 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)을 참조하세요.

## <a name="create-a-new-app-service-provider-project"></a>새 앱 서비스 공급자 프로젝트 만들기

이 방법에서는 편의상 모두를 한 솔루션으로 만듭니다.

-   Microsoft Visual Studio에서 UWP 앱 프로젝트를 만들고 이름을 **AppServiceProvider**로 지정합니다. **새 프로젝트** 대화 상자에서 **템플릿 &gt; 기타 언어 &gt; Visual C# &gt; Windows &gt; Windows 유니버설 &gt; 비어 있는 앱(Windows 유니버설)** 을 선택합니다. 이렇게 하면 다른 UWP 앱에서 앱 서비스 사용할 수 있게 됩니다.
-   프로젝트의 **대상 버전**을 선택하라는 메시지가 나타나면 **10.0.14393** 이상을 선택합니다. 새로운 `SupportsMultipleInstances` 특성을 사용하려면 Visual Studio 2017을 사용해야 하고 대상 버전이 **10.0.15063**(**Windows 10 크리에이터스 업데이트**) 이상이어야 합니다.

<span id="appxmanifest"/>

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>package.appxmanifest에 앱 서비스 확장 추가

AppServiceProvider 프로젝트의 Package.appxmanifest 파일에서 `&lt;Application&gt;` 요소에 다음과 같은 AppService 확장 기능을 추가합니다. 이 예제에서는 `com.Microsoft.Inventory` 서비스를 광고하고 이 앱이 앱 서비스 공급자로 식별됩니다. 실제 서비스가 백그라운드 작업으로 구현됩니다. 앱 서비스 프로젝트에서 다른 앱에 서비스를 공개합니다. 서비스 이름에 역방향 도메인 이름 스타일을 사용하는 것이 좋습니다.

`xmlns:uap4` 네임스페이스 접두사 및 `uap4:SupportsMultipleInstances` 특성은 Windows SDK 버전 10.0.15063 이상을 대상으로 하는 경우에만 유효합니다. 이전 SDK 버전을 대상으로 하는 경우 안전하게 제거할 수 있습니다.

``` xml
<Package
    ...
    xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    ...
    <Applications>
        <Application Id="AppServicesProvider.App"
          Executable="$targetnametoken$.exe"
          EntryPoint="AppServicesProvider.App">
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

**Category** 특성은 이 응용 프로그램을 앱 서비스 공급자로 식별합니다.

**EntryPoint** 특성은 서비스를 구현하는 정규화된 네임스페이스 클래스를 식별하며, 이 예에서 다음으로 이 클래스를 구현할 것입니다.

**SupportsMultipleInstances** 특성은 앱 서비스가 호출될 때마다 새 프로세스에서 실행되어야 함을 나타냅니다. 이 특성은 필수는 아니지만 이 기능이 필요하고 `10.0.15063` SDK(**Windows 10 크리에이터스 업데이트**) 이상을 대상으로 하는 경우에 사용할 수 있습니다. 마찬가지로 앞에 `uap4` 네임스페이스가 붙습니다.

## <a name="create-the-app-service"></a>앱 서비스 만들기

1.  앱 서비스를 백그라운드 작업으로 구현할 수 있습니다. 이렇게 하면 포그라운드 응용 프로그램이 다른 응용 프로그램에서 앱 서비스를 호출할 수 있습니다. 앱 서비스를 백그라운드 작업으로 만들려면 MyAppService라는 솔루션에 새 Windows 런타임 구성 요소 프로젝트를 추가합니다(**파일 &gt; 추가 &gt; 새 프로젝트**). (**새 프로젝트 추가** 대화 상자에서 **설치됨 &gt; 기타 언어 &gt; Visual C# &gt; Windows &gt; Windows Universal &gt; Windows 런타임 구성 요소(Windows Universal)** 선택)
2.  **AppServiceProvider** 프로젝트에서 프로젝트 간 참조를 새 **MyAppService** 프로젝트에 추가합니다(솔루션 탐색기에서 **AppServiceProvider** 프로젝트 > **추가** > **참조** > **프로젝트** > **솔루션**을 마우스 오른쪽 단추로 클릭하고 **MyAppService** > **확인** 선택). 참조를 추가하지 않으면 앱 서비스가 런타임 시 연결되지 않으므로 이 단계가 매우 중요합니다.
3.  MyappService 프로젝트에서 다음 **using** 문을 Class1.cs의 맨 위에 추가합니다.
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  **Class1**의 스텁 코드를 **Inventory**: 라는 새 백그라운드 작업 클래스로 바꿉니다.

    ```cs
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn't terminated.
            taskInstance.Canceled += OnTaskCanceled; // Associate a cancellation handler with the background task.

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // This function is called when the app service receives a request
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

    백그라운드 작업을 만들면 **Run()** 이 호출됩니다. 백그라운드 작업은 **Run**이 완료되고 나면 종료되므로 백그라운드 작업이 요청을 계속 처리하도록 코드가 지연됩니다. 백그라운드 작업으로 구현된 앱 서비스는 호출을 받은 후 약 30초 동안 다시 호출되거나 지연을 사용하지 않는 한 활성 상태를 유지합니다. 앱 서비스가 호출자와 동일한 프로세스에 구현되면 앱 서비스의 수명은 호출자의 수명과 연결됩니다.

    앱 서비스의 수명은 호출자에 따라 달라집니다.

    1. 호출자가 포그라운드이면 앱 서비스 수명은 호출자와 동일합니다.
    2. 호출자가 백그라운드이면 앱 서비스는 30초간 실행됩니다. 지연을 사용하면 일회에 한해 5초가 추가로 제공됩니다.

    작업이 취소되면 **OnTaskCanceled()** 가 호출됩니다. 클라이언트 앱에서 [**AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704)을 삭제하거나 클라이언트 앱이 일시 중단되거나 OS가 종료 또는 절전 상태이거나 OS에 작업을 실행할 리소스가 없는 경우 이 작업이 취소됩니다.

## <a name="write-the-code-for-the-app-service"></a>앱 서비스의 코드 작성

**OnRequestReceived()** 로 앱 서비스의 코드가 이동됩니다. MyAppService의 Class1.cs에 있는 스텁 **OnRequestReceived()** 를 이 예의 코드로 바꿉니다. 이 코드에서는 인벤토리 항목의 인덱스를 가져온 다음 지정된 인벤토리 항목의 이름과 가격을 검색하기 위해 명령 문자열과 함께 서비스에 전달합니다. 여러분의 프로젝트에 대해 오류 처리 코드를 추가합니다.

```cs
private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below to respond to the message
    // and we don't want this call to get cancelled while we are waiting.
    var messageDeferral = args.GetDeferral();

    ValueSet message = args.Request.Message;
    ValueSet returnData = new ValueSet();

    string command = message["Command"] as string;
    int? inventoryIndex = message["ID"] as int?;

    if ( inventoryIndex.HasValue &&
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
        await args.Request.SendResponseAsync(returnData); // Return the data to the caller.
    }
    catch (Exception e)
    {
        // your exception handling code here
    }
    finally
    {
        // Complete the deferral so that the platform knows that we're done responding to the app service call.
        // Note for error handling: this must be called even if SendResponseAsync() throws an exception.
        messageDeferral.Complete();
    }
}
```

이 예에서는 [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722)에 대해 awaitable 메서드를 호출하므로 **OnRequestReceived()** 가 **async**입니다.

서비스가 OnRequestReceived 처리기에서 **async** 메서드를 사용할 수 있도록 지연됩니다. 메시지 처리를 완료할 때까지 **OnRequestReceived**에 대한 호출이 완료되지 않게 합니다.  [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722)는 결과를 호출자에게 보냅니다. **SendResponseAsync**에서는 호출이 완료되어도 신호를 보내지 않습니다. 지연이 완료되어야 [**SendMessageAsync**](https://msdn.microsoft.com/library/windows/apps/dn921712)에 **OnRequestReceived**가 완료되었다는 신호를 보냅니다. **SendResponseAsync()** 에서 예외가 발생된 경우에도 지연을 완료해야 하므로 **SendResponseAsync()** 에 대한 호출이 try/finally 블록에서 래핑됩니다.

앱 서비스에서는 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)를 사용하여 정보를 교환합니다. 전달할 수 있는 데이터의 크기는 시스템 리소스를 통해서만 제한될 수 있습니다. **ValueSet**에서 사용할 사전 정의된 키가 없습니다. 앱 서비스의 프로토콜을 정의하는 데 사용할 키 값을 결정해야 합니다. 이 프로토콜을 염두에 두고 호출자를 작성해야 합니다. 이 예제에서는 `Command`라는 키를 선택했습니다. 이 키의 값을 통해 앱 서비스에서 인벤토리 항목의 이름을 제공할지 아니면 값을 제공할지를 나타냅니다. 인벤토리 이름의 색인은 `ID` 키에 저장됩니다. 반환 값은 `Result` 키에 저장됩니다.

앱 서비스에 대한 호출의 성공 여부를 표시하기 위해 [**AppServiceClosedStatus**](https://msdn.microsoft.com/library/windows/apps/dn921703) enum이 호출자에게 반환됩니다. 앱 서비스 호출이 실패할 수 있는 예로는 리소스가 초과되어 OS에서 서비스 끝점을 중단하는 경우를 들 수 있습니다. [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)를 통해 추가 오류 정보를 반환할 수 있습니다. 이 예제에서는 `Status`라는 키를 사용하여 호출자에게 자세한 오류 정보를 반환합니다.

[**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722)에 대한 호출을 통해 호출자에게 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)를 반환합니다.

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>서비스 앱 배포 및 패키지 패밀리 이름 가져오기

앱 서비스 공급자 앱을 배포해야 클라이언트에서 호출할 수 있습니다. 앱 서비스 앱을 호출하려면 해당 패키지 패밀리 이름도 필요합니다.

앱 서비스 응용 프로그램의 패키지 패밀리 이름을 가져오는 방법은 **AppServiceProvider** 프로젝트에서(예: App.xaml.cs의 `public App()`에서) [**Windows.ApplicationModel.Package.Current.Id.FamilyName**](https://msdn.microsoft.com/library/windows/apps/br224670)을 호출하고 결과를 기록하는 것뿐입니다. Microsoft Visual Studio에서 AppServiceProvider를 실행하려면 솔루션 탐색기 창에서 시작 프로젝트로 설정한 다음 프로젝트를 실행합니다.

패키지 패밀리 이름을 가져오는 또 다른 방법은 솔루션을 배포(**빌드 &gt; 솔루션 배포**)하고 출력 창에서 전체 패키지 이름을 기록(**보기 &gt; 출력**)하는 것입니다. 패키지 이름을 파생시키려면 출력 창의 문자열에서 플랫폼 정보를 제거해야 합니다. 예를 들어 출력 창에 보고된 전체 패키지 이름이 `Microsoft.SDKSamples.AppServicesProvider.CPP_1.0.0.0_x86__8wekyb3d8bbwe`인 경우 패키지 패밀리 이름으로 `1.0.0.0\_x86\_\_" leaving "Microsoft.SDKSamples.AppServicesProvider.CPP_8wekyb3d8bbwe`를 추출합니다.

## <a name="write-a-client-to-call-the-app-service"></a>앱 서비스를 호출하는 클라이언트 작성

1.  **파일 &gt; 추가 &gt; 새 프로젝트**를 선택하여 솔루션에 Windows 유니버설 앱 프로젝트를 추가합니다. **새 프로젝트 추가** 대화 상자에서 **설치됨 &gt; 기타 언어 &gt; Visual C# &gt; Windows &gt; Windows 유니버설 &gt; 비어 있는 앱(Windows 유니버설)** 을 선택하고 이름을 **ClientApp**으로 지정합니다.
2.  ClientApp 프로젝트에서 다음 **using** 문을 MainPage.xaml.cs의 맨 위에 추가합니다.
    ```cs
    >using Windows.ApplicationModel.AppService;
    ```
3.  MainPage.xaml에 텍스트 상자와 단추를 추가합니다.
4.  단추의 단추 클릭 처리기를 추가하고 **async** 키워드를 단추 처리기의 서명에 추가합니다.
5.  단추 클릭 처리기의 스텁을 다음 코드로 바꿉니다. `inventoryService` 필드 선언을 포함해야 합니다.

   ```cs
   private AppServiceConnection inventoryService;
   private async void button_Click(object sender, RoutedEventArgs e)
   {
       // Add the connection.
       if (this.inventoryService == null)
       {
           this.inventoryService = new AppServiceConnection();

           // Here, we use the app service name defined in the app service provider's Package.appxmanifest file in the <Extension> section.
           this.inventoryService.AppServiceName = "com.microsoft.inventory";

           // Use Windows.ApplicationModel.Package.Current.Id.FamilyName within the app service provider to get this value.
           this.inventoryService.PackageFamilyName = "replace with the package family name";

           var status = await this.inventoryService.OpenAsync();
           if (status != AppServiceConnectionStatus.Success)
           {
               textBox.Text= "Failed to connect";
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
           // Get the data  that the service sent  to us.
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
`this.inventoryService.PackageFamilyName = "replace with the package family name";` 줄의 패키지 패밀리 이름을 위의 [서비스 앱 배포 및 패키지 패밀리 이름 가져오기](#deploy-the-service-app-and-get-the-package-family-name)에서 얻은 **AppServiceProvider** 프로젝트의 패키지 패밀리 이름으로 바꿉니다.

코드는 먼저 앱 서비스와 연결합니다. 연결은 `this.inventoryService`를 삭제할 때까지 열린 상태로 유지됩니다. 앱 서비스 이름은 AppServiceProvider 프로젝트의 Package.appxmanifest 파일에 추가한 **AppService Name** 특성과 일치해야 합니다. 이 예제에서는 `<uap:AppService Name="com.microsoft.inventory"/>`입니다.

앱 서비스에 보낼 명령을 지정할 수 있도록 **message**라는 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)이 만들어집니다. 예제 앱 서비스에서는 명령을 사용하여 수행할 두 가지 작업이 나타나게 됩니다. 클라이언트 앱의 텍스트 상자에서 인덱스를 가져온 다음 항목 설명을 가져오도록 `Item` 명령을 사용하여 서비스를 호출합니다. 그런 다음 항목 가격을 가져오도록 `Price` 명령을 사용하여 호출합니다. 단추 텍스트는 결과로 설정됩니다.

[**AppServiceResponseStatus**](https://msdn.microsoft.com/library/windows/apps/dn921724)는 운영 체제에서 앱 서비스에 호출을 연결할 수 있었는지 여부만 나타내므로 앱 서비스에서 수신한 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)의 `Status` 키를 확인하여 요청을 수행할 수 있었는지 확인합니다.

6.  Visual Studio의 솔루션 탐색기 창에서 ClientApp 프로젝트를 시작 프로젝트로 설정한 다음 솔루션을 실행합니다. 입력란에 숫자 1을 입력하고 단추를 클릭합니다. 서비스에서 “의자 : 가격 = 88.99”가 반환되어야 합니다.

    ![의자 가격=88.99를 표시하는 샘플 앱](images/appserviceclientapp.png)

앱 서비스 호출에 실패하면 ClientApp에서 다음을 확인합니다.

1.  인벤토리 서비스 연결에 할당된 패키지 제품군 이름이 AppServiceProvider 앱의 패키지 제품군 이름과 일치하는지 확인합니다. **button\_Click()**`this.inventoryService.PackageFamilyName = "...";`을 참조하세요.
2.  **button\_Click()** 에서 인벤토리 서비스 연결에 할당된 앱 서비스 이름이 AppServiceProvider의 Package.appxmanifest 파일에 있는 앱 서비스 이름과 일치하는지 확인합니다. `this.inventoryService.AppServiceName = "com.microsoft.inventory";`를 참조하세요.
3.  AppServiceProvider 앱이 배포되었는지 확인합니다(솔루션 탐색기에서 솔루션을 마우스 오른쪽 단추로 클릭하고 **배포** 선택).

## <a name="debug-the-app-service"></a>앱 서비스 디버그

1.  앱 서비스 공급자 앱을 배포해야 서비스를 호출할 수 있으므로 디버깅 전에 솔루션이 배포되도록 합니다. (Visual Studio에서 **빌드 &gt; 솔루션 배포**).
2.  솔루션 탐색기에서 **AppServiceProvider** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다. **디버그** 탭에서 **시작 작업**을 **실행하지 않지만 시작되면 내 코드 디버그**로 변경합니다. (C++를 사용하여 앱 서비스 공급자를 구현하는 경우 **디버깅** 탭에서 **응용 프로그램 시작**을 **아니요**로 변경합니다).
3.  MyAppService 프로젝트의 Class1.cs 파일에서 `OnRequestReceived()`에 중단점을 설정합니다.
4.  AppServiceProvider 프로젝트를 시작 프로젝트가 되도록 설정하고 F5를 누릅니다.
5.  시작 메뉴에서(Visual Studio에서가 아님) ClientApp을 시작합니다.
6.  입력란에 숫자 1을 입력하고 단추를 누릅니다. 디버거가 앱 서비스의 중단점에서 앱 서비스 호출을 중단합니다.

## <a name="debug-the-client"></a>클라이언트 디버그

1.  이전 단계의 지침에 따라 앱 서비스를 호출하는 클라이언트를 디버그합니다.
2.  시작 메뉴에서 ClientApp을 실행합니다.
3.  ClientApp.exe 프로세스(ApplicationFrameHost.exe 프로세스가 아님)에 디버거를 연결합니다. (Visual Studio에서 **디버그 &gt; 프로세스에 추가...** 선택)
4.  ClientApp 프로젝트에서 **button_\Click()** 에 중단점을 설정합니다.
5.  ClientApp의 텍스트 상자에 숫자 1을 입력하고 단추를 클릭하면 클라이언트와 앱 서비스 둘 다의 중단점이 적중됩니다.

## <a name="general-app-service-troubleshooting"></a>일반적인 앱 서비스 문제 해결 ##

앱 서비스에 연결하려고 하면 **AppUnavailable** 상태가 되는 경우 다음 사항을 확인합니다.

- 앱 서비스 공급자 프로젝트 앱 및 서비스 프로젝트가 배포되었는지 확인합니다. 클라이언트를 실행하기 전에 둘 다 배포되어 있어야 하며, 그렇지 않으면 클라이언트가 연결할 대상이 없습니다. Visual Studio에서 **빌드** > **솔루션 배포**를 사용하여 배포할 수 있습니다.
- 솔루션 탐색기에서 앱 서비스 공급자 프로젝트에 앱 서비스를 구현하는 프로젝트에 대한 프로젝트-프로젝트 참조가 있는지 확인합니다.
- `<Extensions>` 항목과 그 자녀 요소가 위의 [package.appxmanifest에 앱 서비스 확장 추가](#appxmanifest)에서 지정된 앱 서비스 공급자 프로젝트에 속한 Package.appxmanifest 파일에 추가되었는지 확인합니다.
- 클라이언트에서 앱 서비스 공급자를 호출하는 `AppServiceConnection.AppServiceName` 문자열이 앱 서비스 공급자 프로젝트의 Package.appxmanifest 파일에 지정된 `<uap3:AppService Name="..." />`와 일치하는지 확인합니다.
- `AppServiceConnection.PackageFamilyName`이 위의 [package.appxmanifest에 앱 서비스 확장 추가](#appxmanifest)에서 지정한 앱 서비스 공급자 구성 요소의 패키지 패밀리 이름과 일치하는지 확인합니다.
- 이 예의 서비스 같은 out-of-proc 앱 서비스의 경우 앱 서비스 공급자 프로젝트의 Package.appxmanifest 파일의 `<uap:Extension ...>` 요소에 지정된 `EntryPoint`가 앱 서비스 프로젝트의 `IBackgroundTask`를 구현하는 공용 클래스의 네임스페이스 및 클래스 이름과 일치하는지 확인합니다.

### <a name="troubleshoot-debugging"></a>디버깅 문제 해결

디버거가 앱 서비스 공급자 앱 또는 서비스 프로젝트의 중단점에서 중지하지 않으면 다음 사항을 확인합니다.

- 앱 서비스 공급자 프로젝트 앱 및 서비스 프로젝트가 배포되었는지 확인합니다. 클라이언트를 실행하기 전에 둘 다 배포되어 있어야 합니다. Visual Studio에서 **빌드** > **솔루션 배포**를 사용하여 배포할 수 있습니다.
- 디버깅할 프로젝트가 시작 프로젝트로 설정되어야 하며 해당 프로젝트의 디버깅 속성이 F5 키를 눌러도 프로젝트를 실행하지 않도록 설정되어야 합니다. 프로젝트를 마우스 오른쪽 단추로 클릭한 다음 **속성**을 클릭하고 **디버그**(또는 C++에서는 **디버깅**)를 클릭합니다. C#에서 **시작 작업**을 **실행하지 않지만 시작되면 내 코드 디버그**로 변경합니다. C++에서 **응용 프로그램 시작**을 **아니요**로 설정합니다.

## <a name="remarks"></a>설명

이 예제에서는 백그라운드 작업으로 실행되는 앱 서비스를 만들고 다른 앱에서 이 앱을 호출하는 방법을 간략하게 소개합니다. 기억해야 할 핵심 내용은 앱 서비스를 호스트하는 백그라운드 작업을 만들고, windows.appservice 확장 기능을 앱 서비스 공급자 앱의 Package.appxmanifest 파일에 추가하고, 클라이언트 앱에서 연결할 수 있도록 앱 서비스 공급자 앱의 패키지 패밀리 이름을 확보하고, 앱 서비스 공급자 프로젝트의 프로젝트-프로젝트 참조를 앱 서비스 프로젝트에 추가하고, [**Windows.ApplicationModel.AppService.AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704)을 사용하여 서비스를 호출하는 방법입니다.

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
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn't terminated.
            taskInstance.Canceled += OnTaskCanceled; // Associate a cancellation handler with the background task.

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // Get a deferral because we use an awaitable API below to respond to the message
            // and we don't want this call to get cancelled while we are waiting.
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

            await args.Request.SendResponseAsync(returnData); // Return the data to the caller.
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
