---
author: DelfCo
Description: "세계화는 앱이 다른 글로벌 시장에서 변경이나 사용자 지정 없이도 제대로 작동하도록 앱을 디자인 및 개발하는 프로세스입니다."
Search.SourceType: Video
title: "세계화 및 지역화"
ms.assetid: c0791eec-5bb8-4a13-8977-61d7d98e35ce
label: Intro
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: a34b682cc903555b9d6f64636c3d1cf7bee75b5d

---

# 세계화 및 지역화




Windows는 전 세계에서 문화, 지역 및 언어가 다양한 사용자가 사용합니다. 사용자는 어느 언어든 말할 수 있고 여러 언어를 사용할 수도 있습니다. 지역도 세계 어디든 있을 수 있어서 위치와 언어가 다양할 수 있습니다. *세계화* 및 *지역화*를 사용하여 앱을 쉽게 조정할 수 있도록 디자인하면 앱의 잠재 시장을 늘릴 수 있습니다.

**세계화**는 앱이 다른 글로벌 시장에서 변경이나 사용자 지정 없이도 제대로 작동하도록 앱을 디자인 및 개발하는 프로세스입니다.

예를 들어 다음과 같이 할 수 있습니다.

-   레이블 및 텍스트 문자열에서 다른 언어의 다른 텍스트 길이 및 글꼴 크기를 수용하도록 앱의 레이아웃 디자인
-   텍스트 및 문화권에 종속된 이미지를 앱의 코드나 태그에 하드 코딩하는 대신에 다른 지역 시장에 맞게 조정할 수 있는 리소스에서 검색
-   숫자 값, 날짜, 시간 및 통화 같이 지역에 따라 형식이 다른 데이터를 세계화 API를 사용하여 표시

**지역화**는 특정 지역 시장의 언어, 문화 및 정치적 요구 사항에 맞게 앱을 조정하는 프로세스입니다.

예를 들면 다음과 같습니다.

-   앱의 텍스트 및 레이블을 새 시장에 맞게 번역하고 해당 언어용 리소스를 별도로 만들기
-   필요에 따라 문화권에 종속된 모든 이미지를 수정하고 별도의 리소스에 배치

세계에 판매하기 위해 앱을 준비하는 방법을 간략히 소개하는 [세계화 및 지역화 소개](https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-globalization-and-localization) 동영상을 시청하세요.

## 기사
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">문서</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[권장 사항 및 금지 사항](guidelines-and-checklist-for-globalizing-your-app.md)</p></td>
<td align="left"><p>앱을 광범위한 대상에 대해 세계화하거나 특정 시장을 위해 지역화하는 모범 사례를 따르세요.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[세계화를 대비한 형식 사용](use-global-ready-formats.md)</p></td>
<td align="left"><p>날짜, 시간, 숫자 및 통화의 형식을 적절하게 지정하여 세계화를 대비한 앱을 개발합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[언어 및 지역 관리](manage-language-and-region.md)</p></td>
<td align="left"><p>Windows에서 제공되는 다양한 언어 및 지역 설정을 사용하여 Windows에서 UI 리소스를 선택하고 앱의 UI 요소 형식을 지정하는 방법을 제어합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[날짜 및 시간 형식 지정 패턴 사용](use-patterns-to-format-dates-and-times.md)</p></td>
<td align="left"><p>날짜 및 시간을 원하는 형식으로 정확히 표시하려면 [<strong>Windows.Globalization.DateTimeFormatting</strong>](https://msdn.microsoft.com/library/windows/apps/br206859) API와 사용자 지정 패턴을 사용하세요.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[레이아웃 및 글꼴 조정, RTL 지원](adjust-layout-and-fonts--and-support-rtl.md)</p></td>
<td align="left"><p>RTL(오른쪽에서 왼쪽) 방향으로 읽는 것을 포함하여 여러 언어의 레이아웃과 글꼴을 지원하기 위해 앱을 개발합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[앱 지역화 준비](prepare-your-app-for-localization.md)</p></td>
<td align="left"><p>다른 시장, 언어 또는 지역으로의 앱 지역화를 준비합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[리소스에 UI 문자열 배치](put-ui-strings-into-resources.md)</p></td>
<td align="left"><p>UI용 문자열 리소스를 리소스 파일에 넣습니다. 그러면 코드 또는 태그로부터 해당 문자열을 참조할 수 있습니다.</p></td>
</tr>
</tbody>
</table>

 

UWP(유니버설 Windows 플랫폼) 앱 및 Windows 10에도 적용되며 원래 Windows 8.x용으로 작성된 설명서를 참조하세요.

-   [앱 세계화](https://msdn.microsoft.com/library/windows/apps/xaml/hh965328)
-   [언어 일치](https://msdn.microsoft.com/library/windows/apps/xaml/jj673578.aspx)
-   [NumeralSystem 값](https://msdn.microsoft.com/library/windows/apps/xaml/jj236471.aspx)
-   [국가별 글꼴](https://msdn.microsoft.com/library/windows/apps/xaml/dn263115.aspx)
-   [앱 리소스 및 지역화](https://msdn.microsoft.com/library/windows/apps/xaml/hh710212.aspx)

 

 






<!--HONumber=Aug16_HO3-->


