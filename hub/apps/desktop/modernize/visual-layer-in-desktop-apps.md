---
title: 시각적 계층을 사용하여 데스크톱 앱 현대화
description: 시각적 계층을 사용하여 .NET 또는 Win32 데스크톱 앱의 UI를 향상시킵니다.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 249291c59a31036fa967ac338209404557b57503
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "66215171"
---
# <a name="using-the-visual-layer-in-desktop-apps"></a>데스크톱 앱에서 시각적 계층 사용

이제 비 UWP 데스크톱 애플리케이션의 UWP API를 사용하여 WPF, Windows Forms 및 C++ Win32 애플리케이션의 모양, 느낌 및 기능을 향상시키고, UWP를 통해서만 사용할 수 있는 최신 Windows 10 UI 기능을 활용할 수 있습니다.

대부분의 시나리오에서는 [XAML islands](xaml-islands.md)를 사용하여 최신 XAML 컨트롤을 앱에 추가할 수 있습니다. 그러나 기본 제공 컨트롤을 능가하는 사용자 지정 환경을 만들어야 할 때 시각적 계층 API에 액세스할 수 있습니다.

시각적 계층은 그래픽, 효과 및 애니메이션에 대한 고성능, 유지 모드 API를 제공합니다. 이 계층은 Windows 10 디바이스에서 UI의 기초가 됩니다. UWP XAML 컨트롤은 시각적 계층을 토대로 작성되었으며, 밝기, 깊이, 동작, 재질 및 크기와 같은 [흐름 디자인 시스템](/windows/uwp/design/fluent-design-system/index)의 다양한 측면을 사용할 수 있습니다.

![시각적 계층으로 만든 사용자 인터페이스](images/visual-layer-interop/pull-to-animate.gif)

> _시각적 계층으로 만든 사용자 인터페이스_

## <a name="create-a-visually-engaging-user-interface-in-any-windows-app"></a>모든 Windows 앱에서 시각적으로 매력적인 사용자 인터페이스 만들기

시각적 계층을 사용하면 사용자 지정 그린 콘텐츠(시각적 개체)를 간단하게 합성하고 애플리케이션에서 해당 개체에 강력한 애니메이션, 효과 및 조작을 적용하여 매력적인 환경을 만들 수 있습니다. 시각적 계층은 기존 UI 프레임워크를 대체하지 않습니다. 대신 이러한 프레임워크에 대한 중요한 보완 기능입니다.

시각적 계층을 사용하여 애플리케이션에 고유한 모양과 느낌을 제공하고 다른 애플리케이션과는 분리되는 ID를 설정할 수 있습니다. 또한 애플리케이션을 더 쉽게 사용할 수 있도록 디자인된 흐름 디자인 원칙을 통해 사용자의 더 많은 참여를 이끌 수 있습니다. 예를 들어, 화면에서 항목 간의 관계를 표시하는 시각적 신호 및 애니메이션 효과를 적용한 화면 전환을 만드는 데 사용할 수 있습니다.

## <a name="visual-layer-features"></a>시각적 계층 기능

### <a name="brushes"></a>브러시

[컴퍼지션 브러시](/windows/uwp/composition/composition-brushes)를 사용하여 단색, 그라데이션, 이미지, 비디오, 복잡한 효과 등을 사용하여 UI 개체를 그릴 수 있습니다.

![Material Creator로 만든 계란](images/visual-layer-interop/egg.gif)

> _[Material Creator 데모 앱](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/Demos/MaterialCreator)으로 만든 계란_

### <a name="effects"></a>효과

[컴퍼지션 효과](/windows/uwp/composition/composition-effects)에는 조명, 그림자 및 필터 효과 목록이 포함됩니다. 애니메이션을 적용하고, 사용자 지정하고, 연결한 다음, 시각적 개체에 직접 적용할 수 있습니다. SceneLightingEffect를 컴퍼지션 조명과 결합하여 분위기, 깊이 및 재질을 만들 수 있습니다.

![조명 및 재질](images/visual-layer-interop/light-interop.gif)

> _[Windows UI 컴퍼지션 샘플 갤러리](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)에 표시되는 조명 및 재질_

### <a name="animations"></a>애니메이션

[컴퍼지션 애니메이션](/windows/uwp/composition/composition-animation)은 UI 스레드와는 독립적으로 Compositor 프로세스에서 직접 실행됩니다. 이렇게 하면 많은 수의 명시적인 동시 애니메이션을 실행할 수 있으므로 매끄럽게 확장 및 축소될 수 있습니다. 시간이 지남에 따라 속성 변경을 구동하기 위한 친숙한 KeyFrame 애니메이션 외에도 식을 사용하여 사용자 입력을 비롯한 다양한 속성 간의 수학적 관계를 설정할 수 있습니다. 입력 구동 애니메이션을 사용하면 동적이면서 유연하게 사용자 입력에 응답하는 UI를 만들 수 있으며, 이로 인해 사용자 참여가 더 높아질 수 있습니다.

![시각적 계층으로 만든 사용자 인터페이스](images/visual-layer-interop/swipe-scroller.gif)

> _[Windows UI 컴퍼지션 샘플 갤러리](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)에 표시되는 동작_

## <a name="keep-your-existing-codebase-and-adopt-incrementally"></a>기존 코드베이스를 유지하고 증분 방식으로 채택

기존 애플리케이션의 코드는 손실하고 싶지 않은 상당한 투자에 해당합니다. 시각적 계층을 사용하고 나머지 UI는 기존 프레임워크에 유지하기 위해 콘텐츠의 _islands_를 마이그레이션할 수 있습니다. 즉, 기존 코드베이스를 광범위하게 변경할 필요 없이 애플리케이션 UI를 획기적으로 업데이트하고 개선할 수 있습니다.

## <a name="samples-and-tutorials"></a>샘플 및 자습서

샘플을 실험하여 애플리케이션에서 시각적 계층을 사용하는 방법을 알아봅니다. 이러한 샘플과 자습서에서는 시각적 계층 사용을 시작하고 기능의 작동 방식을 보여 줍니다.

### <a name="win32"></a>Win32

- [Win32에서 시각적 계층 사용](using-the-visual-layer-with-win32.md) 자습서
  - [Hello 컴퍼지션 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Hello 벡터 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloVectors)
- [Virtual Surfaces 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/VirtualSurfaces)
- [화면 캡처 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/ScreenCaptureforHWND)

### <a name="windows-forms"></a>Windows Forms

- [Windows Forms에서 시각적 계층 사용](using-the-visual-layer-with-windows-forms.md) 자습서
  - [Hello 컴퍼지션 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)
- [시각적 계층 통합 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)

### <a name="wpf"></a>WPF

- [WPF에서 시각적 계층 사용](using-the-visual-layer-with-wpf.md) 자습서
  - [Hello 컴퍼지션 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition)
- [시각적 계층 통합 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration)
- [화면 캡처 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/ScreenCapture)

## <a name="limitations"></a>제한 사항

대부분의 시각적 계층 기능은 UWP 앱에서와 같이 데스크톱 애플리케이션에서 호스트될 때 동일하게 작동하지만 일부 기능에는 제한이 있습니다. 다음과 같은 몇 가지 제한 사항에 유의해야 합니다.

- 효과 체인은 효과 설명에 [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm)를 사용합니다. [Win2D NuGet 패키지](https://www.nuget.org/packages/Win2D.uwp)는 데스크톱 애플리케이션에서 지원되지 않으므로 [소스 코드](https://github.com/Microsoft/Win2D)에서 다시 컴파일해야 합니다.
- 적중 테스트를 수행하려면 시각적 트리를 직접 이동하면서 범위를 계산해야 합니다. 이 방식은 UWP의 시각적 계층과 동일합니다. 단, 이 경우에는 적중 테스트를 위해 쉽게 바인딩할 수 있는 XAML 요소가 없습니다.
- 시각적 계층에는 텍스트를 렌더링하기 위한 기본 형식이 없습니다.
- WPF 및 시각적 계층과 같은 두 가지 UI 기술을 함께 사용하는 경우 각각 화면에서 고유한 픽셀을 그리며 픽셀을 공유할 수 없습니다. 따라서 시각적 계층 콘텐츠는 항상 다른 UI 콘텐츠 위에 렌더링됩니다. (이것은 _에어스페이스_ 문제로 알려져 있습니다.) 호스트 UI를 사용하여 시각적 계층 콘텐츠 크기를 조정하고 다른 콘텐츠를 가리지 않도록 하려면 추가적인 코딩 및 테스트를 수행해야 할 수 있습니다.
- 데스크톱 애플리케이션에서 호스트되는 콘텐츠는 DPI에 맞게 자동으로 크기 또는 규모가 조정되지 않습니다. 콘텐츠가 DPI 변경을 처리하도록 하려면 추가 단계가 필요할 수 있습니다. (자세한 내용은 플랫폼별 자습서를 참조하세요.)

## <a name="additional-resources"></a>추가 리소스

- [시각적 계층](/windows/uwp/composition/visual-layer)
- [컴퍼지션 시각적 계층](/windows/uwp/composition/composition-visual-tree)
- [컴퍼지션 브러시](/windows/uwp/composition/composition-brushes)
- [컴퍼지션 효과](/windows/uwp/composition/composition-effects)
- [컴퍼지션 애니메이션](/windows/uwp/composition/composition-animation)

API 참조

- [Windows.UI.Composition](/uwp/api/Windows.UI.Composition)