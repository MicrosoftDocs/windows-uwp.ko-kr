---
description: 개체 참조의 필드가 프라이빗 기본 동작 대신 public 액세스로 정의되도록 XAML 컴파일 동작을 수정합니다.
title: xFieldModifier 특성
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 751cda36fc58d0e6add9204327a74ec947c9fc53
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660918"
---
# <a name="xfieldmodifier-attribute"></a>x:FieldModifier 특성


개체 참조의 필드가 **private** 기본 동작 대신 **public** 액세스로 정의되도록 XAML 컴파일 동작을 수정합니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object x:FieldModifier="public".../>
```

## <a name="dependencies"></a>종속성

동일한 요소에 대해 [x:Name 특성](x-name-attribute.md)도 제공해야 합니다.

## <a name="remarks"></a>설명

**x:FieldModifier** 특성의 값은 프로그래밍 언어에 따라 다릅니다. 유효한 값은 **private**, **public**, **protected**, **internal** 또는 **friend**입니다. 에 대 한 C#, Microsoft Visual Basic 또는 Visual c + + 구성 요소 확장 (C + + /cli CX), 문자열을 제공할 수 있습니다 "public" 또는 "공용"; 값 파서는 특성 값이 대/소문자를 적용 하지 않습니다.

**Private** 액세스가 기본값입니다.

**x:FieldModifier**는 [x:Name 특성](x-name-attribute.md)이 있는 요소에만 해당됩니다. 해당 이름은 public이 된 필드를 참조하는 데 사용되기 때문입니다.

**참고**  Windows 런타임 XAML을 지원 하지 않습니다 **X:classmodifier** 하거나 **X:subclass**합니다.

