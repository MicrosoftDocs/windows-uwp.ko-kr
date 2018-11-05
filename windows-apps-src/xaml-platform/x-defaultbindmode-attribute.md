---
author: jwmsft
description: XAML 태그에서 x:Bind에 대한 기본 모드를 지정합니다.
title: xDefaultBindMode 특성
ms.author: jimwalk
ms.date: 02/08/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2696cb46591757421795b15083ea7fdab54943c5
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6035439"
---
# <a name="xdefaultbindmode-attribute"></a>x:DefaultBindMode 특성

XAML 태그에서 x:Bind에 대한 기본 모드를 지정합니다.

**x:DefaultBindMode**는 Windows 10 버전 1607(1주년 업데이트), SDK 버전 14393부터 사용할 수 있습니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object x:DefaultBindMode="OneTime \| OneWay \| TwoWay" .../>
```

## <a name="remarks"></a>설명

[x:Bind](x-bind-markup-extension.md)이 **OneTime**의 기본 모드를 가집니다. **OneWay**를 사용하면 연결 및 변경 감지 처리 시 더 많은 코드가 생성되기 때문에 성능을 이유로 기본 모드로 선택되었습니다. **x:DefaultBindMode**를 사용하여 태그 트리의 특정 세그먼트에서 x:Bind의 기본 모드를 변경할 수 있습니다. 지정된 모드는 바인딩의 일부로 모드를 명시적으로 지정하지 않은 해당 요소와 하위 요소의 모든 x:Bind 식에 적용됩니다.

## <a name="related-topics"></a>관련 항목

* [x:Bind 태그 확장](x-bind-markup-extension.md)
