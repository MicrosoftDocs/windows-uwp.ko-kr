---
author: mijacobs
description: 투명 한 텍스처를 만드는 브러시 유형입니다.
title: 아크릴 소재
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: rybick
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f9d56090e8fc1de83eeb4e8a68ca1830692c5b2f
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6468850"
---
# <a name="acrylic-material"></a>아크릴 소재

![영웅 이미지](images/header-acrylic.svg)

아크릴은 투명 한 텍스처를 만드는 [브러시](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Media.Brush) 유형입니다. 앱 표면에 아크릴을 적용하여 깊이를 추가하고 시각적 계층을 설정할 수 있습니다.  <!-- By allowing user-selected wallpaper or colors to shine through, acrylic keeps users in touch with the OS personalization they've chosen. -->

> **중요한 API**: [AcrylicBrush 클래스](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.acrylicbrush), [Background 속성](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control.Background)

:::row:::
    :::column:::
        Acrylic in light theme
        ![Acrylic in light theme](images/Acrylic_LightTheme_Base.png)
    :::column-end:::
    :::column:::
        Acrylic in dark theme
        ![Acrylic in dark theme](images/Acrylic_DarkTheme_Base.png)
    :::column-end:::
:::row-end:::

## <a name="acrylic-and-the-fluent-design-system"></a>아크릴 및 Fluent 디자인 시스템

 Fluent 디자인 시스템을 사용하면 조명, 깊이, 움직임, 재질 및 배율이 통합된 선명한 현대식 UI를 만들 수 있습니다. 아크릴은 앱에 물리적 텍스처(소재)과 깊이를 추가하는 Fluent 디자인 시스템 구성 요소입니다. 자세히 알아보려면 [UWP용 Fluent 디자인 개요](../fluent-design-system/index.md)를 확인하십시오.

 ## <a name="video-summary"></a>비디오 요약

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev002/player]

## <a name="examples"></a>예

:::row:::
    :::column span:::
        ![Some image](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    :::column span="2":::
        **XAML Controls Gallery**<br>
        If you have the XAML Controls Gallery app installed, click <a href="xamlcontrolsgallery:/item/Acrylic">here</a> to open the app and see acrylic in action.

        <a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Get the XAML Controls Gallery app (Microsoft Store)</a><br>
        <a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Get the source code (GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="acrylic-blend-types"></a>아크릴 혼합 유형
아크릴의 가장 눈에 띄는 특징은 투명도입니다. 소재를 통과하여 보이는 것을 변경하는 두 가지 아크릴 혼합 유형이 있습니다.
 - **배경 아크릴**은 현재 활성 앱의 뒤에 있는 바탕 화면 배경 화면 및 다른 창을 표시하여 응용 프로그램 창 사이에 깊이를 추가하고 사용자의 개인 기본 설정을 반영합니다.
 - **인앱 아크릴**은 앱 프레임 내부에 깊이감을 더하여 포커스와 계층을 모두 제공합니다.

 ![배경 아크릴](images/BackgroundAcrylic_DarkTheme.png)

 ![인앱 아크릴](images/AppAcrylic_DarkTheme.png)

 아크릴 표면을 레이어로 쌓아 올립니다 레이어: 여러 겹의 배경 아크릴은 방해가 만들 수 있습니다.

## <a name="when-to-use-acrylic"></a>아크릴을 사용하는 경우

* NavigationView 이나 인라인 명령 요소 같은 UI를 지원 하기 위한 인 앱 아크릴을 사용 합니다. 
* 상황에 맞는 메뉴, 플라이 아웃 빛 dimsissable UI 등의 임시 UI 요소에 대 한 배경 아크릴을 사용 합니다.<br />아크릴을 사용 하 여 임시 시나리오에서 임시 UI를 트리거한 콘텐츠와 함께 시각적 관계를 유지 하는 데 도움이 됩니다.

인 앱 아크릴 탐색 표면에를 사용 하는 경우에 아크릴 창 앱의 흐름을 개선 하기 위해 아래에 콘텐츠를 확장 하는 것이 좋습니다. NavigationView를 사용 하 여에서는이를 자동으로 수행 합니다. 그러나 스트라이핑 효과를 여러 가지 아크릴을 가장자리에 배치 하지 마십시오 두 흐린된 표면 사이의 원치 않는 경계선이 만들 수이 합니다. 아크릴, 디자인의 시각적 조화 도구 이지만 잘못 사용할 경우 시각적 노이즈가 발생할 수 있습니다.

앱에 아크릴을 통합 하는 가장 좋은 방법을 결정 하려면 다음 사용 패턴을 고려 하세요.

### <a name="horizontal-navigation-or-commanding"></a>가로 탐색 또는 명령

앱이 NavigationView를 활용할 수 없는 아크릴을 직접 추가 하려는 경우 색조 불투명도 60% 비교적 투명 한 아크릴을 사용 하는 것이 좋습니다.
 - 창이 다른 앱 콘텐츠 위에 오버레이로 열리는 경우 [60% 인앱 아크릴](#acrylic-theme-resources)이어야 합니다.
 - 창이 주 앱 콘텐츠와 함께 나란히 열리는 경우 [60% 배경 아크릴](#acrylic-theme-resources)이어야 합니다.

![앱에서 가로 명령을 사용 하 여 지도 앱](images/Maps_In_App_Acrylic_1.png)

또한 콘텐츠 확장 또는 스크롤 아크릴에서 맨 위에 있는 것 생깁니다 앱 몰입 형 하 고 원활한 환경을.

### <a name="vertical-panes"></a>세로 창

세로 창이 나 앱의 콘텐츠 해제 섹션의 데 도움이 되는 표면에 대 한 불투명 한 배경 아크릴 대신 사용 하는 것이 좋습니다. 서로 다른 창에는 콘텐츠 위에 열면, 같은 NavigationView의 **Collapsed** 또는 **최소** 모드 좋습니다 인 앱 아크릴을 사용 하 여 사용자가 열이 창은 페이지의 컨텍스트를 유지 하기 위해.

### <a name="transient-surfaces"></a>임시 표면

모달이 팝업 메뉴 플라이 아웃을 사용 하 여 앱에 대 한 빠른 해제 창 또는 배경 아크릴을 사용 하는 것이 좋습니다.

![정보는 플라이 아웃을 사용 하 여 메일 앱 패턴](images/Mail_TransientContextMenu.png)

다양 한 컨트롤은 기본적으로 아크릴을 사용 합니다. [MenuFlyouts](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus), [AutoSuggestBox](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/auto-suggest-box), [ComboBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox) 및 빛 dimiss 팝업을 사용 하 여 유사한 컨트롤이 모든 써서 임시 아크릴 호출 되는 경우.

> [!Note]
> 아크릴 표면 렌더링 GPU 형 장치 전력 소비를 증가 하 고 배터리 사용 시간을 줄일 수 있는입니다. 아크릴 효과 장치가 배터리 절약 모드를 입력 하 고 사용자가 모든 앱의 아크릴 효과 비활성화할 수 있습니다 비활성화 자동으로 선택 합니다.

## <a name="usability-and-adaptability"></a>유용성 및 적응성
아크릴은 다양한 디바이스 및 컨텍스트에 맞춰 자동으로 모양을 바꿉니다.

고대비 모드에서는 아크릴 대신 사용자가 선택한 익숙한 배경색이 계속 표시됩니다. 또한 배경 아크릴과 인 앱 아크릴이 모두 단색으로 표시 됩니다.
 - 사용자가 설정에서 투명도 해제할 때 > 개인 설정 > 색상
 - 배터리 절약 모드가 활성화 된 경우
 - 앱이 저사양 하드웨어에서 실행되는 경우

또한 배경 아크릴만 대체할는 투명 한 텍스처를 단색:
 - 바탕 화면에서 앱 창이 비활성화되는 경우
 - UWP 앱이 휴대폰, Xbox, HoloLens 또는 태블릿 모드에서 실행되는 경우

### <a name="legibility-considerations"></a>가독성 고려 사항
앱에서 사용자에게 표시하는 모든 텍스트는 [명암비를 충족](../accessibility/accessible-text-requirements.md)해야 합니다. 우리는 하이 컬러 검은색, 흰색 또는 심지어 미디엄 컬러 회색 텍스트까지 아크릴 위에서 명암비를 충족하도록 아크릴 제작법을 최적화했습니다. 플랫폼에서 제공하는 테마 리소스는 불투명도 80%의 대비 색조 색이 기본값입니다. 아크릴에 하이 컬러 본문 텍스트를 배치하면 가독성을 유지하면서도 색조 불투명도를 낮출 수 있습니다. 어둠 모드에서는 색조 불투명도를 70%로 할 수 있고, 밝음 모드 아크릴은 불투명도 50%에서 명암비를 충족합니다.

테마 컬러 텍스트를 아크릴 표면에 배치하지 않는 것이 좋습니다. 이러한 조합은 15픽셀 글꼴 크기에서 최소 명암비 요구 사항을 통과하지 못할 가능성이 높기 때문입니다. 되도록이면 아크릴 요소 위에 [하이퍼링크](../controls-and-patterns/hyperlinks.md)를 배치하지 마세요. 또한 아크릴 색조 색 또는 불투명도 수준을 테마 리소스에서 제공하는 플랫폼 기본값 범위를 벗어나서 사용자 지정하려는 경우 가독성에 미치는 영향을 염두에 두어야 합니다.

## <a name="acrylic-theme-resources"></a>아크릴 테마 리소스
새로운 XAML AcrylicBrush 또는 미리 정의된 AcrylicBrush 테마 리소스를 사용하여 간편하게 앱 표면에 아크릴을 적용할 수 있습니다. 먼저 인앱 아크릴 또는 배경 아크릴 중에 무엇을 사용할지 결정해야 합니다. 이 문서의 앞부분에서 권장 사항으로 설명한 공통 앱 패턴을 검토하세요.

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
        <td> <b>권장 사용법:</b> 다음은 다양한 사용법에서 제대로 작동하는 범용 아크릴 리소스입니다. 앱에서 텍스트 크기가 18픽셀보다 작은 AltMedium 컬러 보조 텍스트를 사용하는 경우 <a href="../accessibility/accessible-text-requirements.md">명암비 요구 사항을 충족</a>하도록 80% 아크릴 리소스를 텍스트 뒤에 배치합니다. </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowMediumHighBrush, SystemControlAcrylicElementMediumHighBrush <br/> SystemControlBaseHighAcrylicWindowMediumHighBrush, SystemControlBaseHighAcrylicElementMediumHighBrush </td>
        <td align="center"> 70% </td>
        <td> ChromeMedium <br/><br/> BaseHigh </td>
    </tr>
    <tr>
        <td> <b>권장 사용법:</b> 앱에서는 AltMedium 컬러 보조 텍스트, 텍스트 크기가 18 픽셀 이상인 경우 텍스트 뒤에 이러한 더 반투명 70% 아크릴 리소스를 배치할 수 있습니다. 앱의 상단 가로 탐색 및 명령 영역에 이러한 리소스를 사용하는 것이 좋습니다.  </td>
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
 - **TintOpacity**: 색조 계층의 불투명도입니다. 서로 다른 색상에 다른 translucencies 더 멋지게 보일 수 있습니다 있지만 시작 점으로 80% 불투명도 권장 합니다.
 - **BackgroundSource**: 배경 아크릴과 인앱 아크릴 중에서 원하는 것을 지정하는 플래그입니다.
 - **FallbackColor**: 단색 배터리 절약 모드에서 아크릴을 대체 합니다. 배경 아크릴의 경우 앱이 활성 데스크톱 창에 없거나 앱이 휴대폰 및 Xbox에서 실행 중인 경우 대체 색이 아크릴도 대체합니다.

![밝은 테마 아크릴 견본](images/CustomAcrylic_Swatches_LightTheme.png)

![어두운 테마 아크릴 견본](images/CustomAcrylic_Swatches_DarkTheme.png)

아크릴 브러시를 추가하려면 어두운 테마, 밝은 테마, 고대비 테마에 대한 세 개 리소스를 정의합니다. 고대비에서는 어두운/밝은 AcrylicBrush와 동일한 x:Key와 함께 SolidColorBrush를 사용하는 것이 좋습니다.

```xaml
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
        <AcrylicBrush x:Key="MyAcrylicBrush"
            BackgroundSource="HostBackdrop"
            TintColor="#FFFF0000"
            TintOpacity="0.8"
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
            FallbackColor="#FFFF7F7F"/>
    </ResourceDictionary>
</ResourceDictionary.ThemeDictionaries>
```

다음 샘플은 코드에서 AcrylicBrush를 선언하는 방법을 보여 줍니다. 앱에서 여러 OS 대상을 지원하는 경우 사용자의 컴퓨터에서 이 API를 사용할 수 있는지 확인해야 합니다.

```csharp
if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.UI.Xaml.Media.XamlCompositionBrushBase"))
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

앱의 창 원활하게 보이게 하려면 제목 표시줄 영역에서 아크릴을 사용할 수 있습니다. 이 예에서는 [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar) 개체의 [ButtonBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonBackgroundColor) 및 [ButtonInactiveBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonInactiveBackgroundColor) 속성을 [Colors.Transparent](https://docs.microsoft.com/uwp/api/Windows.UI.Colors.Transparent)로 설정하여 제목 표시줄로 아크릴을 확장합니다. 

```csharp
/// Extend acrylic into the title bar. 
private void ExtendAcrylicIntoTitleBar()
{
    CoreApplication.GetCurrentView().TitleBar.ExtendViewIntoTitleBar = true;
    ApplicationViewTitleBar titleBar = ApplicationView.GetForCurrentView().TitleBar;
    titleBar.ButtonBackgroundColor = Colors.Transparent;
    titleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
}
```

여기에 표시된 대로 [Window.Activate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.Activate)를 호출한 이후 이 코드를 앱의 [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) 메서드(_App.xaml.cs_)에 배치하거나 앱의 첫 번째 페이지에 배치할 수 있습니다. 


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
* 앱의 백그라운드 큰 화면에서 데스크톱 arylic 넣지 마세요-이 임시 표면에 주로 사용 되는 아크릴의 멘 탈 모델을 중단 합니다.
* 경계선에 시각적 긴장감이 발생하지 않도록 인앱 아크릴과 배경 아크릴을 바로 인접하게 배치하지 마세요.
* 색조와 불투명도가 동일한 여러 아크릴 창을 나란히 배치하면 원치 않는 시각적 경계선이 생기므로 이렇게 배치하지 마세요.
* 아크릴 표면 위에 테마 컬러 텍스트를 배치하지 마세요.

## <a name="how-we-designed-acrylic"></a>아크릴 설계 방식

아크릴의 고유한 모양과 속성에 도달할 수 있도록 아크릴의 핵심 구성 요소를 세밀하게 조정했습니다. 투명, 흐림 및 노이즈 시각적 깊이 입체감을 추가를 시작 합니다. 아크릴 배경에 배치된 UI의 대비와 가독성을 보장하기 위해 제외 혼합 모드 레이어를 추가했습니다. 마지막으로 개인 설정 기회를 제공하기 위해 컬러 색조를 추가했습니다. 이러한 레이어가 합쳐져서 생생하고 편리한 소재가 완성됩니다.

![아크릴 제작법](images/AcrylicRecipe_Diagram.jpg)
<br/>아크릴 제작법: 배경, 흐림, 제외 혼합, 색/색조 오버레이, 노이즈


## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련 문서

[**강조 표시**](reveal.md)