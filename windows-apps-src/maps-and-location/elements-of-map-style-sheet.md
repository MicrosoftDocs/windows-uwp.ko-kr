---
author: normesta
description: 지도 스타일 시트의 항목 및 속성
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 지도 스타일 시트 참조
ms.author: normesta
ms.date: 03/16/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 지도, 지도 스타일 시트
ms.localizationpriority: medium
ms.openlocfilehash: 11360f9d76fc07d7a6b24bd1e0bfb78df4f1d22d
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4313605"
---
# <a name="map-style-sheet-reference"></a>지도 스타일 시트 참조

Microsoft 매핑 기술 지도의 모양을 정의 하 여 _지도 스타일 시트_ 를 사용 합니다.  지도 스타일 시트 JSON JavaScript Object Notation ()를 사용 하 여 정의 되 고 다양 한 방법으로 포함 하 여 Windows 스토어 응용 프로그램의 [MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol) 에 [MapStyleSheet.ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_) 메서드를 통해 사용할 수 있습니다.

[지도 스타일 시트 편집기](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) 응용 프로그램을 사용 하 여 대화형으로 스타일 시트를 만들 수 있습니다.

다음 JSON 빨간색에서 물 영역을 사용할 수, 물 레이블을 초록색으로, 표시 및 육지 영역을 파란색으로 표시 합니다.

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

지도에서 모든 레이블과 포인트를 제거 하려면이 JSON은 사용할 수 있습니다.

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

<a id="entries" />

## <a name="entries"></a>항목
이 표는 ">" 문자를 사용하여 입력 계층의 수준을 나타냅니다.  또한 어떤 버전의 Windows 각 항목을 지원 하며,이 무시 하는 보여 줍니다.

| 빌드 | Windows 릴리스 이름 |
|-------|----------------------|
| 1506  | 크리에이터스 업데이트      |
| 1629  | Fall Creators Update |
| 1713  | 2018년 4월 업데이트    |

| 이름                         | 속성 그룹            | 1506 | 1629 | 1713 | Next | 설명    |
|------------------------------|---------------------------|------|------|------|------|----------------|
| 버전                      | [버전](#version)       |  ✔   |  ✔   |  ✔   |  ✔   | 사용하고자 하는 스타일 시트 버전입니다. |
| 설정                     | [설정](#settings)     |  ✔   |  ✔   |  ✔   |  ✔   | 전체 스타일 시트에 적용되는 설정입니다. |
| mapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 모든 지도 항목의 상위 항목입니다. |
| > baseMapElement             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 사용자가 아닌 모든 항목의 상위 항목입니다. |
| >> area                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 설명 하는 육지 영역을 사용 합니다.  이러한 구조 항목의 아래에 실제 건물와 혼동 해서는 안됩니다 해야 합니다. |
| >>> airport                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 공항 포괄 하는 영역입니다. |
| >>> areaOfInterest           | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 비즈니스 또는 흥미로운 포인트가 집중된 영역입니다. |
| >>> cemetery                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 묘지 포괄 하는 영역입니다. |
| >>> continent                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 대륙 영역 레이블입니다. |
| >>> education                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 학교 및 교육 다른 기능을 포함 하는 영역입니다. |
| >>> indigenousPeoplesReserve | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 토착 개인적 예약을 포괄 하는 영역입니다. |
| >>> industrial               | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 산업 목적에 사용 되는 영역입니다. |
| >>> island                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 섬 영역의 레이블입니다. |
| >>> medical                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 의료 목적을 위해 사용 되는 영역 (예: 병원 캠퍼스). |
| >>> military                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 군사 사용 하거나 포괄할 군사 기지 영역입니다. |
| >>> nautical                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 관련 항해 목적에 사용 되는 영역입니다. |
| >>> neighborhood             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 주변 영역 레이블입니다. |
| >>> runway                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 비행기 활주로로 사용 되는 영역입니다. |
| >>> sand                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 해변과 같은 모래 영역입니다. |
| >>> shoppingCenter           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 몰 또는 기타 쇼핑 센터에 대한 땅 영역입니다. |
| >>> stadium                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 경기장 포괄 하는 영역입니다. |
| >>> underground              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 지하 영역(예: 지하철역). |
| >>> vegetation               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 숲, 풀 영역 등입니다. |
| >>>> forest                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 숲 지역입니다. |
| >>>> golfCourse              | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 골프 과정을 포괄 하는 영역입니다. |
| >>>> park                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 공원 포괄 하는 영역입니다. |
| >>>> playingField            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 야구장 또는 테니스 코트와 같은 추출된 피치입니다. |
| >>>> reserve                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 특성을 포괄 하는 영역을 예약 합니다. |
| >> point                     | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 모든 포인트 기능 일종의 아이콘을 사용 하 여 그려집니다. |
| >>> address                  | [PointStyle](#pointstyle) |      |      |  ✔   |  ✔   | 숫자 레이블을 주소입니다. |
| >>> naturalPoint             | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 자연 스러운 기능을 나타내는 아이콘. |
| >>>> peak                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 산 봉우리를 나타내는 아이콘입니다. |
| >>>>> volcanicPeak           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 화산 봉우리를 나타내는 아이콘입니다. |
| >>>> waterPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 폭포와 같은 물 기능 위치를 나타내는 아이콘입니다. |
| >>> pointOfInterest          | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 관심 있는 모든 위치를 나타내는 아이콘. |
| >>>> business                | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 모든 비즈니스 locaiton를 나타내는 아이콘. |
| >>>>> attractionPoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 박물관, zoos 등과 같은 여행자 명소를 나타내는 아이콘. |
| >>>>> communityPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 커뮤니티에 일반적으로 사용의 위치를 나타내는 아이콘. |
| >>>>> educationPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 학교 및 기타 교육을 나타내는 아이콘 관련 위치입니다. |
| >>>>> entertainmentPoint     | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 극장, 시네마 및 등과 같은 엔터테인먼트 장소를 나타내는 아이콘. |
| >>>>> essentialServicePoint  | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 주차, 은행, 사례를 살펴보자면 등과 같은 필수 서비스를 나타내는 아이콘. |
| >>>>> foodPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 음식점, 카페 등을 나타내는 아이콘. |
| >>>>> lodgingPoint           | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 호텔 및 기타 숙박 회사를 나타내는 아이콘. |
| >>>>> realEstatePoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 부동산 비즈니스를 나타내는 아이콘. |
| >>>>> shoppingPoint          | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 호텔 및 기타 숙박 회사를 나타내는 아이콘. |
| >>> populatedPlace           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 밀도가 높은 곳의 크기를 나타내는 아이콘(예: 도시 또는 타운). |
| >>>> capital                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 밀도가 높은 장소의 수도를 나타내는 아이콘. |
| >>>>> adminDistrictCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 시/도의 수도를 나타내는 아이콘. |
| >>>>> countryRegionCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 국가 또는 지역 대문자 나타내는 아이콘이 있습니다. |
| >>> roadShield               | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 도로의 간단한 이름을 나타내는 기호. 예: I-5. 설정 항목의 **ImageFamily** 속성을 *색상표* 값으로 설정하는 경우 색상표 값만 사용 |
| >>> roadExit                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 일반적으로 진입 통제 고속도로의 비상구를 나타내는 아이콘. |
| >>> transit                  | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 버스 정류장, 기차 정차역, 공항 등을 나타내는 아이콘. |
| >> political                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 국가, 지역 및 주와 같은 정치적 지역. |
| >>> countryRegion            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 국가 지역 테두리 및 레이블을 합니다. |
| >>> adminDistrict            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin1, 상태, 주 등 테두리 및 레이블을. |
| >>> district                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin2, 국가 등 테두리 및 레이블을. |
| >> structure                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 건물 및 기타 건물 같은 구조. |
| >>> building                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 건물 합니다. |
| >>>> educationBuilding       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 건물 교육용 사용 합니다. |
| >>>> medicalBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 건물 같은 병원 의료 목적으로 사용 합니다. |
| >>>> transitBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 건물 공항 등의 전송 중에 사용 합니다. |
| >> transportation            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 교통망의 일부인 줄(예: 도로, 기차, 및 여객선). |
| >>> road                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 모든 도로를 표시하는 줄. |
| >>>> controlledAccessHighway | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 큰, 제어 된 액세스 회피를 나타내는 줄. |
| >>>>> highSpeedRamp          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 일반적으로 연결 하는 고속 경사로 나타내는 줄 회피 액세스를 제어 합니다. |
| >>>> highway                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 회피를 나타내는 줄. |
| >>>> majorRoad               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 주요도 나타내는 줄. |
| >>>> arterialRoad            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Arterial도 나타내는 줄. |
| >>>> street                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 거리를 나타내는 줄. |
| >>>>> ramp                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 일반적으로 연결 회피 하는 경사로 나타내는 줄. |
| >>>>> unpavedStreet          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Unpaved 거리를 나타내는 줄. |
| >>>> tollRoad                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 비용을 지불 하는로 나타내는 줄. |
| >>> railway                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 철로길. |
| >>> trail                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 공원 또는 자국 하이킹 코스 산책로. |
| >>> 연결부                  | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 관리자 권한 연결부 합니다. |
| >>> waterRoute               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 페리 경로 선. |
| >> water                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 물처럼 보이는 모든 것. 바다 및 물결이 포함됩니다. |
| >>> river                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 강, 물결 또는 기타 수로.  선 또는 다각형일 수 있으며 강이 아닌 수공간에 연결될 수 있습니다. |
| > routeMapElement            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 모든 라우팅 관련된 항목 |
| >> routeLine                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 선 라우트 관련 항목입니다. |
| >>> drivingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 운전 경로 나타내는 줄. |
| >>> scenicRoute              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 멋진 운전 경로 나타내는 줄. |
| >>> walkingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 줄 walking 경로 나타냅니다. |
| > userMapElement             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 모든 사용자 항목입니다. |
| >> userBillboard             | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 기본 [MapBillboard](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 인스턴스에 대한 스타일. |
| >> userLine                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 기본 [MapPolyline](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolyline) 인스턴스에 대한 스타일. |
| >> userModel3D               | [MapElement3D](#mapelement3d) |      |  ✔   |  ✔   |  ✔   | 기본 [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) 인스턴스에 대한 스타일.  주로 renderAsSurface 설정에 사용됩니다. |
| >> userPoint                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 기본 [MapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon) 인스턴스에 대한 스타일. |

<a id="properties" />

## <a name="properties"></a>속성

이 섹션은 각 항목에 사용할 수 있는 속성을 설명합니다.

<a id="version" />

### <a name="version-properties"></a>버전 속성

| 속성                     | 형식    | 설명                                                                                                           |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------|
| 버전                      | 문자열  | 대상 스타일 시트 버전입니다. 적용을 위해 사용됩니다. "1.0"이 기본값이며 기능 업데이트에 대해 "1.*"입니다. |

<a id="settings" />

### <a name="settings-properties"></a>설정 속성

| 속성                     | 형식    | 1506 | 1629 | 1713 | Next | 설명 |
|------------------------------|---------|------|------|------|------|-------------|
| atmosphereVisible            | 부울    |  ✔   |  ✔   |  ✔   |  ✔   | 3D 컨트롤에 대기를 표시할지 여부를 나타내는 플래그. |
| buildingTexturesVisible      | 부울    |      |      |  ✔   |  ✔   | 텍스처가 있는 기호 3D 건물에 텍스처를 표시할지 여부를 나타내는 플래그입니다. |
| fogColor                     | 색상   |  ✔   |  ✔   |  ✔   |  ✔   | 3D 컨트롤에 나타나는 거리 안개의 ARGB색 값. |
| glowColor                    | 색상   |  ✔   |  ✔   |  ✔   |  ✔   | 레이블 빛 및 아이콘 빛에 적용할 수 있는 ARGB색 값. |
| imageFamily                  | 문자열  |  ✔   |  ✔   |  ✔   |  ✔   | 이 스타일을 사용하도록 설정하는 이미지 세트의 이름입니다. 현실 세계의 기호에 따라 고정된 색상을 사용하는 기호의 경우 이 값을 *기본*으로 설정합니다. 색상표를 구성 가능한 색을 사용하는 기호의 경우 이 값을 *색상표*로 설정합니다. |
| landColor                    | 색상   |  ✔   |  ✔   |  ✔   |  ✔   | 육지에 아무 것도 그리기 전의 육지의 ARGB 색 값. |
| logosVisible                 | 부울    |  ✔   |  ✔   |  ✔   |  ✔   | **조직** 속성을 가진 항목이 적절한 로고를 그리거나 일반 아이콘을 사용해야 하는지를 나타내는 플래그. |
| officialColorVisible         | 부울    |  ✔   |  ✔   |  ✔   |  ✔   | 공식 색 속성(예: 중국의 교통 라인)을 가진 항목이 해당 색을 그려야 하는지 여부를 나타내는 플래그. 예를 들어 흑백 지도의 경우 이 값을 해제합니다. |
| rasterRegionsVisible         | 부울    |  ✔   |  ✔   |  ✔   |  ✔   | 벡터 (일본 및 한국) 보다 더 나은 표현이 있는 래스터 지역 그릴 것인지 여부를 나타내는 플래그. |
| shadedReliefVisible          | 부울    |  ✔   |  ✔   |  ✔   |  ✔   | 지도에 점진 음영을 그릴지 여부를 나타내는 플래그. |
| shadedReliefDarkColor        | 색상   |  ✔   |  ✔   |  ✔   |  ✔   | 음영기복의 어두운 면의 색상.  알파 채널은 최대 알파 값을 나타냅니다. |
| shadedReliefLightColor       | 색상   |  ✔   |  ✔   |  ✔   |  ✔   | 음영기복의 밝은 면의 색상.  알파 채널은 최대 알파 값을 나타냅니다. |
| shadowColor                  | 색상   |      |      |      |  ✔️   | 그림자를 사용 하는 아이콘 뒤 그림자의 색. |
| spaceColor                   | 색상   |  ✔   |  ✔   |  ✔   |  ✔   | 지도 주위의 영역에 대한 ARGB 색 값. |
| useDefaultImageColors        | 부울    |  ✔   |  ✔   |  ✔   |  ✔   | SVG에서 원래 색 이미지에서 색상에 대 한 색상표 항목을 조회 하는 대신 사용 해야 하는지 여부를 나타내는 플래그. |

<a id="mapelement" />

### <a name="mapelement-properties"></a>MapElement 요소

| 속성                     | 형식    | 1506 | 1629 | 1713 | Next | 설명 |
|------------------------------|---------|------|------|------|------|-------------|
| backgroundScale              | 부동   |  ✔   |  ✔   |  ✔   |  ✔   | 아이콘의 배경 요소를 확장할 양.  예를 들어 기본값에 *1*, 대형의 두 배 값에 *2*를 사용합니다. |
| fillColor                    | 색상   |  ✔   |  ✔   |  ✔   |  ✔   | 나눠진 경우 다각형, 포인트 아이콘의 배경, 선의 중앙을 채우는 데 사용되는 색. |
| fontFamily                   | 문자열  |  ✔   |  ✔   |  ✔   |  ✔   |  |
| iconColor                    | 색상   |  ✔   |  ✔   |  ✔   |  ✔   | 포인트 아이콘의 중앙에 표시되는 문자 모양의 색. |
| iconScale                    | 부동   |      |  ✔   |  ✔   |  ✔   | 아이콘의 문자 모양을 확장할 양.  예를 들어 기본값에 *1*, 대형의 두 배 값에 *2*를 사용합니다. |
| labelColor                   | 색상   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelOutlineColor            | 색상   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelScale                   | 부동   |  ✔   |  ✔   |  ✔   |  ✔   | 기본 레이블 크기가 조정되는 양. 예를 들어 기본값에 *1*, 대형의 두 배 값에 *2*를 사용합니다. |
| labelVisible                 | 부울    |  ✔   |  ✔   |  ✔   |  ✔   |  |
| overwriteColor               | 부울    |  ✔   |  ✔   |  ✔   |  ✔   | **FillColor**의 알파 값은 **StrokeColor**와 혼합되는 것이 아니라 이를 덮어씁니다. |
| scale                        | 부동   |  ✔   |  ✔   |  ✔   |  ✔   | 전체 포인트의 크기를 조정하는 정도. 예를 들어 기본값에 *1*, 대형의 두 배 값에 *2*를 사용합니다. |
| strokeColor                  | 색상   |  ✔   |  ✔   |  ✔   |  ✔   | 다각형 주위의 윤곽선, 포인트 아이콘의 윤곽선, 선의 색에 사용할 색. |
| strokeWidthScale             | 부동   |  ✔   |  ✔   |  ✔   |  ✔   | 선의 스트로크의 크기를 조정하는 정도. 예를 들어 기본값에 *1*, 대형의 두 배 값에 *2*를 사용합니다. |
| 표시                      | 부울    |  ✔   |  ✔   |  ✔   |  ✔   |  |

<a id="borderedmap" />

### <a name="borderedmapelement"></a>BorderedMapElement

이 속성 그룹은 [MapElement](#mapelement) 속성 그룹에서 상속됩니다.

| 속성                     | 형식    | 1506 | 1629 | 1713 | Next | 설명 |
|------------------------------|---------|------|------|------|------|-------------|
| borderOutlineColor           | 색상   |  ✔   |  ✔   |  ✔   |  ✔   | 채워진 다각형의 두 번째 또는 채워진 테두리 선의 색. |
| borderStrokeColor            | 색상   |  ✔   |  ✔   |  ✔   |  ✔   | 채워진 다각형의 테두리의 기본 선 색상. |
| borderVisible                | 부울    |  ✔   |  ✔   |  ✔   |  ✔   |  |
| borderWidthScale             | 부동   |  ✔   |  ✔   |  ✔   |  ✔   | 양 테두리의 스트로크 됩니다. 예를 들어 기본값에 *1*, 대형의 두 배 값에 *2*를 사용합니다. |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>PointStyle 속성

이 속성 그룹은 [MapElement](#mapelement) 속성 그룹에서 상속됩니다.

| 속성                     | 형식    | 1506 | 1629 | 1713 | Next | 설명 |
|------------------------------|---------|------|------|------|------|-------------|
| 셰이프 배경             | 부동   |      |      |      |  ✔️   | -가 있는 모양이 대체 아이콘의 배경으로 사용 하 여 셰이프입니다. |
| stemAnchorRadiusScale        | 부동   |      |      |  ✔   |  ✔   | 아이콘 스템의 앵커 지점을 확장할 양.  예를 들어 기본값에 *1*, 대형의 두 배 값에 *2*를 사용합니다. |
| stemColor                    | 색상   |  ✔   |  ✔   |  ✔   |  ✔   | 3D 모드에서 해당 아이콘의 하단에서 나오는 줄기의 색. |
| stemHeightScale              | 부동   |      |      |  ✔   |  ✔   | 아이콘 스템의 길이를 확장할 양.  예를 들어 기본값에 *1*, 길이의 두 배 값에 *2*를 사용합니다. |
| stemOutlineColor             | 색상   |  ✔   |  ✔   |  ✔   |  ✔   | 3D 모드에서 해당 아이콘의 하단에서 나오는 줄기 주변의 윤곽선 색. |
| stemWidthScale               | 부동   |  ✔   |  ✔   |  ✔   |  ✔   | 아이콘 스템의 너비를 확장할 양.  예를 들어 기본값에 *1*, 길이의 두 배 값에 *2*를 사용합니다. |

<a id="mapelement3d" />

### <a name="mapelement3d"></a>MapElement3D

이 속성 그룹은 [MapElement](#mapelement) 속성 그룹에서 상속됩니다.

| 속성                     | 형식    | 1506 | 1629 | 1713 | Next | 설명 |
|------------------------------|---------|------|------|------|------|------------|
| renderAsSurface              | 부울    |      |      |  ✔   |  ✔   | 3D 모델이 땅에 대한 깊이 페이드 인 없이 건물 같이 렌더링할 수 있음을 나타내는 플래그입니다. |
