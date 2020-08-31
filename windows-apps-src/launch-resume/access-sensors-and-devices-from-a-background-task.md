---
title: 백그라운드 작업에서 센서 및 디바이스에 액세스
description: DeviceUseTrigger를 사용하면 포그라운드 앱이 일시 중단된 경우에도 유니버설 Windows 앱이 백그라운드로 센서와 주변 기기에 액세스할 수 있습니다.
ms.assetid: B540200D-9FF2-49AF-A224-50877705156B
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 백그라운드 작업
ms.localizationpriority: medium
ms.openlocfilehash: 1939766c2fbb215980143c33f82bca669dc4df0e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167937"
---
# <a name="access-sensors-and-devices-from-a-background-task"></a>백그라운드 작업에서 센서 및 디바이스에 액세스




[**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger)를 사용하면 포그라운드 앱이 일시 중단된 경우에도 유니버설 Windows 앱이 백그라운드로 센서와 주변 기기에 액세스할 수 있습니다. 예를 들어 앱이 실행 되는 위치에 따라 백그라운드 작업을 사용 하 여 장치와 데이터를 동기화 하거나 센서를 모니터링할 수 있습니다. 배터리 수명을 유지 하 고 적절 한 사용자 동의를 보장 하려면이 항목에 설명 된 정책에 따라 [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) 을 사용 합니다.

백그라운드에서 센서 또는 주변 장치에 액세스 하려면 [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger)를 사용 하는 백그라운드 작업을 만듭니다. PC에서이 작업을 수행 하는 방법을 보여 주는 예제는 [사용자 지정 USB 장치 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomUsbDeviceAccess)을 참조 하세요. 휴대폰에 대 한 예제는 [배경 센서 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundSensors)을 참조 하세요.

> [!Important]
> **DeviceUseTrigger** 는 in-process 백그라운드 작업과 함께 사용할 수 없습니다. 이 항목의 정보는 out-of-process로 실행 되는 백그라운드 작업에만 적용 됩니다.

## <a name="device-background-task-overview"></a>장치 배경 작업 개요

앱이 사용자에 게 더 이상 표시 되지 않으면 Windows는 메모리 및 CPU 리소스를 회수 하기 위해 앱을 일시 중단 하거나 종료 합니다. 이렇게 하면 다른 앱을 포그라운드에서 실행 하 고 배터리 소비를 줄일 수 있습니다. 이 경우 백그라운드 작업을 지원 하지 않으면 진행 중인 데이터 이벤트가 모두 손실 됩니다. Windows는 앱이 일시 중단 된 경우에도 백그라운드에서 장치 및 센서에 대해 장기 실행 동기화 및 모니터링 작업을 안전 하 게 수행할 수 있도록 백그라운드 작업 트리거 [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger)을 제공 합니다. 앱 수명 주기에 대 한 자세한 내용은 [시작, 다시 시작 및 백그라운드 작업](index.md)을 참조 하세요. 백그라운드 작업에 대 한 자세한 내용은 [백그라운드 작업을 사용 하 여 앱 지원](support-your-app-with-background-tasks.md)을 참조 하세요.

**참고**    유니버설 Windows 앱의 경우 백그라운드에서 장치를 동기화 하려면 사용자가 앱의 백그라운드 동기화를 승인 해야 합니다. 장치는 활성 i/o를 사용 하 여 PC에 연결 되거나 PC와 쌍으로 연결 되어야 하며, 최대 10 분의 백그라운드 작업을 허용 합니다. 정책 적용에 대 한 자세한 내용은이 항목의 뒷부분에 설명 되어 있습니다.

### <a name="limitation-critical-device-operations"></a>제한 사항: 중요 한 장치 작업

장기 실행 펌웨어 업데이트와 같은 일부 중요 한 장치 작업은 [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger)를 사용 하 여 수행할 수 없습니다. 이러한 작업은 PC 에서만 수행할 수 있으며 [**DeviceServicingTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceServicingTrigger)를 사용 하는 권한 있는 앱 에서만 수행할 수 있습니다. *권한 있는 앱* 은 장치의 제조업체에서 해당 작업을 수행할 수 있는 권한을 부여 받은 앱입니다. 장치 메타 데이터는 장치에 대해 권한 있는 앱으로 지정 된 앱 (있는 경우)을 지정 하는 데 사용 됩니다. 자세한 내용은 장치 [동기화 및 Microsoft Store 장치 앱 업데이트](/windows-hardware/drivers/devapps/device-sync-and-update-for-uwp-device-apps)를 참조 하세요.

## <a name="protocolsapis-supported-in-a-deviceusetrigger-background-task"></a>DeviceUseTrigger 백그라운드 작업에서 지원 되는 프로토콜/a p i

[**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) 를 사용 하는 백그라운드 작업을 통해 앱은 많은 프로토콜/a p i를 통해 통신할 수 있습니다. 대부분의 시스템 트리거 백그라운드 작업에서 지원 되지 않습니다. 유니버설 Windows 앱에서 지원 되는 기능은 다음과 같습니다.

| 프로토콜         | 유니버설 Windows 앱의 DeviceUseTrigger                                                                                                                                                    |
|------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| USB              | ![이 프로토콜은 지원 되지 않습니다.](images/ap-tools.png)                                                                                                                                            |
| 숨겼습니다              | ![이 프로토콜은 지원 되지 않습니다.](images/ap-tools.png)                                                                                                                                            |
| Bluetooth RFCOMM | ![이 프로토콜은 지원 되지 않습니다.](images/ap-tools.png)                                                                                                                                            |
| Bluetooth GATT   | ![이 프로토콜은 지원 되지 않습니다.](images/ap-tools.png)                                                                                                                                            |
| MTP              | ![이 프로토콜은 지원 되지 않습니다.](images/ap-tools.png)                                                                                                                                            |
| 네트워크 유선    | ![이 프로토콜은 지원 되지 않습니다.](images/ap-tools.png)                                                                                                                                            |
| 네트워크 Wi-fi    | ![이 프로토콜은 지원 되지 않습니다.](images/ap-tools.png)                                                                                                                                            |
| IDeviceIOControl | ![deviceservicingtrigger 지원 ideviceiocontrol](images/ap-tools.png)                                                                                                                       |
| 센서 API      | ![deviceservicingtrigger는 유니버설 센서 api ](images/ap-tools.png) ( [유니버설 장치 제품군](../get-started/universal-application-platform-guide.md)의 센서로 제한 됨)를 지원 합니다. |

## <a name="registering-background-tasks-in-the-app-package-manifest"></a>앱 패키지 매니페스트에 백그라운드 작업 등록

앱은 백그라운드 작업의 일부로 실행 되는 코드에서 동기화 및 업데이트 작업을 수행 합니다. 이 코드는 [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) (또는 javascript 앱에 대 한 전용 javascript 페이지)를 구현 하는 Windows 런타임 클래스에 포함 되어 있습니다. [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) 백그라운드 작업을 사용 하려면 앱이 포그라운드 앱의 응용 프로그램 매니페스트 파일에서이 작업을 선언 해야 합니다. 예를 들어 시스템에서 트리거된 백그라운드 작업을 수행 하는 것과 같습니다.

앱 패키지 매니페스트 파일의이 예제에서 **Devicelibrary. SyncContent** 는 [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger)을 사용 하는 백그라운드 작업의 진입점입니다.

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

[**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger)를 사용 하려면 다음 기본 단계를 수행 합니다. 백그라운드 작업에 대 한 자세한 내용은 [백그라운드 작업을 사용 하 여 앱 지원](support-your-app-with-background-tasks.md)을 참조 하세요.

1.  앱은 응용 프로그램 매니페스트에 백그라운드 작업을 등록 하 고 IBackgroundTask를 구현 하는 Windows 런타임 클래스 또는 JavaScript 앱에 대 한 전용 JavaScript 페이지에 백그라운드 작업 코드를 포함 합니다.
2.  앱이 시작 되 면 [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger)형식의 트리거 개체를 만들고 구성 하 고 나중에 사용할 수 있도록 트리거 인스턴스를 저장 합니다.
3.  앱은 백그라운드 작업이 이전에 등록 되었는지 여부를 확인 하 고, 그렇지 않은 경우 트리거에 대해 등록 합니다. 앱에서이 트리거와 연결 된 작업에 대 한 조건을 설정할 수 없습니다.
4.  앱이 백그라운드 작업을 트리거해야 하는 경우 먼저 [**Requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) 를 호출 하 여 앱이 백그라운드 작업을 요청할 수 있는지 확인 해야 합니다.
5.  앱이 백그라운드 작업을 요청할 수 있는 경우 장치 트리거 개체에서 [**Requestasync**](/uwp/api/windows.applicationmodel.background.deviceusetrigger.requestasync) 활성화 메서드를 호출 합니다.
6.  백그라운드 작업은 다른 시스템 백그라운드 작업 (CPU 시간 할당량이 없음)과 같이 제한 되지 않지만 포그라운드 앱의 응답성을 유지 하기 위해 낮은 우선 순위로 실행 됩니다.
7.  그러면 Windows에서 백그라운드 작업을 시작 하기 전에 작업에 대 한 사용자 동의를 요청 하는 것을 포함 하 여 필요한 정책이 충족 되었는지 트리거 유형에 따라 유효성을 검사 합니다.
8.  Windows에서 시스템 조건 및 작업 런타임을 모니터링 하 고 필요한 경우 더 이상 충족 되지 않을 경우 작업을 취소 합니다.
9.  백그라운드 작업에서 진행률 또는 완료를 보고 하면 앱은 등록 된 작업의 진행 및 완료 된 이벤트를 통해 이러한 이벤트를 수신 합니다.

**중요**    [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger)를 사용할 때 고려해 야 할 중요 사항은 다음과 같습니다.

-   [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) 를 사용 하는 백그라운드 작업을 프로그래밍 방식으로 트리거하는 기능은 Windows 8.1 및 Windows Phone 8.1에 처음 도입 되었습니다.

-   PC에서 주변 장치를 업데이트할 때 사용자 동의를 보장 하기 위해 Windows에서 특정 정책을 적용 합니다.

-   추가 정책은 주변 장치를 동기화 하 고 업데이트할 때 사용자 배터리 수명을 유지 하기 위해 적용 됩니다.

-   [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) 를 사용 하는 백그라운드 작업은 최대 백그라운드 시간 (벽 시계 시간)을 포함 하 여 특정 정책 요구 사항이 더 이상 충족 되지 않을 때 Windows에서 취소할 수 있습니다. 이러한 백그라운드 작업을 사용 하 여 주변 장치와 상호 작용 하는 경우 이러한 정책 요구 사항을 고려 하는 것이 중요 합니다.

**팁**    이러한 백그라운드 작업이 작동 하는 방식을 보려면 샘플을 다운로드 합니다. PC에서이 작업을 수행 하는 방법을 보여 주는 예제는 [사용자 지정 USB 장치 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomUsbDeviceAccess)을 참조 하세요. 휴대폰에 대 한 예제는 [배경 센서 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundSensors)을 참조 하세요.
 
## <a name="frequency-and-foreground-restrictions"></a>빈도 및 포그라운드 제한 사항

앱에서 작업을 시작할 수 있는 빈도에 대 한 제한은 없지만 앱은 한 번에 하나의 [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) 백그라운드 작업 작업만 실행할 수 있습니다 (다른 유형의 백그라운드 작업에는 영향을 주지 않음). 앱이 전경에 있는 동안에만 백그라운드 작업을 시작할 수 있습니다. 앱이 전경에 있지 않으면 **DeviceUseTrigger**를 사용 하 여 백그라운드 작업을 시작할 수 없습니다. 첫 번째 백그라운드 작업이 완료 되기 전에 앱이 두 번째 **DeviceUseTrigger** 백그라운드 작업을 시작할 수 없습니다.

## <a name="device-restrictions"></a>디바이스 제한 사항

각 앱은 하나의 [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) 백그라운드 작업을 등록 하 고 실행 하는 것으로 제한 되지만 앱이 실행 되는 장치에서는 여러 앱이 **DeviceUseTrigger** 백그라운드 작업을 등록 하 고 실행할 수 있습니다. 장치에 따라 모든 앱에서 **DeviceUseTrigger** 백그라운드 작업의 총 수에 제한이 있을 수 있습니다. 이렇게 하면 리소스가 제한 된 장치에서 배터리를 보존할 수 있습니다. 자세한 내용은 다음 표를 참조 하세요.

단일 [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) 백그라운드 작업에서 앱은 지원 되는 api와 이전 표에 나와 있는 프로토콜에 의해서만 제한 되는 수에 제한 없이 장치 또는 센서에 액세스할 수 있습니다.

## <a name="background-task-policies"></a>백그라운드 작업 정책

앱에서 [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) 백그라운드 작업을 사용 하는 경우 Windows에서 정책을 적용 합니다. 이러한 정책이 충족 되지 않는 경우 백그라운드 작업이 취소 될 수 있습니다. 이러한 유형의 백그라운드 작업을 사용 하 여 장치 또는 센서와 상호 작용 하는 경우 이러한 정책 요구 사항을 고려 하는 것이 중요 합니다.

### <a name="task-initiation-policies"></a>작업 시작 정책

이 표는 유니버설 Windows 앱에 적용 되는 작업 시작 정책을 나타냅니다.

| 정책 | 유니버설 Windows 앱의 DeviceUseTrigger |
|--------|---------------------------------------------|
| 백그라운드 작업을 트리거할 때 앱이 전경에 있습니다. | ![정책 적용](images/ap-tools.png) |
| 장치가 시스템에 연결 되어 있거나 무선 장치의 범위에 있습니다. | ![정책 적용](images/ap-tools.png) |
| 장치는 지원 되는 장치 주변 Api (USB, HID, Bluetooth, 센서 등의 Windows 런타임 Api)를 사용 하 여 앱에서 액세스할 수 있습니다. 앱에서 장치 또는 센서에 액세스할 수 없는 경우 백그라운드 작업에 대 한 액세스가 거부 됩니다. | ![정책 적용](images/ap-tools.png) |
| 앱에서 제공 하는 백그라운드 작업 진입점이 앱 패키지 매니페스트에 등록 되어 있습니다. | ![정책 적용](images/ap-tools.png) |
| 앱 당 하나의 [DeviceUseTrigger](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) 백그라운드 작업만 실행 됩니다. | ![정책 적용](images/ap-tools.png) |
| 앱이 실행 되는 장치에서 [DeviceUseTrigger](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) 백그라운드 작업의 최대 수가 아직 도달 하지 않았습니다. | **데스크톱 장치 제품군**: 무제한의 태스크를 등록 하 여 병렬로 실행할 수 있습니다. **모바일 장치 제품군**: 512 MB 장치에 대 한 작업 1 개 그렇지 않으면 2 개의 작업을 등록 하 여 병렬로 실행할 수 있습니다. |
| 지원 되는 Api/프로토콜을 사용 하는 경우 앱에서 단일 [DeviceUseTrigger](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) 백그라운드 작업을 통해 액세스할 수 있는 최대 장치 또는 센서의 수입니다. | 무제한 |
| 백그라운드 작업은 화면이 잠길 때 분 마다 400 밀리초의 CPU 시간을 사용 하 고, 화면이 잠기지 않으면 5 분 마다 사용 합니다. 이 정책을 충족 하지 못하면 작업 취소가 발생할 수 있습니다. | ![정책 적용](images/ap-tools.png) |

### <a name="runtime-policy-checks"></a>런타임 정책 검사

작업을 백그라운드에서 실행 하는 동안 Windows에서 다음과 같은 런타임 정책 요구 사항을 적용 합니다. 런타임 요구 사항이 하나라도 충족 되지 않으면 Windows는 장치 백그라운드 작업을 취소 합니다.

이 표는 유니버설 Windows 앱에 적용 되는 런타임 정책을 나타냅니다.

| 정책 확인 | 유니버설 Windows 앱의 DeviceUseTrigger |
|--------------|:-------------------------------------------:|
| 장치가 시스템에 연결 되어 있거나 무선 장치의 범위에 있습니다. | ![정책 확인 적용](images/ap-tools.png) |
| 작업에서 장치에 대 한 일반 i/o를 수행 하 고 있습니다 (5 초 마다 1 개의 i/o). | ![정책 확인 적용](images/ap-tools.png) |
| 앱에서 작업을 취소 하지 않았습니다. | ![정책 확인 적용](images/ap-tools.png) |
| 벽 시계 시간 제한 – 백그라운드에서 앱의 작업을 실행할 수 있는 총 시간입니다. | **데스크톱 장치 제품군**: 10 분 **모바일 장치 제품군**: 시간 제한이 없습니다. 리소스를 절약 하기 위해 한 번에 1 ~ 2 개 작업을 실행할 수 있습니다. |
| 앱이 종료 되지 않았습니다. | ![정책 확인 적용](images/ap-tools.png) |

## <a name="best-practices"></a>모범 사례

[**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) 백그라운드 작업을 사용 하는 앱에 대 한 모범 사례는 다음과 같습니다.

### <a name="programming-a-background-task"></a>백그라운드 작업 프로그래밍

앱에서 [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) 백그라운드 작업을 사용 하면 사용자가 앱을 전환 하 고 포그라운드 앱이 Windows에서 일시 중단 된 경우 포그라운드 앱에서 시작 된 동기화 또는 모니터링 작업이 백그라운드에서 계속 실행 됩니다. 백그라운드 작업 등록, 트리거 및 등록 취소를 위한 전체 모델을 따르는 것이 좋습니다.

1.  [**Requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) 를 호출 하 여 앱이 백그라운드 작업을 요청할 수 있는지 확인 합니다. 백그라운드 작업을 등록 하기 전에이 작업을 수행 해야 합니다.

2.  트리거를 요청 하기 전에 백그라운드 작업을 등록 합니다.

3.  트리거에 진행률 및 완료 이벤트 처리기를 연결 합니다. 앱이 일시 중단에서 반환 되 면 Windows는 백그라운드 작업의 상태를 확인 하는 데 사용할 수 있는 대기 중인 진행률 또는 완료 이벤트를 앱에 제공 합니다.

4.  백그라운드 작업에서 이러한 장치나 센서를 자유롭게 열고 사용할 수 있도록 [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) 백그라운드 작업을 트리거할 때 열려 있는 모든 장치 또는 센서 개체를 닫습니다.

5.  트리거를 등록 합니다.

6.  장치에 액세스 하는 경우 또는 백그라운드 작업에서 배터리의 영향을 신중 하 게 고려해 야 합니다. 예를 들어 센서의 보고서 간격이 너무 자주 실행 되 면 작업을 실행 하 여 휴대폰의 배터리를 신속 하 게 드레이닝 하 게 될 수 있습니다.

7.  백그라운드 작업이 완료 되 면 등록을 취소 합니다.

8.  백그라운드 작업 클래스에서 취소 이벤트를 등록 합니다. 취소 이벤트를 등록 하면 Windows 또는 포그라운드 앱에서 취소 된 경우 백그라운드 작업 코드가 실행 중인 백그라운드 작업을 완전히 중지할 수 있습니다.

9.  앱 종료 (일시 중단 안 함)에서 앱이 더 이상 필요 하지 않은 경우 실행 중인 작업을 등록 취소 하 고 취소 합니다. 메모리 부족 전화와 같은 리소스 제한 시스템에서는 다른 앱이 [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) 백그라운드 작업을 사용할 수 있습니다.

    -   앱이 종료 되 면 실행 중인 작업을 등록 취소 하 고 취소 합니다.

    -   앱이 종료 되 면 백그라운드 작업은 취소 되 고 기존 이벤트 처리기는 기존 백그라운드 작업과의 연결을 끊습니다. 이렇게 하면 백그라운드 작업의 상태를 확인할 수 없습니다. 백그라운드 작업을 등록 취소 하 고 취소 하면 취소 코드에서 백그라운드 작업을 완전히 중지할 수 있습니다.

### <a name="cancelling-a-background-task"></a>백그라운드 작업 취소

포그라운드 앱의 백그라운드에서 실행 중인 작업을 취소 하려면 앱에서 사용 하는 [**BackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) 개체에 대해 등록 취소 메서드를 사용 하 여 [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) 백그라운드 작업을 등록 합니다. **BackgroundTaskRegistration** 에서 [**등록 취소**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.unregister) 메서드를 사용 하 여 백그라운드 작업 등록을 취소 하면 백그라운드 작업 인프라가 백그라운드 작업을 취소 하 게 됩니다.

또한 [**등록 취소**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.unregister) 메서드는 백그라운드 작업의 현재 실행 중인 인스턴스를 완료 하도록 허용 하지 않고 취소 해야 하는지 여부를 나타내는 부울 true 또는 false 값을 추가로 사용 합니다. 자세한 내용은 **등록 취소**에 대 한 API 참조를 참조 하세요.

[**등록을 취소**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.unregister)하는 것 외에도 앱은 [**BackgroundTaskDeferral**](/uwp/api/windows.applicationmodel.background.backgroundtaskdeferral.complete)를 호출 해야 합니다. 이렇게 하면 백그라운드 작업과 연결 된 비동기 작업이 완료 되었음을 시스템에 알립니다.