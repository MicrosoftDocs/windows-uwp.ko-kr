---
description: 코드 숨김이 나 일반 코드에서 인스턴스화된 개체에 액세스할 수 있는 개체 요소를 고유 하 게 식별 합니다.
title: 명령의 xname 특성
ms.assetid: 4FF1F3ED-903A-4305-B2BD-DCD29E0C9E6D
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b5fd9c2b482eb79fcdace2c068afa21bad673de2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161677"
---
# <a name="xname-attribute"></a>x:Name 특성


코드 숨김이 나 일반 코드에서 인스턴스화된 개체에 액세스할 수 있는 개체 요소를 고유 하 게 식별 합니다. 지원 프로그래밍 모델에 적용 되 면 **x:Name** 은 생성자에서 반환 된 개체 참조를 보유 하는 변수와 동일한 것으로 간주할 수 있습니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object x:Name="XAMLNameValue".../>
```

## <a name="xaml-values"></a>XAML 값

| 용어 | Description |
|------|-------------|
| XAMLNameValue | XamlName 문법의 제한 사항을 준수 하는 문자열입니다. |

##  <a name="xamlname-grammar"></a>XamlName 문법

다음은이 XAML 구현에서 키로 사용 되는 문자열에 대 한 규범 문법입니다.

``` syntax
XamlName ::= NameStartChar (NameChar)*
NameStartChar ::= LetterCharacter | '_'
NameChar ::= NameStartChar | DecimalDigit
LetterCharacter ::= ('a'-'z') | ('A'-'Z')
DecimalDigit ::= '0'-'9'
CombiningCharacter::= none
```

-   문자는 더 낮은 ASCII 범위로 제한 되며 보다 구체적으로는 로마 알파벳 대 문자와 소문자, 숫자 및 밑줄 () 문자로 제한 됩니다 \_ .
-   유니코드 문자 범위는 지원 되지 않습니다.
-   이름은 숫자로 시작할 수 없습니다. \_사용자가 숫자를 초기 문자로 제공 하거나 도구에서 숫자가 포함 된 다른 값을 기준으로 **x:Name** 값을 경로도 하는 경우 일부 도구 구현은 문자열에 밑줄 ()을 추가 합니다.

## <a name="remarks"></a>설명

지정 된 **x:Name** 은 XAML이 처리 될 때 기본 코드에서 생성 되는 필드 이름이 되 고, 해당 필드에는 개체에 대 한 참조가 포함 됩니다. 이 필드를 만드는 프로세스는 XAML 파일 및 해당 코드 숨김으로 partial 클래스를 조인 하는 것을 담당 하는 MSBuild 대상 단계에 의해 수행 됩니다. 이 동작은 XAML 언어가 반드시 지정 되는 것은 아닙니다. XAML에 대 한 UWP (유니버설 Windows 플랫폼) 프로그래밍은 해당 프로그래밍 및 응용 프로그램 모델에서 **x:Name** 을 사용 하기 위해 적용 되는 특정 구현입니다.

정의 된 각 **x:Name** 은 xaml 이름 범위 내에서 고유 해야 합니다. 일반적으로 XAML 이름 범위는 로드 된 페이지의 루트 요소 수준에서 정의 되며 단일 XAML 페이지에서 해당 요소 아래에 있는 모든 요소를 포함 합니다. 추가 XAML 이름 범위는 해당 페이지에 정의 된 컨트롤 템플릿 또는 데이터 템플릿에 의해 정의 됩니다. 런타임에, 적용된 컨트롤 템플릿에서 만들어진 개체 트리의 루트에 대해, 그리고 [**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load) 호출에서 만들어진 개체 트리에 의해 다른 XAML 이름 범위가 만들어집니다. 자세한 내용은 [XAML 이름 범위](xaml-namescopes.md)를 참조 하세요.

디자인 도구는 요소를 디자인 화면에 도입할 때 요소에 대해 **x:Name** 값을 자동으로 생성 하는 경우가 많습니다. 자동 생성 구성표는 사용 중인 디자이너에 따라 다르지만 일반적인 체계는 요소를 지 원하는 클래스 이름으로 시작 하는 문자열을 생성 하 고 그 다음에 이동 하는 정수를 생성 하는 것입니다. 예를 들어, 디자이너에 첫 번째 [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) 요소를 도입 하는 경우 XAML에서이 요소의 **x:Name** 특성 값은 "Button1"이 됩니다.

**x:Name** 은 XAML 속성 요소 구문이 나 [**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue)를 사용 하는 코드에서 설정할 수 없습니다. **x:Name** 은 요소에 대해 XAML 특성 구문을 사용 하는 경우에만 설정할 수 있습니다.

**참고**    특히 c + +/CX 응용 프로그램의 경우 XAML 파일 또는 페이지의 루트 요소에 대해 **x:Name** 참조의 지원 필드가 생성 되지 않습니다. C + + 코드 숨김으로 루트 개체를 참조 해야 하는 경우 다른 Api 또는 트리 트래버스를 사용 합니다. 예를 들어 알려진 명명 된 자식 요소에 대해 [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) 을 호출한 다음 [**Parent**](/uwp/api/windows.ui.xaml.frameworkelement.parent)를 호출할 수 있습니다.

### <a name="xname-and-other-name-properties"></a>x:Name 및 기타 이름 속성

UWP XAML에서 사용 되는 일부 형식에는 **이름이 Name**인 속성도 있습니다. 예를 들면 [**FrameworkElement.Name**](/uwp/api/windows.ui.xaml.frameworkelement.name) 및 [**TextElement.Name**](/uwp/api/windows.ui.xaml.documents.textelement.name)입니다.

**이름을** 요소에 대해 설정 가능한 속성으로 사용할 수 있는 경우 XAML에서 **name** 및 **x:Name** 을 서로 바꿔 사용할 수 있지만 동일한 요소에 두 특성이 모두 지정 된 경우 오류가 발생 합니다. **이름** 속성이 있지만 읽기 전용 인 경우도 있습니다 (예: [**VisualState.Name**](/uwp/api/windows.ui.xaml.visualstate.name)). 해당 하는 경우 항상 **x:Name** 을 사용 하 여 XAML에서 해당 요소의 이름을 표시 하 고, 일부 낮은 공통 코드 시나리오에 대해 읽기 전용 **이름이** 존재 합니다.

**참고**  [**FrameworkElement.Name**](/uwp/api/windows.ui.xaml.frameworkelement.name) 는 일반적으로 **x:Name**에 의해 설정 된 값을 변경 하는 방법으로 사용 하면 안 됩니다. 단, 해당 일반 규칙에 대 한 예외가 있는 경우도 있습니다. 일반적인 시나리오에서 XAML 이름 범위를 만들고 정의 하는 작업은 XAML 프로세서 작업입니다. 런타임에 **FrameworkElement.Name** 를 수정 하면 코드 숨김으로 추적 하기 어려운 일관 되지 않은 xaml 이름 범위/전용 필드 명명 맞춤이 생성 될 수 있습니다.

### <a name="xname-and-xkey"></a>x:Name 및 x:Key

**x:Name** 은 [x:Key 특성](x-key-attribute.md)에 대 한 대체 역할을 하기 위해 [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 내의 요소에 특성으로 적용할 수 있습니다. ( **ResourceDictionary** 의 모든 요소에는 X:Key 또는 x:Name 특성이 있어야 함을 나타내는 규칙입니다.) [Storyboarded 애니메이션](../design/motion/storyboarded-animations.md)에 일반적입니다. 자세한 내용은 [ResourceDictionary 및 XAML 리소스 참조](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)섹션을 참조 하세요.