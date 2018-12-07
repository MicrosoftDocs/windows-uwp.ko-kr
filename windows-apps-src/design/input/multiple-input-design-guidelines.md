---
Description: Just as people use a combination of voice and gesture when communicating with each other, multiple types and modes of input can also be useful when interacting with an app.
title: 여러 입력 디자인 지침
ms.assetid: 03EB5388-080F-467C-B272-C92BE00F2C69
label: Multiple inputs
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c67430680854e7940d12af15ecd3c07dcd976802
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8789190"
---
# <a name="multiple-inputs"></a>여러 입력


사람이 서로 커뮤니케이션할 때 음성과 제스처를 결합해서 사용하는 것처럼, 앱을 조작할 때도 여러 유형 및 모드의 입력이 유용할 수 있습니다.


가능한 한 많은 사용자와 장치를 수용하려면 가능한 한 많은 입력 유형(제스처, 음성, 터치, 터치 패드, 마우스 및 키보드)과 작동하도록 앱을 디자인하는 것이 좋습니다. 이렇게 하면 유연성, 유용성 및 접근성이 극대화됩니다.

시작하려면 앱에서 입력을 처리하는 다양한 시나리오를 고려해야 합니다. 앱 전체에서 일관성을 유지하고 플랫폼 컨트롤이 여러 입력 유형에 대한 기본 제공 지원을 제공해야 합니다.

-   사용자가 여러 입력 장치를 통해 응용 프로그램을 조작할 수 있나요?
-   모든 입력 방법이 항상 지원되나요? 특정 컨트롤이 있나요? 특정 시간 또는 상황이 있나요?
-   하나의 입력 방법이 우선적으로 사용되나요?

## <a name="single-or-exclusive-mode-interactions"></a>단일(또는 독점) 모드 조작


단일 모드 조작을 사용하면 여러 입력 형식이 지원되지만 작업당 하나만 사용할 수 있습니다. 예를 들어 명령에는 음성 인식, 탐색에는 제스처, 텍스트 입력에는 근접성에 따라 터치 또는 제스터를 사용합니다.

## <a name="multimodal-interactions"></a>다중 모드 조작

다중 모드 조작에서는 여러 입력 방법이 순차적으로 사용되어 단일 작업을 완료합니다.

음성 + 제스처  
사용자가 제품을 가리킨 다음 "카트에 추가"라고 말합니다.

음성 + 터치  
사용자가 길게 눌러 사진을 선택한 다음 "사진 보내기"라고 말합니다.



