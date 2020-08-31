---
description: 런타임 개체 그래프의 상대 관계를 기준으로 바인딩의 소스를 지정 하는 방법을 제공 합니다.
title: RelativeSource 태그 확장
ms.assetid: B87DEF36-BE1F-4C16-B32E-7A896BD09272
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 383e7b0576b5462879f903b684c332b35ee7d756
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155027"
---
# <a name="relativesource-markup-extension"></a>{RelativeSource} 태그 확장


런타임 개체 그래프의 상대 관계를 기준으로 바인딩의 소스를 지정 하는 방법을 제공 합니다.

## <a name="xaml-attribute-usage-self-mode"></a>XAML 특성 사용 (셀프 모드)

``` syntax
<Binding RelativeSource="{RelativeSource Self}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource Self} ...}" .../>
```

## <a name="xaml-attribute-usage-templatedparent-mode"></a>XAML 특성 사용 (TemplatedParent 모드)

``` syntax
<Binding RelativeSource="{RelativeSource TemplatedParent}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource TemplatedParent} ...}" .../>
```

## <a name="xaml-values"></a>XAML 값

| 용어 | Description |
|------|-------------|
| {RelativeSource Self} | <strong>자체</strong>의 [<strong>모드</strong>](/uwp/api/windows.ui.xaml.data.relativesource.mode) 값을 생성 합니다. 대상 요소는이 바인딩의 소스로 사용 되어야 합니다. 이는 요소의 한 속성을 동일한 요소의 다른 속성에 바인딩하는 데 유용 합니다. |
| {RelativeSource TemplatedParent} | 이 바인딩의 소스로 적용 되는 [<strong>ControlTemplate</strong>](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 을 생성 합니다. 이는 템플릿 수준에서 바인딩에 런타임 정보를 적용 하는 데 유용 합니다. | 

## <a name="remarks"></a>설명

바인딩은 바인딩 **개체 요소의** 특성 또는 [{binding} 태그 확장](binding-markup-extension.md)내의 구성 요소로 [**RelativeSource**](/uwp/api/windows.ui.xaml.data.binding.relativesource) 를 [**설정할 수 있습니다**](/uwp/api/Windows.UI.Xaml.Data.Binding) . 이 때문에 두 가지 XAML 구문이 표시 됩니다.

**RelativeSource** 은 [{Binding} 태그 확장과](binding-markup-extension.md)유사 합니다.  자체 인스턴스를 반환할 수 있는 태그 확장 이며 본질적으로 생성자에 인수를 전달 하는 문자열 기반 생성을 지원 합니다. 이 경우 전달 되는 인수는 [**모드**](/uwp/api/windows.ui.xaml.data.relativesource.mode) 값입니다.

**자체** 모드는 요소의 한 속성을 동일한 요소의 다른 속성에 바인딩하는 데 유용 하며 [**ElementName**](/uwp/api/windows.ui.xaml.data.binding.elementname) 바인딩의 변형 이지만 명명을 요구 하지 않으며 요소를 직접 참조할 수 있습니다. 요소의 속성 하나를 같은 요소의 다른 속성에 바인딩하는 경우 속성은 동일한 속성 형식을 사용 해야 합니다. 그렇지 않으면 바인딩에 [**변환기**](/uwp/api/windows.ui.xaml.data.binding.converter) 를 사용 하 여 값을 변환 해야 합니다. 예를 들어, 변환을 수행 하지 않고 [**너비**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 의 원본으로 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 를 사용할 수 있지만, [**표시**](/uwp/api/Windows.UI.Xaml.Visibility)를 위해 [**IsEnabled**](/uwp/api/windows.ui.xaml.controls.control.isenabled) 를 소스로 사용 하는 변환기가 필요 합니다.

예를 들면 다음과 같습니다. 이 [**사각형**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 은 [{Binding} 태그 확장](binding-markup-extension.md) 을 사용 하 여 해당 [**높이**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 와 [**너비가**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 항상 같으며 사각형으로 렌더링 합니다. 높이도 고정 값으로 설정 됩니다. 이 **사각형** 의 경우 기본 [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 는 **null** **이**아닙니다. 따라서 데이터 컨텍스트 원본을 개체 자체로 설정 하 고 다른 속성에 대 한 바인딩을 사용 하도록 설정 하려면 `RelativeSource={RelativeSource Self}` {binding} 태그 확장 사용에서 인수를 사용 합니다.

```XML
<Rectangle
  Fill="Orange" Width="200"
  Height="{Binding RelativeSource={RelativeSource Self}, Path=Width}"
/>
```

의 또 다른 용도는 `RelativeSource={RelativeSource Self}` 개체의 [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 를 자신에 게 설정 하는 방법입니다.  예를 들어, 다음과 같은 자체 데이터 바인딩에 대해 준비 된 뷰 모델을 이미 제공 하는 사용자 지정 속성으로 [**페이지**](/uwp/api/Windows.UI.Xaml.Controls.Page) 클래스가 확장 된 일부 SDK 예제에서이 기술을 볼 수 있습니다. `<common:LayoutAwarePage ... DataContext="{Binding DefaultViewModel, RelativeSource={RelativeSource Self}}">`

**참고**    **RelativeSource** 에 대 한 xaml 사용에는 바인딩 식의 일부로 Xaml의 [**RelativeSource**](/uwp/api/windows.ui.xaml.data.binding.relativesource) 에 대 한 값을 설정 하는 데 필요한 사용만 표시 됩니다. 이론적으로는 값이 [**RelativeSource**](/uwp/api/Windows.UI.Xaml.Data.RelativeSource)속성을 설정 하는 경우 다른 용도를 사용할 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [XAML 개요](xaml-overview.md)
* [데이터 바인딩 심층 분석](../data-binding/data-binding-in-depth.md)
* [{Binding} 태그 확장](binding-markup-extension.md)
* [**바인딩되**](/uwp/api/Windows.UI.Xaml.Data.Binding)
* [**RelativeSource**](/uwp/api/Windows.UI.Xaml.Data.RelativeSource)