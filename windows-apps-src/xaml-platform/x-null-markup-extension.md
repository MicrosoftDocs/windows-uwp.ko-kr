---
description: XAML 태그에는 속성의 null 값을 지정합니다.
title: xNull 태그 확장
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0589249c65d301e3e74b305b92842fe4d3f61929
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372282"
---
# <a name="xnull-markup-extension"></a>{x:Null} 태그 확장


XAML 태그에는 속성의 **null** 값을 지정합니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object property="{x:Null}" .../>
```

## <a name="remarks"></a>설명

**null**은 C# 및 C++의 null 참조를 위한 키워드입니다. null 참조를 위한 Microsoft Visual Basic 키워드는 **Nothing**입니다.

초기 기본값은 종속성 속성 간에 다를 수 있으며 **null**이 아닐 수도 있습니다. 또한 많은 종속성 속성이 내부 구현 때문에 태그나 코드를 통해 **null**을 값으로 받을 수 없습니다. 이러한 경우 **{x:Null}** 로 XAML 특성 값을 설정하면 파서 예외가 발생할 수 있습니다.

일부 Windows 런타임 형식은 nullable 형식입니다. nullable 형식에 기본값으로 **null**이 아직 포함되어 있지 않으면 XAML에서 **{x:Null}** 을 사용하여 **null** 값으로 설정할 수 있습니다. 시각적 개체를 사용 하는 경우 C++ 구성 요소 확장 (C++/CX), nullable 형식으로 표시 됩니다 [ **platform:: ibox<T>** ](https://docs.microsoft.com/cpp/cppcx/platform-ibox-interface). Microsoft .NET 언어를 사용하는 경우 nullable 형식은 [**Nullable<T>** ](https://docs.microsoft.com/dotnet/api/system.nullable-1?redirectedfrom=MSDN)로 표현됩니다.

## <a name="related-topics"></a>관련 항목

* [**Nullable<T>** ](https://docs.microsoft.com/dotnet/api/system.nullable-1?redirectedfrom=MSDN)
* [**IReference<T>** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.IReference_T_)
 

