---
author: jnHs
Description: "이름을 예약하여 앱을 만들면 앱 게시 작업을 시작할 수 있습니다. 첫 번째 단계에서는 제출을 만듭니다."
title: "앱 제출"
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: "검사 목록"
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: df66981ae8355ea62128a881f02fd6fb891ffb30
ms.lasthandoff: 02/07/2017

---

# <a name="app-submissions"></a>앱 제출


[이름을 예약하여 앱을 만들면](create-your-app-by-reserving-a-name.md) 앱 게시 작업을 시작할 수 있습니다. 첫 번째 단계에서는 **제출**을 만듭니다.

앱이 완성되고 게시할 준비가 되면 제출을 시작하거나 한 줄 코드를 작성하기 전이라도 정보 입력을 시작할 수 있습니다. 제출은 대시보드에 저장되므로 필요할 때마다 작업을 할 수 있습니다.

앱이 게시된 후에는 대시보드에서 또 다른 제출을 만들어서 업데이트된 버전을 게시할 수 있습니다. 새 패키지를 업로드하든, 가격이나 범주와 같은 세부 정보를 변경하든, 새 제출을 만드는 방법으로 필요한 변경 내용을 적용하고 게시할 수 있습니다. 앱에 대한 새 제출을 만들려면 앱 개요 페이지에 표시된 최근 제출 옆의 **업데이트**를 클릭합니다.

> **참고**&nbsp;&nbsp;설명서의 이 섹션에서는 개발자 센터 대시보드에서 앱 제출을 만드는 방법을 설명합니다. 또는 [Windows 스토어 제출 API](../monetize/create-and-manage-submissions-using-windows-store-services.md)를 사용하여 앱 제출을 자동화할 수 있습니다.

## <a name="app-submission-checklist"></a>앱 제출 검사 목록


앱 제출을 만들 때 제공할 수 있는 세부 정보 및 추가 정보 링크는 다음과 같습니다.

제공하거나 지정해야 하는 항목은 다음과 같습니다. 일부 영역은 옵션이거나 원하는 대로 변경할 수 있는 기본값이 제공되어 있습니다.

### <a name="pricing-and-availability-page"></a>가격 책정 및 가용성 페이지
| 필드 이름                    | 참고                                       | 자세한 정보                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **기본 가격**                | 필수                                    | [기본 가격](set-app-pricing-and-availability.md#base-price)              |
| **무료 평가판**                | 기본값: 무료 평가판 없음                      | [평가판 및 앱에서 바로 구매 추가](https://msdn.microsoft.com/library/windows/apps/jj193599)  |
| **지역/국가 및 사용자 지정 가격** | 기본값: 모든 가능한 지역/국가, 사용자 지정 가격 없음 | [가격 책정 및 지역/국가 선택 정의](define-pricing-and-market-selection.md)              |
| **할인 판매 가격 책정**              | 옵션                                    | [앱 및 추가 기능 판매](put-apps-and-add-ons-on-sale.md)                                       |
| **배포 및 표시** | 기본값: 이 앱을 스토어에서 다운로드할 수 있도록 설정 | [배포 및 표시](set-app-pricing-and-availability.md#distribution-and-visibility) |
| **조직 라이선스**    | 기본값: 조직에서 대량 구입할 수 있음 | [조직 라이선스 옵션](organizational-licensing.md)                        |
| **게시 날짜**                | 기본값: 최대한 빨리 게시      | [게시 날짜](set-app-pricing-and-availability.md#publish-date)          |

<span/>

### <a name="app-properties-page"></a>앱 속성 페이지

| 필드 이름                    | 참고                                       | 자세한 정보                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **범주 및 하위 범주**  | 필수                                    | [범주 및 하위 범주 테이블](category-and-subcategory-table.md)       |
| **시스템 요구 사항**      | 옵션                                    | [시스템 요구 사항](enter-app-properties.md#system-requirements)      |
| **앱 선언**          | 기본값: 고객이 장치 또는 이동식 저장소를 대체하기 위해 이 앱을 설치할 수 있음, Windows에서는 이 앱의 데이터를 OneDrive에 대한 자동 백업에 포함할 수 있음 | [앱 선언](app-declarations.md) |

<span/>

### <a name="age-ratings-page"></a>연령별 등급 페이지

| 필드 이름                    | 참고                                       | 자세한 정보                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **연령별 등급**               | 필수                                    | [연령별 등급](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>패키지 페이지

| 필드 이름                    | 참고                                  | 자세한 정보                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **패키지 업로드 제어**    | 필수(패키지 하나 이상)        | [앱 패키지 업로드](upload-app-packages.md) |
| **장치 패밀리 가용성** | 기본값: 패키지에 따름       | [장치 패밀리 가용성](upload-app-packages.md#device-family-availability) |
| **점진적 패키지 배포**   | 옵션(업데이트에만 해당)            | [점진적 패키지 배포](gradual-package-rollout.md) |
| **필수 업데이트**          | 옵션(업데이트에만 해당)            | [필수 업데이트](upload-app-packages.md#mandatory-update)

<span/>

### <a name="store-listings"></a>스토어 목록

앱에서 지원하는 하나 이상의 언어에 대한 필수 정보가 모두 필요합니다. 앱이 지원하는 모든 언어로 [스토어 목록](create-app-store-listings.md)을 제공하는 것이 좋으며 [추가 언어로 스토어 목록을 제공](create-app-store-listings.md#store-listing-languages)할 수도 있습니다.

| 필드 이름                    | 참고                                       | 자세한 정보                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **설명**               | 필수                                    | [호소력 있는 앱 설명 쓰기](write-a-great-app-description.md) |
| **릴리스 정보**             | 옵션                                    | [릴리스 정보](create-app-store-listings.md#release-notes)         |
| **스크린샷**               | 필수(스크린샷 하나 이상)          | [앱 스크린샷 및 이미지](app-screenshots-and-images.md)       |
| **앱 타일 아이콘**             | 옵션이지만, Windows Phone 8.1 이하의 경우 매우 권장 | [앱 타일 아이콘](create-app-store-listings.md#app-tile-icon) |
| **판촉 아트워크**       | 옵션                                    | [앱 스크린샷 및 이미지](app-screenshots-and-images.md)       |
| **앱 기능**              | 옵션                                    | [기능](create-app-store-listings.md#app-features)               |
| **추가 시스템 요구 사항**      | 옵션                                    | [추가 시스템 요구 사항](create-app-store-listings.md#additional-system-requirements) |
| **키워드**                  | 옵션                                    | [키워드](create-app-store-listings.md#keywords)                   |
| **저작권 및 상표 정보** | 옵션                                 | [저작권 및 상표 정보](create-app-store-listings.md#copyright-and-trademark-info) |
| **추가 사용 조건**  | 옵션                                    | [추가 사용 조건](create-app-store-listings.md#additional-license-terms) |
| **웹 사이트**                   | 옵션                                    | [웹 사이트](create-app-store-listings.md#website)                     |
| **지원 연락처 정보**      | 옵션                                    | [지원 연락처 정보](create-app-store-listings.md)                |
| **개인 정보 취급 방침**            | 일부 앱에 필요. [앱 개발자 계약](https://msdn.microsoft.com/library/windows/apps/hh694058) 및 [Windows 스토어 정책](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_5_1) 참조 | [개인 정보 취급 방침](create-app-store-listings.md#privacy-policy) |
| **플랫폼별 스토어 목록** | 옵션                               | [플랫폼별 스토어 목록 만들기](create-platform-specific-store-listings.md) |

<span/>

### <a name="notes-for-certification-page"></a>인증 페이지에 대한 참고 사항

| 필드 이름                    | 참고                                       | 자세한 정보                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **참고**                     | 옵션                                    | [인증에 대한 참고 사항](notes-for-certification.md)             |

<span/>

**참고**&nbsp;&nbsp;LOB(기간 업무) 앱을 엔터프라이즈에 직접 게시하는 방법은 [엔터프라이즈에 LOB 앱 배포](distribute-lob-apps-to-enterprises.md)를 참조하세요.

