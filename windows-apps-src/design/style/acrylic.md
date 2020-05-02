---
description: 반투명 질감을 만드는 유형의 브러시입니다.
title: 아크릴 소재
template: detail.hbs
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: rybick
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9739933f9fd23c6f169c24c4f789e53ba894708d
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80696626"
---
# <a name="acrylic-material"></a>아크릴 소재

![영웅 이미지](images/header-acrylic.svg)

아크릴은 반투명 질감을 만드는 유형의 [브러시](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Brush)입니다. 앱 표면에 아크릴을 적용하여 깊이를 추가하고 시각적 계층을 설정할 수 있습니다.  <!-- By allowing user-selected wallpaper or colors to shine through, acrylic keeps users in touch with the OS personalization they've chosen. -->

> **중요 API**: [AcrylicBrush 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.acrylicbrush), [Background 속성](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.Background)

:::row:::
    :::column:::
밝은 테마의 아크릴 ![밝은 테마의 아크릴](images/Acrylic_LightTheme_Base.png)
    :::column-end:::
    :::column:::
어두운 테마의 아크릴 ![어두운 테마의 아크릴](images/Acrylic_DarkTheme_Base.png)
    :::column-end:::
:::row-end:::

## <a name="acrylic-and-the-fluent-design-system"></a>아크릴 및 Fluent 디자인 시스템

 Fluent 디자인 시스템을 사용하면 조명, 깊이, 움직임, 재질 및 배율이 통합된 선명한 현대식 UI를 만들 수 있습니다. 아크릴은 앱에 물리적 질감(소재)과 깊이를 추가하는 Fluent 디자인 시스템 구성 요소입니다. 자세한 내용은 [UWP용 Fluent 디자인 개요](/windows/apps/fluent-design-system)를 참조하세요.

 ## <a name="video-summary"></a>비디오 요약

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev002/player]

## <a name="examples"></a>예

:::row:::
    :::column span:::
![일부 이미지](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    :::column span="2":::
**XAML 컨트롤 갤러리**<br>
XAML 컨트롤 갤러리 앱이 설치된 경우 <a href="xamlcontrolsgallery:/item/Acrylic">여기</a>를 클릭하여 앱을 열고 아크릴이 어떻게 작동하는지 살펴봅니다.

<a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a><br>
<a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="acrylic-blend-types"></a>아크릴 혼합 유형
아크릴의 가장 눈에 띄는 특징은 투명도입니다. 소재를 통과하여 보이는 것을 변경하는 두 가지 아크릴 혼합 유형이 있습니다.
 - **배경 아크릴**은 현재 활성 앱의 뒤에 있는 바탕 화면 배경 화면 및 다른 창을 표시하여 애플리케이션 창 사이에 깊이를 추가하고 사용자의 개인 기본 설정을 반영합니다.
 - **인앱 아크릴**은 앱 프레임 내부에 깊이감을 더하여 포커스와 계층을 모두 제공합니다.

 ![배경 아크릴](images/BackgroundAcrylic_DarkTheme.png)

 ![인앱 아크릴](images/AppAcrylic_DarkTheme.png)

 신중하게 아크릴 표면을 레이어로 쌓아 올립니다. 여러 겹의 배경 아크릴은 시야를 방해하는 착시 현상을 유발할 수 있습니다.

## <a name="when-to-use-acrylic"></a>아크릴을 사용하는 경우

* 스크롤 또는 상호 작용할 때 콘텐츠가 겹칠 수 있는 표면 같은 UI를 지원하려면 인앱 아크릴을 사용합니다.
* 팝업 메뉴, 플라이아웃, 빠르게 해제할 수 있는 UI 같은 일시적인 UI 요소에는 배경 아크릴을 사용합니다.<br />일시적인 시나리오에 아크릴을 사용하면 일시적인 UI를 트리거한 콘텐츠와의 시각적 관계를 유지하는 데 도움이 됩니다.

탐색 화면에서 인앱 아크릴을 사용하려는 경우 앱 흐름을 개선할 수 있도록 콘텐츠를 아크릴 창 아래로 확장하는 방안을 고려해 보세요. NavigationView를 사용하면 자동으로 확장됩니다. 그러나 스트라이프 효과를 방지하려면 여러 아크릴의 가장자리가 맞닿게 배치하지 마세요. 이렇게 배치하면 흐릿한 두 표면 사이에 원치 않는 이음새가 생길 수 있습니다. 아크릴은 디자인의 시각적 조화를 위한 도구지만, 잘못 사용할 경우 시각적 혼란을 일으킬 수 있습니다.

다음 사용 패턴을 고려하여 앱에 아크릴을 통합하는 가장 좋은 방법을 결정하세요.

### <a name="vertical-panes"></a>세로 창

앱의 콘텐츠를 분할할 수 있는 세로 창 또는 화면의 경우 아크릴 대신 불투명 배경을 사용하는 것이 좋습니다. NavigationView의 **컴팩트** 또는 **최소** 모드처럼 세로 창이 콘텐츠 위에서 열리는 경우 인앱 아크릴을 사용하여 사용자가 이 창을 열었을 때 페이지의 컨텍스트를 유지하는 것이 좋습니다.

### <a name="transient-surfaces"></a>일시적인 화면

메뉴 플라이아웃, 비 모달 팝업 또는 빠른 해제 창이 있는 앱에는 배경 아크릴을 사용하는 것이 좋습니다.

![정보 플라이아웃을 사용하는 메일 앱 패턴](images/Mail_TransientContextMenu.png)

수많은 컨트롤이 기본적으로 아크릴을 사용합니다. [MenuFlyouts](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus), [AutoSuggestBox](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/auto-suggest-box), [ComboBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox) 및 빠른 해제 팝업이 있는 유사 컨트롤은 호출될 때 일시적인 아크릴을 사용합니다.

> [!Note]
> 아크릴 표면을 렌더링하는 작업은 GPU를 많이 소모하며, 이로 인해 디바이스 전력 사용량이 증가하고 배터리 사용 시간이 단축될 수 있습니다. 디바이스가 배터리 절약 모드로 전환되면 아크릴 효과가 자동으로 해제되며, 사용자는 원한다면 모든 앱의 아크릴 효과를 비활성화할 수 있습니다.

## <a name="usability-and-adaptability"></a>유용성 및 적응성
아크릴은 다양한 디바이스 및 컨텍스트에 맞춰 자동으로 모양을 바꿉니다.

고대비 모드에서는 아크릴 대신 사용자가 선택한 익숙한 배경색이 계속 표시됩니다. 또한 다음과 같은 경우 배경 아크릴과 인앱 아크릴이 모두 단색으로 표시됩니다.
 - 사용자가 설정 > 개인 설정 > 색에서 투명도를 해제하는 경우
 - 배터리 절약 모드가 활성화된 경우
 - 앱이 저사양 하드웨어에서 실행되는 경우

뿐만 아니라 다음과 같은 경우 오직 배경 아크릴만이 투명도 및 질감을 단색으로 대체합니다.
 - 바탕 화면에서 앱 창이 비활성화되는 경우
 - UWP 앱이 휴대폰, Xbox, HoloLens 또는 태블릿 모드에서 실행되는 경우

### <a name="legibility-considerations"></a>가독성 고려 사항
앱에서 사용자에게 표시하는 모든 텍스트는 [명암비를 충족](../accessibility/accessible-text-requirements.md)해야 합니다. Microsoft는 하이 컬러 검은색, 흰색 또는 심지어 미디엄 컬러 회색 텍스트까지 아크릴 위에서 명암비를 충족하도록 아크릴 제작법을 최적화했습니다. 플랫폼에서 제공하는 테마 리소스는 불투명도 80%의 대비 색조 색이 기본값입니다. 아크릴에 하이 컬러 본문 텍스트를 배치하면 가독성을 유지하면서도 색조 불투명도를 낮출 수 있습니다. 어둠 모드에서는 색조 불투명도를 70%로 할 수 있고, 밝음 모드 아크릴은 불투명도 50%에서 명암비를 충족합니다.

테마 컬러 텍스트를 아크릴 표면에 배치하지 않는 것이 좋습니다. 이러한 조합은 15픽셀 글꼴 크기에서 최소 명암비 요구 사항을 통과하지 못할 가능성이 높기 때문입니다. 되도록이면 아크릴 요소 위에 [하이퍼링크](../controls-and-patterns/hyperlinks.md)를 배치하지 마세요. 또한 아크릴 색조 색 또는 불투명도 수준을 테마 리소스에서 제공하는 플랫폼 기본값 범위를 벗어나서 사용자 지정하려는 경우 가독성에 미치는 영향을 염두에 두어야 합니다.

## <a name="acrylic-theme-resources"></a>아크릴 테마 리소스
새로운 XAML AcrylicBrush 또는 미리 정의된 AcrylicBrush 테마 리소스를 사용하여 간편하게 앱 표면에 아크릴을 적용할 수 있습니다. 먼저 인앱 아크릴 또는 배경 아크릴 중에 무엇을 사용할지 결정해야 합니다. 이 문서의 앞부분에서 추천 사항으로 설명한 공통 앱 패턴을 검토하세요.

앱 테마를 준수하고 필요한 경우 단색으로 대체하는 배경 및 인앱 아크릴 유형을 위한 브러시 리소스 모음을 만들어 두었습니다. *AcrylicWindow* 리소스는 배경 아크릴을 나타내고 *AcrylicElement*는 인앱 아크릴을 나타냅니다.

<table>
    <tr>
        <th align="center">리소스 키</th>
        <th align="center">색조 불투명도</th>
        <th align="center"><a href="color.md">대체 색</a> </th>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowBrush, SystemControlAcrylicElementBrush <br/> SystemControlChromeLowAcrylicWindowBrush, SystemControlChromeLowAcrylicElementBrush <br/> SystemControlBaseHighAcrylicWindowBrush, SystemControlBaseHighAcrylicElementBrush <br/> SystemControlBaseLowAcrylicWindowBrush, SystemControlBaseLowAcrylicElementBrush <br/> SystemControlAltHighAcrylicWindowBrush, SystemControlAltHighAcrylicElementBrush <br/> SystemControlAltLowAcrylicWindowBrush, SystemControlAltLowAcrylicElementBrush </td>
        <td align="center"> 80% </td>
        <td> ChromeMedium <br/> ChromeLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltHigh <br/><br/> AltLow </td>
    </tr>
    </tr>
        <td> <b>권장 사용법:</b> 다음은 다양한 사용법에서 정상 작동하는 범용 아크릴 리소스입니다. 앱에서 텍스트 크기가 18픽셀보다 작은 AltMedium 컬러 보조 텍스트를 사용하는 경우 <a href="../accessibility/accessible-text-requirements.md">명암비 요구 사항을 충족</a>하도록 80% 아크릴 리소스를 텍스트 뒤에 배치합니다. </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowMediumHighBrush, SystemControlAcrylicElementMediumHighBrush <br/> SystemControlBaseHighAcrylicWindowMediumHighBrush, SystemControlBaseHighAcrylicElementMediumHighBrush </td>
        <td align="center"> 70% </td>
        <td> ChromeMedium <br/><br/> BaseHigh </td>
    </tr>
    <tr>
        <td> <b>권장 사용법:</b> 앱에서 텍스트 크기가 18픽셀 이상인 AltMedium 컬러 보조 텍스트를 사용하는 경우 보다 반투명한 70% 아크릴 리소스를 텍스트 뒤에 배치할 수 있습니다. 앱의 상단 가로 탐색 및 명령 영역에 이러한 리소스를 사용하는 것이 좋습니다.  </td>
    </tr>
    <tr>
        <td> SystemControlChromeHighAcrylicWindowMediumBrush, SystemControlChromeHighAcrylicElementMediumBrush <br/> SystemControlChromeMediumAcrylicWindowMediumBrush, SystemControlChromeMediumAcrylicElementMediumBrush <br/> SystemControlChromeMediumLowAcrylicWindowMediumBrush, SystemControlChromeMediumLowAcrylicElementMediumBrush <br/> SystemControlBaseHighAcrylicWindowMediumBrush, SystemControlBaseHighAcrylicElementMediumBrush <br/> SystemControlBaseMediumLowAcrylicWindowMediumBrush, SystemControlBaseMediumLowAcrylicElementMediumBrush <br/> SystemControlAltMediumLowAcrylicWindowMediumBrush, SystemControlAltMediumLowAcrylicElementMediumBrush  </td>
        <td align="center"> 60% </td>
        <td> ChromeHigh <br/><br/> ChromeMedium <br/><br/> ChromeMediumLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltMediumLow </td>
    </tr>
    <tr>
        <td> <b>권장 사용법:</b> 아크릴 위에 AltHigh 색의 기본 텍스트만 배치할 경우 앱에서 이러한 60% 리소스를 활용할 수 있습니다. 앱의 <a href="../controls-and-patterns/navigationview.md">세로 탐색 창</a>, 다시 말해서 햄버거 메뉴는 60% 아크릴로 그리는 것이 좋습니다. </td>
    </tr>
</table>

색 중립적 아크릴 외에도 사용자 지정된 테마 컬러를 사용하여 아크릴에 색조를 넣는 리소스도 추가되었습니다. 색이 있는 아크릴은 되도록이면 적게 사용하는 것이 좋습니다. 제공된 dark1 및 dark2 변형의 경우 어두운 테마 텍스트 색과 일치하는 흰색 또는 밝은 색 텍스트를 이러한 리소스 위에 배치합니다.
<table>
    <tr>
        <th align="center">리소스 키</th>
        <th align="center">색조 불투명도</th>
        <th align="center"><a href="color.md">색조 및 대체 색</a> </th>
    </tr>
    <tr>
        <td> SystemControlAccentAcrylicWindowAccentMediumHighBrush, SystemControlAccentAcrylicElementAccentMediumHighBrush  </td>
        <td align="center"> 70% </td>
        <td> SystemAccentColor </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark1AcrylicWindowAccentDark1Brush, SystemControlAccentDark1AcrylicElementAccentDark1Brush  </td>
        <td align="center"> 80% </td>
        <td> SystemAccentColorDark1 </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark2AcrylicWindowAccentDark2MediumHighBrush, SystemControlAccentDark2AcrylicElementAccentDark2MediumHighBrush  </td>
        <td align="center"> 70% </td>
        <td> SystemAccentColorDark2 </td>
    </tr>
</table>


특정 표면을 그리려면 다른 브러시 리소스를 적용할 때처럼 위의 테마 리소스 중 하나를 요소 배경에 적용합니다.

```xaml
<Grid Background="{ThemeResource SystemControlAcrylicElementBrush}">
```

## <a name="custom-acrylic-brush"></a>사용자 지정 아크릴 브러시
브랜딩을 표시하거나 페이지의 다른 요소와 시각적으로 균형을 맞추기 위해 앱의 아크릴에 컬러 색조를 추가할 수 있습니다. 회색조 대신 컬러를 표시하려면 다음 속성을 사용하여 사용자 고유의 아크릴 브러시를 정의해야 합니다.
 - **TintColor**: 색/색조 오버레이 계층입니다. RGB 컬러 값과 알파 채널 불투명도를 모두 지정하는 방안을 고려해 보세요.
 - **TintOpacity**: 색조 계층의 불투명도입니다. 반투명도가 다른 여러 색을 사용하면 좀 더 멋지게 보일 수 있지만, 처음에는 80% 불투명도를 권장합니다.
 - **TintLuminosityOpacity**: 배경의 아크릴 화면을 통해 허용되는 채도의 양을 제어합니다.
 - **BackgroundSource**: 배경 아크릴과 인앱 아크릴 중에서 원하는 것을 지정하는 플래그입니다.
 - **FallbackColor**: 배터리 절약 모드에서 아크릴을 대체하는 단색입니다. 배경 아크릴의 경우 앱이 활성 데스크톱 창에 없거나 앱이 휴대폰 및 Xbox에서 실행 중인 경우 대체 색이 아크릴까지 대체합니다.

![밝은 테마 아크릴 견본](images/CustomAcrylic_Swatches_LightTheme.png)

![어두운 테마 아크릴 견본](images/CustomAcrylic_Swatches_DarkTheme.png)

![광도 불투명도와 색조 불투명도의 비교](images/LuminosityVersusTint.png)

아크릴 브러시를 추가하려면 어두운 테마, 밝은 테마, 고대비 테마에 대한 세 가지 리소스를 정의해야 합니다. 고대비에서는 x:Key가 어두운/밝은 AcrylicBrush와 동일한 SolidColorBrush를 사용하는 것이 좋습니다.

> [!Note]
> TintLuminosityOpacity 값을 지정하지 않으면 시스템에서 TintColor 및 TintOpacity에 따라 이 값을 자동으로 조정합니다.

```xaml
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
        <AcrylicBrush x:Key="MyAcrylicBrush"
            BackgroundSource="HostBackdrop"
            TintColor="#FFFF0000"
            TintOpacity="0.8"
            TintLuminosityOpacity="0.5"
            FallbackColor="#FF7F0000"/>
    </ResourceDictionary>

    <ResourceDictionary x:Key="HighContrast">
        <SolidColorBrush x:Key="MyAcrylicBrush"
            Color="{ThemeResource SystemColorWindowColor}"/>
    </ResourceDictionary>

    <ResourceDictionary x:Key="Light">
        <AcrylicBrush x:Key="MyAcrylicBrush"
            BackgroundSource="HostBackdrop"
            TintColor="#FFFF0000"
            TintOpacity="0.8"
            TintLuminosityOpacity="0.5"
            FallbackColor="#FFFF7F7F"/>
    </ResourceDictionary>
</ResourceDictionary.ThemeDictionaries>
```

다음 샘플은 코드에서 AcrylicBrush를 선언하는 방법을 보여줍니다. 앱에서 여러 OS 대상을 지원하는 경우 사용자의 머신에서 이 API를 사용할 수 있는지 확인해야 합니다.

```csharp
if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.UI.Xaml.Media.AcrylicBrush"))
{
    Windows.UI.Xaml.Media.AcrylicBrush myBrush = new Windows.UI.Xaml.Media.AcrylicBrush();
    myBrush.BackgroundSource = Windows.UI.Xaml.Media.AcrylicBackgroundSource.HostBackdrop;
    myBrush.TintColor = Color.FromArgb(255, 202, 24, 37);
    myBrush.FallbackColor = Color.FromArgb(255, 202, 24, 37);
    myBrush.TintOpacity = 0.6;

    grid.Fill = myBrush;
}
else
{
    SolidColorBrush myBrush = new SolidColorBrush(Color.FromArgb(255, 202, 24, 37));

    grid.Fill = myBrush;
}
```

## <a name="extend-acrylic-into-the-title-bar"></a>아크릴을 제목 표시줄로 확장

앱의 창이 끊김 없이 보이게 하려면 제목 표시줄 영역에 아크릴을 사용하면 됩니다. 다음 예제에서는 [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar) 개체의 [ButtonBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonBackgroundColor) 및 [ButtonInactiveBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonInactiveBackgroundColor) 속성을 [Colors.Transparent](https://docs.microsoft.com/uwp/api/Windows.UI.Colors.Transparent)로 설정하여 아크릴을 제목 표시줄로 확장합니다.

```csharp
private void ExtendAcrylicIntoTitleBar()
{
    CoreApplication.GetCurrentView().TitleBar.ExtendViewIntoTitleBar = true;
    ApplicationViewTitleBar titleBar = ApplicationView.GetForCurrentView().TitleBar;
    titleBar.ButtonBackgroundColor = Colors.Transparent;
    titleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
}
```

여기서 보여드리는 것처럼, [Window.Activate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.Activate)를 호출한 후 이 코드를 앱의 [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) 메서드(_App.xaml.cs_)에 배치하거나 앱의 첫 번째 페이지에 배치할 수 있습니다.

```csharp
// Call your extend acrylic code in the OnLaunched event, after
// calling Window.Current.Activate.
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();

        rootFrame.NavigationFailed += OnNavigationFailed;

        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }

    if (e.PrelaunchActivated == false)
    {
        if (rootFrame.Content == null)
        {
            // When the navigation stack isn't restored navigate to the first page,
            // configuring the new page by passing required information as a navigation
            // parameter
            rootFrame.Navigate(typeof(MainPage), e.Arguments);
        }
        // Ensure the current window is active
        Window.Current.Activate();

        // Extend acrylic
        ExtendAcrylicIntoTitleBar();
    }
}
```

또한 앱 제목을 그려야 합니다. 앱 제목은 일반적으로 `CaptionTextBlockStyle`을 사용하는 TextBlock을 통해 제목 표시줄에 자동으로 표시됩니다. 자세한 내용은 [제목 표시줄 사용자 지정](../shell/title-bar.md)을 참조하세요.

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항
* 탐색 창처럼 기본이 아닌 앱 표면의 배경 소재로 아크릴을 사용하세요.
* 앱 주변과 살짝 혼합하여 원활한 환경을 제공할 수 있도록 아크릴을 앱 가장자리 중 하나 이상으로 확장하세요.
* 바탕 화면 아크릴을 앱의 큰 배경 화면에 배치하지 마세요. 이렇게 하면 일시적인 표면에 주로 사용되는 아크릴의 멘탈 모델이 중단됩니다.
* 경계선에 시각적 긴장감이 발생하지 않도록 인앱 아크릴과 배경 아크릴을 바로 인접하게 배치하지 마세요.
* 색조와 불투명도가 동일한 여러 아크릴 창을 나란히 배치하면 원치 않는 시각적 경계선이 생기므로 이렇게 배치하지 마세요.
* 아크릴 표면 위에 테마 컬러 텍스트를 배치하지 마세요.

## <a name="how-we-designed-acrylic"></a>아크릴 설계 방식

아크릴의 고유한 모양과 속성이 온전히 유지되도록 아크릴의 핵심 구성 요소를 세밀하게 조정했습니다. 시각적 깊이와 입체감을 추가하기 위해 반투명도, 흐림 및 노이즈 작업을 시작했습니다. 아크릴 배경에 배치된 UI의 대비와 가독성을 보장하기 위해 제외 혼합 모드 레이어를 추가했습니다. 마지막으로, 개인 설정 기회를 제공하기 위해 컬러 색조를 추가했습니다. 이러한 레이어가 합쳐져서 생생하고 편리한 소재가 완성됩니다.

![아크릴 제작법](images/AcrylicRecipe_Diagram.jpg)
<br/>아크릴 제작법: 배경, 흐림, 제외 혼합, 색/색조 오버레이, 노이즈


## <a name="get-the-sample-code"></a>샘플 코드 가져오기

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery) - 대화형 형식으로 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련된 문서

[**강조 표시**](reveal.md)
