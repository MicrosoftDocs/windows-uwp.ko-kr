---
description: 사용자 지정 리소스 조회 구현에서 제공 되는 리소스에 대 한 참조를 평가 하 여 모든 XAML 특성에 대 한 값을 제공 합니다. 리소스 조회는 CustomXamlResourceLoader 클래스 구현에 의해 수행 됩니다.
title: CustomResource 태그 확장
ms.assetid: 3A59A8DE-E805-4F04-B9D9-A91E053F3642
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 094a2a957493787acef8e97b49b61778fc1ec42f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155107"
---
# <a name="customresource-markup-extension"></a>{CustomResource} 태그 확장


사용자 지정 리소스 조회 구현에서 제공 되는 리소스에 대 한 참조를 평가 하 여 모든 XAML 특성에 대 한 값을 제공 합니다. 리소스 조회는 [**Customxamlresourceloader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) 클래스 구현에 의해 수행 됩니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object property="{CustomResource key}" .../>
```

## <a name="xaml-values"></a>XAML 값

| 용어 | Description |
|------|-------------|
| key | 요청한 리소스의 키입니다. 키가 처음에 할당 되는 방법은 현재 사용 하도록 등록 된 [**Customxamlresourceloader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) 클래스의 구현에만 적용 됩니다. |

## <a name="remarks"></a>설명

**Customresource** 는 사용자 지정 리소스 리포지토리의 다른 곳에서 정의 된 값을 가져오는 기술입니다. 이 기술은 비교적 고급 이며 대부분의 Windows 런타임 앱 시나리오에서 사용 되지 않습니다.

[**Customxamlresourceloader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) 가 구현 되는 방식에 따라 광범위 하 게 달라질 수 있으므로이 항목에서 **customresource** 를 리소스 사전으로 확인 하는 방법은이 항목에서 설명 하지 않습니다.

[**Customxamlresourceloader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) 구현의 [**getresource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource) 메서드는 `{CustomResource}` 태그에서 사용이 발생할 때마다 Windows 런타임 XAML 파서에서 호출 합니다. **Getresource** 에 전달 된 *resourceId* 는 *키* 인수에서 가져오며 다른 입력 매개 변수는 사용이 적용 되는 속성과 같이 컨텍스트에서 제공 됩니다.

`{CustomResource}`사용은 기본적으로 작동 하지 않습니다 ( [**getresource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource) 의 기본 구현이 불완전 함). 유효한 참조를 만들려면 `{CustomResource}` 다음 단계를 각각 수행 해야 합니다.

1.  [**Customxamlresourceloader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) 에서 사용자 지정 클래스를 파생 시키고 [**getresource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource) 메서드를 재정의 합니다. 구현에서 base를 호출 하지 마세요.
2.  [**Customxamlresourceloader. Current**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.current) 를 설정 하 여 초기화 논리에서 클래스를 참조 합니다. 이는 확장 사용이 포함 된 페이지 수준 XAML이 로드 되기 전에 발생 해야 합니다 `{CustomResource}` . **Customxamlresourceloader** 를 설정 하는 한 가지 장소입니다. Current는 [**응용 프로그램**](/uwp/api/Windows.UI.Xaml.Application) 서브 클래스 생성자에 있습니다.
3.  이제 `{CustomResource}` xaml에서 앱이 페이지로 로드 되거나 xaml 리소스 사전 내에서 확장을 사용할 수 있습니다.

**Customresource** 는 태그 확장입니다. 태그 확장은 특성 값을 리터럴 값 또는 처리기 이름이 아닌 다른 값이 되도록 이스케이프해야 하는 요구 사항이 있는 경우 일반적으로 구현되며 이러한 요구 사항은 특정 형식 또는 속성에 형식 변환기를 배치하는 것보다 더 포괄적입니다. XAML의 모든 태그 확장은 \{ 특성 구문에서 "" 및 "" 문자를 사용 합니다 \} .이는 xaml 프로세서에서 태그 확장이 특성을 처리 해야 함을 인식 하는 데 사용 되는 규칙입니다.

## <a name="related-topics"></a>관련 항목

* [ResourceDictionary 및 XAML 리소스 참조](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)
* [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader)
* [**GetResource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource)