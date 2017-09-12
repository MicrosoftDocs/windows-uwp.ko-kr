---
author: normesta
description: "지도 스타일 시트의 항목 및 속성"
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "지도 스타일 시트 참조"
ms.author: normesta
ms.date: 03/16/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 지도, 지도 스타일 시트"
ms.openlocfilehash: 9e2605917027d7be96ac86421e83082aa5f368f9
ms.sourcegitcommit: 23cda44f10059bcaef38ae73fd1d7c8b8330c95e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/19/2017
---
# <a name="map-style-sheet-reference"></a>지도 스타일 시트 참조

JSON(JavaScript Object Notation)을 사용하여 지도 스타일 시트를 만들 수 있습니다.

예를 들어, 물 영역을 빨간색으로, 물 레이블을 초록색으로, 육지 영역을 파란색으로 표시하려면 다음 JSON을 사용할 수 있습니다.

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000", "labelColor":"#00FF00"}}
    }
```
JSON을 사용하여 지도에서 모든 레이블과 포인트를 제거할 수도 있습니다.

```json

    {"version":"1.*", "elements":{"mapElement":{"labelVisible":false},"point":{"visible":false}}}
```

때때로 최종 결과를 위해 속성 값을 변환해야 합니다.  예를 들어, 초목 fillColor에 표시되는 음영이 표시될 대상의 종류에 따라 달라집니다.  정확한 값을 사용하고 ignoreTransform 속성을 사용하여 이런 동작을 끌 수 있습니다.

```json
    {"version":"1.*",
        "settings":{"shadedReliefVisible":false},
        "elements":{"vegetation":{"fillColor":{"value":"#999999","ignoreTransform":true}}}
    }
```

이 항목에서는 지도의 모양과 느낌을 사용자 지정할 수 있는 JSON 항목 및 [속성](#properties)을 보여 줍니다.

<span id="entries" />
## <a name="entries"></a>항목
이 표는 ">" 문자를 사용하여 입력 계층의 수준을 나타냅니다.   

| 이름                         | 속성 그룹            | 설명    |
|------------------------------|---------------------------|----------------|
| 버전                      | [버전](#version)       | 사용하고자 하는 스타일 시트 버전입니다. |
| 설정                     | [설정](#settings)     | 전체 스타일 시트에 적용되는 설정입니다. |
| mapElement                   | [MapElement](#mapelement) | 모든 지도 항목의 상위 항목입니다. |
| > baseMapElement             | [MapElement](#mapelement) | 사용자가 아닌 모든 항목의 상위 항목입니다. |
| >> area                      | [MapElement](#mapelement) | 육지 사용 영역입니다(구조 항목과 혼동하지 마십시오). |
| >>> airport                  | [MapElement](#mapelement) | 공항을 포괄하는 영역입니다. |
| >>> cemetery                 | [MapElement](#mapelement) | 묘지 영역입니다. |
| >>> continent                | [MapElement](#mapelement) | 전체 대륙의 영역입니다. |
| >>> education                | [MapElement](#mapelement) |  |
| >>> indigenousPeoplesReserve | [MapElement](#mapelement) |  |
| >>> island                   | [MapElement](#mapelement) | 섬 영역의 레이블입니다. |
| >>> medical                  | [MapElement](#mapelement) | 의료 목적을 위해 사용되는 지역 영역입니다(예: 병원 캠퍼스). |
| >>> military                 | [MapElement](#mapelement) | 군사 기지 영역입니다. |
| >>> nautical                 | [MapElement](#mapelement) | 항해 목적을 위해 사용되는 육지 영역입니다. |
| >>> neighborhood             | [MapElement](#mapelement) | 인근으로 정의된 영역의 레이블입니다. |
| >>> runway                   | [MapElement](#mapelement) | 활주로를 포함하는 육지 영역입니다. |
| >>> sand                     | [MapElement](#mapelement) | 해변과 같은 모래 영역입니다. |
| >>> shoppingCenter           | [MapElement](#mapelement) | 몰 또는 기타 쇼핑 센터에 대한 땅 영역입니다. |
| >>> stadium                  | [MapElement](#mapelement) | 경기장 영역입니다. |
| >>> vegetation               | [MapElement](#mapelement) | 숲, 풀 영역 등입니다. |
| >>>> forest                  | [MapElement](#mapelement) | 숲 지역입니다. |
| >>>> golfCourse              | [MapElement](#mapelement) |  |
| >>>> park                    | [MapElement](#mapelement) | 공원 영역입니다. |
| >>>> playingField            | [MapElement](#mapelement) | 야구장 또는 테니스 코트와 같은 추출된 피치입니다. |
| >>>> reserve                 | [MapElement](#mapelement) | 자연보호구역 영역입니다. |
| >> point                     | [PointStyle](#pointstyle) | 일종의 아이콘으로 렌더링되는 모든 포인트 기능입니다. |
| >>> naturalPoint             | [PointStyle](#pointstyle) |  |
| >>>> peak                    | [PointStyle](#pointstyle) | 산 봉우리를 나타내는 아이콘입니다. |
| >>>>> volcanicPeak           | [PointStyle](#pointstyle) | 화산 봉우리를 나타내는 아이콘입니다. |
| >>>> waterPoint              | [PointStyle](#pointstyle) | 폭포와 같은 물 기능 위치를 나타내는 아이콘입니다. |
| >>> pointOfInterest          | [PointStyle](#pointstyle) | 음식점, 병원, 학교, 선착장, 스키장 등입니다. |
| >>>> business                | [PointStyle](#pointstyle) | 음식점, 병원, 학교 등. |
| >>>>> foodPoint              | [PointStyle](#pointstyle) | 음식점, 카페 등. |
| >>> populatedPlace           | [PointStyle](#pointstyle) | 밀도가 높은 곳의 크기를 나타내는 아이콘(예: 도시 또는 타운). |
| >>>> capital                 | [PointStyle](#pointstyle) | 밀도가 높은 장소의 수도를 나타내는 아이콘. |
| >>>>> adminDistrictCapital   | [PointStyle](#pointstyle) | 시/도의 수도를 나타내는 아이콘. |
| >>>>> countryRegionCapital   | [PointStyle](#pointstyle) | 국가 또는 지역 대문자 나타내는 아이콘이 있습니다. |
| >>> roadShield               | [PointStyle](#pointstyle) | 도로의 간단한 이름을 나타내는 기호. 예: I-5. 설정 항목의 **ImageFamily** 속성을 *색상표* 값으로 설정하는 경우 색상표 값만 사용 |
| >>> roadExit                 | [PointStyle](#pointstyle) | 일반적으로 진입 통제 고속도로의 비상구를 나타내는 아이콘. |
| >>> transit                  | [PointStyle](#pointstyle) | 버스 정류장, 기차 정차역, 공항 등을 나타내는 아이콘. |
| >> political                 | [BorderedMapElement](#borderedmapelement) | 국가, 지역 및 주와 같은 정치적 지역. |
| >>> countryRegion            | [BorderedMapElement](#borderedmapelement) |  |
| >>> adminDistrict            | [BorderedMapElement](#borderedmapelement) | Admin1, 주, 도 등. |
| >>> district                 | [BorderedMapElement](#borderedmapelement) | Admin2, 국가 등. |
| >> structure                 | [MapElement](#mapelement) | 건물 및 기타 건물 같은 구조. |
| >>> building                 | [MapElement](#mapelement) |  |
| >>>> educationBuilding       | [MapElement](#mapelement) |  |
| >>>> medicalBuilding         | [MapElement](#mapelement) |  |
| >>>> transitBuilding         | [MapElement](#mapelement) |  |
| >> transportation            | [TwoToneLineStyle](#twotonelinestyle) | 교통망의 일부인 줄(예: 도로, 기차, 및 여객선). |
| >>> road                     | [TwoToneLineStyle](#twotonelinestyle) | 모든 도로를 표시하는 줄. |
| >>>> controlledAccessHighway | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>>> highSpeedRamp          | [TwoToneLineStyle](#twotonelinestyle) | 경사로를 나타내는 줄. 일반적으로 이 경사로는 진입 통제 고속도로와 함께 나타납니다. |
| >>>> highway                 | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>> majorRoad               | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>> arterialRoad            | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>> street                  | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>>> ramp                   | [TwoToneLineStyle](#twotonelinestyle) | 고속도로의 입구 및 출구를 나타내는 줄. |
| >>>>> unpavedStreet          | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>> tollRoad                | [TwoToneLineStyle](#twotonelinestyle) | 비용을 지불해야 하는 도로. |
| >>> railway                  | [TwoToneLineStyle](#twotonelinestyle) | 철로길. |
| >>> trail                    | [TwoToneLineStyle](#twotonelinestyle) | 공원 또는 자국 하이킹 코스 산책로. |
| >>> waterRoute               | [TwoToneLineStyle](#twotonelinestyle) | 페리 경로 선. |
| >> water                     | [MapElement](#mapelement) | 물처럼 보이는 모든 것. 바다 및 물결이 포함됩니다. |
| >>> river                    | [MapElement](#mapelement) | 강, 물결 또는 기타 수로.  선 또는 다각형일 수 있으며 강이 아닌 수공간에 연결될 수 있습니다. |
| > routeMapElement            | [MapElement](#mapelement) | 모든 라우팅 항목은 이 항목의 아래 항목입니다. |
| >> routeLine                 | [TwoToneLineStyle](#twotonelinestyle) | 모든 경로 선에 대한 스타일. |
| >>> walkingRoute             | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>> drivingRoute             | [TwoToneLineStyle](#twotonelinestyle) |  |
| > userMapElement             | [MapElement](#mapelement) | 모든 사용자 항목은 이 항목의 아래 항목입니다. |
| >> userPoint                 | [PointStyle](#pointstyle) | 기본 사용자 위치에 대한 스타일. |
| >> userLine                  | [MapElement](#mapelement) | 기본 사용자 선에 대한 스타일. |

<span id="properties" />
## <a name="properties"></a>속성

이 섹션은 각 항목에 사용할 수 있는 속성을 설명합니다.

<span id="version" />
### <a name="version-properties"></a>버전 속성

| 속성                     | 형식    | 설명                                                                                                           |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------|
| 버전                      | 문자열  | 대상 스타일 시트 버전입니다. 적용을 위해 사용됩니다. "1.0"이 기본값이며 기능 업데이트에 대해 "1.*"입니다. |

<span id="settings" />
### <a name="settings-properties"></a>설정 속성

| 속성                     | 형식    | 설명                                                                                                                                                                                                                 |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| atmosphereVisible            | 부울    | 3D 컨트롤에 대기를 표시할지 여부를 나타내는 플래그.                                                                                                                                                     |
| fogColor                     | 색상   | 3D 컨트롤에 나타나는 거리 안개의 ARGB색 값.                                                                                                                                                    |
| glowColor                    | 색상   | 레이블 빛 및 아이콘 빛에 적용할 수 있는 ARGB색 값.                                                                                                                                                     |
| imageFamily                  | 문자열  | 이 스타일을 사용하도록 설정하는 이미지 세트의 이름입니다. 현실 세계의 기호에 따라 고정된 색상을 사용하는 기호의 경우 이 값을 *기본*으로 설정합니다. 색상표를 구성 가능한 색을 사용하는 기호의 경우 이 값을 *색상표*로 설정합니다. |
| landColor                    | 색상   | 육지에 아무 것도 그리기 전의 육지의 ARGB 색 값.                                                                                                                                                     |
| logosVisible                 | 부울    | **조직** 속성을 가진 항목이 적절한 로고를 그리거나 일반 아이콘을 사용해야 하는지를 나타내는 플래그.                                                                                         |
| officialColorVisible         | 부울    | 공식 색 속성(예: 중국의 교통 라인)을 가진 항목이 해당 색을 그려야 하는지 여부를 나타내는 플래그. 예를 들어 흑백 지도의 경우 이 값을 해제합니다.                               |
| rasterRegionsVisible         | 부울    | 벡터로 렌더링하지 않는 래스터 지역(예: 일본 및 한국)을 그릴 것인지 여부를 나타내는 플래그.                                                                                                |
| shadedReliefVisible          | 부울    | 지도에 점진 음영을 그릴지 여부를 나타내는 플래그.                                                                                                                                                  |
| shadedReliefDarkColor        | 색상   | 음영기복의 어두운 면의 색상.  알파 채널은 최대 알파를 나타냅니다. value.                                                                                                                            |
| shadedReliefLightColor       | 색상   | 음영기복의 밝은 면의 색상.  알파 채널은 최대 알파를 나타냅니다. value.                                                                                                                           |
| spaceColor                   | 색상   | 지도 주위의 영역에 대한 ARGB 색 값.                                                                                                                                                                               |
| useDefaultImageColors        | 부울    | 이미지에서 색상에 대한 색상표 항목을 검색하는 것이 아니라 SVG의 원색이 사용되어야 함을 나타내는 플래그.                                                                                |

<span id="mapelement" />
### <a name="mapelement-properties"></a>MapElement 요소

| 속성                     | 형식    | 설명                                                                                                                 |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------------|
| fillColor                    | 색상   | 나눠진 경우 다각형, 포인트 아이콘의 배경, 선의 중앙을 채우는 데 사용되는 색. |
| fontFamily                   | 문자열  |                                                                                                                             |
| labelColor                   | 색상   |                                                                                                                             |
| labelOutlineColor            | 색상   |                                                                                                                             |
| labelScale                   | 부동   | 기본 레이블 크기가 조정되는 양. 예를 들어 기본값에 *1*, 대형의 두 배값에 *2*를 사용합니다.            |
| labelVisible                 | 부울    |                                                                                                                             |
| strokeColor                  | 색상   | 다각형 주위의 윤곽선, 포인트 아이콘의 윤곽선, 선의 색에 사용할 색.                   |
| strokeWidthScale             | 부동   | 선의 스트로크의 크기를 조정하는 정도. 예를 들어 기본값에 *1*, 대형의 두 배값에 *2*를 사용합니다.            |
| 표시                      | 부울    |                                                                                                                             |

<span id="borderedmap" />
### <a name="borderedmapelement"></a>BorderedMapElement

이 속성 그룹은 [MapElement](#mapelement) 속성 그룹에서 상속됩니다.

| 속성                     | 형식    | 설명                                                           |
|------------------------------|---------|-----------------------------------------------------------------------|
| borderOutlineColor           | 색상   | 채워진 다각형의 두 번째 또는 채워진 테두리 선의 색. |
| borderStrokeColor            | 색상   | 채워진 다각형의 테두리의 기본 선 색상.             |
| borderVisible                | 부울    |                                                                       |
| borderWidthScale             | 부동   |                                                                       |

<span id="pointstyle" />
### <a name="pointstyle-properties"></a>PointStyle 속성

이 속성 그룹은 [MapElement](#mapelement) 속성 그룹에서 상속됩니다.

| 속성                     | 형식    | 설명                                                                                                        |
|------------------------------|---------|--------------------------------------------------------------------------------------------------------------------|
| iconColor                    | 색상   | 포인트 아이콘의 중앙에 표시되는 문자 모양의 색.                                                        |
| scale                        | 부동   | 전체 포인트의 크기를 조정하는 정도. 예를 들어 기본값에 *1*, 대형의 두 배값에 *2*를 사용합니다. |
| stemColor                    | 색상   | 3D 모드에서 해당 아이콘의 하단에서 나오는 줄기의 색.                                             |
| stemOutlineColor             | 색상   | 3D 모드에서 해당 아이콘의 하단에서 나오는 줄기 주변의 윤곽선 색.                          |

<span id="twotonelinestyle" />
### <a name="twotonelinestyle-properties"></a>TwoToneLineStyle 속성

이 속성 그룹은 [MapElement](#mapelement) 속성 그룹에서 상속됩니다.

| 속성                     | 형식    | 설명                                                                                          |
|------------------------------|---------|------------------------------------------------------------------------------------------------------|
| overwriteColor               | 부울    |  **FillColor**의 알파 값은 **StrokeColor**와 혼합되는 것이 아니라 이를 덮어씁니다. |
