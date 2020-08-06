---
Description: 사용자 지정 IME (입력기)를 개발 하 여 표준 QWERTY 키보드에서 쉽게 나타낼 수 없는 언어로 사용자 입력 텍스트를 지원할 수 있습니다.
title: IME (입력기) 요구 사항
label: Input Method Editor (IME) requirements
template: detail.hbs
keywords: ime, 입력 방법 편집기, 입력, 상호 작용
ms.date: 07/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ecf150973defb0a431fc7248181ddf648576ac77
ms.sourcegitcommit: 86ce67a03e87fa1282849b2fcb4f89d1cf23a091
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/05/2020
ms.locfileid: "87840081"
---
# <a name="custom-input-method-editor-ime-requirements"></a>사용자 지정 IME (입력기) 요구 사항

이러한 지침 및 요구 사항은 표준 QWERTY 키보드에서 쉽게 나타낼 수 없는 언어로 사용자 입력 텍스트를 지원 하기 위해 사용자 지정 IME (입력기)를 개발 하는 데 도움이 될 수 있습니다.

Ime의 개요는 [ime (Input Method Editor)](input-method-editors.md)를 참조 하세요.

## <a name="default-ime"></a>기본 IME

사용자는 원하는 언어에 대 한 기본 IME (**설정-> 시간 & 언어 > 언어 > 기본 언어-> 언어 팩**)를 선택할 수 있습니다.

:::image type="content" source="images/IMEs/ime-preferred-languages.png" alt-text="기본 설정 언어 설정":::

기본 설정 언어에 대 한 언어 옵션 설정 화면에서 기본 키보드를 선택 합니다.

:::image type="content" source="images/IMEs/ime-preferred-languages-keyboard.png" alt-text="기본 설정 언어 키보드":::

> [!Important]
> 사용자 지정 IME에 대 한 기본 키보드를 설정 하기 위해 레지스트리에 직접 쓰지 않는 것이 좋습니다.

## <a name="compatibility-requirements"></a>호환성 요구 사항

사용자 지정 IME의 기본 호환성 요구 사항은 다음과 같습니다.

### <a name="ime-must-be-compatible-with-windows-apps"></a>IME는 Windows 앱과 호환 되어야 합니다.

[TSF (텍스트 서비스 프레임 워크)](/windows/win32/tsf/text-services-framework) 를 사용 하 여 ime를 구현 합니다. 이전에는 입력 서비스에 대해 [IMM32 (입력기)](/windows/win32/intl/input-method-manager) 를 사용 하는 옵션이 있었습니다. 이제 시스템은 IMM32 (입력 메서드 관리자)를 사용 하 여 구현 된 Ime를 차단 합니다.

앱이 시작 되 면 TSF는 현재 사용자가 선택한 IME에 대 한 IME DLL을 로드 합니다. IME가 로드 되 면 앱과 동일한 앱 컨테이너 제한이 적용 됩니다. 예를 들어 앱이 매니페스트에서 인터넷 액세스를 요청 하지 않은 경우 IME는 인터넷에 액세스할 수 없습니다. 이 동작은 Ime가 보안 계약을 위반할 수 없도록 합니다.

TSF는 앱과 IME의 중간입니다. TSF는 입력 이벤트를 IME에 전달 하 고 사용자가 문자를 선택한 후 IME에서 입력 문자를 다시 받습니다.

이 동작은 이전 버전의 Windows와 동일 하지만 Windows 앱으로 로드 되는 경우 IME의 잠재적 기능에 영향을 줍니다.

IME가 Windows 앱과 데스크톱 앱 간에 다른 기능이 나 UI를 제공 해야 하는 경우 TSF에서 로드 한 DLL이 로드 되는 앱의 유형을 확인 해야 합니다. Ime에서 [ITfThreadMgrEx:: GetActiveFlags](/windows/win32/api/msctf/nf-msctf-itfthreadmgrex-getactiveflags) 메서드를 호출 하 고 TF_TMF_IMMERSIVEMODE 플래그를 확인 하 여 ime가 결과에 따라 다른 응용 프로그램 논리를 트리거합니다.

Windows 앱은 TTS (Table Text Service) Ime를 지원 하지 않습니다.

> [!NOTE]
> TTS Ime를 생성 하는 일부 도구는 Windows에 의해 맬웨어로 표시 되는 Ime를 생성 합니다.

### <a name="ime-must-be-compatible-with-the-system-tray"></a>IME는 시스템 트레이에서 호환 되어야 합니다.

IME 아이콘을 호스트 하는 데는 언어 막대가 없습니다. 대신 현재 입력 옵션을 나타내는 입력 표시기가 시스템 트레이에 표시 됩니다. 입력 표시기에는 현재 실행 중인 IME를 나타내는 IME 브랜딩 아이콘만 표시 됩니다. 또한 ime를 설정 하거나 해제 하는 등 가장 일반적으로 사용 되는 IME 모드 스위치를 수행 하기 위해 사용자가 IME 브랜딩 아이콘 왼쪽에 표시 되는 IME 모드 아이콘이 하나 있습니다.

입력 표시기는 호환 되는 Ime에 대해서만 IME 브랜딩 아이콘 및 모드 아이콘을 표시 합니다. 호환 되지 않는 Ime의 경우 시스템 트레이에 브랜딩 아이콘 및 모드 아이콘이 표시 되지 않습니다. 대신 입력 표시기는 IME 브랜딩 아이콘 대신 언어 약어를 표시 합니다.

독립형 .ico 파일 대신 DLL 또는 EXE 파일에 IME 아이콘을 저장 합니다. IME 아이콘의 디자인은 다음 UI 디자인 지침 섹션에 설명 된 지침을 따라야 합니다.

### <a name="ime-branding-icon"></a>IME 브랜딩 아이콘

입력 표시기는 시스템에 등록 된 경우 ime에 정의 된 리소스 ID를 사용 하 여 IME DLL에서 IME 브랜딩 아이콘을 가져옵니다.

### <a name="ime-mode-icon"></a>IME 모드 아이콘

일부 Ime는 시스템 트레이에 표시 되는 입력 표시기를 사용 하 여 IME 모드 아이콘을 표시 해야 할 수 있습니다. 이 경우 IME는 GUID_LBI_INPUTMODE을 사용 하 여 IME 모드 아이콘을 입력 표시기에 전달 합니다.

시스템 트레이의 입력 표시기에 IME 모드 아이콘을 전달 하는 경우 IME 모드 아이콘의 기본 크기는 16x16 픽셀입니다. UI 크기 조정은 DPI를 따릅니다.

IME 모드 아이콘을 UAC의 입력 표시기 (보안 데스크톱의 사용자 계정 컨트롤)에 전달 하는 경우 IME 모드 아이콘의 기본 크기는 20x20 픽셀입니다. UAC에서 IME 모드의 UI 크기 조정 아이콘은 PPI를 따릅니다.

## <a name="ime-must-work-in-app-container"></a>IME는 앱 컨테이너에서 작동 해야 합니다.

일부 IME 함수는 앱 컨테이너에서 영향을 받습니다.

- **사전 파일** -일반적으로 ime에는 사용자 입력을 특정 문자에 매핑하는 읽기 전용 사전 파일이 있습니다. 앱 컨테이너 내에서 이러한 파일에 액세스 하려면 IME가 Program Files 또는 Windows 디렉터리 아래에 해당 파일을 저장 해야 합니다. 기본적으로 이러한 디렉터리는 앱 컨테이너에서 읽을 수 있으므로 Ime는 이러한 위치에 저장 된 사전 파일에 액세스할 수 있습니다. IME에서 사전 파일을 다른 위치에 저장 해야 하는 경우 앱 컨테이너에서 액세스할 수 있도록 사전 파일의 [Access Control 목록 (ACL)](/windows/win32/secauthz/access-control-lists) 을 명시적으로 조작 해야 합니다.
- **인터넷 업데이트** -IME에서 인터넷의 데이터를 사용 하 여 사전을 업데이트 해야 하는 경우 인터넷 액세스가 항상 허용 되지 않기 때문에 앱 컨테이너 내부에서 안정적으로 작업을 수행할 수 없습니다. 대신 IME는 인터넷 데이터를 사용 하 여 사전 파일을 업데이트 해야 하는 별도의 데스크톱 프로세스를 실행 해야 합니다.
- **즉시 학습** -인터넷에 액세스할 수 있는 앱 컨테이너에서 ime를 실행 하는 경우 ime가 통신할 수 있는 끝점에 대 한 제한이 없습니다. 이 경우 IME는 클라우드 서버를 사용 하 여 즉석 학습 서비스를 제공할 수 있습니다. 사용자가 입력 하는 동안 일부 Ime는 사용자 입력을 즉시 다운로드 하 고 업로드 합니다. 인터넷 액세스는 앱 컨테이너에서 보장 되지 않으므로 항상 허용 되지 않을 수 있습니다.
- **프로세스 간 정보 공유** -ime는 다른 앱 컨테이너에 있는 앱 간의 사용자 입력 기본 설정에 대 한 데이터를 공유 해야 할 수 있습니다. 웹 서비스를 사용 하 여 앱 간에 데이터를 공유 합니다.

> [!Important]
> 앱 컨테이너 보안 규칙을 우회 하려고 하면 IME가 맬웨어로 처리 되 고 차단 될 수 있습니다.

## <a name="ime-and-touch-keyboard"></a>IME 및 터치 키보드

IME는 후보 창의 UI 및 기타 UI 요소를 터치 키보드 아래에 그리지 않아야 합니다. 터치 키보드는 모든 앱 보다 더 높은 z-순서 밴드에 표시 되며 IME UI는 활성화 된 앱과 동일한 z-순서 밴드에 표시 됩니다. 따라서 터치 키보드는 IME UI를 겹치고 숨길 수 있습니다. 대부분의 경우 앱은 터치 키보드를 고려 하 여 창 크기를 조정 해야 합니다. 앱의 크기를 조정 하지 않는 경우 IME는 여전히 [Inputpane](/windows/win32/api/shobjidl_core/nn-shobjidl_core-iframeworkinputpane) API를 사용 하 여 터치 키보드의 위치를 가져올 수 있습니다. IME는 [Location](/windows/win32/api/shobjidl_core/nf-shobjidl_core-iframeworkinputpane-location) 속성을 쿼리하거나 터치 키보드의 표시 및 숨기기 이벤트에 대 한 처리기를 등록 합니다. 표시 키보드는 현재 터치 키보드를 표시 하는 경우에도 사용자가 편집 필드를 누를 때마다 발생 합니다. IME는이 API를 사용 하 여 IME에서 후보 (또는 기타) UI를 그리기 전에 터치 키보드에서 사용 되는 화면 공간을 가져오고, 터치 키보드 아래에 표시 되지 않도록 Ime UI를 리플로우 할 수 있습니다.

### <a name="specifying-the-preferred-touch-keyboard-layout"></a>기본 터치 키보드 레이아웃 지정

IME는 사용할 터치 키보드 레이아웃을 지정할 수 있으며, IME는 터치 최적화 레이아웃을 사용할 수 있습니다. 한국어, 일본어, 중국어 간체 및 중국어 번체 입력 언어의 경우이 기능은 Ime로 제한 됩니다.

터치 키보드에서 지원 되는 7 가지 레이아웃이 있습니다. 세 가지는 클래식 레이아웃 이며 4 개는 터치 최적화 레이아웃입니다. 클래식 레이아웃은 실제 키보드 처럼 보이지만 동작 합니다.

세 가지 클래식 레이아웃은 모두 다른 형식으로 중국어 번체를 입력 하는 데 사용할 수 있습니다.

- 윗주 기반 입력
- Changjie 입력
- Dayi 입력

클래식 레이아웃 외에도 각각의 한국어, 일본어, 중국어 간체 및 중국어 번체 입력 언어에 대 한 터치 최적화 레이아웃이 하나씩 있습니다.

이 기능을 사용 하려면 IME가 텍스트 서비스 프레임 워크 [ITfFunctionProvider](/windows/win32/api/msctf/nn-msctf-itffunctionprovider) API를 사용 하 여 ime에서 내보내는 [ITfFnGetPreferredTouchKeyboardLayout](/windows/win32/api/ctffunc/nn-ctffunc-itffngetpreferredtouchkeyboardlayout) 인터페이스를 구현 해야 합니다.

IME가 ITfFnGetPreferredTouchKeyboardLayout 인터페이스를 지원 하지 않는 경우 IME를 사용 하면 터치 키보드에 표시 되는 언어에 대 한 기본 클래식 레이아웃을 사용 합니다.

IME에서 클래식 레이아웃 중 하나를 기본 레이아웃으로 설정 해야 하는 경우 ITfFnGetPreferredTouchKeyboardLayout 및 ITfFunctionProvider 인터페이스를 지 원하는 것 이상으로 IME 쪽에서 추가 작업이 필요 하지 않습니다. 그러나 터치 최적화 된 레이아웃을 사용 하려면 IME에 추가 작업이 필요 하며,이에 대해서는 다음 섹션에서 설명 합니다.

### <a name="touch-optimized-layout"></a>터치 최적화 레이아웃

한국어, 일본어, 중국어 간체 및 중국어 번체 입력 언어에 대 한 터치 최적화 키보드는 IME On 및 IME Off 변환 모드에 대해 다른 레이아웃을 표시 합니다. 터치 키보드에는 IME 변환 모드를 On 또는 Off로 설정 하는 키가 있지만 키보드의 IME 모드는 편집 컨트롤 간에 포커스가 변경 될 수도 있습니다.

일본어, 중국어 간체 및 중국어 번체 입력 언어에 대 한 터치 최적화 키보드에는 IME가 후보 페이지를 탐색 하는 데 사용 하는 키 또는 키가 포함 되어 있습니다. 일본어 및 중국어 간체의 경우 후보 페이지 키가 터치 최적화 레이아웃에 표시 됩니다. 중국어 번체의 경우 이전 및 다음 후보 페이지에 대 한 별도의 키가 있습니다.

이러한 키를 누르면 터치 키보드는 [SendInput](/windows/win32/api/winuser/nf-winuser-sendinput) 함수를 호출 하 여 다음 유니코드 개인 사용 영역 문자를 포커스가 있는 응용 프로그램으로 보냅니다 .이 문자는 IME에서 가로채서 작업을 수행할 수 있습니다.

- **다음 페이지 (0xF003)** -일본어 및 중국어 간체에 대 한 터치 최적화 키보드에서 후보 페이지 키를 누르거나, 중국어 번체의 경우 터치 최적화 키보드에서 다음 페이지 키를 누르면 보냅니다.
- **이전 페이지 (0xF004)** -후보 페이지 키를 일본어 및 중국어 간체에 대 한 터치 최적화 키보드의 Shift 키와 동시에 누르거나, 기존 중국어의 경우 터치 최적화 키보드에서 이전 페이지 키를 누르면 보냅니다.

이러한 문자는 유니코드 입력으로 전송 됩니다. 다음 단락에서는 텍스트 서비스 프레임 워크 IME에서 받게 되는 키 이벤트 싱크 알림 중에 문자 정보를 추출 하는 방법을 자세히 설명 합니다. 이러한 문자 값은 모든 헤더 파일에 정의 되어 있지 않으므로 코드에서 정의 해야 합니다.

키보드 입력을 차단 하려면 IME를 키 이벤트 싱크로 등록 해야 합니다. SendInput 함수를 사용 하 여 생성 된 유니코드 입력의 경우 [ITfKeyEventSink](/windows/win32/api/msctf/nn-msctf-itfkeyeventsink) 콜백 (OnKeyDown, OnKeyUp, OnTestKeyDown, Ontestkeydown)의 WPARAM 매개 변수는 항상 VK_PACKET 가상 키를 포함 하며 문자를 직접 식별 하지 않습니다.

다음 호출 시퀀스를 구현 하 여 문자에 액세스 합니다.

```cpp
// Keyboard state
BYTE abKbdState[256];
if (!GetKeyboardState(abKbdState))
{
   return 0;
}

// Map virtual key to character code
WCHAR wch;
if (ToUnicode(VK_PACKET, 0, abKbdState, &wch, 1, 0) == 1)
{
   return wch;
}
```

## <a name="ime-search-integration"></a>IME 검색 통합

검색 계약 및 검색 창과의 통합을 통해 사용자에 게 검색 기능을 제공 합니다.

:::image type="content" source="images/IMEs/ime-search-pane.png" alt-text="검색 창 및 IME 제안":::<br/>
*검색 창 및 IME 제안*

검색 창은 사용자가 모든 앱에서 검색을 수행할 수 있는 중앙 위치입니다. IME 사용자의 경우 Windows는 더 높은 효율성과 유용성을 위해 호환 되는 Ime를 Windows와 통합할 수 있는 고유한 검색 환경을 제공 합니다.

검색에 호환 되는 IME를 입력 하는 사용자에 게는 두 가지 주요 이점이 있습니다.

- IME와 검색 환경 간의 원활한 상호 작용. IME 후보는 occluding 검색 제안을 제외 하 고 검색 상자 아래에 인라인으로 표시 됩니다. 사용자는 키보드를 사용 하 여 검색 상자, IME 변환 후보 및 검색 제안 사이를 원활 하 게 탐색할 수 있습니다.
- 응용 프로그램에서 제공 하는 관련 결과 및 제안에 더 빠르게 액세스할 수 있습니다. 앱은 더 많은 관련 제안을 제공 하기 위해 모든 현재 변환 후보에 액세스할 수 있습니다. 검색 제안의 우선 순위를 높이려면 앱에 대 한 관련성을 고려 하 여 변환 합니다. 사용자는 윗주를 입력 하는 것 만으로 변환 하지 않고 원하는 결과를 찾아 선택 합니다.

IME는 다음 조건을 충족 하는 경우 통합 검색 환경과 호환 됩니다.

- Windows 스타일 셸과 호환 됩니다.
- TSF로 된 Ess 모드 Api를 구현 합니다. 자세한 내용은이 항목의 정보를 [참조 하세요.](/windows/win32/tsf/uiless-mode-overview)
- [ITfFnSearchCandidateProvider](/windows/win32/api/ctffunc/nn-ctffunc-itffnsearchcandidateprovider) 및 [ITfIntegratableCandidateListUIElement](/windows/win32/api/ctffunc/nn-ctffunc-itfintegratablecandidatelistuielement)의 TSF 검색 통합 api를 구현 합니다.

검색 창에서 활성화 되 면 호환 되는 IME가 위치에 배치 되 고 UI를 표시할 수 없습니다. 대신, 이전 스크린샷에서와 같이 인라인 후보 목록 컨트롤에 표시 하는 변환 후보를 Windows로 보냅니다.

또한 IME는 현재 검색을 실행 하는 데 사용 해야 하는 후보를 보냅니다. 이러한 후보는 변환 후보와 동일 하거나 검색에 맞게 조정할 수 있습니다.

좋은 검색 후보는 다음 조건을 충족 합니다.

- 접두사가 겹치지 않습니다. 예를 들어 北京大学 and北京는 다른의 접두사 이므로 중복 됩니다.
- 중복 후보가 없습니다. 모든 중복 후보는 결과를 필터링 하는 데 도움이 되지 않으므로 검색에 유용 하지 않습니다. 예를 들어 北京大学와 일치 하는 모든 결과가 北京 일치 합니다.
- 예측 후보가 없고 변환만 있습니다. 예를 들어 사용자가 "be"를 입력 하는 경우 IME는 北를 후보로 반환할 수 있지만 北京大学는 사용할 수 없습니다. 일반적으로 예측 후보는 너무 제한적입니다.

조건을 충족 하지 않는 Ime는 다른 컨트롤과 동일한 방식으로 검색 표시와 호환 되지 않으며 UI 통합 및 검색 후보를 활용할 수 없습니다. 앱은 사용자가 작성을 완료 한 후에만 쿼리를 수신 합니다.

검색 계약을 지 원하는 앱이 쿼리를 받을 때 쿼리 이벤트에는 가장 관련성이 높은 (가능성이 가장 높은)에서 가장 관련성이 높은 모든 대안 (가능성이 낮은 항목)을 포함 하는 "queryTextAlternatives" 배열이 포함 됩니다.

대안을 제공 하는 경우 앱은 각 대안을 쿼리로 처리 하 고 다른 대안과 일치 하는 모든 결과를 반환 해야 합니다. 앱은 사용자가 동시에 여러 쿼리를 실행 하 여 결과를 제공 하는 서비스에 "또는" 쿼리를 실행 하는 것 처럼 동작 해야 합니다. 성능 고려 사항을 위해 앱은 가장 관련성이 가장 높은 대체 항목의 5 ~ 20 개로 제한 되는 경우가 많습니다.

## <a name="ui-design-guidelines"></a>UI 디자인 지침

모든 Ime는 [Windows 앱 디자인 및 코드](/windows/uwp/design/)에 설명 된 사용자 환경 지침을 따라야 합니다.

### <a name="dont-use-sticky-windows"></a>고정 창을 사용 하지 않음

IME 창은 필요한 경우에만 표시 되며 항상 표시 되어서는 안 됩니다. 사용자가 입력 하지 않아도 되는 경우 IME 창이 표시 되지 않습니다. IME 창은 전체 화면 창이 아니어야 합니다. IME 창은 서로 겹치지 않아야 합니다. Windows는 windows 스타일로 디자인 되 고 UI 크기 조정을 따라야 합니다.

### <a name="ime-icons"></a>IME 아이콘

두 종류의 IME 아이콘, 브랜딩 아이콘 및 모드 아이콘이 있습니다. 모든 IME 아이콘은 검정과 흰색 으로만 디자인 해야 합니다. 시스템 트레이 아이콘의 glyphic 모양에서 새 IME 아이콘이 표시 됩니다. 이 스타일은 모든 언어에서이 스타일을 사용 하 여 familial 모양을 보충 하 고 서로 차별화 하는 데 사용할 수 있습니다.

IME 아이콘의 파일 형식은 .ICO입니다. 다음 아이콘 크기를 제공 해야 합니다.

- 16x16 픽셀
- 20x20 픽셀
- 24x24 픽셀
- 32x32 픽셀
- 40x40 픽셀
- 48x48 픽셀

알파 채널이 있는 32 비트 아이콘이 모든 해상도로 제공 되는지 확인 합니다.

IME 브랜드 아이콘은 최신 서체에서 렌더링 된 입력 문자 모양을 배치 하는 흰색 상자에 의해 정의 됩니다. 각 언어 팀은 각 정의 문자 모양을 선택 합니다. 문자 모양이 검정색입니다. 상자에는 1 픽셀의 외부 스트로크가 검정으로 50% 불투명도로 포함 됩니다. "New" 버전은 상자의 왼쪽 위에서 모퉁이가 둥근 모퉁이로 정의 됩니다.

IME 모드 아이콘은 1 픽셀의 외부 스트로크를 50% 불투명도의 검정색으로 포함 하는 현대 서체에서 흰색 입력 문자 모양으로 정의 됩니다.

| 아이콘 | 설명 |
| --- | --- |
| :::image type="content" source="images/IMEs/ime-brand-icon-traditional-chinese.png" alt-text="중국어 번체 ChangeJie의 예제 IME 브랜드 아이콘"::: | 중국어 번체 ChangeJie의 예제 IME 브랜드 아이콘 |
| :::image type="content" source="images/IMEs/ime-brand-icon-traditional-chinese-new.png" alt-text="중국어 번체 새 ChangeJie의 예제 IME 브랜드 아이콘."::: | 중국어 번체 ChangeJie의 예제 IME 브랜드 아이콘 |
| :::image type="content" source="images/IMEs/ime-mode-icon-chinese.png" alt-text="중국어 모드 아이콘"::: | 예를 들어 IME 모드 아이콘이 있습니다. |

### <a name="owned-window"></a>소유 창

후보 UI를 표시 하려면 IME가 창을 소유 하도록 설정 해야 합니다. 그러면 현재 실행 중인 앱 위에 표시 될 수 있습니다. [Itfcontextview:: GetWnd](/windows/win32/api/msctf/nf-msctf-itfcontextview-getwnd) 메서드를 사용 하 여 소유할 창을 검색 합니다. GetWnd에서 오류 또는 NULLHWND를 반환 하는 경우 [GetFocus](/windows/win32/api/msctf/nf-msctf-itfthreadmgr-getfocus) 함수를 호출 합니다.

`if (FAILED(pView->GetWnd(&parentWndHandle)) || (parentWndHandle == nullptr)) { parentWndHandle = GetFocus(); }`

### <a name="ime-candidate-window-interaction-with-light-dismiss-surfaces"></a>밝은 해제 서피스와의 IME 후보 창 상호 작용

팝업 창에 대 한 해제 모델은 사용자가 해당 창을 닫을 수 있기 때문에 "밝은 해제" 라고 합니다. Ime가 Windows 상호 작용 모델에서 잘 작동 하려면 IME 창이 라이트 해제 모델에 참여 해야 합니다.

Light 해제 모델에 참여 하려면 IME가 [NotifyWinEvent](/windows/win32/api/winuser/nf-winuser-notifywinevent) 함수 또는 유사한 함수를 사용 하 여 세 개의 새 Windows 이벤트를 발생 시켜야 합니다. 이러한 새 이벤트는 다음과 같습니다.

- **EVENT_OBJECT_IME_SHOW** -IME가 표시 되 면이 이벤트를 발생 시킵니다.
- **EVENT_OBJECT_IME_HIDE** -IME를 숨기면이 이벤트를 발생 시킵니다.
- **EVENT_OBJECT_IME_CHANGE** -IME를 이동 하거나 크기를 변경할 때이 이벤트를 발생 시킵니다.

### <a name="declaring-compatibility"></a>호환성 선언

Ime는 [ITfCategoryMgr:: RegisterCategory](/windows/win32/api/msctf/nf-msctf-itfcategorymgr-registercategory)를 사용 하 여 IME에 GUID_TFCAT_TIPCAP_IMMERSIVESUPPORT 범주를 등록 하 여 호환 가능 함을 선언 합니다.

### <a name="set-the-default-ime-mode-to-on"></a>기본 IME 모드를 켜기로 설정 합니다.

Ime에 대해 더 나은 UX를 제공 합니다.

## <a name="dpi-scaling-support-for-desktop-applications"></a>데스크톱 응용 프로그램에 대 한 DPI 크기 조정 지원

향상 된 DPI 크기 조정 지원을 통해 각 데스크톱 프로세스의 선언 된 DPI 인식 수준을 쿼리하여 UI의 크기를 조정 해야 하는지 여부를 결정할 수 있습니다. 다중 모니터 시나리오에서 Windows는 각 모니터의 DPI 설정에 맞게 UI의 크기를 조정 합니다.

IME는 각 응용 프로그램 프로세스의 컨텍스트에서 실행 되므로 IME에 대 한 DPI 인식 수준을 선언 하면 안 됩니다. 이렇게 하면 IME가 현재 프로세스의 DPI 인식 수준에서 실행 됩니다.

모든 IME UI 요소에를 실행 하는 프로세스의 UI 요소를 사용 하 여 크기 조정 패리티를 포함 하려면 다른 DPI 값에 적절 하 게 응답 해야 합니다.

> [!NOTE]
> 새 데스크톱 응용 프로그램을 사용 하 여 패리티를 보장 하기 위해 IME는 모니터 당 DPI 인식을 지원 해야 하지만 인식 수준 자체는 선언 하지 않아야 합니다. 시스템은 각 시나리오에서 적절 한 크기 조정 요구 사항을 결정 합니다.

데스크톱 응용 프로그램의 DPI 크기 조정 지원 요구 사항에 대 한 자세한 내용은 [높은 DPI](/windows/win32/hidpi/high-dpi-desktop-application-development-on-windows)를 참조 하세요.

## <a name="ime-installation"></a>IME 설치

Microsoft Visual Studio를 사용 하 여 IME를 빌드하는 경우에는 InstallShield와 같은 타사 설치 관리자를 사용 하 여 IME에 대 한 설치 환경을 만듭니다.

다음 단계에서는 InstallShield를 사용 하 여 IME DLL에 대 한 설치 프로젝트를 만드는 방법을 보여 줍니다.

- Visual Studio를 설치합니다.
- Visual Studio를 시작합니다.
- **파일** 메뉴에서 **새로 만들기** 를 가리키고 **프로젝트**를 선택 합니다. **새 프로젝트** 대화 상자가 열립니다.
- 왼쪽 창에서 **템플릿 > 기타 프로젝트 형식 > 설정 및 배포**로 이동 하 고 **InstallShield 제한 된 버전 사용**을 클릭 한 다음 **확인**을 클릭 합니다. 설치 지침을 따릅니다.
- Visual Studio를 다시 시작합니다.
- IME 솔루션 (.sln) 파일을 엽니다.
- 솔루션 탐색기에서 솔루션을 마우스 오른쪽 단추로 클릭 하 고 **추가**를 가리킨 다음 **새 프로젝트**를 선택 합니다. **새 프로젝트 추가** 대화 상자가 열립니다.
- 왼쪽 트리 뷰 컨트롤에서 **템플릿 > 다른 프로젝트 형식 > InstallShield 제한 된 버전**으로 이동 합니다.
- 가운데 창에서 **InstallShield 제한 된 버전 프로젝트**를 클릭 합니다.
- **이름** 텍스트 상자에 "setupime"를 입력 하 고 **확인**을 클릭 합니다.
- **프로젝트 도우미** 대화 상자에서 **응용 프로그램 정보**를 클릭 합니다.
- 회사 이름 및 다른 필드를 입력 합니다.
- **응용 프로그램 파일**을 클릭 합니다.
- 왼쪽 창에서 **[INSTALLDIR]** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **새 폴더**를 선택 합니다. 폴더 이름을 "플러그 인"으로 합니다.
- **파일 추가**를 클릭 합니다. IME DLL로 이동 하 여 **플러그 인** 폴더에 추가 합니다. IME 사전에 대해이 단계를 반복 합니다.
- IME DLL을 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다. **속성** 대화 상자가 열립니다.
- **속성** 대화 상자에서 **COM & .net 설정** 탭을 클릭 합니다.
- **등록 유형**에서 **직접 등록** 을 선택 하 고 **확인**을 클릭 합니다.
- 솔루션을 빌드합니다. IME DLL이 빌드되고 InstallShield에서 사용자가 Windows에 IME를 설치할 수 있도록 하는 setup.exe 파일을 만듭니다.

사용자 고유의 설치 환경을 만들려면 [ITfInputProcessorProfileMgr:: RegisterProfile](/windows/win32/api/msctf/nf-msctf-itfinputprocessorprofilemgr-registerprofile) 메서드를 호출 하 여 설치 하는 동안 IME를 등록 합니다. 레지스트리 항목을 직접 쓰지 않습니다.

설치 후에 IME를 즉시 사용할 수 있어야 하는 경우 [InstallLayoutOrTip](/windows/win32/tsf/installlayoutortip) 를 호출 하 여 사용자가 사용 하도록 설정 된 입력 방법에 다음 psz 매개 변수에 대 한 형식을 사용 하 여 ime를 추가 합니다.

`<LangID 1>:{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}`

## <a name="ime-accessibility"></a>IME 접근성

Ime가 접근성 요구 사항을 준수 하 고 내레이터를 사용 하도록 하려면 다음 규칙을 구현 합니다. 후보 목록에 액세스할 수 있도록 하려면 Ime가이 규칙을 따라야 합니다.

- 후보 목록에는 변환 후보 목록에 대 한 "IME_Candidate_Window"와 같은 **UIA_AutomationIdPropertyId** 또는 예측 후보 목록의 "IME_Prediction_Window"가 있어야 합니다.
- 후보 목록이 표시 되 고 사라지면 각각 **UIA_MenuOpenedEventId** 및 **UIA_MenuClosedEventId**형식의 이벤트를 발생 시킵니다.
- 현재 선택한 후보가 변경 되 면 후보 목록에서 **UIA_SelectionItem_ElementSelectedEventId**발생 합니다. 선택한 요소의 속성 **UIA_SelectionItemIsSelectedPropertyId** **TRUE**와 같아야 합니다.
- 후보 목록의 각 항목에 대 한 **UIA_NamePropertyId** 는 후보 이름 이어야 합니다. 필요에 따라 **UIA_HelpTextPropertyId**를 통해 후보를 구분 하는 추가 정보를 제공할 수 있습니다.

## <a name="related-topics"></a>관련 항목

- [IME(입력기)](input-method-editors.md)
- [ITfFnGetPreferredTouchKeyboardLayout](/windows/win32/api/ctffunc/nn-ctffunc-itffngetpreferredtouchkeyboardlayout)
- [ITfCompartmentEventSink](/windows/win32/api/msctf/nn-msctf-itfcompartmenteventsink)
- [ITfThreadMgrEx:: GetActiveFlags](/windows/win32/api/msctf/nf-msctf-itfthreadmgrex-getactiveflags)
- [ITfContextView:: GetWnd](/windows/win32/api/msctf/nf-msctf-itfcontextview-getwnd)
- [TF_INPUTPROCESSORPROFILE](/windows/win32/api/msctf/ns-msctf-tf_inputprocessorprofile)
- [ITfFnSearchCandidateProvider](/windows/win32/api/ctffunc/nn-ctffunc-itffnsearchcandidateprovider)
- [ITfIntegratableCandidateListUIElement](/windows/win32/api/ctffunc/nn-ctffunc-itfintegratablecandidatelistuielement)
- [SendInput](/windows/win32/api/winuser/nf-winuser-sendinput)
- [접근성](/windows/uwp/design/accessibility/accessibility)
