---
author: normesta
ms.assetid: 95CF7F3D-9E3A-40AC-A083-D8A375272181
title: 스레드 풀을 사용하기 위한 모범 사례
description: 이 항목에서는 스레드 풀 작업을 위한 모범 사례에 대해 설명합니다.
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 스레드, 스레드 풀
ms.localizationpriority: medium
ms.openlocfilehash: ff607e3b39ea9d9a3731cc1f231fe1eb27b0b155
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6283218"
---
# <a name="best-practices-for-using-the-thread-pool"></a>스레드 풀을 사용하기 위한 모범 사례

이 항목에서는 스레드 풀 작업을 위한 모범 사례에 대해 설명합니다.

## <a name="dos"></a>권장 사항


-   스레드 풀을 사용하여 앱에서 병렬 작업을 수행합니다.

-   작업 항목을 사용하여 UI 스레드를 차단하지 않고 확장된 작업을 수행합니다.

-   오래가지 않고 독립적인 작업 항목을 만듭니다. 작업 항목은 비동기적으로 실행되며 순서에 관계없이 큐에서 풀로 제출될 수 있습니다.

-   [**Windows.UI.Core.CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/BR208211)를 사용하여 업데이트를 UI 스레드로 디스패치합니다.

-   **Sleep** 함수 대신 [**ThreadPoolTimer.CreateTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967921)를 사용합니다.

-   고유한 스레드 관리 시스템을 만드는 대신 스레드 풀을 사용합니다. 스레드 풀은 고급 접근 권한 값을 사용하여 OS 수준에서 실행되며 디바이스 리소스 및 활동에 따라 시스템 전체와 프로세스 내에서 동적으로 조정되도록 최적화되어 있습니다.

-   C++에서 작업 항목 대리자가 Agile 스레딩 모델을 사용하는지 확인합니다(C++ 대리자는 기본적으로 Agile임).

-   사용 시 리소스 할당 오류를 허용할 수 없는 경우 미리 할당된 작업 항목을 사용합니다.

## <a name="donts"></a>금지 사항


-   *period* 값이 &lt;1밀리초보다 작은(0 포함) 주기적 타이머를 만들지 마세요. 이렇게 하면 작업 항목이 일회성 타이머로 동작합니다.

-   *period* 매개 변수에 지정한 시간보다 완료하는 데 더 오래 걸리는 주기적 작업 항목을 제출하지 마세요.

-   백그라운드 작업에서 디스패치된 작업 항목에서 알림 이외의 UI 업데이트를 보내지 마세요. 대신 백그라운드 작업 진행률 및 완료 처리기를 사용합니다(예: [**IBackgroundTaskInstance.Progress**](https://msdn.microsoft.com/library/windows/apps/BR224800)).

-   **async** 키워드를 사용하는 작업 항목 처리기를 사용하는 경우에는, 처리기의 모든 코드가 실행되기 전에 스레드 풀 작업 항목이 완료 상태가 될 수 있습니다. 처리기 내에서 **await** 키워드 뒤에 오는 코드는 작업 항목이 완료 상태로 설정된 후에 실행될 수 있습니다.

-   미리 할당된 작업 항목을 다시 초기화하지 않고 여러 번 실행하지 마세요. [정기 작업 항목 만들기](create-a-periodic-work-item.md)

## <a name="related-topics"></a>관련 항목


* [정기 작업 항목 만들기](create-a-periodic-work-item.md)
* [스레드 풀에 작업 항목 제출](submit-a-work-item-to-the-thread-pool.md)
* [타이머를 사용하여 작업 항목 제출](use-a-timer-to-submit-a-work-item.md)
