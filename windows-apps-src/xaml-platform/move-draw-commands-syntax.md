---
description: 경로 기 하 도형을 XAML 특성 값으로 지정 하는 데 사용할 수 있는 이동 및 그리기 명령 (미니 언어)에 대해 알아봅니다.
title: 이동 및 그리기 명령 구문
ms.assetid: 7772BC3E-A631-46FF-9940-3DD5B9D0E0D9
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a5942dfbcd2f72d456ac352785bce73e75454330
ms.sourcegitcommit: aa88679989ef3c8b726e1bf5a0ed17c1206a414f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92687779"
---
# <a name="move-and-draw-commands-syntax"></a>이동 및 그리기 명령 구문


경로 기 하 도형을 XAML 특성 값으로 지정 하는 데 사용할 수 있는 이동 및 그리기 명령 (미니 언어)에 대해 알아봅니다. 이동 및 그리기 명령은 벡터 그래픽 이나 모양을 직렬화 및 교환 형식으로 출력할 수 있는 많은 디자인 및 그래픽 도구에 사용 됩니다.

## <a name="properties-that-use-move-and-draw-command-strings"></a>명령 문자열 이동 및 그리기를 사용 하는 속성

이동 및 그리기 명령 구문은 XAML의 내부 형식 변환기에서 지원 됩니다 .이 변환기는 명령을 구문 분석 하 고 런타임 그래픽 표현을 생성 합니다. 이 표현은 기본적으로 표시할 수 있는 완성 된 벡터 집합입니다. 벡터 자체는 프레젠테이션 정보를 완료 하지 않습니다. 여전히 요소에 다른 값을 설정 해야 합니다. [**경로**](/uwp/api/Windows.UI.Xaml.Shapes.Path) 개체의 경우에는 [**채우기**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill), [**스트로크**](/uwp/api/windows.ui.xaml.shapes.shape.stroke)및 기타 속성에 대 한 값도 필요 합니다. 그런 다음 해당 **경로** 를 시각적 트리에 연결 해야 합니다. [**Pathicon**](/uwp/api/Windows.UI.Xaml.Controls.PathIcon) 개체에 대해 [**전경**](/uwp/api/windows.ui.xaml.controls.iconelement.foreground) 속성을 설정 합니다.

Windows 런타임에는 move 및 draw 명령을 나타내는 문자열을 사용할 수 있는 두 가지 속성, 즉 [**Path. data**](/uwp/api/windows.ui.xaml.shapes.path.data) 및 [**pathicon 데이터가**](/uwp/api/windows.ui.xaml.controls.pathicon.data)있습니다. 이동 및 그리기 명령을 지정 하 여 이러한 속성 중 하나를 설정 하는 경우 일반적으로 해당 요소의 다른 필수 특성과 함께 XAML 특성 값으로 설정 합니다. 구체적으로 설명 하지 않고 다음과 같이 표시 됩니다.

```xml
<Path x:Name="Arrow" Fill="White" Height="11" Width="9.67"
  Data="M4.12,0 L9.67,5.47 L4.12,10.94 L0,10.88 L5.56,5.47 L0,0.06" />
```

[**Pathgeometry. 그림**](/uwp/api/windows.ui.xaml.media.pathgeometry.figures) 은 이동 및 그리기 명령을 사용할 수도 있습니다. 이동 및 그리기 명령을 사용 하는 [**Pathgeometry**](/uwp/api/Windows.UI.Xaml.Media.PathGeometry) 개체를 [**GeometryGroup**](/uwp/api/Windows.UI.Xaml.Media.GeometryGroup) 개체의 다른 [**Geometry**](/uwp/api/Windows.UI.Xaml.Media.Geometry) 형식과 결합 하 여 [**Path. Data**](/uwp/api/windows.ui.xaml.shapes.path.data)에 대 한 값으로 사용할 수 있습니다. 그러나 특성 정의 데이터에 대 한 이동 및 그리기 명령을 사용 하는 것 만큼 일반적이 지 않습니다.

## <a name="using-move-and-draw-commands-versus-using-a-pathgeometry"></a>이동 및 그리기 명령과 **Pathgeometry** 사용 비교

Windows 런타임 XAML의 경우 이동 및 그리기 명령은 [**그림**](/uwp/api/windows.ui.xaml.media.pathgeometry.figures) 속성 값을 포함 하는 단일 [**pathgeometry**](/uwp/api/Windows.UI.Xaml.Media.PathFigure) 개체를 사용 하 여 [**pathgeometry**](/uwp/api/Windows.UI.Xaml.Media.PathGeometry) 를 생성 합니다. 각 그리기 명령은 해당 단일 **Pathsegment** 의 [**세그먼트**](/uwp/api/windows.ui.xaml.media.pathfigure.segments) 컬렉션에서 [**pathsegment**](/uwp/api/Windows.UI.Xaml.Media.PathSegment) 파생 클래스를 생성 하 고, move [**명령을 통해 시작점을 변경**](/uwp/api/windows.ui.xaml.media.pathfigure.startpoint)하 고, Close 명령 집합의 존재를 **true** 로 [**IsClosed**](/uwp/api/windows.ui.xaml.media.pathfigure.isclosed) 합니다. 런타임에 **데이터** 값을 검사 하는 경우이 구조를 개체 모델로 탐색할 수 있습니다.

## <a name="the-basic-syntax"></a>기본 구문

이동 및 그리기 명령의 구문은 다음과 같이 요약할 수 있습니다.

1.  선택적 채우기 규칙으로 시작 합니다. 일반적으로 **EvenOdd** 기본값을 원하지 않는 경우에만이를 지정 합니다. (나중에 **EvenOdd** 에 대해 자세히 알아봅니다.)
2.  정확히 하나의 move 명령을 지정 합니다.
3.  그리기 명령을 하나 이상 지정 합니다.
4.  닫기 명령을 지정 합니다. 닫기 명령을 생략할 수 있지만이 경우에는 그림을 열어 둡니다 (일반적이 지 않음).

이 구문의 일반적인 규칙은 다음과 같습니다.

-   각 명령은 정확히 한 문자로 표시 됩니다.
-   이 문자는 대문자나 소문자 일 수 있습니다. 대/소문자를 구분 합니다.
-   닫기 명령을 제외한 각 명령에는 일반적으로 하나 이상의 숫자가 붙습니다.
-   명령에 여러 개의 숫자가 있으면 쉼표 또는 공백으로 구분 합니다.

**\[**_Fillrule_ **\]** _moveCommand_ _drawcommand_ **\[** _drawcommand_ **\*\]** **\[** _closeCommand_**\]**

대부분의 그리기 명령에는 _x, y_ 값을 제공 하는 점이 사용 됩니다. \* _점_ 자리 표시 자가 표시 될 때마다 점의 _x, y_ 값에 대해 두 개의 10 진수 값을 제공 하는 것으로 간주할 수 있습니다.

결과가 모호한 경우 공백을 생략 하는 경우가 많습니다. 모든 숫자 집합 (점 및 크기)에 대 한 구분 기호로 쉼표를 사용 하는 경우에는 실제로 모든 공백을 생략할 수 있습니다. 예를 들어,이 사용법은 올바릅니다 `F1M0,58L2,56L6,60L13,51L15,53L6,64z` . 그러나 명확 하 게 하기 위해 명령 사이에 공백을 포함 하는 것이 더 일반적입니다.

10 진수에 대 한 소수점으로 쉼표를 사용 하지 않습니다. 명령 문자열은 XAML에서 해석 되며 **en-us** 로캘에서 사용 되는 것과 다른 문화권별 숫자 서식 지정 규칙을 고려 하지 않습니다.

## <a name="syntax-specifics"></a>구문 세부 사항

**규칙 채우기**

선택적 채우기 규칙에는 두 가지 가능한 값 ( **F0** 또는 **F1** )이 있습니다. **F** 는 항상 대문자입니다. 기본값은 **F0** 입니다. **EvenOdd** 채우기 동작을 생성 하므로 일반적으로 지정 하지 않습니다. **F1 키** 를 사용 하 여 **0이 아닌** 채우기 동작을 가져옵니다. 이러한 채우기 값은 [**Fillrule**](/uwp/api/Windows.UI.Xaml.Media.FillRule) 열거형의 값에 맞춰집니다.

**명령 이동**

새 그림의 시작점을 지정합니다.

| 구문 |
|--------|
| `M `_startPoint_ 시작점 <br/>-또는-<br/>`m`_startPoint_ 시작점|

| 용어 | Description |
|------|-------------|
| _점을_ | [**Point**](/uwp/api/Windows.Foundation.Point) <br/>새 그림의 시작점입니다.|

대문자 **M** *은 시작점이 절대* 좌표 임을 나타냅니다. 소문자 **m** 은 이동 점이 이전 지점에 대 한 오프셋 이거나 이전 점이 없는 경우 (0, 0 *) 임을 나타냅니다* .

**참고**  Move 명령 뒤에 여러 요소를 지정할 수 있습니다. 줄은 줄 명령을 지정한 것 처럼 해당 점에 그려집니다. 그러나 권장 되는 스타일이 아닙니다. 대신 전용 줄 명령을 사용 합니다.

**그리기 명령**

그리기 명령은 선, 가로 선, 세로선, 입방 형 3 차원 곡선, 정방형 3 차원 곡선, 부드러운 입방 형 3 차원 곡선, 부드러운 정방형 3 차원 곡선 및 타원형 원호의 여러 셰이프 명령으로 구성 될 수 있습니다.

모든 그리기 명령의 경우 대/소문자가 중요 합니다. 대문자는 절대 좌표와 소문자를 나타내는 문자는 이전 명령에 상대적인 좌표를 나타냅니다.

세그먼트의 제어점은 선행 세그먼트의 끝점을 기준으로 합니다. 동일한 유형의 명령을 두 번 이상 입력 하는 경우 중복 명령 항목을 생략할 수 있습니다. 예를 들어 `L 100,200 300,400`는 `L 100,200 L 300,400`와 같습니다.

**줄 명령**

현재 점과 지정된 끝점 간에 직선을 만듭니다. `l 20 30` 및 `L 20,30` 은 올바른 줄 명령의 예입니다. [**Linegeometry**](/uwp/api/Windows.UI.Xaml.Media.LineGeometry) 개체에 해당 하는를 정의 합니다.

| 구문 |
|--------|
| `L`_끝점_ <br/>-또는-<br/>`l`_끝점_ |

| 용어 | Description |
|------|-------------|
| 끝점만 | [**Point**](/uwp/api/Windows.Foundation.Point)<br/>선의 끝점입니다.|

**가로선 명령**

현재 점과 지정된 x 좌표 간에 수평선을 만듭니다. `H 90`은 유효한 수평선 명령의 예입니다.

| 구문 |
|--------|
| `H ` _x_ <br/> -또는- <br/>`h ` _x_ |

| 용어 | Description |
|------|-------------|
| x | [**double**](/dotnet/api/system.double) <br/> 선 끝점의 x 좌표입니다. |

**세로줄 명령**

현재 점과 지정된 y 좌표 간에 수직선을 만듭니다. `v 90`은 유효한 수직선 명령의 예입니다.

| 구문 |
|--------|
| `V `_y_ <br/> -또는- <br/> `v `_y_ |

| 용어 | Description |
|------|-------------|
| *y* | [**double**](/dotnet/api/system.double) <br/> 선 끝점의 y 좌표입니다. |

**입방 형 3 차원 곡선 명령**

지정 된 두 제어점 ( *controlPoint1* 및 *controlPoint2* )을 사용 하 여 현재 점과 지정 된 끝점 간에 입방 형 3 차원 곡선을 만듭니다. `C 100,200 200,400 300,200`은 유효한 곡선 명령의 예입니다. [**System.windows.media.beziersegment>**](/uwp/api/Windows.UI.Xaml.Media.BezierSegment) 개체를 사용 하 여 [**pathgeometry**](/uwp/api/Windows.UI.Xaml.Media.PathGeometry) 개체에 해당 하는를 정의 합니다.

| 구문 |
|--------|
| `C `*controlPoint1* *controlPoint2* *끝점* <br/> -또는- <br/> `c `*controlPoint1* *controlPoint2* *끝점* |

| 용어 | Description |
|------|-------------|
| *controlPoint1* | [**Point**](/uwp/api/Windows.Foundation.Point) <br/> 곡선의 시작 접선을 결정하는 곡선의 첫 번째 제어점입니다. |
| *controlPoint2* | [**Point**](/uwp/api/Windows.Foundation.Point) <br/> 곡선의 끝 접선을 결정하는 곡선의 두 번째 제어점입니다. |
| *끝점만* | [**Point**](/uwp/api/Windows.Foundation.Point) <br/> 곡선을 그릴 지점입니다. | 

**정방형 3차원 곡선 명령**

지정 된 제어점 ( *controlPoint* )을 사용 하 여 현재 지점과 지정 된 끝점 사이에 정방형 3 차원 곡선을 만듭니다. `q 100,200 300,200` 는 유효한 정방형 3 차원 곡선 명령의 예입니다. [**QuadraticBezierSegment**](/uwp/api/Windows.UI.Xaml.Media.QuadraticBezierSegment)를 사용 하 여 [**pathgeometry**](/uwp/api/Windows.UI.Xaml.Media.PathGeometry) 에 해당 하는를 정의 합니다.

| 구문 |
|--------|
| `Q `*ControlPoint 끝점* <br/> -또는- <br/> `q `*ControlPoint 끝점* |

| 용어 | Description |
|------|-------------|
| *controlPoint* | [**Point**](/uwp/api/Windows.Foundation.Point) <br/> 곡선의 시작 및 끝 접선을 결정하는 곡선의 제어점입니다. |
| *끝점만* | [**Point**](/uwp/api/Windows.Foundation.Point)<br/> 곡선을 그릴 지점입니다. |

**부드러운 입방 형 3 차원 곡선 명령**

현재 지점과 지정 된 끝점 간에 입방 형 3 차원 곡선을 만듭니다. 첫 번째 제어점은 현재 점을 기준으로 이전 명령의 두 번째 제어점의 리플렉션으로 간주됩니다. 이전 명령이 없거나 이전 명령이 입방 형 3 차원 곡선 명령 또는 부드러운 입방 형 3 차원 곡선 명령이 아닌 경우 첫 번째 제어점이 현재 지점과 일치 하는 것으로 가정 합니다. 두 번째 제어점, 즉 곡선 끝에 대한 제어점은 *controlPoint2* 에 의해 지정됩니다. 예를 들어 `S 100,200 200,300` 은 유효한 부드러운 입방 형 3 차원 곡선 명령입니다. 이 명령은 앞에 곡선 세그먼트가 있었던 [**system.windows.media.beziersegment>**](/uwp/api/Windows.UI.Xaml.Media.BezierSegment) 를 사용 하 여 [**pathgeometry**](/uwp/api/Windows.UI.Xaml.Media.PathGeometry) 에 해당 하는를 정의 합니다.

| 구문 |
|--------|
| `S`*controlPoint2* *끝점* <br/> -또는- <br/>`s`*ControlPoint2 끝점* |

| 용어 | Description |
|------|-------------|
| *controlPoint2* | [**Point**](/uwp/api/Windows.Foundation.Point) <br/> 곡선의 끝 접선을 결정하는 곡선의 제어점입니다. |
| *끝점만* | [**Point**](/uwp/api/Windows.Foundation.Point)<br/> 곡선을 그릴 지점입니다. |

**부드러운 정방형 3 차원 곡선 명령**

현재 지점과 지정 된 끝점 사이에 정방형 3 차원 곡선을 만듭니다. 제어점은 현재 점을 기준으로 이전 명령의 제어점의 리플렉션으로 간주됩니다. 이전 명령이 없거나 이전 명령이 정방형 3 차원 곡선 명령 또는 부드러운 정방형 3 차원 곡선 명령이 아닌 경우 제어점은 현재 지점과 일치 합니다. 이 명령은 앞에 곡선 세그먼트가 있었던 [**QuadraticBezierSegment**](/uwp/api/Windows.UI.Xaml.Media.QuadraticBezierSegment) 를 사용 하 여 [**pathgeometry**](/uwp/api/Windows.UI.Xaml.Media.PathGeometry) 에 해당 하는를 정의 합니다.

| 구문 |
|--------|
| `T`*controlPoint* *끝점* <br/> -또는- <br/> `t`*controlPoint* *끝점* |

| 용어 | Description |
|------|-------------|
| *controlPoint* | [**Point**](/uwp/api/Windows.Foundation.Point)<br/> 곡선의 시작 접선을 결정하는 곡선의 제어점입니다. |
| *끝점만* | [**Point**](/uwp/api/Windows.Foundation.Point)<br/> 곡선을 그릴 지점입니다. |

**타원형 원호 명령**

현재 점과 지정된 끝점 간에 타원형 호를 만듭니다. [**Arcsegment**](/uwp/api/Windows.UI.Xaml.Media.ArcSegment)를 사용 하 여 [**pathgeometry**](/uwp/api/Windows.UI.Xaml.Media.PathGeometry) 에 해당 하는를 정의 합니다.

| 구문 |
|--------|
| `A `*size* *rotationAngle* *isLargeArcFlag* *sweepDirectionFlag* *endPoint* <br/> -또는- <br/>`a `*size* *rotationAngle* *isLargeArcFlag* *sweepDirectionFlag* *endPoint* |

| 용어 | Description |
|------|-------------|
| *size* | [**크기**](/uwp/api/Windows.Foundation.Size)<br/>원호의 x 반지름과 y 반경입니다. |
| *rotationAngle* | [**double**](/dotnet/api/system.double) <br/> 타원의 회전 각도입니다. |
| *isLargeArcFlag* | 호의 각도가 180도보다 커야 하면 1로 설정하고, 그렇지 않으면 0으로 설정합니다. |
| *sweepDirectionFlag* | 호가 양의 각도 방향으로 그려지면 1로 설정하고, 그렇지 않으면 0으로 설정합니다. |
| *끝점만* | [**Point**](/uwp/api/Windows.Foundation.Point) <br/> 호를 그리는 점입니다. |

**닫기 명령**

현재 그림을 끝내고 현재 점을 그림의 시작 점에 연결하는 선을 만듭니다. 이 명령은 그림의 마지막 세그먼트와 첫 번째 세그먼트 사이에 선 조인(모서리)을 만듭니다.

| 구문 |
|--------|
| `Z` <br/> -또는- <br/> `z ` |

**Point 구문**

점의 x 좌표 및 y 좌표를 설명 합니다. [**참고 항목을 참조**](/uwp/api/Windows.Foundation.Point)하세요.

| 구문 |
|--------|
| *x* , *y*<br/> -또는- <br/>*x* *y* |

| 용어 | Description |
|------|-------------|
| *x* | [**double**](/dotnet/api/system.double) <br/> 점의 X 좌표입니다. |
| *y* | [**double**](/dotnet/api/system.double) <br/> 점의 Y 좌표입니다. |

**추가 참고 사항**

표준 숫자 값 대신 다음과 같은 특수 값을 사용할 수도 있습니다. 이러한 값은 대/소문자를 구분합니다.

-   **Infinity** : **PositiveInfinity** 를 나타냅니다.
-   **\- Infinity** : **NegativeInfinity** 를 나타냅니다.
-   **Nan** : **nan** 을 나타냅니다.

Decimals 또는 정수를 사용 하는 대신 과학적 표기법을 사용할 수 있습니다. 예를 들어 `+1.e17` 는 유효한 값입니다.

## <a name="design-tools-that-produce-move-and-draw-commands"></a>이동 및 그리기 명령을 생성 하는 디자인 도구

Blend의 **펜** 도구와 기타 그리기 도구를 Microsoft Visual Studio 2015에 사용 하면 일반적으로 이동 및 그리기 명령을 사용 하 여 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) 개체를 생성 합니다.

컨트롤의 Windows 런타임 XAML 기본 템플릿에 정의 된 컨트롤 부분 중 일부에 기존 이동 및 그리기 명령 데이터가 표시 될 수 있습니다. 예를 들어 일부 컨트롤은 데이터를 이동 및 그리기 명령으로 정의 하는 [**Pathicon**](/uwp/api/Windows.UI.Xaml.Controls.PathIcon) 을 사용 합니다.

XAML 형식으로 벡터를 출력할 수 있는 일반적으로 사용 되는 다른 벡터 그래픽 디자인 도구에 대해 내보내기 또는 플러그 인을 사용할 수 있습니다. 일반적으로 [**경로 데이터**](/uwp/api/windows.ui.xaml.shapes.path.data)에 대 한 이동 및 그리기 명령을 사용 하 여 레이아웃 컨테이너에 [**경로**](/uwp/api/Windows.UI.Xaml.Shapes.Path) 개체를 만듭니다. 서로 다른 브러시를 적용할 수 있도록 XAML에 **경로** 요소가 여러 개 있을 수 있습니다. 이러한 많은 내보내기 또는 플러그 인은 원래 Windows Presentation Foundation (WPF) XAML 또는 Silverlight에 대해 작성 되었지만 XAML 경로 구문은 Windows 런타임 XAML과 동일 합니다. 일반적으로 내보내기에서 XAML 청크를 사용 하 여 Windows 런타임 XAML 페이지에 바로 붙여 넣을 수 있습니다. 그러나 Windows 런타임 XAML이 해당 브러시를 지원 하지 않기 때문에 변환 된 XAML의 일부인 경우에는 **RadialGradientBrush** 을 사용할 수 없습니다.

## <a name="related-topics"></a>관련 항목

* [도형 그리기](../design/controls-and-patterns/shapes.md)
* [브러시 사용](../design/style/brushes.md)
* [**경로 데이터**](/uwp/api/windows.ui.xaml.shapes.path.data)
* [**PathIcon**](/uwp/api/Windows.UI.Xaml.Controls.PathIcon)