---
description: 앱에서 입력 체계를 사용하여 사용자가 콘텐츠를 쉽게 이해할 수 있도록 도와주는 방법을 알아보세요.
title: UWP 앱의 입력 체계
ms.date: 04/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: cb2aef514c8787b5afe11ea5a2818012bfdf2f41
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "72282407"
---
# <a name="typography"></a>입력 체계

![영웅 이미지](images/header-typography.svg)

언어의 시각적 표현인 입력 체계의 주요 목적은 정보 전달입니다. 입력 체계의 스타일이 그 목표를 방해해서는 안 됩니다. 이 문서에서는 사용자가 콘텐츠를 쉽고 효율적으로 이해할 수 있도록 UWP 앱에서 입력 체계 스타일을 지정하는 방법에 대해 설명합니다.

## <a name="font"></a>글꼴

앱의 UI 전체에서 한 가지 글꼴만 사용해야 하며 UWP 앱의 기본 글꼴인 **Segoe UI**를 계속 사용하는 것이 좋습니다. 크기와 픽셀 밀도에 따라 최적의 가독성을 유지하도록 설계되었으며, 시스템의 콘텐츠를 보완하는 깔끔하고 가볍고 개방적인 디자인을 제공합니다.

![Segoe UI 글꼴의 샘플 텍스트](images/type/segoe-sample.svg)

영어 이외의 언어를 표시하거나 앱의 다른 글꼴을 선택하려면 UWP 앱의 권장 글꼴에서 [언어](#languages) 및 [글꼴](#fonts)을 참조하세요.

:::row:::
    :::column:::
![허용](images/do.svg) UI에 사용할 하나의 글꼴을 선택합니다.
    :::column-end:::
    :::column:::
![금지](images/dont.svg) 여러 글꼴을 혼합하지 않습니다.
    :::column-end:::
:::row-end:::

## <a name="size-and-scaling"></a>크기 및 스케일링

UWP 앱의 글꼴 크기는 모든 디바이스에서 자동으로 조정됩니다. 스케일링 알고리즘은 3m 떨어져 있는 Surface Hub 10의 24픽셀 글꼴을 불과 몇 인치 떨어져 있는 5인치 휴대폰의 24픽셀 글꼴처럼 읽을 수 있게 해줍니다.

![다양한 디바이스의 가시거리](images/type/scaling-chart.svg)

스케일링 시스템의 작동 방식 때문에 실제 픽셀이 아닌 유효 픽셀로 디자인하고 있으므로, 화면 크기나 해상도가 다른 경우에도 글꼴 크기를 변경할 필요가 없습니다.

:::row:::
    :::column:::
![허용](images/do.svg) UWP [유형 램프](#type-ramp) 크기 조정을 따릅니다.
    :::column-end:::
    :::column:::
![금지](images/dont.svg) 12픽셀보다 작은 글꼴 크기를 사용합니다.
    :::column-end:::
:::row-end:::

## <a name="hierarchy"></a>계층 구조

:::row:::
    :::column:::
사용자는 페이지를 검색할 때 시각적 계층 구조를 사용합니다. 머리글은 콘텐츠를 요약하고, 본문 텍스트는 자세한 정보를 제공합니다. 앱에서 명확한 시각적 계층 구조를 만들려면 UWP 유형 램프를 따릅니다.
    :::column-end:::
    :::column:::
![텍스트 블록 스타일](images/type/type-hierarchy.svg)
    :::column-end:::
:::row-end:::

### <a name="type-ramp"></a>유형 램프

UWP 유형 램프는 페이지의 유형 스타일 간에 중요한 관계를 설정하므로 사용자가 콘텐츠를 쉽게 읽을 수 있습니다. 모든 크기는 유효 픽셀로 설정되며 모든 디바이스에서 실행되는 UWP 앱에 최적화되어 있습니다.

![유형 램프](images/type/type-ramp.png)

### <a name="using-the-type-ramp"></a>유형 램프 사용

:::row:::
    :::column:::
유형 램프 수준에 XAML [정적 리소스](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp)로 액세스할 수 있습니다. 스타일은 `*TextBlockStyle` 명명 규칙을 따릅니다.
    :::column-end:::
    :::column:::
![텍스트 블록 스타일](images/type/text-block-type-ramp.svg)
    :::column-end:::
:::row-end:::

```XAML
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>
```

:::row:::
    :::column:::
![허용](images/do.svg) 대부분의 텍스트에는 "Body"를 사용합니다.

공간이 제한된 경우에는 제목에 "Base"를 사용합니다.
    :::column-end:::
    :::column:::
![금지](images/dont.svg) 기본 작업 또는 긴 문자열에는 "Caption"을 사용합니다.

텍스트를 줄 바꿈해야 하는 경우 "Header" 또는 "Subheader"를 사용합니다.
    :::column-end:::
:::row-end:::

## <a name="alignment"></a>부합되는 내용

[TextAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.textalignment) 기본값은 왼쪽이고, 대부분의 경우 이 왼쪽 맞춤 방법은 일관된 콘텐츠 고정과 균일한 레이아웃을 제공합니다. RTL 언어에 대한 내용은 [세계화를 지원하도록 레이아웃 및 글꼴 조정](../globalizing/adjust-layout-and-fonts--and-support-rtl.md)을 참조하세요.

![텍스트 왼쪽 맞춤을 표시합니다.](images/type/alignment.svg)

```xaml
<TextBlock TextAlignment="Left">
```

## <a name="character-count"></a>문자 수

:::row:::
    :::column:::
![허용](images/do.svg) 쉽게 읽을 수 있도록 줄당 50~60자를 유지합니다.
    :::column-end:::
    :::column:::
![금지](images/dont.svg) 줄당 20자 미만 또는 60자 이상은 쉽게 읽을 수 없습니다.
    :::column-end:::
:::row-end:::

## <a name="clipping-and-ellipses"></a>클리핑 및 줄임표

텍스트의 양이 사용 가능한 공간을 초과하는 경우 텍스트 클리핑을 사용할 것을 권장하며, 이는 대다수 [UWP 텍스트 컨트롤](../controls-and-patterns/text-controls.md)의 기본 동작입니다.

![일부 텍스트 클리핑이 있는 디바이스 프레임을 보여 줍니다.](images/type/clipping.svg)

```xaml
<TextBlock TextWrapping="WrapWholeWords" TextTrimming="Clip"/>
```

:::row:::
    :::column:::
![허용](images/do.svg) 여러 줄을 사용하도록 설정되면 텍스트를 잘라내고 줄 바꿈합니다.
    :::column-end:::
    :::column:::
![금지](images/dont.svg) 생략 부호를 사용하여 시각적 혼란을 방지합니다.
    :::column-end:::
:::row-end:::

**참고**: 컨테이너가 잘 정의되지 않았거나(예: 차별화된 배경색 없음) 추가 텍스트를 볼 수 있는 링크가 있는 경우 줄임표를 사용하세요.

## <a name="languages"></a>언어 

Segoe UI는 영어, 유럽 언어, 그리스어, 히브리어, 아르메니아어, 그루지야어, 아랍어에서 사용하는 공통 글꼴입니다. 그 외의 언어는 다음 권장 글꼴을 확인하세요.

### <a name="globalizinglocalizing-fonts"></a>글꼴 세계화/지역화

특정 언어에 대한 권장 글꼴 패밀리, 크기, 두께 및 스타일에 프로그래밍 방식으로 액세스하려면 [LanguageFont 글꼴 매핑 API](https://docs.microsoft.com/uwp/api/Windows.Globalization.Fonts.LanguageFont)를 사용합니다. LanguageFont 개체는 UI 헤더, 알림, 본문, 사용자 편집 가능한 문서 본문 글꼴을 비롯하여 콘텐츠의 다양한 범주에 대한 올바른 글꼴 정보에 액세스합니다. 자세한 내용은 [세계화를 지원하도록 레이아웃 및 글꼴 조정](../globalizing/adjust-layout-and-fonts--and-support-rtl.md)을 참조하세요.

### <a name="fonts-for-non-latin-languages"></a>라틴어가 아닌 언어용 글꼴

<table>
<thead>
<tr class="header">
<th align="left">글꼴 패밀리</th>
<th align="left">스타일</th>
<th align="left">참고</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="font-family: Embrima;">Ebrima</td>
<td align="left">보통, 굵게</td>
<td align="left">아프리카어 스크립트(게에즈 문자, 은코 문자, 오스만 문자, 티푸나구 문자, 바이 문자)의 사용자 인터페이스 글꼴</td>
</tr>
<tr class="even">
<td style="font-family: Gadugi;">Gadugi</td>
<td align="left">보통, 굵게</td>
<td align="left">북아메리카어 스크립트(Canadian Syllabics, 체로키 문자)의 사용자 인터페이스 글꼴</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Leelawadee UI;">Leelawadee UI</td>
<td align="left">보통, Semilight, 굵게</td>
<td align="left">동남 아시아어 스크립트(부기 문자, 라오스 문자, 크메르 문자, 태국 문자)의 사용자 인터페이스 글꼴</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Malgun Gothic;">Malgun Gothic</td>
<td align="left">Regular</td>
<td align="left">한국어의 사용자 인터페이스 글꼴</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft JhengHei UI;">Microsoft JhengHei UI</td>
<td align="left">보통, 굵게, 가늘게</td>
<td align="left">중국어(번체)의 사용자 인터페이스 글꼴</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft YaHei UI;">Microsoft YaHei UI</td>
<td align="left">보통, 굵게, 가늘게</td>
<td align="left">중국어(간체)의 사용자 인터페이스 글꼴</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Myanmar Text;">Myanmar Text</td>
<td align="left">Regular</td>
<td align="left">미얀마 문자 스크립트의 대체 글꼴</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Nirmala UI;">Nirmala UI</td>
<td align="left">보통, Semilight, 굵게</td>
<td align="left">남아시아어 스크립트(벵골 문자, 데바나가리 문자, 구자라트 문자, 굴묵키 문자, 카나다 문자, 말라얄람 문자, 오디아 문자, 올 치키 문자, 스리랑카 문자, 소라 솜펭 문자, 타밀 문자, 텔루구 문자)의 사용자 인터페이스 글꼴</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: SimSun;">SimSun</td>
<td align="left">Regular</td>
<td align="left">레거시 중국어 UI 글꼴입니다. </td>
</tr>
<tr class="even">
<td align="left" style="font-family: Yu Gothic UI;">Yu Gothic UI</td>
<td align="left">약간 가늘게, Semilight, 보통, Semibold, 굵게</td>
<td align="left">일본어의 사용자 인터페이스 글꼴</td>
</tr>
</tbody>
</table>

## <a name="fonts"></a>Fonts

### <a name="sans-serif-fonts"></a>Sans-serif 글꼴

Sans-serif 글꼴은 제목 및 UI 요소에 적합합니다. 

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">글꼴 패밀리</th>
<th align="left">스타일</th>
<th align="left">참고</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left" style="font-family: Arial;">Arial</td>
<td align="left">보통, 기울임꼴, 굵게, 굵은 기울임꼴, 블랙</td>
<td align="left">유럽 및 중동 스크립트(라틴 문자, 그리스어, 키릴 자모, 아랍어, 아르메니아어 및 히브리어)를 지원하고 블랙 두께는 유럽 스크립트만 지원합니다.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Calibri;">Calibri</td>
<td align="left">보통, 기울임꼴, 굵게, 굵은 기울임꼴, 가늘게, 가는 기울임꼴</td>
<td align="left">유럽 및 중동 스크립트(라틴 문자, 그리스어, 키릴 자모, 아랍어 및 히브리어)를 지원합니다. 아랍어는 직립으로만 사용할 수 있습니다.</td>
</tr>
<td style="font-family: Consolas;">Consolas</td>
<td>보통, 기울임꼴, 굵게, 굵은 기울임꼴</td>
<td>유럽 스크립트(라틴 문자, 그리스어 및 키릴 자모)를 지원하는 고정 폭 글꼴입니다.</td>
</tr>

<tr>
<td style="font-family: Segoe UI;">Segoe UI</td>
<td>보통, 기울임꼴, 가는 기울임꼴, 검은 기울임꼴, 굵게, 굵은 기울임꼴, 가늘게, Semilight, Semibold, 검은색</td>
<td>유럽 및 중동 스크립트(아랍 문자, 아르메니아 문자, 키릴 자모, 그루지야 문자, 그리스 문자, 히브리 문자, 라틴 문자) 및 리쑤 문자 스크립트의 사용자 인터페이스 글꼴입니다.</td>
</tr>

<tr class="even">
<td style="font-family: Selawik;">Selawik</td>
<td align="left">보통, Semilight, 가늘게, 굵게, Semibold</td>
<td align="left">Segoe UI와 계측적으로 호환되는 오픈 소스 글꼴이며 Segoe UI를 번들로 만들지 않으려는 다른 플랫폼의 앱을 위한 글꼴입니다. <a href="https://github.com/Microsoft/Selawik">GitHub에서 Selawik를 가져옵니다.</a></td>
</tr>

</tbody>
</table>

### <a name="serif-fonts"></a>Serif 글꼴

Serif 글꼴은 대량 텍스트를 표시하는 데 적합합니다. 

<table>
<thead>
<tr class="header">
<th align="left">글꼴 패밀리</th>
<th align="left">스타일</th>
<th align="left">참고</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="font-family: Cambria;">Cambria</td>
<td align="left">Regular</td>
<td align="left">유럽 스크립트(라틴 문자, 그리스어, 키릴 자모)를 지원하는 Serif 글꼴입니다.</td>
</tr>
<tr class="even">
<td style="font-family: Courier New;">Courier New</td>
<td align="left">보통, 기울임꼴, 굵게, 굵은 기울임꼴</td>
<td align="left">Serif 고정 폭 글꼴은 유럽 및 중동 스크립트(라틴 문자, 그리스어, 키릴 자모, 아랍어, 아르메니아어 및 히브리어)를 지원합니다.</td>
</tr>
<tr class="odd">
<td style="font-family: Georgia;">그루지야</td>
<td align="left">보통, 기울임꼴, 굵게, 굵은 기울임꼴</td>
<td align="left">유럽 스크립트(라틴 문자, 그리스어 및 키릴 자모)를 지원합니다.</td>
</tr>

<tr class="even">
<td style="font-family: Times New Roman;">Times New Roman</td>
<td align="left">보통, 기울임꼴, 굵게, 굵은 기울임꼴</td>
<td align="left">유럽 스크립트(라틴 문자, 그리스어, 키릴 자모, 아랍어, 아르메니아어, 히브리어)를 지원하는 레거시 글꼴입니다.</td>
</tr>

</tbody>
</table>

### <a name="symbols-and-icons"></a>기호 및 아이콘

<table>
<thead>
<tr class="header">
<th align="left">글꼴 패밀리</th>
<th align="left">스타일</th>
<th align="left">참고</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Segoe MDL2 자산</td>
<td align="left">Regular</td>
<td align="left">앱 아이콘의 사용자 인터페이스 글꼴 자세한 내용은 <a href="segoe-ui-symbol-font.md">Segoe MDL2 자산 문서</a>를 참조하세요.</td>
</tr>
<tr class="even">
<td align="left">Segoe UI Emoji</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Segoe UI Symbol</td>
<td align="left">Regular</td>
<td align="left">기호의 대체 글꼴</td>
</tr>
</tbody>
</table>

## <a name="related-articles"></a>관련된 문서

* [텍스트 컨트롤](../controls-and-patterns/text-controls.md)
* [XAML 테마 리소스](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp)
* [XAML 스타일](../controls-and-patterns/xaml-styles.md)
* [Microsoft 입력 체계](https://docs.microsoft.com/typography/)
