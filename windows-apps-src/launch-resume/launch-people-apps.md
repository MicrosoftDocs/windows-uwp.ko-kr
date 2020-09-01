---
title: 피플 앱 실행
description: 이 항목에서는 ms 사용자 URI 체계에 대해 설명 합니다. 앱은이 URI 체계를 사용 하 여 특정 작업에 대 한 피플 앱을 시작할 수 있습니다.
ms.assetid: 1E604599-26EF-421C-932F-E9935CDB248E
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dd1048ad0d3c5d542c5d7fb398261f3e29316396
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162617"
---
# <a name="launch-the-people-app"></a>피플 앱 실행

이 항목에서는 **ms 사용자:** URI 체계에 대해 설명 합니다. 앱은이 URI 체계를 사용 하 여 특정 작업에 대 한 피플 앱을 시작할 수 있습니다.

## <a name="ms-people-uri-scheme-reference"></a>ms-사용자: URI 체계 참조

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">결과</th>
<th align="left">URI 체계</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">다른 앱이 사용자 앱 기본 페이지를 시작할 수 있도록 허용 합니다.</td>
<td align="left">ms-사람:</td>
</tr>
<tr class="even">
<td align="left">다른 앱이 피플 앱 설정 페이지를 시작할 수 있도록 허용 합니다.</td>
<td align="left">ms-사람: 설정</td>
</tr>
<tr class="odd">
<td align="left">다른 앱이 검색 결과 페이지에서 사용자 앱을 시작 하는 검색 문자열을 제공할 수 있도록 허용 합니다.
<div class="alert">
<p>매개 변수에는 대/소문자가 구분됩니다.</p>
<p>구문을 올바르게 입력 하지 않거나 검색 문자열 값이 없는 경우 기본 동작은 필터링 없이 전체 연락처 목록을 반환 하는 것입니다.</p>
</div>
<div>
</div></td>
<td align="left">ms-사람: 검색 SearchString = &lt; 연락처 searchstring&gt;</td>
</tr>
<tr class="even">
<td align="left">연락처가 있는 경우 기존 연락처 카드를 시작 합니다. 또는 연락처가 없는 경우 임시 연락처 카드를 시작 합니다. 입력 매개 변수를 제공 하지 않으면 연락처 목록이 포함 된 피플 앱이 시작 됩니다.
<div class="alert">
<p>매개 변수에는 대/소문자가 구분됩니다.</p>
<p>매개 변수의 순서는 중요 하지 않습니다.</p>
<p>일치 하는 항목이 두 개 이상 있는 경우 연락처의 첫 번째 일치 항목을 반환 합니다.</p>
</div>
<div> 
</div></td>
<td align="left">ms-사람: viewcontact? ContactId = &lt; ContactId &gt; &amp; AggregatedId = &lt; aggid &gt; &amp; PhoneNumber = &lt; phonenum &gt; &amp; email = &lt; email &gt; &amp; ContactName = &lt; name &gt; &amp; Contact = 연락처 &lt; obj&gt;</td>
</tr>
<tr class="odd">
<td align="left">지정 된 연락처를 제공 된 전화 번호 또는 전자 메일 주소와 함께 저장 하려면 피플 앱 내의 연락처 저장 페이지를 시작 합니다.
<div class="alert">
<p>매개 변수에는 대/소문자가 구분됩니다.</p>
<p>매개 변수의 순서는 중요 하지 않습니다.</p>
</div>
<div>
</div></td>
<td align="left">ms-사람: savetocontact? PhoneNumber = &lt; phonenum &gt; &amp; email = &lt; email &gt; &amp; ContactName = &lt; name&gt;</td>
</tr>
<tr class="even">
<td align="left">사용자 앱 내에서 새 연락처 추가 페이지를 시작 하 여 지정 된 연락처를 저장 합니다.
<div class="alert"><p><a href="/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriForResultsAsync_Windows_Foundation_Uri_Windows_System_LauncherOptions_Windows_Foundation_Collections_ValueSet_">LaunchUriForResultsAsync</a> 를 사용 하 여 새 연락처 저장 페이지를 엽니다. <strong>LaunchUriAsync</strong> 를 사용 하면 사용자 앱 기본 페이지만 시작 됩니다.</p>
<p>매개 변수에는 대/소문자가 구분됩니다.</p>
<p>매개 변수의 순서는 중요 하지 않습니다.</p>
<p>지원 되는 매개 변수를 임의로 조합 하 여 사용할 수 있습니다.</p>
</div>
<div>
</div></td>
<td align="left">ms-사람: savecontacttask? PhoneNumber = &lt; phonenum &gt; &amp; email = &lt; email &gt; &amp; ContactName = &lt; name&gt;</td>
</tr>
</tbody>
</table>

## <a name="ms-peoplesearch-parameter-reference"></a>ms-사람: 검색: 매개 변수 참조

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">매개 변수</th>
<th align="left">Description</th>
<th align="left">예제</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>SearchString</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처 검색 정보에 대 한 검색 문자열입니다.</p>
<p>전화 번호 또는 연락처 이름입니다.</p></td>
<td align="left"><p>ms-사람: 검색 SearchString = Smith</p></td>
</tr>
</tbody>
</table>

## <a name="ms-peopleviewcontact-parameter-reference"></a>ms-사람: viewcontact: 매개 변수 참조

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">매개 변수</th>
<th align="left">Description</th>
<th align="left">예제</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>ContactId</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 연락처 Id입니다.</p></td>
<td align="left"><p>ms-사람: viewcontact? ContactId = {ContactId}</p></td>
</tr>
<tr class="even">
<td align="left"><b>PhoneNumber</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 전화 번호입니다.</p></td>
<td align="left"><p>ms-사람: viewcontact? PhoneNumber = %2014257069326</p></td>
</tr>
<tr class="odd">
<td align="left"><b>Email</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 전자 메일입니다.</p></td>
<td align="left"><p>ms-사람: viewcontact? 전자 메일 =johnsmith@contsco.com</p></td>
</tr>
<tr class="even">
<td align="left"><b>ContactName</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 이름입니다.</p></td>
<td align="left"><p>ms-사람: viewcontact? ContactName = John %20% Smith</p></td>
</tr>
<tr class="odd">
<td align="left"><b>Contact</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>Contact 개체입니다.</p></td>
<td align="left"><p>ms-사람: viewcontact? Contact = {Serialize 된 연락처}</p></td>
</tr>
</tbody>
</table>

## <a name="ms-peoplesavetocontact-parameter-reference"></a>ms-사용자: savetocontact: 매개 변수 참조

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">매개 변수</th>
<th align="left">Description</th>
<th align="left">예제</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>PhoneNumber</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 전화 번호입니다.</p></td>
<td align="left"><p>ms-사람: savetocontact? PhoneNumber = %2014257069326</p></td>
</tr>
<tr class="even">
<td align="left"><b>Email</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 전자 메일입니다.</p></td>
<td align="left"><p>ms-사람: savetocontact? 전자 메일 =johnsmith@contsco.com</p></td>
</tr>
<tr class="odd">
<td align="left"><b>ContactName</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 이름입니다.</p></td>
<td align="left"><p>ms-사람: savetocontact? 전자 메일 = johnsmith@contsco.com &amp; ContactName = John %20% Smith</p></td>
</tr>
</tbody>
</table>

## <a name="ms-peoplesavecontacttask-parameter-reference"></a>ms-사용자: savecontacttask: 매개 변수 참조

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">매개 변수</th>
<th align="left">Description</th>

</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>회사</b></td>
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
<p>집 주소의 상태입니다.</p></td>

</tr>
<tr class="even">
<td align="left"><b>HomeAddressStreet</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>집 주소의 주소입니다.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>HomeAddressZipCode</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>집 주소의 우편 번호입니다.</p></td>

</tr>
<tr class="even">
<td align="left"><b>HomePhone</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 집 전화입니다.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>JobTitle</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 직함입니다.</p></td>
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
<td align="left"><b>애칭</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 애칭입니다.</p></td>
</tr>

<tr class="even">
<td align="left"><b>참고</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처에 대 한 메모입니다.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>OtherEmail</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 기타 전자 메일입니다.</p></td>
</tr>

<tr class="even">
<td align="left"><b>PersonalEmail</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 개인 전자 메일입니다.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>접미사</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 접미사입니다.</p></td>
</tr>

<tr class="even">
<td align="left"><b>제목</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 제목입니다.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>웹 사이트</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 웹 사이트입니다.</p></td>
</tr>

<tr class="even">
<td align="left"><b>업무 Addressc티</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>회사 주소의 구/군/시입니다.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>회사 Addresscountry</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>회사 주소의 국가입니다.</p></td>
</tr>

<tr class="even">
<td align="left"><b>근무 주소 상태</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>작업 주소의 상태입니다.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>근무 주소</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>작업 주소의 주소입니다.</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkAddressZipCode</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>회사 주소의 우편 번호입니다.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>회사 전자 메일</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 회사 전자 메일입니다.</p></td>
</tr>

<tr class="even">
<td align="left"><b>회사 전화</b></td>
<td align="left"><p>선택 사항입니다.</p>
<p>연락처의 회사 전화 번호입니다.</p></td>
</tr>
</tbody>
</table>