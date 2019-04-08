---
description: C++/WinRT에서 새롭거나 변경된 기능입니다.
title: 새로운 C + + /cli WinRT
ms.date: 01/29/2019
ms.topic: article
keywords: windows 10, uwp, standard, c + +, cpp, winrt, 프로젝션, 뉴스 항목의 새,
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: cb624a93a010dfe9784cf8c26beed12c6cf2f77d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639958"
---
# <a name="whats-new-in-cwinrt"></a>새로운 C + + /cli WinRT

아래 표에서 뉴스가 포함 및 변경 [C + + /cli WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 10.0.17763.0 (Windows 10, 버전 1809)는 Windows SDK의 최신 일반 공급 버전에서는 합니다. 이러한 변경 내용은 이후 SDK Insider Preview 버전 수도 있습니다.

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>뉴스 및 변경 내용을 Windows sdk 버전 10.0.17763.0 (Windows 10, 버전 1809)

| 새롭거나 변경 된 기능 | 추가 정보 |
| - | - |
| **주요 변경 내용**합니다. 해당 코드를 컴파일하려면를 C + + WinRT Windows SDK의 헤더에 종속 되지 않습니다. | 참조 [Windows SDK 헤더 파일에서 격리](#isolation-from-windows-sdk-header-files)아래. |
| Visual Studio 프로젝트 시스템 형식이 변경 되었습니다. | 참조 [대상을 C + + /cli WinRT 프로젝트는 Windows SDK의 최신 버전으로](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)아래. |
| 새로운 함수 및 기본 클래스 컬렉션 개체를 Windows 런타임 함수를 전달할 수 있도록 하거나 고유한 컬렉션 속성 및 컬렉션 형식 구현 됩니다. | 참조 [컬렉션을 사용 하 여 C + + /cli WinRT](collections.md)합니다. |
| 사용할 수는 [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) 태그 확장을 사용 하 여 C + + /cli WinRT 런타임 클래스입니다. | 자세한 내용은 및 코드 예제를 참조 하세요 [데이터 바인딩 개요](/windows/uwp/data-binding/data-binding-quickstart)합니다. |
| 코 루틴을 취소 하는 것에 대 한 지원을 사용 하면 취소 콜백을 등록할 수 있습니다. | 자세한 내용은 및 코드 예제를 참조 하세요 [비동기 작업을 취소 콜백을 취소](concurrency.md#canceling-an-asychronous-operation-and-cancellation-callbacks)합니다. |
| 강력한 또는 약한 참조를 현재 개체는 멤버 함수를 가리키는 대리자를 만들 때 설정할 수 있습니다 (대신 원시 *이* 포인터) 처리기 등록 되어 있는 시점입니다. | 자세한 정보 및 코드 예제를 참조 합니다 **대리자로 멤버 함수를 사용 하는 경우** 섹션의 하위 섹션 [안전 하 게 액세스를 *이* 이벤트처리대리자를사용하여포인터](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate). |
| C + + 표준 Visual Studio의 향상 된 규칙으로 처리 된 않는 버그 수정. LLVM 및 Clang 도구 체인도 더 잘 활용 유효성을 검사 하 고 C + + /cli WinRT의 표준 준수 합니다. | 에 설명 된 문제를 더 이상 접할 [내 새 프로젝트 컴파일되지 이유? Visual Studio 2017을 사용 하 고 (15.8.0 버전 이상), 및 SDK 버전 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

다른 변경 됩니다.

- **주요 변경 내용**합니다. [**winrt::get_abi(winrt::hstring const&)** ](/uwp/cpp-ref-for-winrt/get-abi) 이제 반환 `void*` 대신 `HSTRING`합니다. 사용할 수 있습니다 `static_cast<HSTRING>(get_abi(my_hstring));` HSTRING를 가져오려고 합니다.
- **주요 변경 내용**합니다. [**winrt::put_abi(winrt::hstring&)** ](/uwp/cpp-ref-for-winrt/put-abi) 이제 반환 `void**` 대신 `HSTRING*`합니다. 사용할 수 있습니다 `reinterpret_cast<HSTRING*>(put_abi(my_hstring));` HSTRING * 가져오려고 합니다.
- **주요 변경 내용**합니다. HRESULT로 투영 되 **winrt::hresult**합니다. HRESULT (형식 검사를 수행 하거나 형식 특성을 지원 하기 위해), 필요한 경우 있습니다 `static_cast` 는 **winrt::hresult**합니다. 그렇지 않으면 **winrt::hresult** HRESULT를 포함 하는 만큼 변환 `unknwn.h` 모든 C + 포함 하기 전에 + WinRT 헤더입니다.
- **주요 변경 내용**합니다. GUID 처럼 이제 투영 됩니다 **winrt::guid**합니다. 구현 하는 Api를 사용 해야 합니다 **winrt::guid** GUID 매개 변수에 대 한 합니다. 그렇지 않으면 **winrt::hresult** 으로 포함 하는 GUID 변환 `unknwn.h` 모든 C + 포함 하기 전에 + WinRT 헤더입니다.
- **주요 변경 내용**합니다. 합니다 [ **winrt::handle_type 생성자** ](/uwp/cpp-ref-for-winrt/handle-type#handletypehandletype-constructor) (이제 하기가 어렵다는 것을 사용 하 여 잘못 된 코드를 작성) 명시적 만들어 강화 되었습니다. 원시 핸들 값을 할당 해야 할 경우 호출 된 [ **handle_type::attach 함수** ](/uwp/cpp-ref-for-winrt/handle-type#handletypeattach-function) 대신 합니다.
- **주요 변경 내용**합니다. 서명을 **WINRT_CanUnloadNow** 하 고 **WINRT_GetActivationFactory** 변경 되었습니다. 이러한 함수는 전혀 선언할 돼 있습니다. 대신 포함 `winrt/base.h` (포함 되어 있는 자동으로 포함 하면 모든 C + + WinRT Windows 네임 스페이스 헤더 파일) 이러한 함수 선언을 포함 시킬 합니다.
- 에 대 한 합니다 [ **winrt::clock 구조체**](/uwp/cpp-ref-for-winrt/clock)를 **from_FILETIME/to_FILETIME** 위해 되지 **from_file_time/to_file_time**합니다.
- 필요로 하는 Api **IBuffer** 매개 변수는 간단 합니다. 충분 한 Api에 의존 하지만 대부분의 Api 원하는 컬렉션 또는 배열과 **IBuffer** 는 해당 하는 데 필요한 c + +에서 이러한 Api를 사용 하기 쉽습니다. 관련 된 데이터에 대 한 직접 액세스를 제공 하는이 업데이트는 **IBuffer** c + + 표준 라이브러리 컨테이너에서 사용 하는 동일한 데이터 명명 규칙을 사용 하 여 구현 합니다. 이 또한 일반적으로 대문자를 사용 하 여 시작 하는 메타 데이터 이름의 충돌을 방지 합니다.
- 코드 생성 향상: 코드 크기를 줄이기 위해 다양 한 향상 된 인라인을 높이고 팩터리 캐싱 최적화 합니다.
- 불필요 한 재귀를 제거 합니다. 경우 명령줄 참조 대신 특정 폴더에 `.winmd`서 `cppwinrt.exe` 도구는 더 이상에 대 한 재귀적으로 검색 `.winmd` 파일입니다. 합니다 `cppwinrt.exe` 도구 이제 중복 더 지능적으로 처리를 사용자 오류, 복원 력을 높입니다 고에 잘못 된 형식의 `.winmd` 파일입니다.
- 확정 된 스마트 포인터입니다. 이전에 때 철회 하지 못했습니다 이벤트 revokers 이동 할당 새 값입니다. 이 통해 스마트 포인터 클래스 자체 할당 되지 않은 안정적으로 처리 하는 위치는 문제를 파악 합니다. 기반으로 합니다 [ **winrt::com_ptr 구조체 템플릿을**](/uwp/cpp-ref-for-winrt/com-ptr)합니다. **winrt::com_ptr** 해결 되었는지 처리 고정 이벤트 revokers 의미 체계가 올바르게 나타나도록 이동 할당 시가 해지 합니다.

> [!IMPORTANT]
> 에 중요 한 변경 내용이 합니다 [C + + WinRT Visual Studio 확장 (VSIX)](https://aka.ms/cppwinrt/vsix), 1.0.181002.2, 버전에서 모두 버전 1.0.190128.4 나중입니다. 이러한 변경 내용과 기존 프로젝트의 경우에 미치는 영향에 대 한 자세한 내용은 [Visual Studio 지원 C + + /cli WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) 하 고 [VSIX 확장의 이전 버전](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)합니다.

## <a name="isolation-from-windows-sdk-header-files"></a>Windows SDK 헤더 파일에서 격리

이 잠재적으로 코드에 대 한 주요 변경 내용입니다.

해당 코드를 컴파일하려면를 C + + /cli WinRT 더 이상 Windows SDK의 헤더 파일에 따라 달라 집니다. 또한 C 런타임 라이브러리 (CRT) 및 c + + 표준 템플릿 라이브러리 (STL) 헤더 파일에 모든 Windows SDK 헤더 포함 되지 않습니다. 및 표준 준수를 개선 하는, 의도 하지 않은 종속성을 방지 하 고 으로부터 보호 해야 하는 매크로의 수가 상당히 줄어듭니다.

이 독립성 의미는 C + + /cli WinRT는 이제 자세한 이식 가능 및 표준을 준수 하 고이 포괄적인 크로스 컴파일러 및 플랫폼 간 라이브러리를 가능성입니다. 또한는 C + + /cli WinRT 헤더에 부정적인 영향을 받는 매크로 되지 않습니다.

이전에 생략 하면 C + + /cli WinRT 헤더를 포함할 모든 Windows 프로젝트에서 이제 직접 포함 해야 합니다. 이며, 어떤 경우 든, 항상 모범 사례에 종속 된 헤더를 명시적으로 포함 하 고 다른 라이브러리를 포함에 두지

현재 Windows SDK 헤더 파일 격리의 유일한 예외는 내장 함수 및 수치에 대 한 합니다. 이러한 마지막 남은 종속성 알려진된 문제가 있습니다.

프로젝트에서 해야 할 경우 Windows SDK 헤더와의 상호 운용성을 다시 활성화할 수 있습니다. COM 인터페이스를 구현 하려면 예를 들어, 할 수 있습니다 (에서 루 팅 [ **IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)). 이 예제에서는 포함 `unknwn.h` 모든 C + 포함 하기 전에 + WinRT 헤더입니다. 이렇게 하면 C + + /cli WinRT 기본 라이브러리 클래식 COM 인터페이스를 지원 하기 위해 다양 한 후크를 사용 하도록 설정 합니다. 코드 예제를 참조 하세요 [작성자 COM 구성 요소 C + + /cli WinRT](author-coclasses.md)합니다. 마찬가지로, 형식 및/또는 호출 하려는 함수를 선언 하는 다른 Windows SDK 헤더를 명시적으로 포함 합니다.

## <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>대상을 C + + /cli WinRT 프로젝트는 Windows SDK의 최신 버전으로

최소한의 컴파일러 및 링커 문제 발생 가능성이 있는 프로젝트 대상 다시 지정 하는 방법은 가장 손이 많이 이기도 합니다. 해당 방법은 새 프로젝트 (선택한 Windows SDK 버전 대상 지정)를 만들고 다음에서 파일 복사를 통해 새 프로젝트를 이전 합니다. 이전 섹션 됩니다 `.vcxproj` 및 `.vcxproj.filters` 할 수 있습니다 하는 파일 복사 개 저장 하는 Visual Studio에서 파일을 추가 합니다.

그러나 두 가지 다른 Visual Studio에서 프로젝트 대상을 다시 지정 합니다.

- 프로젝트 속성으로 이동 **일반** \> **Windows SDK 버전**를 선택 하 고 **모든 구성** 하 고 **모든 플랫폼**합니다. 설정할 **Windows SDK 버전** 대상 원하는 버전으로 합니다.
- **솔루션 탐색기**프로젝트 노드를 마우스 오른쪽 단추로 클릭, 클릭 **프로젝트 대상을**, 대상 및 클릭 하려는 버전을 선택 **확인**합니다.

이러한 두 메서드 중 하나를 사용 하 여 모든 컴파일러나 링커 오류가 발생할 경우 솔루션 정리를 시도할 수 있습니다 (**빌드합니다** > **솔루션 정리** 및/또는 모두를 수동으로 삭제 임시 폴더 및 파일)를 다시 빌드하기 전에 합니다.

C + + 컴파일러에서 생성 되는 경우 "*오류 C2039: 'IUnknown':의 구성원이 아닌 '\`전역 네임 스페이스 '*"를 추가한 `#include <unknwn.h>` 의 맨 위에 사용자 `pch.h` 파일 (모든 C + 포함 하기 전에 + WinRT 헤더).

추가 해야 `#include <hstring.h>` 그 후 합니다.

C + + 링커에서 생성 되는 경우 "*LNK2019 오류: 확인 되지 않은 외부 기호 _WINRT_CanUnloadNow@0 함수에서 참조 _VSDesignerCanUnloadNow@0* "를 추가 하 여 해결할 수 있습니다 `#define _VSDESIGNER_DONT_LOAD_AS_DLL` 하에 `pch.h` 파일입니다.
