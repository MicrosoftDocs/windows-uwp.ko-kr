---
author: jwmsft
title: 앱 분석
description: 성능 문제를 위해 앱을 분석합니다.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 346e6790c6578bf861ba1dda937eae6d4d50f00f
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6034229"
---
# <a name="app-analysis-overview"></a>앱 분석 개요

앱 분석은 개발자에게 성능 문제를 미리 알려 주는 도구입니다. 앱 분석은 성능 지침 및 모범 사례 집합에 대해 앱 코드를 실행합니다.

앱 분석은 앱이 실행되는 일반 성능 문제의 규칙 집합에서 문제를 식별합니다. 필요한 경우 앱 분석은 Visual Studio의 타임라인 도구, 원본 정보 및 조사하는 수단을 제공하는 문서를 가리킵니다.

앱 분석의 규칙은 앱을 확인하는 지침 또는 모범 사례를 참조합니다.

## <a name="decoded-image-size-larger-than-render-size"></a>렌더링 크기보다 더 크게 디코딩된 이미지 크기

이미지는 매우 높은 해상도로 캡처되므로 디스크에서 로드된 후 이미지 데이터 및 더 많은 메모리를 디코딩하는 경우 앱에서 CPU를 더 많이 사용할 수 있습니다. 하지만 단지 원본 크기보다 작게 표시하기 위해 고해상도 이미지를 디코딩하고 메모리에 저장하는 것은 아무 의미가 없습니다. 대신, DecodePixelWidth 및 DecodePixelHeight 속성을 사용하여 화면에 정확한 크기로 그려지는 이미지 버전을 만듭니다.

### <a name="impact"></a>영향

기본이 아닌 크기로 이미지를 표시하면 CPU 시간(적절한 크기로 디코딩 및 다운로드 시간 때문) 및 메모리 모두에 부정적인 영향을 줄 수 있습니다.

### <a name="causes-and-solutions"></a>원인 및 해결 방법

#### <a name="image-is-not-being-set-asynchronously"></a>이미지가 비동기적으로 설정되지 않음

앱이 SetSourceAsync() 대신 SetSource()를 사용하고 있습니다. 스트림을 설정할 때 항상 [**SetSource**](https://msdn.microsoft.com/library/windows/apps/BR243255)를 사용하지 말고 [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522)를 대신 사용하여 이미지를 비동기적으로 디코드해야 합니다. 

#### <a name="image-is-being-called-when-the-imagesource-is-not-in-the-live-tree"></a>이미지가 ImageSource가 라이브 트리에 없을 때 호출되고 있습니다.

BitmapImage가 SetSourceAsync 또는 UriSource를 사용하여 콘텐츠를 설정한 다음 라이브 XAML 트리에 연결되었습니다. 원본을 설정하기 전에 항상 [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/BR243235)를 라이브 트리에 연결해야 합니다. 이미지 요소 또는 브러시가 태그에서 지정되면 항상 자동으로 그렇게 됩니다. 아래에 예제가 나와 있습니다. 

**라이브 트리 예제**

예제 1(좋음) - 태그에 지정된 URI(Uniform Resource Identifier)

```xml
<Image x:Name="myImage" UriSource="Assets/cool-image.png"/>
```

예제 2 태그 - 코드 숨김에 지정된 URI

```xml
<Image x:Name="myImage"/>
```

예제 2 코드 숨김(좋음) - UriSource를 설정하기 전에 BitmapImage를 트리에 연결

```vb
var bitmapImage = new BitmapImage();
myImage.Source = bitmapImage;
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
```

예제 2 코드 숨김 (나쁨)-트리에 연결 하기 전에 BitmapImage의 UriSource를 설정 합니다.

```vb
var bitmapImage = new BitmapImage();
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
myImage.Source = bitmapImage;
```

#### <a name="image-brush-is-non-rectangular"></a>이미지 브러시가 직사각형이 아님 

이미지가 사각형이 아닌 브러시에 사용되는 경우 해당 이미지는 소프트웨어 래스터화 경로를 사용하므로 전혀 이미지의 배율을 조정하지 않습니다. 또한 소프트웨어 및 하드웨어 메모리 모두에 이미지의 복사본을 저장해야 합니다. 예를 들어 이미지가 타원에 대한 브러시로 사용되는 경우 잠재적으로 큰 전체 이미지는 내부적으로 두 번 저장됩니다. 사각형이 아닌 브러시를 사용하는 경우 앱은 해당 이미지의 배율을 렌더링되는 크기로 대략 미리 조정합니다.

또는 [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 및 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) 속성을 사용하여 화면에 정확한 크기로 그려지는 이미지 버전을 만들도록 명시적인 디코드 크기를 설정할 수 있습니다.

```xml
<Image>
    <Image.Source>
    <BitmapImage UriSource="ms-appx:///Assets/highresCar.jpg" 
                 DecodePixelWidth="300" DecodePixelHeight="200"/>
    </Image.Source>
</Image>
```

[**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 및 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241)에 대한 단위는 기본적으로 실제 픽셀입니다. [**DecodePixelType**](https://msdn.microsoft.com/library/windows/apps/Dn298545) 속성을 사용하면 이 동작을 변경할 수 있습니다. 즉 **DecodePixelType**을 **Logical**로 설정하면 디코드 크기가 다른 XAML 콘텐츠와 유사하게 시스템의 현재 배율을 자동으로 고려합니다. 그러므로 예를 들어 **DecodePixelWidth** 및 **DecodePixelHeight**를 이미지가 표시되는 이미지 컨트롤의 Height 및 Width 속성과 일치시키려는 경우 **DecodePixelType**을 **Logical**로 설정하는 것이 일반적으로 적절합니다. 실제 픽셀을 사용하는 기본 동작에서는 직접 시스템의 현재 배율을 고려해야 하므로 사용자가 디스플레이 기본 설정을 변경하는 경우 배율 변경 알림을 수신하게 됩니다.

적절한 디코드 크기를 미리 결정할 수 없는 일부 경우, 명시적인 DecodePixelWidth/DecodePixelHeight가 지정되지 않았을 때 XAML의 적절한 크기로 자동 디코드에 맡기는 것이 좋습니다. 이를 사용하면 가장 최선의 노력을 들여서 적절한 크기로 이미지를 디코드할 수 있습니다.

미리 이미지 콘텐츠 크기를 알고 있는 경우 명시적인 디코드 크기를 설정하는 것이 좋습니다. 제공된 디코드 크기가 다른 XAML 요소 크기와 관련되어 있는 경우 [**DecodePixelType**](https://msdn.microsoft.com/library/windows/apps/Dn298545)을 **Logical**로 함께 설정해야 합니다. 예를 들어 Image.Width와 Image.Height를 가진 콘텐츠 크기를 명시적으로 설정하는 경우 DecodePixelType을 DecodePixelType.Logical로 설정하여 동일한 논리적 픽셀 치수를 이미지 컨트롤로 사용한 다음 명시적으로 BitmapImage.DecodePixelWidth 및/또는 BitmapImage.DecodePixelHeight를 사용하여 잠재적으로 메모리가 많이 절약되도록 이미지의 크기를 제어할 수 있습니다.

디코딩된 콘텐츠의 크기를 결정할 때 Image.Stretch를 고려해야 합니다.

#### <a name="images-used-inside-of-bitmapicons-fall-back-to-decoding-to-natural-size"></a>BitmapIcons 내부에서 사용되는 이미지가 기본 크기로 디코딩되도록 대체 

[**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 및 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) 속성을 사용하여 화면에 정확한 크기로 그려지는 이미지 버전을 만들도록 명시적 디코드 크기를 설정합니다.

#### <a name="images-that-appear-extremely-large-on-screen-fall-back-to-decoding-to-natural-size"></a>화면에 매우 크게 표시되는 이미지가 기본 크기로 디코딩되도록 대체 

화면에 매우 크게 표시되는 이미지가 기본 크기로 디코딩되도록 대체됩니다. [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 및 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) 속성을 사용하여 화면에 정확한 크기로 그려지는 이미지 버전을 만들도록 명시적 디코드 크기를 설정합니다.

#### <a name="image-is-hidden"></a>이미지가 숨겨짐

호스트 이미지 요소 또는 브러시 또는 부모 요소에서 Opacity를 0으로 설정하거나 Visibility를 Collapsed로 설정해서 이미지가 숨겨졌습니다. 클리핑이나 투명도로 인해 화면에 표시되지 않는 이미지가 기본 크기로 디코딩되도록 대체될 수 있습니다. 

#### <a name="image-is-using-ninegrid-property"></a>이미지가 NineGrid 속성을 사용하고 있음

이미지가 [**NineGrid**](https://msdn.microsoft.com/library/windows/apps/BR242756)에 사용되는 경우 해당 이미지는 소프트웨어 래스터화 경로를 사용하므로 전혀 이미지의 배율을 조정하지 않습니다. 또한 소프트웨어 및 하드웨어 메모리 모두에 이미지의 복사본을 저장해야 합니다. **NineGrid**를 사용할 때 앱은 해당 이미지의 배율을 렌더링될 크기로 대략 미리 조정해야 합니다.

NineGrid 속성을 사용하는 이미지가 기본 크기로 디코딩되도록 대체됩니다. 원본 이미지에 ninegrid 효과를 추가하는 것이 좋습니다.

#### <a name="decodepixelwidth-or-decodepixelheight-are-set-to-a-size-thats-larger-than-the-image-will-appear-on-screen"></a>DecodePixelWidth 또는 DecodePixelHeight가 화면에 표시될 이미지보다 더 큰 크기로 설정됨 

DecodePixelWidth/Height가 이미지가 화면에 표시되는 것보다 명시적으로 크게 설정되면 앱에서 픽셀당 최대 4바이트의 추가 메모리를 불필요하게 사용하게 되므로 큰 이미지의 경우 순식간에 과도하게 사용됩니다. 또한 이미지가 쌍선형 배율을 사용하면서 축소되어 큰 배율의 경우 흐리게 표시될 수 있습니다.

#### <a name="image-is-decoded-as-part-of-producing-a-drag-and-drop-image"></a>이미지가 끌어서 놓기 이미지 생성의 일부로 디코딩됨

[**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 및 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) 속성을 사용하여 화면에 정확한 크기로 그려지는 이미지 버전을 만들도록 명시적 디코드 크기를 설정합니다.

## <a name="collapsed-elements-at-load-time"></a>로드 시 축소된 요소

앱의 일반 패턴은 처음에 UI의 요소를 숨기고 나중에 표시하는 것입니다. 대부분의 경우 이러한 요소는 로드 시 요소를 만드는 비용을 지불하지 않도록 x:Load 또는 x:DeferLoadStrategy를 사용하여 지연되어야 합니다.

여기에는 나중까지 항목을 숨기는 데 부울-표시 변환기가 사용되는 경우가 포함됩니다.

### <a name="impact"></a>영향

축소된 요소가 다른 요소와 함께 로드되어 로드 시간이 늘어납니다.

### <a name="cause"></a>Cause(원인)

이 규칙은 요소가 로드 시 축소되었기 때문에 트리거되었습니다. 요소를 축소하거나 불투명도를 0으로 설정하면 요소가 만들어지는 것을 방지하지 않습니다. 이 규칙은 기본값이 false인 부울-표시 변환기를 사용하는 앱에서 발생할 수 있습니다.

### <a name="solution"></a>솔루션

[x:Load attribute](../xaml-platform/x-load-attribute.md) 또는 [x:DeferLoadStrategy](https://msdn.microsoft.com/library/windows/apps/Mt204785)를 사용하면 UI 일부의 로드를 지연시켰다가 필요할 때 로드할 수 있습니다. 이는 첫 번째 프레임에 표시되지 않는 UI 처리를 지연하는 좋은 방법입니다. 필요할 때 또는 지연된 논리 집합의 일부로 요소를 로드할 수 있습니다. 로딩을 트리거하려면 로드하려는 요소에서 findName을 호출합니다. x:Load는 요소를 활성화하는 x:DeferLoadStrategy의 기능이 로드되지 않도록 확장하며 로딩 상태가 x:Bind를 통해 제어되도록 합니다.

경우에 따라 UI 부분을 표시하는 데 findName을 사용하지 않는 것이 좋을 수 있습니다. 이는 매우 짧은 대기 시간을 사용하여 단추 클릭 시 UI의 중요한 부분을 실현하려고 하는 경우에 해당됩니다. 이 경우 추가 메모리 비용을 지불하고 더욱 빠른 UI 지연 시간을 원할 수 있으며 이 경우 x:DeferLoadStrategy를 사용하고 실현하고자 하는 요소의 Visibility를 Collapsed로 설정해야 합니다. 페이지가 로드되고 UI 스레드를 사용할 수 있게 되면 요소를 로드하는 데 필요한 경우 findName을 호출할 수 있습니다. 요소의 Visibility를 Visible로 설정하기 전에는 사용자에게 요소가 표시되지 않습니다.

## <a name="listview-is-not-virtualized"></a>ListView가 가상화되지 않음

UI 가상화는 컬렉션 성능을 개선할 수 있는 가장 중요한 기능입니다. 이는 항목을 나타내는 UI 요소가 필요에 따라 만들어짐을 의미합니다. 1000개 항목의 컬렉션에 바인딩된 항목 컨트롤의 경우 동시에 모든 항목에 대한 UI를 만드는 것은 리소스 낭비입니다. 왜냐하면 이들을 동시에 모두 표시할 수 없기 때문입니다. ListView 및 GridView(및 기타 표준 ItemsControl 파생 컨트롤)는 UI 가상화를 수행합니다. 항목이 보기에 가까이(몇 페이지 밖) 스크롤되면 프레임워크가 항목에 대한 UI를 생성하고 이를 캐시합니다. 항목이 다시 표시될 것 같지 않은 경우 프레임워크는 메모리를 회수합니다.

UI 가상화는 컬렉션 성능을 개선하는 데 몇 가지 핵심 요소 중 하나일 뿐입니다. 컬렉션 성능을 개선하기 위한 두 가지 중요한 측면은 컬렉션 항목 및 데이터 가상화의 복잡성을 줄이는 것입니다. Listview 및 Gridview 내에서 컬렉션 성능을 개선하는 방법은 [ListView 및 GridView UI 최적화](https://msdn.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview)와 [ListView 및 Gridview 데이터 가상화](https://msdn.microsoft.com/windows/uwp/debug-test-perf/listview-and-gridview-data-optimization)에 대한 문서를 참조하세요.

### <a name="impact"></a>영향

가상화되지 않은 ItemsControl은 필요 이상으로 많은 해당 자식 항목을 로드하여 로드 시간과 리소스 사용량을 증가시킵니다.

### <a name="cause"></a>Cause(원인)

프레임워크가 표시될 수 있는 요소를 만들어야 하므로 뷰포트 개념이 UI 가상화에 중요합니다. 일반적으로 ItemsControl의 뷰포트는 논리적 컨트롤 크기입니다. 예를 들어 ListView의 뷰포트는 ListView 요소의 너비 및 높이입니다. 일부 패널에서는 자동 크기 조정 행 또는 열을 사용하여 자식 요소(예: ScrollViewer 및 Grid)에 무제한 공간을 허용합니다. 가상화된 ItemsControl이 이와 같은 패널에 배치된 경우 모든 항목을 표시할 수 있는 공간을 차지하므로 가상화에 실패합니다. 

### <a name="solution"></a>솔루션

이 경우 사용 중인 ItemsControl에서 너비와 높이를 설정하여 가상화를 복원합니다.

## <a name="ui-thread-blocked-or-idle-during-load"></a>로드하는 동안 UI 스레드가 차단되거나 유휴 상태임

UI 스레드 차단은 UI 스레드를 차단하는 오프-스레드 실행 함수에 대한 동기 호출을 나타냅니다.  

앱의 시작 성능을 향상시킬 모범 사례에 대한 전체 목록은 [앱 시작 성능에 대한 모범 사례](https://msdn.microsoft.com/windows/uwp/debug-test-perf/best-practices-for-your-app-s-startup-performance) 및 [UI 스레드 응답 유지](https://msdn.microsoft.com/windows/uwp/debug-test-perf/keep-the-ui-thread-responsive)를 참조하세요.

### <a name="impact"></a>영향

로드 중 차단되거나 유휴 상태인 UI 스레드는 레이아웃 및 기타 UI 작업을 차단하여 시작 시간을 증가시킵니다.

### <a name="cause"></a>Cause(원인)

UI 플랫폼 코드와 UI 앱 코드 모두 동일한 UI 스레드에서 실행됩니다. 이 스레드에서는 한 번에 하나의 명령만 실행될 수 있으므로 앱 코드가 이벤트를 처리하는 데 너무 오래 걸리는 경우 프레임워크에서 레이아웃을 실행할 수 없거나 사용자 개입을 나타내는 새 이벤트를 발생시킬 수 있습니다. 앱의 응답성은 작업을 처리하는 UI 스레드의 가용성과 관련이 있습니다.

### <a name="solution"></a>솔루션

앱의 일부가 제대로 작동하지 않더라도 앱과의 상호 작용이 가능할 수 있습니다. 예를 들어 앱에서 검색하는 데 오랜 시간이 걸리는 데이터를 표시할 경우 데이터를 비동기적으로 검색함으로써 코드가 앱의 시작 코드와 별개로 실행되게 할 수 있습니다. 데이터를 사용할 수 있게 되면 앱의 사용자 인터페이스를 그 데이터로 채웁니다. 이 플랫폼은 앱을 응답 가능한 상태로 유지할 수 있도록 여러 API의 비동기 버전을 제공합니다. 비동기 API를 사용하면 활성 실행 스레드가 상당한 시간 동안 차단되는 일이 없습니다. UI 스레드에서 API를 호출할 때 비동기 버전이 있다면 이 버전을 사용하세요.

## <a name="binding-is-being-used-instead-of-xbind"></a>{x:Bind} 대신 {Binding}이 사용됨

이 규칙은 앱에서 {Binding} 문을 사용하는 경우 발생합니다. 앱 성능을 향상하려면 앱에서 {x:Bind}를 사용하는 것이 좋습니다.

### <a name="impact"></a>영향

{Binding}은 {x:Bind}보다 더 많은 시간과 더 많은 메모리를 사용하여 실행됩니다.

### <a name="cause"></a>Cause(원인)

앱이 {x:Bind} 대신 {Binding}을 사용하고 있습니다. {Binding}을 사용하면 특수 작업 집합과 CPU 오버헤드가 발생합니다. {Binding}을 만들면 일련의 할당이 발생하고, 바인딩 대상을 업데이트하면 리플렉션 및 boxing이 발생할 수 있습니다.

### <a name="solution"></a>솔루션

빌드 시 바인딩을 컴파일하는 {x:Bind} 태그 확장을 사용합니다. {x:Bind} 바인딩(컴파일된 바인딩이라고도 함)은 성능이 뛰어나고, 바인딩 식에 대한 컴파일 시간 유효성 검사를 제공하며, 페이지의 partial 클래스로 생성되는 코드 파일에 중단점을 설정할 수 있도록 하여 디버깅을 지원합니다. 

x:Bind는 런타임에 바인딩된 시나리오 등 모든 경우에 적합하지는 않습니다. {x:Bind}를 적용하지 못하는 경우에 대한 전체 목록은 {x:Bind} 설명서를 참조하세요.

## <a name="xname-is-being-used-instead-of-xkey"></a>x:Key 대신 x:Name이 사용됨

ResourceDictionaries는 일반적으로 리소스, 즉 앱이 여러 위치(예: 스타일, 브러시, 템플릿)에서 참조하고자 하는 리소스를 전역 수준에서 저장하는 데 사용됩니다. 일반적으로 요청되지 않은 경우 리소스를 인스턴스화하지 않도록 ResourceDictionaries를 최적화했습니다. 그러나 약간 주의가 필요한 몇몇 위치가 있습니다.

### <a name="impact"></a>영향

x:Name이 있는 모든 리소스는 ResourceDictionary가 만들어지면 바로 인스턴스화됩니다. 이렇게 되는 이유는 x:Name에서 앱에 이 리소스에 대한 필드 액세스가 필요함을 플랫폼에 알리므로 플랫폼이 참조를 생성할 관련 항목을 만들어야 하기 때문입니다.

### <a name="cause"></a>Cause(원인)

앱이 리소스에서 x:Name을 설정하고 있습니다.

### <a name="solution"></a>솔루션

코드 숨김에서 리소스를 참조하지 않는 경우 x:Name 대신 x:Key를 사용합니다.

## <a name="collections-control-is-using-a-non-virtualizing-panel"></a>컬렉션 컨트롤이 비가상화 패널을 사용하고 있음

사용자 지정 항목 패널 템플릿(ItemsPanel 참조)을 제공하는 경우 ItemsWrapGrid 또는 ItemsStackPanel과 같은 가상화 패널을 사용해야 합니다. VariableSizedWrapGrid, WrapGrid 또는 StackPanel을 사용하는 경우에는 가상화할 수 없습니다. 또한 ItemsWrapGrid 또는 ItemsStackPanel을 사용할 경우에만 ChoosingGroupHeaderContainer, ChoosingItemContainer 및 ContainerContentChanging과 같은 ListView 이벤트가 발생됩니다.

UI 가상화는 컬렉션 성능을 개선할 수 있는 가장 중요한 기능입니다. 이는 항목을 나타내는 UI 요소가 필요에 따라 만들어짐을 의미합니다. 1000개 항목의 컬렉션에 바인딩된 항목 컨트롤의 경우 동시에 모든 항목에 대한 UI를 만드는 것은 리소스 낭비입니다. 왜냐하면 이들을 동시에 모두 표시할 수 없기 때문입니다. ListView 및 GridView(및 기타 표준 ItemsControl 파생 컨트롤)는 UI 가상화를 수행합니다. 항목이 보기에 가까이(몇 페이지 밖) 스크롤되면 프레임워크가 항목에 대한 UI를 생성하고 이를 캐시합니다. 항목이 다시 표시될 것 같지 않은 경우 프레임워크는 메모리를 회수합니다.

UI 가상화는 컬렉션 성능을 개선하는 데 몇 가지 핵심 요소 중 하나일 뿐입니다. 컬렉션 성능을 개선하기 위한 두 가지 중요한 측면은 컬렉션 항목 및 데이터 가상화의 복잡성을 줄이는 것입니다. Listview 및 Gridview 내에서 컬렉션 성능을 개선하는 방법은 [ListView 및 GridView UI 최적화](https://msdn.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview)와 [ListView 및 Gridview 데이터 가상화](https://msdn.microsoft.com/windows/uwp/debug-test-perf/listview-and-gridview-data-optimization)에 대한 문서를 참조하세요.

### <a name="impact"></a>영향

가상화되지 않은 ItemsControl은 필요 이상으로 많은 해당 자식 항목을 로드하여 로드 시간과 리소스 사용량을 증가시킵니다.

### <a name="cause"></a>Cause(원인)

가상화를 지원하지 않는 패널을 사용하고 있습니다.

### <a name="solution"></a>솔루션

ItemsWrapGrid 또는 ItemsStackPanel과 같은 가상화 패널을 사용합니다.

## <a name="accessibility-uia-elements-with-no-name"></a>접근성: 이름이 없는 UIA 요소

XAML에서 AutomationProperties.Name을 설정하여 이름을 제공할 수 있습니다. AutomationProperties.Name이 설정되어 있지 않은 경우 많은 자동화 피어가 UIA에 기본 이름을 제공합니다. 

### <a name="impact"></a>영향

사용자가 이름이 없는 요소에 도달하면 해당 요소가 무엇에 관련된 것인지 알 수 없습니다. 

### <a name="cause"></a>Cause(원인)

요소의 UIA 이름이 null이거나 비어 있습니다. 이 규칙은 AutomationProperties.Name의 값이 아니라 UIA에 표시되는 내용을 확인합니다.

### <a name="solution"></a>솔루션

컨트롤의 XAML에서 AutomationProperties.Name 속성을 적절한 지역화된 문자열로 설정합니다.

경우에 따라 올바른 응용 프로그램 수정은 이름을 제공하는 것이 아니라 UIA 요소를 원시 트리를 제외한 모든 곳에서 제거하는 것입니다. XAML에서 AutomationProperties.AccessibilityView = “Raw”를 설정하여 이 작업을 수행할 수 있습니다.

## <a name="accessibility-uia-elements-with-the-same-controltype-should-not-have-the-same-name"></a>접근성: 동일한 Controltype이 있는 UIA 요소는 이름이 동일하지 않아야 함

동일한 UIA 부모를 가진 두 개의 UIA 요소는 Name 및 ControlType이 같지 않아야 합니다. ControlType이 다른 경우 Name이 동일한 두 개의 컨트롤을 가질 수 있습니다. 

이 규칙은 부모가 다른 경우 중복된 이름을 확인하지 않습니다. 그러나 대부분의 경우 부모가 달라도 전체 창 내에서 Name과 ControlType을 복제하지 않아야 합니다. 창 내에서 중복된 이름이 허용되는 경우는 동일한 항목이 있는 두 개의 목록입니다. 이 경우 목록 항목은 Name 및 ControlType이 동일할 것으로 예상됩니다.

### <a name="impact"></a>영향

사용자가 동일한 UIA 부모를 가진 다른 요소로서 Name 및 ControlType이 동일한 요소에 도달하면 해당 사용자는 요소 간에 차이점을 구분할 수가 없을 수 있습니다.

### <a name="cause"></a>Cause(원인)

UIA 부모가 동일한 UIA 요소는 Name 및 ControlType이 동일합니다.

### <a name="solution"></a>솔루션

AutomationProperties.Name을 사용하여 XAML에서 이름을 설정합니다. 이런 작업이 일반적으로 발생하는 목록에서는 바인딩을 사용하여 AutomationProperties.Name의 값을 데이터 원본에 바인딩합니다.


