---
author: TylerMSFT
title: "앱 서비스가 공급자와 동일한 프로세스에서 실행되도록 변환"
description: "별도 백그라운드 프로세스에서 실행된 앱 서비스 코드를 앱 서비스 공급자와 동일한 프로세스 내에서 실행되는 코드로 변환합니다."
translationtype: Human Translation
ms.sourcegitcommit: 9e959a8ae6bf9496b658ddfae3abccf4716957a3
ms.openlocfilehash: 0990e9938bb9bf1794cf58c5541a64f22853b093

---

# 앱 서비스가 공급자와 동일한 프로세스에서 실행되도록 변환

[AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceconnection.aspx)을 사용하면 응용 프로그램이 백그라운드에 있는 앱의 절전 모드를 해제하고 직접 통신 회선을 시작할 수 있습니다.

단일 프로세스 앱 서비스가 도입되면서 실행 중인 두 포그라운드 응용 프로그램이 앱 서비스 연결을 통해 직접 통신 회선을 사용할 수 있습니다. 이제 앱 서비스가 포그라운드 응용 프로그램과 동일한 프로세스에서 실행될 수 있으므로 서비스 코드를 별도 프로젝트로 분리해야 하는 필요성이 제거되는 동시에 앱 간의 통신이 훨씬 더 용이해졌습니다.

다중 프로세스 모델 앱 서비스를 단일 프로세스 모델로 전환하려면 두 가지 변경이 필요합니다. 첫 번째는 매니페스트 변경입니다.

> ```xml
>  <uap:Extension Category="windows.appService">
>          <uap:AppService Name="SingleProcessAppService" />
>  </uap:Extension>
> ```

`EntryPoint` 특성을 제거합니다. 이제 앱 서비스를 호출할 때 [OnBackgroundActivated()](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) 콜백이 콜백 메서드로 사용됩니다.

두 번째 변경은 별도 백그라운드 작업 프로젝트의 서비스 논리를 **OnBackgroundActivated()**에서 호출할 수 있는 메서드로 이동하는 것입니다.

이제 응용 프로그램에서 앱 서비스를 직접 실행할 수 있습니다.  예를 들면 다음과 같습니다.

> ``` cs
> private AppServiceConnection appServiceconnection;
> private BackgroundTaskDeferral appServiceDeferral;
> protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
> {
>     base.OnBackgroundActivated(args);
>     IBackgroundTaskInstance taskInstance = args.TaskInstance;
>     AppServiceTriggerDetails appService = taskInstance.TriggerDetails as AppServiceTriggerDetails;
>     appServiceDeferral = taskInstance.GetDeferral();
>     taskInstance.Canceled += OnAppServicesCanceled;
>     appServiceConnection = appService.AppServiceConnection;
>     appServiceConnection.RequestReceived += OnAppServiceRequestReceived;
>     appServiceConnection.ServiceClosed += AppServiceConnection_ServiceClosed;
> }
>
> private async void OnAppServiceRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
> {
>     AppServiceDeferral messageDeferral = args.GetDeferral();
>     ValueSet message = args.Request.Message;
>     string text = message["Request"] as string;
>              
>     if ("Value" == text)
>     {
>         ValueSet returnMessage = new ValueSet();
>         returnMessage.Add("Response", "True");
>         await args.Request.SendResponseAsync(returnMessage);
>     }
>     messageDeferral.Complete();
> }
>
> private void OnAppServicesCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
> {
>     appServiceDeferral.Complete();
> }
>
> private void AppServiceConnection_ServiceClosed(AppServiceConnection sender, AppServiceClosedEventArgs args)
> {
>     appServiceDeferral.Complete();
> }
> ```

위의 코드에서 `OnBackgroundActivated` 메서드는 앱 서비스 활성화를 처리합니다. [AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceconnection.aspx)을 통한 통신에 필요한 모든 이벤트가 등록되고, 응용 프로그램 간의 통신이 완료되면 완료 상태로 표시될 수 있도록 작업 지연 개체가 저장됩니다.

앱이 요청을 받고 제공된 [ValueSet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset.aspx)를 읽어 `Key` 및 `Value` 문자열이 있는지 확인합니다. 문자열이 있으면 앱 서비스에서 `Response` 및 `True` 문자열 값 쌍을 **AppServiceConnection**의 반대쪽에 있는 앱에 다시 반환합니다.

다른 앱에 연결하고 통신하는 방법에 대한 자세한 내용은 [앱 서비스 만들기 및 사용](https://msdn.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service?f=255&MSPPError=-2147217396)을 참조하세요.



<!--HONumber=Aug16_HO3-->


