---
description: Windows 런타임 app developer 사용자에 게 XAML 언어 및 XAML 개념을 소개 하 고, Windows 런타임 응용 프로그램을 만드는 데 사용 되는 개체를 선언 하 고 특성을 XAML로 설정 하는 다양 한 방법을 설명 합니다.
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
ms.openlocfilehash: 792712256e36b40cd376f0e378bb110ab33bc0fb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173737"
---
# <a name="xaml-overview"></a>XAML 개요

이 문서에서는 Windows 런타임 app developer 사용자를 위한 XAML 언어 및 XAML 개념을 소개 하 고, 개체를 선언 하 고 특성을 설정 하는 다양 한 방법에 대해 설명 합니다 .이는 Windows 런타임 앱을 만드는 데 사용 됩니다.

## <a name="what-is-xaml"></a>XAML이란?

Extensible Application Markup Language (XAML)은 선언적 언어입니다. 특히 XAML은 여러 개체 간의 계층 관계를 보여 주는 언어 구조와 형식의 확장을 지 원하는 지원 형식 규칙을 사용 하 여 개체를 초기화 하 고 개체의 속성을 설정할 수 있습니다. 선언적 XAML 태그에 표시 되는 UI 요소를 만들 수 있습니다. 그런 다음 이벤트에 응답할 수 있는 각 XAML 파일에 대해 별도의 코드 숨겨진 파일을 연결 하 고 원래 XAML에서 선언한 개체를 조작할 수 있습니다.

XAML 언어는 디자인 도구와 IDE (대화형 개발 환경) 사이 또는 기본 개발자와 지역화 개발자 간에 XAML 소스를 교환 하는 것과 같이 개발 프로세스에서 다양 한 도구와 역할 간의 소스 교환을 지원 합니다. XAML을 교환 형식으로 사용 하면 디자이너 역할과 개발자 역할을 분리 하거나 함께 사용할 수 있으며, 디자이너와 개발자는 앱 프로덕션 중에 반복 될 수 있습니다.

Windows 런타임 앱 프로젝트의 일부로 표시 되는 경우 XAML 파일은 .xaml 파일 이름 확장명을 사용 하는 XML 파일입니다.

## <a name="basic-xaml-syntax"></a>기본 XAML 구문

XAML에는 XML을 기반으로 하는 기본 구문이 있습니다. 정의에 따라 유효한 XAML도 유효한 XML 이어야 합니다. 그러나 XAML에는 XML 1.0 사양에 따라 XML에서 유효 하지만 다른 모든 의미를 할당 하는 구문 개념도 있습니다. 예를 들어 XAML은 속성 *요소 구문을*지원 합니다. 여기서 속성 값은 특성의 문자열 값 이나 콘텐츠로 설정 되는 것이 아니라 요소 내에서 설정 될 수 있습니다. 일반 XML로 XAML 속성 요소는 이름에 점이 있는 요소 이므로 일반 XML에 유효 하지만 동일한 의미를 갖지 않습니다.

## <a name="xaml-and-visual-studio"></a>XAML 및 Visual Studio

Microsoft Visual Studio를 사용 하면 XAML 텍스트 편집기와 그래픽으로 표현 되는 XAML 디자인 화면에서 유효한 XAML 구문을 생성할 수 있습니다. Visual Studio를 사용 하 여 앱에 대 한 XAML을 작성 하는 경우 각 키 입력에 구문에 대해 너무 많은 걱정 하지 마세요. IDE는 자동 완성 힌트를 제공 하 여 유효한 XAML 구문을 권장 합니다 .이 힌트를 통해 Microsoft IntelliSense 목록 및 드롭다운의 제안 사항을 표시 하 고 **도구 상자** 창에 UI 요소 라이브러리를 표시 하거나 기타 기술을 사용할 것입니다. XAML을 사용 하는 첫 번째 환경인 경우 참조 또는 다른 토픽에서 XAML 구문을 설명 하는 경우 제한 사항 또는 선택 항목을 설명 하는 데 종종 사용 되는 용어와 구문 규칙을 알고 있는 것이 유용할 수 있습니다. XAML 구문에 대 한 자세한 내용은 별도 항목인 [xaml 구문 가이드](xaml-syntax-guide.md)에서 설명 합니다.

## <a name="xaml-namespaces"></a>XAML 네임 스페이스

일반적인 프로그래밍에서 네임 스페이스는 프로그래밍 엔터티에 대 한 식별자가 해석 되는 방법을 결정 하는 구성 개념입니다. 프로그래밍 프레임 워크는 네임 스페이스를 사용 하 여 사용자가 선언한 식별자를 프레임 워크 선언 식별자에서 분리 하 고, 네임 스페이스 한정자를 통해 식별자를 명확 하 게 구분 하 고, 범위 지정 규칙을 적용할 수 있습니다. XAML에는 XAML 언어에 대해이 용도로 사용 되는 자체 XAML 네임 스페이스 개념이 있습니다. 다음은 XAML이 XML 언어 네임 스페이스 개념을 적용 하 고 확장 하는 방법입니다.

- XAML은 네임 스페이스 선언에 대해 예약 된 XML 특성 **xmlns** 를 사용 합니다. 특성의 값은 일반적으로 XML에서 상속 되는 규칙 인 URI (Uniform Resource Identifier)입니다.
- XAML은 선언에서 접두사를 사용 하 여 기본이 아닌 네임 스페이스를 선언 하 고, 요소 및 특성에서 접두사 사용은 해당 네임 스페이스를 참조 합니다.
- XAML에는 사용 또는 선언에 접두사가 없는 경우 사용 되는 네임 스페이스인 기본 네임 스페이스의 개념이 있습니다. 각 XAML 프로그래밍 프레임 워크에 대해 기본 네임 스페이스를 다르게 정의할 수 있습니다.
- 네임 스페이스 정의는 부모 요소에서 자식 요소로 XAML 파일이 나 구문에서 상속 됩니다. 예를 들어 XAML 파일의 루트 요소에서 네임 스페이스를 정의 하는 경우 해당 파일 내의 모든 요소는 해당 네임 스페이스 정의를 상속 합니다. 페이지에 추가 된 요소가 네임 스페이스를 다시 정의 하는 경우 해당 요소의 하위 항목은 새 정의를 상속 합니다.
- 요소의 특성은 요소의 네임 스페이스를 상속 합니다. XAML 특성에 접두사를 표시 하는 것은 일반적이 지 않습니다.

XAML 파일은 거의 항상 루트 요소에서 기본 XAML 네임 스페이스를 선언 합니다. 기본 XAML 네임 스페이스는 접두사를 사용 하 여 한정 하지 않고 선언할 수 있는 요소를 정의 합니다. 일반적인 Windows 런타임 앱 프로젝트의 경우이 기본 네임 스페이스에는 UI 정의에 사용 되는 Windows 런타임에 대 한 모든 기본 제공 XAML 용어 (기본 컨트롤, 텍스트 요소, XAML 그래픽 및 애니메이션, 데이터 바인딩 및 스타일 지원 형식)가 포함 되어 있습니다. 따라서 Windows 런타임 apps에 대해 작성 하는 대부분의 XAML은 일반적인 UI 요소를 참조할 때 XAML 네임 스페이스 및 접두사를 사용 하지 않도록 할 수 있습니다.

다음은 앱에 대 한 초기 페이지의 템플릿 생성 루트를 표시 하는 코드 조각입니다 <xref:Windows.UI.Xaml.Controls.Page> (여는 태그와 단순화 된 태그 표시). 기본 네임 스페이스와 **x** 네임 스페이스 (다음에 설명)를 선언 합니다.

```xml
<Page
    x:Class="Application1.BlankPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
>
```

## <a name="the-xaml-language-xaml-namespace"></a>XAML 언어 XAML 네임 스페이스입니다.

거의 모든 Windows 런타임 XAML 파일에서 선언 된 하나의 특정 XAML 네임 스페이스는 XAML 언어 네임 스페이스입니다. 이 네임 스페이스는 XAML 언어 사양에 정의 된 요소와 개념을 포함 합니다. 규칙에 따라 XAML 언어 XAML 네임 스페이스는 접두사 "x"에 매핑됩니다. Windows 런타임 app 프로젝트에 대 한 기본 프로젝트 및 파일 템플릿은 항상 기본 XAML 네임 스페이스 (접두사 없음 `xmlns=` )와 xaml 언어 xaml 네임 스페이스 (접두사 "x")를 루트 요소의 일부로 정의 합니다.

"X" 접두사/x m l-언어 XAML 네임 스페이스에는 XAML에서 자주 사용 하는 여러 프로그래밍 구문이 포함 되어 있습니다. 가장 일반적인 항목은 다음과 같습니다.

| 용어 | Description |
|------|-------------|
| [x:Key](x-key-attribute.md) | XAML의 각 리소스에 대 한 고유한 사용자 정의 키를 설정 합니다 <xref:Windows.UI.Xaml.ResourceDictionary> . 키의 토큰 문자열은 **StaticResource** 태그 확장에 대 한 인수 이며, 나중에이 키를 사용 하 여 앱 xaml의 다른 곳에 있는 다른 xaml 사용에서 xaml 리소스를 검색 합니다. |
| [x:Class](x-class-attribute.md) | XAML 페이지에 대 한 코드를 제공 하는 클래스의 코드 네임 스페이스와 코드 클래스 이름을 지정 합니다. 그러면 응용 프로그램을 빌드할 때 빌드 작업에 의해 만들어지거나 조인 된 클래스의 이름이 표시 됩니다. 이러한 빌드 작업은 XAML 태그 컴파일러를 지원 하 고 앱이 컴파일될 때 태그와 코드를 결합 합니다. XAML 페이지의 코드 숨김으로 지원 하려면 이러한 클래스가 있어야 합니다. <xref:Windows.UI.Xaml.Window.Content%2A?displayProperty=nameWithType> 기본 Windows 런타임 활성화 모델에 있습니다. |
| [x:Name](x-name-attribute.md) | XAML로 정의 된 개체 요소가 처리 된 후 런타임 코드에 있는 인스턴스의 런타임 개체 이름을 지정 합니다. 코드에서 명명 된 변수를 선언 하는 것 처럼 XAML에서 **x:Name** 을 설정 한다고 생각할 수 있습니다. 나중에 살펴보겠습니다 .이는 XAML이 Windows 런타임 앱의 구성 요소로 로드 될 때 발생 하는 것입니다. <br/><div class="alert">**참고** <xref:Windows.UI.Xaml.FrameworkElement.Name%2A> 는 프레임 워크에서 유사한 속성 이지만 일부 요소는이를 지원 하지 않습니다. 해당 요소 형식에서 **FrameworkElement.Name** 가 지원 되지 않을 때마다 요소 식별에 대해 **x:Name** 을 사용 합니다. |
| [x:Uid](x-uid-directive.md) | 일부 속성 값에 지역화 된 리소스를 사용 해야 하는 요소를 식별 합니다. **X:Uid**를 사용 하는 방법에 대 한 자세한 내용은 [빠른 시작: UI 리소스 번역](/previous-versions/windows/apps/hh965329(v=win.10))을 참조 하세요. |
| [XAML 기본 데이터 형식](xaml-intrinsic-data-types.md) | 이러한 형식은 특성이 나 리소스에 필요한 경우 단순 값 형식에 대 한 값을 지정할 수 있습니다. 이러한 내장 형식은 일반적으로 각 프로그래밍 언어의 내장 정의의 일부로 정의 되는 단순 값 형식에 해당 합니다. 예를 들어 storyboarded 시각적 상태에서 사용할 **진정한** 부울 값을 나타내는 개체가 필요할 수 있습니다 <xref:Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames> . XAML의 해당 값에 대해 다음과 같이 **X:boolean** 내장 형식을 object 요소로 사용 합니다. <code>&lt;x:Boolean&gt;True&lt;/x:Boolean&gt;</code> |

XAML 언어 XAML 네임 스페이스의 다른 프로그래밍 생성자가 있지만 일반적인 것은 아닙니다.

## <a name="mapping-custom-types-to-xaml-namespaces"></a>XAML 네임 스페이스에 사용자 지정 형식 매핑

XAML의 가장 강력한 기능 중 하나는 Windows 런타임 앱에 대 한 XAML 어휘를 쉽게 확장할 수 있다는 것입니다. 앱의 프로그래밍 언어에서 고유한 사용자 지정 형식을 정의한 다음 XAML 태그에서 사용자 지정 형식을 참조할 수 있습니다. 사용자 지정 형식을 통한 확장 지원은 XAML 언어의 작동 방식에 기본적으로 제공 됩니다. 프레임 워크 또는 앱 개발자는 XAML에서 참조 하는 지원 개체를 만들어야 합니다. 프레임 워크와 앱 개발자는 모두 기본 XAML 구문 규칙을 의미 하거나 그 외의 어휘 개체의 사양에 따라 바인딩됩니다. XAML 언어 XAML 네임 스페이스 형식에 대 한 몇 가지 기대 사항이 있지만 Windows 런타임에서는 필요한 모든 지원을 제공 합니다.

Windows 런타임 core 라이브러리 및 메타 데이터 이외의 라이브러리에서 제공 되는 형식에 대해 XAML을 사용 하는 경우에는 접두사를 사용 하 여 XAML 네임 스페이스를 선언 하 고 매핑해야 합니다. 요소 사용에서 해당 접두사를 사용 하 여 라이브러리에 정의 된 형식을 참조 합니다. 접두사 매핑은 일반적으로 루트 요소에서 다른 XAML 네임 스페이스 정의와 함께 **xmlns** 특성으로 선언 합니다.

사용자 지정 형식을 참조 하는 고유한 네임 스페이스 정의를 만들려면 먼저 **xmlns:** 키워드와 원하는 접두사를 지정 합니다. 이 특성의 값에는 값의 첫 번째 부분으로 **using:** 키워드가 포함되어 있어야 합니다. 값의 나머지 부분은 사용자 지정 형식이 포함 된 특정 코드 지원 네임 스페이스를 이름으로 참조 하는 문자열 토큰입니다.

접두사는 해당 XAML 파일의 나머지 태그에서 XAML 네임 스페이스를 참조 하는 데 사용 되는 태그 토큰을 정의 합니다. 콜론 문자 (:) 는 XAML 네임 스페이스 내에서 참조 되는 접두사와 엔터티 사이를 이동 합니다.

예를 들어 접두사를 네임 스페이스에 매핑하는 특성 구문은 다음과 같습니다. `myTypes` `myCompany.myTypes` `    xmlns:myTypes="using:myCompany.myTypes"` 대표적인 요소 사용은 다음과 같습니다. `<myTypes:CustomButton/>`

Visual C++ 구성 요소 확장에 대 한 특별 고려 사항 (c + +/CX)을 비롯 하 여 사용자 지정 형식에 대 한 XAML 네임 스페이스 매핑에 대 한 자세한 내용은 [xaml 네임 스페이스 및 네임 스페이스 매핑](xaml-namespaces-and-namespace-mapping.md)

## <a name="other-xaml-namespaces"></a>다른 XAML 네임 스페이스

접두사 "d" (디자이너 네임 스페이스의 경우) 및 "mc" (태그 호환성)를 정의 하는 XAML 파일이 종종 표시 됩니다. 일반적으로이는 인프라 지원을 위한 것 이거나 디자인 타임 도구에서 시나리오를 사용 하도록 설정 하는 것입니다. 자세한 내용은 [xaml 네임 스페이스 항목의 "다른 xaml 네임 스페이스" 섹션](xaml-namespaces-and-namespace-mapping.md#other-XAML-namespaces)을 참조 하세요.

## <a name="markup-extensions"></a>태그 확장

태그 확장은 Windows 런타임 XAML 구현에서 자주 사용 되는 XAML 언어 개념입니다. 태그 확장은 종종 지원 형식을 기반으로 요소를 선언 하는 것이 아니라 XAML 파일이 값 또는 동작에 액세스할 수 있도록 하는 일종의 "바로 가기"를 나타냅니다. 일부 태그 확장은 구문을 간소화 하거나 다른 XAML 파일 간의 팩터링을 목표로 하 여 일반 문자열이 나 추가적으로 중첩 된 요소를 사용 하 여 속성을 설정할 수 있습니다.

XAML 특성 구문에서 중괄호 "{"와 "}"는 XAML 태그 확장 사용을 의미 합니다. 이 사용법은 특성 값을 리터럴 문자열 또는 직접 문자열 변환 가능 값으로 처리 하는 일반적인 처리에서 이스케이프 되도록 XAML 처리를 보냅니다. 대신 XAML 파서는 특정 태그 확장에 대 한 동작을 제공 하는 코드를 호출 하 고,이 코드는 XAML 파서에서 필요로 하는 대체 개체 또는 동작 결과를 제공 합니다. 태그 확장은 태그 확장 이름 뒤에 오는 인수를 포함할 수 있으며 중괄호 안에도 포함 됩니다. 일반적으로 평가 된 태그 확장은 개체 반환 값을 제공 합니다. 구문 분석 중에이 반환 값은 소스 XAML에서 태그 확장 사용이 있었던 개체 트리의 위치에 삽입 됩니다.

Windows 런타임 XAML은 기본 XAML 네임 스페이스 아래에 정의 되 고 Windows 런타임 XAML 파서에서 인식 되는 이러한 태그 확장을 지원 합니다.

- [{X:bind}](x-bind-markup-extension.md): 컴파일 시간에 생성 되는 특수 한 용도의 코드를 실행 하 여 런타임 시까지 속성 평가를 지연 하는 데이터 바인딩을 지원 합니다. 이 태그 확장은 다양 한 범위의 인수를 지원 합니다.
- [{Binding}](binding-markup-extension.md): 일반 용도의 런타임 개체 검사를 실행 하 여 런타임 시까지 속성 평가를 지연 하는 데이터 바인딩을 지원 합니다. 이 태그 확장은 다양 한 범위의 인수를 지원 합니다.
- [{StaticResource}](staticresource-markup-extension.md):에 정의 된 리소스 값 참조를 지원 <xref:Windows.UI.Xaml.ResourceDictionary> 합니다. 이러한 리소스는 다른 XAML 파일에 있을 수 있지만 궁극적으로는 로드 시 XAML 파서가 검색할 수 있어야 합니다. 사용의 인수는 `{StaticResource}` 에서 키가 지정 된 리소스의 키 (이름)를 식별 합니다 <xref:Windows.UI.Xaml.ResourceDictionary> .
- [{Themeresource}](themeresource-markup-extension.md): [{StaticResource}](staticresource-markup-extension.md) 와 비슷하지만 런타임 테마 변경 내용에 응답할 수 있습니다. 대부분의 이러한 템플릿은 앱이 실행 되는 동안 테마를 전환 하는 사용자와의 호환성을 위해 디자인 되었기 때문에, Windows 런타임 기본 XAML 템플릿에서 매우 자주 나타납니다.
- [{TemplateBinding}](templatebinding-markup-extension.md): XAML의 컨트롤 템플릿과 런타임에 최종 사용을 지 원하는 [{Binding}](binding-markup-extension.md) 의 특수 한 경우입니다.
- [{RelativeSource}](relativesource-markup-extension.md): 템플릿 기반 부모에서 값이 제공 되는 특정 형식의 템플릿 바인딩을 사용 하도록 설정 합니다.
- [{Customresource}](customresource-markup-extension.md): 고급 리소스 조회 시나리오에 적합 합니다.

Windows 런타임는 [{X:null} 태그 확장](x-null-markup-extension.md)도 지원 합니다. 이를 사용 하 여 XAML에서 [**Nullable**](/dotnet/api/system.nullable-1) 값을 **null** 로 설정 합니다. 예를 들어,에 대 한 컨트롤 템플릿에서이를 사용할 수 있습니다 .이는 <xref:Windows.UI.Xaml.Controls.CheckBox> **null** 을 확정 되지 않은 선택 상태 ("결정 되지 않은" 시각적 상태를 트리거하기)로 해석 합니다.

일반적으로 태그 확장은 응용 프로그램에 대 한 개체 그래프의 다른 부분에서 기존 인스턴스를 반환 하거나 런타임에 값을 지연 시킵니다. 일반적으로 사용 되는 특성 값으로 태그 확장을 사용할 수 있기 때문에 속성 요소 구문이 필요 하지 않을 수 있는 참조 형식 속성에 대 한 값을 제공 하는 태그 확장을 종종 볼 수 있습니다.

예를 들어에서 다시 사용할 수 있는를 참조 하는 구문은 <xref:Windows.UI.Xaml.Style> <xref:Windows.UI.Xaml.ResourceDictionary> 다음과 같습니다 `<Button Style="{StaticResource SearchButtonStyle}"/>` . 는 <xref:Windows.UI.Xaml.Style> 단순 값이 아닌 참조 형식 이므로 사용 하지 않으면 속성 `{StaticResource}` 요소와 속성을 설정 하는 데 필요한 `<Button.Style>` `<Style>` 정의가 필요 <xref:Windows.UI.Xaml.FrameworkElement.Style%2A?displayProperty=nameWithType> 합니다.

태그 확장을 사용 하면 XAML에서 설정할 수 있는 모든 속성을 특성 구문에서 설정할 수 있습니다. 직접 개체 인스턴스화에 대한 특성 구문을 지원하지 않더라도 특성 구문을 사용하여 속성에 대한 참조 값을 제공할 수 있습니다. 또는 XAML 속성이 값 형식으로 채워지거나 새로 생성 된 참조 형식에 의해 채워지는 일반적인 요구 사항을 연기 하는 특정 동작을 사용 하도록 설정할 수 있습니다.

다음 XAML 예제에서는 <xref:Windows.UI.Xaml.FrameworkElement.Style%2A?displayProperty=nameWithType> 특성 구문을 사용 하 여의 속성 값을 설정 하는 방법을 보여 줍니다 <xref:Windows.UI.Xaml.Controls.Border> . <xref:Windows.UI.Xaml.FrameworkElement.Style%2A?displayProperty=nameWithType>속성은 클래스의 인스턴스를 사용 하며 <xref:Windows.UI.Xaml.Style?displayProperty=nameWithType> , 기본적으로 특성 구문 문자열을 사용 하 여 만들 수 없는 참조 형식입니다. 하지만 이 경우 특성은 특정 태그 확장인 [StaticResource](staticresource-markup-extension.md)를 참조합니다. 태그 확장이 처리 될 때 리소스 사전에서 키가 지정 된 리소스로 이전에 정의 된 **스타일** 요소에 대 한 참조를 반환 합니다.

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

태그 확장을 중첩할 수 있습니다. 가장 안쪽의 태그 확장이 먼저 계산됩니다.

태그 확장으로 인해 특성에서 리터럴 "{" 값에 대 한 특수 구문이 필요 합니다. 자세한 내용은 [XAML 구문 가이드](xaml-syntax-guide.md)를 참조 하세요.

## <a name="events"></a>이벤트

XAML은 개체 및 해당 속성에 대 한 선언적 언어 이지만 태그의 개체에 이벤트 처리기를 연결 하는 구문도 포함 됩니다. 그런 다음 XAML 이벤트 구문은 Windows 런타임 프로그래밍 모델을 통해 XAML 선언 이벤트를 통합할 수 있습니다. 이벤트가 처리 되는 개체의 특성 이름으로 이벤트 이름을 지정 합니다. 특성 값의 경우 코드에서 정의 하는 이벤트 처리기 함수의 이름을 지정 합니다. XAML 프로세서는이 이름을 사용 하 여 로드 된 개체 트리에서 대리자 표현을 만들고 지정 된 처리기를 내부 처리기 목록에 추가 합니다. 거의 모든 Windows 런타임 앱은 태그와 코드 숨김이 있는 소스 모두에 의해 정의 됩니다.

다음은 간단한 예제입니다. <xref:Windows.UI.Xaml.Controls.Button>클래스는 이라는 이벤트를 지원 <xref:Windows.UI.Xaml.Controls.Primitives.ButtonBase.Click> 합니다. 사용자가 **단추**를 클릭 한 후에 호출 해야 하는 코드를 실행 하는 **클릭** 에 대 한 처리기를 작성할 수 있습니다. XAML에서 **단추**에 대 한 **클릭** 을 특성으로 지정 합니다. 특성 값의 경우 처리기의 메서드 이름인 문자열을 제공 합니다.

```xml
<Button Click="showUpdatesButton_Click">Show updates</Button>
```

컴파일하는 경우 컴파일러는 이제 `showUpdatesButton_Click` XAML 페이지의 [x:Class](x-class-attribute.md) 값에 선언 된 네임 스페이스의 코드 파일에 정의 된 라는 메서드를 사용할 것으로 예상 합니다. 또한 해당 메서드는 이벤트에 대 한 대리자 계약을 충족 해야 합니다 <xref:Windows.UI.Xaml.Controls.Primitives.ButtonBase.Click> . 예:

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

프로젝트 내에서 XAML은 .xaml 파일로 작성 되며 선호 하는 언어 (c #, Visual Basic, c + +/CX)를 사용 하 여 코드를 작성 하는 파일을 작성 합니다. XAML 파일이 프로젝트에 대 한 빌드 작업의 일부로 태그 컴파일되는 경우 네임 스페이스와 클래스를 XAML 페이지의 루트 요소에 대 한 [x:Class](x-class-attribute.md) 특성으로 지정 하 여 각 xaml 페이지에 대 한 xaml 코드 숨겨진 파일의 위치를 식별 합니다. 이러한 메커니즘이 XAML에서 작동 하는 방식 및 이러한 메커니즘이 프로그래밍 및 응용 프로그램 모델과 어떻게 관련 되는지에 대 한 자세한 내용은 [이벤트 및 라우트된 이벤트 개요](events-and-routed-events-overview.md)를 참조 하세요.

> [!NOTE]
> C + +/CX의 경우 두 개의 코드 숨김이 있습니다. 하나는 헤더 (.xaml)이 고 다른 하나는 구현 (.xaml)입니다. 구현은 헤더를 참조 하며,이는 기술적으로 코드 기반 연결에 대 한 진입점을 나타내는 헤더입니다.

## <a name="resource-dictionaries"></a>리소스 사전

는 <xref:Windows.UI.Xaml.ResourceDictionary> 일반적으로 리소스 사전을 xaml 페이지 또는 별도의 xaml 파일의 영역으로 작성 하 여 수행 되는 일반적인 작업입니다. 리소스 사전 및 사용 방법은이 항목에서 다루지 않는 더 큰 개념 영역입니다. 자세한 내용은 [ResourceDictionary 및 XAML 리소스 참조](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)를 참조 하세요.

## <a name="xaml-and-xml"></a>XAML 및 XML

XAML 언어는 기본적으로 XML 언어를 기반으로 합니다. 그러나 XAML은 XML을 크게 확장 합니다. 특히 스키마 개념은 지원 형식 개념과의 관계 때문에 매우 다르게 처리 되며 연결 된 멤버 및 태그 확장과 같은 언어 요소를 추가 합니다. **xml: lang** 은 XAML에서 올바르지만 구문 분석 동작이 아니라 런타임에 영향을 주지만 일반적으로 프레임 워크 수준 속성에 별칭을 지정 합니다. 자세한 내용은 <xref:Windows.UI.Xaml.FrameworkElement.Language%2A?displayProperty=nameWithType>를 참조하세요. **xml: base** 는 태그에서 유효 하지만 파서는 무시 합니다. **xml: space** 는 올바르지만 [XAML 및 공백](xaml-and-whitespace.md) 항목에 설명 된 시나리오에만 해당 됩니다. **Encoding** 특성은 XAML에서 유효 합니다. UTF-8 및 UTF-16 인코딩만 지원 됩니다. UTF-32 인코딩은 지원 되지 않습니다.

###  <a name="case-sensitivity-in-xaml"></a>XAML에서 대/소문자 구분

XAML은 대/소문자를 구분 합니다. 이는 XML을 기반으로 하는 XAML의 또 다른 결과 이며 대/소문자를 구분 합니다. XAML 요소 및 특성 이름은 대/소문자를 구분 합니다. 특성 값은 대/소문자를 구분 합니다. 특정 속성에 대 한 특성 값이 처리 되는 방법에 따라 달라 집니다. 예를 들어 특성 값이 열거형의 멤버 이름을 선언 하는 경우 멤버 이름 문자열을 형식으로 변환 하 여 열거형 멤버 값을 반환 하는 기본 제공 동작은 대/소문자를 구분 하지 않습니다. 반면 name 속성의 값과 Name **속성이 선언** 하는 이름을 기반으로 하는 개체 작업에 대 한 유틸리티 메서드 **Name** 는 이름 문자열을 대/소문자를 구분 하 여 처리 합니다.

## <a name="xaml-namescopes"></a>XAML 이름 범위

XAML 언어는 XAML 이름 범위의 개념을 정의 합니다. Xaml 이름 범위 개념은 xaml 프로세서가 XAML 요소에 적용 된 **x:Name** 또는 **Name** 의 값을 처리 하는 방법, 특히 이름을 고유 식별자로 사용할 수 있는 범위에 영향을 줍니다. XAML 이름 범위는 별도 항목에서 자세히 설명 합니다. [xaml 이름 범위](xaml-namescopes.md)를 참조 하세요.

## <a name="the-role-of-xaml-in-the-development-process"></a>개발 프로세스의 XAML 역할

XAML은 응용 프로그램 개발 프로세스에서 몇 가지 중요 한 역할을 담당 합니다.

- XAML은 c #, Visual Basic 또는 c + +/CX를 사용 하 여 프로그래밍 하는 경우 해당 UI에서 응용 프로그램의 UI 및 요소를 선언 하는 기본 형식입니다. 일반적으로 프로젝트에 있는 하나 이상의 XAML 파일은 처음에 표시 되는 UI에 대 한 앱의 페이지 비유를 나타냅니다. 추가 XAML 파일은 탐색 UI에 대 한 추가 페이지를 선언할 수 있습니다. 다른 XAML 파일은 템플릿 또는 스타일과 같은 리소스를 선언할 수 있습니다.
- 응용 프로그램의 컨트롤과 UI에 적용 되는 스타일 및 템플릿을 선언 하는 데 XAML 형식을 사용 합니다.
- 기존 컨트롤의 템플릿에 스타일 및 템플릿을 사용 하거나 기본 템플릿을 컨트롤 패키지의 일부로 제공 하는 컨트롤을 정의 하는 경우에 사용할 수 있습니다. 이를 사용 하 여 스타일 및 템플릿을 정의 하는 경우 관련 XAML은 일반적으로 루트를 사용 하는 불연속 XAML 파일로 선언 됩니다 <xref:Windows.UI.Xaml.ResourceDictionary> .
- XAML은 응용 프로그램 UI를 만들고 여러 디자이너 앱 간에 UI 디자인을 교환할 때 디자이너에서 지 원하는 일반적인 형식입니다. 특히 앱에 대 한 XAML은 서로 다른 XAML 디자인 도구 (또는 도구 내의 디자인 창) 간에 서로 다른 방법으로 사용할 수 있습니다.
- 다른 여러 기술 에서도 XAML의 기본 UI를 정의 합니다. Windows Presentation Foundation (WPF) XAML 및 Microsoft Silverlight XAML과의 관계에서 Windows 런타임 XAML은 공유 기본 XAML 네임 스페이스에 대해 동일한 URI를 사용 합니다. Windows 런타임에 대 한 XAML 어휘는 Silverlight에서 사용 되는 XAML 용 XAML 어휘 및 WPF에 의해 약간 낮은 범위에도 크게 겹칩니다. 따라서 XAML은 XAML도 사용 하는 기반이 기술에 대해 원래 정의 된 UI에 대 한 효율적인 마이그레이션 경로를 승격 합니다.
- XAML은 UI의 시각적 모양을 정의 하 고, 관련 된 코드 숨김이 있는 파일은 논리를 정의 합니다. 코드 숨김으로 논리를 변경 하지 않고 UI 디자인을 조정할 수 있습니다. XAML은 디자이너와 개발자 사이의 워크플로를 단순화 합니다.
- Xaml 언어에 대 한 다양 한 비주얼 디자이너 및 디자인 화면 지원으로 인해 XAML은 초기 개발 단계에서 신속 하 게 UI 프로토타입을 지원 합니다.

개발 프로세스의 고유한 역할에 따라 XAML과 상호 작용 하지 않을 수 있습니다. XAML 파일과 상호 작용 하는 정도는 사용 중인 개발 환경, toolboxes 및 속성 편집기와 같은 대화형 디자인 환경 기능을 사용 하는지 여부, Windows 런타임 앱의 범위 및 용도에 따라 달라 집니다. 그럼에도 불구 하 고 응용 프로그램을 개발 하는 동안 텍스트 편집기나 XML 편집기를 사용 하 여 요소 수준에서 XAML 파일을 편집 하 게 될 것입니다. 이 정보를 사용 하 여 텍스트 또는 XML 표현의 XAML을 안전 하 게 편집 하 고 도구, 마크업 컴파일 작업 또는 Windows 런타임 앱의 런타임 단계에서 사용 하는 경우 XAML 파일의 선언과 용도에 대 한 유효성을 유지할 수 있습니다.

## <a name="optimize-your-xaml-for-load-performance"></a>로드 성능을 위해 XAML 최적화

성능에 대 한 모범 사례를 사용 하 여 XAML에서 UI 요소를 정의 하는 몇 가지 팁은 다음과 같습니다. 이러한 팁 중 상당수는 XAML 리소스를 사용 하는 것과 관련이 있지만 편의를 위해 일반적인 XAML 개요에서 여기에 나열 되어 있습니다. XAML 리소스에 대 한 자세한 내용은 [ResourceDictionary 및 xaml 리소스 참조](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)를 참조 하세요. Xaml에서 피해 야 하는 성능 저하 중 일부를 의도적으로 보여 주는 XAML을 비롯 하 여 성능에 대 한 몇 가지 팁을 보려면 [xaml 태그 최적화](../debug-test-perf/optimize-xaml-loading.md)를 참조 하세요.

- XAML에서 자주 동일한 색 브러시를 사용 하는 경우 <xref:Windows.UI.Xaml.Media.SolidColorBrush> 매번 특성 값으로 명명 된 색을 사용 하는 대신를 리소스로 정의 합니다.
- 둘 이상의 UI 페이지에서 동일한 리소스를 사용 하는 경우 각 페이지 대신에서 정의 하는 것이 좋습니다 <xref:Windows.UI.Xaml.Application.Resources%2A> . 반대로 한 페이지 에서만 리소스를 사용 하는 경우에는 **응용 프로그램 리소스** 에서 리소스를 정의 하지 말고이를 필요로 하는 페이지에 대해서만 정의 합니다. 이는 xaml을 설계 하는 동안 xaml을 구분 하는 동시에 XAML 구문 분석 중에 성능을 향상 하는 데 유용 합니다.
- 앱 패키지를 사용 하는 리소스의 경우 사용 하지 않는 리소스 (키가 있지만 앱에 [StaticResource](staticresource-markup-extension.md) 참조가 없는 리소스)를 확인 합니다. 앱을 출시 하기 전에 XAML에서이를 제거 합니다.
- 디자인 리소스 ()를 제공 하는 별도의 XAML 파일을 사용 하는 경우 <xref:Windows.UI.Xaml.ResourceDictionary.MergedDictionaries%2A> 이러한 파일에서 사용 하지 않는 리소스를 주석으로 처리 하거나 제거 하는 것이 좋습니다 둘 이상의 앱에서 사용 중이거나 모든 앱에 대 한 공용 리소스를 제공 하는 공유 XAML 시작 지점이 있는 경우에도 항상 XAML 리소스를 패키지 하 고 잠재적으로 로드 해야 하는 앱입니다.
- 컴퍼지션에 필요 하지 않은 UI 요소를 정의 하지 말고 가능한 경우 기본 컨트롤 템플릿을 사용 합니다 (이러한 템플릿은 이미 테스트 되 고 부하 성능에 대해 확인 됨).
- <xref:Windows.UI.Xaml.Controls.Border>UI 요소의 의도적인 그리기 대신 컨테이너를 사용 합니다. 기본적으로 동일한 픽셀을 여러 번 그리지 않습니다. 과도 한 그리기 및 테스트 하는 방법에 대 한 자세한 내용은을 참조 하십시오 <xref:Windows.UI.Xaml.DebugSettings.IsOverdrawHeatMapEnabled?displayProperty=nameWithType> .
- 또는에 대 한 기본 항목 템플릿을 사용 <xref:Windows.UI.Xaml.Controls.ListView> <xref:Windows.UI.Xaml.Controls.GridView> 합니다. 여기에는 많은 수의 목록 항목에 대 한 시각적 트리를 빌드할 때 성능 문제를 해결 하는 특별 한 **발표자** 논리가 있습니다.

## <a name="debug-xaml"></a>XAML 디버그

XAML은 태그 언어 이므로 Microsoft Visual Studio 내에서 디버깅에 대 한 일반적인 전략 중 일부를 사용할 수 없습니다. 예를 들어 XAML 파일 내에서 중단점을 설정 하는 방법은 없습니다. 그러나 응용 프로그램을 개발 하는 동안 UI 정의 나 기타 XAML 태그를 사용 하 여 문제를 디버그 하는 데 도움이 되는 다른 기술도 있습니다.

XAML 파일에 문제가 발생 하는 경우 가장 일반적인 결과는 일부 시스템 또는 앱이 XAML 구문 분석 예외를 throw 하는 것입니다. Xaml 구문 분석 예외가 있을 때마다 XAML 파서에서 로드 한 XAML이 유효한 개체 트리를 만들지 못했습니다. XAML이 루트 시각적 개체로 로드 되는 응용 프로그램의 첫 번째 "페이지"를 나타내는 경우와 같은 일부 경우에는 XAML 구문 분석 예외를 복구할 수 없습니다.

XAML은 일반적으로 Visual Studio와 같은 IDE 내에서, 그리고 XAML 디자인 화면 중 하나에서 편집 됩니다. Visual Studio는 편집할 때 디자인 타임 유효성 검사 및 XAML 소스에 대 한 오류 검사를 제공 하는 경우가 많습니다. 예를 들어 잘못 된 특성 값을 입력 하는 즉시 XAML 텍스트 편집기에 "물결선"가 표시 될 수 있으며, XAML 컴파일 패스에서 UI 정의에 문제가 발생 한 것을 볼 수 있을 때까지 기다리지 않아도 됩니다.

앱이 실제로 실행 되 면 디자인 타임에 XAML 구문 분석 오류가 감지 되지 않은 경우 CLR (공용 언어 런타임)에서 [**Xamlparseexception**](/dotnet/api/Windows.UI.Xaml.markup.xamlparseexception?view=dotnet-uwp-10.0)으로 보고 합니다. 런타임 **Xamlparseexception**에 대해 수행할 수 있는 작업에 대 한 자세한 내용은 [c #에서 Windows 런타임 앱에 대 한 예외 처리 또는 Visual Basic](/previous-versions/windows/apps/dn532194(v=win.10))를 참조 하세요.

> [!NOTE]
> 코드에 c + +/CX를 사용 하는 앱은 특정 [**Xamlparseexception**](/dotnet/api/Windows.UI.Xaml.markup.xamlparseexception?view=dotnet-uwp-10.0)을 얻지 못합니다. 그러나 예외의 메시지는 오류의 원인이 XAML 관련 임을 명확 하 게 하 고 XAML 파일의 줄 번호와 같은 컨텍스트 정보를 XAML 파일에 포함 하는 것을 명확 **하 게 합니다** .

Windows 런타임 앱 디버깅에 대 한 자세한 정보는 [디버그 세션 시작](/visualstudio/debugger/start-a-debugging-session-for-a-store-app-in-visual-studio-vb-csharp-cpp-and-xaml?view=vs-2015)을 참조 하세요.