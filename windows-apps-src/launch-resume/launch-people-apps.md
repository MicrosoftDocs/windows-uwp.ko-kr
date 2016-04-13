---
title: 피플 앱 시작
description: 이 항목에서는 ms-people URI 체계에 대해 설명합니다. 앱에서 이 URI 스키마를 사용하여 특정 작업에 대한 피플 앱을 실행할 수 있습니다.
ms.assetid: 1E604599-26EF-421C-932F-E9935CDB248E
---

# 피플 앱 실행


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


이 항목에서는 **ms-people:** URI 스키마를 설명합니다. 앱에서 이 URI 스키마를 사용하여 특정 작업에 대한 피플 앱을 실행할 수 있습니다.

## ms-people: URI 스키마 참조


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
**참고** <p>이 매개 변수는 대/소문자를 구분합니다.</p>
<p>구문을 올바르게 입력하지 않거나 검색 문자열 값이 누락된 경우 기본 동작은 필터링 없이 전체 연락처 목록을 반환하는 것입니다.</p>
</div>
<div>
 
</div></td>
<td align="left">ms-people:search?SearchString=&lt;contactsearchinfo&gt;</td>
</tr>
<tr class="even">
<td align="left">연락처가 있는 경우 기존 대화 상대 카드를 실행합니다. 또는 연락처가 없는 경우 임시 대화 상대 카드를 실행합니다. 입력 매개 변수가 없으면 연락처 목록으로 피플 앱이 실행됩니다.
<div class="alert">
**참고** <p>이 매개 변수는 대/소문자를 구분합니다.</p>
<p>매개 변수의 순서는 중요하지 않습니다.</p>
<p>일치하는 항목이 두 개 이상 있는 경우 연락처의 첫 번째 일치 항목이 반환됩니다.</p>
</div>
<div>
 
</div></td>
<td align="left">ms-people:viewcontact:?ContactId=&lt;contactid&gt;&amp;AggregatedId=&lt;aggid&gt;&amp;PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;&amp;Contact=&lt;contactobj&gt;</td>
</tr>
<tr class="odd">
<td align="left">피플 앱 내의 대화 상대 저장 페이지를 실행하여 제공된 전화 번호 또는 메일 주소와 함께 지정된 연락처를 저장합니다.
<div class="alert">
**참고** <p>이 매개 변수는 대/소문자를 구분합니다.</p>
<p>매개 변수의 순서는 중요하지 않습니다.</p>
</div>
<div>
 
</div></td>
<td align="left">ms-people:savetocontact?PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;</td>
</tr>
</tbody>
</table>

 

## ms-people:search: 매개 변수 참조


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
<td align="left">**SearchString**</td>
<td align="left"><p>옵션.</p>
<p>연락처 검색 정보에 대한 검색 문자열입니다.</p>
<p>전화 번호 또는 연락처 이름입니다.</p></td>
<td align="left"><p>ms-people:search?SearchString=Smith</p></td>
</tr>
</tbody>
</table>

 

## ms-people:viewcontact: 매개 변수 참조


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
<td align="left">**ContactId**</td>
<td align="left"><p>옵션.</p>
<p>연락처의 연락처 ID입니다.</p></td>
<td align="left"><p>ms-people:viewcontact?ContactId={ContactId}</p></td>
</tr>
<tr class="even">
<td align="left">**PhoneNumber**</td>
<td align="left"><p>옵션.</p>
<p>연락처의 전화 번호입니다.</p></td>
<td align="left"><p>ms-people:viewcontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="odd">
<td align="left">**메일**</td>
<td align="left"><p>옵션.</p>
<p>연락처의 메일입니다.</p></td>
<td align="left"><p>ms-people:viewcontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="even">
<td align="left">**ContactName**</td>
<td align="left"><p>옵션.</p>
<p>연락처의 이름입니다.</p></td>
<td align="left"><p>ms-people:viewcontact?ContactName=John%20%Smith</p></td>
</tr>
<tr class="odd">
<td align="left">**Contact**</td>
<td align="left"><p>옵션.</p>
<p>연락처 개체입니다.</p></td>
<td align="left"><p>ms-people:viewcontact?Contact={Serialized Contact}</p></td>
</tr>
</tbody>
</table>

 

## ms-people:savetocontact: 매개 변수 참조


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
<td align="left">**PhoneNumber**</td>
<td align="left"><p>옵션.</p>
<p>연락처의 전화 번호입니다.</p></td>
<td align="left"><p>ms-people:savetocontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="even">
<td align="left">**메일**</td>
<td align="left"><p>옵션.</p>
<p>연락처의 메일입니다.</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="odd">
<td align="left">**ContactName**</td>
<td align="left"><p>옵션.</p>
<p>연락처의 이름입니다.</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com&amp;ContactName= John%20%Smith</p></td>
</tr>
</tbody>
</table>

 

 

 





<!--HONumber=Mar16_HO1-->


