---
title: XAML 속성 애니메이션
description: UWP (유니버설 Windows 플랫폼) 컴퍼지션 애니메이션을 사용 하 여 XAML UIElement의 속성에 직접 애니메이션 효과를 주는 방법에 대해 알아봅니다.
ms.date: 09/13/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0a7ab119df407b45fb391aebf50df5d85f89ec0f
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238298"
---
# <a name="animating-xaml-elements-with-composition-animations"></a>컴퍼지션 애니메이션을 사용 하 여 XAML 요소에 애니메이션 효과 주기

이 문서에서는 컴퍼지션 애니메이션의 성능과 XAML 속성 설정의 용이성으로 XAML UIElement에 애니메이션 효과를 주는 새로운 속성을 소개 합니다.

Windows 10, 버전 1809 이전에는 UWP 앱에서 애니메이션을 빌드하기 위한 2 가지 선택 사항이 있었습니다.

- [Storyboarded](storyboarded-animations.md) [ANIMATION](/uwp/api/windows.ui.xaml.media.animation) 와 같은 XAML 구문이 나 ThemeTransition 네임 스페이스의 _*_ 및 _* ThemeAnimation_ 클래스를 사용 합니다.
- [XAML로 시각적 계층 사용](../../composition/using-the-visual-layer-with-xaml.md)에 설명 된 대로 컴퍼지션 애니메이션을 사용 합니다.

시각적 계층을 사용 하면 XAML 구문을 사용 하는 것 보다 더 나은 성능을 제공 합니다. 하지만 [ElementCompositionPreview](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) 를 사용 하 여 요소의 기본 컴퍼지션 [시각적](/uwp/api/windows.ui.composition.visual) 개체를 가져온 다음 컴퍼지션 애니메이션을 사용 하 여 시각적 개체에 애니메이션 효과를 적용 하는 것이 더 복잡 합니다.

Windows 10 버전 1809부터 기본 컴퍼지션 시각적 개체를 가져올 필요 없이 컴퍼지션 애니메이션을 사용 하 여 UIElement의 속성에 직접 애니메이션 효과를 적용할 수 있습니다.

> [!NOTE]
> UIElement에서 이러한 속성을 사용 하려면 UWP 프로젝트 대상 버전이 1809 이상 이어야 합니다. 프로젝트 버전을 구성 하는 방법에 대 한 자세한 내용은 [버전 적응 앱](../../debug-test-perf/version-adaptive-apps.md)을 참조 하세요.

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치 되어 있는 경우 여기를 클릭 하 여 <a href="xamlcontrolsgallery:/item/XamlCompInterop">앱을 열고 동작의 애니메이션 interop를 참조</a>하세요.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="new-rendering-properties-replace-old-rendering-properties"></a>새 렌더링 속성이 이전 렌더링 속성을 대체 합니다.

다음 표에서는 [CompositionAnimation](/uwp/api/windows.ui.composition.compositionanimation)에 애니메이션을 적용할 수 있는 UIElement의 렌더링을 수정 하는 데 사용할 수 있는 속성을 보여 줍니다.

| 속성 | 유형 | Description |
| -- | -- | -- |
| [불투명도](/uwp/api/windows.ui.xaml.uielement.opacity) | Double | 개체의 불투명도 수준입니다. |
| [변환](/uwp/api/windows.ui.xaml.uielement.translation) | Vector3 | 요소의 X/Y/Z 위치를 이동 합니다. |
| [TransformMatrix](/uwp/api/windows.ui.xaml.uielement.transformmatrix) | System.numerics.matrix4x4 | 요소에 적용할 변환 행렬입니다. |
| [크기 조정](/uwp/api/windows.ui.xaml.uielement.scale) | Vector3 | CenterPoint 가운데에 있는 요소의 크기를 조정 합니다. |
| [회전](/uwp/api/windows.ui.xaml.uielement.rotation) | Float | RotationAxis 및 CenterPoint 주위로 요소 회전 |
| [RotationAxis](/uwp/api/windows.ui.xaml.uielement.rotationaxis) | Vector3 | 회전의 축입니다. |
| [CenterPoint](/uwp/api/windows.ui.xaml.uielement.centerpoint) | Vector3 | 눈금 및 회전 중심점 |

TransformMatrix 속성 값은 TransformMatrix, Scale, Rotation, Translation 순서로 눈금, 회전 및 변환 속성과 결합 됩니다.

이러한 속성은 요소의 레이아웃에 영향을 주지 않으므로 이러한 속성을 수정 해도 새 [측정](/uwp/api/windows.ui.xaml.uielement.measure) / [정렬](/uwp/api/windows.ui.xaml.uielement.arrange) 단계가 발생 하지 않습니다.

이러한 속성은 컴퍼지션 [시각적](/uwp/api/windows.ui.composition.visual) 클래스 (시각적 개체에 없는 변환의 경우 제외)에서 같은 이름 속성의 목적과 동작을 포함 합니다.

### <a name="example-setting-the-scale-property"></a>예: Scale 속성 설정

이 예제에서는 단추에 Scale 속성을 설정 하는 방법을 보여 줍니다.

```xaml
<Button Scale="2,2,1" Content="I am a large button" />
```

```csharp
var button = new Button();
button.Content = "I am a large button";
button.Scale = new Vector3(2.0f,2.0f,1.0f);
```

### <a name="mutual-exclusivity-between-new-and-old-properties"></a>새 속성과 이전 속성 간의 상호 독점 성을

> [!NOTE]
> **Opacity** 속성은이 섹션에 설명 된 상호 독점 성을 적용 하지 않습니다. XAML 또는 컴퍼지션 애니메이션을 사용 하는지 여부에 관계 없이 동일한 불투명도 속성을 사용 합니다.

CompositionAnimation에 애니메이션을 적용할 수 있는 속성은 몇 가지 기존 UIElement 속성의 대체 항목입니다.

- [RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform)
- [System.windows.uielement.rendertransformorigin](/uwp/api/windows.ui.xaml.uielement.rendertransformorigin)
- [프로젝션](/uwp/api/windows.ui.xaml.uielement.projection)
- [Transform3D](/uwp/api/windows.ui.xaml.uielement.transform3d)

새 속성을 설정 (또는 애니메이션 효과를 적용) 할 때 이전 속성은 사용할 수 없습니다. 반대로, 이전 속성을 설정 (또는 애니메이션 효과를 적용) 하는 경우에는 새 속성을 사용할 수 없습니다.

ElementCompositionPreview를 사용 하 여 다음 메서드를 통해 시각적 개체를 직접 가져오고 관리 하는 경우에도 새 속성을 사용할 수 없습니다.

- [ElementCompositionPreview](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual)
- [ElementCompositionPreview.SetIsTranslationEnabled](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setistranslationenabled)

> [!IMPORTANT]
> 두 속성 집합의 사용을 혼합 하려고 하면 API 호출이 실패 하 고 오류 메시지가 생성 됩니다.

속성 집합을 지우면 한 속성 집합에서 전환 하는 것이 좋습니다. 그러나이를 단순화 하는 것이 좋습니다. 속성이 DependencyProperty에 의해 지원 되는 경우 (예: ProjectionProperty를 사용 하 여 프로젝션이 지원 됨) ClearValue를 호출 하 여 "사용 안 함" 상태로 복원 합니다. 그렇지 않은 경우 (예: Scale 속성) 속성을 기본값으로 설정 합니다.

## <a name="animating-uielement-properties-with-compositionanimation"></a>CompositionAnimation를 사용 하 여 UIElement 속성에 애니메이션 적용

CompositionAnimation를 사용 하 여 테이블에 나열 된 렌더링 속성에 애니메이션 효과를 적용할 수 있습니다. 이러한 속성은 [Expressionanimation](/uwp/api/windows.ui.composition.expressionanimation)에서 참조할 수도 있습니다.

Uielement의 [startanimation](/uwp/api/windows.ui.xaml.uielement.startanimation) 및 [stopanimation](/uwp/api/windows.ui.xaml.uielement.stopanimation) 메서드를 사용 하 여 uielement 속성에 애니메이션 효과를 적용 합니다.

### <a name="example-animating-the-scale-property-with-a-vector3keyframeanimation"></a>예: Vector3KeyFrameAnimation를 사용 하 여 Scale 속성에 애니메이션 효과 주기

이 예제에서는 단추의 눈금에 애니메이션 효과를 주는 방법을 보여 줍니다.

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateVector3KeyFrameAnimation();

animation.InsertKeyFrame(1.0f, new Vector3(2.0f,2.0f,1.0f));
animation.Duration = TimeSpan.FromSeconds(1);
animation.Target = "Scale";

button.StartAnimation(animation);
```

### <a name="example-animating-the-scale-property-with-an-expressionanimation"></a>예제: ExpressionAnimation을 사용 하 여 Scale 속성에 애니메이션 효과 주기

페이지에는 두 개의 단추가 있습니다. 두 번째 단추는 첫 번째 단추로 크기를 두 번 (눈금을 통해) 애니메이션 효과를 적용 합니다.

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

- [스토리보드 애니메이션](storyboarded-animations.md)
- [XAML과 함께 시각적 계층 사용](../../composition/using-the-visual-layer-with-xaml.md)
- [변환 개요](../layout/transforms.md)
