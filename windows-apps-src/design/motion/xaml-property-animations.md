---
title: XAML 속성 애니메이션
description: 컴퍼지션 애니메이션을 사용 하 여 XAML 요소에 애니메이션을 적용 합니다.
ms.date: 09/13/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 183a5433553ff6fdfcb09f6960f6a642f2c8bc08
ms.sourcegitcommit: cc0ef75f314658b14376eb60ef8e5bb4d7726e04
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65444158"
---
# <a name="animating-xaml-elements-with-composition-animations"></a>컴퍼지션 애니메이션을 사용 하 여 XAML 요소에 애니메이션 적용

이 문서에서는 구성 애니메이션의 성능을 간편 하 게 XAML 속성 설정와 XAML UIElement에 애니메이션을 적용할 수 있는 새 속성을 소개 합니다.

Windows 10 버전 1809, 이전 애니메이션에서 UWP 앱 빌드에 2 개의 선택 했습니다.

- 와 같은 XAML 구문을 사용 하 여 [애니메이션 스토리 보드를 만들었습니다](storyboarded-animations.md), 또는 _* ThemeTransition_ 하 고 _* ThemeAnimation_ 의 클래스는 [ Windows.UI.Xaml.Media.Animation](/uwp/api/windows.ui.xaml.media.animation) 네임 스페이스입니다.
- 에 설명 된 대로 컴퍼지션 애니메이션을 사용 하 여 [시각적 계층을 사용 하 여 XAML을 사용 하 여](../../composition/using-the-visual-layer-with-xaml.md)입니다.

시각적 계층을 사용 하 여 생성 된 XAML을 사용 하 여 보다 나은 성능을 제공 합니다. 하지만 사용 하 여 [ElementCompositionPreview](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) 컴퍼지션의 기본 요소를 가져오는 [Visual](/uwp/api/windows.ui.composition.visual) 개체를 다음 컴퍼지션 애니메이션을 사용 하 여 시각적 개체에 애니메이션 적용이 사용 하는 것이 복잡 합니다.

부터 Windows 10 버전 1809, 직접 필요 없이 애니메이션 컴퍼지션 기본 컴퍼지션 시각적 개체를 사용 하 여 UIElement 속성 애니메이트할 수 있습니다.

> [!NOTE]
> UIElement에서 이러한 속성을 사용 하려면 UWP 프로젝트 대상 버전 1809 이상 이어야 합니다. 프로젝트 버전을 구성 하는 방법에 대 한 자세한 내용은 참조 하세요. [적응 응용 프로그램 버전](../../debug-test-perf/version-adaptive-apps.md)합니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>있는 경우는 <strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱을 설치 하려면 여기를 클릭 <a href="xamlcontrolsgallery:/item/XamlCompInterop">앱을 열고 실행 중인 애니메이션 interop 참조</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="new-rendering-properties-replace-old-rendering-properties"></a>이전 렌더링 속성을 대체 하는 새 렌더링 속성

이 표에서 UIElement 사용 하 여 애니메이션도 수 있는 렌더링을 수정 하 여 속성을 [CompositionAnimation](/uwp/api/windows.ui.composition.compositionanimation)합니다.

| 속성 | 형식 | 설명 |
| -- | -- | -- |
| [불투명도](/uwp/api/windows.ui.xaml.uielement.opacity) | Double | 개체의 불투명도 수준 |
| [번역](/uwp/api/windows.ui.xaml.uielement.translation) | Vector3 | 요소의 X/Y/Z 위치를 이동 합니다. |
| [TransformMatrix](/uwp/api/windows.ui.xaml.uielement.transformmatrix) | Matrix4x4 | 요소에 적용할 변형 행렬 |
| [소수 자릿수](/uwp/api/windows.ui.xaml.uielement.scale) | Vector3 | 가운데에 중심점을 사용 하 여 요소를 확장 합니다. |
| [회전](/uwp/api/windows.ui.xaml.uielement.rotation) | 부동 | 요소 RotationAxis 및 중심점 회전 |
| [RotationAxis](/uwp/api/windows.ui.xaml.uielement.rotationaxis) | Vector3 | 회전의 축 |
| [CenterPoint](/uwp/api/windows.ui.xaml.uielement.centerpoint) | Vector3 | 크기 조정 및 회전 중심점 |

항등 속성 값을 다음 순서 대로 크기 조정, 회전 및 변환 속성을 사용 하 여 결합 됩니다.  항등, 크기 조정, 회전 변환 합니다.

따라서 이러한 속성을 수정 해도 새, 이러한 속성에는 요소의 레이아웃에 영향을 주지 [측정값](/uwp/api/windows.ui.xaml.uielement.measure)/[정렬](/uwp/api/windows.ui.xaml.uielement.arrange) 전달 합니다.

이러한 속성 값에 컴퍼지션에 같은 이름의 속성으로 동일한 목적 및 동작 [Visual](/uwp/api/windows.ui.composition.visual) 클래스 (시각적 개체에 없는 번역) 제외 합니다.

### <a name="example-setting-the-scale-property"></a>예: 확장 속성을 설정

이 예제에서는 단추에 확장 속성을 설정 하는 방법을 보여 줍니다.

```xaml
<Button Scale="2,2,1" Content="I am a large button" />
```

```csharp
var button = new Button();
button.Content = "I am a large button";
button.Scale = new Vector3(2.0f,2.0f,1.0f);
```

### <a name="mutual-exclusivity-between-new-and-old-properties"></a>새 일정과 이전 속성 간에 상호 배타 성은

> [!NOTE]
> 합니다 **불투명도** 속성에는이 섹션에 설명 된 상호 배타 성은 적용 하지 않습니다. XAML 또는 컴퍼지션 애니메이션을 사용 하 든 동일한 불투명도 속성을 사용 합니다.

CompositionAnimation를 사용 하 여 애니메이션을 적용할 수 있는 속성은 여러 기존 UIElement 속성에 대 한 대체:

- [RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform)
- [RenderTransformOrigin](/uwp/api/windows.ui.xaml.uielement.rendertransformorigin)
- [Projection](/uwp/api/windows.ui.xaml.uielement.projection)
- [Transform3D](/uwp/api/windows.ui.xaml.uielement.transform3d)

사용자 설정 (또는 애니메이션 효과 주기) 새 속성을 이전 속성을 사용할 수 없습니다. 반대로 사용자 설정 (또는 애니메이션 효과 주기) 이전 속성을 하는 경우에 새 속성을 사용할 수 없습니다.

ElementCompositionPreview 가져와 시각적 개체를 이러한 메서드를 사용 하 여 직접 관리를 사용 하는 경우에 새 속성을 사용할 수 없습니다.

- [ElementCompositionPreview.GetElementVisual](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual)
- [ElementCompositionPreview.SetIsTranslationEnabled](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setistranslationenabled)

> [!IMPORTANT]
> 조합 하 여 두 개의 속성 집합이 사용 하려고 하면 실패 하 고 오류 메시지에 대 한 API 호출 합니다.

편의상 권장 되지 않습니다 하지만, 선택을 취소 하 여 하나의 속성 집합에서 전환 하는 것이 가능 합니다. 속성을 DependencyProperty에 의해 지원 되는 경우 (예를 들어 UIElement.Projection에서 지 원하는 UIElement.ProjectionProperty), "사용 안 함" 상태로 복원 하려면 ClearValue 호출 합니다. (예: 확장 속성)이 고 그렇지 속성을 기본값으로 설정.

## <a name="animating-uielement-properties-with-compositionanimation"></a>UIElement CompositionAnimation 속성에 애니메이션 적용

CompositionAnimation 테이블에 나열 된 렌더링 속성 애니메이션을 적용할 수 있습니다. 이러한 속성을 참조할 수도 있습니다는 [ExpressionAnimation](/uwp/api/windows.ui.composition.expressionanimation)합니다.

사용 합니다 [StartAnimation](/uwp/api/windows.ui.xaml.uielement.startanimation) 하 고 [StopAnimation](/uwp/api/windows.ui.xaml.uielement.stopanimation) UIElement 속성에 애니메이션 효과를 UIElement 메서드.

### <a name="example-animating-the-scale-property-with-a-vector3keyframeanimation"></a>예: 확장 속성에는 Vector3KeyFrameAnimation 사용 하 여 애니메이션

이 예제에서는 단추의 크기에 애니메이션을 적용 하는 방법을 보여 줍니다.

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateVector3KeyFrameAnimation();

animation.InsertKeyFrame(1.0f, new Vector3(2.0f,2.0f,1.0f));
animation.Duration = TimeSpan.FromSeconds(1);
animation.Target = "Scale";

button.StartAnimation(animation);
```

### <a name="example-animating-the-scale-property-with-an-expressionanimation"></a>예: 확장 속성에는 ExpressionAnimation 사용 하 여 애니메이션

페이지에는 두 개의 단추가 있습니다. 두 번째 단추 애니메이션을 적용 클 두 번 (배율)를 통해 첫 번째 단추입니다.

```xaml
<Button x:Name="sourceButton" Content="Source"/>
<Button x:Name="destinationButton" Content="Destination"/>
```

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateExpressionAnimation("sourceButton.Scale*2");
animation.SetExpressionReferenceParameter("sourceButton", sourceButton);
animation.Target = "Scale";
destinationButton.StartAnimation(animation);
```

## <a name="related-topics"></a>관련 항목

- [애니메이션 스토리 보드를 만들었습니다](storyboarded-animations.md)
- [시각적 계층을 사용 하 여 XAML을 사용 하 여](../../composition/using-the-visual-layer-with-xaml.md)
- [Transform 개요](../layout/transforms.md)
