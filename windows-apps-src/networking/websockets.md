---
author: DelfCo
description: WebSocket은 HTTP(S)를 사용하여 웹을 통해 클라이언트와 서버 간의 신속하고 보안이 유지된 양방향 통신을 위한 메커니즘을 제공합니다.
title: WebSocket
ms.assetid: EAA9CB3E-6A3A-4C13-9636-CCD3DE46E7E2
---

# WebSocket

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

**중요 API**

-   [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)
-   [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)

WebSocket은 HTTP(S)를 사용하여 웹을 통해 클라이언트와 서버 간의 신속하고 보안이 유지된 양방향 통신을 위한 메커니즘을 제공합니다.

[WebSocket 프로토콜](http://tools.ietf.org/html/rfc6455)에서는 데이터가 전이중 단일 소켓 연결을 통해 즉시 전송되므로 두 끝점에서 실시간으로 메시지를 보내고 받을 수 있습니다. WebSocket은 즉각적인 소셜 네트워크 알림 및 최신 정보(예: 게임 통계)의 표시에 보안 및 고속 데이터 전송이 필요한 실시간 게임에 적합합니다. UWP(유니버설 Windows 플랫폼) 개발자는 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 및 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 클래스를 사용하여 Websocket 프로토콜을 지원하는 서버와 연결할 수 있습니다.

| MessageWebSocket                                                         | StreamWebSocket                                                                               |
|--------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| 메시지가 지나치게 크지 않은 일반적인 시나리오에 적합합니다.   | 사진이나 동영상 등 큰 파일이 전송되는 시나리오에 적합합니다. |
| 전체 WebSocket 메시지가 수신되었다는 알림을 사용합니다. | 각 읽기 작업으로 메시지의 섹션을 읽을 수 있습니다.                             |
| UTF-8 메시지와 바이너리 메시지를 모두 지원합니다.                                 | 바이너리 메시지만 지원합니다.                                                                |
| UDP 또는 데이터그램 소켓과 유사합니다.                                     | TCP 또는 스트림 소켓과 유사합니다.                                                            |

대부분의 경우 보안 WebSocket 연결을 사용하여 전송 및 수신되는 데이터를 암호화할 수 있습니다. 이 경우 많은 프록시가 암호화되지 않은 WebSocket 연결을 거부하므로 연결이 성공할 가능성도 높아집니다. WebSocket 프로토콜은 다음 두 가지 URI 스키마를 정의합니다.

-   ws: - 암호화되지 않은 연결에 사용됨
-   wss: - 암호화해야 하는 보안 연결에 사용됨

WebSocket 연결을 암호화하려면 사용 wss: URI 스키마(예: `wss://www.contoso.com/mywebservice`)를 사용합니다.

## MessageWebSocket 사용

[
            **MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)을 사용하면 각 읽기 작업으로 메시지의 섹션을 읽을 수 있습니다. **MessageWebSocket**은 일반적으로 메시지가 너무 크지 않은 시나리오에서 사용됩니다. UTF-8과 이진 파일이 모두 지원됩니다.

이 섹션의 코드는 새 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)을 만들고, WebSocket 서버에 연결하고, 데이터를 서버로 보냅니다. 연결이 성공적으로 설정되면 앱은 데이터가 수신되었음을 나타내는 [**MessageWebSocket.MessageReceived**](https://msdn.microsoft.com/library/windows/apps/br241358) 이벤트가 트리거되기를 기다립니다.

다음 샘플에서는 전송된 모든 문자열을 보낸 사람에게 다시 에코하기만 하는 서비스인 WebSocket.org 에코 서버를 사용합니다. 다음 샘플에서는 "wss:" 프로토콜 지정자를 사용하여 보안 연결을 통해 메시지를 보내고 받습니다.

> [!div class="tabbedCodeSnippets"]
> ```cpp
> void Game::InitWebSockets()
> {
>     // Create a new web socket
>     m_messageWebSocket = ref new MessageWebSocket();
> 
>     // Set the message type to UTF-8
>     m_messageWebSocket->Control->MessageType = Windows::Networking::Sockets::SocketMessageType::Utf8;
> 
>     // Register callbacks for notifications of interest
>     m_messageWebSocket->MessageReceived += 
>        ref new TypedEventHandler<MessageWebSocket^, MessageWebSocketMessageReceivedEventArgs^>(this, &Game::WebSocketMessageReceived);
>     m_messageWebSocket->Closed += ref new TypedEventHandler<IWebSocket^, WebSocketClosedEventArgs^>(this, &Game::WebSocketClosed);
> 
>     // This test code uses the websocket.org echo service to illustrate sending a string and receiving the echoed string back
>     // Note that wss: makes this an encrypted connection.
>     m_serverUri = ref new Uri("wss://echo.websocket.org");
> 
>     // Establish the connection, and set m_socketConnected on success
>     create_task(m_messageWebSocket->ConnectAsync(m_serverUri)).then([this] (task<void> previousTask)
>     {
>         try
>         {
>             // Try getting all exceptions from the continuation chain above this point.
>             previousTask.get();
> 
>             // websocket connected. update state variable
>             m_socketConnected = true;
>             OutputDebugString(L"Successfully initialized websockets\n");
>         }
>         catch (Platform::COMException^ exception)
>         {
>             // Add code here to handle any exceptions
>             // HandleException(exception);
> 
>         }
>     });
> }
> ```
> ```cs
> MessageWebSocket webSock = new MessageWebSocket();
> 
> //In this case we will be sending/receiving a string so we need to set the MessageType to Utf8.
> webSock.Control.MessageType = SocketMessageType.Utf8;
> 
> //Add the MessageReceived event handler.
> webSock.MessageReceived += WebSock_MessageReceived;
> 
> //Add the Closed event handler.
> webSock.Closed += WebSock_Closed;
> 
> Uri serverUri = new Uri("wss://echo.websocket.org");
> 
> try
> {
>     //Connect to the server.
>     await webSock.ConnectAsync(serverUri);
> 
>     //Send a message to the server.
>     await WebSock_SendMessage(webSock, "Hello, world!");
> }
> catch (Exception ex)
> {
>     //Add code here to handle any exceptions
> }
> ```

WebSocket 연결을 초기화한 후에는 코드에서 데이터를 적절히 보내고 받기 위해 다음 작업을 수행해야 합니다.

### MessageWebSocket.MessageReceived 이벤트에 대한 콜백 구현

WebSocket을 사용하여 연결을 설정하고 데이터를 전송하려면 먼저 앱에서 데이터가 수신된 경우 알림을 받기 위한 이벤트 콜백을 등록해야 합니다. [
            **MessageWebSocket.MessageReceived**](https://msdn.microsoft.com/library/windows/apps/br241358) 이벤트가 발생하면 등록된 콜백이 호출되고 [**MessageWebSocketMessageReceivedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br226852)에서 데이터를 받습니다. 다음 예제에서는 전송되는 메시지가 UTF-8 형식인 것으로 가정합니다.

다음 샘플 함수는 연결된 WebSocket 서버에서 문자열을 수신하고 디버거 출력 창에 문자열을 출력합니다.

> [!div class="tabbedCodeSnippets"]
>```cpp
>void Game::WebSocketMessageReceived(MessageWebSocket^ sender, MessageWebSocketMessageReceivedEventArgs^ args)
>{
>    DataReader^ messageReader = args->GetDataReader();
>    messageReader->UnicodeEncoding = Windows::Storage::Streams::UnicodeEncoding::Utf8;
>
>    String^ readString = messageReader->ReadString(messageReader->UnconsumedBufferLength);
>    // Data has been read and is now available from the readString variable.
>    swprintf(m_debugBuffer, 511, L"WebSocket Message received: %s\n", readString->Data());
>    OutputDebugString(m_debugBuffer);
>}
>```
>```csharp
>//The MessageReceived event handler.
>private void WebSock_MessageReceived(MessageWebSocket sender, MessageWebSocketMessageReceivedEventArgs args)
>{
>    DataReader messageReader = args.GetDataReader();
>    messageReader.UnicodeEncoding = UnicodeEncoding.Utf8;
>    string messageString = messageReader.ReadString(messageReader.UnconsumedBufferLength);
>
>    //Add code here to do something with the string that is received.
>}
>```

###  MessageWebSocket.Closed 이벤트에 대한 콜백 구현

WebSocket을 사용하여 연결을 설정하고 데이터를 전송하려면 먼저 앱에서 WebSocket이 WebSocket 서버에 의해 닫힌 경우 알림을 받기 위한 이벤트 콜백을 등록해야 합니다. [
            **MessageWebSocket.Closed**](https://msdn.microsoft.com/library/windows/apps/hh701364) 이벤트가 발생하면 등록된 콜백이 호출되어 WebSocket 서버에 의해 연결이 닫혔음을 나타냅니다.

> [!div class="tabbedCodeSnippets"]
>```cpp
>void Game::WebSocketClosed(IWebSocket^ sender, WebSocketClosedEventArgs^ args)
>{
>    // The method may be triggered remotely by the server sending unsolicited close frame or locally by Close()/delete operator.
>    // This method assumes we saved the connected WebSocket to a variable called m_messageWebSocket
>    if (m_messageWebSocket != nullptr)
>    {
>        delete m_messageWebSocket;
>        m_messageWebSocket = nullptr;
>        OutputDebugString(L"Socket was closed\n");
>    }
>    m_socketConnected = false;
> }
>```
>```csharp
>//The Closed event handler
>private void WebSock_Closed(IWebSocket sender, WebSocketClosedEventArgs args)
>{
>    //Add code here to do something when the connection is closed locally or by the server
>}
>```

###  WebSocket에 메시지 보내기

연결이 설정되면 WebSocket 클라이언트에서 서버에 데이터를 보낼 수 있습니다. [
            **DataWriter.StoreAsync**](https://msdn.microsoft.com/library/windows/apps/br208171) 메서드는 서명되지 않은 int에 매핑되는 매개 변수를 반환합니다. 따라서 연결을 설정하는 작업과 비교하여 메시지를 보내는 작업을 정의하는 방법이 변경됩니다.

**참고** MessageWebSocket의 OutputStream을 사용하여 새 DataWriter 개체를 만든 경우 DataWriter는 OutputStream의 소유권을 가져오며, DataWriter가 범위를 벗어나면 Outputstream의 할당을 취소합니다. 이로 인해 이후에 OutputStream을 사용하려는 시도가 HRESULT 값 0x80000013과 함께 실패합니다. OutputStream 할당 취소를 방지하기 위해 이 코드는 스트림의 소유권을 WebSocket 개체로 반환하는 DataWriter의 DetachStream 메서드를 사용합니다.

다음 함수는 지정된 문자열을 연결된 WebSocket으로 보내고 디버거 출력 창에 확인 메시지를 출력합니다.

> [!div class="tabbedCodeSnippets"]
>```cpp
>void Game::SendWebSocketMessage(Windows::Networking::Sockets::MessageWebSocket^ sendingSocket, Platform::String^ message)
>{
>    if (m_socketConnected)
>    {
>        // WebSocket is connected, so send a message
>        m_messageWriter = ref new DataWriter(sendingSocket->OutputStream);
>
>        m_messageWriter->WriteString(message);
>
>        // Send the data as one complete message
>        create_task(m_messageWriter->StoreAsync()).then([this] (unsigned int)
>        {
>            // Send Completed
>            m_messageWriter->DetachStream();    // give the stream back to m_messageWebSocket
>            OutputDebugString(L"Sent websocket message\n");
>        })
>            .then([this] (task<void>> previousTask)
>        {
>            try
>            {
>                // Try getting all exceptions from the continuation chain above this point.
>                previousTask.get();
>            }
>            catch (Platform::COMException ^ex)
>            {
>                // Add code to handle the exception
>                // HandleException(exception);
>            }
>        });
>    }
>}
>```
>```csharp
>//Send a message to the server.
>private async Task WebSock_SendMessage(MessageWebSocket webSock, string message)
>{
>    DataWriter messageWriter = new DataWriter(webSock.OutputStream);
>    messageWriter.WriteString(message);
>    await messageWriter.StoreAsync();
>}
>```

## WebSocket에서 고급 컨트롤 사용

[
            **MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 및 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)은 고급 컨트롤 사용에 대한 동일한 모델을 따릅니다. 위 기본 클래스에는 각각 고급 컨트롤에 액세스할 수 있는 관련 클래스가 있습니다.

[
            **MessageWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226843)은 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 개체에 대한 소켓 컨트롤 데이터를 제공합니다.
[
            **StreamWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226924)은 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 개체에 대한 소켓 컨트롤 데이터를 제공합니다.
고급 컨트롤을 사용하는 기본 모델은 두 가지 유형의 WebSocket에 대해 모두 동일합니다. 아래 설명에서는 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)을 예로 사용하지만 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)에도 동일한 프로세스를 사용할 수 있습니다.

1.  [
            **StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 개체를 만듭니다.
2.  [
            **StreamWebSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226934) 속성을 사용하여 이 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 개체와 연결된 [**StreamWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226924) 인스턴스를 검색합니다.
3.  [
            **StreamWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226924) 인스턴스에 대한 속성을 가져오거나 설정하여 특정 고급 컨트롤을 가져오거나 설정합니다.

[
            **StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 및 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 둘 다 고급 컨트롤을 설정할 수 있는 경우에 대한 요구 사항이 있습니다.

-   [
            **StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)의 모든 고급 컨트롤에 대해 연결 작업을 실행하기 전에 항상 앱에서 속성을 설정해야 합니다. 이 요구 사항 때문에 **StreamWebSocket** 개체를 만든 즉시 모든 컨트롤 속성을 설정하는 것이 가장 좋습니다. [
            **StreamWebSocket.ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/br226933) 메서드가 호출된 후 컨트롤 속성을 설정하려고 하지 마세요.
-   메시지 형식을 제외하고 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)의 모든 고급 컨트롤에 대해 연결 작업을 실행하기 전에 속성을 설정해야 합니다. **MessageWebSocket** 개체를 만든 즉시 모든 컨트롤 속성을 설정하는 것이 가장 좋습니다. 메시지 형식 외에는 [**MessageWebSocket.ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/br226859)가 호출된 후 컨트롤 속성을 변경하지 마세요.

## WebSocket 정보 클래스

[
            **MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 및 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 각각에는 WebSocket 인스턴스에 대한 추가 정보를 제공하는 해당 클래스가 있습니다.

[
            **MessageWebSocketInformation**](https://msdn.microsoft.com/library/windows/apps/br226849)은 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)에 대한 정보를 제공하며, [**MessageWebSocket.Information**](https://msdn.microsoft.com/library/windows/apps/br226861) 속성을 사용하여 정보 클래스의 인스턴스를 검색합니다.

[
            **StreamWebSocketInformation**](https://msdn.microsoft.com/library/windows/apps/br226929)은 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)에 대한 정보를 제공하며, [**StreamWebSocket.Information**](https://msdn.microsoft.com/library/windows/apps/br226935) 속성을 사용하여 정보 클래스의 인스턴스를 검색합니다.

두 Information 클래스의 모든 속성은 읽기 전용이며, 웹 소켓 객체의 수명 동안 언제든지 현재 정보를 검색할 수 있습니다.

## 네트워크 예외 처리

[
            **MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 또는 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 작업에서 발생한 오류는 **HRESULT** 값으로 반환됩니다. [
            **WebSocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701529) 메서드는 네트워크 오류를 WebSocket 작업에서 [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818) 열거형 값으로 변환하는 데 사용됩니다. 대부분의 **WebErrorStatus** 열거형 값은 기본 HTTP 클라이언트 작업에서 반환한 오류에 해당합니다. 앱은 특정 **WebErrorStatus** 열거형 값을 필터링하여 예외의 원인에 따라 앱 동작을 수정할 수 있습니다.

매개 변수 유효성 검사 오류의 경우 앱은 또한 예외에서 **HRESULT**를 사용하여 예외의 원인이 된 오류에 대한 더 자세한 정보를 알 수 있습니다. 가능한 **HRESULT** 값은 *Winerror.h* 헤더 파일에 나열되어 있습니다. 대부분의 매개 변수 유효성 검사 오류에서 반환되는 **HRESULT**는 **E\_INVALIDARG**입니다.

## WebSocket 작업에 대한 시간 제한 설정

MessageWebSocket 및 StreamWebSocket 클래스는 내부 시스템 서비스를 사용하여 WebSocket 클라이언트 요청을 보내고 서버에서 응답을 받습니다. WebSocket 연결 작업에 사용되는 기본 시간 제한 값은 60초입니다. WebSockets을 지원하는 HTTP 서버가 일시적으로 중단되거나 네트워크 중단으로 차단되어 서버에서WebSocket 연결 요청에 응답하지 않거나 응답할 수 없는 경우 내부 시스템 서비스는 기본 60초간 대기한 후 WebSocket ConnectAsync 메서드에서 예외를 throw하는 오류를 반환합니다. URI의 HTTP 서버 이름에 대한 이름 쿼리에서 이름의 IP 주소를 여러 개 반환하는 경우 내부 시스템 서비스는 각각 실패하기까지 기본 시간 제한 60초를 사용하여 사이트에 대해 최대 5개의 IP 주소를 시도합니다. WebSocket 연결 요청을 하는 앱은 에서 오류가 반환되어 예외가 발생하기까지 몇 분 정도 기다린 후 여러 IP 주소에 대한 연결을 시도할 수 있습니다. 이 동작은 사용자에게 앱 작동이 중지된 것처럼 보일 수 있습니다. WebSocket 연결 후 보내기 및 받기 작업에 사용되는 기본 시간 제한은 30초입니다.

앱의 응답성을 높이고 이러한 문제를 최소화하기 위해 보다 짧은 연결 요청 시간 제한을 설정할 수 있습니다. 이렇게 하면 기본 설정보다 짧은 시간 제한으로 인해 작업에 실패합니다.

StreamWebSocket과 MessageWebSocket 둘 다에 대해 유사한 시간 제한을 설정합니다. 다음 예제에서는 StreamWebSocket에 대한 시간 제한 설정 방법을 보여 주지면 MessageWebSocket에 대한 프로세스도 유사합니다.

1.  타이머를 사용하여 지정된 지연 시간 후에 완료되는 작업을 만듭니다.
2.  cancellation\_token\_source를 사용하여 WebSocket 작업에 대해 취소를 지원하는 작업을 만듭니다.
3.  WebSocket 연결 작업 전에 지정된 시간 제한 지연 시간이 있는 작업이 완료되면 WebSocket 작업을 취소합니다.

다음 예제에서는 지정된 지연 시간 후 완료되는 작업을 만들고 지정된 지연 시간 후 취소되는 두 번째 작업을 만듭니다. 이러한 클래스를 StreamWebSocket 및 MessageWebSocket과 함께 사용하여 연결을 설정하려고 할 때 특정 시간 제한을 설정할 수 있습니다. 예제에서는 취소를 지원하는 cancellation\_token\_source를 사용하여 작업에서 StreamWebSocket.ConnectAsync 메서드를 호출합니다. 시간 제한이 먼저 완료되면 cancellation\_token\_source를 사용하여 WebSocket 연결 작업을 취소합니다.

```cpp
    #include <agents.h>
    #include <ppl.h>
    #include <ppltasks.h>

    using namespace concurrency;
    using namespace std;

    // Creates a task that completes after the specified delay.
    task<void> complete_after(unsigned int timeout)
    {
        // A task completion event that is set when a timer fires.
        task_completion_event<void> tce;

        // Create a non-repeating timer.
        shared_ptr<timer<int>> fire_once(new timer<int>(timeout, 0, nullptr, false));
        
        // Create a call object that sets the completion event after the timer fires.
        shared_ptr<call<int>> callback(new call<int>([tce](int)
        {
            tce.set();
        }));

        // Connect the timer to the callback and start the timer.
        fire_once->link_target(callback.get());
        fire_once->start();

        // Create a task that completes after the completion event is set.
        task<void> event_set(tce);

        // Create a continuation task that cleans up resources and
        // and return that continuation task.
        return event_set.then([callback, fire_once]()
        {
        });
    }

    // Cancels the provided task after the specifed delay, if the task
    // did not complete.
    template<typename T>
    task<T> cancel_after_timeout(task<T> t, cancellation_token_source cts, unsigned int timeout)
    {
        // Create a task that returns true after the specified task completes.
        task<bool> success_task = t.then([](T)
        {
            return true;
        });
        // Create a task that returns false after the specified timeout.
        task<bool> failure_task = complete_after(timeout).then([]
        {
            return false;
        });

        // Create a continuation task that cancels the overall task  
        // if the timeout task finishes first. 
        return (failure_task || success_task).then([t, cts](bool success)
        {
            if (!success)
            {
                // Set the cancellation token. The task that is passed as the 
                // t parameter should respond to the cancellation and stop 
                // as soon as it can.
                cts.cancel();
            }
 
            // Return the original task.
            return t;
        });
    }
```



<!--HONumber=May16_HO2-->


