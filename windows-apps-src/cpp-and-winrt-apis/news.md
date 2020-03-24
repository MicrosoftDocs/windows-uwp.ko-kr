---
description: C++/WinRT에서 새롭거나 변경된 기능입니다.
title: C++/WinRT의 새로운 기능
ms.date: 03/16/2020
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 새로운 기능
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 734544a1294c6a97e70afcbf7ce6b5efc13cf841
ms.sourcegitcommit: eb24481869d19704dd7bcf34e5d9f6a9be912670
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/17/2020
ms.locfileid: "79448589"
---
# <a name="whats-new-in-cwinrt"></a>C++/WinRT의 새로운 기능

이후 버전의 C++/WinRT가 출시됨에 따라 이 항목에서 새롭거나 변경된 기능에 대해 설명합니다.

## <a name="rollup-of-recent-improvementsadditions-as-of-march-2020"></a>최근 개선 사항/추가 사항 롤업(2020년 3월 기준)

### <a name="up-to-23-shorter-build-times"></a>빌드 시간 최대 23% 단축

C++/WinRT 및 C++ 컴파일러 팀이 협력하여 빌드 시간을 단축하기 위해 최선을 다했습니다. C++ 컴파일러가 컴파일 시 오버헤드를 제거하는 데 도움이 되도록 C++/WinRT의 내부 요소를 재구성하는 방법과 C++/WinRT 라이브러리를 처리하도록 C++ 컴파일러 자체를 개선하는 방법을 알아내기 위해 컴파일러 분석 자료를 꼼꼼히 검토했습니다. C++/WinRT는 컴파일러에 최적화되었고, 컴파일러는 C++/WinRT에 최적화되었습니다.

모든 단일 C++/WinRT 프로젝션 네임스페이스 헤더를 포함하는 미리 컴파일된 헤더(PCH)를 빌드하는 최악의 시나리오를 예로 들어 보겠습니다.

| Version | PCH 크기(바이트) | 시간(초) |
| - | - | - |
| 7월 C++/WinRT(Visual C++ 16.3 사용) | 3,004,104,632 | 31 |
| C++/WinRT 버전 2.0.200316.3(Visual C++ 16.5 사용) | 2,393,515,336 | 24 |

크기 20% 감소 및 빌드 시간 23% 단축.

### <a name="improved-msbuild-support"></a>향상된 MSBuild 지원

다양한 시나리오에 대한 [MSBuild](/visualstudio/msbuild/msbuild?view=vs-2019) 지원을 개선하기 위해 많은 노력을 기울였습니다.

### <a name="even-faster-factory-caching"></a>훨씬 더 빠른 팩터리 캐싱

더 나은 인라인 실행 부하 과다 경로를 통해 실행 속도를 높이기 위해 팩터리 캐시의 인라인 처리를 개선했습니다.

아래 [최적화된 EH 코드 생성](#optimized-exception-handling-eh-code-generation)에 설명된 것처럼, 이러한 개선 사항은 코드 크기에 영향을 주지 않습니다. 애플리케이션이 C++ 예외 처리를 많이 사용한다면 `/d2FH4` 옵션을 사용하여 이진을 축소할 수 있습니다. Visual Studio 2019 16.3 이상을 사용하여 생성된 새 프로젝트에는 이 옵션이 기본적으로 설정되어 있습니다.

### <a name="more-efficient-boxing"></a>보다 효율적인 boxing

이제 XAML 애플리케이션에서 [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value)를 더 효율적으로 사용할 수 있습니다([boxing 및 unboxing](/windows/uwp/cpp-and-winrt-apis/boxing) 참조). boxing을 많이 수행하는 애플리케이션은 코드 크기도 줄어듭니다.

### <a name="support-for-implementing-com-interfaces-that-implement-iinspectable"></a>IInspectable을 구현하는 COM 인터페이스 구현 지원

[**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable)을 구현하는 Windows 런타임 이외 COM 인터페이스를 구현해야 하는 경우 이제 C++/WinRT로 구현할 수 있습니다. [IInspectable을 구현하는 COM 인터페이스](https://github.com/microsoft/xlang/pull/603)를 참조하세요.

### <a name="module-locking-improvements"></a>모듈 잠금 기능 향상

이제 모듈 잠금에 대한 제어를 통해 사용자 지정 호스팅 시나리오와 모듈 수준 잠금 제거를 모두 수행할 수 있습니다. [모듈 잠금 기능 향상](https://github.com/microsoft/xlang/pull/583)을 참조하세요.

### <a name="support-for-non-windows-runtime-error-information"></a>Windows 런타임 이외 오류 정보에 대한 지원

일부 API(일부 Windows 런타임 API)는 Windows 런타임 오류 발생 API를 사용하지 않고 오류를 보고합니다. 이와 같은 경우 C++/WinRT는 이제 COM 오류 정보를 사용하는 방식으로 돌아갑니다. [WinRT 이외 오류 정보에 대한 C++/WinRT 지원](https://github.com/microsoft/xlang/pull/582)을 참조하세요.

### <a name="enable-c-module-support"></a>C++ 모듈 지원 사용 

C++모듈 지원을 다시 사용할 수 있지만 실험적 형태로만 지원됩니다. 이 기능은 아직 C++ 컴파일러에서 완성되지 않았습니다.

### <a name="more-efficient-coroutine-resumption"></a>더 효율적인 코루틴 재개

C++/WinRT 코루틴은 이미 잘 작동하지만 이를 개선하는 방법을 계속 찾고 있습니다. [코루틴 재개 확장성 개선](https://github.com/microsoft/xlang/pull/546)을 참조하세요.

### <a name="new-when_all-and-when_any-async-helpers"></a>새로운 **when_all** 및 **when_any** 비동기 도우미

**when_all** 도우미 함수는 제공된 대기 가능 요소가 모두 완료되어야 완료되는 [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) 개체를 만듭니다. **when_any** 도우미 함수는 제공된 대기 가능 요소가 하나라도 완료되어야 완료되는 **IAsyncAction** 개체를 만듭니다. 

[when_any 비동기 도우미 추가](https://github.com/microsoft/xlang/pull/520) 및 [when_all 비동기 도우미 추가](https://github.com/microsoft/xlang/pull/516)를 참조하세요.

### <a name="other-optimizations-and-additions"></a>그 외 최적화 기능 및 추가 기능

또한 디버깅을 간소화하고 내부 요소 및 기본 구현을 최적화하는 다양한 향상 기능을 포함하여 많은 버그 수정과 사소한 최적화 및 추가 기능이 도입되었습니다. 전체 목록을 보려면 이 링크([https://github.com/microsoft/xlang/pulls?q=is%3Apr+is%3Aclosed](https://github.com/microsoft/xlang/pulls?q=is%3Apr+is%3Aclosed))를 따라가세요.

## <a name="news-and-changes-in-cwinrt-20"></a>C++/WinRT 2.0의 새로운 기능 및 변경 내용

[C++/WinRT Visual Studio 확장(VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264), [Microsoft.Windows.CppWinRT NuGet 패키지](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) 및 `cppwinrt.exe` 도구(다운로드 및 설치 방법 포함)에 대한 자세한 내용은 [C++/WinRT, XAML, VSIX 확장 및 NuGet 패키지에 대한 Visual Studio 지원](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)을 참조하세요.

### <a name="changes-to-the-cwinrt-visual-studio-extension-vsix-for-version-20"></a>버전 2.0용 C++/WinRT Visual Studio 확장(VSIX)의 변경 내용

- 이제 디버그 시각화 도우미는 Visual Studio 2019를 지원하며 Visual Studio 2017을 계속 지원합니다.
- 수많은 버그가 수정되었습니다.

### <a name="changes-to-the-microsoftwindowscppwinrt-nuget-package-for-version-20"></a>버전 2.0용 Microsoft.Windows.CppWinRT NuGet 패키지의 변경 내용

- 이제 `cppwinrt.exe` 도구는 Microsoft.Windows.CppWinRT NuGet 패키지에 포함되며 요청 시 각 프로젝트에 대한 플랫폼 프로젝션 헤더를 생성합니다. 따라서 `cppwinrt.exe` 도구는 더 이상 Windows SDK를 사용하지 않지만, 호환성을 위해 계속해서 SDK와 함께 제공됩니다.
- 이제 `cppwinrt.exe`는 각 플랫폼/ 구성 특정 중간 폴더($IntDir) 아래에 프로젝션 헤더를 생성하여 병렬 빌드를 사용하도록 설정합니다.
- 이제 프로젝트 파일을 수동으로 사용자 지정하려는 경우 C++/WinRT 빌드 지원(속성/대상)이 완벽하게 문서화되어 있습니다. Microsoft.Windows.CppWinRT NuGet 패키지 [추가 정보](https://github.com/microsoft/xlang/tree/master/src/package/cppwinrt/nuget/readme.md#customizing)를 참조하세요.
- 수많은 버그가 수정되었습니다.

### <a name="changes-to-cwinrt-for-version-20"></a>버전 2.0용 C++/WinRT의 변경 내용

#### <a name="open-source"></a>오픈 소스

`cppwinrt.exe` 도구는 Windows 런타임 메타데이터(`.winmd`) 파일을 사용하며, 메타데이터에 설명된 API를 ‘프로젝션’하는 헤더 파일 기반 표준 C++ 라이브러리를 이 파일에서 생성합니다.  이 방법으로 C++/WinRT 코드에서 해당 API를 사용할 수 있습니다.

이제 이 도구는 GitHub에서 제공되는 완전한 오픈 소스 프로젝트입니다. [Microsoft\/xlang](https://github.com/Microsoft/xlang)을 방문하여 **src** > **tool** > **cppwinrt**를 클릭합니다.

#### <a name="xlang-libraries"></a>xlang 라이브러리

앞으로 모든 Windows 런타임 및 xlang 도구는 Windows 런타임에서 사용하는 ECMA-335 메타데이터 형식을 구문 분석하기 위해 완전히 이식 가능한 헤더 전용 라이브러리를 기반으로 합니다. 특히 xlang 라이브러리를 사용하여 처음부터 `cppwinrt.exe` 도구를 다시 작성했습니다. 이 도구는 더 정확한 메타데이터 쿼리를 제공하므로 C++/WinRT 언어 프로젝션에 관련된 몇 가지 오래된 문제가 해결됩니다.

#### <a name="fewer-dependencies"></a>종속성 감소

xlang 메타데이터 판독기로 인해 `cppwinrt.exe` 도구 자체의 종속성이 감소합니다. 이 덕분에 도구는 유연성이 훨씬 더 높아지며 특히 제한된 빌드 환경과 같은 더 많은 시나리오에서 사용할 수 있습니다. 특히 이 도구는 더 이상 `RoMetadata.dll`을 사용하지 않습니다.
 
이 종속성은 `cppwinrt.exe` 2.0에 해당합니다.
 
- ADVAPI32.dll
- KERNEL32.dll
- SHLWAPI.dll
- XmlLite.dll

이러한 DLL은 모두 Windows 10뿐만 아니라 Windows 7, 심지어 Windows Vista에서도 사용할 수 있습니다. 이렇게 하려면 Windows 7을 실행하는 이전 빌드 서버에서 이제 `cppwinrt.exe`를 실행하여 프로젝트에 대한 C++ 헤더를 생성할 수 있습니다. 관심이 있는 경우 약간의 작업을 통해 [Windows 7에서 C++/WinRT를 실행](https://github.com/kennykerr/win7)할 수도 있습니다.

위의 목록을 다음과 같은 `cppwinrt.exe` 1.0의 종속성과 대조합니다.

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

Windows 런타임에는 [MIDL 3.0](/uwp/midl-3/predefined-attributes)에서 메서드와 속성을 데코레이트하는 데 사용할 수 있는 새로운 `[noexcept]` 특성이 있습니다. 이 특성이 있다는 것은 지원하는 도구의 경우 구현이 예외를 throw하지 않고 실패한 HRESULT도 반환하지 않음을 나타냅니다. 이 특성을 사용하면 실패할 가능성이 있는 ABI(Application Binary Interface) 호출을 지원하는 데 필요한 예외 처리 오버헤드를 방지하여 언어 프로젝션으로 코드 생성을 최적화할 수 있습니다.

C++/WinRT는 사용 및 제작 코드의 C++ `noexcept` 구현을 둘 다 생성하여 이 특성을 활용합니다. API 메서드 또는 속성에 오류가 없고 코드 크기가 걱정된다면 이 특성을 살펴볼 수 있습니다.

#### <a name="optimized-code-generation"></a>최적화된 코드 생성

이제 C++/WinRT는 백그라운드에서 훨씬 더 효율적인 C++ 원본 코드를 생성하므로 C++ 컴파일러는 최대한 가장 작고 가장 효율적인 이진 코드를 생성할 수 있습니다. 불필요한 해제 정보를 방지하여 예외 처리 비용을 줄이기 위해 많은 개선 사항이 제공됩니다. 대용량 C++/WinRT 코드를 사용하는 이진 파일은 4% 정도의 코드 크기 감소를 보입니다. 명령 수 감소로 인해 코드도 더 효율적입니다(더 빠르게 실행됨).

이 개선 사항에는 사용자에게 제공되는 새로운 interop 기능도 사용됩니다. 이제 리소스 소유자인 모든 C++/WinRT 형식에는 소유권을 직접 가져오는 생성자가 포함되므로 이전 2단계 접근법을 피할 수 있습니다.

```cppwinrt
ABI::Windows::Foundation::IStringable* raw = ...

IStringable projected(raw, take_ownership_from_abi);

printf("%ls\n", projected.ToString().c_str());
```

#### <a name="optimized-exception-handling-eh-code-generation"></a>최적화된 EH(예외 처리) 코드 생성

이 변경 내용은 Microsoft C++ 최적화 프로그램 팀이 예외 처리 비용을 줄이기 위해 수행한 작업을 보완합니다. 코드에서 COM과 같은 ABI(Application Binary Interface)를 많이 사용하는 경우 이 패턴을 따르는 많은 코드가 관찰됩니다.

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

C++/WinRT 자체는 구현된 모든 API에 대해 이 패턴을 생성합니다. 수천 개의 API 함수를 사용하는 경우 최적화가 중요할 수 있습니다. 과거에 최적화 프로그램은 이 catch 블록이 모두 동일하다는 사실을 감지하지 않았으므로, 각 ABI에 관련된 많은 코드가 복제되고 있었으며 이는 시스템 코드에서 예외를 사용하면 큰 이진 파일이 생성된다고 생각하는 원인이 되었습니다. 그러나 Visual Studio 2019부터 C++ 컴파일러는 해당 catch funclet를 모두 접으며 고유한 funclet만 저장합니다. 결과적으로 이 패턴을 많이 사용하는 이진 파일의 코드 크기가 전체적으로 18% 더 감소합니다. 이제 EH 코드는 반환 코드를 사용하는 것보다 더 효율적일 뿐 아니라 더 큰 이진 파일에 대한 걱정은 과거의 일입니다.

#### <a name="incremental-build-improvements"></a>증분 빌드 개선 사항

이제 `cppwinrt.exe` 도구는 생성된 헤더/원본 파일의 출력을 디스크에 있는 기존 파일의 콘텐츠에 비교하며 파일이 실제로 변경된 경우에만 파일을 작성합니다. 이 덕분에 상당한 디스크 I/O 시간이 절약되며 파일은 C++ 컴파일러에서 “더티”로 간주되지 않습니다. 결과적으로 대부분의 경우 다시 컴파일이 방지되거나 감소합니다.

#### <a name="generic-interfaces-are-now-all-generated"></a>이제 제네릭 인터페이스가 모두 생성됩니다.

xlang 메타데이터 판독기로 인해 C++/WinRT는 이제 메타데이터에서 매개 변수가 있는 인터페이스나 제네릭 인터페이스를 생성합니다. [Windows::Foundation::Collections::IVector\<T\>](/uwp/api/windows.foundation.collections.ivector_t_) 같은 인터페이스는 이제 `winrt/base.h`에서 직접 작성되는 대신 메타데이터에서 생성됩니다. 결과적으로 `winrt/base.h` 크기는 절반으로 감소했으며 코드에 바로 최적화가 생성됩니다(수동으로 최적화하기에는 어려움).

> [!IMPORTANT]
> 이제 제공된 예제와 같은 인터페이스는 `winrt/base.h`가 아닌 각 네임스페이스 헤더에 나타납니다. 따라서 인터페이스를 사용하려면 적절한 네임스페이스 헤더를 포함해야 합니다(아직 포함하지 않은 경우).

#### <a name="component-optimizations"></a>구성 요소 최적화

이 업데이트는 아래 섹션에서 설명하는 C++/WinRT에 대한 여러 추가 옵트인 최적화 지원을 추가합니다. 이 최적화는 새로운 변경 내용(지원을 위해 약간 변경해야 할 수 있음)이므로 명시적으로 켜야 합니다. Visual Studio에서 프로젝트 속성 **공용 속성** > **C++/WinRT** > **최적화됨**을 *예*로 설정합니다. 이렇게 하면 `<CppWinRTOptimized>true</CppWinRTOptimized>`를 프로젝트 파일에 추가하는 효과가 있습니다. 또한 명령줄에서 `cppwinrt.exe`를 호출할 때 `-opt[imize]` 스위치를 추가하는 것과 동일한 효과입니다.

프로젝트 템플릿 기반 새 프로젝트는 기본적으로 `-opt`를 사용합니다.

##### <a name="uniform-construction-and-direct-implementation-access"></a>균일한 생성 및 직접 구현 액세스

이 두 가지 최적화에서는 프로젝션된 형식만 사용하는 경우에도 고유한 구현 형식에 대한 구성 요소 직접 액세스를 허용합니다. 공용 API 표면을 사용하려는 경우에는 [**make**](/uwp/cpp-ref-for-winrt/make), [**make_self**](/uwp/cpp-ref-for-winrt/make-self) 및 [**get_self**](/uwp/cpp-ref-for-winrt/get-self)를 사용할 필요가 없습니다. 호출은 호출을 구현으로 이동하도록 컴파일되며 완전히 인라인될 수도 있습니다.

자세한 내용과 및 코드 예제는 [균일한 생성 및 직접 구현 액세스 옵트인](/windows/uwp/cpp-and-winrt-apis/author-apis#opt-in-to-uniform-construction-and-direct-implementation-access)을 참조하세요.

##### <a name="type-erased-factories"></a>형식이 지워진 팩터리

이 최적화는 `module.g.cpp`에서 #include 종속성을 피하므로 단일 구현 클래스가 변경될 때마다 다시 컴파일할 필요가 없습니다. 이에 따라 빌드 성능이 향상됩니다.

#### <a name="smarter-and-more-efficient-modulegcpp-for-large-projects-with-multiple-libs"></a>여러 라이브러리가 포함된 큰 프로젝트에 더 스마트하고 더 효율적인 `module.g.cpp`

이제 `module.g.cpp` 파일에는 **winrt_can_unload_now** 및 **winrt_get_activation_factory**로 명명된 두 가지 구성 가능한 도우미도 추가로 포함됩니다. 이 도우미는 각각 고유한 런타임 클래스를 포함하는 많은 라이브러리로 DLL이 구성된 더 큰 프로젝트용으로 디자인되었습니다. 이 경우 수동으로 DLL의 **DllGetActivationFactory** 및 **DllCanUnloadNow**를 함께 연결해야 합니다. 이 도우미를 사용하면 출처 위조 오류가 방지되어 이 작업을 훨씬 더 쉽게 수행할 수 있습니다. 또한 `cppwinrt.exe` 도구의 `-lib` 플래그는 개별 라이브러리에 고유한 개별적으로 프리앰블(`winrt_xxx`가 아님)을 제공하는 데 사용되므로 각 라이브러리의 함수는 개별적으로 이름이 지정되어 명시적으로 결합될 수 있습니다.

#### <a name="coroutine-support"></a>코루틴 지원

코루틴 지원은 자동으로 포함됩니다. 이전에는 지원이 분산되어 있어 너무 제한적이라고 생각했습니다. 또한 v2.0의 경우 일시적으로 `winrt/coroutine.h` 헤더 파일이 필요했지만 더 이상 필요하지 않습니다. 이제 Windows 런타임 비동기 인터페이스가 생성되었으므로 수동으로 작성하지 않아도 이제 `winrt/Windows.Foundation.h`에 코루틴이 있습니다. 더 효율적으로 유지 및 지원할 수 있다는 것 외에도, [**resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground)와 같은 코루틴 도우미를 더 이상 특정 네임스페이스 헤더의 끝까지 추적할 필요가 없습니다. 오히려 더 자연스럽게 종속성을 포함할 수 있습니다. 또한 이를 통해 **resume_foreground**는 지정된 [**Windows::UI::Core::CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher) 다시 시작을 지원할 뿐 아니라 지정된 [**Windows::System::DispatcherQueue**](/uwp/api/windows.system.dispatcherqueue) 다시 시작도 지원할 수 있습니다. 이전에는 하나의 네임스페이스에만 정의가 포함될 수 있었기 때문에 두 개 중 하나만 지원할 수 있었습니다.

다음은 **DispatcherQueue** 지원 예제입니다.

```cppwinrt
...
#include <winrt/Windows.System.h>
using namespace Windows::System;
...
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

이제 코루틴 도우미는 `[[nodiscard]]`로 데코레이트되기도 하므로 사용 편의성이 향상됩니다. 그러나 코루틴 도우미 작동에 필요한 코루틴 도우미 `co_await`를 수행하는 것을 잊거나 감지하지 못하는 실수를 하면 `[[nodiscard]]`로 인해 컴파일러 경고가 생성됩니다.

#### <a name="help-with-diagnosing-direct-stack-allocations"></a>직접(스택) 할당 진단 지원

프로젝션 및 구현 클래스 이름은 기본적으로 동일하고 네임스페이스로만 구별되므로 두 클래스를 혼동할 수 있고 도우미의 [**make**](/uwp/cpp-ref-for-winrt/make) 패밀리를 사용하는 대신 실수로 스택에서 구현을 만들 수도 있습니다. 해결되지 않은 참조가 진행되는 동안 개체가 소멸될 수 있으므로 경우에 따라 이 문제를 진단하기가 어려울 수 있습니다. 이제 어설션은 디버그 빌드를 위해 이 문제를 발견합니다. 어설션은 코루틴 내부에서 스택 할당을 감지하지 않지만 대부분의 해당 실수를 발견하는 데 유용합니다.

자세한 내용은 [직접 할당 진단](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc)을 참조하세요.

#### <a name="improved-capture-helpers-and-variadic-delegates"></a>향상된 캡처 도우미 및 variadic 대리자

이 업데이트는 프로젝션된 형식을 지원하여 캡처 도우미의 제한 사항을 수정합니다. 이 업데이트는 지금부터 프로젝션된 형식을 반환할 때 Windows 런타임 interop API와 함께 제공됩니다.

또한 variadic(Windows 이외 런타임) 대리자를 만들 때 [**get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) 및 [**get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) 지원을 추가합니다.

#### <a name="support-for-deferred-destruction-and-safe-qi-during-destruction"></a>소멸 중에 지연된 삭제 및 안전한 QI 지원

참조 수를 일시적으로 증가시키는 메서드를 호출하는 것은 런타임 클래스 개체의 소멸자에서 종종 일어나는 일입니다. 참조 수가 0으로 반환되면 개체가 다시 소멸됩니다. XAML 애플리케이션에서는 계층 구조에서 위아래로 일부 정리 구현을 호출하기 위해 소멸자의 [**QueryInterface**](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))(QI)를 수행해야 할 수 있습니다. 하지만 개체의 참조 수가 이미 0에 도달했으므로 QI는 참조 수 바운스 역시 구성합니다.

이 업데이트는 참조 수를 디바운스하는 지원을 추가하여 일단 0에 도달하면 다시 나타나지 않도록 하지만, 소멸 중에 필요한 QI는 일시적으로 허용합니다. 특정 XAML 애플리케이션/컨트롤에서는 이 프로시저를 피할 수 없으며 이제 C++/WinRT가 이 프로시저에 탄력적으로 대처합니다.

구현 형식에서 정적 **final_release** 함수를 제공하면 소멸을 연기할 수 있습니다. **std::unique_ptr** 형식으로 개체에 대해 마지막 남은 포인터가 **final_release**로 전달됩니다. 그러면 해당 포인터의 소유권을 다른 컨텍스트로 이동하도록 선택할 수 있습니다. 이중 소멸을 트리거하지 않고 포인터에 대해 QI를 수행하는 것이 안전합니다. 하지만 참조 수에 대한 실제 변경 값은 개체 소멸 지점에서 0이어야 합니다.

**final_release**의 반환 값은 `void`가 될 수 있으며, 이 값은 [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) 또는 **winrt::fire_and_forget** 등과 같은 비동기 작업 개체입니다.

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

    static void final_release(std::unique_ptr<Sample> self) noexcept
    {
        // Move 'self' as needed to delay destruction.
    }
};
```

아래 예제에서 **MainPage**가 마지막으로 릴리스되면 **final_release**가 호출됩니다. 이 함수는 5초 동안 스레드 풀에서 대기한 후 QI/AddRef/Release 작동에 필요한 페이지 **Dispatcher**를 사용하여 다시 시작합니다. 그 다음, 해당 UI 스레드에서 리소스를 정리합니다. 그리고 마지막으로 **MainPage** 소멸자를 실제로 호출하는 **unique_ptr**을 지웁니다. 이 소멸자에서도 **IFrameworkElement**에 대한 QI가 필요한 **DataContext**는 호출됩니다.

**final_release**를 코루틴으로 구현할 필요는 없습니다. 그러나 작업이 수행되면 이 예제에서처럼 소멸을 다른 스레드로 매우 쉽게 이동할 수 있습니다.

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

    static IAsyncAction final_release(std::unique_ptr<MainPage> self)
    {
        co_await 5s;

        co_await resume_foreground(self->Dispatcher());
        co_await self->resource.CloseAsync();

        // The object is destructed normally at the end of final_release,
        // when the std::unique_ptr<MyClass> destructs. If you want to destruct
        // the object earlier than that, then you can set *self* to `nullptr`.
        self = nullptr;
    }
};
```

자세한 내용은 [지연된 소멸](/windows/uwp/cpp-and-winrt-apis/details-about-destructors#deferred-destruction)을 참조하세요.

#### <a name="improved-support-for-com-style-single-interface-inheritance"></a>COM 스타일 단일 인터페이스 상속에 대한 향상된 지원

또한 Windows 런타임 프로그래밍을 위해 C++/WinRT는 COM 전용 API를 작성하고 사용하는 데 사용됩니다. 이 업데이트를 통해 인터페이스 계층 구조가 있는 COM 서버를 구현할 수 있습니다. 이 서버는 Windows 런타임에 필요하지 않지만 일부 COM 구현에는 필요합니다.

#### <a name="correct-handling-of-out-params"></a>`out` 매개 변수의 올바른 처리

특히 Windows 런타임 배열과 같은 `out` 매개 변수는 사용하기 어려울 수 있습니다. 이 업데이트를 통해 C++/WinRT는 `out` 매개 변수 및 배열에 도달할 때 더 강력하고 탄력적으로 실수에 대처할 수 있으며, 해당 매개 변수가 언어 프로젝션을 통해 도착하는지 또는 원시 ABI를 사용하거나 실수로 변수를 일관성 있게 초기화하지 않는 COM 개발자로부터 도착하는지 여부는 상관이 없습니다. 두 경우 모두에서 C++/WinRT는 리소스의 릴리스를 기억하여 프로젝션된 형식을 ABI에 전달하게 될 때와 0에 도달하거나 ABI를 통해 도착하는 매개 변수를 정리할 때 올바른 작업을 수행합니다.

#### <a name="events-now-handle-invalid-tokens-reliably"></a>이제 이벤트는 잘못된 토큰을 안정적으로 처리함

[**winrt::event**](/uwp/cpp-ref-for-winrt/event) 구현은 이제 **remove** 메서드가 잘못된 토큰 값(배열에 없는 값)으로 호출되는 경우를 적절하게 처리합니다.

#### <a name="coroutine-local-variables-are-now-destroyed-before-the-coroutine-returns"></a>이제 코루틴이 반환되기 전에 코루틴 로컬 변수가 제거됨

코루틴 형식을 구현하는 일반적인 방법에서는 코루틴이 *반환/완료되면*(마지막 일시 중단 이전이 아님) 코루틴 내의 로컬 변수가 제거되도록 허용할 수 있습니다. 이 문제를 피하고 다른 이점을 만들기 위해 대기자 다시 시작은 이제 마지막 일시 중단까지 지연됩니다.

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>Windows SDK 버전 10.0.17763.0(Windows 10 버전 1809)의 새로운 기능 및 변경 내용

아래 표에는 Windows SDK 버전 10.0.17763.0(Windows 10 버전 1809)에서 C++/WinRT의 새로운 기능과 변경 내용이 포함됩니다.

| 새로운 기능 또는 변경된 기능 | 추가 정보 |
| - | - |
| **주요 변경 내용**. C++/WinRT는 컴파일하기 위해 Windows SDK의 헤더를 사용하지 않습니다. | 아래 [Windows SDK 헤더 파일에서 격리](#isolation-from-windows-sdk-header-files)를 참조하세요. |
| Visual Studio 프로젝트 시스템 형식이 변경되었습니다. | 아래 [C++/WinRT 프로젝트의 대상을 Windows SDK 최신 버전으로 다시 지정하는 방법](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)을 참조하세요. |
| Windows 런타임 함수에 컬렉션 개체를 전달하는 데 도움이 되거나 자체 컬렉션 속성과 컬렉션 형식을 구현하는 새로운 함수와 기본 클래스가 있습니다. | [C++/WinRT로 작성된 컬렉션](collections.md)을 참조하세요. |
| C++/WinRT 런타임 클래스와 함께 [{바인딩}](/windows/uwp/xaml-platform/binding-markup-extension) 태그 확장을 사용할 수 있습니다. | 자세한 내용과 코드 예제는 [데이터 바인딩 개요](/windows/uwp/data-binding/data-binding-quickstart)를 참조하세요. |
| 코루틴 취소 지원을 통해 취소 콜백을 등록할 수 있습니다. | 자세한 내용과 코드 예제는 [비동기 작업 취소 및 취소 콜백](concurrency-2.md#canceling-an-asynchronous-operation-and-cancellation-callbacks)을 참조하세요. |
| 멤버 함수를 가리키는 대리자를 만들 때 처리기가 등록된 지점에서 원시 *this* 포인터 대신에 현재 개체에 대한 강력하거나 약한 참조를 설정할 수 있습니다. | 자세한 내용과 코드 예제는 [이벤트 처리 대리자를 사용하여 안전하게 *this* 포인터 액세스](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate) 섹션에서 **멤버 함수를 대리자로 사용하는 경우** 하위 섹션을 참조하세요. |
| Visual Studio의 C++ 표준 규칙이 향상되어 처리되지 않았던 버그가 수정되었습니다. C++/WinRT의 표준 규칙의 유효성을 검사하기 위해 LLVM 및 Clang 도구 체인을 더 효율적으로 이용합니다. | [새 프로젝트가 컴파일되지 않는 이유는 무엇인가요? Visual Studio 2017(버전 15.8.0 이상) 및 SDK 버전 17134를 사용하고 있습니다.](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134)에 설명된 문제가 더 이상 발생하지 않습니다. |

기타 변경 내용은 아래와 같습니다.

- **주요 변경 내용**. [**winrt::get_abi(winrt::hstring const&)** ](/uwp/cpp-ref-for-winrt/get-abi)는 이제 `HSTRING` 대신 `void*`를 반환합니다. `static_cast<HSTRING>(get_abi(my_hstring));`을 사용하여 HSTRING을 가져올 수 있습니다. [ABI의 HSTRING과 상호 운용](interop-winrt-abi.md#interoperating-with-the-abis-hstring)을 참조하세요.
- **주요 변경 내용**. [**winrt::put_abi(winrt::hstring&)** ](/uwp/cpp-ref-for-winrt/put-abi)는 이제 `HSTRING*` 대신 `void**`를 반환합니다. `reinterpret_cast<HSTRING*>(put_abi(my_hstring));`을 사용하여 HSTRING*를 가져올 수 있습니다. [ABI의 HSTRING과 상호 운용](interop-winrt-abi.md#interoperating-with-the-abis-hstring)을 참조하세요.
- **주요 변경 내용**. 이제 HRESULT는 **winrt::hresult**로 프로젝션됩니다. 형식 검사를 수행하거나 형식 특성을 지원하기 위해 HRESULT가 필요한 경우 **::hresult**에 `static_cast`를 수행할 수 있습니다. 그러지 않으면 C++/WinRT 헤더를 포함하기 전에 `unknwn.h`를 포함하는 한 **winrt::hresult**가 HRESULT로 변환됩니다.
- **주요 변경 내용**. GUID는 이제 **winrt::guid**로 프로젝션됩니다. 구현하는 API의 경우 GUID 매개 변수에 **winrt::guid**를 사용해야 합니다. 그러지 않으면 C++/WinRT 헤더를 포함하기 전에 `unknwn.h`를 포함하는 한 **winrt::guid**가 GUID로 변환됩니다. [ABI의 GUID 구조체와 상호 운용](interop-winrt-abi.md#interoperating-with-the-abis-guid-struct)을 참조하세요.
- **주요 변경 내용**. [**winrt::handle_type 생성자**](/uwp/cpp-ref-for-winrt/handle-type#handle_typehandle_type-constructor)가 명시적으로 설정되어 강화되었습니다(이제 이 생성자로 잘못된 코드를 작성하기가 더 어려움). 원시 핸들 값을 할당해야 하는 경우 [**handle_type::attach 함수**](/uwp/cpp-ref-for-winrt/handle-type#handle_typeattach-function)를 대신 호출합니다.
- **주요 변경 내용**. **WINRT_CanUnloadNow** 및 **WINRT_GetActivationFactory**의 서명이 변경되었습니다. 이 함수는 선언하면 안 됩니다. 대신 C++/WinRT Windows 네임스페이스 헤더 파일을 포함하는 경우 자동으로 포함되는 `winrt/base.h`를 포함하여 이 함수의 선언을 포함합니다.
- [**winrt::clock 구조체**](/uwp/cpp-ref-for-winrt/clock)의 경우 **from_FILETIME/to_FILETIME**은 더 이상 사용되지 않고, 대신 **from_file_time/to_file_time**이 사용됩니다.
- **IBuffer** 매개 변수가 필요한 API가 간소화되었습니다. 대부분의 API는 컬렉션 또는 배열을 선호합니다. 하지만 **IBuffer**를 사용하는 API를 더 쉽게 호출할 수 있어야 한다고 생각했습니다. 이 업데이트에서는 **IBuffer** 구현 뒤의 데이터에 직접 액세스할 수 있습니다. C++ 표준 라이브러리 컨테이너에서 사용하는 것과 동일한 데이터 명명 규칙을 사용합니다. 또한 이 규칙은 일반적으로 대문자로 시작하는 메타데이터 이름과 충돌하지 않도록 방지합니다.
- 코드 생성이 향상되었습니다. 코드 크기를 줄이고, 인라인을 개선하고, 팩터리 캐싱을 최적화하기 위한 다양한 개선 사항이 있습니다.
- 불필요한 재귀가 제거되었습니다. 명령줄이 특정 `.winmd`가 아닌 폴더를 참조하는 경우 `cppwinrt.exe` 도구는 더 이상 `.winmd` 파일을 재귀적으로 검색하지 않습니다. 또한 `cppwinrt.exe` 도구는 이제 복제본을 더 적절하게 처리하므로 사용자 오류 및 형식이 잘못된 `.winmd` 파일에 더 탄력적으로 대처할 수 있습니다.
- 스마트 포인터가 강화되었습니다. 이전에는 새 값을 이동 할당할 때 이벤트 취소자가 호출되지 않았습니다. 이를 통해 스마트 포인터가 루트가 [**winrt::com_ptr struct template**](/uwp/cpp-ref-for-winrt/com-ptr)인 자체 할당을 안정적으로 처리하지 않는 문제를 파악할 수 있었습니다. **winrt::com_ptr**이 수정되었으며, 이벤트 취소자는 이동 의미 체계를 제대로 처리하여 할당 시 취소되도록 수정되었습니다.

> [!IMPORTANT]
> 버전 1.0.181002.2 및 이후 버전 1.0.190128.4에서 모두 [C++/WinRT Visual Studio 확장(VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)에 대한 주요 변경 내용이 있습니다. 이 변경 내용에 대한 자세한 내용과 이 변경 내용이 기존 프로젝트에 어떻게 영향을 미치는지 알아보려면 [Visual Studio의 C++/WinRT 지원](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) 및 [이전 버전의 VSIX 확장](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)을 참조하세요.

### <a name="isolation-from-windows-sdk-header-files"></a>Windows SDK 헤더 파일에서 격리

이는 코드의 주요 변경 내용이 될 수 있습니다.

C++/WinRT는 컴파일하기 위해 더 이상 Windows SDK의 헤더 파일을 사용하지 않습니다. CRT(C 런타임 라이브러리) 및 C++ STL(표준 템플릿 라이브러리)에 있는 헤더 파일에도 Windows SDK 헤더가 포함되지 않습니다. 또한 이를 통해 표준을 좀 더 준수하고, 의도치 않은 종속성을 방지하고, 보호해야 하는 매크로 수를 크게 줄입니다.

이 독립성은 C++/WinRT가 이제 좀 더 이식 가능하고 표준을 준수하며 컴파일러 간 라이브러리와 플랫폼 간 라이브러리가 될 가능성을 발전시킨다는 것을 의미합니다. 또한 C++/WinRT 헤더가 매크로에 악영향을 주지 않음을 의미합니다.

이전에 프로젝트에 Windows 헤더를 포함하기 위해 C++/WinRT에 헤더를 남겨 놓은 경우 이제는 직접 헤더를 포함해야 합니다. 언제든지 사용하는 헤더를 명시적으로 포함하며 헤더를 포함하기 위해 다른 라이브러리에 헤더를 남겨 놓지 않는 것이 좋습니다.

현재 Windows SDK 헤더 파일 격리의 유일한 예외는 내장 함수와 숫자입니다. 마지막으로 남은 이 종속성에는 알려진 문제가 없습니다.

프로젝트에서 필요한 경우 Windows SDK 헤더를 통해 interop를 다시 사용하도록 설정할 수 있습니다. 예를 들어 루트가 [**IUnknown**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)인 COM 인터페이스를 구현하려고 할 수 있습니다. 이 예제의 경우 C++/WinRT 헤더를 포함하기 전에 `unknwn.h`를 포함합니다. 이렇게 하면 C++/WinRT 기본 라이브러리가 다양한 후크를 통해 클래식 COM 인터페이스를 지원할 수 있습니다. 코드 예제는 [C++/WinRT를 통한 COM 구성 요소 작성](author-coclasses.md)을 참조하세요. 마찬가지로, 호출하려는 형식 /또는 함수를 선언하는 다른 Windows SDK 헤더를 명시적으로 포함합니다.

### <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>C++/WinRT 프로젝트의 대상을 Windows SDK 최신 버전으로 다시 지정하는 방법

컴파일러 및 링커 문제를 최소화할 수 있는 프로젝트의 대상을 다시 지정하는 방법에는 많은 작업이 필요합니다. 이 방법에서는 선택한 Windows SDK 버전을 대상으로 하는 새 프로젝트를 만든 후 이전 프로젝트에서 새 프로젝트로 파일을 복사합니다. 추가하는 파일을 Visual Studio에 저장하기 위해 복사하기만 하면 되는 이전 `.vcxproj` 및 `.vcxproj.filters` 파일의 섹션이 있습니다.

그러나 Visual Studio에서 프로젝트 대상을 다시 지정하는 두 가지 다른 방법이 있습니다.

- **일반** \> **Windows SDK 버전** 프로젝트 속성으로 차례로 이동한 다음, **모든 구성** 및 **모든 플랫폼**을 선택합니다. **Windows SDK 버전**을 대상으로 지정할 버전으로 설정합니다.
- **솔루션 탐색기**에서 프로젝트 노드를 마우스 오른쪽 단추로 클릭하고, **프로젝트 대상 다시 지정**을 클릭하고, 대상으로 지정할 버전을 선택한 다음, **확인**을 클릭합니다.

이 두 가지 방법 중 하나를 사용한 후 컴파일러 또는 링커 오류가 발생하면 다시 빌드하기 전에 솔루션을 정리할 수 있습니다(**빌드** > **솔루션 정리** 및/또는 수동으로 모든 임시 폴더와 파일 삭제).

C++ 컴파일러에서 “*오류 C2039: ‘IUnknown’: '\`전역 네임스페이스*''의 멤버가 아닙니다.”가 생성되면 C++/WinRT 헤더를 포함하기 전에 `#include <unknwn.h>`를 `pch.h` 파일의 맨 위에 추가합니다.

또한 그 뒤에 `#include <hstring.h>`을 추가해야 할 수 있습니다.

C++ 링커에서 “*오류 LNK2019: 확인되지 않은 외부 기호 _WINRT_CanUnloadNow@0이 _VSDesignerCanUnloadNow@0 함수에서 참조되었습니다.* ”가 생성되면 `#define _VSDESIGNER_DONT_LOAD_AS_DLL`을 `pch.h` 파일에 추가하여 이 오류를 해결할 수 있습니다.
