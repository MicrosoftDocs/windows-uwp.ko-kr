---
title: EDS 매개 변수
assetID: 9475b427-53bc-697b-6d24-1787320260b7
permalink: en-us/docs/xboxlive/rest/edsparameters.html
author: KevinAsgari
description: " EDS 매개 변수"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e115f240eb26ea782d67f2c96fd0f702d1844973
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5557036"
---
# <a name="eds-parameters"></a>EDS 매개 변수

<a id="ID4EO"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

이러한 쿼리 매개 변수 모든 [엔터테인먼트 검색 서비스 (EDS) Api](../uri/marketplace/atoc-reference-marketplace.md)반드시 수락 하지 않으면 있지만 둘 이상의 API에 의해 허용 되는 모든.

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| combinedContentRating| string| 선택 사항입니다. 참조 [GET (/media/ {marketplaceId} / contentRating)](../uri/marketplace/uri-medialocalecontentratingget.md).|
| continuationToken| string| 선택 사항입니다. 연속 토큰에는 특정 시나리오에서 매김에 필요한 서비스 정보를 포함 하는 불투명 blob입니다. 값을 지정 하지 않으면 결과의 첫 번째 페이지 반환 됩니다 (여기서 페이지 크기에 따라 결정 되므로 maxItems 매개 변수)의 두 번째 결과 페이지를 가져오는 데 사용할 수 있는 연속 토큰을 함께. 두 번째 페이지는 세 번째 결과 페이지에 대 한 연속 토큰을 포함 하 고 등.|
| 원하는| string| 선택 사항입니다. 결합 된 필드 이름 API를 참조 하세요.|
| desiredMediaItemTypes| 문자열의 배열| 선택 사항입니다. 이 매개 변수는 응답에서 반환 되는 항목의 종류를 결정 합니다.|
| 도메인| string| 선택 사항입니다. 게임 및 앱 marketplace 컨텍스트를 결정 하는 도메인 매개 변수는 클라이언트에 대 한 호출 됩니다. 기본적으로 도메인 클라이언트 Xbox One 콘텐츠만 요청할 수를 나타내는 "최신" 됩니다. 클라이언트가 도메인 surface Xbox 360 콘텐츠를 전환할 것으로 "Xbox360" 도메인을 지정 해야 합니다. 현재 도메인 간 결과 지원 되지 않습니다. 가능한 값은 다음과 같습니다. <ul><li>Xbox360</li><li>최신</li></ul> SingleMediaGroupSearch "Xbox360" 값만 지원 됩니다. 찾아보기 및 세부 정보 지원 됩니다. crossMediaGroupSearch은 지원 되지 않으며 400 오류를 반환 합니다.|
| 필드| string| 선택 사항입니다. 참조 [GET (/media/ {marketplaceId} /fields)](../uri/marketplace/uri-medialocalefieldsget.md).|
| firstPartyOnly| 부울 값| 선택적 필터 매개 변수입니다. 만 자사 콘텐츠가 반환 됩니다 여부 모두 및 제 3-자사 콘텐츠 쿼리에서 반환 여부를 결정 합니다. |
| freeOnly| 부울 값| 선택적 필터 매개 변수입니다. 만 콘텐츠를 확보 하는 결과 제한 합니다.|
| GroupBy| TK| GroupBy 매개 변수를 그룹으로 결과 집합 것이 아니라 단일 결과 집합을 분류 하는 데 사용 됩니다. 이 매개 변수를 지정 합니다. 각 버킷 항목 수가 maxItems 매개 변수는 따른 여러 목록 항목을 반환 하도록 설정 하는 결과 수정 합니다. <ul><li>MediaGroup-결과 MediaGroup 별로 그룹화 됩니다.</li></ul> |
| hasTrailer| 부울 값| 선택적 필터 매개 변수입니다. 예고편 것은 선택 사항이 경우 또는 반환된 된 항목 예고편을 포함 해야 하는지 여부를 결정 합니다. 값이 true 인 경우 모든 항목에 예고편 있어야 합니다.|
| id| string| 선택 사항입니다. 제공 하는 경우 해당된 ID 사용 하 여 항목의 하위로만 결과 제한 이 매개 변수를 제공 하는 경우에 MediaItemType 지정 해야 합니다. |
| id| 문자열의 배열| 필수. 모든 세부 정보가 반환 됩니다 (최대 10) Id. 참고 하는 ID 모든 문자가 모드로 불법적인 EDS를 보내도록 제대로 URL 인코딩 (ProviderContentId 유형 Id 자체는 일반적으로 전체 url 및 따라서 잘못 된 문자가 포함)는 URL 이어야 합니다.|
| idType| string| 선택 사항입니다. 유형 id 매개 변수에 전달 되는 Id입니다. 유효한 값은 다음과 같습니다. <ul><li>정식 (Bing/Marketplace)</li><li>XboxHexTitle (앱이 콘솔에서 재생)</li></ul>  모든 제공 Id 동일한 idType를 공유 해야 합니다. 이 값을 지정 하지 않으면 모든 Id 정식으로 간주 됩니다.|
| latestOnly| 부울 값| 선택적 필터 매개 변수입니다. 최신 릴리스 날짜를 사용 하 여 이벤트만에 결과 제한합니다.|
| maxItems| 32 비트 부호 있는 정수| 선택 사항입니다. 호출에서 반환 되어야 하는 항목의 최대 수를 결정 합니다. 유효한 값은 1, 25 사이의 숫자. 매개 변수 기본값 25 생략 합니다.|
| mediaGroup| string| 선택 사항입니다. 미디어 그룹 Id입니다. 모든 제공 Id 같은 미디어 그룹을 공유 해야 합니다.|
| MediaItemType| string| 선택 사항입니다. ID id 매개 변수에서 지정 된 항목의 형식입니다. Id 매개 변수를 제공 하는 경우에이 매개 변수 지정 해야 합니다.|
| orderBy| string| 필수. OrderBy 매개 변수를 반환 하는 데 사용 되는 항목을 정렬할 하는 방법을 결정 합니다. 이 필드에 대 한 일반적인 값 여기에 나열 되어 있지만 일부 Api는 추가 값을 지원할 수 있습니다.<ul><li>playCountDaily-횟수 하 여 가장 최근 날짜에 대 한 미디어를 재생 합니다.</li><li>-freeAndPaidCountDaily 하 여 가장 최근 날짜에 대 한 무료 및 유료 구매의 수입니다.</li><li>-paidCountAllTime 하 여 모든 시간에 대 한 유일한 유료 구매의 수입니다.</li><li>-paidCountDaily 하 여 가장 최근 날짜에 대 한 유료 구매만의 수입니다.</li><li>digitalReleaseDate-다운로드에 사용할 수 있는 날짜입니다.</li><li>releaseDate-저장소에서 사용할 수 있는 날짜 (가능한 경우) 디지털 릴리스 날짜를 대체 합니다.</li><li>userRatings-평균 사용자 등급에 따라 합니다.</li></ul> |
| preferredProvider| string| 선택 사항입니다. 사용자 Comcast Xfinity 또는 Verizon FIOS 등의 기본 콘텐츠 공급자가 하는 경우에 해당 공급자의 ID는 전달할 수 있습니다. 각 항목에 대 한 변경 되지 않습니다 항목의 실제 순서 동안 지정 된 공급자의 정보 (기본 콘텐츠 공급자가 사용할 수 있는 항목) 하는 경우 공급자의 목록 맨 위에 있는 표시 됩니다.|
| q| string| 필수. 검색에 사용 된 용어를 쿼리 합니다.|
| queryRefiners| 문자열의 배열| 선택 사항입니다. [EDS 쿼리 구체화](edsqueryrefiners.md)목록을 참조 하세요.|
| 관계| string| 선택 사항입니다. 지정 된 관계 형식과 일치 하는 다른 제품에 대 한 검색을 바탕으로 ID 매개 변수를 사용 하는 필터: <ul><li>bundledWith-찾기 번들 제품 ID 매개 변수를 해당 번들의 일부로 포함 하는 위치입니다.</li><li>bundledProducts-ID 매개 변수로 지정 된 번들에 포함 된 제품을 찾습니다.</li></ul>  이 매개 변수를 사용 하 여 시장 (찾아보기 호출에서 반환 될 수 있습니다)에 표시 되는 제품만 반환 됩니다. 번들 숨겨진된 제품에 있는 경우 번들의 일부인 여전히 있지만 이러한 결과에 반환 되지 않습니다.|
  | ScopeId | string | 이 매개 변수는 비디오 미디어에 대 한 역방향 조회 시나리오에서 사용 됩니다. |
  | ScopeIdType | string | 이 매개 변수는 비디오 미디어에 대 한 역방향 조회 시나리오에서 사용 됩니다. 가능한 값: 제목입니다. |
  | skipItems | 32 비트 부호 있는 정수 | 선택 사항입니다. 비 그룹 간 시나리오에서는 페이징, skipItems 매개 변수는 항목 수가 보았습니다 이미 결정 데 (및 따라서 항목에 대해 표시 되어야 먼저 결과의 설정). 값이 0부터 시작 하므로 skipItems = 0 (또는 간단 하 게 skipItems을 제공 하지)의 목록 시작 시 검색을 시작 합니다. skipItems = 3 목록의 처음 세 개의 항목을 건너뛰고 고 네 번째 항목을 사용 하 여 검색을 시작 합니다. |
  | subscriptionLevel | 문자열의 배열 | 선택적 필터 매개 변수입니다. SubscriptionLevel 매개 변수 (예: 사용자에 있는지 여부를 유료 구독 또는 무료 구독)가 사용자 구독 유형을 결정 합니다. 가능한 값은 다음과 같습니다. <ul><li>골드: 사용자가 유료 구독</li><li>실버: 사용자가 무료 구독.</li></ul>  |
| TargetDevices| string| EDS 대상 장치에 대 한 제품을 필터링 하는 유연성을 제공 합니다. 항목에 대해 반환 된 제품 (ProviderContent 또는 가용성)는 대상 디바이스를 제한할 수 있습니다.|
| topRatedOnly| 부울 값| 선택적 필터 매개 변수입니다. Best-rated 콘텐츠만에 결과 제한합니다.|

<a id="ID4EGEAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EIEAC"></a>


### <a name="parent"></a>부모

[추가 참조](atoc-xboxlivews-reference-additional.md)


<a id="ID4EUEAC"></a>


### <a name="further-information"></a>자세한 정보

[마켓플레이스 URI](../uri/marketplace/atoc-reference-marketplace.md)
