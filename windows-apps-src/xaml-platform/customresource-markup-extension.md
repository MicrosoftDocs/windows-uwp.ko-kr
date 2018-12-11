---
description: 사용자 지정 리소스 조회 구현에서 제공하는 리소스에 대한 참조를 평가하여 XAML 특성에 대한 값을 제공합니다. 리소스 조회는 CustomXamlResourceLoader 클래스 구현으로 수행됩니다.
title: CustomResource 태그 확장
ms.assetid: 3A59A8DE-E805-4F04-B9D9-A91E053F3642
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7eabcb188aa1687d36d4b4e6f432783aa68969de
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8872301"
---
# <a name="customresource-markup-extension"></a>{CustomResource} 태그 확장


사용자 지정 리소스 조회 구현에서 제공하는 리소스에 대한 참조를 평가하여 XAML 특성에 대한 값을 제공합니다. 리소스 조회는 [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) 클래스 구현으로 수행됩니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object property="{CustomResource key}" .../>
```

## <a name="xaml-values"></a>XAML 값

| 용어 | 설명 |
|------|-------------|
| 키 | 요청된 리소스에 대한 키입니다. 키가 처음 할당되는 방법은 현재 사용하도록 등록되어 있는 [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) 클래스의 구현과 관련이 있습니다. |

## <a name="remarks"></a>설명

**CustomResource**는 사용자 지정 리소스 리포지토리에 정의된 값을 가져오는 기술입니다. 이 기술은 비교적 고급 기술이며 대부분의 Windows 런타임 앱 시나리오에서 사용하지 않습니다.

**CustomResource**를 리소스 사전에서 확인하는 방법은 [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327)가 구현되는 방법에 따라 광범위하게 달라질 수 있으므로 이 항목에서 설명하지 않습니다.

[**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) 구현의 [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340) 메서드는 태그에서 `{CustomResource}`가 사용될 때마다 Windows 런타임 XAML 파서에서 호출합니다. **GetResource**에 전달된 *resourceId*는 *key* 인수에서 제공되며 다른 입력 매개 변수는 사용이 적용되는 속성과 같은 컨텍스트에서 제공됩니다.

`{CustomResource}` 사용은 기본적으로 작동하지 않습니다([**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340)의 기본 구현이 완료되지 않음). 유효한 `{CustomResource}` 참조를 만들려면 다음 각 단계를 수행해야 합니다.

1.  [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327)에서 사용자 지정 클래스를 파생하여 [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340) 메서드를 재정의합니다. 구현에서 기본을 호출하지 않아야 합니다.
2.  초기화 로직의 클래스를 참조하도록 [**CustomXamlResourceLoader.Current**](https://msdn.microsoft.com/library/windows/apps/br243328)를 설정합니다. 이 작업은 `{CustomResource}` 확장 사용법이 포함된 페이지 수준 XAML이 로드되기 전에 이루어져야 합니다. **CustomXamlResourceLoader.Current**를 설정하는 위치 중 하나는 [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) 코드 숨김 템플릿에서 자동 생성된 App.xaml 하위 클래스 생성자입니다.
3.  이제 앱이 페이지로 로드되는 XAML에서나 XAML 리소스 사전에서 `{CustomResource}` 확장을 사용할 수 있습니다.

**CustomResource**는 태그 확장입니다. 태그 확장은 특정 값을 리터럴 값 또는 처리기 이름이 아닌 다른 값이 되도록 이스케이프해야 하는 요구 사항이 있는 경우 구현되며, 이러한 요구 사항은 특정 형식 또는 속성에 형식 변환기를 배치하는 것보다 더 포괄적입니다. XAML의 모든 태그 확장은 특성 구문에 "\{" 및 "\}" 문자를 사용하며, 여기서 특성 구문은 XAML 프로세서가 태그 확장이 특성을 처리해야 함을 인식하는 데 사용하는 규칙입니다.

## <a name="related-topics"></a>관련 항목

* [ResourceDictionary 및 XAML 리소스 참조](https://msdn.microsoft.com/library/windows/apps/mt187273)
* [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327)
* [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340)

