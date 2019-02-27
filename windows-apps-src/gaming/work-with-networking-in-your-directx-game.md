---
title: 게임의 네트워킹
description: 네트워킹 기능을 개발하고 DirectX 게임에 통합하는 방법을 알아봅니다.
ms.assetid: 212eee15-045c-8ba1-e274-4532b2120c55
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 네트워킹, directx
ms.localizationpriority: medium
ms.openlocfilehash: e3dc77b48feb0c7ceba9fa3cede82c1a44687d0d
ms.sourcegitcommit: 175d0fc32db60017705ab58136552aee31407412
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2019
ms.locfileid: "9114629"
---
# <a name="networking-for-games"></a>게임의 네트워킹



네트워킹 기능을 개발하고 DirectX 게임에 통합하는 방법을 알아봅니다.

## <a name="concepts-at-a-glance"></a>개념 개요


간단한 독립 실행형 게임에서 대규모 멀티 플레이어 게임에 이르기까지 DirectX 게임에서 다양한 네트워킹 기능을 사용할 수 있습니다. 가장 간단한 네트워킹 사용은 중앙 네트워크 서버에 사용자 이름과 게임 점수를 저장하는 것입니다.

네트워킹 API는 인프라(클라이언트-서버 또는 인터넷 피어 투 피어) 모델을 사용하는 멀티 플레이어 게임과 애드혹(로컬 피어 투 피어) 게임에 필요합니다. 서버 기반 멀티 플레이어 게임의 경우 일반적으로 중앙 게임 서버가 게임 작업을 대부분 처리하고 클라이언트 게임 앱은 입력, 그래픽 표시, 오디오 재생 및 기타 기능에 사용됩니다. 네트워크 전송 속도 및 대기 시간은 만족스러운 게임 환경에 중요합니다.

피어 투 피어 게임의 경우 각 플레이어의 앱이 입력과 그래픽을 처리합니다. 대부분의 경우 게임 플레이어가 서로 가까운 곳에 있으므로 네트워크 대기 시간이 더 짧지만 여전히 중요합니다. 피어를 검색하고 연결을 설정하는 방법도 문제가 됩니다.

싱글 플레이어 게임의 경우 대체로 중앙 웹 서버 또는 서비스를 사용하여 사용자 이름, 게임 점수 및 기타 정보를 저장합니다. 이러한 게임에서는 네트워크 전송의 속도 및 대기 시간이 게임 작업에 직접 영향을 주지 않으므로 덜 중요합니다.

네트워크 상태는 언제든지 변할 수 있으므로 네트워킹 API를 사용하는 모든 게임은 발생할 수 있는 네트워크 예외를 처리해야 합니다. 네트워크 예외 처리에 대한 자세한 내용은 [네트워킹 기본 사항](https://msdn.microsoft.com/library/windows/apps/mt280233)을 참조하세요.

방화벽과 웹 프록시는 공통적으로 사용되며, 네트워킹 기능을 사용하는 능력에 영향을 줄 수 있습니다. 네트워킹을 사용하는 게임은 방화벽과 프록시를 제대로 처리하도록 준비해야 합니다.

모바일 장치의 경우 로밍 또는 데이터 비용이 상당할 수 있는 데이터 통신 연결 네트워크를 사용할 때는 사용 가능한 네트워크 리소스를 모니터링하고 적절하게 동작하는 것이 중요합니다.

네트워크 격리는 Windows에서 사용하는 앱 보안 모델의 일부입니다. Windows에서는 적극적으로 네트워크 경계를 검색하고 네트워크 격리를 위한 네트워크 액세스 제한을 적용합니다. 앱은 네트워크 액세스 범위를 정의하기 위해 네트워크 격리 기능을 선언해야 합니다. 이러한 기능을 선언하지 않으면 앱이 네트워크 리소스에 액세스할 수 없습니다. Windows에서 앱에 네트워크 격리를 적용하는 방법에 대한 자세한 내용은 [네트워크 격리 기능을 구성하는 방법](https://msdn.microsoft.com/library/windows/apps/hh770532)을 참조하세요.

## <a name="design-considerations"></a>디자인 고려 사항


DirectX 게임에서는 다양한 네트워킹 API를 사용할 수 있습니다. 따라서 올바른 API를 선택하는 것이 중요합니다. Windows는 앱이 인터넷이나 개인 네트워크를 통해 다른 컴퓨터 및 장치와 통신하는 데 사용할 수 있는 다양한 네트워킹 API를 지원합니다. 첫 번째로 수행할 단계는 앱에 필요한 네트워킹 기능이 무엇인지 파악하는 것입니다.

게임에 많이 사용되는 네트워크 API는 다음과 같습니다.

-   TCP 및 소켓 - 안정적인 연결을 제공합니다. 보안이 필요 없는 게임 작업에는 TCP를 사용합니다. TCP를 사용하면 서버가 쉽게 확장되므로 인프라(클라이언트-서버 또는 인터넷 피어 투 피어) 모델을 사용하는 게임에서 자주 사용됩니다. Wi-Fi Direct 및 Bluetooth를 통해 애드혹(로컬 피어 투 피어) 게임에서 TCP를 사용할 수도 있습니다. TCP는 게임 개체 이동, 문자 조작, 텍스트 채팅 및 기타 작업에 주로 사용됩니다. [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 클래스는 Microsoft Store 게임에서 사용할 수 있는 TCP 소켓을 제공 합니다. **StreamSocket** 클래스는 [**Windows::Networking::Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 네임스페이스의 관련 클래스와 함께 사용됩니다.
-   SSL을 사용한 TCP 및 소켓 - 도청을 차단하는 안정적인 연결을 제공합니다. 보안이 필요한 게임 작업에는 SSL과 함께 TCP 연결을 사용합니다. SSL의 암호화 및 오버헤드로 인해 대기 시간과 성능이 저하되므로 보안이 필요할 때만 사용됩니다. SSL을 사용한 TCP는 로그인, 구매 및 거래 자산, 게임 문자 생성 및 관리에 주로 사용됩니다. [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 클래스는 SSL을 지원하는 TCP 소켓을 제공합니다.
-   UDP 및 소켓 - 오버헤드가 작은 신뢰할 수 없는 네트워크 전송을 제공합니다. UDP는 짧은 대기 시간이 필요하고 일부 패킷 손실을 허용할 수 있는 게임 작업에 사용됩니다. 전투 게임, 슈팅 및 추적 프로그램, 네트워크 오디오 및 음성 채팅에 주로 사용됩니다. [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) 클래스는 Microsoft Store 게임에서 사용할 수 있는 UDP 소켓을 제공 합니다. **DatagramSocket** 클래스는 [**Windows::Networking::Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 네임스페이스의 관련 클래스와 함께 사용됩니다.
-   HTTP 클라이언트 - HTTP 서버에 대한 안정적인 연결을 제공합니다. 가장 일반적인 네트워킹 시나리오는 정보를 검색하거나 저장하기 위해 웹 사이트에 액세스하는 것입니다. 간단한 예로 웹 사이트를 사용하여 사용자 정보와 게임 점수를 저장하는 게임이 있습니다. 보안을 위해 SSL과 함께 사용할 경우 HTTP 클라이언트를 로그인, 구매, 거래 자산, 게임 문자 생성 및 관리에 사용할 수 있습니다. [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 클래스는 최신 HTTP 클라이언트 API 사용에 대 한 Microsoft Store 게임에서 제공합니다. **HttpClient** 클래스는 [**Windows::Web::Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) 네임스페이스의 관련 클래스와 함께 사용됩니다.

## <a name="handling-network-exceptions-in-your-directx-game"></a>DirectX 게임에서 네트워크 예외 처리


DirectX 게임에서 네트워크 예외가 발생하면 이는 중요한 문제나 오류가 있음을 나타냅니다. 네트워킹 API 사용 시 여러 가지 이유로 예외가 발생할 수 있습니다. 예외는 종종 네트워크 연결의 변경 또는 원격 호스트나 서버의 기타 네트워킹 문제로 인해 발생할 수 있습니다.

네트워킹 API를 사용하는 경우 몇 가지 예외의 원인은 다음과 같습니다.

-   사용자가 입력한 호스트 이름 또는 URI에 오류가 포함되어 있고 이러한 입력이 유효하지 않음
-   호스트 이름 또는 URI를 조회할 때 이름 확인 실패
-   네트워크 연결 끊김 또는 변경
-   소켓 또는 HTTP 클라이언트 API를 사용하는 네트워크 연결 오류
-   네트워크 서버 또는 원격 끝점 오류
-   기타 네트워킹 오류

네트워크 오류(예: 연결 끊김 또는 변경, 연결 실패 및 서버 오류)로 인한 예외는 언제든지 발생할 수 있습니다. 이러한 오류로 인해 예외가 발생합니다. 앱에서 처리되지 않은 예외로 인해 전체 앱이 런타임에 종료될 수 있습니다.

따라서 대부분의 비동기 네트워크 메서드를 호출할 때 예외를 처리하는 코드를 작성해야 합니다. 경우에 따라 예외가 발생하면 문제를 해결하는 방법으로 네트워크 방법을 다시 시도할 수 있습니다. 다른 경우 앱이 이전에 캐시된 데이터를 사용하여 네트워크 연결 없이 계속되도록 계획해야 합니다.

UWP(유니버설 Windows 플랫폼) 앱에서는 일반적으로 단일 예외가 발생합니다. 예외 처리기는 예외의 원인에 대해 보다 자세한 정보를 검색하므로 오류를 더 잘 이해하고 적절한 의사 결정을 내릴 수 있습니다.

UWP 앱인 DirectX 게임에 예외가 발생하면 오류의 원인으로 **HRESULT** 값을 검색할 수 있습니다. *Winerror.h* 포함 파일에는 네트워크 오류를 포함하여 가능한 **HRESULT** 값의 큰 목록이 포함되어 있습니다.

네트워킹 API는 예외의 원인에 대해 자세한 정보를 검색하는 여러 가지 메서드를 지원합니다.

-   예외를 일으킨 오류의 **HRESULT** 값을 검색하기 위한 메서드. 잠재적 **HRESULT** 값의 가능한 목록은 크기가 크며 지정되지 않습니다. 네트워킹 API를 사용할 경우 **HRESULT** 값을 검색할 수 있습니다.
-   **HRESULT** 값을 열거형 값으로 변환하는 도우미 메서드. 가능한 열거 값의 목록은 지정되며 비교적 크기가 작습니다. 도우미 메서드는 [**Windows::Networking::Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960)의 소켓 클래스에 사용할 수 있습니다.

### <a name="exceptions-in-windowsnetworkingsockets"></a>Windows.Networking.Sockets의 예외

전달된 문자열이 유효한 호스트 이름이 아닌 경우(호스트 이름에 허용되지 않는 문자 포함) 소켓과 함께 사용된 [**HostName**](https://msdn.microsoft.com/library/windows/apps/br207113) 클래스에 대한 생성자에서 예외가 발생할 수 있습니다. 앱이 게이밍을 위한 피어 연결의 **HostName**에 대해 사용자 입력을 받으면 생성자는 try/catch 블록에 있게 됩니다. 예외가 발생하면 앱에서 사용자에게 알리고 새 호스트 이름을 요청할 수 있습니다.

사용자가 입력한 호스트 이름의 문자열을 확인하기 위한 코드 추가

```cpp

    // Define some variables at the class level.
    Windows::Networking::HostName^ remoteHost;

    bool isHostnameFromUser = false;
    bool isHostnameValid = false;

    ///...

    // If the value of 'remoteHostname' is set by the user in a control as input 
    // and is therefore untrusted input and could contain errors. 
    // If we can't create a valid hostname, we notify the user in statusText 
    // about the incorrect input.

    String ^hostString = remoteHostname;

    try 
    {
        remoteHost = ref new Windows::Networking:Host(hostString);
        isHostnameValid = true;
    }
    catch (InvalidArgumentException ^ex)
    {
        statusText->Text = "You entered a bad hostname, please re-enter a valid hostname.";
        return;
    }

    isHostnameFromUser = true;


    // ... Continue with code to execute with a valid hostname.
```

[**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 네임스페이스에는 소켓을 사용할 때 오류를 처리하는 편리한 도우미 메서드와 열거형이 있습니다. 특정 네트워크 예외를 앱에서 다르게 처리하는 데 유용합니다.

[**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319), [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 또는 [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) 작업에서 오류가 발생하면 예외가 발생하게 됩니다. 예외의 원인은 **HRESULT** 값으로 표현되는 오류 값입니다. [**SocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701462) 메서드는 네트워크 오류를 소켓 작업에서 [**SocketErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh701457) 열거형 값으로 변환하는 데 사용됩니다. 대부분의 **SocketErrorStatus** 열거형 값은 기본 Windows 소켓 작업에서 반환한 오류에 해당합니다. 앱은 특정 **SocketErrorStatus** 열거형 값을 필터링하여 예외의 원인에 따라 앱 동작을 수정할 수 있습니다.

매개 변수 유효성 검사 오류의 경우 앱은 또한 예외에서 **HRESULT**를 사용하여 예외의 원인이 된 오류에 대한 더 자세한 정보를 알 수 있습니다. 가능한 **HRESULT** 값은 *Winerror.h* 헤더 파일에 나열되어 있습니다. 대부분의 매개 변수 유효성 검사 오류에서 반환되는 **HRESULT**는 **E\_INVALIDARG**입니다.

스트림 소켓 연결을 시도할 때 예외를 처리하기 위한 코드 추가

```cpp
using namespace Windows::Networking;
using namespace Windows::Networking::Sockets;

    
    // Define some more variables at the class level.

    bool isSocketConnected = false
    bool retrySocketConnect = false;

    // The number of times we have tried to connect the socket.
    unsigned int retryConnectCount = 0;

    // The maximum number of times to retry a connect operation.
    unsigned int maxRetryConnectCount = 5; 
    ///...

    // We pass in a valid remoteHost and serviceName parameter.
    // The hostname can contain a name or an IP address.
    // The servicename can contain a string or a TCP port number.

    StreamSocket ^ socket = ref new StreamSocket();
    SocketErrorStatus errorStatus; 
    HResult hr;

    // Save the socket, so any subsequent steps can use it.
    CoreApplication::Properties->Insert("clientSocket", socket);

    // Connect to the remote server. 
    create_task(socket->ConnectAsync(
            remoteHost,
            serviceName,
            SocketProtectionLevel::PlainSocket)).then([this] (task<void> previousTask)
    {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.get();

            isSocketConnected = true;
            // Mark the socket as connected. We do not really care about the value of the property, but the mere 
            // existence of  it means that we are connected.
            CoreApplication::Properties->Insert("connected", nullptr);
        }
        catch (Exception^ ex)
        {
            hr = ex.HResult;
            errorStatus = SocketStatus::GetStatus(hr); 
            if (errorStatus != Unknown)
            {
                                                                switch (errorStatus) 
                   {
                    case HostNotFound:
                        // If the hostname is from the user, this may indicate a bad input.
                        // Set a flag to ask the user to re-enter the hostname.
                        isHostnameValid = false;
                        return;
                        break;
                    case ConnectionRefused:
                        // The server might be temporarily busy.
                        retrySocketConnect = true;
                        return;
                        break; 
                    case NetworkIsUnreachable: 
                        // This could be a connectivity issue.
                        retrySocketConnect = true;
                        break;
                    case UnreachableHost: 
                        // This could be a connectivity issue.
                        retrySocketConnect = true;
                        break;
                    case NetworkIsDown: 
                        // This could be a connectivity issue.
                        retrySocketConnect = true;
                        break;
                    // Handle other errors. 
                    default: 
                        // The connection failed and no options are available.
                        // Try to use cached data if it is available. 
                        // You may want to tell the user that the connect failed.
                        break;
                }
                }
                else 
                {
                    // Received an Hresult that is not mapped to an enum.
                    // This could be a connectivity issue.
                    retrySocketConnect = true;
                }
            }
        });
    }

```

### <a name="exceptions-in-windowswebhttp"></a>Windows.Web.Http의 예외

전달된 문자열이 유효한 URI가 아닌 경우(URI에 허용되지 않는 문자 포함) [**Windows::Web::Http::HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)과(와) 함께 사용되는 [**Windows::Foundation::Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 클래스의 생성자가 예외를 일으킬 수 있습니다. C++에는 URI에 대한 문자열을 시도 및 구문 분석할 메서드가 없습니다. 앱이 **Windows::Foundation::Uri**에 대해 사용자 입력을 받으면 생성자는 try/catch 블록에 있게 됩니다. 예외가 발생하면 앱은 사용자에게 알리고 새 URI를 요청할 수 있습니다.

앱은 또한 URI의 스키마가 HTTP 또는 HTTPS인지 확인해야 합니다. [**Windows::Web::Http::HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)에서는 이 두 가지 스키마만 지원하기 때문입니다.

사용자가 입력한 URI의 문자열을 확인하기 위한 코드 추가

```cpp

    // Define some variables at the class level.
    Windows::Foundation::Uri^ resourceUri;

    bool isUriFromUser = false;
    bool isUriValid = false;

    ///...

    // If the value of 'inputUri' is set by the user in a control as input 
    // and is therefore untrusted input and could contain errors. 
    // If we can't create a valid hostname, we notify the user in statusText 
    // about the incorrect input.

    String ^uriString = inputUri;

    try 
    {
        isUriValid = false;
        resourceUri = ref new Windows::Foundation:Uri(uriString);

        if (resourceUri->SchemeName != "http" && resourceUri->SchemeName != "https")
        {
            statusText->Text = "Only 'http' and 'https' schemes supported. Please re-enter URI";
            return;
        }
        isUriValid = true;
    }
    catch (InvalidArgumentException ^ex)
    {
        statusText->Text = "You entered a bad URI, please re-enter Uri to continue.";
        return;
    }

    isUriFromUser = true;


    // ... Continue with code to execute with a valid URI.
```

[**Windows::Web::Http**](https://msdn.microsoft.com/library/windows/apps/windows.web.http.aspx) 네임스페이스에는 편의 기능이 부족합니다. 따라서 이 네임스페이스에서 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 및 다른 클래스를 사용하는 앱은 **HRESULT** 값을 사용해야 합니다.

C++를 사용하는 앱에서 [**Platform::Exception**](https://msdn.microsoft.com/library/windows/apps/hh755825.aspx)은 앱 실행 중 예외가 발생하는 경우의 오류를 나타냅니다. [**Platform::Exception::HResult**](https://msdn.microsoft.com/library/windows/apps/hh763371.aspx) 속성은 특정 예외에 할당된 **HRESULT**를 반환합니다. [**Platform::Exception::Message**](https://msdn.microsoft.com/library/windows/apps/hh763375.aspx) 속성은 **HRESULT** 값과 연결된 시스템 제공 문자열을 반환합니다. 가능한 **HRESULT** 값은 *Winerror.h* 헤더 파일에 나열되어 있습니다. 앱은 특정 **HRESULT** 값을 필터링하여 예외의 원인에 따라 앱 동작을 수정할 수 있습니다.

대부분의 매개 변수 유효성 검사 오류에서 반환되는 **HRESULT**는 **E\_INVALIDARG**입니다. 일부 잘못된 메서드 호출의 경우 반환되는 **HRESULT**는 **E\_ILLEGAL\_METHOD\_CALL**입니다.

HTTP 서버에 연결하기 위해 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)를 사용하려 할 때의 예외를 처리하기 위한 코드 추가

```cpp
using namespace Windows::Foundation;
using namespace Windows::Web::Http;
    
    // Define some more variables at the class level.

    bool isHttpClientConnected = false
    bool retryHttpClient = false;

    // The number of times we have tried to connect the socket
    unsigned int retryConnectCount = 0;

    // The maximum number of times to retry a connect operation.
    unsigned int maxRetryConnectCount = 5; 
    ///...

    // We pass in a valid resourceUri parameter.
    // The URI must contain a scheme and a name or an IP address.

    HttpClient ^ httpClient = ref new HttpClient();
    HResult hr;

    // Save the httpClient, so any subsequent steps can use it.
    CoreApplication::Properties->Insert("httpClient", httpClient);

    // Send a GET request to the HTTP server. 
    create_task(httpClient->GetAsync(resourceUri)).then([this] (task<void> previousTask)
    {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.get();

            isHttpClientConnected = true;
            // Mark the HttClient as connected. We do not really care about the value of the property, but the mere 
            // existence of  it means that we are connected.
            CoreApplication::Properties->Insert("connected", nullptr);
        }
        catch (Exception^ ex)
        {
            hr = ex.HResult;
                                                switch (errorStatus) 
               {
                case WININET_E_NAME_NOT_RESOLVED:
                    // If the Uri is from the user, this may indicate a bad input.
                    // Set a flag to ask user to re-enter the Uri.
                    isUriValid = false;
                    return;
                    break;
                case WININET_E_CANNOT_CONNECT:
                    // The server might be temporarily busy.
                    retryHttpClientConnect = true;
                    return;
                    break; 
                case WININET_E_CONNECTION_ABORTED: 
                    // This could be a connectivity issue.
                    retryHttpClientConnect = true;
                    break;
                case WININET_E_CONNECTION_RESET: 
                    // This could be a connectivity issue.
                    retryHttpClientConnect = true;
                    break;
                case INET_E_RESOURCE_NOT_FOUND: 
                    // The server cannot locate the resource specified in the uri.
                    // If the Uri is from user, this may indicate a bad input.
                    // Set a flag to ask the user to re-enter the Uri
                    isUriValid = false;
                    return;
                    break;
                // Handle other errors. 
                default: 
                    // The connection failed and no options are available.
                    // Try to use cached data if it is available. 
                    // You may want to tell the user that the connect failed.
                    break;
            }
            else 
            {
                // Received an Hresult that is not mapped to an enum.
                // This could be a connectivity issue.
                retrySocketConnect = true;
            }
        }
    });
    

```

## <a name="related-topics"></a>관련 항목


**다른 리소스**

* [데이터그램 소켓을 사용하여 연결](https://msdn.microsoft.com/library/windows/apps/xaml/jj635238)
* [스트림 소켓을 사용하여 네트워크 리소스에 연결](https://msdn.microsoft.com/library/windows/apps/xaml/jj150599)
* [네트워크 서비스에 연결](https://msdn.microsoft.com/library/windows/apps/xaml/hh452976)
* [웹 서비스에 연결](https://msdn.microsoft.com/library/windows/apps/xaml/hh761504)
* [네트워킹 기본 사항](https://msdn.microsoft.com/library/windows/apps/mt280233)
* [네트워크 격리 기능을 구성하는 방법](https://msdn.microsoft.com/library/windows/apps/hh770532)
* [루프백을 사용하도록 설정하고 네트워크 격리를 디버그하는 방법](https://msdn.microsoft.com/library/windows/apps/hh780593)

**참조**

* [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319)
* [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)
* [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)
* [**Windows::Web::Http**](https://msdn.microsoft.com/library/windows/apps/dn279692)
* [**Windows::Networking::Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960)

**샘플**

* [DatagramSocket 샘플](https://go.microsoft.com/fwlink/p/?LinkID=243037)
* [HttpClient 샘플]( https://go.microsoft.com/fwlink/p/?linkid=242550)
* [근접 연결 샘플](https://go.microsoft.com/fwlink/p/?linkid=245082)
* [StreamSocket 샘플](https://go.microsoft.com/fwlink/p/?linkid=243037)
