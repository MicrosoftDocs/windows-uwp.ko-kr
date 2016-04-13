---
title: 앱 서비스 만들기 및 사용
description: 다른 UWP 앱에 서비스를 제공할 수 있는 UWP(유니버설 Windows 플랫폼)를 작성하는 방법과 이러한 서비스를 사용하는 방법을 알아봅니다.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
---

# 앱 서비스 만들기 및 사용


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


다른 UWP 앱에 서비스를 제공할 수 있는 UWP(유니버설 Windows 플랫폼)를 작성하는 방법과 이러한 서비스를 사용하는 방법에 대해 알아봅니다.

## 새 앱 서비스 공급자 프로젝트 만들기


이 방법에서는 편의상 모두를 한 솔루션으로 만듭니다.

-   Microsoft Visual Studio 2015에서 UWP 앱 프로젝트를 만들고 AppServiceProvider로 이름을 지정합니다. **새 프로젝트** 대화 상자에서 **템플릿 &gt; 기타 언어 &gt; Visual C# &gt; Windows &gt; Windows 유니버설 &gt; 비어 있는 앱(Windows 유니버설)**을 선택합니다. 이 앱에서 앱 서비스를 제공합니다.

## package.appxmanifest에 앱 서비스 확장 추가


AppServiceProvider 프로젝트의 Package.appxmanifest 파일에서 **&lt;Application&gt;** 요소에 다음과 같은 AppService 확장 기능을 추가합니다. 이 예제에서는 `com.Microsoft.Inventory` 서비스를 광고하고 이 앱이 앱 서비스 공급자로 식별됩니다. 실제 서비스가 백그라운드 작업으로 구현됩니다. 앱 서비스 앱에서 다른 앱에 서비스를 공개합니다. 서비스 이름에 역방향 도메인 이름 스타일을 사용하는 것이 좋습니다.

``` syntax
... 
<Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="AppServiceProvider.App">
      <Extensions>
        <uap:Extension Category="windows.appService" EntryPoint="MyAppService.Inventory">
          <uap:AppService Name="com.microsoft.inventory"/>
        </uap:Extension>
      </Extensions>
      ...
    </Application>
</Applications>
```

**Category** 특성을 통해 이 응용 프로그램을 앱 서비스 공급자로 식별합니다.

**EntryPoint** 특성을 통해 서비스를 구현하는 클래스를 식별합니다. 이 예에서 다음으로 이 클래스를 구현할 것입니다.

## 앱 서비스 만들기


1.  앱 서비스는 백그라운드 작업으로 구현됩니다. 따라서 포그라운드 응용 프로그램이 백그라운드 작업 방식으로 작업을 수행하도록 다른 응용 프로그램에서 앱 서비스를 호출할 수 있습니다. MyAppService라는 솔루션에 새 Windows 런타임 구성 요소 프로젝트를 추가합니다(**파일 &gt; 추가 &gt; 새 프로젝트**). ( **새 프로젝트 추가** 대화 상자에서 **설치됨 &gt; 기타 언어 &gt; Visual C# &gt; Windows &gt; Windows Universal &gt; Windows 런타임 구성 요소(Windows Universal 선택)**
2.  AppServiceProvider 프로젝트에서 MyAppService 프로젝트에 참조를 추가합니다.
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
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn&#39;t terminated.
            taskInstance.Canceled += OnTaskCanceled; // Associate a cancellation handler with the background task.

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
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

    백그라운드 작업을 만들면 **Run()**이 호출됩니다. 백그라운드 작업은 **Run**이 완료되고 나면 종료되므로 백그라운드 작업이 요청을 계속 처리하도록 코드가 지연됩니다.

    작업이 취소되면 **OnTaskCanceled()**가 호출됩니다. 클라이언트 앱에서 [**AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704)을 삭제하거나 클라이언트 앱이 일시 중단되거나 OS가 종료 또는 절전 상태이거나 OS에 작업을 실행할 리소스가 없는 경우 이 작업이 취소됩니다.

## 앱 서비스의 코드 작성


**OnRequestedReceived()**로 앱 서비스의 코드가 이동됩니다. MyAppService의 Class1.cs에 있는 스텁 **OnRequestedReceived()**를 이 예의 코드로 바꿉니다. 이 코드에서는 인벤토리 항목의 인덱스를 가져온 다음 지정된 인벤토리 항목의 이름과 가격을 검색하기 위해 명령 문자열과 함께 서비스에 전달합니다. 편의를 위해 오류 처리 코드는 제거되었습니다.

```cs
private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below to respond to the message
    // and we don&#39;t want this call to get cancelled while we are waiting.
    var messageDeferral = args.GetDeferral();

    ValueSet message = args.Request.Message;
    ValueSet returnData = new ValueSet();

    string command = message["Command"] as string;
    int? inventoryIndex = message["ID"] as int?;

    if ( inventoryIndex.HasValue &amp;&amp;
         inventoryIndex.Value >= 0 &amp;&amp;
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
    messageDeferral.Complete(); // Complete the deferral so that the platform knows that we&#39;re done responding to the app service call.
}
```

이 예제에서는 [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722)에 대해 awaitable 메서드를 호출하므로 **OnRequestedReceived()**가 **async**입니다.

서비스가 OnRequestReceived 처리기에서 **async** 메서드를 사용할 수 있도록 지연됩니다. 메시지 처리를 완료할 때까지 OnRequestReceived에 대한 호출이 완료되지 않게 합니다. [
            **SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722)는 완료와 함께 응답을 보내는 데 사용합니다. **SendResponseAsync**에서는 호출이 완료되어도 신호를 보내지 않습니다. 지연이 완료되어야 [**SendMessageAsync**](https://msdn.microsoft.com/library/windows/apps/dn921712)에 OnRequestReceived가 완료되었다는 신호를 보냅니다.

앱 서비스에서는 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)를 사용하여 정보를 교환합니다. 전달할 수 있는 데이터의 크기는 시스템 리소스를 통해서만 제한될 수 있습니다. **ValueSet**에서 사용할 사전 정의된 키가 없습니다. 앱 서비스의 프로토콜을 정의하는 데 사용할 키 값을 결정해야 합니다. 이 프로토콜을 염두에 두고 호출자를 작성해야 합니다. 이 예제에서는 "Command"라는 키를 선택했습니다. 이 키의 값을 통해 앱 서비스에서 인벤토리 항목의 이름을 제공할지 아니면 값을 제공할지를 나타냅니다. 인벤토리 이름의 색인은 "ID" 키에 저장됩니다. 반환 값은 "Result" 키에 저장됩니다.

앱 서비스에 대한 호출의 성공 여부를 표시하기 위해 [**AppServiceClosedStatus**](https://msdn.microsoft.com/library/windows/apps/dn921703) enum이 호출자에게 반환됩니다. OS에서 서비스 끝점을 중단하거나 리소스가 초과되거나 하는 등이 앱 서비스에 대한 호출이 실패하는 예입니다. [
            **ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)를 통해 추가 오류 정보를 반환할 수 있습니다. 이 예제에서는 "Status"라는 키를 사용하여 호출자에게 자세한 오류 정보를 반환합니다.

[
            **SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722)에 대한 호출을 통해 호출자에게 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)를 반환합니다.

## 서비스 앱 배포 및 패키지 패밀리 이름 가져오기


앱 서비스 공급자 앱을 배포해야 클라이언트에서 호출할 수 있습니다. 앱 서비스 앱을 호출하려면 해당 패키지 패밀리 이름도 필요합니다.

-   앱 서비스 응용 프로그램의 패키지 패밀리 이름을 가져오는 방법은 **AppServiceProvider** 프로젝트에서(예: App.xaml.cs의 `public App()`에서) [**Windows.ApplicationModel.Package.Current.Id.FamilyName**](https://msdn.microsoft.com/library/windows/apps/br224670)을 호출하고 결과를 기록하는 것뿐입니다. Microsoft Visual Studio에서 AppServiceProvider를 실행하려면 솔루션 탐색기 창에서 시작 프로젝트로 설정한 다음 프로젝트를 실행합니다.
-   패키지 패밀리 이름을 가져오는 또 다른 방법은 솔루션을 배포(**빌드 &gt; 솔루션 배포**)하고 출력 창에서 전체 패키지 이름을 기록(**보기 &gt; 출력**)하는 것입니다. 패키지 이름을 파생시키려면 출력 창의 문자열에서 플랫폼 정보를 제거해야 합니다. 예를 들어 출력 창에 보고된 전체 패키지 이름이 "9fe3058b-3de0-4e05-bea7-84a06f0ee4f0\_1.0.0.0\_x86\_\_yd7nk54bq29ra"이면 "9fe3058b-3de0-4e05-bea7-84a06f0ee4f0\_yd7nk54bq29ra"를 패키지 패밀리 이름으로 두고 "1.0.0.0\_x86\_\_"을 추출합니다.

## 앱 서비스를 호출하는 클라이언트 작성


1.  ClientApp이라는 솔루션에 새로운 빈 Windows 유니버설 앱 프로젝트를 추가합니다(**파일 &gt; 추가 &gt; 새 프로젝트**). ( **새 프로젝트 추가** 대화 상자에서 **설치됨 &gt; 기타 언어 &gt; Visual C# &gt; Windows &gt; Windows 유니버설 &gt; 비어 있는 앱(Windows 유니버설)**선택).
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

            // Here, we use the app service name defined in the app service provider&#39;s Package.appxmanifest file in the &lt;Extension&gt; section. 
            this.inventoryService.AppServiceName = "com.microsoft.inventory";

            // Use Windows.ApplicationModel.Package.Current.Id.FamilyName within the app service provider to get this value.
            this.inventoryService.PackageFamilyName = "replace with the package family name";

            var status = await this.inventoryService.OpenAsync();
            if (status != AppServiceConnectionStatus.Success)
            {
                button.Content = "Failed to connect";
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

        button.Content = result;
    }
    ```

    Replace the package family name in the line `this.inventoryService.PackageFamilyName = "replace with the package family name";` with the package family name of the **AppServiceProvider** project that you obtained in \[Step 5: Deploy the service app and get the package family name\].

    The code first establishes a connection with the app service. The connection will remain open until you dispose **this.inventoryService**. The app service name must match the **AppService Name** attribute that you added to the AppServiceProvider project's Package.appxmanifest file. In this example, it is `<uap:AppService Name="com.microsoft.inventory"/>`.

    A [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) named **message** is created to specify the command that we want to send to the app service. The example app service expects a command to indicate which of two actions to take. We get the index from the textbox in the ClientApp, and then call the service with the "Item" command to get the description of the item. Then, we make the call with the "Price" command to get the item's price. The button text is set to the result.

    Because [**AppServiceResponseStatus**](https://msdn.microsoft.com/library/windows/apps/dn921724) only indicates whether the operating system was able to connect the call to the app service, we check the "Status" key in the [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) we receive from the app service to ensure that it was able to fulfill the request.

6.  In Visual Studio, set the ClientApp project to be the startup project in the Solution Explorer window and run the solution. Enter the number 1 into the text box and click the button. You should get "Chair : Price = 88.99" back from the service.

    ![sample app displaying chair price=88.99](images/appserviceclientapp.png)

If the app service call fails, check the following in the ClientApp:

1.  Verify that the package family name assigned to the inventory service connection matches the package family name of the AppServiceProvider app. See: **button\_Click()**`this.inventoryService.PackageFamilyName = "...";`).
2.  In **button\_Click()**, verify that the app service name that is assigned to the inventory service connection matches the app service name in the AppServiceProvider's Package.appxmanifest file. See: `this.inventoryService.AppServiceName = "com.microsoft.inventory";`.
3.  Ensure that the AppServiceProvider app has been deployed (In the Solution Explorer, right-click the solution and choose **Deploy**).

## Debug the app service


1.  Ensure that the entire solution is deployed before debugging because the app service provider app must be deployed before the service can be called. (In Visual Studio, **Build &gt; Deploy Solution**).
2.  In the Solution Explorer, right-click the AppServiceProvider project and choose **Properties**. From the **Debug** tab, change the **Start action** to **Do not launch, but debug my code when it starts**.
3.  In the MyAppService project, in the Class1.cs file, set a breakpoint in OnRequestReceived().
4.  Set the AppServiceProvider project to be the startup project and press F5.
5.  Start ClientApp from the Start menu (not from Visual Studio).
6.  Enter the number 1 into the text box and press the button. The debugger will stop in the app service call on the breakpoint in your app service.

## Debug the client


1.  Follow the instructions in the preceding step to debug the app service.
2.  Launch ClientApp from the Start menu.
3.  Attach the debugger to the ClientApp.exe process (not the ApplicationFrameHost.exe process). (In Visual Studio, choose **Debug &gt; Attach to Process...**.)
4.  In the ClientApp project, set a breakpoint in **button\_Click()**.
5.  The breakpoints in both the client and the app service will now be hit when you enter the number 1 into the text box of the ClientApp and click the button.

## Remarks


This example provides a simple introduction to creating an app service and calling it from another app. The key things to note are the creation of a background task to host the app service, the addition of the windows.appservice extension to the app service provider app's Package.appxmanifest file, obtaining the package family name of the app service provider app so that we can connect to it from the client app, and using [**Windows.ApplicationModel.AppService.AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704) to call the service.

## Full code for MyAppService


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
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn&#39;t terminated.
            taskInstance.Canceled += OnTaskCanceled; // Associate a cancellation handler with the background task.

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // Get a deferral because we use an awaitable API below to respond to the message
            // and we don&#39;t want this call to get cancelled while we are waiting.
            var messageDeferral = args.GetDeferral();

            ValueSet message = args.Request.Message;
            ValueSet returnData = new ValueSet();

            string command = message["Command"] as string;
            int? inventoryIndex = message["ID"] as int?;

            if (inventoryIndex.HasValue &amp;&amp;
                 inventoryIndex.Value >= 0 &amp;&amp;
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
            messageDeferral.Complete(); // Complete the deferral so that the platform knows that we&#39;re done responding to the app service call.
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

## 관련 항목


* [백그라운드 작업을 사용하여 앱 지원](support-your-app-with-background-tasks.md)

 

 





<!--HONumber=Mar16_HO1-->


