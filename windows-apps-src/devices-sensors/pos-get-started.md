---
author: TerryWarwick
title: 서비스 지점 시작하기
description: 이 문서는 서비스 지점 UWP API 시작에 대한 정보를 포함하고 있습니다.
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 64a7fdbb0cfbeeedb0d129c809e05f9926a8a71d
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832297"
---
# <a name="getting-started-with-point-of-service"></a>서비스 지점 시작하기

서비스 지점, POS(Point of Sale), 서비스 지점 디바이스는 소매 거래를 용이하게 해주는 컴퓨터 주변 장치입니다. 서비스 지점 디바이스로는 전자식 현금 등록기, 바코드 스캐너, 자기 띠 판독기 및 영수증 프린터가 있습니다.

이 문서에서는 UWP(유니버설 Windows 플랫폼) 서비스 지점 API를 사용하여 서비스 지점 디바이스를 연결하는 기본적인 방법을 배울 것입니다. 디바이스 열거, 디바이스 기능 확인, 디바이스 클레임, 디바이스 공유에 대해 다룰 것입니다. 바코드 스캐너 디바이스를 예로 들고 있지만, 여기 나온 거의 모든 지침은 UWP와 호환 가능한 모든 서비스 지점 디바이스치에 적용됩니다 (지원 디바이스의 목록은 [서비스 지점 디바이스 지원](pos-device-support.md)을 참조).

## <a name="finding-and-connecting-to-point-of-service-peripherals"></a>서비스 지점 주변 장치를 찾아서 연결

앱에서 서비스 지점 디바이스를 사용할 수 있으려면 먼저 앱이 실행 중인 PC와 페어링이 되어야 합니다. 서비스 지점 디바이스는 프로그래밍 방식이나 설정 앱을 통해 여러 방법으로 연결할 수 있습니다.

### <a name="connecting-to-devices-by-using-the-settings-app"></a>설정 앱을 사용하여 디바이스에 연결
바코드 스캐너와 같은 서비스 지점 디바이스를 PC에 연결하면 다른 디바이스와 마찬가지로 표시됩니다. 설정 앱의 **디바이스 > Bluetooth 및 기타 디바이스** 섹션에서 찾을 수 있습니다. 그런 다음 **Bluetooth 또는 기타 디바이스 추가**를 선택하여 서비스 지점 디바이스에 페어링할 수 있습니다.

일부 서비스 지점 디바이스는 서비스 지점 API를 사용하여 프로그래밍 방식으로 열거할 때까지 설정 앱에 나타나지 않을 수 있습니다.

### <a name="getting-a-single-point-of-service-device-with-getdefaultasync"></a>GetDefaultAsync로 단일 서비스 지점 디바이스 얻기
간단한 사용 사례에서 앱이 실행 중인 PC에 오직 하나의 서비스 지점 주변 장치만 연결할 수 있으며, 가능한 신속하게 이를 설정하고 싶을 수 있습니다. 이를 위해서는 다음과 같이 **GetDefaultAsync**로 "기본" 디바이스를 검색합니다.

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

기본 디바이스가 발견되면 디바이스 개체가 검색되고 클레임할 준비가 됩니다. 디바이스의 "클레임"은 응용 프로그램이 단독으로 액세스하여, 여러 프로세스 사이의 명령 충돌을 방지하는 것입니다.

> [!NOTE] 
> 하나 이상의 서비스 지점 디바이스가 PC에 연결되어 있으면 **GetDefaultAsync**는 첫 번째 디바이스를 반환합니다. 따라서 응용 프로그램에 표시되는 서비스 지점 디바이스가 하나인지 확실하지 않으면 **FindAllAsync**를 사용합니다.

### <a name="enumerating-a-collection-of-devices-with-findallasync"></a>FindAllAsync를 사용한 디바이스 컬렉션 열거

둘 이상의 디바이스가 연결되어 있으면, 클레임하려는 디바이스를 찾기 위해 **PointOfService** 디바이스 개체의 컬렉션을 열거해야 합니다. 예를 들어, 다음 코드는 현재 연결된 모든 바코드 스캐너의 컬렉션 만들고 컬렉션에서 특정 이름의 스캐너를 검색합니다.

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;

string selector = BarcodeScanner.GetDeviceSelector();       
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
    if (devInfo.Name.Contains("1200G"))
    {
        Debug.WriteLine(" Found one");
    }
}
```

### <a name="scoping-the-device-selection"></a>디바이스 선택 범위 지정
디바이스에 연결할 때, 앱이 액세스할 서비스 지점 주변 장치의 일부만 검색하도록 제한할 수 있습니다. **GetDeviceSelector** 메서드를 사용하면 특정 메서드를 사용하여 연결된 디바이스(예: Bluetooth, USB 등)만 선택하도록 범위를 지정할 수 있습니다. **Bluetooth**, **IP**, **로컬** 또는 **모든 연결 유형**의 디바이스를 검색하는 선택기를 만들 수 있습니다. 무선 디바이스 검색은 로컬(유선) 검색에 비해 시간이 오래 걸리므로 이는 유용할 수 있습니다. **FindAllAsync**를 **로컬** 연결 유형으로 제한하여 로컬 디바이스 연결에 대한 결정된 대기 시간을 유지할 수 있습니다. 예를 들어, 이 코드는 로컬 연결을 통해 액세스할 수 있는 모든 바코드 스캐너를 검색합니다. 

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

### <a name="reacting-to-device-connection-changes-with-devicewatcher"></a>DeviceWatcher를 사용하여 디바이스 연결 변경에 대응

앱 실행 중 디바이스 연결이 끊어졌거나, 업데이트되었거나, 새 디바이스를 추가해야 하는 경우가 있습니다. **DeviceWatcher** 클래스를 사용하여 디바이스 관련 이벤트에 액세스하고, 앱이 적절히 반응할 수 있도록 합니다. **DeviceWatcher**를 사용하는 방법의 예는 다음과 같습니다. 디바이스가 추가, 제거 또는 업데이트되면 메서드 스텁이 호출됩니다.

```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
deviceWatcher.Added += DeviceWatcher_Added;
deviceWatcher.Removed += DeviceWatcher_Removed;
deviceWatcher.Updated += DeviceWatcher_Updated;

void DeviceWatcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    // TODO: Add the DeviceInformation object to your collection
}

void DeviceWatcher_Removed(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Remove the item in your collection associated with DeviceInformationUpdate
}

void DeviceWatcher_Updated(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Update your collection with information from DeviceInformationUpdate
}
```

## <a name="checking-the-capabilities-of-a-point-of-service-device"></a>서비스 지점 디바이스의 기능 확인
바코드 스캐너와 같은 동일한 디바이스 클래스 내에서도 모델에 따라 각 디바이스의 특성이 상당히 다를 수 있습니다. 앱에 특정 디바이스 특성이 필요한 경우, 이 특성이 지원되는지 연결된 각 디바이스 개체를 검사해야 합니다. 예를 들어, 회사에서 특정 바코드 인쇄 패턴을 사용하여 레이블을 만들어야 할 수 있습니다. 연결된 바코드 스캐너가기호를 지원하는지 확인하는 방법은 다음과 같습니다. 

> [!NOTE]
> 기호 체계는 바코드가 메시지 인코딩을 위해 사용하는 언어 매핑입니다.

```Csharp
try
{
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(deviceId);
    if (await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32))
    {
        Debug.WriteLine("Has symbology");
    }
}
catch (Exception ex)
{
    Debug.WriteLine("FromIdAsync() - " + ex.Message);
}
```

### <a name="using-the-devicecapabilities-class"></a>Device.Capabilities 클래스 사용
**Device.Capabilities** 클래스는 모든 서비스 지점 디바이스 클래스의 특성으로, 각 디바이스에 대한 일반 정보를 얻는 데 사용할 수 있습니다. 예를 들어, 이 예에서는 디바이스가 통계 보고를 지원하는지 확인하고, 지원할 경우 지원되는 모든 형식에 대한 통계를 검색합니다.

```Csharp
try
{
    if (barcodeScanner.Capabilities.IsStatisticsReportingSupported)
    {
        Debug.WriteLine("Statistics reporting is supported");

        string[] statTypes = new string[] {""};
        IBuffer ibuffer = await barcodeScanner.RetrieveStatisticsAsync(statTypes);
    }
}
catch (Exception ex)
{
    Debug.WriteLine("EX: RetrieveStatisticsAsync() - " + ex.Message);
}
```

## <a name="claiming-a-point-of-service-device"></a>서비스 지점 디바이스 클레임
활성 입력 또는 출력을 위해 서비스 지점 디바이스를 사용하려면 먼저 클레임해야 합니다. 이는 응용 프로그램에 여러 기능에 대한 독점 액세스 권한을 부여합니다. 이 코드는 전에 설명한 메서드 중 하나로 바코드 스캐너 디바이스를 찾은 다음 클레임하는 방법을 보여 줍니다.

```Csharp
try
{
    claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();
}
catch (Exception ex)
{
    Debug.WriteLine("EX: ClaimScannerAsync() - " + ex.Message);
}
```

### <a name="retaining-the-device"></a>디바이스 유지
네트워크 또는 Bluetooth 연결을 통해 서비스 지점 디바이스를 사용할 경우, 네트워크의 다른 앱과 디바이스를 공유하고자 할 수도 있습니다 (이에 대한 자세한 내용은 [디바이스 공유](#sharing-a-device-between-apps) 참조). 그렇지 않다면 장시간 사용을 위해 디바이스를 유지해야 할 것입니다. 이 예는 다른 앱이 디바이스를 요청했을 때 클레임한 바코드 스캐너를 유지하는 방법을 보여 줍니다.

```Csharp
claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner e)
{
    e.RetainDevice();  // Retain exclusive access to the device
}
```

## <a name="input-and-output"></a>입력 및 출력

디바이스를 클레임하면 사용 준비가 거의 된 것입니다. 디바이스에서 입력을 받으려면, 데이터를 받도록 대리자로 설정하고 활성화해야 합니다. 아래 예에서는 바코드 스캐너 디바이스를 클레임하고, 디코드 속성을 설정하고, **EnableAsync**를 호출하여 디바이스의 디코딩된 입력을 사용하도록 설정합니다. 이 프로세스는 디바이스 클래스마다 다르므로, 바코드가 아닌 디바이스에 대한 대리자를 설정하는 방법에 대한 지침은 관련 [UWP 앱 샘플](https://github.com/Microsoft/Windows-universal-samples#devices-and-sensors)을 참조하세요.

```Csharp
try
{
    claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();
    if (claimedBarcodeScanner != null)
    {
        claimedBarcodeScanner.DataReceived += claimedBarcodeScanner_DataReceived;
        claimedBarcodeScanner.IsDecodeDataEnabled = true;
        await claimedBarcodeScanner.EnableAsync();
    }
}
catch (Exception ex)
{
    Debug.WriteLine("EX: ClaimScannerAsync() - " + ex.Message);
}


void claimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    string symbologyName = BarcodeSymbologies.GetName(args.Report.ScanDataType);
    var scanDataLabelReader = DataReader.FromBuffer(args.Report.ScanDataLabel);
    string barcode = scanDataLabelReader.ReadString(args.Report.ScanDataLabel.Length);
}
```

## <a name="sharing-a-device-between-apps"></a>앱 사이의 디바이스 공유

서비스 지점 디바이스는 짧은 기간에 하나 이상의 앱이 액세스해야 하는 경우에도 사용됩니다.  디바이스는 여러 앱이 로컬(USB 또는 기타 유선 연결)로 연결되거나, Bluetooth 또는 IP 네트워크를 통해 공유할 수 있습니다. 각 앱의 필요에 따라 한 프로세스가 디바이스의 클레임을 해제해야 할 수 있습니다. 이 코드는 클레임된 바코드 스캐너 디바이스를 해제하여 다른 앱이 클레임 및 사용할 수 있도록 합니다.

```Csharp
if (claimedBarcodeScanner != null)
{
    claimedBarcodeScanner.Dispose();
    claimedBarcodeScanner = null;
}
```

> [!NOTE]
> 클레임된 서비스 지점 디바이스 클래스와 클레임이 해제된 서비스 지점 디바이스 클래스는 모두 [IClosable 인터페이스](https://docs.microsoft.com/uwp/api/windows.foundation.iclosable)를 구현합니다. 디바이스가 네트워크 또는 Bluetooth를 통해 앱에 연결된 경우, 클레임된 개체와 클레임 해제된 개체 모두 삭제해야 다른 앱이 연결할 수 있습니다.

## <a name="see-also"></a>참고 항목
+ [바코드 스캐너 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [현금 출납기 샘플]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [라인 디스플레이 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [자기 띠 판독기 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [POSPrinter 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

