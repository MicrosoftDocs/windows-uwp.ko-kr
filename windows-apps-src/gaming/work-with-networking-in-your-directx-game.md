---
title: 게임을 위한 네트워킹
description: DirectX 게임에 네트워킹 기능을 개발 하 고 통합 하는 방법에 대해 알아봅니다.
ms.assetid: 212eee15-045c-8ba1-e274-4532b2120c55
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 네트워킹, directx
ms.localizationpriority: medium
ms.openlocfilehash: 2e693016fa6b87f231c1cbbfac4c2e55d44623c9
ms.sourcegitcommit: 2571af6bf781a464a4beb5f1aca84ae7c850f8f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82606372"
---
# <a name="networking-for-games"></a>게임을 위한 네트워킹



DirectX 게임에 네트워킹 기능을 개발 하 고 통합 하는 방법에 대해 알아봅니다.

## <a name="concepts-at-a-glance"></a>한눈에 개념 보기


DirectX 게임에서 다양 한 네트워킹 기능을 사용할 수 있습니다 .이는 여러 플레이어 게임을 대규모 하는 간단한 독립 실행형 게임 인지 여부입니다. 네트워킹을 가장 간단 하 게 사용 하는 것은 중앙 네트워크 서버에 사용자 이름과 게임 점수를 저장 하는 것입니다.

네트워킹 Api는 인프라 (클라이언트-서버 또는 인터넷 피어 투 피어) 모델과 임시 (로컬 피어 투 피어) 게임을 사용 하는 다중 플레이어 게임에 필요 합니다. 서버 기반 다중 플레이어 게임의 경우 중앙 게임 서버는 일반적으로 대부분의 게임 작업을 처리 하 고 클라이언트 게임 앱은 입력에 사용 되 고 그래픽을 표시 하 고 오디오를 재생 하는 등의 기능을 수행 합니다. 네트워크 전송 속도와 대기 시간은 만족 스러운 게임 환경에서 매우 중요 합니다.

피어 투 피어 게임의 경우 각 플레이어의 앱이 입력 및 그래픽을 처리 합니다. 대부분의 경우 게임 플레이어는 네트워크 대기 시간이 더 낮아야 하지만 여전히 문제가 될 수 있도록 근접 한 위치에 있습니다. 피어를 검색 하 고 연결을 설정 하는 것이 문제가 될 수 있습니다.

단일 플레이어 게임의 경우 중앙 웹 서버 또는 서비스를 사용 하 여 사용자 이름, 게임 점수 및 기타 기타 정보를 저장 하는 경우가 많습니다. 이러한 게임에서는 게임 작업에 직접적인 영향을 주지 않으므로 네트워킹 전송 속도와 대기 시간이 그다지 중요 하지 않습니다.

네트워크 상태는 언제 든 지 변경 될 수 있으므로 네트워킹 Api를 사용 하는 게임에서 발생할 수 있는 네트워크 예외를 처리 해야 합니다. 네트워크 예외 처리에 대한 자세한 내용은 [네트워킹 기본 사항](https://docs.microsoft.com/windows/uwp/networking/networking-basics)을 참조하세요.

방화벽과 웹 프록시는 일반적 이며 네트워킹 기능을 사용 하는 기능에 영향을 줄 수 있습니다. 방화벽 및 프록시를 올바르게 처리 하기 위해 네트워킹을 사용 하는 게임을 준비 해야 합니다.

모바일 장치의 경우 사용 가능한 네트워크 리소스를 모니터링 하 고 로밍 또는 데이터 비용이 중요할 수 있는 요금제 네트워크에서 적절 하 게 동작 해야 합니다.

네트워크 격리는 Windows에서 사용 하는 앱 보안 모델의 일부입니다. Windows에서 네트워크 경계를 검색 하 고 네트워크 격리에 대 한 네트워크 액세스 제한을 적용 합니다. 앱은 네트워크 액세스 범위를 정의 하기 위해 네트워크 격리 기능을 선언 해야 합니다. 이러한 기능을 선언 하지 않으면 앱에서 네트워크 리소스에 액세스할 수 없습니다. Windows에서 앱에 대 한 네트워크 격리를 적용 하는 방법에 대 한 자세한 내용은 [네트워크 격리 기능을 구성 하는 방법](https://docs.microsoft.com/previous-versions/windows/apps/hh770532(v=win.10))을 참조 하세요.

## <a name="design-considerations"></a>디자인 고려 사항


DirectX 게임에서 다양 한 네트워킹 Api를 사용할 수 있습니다. 따라서 올바른 API를 선택 하는 것이 중요 합니다. Windows는 앱이 인터넷 또는 개인 네트워크를 통해 다른 컴퓨터와 장치와 통신 하는 데 사용할 수 있는 다양 한 네트워킹 Api를 지원 합니다. 첫 번째 단계는 앱에 필요한 네트워킹 기능을 파악 하는 것입니다.

가장 인기 있는 게임의 네트워크 Api는 다음과 같습니다.

-   TCP 및 소켓-안정적인 연결을 제공 합니다. 보안이 필요 하지 않은 게임 작업에 TCP를 사용 합니다. TCP를 사용 하면 서버를 쉽게 확장할 수 있으므로 인프라 (클라이언트 서버 또는 인터넷 피어 투 피어) 모델을 사용 하는 게임에서 일반적으로 사용 됩니다. TCP는 Wi-fi Direct 및 Bluetooth를 통해 임시 (로컬 피어 투 피어) 게임에서 사용할 수도 있습니다. TCP는 게임 개체 이동, 문자 상호 작용, 텍스트 채팅 및 기타 작업에 일반적으로 사용 됩니다. [**Streamsocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) 클래스는 Microsoft Store 게임에서 사용할 수 있는 TCP 소켓을 제공 합니다. **Streamsocket** 클래스는 [**Windows:: 네트워킹:: Sockets**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets) 네임 스페이스의 관련 클래스와 함께 사용 됩니다.
-   SSL을 사용 하는 TCP 및 소켓-도청을 방지 하는 안정적인 연결을 제공 합니다. 보안이 필요한 게임 작업에 SSL과 함께 TCP 연결을 사용 합니다. SSL의 암호화 및 오버 헤드로 인해 대기 시간 및 성능에 비용이 추가 되므로 보안이 필요한 경우에만 사용 됩니다. TCP with SSL은 일반적으로 로그인, 구매 및 거래 자산, 게임 문자 생성 및 관리에 사용 됩니다. [**Streamsocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) 클래스는 SSL을 지 원하는 TCP 소켓을 제공 합니다.
-   UDP 및 소켓-오버 헤드가 낮은 불안정 한 네트워크 전송을 제공 합니다. UDP는 짧은 대기 시간을 필요로 하 고 일부 패킷 손실을 허용할 수 있는 게임 작업에 사용 됩니다. 게임, 촬영 및 tracers, 네트워크 오디오, 음성 채팅 등을 방지 하는 데 주로 사용 됩니다. [**DatagramSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket) 클래스는 Microsoft Store 게임에서 사용할 수 있는 UDP 소켓을 제공 합니다. **DatagramSocket** 클래스는 [**Windows:: 네트워킹:: Sockets**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets) 네임 스페이스의 관련 클래스와 함께 사용 됩니다.
-   HTTP 클라이언트-HTTP 서버에 대 한 신뢰할 수 있는 연결을 제공 합니다. 가장 일반적인 네트워킹 시나리오는 웹 사이트에 액세스 하 여 정보를 검색 하거나 저장 하는 것입니다. 간단한 예로 사용자 정보 및 게임 점수를 저장 하는 웹 사이트를 사용 하는 게임이 있습니다. SSL과 함께 보안을 사용 하는 경우 HTTP 클라이언트를 로그인, 구매, 거래 자산, 게임 문자 생성 및 관리에 사용할 수 있습니다. [**Httpclient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 클래스는 Microsoft Store 게임에서 사용할 최신 HTTP 클라이언트 API를 제공 합니다. **Httpclient** 클래스는 [**Windows:: Web:: Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) 네임 스페이스의 관련 클래스와 함께 사용 됩니다.

## <a name="handling-network-exceptions-in-your-directx-game"></a>DirectX 게임에서 네트워크 예외 처리


DirectX 게임에서 네트워크 예외가 발생 하는 경우이는 심각한 문제나 실패를 나타냅니다. 네트워킹 Api를 사용 하는 경우 여러 가지 이유로 예외가 발생할 수 있습니다. 네트워크 연결이 변경 되거나 원격 호스트나 서버와 관련 된 다른 네트워킹 문제가 발생 하는 경우가 종종 있습니다.

네트워킹 Api를 사용할 때 예외가 발생 하는 원인은 다음과 같습니다.

-   호스트 이름 또는 URI에 대 한 사용자 입력에 오류가 있으며 잘못 되었습니다.
-   호스트 이름 또는 URi를 조회할 때 이름 확인 실패
-   네트워크 연결의 손실 또는 변경
-   소켓이 나 HTTP 클라이언트 Api를 사용 하 여 네트워크 연결에 실패 했습니다.
-   네트워크 서버 또는 원격 끝점 오류입니다.
-   기타 네트워킹 오류입니다.

네트워크 오류의 예외 (예: 연결 손실 또는 변경, 연결 실패 및 서버 오류)는 언제 든 지 발생할 수 있습니다. 이러한 오류가 발생 하면 예외가 throw 됩니다. 응용 프로그램에서 예외를 처리 하지 않으면 런타임에 의해 전체 앱이 종료 될 수 있습니다.

대부분의 비동기 네트워크 메서드를 호출할 때 예외를 처리 하는 코드를 작성 해야 합니다. 경우에 따라 예외가 발생 하면 문제를 해결 하는 방법으로 네트워크 메서드를 다시 시도할 수 있습니다. 다른 경우에는 이전에 캐시 된 데이터를 사용 하 여 네트워크 연결 없이 앱이 계속 진행 되도록 계획 해야 할 수도 있습니다.

UWP (유니버설 Windows 플랫폼) 앱은 일반적으로 단일 예외를 throw 합니다. 예외 처리기는 예외 원인에 대 한 자세한 정보를 검색 하 여 오류를 보다 잘 이해 하 고 적절 한 결정을 내릴 수 있습니다.

UWP 앱 인 DirectX 게임에서 예외가 발생 하면 오류의 원인에 대 한 **HRESULT** 값을 검색할 수 있습니다. *Winerror.h* 포함 파일에는 네트워크 오류가 포함 된 가능한 **HRESULT** 값의 많은 목록이 포함 되어 있습니다.

네트워킹 Api는 예외의 원인에 대 한 자세한 정보를 검색 하는 다양 한 방법을 지원 합니다.

-   예외를 발생 시킨 오류의 **HRESULT** 값을 검색 하는 메서드입니다. 잠재적 **HRESULT** 값의 가능한 목록은 크고 지정 되지 않습니다. 네트워킹 Api를 사용 하는 경우 **HRESULT** 값을 검색할 수 있습니다.
-   **HRESULT** 값을 열거형 값으로 변환 하는 도우미 메서드입니다. 가능한 열거형 값 목록이 지정 되 고 상대적으로 작습니다. 도우미 메서드는 [**Windows:: 네트워킹:: Sockets**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets)의 소켓 클래스에 사용할 수 있습니다.

### <a name="exceptions-in-windowsnetworkingsockets"></a>Windows의 예외

소켓에 사용 되는 [**호스트**](https://docs.microsoft.com/uwp/api/Windows.Networking.HostName) 이름 클래스의 생성자는 전달 된 문자열이 유효한 호스트 이름이 아닌 경우 예외를 throw 할 수 있습니다 (호스트 이름에 허용 되지 않는 문자 포함). 앱에서 게임에 대 한 피어 연결에 대 한 **호스트 이름** 에 대 한 사용자의 입력을 가져오는 경우 생성자는 try/catch 블록에 있어야 합니다. 예외가 throw 되 면 앱에서 사용자에 게 알리고 새 호스트 이름을 요청할 수 있습니다.

사용자의 호스트 이름에 대 한 문자열의 유효성을 검사 하는 코드를 추가 합니다.

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

[**Windows. 네트워킹과**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets) 네임 스페이스에는 소켓을 사용할 때 오류를 처리 하기 위한 편리한 도우미 메서드 및 열거가 있습니다. 이는 앱에서 특정 네트워크 예외를 다르게 처리 하는 데 유용할 수 있습니다.

[**DatagramSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket), [**Streamsocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket)또는 [**StreamSocketListener**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocketListener) 작업에서 오류가 발생 하면 예외가 throw 됩니다. 예외의 원인은 **HRESULT** 값으로 표시 되는 오류 값입니다. [**GetStatus**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.socketerror.getstatus) 메서드는 소켓 작업에서 [**SocketErrorStatus**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.SocketErrorStatus) 열거형 값으로 네트워크 오류를 변환 하는 데 사용 됩니다. 대부분의 **SocketErrorStatus** 열거형 값은 네이티브 Windows 소켓 작업에서 반환 되는 오류에 해당 합니다. 앱은 특정 **SocketErrorStatus** 열거 값을 필터링 하 여 예외의 원인에 따라 앱 동작을 수정할 수 있습니다.

매개 변수 유효성 검사 오류의 경우 응용 프로그램은 예외의 **HRESULT** 를 사용 하 여 예외를 발생 시킨 오류에 대 한 자세한 정보를 확인할 수도 있습니다. 가능한 **HRESULT** 값은 *winerror.h* 헤더 파일에 나와 있습니다. 대부분의 매개 변수 유효성 검사 오류에 대해 반환 되는 **HRESULT** 는 **\_E invalidarg**입니다.

스트림 소켓 연결을 시도할 때 예외를 처리 하는 코드를 추가 합니다.

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

### <a name="exceptions-in-windowswebhttp"></a>Windows의 예외

Windows:: [**Web:: Http:: HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 에 사용 되는 [**Windows:: Foundation:: uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri) 클래스의 생성자는 전달 된 문자열이 유효한 uri가 아닌 경우 (Uri에서 허용 되지 않는 문자 포함) 예외를 throw 할 수 있습니다. C + +에서는 URI에 대 한 문자열을 시도 하 고 구문 분석할 수 있는 메서드가 없습니다. 앱이 **Windows:: Foundation:: Uri**에 대 한 사용자의 입력을 가져오는 경우 생성자는 try/catch 블록에 있어야 합니다. 예외가 throw 되 면 앱에서 사용자에 게 알리고 새 URI를 요청할 수 있습니다.

또한 응용 프로그램은 URI의 체계가 HTTP 또는 HTTPS 인지 확인 해야 합니다 .이는 [**Windows:: Web:: HTTP:: HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)에서 지원 되는 유일한 체계 이기 때문입니다.

사용자의 URI에 대 한 문자열의 유효성을 검사 하는 코드를 추가 합니다.

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

[**Windows:: Web:: Http**](https://docs.microsoft.com/uwp/api/windows.web.http) 네임 스페이스에는 편리한 함수가 없습니다. 따라서 [**Httpclient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 및이 네임 스페이스의 다른 클래스를 사용 하는 앱은 **HRESULT** 값을 사용 해야 합니다.

C + +를 사용 하는 앱에서 [**Platform:: exception**](https://docs.microsoft.com/cpp/cppcx/platform-exception-class) 은 예외가 발생할 때 앱을 실행 하는 동안 오류를 나타냅니다. [**Platform:: Exception:: HResult**](https://docs.microsoft.com/cpp/cppcx/platform-exception-class#hresult) 속성은 특정 예외에 할당 된 **HResult** 를 반환 합니다. [**Platform:: Exception:: Message**](https://docs.microsoft.com/cpp/cppcx/platform-exception-class#message) 속성은 **HRESULT** 값과 연결 된 시스템 제공 문자열을 반환 합니다. 가능한 **HRESULT** 값은 *winerror.h* 헤더 파일에 나와 있습니다. 앱은 예외의 원인에 따라 앱 동작을 수정 하는 특정 **HRESULT** 값을 기준으로 필터링 할 수 있습니다.

대부분의 매개 변수 유효성 검사 오류에 대해 반환 되는 **HRESULT** 는 **\_E invalidarg**입니다. 일부 잘못된 메서드 호출의 경우 반환되는 **HRESULT**는 **E\_ILLEGAL\_METHOD\_CALL**입니다.

[**Httpclient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 를 사용 하 여 HTTP 서버에 연결 하려고 할 때 예외를 처리 하는 코드를 추가 합니다.

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


**기타 리소스**

* [데이터 그램 소켓을 사용 하 여 연결](https://docs.microsoft.com/previous-versions/windows/apps/jj635238(v=win.10))
* [스트림 소켓을 사용 하 여 네트워크 리소스에 연결](https://docs.microsoft.com/previous-versions/windows/apps/jj150599(v=win.10))
* [네트워크 서비스에 연결](https://docs.microsoft.com/previous-versions/windows/apps/hh452976(v=win.10))
* [웹 서비스에 연결](https://docs.microsoft.com/previous-versions/windows/apps/hh761504(v=win.10))
* [네트워킹 기본 사항](https://docs.microsoft.com/windows/uwp/networking/networking-basics)
* [네트워크 격리 기능을 구성 하는 방법](https://docs.microsoft.com/previous-versions/windows/apps/hh770532(v=win.10))
* [루프백 및 디버그 네트워크 격리를 사용 하도록 설정 하는 방법](https://docs.microsoft.com/previous-versions/windows/apps/hh780593(v=win.10))

**참조**

* [**DatagramSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket)
* [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)
* [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket)
* [**Windows:: Web:: Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http)
* [**Windows:: 네트워킹:: 소켓**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets)

**샘플**

* [DatagramSocket 샘플](https://code.msdn.microsoft.com/windowsapps/StreamSocket-Sample-8c573931)
* [HttpClient 샘플]( https://code.msdn.microsoft.com/windowsapps/HttpClient-sample-55700664)
* [근접 샘플](https://code.msdn.microsoft.com/windowsapps/Proximity-Sample-88129731)
* [StreamSocket 샘플](https://code.msdn.microsoft.com/windowsapps/StreamSocket-Sample-8c573931)
