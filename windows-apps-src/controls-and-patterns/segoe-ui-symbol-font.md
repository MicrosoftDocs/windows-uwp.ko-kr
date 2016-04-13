---
Description: 이 문서에서는 Segoe MDL2 자산 글꼴과 함께 제공되는 문자 모양을 나열하고 사용 지침을 제공합니다.
Search.Refinement.TopicID: 184
title: Segoe MDL2 아이콘 지침
ms.assetid: DFB215C2-8A61-4957-B662-3B1991AC9BE1
label: Segoe MDL2 아이콘
template: detail.hbs
---

# Segoe MDL2 아이콘에 대한 지침


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**Symbol 열거(XAML)**](https://msdn.microsoft.com/library/windows/apps/dn252842)
-   [**AppBarIcon 열거(HTML)**](https://msdn.microsoft.com/library/windows/apps/hh770557)

이 문서에서는 Segoe MDL2 Assets 글꼴과 함께 제공되는 문자 모양을 나열하고 사용 지침을 제공합니다. 글꼴을 얻으려면 Windows 10을 설치해야 합니다.

## <span id="Recommendations"> </span> <span id="recommendations"> </span> <span id="RECOMMENDATIONS"> </span>권장 사항


-   **Segoe MDL2 자산** 글꼴을 명시적으로 지정할 수 있을 때만 이러한 문자 모양을 사용하세요.

## <span id="Additional_usage_guidance"> </span> <span id="additional_usage_guidance"> </span> <span id="ADDITIONAL_USAGE_GUIDANCE"> </span>추가 사용법 지침


Windows 8/8.1 **Segoe UI Symbol** 아이콘 글꼴은 Windows 10의 릴리스로 유효한 **Segoe MDL2 Assets** 글꼴로 대체되었습니다. 이전 글꼴과 거의 같은 방식에서 사용할 수 있지만 아이콘이 인쇄 기준선 대신 글꼴의 em 정사각형 내에 정렬되도록 글꼴 메트릭이 설정된 Windows 10 아이콘 스타일에서는 많은 문자 모양이 다시 그려졌습니다.

**참고** **Em**은 글꼴의 측정 단위입니다. 글꼴의 1 Em은 72ppi로 지정된 포인트 값의 100%와 같습니다. 예를 들어 16pt는 72ppi에서 16px과 같습니다(100% 플라토라고도 함). 새 MDL2 글꼴은 아이콘 영역의 공간이 정사각형 Em이 되도록 디자인되었습니다. 따라서 코드에서 너비와 높이를 16px로 지정하면 16x16px 아이콘 공간이 생성됩니다. 이는 아이콘이 항상 전체 공간을 차지함을 의미하는 것은 아닙니다.

 

**Segoe UI Symbol**을 계속 "레거시" 리소스로 사용할 수 있습니다. 그러나 새 **Segoe MDL2 Assets**를 사용하도록 모든 응용 프로그램을 업데이트하는 것이 좋습니다.

**Segoe MDL2 Assets** 글꼴에 포함된 아이콘 및 UI 컨트롤은 대부분 PUA(Private Use Area of Unicode)에 매핑됩니다. PUA를 통해 글꼴 개발자는 기존 코드 점에 매핑되지 않는 문자 모양에 개인 유니코드 값을 할당할 수 있습니다. 이 글꼴은 기호 글꼴을 만들 때 유용하지만 상호 운용성 문제가 발생될 수 있습니다. 글꼴을 사용할 수 없으면 문자 모양이 표시되지 않습니다. **Segoe MDL2 Assets** 글꼴을 지정할 수 있을 때만 이러한 문자 모양을 사용하세요.

타일 작업을 수행하는 경우에는 타일 글꼴을 지정할 수 없고 글꼴 대체를 통해 PUA 문자 모양을 사용할 수 없기 때문에 이러한 문자 모양을 사용할 수 없습니다.

**Segoe UI Symbol**과 달리 **Segoe MDL2 Assets** 글꼴의 아이콘은 텍스트와 함께 인라인으로 사용되지 않습니다. 즉, 점진적 표시 화살표와 같은 일부 이전 "힌트"가 더 이상 적용되지 않습니다. 마찬가지로, 모든 새 아이콘은 크기와 위치가 동일하게 지정되므로 너비를 0으로 만들 필요가 없습니다. 하나의 집합으로 작동하는지 확인하기만 하면 됩니다. 이상적으로, 하나의 집합으로 디자인된 두 개의 아이콘을 오버레이할 수 있습니다. 이는 코드에서 색 지정을 허용하기 위한 것입니다. 예를 들어 U+EA3A 및 U+EA3B는 시작 타일 배지 상태를 위해 만들어졌습니다. 이미 중앙에 배치되어 있기 때문에 원 채우기 색을 지정하여 다른 상태를 나타낼 수 있습니다.

Segoe UI Symbol은 “너비가 0”인 문자 모양을 계층화 및 색 지정에 사용합니다. 이 예제에서는 너비가 0인 빨간색 하트(U+E00B) 위에 검은색 윤곽선(U+E006)을 그릴 수 있습니다.

![폭이 0인 문자 모양 사용](images/segoe-ui-symbol-layering.png)

**Segoe MDL2 Assets**의 모든 문자 모양은 높이와 왼쪽 원점이 일관된 동일한 고정 너비를 가지므로 서로 위에 직접 문자 모양을 그려 계층화 및 색 지정 효과를 얻을 수 있습니다.

또한 많은 아이콘에는 아랍어, 페르시아어 및 히브리어와 같은 오른쪽에서 왼쪽 텍스트 방향을 사용하는 언어에 사용할 수 있는 미러된 양식이 있습니다.

C#/VB/C++ 및 XAML로 앱을 개발하는 경우 유니코드 ID 대신 [**Symbol enumeration**](https://msdn.microsoft.com/library/windows/apps/dn252842)을 사용하여 Segoe MDL2 자산 글꼴에서 글자 모양을 지정할 수 있습니다. JavaScript 및 HTML로 앱을 개발하는 경우 유니코드 ID 대신 [**AppBarIcon 열거형**](https://msdn.microsoft.com/library/windows/apps/hh770557)을 사용하여 **Segoe MDL2 자산** 글꼴에서 문자 모양을 지정할 수 있습니다.

**Segoe MDL2 Assets** 글꼴에는 여기에 나와 있는 것보다 훨씬 많은 아이콘이 포함되어 있다는 점을 기억해야 합니다. 이러한 아이콘은 대부분 특별한 용도에 사용되며 다른 곳에서 일반적으로 사용되지 않습니다.

## <span id="Hearts"> </span> <span id="hearts"> </span> <span id="HEARTS"> </span>하트


<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">코드</th>
<th align="left">기호</th>
<th align="left">열거형</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>U+E006</p></td>
<td align="left"><img src="images/heartlegacy.png" alt="HeartLegacy" /></td>
<td align="left"><p>HeartLegacy</p></td>
<td align="left"><p>윤곽선으로 표시된 하트</p></td>
</tr>
<tr class="even">
<td align="left"><p>U+E0A5</p></td>
<td align="left"><img src="images/heartfilllegacy.png" alt="HeartFillLegacy" /></td>
<td align="left"><p>HeartFillLegacy</p></td>
<td align="left"><p>단색 하트</p></td>
</tr>
<tr class="odd">
<td align="left"><p>U+E007</p></td>
<td align="left"><img src="images/heartbrokenlegacy.png" alt="HeartBrokenLegacy" /></td>
<td align="left"><p>HeartBrokenLegacy</p></td>
<td align="left"><p>깨진 하트</p></td>
</tr>
<tr class="even">
<td align="left"><p>U+E00B</p></td>
<td align="left"><img src="images/heartfillzerowidthlegacy.png" alt="HeartFillZeroWidthLegacy" /></td>
<td align="left"><p>HeartFillZeroWidthLegacy</p></td>
<td align="left"><p>단색 하트(0 너비)</p></td>
</tr>
<tr class="odd">
<td align="left"><p>U+E00C</p></td>
<td align="left"><img src="images/heartbrokenzerowidthlegacy.png" alt="HeartBrokenZeroWidthLegacy" /></td>
<td align="left"><p>HeartBrokenZeroWidthLegacy</p></td>
<td align="left"><p>깨진 하트(0 너비)</p></td>
</tr>
</tbody>
</table>

 

## <span id="Rating_stars"> </span> <span id="rating_stars"> </span> <span id="RATING_STARS"> </span>등급 별


<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">코드</th>
<th align="left">기호</th>
<th align="left">열거형</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">U+E224</td>
<td align="left"><img src="images/ratingstarlegacy.png" alt="RatingStarLegacy" /></td>
<td align="left">RatingStarLegacy</td>
<td align="left">윤곽선으로 표시된 별</td>
</tr>
<tr class="even">
<td align="left">U+E0B4</td>
<td align="left"><img src="images/ratingstarfilllegacy.png" alt="RatingStarFillLegacy" /></td>
<td align="left">RatingStarFillLegacy</td>
<td align="left">단색 별</td>
</tr>
<tr class="odd">
<td align="left">U+E00A</td>
<td align="left"><img src="images/ratingstarfillzerowidthlegacy.png" alt="RatingStarFillZeroWidthLegacy" /></td>
<td align="left">RatingStarFillZeroWidthLegacy</td>
<td align="left">단색 별(0 너비)</td>
</tr>
<tr class="even">
<td align="left">U+E082</td>
<td align="left"><img src="images/ratingstarfillreducedpaddinghtmllegacy.png" alt="RatingStarFillReducedPaddingHTMLLegacy" /></td>
<td align="left">RatingStarFillReducedPaddingHTMLLegacy</td>
<td align="left">단색 별(HTML에서 사용하도록 안쪽 여백이 줄어듬)</td>
</tr>
<tr class="odd">
<td align="left">U+E0B5</td>
<td align="left"><img src="images/ratingstarfillsmalllegacy.png" alt="RatingStarFillSmallLegacy" /></td>
<td align="left">RatingStarFillSmallLegacy</td>
<td align="left">작은 별</td>
</tr>
<tr class="even">
<td align="left">U+E7C6</td>
<td align="left"><img src="images/halfstarleft.png" alt="HalfStarLeft" /></td>
<td align="left">HalfStarLeft</td>
<td align="left">반쪽 별, 왼쪽</td>
</tr>
<tr class="odd">
<td align="left"><p>U+E7C7</p></td>
<td align="left"><img src="images/halfstarright.png" alt="HalfStarRight" /></td>
<td align="left">HalfStarRight</td>
<td align="left">반쪽 별, 오른쪽</td>
</tr>
</tbody>
</table>

 

## <span id="Checkbox_components"> </span> <span id="checkbox_components"> </span> <span id="CHECKBOX_COMPONENTS"> </span>확인란 구성 요소


|        |                                                                                |                                 |                         |
|--------|--------------------------------------------------------------------------------|---------------------------------|-------------------------|
| 코드   | 기호                                                                         | 열거형                            | 설명             |
| U+E001 | ![checkmarklegacy](images/checkmarklegacy.png)                                 | CheckMarkLegacy                 | 확인 표시              |
| U+E002 | ![checkboxfilllegacy](images/checkboxfilllegacy.png)                           | CheckboxFillLegacy              | 채워진 확인란         |
| U+E003 | ![checkboxlegacy](images/checkboxlegacy.png)                                   | CheckboxLegacy                  | 확인란                |
| U+E004 | ![checkboxindeterminatelegacy](images/checkboxindeterminatelegacy.png)         | CheckboxIndeterminateLegacy     | 확정되지 않은 상태     |
| U+E005 | ![checkboxcompositereversedlegacy](images/checkboxcompositereversedlegacy.png) | CheckboxCompositeReversedLegacy | 예약됨                |
| U+E008 | ![checkmarkzerowidthlegacy](images/checkmarkzerowidthlegacy.png)               | CheckMarkZeroWidthLegacy        | 확인 표시(0 너비) |
| U+E009 | ![checkboxfillzerowidthlegacy](images/checkboxfillzerowidthlegacy.png)         | CheckboxFillZeroWidthLegacy     | 채우기(0 너비)       |
| U+E0A2 | ![checkboxcompositelegacy](images/checkboxcompositelegacy.png)                 | CheckboxCompositeLegacy         | 복합               |
| U+E739 | ![확인란](images/checkbox.png)                                               | 확인란                        | 확인란                |
| U+E73A | ![checkboxcomposite](images/checkboxcomposite.png)                             | CheckboxComposite               | 복합 확인란      |
| U+E73B | ![checkboxfill](images/checkboxfill.png)                                       | CheckboxFill                    | 채워진 확인란         |
| U+E73C | ![checkboxindeterminate](images/checkboxindeterminate.png)                     | CheckboxIndeterminate           | 확정되지 않은 상태     |
| U+E73D | ![checkboxcompositereversed](images/checkboxcompositereversed.png)             | CheckboxCompositeReversed       | 반전된 복합      |
| U+E73E | ![checkmark](images/checkmark.png)                                             | CheckMark                       | 확인 표시              |

 

## <span id="Miscellaneous"> </span> <span id="miscellaneous"> </span> <span id="MISCELLANEOUS"> </span>기타


<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">코드</th>
<th align="left">기호</th>
<th align="left">열거형</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>U+E134</p></td>
<td align="left"><img src="images/commentlegacy.png" alt="CommentLegacy" /></td>
<td align="left">CommentLegacy</td>
<td align="left">설명</td>
</tr>
<tr class="even">
<td align="left"><p>U+E113</p></td>
<td align="left"><img src="images/favoritelegacy.png" alt="FavoriteLegacy" /></td>
<td align="left">FavoriteLegacy</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left"><p>U+E195</p></td>
<td align="left"><img src="images/unfavoritelegacy.png" alt="UnfavoriteLegacy" /></td>
<td align="left">UnfavoriteLegacy</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>U+E734</p></td>
<td align="left"><img src="images/favoritestar.png" alt="FavoriteStar" /></td>
<td align="left">FavoriteStar</td>
<td align="left">윤곽선으로 표시된 즐겨찾기</td>
</tr>
<tr class="odd">
<td align="left"><p>U+E735</p></td>
<td align="left"><img src="images/favoritestarfill.png" alt="FavoriteStarFill" /></td>
<td align="left">FavoriteStarFill</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>U+E8D9</p></td>
<td align="left"><img src="images/unfavorite.png" alt="Unfavorite" /></td>
<td align="left">Unfavorite</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left"><p>U+E19F</p></td>
<td align="left"><img src="images/likelegacy.png" alt="LikeLegacy" /></td>
<td align="left">LikeLegacy</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>U+E19E</p></td>
<td align="left"><img src="images/dislikelegacy.png" alt="DislikeLegacy" /></td>
<td align="left">DislikeLegacy</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left"><p>U+E19D</p></td>
<td align="left"><img src="images/likedislikelegacy.png" alt="LikeDislikeLegacy" /></td>
<td align="left">LikeDislikeLegacy</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>U+E116</p></td>
<td align="left"><img src="images/videolegacy.png" alt="VideoLegacy" /></td>
<td align="left">VideoLegacy</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left"><p>U+E714</p></td>
<td align="left"><img src="images/video.png" alt="Video" /></td>
<td align="left">비디오</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>U+E20B</p></td>
<td align="left"><img src="images/mailmessagelegacy.png" alt="MailMessageLegacy" /></td>
<td align="left">MailMessageLegacy</td>
<td align="left">메일 레거시</td>
</tr>
<tr class="odd">
<td align="left"><p>U+E248</p></td>
<td align="left"><img src="images/replylegacy.png" alt="ReplyLegacy" /></td>
<td align="left">ReplyLegacy</td>
<td align="left">회신</td>
</tr>
<tr class="even">
<td align="left"><p>U+E249</p></td>
<td align="left"><img src="images/favorite2legacy.png" alt="Favorite2Legacy" /></td>
<td align="left">Favorite2Legacy</td>
<td align="left">채워진 즐겨찾기</td>
</tr>
<tr class="odd">
<td align="left"><p>U+E24E</p></td>
<td align="left"><img src="images/unfavorite2legacy.png" alt="Unfavorite2Legacy" /></td>
<td align="left">Unfavorite2Legacy</td>
<td align="left">Unfavorite</td>
</tr>
<tr class="even">
<td align="left"><p>U+E25A</p></td>
<td align="left"><img src="images/mobilecontactlegacy.png" alt="MobileContactLegacy" /></td>
<td align="left">MobileContactLegacy</td>
<td align="left">모바일 연락처</td>
</tr>
<tr class="odd">
<td align="left"><p>U+E25B</p></td>
<td align="left"><img src="images/blockedlegacy.png" alt="BlockedLegacy" /></td>
<td align="left">BlockedLegacy</td>
<td align="left">차단된 연락처</td>
</tr>
<tr class="even">
<td align="left"><p>U+E25C</p></td>
<td align="left"><img src="images/typingindicatorlegacy.png" alt="TypingIndicatorLegacy" /></td>
<td align="left">TypingIndicatorLegacy</td>
<td align="left">입력 표시기</td>
</tr>
<tr class="odd">
<td align="left"><p>U+E25D</p></td>
<td align="left"><img src="images/presencechickletvideolegacy.png" alt="PresenceChickletVideoLegacy" /></td>
<td align="left">PresenceChickletVideoLegacy</td>
<td align="left">비디오 현재 상태 치클릿</td>
</tr>
<tr class="even">
<td align="left"><p>U+E25E</p></td>
<td align="left"><img src="images/presencechickletlegacy.png" alt="PresenceChickletLegacy" /></td>
<td align="left">PresenceChickletLegacy</td>
<td align="left">현재 상태 치클릿</td>
</tr>
</tbody>
</table>

 

## <span id="Scroll_bar_arrows"> </span> <span id="scroll_bar_arrows"> </span> <span id="SCROLL_BAR_ARROWS"> </span>스크롤 막대 화살표


| 코드   | 기호                                                                   | 열거형                         |
|--------|--------------------------------------------------------------------------|------------------------------|
| U+E00E | ![scrollchevronleftlegacy](images/scrollchevronleftlegacy.png)           | ScrollChevronLeftLegacy      |
| U+E00F | ![scrollchevronrightlegacy](images/scrollchevronrightlegacy.png)         | ScrollChevronRightLegacy     |
| U+E010 | ![scrollchevronuplegacy](images/scrollchevronuplegacy.png)               | ScrollChevronUpLegacy        |
| U+E011 | ![scrollchevrondownlegacy](images/scrollchevrondownlegacy.png)           | ScrollChevronDownLegacy      |
| U+E016 | ![scrollchevronleftboldlegacy](images/scrollchevronleftboldlegacy.png)   | ScrollChevronLeftBoldLegacy  |
| U+E017 | ![scrollchevronrightboldlegacy](images/scrollchevronrightboldlegacy.png) | ScrollChevronRightBoldLegacy |
| U+E018 | ![scrollchevronupboldlegacy](images/scrollchevronupboldlegacy.png)       | ScrollChevronUpBoldLegacy    |
| U+E019 | ![scrollchevrondownboldlegacy](images/scrollchevrondownboldlegacy.png)   | ScrollChevronDownBoldLegacy  |

 

## <span id="Back_buttons"> </span> <span id="back_buttons"> </span> <span id="BACK_BUTTONS"> </span>뒤로 단추


뒤로 단추에 대한 레거시 문자 모양은 바깥쪽 원의 두께가 20pt 및 42pt와 모두 일치되도록 두 가지 다른 크기로 제공됩니다. 두 개의 새 비례 크롬 뒤로 단추도 사용할 수 있습니다. 이러한 문자 모양을 계층화를 지원하도록 설계되었습니다.

| 코드   | 기호                                                                     | 열거형                          | 설명                               |
|--------|----------------------------------------------------------------------------|-------------------------------|-------------------------------------------|
| U+E0C4 | ![backbttnarrow20legacy](images/backbttnarrow20legacy.png)                 | BackBttnArrow20Legacy         | 뒤로 단추 화살표, 20pt                   |
| U+E0A6 | ![backbttnarrow42legacy](images/backbttnarrow42legacy.png)                 | BackBttnArrow42Legacy         | 뒤로 단추 화살표, 42pt                   |
| U+E0AD | ![backbttnmirroredarrow20legacy](images/backbttnmirroredarrow20legacy.png) | BackBttnMirroredArrow20Legacy | 미러된 반전 뒤로 단추 화살표, 20pt |
| U+E0AB | ![backbttnmirroredarrow42legacy](images/backbttnmirroredarrow42legacy.png) | BackBttnMirroredArrow42Legacy | 미러된 반전 뒤로 단추 화살표, 42pt |
| U+E830 | ![chromeback](images/chromeback.png)                                       | ChromeBack                    | 크롬 뒤로 단추                        |
| U+EA47 | ![chromebackmirrored](images/chromebackmirrored.png)                       | ChromeBackMirrored            | 미러된 크롬 뒤로 단추               |

 

## <span id="Back_arrows_for_HTML"> </span> <span id="back_arrows_for_html"> </span> <span id="BACK_ARROWS_FOR_HTML"> </span>HTML의 뒤로 화살표


이러한 문자 모양 주위에 원을 만들려면 코드를 더 추가합니다.

| 코드   | 기호                                                         | 열거형                    | 설명                |
|--------|----------------------------------------------------------------|-------------------------|----------------------------|
| U+E0D5 | ![arrowhtmllegacy](images/arrowhtmllegacy.png)                 | ArrowHTMLLegacy         | HTML용 화살표         |
| U+E0AE | ![arrowhtmlmirroredlegacy](images/arrowhtmlmirroredlegacy.png) | ArrowHTMLMirroredLegacy | U+E0D5의 미러된 버전 |

 

## <span id="AppBar_glyphs"> </span> <span id="appbar_glyphs"> </span> <span id="APPBAR_GLYPHS"> </span>AppBar 문자 모양


다음 목록의 문자 모양은 [**AppBar**](https://msdn.microsoft.com/library/windows/apps/br229670)에 사용합니다. 규칙에 따라 열거형 이름으로 참조되며, 원 없는 20x20px 아이콘으로 디자인되었습니다.

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">코드</th>
<th align="left">기호</th>
<th align="left">열거형</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">U+E8FB</td>
<td align="left"><img src="images/accept.png" alt="Accept" /></td>
<td align="left">Accept</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E910</td>
<td align="left"><img src="images/accounts.png" alt="Accounts" /></td>
<td align="left">Accounts</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E710</td>
<td align="left"><img src="images/add.png" alt="Add" /></td>
<td align="left">Add</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8FA</td>
<td align="left"><img src="images/addfriend.png" alt="Addfriend" /></td>
<td align="left">AddFriend</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E7EF</td>
<td align="left"><img src="images/admin.png" alt="Admin" /></td>
<td align="left">Admin</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8E3</td>
<td align="left"><img src="images/aligncenter.png" alt="Aligncenter" /></td>
<td align="left">AlignCenter</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8E4</td>
<td align="left"><img src="images/alignleft.png" alt="Alignleft" /></td>
<td align="left">AlignLeft</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8E2</td>
<td align="left"><img src="images/alignright.png" alt="Alignright" /></td>
<td align="left">Alignright</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E71D</td>
<td align="left"><img src="images/allapps.png" alt="Allapps" /></td>
<td align="left">AllApps</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E723</td>
<td align="left"><img src="images/attach.png" alt="Attach" /></td>
<td align="left">Attach</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A2</td>
<td align="left"><img src="images/attachcamera.png" alt="Attachcamera" /></td>
<td align="left">AttachCamera</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8D6</td>
<td align="left"><img src="images/audio.png" alt="Audio" /></td>
<td align="left">Audio</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E72B</td>
<td align="left"><img src="images/back.png" alt="Back" /></td>
<td align="left">Back</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E73F</td>
<td align="left"><img src="images/backtowindow.png" alt="Backtowindow" /></td>
<td align="left">BackToWindow</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8F8</td>
<td align="left"><img src="images/blockcontact.png" alt="Blockcontact" /></td>
<td align="left">BlockContact</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8DD</td>
<td align="left"><img src="images/bold.png" alt="Bold" /></td>
<td align="left">Bold</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A4</td>
<td align="left"><img src="images/bookmarks.png" alt="Bookmarks" /></td>
<td align="left">Bookmarks</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7C5</td>
<td align="left"><img src="images/browsephotos.png" alt="Browsephotos" /></td>
<td align="left">BrowsePhotos</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8FD</td>
<td align="left"><img src="images/bulletedlist.png" alt="Bulletedlist" /></td>
<td align="left">BulletedList</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8EF</td>
<td align="left"><img src="images/calculator.png" alt="Calculator" /></td>
<td align="left">Calculator</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E787</td>
<td align="left"><img src="images/calendar.png" alt="Calendar" /></td>
<td align="left">Calendar</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8BF</td>
<td align="left"><img src="images/calendarday.png" alt="Calendarday" /></td>
<td align="left">CalendarDay</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8F5</td>
<td align="left"><img src="images/calendarreply.png" alt="Calendarreply" /></td>
<td align="left">CalendarReply</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8C0</td>
<td align="left"><img src="images/calendarweek.png" alt="Calendarweek" /></td>
<td align="left">CalendarWeek</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E722</td>
<td align="left"><img src="images/camera.png" alt="Camera" /></td>
<td align="left">Camera</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E711</td>
<td align="left"><img src="images/cancel.png" alt="Cancel" /></td>
<td align="left">Cancel</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8BA</td>
<td align="left"><img src="images/caption.png" alt="Caption" /></td>
<td align="left">Caption</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7F0</td>
<td align="left"><img src="images/cc.png" alt="CC" /></td>
<td align="left">CC</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8EA</td>
<td align="left"><img src="images/cellphone.png" alt="Cellphone" /></td>
<td align="left">Cellphone</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8C1</td>
<td align="left"><img src="images/characters.png" alt="Characters" /></td>
<td align="left">Characters</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E894</td>
<td align="left"><img src="images/clear.png" alt="Clear" /></td>
<td align="left">Clear</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8E6</td>
<td align="left"><img src="images/clearselection.png" alt="Clearselection" /></td>
<td align="left">ClearSelection</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E89F</td>
<td align="left"><img src="images/closepane.png" alt="Closepane" /></td>
<td align="left">ClosePane</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E753</td>
<td align="left"><img src="images/cloud.png" alt="Cloud" /></td>
<td align="left">Cloud</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E90A</td>
<td align="left"><img src="images/comment.png" alt="Comment" /></td>
<td align="left">Comment</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E77B</td>
<td align="left"><img src="images/contact.png" alt="Contact" /></td>
<td align="left">Contact</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8D4</td>
<td align="left"><img src="images/contact2.png" alt="Contact2" /></td>
<td align="left">Contact2</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E779</td>
<td align="left"><img src="images/contactinfo.png" alt="Contactinfo" /></td>
<td align="left">ContactInfo</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8CF</td>
<td align="left"><img src="images/contactpresence.png" alt="Contactpresence" /></td>
<td align="left">ContactPresence</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8C8</td>
<td align="left"><img src="images/copy.png" alt="Copy" /></td>
<td align="left">Copy</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E7A8</td>
<td align="left"><img src="images/crop.png" alt="Crop" /></td>
<td align="left">Crop</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8C6</td>
<td align="left"><img src="images/cut.png" alt="Cut" /></td>
<td align="left">Cut</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E74D</td>
<td align="left"><img src="images/delete.png" alt="Delete" /></td>
<td align="left">Delete</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8F0</td>
<td align="left"><img src="images/directions.png" alt="Directions" /></td>
<td align="left">Directions</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8D8</td>
<td align="left"><img src="images/disableupdates.png" alt="Disableupdates" /></td>
<td align="left">DisableUpdates</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8CD</td>
<td align="left"><img src="images/disconnectdrive.png" alt="Disconnectdrive" /></td>
<td align="left">DisconnectDrive</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8E0</td>
<td align="left"><img src="images/dislike.png" alt="Dislike" /></td>
<td align="left">Dislike</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E90E</td>
<td align="left"><img src="images/dockbottom.png" alt="Dockbottom" /></td>
<td align="left">DockBottom</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E90C</td>
<td align="left"><img src="images/dockleft.png" alt="Dockleft" /></td>
<td align="left">DockLeft</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E90D</td>
<td align="left"><img src="images/dockright.png" alt="Dockright" /></td>
<td align="left">DockRight</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A5</td>
<td align="left"><img src="images/document.png" alt="Document" /></td>
<td align="left">Document</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E896</td>
<td align="left"><img src="images/download.png" alt="Download" /></td>
<td align="left">다운로드</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E70F</td>
<td align="left"><img src="images/edit.png" alt="Edit" /></td>
<td align="left">Edit</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E899</td>
<td align="left"><img src="images/emoji.png" alt="Emoji" /></td>
<td align="left">Emoji</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E76E</td>
<td align="left"><img src="images/emoji2.png" alt="Emoji2" /></td>
<td align="left">Emoji2</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E728</td>
<td align="left"><img src="images/favoritelist.png" alt="Favoritelist" /></td>
<td align="left">FavoriteList</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E734</td>
<td align="left"><img src="images/favoritestar.png" alt="Favoritestar" /></td>
<td align="left">FavoriteStar</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E735</td>
<td align="left"><img src="images/favoritestarfill.png" alt="Favoritestarfill" /></td>
<td align="left">FavoriteStarFill</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E71C</td>
<td align="left"><img src="images/filter.png" alt="Filter" /></td>
<td align="left">Filter</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E11A</td>
<td align="left"><img src="images/findlegacy.png" alt="Findlegacy" /></td>
<td align="left">FindLegacy</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E7C1</td>
<td align="left"><img src="images/flag.png" alt="Flag" /></td>
<td align="left">Flag</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8B7</td>
<td align="left"><img src="images/folder.png" alt="Folder" /></td>
<td align="left">Folder</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8D2</td>
<td align="left"><img src="images/font.png" alt="Font" /></td>
<td align="left">Font</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8D3</td>
<td align="left"><img src="images/fontcolor.png" alt="Fontcolor" /></td>
<td align="left">Fontcolor</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8E7</td>
<td align="left"><img src="images/fontdecrease.png" alt="Fontdecrease" /></td>
<td align="left">FontDecrease</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8E8</td>
<td align="left"><img src="images/fontincrease.png" alt="Fontincrease" /></td>
<td align="left">FontIncrease</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8E9</td>
<td align="left"><img src="images/fontsize.png" alt="Fontsize" /></td>
<td align="left">FontSize</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E72A</td>
<td align="left"><img src="images/forward.png" alt="Forward" /></td>
<td align="left">Forward</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E908</td>
<td align="left"><img src="images/fourbars.png" alt="Fourbars" /></td>
<td align="left">FourBars</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E740</td>
<td align="left"><img src="images/fullscreen.png" alt="Fullscreen" /></td>
<td align="left">FullScreen</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E774</td>
<td align="left"><img src="images/globe.png" alt="Globe" /></td>
<td align="left">Globe</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8AD</td>
<td align="left"><img src="images/go.png" alt="Go" /></td>
<td align="left">Go</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8FC</td>
<td align="left"><img src="images/gotostart.png" alt="Gotostart" /></td>
<td align="left">GoToStart</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8D1</td>
<td align="left"><img src="images/gototoday.png" alt="Gototoday" /></td>
<td align="left">GoToToday</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E778</td>
<td align="left"><img src="images/hangup.png" alt="Hangup" /></td>
<td align="left">Hangup</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E897</td>
<td align="left"><img src="images/help.png" alt="Help" /></td>
<td align="left">Help</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8C5</td>
<td align="left"><img src="images/hidebcc.png" alt="Hidebcc" /></td>
<td align="left">HideBCC</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7E6</td>
<td align="left"><img src="images/highlight.png" alt="Highlight" /></td>
<td align="left">Highlight</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E80F</td>
<td align="left"><img src="images/home.png" alt="Home" /></td>
<td align="left">Home</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8B5</td>
<td align="left"><img src="images/import.png" alt="Import" /></td>
<td align="left">Import</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8B6</td>
<td align="left"><img src="images/importall.png" alt="Importall" /></td>
<td align="left">ImportAll</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8C9</td>
<td align="left"><img src="images/important.png" alt="Important" /></td>
<td align="left">중요</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8DB</td>
<td align="left"><img src="images/italic.png" alt="Italic" /></td>
<td align="left">Italic</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E765</td>
<td align="left"><img src="images/keyboardclassic.png" alt="Keyboardclassic" /></td>
<td align="left">KeyboardClassic</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E89B</td>
<td align="left"><img src="images/leavechat.png" alt="Leavechat" /></td>
<td align="left">LeaveChat</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8F1</td>
<td align="left"><img src="images/library.png" alt="Library" /></td>
<td align="left">Library</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8E1</td>
<td align="left"><img src="images/like.png" alt="Like" /></td>
<td align="left">Like</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8DF</td>
<td align="left"><img src="images/likedislike.png" alt="Likedislike" /></td>
<td align="left">LikeDislike</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E71B</td>
<td align="left"><img src="images/link.png" alt="Link" /></td>
<td align="left">Link</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+EA37</td>
<td align="left"><img src="images/list.png" alt="List" /></td>
<td align="left">List</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E81D</td>
<td align="left"><img src="images/location.png" alt="Location" /></td>
<td align="left">위치</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E715</td>
<td align="left"><img src="images/mail.png" alt="Mail" /></td>
<td align="left">Mail</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A8</td>
<td align="left"><img src="images/mailfill.png" alt="Mailfill" /></td>
<td align="left">MailFill</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E89C</td>
<td align="left"><img src="images/mailforward.png" alt="Mailforward" /></td>
<td align="left">MailForward</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8CA</td>
<td align="left"><img src="images/mailreply.png" alt="Mailreply" /></td>
<td align="left">MailReply</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8C2</td>
<td align="left"><img src="images/mailreplyall.png" alt="Mailreplyall" /></td>
<td align="left">MailReplyAll</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E912</td>
<td align="left"><img src="images/manage.png" alt="Manage" /></td>
<td align="left">Manage</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8CE</td>
<td align="left"><img src="images/mapdrive.png" alt="Mapdrive" /></td>
<td align="left">MapDrive</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E707</td>
<td align="left"><img src="images/mappin.png" alt="Mappin" /></td>
<td align="left">Mappin</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E77C</td>
<td align="left"><img src="images/memo.png" alt="Memo" /></td>
<td align="left">Memo</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8BD</td>
<td align="left"><img src="images/message.png" alt="Message" /></td>
<td align="left">Message</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E720</td>
<td align="left"><img src="images/microphone.png" alt="Microphone" /></td>
<td align="left">Microphone</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E712</td>
<td align="left"><img src="images/more.png" alt="More" /></td>
<td align="left">More</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8DE</td>
<td align="left"><img src="images/movetofolder.png" alt="Movetofolder" /></td>
<td align="left">MoveToFolder</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E90B</td>
<td align="left"><img src="images/musicinfo.png" alt="Musicinfo" /></td>
<td align="left">MusicInfo</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E74F</td>
<td align="left"><img src="images/mute.png" alt="Mute" /></td>
<td align="left">Mute</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8F4</td>
<td align="left"><img src="images/newfolder.png" alt="Newfolder" /></td>
<td align="left">NewFolder</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E78B</td>
<td align="left"><img src="images/newwindow.png" alt="Newwindow" /></td>
<td align="left">NewWindow</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E893</td>
<td align="left"><img src="images/next.png" alt="Next" /></td>
<td align="left">Next</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E905</td>
<td align="left"><img src="images/onebar.png" alt="Onebar" /></td>
<td align="left">OneBar</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8E5</td>
<td align="left"><img src="images/openfile.png" alt="Openfile" /></td>
<td align="left">OpenFile</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8DA</td>
<td align="left"><img src="images/openlocal.png" alt="Openlocal" /></td>
<td align="left">OpenLocal</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A0</td>
<td align="left"><img src="images/openpane.png" alt="Openpane" /></td>
<td align="left">OpenPane</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7AC</td>
<td align="left"><img src="images/openwith.png" alt="Openwith" /></td>
<td align="left">OpenWith</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8B4</td>
<td align="left"><img src="images/orientation.png" alt="Orientation" /></td>
<td align="left">Orientation</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7EE</td>
<td align="left"><img src="images/otheruser.png" alt="Otheruser" /></td>
<td align="left">OtherUser</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E1CE</td>
<td align="left"><img src="images/outlinestarlegacy.png" alt="Outlinestarlegacy" /></td>
<td align="left">OutlineStarLegacy</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7C3</td>
<td align="left"><img src="images/page.png" alt="Page" /></td>
<td align="left">Page</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E77F</td>
<td align="left"><img src="images/paste.png" alt="Paste" /></td>
<td align="left">Paste</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E769</td>
<td align="left"><img src="images/pause.png" alt="Pause" /></td>
<td align="left">Pause</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E716</td>
<td align="left"><img src="images/people.png" alt="People" /></td>
<td align="left">People</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8D7</td>
<td align="left"><img src="images/permissions.png" alt="Permissions" /></td>
<td align="left">Permissions</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E717</td>
<td align="left"><img src="images/phone.png" alt="Phone" /></td>
<td align="left">Phone</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E780</td>
<td align="left"><img src="images/phonebook.png" alt="Phonebook" /></td>
<td align="left">PhoneBook</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E718</td>
<td align="left"><img src="images/pin.png" alt="Pin" /></td>
<td align="left">Pin</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E768</td>
<td align="left"><img src="images/play.png" alt="Play" /></td>
<td align="left">Play</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8F3</td>
<td align="left"><img src="images/postupdate.png" alt="Postupdate" /></td>
<td align="left">PostUpdate</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8FF</td>
<td align="left"><img src="images/preview.png" alt="Preview" /></td>
<td align="left">Preview</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A1</td>
<td align="left"><img src="images/previewlink.png" alt="Previewlink" /></td>
<td align="left">PreviewLink</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E892</td>
<td align="left"><img src="images/previous.png" alt="Previous" /></td>
<td align="left">Previous</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8D0</td>
<td align="left"><img src="images/priority.png" alt="Priority" /></td>
<td align="left">Priority</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8A6</td>
<td align="left"><img src="images/protecteddocument.png" alt="Protecteddocument" /></td>
<td align="left">ProtectedDocument</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8C3</td>
<td align="left"><img src="images/read.png" alt="Read" /></td>
<td align="left">Read</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7A6</td>
<td align="left"><img src="images/redo.png" alt="Redo" /></td>
<td align="left">Redo</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E72C</td>
<td align="left"><img src="images/refresh.png" alt="Refresh" /></td>
<td align="left">Refresh</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8AF</td>
<td align="left"><img src="images/remote.png" alt="Remote" /></td>
<td align="left">Remote</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E738</td>
<td align="left"><img src="images/remove.png" alt="Remove" /></td>
<td align="left">Remove</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8AC</td>
<td align="left"><img src="images/rename.png" alt="Rename" /></td>
<td align="left">Rename</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E90F</td>
<td align="left"><img src="images/repair.png" alt="Repair" /></td>
<td align="left">Repair</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8EE</td>
<td align="left"><img src="images/repeatall.png" alt="Repeatall" /></td>
<td align="left">RepeatAll</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8ED</td>
<td align="left"><img src="images/repeatone.png" alt="Repeatone" /></td>
<td align="left">RepeatOne</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E730</td>
<td align="left"><img src="images/reporthacked.png" alt="Reporthacked" /></td>
<td align="left">ReportHacked</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8EB</td>
<td align="left"><img src="images/reshare.png" alt="Reshare" /></td>
<td align="left">Reshare</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7AD</td>
<td align="left"><img src="images/rotate.png" alt="Rotate" /></td>
<td align="left">Rotate</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E89E</td>
<td align="left"><img src="images/rotatecamera.png" alt="Rotatecamera" /></td>
<td align="left">RotateCamera</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E74E</td>
<td align="left"><img src="images/save.png" alt="Save" /></td>
<td align="left">Save</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E78C</td>
<td align="left"><img src="images/savelocal.png" alt="Savelocal" /></td>
<td align="left">SaveLocal</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8FE</td>
<td align="left"><img src="images/scan.png" alt="Scan" /></td>
<td align="left">Scan</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8B3</td>
<td align="left"><img src="images/selectall.png" alt="Selectall" /></td>
<td align="left">SelectAll</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E724</td>
<td align="left"><img src="images/send.png" alt="Send" /></td>
<td align="left">Send</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E7B5</td>
<td align="left"><img src="images/setlockscreen.png" alt="Setlockscreen" /></td>
<td align="left">SetLockScreen</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E97B</td>
<td align="left"><img src="images/settile.png" alt="Settile" /></td>
<td align="left">SetTile</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E713</td>
<td align="left"><img src="images/settings.png" alt="Settings" /></td>
<td align="left">Settings</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E72D</td>
<td align="left"><img src="images/share.png" alt="Share" /></td>
<td align="left">Share</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E719</td>
<td align="left"><img src="images/shop.png" alt="Shop" /></td>
<td align="left">Shop</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8C4</td>
<td align="left"><img src="images/showbcc.png" alt="Showbcc" /></td>
<td align="left">ShowBCC</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8BC</td>
<td align="left"><img src="images/showresults.png" alt="Showresults" /></td>
<td align="left">ShowResults</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8B1</td>
<td align="left"><img src="images/shuffle.png" alt="Shuffle" /></td>
<td align="left">Shuffle</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E786</td>
<td align="left"><img src="images/slideshow.png" alt="Slideshow" /></td>
<td align="left">Slideshow</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E1CF</td>
<td align="left"><img src="images/solidstarlegacy.png" alt="Solidstarlegacy" /></td>
<td align="left">SolidStarLegacy</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8CB</td>
<td align="left"><img src="images/sort.png" alt="Sort" /></td>
<td align="left">Sort</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E71A</td>
<td align="left"><img src="images/stop.png" alt="Stop" /></td>
<td align="left">Stop</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E913</td>
<td align="left"><img src="images/street.png" alt="Street" /></td>
<td align="left">Street</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8AB</td>
<td align="left"><img src="images/switch.png" alt="Switch" /></td>
<td align="left">Switch</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8F9</td>
<td align="left"><img src="images/switchapps.png" alt="Switchapps" /></td>
<td align="left">SwitchApps</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E895</td>
<td align="left"><img src="images/sync.png" alt="Sync" /></td>
<td align="left">Sync</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8F7</td>
<td align="left"><img src="images/syncfolder.png" alt="Syncfolder" /></td>
<td align="left">SyncFolder</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8EC</td>
<td align="left"><img src="images/tag.png" alt="Tag" /></td>
<td align="left">Tag</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E907</td>
<td align="left"><img src="images/threebars.png" alt="Threebars" /></td>
<td align="left">ThreeBars</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7C9</td>
<td align="left"><img src="images/touchpointer.png" alt="Touchpointer" /></td>
<td align="left">TouchPointer</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E78A</td>
<td align="left"><img src="images/trim.png" alt="Trim" /></td>
<td align="left">Trim</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E906</td>
<td align="left"><img src="images/twobars.png" alt="Twobars" /></td>
<td align="left">TwoBars</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E89A</td>
<td align="left"><img src="images/twopage.png" alt="Twopage" /></td>
<td align="left">TwoPage</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8DC</td>
<td align="left"><img src="images/underline.png" alt="Underline" /></td>
<td align="left">Underline</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E7A7</td>
<td align="left"><img src="images/undo.png" alt="Undo" /></td>
<td align="left">Undo</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8D9</td>
<td align="left"><img src="images/unfavorite.png" alt="Unfavorite" /></td>
<td align="left">UnFavorite</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E77A</td>
<td align="left"><img src="images/unpin.png" alt="Unpin" /></td>
<td align="left">UnPin</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8F6</td>
<td align="left"><img src="images/unsyncfolder.png" alt="Unsyncfolder" /></td>
<td align="left">UnSyncFolder</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E74A</td>
<td align="left"><img src="images/up.png" alt="Up" /></td>
<td align="left">Up</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E898</td>
<td align="left"><img src="images/upload.png" alt="Upload" /></td>
<td align="left">Upload</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8AA</td>
<td align="left"><img src="images/videochat.png" alt="Videochat" /></td>
<td align="left">VideoChat</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E890</td>
<td align="left"><img src="images/view.png" alt="View" /></td>
<td align="left">View</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A9</td>
<td align="left"><img src="images/viewall.png" alt="Viewall" /></td>
<td align="left">ViewAll</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E767</td>
<td align="left"><img src="images/volume.png" alt="Volume" /></td>
<td align="left">Volume</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8B8</td>
<td align="left"><img src="images/webcam.png" alt="Webcam" /></td>
<td align="left">Webcam</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E909</td>
<td align="left"><img src="images/world.png" alt="World" /></td>
<td align="left">World</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E904</td>
<td align="left"><img src="images/zerobars.png" alt="Zerobars" /></td>
<td align="left">ZeroBars</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E71E</td>
<td align="left"><img src="images/zoom.png" alt="Zoom" /></td>
<td align="left">Zoom</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A3</td>
<td align="left"><img src="images/zoomin.png" alt="Zoomin" /></td>
<td align="left">ZoomIn</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E71F</td>
<td align="left"><img src="images/zoomout.png" alt="Zoomout" /></td>
<td align="left">ZoomOut</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

## <span id="Battery_icons"> </span> <span id="battery_icons"> </span> <span id="BATTERY_ICONS"> </span>배터리 아이콘


| 코드 | 기호                                             | 열거형              | 코드 | 기호                                                  | 열거형                |
|------|----------------------------------------------------|-------------------|------|---------------------------------------------------------|---------------------|
| E996 | ![batteryunknown](images/batteryunknown.png)       | BatteryUnknown    | EC02 | ![mobbatteryunknown](images/mobbatteryunknown.png)      | MobBatteryUnknown   |
| E850 | ![battery0](images/battery0.png)                   | Battery0          | EBA0 | ![mobbattery0](images/mobbattery0.png)                  | MobBattery0         |
| E851 | ![battery1](images/battery1.png)                   | Battery1          | EBA1 | ![mobbattery1](images/mobbattery1.png)                  | MobBattery1         |
| E852 | ![battery2](images/battery2.png)                   | Battery2          | EBA2 | ![mobbattery2](images/mobbattery2.png)                  | MobBattery2         |
| E853 | ![battery3](images/battery3.png)                   | Battery3          | EBA3 | ![mobbattery3](images/mobbattery3.png)                  | MobBattery3         |
| E854 | ![battery4](images/battery4.png)                   | Battery4          | EBA4 | ![mobbattery4](images/mobbattery4.png)                  | MobBattery4         |
| E855 | ![battery5](images/battery5.png)                   | Battery5          | EBA5 | ![mobbattery5](images/mobbattery5.png)                  | MobBattery5         |
| E856 | ![battery6](images/battery6.png)                   | Battery6          | EBA6 | ![mobbattery6](images/mobbattery6.png)                  | MobBattery6         |
| E857 | ![battery7](images/battery7.png)                   | Battery7          | EBA7 | ![mobbattery7](images/mobbattery7.png)                  | MobBattery7         |
| E858 | ![battery8](images/battery8.png)                   | Battery8          | EBA8 | ![mobbattery8](images/mobbattery8.png)                  | MobBattery8         |
| E859 | ![battery9](images/battery9.png)                   | Battery9          | EBA9 | ![mobbattery9](images/mobbattery9.png)                  | MobBattery9         |
| E83F | ![battery10](images/battery10.png)                 | Battery10         | EBA0 | ![mobbatter10](images/mobbattery10.png)                 | MobBatter10         |
| E85A | ![batterycharging0](images/batterycharging0.png)   | BatteryCharging0  | EBAB | ![mobbatterycharging0](images/mobbatterycharging0.png)  | MobBatteryCharging0 |
| E85B | ![batterycharging1](images/batterycharging1.png)   | BatteryCharging1  | EBAC | ![mobbatterycharging1](images/mobbatterycharging1.png)  | MobBatteryCharging1 |
| E85C | ![batterycharging2](images/batterycharging2.png)   | BatteryCharging2  | EBAD | ![mobbatterycharging2](images/mobbatterycharging2.png)  | MobBatteryCharging2 |
| E85D | ![batterycharging3](images/batterycharging3.png)   | BatteryCharging3  | EBAE | ![mobbatterycharging3](images/mobbatterycharging3.png)  | MobBatteryCharging3 |
| E85E | ![batterycharging4](images/batterycharging4.png)   | BatteryCharging4  | EBAF | ![mobbatterycharging4](images/mobbatterycharging4.png)  | MobBatteryCharging4 |
| E85F | ![batterycharging5](images/batterycharging5.png)   | BatteryCharging5  | EBB0 | ![mobbatterycharging5](images/mobbatterycharging5.png)  | MobBatteryCharging5 |
| E860 | ![batterycharging6](images/batterycharging6.png)   | BatteryCharging6  | EBB1 | ![mobbatterycharging6](images/mobbatterycharging6.png)  | MobBatteryCharging6 |
| E861 | ![batterycharging7](images/batterycharging7.png)   | BatteryCharging7  | EBB2 | ![mobbatterycharging7](images/mobbatterycharging7.png)  | MobBatteryCharging7 |
| E862 | ![batterycharging8](images/batterycharging8.png)   | BatteryCharging8  | EBB3 | ![mobbatterycharging8](images/mobbatterycharging8.png)  | MobBatteryCharging8 |
| E83E | ![batterycharging9](images/batterycharging9.png)   | BatteryCharging9  | EBB4 | ![mobbatterycharging9](images/mobbatterycharging9.png)  | MobBatteryCharging9 |
| ea93 | ![batterycharging10](images/batterycharging10.png) | BatteryCharging10 | EBB5 | ![mobbatterychargin10](images/mobbatterycharging10.png) | MobBatteryChargin10 |
| E863 | ![batterysaver0](images/batterysaver0.png)         | BatterySaver0     | EBB6 | ![mobbatterysaver0](images/mobbatterysaver0.png)        | MobBatterySaver0    |
| E864 | ![batterysaver1](images/batterysaver1.png)         | BatterySaver1     | EBB7 | ![mobbatterysaver1](images/mobbatterysaver1.png)        | MobBatterySaver1    |
| E865 | ![batterysaver2](images/batterysaver2.png)         | BatterySaver2     | EBB8 | ![mobbatterysaver2](images/mobbatterysaver2.png)        | MobBatterySaver2    |
| E866 | ![batterysaver3](images/batterysaver3.png)         | BatterySaver3     | EBB9 | ![mobbatterysaver3](images/mobbatterysaver3.png)        | MobBatterySaver3    |
| E867 | ![batterysaver4](images/batterysaver4.png)         | BatterySaver4     | EBBA | ![mobbatterysaver4](images/mobbatterysaver4.png)        | MobBatterySaver4    |
| E868 | ![batterysaver5](images/batterysaver5.png)         | BatterySaver5     | EBBB | ![mobbatterysaver5](images/mobbatterysaver5.png)        | MobBatterySaver5    |
| E869 | ![batterysaver6](images/batterysaver6.png)         | BatterySaver6     | EBBC | ![mobbatterysaver6](images/mobbatterysaver6.png)        | MobBatterySaver6    |
| E86A | ![batterysaver7](images/batterysaver7.png)         | BatterySaver7     | EBBD | ![mobbatterysaver7](images/mobbatterysaver7.png)        | MobBatterySaver7    |
| E86B | ![batterysaver8](images/batterysaver8.png)         | BatterySaver8     | EBBE | ![mobbatterysaver8](images/mobbatterysaver8.png)        | MobBatterySaver8    |
| EA94 | ![batterysaver9](images/batterysaver9.png)         | BatterySaver9     | EBBF | ![mobbatterysaver9](images/mobbatterysaver9.png)        | MobBatterySaver9    |
| EA95 | ![batterysaver10](images/batterysaver10.png)       | BatterySaver10    | EBC0 | ![mobbatterysaver10](images/mobbatterysaver10.png)      | MobBatterySaver10   |

 

## <span id="related_topics"> </span>관련 항목


**디자이너용**
* [글꼴에 대한 지침](fonts.md)
* [RTL(오른쪽에서 왼쪽)로 작성되는 언어에 대한 W3C](http://www.i18nguy.com/temp/rtl.mdl)
**개발자용(XAML)**
* [**Symbol 열거형**](https://msdn.microsoft.com/library/windows/apps/dn252842)


 






<!--HONumber=Mar16_HO1-->


