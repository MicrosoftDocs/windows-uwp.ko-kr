---
author: jwmsft
description: 리소스로 만들어지고 참조되며 ResourceDictionary 내에 있는 요소를 고유하게 식별합니다.
title: xKey 특성
ms.assetid: 141FC5AF-80EE-4401-8A1B-17CB22C2277A
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8d48ccb93a411e92b57059192de38366f27353a3
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5979360"
---
# <a name="xkey-attribute"></a>x:Key 특성


리소스로 만들어지고 참조되며 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 내에 있는 요소를 고유하게 식별합니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<ResourceDictionary>
  <object x:Key="stringKeyValue".../>
</ResourceDictionary>
```

## <a name="xaml-attribute-usage-implicit-resourcedictionary"></a>XAML 특성 사용(암시적 **ResourceDictionary**)

``` syntax
<object.Resources>
  <object x:Key="stringKeyValue".../>
</object.Resources>
```

## <a name="xaml-values"></a>XAML 값

| 용어 | 설명 |
|------|-------------|
| object | 공유 가능한 모든 개체입니다. [ResourceDictionary 및 XAML 리소스 참조](https://msdn.microsoft.com/library/windows/apps/mt187273)를 참조하세요. |
| stringKeyValue | 키로 사용되는 실제 문자열이며 _XamlName_&gt; 문법을 따라야 합니다. 아래 "XamlName 문법"을 참조하세요. | 

##  <a name="xamlname-grammar"></a>XamlName 문법

다음은 UWP(유니버설 Windows 플랫폼) XAML 구현에서 키로 사용되는 문자열에 대한 규범 문법입니다.

``` syntax
XamlName ::= NameStartChar (NameChar)*
NameStartChar ::= LetterCharacter | '_'
NameChar ::= NameStartChar | DecimalDigit
LetterCharacter ::= ('a'-'z') | ('A'-'Z')
DecimalDigit ::= '0'-'9'
CombiningCharacter::= none
```

-   문자는 낮은 ASCII 범위에 있는 알파벳 대/소문자, 숫자 및 밑줄(\_) 문자로 제한됩니다.
-   유니코드 문자 범위는 지원되지 않습니다.
-   이름은 숫자로 시작될 수 없습니다.

## <a name="remarks"></a>설명

일반적으로 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)의 자식 요소는 해당 사전 내에서 고유한 키 값을 지정하는 **x:Key** 특성을 포함합니다. 키 고유성은 요소가 로드될 때 XAML 프로세서에 의해 적용됩니다. **x:Key** 값이 고유하지 않으면 XAML 구문 분석 예외가 발생합니다. [{StaticResource} 태그 확장](staticresource-markup-extension.md)에서 요청하는 경우 확인되지 않은 키가 있어도 XAML 구문 분석 예외가 발생합니다.

**x:Key**와 [x:Name](x-name-attribute.md)은 동일한 개념이 아닙니다. **x:Key**는 리소스 사전에서만 사용되고, x:Name은 XAML의 모든 영역에서 사용됩니다. 키 값을 사용하여 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)을 호출하는 경우 키가 지정된 리소스는 검색되지 않습니다. 리소스 사전에 정의된 개체에는 **x:Key**, **x:Name** 또는 둘 다 있습니다. 키와 이름이 일치할 필요는 없습니다.

표시된 암시적 구문에서 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 개체는 XAML 프로세서가 새 개체를 생성하여 [**Resources**](https://msdn.microsoft.com/library/windows/apps/br208740) 컬렉션을 채우는 방법을 암시적으로 지정합니다.

**x:Key** 지정에 해당하는 코드는 기본 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)와 함께 키를 사용하는 모든 작업입니다. 예를 들어 리소스의 태그에 적용된 **x:Key**는 리소스를 **ResourceDictionary**에 추가할 때 지정하는 **Insert**의 *key* 매개 변수 값과 동일합니다.

리소스 사전의 항목은 [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) 또는 [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391)을 대상으로 하는 경우 **x:Key**에 대한 값을 생략할 수 있습니다. 각각의 경우 리소스 항목의 암시적인 키는 문자열로 해석된 **TargetType** 값입니다. 자세한 내용은 [빠른 시작: 컨트롤 스타일 지정](https://msdn.microsoft.com/library/windows/apps/hh465498) 및 [ResourceDictionary 및 XAML 리소스 참조](https://msdn.microsoft.com/library/windows/apps/mt187273)를 확인하세요.

