---
title: 그래픽 진단 도구
description: Visual Studio의 그래픽 디버깅, 그래픽 프레임 분석 및 GPU 사용을 비롯하여 그래픽 진단 기능을 가져오고 사용하는 방법에 대해 알아봅니다.
ms.assetid: 629ea462-18ed-a333-07e9-cc87ea2dcd93
ms.date: 11/20/2019
ms.topic: article
keywords: windows 10, uwp, 게임, 그래픽, 진단, 도구, directx
ms.localizationpriority: medium
ms.openlocfilehash: 14711a356f00ac027bf990554c90547017b9cc4d
ms.sourcegitcommit: f464e5157ca22cfe31075f2f1219b8227587bf03
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74263094"
---
# <a name="graphics-diagnostics-tools"></a>그래픽 진단 도구

그래픽 진단 도구는 Windows 10에서 선택적 기능으로 사용할 수 있습니다. 런타임 및 Visual Studio에서 제공 하는 그래픽 진단 기능을 사용 하 여 DirectX 앱 또는 게임을 개발 하려면 선택적 그래픽 도구 기능을 설치 합니다.

1. **설정** > **앱** > **앱 & 기능/옵션 기능**으로 이동 합니다.
2. **설치 된 기능**아래에 **그래픽 도구가** 이미 나열 되어 있으면 작업이 완료 된 것입니다. 그렇지 않으면 **기능 추가**를 클릭 합니다.
3. **그래픽 도구**를 검색 하 고 선택 하 고 **설치**를 클릭 합니다.

그래픽 진단 기능에는 DirectX 런타임의 Direct3D 디버그 장치(Direct3D SDK 계층을 통한) 생성 기능 외에도 그래픽 디버깅, 프레임 분석 및 GPU 사용이 포함됩니다.

-   그래픽 디버깅을 이용하면 앱에서 수행한 Direct3D 호출을 추적할 수 있습니다. 그런 다음 해당 호출을 재생하고, 매개 변수를 검사하고, 셰이더를 디버그 및 실험해 보고, 그래픽 자산을 시각화하여 렌더링 문제를 진단합니다. Windows Pc, 시뮬레이터 또는 장치에서 로그를 가져온 다음 다른 하드웨어에서 다시 재생할 수 있습니다.
-   Visual Studio의 그래픽 프레임 분석는 그래픽 디버깅 로그에서 실행 되며 Direct3D 그리기 호출에 대 한 기준선 타이밍을 수집 합니다. 그런 다음 다양 한 그래픽 설정을 수정 하 여 일련의 실험을 수행 하 고 타이밍 결과 테이블을 생성 합니다. 이 데이터를 사용하여 앱에서 그래픽 성능 문제를 파악하고 다양한 실험 결과를 검토하여 성능 향상을 위한 기회를 식별할 수 있습니다.
-   Visual Studio의 GPU 사용을 통해 실시간으로 GPU 사용을 모니터링 수 있습니다. CPU 및 GPU에 의해 처리 되는 작업의 타이밍 데이터를 수집 하 고 분석 하므로 병목 현상이 발생 한 위치를 확인할 수 있습니다.

## <a name="related-topics"></a>관련 항목

[Visual Studio의 그래픽 진단 개요](/visualstudio/debugger/overview-of-visual-studio-graphics-diagnostics?view=vs-2015)