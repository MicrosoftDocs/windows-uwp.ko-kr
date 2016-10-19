---
author: TylerMSFT
title: "결과에 대한 앱 실행"
description: "다른 앱에서 앱을 시작하고 두 사이에서 데이터를 교환하는 방법을 알아봅니다. 이를 결과에 의한 앱 시작이라고 합니다."
ms.assetid: AFC53D75-B3DD-4FF6-9FC0-9335242EE327
translationtype: Human Translation
ms.sourcegitcommit: 213384a194513a0f98a5f37e7f0e0849bf0a66e2
ms.openlocfilehash: d8d7f73e06d627eaa53deaf26f778c122113a9d6

---

# 결과에 대한 앱 실행


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686)
-   [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)

다른 앱에서 앱을 시작하고 두 사이에서 데이터를 교환하는 방법을 알아봅니다. 이것은 *결과에 의한 앱 시작*이라고 합니다. 여기에 있는 예제에서는 [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686)를 사용하여 결과를 위해 앱을 실행하는 방법을 보여줍니다.

Windows 10의 새로운 앱 간 통신 API를 통해 Windows 앱과 Windows 웹앱에서 앱을 시작하고 데이터 및 파일을 교환할 수 있습니다. 이렇게 하면 여러 앱에서 매시업 솔루션을 빌드할 수 있습니다. 이러한 새 API를 사용하면 이전에는 여러 앱을 사용해야 했던 복잡한 작업도 원활하게 처리할 수 있습니다. 예를 들어 앱에서 소셜 네트워킹 앱을 실행하여 연락처를 선택하거나 체크 아웃 앱을 실행하여 지급 프로세스를 완료할 수 있습니다.

결과를 위해 시작할 앱은 시작된 앱이라고 합니다. 앱을 시작하는 앱은 호출 앱이라고 합니다. 이 예에서는 호출 앱과 시작된 앱을 모두 작성합니다.

## 1단계: 결과를 위해 실행할 앱에서 처리할 프로토콜 등록


시작된 앱의 Package.appxmanifest 파일에서 **&lt;Application&gt;** 섹션에 프로토콜 확장을 추가합니다. 다음 예제에서는 **test-app2app**이라는 가상 프로토콜을 사용합니다.

프로토콜 확장의 **ReturnResults** 특성에서 다음 값 중 하나를 허용합니다.

-   **optional**—[**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686) 메서드를 사용하여 결과를 위해 또는 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)를 사용하여 결과가 아닌 다른 용도로 앱을 시작할 수 있습니다. **optional**을 사용하는 경우 시작된 앱이 결과를 위해 시작되었는지 확인해야 합니다. 이 작업은 [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 이벤트 인수를 확인하여 수행할 수 있습니다. 인수의 [**IActivatedEventArgs.Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) 속성에서 [**ActivationKind.ProtocolForResults**](https://msdn.microsoft.com/library/windows/apps/br224693)를 반환하거나 이벤트 인수의 형식이 [**ProtocolActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224742)인 경우 앱은 **LaunchUriForResultsAsync**를 통해 시작된 것입니다.
-   **always**—결과를 위해서만 앱을 시작할 수 있습니다. 즉, [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686)에만 응답할 수 있습니다.
-   **none**—결과를 위해 앱을 시작할 수 없습니다. [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)에만 응답할 수 있습니다.

이 프로토콜 확장 예에서 앱은 결과를 위해서만 시작할 수 있습니다. 여기서는 아래 논의된 **OnActivated** 메서드의 내부 논리를 간소화합니다. 앱을 활성화할 수 있는 다른 방법이 아닌 결과를 위해 시작된 경우만 처리하면 되기 때문입니다.

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

## 2단계: 결과를 위해 실행하는 앱에서 Application.OnActivated 재정의


이 메서드가 시작된 앱에 아직 없는 경우 App.xaml.cs에 정의된 `App` 클래스에서 만드세요.

소셜 네트워크에서 친구를 선택할 수 있는 앱에서 이 함수를 통해 피플 선택 페이지를 열 수 있습니다. 다음 예제에서는 앱이 결과를 위해 활성화 될 때 **LaunchedForResultsPage**라는 페이지가 표시됩니다. **using** 문이 파일의 맨 위에 포함되어 있는지 확인하세요.

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

Package.appxmanifest 파일의 프로토콜 확장이 **ReturnResults**를 **always**로 지정하기 때문에 위의 코드가 [**ProtocolForResultsActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn906905)에 `args`를 직접 캐스팅할 수 있습니다. 이때 **ProtocolForResultsActivatedEventArgs**만 이 앱에 대한 **OnActivated**로 전송됩니다. 결과를 위한 시작이 아닌 다른 방법으로 앱을 활성화할 수 있는 경우 [**IActivatedEventArgs.Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) 속성이 [**ActivationKind.ProtocolForResults**](https://msdn.microsoft.com/library/windows/apps/br224693)를 반환하는지 여부를 알기 위해 앱이 결과를 위해 시작되었는지를 알아볼 수 있습니다.

## 3단계: 결과를 위해 실행된 앱에 ProtocolForResultsOperation 필드 추가


```cs
private Windows.System.ProtocolForResultsOperation _operation = null;
```

[**ProtocolForResultsOperation**](https://msdn.microsoft.com/library/windows/apps/dn906913) 필드를 사용하여 시작된 앱이 호출 앱으로 결과를 반환할 준비가 되었을 때 신호를 보냅니다. 이 예제에서는 페이지에서 결과를 위한 시작 작업을 완료하고 그에 대한 액세스 권한이 필요하므로 필드가 **LaunchedForResultsPage** 클래스에 추가됩니다.

## 4단계: 결과를 위해 실행하는 앱에서 OnNavigatedTo() 재정의


결과를 위해 앱을 시작할 때 표시할 페이지에서 [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) 메서드를 재정의합니다. 이 메서드가 아직 없는 경우 &lt;pagename&gt;.xaml.cs에 정의된 페이지의 클래스에서 만드세요. 다음 **using** 문이 파일의 맨 위에 포함되어 있는지 확인하세요.

```cs
using Windows.ApplicationModel.Activation
```

[**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) 메서드의 [**NavigationEventArgs**](https://msdn.microsoft.com/library/windows/apps/br243285) 개체에는 호출 앱에서 전달된 데이터가 포함됩니다. 데이터는 100KB를 초과할 수 없으며 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 개체에 저장됩니다.

이 예제 코드에서 시작된 앱은 호출 앱에서 전송된 데이터가 **TestData**라는 키의 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)에 있어야 합니다. 이 위치가 예제 호출 앱이 데이터를 보내도록 코드화된 곳이기 때문입니다.

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

## 5단계: 호출자가 앱에 데이터를 반환하는 코드 작성


시작된 앱에서 [**ProtocolForResultsOperation**](https://msdn.microsoft.com/library/windows/apps/dn906913)을 사용하여 호출 앱에 데이터를 반환합니다. 이 예제 코드에서는 호출 앱으로 반환할 값이 포함된 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 개체가 생성됩니다. 그런 다음 **ProtocolForResultsOperation** 필드를 사용하여 호출 앱에 값을 보냅니다.

```cs
    ValueSet result = new ValueSet();
    result["ReturnedData"] = "The returned result";
    _operation.ReportCompleted(result);
```

## 6단계: 결과를 위해 앱을 실행하고 반환 된 데이터를 가져오는 코드 작성


이 예제 코드에 표시된 대로 호출 앱의 비동기 메서드에서 앱을 시작합니다. 코드를 컴파일하는 데 필요한 **using** 문을 참고하세요.

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

이 예제에서 **TestData** 키가 포함된 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)이 시작된 앱에 전달됩니다. 시작된 앱에서 호출자에게 반환되는 결과가 포함된 **ReturnedData**라는 키가 있는 **ValueSet**을 만듭니다.

호출 앱을 실행하기 전에 결과를 위해 시작할 앱을 빌드하고 배포해야 합니다. 그러지 않은 경우에는 [**LaunchUriResult.Status**](https://msdn.microsoft.com/library/windows/apps/dn906892)에서 **LaunchUriStatus.AppUnavailable**을 보고합니다.

[**TargetApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/dn893511)을 설정할 때 시작된 앱의 제품군 이름이 필요합니다. 패밀리 이름을 가져오는 한 가지 방법은 시작된 앱에서 다음을 호출하는 것입니다.

```cs
string familyName = Windows.ApplicationModel.Package.Current.Id.FamilyName;
```

## 설명


이 방법의 예제에서는 결과를 위해 앱을 시작하는 "hello world"를 소개합니다. 주의해야 할 사항은 새로운 [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686) API를 사용하여 앱을 비동기적으로 시작하고 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 클래스를 통해 통신할 수 있다는 것입니다. **ValueSet**을 통해 전달되는 데이터를 100KB로 제한됩니다. 더 많은 양의 데이터를 전달해야 하는 경우 [**SharedStorageAccessManager**](https://msdn.microsoft.com/library/windows/apps/dn889985) 클래스로 파일을 공유하여 앱 간에 전달할 수 있는 파일 토큰을 만들 수 있습니다. 예를 들어 `inputData`라는 **ValueSet**이 있는 경우 시작된 앱과 공유할 파일에 토큰을 저장할 수 있습니다.

```cs
inputData["ImageFileToken"] = SharedStorageAccessManager.AddFile(myFile);
```

그런 다음 **LaunchUriForResultsAsync**를 통해 시작된 앱에 전달합니다.

## 관련 항목


* [**LaunchUri**](https://msdn.microsoft.com/library/windows/apps/hh701476)
* [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686)
* [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)

 

 



<!--HONumber=Aug16_HO3-->


