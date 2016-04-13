---
Description: 사람이 서로 커뮤니케이션할 때 음성과 제스처를 결합해서 사용하는 것처럼, 앱을 조작할 때도 여러 유형 및 모드의 입력이 유용할 수 있습니다.
title: 여러 입력 디자인 지침
ms.assetid: 03EB5388-080F-467C-B272-C92BE00F2C69
label: 여러 입력
template: detail.hbs
---

# 여러 입력


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


사람이 서로 커뮤니케이션할 때 음성과 제스처를 결합해서 사용하는 것처럼, 앱을 조작할 때도 여러 유형 및 모드의 입력이 유용할 수 있습니다.


가능한 한 많은 사용자와 디바이스를 수용하려면 가능한 한 많은 입력 유형(제스처, 음성, 터치, 터치 패드, 마우스 및 키보드)과 작동하도록 앱을 디자인하는 것이 좋습니다. 이렇게 하면 유연성, 유용성 및 접근성이 극대화됩니다.

시작하려면 앱에서 입력을 처리하는 다양한 시나리오를 고려해야 합니다. 앱 전체에서 일관성을 유지하고 플랫폼 컨트롤이 여러 입력 유형에 대한 기본 제공 지원을 제공해야 합니다.

-   사용자가 여러 입력 장치를 통해 응용 프로그램을 조작할 수 있나요?
-   모든 입력 방법이 항상 지원되나요? 특정 컨트롤이 있나요? 특정 시간 또는 상황이 있나요?
-   하나의 입력 방법이 우선적으로 사용되나요?

## <span id="Single__or_exclusive_-mode_interactions_"> </span> <span id="single__or_exclusive_-mode_interactions_"> </span> <span id="SINGLE__OR_EXCLUSIVE_-MODE_INTERACTIONS_"> </span>단일(또는 독점) 모드 조작


단일 모드 조작을 사용하면 여러 입력 형식이 지원되지만 작업당 하나만 사용할 수 있습니다. 예를 들어 명령에는 음성 인식, 탐색에는 제스처, 텍스트 입력에는 근접성에 따라 터치 또는 제스처를 사용합니다.

## <span id="Multimodal_interactions"> </span> <span id="multimodal_interactions"> </span> <span id="MULTIMODAL_INTERACTIONS"> </span>다중 모드 조작


다중 모드 조작에서는 여러 입력 방법이 순차적으로 사용되어 단일 작업을 완료합니다.

<span id="Speech___gesture"> </span> <span id="speech___gesture"> </span> <span id="SPEECH___GESTURE"> </span>음성 + 제스처  
사용자가 제품을 가리킨 다음 "카트에 추가"라고 말합니다.

<span id="Speech___touch"> </span> <span id="speech___touch"> </span> <span id="SPEECH___TOUCH"> </span>음성 + 터치  
사용자가 길게 눌러 사진을 선택한 다음 "사진 보내기"라고 말합니다.





<!--HONumber=Mar16_HO1-->


