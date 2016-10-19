---
author: DelfCo
description: "UWP(유니버설 Windows 플랫폼) 개발자는 Windows.Networking.Sockets 및 Winsock을 둘 다 사용하여 다른 디바이스와 통신할 수 있습니다."
title: "소켓"
ms.assetid: 23B10A3C-E33F-4CD6-92CB-0FFB491472D6
translationtype: Human Translation
ms.sourcegitcommit: 4557fa59d377edc2ae5bf5a9be63516d152949bb
ms.openlocfilehash: 49a9ae4d7d3994ad7fbb78fc9dc60cdd9dca07c3

---

# 소켓

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

**중요 API**

-   [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960)
-   [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms740673)

UWP(유니버설 Windows 플랫폼) 개발자는 [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 및 [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms737523)을 둘 다 사용하여 다른 디바이스와 통신할 수 있습니다. 이 항목에서는 네트워킹 작업을 수행하기 위한 **Windows.Networking.Sockets** 네임스페이스 사용에 대해 자세한 지침을 제공합니다.

>**참고** [네트워크 격리](https://msdn.microsoft.com/library/windows/apps/hh770532.aspx)의 일부로 시스템에서는 로컬 루프백 주소(127.0.0.0)를 통해서나 로컬 IP 주소를 명시적으로 지정하여 동일한 시스템에서 실행 중인 UWP 앱 두 개 사이의 소켓 연결(소켓 또는 WinSock) 설정을 금지합니다. 두 UWP 앱 간의 통신을 위해 소켓을 사용할 수 없다는 것을 의미합니다. UWP는 앱 간 통신을 위해 다른 메커니즘을 제공합니다. 자세한 내용은 [앱 간 통신](https://msdn.microsoft.com/windows/uwp/app-to-app/index)을 참조하세요.

## 기본 TCP 소켓 작업

TCP 소켓은 수명이 긴 연결에 대해 각 방향으로 하위 수준 네트워크 데이터 전송을 제공합니다. TCP 소켓은 인터넷에서 사용되는 대부분의 네트워크 프로토콜에서 사용하는 기본 기능입니다. 이 섹션에서는 [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 네임스페이스의 일부로써 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 및 [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) 클래스를 사용하여 TCP 스트림 소켓과 함께 데이터를 보내고 받도록 UWP 앱을 설정하는 방법을 보여 줍니다. 이 섹션에서는 기본 TCP 작업을 보여 주기 위해 에코 서버 및 클라이언트의 역할을 할 아주 간단한 앱을 만들 것입니다.

**TCP 에코 서버 만들기**

다음 코드 예제는 [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) 개체를 만들고 들어오는 TCP 연결에 대해 수신을 시작하는 방법을 보여 줍니다.

```csharp
try
{
    //Create a StreamSocketListener to start listening for TCP connections.
    Windows.Networking.Sockets.StreamSocketListener socketListener = new Windows.Networking.Sockets.StreamSocketListener();

    //Hook up an event handler to call when connections are received.
    socketListener.ConnectionReceived += SocketListener_ConnectionReceived;

    //Start listening for incoming TCP connections on the specified port. You can specify any port that' s not currently in use.
    await socketListener.BindServiceNameAsync("1337");
}
catch (Exception e)
{
    //Handle exception.
}
```

다음 코드 예제는 위 예제에서 [**StreamSocketListener.ConnectionReceived**](https://msdn.microsoft.com/library/windows/apps/hh701494) 이벤트에 연결된 SocketListener\_ConnectionReceived 이벤트 처리기를 구현합니다. 이 이벤트 처리기는 원격 클라이언트가 에코 서버와 연결을 설정할 때마다 [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) 클래스에 의해 호출됩니다.

```csharp
private async void SocketListener_ConnectionReceived(Windows.Networking.Sockets.StreamSocketListener sender, 
    Windows.Networking.Sockets.StreamSocketListenerConnectionReceivedEventArgs args)
{
    //Read line from the remote client.
    Stream inStream = args.Socket.InputStream.AsStreamForRead();
    StreamReader reader = new StreamReader(inStream);
    string request = await reader.ReadLineAsync();
    
    //Send the line back to the remote client.
    Stream outStream = args.Socket.OutputStream.AsStreamForWrite();
    StreamWriter writer = new StreamWriter(outStream);
    await writer.WriteLineAsync(request);
    await writer.FlushAsync();
}
```

**TCP 에코 클라이언트 만들기**

다음 코드 예제는 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 개체를 만들고 원격 서버에 연결을 설정하고 요청을 보내고 응답을 받는 방법을 보여 줍니다.

```csharp
try
{
    //Create the StreamSocket and establish a connection to the echo server.
    Windows.Networking.Sockets.StreamSocket socket = new Windows.Networking.Sockets.StreamSocket();
    
    //The server hostname that we will be establishing a connection to. We will be running the server and client locally,
    //so we will use localhost as the hostname.
    Windows.Networking.HostName serverHost = new Windows.Networking.HostName("localhost");
    
    //Every protocol typically has a standard port number. For example HTTP is typically 80, FTP is 20 and 21, etc.
    //For the echo server/client application we will use a random port 1337.
    string serverPort = "1337";
    await socket.ConnectAsync(serverHost, serverPort);

    //Write data to the echo server.
    Stream streamOut = socket.OutputStream.AsStreamForWrite();
    StreamWriter writer = new StreamWriter(streamOut);
    string request = "test";
    await writer.WriteLineAsync(request);
    await writer.FlushAsync();

    //Read data from the echo server.
    Stream streamIn = socket.InputStream.AsStreamForRead();
    StreamReader reader = new StreamReader(streamIn);
    string response = await reader.ReadLineAsync();
}
catch (Exception e)
{
    //Handle exception here.            
}
```

## 기본 UDP 소켓 작업

UDP 소켓은 연결이 필요하지 않는 네트워크 통신에 대해 어느 방향에서나 하위 수준 네트워크 데이터 전송을 제공합니다. UDP 소켓은 두 끝점에서 연결을 유지하지 않으므로 원격 컴퓨터 간 네트워킹에 대해 빠르고 간단한 솔루션을 제공합니다. 그러나 UDP 소켓은 네트워크 패킷의 무결성이나 원격 대상에 도착하는지 여부를 전혀 확인하지 않습니다. UDP 소켓을 사용하는 응용 프로그램의 몇 가지 예로는 로컬 네트워크 검색 및 로컬 채팅 클라이언트가 있습니다. 이 섹션에서는 간단한 에코 서버 및 클라이언트를 만들어 UDP 메시지를 보내고 받기 위해 [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) 클래스를 사용하는 것을 보여 줍니다.

**UDP 에코 서버 만들기**

다음 코드 예제는 들어오는 UDP 메시지를 수신할 수 있도록 [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) 개체를 만들고 특정 포트에 바인딩하는 방법을 보여 줍니다.

```csharp
Windows.Networking.Sockets.DatagramSocket socket = new Windows.Networking.Sockets.DatagramSocket();

socket.MessageReceived += Socket_MessageReceived;

//You can use any port that is not currently in use already on the machine.
string serverPort = "1337";

//Bind the socket to the serverPort so that we can start listening for UDP messages from the UDP echo client.
await socket.BindServiceNameAsync(serverPort);
```

다음 코드 예제에서는 클라이언트에서 수신된 메시지를 읽고 같은 메시지를 다시 보내기 위해 **Socket\_MessageReceived** 이벤트 처리기를 구현합니다.

```csharp
private async void Socket_MessageReceived(Windows.Networking.Sockets.DatagramSocket sender, Windows.Networking.Sockets.DatagramSocketMessageReceivedEventArgs args)
{
    //Read the message that was received from the UDP echo client.
    Stream streamIn = args.GetDataStream().AsStreamForRead();
    StreamReader reader = new StreamReader(streamIn);
    string message = await reader.ReadLineAsync();

    //Create a new socket to send the same message back to the UDP echo client.
    Windows.Networking.Sockets.DatagramSocket socket = new Windows.Networking.Sockets.DatagramSocket();
    
    //Use a separate port number for the UDP echo client because both will be unning on the same machine.
    string clientPort = "1338"
    Stream streamOut = (await socket.GetOutputStreamAsync(args.RemoteAddress, clientPort)).AsStreamForWrite();
    StreamWriter writer = new StreamWriter(streamOut);
    await writer.WriteLineAsync(message);
    await writer.FlushAsync();
}
```

**UDP 에코 클라이언트 만들기**

다음 코드 예제는 들어오는 UDP 메시지를 수신하고 UDP 메시지를 UDP 에코 서버로 보낼 수 있도록 [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) 개체를 만들고 특정 포트에 바인딩하는 방법을 보여 줍니다.

```csharp
private async void testUdpSocketServer()
{
    Windows.Networking.Sockets.DatagramSocket socket = new Windows.Networking.Sockets.DatagramSocket();
    
    socket.MessageReceived += Socket_MessageReceived;
    
    //You can use any port that is not currently in use already on the machine. We will be using two separate and random 
    //ports for the client and server because both the will be running on the same machine.
    string serverPort = "1337";
    string clientPort = "1338";
    
    //Because we will be running the client and server on the same machine, we will use localhost as the hostname.
    Windows.Networking.HostName serverHost = new Windows.Networking.HostName("localhost");
    
    //Bind the socket to the clientPort so that we can start listening for UDP messages from the UDP echo server.
    await socket.BindServiceNameAsync(clientPort);
                
    //Write a message to the UDP echo server.
    Stream streamOut = (await socket.GetOutputStreamAsync(serverHost, serverPort)).AsStreamForWrite();
    StreamWriter writer = new StreamWriter(streamOut);
    string message = "Hello, world!";
    await writer.WriteLineAsync(message);
    await writer.FlushAsync();
}
```

다음 코드 예제에서는 UDP 에코 서버에서 수신된 메시지를 읽기 위해 **Socket\_MessageReceived** 이벤트 처리기를 구현합니다.

```csharp
private async void Socket_MessageReceived(Windows.Networking.Sockets.DatagramSocket sender, 
    Windows.Networking.Sockets.DatagramSocketMessageReceivedEventArgs args)
{
    //Read the message that was received from the UDP echo server.
    Stream streamIn = args.GetDataStream().AsStreamForRead();
    StreamReader reader = new StreamReader(streamIn);
    string message = await reader.ReadLineAsync();
}
```

## 백그라운드 작업 및 소켓 브로커

앱이 소켓에서 연결 또는 데이터를 받으면 앱이 포그라운드에 있지 않을 때 이러한 작업을 올바르게 수행할 수 있도록 준비해야 합니다. 이를 위해 소켓 브로커를 사용합니다. 소켓 브로커를 사용하는 방법에 대한 자세한 내용은 [백그라운드에서의 네트워크 통신](network-communications-in-the-background.md)을 참조하세요.

## 일괄 처리된 전송

Windows 10부터 Windows.Networking.Sockets에서 일괄 처리된 전송을 지원합니다. 이를 통해 각 버퍼를 개별적으로 전송하는 경우보다 훨씬 낮은 컨텍스트 전환 오버헤드로 여러 데이터 버퍼를 함께 전송할 수 있습니다. 이는 앱에서 VoIP, VPN 또는 많은 데이터를 가급적 효율적으로 이동하는 작업이 기타 작업을 수행하는 경우에 특히 유용합니다.

소켓에서 WriteAsync를 호출할 때마다 네트워크 스택에 도달하는 커널 전환이 트리거됩니다. 앱에서 한 번에 많은 버퍼를 작성할 경우 작성 시마다 별도의 커널 전환이 발생하고 이로 인해 상당한 오버헤드가 생성됩니다. 새로운 일괄 처리된 전송 패턴은 커널 전환 빈도를 최적화합니다. 이 기능은 현재 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 및 연결된 [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) 인스턴스로 제한됩니다.

다음은 앱에서 최적화되지 않은 방식으로 많은 버퍼를 전송하는 방법에 대한 예제입니다.

```csharp
// Send a set of packets inefficiently, one packet at a time.
// This is not recommended if you have many packets to send
IList<IBuffer> packetsToSend = PreparePackets();
var outputStream = stream.OutputStream;

foreach (IBuffer packet in packetsToSend)
{
    // Incurs kernel transition overhead for each packet
    await outputStream.WriteAsync(packet);
}
```

다음 예제에서는 많은 버퍼를 보다 효율적으로 전송하는 방법을 보여 줍니다. 이 기술은 C# 언어에 고유한 기능을 사용하므로 C# 프로그래머만 사용할 수 있습니다. 이 예제에서는 한 번에 여러 패킷을 보내 시스템에서 전송을 일괄 처리하고 커널 전환을 최적화하여 성능을 향상시킬 수 있도록 합니다.

```csharp
// More efficient way to send packets.
// This way enables the system to do batched sends
IList<IBuffer> packetsToSend = PreparePackets();
var outputStream = stream.OutputStream;

int i = 0;
Task[] pendingTasks = new Tast[packetsToSend.Count];
foreach (IBuffer packet in packetsToSend)
{
    // track all pending writes as tasks, but do not wait on one before peforming the next
    pendingTasks[i++] = outputStream.WriteAsync(packet).AsTask();
    // Do not modify any buffer' s contents until the pending writes are complete.
}
// Now, wait for all of the pending writes to complete
await Task.WaitAll(pendingTasks);
```

다음 예제에서는 일괄 처리된 전송과 호환되는 방식으로 많은 버퍼를 전송하는 또 다른 방법을 보여 줍니다. C# 관련 기능을 사용하지 않기 때문에 모든 언어에 적용됩니다(여기서는 C#으로 보여 줌). 대신 Windows 10의 새로운 기능인 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 및 [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) 클래스의 **OutputStream** 멤버에서 변경된 동작을 사용합니다.

```csharp
// More efficient way to send packets in Windows 10, using the new behavior of OutputStream.FlushAsync().
int i = 0;
IList<IBuffer> packetsToSend = PreparePackets();
var outputStream = socket.OutputStream;

var pendingWrites = new IAsyncOperationWithProgress<uint,uint> [packetsToSend.Count];

foreach (IBuffer packet in packetsToSend)
{
    pendingWrites[i++] = outputStream.WriteAsync(packet);
    // Do not modify any buffer' s contents until the pending writes are complete.
}

// Wait for all pending writes to complete. This step enables batched sends on the output stream.
await outputStream.FlushAsync();
```

이전 버전의 Windows에서는 **FlushAsync**가 즉시 반환되므로 스트림의 일부 작업이 완료되지 않을 수 있었습니다. Windows 10에서는 이 동작이 변경되었습니다. 이제 **FlushAsync**는 출력 스트림의 모든 작업이 완료된 후에 반환됩니다.

코드에서 일괄 처리된 쓰기를 사용할 경우 몇 가지 중요한 제한 사항이 있습니다.

-   비동기 쓰기가 완료될 때까지 작성 중인 **IBuffer** 인스턴스의 내용을 수정할 수 없습니다.
-   **FlushAsync** 패턴은 **StreamSocket.OutputStream** 및 **DatagramSocket.OutputStream**에서만 작동합니다.
-   **FlushAsync** 패턴은 Windows 10 이상에서만 작동합니다.
-   그 밖의 경우에는 **FlushAsync** 패턴 대신 **Task.WaitAll**을 사용합니다.

## DatagramSocket에 대한 포트 공유

Windows 10에는 새로운 [**DatagramSocketControl**](https://msdn.microsoft.com/library/windows/apps/hh701190) 속성인 [**MulticastOnly**](https://msdn.microsoft.com/library/windows/apps/dn895368)가 도입되었습니다. 이를 통해 해당 **DatagramSocket**을 같은 주소/포트에 바인딩된 Win32 또는 WinRT 멀티캐스트 소켓과 함께 사용할 수 있습니다.

## StreamSocket 클래스를 사용하여 클라이언트 인증서 제공

[**Windows.Networking.StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 클래스는 SSL/TLS를 사용하여 앱에서 통신하는 서버를 인증하도록 지원합니다. 앱에서 자체적으로 TLS 클라이언트 인증서를 사용하여 서버에 인증해야 하는 경우도 있습니다. Windows 10에서는 [**StreamSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226893) 개체에서 클라이언트 인증서를 제공할 수 있습니다(TLS 핸드셰이크를 시작하기 전에 설정해야 함). 서버에서 클라이언트 인증서를 요청한 경우 Windows에서 제공된 인증서로 응답합니다.

다음은 이를 구현하는 방법을 보여 주는 코드 조각입니다.

```csharp
var socket = new StreamSocket();
Windows.Security.Cryptography.Certificates.Certificate certificate = await GetClientCert();
socket.Control.ClientCertificate = certificate;
await socket.ConnectAsync(destination, SocketProtectionLevel.Tls12);
```

## Windows.Networking.Sockets의 예외

전달된 문자열이 유효한 호스트 이름이 아닌 경우(호스트 이름에 허용되지 않는 문자 포함) 소켓과 함께 사용된 [**HostName**](https://msdn.microsoft.com/library/windows/apps/br207113) 클래스에 대한 생성자에서 예외가 발생할 수 있습니다. 앱이 **HostName**에 대해 사용자 입력을 받으면 생성자는 try/catch 블록에 있게 됩니다. 예외가 발생하면 앱에서 사용자에게 알리고 새 호스트 이름을 요청할 수 있습니다.

[**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 네임 스페이스에는 소켓 및 WebSocket을 사용할 때 오류를 처리하는 편리한 도우미 메서드와 열거형이 있습니다. 특정 네트워크 예외를 앱에서 다르게 처리하는 데 유용합니다.

[**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319), [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 또는 [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) 작업에서 발생한 오류는 **HRESULT** 값으로 반환됩니다. [**SocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701462) 메서드는 네트워크 오류를 소켓 작업에서 [**SocketErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh701457) 열거형 값으로 변환하는 데 사용됩니다. 대부분의 **SocketErrorStatus** 열거형 값은 기본 Windows 소켓 작업에서 반환한 오류에 해당합니다. 앱은 특정 **SocketErrorStatus** 열거형 값을 필터링하여 예외의 원인에 따라 앱 동작을 수정할 수 있습니다.

[**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 또는 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 작업에서 발생한 오류는 **HRESULT** 값으로 반환됩니다. [**WebSocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701529) 메서드는 네트워크 오류를 WebSocket 작업에서 [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818) 열거형 값으로 변환하는 데 사용됩니다. 대부분의 **WebErrorStatus** 열거형 값은 기본 HTTP 클라이언트 작업에서 반환한 오류에 해당합니다. 앱은 특정 **WebErrorStatus** 열거형 값을 필터링하여 예외의 원인에 따라 앱 동작을 수정할 수 있습니다.

매개 변수 유효성 검사 오류의 경우 앱은 또한 예외에서 **HRESULT**를 사용하여 예외의 원인이 된 오류에 대한 더 자세한 정보를 알 수 있습니다. 가능한 **HRESULT** 값은 *Winerror.h* 헤더 파일에 나열되어 있습니다. 대부분의 매개 변수 유효성 검사 오류에서 반환되는 **HRESULT**는 **E\_INVALIDARG**입니다.

## Winsock API

UWP 앱에서도 [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms740673)을 사용할 수 있습니다. 지원되는 Winsock API는 Windows Phone 8.1Microsoft Silverlight의 Winsock API를 기반으로 하며, 대부분의 형식, 속성 및 메서드를 지원합니다(오래된 것으로 간주되는 일부 API는 제거됨). Winsock 프로그래밍에 대한 자세한 내용은 [여기](https://msdn.microsoft.com/library/windows/desktop/ms740673)에서 확인할 수 있습니다.





<!--HONumber=Aug16_HO3-->


