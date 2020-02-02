---
title: 앱 확장 만들기 및 호스팅
description: 사용자가 Microsoft Store에서 설치할 수 있는 패키지를 통해 앱을 확장할 수 있도록 하는 앱 확장을 작성 하 고 호스팅합니다.
keywords: 앱 확장, 앱 서비스, 백그라운드
ms.date: 01/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d315fb89f38e517e61194adf5b75a28b4675de9c
ms.sourcegitcommit: 09571e1c6a01fabed773330aa7ead459a47d94f7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76929281"
---
# <a name="create-and-host-an-app-extension"></a>앱 확장 만들기 및 호스팅

이 문서에서는 Windows 10 앱 확장을 만들고 앱에서 호스트 하는 방법을 보여 줍니다. 앱 확장은 UWP 앱 및 패키지 된 [데스크톱 앱](/windows/apps/desktop/modernize/#msix-packages)에서 지원 됩니다.

앱 확장을 만드는 방법을 보여 주기 위해이 문서에서는 [수학 확장 코드 샘플](https://github.com/MicrosoftDocs/windows-topic-specific-samples/tree/MathExtensionSample)의 패키지 매니페스트 XML 및 코드 조각을 사용 합니다. 이 샘플은 UWP 앱 이지만 샘플에서 설명 하는 기능은 패키지 된 데스크톱 앱에도 적용 됩니다. 다음 지침에 따라 샘플을 시작 하세요.

- [수학 확장 코드 샘플](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/MathExtensionSample.zip)을 다운로드 하 고 압축을 풉니다.
- Visual Studio 2019에서 MathExtensionSample를 엽니다. 빌드 유형을 x86(**빌드** > **구성 관리자**에서 두 프로젝트에 대해 **플랫폼**을 **x86**으로 변경)으로 설정합니다.
- 솔루션 배포합니다(**빌드** > **솔루션 배포**).

## <a name="introduction-to-app-extensions"></a>앱 확장 소개

Windows 10에서 앱 확장은 다른 플랫폼에서 플러그 인, 추가 기능 및 추가 기능을 수행 하는 것과 비슷한 기능을 제공 합니다. 앱 확장은 Windows 10 기념일 버전 (버전 1607, 빌드 10.0.14393)에 도입 되었습니다.

앱 확장은 호스트 앱과 콘텐츠 및 배포 이벤트를 공유할 수 있도록 하는 확장 선언이 있는 UWP 앱 또는 패키지 된 데스크톱 앱입니다. 확장 앱은 여러 확장을 제공할 수 있습니다.

앱 확장은 UWP 앱 또는 패키지 된 데스크톱 응용 프로그램 이기 때문에 완전히 작동 하는 앱이 될 수 있으며, 호스트 확장이 가능 하 고, 다른 앱에 대 한 확장을 제공할 수도 있습니다. 즉, 별도의 앱 패키지를 만들지 않아도 됩니다.

앱 확장 호스트를 만들 때 앱에 대한 에코시스템을 개발하여 다른 개발자가 예상하지 못했던 방법이나 리소스로 앱을 향상시킬 수 있는 기회를 만들 수 있습니다. Microsoft Office 확장, Visual Studio 확장, 브라우저 확장 등을 고려해 보세요. 이는 함께 제공되는 기능을 뛰어넘어 앱에 대한 더 풍부한 환경을 제공합니다. 확장은 앱에 가치와 지속성을 더할 수 있습니다.

앱 확장 관계를 설정하려면 다음을 수행해야 합니다.

1. 앱을 확장 호스트로 선언합니다.
2. 앱을 확장으로 선언합니다.
3. 앱 서비스, 백그라운드 작업 또는 다른 방법으로 확장을 구현할지 여부를 결정합니다.
4. 호스트와 해당 확장이 통신하는 방법을 정의합니다.
5. 호스트 앱에서 [Windows.ApplicationModel.AppExtensions](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppExtensions) API를 사용하여 확장에 액세스할 수 있습니다.

확장을 통해 새로운 기능을 추가할 수 있는 가상 계산기를 구현하는 [Math Extension 코드 샘플](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/MathExtensionSample.zip)을 검사하여 이 작업이 어떻게 수행되는지 살펴보겠습니다. Microsoft Visual Studio 2019의 코드 샘플에서 **MathExtensionSample** 를 로드 합니다.

![Math Extension 코드 샘플](images/mathextensionhost-calctab.png)

## <a name="declare-an-app-to-be-an-extension-host"></a>앱을 확장 호스트로 선언

앱은 Package.appxmanifest 파일에서 `<AppExtensionHost>` 요소를 선언하여 자체적으로 앱 확장 호스트로 식별합니다. **MathExtensionHost** 프로젝트의 **Package.appxmanifest** 파일을 확인하여 이 작업이 어떻게 수행되는지 살펴봅니다.

_MathExtensionHost 프로젝트의 appxmanifest.xml_
```xml
<Package
  ...
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
  ...
    <Applications>
      <Application Id="App" ... >
        ...
        <Extensions>
            <uap3:Extension Category="windows.appExtensionHost">
                <uap3:AppExtensionHost>
                  <uap3:Name>com.microsoft.mathext</uap3:Name>
                </uap3:AppExtensionHost>
          </uap3:Extension>
        </Extensions>
      </Application>
    </Applications>
    ...
</Package>
```

`xmlns:uap3="http://..."` 및 `IgnorableNamespaces`의 `uap3`에 주목합니다. uap3 네임스페이스를 사용하기 때문에 이것이 필요합니다.

`<uap3:Extension Category="windows.appExtensionHost">`은이 앱을 확장 호스트로 식별 합니다.

`<uap3:AppExtensionHost>`의 **이름** 요소는 _확장 계약_ 이름입니다. 확장에서 동일한 확장 계약 이름을 지정하면 호스트가 해당 확장 계약 이름을 찾을 수 있습니다. 일반적으로 다른 확장 계약 이름과 잠재적인 충돌을 피하기 위해 앱 또는 게시자 이름을 사용하여 확장 계약 이름을 빌드하는 것이 좋습니다.

동일한 앱에서 여러 호스트 및 여러 확장을 정의할 수 있습니다. 이 예에서는 하나의 호스트를 선언합니다. 확장은 다른 앱에서 정의됩니다.

## <a name="declare-an-app-to-be-an-extension"></a>앱을 확장으로 선언

앱은 **Package.appxmanifest** 파일에서 `<uap3:AppExtension>` 요소를 선언하여 자체적으로 앱 확장으로 식별합니다. **MathExtension** 프로젝트의 **Package.appxmanifest** 파일을 열고 이 작업이 어떻게 수행되는지 살펴봅니다.

_MathExtension 프로젝트의 appxmanifest.xml:_
```xml
<Package
  ...
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
  ...
    <Applications>
      <Application Id="App" ... >
        ...
        <Extensions>
          ...
          <uap3:Extension Category="windows.appExtension">
            <uap3:AppExtension Name="com.microsoft.mathext"
                               Id="power"
                               DisplayName="x^y"
                               Description="Exponent"
                               PublicFolder="Public">
              <uap3:Properties>
                <Service>com.microsoft.powservice</Service>
              </uap3:Properties>
              </uap3:AppExtension>
          </uap3:Extension>
        </Extensions>
      </Application>
    </Applications>
    ...
</Package>
```

`xmlns:uap3="http://..."` 라인 및 `IgnorableNamespaces`의 `uap3`에 다시 주목합니다. `uap3` 네임스페이스를 사용하기 때문에 이것이 필요합니다.

`<uap3:Extension Category="windows.appExtension">`은이 앱을 확장으로 식별 합니다.

`<uap3:AppExtension>` 특성의 의미는 다음과 같습니다.

|특성|설명|필수|
|---------|-----------|:------:|
|**이름**|확장 계약 이름입니다. 호스트에서 선언된 **이름**과 일치하면 해당 호스트는 이 확장을 찾을 수 있습니다.| :heavy_check_mark: |
|**ID**| 이 확장을 고유하게 식별합니다. 동일한 확장 계약 이름을 사용하는 여러 개의 확장이 있을 수 있으므로(여러 확장을 지원하는 페인트 앱을 상상할 수 있음) ID를 사용하여 구분할 수 있습니다. 앱 확장 호스트는 ID를 사용하여 확장 유형에 대한 정보를 추측할 수 있습니다. 예를 들어 데스크톱용으로 설계된 확장과 차별화 요소인 ID를 사용하여 모바일용으로 설계된 확장 중 하나를 사용할 수 있습니다. 아래에서 설명하는 **속성** 요소를 사용할 수도 있습니다.| :heavy_check_mark: |
|**DisplayName**| 호스트 앱에서 사용자에 대한 확장을 식별하는 데 사용할 수 있습니다. 쿼리 가능하며, 지역화를 위해 [새로운 리소스 관리 시스템](https://docs.microsoft.com/windows/uwp/app-resources/using-mrt-for-converted-desktop-apps-and-games)(`ms-resource:TokenName`)을 사용할 수 있습니다. 지역화된 콘텐츠는 호스트 앱이 아니라 앱 확장 패키지에서 로드됩니다. | |
|**설명** | 호스트 앱에서 사용자에 대한 확장을 설명하는 데 사용할 수 있습니다. 쿼리 가능하며, 지역화를 위해 [새로운 리소스 관리 시스템](https://docs.microsoft.com/windows/uwp/app-resources/using-mrt-for-converted-desktop-apps-and-games)(`ms-resource:TokenName`)을 사용할 수 있습니다. 지역화된 콘텐츠는 호스트 앱이 아니라 앱 확장 패키지에서 로드됩니다. | |
|**PublicFolder**|확장 호스트와 콘텐츠를 공유할 수 있는 패키지 루트에 상대적인 폴더의 이름입니다. 일반적으로 이름은 "공개"이지만 확장의 폴더와 일치하는 이름을 사용할 수 있습니다.| :heavy_check_mark: |

`<uap3:Properties>`은 호스트에서 런타임에 읽을 수 있는 사용자 지정 메타 데이터를 포함 하는 선택적 요소입니다. 코드 샘플에서 확장은 앱 서비스로 구현되므로 호스트는 해당 앱 서비스의 이름을 가져와서 호출할 수 있어야 합니다. 앱 서비스의 이름은 미리 정의한 <Service> 요소에 정의되어 있습니다(원하는 대로 부를 수 있음). 코드 샘플의 호스트는 런타임 시 이 속성을 검색하여 앱 서비스의 이름을 확인합니다.

## <a name="decide-how-you-will-implement-the-extension"></a>확장을 구현하는 방법을 결정합니다.

[앱 확장에 대한 빌드 2016 세션](https://channel9.msdn.com/Events/Build/2016/B808)에서는 호스트와 확장 간에 공유되는 공용 폴더를 사용하는 방법을 보여 줍니다. 이 예에서 확장은 호스트가 호출하는 공용 폴더에 저장된 Javascript 파일에 의해 구현됩니다. 이러한 접근 방식은 가볍고 컴파일할 필요가 없다는 장점이 있으며 확장에 대한 지침 및 호스트 앱의 Microsoft Store 페이지에 대한 링크를 제공하는 기본 방문 페이지 생성을 지원할 수 있습니다. 자세한 내용은 [빌드 2016 앱 확장 코드 샘플](https://github.com/Microsoft/App-Extensibility-Sample)을 참조하세요. 특히 **InvertImageExtension** 프로젝트 및 **ExtensibilitySample** 프로젝트의 ExtensionManager.cs에 있는 `InvokeLoad()`를 참조하세요.

이 예에서는 앱 서비스를 사용하여 확장을 구현합니다. 앱 서비스에는 다음과 같은 이점이 있습니다.

- 확장이 충돌하면 호스트 앱이 자체 프로세스에서 실행되기 때문에 호스트 앱이 중단되지 않습니다.
- 원하는 언어를 사용하여 서비스를 구현할 수 있습니다. 호스트 앱을 구현하는 데 사용된 언어와 일치할 필요는 없습니다.
- 앱 서비스는 호스트와 다른 기능을 가진 자체 앱 컨테이너에 대한 액세스 권한을 가집니다.
- 서비스 중인 데이터와 호스트 앱 사이에는 격리가 있습니다.

### <a name="host-app-service-code"></a>호스트 앱 서비스 코드

확장의 앱 서비스를 호출하는 호스트 코드는 다음과 같습니다.

_MathExtensionHost 프로젝트의 ExtensionManager.cs_
```cs
public async Task<double> Invoke(ValueSet message)
{
    if (Loaded)
    {
        try
        {
            // make the app service call
            using (var connection = new AppServiceConnection())
            {
                // service name is defined in appxmanifest properties
                connection.AppServiceName = _serviceName;
                // package Family Name is provided by the extension
                connection.PackageFamilyName = AppExtension.Package.Id.FamilyName;

                // open the app service connection
                AppServiceConnectionStatus status = await connection.OpenAsync();
                if (status != AppServiceConnectionStatus.Success)
                {
                    Debug.WriteLine("Failed App Service Connection");
                }
                else
                {
                    // Call the app service
                    AppServiceResponse response = await connection.SendMessageAsync(message);
                    if (response.Status == AppServiceResponseStatus.Success)
                    {
                        ValueSet answer = response.Message as ValueSet;
                        if (answer.ContainsKey("Result")) // When our app service returns "Result", it means it succeeded
                        {
                            return (double)answer["Result"];
                        }
                    }
                }
            }
        }
        catch (Exception)
        {
             Debug.WriteLine("Calling the App Service failed");
        }
    }
    return double.NaN; // indicates an error from the app service
}
```

이것은 앱 서비스를 호출하기 위한 일반적인 코드입니다. 앱 서비스를 구현하고 호출하는 방법에 대한 자세한 내용은 [앱 서비스를 만들고 사용하는 방법](how-to-create-and-consume-an-app-service.md)을 참조하세요.

호출할 앱 서비스의 이름이 어떻게 결정되는지 주목해야 합니다. 호스트는 확장 구현에 대한 정보를 갖고 있지 않기 때문에, 확장은 해당 앱 서비스의 이름을 제공해야 합니다. 코드 샘플에서, 확장은 다음과 같이 `<uap3:Properties>` 요소에 있는 파일에서 앱 서비스 이름을 선언합니다.

_MathExtension 프로젝트의 appxmanifest.xml_
```xml
    ...
    <uap3:Extension Category="windows.appExtension">
      <uap3:AppExtension ...>
        <uap3:Properties>
          <Service>com.microsoft.powservice</Service>
        </uap3:Properties>
        </uap3:AppExtension>
    </uap3:Extension>
```

`<uap3:Properties>`요소에서 자체 XML을 정의할 수 있습니다. 이 경우 앱 서비스의 이름을 정의하여 호스트가 확장을 호출할 때 앱 서비스의 이름을 지정할 수 있습니다.

호스트가 확장을 로드하면 다음과 같은 코드가 확장의 Package.appxmanifest에 정의된 속성에서 서비스의 이름을 추출합니다.

_ExtensionManager.cs의 `Update()` MathExtensionHost 프로젝트_
```cs
...
var properties = await ext.GetExtensionPropertiesAsync() as PropertySet;

...
#region Update Properties
// update app service information
_serviceName = null;
if (_properties != null)
{
   if (_properties.ContainsKey("Service"))
   {
       PropertySet serviceProperty = _properties["Service"] as PropertySet;
       this._serviceName = serviceProperty["#text"].ToString();
   }
}
#endregion
```

앱 서비스의 이름이 `_serviceName`에 저장되면 호스트는 이를 사용하여 앱 서비스를 호출할 수 있습니다.

앱 서비스를 호출하려면 앱 서비스가 포함된 패키지의 패밀리 이름도 필요합니다. 다행히 앱 확장 API는 줄에서 얻을 수 있는이 정보를 제공 합니다. `connection.PackageFamilyName = AppExtension.Package.Id.FamilyName;`

### <a name="define-how-the-host-and-the-extension-will-communicate"></a>호스트와 해당 확장이 통신하는 방법을 정의합니다.

앱 서비스에서는 [ValueSet](https://docs.microsoft.com/uwp/api/windows.foundation.collections.valueset)를 사용하여 정보를 교환합니다. 호스트 작성자는 유연한 확장과 통신하기 위한 프로토콜을 제시해야 합니다. 코드 샘플에서 이는 향후 1, 2 또는 그 이상의 인수를 취할 수 있는 확장을 의미합니다.

이 예제에서 인수에 대 한 프로토콜은 ' Arg ' 라는 키 값 쌍을 포함 하는 **Valueset** 이며 인수 번호 (예: `Arg1` 및 `Arg2`)입니다. 호스트는 **ValueSet**에 있는 모든 인수를 전달하며 확장은 필요한 인수를 사용합니다. 확장이 결과를 계산할 수 있는 경우 호스트는 확장에서 반환된 **ValueSet**가 계산 값을 포함하는 `Result`라는 키를 가질 것으로 예상합니다. 해당 키가 없으면 호스트는 확장이 계산을 완료할 수 없다고 가정합니다.

### <a name="extension-app-service-code"></a>확장 앱 서비스 코드

코드 샘플에서 확장의 앱 서비스는 백그라운드 작업으로 구현되지 않습니다. 대신, 앱 서비스를 호스팅하는 확장 앱과 동일한 프로세스에서 실행되는 단일 proc 앱 서비스 모델을 사용합니다. 이는 호스트 앱과 다른 프로세스이며, 프로세스 분리의 이점을 제공하는 동시에 확장 프로세스와 앱 서비스를 구현하는 백그라운드 프로세스 간의 통신을 피함으로써 성능상의 이점을 얻을 수 있습니다. [앱 서비스가 호스트 앱과 동일한 프로세스에서 실행되도록 변환](convert-app-service-in-process.md)을 참조하여 백그라운드 서비스로 실행되는 앱 서비스와 동일한 프로세스에서 실행되는 앱 서비스의 차이점을 확인하세요.

앱 서비스가 활성화되면 시스템은 `OnBackgroundActivate()`를 만듭니다. 이 코드는 이벤트 처리기가 실제 앱 서비스 호출(`OnAppServiceRequestReceived()`)을 처리할 뿐 아니라 취소 또는 닫힌 이벤트를 처리하는 지연 개체를 가져오는 것과 같은 정리 이벤트를 처리하도록 설정합니다.

_MathExtension 프로젝트의 App.xaml.cs._
```cs
protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    base.OnBackgroundActivated(args);

    if ( _appServiceInitialized == false ) // Only need to setup the handlers once
    {
        _appServiceInitialized = true;

        IBackgroundTaskInstance taskInstance = args.TaskInstance;
        taskInstance.Canceled += OnAppServicesCanceled;

        AppServiceTriggerDetails appService = taskInstance.TriggerDetails as AppServiceTriggerDetails;
        _appServiceDeferral = taskInstance.GetDeferral();
        _appServiceConnection = appService.AppServiceConnection;
        _appServiceConnection.RequestReceived += OnAppServiceRequestReceived;
        _appServiceConnection.ServiceClosed += AppServiceConnection_ServiceClosed;
    }
}
```

확장의 작업을 수행하는 코드는 `OnAppServiceRequestReceived()`에 있습니다. 이 함수는 계산을 수행하기 위해 앱 서비스가 호출될 때 호출됩니다. 이는 **ValueSet**에서 필요한 값을 추출합니다. 계산을 수행할 수 있는 경우 호스트에 반환된 **ValueSet**에 **Result**라는 키 아래에 결과를 저장합니다. 이 호스트와 확장이 통신하는 방법에 대해 정의된 프로토콜에 따라 **Result** 키가 있으면 성공을 나타내고 그렇지 않으면 실패를 나타냅니다.

_MathExtension 프로젝트의 App.xaml.cs._
```cs
private async void OnAppServiceRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below (SendResponseAsync()) to respond to the message
    // and we don't want this call to get cancelled while we are waiting.
    AppServiceDeferral messageDeferral = args.GetDeferral();
    ValueSet message = args.Request.Message;
    ValueSet returnMessage = new ValueSet();

    double? arg1 = Convert.ToDouble(message["arg1"]);
    double? arg2 = Convert.ToDouble(message["arg2"]);
    if (arg1.HasValue && arg2.HasValue)
    {
        returnMessage.Add("Result", Math.Pow(arg1.Value, arg2.Value)); // For this sample, the presence of a "Result" key will mean the call succeeded
    }

    await args.Request.SendResponseAsync(returnMessage);
    messageDeferral.Complete();
}
```

## <a name="manage-extensions"></a>확장 관리

호스트와 확장 사이의 관계를 구현하는 방법을 알아보았으므로 이제 호스트가 시스템에 설치된 확장을 찾고, 확장을 포함하는 패키지의 추가 및 제거에 어떻게 응답하는지 살펴보겠습니다.

Microsoft Store는 패키지로 확장을 제공합니다. [AppExtensionCatalog](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions.appextensioncatalog)는 호스트의 확장 계약 이름과 일치하는 확장을 포함하는 설치된 패키지를 찾고 호스트와 관련된 앱 확장 패키지를 설치하거나 제거할 때 실행되는 이벤트를 제공합니다.

코드 샘플에서 `ExtensionManager` 클래스(**MathExtensionHost** 프로젝트의 **ExtensionManager.cs** 에 정의됨)는 확장을 로드하고 확장 패키지 설치 및 제거에 응답하기 위한 논리를 래핑합니다.

`ExtensionManager` 생성자는 `AppExtensionCatalog`를 사용하여 시스템에서 호스트와 동일한 확장 계약 이름을 가진 앱 확장을 찾습니다.

_MathExtensionHost 프로젝트의 ExtensionManager.cs._
```cs
public ExtensionManager(string extensionContractName)
{
   // catalog & contract
   ExtensionContractName = extensionContractName;
   _catalog = AppExtensionCatalog.Open(ExtensionContractName);
   ...
}
```

확장 패키지가 설치되면 `ExtensionManager`는 호스트와 동일한 확장 계약 이름을 가진 패키지의 확장에 대한 정보를 수집합니다. 설치는 영향을 받는 확장 정보가 업데이트되는 경우의 업데이트를 나타낼 수 있습니다. 확장 패키지가 제거되면 `ExtensionManager`는 영향을 받는 확장에 대한 정보를 제거하여 사용자가 더 이상 사용할 수 없는 확장을 알게 합니다.

`Extension` 클래스(**MathExtensionHost** 프로젝트의 **ExtensionManager.cs**에 정의됨)는 확장 ID, 설명, 로고 및 사용자가 확장을 사용하도록 설정했는지 여부와 같은 앱 관련 정보에 액세스하기 위한 코드 샘플용으로 생성되었습니다.

확장이 로드(**ExtensionManager.cs**의 `Load()` 참조)되었다는 것은 패키지 상태가 양호하고 ID, 로고, 설명 및 공용 폴더(이 샘플에서는 사용하지 않고 가져오는 방법을 보여 주기만 함)를 얻었다는 점을 의미합니다. 확장 패키지 자체는 로드되지 않습니다.

언로드 개념은 더 이상 사용자에게 표시되지 않아야 할 확장을 추적하는 데 사용됩니다.

확장, 이름, 설명 및 로고가 UI에 바인딩된 데이터가 될 수 있도록 `ExtensionManager`는 컬렉션 `Extension` 인스턴스를 제공합니다. **ExtensionsTab** 페이지는 이 컬렉션에 바인딩되며 확장을 활성화/비활성화하고 제거하는 데 필요한 UI를 제공합니다.

![확장 탭 예제 UI](images/mathextensionhost-extensiontab.png)

 확장이 제거되면 시스템은 사용자에게 해당 확장 또는 다른 확장이 포함된 패키지를 제거할 것인지 확인하는 메시지를 표시합니다. 사용자가 동의하면 패키지가 제거되고 `ExtensionManager`는 제거된 패키지의 확장을 호스트 앱에 대해 사용할 수 있는 확장 목록에서 제거합니다.

 ![UI 제거](images/mathextensionhost-uninstall.png)

## <a name="debugging-app-extensions-and-hosts"></a>앱 확장 및 호스트 디버깅

종종 확장 호스트와 확장은 동일한 솔루션의 일부가 아닙니다. 이 경우 호스트와 확장을 디버깅하려면 다음을 수행합니다.

1. Visual Studio의 한 인스턴스에서 호스트 프로젝트를 로드합니다.
2. Visual Studio의 다른 인스턴스에서 확장을 로드합니다.
3. 디버거에서 호스트 앱을 시작합니다.
4. 디버거에서 확장 앱을 시작합니다. (확장을 디버그하는 대신 배포하여 호스트의 패키지 설치 이벤트를 테스트하려면 **빌드 &gt; 솔루션 배포**를 수행합니다.)

이제 호스트와 확장에서 중단점에 도달할 수 있습니다.
확장 앱 자체 디버깅을 시작하면 앱의 빈 창이 표시됩니다. 빈 창이 보이지 않게 하려면 확장 프로젝트의 디버깅 설정을 앱이 시작되지 않게 변경하여 시작할 때 디버그되도록 할 수 있습니다(확장 프로젝트를 마우스 오른쪽 버튼으로 클릭하고 **속성** > **디버그** > **시작하지 않음(시작 시 코드 디버그)** 선택). 확장 프로젝트 디버깅(**F5**)을 시작해야 하지만 이 프로젝트는 호스트가 확장을 활성화하고 해당 확장의 중단점에 도달할 때까지 기다리게 됩니다.

**코드 샘플 디버그**

코드 샘플에서 호스트와 확장은 동일한 솔루션에 있습니다. 디버그하려면 다음을 수행합니다.

1. **MathExtensionHost**는 시작 프로젝트여야 합니다(**MathExtensionHost** 프로젝트를 마우스 오른쪽 버튼으로 클릭하고 **시작 프로젝트로 설정** 클릭).
2. **MathExtensionHost** 프로젝트에 있는 ExtensionManager.cs의 `Invoke`에 중단점을 삽입합니다.
3. **F5**로 **MathExtensionHost** 프로젝트를 실행합니다.
4. **MathExtension** 프로젝트에 있는 App.xaml.cs의 `OnAppServiceRequestReceived`에 중단점을 삽입합니다.
5. 배포하고 패키지 설치 이벤트를 호스트에 트리거할 **MathExtension** 프로젝트 디버깅을 시작합니다(**MathExtension** 프로젝트를 마우스 오른쪽 버튼으로 클릭하고 **디버그 > 새 인스턴스 시작** 선택).
6. **MathExtensionHost** 앱에서 **계산** 페이지로 이동하고 **x^y**를 클릭하여 확장을 활성화합니다. `Invoke()` 중단점에 먼저 도달하고 확장 앱 서비스 호출이 발생하는 것을 확인할 수 있습니다. 그런 다음 `OnAppServiceRequestReceived()` 메서드가 실행되면 앱 서비스가 결과를 계산하고 반환하는 것을 확인할 수 있습니다.

**App service로 구현 된 확장 문제 해결**

확장 호스트가 확장의 앱 서비스에 연결하는 데 문제가 있는 경우 `<uap:AppService Name="...">` 특성이 `<Service>` 요소에 배치한 항목과 일치하게 합니다. 일치하지 않는 경우 확장이 호스트에 제공하는 서비스 이름은 구현한 앱 서비스 이름과 일치하지 않게 되며, 호스트가 확장을 활성화할 수 없습니다.

_MathExtension 프로젝트의 appxmanifest.xml:_
```xml
<Extensions>
   <uap:Extension Category="windows.appService">
     <uap:AppService Name="com.microsoft.sqrtservice" />      <!-- This must match the contents of <Service>...</Service> -->
   </uap:Extension>
   <uap3:Extension Category="windows.appExtension">
     <uap3:AppExtension Name="com.microsoft.mathext" Id="sqrt" DisplayName="Sqrt(x)" Description="Square root" PublicFolder="Public">
       <uap3:Properties>
         <Service>com.microsoft.powservice</Service>   <!-- this must match <uap:AppService Name=...> -->
       </uap3:Properties>
     </uap3:AppExtension>
   </uap3:Extension>
</Extensions>   
```

## <a name="a-checklist-of-basic-scenarios-to-test"></a>테스트할 기본 시나리오 검사 목록

확장 호스트를 빌드하고 확장을 얼마나 잘 지원하는지 테스트할 준비가 되면 다음과 같은 몇 가지 기본 시나리오를 시도해 봅니다.

- 호스트를 실행한 다음 확장 앱을 배포합니다.  
    - 호스트가 실행되는 동안 제공되는 새로운 확장을 선택합니까?  
- 확장 앱을 배포한 다음 호스트를 배포하고 실행합니다.
    - 호스트가 기존 확장을 선택합니까?  
- 호스트를 실행한 다음 확장 앱을 제거합니다.
    - 호스트가 올바르게 제거를 감지합니까?
- 호스트를 실행한 다음 확장 앱을 최신 버전으로 업데이트합니다.
    - 호스트가 변경 사항을 선택하고 이전 확장 버전을 제대로 언로드합니까?  

**테스트할 고급 시나리오:**

- 호스트를 실행하고 확장 앱을 이동식 미디어로 이동한 다음 미디어를 제거합니다.
    - 호스트가 패키지 상태의 변경을 감지하고 확장을 비활성화합니까?
- 호스트를 실행한 다음 확장 앱을 손상시킵니다(유효하지 않게 하거나 다르게 서명하는 방식 등).
    - 호스트가 훼손된 확장을 감지하고 올바르게 처리합니까?
- 호스트를 실행한 다음 잘못된 콘텐츠나 속성을 가진 확장 앱을 배포합니다.
    - 호스트가 잘못된 콘텐츠를 감지하고 올바르게 처리합니까?

## <a name="design-considerations"></a>디자인 고려 사항

- 확장을 사용할 수 있는 사용자를 표시하고 사용자가 활성화/비활성화할 수 있는 UI를 제공합니다. 패키지가 오프라인 상태가 되어 사용할 수 없게 되는 확장에 대해 문자 모양을 추가하는 것도 고려해 볼 수 있습니다.
- 확장자를 얻을 수 있는 위치로 사용자를 안내합니다. 확장 페이지는 앱과 함께 사용할 수 있는 확장 목록을 가져오는 Microsoft Store 검색 쿼리를 제공할 수 있습니다.
- 확장의 추가 및 제거를 사용자에게 알리는 방법을 고려하는 것이 좋습니다. 새 확장이 설치된 시기에 대한 알림을 생성하고 이를 활성화도록 사용자를 초대할 수 있습니다. 사용자가 제어할 수 있도록 확장을 기본적으로 비활성화해야 합니다.

## <a name="how-app-extensions-differ-from-optional-packages"></a>앱 확장이 선택적 패키지와 다른 점

[선택적 패키지](/windows/msix/package/optional-packages)와 앱 확장 사이의 주요 차이점은 오픈 에코시스템 및 폐쇄형 에코시스템의 차이, 종속 패키지 및 독립 패키지의 차이입니다.

앱 확장은 오픈 에코시스템에 참여합니다. 앱이 앱 확장을 호스팅할 수 있는 경우, 확장에서 정보를 전달/수신하는 방법을 준수하는 한 누구든지 호스트에 대한 확장을 작성할 수 있습니다. 이는 게시자가 앱과 함께 사용할 수 있는 선택적 패키지를 만들 수 있는 대상을 결정하는 폐쇄형 에코시스템에 참여하는 선택적 패키지와 다릅니다.

앱 확장은 독립적 패키지이며 독립 실행형 앱이 될 수 있습니다. 다른 앱에 대한 배포 종속성을 가질 수 없습니다. 선택적 패키지에는 기본 패키지가 필요 하며, 패키지 없이는 실행할 수 없습니다.

게임에 대한 확장 팩은 선택적 패키지의 좋은 후보가 될 수 있습니다. 이는 게임에 밀접하게 연결되어 있기 때문에 게임과 독립적으로 실행할 수 없으며 에코시스템의 개발자가 확장 팩을 만들지 못하게 해야 할 수 있습니다.

동일한 게임에 사용자 지정할 수 있는 UI 추가 기능 또는 테마가 있는 경우, 확장을 제공하는 앱이 자체적으로 실행될 수 있고 타사가 제작할 수 있기 때문에 앱 확장을 선택하는 것이 좋습니다.

## <a name="remarks"></a>설명

이 항목에서는 앱 확장에 대해 소개합니다. 주의해야 할 핵심 사항은 호스트를 만들고 이를 Package.appxmanifest 파일에 표시하고, 확장을 만들고 이를 Package.appxmanifest 파일에 표시하고, 확장 (예: 앱 서비스, 백그라운드 작업 또는 다른 수단)을 구현하는 방법을 결정하고, 호스트가 확장과 통신하는 방법을 정의하고, [AppExtensions API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)를 사용하여 확장에 액세스하고 관리하는 것입니다.

## <a name="related-topics"></a>관련 항목

* [앱 확장 소개](https://blogs.msdn.microsoft.com/appinstaller/2017/05/01/introduction-to-app-extensions/)
* [앱 확장에 대 한 빌드 2016 세션](https://channel9.msdn.com/Events/Build/2016/B808)
* [빌드 2016 앱 확장 코드 샘플](https://github.com/Microsoft/App-Extensibility-Sample)
* [백그라운드 작업을 사용 하 여 앱 지원](support-your-app-with-background-tasks.md)
* [앱 서비스를 만들고 사용하는 방법](how-to-create-and-consume-an-app-service.md).
* [AppExtensions 네임 스페이스](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)
* [서비스, 확장 및 패키지를 사용 하 여 앱 확장](https://docs.microsoft.com/windows/uwp/launch-resume/extend-your-app-with-services-extensions-packages)
