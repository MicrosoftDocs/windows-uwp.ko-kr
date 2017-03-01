---
author: msatranjr
ms.assetid: 28B30708-FE08-4BE9-AE11-5429F963C330
title: Bluetooth GATT
description: "이 문서에서는 세 가지 일반적인 GATT 시나리오에 대한 샘플 코드와 함께 UWP(유니버설 Windows 플랫폼) 앱의 Bluetooth GATT(일반 특성 프로필) 개요를 제공합니다."
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: c6187f4bfe6f2940b8dbfea0e6441f2fa9ac2c66
ms.lasthandoff: 02/07/2017

---
# <a name="bluetooth-gatt"></a>Bluetooth GATT

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [아카이브](http://go.microsoft.com/fwlink/p/?linkid=619132)를 참조하세요. \]

** 중요 API **

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)

이 문서에서는 세 가지 일반적인 GATT 시나리오, 즉 Bluetooth 데이터 검색, Bluetooth LE 온도계 장치 제어 및 Bluetooth LE 장치 데이터 프레젠테이션 제어에 대한 샘플 코드와 함께 UWP(유니버설 Windows 플랫폼) 앱의 Bluetooth 일반 특성 프로필(GATT)에 대한 개요를 제공합니다.

## <a name="overview"></a>개요

개발자는 [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685) 네임스페이스의 API를 사용하여 Bluetooth LE 서비스, 설명자 및 특성에 액세스할 수 있습니다. Bluetooth LE 장치는 다음 컬렉션을 통해 장치의 기능을 표시합니다.

-   기본 서비스
-   포함된 서비스
-   특성
-   설명자

기본 서비스는 LE 장치의 기능 계약을 정의하고 서비스를 정의하는 특성 컬렉션을 포함합니다. 그리고 이러한 특성에는 특성을 설명하는 설명자가 포함됩니다.

Bluetooth GATT API는 원시 전송에 대한 액세스보다는 개체와 함수를 표시합니다. 드라이버 수준에서 기본 서비스는 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API를 사용하여 Bluetooth LED 장치의 자식 장치 노드로 열거됩니다.

Bluetooth GATT API를 통해 개발자는 다음 작업을 수행할 수 있는 Bluetooth LED 장치도 사용할 수 있습니다.

-   서비스/특성/설명서 검색 수행
-   특성/설명자 값 읽기 및 쓰기
-   특성의 ValueChanged 이벤트에 대한 콜백 등록

Bluetooth GATT API는 일반적인 속성을 처리하고 장치 관리 및 구성을 지원할 적당한 기본값을 제공하여 개발을 간소화합니다. 이러한 API는 앱에서 Bluetooth LE 장치의 기능에 액세스할 수 있는 수단을 개발자에게 제공합니다.

유용한 구현을 만들려면 개발자가 응용 프로그램에서 사용하려는 GATT 서비스 및 특성에 대해 사전에 잘 알고 있어야 하며 API에서 제공하는 이진 데이터가 사용자에게 제공되기 전에 유용한 데이터로 변환되도록 특정 특성 값을 처리해야 합니다. Bluetooth GATT API는 Bluetooth LE 장치와 통신하는 데 필요한 기본 요소만 표시합니다. 데이터를 해석하려면 Bluetooth SIG 표준 프로필이나 장치 공급업체에서 구현한 사용자 지정 프로필을 통해 응용 프로그램 프로필을 정의해야 합니다. 프로필은 교환되는 데이터가 어떻게 표현되고 그 데이터를 어떻게 해석해야 하는지를 규정하는 응용 프로그램과 장치 간의 바인딩 계약을 만듭니다.

편의상 Bluetooth SIG는 [사용 가능한 공용 프로필](http://go.microsoft.com/fwlink/p/?LinkID=317977) 목록을 유지합니다.

## <a name="retrieve-bluetooth-data"></a>Bluetooth 데이터 수신

이 예제에서는 앱이 Bluetooth LE 건강 온도계 서비스를 구현하는 Bluetooth 장치의 온도 측정치를 사용합니다. 앱은 새 온도 측정을 사용할 수 있을 때 알리도록 지정합니다. "온도계 특성 값 변경" 이벤트에 대한 이벤트 처리기를 등록하면 앱은 포그라운드에서 실행하는 동안 특성 값 변경 이벤트 알림을 받습니다.

이 앱은 일시 중단될 때 모든 디바이스 리소스를 해제해야 하며 다시 시작될 때 디바이스 열거형과 초기화를 다시 수행해야 합니다. 백그라운드에서 디바이스 조작이 필요한 경우 [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx) 또는 [GattCharacteristicNotificationTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.gattcharacteristicnotificationtrigger.aspx)를 살펴보세요. 일반적으로 DeviceUseTrigger는 더 자주 발생하는 이벤트에 적절하고, GattCharacteristicNotificationTrigger는 드물게 발생하는 이벤트 처리에 적절합니다.  

```csharp
double convertTemperatureData(byte[] temperatureData)
{
    // Read temperature data in IEEE 11703 floating point format
    // temperatureData[0] contains flags about optional data - not used
    UInt32 mantissa = ((UInt32)temperatureData[3] << 16) |
        ((UInt32)temperatureData[2] << 8) |
        ((UInt32)temperatureData[1]);

    Int32 exponent = (Int32)temperatureData[4];

    return mantissa * Math.Pow(10.0, exponent);
}

async void Initialize()
{
    var themometerServices = await Windows.Devices.Enumeration
        .DeviceInformation.FindAllAsync(GattDeviceService
            .GetDeviceSelectorFromUuid(
                GattServiceUuids.HealthThermometer),
        null);

    GattDeviceService firstThermometerService = await
        GattDeviceService.FromIdAsync(themometerServices[0].Id);

    serviceNameTextBlock.Text = "Using service: " + 
        themometerServices[0].Name;

    GattCharacteristic thermometerCharacteristic =
        firstThermometerService.GetCharacteristics(
            GattCharacteristicUuids.TemperatureMeasurement)[0];

    thermometerCharacteristic.ValueChanged += temperatureMeasurementChanged;

    await thermometerCharacteristic
        .WriteClientCharacteristicConfigurationDescriptorAsync(
            GattClientCharacteristicConfigurationDescriptorValue.Notify);
}

void temperatureMeasurementChanged(
    GattCharacteristic sender,
    GattValueChangedEventArgs eventArgs)
{
    byte[] temperatureData = new byte[eventArgs.CharacteristicValue.Length];
    Windows.Storage.Streams.DataReader.FromBuffer(
        eventArgs.CharacteristicValue).ReadBytes(temperatureData);

    var temperatureValue = convertTemperatureData(temperatureData);

    temperatureTextBlock.Text = temperatureValue.ToString();
}
```

```cpp
double MainPage::ConvertTemperatureData(
    const Array<unsigned char>^ temperatureData)
{
    unsigned mantissa = ((unsigned)temperatureData[3] << 16) |
        ((unsigned)temperatureData[2] << 8) |
        ((unsigned)temperatureData[1]);

    int exponent = (int)temperatureData[4];

    return mantissa * pow(10.0, (double)exponent);
}

void MainPage::Initialize()
{
    create_task(DeviceInformation::FindAllAsync(
        GattDeviceService::GetDeviceSelectorFromUuid(
            GattServiceUuids::HealthThermometer), 
        nullptr)).then(
            [this] (DeviceInformationCollection^ thermometerServices) 
    {
        create_task(GattDeviceService::FromIdAsync(
            thermometerServices->GetAt(0)->Id))
            .then([this] (GattDeviceService^ firstThermometerService) 
        {
            GattCharacteristic^ thermometerCharacteristic = 
                firstThermometerService->GetCharacteristics(
                    GattCharacteristicUuids::TemperatureMeasurement)
                        ->GetAt(0);

            thermometerCharacteristic->ValueChanged += 
                ref new TypedEventHandler<
                    GattCharacteristic^, 
                    GattValueChangedEventArgs^>(
                        this, &MainPage::TemperatureMeasurementChanged);

            create_task(thermometerCharacteristic->
                WriteClientCharacteristicConfigurationDescriptorAsync(
                GattClientCharacteristicConfigurationDescriptorValue
                    ::Notify));
        });
    });
}


void MainPage::TemperatureMeasurementChanged(
    GattCharacteristic^ sender,
    GattValueChangedEventArgs^ eventArgs)
{
    auto temperatureData =  ref new Array<unsigned char>(
        eventArgs->CharacteristicValue->Length);
    DataReader::FromBuffer(eventArgs->CharacteristicValue)
        ->ReadBytes(temperatureData);

    double temperatureValue = ConvertTemperatureData(temperatureData);
    std::wstringstream str;
    str << temperatureValue;

    temperatureTextBlock->Text = ref new String(str.str().c_str());
}
```

## <a name="control-a-bluetooth-le-thermometer-device"></a>Bluetooth LE 온도계 장치 제어

이 예제에서는 UWP 앱이 가상 Bluetooth LE 온도계 장치용 컨트롤러로 작동합니다. 이 장치는 [**HealthThermometer**](https://msdn.microsoft.com/library/windows/apps/Dn297603) 프로필의 표준 특성 외에도, 사용자가 섭씨 또는 화씨로 판독 값을 검색할 수 있도록 하는 형식 특성을 선언합니다. 이 앱에서는 신뢰할 수 있는 쓰기 트랜잭션을 사용하여 형식과 측정 간격이 단일 값으로 설정되도록 합니다.

```csharp
// Uuid of the "Format" Characteristic Value
Guid formatCharacteristicUuid = 
    new Guid("{00000000-0000-0000-0000-000000000010}");

// Constant representing a Fahrenheit scale temperature measurement
const byte FahrenheitReading = 1;
async void Initialize()
{
    var themometerServices = await Windows.Devices.Enumeration
        .DeviceInformation.FindAllAsync(GattDeviceService
            .GetDeviceSelectorFromUuid(
                GattServiceUuids.HealthThermometer),
        null);

    GattDeviceService thermometerService = await
        GattDeviceService.FromIdAsync(themometerServices[0].Id);

    serviceNameTextBlock.Text = "Using service: " + 
        themometerServices[0].Name;

    GattCharacteristic intervalCharacteristic = thermometerService
        .GetCharacteristics(GattCharacteristicUuids.MeasurementInterval)[0];

    GattCharacteristic formatCharacteristic = thermometerService
        .GetCharacteristics(formatCharacteristicUuid)[0];

    GattReliableWriteTransaction gattTransaction = 
        new GattReliableWriteTransaction();

    var writer = new Windows.Storage.Streams.DataWriter();

    // Get a temperature reading every 60 seconds
    writer.WriteUInt16(60);

    gattTransaction.WriteValue(
        intervalCharacteristic, 
        writer.DetachBuffer());

    // Get the reading on the Fahrenheit scale
    writer.WriteByte(FahrenheitReading);

    gattTransaction.WriteValue(
        formatCharacteristic, 
        writer.DetachBuffer());

    GattCommunicationStatus status = await gattTransaction.CommitAsync();

    if (GattCommunicationStatus.Unreachable == status)
    {
        statusTextBlock.Text = "Writing to your device failed !";
    }
}
```

```cpp
// Uuid of the "Format" Characteristic Value
Guid formatCharacteristicUuid(0x00000000, 0x0000, 0x0000, 0x00, 0x00, 
                                  0x00, 0x00, 0x00, 0x00, 0x00, 0x10);

// Constant representing a Fahrenheit scale temperature measurement
const unsigned char FAHRENHEIT_READING = 1;

void MainPage::Initialize()
{
    create_task(DeviceInformation::FindAllAsync(
        GattDeviceService::GetDeviceSelectorFromUuid(
            GattServiceUuids::HealthThermometer), 
        nullptr)).then(
            [this] (DeviceInformationCollection^ thermometerServices) 
    {
        create_task(GattDeviceService::FromIdAsync(
            thermometerServices->GetAt(0)->Id)).then([this] (
                GattDeviceService^ thermometerService) 
        {
            GattCharacteristic^ intervalCharacteristic = 
                thermometerService->GetCharacteristics(
                    GattCharacteristicUuids::MeasurementInterval)
                        ->GetAt(0);

            GattCharacteristic^ formatCharacteristic = 
                thermometerService->GetCharacteristics(
                    formatCharacteristicUuid)->GetAt(0);

            GattReliableWriteTransaction^ gattTransaction = 
                ref new GattReliableWriteTransaction();

            DataWriter^ writer = ref new DataWriter();

            // Get a temperature reading every 60 seconds
            writer->WriteUInt16(60);

            gattTransaction->WriteValue(
                intervalCharacteristic, 
                writer->DetachBuffer());

            writer->WriteByte(FAHRENHEIT_READING);

            gattTransaction->WriteValue(
                formatCharacteristic, 
                writer->DetachBuffer());

            create_task(gattTransaction->CommitAsync())
                .then([this] (GattCommunicationStatus status) 
            {
                if (GattCommunicationStatus::Unreachable == status) 
                { 
                    statusTextBlock->Text = 
                        ref new String(L"Writing to your device failed !");
                }
            });
        });
    });

```

## <a name="control-the-presentation-of-bluetooth-le-device-data"></a>Bluetooth LE 장치 데이터 프레젠테이션 제어

Bluetooth LE 장치는 현재 배터리 수준을 사용자에게 제공하는 배터리 서비스를 표시할 수 있습니다. 이 배터리 서비스에는 배터리 잔량 데이터를 유연하게 해석할 수 있도록 하는 [**PresentationFormats**](https://msdn.microsoft.com/library/windows/apps/Dn263742) 설명자(선택 사항)가 포함되어 있습니다. 이 시나리오에서는 해당 장치와 작동하는 앱의 예를 제공하고 **PresentationFormats** 속성을 사용하여 사용자에게 제공하기 전에 특성 값의 서식을 지정합니다.

```csharp
async void Initialize()
{
    var batteryServices = await Windows.Devices.Enumeration
        .DeviceInformation.FindAllAsync(GattDeviceService
            .GetDeviceSelectorFromUuid(GattServiceUuids.Battery),
        null);

    if (batteryServices.Count > 0)
    {
        // Use the first Battery service on the system
        GattDeviceService batteryService = await GattDeviceService
            .FromIdAsync(batteryServices[0].Id);

        // Use the first Characteristic of that Service
        GattCharacteristic batteryLevelCharacteristic =
            batteryService.GetCharacteristics(
                GattCharacteristicUuids.BatteryLevel)[0];

        batteryLevelCharacteristic.ValueChanged += batteryLevelChanged;
    }
    else
    {
        statusTextBlock.Text = "No Battery services found !";
    }
}

void batteryLevelChanged(
    GattCharacteristic sender,
    GattValueChangedEventArgs eventArgs)
{
    byte levelData = Windows.Storage.Streams.DataReader
        .FromBuffer(eventArgs.CharacteristicValue).ReadByte();

    double levelValue;

    if (sender.PresentationFormats.Count > 0)
    {
        levelValue = levelData * 
            Math.Pow(10.0, sender.PresentationFormats[0].Exponent);
    }
    else
    {
        levelValue = (double)levelData;
    }

    batteryLevelTextBlock.Text = levelValue.ToString();
}
```

```cpp
void MainPage::Initialize()
{
    create_task(DeviceInformation::FindAllAsync(
        GattDeviceService::GetDeviceSelectorFromUuid(
            GattServiceUuids::Battery), 
        nullptr)).then([this] (DeviceInformationCollection^ batteryServices) 
    {
        create_task(GattDeviceService::FromIdAsync(
            batteryServices->GetAt(0)->Id)).then([this] (
                GattDeviceService^ batteryService) 
        {
            GattCharacteristic^ batteryLevelCharacteristic = 
                batteryService->GetCharacteristics(
                    GattCharacteristicUuids::BatteryLevel)->GetAt(0);

            batteryLevelCharacteristic->ValueChanged += 
                ref new TypedEventHandler<
                    GattCharacteristic^, 
                    GattValueChangedEventArgs^>
                    (this, &MainPage::BatteryLevelChanged);

            create_task(batteryLevelCharacteristic
                ->WriteClientCharacteristicConfigurationDescriptorAsync(
                GattClientCharacteristicConfigurationDescriptorValue
                    ::Notify));
        });
    });
}

void MainPage::BatteryLevelChanged(
    GattCharacteristic^ sender,
    GattValueChangedEventArgs^ eventArgs)
{
    unsigned char batteryLevelData = DataReader::FromBuffer(
        eventArgs->CharacteristicValue)->ReadByte();

    // if this characteristic has a presentation format
    // use that information to format the value
    double batteryLevelValue;
    if (sender->PresentationFormats->Size > 0)
    {
        batteryLevelValue = batteryLevelData * 
            pow(10.0, sender->PresentationFormats->GetAt(0)->Exponent);
    }
    else
    {
        batteryLevelValue = batteryLevelData;
    }

    std::wstringstream str;
    str << batteryLevelValue;
    batteryLevelTextBlock->Text = 
        ref new String(str.str().c_str());
}
```




