---
author: TylerMSFT
ms.assetid: beac6333-655a-4bcf-9caf-bba15f715ea5
title: 스레딩 및 비동기 프로그래밍
description: 스레드 및 비동기 프로그래밍을 사용하면 앱이 병렬 스레드에서 비동기식으로 작업할 수 있습니다.
ms.author: twhitney
ms.date: 05/14/2018
ms.topic: article
keywords: windows 10, uwp, 비동기, 스레드, 스레딩
ms.localizationpriority: medium
ms.openlocfilehash: f01142695b676ebadea2f227cf5f8beb65ba6f9c
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5598653"
---
# <a name="threading-and-async-programming"></a>스레딩 및 비동기 프로그래밍
스레드 및 비동기 프로그래밍을 사용하면 앱이 병렬 스레드에서 비동기식으로 작업할 수 있습니다.

앱은 스레드 풀을 사용하여 병렬 스레드에서 비동기로 작업할 수 있습니다. 스레드 풀은 스레드 집합을 관리하며, 사용 가능한 경우 큐를 사용하여 작업 항목을 스레드에 할당합니다. 스레드 풀은 UI를 차단하지 않고 확장된 작업을 하는 데 사용될 수 있기 때문에 Windows 런타임에서 사용 가능한 비동기 프로그래밍 패턴과 유사하지만 스레드 풀이 비동기 프로그래밍 패턴보다 더 강력한 제어 기능을 제공하며 여러 작업 항목을 병렬로 완료하는 데 사용될 수 있습니다. 스레드 풀을 사용하여 다음 작업을 할 수 있습니다.

-   작업 항목을 제출하고, 우선 순위를 제어하고, 작업 항목을 취소합니다.

-   타이머와 주기적 타이머를 사용하여 작업 항목을 예약합니다.

-   중요 작업 항목을 위해 리소스를 따로 설정합니다.

-   명명된 이벤트와 세마포에 대한 응답으로 작업 항목을 실행합니다.

스레드 풀을 사용할 경우 스레드를 만들고 배포하는 오버헤드가 감소하기 때문에 스레드를 관리하는 것보다 더 효율적입니다. 즉, 여러 CPU 코어에서 스레드를 최적화할 수 있으며 백그라운드 작업이 실행 중인 경우 앱 간에 스레드 리소스의 균형을 조정할 수 있습니다. 기본 제공 스레드 풀을 사용할 경우 스레드 관리 체제 대신 작업을 수행하는 코드 작성에 주력할 수 있기 때문에 편리합니다.

| 항목                                                                                                          | 설명                         |
|----------------------------------------------------------------------------------------------------------------|-------------------------------------|
| [비동기 프로그래밍(UWP 앱)](asynchronous-programming-universal-windows-platform-apps.md)              | 유니버설 Windows 플랫폼 (UWP)의 비동기 프로그래밍 및 C#, Microsoft Visual Basic.NET VisualC + + 구성 요소 확장에서의 표현에 설명 (C + + CX), 및 JavaScript 합니다. |
| [C++/CX의 비동기 프로그래밍(UWP 앱)](asynchronous-programming-in-cpp-universal-windows-platform-apps.md)| 이 문서에서는 ppltasks.h의 <code>concurrency</code> 네임스페이스에 정의된 <code>task</code> 클래스를 사용하여 C++/CX의 비동기 메서드를 이용하는 권장 방법을 설명합니다. |
| [스레드 풀을 사용하기 위한 모범 사례](best-practices-for-using-the-thread-pool.md)                         | 이 항목에서는 스레드 풀 작업을 위한 모범 사례에 대해 설명합니다. |
| [C# 또는 Visual Basic에서 비동기식 API 호출](call-asynchronous-apis-in-csharp-or-visual-basic.md)             | UWP(유니버설 Windows 플랫폼)에는 여러 비동기 API가 포함되어 있으므로 앱이 장시간 작업을 수행하는 동안에도 응답 가능한 상태를 유지할 수 있습니다. 이 항목에서는 C# 또는 Microsoft Visual Basic에서 UWP의 비동기 메서드를 사용하는 방법을 설명합니다. |
| [정기 작업 항목 만들기](create-a-periodic-work-item.md)                                                   | 주기적으로 반복되는 작업 항목을 만드는 방법을 알아봅니다. |
| [스레드 풀에 작업 항목 제출](submit-a-work-item-to-the-thread-pool.md)                               | 스레드 풀에 작업 항목을 제출하여 별도 스레드에서 작업하는 방법을 알아보세요. |
| [타이머를 사용하여 작업 항목 제출](use-a-timer-to-submit-a-work-item.md)                                       | 타이머가 경과된 후 실행되는 작업 항목을 만드는 방법을 알아봅니다. |
