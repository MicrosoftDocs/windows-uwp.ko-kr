---
title: 앱 서비스 만들기 및 사용
description: 다른 UWP 앱에 서비스를 제공할 수 있는 UWP(유니버설 Windows 플랫폼) 앱을 작성하는 방법과 해당 서비스를 사용하는 방법을 알아봅니다.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: 앱에 통신, IPC, 메시징, 백그라운드 통신을 앱에 app service는 백그라운드 프로세스 간 통신
ms.date: 01/16/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 96ecad8494ad82dc4927b3675ad322f07a8ca7f3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643578"
---
# <a name="create-and-consume-an-app-service"></a>앱 서비스 만들기 및 사용

앱 서비스는 다른 UWP 앱에 서비스를 제공하는 UWP 앱입니다. 장치에서 앱 서비스는 웹 서비스와 비슷합니다. 앱 서비스는 호스트 앱에서 배경 작업으로 실행되고 다른 앱에 서비스를 제공할 수 있습니다. 예를 들어, 앱 서비스는 다른 앱에서 사용할 수 있는 바코드 스캐너 서비스를 제공할 수 있습니다. 또는 앱의 Enterprise Suite에 해당 제품군의 다른 앱에서 사용할 수 있는 일반 맞춤법 검사 앱 서비스가 있습니다.  Windows 10, 버전 1607부터 앱 서비스를 통해 앱이 동일한 장치에서 호출할 수 있는 UI 없는 서비스를 원격 장치에 만들 수 있습니다.

Windows 10 버전 1607부터 호스트 앱과 같은 프로세스에서 실행되는 앱 서비스를 만들 수 있습니다. 이 문서는 별도 백그라운드 프로세스에서 실행되는 앱 서비스 만들기 및 사용에 중점을 둡니다. 공급자와 같은 프로세스에서 앱 서비스를 실행하는 방법에 대한 자세한 내용은 [앱 서비스가 호스트 앱과 동일한 프로세스에서 실행되도록 변환](convert-app-service-in-process.md)을 참조하세요.

앱 서비스 코드 샘플은 [유니버설 Windows 플랫폼(UWP) 앱 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)을 참조하세요.

## <a name="create-a-new-app-service-provider-project"></a>새 앱 서비스 공급자 프로젝트 만들기

이 방법에서는 편의상 모두를 한 솔루션으로 만듭니다.

1. Visual Studio 2015 이상에서는 새 UWP 앱 프로젝트를 만들고 이름을 **AppServiceProvider**합니다.
    1. 선택 **파일 > 새로 만들기 > 프로젝트...** 
    2. 에 **새 프로젝트** 대화 상자에서 **설치 됨 > Visual C# > 비어 있는 앱 (유니버설 Windows)** 합니다. 이렇게 하면 다른 UWP 앱에서 앱 서비스 사용할 수 있게 됩니다.
    3. 프로젝트 이름을 **AppServiceProvider**, 한 위치를 선택 하 고 클릭 **확인**합니다.

2. 선택 하 라는 메시지가 표시 되는 경우는 **대상** 하 고 **최소 버전** 이상 프로젝트를 선택 **10.0.14393**합니다. 새 사용 하려는 경우 **SupportsMultipleInstances** 특성을 사용 해야 Visual Studio 2017 및 대상 **10.0.15063** (**Windows 10 크리에이터 스 업데이트**) 이상.

<span id="appxmanifest"/>

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>Package.appxmanifest에는 app service 확장 추가

에 **AppServiceProvider** 프로젝트를 열고 합니다 **Package.appxmanifest** 텍스트 편집기에서 파일: 

1. 마우스 오른쪽 단추로 클릭 합니다 **솔루션 탐색기**합니다. 
2. 선택 **열리고**합니다. 
3. 선택 **XML (텍스트) 편집기**합니다. 

다음을 추가 합니다 `AppService` 확장 된 `<Application>` 요소입니다. 이 예제에서는 `com.microsoft.inventory` 서비스를 광고하고 이 앱이 앱 서비스 공급자로 식별됩니다. 실제 서비스가 백그라운드 작업으로 구현됩니다. 앱 서비스 프로젝트에서 다른 앱에 서비스를 공개합니다. 서비스 이름에 역방향 도메인 이름 스타일을 사용하는 것이 좋습니다.

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

`Category` 특성은이 응용 프로그램을 app service 공급자를 식별 합니다.

`EntryPoint` 특성은 다음 구현 하는 서비스를 구현 하는 정규화 된 네임 스페이스 클래스를 식별 합니다.

`SupportsMultipleInstances` app service는 호출 될 때마다 실행 되도록 새 프로세스에서 특성을 나타냅니다. 이 필요 하지 않지만 해당 기능이 필요 하 여 10.0.15063 대상으로 하는 경우 사용할 수 있는 SDK (**Windows 10 크리에이터 스 업데이트**) 이상. 마찬가지로 앞에 `uap4` 네임스페이스가 붙습니다.

## <a name="create-the-app-service"></a>앱 서비스 만들기

1.  앱 서비스를 백그라운드 작업으로 구현할 수 있습니다. 이렇게 하면 포그라운드 응용 프로그램이 다른 응용 프로그램에서 앱 서비스를 호출할 수 있습니다. 백그라운드 작업으로 app service를 만들려면 새 Windows 런타임 구성 요소 프로젝트를 솔루션에 추가 합니다 (**파일 &gt; 추가 &gt; 새 프로젝트**) 이라는 **MyAppService**합니다. 에 **새 프로젝트 추가** 대화 상자에서 **설치 됨 > Visual C# > Windows 런타임 구성 요소 (유니버설 Windows)** 합니다.
2.  에 **AppServiceProvider** 프로젝트에 새 프로젝트 간 참조를 추가 합니다 **MyAppService** 프로젝트 (에 **솔루션 탐색기**, 를마우스오른쪽단추로클릭 **AppServiceProvider** 프로젝트 > **추가** > **참조** > **프로젝트**  >   **솔루션**을 선택 **MyAppService** > **확인**). 참조를 추가하지 않으면 앱 서비스가 런타임 시 연결되지 않으므로 이 단계가 매우 중요합니다.
3.  에 **MyAppService** 프로젝트에서 다음을 추가 합니다 **사용 하 여** 의 맨 위에 문을 **Class1.cs**:
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  이름 바꾸기 **Class1.cs** 하 **Inventory.cs**에 대 한 스텁 코드를 바꾸고 **Class1** 라는 새 백그라운드 작업 클래스를 사용 하 여 **인벤토리**:

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

    **실행** 백그라운드 작업이 만들어질 때 호출 됩니다. 백그라운드 작업은 **Run**이 완료되고 나면 종료되므로 백그라운드 작업이 요청을 계속 처리하도록 코드가 지연됩니다. 백그라운드 작업으로 구현 되는 app service는 해당 기간 내에서 다시 호출 아니면 수행 되는 지연 호출을 받은 후 약 30 초 동안 활성 상태로 유지 됩니다. App service는 호출자와 같은 프로세스로 구현 하는 경우 app service의 수명 동안 호출자의 수명에 연결 됩니다.

    앱 서비스의 수명은 호출자에 따라 달라집니다.

    * 호출자가 전경, 앱 서비스 수명 호출자에 게 동일 합니다.
    * 호출자에 게 백그라운드에서 경우 app service를 실행 하는 데 30 초를 가져옵니다. 지연을 사용하면 일회에 한해 5초가 추가로 제공됩니다.

    **OnTaskCanceled** 작업이 취소 될 때 호출 됩니다. 클라이언트 앱을 삭제 하는 경우 작업이 취소 되는 [AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/dn921704), 클라이언트 앱이 일시 중단, OS 종료 되거나 절전 또는 운영 체제 태스크를 실행 하는 리소스 부족 합니다.

## <a name="write-the-code-for-the-app-service"></a>앱 서비스의 코드 작성

**OnRequestReceived** app service에 대 한 코드를 이동 하는 위치는입니다. 스텁 바꿉니다 **OnRequestReceived** 에 **MyAppService**의 **Inventory.cs** 이 예제의 코드를 사용 하 여 합니다. 이 코드에서는 인벤토리 항목의 인덱스를 가져온 다음 지정된 인벤토리 항목의 이름과 가격을 검색하기 위해 명령 문자열과 함께 서비스에 전달합니다. 여러분의 프로젝트에 대해 오류 처리 코드를 추가합니다.

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

사실은 **OnRequestReceived** 됩니다 **비동기** 있도록 하는 awaitable 메서드를 호출 하기 때문에 [SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722) 이 예제.

지연은 서비스를 사용할 수 있도록 만들어진 **비동기** 메서드는 **OnRequestReceived** 처리기입니다. 메시지 처리를 완료할 때까지 **OnRequestReceived**에 대한 호출이 완료되지 않게 합니다.  [SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722) 호출자에 게 결과 보냅니다. **SendResponseAsync**에서는 호출이 완료되어도 신호를 보내지 않습니다. 신호를 보내는 지연 완료 것 [SendMessageAsync](https://msdn.microsoft.com/library/windows/apps/dn921712) 하는 **OnRequestReceived** 완료 합니다. 에 대 한 호출 **SendResponseAsync** 경우에도 지연을 완료 해야 하기 때문에 try/finally 블록에 래핑됩니다 **SendResponseAsync** 예외를 throw 합니다.

앱 서비스 사용 [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) 정보를 교환 하는 개체입니다. 전달할 수 있는 데이터의 크기는 시스템 리소스를 통해서만 제한될 수 있습니다. **ValueSet**에서 사용할 사전 정의된 키가 없습니다. 앱 서비스의 프로토콜을 정의하는 데 사용할 키 값을 결정해야 합니다. 이 프로토콜을 염두에 두고 호출자를 작성해야 합니다. 이 예제에서는 `Command`라는 키를 선택했습니다. 이 키의 값을 통해 앱 서비스에서 인벤토리 항목의 이름을 제공할지 아니면 값을 제공할지를 나타냅니다. 인벤토리 이름의 색인은 `ID` 키에 저장됩니다. 반환 값은 `Result` 키에 저장됩니다.

앱 서비스에 대한 호출의 성공 여부를 표시하기 위해 [AppServiceClosedStatus](https://msdn.microsoft.com/library/windows/apps/dn921703) enum이 호출자에게 반환됩니다. 앱 서비스 호출이 실패할 수 있는 예로는 리소스가 초과되어 OS에서 서비스 끝점을 중단하는 경우를 들 수 있습니다. [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131)를 통해 추가 오류 정보를 반환할 수 있습니다. 이 예제에서는 `Status`라는 키를 사용하여 호출자에게 자세한 오류 정보를 반환합니다.

[SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722)에 대한 호출을 통해 호출자에게 [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131)를 반환합니다.

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>서비스 앱 배포 및 패키지 패밀리 이름 가져오기

클라이언트에서 호출 하기 전에 app service 공급자를 배포 해야 합니다. 선택 하 여 배포할 수 있습니다 **빌드 > 솔루션 배포** Visual Studio에서.

메서드를 호출 하기 위해 app service 공급자의 패키지 패밀리 이름을 해야 합니다. 열어 가져올 수 있습니다 합니다 **AppServiceProvider** 프로젝트의 **Package.appxmanifest** 디자이너 뷰에서 파일 (에서 두 번 클릭 합니다 **솔루션 탐색기**). 선택 합니다 **패키징** 탭에서 다음 값을 복사 **패키지 제품군 이름**, 다른 곳에 붙여 메모장과 같은 지금은 및 합니다.

## <a name="write-a-client-to-call-the-app-service"></a>앱 서비스를 호출하는 클라이언트 작성

1.  **파일 &gt; 추가 &gt; 새 프로젝트**를 선택하여 솔루션에 Windows 유니버설 앱 프로젝트를 추가합니다. 에 **새 프로젝트 추가** 대화 상자에서 **설치 됨 > Visual C# > 비어 있는 앱 (유니버설 Windows)** 하 고 이름을 **ClientApp**.

2.  에 **ClientApp** 프로젝트에서 다음을 추가 합니다 **사용 하 여** 의 맨 위에 문을 **MainPage.xaml.cs**:
    ```cs
    using Windows.ApplicationModel.AppService;
    ```

3.  라는 입력란을 추가 **텍스트 상자** 고 하는 단추 **MainPage.xaml**.

4.  단추 추가 호출 단추에 대 한 처리기를 클릭 **button_Click**, 키워드를 추가 하 고 **비동기** 단추 처리기의 시그니처를 합니다.

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
    > 변수에 배치 하는 것이 아니라 문자열 리터럴, 붙여 넣어야 합니다. 변수를 사용 하는 경우에 작동 하지 않습니다.

    코드는 먼저 앱 서비스와 연결합니다. 연결은 `this.inventoryService`를 삭제할 때까지 열린 상태로 유지됩니다. App service 이름 일치 해야 합니다는 `AppService` 요소의 `Name` 특성에 추가 합니다 **AppServiceProvider** 프로젝트의 **Package.appxmanifest** 파일입니다. 이 예제에서는 `<uap3:AppService Name="com.microsoft.inventory"/>`입니다.

    A [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) 라는 `message` app service에 전송 하려는 명령을 지정 하려면 만들어집니다. 예제 앱 서비스에서는 명령을 사용하여 수행할 두 가지 작업이 나타나게 됩니다. 클라이언트 앱에서 텍스트 상자에서 인덱스를 가져올 하 고 사용 하 여 서비스 호출을 `Item` 명령 항목의 설명을 가져옵니다. 그런 다음 항목 가격을 가져오도록 `Price` 명령을 사용하여 호출합니다. 단추 텍스트는 결과로 설정됩니다.

    때문에 [AppServiceResponseStatus](https://msdn.microsoft.com/library/windows/apps/dn921724) 만 운영 체제 연결할 app service에 대 한 호출을 확인 했는지를 나타내는 합니다 `Status` 키를 [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) 앱에서 수신 서비스 요청을 수행할 수 있음을 확인입니다.

6. 설정 합니다 **ClientApp** 프로젝트를 시작 프로젝트로 (에서 마우스 오른쪽 단추로 클릭 합니다 **솔루션 탐색기** > **시작 프로젝트로 설정**) 솔루션 및 실행 합니다. 입력란에 숫자 1을 입력하고 단추를 클릭합니다. 가져와야 "Chair: 가격 = 88.99"서비스에서 다시 합니다.

    ![의자 가격=88.99를 표시하는 샘플 앱](images/appserviceclientapp.png)

앱 서비스 호출에 실패 하는 경우 다음을 확인 합니다 **ClientApp** 프로젝트:

1.  Inventory 서비스 연결에 할당 된 패키지 제품군 이름을의 패키지 패밀리 이름을 일치 하는지 확인 합니다 **AppServiceProvider** 앱. 줄을 확인 하세요 **단추\_클릭** 사용 하 여 `this.inventoryService.PackageFamilyName = "...";`입니다.
2.  **단추\_클릭**, 인벤토리 서비스 연결에 할당 되는 app service 이름에서 앱 서비스 이름과 일치 하는지 확인 합니다 **AppServiceProvider**의  **Package.appxmanifest** 파일입니다. `this.inventoryService.AppServiceName = "com.microsoft.inventory";`를 참조하세요.
3.  있는지 확인 합니다 **AppServiceProvider** 앱이 배포 된 합니다. (에 **솔루션 탐색기**솔루션을 마우스 오른쪽 단추로 클릭 하 고 선택 **솔루션 배포**).

## <a name="debug-the-app-service"></a>앱 서비스 디버그

1.  앱 서비스 공급자 앱을 배포해야 서비스를 호출할 수 있으므로 디버깅 전에 솔루션이 배포되도록 합니다. (Visual Studio에서 **빌드 &gt; 솔루션 배포**).
2.  에 **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 합니다 **AppServiceProvider** 선택한 프로젝트 **속성**. **디버그** 탭에서 **시작 작업**을 **실행하지 않지만 시작되면 내 코드 디버그**로 변경합니다. (C++를 사용하여 앱 서비스 공급자를 구현하는 경우 **디버깅** 탭에서 **응용 프로그램 시작**을 **아니요**로 변경합니다).
3.  에 **MyAppService** 프로젝트에 **Inventory.cs** 파일 중단점을 설정 합니다 **OnRequestReceived**.
4.  설정 합니다 **AppServiceProvider** 프로젝트를 시작 프로젝트 및 키를 눌러 **F5**합니다.
5.  시작 **ClientApp** (없습니다: Visual Studio)에서 시작 합니다.
6.  입력란에 숫자 1을 입력하고 단추를 누릅니다. 디버거가 앱 서비스의 중단점에서 앱 서비스 호출을 중단합니다.

## <a name="debug-the-client"></a>클라이언트 디버그

1.  이전 단계의 지침에 따라 앱 서비스를 호출하는 클라이언트를 디버그합니다.
2.  시작할 **ClientApp** 시작 합니다.
3.  디버거를 연결 합니다 **ClientApp.exe** 프로세스 (되지 합니다 **ApplicationFrameHost.exe** 프로세스). (Visual Studio에서 **디버그 &gt; 프로세스에 추가...** 선택)
4.  에 **ClientApp** 프로젝트 중단점을 설정 합니다 **단추\_클릭**합니다.
5.  텍스트 상자에 숫자 1을 입력 하는 경우 클라이언트와 app service의 중단점을 적중 이제 것입니다 **ClientApp** 단추를 클릭 합니다.

## <a name="general-app-service-troubleshooting"></a>일반적인 앱 서비스 문제 해결

발생 하는 경우는 **AppUnavailable** 상태는 app service에 연결을 시도 후 다음을 확인 합니다.

- 앱 서비스 공급자 프로젝트 앱 및 서비스 프로젝트가 배포되었는지 확인합니다. 클라이언트를 실행하기 전에 둘 다 배포되어 있어야 하며, 그렇지 않으면 클라이언트가 연결할 대상이 없습니다. Visual Studio에서 **빌드** > **솔루션 배포**를 사용하여 배포할 수 있습니다.
- 에 **솔루션 탐색기**, 응용 프로그램 서비스 공급자 프로젝트 app service를 구현 하는 프로젝트에 대 한 프로젝트 간 참조에 있는지 확인 합니다.
- 있는지 확인 합니다 `<Extensions>` 항목과 해당 자식 요소에 추가 된 합니다 **Package.appxmanifest** 에서 위에 지정 된 대로 응용 프로그램 서비스 공급자 프로젝트에 속한 파일 [앱 서비스 확장을 추가 Package.appxmanifest](#appxmanifest)합니다.
- 있는지 확인 합니다 [AppServiceConnection.AppServiceName](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.appservicename) app service 공급자를 호출 하는 클라이언트의 문자열과 일치 합니다 `<uap3:AppService Name="..." />` 앱 서비스 공급자 프로젝트에 지정 된 **Package.appxmanifest**  파일입니다.
- 있는지 확인 합니다 [AppServiceConnection.PackageFamilyName](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.packagefamilyname) 앱 서비스 공급자 구성 요소에서 위에 지정 된 대로 패키지 패밀리 이름 일치 [Package.appxmanifest에는 app service 확장 추가](#appxmanifest)
- 이 예제의 것과 같은-의-out-proc app services에 대 한 유효성을 검사 하는 `EntryPoint` 에 지정 된를 `<uap:Extension ...>` 앱 서비스 공급자 프로젝트 요소의 **Package.appxmanifest** 파일 네임 스페이스와 일치 하 및 구현 하는 공용 클래스의 클래스 이름을 [IBackgroundTask](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask) 앱 서비스 프로젝트에서.

### <a name="troubleshoot-debugging"></a>디버깅 문제 해결

디버거가 앱 서비스 공급자 앱 또는 서비스 프로젝트의 중단점에서 중지하지 않으면 다음 사항을 확인합니다.

- 앱 서비스 공급자 프로젝트 앱 및 서비스 프로젝트가 배포되었는지 확인합니다. 클라이언트를 실행하기 전에 둘 다 배포되어 있어야 합니다. Visual Studio에서 **빌드** > **솔루션 배포**를 사용하여 배포할 수 있습니다.
- 디버깅 하려는 프로젝트가 시작 프로젝트로 설정 되어 있는지 및 프로젝트를 실행 되지 않거나 해당 프로젝트에 대 한 디버깅 속성 설정 되어 있는지 확인 하면 **F5** 눌러져 있습니다. 프로젝트를 마우스 오른쪽 단추로 클릭한 다음 **속성**을 클릭하고 **디버그**(또는 C++에서는 **디버깅**)를 클릭합니다. C#에서 **시작 작업**을 **실행하지 않지만 시작되면 내 코드 디버그**로 변경합니다. C++에서 **응용 프로그램 시작**을 **아니요**로 설정합니다.

## <a name="remarks"></a>설명

이 예제에서는 백그라운드 작업으로 실행되는 앱 서비스를 만들고 다른 앱에서 이 앱을 호출하는 방법을 간략하게 소개합니다. 점이 주요 사항은 다음과 같습니다.

* 앱 서비스를 호스팅하는 백그라운드 작업을 만듭니다.
* 추가 된 `windows.appService` 앱 서비스 공급자의 확장 **Package.appxmanifest** 파일입니다.
* 클라이언트 앱에서에 연결할 수 있도록 app service 공급자의 패키지 패밀리 이름을 가져옵니다.
* 응용 프로그램 서비스 공급자 프로젝트에서 응용 프로그램 서비스 프로젝트에 대 한 프로젝트 간 참조를 추가 합니다.
* 사용 하 여 [Windows.ApplicationModel.AppService.AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/dn921704) 서비스를 호출 합니다.

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
* [백그라운드 작업을 사용 하 여 앱을 지원 합니다.](support-your-app-with-background-tasks.md)
* [앱 서비스 코드 샘플 (C#, c + + 및 VB)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)
