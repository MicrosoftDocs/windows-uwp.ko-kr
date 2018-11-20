---
author: jwmsft
description: 컨트롤 템플릿의 속성 값을 템플릿 기반 컨트롤에서 노출되는 몇몇 다른 속성 값에 연결합니다. TemplateBinding은 XAML의 ControlTemplate 정의 내에서만 사용할 수 있습니다.
title: TemplateBinding 태그 확장
ms.assetid: FDE71086-9D42-4287-89ED-8FBFCDF169DC
ms.author: jimwalk
ms.date: 10/29/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d4aaca880caf30b46cb1ed26d66700bb12d76404
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7289623"
---
# <a name="templatebinding-markup-extension"></a>{TemplateBinding} 태그 확장

컨트롤 템플릿의 속성 값을 템플릿 기반 컨트롤에서 노출되는 몇몇 다른 속성 값에 연결합니다. **TemplateBinding**은 XAML의 [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) 정의 내에서만 사용할 수 있습니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object propertyName="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-attribute-usage-for-setter-property-in-template-or-style"></a>XAML 특성 사용(템플릿 또는 스타일의 Setter 속성의 경우)

``` syntax
<Setter Property="propertyName" Value="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-values"></a>XAML 값

| 용어 | 설명 |
|------|-------------|
| propertyName | setter 구문에 설정할 속성의 이름입니다. 이 속성은 종속성 속성이어야 합니다. |
| sourceProperty | 템플릿을 기반으로 만들 형식에 존재하는 다른 종속성 속성의 이름입니다. |

## <a name="remarks"></a>설명

사용자 지정 컨트롤을 만드는 경우 또는 기존 컨트롤에 대한 컨트롤 템플릿을 바꾸는 경우 **TemplateBinding** 사용은 컨트롤 템플릿 정의 방법의 핵심 부분입니다. 자세한 내용은 [빠른 시작: 컨트롤 템플릿](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)을 참조하세요.

*propertyName* 및 *targetProperty*에서 동일한 속성 이름을 사용하는 것은 일반적입니다. 이 경우 컨트롤은 자체적으로 속성을 정의하고 정의한 속성을 해당 구성 요소 일부 중 하나인 직관적인 이름을 사용하는 기존 속성에 전달할 수 있습니다. 예를 들어 컨트롤 합성으로 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)을 통합하는 컨트롤을 사용하여 컨트롤의 고유 **Text** 속성을 표시하면 이 XAML을 컨트롤 템플릿의 일부로 포함할 수 있습니다. `<TextBlock Text="{TemplateBinding Text}" .... />`

원본 속성 및 대상 속성의 값으로 사용된 형식이 일치해야 합니다. **TemplateBinding**을 사용 중인 경우에는 변환기를 도입하는 기회가 없습니다. 값이 일치하지 않으면 XAML을 구문 분석할 때 오류가 발생합니다. 변환기가 필요한 경우 템플릿 바인딩을 위한 자세한 구문을 사용할 수 있습니다(예: ). `{Binding RelativeSource={RelativeSource TemplatedParent}, Converter="..." ...}`

XAML의 [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) 정의 밖에서 **TemplateBinding**을 사용하려고 하면 파서 오류가 발생합니다.

템플릿 기반 상위 값이 또 하나의 바인딩으로서 지연되는 경우 **TemplateBinding**을 사용할 수 있습니다. **TemplateBinding**에 대한 평가는 필수 런타임 바인딩이 값을 가질 때까지 대기할 수 있습니다.

**TemplateBinding**은 항상 단방향 바인딩입니다. 관련된 두 속성 모두가 종속성 속성이어야 합니다.

**TemplateBinding**은 태그 확장입니다. 태그 확장은 특정 값을 리터럴 값 또는 처리기 이름이 아닌 다른 값이 되도록 이스케이프해야 하는 요구 사항이 있는 경우 구현되며, 이러한 요구 사항은 특정 형식 또는 속성에 형식 변환기를 배치하는 것보다 더 포괄적입니다. XAML의 모든 태그 확장은 특성 구문에 "{" 및 "}" 문자를 사용하며, 여기서 특성 구문은 XAML 프로세서가 태그 확장이 특성을 처리해야 함을 인식하는 데 사용하는 규칙입니다.

**참고**Windows 런타임 XAML 프로세서 구현에는 **TemplateBinding**에 대 한 지원 클래스 표현이 없습니다. **TemplateBinding**은 XAML 태그에서만 사용됩니다. 코드에서 이 동작을 재현하는 간단한 방법은 없습니다.

### <a name="xbind-in-controltemplate"></a>ControlTemplate에서 X:bind

> [!NOTE]
> Windows 10, 버전 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk))는 ControlTemplate에서 X:bind를 사용 하려면 이상. 대상 버전에 대한 자세한 내용은 [버전 적응 코드](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)를 참조하세요.

Windows 10, 버전 1809부터 **X:bind** 태그 확장을 사용할 수 **TemplateBinding** [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391)에서 사용 되는 곳입니다. 

[TargetType](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate.targettype) 속성이 필요 (옵션 아님)에 [ControlTemplate](https://msdn.microsoft.com/library/windows/apps/br209391) **X:bind**를 사용 하는 경우.

**X:bind** 지원 [ControlTemplate](https://msdn.microsoft.com/library/windows/apps/br209391)에서 원활 하 게으로 양방향 바인딩을으로 두 [함수 바인딩](../data-binding/function-bindings.md) 을 사용할 수 있습니다.

이 예제에서는 **TextBlock.Text** 속성 **Button.Content.ToString**으로 계산 됩니다. ControlTemplate에서 TargetType 데이터 소스 역할을 하 고 부모 TemplateBinding와 동일한 결과 수행 합니다.

```xaml
<ControlTemplate TargetType="Button">
    <Grid>
        <TextBlock Text="{x:Bind Content, Mode=OneWay}"/>
    </Grid>
</ControlTemplate>
```

## <a name="related-topics"></a>관련 항목

* [빠른 시작: 컨트롤 템플릿](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)
* [데이터 바인딩 심층 분석](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391)
* [XAML 개요](xaml-overview.md)
* [종속성 속성 개요](dependency-properties-overview.md)
 

