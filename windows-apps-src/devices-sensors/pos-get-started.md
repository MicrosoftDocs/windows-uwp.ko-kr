---
title: 서비스 지점 시작
description: 이 문서에는 서비스 Windows 런타임 Api를 시작 하는 방법에 대 한 정보가 포함 되어 있습니다.
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: f5f19d1337a7ae49f46ab65d8420fedb775eeb2f
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730381"
---
# <a name="getting-started-with-point-of-service"></a>서비스 지점 시작

서비스 지점, pos 또는 pos 장치는 소매 거래를 용이 하 게 하는 데 사용 되는 컴퓨터 주변 장치입니다. 서비스 지점의 예로는 전자 현금 등록, 바코드 스캐너, 자기 줄무늬 판독기, 수신 프린터 등이 있습니다.

여기서는 Windows 런타임 서비스 지점 Api를 사용 하 여 서비스 지점과의 상호 작용에 대 한 기본 사항을 알아봅니다. 장치 열거, 장치 기능 확인, 장치 클레임 및 장치 공유에 대해 설명 합니다. 예를 들어 바코드 스캐너 장치를 사용 하지만 여기에 나오는 거의 모든 지침은 UWP 호환 서비스 장치 지점에 적용 됩니다. 지원 되는 장치 목록은 [서비스 장치 지원 지점](pos-device-support.md)을 참조 하세요.

## <a name="finding-and-connecting-to-point-of-service-peripherals"></a>서비스 주변 장치 찾기 및 연결

앱에서 서비스 장치를 사용 하려면 먼저 앱이 실행 되는 PC와 쌍을 이루어야 합니다. 프로그래밍 방식으로 또는 설정 앱을 통해 서비스의 지점에 연결 하는 방법에는 여러 가지가 있습니다.

### <a name="connecting-to-devices-by-using-the-settings-app"></a>설정 앱을 사용 하 여 장치에 연결
바코드 스캐너와 같은 지점의 서비스 장치를 PC에 연결 하면 다른 장치와 마찬가지로 표시 됩니다. 설정 앱의 **Bluetooth & 기타 장치 > 장치** 섹션에서 찾을 수 있습니다. 여기서는 **Bluetooth 또는 기타 장치 추가**를 선택 하 여 서비스 지점과 쌍을 연결할 수 있습니다.

서비스 Api의 시점을 사용 하 여 프로그래밍 방식으로 열거 될 때까지 일부 서비스 장치는 설정 앱에 표시 되지 않을 수 있습니다.

### <a name="getting-a-single-point-of-service-device-with-getdefaultasync"></a>GetDefaultAsync를 사용 하 여 단일 서비스 지점 가져오기
간단한 사용 사례에서 앱이 실행 되 고 있는 PC에는 가능한 한 빨리 설정 하는 것이 유일한 서비스 주변 기기가 있을 수 있습니다. 이렇게 하려면 아래와 같이 **GetDefaultAsync** 메서드를 사용 하 여 "기본" 장치를 검색 합니다.

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

기본 장치가 발견 되 면 검색 된 장치 개체를 요청할 준비가 된 것입니다. "클레임"은 응용 프로그램에 대 한 단독 액세스를 제공 하 여 여러 프로세스에서 충돌 하는 명령을 방지 합니다.

> [!NOTE] 
> 두 개 이상의 서비스 장치가 PC에 연결 된 경우 **GetDefaultAsync** 는 검색 한 첫 번째 장치를 반환 합니다. 이러한 이유로 응용 프로그램에는 하나의 서비스 장치만 표시 되는 경우를 제외 하 고 **FindAllAsync** 를 사용 합니다.

### <a name="enumerating-a-collection-of-devices-with-findallasync"></a>FindAllAsync를 사용 하 여 장치 컬렉션 열거

둘 이상의 장치에 연결 된 경우에는 **Pointofservice** 장치 개체의 컬렉션을 열거 하 여 클레임 하려는 항목을 찾아야 합니다. 예를 들어 다음 코드는 현재 연결 된 모든 바코드 스캐너의 컬렉션을 만든 다음 특정 이름을 가진 스캐너의 컬렉션을 검색 합니다.

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

### <a name="scoping-the-device-selection"></a>장치 선택 범위 지정
장치에 연결할 때 앱이 액세스할 수 있는 서비스 주변 장치의 하위 집합으로 검색을 제한 하는 것이 좋습니다. **Getdeviceselector** 메서드를 사용 하 여 특정 방법 (BLUETOOTH, USB 등)만 연결 된 장치를 검색 하도록 선택 범위를 지정할 수 있습니다. **Bluetooth**, **IP**, **로컬**또는 **모든 연결 유형을**통해 장치를 검색 하는 선택기를 만들 수 있습니다. 이는 무선 장치 검색에 로컬 (유선) 검색에 비해 시간이 오래 걸리기 때문에 유용할 수 있습니다. 로컬 연결 형식으로 **FindAllAsync** **를 제한** 하 여 로컬 장치 연결에 대 한 결정적 대기 시간을 보장할 수 있습니다. 예를 들어이 코드는 로컬 연결을 통해 액세스할 수 있는 모든 바코드 스캐너를 검색 합니다. 

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

### <a name="reacting-to-device-connection-changes-with-devicewatcher"></a>DeviceWatcher를 사용 하 여 장치 연결 변경 사항에 대응

앱이 실행 되 면 장치에서 장치를 분리 하거나 업데이트 하거나 새 장치를 추가 해야 하는 경우가 있습니다. **Devicewatcher** 클래스를 사용 하 여 장치 관련 이벤트에 액세스할 수 있으므로 앱이 그에 따라 응답할 수 있습니다. 장치를 추가, 제거 또는 업데이트 하는 경우 메서드 스텁을 호출 하는 **Devicewatcher**를 사용 하는 방법에 대 한 예제는 다음과 같습니다.

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

## <a name="checking-the-capabilities-of-a-point-of-service-device"></a>서비스 지점의 기능 확인
바코드 스캐너와 같은 장치 클래스 내 에서도 각 장치의 특성은 모델 간에 크게 다를 수 있습니다. 앱에 특정 장치 특성이 필요한 경우 연결 된 각 장치 개체를 검사 하 여 특성이 지원 되는지 여부를 확인 해야 할 수 있습니다. 예를 들어 비즈니스에 특정 바코드 인쇄 패턴을 사용 하 여 레이블을 만들어야 하는 경우가 있을 수 있습니다. 연결 된 바코드 스캐너에서 기호를 지원 하는지 여부를 확인할 수 있는 방법은 다음과 같습니다. 

> [!NOTE]
> 기호는 바코드에서 메시지를 인코딩하는 데 사용 하는 언어 매핑입니다.

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

### <a name="using-the-devicecapabilities-class"></a>Capabilities 클래스 사용
**Capabilities** 클래스는 모든 서비스 장치 클래스 지점의 특성으로, 각 장치에 대 한 일반 정보를 가져오는 데 사용할 수 있습니다. 예를 들어이 예제에서는 장치에서 통계 보고를 지원 하는지 여부를 확인 하 고, 해당 하는 경우 지원 되는 모든 유형에 대 한 통계를 검색 합니다.

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

## <a name="claiming-a-point-of-service-device"></a>서비스 지점 장치 클레임
활성 입력 또는 출력에 대 한 서비스 지점 장치를 사용 하려면 먼저이를 요청 하 여 응용 프로그램에 다양 한 기능에 대 한 단독 액세스 권한을 부여 해야 합니다. 이 코드는 앞에서 설명한 방법 중 하나를 사용 하 여 장치를 찾은 후 바코드 스캐너 장치를 요청 하는 방법을 보여 줍니다.

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

### <a name="retaining-the-device"></a>장치 보존
네트워크 또는 Bluetooth 연결을 통해 특정 서비스 장치를 사용 하는 경우 장치를 네트워크의 다른 앱과 공유할 수 있습니다. 이에 대 한 자세한 내용은 [장치 공유](#sharing-a-device-between-apps)를 참조 하세요. 다른 경우에는 장기간 사용 하기 위해 장치를 유지 하려고 할 수 있습니다. 이 예제에서는 다른 앱에서 장치를 해제 하도록 요청한 후에 요청한 바코드 스캐너를 유지 하는 방법을 보여 줍니다.

```Csharp
claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner e)
{
    e.RetainDevice();  // Retain exclusive access to the device
}
```

## <a name="input-and-output"></a>입력 및 출력

장치를 요청한 후에는 장치를 사용 하는 것이 거의 준비 되었습니다. 장치에서 입력을 받으려면 대리자가 데이터를 받도록 설정 하 고 사용 하도록 설정 해야 합니다. 아래 예제에서는 바코드 스캐너 장치를 요청 하 고 디코드 속성을 설정한 다음 **Enableasync** 를 호출 하 여 장치에서 디코딩된 입력을 사용 하도록 설정 합니다. 이 프로세스는 장치 클래스 마다 다르므로 바코드가 아닌 장치에 대 한 대리자를 설정 하는 방법에 대 한 지침은 관련 [UWP 앱 샘플](https://github.com/Microsoft/Windows-universal-samples#devices-and-sensors)을 참조 하세요.

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

## <a name="sharing-a-device-between-apps"></a>앱 간에 장치 공유

서비스 지점 장치는 둘 이상의 앱이 잠시 동안 액세스 해야 하는 경우에 자주 사용 됩니다.  여러 앱에 로컬로 연결 하거나 (USB 또는 기타 유선 연결) Bluetooth 또는 IP 네트워크를 통해 장치를 공유할 수 있습니다. 각 앱의 요구 사항에 따라 장치에서 해당 클레임을 삭제 해야 할 수도 있습니다. 이 코드는 요청한 바코드 스캐너 장치를 삭제 하 여 다른 앱이 클레임을 사용 하 고 사용할 수 있도록 합니다.

```Csharp
if (claimedBarcodeScanner != null)
{
    claimedBarcodeScanner.Dispose();
    claimedBarcodeScanner = null;
}
```

> [!NOTE]
> 요청 된 지점 및 취소 서비스 장치 클래스 모두 [은 windows.foundation.iclosable 인터페이스](https://docs.microsoft.com/uwp/api/windows.foundation.iclosable)를 구현 합니다. 장치가 네트워크 또는 Bluetooth를 통해 앱에 연결 된 경우 다른 앱이 연결 하려면 먼저 요청 된 개체와 취소 개체를 모두 삭제 해야 합니다.

## <a name="see-also"></a>참고 항목
+ [바코드 스캐너 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [현금 서랍 샘플]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [줄 표시 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [자기 줄무늬 판독기 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [POSPrinter 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

