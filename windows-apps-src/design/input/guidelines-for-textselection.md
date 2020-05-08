---
Description: 이 항목에서는 텍스트, 이미지 및 컨트롤을 선택 하 고 조작 하는 새로운 Windows UI에 대해 설명 하 고 Windows 앱에서 이러한 새 선택 및 조작 메커니즘을 사용할 때 고려해 야 하는 사용자 환경 지침을 제공 합니다.
title: 텍스트 및 이미지 선택
ms.assetid: d973ffd8-602e-47b5-ab0b-4b2a964ec53d
label: Selecting text and images
template: detail.hbs
keywords: 키보드, 텍스트, 입력, 사용자 상호 작용
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a118a7160842154a656e0f2d29783b1b2e676755
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970098"
---
# <a name="selecting-text-and-images"></a>텍스트 및 이미지 선택


이 문서에서는 텍스트, 이미지 및 컨트롤을 선택 하 고 조작 하는 방법을 설명 하 고 앱에서 이러한 메커니즘을 사용할 때 고려해 야 하는 사용자 환경 지침을 제공 합니다.

> **중요 한 api**: [**windows. .xaml**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input). input, [**windows**](https://docs.microsoft.com/uwp/api/Windows.UI.Input)
 


## <a name="dos-and-donts"></a>실행 사항 및 금지 사항


-   사용자 고유의 그리퍼 UI를 구현할 때 글꼴 문자 모양을 사용 합니다. 그리퍼는 시스템 차원에서 사용할 수 있는 두 맑은 고딕 글꼴의 조합입니다. 글꼴 리소스를 사용 하면 다양 한 dpi에서 렌더링 문제를 간소화 하 고 다양 한 UI 크기 조정 안정화 될 잘 작동 합니다. 사용자 고유의 위치 조정 막대을 구현 하는 경우 다음 UI 특성을 공유 해야 합니다.

    -   원형 도형
    -   모든 배경에 대해 표시
    -   일관된 크기
-   그리퍼 UI를 수용할 수 있도록 선택 가능한 내용 주위에 여백을 제공 합니다. 앱에서 위아래로 이동 하지 않는 영역에서 텍스트 선택을 사용 하도록 설정 하는 경우 다음 그림에 표시 된 것과 같이 텍스트 영역의 왼쪽 및 오른쪽에 1/2 그리퍼 여백을 허용 하 고 텍스트 영역의 위쪽 및 아래쪽에는 1 개의 그리퍼 height를 허용 합니다. 이렇게 하면 전체 그리퍼 UI가 사용자에 게 노출 되 고 다른 가장자리 기반 UI와의 의도 하지 않은 상호 작용을 최소화 합니다.

    ![텍스트 선택 그리퍼 여백](images/textselection-gripper-margins.png)

-   상호 작용 하는 동안 위치 조정 막대 UI를 숨깁니다. 상호 작용 하는 동안 위치 조정 막대의 폐색를 제거 합니다. 이는 위치 조정 기능이 손가락으로 완전히 가려져 있지 않거나 여러 텍스트 선택 위치 조정 막대 경우에 유용 합니다. 이렇게 하면 자식 창을 표시할 때 시각적 아티팩트가 제거 됩니다.

-   컨트롤, 레이블, 이미지, 소유 콘텐츠 등의 UI 요소를 선택할 수 없습니다. 일반적으로 Windows 응용 프로그램에서는 특정 컨트롤 내 에서만 선택할 수 있습니다. 단추, 레이블 및 로고와 같은 컨트롤은 선택할 수 없습니다. 앱에 대 한 선택이 문제 인지 여부를 평가 하 고, 선택 하는 경우 선택이 금지 되어야 하는 UI 영역을 확인 합니다. 

## <a name="additional-usage-guidance"></a>추가 사용법 지침


텍스트 선택 및 조작은 특히 터치 상호 작용으로 인 한 사용자 환경 문제에 취약 합니다. 마우스, 펜/스타일러스 및 키보드 입력은 매우 세분화 되어 있습니다. 마우스 클릭 또는 펜/스타일러스 접촉은 일반적으로 단일 픽셀에 매핑되고 키가 눌러져 있거나 눌러져 있지 않습니다. 터치식 입력은 세부적인 것은 아닙니다. fingertip의 전체 표면을 화면에서 특정 x-y 위치로 매핑하여 텍스트 캐럿을 정확 하 게 배치 하는 것은 어렵습니다.

**고려 사항 및 권장 사항**

Windows의 언어 프레임 워크를 통해 노출 되는 기본 제공 컨트롤을 사용 하 여 선택 및 조작 동작을 비롯 한 전체 플랫폼 사용자 상호 작용 환경을 제공 하는 앱을 빌드합니다. 대부분의 Windows 앱에 대해 기본 제공 컨트롤의 상호 작용 기능을 충분히 찾을 수 있습니다.

표준 Windows 텍스트 컨트롤을 사용 하는 경우이 항목에서 설명 하는 선택 동작 및 시각적 개체를 사용자 지정할 수 없습니다.

**텍스트 선택**

앱이 텍스트 선택을 지 원하는 사용자 지정 UI를 필요로 하는 경우 여기에 설명 된 Windows 선택 동작을 따르는 것이 좋습니다.

**편집 가능 하 고 편집할 수 없는 콘텐츠**


터치를 사용 하 여 선택 영역은 주로 삽입 커서를 설정 하거나 단어를 선택 하는 탭과 같은 제스처를 통해 수행 되며 선택 항목을 수정 하는 슬라이드를 사용 합니다. 다른 Windows touch 상호 작용과 마찬가지로 시간 지정 된 상호 작용은 정보 UI를 표시 하기 위해 누르고 있는 제스처로 제한 됩니다. 자세한 내용은 [시각적 피드백에 대 한 지침](guidelines-for-visualfeedback.md)을 참조 하세요.

Windows에서는 선택 상호 작용에 대 한 두 가지 가능한 상태, 편집 가능 및 편집 불가능 상태를 인식 하 고 그에 따라 선택 UI, 피드백 및 기능을 조정 합니다.

**편집 가능한 콘텐츠**

단어의 왼쪽 절반에서 마우스를 누르면 커서가 바로 왼쪽에 배치 되 고 오른쪽 절반 안쪽을 누르면 커서가 단어의 바로 오른쪽에 배치 됩니다.

다음 이미지에서는 단어의 시작 또는 끝 부분을 탭 하 여 그리퍼로 초기 삽입 커서를 넣는 방법을 보여 줍니다.

![단어의 왼쪽을 탭 하거나 길게 눌러 해당 단어의 시작 부분에 캐럿 및 그리퍼를 추가 합니다. 단어의 오른쪽을 탭 하거나 길게 눌러 해당 단어의 끝에 캐럿과 그리퍼를 넣습니다.](images/textselection-place-caret.png)

다음 이미지에서는 그리퍼를 끌어서 선택 항목을 조정 하는 방법을 보여 줍니다.

![선택 영역을 조정 하려면 어느 방향으로 나 그리퍼를 두 방향으로 끕니다. 첫 번째 그리퍼는 고정 되어 있고 두 번째 그리퍼는 표시 됩니다. 다음으로 조정 하 여 나중에 조정 하십시오.](images/adjust-selection.png)

다음 이미지는 선택 영역 내에서 탭 하거나 위치를 탭 하 여 상황에 맞는 메뉴를 호출 하는 방법을 보여 줍니다 (길게 누르기를 사용할 수도 있음).

![상황에 맞는 메뉴를 호출 하려면 선택 영역 또는 위치 조정 내에서 탭 하거나 길게 누릅니다.](images/textselection-show-context.png)

**이러한 상호**작용은 철자가 잘못 된 단어의 경우 약간 달라 집니다.   철자가 잘못 된 것으로 표시 된 단어를 누르면 단어 전체를 강조 표시 하 고 제안 된 맞춤법 상황에 맞는 메뉴를 호출 합니다.

 

**편집할 수 없는 콘텐츠**

다음 이미지는 단어 내부를 탭 하 여 단어를 선택 하는 방법을 보여 줍니다 (초기 선택에는 공백이 포함 되지 않음).

![단어 내에서 탭 하 여 선택 합니다 (초기 선택에는 공백이 포함 되지 않음).](images/select-word.png)

편집 가능한 텍스트와 동일한 절차에 따라 선택 항목을 조정 하 고 상황에 맞는 메뉴를 표시 합니다.

**개체 조작**

가능 하면 Windows 앱에서 사용자 지정 개체 조작을 구현할 때 텍스트 선택과 동일한 (또는 유사한) 그리퍼 리소스를 사용 합니다. 이를 통해 플랫폼 간에 일관 된 상호 작용 환경을 제공할 수 있습니다.

예를 들어 다음 이미지와 같이 조정 가능한 진행률 표시줄을 제공 하는 크기 조정 및 자르기 또는 미디어 플레이어 앱을 지 원하는 이미지 처리 앱에서 위치 조정 막대을 사용할 수도 있습니다.

![진행률 그리퍼가 있는 미디어 플레이어](images/gripper-mediaplayer.png)

*조정 가능한 진행률 표시줄이 있는 미디어 플레이어입니다.*

![자르기 위치 조정 막대 이미지](images/gripper-imagemanip.png)

*자르기 위치 조정 막대를 사용 하는 이미지 편집기*

## <a name="related-articles"></a>관련된 문서

### <a name="for-developers"></a>개발자용

- [사용자 지정 사용자 조작](https://docs.microsoft.com/windows/uwp/design/layout/index)

### <a name="samples"></a>샘플

- [기본 입력 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [짧은 대기 시간 입력 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [사용자 상호 작용 모드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [포커스 화면 효과 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>보관 샘플

- [Input: XAML 사용자 입력 이벤트 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [입력: 장치 기능 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [입력: 터치 적중 테스트 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [XAML 스크롤, 패닝 및 확대/축소 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [입력: 간소화 된 잉크 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [입력: Windows 8 제스처 샘플](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
- [Input: 조작 및 제스처 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [DirectX touch 입력 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
