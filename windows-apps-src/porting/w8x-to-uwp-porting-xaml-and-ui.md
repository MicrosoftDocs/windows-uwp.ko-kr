---
description: 선언적 XAML 태그 형식으로 UI를 정의하는 방법을 사용하면 유니버설 8.1 앱에서 UWP(유니버설 Windows 플랫폼) 앱으로 매우 원활하게 변환됩니다.
title: Windows 런타임 8.x XAML 및 UI를 UWP로 포팅
ms.assetid: 78b86762-7359-474f-b1e3-c2d7cf9aa907
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7b8bb652c3d8b978d631da2e529662a455310458
ms.sourcegitcommit: ff131135248c85a8a2542fc55437099d549cfaa5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2019
ms.locfileid: "9117853"
---
# <a name="porting-windows-runtime-8x-xaml-and-ui-to-uwp"></a>Windows 런타임 8.x XAML 및 UI를 UWP로 포팅


이전 항목에서는 [문제 해결](w8x-to-uwp-troubleshooting.md)에 대해 살펴보았습니다.

선언적 XAML 태그 형식으로 UI를 정의하는 방법을 사용하면 유니버설 8.1 앱에서 UWP(유니버설 Windows 플랫폼) 앱으로 매우 원활하게 변환됩니다. 사용 중인 시스템 리소스 키 또는 사용자 지정 템플릿을 약간 조정해야 할 수도 있지만 대부분의 태그는 호환됩니다. 보기 모델의 명령적 코드의 경우는 거의 또는 전혀 변경할 필요가 없습니다. UI 요소를 조작하는 프레젠테이션 계층에 있는 코드의 상당 부분 또는 대부분도 간단히 포팅할 수 있습니다.

## <a name="imperative-code"></a>명령적 코드

프로젝트 빌드 단계로 진행하려면 필수가 아닌 코드에 주석이나 설명을 추가할 수 있습니다. 그런 다음 문제를 한 번에 하나씩 반복하고 이 섹션의 다음 항목과 이전 [문제 해결](w8x-to-uwp-troubleshooting.md) 항목을 참조하여 모든 빌드 및 런타임 문제를 해결하고 포팅을 완료합니다.

## <a name="adaptiveresponsive-ui"></a>적응/반응형 UI

앱이 고유한 화면 크기와 해상도가 구현된 광범위한 디바이스에서 실행될 수도 있으므로 앱을 포팅하는 최소한의 단계 이상을 진행하고 앱이 해당 디바이스에서 최상으로 표시되도록 UI를 맞춤화하려고 합니다. 적응 Visual State Manager 기능을 사용하여 동적으로 창 크기를 검색하고 그 결과에 따라 레이아웃을 변경할 수 있으며, 이러한 작업을 수행하는 방법에 대한 예제는 Bookstore2 사례 연구 항목의 [적응 UI](w8x-to-uwp-case-study-bookstore2.md) 섹션에 나와 있습니다.

## <a name="back-button-handling"></a>뒤로 단추 처리

유니버설 8.1 앱, Windows 런타임 8.x 앱 및 Windows Phone 스토어 앱은 다른 접근 방식을 표시 하는 UI와 뒤로 단추의 처리 하는 이벤트입니다. 하지만 windows 10 앱에 대 한 단일 접근 방식을 앱에서 사용할 수 있습니다. 모바일 장치에서는 단추가 장치의 용량성 단추로 또는 셸의 단추로 제공됩니다. 데스크톱 장치에서는 앱 내에서 뒤로 탐색이 가능할 때마다 앱의 크롬에 단추를 추가합니다. 그러면 해당 단추는 태블릿 모드의 작업 표시줄에 또는 창으로 표시된 앱의 제목 표시줄에 표시됩니다. 뒤로 단추 이벤트는 모든 디바이스 패밀리에서 범용 개념이며, 하드웨어 또는 소프트웨어에서 구현된 단추는 동일한 [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596) 이벤트를 발생시킵니다.

아래 예제는 모든 디바이스 패밀리에서 작동하며, 동일한 처리가 모든 페이지에 적용되고 탐색을 확인(예: 저장되지 않은 변경에 대한 경고)할 필요가 없는 경우에 적합합니다.

```csharp
   // app.xaml.cs

    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        [...]

        Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
        rootFrame.Navigated += RootFrame_Navigated;
    }

    private void RootFrame_Navigated(object sender, NavigationEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        // Note: On device families that have no title bar, setting AppViewBackButtonVisibility can safely execute 
        // but it will have no effect. Such device families provide back button UI for you.
        if (rootFrame.CanGoBack)
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Visible;
        }
        else
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Collapsed;
        }
    }

    private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        if (rootFrame.CanGoBack)
        {
            rootFrame.GoBack();
        }
    }
```

또한 모든 디바이스 패밀리에서 프로그래밍 방식으로 앱을 종료하는 단일 접근 방식도 있습니다.

```csharp
   Windows.UI.Xaml.Application.Current.Exit();
```

## <a name="charms"></a>참 메뉴

참와 통합 하는 코드를 변경 하지 않아도 됩니다. 하지만 대신할 UI 앱이 windows 10 셸의 일부가 없는 참 막대의 위치를 추가 해야 합니다. Windows 10에서 실행 되는 유니버설 8.1 앱에는 고유한 대체 UI 앱의 제목 표시줄에는 시스템 렌더링 크롬에서 제공 합니다.

## <a name="controls-and-control-styles-and-templates"></a>컨트롤과 컨트롤 스타일 및 템플릿

Windows 10에서 실행 되는 유니버설 8.1 앱은 8.1 모양 및 컨트롤에 대 한 동작을 유지 합니다. 하지만 해당 앱을 windows 10 앱에 포팅하는 경우 일부의 차이점이 있습니다 모양 및 동작을 고려해 야 합니다. 아키텍처 및 컨트롤의 디자인은 대부분의 디자인 언어, 간소화 및 유용성 향상 주위 변경 기본적으로 windows 10 앱에 대 한 변경 되지 않습니다.

**참고**  PointerOver 시각적 상태는 사용자 지정 스타일/템플릿이 windows 10 앱 및 Windows 런타임 8.x 앱 하지만 Windows Phone 스토어 앱에 없는 합니다. 이러한 이유로 (그리고 때문에 windows 10 앱에 대 한 지원 되는 시스템 리소스 키)를 다시 사용 하는 사용자 지정 스타일/템플릿이 Windows 런타임 8.x 앱에서 앱을 windows 10으로 포팅할 때 하는 것이 좋습니다.
확인 하려는 경우 사용자 지정 스타일/템플릿이 최신 시각적 상태 집합을 사용 하는 및 기본 스타일/템플릿에 수행한 성능 개선 으로부터 이점을 얻도록 다음 새 windows 10 기본 템플릿의 복사본을 편집 하 고 다시 적용 합니다 사용자 지정 합니다. 성능 개선의 한 가지 예는 이전에 **ContentPresenter** 또는 패널을 묶은 **Border**가 제거되었으며 이제 자식 요소가 테두리를 렌더링한다는 점입니다.

다음은 컨트롤에 대한 변경의 더욱 구체적인 몇 가지 예입니다.

| 컨트롤 이름 | 변경 |
|--------------|--------|
| **AppBar**   | **AppBar** 컨트롤이 ([**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) 대신 권장)를 사용 하는 경우 다음 숨겨져 있지 않기 windows 10 앱에서는 기본적으로 합니다. [**AppBar.ClosedDisplayMode**](https://msdn.microsoft.com/library/windows/apps/dn633872) 속성으로 이를 제어할 수 있습니다. |
| **AppBar**, [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) | Windows 10 앱에서는 [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) 및 **AppBar** **볼 수록,** 단추 (줄임표) 해야합니다. |
| [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) | Windows 런타임 8.x 앱에서 [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) 의 보조 명령이 항상 표시 됩니다. Windows Phone 스토어 앱 및 windows 10 앱에서의 명령 모음 열립니다 될 때까지 표시 되지 않습니다. |
| [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) | Windows Phone 스토어 앱의 경우 [**CommandBar.IsSticky**](https://msdn.microsoft.com/library/windows/apps/hh701944) 값은 명령 모음이 빠른 해제가 가능한지 여부에 영향을 주지 않습니다. **IsSticky** true 이면 **CommandBar** 로 설정 된 경우 앱은 windows 10에 대 한 빠른 해제 제스처로 무시 합니다. |
| [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) | Windows 10 앱에서 [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) [**EdgeGesture.Completed**](https://msdn.microsoft.com/library/windows/apps/hh701622) 나 [**UIElement.RightTapped**](https://msdn.microsoft.com/library/windows/apps/br208984) 이벤트를 처리 하지 않습니다. 또한 탭과 위로 살짝 밀기에 응답하지 않습니다. 여전히 이러한 이벤트를 처리하는 옵션이 있다면 [**IsOpen**](https://msdn.microsoft.com/library/windows/apps/hh701939)으로 설정합니다. |
| [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/dn298584), [**TimePicker**](https://msdn.microsoft.com/library/windows/apps/dn299280) | [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/dn298584) 및 [**TimePicker**](https://msdn.microsoft.com/library/windows/apps/dn299280)에 대한 시각적 변경에 따른 앱의 모양 변경을 검토합니다. 모바일 장치에서 실행 되는 windows 10 앱, 이러한 컨트롤이 선택 페이지를 더 이상 이동 않지만 대신 빠른 해제가 가능한 팝업을 사용 합니다. |
| [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/dn298584), [**TimePicker**](https://msdn.microsoft.com/library/windows/apps/dn299280) | Windows 10 앱에서는 플라이 아웃 내부 [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/dn298584) 또는 [**TimePicker**](https://msdn.microsoft.com/library/windows/apps/dn299280) 를 넣을 수 없습니다. 이러한 컨트롤을 팝업 유형의 컨트롤로 표시를 원하는 경우 사용할 수 있습니다 [**DatePickerFlyout**](https://msdn.microsoft.com/library/windows/apps/dn625013) 및 [**TimePickerFlyout**](https://msdn.microsoft.com/library/windows/apps/dn608313)합니다. |
| **GridView**, **ListView** | **GridView**/**ListView**의 경우 [GridView 및 ListView 변경](#gridview-and-listview-changes)을 참조하세요. |
| [**허브**](https://msdn.microsoft.com/library/windows/apps/dn251843) | Windows Phone 스토어 앱에서는 [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843) 컨트롤이 마지막 섹션에서 첫 번째 섹션까지 래핑합니다. Windows 런타임 8.x 앱 및 windows 10 앱에서는 허브 섹션이 래핑하지 않습니다. |
| [**허브**](https://msdn.microsoft.com/library/windows/apps/dn251843) | Windows Phone 스토어 앱에서는 [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843) 컨트롤의 배경 이미지가 허브 섹션을 기준으로 시차 효과를 내면서 이동합니다. Windows 런타임 8.x 앱 및 windows 10 앱에서는 시차가 사용 되지 않습니다. |
| [**허브**](https://msdn.microsoft.com/library/windows/apps/dn251843)  | 유니버설 8.1 앱에서는 [**HubSection.IsHeaderInteractive**](https://msdn.microsoft.com/library/windows/apps/dn251917) 속성으로 섹션 헤더(및 그 옆에서 렌더링된 펼침 단추 문자 모양)가 대화형이 됩니다. Windows 10 앱에는 헤더 옆에 대화형 "자세히" 어포던스가 있지만 헤더 자체가 대화형은 아닙니다. **IsHeaderInteractive**에서 여전히 조작이 [**Hub.SectionHeaderClick**](https://msdn.microsoft.com/library/windows/apps/dn251953) 이벤트를 발생시킬지 여부를 결정합니다. |
| **MessageDialog** | **MessageDialog**를 사용하려면 더욱 유연한 [**ContentDialog**](https://msdn.microsoft.com/library/windows/apps/dn633972)를 사용하는 것이 좋습니다. 또한 [XAML UI 기본 사항](https://go.microsoft.com/fwlink/p/?linkid=619992) 샘플을 참조하세요. |
| **ListPickerFlyout**, **PickerFlyout**  | **ListPickerFlyout** 및 **PickerFlyout** windows 10 앱에 대 한 사용 되지 않습니다. 단일 선택 플라이아웃의 경우 [**MenuFlyout**](https://msdn.microsoft.com/library/windows/apps/dn299030)을 사용하고, 더욱 복잡한 환경의 경우 [**Flyout**](https://msdn.microsoft.com/library/windows/apps/dn279496)을 사용합니다. |
| [**PasswordBox**](https://msdn.microsoft.com/library/windows/apps/br227519) | [**PasswordBox.IsPasswordRevealButtonEnabled**](https://msdn.microsoft.com/library/windows/apps/hh702579) 속성은 windows 10 앱을 중단 하 고 설정 해도 효과가 없습니다. 사용 하 여 [**PasswordBox.PasswordRevealMode**](https://msdn.microsoft.com/library/windows/apps/dn890867) 대신, **미리 보기** (있는 눈 문자 모양이 표시, 같은 Windows 런타임 8.x 앱에서) 기본적으로 설정 됩니다. 또한 [암호 상자에 대한 지침](https://msdn.microsoft.com/library/windows/apps/dn596103)을 참조하세요. |
| [**피벗**](https://msdn.microsoft.com/library/windows/apps/dn608241) | [**Pivot**](https://msdn.microsoft.com/library/windows/apps/dn608241) 컨트롤은 이제 범용이며 더 이상 모바일 디바이스로 사용이 제한되지 않습니다. |
| [**SearchBox**](https://msdn.microsoft.com/library/windows/apps/dn252771) | [**SearchBox**](https://msdn.microsoft.com/library/windows/apps/dn252803)는 범용 디바이스 패밀리에 구현되어 있지만 모바일 디바이스에서 완전히 작동하지 않습니다. [AutoSuggestBox를 위해 더 이상 사용되지 않는 SearchBox](#searchbox-deprecated-in-favor-of-autosuggestbox)를 참조하세요. |
| **SemanticZoom** | **SemanticZoom**에 대해서는 [SemanticZoom 변경](#semanticzoom-changes)을 참조하세요. |
| [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527)  | [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527)의 일부 기본 속성이 변경되었습니다. [**HorizontalScrollMode**](https://msdn.microsoft.com/library/windows/apps/br209549)는 **Auto**이고, [**VerticalScrollMode**](https://msdn.microsoft.com/library/windows/apps/br209589)는 **Auto**이며, [**ZoomMode**](https://msdn.microsoft.com/library/windows/apps/br209601)는 **Disabled**입니다. 새 기본값이 앱에 적합하지 않으면 스타일에서 변경하거나 컨트롤 자체의 로컬 값으로 변경할 수 있습니다.  |
| [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) | Windows 런타임 8.x 앱에서 맞춤법 검사 기본적으로 꺼져 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)에 대 한 합니다. Windows Phone 스토어 앱 및 windows 10 앱에서는 기본적으로 켜져 있습니다. |
| [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) | [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)의 기본 글꼴 크기가 11에서 15로 변경되었습니다. |
| [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) | [**TextBox.TextReadingOrder**](https://msdn.microsoft.com/library/windows/apps/dn252859)의 기본값이 **Default**에서 **DetectFromContent**로 변경되었습니다. 값이 적합하지 않으면 **UseFlowDirection**을 사용합니다. **Default**는 사용되지 않습니다. |
| Various | 테마 컬러는 Windows Phone 스토어 앱 및 windows 10 앱 하지만 Windows 런타임 8.x 앱을 적용합니다.  |

UWP 앱 컨트롤에 대한 자세한 내용은 [기능별 컨트롤](https://msdn.microsoft.com/library/windows/apps/mt185405), [컨트롤 목록](https://msdn.microsoft.com/library/windows/apps/mt185406) 및 [컨트롤에 대한 지침](https://msdn.microsoft.com/library/windows/apps/dn611856)을 참조하세요.

##  <a name="design-language-in-windows10"></a>Windows 10의 디자인 언어

유니버설 8.1 앱과 windows 10 앱 사이의 디자인 언어의 몇 가지 작지만 중요 한 차이점이 있습니다. 자세한 내용은 [디자인](https://developer.microsoft.com/en-us/windows/apps/design)을 참조하세요. 디자인 언어의 변경에도 불구하고 디자인 원칙은 일관되게 유지됩니다. 즉, 세부 정보에 신경을 쓰지만 항상 크롬이 아니라 콘텐츠에 집중하여 간결함을 추구하며, 시각적 요소를 적극적으로 최소화하고, 디지털 도메인에 대한 인증 상태를 유지하며, 특히 입력 체계에 시각적 계층 구조를 사용하며, 그리드에 디자인을 사용하고 유연한 애니메이션을 사용하여 독창적인 사용자 환경을 제공합니다.

## <a name="effective-pixels-viewing-distance-and-scale-factors"></a>유효 픽셀, 가시거리 및 배율 인수

이전에 보기 픽셀은 장치의 실제 물리적 크기와 해상도에서 UI 요소의 크기 및 레이아웃을 추상화하는 방법이었습니다. 이제 보기 픽셀은 유효 픽셀로 발전했으며 해당 용어의 설명, 의미하는 내용 및 이러한 용어가 제공하는 추가 가치는 다음과 같습니다.

“해상도”라는 용어는 일반적으로 생각하는 픽셀 수가 아닌 픽셀 밀도 기준을 의미합니다. “유효 해상도”는 장치의 가시거리와 물리적 픽셀 크기의 차이를 고려하여 이미지 또는 문자 모양을 구성하는 물리적 픽셀 수를 눈으로 확인할 수 있는 방법입니다(픽셀 밀도는 물리적 픽셀 크기의 역). 유효 해상도는 사용자 중심이므로 환경을 구축하는 데 유용한 메트릭입니다. 모든 요소를 이해하고 UI 요소의 크기를 제어하여 유용한 사용자 환경을 구축할 수 있습니다.

장치마다 유효 픽셀 수가 다릅니다. 가장 작은 장치의 경우 320epx에서부터 중간 크기 모니터의 경우 최대 1024epx 이상의 상당한 너비에 이르기까지 다양합니다. 기존과 마찬가지로 자동 크기 조정 요소 및 동적 레이아웃 패널을 계속 사용할 수 있습니다. XAML 태그에서 고정 크기로 UI 요소의 속성을 설정할 수 있는 경우가 몇 가지 있습니다. 배율 인수는 앱이 실행되는 장치와 사용자가 지정한 디스플레이 설정에 따라 앱에 자동으로 적용됩니다. 또한 배율 인수는 고정 크기의 모든 UI 요소를 유지하여 다양한 화면 크기에서 사용자에게 거의 일정한 크기의 터치(및 읽기) 대상을 제공하는 역할을 합니다. 또한 동적 레이아웃과 함께 사용하면 단순히 장치에 따라 UI 크기를 광학적으로 조정하지는 않습니다. 대신 사용 가능한 공간에 적절한 콘텐츠 양을 맞추기 위해 필요한 작업을 수행합니다.

앱이 모든 디스플레이에서 최상의 환경을 제공하므로, 각기 특정 배율 인수에 적합한 여러 가지 크기로 각 비트맵 자산을 만드는 것이 좋습니다. 100%, 200% 및 400% 배율의 자산을 제공(해당하는 우선 순위로)하면 대부분의 경우에 중간 배율에서 좋은 결과가 나옵니다.

**참고**경우 어떤 이유로 든 할 수 없습니다 둘 이상의 크기로 자산을 만들을 100% 배율 자산을 만듭니다. Microsoft Visual Studio에서 UWP 앱의 기본 프로젝트 템플릿은 브랜딩 자산(타일 이미지 및 로고)을 하나의 크기로만 제공하지만 100% 배율은 아닙니다. 고유한 앱의 자산을 작성할 때 이 섹션의 지침에 따라 100%, 200% 및 400% 크기를 제공하고 자산 팩을 사용하세요.

복잡한 아트워크가 있으면 훨씬 더 큰 크기로 자산을 제공할 수 있습니다. 벡터 아트를 시작하면 모든 배율에서 고품질의 자산을 생성하기가 비교적 쉽습니다.

모든 배율 인수를 지원 하도록 하지만 전체 windows 10 앱에 대 한 배율 인수 목록은 100%, 125%, 150%, 200%, 250%, 300% 및 400%는 권장 하지 않습니다. 여러 배율의 자산을 제공하면 스토어에서 각 장치에 맞는 올바른 크기의 자산을 선택하고 해당 자산만 다운로드됩니다. 스토어에서 장치의 DPI에 따라 다운로드할 자산을 선택합니다. 140%, 220% 등의 배율 인수로 Windows 런타임 8.x 앱에서 자산을 다시 사용할 수 있지만 새 배율 인수 중 하나에서 실행 되는 앱 및 되므로 일부 비트맵 크기 조정은 어쩔 수 없습니다. 다양한 장치에서 앱을 테스트하여 결과가 만족스러운지 여부를 확인하세요.

사용할 수도 있습니다 다시 Windows 런타임 8.x 앱에서 XAML 태그 리터럴 차원 값이 태그 (크기 모양 또는 기타 요소, 입력 체계)에 사용 되는 위치입니다. 있지만 경우에 따라 큰 배율 인수가 유니버설 8.1 앱 보다 windows 10 앱에 대 한 장치에서 사용 됩니다 (예를 들어 150%는 여기서 140% 였던 및 200%는 180% 있던 사용 됩니다). 따라서 이러한 리터럴 값은 windows 10에 비해 너무 큰 이제을 찾을 경우 크다고 보세요 0.8로 곱해 야 합니다. 자세한 내용은 [UWP 앱용 반응형 디자인 101](https://msdn.microsoft.com/library/windows/apps/dn958435)을 참조하세요.

## <a name="gridview-and-listview-changes"></a>GridView 및 ListView 변경

컨트롤을 세로로(이전에 기본적으로 가로였던 대신) 스크롤할 수 있도록 하기 위해 [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705)에 대한 기본 스타일 setter에 몇 가지 변경 내용이 적용되었습니다. 프로젝트의 기본 스타일 복사본을 편집하면 복사본에 이러한 변경이 없으므로 수동으로 변경해야 합니다. 변경 목록은 다음과 같습니다.

-   [**ScrollViewer.HorizontalScrollBarVisibility**](https://msdn.microsoft.com/library/windows/apps/br209547)에 대한 setter가 **Auto**에서 **Disabled**로 변경되었습니다.
-   [**ScrollViewer.VerticalScrollBarVisibility**](https://msdn.microsoft.com/library/windows/apps/br209587)에 대한 setter가 **Disabled**에서 **Auto**로 변경되었습니다.
-   [**ScrollViewer.HorizontalScrollMode**](https://msdn.microsoft.com/library/windows/apps/br209549)에 대한 setter가 **Enabled**에서 **Disabled**로 변경되었습니다.
-   [**ScrollViewer.VerticalScrollMode**](https://msdn.microsoft.com/library/windows/apps/br209589)에 대한 setter가 **Disabled**에서 **Enabled**로 변경되었습니다.
-   [**ItemsPanel**](https://msdn.microsoft.com/library/windows/apps/br242826)에 대한 setter에서 [**ItemsWrapGrid.Orientation**](https://msdn.microsoft.com/library/windows/apps/dn298907) 값이 **Vertical**에서 **Horizontal**로 변경되었습니다.

마지막 변경 내용(**Orientation**에 대한 변경)이 모순되는 것처럼 보인다면 래핑 그리드에 대해 이야기한 내용을 기억해 보세요. 가로 방향 래핑 그리드(새 값)는 텍스트가 가로로 표시되고 다음 줄이 아래쪽으로 페이지 끝에서 중단되는 쓰기 시스템과 유사합니다. 세로로 스크롤되는 텍스트의 페이지입니다. 반대로, 세로 방향 래핑 그리드 (이전 값)는 텍스트가 세로로 표시되고 따라서 가로로 스크롤되는 쓰기 시스템과 유사합니다.

다음은 [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705) 의 측면 및 있는 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 변경 또는 windows 10에서 지원 되지 않습니다.

-   [**IsSwipeEnabled**](https://msdn.microsoft.com/library/windows/apps/hh702518) 속성 (Windows 런타임 8.x 앱에만 해당)는 windows 10 앱에 대 한 지원 되지 않습니다. API는 여전히 존재하지만, 효과가 없는 설정입니다. 이전의 모든 선택 제스처는 아래쪽으로 살짝 밀기(데이터가 검색할 수 없다는 것을 표시하므로 지원되지 않음) 및 마우스 오른쪽 단추로 클릭(상황에 맞는 메뉴를 표시하기 위해 예약됨)을 제외하고 지원됩니다.
-   [**ReorderMode**](https://msdn.microsoft.com/library/windows/apps/dn625099) 속성 (Windows Phone 스토어 앱에만 해당)는 windows 10 앱에 대 한 지원 되지 않습니다. API는 여전히 존재하지만, 효과가 없는 설정입니다. 대신 **GridView** 또는 **ListView**에서 [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/br208912) 및 [**CanReorderItems**](https://msdn.microsoft.com/library/windows/apps/br242882)를 true로 설정합니다. 그러면 사용자가 누르고 있기(또는 클릭하고 끌기) 제스처를 사용하여 순서를 바꿀 수 있습니다.
-   Windows 10을 개발할 때 사용 하 여 [**ListViewItemPresenter**](https://msdn.microsoft.com/library/windows/apps/dn298500) [**GridViewItemPresenter**](https://msdn.microsoft.com/library/windows/apps/dn279298) 대신 항목 컨테이너 스타일에 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 및 [**gridview**](https://msdn.microsoft.com/library/windows/apps/br242705). 기본 항목 컨테이너 스타일의 복사본을 편집하면 올바른 형식을 가져올 수 있습니다.
-   선택 시각 효과가 windows 10 앱에 대 한 변경 되었습니다. [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/br242915)를 **Multiple**로 설정하면 기본적으로 확인란이 각 항목에 대해 렌더링됩니다. **ListView** 항목의 기본 설정은 항목 옆 인라인에 확인란을 배치한다는 것이며 결과적으로 항목의 나머지에서 차지하는 공간이 약간 축소되며 이동됩니다. **GridView** 항목의 경우 기본적으로 항목의 위에 확인란을 오버레이합니다. 그러나 어떤 경우에도 확인란의 레이아웃(인라인 또는 오버레이)을 [**CheckMode**](https://msdn.microsoft.com/library/windows/apps/dn913923) 속성으로 제어할 수 있으며, 아래 예제와 같이 항목 컨테이너 스타일 내부의 [**ListViewItemPresenter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.aspx) 요소에서 표시할지 여부를 [**SelectionCheckMarkVisualEnabled**](https://msdn.microsoft.com/library/windows/apps/dn298541) 속성으로 제어할 수 있습니다.
-   Windows 10의 [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/dn298914) 는 이벤트가 항목당 두 UI 가상화 중: 회수에 대해, 한 번 다시 사용 합니다. [**InRecycleQueue**](https://msdn.microsoft.com/library/windows/apps/dn279443) 값이 **true**이고 수행할 특별한 회수 작업이 없는 경우 동일한 항목이 다시 사용될 때(이 경우 **InRecycleQueue**가 **false**) 다시 입력되므로 이벤트 처리기를 즉시 종료할 수 있습니다.

```xml
<Style x:Key="CustomItemContainerStyle" TargetType="ListViewItem|GridViewItem">
    ...
    <Setter.Value>
        <ControlTemplate TargetType="ListViewItem|GridViewItem">
            <ListViewItemPresenter CheckMode="Inline|Overlay" ... />
        </ControlTemplate>
    </Setter.Value>
    ...
</Style>
```

![인라인 확인란이 있는 listviewitempresenter](images/w8x-to-uwp-case-studies/ui-listviewbase-cb-inline.jpg)

인라인 확인란이 있는 ListViewItemPresenter

![확인란이 오버레이된 listviewitempresenter](images/w8x-to-uwp-case-studies/ui-listviewbase-cb-overlay.jpg)

확인란이 오버레이된 ListViewItemPresenter

-   위의 이유로 아래쪽으로 살짝 밀기 및 마우스 오른쪽 단추로 클릭 제스처를 선택 항목에서 제거하면 조작 모델이 변경되었습니다. 그 결과 중 하나로 이제 [**ItemClick**](https://msdn.microsoft.com/library/windows/apps/br242904) 및 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) 이벤트를 함께 사용할 수 있습니다. Windows 10 앱에 대 한 시나리오를 검토 하 고 "선택" 또는 "호출" 조작 모델을 채택할 지 여부를 결정 합니다. 자세한 내용은 [조작 모드를 변경하는 방법](https://msdn.microsoft.com/library/windows/apps/xaml/hh780625)을 참조하세요.
-   [**ListViewItemPresenter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.aspx)의 스타일을 지정하는 데 사용하는 속성이 일부 변경되었습니다. 새로운 속성은 [**CheckBoxBrush**](https://msdn.microsoft.com/library/windows/apps/dn913905), [**PressedBackground**](https://msdn.microsoft.com/library/windows/apps/dn913931), [**SelectedPressedBackground**](https://msdn.microsoft.com/library/windows/apps/dn913937) 및 [**FocusSecondaryBorderBrush**](https://msdn.microsoft.com/library/windows/apps/dn898370)입니다. Windows 10 앱에 대 한 무시 되는 속성은 [**안쪽 여백**](https://msdn.microsoft.com/library/windows/apps/dn424775) ( [**ContentMargin**](https://msdn.microsoft.com/library/windows/apps/dn424773) 를 대신 사용), [**CheckHintBrush**](https://msdn.microsoft.com/library/windows/apps/dn298504), [**CheckSelectingBrush**](https://msdn.microsoft.com/library/windows/apps/dn298506), [**PointerOverBackgroundMargin**](https://msdn.microsoft.com/library/windows/apps/dn424778), [**ReorderHintOffset**](https://msdn.microsoft.com/library/windows/apps/dn298528), [** SelectedBorderThickness**](https://msdn.microsoft.com/library/windows/apps/dn298533), 및 [**SelectedPointerOverBorderBrush**](https://msdn.microsoft.com/library/windows/apps/dn298539)합니다.

다음 표에서는 [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/br242919) 및 [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/hh738501) 컨트롤 템플릿의 시각적 상태 및 시각적 상태 그룹에 대한 변경 내용을 설명합니다.

| 8.1                 |                         | Windows 10        |                     |
|---------------------|-------------------------|-------------------|---------------------|
| CommonStates        |                         | CommonStates      |                     |
|                     | 일반                  |                   | 일반              |
|                     | PointerOver             |                   | PointerOver         |
|                     | Pressed                 |                   | Pressed             |
|                     | PointerOverPressed      |                   | [사용할 수 없음]       |
|                     | 사용 안 함                |                   | [사용할 수 없음]       |
|                     | [사용할 수 없음]           |                   | PointerOverSelected |
|                     | [사용할 수 없음]           |                   | 선택함            |
|                     | [사용할 수 없음]           |                   | PressedSelected     |
| [사용할 수 없음]       |                         | DisabledStates    |                     |
|                     | [사용할 수 없음]           |                   | 사용 안 함            |
|                     | [사용할 수 없음]           |                   | 사용             |
| SelectionHintStates |                         | [사용할 수 없음]     |                     |
|                     | VerticalSelectionHint   |                   | [사용할 수 없음]       |
|                     | HorizontalSelectionHint |                   | [사용할 수 없음]       |
|                     | NoSelectionHint         |                   | [사용할 수 없음]       |
| [사용할 수 없음]       |                         | MultiSelectStates |                     |
|                     | [사용할 수 없음]           |                   | MultiSelectDisabled |
|                     | [사용할 수 없음]           |                   | MultiSelectEnabled  |
| SelectionStates     |                         | [사용할 수 없음]     |                     |
|                     | 선택 취소             |                   | [사용할 수 없음]       |
|                     | 선택되지 않음              |                   | [사용할 수 없음]       |
|                     | UnselectedPointerOver   |                   | [사용할 수 없음]       |
|                     | UnselectedSwiping       |                   | [사용할 수 없음]       |
|                     | 선택               |                   | [사용할 수 없음]       |
|                     | 선택함                |                   | [사용할 수 없음]       |
|                     | SelectedSwiping         |                   | [사용할 수 없음]       |
|                     | SelectedUnfocused       |                   | [사용할 수 없음]       |

사용자 지정 [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/br242919) 또는 [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/hh738501) 컨트롤 템플릿이 있다면 위의 변경 내용을 고려하여 해당 템플릿을 검토합니다. 새 기본 템플릿의 복사본을 편집하고 각각에 사용자 지정을 다시 적용하여 시작하는 것이 좋습니다. 어떠한 이유로 그렇게 할 수 없는 경우 기존 템플릿을 편집한 후 다음과 같은 몇 가지 일반 지침에 따라 작업을 시작할 수 있습니다.

-   새로운 MultiSelectStates 시각적 상태 그룹을 추가합니다.
-   새로운 MultiSelectDisabled 시각적 상태를 추가합니다.
-   새로운 MultiSelectEnabled 시각적 상태를 추가합니다.
-   새로운 DisabledStates 시각적 상태 그룹을 추가합니다.
-   새로운 Enabled 시각적 상태를 추가합니다.
-   CommonStates 시각적 상태 그룹에서 PointerOverPressed 시각적 상태를 제거합니다.
-   Disabled 시각적 상태를 DisabledStates 시각적 상태 그룹으로 이동합니다.
-   새로운 PointerOverSelected 시각적 상태를 추가합니다.
-   새로운 PressedSelected 시각적 상태를 추가합니다.
-   SelectedHintStates 시각적 상태 그룹을 제거합니다.
-   SelectionStates 시각적 상태 그룹에서 Selected 시각적 상태를 CommonStates 시각적 상태 그룹으로 이동합니다.
-   전체 SelectionStates 시각적 상태 그룹을 제거합니다.

## <a name="localization-and-globalization"></a>지역화 및 세계화

UWP 앱 프로젝트의 유니버설 8.1 프로젝트에서 Resources.resw 파일을 다시 사용할 수 있습니다. 파일을 복사한 후 프로젝트에 추가하고 **빌드 작업**을 **PRIResource**로 설정하고 **출력 디렉터리로 복사**를 **복사 안 함**으로 설정합니다. [**ResourceContext.QualifierValues**](https://msdn.microsoft.com/library/windows/apps/br206071)에서는 디바이스 패밀리 리소스 선택 배율에 따라 디바이스 패밀리 관련 리소스를 로드하는 방법에 대해 설명합니다.

## <a name="play-to"></a>재생

[**Windows.Media.PlayTo**](https://msdn.microsoft.com/library/windows/apps/br207025) 네임 스페이스의 Api는 [**Windows.Media.Casting**](https://msdn.microsoft.com/library/windows/apps/dn972568) Api 위해 windows 10 앱에 대 한 사용 되지 않습니다.

## <a name="resource-keys-and-textblock-style-sizes"></a>리소스 키 및 TextBlock 스타일 크기

Windows 10에 대 한 디자인 언어가 발전 하 고 특정 시스템 스타일이 변경 했습니다. 어떤 경우에는 변경된 스타일 속성과 어울리도록 보기의 시각적 디자인을 다시 확인할 수 있습니다.

다른 경우에는 리소스 키가 더 이상 지원되지 않습니다. Visual Studio의 XAML 태그 편집기는 확인할 수 없는 리소스 키에 대한 참조를 강조 표시합니다. 예를 들어 XAML 태그 편집기에서는 스타일 키 `ListViewItemTextBlockStyle`에 대한 참조에 빨간색 구부러진 곡선으로 밑줄을 표시합니다. 문제가 수정되지 않은 경우 해당 키를 에뮬레이터 또는 장치에 배포하려고 하면 앱이 즉시 종료됩니다. 따라서 XAML 태그가 정확한지 유의해야 합니다. Visual Studio는 이러한 문제를 식별하는 데 유용한 도구입니다.

계속 지원되는 키의 경우 디자인 언어의 변경은 일부 스타일에서 설정한 속성이 변경되었음을 의미합니다. 예를 들어 `TitleTextBlockStyle` Windows 런타임 8.x 앱에서 14.667px 및 Windows Phone 스토어 앱에서 18.14px **FontSize** 으로 설정 합니다. 하지만 동일한 스타일 **FontSize** windows 10 앱에서 훨씬 더 큰 24px로 설정 합니다. 디자인 및 레이아웃을 검토하고 올바른 위치에 적절한 스타일을 사용하세요. 자세한 내용은 [글꼴에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh700394.aspx) 및 [UWP 앱 디자인](https://developer.microsoft.com/en-us/windows/apps/design)을 참조하세요.

다음은 더 이상 지원되지 않는 키의 전체 목록입니다.

-   CheckBoxAndRadioButtonMinWidthSize
-   CheckBoxAndRadioButtonTextPaddingThickness
-   ComboBoxFlyoutListPlaceholderTextOpacity
-   ComboBoxFlyoutListPlaceholderTextThemeMargin
-   ComboBoxHighlightedBackgroundThemeBrush
-   ComboBoxHighlightedBorderThemeBrush
-   ComboBoxHighlightedForegroundThemeBrush
-   ComboBoxInlinePlaceholderTextForegroundThemeBrush
-   ComboBoxInlinePlaceholderTextThemeFontWeight
-   ComboBoxItemDisabledThemeOpacity
-   ComboBoxItemHighContrastBackgroundThemeMargin
-   ComboBoxItemMinHeightThemeSize
-   ComboBoxPlaceholderTextBlockStyle
-   ComboBoxPlaceholderTextThemeMargin
-   CommandBarBackgroundThemeBrush
-   CommandBarForegroundThemeBrush
-   ContentDialogButton1HostPadding
-   ContentDialogButton2HostPadding
-   ContentDialogButtonsMinHeight
-   ContentDialogContentLandscapeWidth
-   ContentDialogContentMinHeight
-   ContentDialogDimmingColor
-   ContentDialogTitleMinHeight
-   ControlContextualInfoTextBlockStyle
-   ControlHeaderContentPresenterStyle
-   ControlHeaderTextBlockStyle
-   FlyoutContentPanelLandscapeThemeMargin
-   FlyoutContentPanelPortraitThemeMargin
-   GrabberMargin
-   GridViewItemMargin
-   GridViewItemPlaceholderBackgroundThemeBrush
-   GroupHeaderTextBlockStyle
-   HeaderContentPresenterStyle
-   HighContrastBlack
-   HighContrastWhite
-   HubHeaderCharacterSpacing
-   HubHeaderFontSize
-   HubHeaderMarginThickness
-   HubSectionHeaderCharacterSpacing
-   HubSectionHeaderFontSize
-   HubSectionHeaderMarginThickness
-   HubSectionMarginThickness
-   InlineWindowPlayPauseMargin
-   ItemTemplate
-   LeftFullWindowMargin
-   LeftMargin
-   ListViewEmptyStaticTextBlockStyle
-   ListViewItemContentTextBlockStyle
-   ListViewItemContentTranslateX
-   ListViewItemMargin
-   ListViewItemMultiselectCheckBoxMargin
-   ListViewItemSubheaderTextBlockStyle
-   ListViewItemTextBlockStyle
-   MediaControlPanelAudioThemeBrush
-   MediaControlPanelPhoneVideoThemeBrush
-   MediaControlPanelVideoThemeBrush
-   MediaControlPanelVideoThemeColor
-   MediaControlPlayPauseThemeBrush
-   MediaControlTimeRowThemeBrush
-   MediaControlTimeRowThemeColor
-   MediaDownloadProgressIndicatorThemeBrush
-   MediaErrorBackgroundThemeBrush
-   MediaTextThemeBrush
-   MenuFlyoutBackgroundThemeBrush
-   MenuFlyoutBorderThemeBrush
-   MenuFlyoutLandscapeThemePadding
-   MenuFlyoutLeftLandscapeBorderThemeThickness
-   MenuFlyoutPortraitBorderThemeThickness
-   MenuFlyoutPortraitThemePadding
-   MenuFlyoutRightLandscapeBorderThemeThickness
-   MessageDialogContentStyle
-   MessageDialogTitleStyle
-   MinimalWindowMargin
-   PasswordBoxCheckBoxThemeMargin
-   PhoneAccentBrush
-   PhoneBackgroundBrush
-   PhoneBackgroundColor
-   PhoneBaseBlackColor
-   PhoneBaseHighColor
-   PhoneBaseLowColor
-   PhoneBaseLowSolidColor
-   PhoneBaseMediumHighColor
-   PhoneBaseMediumMidColor
-   PhoneBaseMediumMidSolidColor
-   PhoneBaseMidColor
-   PhoneBaseWhiteColor
-   PhoneBorderThickness
-   PhoneButtonBasePressedForegroundBrush
-   PhoneButtonContentPadding
-   PhoneButtonFontWeight
-   PhoneButtonMinHeight
-   PhoneButtonMinWidth
-   PhoneChromeBrush
-   PhoneChromeColor
-   PhoneControlBackgroundColor
-   PhoneControlDisabledColor
-   PhoneControlForegroundColor
-   PhoneDisabledBrush
-   PhoneDisabledColor
-   PhoneFontFamilyLight
-   PhoneFontFamilySemiBold
-   PhoneForegroundBrush
-   PhoneForegroundColor
-   PhoneHighContrastSelectedBackgroundThemeBrush
-   PhoneHighContrastSelectedForegroundThemeBrush
-   PhoneImagePlaceholderColor
-   PhoneLowBrush
-   PhoneMidBrush
-   PhonePageBackgroundColor
-   PhonePivotLockedTranslation
-   PhonePivotUnselectedItemOpacity
-   PhoneRadioCheckBoxBorderBrush
-   PhoneRadioCheckBoxBrush
-   PhoneRadioCheckBoxCheckBrush
-   PhoneRadioCheckBoxPressedBrush
-   PhoneStrokeThickness
-   PhoneTextHighColor
-   PhoneTextLowColor
-   PhoneTextMidColor
-   PhoneTextOverAccentColor
-   PhoneTouchTargetLargeOverhang
-   PhoneTouchTargetOverhang
-   PivotHeaderItemPadding
-   PlaceholderContentPresenterStyle
-   ProgressBarHighContrastAccentBarThemeBrush
-   ProgressBarIndeterminateRectagleThemeSize
-   ProgressBarRectangleStyle
-   ProgressRingActiveBackgroundOpacity
-   ProgressRingElipseThemeMargin
-   ProgressRingElipseThemeSize
-   ProgressRingTextForegroundThemeBrush
-   ProgressRingTextThemeMargin
-   ProgressRingThemeSize
-   RichEditBoxTextThemeMargin
-   RightFullWindowMargin
-   RightMargin
-   ScrollBarMinThemeHeight
-   ScrollBarMinThemeWidth
-   ScrollBarPanningThumbThemeHeight
-   ScrollBarPanningThumbThemeWidth
-   SliderThumbDisabledBorderThemeBrush
-   SliderTrackBorderThemeBrush
-   SliderTrackDisabledBorderThemeBrush
-   TextBoxBackgroundColor
-   TextBoxBorderColor
-   TextBoxDisabledHeaderForegroundThemeBrush
-   TextBoxFocusedBackgroundThemeBrush
-   TextBoxForegroundColor
-   TextBoxPlaceholderColor
-   TextControlHeaderMarginThemeThickness
-   TextControlHeaderMinHeightSize
-   TextStyleExtraExtraLargeFontSize
-   TextStyleExtraLargePlusFontSize
-   TextStyleMediumFontSize
-   TextStyleSmallFontSize
-   TimeRemainingElementMargin

## <a name="searchbox-deprecated-in-favor-of-autosuggestbox"></a>AutoSuggestBox를 위해 더 이상 사용되지 않는 SearchBox

[**SearchBox**](https://msdn.microsoft.com/library/windows/apps/dn252803)는 범용 디바이스 패밀리에 구현되어 있지만 모바일 디바이스에서 완전히 작동하지 않습니다. 유니버설 검색 환경에 [**AutoSuggestBox**](https://msdn.microsoft.com/library/windows/apps/dn633874)를 사용합니다. 일반적으로 **AutoSuggestBox**를 사용하여 검색 환경을 구현하는 방법은 다음과 같습니다.

사용자가 입력을 시작하면 **UserInput**이라는 이유의 **TextChanged** 이벤트가 발생합니다. 그러면 제안 목록을 채우고 [**AutoSuggestBox**](https://msdn.microsoft.com/library/windows/apps/dn633874)의 **ItemsSource**를 설정합니다. 사용자가 목록을 탐색하면 **SuggestionChosen** 이벤트가 발생합니다(또한 **TextMemberDisplayPath**를 설정한 경우 텍스트 상자가 지정된 속성으로 자동으로 채워짐). 사용자가 Enter 키를 사용하여 선택을 전송하면 **QuerySubmitted** 이벤트가 발생합니다. 이때 해당 제안에 대한 작업을 수행할 수 있습니다(이 경우 지정된 콘텐츠에 대한 세부 정보가 있는 다른 페이지로 이동할 가능성이 가장 큼). **SearchBoxQuerySubmittedEventArgs**의 **LinguisticDetails** 및 **Language** 속성은 더 이상 지원되지 않습니다(이 기능을 지원하는 동등한 API가 있음). **KeyModifiers**도 더 이상 지원되지 않습니다.

[**AutoSuggestBox**](https://msdn.microsoft.com/library/windows/apps/dn633874)는 입력기(IME)도 지원합니다. "찾기" 아이콘을 표시할 수도 있습니다(아이콘 조작 시 **QuerySubmitted** 이벤트가 발생함).

```xml
   <AutoSuggestBox ... >
        <AutoSuggestBox.QueryIcon>
            <SymbolIcon Symbol="Find"/>
        </AutoSuggestBox.QueryIcon>
    </AutoSuggestBox>
```

[AutoSuggestBox 포팅 샘플](https://go.microsoft.com/fwlink/p/?linkid=619996)을 참조하세요.

## <a name="semanticzoom-changes"></a>SemanticZoom 변경

Windows Phone 모델에서 [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601)에 대한 축소 제스처가 그룹 헤더를 탭하거나 클릭하는 것으로 수렴되었습니다(따라서 데스크톱 컴퓨터에서 축소에 해당하는 빼기 단추가 더 이상 표시되지 않음). 이제 모든 장치에서 무료로 동일하고 일관된 동작을 가져옵니다. Windows Phone 모델의 한 가지 표면적인 차이는 축소 보기(점프 목록)가 확대 보기를 오버레이하는 대신 확대 보기를 대체한다는 점입니다. 이러한 이유로 축소 보기에서 모든 반투명 배경을 제거할 수 있습니다.

Windows Phone 스토어 앱에서는 축소 viewexpands thesize 화면을 합니다. Windows 런타임 8.x 앱 및 windows 10 앱에서는 축소 보기의 크기는 **SemanticZoom** 컨트롤의 범위로 제한 됩니다.

Windows Phone 스토어 앱에서 축소 보기 뒤의 콘텐츠(z-순서로)는 축소 보기의 백그라운드에 투명도가 있는 경우 비쳐 보입니다. Windows 런타임 8.x 앱 및 windows 10 앱에서는 아무것도 축소 보기 뒤에 표시 되지 않습니다.

Windows 런타임 8.x 앱에서는 앱이 비활성화 되 고 다시 활성화 때 (표시 되 고) 경우 축소 보기가 사라집니다 하 고 확대 보기 대신 표시 됩니다. Windows Phone 스토어 앱 및 windows 10 앱에서는 축소 보기 계속 표시 됩니다 표시 될 경우.

Windows Phone 스토어 앱 및 windows 10 앱에서는 뒤로 단추를 누를 때 축소 보기가 사라집니다. Windows 런타임 8.x 앱에 대 한 처리가 없는 기본 제공 뒤로 단추, 질문에 게 적용 하지 않습니다.

## <a name="settings"></a>설정

Windows 런타임 8.x **SettingsPane** 클래스 windows 10에 대 한 적합 하지 않습니다. 대신, 설정 페이지를 빌드하는 것 외에도 사용자가 앱 내에서 해당 페이지에 액세스할 수 있는 방법을 제공해야 합니다. 탐색 창에 고정된 마지막 항목으로 최상위 수준에 이 앱 설정 페이지를 표시하는 것이 좋지만 전체 옵션 집합은 다음과 같습니다.

-   탐색 창. 설정이 선택 항목의 탐색 목록에서 마지막 항목이고 맨 아래에 고정되어야 합니다.
-   앱 바/도구 모음(탭 보기 또는 피벗 레이아웃 내). 설정이 앱 바 또는 도구 모음 메뉴 플라이아웃의 마지막 항목이어야 합니다. 설정을 탐색 내 최상위 항목 중 하나로 설정하지 않는 것이 좋습니다.
-   허브. 설정이 메뉴 플라이아웃 내부에 있어야 합니다(허브 레이아웃 내의 도구 모음 메뉴 또는 앱 바 메뉴에서 제공될 수 있음).

또한 설정을 마스터-세부 정보 창 내에 숨기지 않는 것이 좋습니다.

설정 페이지는 앱 창 전체를 채워야 하며, 설정 페이지에도 정보 및 피드백이 있어야 합니다. 설정 페이지의 디자인에 대한 지침은 [앱 설정에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh770544)을 참조하세요.

## <a name="text"></a>텍스트

텍스트(또는 입력 체계)는 UWP 앱의 중요한 측면이며, 포팅하는 동안 보기가 새 디자인 언어와 조화를 이루도록 보기의 시각적 디자인을 다시 검토할 수 있습니다. 다음 그림을 사용하여 사용 가능한 UWP(유니버설 Windows 플랫폼)  **TextBlock** 시스템 스타일을 찾을 수 있습니다. 사용한 WindowsPhone Silverlight 스타일에 해당 하는 것을 찾습니다. 또는 범용 스타일 및 시간별로 WindowsPhone Silverlight 시스템 스타일의 속성을 복사 수 있습니다.

![Windows 10 앱의 시스템 textblock 스타일](images/label-uwp10stylegallery.png) <br/>Windows 10 앱의 시스템 TextBlock 스타일

Windows 런타임 8.x 앱 및 Windows Phone 스토어 앱에서 기본 글꼴 패밀리는 전역 사용자 인터페이스를 사용 합니다. Windows 10 앱에서 기본 글꼴 패밀리는 맑은 고딕입니다. 결과적으로 앱에서 글꼴 메트릭은 다르게 보일 수 있습니다. 8.1 텍스트의 모양을 재현하려는 경우 [**LineHeight**](https://msdn.microsoft.com/library/windows/apps/br209671) 및 [**LineStackingStrategy**](https://msdn.microsoft.com/library/windows/apps/br244362)와 같은 속성을 사용하여 고유한 메트릭을 설정할 수 있습니다.

텍스트에 대 한 기본 언어는 빌드의 언어 또는 en Windows 런타임 8.x 앱 및 Windows Phone 스토어 앱에서 설정-주세요. Windows 10 앱에서는 기본 언어 (글꼴 대체) 상위 앱 언어로 설정 됩니다. [**FrameworkElement.Language**](https://msdn.microsoft.com/library/windows/apps/hh702066)를 명시적으로 설정할 수 있지만, 해당 속성에 대해 값을 설정하지 않은 경우 더 나은 글꼴 대체 동작을 사용할 수 있습니다.

자세한 내용은 [글꼴에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh700394.aspx) 및 [UWP 앱 디자인](https://go.microsoft.com/fwlink/p/?LinkID=533896)을 참조하세요. 또한 텍스트 컨트롤에 대한 변경은 위의 [컨트롤](#controls-and-control-styles-and-templates) 섹션을 참조하세요.

## <a name="theme-changes"></a>테마 변경

유니버설 8.1 앱의 경우 기본 테마는 기본적으로 어둡습니다. Windows 10 장치에 대 한 기본 테마를 변경 되었지만 App.xaml에서 요청된 된 테마를 선언 하 여 사용 되는 테마를 제어할 수 있습니다. 예를 들어 모든 디바이스에서 어두운 테마를 사용하려면 루트 응용 프로그램 요소에 `RequestedTheme="Dark"`를 추가합니다.

## <a name="tiles-and-toasts"></a>타일 및 알림

타일 및 알림의 현재 사용 중인 템플릿이 windows 10 앱에서 작업을 계속 합니다. 그렇지만 사용할 수 있는 새로운 조정 가능 템플릿이 있으며 이러한 템플릿은 [알림, 타일 및 배지](https://msdn.microsoft.com/library/windows/apps/mt185606)에 설명되어 있습니다.

이전에 데스크톱 컴퓨터에서는 알림 메시지가 일시적인 메시지였습니다. 놓치거나 무시해버리면 사라져서 더 이상 검색 가능하지 않았습니다. Windows Phone에서는 알림 메시지를 무시하거나 일시적으로 해제하는 경우 알림 센터로 이동됩니다. 이제 알림 센터는 더 이상 모바일 디바이스 패밀리로 제한되지 않습니다.

알림 메시지를 보내기 위해 더 이상 기능을 선언해야 할 필요가 없습니다.

## <a name="window-size"></a>창 크기

유니버설 8.1 앱의 경우 [**ApplicationView**](https://msdn.microsoft.com/library/windows/apps/dn391667) 앱 매니페스트 요소가 최소 창 너비를 선언하는 데 사용됩니다. UWP 앱에서 명령 코드로 최소 크기(너비 및 높이)를 지정할 수 있습니다. 기본 최소 크기는 500x320epx이며, 이 크기는 허용되는 가장 작은 최소 크기이기도 합니다. 허용되는 가장 큰 최소 크기는 500x500epx입니다.

```csharp
   Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetPreferredMinSize
        (new Size { Width = 500, Height = 500 });
```

다음 항목은 [I/O, 장치 및 앱 모델에 대한 포팅](w8x-to-uwp-input-and-sensors.md)입니다.

