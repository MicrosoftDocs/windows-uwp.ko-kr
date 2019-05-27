---
description: C++/WinRT에서 새롭거나 변경된 기능입니다.
title: 새로운 기능 C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c + +, cpp, winrt, 프로젝션, 뉴스 항목의 새,
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b84736e41e039d350a849c55fead008cbab5fdea
ms.sourcegitcommit: bc64db47b6ff326f15cac15fc2cfd709fa7f877b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/15/2019
ms.locfileid: "65626218"
---
# <a name="whats-new-in-cwinrt"></a>새로운 기능 C++/WinRT

## <a name="news-and-changes-in-cwinrt-20"></a>뉴스 및 변경 내용에서 C++WinRT 2.0

대 한 자세한 내용은 합니다 [ C++WinRT Visual Studio 확장 (VSIX)](https://aka.ms/cppwinrt/vsix)는 [Microsoft.Windows.CppWinRT NuGet 패키지](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/), 및를 `cppwinrt.exe` 도구&mdash;하는 방법을 비롯 하 여 획득 및 설치&mdash;참조 [Visual Studio 지원에 대 한 C++/WinRT, XAML, VSIX 확장 및 NuGet 패키지](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)합니다.

### <a name="changes-to-the-cwinrt-visual-studio-extension-vsix-for-version-20"></a>변경 된 C++WinRT 확장 VSIX (Visual Studio) 버전 2.0에 대 한

- 디버그 시각화 도우미는 이제 Visual Studio 2019; Visual Studio 2017을 지원 하기 위해 계속 뿐만 아니라 합니다.
- 수많은 버그 수정 내용이 적용 되었습니다.

### <a name="changes-to-the-microsoftwindowscppwinrt-nuget-package-for-version-20"></a>Microsoft.Windows.CppWinRT NuGet 패키지 버전 2.0에 대 한 변경

- `cppwinrt.exe` 도구 이제 Microsoft.Windows.CppWinRT NuGet 패키지에 포함 되었으며 도구는 필요에 따라 각 프로젝트에 대 한 플랫폼 프로젝션 헤더를 생성 합니다. 결과적으로 `cppwinrt.exe` 도구 더 이상 종속 Windows SDK (있지만, 여전히 도구 호환성을 위해 SDK와 함께 제공 됩니다).
- `cppwinrt.exe` 이제 각 플랫폼/구성 관련 중간 폴더 ($IntDir) 병렬 빌드를 사용 하도록 설정 하려면 아래 프로젝션 헤더를 생성 합니다.
- C++/WinRT 빌드 지원 (속성/대상이)는 이제 완벽 하 게 문서화 되어 수동으로 프로젝트 파일 사용자 지정 하려는 경우. 참조 [Microsoft.Windows.CppWinRT NuGet 패키지](https://github.com/Microsoft/xlang/blob/master/src/package/cppwinrt/nuget/readme.md)합니다.
- 수많은 버그 수정 내용이 적용 되었습니다.

### <a name="changes-to-cwinrt-for-version-20"></a>변경 C++버전 2.0에 대 한 /WinRT

#### <a name="open-source"></a>오픈 소스

합니다 `cppwinrt.exe` 도구는 Windows 런타임 메타 데이터를 가져오고 (`.winmd`) 파일 및 헤더 파일 기반 표준에서 생성 C++ 라이브러리는 *프로젝트* 메타 데이터에 설명 된 Api입니다. 이런 방식으로 이러한 Api를 사용할 수 있습니다 프로그램 C++/WinRT 코드입니다.

이 도구는 이제 완전히 오픈 소스 프로젝트, GitHub에서 사용할 수 있습니다. 방문 [Microsoft\/xlang](https://github.com/Microsoft/xlang)를 클릭 한 다음 및 **src** > **도구** > **cppwinrt**합니다.

#### <a name="xlang-libraries"></a>xlang 라이브러리

(에 대 한 Windows 런타임에서 사용 하는 ECMA-335 메타 데이터 형식 구문 분석) 완전히 이식 가능한 헤더 전용 라이브러리를 모든 Windows 런타임 및 앞으로 도구는 xlang의 기본을 형성 합니다. 주목할 만한 것도 빌드하며는 `cppwinrt.exe` xlang 라이브러리를 사용 하 여부터 도구입니다. 이렇게 하면 훨씬 더 정확 하 게 메타 데이터 쿼리를 위해 몇 가지 문제를 해결 합니다 C++/WinRT 언어 프로젝션 합니다.

#### <a name="fewer-dependencies"></a>더 적은 종속성

Xlang 메타 데이터 판독기 인해는 `cppwinrt.exe` 도구 자체에 더 적은 수의 종속성. 따라서 훨씬 유연한 더 많은 시나리오에서 사용할 수 없게 될 뿐만 아니라&mdash;특히 제한 된 작성 환경. 특히, 더 이상 의존 `RoMetadata.dll`합니다.
 
에 대 한 종속성을 이들은 `cppwinrt.exe` 2.0.
 
- api-ms-win-core-processenvironment-l1-1-0.dll
- api-ms-win-core-libraryloader-l1-2-0.dll
- XmlLite.dll
- api-ms-win-core-memory-l1-1-0.dll
- api-ms-win-core-handle-l1-1-0.dll
- api-ms-win-core-file-l1-1-0.dll
- SHLWAPI.dll
- ADVAPI32.dll
- KERNEL32.dll
- api-ms-win-core-rtlsupport-l1-1-0.dll
- api-ms-win-core-processthreads-l1-1-0.dll
- api-ms-win-core-heap-l1-1-0.dll
- api-ms-win-core-console-l1-1-0.dll
- api-ms-win-core-localization-l1-2-0.dll

이러한 종속성으로 인해 대비는 `cppwinrt.exe` 1.0에 있습니다.

- ADVAPI32.dll
- SHELL32.dll
- api-ms-win-core-file-l1-1-0.dll
- XmlLite.dll
- api-ms-win-core-libraryloader-l1-2-0.dll
- api-ms-win-core-processenvironment-l1-1-0.dll
- RoMetadata.dll
- SHLWAPI.dll
- KERNEL32.dll
- api-ms-win-core-rtlsupport-l1-1-0.dll
- api-ms-win-core-heap-l1-1-0.dll
- api-ms-win-core-timezone-l1-1-0.dll
- api-ms-win-core-console-l1-1-0.dll
- api-ms-win-core-localization-l1-2-0.dll
- OLEAUT32.dll
- api-ms-win-core-winrt-error-l1-1-0.dll
- api-ms-win-core-winrt-error-l1-1-1.dll
- api-ms-win-core-winrt-l1-1-0.dll
- api-ms-win-core-winrt-string-l1-1-0.dll
- api-ms-win-core-synch-l1-1-0.dll
- api-ms-win-core-threadpool-l1-2-0.dll
- api-ms-win-core-com-l1-1-0.dll
- api-ms-win-core-com-l1-1-1.dll
- api-ms-win-core-synch-l1-2-0.dll 

#### <a name="the-windows-runtime-noexcept-attribute"></a>Windows 런타임 `noexcept` 특성

Windows 런타임에 있는 새 `[noexcept]` 프로그램의 메서드와 속성을 데코 레이트 하는 데 사용할 수 있는 특성인 [MIDL 3.0](/uwp/midl-3/predefined-attributes)합니다. 구현에서 예외를 throw 하지는 도구를 지원 하기 위해 특성의 존재를 나타냅니다 (실패 한 반환도 HRESULT)입니다. 따라서 잠재적으로 실패할 수 있는 응용 프로그램 이진 인터페이스 (ABI) 호출을 지원 하는 데 필요한 예외 처리 오버 헤드를 방지 하 여 코드 생성을 최적화 하기 위해 언어 프로젝션 합니다.

C++/ WinRT 활용이 생성 하 여 C++ `noexcept` 구현의 사용 및 코드를 작성 합니다. API 메서드 또는 실패를 확보 하는 속성이 코드 크기에 대해 염려 하는 경우이 특성을 조사할 수 있습니다.

#### <a name="optimized-code-generation"></a>최적화 된 코드 생성

C++/ WinRT 이제 훨씬 더 효율적으로 생성 C++ 원본 (백그라운드) 되도록 코드를 C++ 컴파일러에서 가장 작은 가장 효율적인 이진 코드를 가능한를 생성할 수 있습니다. 예외 처리의 비용을 절감 하는 데 맞게는 다양 한 향상 된 기능 불필요 한 방지 하 여 정보를 해제 합니다. 많은 양의 사용 하는 이진 파일 C++/WinRT 코드 약 코드 크기가 4% 감소가 표시 됩니다. 코드를 더 효율적 (더 빠르게 실행 됩니다) 이기도 축소 된 명령 수입니다.

이러한 향상 된이 기능 에서도 사용할 수 있는 새 interop 기능을 사용 합니다. 모든는 C++/이제 리소스 소유자는 WinRT 형식 방지 이전 2 단계 접근 방식에 대 한 소유권을 직접 생성자를 포함 합니다.

```cppwinrt
ABI::Windows::Foundation::IStringable* raw = ...

IStringable projected(raw, take_ownership_from_abi);

printf("%ls\n", projected.ToString().c_str());
```

#### <a name="optimized-exception-handling-eh-code-generation"></a>최적화 된 예외 처리 (EH) 코드 생성

이 변경 작업을 Microsoft에 의해 실행 되었는가 보완 C++ 예외 처리의 비용을 줄이기 위해 최적화 프로그램은 팀입니다. 코드에서 응용 프로그램 이진 인터페이스 (Abi) (예: COM)을 많이 사용 하는 경우이 패턴에 따라 코드의 많은 확인할 수 있습니다.

```cpp
int32_t Function() noexcept
{
    try
    {
        // code here constitutes unique value.
    }
    catch (...)
    {
        // code here is always duplicated.
    }
}
```

C++/ 자체 WinRT 구현 되는 모든 API에 대 한이 패턴을 생성 합니다. 수천 개의 API 함수를 사용 하 여 모든 최적화 중요할 수 있습니다. 과거에는 최적화 프로그램은 해당 catch 블록 감지 하지 않습니다. 모두 동일한 되므로 많은 (에 시스템 코드에서 예외를 사용 하 여 큰 이진 파일을 생성 하는 스레드도 포함)이 표시 되는 각 ABI 관련 코드를 복제 되었습니다. 그러나, Visual Studio 2019에서에서는 C++ 컴파일러 모든 접기 funclets, catch 및 고유 수 있는 저장 합니다. 결과이 패턴에 크게 의존 하는 이진 파일에 대 한 코드 크기를 추가 하 여 전체 18% 감소 합니다. EH 코드 이제 반환 코드를 사용 하 여 보다 효율적 뿐만 아니라 큰 이진 파일에 대 한 우려가도 이제는 과거의 방식입니다.

#### <a name="incremental-build-improvements"></a>증분 빌드 기능 향상

`cppwinrt.exe` 도구는 이제 디스크에 기존 파일의 콘텐츠에 대해 생성 된 헤더/소스 파일의 출력을 비교 하 고 파일이 실제로 변경 된 경우만 파일 기록 합니다. 파일 "더티"으로 간주 되지 않습니다 보장 및 디스크 I/O 사용 하 여 상당한 시간을 저장 하는이 C++ 컴파일러. 결과 다시 컴파일을 발생 하지 않으며 되었거나 대부분의 감소입니다.

#### <a name="generic-interfaces-are-now-all-generated"></a>제네릭 인터페이스는 이제 생성 된 모든

Xlang 메타 데이터 판독기로 인해 C++/WinRT 메타 데이터에서 이제 모든 매개 변수가 있는 경우, 또는 제네릭 인터페이스를 생성 합니다. 와 같은 인터페이스 [Windows::Foundation::Collections::IVector\<T\> ](/uwp/api/windows.foundation.collections.ivector_t_) 는 이제 메타 데이터에서 생성 되지 않고 필기 `winrt/base.h`합니다. 결과 크기 `winrt/base.h` 절반으로 줄어들었습니다 (이었던 직접 롤백 방식 어려울) 코드를 마우스 오른쪽 단추로 최적화 생성 됩니다.

> [!IMPORTANT]
> 제공 된 예제와 같은 인터페이스에는 이제의 헤더에는 해당 네임 스페이스를 대신 표시 `winrt/base.h`합니다. 따라서 아직 수행 하는 경우에 인터페이스를 사용 하려면 적절 한 네임 스페이스 헤더를 포함 해야 합니다.

#### <a name="component-optimizations"></a>구성 최적화

이 업데이트는 몇 가지 추가 옵트인에 대 한 최적화에 대 한 지원 추가 C++/WinRT를 아래 섹션에서 설명 합니다. 켜도록 명시적으로 사용 해야 이러한 최적화는 주요 변경 사항 (지원 하기 위해 약간 변경 해야 할 수 있습니다)는 변경 내용 때문에 합니다 `cppwinrt.exe` 도구의 `-opt` 플래그입니다.

새 프로젝트 (프로젝트 템플릿에서)를 사용할지 `-opt` 기본적으로 합니다.

##### <a name="uniform-construction-and-direct-implementation-access"></a>Uniform 생성과 직접 구현은 액세스

프로젝션 된 형식을 사용 하는 경우에 자체 구현 형식에 구성 요소 직접 액세스를 허용 하는 이러한 두 가지 최적화 합니다. 사용 하지 않아도 됩니다 [ **확인**](/uwp/cpp-ref-for-winrt/make)에 [ **make_self**](/uwp/cpp-ref-for-winrt/make-self), 또는 [ **get_self** ](/uwp/cpp-ref-for-winrt/get-self) 공용 API 화면을 사용 하려는 경우. 구현에 대 한 직접 호출까지 호출을 컴파일되지 및 것도 않을 전적으로 인라인 처리 합니다.

##### <a name="type-erased-factories"></a>팩터리 형식이 지워진

이 최적화를 방지를 #include에서 종속성 `module.g.cpp` 없습니다 다시 필요한 변경에 발생 하는 단일 구현을 클래스일 때마다 있도록 합니다. 결과 향상 된 빌드 성능입니다.

#### <a name="smarter-and-more-efficient-modulegcpp-for-large-projects-with-multiple-libs"></a>더 스마트 하 고 더 효율적인 `module.g.cpp` 여러 라이브러리를 사용 하 여 대규모 프로젝트

합니다 `module.g.cpp` 파일에 현재 포함 되어 두 가지 추가 구성 가능한 도우미 라는 **winrt_can_unload_now**, 및 **winrt_get_activation_factory**합니다. 이러한 다양 한 라이브러리, 고유한 런타임 클래스를 사용 하 여 각 DLL은 이루어진 대규모 프로젝트에 대 한 설계 되었습니다. 이 경우 수동으로 함께 붙이기 위한 DLL의 해야 **DllGetActivationFactory** 하 고 **DllCanUnloadNow**합니다. 이러한 도우미 훨씬 쉽게는 의사 학자 금 보조 발생 오류를 방지 하 여 수행할 수 있습니다. 합니다 `cppwinrt.exe` 도구의 `-lib` 플래그 자체 머리말 각 개별 lib를 제공 하기 위해 사용할 수도 있습니다 (대신 `winrt_xxx`) 각 라이브러리의 함수를 개별적으로 라는 하 고 따라서 명확 하 게 결합 될 수 있습니다.

#### <a name="new-winrtcoroutineh-header"></a>새 `winrt/coroutine.h` 헤더

합니다 `winrt/coroutine.h` 헤더는 모두를 위한 새 홈 C++/WinRT의 코 루틴 지원 합니다. 이 지원은 내 몇 개의 위치에 상주 하는 이전에 생각 했던는 너무 제한 되었습니다. 이제 상주 직접 작성 하는 것이 아니라 이제 Windows 런타임 비동기 인터페이스를 생성 하므로 `winrt/Windows.Foundation.h`합니다. 코 루틴 외에도 유지 가능 하 고 지원 되 고, 의미와 같은 도우미 [ **resume_foreground** ](/uwp/cpp-ref-for-winrt/resume-foreground) 특정 네임 스페이스 헤더의 끝 추가 될 필요가 없어졌습니다. 대신, 해당 종속성 보다 자연스럽 게 포함할 수 있습니다. 이렇게 추가 하면 **resume_foreground** 뿐만 아니라에서 다시 시작을 지원 하기 위해를 지정 [ **Windows::UI::Core::CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher), 하지만 수 이제도 지원에서 다시 시작을 지정 [ **Windows::System::DispatcherQueue**](/uwp/api/windows.system.dispatcherqueue)합니다. 이전에 하나만 지원 될 수 있습니다. 두 정의 네임 스페이스가 하나에 상주할 수 있으므로 합니다.

예로 **DispatcherQueue** 지원 합니다.

```cppwinrt
fire_and_forget Async(DispatcherQueueController controller)
{
    bool queued = co_await resume_foreground(controller.DispatcherQueue());
    assert(queued);

    // This is just to simulate queue failure...
    co_await controller.ShutdownQueueAsync();

    queued = co_await resume_foreground(controller.DispatcherQueue());
    assert(!queued);
}
```

코 루틴 도우미도로 데코레이팅 되었습니다 이제 `[[nodiscard]]`시켜 해당 유용성을 개선 합니다. 경우에 (잊어버리거나 알지 못합니다 해야) `co_await` 작동 하려면 다음을로 인해 하 `[[nodiscard]]`, 이러한 실수 이제 컴파일러에서 경고를 생성 합니다.

#### <a name="help-with-diagnosing-stack-allocations"></a>스택 할당 진단에 도움

이기 때문에 예상 및 구현 클래스는 기본적으로 동일 이름과 다른 네임 스페이스를 사용 하는 대신 다른 항목에 대 한 실수로 하 고 실수로 스택에 구현을 만드는 [ **수행** ](/uwp/cpp-ref-for-winrt/make) 도우미 제품군입니다. 이 개체는 처리 중인 참조가 여전히 진행에서 중일 소멸 될 수 있습니다 경우에 따라 진단 하기 어려울 수 있습니다. 이제 어설션을 디버그 빌드에 대해이를 선택 합니다. 어설션이 스택 할당 코 루틴 내에서 검색 하지 않습니다, 대부분의 오류를 catch 하는 데 도움이 됩니다 그럼에도 불구 하 고 합니다.

#### <a name="improved-capture-helpers-and-variadic-delegates"></a>향상 된 캡처 도우미 및 variadic 대리자

이 업데이트 프로젝션 된 형식으로 지원 하 여 캡처 도우미의 한계를 해결 합니다. 이 제공 됩니다 고 Windows Runtime interop Api를 사용 하 여 프로젝션된 된 형식을 반환 하는 경우.

이 업데이트에 대 한 지원 추가 [ **get_strong** ](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) 하 고 [ **get_weak** ](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) variadic (비-Windows 런타임) 대리자를 만들 때.

#### <a name="support-for-deferred-destruction-and-safe-qi-during-destruction"></a>지연 된 소멸과 안전 하 게 QI 소멸 중에 대 한 지원

XAML 응용 프로그램을 자체 수행 하는 데 필요로 인해 어려움에 액세스할 수는 [ **QueryInterface** ](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) (QI) 계층 위아래로 일부 정리 구현을 호출 하기 위해 소멸자가 있습니다. 그러나 0에 도달 하면 개체의 참조 횟수가 이미 후 호출을 QI는 포함 됩니다. 이 업데이트는 debouncing이 부활 없습니다 수; 0에 도달 하면 확인 하 고 참조 횟수에 대 한 지원을 추가 합니다. 소멸 하는 동안 필요한 임시 수 QI 있습니다. 이 절차는 특정 XAML 응용 프로그램/컨트롤을 피할 수 없습니다 하 고 C++/WinRT에 복원 되었습니다.

소멸 정적 함으로써 지연 될 수 있습니다 **final_release** 함수 및 소유권 이동 합니다 **unique_ptr** 다른 컨텍스트를 합니다.

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    hstring ToString()
    {
        return L"Sample";
    }

    ~Sample()
    {
        // Called when the unique_ptr below is reset.
    }

    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        // Move 'ptr' as needed to delay destruction.
    }
};
```

아래 예제에서는 한 번에에서는 **MainPage** (마지막 시간)를 위해 해제 됩니다 **final_release** 라고 합니다. 함수는 5 초 대기 (스레드 풀의 경우)를 소비 하 고 다음 다시 시작 하기 페이지를 사용 하 **디스패처** (QI/AddRef/릴리스 작업 필요). 다음 지웁니다를 **unique_ptr**, 있어를 **MainPage** 소멸자는 실제로 호출 합니다. 도 여기서 **DataContext** 가 호출에 대 한 QI 필요 **IFrameworkElement**합니다. 당연히 구현할 필요가 없습니다 하 **final_release** 코 루틴으로 합니다. 작동 하는 있고는 매우 간단 하 게 소멸 다른 스레드로 이동 합니다.

```cppwinrt
struct MainPage : PageT<MainPage>
{
    MainPage()
    {
    }

    ~MainPage()
    {
        DataContext(nullptr);
    }

    static IAsyncAction final_release(std::unique_ptr<MainPage> ptr)
    {
        co_await 5s;

        co_await resume_foreground(ptr->Dispatcher());

        ptr = nullptr;
    }
};
```

#### <a name="improved-support-for-com-style-single-interface-inheritance"></a>COM 스타일 단일 인터페이스 상속에 대 한 지원 향상된

및 Windows 런타임 프로그래밍 C++/WinRT는 또한 작성 하 고 COM 전용 Api를 사용 하는 데 사용 됩니다. 이 업데이트를 사용 하면 인터페이스 계층 구조는 있는 COM 서버를 구현할 수 있습니다. Windows 런타임;에서는 필요 하지 않습니다. 하지만 일부 COM 구현에 필요 합니다.

#### <a name="correct-handling-of-out-params"></a>올바른 처리 `out` 매개 변수

사용 하기 어려울 수 있습니다 `out` params; 특히 Windows 런타임 배열입니다. 이 업데이트를 사용 하 여 C++WinRT는 훨씬 더 강력 하 고 실수에 복원 력이 하더라도 `out` 매개 변수 및 배열; COM 개발자는 대상 및 원시 ABI를 사용 하는 사람 또는 언어 프로젝션을 통해 이러한 매개 변수 도착 하는지 여부를 변수를 일관 되 게 초기화 하지 않아 실수를 합니다. 두 경우 모두 C++/ (기억 하 여 리소스를 해제할 수), 프로젝션 된 형식 ABI를 전달 하는 데 있어 시간과 영구 또는 abi 전반에서 도착 하는 매개 변수를 정리 하는 데 있어 WinRT 올바른 작업을 지금 수행 합니다.

#### <a name="events-now-handle-invalid-tokens-reliably"></a>이벤트 이제 토큰을 처리할 잘못 된 안정적으로

[ **winrt::event** ](/uwp/cpp-ref-for-winrt/event) 구현은 이제 정상적으로 처리 하는 경우 여기서 해당 **제거** 토큰 값이 잘못 된 메서드는 (에서 존재 하지 않는 값을 배열)입니다.

#### <a name="coroutine-locals-are-now-destroyed-before-the-coroutine-returns"></a>코 루틴의 지역 변수는 코 루틴에서 반환 되기 전에 제거 이제 됩니다.

코 루틴 형식을 구현 하는 기존의 방법은 지역 내는 코 루틴에서 제거할 수 있습니다 *후* 는 코 루틴 일 반환/완료 (대신 최종 일시 중단 하기 전에). 기타 혜택을 계산 하 고이 문제를 방지 하기 위해 모든 대기자 다시 시작은 이제 최종 일시 중단 될 때까지 지연 됩니다.

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>뉴스 및 변경 내용을 Windows sdk 버전 10.0.17763.0 (Windows 10, 버전 1809)

아래 표에서 뉴스를 포함 하 고 변경에 대 한 C++Windows sdk 버전 10.0.17763.0 /WinRT (Windows 10, 버전 1809).

| 새롭거나 변경 된 기능 | 추가 정보 |
| - | - |
| **주요 변경 내용**합니다. 를 컴파일할 수 것에 대 한 C++/WinRT Windows SDK의 헤더에 종속 되지 않습니다. | 참조 [Windows SDK 헤더 파일에서 격리](#isolation-from-windows-sdk-header-files)아래. |
| Visual Studio 프로젝트 시스템 형식이 변경 되었습니다. | 참조 [대상을 다시 지정 하는 방법에 C++Windows SDK의 최신 버전으로 /WinRT 프로젝트](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)아래. |
| 새로운 함수 및 기본 클래스 컬렉션 개체를 Windows 런타임 함수를 전달할 수 있도록 하거나 고유한 컬렉션 속성 및 컬렉션 형식 구현 됩니다. | 참조 [컬렉션과 C++/WinRT](collections.md)합니다. |
| 사용할 수는 [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) 사용 하 여 태그 확장에 C++/WinRT 런타임 클래스입니다. | 자세한 내용은 및 코드 예제를 참조 하세요 [데이터 바인딩 개요](/windows/uwp/data-binding/data-binding-quickstart)합니다. |
| 코 루틴을 취소 하는 것에 대 한 지원을 사용 하면 취소 콜백을 등록할 수 있습니다. | 자세한 내용은 및 코드 예제를 참조 하세요 [비동기 작업을 취소 콜백을 취소](concurrency.md#canceling-an-asychronous-operation-and-cancellation-callbacks)합니다. |
| 강력한 또는 약한 참조를 현재 개체는 멤버 함수를 가리키는 대리자를 만들 때 설정할 수 있습니다 (대신 원시 *이* 포인터) 처리기 등록 되어 있는 시점입니다. | 자세한 정보 및 코드 예제를 참조 합니다 **대리자로 멤버 함수를 사용 하는 경우** 섹션의 하위 섹션 [안전 하 게 액세스를 *이* 이벤트처리대리자를사용하여포인터](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate). |
| Visual Studio의 향상 된 규칙에 의해 처리 된 않는 버그가 수정 되는 C++ 표준입니다. LLVM 및 Clang 도구 체인은 뛰어납니다의 유효성을 검사 하는 데 C++/WinRT의 표준 준수 합니다. | 에 설명 된 문제를 더 이상 접할 [내 새 프로젝트 컴파일되지 이유? Visual Studio 2017을 사용 하 고 (15.8.0 버전 이상), 및 SDK 버전 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

다른 변경 됩니다.

- **주요 변경 내용**합니다. [**winrt::get_abi(winrt::hstring const&)** ](/uwp/cpp-ref-for-winrt/get-abi) 이제 반환 `void*` 대신 `HSTRING`합니다. 사용할 수 있습니다 `static_cast<HSTRING>(get_abi(my_hstring));` HSTRING를 가져오려고 합니다.
- **주요 변경 내용**합니다. [**winrt::put_abi(winrt::hstring&)** ](/uwp/cpp-ref-for-winrt/put-abi) 이제 반환 `void**` 대신 `HSTRING*`합니다. 사용할 수 있습니다 `reinterpret_cast<HSTRING*>(put_abi(my_hstring));` HSTRING * 가져오려고 합니다.
- **주요 변경 내용**합니다. HRESULT로 투영 되 **winrt::hresult**합니다. HRESULT (형식 검사를 수행 하거나 형식 특성을 지원 하기 위해), 필요한 경우 있습니다 `static_cast` 는 **winrt::hresult**합니다. 그렇지 않으면 **winrt::hresult** HRESULT를 포함 하는 만큼 변환 `unknwn.h` 모든 C + 포함 하기 전에 + WinRT 헤더입니다.
- **주요 변경 내용**합니다. GUID 처럼 이제 투영 됩니다 **winrt::guid**합니다. 구현 하는 Api를 사용 해야 합니다 **winrt::guid** GUID 매개 변수에 대 한 합니다. 이 고, 그렇지 **winrt::guid** 으로 포함 하는 GUID 변환 `unknwn.h` 하나를 포함 하기 전에 C++/WinRT 헤더입니다.
- **주요 변경 내용**합니다. 합니다 [ **winrt::handle_type 생성자** ](/uwp/cpp-ref-for-winrt/handle-type#handle_typehandle_type-constructor) (이제 하기가 어렵다는 것을 사용 하 여 잘못 된 코드를 작성) 명시적 만들어 강화 되었습니다. 원시 핸들 값을 할당 해야 할 경우 호출 된 [ **handle_type::attach 함수** ](/uwp/cpp-ref-for-winrt/handle-type#handle_typeattach-function) 대신 합니다.
- **주요 변경 내용**합니다. 서명을 **WINRT_CanUnloadNow** 하 고 **WINRT_GetActivationFactory** 변경 되었습니다. 이러한 함수는 전혀 선언할 돼 있습니다. 대신 포함 `winrt/base.h` (하나를 포함 하는 경우 자동으로 포함 되어 C++WinRT Windows 네임 스페이스 헤더 파일) 이러한 함수 선언을 포함 시킬 합니다.
- 에 대 한 합니다 [ **winrt::clock 구조체**](/uwp/cpp-ref-for-winrt/clock)를 **from_FILETIME/to_FILETIME** 위해 되지 **from_file_time/to_file_time**합니다.
- 필요로 하는 Api **IBuffer** 매개 변수는 간단 합니다. 충분 한 Api에 의존 하지만 대부분의 Api 원하는 컬렉션 또는 배열과 **IBuffer** 에서 이러한 Api를 사용 하기 쉽도록 필요 하다는 C++합니다. 뒤의 데이터에 대 한 직접 액세스를 제공 하는이 업데이트는 **IBuffer** 사용 하는 동일한 데이터 명명 규칙을 사용 하 여 구현 합니다 C++ 표준 라이브러리 컨테이너입니다. 이 또한 일반적으로 대문자를 사용 하 여 시작 하는 메타 데이터 이름의 충돌을 방지 합니다.
- 코드 생성 향상: 코드 크기를 줄이기 위해 다양 한 향상 된 인라인을 높이고 팩터리 캐싱 최적화 합니다.
- 불필요 한 재귀를 제거 합니다. 경우 명령줄 참조 대신 특정 폴더에 `.winmd`서 `cppwinrt.exe` 도구는 더 이상에 대 한 재귀적으로 검색 `.winmd` 파일입니다. 합니다 `cppwinrt.exe` 도구 이제 중복 더 지능적으로 처리를 사용자 오류, 복원 력을 높입니다 고에 잘못 된 형식의 `.winmd` 파일입니다.
- 확정 된 스마트 포인터입니다. 이전에 때 철회 하지 못했습니다 이벤트 revokers 이동 할당 새 값입니다. 이 통해 스마트 포인터 클래스 자체 할당 되지 않은 안정적으로 처리 하는 위치는 문제를 파악 합니다. 기반으로 합니다 [ **winrt::com_ptr 구조체 템플릿을**](/uwp/cpp-ref-for-winrt/com-ptr)합니다. **winrt::com_ptr** 해결 되었는지 처리 고정 이벤트 revokers 의미 체계가 올바르게 나타나도록 이동 할당 시가 해지 합니다.

> [!IMPORTANT]
> 에 중요 한 변경 내용이 합니다 [ C++WinRT Visual Studio 확장 (VSIX)](https://aka.ms/cppwinrt/vsix), 1.0.181002.2, 버전에서 모두 버전 1.0.190128.4 나중입니다. 이러한 변경 내용과 기존 프로젝트의 경우에 미치는 영향에 대 한 자세한 내용은 [Visual Studio 지원에 대 한 C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) 하 고 [VSIX 확장의 이전 버전](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)합니다.

### <a name="isolation-from-windows-sdk-header-files"></a>Windows SDK 헤더 파일에서 격리

이 잠재적으로 코드에 대 한 주요 변경 내용입니다.

를 컴파일할 수 것에 대 한 C++/WinRT 더 이상 Windows SDK의 헤더 파일에 따라 달라 집니다. C 런타임 라이브러리 (CRT) 헤더 파일 및 C++ 표준 템플릿 라이브러리 (STL)도 포함 되지 않습니다 Windows SDK 헤더입니다. 및 표준 준수를 개선 하는, 의도 하지 않은 종속성을 방지 하 고 으로부터 보호 해야 하는 매크로의 수가 상당히 줄어듭니다.

즉이 독립성 C++/WinRT는 이제 자세한 이식 가능 및 표준 준수 및이 포괄적인 크로스 컴파일러 및 플랫폼 간 라이브러리를 가능성입니다. 의미 하기도 합니다 C++/WinRT 헤더에 부정적인 영향을 받는 매크로 되지 않습니다.

이전에 유지 되도록 하는 경우 C++/WinRT 헤더를 포함할 모든 Windows 프로젝트에서 이제 직접 포함 해야 합니다. 이며, 어떤 경우 든, 항상 모범 사례에 종속 된 헤더를 명시적으로 포함 하 고 다른 라이브러리를 포함에 두지

현재 Windows SDK 헤더 파일 격리의 유일한 예외는 내장 함수 및 수치에 대 한 합니다. 이러한 마지막 남은 종속성 알려진된 문제가 있습니다.

프로젝트에서 해야 할 경우 Windows SDK 헤더와의 상호 운용성을 다시 활성화할 수 있습니다. COM 인터페이스를 구현 하려면 예를 들어, 할 수 있습니다 (에서 루 팅 [ **IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)). 이 예제에서는 포함 `unknwn.h` 모든 C + 포함 하기 전에 + WinRT 헤더입니다. 이렇게 하면은 C++/WinRT 기본 라이브러리 클래식 COM 인터페이스를 지원 하기 위해 다양 한 후크를 사용 하도록 설정 합니다. 코드 예제를 참조 하세요 [사용 하 여 만든 COM 구성 요소 C++/WinRT](author-coclasses.md)합니다. 마찬가지로, 형식 및/또는 호출 하려는 함수를 선언 하는 다른 Windows SDK 헤더를 명시적으로 포함 합니다.

### <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>대상 다시 지정 하는 방법에 C++Windows SDK의 최신 버전으로 /WinRT 프로젝트

최소한의 컴파일러 및 링커 문제 발생 가능성이 있는 프로젝트 대상 다시 지정 하는 방법은 가장 손이 많이 이기도 합니다. 해당 방법은 새 프로젝트 (선택한 Windows SDK 버전 대상 지정)를 만들고 다음에서 파일 복사를 통해 새 프로젝트를 이전 합니다. 이전 섹션 됩니다 `.vcxproj` 및 `.vcxproj.filters` 할 수 있습니다 하는 파일 복사 개 저장 하는 Visual Studio에서 파일을 추가 합니다.

그러나 두 가지 다른 Visual Studio에서 프로젝트 대상을 다시 지정 합니다.

- 프로젝트 속성으로 이동 **일반** \> **Windows SDK 버전**를 선택 하 고 **모든 구성** 하 고 **모든 플랫폼**합니다. 설정할 **Windows SDK 버전** 대상 원하는 버전으로 합니다.
- **솔루션 탐색기**프로젝트 노드를 마우스 오른쪽 단추로 클릭, 클릭 **프로젝트 대상을**, 대상 및 클릭 하려는 버전을 선택 **확인**합니다.

이러한 두 메서드 중 하나를 사용 하 여 모든 컴파일러나 링커 오류가 발생할 경우 솔루션 정리를 시도할 수 있습니다 (**빌드합니다** > **솔루션 정리** 및/또는 모두를 수동으로 삭제 임시 폴더 및 파일)를 다시 빌드하기 전에 합니다.

경우는 C++ 컴파일러 생성 "*오류 C2039: 'IUnknown':의 구성원이 아닌 '\`전역 네임 스페이스 '*"를 추가한 `#include <unknwn.h>` 의 맨 위에 사용자 `pch.h` 파일 (모든 C + 포함 하기 전에 + WinRT 헤더).

추가 해야 `#include <hstring.h>` 그 후 합니다.

경우는 C++ 링커 생성 "*LNK2019 오류: 확인 되지 않은 외부 기호 _WINRT_CanUnloadNow@0 함수에서 참조 _VSDesignerCanUnloadNow@0* "를 추가 하 여 해결할 수 있습니다 `#define _VSDESIGNER_DONT_LOAD_AS_DLL` 에 하 `pch.h` 파일입니다.
