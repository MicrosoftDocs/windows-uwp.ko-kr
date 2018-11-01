---
author: jwmsft
description: 이미 정의된 리소스에 대한 참조를 평가하여 모든 XAML 특성에 대한 값을 제공합니다. 리소스는 ResourceDictionary에 정의되며 StaticResource 사용은 ResourceDictionary의 리소스 키를 참조합니다.
title: StaticResource 태그 확장
ms.assetid: D50349B5-4588-4EBD-9458-75F629CCC395
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 83919cc46694279bc35e046c97acf27c64a196f5
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5875185"
---
# <a name="staticresource-markup-extension"></a>{StaticResource} 태그 확장


이미 정의된 리소스에 대한 참조를 평가하여 모든 XAML 특성에 대한 값을 제공합니다. 리소스는 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)에 정의되며 **StaticResource** 사용은 **ResourceDictionary**의 리소스 키를 참조합니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object property="{StaticResource key}" .../>
```

## <a name="xaml-values"></a>XAML 값

| 용어 | 설명 |
|------|-------------|
| 키 | 요청된 리소스에 대한 키입니다. 이 키는 처음에 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)에 의해 할당됩니다. 리소스 키는 XamlName 문법에 정의된 문자열일 수 있습니다. |

## <a name="remarks"></a>설명

**StaticResource**는 XAML 리소스 사전에 정의된 XAML 특성 값을 가져오기 위한 기술입니다. 값이 여러 속성 값에서 공유되거나 XAML 리소스 사전이 XAML 패키징 또는 팩터링 기술로 사용되어 값이 리소스 사전에 포함되었을 수도 있습니다. XAML 패키징 기술의 예로는 컨트롤의 테마 사전이 있습니다. 다른 예는 리소스 대체에 사용되는 병합된 리소스 사전입니다.

**StaticResource**는 요청된 리소스의 키를 지정하는 인수 하나를 사용합니다. Windows 런타임 XAML에서 리소스 키는 항상 문자열입니다. 초기에 리소스 키를 지정하는 방법에 대한 자세한 내용은 [x:Key 특성](x-key-attribute.md)을 참조하세요.

**StaticResource**가 리소스 사전의 항목으로 확인하는 규칙은 이 항목에서 설명하지 않습니다. 이 규칙은 참조 및 리소스가 모두 템플릿에 있는지, 병합된 리소스 사전이 사용되는지 등에 따라 다릅니다. 리소스 정의 방법과 샘플 코드를 포함한 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)의 적절한 사용 방법에 대한 자세한 내용은 [ResourceDictionary 및 XAML 리소스 참조](https://msdn.microsoft.com/library/windows/apps/mt187273)를 확인하세요.

**중요 한**  **StaticResource** 정의 된 리소스에 대 한 전방 참조를 만들려고 해서는 안 XAML 파일 내에서 구문적으로 더 합니다. 이러한 시도는 지원되지 않습니다. 전방 참조가 실패하지 않더라도 전방 참조를 만들려고 하면 성능이 저하됩니다. 최상의 결과를 얻으려면 전방 참조를 피할 수 있도록 리소스 사전의 컴퍼지션을 조정하세요.

확인할 수 없는 키에 **StaticResource**를 지정하려고 하면 런타임에 XAML 구문 분석 예외가 발생합니다. 디자인 도구에서 경고나 오류를 제공할 수도 있습니다.

Windows 런타임 XAML 프로세서 구현에는 **StaticResource** 기능을 위한 지원 클래스 표현이 없습니다. **StaticResource**는 XAML에서만 사용됩니다. 가장 근접하게 코드에 상당하는 방법은 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)의 컬렉션 API를 사용하는 것입니다(예: [**Contains**](https://msdn.microsoft.com/library/windows/apps/jj635925) 또는 [**TryGetValue**](https://msdn.microsoft.com/library/windows/apps/jj603139) 호출).

[{ThemeResource} 태그 확장](themeresource-markup-extension.md)은 다른 위치에 있는 명명된 리소스를 참조하는 비슷한 태그 확장입니다. 차이점은 {ThemeResource} 태그 확장에는 현재 시스템 테마에 따라 여러 다른 리소스를 반환하는 기능이 있다는 점입니다. 자세한 내용은 [{ThemeResource} 태그 확장](themeresource-markup-extension.md)을 참조하세요.

**StaticResource**은 태그 확장입니다. 태그 확장은 특정 값을 리터럴 값 또는 처리기 이름이 아닌 다른 값이 되도록 이스케이프해야 하는 요구 사항이 있는 경우 구현되며, 이러한 요구 사항은 특정 형식 또는 속성에 형식 변환기를 배치하는 것보다 더 포괄적입니다. XAML의 모든 태그 확장은 특성 구문에 "\{" 및 "\}" 문자를 사용하며, 여기서 특성 구문은 XAML 프로세서가 태그 확장이 특성을 처리해야 함을 인식하는 데 사용하는 규칙입니다.

### <a name="an-example-staticresource-usage"></a>{StaticResource} 사용 예제

이 예제 XAML은 [XAML 데이터 바인딩 샘플](http://go.microsoft.com/fwlink/p/?linkid=226854)에서 가져온 것입니다.

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

이 특정 예제에서는 사용자 지정 클래스가 지원하는 개체를 만들고 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)에서 이 개체를 리소스로 만듭니다. 유효한 리소스가 되려면 이 `local:S2Formatter` 요소에 **x:Key** 특성 값도 있어야 합니다. 특성 값은 "GradeConverter"로 설정되었습니다.

그런 다음 XAML에서 `{StaticResource GradeConverter}`가 표시되는 위치까지 리소스가 요청됩니다.

{StaticResource} 태그 확장 사용을 통해 다른 태그 확장인 [{Binding} 태그 확장](binding-markup-extension.md)의 속성이 설정되므로 여기서 중첩된 태그 확장 두 개가 사용됩니다. 안쪽 태그 확장이 먼저 평가되므로 리소스를 먼저 획득한 후 값으로 사용할 수 있습니다. 이 동일한 예제가 {Binding} 태그 확장에도 나와 있습니다.

## <a name="design-time-tools-support-for-the-staticresource-markup-extension"></a>**{StaticResource}** 태그 확장을 위한 디자인 타임 도구 지원

Microsoft Visual Studio2013 XAML 페이지에서 **{StaticResource}** 태그 확장을 사용 하는 경우 Microsoft IntelliSense 드롭다운에서 가능한 키 값을 포함할 수 있습니다. 예를 들어 "{StaticResource"를 입력하기 시작하면 즉시 현재 조회 범위의 리소스 키가 IntelliSense 드롭다운에 표시됩니다. 페이지 수준([**FrameworkElement.Resources**](https://msdn.microsoft.com/library/windows/apps/br208740)) 및 앱 수준([**Application.Resources**](https://msdn.microsoft.com/library/windows/apps/br242338))에 있는 일반적인 리소스 외에, [XAML 테마 리소스](https://msdn.microsoft.com/library/windows/apps/mt187274) 및 프로젝트에서 사용 중인 확장의 리소스도 표시됩니다.

리소스 키가 **{StaticResource}** 에서 일부로 사용되어 존재하는 경우 **정의로 이동**(F12) 기능이 해당 리소스를 확인하고 리소스가 정의되어 있는 사전을 표시할 수 있습니다. 테마 리소스의 경우에는 디자인 타임의 generic.xaml에 적용됩니다.

## <a name="related-topics"></a>관련 항목

* [ResourceDictionary 및 XAML 리소스 참조](https://msdn.microsoft.com/library/windows/apps/mt187273)
* [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)
* [x:Key 특성](x-key-attribute.md)
* [{ThemeResource} 태그 확장](themeresource-markup-extension.md)

