---
description: 뉴스와 변경 C + + WinRT 합니다.
title: 새로운 C + + WinRT
ms.date: 01/29/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c + +, cpp, winrt, 프로젝션, 뉴스, 어떤의 새
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b46aaf9948587ef79a9c5bf73064b1a70c0e9c3a
ms.sourcegitcommit: a71122082947b4cc3d157465e402746760d1d5c2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2019
ms.locfileid: "9035708"
---
# <a name="whats-new-in-cwinrt"></a>새로운 C + + WinRT

아래 표에 뉴스를 포함 하 고 변경 [C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 10.0.17763.0 (Windows 10, 버전 1809)는 Windows SDK의 일반적으로 사용 가능한 최신 버전입니다. 이러한 변경 내용은 이후 SDK 참가자 미리 보기 버전에 나타날 수도 있습니다.

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>뉴스 및 변경 내용, Windows SDK 버전 10.0.17763.0 (Windows 10, 버전 1809)

| 새로운 또는 변경 된 기능 | 추가 정보 |
| - | - |
| **주요 변경 내용**입니다. 컴파일, C + + WinRT에서 Windows SDK 헤더에 따라 달라 지지 않습니다. | [Windows SDK 헤더 파일에서 격리](#isolation-from-windows-sdk-header-files)아래를 참조 하세요. |
| Visual Studio 프로젝트 시스템 형식을 변경 되었습니다. | 참조 [방법을 대상을 C + + Windows SDK의 최신 버전으로 WinRT 프로젝트](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)아래의 합니다. |
| 새로운 기능 및 컬렉션 개체는 Windows 런타임 함수를 전달할 수 있도록 하거나 고유한 컬렉션 속성 및 컬렉션 형식과 구현 하려면 기본 클래스가 있습니다. | 참조 [컬렉션 C + + WinRT](collections.md). |
| [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) 태그 확장을 사용 하 여 C + + WinRT 런타임 클래스. | 자세한 내용 및 코드 예제를 [데이터 바인딩 개요](/windows/uwp/data-binding/data-binding-quickstart)를 참조 하세요. |
| 코 루틴을 취소 하는 것에 대 한 지원을 취소 콜백을 등록할 수 있습니다. | 자세한 정보 및 코드 예제에 대 한 [비동기 작업을 취소 콜백을 취소](concurrency.md#canceling-an-asychronous-operation-and-cancellation-callbacks)를 참조 하세요. |
| 멤버 함수를 가리키는 대리자를 만들 때 강한 또는 약한 참조 처리기 등록 되어 있는 시점 (대신 원시 *이* 포인터) 현재 개체를 설정할 수 있습니다. | 자세한 내용 및 코드 예제를 [안전 하 게 이벤트 처리 대리자를 사용 하 여 *이* 포인터에 액세스](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)섹션에서 **멤버 함수를 대리인으로 사용 하는 경우** 하위 섹션을 참조 하세요. |
| 버그 표준 c + +에 대 한 Visual Studio의 개선 된 규칙으로 처리 된 않는 고정 됩니다. LLVM 및 Clang 도구 체인도 더 활용 유효성을 검사할 C + + /winrt의 표준 준수 합니다. | 문제 [에 설명 된 이유 없습니다 새 프로젝트 더 이상 나옵니다 컴파일? Visual Studio 2017을 사용 하 고 (15.8.0 버전 이상), 및 SDK 버전 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

기타 변경 합니다.

- **주요 변경 내용**입니다. 이제 반환 [**winrt::get_abi(winrt::hstring const&)**](/uwp/cpp-ref-for-winrt/get-abi) `void*` 대신 `HSTRING`합니다. 사용할 수 있습니다 `static_cast<HSTRING>(get_abi(my_hstring));` 는 HSTRING 가져옵니다.
- **주요 변경 내용**입니다. 이제 반환 [**winrt::put_abi(winrt::hstring&)**](/uwp/cpp-ref-for-winrt/put-abi) `void**` 대신 `HSTRING*`합니다. 사용할 수 있습니다 `reinterpret_cast<HSTRING*>(put_abi(my_hstring));` HSTRING * 가져옵니다.
- **주요 변경 내용**입니다. HRESULT은 **winrt::hresult**처럼 이제 투영 됩니다. HRESULT (형식 검사를 수행 하려면 또는 형식 특성을 지원 하기 위해), 필요한 경우 할 수 있습니다 `static_cast` **winrt::hresult**합니다. 그렇지 않으면 **winrt::hresult** 변환 HRESULT 포함으로 `unknwn.h` 전에 포함 하는 C + + /winrt 헤더.
- **주요 변경 내용**입니다. GUID는 이제 **winrt::guid**처럼 투영 됩니다. 구현 하는 Api, **winrt::guid** GUID 매개 변수에 대 한 사용 해야 합니다. 그렇지 않은 경우 **winrt::hresult** 변환 GUID로 포함 `unknwn.h` 전에 포함 하는 C + + /winrt 헤더.
- **주요 변경 내용**입니다. [**Winrt::handle_type 생성자**](/uwp/cpp-ref-for-winrt/handle-type#handletypehandletype-constructor) (어렵다는 이제 받는지 잘못 된 코드를 작성) 명시적 함으로써 강화 되었습니다. 원시 핸들 값을 할당 해야 하는 경우 대신 [**handle_type::attach 함수**](/uwp/cpp-ref-for-winrt/handle-type#handletypeattach-function) 를 호출 합니다.
- **주요 변경 내용**입니다. **WINRT_CanUnloadNow** 및 **WINRT_GetActivationFactory** 의 서명을 변경 되었습니다. 이러한 함수는 전혀 선언 돼 합니다. 대신 포함 `winrt/base.h` (포함 되어 있는 자동으로 포함 하면 모든 C + + /winrt Windows 네임 스페이스 헤더 파일) 이러한 함수는 선언을 포함 하도록.
- [**Winrt::clock 구조체**](/uwp/cpp-ref-for-winrt/clock)에 대 한 **from_FILETIME/to_FILETIME** **from_file_time/to_file_time**를 위해이 사용 되지 않습니다.
- **IBuffer** 매개 변수를 것으로 예상 되는 Api 하기 쉽습니다. 대부분의 Api 원하는 컬렉션 또는 배열 있지만 충분 한 Api c + +에서 이러한 Api를 사용 하기 쉬우며 되어야 하는 데 필요한 **IBuffer** 에 의존 합니다. 이 업데이트는 c + + 표준 라이브러리 컨테이너에서 사용 되는 동일한 데이터 명명 규칙을 사용 하 여 **IBuffer** 구현 관련 된 데이터에 직접 액세스를 제공 합니다. 이 또한 아니므로 대문자로 시작 하는 메타 데이터 이름을와 충돌을 방지 합니다.
- 코드 생성 향상: 코드 크기를 줄이기 위해 다양 한 개선, 인라인을 개선 하 고 공장 캐싱 최적화 합니다.
- 불필요 한 재귀를 제거 합니다. 때 명령줄 참조 아니라 특정 폴더에 `.winmd`, `cppwinrt.exe` 도구를 재귀적으로 더 이상 검색 `.winmd` 파일. `cppwinrt.exe` 도구가 이제 처리 중복 더 지능적으로 사용자 오류 력이 이며에 잘못 된 형식의 `.winmd` 파일.
- 스마트 포인터를 강화 합니다. 이전 버전에서는 이벤트 revokers 경우 해지 하지 못했습니다 이동-할당 된 새 값입니다. 이 도움이 스마트 포인터 클래스 자체 할당; 미치지 안정적으로 처리 하는 위치는 문제를 다루지 않습니다. [**winrt:: com_ptr 구조체 템플릿**](/uwp/cpp-ref-for-winrt/com-ptr)루트가 지정 됩니다. **winrt:: com_ptr** 는 문제가 수정 하 고 처리에 고정 이벤트 revokers 이동 의미 체계 올바르게 할당 시 취소할 수 있도록 합니다.

> [!NOTE]
> 중요 한 변경 내용 합니다 [C + + WinRT Visual Studio Extension (VSIX)](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-and-the-vsix), 모두 버전 1.0.181002.2, 그리고 1.0.190128.4 버전입니다. 이러한 변경 및 프로젝트에 미치는 영향 수의 내용은 참조 [Visual Studio 지원 C + + /winrt 및 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-and-the-vsix)합니다.

## <a name="isolation-from-windows-sdk-header-files"></a>Windows SDK 헤더 파일에서 격리

잠재적으로 코드에 대 한 주요 변경 내용입니다.

컴파일, C + + WinRT 더 이상 Windows SDK에서 헤더 파일에 따라 달라 집니다. 또한 C 런타임 라이브러리 (CRT) 및 c + + 템플릿 라이브러리 (STL 표준)의 헤더 파일 모든 Windows SDK 헤더를 포함 하지 마세요. 표준 준수를 개선 하는 실수로 종속성을 피할 수 및 방지 해야 하는 매크로 수를 크게 줄입니다 및 합니다.

이러한 독립성 의미는 C + + /winrt는 이제 더 많은 노트북 및 표준 규격 하 고 있는 가능성을 되는 플랫폼 간 및 크로스 컴파일러 라이브러리를 furthers 합니다. 또한는 의미 C + + /winrt 헤더에 부정적인 영향을 받는 매크로 되지 않습니다.

두면 이전에를 C + + /winrt를 직접 포함 해야 이제 다음 프로젝트에서 모든 Windows 헤더를 포함 합니다. 인 어떤 경우 든 항상 유용한 명시적으로,에 의존 하는 헤더를 포함 하는 방법 및 다른 라이브러리를 포함 하도록 두지 합니다.

현재 Windows SDK 헤더 파일 격리의 유일한 예외는 내부 및 숫자에 대 한 있습니다. 이러한 마지막 나머지 종속성을 사용 하 여 알려진 문제가 있습니다.

프로젝트에서 하는 경우 Windows SDK 헤더와의 상호 운용성 다시 사용할 수 있습니다. 예를 들어 ( [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)의 신뢰가) COM 인터페이스를 구현 수 있습니다. 이 예에서 포함 `unknwn.h` 전에 포함 하는 C + + /winrt 헤더. 이렇게 하면 C + + WinRT 기본 라이브러리 클래식 COM 인터페이스를 지원 하기 위해 다양 한 후크를 사용 하도록 설정 합니다. 코드 예제를 보려면 [작성자 COM 구성 요소 C + + WinRT](author-coclasses.md). 마찬가지로, 형식 및/또는 호출 하려는 함수를 선언 하는 다른 Windows SDK 헤더를 명시적으로 포함 합니다.

## <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>방법 대상을 C + + Windows SDK의 최신 버전으로 WinRT 프로젝트

프로젝트 최소 컴파일러 및 링커 문제 발생 가능성이 있는 대상 변경 하는 방법은 가장 손이 많이 이기도 합니다. (선택한 Windows SDK 버전을 대상으로) 새 프로젝트를 만들고 다음 파일을 복사 새 프로젝트에서 기존의 해당 방법은 방법입니다. 기존의의 섹션 됩니다 `.vcxproj` 및 `.vcxproj.filters` 수 있는 파일 복사 넘는 Visual Studio에서 파일을 추가 하면 됩니다.

그러나는 Visual Studio에서 프로젝트 대상을 다른 두 가지가 있습니다.

- 프로젝트 속성 **일반**로 이동 \> **Windows SDK 버전**선택 **모든 구성** 및 **모든 플랫폼**입니다. **Windows SDK 버전** 대상으로 지정할 버전으로 설정 합니다.
- **솔루션 탐색기**프로젝트 노드를 마우스 오른쪽 단추로 클릭 **대상을 프로젝트**를 클릭를 대상으로 하려는 버전 선택한 후 **확인**을 클릭 합니다.

이러한 두 가지 방법 중 하나를 사용 하 여 모든 컴파일러 또는 링커 오류가 발생할 경우 솔루션 정리를 시도할 수 있습니다 (**빌드** > **솔루션 정리** 모든 임시 폴더 및 파일을 수동으로 삭제 및/또는)를 다시 빌드하기 전에 합니다.

C + + 컴파일러를 생성 하는 경우 "*C2039 오류: 'IUnknown':의 구성원이 아닙니다 ' \'global 네임 스페이스 '*", 다음 추가 `#include <unknwn.h>` 의 맨 위에 `pch.h` 파일 (전에 포함 하는 C + + /winrt 헤더).

추가 해야 `#include <hstring.h>` 그러고 합니다.

C + + 링커를 생성 하는 경우 "*오류 LNK2019: 외부 기호가 _WINRT_CanUnloadNow@0 함수에서 참조 _VSDesignerCanUnloadNow@0 *"을를 추가 하 여 해결할 수 있습니다 `#define _VSDESIGNER_DONT_LOAD_AS_DLL` 를 `pch.h` 파일.
