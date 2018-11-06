---
author: mijacobs
Description: By using a wizard in Visual Studio, you can generate push notifications from a mobile service that was created with Azure Mobile Services.
title: 푸시 알림 마법사에서 생성된 코드
ms.assetid: 340F55C1-0DDF-4233-A8E4-C15EF9030785
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10 uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6b39211c4b21a68fc0e563f73805805dcf1f4641
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2018
ms.locfileid: "6051623"
---
# <a name="code-generated-by-the-push-notification-wizard"></a>푸시 알림 마법사에서 생성된 코드
 

Visual Studio의 마법사를 사용하여 Azure Mobile Services와 함께 만든 모바일 서비스에서 푸시 알림을 생성할 수 있습니다. Visual Studio 마법사는 시작하는 데 유용한 코드를 생성합니다. 이 항목에서는 마법사에서 프로젝트를 수정하는 방법, 생성된 코드가 수행하는 작업, 이 코드를 사용하는 방법 및 푸시 알림을 최대한 활용하기 위해 다음에 수행할 수 있는 작업에 대해 설명합니다. [WNS(Windows 푸시 알림 서비스) 개요](windows-push-notification-services--wns--overview.md)를 참조하세요.

## <a name="how-the-wizard-modifies-your-project"></a>마법사에서 프로젝트를 수정하는 방법


푸시 알림 마법사는 다음과 같은 방법으로 프로젝트를 수정합니다.

-   모바일 서비스 관리 클라이언트(MobileServicesManagedClient.dll)에 대한 참조를 추가합니다. JavaScript 프로젝트에는 적용되지 않습니다.
-   서비스 아래의 하위 폴더에 파일을 추가하고 파일 이름을 push.register.cs, push.register.vb, push.register.cpp 또는 push.register.js로 지정합니다.
-   모바일 서비스의 데이터베이스 서버에 채널 테이블을 만듭니다. 이 테이블에는 푸시 알림을 앱 인스턴스로 보내는 데 필요한 정보가 들어 있습니다.
-   삭제, 삽입, 읽기 및 업데이트의 네 가지 기능에 대한 스크립트를 만듭니다.
-   사용자 지정 API를 사용하여 모든 클라이언트에 푸시 알림을 보내는 notifyallusers.js 스크립트를 만듭니다.
-   App.xaml.cs, App.xaml.vb 또는 App.xaml.cpp 파일에 선언을 추가하거나 JavaScript 프로젝트에 대한 새 파일 service.js에 선언을 추가합니다. 이 선언에서는 모바일 서비스에 연결하는 데 필요한 정보가 들어 있는 MobileServiceClient 개체를 선언합니다. App.*MyServiceName*Client라는 이름을 사용하여 앱의 임의 페이지에서 이름이 *MyServiceName*Client인 이 MobileServiceClient 개체에 액세스할 수 있습니다.

services.js 파일에는 다음 코드가 포함되어 있습니다.

```js
var <mobile-service-name>Client = new Microsoft.WindowsAzure.MobileServices.MobileServiceClient(
                "https://<mobile-service-name>.azure-mobile.net/",
                "<your client secret>");
```

## <a name="registration-for-push-notifications"></a>푸시 알림 등록


push.register.\*에서 UploadChannel 메서드는 푸시 알림을 수신할 디바이스를 등록합니다. 스토어는 설치된 앱 인스턴스를 추적하고 푸시 알림 채널을 제공합니다. [**PushNotificationChannelManager**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager)을 참조하세요.

클라이언트 코드는 JavaScript 백 엔드와 .NET 백 엔드 모두 비슷합니다. 기본적으로 JavaScript 백 엔드 서비스에 대해 푸시 알림을 추가하면 notifyAllUsers 사용자 지정 API에 대한 샘플 호출이 UploadChannel 메서드에 삽입됩니다.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using Microsoft.WindowsAzure.MobileServices;
using Newtonsoft.Json.Linq;

namespace App2
{
    internal class mymobileservice1234Push
    {
        public async static void UploadChannel()
        {
            var channel = await Windows.Networking.PushNotifications.PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            try
            {
                await App.mymobileservice1234Client.GetPush().RegisterNativeAsync(channel.Uri);
                await App.mymobileservice1234Client.InvokeApiAsync("notifyAllUsers");
            }
            catch (Exception exception)
            {
                HandleRegisterException(exception);
            }
        }

        private static void HandleRegisterException(Exception exception)
        {
            
        }
    }
}
```

```vb
Imports Microsoft.WindowsAzure.MobileServices
Imports Newtonsoft.Json.Linq

Friend Class mymobileservice1234Push
    Public Shared Async Sub UploadChannel()
        Dim channel = Await Windows.Networking.PushNotifications.PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync()

        Try
            Await App.mymobileservice1234Client.GetPush().RegisterNativeAsync(channel.Uri)
            Await App.mymobileservice1234Client.GetPush().RegisterNativeAsync(channel.Uri, New String() {"tag1", "tag2"})
            Await App.mymobileservice1234Client.InvokeApiAsync("notifyAllUsers")
        Catch exception As Exception
            HandleRegisterException(exception)
        End Try
    End Sub

    Private Shared Sub HandleRegisterException(exception As Exception)

    End Sub
End Class
```

```c++
#include "pch.h"
#include "services\mobile services\mymobileservice1234\mymobileservice1234Push.h"

using namespace AzureMobileHelper;

using namespace web;
using namespace concurrency;

using namespace Windows::Networking::PushNotifications;

void mymobileservice1234Push::UploadChannel()
{
    create_task(PushNotificationChannelManager::CreatePushNotificationChannelForApplicationAsync()).
    then([] (PushNotificationChannel^ newChannel) 
    {
        return mymobileservice1234MobileService::GetClient().get_push().register_native(newChannel->Uri->Data());
    }).then([]()
    {
        return mymobileservice1234MobileService::GetClient().invoke_api(L"notifyAllUsers");
    }).then([](task<json::value> result)
    {
        try
        {
            result.wait();
        }
        catch(...)
        {
            HandleExceptionsComingFromTheServer();
        }
    });
}

void mymobileservice1234Push::HandleExceptionsComingFromTheServer()
{
}
```

```js
(function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;

    app.addEventListener("activated", function (args) {
        if (args.detail.kind == activation.ActivationKind.launch) {
            Windows.Networking.PushNotifications.PushNotificationChannelManager.createPushNotificationChannelForApplicationAsync()
                .then(function (channel) {
                    mymobileserviceclient1234Client.push.registerNative(channel.Uri, new Array("tag1", "tag2"))
                    return mymobileservice1234Client.push.registerNative(channel.uri);
                })
                .done(function (registration) {
                    return mymobileservice1234Client.invokeApi("notifyAllUsers");
                }, function (error) {
                    // Error

                });
        }
    });
})();
```

푸시 알림 태그를 사용하여 알림을 클라이언트의 하위 집합으로 제한할 수 있습니다. registerNative 메서드 또는 RegisterNativeAsync 메서드에 대해 태그를 지정하지 않고 모든 푸시 알림을 등록하거나, 두 번째 인수인 태그 배열을 제공하여 태그와 함께 등록할 수 있습니다. 하나 이상의 태그와 함께 등록할 경우 해당 태그와 일치하는 알림만 수신됩니다.

## <a name="server-side-scripts-javascript-backend-only"></a>서버 쪽 스크립트(JavaScript 백 엔드에만 해당)


JavaScript 백 엔드를 사용하는 모바일 서비스의 경우 삭제, 삽입, 읽기 또는 업데이트 작업을 수행하면 서버 쪽 스크립트가 실행됩니다. 스크립트에서 이러한 작업을 구현하지는 않지만 클라이언트의 Windows Mobile REST API 호출에서 이러한 이벤트를 트리거할 때 실행됩니다. 그런 다음 스크립트는 request.execute 또는 request.respond 호출을 통해 호출 컨텍스트에 대한 응답을 실행하여 직접 제어를 작업에 전달합니다. [Azure Mobile Services REST API 참조](http://go.microsoft.com/fwlink/p/?linkid=511139)를 참조하세요.

서버 쪽 스크립트에서는 다양한 함수를 사용할 수 있습니다. [Azure 모바일 서비스 테이블 작업 등록](http://go.microsoft.com/fwlink/p/?linkid=511140)을 참조하세요. 사용 가능한 모든 함수에 대한 참조를 보려면 [Mobile Services 서버 스크립트 참조](http://go.microsoft.com/fwlink/p/?linkid=257676)를 참조하세요.

Notifyallusers.js의 다음 사용자 지정 API 코드도 만들어집니다.

```js
exports.post = function(request, response) {
    response.send(statusCodes.OK,{ message : 'Hello World!' })
    
    // The following call is for illustration purpose only
    // The call and function body should be moved to a script in your app
    // where you want to send a notification
    sendNotifications(request);
};

// The following code should be moved to appropriate script in your app where notification is sent
function sendNotifications(request) {
    var payload = '<?xml version="1.0" encoding="utf-8"?><toast><visual><binding template="ToastText01">' +
        '<text id="1">Sample Toast</text></binding></visual></toast>';
    var push = request.service.push; 
    push.wns.send(null,
        payload,
        'wns/toast', {
            success: function (pushResponse) {
                console.log("Sent push:", pushResponse);
            }
        });
}
```

SendNotifications 함수는 단일 알림을 알림 메시지로 보냅니다. 다른 형식의 푸시 알림을 사용할 수도 있습니다.

**팁**스크립트를 편집 하는 동안 도움말을 가져오는 방법에 대 한 자세한 내용은 [서버 쪽 JavaScript에 IntelliSense 사용](http://go.microsoft.com/fwlink/p/?LinkId=309275)을 참조 하세요.

 

## <a name="push-notification-types"></a>푸시 알림 형식


Windows에서는 푸시 알림이 아닌 알림을 지원합니다. 알림에 대한 일반적인 내용은 [알림 전달 방법 선택](choosing-a-notification-delivery-method.md)을 참조하세요.

알림 메시지는 사용이 간단하며, 생성된 채널 테이블에 대한 Insert.js 코드에서 예제를 검토할 수 있습니다. 타일 또는 배지 알림을 사용하려는 경우 타일 및 배지에 대한 XML 템플릿을 만들고 템플릿에 패키지된 정보의 인코딩을 지정해야 합니다. [타일, 배지 및 알림 메시지 작업](https://msdn.microsoft.com/library/windows/apps/xaml/hh868259)을 참조하세요.

Windows는 푸시 알림에 응답하기 때문에 앱이 실행되지 않을 때 이러한 알림을 대부분 처리할 수 있습니다. 예를 들어 푸시 알림을 통해 사용자는 로컬 메일 앱이 실행되지 않을 때도 새 메일 메시지가 도착한 것을 알 수 있습니다. Windows는 텍스트 메시지의 첫째 줄과 같은 메시지를 표시하여 알림 메시지를 처리합니다. 또한 새 메일 메시지 수가 반영되도록 앱의 라이브 타일을 업데이트하여 타일 또는 배지 알림을 처리합니다. 이런 방식으로 앱의 사용자에게 새 정보를 확인하라는 메시지를 표시할 수 있습니다. 앱이 실행되고 있으면 원시 알림을 받을 수 있으며, 원시 알림을 사용하여 데이터를 앱에 보낼 수 있습니다. 앱이 실행되고 있지 않으면 푸시 알림을 모니터링하도록 백그라운드 작업을 설정할 수 있습니다.

이러한 알림은 사용자 리소스를 사용하며, 과도하게 사용할 경우 방해가 될 수 있으므로 UWP(유니버설 Windows 플랫폼) 앱에 대한 지침에 따라 푸시 알림을 사용해야 합니다. [푸시 알림에 대한 지침 및 검사 목록](https://msdn.microsoft.com/library/windows/apps/hh761462)을 참조하세요.

푸시 알림으로 라이브 타일을 업데이트하는 경우 [타일 및 배지에 대한 지침 및 검사 목록](https://msdn.microsoft.com/library/windows/apps/hh465403)도 따라야 합니다.

## <a name="next-steps"></a>다음 단계


### <a name="using-the-windows-push-notification-services-wns"></a>WNS(Windows 푸시 알림 서비스) 사용

모바일 서비스에서 충분한 유연성을 제공하지 않는 경우, C# 또는 Visual Basic으로 서버 코드를 작성하려는 경우 또는 클라우드 서비스가 이미 있으며 해당 서비스에서 푸시 알림을 보내려는 경우 WNS(Windows 푸시 알림 서비스)를 직접 호출할 수 있습니다. WNS를 직접 호출하면 데이터베이스 또는 다른 웹 서비스의 데이터를 모니터링하는 작업자 역할과 같은 고유한 클라우드 서비스에서 푸시 알림을 보낼 수 있습니다. 클라우드 서비스에서 푸시 알림을 앱에 보내려면 WNS에 인증해야 합니다. [Windows 푸시 알림 서비스에 인증하는 방법(JavaScript)](https://msdn.microsoft.com/library/windows/apps/hh465407) 또는 [(C#/C++/VB)](https://msdn.microsoft.com/library/windows/apps/xaml/hh868206)을 참조하세요.

모바일 서비스에서 예약된 작업을 실행하여 푸시 알림을 보낼 수도 있습니다. [모바일 서비스에서 되풀이 작업 예약](http://go.microsoft.com/fwlink/p/?linkid=301694)을 참조하세요.

**경고**푸시 알림 마법사를 한 번 실행 한 후 다시 실행 하지 마세요 마법사는 다른 모바일 서비스에 대 한 등록 코드를 추가 합니다. 프로젝트당 마법사를 두 번 이상 실행하면 [**CreatePushNotificationChannelForApplicationAsync**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync) 메서드에 대한 호출이 겹치게 되어 런타임 예외를 발생하는 코드가 생성됩니다. 두 개 이상의 모바일 서비스에 대해 푸시 알림을 등록하려면 마법사를 한 번 실행한 다음 **CreatePushNotificationChannelForApplicationAsync**에 대한 호출이 동시에 실행되지 않도록 등록 코드를 다시 작성합니다. 예를 들어 **CreatePushNotificationChannelForApplicationAsync**에 대한 호출을 포함하여 push.register.\*의 마법사 생성 코드를 OnLaunched 이벤트 외부로 이동하면 이 작업을 완료할 수 있지만 이 작업의 특징은 앱의 아키텍처에 따라 다릅니다.

 

## <a name="related-topics"></a>관련 항목


* [WNS(Windows 푸시 알림 서비스) 개요](windows-push-notification-services--wns--overview.md)
* [원시 알림 개요](raw-notification-overview.md)
* [Microsoft Azure Mobile Services에 연결(JavaScript)](https://msdn.microsoft.com/library/windows/apps/dn263160)
* [Microsoft Azure Mobile Services에 연결(C#/C++/VB )](https://msdn.microsoft.com/library/windows/apps/xaml/dn263175)
* [빠른 시작: 모바일 서비스에 대한 푸시 알림 추가(JavaScript)](https://msdn.microsoft.com/library/windows/apps/dn263163)
 

 




