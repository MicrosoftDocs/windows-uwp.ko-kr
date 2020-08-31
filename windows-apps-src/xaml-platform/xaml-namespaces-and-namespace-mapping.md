---
description: 이 항목에서는 대부분의 XAML 파일의 루트 요소에 있는 XML/XAML 네임 스페이스 (xmlns) 매핑에 대해 설명 합니다. 또한 사용자 지정 형식 및 어셈블리에 대해 비슷한 매핑을 생성 하는 방법에 대해서도 설명 합니다.
title: XAML 네임스페이스 및 네임스페이스 매핑
ms.assetid: A19DFF78-E692-47AE-8221-AB5EA9470E8B
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 557301873cbea09d3601b09254c5a296ceb9fc82
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168997"
---
# <a name="xaml-namespaces-and-namespace-mapping"></a>XAML 네임스페이스 및 네임스페이스 매핑


이 항목에서는 대부분의 XAML 파일의 루트 요소에 있는 XML/XAML 네임 스페이스 (**xmlns**) 매핑에 대해 설명 합니다. 또한 사용자 지정 형식 및 어셈블리에 대해 비슷한 매핑을 생성 하는 방법에 대해서도 설명 합니다.

## <a name="how-xaml-namespaces-relate-to-code-definition-and-type-libraries"></a>XAML 네임 스페이스를 코드 정의 및 형식 라이브러리와 연결 하는 방법

일반적으로 응용 프로그램을 Windows 런타임 하는 응용 프로그램의 경우에는 XAML을 사용 하 여 개체, 해당 개체의 속성 및 계층으로 표시 되는 개체 속성 관계를 선언 합니다. XAML에서 선언 하는 개체는 형식 라이브러리 또는 다른 프로그래밍 기술 및 언어에서 정의 하는 다른 표현에 의해 지원 됩니다. 이러한 라이브러리는 다음과 같을 수 있습니다.

-   Windows 런타임에 대 한 기본 제공 개체 집합입니다. 이는 고정 된 개체 집합 이며 XAML에서 이러한 개체에 액세스 하려면 내부 형식 매핑 및 활성화 논리를 사용 합니다.
-   Microsoft 또는 타사에서 제공 하는 분산 라이브러리
-   앱이 통합 하 고 패키지를 재배포 하는 타사 컨트롤의 정의를 나타내는 라이브러리입니다.
-   프로젝트에 포함 되 고 사용자 코드 정의의 일부 또는 전부를 포함 하는 고유한 라이브러리.

지원 형식 정보는 특정 XAML 네임 스페이스 정의와 연결 됩니다. Windows 런타임와 같은 XAML 프레임 워크는 여러 어셈블리와 여러 코드 네임 스페이스를 집계 하 여 단일 XAML 네임 스페이스에 매핑할 수 있습니다. 이렇게 하면 더 큰 프로그래밍 프레임 워크 또는 기술을 포함 하는 XAML 어휘 개념을 사용할 수 있습니다. XAML 어휘는 매우 광범위 하 게 사용할 수 있습니다. 예를 들어이 참조에서 Windows 런타임 앱에 대해 문서화 된 대부분의 XAML은 단일 XAML 어휘를 구성 합니다. XAML 어휘도 확장 가능 합니다. 즉, 지원 코드 정의에 형식을 추가 하 여 확장 하 여 XAML 어휘에 대 한 매핑된 네임 스페이스 소스로 이미 사용 되는 코드 네임 스페이스에 형식을 포함 해야 합니다.

XAML 프로세서는 런타임 개체 표현을 만들 때 해당 XAML 네임 스페이스와 연결 된 지원 어셈블리에서 형식 및 멤버를 조회할 수 있습니다. 이러한 이유로 XAML은 개체 생성 동작의 정의를 공식화 하 고 교환 하는 방법 및 XAML이 UWP 앱에 대 한 UI 정의 기술로 사용 되는 이유 때문에 유용 합니다.

## <a name="xaml-namespaces-in-typical-xaml-markup-usage"></a>일반적인 XAML 태그 사용의 XAML 네임 스페이스

XAML 파일은 거의 항상 루트 요소에서 기본 XAML 네임 스페이스를 선언 합니다. 기본 XAML 네임 스페이스는 접두사를 사용 하 여 한정 하지 않고 선언할 수 있는 요소를 정의 합니다. 예를 들어 요소를 선언 하는 경우 `<Balloon />` XAML 파서는 요소 **풍선이** 있고 기본 xaml 네임 스페이스에서 유효 하다 고 간주 합니다. 이와 대조적으로, **풍선이** 정의 된 기본 XAML 네임 스페이스에 없는 경우 접두사를 사용 하 여 해당 요소 이름을 한 정해야 합니다 (예:) `<party:Balloon />` . 접두사는 요소가 기본 네임 스페이스와 다른 XAML 네임 스페이스에 존재 함을 나타내며,이 요소를 사용 하려면 먼저 XAML 네임 스페이스를 접두사 **파티** 에 매핑해야 합니다. XAML 네임 스페이스는 선언 된 특정 요소 뿐만 아니라 XAML 구조체의 해당 요소에 포함 된 모든 요소에 적용 됩니다. 이러한 이유로 xaml 네임 스페이스는 거의 항상 XAML 파일의 루트 요소에서 선언 되므로이 상속을 활용할 수 있습니다.

## <a name="the-default-and-xaml-language-xaml-namespace-declarations"></a>기본 및 XAML 언어 XAML 네임 스페이스 선언

대부분의 XAML 파일의 루트 요소 내에는 두 개의 **xmlns** 선언이 있습니다. 첫 번째 선언은 XAML 네임 스페이스를 기본값으로 매핑합니다. `xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"`

이는 XAML을 UI 정의 태그 형식으로 사용 하는 여러 선행 Microsoft 기술에도 사용 되는 동일한 XAML 네임 스페이스 식별자입니다. 동일한 식별자를 사용 하는 것은 의도적인 것 이며, c + +, c # 또는 Visual Basic를 사용 하 여 이전에 정의한 UI를 Windows 런타임 앱으로 마이그레이션하는 경우에 유용 합니다.

두 번째 선언은 XAML 정의 언어 요소에 대 한 별도의 XAML 네임 스페이스를 매핑하고 (일반적으로)를 "x:" 접두사에 매핑합니다. `xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"`

이 **xmlns** 값과이 값이 매핑되는 "x:" 접두사는 XAML을 사용 하는 여러 선행 Microsoft 기술에 사용 되는 정의와 동일 합니다.

이러한 선언 간의 관계는 XAML은 언어 정의이 고 Windows 런타임는 xaml을 언어로 사용 하 고 해당 형식이 XAML에서 참조 되는 특정 어휘를 정의 하는 하나의 구현입니다.

XAML 언어는 특정 언어 요소를 지정 하 고 이러한 각 요소는 XAML 네임 스페이스에 대해 작동 하는 XAML 프로세서 구현을 통해 액세스할 수 있어야 합니다. XAML 언어 XAML 네임 스페이스에 대 한 "x:" 매핑 규칙 뒤에는 프로젝트 템플릿, 샘플 코드 및 언어 기능에 대 한 설명서가 나옵니다. XAML 언어 네임 스페이스는 c + +, c # 또는 Visual Basic를 사용 하는 기본 Windows 런타임 앱에도 필요한 몇 가지 자주 사용 되는 기능을 정의 합니다. 예를 들어 partial 클래스를 통해 코드 숨김이 XAML 파일에 조인 하려면 해당 클래스의 이름을 관련 XAML 파일의 루트 요소에 있는 [x:Class 특성](x-class-attribute.md) 으로 만들어야 합니다. 또는 XAML 페이지에 [ResourceDictionary 및 xaml 리소스 참조](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md) 의 키가 있는 리소스로 정의 된 모든 요소는 해당 개체 요소에 대해 [x:Key 특성이](x-key-attribute.md) 설정 되어 있어야 합니다.

## <a name="code-namespaces-that-map-to-the-default-xaml-namespace"></a>기본 XAML 네임 스페이스에 매핑되는 코드 네임 스페이스

다음는 현재 기본 XAML 네임 스페이스에 매핑되는 코드 네임 스페이스의 목록입니다.

* Windows.UI
* Windows.UI.Xaml
* Windows.UI.Xaml.Automation
* Windows.UI.Xaml.Automation.Peers
* Windows.UI.Xaml.Automation.Provider
* Windows.UI.Xaml.Automation.Text
* Windows.UI.Xaml.Controls
* Windows.UI.Xaml.Controls.Primitives
* Windows.UI.Xaml.Data
* Windows.UI.Xaml.Documents
* Windows.UI.Xaml.Input
* Windows.UI.Xaml.Interop
* Windows.UI.Xaml.Markup
* Windows.UI.Xaml.Media
* Windows.UI.Xaml.Media.Animation
* Windows.UI.Xaml.Media.Imaging
* Windows.UI.Xaml.Media.Media3D
* Windows.UI.Xaml.Navigation
* Windows.UI.Xaml.Resources
* Windows.UI.Xaml.Shapes
* Windows. .Xaml. 스레딩
* Windows.UI.Text

<span id="other-XAML-namespaces"/>

## <a name="other-xaml-namespaces"></a>다른 XAML 네임 스페이스

기본 네임 스페이스와 XAML 언어 XAML 네임 스페이스 "x:" 외에도 Microsoft Visual Studio에서 생성 된 앱에 대 한 초기 기본 XAML에서 다른 매핑된 XAML 네임 스페이스를 볼 수 있습니다.

### <a name="d-httpschemasmicrosoftcomexpressionblend2008"></a>**d: ( `http://schemas.microsoft.com/expression/blend/2008` )**

"D:" XAML 네임 스페이스는 Microsoft Visual Studio의 XAML 디자인 화면에서 디자이너 지원과 같이 디자이너를 지원 하기 위한 것입니다. "D:" XAML 네임 스페이스는 XAML 요소에 디자이너 또는 디자인 타임 특성을 사용 합니다. 이러한 디자이너 특성은 XAML 동작의 디자인 측면에만 영향을 줍니다. 응용 프로그램이 실행 될 때 Windows 런타임 XAML 파서에서 동일한 XAML이 로드 되 면 디자이너 특성이 무시 됩니다. 일반적으로 디자이너 특성은 모든 XAML 요소에서 유효 하지만 실제로 디자이너 특성을 직접 적용 하는 것이 적절 한 시나리오도 있습니다. 특히 디자이너 특성의 대부분은 데이터 바인딩을 사용 하는 XAML 및 코드를 개발 하는 동안 데이터 컨텍스트 및 데이터 소스와 상호 작용 하는 데 더 나은 환경을 제공 하기 위한 것입니다.

-   **d:DesignHeight 및 d:DesignWidth 특성:** 이러한 특성은 Visual Studio 또는 다른 XAML 디자이너 화면에서 만드는 XAML 파일의 루트에 적용 되는 경우도 있습니다. 예를 들어 이러한 특성은 새 **usercontrol** 을 앱 프로젝트에 추가 하는 경우 만들어지는 XAML의 [**UserControl**](/uwp/api/Windows.UI.Xaml.Controls.UserControl) 루트에 설정 됩니다. 이러한 특성을 사용 하면 xaml 콘텐츠의 컴퍼지션을 보다 쉽게 디자인할 수 있습니다. 따라서 XAML 콘텐츠를 컨트롤 인스턴스나 더 큰 UI 페이지의 다른 부분에 사용 하는 경우에도 존재 하는 레이아웃 제약 조건을 미리 결정할 수 있습니다.

   **참고**    Microsoft Silverlight에서 XAML을 마이그레이션하는 경우 전체 UI 페이지를 나타내는 루트 요소에 이러한 특성이 있을 수 있습니다. 이 경우 특성을 제거 하는 것이 좋습니다. 시뮬레이터와 같은 XAML 디자이너의 다른 기능은 **d:DesignHeight** 및 **d:DesignWidth**를 사용 하는 고정 크기 페이지 레이아웃 보다 크기 조정 및 보기 상태를 처리 하는 페이지 레이아웃을 디자인 하는 데 더 유용할 수 있습니다.

-   **d:DataContext 특성:** 페이지 루트 또는 컨트롤에 대해이 특성을 설정 하 여 개체에 포함 된 명시적 또는 상속 된 [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 를 재정의할 수 있습니다.
-   **d:DesignSource 특성:** [**소스**](/uwp/api/windows.ui.xaml.data.collectionviewsource.source)를 재정의 하는 [**collectionviewsource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource)에 대 한 디자인 타임 데이터 소스를 지정 합니다.
-   **d:DesignInstance 및 d:DesignData 태그 확장:** 이러한 태그 확장은 **d:DataContext** 또는 **d:DesignSource**에 대 한 디자인 타임 데이터 리소스를 제공 하는 데 사용 됩니다. 여기서는 디자인 타임 데이터 리소스를 사용 하는 방법을 완벽 하 게 문서화 하지 않습니다. 자세한 내용은 [디자인 타임 특성](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ff602277(v=vs.95))을 참조 하세요. 몇 가지 사용 예제는 [디자인 화면의 샘플 데이터 및 프로토타입](../data-binding/displaying-data-in-the-designer.md)작성을 참조 하세요.

### <a name="mc-httpschemasopenxmlformatsorgmarkup-compatibility2006"></a>**mc: ( `http://schemas.openxmlformats.org/markup-compatibility/2006` )**

"mc:"는 XAML을 읽기 위한 태그 호환성 모드를 나타내고 지원 합니다. 일반적으로 "d:" 접두사는 **mc: Ignorable**특성에 연결 됩니다. 이 방법을 사용 하면 런타임 XAML 파서가 "d:"에서 디자인 특성을 무시할 수 있습니다.

### <a name="local-and-common"></a>**로컬:** 및 **일반:**

"local:"은 템플릿 기반 UWP 앱 프로젝트에 대 한 XAML 페이지 내에서 자주 매핑되는 접두사입니다. 이 클래스는 app.xaml [특성과](x-class-attribute.md) app.xaml을 비롯 한 모든 XAML 파일에 대 한 코드를 포함 하기 위해 만든 동일한 네임 스페이스를 참조 하도록 매핑됩니다. 이 네임 스페이스의 XAML에서 사용할 사용자 지정 클래스를 정의 하는 경우 **local:** prefix를 사용 하 여 xaml에서 사용자 지정 형식을 참조할 수 있습니다. 템플릿 기반 UWP 앱 프로젝트에서 제공 되는 관련 접두사는 **일반적**입니다. 이 접두사는 변환기 및 명령과 같은 유틸리티 클래스를 포함 하는 중첩 된 "Common" 네임 스페이스를 나타내며 **솔루션 탐색기** 뷰의 공용 폴더에서 정의를 찾을 수 있습니다.

### <a name="vsm"></a>**vsm**

사용하지 마십시오. "vsm:"은 다른 Microsoft 기술에서 가져온 이전 XAML 템플릿에서 때때로 표시 되는 접두사입니다. 네임 스페이스는 원래 레거시 네임 스페이스 도구 문제를 해결 했습니다. Windows 런타임에 사용 하는 모든 XAML에서 "vsm:"에 대 한 XAML 네임 스페이스 정의를 삭제 하 고 대신 기본 XAML 네임 스페이스를 사용 하도록 [**Visualstate**](/uwp/api/Windows.UI.Xaml.VisualState), [**visualstategroup**](/uwp/api/Windows.UI.Xaml.VisualStateGroup) 및 관련 개체에 대 한 접두사 사용을 변경 해야 합니다. XAML 마이그레이션에 대 한 자세한 내용은 [Silverlight 또는 WPF XAML/code를 Windows 런타임 앱으로 마이그레이션](/previous-versions/windows/apps/br229571(v=win.10))을 참조 하세요.

## <a name="mapping-custom-types-to-xaml-namespaces-and-prefixes"></a>XAML 네임 스페이스 및 접두사에 사용자 지정 형식 매핑

고유 사용자 지정 유형에 액세스하는 데 XAML을 사용할 수 있도록 XAML 네임스페이스를 매핑할 수 있습니다. 즉, 사용자 지정 형식을 정의 하 고이를 사용 하기 위해 접두사와 함께 XAML 네임 스페이스를 할당 하는 코드 표현에 있는 코드 네임 스페이스를 매핑합니다. XAML에 대 한 사용자 지정 형식은 Microsoft .NET 언어 (c # 또는 Microsoft Visual Basic) 또는 c + +에서 정의할 수 있습니다. 매핑은 **xmlns** 접두사를 정의 하 여 수행 됩니다. 예를 들어는 `xmlns:myTypes` 토큰을 사용 하 여 모든 사용을 접두사로 사용 하 여 액세스 되는 새 XAML 네임 스페이스를 정의 합니다 `myTypes:` .

**Xmlns** 정의는 접두사 명명 뿐만 아니라 값을 포함 합니다. 값은 등호 뒤에 나오는 따옴표 안에 표시 되는 문자열입니다. 일반적인 XML 규칙은 XML 네임 스페이스를 URI (Uniform Resource Identifier)와 연결 하 여 고유성 및 식별 규칙을 사용 하는 것입니다. 또한 기본 XAML 네임 스페이스와 XAML 언어 XAML 네임 스페이스 뿐만 아니라 Windows 런타임 XAML에서 사용 되는 덜 사용 되는 XAML 네임 스페이스에 대 한이 규칙도 볼 수 있습니다. 그러나 URI를 지정 하는 대신 사용자 지정 형식을 매핑하는 XAML 네임 스페이스의 경우 "using:" 토큰을 사용 하 여 접두사 정의를 시작 합니다. "Using:" 토큰을 따라 코드 네임 스페이스의 이름을로 합니다.

예를 들어 "CustomClasses" 네임 스페이스를 참조 하 고 해당 네임 스페이스 또는 어셈블리의 클래스를 XAML의 개체 요소로 사용할 수 있도록 하는 "custom1" 접두사를 매핑하려면 XAML 페이지에 루트 요소에 대 한 다음 매핑이 포함 되어야 합니다. `xmlns:custom1="using:CustomClasses"`

동일한 페이지 범위의 부분 클래스를 매핑할 필요가 없습니다. 예를 들어 페이지의 XAML UI 정의에서 이벤트를 처리 하기 위해 정의한 모든 이벤트 처리기를 참조 하는 데 접두사가 필요 하지 않습니다. 또한 c + +, c # 또는 Visual Basic를 사용 하 여 Windows 런타임 앱에 대해 생성 된 프로젝트의 Visual Studio 시작 XAML 페이지는 대부분 프로젝트 지정 기본 네임 스페이스와 부분 클래스 정의에 사용 되는 네임 스페이스를 참조 하는 "local:" 접두사를 이미 매핑합니다.

### <a name="clr-language-rules"></a>CLR 언어 규칙

.NET 언어 (c # 또는 Microsoft Visual Basic)에서 지원 코드를 작성 하는 경우 네임 스페이스 이름의 일부로 점 (".")을 사용 하는 규칙을 사용 하 여 코드 네임 스페이스의 개념적 계층 구조를 만들 수 있습니다. 네임 스페이스 정의에 점이 포함 되어 있으면 "using:" 토큰 후에 지정 하는 값의 일부가 점입니다.

코드 기반 파일이 나 코드 정의 파일이 c + + 파일인 경우 XAML 구문에 차이가 없도록 CLR (공용 언어 런타임) 언어 폼을 따르는 특정 규칙이 있습니다. C + +에서 중첩 된 네임 스페이스를 선언 하는 경우 "using:" 토큰 다음에 오는 값을 지정할 때 연속 된 중첩 된 네임 스페이스 문자열 간의 구분 기호는 "::"이 아니라 "." 여야 합니다.

XAML과 함께 사용 하기 위해 코드를 정의할 때 중첩 형식 (예: 클래스 내에 열거형 중첩)을 사용 하지 마세요. 중첩 형식은 평가할 수 없습니다. XAML 파서는 점이 네임 스페이스 이름의 일부가 아니라 중첩 된 형식 이름의 일부임을 구분할 수 있는 방법이 없습니다.

## <a name="custom-types-and-assemblies"></a>사용자 지정 형식 및 어셈블리

XAML 네임 스페이스에 대 한 지원 형식을 정의 하는 어셈블리의 이름이 매핑에서 지정 되지 않았습니다. 어셈블리를 사용할 수 있는 논리는 앱 정의 수준에서 제어 되며 기본 앱 배포 및 보안 원칙의 일부입니다. XAML에 대 한 코드 정의 소스로 포함 하려는 모든 어셈블리를 프로젝트 설정에서 종속 어셈블리로 선언 합니다. 자세한 내용은 [c #에서 Windows 런타임 구성 요소 만들기 및 Visual Basic](/previous-versions/windows/apps/hh441572(v=vs.140))를 참조 하세요.

기본 앱의 응용 프로그램 정의 또는 페이지 정의에서 사용자 지정 형식을 참조 하는 경우에는 추가 종속 어셈블리 구성 없이 해당 형식을 사용할 수 있지만 여전히 이러한 형식을 포함 하는 코드 네임 스페이스를 매핑해야 합니다. 일반적인 규칙은 지정 된 XAML 페이지의 기본 코드 네임 스페이스에 대 한 접두사 "local"을 매핑하는 것입니다. 이 규칙은 종종 XAML 프로젝트용 프로젝트 템플릿 시작에 포함 됩니다.

## <a name="attached-properties"></a>연결된 속성

연결 된 속성을 참조 하는 경우 연결 된 속성 이름의 소유자 형식 부분은 기본 XAML 네임 스페이스에 있거나 접두사가 있어야 합니다. 특성의 요소와는 별도로 접두사를 지정 하는 것은 드문 경우 이지만 특히 사용자 지정 연결 된 속성의 경우에는 반드시 필요한 경우가 있습니다. 자세한 내용은 [사용자 지정 연결 된 속성](custom-attached-properties.md)을 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [XAML 개요](xaml-overview.md)
* [XAML 구문 가이드](xaml-syntax-guide.md)
* [C # 및 Visual Basic에서 Windows 런타임 구성 요소 만들기](/previous-versions/windows/apps/hh441572(v=vs.140))
* [Windows 런타임 앱에 대 한 c #, VB 및 c + + 프로젝트 템플릿](/previous-versions/windows/apps/hh768232(v=win.10))
* [Silverlight 또는 WPF XAML/코드를 Windows 런타임 앱으로 마이그레이션](/previous-versions/windows/apps/br229571(v=win.10))
 