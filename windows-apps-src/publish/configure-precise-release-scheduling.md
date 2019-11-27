---
Description: 스토어에서 앱을 사용할 수 있는 정확한 날짜와 시간을 설정할 수 있습니다. 지역/국가 별로 날짜를 사용자 지정할 수 있습니다.
title: 정확한 릴리스 일정 구성
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, 일정, 발매 날짜, 날짜, 실행
ms.localizationpriority: medium
ms.openlocfilehash: ec9ee00aaa350fc48185cc6328674ac2d8f62ea5
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690360"
---
# <a name="configure-precise-release-scheduling"></a>정확한 릴리스 일정 구성

**가격 책정 및 가용성** 페이지의 [일정](set-app-pricing-and-availability.md) 섹션에서 Microsoft Store에서 앱을 사용할 수 있는 정확한 날짜와 시간을 설정할 수 있기 때문에 유연성이 뛰어나며 시장별로 날짜를 사용자 지정할 수 있습니다.

> [!NOTE]
> 이 항목은 앱을 가리키지만, 추가 기능 제출을 위한 릴리스 일정은 동일한 프로세스를 사용합니다.

Microsoft Store에서 앱을 더 이상 사용할 수 없게 만드는 날짜도 추가로 설정할 수 있습니다. Microsoft Store에서 검색이나 탐색으로 제품을 찾을 수 없지만, 직접 링크가 있는 고객은 제품의 Microsoft Store 목록을 볼 수 있습니다. 그러나 제품을 이미 소유하고 있거나, [홍보 코드](generate-promotional-codes.md)를 갖고 있으며, Windows 10 장치를 사용하고 있는 경우에만 다운로드 할 수 있습니다.

기본적으로 (**표시 여부** 섹션에서 [앱을 사용할 수 있지만, Microsoft Store에서 검색할 수 없도록 설정](choose-visibility-options.md#discoverability) 옵션을 선택한 경우를 제외하고), 앱 인증과 게시 프로세스가 완료되는 즉시 고객이 앱을 사용할 수 있습니다. 다른 날짜를 선택 하려면 **옵션 표시**를 선택해 이 섹션을 확장하세요.

**표시 여부** 섹션에서 **앱을 사용할 수 있지만 Microsoft Store에서 검색할 수 없도록 설정** 옵션을 선택했다면 [일정](choose-visibility-options.md#discoverability)에서 날짜를 구성할 수 없습니다. 앱이 고객에게 릴리스되지 않아 구성할 릴리스 날짜가 없기 때문입니다.

> [!IMPORTANT]
> 일정 섹션에서 지정한 날짜는 Windows 10 고객에게만 적용됩니다.
>
>이전에 게시 된 앱에서 이전 OS 버전을 지 원하는 경우 선택한 **중지 취득** 날짜는 해당 고객에 게 적용 되지 않습니다. [표시 유형](choose-visibility-options.md#discoverability) 섹션에서 새 선택 항목을 사용 하 여 업데이트를 제출 하지 않거나 앱 **개요** 페이지에서 **앱을 사용할 수 없음** 을 선택 하는 경우에도 앱을 가져올 수 있습니다.


## <a name="base-schedule"></a>기본 일정

기본 일정에서 선택한 사항은 나중에 [특정 지역/국가 대해 사용자 지정](#customize-the-schedule-for-specific-markets)을 선택해 특정 지역/국가(또는 지역/국가 그룹)을 대상으로 날짜를 추가한 경우를 제외하고, 앱을 사용할 수 있는 모든 지역/국가에 적용됩니다.

**릴리스** 및 **취득 중지** 두 가지 옵션이 표시됩니다. 

## <a name="release"></a>릴리스

**릴리스** 드롭다운을 이용, Microsoft Store에서 앱을 사용할 수 있는 때를 설정할 수 있습니다. Microsoft Store에서 검색이나 탐색으로 앱을 찾을 수 있고, 고객이 Microsoft Store 목록을 확인해 앱을 취득할 수 있습니다.

>[!NOTE]
> 앱이 Microsoft Store에 게시되어 사용할 수 있는 상태가 된 후에는 **릴리스** 날짜를 선택할 수 없습니다(앱을 이미 출시했기 때문).

다음은 제품 **릴리스** 일정에서 구성할 수 있는 옵션들입니다.
- **가능한 한 빨리**: 제품 인증과 게시가 완료된 즉시 릴리스됩니다. 이 옵션이 기본 옵션입니다.
- **지정(at)** : 제품을 선택한 날짜와 시간에 릴리스합니다. 두 가지 추가 옵션이 있습니다.
   - **UTC**: 선택한 시간이 UTC(Universal Coordinated Time) 시간이기 때문에 모든 지역/국가에서 동일한 시간에 앱이 릴리스 됩니다.
   - **로컬**: 선택한 시간을 지역/국가의 시간대에 사용합니다. (시간대가 하나 이상인 지역/국가의 경우, 하나의 시간대만 사용합니다. 미국의 경우 동부 표준 시간대가 사용 됩니다. 표준 시간대의 포괄적인 목록은이 페이지 아래쪽에 표시 됩니다.
- **예약하지 않음**: Microsoft Store에서 앱을 사용할 수 없습니다. 이 옵션을 선택하는 경우, 새로 제출하고 다른 옵션 중 하나를 선택해 나중에 Microsoft Store에서 앱을 사용할 수 있도록 설정할 수 있습니다.


## <a name="stop-acquisition"></a>취득 중지

**취득 중지** 드롭다운을 이용, 새 고객이 Microsoft Store에서 앱을 취득하지 못하고, 발견하지 못하도록 만드는 날짜와 시간을 설정할 수 있습니다. 하나 이상의 앱을 사용하는 것을 조정해야 할 때 등, 새 고객에게 더 이상 앱을 제공하지 않는 시기를 정확히 관리하기 원할 때 유용합니다.

**취득 주지**의 기본값은 '사용하지 않음'으로 설정되어 있습니다. 이를 변경하려면 드롭다운에서 **지정(at)** 을 선택해, 위에서 설명한대로 날짜와 시간을 지정하세요. 선택한 날짜와 시간부터 고객이 더 이상 앱을 취득할 수 없습니다.

이 옵션은 **이 앱을 검색 가능 하지만 표시 안 함 섹션에서 사용할 수** 없도록 선택 하 고 취득 중지를 [](choose-visibility-options.md#discoverability) 선택 하는 것과 동일한 영향을 줍니다 **. 직접 링크를 사용 하는 모든 고객은 제품의 상점 목록을 볼 수 있지만 이전에 제품을 소유 하 고 있거나 프로 모션 코드가 있고 Windows 10 장치를 사용 하는 경우에만 다운로드할 수 있습니다** . 앱이 새 고객에게 제공되지 않게 하려면 앱 개요 페이지에서 **앱을 사용할 수 없도록 설정**을 클릭합니다. 자세한 내용은 [스토어에서 앱 제거](guidance-for-app-package-management.md#removing-an-app-from-the-store)를 참조하세요.

> [!TIP]
> **취득 중지**날짜를 선택했는데, 나중에 앱을 다시 사용할 수 있도록 만들려면 새로 제출하고, **취득 중지**를 **사용 안함**으로 바꿔야 합니다. 업데이트한 제출이 게시된 후 앱이 다시 사용할 수 있는 상태가 됩니다.

## <a name="customize-the-schedule-for-specific-markets"></a>특정 지역/국가의 일정을 사용자 지정 

위에서 선택한 옵션이 앱을 제공하는 모든 지역/국가에 적용되는 것이 기본값입니다. 특정 지역/국가의 가격을 사용자 지정하려면 **특정 지역/국가 사용자 지정**을 클릭합니다. **지역/국가 선택** 팝업 창이 앱을 사용할 수 있도록 설정한 모든 지역/국가 목록을 표시합니다. [시장](define-pricing-and-market-selection.md) 섹션에서 제외한 시장들은 표시가 되지 않습니다. 

특정 지역/국가에 대한 일정을 추가하려면, 지역/국가를 선택한 후 **저장**을 클릭합니다. 위에서 설명한 **릴리스** 및 **취득 중지** 옵션이 표시되지만, 해당 지역/국가에만 선택 사항이 적용될 것입니다.

여러 지역/국가에 적용할 일정을 추가하려면 *지역/국가 그룹*을 생성합니다. 포함시킬 지역/국가를 선택한 후 해당 그룹의 이름을 입력합니다. (이름은 참고용으로 고객은 볼 수 없습니다.) 예를 들어, 북미에 대한 지역/국가 그룹을 생성하고 싶다면 **캐나다**, **멕시코**,**미국**을 선택한 후 **북미** 또는 다른 이름을 지정합니다. 지역/국가 그룹을 생성한 후 **저장**을 클릭합니다. 그러면 위에서 설명한 **릴리스** 및 **취득 중지** 옵션이 표시되지만, 해당 지역/국가 그룹에만 선택 사항이 적용되어 있을 것입니다.

추가로 다른 지역/국가의 일정을 사용자 지정하거나, 추가 지역/국가 그룹을 만들려면 **특정 지역/국가 사용자 지정**을 클릭한 후 위 단계를 반복합니다. 지역/국가 그룹의 지역/국가를 변경하려면 해당 이름을 선택합니다. 시장 그룹(또는 개별 시장)에 대한 사용자 지정 일정을 제거하려면 **제거**를 클릭합니다.

> [!NOTE]
> 하나의 시장은 **일정** 섹션에서 사용하는 시장 그룹 중 하나에만 소속됩니다. 

## <a name="global-time-zones"></a>글로벌 표준 시간대

다음은 각 시장에서 사용 되는 특정 표준 시간대를 보여 주는 테이블입니다. 따라서 제출 시 현지 시간을 사용 하는 경우 (예: 오전 9 시 local에서 릴리스), 각 시장에서 출시 되는 시간을 확인할 수 있습니다. 특히 두 번 이상의 시장에 도움이 됩니다. 1 (캐나다)와 같습니다.

<a name="market--time-zone"></a>시장: 표준 시간대
==================
아프가니스탄: (UTC + 04:30) 카불 알바니아: (utc + 01:00) 사라예보, 스코페, 바르샤바, 자그레브 알제리: (UTC + 01:00) 사라예보, 스코페, 바르샤바, 자그레브 미국령 사모아: (utc + 13:00) 사모아: (utc + 01:00) 사라예보, 스코페, 바르샤바, 자그레브 앙골라: (UTC + 01:00) 서 부 중앙 아프리카 앵귈라: (UTC-04:00) 대서양 표준시 (캐나다) 남극: (utc + 12:00) 오클랜드, 웰링턴 앤티가 바부다: (utc-04:00) 대서양 표준시 (캐나다) 아르헨티나: (utc-03:00) 부에노스아이레스: (utc + 04:00) 아부다비, 무스카트 아루바: ( UTC-04:00) 대서양 표준시 (캐나다) 오스트레일리아: (UTC + 10:00) 캔버라, 멜버른, 시드니 오스트리아: (UTC + 01:00) 암스테르담, 베를린, 베른, 로마, 스톡홀름, 빈 아제르바이잔: (UTC + 04:00) 바쿠 바하마,: (UTC-05:00) 동부 표준시 (US & 캐나다) 바레인: (utc + 04:00) 아부다비, 무스카트 방글라데시: (UTC + 06:00) 다카 바베이도스: (utc-04:00) 대서양 표준시 (캐나다) 벨로루시: (utc + 03:00) 민스크 벨기에: (utc + 01:00) 브뤼셀, 코펜하겐, 마드리드, 파리 벨리즈: (UTC-06:00) 중부 표준시 (US & 캐나다) 베냉: (UTC + 01:00) 서 중앙 아프리카 버뮤다: (UTC-04:00) 대서양 표준시 (캐나다) 부탄: (UTC + 06:00) 다카 베네수엘라 공화국: (utc-04:00) 카라카스 볼리비아: (utc-04:00) 조지타운, 라파스, 마나우스, San Juan 보네르, 세인트 티위 및 사바: (UTC-04:00) 대서양 표준시 (캐나다) 보스니아 헤르체고비나: (UTC + 01:00) 사라예보, 스코페, 바르샤바, 자그레브 보츠와나: (utc + 01:00) 서 중앙 아프리카 부베이 섬: (utc + 00:00) 몬로비아, 레이캬비크 브라질: (utc-03:00) 브라질리아 영국령 인디언 해상 지역: (UTC + 06:00) 다카 영국령 버진 아일랜드: (UTC-04:00) 대서양 표준시 (캐나다) 브루나이: (UTC + 08:00) 이르쿠츠크 불가리아: (utc + 02:00) 키시네프 부르키나파소 파소: (utc + 00:00) 몬로비아, 레이캬비크 부룬디: (utc + 02:00) 하라레, 프리토리아 CÃ ́te: (UTC + 00:00) 몬로비아, 레이캬비크 캄보디아: (UTC + 07:00) 방콕, 하노이, 자카르타 카메룬: (utc + 01:00) 서 중앙 아프리카 캐나다: (UTC-05:00) 동부 표준시 (US & 캐나다) 카보베르데: (UTC-01:00) 카보베르데
Cayman Islands: (UTC-05:00) Eastern Time (US & Canada) Central African Republic: (UTC+01:00) West Central Africa Chad: (UTC+01:00) West Central Africa Chile: (UTC-04:00) Santiago China: (UTC+08:00) Beijing, Chongqing, Hong Kong, Urumqi Christmas Island: (UTC+07:00) Krasnoyarsk Cocos (Keeling) Islands: (UTC+06:30) Yangon (Rangoon) Colombia: (UTC-05:00) Bogota, Lima, Quito, Rio Branco Comoros: (UTC+03:00) Nairobi Congo: (UTC+01:00) West Central Africa Congo (DRC): (UTC+01:00) West Central Africa Cook Islands: (UTC-10:00) Hawaii Costa Rica: (UTC-06:00) Central Time (US & Canada) Croatia: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb CuraÃ§ao: (UTC-04:00) Cuiaba Cyprus: (UTC+02:00) Chisinau Czech Republic: (UTC+01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague Denmark: (UTC+01:00) Brussels, Copenhagen, Madrid, Paris Djibouti: (UTC+03:00) Nairobi Dominica: (UTC-04:00) Atlantic Time (Canada) Dominican Republic: (UTC-04:00) Atlantic Time (Canada) Ecuador: (UTC-05:00) Bogota, Lima, Quito, Rio Branco Egypt: (UTC+02:00) Chisinau El Salvador: (UTC-06:00) Central Time (US & Canada) Equatorial Guinea: (UTC+01:00) West Central Africa Eritrea: (UTC+03:00) Nairobi Estonia: (UTC+02:00) Chisinau Ethiopia: (UTC+03:00) Nairobi Falkland Islands (Islas Malvinas): (UTC-04:00) Santiago Faroe Islands: (UTC+00:00) Dublin, Edinburgh, Lisbon, London Fiji: (UTC+12:00) Fiji Finland: (UTC+02:00) Helsinki, Kyiv, Riga, Sofia, Tallinn, Vilnius France: (UTC+01:00) Brussels, Copenhagen, Madrid, Paris French Guiana: (UTC-03:00) Cayenne, Fortaleza French Polynesia: (UTC-10:00) Hawaii French Southern and Antarctic Lands: (UTC+05:00) Ashgabat, Tashkent Gabon: (UTC+01:00) West Central Africa Gambia, The: (UTC+00:00) Monrovia, Reykjavik Georgia: (UTC-05:00) Eastern Time (US & Canada) Germany: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Ghana: (UTC+00:00) Monrovia, Reykjavik Gibraltar: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Greece: (UTC+02:00) Athens, Bucharest Greenland: (UTC+00:00) Monrovia, Reykjavik Grenada: (UTC-04:00) Atlantic Time (Canada) Guadeloupe: (UTC-04:00) Atlantic Time (Canada) Guam: (UTC+10:00) Guam, Port Moresby Guatemala: (UTC-06:00) Central Time (US & Canada) Guernsey: (UTC+00:00) Monrovia, Reykjavik Guinea: (UTC+00:00) Monrovia, Reykjavik Guinea-Bissau: (UTC+00:00) Monrovia, Reykjavik Guyana: (UTC-04:00) Atlantic Time (Canada) Haiti: (UTC-05:00) Eastern Time (US & Canada) Heard Island and McDonald Islands: (UTC-05:00) Bogota, Lima, Quito, Rio Branco Holy See (Vatican City): (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Honduras: (UTC-06:00) Central Time (US & Canada) Hong Kong SAR: (UTC+08:00) Beijing, Chongqing, Hong Kong, Urumqi Hungary: (UTC+01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague Iceland: (UTC+00:00) Monrovia, Reykjavik India: (UTC+05:30) Chennai, Kolkata, Mumbai, New Delhi Indonesia: (UTC+07:00) Bangkok, Hanoi, Jakarta Iraq: (UTC+04:00) Abu Dhabi, Muscat Ireland: (UTC+00:00) Dublin, Edinburgh, Lisbon, London Israel: (UTC+02:00) Jerusalem Italy: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Jamaica: (UTC-05:00) Eastern Time (US & Canada) Japan: (UTC+09:00) Osaka, Sapporo, Tokyo Jersey: (UTC+00:00) Monrovia, Reykjavik Jordan: (UTC+02:00) Chisinau Kazakhstan: (UTC+05:00) Ashgabat, Tashkent Kenya: (UTC+03:00) Nairobi Kiribati: (UTC+14:00) Kiritimati Island Korea: (UTC+09:00) Seoul Kuwait: (UTC+04:00) Abu Dhabi, Muscat Kyrgyzstan: (UTC+06:00) Astana Laos: (UTC+07:00) Bangkok, Hanoi, Jakarta Latvia: (UTC+02:00) Chisinau Lebanon: (UTC+02:00) Chisinau Lesotho: (UTC+02:00) Harare, Pretoria Liberia: (UTC+00:00) Monrovia, Reykjavik Libya: (UTC+02:00) Chisinau Liechtenstein: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Lithuania: (UTC+02:00) Chisinau Luxembourg: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Macao SAR: (UTC+08:00) Beijing, Chongqing, Hong Kong, Urumqi Macedonia, FYROM: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Madagascar: (UTC+03:00) Nairobi Malawi: (UTC+02:00) Harare, Pretoria Malaysia: (UTC+08:00) Kuala Lumpur, Singapore Maldives: (UTC+05:00) Ashgabat, Tashkent Mali: (UTC+00:00) Monrovia, Reykjavik Malta: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Man, Isle of: (UTC+00:00) Dublin, Edinburgh, Lisbon, London Marshall Islands: (UTC+12:00) Petropavlovsk-Kamchatsky - Old Martinique: (UTC-04:00) Atlantic Time (Canada) Mauritania: (UTC+00:00) Monrovia, Reykjavik Mauritius: (UTC+04:00) Port Louis Mayotte: (UTC+03:00) Nairobi Mexico: (UTC-06:00) Guadalajara, Mexico City, Monterrey Micronesia: (UTC+10:00) Guam, Port Moresby Moldova: (UTC+02:00) Chisinau Monaco: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Mongolia: (UTC+07:00) Krasnoyarsk Montenegro: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Montserrat: (UTC-04:00) Atlantic Time (Canada) Morocco: (UTC+01:00) Casablanca Mozambique: (UTC+02:00) Harare, Pretoria Myanmar: (UTC+06:30) Yangon (Rangoon) Namibia: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Nauru: (UTC+12:00) Petropavlovsk-Kamchatsky - Old Nepal: (UTC+05:45) Kathmandu Netherlands: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna New Caledonia: (UTC+11:00) Solomon Is., New Caledonia New Zealand: (UTC+12:00) Auckland, Wellington Nicaragua: (UTC-06:00) Central Time (US & Canada) Niger: (UTC+01:00) West Central Africa Nigeria: (UTC+01:00) West Central Africa Niue: (UTC+13:00) Samoa Norfolk Island: (UTC+11:00) Solomon Is., New Caledonia Northern Mariana Islands: (UTC+10:00) Guam, Port Moresby Norway: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Oman: (UTC+04:00) Abu Dhabi, Muscat Pakistan: (UTC+05:00) Islamabad, Karachi Palau: (UTC+09:00) Osaka, Sapporo, Tokyo Palestinian Authority: (UTC+02:00) Chisinau Panama: (UTC-05:00) Eastern Time (US & Canada) Papua New Guinea: (UTC+10:00) Vladivostok Paraguay: (UTC-04:00) Asuncion Peru: (UTC-05:00) Bogota, Lima, Quito, Rio Branco Philippines: (UTC+08:00) Kuala Lumpur, Singapore Pitcairn Islands: (UTC-08:00) Pacific Time (US & Canada) Poland: (UTC+01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague Portugal: (UTC+00:00) Dublin, Edinburgh, Lisbon, London Qatar: (UTC+04:00) Abu Dhabi, Muscat Reunion: (UTC+04:00) Port Louis Romania: (UTC+02:00) Chisinau ROW: (UTC-07:00) Mountain Time (US & Canada) Russia: (UTC+03:00) Moscow, St. Petersburg Rwanda: (UTC+02:00) Harare, Pretoria SÃ£o TomÃ© and PrÃ­ncipe: (UTC+00:00) Monrovia, Reykjavik Saint BarthÃ©lemy: (UTC+04:00) Yerevan Saint Helena, Ascension and Tristan da Cunha: (UTC+00:00) Dublin, Edinburgh, Lisbon, London Saint Kitts and Nevis: (UTC-04:00) Atlantic Time (Canada) Saint Lucia: (UTC-04:00) Atlantic Time (Canada) Saint Martin (French Part): (UTC-04:00) Atlantic Time (Canada) Saint Pierre and Miquelon: (UTC-02:00) Mid-Atlantic - Old Saint Vincent and the Grenadines: (UTC-04:00) Atlantic Time (Canada) Samoa: (UTC+13:00) Samoa San Marino: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Saudi Arabia: (UTC+03:00) Kuwait, Riyadh Senegal: (UTC+00:00) Monrovia, Reykjavik Serbia: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Seychelles: (UTC+04:00) Abu Dhabi, Muscat Sierra Leone: (UTC+00:00) Monrovia, Reykjavik Singapore: (UTC+08:00) Kuala Lumpur, Singapore Sint Maarten (Dutch Part): (UTC-04:00) Atlantic Time (Canada) Slovakia: (UTC+01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague Slovenia: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Solomon Islands: (UTC+11:00) Solomon Is., New Caledonia Somalia: (UTC+03:00) Nairobi South Africa: (UTC+02:00) Harare, Pretoria South Georgia and the South Sandwich Islands: (UTC-02:00) Mid-Atlantic - Old Spain: (UTC+01:00) Brussels, Copenhagen, Madrid, Paris Sri Lanka: (UTC+05:30) Chennai, Kolkata, Mumbai, New Delhi Suriname: (UTC-03:00) Cayenne, Fortaleza Svalbard and Jan Mayen: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Swaziland: (UTC+02:00) Harare, Pretoria Sweden: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Switzerland: (UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna Taiwan: (UTC+08:00) Taipei Tajikistan: (UTC+05:00) Ashgabat, Tashkent Tanzania: (UTC+03:00) Nairobi Thailand: (UTC+07:00) Bangkok, Hanoi, Jakarta Timor-Leste: (UTC+09:00) Seoul Togo: (UTC+00:00) Monrovia, Reykjavik Tokelau: (UTC+13:00) Nuku'alofa Tonga: (UTC+13:00) Nuku'alofa Trinidad and Tobago: (UTC-04:00) Atlantic Time (Canada) Tunisia: (UTC+01:00) Sarajevo, Skopje, Warsaw, Zagreb Turkey: (UTC+03:00) Istanbul Turkmenistan: (UTC+05:00) Ashgabat, Tashkent Turks and Caicos Islands: (UTC-05:00) Eastern Time (US & Canada) Tuvalu: (UTC+12:00) Petropavlovsk-Kamchatsky - Old U.S. Minor Outlying Islands: (UTC+13:00) Samoa U.S. Virgin Islands: (UTC-04:00) Atlantic Time (Canada) Uganda: (UTC+03:00) Nairobi Ukraine: (UTC+02:00) Chisinau United Arab Emirates: (UTC+04:00) Abu Dhabi, Muscat United Kingdom: (UTC+00:00) Dublin, Edinburgh, Lisbon, London United States: (UTC-05:00) Eastern Time (US & Canada) Uruguay: (UTC-03:00) Brasilia Uzbekistan: (UTC+05:00) Ashgabat, Tashkent Vanuatu: (UTC+11:00) Solomon Is., New Caledonia Vietnam: (UTC+07:00) Bangkok, Hanoi, Jakarta Wallis and Futuna: (UTC+12:00) Petropavlovsk-Kamchatsky - Old Western Sahara (Disputed): (UTC+00:00) Dublin, Edinburgh, Lisbon, London Yemen: (UTC+04:00) Abu Dhabi, Muscat Zambia: (UTC+02:00) Harare, Pretoria Zimbabwe: (UTC+02:00) Harare, Pretoria
