---
author: jwmsft
description: 이 항목에서는 대부분의 XAML 파일 루트 요소에 있는 XML/XAML 네임스페이스(xmlns) 매핑에 대해 설명합니다. 사용자 지정 유형 및 어셈블리를 위해 유사한 매핑을 생성하는 방법도 설명합니다.
title: XAML 네임스페이스 및 네임스페이스 매핑
ms.assetid: A19DFF78-E692-47AE-8221-AB5EA9470E8B
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a1aebe3d9aac460d444a5dffcd63142300c022b7
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5990773"
---
# <a name="xaml-namespaces-and-namespace-mapping"></a>XAML 네임스페이스 및 네임스페이스 매핑


이 항목에서는 대부분의 XAML 파일 루트 요소에 있는 XML/XAML 네임스페이스(**xmlns**) 매핑에 대해 설명합니다. 사용자 지정 유형 및 어셈블리를 위해 유사한 매핑을 생성하는 방법도 설명합니다.

## <a name="how-xaml-namespaces-relate-to-code-definition-and-type-libraries"></a>XAML 네임스페이스가 정의 및 유형 라이브러리와 관련되는 방식

XAML은 일반적인 목적과 Windows 런타임 앱 프로그래밍에 적용하기 위한 목적으로 개체, 해당 개체의 속성 그리고 계층으로 표시되는 개체-속성 관계를 선언하는 데 사용됩니다. XAML에서 선언하는 개체는 다른 프로그래밍 기술 및 언어로 정의된 유형 라이브러리나 다른 표현에서 지원합니다. 이러한 라이브러리로는 다음이 있습니다.

-   Windows 런타임의 기본 제공 개체 집합. 고정된 개체 집합이며 XAML에서 이러한 개체에 액세스하는 경우 내부 유형 매핑 및 활성화 논리를 사용합니다.
-   Microsoft 또는 타사에서 제공하는 분산 라이브러리
-   앱이 통합하고 패키지가 재배포하는 타사 컨트롤의 정의를 나타내는 라이브러리
-   프로젝트의 일부이며 사용자 코드 정의의 일부 또는 전부가 포함된 고유 라이브러리

지원 유형 정보는 특정 XAML 네임스페이스 정의와 관련됩니다. Windows 런타임 등의 XAML 프레임워크는 여러 어셈블리 및 여러 코드 네임스페이스를 집계하여 하나의 XAML 네임스페이스에 매핑할 수 있습니다. 이를 통해 더 큰 프로그래밍 프레임워크 또는 기술을 설명하는 XAML 용어 모음의 개념을 활용할 수 있습니다. XAML 용어 모음은 상당히 종합적일 수 있습니다. 예를 들어 이 참조에서 Windows 런타임 앱에 대해 문서화된 대부분의 XAML이 하나의 XAML 용어 모음을 구성합니다. XAML 용어 모음은 확장 가능하기도 합니다. 지원 코드 정의에 유형을 추가하여 확장하고 이미 XAML 용어 모음의 매핑된 네임스페이스 소스로 사용된 코드 네임스페이스에서 이 유형을 포함하도록 합니다.

XAML 프로세서는 런타임 개체 표시를 만들 때 해당 XAML 네임스페이스와 관련된 지원 어셈블리에서 유형 및 멤버를 찾을 수 있습니다. 이러한 이유로 XAML은 개체 생성 동작의 정의를 형식화하고 교환하는 유용한 방법이 되었으며 UWP 앱의 UI 정의 기술로 사용됩니다.

## <a name="xaml-namespaces-in-typical-xaml-markup-usage"></a>일반 XAML 태그 사용에서의 XAML 네임스페이스

XAML 파일은 거의 항상 해당 루트 요소에서 기본 XAML 네임스페이스를 선언합니다. 기본 XAML 네임스페이스는 접두사로 자격을 부여하지 않고 선언할 수 있는 요소를 정의합니다. 예를 들어 `<Balloon />` 요소를 선언하면 XAML 파서는 **Balloon** 요소가 기본 XAML 네임스페이스에 존재하며 유효할 것으로 예상합니다. 반대로, 정의된 기본 XAML 네임스페이스에 **Balloon**이 없으면 `<party:Balloon />` 같은 접두사를 사용하여 대신 해당 요소 이름에 자격을 부여해야 합니다. 이 접두사는 기본 네임스페이스가 아닌 다른 XAML 네임스페이스에 요소가 있으며 이 요소를 사용하려면 먼저 **party** 접두사에 XAML 네임스페이스를 매핑해야 함을 나타냅니다. XAML 네임스페이스는 선언되는 특정 요소에 적용되며 XAML 구조에서 해당 요소에 포함되어 있는 모든 요소에도 적용됩니다. 이러한 이유로 XAML 네임스페이스는 거의 항상 XAML 파일의 루트 요소에서 선언되어 이 상속을 이용합니다.

## <a name="the-default-and-xaml-language-xaml-namespace-declarations"></a>기본 및 XAML 언어 XAML 네임스페이스 선언

대부분의 XAML 파일의 루트 요소 내에는 두 개의 **xmlns** 선언이 있습니다. 첫 번째 선언은 XAML 네임스페이스를 기본값으로 매핑합니다. `xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"`

XAML을 UI 정의 태그 형식으로 사용하는 여러 이전 Microsoft 기술에 사용된 XAML 네임스페이스 식별자와 동일합니다. 동일한 식별자 사용은 의도적이며 C++, C# 또는 Visual Basic으로 작성한 Windows 런타임 앱으로 이전에 정의된 UI를 마이그레이션하는 경우에 유용합니다.

두 번째 선언은 XAML 정의 언어 요소의 개별 XAML 네임스페이스를 일반적으로 "x:" 접두사에 매핑합니다. `xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"`

이 **xmlns** 값과, 이 값이 매핑된 "x:" 접두사도 XAML을 사용하는 몇 가지 이전 Microsoft 기술에 사용된 정의와 동일합니다.

이러한 선언 사이의 관계를 보면, XAML은 언어 정의이며 Windows 런타임은 XAML을 언어로 사용하고 해당 유형이 XAML에서 참조되는 특정 용어 모음을 정의하는 하나의 구현입니다.

XAML 언어는 특정 언어 요소를 지정하며 이러한 요소는 각각 XAML 네임스페이스에 대해 작동하는 XAML 프로세서 구현을 통해 액세스할 수 있어야 합니다. 프로젝트 템플릿, 샘플 코드 및 언어 기능 설명서는 XAML 언어 XAML 네임스페이스의 "x:" 매핑 규칙을 따릅니다. XAML 언어 네임스페이스는 공통으로 사용되는 여러 기능을 정의합니다. 이러한 기능은 C++, C# 또는 Visual Basic으로 작성된 기본 Windows 런타임 앱에도 필요합니다. 예를 들어 partial 클래스를 통해 코드 숨김을 XAML 파일에 조인하려면 먼저 관련 XAML 파일의 루트 요소에서 해당 클래스를 [x:Class 특성](x-class-attribute.md)으로 명명해야 합니다. 또는 XAML 페이지에 [ResourceDictionary 및 XAML 리소스 참조](https://msdn.microsoft.com/library/windows/apps/mt187273)의 키 입력 리소스로 정의된 모든 요소는 해당 개체 요소에서 [x:Key 특성](x-key-attribute.md)이 설정되어 있어야 합니다.

<span id="other-XAML-namespaces"/>

## <a name="other-xaml-namespaces"></a>다른 XAML 네임스페이스

기본 네임스페이스 및 XAML 언어 XAML 네임스페이스 "x:" 외에도 다른 매핑된 XAML 네임스페이스를 Microsoft Visual Studio에서 생성된 앱용 초기 기본 XAML에서 볼 수 있습니다.

### **<a name="d-httpschemasmicrosoftcomexpressionblend2008"></a>d: (`http://schemas.microsoft.com/expression/blend/2008`)**

"d:" XAML 네임스페이스는 디자이너 지원 특히 Microsoft Visual Studio의 XAML 디자인 화면에서 디자이너를 지원하기 위한 것입니다. "d:" XAML 네임스페이스를 통해 XAML 요소에서 디자이너 또는 디자인-시간 특성을 사용할 수 있습니다. 이러한 디자이너 특성은 XAML 작동 방식의 디자인 측면에만 영향을 줍니다. 앱 실행 시 Windows 런타임 XAML 파서에 동일한 XAML이 로드되면 디자이너 특성은 무시됩니다. 일반적으로 디자이너 특성은 모든 XAML 요소에서 유효하지만 실제로 디자이너 특성 적용이 적절한 시나리오는 한정되어 있습니다. 특히 많은 디자이너 특성이 데이터 바인딩을 사용하는 코드와 XAML을 개발하는 동안 데이터 컨텍스트와 데이터 원본 조작 환경을 향상시키기 위한 것입니다.

-   **d:DesignHeight 및 d:DesignWidth 특성:** 경우에 따라 두 특성은 Visual Studio 또는 다른 XAML 디자이너 화면에서 만드는 XAML 파일의 루트에 적용됩니다. 예를 들어 앱 프로젝트에 새 **UserControl**을 추가할 경우 만들어지는 XAML의 [**UserControl**](https://msdn.microsoft.com/library/windows/apps/br227647) 루트에 이 특성이 설정됩니다. 두 특성을 사용하여 XAML 콘텐츠 컴퍼지션을 쉽게 디자인할 수 있으므로 XAML 콘텐츠를 컨트롤 인스턴스나 큰 UI 페이지의 다른 부분에 사용할 경우 발생할 수 있는 레이아웃 제약 조건을 어느 정도 예상할 수 있습니다.

   **참고**Microsoft Silverlight에서 XAML을 마이그레이션하는 경우 전체 UI 페이지를 나타내는 루트 요소에 이러한 특성을 가질 수 있습니다. 이 경우 특성을 제거하는 것이 좋습니다. 시뮬레이터 같은 XAML 디자이너의 다른 기능은 **d:DesignHeight** 및 **d:DesignWidth**를 사용한 고정 크기 페이지 레이아웃보다 크기 조정 및 보기 상태를 처리하는 페이지 레이아웃 디자인에 더 유용합니다.

-   **d:DataContext 특성:** 페이지 루트나 컨트롤에 이 특성을 설정하면 특성이 없을 경우 개체에 포함되는 명시적 또는 상속된 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713)를 재정의할 수 있습니다.
-   **d:DesignSource 특성:** [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/br209833)에 대해 디자인 타임 데이터 원본을 지정하고 [**Source**](https://msdn.microsoft.com/library/windows/apps/br209835)를 재정의합니다.
-   **d:DesignInstance 및 d:DesignData 태그 확장:** 두 태그 확장은 **d:DataContext** 또는 **d:DesignSource**에 대한 디자인 타임 데이터 리소스를 제공하는 데 사용됩니다. 디자인 타임 데이터 리소스를 사용하는 방법에 대해서는 여기서 자세히 설명하지 않습니다. 자세한 내용은 [디자인 타임 특성](http://go.microsoft.com/fwlink/p/?LinkId=272504)을 참조하세요. 일부 사용 예제는 [디자인 화면의 샘플 데이터 및 프로토타입 생성용 샘플 데이터](https://msdn.microsoft.com/library/windows/apps/mt517866)를 참조하세요.

### **<a name="mc-httpschemasopenxmlformatsorgmarkup-compatibility2006"></a>mc: (`http://schemas.openxmlformats.org/markup-compatibility/2006`)**

"mc:"는 XAML 읽기에 필요한 태그 호환 모드를 나타내고 지원합니다. 일반적으로 "d:" 접두사는 **mc:Ignorable** 특성과 관련됩니다. 이 기술을 통해 런타임 XAML 파서에서 "d:"의 디자인 특성을 무시할 수 있습니다.

### <a name="local-and-common"></a>**local:** 및 **common:**

"local:" 템플릿 기반의 UWP 앱 프로젝트에 대해 XAML 페이지 내에서 자주 매핑되는 접두사입니다. [x:Class 특성](x-class-attribute.md) 및 app.xaml을 비롯한 모든 XAML 파일에 대한 코드를 포함하도록 생성된 동일한 네임스페이스를 참조하도록 매핑되어 있습니다. 이 동일한 네임스페이스에서 XAML에 사용할 사용자 지정 클래스를 정의하는 경우 **local:** 접두사를 사용하여 XAML의 사용자 지정 유형을 참조할 수 있습니다. 템플릿 기반의 UWP 앱 프로젝트에서 가져온 관련 접두사는 **common:** 입니다. 이 접두사는 변환기 및 명령과 같은 유틸리티 클래스를 포함하고 있는 중첩된 "Common" 네임스페이스를 참조하며, 개발자는 **솔루션 탐색기** 보기에서 Common 폴더에 있는 정의를 찾을 수 있습니다.

### **<a name="vsm"></a>vsm:**

사용하지 마세요. "vsm:"은 다른 Microsoft 기술에서 가져온 이전 XAML 템플릿에서 볼 수 있는 접두사입니다. 이 네임스페이스는 원래 레거시 네임스페이스 도구 문제를 해결했습니다. Windows 런타임용으로 사용하는 모든 XAML에서 "vsm:"에 대한 XAML 네임스페이스 정의를 삭제하고 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007), [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/br209014) 및 관련 개체에 대한 접두사 사용을, 기본 XAML 네임스페이스를 대신 사용하도록 변경해야 합니다. XAML 마이그레이션에 대한 자세한 내용은 [Windows 런타임 앱으로 Silverlight 또는 WPF XAML/코드 마이그레이션](https://msdn.microsoft.com/library/windows/apps/br229571)을 참조하세요.

## <a name="mapping-custom-types-to-xaml-namespaces-and-prefixes"></a>XAML 네임스페이스 및 접두사에 사용자 지정 유형 매핑

고유 사용자 지정 유형에 액세스하는 데 XAML을 사용할 수 있도록 XAML 네임스페이스를 매핑할 수 있습니다. 다시 말해 사용자 지정 유형을 정의하는 코드 표현에 있는 그대로 코드 네임스페이스를 매핑한 후 사용을 위한 접두사와 함께 XAML 네임스페이스에 할당합니다. XAML의 사용자 지정 유형은 Microsoft .NET 언어(C# 또는 Microsoft Visual Basic) 또는 C++로 정의될 수 있습니다. 매핑은 **xmlns** 접두사를 정의하여 수행합니다. 예를 들어 `xmlns:myTypes`에서는 `myTypes:` 토큰으로 모든 사용에 접두사를 지정하여 액세스하는 새 XAML 네임스페이스를 정의합니다.

**xmlns** 정의는 접두사 명명은 물론 값도 포함합니다. 이 값은 등호 뒤, 따옴표 안에 오는 문자열입니다. 일반적인 XML 규칙은 고유성 및 식별 규칙이 존재하도록 XML 네임스페이스를 URI(Uniform Resource Identifier)와 연결하는 것입니다. Windows 런타임 XAML에서 드물게 사용되는 일부 XAML 네임스페이스는 물론 기본 XAML 네임스페이스와 XAML 언어 XAML 네임스페이스에서도 이 규칙을 볼 수 있습니다. 그러나 URI를 지정하지 않고 사용자 지정 유형을 매핑하는 XAML 네임스페이스의 경우 "using:" 토큰을 사용하여 접두사 정의를 시작합니다. "using:" 토큰 다음에는 코드 네임스페이스를 명명합니다.

예를 들어 "CustomClasses" 네임스페이스를 참조할 수 있는 "custom1" 접두사를 매핑하여 해당 네임스페이스 또는 어셈블리의 클래스를 XAML의 개체 요소로 사용하려면 XAML 페이지에 루트 요소에 대한 다음 매핑이 있어야 합니다. `xmlns:custom1="using:CustomClasses"`

동일한 페이지 범위의 partial 클래스는 매핑할 필요가 없습니다. 예를 들어 페이지 XAML UI 정의의 이벤트를 처리하기 위해 정의한 이벤트 처리기를 참조하는 데는 접두사가 필요하지 않습니다. C++, C# 또는 Visual Basic으로 작성된 Windows 런타임 앱 프로젝트가 생성된 Visual Studio의 여러 시작 XAML 페이지도 이미 프로젝트에서 지정한 기본 네임스페이스와 partial 클래스 정의에 사용된 네임스페이스를 참조하는 "local:" 접두사를 매핑합니다.

### <a name="clr-language-rules"></a>CLR 언어 규칙

지원 코드를 .NET 언어(C# 또는 Microsoft Visual Basic)로 작성하는 경우 코드 네임스페이스의 개념 계층을 만들기 위해 점(".")을 네임스페이스 이름의 일부로 사용하는 규칙을 사용할 수 있습니다. 네임스페이스 정의에 점이 포함되는 경우 이 점은 "using:" 토큰 뒤에 지정하는 값의 일부여야 합니다.

코드 숨김 파일 또는 코드 정의 파일이 C++ 파일일 경우 CLR(공용 언어 런타임) 언어 폼을 계속 따르는 특정 규칙이 있어서 XAML 구문에서 차이가 없습니다. C++에서 중첩 네임스페이스를 선언하면 "using:" 토큰 뒤의 값을 지정하는 경우 연속된 중첩 네임스페이스 문자열 사이의 구분 기호가 "::"이 아닌 "."여야 합니다.

XAML에서 사용할 코드를 정의할 때 중첩 유형(예: 클래스 내에 열거 중첩)을 사용하지 마세요. 중첩 유형은 평가할 수 없습니다. XAML 파서는 점이 네임스페이스 이름의 일부가 아니라 중첩 형식 이름의 일부인지 구별할 수 없습니다.

## <a name="custom-types-and-assemblies"></a>사용자 지정 유형 및 어셈블리

XAML 네임스페이스에 대한 지원 유형을 정의하는 어셈블리 이름은 매핑에서 지정되지 않습니다. 사용 가능한 어셈블리에 대한 논리는 앱 정의 수준에서 제어되며 기본 앱 배포 및 보안 원칙의 일부입니다. XAML에 대한 코드 정의 소스로 포함시키려는 모든 어셈블리를 프로젝트 설정의 종속 어셈블리로 선언하세요. 자세한 내용은 [C# 및 Visual Basic에서 Windows 런타임 구성 요소 만들기](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)를 참조하세요.

기본 앱의 응용 프로그램 정의 또는 페이지 정의에서 사용자 지정 유형을 참조하는 경우 추가 종속 어셈블리 구성 없이 해당 유형을 사용할 수 있으나 여전히 해당 유형이 포함된 코드 네임스페이스를 매핑해야 합니다. 일반적인 규칙은 지정된 XAML 페이지의 기본 코드 네임스페이스에 대해 "local" 접두사를 매핑하는 것입니다. 이 규칙은 XAML 프로젝트의 시작 프로젝트 템플릿에 포함되는 경우가 많습니다.

## <a name="attached-properties"></a>연결된 속성

연결된 속성을 참조하는 경우 연결된 속성 이름이 소유자 형식 부분은 기본 XAML 네임스페이스로 사용되거나 접두사를 붙일 수 있습니다. 특성 요소와 별개로 특정에 접두사를 붙이는 경우는 드물지만 가끔 필요할 때, 특히 사용자 지정 연결된 속성의 경우에는 접두사를 붙입니다. 자세한 내용은 [사용자 지정 연결된 속성](custom-attached-properties.md)을 참조하세요.

## <a name="related-topics"></a>관련 항목

* [XAML 개요](xaml-overview.md)
* [XAML 구문 가이드](xaml-syntax-guide.md)
* [C# 및 Visual Basic에서 Windows 런타임 구성 요소 만들기](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)
* [Windows 런타임 앱용 C#, VB 및 C++ 프로젝트 템플릿](https://msdn.microsoft.com/library/windows/apps/hh768232)
* [Silverlight 또는 WPF XAML/코드를 Windows 런타임 앱으로 마이그레이션](https://msdn.microsoft.com/library/windows/apps/br229571)
 

