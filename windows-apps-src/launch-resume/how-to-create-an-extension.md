---
title: 앱 확장 만들기 및 호스팅
description: 사용자가 Microsoft Store에서 설치할 수 있는 패키지를 통해 앱을 확장할 수 있도록 하는 앱 확장을 작성 하 고 호스팅합니다.
keywords: 앱 확장, 앱 서비스, 배경
ms.date: 01/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 122c7c4d206c014d7d76cdab0b1b8fc66c0c9371
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158847"
---
# <a name="create-and-host-an-app-extension"></a>앱 확장 만들기 및 호스팅

이 문서에서는 Windows 10 앱 확장을 만들고 앱에서 호스트 하는 방법을 보여 줍니다. 앱 확장은 UWP 앱 및 패키지 된 [데스크톱 앱](/windows/apps/desktop/modernize/#msix-packages)에서 지원 됩니다.

앱 확장을 만드는 방법을 보여 주기 위해이 문서에서는 [수학 확장 코드 샘플](https://github.com/MicrosoftDocs/windows-topic-specific-samples/tree/MathExtensionSample)의 패키지 매니페스트 XML 및 코드 조각을 사용 합니다. 이 샘플은 UWP 앱 이지만 샘플에서 설명 하는 기능은 패키지 된 데스크톱 앱에도 적용 됩니다. 다음 지침에 따라 샘플을 시작 하세요.

- [수학 확장 코드 샘플](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/MathExtensionSample.zip)을 다운로드 하 고 압축을 풉니다.
- Visual Studio 2019에서 MathExtensionSample를 엽니다. 빌드 형식을 x86 (**빌드**  >  **Configuration Manager**로 설정한 다음 두 프로젝트에 대해 **플랫폼** 을 **x 86** 으로 변경 합니다).
- 솔루션 배포: **Build**  >  **솔루션 배포**를 빌드합니다.

## <a name="introduction-to-app-extensions"></a>앱 확장 소개

Windows 10에서 앱 확장은 다른 플랫폼에서 플러그 인, 추가 기능 및 추가 기능을 수행 하는 것과 비슷한 기능을 제공 합니다. 앱 확장은 Windows 10 기념일 버전 (버전 1607, 빌드 10.0.14393)에 도입 되었습니다.

앱 확장은 호스트 앱과 콘텐츠 및 배포 이벤트를 공유할 수 있도록 하는 확장 선언이 있는 UWP 앱 또는 패키지 된 데스크톱 앱입니다. 확장 앱은 여러 확장을 제공할 수 있습니다.

앱 확장은 UWP 앱 또는 패키지 된 데스크톱 응용 프로그램 이기 때문에 완전히 작동 하는 앱이 될 수 있으며, 호스트 확장이 가능 하 고, 다른 앱에 대 한 확장을 제공할 수도 있습니다. 즉, 별도의 앱 패키지를 만들지 않아도 됩니다.

앱 확장 호스트를 만들 때 다른 개발자가에 대 한 리소스를 기대 하지 않는 방식으로 앱을 향상 시킬 수 있는 응용 프로그램을 중심으로 에코 시스템을 개발할 수 있는 기회를 만듭니다. Microsoft Office 확장, Visual Studio 확장, 브라우저 확장 등을 고려 합니다. 이러한 앱은 제공 된 기능을 초과 하는 앱에 대해 더 다양 한 환경을 만듭니다. 확장은 응용 프로그램에 값 및 수명을 추가할 수 있습니다.

높은 수준에서 앱 확장 관계를 설정 하려면 다음을 수행 해야 합니다.

1. 앱을 확장 호스트로 선언 합니다.
2. 앱을 확장으로 선언 합니다.
3. 확장을 app service로 구현할지, 백그라운드 작업으로 구현할지 아니면 다른 방법으로 구현할지 결정 합니다.
4. 호스트 및 해당 확장이 통신 하는 방법을 정의 합니다.
5. 확장에 액세스 하려면 호스트 앱의 [Windows ApplicationModel. appextensions](/uwp/api/Windows.ApplicationModel.AppExtensions) API를 사용 합니다.

확장을 사용 하 여에 새 함수를 추가할 수 있는 가상 계산기를 구현 하는 [수학 확장 코드 샘플](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/MathExtensionSample.zip) 을 검사 하 여이 작업을 수행 하는 방법을 살펴보겠습니다. Microsoft Visual Studio 2019의 코드 샘플에서 **MathExtensionSample** 를 로드 합니다.

![수학 확장 코드 샘플](images/mathextensionhost-calctab.png)

## <a name="declare-an-app-to-be-an-extension-host"></a>앱을 확장 호스트로 선언

앱은 appxmanifest.xml 파일에 요소를 선언 하 여 자신을 앱 확장 호스트로 식별 `<AppExtensionHost>` 합니다. 이 작업을 수행 하는 방법을 보려면 **MathExtensionHost** 프로젝트의 **appxmanifest.xml** 파일을 참조 하세요.

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

및의가 `xmlns:uap3="http://..."` 있는지 확인 합니다 `uap3` `IgnorableNamespaces` . Uap3 네임 스페이스를 사용 하기 때문에 필요 합니다.

`<uap3:Extension Category="windows.appExtensionHost">` 이 앱을 확장 호스트로 식별 합니다.

의 **Name** 요소는 `<uap3:AppExtensionHost>` _확장 계약_ 이름입니다. 확장 프로그램에서 동일한 확장명 계약 이름을 지정 하면 호스트에서 해당 이름을 찾을 수 있습니다. 규칙에 따라 다른 확장 계약 이름과의 충돌을 방지 하기 위해 앱 또는 게시자 이름을 사용 하 여 확장 계약 이름을 작성 하는 것이 좋습니다.

동일한 앱에서 여러 호스트와 여러 확장을 정의할 수 있습니다. 이 예제에서는 하나의 호스트를 선언 합니다. 확장은 다른 앱에 정의 되어 있습니다.

## <a name="declare-an-app-to-be-an-extension"></a>앱을 확장으로 선언

앱은 `<uap3:AppExtension>` **appxmanifest.xml** 파일에서 요소를 선언 하 여 자신을 앱 확장으로 식별 합니다. **MathExtension** 프로젝트에서 **appxmanifest.xml** 파일을 열어이 작업을 수행 하는 방법을 확인 합니다.

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

다시, `xmlns:uap3="http://..."` 줄 및의 현재 상태를 확인 `uap3` `IgnorableNamespaces` 합니다. 이는 네임 스페이스를 사용 하기 때문에 필요 `uap3` 합니다.

`<uap3:Extension Category="windows.appExtension">` 이 앱을 확장으로 식별 합니다.

특성의 의미는 다음과 같습니다 `<uap3:AppExtension>` .

|특성|Description|필수|
|---------|-----------|:------:|
|**이름**|확장 계약 이름입니다. 호스트에 선언 된 **이름과** 일치 하면 해당 호스트는이 확장을 찾을 수 있습니다.| :heavy_check_mark: |
|**ID**| 이 확장을 고유 하 게 식별 합니다. 동일한 확장 계약 이름을 사용 하는 확장이 여러 개 있을 수 있으므로 (여러 확장을 지 원하는 그림판 앱을 가정) ID를 사용 하 여 서로 구분할 수 있습니다. 앱 확장 호스트는 ID를 사용 하 여 확장 형식에 대 한 정보를 유추할 수 있습니다. 예를 들어 데스크톱에 대해 디자인 된 확장 프로그램 하 나와 모바일의 경우 ID가 구분자로 사용 될 수 있습니다. 또한 아래에 설명 된 **속성** 요소를 사용할 수 있습니다.| :heavy_check_mark: |
|**표시 이름**| 호스트 앱에서 사용자에 대 한 확장을 식별 하는 데 사용할 수 있습니다. 이 클래스는 지역화를 위해 [새 리소스 관리 시스템](../app-resources/using-mrt-for-converted-desktop-apps-and-games.md) ()에서 쿼리할 수 있으며 사용할 수 있습니다 `ms-resource:TokenName` . 지역화 된 콘텐츠는 호스트 앱이 아닌 앱 확장 패키지에서 로드 됩니다. | |
|**설명** | 호스트 앱에서 사용자에 대 한 확장을 설명 하는 데 사용할 수 있습니다. 이 클래스는 지역화를 위해 [새 리소스 관리 시스템](../app-resources/using-mrt-for-converted-desktop-apps-and-games.md) ()에서 쿼리할 수 있으며 사용할 수 있습니다 `ms-resource:TokenName` . 지역화 된 콘텐츠는 호스트 앱이 아닌 앱 확장 패키지에서 로드 됩니다. | |
|**PublicFolder**|확장 호스트와 콘텐츠를 공유할 수 있는 패키지 루트를 기준으로 하는 폴더의 이름입니다. 규칙에 따라 이름은 "Public" 이지만 확장의 폴더와 일치 하는 모든 이름을 사용할 수 있습니다.| :heavy_check_mark: |

`<uap3:Properties>` 는 런타임에서 읽을 수 있는 사용자 지정 메타 데이터를 포함 하는 선택적 요소입니다. 코드 샘플에서는 확장이 app service로 구현 되므로 호스트에서 해당 app service의 이름을 가져올 수 있는 방법이 필요 합니다. App service의 이름이 정의 된 요소에 정의 되어 있습니다 <Service> (원하는 항목을 호출할 수 있음). 코드 샘플의 호스트는 런타임에이 속성을 검색 하 여 app service의 이름을 알아봅니다.

## <a name="decide-how-you-will-implement-the-extension"></a>확장을 구현 하는 방법을 결정 합니다.

[앱 확장에 대 한 빌드 2016 세션](https://channel9.msdn.com/Events/Build/2016/B808) 에서는 호스트와 확장 간에 공유 되는 공용 폴더를 사용 하는 방법을 보여 줍니다. 이 예제에서 확장은 호스트가 호출 하는 공용 폴더에 저장 된 Javascript 파일에 의해 구현 됩니다. 이러한 접근 방식은 lightweight이 고 컴파일이 필요 하지 않으며, 확장에 대 한 지침을 제공 하는 기본 방문 페이지와 호스트 앱의 Microsoft Store 페이지에 대 한 링크를 제공 하도록 지원할 수 있습니다. 자세한 내용은 [빌드 2016 앱 확장 코드 샘플](https://github.com/Microsoft/App-Extensibility-Sample) 을 참조 하세요. 특히 ExtensibilitySample 프로젝트에서 **InvertImageExtension** 프로젝트 및 `InvokeLoad()` ExtensionManager.cs를 참조 하세요 **ExtensibilitySample** .

이 예제에서는 app service를 사용 하 여 확장을 구현 합니다. App services의 이점은 다음과 같습니다.

- 확장 프로그램이 충돌 하는 경우 호스트 앱이 자체 프로세스에서 실행 되므로 호스트 앱이 작동 중단 되지 않습니다.
- 선택한 언어를 사용 하 여 서비스를 구현할 수 있습니다. 호스트 앱을 구현 하는 데 사용 되는 언어와 일치 하지 않아도 됩니다.
- App service는 자체 앱 컨테이너에 액세스할 수 있습니다 .이는 호스트의 기능과 다를 수 있습니다.
- 서비스와 호스트 응용 프로그램의 데이터 간에 격리가 있습니다.

### <a name="host-app-service-code"></a>호스트 app service 코드

확장의 app service를 호출 하는 호스트 코드는 다음과 같습니다.

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

이는 app service를 호출 하는 일반적인 코드입니다. 앱 서비스를 구현 하 고 호출 하는 방법에 대 한 자세한 내용은 [app service를 만들고 사용 하는 방법](how-to-create-and-consume-an-app-service.md)을 참조 하세요.

한 가지 주의할 점은 호출 하는 app service의 이름을 결정 하는 방법입니다. 호스트에 확장의 구현에 대 한 정보가 없기 때문에 확장에서 해당 app service의 이름을 제공 해야 합니다. 코드 샘플에서 확장은 요소에 있는 해당 파일의 app service 이름을 선언 합니다 `<uap3:Properties>` .

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

요소에 사용자 고유의 XML을 정의할 수 있습니다 `<uap3:Properties>` . 이 경우 호스트에서 확장을 호출할 때 사용할 수 있도록 app service의 이름을 정의 합니다.

호스트에서 확장을 로드 하면 다음과 같은 코드가 확장의 appxmanifest.xml에 정의 된 속성에서 서비스 이름을 추출 합니다.

_`Update()` ExtensionManager.cs의 MathExtensionHost 프로젝트에서_
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

호스트는에 저장 된 app service의 이름을 `_serviceName` 사용 하 여 app service를 호출할 수 있습니다.

App service를 호출 하려면 app service를 포함 하는 패키지의 제품군 이름도 필요 합니다. 다행히 앱 확장 API는 줄에서 얻을 수 있는이 정보를 제공 합니다. `connection.PackageFamilyName = AppExtension.Package.Id.FamilyName;`

### <a name="define-how-the-host-and-the-extension-will-communicate"></a>호스트 및 확장이 통신 하는 방법 정의

App services에서는 [Valueset](/uwp/api/windows.foundation.collections.valueset) 을 사용 하 여 정보를 교환 합니다. 호스트의 작성자는 유연 하 게 확장 된 확장과 통신 하기 위한 프로토콜을 사용 해야 합니다. 코드 샘플에서는 앞으로 1 개, 2 개 또는 더 많은 인수를 사용할 수 있는 확장에 대 한 회계를 의미 합니다.

이 예제에서 인수에 대 한 프로토콜은 ' Arg ' 라는 키 값 쌍을 포함 하는 **Valueset** 및와 같이 인수 번호입니다 `Arg1` `Arg2` . 호스트는 **Valueset**의 모든 인수를 전달 하 고 확장은 필요한 인수를 사용 합니다. 확장에서 결과를 계산할 수 있으면 호스트는 확장에서 반환 된 **Valueset** 에 `Result` 계산 값을 포함 하는 라는 키가 있어야 합니다. 해당 키가 없는 경우 호스트는 확장에서 계산을 완료할 수 없다고 가정 합니다.

### <a name="extension-app-service-code"></a>확장 app service 코드

코드 샘플에서 확장의 app service는 백그라운드 작업으로 구현 되지 않습니다. 대신, app service가 해당 서비스를 호스팅하는 확장 앱과 동일한 프로세스에서 실행 되는 단일 proc app service 모델을 사용 합니다. 이는 여전히 호스트 앱과 다른 프로세스 이며, 프로세스 분리의 이점을 제공 하는 반면, 확장 프로세스와 app service를 구현 하는 백그라운드 프로세스 간의 프로세스 간 통신을 방지 하 여 성능상의 이점을 얻을 수 있습니다. 백그라운드 작업으로 실행 되는 app service와 동일한 프로세스에서 실행 되는 app service 간의 차이점을 확인 하려면 [호스트 앱과 동일한 프로세스에서 실행 되도록 app Service 변환](convert-app-service-in-process.md) 을 참조 하세요.

`OnBackgroundActivate()`앱 서비스가 활성화 되 면 시스템에서를 수행 합니다. 이 코드는 이벤트 처리기가 제공 될 때 () 실제 app service 호출을 처리 하 고 `OnAppServiceRequestReceived()` 취소 또는 종결 이벤트를 처리 하는 지연 개체를 가져오는 등의 정리 이벤트를 처리 하도록 이벤트 처리기를 설정 합니다.

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

확장의 작업을 수행 하는 코드는에 `OnAppServiceRequestReceived()` 있습니다. 이 함수는 계산을 수행 하기 위해 app service를 호출할 때 호출 됩니다. **Valueset**에서 필요한 값을 추출 합니다. 계산을 수행할 수 있는 경우 결과를 호스트로 반환 된 **Valueset** 의 키 **결과**에 배치 합니다. 이 호스트와 해당 확장이 통신 하는 방법에 대해 정의 된 프로토콜에 따라 **결과** 키가 있으면 성공이 표시 됩니다. 그렇지 않으면 실패 합니다.

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

이제 호스트와 해당 확장 간의 관계를 구현 하는 방법을 살펴보았으므로 호스트에서 시스템에 설치 된 확장을 찾고 확장을 포함 하는 패키지의 추가 및 제거에 반응 하는 방법을 알아보겠습니다.

Microsoft Store는 확장을 패키지로 제공 합니다. [Appextensioncatalog](/uwp/api/windows.applicationmodel.appextensions.appextensioncatalog) 는 호스트의 확장 계약 이름과 일치 하는 확장이 포함 된 설치 된 패키지를 찾고 해당 호스트와 관련 된 앱 확장 패키지를 설치 하거나 제거할 때 발생 하는 이벤트를 제공 합니다.

코드 샘플에서 `ExtensionManager` 클래스 ( **MathExtensionHost** 프로젝트의 **ExtensionManager.cs** 에 정의 됨)는 확장을 로드 하 고 확장 패키지 설치 및 제거에 대 한 응답을 위한 논리를 래핑합니다.

`ExtensionManager`생성자는를 사용 하 여 `AppExtensionCatalog` 호스트와 동일한 확장명 계약 이름을 가진 시스템에서 앱 확장을 찾습니다.

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

확장 패키지가 설치 되 면는 `ExtensionManager` 호스트와 동일한 확장 계약 이름을 가진 패키지의 확장에 대 한 정보를 수집 합니다. 설치는 영향을 받는 확장 정보가 업데이트 되는 경우 업데이트를 나타낼 수 있습니다. 확장 패키지가 제거 되 면에서 영향을 `ExtensionManager` 받는 확장 프로그램에 대 한 정보를 제거 하 여 더 이상 사용할 수 없는 확장을 사용자에 게 알려 줍니다.

`Extension`코드 샘플은 사용자가 확장을 사용 하도록 설정 했는지 여부와 같은 확장 ID, 설명, 로고 및 앱 관련 정보에 액세스 하는 클래스 ( **MathExtensionHost** 프로젝트의 **ExtensionManager.cs** 에 정의 됨)를 만들었습니다.

확장이 로드 됨 ( `Load()` **ExtensionManager.cs**의 참조)은 패키지 상태가 양호 하 고 ID, 로고, 설명 및 공용 폴더 (이 샘플에서는 사용 하지 않음)를 획득 했음을 의미 합니다. 확장 패키지 자체가 로드 되 고 있지 않습니다.

언로드의 개념은 더 이상 사용자에 게 표시 되지 않아야 하는 확장을 추적 하는 데 사용 됩니다.

는 `ExtensionManager` `Extension` 확장, 이름, 설명 및 로고를 UI에 바인딩할 수 있도록 컬렉션 인스턴스를 제공 합니다. **ExtensionsTab** 페이지는이 컬렉션에 바인딩되고 확장을 활성화/비활성화 하 고 제거 하는 UI를 제공 합니다.

![확장 탭 예제 UI](images/mathextensionhost-extensiontab.png)

 확장이 제거 되 면 시스템은 확장을 포함 하는 패키지를 제거할지 확인 하는 메시지를 표시 합니다 .이 경우 다른 확장이 포함 될 수도 있습니다. 사용자가 동의한 경우 패키지가 제거 되 고가 `ExtensionManager` 호스트 앱에서 사용할 수 있는 확장 목록에서 제거 된 패키지의 확장을 제거 합니다.

 ![UI 제거](images/mathextensionhost-uninstall.png)

## <a name="debugging-app-extensions-and-hosts"></a>앱 확장 및 호스트 디버깅

확장 호스트 및 확장이 동일한 솔루션에 포함 되지 않는 경우가 종종 있습니다. 이 경우 호스트 및 확장을 디버깅 하려면 다음을 수행 합니다.

1. Visual Studio의 한 인스턴스에서 호스트 프로젝트를 로드 합니다.
2. Visual Studio의 다른 인스턴스에서 확장을 로드 합니다.
3. 디버거에서 호스트 앱을 시작 합니다.
4. 디버거에서 확장 앱을 시작 합니다. (디버그 하는 대신 확장을 배포 하 여 호스트의 패키지 설치 이벤트를 테스트 하려면 대신 ** &gt; 솔루션 배포 빌드**를 수행 합니다.)

이제 호스트 및 확장의 중단점에 도달할 수 있습니다.
확장 앱 자체의 디버깅을 시작 하면 앱에 대 한 빈 창이 표시 됩니다. 빈 창을 표시 하지 않으려면 확장 프로젝트에 대 한 디버깅 설정을 변경 하 여 앱이 시작 되지 않도록 하 고, 앱이 시작 될 때 디버그 설정을 디버그할 수 있습니다 (확장 프로젝트를 마우스 오른쪽 단추로 클릭). **속성**  >  **디버그** > 시작 하지 않음 ( **시작 시 코드 디버그**)을 선택 합니다. 확장 프로젝트를 디버깅 (**F5**) 해야 하지만, 호스트에서 확장을 활성화할 때까지 대기한 다음 확장의 중단점이 적중 됩니다.

**코드 샘플 디버그**

코드 샘플에서 호스트와 확장은 동일한 솔루션에 있습니다. 디버깅 하려면 다음을 수행 합니다.

1. **MathExtensionHost** 이 시작 프로젝트 인지 확인 합니다. **MathExtensionHost** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **시작 프로젝트로 설정**을 클릭 합니다.
2. `Invoke` **MathExtensionHost** 프로젝트에서 ExtensionManager.cs의에 중단점을 배치 합니다.
3. **F5 키** 를 눌러 **MathExtensionHost** 프로젝트를 실행 합니다.
4. `OnAppServiceRequestReceived` **MathExtension** 프로젝트에서 App.xaml.cs의에 중단점을 배치 합니다.
5. **MathExtension** 프로젝트 디버깅을 시작 합니다. ( **MathExtension** 프로젝트를 마우스 오른쪽 단추로 클릭 하 **> 새 인스턴스를 시작**합니다.) 패키지를 배포 하 고 호스트에서 패키지 설치 이벤트를 트리거합니다.
6. **MathExtensionHost** 앱에서 **계산** 페이지로 이동 하 고 **x ^ y** 를 클릭 하 여 확장을 활성화 합니다. `Invoke()`중단점이 먼저 적중 되 고, 수행 되는 확장 app service 호출을 볼 수 있습니다. 그런 다음 `OnAppServiceRequestReceived()` 확장의 메서드가 적중 되 면 app service가 결과를 계산 하 고 반환 하는 것을 볼 수 있습니다.

**App service로 구현 된 확장 문제 해결**

확장 호스트에서 확장을 위해 app service에 연결 하는 데 문제가 있는 경우 `<uap:AppService Name="...">` 특성이 요소에 입력 한 내용과 일치 하는지 확인 `<Service>` 합니다. 일치 하지 않는 경우 확장에서 제공 하는 서비스 이름이 구현한 app service 이름과 일치 하지 않으며 호스트는 확장을 활성화할 수 없습니다.

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

## <a name="a-checklist-of-basic-scenarios-to-test"></a>테스트할 기본 시나리오의 검사 목록

확장 호스트를 빌드하고 확장을 지 원하는 방법을 테스트할 준비가 되 면 다음을 시도해 볼 수 있는 몇 가지 기본적인 시나리오는 다음과 같습니다.

- 호스트를 실행 한 다음 확장 앱을 배포 합니다.  
    - 호스트에서 실행 되는 동안 새 확장을 선택 하나요?  
- 확장 앱을 배포 하 고 호스트를 배포 및 실행 합니다.
    - 호스트가 이전에 기존 확장을 선택 하나요?  
- 호스트를 실행 한 다음 확장 앱을 제거 합니다.
    - 호스트가 제거를 올바르게 감지 합니까?
- 호스트를 실행 하 고 확장 앱을 최신 버전으로 업데이트 합니다.
    - 호스트가 변경을 선택 하 고 이전 버전의 확장을 제대로 언로드하고 싶으세요?  

**테스트할 고급 시나리오:**

- 호스트를 실행 하 고 확장 앱을 이동식 미디어로 이동 하 여 미디어를 제거 합니다.
    - 호스트에서 패키지 상태의 변경 내용을 감지 하 고 확장을 사용 하지 않도록 설정 하 시겠습니까?
- 호스트를 실행 한 다음 확장 앱을 손상 시키는 경우 (유효 하지 않음, 다른 방식으로 서명 됨 등)
    - 호스트가 변조 된 확장을 검색 하 고 올바르게 처리 하나요?
- 호스트를 실행 하 고 잘못 된 콘텐츠나 속성이 있는 확장 앱을 배포 합니다.
    - 호스트에서 잘못 된 콘텐츠를 검색 하 여 올바르게 처리 하나요?

## <a name="design-considerations"></a>설계 고려 사항

- 사용할 수 있는 확장 프로그램을 표시 하 고 사용 하거나 사용 하지 않도록 설정할 수 있는 UI를 제공 합니다. 패키지를 오프 라인으로 전환 하는 등의 방법으로 사용할 수 없게 되는 확장에 대 한 문자 모양을 추가할 수도 있습니다.
- 사용자에 게 확장을 가져올 수 있는 위치를 전달 합니다. 확장 페이지에서 앱과 함께 사용할 수 있는 확장 목록을 표시 하는 Microsoft Store 검색 쿼리를 제공할 수 있습니다.
- 확장의 추가 및 제거를 사용자에 게 알리는 방법을 고려 합니다. 새 확장이 설치 된 경우에 대 한 알림을 만들고 사용자를 초대 하 여 사용 하도록 설정할 수 있습니다. 사용자가 제어할 수 있도록 확장은 기본적으로 사용 하지 않도록 설정 해야 합니다.

## <a name="how-app-extensions-differ-from-optional-packages"></a>앱 확장이 선택적 패키지와 어떻게 다른 지

[선택적 패키지](/windows/msix/package/optional-packages) 와 앱 확장 간의 주요 차이점은 열려 있는 에코 시스템 및 닫힌 에코 시스템 및 종속 패키지와 독립 패키지입니다.

앱 확장은 오픈 에코 시스템에 참여 합니다. 앱이 앱 확장을 호스팅할 수 있는 경우, 모든 사용자가 확장에서 정보를 전달/수신 하는 메서드를 준수 하는 한 호스트에 대 한 확장을 작성할 수 있습니다. 이는 게시자가 앱과 함께 사용할 수 있는 선택적 패키지를 만들 수 있는 사람을 결정 하는 닫힌 에코 시스템에 참여 하는 선택적 패키지와는 다릅니다.

앱 확장은 독립 패키지 이며 독립 실행형 앱 일 수 있습니다. 다른 앱에 대 한 배포 종속성을 가질 수 없습니다.선택적 패키지에는 기본 패키지가 필요 하며, 패키지 없이는 실행할 수 없습니다.

게임에 대 한 확장 팩은 게임에 긴밀 하 게 연결 되어 있으며, 게임과 독립적으로 실행할 수 없고, 에코 시스템의 모든 개발자가 확장 팩을 만드는 것을 원하지 않을 수 있으므로 선택적 패키지에 적합 합니다.

동일한 게임에서 사용자 지정 가능한 UI 추가 기능 또는 테마를 사용 하는 경우 확장을 제공 하는 앱이 자체적으로 실행 될 수 있고 타사에서 해당 앱을 만들 수 있기 때문에 앱 확장을 선택 하는 것이 좋습니다.

## <a name="remarks"></a>설명

이 항목에서는 앱 확장에 대해 소개 합니다. 기억해 야 할 주요 사항은 호스트를 만들고 해당 패키지에 표시 하는 것입니다. appxmanifest.xml 파일을 사용 하 여 확장을 만들고 appxmanifest.xml 파일에 표시 하 여 확장을 구현 하는 방법 (예: app service, 백그라운드 작업 또는 다른 방법)을 결정 하 고, 호스트가 확장과 통신 하는 방법을 정의 하 고, [APPEXTENSIONS API](/uwp/api/windows.applicationmodel.appextensions) 를 사용 하 여 확장에 액세스 하 고 관리 합니다.

## <a name="related-topics"></a>관련 항목

* [앱 확장 소개](/windows/msix/)
* [앱 확장에 대 한 빌드 2016 세션](https://channel9.msdn.com/Events/Build/2016/B808)
* [빌드 2016 앱 확장 코드 샘플](https://github.com/Microsoft/App-Extensibility-Sample)
* [백그라운드 작업을 사용하여 앱 지원](support-your-app-with-background-tasks.md)
* [App service를 만들고 사용 하는 방법](how-to-create-and-consume-an-app-service.md)
* [AppExtensions 네임 스페이스](/uwp/api/windows.applicationmodel.appextensions)
* [서비스, 확장 및 패키지로 앱 확장](./extend-your-app-with-services-extensions-packages.md)