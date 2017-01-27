---
author: Jwmsft
Description: "UWP 앱에 대한 글꼴을 선택하고 글꼴 크기와 색을 지정하는 경우 다음 지침을 따릅니다."
title: "UWP 앱의 글꼴"
ms.assetid: 1B8B90AD-CDC4-4997-ACDE-871C1E94A929
label: Fonts
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: 0b25dc91a5ec82a83ae24a41854e9eeab8990128

---


# <a name="fonts-for-uwp-apps"></a>UWP 앱의 글꼴

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

이 문서에는 UWP 앱에 권장되는 글꼴이 나열되어 있습니다. 이러한 글꼴은 UWP 앱을 지원하는 모든 Windows 10 버전에서 사용이 보장됩니다.

<div class="important-apis" >
<b>중요 API</b><br/>
<ul>
<li>[**FontFamily 속성**](https://msdn.microsoft.com/library/windows/apps/br209655)</li>
</ul>
</div>

[UWP 입력 체계 가이드](typography.md)는 앱에 Segoe UI 글꼴 사용을 권장합니다. Segoe UI가 대부분의 앱에 적합하긴 하지만 모든 앱에 사용할 필요는 없습니다. 독서 또는 영어가 아닌 특정 언어로 텍스트를 표시하는 경우 등 특정 시나리오에는 다른 글꼴을 사용할 수 있습니다. 
 
## <a name="sans-serif-fonts"></a>Sans-serif 글꼴

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
<th align="left">노트</th>
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

<tr class="odd">
<td>Segoe UI Historic</td>
<td align="left">보통</td>
<td align="left">기록 스크립트의 대체 글꼴</td>
</tr>

<tr class="even">
<td style="font-family: Selawik;">Selawik</td>
<td align="left">보통, Semilight, 가늘게, 굵게, Semibold</td>
<td align="left">Segoe UI와 계측적으로 호환되는 오픈 소스 글꼴이며 Segoe UI를 번들로 만들지 않으려는 다른 플랫폼의 앱을 위한 글꼴입니다. [Github에서 Selawik를 가져옵니다.](https://github.com/Microsoft/Selawik)</td>
</tr>

<tr class="even">
<td style="font-family: Verdana;">Verdana</td>
<td align="left">보통, 기울임꼴, 굵게, 굵은 기울임꼴</td>
<td align="left">유럽 스크립트(라틴 문자, 그리스어, 키릴 자모 및 아르메니아어)를 지원합니다.</td>
</tr>

</tbody>
</table>


## <a name="serif-fonts"></a>Serif 글꼴

Serif 글꼴은 대량 텍스트를 표시하는 데 적합합니다. 

<table>
<thead>
<tr class="header">
<th align="left">글꼴 패밀리</th>
<th align="left">스타일</th>
<th align="left">노트</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="font-family: Cambria;">Cambria</td>
<td align="left">보통</td>
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

## <a name="symbols-and-icons"></a>기호 및 아이콘


<table>
<thead>
<tr class="header">
<th align="left">글꼴 패밀리</th>
<th align="left">스타일</th>
<th align="left">노트</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Segoe MDL2 자산</td>
<td align="left">보통</td>
<td align="left">앱 아이콘의 사용자 인터페이스 글꼴 자세한 내용은 [Segoe MDL2 자산 문서](segoe-ui-symbol-font.md)를 참조하세요.</td>
</tr>
<tr class="even">
<td align="left">Segoe UI Emoji</td>
<td align="left">보통</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Segoe UI Symbol</td>
<td align="left">보통</td>
<td align="left">기호의 대체 글꼴</td>
</tr>
</tbody>
</table>



## <a name="fonts-for-non-latin-languages"></a>라틴어가 아닌 언어용 글꼴

하지만 이러한 글꼴 중 대부분은 라틴 문자를 제공합니다.

<table>
<thead>
<tr class="header">
<th align="left">글꼴 패밀리</th>
<th align="left">스타일</th>
<th align="left">노트</th>
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
<tr class="even">
<td align="left">자바어 텍스트 보통 자바어 스크립트의 대체 글꼴</td>
<td align="left">보통</td>
<td align="left">자바어 스크립트의 대체 글꼴</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Leelawadee UI;">Leelawadee UI</td>
<td align="left">보통, Semilight, 굵게</td>
<td align="left">동남 아시아어 스크립트(부기 문자, 라오스 문자, 크메르 문자, 태국 문자)의 사용자 인터페이스 글꼴</td>
</tr>

<tr class="odd">
<td align="left" style="font-family: Malgun Gothic;">Malgun Gothic</td>
<td align="left">보통</td>
<td align="left">한국어의 사용자 인터페이스 글꼴</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft Himalaya;">Microsoft Himalaya</td>
<td align="left">보통</td>
<td align="left">티베트어 스크립트의 대체 글꼴</td>
</tr>
<!--
<tr class="odd">
<td align="left" style="font-family: Microsoft JhengHei;">Microsoft JhengHei</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
-->
<tr class="even">
<td align="left" style="font-family: Microsoft JhengHei UI;">Microsoft JhengHei UI</td>
<td align="left">보통, 굵게, 가늘게</td>
<td align="left">중국어(번체)의 사용자 인터페이스 글꼴</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft New Tai Lue;">Microsoft New Tai Lue</td>
<td align="left">보통</td>
<td align="left">New Tai Lue 스크립트의 대체 글꼴</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft PhagsPa;">Microsoft PhagsPa</td>
<td align="left">보통</td>
<td align="left">파 스파 문자 스크립트의 대체 글꼴</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft Tai Le;">Microsoft Tai Le</td>
<td align="left">보통</td>
<td align="left">타이레 스크립트의 대체 글꼴</td>
</tr>
<!--
<tr class="even">
<td align="left" style="font-family: Microsoft YaHei;">Microsoft YaHei</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
-->
<tr class="odd">
<td align="left" style="font-family: Microsoft YaHei UI;">Microsoft YaHei UI</td>
<td align="left">보통, 굵게, 가늘게</td>
<td align="left">중국어(간체)의 사용자 인터페이스 글꼴</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft Yi Baiti;">Microsoft Yi Baiti</td>
<td align="left">보통</td>
<td align="left">이 문자 스크립트의 대체 글꼴</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Mongolian Baiti;">Mongolian Baiti</td>
<td align="left">보통</td>
<td align="left">몽골어 스크립트의 대체 글꼴</td>
</tr>
<tr class="even">
<td align="left" style="font-family: MV Boli;">MV Boli</td>
<td align="left">보통</td>
<td align="left">타나 문자 스크립트의 대체 글꼴</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Myanmar Text;">Myanmar Text</td>
<td align="left">보통</td>
<td align="left">미얀마 문자 스크립트의 대체 글꼴</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Nirmala UI;">Nirmala UI</td>
<td align="left">보통, Semilight, 굵게</td>
<td align="left">남아시아어 스크립트(벵골 문자, 데바나가리 문자, 구자라트 문자, 굴묵키 문자, 카나다 문자, 말라얄람 문자, 오디아 문자, 올 치키 문자, 스리랑카 문자, 소라 솜펭 문자, 타밀 문자, 텔루구 문자)의 사용자 인터페이스 글꼴</td>
</tr>

<tr class="odd">
<td align="left" style="font-family: SimSun;">SimSun</td>
<td align="left">보통</td>
<td align="left">레거시 중국어 UI 글꼴입니다. </td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Yu Gothic;">Yu Gothic</td>
<td align="left">Medium</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left" style="font-family: Yu Gothic UI;">Yu Gothic UI</td>
<td align="left">보통</td>
<td align="left">일본어의 사용자 인터페이스 글꼴</td>
</tr>
</tbody>
</table>


## <a name="globalizinglocalizing-fonts"></a>글꼴 세계화/지역화
특정 언어에 대한 권장 글꼴 패밀리, 크기, 두께 및 스타일에 프로그래밍 방식으로 액세스하려면 [LanguageFont 글꼴 매핑 API](https://msdn.microsoft.com/library/windows/apps/br206864)를 사용합니다. LanguageFont 개체는 UI 헤더, 알림, 본문, 사용자 편집 가능한 문서 본문 글꼴을 비롯하여 콘텐츠의 다양한 범주에 대한 올바른 글꼴 정보에 액세스합니다. 자세한 내용은 [세계화를 지원하도록 레이아웃 및 글꼴 조정](https://msdn.microsoft.com/windows/uwp/globalizing/adjust-layout-and-fonts--and-support-rtl)을 참조하세요.


## <a name="get-the-samples"></a>샘플 다운로드

* [다운로드 가능한 글꼴 샘플](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlCloudFontIntegration)
* [UI 기본 사항 샘플](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)
* [DirectWrite를 사용한 줄 간격 샘플](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/DWriteLineSpacingModes) 

## <a name="related-articles"></a>관련 문서

* [세계화를 지원하도록 레이아웃 및 글꼴 조정](https://msdn.microsoft.com/windows/uwp/globalizing/adjust-layout-and-fonts--and-support-rtl)
* [Segoe MDL2](segoe-ui-symbol-font.md)
* [텍스트 컨트롤](../controls-and-patterns/text-controls.md)
* [XAML 테마 리소스](https://msdn.microsoft.com/library/windows/apps/mt187274)

 

 







<!--HONumber=Dec16_HO2-->


