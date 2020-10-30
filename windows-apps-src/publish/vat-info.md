---
description: 파트너 센터 등록 프로세스 중에 VAT ID 번호를 제공 해야 하는 경우 시작 하는 데 도움이 되는 몇 가지 정보는 다음과 같습니다.
title: VAT 정보
ms.assetid: 93834A6B-86D6-4647-AC01-CBCA3CB7A578
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bd863210fad66d9494d9d71115966656ccbb8893
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034926"
---
# <a name="vat-info"></a>VAT 정보


파트너 센터 등록 프로세스 중에 VAT ID 번호를 제공 해야 하는 경우 시작 하는 데 도움이 되는 몇 가지 정보는 다음과 같습니다.

## <a name="understanding-vat-numbers"></a>VAT 번호 이해


값 부가 세금 (VAT) 번호는 유럽 연합의 국가 또는 지역에 사용 되는 식별자입니다. 자세한 내용은 유럽 연합 공식 [Vies 사이트](http://ec.europa.eu/taxation_customs/vies/vieshome.do)를 참조 하세요.

## <a name="accepted-formats-for-vat-numbers"></a>VAT 번호에 허용 되는 형식


Microsoft는 세금 조언을 제공 하지 않으며 다음 표는 참고 자료로만 제공 됩니다. 이 지침이 Microsoft에서 VAT 번호를 제공 하는 데 충분 하지 않은 경우 최근 변경 내용에 대 한 현지 세금 기관을 확인 해야 합니다.

<table Responsive="true">
<tr><th>국가/지역</th><th>VAT 정보</th></tr>
<tr><td data-th="Country/region">오스트리아
</td><td data-th="VAT info">
<ul>
<li>VAT 숫자 형식: 1 자에서 8 자리</li>
<li>국가/지역 코드: AT</li>
<li>예: U12345678</li>
<li>참고: 첫 번째 문자는 항상 ' U '입니다.
</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">벨기에</td><td data-th="VAT info">
<ul>
<li>VAT 숫자 형식: 10 자리</li>
<li>국가/지역 코드: be</li>
<li>예: 1234567890</li>
<li>참고: 1 월 2007 일 이전의 9 자리 숫자입니다.</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">불가리아</td><td data-th="VAT info">
<ul>
<li>VAT 숫자 형식: 9 또는 10 자리</li>
<li>국가/지역 코드: BG</li>
<li>예: 123456789 또는 0123456789</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">크로아티아</td><td data-th="VAT info">
<ul>
<li>VAT 번호 형식: 2 문자 및 11 자리</li>
<li>국가/지역 코드: HR</li>
<li>예: HR12345678901</li>
<li>참고: 첫 문자는 항상 ' HR '입니다.</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">키프로스</td><td data-th="VAT info">
<ul>
<li>VAT 숫자 형식: 2 자에서 8 자리 및 1 자</li>
<li>국가/지역 번호: CY</li>
<li>예: 12345678, 123456789 또는 0123456789</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">체코</td><td data-th="VAT info">
<ul>
<li>VAT 번호 형식: 8, 9 또는 10 자리</li>
<li>국가/지역 코드: CZ</li>
<li>예: 12345678, 123456789 또는 0123456789</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">덴마크</td><td data-th="VAT info">
<ul>
<li>VAT 숫자 형식: 8 자리</li>
<li>국가/지역 코드: 진한</li>
<li>예: 12345678</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">에스토니아</td><td data-th="VAT info">
<ul>
<li>VAT 번호 형식: 9 자리</li>
<li>국가/지역 코드: EE</li>
<li>예: 123456789</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">핀란드</td><td data-th="VAT info">
<ul>
<li>VAT 숫자 형식: 숫자</li>
<li>국가/지역 번호: FI</li>
<li>예: 12345678</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">프랑스</td><td data-th="VAT info">
<ul>
<li>VAT 번호 형식: 11 자리</li>
<li>국가/지역 코드: FR</li>
<li>예: 12345678901, X1234567890, 1X123456789 또는 XX123456789</li>
<li>참고: I 또는 Q를 첫 번째 또는 두 번째 문자로 포함 하거나, 첫 번째 및 두 번째 문자를 제외한 모든 알파벳 문자를 포함 하거나, 그 뒤에 9 자리가 올 수 있습니다.</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">독일</td><td data-th="VAT info">
<ul>
<li>VAT 번호 형식: 9 자리</li>
<li>국가/지역 번호: DE</li>
<li>예: 123456789</li>
<li>참고: 9 자리 ' Umsatzsteur Identifikationnummer ' (Ust ID Nr) 여야 합니다.</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">그리스</td><td data-th="VAT info">
<ul>
<li>VAT 번호 형식: 9 자리</li>
<li>국가/지역 코드: 엘살바도르, GR</li>
<li>예: 123456789</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">헝가리</td><td data-th="VAT info">
<ul>
<li>VAT 숫자 형식: 8 자리</li>
<li>국가/지역 코드: HU-HU</li>
<li>예: 12345678</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">아일랜드</td><td data-th="VAT info">
<ul>
<li>VAT 숫자 형식: 8 자리</li>
<li>국가/지역 코드: IE</li>
<li>예: 1234567X 또는 1X34567X</li>
<li>참고: 1 또는 2 개의 영문자 (last 또는 second와 last)를 포함 합니다.</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">이탈리아</td><td data-th="VAT info">
<ul>
<li>VAT 번호 형식: 11 자리</li>
<li>국가/지역 코드: IT</li>
<li>예: 12345678901</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">라트비아</td><td data-th="VAT info">
<ul>
<li>VAT 번호 형식: 11 자리</li>
<li>국가/지역 코드: LV</li>
<li>예: 01234567890</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">리투아니아</td><td data-th="VAT info">
<ul>
<li>VAT 번호 형식: 9 또는 12 자리</li>
<li>국가/지역 코드: LT</li>
<li>예: 123456789 또는 012345678901</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">룩셈부르크</td><td data-th="VAT info">
<ul>
<li>VAT 숫자 형식: 8 자리</li>
<li>국가/지역 코드: LU</li>
<li>예: 12345678</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">몰타</td><td data-th="VAT info">
<ul>
<li>VAT 숫자 형식: 2 문자 및 8 자리</li>
<li>국가/지역 코드: MT</li>
<li>예: MT12345678</li>
<li>참고: 첫 문자는 항상 ' MT '입니다.</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">네덜란드</td><td data-th="VAT info">
<ul>
<li>VAT 번호 형식: 11 자리 및 1 자</li>
<li>국가/지역 코드: NL</li>
<li>예: 123456789B01</li>
<li>참고: 열 번째 문자는 항상 ' B '입니다.</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">폴란드</td><td data-th="VAT info">
<ul>
<li>VAT 숫자 형식: 10 자리</li>
<li>국가/지역 번호: PL</li>
<li>예: 1234567890</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">포르투갈</td><td data-th="VAT info">
<ul>
<li>VAT 번호 형식: 9 자리</li>
<li>국가/지역 번호: PT</li>
<li>예: 123456789</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">루마니아</td><td data-th="VAT info">
<ul>
<li>VAT 숫자 형식: 2 문자 및 8-10 자리 숫자</li>
<li>국가/지역 코드: RO</li>
<li>예: RO12345678, RO123456789 또는 RO1234567890</li>
<li>참고: 첫 번째 문자는 항상 ' RO '입니다.</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">슬로바키아</td><td data-th="VAT info">
<ul>
<li>VAT 숫자 형식: 10 자리</li>
<li>국가/지역 번호:.</li>
<li>예: 1234567890</li>
<li>참고: 첫 번째 문자는 항상 ' SI '입니다.</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">슬로베니아</td><td data-th="VAT info">
<ul>
<li>VAT 숫자 형식: 2 문자 및 8 자리</li>
<li>국가/지역 코드: SI</li>
<li>예: SI12345678</li>
<li>참고: 첫 번째 문자는 항상 ' SI '입니다.</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">스페인</td><td data-th="VAT info">
<ul>
<li>VAT 번호 형식: 9 자리</li>
<li>국가/지역 코드: ES</li>
<li>예: X12345678, 12345678X 또는 X1234567X</li>
<li>참고: 1 또는 2 문자를 포함 합니다. first, last 또는 first와 last 중 하나를 포함 합니다.</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">스웨덴</td><td data-th="VAT info">
<ul>
<li>VAT 번호 형식: 12 자리</li>
<li>국가/지역 코드: SE</li>
<li>예: 123456789001</li>
<li>참고: 마지막 2 문자는 ' 01 ' 이어야 합니다.</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">영국</td><td data-th="VAT info">
<ul>
<li>VAT 번호 형식: 9 또는 12 자리</li>
<li>국가/지역 번호: GB</li>
<li>예: 123456789 또는 123456789001</li>
<li>참고: 일반적으로 9 자리 숫자 이지만 숫자가 그룹 내의 하위 회사를 나타내는 경우 12 자리가 됩니다.</li>
</ul>
</td></tr>
</table>
                                                                                                                                                                  

 

 




