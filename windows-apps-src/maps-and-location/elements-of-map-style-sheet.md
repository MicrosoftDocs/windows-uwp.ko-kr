---
description: 지도 스타일 시트의 항목 및 속성
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 지도 스타일 시트 참조
ms.date: 03/19/2017
ms.topic: article
keywords: Windows 10, uwp, 지도, 지도 스타일 시트
ms.localizationpriority: medium
ms.openlocfilehash: 723426b4affec4251f26485ac7ecc1fca307c102
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867707"
---
# <a name="map-style-sheet-reference"></a>지도 스타일 시트 참조

Microsoft 매핑 기술은 _지도 스타일 시트_ 를 사용 하 여 지도의 모양을 정의 합니다.  지도 스타일 시트는 JavaScript Object Notation (JSON)를 사용 하 여 정의 되며 [Mapstylesheet. ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_) 메서드를 통해 Windows 스토어 응용 프로그램의 [없습니다](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol) 을 비롯 한 다양 한 방법으로 사용할 수 있습니다.

스타일 시트는 [지도 스타일 시트 편집기](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) 응용 프로그램을 사용 하 여 대화형으로 만들 수 있습니다.

다음 JSON을 사용 하면 워터 마크를 빨간색으로 표시 하 고, 워터 마크 레이블을 녹색으로 표시 하 고, 육지 영역을 파란색으로 표시할 수 있습니다.

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

이 JSON을 사용 하 여 맵에서 모든 레이블과 요소를 제거할 수 있습니다.

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

이 항목에서는 지도의 모양과 느낌을 사용자 지정할 수 있는 JSON 항목 및 [속성](#properties)을 보여 줍니다.  이러한 속성은 [MapStyleSheetEntry](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.mapstylesheetentry) 속성을 통해 사용자 지도 요소에도 적용할 수 있습니다.

<a id="entries" />

## <a name="entries"></a>항목
이 표는 ">" 문자를 사용하여 입력 계층의 수준을 나타냅니다.  또한 각 항목을 지 원하는 Windows 버전을 보여 주고이를 무시 합니다.

| 버전 | Windows 릴리스 이름 |
|---------|----------------------|
|  1703   | 크리에이터스 업데이트      |
|  1709   | Fall Creators Update |
|  1803   | 2018년 4월 업데이트    |
|  1809   | 2018 10 월 업데이트  |
|  1903   | 2019 년 5 월 업데이트      |

| 이름                               | 속성 그룹            | 1703 | 1709 | 1803 | 1809 | 1903 | 설명    |
|------------------------------------|---------------------------|------|------|------|------|------|----------------|
| version                            | [버전(Version)](#version)       |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 사용하고자 하는 스타일 시트 버전입니다. |
| 설정                           | [설정](#settings)     |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 전체 스타일 시트에 적용되는 설정입니다. |
| mapElement                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 모든 지도 항목의 상위 항목입니다. |
| > baseMapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 사용자가 아닌 모든 항목의 상위 항목입니다. |
| >> area                            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 육지 사용을 설명 하는 영역입니다.  구조 항목 아래에 있는 물리적 건물과 혼동 해서는 안 됩니다. |
| >>> airport                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 공항을 포함 하는 영역입니다. |
| >>> areaOfInterest                 | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | 비즈니스 또는 흥미로운 포인트가 집중된 영역입니다. |
| >>> cemetery                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Cemeteries를 포함 하는 영역입니다. |
| >>> continent                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 대륙 영역 레이블. |
| >>> education                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 학교 및 기타 교육 시설을 포함 하는 영역입니다. |
| >>> indigenousPeoplesReserve       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Indigenous peoples 예약 된 영역입니다. |
| >>> industrial                     | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | 산업 용도로 사용 되는 영역입니다. |
| >>> island                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 섬 영역 레이블. |
| >>> medical                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 의료 목적으로 사용 되는 영역 (예: 병원 캠퍼스). |
| >>> military                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 군사 베이스를 포함 하거나 군사를 사용 하는 영역입니다. |
| >>> nautical                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 항해 관련 목적으로 사용 되는 영역입니다. |
| >>> neighborhood                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 환경 영역 레이블입니다. |
| >>> runway                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 비행기 활주로 사용 되는 영역입니다. |
| >>> sand                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 해변과 같은 모래 영역입니다. |
| >>> shoppingCenter                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 몰 또는 기타 쇼핑 센터에 대한 땅 영역입니다. |
| >>> stadium                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 경기장를 포함 하는 영역입니다. |
| >>> underground                    | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | 지하 영역(예: 지하철역). |
| >>> vegetation                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 숲, 풀 영역 등입니다. |
| >>>> forest                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 숲 지역입니다. |
| >>>> golfCourse                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 골프 코스를 포함 하는 영역입니다. |
| >>>> park                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 공원를 포함 하는 영역입니다. |
| >>>> playingField                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 야구장 또는 테니스 코트와 같은 추출된 피치입니다. |
| >>>> reserve                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 특성을 포함 하는 영역을 예약 합니다. |
| > > frozenWater                     | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Glacier와 같은 고정 된 물입니다. |
| >> point                           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 일종의 아이콘을 사용 하 여 그린 모든 point 기능입니다. |
| >>> address                        | [PointStyle](#pointstyle) |      |      |  ✔   |  ✔   |  ✔   | 주소 번호 레이블. |
| >>> naturalPoint                   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 자연 기능을 나타내는 아이콘입니다. |
| >>>> peak                          | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 산 봉우리를 나타내는 아이콘입니다. |
| >>>>> volcanicPeak                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 화산 봉우리를 나타내는 아이콘입니다. |
| >>>> waterPoint                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 폭포와 같은 물 기능 위치를 나타내는 아이콘입니다. |
| >>> pointOfInterest                | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 모든 관심 있는 위치를 나타내는 아이콘입니다. |
| >>>> business                      | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 모든 비즈니스 위치를 나타내는 아이콘입니다. |
| > > > > > attractionPoint              | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Museums, zoos 등의 tourist 맛보기을 나타내는 아이콘 |
| > > > > > > amusementPlacePoint         | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 게 즐거움 place 아이콘. |
| > > > > > > aquariumPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Aquarium 아이콘 |
| > > > > > > artGalleryPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 아트 갤러리 아이콘. |
| > > > > > > campPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 캠프 아이콘. |
| > > > > > > fishingPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 낚 싯 아이콘. |
| > > > > > > gardenPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 정원 아이콘. |
| > > > > > > hikingPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 하이킹 아이콘 |
| > > > > > > libraryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 라이브러리 아이콘. |
| > > > > > > museumPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 박물관 아이콘. |
| > > > > > > naturalPlacePoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 자연 스러운 아이콘. |
| > > > > > > parkPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 파킹 아이콘. |
| > > > > > > restAreaPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Rest 영역 아이콘. |
| > > > > > > touristAttractionPoint      | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Tourist 인력 아이콘 |
| > > > > > > zooPoint                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 동물원 아이콘. |
| > > > > > communityPoint               | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 커뮤니티에 대 한 일반적인 사용 위치를 나타내는 아이콘입니다. |
| > > > > > > conventionCenterPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 규칙 센터 아이콘. |
| > > > > > > financialPoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 재무 아이콘. |
| > > > > > > governmentPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 정부 아이콘. |
| > > > > > > informationTechnologyPoint  | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 정보 기술 아이콘. |
| > > > > > > palacePoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 왕궁 아이콘. |
| > > > > > > pollingStationPoint         | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 폴링 스테이션 아이콘. |
| > > > > > > researchPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 리서치 아이콘. |
| > > > > > educationPoint               | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 학교 및 기타 교육 관련 위치를 나타내는 아이콘입니다. |
| > > > > > entertainmentPoint           | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 극장, cinemas 등의 엔터테인먼트 장소을 나타내는 아이콘 |
| > > > > > > artsPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 예술 아이콘. |
| > > > > > > bowlingPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 볼링 아이콘 |
| > > > > > > casinoPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 카지노 아이콘 |
| > > > > > > golfCoursePoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 골프 과정 아이콘. |
| > > > > > > gymPoint                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 체육관 아이콘 |
| > > > > > > marinaPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Marina 아이콘 |
| > > > > > > movieTheaterPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 영화 극장 아이콘. |
| > > > > > > nightClubPoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 야간 클럽 아이콘. |
| > > > > > > recreationPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 레크리에이션 아이콘. |
| > > > > > > skatingPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Skating 아이콘 |
| > > > > > > skiAreaPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Ski area 아이콘. |
| > > > > > > stadiumPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 수영 수영장 아이콘. |
| > > > > > > swimmingPoolPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 수영 수영장 아이콘. |
| > > > > > > theaterPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 극장 아이콘. |
| > > > > > > wineryPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Winery에서는 아이콘 |
| > > > > > essentialServicePoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 파킹, 은행, 가스 등의 필수 서비스를 나타내는 아이콘 |
| > > > > > > aTMPoint                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | ATM 아이콘. |
| > > > > > > automobileRentalPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 자동차 임대 아이콘. |
| > > > > > > automobileRepairPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 자동차 복구 아이콘. |
| > > > > > > bankPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Bank 아이콘. |
| > > > > > > clinicPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 클리닉 아이콘. |
| > > > > > > electricChargingStationPoint| [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 전기 충전 스테이션 아이콘. |
| > > > > > > fireStationPoint            | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | FireStation 아이콘 |
| > > > > > > gasStationPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | GasStation 아이콘 |
| > > > > > > groceryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 식료품 아이콘. |
| > > > > > > hospitalPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 병원 아이콘. |
| > > > > > > laundryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Laundry 아이콘 |
| > > > > > > liquorAndWineStorePoint     | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Liquor 및 와인 스토어 아이콘. |
| > > > > > > mailPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 메일 아이콘. |
| > > > > > > marketPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 시장 아이콘. |
| > > > > > > parkingPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 파킹 아이콘. |
| > > > > > > petsPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 애완 동물 아이콘. |
| > > > > > > pharmacyPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Pharmacy 아이콘 |
| > > > > > > policePoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 경찰 아이콘 |
| > > > > > > postalServicePoint          | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 우편 서비스 아이콘. |
| > > > > > > professionalPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 전문 서비스 아이콘. |
| > > > > > > toiletPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 화장실 아이콘. |
| > > > > > > veterinarianPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Veterinarian 아이콘 |
| >>>>> foodPoint                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 식당, cafés 등을 나타내는 아이콘입니다. |
| > > > > > > barAndGrillPoint            | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 가로 막대형 및 그릴 아이콘. |
| > > > > > > 바 지점                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 막대 아이콘. |
| > > > > > > breweryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Brewery 아이콘 |
| > > > > > > cafePoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Cafe 아이콘 |
| > > > > > > iceCreamShopPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 아이스크림 아이콘. |
| > > > > > > restaurantPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 식당 아이콘. |
| > > > > > > teaShopPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | TeaShop 아이콘 |
| > > > > > lodgingPoint                 | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 호텔 및 기타 숙박 비즈니스를 나타내는 아이콘입니다. |
| > > > > > > gotelPoint                  | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 호텔 아이콘. |
| > > > > > realEstatePoint              | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 부동산 비즈니스를 나타내는 아이콘입니다. |
| > > > > > shoppingPoint                | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 호텔 및 기타 숙박 비즈니스를 나타내는 아이콘입니다. |
| > > > > > > automobileDealerPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 자동차 판매자 아이콘. |
| > > > > > > beautyAndSpaPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 장점 및 spa 아이콘. |
| > > > > > > bookstorePoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 서 점 아이콘. |
| > > > > > > departmentStorePoint        | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 부서 스토어 아이콘. |
| > > > > > > electronicsShopPoint        | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 전자 상점 아이콘. |
| > > > > > > golfShopPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 골프 상점 아이콘. |
| > > > > > > homeApplianceShopPoint      | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Home 어플라이언스 상점 아이콘. |
| > > > > > > mallPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 몰 아이콘. |
| > > > > > > phoneShopPoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 전화 쇼핑 아이콘. |
| >>> populatedPlace                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 밀도가 높은 곳의 크기를 나타내는 아이콘(예: 도시 또는 타운). |
| >>>> capital                       | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 밀도가 높은 장소의 수도를 나타내는 아이콘. |
| >>>>> adminDistrictCapital         | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 시/도의 수도를 나타내는 아이콘. |
| > > > > > > majorAdminDistrictCapital   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 시/도의 주 대문자를 나타내는 아이콘입니다. |
| > > > > > > minorAdminDistrictCapital   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 시/도의 부 (대문자)를 나타내는 아이콘입니다. |
| >>>>> countryRegionCapital         | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 국가 또는 지역 대문자 나타내는 아이콘이 있습니다. |
| > > > > majorPopulatedPlace           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 주 채워진 위치의 크기를 나타내는 아이콘입니다. |
| > > > > minorPopulatedPlace           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 작은 채워진 위치의 크기를 나타내는 아이콘입니다. |
| >>> roadShield                     | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 도로의 간단한 이름을 나타내는 기호. 예를 들면 다음과 같습니다. 5). 설정 항목의 **ImageFamily** 속성을 *색상표* 값으로 설정하는 경우 색상표 값만 사용 |
| >>> roadExit                       | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 일반적으로 진입 통제 고속도로의 비상구를 나타내는 아이콘. |
| >>> transit                        | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 버스 정류장, 기차 정차역, 공항 등을 나타내는 아이콘. |
| >> political                       | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 국가, 지역 및 주와 같은 정치적 지역. |
| >>> countryRegion                  | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 국가 영역 테두리 및 레이블. |
| >>> adminDistrict                  | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 1, 시/도, 시/도, 경계, 레이블 등이 있습니다. |
| >>> district                       | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 2, 군, 테두리 및 레이블. |
| >> structure                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 건물 및 기타 건물 같은 구조. |
| >>> building                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 건물. |
| >>>> educationBuilding             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 교육용으로 사용 되는 건물입니다. |
| >>>> medicalBuilding               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 병원와 같은 의료 목적에 사용 되는 건물입니다. |
| >>>> transitBuilding               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 공항 같은 전송에 사용 되는 건물입니다. |
| >> transportation                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 교통망의 일부인 줄(예: 도로, 기차, 및 여객선). |
| >>> road                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 모든 도로를 표시하는 줄. |
| >>>> controlledAccessHighway       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 제어 된 크고 톨게이트을 나타내는 줄입니다. |
| >>>>> highSpeedRamp                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 일반적으로 제어 되는 액세스 톨게이트에 연결 되는 고속 램프를 나타내는 선입니다. |
| >>>> highway                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 톨게이트을 나타내는 줄입니다. |
| >>>> majorRoad                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 주도로를 나타내는 선입니다. |
| >>>> arterialRoad                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Arterial도로를 나타내는 선입니다. |
| >>>> street                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 거리를 나타내는 선입니다. |
| >>>>> ramp                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 일반적으로 톨게이트에 연결 하는 램프를 나타내는 선입니다. |
| >>>>> unpavedStreet                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Unpaved 거리를 나타내는 선입니다. |
| >>>> tollRoad                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 사용할 비용 금액을 나타내는 선입니다. |
| >>> railway                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 철로길. |
| >>> trail                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 공원 또는 자국 하이킹 코스 산책로. |
| > > > walkway                        | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | 상승 된 walkway. |
| >>> waterRoute                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 페리 경로 선. |
| >> water                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 물처럼 보이는 모든 것. 바다 및 물결이 포함됩니다. |
| >>> river                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 강, 물결 또는 기타 수로.  선 또는 다각형일 수 있으며 강이 아닌 수공간에 연결될 수 있습니다. |
| > routeMapElement                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 모든 라우팅 관련 항목입니다. |
| >> routeLine                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Route line 관련 항목입니다. |
| >>> drivingRoute                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 구동 경로를 나타내는 줄입니다. |
| >>> scenicRoute                    | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | Scenic 구동 경로를 나타내는 줄입니다. |
| >>> walkingRoute                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 탐색 경로를 나타내는 줄입니다. |
| > userMapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 모든 사용자 항목입니다. |
| >> userBillboard                   | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | 기본 [MapBillboard](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 인스턴스에 대한 스타일. |
| >> userLine                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 기본 [MapPolyline](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolyline) 인스턴스에 대한 스타일. |
| >> userModel3D                     | [MapElement3D](#mapelement3d) |      |  ✔   |  ✔   |  ✔   |  ✔   | 기본 [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) 인스턴스에 대한 스타일.  주로 renderAsSurface 설정에 사용됩니다. |
| >> userPoint                       | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 기본 [MapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon) 인스턴스에 대한 스타일. |

<a id="properties" />

## <a name="properties"></a>Properties

이 섹션은 각 항목에 사용할 수 있는 속성을 설명합니다.

<a id="version" />

### <a name="version-properties"></a>버전 속성

| 속성                     | type    | 설명                                                                                                           |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------|
| version                      | 문자열  | 대상 스타일 시트 버전입니다. 적용을 위해 사용됩니다. "1.0"이 기본값이며 기능 업데이트에 대해 "1.*"입니다. |

<a id="settings" />

### <a name="settings-properties"></a>설정 속성

| 속성                     | type    | 1703 | 1709 | 1803 | 1809 | 1903 | 설명 |
|------------------------------|---------|------|------|------|------|------|-------------|
| atmosphereVisible            | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 3D 컨트롤에 대기를 표시할지 여부를 나타내는 플래그. |
| buildingTexturesVisible      | Bool    |      |      |  ✔   |  ✔   |  ✔   | 텍스처가 있는 기호 3D 건물에 텍스처를 표시할지 여부를 나타내는 플래그입니다. |
| fogColor                     | 색   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 3D 컨트롤에 나타나는 거리 안개의 ARGB색 값. |
| glowColor                    | 색   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 레이블 빛 및 아이콘 빛에 적용할 수 있는 ARGB색 값. |
| imageFamily                  | 문자열  |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 이 스타일을 사용하도록 설정하는 이미지 세트의 이름입니다. 현실 세계의 기호에 따라 고정된 색상을 사용하는 기호의 경우 이 값을 *기본*으로 설정합니다. 색상표를 구성 가능한 색을 사용하는 기호의 경우 이 값을 *색상표*로 설정합니다. |
| landColor                    | 색   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 육지에 아무 것도 그리기 전의 육지의 ARGB 색 값. |
| logosVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | **조직** 속성을 가진 항목이 적절한 로고를 그리거나 일반 아이콘을 사용해야 하는지를 나타내는 플래그. |
| officialColorVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 공식 색 속성(예: 중국의 교통 라인)을 가진 항목이 해당 색을 그려야 하는지 여부를 나타내는 플래그. 예를 들어 흑백 지도의 경우 이 값을 해제합니다. |
| rasterRegionsVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 벡터 (일본 및 대한민국) 보다 더 나은 표현이 있는 래스터 영역을 그릴지 여부를 나타내는 플래그입니다. |
| shadedReliefVisible          | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 지도에 점진 음영을 그릴지 여부를 나타내는 플래그. |
| shadedReliefDarkColor        | 색   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 음영기복의 어두운 면의 색상.  알파 채널은 최대 알파 값을 나타냅니다. |
| shadedReliefLightColor       | 색   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 음영기복의 밝은 면의 색상.  알파 채널은 최대 알파 값을 나타냅니다. |
| shadowColor                  | 색   |      |      |      |  ✔   |  ✔   | 그림자를 사용 하는 아이콘 뒤의 그림자 색입니다. |
| spaceColor                   | 색   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 지도 주위의 영역에 대한 ARGB 색 값. |
| useDefaultImageColors        | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 이미지 색의 색상표 항목을 조회 하는 대신 SVG의 원래 색을 사용할지 여부를 나타내는 플래그입니다. |

<a id="mapelement" />

### <a name="mapelement-properties"></a>MapElement 요소

| 속성                     | type    | 1703 | 1709 | 1803 | 1809 | 1903 | 설명 |
|------------------------------|---------|------|------|------|------|------|-------------|
| backgroundScale              | 부동   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 아이콘의 배경 요소를 확장할 양.  예를 들어 기본값에 *1*, 대형의 두 배 값에 *2*를 사용합니다. |
| fillColor                    | 색   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 나눠진 경우 다각형, 포인트 아이콘의 배경, 선의 중앙을 채우는 데 사용되는 색. |
| fontFamily                   | 문자열  |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| fontWeight                   | String  |      |      |      |      |  ✔   | 스트로크의 밝기 또는 heaviness을 기준으로 하는 서체의 밀도입니다. "**Light**", "**Normal**", "**SemiBold**" 및 "**Bold**"를 설정할 수 있습니다. |
| iconColor                    | 색   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 포인트 아이콘의 중앙에 표시되는 문자 모양의 색. |
| iconScale                    | 부동   |      |  ✔   |  ✔   |  ✔   |  ✔   | 아이콘의 문자 모양을 확장할 양.  예를 들어 기본값에 *1*, 대형의 두 배 값에 *2*를 사용합니다. |
| labelColor                   | 색   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelOutlineColor            | 색   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelScale                   | 부동   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 기본 레이블 크기가 조정되는 양. 예를 들어 기본값에 *1*, 대형의 두 배 값에 *2*를 사용합니다. |
| labelVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| overwriteColor               | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | **FillColor**의 알파 값은 **StrokeColor**와 혼합되는 것이 아니라 이를 덮어씁니다. |
| scale                        | 부동   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 전체 포인트의 크기를 조정하는 정도. 예를 들어 기본값에 *1*, 대형의 두 배 값에 *2*를 사용합니다. |
| strokeColor                  | 색   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 다각형 주위의 윤곽선, 포인트 아이콘의 윤곽선, 선의 색에 사용할 색. |
| strokeWidthScale             | 부동   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 선의 스트로크의 크기를 조정하는 정도. 예를 들어 기본값에 *1*, 대형의 두 배 값에 *2*를 사용합니다. |
| 표시                      | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |

<a id="borderedmap" />

### <a name="borderedmapelement"></a>BorderedMapElement

이 속성 그룹은 [MapElement](#mapelement) 속성 그룹에서 상속됩니다.

| 속성                     | type    | 1703 | 1709 | 1803 | 1809 | 1903 | 설명 |
|------------------------------|---------|------|------|------|------|------|-------------|
| borderOutlineColor           | 색   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 채워진 다각형의 두 번째 또는 채워진 테두리 선의 색. |
| borderStrokeColor            | 색   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 채워진 다각형의 테두리의 기본 선 색상. |
| borderVisible                | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| borderWidthScale             | 부동   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 테두리의 스트로크 크기를 조정 하는 크기입니다. 예를 들어 기본값에 *1*, 대형의 두 배 값에 *2*를 사용합니다. |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>PointStyle 속성

이 속성 그룹은 [MapElement](#mapelement) 속성 그룹에서 상속됩니다.

| 속성                     | type    | 1703 | 1709 | 1803 | 1809 | 1903 | 설명 |
|------------------------------|---------|------|------|------|------|------|-------------|
| shadowVisible                | Bool    |      |      |      |      |  ✔   | 아이콘 그림자를 표시할지 여부를 나타내는 플래그입니다. |
| 모양-배경             | 문자열  |      |      |      |      |  ✔   | 아이콘의 배경으로 사용 하는 모양입니다 .이 셰이프는 여기에 있는 모양을 바꿉니다. |
| 셰이프-아이콘                   | 문자열  |      |      |      |      |  ✔   | 아이콘의 전경 문자 모양으로 사용할 셰이프입니다 .이 셰이프는 여기에 있는 모양을 바꿉니다. |
| stemAnchorRadiusScale        | 부동   |      |      |  ✔   |  ✔   |  ✔   | 아이콘 스템의 앵커 지점을 확장할 양.  예를 들어 기본값에 *1*, 대형의 두 배 값에 *2*를 사용합니다. |
| stemColor                    | 색   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 3D 모드에서 해당 아이콘의 하단에서 나오는 줄기의 색. |
| stemHeightScale              | 부동   |      |      |  ✔   |  ✔   |  ✔   | 아이콘 스템의 길이를 확장할 양.  예를 들어 기본값에 *1*, 길이의 두 배 값에 *2*를 사용합니다. |
| stemOutlineColor             | 색   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 3D 모드에서 해당 아이콘의 하단에서 나오는 줄기 주변의 윤곽선 색. |
| stemWidthScale               | 부동   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 아이콘 스템의 너비를 확장할 양.  예를 들어 기본값에 *1*, 길이의 두 배 값에 *2*를 사용합니다. |

<a id="mapelement3d" />

### <a name="mapelement3d"></a>MapElement3D

이 속성 그룹은 [MapElement](#mapelement) 속성 그룹에서 상속됩니다.

| 속성                     | type    | 1703 | 1709 | 1803 | 1809 | 1903 | 설명 |
|------------------------------|---------|------|------|------|------|------|------------|
| renderAsSurface              | Bool    |      |      |  ✔   |  ✔   |  ✔   | 3D 모델이 땅에 대한 깊이 페이드 인 없이 건물 같이 렌더링할 수 있음을 나타내는 플래그입니다. |
