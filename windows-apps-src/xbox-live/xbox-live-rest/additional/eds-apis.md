---
title: 보조 EDS API
assetID: 5729ab80-e88d-0190-fb61-bd0cc4f134f6
permalink: en-us/docs/xboxlive/rest/eds-apis.html
author: KevinAsgari
description: " 보조 EDS API"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 791ec5e593d90cf52b91cca863df02da2db97f5f
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5936067"
---
# <a name="auxiliary-eds-apis"></a>보조 EDS API

여러 엔터테인먼트 검색 서비스 (EDS) Api 직접 내용에 대 한 정보를 제공 하지 하지만 드라이브 일반적인 UI 모델 또는 서비스를 사용 하는 방법에 대 한 일반 정보를 제공 하는 경우

<a id="ID4EQ"></a>


## <a name="auxiliary-apis"></a>보조 Api

| API| URI| 설명|
| --- | --- | --- |
| API 매개 변수 값| / {로캘} / 메타 데이터| 서비스에 대 한 호출에 사용할 수 있는 매개 변수에 가능한 값의 열거|
| 콘텐츠 등급 생성기를 결합합니다.| / {로캘} / contentRating| 잠재적으로 유해한 또는 명시적 콘텐츠 필터링에 대 한 다른 Api에 사용할 수 있는 값을 만듭니다. 자세한 내용은 아래를 참조 하세요.|
| 결합 된 필드 이름 생성기| / {로캘} /fields| 자세히 Api 필드가 반환 되는 컨트롤을 사용할 수 있는 값을 만듭니다. 자세한 내용은 아래를 참조 하세요.|

<a id="ID4EBC"></a>


### <a name="api-parameter-values"></a>API 매개 변수 값

이 API 서비스와 함께 사용할 수 있는 매개 변수를 설명 합니다. 반환 된 정보는 지역화 된 텍스트와 각 매개 변수가 함께 제공 하기 때문에 UI 클라이언트에서 사용할 수 있습니다.

아래 Api 중 모든 쿼리 매개 변수를 수락합니다.

| API| URI| 설명|
| --- | --- | --- | --- | --- | --- |
| 형식| / {로캘} / 메타 데이터/mediaGroups| 미디어 그룹의 전체 목록|
| 미디어 항목 미디어 그룹당 형식| / {로캘} / /metadata/mediagroups/ {mediaItemTypeGroup} / mediaItemTypes| 미디어 목록 항목 형식 지정 된 미디어 그룹 내에 포함 합니다.|
| 모든 미디어 항목 유형| / {로캘} / 메타 데이터/mediaItemTypes| 미디어 항목 종류의 전체 목록|
| 미디어 항목 종류에 따라 필드| / {로캘} / /metadata/mediaitemtypes/ {mediaItemType} /fields| 단일 미디어 항목 형식에는 필드 목록|
| 쿼리 구체화| / {로캘} / /metadata/mediaitemtypes/ {mediaItemType} / queryRefiners| 지정 된 미디어 항목 유형에 대 한 지원 쿼리 구체화 목록|
| 모든 쿼리 구체화 값| / {로캘} / 메타 데이터/mediaItemTypes / {mediaItemType} {queryRefiner} /queryRefiners/| 지정 된 미디어에 대 한 지정 된 쿼리 구체화에 대 한 값 항목 유형|
| 모든 쿼리 구체화 하위 값| / {로캘} / 메타 데이터/mediaItemTypes / {mediaItemType} {queryRefiner} /queryRefiners/ / subQueryRefinerValues| 지정 된 쿼리 구체화 값 (예: "subgenres 주어진 장르의")에 대 한 하위 값의 목록입니다. 쿼리 구체화 값에 전달할 수 URI 획에서 금지 문자로 쿼리 구체화 값을 허용 하도록 완료 되는 "queryRefinerValue" 라는 쿼리 문자열 매개 변수로 전달 됩니다.|
| 정렬 합니다.| / {로캘} / /metadata/mediaitemtypes/ {mediaItemType} / sortOrders| 정렬 순서 지정 된 미디어 항목 유형에 대 한 목록|

<a id="ID4EEF"></a>


### <a name="combined-content-rating-generator"></a>콘텐츠 등급 생성기를 결합합니다.

복잡 한 작업은 자식 볼 수 있는 콘텐츠 위에 자녀 보호를 적용 합니다. 각 미디어 항목 형식에는 자체 등급 시스템 할 뿐만 아니라 이러한 등급 시스템 국가 마다 다를 수 있습니다. 이 데이터를 제대로 모든 항목을 필터링 하려면 지정 해야 하는 다른 여러 가지는 것을 의미 합니다.

모든 매개 변수를 모든 API 호출에 지정 하는 대신이 API의 다른 Api combinedContentRating 매개 변수를 전달 하 고 여전히 동일한 정보를 전달 하기 위한 값을 생성 합니다. 이이 API에 전달 되는 여러 매개 변수가 다른 Api에 대 한 단일, 재사용 가능한 값으로 축소 된 대로 Api를 사용 하 고 유지 관리 쉽게 수 있도록 설계 되었습니다.

이 API에 의해 반환 되는 정확한 값 결국 변경 될 수 있지만 해야 바뀌는 매우 드물게 (예: EDS 릴리스의) 따라서 오랜 시간 동안 캐시 될 수 있습니다. CombinedContentRating 매개 변수에서 의미 있는 오류 메시지가 표시에 전달 된 값 잘못 된 경우 표시 되는 호출자 단순히 수락 모든 API는 다시 업데이트 된 값을 얻으려면이 API를 호출 해야 합니다. API combinedContentRating 매개 변수를 수락 하지만 하나 제공 하지 않는 경우 콘텐츠 필터링 없이 적용 됩니다 자녀에 따라

> [!NOTE]
> 그렇다고 "안전한"만 콘텐츠는 반환 했음을-의미 명시적 잠재적으로 콘텐츠를 비롯 한 모든 콘텐츠 반환 됩니다).



<a id="ID4EWF"></a>


### <a name="combined-field-name"></a>결합 된 필드 이름

EDS Api, 기본적으로 각 항목에 대 한 필드 집합이 매우 작은 최소를 반환합니다.

   * 미디어 항목 유형
   * 미디어 그룹
   * Id
   * 이름

자세한 정보를 가져오려면 Api는 데이터의 추가 조각을 반환할지를 지정 하는 "필드" 매개 변수를 수락 합니다. 많은 가능한 필드를 각 API 호출에 대 한 전체에 자신의 이름을 지정 하는 요청 크게 볼록 합니다. 대신, 이름은 다른 Api로 전달 될 수 있는 훨씬 더 작은 값을 생성할이 API에 전달할 수 있습니다.

제공 된 값이 매개이 변수를 허용 하는 모든 API에 대 한 모든 지정 된 미디어 항목 종류의 모든 필드의 상위 집합 이어야 합니다. 서로 다른 미디어 항목 유형에 대 한 필드를 지정 하는 것이 불가능 합니다. 그러나 필드 하나 이상의 미디어 항목 형식을 다른을 제외한이 적용 되는 경우에 표시 됩니다 미디어 항목 종류 데이터 존재 하는 위치 (예: "AvatarBodyType" 필드 이름 결합 API에 대 한 호출에 포함 되어만 AvatarItems 필드를 포함 됩니다).

이 API에서 반환 되는 값은 매우 캐시할-사실, EDS의 배포 사이 제외 하 고 변경 되지 않아야 합니다. 필요한 캐싱의 경우 캐시 마지막 사용자의 세션 보다 더 이상 것이 좋습니다.

실제 필드 이름은 적용 하는 것 외에도이 API 수락 "모든" 유효한 값으로 합니다. 이렇게 하면 각 필드를 지정할 수 있는 값을 생성 됩니다. "All" 값은 개발, 디버깅 및 테스트 용도로 사용할 수 있습니다.

<a id="ID4ERG"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4ETG"></a>


##### <a name="parent"></a>부모  

[추가 참조](atoc-xboxlivews-reference-additional.md)


<a id="ID4E6G"></a>


##### <a name="further-information"></a>자세한 정보

[마켓플레이스 URI](../uri/marketplace/atoc-reference-marketplace.md)
