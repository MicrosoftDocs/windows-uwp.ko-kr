---
description: Windows 런타임 앱 개발자에게 XAML 언어와 XAML 개념을 소개하고 XAML에서 Windows 런타임 앱을 만드는 데 사용되는 개체를 선언하고 특성을 설정하는 다양한 방법을 설명합니다.
title: XAML 개요
ms.assetid: 48041B37-F1A8-44A4-BB8E-1D4DE30E7823
ms.date: 07/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 639f552a240cf8d28d1a2a0ce530315671128746
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8874633"
---
# <a name="xaml-overview"></a>XAML 개요

Windows 런타임 앱 개발자에게 XAML 언어와 XAML 개념을 소개하고 XAML에서 Windows 런타임 앱을 만드는 데 사용되는 개체를 선언하고 특성을 설정하는 다양한 방법을 설명합니다.

## <a name="what-is-xaml"></a>XAML이란?

XAML(Extensible Application Markup Language)은 선언적 언어입니다. 구체적으로 XAML은 여러 개체 간의 계층 구조 관계를 보여 주는 언어 구조와 형식 확장을 지원하는 백업 입력 규칙을 사용하여 개체를 초기화하고 개체 속성을 설정할 수 있습니다. 선언적 XAML 태그에서 보이는 UI 요소를 만들 수 있습니다. 그런 다음 XAML 파일별로 이벤트에 응답할 수 있는 별도의 코드 숨김 파일을 연결하고 XAML에 원래 선언된 개체를 조작할 수 있습니다.

XAML 언어는 개발 프로세스에서 다른 도구와 역할 간에 원본 교환을 지원합니다(예: 디자인 도구와 IDE 간에 또는 기본 개발자와 지역화 개발자 간에 XAML 원본 교환). XAML을 교환 형식으로 사용하면 디자이너 역할 및 개발자 역할의 분리 또는 통합 상태를 유지하고 앱을 생성하는 동안 디자이너와 개발자를 반복할 수 있습니다.

Windows 런타임 앱 프로젝트의 일부로 보는 경우 XAML 파일은 파일 이름 확장명이 .xaml인 XML 파일입니다.

## <a name="basic-xaml-syntax"></a>기본 XAML 구문

XAML은 XML을 기반으로 하는 기본 구문을 사용합니다. 의미상 유효한 XAML은 또한 유효한 XML이 됩니다. 그러나 XAML은 XML 1.0 사양의 XML에서 유효한 반면 다른 더 완전한 의미가 할당된 구문 개념을 사용합니다. 예를 들어 XAML은 특성의 문자열 값 또는 콘텐츠 대신 요소 내부에서 속성 값을 설정할 수 있는 *속성 요소 구문*을 지원합니다. 일반적인 XML의 경우 XAML 속성 요소는 이름에 점이 있는 요소이므로 일반 XML에 유효하지만 같은 의미를 가지지는 않습니다.

## <a name="xaml-and-microsoft-visual-studio"></a>XAML 및 Microsoft Visual Studio

Microsoft Visual Studio를 사용하면 XAML 텍스트 편집기와 그래픽 위주의 XAML 디자인 화면 모두에서 유효한 XAML 구문을 생성할 수 있습니다. 따라서 Visual Studio를 사용하여 앱에 대한 XAML을 작성할 때는 키 입력 방식 구문에 대해서는 크게 걱정할 필요가 없습니다. IDE는 자동 완성 힌트를 제공하거나, Microsoft IntelliSense 목록 및 드롭다운에 텍스트 제안을 표시하거나, 도구 상자에 UI 요소 라이브러리를 표시하거나, 다른 기술을 제공하여 유효한 XAML 구문을 작성하는 데 도움을 줍니다. XAML을 처음 사용하는 경우 참조 또는 다른 항목에서 XAML 구문에 대해 설명할 때 구문 규칙 특히, 제한 사항이나 선택 옵션을 설명하는 데 사용되는 용어에 대해 알아두면 좋습니다. XAML 구문에 대한 이러한 세부 요점에 대해서는 [XAML 구문 가이드](xaml-syntax-guide.md) 항목에서 별도로 설명합니다.

## <a name="xaml-namespaces"></a>XAML 네임스페이스

일반 프로그래밍에서 네임스페이스는 프로그래밍 엔터티에 대한 식별자의 해석 방법을 결정하는 구성 개념입니다. 네임스페이스를 사용하면 프로그래밍 프레임워크가 사용자 선언 식별자를 프레임워크 선언 식별자와 분리하고 네임스페이스 자격 부여를 통해 식별자를 명확히 하고 이름 범위 규칙을 강제하는 등의 작업이 가능합니다. XAML에는 이 용도로 XAML 언어에 제공되는 자체 XAML 네임스페이스 개념이 있습니다. XAML에서 XML 언어 네임스페이스 개념을 적용하고 확장하는 방법은 다음과 같습니다.

-   XAML은 네임스페이스 선언을 위해 예약된 XML 특성 **xmlns**를 사용합니다. 이 특성의 값은 일반적으로 XML에서 상속된 규칙인 URI(Uniform Resource Identifier)입니다.
-   XAML은 선언에 접두사를 사용하여 비기본 네임스페이스를 선언하고, 요소 및 특성의 접두사 사용은 해당 네임스페이스를 참조합니다.
-   XAML에는 사용 또는 선언에 접두사가 없을 때 사용되는 네임스페이스인 기본 네임스페이스라는 개념이 있습니다. 기본 네임스페이스는 XAML 프로그래밍 프레임워크별로 서로 다르게 정의할 수 있습니다.
-   네임스페이스 정의는 XAML 파일 또는 구문을 통해 부모 요소에서 자식 요소로 상속됩니다. 예를 들어 XAML 파일의 루트 요소에서 네임스페이스를 정의하는 경우 해당 파일 내의 모든 요소가 네임스페이스 정의를 상속합니다. 페이지 내의 요소가 네임스페이스를 다시 정의하는 경우 해당 요소의 하위 항목이 새 정의를 상속합니다.
-   요소 특성은 해당 요소의 네임스페이스를 상속합니다. XAML 특성에 접두사를 표시하는 것은 일반적이지 않습니다.

XAML 파일은 거의 항상 해당 루트 요소에서 기본 XAML 네임스페이스를 선언합니다. 기본 XAML 네임스페이스는 접두사로 자격을 부여하지 않고 선언할 수 있는 요소를 정의합니다. 일반적인 Windows 런타임 앱 프로젝트의 경우, 이 기본 네임스페이스에는 기본 컨트롤, 텍스트 요소, XAML 그래픽 및 애니메이션, 데이터 바인딩 및 스타일 지원 유형 등과 같이 UI 정의에 사용되는 Windows 런타임에 대한 모든 기본 제공 XAML 어휘가 포함되어 있습니다. Windows 런타임 앱에 대해 작성하는 대부분의 XAML은 공용 UI 요소를 참조할 때 XAML 네임스페이스 및 접두사의 사용을 방지할 수 있습니다.

다음은 앱의 초기 페이지에 대해 템플릿으로 만든 [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) 루트를 보여 주는 코드 조각입니다(여는 태그만 표시하여 간소화함). 기본 네임스페이스를 선언하며 **x** 네임스페이스도 선언합니다(다음에 설명할 예정).

```xml
<Page
    x:Class="Application1.BlankPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
>
```

## <a name="the-xaml-language-xaml-namespace"></a>XAML 언어 XAML 네임스페이스

거의 모든 Windows 런타임 XAML 파일에 선언되는 한 가지 특정 XAML 네임스페이스는 XAML 언어 네임스페이스입니다. 이 네임스페이스에는 XAML 언어 및 언어 사양으로 정의된 요소 및 개념이 포함되어 있습니다. 일반적으로 XAML 언어 XAML 네임스페이스는 "x" 접두사에 매핑됩니다. Windows 런타임 앱 프로젝트의 기본 프로젝트 및 파일 템플릿에서는 항상 기본 XAML 네임스페이스(접두사 없는 `xmlns=`)와 XAML 언어 XAML 네임스페이스(접두사 "x")를 모두 루트 요소의 일부로 정의합니다.

"x" 접두사/XAML 언어 XAML 네임스페이스는 XAML에서 자주 사용되는 여러 프로그래밍 구조를 포함합니다. 가장 일반적인 구조는 다음과 같습니다.

| 용어 | 설명 |
|------|-------------|
| [x:Key](x-key-attribute.md) | XAML [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)의 각 리소스에 대해 고유한 사용자 정의 키를 설정합니다. 키의 토큰 문자열은 **StaticResource** 태그 확장에 대한 인수이며, 나중에 이 키를 사용하여 앱 XAML의 다른 위치에서 달리 사용된 XAML의 XAML 리소스를 검색합니다. |
| [x:Class](x-class-attribute.md) | XAML 페이지에 대한 코드 숨김을 제공하는 클래스의 코드 네임스페이스 및 코드 클래스 이름을 지정합니다. 이 이름은 앱을 빌드할 때 빌드 작업에 의해 만들어지거나 조인되는 클래스를 지정합니다. 이 빌드 작업은 XAML 태그 컴파일러를 지원하고 앱이 컴파일될 때 태그와 코드 숨김을 결합합니다. XAML 페이지에 대해 코드 숨김을 지원하려면 이런 클래스가 있어야 합니다. 기본 Windows 런타임 활성화 모델의 [**Window.Content**](https://msdn.microsoft.com/library/windows/apps/br209051)입니다. |
| [x:Name](x-name-attribute.md) | XAML에 정의된 개체 요소를 처리한 후 런타임 코드에 존재하는 인스턴스에 대한 런타임 개체 이름을 지정합니다. XAML에서 **x:Name**을 설정하는 것을 코드에서 명명된 변수를 선언하는 것과 같다고 생각하면 됩니다. 나중에 알게 되겠지만, XAML이 Windows 런타임 앱의 구성 요소로 로드될 때 이와 똑같은 상황이 발생합니다. <br/><div class="alert">**참고** [**FrameworkElement.Name**](https://msdn.microsoft.com/library/windows/apps/br208735) 프레임 워크에서 비슷한 속성 이지만 모든 요소가를 지원 합니다. 해당 요소 형식에서 **FrameworkElement.Name**이 지원되지 않는 경우 요소 식별을 위해 **x:Name**을 사용합니다. |
| [x:Uid](x-uid-directive.md) | 해당 속성 값 중 일부에 대해 지역화된 리소스를 사용해야 하는 요소를 식별합니다. **x:Uid**를 사용하는 방법에 대한 자세한 내용은 [빠른 시작: UI 리소스 변환](https://msdn.microsoft.com/library/windows/apps/xaml/hh965329)을 참조하세요. |
| [XAML 기본 데이터 형식](xaml-intrinsic-data-types.md) | 이 형식은 특성 또는 리소스에 필요할 경우 단순 값 형식에 대한 값을 지정할 수 있습니다. 이러한 내부 형식은 일반적으로 각 프로그래밍 언어의 내부 정의의 일부로 정의되는 단순 값 형식에 해당합니다. 예를 들어 [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/br210320) 스토리보드 시각적 상태에서 사용할 **true** 부울 값을 나타내는 개체가 필요할 수 있습니다. XAML 해당 값의 경우 **x:Boolean** 내부 형식을 다음과 같이 개체 요소로 사용합니다. <code>&lt;x:Boolean&gt;True&lt;/x:Boolean&gt;</code> | 

XAML 언어 XAML 네임스페이스의 다른 프로그래밍 구조도 있지만 잘 사용되지 않습니다.

## <a name="mapping-custom-types-to-xaml-namespaces"></a>XAML 네임스페이스에 사용자 지정 유형 매핑

XAML이 언어로서 가진 가장 강력한 측면 중 하나는 Windows 런타임 앱에 맞게 XAML 어휘를 쉽게 확장할 수 있다는 점입니다. 앱의 프로그래밍 언어로 사용자 지정 유형을 고유하게 정의한 다음 XAML 태그에서 사용자 지정 유형을 참조할 수 있습니다. 사용자 지정 유형을 통한 확장 지원은 기본적으로 XAML 언어 작동 방식에 기본적으로 제공됩니다. 프레임워크 또는 앱 개발자는 XAML이 참조하는 백업 개체를 만들어야 합니다. 프레임워크 개발자나 앱 개발자는 어휘의 개체가 표현하는 사항에 대한 사양을 준수하지 않아도 되며 기본 XAML 구문 규칙을 벗어나지 않습니다. XAML-언어 XAML 네임스페이스 형식이 담당해야 하는 사항에 대해 어느 정도 기대치가 있지만, Windows 런타임에서 필수적인 사항이 모두 지원됩니다.

Windows 런타임 핵심 라이브러리와 메타데이터 이외의 다른 라이브러리에서 가져온 유형에 대해 XAML을 사용하는 경우 접두사를 사용하여 XAML 네임스페이스를 선언하고 매핑해야 합니다. 요소 사용에서 이 접두사를 사용하여 어휘에서 정의된 유형을 참조합니다. 일반적으로 루트 요소에서 다른 XAML 네임스페이스 정의와 함께 접두사 매핑을 **xmlns** 특성으로 선언합니다.

사용자 지정 유형을 참조하는 고유한 네임스페이스 정의를 만들려면 먼저 키워드 **xmlns:** 를 지정한 다음 원하는 접두사를 지정합니다. 이 특성의 값에는 값의 첫 번째 부분으로 **using:** 키워드가 포함되어 있어야 합니다. 값의 나머지 부분은 사용자 지정 유형이 포함된 특정 코드 지원 네임스페이스를 이름으로 참조하는 문자열 토큰입니다.

접두사는 태그의 나머지 부분에 있는 해당 XAML 네임스페이스를 참조하는 데 사용되는 태그 토큰을 정의합니다. XAML 네임스페이스 내에서 참조되는 엔터티와 접두사 사이에는 콜론(:)이 있습니다.

예를 들어 `myTypes` 접두사를 `myCompany.myTypes` 네임스페이스에 매핑하는 특성 구문은 `    xmlns:myTypes="using:myCompany.myTypes"`이며 대표적인 요소의 사용 예는 다음과 같습니다. `<myTypes:CustomButton/>`

사용자 지정 형식에 대 한 매핑 XAML 네임 스페이스에 대 한 자세한 내용은 VisualC + + 구성 요소 확장에 대 한 특수 고려 사항을 포함 하 여 (C + + CX), [XAML 네임 스페이스 및 네임 스페이스 매핑을](xaml-namespaces-and-namespace-mapping.md)참조 하세요.

## <a name="other-xaml-namespaces"></a>다른 XAML 네임스페이스

"d"(디자이너 네임스페이스) 및 "mc"(태그 호환성) 접두사를 정의하는 XAML 파일이 표시되는 경우가 있습니다. 일반적으로 이러한 파일은 인프라를 지원하거나, 디자인 타임 도구의 시나리오를 가능하게 하기 위한 것입니다. 자세한 내용은 [XAML 네임스페이스 항목의 "기타 XAML 네임스페이스" 섹션](xaml-namespaces-and-namespace-mapping.md#other-XAML-namespaces)을 참조하세요.

## <a name="markup-extensions"></a>태그 확장

태그 확장은 Windows 런타임 XAML 구현에서 주로 사용되는 XAML 언어 개념입니다. 태그 확장은 종종 XAML 파일이 지원 형식을 기반으로 요소를 선언하지 않는 값이나 동작에 액세스하도록 지원하는 일부 종류의 "바로 가기"를 나타냅니다. 일부 태그 확장은 구문 간소화 또는 서로 다른 XAML 파일 사이의 팩터링을 위해 일반 문자열 또는 추가 중첩 요소를 사용하여 속성을 설정할 수 있습니다.

XAML 특성 구문에서 "{" 및 "}" 중괄호는 XAML 태그 확장 사용법을 나타냅니다. 이 사용법에서는 XAML을 일반 특성 값 처리에서 이스케이프하여 리터럴 문자열 또는 문자열로 직접 변환 가능한 값으로 처리하지 않도록 지시합니다. 대신 XAML 파서는 특정 태그 확장을 위한 동작을 제공하고 XAML 파서에 필요한 대체 개체 또는 동작 결과를 제공하는 코드를 호출합니다. 태그 확장은 태그 확장 이름을 따르며 중괄호 안에 포함된 인수를 취할 수 있습니다. 일반적으로 평가된 태그 확장은 개체 반환 값을 제공합니다. 구분 분석 과정에서, 이 반환 값은 원본 XAML에서 태그 확장이 사용되었던 개체 트리 위치에 삽입됩니다.

Windows 런타임 XAML은 기본 XAML 네임스페이스 아래에 정의되고 Windows 런타임 XAML 파서에서 인식되는 다음과 같은 태그 확장을 지원합니다.

-   [{xBind}](x-bind-markup-extension.md): 컴파일 타임에 생성하는 특수 용도의 코드를 실행하여 런타임까지 속성 평가를 지연하는 데이터 바인딩을 지원합니다. 이 태그 확장은 다양한 인수를 지원합니다.
-   [{Binding}](binding-markup-extension.md): 범용 런타임 개체 검사를 실행하여 런타임까지 속성 평가를 지연하는 데이터 바인딩을 지원합니다. 이 태그 확장은 다양한 인수를 지원합니다.
-   [{StaticResource}](staticresource-markup-extension.md): [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)에 정의되는 참조 리소스 값을 지원합니다. 이 리소스는 다른 XAML 파일에 있을 수 있지만 궁극적으로 로드할 때 XAML 파서에서 검색할 수 있어야 합니다. `{StaticResource}` 사용 인수는 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)의 키가 지정된 리소스의 키(이름)를 식별합니다.
-   [{ThemeResource}](themeresource-markup-extension.md): [{StaticResource}](staticresource-markup-extension.md)와 비슷하지만 런타임 테마 변경에 응답할 수 있습니다. {ThemeResource}는 Windows 런타임 기본 XAML 템플릿에서 상당히 자주 나타납니다. 이 템플릿 중 대부분이 앱 실행 중 사용자의 테마 전환과 호환되도록 설계되기 때문입니다.
-   [{TemplateBinding}](templatebinding-markup-extension.md): XAML의 컨트롤 템플릿과 런타임에서 이 템플릿의 최종적인 사용을 지원하는 [{Binding}](binding-markup-extension.md)의 특별한 경우입니다.
-   [{RelativeSource}](relativesource-markup-extension.md): 템플릿 기반 상위 항목에서 값이 제공되는 특정 형식의 템플릿 바인딩을 지원합니다.
-   [{CustomResource}](customresource-markup-extension.md): 고급 리소스 조회 시나리에 해당합니다.

Windows 런타임은 또한 [{x:Null} 태그 확장](x-null-markup-extension.md)을 지원합니다. XAML에서 이 태그 확장을 사용하여 [**Nullable**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx) 값을 **null**로 설정합니다. 예를 들어 **null**을 결정할 수 없는 확인 상태로 해석하는 [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316)에 대한 컨트롤 템플릿에서 이 태그 확장을 사용할 수 있습니다("결정할 수 없는" 시각적 상태 트리거).

태그 확장은 일반적으로 앱에 대한 개체 그래프의 일부 다른 부분에서 기존 인스턴스를 반환하거나 값을 런타임까지 지연합니다. 태그 확장을 특성 값으로 사용할 수 있으며 일반적인 사용 예이므로 그렇지 않은 경우 속성 요소 구문이 필요할 수 있는 참조 유형 속성에 대한 값을 제공하는 태그 확장을 종종 볼 수 있습니다.

예를 들어 다음은 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)에서 다시 사용할 수 있는 [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849)을 참조하기 위한 구문입니다. `<Button Style="{StaticResource SearchButtonStyle}"/>`. [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849)은 단순 값이 아니라 참조 형식이므로 `{StaticResource}`를 사용하지 않을 경우 [**FrameworkElement.Style**](https://msdn.microsoft.com/library/windows/apps/br208743) 속성을 설정하려면 XAML 내부에 `<Button.Style>` 속성 요소 및 `<Style>` 정의가 필요합니다.

태그 확장을 사용하면 XAML에서 설정할 수 있는 모든 속성을 잠재적으로 특성 구문에서 설정할 수 있게 됩니다. 직접 개체 인스턴스화에 대한 특성 구문을 지원하지 않더라도 특성 구문을 사용하여 속성에 대한 참조 값을 제공할 수 있습니다. 값 형식 또는 새로 만든 참조 형식을 통해 XAML 속성을 채워야 한다는 일반 요구 사항을 지연하는 특정 동작을 사용할 수 있습니다.

세부적으로, 다음 XAML 예제는 특성 구문을 사용하여 [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250)의 [**Style**](https://msdn.microsoft.com/library/windows/apps/br208743) 속성 값을 설정합니다. [**Style**](https://msdn.microsoft.com/library/windows/apps/br208743) 속성은 기본적으로 특성 구문 문자열을 사용하여 만들 수 없는 참조 형식인 [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) 클래스의 인스턴스를 가져옵니다. 하지만 이 경우 특성은 [StaticResource](staticresource-markup-extension.md) 태그 확장을 참조합니다. 이 태그 확장은 처리되면 앞에서 리소스 사전에서 키가 지정된 리소스로 정의한 **Style** 요소에 대한 참조를 반환합니다.

```xml
<Canvas.Resources>
  <Style TargetType="Border" x:Key="PageBackground">
    <Setter Property="BorderBrush" Value="Blue"/>
    <Setter Property="BorderThickness" Value="5"/>
  </Style>
</Canvas.Resources>
...
<Border Style="{StaticResource PageBackground}">
  ...
</Border>
```

태그 확장을 중첩시킬 수 있습니다. 가장 안쪽 태그 확장이 가장 먼저 평가됩니다.

태그 확장으로 인해 특성의 리터럴 "{" 값에 특수 구문이 필요합니다. 자세한 내용은 [XAML 구문 가이드](xaml-syntax-guide.md)를 참조하세요.

## <a name="events"></a>이벤트

XAML은 개체와 개체 속성에 대한 선언적 언어이지만, 이벤트 처리기를 태그의 개체에 연결하는 구문도 포함합니다. XAML 이벤트 구문은 Windows 런타임 프로그래밍 모델을 통해 XAML로 선언된 이벤트를 통합할 수 있습니다. 이벤트가 처리되는 개체에 대한 특성 이름으로 이벤트 이름을 지정합니다. 특성 값에 대해서는 코드에 정의된 이벤트 처리기 함수의 이름을 지정합니다. XAML 프로세서는 이 이름을 사용하여 로드된 개체 트리에서 위임 표현을 만들고 지정된 처리기를 내부 처리기 목록에 추가합니다. 거의 모든 Windows 런타임 앱은 태그와 코드 숨김 소스를 모두 사용하여 정의됩니다.

다음은 간단한 예제입니다. [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 클래스가 이름이 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737)인 이벤트를 지원합니다. 사용자의 **Button** 클릭 후에 호출되는 코드를 실행하는 **Click**에 대한 처리기를 작성할 수 있습니다. XAML에서 **Click**을 **Button**에 대한 특성으로 지정합니다. 특성 값으로 처리기의 메서드 이름인 문자열을 제공합니다.

```xml
<Button Click="showUpdatesButton-Click">Show updates</Button>
```

컴파일하면 이제 컴파일러가 이름이 `showUpdatesButton-Click`인 메서드가 코드 숨김 파일에 정의되어 있고 네임스페이스에서 XAML 페이지의 [x:Class](x-class-attribute.md) 값에 선언되어 있다고 예상합니다. 또한 이 메서드는 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 이벤트의 위임 계약을 충족해야 합니다. 예:

```csharp
namespace App1
{
    public sealed partial class MainPage: Page {
        ...
        private void showUpdatesButton_Click (object sender, RoutedEventArgs e) {
            //your code
        }
    }
}
```

```vb
' Namespace included at project level
Public NotInheritable Class MainPage
    Inherits Page
        ...
        Private Sub showUpdatesButton_Click (sender As Object, e As RoutedEventArgs e)
            ' your code
        End Sub
    ...
End Class
```

```cppwinrt
namespace winrt::App1::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        ...
        void showUpdatesButton_Click(Windows::Foundation::IInspectable const&, Windows::UI::Xaml::RoutedEventArgs const&);
    };
}
```

```cpp
// .h
namespace App1
{
    public ref class MainPage sealed {
        ...
    private:
        void showUpdatesButton_Click(Object^ sender, RoutedEventArgs^ e);
    };
}
```

프로젝트 내에서 XAML은 .xaml 파일로 기록되며, 원하는 언어(C#, Visual Basic, C++/CX)를 사용하여 코드 숨김 파일을 작성할 수 있습니다. 프로젝트에 대한 빌드 작업 중에 XAML 파일을 태그로 컴파일할 때 XAML 페이지 루트 요소의 [x:Class](x-class-attribute.md) 특성으로 네임스페이스와 클래스를 지정하여 각 XAML 페이지에 대한 XAML 코드 숨김 파일 위치를 식별합니다. XAML에서 이러한 메커니즘을 사용하는 방법과 프로그래밍 및 응용 프로그램 모델과의 관계에 대한 자세한 내용은 [이벤트 및 라우트된 이벤트 개요](events-and-routed-events-overview.md)를 참조하세요.

**참고**C + /CX의 경우 개의 두 개의 코드 숨김 파일, 하나는 헤더 (. xaml.h) 이며 다른 구현 (. xaml.cpp). 구현은 헤더를 참조하고, 엄밀히 말하자면 헤더는 코드 숨김 연결의 진입점을 나타냅니다.

## <a name="resource-dictionaries"></a>리소스 사전

[**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)를 만드는 것은 일반적으로 XAML 페이지의 영역 또는 개별 XAML 파일로 리소스 사전을 작성하여 수행하는 일반 작업입니다. 리소스 사전 및 이 사전을 사용하는 방법은 이 항목의 범위를 벗어나는 더 큰 개념 분야입니다. 자세한 내용은 [ResourceDictionary 및 XAML 리소스 참조](https://msdn.microsoft.com/library/windows/apps/mt187273)를 확인하세요.

## <a name="xaml-and-xml"></a>XAML 및 XML

XAML 언어는 기본적으로 XML 언어를 기반으로 합니다. 하지만 XAML은 XML을 현저하게 확장합니다. 특히 백업 형식 개념과의 관계로 인해 스키마 개념을 완전히 다르게 처리하고 연결된 멤버, 태그 확장 등과 같은 언어 요소를 추가합니다. **xml:lang**는 XAML에서도 유효하지만, 구문 분석 동작이 아닌 런타임에 영향을 주며 일반적으로 프레임워크 수준 속성으로 별칭이 지정됩니다. 자세한 내용은 [**FrameworkElement.Language**](https://msdn.microsoft.com/library/windows/apps/hh702066)를 참조하세요. **xml:base**는 태그에 유효하지만 파서에서는 무시됩니다. **xml:space**는 유효하지만 [XAML 및 공백](xaml-and-whitespace.md) 항목에 설명된 시나리오에만 적합합니다. **encoding** 특성은 XAML에서 유효합니다. UTF-8 및 UTF-16 인코딩만 지원됩니다. UTF-32 인코딩은 지원되지 않습니다.

###  <a name="case-sensitivity-in-xaml"></a>XAML의 대/소문자 구분

XAML은 대/소문자를 구분합니다. 이는 대/소문자를 구분하는 XML을 기반으로 하는 XAML의 다른 결과입니다. XAML 요소 및 특성의 이름은 대/소문자를 구분합니다. 특성 값은 대/소문자를 구분할 수 있지만, 특정 속성에 대한 특성 값이 처리되는 방법에 따라 달라집니다. 예를 들어 특성 값이 열거형의 멤버 이름을 선언하는 경우 멤버 이름 문자열의 형식을 변환하여 열거형 멤버 값을 반환하는 기본 제공 동작은 대/소문자를 구분하지 않습니다. 반대로 **Name** 속성 값과 **Name** 속성이 선언하는 이름을 기반으로 개체 작업을 하는 유틸리티 메서드는 이름 문자열의 대/소문자를 구분합니다.

## <a name="xaml-namescopes"></a>XAML 이름 범위

XAML 언어는 XAML namescope 개념을 정의합니다. XAML namescope 개념은 XAML 프로세서에서 XAML 요소에 적용된 **x:Name** 또는 **Name** 값을 처리하는 방법 특히, 이름을 고유한 식별자로 사용해야 하는 범위에 영향을 줍니다. XAML namescopes에 대한 자세한 내용은 [XAML namescopes](xaml-namescopes.md)를 참조하세요.

## <a name="the-role-of-xaml-in-the-development-process"></a>개발 프로세스에서 XAML의 역할

XAML은 앱 개발 프로세스에서 여러 중요한 역할을 합니다.

-   C#, Visual Basic 또는 C++/CX.를 사용하여 프로그래밍하는 경우 XAML은 앱 UI와 해당 UI의 요소를 선언하는 데 사용되는 기본 형식입니다. 처음에 표시된 UI의 경우 일반적으로 프로젝트에서 하나 이상의 XAML 파일이 앱의 페이지를 은유적으로 표시합니다. 추가 XAML 파일이 탐색 UI의 추가 페이지를 선언할 수 있습니다. 다른 XAML 파일은 템플릿이나 스타일 같은 리소스를 선언할 수 있습니다.
-   XAML 형식은 앱의 컨트롤 및 UI에 적용되는 스타일 및 템플릿을 선언하는 데 사용됩니다.
-   기존 컨트롤에 템플릿을 지정하거나 기본 템플릿을 컨트롤 패키지의 일부로 제공하는 컨트롤을 정의한 경우 스타일 및 템플릿을 사용할 수 있습니다. 스타일 및 템플릿을 정의하는 데 사용하는 경우 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 루트를 사용하여 관련 XAML을 별개의 XAML 파일로 선언하는 경우가 많습니다.
-   XAML은 앱 UI를 만들고 서로 다른 디자이너 앱 사이에서 UI 디자인을 교환하는 데 도움을 주는 디자이너 지원의 일반적인 형식입니다. 특히 앱의 XAML은 다른 XAML 디자인 도구(또는 도구 내의 디자인 화면) 간에 교환될 수 있습니다.
-   여러 다른 기술도 XAML에서 기본 UI를 정의합니다. WPF(Windows Presentation Foundation) XAML 및 Microsoft Silverlight XAML과 관련하여 Windows 런타임 앱에 대한 XAML은 공유되는 기본 XAML 네임스페이스에 대한 동일한 URI를 사용합니다. Windows 런타임 앱의 XAML 용어 모음은 Silverlight에서도 사용되고 WPF의 경우 약간 적은 범위에서 사용되는 UI용 XAML 용어 모음과 많은 부분에서 겹칩니다. 따라서 XAML은 마찬가지로 XAML을 사용하는 이전 기술에 대해 원래 정의된 UI에 효율적인 마이그레이션 경로를 제공합니다.
-   XAML은 UI의 시각적 모양을 정의하고 관련된 코드 숨김 파일은 논리를 정의합니다. 코드 숨김의 논리를 변경하지 않고도 UI 디자인을 조정할 수 있습니다. XAML은 디자이너와 개발자 간 워크플로를 단순화합니다.
-   XAML 언어에 대한 비주얼 디자이너 및 디자인 화면 지원이 풍부하여 XAML은 초기 개발 단계에서 빠른 UI 프로토타입 지정을 지원합니다.

개발 프로세스에서 수행하는 고유 역할에 따라 XAML을 별로 많이 조작하지 않을 수 있습니다. XAML 파일을 조작하는 정도는 사용하는 개발 환경, 도구 상자 및 속성 편집기 같은 대화형 디자인 환경 기능을 사용하는지 여부 그리고 Windows 런타임 앱의 범위 및 목적에 따라 다릅니다. 하지만 앱 개발 중에는 텍스트 또는 XML 편집기를 사용하여 요소 수준에서 XAML 파일을 편집할 가능성이 큽니다. 이러한 정보를 사용하여 개발자는 확신을 가지고 텍스트 또는 XML 표현의 XAML을 편집할 수 있으며 도구, 태그 컴파일 작업 또는 Windows 런타임 앱의 런타임 단계에서 사용되는 경우 해당 XAML 파일의 선언 및 목적 유효성을 관리할 수 있습니다.

## <a name="optimize-your-xaml-for-load-performance"></a>로드 성능을 위해 XAML을 최적화합니다.

다음은 성능을 위한 모범 사례를 활용하여 XAML에서 UI 요소를 정의하기 위한 몇 가지 팁입니다. 이 팁의 상당수는 XAML 리소스 사용과 관련이 있지만 편의상 XAML에 대한 전반적인 개요에서 설명합니다. XAML 리소스에 대한 자세한 내용은 [ResourceDictionary 및 XAML 리소스 참조](https://msdn.microsoft.com/library/windows/apps/mt187273)를 확인하세요. XAML에서 사용하지 말아야 하는 성능 저하 예를 의도적으로 보여 주는 XAML을 포함하여 성능에 대한 추가 정보를 보려면 [XAML 태그 최적화](https://msdn.microsoft.com/library/windows/apps/mt204779)를 참조하세요.

-   XAML에서 같은 색 브러시를 자주 사용하는 경우에는 매번 명명된 색을 특성 값으로 사용하지 말고 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962)를 리소스로 정의합니다.
-   여러 UI 페이지에서 같은 리소스를 사용하는 경우에는 각 페이지가 아니라 [**Application.Resources**](https://msdn.microsoft.com/library/windows/apps/br242338)에서 리소스를 정의하는 것이 좋습니다. 이와 반대로 특정 리소스를 한 페이지에서만 사용하는 경우에는 **Application.Resources**에서 리소스를 정의하지 말고 대신, 필요한 페이지에 대해서만 리소스를 정의합니다. 앱 디자인 도중의 XAML 팩터링 및 XAML 구문 분석 과정의 성능을 위해 좋은 방법입니다.
-   앱에 패키징되는 리소스의 경우 사용되지 않는 리소스(키가 있지만 키를 사용하는 앱에 [StaticResource](staticresource-markup-extension.md) 참조가 없는 리소스)가 있는지 확인합니다. 앱을 릴리스하기 전에 XAML에서 이 리소스를 제거합니다.
-   디자인 리소스([**MergedDictionaries**](https://msdn.microsoft.com/library/windows/apps/br208801))를 제공하는 별도의 XAML 파일을 사용하고 있다면 사용되지 않는 리소스를 주석으로 처리하거나 이 파일에서 제거하는 것이 좋습니다. 두 개 이상의 앱에서 사용 중이거나 모든 앱의 공통 리소스를 제공하는 공유 XAML 시작 지점이 있는 경우에도 매번 XAML 리소스를 패키지화하고 잠재적으로 로드해야 하는 것은 여전히 개발자의 앱입니다.
-   컴퍼지션에 필요 없는 UI 요소를 정의하지 말고, 가능한 한 항상 기본 컨트롤 템플릿을 사용하세요. 이 템플릿은 테스트를 거쳤으며 로드 성능이 검증되었습니다.
-   UI 요소를 의도적으로 과도하게 그리지 말고 [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250)와 같은 컨테이너를 사용합니다. 기본적으로, 같은 픽셀을 여러 번 그리지 않습니다. 과도한 그리기 및 이를 테스트하는 방법에 대한 자세한 내용은 [**DebugSettings.IsOverdrawHeatMapEnabled**](https://msdn.microsoft.com/library/windows/apps/hh701823)를 참조하세요.
-   [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 또는 [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705)에 대한 기본 항목 템플릿을 사용하세요. 여기에는 많은 목록 항목에 대해 시각적 트리를 빌드할 때 성능 문제를 해결하는 특수한 **Presenter** 논리가 있습니다.

## <a name="debugging-xaml"></a>XAML 디버깅

XAML은 생성 언어이기 때문에 Microsoft Visual Studio 내에서의 디버깅에 대한 일반적인 몇 가지 전략을 사용할 수 없습니다. 예를 들어, XAML 파일 내에 중단점을 설정할 수 있는 방법은 없습니다. 그러나 아직 앱을 개발하는 동안 UI 정의나 다른 XAML 태그에서 발생하는 문제를 디버그하는 데 도움이 되는 다른 기술이 있습니다.

XAML 파일에 문제가 있는 경우 가장 일반적인 결과는 시스템 또는 앱에 XAML 구문 분석 예외가 발생하는 것입니다. XAML 구문 분석 예외가 있을 때마다, XAML 파서가 로드한 XAML은 유효한 개체 트리를 만들지 못합니다. 일부 경우(예: XAML이 루트 시각 효과로 로드된 응용 프로그램의 첫 번째 "페이지"를 나타내는 경우) XAML 구문 분석 예외를 복구할 수 없습니다.

XAML은 흔히 Visual Studio와 같은 IDE 및 해당 XAML 디자인 화면 중 하나 내에서 편집됩니다. 흔히 Visual Studio에서는 XAML 편집 시 디자인 타임 유효성 검사 및 XAML 원본의 오류 검사를 제공할 수 있습니다. 예를 들어, 잘못된 특성 값을 입력하는 즉시 XAML 텍스트 편집기에 "물결선"이 표시되므로 UI 정의에 문제가 있는지 확인하기 위해 XAML 컴파일 단계까지 기다릴 필요도 없습니다.

앱이 실제로 실행되고 나면, 디자인 타임에 XAML 구문 분석 오류가 검색되지 않은 채로 지나간 경우 이러한 오류는 CLR(공용 언어 런타임)에 의해 [**XamlParseException**](https://msdn.microsoft.com/library/windows/apps/hh673774)으로 보고됩니다. 런타임 **XamlParseException**에 대해 수행할 수 있는 작업에 대한 자세한 내용은 [C# 또는 Visual Basic으로 작성된 Windows 런타임 앱의 예외 처리](https://msdn.microsoft.com/library/windows/apps/dn532194)를 참조하세요.

**참고**앱을 사용 하 여 C + + /CX 코드를 특정 [**XamlParseException**](https://msdn.microsoft.com/library/windows/apps/hh673774)다운로드 하지 않습니다. 하지만 **XamlParseException**과 마찬가지로, 예외의 메시지는 오류의 근원이 XAML과 관련이 있음을 명확히 하며 XAML 파일의 줄 수와 같은 컨텍스트 정보를 포함합니다.

Windows 런타임 앱 디버깅에 대한 자세한 내용은 [디버그 세션 시작](https://msdn.microsoft.com/library/windows/apps/xaml/hh781607.aspx)을 참조하세요.