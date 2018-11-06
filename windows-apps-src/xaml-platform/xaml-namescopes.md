---
author: jwmsft
description: XAML 이름 범위에는 개체의 XAML 정의 이름과 이에 해당하는 인스턴스 항목 사이의 관계가 저장됩니다. 이 개념은 다른 프로그래밍 언어 및 기술에서 사용되는 이름 범위라는 용어의 보다 넓은 의미에 가깝습니다.
title: XAML 이름 범위
ms.assetid: EB060CBD-A589-475E-B83D-B24068B54C21
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8fcbc1566d2b2b5ffc6889a57dd7656a3466d2a9
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2018
ms.locfileid: "6049624"
---
# <a name="xaml-namescopes"></a>XAML 이름 범위


*XAML 이름 범위*에는 개체의 XAML 정의 이름과 이에 해당하는 인스턴스 항목 사이의 관계가 저장됩니다. 이 개념은 다른 프로그래밍 언어 및 기술에서 사용되는 *이름 범위*라는 용어의 보다 넓은 의미에 가깝습니다.

## <a name="how-xaml-namescopes-are-defined"></a>XAML 이름 범위 정의 방법

XAML 이름 범위의 이름을 사용하면 처음 XAML에 선언된 개체를 참조할 수 있습니다. XAML의 내부 구문 분석 결과로 런타임 시 XAML 선언에서 이들 개체의 관계 전체 또는 일부를 유지하는 개체 집합이 만들어집니다. 이러한 관계는 생성된 개체의 특정 개체 속성으로 유지되거나 프로그래밍 모델 API에서 유틸리티 메서드에 노출됩니다.

XAML 이름 범위의 이름을 사용하는 가장 일반적인 경우는 개체 인스턴스에 대한 직접 참조로, 이 참조는 partial 클래스 템플릿에 생성된 **InitializeComponent** 메서드와 함께 결합된 태그 컴파일 단계(프로젝트 빌드 작업)에서 사용할 수 있습니다.

또한 런타임 시 유틸리티 메서드 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)을 직접 사용하여 XAML 태그에서 이름으로 정의된 개체에 대한 참조를 반환할 수도 있습니다.

### <a name="more-about-build-actions-and-xaml"></a>빌드 작업 및 XAML에 대한 자세한 정보

기술적으로는 XAML 자체가 태그 컴파일러로 처리되는 동시에 코드 숨김을 위해 정의하는 partial 클래스와 XAML이 함께 컴파일됩니다. 태그에 정의된 각 개체 요소와 **Name** 또는 [x:Name 특성](x-name-attribute.md)은 XAML 이름과 일치하는 이름으로 내부 필드를 생성합니다. 이 필드는 처음에는 비어 있습니다. 그런 다음 클래스는 모든 XAML이 로드된 후에만 호출되는 **InitializeComponent** 메서드를 생성합니다. **InitializeComponent** 논리에서, 각 내부 필드는 동일한 이름 문자열에 대한 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) 반환 값으로 채워집니다. 컴파일 후 Windows 런타임 앱 프로젝트의 /obj 하위 폴더에서 각 XAML 페이지에 대해 만들어지는 ".g"(생성된(generated)) 파일을 보면 이 인프라를 직접 확인할 수 있습니다. 또한 필드 및 **InitializeComponent** 메서드를 리플렉션하거나 해당 인터페이스 언어 콘텐츠를 확인하면 결과 어셈블리의 멤버로 이 필드와 메서드를 볼 수 있습니다.

**참고**VisualC + + 구성 요소 확장에 대 한 구체적으로 (C + + CX) 앱의 경우 **X:name** 참조에 대 한 지원 필드는 XAML 파일의 루트 요소에 대해 만들어지지 않습니다. C++/CX 코드 숨김에서 루트 개체를 참조해야 하는 경우 다른 API나 트리 통과를 사용합니다. 예를 들어 알려진 명명된 자식 요소에 대해 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)을 호출한 다음 [**Parent**](https://msdn.microsoft.com/library/windows/apps/br208739)를 호출할 수 있습니다.

## <a name="creating-objects-at-run-time-with-xamlreaderload"></a>XamlReader.Load로 런타임에 개체 만들기

XAML은 [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) 메서드의 문자열 입력으로 사용할 수도 있습니다. 이 메서드는 초기 XAML 소스 구문 분석 작업과 비슷하게 작동합니다. **XamlReader.Load**는 연결이 끊긴 개체 트리를 런타임에 새로 만듭니다. 그러면 연결이 끊긴 트리를 기본 개체 트리의 특정 지점에 연결할 수 있습니다. 만들어진 개체 트리를 **Children**과 같은 콘텐츠 속성 컬렉션에 추가하거나, 개체 값을 사용하는 다른 속성을 설정하여(예: [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) 속성 값에 대해 새 [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101) 로드) 명시적으로 연결해야 합니다.

### <a name="xaml-namescope-implications-of-xamlreaderload"></a>XamlReader.Load의 XAML 이름 범위 의미

[**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048)에 의해 만들어진 새 개체 트리에서 정의한 예비 XAML 이름 범위는 제공된 XAML의 정의된 이름이 고유한지 평가합니다. 제공된 XAML 내에서 이름이 고유하지 않으면 **XamlReader.Load**에서 예외가 발생합니다. 연결이 끊긴 개체 트리는 기본 응용 프로그램 개체 트리에 연결될 경우 해당 XAML 이름 범위를 기본 응용 프로그램 XAML 이름 범위와 병합하려고 시도하지 않습니다. 그러므로 트리를 연결해도 앱의 개체 트리는 통합되지만 트리의 XAML 이름 범위는 여전히 서로 독립적인 상태입니다. 분할은 개체 간의 연결 지점에서 발생하며 **XamlReader.Load** 호출에서 반환하는 값이 될 일부 속성을 여기에 설정합니다.

이와 같이 연결이 끊긴 개별 XAML 이름 범위가 있으면 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) 메서드 호출 및 직접 관리되는 개체 참조가 더 이상 통합된 XAML 이름 범위에 대해 작동하지 않기 때문에 상황이 복잡해집니다. 대신, **FindName**이 호출되는 특정 개체가 범위를 암시적으로 나타내게 되는데, 이 범위는 호출 개체가 포함된 XAML 이름 범위입니다. 직접 관리되는 개체 참조의 경우에는 코드가 존재하는 클래스에 의해 범위가 암시적으로 지정됩니다. 일반적으로 앱 콘텐츠 "page"의 런타임 조작에 대한 코드 숨김은 루트 "page"를 지원하는 partial 클래스에 존재하므로, XAML 이름 범위는 루트 XAML 이름 범위입니다.

루트 XAML 이름 범위에서 명명된 개체를 가져오기 위해 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)을 호출하면 이 메서드는 [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048)에 의해 만들어진 개별 XAML 이름 범위에서 개체를 찾지 않습니다. 이와 반대로, 개별 XAML 이름 범위 외부에서 가져온 개체에서 **FindName**을 호출하면 이 메서드는 루트 XAML 이름 범위에서 명명된 개체를 찾지 않습니다.

이 개별 XAML 이름 범위 문제는 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) 호출을 사용하여 XAML 이름 범위에서 이름으로 개체를 찾을 경우에만 영향을 줍니다.

다음과 같은 여러 가지 기술을 사용하여 다른 XAML 이름 범위에 정의되어 있는 개체에 대한 참조를 가져올 수 있습니다.

-   [**Parent**](https://msdn.microsoft.com/library/windows/apps/br208739) 및/또는 개체 트리 구조에 존재하는 것으로 확인된 컬렉션 속성(예: [**Panel.Children**](https://msdn.microsoft.com/library/windows/apps/br227514)에서 반환한 컬렉션)을 사용하여 전체 트리를 단계별로 실행합니다.
-   개별 XAML 이름 범위에서 호출하는 경우 루트 XAML 이름 범위를 가져오려면 항상 현재 표시된 주 창에 대한 참조를 가져오는 것이 간편합니다. `Window.Current.Content` 호출이 포함된 한 줄의 코드로 현재 응용 프로그램 창에서 눈에 보이는 루트(콘텐츠 소스라고도 하는 루트 XAML 요소)를 가져올 수 있습니다. 그런 다음 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706)로 캐스팅하고 이 범위에서 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)을 호출할 수 있습니다.
-   루트 XAML 이름 범위에서 호출하는 경우 개별 XAML 이름 범위 내의 개체를 가져오려면 코드에서 해당 작업을 계획하고, [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048)에서 반환된 다음 기본 개체 트리에 추가된 개체에 대한 참조를 보존하는 것이 가장 바람직합니다. 이제 이 개체는 개별 XAML 이름 범위 안에서 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)을 호출할 수 있는 유효한 개체입니다. 이 개체는 전역 변수로 유지할 수도 있고 메서드 매개 변수를 사용하여 전달할 수도 있습니다.
-   시각적 트리를 확인하면 이름 및 XAML 이름 범위와 관련하여 고려해야 할 사항을 무시할 수 있습니다. [**VisualTreeHelper**](https://msdn.microsoft.com/library/windows/apps/br243038) API를 사용하여 시각적 트리를 위치 및 인덱스만을 기준으로 부모 개체 및 자식 컬렉션 관점에서 탐색할 수 있습니다.

## <a name="xaml-namescopes-in-templates"></a>템플릿의 XAML 이름 범위

XAML의 템플릿을 사용하면 간편하게 콘텐츠를 다시 사용 및 적용할 수 있지만 템플릿 수준에서 이름이 정의된 요소가 템플릿에 포함될 수도 있습니다. 동일한 템플릿이 한 페이지에서 여러 번 사용될 수 있기 때문에 템플릿에서는 스타일 또는 템플릿이 적용되는 포함 페이지와는 별도로 자체 XAML 이름 범위를 정의합니다. 다음 예를 참조하세요.

```xml
<Page
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
  xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"  >
  <Page.Resources>
    <ControlTemplate x:Key="MyTemplate">
      ....
      <TextBlock x:Name="MyTextBlock" />
    </ControlTemplate>
  </Page.Resources>
  <StackPanel>
    <SomeControl Template="{StaticResource MyTemplate}" />
    <SomeControl Template="{StaticResource MyTemplate}" />
  </StackPanel>
</Page>
```

여기에서는 서로 다른 두 컨트롤에 동일한 템플릿이 적용됩니다. 템플릿에 개별 XAML 이름 범위가 없으면 템플릿에 사용된 "MyTextBlock" 이름 때문에 이름 충돌이 발생합니다. 템플릿의 각 인스턴스화에는 고유한 XAML 이름 범위가 있으므로 이 예에서 인스턴스화된 각 템플릿의 XAML 이름 범위에는 정확하게 하나의 이름만 포함됩니다. 하지만 루트 XAML 이름 범위에는 어떤 템플릿의 이름도 포함되지 않습니다.

XAML 이름 범위는 서로 독립적이므로 템플릿이 적용되는 페이지의 범위에서 템플릿 내에 명명된 요소를 찾으려면 다른 방법이 필요합니다. 개체 트리의 일부 개체에서 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)을 호출하는 대신 템플릿이 적용된 개체를 가져온 다음 [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416)를 호출합니다. 컨트롤 작성자가 적용된 템플릿에서 명명된 특정 요소가 컨트롤 자체로 정의되는 동작의 대상이 되는 규칙을 생성하려는 경우 컨트롤 구현 코드의 **GetTemplateChild** 메서드를 사용할 수 있습니다. **GetTemplateChild** 메서드는 보호되어 있으므로 컨트롤 작성자만 액세스할 수 있습니다. 또한 이름 부분 및 템플릿 부분에 이름을 지정하고 이 이름을 컨트롤 클래스에 적용된 특성 값으로 보고하기 위해 컨트롤 작성자가 따라야 하는 규칙이 있습니다. 다른 템플릿을 적용하고자 하는 컨트롤 사용자는 컨트롤 기능을 유지하기 위해 명명된 부분을 바꿔야 하지만 이 기술을 사용하면 컨트롤 사용자가 중요한 부분의 이름을 찾을 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [XAML 개요](xaml-overview.md)
* [x:Name 특성](x-name-attribute.md)
* [빠른 시작: 컨트롤 템플릿](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)
* [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048)
* [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)
 

