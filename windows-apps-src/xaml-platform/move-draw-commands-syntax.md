---
author: jwmsft
description: 경로 도형을 XAML 특성 값으로 지정하는 데 사용할 수 있는 이동 및 그리기 명령(미니 언어)에 대해 알아봅니다.
title: 이동 및 그리기 명령 구문
ms.assetid: 7772BC3E-A631-46FF-9940-3DD5B9D0E0D9
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d77049cbaa289fe8621e8cf91883952e6edda9b2
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5745193"
---
# <a name="move-and-draw-commands-syntax"></a>이동 및 그리기 명령 구문


경로 도형을 XAML 특성 값으로 지정하는 데 사용할 수 있는 이동 및 그리기 명령(미니 언어)에 대해 알아봅니다. 이동 및 그리기 명령은 벡터 그래픽이나 셰이프를 직렬화 형식과 교환 형식으로 출력할 수 있는 많은 디자인 및 그래픽 도구에서 사용됩니다.

## <a name="properties-that-use-move-and-draw-command-strings"></a>이동 및 그리기 명령 문자열을 사용하는 속성

이동 및 그리기 명령 구문은 명령을 구문 분석하고 런타임 그래픽 표현을 생성하는 XAML용 내부 유형 컨버터에서 지원됩니다. 이 표현은 기본적으로 즉시 표현 가능한 완료된 벡터 집합입니다. 벡터 자체는 표현 세부 사항을 완성하지 않으며, 요소에 대해 다른 값을 설정해야 합니다. [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) 개체의 경우 [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill), [**Stroke**](https://msdn.microsoft.com/library/windows/apps/br243383) 및 다른 속성의 값도 필요하며 그런 다음에는 **Path**를 시각적 트리에 연결해야 합니다. [**PathIcon**](https://msdn.microsoft.com/library/windows/apps/dn252722) 개체의 경우 [**Foreground**](https://msdn.microsoft.com/library/windows/apps/dn251974) 속성을 설정합니다.

Windows 런타임에는 이동 및 그리기 명령을 표현하는 문자열을 사용할 수 있는 두 개의 속성, 즉 [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/br243356) 및 [**PathIcon.Data**](https://msdn.microsoft.com/library/windows/apps/dn252723)가 있습니다. 이동 및 그리기 명령을 지정하여 이 속성 중 하나를 설정하는 경우 일반적으로 해당 요소의 다른 필수 특성과 함께 속성을 XAML 특성 값으로 설정하게 됩니다. 대략적인 모양은 다음과 같습니다.

```xml
<Path x:Name="Arrow" Fill="White" Height="11" Width="9.67"
  Data="M4.12,0 L9.67,5.47 L4.12,10.94 L0,10.88 L5.56,5.47 L0,0.06" />
```

[**PathGeometry.Figures**](https://msdn.microsoft.com/library/windows/apps/br210169) 역시 이동 및 그리기 명령을 사용할 수 있습니다. 이동 및 그리기 명령을 사용하는 [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168) 개체를 [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/br243356) 값으로 사용하게 될 [**GeometryGroup**](https://msdn.microsoft.com/library/windows/apps/br210057) 개체의 다른 [**Geometry**](/uwp/api/Windows.UI.Xaml.Media.Geometry) 유형과 결합할 수도 있습니다. 하지만 이것은 특성 정의 데이터에 대해 이동 및 그리기 명령을 사용하는 것만큼 일반적이지는 않습니다.

## <a name="using-move-and-draw-commands-versus-using-a-pathgeometry"></a>이동 및 그리기 명령 사용과 **PathGeometry** 사용 비교

Windows 런타임 XAML의 경우 이동 및 그리기 명령은 [**Figures**](https://msdn.microsoft.com/library/windows/apps/br210169) 속성 값이 있는 단일 [**PathFigure**](https://msdn.microsoft.com/library/windows/apps/br210143) 개체를 사용하여 [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168)를 생성합니다. 각각의 그리기 명령은 이 단일 **PathFigure**의 [**Segments**](https://msdn.microsoft.com/library/windows/apps/br210164) 컬렉션에서 [**PathSegment**](https://msdn.microsoft.com/library/windows/apps/br210174) 파생 클래스를 생성하고, 이동 명령은 [**StartPoint**](https://msdn.microsoft.com/library/windows/apps/br210166)를 변경하며, 닫기 명령의 존재는 [**IsClosed**](https://msdn.microsoft.com/library/windows/apps/br210159)를 **true**로 설정합니다. 런타임에 **Data** 값을 검사하는 경우 이 구조를 개체 모델로 탐색할 수 있습니다.

## <a name="the-basic-syntax"></a>기본 구문

이동 및 그리기 명령 구문은 다음과 같이 요약할 수 있습니다.

1.  선택적인 채우기 규칙을 시작합니다. 일반적으로 **EvenOdd** 기본값을 사용하지 않으려는 경우에만 이를 지정합니다. (**EvenOdd**에 대한 자세한 내용은 뒷부분에서 설명합니다.)
2.  이동 명령을 정확히 하나만 지정합니다.
3.  그리기 명령을 하나 이상 지정합니다.
4.  닫기 명령을 지정합니다. 닫기 명령을 생략할 수 있지만 그러면 그림이 열린 채로 유지될 수 있습니다. 이는 일반적인 동작이 아닙니다.

이 구문의 일반 규칙은 다음과 같습니다.

-   각 명령은 정확히 하나의 문자로 표현됩니다.
-   이 문자는 대문자이거나 소문자일 수 있습니다. 대/소문자를 구분합니다(이후에 설명).
-   닫기 명령을 제외한 각 명령 뒤에는 일반적으로 하나 이상의 숫자가 나옵니다.
-   명령에 대해 숫자가 여러 개인 경우 쉼표나 공백을 사용하여 구분합니다.

**\[**_fillRule_**\]** _moveCommand_ _drawCommand_ **\[**_drawCommand_**\*\]** **\[**_closeCommand_**\]**

다수의 그리기 명령은 점을 사용하며, 이 점에서 _x,y_ 값을 제공합니다. \*_points_ 자리 표시자가 표시될 때마다 점의 _x,y_ 값에 대해 두 개의 10진수 값을 제공한다고 가정할 수 있습니다.

결과가 모호하지 않다면 공백을 종종 생략할 수 있습니다. 모든 숫자 집합(포인트 및 크기)에 대한 구분 기호로 쉼표를 사용하는 경우 실제로 모든 공백을 생략해도 됩니다. 예를 들어 다음과 같이 사용해도 유효합니다. `F1M0,58L2,56L6,60L13,51L15,53L6,64z`. 하지만 명료하게 하기 위해 명령 사이에 공백을 포함하는 것이 보다 일반적입니다.

쉼표를 10진수의 소수점으로 사용하지 마세요. 명령 문자열은 XAML에 의해 해석되며 **en-us** 로캘에서 사용되는 것과 다른 문화권별 숫자 형식 지정 규칙이 적용되지 않습니다.

## <a name="syntax-specifics"></a>구문 세부 정보

**채우기 규칙**

선택적인 채우기 규칙에 가능한 두 개의 값, 즉 **F0** 또는 **F1**이 있습니다. **F**는 항상 대문자입니다. **F0**은 기본값으로, **EvenOdd** 채우기 동작을 생성하며 일반적으로 이를 지정하지 않습니다. **Nonzero** 채우기 동작을 얻으려면 **F1**을 사용하세요. 이러한 채우기 값은 [**FillRule**](https://msdn.microsoft.com/library/windows/apps/br210030) 열거형의 값과 정렬됩니다.

**이동 명령**

새 그림의 시작점을 지정합니다.

| 구문 |
|--------|
| `M ` _startPoint_ <br/>- 또는 -<br/>`m` _startPoint_|

| 용어 | 설명 |
|------|-------------|
| _startPoint_ | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/>새 그림의 시작점입니다.|

대문자 **M**은 *startPoint*가 절대 좌표임을 나타내고, 소문자 **m**은 *startPoint*가 이전 점의 오프셋이거나 이전 점이 없는 경우 (0,0)임을 나타냅니다.

**참고**는 이동 명령 뒤의 다중 포인트를 지정할 수 있습니다. 선 명령을 지정한 경우처럼 해당 점으로 선이 그려집니다. 하지만 이는 권장 스타일이 아니므로, 대신 전용 선 명령을 사용하세요.

**그리기 명령**

그리기 명령은 선, 가로선, 세로선, 입방형 3차원 곡선, 정방형 3차원 곡선, 부드러운 입방형 3차원 곡선, 부드러운 정방형 3차원 곡선, 타원형 호 등 다수의 셰이프 명령으로 구성될 수 있습니다.

모든 그리기 명령에서 대문자는 중요합니다. 대문자는 절대 좌표를 나타내고, 소문자는 이전 명령에 대해 상대적인 좌표를 나타냅니다.

세그먼트의 제어점은 이전 세그먼트의 끝점에 대해 상대적입니다. 동일한 유형의 여러 명령을 순차적으로 입력할 때 중복된 명령 항목을 생략해도 됩니다. 예를 들어 `L 100,200 300,400`과 `L 100,200 L 300,400`은 동일합니다.

**선 명령**

현재 점과 지정된 끝점 사이에 직선을 만듭니다. `l 20 30` 및 `L 20,30`은 유효한 선 명령의 예입니다. [**LineGeometry**](https://msdn.microsoft.com/library/windows/apps/br210117) 개체에 상응하는 항목을 정의합니다.

| 구문 |
|--------|
| `L` _endPoint_ <br/>- 또는 -<br/>`l` _endPoint_ |

| 용어 | 설명 |
|------|-------------|
| endPoint | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)<br/>선의 끝점입니다.|

**가로선 명령**

현재 점과 지정된 x 좌표 사이에 가로선을 만듭니다. `H 90` 은 유효한 가로선 명령의 예입니다.

| 구문 |
|--------|
| `H ` _x_ <br/> - 또는 - <br/>`h ` _x_ |

| 용어 | 설명 |
|------|-------------|
| x | [**이중**](https://msdn.microsoft.com/library/windows/apps/system.double.aspx) <br/> 선 끝점의 x 좌표입니다. |

**세로선 명령**

현재 점과 지정된 y 좌표 사이에 세로선을 만듭니다. `v 90` 은 유효한 세로선 명령의 예입니다.

| 구문 |
|--------|
| `V ` _Y_ <br/> - 또는 - <br/> `v ` _Y_ |

| 용어 | 설명 |
|------|-------------|
| *Y* | [**이중**](https://msdn.microsoft.com/library/windows/apps/system.double.aspx) <br/> 선 끝점의 y 좌표입니다. |

**입방형 3차원 곡선 명령**

두 개의 지정된 제어점(*controlPoint1* 및 *controlPoint2*)을 사용하여 현재 점과 지정된 끝점 사이에 입방형 3차원 곡선을 만듭니다. `C 100,200 200,400 300,200` 은 유효한 곡선 명령의 예입니다. [**BezierSegment**](https://msdn.microsoft.com/library/windows/apps/br228068) 개체가 있는 [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168) 개체에 상응하는 항목을 정의합니다.

| 구문 |
|--------|
| `C ` *controlPoint1* *controlPoint2* *endPoint* <br/> - 또는 - <br/> `c ` *controlPoint1* *controlPoint2* *endPoint* |

| 용어 | 설명 |
|------|-------------|
| *controlPoint1* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> 곡선의 첫 번째 제어점으로, 곡선의 시작 접선을 결정합니다. |
| *controlPoint2* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> 곡선의 두 번째 제어점으로, 곡선의 끝 접선을 결정합니다. |
| *endPoint* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> 곡선을 그려 도달하는 점입니다. | 

**정방형 3차원 곡선 명령**

지정된 제어점(*controlPoint*)을 사용하여 현재 점과 지정된 끝점 사이에 정방형 3차원 곡선을 만듭니다. `q 100,200 300,200` 은 유효한 정방형 3차원 곡선 명령의 예입니다. [**QuadraticBezierSegment**](https://msdn.microsoft.com/library/windows/apps/br210249)가 있는 [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168)에 상응하는 항목을 정의합니다.

| 구문 |
|--------|
| `Q ` *controlPoint endPoint* <br/> - 또는 - <br/> `q ` *controlPoint endPoint* |

| 용어 | 설명 |
|------|-------------|
| *controlPoint* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> 곡선의 제어점으로, 곡선의 시작 접선과 끝 접선을 결정합니다. |
| *endPoint* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)<br/> 곡선을 그려 도달하는 점입니다. |

**부드러운 입방형 3차원 곡선 명령**

현재 점과 지정된 끝점 사이에 입방형 3차원 곡선을 만듭니다. 첫 번째 제어점은 현재 점에 대해 상대적인 이전 명령의 두 번째 제어점의 반사로 간주됩니다. 이전 명령이 없는 경우 또는 이전 명령이 입방형 3차원 곡선 명령이나 부드러운 입방형 3차원 곡선 명령이 아닌 경우에는 첫 번째 제어점이 현재 점과 일치한다고 가정합니다. 두 번째 제어점, 즉 곡선 끝에 대한 제어점은 *controlPoint2*에 의해 지정됩니다. 예를 들어 `S 100,200 200,300`은 유효한 부드러운 입방형 3차원 곡선 명령입니다. 이 명령은 이전 곡선 세그먼트가 있던 [**BezierSegment**](https://msdn.microsoft.com/library/windows/apps/br228068)가 있는 [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168)에 상응하는 항목을 정의합니다.

| 구문 |
|--------|
| `S` *controlPoint2* *endPoint* <br/> - 또는 - <br/>`s` *controlPoint2 endPoint* |

| 용어 | 설명 |
|------|-------------|
| *controlPoint2* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> 곡선의 제어점으로, 곡선의 끝 접선을 결정합니다. |
| *endPoint* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)<br/> 곡선을 그려 도달하는 점입니다. |

**부드러운 정방형 3차원 곡선 명령**

현재 점과 지정된 끝점 사이에 정방형 3차원 곡선을 만듭니다. 제어점은 현재 점에 대해 상대적인 이전 명령의 제어점의 반사로 간주됩니다. 이전 명령이 없는 경우 또는 이전 명령이 정방형 3차원 곡선 명령이나 부드러운 정방형 3차원 곡선 명령이 아닌 경우에는 제어점이 현재 점과 일치합니다. 이 명령은 이전 곡선 세그먼트가 있던 [**QuadraticBezierSegment**](https://msdn.microsoft.com/library/windows/apps/br210249)가 있는 [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168)에 상응하는 항목을 정의합니다.

| 구문 |
|--------|
| `T` *controlPoint* *endPoint* <br/> - 또는 - <br/> `t` *controlPoint* *endPoint* |

| 용어 | 설명 |
|------|-------------|
| *controlPoint* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)<br/> 곡선의 제어점으로, 곡선의 시작 및 끝 접선을 결정합니다. |
| *endPoint* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)<br/> 곡선을 그려 도달하는 점입니다. |

**타원형 호 명령**

현재 점과 지정된 끝점 사이에 타원형 호를 만듭니다. [**ArcSegment**](https://msdn.microsoft.com/library/windows/apps/br228054)가 있는 [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168)에 상응하는 항목을 정의합니다.

| 구문 |
|--------|
| `A ` *size* *rotationAngle* *isLargeArcFlag* *sweepDirectionFlag* *endPoint* <br/> - 또는 - <br/>`a ` *sizerotationAngleisLargeArcFlagsweepDirectionFlagendPoint* |

| 용어 | 설명 |
|------|-------------|
| *size* | [**크기**](https://msdn.microsoft.com/library/windows/apps/br225995)<br/>호의 x 반경 및 y 반경입니다. |
| *rotationAngle* | [**이중**](https://msdn.microsoft.com/library/windows/apps/system.double.aspx) <br/> 타원의 회전(도 단위)입니다. |
| *isLargeArcFlag* | 호의 각도가 180도 이상이어야 하는 경우 1로 설정하고, 그렇지 않으면 0으로 설정합니다. |
| *sweepDirectionFlag* | 호가 양수 각도 방향으로 그려지는 경우 1로 설정하고, 그렇지 않으면 0으로 설정합니다. |
| *endPoint* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> 호를 그려 도달하는 점입니다.|
 
**닫기 명령**

현재 그림을 끝내고 현재 점을 그림 시작점에 연결하는 선을 만듭니다. 이 명령은 그림의 마지막 세그먼트와 첫 번째 세그먼트 사이에 선 이음(모서리)을 만듭니다.

| 구문 |
|--------|
| `Z` <br/> 또는 <br/> `z ` |

**점 구문**

점의 x 좌표 및 y 좌표를 설명합니다. [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)를 참조하세요.

| 구문 |
|--------|
| *x*,*y*<br/> 또는 <br/>*x* *y* |

| 용어 | 설명 |
|------|-------------|
| *x* | [**이중**](https://msdn.microsoft.com/library/windows/apps/system.double.aspx) <br/> 점의 x 좌표입니다. |
| *Y* | [**이중**](https://msdn.microsoft.com/library/windows/apps/system.double.aspx) <br/> 점의 y 좌표입니다. |

**추가 참고 사항**

표준 숫자 값 대신 다음과 같은 특수 값을 사용할 수도 있습니다. 이러한 값은 대/소문자를 구분합니다.

-   **Infinity**: **PositiveInfinity**를 나타냅니다.
-   **\-Infinity**: **NegativeInfinity**를 나타냅니다.
-   **NaN**: **NaN**을 나타냅니다.

10진수나 정수를 사용하는 대신 과학적 표기법을 사용할 수 있습니다. 예를 들어 `+1.e17`은 유효한 값입니다.

## <a name="design-tools-that-produce-move-and-draw-commands"></a>이동 및 그리기 명령을 생성하는 디자인 도구

Blend for Microsoft Visual Studio2015에서에서 **펜** 도구 및 다른 그리기 도구를 사용 하는 이동을 사용 하는 [**경로**](/uwp/api/Windows.UI.Xaml.Shapes.Path) 개체를 생성 및 그리기 명령을 일반적으로 합니다.

컨트롤에 대한 Windows 런타임 XAML 기본 템플릿에 정의된 일부 컨트롤 파트에 기존 이동 및 그리기 명령 데이터가 있을 수 있습니다. 예를 들어 일부 컨트롤은 이동 및 그리기 명령으로 정의된 데이터가 포함된 [**PathIcon**](https://msdn.microsoft.com/library/windows/apps/dn252722)을 사용합니다.

XAML 형식으로 벡터를 출력할 수 있으며 일반적으로 사용되는 다른 벡터 그래픽 디자인 도구에 사용할 수 있는 내보내기 또는 플러그 인이 있습니다. 이 내보내기 또는 플러그 인은 [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/br243356)에 대한 이동 및 그리기 명령과 함께 레이아웃 컨테이너에서 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) 개체를 만듭니다. 다양한 브러시를 적용할 수 있도록 XAML에서 여러 **Path** 요소가 있을 수 있습니다. 이 내보내기 또는 플러그 인 중 상당수는 원래 WPF(Windows Presentation Foundation) XAML 또는 Silverlight용으로 작성되었지만, XAML 경로 구문은 Windows 런타임 XAML과 동일합니다. 일반적으로 내보내기에서 가져온 XAML의 청크를 사용하여 Windows 런타임 XAML 페이지에 직접 붙여 넣을 수 있습니다. 하지만 변환된 XAML의 일부인 경우 **RadialGradientBrush**를 사용할 수 없습니다. Windows 런타임 XAML에서 해당 브러시가 지원되지 않기 때문입니다.

## <a name="related-topics"></a>관련 항목

* [셰이프 그리기](https://msdn.microsoft.com/library/windows/apps/mt280380)
* [브러시 사용](https://msdn.microsoft.com/library/windows/apps/mt280383)
* [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/br243356)
* [**PathIcon**](https://msdn.microsoft.com/library/windows/apps/dn252722)

