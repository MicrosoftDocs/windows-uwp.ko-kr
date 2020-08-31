---
description: 컨트롤 템플릿의 속성 값을 템플릿 기반 컨트롤의 다른 노출 된 속성 값에 연결 합니다. TemplateBinding은 XAML의 ControlTemplate 정의 내에서만 사용할 수 있습니다.
title: TemplateBinding 태그 확장
ms.assetid: FDE71086-9D42-4287-89ED-8FBFCDF169DC
ms.date: 10/29/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3b368ca2f429d52674ba1cb3493012d54dc0848a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154927"
---
# <a name="templatebinding-markup-extension"></a>{TemplateBinding} 태그 확장

컨트롤 템플릿의 속성 값을 템플릿 기반 컨트롤의 다른 노출 된 속성 값에 연결 합니다. **TemplateBinding** 은 XAML의 [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 정의 내 에서만 사용할 수 있습니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object propertyName="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-attribute-usage-for-setter-property-in-template-or-style"></a>XAML 특성 사용 (템플릿 또는 스타일의 Setter 속성에 사용)

``` syntax
<Setter Property="propertyName" Value="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-values"></a>XAML 값

| 용어 | Description |
|------|-------------|
| propertyName | Setter 구문에 설정 되는 속성의 이름입니다. 이 속성은 종속성 속성 이어야 합니다. |
| sourceProperty | 템플릿 기반 형식에 있는 다른 종속성 속성의 이름입니다. |

## <a name="remarks"></a>설명

사용자 지정 컨트롤 작성자 이거나 기존 컨트롤에 대 한 컨트롤 템플릿을 바꾸는 경우 **TemplateBinding** 을 사용 하는 것은 컨트롤 템플릿을 정의 하는 방법의 기본적인 부분입니다. 자세한 내용은 [빠른 시작: 컨트롤 템플릿](/previous-versions/windows/apps/hh465374(v=win.10))을 참조 하세요.

동일한 속성 이름을 사용 하는 *propertyName* 및 *system.windows.media.animation.storyboard.targetproperty* 의 경우에는 매우 일반적입니다. 이 경우 컨트롤은 자체적으로 속성을 정의 하 고 속성을 해당 구성 요소 파트 중 하나의 기존 및 직관적으로 명명 된 속성으로 전달할 수 있습니다. 예를 들어, 컨트롤의 자체 **텍스트** 속성을 표시 하는 데 사용 되는 해당 합성에 [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 을 통합 하는 컨트롤은이 XAML을 컨트롤 템플릿의 일부로 포함할 수 있습니다.`<TextBlock Text="{TemplateBinding Text}" .... />`

원본 속성 및 대상 속성 값으로 사용 되는 형식은와 일치 해야 합니다. **TemplateBinding**을 사용 하는 경우 변환기를 도입할 기회가 없습니다. 값 일치에 실패 하면 XAML을 구문 분석할 때 오류가 발생 합니다. 변환기가 필요한 경우 다음과 같은 템플릿 바인딩에 대해 자세한 구문을 사용할 수 있습니다. `{Binding RelativeSource={RelativeSource TemplatedParent}, Converter="..." ...}`

XAML에서 [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 정의 외부의 **TemplateBinding** 을 사용 하려고 하면 파서 오류가 발생 합니다.

템플릿 기반 부모 값도 다른 바인딩으로 지연 되는 경우 **TemplateBinding** 을 사용할 수 있습니다. **TemplateBinding** 의 평가는 필요한 런타임 바인딩에 값이 있을 때까지 대기할 수 있습니다.

**TemplateBinding** 은 항상 단방향 바인딩입니다. 관련된 두 속성은 모두 종속성 속성이어야 합니다.

**TemplateBinding** 은 태그 확장입니다. 태그 확장은 특성 값을 리터럴 값 또는 처리기 이름이 아닌 다른 값이 되도록 이스케이프해야 하는 요구 사항이 있는 경우 일반적으로 구현되며 이러한 요구 사항은 특정 형식 또는 속성에 형식 변환기를 배치하는 것보다 더 포괄적입니다. XAML의 모든 태그 확장은 특성 구문에서 "{" 및 "}" 문자를 사용 합니다 .이는 XAML 프로세서가 태그 확장이 특성을 처리 해야 함을 인식 하는 데 사용 되는 규칙입니다.

**참고**    Windows 런타임 XAML 프로세서 구현에는 **TemplateBinding**에 대 한 지원 클래스 표현이 없습니다. **TemplateBinding** 은 XAML 태그에만 사용할 수 있습니다. 코드에서 동작을 재현 하는 간단한 방법은 없습니다.

### <a name="xbind-in-controltemplate"></a>ControlTemplate의 x:Bind

> [!NOTE]
> ControlTemplate에서 x:Bind를 사용 하려면 Windows 10, 버전 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 이상이 필요 합니다. 대상 버전에 대한 자세한 내용은 [버전 적응 코드](../debug-test-perf/version-adaptive-code.md)를 참조하세요.

Windows 10 버전 1809부터 [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)에서 **TemplateBinding** 을 사용 하는 모든 곳에서 **x:bind** 태그 확장을 사용할 수 있습니다. 

**X:bind**를 사용 하는 경우 [ControlTemplate](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 에 [TargetType](/uwp/api/windows.ui.xaml.controls.controltemplate.targettype) 속성이 필요 합니다 (옵션 아님).

**X:bind** 지원을 사용 하면 [ControlTemplate](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)의 양방향 바인딩과 함께 [함수 바인딩을](../data-binding/function-bindings.md) 사용할 수 있습니다.

이 예제에서 **TextBlock. Text** 속성은 **Button. Content. ToString**으로 평가 됩니다. ControlTemplate의 TargetType은 데이터 원본 역할을 하며 부모에 대 한 TemplateBinding과 동일한 결과를 생성 합니다.

```xaml
<ControlTemplate TargetType="Button">
    <Grid>
        <TextBlock Text="{x:Bind Content, Mode=OneWay}"/>
    </Grid>
</ControlTemplate>
```

## <a name="related-topics"></a>관련 항목

* [빠른 시작: 컨트롤 템플릿](/previous-versions/windows/apps/hh465374(v=win.10))
* [데이터 바인딩 심층 분석](../data-binding/data-binding-in-depth.md)
* [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)
* [XAML 개요](xaml-overview.md)
* [종속성 속성 개요](dependency-properties-overview.md)
 