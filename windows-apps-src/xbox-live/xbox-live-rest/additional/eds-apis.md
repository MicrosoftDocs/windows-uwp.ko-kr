---
title: 보조 EDS API
assetID: 5729ab80-e88d-0190-fb61-bd0cc4f134f6
permalink: en-us/docs/xboxlive/rest/eds-apis.html
author: KevinAsgari
description: " 보조 EDS API"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ecdf3d885a518f622dae00b4b4a98979c3bdefe9
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2018
ms.locfileid: "4469976"
---
# <a name="auxiliary-eds-apis"></a>보조 EDS API

여러 엔터테인먼트 검색 서비스 (EDS) 직접 내용에 대 한 정보를 제공 하지 하지만 일반적인 UI 모델 드라이브 또는 서비스를 사용 하는 방법에 대 한 일반 정보를 제공 하는 Api 있습니다.

<a id="ID4EQ"></a>


## <a name="auxiliary-apis"></a>보조 Api

| API| URI| 설명|
| --- | --- | --- |
| API 매개 변수 값| / {로캘} / 메타 데이터| 가능한 값의 서비스에 대 한 호출에 사용할 수 있는 매개 변수는 열거형|
| 콘텐츠 등급 생성기를 결합합니다.| / {로캘} / contentRating| 잠재적으로 유해한 또는 명시적 콘텐츠 필터링에 대 한 다른 Api에 사용할 수 있는 값을 만듭니다. 자세한 내용은 아래를 참조 하세요.|
| 결합 된 필드 이름 생성기| / {로캘} /fields| 필드에 반환 되는 컨트롤에 세부 정보 Api에에서 사용할 수 있는 값을 만듭니다. 자세한 내용은 아래를 참조 하세요.|

<a id="ID4EBC"></a>


### <a name="api-parameter-values"></a>API 매개 변수 값

이 API 서비스와 함께 사용할 수 있는 매개 변수를 설명 합니다. 반환 된 정보는 지역화 된 텍스트와 각 매개 변수가 함께 제공 하기 때문에 UI 클라이언트에서 사용할 수 있습니다.

아래 Api 중 쿼리 매개 변수를 수락합니다.

| API| URI| 설명|
| --- | --- | --- | --- | --- | --- |
| 형식| / {로캘} / 메타 데이터/mediaGroups| 미디어 그룹의 전체 목록|
| 미디어 항목 미디어 그룹당 형식| / {로캘} / /metadata/mediagroups/ {mediaItemTypeGroup} / mediaItemTypes| 미디어의 목록 항목 주어진된 미디어 그룹에 포함 된 형식.|
| 모든 미디어 항목 유형| / {로캘} / 메타 데이터/mediaItemTypes| 미디어 항목 종류의 전체 목록|
| 미디어 항목 종류에 따라 필드| / {로캘} / /metadata/mediaitemtypes/ {mediaItemType} /fields| 단일 미디어 항목 형식에는 필드 목록|
| 쿼리 구체화| / {로캘} / /metadata/mediaitemtypes/ {mediaItemType} / queryRefiners| 지정 된 미디어 항목 형식에 대해 지원 되는 쿼리 구체화 목록|
| 모든 쿼리 구체화 값| / {로캘} / 메타 데이터/mediaItemTypes / {mediaItemType} {queryRefiner} /queryRefiners/| 항목 유형 지정 된 미디어에 대 한 지정 된 쿼리 구체화에 대 한 값|
| 모든 쿼리 구체화 하위 값| / {로캘} / 메타 데이터/mediaItemTypes / {mediaItemType} {queryRefiner} /queryRefiners/ / subQueryRefinerValues| 지정 된 쿼리 구체화 값 (예: "subgenres 주어진 장르의")에 대 한 하위 값의 목록입니다. 쿼리 구체화 값에 전달 될 URI 획에서 금지 문자로 쿼리 구체화 값을 허용 하도록 완료 되는 "queryRefinerValue" 라는 쿼리 문자열 매개 변수로 전달 됩니다.|
| 정렬 합니다.| / {로캘} / /metadata/mediaitemtypes/ {mediaItemType} / sortOrders| 정렬 순서 지정 된 미디어 항목 형식에 대 한 목록|

<a id="ID4EEF"></a>


### <a name="combined-content-rating-generator"></a>콘텐츠 등급 생성기를 결합합니다.

자식 볼 수 있는 콘텐츠 위에 자녀 보호를 적용 하는 한편 복잡 한 작업입니다. 각 미디어 항목 형식에는 자체 등급 시스템 할 뿐만 아니라 이러한 등급 시스템 국가 마다 다를 수 있습니다. 즉, 데이터를 제대로 모든 항목을 필터링 하려면 지정 해야 하는 다른 여러 가지 있습니다.

모든 매개 변수를 모든 API 호출에 지정 하는 대신이 API의 다른 Api combinedContentRating 매개 변수를 전달 하 고 여전히 동일한 정보를 전달 하기 위한 값을 생성 합니다. 이 쉽게 Api를 사용 하 고 유지 하 고,이 API에 전달 된 여러 매개 변수는 다른 Api에 대 한 단일, 재사용 가능한 값으로 축소 된 대로 설계 되었습니다.

이 API에서 반환 되는 정확한 값 결국 변경 될 수 있지만 해야 바뀌는 매우 드물게 (예: EDS 릴리스의) 따라서 오랜 시간 동안 캐시 될 수 있습니다. CombinedContentRating 매개 변수에서 의미 있는 오류 메시지가 표시에 전달 된 값이 잘못 된 경우 표시 되는 호출자 단순히 수락 모든 API에서이 API는 업데이트 된 값을 가져오려면를 다시 호출 해야 합니다. API 수락 combinedContentRating 매개 변수를 입력 하지 않은 경우, 콘텐츠 필터링 없이 적용 됩니다 자녀 기반

> [!NOTE]
> 그렇다고 해당 콘텐츠를 "안전한"만 반환 됩니다-의미 명시적 잠재적으로 콘텐츠를 포함 한 모든 콘텐츠 반환 됩니다).



<a id="ID4EWF"></a>


### <a name="combined-field-name"></a>결합 된 필드 이름

EDS Api, 기본적으로 각 항목에 대 한 필드 집합이 매우 작은 최소 반환합니다.

   * 미디어 항목 유형
   * 미디어 그룹
   * Id
   * 이름

자세한 정보를 가져오려면 Api는 데이터의 어떤 추가 조각 반환할지를 지정 하는 "필드" 매개 변수를 수락 합니다. 많은 가능한 필드를 각 API 호출에 대 한 전체에 자신의 이름을 지정 하는 요청 크게 볼록 합니다. 대신, 이름은 다른 Api로 전달 될 수 있는 훨씬 더 작은 값을 생성할이 API에 전달할 수 있습니다.

이 매개 변수를 허용 하는 모든 API를 제공된 된 값에는 모든 지정 된 미디어 항목 종류의 모든 필드의 상위 집합 해야 합니다. 서로 다른 미디어 항목 형식에 대 한 필드를 지정 하는 것이 불가능 합니다. 그러나 필드 하나 이상의 미디어 항목 유형 다른을 제외한이 적용 되는 경우에 표시 됩니다 미디어 항목 형식 데이터 존재 하는 위치 (예: "AvatarBodyType" 필드 이름 결합 API에 대 한 호출에 포함 되어만 AvatarItems 필드를 포함 됩니다).

이 API에서 반환 된 값은 매우 캐시할-사실, EDS의 배포 사이 제외 하 고 변경 되지 않아야 합니다. 필요한 캐싱의 경우 캐시 마지막 사용자의 세션 보다 더 이상 것이 좋습니다.

실제 필드 이름, 수락 하는 것 외에도이 API가 허용 "모든" 유효한 값으로 합니다. 이렇게 하면 각 필드를 지정할 수 있는 값을 생성 됩니다. "모든" 값은 개발, 디버깅 및 테스트 용도로 사용할 수 있습니다.

<a id="ID4ERG"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4ETG"></a>


##### <a name="parent"></a>부모  

[추가 참조](atoc-xboxlivews-reference-additional.md)


<a id="ID4E6G"></a>


##### <a name="further-information"></a>자세한 정보

[마켓플레이스 URI](../uri/marketplace/atoc-reference-marketplace.md)
