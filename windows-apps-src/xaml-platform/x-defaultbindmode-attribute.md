---
description: XAML 태그에서 x:DefaultBindMode 특성을 사용 하 여 OneTime 이외의 x:Bind에 대 한 기본 모드를 지정 하는 방법을 알아봅니다.
title: xDefaultBindMode 특성
ms.date: 02/08/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 817b89dc8f3ab7952cdbfc53489eb1afb12024f2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166877"
---
# <a name="xdefaultbindmode-attribute"></a>x:DefaultBindMode 특성

XAML 태그에서 x:Bind. 기본 모드를 지정 합니다.

**X:defaultbindmode** 는 Windows 10 버전 1607 (기념일 업데이트), SDK 버전 14393부터 사용할 수 있습니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object x:DefaultBindMode="OneTime \| OneWay \| TwoWay" .../>
```

## <a name="remarks"></a>설명

[X:bind](x-bind-markup-extension.md) 의 기본 모드는 **OneTime**입니다. **OneWay**를 사용하면 연결 및 변경 감지 처리 시 더 많은 코드가 생성되기 때문에 성능을 이유로 기본 모드로 선택되었습니다. **X:defaultbindmode** 를 사용 하 여 태그 트리의 특정 세그먼트에 대 한 x:bind의 기본 모드를 변경할 수 있습니다. 지정 된 모드는 해당 요소 및 해당 자식에 대 한 모든 x:Bind 식에 적용 되며, 명시적으로 모드를 바인딩의 일부로 지정 하지 않습니다.

## <a name="related-topics"></a>관련 항목

* [x:Bind 태그 확장](x-bind-markup-extension.md)
