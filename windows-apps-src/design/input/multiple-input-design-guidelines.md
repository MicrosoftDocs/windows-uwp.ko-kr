---
description: 사용자가 서로 통신할 때 음성 및 제스처의 조합을 사용 하는 것 처럼, 앱과 상호 작용 하는 경우 입력의 여러 유형과 모드가 유용할 수도 있습니다.
title: 여러 입력 디자인 지침
ms.assetid: 03EB5388-080F-467C-B272-C92BE00F2C69
label: Multiple inputs
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dba9ff049355a7319c6e653b3ecbe5524547b8b1
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034676"
---
# <a name="multiple-inputs"></a>여러 입력


사용자가 서로 통신할 때 음성 및 제스처의 조합을 사용 하는 것 처럼, 앱과 상호 작용 하는 경우 입력의 여러 유형과 모드가 유용할 수도 있습니다.


가능한 한 많은 사용자 및 장치를 수용 하려면 가능한 많은 입력 유형 (제스처, 음성, 터치, 터치 패드, 마우스 및 키보드)으로 작동 하도록 앱을 설계 하는 것이 좋습니다. 이렇게 하면 유연성, 유용성 및 접근성이 극대화 됩니다.

시작 하려면 앱에서 입력을 처리 하는 다양 한 시나리오를 고려 하세요. 응용 프로그램 전체에서 일관성을 유지 하 고, 플랫폼 컨트롤이 여러 입력 형식에 대 한 기본 제공 지원을 제공 한다는 점에 주의 하세요.

-   사용자가 여러 입력 장치를 통해 응용 프로그램과 상호 작용할 수 있나요?
-   모든 입력 방법이 항상 지원 되나요? 특정 컨트롤을 사용 하는 경우 특정 시간 또는 상황에서
-   하나의 입력 방법이 우선적으로 사용되나요?

## <a name="single-or-exclusive-mode-interactions"></a>단일 (또는 단독) 모드 상호 작용


단일 모드 상호 작용을 사용 하는 경우 여러 입력 형식이 지원 되지만 작업 당 하나만 사용할 수 있습니다. 예: 명령에 대 한 음성 인식과 탐색을 위한 제스처 또는 근접성에 따라 터치 또는 제스처를 사용 하는 텍스트 항목입니다.

## <a name="multimodal-interactions"></a>다중 모달 상호 작용

다중 모달 상호 작용을 사용 하면 시퀀스의 여러 입력 메서드를 사용 하 여 단일 작업을 완료 합니다.

음성 + 제스처  
사용자가 제품을 가리키고 "카트에 추가" 라고 표시 됩니다.

음성 + 터치  
사용자가 길게 누르기를 사용 하 여 사진을 선택 하 고 "사진 보내기" 라고 표시 합니다.



