---
description: 리소스로 생성 되 고 참조 되며 ResourceDictionary 내에 존재 하는 요소를 고유 하 게 식별 합니다.
title: xKey 특성
ms.assetid: 141FC5AF-80EE-4401-8A1B-17CB22C2277A
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 45cdc3bcf766cd110498e357052da150ea96fa21
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161697"
---
# <a name="xkey-attribute"></a>x:Key 특성


리소스로 생성 되 고 참조 되며 [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)내에 존재 하는 요소를 고유 하 게 식별 합니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<ResourceDictionary>
  <object x:Key="stringKeyValue".../>
</ResourceDictionary>
```

## <a name="xaml-attribute-usage-implicit-resourcedictionary"></a>XAML 특성 사용 (암시적 **ResourceDictionary**)

``` syntax
<object.Resources>
  <object x:Key="stringKeyValue".../>
</object.Resources>
```

## <a name="xaml-values"></a>XAML 값

| 용어 | Description |
|------|-------------|
| 개체 | 공유 가능한 개체입니다. [ResourceDictionary 및 XAML 리소스 참조를](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)참조 하세요. |
| stringKeyValue | _XamlName_> 문법을 준수 해야 하는 키로 사용 되는 true 문자열입니다. 아래의 "XamlName 문법"을 참조 하세요. | 

##  <a name="xamlname-grammar"></a>XamlName 문법

다음은 UWP (유니버설 Windows 플랫폼) XAML 구현에서 키로 사용 되는 문자열에 대 한 규범 문법입니다.

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
-   이름은 숫자로 시작할 수 없습니다.

## <a name="remarks"></a>설명

[**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 의 자식 요소에는 일반적으로 해당 사전 내에서 고유한 키 값을 지정 하는 **x:Key** 특성이 포함 됩니다. 키 고유성은 XAML 프로세서에서 로드 시 적용 됩니다. 고유 하지 않은 **x:Key** 값은 XAML 구문 분석 예외를 발생 합니다. [{StaticResource} 태그 확장](staticresource-markup-extension.md)에서 요청 하는 경우 확인 되지 않은 키로 인해 XAML 구문 분석 예외도 발생 합니다.

**x:Key** 와 [x:Name](x-name-attribute.md) 의 개념은 동일 하지 않습니다. **x:Key** 는 리소스 사전 에서만 사용 됩니다. x:Name은 모든 XAML 영역에 사용 됩니다. 키 값을 사용 하는 [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) 호출은 키가 아닌 리소스를 검색 하지 않습니다. 리소스 사전에 정의 된 개체에는 **x:Key**, **x:Name** 또는 both가 있을 수 있습니다. 키와 이름은 일치 하지 않아도 됩니다.

표시 된 암시적 구문에서 [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 개체는 XAML 프로세서가 새 개체를 생성 하 여 [**리소스**](/uwp/api/windows.ui.xaml.frameworkelement.resources) 컬렉션을 채우는 방법에서 암시적입니다.

**X:Key** 를 지정 하는 것과 동일한 코드는 기본 [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)와 키를 사용 하는 작업입니다. 예를 들어 리소스에 대 한 태그에서 적용 된 **x:Key** 는 **ResourceDictionary**에 리소스를 추가할 때 **Insert** 의 *Key* 매개 변수 값과 동일 합니다.

리소스 사전의 항목은 [**Style**](/uwp/api/Windows.UI.Xaml.Style) 또는 [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)을 대상으로 하는 경우 **x:Key**에 대한 값을 생략할 수 있습니다. 각각의 경우 리소스 항목의 암시적인 키는 문자열로 해석된 **TargetType** 값입니다. 자세한 내용은 [빠른 시작: 컨트롤 스타일](/previous-versions/windows/apps/hh465498(v=win.10)) 지정 및 [ResourceDictionary 및 XAML 리소스 참조](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)를 참조 하세요.