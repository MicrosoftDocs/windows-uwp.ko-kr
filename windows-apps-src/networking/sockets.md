---
description: 소켓은 많은 네트워킹 프로토콜이 구현되는 하위 수준 데이터 전송 기술입니다. UWP는 연결이 오래되었거나 설정된 연결이 필요하지 않은지 여부에 관계없이 클라이언트-서버 또는 피어 투 피어 애플리케이션에 대한 TCP 및 UDP 소켓 클래스를 제공합니다.
title: 소켓
ms.assetid: 23B10A3C-E33F-4CD6-92CB-0FFB491472D6
ms.date: 06/03/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 42913aae69e5d049530d649c031351f4f3ab9ace
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75684970"
---
# <a name="sockets"></a>소켓
소켓은 많은 네트워킹 프로토콜이 구현되는 하위 수준 데이터 전송 기술입니다. UWP는 연결이 오래되었거나 설정된 연결이 필요하지 않은지 여부에 관계없이 클라이언트-서버 또는 피어 투 피어 애플리케이션에 대한 TCP 및 UDP 소켓 클래스를 제공합니다.

이 항목에서는 [**Windows.Networking.Sockets**](/uwp/api/Windows.Networking.Sockets) 네임스페이스에 있는 UWP(유니버설 Windows 플랫폼) 소켓 클래스를 사용하는 방법을 다룹니다. 하지만 UWP 앱에서 [Windows 소켓 2(Winsock)](https://docs.microsoft.com/windows/desktop/WinSock/windows-sockets-start-page-2)도 사용할 수 있습니다.

> [!NOTE]
> [네트워크 격리](https://docs.microsoft.com/previous-versions/windows/apps/hh770532(v=win.10))의 결과로 Windows는 로컬 루프백 주소(127.0.0.0)를 통해서나 로컬 IP 주소를 명시적으로 지정하여 동일한 머신에서 실행 중인 UWP 앱 두 개 사이의 소켓 연결(소켓 또는 WinSock) 설정을 허용하지 않습니다. UWP 앱이 서로 통신할 수 있도록 하는 메커니즘에 대한 자세한 내용은 [앱 간 통신](/windows/uwp/app-to-app/index)을 참조하세요.

## <a name="build-a-basic-tcp-socket-client-and-server"></a>기본 TCP 소켓 클라이언트 및 서버 빌드
TCP(Transmission Control Protocol) 소켓은 지속 시간이 긴 연결에 대해 각 방향으로 하위 수준 네트워크 데이터 전송을 제공합니다. TCP 소켓은 인터넷에서 사용되는 대부분의 네트워크 프로토콜에서 사용하는 기본 기능입니다. 기본 TCP 작업을 설명하기 위해 아래의 예제 코드는 에코 클라이언트 및 서버를 형성하기 위해 TCP를 통해 데이터를 주고 받는 [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket) 및 [**StreamSocketListener**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener)를 보여줍니다.

최대한 이동 부분이 적게 하고&mdash;현재 네트워크 격리 문제를 피하려면&mdash;새 프로젝트를 만들고 클라이언트 및 서버 코드 둘 다 동일한 프로젝트 아래에 놓습니다.

프로젝트에서 [앱 접근 권한 값을 선언](../packaging/app-capability-declarations.md)해야 합니다. 앱 패키지 매니페스트 원본 파일(`Package.appxmanifest` 파일)을 열고 접근 권한 값 탭에서 **프라이빗 네트워크(클라이언트 및 서버)** 를 확인합니다. `Package.appxmanifest` 태그에서 어떻게 표시되는지 보여줍니다.

```xml
<Capability Name="privateNetworkClientServer" />
```

인터넷을 통해 연결하는 경우 `privateNetworkClientServer` 대신 `internetClientServer`를 선언할 수 있습니다. **StreamSocket** 및 **StreamSocketListener** 둘 다 하나 또는 다른 기타 앱 접근 권한 값을 선언해야 합니다.

### <a name="an-echo-client-and-server-using-tcp-sockets"></a>TCP 소켓을 사용하는 에코 클라이언트 및 서버
[**StreamSocketListener**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener)를 구성하고 들어오는 TCP 연결을 듣기 시작합니다. 클라이언트가 **StreamSocketListener**와 연결을 설정할 때마다 [**StreamSocketListener.ConnectionReceived**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener.ConnectionReceived) 이벤트가 발생합니다.

또한 [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket)을 서버에 대한 연결을 설정하고, 요청을 보내고 응답을 받습니다.

이름이 `StreamSocketAndListenerPage`인 새 **페이지**를 만듭니다. `StreamSocketAndListenerPage.xaml`에 XAML 태그를 넣고 `StreamSocketAndListenerPage` 클래스 안에 명령적 코드를 넣습니다.

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <StackPanel>
        <TextBlock Margin="9.6,0" Style="{StaticResource TitleTextBlockStyle}" Text="TCP socket example"/>
        <TextBlock Margin="7.2,0,0,0" Style="{StaticResource HeaderTextBlockStyle}" Text="StreamSocket &amp; StreamSocketListener"/>
    </StackPanel>

    <Grid Grid.Row="1">
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        <TextBlock Margin="9.6" Style="{StaticResource SubtitleTextBlockStyle}" Text="client"/>
        <ListBox x:Name="clientListBox" Grid.Row="1" Margin="9.6"/>
        <TextBlock Grid.Column="1" Margin="9.6" Style="{StaticResource SubtitleTextBlockStyle}" Text="server"/>
        <ListBox x:Name="serverListBox" Grid.Column="1" Grid.Row="1" Margin="9.6"/>
    </Grid>
</Grid>
```

```csharp
// Every protocol typically has a standard port number. For example, HTTP is typically 80, FTP is 20 and 21, etc.
// For this example, we'll choose an arbitrary port number.
static string PortNumber = "1337";

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    this.StartServer();
    this.StartClient();
}

private async void StartServer()
{
    try
    {
        var streamSocketListener = new Windows.Networking.Sockets.StreamSocketListener();

        // The ConnectionReceived event is raised when connections are received.
        streamSocketListener.ConnectionReceived += this.StreamSocketListener_ConnectionReceived;

        // Start listening for incoming TCP connections on the specified port. You can specify any port that's not currently in use.
        await streamSocketListener.BindServiceNameAsync(StreamSocketAndListenerPage.PortNumber);

        this.serverListBox.Items.Add("server is listening...");
    }
    catch (Exception ex)
    {
        Windows.Networking.Sockets.SocketErrorStatus webErrorStatus = Windows.Networking.Sockets.SocketError.GetStatus(ex.GetBaseException().HResult);
        this.serverListBox.Items.Add(webErrorStatus.ToString() != "Unknown" ? webErrorStatus.ToString() : ex.Message);
    }
}

private async void StreamSocketListener_ConnectionReceived(Windows.Networking.Sockets.StreamSocketListener sender, Windows.Networking.Sockets.StreamSocketListenerConnectionReceivedEventArgs args)
{
    string request;
    using (var streamReader = new StreamReader(args.Socket.InputStream.AsStreamForRead()))
    {
        request = await streamReader.ReadLineAsync();
    }

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add(string.Format("server received the request: \"{0}\"", request)));

    // Echo the request back as the response.
    using (Stream outputStream = args.Socket.OutputStream.AsStreamForWrite())
    {
        using (var streamWriter = new StreamWriter(outputStream))
        {
            await streamWriter.WriteLineAsync(request);
            await streamWriter.FlushAsync();
        }
    }

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add(string.Format("server sent back the response: \"{0}\"", request)));

    sender.Dispose();

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add("server closed its socket"));
}

private async void StartClient()
{
    try
    {
        // Create the StreamSocket and establish a connection to the echo server.
        using (var streamSocket = new Windows.Networking.Sockets.StreamSocket())
        {
            // The server hostname that we will be establishing a connection to. In this example, the server and client are in the same process.
            var hostName = new Windows.Networking.HostName("localhost");

            this.clientListBox.Items.Add("client is trying to connect...");

            await streamSocket.ConnectAsync(hostName, StreamSocketAndListenerPage.PortNumber);

            this.clientListBox.Items.Add("client connected");

            // Send a request to the echo server.
            string request = "Hello, World!";
            using (Stream outputStream = streamSocket.OutputStream.AsStreamForWrite())
            {
                using (var streamWriter = new StreamWriter(outputStream))
                {
                    await streamWriter.WriteLineAsync(request);
                    await streamWriter.FlushAsync();
                }
            }

            this.clientListBox.Items.Add(string.Format("client sent the request: \"{0}\"", request));

            // Read data from the echo server.
            string response;
            using (Stream inputStream = streamSocket.InputStream.AsStreamForRead())
            {
                using (StreamReader streamReader = new StreamReader(inputStream))
                {
                    response = await streamReader.ReadLineAsync();
                }
            }

            this.clientListBox.Items.Add(string.Format("client received the response: \"{0}\" ", response));
        }

        this.clientListBox.Items.Add("client closed its socket");
    }
    catch (Exception ex)
    {
        Windows.Networking.Sockets.SocketErrorStatus webErrorStatus = Windows.Networking.Sockets.SocketError.GetStatus(ex.GetBaseException().HResult);
        this.clientListBox.Items.Add(webErrorStatus.ToString() != "Unknown" ? webErrorStatus.ToString() : ex.Message);
    }
}
```

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>
#include <winrt/Windows.UI.Core.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
#include <sstream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::StreamSocketListener m_streamSocketListener;
    Windows::Networking::Sockets::StreamSocket m_streamSocket;

public:
    void OnNavigatedTo(NavigationEventArgs const& /* e */)
    {
        StartServer();
        StartClient();
    }

private:
    IAsyncAction StartServer()
    {
        try
        {
            // The ConnectionReceived event is raised when connections are received.
            m_streamSocketListener.ConnectionReceived({ this, &StreamSocketAndListenerPage::OnConnectionReceived });

            // Start listening for incoming TCP connections on the specified port. You can specify any port that's not currently in use.
            // Every protocol typically has a standard port number. For example, HTTP is typically 80, FTP is 20 and 21, etc.
            // For this example, we'll choose an arbitrary port number.
            co_await m_streamSocketListener.BindServiceNameAsync(L"1337");
            serverListBox().Items().Append(winrt::box_value(L"server is listening..."));
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus{ Windows::Networking::Sockets::SocketError::GetStatus(ex.to_abi()) };
            serverListBox().Items().Append(webErrorStatus != Windows::Networking::Sockets::SocketErrorStatus::Unknown ? winrt::box_value(winrt::to_hstring((int32_t)webErrorStatus)) : winrt::box_value(winrt::to_hstring(ex.to_abi())));
        }
    }

    IAsyncAction OnConnectionReceived(Windows::Networking::Sockets::StreamSocketListener /* sender */, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs args)
    {
        try
        {
            auto socket{ args.Socket() }; // Keep the socket referenced, and alive.
            DataReader dataReader{ socket.InputStream() };

            unsigned int bytesLoaded = co_await dataReader.LoadAsync(sizeof(unsigned int));

            unsigned int stringLength = dataReader.ReadUInt32();
            bytesLoaded = co_await dataReader.LoadAsync(stringLength);
            winrt::hstring request = dataReader.ReadString(bytesLoaded);

            serverListBox().Dispatcher().RunAsync(CoreDispatcherPriority::Normal, [=]()
            {
                std::wstringstream wstringstream;
                wstringstream << L"server received the request: \"" << request.c_str() << L"\"";
                serverListBox().Items().Append(winrt::box_value(wstringstream.str().c_str()));
            });

            // Echo the request back as the response.
            DataWriter dataWriter{ socket.OutputStream() };
            dataWriter.WriteUInt32(request.size());
            dataWriter.WriteString(request);
            co_await dataWriter.StoreAsync();
            dataWriter.DetachStream();

            serverListBox().Dispatcher().RunAsync(CoreDispatcherPriority::Normal, [=]()
            {
                std::wstringstream wstringstream;
                wstringstream << L"server sent back the response: \"" << request.c_str() << L"\"";
                serverListBox().Items().Append(winrt::box_value(wstringstream.str().c_str()));
            });

            m_streamSocketListener = nullptr;

            serverListBox().Dispatcher().RunAsync(CoreDispatcherPriority::Normal, [=]()
            {
                serverListBox().Items().Append(winrt::box_value(L"server closed its socket"));
            });
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus{ Windows::Networking::Sockets::SocketError::GetStatus(ex.to_abi()) };
            serverListBox().Dispatcher().RunAsync(CoreDispatcherPriority::Normal, [=]()
            {
                serverListBox().Items().Append(webErrorStatus != Windows::Networking::Sockets::SocketErrorStatus::Unknown ? winrt::box_value(winrt::to_hstring((int32_t)webErrorStatus)) : winrt::box_value(winrt::to_hstring(ex.to_abi())));
            });
        }
    }

    IAsyncAction StartClient()
    {
        try
        {
            // Establish a connection to the echo server.

            // The server hostname that we will be establishing a connection to. In this example, the server and client are in the same process.
            Windows::Networking::HostName hostName{ L"localhost" };

            clientListBox().Items().Append(winrt::box_value(L"client is trying to connect..."));

            co_await m_streamSocket.ConnectAsync(hostName, L"1337");
            clientListBox().Items().Append(winrt::box_value(L"client connected"));

            // Send a request to the echo server.
            DataWriter dataWriter{ m_streamSocket.OutputStream() };
            winrt::hstring request{ L"Hello, World!" };
            dataWriter.WriteUInt32(request.size());
            dataWriter.WriteString(request);

            co_await dataWriter.StoreAsync();

            std::wstringstream wstringstream;
            wstringstream << L"client sent the request: \"" << request.c_str() << L"\"";
            clientListBox().Items().Append(winrt::box_value(wstringstream.str().c_str()));

            co_await dataWriter.FlushAsync();
            dataWriter.DetachStream();

            // Read data from the echo server.
            DataReader dataReader{ m_streamSocket.InputStream() };
            unsigned int bytesLoaded = co_await dataReader.LoadAsync(sizeof(unsigned int));
            unsigned int stringLength = dataReader.ReadUInt32();
            bytesLoaded = co_await dataReader.LoadAsync(stringLength);
            winrt::hstring response{ dataReader.ReadString(bytesLoaded) };

            wstringstream.str(L"");
            wstringstream << L"client received the response: \"" << response.c_str() << L"\"";
            clientListBox().Items().Append(winrt::box_value(wstringstream.str().c_str()));

            m_streamSocket = nullptr;

            clientListBox().Items().Append(winrt::box_value(L"client closed its socket"));
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus{ Windows::Networking::Sockets::SocketError::GetStatus(ex.to_abi()) };
            serverListBox().Items().Append(webErrorStatus != Windows::Networking::Sockets::SocketErrorStatus::Unknown ? winrt::box_value(winrt::to_hstring((int32_t)webErrorStatus)) : winrt::box_value(winrt::to_hstring(ex.to_abi())));
        }
    }
```

```cpp
#include <ppltasks.h>
#include <sstream>
...
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::StreamSocketListener^ streamSocketListener;
    Windows::Networking::Sockets::StreamSocket^ streamSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->StartServer();
        this->StartClient();
    }

private:
    void StartServer()
    {
        try
        {
            this->streamSocketListener = ref new Windows::Networking::Sockets::StreamSocketListener();

            // The ConnectionReceived event is raised when connections are received.
            streamSocketListener->ConnectionReceived += ref new TypedEventHandler<Windows::Networking::Sockets::StreamSocketListener^, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^>(this, &StreamSocketAndListenerPage::StreamSocketListener_ConnectionReceived);

            // Start listening for incoming TCP connections on the specified port. You can specify any port that's not currently in use.
            // Every protocol typically has a standard port number. For example, HTTP is typically 80, FTP is 20 and 21, etc.
            // For this example, we'll choose an arbitrary port number.
            Concurrency::create_task(streamSocketListener->BindServiceNameAsync(L"1337")).then(
                [=]
            {
                this->serverListBox->Items->Append(L"server is listening...");
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus = Windows::Networking::Sockets::SocketError::GetStatus(ex->HResult);
            this->serverListBox->Items->Append(webErrorStatus.ToString() != L"Unknown" ? webErrorStatus.ToString() : ex->Message);
        }
    }

    void StreamSocketListener_ConnectionReceived(Windows::Networking::Sockets::StreamSocketListener^ sender, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^ args)
    {
        try
        {
            auto dataReader = ref new DataReader(args->Socket->InputStream);

            Concurrency::create_task(dataReader->LoadAsync(sizeof(unsigned int))).then(
                [=](unsigned int bytesLoaded)
            {
                unsigned int stringLength = dataReader->ReadUInt32();
                Concurrency::create_task(dataReader->LoadAsync(stringLength)).then(
                    [=](unsigned int bytesLoaded)
                {
                    Platform::String^ request = dataReader->ReadString(bytesLoaded);
                    this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
                        [=]
                    {
                        std::wstringstream wstringstream;
                        wstringstream << L"server received the request: \"" << request->Data() << L"\"";
                        this->serverListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
                    }));

                    // Echo the request back as the response.
                    auto dataWriter = ref new DataWriter(args->Socket->OutputStream);
                    dataWriter->WriteUInt32(request->Length());
                    dataWriter->WriteString(request);
                    Concurrency::create_task(dataWriter->StoreAsync()).then(
                        [=](unsigned int)
                    {
                        dataWriter->DetachStream();

                        this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
                            [=]()
                        {
                            std::wstringstream wstringstream;
                            wstringstream << L"server sent back the response: \"" << request->Data() << L"\"";
                            this->serverListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
                        }));

                        delete this->streamSocketListener;
                        this->streamSocketListener = nullptr;

                        this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler([=]() {this->serverListBox->Items->Append(L"server closed its socket"); }));
                    });
                });
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus = Windows::Networking::Sockets::SocketError::GetStatus(ex->HResult);
            this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler([=]() {this->serverListBox->Items->Append(webErrorStatus.ToString() != L"Unknown" ? webErrorStatus.ToString() : ex->Message); }));
        }
    }

    void StartClient()
    {
        try
        {
            // Create the StreamSocket and establish a connection to the echo server.
            this->streamSocket = ref new Windows::Networking::Sockets::StreamSocket();

            // The server hostname that we will be establishing a connection to. In this example, the server and client are in the same process.
            auto hostName = ref new Windows::Networking::HostName(L"localhost");

            this->clientListBox->Items->Append(L"client is trying to connect...");

            Concurrency::create_task(this->streamSocket->ConnectAsync(hostName, L"1337")).then(
                [=](Concurrency::task< void >)
            {
                this->clientListBox->Items->Append(L"client connected");

                // Send a request to the echo server.
                auto dataWriter = ref new DataWriter(this->streamSocket->OutputStream);
                auto request = ref new Platform::String(L"Hello, World!");
                dataWriter->WriteUInt32(request->Length());
                dataWriter->WriteString(request);

                Concurrency::create_task(dataWriter->StoreAsync()).then(
                    [=](Concurrency::task< unsigned int >)
                {
                    std::wstringstream wstringstream;
                    wstringstream << L"client sent the request: \"" << request->Data() << L"\"";
                    this->clientListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));

                    Concurrency::create_task(dataWriter->FlushAsync()).then(
                        [=](Concurrency::task< bool >)
                    {
                        dataWriter->DetachStream();

                        // Read data from the echo server.
                        auto dataReader = ref new DataReader(this->streamSocket->InputStream);
                        Concurrency::create_task(dataReader->LoadAsync(sizeof(unsigned int))).then(
                            [=](unsigned int bytesLoaded)
                        {
                            unsigned int stringLength = dataReader->ReadUInt32();
                            Concurrency::create_task(dataReader->LoadAsync(stringLength)).then(
                                [=](unsigned int bytesLoaded)
                            {
                                Platform::String^ response = dataReader->ReadString(bytesLoaded);
                                this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
                                    [=]
                                {
                                    std::wstringstream wstringstream;
                                    wstringstream << L"client received the response: \"" << response->Data() << L"\"";
                                    this->clientListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));

                                    delete this->streamSocket;
                                    this->streamSocket = nullptr;

                                    this->clientListBox->Items->Append(L"client closed its socket");
                                }));
                            });
                        });
                    });
                });
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus = Windows::Networking::Sockets::SocketError::GetStatus(ex->HResult);
            this->serverListBox->Items->Append(webErrorStatus.ToString() != L"Unknown" ? webErrorStatus.ToString() : ex->Message);
        }
    }
```

## <a name="references-to-streamsockets-in-c-ppl-continuations-applies-to-ccx-primarily"></a>C++ PPL 연속에서 StreamSockets 참조(주로 C++/CX에 적용)
> [!NOTE]
> C++/WinRT 코루틴을 사용하는 경우, 값으로 매개 변수를 전달하며 이 문제는 적용되지 않습니다. 매개 변수-전달 권장 사항은 [C++/WinRT로 동시성 및 비동기 작업](/windows/uwp/cpp-and-winrt-apis/concurrency#parameter-passing)을 참조하세요.

[**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket?branch=live)은 해당 입력/출력 스트림에 활성 읽기/쓰기가 있는 한 지속됩니다(예: [**StreamSocketListener.ConnectionReceived**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener.ConnectionReceived) 이벤트 처리기에서 액세스할 수 있는 [**StreamSocketListenerConnectionReceivedEventArgs.Socket**](/uwp/api/windows.networking.sockets.streamsocketlistenerconnectionreceivedeventargs.Socket)). [**DataReader.LoadAsync**](/uwp/api/windows.storage.streams.datareader.loadasync)(또는 `ReadAsync/WriteAsync/StoreAsync`)를 호출하면 **LoadAsync**의 **완료됨** 완료 처리기(있는 경우) 실행이 완료될 때까지 소켓의 입력 스트림을 통해 소켓에 대한 참조를 가집니다.

병렬 패턴 라이브러리(PPL)는 작업 연속 인라인을 기본적으로 예약하지 않습니다. 즉, 연속 작업 추가(`task::then()`로)는 연속 작업이 인라인을 완료 처리기로 실행하는 것을 보장하지 않습니다.

```cpp
void StreamSocketListener_ConnectionReceived(Windows::Networking::Sockets::StreamSocketListener^ sender, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^ args)
{
    auto dataReader = ref new DataReader(args->Socket->InputStream);
    Concurrency::create_task(dataReader->LoadAsync(sizeof(unsigned int))).then(
        [=](unsigned int bytesLoaded)
    {
        // Work in here isn't guaranteed to execute inline as the completion handler of the LoadAsync.
    });
}
```

**StreamSocket**의 관점에서는 완료 처리기는 연속 본문이 실행되기 전에 실행을 완료하며 소켓은 폐기할 수 있습니다. 따라서 해당 연속 내에서 사용하고자 하는 경우 소켓이 폐기되지 않도록 하려면 람다 캡처를 통해 소켓을 직접, 또는 계속 연속 내부에서 `args->Socket`에 액세스하여 간접적으로 참조하여 사용하거나, 연속 작업을 강제로 인라인으로 적용해야 합니다. [StreamSocket 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/StreamSocket)에서 실행 중인 첫 번째 기술(람다 캡처) 볼 수 있습니다. 위 [기본 TCP 소켓 클라이언트 및 서버 빌드](#build-a-basic-tcp-socket-client-and-server) 섹션의 C++/CX 코드는 두 번째 기술을 사용하여&mdash;요청을 응답으로 다시 보내며 가장 안쪽의 연속 중 하나에서 `args->Socket`에 액세스합니다.

세 번째 방법은 응답을 다시 보내지 않는 경우 적절합니다. `task_continuation_context::use_synchronous_execution()` 옵션을 사용하여 PPL이 연속 본문 인라인을 실행하도록 할 수 있습니다. 작업을 수행하는 방법을 보여주는 코드 예는 다음과 같습니다.

```cpp
void StreamSocketListener_ConnectionReceived(Windows::Networking::Sockets::StreamSocketListener^ sender, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^ args)
{
    auto dataReader = ref new DataReader(args->Socket->InputStream);

    Concurrency::create_task(dataReader->LoadAsync(sizeof(unsigned int))).then(
        [=](unsigned int bytesLoaded)
    {
        unsigned int messageLength = dataReader->ReadUInt32();
        Concurrency::create_task(dataReader->LoadAsync(messageLength)).then(
            [=](unsigned int bytesLoaded)
        {
            Platform::String^ request = dataReader->ReadString(bytesLoaded);
            this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
                [=]
            {
                std::wstringstream wstringstream;
                wstringstream << L"server received the request: \"" << request->Data() << L"\"";
                this->serverListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
            }));
        });
    }, Concurrency::task_continuation_context::use_synchronous_execution());
}
```

이 동작은 [**Windows.Networking.Sockets**](/uwp/api/Windows.Networking.Sockets?branch=live) 네임스페이스의 모든 소켓 및 WebSockets 클래스에 적용됩니다. 하지만 클라이언트 측 시나리오는 대부분 구성원 변수에 소켓을 저장하므로 위에서 설명한 대로 문제는 [**StreamSocketListener.ConnectionReceived**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener.ConnectionReceived) 시나리오에 가장 적절합니다.

## <a name="build-a-basic-udp-socket-client-and-server"></a>기본 UDP 소켓 클라이언트 및 서버 빌드
UDP(User Datagram Protocol) 소켓은 어느 방향으로든 하위 수준 네트워크 데이터 전송을 제공하는 점에서 TCP 소켓과 비슷합니다. 하지만 TCP 소켓은 지속 시간이 긴 연결용이지만, UDP 소켓은 설정된 연결이 필요하지 않은 애플리케이션용입니다. UDP 소켓은 두 엔드포인트에서 연결을 유지하지 않으므로 원격 컴퓨터 간 네트워킹을 위한 빠르고 간단한 솔루션입니다. 그러나 UDP 소켓은 네트워크 패킷의 무결성이나 패킷이 원격 대상에 도착하는지 여부를 전혀 확인하지 않습니다. 따라서 이를 허용하도록 앱을 설계해야 합니다. UDP 소켓을 사용하는 애플리케이션의 몇 가지 예로는 로컬 네트워크 검색 및 로컬 채팅 클라이언트가 있습니다.

기본 UDP 작업을 설명하기 위해 아래의 예제 코드는 에코 클라이언트 및 서버를 형성하기 위해 UDP를 통해 데이터를 주고 받는 데 사용되는 [**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket) 클래스를 보여줍니다. 새 프로젝트를 만들고 클라이언트 및 서버 코드를 같은 프로젝트 아래에 넣습니다. TCP 소켓과 마찬가지로 **프라이빗 네트워크(클라이언트 및 서버)** 앱 접근 권한 값을 선언해야 합니다.

### <a name="an-echo-client-and-server-using-udp-sockets"></a>UDP 소켓을 사용하는 에코 클라이언트 및 서버
[**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket)을 구성하여 에코 서버 역할을 재생하고, 특정 포트 번호에 이를 연결하고, 수신 UDP 메시지를 듣고, 이를 다시 보냅니다. 소켓에서 메시지가 수신될 때 [**DatagramSocket.MessageReceived**](/uwp/api/Windows.Networking.Sockets.DatagramSocket.MessageReceived) 이벤트가 발생합니다.

다른 **DatagramSocket**을 구성하여 에코 클라이언트 역할을 재생하고, 특정 포트 번호에 이를 연결하고, UDP 메시지를 보내고, 응답을 수신합니다.

이름이 `DatagramSocketPage`인 새 **페이지**를 만듭니다. `DatagramSocketPage.xaml`에 XAML 태그를 넣고 `DatagramSocketPage` 클래스 안에 명령적 코드를 넣습니다.

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <StackPanel>
        <TextBlock Margin="9.6,0" Style="{StaticResource TitleTextBlockStyle}" Text="UDP socket example"/>
        <TextBlock Margin="7.2,0,0,0" Style="{StaticResource HeaderTextBlockStyle}" Text="DatagramSocket"/>
    </StackPanel>

    <Grid Grid.Row="1">
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        <TextBlock Margin="9.6" Style="{StaticResource SubtitleTextBlockStyle}" Text="client"/>
        <ListBox x:Name="clientListBox" Grid.Row="1" Margin="9.6"/>
        <TextBlock Grid.Column="1" Margin="9.6" Style="{StaticResource SubtitleTextBlockStyle}" Text="server"/>
        <ListBox x:Name="serverListBox" Grid.Column="1" Grid.Row="1" Margin="9.6"/>
    </Grid>
</Grid>
```

```csharp
// Every protocol typically has a standard port number. For example, HTTP is typically 80, FTP is 20 and 21, etc.
// For this example, we'll choose different arbitrary port numbers for client and server, since both will be running on the same machine.
static string ClientPortNumber = "1336";
static string ServerPortNumber = "1337";

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    this.StartServer();
    this.StartClient();
}

private async void StartServer()
{
    try
    {
        var serverDatagramSocket = new Windows.Networking.Sockets.DatagramSocket();

        // The ConnectionReceived event is raised when connections are received.
        serverDatagramSocket.MessageReceived += ServerDatagramSocket_MessageReceived;

        this.serverListBox.Items.Add("server is about to bind...");

        // Start listening for incoming TCP connections on the specified port. You can specify any port that's not currently in use.
        await serverDatagramSocket.BindServiceNameAsync(DatagramSocketPage.ServerPortNumber);

        this.serverListBox.Items.Add(string.Format("server is bound to port number {0}", DatagramSocketPage.ServerPortNumber));
    }
    catch (Exception ex)
    {
        Windows.Networking.Sockets.SocketErrorStatus webErrorStatus = Windows.Networking.Sockets.SocketError.GetStatus(ex.GetBaseException().HResult);
        this.serverListBox.Items.Add(webErrorStatus.ToString() != "Unknown" ? webErrorStatus.ToString() : ex.Message);
    }
}

private async void ServerDatagramSocket_MessageReceived(Windows.Networking.Sockets.DatagramSocket sender, Windows.Networking.Sockets.DatagramSocketMessageReceivedEventArgs args)
{
    string request;
    using (DataReader dataReader = args.GetDataReader())
    {
        request = dataReader.ReadString(dataReader.UnconsumedBufferLength).Trim();
    }

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add(string.Format("server received the request: \"{0}\"", request)));

    // Echo the request back as the response.
    using (Stream outputStream = (await sender.GetOutputStreamAsync(args.RemoteAddress, DatagramSocketPage.ClientPortNumber)).AsStreamForWrite())
    {
        using (var streamWriter = new StreamWriter(outputStream))
        {
            await streamWriter.WriteLineAsync(request);
            await streamWriter.FlushAsync();
        }
    }

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add(string.Format("server sent back the response: \"{0}\"", request)));

    sender.Dispose();

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add("server closed its socket"));
}

private async void StartClient()
{
    try
    {
        // Create the DatagramSocket and establish a connection to the echo server.
        var clientDatagramSocket = new Windows.Networking.Sockets.DatagramSocket();

        clientDatagramSocket.MessageReceived += ClientDatagramSocket_MessageReceived;

        // The server hostname that we will be establishing a connection to. In this example, the server and client are in the same process.
        var hostName = new Windows.Networking.HostName("localhost");

        this.clientListBox.Items.Add("client is about to bind...");

        await clientDatagramSocket.BindServiceNameAsync(DatagramSocketPage.ClientPortNumber);

        this.clientListBox.Items.Add(string.Format("client is bound to port number {0}", DatagramSocketPage.ClientPortNumber));

        // Send a request to the echo server.
        string request = "Hello, World!";
        using (var serverDatagramSocket = new Windows.Networking.Sockets.DatagramSocket())
        {
            using (Stream outputStream = (await serverDatagramSocket.GetOutputStreamAsync(hostName, DatagramSocketPage.ServerPortNumber)).AsStreamForWrite())
            {
                using (var streamWriter = new StreamWriter(outputStream))
                {
                    await streamWriter.WriteLineAsync(request);
                    await streamWriter.FlushAsync();
                }
            }
        }

        this.clientListBox.Items.Add(string.Format("client sent the request: \"{0}\"", request));
    }
    catch (Exception ex)
    {
        Windows.Networking.Sockets.SocketErrorStatus webErrorStatus = Windows.Networking.Sockets.SocketError.GetStatus(ex.GetBaseException().HResult);
        this.clientListBox.Items.Add(webErrorStatus.ToString() != "Unknown" ? webErrorStatus.ToString() : ex.Message);
    }
}

private async void ClientDatagramSocket_MessageReceived(Windows.Networking.Sockets.DatagramSocket sender, Windows.Networking.Sockets.DatagramSocketMessageReceivedEventArgs args)
{
    string response;
    using (DataReader dataReader = args.GetDataReader())
    {
        response = dataReader.ReadString(dataReader.UnconsumedBufferLength).Trim();
    }

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.clientListBox.Items.Add(string.Format("client received the response: \"{0}\"", response)));

    sender.Dispose();

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.clientListBox.Items.Add("client closed its socket"));
}
```

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>
#include <winrt/Windows.UI.Core.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
#include <sstream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::DatagramSocket m_clientDatagramSocket;
    Windows::Networking::Sockets::DatagramSocket m_serverDatagramSocket;

public:
    void OnNavigatedTo(NavigationEventArgs const& /* e */)
    {
        StartServer();
        StartClient();
    }

private:
    IAsyncAction StartServer()
    {
        try
        {
            // The ConnectionReceived event is raised when connections are received.
            m_serverDatagramSocket.MessageReceived({ this, &DatagramSocketPage::ServerDatagramSocket_MessageReceived });

            serverListBox().Items().Append(winrt::box_value(L"server is about to bind..."));

            // Start listening for incoming TCP connections on the specified port. You can specify any port that's not currently in use.
            co_await m_serverDatagramSocket.BindServiceNameAsync(L"1337");
            serverListBox().Items().Append(winrt::box_value(L"server is bound to port number 1337"));
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus{ Windows::Networking::Sockets::SocketError::GetStatus(ex.to_abi()) };
            serverListBox().Items().Append(webErrorStatus != Windows::Networking::Sockets::SocketErrorStatus::Unknown ? winrt::box_value(winrt::to_hstring((int32_t)webErrorStatus)) : winrt::box_value(winrt::to_hstring(ex.to_abi())));
        }
    }

    IAsyncAction ServerDatagramSocket_MessageReceived(Windows::Networking::Sockets::DatagramSocket sender, Windows::Networking::Sockets::DatagramSocketMessageReceivedEventArgs args)
    {
        DataReader dataReader{ args.GetDataReader() };
        winrt::hstring request{ dataReader.ReadString(dataReader.UnconsumedBufferLength()) };

        serverListBox().Dispatcher().RunAsync(CoreDispatcherPriority::Normal, [=]()
        {
            std::wstringstream wstringstream;
            wstringstream << L"server received the request: \"" << request.c_str() << L"\"";
            serverListBox().Items().Append(winrt::box_value(wstringstream.str().c_str()));
        });

        // Echo the request back as the response.
        IOutputStream outputStream = co_await sender.GetOutputStreamAsync(args.RemoteAddress(), L"1336");
        DataWriter dataWriter{ outputStream };
        dataWriter.WriteString(request);

        co_await dataWriter.StoreAsync();
        dataWriter.DetachStream();

        serverListBox().Dispatcher().RunAsync(CoreDispatcherPriority::Normal, [=]()
        {
            std::wstringstream wstringstream;
            wstringstream << L"server sent back the response: \"" << request.c_str() << L"\"";
            serverListBox().Items().Append(winrt::box_value(wstringstream.str().c_str()));

            m_serverDatagramSocket = nullptr;

            serverListBox().Items().Append(winrt::box_value(L"server closed its socket"));
        });
    }

    IAsyncAction StartClient()
    {
        try
        {
            m_clientDatagramSocket.MessageReceived({ this, &DatagramSocketPage::ClientDatagramSocket_MessageReceived });

            // Establish a connection to the echo server.
            // The server hostname that we will be establishing a connection to. In this example, the server and client are in the same process.
            Windows::Networking::HostName hostName{ L"localhost" };

            clientListBox().Items().Append(winrt::box_value(L"client is about to bind..."));

            co_await m_clientDatagramSocket.BindServiceNameAsync(L"1336");
            clientListBox().Items().Append(winrt::box_value(L"client is bound to port number 1336"));

            // Send a request to the echo server.
            Windows::Networking::Sockets::DatagramSocket serverDatagramSocket;
            IOutputStream outputStream = co_await m_serverDatagramSocket.GetOutputStreamAsync(hostName, L"1337");

            winrt::hstring request{ L"Hello, World!" };
            DataWriter dataWriter{ outputStream };
            dataWriter.WriteString(request);
            co_await dataWriter.StoreAsync();
            dataWriter.DetachStream();

            std::wstringstream wstringstream;
            wstringstream << L"client sent the request: \"" << request.c_str() << L"\"";
            clientListBox().Items().Append(winrt::box_value(wstringstream.str().c_str()));
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus{ Windows::Networking::Sockets::SocketError::GetStatus(ex.to_abi()) };
            serverListBox().Items().Append(webErrorStatus != Windows::Networking::Sockets::SocketErrorStatus::Unknown ? winrt::box_value(winrt::to_hstring((int32_t)webErrorStatus)) : winrt::box_value(winrt::to_hstring(ex.to_abi())));
        }
    }

    void ClientDatagramSocket_MessageReceived(Windows::Networking::Sockets::DatagramSocket const& /* sender */, Windows::Networking::Sockets::DatagramSocketMessageReceivedEventArgs const& args)
    {
        DataReader dataReader{ args.GetDataReader() };
        winrt::hstring response{ dataReader.ReadString(dataReader.UnconsumedBufferLength()) };
        clientListBox().Dispatcher().RunAsync(CoreDispatcherPriority::Normal, [=]()
        {
            std::wstringstream wstringstream;
            wstringstream << L"client received the response: \"" << response.c_str() << L"\"";
            clientListBox().Items().Append(winrt::box_value(wstringstream.str().c_str()));
        });

        m_clientDatagramSocket = nullptr;

        clientListBox().Dispatcher().RunAsync(CoreDispatcherPriority::Normal, [=]()
        {
            clientListBox().Items().Append(winrt::box_value(L"client closed its socket"));
        });
    }
```

```cpp
#include <ppltasks.h>
#include <sstream>
...
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::DatagramSocket^ clientDatagramSocket;
    Windows::Networking::Sockets::DatagramSocket^ serverDatagramSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->StartServer();
        this->StartClient();
    }

private:
    void StartServer()
    {
        try
        {
            this->serverDatagramSocket = ref new Windows::Networking::Sockets::DatagramSocket();

            // The ConnectionReceived event is raised when connections are received.
            this->serverDatagramSocket->MessageReceived += ref new TypedEventHandler<Windows::Networking::Sockets::DatagramSocket^, Windows::Networking::Sockets::DatagramSocketMessageReceivedEventArgs^>(this, &DatagramSocketPage::ServerDatagramSocket_MessageReceived);

            this->serverListBox->Items->Append(L"server is about to bind...");

            // Start listening for incoming TCP connections on the specified port. You can specify any port that's not currently in use.
            Concurrency::create_task(this->serverDatagramSocket->BindServiceNameAsync("1337")).then(
                [=]
            {
                this->serverListBox->Items->Append(L"server is bound to port number 1337");
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus = Windows::Networking::Sockets::SocketError::GetStatus(ex->HResult);
            this->serverListBox->Items->Append(webErrorStatus.ToString() != L"Unknown" ? webErrorStatus.ToString() : ex->Message);
        }
    }

    void ServerDatagramSocket_MessageReceived(Windows::Networking::Sockets::DatagramSocket^ sender, Windows::Networking::Sockets::DatagramSocketMessageReceivedEventArgs^ args)
    {
        DataReader^ dataReader = args->GetDataReader();
        Platform::String^ request = dataReader->ReadString(dataReader->UnconsumedBufferLength);
        this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
            [=]
        {
            std::wstringstream wstringstream;
            wstringstream << L"server received the request: \"" << request->Data() << L"\"";
            this->serverListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
        }));

        // Echo the request back as the response.
        Concurrency::create_task(sender->GetOutputStreamAsync(args->RemoteAddress, "1336")).then(
            [=](IOutputStream^ outputStream)
        {
            auto dataWriter = ref new DataWriter(outputStream);
            dataWriter->WriteString(request);
            Concurrency::create_task(dataWriter->StoreAsync()).then(
                [=](unsigned int)
            {
                dataWriter->DetachStream();

                this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
                    [=]()
                {
                    std::wstringstream wstringstream;
                    wstringstream << L"server sent back the response: \"" << request->Data() << L"\"";
                    this->serverListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));

                    delete this->serverDatagramSocket;
                    this->serverDatagramSocket = nullptr;

                    this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler([=]() {this->serverListBox->Items->Append(L"server closed its socket"); }));
                }));
            });
        });
    }

    void StartClient()
    {
        try
        {
            // Create the DatagramSocket and establish a connection to the echo server.
            this->clientDatagramSocket = ref new Windows::Networking::Sockets::DatagramSocket();

            this->clientDatagramSocket->MessageReceived += ref new TypedEventHandler<Windows::Networking::Sockets::DatagramSocket^, Windows::Networking::Sockets::DatagramSocketMessageReceivedEventArgs^>(this, &DatagramSocketPage::ClientDatagramSocket_MessageReceived);

            // The server hostname that we will be establishing a connection to. In this example, the server and client are in the same process.
            auto hostName = ref new Windows::Networking::HostName(L"localhost");

            this->clientListBox->Items->Append(L"client is about to bind...");

            Concurrency::create_task(this->clientDatagramSocket->BindServiceNameAsync("1336")).then(
                [=]
            {
                this->clientListBox->Items->Append(L"client is bound to port number 1336");
            });

            // Send a request to the echo server.
            auto serverDatagramSocket = ref new Windows::Networking::Sockets::DatagramSocket();
            Concurrency::create_task(serverDatagramSocket->GetOutputStreamAsync(hostName, "1337")).then(
                [=](IOutputStream^ outputStream)
            {
                auto request = ref new Platform::String(L"Hello, World!");
                auto dataWriter = ref new DataWriter(outputStream);
                dataWriter->WriteString(request);
                Concurrency::create_task(dataWriter->StoreAsync()).then(
                    [=](unsigned int)
                {
                    dataWriter->DetachStream();
                    std::wstringstream wstringstream;
                    wstringstream << L"client sent the request: \"" << request->Data() << L"\"";
                    this->clientListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
                });

            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus = Windows::Networking::Sockets::SocketError::GetStatus(ex->HResult);
            this->serverListBox->Items->Append(webErrorStatus.ToString() != L"Unknown" ? webErrorStatus.ToString() : ex->Message);
        }
    }

    void ClientDatagramSocket_MessageReceived(Windows::Networking::Sockets::DatagramSocket^ sender, Windows::Networking::Sockets::DatagramSocketMessageReceivedEventArgs^ args)
    {
        DataReader^ dataReader = args->GetDataReader();
        Platform::String^ response = dataReader->ReadString(dataReader->UnconsumedBufferLength);
        this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
            [=]
        {
            std::wstringstream wstringstream;
            wstringstream << L"client received the response: \"" << response->Data() << L"\"";
            this->clientListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
        }));

        delete this->clientDatagramSocket;
        this->clientDatagramSocket = nullptr;

        this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler([=]() {this->clientListBox->Items->Append(L"client closed its socket"); }));
    }
```

## <a name="background-operations-and-the-socket-broker"></a>백그라운드 작업 및 소켓 브로커
소켓 브로커와 컨트롤 채널 트리거를 사용하여 백그라운드에서 앱이 제대로 연결 또는 데이터를 받도록 할 수 있습니다. 자세한 내용은 [백그라운드에서의 네트워크 통신](network-communications-in-the-background.md)을 참조하세요.

## <a name="batched-sends"></a>일괄 처리된 전송
소켓과 관련된 스트림에 쓸 때 커널 모드(네트워크 스택이 위치함)에 대한 사용자 모드(코드)에서 전환이 발생합니다. 한 번에 많은 버퍼를 쓰면 이러한 반복되는 전환이 상당한 오버헤드로 이어집니다. 전송을 일괄 처리하는 것이 여러 데이터 버퍼를 보내고 오버헤드를 피하는 방법입니다. 이는 앱에서 VoIP, VPN 또는 많은 데이터를 가급적 효율적으로 이동하는 작업이 기타 작업을 수행하는 경우에 특히 유용합니다.

이 섹션은 [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket) 또는 연결된 [**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket)으로 사용할 수 있는 몇 가지 일괄 처리된 전송 기술을 보여줍니다.

기준을 얻기 위해 여러 버퍼를 비효율적인 방식으로 전송하는 방법을 살펴보겠습니다. **StreamSocket**을 사용하는 최소 데모는 다음과 같습니다.

```csharp
protected override async void OnNavigatedTo(NavigationEventArgs e)
{
    var streamSocketListener = new Windows.Networking.Sockets.StreamSocketListener();
    streamSocketListener.ConnectionReceived += this.StreamSocketListener_ConnectionReceived;
    await streamSocketListener.BindServiceNameAsync("1337");

    var streamSocket = new Windows.Networking.Sockets.StreamSocket();
    await streamSocket.ConnectAsync(new Windows.Networking.HostName("localhost"), "1337");
    this.SendMultipleBuffersInefficiently(streamSocket, "Hello, World!");
    //this.BatchedSendsCSharpOnly(streamSocket, "Hello, World!");
    //this.BatchedSendsAnyUWPLanguage(streamSocket, "Hello, World!");
}

private async void StreamSocketListener_ConnectionReceived(Windows.Networking.Sockets.StreamSocketListener sender, Windows.Networking.Sockets.StreamSocketListenerConnectionReceivedEventArgs args)
{
    using (var dataReader = new DataReader(args.Socket.InputStream))
    {
        dataReader.InputStreamOptions = InputStreamOptions.Partial;
        while (true)
        {
            await dataReader.LoadAsync(256);
            if (dataReader.UnconsumedBufferLength == 0) break;
            IBuffer requestBuffer = dataReader.ReadBuffer(dataReader.UnconsumedBufferLength);
            string request = Windows.Security.Cryptography.CryptographicBuffer.ConvertBinaryToString(Windows.Security.Cryptography.BinaryStringEncoding.Utf8, requestBuffer);
            Debug.WriteLine(string.Format("server received the request: \"{0}\"", request));
        }
    }
}

// This implementation incurs kernel transition overhead for each packet written.
private async void SendMultipleBuffersInefficiently(Windows.Networking.Sockets.StreamSocket streamSocket, string message)
{
    var packetsToSend = new List<IBuffer>();
    for (int count = 0; count < 5; ++count) { packetsToSend.Add(Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(message, Windows.Security.Cryptography.BinaryStringEncoding.Utf8)); }

    foreach (IBuffer packet in packetsToSend)
    {
        await streamSocket.OutputStream.WriteAsync(packet);
    }
}
```

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Security.Cryptography.h>
#include <winrt/Windows.Storage.Streams.h>
#include <winrt/Windows.UI.Core.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
#include <sstream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::StreamSocketListener m_streamSocketListener;
    Windows::Networking::Sockets::StreamSocket m_streamSocket;

public:
    IAsyncAction OnNavigatedTo(NavigationEventArgs /* e */)
    {
        m_streamSocketListener.ConnectionReceived({ this, &BatchedSendsPage::OnConnectionReceived });
        co_await m_streamSocketListener.BindServiceNameAsync(L"1337");

        co_await m_streamSocket.ConnectAsync(Windows::Networking::HostName{ L"localhost" }, L"1337");
        SendMultipleBuffersInefficientlyAsync(L"Hello, World!");
        //BatchedSendsAnyUWPLanguageAsync(L"Hello, World!");
    }

private:
    IAsyncAction OnConnectionReceived(Windows::Networking::Sockets::StreamSocketListener const& /* sender */, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs const& args)
    {
        DataReader dataReader{ args.Socket().InputStream() };
        dataReader.InputStreamOptions(Windows::Storage::Streams::InputStreamOptions::Partial);

        while (true)
        {
            unsigned int bytesLoaded = co_await dataReader.LoadAsync(256);
            if (bytesLoaded == 0) break;
            winrt::hstring message{ dataReader.ReadString(bytesLoaded) };
            ::OutputDebugString(message.c_str());
        }
    }

    // This implementation incurs kernel transition overhead for each packet written.
    IAsyncAction SendMultipleBuffersInefficientlyAsync(winrt::hstring message)
    {
        co_await winrt::resume_background();

        std::vector< IBuffer > packetsToSend;
        for (unsigned int count = 0; count < 5; ++count)
        {
            packetsToSend.push_back(Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(message, Windows::Security::Cryptography::BinaryStringEncoding::Utf8));
        }

        for (auto const& element : packetsToSend)
        {
            m_streamSocket.OutputStream().WriteAsync(element).get();
        }
    }
```

```cpp
#include <ppltasks.h>
#include <sstream>
...
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::StreamSocketListener^ streamSocketListener;
    Windows::Networking::Sockets::StreamSocket^ streamSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->streamSocketListener = ref new Windows::Networking::Sockets::StreamSocketListener();
        streamSocketListener->ConnectionReceived += ref new TypedEventHandler<Windows::Networking::Sockets::StreamSocketListener^, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^>(this, &BatchedSendsPage::StreamSocketListener_ConnectionReceived);
        Concurrency::create_task(this->streamSocketListener->BindServiceNameAsync(L"1337")).then(
            [=]
        {
            this->streamSocket = ref new Windows::Networking::Sockets::StreamSocket();
            Concurrency::create_task(this->streamSocket->ConnectAsync(ref new Windows::Networking::HostName(L"localhost"), L"1337")).then(
                [=](Concurrency::task< void >)
            {
                this->SendMultipleBuffersInefficiently(L"Hello, World!");
                // this->BatchedSendsAnyUWPLanguage(L"Hello, World!");
            }, Concurrency::task_continuation_context::use_synchronous_execution());
        });
    }

private:
    void StreamSocketListener_ConnectionReceived(Windows::Networking::Sockets::StreamSocketListener^ sender, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^ args)
    {
        auto dataReader = ref new DataReader(args->Socket->InputStream);
        dataReader->InputStreamOptions = Windows::Storage::Streams::InputStreamOptions::Partial;
        this->ReceiveStringRecurse(dataReader, args->Socket);
    }

    void ReceiveStringRecurse(DataReader^ dataReader, Windows::Networking::Sockets::StreamSocket^ streamSocket)
    {
        Concurrency::create_task(dataReader->LoadAsync(256)).then(
            [this, dataReader, streamSocket](unsigned int bytesLoaded)
        {
            if (bytesLoaded == 0) return;
            Platform::String^ message = dataReader->ReadString(bytesLoaded);
            ::OutputDebugString(message->Data());
            this->ReceiveStringRecurse(dataReader, streamSocket);
        });
    }

    // This implementation incurs kernel transition overhead for each packet written.
    void SendMultipleBuffersInefficiently(Platform::String^ message)
    {
        std::vector< IBuffer^ > packetsToSend{};
        for (unsigned int count = 0; count < 5; ++count)
        {
            packetsToSend.push_back(Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(message, Windows::Security::Cryptography::BinaryStringEncoding::Utf8));
        }

        for (auto element : packetsToSend)
        {
            Concurrency::create_task(this->streamSocket->OutputStream->WriteAsync(element)).wait();
        }
    }
```

더 효과적인 방법인 이 첫 번째 예는 C#을 사용하는 경우에만 적절합니다. `OnNavigatedTo`를 변경하여 `SendMultipleBuffersInefficiently` 또는 `SendMultipleBuffersInefficientlyAsync` 대신 `BatchedSendsCSharpOnly`를 호출합니다.

```csharp
// A C#-only technique for batched sends.
private async void BatchedSendsCSharpOnly(Windows.Networking.Sockets.StreamSocket streamSocket, string message)
{
    var packetsToSend = new List<IBuffer>();
    for (int count = 0; count < 5; ++count) { packetsToSend.Add(Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(message, Windows.Security.Cryptography.BinaryStringEncoding.Utf8)); }

    var pendingTasks = new System.Threading.Tasks.Task[packetsToSend.Count];

    for (int index = 0; index < packetsToSend.Count; ++index)
    {
        // track all pending writes as tasks, but don't wait on one before beginning the next.
        pendingTasks[index] = streamSocket.OutputStream.WriteAsync(packetsToSend[index]).AsTask();
        // Don't modify any buffer's contents until the pending writes are complete.
    }

    // Wait for all of the pending writes to complete.
    System.Threading.Tasks.Task.WaitAll(pendingTasks);
}
```

이 다음 예는 C#뿐 아니라 모든 UWP 언어에 대해 적절합니다. 이는 전송을 일괄 처리하는 [**StreamSocket.OutputStream**](/uwp/api/windows.networking.sockets.streamsocket.OutputStream) 및 [**DatagramSocket.OutputStream**](/uwp/api/windows.networking.sockets.datagramsocket.OutputStream)의 동작에 의존합니다. Windows 10 기준으로 이 기술은 출력 스트림의 모든 작업이 완료된 후에만 반환되는 해당 출력 스트림에 [**FlushAsync**](/uwp/api/windows.storage.streams.ioutputstream.FlushAsync)를 호출합니다.

```csharp
// An implementation of batched sends suitable for any UWP language.
private async void BatchedSendsAnyUWPLanguage(Windows.Networking.Sockets.StreamSocket streamSocket, string message)
{
    var packetsToSend = new List<IBuffer>();
    for (int count = 0; count < 5; ++count) { packetsToSend.Add(Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(message, Windows.Security.Cryptography.BinaryStringEncoding.Utf8)); }

    var pendingWrites = new IAsyncOperationWithProgress<uint, uint>[packetsToSend.Count];

    for (int index = 0; index < packetsToSend.Count; ++index)
    {
        // track all pending writes as tasks, but don't wait on one before beginning the next.
        pendingWrites[index] = streamSocket.OutputStream.WriteAsync(packetsToSend[index]);
        // Don't modify any buffer's contents until the pending writes are complete.
    }

    // Wait for all of the pending writes to complete. This step enables batched sends on the output stream.
    await streamSocket.OutputStream.FlushAsync();
}
```

```cppwinrt
// An implementation of batched sends suitable for any UWP language.
IAsyncAction BatchedSendsAnyUWPLanguageAsync(winrt::hstring message)
{
    std::vector< IBuffer > packetsToSend{};
    std::vector< IAsyncOperationWithProgress< unsigned int, unsigned int > > pendingWrites{};
    for (unsigned int count = 0; count < 5; ++count)
    {
        packetsToSend.push_back(Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(message, Windows::Security::Cryptography::BinaryStringEncoding::Utf8));
    }

    for (auto const& element : packetsToSend)
    {
        // track all pending writes as tasks, but don't wait on one before beginning the next.
        pendingWrites.push_back(m_streamSocket.OutputStream().WriteAsync(element));
        // Don't modify any buffer's contents until the pending writes are complete.
    }

    // Wait for all of the pending writes to complete. This step enables batched sends on the output stream.
    co_await m_streamSocket.OutputStream().FlushAsync();
}
```

```cpp
private:
    // An implementation of batched sends suitable for any UWP language.
    void BatchedSendsAnyUWPLanguage(Platform::String^ message)
    {
        std::vector< IBuffer^ > packetsToSend{};
        std::vector< IAsyncOperationWithProgress< unsigned int, unsigned int >^ >pendingWrites{};
        for (unsigned int count = 0; count < 5; ++count)
        {
            packetsToSend.push_back(Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(message, Windows::Security::Cryptography::BinaryStringEncoding::Utf8));
        }

        for (auto element : packetsToSend)
        {
            // track all pending writes as tasks, but don't wait on one before beginning the next.
            pendingWrites.push_back(this->streamSocket->OutputStream->WriteAsync(element));
            // Don't modify any buffer's contents until the pending writes are complete.
        }

        // Wait for all of the pending writes to complete. This step enables batched sends on the output stream.
        Concurrency::create_task(this->streamSocket->OutputStream->FlushAsync());
    }
```

코드에서 일괄 처리된 전송을 사용할 경우 몇 가지 중요한 제한 사항이 있습니다.

-   비동기 쓰기가 완료될 때까지 작성 중인 **IBuffer** 인스턴스의 내용을 수정할 수 없습니다.
-   **FlushAsync** 패턴은 **StreamSocket.OutputStream** 및 **DatagramSocket.OutputStream**에서만 작동합니다.
-   **FlushAsync** 패턴은 Windows 10 이상에서만 작동합니다.
-   그 밖의 경우에는 **FlushAsync** 패턴 대신 [**Task.WaitAll**](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task.waitall?view=netcore-2.0#System_Threading_Tasks_Task_WaitAll_System_Threading_Tasks_Task___)을 사용합니다.

## <a name="port-sharing-for-datagramsocket"></a>DatagramSocket에 대한 포트 공유
[**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket)을 구성하여 동일한 주소/포트에 바인딩된 다른 Win32 또는 UWP 멀티캐스트 소켓과 함께 사용할 수 있습니다. 소켓을 바인딩 또는 연결하기 전에 [**DatagramSocketControl.MulticastOnly**](/uwp/api/Windows.Networking.Sockets.DatagramSocketControl.MulticastOnly)`true`로 설정하여 이 작업을 수행할 수 있습니다. 이 [**DatagramSocket.Control**](/uwp/api/windows.networking.sockets.datagramsocket.Control) 속성을 통해 **DatagramSocket** 개체 자체에서 **DatagramSocketControl**의 인스턴스에 액세스할 수 있습니다.

## <a name="providing-a-client-certificate-with-the-streamsocket-class"></a>StreamSocket 클래스를 사용하여 클라이언트 인증서 제공
[**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket)은 클라이언트 앱에서 통신하는 서버를 인증하기 위해 SSL/TLS 사용을 지원합니다. 클라이언트 앱에서 자체적으로 SSL/TLS 클라이언트 인증서를 사용하여 서버에 인증해야 하는 경우도 있습니다. 소켓을 바인딩 또는 연결하기 전에(SSL/TLS 핸드셰이크가 시작하지 전에 설정해야 함) [**StreamSocketControl.ClientCertificate**](/uwp/api/windows.networking.sockets.streamsocketcontrol.ClientCertificate) 속성으로 클라이언트 인증서를 제공할 수 있습니다. 이 [**StreamSocket.Control**](/uwp/api/windows.networking.sockets.streamsocket.Control) 속성을 통해 **StreamSocket** 개체 자체에서 **StreamSocketControl**의 인스턴스에 액세스할 수 있습니다. 서버에서 클라이언트 인증서를 요청한 경우 Windows는 사용자가 제공한 클라이언트 인증서로 응답합니다.

이 최소 코드 예에 나타난 대로 [**SocketProtectionLevel**](/uwp/api/windows.networking.sockets.socketprotectionlevel)를 가지는 [**StreamSocket.ConnectAsync**](/uwp/api/windows.networking.sockets.streamsocket.connectasync)의 재정의를 사용합니다.

> [!IMPORTANT]
> 아래의 코드 예제에서 설명된 대로 이 코드가 작동하려면 프로젝트가 sharedUserCertificates 앱 접근 권한 값을 선언해야 합니다.

```csharp
// For this code to work, you need at least one certificate to be present in the user MY certificate store.
// Plugging a smartcard into a smartcard reader connected to your PC will achieve that.
// Also, your project needs to declare the sharedUserCertificates app capability.
var certificateQuery = new Windows.Security.Cryptography.Certificates.CertificateQuery();
certificateQuery.StoreName = "MY";
IReadOnlyList<Windows.Security.Cryptography.Certificates.Certificate> certificates = await Windows.Security.Cryptography.Certificates.CertificateStores.FindAllAsync(certificateQuery);
if (certificates.Count > 0)
{
    streamSocket.Control.ClientCertificate = certificates[0];
    await streamSocket.ConnectAsync(hostName, "1337", Windows.Networking.Sockets.SocketProtectionLevel.Tls12);
}
```

```cppwinrt
// For this code to work, you need at least one certificate to be present in the user MY certificate store.
// Plugging a smartcard into a smartcard reader connected to your PC will achieve that.
// Also, your project needs to declare the sharedUserCertificates app capability.
Windows::Security::Cryptography::Certificates::CertificateQuery certificateQuery;
certificateQuery.StoreName(L"MY");
IVectorView< Windows::Security::Cryptography::Certificates::Certificate > certificates = co_await Windows::Security::Cryptography::Certificates::CertificateStores::FindAllAsync(certificateQuery);

if (certificates.Size() > 0)
{
    m_streamSocket.Control().ClientCertificate(certificates.GetAt(0));
    co_await m_streamSocket.ConnectAsync(Windows::Networking::HostName{ L"localhost" }, L"1337", Windows::Networking::Sockets::SocketProtectionLevel::Tls12);
    ...
}
```

```cpp
// For this code to work, you need at least one certificate to be present in the user MY certificate store.
// Plugging a smartcard into a smartcard reader connected to your PC will achieve that.
// Also, your project needs to declare the sharedUserCertificates app capability.
auto certificateQuery = ref new Windows::Security::Cryptography::Certificates::CertificateQuery();
certificateQuery->StoreName = L"MY";
Concurrency::create_task(Windows::Security::Cryptography::Certificates::CertificateStores::FindAllAsync(certificateQuery)).then(
    [=](IVectorView< Windows::Security::Cryptography::Certificates::Certificate^ >^ certificates)
{
    if (certificates->Size > 0)
    {
        this->streamSocket->Control->ClientCertificate = certificates->GetAt(0);
        Concurrency::create_task(this->streamSocket->ConnectAsync(ref new Windows::Networking::HostName(L"localhost"), L"1337", Windows::Networking::Sockets::SocketProtectionLevel::Tls12)).then(
            [=]
        {
            ...
        });
    }
});
```

## <a name="handling-exceptions"></a>예외 처리
[**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket), [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket) 또는 [**StreamSocketListener**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener) 작업에서 발생한 오류는 **HRESULT** 값으로 반환됩니다. 이 **HRESULT** 값을 [**SocketError.GetStatus**](/uwp/api/windows.networking.sockets.socketerror.getstatus) 메서드로 전달하여 [**SocketErrorStatus**](/uwp/api/Windows.Networking.Sockets.SocketErrorStatus) 열거형 값으로 변환할 수 있습니다.

대부분의 **SocketErrorStatus** 열거형 값은 기본 Windows 소켓 작업에서 반환한 오류에 해당합니다. 앱은 특정 **SocketErrorStatus** 열거형 값을 전환하여 예외의 원인에 따라 앱 동작을 수정할 수 있습니다.

매개 변수 유효성 검사 오류의 경우 예외에서 **HRESULT**를 사용하여 오류에 대한 자세한 정보를 얻을 수 있습니다. 가능한 **HRESULT** 값은 `Winerror.h`에 나열되며, SDK 설치에서 확인할 수 있습니다(예를 들어 `C:\Program Files (x86)\Windows Kits\10\Include\<VERSION>\shared` 폴더에서). 대부분의 매개 변수 유효성 검사 오류에서 반환되는 **HRESULT**는 **E_INVALIDARG**입니다.

전달된 문자열이 유효한 호스트 이름이 아닌 경우 [**HostName**](/uwp/api/Windows.Networking.HostName) 생성자는 예외를 throw할 수 있습니다. 예를 들어, 허용되지 않는 문자를 포함하며 사용자가 앱에 호스트 이름을 입력한 것 같습니다. catch/try 블록 내에 **HostName**을 구성합니다. 그러한 방식으로 예외가 발생하면 앱에서 사용자에게 알리고 새 호스트 이름을 요청할 수 있습니다.

## <a name="important-apis"></a>중요 API
* [CertificateQuery](/uwp/api/windows.security.cryptography.certificates.certificatequery)
* [CertificateStores.FindAllAsync](/uwp/api/windows.security.cryptography.certificates.certificatestores.findallasync)
* [DatagramSocket](/uwp/api/Windows.Networking.Sockets.DatagramSocket)
* [DatagramSocket.BindServiceNameAsync](/uwp/api/windows.networking.sockets.datagramsocket.bindservicenameasync)
* [DatagramSocket.Control](/uwp/api/windows.networking.sockets.datagramsocket.Control)
* [DatagramSocket.GetOutputStreamAsync](/uwp/api/windows.networking.sockets.datagramsocket.getoutputstreamasync)
* [DatagramSocket.MessageReceived](/uwp/api/Windows.Networking.Sockets.DatagramSocket.MessageReceived)
* [DatagramSocketControl.MulticastOnly](/uwp/api/Windows.Networking.Sockets.DatagramSocketControl.MulticastOnly)
* [DatagramSocketMessageReceivedEventArgs](/uwp/api/windows.networking.sockets.datagramsocketmessagereceivedeventargs)
* [DatagramSocketMessageReceivedEventArgs.GetDataReader](/uwp/api/windows.networking.sockets.datagramsocketmessagereceivedeventargs.GetDataReader)
* [DataReader.LoadAsync](/uwp/api/windows.storage.streams.datareader.loadasync)
* [IOutputStream.FlushAsync](/uwp/api/windows.storage.streams.ioutputstream.FlushAsync)
* [SocketError.GetStatus](/uwp/api/windows.networking.sockets.socketerror.getstatus)
* [SocketErrorStatus](/uwp/api/Windows.Networking.Sockets.SocketErrorStatus)
* [SocketProtectionLevel](/uwp/api/windows.networking.sockets.socketprotectionlevel)
* [StreamSocket](/uwp/api/Windows.Networking.Sockets.StreamSocket)
* [StreamSocketControl.ClientCertificate](/uwp/api/windows.networking.sockets.streamsocketcontrol.ClientCertificate)
* [StreamSocket.ConnectAsync](/uwp/api/windows.networking.sockets.streamsocket.connectasync)
* [StreamSocket.InputStream](/uwp/api/windows.networking.sockets.streamsocket.InputStream)
* [StreamSocket.OutputStream](/uwp/api/windows.networking.sockets.streamsocket.OutputStream)
* [StreamSocketListener](/uwp/api/Windows.Networking.Sockets.StreamSocketListener)
* [StreamSocketListener.BindServiceNameAsync](/uwp/api/windows.networking.sockets.streamsocketlistener.bindservicenameasync)
* [StreamSocketListener.ConnectionReceived](/uwp/api/Windows.Networking.Sockets.StreamSocketListener.ConnectionReceived)
* [StreamSocketListenerConnectionReceivedEventArgs](/uwp/api/windows.networking.sockets.streamsocketlistenerconnectionreceivedeventargs)
* [Windows.Networking.Sockets](/uwp/api/Windows.Networking.Sockets)

## <a name="related-topics"></a>관련 항목
* [앱 간 통신](/windows/uwp/app-to-app/index)
* [C++/WinRT를 통한 동시성 및 비동기 작업](/windows/uwp/cpp-and-winrt-apis/concurrency)
* [네트워크 접근 권한 값을 설정하는 방법](https://docs.microsoft.com/previous-versions/windows/apps/hh770532(v=win.10))
* [Windows 소켓 2(Winsock)](https://docs.microsoft.com/windows/desktop/WinSock/windows-sockets-start-page-2)

## <a name="samples"></a>샘플
* [StreamSocket 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/StreamSocket)
