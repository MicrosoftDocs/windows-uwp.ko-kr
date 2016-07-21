---
author: jwmsft
description: "런타임 개체 그래프에 있는 상대적 관계의 측면에서 바인딩의 원본을 지정하는 수단을 제공합니다."
title: "RelativeSource 태그 확장"
ms.assetid: B87DEF36-BE1F-4C16-B32E-7A896BD09272
translationtype: Human Translation
ms.sourcegitcommit: ec4c9b87655425e82a1cb792d0acc6bee265e9d2
ms.openlocfilehash: 9f0bb49e701806f8635d93fa495cdab6486a4ea3

---

# {RelativeSource} 태그 확장

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

런타임 개체 그래프에 있는 상대적 관계의 측면에서 바인딩의 원본을 지정하는 수단을 제공합니다.

## XAML 특성 사용(Self 모드)

``` syntax
<Binding RelativeSource="{RelativeSource Self}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource Self} ...}" .../>
```

## XAML 특성 사용(TemplatedParent 모드)

``` syntax
<Binding RelativeSource="{RelativeSource TemplatedParent}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource TemplatedParent} ...}" .../>
```

## XAML 값

| 용어 | 설명 |
|------|-------------|
| {RelativeSource Self} | <strong>Self</strong>의 [<strong>Mode</strong>](https://msdn.microsoft.com/library/windows/apps/br209915) 값을 생성합니다. 대상 요소가 이 바인딩의 원본으로 사용되어야 합니다. 요소의 한 속성을 동일한 요소의 다른 속성에 바인딩하는 데 유용합니다. |
| {RelativeSource TemplatedParent} | 이 바인딩의 원본으로 적용되는 [<strong>ControlTemplate</strong>](https://msdn.microsoft.com/library/windows/apps/br209391)을 생성합니다. 템플릿 수준에서 런타임 정보를 바인딩에 적용하는 데 유용합니다. | 

## 설명

[**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)은 [**Binding.RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831)를 **Binding** 개체 요소의 속성이나 [{Binding} markup extension](binding-markup-extension.md) 내의 구성 요소로 설정할 수 있습니다. 이 때문에 두 가지 XAML 구문이 표시됩니다.

**RelativeSource**는 [{Binding} 태그 확장](binding-markup-extension.md)과 유사하며,  인스턴스를 반환하여 생성자에 인수를 전달하는 문자열 기반 생성을 지원할 수 있는 태그 확장입니다. 이 경우에 전달될 인수는 [**Mode**](https://msdn.microsoft.com/library/windows/apps/br209915) 값입니다.

**Self** 모드는 요소의 한 속성을 동일한 요소의 다른 속성에 바인딩하는 데 유용하며, [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) 바인딩에 대한 변형이지만 요소의 명명과 자체 참조가 필요하지 않습니다. 요소의 한 속성을 동일한 요소의 다른 속성에 바인딩하는 경우 속성이 동일한 속성 형식을 사용해야 하거나, 사용자가 바인딩에 대해 [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826)를 사용하여 값을 변환해야 합니다. 예를 들어 [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718)를 [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751)의 원본으로 사용하는 경우 변환하지 않아도 되지만 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/br209419)를 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br209006)의 원본으로 사용하려면 변환기가 필요합니다.

예를 들면 다음과 같습니다. 이 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371)은 해당 [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) 및 [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751)가 항상 같고 자체가 정사각형으로 렌더링되도록 [{Binding} 태그 확장](binding-markup-extension.md)을 사용합니다. 높이만 고정 값으로 설정됩니다. 이 **Rectangle**의 경우 기본 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713)는 **null**이며 **this**가 아닙니다. 따라서 데이터 컨텍스트 원본이 개체 자체가 되도록 설정하고 다른 속성에 대한 바인딩을 지원할 수 있도록 {Binding} 태그 확장 사용법에서 `RelativeSource={RelativeSource Self}` 인수를 사용합니다.

```XML
<Rectangle
  Fill="Orange" Width="200"
  Height="{Binding RelativeSource={RelativeSource Self}, Path=Width}"
/>
```

`RelativeSource={RelativeSource Self}`를 사용하여 개체의 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713)를 자체로 설정하는 방법도 있습니다.  예를 들어 이 방법은 [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) 클래스가 고유한 데이터 바인딩을 위해 즉시 사용 가능한 보기 모델을 이미 제공하고 있는 사용자 지정 속성을 사용하여 확장된 일부 SDK 예제에서 확인할 수 있습니다. 예는 다음과 같습니다. `<common:LayoutAwarePage ... DataContext="{Binding DefaultViewModel, RelativeSource={RelativeSource Self}}">`

**참고** **RelativeSource**의 XAML 사용은 의도한 사용(XAML에서 [**Binding.RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831)의 값을 바인딩 식의 일부로 설정함)만 보여 줍니다. 이론적으로, 값이 [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209913)인 속성을 설정하는 경우 다른 사용이 가능합니다.

## 관련 항목

* [XAML 개요](xaml-overview.md)
* [데이터 바인딩 심층 분석](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [{Binding} 태그 확장](binding-markup-extension.md)
* [**바인딩**](https://msdn.microsoft.com/library/windows/apps/br209820)
* [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209913)




<!--HONumber=Jun16_HO5-->


