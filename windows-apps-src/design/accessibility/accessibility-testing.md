---
Description: Windows 앱에 액세스할 수 있도록 절차를 테스트 합니다.
ms.assetid: 272D9C9E-B179-4F5A-8493-926D007A0225
title: 접근성 테스트
label: Accessibility testing
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1da900732257babc0d53453fa4b9b2c9196e7e6d
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91216416"
---
# <a name="accessibility-testing"></a>접근성 테스트  

Windows 앱에 액세스할 수 있도록 절차를 테스트 합니다.

## <a name="run-accessibility-testing-tools"></a>내게 필요한 옵션 테스트 도구 실행

Windows SDK (소프트웨어 개발 키트)에는 [**Accscope**](/windows/desktop/WinAuto/accscope), [**검사**](/windows/desktop/WinAuto/inspect-objects) 및 [**UI 접근성 검사기**](/windows/desktop/WinAuto/ui-accessibility-checker)와 같은 몇 가지 내게 필요한 옵션 테스트 도구가 포함 되어 있습니다. 이러한 도구는 앱의 접근성을 확인 하는 데 도움이 됩니다. 모든 앱 시나리오와 UI 요소를 확인 해야 합니다.

Microsoft Visual Studio 명령 프롬프트 또는 Windows SDK tools 폴더 (개발 컴퓨터에 Windows SDK가 설치 된 bin 하위 디렉터리)에서 내게 필요한 옵션 테스트 도구를 시작할 수 있습니다.
  
> [!VIDEO https://www.youtube.com/embed/ce0hKQfY9B8]
  
### <a name="accscope"></a>**계정 범위**  

개발자와 테스터는 [**Accscope**](/windows/desktop/WinAuto/accscope) 도구를 사용 하 여 응용 프로그램 개발 및 디자인 중에 응용 프로그램의 개발 및 디자인 중에 응용 프로그램의 액세스 가능성을 평가할 수 있습니다. 앱을 사용 하 여 내레이터 내게 필요한 옵션 시나리오를 테스트 하는 데 특히 적합 합니다.

### <a name="inspect"></a>**검사**  

[**검사**](/windows/desktop/WinAuto/inspect-objects) 를 통해 UI 요소를 선택 하 고 해당 액세스 가능성 데이터를 볼 수 있습니다. UI 자동화 트리에서 Microsoft UI Automation 속성 및 컨트롤 패턴을 보고 자동화 요소의 탐색 구조를 테스트할 수 있습니다. Ui를 개발할 때 **조사** 를 사용 하 여 ui 자동화에서 접근성 특성이 노출 되는 방식을 확인 합니다. 경우에 따라 기본 XAML 컨트롤에 대해 이미 구현 된 UI 자동화 지원에서 특성을 가져올 수 있습니다. 다른 경우에 특성은 XAML 태그에 설정 된 특정 값에서 비롯 되 고 [**Automationproperties**](/uwp/api/windows.ui.xaml.automation.automationproperties) 는 연결 된 속성입니다.

다음 이미지는 메모장의 **편집** 메뉴 요소에 대 한 UI 자동화 속성을 쿼리 하는 [**검사**](/windows/desktop/WinAuto/inspect-objects) 도구를 보여 줍니다.

![검사 도구의 스크린샷](./images/inspect.png)

### <a name="ui-accessibility-checker"></a>UI 액세스 가능성 검사기

**UI 접근성 검사 (accchecker)** 는 런타임에 접근성 문제를 검색 하는 데 도움이 됩니다. UI가 완료 되 고 작동 하면 **Accchecker** 를 사용 하 여 다양 한 시나리오를 테스트 하 고, 런타임 접근성 정보의 정확성을 확인 하 고, 런타임 문제를 검색 합니다. **Accchecker** 는 UI 또는 명령줄 모드에서 실행할 수 있습니다. UI 모드 도구를 실행 하려면 Windows SDK bin 디렉터리에서 **Accchecker** 디렉터리를 열고 acccheckui.exe를 실행 한 다음 **도움말** 메뉴를 클릭 합니다.

### <a name="ui-automation-verify"></a>UI 자동화 확인

Ui **Automation verify (UIA verify)** 는 ui 자동화 구현에 대 한 자동화 된 테스트 및 확인 프레임 워크입니다. **UIA Verify** 는 테스트 코드에 통합 하 고 UI 자동화 시나리오에 대 한 정기적이 고 자동화 된 테스트 또는 별색 검사를 수행할 수 있습니다. **UIA Verify**를 실행 하려면 UIAVerify 하위 디렉터리에서 VisualUIAVerifyNative.exe를 실행 합니다.

### <a name="accessible-event-watcher"></a>액세스 가능한 이벤트 감시자

[**액세스 가능한 이벤트 감시자 (AccEvent)**](/windows/desktop/WinAuto/accessible-event-watcher) 는 ui 변경이 발생할 때 응용 프로그램의 ui 요소가 적절 한 ui 자동화 및 Microsoft Active Accessibility 이벤트를 발생 시키는 지 여부를 테스트 합니다. UI의 변경 내용이 포커스를 변경 하거나, UI 요소를 호출 하거나, 선택 하거나, 상태 또는 속성이 변경 될 때 발생할 수 있습니다.

> [!NOTE]
> 설명서에 언급 된 대부분의 내게 필요한 옵션 테스트 도구는 휴대폰이 아닌 PC에서 실행 됩니다. 에뮬레이터를 개발 하 고 사용 하는 동안 일부 도구를 실행할 수 있지만 이러한 도구의 대부분은 에뮬레이터 내에서 UI 자동화 트리를 노출할 수 없습니다.

## <a name="test-keyboard-accessibility"></a>키보드 접근성 테스트

키보드 접근성을 테스트 하는 가장 좋은 방법은 마우스를 분리 하거나 태블릿 장치를 사용 하는 경우 화상 키보드를 사용 하는 것입니다. _Tab_ 키를 사용 하 여 키보드 접근성 탐색을 테스트 합니다. _Tab_ 키를 사용 하 여 모든 대화형 UI 요소를 순환할 수 있습니다. 복합 UI 요소의 경우 화살표 키를 사용 하 여 요소 부분 사이를 탐색할 수 있는지 확인 합니다. 예를 들어 키보드 키를 사용 하 여 항목 목록을 탐색할 수 있습니다. 마지막으로, 일반적으로 Enter 또는 스페이스바 키를 사용 하 여 해당 요소에 포커스가 있을 때 키보드를 사용 하 여 모든 대화형 UI 요소를 호출할 수 있는지 확인 합니다.

## <a name="verify-the-contrast-ratio-of-visible-text"></a>표시 되는 텍스트의 대비 비율 확인

색 대비 도구를 사용 하 여 표시 되는 텍스트 대비 비율이 허용 되는지 확인 합니다. 예외에는 비활성 UI 요소, 정보를 전달 하지 않고 의미를 변경 하지 않고 다시 정렬할 수 있는 로고 또는 장식 텍스트가 포함 됩니다. 대조 비율 및 예외에 대 한 자세한 내용은 [내게 필요한 옵션 지원 텍스트](accessible-text-requirements.md) 를 참조 하세요. 대비 비율을 테스트할 수 있는 도구는 [WCAG 2.0 G18 (리소스 섹션) 기술](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources) 을 참조 하세요.

> [!NOTE]
> WCAG 2.0 G18에 대 한 기술 별로 나열 된 도구 중 일부는 UWP 앱에서 대화형으로 사용할 수 없습니다. 도구에서 전경 및 배경색 값을 수동으로 입력 하 고, 앱 UI의 화면 캡처를 만든 다음, 화면 캡처 이미지에서 대비 비율 도구를 실행 하거나, 앱에서 이미지를 로드 하는 대신 이미지 편집 프로그램에서 소스 비트맵 파일을 여는 동안 도구를 실행 해야 할 수 있습니다.

## <a name="verify-your-app-in-high-contrast"></a>고대비에서 앱 확인

고대비 테마가 활성화 된 상태에서 앱을 사용 하 여 모든 UI 요소가 올바르게 표시 되는지 확인 합니다. 모든 텍스트를 읽을 수 있어야 하며 모든 이미지를 지워야 합니다. 컨트롤에서 제공 하는 테마 문제를 수정 하려면 XAML 테마 사전 리소스 또는 컨트롤 템플릿을 조정 합니다. 테마 또는 컨트롤 (예: 이미지 파일)에서 중요 한 고대비 문제가 발생 하지 않는 경우 고대비 테마가 활성화 되어 있을 때 사용할 별도의 버전을 제공 합니다.

## <a name="verify-your-app-with-display-settings"></a>표시 설정으로 앱 확인  

디스플레이의 dpi (인치당 도트 수) 값을 조정 하는 시스템 표시 옵션을 사용 하 고 dpi 값이 변경 될 때 앱 UI가 올바르게 확장 되도록 합니다. 일부 사용자는 dpi 값을 내게 필요한 옵션으로 변경 하 고 속성을 표시 하는 것 뿐만 아니라 **쉽게 액세스할** 수 있습니다. 문제가 발견 되 면 [레이아웃 크기 조정에 대 한 지침](https://developer.microsoft.com/windows/apps/design) 에 따라 다양 한 크기 조정 요소에 대 한 추가 리소스를 제공 합니다.

## <a name="verify-main-app-scenarios-by-using-narrator"></a>내레이터를 사용 하 여 주요 앱 시나리오 확인

내레이터를 사용 하 여 앱에 대 한 화면 읽기 환경을 테스트 합니다.

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode/player]

**마우스 및 키보드를 사용 하 여 내레이터를 사용 하 여 앱을 테스트 하려면 다음 단계를 따르세요.**

1. _Windows 로고 키 + Ctrl + Enter_를 눌러 내레이터를 시작 합니다. Windows 10 버전 1607 이전 버전에서 _windows 로고 키 + Enter_ 를 사용 하 여 내레이터를 시작 합니다.
2. _Tab_ 키, 화살표 키, _Caps lock + 화살표 키_를 사용 하 여 키보드로 앱을 탐색 합니다.
3. 앱을 탐색할 때 내레이터가 UI의 요소를 읽고 다음을 확인 하는 것으로 수신 합니다.
    - 각 컨트롤에 대해 내레이터가 표시 되는 모든 콘텐츠를 읽도록 합니다. 또한 내레이터가 각 컨트롤의 이름, 적용 가능한 상태 (선택 됨, 선택 됨 등), 컨트롤 형식 (단추, 확인란, 목록 항목 등)을 읽도록 합니다.
    - 요소가 interactive 인 경우 _Caps lock + Enter_를 눌러 해당 작업을 호출 하는 데 내레이터를 사용할 수 있는지 확인 합니다.
    - 각 테이블에 대해 내레이터가 테이블 이름, 테이블 설명 (있는 경우) 및 행 머리글과 열 머리글을 올바르게 읽도록 합니다.
4. _Caps lock + Shift + enter_ 를 눌러 앱을 검색 하 고 모든 컨트롤이 검색 목록에 표시 되 고 컨트롤 이름이 지역화 되 고 읽을 수 있는지 확인 합니다.
5. 모니터를 끄고 키보드와 내레이터만 사용 하 여 기본 앱 시나리오를 수행 해 보세요. 내레이터 명령 및 바로 가기의 전체 목록을 가져오려면 _Caps lock + F1_키를 누릅니다.

Windows 10 버전 1607부터 내레이터에서 새로운 개발자 모드를 도입 했습니다. _컨트롤 + Caps lock + F12_키를 눌러 내레이터가 이미 실행 중인 경우 개발자 모드를 설정 합니다. 개발자 모드를 사용 하는 경우 화면이 마스킹 되 고 액세스 가능한 개체와 내레이터에 프로그래밍 방식으로 노출 되는 관련 텍스트만 강조 표시 됩니다. 이를 통해 내레이터에 노출 되는 정보를 시각적으로 볼 수 있습니다.

**다음 단계를 사용 하 여 내레이터의 터치 모드를 사용 하 여 앱을 테스트 합니다.**

> [!NOTE]
> 내레이터가 4 + 연락처를 지 원하는 장치에서 자동으로 터치 모드로 전환 됩니다. 내레이터는 기본 화면에서 다중 모니터 시나리오 또는 멀티 터치 디지타이저를 지원 하지 않습니다.

1. UI에 대해 알아보고 레이아웃을 탐색 하세요.
    - **단일 손가락 살짝 밀기 제스처를 사용 하 여 UI를 탐색 합니다.** 왼쪽 또는 오른쪽 swipes를 사용 하 여 항목 사이를 이동 하 고 swipes를 위아래로 이동 하 여 탐색 중인 항목의 범주를 변경 합니다. 범주에는 모든 항목, 링크, 테이블, 헤더 등이 포함 됩니다. 단일 손가락 살짝 밀기 제스처를 사용 하 여 탐색 하는 것은 _Caps lock + 화살표로_이동 하는 것과 비슷합니다.
    - **탭 제스처를 사용 하 여 포커스를 받을 수 있는 요소를 탐색할 수 있습니다.** 오른쪽 또는 왼쪽으로 3 손가락으로 살짝 밀기는 키보드의 _Tab_ _키와 Shift + tab_ 을 사용 하 여 탐색 하는 것과 같습니다.
    - **단일 손가락으로 UI를 공간적으로 조사 합니다.** 한 손가락을 위아래로 끌어 놓거나 왼쪽 및 오른쪽으로 끌어 내레이터가 손가락 아래 항목을 읽도록 합니다. 마우스는 단일 손가락을 끄는 것과 동일한 적중 테스트 논리를 사용 하기 때문에 다른 방법으로 사용할 수 있습니다.
    - **3 개의 손가락으로 살짝 밀어 전체 창 및 모든 해당 콘텐츠를 읽습니다**. 이는 _Caps lock + W_를 사용 하는 것과 같습니다.

    연결할 수 없는 중요 한 UI가 있는 경우 접근성 문제가 있을 수 있습니다.

2. 컨트롤과 상호 작용 하 여 기본 및 보조 작업과 스크롤 동작을 테스트 합니다.

    기본 작업에는 단추 활성화, 텍스트 캐럿 배치, 컨트롤로 포커스 설정 등이 포함 됩니다. 보조 작업에는 목록 항목을 선택 하거나 여러 옵션을 제공 하는 단추를 확장 하는 등의 작업이 포함 됩니다.

    - 기본 작업을 테스트 하려면 두 번 탭 하거나 한 손가락으로 누르고 다른 손가락을 탭 합니다.
    - 보조 작업을 테스트 하려면: 삼중 누르기를 사용 하거나 한 손가락으로 누르고 다른 손가락을 두 번 누릅니다.
    - 스크롤 동작을 테스트 하려면: 두 손가락 swipes를 사용 하 여 원하는 방향으로 스크롤합니다.

    일부 컨트롤은 추가 작업을 제공 합니다. 전체 목록을 표시 하려면 단일 네 손가락 탭을 입력 합니다.

    컨트롤이 마우스나 키보드에 응답 하지만 기본 또는 보조 터치 상호 작용에 응답 하지 않는 경우 컨트롤에서 추가 [UI 자동화](/windows/desktop/WinAuto/entry-uiauto-win32) 컨트롤 패턴을 구현 해야 할 수 있습니다.

또한 사용자의 앱을 사용 하 여 내레이터 내게 필요한 옵션 시나리오를 테스트 하려면 [**Accscope**](/windows/desktop/WinAuto/accscope) 도구를 사용 하는 것이 좋습니다. [**Accscope 도구 항목**](/windows/desktop/WinAuto/accscope) 에서는 내레이터 시나리오를 테스트 하기 위해 **accscope** 를 구성 하는 방법을 설명 합니다.

## <a name="examine-the-ui-automation-representation-for-your-app"></a>앱에 대 한 UI 자동화 표현 검사

앞에서 언급한 다수의 UI 자동화 테스트 도구를 사용하면 의도적으로 앱의 모양을 고려하지 않고 대신 앱을 UI 자동화 요소 구조로 표현하는 방법으로 앱을 볼 수 있습니다. 이는 UI 자동화 클라이언트 (주로 보조 기술)가 접근성 시나리오에서 앱과 상호 작용 하는 방법입니다.

[**Accscope**](/windows/desktop/WinAuto/accscope) 도구는 시각적 표현 또는 목록으로 UI 자동화 요소를 볼 수 있기 때문에 앱의 특히 흥미로운 뷰를 제공 합니다. 시각화를 사용 하는 경우 앱 UI의 시각적 모양에 상관 관계를 지정할 수 있는 방법으로 파트를 드릴 다운할 수 있습니다. UI에 모든 논리를 할당 하기 전에 가장 이른 UI 프로토타입에 대 한 액세스 가능성을 테스트 하 여 앱에 대 한 시각적 상호 작용 및 내게 필요한 옵션-시나리오 탐색이 균형을 유지 하는지 확인할 수도 있습니다.

테스트할 수 있는 한 가지 측면은 UI 자동화 요소 뷰에 표시 하지 않으려는 요소가 있는지 여부입니다. 뷰에서 생략 하려는 요소가 있거나 누락 된 요소가 있는 경우에는 [**AccessibilityView**](/uwp/api/windows.ui.xaml.automation.automationproperties.accessibilityviewproperty) xaml 연결 된 속성을 사용 하 여 접근성 뷰에 XAML 컨트롤이 표시 되는 방법을 조정할 수 있습니다. 기본 액세스 가능성 뷰를 확인 한 후에는 사용자가 대화형이 고 컨트롤 뷰에 노출 되는 각 파트에 도달할 수 있는지 확인 하기 위해 화살표 키로 설정 된 대로 탭 시퀀스 또는 공간 탐색을 다시 확인할 수 있습니다.

## <a name="related-topics"></a>관련 항목

- [접근성](accessibility.md)
- [예방 방법](practices-to-avoid.md)
- [UI 자동화](/windows/desktop/WinAuto/entry-uiauto-win32)
- [Windows의 내게 필요한 옵션](https://www.microsoft.com/accessibility/)
- [내레이터 시작](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator)
