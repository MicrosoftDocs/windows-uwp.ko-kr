---
title: 앱 서비스가 호스트 앱과 동일한 프로세스에서 실행되도록 변환
description: 별도의 백그라운드 프로세스에서 실행 되는 app service 코드를 app service provider와 동일한 프로세스 내에서 실행 되는 코드로 변환 합니다.
ms.date: 11/03/2017
ms.topic: article
keywords: windows 10, uwp, app service
ms.assetid: 30aef94b-1b83-4897-a2f1-afbb4349696a
ms.localizationpriority: medium
ms.openlocfilehash: d1bdd8b72cafe3dd719f7ee1733e13e1531f8c7e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162717"
---
# <a name="convert-an-app-service-to-run-in-the-same-process-as-its-host-app"></a>앱 서비스가 호스트 앱과 동일한 프로세스에서 실행되도록 변환

[AppServiceConnection](/uwp/api/windows.applicationmodel.appservice.appserviceconnection) 를 사용 하면 다른 응용 프로그램에서 백그라운드에서 앱의 절전 모드를 해제 하 고 직접 통신을 시작할 수 있습니다.

In-process App Services의 도입으로, 실행 중인 두 포그라운드 응용 프로그램은 App service 연결을 통해 직접적인 통신 줄을 가질 수 있습니다. 이제 응용 프로그램 간의 통신을 훨씬 쉽게 하 고 서비스 코드를 별도의 프로젝트로 분리 하지 않아도 되도록 포그라운드 응용 프로그램과 동일한 프로세스에서 App Services를 실행할 수 있습니다.

In-process 모델 App Service를 in-process 모델로 설정 하려면 두 가지를 변경 해야 합니다. 첫 번째는 매니페스트 변경입니다.

> ```xml
> <Package
>    ...
>   <Applications>
>       <Application Id=...
>           ...
>           EntryPoint="...">
>           <Extensions>
>               <uap:Extension Category="windows.appService">
>                   <uap:AppService Name="InProcessAppService" />
>               </uap:Extension>
>           </Extensions>
>           ...
>       </Application>
>   </Applications>
> ```

`EntryPoint` `<Extension>` 지금은 [OnBackgroundActivated ()](/uwp/api/windows.ui.xaml.application.onbackgroundactivated) 가 app service를 호출할 때 사용 되는 진입점 이므로 요소에서 특성을 제거 합니다.

두 번째 변경 작업은 서비스 논리를 별도의 백그라운드 작업 프로젝트에서 **OnBackgroundActivated ()** 에서 호출할 수 있는 메서드로 이동 하는 것입니다.

이제 응용 프로그램에서 직접 App Service를 실행할 수 있습니다. 예를 들어 App.xaml.cs에서 다음을 수행 합니다.

[!NOTE] 아래 코드는 예 1 (out-of-process 서비스)에 제공 된 코드와 다릅니다. 아래 코드는 설명 목적 으로만 제공 되며 예제 2 (in-process 서비스)의 일부로 사용 하면 안 됩니다.  예제 1 (in-process 서비스)에서 예제 2 (in-process 서비스)로의 전환을 계속 하려면 아래 설명 코드 대신 예제 1에 제공 된 코드를 계속 사용 합니다.

``` cs
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
...

sealed partial class App : Application
{
  private AppServiceConnection _appServiceConnection;
  private BackgroundTaskDeferral _appServiceDeferral;

  ...

  protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args)
  {
      base.OnBackgroundActivated(args);
      IBackgroundTaskInstance taskInstance = args.TaskInstance;
      AppServiceTriggerDetails appService = taskInstance.TriggerDetails as AppServiceTriggerDetails;
      _appServiceDeferral = taskInstance.GetDeferral();
      taskInstance.Canceled += OnAppServicesCanceled;
      _appServiceConnection = appService.AppServiceConnection;
      _appServiceConnection.RequestReceived += OnAppServiceRequestReceived;
      _appServiceConnection.ServiceClosed += AppServiceConnection_ServiceClosed;
  }

  private async void OnAppServiceRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
  {
      AppServiceDeferral messageDeferral = args.GetDeferral();
      ValueSet message = args.Request.Message;
      string text = message["Request"] as string;

      if ("Value" == text)
      {
          ValueSet returnMessage = new ValueSet();
          returnMessage.Add("Response", "True");
          await args.Request.SendResponseAsync(returnMessage);
      }
      messageDeferral.Complete();
  }

  private void OnAppServicesCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
  {
      _appServiceDeferral.Complete();
  }

  private void AppServiceConnection_ServiceClosed(AppServiceConnection sender, AppServiceClosedEventArgs args)
  {
      _appServiceDeferral.Complete();
  }
}
```

위의 코드에서 메서드는 `OnBackgroundActivated` app service 활성화를 처리 합니다. [AppServiceConnection](/uwp/api/windows.applicationmodel.appservice.appserviceconnection) 를 통한 통신에 필요한 모든 이벤트는 등록 되며, 작업 지연 개체는 응용 프로그램 간의 통신이 완료 될 때 완료 된 것으로 표시 될 수 있도록 저장 됩니다.

앱이 요청을 받고 제공 된 [Valueset](/uwp/api/windows.foundation.collections.valueset) 을 읽어 `Key` 와 문자열이 있는지 확인 `Value` 합니다. 이러한 값이 있는 경우 app service는 `Response` `True` **AppServiceConnection**의 다른 쪽에 있는 앱으로 다시 및 문자열 값 쌍을 반환 합니다.

[App Service 만들기 및 사용](./how-to-create-and-consume-an-app-service.md?f=255&MSPPError=-2147217396)에서 다른 앱에 연결 하 고 통신 하는 방법에 대해 자세히 알아보세요.