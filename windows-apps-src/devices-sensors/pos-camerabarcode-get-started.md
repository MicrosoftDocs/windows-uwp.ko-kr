---
author: TerryWarwick
title: 카메라 바코드 스캐너 시작하기
description: 카메라 바코드 스캐너를 사용 하는 방법 학습
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 12aabff66fc116f510dced78aa56f3df5f84c850
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7284308"
---
# <a name="getting-started-with-a-camera-barcode-scanner"></a>카메라 바코드 스캐너 시작하기
## <a name="step-1-add-capability-declarations-to-your-app-manifest"></a>1단계: 앱 매니페스트에 기능 선언 추가
1. Microsoft Visual Studio의 **솔루션 탐색기**에서 **package.appxmanifest** 항목을 두 번 클릭하여 응용 프로그램 매니페스트 디자이너를 엽니다.
2. **기능** 탭 선택
3. **웹캠** 및 **PointOfService** 확인란을 클릭 

>[!NOTE] 
> 소프트웨어 디코더가 카메라에서 프레임을 수신하여 디코딩을 수행하고 응용 프로그램에서 미리 보기를 제공하려면 **웹캠** 기능이 필요합니다.

## <a name="step-2-add-using-directives"></a>2단계: 지시문을 사용하여 추가

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;
```
## <a name="step-3-define-your-device-selector"></a>3단계: 장치 선택기 정의

### **<a name="option-a-find-all-barcode-scanners"></a>옵션 A: 모든 바코드 스캐너 찾기**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector();       
```

### **<a name="option-b-scoping-device-selector-to-connection-type"></a>옵션 B: 연결 유형으로 장치 선택기 범위 지정**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

## <a name="step-4-enumerate-barcode-scanners"></a>4단계: 바코드 스캐너 열거
응용 프로그램의 장치 목록이 변경되지 않는 경우에는 *FindAllAsync*에서 단 한번 스냅샷을 나열할 수 있지만, 응용 프로그램의 수명 동안 바코드 스캐너 목록을 변경할 수 있다는 믿음이 있는 경우에는 *DeviceWatcher*를 대신 사용해야 합니다.  

> [!Important] 
> PointOfService 장치를 나열하기 위해 GetDefaultAsync를 사용하면 클래스에서 확인된 첫 번째 장치만 반환하면 세션 간의 변경이 가능하기 때문에 일관되지 않은 동작이 유발될 수 있습니다.

### **<a name="option-a-enumerate-a-snapshot-of-barcode-scanners"></a>옵션 A: 바코드 스캐너의 스냅숍 나열**
```Csharp
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

> [!TIP]
> *FindAllAsync* 사용에 대한 자세한 내용은 [*장치의 스냅숏 나열*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-a-snapshot-of-devices)을 참조하세요.

### **<a name="option-b-enumerate-and-watch-for-changes-in-available-barcode-scanners"></a>옵션 B: 사용할 수 있는 바코드 스캐너의 변경 사항을 나열 및 모니터링**
```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);

// TODO: Add Event Listeners and Handlers
```
> [!TIP]
> 자세한 내용은 [*장치 변경 사항 나열 및 모니터링*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices)과 [*DeviceWatcher*](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)를 참조하세요.

## <a name="step-5-identify-camera-barcode-scanners"></a>5단계: 카메라 바코드 스캐너 식별
카메라 바코드 스캐너는 Windows가 컴퓨터에 연결된 카메라(들)를 소프트웨어 디코더에 페어링하는 과정에서 역동적으로 생성됩니다.  카메라와 디코더를 페어링할 때마다 바코드 스캐너가 완벽하게 작동합니다.

그 결과로 생성된 장치 컬렉션의 각 바코드 스캐너에서 [*BarcodeScanner.VideoDeviceID*](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.videodeviceid#Windows_Devices_PointOfService_BarcodeScanner_VideoDeviceId) 속성을 클릭하면 카메라 바코드 스캐너인지 물리적 바코드 스캐너인지 판단할 수 있습니다.  NULL이 아닌 VideoDeviceID는 장치 컬렉션의 바코드 스캐너 개체가 카메라 바코드 스캐너임을 나타냅니다.  카메라 바코드 스캐너가 하나 이상 있을 경우에는 물리적 바코드 스캐너를 제외한 별도의 컬렉션을 빌드하고 싶을 수 있습니다. 

Windows와 함께 배송된 바코더를 사용하는 카메라 바코드 스캐너가 다음과 같이 표시됩니다. 

> Microsoft BarcodeScanner(*여기 카메라 이름*)

컴퓨터 섀시에 카메라가 내장되어 있고 카메라가 한 대 이상이면 *전방* 및 *후방*의 이름이 다를 수 있습니다.  추가 소프트웨어 디코더는 추후에 사용할 수 있으며, 다른 이름 지정 체계를 따를 수 있습니다.

## <a name="step-6-claim-the-camera-barcode-scanner"></a>6단계: 카메라 바코드 스캐너 클레임 
[BarcodeScanner.ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync)를 사용하여 카메라 바코드 스캐너에 대한 배타적 이용권을 획득합니다.

## <a name="step-7-system-provided-preview"></a>7단계: 시스템에서 제공되는 미리 보기
사용자가 바코드에서 성공적으로 카메라를 겨냥하려면 카메라 미리 보기가 필요합니다.  Windows가 제공하는 간단한 카메라 미리 보기를 통해 카메라 바코드 스캐너의 기본 컨트롤을 사용할 수 있도록 지정하는 대화 상자를 시작할 수 있습니다.  대화 상자를 열려면 [ClaimedBarcodeScanner.ShowVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.showvideopreviewasync)를, 종료 시 대화 상자를 닫으려면 [ClaimedBarcodeScanner.HideVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.hidevideopreview)를 호출하세요.

> [!TIP]
> 응용 프로그램에서 카메라 바코드 스캐너에 대한 미리 보기를 호스팅하는 방법은 [미리 보기 호스팅](pos-camerabarcode-hosting-preview.md)을 참조하세요.

## <a name="step-8-initiate-scan"></a>8 단계: 시작 검사 
[**StartSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync)을 호출하여 스캔 프로세스를 시작할 수 있습니다.  
[**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 값에 따라 스캐너는 오직 하나의 바코드만 스캔한 다음 중지하거나 [**StopSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync)가 호출될 때까지 계속해서 스캔을 할 수 있습니다.

바코드가 디코딩될 때 스캐너 동작을 제어하도록 [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived)를 원하는 값으로 설정합니다.

| 값 | 설명 |
| ----- | ----------- |
| True   | 오직 하나의 바코드만 스캔하고 중지 |
| False  | 중단 없이 지속적으로 바코드 스캔 |