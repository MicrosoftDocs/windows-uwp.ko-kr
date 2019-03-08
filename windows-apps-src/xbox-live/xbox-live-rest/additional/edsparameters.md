---
title: EDS 매개 변수
assetID: 9475b427-53bc-697b-6d24-1787320260b7
permalink: en-us/docs/xboxlive/rest/edsparameters.html
description: " EDS 매개 변수"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5412bcebc73dfd25d81b2073527e64e81de1bba4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655418"
---
# <a name="eds-parameters"></a>EDS 매개 변수

<a id="ID4EO"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

이러한 쿼리 매개 변수 모두에서 반드시 수락 되지 않습니다 [엔터테인먼트 검색 서비스 (EDS) Api](../uri/marketplace/atoc-reference-marketplace.md), 하지만 둘 이상의 API에서 허용 하는 모든.

| 매개 변수| 형식| 설명|
| --- | --- | --- |
| combinedContentRating| 문자열| 선택 사항. 참조 [GET (/media/ {marketplaceId} / contentRating)](../uri/marketplace/uri-medialocalecontentratingget.md)합니다.|
| continuationToken| 문자열| 선택 사항. 연속 토큰은 서비스 특정 시나리오의 페이지 매김에 대 한 필요한 정보를 포함 하는 불투명 blob입니다. 값 생략 결과의 첫 페이지는 (여기서 페이지 크기에 따라 결정 되므로 maxItems 매개 변수) 함께 반환 결과의 두 번째 페이지를 가져오는 데 사용할 수 있는 연속 토큰입니다. 두 번째 페이지는 결과의 세 번째 페이지에 대 한 연속 토큰이 포함 등에입니다.|
| 원하는| 문자열| 선택 사항. 결합 된 필드 이름을 API를 참조 하세요.|
| desiredMediaItemTypes| 문자열의 배열| 선택 사항. 이 매개 변수는 응답에서 반환할 항목의 형식을 결정 합니다.|
| 도메인| 문자열| 선택 사항. 게임 및 앱 marketplace 컨텍스트를 결정 하는 도메인 매개 변수는 클라이언트에 대 한 호출 됩니다. 기본 도메인은 클라이언트 에서만 Xbox One 콘텐츠 요청을 나타내는 "최신"입니다. 클라이언트에서 도메인을 Xbox 360 콘텐츠 전환 하려는 경우 도메인 "Xbox360"으로 지정 해야 합니다. 현재 도메인 간 결과 지원 되지 않습니다. 가능한 값은 다음과 같습니다. <ul><li>Xbox360</li><li>최신</li></ul> SingleMediaGroupSearch에 대 한 "Xbox360" 값만 지원 됩니다. 찾아보기 및 세부 정보는 지원 됩니다. crossMediaGroupSearch은 지원 되지 않으며 400 오류가 반환 됩니다.|
| 필드| 문자열| 선택 사항. 참조 [GET (/media/ {marketplaceId} 필드 /)](../uri/marketplace/uri-medialocalefieldsget.md)합니다.|
| firstPartyOnly| 부울 값| 선택적 필터 매개 변수입니다. 첫 번째 파티만 콘텐츠는 반환 여부를 모두 첫 번째 자사 및 타사 콘텐츠는 쿼리에서 반환 된 되는지 결정 됩니다. |
| freeOnly| 부울 값| 선택적 필터 매개 변수입니다. 무료 콘텐츠 결과 제한 합니다.|
| GroupBy| TK| GroupBy 매개 변수는 그룹으로 결과 집합 대신 단일 결과 집합을 분류 하는 데 사용 됩니다. 이 매개 변수를 지정 하는 maxItems 매개 변수가 각 버킷의 항목 수를 결정 하는 경우 여러 항목 목록을 반환 하도록 결과 집합을 수정 합니다. <ul><li>MediaGroup-결과 MediaGroup 그룹화 됩니다.</li></ul> |
| hasTrailer| 부울 값| 선택적 필터 매개 변수입니다. 트레일러가 있으면 선택 사항 인지 또는 반환 된 항목의 트레일러가 있어야 하는지 여부를 결정 합니다. 값이 true 이면 모든 항목 트레일러가 있어야 합니다.|
| id| 문자열| 선택 사항. 제공 된 경우 지정된 된 ID 사용 하 여 항목의 자식일 수만 결과가 제한 이 매개 변수를 제공 하는 경우 MediaItemType도 지정 해야 합니다. |
| id| 문자열의 배열| 필수. 모든 세부 정보가 반환 됩니다 (최대 10 명) Id입니다. 참고는 ID 하나에 잘못 된 문자를 포함 (ProviderContentId 형식 Id 자체는 일반적으로 전체 url 및 잘못 된 문자가 포함 되어 있으므로) URL을 URL로 인코딩되어야 EDS 전송할 수 있도록 합니다.|
| idType| 문자열| 선택 사항. Id 매개 변수에 전달 되는 Id의 형식입니다. 유효한 값은 다음과 같습니다. <ul><li>정식 (Bing/Marketplace)</li><li>XboxHexTitle (콘솔에서 재생 하는 앱)</li></ul>  모든 제공 Id 같은 idType를 공유 해야 합니다. 이 값을 생략 하면 모든 Id Canonical 간주 됩니다.|
| latestOnly| 부울 값| 선택적 필터 매개 변수입니다. 최신 릴리스 날짜가 포함 된만 결과 제한합니다.|
| maxItems| 32 비트 부호 있는 정수| 선택 사항. 호출에서 반환 되어야 하는 항목의 최대 수를 결정 합니다. 유효한 값은 1에서 25 사이의 숫자입니다. 생략 하면 25 매개 변수 기본값은입니다.|
| mediaGroup| 문자열| 선택 사항. 미디어 그룹 Id입니다. 모든 제공 Id 같은 미디어 그룹을 공유 해야 합니다.|
| MediaItemType| 문자열| 선택 사항. Id 매개 변수에 지정 된 ID를 가진 항목의 형식입니다. Id 매개 변수에 제공 하는 경우이 매개 변수도 지정 해야 합니다.|
| orderBy| 문자열| 필수. OrderBy 매개 변수는 반환 되는 항목을 정렬 방법을 결정 합니다. 이 필드에 대 한 공통의 가치는 여기에 나열 되지만 일부 Api는 추가 값을 지원할 수 있습니다.<ul><li>playCountDaily-횟수에서 가장 최근 날짜에 대 한 미디어를 재생 합니다.</li><li>freeAndPaidCountDaily-가장 최근 날짜에 대 한 무료 및 유료 구매 개수로 합니다.</li><li>paidCountAllTime-모든 시간에 대 한 유일한 유료 구매를 수 여 합니다.</li><li>paidCountDaily-가장 최근 날짜에 대 한 유료 구매만 개수로 합니다.</li><li>digitalReleaseDate-다운로드에 사용할 수 있는 날짜입니다.</li><li>releaseDate-저장소에 사용할 수 있는 날짜 (있는 경우) 디지털 출시일을 대체 합니다.</li><li>userRatings-사용자 평균 등급에 따라 합니다.</li></ul> |
| preferredProvider| 문자열| 선택 사항. 사용자가 Comcast Xfinity 또는 Verizon FIOS와 같은 기본 콘텐츠 공급자에 해당 공급자의 ID는 전달할 수 있습니다. 각 항목에 대해 변경 되지 않습니다 항목의 실제 순서는 동안 지정 된 공급자의 정보 (선호 하는 콘텐츠 공급자는 항목을 사용할 수 있는 경우) 공급자의 목록 맨 위에 있는 표시 됩니다.|
| q:| 문자열| 필수. 검색에 사용 된 용어를 쿼리 합니다.|
| queryRefiners| 문자열의 배열| 선택 사항. 목록은 [EDS 쿼리 구체화](edsqueryrefiners.md)합니다.|
| 관계| 문자열| 선택 사항. 지정 된 관계 유형과 일치 하는 다른 제품에 대 한 검색을 기반으로 ID 매개 변수를 사용 하는 필터: <ul><li>bundledWith-찾기 번들 제품 ID 매개 변수를 해당 번들의 일부로 포함 하는 위치입니다.</li><li>bundledProducts-ID 매개 변수로 지정 된 번들에 포함 된 제품을 찾습니다.</li></ul>  이 매개 변수를 사용 하 여 (찾아보기 호출에서 반환 될 수 있습니다) marketplace에 표시 된 제품만 반환 됩니다. 번들에 숨겨진된 제품 번들의 일부인 여전히 있지만 이러한 결과에 반환 되지 않습니다.|
  | ScopeId | 문자열 | 이 매개 변수는 비디오 미디어에 대 한 역방향 조회 시나리오에서 사용 됩니다. |
  | ScopeIdType | 문자열 | 이 매개 변수는 비디오 미디어에 대 한 역방향 조회 시나리오에서 사용 됩니다. 가능한 값: 제목입니다. |
  | skipItems | 32 비트 부호 있는 정수 | 선택 사항. 비 그룹 간 시나리오에서 페이징, skipItems 매개 변수를 이미 표시 된 항목 수 확인 사용 (및 따라서 항목에 대해 표시 된 경우 먼저 결과 집합에서). 값이 0부터 시작 하므로 skipItems = 0 (또는 단순히 skipItems 제공 안 함) 목록의 시작 부분에서 검색을 시작 합니다. skipItems = 3은 목록의 처음 세 개의 항목을 건너뛰고 네 번째 항목을 사용 하 여 검색을 시작 합니다. |
  | subscriptionLevel | 문자열의 배열 | 선택적 필터 매개 변수입니다. SubscriptionLevel 매개 변수는 사용자 구독 유형 (예: 사용자에 있는지 여부는 무료 구독 또는 유료 구독)가 결정 합니다. 가능한 값은 다음과 같습니다. <ul><li>gold: 사용자가 유료 구독</li><li>silver: 사용자는 무료 구독을 있습니다.</li></ul>  |
| TargetDevices| 문자열| EDS 제공 대상 장치에 대 한 필터링 할 수 있는 유연성을 제공 합니다. 항목에 대해 반환 된 제안 (ProviderContent 또는 가용성)는 대상 장치로 제한할 수 있습니다.|
| topRatedOnly| 부울 값| 선택적 필터 매개 변수입니다. 최고 등급된 콘텐츠만 한 결과 제한합니다.|

<a id="ID4EGEAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EIEAC"></a>


### <a name="parent"></a>Parent

[추가 참조](atoc-xboxlivews-reference-additional.md)


<a id="ID4EUEAC"></a>


### <a name="further-information"></a>추가 정보

[Marketplace Uri](../uri/marketplace/atoc-reference-marketplace.md)
