---
author: TylerMSFT
title: 피플 앱 시작
description: 이 항목에서는 ms-people URI 체계에 대해 설명합니다. 앱에서 이 URI 스키마를 사용하여 특정 작업에 대한 피플 앱을 실행할 수 있습니다.
ms.assetid: 1E604599-26EF-421C-932F-E9935CDB248E
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0b87a49f24035215d44dbabcf9e401ddfefdff47
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5989961"
---
# <a name="launch-the-people-app"></a>피플 앱 실행

이 항목에서는 **ms-people:** URI 스키마를 설명합니다. 앱에서 이 URI 스키마를 사용하여 특정 작업에 대한 피플 앱을 실행할 수 있습니다.

## <a name="ms-people-uri-scheme-reference"></a>ms-people: URI 스키마 참조

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">결과</th>
<th align="left">URI 스키마</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">다른 앱에서 피플 앱 기본 페이지를 실행하도록 허용합니다.</td>
<td align="left">ms-people:</td>
</tr>
<tr class="even">
<td align="left">다른 앱에서 피플 앱 설정 페이지를 실행하도록 허용합니다.</td>
<td align="left">ms-people:settings</td>
</tr>
<tr class="odd">
<td align="left">다른 앱에서 검색 결과 페이지를 통해 피플 앱을 실행하는 검색 문자열을 제공하도록 허용합니다.
<div class="alert">
<p>이 매개 변수는 대/소문자를 구분합니다.</p>
<p>구문을 올바르게 입력하지 않거나 검색 문자열 값이 누락된 경우 기본 동작은 필터링 없이 전체 연락처 목록을 반환하는 것입니다.</p>
</div>
<div>
</div></td>
<td align="left">ms-people:search?SearchString=&lt;contactsearchinfo&gt;</td>
</tr>
<tr class="even">
<td align="left">연락처가 있는 경우 기존 대화 상대 카드를 실행합니다. 또는 연락처가 없는 경우 임시 대화 상대 카드를 실행합니다. 입력 매개 변수가 없으면 연락처 목록으로 피플 앱이 실행됩니다.
<div class="alert">
<p>이 매개 변수는 대/소문자를 구분합니다.</p>
<p>매개 변수의 순서는 중요하지 않습니다.</p>
<p>일치하는 항목이 두 개 이상 있는 경우 연락처의 첫 번째 일치 항목이 반환됩니다.</p>
</div>
<div> 
</div></td>
<td align="left">ms-people:viewcontact:?ContactId=&lt;contactid&gt;&amp;AggregatedId=&lt;aggid&gt;&amp;PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;&amp;Contact=&lt;contactobj&gt;</td>
</tr>
<tr class="odd">
<td align="left">피플 앱 내의 대화 상대 저장 페이지를 실행하여 제공된 전화 번호 또는 이메일 주소와 함께 지정된 연락처를 저장합니다.
<div class="alert">
<p>이 매개 변수는 대/소문자를 구분합니다.</p>
<p>매개 변수의 순서는 중요하지 않습니다.</p>
</div>
<div>
</div></td>
<td align="left">ms-people:savetocontact?PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;</td>
</tr>
<tr class="even">
<td align="left">피플 앱 내에서 새 연락처 추가 페이지를 실행하여 지정된 연락처를 저장합니다.
<div class="alert"><p><a href="https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriForResultsAsync_Windows_Foundation_Uri_Windows_System_LauncherOptions_Windows_Foundation_Collections_ValueSet_">LaunchUriForResultsAsync</a>를 사용하여 새 연락처 저장 페이지를 엽니다. <strong>LaunchUriAsync</strong>를 사용하면 피플 앱 기본 페이지만 실행됩니다.</p>
<p>이 매개 변수는 대/소문자를 구분합니다.</p>
<p>매개 변수의 순서는 중요하지 않습니다.</p>
<p>지원되는 매개 변수를 원하는 대로 조합하여 사용할 수 있습니다.</p>
</div>
<div>
</div></td>
<td align="left">ms-people:savecontacttask?PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;</td>
</tr>
</tbody>
</table>

## <a name="ms-peoplesearch-parameter-reference"></a>ms-people:search: 매개 변수 참조

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">매개 변수</th>
<th align="left">설명</th>
<th align="left">예제</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>SearchString</b></td>
<td align="left"><p>선택 사항.</p>
<p>연락처 검색 정보에 대한 검색 문자열입니다.</p>
<p>전화 번호 또는 연락처 이름입니다.</p></td>
<td align="left"><p>ms-people:search?SearchString=Smith</p></td>
</tr>
</tbody>
</table>

## <a name="ms-peopleviewcontact-parameter-reference"></a>ms-people:viewcontact: 매개 변수 참조

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">매개 변수</th>
<th align="left">설명</th>
<th align="left">예제</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>ContactId</b></td>
<td align="left"><p>선택 사항.</p>
<p>연락처의 연락처 ID입니다.</p></td>
<td align="left"><p>ms-people:viewcontact?ContactId={ContactId}</p></td>
</tr>
<tr class="even">
<td align="left"><b>PhoneNumber</b></td>
<td align="left"><p>선택 사항.</p>
<p>연락처의 전화 번호입니다.</p></td>
<td align="left"><p>ms-people:viewcontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="odd">
<td align="left"><b>메일</b></td>
<td align="left"><p>선택 사항.</p>
<p>연락처의 메일입니다.</p></td>
<td align="left"><p>ms-people:viewcontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="even">
<td align="left"><b>ContactName</b></td>
<td align="left"><p>선택 사항.</p>
<p>연락처의 이름입니다.</p></td>
<td align="left"><p>ms-people:viewcontact?ContactName=John%20%Smith</p></td>
</tr>
<tr class="odd">
<td align="left"><b>Contact</b></td>
<td align="left"><p>선택 사항.</p>
<p>연락처 개체입니다.</p></td>
<td align="left"><p>ms-people:viewcontact?Contact={Serialized Contact}</p></td>
</tr>
</tbody>
</table>

## <a name="ms-peoplesavetocontact-parameter-reference"></a>ms-people:savetocontact: 매개 변수 참조

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">매개 변수</th>
<th align="left">설명</th>
<th align="left">예제</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>PhoneNumber</b></td>
<td align="left"><p>선택 사항.</p>
<p>연락처의 전화 번호입니다.</p></td>
<td align="left"><p>ms-people:savetocontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="even">
<td align="left"><b>메일</b></td>
<td align="left"><p>선택 사항.</p>
<p>연락처의 메일입니다.</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="odd">
<td align="left"><b>ContactName</b></td>
<td align="left"><p>선택 사항.</p>
<p>연락처의 이름입니다.</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com&amp;ContactName= John%20%Smith</p></td>
</tr>
</tbody>
</table>

## <a name="ms-peoplesavecontacttask-parameter-reference"></a>ms-people:savecontacttask: 매개 변수 참조

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">매개 변수</th>
<th align="left">설명</th>

</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>Company</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 회사 이름입니다.</p></td>

</tr>
<tr class="even">
<td align="left"><b>FirstName</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 이름입니다.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>HomeAddressCity</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>집 주소의 구/군/시입니다.</p></td>

</tr>
<tr class="even">
<td align="left"><b>HomeAddressCountry</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>집 주소의 국가입니다.</p></td>

</tr>
<tr class="odd">
<td align="left"><b>HomeAddressState</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>집 주소의 시/도입니다.</p></td>

</tr>
<tr class="even">
<td align="left"><b>HomeAddressStreet</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>집 주소의 상세 주소입니다.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>HomeAddressZipCode</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>집 주소의 우편 번호입니다.</p></td>

</tr>
<tr class="even">
<td align="left"><b>HomePhone</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 집 전화 번호입니다.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>JobTitle</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 직위입니다.</p></td>
</tr>

<tr class="even">
<td align="left"><b>LastName</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 성입니다.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>MiddleName</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 중간 이름입니다.</p></td>
</tr>

<tr class="even">
<td align="left"><b>MobilePhone</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 휴대폰 번호입니다.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Nickname</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 별명입니다.</p></td>
</tr>

<tr class="even">
<td align="left"><b>Notes</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처에 대한 참고 사항입니다.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>OtherEmail</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 기타 전자 메일입니다.</p></td>
</tr>

<tr class="even">
<td align="left"><b>PersonalEmail</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 개인 저자 메일입니다.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Suffix</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 호칭입니다.</p></td>
</tr>

<tr class="even">
<td align="left"><b>Title</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 직함입니다.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Website</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 웹 사이트입니다.</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkAddressCity</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>회사 주소의 구/군/시입니다.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>WorkAddressCountry</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>회사 주소의 국가입니다.</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkAddressState</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>회사 주소의 시/도입니다.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>WorkAddressStreet</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>회사 주소의 상세 주소입니다.</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkAddressZipCode</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>회사 주소의 우편 번호입니다.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>WorkEmail</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 회사 전자 메일입니다.</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkPhone</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 회사 전화 번호입니다.</p></td>
</tr>
</tbody>
</table>
