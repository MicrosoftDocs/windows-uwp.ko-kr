---
title: .NET TraceProcessing 이란?-.NET TraceProcessing
description: 이 개요에서는 .NET TraceProcessing에 대해 알아봅니다.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: overview
ms.openlocfilehash: 03f88d6bd1ac150d94f73f9f9177249c3b5ae78f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170557"
---
# <a name="process-etw-traces-in-net"></a>.NET의 프로세스 ETW 추적

[ETW (ETW(Windows용 이벤트 추적))](/windows/win32/etw/event-tracing-portal) 는 Windows 운영 체제에 기본 제공 되는 강력한 추적 컬렉션 시스템입니다. Windows는 컨텍스트 전환, 메모리 할당, 프로세스 만들기 및 종료와 같은 이벤트에 대 한 커널까지 시스템 동작에 대 한 데이터를 포함 하 여 ETW와 긴밀 하 게 통합 됩니다. ETW에서 사용할 수 있는 시스템 차원의 데이터를 사용 하면 전체 시스템의 여러 구성 요소 간의 상호 작용을 확인 해야 하는 종단 간 성능 분석과 기타 질문에 적합 합니다.

텍스트 로깅과 달리 ETW는 자동화 된 데이터 처리를 위해 설계 된 구조화 된 이벤트를 제공 합니다. Microsoft는 ETW 추적 파일 (.etl)에 캡처되는 추적 데이터를 시각화 하 고 탐색 하기 위한 그래픽 인터페이스를 제공 하는 [WPA (Windows 성능 분석기](/windows-hardware/test/wpt/windows-performance-analyzer))를 비롯 하 여 이러한 구조화 된 이벤트를 기반으로 강력한 도구를 구축 했습니다.

Microsoft 내에서는 ETW 추적을 사용 하 여 Windows의 새 빌드 성능을 측정 합니다. Windows 엔지니어링 시스템에서 데이터 볼륨을 생성 하는 경우 자동화 된 분석이 필요 합니다. 자동화 된 추적 분석을 위해 c # 및 .NET을 많이 사용 하므로 많은 종류의 ETW 추적 데이터에 액세스 하기 위한 [.Net TraceProcessing API](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All) 를 만들었습니다. 이 기술은 Windows 성능 분석기 내에서 여러 테이블의 기능을 사용 하는 데에도 사용 됩니다.

.NET TraceProcessing NuGet 패키지를 사용 하면 Microsoft에서 Windows를 분석 하는 데 사용 하는 것과 동일한 도구를 사용 하 여 자체 응용 프로그램 및 시스템을 분석할 수 있습니다.

## <a name="next-steps"></a>다음 단계

이 개요에서는 .NET TraceProcessing에 대해 알아보았습니다.

다음 단계는 [첫 번째 추적을 처리](quickstart.md)하는 것입니다.

## <a name="related-topics"></a>관련 항목

* [추적 데이터 액세스](tutorial.md)
* [TraceProcessor 확장](extensibility.md)
* [기호 로드](symbols.md)
* [스트리밍 사용](streaming.md)
* [샘플](https://github.com/microsoft/eventtracing-processing-samples)
* [API 참조](reference.md)