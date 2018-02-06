---
author: mijacobs
description: "부분적으로 투명한 텍스처를 만드는 일종의 브러시입니다."
title: "아크릴 소재"
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: rybick
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: high
ms.openlocfilehash: 8f8266878356ae182b6abf59a6729bf7066d6e4c
ms.sourcegitcommit: 4b522af988273946414a04fbbd1d7fde40f8ba5e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/08/2018
---
# <a name="acrylic-material"></a>아크릴 소재

아크릴은 부분적으로 투명한 텍스처를 만드는 일종의 [브러시](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Media.Brush)입니다. 앱 표면에 아크릴을 적용하여 깊이를 추가하고 시각적 계층을 설정할 수 있습니다.  <!-- By allowing user-selected wallpaper or colors to shine through, Acrylic keeps users in touch with the OS personalization they've chosen. -->

> **중요한 API**: [AcrylicBrush 클래스](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.acrylicbrush), [Background 속성](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Background)


![밝은 테마의 아크릴](images/Acrylic_DarkTheme_Base.png)

![어두운 테마의 아크릴](images/Acrylic_LightTheme_Base.png)

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/Acrylic">앱을 열고 작동 중인 아크릴을 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>비디오 요약

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev002/player]

## <a name="acrylic-and-the-fluent-design-system"></a>아크릴 및 Fluent 디자인 시스템

 Fluent 디자인 시스템을 사용하면 조명, 깊이, 움직임, 재질 및 배율이 통합된 선명한 현대식 UI를 만들 수 있습니다. 아크릴은 앱에 물리적 텍스처(소재)과 깊이를 추가하는 Fluent 디자인 시스템 구성 요소입니다. 자세히 알아보려면 [UWP용 Fluent 디자인 개요](../fluent-design-system/index.md)를 확인하십시오.

## <a name="when-to-use-acrylic"></a>아크릴을 사용하는 경우

앱 내 탐색이나 명령 요소 같은 지원 UI는 아크릴 표면에 배치하는 것이 좋습니다. 이 소재는 임시 UI를 트리거한 콘텐츠와의 시각적 관계를 유지하는 데 도움이 되므로 대화나 플라이아웃 같은 임시 UI 요소에도 도움이 됩니다. 아크릴은 배경 소재로 사용되어 시각적 개별 창을 표시하도록 설계되었으므로 세밀하게 표시되는 전경 요소에는 아크릴을 적용하지 마세요.

기본 앱 콘텐츠 뒤의 표면에는 단색의 불투명 배경을 사용해야 합니다.

창의 제목 표시줄을 포함하여 하나 이상의 앱 가장자리로 아크릴을 확장하여 시각적 흐름을 개선하는 방법을 고려해 보세요. 서로 인접한 여러 종류의 혼합 아크릴을 겹쳐서 스트라이핑 효과를 예방하세요. 아크릴은 디자인의 시각적 조화를 위한 도구지만 잘못 사용할 경우 시각적 혼란을 일으킬 수 있습니다.

다음 사용 패턴을 고려하여 앱에 아크릴을 통합하는 가장 좋은 방법을 결정합니다.

### <a name="vertical-acrylic-pane"></a>세로 아크릴 창

세로 탐색을 사용하는 앱의 경우 탐색 요소가 포함된 보조 창에 아크릴을 적용하는 것이 좋습니다.

![단일 세로 아크릴 창을 사용하는 앱 패턴](images/acrylic_app-pattern_vertical.png)

[NavigationView](../controls-and-patterns/navigationview.md)는 앱에 탐색을 추가하기 위한 새로운 공통 컨트롤이며 시각적 디자인에 아크릴이 포함되어 있습니다. NavigationView의 창은 기본 콘텐츠와 함께 나란히 창을 여는 경우 배경 아크릴을 표시하며, 창이 오버레이로 열리는 경우 자동으로 인앱 아크릴로 전환됩니다.

앱에서 NavigationView를 활용할 수 없고 아크릴을 직접 추가하려는 경우 색조 불투명도 60%의 비교적 투명한 아크릴을 사용하는 것이 좋습니다.
 - 창이 다른 앱 콘텐츠 위에 오버레이로 열리는 경우 [60% 인앱 아크릴](#acrylic-theme-resources)이어야 합니다.
 - 창이 주 앱 콘텐츠와 함께 나란히 열리는 경우 [60% 배경 아크릴](#acrylic-theme-resources)이어야 합니다.

### <a name="multiple-acrylic-panes"></a>여러 아크릴 창

서로 다른 창 세 개가 있는 앱의 경우 기본이 아닌 콘텐츠에 아크릴을 추가하는 것이 좋습니다.
 - 기본 콘텐츠와 가까이에 있는 두 번째 창에는 [80% 배경 아크릴](#acrylic-theme-resources)을 사용합니다.
 - 기본 콘텐츠에서 멀리 떨어진 세 번째 창에는 [60% 배경 아크릴](#acrylic-theme-resources)을 사용합니다.

![두 개의 세로 아크릴 창을 사용하는 앱 패턴](images/acrylic_app-pattern_double-vertical.png)

### <a name="horizontal-acrylic-pane"></a>가로 아크릴 창

앱 위에 가로 탐색, 명령 또는 다른 강력한 가로 요소를 사용하는 앱의 경우 이 시각적 요소에 [70% 아크릴](#acrylic-theme-resources)을 적용하는 것이 좋습니다.

![가로 아크릴 창을 사용하는 앱 패턴](images/acrylic_app-pattern_horizontal.png)

확대/축소 가능한 연속 콘텐츠를 강조하는 캔버스 앱은 사용자가 이 콘텐츠에 연결할 수 있도록 위쪽 표시줄에 인앱 아크릴을 사용해야 합니다. 캔버스 앱의 예로는 지도, 그리기 및 드로잉을 들 수 있습니다.

연속 캔버스가 없는 앱의 경우 배경 아크릴을 사용하여 사용자를 전체 데스크톱 환경에 연결하는 것이 좋습니다.

### <a name="acrylic-in-utility-apps"></a>유틸리티 앱의 아크릴

위젯 또는 간단한 앱은 앱 창 내에 아크릴 가장자리를 그림으로써 유틸리티 앱으로서의 사용을 늘릴 수 있습니다. 일반적으로 이 범주에 속하는 앱은 사용자의 참여 시간이 짧으며, 사용자의 전체 바탕 화면을 차지하지 않습니다. 예로는 계산기와 알림 센터가 있습니다.

![전체 배경으로 아크릴을 사용한 계산기 유틸리티 앱](images/acrylic_app-pattern_full.png)

> [!Note]
> 아크릴 표면을 렌더링하는 작업은 GPU를 많이 소모하며, 이로 인해 디바이스 전력 사용량이 증가하고 배터리 사용 시간이 단축될 수 있습니다. 디바이스가 배터리 절약 모드로 전환되면 아크릴 효과가 자동으로 해제되며, 사용자는 원한다면 모든 앱의 아크릴 효과를 비활성화할 수 있습니다.


## <a name="acrylic-blend-types"></a>아크릴 혼합 유형
아크릴의 가장 눈에 띄는 특징은 투명도입니다. 소재를 통과하여 보이는 것을 변경하는 두 가지 아크릴 혼합 유형이 있습니다.
 - **배경 아크릴**은 현재 활성 앱의 뒤에 있는 바탕 화면 배경 화면 및 다른 창을 표시하여 응용 프로그램 창 사이에 깊이를 추가하고 사용자의 개인 기본 설정을 반영합니다.
 - **인앱 아크릴**은 앱 프레임 내부에 깊이감을 더하여 포커스와 계층을 모두 제공합니다.

 ![배경 아크릴](images/BackgroundAcrylic_DarkTheme.png)

 ![인앱 아크릴](images/AppAcrylic_DarkTheme.png)

 신중하게 아크릴 표면을 레이어로 쌓아 올립니다. 배경 아크릴은 그 이름에서 알 수 있듯이 z-순서에서 사용자와 가까우면 안 됩니다. 여러 겹의 배경 아크릴은 예상치 못한 착시 현상을 유발하는 경향이 있으므로 역시 피해야 합니다. 아크릴을 레이어로 쌓아 올리기로 선택하는 경우 레이어가 시각적으로 뷰어와 좀 더 가까워지도록 인앱 아크릴을 사용하고 아크릴의 색조를 밝게 하는 방법을 고려해 보세요.


## <a name="usability-and-adaptability"></a>유용성 및 적응성
아크릴은 다양한 디바이스 및 컨텍스트에 맞춰 자동으로 모양을 바꿉니다.

고대비 모드에서는 아크릴 대신 사용자가 선택한 익숙한 배경색이 계속 표시됩니다. 또한 다음과 같은 경우 배경 아크릴과 인앱 아크릴이 모두 단색으로 표시됩니다.
 - 사용자가 개인 설정에서 투명도를 해제하는 경우
 - 배터리 절약 모드가 활성화된 경우
 - 앱이 저사양 하드웨어에서 실행되는 경우

뿐만 아니라 다음과 같은 경우 배경 아크릴만 투명도 및 텍스처를 단색으로 대체합니다.
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
        <th align="center">[대체 색](color.md)</th>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowBrush, SystemControlAcrylicElementBrush <br/> SystemControlChromeLowAcrylicWindowBrush, SystemControlChromeLowAcrylicElementBrush <br/> SystemControlBaseHighAcrylicWindowBrush, SystemControlBaseHighAcrylicElementBrush <br/> SystemControlBaseLowAcrylicWindowBrush, SystemControlBaseLowAcrylicElementBrush <br/> SystemControlAltHighAcrylicWindowBrush, SystemControlAltHighAcrylicElementBrush <br/> SystemControlAltLowAcrylicWindowBrush, SystemControlAltLowAcrylicElementBrush </td>
        <td align="center"> 80% </td>
        <td> ChromeMedium <br/> ChromeLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltHigh <br/><br/> AltLow </td>
    </tr>
    </tr>
        <td> **권장 사용법:** 다음은 다양한 사용법에서 제대로 작동하는 범용 아크릴 리소스입니다. 앱에서 텍스트 크기가 18픽셀보다 작은 AltMedium 컬러 보조 텍스트를 사용하는 경우 [명암비 요구 사항을 충족](../accessibility/accessible-text-requirements.md)하도록 80% 아크릴 리소스를 텍스트 뒤에 배치합니다. </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowMediumHighBrush, SystemControlAcrylicElementMediumHighBrush <br/> SystemControlBaseHighAcrylicWindowMediumHighBrush, SystemControlBaseHighAcrylicElementMediumHighBrush </td>
        <td align="center"> 70% </td>
        <td> ChromeMedium <br/><br/> BaseHigh </td>
    </tr>
    <tr>
        <td> **권장 사용법:** 앱에서 텍스트 크기가 18픽셀 이상인 AltMedium 컬러 보조 텍스트를 사용하는 경우 보다 투명한 70% 아크릴 리소스를 텍스트 뒤에 배치할 수 있습니다. 앱의 상단 가로 탐색 및 명령 영역에 이러한 리소스를 사용하는 것이 좋습니다.  </td>
    </tr>
    <tr>
        <td> SystemControlChromeHighAcrylicWindowMediumBrush, SystemControlChromeHighAcrylicElementMediumBrush <br/> SystemControlChromeMediumAcrylicWindowMediumBrush, SystemControlChromeMediumAcrylicElementMediumBrush <br/> SystemControlChromeMediumLowAcrylicWindowMediumBrush, SystemControlChromeMediumLowAcrylicElementMediumBrush <br/> SystemControlBaseHighAcrylicWindowMediumBrush, SystemControlBaseHighAcrylicElementMediumBrush <br/> SystemControlBaseMediumLowAcrylicWindowMediumBrush, SystemControlBaseMediumLowAcrylicElementMediumBrush <br/> SystemControlAltMediumLowAcrylicWindowMediumBrush, SystemControlAltMediumLowAcrylicElementMediumBrush  </td>
        <td align="center"> 60% </td>
        <td> ChromeHigh <br/><br/> ChromeMedium <br/><br/> ChromeMediumLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltMediumLow </td>
    </tr>
    <tr>
        <td> **권장 사용법:** 아크릴 위에 AltHigh 색의 기본 텍스트만 배치할 경우 앱에서 이러한 60% 리소스를 활용할 수 있습니다. 앱의 [세로 탐색 창](../controls-and-patterns/navigationview.md), 다시 말해서 햄버거 메뉴는 60% 아크릴로 그리는 것이 좋습니다. </td>
    </tr>
</table>

색 중립적 아크릴 외에도 사용자 지정된 테마 컬러를 사용하여 아크릴에 색조를 넣는 리소스도 추가되었습니다. 색이 있는 아크릴은 되도록이면 적게 사용하는 것이 좋습니다. 제공된 dark1 및 dark2 변형의 경우 어두운 테마 텍스트 색과 일치하는 흰색 또는 밝은 색 텍스트를 이러한 리소스 위에 배치합니다.
<table>
    <tr>
        <th align="center">리소스 키</th>
        <th align="center">색조 불투명도</th>
        <th align="center">[색조 및 대체 색](color.md)</th>
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
 - **TintOpacity**: 색조 계층의 불투명도입니다. 투명도가 다른 여러 색을 사용하면 좀 더 멋지게 보일 수도 있지만 처음에는 80% 불투명도를 권장합니다.
 - **BackgroundSource**: 배경 아크릴과 인앱 아크릴 중에서 원하는 것을 지정하는 플래그입니다.
 - **FallbackColor**: 배터리 부족 모드에서 아크릴을 대체하는 단색입니다. 배경 아크릴의 경우 앱이 활성 데스크톱 창에 없거나 앱이 휴대폰 및 Xbox에서 실행 중인 경우 대체 색이 아크릴도 대체합니다.


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

앱의 창 원활하게 보이게 하려면 제목 표시줄 영역에서 아크릴을 사용할 수 있습니다. 이 예에서는 [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar) 개체의 [ButtonBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar#Windows_UI_ViewManagement_ApplicationViewTitleBar_ButtonBackgroundColor) 및 [ButtonInactiveBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar#Windows_UI_ViewManagement_ApplicationViewTitleBar_ButtonInactiveBackgroundColor) 속성을 [Colors.Transparent](https://docs.microsoft.com/uwp/api/Windows.UI.Colors#Windows_UI_Colors_Transparent)로 설정하여 제목 표시줄로 아크릴을 확장합니다. 

```csharp
/// Extend acrylic into the title bar. 
private void extendAcrylicIntoTitleBar()
{
    CoreApplication.GetCurrentView().TitleBar.ExtendViewIntoTitleBar = true;
    ApplicationViewTitleBar titleBar = ApplicationView.GetForCurrentView().TitleBar;
    titleBar.ButtonBackgroundColor = Colors.Transparent;
    titleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
}
```

여기에 표시된 대로 [Window.Activate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window#Windows_UI_Xaml_Window_Activate)를 호출한 이후 이 코드를 앱의 [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) 메서드(_App.xaml.cs_)에 배치하거나 앱의 첫 번째 페이지에 배치할 수 있습니다. 


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
        extendAcrylicIntoTitleBar();
    }
}
```

또한 앱 제목을 그려야 합니다. 앱 제목은 일반적으로 `CaptionTextBlockStyle`을 사용하는 TextBlock을 통해 제목 표시줄에 자동으로 표시됩니다. 자세한 내용은 [제목 표시줄 사용자 지정](../shell/title-bar.md)을 참조하세요.

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항
* 탐색 창처럼 기본이 아닌 앱 표면의 배경 소재로 아크릴을 사용하세요.
* 앱 주변과 살짝 혼합하여 원활한 환경을 제공할 수 있도록 아크릴을 앱 가장자리 중 하나 이상으로 확장하세요.
* 경계선에 시각적 긴장감이 발생하지 않도록 인앱 아크릴과 배경 아크릴을 바로 인접하게 배치하지 마세요.
* 색조와 불투명도가 동일한 여러 아크릴 창을 나란히 배치하면 원치 않는 시각적 경계선이 생기므로 이렇게 배치하지 마세요.
* 아크릴 표면 위에 테마 컬러 텍스트를 배치하지 마세요.

## <a name="how-we-designed-acrylic"></a>아크릴 설계 방식

아크릴의 고유한 모양과 속성에 도달할 수 있도록 아크릴의 핵심 구성 요소를 세밀하게 조정했습니다. 시각적 깊이와 입체감을 추가하기 위해 투명도, 흐림 및 노이즈를 시작했습니다. 아크릴 배경에 배치된 UI의 대비와 가독성을 보장하기 위해 제외 혼합 모드 레이어를 추가했습니다. 마지막으로 개인 설정 기회를 제공하기 위해 컬러 색조를 추가했습니다. 이러한 레이어가 합쳐져서 생생하고 편리한 소재가 완성됩니다.

![아크릴 제작법](images/AcrylicRecipe_Diagram.png)
<br/>아크릴 제작법: 배경, 흐림, 제외 혼합, 색/색조 오버레이, 노이즈

<!--
<div class="microsoft-internal-note">
When designing your app, please utilize these [design resources](http://uni/DesignDepot.FrontEnd/#/Search?t=Resources%7CNeon%7CToolkit&f=Acrylic%20Material) to show acrylic in comps. The linked templates are the most accurate way to represent acrylic material in Photoshop and Illustrator. The ordering, as noted in the recipe diagram above, should start from the top: <br/>
 - Noise asset (tiled) at 2% opacity <br/>
 - Base color/tint/alpha layer <br/>
 - Exclusion blend (white @ 10% opacity) <br/>
 - Gaussian blur (30px radius) <br/>
</div>
-->

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련 문서

[**강조 표시**](reveal.md)
