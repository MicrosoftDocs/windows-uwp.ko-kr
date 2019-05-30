---
description: 코드 숨김 또는 일반 코드에서 인스턴스화된 개체에 액세스하기 위해 개체 요소를 고유하게 식별합니다.
title: xName 특성
ms.assetid: 4FF1F3ED-903A-4305-B2BD-DCD29E0C9E6D
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 187a937c82c83f4653e58e7699a21051d03b09e7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372298"
---
# <a name="xname-attribute"></a>x:Name 특성


코드 숨김 또는 일반 코드에서 인스턴스화된 개체에 액세스하기 위해 개체 요소를 고유하게 식별합니다. 백업 프로그래밍 모델에 적용하는 경우 **x:Name**은 생성자에서 반환한 개체 참조를 보유하는 변수와 같은 것으로 간주됩니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object x:Name="XAMLNameValue".../>
```

## <a name="xaml-values"></a>XAML 값

| 용어 | 설명 |
|------|-------------|
| XAMLNameValue | XamlName 문법의 제한을 따르는 문자열입니다. |

##  <a name="xamlname-grammar"></a>XamlName 문법

다음은 이 XAML 구현에서 키로 사용되는 문자열에 대한 규범 문법입니다.

``` syntax
XamlName ::= NameStartChar (NameChar)*
NameStartChar ::= LetterCharacter | '_'
NameChar ::= NameStartChar | DecimalDigit
LetterCharacter ::= ('a'-'z') | ('A'-'Z')
DecimalDigit ::= '0'-'9'
CombiningCharacter::= none
```

-   문자는 ASCII 하한값을 로마 알파벳 대문자 및 소문자, 숫자 및 밑줄을 좀 더 구체적으로 제한 (\_) 문자입니다.
-   유니코드 문자 범위는 지원되지 않습니다.
-   이름은 숫자로 시작될 수 없습니다. 일부 도구에서는 앞에 밑줄 (\_) 사용자의 초기 문자 또는 도구 경로도 숫자를 제공 하는 경우 문자열로 **X:name** 숫자를 포함 하는 다른 값을 기반으로 하는 값입니다.

## <a name="remarks"></a>설명

지정한 **x:Name**은 XAML이 처리될 때 기본 코드에서 만들어지는 필드의 이름으로 사용되며, 이 필드에는 개체에 대한 참조가 저장됩니다. 이 필드를 만드는 프로세스는 MSBuild 대상 단계에서 수행되며, 이 때 XAML 파일 및 해당 코드 숨김에 대한 partial 클래스의 결합도 처리됩니다. 이 동작이 XAML 언어로 지정될 필요는 없습니다. 이 동작은 **x:Name**을 자체 프로그래밍 및 응용 프로그램 모델에서 사용하기 위해 XAML용 UWP(유니버설 Windows 플랫폼) 프로그래밍에서 적용하는 특별한 구현입니다.

정의되는 각 **x:Name**은 XAML 이름 범위 내에서 고유해야 합니다. 일반적으로 XAML 이름 범위는 로드된 페이지의 루트 요소 수준에서 정의되며 해당 요소 아래의 모든 요소를 하나의 XAML 페이지에 포함합니다. 추가 XAML 이름 범위는 이 페이지에 정의된 모든 컨트롤 템플릿이나 데이터 템플릿에서 정의됩니다. 런타임에, 적용된 컨트롤 템플릿에서 만들어진 개체 트리의 루트에 대해, 그리고 [**XamlReader.Load**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.xamlreader.load) 호출에서 만들어진 개체 트리에 의해 다른 XAML 이름 범위가 만들어집니다. 자세한 내용은 [XAML 이름 범위](xaml-namescopes.md)를 참조하세요.

디자인 도구는 디자인 화면에 표시될 때 요소의 **x:Name** 값을 자동으로 생성하기도 합니다. 자동 생성 스키마는 사용 중인 디자인 도구에 따라 달라지지만 일반적으로 요소를 지원하는 클래스 이름으로 시작하고 advancing 정수가 뒤따르는 문자열을 생성합니다. 예를 들어 첫 번째 [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) 요소를 디자인 도구에 표시하면 이 요소는 XAML에서 "Button1"의 **x:Name** 특성 값을 가진 요소가 됩니다.

**x:Name**은 XAML 속성 요소 구문 또는 [**SetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.setvalue)를 사용하는 코드에서 설정할 수 없습니다. **x:Name**은 요소의 XAML 특성 구문을 사용하여 설정해야만 합니다.

**참고**  맞게 C++/CX 앱, 지원 필드를 **X:name** XAML 파일 또는 페이지의 루트 요소에 대 한 참조를 만들어지지 않습니다. C++ 코드 숨김에서 루트 개체를 참조해야 하는 경우 다른 API나 트리 통과를 사용합니다. 예를 들어 알려진 명명된 자식 요소에 대해 [**FindName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.findname)을 호출한 다음 [**Parent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.parent)를 호출할 수 있습니다.

### <a name="xname-and-other-name-properties"></a>x:Name 및 기타 Name 속성

UWP XAML에서 사용된 일부 형식에는 **Name**이라는 속성도 있습니다. 예를 들어 [**FrameworkElement.Name**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.name) 및 [**TextElement.Name**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.textelement.name)입니다.

**Name**을 요소의 설정 가능한 속성으로 사용할 수 있는 경우 XAML에서 **Name** 및 **x:Name**을 서로 바꿔서 사용할 수 있지만, 동일한 요소에 두 특성을 모두 지정하면 오류가 발생합니다. **Name** 속성이 있지만 읽기 전용인 경우도 있습니다(예: [**VisualState.Name**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.visualstate.name)). 이 경우 XAML에서 항상 **x:Name**을 사용하여 해당 요소의 이름을 지정하며 읽기 전용 **Name**은 덜 일반적인 일부 코드 시나리오에 사용됩니다.

**참고** 일반적으로   [**FrameworkElement.Name**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.name)은 원래 **x:Name**으로 설정된 값을 변경하기 위한 방법으로 사용해서는 안 됩니다. 단, 이 일반 규칙에 대한 예외 시나리오도 있습니다. XAML 이름 범위의 생성 및 정의는 XAML 프로세서 작업입니다. 런타임에 **FrameworkElement.Name**을 수정하면 XAML 이름 범위/전용 필드의 이름 지정이 일치하지 않게 되므로 코드 숨김에서 추적하기 어렵습니다.

### <a name="xname-and-xkey"></a>x:Name 및 x:Key

**x:Name**은 [x:Key 특성](x-key-attribute.md)의 대체 항목으로 작용하도록 [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) 내의 요소에 특성으로 적용할 수 있습니다. (규칙 것는의 모든 요소를 **ResourceDictionary** X:key 또는 X:name 특성이 있어야 합니다.) 일반적으로 이것이 [애니메이션 스토리 보드 화](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations)합니다. 자세한 내용은 [ResourceDictionary 및 XAML 리소스 참조](https://docs.microsoft.com/windows/uwp/controls-and-patterns/resourcedictionary-and-xaml-resource-references) 섹션을 참조하세요.

