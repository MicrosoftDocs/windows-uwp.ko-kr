---
author: jwmsft
description: 태그 요소의 고유 식별자를 제공합니다. UWP(유니버설 Windows 플랫폼) XAML의 경우 이 고유 식별자는 .resw 리소스 파일의 리소스 사용과 같은 XAML 지역화 프로세스 및 도구에서 사용됩니다.
title: xUid 지시어
ms.assetid: 9FD6B62E-D345-44C6-B739-17ED1A187D69
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4bec4bd5d35fc2bb3013b37c1386520a769ddeb6
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5571038"
---
# <a name="xuid-directive"></a>x:Uid 지시어


태그 요소의 고유 식별자를 제공합니다. UWP(유니버설 Windows 플랫폼) XAML의 경우 이 고유 식별자는 .resw 리소스 파일의 리소스 사용과 같은 XAML 지역화 프로세스 및 도구에서 사용됩니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object x:Uid="stringID".../>
```

## <a name="xaml-values"></a>XAML 값

| 용어 | 설명 |
|------|-------------|
| stringID | 앱에서 XAML 요소를 고유하게 식별하는 문자열로, 리소스 파일에서 리소스 경로의 일부가 됩니다. 설명을 참조하세요.| 

## <a name="remarks"></a>설명

**x:Uid**를 사용하여 XAML에서 개체 요소를 식별합니다. 일반적으로 이 개체 요소는 컨트롤 클래스 또는 UI에 표시되는 다른 요소의 인스턴스입니다. **x:Uid**에서 사용하는 문자열과 리소스 파일에서 사용하는 문자열 간의 관계에서 리소스 파일 문자열은 **x:Uid** 뒤에 점(.)과 지역화되는 요소의 특정 속성 이름이 추가된 것입니다. 다음 예를 참조하세요.

``` syntax
<Button x:Uid="GoButton" Content="Go"/>
```

표시 텍스트 **Go**를 바꿀 콘텐츠를 지정하려면 리소스 파일에서 가져올 새 리소스를 지정해야 합니다. 리소스 파일에 "GoButton.Content"라는 리소스 항목이 있어야 합니다. 이 경우 [**Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content)는 [**Button**](/uwp/api/windows.ui.xaml.controls.button) 클래스에 상속된 특정 속성입니다. 이 단추의 다른 속성에 대해 지역화된 값을 제공할 수도 있습니다. 예를 들어 "GoButton.FlowDirection"에 대해 리소스 기반 값을 제공할 수 있습니다. **x:Uid** 및 리소스 파일을 함께 사용하는 방법에 대한 자세한 내용은 [UI와 앱 패키지 매니페스트에 문자열 지역화](../app-resources/localize-strings-ui-manifest.md)를 참조하세요.

**x:Uid** 값으로 사용할 수 있는 문자열의 유효성은 실제로 리소스 파일과 리소스 경로에서 식별자로 적합한 문자열이 제어합니다.

**x:Uid**는 명시된 XAML 지역화 시나리오로 인해 지역화에 사용되는 식별자가 **x:Name**의 프로그래밍 모델 암시에 대한 종속성이 없도록 **x:Name**에서 분리합니다. 또한 **x:Name**은 XAML 이름 범위 개념이 제어하고 **x:Uid**의 고유성은 PRI(패키지 리소스 인덱스) 시스템이 제어합니다. 자세한 내용은 [리소스 관리 시스템](../app-resources/resource-management-system.md)을 참조하세요.

UWP XAML은 **x:Uid** 고유성에 있어서 이전에 활용된 XAML 기술하고는 약간 다른 규칙을 가지고 있습니다. UWP XAML의 경우 이 지시문은 같은 **x:Uid** ID 값을 여러 XAML 요소에서 지시문으로 존재하도록 합니다. 하지만 이러한 요소는 각각 리소스 파일에서 리소스를 확인할 때 동일한 해결 논리를 공유해야 합니다. 또한 프로젝트의 모든 XAML 파일은 **x:Uid** 해결을 목적으로 단일 리소스 범위를 공유합니다. 개별 XAML 파일로 정렬하는 **x:Uid** 범위 개념은 없습니다.

PRI(패키지 리소스 인덱스) 시스템의 기본 제공 기능이 아니라 리소스 경로를 사용하는 경우도 있습니다. **x:Uid** 값으로 사용된 문자열은 모두 ms-resource:///Resources/로 시작하는 리소스 경로를 정의하며 **x:Uid** 문자열을 포함합니다. 경로는 리소스 파일에 지정되거나 달리 대상으로 지정된 속성 이름으로 완료됩니다.

Windows 런타임 XAML에서 허용되지 않는 **x:Uid**를 속성 요소에 사용하지 마세요.

