---
author: jwmsft
description: "개체 참조의 필드가 private 기본 동작 대신 public 액세스로 정의되도록 XAML 컴파일 동작을 수정합니다."
title: "xFieldModifier 특성"
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
translationtype: Human Translation
ms.sourcegitcommit: 3144758352b99f8c145a3c7be8a6c43d6a002104
ms.openlocfilehash: f812c9498d5519aac8ab750f0c55423966a63464

---

# x:FieldModifier 특성

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

개체 참조의 필드가 **private** 기본 동작 대신 **public** 액세스로 정의되도록 XAML 컴파일 동작을 수정합니다.

## XAML 특성 사용

``` syntax
<object x:FieldModifier="public".../>
```

## 종속성

동일한 요소에 대해 [x:Name 특성](x-name-attribute.md)도 제공해야 합니다.

## 설명

**x:FieldModifier** 특성의 값은 프로그래밍 언어에 따라 다릅니다. 유효한 값은 **private**, **public**, **protected**, **internal** 또는 **friend**입니다. C#, Microsoft Visual Basic 또는 Visual C++ 구성 요소 확장(C++/CX)의 경우 문자열 값 "public" 또는 "Public"을 지정할 수 있습니다. 파서가 이 특성 값에 대/소문자를 적용하지 않습니다.

**Private** 액세스가 기본값입니다.

**x:FieldModifier**는 [x:Name 특성](x-name-attribute.md)이 있는 요소에만 해당됩니다. 해당 이름은 public이 된 필드를 참조하는 데 사용되기 때문입니다.

**참고** Windows 런타임 XAML은 **x:ClassModifier** 또는 **x:Subclass**를 지원하지 않습니다.




<!--HONumber=Aug16_HO3-->


