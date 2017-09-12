---
author: mijacobs
description: "색은 앱의 다양한 수준의 정보를 통해 탐색하는 직관적인 방식을 제공하며 상호 작용 모델을 강화하는 데 중요한 도구 역할을 합니다."
title: "색"
ms.assetid: 3ba7176f-ac47-498c-80ed-4448edade8ad
template: detail.hbs
extraBodyClass: style-color
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
design-contact: rybick
doc-status: Published
ms.openlocfilehash: fd37d69c2e9b20b46c34e6071f302bd55bbbba26
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/22/2017
---
# <a name="color"></a>색

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

색은 앱의 다양한 수준의 정보를 통해 탐색하는 직관적인 방식을 제공하며 상호 작용 모델을 강화하는 데 중요한 도구 역할을 합니다.

Windows에서 색도 개인 고유의 것입니다. 색과 밝은 테마를 선택하거나 어두운 테마를 선택하여 환경에 전체적으로 반영할 수 있습니다.

## <a name="accent-color"></a>테마 컬러

사용자는 *설정 &gt; 개인 설정 &gt; 색*에서 강조색이라고 하는 단일 색을 선택할 수 있습니다. 사용자는 48색 견본의 큐레이팅된 집합에서 테마를 선택할 수 있습니다(TV에 적합한 21색 색상표가 포함된 Xbox의 경우 제외).

### <a name="default-accent-colors"></a>기본 테마 컬러
<table class="uwpd-color-table" style="border: solid 4px white;">
        <tr >
            <td class="uwpd-color-table" style="background-color: #FFB900">FFB900</td>
            <td class="uwpd-color-table" style=" background-color: #E74856">E74856</td>
            <td class="uwpd-color-table" style=" background-color: #0078D7">0078D7</td>
            <td class="uwpd-color-table" style=" background-color: #0099BC">0099BC</td>
            <td class="uwpd-color-table" style=" background-color: #7A7574">7A7574</td>
            <td class="uwpd-color-table" style=" background-color: #767676">767676</td>
        </tr>
        <tr >
            <td class="uwpd-color-table" style=" background-color: #FF8C00">FF8C00</td>
            <td class="uwpd-color-table" style=" background-color: #E81123">E81123</td>
            <td class="uwpd-color-table" style=" background-color: #0063B1">0063B1</td>
            <td class="uwpd-color-table" style=" background-color: #2D7D9A">2D7D9A</td>
            <td class="uwpd-color-table" style=" background-color: #5D5A58">5D5A58</td>
            <td class="uwpd-color-table" style=" background-color: #4C4A48" >4C4A48</td>
        </tr>
        <tr >
            <td class="uwpd-color-table" style=" background-color: #F7630C" >F7630C</td>
            <td class="uwpd-color-table" style=" background-color: #EA005E" >EA005E</td>
            <td class="uwpd-color-table" style=" background-color: #8E8CD8" >8E8CD8</td>
            <td class="uwpd-color-table" style=" background-color: #00B7C3" >00B7C3</td>
            <td class="uwpd-color-table" style=" background-color: #68768A" >68768A</td>
            <td class="uwpd-color-table" style=" background-color: #69797E" >69797E</td>
        </tr>
        <tr >
            <td class="uwpd-color-table" style=" background-color: #CA5010" >CA5010</td>
            <td class="uwpd-color-table" style=" background-color: #C30052" >C30052</td>
            <td class="uwpd-color-table" style=" background-color: #6B69D6" >6B69D6</td>
            <td class="uwpd-color-table" style=" background-color: #038387" >038387</td>
            <td class="uwpd-color-table" style=" background-color: #515C6B" >515C6B</td>
            <td class="uwpd-color-table" style=" background-color: #4A5459" >4A5459</td>
        </tr>
        <tr >
            <td class="uwpd-color-table" style=" background-color: #DA3B01" >DA3B01</td>
            <td class="uwpd-color-table" style=" background-color: #E3008C" >E3008C</td>
            <td class="uwpd-color-table" style=" background-color: #8764B8" >8764B8</td>
            <td class="uwpd-color-table" style=" background-color: #00B294" >00B294</td>
            <td class="uwpd-color-table" style=" background-color: #567C73" >567C73</td>
            <td class="uwpd-color-table" style=" background-color: #647C64" >647C64</td>
        </tr>
        <tr >
            <td class="uwpd-color-table" style=" background-color: #EF6950" >EF6950</td>
            <td class="uwpd-color-table" style=" background-color: #BF0077" >BF0077</td>
            <td class="uwpd-color-table" style=" background-color: #744DA9" >744DA9</td>
            <td class="uwpd-color-table" style=" background-color: #018574" >018574</td>
            <td class="uwpd-color-table" style=" background-color: #486860" >486860</td>
            <td class="uwpd-color-table" style=" background-color: #525E54" >525E54</td>
        </tr>
        <tr >
            <td class="uwpd-color-table" style=" background-color: #D13438" >D13438</td>
            <td class="uwpd-color-table" style=" background-color: #C239B3" >C239B3</td>
            <td class="uwpd-color-table" style=" background-color: #B146C2" >B146C2</td>
            <td class="uwpd-color-table" style=" background-color: #00CC6A" >00CC6A</td>
            <td class="uwpd-color-table" style=" background-color: #498205" >498205</td>
            <td class="uwpd-color-table" style=" background-color: #847545" >847545</td>
        </tr>
        <tr >
            <td class="uwpd-color-table" style=" background-color: #FF4343" >FF4343</td>
            <td class="uwpd-color-table" style=" background-color: #9A0089" >9A0089</td>
            <td class="uwpd-color-table" style=" background-color: #881798" >881798</td>
            <td class="uwpd-color-table" style=" background-color: #10893E" >10893E</td>
            <td class="uwpd-color-table" style=" background-color: #107C10" >107C10</td>
            <td class="uwpd-color-table" style=" background-color: #7E735F" >7E735F</td>
        </tr>

</table>

### <a name="xbox-accent-colors"></a>Xbox 테마 컬러
  <table class="uwpd-color-table" style="border: solid 4px white;">
      <tr >
          <td class="uwpd-color-table" style="background-color: #EB8C10" >EB8C10</td>
          <td class="uwpd-color-table" style="background-color: #ED5588" >ED5588</td>
          <td class="uwpd-color-table" style="background-color: #1073D6" >1073D6</td>
          <td class="uwpd-color-table" style="background-color: #148282" >148282</td>
          <td class="uwpd-color-table" style="background-color: #107C10" >107C10</td>
          <td class="uwpd-color-table" style="background-color: #4C4A4B" >4C4A4B</td>
      </tr>
      <tr >
          <td class="uwpd-color-table" style="background-color: #EB4910" >EB4910</td>
          <td class="uwpd-color-table" style="background-color: #BF1077" >BF1077</td>
          <td class="uwpd-color-table" style="background-color: #193E91" >193E91</td>
          <td class="uwpd-color-table" style="background-color: #54A81B" >54A81B</td>
          <td class="uwpd-color-table" style="background-color: #737373" >737373</td>
          <td class="uwpd-color-table" style="background-color: #7E715C" >7E715C</td>
      </tr>
      <tr >
          <td class="uwpd-color-table" style="background-color: #E31123" >E31123</td>
          <td class="uwpd-color-table" style="background-color: #B144C0" >B144C0</td>
          <td class="uwpd-color-table" style="background-color: #1081CA" >1081CA</td>
          <td class="uwpd-color-table" style="background-color: #547A72" >547A72</td>
          <td class="uwpd-color-table" style="background-color: #677488" >677488</td>
          <td class="uwpd-color-table" style="background-color: #724F2F" >724F2F</td>
      </tr>
      <tr >
          <td class="uwpd-color-table" style="background-color: #A21025" >A21025</td>
          <td class="uwpd-color-table" style="background-color: #744DA9" >744DA9</td>
          <td class="uwpd-color-table" style="background-color: #108272" >108272</td>
          <td class="uwpd-color-table"></td>
          <td class="uwpd-color-table"></td>
          <td class="uwpd-color-table"></td>
      </tr>
  </table>

특히 텍스트와 아이콘에는 테마 컬러를 배경으로 사용하지 마세요. 테마 컬러가 변할 수 있으므로 테마 컬러를 배경으로 사용해야 하는 경우 전경 텍스트를 쉽게 읽을 수 있도록 몇 가지 추가 작업을 수행해야 합니다. 예를 들어 텍스트가 흰색이고 테마 컬러가 밝은 회색인 경우 흰색과 밝은 회색 사이의 명암비가 작기 때문에 텍스트가 잘 안 보입니다. 테마 컬러를 테스트하여 어두운 색인지 확인하는 방법으로 이 문제를 해결할 수 있습니다.  

다음 알고리즘을 사용하여 배경색이 밝은 색인지 아니면 어두운 색인지 확인합니다.

```C#
void accentColorUpdated(FrameworkElement elementWithText)
{
    var uiSettings = new Windows.UI.ViewManagement.UISettings();
    Windows.UI.Color c = uiSettings.GetColorValue(UIColorType.Accent);

    bool colorIsDark = (5 * c.G + 2 * c.R + c.B) <= 8 * 128;
    if (colorIsDark)
    {
        elementWithText.RequestedTheme = ElementTheme.Light;
    }
    else
    {
        elementWithText.RequestedTheme = ElementTheme.Dark;
    }
}
```


```JS
function accentColorUpdated(elementWithText)
{
    var uiSettings = new Windows.UI.ViewManagement.UISettings();
    Windows.UI.Color c = uiSettings.GetColorValue(UIColorType.Accent);
    var colorIsDark (5 * c.g + 2 * c.r + c.b) <= 8 * 128;
    if (colorIsDark)
    {
        elementWithText.RequestedTheme = ElementTheme.Light;
    }
    else
    {
        elementWithText.RequestedTheme = ElementTheme.Dark;
    }     
}
```

테마 컬러가 밝은 색인지 아니면 어두운 색인지 확인한 후에는 적절한 전경 색을 선택합니다. 어두운 배경에는 밝은 테마의 SystemControlForegroundBaseHighBrush를 사용하고 밝은 배경에는 어두운 테마 버전을 사용하는 것이 좋습니다.

## <a name="color-palette-building-blocks"></a>색상표 구성 요소

테마 컬러를 선택하면 색 광도의 HSB 값에 따라 테마 컬러의 밝고 어두운 음영이 만들어집니다. 앱은 음영 변형을 사용하여 시각적 계층 구조를 만들고 상호 작용 지침을 제공할 수 있습니다.

하이퍼링크는 기본적으로 사용자의 테마 컬러를 사용 합니다. 페이지 배경이 유사한 색인 경우 효과적인 대비를 위해 하이퍼링크에 테마 음영을 더 밝거나 더 어둡게 지정할 수 있습니다.


<table class="uwpd-color-table" style="border: solid 4px white; width: 30pc">
   <caption>기본 테마 컬러의 다양한 음영(밝음/어두움)입니다.</caption>
    <tr>
        <td class="uwpd-color-table" style="background-color: #A6D8FF; color: black">3 색조 밝게</td>
    </tr>
    <tr>
        <td class="uwpd-color-table" style="background-color: #76B9ED; color: black">2 색조 밝게</td>
    </tr>
    <tr>
        <td class="uwpd-color-table" style="background-color: #429CE3; color: black">1 색조 밝게</td>
    </tr>
    <tr>
        <td class="uwpd-color-table" style="background-color: #0078D7; color: white">샘플 테마 컬러</td>
    </tr>
    <tr>
        <td class="uwpd-color-table" style="background-color: #005A9E; color: white">1 색조 어둡게</td>
    </tr>
    <tr>
        <td class="uwpd-color-table" style="background-color: #004275; color: white">2 색조 어둡게</td>
    </tr>
    <tr>
        <td class="uwpd-color-table" style="background-color: #002642; color: white">3 색조 어둡게</td>
    </tr>
</table>

<div class="uwpd-image-with-caption">
    <img src="images/action_center_redline_zoom.png" alt="Redlines for Colored Action Center" />
    <div>디자인 사양에 색 논리가 적용되는 방법에 대한 예입니다.</div>
</div>

>[!NOTE]
>XAML에서는 `SystemAccentColor`라는 [테마 리소스](https://msdn.microsoft.com/library/windows/apps/Mt187274.aspx)가 기본 테마 컬러로 표시됩니다. `SystemAccentColorLight3`, `SystemAccentColorLight2`, `SystemAccentColorLight1`, `SystemAccentColorDark1`, `SystemAccentColorDark2`, 및 `SystemAccentColorDark3`를 음영으로 사용할 수 있습니다. 또한 [UISettings.GetColorValue](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.uisettings.getcolorvalue.aspx) 및 [UIColorType](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx) 열거를 통해 프로그래밍 방식으로 사용할 수 있습니다.


## <a name="color-theming"></a>색 테마 지정

사용자는 또한 시스템에 대해 밝거나 어두운 테마를 선택할 수 있습니다. 일부 앱은 사용자의 기본 설정에 따라 해당 테마를 변경하도록 선택하지만 옵트아웃(opt out)하는 앱도 있습니다.

밝은 테마를 사용하는 앱은 생산성 앱과 관련된 시나리오에 적합합니다. 예제는 Microsoft Office와 함께 사용할 수 있는 앱의 제품군입니다. 밝은 테마는 장기간의 작업과 함께 긴 길이의 텍스트를 읽기 쉽게 합니다.

어두운 테마는 미디어 중심인 앱의 콘텐츠 또는 풍부한 동영상 및 이미지로 사용자가 나타내려는 시나리오의 대비를 더 잘 보이도록 해 줍니다. 이러한 시나리오에서는 영화 시청 환경이 기본 작업일 수 있지만 읽기는 반드시 기본 작업이지는 않으며 주변 조명이 어두운 상태에서 표시됩니다.

앱이 이러한 설명과 맞지 않는 경우 다음 시스템 테마를 고려하면 사용자에게 적합한 테마를 결정할 수 있습니다.

테마를 더 쉽게 디자인하기 위해 Windows에서는 자동으로 테마에 맞게 조정되는 추가 색상표를 제공합니다.

### <a name="light-theme"></a>밝은 테마
#### <a name="base"></a>기본
![기본 밝은 테마](images/themes-light-base.png)
#### <a name="alt"></a>대체
![대체 밝은 테마](images/themes-light-alt.png)
#### <a name="list"></a>목록
![목록 밝은 테마](images/themes-light-list.png)
#### <a name="chrome"></a>크롬
![크롬 밝은 테마](images/themes-light-chrome.png)
### <a name="dark-theme"></a>어두운 테마
#### <a name="base"></a>기본
![기본 어두운 테마](images/themes-dark-base.png)
#### <a name="alt"></a>대체
![대체 어두운 테마](images/themes-dark-alt.png)
#### <a name="list"></a>목록
![목록 어두운 테마](images/themes-dark-list.png)
#### <a name="chrome"></a>크롬
![크롬 어두운 테마](images/themes-dark-chrome.png)


## <a name="changing-the-theme"></a>테마 변경

App.xaml에서 **RequestedTheme** 속성을 변경하여 쉽게 테마를 변경할 수 있습니다.

```XAML
<Application
    x:Class="App9.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App9"
    RequestedTheme="Dark">

</Application>
```

**RequestedTheme**를 제거하면 응용 프로그램에 사용자의 앱 모드 설정이 적용되고 어둡거나 밝은 테마로 앱을 표시하도록 선택할 수 있음을 의미합니다.

테마는 앱의 모양에 큰 영향을 미치므로 앱을 만들 때 테마를 고려해야 합니다.

## <a name="accessibility"></a>접근성

색상표는 화면 사용에 최적화됩니다. 최적화된 읽기 환경을 위해 배경에 대한 텍스트 대비를 4.5: 1로 유지하는 것이 좋습니다. [명암비](http://leaverou.github.io/contrast-ratio/)와 같이 무료로 색 통과 여부를 테스트할 수 있는 많은 도구가 있습니다.

## <a name="related-articles"></a>관련 문서

* [XAML 스타일](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/xaml-theme-resources)
* [XAML 테마 리소스](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/xaml-theme-resources)
