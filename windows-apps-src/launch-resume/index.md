---
author: TylerMSFT
title: 실행, 다시 시작 및 백그라운드 작업
description: 이 섹션에서는 UWP(유니버설 Windows 플랫폼) 앱을 시작, 일시 중단, 다시 시작 및 종료할 때 발생하는 상황을 설명합니다.
ms.assetid: 75011D52-1511-4ECF-9DF6-52CBBDB15BD7
ms.author: twhitney
ms.date: 10/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 백그라운드 작업, 응용 프로그램 서비스에 연결 된 장치, 원격 시스템
ms.localizationpriority: medium
ms.openlocfilehash: 142eba8eb919ed25632f44a6f185ae40e16dec6b
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/23/2018
ms.locfileid: "2810289"
---
# <a name="launching-resuming-and-background-tasks"></a>실행, 다시 시작 및 백그라운드 작업


이 섹션의 내용은 다음과 같습니다.

- UWP(유니버설 Windows 플랫폼) 앱을 실행, 일시 중단, 다시 시작 및 종료할 때 발생하는 상황
- URI를 사용하거나 파일 활성화를 통해 앱을 실행하는 방법
- UWP(유니버설 Windows) 앱이 다른 앱과 데이터 및 기능을 공유할 수 있도록 하는 앱 서비스를 사용하는 방법
- UWP 앱 자체가 포그라운드에 없는 동안 작동할 수 있도록 하는 백그라운드 작업을 사용하는 방법
- 장치 간에 일관된 사용자 환경을 만들 수 있도록 연결된 장치를 검색하고, 다른 장치에서 앱을 실행하고, 원격 장치의 앱 서비스와 통신하는 방법
- 앱 확장 및 구성 요소화를 위한 올바른 기술을 선택하는 방법
- 앱의 시작 화면을 추가 및 구성하는 방법
- 사용자가 Microsoft Store에서 설치할 수 있는 패키지를 통해 앱 확장을 작성하는 방법

## <a name="the-app-lifecycle"></a>앱 수명 주기

이 섹션에서는 활성화된 시점부터 닫힐 때까지의 Windows 10 UWP(유니버설 Windows 플랫폼) 앱 수명 주기를 자세히 설명합니다.

| 항목 | 설명 |
|-------|-------------|
| [앱 수명 주기](app-lifecycle.md)               | UWP 앱의 수명 주기 및 Windows에서 앱을 실행, 일시 중단 및 다시 시작할 때 발생하는 상황을 알아봅니다. |
| [앱 사전 실행 처리](handle-app-prelaunch.md) | 앱 사전 실행 처리 방법에 대해 알아봅니다.                                                                              |
| [앱 활성화 처리](activate-an-app.md)     | 앱 활성화 처리 방법에 대해 알아봅니다.                                                                             |
| [앱 일시 중단 처리](suspend-an-app.md)         | 시스템에서 앱을 일시 중단할 때 중요한 응용 프로그램 데이터를 저장하는 방법을 배웁니다.                                 |
| [앱 다시 시작 처리](resume-an-app.md)           | 시스템에서 앱을 다시 시작할 때 표시 콘텐츠를 새로 고치는 방법을 알아봅니다.                                        |
| [앱이 백그라운드로 이동할 때 메모리 회수](reduce-memory-usage.md) | 앱이 종료되지 않도록 백그라운드 상태에 있을 때 앱에서 사용하는 메모리 양을 줄이는 방법을 알아봅니다.|
| [확장 실행을 사용하여 앱 일시 중단 연기](run-minimized-with-extended-execution.md) | 확장 실행을 사용하여 앱이 최소화된 상태에서 계속 실행되도록 하는 방법을 살펴봅니다. |

## <a name="launch-apps"></a>앱 실행

| 항목 | 설명 |
|-------|-------------|
| [유니버설 Windows 플랫폼 콘솔 앱 만들기](console-uwp.md) | 콘솔 창에서 실행되는 유니버설 Windows 플랫폼 앱을 작성하는 방법을 알아보세요. |
| [다중 인스턴스 UWP 앱 만들기](multi-instance-uwp.md) | 다중 인스턴스 유니버설 Windows 플랫폼 앱을 작성하는 방법을 알아보세요. |

[URI를 사용하여 앱 실행](launch-app-with-uri.md) 섹션에서는 URI(Uniform Resource Identifier)를 사용하여 앱을 실행하는 방법을 자세히 설명합니다.

| 항목 | 설명 |
|-------|-------------|
| [URI에 대한 기본 앱 실행](launch-default-app.md) | URI(Uniform Resource Identifier)에 대한 기본 앱 시작 방법을 알아봅니다. URI를 사용하면 다른 앱을 실행하여 특정 작업을 수행할 수 있습니다. 이 항목에서는 Windows에 기본 제공되는 다양한 URI 스키마에 대해서도 간략하게 설명합니다. |
| [URI 활성화 처리](handle-uri-activation.md) | 앱을 URI(Uniform Resource Identifier) 체계 이름의 기본 처리기로 등록하는 방법을 알아봅니다. |
| [결과를 위한 앱 실행](how-to-launch-an-app-for-results.md) | 다른 앱에서 앱을 시작하고 두 사이에서 데이터를 교환하는 방법을 알아봅니다. 이를 결과를 위한 앱 실행이라고 합니다. |
| [ms-tonepicker URI 체계를 사용하여 톤 선택 및 저장](launch-ringtone-picker.md) | 이 항목에서는 ms-tonepicker URI 체계 및 이 체계를 통해 톤 선택기를 표시하여 톤을 선택하고, 톤을 저장하고, 톤의 식별 이름을 가져오는 방법을 설명합니다. |
| [Windows 설정 앱 실행](launch-settings-app.md) | 앱에서 Windows 설정 앱을 시작하는 방법을 알아봅니다. 이 항목에서는 ms-settings URI 체계에 대해 설명합니다. 이 URI 스키마로 Windows 설정 앱을 실행하여 특정 설정 페이지를 표시할 수 있습니다. |
| [Microsoft Store 앱 실행](launch-store-app.md) | 이 항목에서는 ms-windows-store URI 스키마에 대해 설명합니다. 이 URI 스키마로 UWP 앱을 실행하여 Microsoft Store의 특정 페이지를 표시할 수 있습니다. |
| [Windows 지도 앱 실행](launch-maps-app.md) | 앱에서 Windows 지도 앱을 실행하는 방법을 알아봅니다. |
| [피플 앱 실행](launch-people-apps.md) | 이 항목에서는 ms-people URI 체계에 대해 설명합니다. 앱에서 이 URI 체계를 사용하여 특정 작업을 위한 피플 앱을 실행할 수 있습니다. |
| [앱 URI 처리기로 웹과 앱 연결 지원](web-to-app-linking.md) | 앱 URI 처리기를 사용하여 사용자의 앱 참여를 강화합니다. |

[파일 활성화를 통해 앱 실행](launch-app-from-file.md) 섹션에서는 특정 형식의 파일을 열 때 실행되도록 앱을 설정하는 방법을 자세히 설명합니다.

| 항목 | 설명 |
|-------|-------------|
| [파일에 대한 기본 앱 실행](launch-the-default-app-for-a-file.md) | 파일에 대한 기본 앱을 실행하는 방법을 알아봅니다. |
| [파일 활성화 처리](handle-file-activation.md) | 앱을 특정 파일 형식의 기본 처리기로 등록하는 방법을 알아봅니다. |

아래에서 앱 실행과 관련된 다른 항목을 참조하세요.

| 항목 | 설명 |
|-------|-------------|
| [장치 간 사용자 활동 계속 수행](useractivities.md) | 사용자가 종료한 지점에서 앱을 실행하여 장치 간에 앱을 다시 사용할 수 있습니다. |
| [자동 실행을 사용한 자동 시작](auto-launching-with-autoplay.md) | 자동 실행을 사용하면 사용자가 디바이스를 PC에 연결할 때 앱을 옵션으로 제공할 수 있습니다. 여기에는 카메라 또는 미디어 플레이어 등의 볼륨 이외의 디바이스나 USB 드라이브, SD 카드 또는 DVD 등의 볼륨 디바이스가 포함됩니다. |
| [예약된 파일 및 URI 스키마 이름](reserved-uri-scheme-names.md) | 이 항목에는 앱에 사용할 수 없는 예약된 파일 및 URI 스키마 이름이 나열됩니다. |

## <a name="app-services-and-extensions"></a>앱 서비스 및 확장

[앱 서비스 및 확장](app-services.md) 섹션에서는 앱 간에 데이터와 기능을 공유할 수 있도록 앱 서비스를 UWP 앱에 통합하는 방법을 설명합니다.

| 항목 | 설명 |
|-------|-------------|
| [앱 서비스 만들기 및 사용](how-to-create-and-consume-an-app-service.md) | 다른 UWP 앱에 서비스를 제공할 수 있는 UWP(유니버설 Windows 플랫폼)를 작성하는 방법과 이러한 서비스를 사용하는 방법을 알아봅니다. |
| [앱 서비스가 호스트 앱과 동일한 프로세스에서 실행되도록 변환](convert-app-service-in-process.md) | 별도 백그라운드 프로세스에서 실행된 앱 서비스 코드를 앱 서비스 공급자와 동일한 프로세스 내에서 실행되는 코드로 변환합니다. |
| [앱 서비스, 확장 및 패키지로 앱 확장](extend-your-app-with-services-extensions-packages.md) | 앱을 확장하고 구성 요소화하기 위해 사용할 기술을 결정하고 각각에 대한 개요를 얻습니다. |
| [앱 확장 만들기 및 사용](how-to-create-an-extension.md) | 사용자가 Microsoft Store에서 설치할 수 있는 패키지를 통해 앱을 확장하는 UWP(유니버설 Windows 플랫폼) 앱 확장을 작성하고 호스팅할 수 있습니다. |

## <a name="background-tasks"></a>백그라운드 작업

[백그라운드 작업](support-your-app-with-background-tasks.md) 섹션에서는 트리거에 대한 응답으로 백그라운드에서 경량 코드가 실행되도록 하는 방법을 보여 줍니다.

| 항목 | 설명 |
|-------|-------------|
| [백그라운드 작업 지침](guidelines-for-background-tasks.md)                                       | 앱이 백그라운드 작업 실행을 위한 요구 사항을 충족하는지 확인합니다. |
| [백그라운드 작업에서 센서 및 장치에 액세스](access-sensors-and-devices-from-a-background-task.md)   | [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)를 사용하면 포그라운드 앱이 일시 중단된 경우에도 유니버설 Windows 앱이 백그라운드로 센서와 주변 장치에 액세스할 수 있습니다. |
| [In-process 백그라운드 작업 만들기 및 등록](create-and-register-an-inproc-background-task.md)       | 포그라운드 앱과 같은 프로세스에서 실행되는 백그라운드 작업을 만들고 등록합니다. |
| [Out-of-process 백그라운드 작업 만들기 및 등록](create-and-register-a-background-task.md)           | 앱과 별도의 프로세스로 실행하는 백그라운드 작업을 만들고 등록한 다음, 앱이 포그라운드에 없는 경우 실행되도록 등록합니다. |
| [Out-of-process 백그라운드 작업을 In-process 백그라운드 작업으로 변환](convert-out-of-process-background-task.md) | Out-of-process 백그라운드 작업을 포그라운드 앱과 동일한 프로세스로 실행하는 In-process 백그라운드 작업으로 변환하는 방법에 대해 알아봅니다.|
| [백그라운드 작업 디버그](debug-a-background-task.md)                                                       | Windows 이벤트 로그에서 백그라운드 작업 활성화 및 디버그 추적을 비롯한 백그라운드 작업을 디버그하는 방법을 알아봅니다. |
| [응용 프로그램 매니페스트에서 백그라운드 작업 선언](declare-background-tasks-in-the-application-manifest.md) | 앱 매니페스트에서 백그라운드 작업을 확장으로 선언하여 사용할 수 있습니다. |
| [백그라운드 작업 등록 그룹화](group-background-tasks.md)                                             | 그룹으로 백그라운드 작업 등록 격리 |
| [취소된 백그라운드 작업 처리](handle-a-cancelled-background-task.md)                                 | 영구적 저장소를 통해 앱에 취소를 보고하여 취소 요청을 인식하고 작업을 중지하는 백그라운드 작업을 만드는 방법을 알아봅니다. |
| [백그라운드 작업 진행 및 완료 모니터링](monitor-background-task-progress-and-completion.md)       | 앱에서 백그라운드 작업 진행률 및 완료를 인식하는 방법에 대해 알아봅니다. |
| [백그라운드 작업 최적화](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) |백그라운드에서 사용하는 에너지를 줄이고 백그라운드 작업에 대한 사용자 설정을 조작하는 방법을 알아봅니다. |
| [백그라운드 작업 등록](register-a-background-task.md)                                                 | 대부분의 백그라운드 작업을 안전하게 등록하기 위해 재사용할 수 있는 함수를 만드는 방법을 알아봅니다. |
| [백그라운드 작업으로 시스템 이벤트에 응답](respond-to-system-events-with-background-tasks.md)         | [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839) 이벤트에 응답하는 백그라운드 작업을 만드는 방법을 알아봅니다. |
| [타이머에 따라 백그라운드 작업 실행](run-a-background-task-on-a-timer-.md)                                    | 일회성 백그라운드 작업을 예약하거나 정기적 백그라운드 작업을 실행하는 방법을 알아봅니다. |
| [백그라운드에서 무기한 실행](run-in-the-background-indefinetly.md)                                    | 접근 권한 값을 사용하여 백그라운드 작업 또는 확장된 실행 세션을 백그라운드에서 무기한 실행하세요. |
| [앱 내에서 백그라운드 작업 트리거](trigger-background-task-from-app.md) | [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger)를 사용하여 앱 내에서 백그라운드 작업을 활성화하는 방법을 알아봅니다.|
| [백그라운드 작업 실행 조건 설정](set-conditions-for-running-a-background-task.md)             | 백그라운드 작업이 실행되는 시간을 제어하는 조건을 설정하는 방법에 대해 알아봅니다. |
| [백그라운드에서 데이터 전송](https://msdn.microsoft.com/library/windows/apps/mt280377)                 | 백그라운드 전송 API를 사용하여 백그라운드에서 파일을 복사합니다. |
| [백그라운드 작업에서 라이브 타일 업데이트](update-a-live-tile-from-a-background-task.md)                   | 백그라운드 작업을 사용하여 앱의 라이브 타일을 새 콘텐츠로 업데이트합니다. |
| [유지 관리 트리거 사용](use-a-maintenance-trigger.md)                                                   | 장치가 연결되어 있는 동안 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) 클래스를 사용하여 경량 코드를 실행하는 방법을 알아봅니다. |

## <a name="remote-systems"></a>원격 시스템

[연결된 앱 및 장치(프로젝트 로마)](connected-apps-and-devices.md) 섹션에서는 원격 시스템 플랫폼을 사용하여 원격 장치를 검색하고, 원격 장치에서 앱을 실행하고, 원격 장치의 앱 서비스와 통신하는 방법을 설명합니다.

| 항목 | 설명 |
|-------|-------------|
| [원격 디바이스 검색](discover-remote-devices.md)  | 연결할 수 있는 장치를 검색하는 방법을 알아봅니다. |
| [원격 디바이스에서 앱 시작](launch-a-remote-app.md) | 원격 장치에서 앱을 시작하는 방법을 알아봅니다.  |
| [원격 앱 서비스와 통신](communicate-with-a-remote-app-service.md) | 원격 장치에서 앱을 조작하는 방법을 알아봅니다. |
| [원격 세션을 통해 장치 연결](remote-sessions.md) | 원격 세션에서 여러 장치를 연결하여 공유되는 환경을 만듭니다. |

## <a name="splash-screens"></a>시작 화면

[시작 화면](splash-screens.md) 섹션에서는 앱의 시작 화면을 설정 및 구성하는 방법을 설명합니다.

| 항목 | 설명 |
|-------|-------------|
| [시작 화면 추가](add-a-splash-screen.md) | 앱의 시작 화면 이미지와 배경색을 설정합니다. |
| [시작 화면을 더 오래 표시](create-a-customized-splash-screen.md) | 앱의 연장된 시작 화면을 만들어 시작 화면을 더 오랫동안 표시합니다. 이 연장된 화면은 앱을 시작할 때 표시되는 시작 화면을 모방하며 사용자 지정할 수 있습니다. |
