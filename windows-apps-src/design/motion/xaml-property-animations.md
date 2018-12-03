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
ms.openlocfilehash: 81da1e769ab171e47a4f4046e8ec7e7c84ecf2d1
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8461607"
---
# <a name="animating-xaml-elements-with-composition-animations"></a>컴퍼지션 애니메이션을 사용 하 여 애니메이션 효과 주는 XAML 요소

이 문서에서는 컴퍼지션 애니메이션의 성능 및 XAML 속성을 설정의 접근성을 사용 하 여 XAML UIElement에 애니메이션을 적용할 수 있는 새로운 속성이 소개 합니다.

Windows 10, 버전 1809 이전 UWP 앱에서 애니메이션을 빌드하는 데 2 선택 했습니다.

- [스토리 보드 애니메이션](storyboarded-animations.md)의 경우와 같은 XAML 구조를 사용 하 여 또는 _* ThemeTransition_ 및 _* ThemeAnimation_ [Windows.UI.Xaml.Media.Animation](/uwp/api/windows.ui.xaml.media.animation) 네임 스페이스의 클래스.
- [XAML 사용 하 여 시각적 계층을 사용 하 여](../../composition/using-the-visual-layer-with-xaml.md)에 설명 된 대로 컴퍼지션 애니메이션을 사용 합니다.

시각적 계층을 사용 하 여 XAML을 사용 하 여 생성 하는 것 보다 더 나은 성능을 제공 합니다. 하지만 요소의 기본 컴퍼지션 [시각적](/uwp/api/windows.ui.composition.visual) 개체를 가져오려면 [ElementCompositionPreview](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) 를 사용 하 고 컴퍼지션 애니메이션을 사용 하 여 시각적 애니메이션을 적용 한 다음는 사용 하 여 더 복잡 합니다.

Windows 10, 버전 1809부터 기본 컴퍼지션 시각적 가져오려면 필요 없이 컴퍼지션 애니메이션을 사용 하 여 직접 UIElement에서 속성을 애니메이션할 수 있습니다.

> [!NOTE]
> UIElement에서 이러한 속성을 사용 하려면 UWP 프로젝트 대상 버전 1809 이상 이어야 합니다. 프로젝트 버전을 구성 하는 방법에 대 한 자세한 내용은 [버전 적응 앱](../../debug-test-perf/version-adaptive-apps.md)을 참조 하세요.

## <a name="new-rendering-properties-replace-old-rendering-properties"></a>새로운 렌더링 속성 대체 이전 렌더링 속성

이 표에 [CompositionAnimation](/uwp/api/windows.ui.composition.compositionanimation)를 사용 하 여 애니메이션도 수 있는 UIElement의 렌더링을 수정 하는 데 사용할 수 있는 속성을 보여 줍니다.

| 속성 | 형식 | 설명 |
| -- | -- | -- |
| [불투명도](/uwp/api/windows.ui.xaml.uielement.opacity) | 이중 | 개체의 불투명도 |
| [Translation](/uwp/api/windows.ui.xaml.uielement.translation) | Vector3 | 요소의 X/Y/Z 위치를 이동 합니다. |
| [Transformmatrix 등이 있습니다](/uwp/api/windows.ui.xaml.uielement.transformmatrix) | Matrix4x4 | 요소에 적용할 변환 매트릭스 |
| [배율](/uwp/api/windows.ui.xaml.uielement.scale) | Vector3 | 배율 요소를 중심으로 중심점 |
| [회전](/uwp/api/windows.ui.xaml.uielement.rotation) | 부동 | RotationAxis 및 중심점 요소 회전 |
| [RotationAxis](/uwp/api/windows.ui.xaml.uielement.rotationaxis) | Vector3 | 회전 축 |
| [CenterPoint](/uwp/api/windows.ui.xaml.uielement.centerpoint) | Vector3 | 배율 및 회전의 중심점 |

항등 속성 값은 다음 순서로 배율, 회전 및 번역 속성과 결합: 항등, 배율, 회전, 변환 합니다.

이러한 속성은 요소의 레이아웃에 영향을 주지, 따라서 이러한 속성을 수정 해도 새 [측정](/uwp/api/windows.ui.xaml.uielement.measure)발생 하지 않습니다/[정렬](/uwp/api/windows.ui.xaml.uielement.arrange) 단계입니다.

이러한 속성은 동일한 목적 및 동작 컴퍼지션 [시각적](/uwp/api/windows.ui.composition.visual) 클래스 (제외 하 고 번역 Visual에 없는)에서 같은 명명 된 속성으로 사용 합니다.

### <a name="example-setting-the-scale-property"></a>예: 배율 속성 설정

이 예제에서는 단추에서 Scale 속성을 설정 하는 방법을 보여 줍니다.

```xaml
<Button Scale="2,2,1" Content="I am a large button" />
```

```csharp
var button = new Button();
button.Content = "I am a large button";
button.Scale = new Vector3(2.0f,2.0f,1.0f);
```

### <a name="mutual-exclusivity-between-new-and-old-properties"></a>신규 및 기존 속성 간의 상호 배타적

> [!NOTE]
> **Opacity** 속성에는이 섹션에 설명 된 상호 배타적을 적용 하지 않습니다. XAML 또는 컴퍼지션 애니메이션을 사용 하 든 동일한 Opacity 속성을 사용 합니다.

CompositionAnimation를 사용 하 여 애니메이션을 적용할 수 있는 속성은 여러 기존 UIElement 속성에 대 한 대체:

- [RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform)
- [RenderTransformOrigin](/uwp/api/windows.ui.xaml.uielement.rendertransformorigin)
- [프로젝션](/uwp/api/windows.ui.xaml.uielement.projection)
- [Transform3D](/uwp/api/windows.ui.xaml.uielement.transform3d)

설정 (애니메이션)의 새 속성을 하거나 이전 속성을 사용할 수 없습니다. 반대로, 사용자 설정 (또는 애니메이션)의 이전 속성, 경우에 새 속성을 사용할 수 없습니다.

ElementCompositionPreview를 사용 하 여를 가져오고 이러한 메서드를 사용 하 여 직접 시각적 개체를 관리 하는 경우에 새 속성 사용할 수 없습니다.

- [ElementCompositionPreview.GetElementVisual](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual)
- [ElementCompositionPreview.SetIsTranslationEnabled](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setistranslationenabled)

> [!IMPORTANT]
> 두 속성 집합의 사용을 혼합 하려고 하면 실패 하 고 오류 메시지에 대 한 API 호출 합니다.

편의상 권장 하지는 않지만, 취소 하 여 속성 집합에서 전환 하는 것이 가능 합니다. 속성 DependencyProperty에 의해 지원 받지는 경우 (예를 들어 UIElement.Projection에 의해 지원 받지는 UIElement.ProjectionProperty), "사용 하지 않는" 상태로 복원 하는 ClearValue 호출 합니다. (예: 배율 속성), 그렇지 않으면 속성의 기본값을 설정 합니다.

## <a name="animating-uielement-properties-with-compositionanimation"></a>CompositionAnimation 사용 하 여 애니메이션 효과 주는 UIElement 속성

CompositionAnimation 포함 된 테이블에 나열 된 렌더링 속성에 애니메이션 효과 줄 수 있습니다. 또한는 [ExpressionAnimation](/uwp/api/windows.ui.composition.expressionanimation)으로 이러한 속성을 참조할 수 있습니다.

UIElement 속성에 애니메이션 효과를 마친 UIElement에서 [StartAnimation](/uwp/api/windows.ui.xaml.uielement.startanimation) 및 [StopAnimation](/uwp/api/windows.ui.xaml.uielement.stopanimation) 메서드를 사용 합니다.

### <a name="example-animating-the-scale-property-with-a-vector3keyframeanimation"></a>예: Vector3KeyFrameAnimation 사용 하 여 배율 속성에 애니메이션 효과

이 예제에서는 단추의 배율에 애니메이션을 적용 하는 방법을 보여 줍니다.

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateVector3KeyFrameAnimation();

animation.InsertKeyFrame(1.0f, new Vector3(2.0f,2.0f,1.0f));
animation.Duration = TimeSpan.FromSeconds(1);
animation.Target = "Scale";

button.StartAnimation(animation);
```

### <a name="example-animating-the-scale-property-with-an-expressionanimation"></a>예: ExpressionAnimation 사용 하 여 배율 속성에 애니메이션 효과

페이지에 두 개의 단추가 있습니다. 두 번째 단추 (scale)를 통해 크게 두 번 하려면 애니메이션으로 첫 번째 단추입니다.

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
- [시각적 계층을 XAML과 함께 사용](../../composition/using-the-visual-layer-with-xaml.md)
- [변환 개요](../layout/transforms.md)
