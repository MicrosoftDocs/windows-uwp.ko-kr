---
author: Jwmsft
ms.assetid: 54CC0BD4-1961-44D7-AB40-6E8B58E42D65
title: 셰이프 그리기
description: 타원, 사각형, 다각형, 패스 같은 다양한 셰이프를 그리는 방법을 알아봅니다. Path 클래스를 사용하면 XAML UI에서 매우 복잡한 벡터 기반 그리기 언어를 시각화할 수 있습니다. 예를 들어 베지어 곡선을 그릴 수 있습니다.
ms.author: jimwalk
ms.date: 11/16/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 984653ad20fc40035528ab7e32b904e64d6ff8c5
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5695977"
---
# <a name="draw-shapes"></a>셰이프 그리기

타원, 사각형, 다각형, 패스 같은 다양한 셰이프를 그리는 방법에 대해 알아봅니다. [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) 클래스를 사용하면 XAML UI에서 매우 복잡한 벡터 기반 그리기 언어를 시각화할 수 있습니다. 예를 들어 베지어 곡선을 그릴 수 있습니다.

> **중요 API**: [경로 클래스](/uwp/api/Windows.UI.Xaml.Shapes.Path), [Windows.UI.Xaml.Shapes 네임스페이스](/uwp/api/Windows.UI.Xaml.Shapes), [Windows.UI.Xaml.Media 네임스페이스](/uwp/api/Windows.UI.Xaml.Media)


두 개의 클래스 집합([**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) 클래스 및 [**Geometry**](/uwp/api/Windows.UI.Xaml.Media.Geometry) 클래스)이 XAML UI의 공간 영역을 정의합니다. 이 두 클래스의 주요 차이점은 **Shape**에는 연결된 브러시가 있어 화면으로 렌더링될 수 있고, **Geometry**는 또 다른 UI 속성에 정보를 제공하는 데 도움이 되지 않는 경우 단순히 공간 영역만 정의하고 렌더링되지는 않는다는 점입니다. **Shape**를 **Geometry**에 의해 정의된 경계를 가진 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911)로 생각할 수 있습니다. 이 항목에서는 주로 **Shape** 클래스에 대해 설명합니다.

[**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) 클래스는 [**Line**](/uwp/api/Windows.UI.Xaml.Shapes.Line), [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse), [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle), [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon), [**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline) 및 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path)입니다. **Path**는 임의의 기하 도형을 정의할 수 있기 때문에 흥미롭고, [**Geometry**](/uwp/api/Windows.UI.Xaml.Media.Geometry) 클래스는 **Path**의 일부를 정의하는 한 가지 방법이기 때문에 여기에 포함됩니다.

## <a name="fill-and-stroke-for-shapes"></a>셰이프 채우기 및 스트로크

[**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape)를 앱 캔버스에 렌더링하려면 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush)를 연결해야 합니다. **Shape**의 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) 속성을 원하는 **Brush**로 설정합니다. 브러시에 대한 자세한 내용은 [브러시 사용](../style/brushes.md)을 참조하세요.

[**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape)에는 셰이프 경계 주변에 그려진 선인 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke)도 있을 수 있습니다. **Stroke**에도 모양을 정의하는 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush)가 필요하며 0이 아닌 [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness) 값이 있어야 합니다. **StrokeThickness**는 셰이프 가장자리 주변의 경계 두께를 정의하는 속성입니다. **Stroke**에 대해 **Brush** 값을 지정하지 않거나 **StrokeThickness**를 0으로 설정하면 셰이프 주변 경계가 그려지지 않습니다.

## <a name="ellipse"></a>타원

[**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse)은 경계가 곡선인 셰이프입니다. 기본 **Ellipse**를 만들려면 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)에 대해 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width), [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 및 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush)를 지정합니다.

다음 예제에서는 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)가 200이고 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height)가 200인 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse)를 만들고, [**SteelBlue**](https://msdn.microsoft.com/library/windows/apps/Hh748056) 색의 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)를 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)로 사용합니다.

```xaml
<Ellipse Fill="SteelBlue" Height="200" Width="200" />
```

```csharp
var ellipse1 = new Ellipse();
ellipse1.Fill = new SolidColorBrush(Windows.UI.Colors.SteelBlue);
ellipse1.Width = 200;
ellipse1.Height = 200;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(ellipse1);
```

다음은 렌더링된 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse)입니다.

![렌더링된 타원](images/shapes-ellipse.jpg)

이 경우의 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse)는 대부분의 사람들이 원이라고 생각하는 모양이지만, XAML에서 원 셰이프를 선언하는 방식으로 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 및 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height)가 같은 **Ellipse**를 사용합니다.

UI 레이아웃에 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse)을 배치할 때 그 크기는 해당 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 및 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height)의 사각형과 같다고 가정됩니다. 경계 외부의 영역에는 렌더링이 없지만 레이아웃 슬롯 크기에 포함됩니다.

6개의 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) 요소 집합은 [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/BR227538) 컨트롤에 대한 컨트롤 템플릿의 요소이며, 2개의 동심원 **Ellipse** 요소는 [**RadioButton**](https://msdn.microsoft.com/library/windows/apps/BR227544)의 일부입니다.

## <a name="span-idrectanglespanspan-idrectanglespanspan-idrectanglespanrectangle"></a><span id="Rectangle"></span><span id="rectangle"></span><span id="RECTANGLE"></span>사각형

[**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)은 대변이 동일한 4개의 변을 가진 셰이프입니다. 기본 **Rectangle**을 만들려면 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width), [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 및 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)을 지정합니다.

[**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)의 모서리를 둥글게 만들 수 있습니다. 모서리를 둥글게 만들려면 [**RadiusX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusx.aspx) 및 [**RadiusY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusy) 속성에 대해 값을 지정합니다. 이 두 속성은 모서리의 곡선을 정의하는 타원의 x-축과 y-축을 지정합니다. **RadiusX**의 최대값은 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)를 2로 나눈 값이고 **RadiusY**의 최대값은 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height)를 2로 나눈 값입니다.

다음 예제에서는 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)가 200이고 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height)가 100인 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)을 만듭니다. [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)에 대해서는 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)의 [**Blue**](https://msdn.microsoft.com/library/windows/apps/Hh747837) 값을 사용하고 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke)에 대해서는 **SolidColorBrush**의 [**Black**](https://msdn.microsoft.com/library/windows/apps/Hh747833) 값을 사용합니다. 여기에서는 [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness)를 3으로 설정합니다. [**RadiusX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusx.aspx) 속성을 50으로 설정하고 [**RadiusY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusy) 속성을 10으로 설정하여 **Rectangle**의 모서리를 둥글게 만듭니다.

```xaml
<Rectangle Fill="Blue"
           Width="200"
           Height="100"
           Stroke="Black"
           StrokeThickness="3"
           RadiusX="50"
           RadiusY="10" />
```

```csharp
var rectangle1 = new Rectangle();
rectangle1.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
rectangle1.Width = 200;
rectangle1.Height = 100;
rectangle1.Stroke = new SolidColorBrush(Windows.UI.Colors.Black);
rectangle1.StrokeThickness = 3;
rectangle1.RadiusX = 50;
rectangle1.RadiusY = 10;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(rectangle1);
```

다음은 렌더링된 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)입니다.

![렌더링된 사각형](images/shapes-rectangle.jpg)

**팁**시나리오도 있습니다 UI 정의에 있는 [**사각형**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)을 사용 하는 대신 [**테두리**](https://msdn.microsoft.com/library/windows/apps/BR209250) 를 더 적절할 수 있습니다. 다른 콘텐츠 주위에 사각형 셰이프를 만들려는 경우 **Border**를 사용하는 것이 더 나을 수 있습니다. 그러면 자식 콘텐츠를 사용할 수 있고 **Rectangle**처럼 높이와 너비에 고정 치수를 사용하는 대신 해당 콘텐츠를 둘러싸도록 크기가 자동으로 지정됩니다. **Border**에는 [**CornerRadius**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.border.cornerradius) 속성을 설정할 경우 모서리를 둥글게 하는 옵션도 있습니다.

반면에 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)은 컨트롤 컴퍼지션에 더 적합한 선택일 수 있습니다. **Rectangle** 셰이프는 포커스 가능 컨트롤의 "FocusVisual" 부분을 사용되므로 많은 컨트롤 템플릿에 표시됩니다. 이 직사각형은 컨트롤이 "Focused" 시각적 상태일 때마다 표시되며 다른 상태에서는 숨겨집니다.

## <a name="polygon"></a>다각형

[**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon)은 임의 개수의 점으로 정의된 경계가 있는 셰이프입니다. 한 점에서 다음 점으로 선을 연결하고 마지막 점이 첫 번째 점으로 연결되어 경계가 만들어집니다. [**Points**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polygon.points.aspx) 속성은 경계를 구성하는 점의 컬렉션을 정의합니다. XAML에서는 쉼표로 구분된 목록으로 점을 정의합니다. 코드 숨김에서는 [**PointCollection**](https://msdn.microsoft.com/library/windows/apps/BR210220)을 사용하여 점을 정의하고 각 개별 점을 [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 값으로 컬렉션에 추가합니다.

시작점과 끝점이 모두 동일한 [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 값으로 지정되도록 점을 명시적으로 선언할 필요는 없습니다. [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon)의 렌더링 논리에서 닫힌 셰이프를 정의한다고 가정하여 끝점을 시작점에 암시적으로 연결합니다.

다음 예제에서는 `(10,200)`, `(60,140)`, `(130,140)` 및 `(180,200)`으로 설정된 4개의 점을 가진 [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon) 을 만듭니다. [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)에 대해서는 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)의 [**LightBlue**](https://msdn.microsoft.com/library/windows/apps/Hh747960) 값을 사용하고 경계 윤곽이 없도록 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke)의 값은 없습니다.

```xaml
<Polygon Fill="LightBlue"
         Points="10,200,60,140,130,140,180,200" />
```

```csharp
var polygon1 = new Polygon();
polygon1.Fill = new SolidColorBrush(Windows.UI.Colors.LightBlue);

var points = new PointCollection();
points.Add(new Windows.Foundation.Point(10, 200));
points.Add(new Windows.Foundation.Point(60, 140));
points.Add(new Windows.Foundation.Point(130, 140));
points.Add(new Windows.Foundation.Point(180, 200));
polygon1.Points = points;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(polygon1);
```

다음은 렌더링된 [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon)입니다.

![렌더링된 다각형](images/shapes-polygon.jpg)

**팁** [**지점**](https://msdn.microsoft.com/library/windows/apps/BR225870) 값은 종종 셰이프의 정점 선언이 아닌 다른 시나리오에 대 한 XAML에서 한 형식으로 사용 됩니다. 예를 들어 **Point**는 터치 이벤트에 대한 이벤트 데이터의 일부이므로 좌표 공간에서 터치 작업이 발생한 위치를 정확하게 알 수 있습니다. **Point** 및 XAML 또는 코드에서 사용하는 방법에 대한 자세한 내용은 [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870)에 대한 API 참조 항목을 참조하세요.

## <a name="line"></a>선

[**Line**](/uwp/api/Windows.UI.Xaml.Shapes.Line)은 좌표 공간에서 두 점 사이에 그려진 선입니다. **Line**은 내부 공간이 없기 때문에 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)에 제공된 값을 모두 무시합니다. **Line**에 대해서는 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) 및 [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness) 속성의 값을 지정해야 하며, 그러지 않으면 **Line**이 렌더링되지 않습니다.

[**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 값을 사용하여 [**Line**](/uwp/api/Windows.UI.Xaml.Shapes.Line) 셰이프를 지정하지는 않으며 [**X1**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.x1.aspx), [**Y1**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.y1.aspx), [**X2**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.x2.aspx) 및 [**Y2**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.y2.aspx)에 대해 불연속 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) 값을 사용합니다. 이렇게 하면 가로줄 또는 세로줄의 표시가 최소화됩니다. 예를 들어 `<Line Stroke="Red" X2="400"/>`은 400픽셀 길이의 가로줄을 정의합니다. 다른 X,Y 속성은 기본적으로 0이므로 점과 관련하여 이 XAML은 `(0,0)`에서 `(400,0)`까지 선을 그립니다. 그런 다음 (0,0)이 아닌 다른 점에서 시작하도록 하려면 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027)을 사용하여 전체 **Line**을 이동할 수 있습니다.

```xaml
<Line Stroke="Red" X2="400"/>
```

```csharp
var line1 = new Line();
line1.Stroke = new SolidColorBrush(Windows.UI.Colors.Red);
line1.X2 = 400;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(line1);
```

## <a name="span-idpolylinespanspan-idpolylinespanspan-idpolylinespan-polyline"></a><span id="_Polyline"></span><span id="_polyline"></span><span id="_POLYLINE"></span> 폴리라인

[**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline)은 **Polyline**의 마지막 점이 첫 번째 점에 연결되지 않는다는 것을 제외하고, 점 집합에 의해 셰이프의 경계가 정의된다는 점에서 [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon)과 유사합니다.

**참고**  동일한 시작 지점을 명시적으로 가지기 및 [**포인트**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx) 끝점 설정 [**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline)있지만 경우 아마도 수를 사용한 [**다각형**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon) 대신 합니다.

[**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline)의 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)을 지정하면 **Polyline**에 설정된 [**Points**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx)의 시작점과 끝점이 교차하지 않아도 **Fill**이 셰이프의 내부 공간을 그립니다. **Fill**을 지정하지 않으면 **Polyline**은 연속하는 선의 시작점과 끝점이 교차하는 개별 [**Line**](/uwp/api/Windows.UI.Xaml.Shapes.Line) 요소를 여러 개 지정한 경우에 렌더링되는 모양과 유사합니다.

[**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon)에서와 마찬가지로 [**Points**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx) 속성은 경계를 구성하는 점의 컬렉션을 정의합니다. XAML에서는 쉼표로 구분된 목록으로 점을 정의합니다. 코드 숨김에서는 [**PointCollection**](https://msdn.microsoft.com/library/windows/apps/BR210220)을 사용하여 점을 정의하고 각 개별 점을 [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 구조로 컬렉션에 추가합니다.

다음 예제에서는 `(10,200)`, `(60,140)`, `(130,140)` 및 `(180,200)`으로 설정된 4개의 점을 가진 [**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline)을 만듭니다. [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke)도 정의되지만 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)은 정의되지 않습니다.

```xaml
<Polyline Stroke="Black"
        StrokeThickness="4"
        Points="10,200,60,140,130,140,180,200" />
```

```csharp
var polyline1 = new Polyline();
polyline1.Stroke = new SolidColorBrush(Windows.UI.Colors.Black);
polyline1.StrokeThickness = 4;

var points = new PointCollection();
points.Add(new Windows.Foundation.Point(10, 200));
points.Add(new Windows.Foundation.Point(60, 140));
points.Add(new Windows.Foundation.Point(130, 140));
points.Add(new Windows.Foundation.Point(180, 200));
polyline1.Points = points;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(polyline1);
```

다음은 렌더링된 [**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline)입니다. 첫 번째 점과 마지막 점이 [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon)처럼 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) 윤곽선으로 연결되지 않는다는 점에 유의하시기 바랍니다.

![렌더링된 폴리라인](images/shapes-polyline.jpg)

## <a name="path"></a>패스

[**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path)는 임의의 기하 도형을 정의하는 데 사용할 수 있기 때문에 가장 유용한 [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape)입니다. 하지만 유용성에는 복잡성이 따릅니다. 이제 XAML에서 기본 **Path**를 만드는 방법을 살펴보겠습니다.

[**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 속성으로 패스의 기하 도형을 정의합니다. **Data**를 설정하는 다음 두 가지 기술이 있습니다.

- XAML에서 [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data)의 문자열 값을 설정할 수 있습니다. 이 형식에서 **Path.Data** 값은 그래픽에 직렬화 형식을 사용하고 있습니다. 일반적으로 처음 설정된 후에는 이 값을 문자열 형식으로 텍스트 편집하지 않습니다. 대신, 화면에서 디자인 또는 그리기 방식으로 작업할 수 있게 하는 디자인 도구를 사용합니다. 출력을 저장하거나 내보내면 **Path.Data** 정보가 포함된 XAML 파일 또는 XAML 문자열이 작성됩니다.
- [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 속성은 단일 [**Geometry**](/uwp/api/Windows.UI.Xaml.Media.Geometry) 개체로 설정할 수 있습니다. 이 작업은 코드나 XAML에서 수행할 수 있습니다. 이 단일 **Geometry**는 일반적으로 개체 모델을 위해 여러 기하 도형 정의를 단일 개체로 합성할 수 있는 컨테이너 역할을 하는 [**GeometryGroup**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.geometrygroup)입니다. 이 작업의 가장 일반적인 이유는 [**PathFigure**](https://msdn.microsoft.com/library/windows/apps/BR210143)의 [**Segments**](https://msdn.microsoft.com/library/windows/apps/BR210164) 값으로 정의할 수 있는 곡선 및 복합 셰이프(예: [**BezierSegment**](https://msdn.microsoft.com/library/windows/apps/BR228068))를 하나 이상 사용하기 위해서입니다.

다음 예제에서는 Blend for Visual Studio를 사용하여 몇 개의 벡터 셰이프만 작성한 다음 결과를 XAML로 저장할 경우 생성될 수 있는 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path)를 보여 줍니다. 전체 **Path**는 베지어 곡선 세그먼트와 직선 세그먼트로 구성됩니다. 다음 예제에서는 [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 직렬화 형식에 있는 요소의 몇 가지 예를 제공하고 숫자가 나타내는 의미를 설명합니다.

이 [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data)는 "M"으로 표시되는 이동 명령으로 시작하며 여기서 패스의 절대 시작점이 설정됩니다.

첫 번째 세그먼트는 `(100,200)`에서 시작하여 `(400,175)`에서 끝나는 입방형 3차원 곡선이며, 두 개의 제어점 `(100,25)` 및 `(400,350)`을 사용하여 그려집니다. 이 세그먼트는 [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 특성 문자열에서 "C" 명령으로 표시됩니다.

두 번째 세그먼트는 절대 가로줄 명령 "H"에서 시작하여 이전 하위 경로의 끝점 `(400,175)`에서 새로운 끝점 `(280,175)`로 그려지는 선을 지정합니다. 이 명령은 가로줄 명령이므로 지정되는 값은 x 좌표입니다.

```xaml
<Path Stroke="DarkGoldenRod" 
      StrokeThickness="3"
      Data="M 100,200 C 100,25 400,350 400,175 H 280" />
```

다음은 렌더링된 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path)입니다.

![렌더링된 경로](images/shapes-path.jpg)

다음 예제에서는 이미 설명한 다른 기술 즉, [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/BR210168)가 포함된 [**GeometryGroup**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.geometrygroup)의 사용법을 보여 줍니다. 이 예제에서는 **PathGeometry**: [**PathFigure**](https://msdn.microsoft.com/library/windows/apps/BR210143)의 일부로 사용할 수 있는 영향을 주는 기하 도형 형식 일부와 [**PathFigure.Segments**](https://msdn.microsoft.com/library/windows/apps/BR210164)에서 세그먼트가 될 수 있는 다양한 요소를 실행합니다.

```xaml
<Path Stroke="Black" StrokeThickness="1" Fill="#CCCCFF">
    <Path.Data>
        <GeometryGroup>
            <RectangleGeometry Rect="50,5 100,10" />
            <RectangleGeometry Rect="5,5 95,180" />
            <EllipseGeometry Center="100, 100" RadiusX="20" RadiusY="30"/>
            <RectangleGeometry Rect="50,175 100,10" />
            <PathGeometry>
                <PathGeometry.Figures>
                    <PathFigureCollection>
                        <PathFigure IsClosed="true" StartPoint="50,50">
                            <PathFigure.Segments>
                                <PathSegmentCollection>
                                    <BezierSegment Point1="75,300" Point2="125,100" Point3="150,50"/>
                                    <BezierSegment Point1="125,300" Point2="75,100"  Point3="50,50"/>
                                </PathSegmentCollection>
                            </PathFigure.Segments>
                        </PathFigure>
                    </PathFigureCollection>
                </PathGeometry.Figures>
            </PathGeometry>
        </GeometryGroup>
    </Path.Data>
</Path>
```

```csharp
var path1 = new Windows.UI.Xaml.Shapes.Path();
path1.Fill = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 204, 204, 255));
path1.Stroke = new SolidColorBrush(Windows.UI.Colors.Black);
path1.StrokeThickness = 1;

var geometryGroup1 = new GeometryGroup();
var rectangleGeometry1 = new RectangleGeometry();
rectangleGeometry1.Rect = new Rect(50, 5, 100, 10);
var rectangleGeometry2 = new RectangleGeometry();
rectangleGeometry2.Rect = new Rect(5, 5, 95, 180);
geometryGroup1.Children.Add(rectangleGeometry1);
geometryGroup1.Children.Add(rectangleGeometry2);

var ellipseGeometry1 = new EllipseGeometry();
ellipseGeometry1.Center = new Point(100, 100);
ellipseGeometry1.RadiusX = 20;
ellipseGeometry1.RadiusY = 30;
geometryGroup1.Children.Add(ellipseGeometry1);

var pathGeometry1 = new PathGeometry();
var pathFigureCollection1 = new PathFigureCollection();
var pathFigure1 = new PathFigure();
pathFigure1.IsClosed = true;
pathFigure1.StartPoint = new Windows.Foundation.Point(50, 50);
pathFigureCollection1.Add(pathFigure1);
pathGeometry1.Figures = pathFigureCollection1;

var pathSegmentCollection1 = new PathSegmentCollection();
var pathSegment1 = new BezierSegment();
pathSegment1.Point1 = new Point(75, 300);
pathSegment1.Point2 = new Point(125, 100);
pathSegment1.Point3 = new Point(150, 50);
pathSegmentCollection1.Add(pathSegment1);

var pathSegment2 = new BezierSegment();
pathSegment2.Point1 = new Point(125, 300);
pathSegment2.Point2 = new Point(75, 100);
pathSegment2.Point3 = new Point(50, 50);
pathSegmentCollection1.Add(pathSegment2);
pathFigure1.Segments = pathSegmentCollection1;

geometryGroup1.Children.Add(pathGeometry1);
path1.Data = geometryGroup1;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(path1);
```

다음은 렌더링된 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path)입니다.

![렌더링된 경로](images/shapes-path-2.png)

[**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/BR210168)를 사용하면 [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 문자열을 채우는 것보다 읽기 쉬울 수 있습니다. 반면에 [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data)는 SVG(스케일러블 벡터 그래픽) 이미지 경로 정의와 호환되는 구문을 사용하므로 SVG에서 그래픽을 이식하거나 Blend와 같은 도구에서 출력으로 사용하는 데 유용할 수 있습니다.
