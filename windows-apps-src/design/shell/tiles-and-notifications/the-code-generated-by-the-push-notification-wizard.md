---
description: Visual Studio의 마법사를 사용하여 Azure Mobile Services와 함께 만든 모바일 서비스에서 푸시 알림을 생성할 수 있습니다.
title: 푸시 알림 마법사에서 생성된 코드
ms.assetid: 340F55C1-0DDF-4233-A8E4-C15EF9030785
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f9af0301dcf8944127ab814155466335940642f0
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034446"
---
# <a name="code-generated-by-the-push-notification-wizard"></a>푸시 알림 마법사에서 생성된 코드
 

Visual Studio의 마법사를 사용하여 Azure Mobile Services와 함께 만든 모바일 서비스에서 푸시 알림을 생성할 수 있습니다. Visual Studio 마법사는 시작하는 데 유용한 코드를 생성합니다. 이 항목에서는 마법사에서 프로젝트를 수정하는 방법, 생성된 코드가 수행하는 작업, 이 코드를 사용하는 방법 및 푸시 알림을 최대한 활용하기 위해 다음에 수행할 수 있는 작업에 대해 설명합니다. [WNS(Windows 푸시 알림 서비스) 개요](windows-push-notification-services--wns--overview.md)를 참조하세요.

## <a name="how-the-wizard-modifies-your-project"></a>마법사에서 프로젝트를 수정 하는 방법


푸시 알림 마법사는 다음과 같은 방법으로 프로젝트를 수정 합니다.

-   Mobile Services 관리 클라이언트 (MobileServicesManagedClient.dll)에 대 한 참조를 추가 합니다. JavaScript 프로젝트에는 적용 되지 않습니다.
-   서비스 아래의 하위 폴더에 파일을 추가 하 고 파일 이름을 push.register.cs, 또는 push.register.js로 만듭니다.
-   모바일 서비스용 데이터베이스 서버에 채널 테이블을 만듭니다. 테이블에는 앱 인스턴스에 푸시 알림을 보내는 데 필요한 정보가 포함 되어 있습니다.
-   는 삭제, 삽입, 읽기 및 업데이트의 네 가지 함수에 대 한 스크립트를 만듭니다.
-   모든 클라이언트에 푸시 알림을 보내는 notifyallusers.js 사용자 지정 API를 사용 하 여 스크립트를 만듭니다.
-   App.xaml.cs, app.xaml 또는 app.xaml 파일에 선언을 추가 하거나 JavaScript 프로젝트에 대 한 새 파일 service.js에 선언을 추가 합니다. 선언은 모바일 서비스에 연결 하는 데 필요한 정보를 포함 하는 MobileServiceClient 개체를 선언 합니다. 이름 App. *Myservicename* client를 사용 하 여 앱의 모든 페이지에서 *myservicename* client 라는이 MobileServiceClient 개체에 액세스할 수 있습니다.

services.js 파일에는 다음 코드가 포함 되어 있습니다.

```js
var <mobile-service-name>Client = new Microsoft.WindowsAzure.MobileServices.MobileServiceClient(
                "https://<mobile-service-name>.azure-mobile.net/",
                "<your client secret>");
```

## <a name="registration-for-push-notifications"></a>푸시 알림 등록


\*UploadChannel 메서드는 푸시 알림을 받기 위해 장치를 등록 합니다. 스토어는 응용 프로그램의 설치 된 인스턴스를 추적 하 고 푸시 알림 채널을 제공 합니다. [**Pushnotificationchannelmanager**](/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager)를 참조 하세요.

클라이언트 코드는 JavaScript 백 엔드와 .NET 백 엔드 모두와 비슷합니다. 기본적으로 JavaScript 백 엔드 서비스에 대 한 푸시 알림을 추가 하면 notifyAllUsers custom API에 대 한 샘플 호출이 UploadChannel 메서드에 삽입 됩니다.

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

푸시 알림 태그는 클라이언트의 하위 집합에 대 한 알림을 제한 하는 방법을 제공 합니다. 을 사용 하 여 registerNative method (또는 RegisterNativeAsync) 메서드에 태그를 지정 하지 않고 모든 푸시 알림을 등록 하거나 두 번째 인수인 태그 배열을 제공 하 여 태그를 등록할 수 있습니다. 하나 이상의 태그를 사용 하 여 등록 하면 해당 태그와 일치 하는 알림만 받게 됩니다.

## <a name="server-side-scripts-javascript-backend-only"></a>서버 쪽 스크립트 (JavaScript 백 엔드에만 해당)


JavaScript 백 엔드를 사용 하는 mobile services의 경우 삭제, 삽입, 읽기 또는 업데이트 작업이 발생 하면 서버 쪽 스크립트가 실행 됩니다. 이 스크립트는 이러한 작업을 구현 하지 않지만 클라이언트에서 Windows Mobile REST API를 호출 하 여 이러한 이벤트를 트리거할 때 실행 됩니다. 그런 다음 스크립트는 request.exe귀여운 또는 request를 호출 하 여 작업 자체에 대 한 제어를 전달 합니다. 응답 하 여 호출 컨텍스트에 응답을 발급 합니다. [Azure Mobile Services REST API 참조](/previous-versions/azure/reference/jj710108(v=azure.100))를 참조 하세요.

서버 쪽 스크립트에서 다양 한 함수를 사용할 수 있습니다. [Azure Mobile Services에서 테이블 작업 등록을](https://msdn.microsoft.com/library/azure/dn167708.aspx)참조 하세요. 사용할 수 있는 모든 함수에 대 한 참조는 [Mobile Services 서버 스크립트 참조](/previous-versions/azure/reference/jj554226(v=azure.100))를 참조 하세요.

Notifyallusers.js에서 다음과 같은 사용자 지정 API 코드도 생성 됩니다.

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

SendNotifications 함수는 알림 메시지로 단일 알림을 보냅니다. 다른 형식의 푸시 알림을 사용할 수도 있습니다.

**팁**  스크립트를 편집 하는 동안 도움을 받는 방법에 대 한 자세한 내용은 [서버 쪽 JavaScript에 대해 IntelliSense 사용](https://blogs.msdn.com/b/visualstudio/archive/2013/07/26/enabling-intellisense-for-mobile-services-javascript-in-visual-studio.aspx)을 참조 하세요.

 

## <a name="push-notification-types"></a>푸시 알림 유형


Windows에서는 푸시 알림이 아닌 알림을 지원 합니다. 알림에 대 한 일반 정보는 [알림 배달 방법 선택](choosing-a-notification-delivery-method.md)을 참조 하세요.

알림 메시지는 사용 하기 쉬우며 사용자를 위해 생성 된 채널 테이블의 Insert.js 코드에서 예제를 검토할 수 있습니다. 타일 또는 배지 알림을 사용할 계획인 경우 타일 및 배지에 대 한 XML 템플릿을 만들고 템플릿에서 패키지 정보의 인코딩을 지정 해야 합니다. [타일, 배지 및 알림 작업](/previous-versions/windows/apps/hh868259(v=win.10))을 참조 하세요.

Windows는 푸시 알림에 응답 하므로 앱이 실행 되 고 있지 않을 때 이러한 알림을 대부분 처리할 수 있습니다. 예를 들어 푸시 알림을 통해 로컬 메일 앱이 실행 되 고 있지 않은 경우에도 새 메일 메시지를 사용할 수 있는 시기를 사용자에 게 알릴 수 있습니다. Windows는 텍스트 메시지의 첫 번째 줄과 같은 메시지를 표시 하 여 알림 메시지를 처리 합니다. Windows는 새 메일 메시지 수를 반영 하도록 앱의 라이브 타일을 업데이트 하 여 타일 또는 배지 알림을 처리 합니다. 이러한 방식으로 앱의 사용자에 게 새 정보를 확인 하도록 요청할 수 있습니다. 앱이 실행 되는 동안 원시 알림을 받을 수 있으며 앱에 데이터를 전송 하는 데 사용할 수 있습니다. 앱이 실행 되 고 있지 않은 경우 푸시 알림을 모니터링 하는 백그라운드 작업을 설정할 수 있습니다.

Windows 앱에 대 한 지침에 따라 푸시 알림을 사용 해야 합니다. 이러한 알림은 사용자의 리소스를 사용 하 고 남용 경우 혼란을 받을 수 있기 때문입니다. [푸시 알림에 대 한 지침 및 검사 목록](./windows-push-notification-services--wns--overview.md)을 참조 하세요.

푸시 알림을 사용 하 여 라이브 타일을 업데이트 하는 경우 [타일 및 배지에 대 한 지침 및 검사 목록](./creating-tiles.md)의 지침을 따라야 합니다.

## <a name="next-steps"></a>다음 단계


### <a name="using-the-windows-push-notification-services-wns"></a>WNS (Windows Push Notification Services) 사용

Mobile Services에서 충분 한 유연성을 제공 하지 않거나, c # 또는 Visual Basic에서 서버 코드를 작성 하려는 경우 또는 클라우드 서비스가 이미 있고이 서비스에서 푸시 알림을 보내려는 경우에는 WNS (Windows Push Notification Services)를 직접 호출할 수 있습니다. WNS를 직접 호출 하 여 데이터베이스 또는 다른 웹 서비스의 데이터를 모니터링 하는 작업자 역할과 같은 클라우드 서비스에서 푸시 알림을 보낼 수 있습니다. 클라우드 서비스는 앱에 푸시 알림을 보내려면 WNS를 사용 하 여 인증 해야 합니다. [Windows 푸시 알림 서비스를 사용 하 여 인증 하는 방법 (JavaScript)](/previous-versions/windows/apps/hh465407(v=win.10)) 또는 [(c #/C + +/vb)](/previous-versions/windows/apps/hh868206(v=win.10))을 참조 하세요.

모바일 서비스에서 예약 된 작업을 실행 하 여 푸시 알림을 보낼 수도 있습니다. [모바일 서비스에서 되풀이 작업 예약](/azure/)을 참조하세요.

**경고**  푸시 알림 마법사를 한 번 실행 한 후에는 마법사를 두 번 실행 하 여 다른 모바일 서비스의 등록 코드를 추가 하지 마세요. 프로젝트 마다 마법사를 두 번 이상 실행 하면 [**CreatePushNotificationChannelForApplicationAsync**](/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync) 메서드에 대 한 호출이 겹쳐지는 코드를 생성 하 여 런타임 예외가 발생 합니다. 둘 이상의 모바일 서비스에 대 한 푸시 알림을 등록 하려면 마법사를 한 번 실행 한 다음 **CreatePushNotificationChannelForApplicationAsync** 에 대 한 호출이 동시에 실행 되지 않도록 등록 코드를 다시 작성 합니다. 예를 들어, 마법사에서 생성 된 코드를 push. register로 이동 하 여이를 수행할 수 있습니다. \* ( **CreatePushNotificationChannelForApplicationAsync** 에 대 한 호출 포함)은 onlaunched 된 이벤트 외부에 있지만이에 대 한 구체적인 내용은 앱의 아키텍처에 따라 달라 집니다.

 

## <a name="related-topics"></a>관련 항목


* [WNS(Windows 푸시 알림 서비스) 개요](windows-push-notification-services--wns--overview.md)
* [푸시 알림 개요](raw-notification-overview.md)
* [Windows Azure Mobile Services에 연결 (JavaScript)](/previous-versions/windows/apps/dn263160(v=win.10))
* [Windows Azure Mobile Services에 연결 (c #/C + +/VB)](/previous-versions/windows/apps/dn263175(v=win.10))
* [빠른 시작: 모바일 서비스에 대 한 푸시 알림 추가 (JavaScript)](/previous-versions/windows/apps/dn263163(v=win.10))
 

 
