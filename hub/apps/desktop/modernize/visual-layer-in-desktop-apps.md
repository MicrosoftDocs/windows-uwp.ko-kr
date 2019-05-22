---
title: 시각적 계층을 사용 하 여 데스크톱 앱을 현대화
description: .NET 또는 Win32 데스크톱 앱의 UI를 향상 시키기 위해 시각적 계층을 사용 합니다.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 042e34b8d09c2bae5cefb4227ef2a104a43861a6
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984973"
---
# <a name="using-the-visual-layer-in-desktop-apps"></a>시각적 계층을 사용 하 여 데스크톱 앱에서

이제 UWP Api 비 UWP 데스크톱 응용 프로그램의 모양, 느낌 및 WPF에서 Windows Forms의 기능을 향상 시키기 위해 사용할 수 있습니다 및 C++ Win32 응용 프로그램을 UWP 통해서만 사용할 수 있는 최신 Windows 10 UI 기능을 활용 합니다.

많은 시나리오에서 사용할 수 있습니다 [XAML 제도](xaml-islands.md) 앱에 최신 XAML 컨트롤을 추가 합니다. 그러나 기본 제공 컨트롤 보다 우수한 사용자 지정 환경을 만들 수는 경우 시각적 계층 Api를 액세스할 수 있습니다.

시각적 계층 고성능, 그래픽, 효과 및 애니메이션에 대 한 유지 모드 API를 제공합니다. Windows 10 장치에서 UI에 대 한 기초가 됩니다. 시각적 계층을 기반으로 UWP XAML 컨트롤은 및의 다양 한 측면을 사용 하도록 설정 하는 [Fluent Design System](/windows/uwp/design/fluent-design-system/index)광원, 깊이, 동작, 자료 및 크기 조정 등입니다.

![시각적 계층을 사용 하 여 만든 사용자 인터페이스](images/visual-layer-interop/pull-to-animate.gif)

> _시각적 계층을 사용 하 여 만든 사용자 인터페이스_

## <a name="create-a-visually-engaging-user-interface-in-any-windows-app"></a>모든 Windows 앱에 시각적으로 매력적인 사용자 인터페이스 만들기

시각적 계층을 통해 간단한 합성 그리기 콘텐츠를 사용자 지정 하 고 있습니다. (시각적 개체)를 사용 하 여 응용 프로그램에서 해당 개체에 강력한 애니메이션, 효과 및 조작을 적용 하 여 매력적인 환경을 만들 수 있습니다. 시각적 계층을 대체 하기 위한 모든 기존 UI 프레임 워크입니다. 대신 이러한 프레임 워크에 중요 한 보완 것입니다.

시각적 계층을 사용 하 여 응용 프로그램을 고유한 모양과 느낌을 제공 하 고 다른 응용 프로그램 외에도 설정 하는 id를 설정할 수 있습니다. 또한 사용자의 참여를 그리기 쉽게 응용 프로그램을 사용 하도록 설계 된 Fluent 디자인 원칙을 수 있습니다. 예를 들어, 시각 신호를 만들고 화면에 항목 간의 관계를 보여 주는 애니메이션된 화면 전환을 사용할 수 있습니다.

## <a name="visual-layer-features"></a>시각적 계층 기능

### <a name="brushes"></a>브러시

[컴퍼지션 브러시](/windows/uwp/composition/composition-brushes) 단색, 그라데이션, 이미지, 비디오, 복잡 한 효과 등을 사용 하 여 UI 개체를 그릴 수 있습니다.

![자료의 작성자를 사용 하 여 만든 알](images/visual-layer-interop/egg.gif)

> _사용 하 여 만든 알 합니다 [자료 작성자 데모 앱](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/Demos/MaterialCreator)합니다._

### <a name="effects"></a>효과

[컴퍼지션 효과](/windows/uwp/composition/composition-effects) 광원, 그림자, 및 필터 효과의 목록을 포함 합니다. 이러한 수 수 애니메이션, 사용자 지정 연결을 하 고 시각적 개체에 직접 적용 합니다. SceneLightingEffect atmosphere, 깊이 및 자료를 만들려면 컴퍼지션 조명으로 결합할 수 있습니다.

![광원 및 재질](images/visual-layer-interop/light-interop.gif)

> _에 설명 된 광원 및 재질 합니다 [Windows UI 컴퍼지션 샘플 갤러리](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)합니다._

### <a name="animations"></a>애니메이션

[컴퍼지션 애니메이션](/windows/uwp/composition/composition-animation) UI 스레드의 독립 compositor 프로세스에서 직접 실행 합니다. 이렇게 하면 smoothness 및 크기와 많은 수의 동시, 명시적 애니메이션을 실행할 수 있습니다. 친숙 한 키 프레임 애니메이션 드라이브 속성 변경 내용을 시간이 지남에 따라 외에도 사용자 입력을 포함 하 여 다른 속성 간의 수학적 관계를 설정 하려면 식을 사용할 수 있습니다. 입력된 기반된 애니메이션을 동적으로 및 유동적으로 더 높은 사용자 참여 개선할 수 있는 사용자 입력에 응답 하는 UI를 만들 수 있습니다.

![시각적 계층을 사용 하 여 만든 사용자 인터페이스](images/visual-layer-interop/swipe-scroller.gif)

> _에 설명 된 동작을 [Windows UI 컴퍼지션 샘플 갤러리](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)합니다._

## <a name="keep-your-existing-codebase-and-adopt-incrementally"></a>기존 코드 베이스를 유지 하 고 단계적으로 채택

기존 응용 프로그램의 코드를 손실 하 고 싶지 않은 많은 투자를 나타냅니다. 마이그레이션할 수 있습니다 _제도_ 콘텐츠의 시각적 계층을 사용 하는 기존 프레임 워크에서 UI의 나머지 부분을 유지 합니다. 이에 허용 되는 중요 한 업데이트 및 향상 된 응용 프로그램 UI 기존 코드에 대 한 광범위 한 변경 하지 않고도 기본을 의미 합니다.

## <a name="samples-and-tutorials"></a>샘플 및 자습서

샘플을 사용 하 여 응용 프로그램에서 시각적 계층을 사용 하는 방법에 알아봅니다. 이러한 샘플 및 자습서 도움말 시각적 계층을 사용 하 여 시작 하 고 기능이 작동 하는 방법을 보여 줍니다.

### <a name="win32"></a>Win32

- [시각적 계층을 사용 하 여 win32](using-the-visual-layer-with-win32.md) 자습서
  - [Hello 컴퍼지션 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Hello 벡터 예제](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloVectors)
- [가상 화면 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/VirtualSurfaces)
- [화면 캡처 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/ScreenCaptureforHWND)

### <a name="windows-forms"></a>Windows Forms

- [시각적 계층을 사용 하 여 Windows Forms를 사용 하 여](using-the-visual-layer-with-windows-forms.md) 자습서
  - [Hello 컴퍼지션 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)
- [시각적 계층 통합 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)

### <a name="wpf"></a>WPF

- [시각적 계층을 사용 하 여 WPF를 사용 하 여](using-the-visual-layer-with-wpf.md) 자습서
  - [Hello 컴퍼지션 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition)
- [시각적 계층 통합 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration)
- [화면 캡처 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/ScreenCapture)

## <a name="limitations"></a>제한 사항

다양 한 시각적 계층 기능으로 UWP 앱에서 데스크톱 응용 프로그램에서 호스트 되는 경우 동일 하 게 작동을 하는 동안 일부 기능 제한이 있습니다. 다음은 몇 가지 알아야 할 제한 사항:

- 효과 체인에 의존 [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) 영향 설명 합니다. 합니다 [Win2D NuGet 패키지](https://www.nuget.org/packages/Win2D.uwp) 에서 다시 컴파일을 해야 하도록 데스크톱 응용 프로그램에서 지원 되지 않습니다 합니다 [소스 코드](https://github.com/Microsoft/Win2D)합니다.
- 수행할 적중 횟수 테스트를 직접 시각적 트리를 탐색 하 여 경계 계산을 수행 해야 합니다. 이이 경우에 적중 테스트를 쉽게 바인딩할 수 있습니다 XAML 요소가 없는. 점을 제외 하 고 UWP에서 시각적 계층에서와 동일.
- 시각적 계층에는 텍스트 렌더링에 대 한 기본 형식이 없습니다.
- 두 가지 UI 기술을 함께 사용 하는 경우 예: WPF 및 시각적 계층으로 화면의 픽셀 자체 그리기를 담당 하는 각 되며 픽셀을 공유할 수 없습니다. 결과적으로, 시각적 계층 콘텐츠는 항상 다른 UI 콘텐츠 위에 렌더링 됩니다. (이것은 _어_ 문제.) 추가 코딩을 수행 해야 하 고 시각적 계층에서 사용 하는 내용을 확인 하는 테스트 호스트 UI 사용 하 여 크기를 조정 기타 콘텐츠를 채워집니다 하지 않습니다.
- 데스크톱 응용 프로그램에서 호스팅되는 콘텐츠 자동으로 DPI에 대 한 확장 하거나 크기를 조정 하지 않습니다. DPI 변경 내용을 처리 하는 내용을 확인 하려면 추가 단계가 필요할 수 있습니다. (자세한 정보에 대 한 플랫폼 특정 자습서를 참조 하세요.)

## <a name="additional-resources"></a>추가 리소스

- [시각적 계층](/windows/uwp/composition/visual-layer)
- [시각적 컴퍼지션](/windows/uwp/composition/composition-visual-tree)
- [컴퍼지션 브러시](/windows/uwp/composition/composition-brushes)
- [컴퍼지션 효과](/windows/uwp/composition/composition-effects)
- [컴퍼지션 애니메이션](/windows/uwp/composition/composition-animation)

API 참조

- [Windows.UI.Composition](/uwp/api/Windows.UI.Composition)