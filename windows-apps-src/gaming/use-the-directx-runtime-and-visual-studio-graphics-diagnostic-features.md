---
author: mtoepke
title: 그래픽 진단 도구
description: Visual Studio의 그래픽 디버깅, 그래픽 프레임 분석 및 GPU 사용을 비롯하여 그래픽 진단 기능을 가져오고 사용하는 방법에 대해 알아봅니다.
ms.assetid: 629ea462-18ed-a333-07e9-cc87ea2dcd93
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 그래픽, 진단, 도구, directx
ms.localizationpriority: medium
ms.openlocfilehash: aa1c14d15a966f23b86753cf8e5e62e067d10310
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6980868"
---
# <a name="graphics-diagnostics-tools"></a>그래픽 진단 도구



Windows10, 그래픽 진단 도구 된 이제 Windows에서 사용할 수 있는 선택적 기능으로 합니다. 런타임 및 Visual Studio에서 제공하는 그래픽 진단 기능을 사용하여 DirectX 앱 또는 게임을 개발하려면 다음과 같이 선택적 그래픽 도구 기능을 설치합니다.

1.  **설정**으로 이동한 **앱**선택 **선택적 기능 관리를**차례로 클릭 합니다.
2.  **기능 추가**를 클릭합니다.   
3.  **선택적 기능** 목록에서 **그래픽 도구**를 선택한 다음 **설치**를 클릭합니다.

그래픽 진단 기능에는 DirectX 런타임의 Direct3D 디버그 장치(Direct3D SDK 계층을 통한) 생성 기능 외에도 그래픽 디버깅, 프레임 분석 및 GPU 사용이 포함됩니다.

-   그래픽 디버깅을 이용하면 앱에서 수행한 Direct3D 호출을 추적할 수 있습니다. 그런 다음 해당 호출을 재생하고, 매개 변수를 검사하고, 셰이더를 디버그 및 실험해 보고, 그래픽 자산을 시각화하여 렌더링 문제를 진단합니다. Windows PC, 시뮬레이터 또는 장치에서 로그를 생성하고 다른 하드웨어에서 재생할 수 있습니다.
-   Visual Studio의 그래픽 프레임 분석은 그래픽 디버깅 로그에서 실행되고 Direct3D 그리기 호출에 대한 기준 타이밍을 수집합니다. 그런 다음 다양한 그래픽 설정을 수정하여 실험 집합을 수행하고 타이밍 결과 테이블을 생성합니다. 이 데이터를 사용하여 앱에서 그래픽 성능 문제를 파악하고 다양한 실험 결과를 검토하여 성능 향상을 위한 기회를 식별할 수 있습니다.
-   Visual Studio의 GPU 사용을 통해 실시간으로 GPU 사용을 모니터링 수 있습니다. CPU 및 GPU에서 처리하는 작업의 타이밍 데이터를 수집하고 분석하여 병목이 있는 위치를 확인할 수 있습니다.

## <a name="related-topics"></a>관련 항목


[Visual Studio의 그래픽 진단 개요](http://go.microsoft.com/fwlink/p/?LinkID=526382)

 

 




