---
description: XAML 이름 범위는 개체의 XAML 정의 이름과 해당 인스턴스 간의 관계를 저장 합니다. 이 개념은 다른 프로그래밍 언어 및 기술에서 용어 이름 범위의 광범위 한 의미와 비슷합니다.
title: XAML 이름 범위
ms.assetid: EB060CBD-A589-475E-B83D-B24068B54C21
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e4fa430cdd6c2f7b47576478ec30f0f139b80363
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173757"
---
# <a name="xaml-namescopes"></a>XAML 이름 범위


*Xaml 이름 범위* 는 개체의 xaml 정의 이름과 해당 인스턴스 간의 관계를 저장 합니다. 이 개념은 다른 프로그래밍 언어 및 기술에서 용어 *이름* 범위의 광범위 한 의미와 비슷합니다.

## <a name="how-xaml-namescopes-are-defined"></a>XAML 이름 범위를 정의 하는 방법

XAML 이름 범위에서 이름은 사용자 코드에서 처음에 XAML로 선언 된 개체를 참조할 수 있도록 합니다. XAML 구문 분석의 내부 결과는 런타임에서 이러한 개체가 XAML 선언에 있던 관계의 일부 또는 모두를 유지 하는 개체 집합을 만드는 것입니다. 이러한 관계는 생성 된 개체의 특정 개체 속성으로 유지 되거나 프로그래밍 모델 Api의 유틸리티 메서드에 노출 됩니다.

XAML 이름 범위에서 가장 일반적으로 사용 되는 개체는 개체 인스턴스에 대 한 직접 참조로 서, partial 클래스 템플릿에서 생성 된 **InitializeComponent** 메서드와 결합 된 프로젝트 빌드 작업으로 태그 컴파일 pass에서 사용 하도록 설정 됩니다.

런타임에 유틸리티 메서드 [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) 을 사용 하 여 XAML 태그의 이름으로 정의 된 개체에 대 한 참조를 반환할 수도 있습니다.

### <a name="more-about-build-actions-and-xaml"></a>빌드 작업 및 XAML에 대 한 자세한 정보

기술적으로 수행 되는 작업은 xaml 자체에서 코드 숨김으로 정의 된 XAML 및 partial 클래스가 함께 컴파일되는 동시에 태그 컴파일러를 통과 하는 것입니다. 태그에 정의 된 **name** 또는 [x:Name 특성이](x-name-attribute.md) 있는 각 개체 요소는 XAML 이름과 일치 하는 이름의 내부 필드를 생성 합니다. 이 필드는 처음에는 비어 있습니다. 그런 다음 클래스는 모든 XAML이 로드 된 후에만 호출 되는 **InitializeComponent** 메서드를 생성 합니다. **InitializeComponent** 논리 내에서 각 내부 필드는 해당 하는 이름 문자열에 대 한 [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) 반환 값으로 채워집니다. 컴파일 후에 Windows 런타임 앱 프로젝트의/obj 하위 폴더에 있는 각 XAML 페이지에 대해 생성 된 ". g" (생성 된) 파일을 살펴보면이 인프라를 확인할 수 있습니다. 또한 필드와 **InitializeComponent** 메서드를이를 반영 하거나 인터페이스 언어 내용을 검사 하는 경우 결과 어셈블리의 멤버로 볼 수 있습니다.

**참고**    특히 Visual C++ 구성 요소 확장 (c + +/CX) 앱의 경우 XAML 파일의 루트 요소에 대해 **x:Name** 참조의 지원 필드가 생성 되지 않습니다. C + +/CX 코드 숨김으로 루트 개체를 참조 해야 하는 경우 다른 Api 또는 트리 트래버스를 사용 합니다. 예를 들어 알려진 명명 된 자식 요소에 대해 [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) 을 호출한 다음 [**Parent**](/uwp/api/windows.ui.xaml.frameworkelement.parent)를 호출할 수 있습니다.

## <a name="creating-objects-at-run-time-with-xamlreaderload"></a>런타임에 XamlReader를 사용 하 여 개체 만들기

XAML은 [**XamlReader**](/uwp/api/windows.ui.xaml.markup.xamlreader.load) 메서드에 대 한 문자열 입력으로 사용 될 수도 있습니다 .이는 초기 xaml 소스 구문 분석 작업에와 유사 작용 합니다. **XamlReader** 런타임에 개체의 연결 되지 않은 새 트리를 만듭니다. 그러면 연결 되지 않은 트리를 주 개체 트리의 특정 지점에 연결할 수 있습니다. 만든 개체 트리를 **자식과**같은 콘텐츠 속성 컬렉션에 추가 하거나 개체 값을 사용 하는 다른 속성을 설정 하 여 명시적으로 연결 해야 합니다 (예: [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) 속성 값에 대 한 새 [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) 로드).

### <a name="xaml-namescope-implications-of-xamlreaderload"></a>XamlReader의 XAML 이름 범위 의미

[**XamlReader**](/uwp/api/windows.ui.xaml.markup.xamlreader.load) 에서 만든 새 개체 트리로 정의 된 예비 xaml 이름 범위는 제공 된 xaml에서 정의 된 모든 이름을 고유 하 게 평가 합니다. 제공 된 XAML의 이름이이 시점에서 내부적으로 고유 하지 않은 경우 **XamlReader** 는 예외를 throw 합니다. 주 응용 프로그램 개체 트리에 연결 된 경우 연결이 끊어진 개체 트리는 해당 XAML 이름 범위를 주 응용 프로그램 XAML 이름 범위와 병합 하려고 시도 하지 않습니다. 트리를 연결한 후에는 앱에 통합 개체 트리가 있지만 해당 트리의 내부에는 불연속 XAML 이름 범위가 있습니다. 분할은 개체 간 연결 지점에서 발생 합니다. 여기서 일부 속성은 **XamlReader** 호출에서 반환 된 값으로 설정 됩니다.

불연속 및 연결이 끊어진 XAML 이름 범위를 사용할 경우에는 [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) 메서드 호출과 직접 관리 되는 개체 참조가 통합 xaml 이름 범위에 대해 더 이상 작동 하지 않습니다. 대신, **FindName** 이 호출 되는 특정 개체는 범위를 암시 하며 범위는 호출 하는 개체가 포함 된 XAML 이름 범위입니다. 직접 관리 되는 개체 참조 사례에서 범위는 코드가 존재 하는 클래스에 의해 암시 됩니다. 일반적으로 응용 프로그램 콘텐츠의 "페이지"에 대 한 런타임 상호 작용에 대 한 코드 숨김이 루트 "페이지"를 지 원하는 partial 클래스에 있으므로 XAML 이름 범위는 루트 XAML 이름 범위입니다.

[**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) 을 호출 하 여 루트 xaml 이름 범위에 명명 된 개체를 가져오는 경우 메서드는 [**XamlReader**](/uwp/api/windows.ui.xaml.markup.xamlreader.load)에서 만든 불연속 xaml 이름 범위에서 개체를 찾지 않습니다. 반대로 불연속 XAML 이름 범위 밖에서 가져온 개체에서 **FindName** 을 호출 하는 경우 메서드는 루트 xaml 이름 범위에서 명명 된 개체를 찾을 수 없습니다.

이 불연속 XAML 이름 범위 문제는 [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) 호출을 사용할 때 XAML 이름 범위에서 이름으로 개체를 찾는 데만 영향을 줍니다.

다른 XAML 이름 범위에 정의 된 개체에 대 한 참조를 가져오려면 다음과 같은 여러 가지 방법을 사용할 수 있습니다.

-   개체 트리 구조에 존재 하는 것으로 알려진 [**부모**](/uwp/api/windows.ui.xaml.frameworkelement.parent) 및/또는 컬렉션 속성을 사용 하 여 불연속 단계의 전체 트리를 탐색 합니다 (예: [**Panel. 자식**](/uwp/api/windows.ui.xaml.controls.panel.children)에 의해 반환 되는 컬렉션).
-   불연속 XAML 이름 범위에서를 호출 하 고 루트 XAML 이름 범위를 원하는 경우 현재 표시 된 주 창에 대 한 참조를 항상 쉽게 가져올 수 있습니다. 호출을 사용 하 여 코드의 한 줄에서 현재 응용 프로그램 창에 있는 시각적 루트 (콘텐츠 소스가 라고도 함)를 가져올 수 있습니다 `Window.Current.Content` . 그런 다음 [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement) 로 캐스팅 하 고이 범위에서 [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) 을 호출할 수 있습니다.
-   루트 XAML 이름 범위에서를 호출 하 고 불연속 XAML 이름 범위 내에서 개체를 원하는 경우 코드에서 미리 계획 하 고 [**XamlReader**](/uwp/api/windows.ui.xaml.markup.xamlreader.load) 에서 반환 된 개체에 대 한 참조를 유지 한 다음 주 개체 트리에 추가 하는 것이 가장 좋습니다. 이 개체는 이제 불연속 XAML 이름 범위 내에서 [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) 을 호출 하는 데 유효한 개체입니다. 이 개체를 전역 변수로 사용할 수 있도록 유지 하거나, 메서드 매개 변수를 사용 하 여 전달할 수 있습니다.
-   시각적 트리를 검사 하면 이름 및 XAML 이름 범위를 고려 하지 않아도 됩니다. [**VisualTreeHelper**](/uwp/api/Windows.UI.Xaml.Media.VisualTreeHelper) API를 사용 하면 위치 및 인덱스를 기준으로 하 여 부모 개체와 자식 컬렉션을 기준으로 시각적 트리를 트래버스할 수 있습니다.

## <a name="xaml-namescopes-in-templates"></a>템플릿의 XAML 이름 범위

XAML의 템플릿은 간단한 방법으로 콘텐츠를 다시 사용 하 고 다시 적용 하는 기능을 제공 하지만 템플릿 수준에서 정의 된 이름의 요소를 템플릿에 포함할 수도 있습니다. 이런 템플릿은 한 페이지에서 여러 번 사용될 수 있기 때문에 이러한 이유로 템플릿은 스타일이 나 템플릿이 적용 되는 포함 하는 페이지와 관계 없이 고유한 XAML 이름 범위를 정의 합니다. 다음 예제를 참조하세요.

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

여기에서는 동일한 템플릿이 두 개의 다른 컨트롤에 적용 됩니다. 템플릿에 불연속 XAML 이름 범위가 없는 경우 템플릿에 사용 되는 "MyTextBlock" 이름은 이름 충돌을 발생 시킵니다. 템플릿의 각 인스턴스에는 고유한 XAML 이름 범위가 있으므로 이 예제에서 인스턴스화된 각 템플릿의 XAML 이름 범위에는 정확하게 하나의 이름만 포함됩니다. 그러나 루트 XAML 이름 범위에는 두 템플릿의 이름이 포함 되지 않습니다.

별도의 XAML 이름 범위 때문에 템플릿이 적용 되는 페이지 범위에서 템플릿 내에 명명 된 요소를 찾는 것은 다른 기술이 필요 합니다. 개체 트리의 일부 개체에서 [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) 을 호출 하는 대신 템플릿이 적용 된 개체를 먼저 가져온 다음 [**system.windows.frameworkelement.gettemplatechild**](/uwp/api/windows.ui.xaml.controls.control.gettemplatechild)를 호출 합니다. 컨트롤 작성자 인 경우 적용 된 템플릿의 특정 명명 된 요소가 컨트롤 자체에서 정의한 동작의 대상이 되는 규칙을 생성 하는 경우 컨트롤 구현 코드에서 **system.windows.frameworkelement.gettemplatechild** 메서드를 사용할 수 있습니다. **System.windows.frameworkelement.gettemplatechild** 메서드는 protected 이므로 컨트롤 작성자만 액세스할 수 있습니다. 또한 컨트롤 작성자가 파트 및 템플릿 파트의 이름을 지정 하 고이를 컨트롤 클래스에 적용 되는 특성 값으로 보고 하기 위해 따라야 하는 규칙도 있습니다. 이 기법을 사용 하면 중요 한 파트의 이름을 검색할 수 있습니다. 다른 템플릿을 적용할 수 있는 사용자는 제어 기능을 유지 하기 위해 명명 된 파트를 대체 해야 합니다.

## <a name="related-topics"></a>관련 항목

* [XAML 개요](xaml-overview.md)
* [x:Name 특성](x-name-attribute.md)
* [빠른 시작: 컨트롤 템플릿](/previous-versions/windows/apps/hh465374(v=win.10))
* [**XamlReader**](/uwp/api/windows.ui.xaml.markup.xamlreader.load)
* [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname)
 