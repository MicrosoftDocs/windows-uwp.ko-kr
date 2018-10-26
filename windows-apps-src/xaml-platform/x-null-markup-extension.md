---
author: jwmsft
description: XAML 태그에는 속성의 null 값을 지정합니다.
title: xNull 태그 확장
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b6033a01ee811977b3a37f820217005fdbd80616
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5596891"
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

일부 Windows 런타임 형식은 nullable 형식입니다. nullable 형식에 기본값으로 **null**이 아직 포함되어 있지 않으면 XAML에서 **{x:Null}** 을 사용하여 **null** 값으로 설정할 수 있습니다. VisualC + + 구성 요소 확장을 사용 하는 경우 (C + + CX), nullable 형식으로 표현 됩니다 [**platform:: ibox<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/jj606120.aspx). Microsoft .NET 언어를 사용하는 경우 nullable 형식은 [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx)로 표현됩니다.

## <a name="related-topics"></a>관련 항목

* [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx)
* [**IReference<T>**](https://msdn.microsoft.com/library/windows/apps/br225864)
 

