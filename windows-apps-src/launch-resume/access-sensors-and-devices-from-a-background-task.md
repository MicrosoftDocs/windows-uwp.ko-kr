---
author: muhsinking
title: 백그라운드 작업에서 센서 및 디바이스에 액세스
description: DeviceUseTrigger를 사용하면 포그라운드 앱이 일시 중단된 경우에도 유니버설 Windows 앱이 백그라운드로 센서와 주변 장치에 액세스할 수 있습니다.
ms.assetid: B540200D-9FF2-49AF-A224-50877705156B
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 백그라운드 작업
ms.localizationpriority: medium
ms.openlocfilehash: 99f853da53302d4080bfa9462da0ec524e8d2064
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/11/2018
ms.locfileid: "3848907"
---
# <a name="access-sensors-and-devices-from-a-background-task"></a>백그라운드 작업에서 센서 및 장치에 액세스




[**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)를 사용하면 포그라운드 앱이 일시 중단된 경우에도 유니버설 Windows 앱이 백그라운드로 센서와 주변 장치에 액세스할 수 있습니다. 예를 들어 앱이 실행 중인 위치에 따라 백그라운드 작업을 사용하여 장치와 데이터를 동기화하거나 센서를 모니터링할 수 있습니다. 배터리 수명을 보존하고 적절한 사용자가 동의할 수 있도록 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)에는 이 항목에서 설명하는 정책이 적용됩니다.

백그라운드에서 센서나 주변 장치에 액세스하려면 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)를 사용하는 백그라운드 작업을 만듭니다. PC에서 이 작업을 만드는 방법을 보여 주는 예제는 [사용자 지정 USB 장치 샘플](http://go.microsoft.com/fwlink/p/?LinkId=301975 )을 참조하세요. 휴대전화에 대한 예제는 [백그라운드 센서 샘플](http://go.microsoft.com/fwlink/p/?LinkId=393307)을 참조하세요.

> [!Important]
> **DeviceUseTrigger**는 프로세스 백그라운드 작업에서 사용할 수 없습니다. 이 항목의 내용은 Out-of-proces를 실행하는 백그라운드 작업에만 적용됩니다.

## <a name="device-background-task-overview"></a>디바이스 백그라운드 작업 개요

앱이 사용자에게 더 이상 보이지 않을 때 Windows는 메모리 및 CPU 리소스를 회수하기 위해 앱을 일시 중단하거나 종료합니다. 그러면 다른 앱이 포그라운드로 실행될 수 있으므로 배터리 소모가 줄어듭니다. 이 경우 백그라운드 작업을 사용하지 않고는 지속적인 데이터 이벤트가 손실됩니다. Windows는 백그라운드 작업 트리거인 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)를 제공하므로, 앱이 일시 중단된 경우에도 앱이 장치와 센서에서 백그라운드로 안전하게 장기 실행 동기화와 모니터링 작업을 수행할 수 있습니다. 앱 수명 주기에 대한 자세한 내용은 [실행, 다시 시작 및 백그라운드 작업](index.md)을 참조하세요. 백그라운드 작업에 대한 자세한 내용은 [백그라운드 작업을 통해 앱 지원](support-your-app-with-background-tasks.md)을 참조하세요.

**참고**  유니버설 Windows 앱에서 백그라운드로 장치를 동기화하려면 사용자가 앱의 백그라운드 동기화를 승인해야 합니다. 또한 장치가 PC에 연결되거나 쌍을 이루고 활성 I/O가 있어야 하며, 최대 10분 동안 백그라운드 작업을 실행할 수 있습니다. 정책 적용에 대한 자세한 내용은 이 항목의 뒷부분에서 설명합니다.

### <a name="limitation-critical-device-operations"></a>제한 사항: 중요한 장치 작업

장기 실행 펌웨어 업데이트와 같은 일부 중요한 디바이스 작업은 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)로 수행할 수 없습니다. 이러한 작업은 PC에서만 수행할 수 있으며 [**DeviceServicingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297315)를 사용하는 권한 있는 앱만 수행할 수 있습니다. *권한 있는 앱*은 제조업체가 이러한 작업을 수행할 권한을 부여한 앱입니다. 장치 메타데이터는 장치에 대한 권한 있는 앱(있는 경우)을 지정하는 데 사용합니다. 자세한 내용은 [Microsoft Store 장치 앱용 장치 동기화 및 업데이트](http://go.microsoft.com/fwlink/p/?LinkId=306619)를 참조하세요.

## <a name="protocolsapis-supported-in-a-deviceusetrigger-background-task"></a>DeviceUseTrigger 백그라운드 작업에서 지원되는 프로토콜/API

[**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)를 사용하는 백그라운드 작업에서는 앱이 시스템 트리거 백그라운드 작업에서 지원되지 않는 많은 프로토콜/API를 통해 통신할 수 있습니다. 유니버설 Windows 앱에서 지원되는 프로토콜은 다음과 같습니다.

| 프로토콜         | 유니버설 Windows 앱의 DeviceUseTrigger                                                                                                                                                    |
|------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| USB              | ![이 프로토콜은 지원됩니다.](images/ap-tools.png)                                                                                                                                            |
| HID              | ![이 프로토콜은 지원됩니다.](images/ap-tools.png)                                                                                                                                            |
| Bluetooth RFCOMM | ![이 프로토콜은 지원됩니다.](images/ap-tools.png)                                                                                                                                            |
| Bluetooth GATT   | ![이 프로토콜은 지원됩니다.](images/ap-tools.png)                                                                                                                                            |
| MTP              | ![이 프로토콜은 지원됩니다.](images/ap-tools.png)                                                                                                                                            |
| 유선 네트워크    | ![이 프로토콜은 지원됩니다.](images/ap-tools.png)                                                                                                                                            |
| Wi-Fi 네트워크    | ![이 프로토콜은 지원됩니다.](images/ap-tools.png)                                                                                                                                            |
| IDeviceIOControl | ![deviceservicingtrigger는 ideviceiocontrol을 지원함](images/ap-tools.png)                                                                                                                       |
| 센서 API      | ![deviceservicingtrigger는 유니버설 센서 API를 지원함](images/ap-tools.png)([유니버설 장치 패밀리](https://msdn.microsoft.com/library/windows/apps/dn894631)의 센서로 제한됨) |

## <a name="registering-background-tasks-in-the-app-package-manifest"></a>앱 패키지 매니페스트에서 백그라운드 작업 등록

앱은 백그라운드 작업의 일부로 실행되는 코드에서 동기화 및 업데이트 작업을 수행합니다. 이 코드는 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)를 구현하는 Windows 런타임 클래스(또는 JavaScript 앱용 전용 JavaScript 페이지)에 포함되어 있습니다. [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 백그라운드 작업을 사용하려면 앱이 시스템 트리거 백그라운드 작업의 경우와 마찬가지로 포그라운드 앱의 앱 매니페스트 파일에서 선언해야 합니다.

이 앱 패키지 매니페스트 파일의 예제에서 **DeviceLibrary.SyncContent**는 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)를 사용하는 백그라운드 작업의 진입점입니다.

```xml
<Extensions>
  <Extension Category="windows.backgroundTasks" EntryPoint="DeviceLibrary.SyncContent">
    <BackgroundTasks>
      <m2:Task Type="deviceUse" />
    </BackgroundTasks>
  </Extension>
</Extensions>
```

## <a name="introduction-to-using-deviceusetrigger"></a>DeviceUseTrigger 사용 소개

[**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)를 사용하려면 다음 기본 단계를 따릅니다. 백그라운드 작업에 대한 자세한 내용은 [백그라운드 작업을 사용하여 앱 지원](support-your-app-with-background-tasks.md)을 참조하세요.

1.  앱은 백그라운드 작업을 앱 매니페스트에 등록하고 IBackgroundTask를 구현하는 Windows 런타임 클래스 또는 JavaScript 앱용 전용 JavaScript 페이지에 백그라운드 작업 코드를 포함합니다.
2.  앱을 시작하면 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 형식의 트리거 개체를 만들고 구성하며 나중에 사용하도록 트리거 인스턴스를 저장합니다.
3.  앱은 백그라운드 작업이 이전에 등록되었는지 여부를 확인하고, 등록되지 않은 경우 트리거에 대해 등록합니다. 앱에서 이 트리거와 연결된 작업에 조건을 설정할 수는 없습니다.
4.  앱이 백그라운드 작업을 트리거해야 하는 경우, 앱이 백그라운드 작업을 요청할 수 있는지 확인하기 위해 먼저 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)를 호출해야 합니다.
5.  앱이 백그라운드 작업을 요청할 수 있는 경우 장치 트리거 개체의 [**RequestAsync**](https://msdn.microsoft.com/library/windows/apps/dn297341) 활성화 메서드를 호출합니다.
6.  사용자 백그라운드 작업은 다른 시스템 백그라운드 작업처럼 제한되지 않지만(CPU 시간 할당량 없음), 포그라운드 앱의 응답을 유지하기 위해 감소된 우선 순위로 실행됩니다.
7.  Windows는 트리거 유형에 따라 백그라운드 작업을 시작하기 전에 작업에 대한 사용자 동의 요청을 포함하여 필요한 정책이 충족되었는지 확인합니다.
8.  Windows는 시스템 조건 및 작업 런타임을 모니터링하고, 필요한 경우 필수 조건이 더 이상 충족되지 않으면 작업을 취소합니다.
9.  백그라운드 작업이 진행 중 또는 완료를 보고하면 앱은 등록된 작업의 진행 중 및 완료 이벤트를 통해 이러한 이벤트를 받게 됩니다.

**중요**  
[**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)를 사용할 때 다음과 같은 중요한 점을 고려해야 합니다.

-   
  [
    **DeviceUseTrigger**
  ](https://msdn.microsoft.com/library/windows/apps/dn297337)를 사용하는 백그라운드 작업을 프로그래밍 방식으로 트리거하는 기능은 Windows8.1 및 Windows Phone 8.1에서 처음 소개되었습니다.

-   PC에서 주변 장치를 업데이트할 때 사용자 동의를 확인하기 위해 Windows에서 특정 정책을 적용합니다.

-   추가 정책은 주변 장치를 동기화 및 업데이트할 때 사용자 배터리 사용 시간을 보존하기 위해 적용됩니다.

-   최대 백그라운드 시간(벽시계 시간)을 포함하여 특정 정책 요구 사항이 더 이상 충족되지 않을 경우 Windows에서 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)를 사용하는 백그라운드 작업을 취소할 수 있습니다. 백그라운드 작업을 사용하여 주변 장치를 조작하는 경우 이러한 정책 요구 사항을 고려 하는 것이 중요합니다.

**팁**  백그라운드 작업의 작동 방식을 확인하려면 샘플을 다운로드하세요. PC에서 이 작업을 만드는 방법을 보여 주는 예제는 [사용자 지정 USB 장치 샘플](http://go.microsoft.com/fwlink/p/?LinkId=301975 )을 참조하세요. 휴대전화에 대한 예제는 [백그라운드 센서 샘플](http://go.microsoft.com/fwlink/p/?LinkId=393307)을 참조하세요.
 
## <a name="frequency-and-foreground-restrictions"></a>빈도 및 포그라운드 제한

앱이 작업을 시작할 수 있는 빈도에는 제한이 없지만 앱은 한 번에 하나의 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 백그라운드 작업만 실행할 수 있으며(다른 형식의 백그라운드 작업에는 영향을 미치지 않음), 앱이 포그라운드에 있을 때만 백그라운드 작업을 시작할 수 있습니다. 앱이 포그라운드에 없는 경우 **DeviceUseTrigger**로 백그라운드 작업을 시작할 수 없습니다. 첫 번째 백그라운드 작업이 완료되기 전에는 앱이 두 번째 **DeviceUseTrigger** 백그라운드 작업을 시작할 수 없습니다.

## <a name="device-restrictions"></a>장치 제한 사항

각 앱은 하나의 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 백그라운드 작업만 등록하여 실행하도록 제한되지만, 디바이스(앱이 실행 중인)에서 여러 앱이 **DeviceUseTrigger** 백그라운드 작업을 등록하고 실행하도록 허용할 수 있습니다. 장치에 따라 모든 앱에서 수행되는 **DeviceUseTrigger** 백그라운드 작업의 총 수가 제한될 수 있습니다. 따라서 리소스가 제한된 장치에서 배터리를 유지하는 데 도움이 됩니다. 자세한 내용은 다음 표를 참조하세요.

앱은 단일 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 백그라운드 작업을 통해 주변 장치나 센서를 개수에 제한 없이 액세스할 수 있습니다. 이전 표에 나열된 지원 API와 프로토콜에 따라서만 액세스가 제한됩니다.

## <a name="background-task-policies"></a>백그라운드 작업 정책

Windows는 앱이 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 백그라운드 작업을 사용할 때 정책을 적용합니다. 이러한 정책이 충족되지 않으면 백그라운드 작업이 취소될 수 있습니다. 이 유형의 백그라운드 작업을 사용하여 장치 또는 센서를 조작하는 경우 이러한 정책 요구 사항을 고려하는 것이 중요합니다.

### <a name="task-initiation-policies"></a>작업 시작 정책

이 표에는 유니버설 Windows 앱에 적용되는 작업 시작 정책이 나와 있습니다.

| 정책 | 유니버설 Windows 앱의 DeviceUseTrigger |
|--------|---------------------------------------------|
| 백그라운드 작업을 트리거할 때 앱은 포그라운드에 있습니다. | ![정책이 적용됨](images/ap-tools.png) |
| 장치가 시스템에 연결되어 있거나 무선 장치의 범위에 있습니다. | ![정책이 적용됨](images/ap-tools.png) |
| 지원되는 주변 장치 API(USB, HID, Bluetooth, 센서 등의 Windows 런타임 API)를 사용하여 앱이 장치에 액세스할 수 있습니다. 앱이 장치 또는 센서에 액세스할 수 없는 경우 백그라운드 작업에 대한 액세스가 거부됩니다. | ![정책이 적용됨](images/ap-tools.png) |
| 앱에서 제공하는 백그라운드 작업 진입점이 앱 패키지 매니페스트에 등록되어 있습니다. | ![정책이 적용됨](images/ap-tools.png) |
| 앱당 하나의 [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/dn297337) 백그라운드 작업만 실행합니다. | ![정책이 적용됨](images/ap-tools.png) |
| 앱이 실행 중인 장치에서 아직 [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/dn297337) 백그라운드 작업의 최대 수에 도달하지 않았습니다. | **데스크톱 장치 패밀리**: 개수에 제한 없이 작업을 등록하고 병렬로 실행할 수 있습니다. **모바일 장치 패밀리**: 512MB 장치에서는 1개의 작업, 그 외 장치에서는 2개의 작업을 등록하고 병렬로 실행할 수 있습니다. |
| 지원되는 API/프로토콜을 사용할 때 앱이 단일 [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/dn297337) 백그라운드 작업에서 액세스할 수 있는 주변 장치나 센서의 최대수입니다. | 제한 없음 |
| 백그라운드 작업은 화면이 잠겨 있는 경우 분당 또는 화면이 잠겨 있지 않은 경우 5분당 400ms CPU 시간(1GHz CPU 가정)을 소모합니다. 이 정책을 준수하지 않으면 작업이 취소될 수 있습니다. | ![정책이 적용됨](images/ap-tools.png) |

### <a name="runtime-policy-checks"></a>런타임 정책 검사

작업이 백그라운드에서 실행되는 동안 Windows는 다음과 같은 런타임 정책 요구 사항을 적용합니다. 런타임 요구 사항 중 하나라도 true가 아니면 장치 백그라운드 작업이 취소됩니다.

이 표에는 유니버설 Windows 앱에 적용되는 런타임 정책이 나와 있습니다.

| 정책 검사 | 유니버설 Windows 앱의 DeviceUseTrigger |
|--------------|:-------------------------------------------:|
| 장치가 시스템에 연결되어 있거나 무선 장치의 범위에 있습니다. | ![정책 검사가 적용됨](images/ap-tools.png) |
| 작업에서 장치에 정기적인 I/O를 수행하고 있습니다(5초마다 1 I/O). | ![정책 검사가 적용됨](images/ap-tools.png) |
| 앱이 작업을 취소하지 않았습니다. | ![정책 검사가 적용됨](images/ap-tools.png) |
| 벽시계 시간 제한 – 앱 작업이 백그라운드에서 실행될 수 있는 총 시간입니다. | **데스크톱 장치 패밀리**: 10분. **모바일 장치 패밀리**: 시간 제한 없음. 리소스를 절약하기 위해 한 번에 한두 개 작업만 실행할 수 있습니다. |
| 앱이 종료되지 않았습니다. | ![정책 검사가 적용됨](images/ap-tools.png) |

## <a name="best-practices"></a>모범 사례

다음은 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 백그라운드 작업을 사용하는 앱의 모범 사례입니다.

### <a name="programming-a-background-task"></a>백그라운드 작업 프로그래밍

앱에서 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 백그라운드 작업을 사용하면 사용자가 앱을 전환하고 Windows에서 포그라운드 앱을 일시 중단한 경우에도 포그라운드에서 시작된 동기화 또는 모니터링 작업이 백그라운드에서 계속 실행됩니다. 백그라운드 작업을 등록, 트리거 및 등록 취소하기 위한 다음과 같은 전체 모델을 따르는 것이 좋습니다.

1.  앱이 백그라운드 작업을 요청할 수 있는지 확인하려면 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)를 호출합니다. 백그라운드 작업을 등록하기 전에 이 호출을 완료해야 합니다.

2.  트리거를 요청하기 전에 백그라운드 작업을 등록합니다.

3.  진행 중 및 완료 이벤트 처리기를 트리거에 연결합니다. 앱이 일시 중단에서 반환되면 Windows는 백그라운드 작업의 상태를 확인하는 데 사용할 수 있는 대기 진행 중 또는 완료 이벤트를 앱에 제공합니다.

4.  [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 백그라운드 작업을 트리거할 때 열려 있는 장치 또는 센서 개체를 모두 닫아 백그라운드 작업이 이러한 장치와 센서를 열고 사용할 수 있게 합니다.

5.  트리거를 등록합니다.

6.  백그라운드 작업에서 장치나 센서에 액세스할 때 배터리에 미치는 영향을 신중히 고려해야 합니다. 예를 들어, 센서의 보고 간격이 너무 잦으면 작업이 너무 자주 실행되어 휴대전화의 배터리가 소모됩니다.

7.  백그라운드 작업이 완료되면 트리거의 등록을 취소합니다.

8.  백그라운드 작업 클래스에서 취소 이벤트에 등록합니다. 취소 이벤트에 등록하면 백그라운드 작업 코드를 통해 Windows 또는 포그라운드 앱에서 취소한 실행 중인 백그라운드 작업을 깔끔하게 중지할 수 있습니다.

9.  앱이 일시 중단이 아니라 종료되는 경우, 실행 중인 작업이 앱에 더 이상 필요하지 않으면 작업의 등록을 해제하고 취소합니다. 그러면 메모리 부족 휴대전화와 같이 리소스가 제한된 시스템에서 다른 앱이 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 백그라운드 작업을 사용할 수 있게 됩니다.

    -   앱을 종료할 때 실행 중인 모든 작업을 등록 취소하고 취소합니다.

    -   앱을 종료하면 백그라운드 작업이 취소되고 기존 백그라운드 작업에서 기존 이벤트 처리기의 연결이 끊어집니다. 이 경우 백그라운드 작업의 상태를 확인할 수 없습니다. 백그라운드 작업을 등록 취소하고 취소하면 취소 코드를 통해 백그라운드 작업을 깔끔하게 중지할 수 있습니다.

### <a name="cancelling-a-background-task"></a>백그라운드 작업 취소

백그라운드로 실행 중인 작업을 포그라운드 앱에서 취소하려면 앱에서 사용하는 [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786) 개체의 Unregister 메서드를 사용하여 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 백그라운드 작업을 등록합니다. **BackgroundTaskRegistration**에서 [**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229869) 메서드를 사용하여 백그라운드 작업의 등록을 취소하면 백그라운드 작업 인프라가 백그라운드 작업을 취소하게 됩니다.

또한 [**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229869) 메서드는 부울 true 또는 false 값을 사용하여 현재 실행 중인 백그라운드 작업 인스턴스가 완료되기 전에 취소해야 하는지 여부를 나타냅니다. 자세한 내용은 **Unregister**의 API 참조를 확인하세요.

[**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229869) 외에도 앱은 [**BackgroundTaskDeferral.Complete**](https://msdn.microsoft.com/library/windows/apps/hh700504)를 호출해야 합니다. 그러면 백그라운드 작업과 관련된 비동기 작업이 완료되었음을 시스템에 알립니다.
