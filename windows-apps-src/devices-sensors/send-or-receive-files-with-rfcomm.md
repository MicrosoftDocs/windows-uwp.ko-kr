---
ms.assetid: 5B3A6326-15EE-4618-AA8C-F1C7FB5232FB
title: Bluetooth RFCOMM
description: 이 문서에서는 파일을 보내거나 받는 방법에 대한 예제 코드와 함께 UWP(유니버설 Windows 플랫폼) 앱의 Bluetooth RFCOMM에 대한 개요를 제공합니다.
---
# Bluetooth RFCOMM

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

** 중요 API **

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.Rfcomm**](https://msdn.microsoft.com/library/windows/apps/Dn263529)

이 문서에서는 파일을 보내거나 받는 방법에 대한 예제 코드와 함께 UWP(유니버설 Windows 플랫폼) 앱의 Bluetooth RFCOMM에 대한 개요를 제공합니다.

## 개요

[
            **instantiation**](https://msdn.microsoft.com/library/windows/apps/BR225654) 네임스페이스의 API는 [**Windows.Devices.Bluetooth.Rfcomm**](https://msdn.microsoft.com/library/windows/apps/Dn263529) 및 [**enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459)을 포함하여 Windows.Devices에 대한 기존 패턴을 기반으로 합니다. 데이터 읽기 및 쓰기는 [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/BR241791) 및 [**established data stream patterns**](https://msdn.microsoft.com/library/windows/apps/BR208119)의 개체를 사용하도록 디자인되었습니다. SDP(세션 설명 프로토콜) 특성마다 값과 필요한 형식이 있습니다. 그러나 일부 일반적인 장치에 값 형식이 잘못된 SDP 특성이 구현되어 있습니다. 또한 여러 RFCOMM 사용에는 추가 SDP 특성이 필요하지 않습니다. 이러한 이유로, 이 API는 개발자가 필요한 정보를 얻을 수 있는 구분 분석되지 않은 SDP 데이터에 대한 액세스를 제공합니다.

RFCOMM API에는 서비스 식별자 개념이 사용됩니다. 서비스 식별자는 128비트 GUID지만 일반적으로는 16비트 또는 32비트 정수로도 지정됩니다. RFCOMM API는 128비트 GUID는 물론 32비트 정수로도 지정 및 사용할 수 있도록 하는 서비스 식별자용 래퍼를 제공하지만, 16비트 정수는 제공하지 않습니다. 언어가 32비트 정수까지 자동으로 확대되고 식별자가 여전히 올바르게 생성될 수 있으므로 이는 API에 문제가 되지 않습니다.

앱은 백그라운드 작업으로 다단계 장치 작업을 수행할 수 있으므로 앱이 백그라운드로 이동되고 일시 중단된 경우에도 실행을 완료할 수 있습니다. 따라서 사용자가 앉아서 진행률 표시기를 지켜보지 않더라도 콘텐츠 동기화와 지속적인 설정 또는 펌웨어 변경 등 신뢰할 수 있는 장치 서비스가 가능합니다. 장치 서비스에는 [**DeviceServicingTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn297315)를 사용하고 콘텐츠 동기화에는 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn297337)를 사용합니다. 이러한 백그라운드 작업은 앱이 백그라운드에서 실행될 수 있는 시간을 제한하며, 무기한 작업이나 무기한 동기화를 허용하지 않습니다.

## 파일을 클라이언트로 보내기

파일을 보낼 때 가장 기본적인 시나리오는 원하는 서비스에 따라 연결된 장치에 연결하는 것입니다. 여기에는 다음 단계가 포함됩니다.

-   **RfcommDeviceService.GetDeviceSelector\*** 함수를 사용하여 원하는 서비스의 쌍으로 구성된 디바이스 인스턴스를 열거하는 데 사용할 수 있는 AQS 쿼리를 생성할 수 있습니다.
-   열거된 디바이스를 선택하고, [**RfcommDeviceService**](https://msdn.microsoft.com/library/windows/apps/Dn263463)를 만들고, 필요에 따라 SDP 특성을 읽습니다([**established data helpers**](https://msdn.microsoft.com/library/windows/apps/BR208119)를 사용하여 특성 데이터 구문 분석).
-   소켓을 만들고 [**RfcommDeviceService.ConnectionHostName**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.rfcomm.rfcommdeviceservice.connectionhostname.aspx) 및 [**RfcommDeviceService.ConnectionServiceName**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.rfcomm.rfcommdeviceservice.connectionservicename.aspx) 속성을 사용하여 적절한 매개 변수로 원격 장치 서비스에 [**StreamSocket.ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/Hh701504)합니다.
-   설정된 데이터 스트림 패턴을 따라 파일의 데이터 청크를 읽어와 소켓의 [**StreamSocket.OutputStream**](https://msdn.microsoft.com/library/windows/apps/BR226920)을 통해 장치로 보낼 수 있습니다.

```csharp
Windows.Devices.Bluetooth.RfcommDeviceService _service;
Windows.Networking.Sockets.StreamSocket _socket;

async void Initialize()
{
    // Enumerate devices with the object push service
    auto services =
        await Windows.Devices.Enumeration.DeviceInformation.FindAllAsync(
            RfcommDeviceService.GetDeviceSelector(
                RfcommServiceId.ObexObjectPush));

    if (services.Count > 0) 
    {
        // Initialize the target Bluetooth BR device
        auto service = await RfcommDeviceService.FromIdAsync(services[0].Id);

        // Check that the service meets this App’s minimum requirement
        if (SupportsProtection(service) &amp;&amp; IsCompatibleVersion(service))
        {
            _service = service;

            // Create a socket and connect to the target
            _socket = new StreamSocket();
            await _socket.ConnectAsync(
                _service.ConnectionHostName,
                _service.ConnectionServiceName,
                SocketProtectionLevel
                    .BluetoothEncryptionAllowNullAuthentication);

            // The socket is connected. At this point the App can wait for
            // the user to take some action, e.g. click a button to send a
            // file to the device, which could invoke the Picker and then
            // send the picked file. The transfer itself would use the
            // Sockets API and not the Rfcomm API, and so is omitted here for
            // brevity.
        }
    }
}

// This App requires a connection that is encrypted but does not care about
// whether its authenticated.
bool SupportsProtection(RfcommDeviceService service)
{
    switch (service.ProtectionLevel)
    {
    case SocketProtectionLevel.PlainSocket:
        if ((service.MaximumProtectionLevel == SocketProtectionLevel
                .BluetoothEncryptionWithAuthentication)
            || (service.MaximumProtectionLevel == SocketProtectionLevel
                .BluetoothEncryptionAllowNullAuthentication)
        {
            // The connection can be upgraded when opening the socket so the
            // App may offer UI here to notify the user that Windows may
            // prompt for a PIN exchange.
            return true;
        }
        else
        {
            // The connection cannot be upgraded so an App may offer UI here
            // to explain why a connection won’t be made.
            return false;
        }
    case SocketProtectionLevel.BluetoothEncryptionWithAuthentication:
        return true;
    case SocketProtectionLevel.BluetoothEncryptionAllowNullAuthentication:
        return true;
    }
    return false;
}

// This App relies on CRC32 checking available in version 2.0 of the service.
const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint MINIMUM_SERVICE_VERSION = 200;
bool IsCompatibleVersion(RfcommDeviceService service)
{
    auto attributes = await service.GetSdpRawAttributesAsync(
        BluetothCacheMode.Uncached);
    auto attribute = attributes[SERVICE_VERSION_ATTRIBUTE_ID];
    auto reader = DataReader.FromBuffer(attribute);

    // The first byte contains the attribute' s type
    byte attributeType = reader.ReadByte();
    if (attributeType == SERVICE_VERSION_ATTRIBUTE_TYPE)
    {
        // The remainder is the data
        uint version = reader.Uint32();
        return version >= MINIMUM_SERVICE_VERSION;
    }
}
```

```cpp
Windows::Devices::Bluetooth::RfcommDeviceService^ _service;
Windows::Networking::Sockets::StreamSocket^ _socket;

void Initialize()
{
    // Enumerate devices with the object push service
    create_task(
        Windows::Devices::Enumeration::DeviceInformation::FindAllAsync(
            RfcommDeviceService::GetDeviceSelector(
                RfcommServiceId::ObexObjectPush)))
    .then([](DeviceInformationCollection^ services)
    {
        if (services->Size > 0) 
        {
            // Initialize the target Bluetooth BR device
            create_task(RfcommDeviceService::FromIdAsync(services[0]->Id))
            .then([](RfcommDeviceService^ service)
            {
                // Check that the service meets this App’s minimum
                // requirement
                if (SupportsProtection(service)
                    &amp;&amp; IsCompatibleVersion(service))
                {
                    _service = service;

                    // Create a socket and connect to the target
                    _socket = ref new StreamSocket();
                    create_task(_socket->ConnectAsync(
                        _service->ConnectionHostName,
                        _service->ConnectionServiceName,
                        SocketProtectionLevel
                            ::BluetoothEncryptionAllowNullAuthentication)
                    .then([](void)
                    {
                        // The socket is connected. At this point the App can
                        // wait for the user to take some action, e.g. click
                        // a button to send a file to the device, which could
                        // invoke the Picker and then send the picked file.
                        // The transfer itself would use the Sockets API and
                        // not the Rfcomm API, and so is omitted here for
                        //brevity.
                    });
                }
            });
        }
    });
}

// This App requires a connection that is encrypted but does not care about
// whether its authenticated.
bool SupportsProtection(RfcommDeviceService^ service)
{
    switch (service->ProtectionLevel)
    {
    case SocketProtectionLevel->PlainSocket:
        if ((service->MaximumProtectionLevel == SocketProtectionLevel
                ::BluetoothEncryptionWithAuthentication)
            || (service->MaximumProtectionLevel == SocketProtectionLevel
                ::BluetoothEncryptionAllowNullAuthentication)
        {
            // The connection can be upgraded when opening the socket so the
            // App may offer UI here to notify the user that Windows may
            // prompt for a PIN exchange.
            return true;
        }
        else
        {
            // The connection cannot be upgraded so an App may offer UI here
            // to explain why a connection won’t be made.
            return false;
        }
    case SocketProtectionLevel::BluetoothEncryptionWithAuthentication:
        return true;
    case SocketProtectionLevel::BluetoothEncryptionAllowNullAuthentication:
        return true;
    }
    return false;
}

// This App relies on CRC32 checking available in version 2.0 of the service.
const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint MINIMUM_SERVICE_VERSION = 200;
bool IsCompatibleVersion(RfcommDeviceService^ service)
{
    auto attributes = await service->GetSdpRawAttributesAsync(
        BluetoothCacheMode::Uncached);
    auto attribute = attributes[SERVICE_VERSION_ATTRIBUTE_ID];
    auto reader = DataReader.FromBuffer(attribute);

    // The first byte contains the attribute' s type
    byte attributeType = reader->ReadByte();
    if (attributeType == SERVICE_VERSION_ATTRIBUTE_TYPE)
    {
        // The remainder is the data
        uint version = reader->Uint32();
        return version >= MINIMUM_SERVICE_VERSION;
    }
}
```

## 파일을 서버로 받기

또 다른 일반적인 RFCOMM 앱 시나리오는 PC에서 서비스를 호스팅하고 다른 장치를 위해 이 서비스를 표시하는 것입니다.

-   [
            **RfcommServiceProvider**](https://msdn.microsoft.com/library/windows/apps/Dn263511)를 만들어 원하는 서비스를 보급합니다.
-   필요에 따라 [**established data helpers**](https://msdn.microsoft.com/library/windows/apps/BR208119)를 사용하여 특성 데이터를 생성함으로써 SDP 특성을 설정하고 다른 장치에서 검색하도록 SDP 레코드를 알리기 시작합니다.
-   클라이언트 장치에 연결하기 위해 소켓 수신기를 만들어서 들어오는 연결 요청에 대한 수신 대기를 시작합니다.
-   연결이 수신되면 향후 처리를 위해 연결된 소켓을 저장합니다.
-   설정된 데이터 스트림 패턴을 따라서 소켓의 InputStream에서 데이터 청크를 읽어와 파일에 저장합니다.

```csharp
Windows.Devices.Bluetooth.RfcommServiceProvider _provider;

async void Initialize()
{
    // Initialize the provider for the hosted RFCOMM service
    _provider = await Windows.Devices.Bluetooth.
        RfcommServiceProvider.CreateAsync(RfcommServiceId.ObexObjectPush);

    // Create a listener for this service and start listening
    StreamSocketListener listener = new StreamSocketListener();
    listener.ConnectionReceived += OnConnectionReceived;
    await listener.BindServiceNameAsync(
        _provider.ServiceId.AsString(),
        SocketProtectionLevel
            .BluetoothEncryptionAllowNullAuthentication);

    // Set the SDP attributes and start advertising
    InitializeServiceSdpAttributes(_provider);
    _provider.StartAdvertising();
}

const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint SERVICE_VERSION = 200;
void InitializeServiceSdpAttributes(RfcommServiceProvider provider)
{
    auto writer = new Windows.Storage.Streams.DataWriter();

    // First write the attribute type
    writer.WriteByte(SERVICE_VERSION_ATTRIBUTE_TYPE)
    // Then write the data
    writer.WriteUint32(SERVICE_VERSION);
    
    auto data = writer.DetachBuffer();
    provider.SdpRawAttributes.Add(SERVICE_VERSION_ATTRIBUTE_ID, data);
}

void OnConnectionReceived(
    StreamSocketListener listener,
    StreamSocketListenerConnectionReceivedEventArgs args)
{
    // Stop advertising/listening so that we’re only serving one client
    _provider.StopAdvertising();
    await listener.Close();
    _socket = args.Socket;

    // The client socket is connected. At this point the App can wait for
    // the user to take some action, e.g. click a button to receive a file
    // from the device, which could invoke the Picker and then save the
    // received file to the picked location. The transfer itself would use
    // the Sockets API and not the Rfcomm API, and so is omitted here for
    // brevity.
}
```

```cpp
Windows::Devices::Bluetooth::RfcommServiceProvider^ _provider;

void Initialize()
{
    // Initialize the provider for the hosted RFCOMM service
    create_task(Windows::Devices::Bluetooth.
        RfcommServiceProvider::CreateAsync(
            RfcommServiceId::ObexObjectPush))
    .then([](RfcommServiceProvider^ provider) -> task<void> {
        _provider = provider;

        // Create a listener for this service and start listening
        auto listener = ref new StreamSocketListener();
        listener->ConnectionReceived += ref new TypedEventHandler<
                StreamSocketListener^,
                StreamSocketListenerConnectionReceivedEventArgs^>
           (&amp;OnConnectionReceived);
        return create_task(listener->BindServiceNameAsync(
            _provider->ServiceId->AsString(),
            SocketProtectionLevel
                ::BluetoothEncryptionAllowNullAuthentication));
    }).then([listener](void) {
        // Set the SDP attributes and start advertising
        InitializeServiceSdpAttributes(_provider);
        _provider->StartAdvertising();
    });
}

const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint SERVICE_VERSION = 200;
void InitializeServiceSdpAttributes(RfcommServiceProvider^ provider)
{
    auto writer = ref new Windows::Storage::Streams::DataWriter();

    // First write the attribute type
    writer->WriteByte(SERVICE_VERSION_ATTRIBUTE_TYPE)
    // Then write the data
    writer->WriteUint32(SERVICE_VERSION);
    
    auto data = writer->DetachBuffer();
    provider->SdpRawAttributes->Add(SERVICE_VERSION_ATTRIBUTE_ID, data);
}

void OnConnectionReceived(
    StreamSocketListener^ listener,
    StreamSocketListenerConnectionReceivedEventArgs^ args)
{
    // Stop advertising/listening so that we’re only serving one client
    _provider->StopAdvertising();
    create_task(listener->Close())
    .then([args](void) {
        _socket = args->Socket;

        // The client socket is connected. At this point the App can wait for
        // the user to take some action, e.g. click a button to receive a
        // file from the device, which could invoke the Picker and then save
        // the received file to the picked location. The transfer itself
        // would use the Sockets API and not the Rfcomm API, and so is
        // omitted here for brevity.
    });
}
```



<!--HONumber=Mar16_HO1-->


