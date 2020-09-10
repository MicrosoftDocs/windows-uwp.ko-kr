---
description: C++/WinRT는 헤더 파일 기반 라이브러리로 구현된 WinRT(Windows 런타임) API용 완전한 표준 C++17 언어 프로젝션입니다.
title: C++/WinRT
ms.date: 04/18/2019
ms.topic: article
keywords: Windows 10, UWP, 표준, C++, CPP, WinRT, 프로젝션
ms.localizationpriority: medium
ms.openlocfilehash: 3f05bbd1ad5ea770e96ebbbd74c3a980ae0585b7
ms.sourcegitcommit: 6009896ead442b378106d82870f249dc8b55b886
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89643779"
---
# <a name="cwinrt"></a>C++/WinRT

[C++/WinRT](./intro-to-using-cpp-with-winrt.md)는 헤더 파일 기반 라이브러리로 구현된 WinRT(Windows 런타임) API용 완전한 표준 C++17 언어 프로젝션이며, 최신 Windows API에 대해 최고 수준의 액세스를 제공하도록 설계되었습니다. C++/WinRT를 사용하면 모든 표준 호환 C++17 컴파일러를 통해 Windows 런타임 API를 작성하고 사용할 수 있습니다. Windows SDK는 C++/WinRT를 포함하며, 10.0.17134.0 버전(Windows 10 버전 1803)에서 도입되었습니다.

C++/WinRT는 아름답고 빠른 Windows용 코드를 작성하는 데 관심이 있는 모든 개발자에게 적합합니다. 그 이유는 다음과 같습니다.

## <a name="the-case-for-cwinrt"></a>C++/WinRT의 경우
&nbsp;
> [!VIDEO https://www.youtube.com/embed/TLSul1XxppA]

C++ 프로그래밍 언어는 엔터프라이즈 및 ISV(Independent Software Vendor) 부문 *모두*에서 높은 수준의 정확성, 품질 및 성능이 가치가 있는 애플리케이션에 사용합니다. 예를 들어 시스템 프로그래밍, 리소스가 제한된 포함 및 모바일 시스템, 게임 및 그래픽, 디바이스 드라이버, 산업/과학/의학 애플리케이션 등이 있습니다.

언어적 관점에서 C++는 항상 형식이 풍부하고 가벼운 추상화를 작성하고 사용하는 것에 관련되어 있습니다. 하지만 원시 포인터, 원시 루프 및 C++98의 까다로운 메모리 할당 및 해제 이후로 언어가 급격하게 바뀌었습니다. 최신 C++(C++11 이후)는 아이디어, 단순성, 가독성 및 버그 발생에 대한 훨씬 낮은 가능성을 명확하게 표현하고 있습니다.

C++를 사용하여 Windows 런타임 API를 작성하고 사용하는 경우 C++/WinRT가 있습니다. 이는 Microsoft에서 추천하는 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) 언어 프로젝션 및 [Windows 런타임 C++ 템플릿 라이브러리(WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live)용 대체 언어입니다.

C++/WinRT를 사용하는 경우 표준 C++ 데이터 형식, 알고리즘 및 키워드가 사용됩니다. 프로젝션에는 자체의 사용자 지정 데이터 형식이 있지만, 대부분의 경우 표준 형식과 적절히 변환할 수 있으므로 해당 데이터 형식에 대해 자세히 알고 있을 필요가 없습니다. 또한 사용하는 데 익숙한 표준 C++ 언어 기능과 이미 갖고 있는 소스 코드를 계속 사용할 수 있습니다. Win32에서 UWP까지에 이르는 모든 C++ 애플리케이션에서 C++/WinRT를 통해 Windows 런타임 API를 매우 쉽게 호출할 수 있습니다.

C++/WinRT는 Windows 런타임에 사용되는 다른 언어 옵션보다 성능이 뛰어나고 더 작은 이진 파일을 생성합니다. 심지어 ABI 인터페이스를 직접 사용하여 작성한 코드보다도 우수합니다. 이는 추상화에서 Visual C++ 컴파일러가 최적화하도록 설계된 최신 C++ 관용구를 사용하기 때문입니다. 여기에는 매직 정적, 빈 기본 클래스, **strlen** 생략뿐만 아니라 특히 C++/WinRT의 성능 향상을 목표로 하는 최신 버전의 Visual C++에서 제공하는 다수의 최신 최적화가 포함됩니다.

프로젝트에 C++/WinRT를 점진적으로 도입하는 방법은 여러 가지가 있습니다. [Windows 런타임 구성 요소](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)를 사용하거나 C++/CX와 상호 운용할 수 있습니다. 자세한 내용은 [C++/WinRT와 C++/CX 간의 상호 운용성](./interop-winrt-cx.md)을 참조하세요.

C++/WinRT로 포팅하는 방법에 대한 자세한 내용은 다음 리소스를 참조하세요.

- [C++/CX에서 C++/WinRT로 이동](./move-to-winrt-from-cx.md)
- [WRL에서 C++/WinRT로 이동](./move-to-winrt-from-wrl.md)
- [C#에서 C++/WinRT로 이동](./move-to-winrt-from-csharp.md)

또한 [C++/WinRT 샘플 앱은 어디에서 찾을 수 있나요?](/windows/uwp/cpp-and-winrt-apis/faq#where-can-i-find-cwinrt-sample-apps)도 참조하세요.

### <a name="topics-about-cwinrt"></a>C++/WinRT 관련 항목

| 항목 | 설명 |
| - | - |
| [C++/WinRT 소개](./intro-to-using-cpp-with-winrt.md) | Windows 런타임 API용 표준 C++ 언어 프로젝션인 C++/WinRT에 대한 소개입니다. |
| [C++/WinRT 시작](./get-started.md) | C++/WinRT 사용 속도를 높이려면 이 항목에서 안내하는 단순한 코드 예제를 살펴봅니다. |
| [C++/WinRT의 새로운 기능](./news.md) | C++/WinRT에서 새롭거나 변경된 기능입니다. |
| [질문과 대답](./faq.md) | C++/WinRT를 통해 Windows 런타임 API를 작성하거나 사용하는 것과 관련하여 발생할 수 있는 질문에 대한 대답입니다. |
| [문제 해결](./troubleshooting.md) | 이 항목에 나와 있는 문제 증상 및 해결 방법에 대한 표는 새 코드를 자르거나 기존 앱을 이식할지를 결정하는 데 도움이 될 수 있습니다. |
| [Photo Editor C++/WinRT 샘플 애플리케이션](./photo-editor-sample.md) | Photo Editor는 C++/WinRT 언어 프로젝션을 통한 개발을 보여 주는 UWP 샘플 애플리케이션입니다. 샘플 애플리케이션을 사용하면 **사진** 라이브러리에서 사진을 검색한 다음, 다양한 사진 효과를 사용하여 선택한 이미지를 편집할 수 있습니다. | 
| [문자열 처리](./strings.md) | C++/WinRT를 사용하면 표준 C++ 와이드 문자열 형식을 사용하여 Windows 런타임 API를 호출하거나, [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 형식을 사용할 수 있습니다. |
| [표준 C++ 데이터 형식 및 C++/WinRT](./std-cpp-data-types.md) | C++/WinRT를 사용하면 표준 C++ 데이터 형식을 사용하여 Windows 런타임 API를 호출할 수 있습니다. |
| [스칼라 값을 IInspectable로 boxing 및 unboxing](./boxing.md) | 스칼라 값은 **IInspectable**이 필요한 함수에 전달되기 전에 참조 클래스 개체 내에 래핑되어야 합니다. 이 래핑 프로세스에 대해 값을 *boxing*한다고 합니다. |
| [C++/WinRT를 통한 API 사용](./consume-apis.md) | Windows, 타사 구성 요소 공급업체 또는 사용자 자신이 구현하는지 여부에 관계없이 C++/WinRT API를 사용하는 방법을 보여 줍니다. |
| [C++/WinRT를 통한 API 작성](./author-apis.md) | **winrt::implements** 기본 구조체를 직접 또는 간접적으로 사용하여 C++/WinRT API를 작성하는 방법을 보여 줍니다. |
| [C++/WinRT를 통한 오류 처리](./error-handling.md) | C++/WinRT를 사용하여 프로그래밍할 때 오류를 처리하기 위한 전략에 대해 설명합니다. |
| [대리자를 사용하여 이벤트 처리](./handle-events.md) | C++/WinRT를 사용하여 이벤트 처리 대리자를 등록하거나 취소하는 방법을 보여 줍니다. |
| [이벤트 작성](./author-events.md) | 이벤트를 발생시키는 런타임 클래스가 포함된 Windows 런타임 구성 요소를 작성하는 방법을 보여 줍니다. 또한 구성 요소를 사용하고 이벤트를 처리하는 앱도 보여 줍니다. |
| [C++/WinRT로 작성된 컬렉션](./collections.md) | C++/WinRT는 컬렉션을 구현하거나 전달하려는 경우 많은 시간과 노력을 절약할 수 있는 함수와 기본 클래스를 제공합니다. |
| [동시성 및 비동기 작업](./concurrency.md) | C++/WinRT를 사용하여 Windows 런타임 비동기 개체를 만들고 사용하는 방법을 보여 줍니다. |
| [한층 향상한 동시성, 비동기 프로그래밍 기능](./concurrency-2.md) | C++/WinRT로 동시성, 비동기 작업을 모두 수행하는 고급 시나리오. |
| [XAML 컨트롤(C++/WinRT 속성에 바인딩)](./binding-property.md) | XAML 컨트롤에 효과적으로 바인딩할 수 있는 속성은 *식별할 수 있는*(observable) 속성이라고 합니다. 이 항목에서는 식별할 수 있는 속성을 구현하고 사용하는 방법과 XAML 컨트롤에 바인딩하는 방법을 보여 줍니다. |
| [XAML 항목 컨트롤(C++/WinRT 컬렉션에 바인딩)](./binding-collection.md) | XAML 항목에 효과적으로 바인딩할 수 있는 컬렉션은 *식별할 수 있는*(observable) 컬렉션이라고 합니다. 이 항목에서는 식별할 수 있는 컬렉션을 구현하고 사용하는 방법과 XAML 항목에 바인딩하는 방법을 보여 줍니다. |
| [C++/WinRT를 사용한 XAML 사용자 지정(템플릿 기반) 컨트롤](./xaml-cust-ctrl.md) | C++/WinRT를 사용하여 간단한 사용자 지정 컨트롤을 만드는 단계를 안내합니다. 여기에 나와 있는 정보에 따라 기능이 풍부하고 사용자 지정 가능한 UI 컨트롤을 만들 수 있습니다. |
| [매개 변수를 ABI 경계로 전달](./pass-parms-to-abi.md) | C++/WinRT는 일반적인 경우에 대한 자동 변환을 제공하여 매개 변수를 ABI 경계에 전달하는 것을 단순화합니다. |
| [C++/WinRT를 통한 COM 구성 요소 사용](./consume-com.md) | 전체 Direct2D 코드 예제를 사용하여 C++/WinRT를 통해 COM 클래스 및 인터페이스를 사용하는 방법을 보여 줍니다. |
| [C++/WinRT를 통한 COM 구성 요소 작성](./author-coclasses.md) | C++/WinRT는 Windows 런타임 클래스를 작성하는 데 도움이 되는 것처럼 클래식 COM 구성 요소를 작성하는 데도 도움이 됩니다. |
| [C++/CX에서 C++/WinRT로 이동](./move-to-winrt-from-cx.md) | 이 항목에서는 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 프로젝트의 소스 코드를 [C++/WinRT](./intro-to-using-cpp-with-winrt.md)의 해당하는 소스 코드로 포팅하는 데 관련된 기술 세부 정보에 대해 설명합니다. |
| [C++/WinRT와 C++/CX 간의 상호 운용성](./interop-winrt-cx.md) | 이 항목에서는 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 개체와 [C++/WinRT](./intro-to-using-cpp-with-winrt.md) 개체 간에 변환하는 데 사용할 수 있는 두 가지 도우미 함수를 보여 줍니다. |
| [C++/WinRT와 C++/CX 간의 비동기성 및 상호 운용성](./interop-winrt-cx-async.md) | [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)에서 [C++/WinRT](./intro-to-using-cpp-with-winrt.md)로 점진적으로 포팅하는 방법을 설명하는 고급 항목입니다. PPL(병렬 패턴 라이브러리) 작업 및 코루틴이 동일한 프로젝트에 나란히 존재할 수 있는 방법을 보여줍니다. |
| [WRL에서 C++/WinRT로 이동](./move-to-winrt-from-wrl.md) | [Windows 런타임 C++ 템플릿 라이브러리(WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) 코드를 [C++/WinRT](./intro-to-using-cpp-with-winrt.md)의 상응하는 코드로 포팅하는 방법을 보여줍니다. |
| [C#&mdash;사례 연구에서 클립보드 샘플을 C++/WinRT로 포팅](./clipboard-to-winrt-from-csharp.md) | 이 항목에서는 [UWP(유니버설 Windows 플랫폼) 앱 샘플](https://github.com/microsoft/Windows-universal-samples) 중 하나를 [C#](/visualstudio/get-started/csharp)에서 [C++/WinRT](./intro-to-using-cpp-with-winrt.md)로 포팅하는 사례 연구를 제공합니다. 연습을 진행하면서 샘플을 직접 이식하여 이식을 연습해보고 값진 경험을 얻을 수 있습니다. |
| [C#에서 C++/WinRT로 이동](./move-to-winrt-from-csharp.md) | 이 항목에서는 [C#](/visualstudio/get-started/csharp) 프로젝트의 소스 코드를 [C++/WinRT](./intro-to-using-cpp-with-winrt.md)의 해당 소스 코드로 이식하는 데 관련된 기술 세부 정보를 포괄적으로 분류합니다. |
| [C++/WinRT와 ABI 사이의 상호 운용성](./interop-winrt-abi.md) | ABI(애플리케이션 이진 인터페이스)와 C++/WinRT 개체 간에 변환하는 방법을 보여 줍니다. |
| [C++/WinRT의 강력하고 약한 참조](./weak-references.md) | Windows 런타임은 참조 계산 시스템입니다. 이러한 시스템에서는 강한 참조와 약한 참조의 중요성과 차이점을 인식해야 합니다. |
| [Agile 개체](./agile-objects.md) | Agile 개체는 모든 스레드에서 액세스할 수 있는 개체입니다. C++/WinRT 형식은 기본적으로 Agile이지만 옵트아웃할 수 있습니다. |
| [직접 할당 진단](./diag-direct-alloc.md) | 필요에 따라 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 도우미 제품군을 사용하는 대신 스택에서 구현 유형의 개체를 만드는 오류를 진단하는 데 도움이 되는 C++/WinRT 2.0 기능에 대해 자세히 설명합니다. |
| [구현 형식에 대한 확장 지점](./details-about-destructors.md) | C++/WinRT 2.0의 이러한 확장 지점을 사용하면 구현 형식의 소멸을 지연하고, 소멸 중에 안전하게 쿼리하고, 프로젝션 메서드에서 항목을 연결하거나 종료할 수 있습니다. |
| [간단한 C++/WinRT Windows UI 라이브러리 예제](./simple-winui-example.md) | C++/WinRT 프로젝트 내에서 WinUI를 위해 간단한 지원을 추가하는 과정을 안내합니다. |
| [C++/WinRT를 사용한 Windows 런타임 구성 요소](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md) | 이 항목에서는 C++/WinRT를 사용하여 Windows 런타임 언어로 빌드된 유니버설 Windows 앱에서 호출할 수 있는 구성 요소인 Windows 런타임 구성 요소를 만들고 사용하는 방법을 보여 줍니다. |

### <a name="topics-about-the-c-language"></a>C++ 언어 관련 항목

| 항목 | 설명 |
| - | - |
| [값 범주 및 해당 참조](./cpp-value-categories.md) | C++에 존재하는 값의 다양한 범주에 대해 설명합니다. 분명히 lvalues와 rvalues에 대해 들어 보았을 것이지만, 다른 종류도 있습니다. |

## <a name="important-apis"></a>중요 API
* [winrt 네임스페이스](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>관련 항목
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Windows 런타임 C++ 템플릿 라이브러리(WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)