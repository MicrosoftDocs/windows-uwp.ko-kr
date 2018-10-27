---
author: stevewhims
description: 네트워크 통신을 계속하려면 백그라운드가 아닌 상태에서 앱이 백그라운드 작업을 사용하고 소켓 브로커나 컨트롤 채널 트리거를 사용할 수 있습니다.
title: 백그라운드 네트워크 통신
ms.assetid: 537F8E16-9972-435D-85A5-56D5764D3AC2
ms.author: stwhi
ms.date: 06/14/2018
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 34fad804bb36ad1b4ce92a56772c33318e10faa8
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5711280"
---
# <a name="network-communications-in-the-background"></a>백그라운드 네트워크 통신
네트워크 통신을 포그라운드에 없을 때 계속 해 서 앱 백그라운드 작업 및 이러한 두 옵션 중 하나를 사용할 수 있습니다.
- 소켓 브로커. 사용 하는 경우 앱 소켓 오랜 기간 연결에 대 한 다음, 포그라운드를 벗어날 때, 시스템 소켓 브로커에 소켓의 소유권을 위임할 수 것입니다. 그런 다음 브로커:; 소켓에 트래픽이 도착 하면 앱을 활성화 합니다. 소유권을 다시 앱입니다. 한 앱에는 다음 도착 하는 트래픽을 처리 합니다.
- 컨트롤 채널 트리거 

## <a name="performing-network-operations-in-background-tasks"></a>백그라운드 작업으로 네트워크 작업 수행
- [SocketActivityTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.socketactivitytrigger)를 사용하여 패킷이 수신되고 지속 시간이 짧은 작업을 수행해야 하는 경우 백그라운드 작업을 활성화합니다. 작업을 수행한 후 전원을 절약 하기 위해 백그라운드 작업을 종료 해야 합니다.
- [ControlChannelTrigger](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger)를 사용하여 패킷이 수신되고 지속 시간이 긴 작업을 수행해야 하는 경우 백그라운드 작업을 활성화합니다.

**네트워크 관련 조건 및 플래그**

- 백그라운드 작업에 **InternetAvailable** 조건을 추가[BackgroundTaskBuilder.AddCondition](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)하여 네트워크 스택이 실행될 때까지 백그라운드 작업의 트리거를 지연시킵니다. 네트워크가 가동되어야 백그라운드 작업이 실행되기 때문에 이 조건을 적용하면 전원이 절약됩니다. 이 조건은 실시간 정품 인증을 제공하지 않습니다.

어떤 트리거를 사용하든, 백그라운드 작업에 [IsNetworkRequested](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder)를 설정하여 백그라운드 작업이 실행되는 동안 네트워크 가동을 유지해야 합니다. 이렇게 하면 디바이스가 연결된 대기 상태 모드인 경우에도 작업 실행 중 네트워크를 계속 유지하도록 백그라운드 작업 인프라에 지시할 수 있습니다. 백그라운드 작업 **IsNetworkRequested**을 사용 하지 않는 한 다음 백그라운드 작업이 네트워크 연결 된 대기 상태 모드 (예: 휴대폰 화면이 꺼져 있는 경우)에 액세스할 수 없으므로 되지 않습니다.

## <a name="socket-broker-and-the-socketactivitytrigger"></a>소켓 브로커 및 SocketActivityTrigger
앱에서 [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319), [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 또는 [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) 연결을 사용하는 경우 [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009) 및 소켓 브로커를 사용하여 앱이 전경에 없을 때 앱에 대한 트래픽이 도착하면 알림을 받도록 해야 합니다.

앱이 활성 상태가 아닐 때 앱이 소켓에서 데이터를 수신하고 수신한 데이터를 처리하도록 하려면 앱은 시작 시 일회성 설정 작업을 수행한 다음 비활성 상태로 전환할 때 소켓 브로커에 소켓 소유권을 이전해야 합니다.

일회성 설정 단계는 트리거 만들기, 트리거를 위해 백그라운드 작업 등록 및 소켓 브로커를 위해 소켓 설정 등입니다.
  - **SocketActivityTrigger**를 만들고 TaskEntryPoint 매개 변수가 수신된 패킷을 처리하기 위한 코드로 설정되어 있는 트리거에 대해 백그라운드 작업을 등록합니다.
```csharp
            var socketTaskBuilder = new BackgroundTaskBuilder();
            socketTaskBuilder.Name = _backgroundTaskName;
            socketTaskBuilder.TaskEntryPoint = _backgroundTaskEntryPoint;
            var trigger = new SocketActivityTrigger();
            socketTaskBuilder.SetTrigger(trigger);
            _task = socketTaskBuilder.Register();
```
  - 소켓을 바인딩하기 전에 소켓에서 **EnableTransferOwnership**을 호출합니다.
```csharp
           _tcpListener = new StreamSocketListener();

           // Note that EnableTransferOwnership() should be called before bind,
           // so that tcpip keeps required state for the socket to enable connected
           // standby action. Background task Id is taken as a parameter to tie wake pattern
           // to a specific background task.  
           _tcpListener. EnableTransferOwnership(_task.TaskId,SocketActivityConnectedStandbyAction.Wake);
           _tcpListener.ConnectionReceived += OnConnectionReceived;
           await _tcpListener.BindServiceNameAsync("my-service-name");
```

소켓이 올바르게 설정되면 앱이 일시 중단되려고 할 때 소켓에서 **TransferOwnership**을 호출하여 소켓 브로커로 이전합니다. 브로커는 소켓을 모니터링하고 데이터가 수신되면 백그라운드 작업을 활성화합니다. 다음 예제에는 **StreamSocketListener** 소켓에 대해 이전을 수행하는 유틸리티 **TransferOwnership** 함수가 포함되어 있습니다. 여러 유형의 소켓이 각각 고유한 **TransferOwnership** 메서드를 가지고 있으므로 소유권을 이전하는 소켓에 적합한 메서드를 호출해야 합니다.) 코드에는 사용하는 각 소켓 유형에 대해 하나의 구현으로 오버로드된 **TransferOwnership** 도우미가 포함되어 있을 수 있습니다. 따라서 **OnSuspending** 코드는 계속 읽기 쉽게 유지됩니다.

앱에서 소켓의 소유권을 소켓 브로커로 이전하고 다음 메서드 중 적절한 메서드를 사용하여 백그라운드 작업에 대한 ID를 전달합니다.
-   [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319)의 [**TransferOwnership**](https://msdn.microsoft.com/library/windows/apps/dn804256) 메서드 중 하나
-   [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)의 [**TransferOwnership**](https://msdn.microsoft.com/library/windows/apps/dn781433) 메서드 중 하나
-   [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906)의 [**TransferOwnership**](https://msdn.microsoft.com/library/windows/apps/dn804407) 메서드 중 하나

```csharp

// declare int _transferOwnershipCount as a field.

private void TransferOwnership(StreamSocketListener tcpListener)
{
    await tcpListener.CancelIOAsync();

    var dataWriter = new DataWriter();
    ++_transferOwnershipCount;
    dataWriter.WriteInt32(transferOwnershipCount);
    var context = new SocketActivityContext(dataWriter.DetachBuffer());
    tcpListener.TransferOwnership(_socketId, context);
}

private void OnSuspending(object sender, SuspendingEventArgs e)
{
    var deferral = e.SuspendingOperation.GetDeferral();

    TransferOwnership(_tcpListener);
    deferral.Complete();
}
```
백그라운드 작업의 이벤트 처리기에서:
   -  먼저, 비동기 메서드를 사용하여 이벤트를 처리할 수 있도록 백그라운드 작업 지연을 가져옵니다.
```csharp
var deferral = taskInstance.GetDeferral();
```
   -  다음으로 이벤트 인수에서 SocketActivityTriggerDetails를 추출하고 이벤트가 발생한 이유를 찾습니다.
```csharp
var details = taskInstance.TriggerDetails as SocketActivityTriggerDetails;
    var socketInformation = details.SocketInformation;
    switch (details.Reason)
```
   -   소켓 작업으로 인해 이벤트가 발생한 경우 소켓에 DataReader를 만들고 판독기를 비동기적으로 로드한 다음 앱의 디자인에 따라 데이터를 사용합니다. 추가 소켓 활동에 대해 다시 알림을 받으려면 소켓의 소유권을 다시 소켓 브로커에 반환해야 합니다.

   다음 예제에서는 소켓에서 받은 텍스트가 알림에 표시됩니다.

```csharp
case SocketActivityTriggerReason.SocketActivity:
            var socket = socketInformation.StreamSocket;
            DataReader reader = new DataReader(socket.InputStream);
            reader.InputStreamOptions = InputStreamOptions.Partial;
            await reader.LoadAsync(250);
            var dataString = reader.ReadString(reader.UnconsumedBufferLength);
            ShowToast(dataString);
            socket.TransferOwnership(socketInformation.Id); /* Important! */
            break;
```

   -   연결 유지 타이머가 만료되었으므로 이벤트가 발생한 경우 코드는 소켓의 연결을 유지하고 연결 유지 타이머를 다시 시작하기 위해 소켓에 일부 데이터를 보내야 합니다. 다시 말하자면 추가 이벤트 알림을 받기 위해 소켓의 소유권을 다시 소켓 브로커에 반환하는 것이 중요합니다.

```csharp
case SocketActivityTriggerReason.KeepAliveTimerExpired:
            socket = socketInformation.StreamSocket;
            DataWriter writer = new DataWriter(socket.OutputStream);
            writer.WriteBytes(Encoding.UTF8.GetBytes("Keep alive"));
            await writer.StoreAsync();
            writer.DetachStream();
            writer.Dispose();
            socket.TransferOwnership(socketInformation.Id); /* Important! */
            break;
```

   -   소켓이 닫혔으므로 이벤트가 발생한 경우 소켓을 다시 설정하고 새 소켓을 만든 후 소켓의 소유권을 소켓 브로커로 이전합니다. 이 샘플에서는 새 소켓 연결을 설정하는데 사용할 수 있도록 호스트 이름 및 포트가 로컬 설정에 저장됩니다.

```csharp
case SocketActivityTriggerReason.SocketClosed:
            socket = new StreamSocket();
            socket.EnableTransferOwnership(taskInstance.Task.TaskId, SocketActivityConnectedStandbyAction.Wake);
            if (ApplicationData.Current.LocalSettings.Values["hostname"] == null)
            {
                break;
            }
            var hostname = (String)ApplicationData.Current.LocalSettings.Values["hostname"];
            var port = (String)ApplicationData.Current.LocalSettings.Values["port"];
            await socket.ConnectAsync(new HostName(hostname), port);
            socket.TransferOwnership(socketId);
            break;
```

   -   이벤트 알림 처리를 완료한 후에 지연을 완료하는 것을 잊지 마세요.

```csharp
  deferral.Complete();
```

[**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009) 및 소켓 브로커 사용을 보여 주는 전체 샘플은 [SocketActivityStreamSocket 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620606)을 참조하세요. 소켓 초기화는 Scenario1\_Connect.xaml.cs에서 수행되며 백그라운드 작업 구현은 SocketActivityTask.cs에서 수행됩니다.

이 항목에서 설명한 대로 작업을 수행하기 위해 샘플에서는 새 소켓을 만들거나 기존 소켓을 획득하자마자 **OnSuspending** 이벤트 처리기를 사용하는 대신 **TransferOwnership**을 호출합니다. 이는 샘플은 [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009)를 설명하는 데 중점을 두고 있으며 실행되는 동안 다른 활동에 대해 소켓을 사용하지 않기 때문입니다. 앱이 더 복잡해질 수 있으며 **OnSuspending**을 호출하는 시기를 결정할 때 **TransferOwnership**을 사용해야 합니다.

## <a name="control-channel-triggers"></a>컨트롤 채널 트리거
먼저 CCT(컨트롤 채널 트리거)를 적절하게 사용하고 있는지 확인합니다. [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319), [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)또는 [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) 연결을 사용 중인 경우 [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009)를 사용 하도록 권장 합니다. **StreamSocket**에 대해 CCT를 사용할 수 있지만 이러한 CCT는 더 많은 리소스를 사용하고 연결된 대기 상태 모드에서 작동하지 않을 수 있습니다.

WebSockets, [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151), [**System.Net.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)또는 [**Windows.Web.Http.HttpClient**](/uwp/api/windows.web.http.httpclient)를 사용 하는 경우 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)사용 해야 합니다.

## <a name="controlchanneltrigger-with-websockets"></a>ControlChannelTrigger와 WebSockets
[**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 또는 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)을 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)와 함께 사용할 경우 몇 가지 특별히 고려해야 하는 사항이 있습니다. **MessageWebSocket** 또는 **StreamWebSocket**을 **ControlChannelTrigger**와 함께 사용할 때 따라야 하는 몇 가지 전송별 사용 패턴과 모범 사례가 있습니다. 또한 이러한 고려 사항은 **StreamWebSocket**에서 패킷을 수신하는 요청이 처리되는 방식에도 영향을 줍니다. **MessageWebSocket**에서 패킷을 수신하는 요청은 영향을 받지 않습니다.

[**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 또는 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)을 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)와 함께 사용할 때는 다음과 같은 사용 패턴과 모범 사례를 따라야 합니다.

-   중요한 소켓 수신은 항상 알림으로 전달되어야 합니다. 그래야만 푸시 알림 작업이 발생할 수 있습니다.
-   WebSocket 프로토콜은 keep-alive 메시지에 대한 표준 모델을 정의합니다. [**WebSocketKeepAlive**](https://msdn.microsoft.com/library/windows/apps/hh701531) 클래스는 클라이언트에서 시작한 WebSocket 프로토콜 keep-alive 메시지를 사용자에게 보낼 수 있습니다. **WebSocketKeepAlive** 클래스는 앱에 의해 KeepAliveTrigger에 대한 TaskEntryPoint로 등록되어야 합니다.

일부 고려 사항은 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)에서 패킷을 수신하는 요청이 처리되는 방식에도 영향을 줍니다. 특히 **StreamWebSocket**을 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)와 함께 사용할 경우 앱은 읽기를 처리하기 위해 C# 및 VB.NET의 **await** 모델이나 C++의 Tasks 대신 원시 비동기 패턴을 사용해야 합니다. 원시 비동기 패턴은 이 섹션의 뒷부분에 나오는 코드 샘플에서 확인할 수 있습니다.

원시 비동기 패턴을 사용하면 Windows에서 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)에 대한 백그라운드 작업의 [**IBackgroundTask.Run**](https://msdn.microsoft.com/library/windows/apps/br224811) 메서드를 수신 완료 콜백의 반환값과 동기화할 수 있습니다. **Run** 메서드는 모든 완료 콜백이 반환된 후에 호출됩니다. 따라서 앱은 **Run** 메서드가 호출되기 전에 데이터/오류를 받습니다.

앱은 다른 읽기를 게시해야만 완료 콜백에서 컨트롤을 반환받습니다. 또한 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 또는[**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 전송에서 [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119)를 직접 사용하면 안 됩니다. 그럴 경우 위에서 설명한 동기화가 중단되기 때문입니다. 전송에서 [**DataReader.LoadAsync**](https://msdn.microsoft.com/library/windows/apps/br208135) 메서드를 직접 사용하는 것은 지원되지 않습니다. 대신 [**StreamWebSocket.InputStream**](https://msdn.microsoft.com/library/windows/apps/br226936) 속성의 [**IInputStream.ReadAsync**](https://msdn.microsoft.com/library/windows/apps/br241719) 메서드에서 반환된 [**IBuffer**](https://msdn.microsoft.com/library/windows/apps/br241656)를 나중에 [**DataReader.FromBuffer**](https://msdn.microsoft.com/library/windows/apps/br208133) 메서드에 전달하여 더 처리할 수 있습니다.

다음 샘플에서는 원시 비동기 패턴을 사용하여 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)에서 읽기를 처리하는 방법을 보여 줍니다.

```csharp
void PostSocketRead(int length)
{
    try
    {
        var readBuf = new Windows.Storage.Streams.Buffer((uint)length);
        var readOp = socket.InputStream.ReadAsync(readBuf, (uint)length, InputStreamOptions.Partial);
        readOp.Completed = (IAsyncOperationWithProgress<IBuffer, uint>
            asyncAction, AsyncStatus asyncStatus) =>
        {
            switch (asyncStatus)
            {
                case AsyncStatus.Completed:
                case AsyncStatus.Error:
                    try
                    {
                        // GetResults in AsyncStatus::Error is called as it throws a user friendly error string.
                        IBuffer localBuf = asyncAction.GetResults();
                        uint bytesRead = localBuf.Length;
                        readPacket = DataReader.FromBuffer(localBuf);
                        OnDataReadCompletion(bytesRead, readPacket);
                    }
                    catch (Exception exp)
                    {
                        Diag.DebugPrint("Read operation failed:  " + exp.Message);
                    }
                    break;
                case AsyncStatus.Canceled:

                    // Read is not cancelled in this sample.
                    break;
           }
       };
   }
   catch (Exception exp)
   {
       Diag.DebugPrint("failed to post a read failed with error:  " + exp.Message);
   }
}
```

읽기 완료 처리기는 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)에 대한 백그라운드 작업의 [**IBackgroundTask.Run**](https://msdn.microsoft.com/library/windows/apps/br224811) 메서드가 호출되기 전에 발생합니다. Windows의 내부 동기화는 앱이 읽기 완료 콜백에서 반환될 때까지 대기합니다. 앱은 일반적으로 읽기 완료 콜백의 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 또는 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)에서 데이터 또는 오류를 신속하게 처리합니다. 메시지 자체는 **IBackgroundTask.Run** 메서드의 컨텍스트 내에서 처리됩니다. 아래 샘플에서는 읽기 완료 처리기가 메시지를 삽입하고 백그라운드 작업이 나중에 처리하는 메시지 큐를 사용하여 이에 대해 설명합니다.

다음 샘플에서는 원시 비동기 패턴을 사용하여 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)에서 읽기를 처리하는 읽기 완료 처리기를 보여 줍니다.

```csharp
public void OnDataReadCompletion(uint bytesRead, DataReader readPacket)
{
    if (readPacket == null)
    {
        Diag.DebugPrint("DataReader is null");

        // Ideally when read completion returns error,
        // apps should be resilient and try to
        // recover if there is an error by posting another recv
        // after creating a new transport, if required.
        return;
    }
    uint buffLen = readPacket.UnconsumedBufferLength;
    Diag.DebugPrint("bytesRead: " + bytesRead + ", unconsumedbufflength: " + buffLen);

    // check if buffLen is 0 and treat that as fatal error.
    if (buffLen == 0)
    {
        Diag.DebugPrint("Received zero bytes from the socket. Server must have closed the connection.");
        Diag.DebugPrint("Try disconnecting and reconnecting to the server");
        return;
    }

    // Perform minimal processing in the completion
    string message = readPacket.ReadString(buffLen);
    Diag.DebugPrint("Received Buffer : " + message);

    // Enqueue the message received to a queue that the push notify
    // task will pick up.
    AppContext.messageQueue.Enqueue(message);

    // Post another receive to ensure future push notifications.
    PostSocketRead(MAX_BUFFER_LENGTH);
}
```

Websockets에 대한 추가 정보는 keep-alive 처리기입니다. WebSocket 프로토콜은 keep-alive 메시지에 대한 표준 모델을 정의합니다.

[**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 또는 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)을 사용할 경우 [**WebSocketKeepAlive**](https://msdn.microsoft.com/library/windows/apps/hh701531) 클래스 인스턴스를 KeepAliveTrigger에 대한 [**TaskEntryPoint**](https://msdn.microsoft.com/library/windows/apps/br224774)로 등록해야 앱이 일시 중단되지 않고 keep-alive 메시지를 주기적으로 서버(원격 끝점)에 전송할 수 있습니다. 이 작업은 패키지 매니페스트에서뿐 아니라 라운드 등록 앱 코드의 일부로도 수행해야 합니다.

[**Windows.Sockets.WebSocketKeepAlive**](https://msdn.microsoft.com/library/windows/apps/hh701531)의 이 작업 진입점을 다음 두 곳에서 지정해야 합니다.

-   KeepAliveTrigger 트리거를 만들 때 원본 코드에서(아래 예제 참조)
-   앱 패키지 매니페스트의 keepalive 백그라운드 작업 선언에서

다음 샘플은 앱 매니페스트의 &lt;Application&gt; 요소 아래에 네트워크 트리거 알림 및 keepalive 트리거를 추가합니다.

```xml
  <Extensions>
    <Extension Category="windows.backgroundTasks"
         Executable="$targetnametoken$.exe"
         EntryPoint="Background.PushNotifyTask">
      <BackgroundTasks>
        <Task Type="controlChannel" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks"
         Executable="$targetnametoken$.exe"
         EntryPoint="Windows.Networking.Sockets.WebSocketKeepAlive">
      <BackgroundTasks>
        <Task Type="controlChannel" />
      </BackgroundTasks>
    </Extension>
  </Extensions>
```

앱은 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)의 컨텍스트에서 **await** 문을 사용하고 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923), [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 또는 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)에서 비동기 작업을 사용할 때 특히 주의해야 합니다. **Task&lt;bool&gt;** 개체는 **StreamWebSocket**의 푸시 알림과 WebSocket keep-alive에 대한 **ControlChannelTrigger**를 등록하고 전송을 연결하는 데 사용할 수 있습니다. 등록 과정에서 **StreamWebSocket** 전송은 **ControlChannelTrigger**에 대한 전송으로 설정되고 읽기가 게시됩니다. **Task.Result**는 작업의 모든 단계가 실행되고 메시지 본문에 문을 반환할 때까지 현재 스레드를 차단합니다. 메서드가 true 또는 false를 반환해야 작업이 해결됩니다. 이렇게 하면 전체 메서드가 실행됩니다. **Task**는 **Task**에서 보호되는 **await** 문을 여러 개 포함할 수 있습니다. **StreamWebSocket** 또는 **MessageWebSocket**이 전송으로 사용되는 경우 이 패턴을 **ControlChannelTrigger** 개체와 함께 사용해야 합니다. 완료하는 데 오랜 시간이 걸리는 작업(예: 일반적인 비동기 읽기 작업)의 경우 앱이 앞에서 설명한 원시 비동기 패턴을 사용해야 합니다.

다음 샘플에서는 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)의 푸시 알림 및 WebSocket keep-alive에 대한 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)를 등록합니다.

```csharp
private bool RegisterWithControlChannelTrigger(string serverUri)
{
    // Make sure the objects are created in a system thread
    // Demonstrate the core registration path
    // Wait for the entire operation to complete before returning from this method.
    // The transport setup routine can be triggered by user control, by network state change
    // or by keepalive task
    Task<bool> registerTask = RegisterWithCCTHelper(serverUri);
    return registerTask.Result;
}

async Task<bool> RegisterWithCCTHelper(string serverUri)
{
    bool result = false;
    socket = new StreamWebSocket();

    // Specify the keepalive interval expected by the server for this app
    // in order of minutes.
    const int serverKeepAliveInterval = 30;

    // Specify the channelId string to differentiate this
    // channel instance from any other channel instance.
    // When background task fires, the channel object is provided
    // as context and the channel id can be used to adapt the behavior
    // of the app as required.
    const string channelId = "channelOne";

    // For websockets, the system does the keepalive on behalf of the app
    // But the app still needs to specify this well known keepalive task.
    // This should be done here in the background registration as well
    // as in the package manifest.
    const string WebSocketKeepAliveTask = "Windows.Networking.Sockets.WebSocketKeepAlive";

    // Try creating the controlchanneltrigger if this has not been already
    // created and stored in the property bag.
    ControlChannelTriggerStatus status;

    // Create the ControlChannelTrigger object and request a hardware slot for this app.
    // If the app is not on LockScreen, then the ControlChannelTrigger constructor will
    // fail right away.
    try
    {
        channel = new ControlChannelTrigger(channelId, serverKeepAliveInterval,
                                   ControlChannelTriggerResourceType.RequestHardwareSlot);
    }
    catch (UnauthorizedAccessException exp)
    {
        Diag.DebugPrint("Is the app on lockscreen? " + exp.Message);
        return result;
    }

    Uri serverUriInstance;
    try
    {
        serverUriInstance = new Uri(serverUri);
    }
    catch (Exception exp)
    {
        Diag.DebugPrint("Error creating URI: " + exp.Message);
        return result;
    }

    // Register the apps background task with the trigger for keepalive.
    var keepAliveBuilder = new BackgroundTaskBuilder();
    keepAliveBuilder.Name = "KeepaliveTaskForChannelOne";
    keepAliveBuilder.TaskEntryPoint = WebSocketKeepAliveTask;
    keepAliveBuilder.SetTrigger(channel.KeepAliveTrigger);
    keepAliveBuilder.Register();

    // Register the apps background task with the trigger for push notification task.
    var pushNotifyBuilder = new BackgroundTaskBuilder();
    pushNotifyBuilder.Name = "PushNotificationTaskForChannelOne";
    pushNotifyBuilder.TaskEntryPoint = "Background.PushNotifyTask";
    pushNotifyBuilder.SetTrigger(channel.PushNotificationTrigger);
    pushNotifyBuilder.Register();

    // Tie the transport method to the ControlChannelTrigger object to push enable it.
    // Note that if the transport' s TCP connection is broken at a later point of time,
    // the ControlChannelTrigger object can be reused to plug in a new transport by
    // calling UsingTransport API again.
    try
    {
        channel.UsingTransport(socket);

        // Connect the socket
        //
        // If connect fails or times out it will throw exception.
        // ConnectAsync can also fail if hardware slot was requested
        // but none are available
        await socket.ConnectAsync(serverUriInstance);

        // Call WaitForPushEnabled API to make sure the TCP connection has
        // been established, which will mean that the OS will have allocated
        // any hardware slot for this TCP connection.
        //
        // In this sample, the ControlChannelTrigger object was created by
        // explicitly requesting a hardware slot.
        //
        // On systems that without connected standby, if app requests hardware slot as above,
        // the system will fallback to a software slot automatically.
        //
        // On systems that support connected standby,, if no hardware slot is available, then app
        // can request a software slot by re-creating the ControlChannelTrigger object.
        status = channel.WaitForPushEnabled();
        if (status != ControlChannelTriggerStatus.HardwareSlotAllocated
            && status != ControlChannelTriggerStatus.SoftwareSlotAllocated)
        {
            throw new Exception(string.Format("Neither hardware nor software slot could be allocated. ChannelStatus is {0}", status.ToString()));
        }

        // Store the objects created in the property bag for later use.
        CoreApplication.Properties.Remove(channel.ControlChannelTriggerId);

        var appContext = new AppContext(this, socket, channel, channel.ControlChannelTriggerId);
        ((IDictionary<string, object>)CoreApplication.Properties).Add(channel.ControlChannelTriggerId, appContext);
        result = true;

        // Almost done. Post a read since we are using streamwebsocket
        // to allow push notifications to be received.
        PostSocketRead(MAX_BUFFER_LENGTH);
    }
    catch (Exception exp)
    {
         Diag.DebugPrint("RegisterWithCCTHelper Task failed with: " + exp.Message);

         // Exceptions may be thrown for example if the application has not
         // registered the background task class id for using real time communications
         // broker in the package manifest.
    }
    return result
}
```

[**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 또는 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)을 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)와 함께 사용하는 방법에 대해서는 [ControlChannelTrigger StreamWebSocket 샘플](http://go.microsoft.com/fwlink/p/?linkid=251232)을 참조하세요.

## <a name="controlchanneltrigger-with-httpclient"></a>ControlChannelTrigger와 HttpClient
[HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637)를 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)와 함께 사용할 경우 몇 가지 특별히 고려해야 하는 사항이 있습니다. [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637)를 **ControlChannelTrigger**와 함께 사용할 때 따라야 하는 몇 가지 전송별 사용 패턴과 모범 사례가 있습니다. 또한 이러한 고려 사항은 [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637)에서 패킷을 수신하는 요청이 처리되는 방식에도 영향을 줍니다.

**참고** [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) SSL을 사용 하는 네트워크 트리거 기능 및 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)사용 하 여 현재 지원 되지 않습니다.
 
[HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637)을 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)와 함께 사용할 때는 다음과 같은 사용 패턴과 모범 사례를 따라야 합니다.

-   앱은 특정 URI로 요청을 보내기 전에 [System.Net.Http](http://go.microsoft.com/fwlink/p/?linkid=227894) 네임스페이스에서 [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) 또는 [HttpClientHandler](http://go.microsoft.com/fwlink/p/?linkid=241638) 개체에 여러 속성 및 헤더를 설정해야 합니다.
-   앱은 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)와 함께 사용할 [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) 전송을 만들기 전에 초기 요청을 만들어 전송을 테스트하고 적절하게 설정해야 합니다. 앱에서 전송이 적절히 설정되었다고 판단하면 [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) 개체는 **ControlChannelTrigger**와 함께 사용할 전송 개체로 구성될 수 있습니다. 이 프로세스는 일부 시나리오에서 전송을 통해 설정된 연결이 끊어지는 것을 방지하기 위해 설계되었습니다. SSL 인증서를 사용할 경우, 또는 선택할 수 있는 인증서가 여러 개인 경우 PIN 입력에 사용할 대화 상자가 앱에 필요할 수 있습니다. 또한 프록시 인증 및 서버 인증이 필요할 수 있습니다. 프록시 또는 서버 인증이 만료되면 연결이 닫힙니다. 앱에서 이러한 인증 만료 문제를 처리할 수 있는 방법은 타이머를 설정하는 것입니다. HTTP 리디렉션이 필요한 경우에는 두 번째 연결을 안정적으로 설정할 수 있는지 여부가 확실치 않습니다. 초기 테스트 요청은 **ControlChannelTrigger** 개체와 함께 전송으로서 [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) 개체를 사용하기 전에 앱이 가장 최근에 리디렉션된 URL을 사용할 수 있음을 보장합니다.

다른 네트워크 전송과 달리 [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) 개체는 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) 개체의 [**UsingTransport**](https://msdn.microsoft.com/library/windows/apps/hh701175) 메서드로 직접 전달할 수 없습니다. 대신 [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) 개체 및 **ControlChannelTrigger**와 함께 사용하기 위해 [HttpRequestMessage](http://go.microsoft.com/fwlink/p/?linkid=259153) 개체를 특별히 생성해야 합니다. [HttpRequestMessage](http://go.microsoft.com/fwlink/p/?linkid=259153) 개체는 [RtcRequestFactory.Create](http://go.microsoft.com/fwlink/p/?linkid=259154) 메서드를 사용하여 만듭니다. 그런 다음 만든 [HttpRequestMessage](http://go.microsoft.com/fwlink/p/?linkid=259153) 개체를 **UsingTransport** 메서드에 전달합니다.

다음 샘플에서는 [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) 개체 및 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)와 함께 사용할 [HttpRequestMessage](http://go.microsoft.com/fwlink/p/?linkid=259153) 개체를 생성하는 방법을 보여 줍니다.

```csharp
using System;
using System.Net;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;
using Windows.Networking.Sockets;

public HttpRequestMessage httpRequest;
public HttpClient httpClient;
public HttpRequestMessage httpRequest;
public ControlChannelTrigger channel;
public Uri serverUri;

private void SetupHttpRequestAndSendToHttpServer()
{
    try
    {
        // For HTTP based transports that use the RTC broker, whenever we send next request, we will abort the earlier
        // outstanding http request and start new one.
        // For example in case when http server is taking longer to reply, and keep alive trigger is fired in-between
        // then keep alive task will abort outstanding http request and start a new request which should be finished
        // before next keep alive task is triggered.
        if (httpRequest != null)
        {
            httpRequest.Dispose();
        }

        httpRequest = RtcRequestFactory.Create(HttpMethod.Get, serverUri);

        SendHttpRequest();
    }
        catch (Exception e)
    {
        Diag.DebugPrint("Connect failed with: " + e.ToString());
        throw;
    }
}
```

일부 특수 고려 사항은 응답 수신을 시작하는 [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637)의 HTTP 요청을 보내는 요청이 처리되는 방식에 영향을 줍니다. 특히 [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637)를 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)와 함께 사용할 경우 앱은 **await** 모델 대신 Task를 사용하여 보내기를 처리해야 합니다.

[HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637)를 사용하면 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)에 대한 백그라운드 작업의 [**IBackgroundTask.Run**](https://msdn.microsoft.com/library/windows/apps/br224811) 메서드를 수신 완료 콜백의 반환값과 동기화할 수 있습니다. 이 때문에 앱은 **Run** 메서드에서 차단 HttpResponseMessage 방법을 사용하고 전체 응답이 수신될 때까지 기다릴 수만 있습니다.

[HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637)를 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)와 함께 사용하면 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882), [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 또는 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 전송과 크게 다릅니다. [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) 수신 콜백은 [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) 코드 이후 Task를 통해 앱에 배달됩니다. 따라서 **ControlChannelTrigger** 푸시 알림 작업은 데이터 또는 오류가 앱에 디스패치되는 즉시 발생합니다. 아래 샘플 코드에서는 [HttpClient.SendAsync](http://go.microsoft.com/fwlink/p/?linkid=241637) 메서드에서 반환된 responseTask를 푸시 알림 작업이 인라인으로 선택 및 처리하는 글로벌 저장소에 저장합니다.

다음 샘플에서는 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)와 함께 사용할 경우 [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637)에서 보내기 요청을 처리하는 방법을 보여 줍니다.

```csharp
using System;
using System.Net;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;
using Windows.Networking.Sockets;

private void SendHttpRequest()
{
    if (httpRequest == null)
    {
        throw new Exception("HttpRequest object is null");
    }

    // Tie the transport method to the controlchanneltrigger object to push enable it.
    // Note that if the transport' s TCP connection is broken at a later point of time,
    // the controlchanneltrigger object can be reused to plugin a new transport by
    // calling UsingTransport API again.
    channel.UsingTransport(httpRequest);

    // Call the SendAsync function to kick start the TCP connection establishment
    // process for this http request.
    Task<HttpResponseMessage> httpResponseTask = httpClient.SendAsync(httpRequest);

    // Call WaitForPushEnabled API to make sure the TCP connection has been established,
    // which will mean that the OS will have allocated any hardware slot for this TCP connection.
    ControlChannelTriggerStatus status = channel.WaitForPushEnabled();
    Diag.DebugPrint("WaitForPushEnabled() completed with status: " + status);
    if (status != ControlChannelTriggerStatus.HardwareSlotAllocated
        && status != ControlChannelTriggerStatus.SoftwareSlotAllocated)
    {
        throw new Exception("Hardware/Software slot not allocated");
    }

    // The HttpClient receive callback is delivered via a Task to the app.
    // The notification task will fire as soon as the data or error is dispatched
    // Enqueue the responseTask returned by httpClient.sendAsync
    // into a queue that the push notify task will pick up and process inline.
    AppContext.messageQueue.Enqueue(httpResponseTask);
}
```

다음 샘플에서는 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)를 사용할 경우 [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637)에서 받은 응답을 읽는 방법을 보여 줍니다.

```csharp
using System.Net;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;

public string ReadResponse(Task<HttpResponseMessage> httpResponseTask)
{
    string message = null;
    try
    {
        if (httpResponseTask.IsCanceled || httpResponseTask.IsFaulted)
        {
            Diag.DebugPrint("Task is cancelled or has failed");
            return message;
        }
        // We' ll wait until we got the whole response.
        // This is the only supported scenario for HttpClient for ControlChannelTrigger.
        HttpResponseMessage httpResponse = httpResponseTask.Result;
        if (httpResponse == null || httpResponse.Content == null)
        {
            Diag.DebugPrint("Cannot read from httpresponse, as either httpResponse or its content is null. try to reset connection.");
        }
        else
        {
            // This is likely being processed in the context of a background task and so
            // synchronously read the Content' s results inline so that the Toast can be shown.
            // before we exit the Run method.
            message = httpResponse.Content.ReadAsStringAsync().Result;
        }
    }
    catch (Exception exp)
    {
        Diag.DebugPrint("Failed to read from httpresponse with error:  " + exp.ToString());
    }
    return message;
}
```

[HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637)를 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)와 함께 사용하는 방법에 대해서는 [ControlChannelTrigger HttpClient 샘플](http://go.microsoft.com/fwlink/p/?linkid=258323)을 참조하세요.

## <a name="controlchanneltrigger-with-ixmlhttprequest2"></a>ControlChannelTrigger와 IXMLHttpRequest2
[**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151)를 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)와 함께 사용할 경우 몇 가지 특별히 고려해야 하는 사항이 있습니다. **IXMLHTTPRequest2**를 **ControlChannelTrigger**와 함께 사용할 때 따라야 하는 몇 가지 전송별 사용 패턴과 모범 사례가 있습니다. **ControlChannelTrigger**를 사용하면 **IXMLHTTPRequest2**에서 HTTP 요청을 보내거나 받는 요청이 처리되는 방식에 영향을 주지 않습니다.

[**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151)를 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)와 함께 사용할 경우 사용 패턴 및 모법 사례

-   전송으로 사용될 때 [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151) 개체는 하나의 요청/응답 수명 주기만 갖습니다. [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) 개체와 함께 사용하면 한 번 **ControlChannelTrigger** 개체를 만들어 설정한 다음 새 **IXMLHTTPRequest2** 개체와 연결될 때마다 [**UsingTransport**](https://msdn.microsoft.com/library/windows/apps/hh701175) 메서드를 반복적으로 호출하는 데 편리합니다. 앱이 할당된 리소스 제한을 초과하지 않으려면 새 **IXMLHTTPRequest2** 개체를 제공하기 전에 이전 **IXMLHTTPRequest2** 개체를 삭제해야 합니다.
-   앱은 [**Send**](https://msdn.microsoft.com/library/windows/desktop/hh831164) 메서드를 호출하기 전에 [**SetProperty**](https://msdn.microsoft.com/library/windows/desktop/hh831167) 및 [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/desktop/hh831168) 메서드를 호출하여 HTTP 전송을 설정해야 합니다.
-   앱은 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)와 함께 사용할 전송을 만들기 전에 초기 [**Send**](https://msdn.microsoft.com/library/windows/desktop/hh831164) 요청을 만들어 전송을 테스트하고 적절하게 설정해야 합니다. 앱에서 전송이 적절히 설정되었다고 판단하면 [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151) 개체는 **ControlChannelTrigger**와 함께 사용할 전송 개체로 구성될 수 있습니다. 이 프로세스는 일부 시나리오에서 전송을 통해 설정된 연결이 끊어지는 것을 방지하기 위해 설계되었습니다. SSL 인증서를 사용할 경우, 또는 선택할 수 있는 인증서가 여러 개인 경우 PIN 입력에 사용할 대화 상자가 앱에 필요할 수 있습니다. 또한 프록시 인증 및 서버 인증이 필요할 수 있습니다. 프록시 또는 서버 인증이 만료되면 연결이 닫힙니다. 앱에서 이러한 인증 만료 문제를 처리할 수 있는 방법은 타이머를 설정하는 것입니다. HTTP 리디렉션이 필요한 경우에는 두 번째 연결을 안정적으로 설정할 수 있는지 여부가 확실치 않습니다. 초기 테스트 요청은 **ControlChannelTrigger** 개체와 함께 전송으로서 **IXMLHTTPRequest2** 개체를 사용하기 전에 앱이 가장 최근에 리디렉션된 URL을 사용할 수 있음을 보장합니다.

[**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151)를 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)와 함께 사용하는 방법에 대해서는 [IXMLHTTPRequest2와 함께 ControlChannelTrigger 사용 샘플](http://go.microsoft.com/fwlink/p/?linkid=258538)(영문)을 참조하세요.

## <a name="important-apis"></a>중요 API
* [SocketActivityTrigger](/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger)
* [ControlChannelTrigger](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger)
