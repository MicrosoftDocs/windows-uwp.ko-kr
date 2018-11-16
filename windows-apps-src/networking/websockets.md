---
author: stevewhims
description: WebSockets는 HTTP(S)를 사용하고 UTF-8 및 이진 메시지를 둘 다 지원하여 웹을 통해 클라이언트와 서버 간의 신속하고 보안이 유지된 양방향 통신을 위한 메커니즘을 제공합니다.
title: WebSockets
ms.assetid: EAA9CB3E-6A3A-4C13-9636-CCD3DE46E7E2
ms.author: stwhi
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, uwp, 네트워킹, websocket, messagewebsocket, streamwebsocket
ms.localizationpriority: medium
ms.openlocfilehash: eaba8d7b87d9e2762d13bd86629ee4a996e2d5e8
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7103741"
---
# <a name="websockets"></a>WebSockets
WebSockets는 HTTP(S)를 사용하고 UTF-8 및 이진 메시지를 둘 다 지원하여 웹을 통해 클라이언트와 서버 간의 신속하고 보안이 유지된 양방향 통신을 위한 메커니즘을 제공합니다.

[WebSocket 프로토콜](http://tools.ietf.org/html/rfc6455)에서는 데이터가 전이중 단일 소켓 연결을 통해 즉시 전송되므로 두 끝점에서 실시간으로 메시지를 보내고 받을 수 있습니다. WebSockets는 멀티 플레이어 게임(실시간 및 턴 기반 모두), 즉각적인 소셜 네트워크 알림, 주식 또는 날씨 정보의 최신 표시, 보안 및 고속 데이터 전송이 필요한 기타 앱에 적합합니다.

WebSocket 연결을 설정하려면 특정 HTTP 기반 핸드셰이크가 클라이언트와 서버 간에 교환됩니다. 성공하면 응용 프로그램 계층 프로토콜은 이전에 설정한 TCP 연결을 사용하여 HTTP에서 WebSockets로 "업그레이드"됩니다. 이렇게 되면 HTTP는 완전히 제외됩니다. 데이터는 WebSocket 연결을 종료할 때까지 양 끝점에 의해 WebSocket 프로토콜을 사용하여 송수신될 수 있습니다.

**참고** 서버가 WebSocket 프로토콜도 사용하지 않는 경우 클라이언트는 WebSockets를 사용하여 데이터를 전송할 수 없습니다. 서버가 WebSockets를 지원하지 않는 경우 데이터를 전송하는 다른 방법을 사용해야 합니다.

UWP(유니버설 Windows 플랫폼)는 WebSockets의 클라이언트 및 서버 사용에 대한 지원을 제공합니다. [**Windows.Networking.Sockets**](/uwp/api/windows.networking.sockets) 네임스페이스는 [**MessageWebSocket**](/uwp/api/windows.networking.sockets.messagewebsocket) 및 [**StreamWebSocket**](/uwp/api/windows.networking.sockets.streamwebsocket) 클라이언트에서 사용할 수 있도록 두 가지 WebSocket 클래스를 정의합니다. 여기서는 이러한 두 WebSocket 클래스를 비교합니다.

| [MessageWebSocket](/uwp/api/windows.networking.sockets.messagewebsocket) | [StreamWebSocket](/uwp/api/windows.networking.sockets.streamwebsocket) |
| - | - |
| 전체 WebSocket 메시지는 단일 작업에서 읽고 써집니다. | 각 읽기 작업으로 메시지의 섹션을 읽을 수 있습니다. |
| 메시지가 매우 크지 않은 경우에 적합합니다. | 사진이나 동영상 등 매우 큰 파일이 전송되는 경우에 적합합니다. |
| UTF-8 메시지와 이진 메시지를 모두 지원합니다. | 이진 메시지만 지원합니다. |
| [UDP 또는 데이터그램 소켓](sockets.md#build-a-basic-udp-socket-client-and-server)과 유사하지만(잦은 작은 메시지용인 면에서) TCP의 안정성, 패킷 순서 보장 및 정체 제어를 지원합니다. | [TCP 또는 스트림 소켓](sockets.md#build-a-basic-tcp-socket-client-and-server)과 유사합니다. |

## <a name="secure-your-connection-with-tlsssl"></a>TLS/SSL로 연결 보안
대부분의 경우 보안 WebSocket 연결을 사용하여 전송 및 수신되는 데이터를 암호화할 수 있습니다. 이 경우 방화벽 또는 프록시 등의 여러 중개자가 암호화되지 않은 WebSocket 연결을 거부하므로 연결이 성공할 가능성도 높아집니다. [WebSocket 프로토콜](https://tools.ietf.org/html/rfc6455#section-3)은 다음 두 가지 URI 스키마를 정의합니다.

| URI 스키마 | 용도 |
| - | - |
| wss: | 암호화해야 하는 보안 연결에 사용됩니다. |
| ws: | 암호화되지 않은 연결에 사용됩니다. |

WebSocket 연결을 암호화하려면 `wss:` URI 스키마를 사용합니다. 예를 들면 다음과 같습니다.

```csharp
protected override async void OnNavigatedTo(NavigationEventArgs e)
{
    var webSocket = new Windows.Networking.Sockets.MessageWebSocket();
    await webSocket.ConnectAsync(new Uri("wss://www.contoso.com/mywebservice"));
}
```

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
#include <sstream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml::Navigation;
...
IAsyncAction OnNavigatedTo(NavigationEventArgs /* e */)
{
    Windows::Networking::Sockets::MessageWebSocket webSocket;
    co_await webSocket.ConnectAsync(Uri{ L"wss://www.contoso.com/mywebservice" });
}
```

## <a name="use-messagewebsocket-to-connect"></a>MessageWebSocket을 사용하여 연결
[**MessageWebSocket**](/uwp/api/windows.networking.sockets.messagewebsocket)을 사용하여 전체 WebSocket 메시지를 단일 작업에서 읽고 쓰도록 할 수 있습니다. 따라서 메시지가 매우 크지 않은 경우에 적합합니다. 클래스는 UTF-8 메시지와 이진 메시지를 모두 지원합니다.

아래의 예제 코드는 전송된 모든 메시지를 보낸 사람에게 다시 전달하기만 하는 서비스인 WebSocket.org 에코 서버를 사용합니다.

```csharp
private Windows.Networking.Sockets.MessageWebSocket messageWebSocket;

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    this.messageWebSocket = new Windows.Networking.Sockets.MessageWebSocket();

    // In this example, we send/receive a string, so we need to set the MessageType to Utf8.
    this.messageWebSocket.Control.MessageType = Windows.Networking.Sockets.SocketMessageType.Utf8;

    this.messageWebSocket.MessageReceived += WebSocket_MessageReceived;
    this.messageWebSocket.Closed += WebSocket_Closed;

    try
    {
        Task connectTask = this.messageWebSocket.ConnectAsync(new Uri("wss://echo.websocket.org")).AsTask();
        connectTask.ContinueWith(_ => this.SendMessageUsingMessageWebSocketAsync("Hello, World!"));
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add additional code here to handle exceptions.
    }
}

private async Task SendMessageUsingMessageWebSocketAsync(string message)
{
    using (var dataWriter = new DataWriter(this.messageWebSocket.OutputStream))
    {
        dataWriter.WriteString(message);
        await dataWriter.StoreAsync();
        dataWriter.DetachStream();
    }
    Debug.WriteLine("Sending message using MessageWebSocket: " + message);
}

private void WebSocket_MessageReceived(Windows.Networking.Sockets.MessageWebSocket sender, Windows.Networking.Sockets.MessageWebSocketMessageReceivedEventArgs args)
{
    try
    {
        using (DataReader dataReader = args.GetDataReader())
        {
            dataReader.UnicodeEncoding = Windows.Storage.Streams.UnicodeEncoding.Utf8;
            string message = dataReader.ReadString(dataReader.UnconsumedBufferLength);
            Debug.WriteLine("Message received from MessageWebSocket: " + message);
            this.messageWebSocket.Dispose();
        }
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add additional code here to handle exceptions.
    }
}

private void WebSocket_Closed(Windows.Networking.Sockets.IWebSocket sender, Windows.Networking.Sockets.WebSocketClosedEventArgs args)
{
    Debug.WriteLine("WebSocket_Closed; Code: " + args.Code + ", Reason: \"" + args.Reason + "\"");
    // Add additional code here to handle the WebSocket being closed.
}
```

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
#include <sstream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::MessageWebSocket m_messageWebSocket;
    winrt::event_token m_messageReceivedEventToken;
    winrt::event_token m_closedEventToken;

public:
    IAsyncAction OnNavigatedTo(NavigationEventArgs /* e */)
    {
        // In this example, we send/receive a string, so we need to set the MessageType to Utf8.
        m_messageWebSocket.Control().MessageType(Windows::Networking::Sockets::SocketMessageType::Utf8);

        m_messageReceivedEventToken = m_messageWebSocket.MessageReceived({ this, &MessageWebSocketPage::OnWebSocketMessageReceived });
        m_closedEventToken = m_messageWebSocket.Closed({ this, &MessageWebSocketPage::OnWebSocketClosed });

        try
        {
            co_await m_messageWebSocket.ConnectAsync(Uri{ L"wss://echo.websocket.org" });
            SendMessageUsingMessageWebSocketAsync(L"Hello, World!");
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }

private:
    IAsyncAction SendMessageUsingMessageWebSocketAsync(std::wstring message)
    {
        DataWriter dataWriter{ m_messageWebSocket.OutputStream() };
        dataWriter.WriteString(message);

        co_await dataWriter.StoreAsync();
        dataWriter.DetachStream();
        std::wstringstream wstringstream;
        wstringstream << L"Sending message using MessageWebSocket: " << message.c_str() << std::endl;
        ::OutputDebugString(wstringstream.str().c_str());
    }

    void OnWebSocketMessageReceived(Windows::Networking::Sockets::MessageWebSocket const& /* sender */, Windows::Networking::Sockets::MessageWebSocketMessageReceivedEventArgs const& args)
    {
        try
        {
            DataReader dataReader{ args.GetDataReader() };

            dataReader.UnicodeEncoding(Windows::Storage::Streams::UnicodeEncoding::Utf8);
            auto message = dataReader.ReadString(dataReader.UnconsumedBufferLength());
            std::wstringstream wstringstream;
            wstringstream << L"Message received from MessageWebSocket: " << message.c_str() << std::endl;
            ::OutputDebugString(wstringstream.str().c_str());
            m_messageWebSocket.Close(1000, L"");
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }

    void OnWebSocketClosed(Windows::Networking::Sockets::IWebSocket const& /* sender */, Windows::Networking::Sockets::WebSocketClosedEventArgs const& args)
    {
        std::wstringstream wstringstream;
        wstringstream << L"WebSocket_Closed; Code: " << args.Code() << ", Reason: \"" << args.Reason().c_str() << "\"" << std::endl;
        ::OutputDebugString(wstringstream.str().c_str());
        // Add additional code here to handle the WebSocket being closed.
    }
```

```cpp
#include <ppltasks.h>
#include <sstream>
...
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::MessageWebSocket^ messageWebSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->messageWebSocket = ref new Windows::Networking::Sockets::MessageWebSocket();

        // In this example, we send/receive a string, so we need to set the MessageType to Utf8.
        this->messageWebSocket->Control->MessageType = Windows::Networking::Sockets::SocketMessageType::Utf8;

        this->messageWebSocket->MessageReceived += ref new TypedEventHandler<Windows::Networking::Sockets::MessageWebSocket^, Windows::Networking::Sockets::MessageWebSocketMessageReceivedEventArgs^>(this, &MessageWebSocketPage::WebSocket_MessageReceived);
        this->messageWebSocket->Closed += ref new TypedEventHandler<Windows::Networking::Sockets::IWebSocket^, Windows::Networking::Sockets::WebSocketClosedEventArgs^>(this, &MessageWebSocketPage::WebSocket_Closed);

        try
        {
            auto connectTask = Concurrency::create_task(this->messageWebSocket->ConnectAsync(ref new Uri(L"wss://echo.websocket.org")));
            connectTask.then([this] { this->SendMessageUsingMessageWebSocketAsync(L"Hello, World!"); });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }

private:
    void SendMessageUsingMessageWebSocketAsync(Platform::String^ message)
    {
        auto dataWriter = ref new DataWriter(this->messageWebSocket->OutputStream);
        dataWriter->WriteString(message);

        Concurrency::create_task(dataWriter->StoreAsync()).then(
            [=](unsigned int)
        {
            dataWriter->DetachStream();
            std::wstringstream wstringstream;
            wstringstream << L"Sending message using MessageWebSocket: " << message->Data() << std::endl;
            ::OutputDebugString(wstringstream.str().c_str());
        });
    }

    void WebSocket_MessageReceived(Windows::Networking::Sockets::MessageWebSocket^ sender, Windows::Networking::Sockets::MessageWebSocketMessageReceivedEventArgs^ args)
    {
        try
        {
            DataReader^ dataReader = args->GetDataReader();

            dataReader->UnicodeEncoding = Windows::Storage::Streams::UnicodeEncoding::Utf8;
            Platform::String^ message = dataReader->ReadString(dataReader->UnconsumedBufferLength);
            std::wstringstream wstringstream;
            wstringstream << L"Message received from MessageWebSocket: " << message->Data() << std::endl;
            ::OutputDebugString(wstringstream.str().c_str());
            this->messageWebSocket->Close(1000, L"");
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }

    void WebSocket_Closed(Windows::Networking::Sockets::IWebSocket^ sender, Windows::Networking::Sockets::WebSocketClosedEventArgs^ args)
    {
        std::wstringstream wstringstream;
        wstringstream << L"WebSocket_Closed; Code: " << args->Code << ", Reason: \"" << args->Reason->Data() << "\"" << std::endl;
        ::OutputDebugString(wstringstream.str().c_str());
        // Add additional code here to handle the WebSocket being closed.
    }
```

### <a name="handle-the-messagewebsocketmessagereceived-and-messagewebsocketclosed-events"></a>MessageWebSocket.MessageReceived 및 MessageWebSocket.Closed 이벤트 처리
위 예제에서 나타난 것 같이 연결을 설정하고 **MessageWebSocket**으로 데이터를 보내기 전에 [**MessageWebSocket.MessageReceived**](/uwp/api/windows.networking.sockets.messagewebsocket.MessageReceived) 및 [**MessageWebSocket.Closed**](/uwp/api/windows.networking.sockets.messagewebsocket.Closed) 이벤트를 구독해야 합니다.
 
데이터를 받을 때 **MessageReceived**가 발생합니다. 데이터는 [**MessageWebSocketMessageReceivedEventArgs**](/uwp/api/windows.networking.sockets.messagewebsocketmessagereceivedeventargs)를 통해 액세스할 수 있습니다. 클라이언트 또는 서버가 소켓을 종료할 때 **Closed**가 발생합니다.
 
### <a name="send-data-on-a-messagewebsocket"></a>MessageWebSocket에서 데이터 보내기
연결이 설정되면 서버에 데이터를 보낼 수 있습니다. 데이터를 쓰기 위해 [**MessageWebSocket.OutputStream**](https://docs.microsoft.com/en-us/uwp/api/Windows.Networking.Sockets.MessageWebSocket.OutputStream) 속성과 [**DataWriter**](/uwp/api/windows.storage.streams.datawriter)를 사용하여 이 작업을 수행할 수 있습니다. 

**참고** **DataWriter**는 출력 스트림의 소유권을 갖습니다. **DataWriter**가 범위 이외에 있을 때 출력 스트림이 연결되어 있으면 **DataWriter**는 출력 스트림의 할당을 취소합니다. 그 이후 출력 스트림을 사용하려는 시도가 HRESULT 값 0x80000013과 함께 실패합니다. 하지만 [**DataWriter.DetachStream**](/uwp/api/windows.storage.streams.datawriter.DetachStream)을 호출하여 **DataWriter**에서 출력 스트림을 분리하고 스트림의 소유권을 **MessageWebSocket**으로 반환할 수 있습니다.

## <a name="use-streamwebsocket-to-connect"></a>StreamWebSocket을 사용하여 연결
[**StreamWebSocket**](/uwp/api/windows.networking.sockets.streamwebsocket)을 사용하여 각 읽기 작업으로 메시지의 섹션을 읽을 수 있습니다. 따라서 사진이나 동영상 등 매우 큰 파일이 전송되는 경우에 적합합니다. 클래스는 이진 메시지만 지원합니다.

아래의 예제 코드는 전송된 모든 메시지를 보낸 사람에게 다시 전달하기만 하는 서비스인 WebSocket.org 에코 서버를 사용합니다.

```csharp
private Windows.Networking.Sockets.StreamWebSocket streamWebSocket;

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    this.streamWebSocket = new Windows.Networking.Sockets.StreamWebSocket();

    this.streamWebSocket.Closed += WebSocket_Closed;

    try
    {
        Task connectTask = this.streamWebSocket.ConnectAsync(new Uri("wss://echo.websocket.org")).AsTask();

        connectTask.ContinueWith(_ =>
        {
            Task.Run(() => this.ReceiveMessageUsingStreamWebSocket());
            Task.Run(() => this.SendMessageUsingStreamWebSocket(new byte[] { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09 }));
        });
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add code here to handle exceptions.
    }
}

private async void ReceiveMessageUsingStreamWebSocket()
{
    try
    {
        using (var dataReader = new DataReader(this.streamWebSocket.InputStream))
        {
            dataReader.InputStreamOptions = InputStreamOptions.Partial;
            await dataReader.LoadAsync(256);
            byte[] message = new byte[dataReader.UnconsumedBufferLength];
            dataReader.ReadBytes(message);
            Debug.WriteLine("Data received from StreamWebSocket: " + message.Length + " bytes");
        }
        this.streamWebSocket.Dispose();
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add code here to handle exceptions.
    }
}

private async void SendMessageUsingStreamWebSocket(byte[] message)
{
    try
    {
        using (var dataWriter = new DataWriter(this.streamWebSocket.OutputStream))
        {
            dataWriter.WriteBytes(message);
            await dataWriter.StoreAsync();
            dataWriter.DetachStream();
        }
        Debug.WriteLine("Sending data using StreamWebSocket: " + message.Length.ToString() + " bytes");
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add code here to handle exceptions.
    }
}

private void WebSocket_Closed(Windows.Networking.Sockets.IWebSocket sender, Windows.Networking.Sockets.WebSocketClosedEventArgs args)
{
    Debug.WriteLine("WebSocket_Closed; Code: " + args.Code + ", Reason: \"" + args.Reason + "\"");
    // Add additional code here to handle the WebSocket being closed.
}
```

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
#include <sstream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::StreamWebSocket m_streamWebSocket;
    winrt::event_token m_closedEventToken;

public:
    IAsyncAction OnNavigatedTo(NavigationEventArgs /* e */)
    {
        m_closedEventToken = m_streamWebSocket.Closed({ this, &StreamWebSocketPage::OnWebSocketClosed });

        try
        {
            co_await m_streamWebSocket.ConnectAsync(Uri{ L"wss://echo.websocket.org" });
            ReceiveMessageUsingStreamWebSocket();
            SendMessageUsingStreamWebSocket({ 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09 });
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }

private:
    IAsyncAction SendMessageUsingStreamWebSocket(std::vector< byte > message)
    {
        try
        {
            DataWriter dataWriter{ m_streamWebSocket.OutputStream() };
            dataWriter.WriteBytes(message);

            co_await dataWriter.StoreAsync();
            dataWriter.DetachStream();
            std::wstringstream wstringstream;
            wstringstream << L"Sending data using StreamWebSocket: " << message.size() << L" bytes" << std::endl;
            ::OutputDebugString(wstringstream.str().c_str());
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }

    IAsyncAction ReceiveMessageUsingStreamWebSocket()
    {
        try
        {
            DataReader dataReader{ m_streamWebSocket.InputStream() };
            dataReader.InputStreamOptions(InputStreamOptions::Partial);

            unsigned int bytesLoaded = co_await dataReader.LoadAsync(256);
            std::vector< byte > message(bytesLoaded);
            dataReader.ReadBytes(message);
            std::wstringstream wstringstream;
            wstringstream << L"Data received from StreamWebSocket: " << message.size() << " bytes" << std::endl;
            ::OutputDebugString(wstringstream.str().c_str());
            m_streamWebSocket.Close(1000, L"");
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }

    void OnWebSocketClosed(Windows::Networking::Sockets::IWebSocket const&, Windows::Networking::Sockets::WebSocketClosedEventArgs const& args)
    {
        std::wstringstream wstringstream;
        wstringstream << L"WebSocket_Closed; Code: " << args.Code() << ", Reason: \"" << args.Reason().c_str() << "\"" << std::endl;
        ::OutputDebugString(wstringstream.str().c_str());
        // Add additional code here to handle the WebSocket being closed.
    }
```

```cpp
#include <ppltasks.h>
#include <sstream>
...
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::StreamWebSocket^ streamWebSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->streamWebSocket = ref new Windows::Networking::Sockets::StreamWebSocket();

        this->streamWebSocket->Closed += ref new TypedEventHandler<Windows::Networking::Sockets::IWebSocket^, Windows::Networking::Sockets::WebSocketClosedEventArgs^>(this, &StreamWebSocketPage::WebSocket_Closed);

        try
        {
            auto connectTask = Concurrency::create_task(this->streamWebSocket->ConnectAsync(ref new Uri(L"wss://echo.websocket.org")));

            connectTask.then(
                [=]
            {
                this->ReceiveMessageUsingStreamWebSocket();
                this->SendMessageUsingStreamWebSocket(ref new Platform::Array< byte >{ 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09 });
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }

private:
    void SendMessageUsingStreamWebSocket(const Platform::Array< byte >^ message)
    {
        try
        {
            auto dataWriter = ref new DataWriter(this->streamWebSocket->OutputStream);
            dataWriter->WriteBytes(message);

            Concurrency::create_task(dataWriter->StoreAsync()).then(
                [=](Concurrency::task< unsigned int >) // task< unsigned int > instead of unsigned int in order to handle any exceptions thrown in StoreAsync().
            {
                dataWriter->DetachStream();
                std::wstringstream wstringstream;
                wstringstream << L"Sending data using StreamWebSocket: " << message->Length << L" bytes" << std::endl;
                ::OutputDebugString(wstringstream.str().c_str());
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }

    void ReceiveMessageUsingStreamWebSocket()
    {
        try
        {
            DataReader^ dataReader = ref new DataReader(this->streamWebSocket->InputStream);
            dataReader->InputStreamOptions = InputStreamOptions::Partial;

            Concurrency::create_task(dataReader->LoadAsync(256)).then(
                [=](unsigned int bytesLoaded)
            {
                auto message = ref new Platform::Array< byte >(bytesLoaded);
                dataReader->ReadBytes(message);
                std::wstringstream wstringstream;
                wstringstream << L"Data received from StreamWebSocket: " << message->Length << " bytes" << std::endl;
                ::OutputDebugString(wstringstream.str().c_str());
                this->streamWebSocket->Close(1000, L"");
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }

    void WebSocket_Closed(Windows::Networking::Sockets::IWebSocket^ sender, Windows::Networking::Sockets::WebSocketClosedEventArgs^ args)
    {
        std::wstringstream wstringstream;
        wstringstream << L"WebSocket_Closed; Code: " << args->Code << ", Reason: \"" << args->Reason->Data() << "\"" << std::endl;
        ::OutputDebugString(wstringstream.str().c_str());
        // Add additional code here to handle the WebSocket being closed.
    }
```

### <a name="handle-the-streamwebsocketclosed-event"></a>StreamWebSocket.Closed 이벤트 처리
연결을 설정하고 **StreamWebSocket**을 사용하여 데이터를 보내기 전에 [**StreamWebSocket.Closed**](/uwp/api/windows.networking.sockets.streamwebsocket.Closed) 이벤트를 구독해야 합니다. 클라이언트 또는 서버가 소켓을 종료할 때 **Closed**가 발생합니다.
 
### <a name="send-data-on-a-streamwebsocket"></a>StreamWebSocket에서 데이터 보내기
연결이 설정되면 서버에 데이터를 보낼 수 있습니다. 데이터를 쓰기 위해 [**StreamWebSocket.OutputStream**](https://docs.microsoft.com/en-us/uwp/api/Windows.Networking.Sockets.StreamWebSocket.OutputStream) 속성과 [**DataWriter**](/uwp/api/windows.storage.streams.datawriter)를 사용하여 이 작업을 수행할 수 있습니다.

**참고** 동일한 소켓에 더 많은 데이터를 쓰려는 경우 [**DataWriter.DetachStream**](/uwp/api/windows.storage.streams.datawriter.DetachStream)을 호출하여 **DataWriter**가 범위를 벗어나기 전에 출력 스트림을 **DataWriter**에서 분리해야 합니다. 이 작업은 스트림의 소유권을 **MessageWebSocket**으로 반환합니다.

### <a name="receive-data-on-a-streamwebsocket"></a>StreamWebSocket에서 데이터 받기
데이터를 읽기 위해 [**StreamWebSocket.InputStream**](https://docs.microsoft.com/en-us/uwp/api/Windows.Networking.Sockets.StreamWebSocket.InputStream) 속성과 [**DataReader**](/uwp/api/windows.storage.streams.datareader)를 사용할 수 있습니다.

## <a name="advanced-options-for-messagewebsocket-and-streamwebsocket"></a>MessageWebSocket 및 StreamWebSocket용 고급 옵션
연결을 설정하기 전에 [**MessageWebSocketControl**](/uwp/api/windows.networking.sockets.messagewebsocketcontrol) 또는 [**StreamWebSocketControl**](/uwp/api/windows.networking.sockets.streamwebsocketcontrol)에서 속성을 설정하여 소켓에서 고급 옵션을 설정할 수 있습니다. 적절한 그 [**MessageWebSocket.Control**](/uwp/api/windows.networking.sockets.messagewebsocket.control) 속성 또는 그 [**StreamWebSocket.Control**](/uwp/api/windows.networking.sockets.streamwebsocket.control) 속성을 통해 소켓 개체 자체에서 이러한 클래스의 인스턴스에 액세스할 수 있습니다.

**StreamWebSocket** 사용에 대한 예는 다음과 같습니다. 동일한 패턴이 **MessageWebSocket**에 적용됩니다.

```csharp
var streamWebSocket = new Windows.Networking.Sockets.StreamWebSocket();

// By default, the Nagle algorithm is not used. This overrides that, and causes it to be used.
streamWebSocket.Control.NoDelay = false;

await streamWebSocket.ConnectAsync(new Uri("wss://echo.websocket.org"));
```

```cppwinrt
Windows::Networking::Sockets::StreamWebSocket streamWebSocket;

// By default, the Nagle algorithm is not used. This overrides that, and causes it to be used.
streamWebSocket.Control().NoDelay(false);

auto connectAsyncAction = streamWebSocket.ConnectAsync(Uri{ L"wss://echo.websocket.org" });
```

```cpp
auto streamWebSocket = ref new Windows::Networking::Sockets::StreamWebSocket();

// By default, the Nagle algorithm is not used. This overrides that, and causes it to be used.
streamWebSocket->Control->NoDelay = false;

auto connectTask = Concurrency::create_task(streamWebSocket->ConnectAsync(ref new Uri(L"wss://echo.websocket.org")));
```

**참고** **ConnectAsync**를 호출한 *후* 컨트롤 속성을 변경하려고 하지 마세요. 이 규칙의 유일한 예외는 [MessageWebSocketControl.MessageType](/uwp/api/windows.networking.sockets.messagewebsocketcontrol.MessageType)입니다.

## <a name="websocket-information-classes"></a>WebSocket 정보 클래스
[**MessageWebSocket**](/uwp/api/windows.networking.sockets.messagewebsocket) 및 [**StreamWebSocket**](/uwp/api/windows.networking.sockets.streamwebsocket) 각각에는 개체에 대한 추가 정보를 제공하는 해당 클래스가 있습니다.

[**MessageWebSocketInformation**](/uwp/api/windows.networking.sockets.messagewebsocketinformation)은 **MessageWebSocket**에 대한 정보를 제공하며, [**MessageWebSocket.Information**](/uwp/api/windows.networking.sockets.messagewebsocket.Information) 속성을 사용하여 이 인스턴스를 검색합니다.

[**StreamWebSocketInformation**](/uwp/api/Windows.Networking.Sockets.StreamWebSocketInformation)은 **StreamWebSocket**에 대한 정보를 제공하며, [**StreamWebSocket.Information**](/uwp/api/Windows.Networking.Sockets.StreamWebSocket.Information) 속성을 사용하여 이 인스턴스를 검색합니다.

이 정보 클래스의 속성은 읽기 전용이지만 이를 사용하여 웹 소켓 개체의 수명 동안 언제든지 정보를 검색할 수 있습니다.

## <a name="handling-exceptions"></a>처리 예외
[**MessageWebSocket**](/uwp/api/Windows.Networking.Sockets.MessageWebSocket) 또는 [**StreamWebSocket**](/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 작업에서 발생한 오류는 **HRESULT** 값으로 반환됩니다. 이 **HRESULT** 값을 [**WebSocketError.GetStatus**](/uwp/api/windows.networking.sockets.websocketerror.getstatus) 메서드로 전달하여 [**WebErrorStatus**](/uwp/api/Windows.Web.WebErrorStatus) 열거형 값으로 변환할 수 있습니다.

대부분의 **WebErrorStatus** 열거형 값은 기본 HTTP 클라이언트 작업에서 반환한 오류에 해당합니다. 앱은 특정 **WebErrorStatus** 열거형 값을 켜 예외의 원인에 따라 앱 동작을 수정할 수 있습니다.

매개 변수 유효성 검사 오류의 경우 예외에서 **HRESULT**를 사용하여 오류에 대한 더 자세한 정보를 알 수 있습니다. 가능한 **HRESULT** 값은 `Winerror.h`에 나열되며, SDK 설치에서 확인할 수 있습니다(예를 들어 `C:\Program Files (x86)\Windows Kits\10\Include\<VERSION>\shared` 폴더에서). 대부분의 매개 변수 유효성 검사 오류에서 반환되는 **HRESULT**는 **E_INVALIDARG**입니다.

## <a name="setting-timeouts-on-websocket-operations"></a>WebSocket 작업에 대한 시간 제한 설정
**MessageWebSocket** 및 **StreamWebSocket**은 내부 시스템 서비스를 사용하여 WebSocket 클라이언트 요청을 보내고 서버에서 응답을 받습니다. WebSocket 연결 작업에 사용되는 기본 시간 제한 값은 60초입니다. WebSockets를 지원하는 HTTP 서버가 WebSocket 연결 요청에 응답하지 않거나 응답할 수 없는 경우(일시적 중단 또는 네트워크 중단으로 차단됨) 내부 시스템 서비스는 기본 60초 간 대기한 후 오류를 반환합니다. 해당 오류는 WebSocket **ConnectAsync** 메서드에 예외를 발생시킵니다. WebSocket 연결이 설정된 후 전송 및 수신 작업에 대해 기본 시간 제한은 30초입니다.

URI의 HTTP 서버 이름에 대한 이름 쿼리에서 이름의 IP 주소를 여러 개 반환하는 경우 내부 시스템 서비스는 각각 실패하기까지 기본 시간 제한 60초를 사용하여 사이트에 대해 최대 5개의 IP 주소를 시도합니다. 따라서 앱은 예외를 처리하기 전에 여러 IP 주소에 연결하기 위해 시도하는 것을 몇 분 정도 기다릴 수 있습니다. 이 동작은 사용자에게 앱 작동이 중지된 것처럼 보일 수 있습니다. 

앱의 응답성을 높이고 이러한 문제를 최소화하기 위해 연결 요청에 더 짧은 시간 제한을 설정할 수 있습니다. **MessageWebSocket** and **StreamWebSocket**둘 다에 대해 유사한 방식으로 시간 제한을 설정합니다.

```csharp
private Windows.Networking.Sockets.MessageWebSocket messageWebSocket;

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    this.messageWebSocket = new Windows.Networking.Sockets.MessageWebSocket();

    try
    {
        var cancellationTokenSource = new CancellationTokenSource();
        var connectTask = this.messageWebSocket.ConnectAsync(new Uri("wss://echo.websocket.org")).AsTask(cancellationTokenSource.Token);

        // Cancel connectTask after 5 seconds.
        cancellationTokenSource.CancelAfter(TimeSpan.FromMilliseconds(5000));

        connectTask.ContinueWith((antecedent) =>
        {
            if (antecedent.Status == TaskStatus.RanToCompletion)
            {
                // connectTask ran to completion, so we know that the MessageWebSocket is connected.
                // Add additional code here to use the MessageWebSocket.
            }
            else
            {
                // connectTask timed out, or faulted.
            }
        });
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add additional code here to handle exceptions.
    }
}
```

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
#include <sstream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::MessageWebSocket m_messageWebSocket;

    IAsyncAction TimeoutAsync()
    {
        // Return control to the caller, and resume again to complete the async action after the timeout period.
        // 5 seconds, in this example.
        co_await(std::chrono::seconds{ 5 });
    }

public:
    IAsyncAction OnNavigatedTo(NavigationEventArgs /* e */)
    {
        try
        {
            // Return control to the caller, and then immediately resume on a thread pool thread.
            co_await winrt::resume_background();

            auto connectAsyncAction = m_messageWebSocket.ConnectAsync(Uri{ L"wss://echo.websocket.org" });

            TimeoutAsync().Completed([connectAsyncAction](IAsyncAction const& sender, AsyncStatus const)
            {
                // TimeoutAsync completes after the timeout period. After that period, it's safe
                // to cancel the ConnectAsync action even if it has already completed.
                connectAsyncAction.Cancel();
            });

            try
            {
                // Block until the ConnectAsync action completes or is canceled.
                connectAsyncAction.get();
            }
            catch (winrt::hresult_error const& ex)
            {
                std::wstringstream wstringstream;
                wstringstream << L"ConnectAsync threw an exception: " << ex.message().c_str() << std::endl;
                ::OutputDebugString(wstringstream.str().c_str());
            }

            if (connectAsyncAction.Status() == AsyncStatus::Completed)
            {
                // connectTask ran to completion, so we know that the MessageWebSocket is connected.
                // Add additional code here to use the MessageWebSocket.
            }
            else
            {
                // connectTask did not run to completion.
            }
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }
```

```cpp
#include <agents.h>
#include <ppltasks.h>
#include <sstream>
...
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::MessageWebSocket^ messageWebSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->messageWebSocket = ref new Windows::Networking::Sockets::MessageWebSocket();

        try
        {
            Concurrency::cancellation_token_source cancellationTokenSource;
            Concurrency::cancellation_token cancellationToken = cancellationTokenSource.get_token();

            auto connectTask = Concurrency::create_task(this->messageWebSocket->ConnectAsync(ref new Uri(L"wss://echo.websocket.org")), cancellationToken);

            // This continuation task returns true should connectTask run to completion.
            Concurrency::task< bool > taskRanToCompletion = connectTask.then([](void)
            {
                return true;
            });

            // This task returns false after the specified timeout. 5 seconds, in this example.
            Concurrency::task< bool > taskTimedout = Concurrency::create_task([]() -> bool
            {
                Concurrency::task_completion_event< void > taskCompletionEvent;

                // A call object that sets the task completion event.
                auto call = std::make_shared< Concurrency::call< int > >([taskCompletionEvent](int)
                {
                    taskCompletionEvent.set();
                });

                // A non-repeating timer that calls the call object when the timer fires.
                auto nonRepeatingTimer = std::make_shared< Concurrency::timer < int > >(5000, 0, call.get(), false);
                nonRepeatingTimer->start();

                // A task that completes after the completion event is set.
                Concurrency::task< void > taskWaitForCompletionEvent(taskCompletionEvent);

                return taskWaitForCompletionEvent.then([]() {return false; }).get();
            });

            (taskRanToCompletion || taskTimedout).then([this, cancellationTokenSource](bool connectTaskRanToCompletion)
            {
                if (connectTaskRanToCompletion)
                {
                    // connectTask ran to completion, so we know that the MessageWebSocket is connected.
                    // Add additional code here to use the MessageWebSocket.
                }
                else
                {
                    // taskTimedout ran to completion, so we should cancel connectTask via the cancellation_token_source.
                    cancellationTokenSource.cancel();
                }
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }
```

## <a name="important-apis"></a>중요 API
* [DataReader](/uwp/api/Windows.Storage.Streams.DataReader)
* [DataWriter](/uwp/api/Windows.Storage.Streams.DataWriter)
* [DataWriter.DetachStream](/uwp/api/windows.storage.streams.datawriter.DetachStream)
* [MessageWebSocket](/uwp/api/windows.networking.sockets.messagewebsocket)
* [MessageWebSocket.Closed](/uwp/api/Windows.Networking.Sockets.MessageWebSocket.Closed)
* [MessageWebSocket.ConnectAsync](/uwp/api/windows.networking.sockets.messagewebsocket.connectasync)
* [MessageWebSocket.Control](/uwp/api/windows.networking.sockets.messagewebsocket.control)
* [MessageWebSocket.Information](/uwp/api/Windows.Networking.Sockets.MessageWebSocket.Information)
* [MessageWebSocket.MessageReceived](/uwp/api/Windows.Networking.Sockets.MessageWebSocket.MessageReceived)
* [MessageWebSocket.OutputStream](https://docs.microsoft.com/en-us/uwp/api/Windows.Networking.Sockets.MessageWebSocket.OutputStream)
* [MessageWebSocketControl](/uwp/api/Windows.Networking.Sockets.MessageWebSocketControl)
* [MessageWebSocketControl.MessageType](/uwp/api/Windows.Networking.Sockets.MessageWebSocketControl.MessageType)
* [MessageWebSocketInformation](/uwp/api/Windows.Networking.Sockets.MessageWebSocketInformation)
* [MessageWebSocketMessageReceivedEventArgs](/uwp/api/Windows.Networking.Sockets.MessageWebSocketMessageReceivedEventArgs)
* [SocketMessageType](/uwp/api/windows.networking.sockets.socketmessagetype)
* [StreamWebSocket](/uwp/api/Windows.Networking.Sockets.StreamWebSocket)
* [StreamWebSocket.Closed](/uwp/api/Windows.Networking.Sockets.StreamWebSocket.Closed)
* [StreamSocket.ConnectAsync](/uwp/api/windows.networking.sockets.streamsocket.connectasync)
* [StreamWebSocket.Control](/uwp/api/windows.networking.sockets.streamwebsocket.control)
* [StreamWebSocket.Information](/uwp/api/windows.networking.sockets.streamwebsocket.Information)
* [StreamWebSocket.InputStream](https://docs.microsoft.com/en-us/uwp/api/Windows.Networking.Sockets.StreamWebSocket.InputStream)
* [StreamWebSocket.OutputStream](https://docs.microsoft.com/en-us/uwp/api/Windows.Networking.Sockets.StreamWebSocket.OutputStream)
* [StreamWebSocketControl](/uwp/api/Windows.Networking.Sockets.StreamWebSocketControl)
* [StreamWebSocketInformation](/uwp/api/Windows.Networking.Sockets.StreamWebSocketInformation)
* [WebErrorStatus](/uwp/api/Windows.Web.WebErrorStatus) 
* [WebSocketError.GetStatus](/uwp/api/windows.networking.sockets.websocketerror.getstatus)
* [Windows.Networking.Sockets](/uwp/api/Windows.Networking.Sockets)

## <a name="related-topics"></a>관련 항목
* [WebSocket 프로토콜](http://tools.ietf.org/html/rfc6455)
* [소켓](sockets.md)

## <a name="samples"></a>샘플
* [WebSocket 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620623)
