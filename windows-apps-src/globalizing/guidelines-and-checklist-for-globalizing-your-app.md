---
Description: 앱을 광범위한 대상에 대해 세계화하거나 특정 시장을 위해 지역화하는 모범 사례를 따르세요.
Search.Refinement.TopicID: 180
title: 세계화 및 지역화에 대한 지침
ms.assetid: 0342DC3F-DDD1-4DD4-872E-A4EC340CAE79
label: Do's and don'ts
template: detail.hbs
---

# 세계화 및 지역화 권장 사항 및 금지 사항


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**세계화**](https://msdn.microsoft.com/library/windows/apps/br206813)
-   [**Globalization.NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136)
-   [**Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
-   [**리소스**](https://msdn.microsoft.com/library/windows/apps/br206022)
-   [**Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039)

앱을 광범위한 대상에 대해 세계화하거나 특정 시장을 위해 지역화하는 모범 사례를 따르세요.



## <span id="guidelines_for_internationalization"> </span> <span id="GUIDELINES_FOR_INTERNATIONALIZATION"> </span>세계화

UI에 적합한 포괄적 조건과 이미지를 선택하고, [**Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) API를 사용하여 앱 데이터의 서식을 지정하고, 위치 또는 언어를 기반으로 가정하는 것을 피하여 앱을 시장에 따라 쉽게 조정할 수 있도록 준비하세요.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">권장 사항</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>올바른 숫자, 날짜, 시간, 주소 및 전화 번호 형식을 사용하세요.</p></td>
<td align="left"><p>숫자, 날짜, 시간 및 기타 데이터 양식에 사용되는 형식은 문화, 지역, 언어 및 시장에 따라 다릅니다. 숫자, 날짜, 시간 또는 기타 데이터를 표시하는 경우 [<strong>Globalization</strong>](https://msdn.microsoft.com/library/windows/apps/br206813) API를 사용하여 특정 대상에게 적합한 형식을 가져옵니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>국제 용지 크기를 지원합니다.</p></td>
<td align="left"><p>가장 일반적인 용지 크기는 국가마다 다르므로, 인쇄와 같이 용지 크기에 따라 달라지는 기능이 포함되어 있는 경우 일반적인 국가별 크기를 지원하는지 테스트하여 확인해야 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>국가별 측정 단위 및 통화를 지원합니다.</p></td>
<td align="left"><p>사용되는 단위와 배율은 국가마다 다르지만, 미터법과 영국식 시스템이 가장 일반적으로 사용됩니다. 길이, 온도, 면적 등을 측정할 경우 [<strong>CurrenciesInUse</strong>](https://msdn.microsoft.com/library/windows/apps/br206793) 속성을 사용하여 올바른 시스템 측정치를 가져옵니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>텍스트와 글꼴을 올바르게 표시합니다.</p></td>
<td align="left"><p>적합한 글꼴, 글꼴 크기 및 텍스트 방향은 시장마다 다릅니다.</p>
<p>자세한 내용은 [<strong>레이아웃 및 글꼴 조정, RTL 지원</strong>](adjust-layout-and-fonts--and-support-rtl.md)을 참조하세요.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>문자 인코딩에 대해 유니코드를 사용합니다.</p></td>
<td align="left"><p>기본적으로 최신 버전의 Microsoft Visual Studio에서는 모든 문서에 대해 유니코드 문자 인코딩을 사용합니다. 다른 편집기를 사용하는 경우 원본 파일을 적절한 유니코드 문자 인코딩으로 저장해야 합니다. 모든 Windows 런타임 API는 UTF-16 인코딩 문자열을 반환합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>입력 언어를 기록합니다.</p></td>
<td align="left"><p>개발자 앱에서 사용자가 텍스트를 입력해야 하는 경우 입력 언어를 기록합니다. 그러면 나중에 입력 내용을 사용자에게 표시할 때 적절한 서식으로 표시됩니다. [<strong>CurrentInputMethodLanguage</strong>](https://msdn.microsoft.com/library/windows/apps/hh700658) 속성을 사용하여 현재 입력 언어를 가져옵니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>언어로 사용자의 위치를 가정하거나 위치로 사용자의 언어를 가정하지 마세요.</p></td>
<td align="left"><p>Windows에서 사용자의 언어와 위치는 별개의 개념입니다. 사용자가 특정 지역의 변형된 언어(예: 영어[영국] en-gb)로 말하지만, 완전히 다른 국가 또는 지역에 있을 수 있습니다. 앱에 사용자의 언어(예: UI 텍스트용) 또는 위치(예: 라이선스 발급용) 정보가 필요한지 여부를 고려하세요.</p>
<p>자세한 내용은 [<strong>언어 및 지역 관리</strong>](manage-language-and-region.md)를 참조하세요.</p></td>
</tr>
<tr class="even">
<td align="left"><p>구어체와 비유를 사용하지 마세요.</p></td>
<td align="left"><p>문화와 연령 등 특정 인구 그룹에서 사용하는 언어는 해당 그룹의 사람들만 사용하므로 이해하거나 번역하기가 어렵습니다. 마찬가지로 은유는 사람마다 다르게 받아들일 수 있습니다. 예를 들어 &quot;bluebird&quot;는 스키 문화에 속하는 사람들에게는 특별한 의미가 있지만, 그렇지 않은 사람들은 이해하지 못합니다. 앱을 지역화하고 구어체를 사용하는 경우 지역화 작업자에게 의미와 표현 방식을 적절히 설명해야 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>기술적 전문 용어, 약어 또는 약자는 사용하지 않습니다.</p></td>
<td align="left"><p>기술적인 언어는 기술 지식이 부족한 사용자나 다른 문화권이나 지역의 사람들이 이해하기 어렵고 번역하기도 어렵습니다. 사람들은 일상 대화에서 그러한 단어를 사용하지 않습니다. 하드웨어와 소프트웨어 문제를 식별하기 위해 오류 메시지에 종종 기술적인 언어가 사용됩니다. 더러 이렇게 할 필요가 있는 경우도 있지만 기술적이지 않은 문자열로 다시 작성해야 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>불쾌감을 줄 수 있는 이미지를 사용하지 마세요.</p></td>
<td align="left"><p>이미지가 개발자의 문화에는 적절하더라도 다른 문화에서는 불쾌하거나 오해를 일으킬 수 있습니다. 국기 또는 정치적 운동과 연관된 종교 기호, 동물 또는 색상 조합을 사용하지 마세요.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>맵에서나 종교를 말할 때 정치적으로 불쾌할 수 있는 단어를 피하세요.</p></td>
<td align="left"><p>맵은 분쟁 지역이나 국경을 포함할 수 있으므로 정치적으로 불쾌함을 주기가 쉽습니다. 따라서 나라를 선택하는 데 사용되는 모든 UI에서 나라를 &quot국가/지역&quot;으로 나타내야 합니다. 주소 양식 등에서 분쟁 지역을 &quot;국가&quot; 목록에 넣을 경우 문제가 발생할 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>직접적인 문자열 비교를 사용하여 언어 태그를 비교하지 마세요.</p></td>
<td align="left"><p>BCP-47 언어 태그는 복잡합니다. 언어 태그를 비교할 경우 많은 문제(예: 스크립트 정보, 레거시 태그 및 여러 지역별 변형의 일치 문제)가 있습니다. Windows의 리소스 관리 시스템에서 일치 작업을 자동으로 처리합니다. 리소스 집합을 원하는 언어로 지정할 수 있습니다. 그러면 시스템에서 사용자와 앱에 적절한 언어를 자동으로 선택합니다.</p>
<p>리소스 관리에 대한 자세한 내용은 [<strong>앱 리소스 정의</strong>](https://msdn.microsoft.com/library/windows/apps/xaml/hh965321)를 참조하세요.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>항상 알파벳 순서로 정렬된다고 생각하지 마세요.</p></td>
<td align="left"><p>라틴어 스크립트를 사용하지 않는 언어의 경우 발음, 펜 스트로크 수 및 기타 요소를 기반으로 정렬됩니다. 라틴어 스크립트를 사용하는 언어도 항상 알파벳 순으로 정렬되는 것은 아닙니다. 예를 들어 일부 국가의 전화 번호부는 알파벳 순으로 정렬되지 않습니다. 시스템에서 자동으로 정렬할 수 있지만, 개발자가 정렬 알고리즘을 직접 만들 경우 대상 시장에서 사용되는 정렬 방법을 고려해야 합니다.</p></td>
</tr>
</tbody>
</table>

 

## <span id="guidelines_for_localization"> </span> <span id="GUIDELINES_FOR_LOCALIZATION"> </span>지역화

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">권장 사항</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>코드에서 리소스(예: UI 문자열 및 이미지)를 분리합니다.</p></td>
<td align="left"><p>리소스(예: 문자열 및 이미지)가 코드와 분리되도록 앱을 디자인하세요. 그러면 서로 다른 배율 요소, 접근성 옵션, 무수한 사용자 및 기기 환경에 맞게 문자열과 이미지를 독립적으로 유지 관리하고 지역화하고 사용자 지정할 수 있습니다.</p>
<p>문자열 리소스를 앱의 코드와 분리하여 단일의 언어 독립 코드베이스를 만드세요. 앱 코드와 태그에서 문자열을 항상 구분하고 ResW 또는 ResJSON 파일과 같은 리소스 파일에 배치합니다.</p>
<p>Windows의 리소스 인프라를 사용하여 사용자의 런타임 환경과 일치하도록 선별된, 가장 적절한 리소스를 처리하세요.</p></td>
</tr>
<tr class="even">
<td align="left"><p>다른 지역화 가능한 리소스 파일을 분리합니다.</p></td>
<td align="left"><p>지역화할 다른 파일(예: 번역해야 하거나 문화적 중요성으로 인해 변경해야 하는 텍스트가 포함된 이미지)을 가져와서 언어 이름으로 태그 지정된 폴더에 넣습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>기본 언어를 설정하고 기본 언어로 된 리소스를 포함하여 모든 리소스를 표시합니다.</p></td>
<td align="left"><p>항상 앱 매니페스트(package.appxmanifest)에서 앱에 대한 기본 언어를 적절하게 설정합니다. 기본 언어에 따라 사용자가 앱에서 지원되는 언어를 사용하지 않을 때 사용되는 언어가 결정됩니다. 시스템에서 리소스의 언어와 특정 상황에서 리소스가 사용되는 방법을 알 수 있도록 기본 언어 리소스(예: en-us/Logo.png)를 해당 언어로 표시합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>지역화할 앱의 리소스를 결정합니다.</p></td>
<td align="left"><p>앱을 다른 시장용으로 지역화할 경우 무엇을 변경해야 합니까? 텍스트 문자열을 다른 언어로 번역해야 합니다. 이미지를 다른 문화에 맞게 조정해야 합니다. 지역화가 앱에서 사용되는 다른 리소스(예: 오디오, 비디오)에 미치는 영향을 고려합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>코드 및 태그에서 리소스 식별자를 사용하여 리소스를 참조합니다.</p></td>
<td align="left"><p>태그의 이미지에 대해 문자열 리터럴이나 특정 파일 이름을 사용하는 대신 리소스에 대한 참조를 사용하세요. 각 리소스에 대해 고유한 식별자를 사용해야 합니다. 자세한 내용은 [<strong>한정자를 사용하여 리소스 이름을 지정하는 방법</strong>](https://msdn.microsoft.com/library/windows/apps/xaml/Hh965324)을 참조하세요.</p>
<p>시스템이 변경되어 다른 한정자 집합을 사용하기 시작할 때 발생하는 이벤트를 수신합니다. 올바른 리소스를 로드할 수 있도록 문서를 다시 처리합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>텍스트 크기를 확대할 수 있도록 허용합니다.</p></td>
<td align="left"><p>번역할 때 텍스트 크기가 확대될 수 있으므로 텍스트 버퍼를 동적으로 할당합니다. 정적 버퍼를 사용해야 하는 경우 버퍼를 매우 크게(영어 문자열 길이의 2배 정도) 설정하여 문자열을 번역할 때 확대되는 크기를 수용할 수 있도록 합니다. 사용자 인터페이스에 대해 사용 가능한 공간을 제한할 수도 있습니다. 지역화된 언어를 수용하려면 문자열 길이를 영어보다 약 40% 정도 더 길게 설정해야 합니다. 짧은 문자열(예: 한 단어)의 경우 300% 더 넓은 공간이 필요할 수 있습니다. 또한 컨트롤에서 여러 줄 지원과 텍스트 줄바꿈을 사용하면 각 문자열을 표시하기 위한 더 많은 공간을 제공할 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>미러링을 지원합니다.</p></td>
<td align="left"><p>텍스트 정렬 및 읽기 순서는 영어의 경우 왼쪽에서 오른쪽으로 아랍어 또는 히브리어의 경우 RTL(오른쪽에서 왼쪽으로) 방식이 사용됩니다. 제품을 다른 읽기 순서를 사용하는 언어로 지역화할 경우 UI 요소의 레이아웃이 미러링을 지원해야 합니다. 항목(예: 뒤로 단추), UI 전환 효과 및 이미지를 미러링해야 할 수도 있습니다.</p>
<p>자세한 내용은 [<strong>레이아웃 및 글꼴 조정, RTL 지원</strong>](adjust-layout-and-fonts--and-support-rtl.md)을 참조하세요.</p></td>
</tr>
<tr class="even">
<td align="left"><p>문자열을 설명합니다.</p></td>
<td align="left"><p>문자열이 올바르게 설명되어 있는지 확인하고 번역해야 할 문자열만 번역사에게 제공하도록 하세요. 지나친 지역화는 문제의 일반적인 원인이 됩니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>짧은 문자열을 사용합니다.</p></td>
<td align="left"><p>문자열이 짧을수록 번역하거나 번역을 재활용하기 쉽습니다. 번역을 재활용하면 동일한 문자열이 번역가에게 두 번 전송되지 않으므로 비용이 절감됩니다.</p>
<p>일부 지역화 도구에서는 8192자를 초과하는 문자열이 지원되지 않을 수 있으므로, 문자열 길이를 4000자 이하로 유지하세요.</p></td>
</tr>
<tr class="even">
<td align="left"><p>전체 문장을 포함하는 문자열을 제공합니다.</p></td>
<td align="left"><p>단어가 문장의 위치에 따라 다르게 번역될 수 있으므로 문장을 개별 단어로 나누지 않고 전체 문장을 포함하는 문자열을 제공합니다. 구문에 매개 변수가 여러 개 있는 경우 모든 언어에서 매개 변수가 동일한 순서로 유지된다고 생각하지 마세요.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>지역화에 맞게 이미지 및 오디오 파일을 최적화합니다.</p></td>
<td align="left"><p>이미지에서 텍스트를 사용하거나 오디오 파일에서 말하기를 방지하여 지역화 비용을 줄입니다. 읽는 방향이 다른 언어로 지역화할 경우 대칭 이미지 및 효과를 사용하여 미러링을 쉽게 지원할 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>문자열을 다른 컨텍스트에서 다시 사용하지 마세요.</p></td>
<td align="left"><p>아무리 간단한 단어(예: &quot;on&quot; 및 &quot;off&quot;)라도 컨텍스트에 따라 다르게 번역될 수 있으므로 문자열을 다른 컨텍스트에서 다시 사용하지 마세요.</p></td>
</tr>
</tbody>
</table>

 

## <span id="related_topics"> </span>관련 문서


**샘플**
* [응용 프로그램 리소스 및 지역화 샘플(영문)](http://go.microsoft.com/fwlink/p/?linkid=254478)
* [세계화 기본 설정 샘플](http://go.microsoft.com/fwlink/p/?linkid=231608)
 

 





<!--HONumber=Mar16_HO4-->


