---
description: XAML 태그에서 x:Null 태그 확장을 사용 하 여 속성에 Null 값을 지정 하는 방법에 대해 알아봅니다.
title: xNull 태그 확장
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 46256a5456583f9e434f76a2d06bc552c2cb0d3f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169037"
---
# <a name="xnull-markup-extension"></a>{x:Null} 태그 확장


XAML 태그에서 속성에 대해 **null** 값을 지정 합니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object property="{x:Null}" .../>
```

## <a name="remarks"></a>설명

**null** 은 c # 및 c + +에 대 한 null 참조 키워드입니다. Null 참조의 Microsoft Visual Basic 키워드는 **Nothing**입니다.

초기 기본값은 종속성 속성에 따라 다를 수 있으며 반드시 **null**일 수는 없습니다. 또한 많은 종속성 속성은 내부 구현으로 인해 태그 또는 코드를 통해 값으로 **null** 을 허용 하지 않습니다. 이러한 경우에는 XAML 특성 값을 **{X:null}** 로 설정 하면 파서 예외가 발생할 수 있습니다.

일부 Windows 런타임 형식은 nullable입니다. Nullable 형식에 기본적으로 **null** 이 없는 경우에는 XAML에서 **{x:null}** 을 사용 하 여 **null** 값으로 설정할 수 있습니다. Visual C++ 구성 요소 확장 (c + +/CX)을 사용 하는 경우 nullable 형식은 [**Platform:: <T> ibox**](/cpp/cppcx/platform-ibox-interface)로 표시 됩니다. Microsoft .NET 언어를 사용 하는 경우 nullable 형식은 [**nullable <T> **](/dotnet/api/system.nullable-1)로 표시 됩니다.

## <a name="related-topics"></a>관련 항목

* [**않는<T>**](/dotnet/api/system.nullable-1)
* [**IReference<T>**](/uwp/api/Windows.Foundation.IReference_T_)
 