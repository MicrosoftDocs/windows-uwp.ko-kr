---
author: tbd
description: "XAML 태그에서 x:Bind에 대한 기본 모드 지정"
title: "xDefaultBindMode 태그 확장"
ms.assetid: 
ms.author: 
ms.date: 
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 0fa037b4c59566cb1b9bacd4d2e36520a86c508d
ms.sourcegitcommit: ba0d20f6fad75ce98c25ceead78aab6661250571
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/24/2017
---
# <a name="xdefaultbindmode-markup-extension"></a>{x:DefaultBindMode} 태그 확장

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows8.x 문서는 [아카이브](http://go.microsoft.com/fwlink/p/?linkid=619132)를 참조하세요. \]

XAML 태그에서 x:Bind에 대한 기본 모드를 지정합니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object x:DefaultBindMode="OneTime \| OneWay \| TwoWay" .../>
```

## <a name="remarks"></a>설명

x:Bind에는 기본 모드인 OneTime이 있으며, OneTime을 사용하면 연결 및 변경 감지 처리 시 더 많은 코드가 생성되기 때문에 기본 모드로 선택되었습니다. **x:DefaultBindMode**를 사용하여 특정 부분의 태그 트리에서 x:Bind의 기본 모드를 변경할 수 있습니다. 선택된 모드는 바인딩의 일부로 모드를 명시적으로 지정하지 않은 해당 요소와 하위 요소의 모든 x:Bind 식에 적용됩니다.

## <a name="related-topics"></a>관련 항목

* [**x:Bind**](https://docs.microsoft.com/en-us/windows/uwp/xaml-platform/x-bind-markup-extension)
