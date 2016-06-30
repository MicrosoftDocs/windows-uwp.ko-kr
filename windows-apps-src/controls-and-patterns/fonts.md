---
author: Jwmsft
Description: "글꼴을 선택하고 글꼴 크기와 색을 지정하는 경우 다음 지침을 따릅니다."
title: "글꼴"
ms.assetid: 1B8B90AD-CDC4-4997-ACDE-871C1E94A929
label: Fonts
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 268884c011caa18f3afb6d8b0d9cfda1ec27f51e

---

# 글꼴에 대한 지침

**중요 API**

-   [**FontFamily 속성**](https://msdn.microsoft.com/library/windows/apps/br209655)

글꼴 크기, 두께, 색, 자간 및 공백을 적절하게 사용하면 UWP(유니버설 Windows 플랫폼) 앱이 간결하게 정리되어 사용하기 쉬워집니다. 글꼴을 선택하고 글꼴 크기와 색을 지정하는 경우 다음 지침을 따릅니다.

Segoe UI Symbol 아이콘 목록을 찾는 경우 [**Guidelines for Segoe UI Symbol icons**](segoe-ui-symbol-font.md)을 참조하세요.

## <span id="The_Windows_10_type_ramp"></span><span id="the_windows_10_type_ramp"></span><span id="THE_WINDOWS_10_TYPE_RAMP"></span>Windows 10 형식 램프


유형 램프는 헤드라인과 본문 텍스트 간에 중요한 디자인 관계를 설정하고 각 수준 간의 명확하고 쉽게 이해할 수 있는 계층 구조를 보장합니다. 사용자는 정보를 찾을 위치와 페이지를 구문 분석하는 방법을 즉시 이해할 수 있습니다.

UWP 앱에 대해 권장하는 형식 램프는 다음과 같습니다.

| 텍스트 스타일 | 서체 | 두께    | 크기(epx) | 줄 간격(epx) | 단어 간격 | 자간(1/1000em) | XAML 스타일 키          |
|------------|----------|-----------|------------|--------------------|--------------|----------------------|-------------------------|
| 헤더     | Segoe UI | 가늘게     | 46         | 56                 | 100%         | 0                    | HeaderTextBlockStyle    |
| 하위 머리글  | Segoe UI | 가늘게     | 34         | 40                 | 100%         | 0                    | SubheaderTextBlockStyle |
| 제목      | Segoe UI | Semilight | 24         | 28                 | 100%         | 0                    | TitleTextBlockStyle     |
| 부제목   | Segoe UI | 보통   | 20         | 24                 | 100%         | 0                    | SubtitleTextBlockStyle  |
| 기본       | Segoe UI | Semibold  | 15         | 20                 | 100%         | 0                    | BaseTextBlockStyle      |
| 본문       | Segoe UI | 보통   | 15         | 20                 | 100%         | 0                    | BodyTextBlockStyle      |
| 설명    | Segoe UI | 보통   | 12         | 14                 | 100%         | 0                    | CaptionTextBlockStyle   |

 

## <span id="Recommended_fonts"></span><span id="recommended_fonts"></span><span id="RECOMMENDED_FONTS"></span>권장되는 글꼴


모든 항목에 Segoe UI 글꼴을 사용할 필요는 없습니다. 독서 또는 영어가 아닌 언어로 텍스트를 표시하는 경우 등 특정 시나리오에는 다른 글꼴을 사용할 수 있습니다.

UWP 앱을 지원하는 모든 Windows 10 버전에서 사용이 보장되는 글꼴 목록은 다음과 같습니다.

**참고** 이 목록에 없는 글꼴을 사용하는 경우 앱이 Microsoft 서비스에서 글꼴 데이터를 자동으로 다운로드하도록 트리거할 수 있습니다. 이렇게 할 경우 특히 모바일 디바이스에서 문제가 될 수 있는 성능 및 기타 영향이 있을 수 있습니다. 특히, 사용자의 모바일 데이터 요금제가 사용되거나 모바일 데이터 사용 요금이 부과될 수 있습니다. 모바일 디바이스에서 사용할 수 있는 UWP 앱은 이 목록에 있는 글꼴 이외의 글꼴을 UI 콘텐츠에 사용해서는 안 됩니다.

 

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
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Arial</td>
<td align="left">보통, 기울임꼴, 굵게, 굵은 기울임꼴, 블랙</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Calibri</td>
<td align="left">보통, 기울임꼴, 굵게, 굵은 기울임꼴, 가늘게, 가는 기울임꼴</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Cambria</td>
<td align="left">보통</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Cambria Math</td>
<td align="left">보통</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Comic Sans MS</td>
<td align="left">보통, 기울임꼴, 굵게, 굵은 기울임꼴</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Courier New</td>
<td align="left">보통, 기울임꼴, 굵게, 굵은 기울임꼴</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Ebrima</td>
<td align="left">보통, 굵게</td>
<td align="left">아프리카어 스크립트(게에즈 문자, 은코 문자, 오스만 문자, 티푸나구 문자, 바이 문자)의 사용자 인터페이스 글꼴</td>
</tr>
<tr class="even">
<td align="left">Gadugi</td>
<td align="left">보통</td>
<td align="left">북아메리카어 스크립트(Canadian Syllabics, 체로키 문자)의 사용자 인터페이스 글꼴</td>
</tr>
<tr class="odd">
<td align="left">그루지야</td>
<td align="left">보통, 기울임꼴, 굵게, 굵은 기울임꼴</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">자바어 텍스트 보통 자바어 스크립트의 대체 글꼴</td>
<td align="left">보통</td>
<td align="left">자바어 스크립트의 대체 글꼴</td>
</tr>
<tr class="odd">
<td align="left">Leelawadee UI</td>
<td align="left">보통, Semilight, 굵게</td>
<td align="left">동남 아시아어 스크립트(부기 문자, 라오스 문자, 크메르 문자, 태국 문자)의 사용자 인터페이스 글꼴</td>
</tr>
<tr class="even">
<td align="left">Lucida Console</td>
<td align="left">보통</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Malgun Gothic</td>
<td align="left">보통</td>
<td align="left">한국어의 사용자 인터페이스 글꼴</td>
</tr>
<tr class="even">
<td align="left">Microsoft Himalaya</td>
<td align="left">보통</td>
<td align="left">티베트어 스크립트의 대체 글꼴</td>
</tr>
<tr class="odd">
<td align="left">Microsoft JhengHei</td>
<td align="left">보통</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Microsoft JhengHei UI</td>
<td align="left">보통</td>
<td align="left">중국어(번체)의 사용자 인터페이스 글꼴</td>
</tr>
<tr class="odd">
<td align="left">Microsoft New Tai Lue</td>
<td align="left">보통</td>
<td align="left">New Tai Lue 스크립트의 대체 글꼴</td>
</tr>
<tr class="even">
<td align="left">Microsoft PhagsPa</td>
<td align="left">보통</td>
<td align="left">파 스파 문자 스크립트의 대체 글꼴</td>
</tr>
<tr class="odd">
<td align="left">Microsoft Tai Le</td>
<td align="left">보통</td>
<td align="left">타이레 스크립트의 대체 글꼴</td>
</tr>
<tr class="even">
<td align="left">Microsoft YaHei</td>
<td align="left">보통</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Microsoft YaHei UI</td>
<td align="left">보통</td>
<td align="left">중국어(간체)의 사용자 인터페이스 글꼴</td>
</tr>
<tr class="even">
<td align="left">Microsoft Yi Baiti</td>
<td align="left">보통</td>
<td align="left">이 문자 스크립트의 대체 글꼴</td>
</tr>
<tr class="odd">
<td align="left">Mongolian Baiti</td>
<td align="left">보통</td>
<td align="left">몽골어 스크립트의 대체 글꼴</td>
</tr>
<tr class="even">
<td align="left">MV Boli</td>
<td align="left">보통</td>
<td align="left">타나 문자 스크립트의 대체 글꼴</td>
</tr>
<tr class="odd">
<td align="left">Myanmar Text</td>
<td align="left">보통</td>
<td align="left">미얀마 문자 스크립트의 대체 글꼴</td>
</tr>
<tr class="even">
<td align="left">Nirmala UI</td>
<td align="left">보통, Semilight, 굵게</td>
<td align="left">남아시아어 스크립트(벵골 문자, 데바나가리 문자, 구자라트 문자, 굴묵키 문자, 카나다 문자, 말라얄람 문자, 오디아 문자, 올 치키 문자, 스리랑카 문자, 소라 솜펭 문자, 타밀 문자, 텔루구 문자)의 사용자 인터페이스 글꼴</td>
</tr>
<tr class="odd">
<td align="left">Segoe MDL2 자산</td>
<td align="left">보통</td>
<td align="left">앱 아이콘의 사용자 인터페이스 글꼴</td>
</tr>
<tr class="even">
<td align="left">맑은 고딕</td>
<td align="left">보통</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Segoe UI</td>
<td align="left">보통, 기울임꼴, 굵게, 굵게 기울임꼴, 가늘게, Semilight, Semibold, 블랙</td>
<td align="left">유럽 및 중동 스크립트(아랍 문자, 아르메니아 문자, 키릴 자모, 그루지야 문자, 그리스 문자, 히브리 문자, 라틴 문자) 및 리쑤 문자 스크립트의 사용자 인터페이스 글꼴</td>
</tr>
<tr class="even">
<td align="left">Segoe UI Emoji</td>
<td align="left">보통</td>
<td align="left">Windows Phone에 제공되는 버전에는 컬러 배경에서도 눈에 띄도록 각 이모티콘 주위에 흰색 윤곽선이 포함되어 있습니다. Windows에 제공되는 버전과 계측적으로 호환됩니다.</td>
</tr>
<tr class="odd">
<td align="left">Segoe UI Historic</td>
<td align="left">보통</td>
<td align="left">기록 스크립트의 대체 글꼴</td>
</tr>
<tr class="even">
<td align="left">Segoe UI Symbol</td>
<td align="left">보통</td>
<td align="left">기호의 대체 글꼴</td>
</tr>
<tr class="odd">
<td align="left">SimSun</td>
<td align="left">보통</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Times New Roman</td>
<td align="left">보통, 기울임꼴, 굵게, 굵은 기울임꼴</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Trebuchet MS</td>
<td align="left">보통, 기울임꼴, 굵게, 굵은 기울임꼴</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Verdana</td>
<td align="left">보통, 기울임꼴, 굵게, 굵은 기울임꼴</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Webdings</td>
<td align="left">보통</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Wingdings</td>
<td align="left">보통</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Yu Gothic</td>
<td align="left">Medium</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Yu Gothic UI</td>
<td align="left">보통</td>
<td align="left">일본어의 사용자 인터페이스 글꼴</td>
</tr>
</tbody>
</table>

 

## <span id="related_topics"></span>관련 항목

**디자이너용**
* [레이블(또는 텍스트 블록)](labels.md)
* [Segoe UI Symbol 아이콘](segoe-ui-symbol-font.md) 
           **개발자용(XAML)**
* [XAML 테마 리소스](https://msdn.microsoft.com/library/windows/apps/mt187274)
* [앱 페이지 레이아웃](https://msdn.microsoft.com/library/windows/apps/hh872191)
* [Segoe UI Symbol 아이콘](segoe-ui-symbol-font.md)
* [**TextBlock.FontFamily 속성**](https://msdn.microsoft.com/library/windows/apps/br209655)

**샘플**
* [XAML 텍스트 표시 샘플](http://go.microsoft.com/fwlink/p/?linkid=238578)
* [CSS 스타일: 앱 샘플 브랜딩](http://go.microsoft.com/fwlink/p/?linkid=231641)
* [언어 글꼴 매핑 샘플](http://go.microsoft.com/fwlink/p/?linkid=231603)
 

 







<!--HONumber=Jun16_HO4-->


