---
author: Karl-Bridge-Microsoft
Description: This topic describes the new Windows UI for selecting and manipulating text, images, and controls and provides user experience guidelines that should be considered when using these new selection and manipulation mechanisms in your UWP app.
title: 텍스트 및 이미지 선택
ms.assetid: d973ffd8-602e-47b5-ab0b-4b2a964ec53d
label: Selecting text and images
template: detail.hbs
keywords: 키보드, 텍스트, 입력, 사용자 조작
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c0bc236fd3e9e37a759f83e3f24bfcad4817f068
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5564371"
---
# <a name="selecting-text-and-images"></a>텍스트 및 이미지 선택


이 문서에서는 텍스트, 이미지 및 컨트롤을 선택하고 조작하는 방법을 설명하고 앱에서 이러한 메커니즘을 사용할 때 고려해야 할 사용자 환경 지침을 제공합니다.

> **중요 API**: [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994), [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)
 


## <a name="dos-and-donts"></a>권장 사항 및 금지 사항


-   고유한 위치 조정 막대 UI를 구현하는 경우 글꼴 문자 모양을 사용합니다. 위치 조정 막대는 시스템 전체에서 사용할 수 있는 두 가지 Segoe UI 글꼴의 조합입니다. 글꼴 리소스를 사용하면 여러 dpi에서 렌더링 문제가 간소화되며 다양한 UI 크기 조정에서도 제대로 작동합니다. 고유한 위치 조정 막대를 구현하는 경우 다음 UI 특성을 공유해야 합니다.

    -   원형 모양
    -   모든 배경에서 표시
    -   일관된 크기
-   위치 조정 막대 UI가 포함되도록 선택 가능한 콘텐츠 주위에 여백을 제공합니다. 앱이 이동/스크롤되지 않는 영역에서 텍스트 선택을 사용하도록 설정할 경우 다음 그림과 같이 텍스트 영역의 왼쪽 및 오른쪽에 1/2 위치 조정 막대 여백과 텍스트 영역의 위쪽 및 아래쪽에 1 위치 조정 막대 높이를 허용합니다. 이렇게 하면 전체 위치 조정 막대 UI가 사용자에게 표시되며 다른 가장자리 기반 UI의 무인 조작이 최소화됩니다.

    ![텍스트 선택 위치 조정 막대 여백](images/textselection-gripper-margins.png)

-   조작 중에 위치 조정 막대 UI를 숨깁니다. 조작 중에 위치 조정 막대에 의한 폐색을 제거합니다. 이 기능은 위치 조정 막대가 손가락으로 완전히 가려지지 않았거나 텍스트 선택 위치 조정 막대가 여러 개 있는 경우에 유용합니다. 따라서 자식 창을 표시할 때 시각적 아티팩트가 제거됩니다.

-   컨트롤, 레이블, 이미지, 소유 콘텐츠 등의 UI 요소를 선택할 수 없게 합니다. 일반적으로 Windows 응용 프로그램은 특정 컨트롤 내에서만 선택을 허용합니다. 단추, 레이블, 로고 등의 컨트롤은 선택할 수 없습니다. 앱에서 선택 작업이 중요한지 평가하고 중요한 경우 선택이 금지되어야 하는 UI 영역을 식별합니다. 

## <a name="additional-usage-guidance"></a>추가 사용법 지침


텍스트 선택 및 조작은 터치 조작으로 인해 발생하는 사용자 환경 문제에 특히 영향을 미칠 수 있습니다. 마우스, 펜/스타일러스 및 키보드 입력은 고도로 세분화되어 있습니다. 마우스 클릭이나 펜/스타일러스 접촉은 일반적으로 단일 픽셀에 매핑되고 키는 누른 상태 또는 누르지 않은 상태가 됩니다. 그렇지만 터치식 입력은 세분화되어 있지 않습니다. 한 손가락의 전체 표면을 화면의 특정 x-y 위치에 매핑시켜 텍스트 캐럿을 정확히 배치하는 것은 어려운 일입니다.

**고려 사항 및 권장 지침**

선택 및 조작 동작을 비롯 한 전체 플랫폼 사용자 조작 환경을 제공 하는 Windowsto 빌드 앱의 언어 프레임 워크를 통해 노출 하는 기본 제공 컨트롤을 사용 합니다. 기본 제공 컨트롤의 조작 기능은 대부분의 UWP 앱에서 충분히 작동합니다.

표준 UWP 텍스트 컨트롤을 사용할 경우에는 이 항목에 설명된 선택 동작 및 시각 효과를 사용자 지정할 수 없습니다.

**텍스트 선택**

앱에서 텍스트 선택을 지 원하는 사용자 지정 UI에 필요한 경우에 여기에 설명 된 Windowsselection 동작을 따르는 것이 좋습니다.

**편집 가능 및 편집 불가능 콘텐츠**


터치를 사용할 경우 선택 조작은 주로 삽입 커서를 설정하거나 단어를 선택하기 위한 탭하기, 선택 영역을 수정하기 위한 밀기 등의 제스처를 통해 수행됩니다. 다른 Windowstouch 상호 작용에서와 마찬가지로 시간이 제한 된 조작은 누르기 제한 되며 정보 UI를 표시 하는 제스처를 보유 합니다. 자세한 내용은 [시각적 피드백에 대한 지침](guidelines-for-visualfeedback.md)을 참조하세요.

두 Windowsrecognizes 가능한 선택 조작에 편집 가능 및 편집 불가능 상태 및 선택 UI, 피드백 및 기능을 적절 하 게 조정 합니다.

**편집 가능 콘텐츠**

단어의 왼쪽 절반에서 탭하면 단어 바로 왼쪽에 커서가 배치되고, 오른쪽 절반에서 탭하면 단어 바로 오른쪽에 커서가 배치됩니다.

다음 이미지는 단어 맨 처음이나 끝 부분을 탭하여 위치 조정 막대가 있는 초기 삽입 커서를 배치하는 방법을 보여 줍니다.

![단어 맨 앞에 캐럿 및 위치 조정 막대를 배치하려면 단어 왼쪽을 탭(또는 길게 누르기)합니다. 단어 맨 끝에 캐럿 및 위치 조정 막대를 배치하려면 단어 오른쪽을 탭(또는 길게 누르기)합니다.](images/textselection-place-caret.png)

다음 이미지는 위치 조정 막대를 끌어 선택을 조정하는 방법을 보여 줍니다.

![위치 조정 막대를 한 방향으로 끌어 선택 영역을 조정합니다(첫 번째 위치 조정 막대는 고정되어 있고 두 번째 위치 조정 막대가 표시됨). 후속 조정을 하려면 두 위치 조정 막대 중 하나를 끕니다.](images/adjust-selection.png)

다음 이미지는 선택 영역 내부나 위치 조정 막대를 탭하여 상황에 맞는 메뉴를 호출하는 방법을 보여 줍니다(길게 누르기를 사용할 수도 있음).

![선택 영역 내부 또는 위치 조정 막대를 탭하여(또는 길게 눌러) 상황에 맞는 메뉴를 호출합니다.](images/textselection-show-context.png)

**참고**철자가 틀린된 단어의 경우 이러한 조작이 다소 합니다. 철자가 틀린 것으로 표시된 단어를 탭하면 전체 단어가 강조 표시되고 제안되는 맞춤법 상황에 맞는 메뉴가 호출됩니다.

 

**편집 불가능 콘텐츠**

다음 이미지는 단어 내부를 탭하여 단어를 선택하는 방법을 보여 줍니다(초기 선택에는 공백이 포함되지 않음).

![단어 내부를 탭하여 선택합니다(초기 선택에는 공백이 포함되지 않음).](images/select-word.png)

편집 가능 텍스트에 대해서도 동일한 절차에 따라 선택을 조정하고 상황에 맞는 메뉴를 표시합니다.

**개체 조작**

가능한 경우 UWP 앱에서 사용자 지정 개체 조작을 구현할 때 텍스트 선택과 동일한(또는 유사한) 위치 조정 막대 리소스를 사용합니다. 이렇게 하면 플랫폼 전체에서 일관된 조작 환경을 제공할 수 있습니다.

예를 들어 크기 조정 및 자르기를 지원하는 이미지 처리 앱이나 조정 가능한 진행률 표시줄을 제공하는 미디어 플레이어 앱에서 위치 조정 막대를 다음 그림과 같이 사용할 수도 있습니다.

![진행률 위치 조정 막대가 있는 미디어 플레이어](images/gripper-mediaplayer.png)

*조정 가능한 진행률 표시줄이 있는 미디어 플레이어입니다.*

![자르기 위치 조정 막대가 있는 이미지](images/gripper-imagemanip.png)

*자르기 위치 조정 막대가 있는 이미지 편집기입니다.*

## <a name="related-articles"></a>관련 문서



**개발자용**
* [사용자 지정 사용자 조작](https://msdn.microsoft.com/library/windows/apps/mt185599)

**샘플**
* [기본 입력 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [짧은 대기 시간 입력 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [사용자 조작 모드 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [포커스 화면 효과 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**보관 샘플**
* [입력: XAML 사용자 입력 이벤트 샘플](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [입력: 디바이스 기능 샘플](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [입력: 터치 적중 횟수 테스트 샘플](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML 스크롤, 이동 및 확대/축소 샘플](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [입력: 간단한 잉크 샘플](http://go.microsoft.com/fwlink/p/?linkid=246570)
* [입력: Windows 8 제스처 샘플](http://go.microsoft.com/fwlink/p/?LinkId=264995)
* [입력: 조작 및 제스처(C++) 샘플](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [DirectX 터치 입력 샘플](http://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 




