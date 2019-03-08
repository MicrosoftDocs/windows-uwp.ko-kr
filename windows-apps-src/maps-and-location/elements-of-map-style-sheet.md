---
description: 지도 스타일 시트의 항목 및 속성
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 지도 스타일 시트 참조
ms.date: 03/19/2017
ms.topic: article
keywords: Windows 10, uwp, 지도, 지도 스타일 시트
ms.localizationpriority: medium
ms.openlocfilehash: f199e08f74ace4e6c8b123a701af19469b029aed
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608418"
---
# <a name="map-style-sheet-reference"></a>지도 스타일 시트 참조

Microsoft 매핑 기술을 사용 하 여 _스타일 시트를 매핑할_ maps의 모양을 정의할 수 있습니다.  맵 스타일 시트 개체 JSON (JavaScript Notation)을 사용 하 여 정의 되 고 Windows 스토어 응용 프로그램의 비롯 한 다양 한 방법으로 사용할 수 있습니다 [MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol) 를 통해 합니다 [MapStyleSheet.ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_) 메서드입니다.

대화형으로 사용 하 여 스타일 시트를 만들 수 있습니다 합니다 [맵 스타일 시트 편집기](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) 응용 프로그램입니다.

다음 JSON 빨간색으로 표시 되는 최고 수 위 영역을 만드는 데 사용할 수 있습니다, 물 레이블이 녹색으로 표시 및 land 영역은 파란색으로 표시 합니다.

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

이 JSON 맵에서 모든 레이블 및 지점을 제거 하려면 사용할 수 있습니다.

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

이 항목에서는 지도의 모양과 느낌을 사용자 지정할 수 있는 JSON 항목 및 [속성](#properties)을 보여 줍니다.  이러한 속성을 통해 사용자 지도 요소에 적용할 수도 있습니다는 [MapStyleSheetEntry](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.mapstylesheetentry) 속성입니다.

<a id="entries" />

## <a name="entries"></a>항목
이 표는 ">" 문자를 사용하여 입력 계층의 수준을 나타냅니다.  각 항목을 지 원하는 Windows의 버전 및이 무시 하는 방법도 보여줍니다.

| 버전 | Windows 릴리스 이름 |
|---------|----------------------|
|  1703   | 크리에이터스 업데이트      |
|  1709   | Fall Creators Update |
|  1803   | 2018년 4월 업데이트    |
|  1809   | 2018 년 10 월 업데이트  |

| 이름                         | 속성 그룹            | 1703 | 1709 | 1803 | 1809 | 설명    |
|------------------------------|---------------------------|------|------|------|------|----------------|
| version                      | [버전](#version)       |  ✔   |  ✔   |  ✔   |  ✔   | 사용하고자 하는 스타일 시트 버전입니다. |
| 설정                     | [설정](#settings)     |  ✔   |  ✔   |  ✔   |  ✔   | 전체 스타일 시트에 적용되는 설정입니다. |
| mapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 모든 지도 항목의 상위 항목입니다. |
| > baseMapElement             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 사용자가 아닌 모든 항목의 상위 항목입니다. |
| >> area                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Land를 설명 하는 영역을 사용 합니다.  이러한 구조 항목 아래에 실제 건물을 사용 하 여 혼동 하면 안 됩니다. |
| >>> airport                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 공항을 포함 하는 영역을 선택 합니다. |
| >>> areaOfInterest           | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 비즈니스 또는 흥미로운 포인트가 집중된 영역입니다. |
| >>> cemetery                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Cemeteries을 포함 하는 영역을 선택 합니다. |
| >>> continent                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 대륙 영역 레이블입니다. |
| >>> education                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 학교와 교육 다른 기능을 포함 하는 영역을 선택 합니다. |
| >>> indigenousPeoplesReserve | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 토착 행위를 포함 하는 영역을 예약 합니다. |
| >>> industrial               | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 산업을 위해 사용 되는 영역입니다. |
| >>> island                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 아일랜드 영역 레이블입니다. |
| >>> medical                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 의료 목적에 사용 되는 영역 (예: 병원 캠퍼스). |
| >>> military                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 군 자료를 포함 하거나 군 사용 하는 영역을 선택 합니다. |
| >>> nautical                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 항해 관련된 목적에 사용 되는 영역입니다. |
| >>> neighborhood             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 환경 영역 레이블입니다. |
| >>> runway                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 비행기가는 활주로으로 사용 되는 영역을 선택 합니다. |
| >>> sand                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 해변과 같은 모래 영역입니다. |
| >>> shoppingCenter           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 몰 또는 기타 쇼핑 센터에 대한 땅 영역입니다. |
| >>> stadium                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 경기장을 포함 하는 영역을 선택 합니다. |
| >>> underground              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 지하 영역(예: 지하철역). |
| >>> vegetation               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 숲, 풀 영역 등입니다. |
| >>>> forest                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 숲 지역입니다. |
| >>>> golfCourse              | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 골프 과정을 포함 하는 영역을 선택 합니다. |
| >>>> park                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 공원을 포함 하는 영역을 선택 합니다. |
| >>>> playingField            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 야구장 또는 테니스 코트와 같은 추출된 피치입니다. |
| >>>> reserve                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 특성을 포함 하는 영역을 예약 합니다. |
| >> point                     | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 일종의 아이콘을 사용 하 여 그린 모든 지점 기능입니다. |
| >>> address                  | [PointStyle](#pointstyle) |      |      |  ✔   |  ✔   | 숫자 레이블을 해결 합니다. |
| >>> naturalPoint             | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 자연 스러운 기능을 나타내는 아이콘을 선택 합니다. |
| >>>> peak                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 산 봉우리를 나타내는 아이콘입니다. |
| >>>>> volcanicPeak           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 화산 봉우리를 나타내는 아이콘입니다. |
| >>>> waterPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 폭포와 같은 물 기능 위치를 나타내는 아이콘입니다. |
| >>> pointOfInterest          | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 모든 흥미로운 위치를 나타내는 아이콘을 선택 합니다. |
| >>>> business                | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 모든 비즈니스 위치를 나타내는 아이콘을 선택 합니다. |
| >>>>> attractionPoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 여행자 실감 박물관, 동물원 등을 나타내는 아이콘을 선택 합니다. |
| >>>>> communityPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 커뮤니티에 범용의 위치를 나타내는 아이콘을 선택 합니다. |
| >>>>> educationPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 학교와 기타 교육을 나타내는 아이콘 관련 위치입니다. |
| >>>>> entertainmentPoint     | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 극장, 시네마 및 등과 같은 엔터테인먼트 장소를 나타내는 아이콘을 선택 합니다. |
| >>>>> essentialServicePoint  | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 파킹, 은행, 가스 등과 같은 필수 서비스를 나타내는 아이콘을 선택 합니다. |
| >>>>> foodPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 식당, 카페, 등을 나타내는 아이콘을 선택 합니다. |
| >>>>> lodgingPoint           | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 호텔 및 다른 숙박 업체를 나타내는 아이콘을 선택 합니다. |
| >>>>> realEstatePoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 부동산 비즈니스를 나타내는 아이콘을 선택 합니다. |
| >>>>> shoppingPoint          | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 호텔 및 다른 숙박 업체를 나타내는 아이콘을 선택 합니다. |
| >>> populatedPlace           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 밀도가 높은 곳의 크기를 나타내는 아이콘(예: 도시 또는 타운). |
| >>>> capital                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 밀도가 높은 장소의 수도를 나타내는 아이콘. |
| >>>>> adminDistrictCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 시/도의 수도를 나타내는 아이콘. |
| >>>>> countryRegionCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 국가 또는 지역 대문자 나타내는 아이콘이 있습니다. |
| >>> roadShield               | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 도로의 간단한 이름을 나타내는 기호. (예: I-5). 설정 항목의 **ImageFamily** 속성을 *색상표* 값으로 설정하는 경우 색상표 값만 사용 |
| >>> roadExit                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 일반적으로 진입 통제 고속도로의 비상구를 나타내는 아이콘. |
| >>> transit                  | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 버스 정류장, 기차 정차역, 공항 등을 나타내는 아이콘. |
| >> political                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 국가, 지역 및 주와 같은 정치적 지역. |
| >>> countryRegion            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 국가 지역 테두리 및 레이블입니다. |
| >>> adminDistrict            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 등, 시/도, 상태, Admin1 테두리 및 레이블을 지정 합니다. |
| >>> district                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin2, 군 등 테두리 및 레이블을 지정 합니다. |
| >> structure                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 건물 및 기타 건물 같은 구조. |
| >>> building                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 건물 합니다. |
| >>>> educationBuilding       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 건물 교육에 사용 합니다. |
| >>>> medicalBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 건물 병원과 같이 의료 목적으로 사용 합니다. |
| >>>> transitBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 건물 공항 같은 전송에 사용 합니다. |
| >> transportation            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 교통망의 일부인 줄(예: 도로, 기차, 및 여객선). |
| >>> road                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 모든 도로를 표시하는 줄. |
| >>>> controlledAccessHighway | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 큰 제어 된 액세스 권한을 고속도 나타내는 선입니다. |
| >>>>> highSpeedRamp          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 일반적으로 연결 하는 고속 램프를 나타내는 선 고속도 액세스를 제어 합니다. |
| >>>> highway                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 고속도 나타내는 선입니다. |
| >>>> majorRoad               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 주요도 나타내는 선입니다. |
| >>>> arterialRoad            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Arterial도 나타내는 선입니다. |
| >>>> street                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 거리를 나타내는 선입니다. |
| >>>>> ramp                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 고속도로에 연결 하는 일반적으로 경사를 나타내는 선입니다. |
| >>>>> unpavedStreet          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Unpaved 거리를 나타내는 선입니다. |
| >>>> tollRoad                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 사용에 대 한 비용으로로 나타내는 선입니다. |
| >>> railway                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 철로길. |
| >>> trail                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 공원 또는 자국 하이킹 코스 산책로. |
| >>> 보 행 통로                  | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 관리자 권한 보 행 통로입니다. |
| >>> waterRoute               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 페리 경로 선. |
| >> water                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 물처럼 보이는 모든 것. 바다 및 물결이 포함됩니다. |
| >>> river                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 강, 물결 또는 기타 수로.  선 또는 다각형일 수 있으며 강이 아닌 수공간에 연결될 수 있습니다. |
| > routeMapElement            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 모든 라우팅 관련된 항목 |
| >> routeLine                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 선 라우팅 관련 항목입니다. |
| >>> drivingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 주행 경로 나타내는 선입니다. |
| >>> scenicRoute              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Scenic 주행 경로 나타내는 선입니다. |
| >>> walkingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 줄 경로 탐색을 나타냅니다. |
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
| version                      | 문자열  | 대상 스타일 시트 버전입니다. 적용을 위해 사용됩니다. "1.0"이 기본값이며 기능 업데이트에 대해 "1.*"입니다. |

<a id="settings" />

### <a name="settings-properties"></a>설정 속성

| 속성                     | 형식    | 1703 | 1709 | 1803 | 1809 | 설명 |
|------------------------------|---------|------|------|------|------|-------------|
| atmosphereVisible            | 부울    |  ✔   |  ✔   |  ✔   |  ✔   | 3D 컨트롤에 대기를 표시할지 여부를 나타내는 플래그. |
| buildingTexturesVisible      | 부울    |      |      |  ✔   |  ✔   | 텍스처가 있는 기호 3D 건물에 텍스처를 표시할지 여부를 나타내는 플래그입니다. |
| fogColor                     | 색   |  ✔   |  ✔   |  ✔   |  ✔   | 3D 컨트롤에 나타나는 거리 안개의 ARGB색 값. |
| glowColor                    | 색   |  ✔   |  ✔   |  ✔   |  ✔   | 레이블 빛 및 아이콘 빛에 적용할 수 있는 ARGB색 값. |
| imageFamily                  | 문자열  |  ✔   |  ✔   |  ✔   |  ✔   | 이 스타일을 사용하도록 설정하는 이미지 세트의 이름입니다. 현실 세계의 기호에 따라 고정된 색상을 사용하는 기호의 경우 이 값을 *기본*으로 설정합니다. 색상표를 구성 가능한 색을 사용하는 기호의 경우 이 값을 *색상표*로 설정합니다. |
| landColor                    | 색   |  ✔   |  ✔   |  ✔   |  ✔   | 육지에 아무 것도 그리기 전의 육지의 ARGB 색 값. |
| logosVisible                 | 부울    |  ✔   |  ✔   |  ✔   |  ✔   | **조직** 속성을 가진 항목이 적절한 로고를 그리거나 일반 아이콘을 사용해야 하는지를 나타내는 플래그. |
| officialColorVisible         | 부울    |  ✔   |  ✔   |  ✔   |  ✔   | 공식 색 속성(예: 중국의 교통 라인)을 가진 항목이 해당 색을 그려야 하는지 여부를 나타내는 플래그. 예를 들어 흑백 지도의 경우 이 값을 해제합니다. |
| rasterRegionsVisible         | 부울    |  ✔   |  ✔   |  ✔   |  ✔   | 벡터 (일본 및 한국) 보다 더 나은 표현이 있는 래스터 영역을 그릴 것인지 여부를 나타내는 플래그입니다. |
| shadedReliefVisible          | 부울    |  ✔   |  ✔   |  ✔   |  ✔   | 지도에 점진 음영을 그릴지 여부를 나타내는 플래그. |
| shadedReliefDarkColor        | 색   |  ✔   |  ✔   |  ✔   |  ✔   | 음영기복의 어두운 면의 색상.  알파 채널 최대 알파 값을 나타냅니다. |
| shadedReliefLightColor       | 색   |  ✔   |  ✔   |  ✔   |  ✔   | 음영기복의 밝은 면의 색상.  알파 채널 최대 알파 값을 나타냅니다. |
| shadowColor                  | 색   |      |      |      |  ✔   | 그림자를 사용 하는 아이콘 뒤에 그림자의 색입니다. |
| spaceColor                   | 색   |  ✔   |  ✔   |  ✔   |  ✔   | 지도 주위의 영역에 대한 ARGB 색 값. |
| useDefaultImageColors        | 부울    |  ✔   |  ✔   |  ✔   |  ✔   | SVG 원래 색 색상표 항목 이미지의에서 색상을 조회 하는 대신 사용 해야 하는지 여부를 나타내는 플래그입니다. |

<a id="mapelement" />

### <a name="mapelement-properties"></a>MapElement 요소

| 속성                     | 형식    | 1703 | 1709 | 1803 | 1809 | 설명 |
|------------------------------|---------|------|------|------|------|-------------|
| backgroundScale              | 부동   |  ✔   |  ✔   |  ✔   |  ✔   | 아이콘의 배경 요소를 확장할 양.  예를 들어 기본값에 *1*, 대형의 두 배 값에 *2*를 사용합니다. |
| fillColor                    | 색   |  ✔   |  ✔   |  ✔   |  ✔   | 나눠진 경우 다각형, 포인트 아이콘의 배경, 선의 중앙을 채우는 데 사용되는 색. |
| fontFamily                   | 문자열  |  ✔   |  ✔   |  ✔   |  ✔   |  |
| iconColor                    | 색   |  ✔   |  ✔   |  ✔   |  ✔   | 포인트 아이콘의 중앙에 표시되는 문자 모양의 색. |
| iconScale                    | 부동   |      |  ✔   |  ✔   |  ✔   | 아이콘의 문자 모양을 확장할 양.  예를 들어 기본값에 *1*, 대형의 두 배 값에 *2*를 사용합니다. |
| labelColor                   | 색   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelOutlineColor            | 색   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelScale                   | 부동   |  ✔   |  ✔   |  ✔   |  ✔   | 기본 레이블 크기가 조정되는 양. 예를 들어 기본값에 *1*, 대형의 두 배 값에 *2*를 사용합니다. |
| labelVisible                 | 부울    |  ✔   |  ✔   |  ✔   |  ✔   |  |
| overwriteColor               | 부울    |  ✔   |  ✔   |  ✔   |  ✔   | **FillColor**의 알파 값은 **StrokeColor**와 혼합되는 것이 아니라 이를 덮어씁니다. |
| scale                        | 부동   |  ✔   |  ✔   |  ✔   |  ✔   | 전체 포인트의 크기를 조정하는 정도. 예를 들어 기본값에 *1*, 대형의 두 배 값에 *2*를 사용합니다. |
| strokeColor                  | 색   |  ✔   |  ✔   |  ✔   |  ✔   | 다각형 주위의 윤곽선, 포인트 아이콘의 윤곽선, 선의 색에 사용할 색. |
| strokeWidthScale             | 부동   |  ✔   |  ✔   |  ✔   |  ✔   | 선의 스트로크의 크기를 조정하는 정도. 예를 들어 기본값에 *1*, 대형의 두 배 값에 *2*를 사용합니다. |
| 표시                      | 부울    |  ✔   |  ✔   |  ✔   |  ✔   |  |

<a id="borderedmap" />

### <a name="borderedmapelement"></a>BorderedMapElement

이 속성 그룹은 [MapElement](#mapelement) 속성 그룹에서 상속됩니다.

| 속성                     | 형식    | 1703 | 1709 | 1803 | 1809 | 설명 |
|------------------------------|---------|------|------|------|------|-------------|
| borderOutlineColor           | 색   |  ✔   |  ✔   |  ✔   |  ✔   | 채워진 다각형의 두 번째 또는 채워진 테두리 선의 색. |
| borderStrokeColor            | 색   |  ✔   |  ✔   |  ✔   |  ✔   | 채워진 다각형의 테두리의 기본 선 색상. |
| borderVisible                | 부울    |  ✔   |  ✔   |  ✔   |  ✔   |  |
| borderWidthScale             | 부동   |  ✔   |  ✔   |  ✔   |  ✔   | 테두리의 스트로크는 크기를 조정 하는 양입니다. 예를 들어 기본값에 *1*, 대형의 두 배 값에 *2*를 사용합니다. |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>PointStyle 속성

이 속성 그룹은 [MapElement](#mapelement) 속성 그룹에서 상속됩니다.

| 속성                     | 형식    | 1703 | 1709 | 1803 | 1809 | 설명 |
|------------------------------|---------|------|------|------|------|-------------|
| 모양 배경             | 부동   |      |      |      |  ✔   | 존재 하는 모든 셰이프를 대체 아이콘-의 배경으로 사용할 셰이프입니다. |
| stemAnchorRadiusScale        | 부동   |      |      |  ✔   |  ✔   | 아이콘 스템의 앵커 지점을 확장할 양.  예를 들어 기본값에 *1*, 대형의 두 배 값에 *2*를 사용합니다. |
| stemColor                    | 색   |  ✔   |  ✔   |  ✔   |  ✔   | 3D 모드에서 해당 아이콘의 하단에서 나오는 줄기의 색. |
| stemHeightScale              | 부동   |      |      |  ✔   |  ✔   | 아이콘 스템의 길이를 확장할 양.  예를 들어 기본값에 *1*, 길이의 두 배 값에 *2*를 사용합니다. |
| stemOutlineColor             | 색   |  ✔   |  ✔   |  ✔   |  ✔   | 3D 모드에서 해당 아이콘의 하단에서 나오는 줄기 주변의 윤곽선 색. |
| stemWidthScale               | 부동   |  ✔   |  ✔   |  ✔   |  ✔   | 아이콘 스템의 너비를 확장할 양.  예를 들어 기본값에 *1*, 길이의 두 배 값에 *2*를 사용합니다. |

<a id="mapelement3d" />

### <a name="mapelement3d"></a>MapElement3D

이 속성 그룹은 [MapElement](#mapelement) 속성 그룹에서 상속됩니다.

| 속성                     | 형식    | 1703 | 1709 | 1803 | 1809 | 설명 |
|------------------------------|---------|------|------|------|------|------------|
| renderAsSurface              | 부울    |      |      |  ✔   |  ✔   | 3D 모델이 땅에 대한 깊이 페이드 인 없이 건물 같이 렌더링할 수 있음을 나타내는 플래그입니다. |
