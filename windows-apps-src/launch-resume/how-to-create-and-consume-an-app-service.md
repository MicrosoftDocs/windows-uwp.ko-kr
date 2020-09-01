---
title: 앱 서비스 만들기 및 사용
description: 다른 UWP 앱에 서비스를 제공할 수 있는 UWP (유니버설 Windows 플랫폼) 앱을 작성 하는 방법 및 이러한 서비스를 사용 하는 방법에 대해 알아봅니다.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: 앱 간 통신, 프로세스 간 통신, IPC, 백그라운드 메시징, 백그라운드 통신, 앱-앱, app service
ms.date: 01/16/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bd67c8a94ae7fc46956f5a6f7c3358e0e28965d2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158827"
---
# <a name="create-and-consume-an-app-service"></a>앱 서비스 만들기 및 사용

App services는 다른 UWP 앱에 서비스를 제공 하는 UWP 앱입니다. 장치에서 웹 서비스와 유사 합니다. App service는 호스트 앱에서 백그라운드 작업으로 실행 되며 다른 앱에 서비스를 제공할 수 있습니다. 예를 들어 app service는 다른 앱이 사용할 수 있는 바코드 스캐너 서비스를 제공할 수 있습니다. 또는 응용 프로그램의 엔터프라이즈 제품군에는 제품군의 다른 앱에서 사용할 수 있는 일반적인 맞춤법 검사 app service가 있습니다.  앱 서비스를 사용 하면 앱이 동일한 장치에서 호출할 수 있는 UI 없는 서비스를 만들 수 있으며, 원격 장치에서 Windows 10, 버전 1607부터 시작할 수 있습니다.

Windows 10 버전 1607부터 호스트 앱과 동일한 프로세스에서 실행 되는 앱 서비스를 만들 수 있습니다. 이 문서에서는 별도의 백그라운드 프로세스에서 실행 되는 app service를 만들고 사용 하는 방법을 중점적으로 다룹니다. 공급자와 동일한 프로세스에서 app service를 실행 하는 방법에 대 한 자세한 내용은 [호스트 앱과 동일한 프로세스에서 실행 되도록 app Service 변환을](convert-app-service-in-process.md) 참조 하세요.

App service 코드 샘플은 [UWP (유니버설 Windows 플랫폼) 앱 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)을 참조 하세요.

## <a name="create-a-new-app-service-provider-project"></a>새 app service 공급자 프로젝트 만들기

이 방법에서는 간단 하 게 하기 위해 하나의 솔루션에 모든 항목을 만듭니다.

1. Visual Studio 2015 이상에서 새 UWP 앱 프로젝트를 만들고 이름을 **AppServiceProvider**로 표시 합니다.
    1. **파일 > 새 > 프로젝트** ...를 선택 합니다. 
    2. **새 프로젝트 만들기** 대화 상자에서 **비어 있는 앱 (유니버설 Windows) c #** 을 선택 합니다. 다른 UWP 앱에서 app service를 사용할 수 있도록 하는 앱입니다.
    3. **다음**을 클릭 하 고 프로젝트 이름을 **AppServiceProvider**로 지정한 다음 해당 위치를 선택 하 고 **만들기**를 클릭 합니다.

2. 프로젝트의 **대상** 및 **최소 버전** 을 선택 하 라는 메시지가 표시 되 면 최소 **10.0.14393**를 선택 합니다. 새 **SupportsMultipleInstances** 특성을 사용 하려면 visual studio 2017 또는 visual studio 2019 및 대상 **10.0.15063** (**Windows 10 크리에이터 업데이트**) 이상을 사용 해야 합니다.

<span id="appxmanifest"/>

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>Appxmanifest.xml에 app service 확장을 추가 합니다.

**AppServiceProvider** 프로젝트에서 **appxmanifest.xml** 파일을 텍스트 편집기에서 엽니다. 

1. **솔루션 탐색기**에서이를 마우스 오른쪽 단추로 클릭 합니다. 
2. **연결 프로그램을**선택 합니다. 
3. **XML (텍스트) 편집기**를 선택 합니다. 

요소 내에 다음 확장을 추가 합니다 `AppService` `<Application>` . 이 예제에서는 서비스를 알리고 `com.microsoft.inventory` 앱 서비스 공급자로이 앱을 식별 합니다. 실제 서비스는 백그라운드 작업으로 구현 됩니다. App service 프로젝트는 다른 앱에 서비스를 노출 합니다. 서비스 이름에는 역방향 도메인 이름 스타일을 사용 하는 것이 좋습니다.

`xmlns:uap4`네임 스페이스 접두사 및 `uap4:SupportsMultipleInstances` 특성은 Windows SDK 버전 10.0.15063 이상을 대상으로 하는 경우에만 유효 합니다. 이전 SDK 버전을 대상으로 하는 경우 안전 하 게 제거할 수 있습니다.

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

`Category`특성은이 응용 프로그램을 app service 공급자로 식별 합니다.

`EntryPoint`특성은 다음에 구현할 서비스를 구현 하는 정규화 된 네임 스페이스 클래스를 식별 합니다.

`SupportsMultipleInstances`특성은 새 프로세스에서 실행 되어야 하는 app service가 호출 될 때마다를 나타냅니다. 필수는 아니지만이 기능이 필요 하 고 10.0.15063 SDK (**Windows 10 크리에이터 업데이트**) 이상을 대상으로 하는 경우에 사용할 수 있습니다. 또한 네임 스페이스 앞에와 야 합니다 `uap4` .

## <a name="create-the-app-service"></a>App service 만들기

1.  앱 서비스를 백그라운드 작업으로 구현할 수 있습니다. 이를 통해 포그라운드 응용 프로그램은 다른 응용 프로그램에서 app service를 호출할 수 있습니다. 앱 서비스를 백그라운드 작업으로 만들려면 **MyAppService**이라는 새 Windows 런타임 구성 요소 프로젝트를 솔루션 (**파일 &gt; 추가 &gt; 새 프로젝트**)에 추가 합니다. **새 프로젝트 추가** 대화 상자에서 **설치 > Visual c # > Windows 런타임 구성 요소 (유니버설 Windows)** 를 선택 합니다.
2.  **AppServiceProvider** 프로젝트에서 새 **MyAppService** 프로젝트에 프로젝트 간 참조를 추가 합니다. ( **솔루션 탐색기**에서 **AppServiceProvider** 프로젝트를 마우스 오른쪽 단추로 클릭 하 > 참조 프로젝트 **추가**솔루션을 마우스 오른쪽 단추로 클릭  >  **Reference**  >  **Projects**  >  **Solution**하 고 **MyAppService**  >  **OK**를 선택 합니다. 참조를 추가 하지 않으면 앱 서비스가 런타임에 연결 되지 않으므로이 단계는 중요 합니다.
3.  **MyAppService** 프로젝트에서 **Class1.cs**의 맨 위에 다음 **using** 문을 추가 합니다.
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  **Class1.cs** 의 이름을 **Inventory.cs**로 바꾸고 **Class1** 의 스텁 코드를 **Inventory**이라는 새 백그라운드 작업 클래스로 바꿉니다.

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

    이 클래스는 app service에서 작업을 수행 하는 위치입니다.

    **실행** 은 백그라운드 작업을 만들 때 호출 됩니다. 백그라운드 작업은 **실행** 이 완료 된 후 종료 되므로 백그라운드 작업이 요청을 처리할 수 있도록 코드에서 지연이 발생 합니다. 백그라운드 작업으로 구현 되는 app service는 해당 기간 내에 다시 호출 되거나 지연이 발생 하는 경우를 제외 하 고는 호출을 받은 후 약 30 초 동안 활성 상태로 유지 됩니다. App service가 호출자와 동일한 프로세스에서 구현 되는 경우 app service의 수명은 호출자의 수명에 연결 됩니다.

    App service의 수명은 호출자에 따라 다릅니다.

    * 호출자가 전경에 있으면 app service 수명은 호출자와 동일 합니다.
    * 호출자가 백그라운드에 있으면 app service를 실행 하는 데 30 초 정도 걸립니다. 지연 시간을 초과 하는 것은 5 초를 추가로 제공 합니다.

    **Ontaskcanceled** 는 작업이 취소 될 때 호출 됩니다. 클라이언트 앱이 [AppServiceConnection](/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection)를 삭제 하거나, 클라이언트 앱이 일시 중단 되거나, os가 종료 되거나 절전 모드로 전환 되거나, os에서 작업을 실행 하는 데 리소스가 부족 한 경우 작업이 취소 됩니다.

## <a name="write-the-code-for-the-app-service"></a>App service에 대 한 코드 작성

**Onrequestreceived** 는 app service에 대 한 코드가 이동 하는 위치입니다. **MyAppService**의 **Inventory.cs** 에 있는 스텁 **onrequestreceived** 를이 예제의 코드로 바꿉니다. 이 코드에서는 인벤토리 항목의 인덱스를 가져온 다음 지정된 인벤토리 항목의 이름과 가격을 검색하기 위해 명령 문자열과 함께 서비스에 전달합니다. 사용자 고유의 프로젝트에 대해 오류 처리 코드를 추가 합니다.

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

이 예제에서 [Sendresponseasync](/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) 에 대 한 대기 가능 메서드 호출을 수행 하기 때문에 **onrequestreceived** 는 **비동기** 입니다.

서비스에서 **Onrequestreceived** 처리기의 **비동기** 메서드를 사용할 수 있도록 지연이 발생 합니다. 메시지 처리가 완료 될 때까지 **Onrequestreceived** 호출이 완료 되지 않도록 합니다.  [Sendresponseasync](/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) 는 결과를 호출자에 게 보냅니다. **Sendresponseasync** 는 호출 완료를 신호 하지 않습니다. **Onrequestreceived** 가 완료 되었음을 [SendMessageAsync](/uwp/api/windows.applicationmodel.appservice.appserviceconnection.sendmessageasync) 에 알리는 지연의 완료입니다. Sendresponseasync **에 대 한** 호출은 **sendresponseasync** 가 예외를 throw 하는 경우에도 지연을 완료 해야 하기 때문에 try/finally 블록에 래핑됩니다.

App services는 [Valueset](/uwp/api/Windows.Foundation.Collections.ValueSet) 개체를 사용 하 여 정보를 교환 합니다. 전달할 수 있는 데이터의 크기는 시스템 리소스에 의해서만 제한 됩니다. **Valueset**에 사용할 미리 정의 된 키가 없습니다. 앱 서비스에 대 한 프로토콜을 정의 하는 데 사용할 키 값을 결정 해야 합니다. 호출자는 해당 프로토콜을 염두에 두어야 합니다. 이 예제에서는 `Command` app service에서 재고 항목 또는 가격의 이름을 제공할지 여부를 나타내는 값을 가진 라는 키를 선택 했습니다. 인벤토리 이름의 인덱스는 키 아래에 저장 됩니다 `ID` . 반환 값은 키 아래에 저장 됩니다 `Result` .

[AppServiceClosedStatus](/uwp/api/Windows.ApplicationModel.AppService.AppServiceClosedStatus) 열거형은 호출자에 게 반환 되어 app service에 대 한 호출이 성공 했는지 아니면 실패 했는지를 나타냅니다. 앱 서비스에 대 한 호출이 실패 하는 방법의 예는 OS가 해당 리소스를 초과 하 여 서비스 끝점을 중단 하는 경우입니다. [Valueset](/uwp/api/Windows.Foundation.Collections.ValueSet)을 통해 추가 오류 정보를 반환할 수 있습니다. 이 예제에서는 라는 키를 사용 하 여 `Status` 더 자세한 오류 정보를 호출자에 게 반환 합니다.

[Sendresponseasync](/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) 를 호출 하면 호출자에 게 [valueset](/uwp/api/Windows.Foundation.Collections.ValueSet) 이 반환 됩니다.

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>서비스 앱을 배포 하 고 패키지 제품군 이름 가져오기

클라이언트에서 호출 하려면 먼저 app service 공급자를 배포 해야 합니다. 빌드 > Visual Studio에서 **솔루션 배포** 를 선택 하 여 배포할 수 있습니다.

또한이를 호출 하려면 app service 공급자의 패키지 패밀리 이름도 필요 합니다. 디자이너 뷰에서 **AppServiceProvider** 프로젝트의 **appxmanifest.xml** 파일을 열어 가져올 수 있습니다 ( **솔루션 탐색기**에서 두 번 클릭). **패키징** 탭을 선택 하 고 **패키지 패밀리 이름**옆에 있는 값을 복사 하 여 지금 메모장과 같은 위치에 붙여넣습니다.

## <a name="write-a-client-to-call-the-app-service"></a>앱 서비스를 호출 하는 클라이언트 작성

1.  새 빈 Windows 유니버설 앱 프로젝트를 **파일 &gt; 추가 &gt; 새 프로젝트**를 사용 하 여 솔루션에 추가 합니다. **새 프로젝트 추가** 대화 상자에서 **설치 > Visual c # > 비어 있는 앱 (유니버설 Windows)** 을 선택 하 고 이름을 **ClientApp**로 입력 합니다.

2.  **ClientApp** 프로젝트에서 **MainPage.xaml.cs**의 맨 위에 다음 **using** 문을 추가 합니다.
    ```cs
    using Windows.ApplicationModel.AppService;
    ```

3.  **텍스트 상자와 단추** 를 **mainpage .xaml**에 추가 합니다.

4.  **Button_Click**단추에 대 한 단추 클릭 처리기를 추가 하 고 단추 처리기의 서명에 **async** 키워드를 추가 합니다.

5. 단추 클릭 처리기의 스텁을 다음 코드로 바꿉니다. `inventoryService`필드 선언을 포함 해야 합니다.
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
    
    줄의 패키지 패밀리 이름을 `this.inventoryService.PackageFamilyName = "Replace with the package family name";` 서비스 앱 배포의 위에서 구한 **AppServiceProvider** 프로젝트의 패키지 패밀리 이름으로 바꾸고 [패키지 제품군 이름을 가져옵니다](#deploy-the-service-app-and-get-the-package-family-name).

    > [!NOTE]
    > 문자열 리터럴을 변수에 넣지 말고 붙여 넣어야 합니다. 변수를 사용 하는 경우에는 작동 하지 않습니다.

    Code first는 app service에 대 한 연결을 설정 합니다. 삭제할 때까지 연결이 열린 상태로 유지 됩니다 `this.inventoryService` . App service 이름은 `AppService` `Name` **AppServiceProvider** 프로젝트의 **appxmanifest.xml** 파일에 추가한 요소의 특성과 일치 해야 합니다. 이 예제에서는 `<uap3:AppService Name="com.microsoft.inventory"/>`입니다.

    [ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet) `message` App service로 보내려는 명령을 지정 하기 위해 라는 valueset가 만들어집니다. 예제 app service에서는 명령을 사용 하 여 수행할 두 작업을 나타낼 것으로 예상 합니다. 클라이언트 앱의 텍스트 상자에서 인덱스를 가져온 다음 명령을 사용 하 여 서비스를 호출 하 여 항목에 대 `Item` 한 설명을 가져옵니다. 그런 다음 명령을 사용 하 여 항목의 가격을 가져오는 호출을 수행 합니다 `Price` . 단추 텍스트는 결과로 설정 됩니다.

    [AppServiceResponseStatus](/uwp/api/Windows.ApplicationModel.AppService.AppServiceResponseStatus) 는 운영 체제가 앱 서비스에 대 한 호출을 연결할 수 있는지 여부를 나타내므로 `Status` 앱 서비스에서 받은 [valueset](/uwp/api/Windows.Foundation.Collections.ValueSet) 의 키를 확인 하 여 요청을 수행할 수 있는지 확인 합니다.

6. **ClientApp** 프로젝트를 시작 프로젝트로 설정 (시작 프로젝트로 설정 솔루션 탐색기에서 마우스 오른쪽 단추로 클릭 **Solution Explorer**  >  **Set as StartUp Project**) 하 고 솔루션을 실행 합니다. 텍스트 상자에 숫자 1을 입력 하 고 단추를 클릭 합니다. 서비스에서 "이유: Price = 88.99"를 다시 받아야 합니다.

    ![이유 가격 = 88.99을 표시 하는 샘플 앱](images/appserviceclientapp.png)

App service 호출이 실패 하면 **ClientApp** 프로젝트에서 다음을 확인 합니다.

1.  인벤토리 서비스 연결에 할당 된 패키지 제품군 이름이 **AppServiceProvider** 앱의 패키지 패밀리 이름과 일치 하는지 확인 합니다. 에서 줄을 클릭 하 여 **단추를 \_ 클릭** `this.inventoryService.PackageFamilyName = "...";` 합니다.
2.  **단추 \_ 클릭**에서 인벤토리 서비스 연결에 할당 된 App service 이름이 **AppServiceProvider**의 **appxmanifest.xml** 파일에 있는 app service 이름과 일치 하는지 확인 합니다. 을 참조 `this.inventoryService.AppServiceName = "com.microsoft.inventory";` 하세요.
3.  **AppServiceProvider** 앱이 배포 되었는지 확인 합니다. **솔루션 탐색기**에서 솔루션을 마우스 오른쪽 단추로 클릭 하 고 **솔루션 배포**를 선택 합니다.

## <a name="debug-the-app-service"></a>App service 디버그

1.  서비스를 호출 하기 전에 app service provider 앱을 배포 해야 하기 때문에 디버깅 하기 전에 솔루션이 배포 되었는지 확인 합니다. Visual Studio에서 ** &gt; 솔루션 배포를 빌드합니다**.
2.  **솔루션 탐색기**에서 **AppServiceProvider** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다. **디버그** 탭에서 **시작 작업** 을 시작 **하지 않음 (시작 시 코드 디버그**)으로 변경 합니다. C + +를 사용 하 여 app service 공급자를 구현 하는 경우 **디버깅** 탭에서 **응용 프로그램 시작** 을 **아니요**로 변경 합니다.
3.  **MyAppService** 프로젝트의 **Inventory.cs** 파일에서 **onrequestreceived**에서 중단점을 설정 합니다.
4.  **AppServiceProvider** 프로젝트를 시작 프로젝트로 설정 하 고 **f5**키를 누릅니다.
5.  시작 메뉴 (Visual Studio가 아닌)에서 **ClientApp** 를 시작 합니다.
6.  텍스트 상자에 숫자 1을 입력 하 고 단추를 누릅니다. 앱 서비스의 중단점에 대 한 app service 호출에서 디버거가 중지 됩니다.

## <a name="debug-the-client"></a>클라이언트 디버그

1.  이전 단계의 지침에 따라 app service를 호출 하는 클라이언트를 디버깅 합니다.
2.  시작 메뉴에서 **ClientApp** 를 시작 합니다.
3.  디버거를 **ApplicationFrameHost.exe** 프로세스가 아닌 **ClientApp.exe** 프로세스에 연결 합니다. Visual Studio에서 **디버그 &gt; 프로세스에 연결 ...** 을 선택 합니다.
4.  **ClientApp** 프로젝트에서 **단추를 \_ 클릭**하 여 중단점을 설정 합니다.
5.  이제 **ClientApp** 의 텍스트 상자에 숫자 1을 입력 하 고 단추를 클릭 하면 클라이언트와 app service의 중단점이 모두 적중 됩니다.

## <a name="general-app-service-troubleshooting"></a>일반 앱 서비스 문제 해결

앱 서비스에 연결을 시도한 후에 **Appunavailable 수 없음** 상태가 발생 하는 경우 다음을 확인 합니다.

- App service 공급자 프로젝트 및 app service 프로젝트가 배포 되었는지 확인 합니다. 클라이언트에 연결할 항목이 없으면 클라이언트를 실행 하기 전에 두 가지를 모두 배포 해야 합니다. **빌드**  >  **배포 솔루션**을 사용 하 여 Visual Studio에서 배포할 수 있습니다.
- **솔루션 탐색기**에서 app service 공급자 프로젝트가 app service를 구현 하는 프로젝트에 대 한 프로젝트 간 참조를 포함 하는지 확인 합니다.
- `<Extensions>`항목 및 해당 자식 요소가 위에 지정 된 대로 [appxmanifest.xml에 app Service 확장 추가](#appxmanifest)에 지정 된 대로 app service 공급자 프로젝트에 속한 **appxmanifest.xml** 파일에 추가 되었는지 확인 합니다.
- App service 공급자를 호출 하는 클라이언트의 [AppServiceConnection](/uwp/api/windows.applicationmodel.appservice.appserviceconnection.appservicename) 가 `<uap3:AppService Name="..." />` app service 공급자 프로젝트의 **appxmanifest.xml** 파일에 지정 된와 일치 하는지 확인 합니다.
- [PackageFamilyName가 AppServiceConnection](/uwp/api/windows.applicationmodel.appservice.appserviceconnection.packagefamilyname) 에 [App service 확장 추가](#appxmanifest) 에 지정 된 app service 공급자 구성 요소의 패키지 패밀리 이름과 일치 하는지 확인 합니다. appxmanifest.xml
- 이 예제의 경우와 같은 in-proc 앱 서비스의 경우 `EntryPoint` `<uap:Extension ...>` app service 공급자 프로젝트의 **appxmanifest.xml** 파일에 지정 된가 App service 프로젝트에서 [IBackgroundTask](/uwp/api/windows.applicationmodel.background.ibackgroundtask) 를 구현 하는 public 클래스의 네임 스페이스 및 클래스 이름과 일치 하는지 확인 합니다.

### <a name="troubleshoot-debugging"></a>디버깅 문제 해결

디버거가 app service 공급자 또는 app service 프로젝트의 중단점에서 중지 되지 않는 경우 다음을 확인 합니다.

- App service 공급자 프로젝트 및 app service 프로젝트가 배포 되었는지 확인 합니다. 클라이언트를 실행 하기 전에 두 가지를 모두 배포 해야 합니다. **빌드**  >  **배포 솔루션**을 사용 하 여 Visual Studio에서 배포할 수 있습니다.
- 디버깅 하려는 프로젝트가 시작 프로젝트로 설정 되어 있는지 확인 하 고, **F5 키** 를 누를 때 해당 프로젝트의 디버깅 속성이 프로젝트를 실행 하지 않도록 설정 되어 있는지 확인 합니다. 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **속성**을 클릭 한 다음 **디버그** (또는 c + +에서 **디버깅** )를 클릭 합니다. C #에서 **시작 작업** 을 시작 하지 않음 (시작 **시 코드 디버그**)으로 변경 합니다. C + +에서 **응용 프로그램 시작** 을 **아니요**로 설정 합니다.

## <a name="remarks"></a>설명

이 예제에서는 백그라운드 작업으로 실행 되 고 다른 앱에서 호출 하는 app service를 만드는 방법을 소개 합니다. 유의 해야 할 주요 사항은 다음과 같습니다.

* App service를 호스트 하는 백그라운드 작업을 만듭니다.
* `windows.appService`App service 공급자의 **appxmanifest.xml** 파일에 확장을 추가 합니다.
* 클라이언트 앱에서 연결할 수 있도록 app service 공급자의 패키지 패밀리 이름을 가져옵니다.
* App service 공급자 프로젝트에서 app service 프로젝트에 프로젝트 간 참조를 추가 합니다.
* [AppService AppServiceConnection](/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection) 를 사용 하 여 서비스를 호출 합니다.

## <a name="full-code-for-myappservice"></a>MyAppService에 대 한 전체 코드

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
* [App service 코드 샘플 (c #, c + + 및 VB)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)