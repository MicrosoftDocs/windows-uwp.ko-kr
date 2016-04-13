---
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: 시각적 계층
description: Windows.UI.Composition API는 프레임워크 계층(XAML)과 그래픽 계층(DirectX) 간의 컴퍼지션 계층에 대한 액세스를 제공합니다.
---
# 시각적 계층

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

Windows 10에서는 모든 Windows 응용 프로그램(데스크톱 또는 모바일)에 사용할 수 있는 새로운 통합 작성자 및 렌더링 엔진을 만들기 위해 중요한 작업이 수행되었습니다. 이러한 작업 결과 Windows.UI.Composition이라는 통합 컴퍼지션 WinRT API가 개발되었으며 이 API는 새 작성기 기반의 애니메이션 및 효과와 함께 새로운 경량 컴퍼지션 개체에 대한 액세스를 제공합니다.

Windows.UI.Composition은 모든 UWP(유니버설 Windows 플랫폼) 응용 프로그램에서 호출하여 컴퍼지션 개체, 애니메이션 및 효과를 응용 프로그램에 직접 만들 수 있는 선언적 [유지 모드](https://msdn.microsoft.com/library/windows/desktop/ff684178.aspx) API입니다. 이 API는 XAML 등의 기존 프레임워크에 대한 강력한 보완 제품으로서 UWP 응용 프로그램 개발자에게 응용 프로그램에 추가할 수 있는 친숙한 C# 표면을 제공합니다. 이러한 API를 사용하면 프레임워크가 없는 DX 스타일의 응용 프로그램을 만들 수 있습니다.

XAML 개발자는 사용자 지정 UI 작업을 수행할 때마다 그래픽 계층까지 "끌어 놓고" DirectX 및 C++를 사용하는 대신, C#의 컴퍼지션 계층에 끌어 놓고 컴퍼지션 계층에서 WinRT를 사용하여 사용자 지정 작업을 수행함으로써 XAML 응용 프로그램 개체의 "컴퍼지션 섬"을 만들 수 있습니다.

![](images/layers-win-ui-composition.png)
## <span id="Composition_Objects_and_The_Compositor"> </span> <span id="composition_objects_and_the_compositor"> </span> <span id="COMPOSITION_OBJECTS_AND_THE_COMPOSITOR"> </span>컴퍼지션 개체 및 작성자

컴퍼지션 개체는 컴퍼지션 개체에 대한 팩터리 역할을 하는 [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789)에 의해 만들어집니다. 작성자는 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 개체를 만들어 API의 모든 다른 기능 및 컴퍼지션 개체가 사용하고 빌드되는 시각적 트리 구조를 만들 수 있습니다.

API를 사용하면 개발자가 시각적 트리의 단일 노드를 나타내는 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 개체를 하나 또는 여러 개 정의하고 만들 수 있습니다.

시각적 개체는 다른 시각적 개체의 컨테이너가 되거나 콘텐츠 시각적 개체를 호스트할 수 있습니다. API를 사용하면 계층의 특정 작업에 대해 명확한 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 개체 집합을 제공하여 사용하기 쉽게 만들 수 있습니다.

-   [
            **Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) – 기준 개체입니다. 대부분의 속성은 여기에 있으며 다른 시각적 개체에 상속됩니다.
-   [
            **ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810) – [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)에서 파생되며 자식 시각적 개체를 삽입할 수 있는 기능을 추가합니다.
-   [
            **SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) – [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810)에서 파생되며 이미지, 효과 및 swapchain 형태의 콘텐츠를 포함합니다.
-   [
            **Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) – 응용 프로그램과 시스템 작성자 프로세스 간의 관계를 관리하는 개체 팩터리입니다.

작성자는 트리의 시각적 개체를 클리핑 또는 변환하는 데 사용되는 많은 컴퍼지션 개체뿐 아니라 다양한 애니메이션 및 효과 집합의 팩터리이기도 합니다.

## <span id="Effects_System"> </span> <span id="effects_system"> </span> <span id="EFFECTS_SYSTEM"> </span>효과 시스템

Windows.UI.Composition은 애니메이션 효과 지정, 사용자 지정 및 연결할 수 있는 실시간 효과를 지원합니다. 효과에는 2D 아핀 변형, 산술 합성, 혼합, 색 소싱, 합성, 대비, 노출, 회색조, 감마 전달, 색상 회전, 반전, 범위 제한, 세피아, 온도 및 색조가 포함됩니다.

자세한 내용은 [컴퍼지션 효과](composition-effects.md) 개요를 참조하세요.

## <span id="Animation_System"> </span> <span id="animation_system"> </span> <span id="ANIMATION_SYSTEM"> </span>애니메이션 시스템

Windows.UI.Composition에는 키 프레임 애니메이션과 표현 애니메이션을 설정할 수 있는 표현적, 프레임워크 독립적 애니메이션 시스템이 포함되어 있습니다. 이러한 시스템은 시각적 개체 이동, 변형 또는 클리핑 수행 또는 효과 애니메이션 등에 사용됩니다. 작성자 프로세스에서 직접 실행하여 부드러움과 배율을 조정함으로써 많은 양의 고유 애니메이션을 동시에 실행할 수 있습니다.

자세한 내용은 [컴퍼지션 애니메이션](composition-animation.md) 개요를 참조하세요.

## <span id="XAML_Interoperation"> </span> <span id="xaml_interoperation"> </span> <span id="XAML_INTEROPERATION"> </span>XAML 상호 운용

처음부터 시각적 트리를 만드는 방법 외에 컴퍼지션 API에서는 [**Windows.UI.Xaml.Hosting**](https://msdn.microsoft.com/library/windows/apps/Hh701908)의 [**ElementCompositionPreview**](https://msdn.microsoft.com/library/windows/apps/Mt608976) 클래스를 사용하여 기존 XAML UI를 상호 운용할 수 있습니다.


**참고**  
이 문서는 UWP(유니버설 Windows 플랫폼) 앱을 작성하는 Windows 10 개발자용입니다. Windows 8.x 또는 Windows Phone 8.x를 개발하는 경우 [보관된 문서](http://go.microsoft.com/fwlink/p/?linkid=619132)를 참조하세요.

 

## <span id="Additional_Resources_"> </span> <span id="additional_resources_"> </span> <span id="ADDITIONAL_RESOURCES_"> </span>추가 리소스:

-   이 API에 대한 Kenny Kerr의 MSDN 문서 [Graphics and Animation - Windows Composition Turns 10](https://msdn.microsoft.com/magazine/mt590968)(그래픽 및 애니메이션 - Windows 컴퍼지션(Windows 10))을 참조하세요.
-   [컴퍼지션 GitHub](https://github.com/Microsoft/composition)의 컴퍼지션 샘플
-   [
            **API에 대한 전체 참조 설명서**](https://msdn.microsoft.com/library/windows/apps/Dn706878).
-   알려진 문제: [알려진 문제](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues)

 

 






<!--HONumber=Mar16_HO1-->


