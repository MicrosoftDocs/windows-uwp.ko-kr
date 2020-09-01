---
Description: IME (입력기)는 사용자가 표준 QWERTY 키보드에서 쉽게 나타낼 수 없는 언어로 텍스트를 입력할 수 있도록 하는 소프트웨어 구성 요소입니다.
title: IME (입력기)
label: Input Method Editors (IME)
template: detail.hbs
keywords: ime, 입력 방법 편집기, 입력, 상호 작용
ms.date: 07/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8e7782dea8cd634fd9fe3bac4a3e4c870cd680e9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159947"
---
# <a name="input-method-editors-ime"></a>IME (입력기)

IME (입력기)는 사용자가 표준 QWERTY 키보드에서 쉽게 나타낼 수 없는 언어로 텍스트를 입력할 수 있도록 하는 소프트웨어 구성 요소입니다. 이는 일반적으로 다양 한 동아시아 언어와 같이 사용자가 작성 한 언어의 문자 수로 인해 발생 합니다.

사용자는 단일 키보드 키에 표시 되는 각 문자 대신 IME로 해석 되는 키 조합을 입력 합니다. IME는 키 스트로크 집합과 일치 하는 문자 또는 선택할 후보 문자 목록을 생성 합니다. 그러면 사용자가 상호 작용 하는 편집 컨트롤에 선택한 문자가 삽입 됩니다.

> [!NOTE]
> Ime는 하드웨어 키보드와 화상 키보드 또는 터치 키보드를 모두 지원할 수 있습니다.

앱은 IME와 직접 상호 작용할 필요가 없습니다. 터치 키보드와 마찬가지로 IME는 시스템에 기본 제공 됩니다. 앱에 텍스트 입력이 있고 IME가 필요한 언어에서 텍스트 입력을 지원 하려는 경우 텍스트 입력에 대 한 종단 간 고객 환경을 테스트 해야 합니다. 그러면 UI를 조정 하는 것과 같은 문제를 해결 하 여 터치 키보드 또는 IME 후보 창에서 폐색 하지 않을 수 있습니다.

## <a name="creating-an-ime"></a>IME 만들기

모든 사용자에 게 훌륭한 입력 환경을 제공 하기 위해 Microsoft는 다양 한 언어에 대해 기본 제공 되는 Ime를 생성 합니다.

기본 입력기 외에도 사용자가 바로 사용할 수 있는 IME와 마찬가지로 사용자 지정 Ime를 빌드할 수 있습니다.

모든 Ime는 악의적인 Ime를 중지 하 고 모든 Ime의 보안 및 사용자 환경을 개선 하기 위해 확정 된 Windows 시스템에서 실행 됩니다.

사용자 지정 Ime는 기본 터치 키보드에 연결 하 고 해당 레이아웃을 사용 하 여 최종 사용자가 터치 키보드에 IME를 사용할 수 있습니다. 그러나 사용자 고유의 독립적인 터치 키보드를 제공할 수 없으며 터치 키보드의 특정 기능을 제공 하는 특정 기능을 사용자 지정 Ime에서 사용할 수 없습니다.

## <a name="requirements-for-imes"></a>Ime 요구 사항

타사 IME는 다음과 같은 요구 사항을 충족 해야 합니다.

- 디지털 서명 해야 합니다.
- 적절 한 IME 플래그가 올바르게 설정 된 상태에서 [TSF (텍스트 서비스 프레임 워크)](/windows/win32/tsf/text-services-framework) 를 인식 해야 합니다.
- [Ime (입력기) 요구 사항](input-method-editor-requirements.md) 및 [디자인 및 코드 Windows 앱](../index.md) 에 설명 된 지침을 따라야 합니다.

이러한 요구 사항을 충족 하지 않는 타사 IME의 실행이 차단 됩니다.

> [!NOTE]
> 레거시 사용자 지정 Ime는 데스크톱 앱에서 실행할 수 있지만 Windows 앱에서 차단 됩니다.

또한 Windows Defender는 시스템에서 악성 입력기를 제거 합니다. 따라서 IME 코딩 요구 사항에 대해 잘 알고 있어야 합니다. 자세한 내용은 [ime (입력기) 요구 사항](input-method-editor-requirements.md)을 참조 하세요.

## <a name="design-guidelines-for-imes"></a>Ime 디자인 지침

Ime의 모범 사례 및 디자인 지침에 대 한 자세한 내용은 [ime (입력기) 요구 사항](input-method-editor-requirements.md) 을 참조 하세요. 일반적으로 모든 IME Ui는 다음을 수행 해야 합니다.

- Windows 런타임 앱에 대 한 UX 지침을 따릅니다.
- 모달 환경을 사용 하지 않고 필요한 경우 IME 창만 표시
- 검정색과 흰색 으로만 아이콘 포함

## <a name="related-topics"></a>관련 항목

- [IME (입력기) 요구 사항](input-method-editor-requirements.md)
- [ITfFnGetPreferredTouchKeyboardLayout](/windows/win32/api/ctffunc/nn-ctffunc-itffngetpreferredtouchkeyboardlayout)
- [ITfCompartmentEventSink](/windows/win32/api/msctf/nn-msctf-itfcompartmenteventsink)
- [ITfThreadMgrEx:: GetActiveFlags](/windows/win32/api/msctf/nf-msctf-itfthreadmgrex-getactiveflags)
- [ITfContextView:: GetWnd](/windows/win32/api/msctf/nf-msctf-itfcontextview-getwnd)
- [TF_INPUTPROCESSORPROFILE](/windows/win32/api/msctf/ns-msctf-tf_inputprocessorprofile)
- [SendInput](/windows/win32/api/winuser/nf-winuser-sendinput)