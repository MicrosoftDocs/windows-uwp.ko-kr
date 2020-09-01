---
title: 결과를 위한 앱 실행
description: 다른 앱에서 앱을 시작 하 고 두 앱 간에 데이터를 교환 하는 방법을 알아봅니다. 이를 결과에 대 한 앱 시작 이라고 합니다.
ms.assetid: AFC53D75-B3DD-4FF6-9FC0-9335242EE327
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fa018920f069c0b4f1d963c6cdfd3213df08fb45
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158768"
---
# <a name="launch-an-app-for-results"></a>결과를 위한 앱 실행




**중요 API**

-   [**LaunchUriForResultsAsync**](/uwp/api/windows.system.launcher.launchuriforresultsasync)
-   [**ValueSet**](/uwp/api/Windows.Foundation.Collections.ValueSet)

다른 앱에서 앱을 시작 하 고 두 앱 간에 데이터를 교환 하는 방법을 알아봅니다. 이를 *결과에 대 한 앱 시작*이라고 합니다. 다음 예제에서는 [**LaunchUriForResultsAsync**](/uwp/api/windows.system.launcher.launchuriforresultsasync) 를 사용 하 여 결과에 대 한 앱을 시작 하는 방법을 보여 줍니다.

Windows 10의 새로운 앱 간 통신 Api를 통해 Windows 앱 및 Windows Web apps에서 앱을 시작 하 고 데이터 및 파일을 교환할 수 있습니다. 이렇게 하면 여러 앱에서 매시업 솔루션을 빌드할 수 있습니다. 이러한 새 Api를 사용 하 여 사용자가 여러 앱을 사용 해야 하는 복잡 한 작업을 이제 원활 하 게 처리할 수 있습니다. 예를 들어 앱에서 소셜 네트워킹 앱을 시작 하 여 연락처를 선택 하거나, 체크 아웃 앱을 시작 하 여 지불 프로세스를 완료할 수 있습니다.

결과를 위해 시작할 앱은 시작 된 앱 이라고 합니다. 앱을 시작 하는 앱은 호출 하는 앱 이라고 합니다. 이 예제에서는 호출 하는 앱과 시작 된 앱을 모두 작성 합니다.

## <a name="step-1-register-the-protocol-to-be-handled-in-the-app-that-youll-launch-for-results"></a>1 단계: 앱에서 처리할 프로토콜을 등록 하 여 결과를 시작 합니다.


시작 된 앱의 appxmanifest.xml 파일에서 프로토콜 확장을 ** &lt; 응용 프로그램 &gt; ** 섹션에 추가 합니다. 이 예제에서는 **app2app**라는 가상의 프로토콜을 사용 합니다.

프로토콜 확장의 **Returnresults** 특성은 다음 값 중 하나를 허용 합니다.

-   **선택 사항**- [**LaunchUriForResultsAsync**](/uwp/api/windows.system.launcher.launchuriforresultsasync) 메서드를 사용 하 여 결과에 대해 앱을 시작 하거나 [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync)를 사용 하는 결과에 대해 실행할 수 없습니다. **optional**을 사용하는 경우 시작된 앱이 결과를 위해 시작되었는지 확인해야 합니다. [**Onactivated**](/uwp/api/windows.ui.xaml.application.onactivated) 된 이벤트 인수를 확인 하 여이 작업을 수행할 수 있습니다. 인수의 [**IActivatedEventArgs**](/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.kind) 속성이 [**ActivationKind**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind)을 반환 하거나 이벤트 인수의 형식이 [**ProtocolActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs)인 경우 앱은 **LaunchUriForResultsAsync**를 통해 시작 됩니다.
-   **always**-앱은 결과에 대해서만 시작할 수 있습니다. 즉, [**LaunchUriForResultsAsync**](/uwp/api/windows.system.launcher.launchuriforresultsasync)에만 응답할 수 있습니다.
-   **없음**-결과에 대해 앱을 시작할 수 없습니다. [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync)에만 응답할 수 있습니다.

이 프로토콜-확장 예제에서 앱은 결과에 대해서만 시작할 수 있습니다. 이렇게 하면 응용 프로그램을 활성화할 수 있는 다른 방법이 아니라 "시작 된 결과만 처리 해야 합니다."의 경우를 처리 해야 하기 때문에 아래에서 설명 하는 **Onactivated** 된 메서드 내의 논리가 간소화 됩니다.

```xml
<Applications>
   <Application ...>

     <Extensions>
       <uap:Extension Category="windows.protocol">
         <uap:Protocol Name="test-app2app" ReturnResults="always">
           <uap:DisplayName>Test app-2-app</uap:DisplayName>
         </uap:Protocol>
       </uap:Extension>
     </Extensions>

   </Application>
</Applications>
```

## <a name="step-2-override-applicationonactivated-in-the-app-that-youll-launch-for-results"></a>2 단계: 결과를 시작 하는 앱에서 응용 프로그램을 다시 정의 합니다.


이 메서드가 시작 된 앱에 아직 없는 경우 `App` App.xaml.cs에 정의 된 클래스 내에서 만듭니다.

소셜 네트워크에서 친구를 선택할 수 있는 앱에서이 기능은 사람 선택 페이지를 열 수 있습니다. 다음 예제에서는 결과에 대해 앱이 활성화 될 때 **LaunchedForResultsPage** 라는 페이지가 표시 됩니다. **using** 문이 파일의 맨 위에 포함되어 있는지 확인하세요.

```cs
using Windows.ApplicationModel.Activation;
...
protected override void OnActivated(IActivatedEventArgs args)
{
    // Window management
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame == null)
    {
        rootFrame = new Frame();
        Window.Current.Content = rootFrame;
    }

    // Code specific to launch for results
    var protocolForResultsArgs = (ProtocolForResultsActivatedEventArgs)args;
    // Open the page that we created to handle activation for results.
    rootFrame.Navigate(typeof(LaunchedForResultsPage), protocolForResultsArgs);

    // Ensure the current window is active.
    Window.Current.Activate();
}
```

Appxmanifest.xml 파일의 프로토콜 확장은 **Returnresults** 를 **always**로 지정 하기 때문에 방금 표시 된 코드는 `args` 이 앱에 대해 **onactivated** 된 상태로 전송 되는 경우에만 **ProtocolForResultsActivatedEventArgs** 를 신뢰 하는 [**ProtocolForResultsActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.ProtocolForResultsActivatedEventArgs) 로 직접 캐스팅할 수 있습니다. 결과를 시작 하는 것 외에 다른 방법으로 앱을 활성화할 수 있는 경우 [**IActivatedEventArgs**](/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.kind) 속성이 [**ActivationKind**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind) 을 반환 하는지 여부를 확인 하 여 앱이 결과에 대해 시작 되었는지 여부를 확인할 수 있습니다.

## <a name="step-3-add-a-protocolforresultsoperation-field-to-the-app-you-launch-for-results"></a>3 단계: 결과를 위해 시작 하는 앱에 ProtocolForResultsOperation 필드 추가


```cs
private Windows.System.ProtocolForResultsOperation _operation = null;
```

[**ProtocolForResultsOperation**](/uwp/api/windows.applicationmodel.activation.protocolforresultsactivatedeventargs.protocolforresultsoperation) 필드를 사용 하 여 시작 된 앱이 호출 앱에 결과를 반환할 준비가 되었을 때 신호를 보낼 수 있습니다. 이 예제에서는 해당 페이지에서 결과 실행 작업을 완료 하 고 해당 페이지에 액세스 해야 하므로 필드가 **LaunchedForResultsPage** 클래스에 추가 됩니다.

## <a name="step-4-override-onnavigatedto-in-the-app-you-launch-for-results"></a>4 단계: 결과를 위해 시작 하는 앱에서 OnNavigatedTo () 재정의


앱이 시작 될 때 결과를 표시 하는 페이지에서 [**OnNavigatedTo**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) 메서드를 재정의 합니다. 이 메서드가 아직 없으면 클래스 내에서 xaml.cs에 정의 된 페이지를 만듭니다 &lt; &gt; . 다음 **using** 문을 파일 맨 위에 포함 해야 합니다.

```cs
using Windows.ApplicationModel.Activation
```

[**OnNavigatedTo**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) 메서드의 [**navigationeventargs**](/uwp/api/Windows.UI.Xaml.Navigation.NavigationEventArgs) 개체는 호출 하는 앱에서 전달 된 데이터를 포함 합니다. 데이터는 100KB를 초과할 수 없으며 [**Valueset**](/uwp/api/Windows.Foundation.Collections.ValueSet) 개체에 저장 됩니다.

이 예제 코드에서 시작 된 앱은 호출 앱에서 보낸 데이터가 **TestData**이라는 키 아래에 있는 [**valueset**](/uwp/api/Windows.Foundation.Collections.ValueSet) 에 있다고 예상 합니다 .이는 예제의 호출 앱이 send로 코딩 되기 때문입니다.

```cs
using Windows.ApplicationModel.Activation;
...
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    var protocolForResultsArgs = e.Parameter as ProtocolForResultsActivatedEventArgs;
    // Set the ProtocolForResultsOperation field.
    _operation = protocolForResultsArgs.ProtocolForResultsOperation;

    if (protocolForResultsArgs.Data.ContainsKey("TestData"))
    {
        string dataFromCaller = protocolForResultsArgs.Data["TestData"] as string;
    }
}
...
private Windows.System.ProtocolForResultsOperation _operation = null;
```

## <a name="step-5-write-code-to-return-data-to-the-calling-app"></a>5 단계: 호출 하는 앱에 데이터를 반환 하는 코드 작성


시작 된 앱에서 [**ProtocolForResultsOperation**](/uwp/api/windows.applicationmodel.activation.protocolforresultsactivatedeventargs.protocolforresultsoperation) 를 사용 하 여 호출 하는 앱에 데이터를 반환 합니다. 이 예제 코드에서는 호출 하는 앱에 반환할 값을 포함 하는 [**Valueset**](/uwp/api/Windows.Foundation.Collections.ValueSet) 개체가 만들어집니다. 그런 다음 **ProtocolForResultsOperation** 필드를 사용 하 여 값을 호출 하는 앱으로 보냅니다.

```cs
    ValueSet result = new ValueSet();
    result["ReturnedData"] = "The returned result";
    _operation.ReportCompleted(result);
```

## <a name="step-6-write-code-to-launch-the-app-for-results-and-get-the-returned-data"></a>6 단계: 결과를 위해 앱을 시작 하 고 반환 된 데이터를 가져오는 코드 작성


이 예제 코드에 표시 된 것 처럼 호출 하는 앱의 비동기 메서드 내에서 앱을 시작 합니다. 코드를 컴파일하는 데 필요한 **using** 문을 적어둡니다.

```cs
using System.Threading.Tasks;
using Windows.System;
...

async Task<string> LaunchAppForResults()
{
    var testAppUri = new Uri("test-app2app:"); // The protocol handled by the launched app
    var options = new LauncherOptions();
    options.TargetApplicationPackageFamilyName = "67d987e1-e842-4229-9f7c-98cf13b5da45_yd7nk54bq29ra";

    var inputData = new ValueSet();
    inputData["TestData"] = "Test data";

    string theResult = "";
    LaunchUriResult result = await Windows.System.Launcher.LaunchUriForResultsAsync(testAppUri, options, inputData);
    if (result.Status == LaunchUriStatus.Success &&
        result.Result != null &&
        result.Result.ContainsKey("ReturnedData"))
    {
        ValueSet theValues = result.Result;
        theResult = theValues["ReturnedData"] as string;
    }
    return theResult;
}
```

이 예제에서는 **TestData** 키를 포함 하는 [**valueset**](/uwp/api/Windows.Foundation.Collections.ValueSet) 이 시작 된 앱에 전달 됩니다. 시작 된 앱은 호출자에 게 반환 되는 결과를 포함 하는 **ReturnedData** 라는 키를 사용 하 여 **valueset** 을 만듭니다.

호출 하는 앱을 실행 하기 전에 결과에 대해 시작할 앱을 빌드하고 배포 해야 합니다. 그렇지 않으면 [**Launchuriresult. Status**](/uwp/api/Windows.System.LaunchUriStatus) 는 **LaunchUriStatus**을 보고 하지 않습니다.

[**TargetApplicationPackageFamilyName**](/uwp/api/windows.system.launcheroptions.targetapplicationpackagefamilyname)을 설정할 때 시작 된 앱의 제품군 이름이 필요 합니다. 제품군 이름을 가져오는 한 가지 방법은 시작 된 앱 내에서 다음 호출을 수행 하는 것입니다.

```cs
string familyName = Windows.ApplicationModel.Package.Current.Id.FamilyName;
```

## <a name="remarks"></a>설명


이 방법의 예제에서는 결과를 위해 앱을 시작 하는 "hello 세계" 소개를 제공 합니다. 주요 사항은 새로운 [**LaunchUriForResultsAsync**](/uwp/api/windows.system.launcher.launchuriforresultsasync) API를 사용 하 여 앱을 비동기적으로 시작 하 고 [**valueset**](/uwp/api/Windows.Foundation.Collections.ValueSet) 클래스를 통해 통신할 수 있다는 것입니다. **Valueset** 을 통해 데이터를 전달 하는 것은 100KB로 제한 됩니다. 더 많은 양의 데이터를 전달 해야 하는 경우 [**SharedStorageAccessManager**](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) 클래스를 사용 하 여 응용 프로그램 간에 전달할 수 있는 파일 토큰을 만들어 파일을 공유할 수 있습니다. 예를 들어 라는 **Valueset** 가 지정 된 경우 `inputData` 시작 된 앱과 공유 하려는 파일에 토큰을 저장할 수 있습니다.

```cs
inputData["ImageFileToken"] = SharedStorageAccessManager.AddFile(myFile);
```

그런 다음 **LaunchUriForResultsAsync**을 통해 시작 된 앱에 전달 합니다.

## <a name="related-topics"></a>관련 항목


* [**LaunchUri**](/uwp/api/windows.system.launcher.launchuriasync)
* [**LaunchUriForResultsAsync**](/uwp/api/windows.system.launcher.launchuriforresultsasync)
* [**ValueSet**](/uwp/api/Windows.Foundation.Collections.ValueSet)

 

 