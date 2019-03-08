---
title: 보조 EDS API
assetID: 5729ab80-e88d-0190-fb61-bd0cc4f134f6
permalink: en-us/docs/xboxlive/rest/eds-apis.html
description: " 보조 EDS API"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3f2f729359f52b09879e7227ede71e238fe63801
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603238"
---
# <a name="auxiliary-eds-apis"></a>보조 EDS API

여러 엔터테인먼트 검색 서비스 (EDS) Api, 콘텐츠에 대 한 정보를 직접 제공 하지 않습니다 하지만 드라이브 일반적인 UI 모델 도움말 또는 서비스를 사용 하는 방법에 대 한 일반 정보를 제공 하는 경우

<a id="ID4EQ"></a>


## <a name="auxiliary-apis"></a>보조 Api

| API| URI| 설명|
| --- | --- | --- |
| API 매개 변수 값| /{locale}/metadata| 서비스에 대 한 호출에서 사용할 수 있는 매개 변수의 가능한 값의 열거형|
| 콘텐츠 등급 생성기를 결합합니다.| /{locale}/contentRating| 잠재적으로 유해한 또는 명시적 콘텐츠를 필터링 하는 것에 대 한 다른 Api에 사용할 수 있는 값을 만듭니다. 자세한 내용은 아래를 참조 하세요.|
| 결합 된 필드 이름 생성기| /{locale}/fields| 어떤 필드가 반환될지는 컨트롤에 세부 정보 Api에서에서 사용할 수 있는 값을 만듭니다. 자세한 내용은 아래를 참조 하세요.|

<a id="ID4EBC"></a>


### <a name="api-parameter-values"></a>API 매개 변수 값

이 API는 서비스를 사용 하 여 사용할 수 있는 매개 변수를 설명 합니다. 반환 된 정보를 클라이언트 UI에서 사용할 이므로 지역화 된 텍스트와 각 매개 변수를 함께 제공 합니다.

아래 Api 하나도 쿼리 매개 변수를 허용 합니다.

| API| URI| 설명|
| --- | --- | --- | --- | --- | --- |
| 형식| /{locale}/metadata/mediaGroups| 미디어 그룹의 전체 목록|
| 미디어 항목 미디어 그룹당 형식| /{locale}/metadata/mediaGroups/{mediaItemTypeGroup}/mediaItemTypes| 미디어의 목록 항목의 지정 된 미디어 그룹 내에 포함 된 형식입니다.|
| 모든 미디어 항목 형식| /{locale}/metadata/mediaItemTypes| 미디어 항목 종류의 전체 목록|
| 각 미디어 항목 형식 필드| /{locale}/metadata/mediaItemTypes/{mediaItemType}/fields| 단일 미디어 항목 형식에서 필드 목록|
| 쿼리 구체화| /{locale}/metadata/mediaItemTypes/{mediaItemType}/queryRefiners| 지정 된 미디어 항목 유형에 대해 지원 되는 쿼리 구체화의 목록|
| 모든 쿼리 구체화 값| /{locale}/metadata/mediaItemTypes/{mediaItemType}/queryRefiners/{queryRefiner}| 지정 된 미디어에 대 한 지정 된 쿼리 구체화 값 항목 형식|
| 모든 쿼리 구체화 하위 값| /{locale}/metadata/mediaItemTypes/{mediaItemType}/queryRefiners/{queryRefiner}/subQueryRefinerValues| 지정 된 쿼리 구체화 값 (예: "subgenres 지정 장르의")에 대 한 하위 값의 목록입니다. 쿼리 구체화 값에 "queryRefinerValue" 전달할 URI 형태소에서 사용할 수 없는 문자를 사용 하 여 쿼리 구체화 값을 허용 하기 위해 수행 됩니다 명명 된 쿼리 문자열 매개 변수로 전달 됩니다.|
| 정렬 합니다.| /{locale}/metadata/mediaItemTypes/{mediaItemType}/sortOrders| 지정 된 미디어 항목 형식에 대 한 정렬 순서 목록|

<a id="ID4EEF"></a>


### <a name="combined-content-rating-generator"></a>콘텐츠 등급 생성기를 결합합니다.

자식을 볼 수 있는 콘텐츠에 대해 자녀를 적용 하는 것은 복잡 한 작업입니다. 각 미디어 항목 유형 자체 등급 시스템이 있습니까 할 뿐만 아니라 이러한 등급 시스템 국가 마다 다를 수 있습니다. 이 제대로 모든 항목을 필터링 하도록 지정 해야 하는 데이터의 일부의 조각은 것을 의미 합니다.

모든 매개 변수를 모든 API 호출에서 지정 하는 대신이 API 다른 api에서 combinedContentRating 매개 변수로 전달 하 고 여전히과 동일한 정보를 전달 하는 값을 생성 합니다. 이 Api를 쉽게 수행할 수 있도록 사용 및 유지 관리 하 고,이 API에 전달 하는 여러 매개 변수는 다른 Api에 대 한 단일, 다시 사용할 수 있는 값으로 축소 된 형태로 설계 되었습니다.

자주 변경 해야이 API에서 반환 된 정확한 값을 최종적으로 변경 될 수 있지만 (예: EDS의 릴리스 사이) 따라서 오랜 시간 동안 캐시 될 수 없습니다. CombinedContentRating 매개 변수에서 의미 있는 오류 메시지가 표시 전달 된 값을 유효 하지 않은 경우 표시 되는 호출자에 게 단순히 수락 하는 모든 API는 업데이트 된 값을 가져오려고 다시이 API를 호출 해야 합니다. API combinedContentRating 매개 변수를 허용 하지만 입력 하지 않은 경우 콘텐츠 필터링을 하지 않고 수행 됩니다 자녀 기반

> [!NOTE]
> "안전한"만 콘텐츠는 반환 된-잠재적 성인 등급 콘텐츠를 포함 하 여 모든 콘텐츠가 반환 됩니다 의미).



<a id="ID4EWF"></a>


### <a name="combined-field-name"></a>결합 된 필드 이름

EDS Api를 기본적으로 각 항목에 대 한 필드의 매우 작은 최소 집합을 반환 합니다.

   * 미디어 항목 유형
   * 미디어 그룹
   * Id
   * 이름

자세한 정보를 얻으려면 Api는 추가 데이터 부분을 반환할지를 지정 하는 "필드" 매개 변수를 허용 합니다. 여러 가능한 필드 이기 때문에 각 API 호출에 대 한 전체에 해당 이름을 지정 하는 크게 블 로트 요청 합니다. 대신 이름은 다른 Api에 전달할 수 있는 훨씬 더 작은 값을 생성 하는이 API에 전달할 수 있습니다.

제공 된 값이 매개이 변수를 허용 하는 모든 API에 대 한 모든 지정 된 미디어 항목 형식에 모든 필드의 상위 집합 이어야 합니다. 다양 한 미디어 항목 형식에 대 한 필드의 다른 집합을 지정 하는 것이 불가능 합니다. 그러나 필드 하나 이상의 미디어 항목 형식을 다른을 제외한이 적용 되는 경우에 나타나며 미디어 항목 형식 데이터가 존재 하는 (예: "AvatarBodyType" 결합 필드 이름 API에 대 한 호출에 포함 되어만 AvatarItems 필드가 포함 됩니다).

이 API에서 반환 되는 값은 실제로-캐시가, EDS의 배포 간에 제외 하 고 변경 되지 않아야 합니다. 캐싱 필요한 경우 캐시 마지막 사용자의 세션 보다 길지 않은 것이 좋습니다.

실제 필드 이름이 수락 하는 것 외에도이 API가 허용 "all"을 유효한 값으로. 이 지정 하는 것이 불가능 각 필드를 포함 하는 값을 생성 됩니다. "All" 값은 개발, 디버깅 및 테스트 목적에만 사용할 수 있습니다.

<a id="ID4ERG"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4ETG"></a>


##### <a name="parent"></a>Parent  

[추가 참조](atoc-xboxlivews-reference-additional.md)


<a id="ID4E6G"></a>


##### <a name="further-information"></a>추가 정보

[Marketplace Uri](../uri/marketplace/atoc-reference-marketplace.md)
