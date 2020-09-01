---
ms.assetid: 95CF7F3D-9E3A-40AC-A083-D8A375272181
title: 스레드 풀을 사용하기 위한 모범 사례
description: 스레드 풀을 사용 하 여 병렬 스레드에서 비동기 작업을 수행 하기 위한 모범 사례를 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 스레드, 스레드 풀
ms.localizationpriority: medium
ms.openlocfilehash: 692dcae11c80e817d1d400e66e5f90b65df8b249
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161717"
---
# <a name="best-practices-for-using-the-thread-pool"></a>스레드 풀을 사용하기 위한 모범 사례

이 항목에서는 스레드 풀을 사용 하기 위한 모범 사례에 대해 설명 합니다.

## <a name="dos"></a>Do


-   스레드 풀을 사용 하 여 앱에서 병렬 작업을 수행 합니다.

-   작업 항목을 사용 하 여 UI 스레드를 차단 하지 않고 확장 된 작업을 수행할 수 있습니다.

-   수명이 짧고 독립적인 작업 항목을 만듭니다. 작업 항목은 비동기적으로 실행 되며 큐에서 임의의 순서로 풀에 제출할 수 있습니다.

-   [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher)를 사용 하 여 ui 스레드에 업데이트를 디스패치합니다.

-   **Sleep** 함수 대신 [**windows.system.threading.threadpooltimer**](/uwp/api/windows.system.threading.threadpooltimer.createtimer) 을 사용 합니다.

-   사용자 고유의 스레드 관리 시스템을 만드는 대신 스레드 풀을 사용 합니다. 스레드 풀은 고급 기능을 사용 하 여 OS 수준에서 실행 되며 프로세스 및 시스템 전체에서 장치 리소스 및 작업에 따라 동적으로 크기를 조정할 수 있도록 최적화 되어 있습니다.

-   C + +에서 작업 항목 대리자가 agile 스레딩 모델을 사용 하는지 확인 합니다 (기본적으로 c + + 대리자는 agile).

-   사용할 때 리소스 할당 실패를 허용할 수 없는 경우 미리 할당 된 작업 항목을 사용 합니다.

## <a name="donts"></a>일과


-   *기간* 값이 &lt; 1 밀리초 (0 포함) 인 주기적인 타이머를 만들지 마세요. 이렇게 하면 작업 항목이 단일 샷 타이머로 동작 합니다.

-   *기간* 매개 변수에서 지정한 시간 보다 완료 하는 데 시간이 오래 걸리는 정기 작업 항목을 제출 하지 마세요.

-   백그라운드 작업에서 디스패치 된 작업 항목에서 알림을 및 알림 이외의 UI 업데이트를 보내지 마세요. 대신 백그라운드 작업 진행률 및 완료 처리기 (예: [**IBackgroundTaskInstance**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.progress))를 사용 합니다.

-   **Async** 키워드를 사용 하는 작업 항목 처리기를 사용 하는 경우에는 처리기의 모든 코드가 실행 되기 전에 스레드 풀 작업 항목을 전체 상태로 설정할 수 있습니다. 처리기 내의 **wait** 키워드 다음에 오는 코드는 작업 항목이 완료 상태로 설정 된 후에 실행 될 수 있습니다.

-   미리 할당 된 작업 항목을 다시 초기화 하지 않고 두 번 이상 실행 해서는 안 됩니다. [정기 작업 항목 만들기](create-a-periodic-work-item.md)

## <a name="related-topics"></a>관련 항목


* [정기 작업 항목 만들기](create-a-periodic-work-item.md)
* [스레드 풀에 작업 항목 제출](submit-a-work-item-to-the-thread-pool.md)
* [타이머를 사용하여 작업 항목 제출](use-a-timer-to-submit-a-work-item.md)