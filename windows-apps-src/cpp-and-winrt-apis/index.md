---
author: stevewhims
description: C++/WinRT는 헤더 파일 기반 라이브러리로 구현된 WinRT(Windows 런타임) API를 위한 완전한 표준 C++17 언어 프로젝션입니다.
title: C++/WinRT
ms.author: stwhi
ms.date: 05/14/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션
ms.localizationpriority: medium
ms.openlocfilehash: e9c5cb8a0f81513038a18522c39f0138bb25ab27
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4694600"
---
# <a name="cwinrt"></a>C++/WinRT

[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 는 Windows 런타임 (WinRT) Api에 대 한 프로그램 완전 한 표준 최신 C + + 17 언어 프로젝션 헤더 파일 기반 라이브러리로 구현 및 최고 수준의 액세스를 사용 하 여 오늘날 Windows API를 제공 하도록 설계 되었습니다. C++/WinRT에서는 모든 표준과 호환되는 C++17 컴파일러를 통해 Windows 런타임 API를 작성하고 사용할 수 있습니다. Windows SDK는 C++/WinRT를 포함하며, 버전 10.0.17134.0(Windows 10, 버전 1803)에서 도입되었습니다.

C++/WinRT는 아름답고 빠른 Windows용 코드로 작성하려는 모든 개발자에게 적합합니다. 그 이유는 다음과 같습니다.

## <a name="the-case-for-cwinrt"></a>C++/WinRT의 경우
&nbsp;
> [!VIDEO https://www.youtube.com/embed/TLSul1XxppA]

C++ 프로그래밍 언어는 기업을 *비롯해* 높은 정확성, 품질 및 성능을 소중하게 생각하는 독립 소프트웨어 공급업체(ISV)에서도 응용 프로그램을 개발하는 데 사용됩니다. 예를 들어, 시스템 프로그래밍, 리소스가 제한적인 임베디드 및 모바일 시스템, 게임 및 그래픽, 장치 드라이버, 산업/과학/의학 분야 등 매우 광범위합니다.

언어적 관점에서 보면 C++는 항상 가벼우면서 다양한 형식을 지원하는 추상화를 작성하고 사용하는 데 초점을 맞춰왔습니다. 하지만 원시 포인터와 원시 루프, 그리고 C++98의 까다로운 메모리 할당 및 해제 이후로 언어가 급격하게 바뀌었습니다. 최신 C++(C++11 이후)는 명확한 아이디어의 표현, 간편성, 가독성, 그리고 대폭 낮춘 버그 발생 가능성까지 모두 갖추고 있습니다.

C++를 사용하여 Windows 런타임 API를 작성하고 활용할 때는 C++/WinRT가 있습니다. 이 Microsoft의 권장된 대체 합니다 [C + + CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) 언어 프로젝션 및 [Windows 런타임 c + + 템플릿 라이브러리 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live).

C++/WinRT를 사용할 때는 표준 C++ 데이터 형식, 알고리즘 및 키워드를 사용합니다. 프로젝션에 사용자 지정 데이터 형식이 있지만 대부분 경우 표준 형식과 서로 변환이 가능하기 때문에 사용자 지정 데이터 형식을 알아둘 필요는 없습니다. 또한 사용하는 데 익숙한 표준 C++ 언어 기능을 비롯해 이미 가지고 있는 소스 코드도 계속해서 사용할 수 있습니다. C++/WinRT에서는 Win32에서 UWP에 이르는 모든 C++ 응용 프로그램의 Windows 런타임 API를 매우 쉽게 호출할 수 있습니다.

C++/WinRT는 Windows 런타임에서 사용되는 다른 언어 옵션보다 더욱 뛰어날 뿐만 아니라 더욱 작은 용량의 이진 파일을 생성합니다. 심지어 ABI 인터페이스에서 직접 작성된 코드보다 우수합니다. 이는 추상화가 Visual C++ 컴파일러가 최적화된 최신 C++ 이디엄을 사용하기 때문입니다. 여기에는 매직 정적, 빈 기본 클래스, **strlen** 생략, 그리고 C++/WinRT 성능 개선이 목표인 최신 버전의 Visual C++에서 새롭게 제공하는 다수의 최적화 기능들이 포함됩니다.

### <a name="topics-about-cwinrt"></a>항목에 대해 C + + WinRT

| 항목 | 설명 |
| - | - |
| [C++/WinRT 소개](intro-to-using-cpp-with-winrt.md) | C++/WinRT 소개&mdash;Windows 런타임 API용 표준 C++ 언어 프로젝션 |
| [C++/WinRT 시작](get-started.md) | C++/WinRT 사용 속도를 높이기 위해 이 항목은 단순한 코드 예제를 안내합니다. |
| [새로운 C + + WinRT](news.md) | 뉴스와 변경 C + + WinRT 합니다. |
| [FAQ](faq.md) | C++/WinRT를 통해 Windows 런타임 API를 작성하거나 사용하면서 가질 수 있는 질문에 대해 답변을 제공합니다. |
| [문제 해결](troubleshooting.md) | 이번 항목에서 증상 문제 및 해결 방법을 나타낸 표는 새로운 코드를 자르거나 기존 앱을 이식할지 결정하는 데 도움이 될 수 있습니다. |
| [사진 편집기 C++/WinRT 샘플 응용 프로그램](photo-editor-sample.md) | 사진 편집기는 C++/WinRT 언어 프로젝션을 사용한 개발을 보여 주는 UWP 샘플 응용 프로그램입니다. 샘플 응용 프로그램을 사용하여 **사진** 라이브러리에서 사진을 검색한 다음 다양한 사진 효과를 사용하여 선택한 이미지를 편집합니다. | 
| [문자열 처리](strings.md) | C++/WinRT에서는 표준 C++ 전각 문자열 형식을 사용하여 Windows 런타임 API를 호출하거나, 혹은 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 형식을 사용할 수 있습니다. |
| [표준 C++ 데이터 형식 및 C++/WinRT](std-cpp-data-types.md) | C++/WinRT에서는 표준 C++ 데이터 형식을 사용하여 Windows 런타임 API를 호출할 수 있습니다. |
| [스칼라 값을 IInspectable로 박싱(boxing) 및 언박싱(unboxing)](boxing.md) | 스칼라 값은 **IInspectable**이 필요한 함수로 전달되기 전에 참조 클래스 개체 내에서 래핑되어야 합니다. 이러한 래핑 프로세스를 두고 값을 *박싱(boxing)* 한다고 합니다. |
| [C++/WinRT를 통한 API 사용](consume-apis.md) | 이 항목에서는 Windows, 외부 구성 요소 공급업체 또는 사용자 자신 중 어디에서 구현되었든 상관없이 C++/WinRT API를 사용하는 방법에 대해서 설명합니다. |
| [C++/WinRT를 통한 API 작성](author-apis.md) | 이 항목에서는 **winrt::implements** 기본 구조체를 직/간접적으로 사용하여 C++/WinRT API를 작성하는 방법에 대해서 설명합니다. |
| [C++/WinRT를 통한 오류 처리](error-handling.md) | 이 항목은 C++/WinRT로 프로그래밍하는 경우 오류를 처리하기 위한 전략을 소개합니다. |
| [대리자를 사용한 이벤트 처리](handle-events.md) | 이 항목에서는 C++/WinRT를 사용해 이벤트 처리 대리자를 등록하거나 취소하는 방법에 대해서 설명합니다. |
| [이벤트 작성](author-events.md) | 이 항목에서는 이벤트가 발생하는 런타임 클래스를 포함해 Windows 런타임 구성 요소를 작성하는 방법에 대해서 설명합니다. 또한 구성 요소를 사용하여 이벤트를 처리하는 앱에 대해서도 설명합니다. |
| [C++/WinRT로 작성된 컬렉션](collections.md) | C + + WinRT 함수와 많은 구현 및/또는 컬렉션을 전달 하려는 경우 시간과 노력을 저장 하는 기본 클래스를 제공 합니다. |
| [동시성 및 비동기 작업](concurrency.md) | 이 항목에서는 C++/WinRT를 통해 Windows 런타임 비동기 개체를 생성하고 사용하는 방법에 대해서 설명합니다. |
| [XAML 컨트롤, C++/WinRT 속성 바인딩](binding-property.md) | XAML 컨트롤에 효과적으로 바인딩되는 속성은 *관찰 가능한* 속성으로 알려져 있습니다. 이 항목에서는 관찰 가능한 속성을 구현하여 사용하는 방법과 XAML 컨트롤에 바인딩하는 방법에 대해서 설명합니다. |
| [XAML 항목 컨트롤, C++/WinRT 컬렉션 바인딩](binding-collection.md) | XAML 항목에 효과적으로 바인딩되는 컬렉션은 *관찰 가능한* 컬렉션으로 알려져 있습니다. 이 항목에서는 관찰 가능한 컬렉션을 구현하여 사용하는 방법과 XAML 항목에 바인딩하는 방법에 대해서 설명합니다. |
| [C++/WinRT을 사용한 XAML 사용자 지정(템플릿) 컨트롤](xaml-cust-ctrl.md) | 이 항목에서는 C +를 사용 하 여 간단한 사용자 지정 컨트롤을 만드는 과정을 단계별로 안내 + WinRT 합니다. 고유한 기능이 풍부 하 고 사용자 지정 가능한 UI 컨트롤을 만드는 정보를 여기에서 빌드할 수 있습니다. |
| [C++/WinRT로 작성된 COM 구성 요소 사용](consume-com.md) | 이 항목에서는 전체 Direct2D 코드 예제를 사용 하 여 C +를 사용 하는 방법을 보여를 + WinRT COM 클래스와 인터페이스를 사용 하도록 합니다. |
| [C++/WinRT으로 COM 구성 요소 작성](author-coclasses.md) | C + + WinRT 하는 데 유용한 클래식 COM 구성 요소를 작성 하면 Windows 런타임 클래스를 작성 하는 것 처럼 합니다. |
| [C++/WinRT와 C++/CX 사이의 상호 운용성](interop-winrt-cx.md) | 이번 항목에서는 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) 개체와 C++/WinRT 개체를 서로 변환하는 데 사용할 수 있는 두 가지 도우미 함수에 대해서 설명합니다. |
| [C++/CX에서 C++/WinRT로 이동](move-to-winrt-from-cx.md) | 이 항목은 C++/CX 코드를 C++/WinRT의 해당 코드에 포트하는 방법을 보여 줍니다. |
| [C++/WinRT와 ABI 사이의 상호 운용성](interop-winrt-abi.md) | 이번 항목에서는 응용 프로그램 이진 인터페이스(ABI)와 C++/WinRT 개체를 서로 변환하는 방법에 대해서 설명합니다. |
| [WRL에서 C++/WinRT로 이동](move-to-winrt-from-wrl.md) | 이 항목은 [Windows 런타임 C++ 템플릿 라이브러리(WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) 코드를 C++/WinRT의 해당 코드에 포트하는 방법을 보여 줍니다. |
| [강력한 및 약한 참조 C + + WinRT](weak-references.md) | Windows 런타임에서 참조 계산 시스템; 이러한 시스템의 성과 구별 하는 방법에 대 한 알 수 이기도 하 고 강력한 및 약한 참조 합니다. |
| [Agile 개체](agile-objects.md) | Agile 개체란 어떤 스레드에서든지 액세스할 수 있는 개체를 말합니다. C++/WinRT 형식은 기본적으로 Agile이지만 옵트아웃으로 선택하지 않을 수도 있습니다. |

### <a name="topics-about-the-c-language"></a>C + + 언어에 대 한 항목

| 항목 | 설명 |
| - | - |
| [값 범주 및 참조](cpp-value-categories.md) | 이 항목에서는 c + +에 존재 하는 값의 다양 한 범주를 설명 합니다. 여러분 봤 lvalue 및 rvalue, 이지만 다른 종류도 합니다. |

## <a name="important-apis"></a>중요 API
* [winrt 네임스페이스](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>관련 항목
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Windows 런타임 C++ 템플릿 라이브러리(WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)
