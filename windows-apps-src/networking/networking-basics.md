---
author: stevewhims
description: 네트워크 지원 앱에 대해 수행해야 하는 사항입니다.
title: 네트워킹 기본 사항
ms.assetid: 1F47D33B-6F00-4F74-A52D-538851FD38BE
ms.author: stwhi
ms.date: 06/01/2018
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 50ac9fcf984fa6c4ebad7e480ebfc2d002256e26
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7167023"
---
# <a name="networking-basics"></a>네트워킹 기본 사항
네트워크 지원 앱에 대해 수행해야 하는 사항입니다.

## <a name="capabilities"></a>접근 권한 값
네트워킹을 사용하려면 앱 매니페스트에 적절한 접근 권한 값 요소를 추가해야 합니다. 네트워크 접근 권한 값이 앱 매니페스트에 지정되지 않은 경우에는 앱에 네트워킹 접근 권한 값이 없으므로 네트워크 연결 시도가 실패합니다.

다음은 가장 많이 사용되는 네트워킹 접근 권한 값입니다.

| 접근 권한 값 | 설명 |
|------------|-------------|
| **internetClient** | 인터넷 및 공공 장소(예: 공항, 커피숍)의 네트워크에 대한 아웃바운드 액세스를 제공합니다. 인터넷 액세스가 필요한 대부분의 앱은 이 접근 권한 값을 사용해야 합니다. |
| **internetClientServer** | 앱에 인터넷 및 공공 장소(예: 공항, 커피숍)의 네트워크로부터의 인바운드 및 아웃바운드 액세스를 제공합니다. |
| **privateNetworkClientServer** | 앱에 사용자가 신뢰하는 장소(예: 집, 회사)에서의 인바운드 및 아웃바운드 네트워크 액세스를 제공합니다. |

특정 상황에서 앱에 필요할 수 있는 다른 접근 권한 값도 있습니다.

| 접근 권한 값 | 설명 |
|------------|-------------|
| **enterpriseAuthentication** | 앱이 도메인 자격 증명이 필요한 네트워크 리소스에 연결할 수 있게 합니다. 이 접근 권한 값을 사용하려면 도메인 관리자가 모든 앱에 대해 해당 접근 권한 값을 사용하도록 설정해야 합니다. 개인 인트라넷의 Sharepoint 서버에서 데이터를 검색하는 앱을 예로 들 수 있습니다. <br/> 이 접근 권한 값을 사용할 경우 자격 증명이 필요한 네트워크의 네트워크 리소스에 액세스하는 데 사용자의 자격 증명을 사용할 수 있습니다. 이 접근 권한 값이 있는 앱은 네트워크에서 사용자로 가장할 수 있습니다. <br/> 앱이 인증 프록시를 통해 인터넷에 액세스하도록 허용하는 데는 이 접근 권한 값이 필요하지 않습니다. |
| **proximity** | 컴퓨터와 가까운 거리에 있는 디바이스와의 근거리 근접 통신에 필요합니다. 근거리 근접을 사용하여 가까운 장치의 응용 프로그램과 연결하거나 데이터를 보낼 수 있습니다. <br/> 이 접근 권한 값을 사용할 경우, 사용자가 초대를 보내거나 수락하는 데 동의한다면 앱에서 네트워크에 액세스하여 가까운 거리의 디바이스에 연결할 수 있습니다. |
| **sharedUserCertificates** | 이 접근 권한 값은 앱이 소프트웨어 및 하드웨어 인증서(예: 스마트 카드 인증서)에 액세스할 수 있게 합니다. 런타임에 이 접근 권한 값을 호출하면 사용자는 카드를 삽입하거나 인증서를 선택하는 것과 같은 조치를 수행해야 합니다. <br/> 이 접근 권한 값을 사용할 경우 앱에서 식별하는 데 소프트웨어 및 하드웨어 인증서 또는 스마트 카드를 사용합니다. 회사, 은행 또는 정부 기관에서 식별의 용도로 이 접근 권한 값을 사용할 수 있습니다. |

## <a name="communicating-when-your-app-is-not-in-the-foreground"></a>앱이 포그라운드에 없는 경우의 통신
[백그라운드 작업을 사용하여 앱 지원](https://msdn.microsoft.com/library/windows/apps/mt299103) 섹션에서는 앱이 포그라운드에 없을 때 작업을 위해 백그라운드 작업을 사용하는 방법에 대한 일반적인 정보를 제공합니다. 더 구체적으로 말하자면, 코드가 현재 포그라운드 앱용이 아니고 데이터가 네트워크를 통해 수신된 경우 알림을 받을 수 있도록 특별한 조치를 취해야 합니다. Windows8,에서는 이러한 목적 컨트롤 채널 트리거를 사용 하 고 Windows10에서 계속 지원 됩니다. 컨트롤 채널 트리거를 사용하는 방법에 대한 전체 정보는 [**here**](https://msdn.microsoft.com/library/windows/apps/hh701032)에서 확인할 수 있습니다. Windows10의 새로운 기술에서는 푸시 사용 스트림 소켓과 같은 일부 시나리오에 대 한 낮은 오버 헤드를 사용 하 여 더 나은 기능을 제공: 소켓 브로커와 소켓 작업 트리거 합니다.

앱에서 [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319), [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 또는 [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906)를 사용하는 경우 시스템에서 제공된 소켓 브로커로 열린 소켓의 소유권을 전송하고 포그라운드에서 벗어나거나 종료할 수도 있습니다. 전송된 소켓에 대한 연결이 설정되거나, 해당 소켓에 트래픽이 도착하면 앱 또는 지정된 백그라운드 작업이 활성화됩니다. 앱이 실행되지 않는 경우 앱이 시작됩니다. 그런 다음 소켓 브로커가 [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009)를 사용하여 새 트래픽이 도착했음을 앱에 알립니다. 앱은 소켓 브로커에서 소켓을 회수하고 해당 소켓에서 트래픽을 처리합니다. 따라서 앱은 네트워크 트래픽을 적극적으로 처리하지 않을 때 보다 적은 시스템 리소스를 사용합니다.

소켓 브로커는 동일한 기능을 제공하지만 제한 사항 및 메모리 공간이 더 적기 때문에 해당되는 경우 컨트롤 채널 트리거를 대체하기 위한 용도로 사용됩니다. 소켓 브로커는 화면 앱을 잠그지 않는 앱에서 사용될 수 있으며, 다른 장치와 동일한 방식으로 휴대폰에서 사용됩니다. 트래픽이 도착했을 때 소켓 브로커에서 활성화하기 위해 앱을 실행할 필요가 없습니다. 소켓 브로커는 컨트롤 채널 트리거에서 지원하지 않는 TCP 소켓의 수신 대기도 지원합니다.

### <a name="choosing-a-network-trigger"></a>네트워크 트리거 선택
특정한 종류의 트리거가 적합한 몇 가지 시나리오가 있습니다. 앱에서 사용할 트리거 종류를 선택할 때 다음 사항을 고려합니다.

-   [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151), [**System.Net.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 또는 [System.Net.Http.HttpClientHandler](http://go.microsoft.com/fwlink/p/?linkid=241638)를 사용하는 경우 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)를 사용해야 합니다.
-   푸시 사용 **StreamSockets**를 사용하는 경우 컨트롤 채널 트리거를 사용할 수 있지만 [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009)를 사용하는 것이 더 좋습니다. 후자는 연결이 적극적으로 사용되지 않는 경우 시스템에서 메모리를 확보하고 전원 요구 사항을 줄일 수 있습니다.
-   앱에서 네트워크 요청을 적극적으로 처리하지 않는 경우 앱의 메모리 공간을 최소화하려면 가능한 경우 [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009)를 사용하는 것이 좋습니다.
-   시스템은 연결된 대기 상태 모드에 있는 동안 앱에서 데이터를 받을 수 있도록 하려면 [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009)를 사용하는 것이 좋습니다.

소켓 브로커를 사용하는 방법에 대한 자세한 내용과 예는 [백그라운드에서의 네트워크 통신](network-communications-in-the-background.md)을 참조하세요.

## <a name="secured-connections"></a>보안 연결
SSL(Secure Sockets Layer) 및 더 최근의 TLS(전송 계층 보안)는 네트워크 통신에 인증 및 암호화를 제공하도록 설계된 암호화 프로토콜입니다. 이러한 프로토콜은 네트워크 데이터를 보내고 받을 때 도청 및 변조를 방지하도록 설계되었습니다. 이러한 프로토콜은 프로토콜 교환에 클라이언트-서버 모델을 사용합니다. 이러한 프로토콜은 또한 디지털 인증서 및 인증 기관을 사용하여 서버가 인증이 필요한 개체인지 확인합니다.

### <a name="creating-secure-socket-connections"></a>보안 소켓 연결 만들기
클라이언트와 서버 간 통신에 SSL/TLS를 사용하도록 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 개체를 구성할 수 있습니다. SSL/TLS에 대한 이 지원은 클라이언트가 SSL/TLS 협상 중일 때 **StreamSocket** 개체를 사용하는 것으로 제한됩니다. **StreamSocket** 클래스에서 서버로서의 SSL/TLS 협상을 구현하지 않으므로 들어오는 통신을 수신할 때 [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906)가 생성한 **StreamSocket**에서 SSL/TLS를 사용할 수 없습니다.

SSL/TLS를 통해 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 연결의 보안을 유지하는 방법은 다음 두 가지입니다.

-   [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) - 네트워크 서비스에 대한 초기 연결을 설정하고 모든 통신에 SSL/TLS를 사용하도록 즉시 협상합니다.
-   [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) - 처음에 암호화 없이 네트워크 서비스에 연결합니다. 앱은 데이터를 보내거나 받을 수 있습니다. 그런 다음 이후의 모든 통신에 SSL/TLS를 사용하도록 연결을 업그레이드합니다.

SocketProtectionLevel은 앱이 연결을 설정하거나 업그레이드하고자 하는, 원하는 소켓 보호 수준을 지정합니다. 그러나 설정된 연결의 최종 보호 수준은 연결의 두 끝점 간의 협상 프로세스에서 결정됩니다. 결과적으로, 다른 끝점에서 더 낮은 수준을 요청하는 경우에는 지정한 것보다 낮은 보호 수준을 얻게 될 수 있습니다. 

 비동기 작업이 완료된 후, [**StreamSocketinformation.ProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/hh967868) 속성을 통해 [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) 또는 [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) 호출에 사용된 요청 받은 보호 수준을 검색할 수 있습니다. 그러나 이는 연결에 사용 중인 실제 보호 수준을 반영하지 않습니다.

> [!NOTE]
> 코드는 암시적으로 특정 보호 수준 사용에 의존하거나 특정 보안 수준이 기본적으로 사용된다는 가정에 의존해서는 안 됩니다. 보안 환경은 지속적으로 변화하며, 알려진 단점이 있는 프로토콜을 사용하는 것을 피하기 위해 프로토콜 및 기본 보호 수준은 시간이 지남에 따라 변경됩니다. 기본값은 개별 컴퓨터 구성에 따라, 또는 설치된 소프트웨어 및 적용된 패치에 따라 다양할 수 있습니다. 앱을 사용하려면 특정 보안 수준을 사용해야 하는 경우 해당 수준을 명시적으로 지정한 다음 설정된 연결에서 이 수준이 실제로 사용되고 있는지 확인해야 합니다.

### <a name="use-connectasync"></a>ConnectAsync 사용
[**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504)를 사용하면 네트워크 서비스에 대한 초기 연결을 설정한 다음 모든 통신에 SSL/TLS를 사용하도록 즉시 협상할 수 있습니다. *protectionLevel* 매개 변수 전달을 지원하는 두 가지 **ConnectAsync** 메서드는 다음과 같습니다.

-   [**ConnectAsync(EndpointPair, SocketProtectionLevel)**](https://msdn.microsoft.com/library/windows/apps/hh701511) - [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 개체에서 비동기 작업을 시작하여 [**EndpointPair**](https://msdn.microsoft.com/library/windows/apps/hh700953) 개체 및 [**SocketProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/br226880)로 지정된 원격 네트워크 대상에 연결합니다.
-   [**ConnectAsync(HostName, String, SocketProtectionLevel)**](https://msdn.microsoft.com/library/windows/apps/br226916) - [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 개체에서 비동기 작업을 시작하여 원격 호스트 이름, 원격 서비스 이름 및 [**SocketProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/br226880)로 지정한 원격 대상에 연결합니다.

위 [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) 메서드 중 하나를 호출할 때 *protectionLevel* 매개 변수가 **Windows.Networking.Sockets.SocketProtectionLevel.Ssl**로 설정되어 있으면 암호화에 SSL/TLS를 사용하도록 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)을 설정해야 합니다. 이 값에는 암호화가 필요하고 NULL 암호를 사용할 수 없습니다.

이러한 [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) 메서드 중 하나에 사용할 일반적인 순서는 동일합니다.

-   [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)을 만듭니다.
-   소켓에 대한 고급 옵션이 필요하면 [**StreamSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226917) 속성을 사용하여 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 개체와 연결된 [**StreamSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226893) 인스턴스를 가져옵니다. **StreamSocketControl**에 대한 속성을 설정합니다.
-   위 [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) 메서드 중 하나를 호출하여 원격 대상에 연결하는 작업을 시작하고 SSL/TLS를 사용하도록 즉시 협상합니다.
-   실제로 [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504)를 사용하여 협상된 SSL 수준은 비동기 작업이 완료된 후에 [**StreamSocketinformation.ProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/hh967868) 속성을 가져와서 결정할 수 있습니다.

다음 예제에서는 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)을 만들고 네트워크 서비스에 대한 연결을 시도한 다음 SSL/TLS를 사용하도록 즉시 협상합니다. 협상에 성공하면 클라이언트와 네트워크 서버 간에 **StreamSocket**을 사용하는 모든 네트워크 통신이 암호화됩니다.

```csharp
using Windows.Networking;
using Windows.Networking.Sockets;

    // Define some variables and set values
    StreamSocket clientSocket = new StreamSocket();
     
    HostName serverHost = new HostName("www.contoso.com");
    string serverServiceName = "https";
    
    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 
    
    // Try to connect to contoso using HTTPS (port 443)
    try {

        // Call ConnectAsync method with SSL
        await clientSocket.ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel.Ssl);

        NotifyUser("Connected");
    }
    catch (Exception exception) {
        // If this is an unknown status it means that the error is fatal and retry will likely fail.
        if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
            throw;
        }
        
        NotifyUser("Connect failed with error: " + exception.Message);
        // Could retry the connection, but for this simple example
        // just close the socket.
        
        clientSocket.Dispose();
        clientSocket = null; 
    }
           
    // Add code to send and receive data using the clientSocket
    // and then close the clientSocket
```

```cppwinrt
#include <winrt/Windows.Networking.Sockets.h>

using namespace winrt;
...
    // Define some variables, and set values.
    Windows::Networking::Sockets::StreamSocket clientSocket;

    Windows::Networking::HostName serverHost{ L"www.contoso.com" };
    winrt::hstring serverServiceName{ L"https" };

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages.

    // Try to connect to the server using HTTPS and SSL (port 443).
    try
    {
        co_await clientSocket.ConnectAsync(serverHost, serverServiceName, Windows::Networking::Sockets::SocketProtectionLevel::Tls12);
        NotifyUser(L"Connected");
    }
    catch (winrt::hresult_error const& exception)
    {
        NotifyUser(L"Connect failed with error: " + exception.message());
        clientSocket = nullptr;
    }
    // Add code to send and receive data using the clientSocket,
    // then set the clientSocket to nullptr when done to close it.
```

```cpp
using Windows::Networking;
using Windows::Networking::Sockets;

    // Define some variables and set values
    StreamSocket^ clientSocket = new ref StreamSocket();
 
    HostName^ serverHost = new ref HostName("www.contoso.com");
    String serverServiceName = "https";

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 

    // Try to connect to the server using HTTPS and SSL (port 443)
    task<void>(clientSocket->ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel::SSL)).then([this] (task<void> previousTask) {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();
            NotifyUser("Connected");
        }
        catch (Exception^ exception)
        {
            NotifyUser("Connect failed with error: " + exception->Message);
            
            clientSocket.Close();
            clientSocket = null;
        }
    });
    // Add code to send and receive data using the clientSocket
    // Then close the clientSocket when done
```

### <a name="use-upgradetosslasync"></a>UpgradeToSslAsync 사용
코드에 [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922)를 사용하면 이 코드는 암호화하지 않고 먼저 네트워크 서비스에 대한 연결을 설정합니다. 앱은 일부 데이터를 보내거나 받은 다음 이후의 모든 통신에 SSL/TLS를 사용하도록 연결을 업그레이드할 수 있습니다.

[**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) 메서드는 두 개의 매개 변수를 사용합니다. *protectionLevel* 매개 변수는 원하는 보호 수준을 나타냅니다. *validationHostName* 매개 변수는 SSL로 업그레이드할 때 유효성 검사에 사용되는 원격 네트워크 대상의 호스트 이름입니다. 일반적으로 *validationHostName*은 앱이 처음에 연결을 설정하는 데 사용한 것과 같은 호스트 이름입니다. **UpgradeToSslAsync**를 호출할 때 *protectionLevel* 매개 변수가 **Windows.System.Socket.SocketProtectionLevel.Ssl**로 설정되어 있으면 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)은 소켓을 통한 이후 통신에 대한 암호화에 SSL/TLS를 사용해야 합니다. 이 값에는 암호화가 필요하고 NULL 암호를 사용할 수 없습니다.

[**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) 메서드에 사용하는 일반적인 순서는 다음과 같습니다.

-   [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)을 만듭니다.
-   소켓에 대한 고급 옵션이 필요하면 [**StreamSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226917) 속성을 사용하여 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 개체와 연결된 [**StreamSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226893) 인스턴스를 가져옵니다. **StreamSocketControl**에 대한 속성을 설정합니다.
-   데이터를 암호화하지 않은 상태로 보내고 받아야 할 경우 지금 보냅니다.
-   [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) 메서드를 호출하여 작업을 시작하고 SSL/TLS를 사용하도록 연결을 업그레이드합니다.
-   실제로 [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922)를 사용하여 협상된 SSL 수준은 비동기 작업이 완료된 후에 [**StreamSocketinformation.ProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/hh967868) 속성을 가져와서 결정할 수 있습니다.

다음 예제에서는 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)을 만들고 네트워크 서비스에 대한 연결을 시도한 후 일부 초기 데이터를 보내고 SSL/TLS를 사용하도록 협상합니다. 협상에 성공하면 클라이언트와 네트워크 서버 간에 **StreamSocket**을 사용하는 모든 네트워크 통신이 암호화됩니다.

```csharp
using Windows.Networking;
using Windows.Networking.Sockets;
using Windows.Storage.Streams;

    // Define some variables and set values
    StreamSocket clientSocket = new StreamSocket();
 
    HostName serverHost = new HostName("www.contoso.com");
    string serverServiceName = "http";

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 

    // Try to connect to contoso using HTTP (port 80)
    try {
        // Call ConnectAsync method with a plain socket
        await clientSocket.ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel.PlainSocket);

        NotifyUser("Connected");

    }
    catch (Exception exception) {
        // If this is an unknown status it means that the error is fatal and retry will likely fail.
        if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
            throw;
        }

        NotifyUser("Connect failed with error: " + exception.Message, NotifyType.ErrorMessage);
        // Could retry the connection, but for this simple example
        // just close the socket.

        clientSocket.Dispose();
        clientSocket = null; 
        return;
    }

    // Now try to send some data
    DataWriter writer = new DataWriter(clientSocket.OutputStream);
    string hello = "Hello, World! ☺ ";
    Int32 len = (int) writer.MeasureString(hello); // Gets the UTF-8 string length.
    writer.WriteInt32(len);
    writer.WriteString(hello);
    NotifyUser("Client: sending hello");

    try {
        // Call StoreAsync method to store the hello message
        await writer.StoreAsync();

        NotifyUser("Client: sent data");

        writer.DetachStream(); // Detach stream, if not, DataWriter destructor will close it.
    }
    catch (Exception exception) {
        NotifyUser("Store failed with error: " + exception.Message);
        // Could retry the store, but for this simple example
            // just close the socket.

            clientSocket.Dispose();
            clientSocket = null; 
            return;
    }

    // Now upgrade the client to use SSL
    try {
        // Try to upgrade to SSL
        await clientSocket.UpgradeToSslAsync(SocketProtectionLevel.Ssl, serverHost);

        NotifyUser("Client: upgrade to SSL completed");
           
        // Add code to send and receive data 
        // The close clientSocket when done
    }
    catch (Exception exception) {
        // If this is an unknown status it means that the error is fatal and retry will likely fail.
        if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
            throw;
        }

        NotifyUser("Upgrade to SSL failed with error: " + exception.Message);

        clientSocket.Dispose();
        clientSocket = null; 
        return;
    }
```

```cppwinrt
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>

using namespace winrt;
using namespace Windows::Storage::Streams;
...
    // Define some variables, and set values.
    Windows::Networking::Sockets::StreamSocket clientSocket;

    Windows::Networking::HostName serverHost{ L"www.contoso.com" };
    winrt::hstring serverServiceName{ L"https" };

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages. 

    // Try to connect to the server using HTTP (port 80).
    try
    {
        co_await clientSocket.ConnectAsync(serverHost, serverServiceName, Windows::Networking::Sockets::SocketProtectionLevel::PlainSocket);
        NotifyUser(L"Connected");
    }
    catch (winrt::hresult_error const& exception)
    {
        NotifyUser(L"Connect failed with error: " + exception.message());
        clientSocket = nullptr;
    }

    // Now, try to send some data.
    DataWriter writer{ clientSocket.OutputStream() };
    winrt::hstring hello{ L"Hello, World! ☺ " };
    uint32_t len{ writer.MeasureString(hello) }; // Gets the size of the string, in bytes.
    writer.WriteInt32(len);
    writer.WriteString(hello);
    NotifyUser(L"Client: sending hello");

    try
    {
        co_await writer.StoreAsync();
        NotifyUser(L"Client: sent hello");

        writer.DetachStream(); // Detach the stream when you want to continue using it; otherwise, the DataWriter destructor closes it.
    }
    catch (winrt::hresult_error const& exception)
    {
        NotifyUser(L"Store failed with error: " + exception.message());
        // We could retry the store operation. But, for this simple example, just close the socket by setting it to nullptr.
        clientSocket = nullptr;
        co_return;
    }

    // Now, upgrade the client to use SSL.
    try
    {
        co_await clientSocket.UpgradeToSslAsync(Windows::Networking::Sockets::SocketProtectionLevel::Tls12, serverHost);
        NotifyUser(L"Client: upgrade to SSL completed");

        // Add code to send and receive data using the clientSocket,
        // then set the clientSocket to nullptr when done to close it.
    }
    catch (winrt::hresult_error const& exception)
    {
        // If this is an unknown status, then the error is fatal and retry will likely fail.
        Windows::Networking::Sockets::SocketErrorStatus socketErrorStatus{ Windows::Networking::Sockets::SocketError::GetStatus(exception.to_abi()) };
        if (socketErrorStatus == Windows::Networking::Sockets::SocketErrorStatus::Unknown)
        {
            throw;
        }

        NotifyUser(L"Upgrade to SSL failed with error: " + exception.message());
        // We could retry the store operation. But for this simple example, just close the socket by setting it to nullptr.
        clientSocket = nullptr;
        co_return;
    }
```

```cpp
using Windows::Networking;
using Windows::Networking::Sockets;
using Windows::Storage::Streams;

    // Define some variables and set values
    StreamSocket^ clientSocket = new ref StreamSocket();
 
    Hostname^ serverHost = new ref HostName("www.contoso.com");
    String serverServiceName = "http";

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 

    // Try to connect to contoso using HTTP (port 80)
    task<void>(clientSocket->ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel::PlainSocket)).then([this] (task<void> previousTask) {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();
            NotifyUser("Connected");
        }
        catch (Exception^ exception)
        {
            NotifyUser("Connect failed with error: " + exception->Message);
 
            clientSocket->Close();
            clientSocket = null;
        }
    });
       
    // Now try to send some data
    DataWriter^ writer = new ref DataWriter(clientSocket.OutputStream);
    String hello = "Hello, World! ☺ ";
    Int32 len = (int) writer->MeasureString(hello); // Gets the UTF-8 string length.
    writer->writeInt32(len);
    writer->writeString(hello);
    NotifyUser("Client: sending hello");

    task<void>(writer->StoreAsync()).then([this] (task<void> previousTask) {
        try {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();

            NotifyUser("Client: sent hello");

            writer->DetachStream(); // Detach stream, if not, DataWriter destructor will close it.
       }
       catch (Exception^ exception) {
               NotifyUser("Store failed with error: " + exception->Message);
               // Could retry the store, but for this simple example
               // just close the socket.
 
               clientSocket->Close();
               clientSocket = null;
               return
       }
    });

    // Now upgrade the client to use SSL
    task<void>(clientSocket->UpgradeToSslAsync(clientSocket.SocketProtectionLevel.Ssl, serverHost)).then([this] (task<void> previousTask) {
        try {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();

           NotifyUser("Client: upgrade to SSL completed");
           
           // Add code to send and receive data 
           // Then close clientSocket when done
        }
        catch (Exception^ exception) {
            // If this is an unknown status it means that the error is fatal and retry will likely fail.
            if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
                throw;
            }

            NotifyUser("Upgrade to SSL failed with error: " + exception.Message);

            clientSocket->Close();
            clientSocket = null; 
            return;
        }
    });
```

### <a name="creating-secure-websocket-connections"></a>보안 WebSocket 연결 만들기
기존 소켓 연결과 마찬가지로 WebSocket 연결도 UWP 앱용 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 및 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 기능을 사용할 경우 TLS(전송 계층 보안)/SSL(Secure Sockets Layer)을 통해 암호화할 수 있습니다. 대부분의 경우 보안 WebSocket 연결을 사용합니다. 이 경우 많은 프록시가 암호화되지 않은 WebSocket 연결을 거부하므로 연결이 성공할 가능성이 높아집니다.

네트워크 서비스에 대한 보안 소켓 연결을 만들거나 해당 연결로 업그레이드하는 방법에 대한 예는 [TLS/SSL을 통해 WebSocket 연결의 보안을 유지하는 방법](https://msdn.microsoft.com/library/windows/apps/xaml/hh994399)을 참조하세요.

서버에서 초기 핸드셰이크를 완료하려면 TLS/SSL 암호화 외에도 **Sec-WebSocket-Protocol** 헤더 값이 필요할 수 있습니다. [**StreamWebSocketInformation.Protocol**](https://msdn.microsoft.com/library/windows/apps/hh701514) 및 [**MessageWebSocketInformation.Protocol**](https://msdn.microsoft.com/library/windows/apps/hh701358) 속성으로 표시되는 이 값은 연결의 프로토콜 버전을 나타내며 서버에서 여는 핸드셰이크 및 나중에 교환할 데이터를 올바르게 해석할 수 있도록 합니다. 이 프로토콜 정보를 사용하면 서버가 들어오는 데이터를 안전한 방식으로 해석할 수 없는 모든 지점에서 연결을 닫을 수 있습니다.

클라이언트의 초기 요청에 이 값이 포함되어 있지 않거나 서버에서 예상하는 값과 일치하지 않는 값을 제공하면 WebSocket 핸드셰이크 오류 시 예상한 값이 서버에서 클라이언트로 전송됩니다.

## <a name="authentication"></a>인증
네트워크를 통해 연결할 때 인증 자격 증명을 제공하는 방법입니다.

### <a name="providing-a-client-certificate-with-the-streamsocket-class"></a>StreamSocket 클래스를 사용하여 클라이언트 인증서 제공
[**Windows.Networking.StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 클래스는 SSL/TLS를 사용하여 앱에서 통신하는 서버를 인증하도록 지원합니다. 앱에서 자체적으로 TLS 클라이언트 인증서를 사용하여 서버에 인증해야 하는 경우도 있습니다. Windows10, [**StreamSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226893) 개체 (이 설정 되어야 합니다 TLS 핸드셰이크를 시작 하기 전에)에서 클라이언트 인증서를 제공할 수 있습니다. 서버에서 클라이언트 인증서를 요청한 경우 Windows에서 제공된 인증서로 응답합니다.

다음은 이를 구현하는 방법을 보여 주는 코드 조각입니다.

```csharp
var socket = new StreamSocket();
Windows.Security.Cryptography.Certificates.Certificate certificate = await GetClientCert();
socket.Control.ClientCertificate = certificate;
await socket.ConnectAsync(destination, SocketProtectionLevel.Tls12);
```

### <a name="providing-authentication-credentials-to-a-web-service"></a>웹 서비스에 인증 자격 증명 제공
앱이 보안 웹 서비스와 상호 작용할 수 있도록 하는 네트워킹 API는 각각 서버 및 프록시 인증 자격 증명으로 클라이언트를 초기화하거나 요청 헤더를 설정하는 고유한 메서드를 제공합니다. 각 메서드는 이러한 자격 증명이 사용되는 사용자 이름, 암호 및 리소스를 나타내는 [**PasswordCredential**](https://msdn.microsoft.com/library/windows/apps/br227061) 개체로 설정됩니다. 다음 표에서는 이러한 API의 매핑을 제공합니다.

| **WebSocket** | [**MessageWebSocketControl.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br226848) |
|-------------------------|----------------------------------------------------------------------------------------------------------|
|  | [**MessageWebSocketControl.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br226847) |
|  | [**StreamWebSocketControl.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br226928) |
|  | [**StreamWebSocketControl.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br226927) |
| **백그라운드 전송** | [**BackgroundDownloader.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/hh701076) |
|  | [**BackgroundDownloader.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/hh701068) |
|  | [**BackgroundUploader.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/hh701184) |
|  | [**BackgroundUploader.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/hh701178) |
| **신디케이션** | [**SyndicationClient(PasswordCredential)**](https://msdn.microsoft.com/library/windows/apps/hh702355) |
|  | [**SyndicationClient.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br243461) |
|  | [**SyndicationClient.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br243459) |
| **AtomPub** | [**AtomPubClient(PasswordCredential)**](https://msdn.microsoft.com/library/windows/apps/hh702262) |
|  | [**AtomPubClient.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br243428) |
|  | [**AtomPubClient.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br243423) |

## <a name="handling-network-exceptions"></a>네트워크 예외 처리
대부분의 프로그래밍 영역에서 예외는 프로그램의 일부 결함으로 인해 발생하는 중요한 문제나 오류를 나타냅니다. 네트워크 프로그래밍에는 예외에 대한 추가적인 원인(예: 네트워크 자체 및 네트워크 통신 특성)이 있습니다. 네트워크 통신은 본질적으로 신뢰할 수 없으며 예기치 않은 오류가 발생하기 쉽습니다. 앱에서 네트워킹을 사용하는 각 방법에 대해 일부 상태 정보를 유지 관리해야 하며, 앱 코드에서 해당 상태 정보를 업데이트하고 앱이 통신 오류를 다시 설정하거나 다시 시도할 수 있는 적절한 논리를 시작하여 네트워크를 예외를 처리해야 합니다.

유니버설 Windows 앱에서 예외를 throw한 경우 예외 처리기는 예외의 원인에 대해 보다 자세한 정보를 검색하므로 오류를 더 잘 이해하고 적절한 의사 결정을 내릴 수 있습니다.

각 언어 프로젝션은 보다 자세한 정보에 액세스하는 메서드를 지원합니다. 예외는 유니버설 Windows 앱에서 **HRESULT** 값으로 표시됩니다. *Winerror.h* 포함 파일에는 네트워크 오류를 포함하여 가능한 **HRESULT** 값의 매우 큰 목록이 포함되어 있습니다.

네트워킹 API는 예외의 원인에 대해 자세한 정보를 검색하는 여러 가지 메서드를 지원합니다.

-   일부 API는 예외의 **HRESULT** 값을 열거형 값으로 변환하는 도우미 메서드를 제공합니다.
-   다른 API는 실제 **HRESULT** 값을 검색하는 메서드를 제공합니다.

## <a name="related-topics"></a>관련 항목
* [Windows 10의 향상된 네트워킹 API 기능](http://blogs.windows.com/buildingapps/2015/07/02/networking-api-improvements-in-windows-10/)
