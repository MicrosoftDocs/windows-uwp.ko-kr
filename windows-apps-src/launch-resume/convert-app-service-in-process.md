---
author: TylerMSFT
title: 앱 서비스가 호스트 앱과 동일한 프로세스에서 실행되도록 변환
description: 별도 백그라운드 프로세스에서 실행된 앱 서비스 코드를 앱 서비스 공급자와 동일한 프로세스 내에서 실행되는 코드로 변환합니다.
ms.author: twhitney
ms.date: 11/03/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 앱 서비스
ms.assetid: 30aef94b-1b83-4897-a2f1-afbb4349696a
ms.localizationpriority: medium
ms.openlocfilehash: a77ea3cefcc423e710ab0afebb3fa064e61507ec
ms.sourcegitcommit: 7aa1933e6970f878faf50d59e1f799b90afd7cc7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/05/2018
ms.locfileid: "3378639"
---
# <a name="convert-an-app-service-to-run-in-the-same-process-as-its-host-app"></a>앱 서비스가 호스트 앱과 동일한 프로세스에서 실행되도록 변환

[AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceconnection.aspx)을 사용하면 응용 프로그램이 백그라운드에 있는 앱의 절전 모드를 해제하고 직접 통신 회선을 시작할 수 있습니다.

In-process 앱 서비스가 도입되면서 실행 중인 두 포그라운드 응용 프로그램이 앱 서비스 연결을 통해 직접 통신 회선을 사용할 수 있습니다. 이제 앱 서비스가 포그라운드 응용 프로그램과 동일한 프로세스에서 실행될 수 있으므로 앱 간의 통신이 훨씬 더 용이해지고 서비스 코드를 별도 프로젝트로 분리하지 않아도 됩니다.

Out-of-process 모델 앱 서비스를 In-process 모델로 전환하려면 두 가지 변경이 필요합니다. 첫 번째는 매니페스트 변경입니다.

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

제거는 `EntryPoint` 에서 특성은 `<Extension>` 요소 이제 [onbackgroundactivated ()](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) 는 앱 서비스를 호출할 때 사용할 진입점 이므로 합니다.

두 번째 변경은 별도 백그라운드 작업 프로젝트의 서비스 논리를 **OnBackgroundActivated()** 에서 호출할 수 있는 메서드로 이동하는 것입니다.

이제 응용 프로그램에서 앱 서비스를 직접 실행할 수 있습니다. App.xaml.cs의 예를 들어:

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

위의 코드에서 `OnBackgroundActivated` 메서드는 앱 서비스 활성화를 처리합니다. [AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceconnection.aspx)을 통한 통신에 필요한 모든 이벤트가 등록되고, 응용 프로그램 간의 통신이 완료되면 완료 상태로 표시될 수 있도록 작업 지연 개체가 저장됩니다.

앱이 요청을 받고 제공된 [ValueSet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset.aspx)를 읽어 `Key` 및 `Value` 문자열이 있는지 확인합니다. 문자열이 있으면 앱 서비스에서 `Response` 및 `True` 문자열 값 쌍을 **AppServiceConnection**의 반대쪽에 있는 앱에 다시 반환합니다.

다른 앱에 연결하고 통신하는 방법에 대한 자세한 내용은 [앱 서비스 만들기 및 사용](https://msdn.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service?f=255&MSPPError=-2147217396)을 참조하세요.
