---
description: 이미 정의 된 리소스에 대 한 참조를 계산 하 여 모든 XAML 특성에 대 한 값을 제공 합니다. 리소스는 ResourceDictionary에서 정의 되 고 StaticResource usage는 ResourceDictionary에서 해당 리소스의 키를 참조 합니다.
title: StaticResource 태그 확장
ms.assetid: D50349B5-4588-4EBD-9458-75F629CCC395
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 32d09d4366d5ee05fe9581c4c8076811cfa0436f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155007"
---
# <a name="staticresource-markup-extension"></a>{StaticResource} 태그 확장


이미 정의 된 리소스에 대 한 참조를 계산 하 여 모든 XAML 특성에 대 한 값을 제공 합니다. 리소스는 [**resourcedictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)에서 정의 되 고 **StaticResource** usage는 **resourcedictionary**에서 해당 리소스의 키를 참조 합니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object property="{StaticResource key}" .../>
```

## <a name="xaml-values"></a>XAML 값

| 용어 | Description |
|------|-------------|
| key | 요청한 리소스의 키입니다. 이 키는 초기에 [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)에 의해 할당 됩니다. 리소스 키는 XamlName 문법에 정의 된 임의의 문자열일 수 있습니다. |

## <a name="remarks"></a>설명

**StaticResource** 는 xaml 리소스 사전의 다른 곳에서 정의 된 xaml 특성에 대 한 값을 가져오는 기술입니다. 값은 여러 속성 값에서 공유 하기 위한 것 이거나 XAML 리소스 사전이 XAML 패키징 또는 팩터링 기술로 사용 되기 때문에 리소스 사전에 배치 될 수 있습니다. XAML 패키징 기술의 예로는 컨트롤에 대 한 테마 사전이 있습니다. 또 다른 예는 리소스 대체에 사용 되는 병합 된 리소스 사전입니다.

**StaticResource** 는 요청 된 리소스에 대 한 키를 지정 하는 인수 하나를 사용 합니다. 리소스 키는 항상 Windows 런타임 XAML의 문자열입니다. 리소스 키를 처음 지정 하는 방법에 대 한 자세한 내용은 [x:Key 특성](x-key-attribute.md)을 참조 하세요.

**StaticResource** 리소스 사전의 항목을 확인 하는 규칙은이 항목에서 설명 하지 않습니다. 이는 참조와 리소스가 모두 템플릿에 있는지 여부, 병합 된 리소스 사전이 사용 되는지 여부 등에 따라 달라 집니다. 리소스를 정의 하 고 샘플 코드를 비롯 한 [**resourcedictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)를 적절히 사용 하는 방법에 대 한 자세한 내용은 [resourcedictionary 및 XAML 리소스 참조](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)를 참조 하세요.

**중요**    **StaticResource** 는 XAML 파일 내에서 어휘 적으로 정의 된 리소스에 대 한 전방 참조를 만들려고 해서는 안 됩니다. 이 작업을 수행 하는 것은 지원 되지 않습니다. 전방 참조가 실패 하지 않는 경우에도 그 중 하나를 수행 하려고 하면 성능이 저하 됩니다. 최상의 결과를 위해 전방 참조를 피할 수 있도록 리소스 사전의 컴퍼지션을 조정 합니다.

확인할 수 없는 키에 대 한 **StaticResource** 을 지정 하려고 하면 런타임에 XAML 구문 분석 예외가 throw 됩니다. 디자인 도구에서 경고나 오류를 제공할 수도 있습니다.

Windows 런타임 XAML 프로세서 구현에는 **StaticResource** 기능에 대 한 지원 클래스 표현이 없습니다. **StaticResource** 는 XAML 에서만 사용 됩니다. 가장 근접하게 코드에 상당하는 방법은 [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)의 컬렉션 API를 사용하는 것입니다(예: [**Contains**](/uwp/api/windows.ui.xaml.resourcedictionary.contains) 또는 [**TryGetValue**](/uwp/api/windows.ui.xaml.resourcedictionary.trygetvalue) 호출).

[{Themeresource} 태그 확장](themeresource-markup-extension.md) 은 다른 위치에 있는 명명 된 리소스를 참조 하는 비슷한 태그 확장입니다. 차이점은 {ThemeResource} 태그 확장은 활성 상태인 시스템 테마에 따라 다른 리소스를 반환 하는 기능을 제공 한다는 것입니다. 자세한 내용은 [{Themeresource} 태그 확장](themeresource-markup-extension.md)을 참조 하세요.

**StaticResource** 은 태그 확장입니다. 태그 확장은 특성 값을 리터럴 값 또는 처리기 이름이 아닌 다른 값이 되도록 이스케이프해야 하는 요구 사항이 있는 경우 일반적으로 구현되며 이러한 요구 사항은 특정 형식 또는 속성에 형식 변환기를 배치하는 것보다 더 포괄적입니다. XAML의 모든 태그 확장은 \{ 특성 구문에서 "" 및 "" 문자를 사용 합니다 \} .이는 xaml 프로세서에서 태그 확장이 특성을 처리 해야 함을 인식 하는 데 사용 되는 규칙입니다.

### <a name="an-example-staticresource-usage"></a>예제 {StaticResource} 사용

이 예제 XAML은 [xaml 데이터 바인딩 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind)에서 가져온 것입니다.

```xml
<StackPanel Margin="5">
    <!-- Add converter as a resource to reference it from a Binding. --> 
    <StackPanel.Resources>
        <local:S2Formatter x:Key="GradeConverter"/>
    </StackPanel.Resources>
    <TextBlock Style="{StaticResource BasicTextStyle}" Text="Percent grade:" Margin="5" />
    <Slider x:Name="sliderValueConverter" Minimum="1" Maximum="100" Value="70" Margin="5"/>
    <TextBlock Style="{StaticResource BasicTextStyle}" Text="Letter grade:" Margin="5"/>
    <TextBox x:Name="tbValueConverterDataBound"
      Text="{Binding ElementName=sliderValueConverter, Path=Value, Mode=OneWay,  
        Converter={StaticResource GradeConverter}}" Margin="5" Width="150"/> 
</StackPanel> 
```

이 특정 예제에서는 사용자 지정 클래스에서 지 원하는 개체를 만들어 [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)의 리소스로 만듭니다. 유효한 리소스가 되려면이 요소에는 `local:S2Formatter` **x:Key** 특성 값도 있어야 합니다. 특성의 값은 "GradeConverter"로 설정 됩니다.

그런 다음이 리소스는 XAML에서 약간 더 많이 요청 됩니다 (표시) `{StaticResource GradeConverter}` .

{StaticResource} 태그 확장 사용이 다른 태그 확장 [{Binding} 태그 확장](binding-markup-extension.md)의 속성을 설정 하는 방법에 유의 하세요. 여기에는 두 개의 중첩 된 태그 확장 용도가 있습니다. 먼저 먼저 계산 된 후에는 리소스를 가져와 값으로 사용할 수 있습니다. 이 동일한 예제는 {Binding} 태그 확장에도 표시 됩니다.

## <a name="design-time-tools-support-for-the-staticresource-markup-extension"></a>**{StaticResource}** 태그 확장에 대 한 디자인 타임 도구 지원

Microsoft Visual Studio 2013에는 XAML 페이지에서 **{StaticResource}** 태그 확장을 사용할 때 Microsoft IntelliSense 드롭다운에 가능한 키 값이 포함 될 수 있습니다. 예를 들어 "{StaticResource"를 입력 하는 즉시 현재 조회 범위의 모든 리소스 키가 IntelliSense 드롭다운에 표시 됩니다. 페이지 수준 ([**FrameworkElement. 리소스**](/uwp/api/windows.ui.xaml.frameworkelement.resources)) 및 앱 수준 ([**응용 프로그램**](/uwp/api/windows.ui.xaml.application.resources))에 있는 일반적인 리소스 외에도 [XAML 테마 리소스](../design/controls-and-patterns/xaml-theme-resources.md)및 프로젝트에서 사용 하는 모든 확장의 리소스도 볼 수 있습니다.

리소스 키가 **{StaticResource}** 사용의 일부로 존재 하는 경우, **정의로 이동** (F12) 기능은 해당 리소스를 확인 하 고 정의 된 사전을 보여줄 수 있습니다. 테마 리소스의 경우 디자인 타임에 대해 xaml로 이동 합니다.

## <a name="related-topics"></a>관련 항목

* [ResourceDictionary 및 XAML 리소스 참조](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)
* [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)
* [x:Key 특성](x-key-attribute.md)
* [{ThemeResource} 태그 확장](themeresource-markup-extension.md)