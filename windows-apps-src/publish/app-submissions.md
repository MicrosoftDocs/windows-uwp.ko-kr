---
author: jnHs
Description: Once you've created your app by reserving a name, you can start working on getting it published. The first step is to create a submission.
title: 앱 제출
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: 검사 목록, Windows, UWP, 제출, 제출하다, 게임, 앱, 제출하기
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 55235c78df29513e8d7b28e7643aec5c3a256f1d
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6857335"
---
# <a name="app-submissions"></a>앱 제출


[이름을 예약하여 앱을 만들면](create-your-app-by-reserving-a-name.md) 앱 게시 작업을 시작할 수 있습니다. 첫 번째 단계에서는 **제출**을 만듭니다.

앱이 완성되고 게시할 준비가 되면 제출을 시작하거나 한 줄 코드를 작성하기 전이라도 정보 입력을 시작할 수 있습니다. 다시 하 고 되었고 필요할 때마다 작업을 업데이트 하면 제출에 저장 됩니다.

> [!NOTE]
> Microsoft Store에 앱을 제출 하려면 [파트너 센터](https://partner.microsoft.com/dashboard) 에서 활성 [개발자 계정이](http://go.microsoft.com/fwlink/p/?LinkId=615100) 있어야 합니다.

앱이 게시 된 후에 파트너 센터에서 또 다른 제출을 만들어서 업데이트 된 버전을 게시할 수 있습니다. 새 패키지를 업로드하든, 가격이나 범주와 같은 세부 정보를 변경하든, 새 제출을 만드는 방법으로 필요한 변경 내용을 적용하고 게시할 수 있습니다. 게시 된 앱에 대 한 새 제출을 만들려면 **개요** 페이지에 표시 된 최근 제출 옆에 있는 **업데이트** 를 클릭 합니다. 수도 있습니다 [스토어에서 앱을 제거](guidance-for-app-package-management.md#removing-an-app-from-the-store) 이렇게 (및 다음 사용할 수 있도록 나중에 다시 원할 경우) 하는 경우.

> [!NOTE]
> 설명서의이 섹션에는 파트너 센터에서 앱 제출을 만드는 방법을 설명 합니다. 또는 [Microsoft Store 제출 API](../monetize/create-and-manage-submissions-using-windows-store-services.md)를 사용하여 앱 제출을 자동화할 수 있습니다.

> [!IMPORTANT]
> 새로 만든 제품 2018 년 10 월 31 일 기준 Windows 8.x/Windows를 대상으로 하는 패키지를 포함할 수 없습니다 Phone 8.x 이전 버전입니다. 자세한 내용은이 [블로그 게시물](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/#SzKghBbqDMlmAO4c.97)을 참조 하세요.

## <a name="app-submission-checklist"></a>앱 제출 검사 목록

앱 제출을 만들 때 제공할 수 있는 세부 정보 및 추가 정보 링크는 다음과 같습니다.

제공하거나 지정해야 하는 항목은 다음과 같습니다. 일부 영역은 옵션이거나 원하는 대로 변경할 수 있는 기본값이 제공되어 있습니다. 여기에 나열 된 순서로 이러한 섹션에 사용할 필요가 없습니다.

### <a name="pricing-and-availability-page"></a>가격 책정 및 가용성 페이지
| 필드 이름                    | 참고                                       | 자세한 정보                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **시장**                   | 기본값: 모든 가능한 시장  | [가격 책정 및 지역/국가 선택 정의](define-pricing-and-market-selection.md)         |
| **대상 그룹**                | 기본값: 공중 대상 | [대상 그룹](choose-visibility-options.md#audience) |
| **검색 기능**                | 기본값: 이 앱을 Store에서 다운로드해 사용할 수 있도록 설정 | [검색 기능](choose-visibility-options.md#discoverability) |
| **일정**                  | 기본값: 최대한 빨리 게시        | [정확한 릴리스 일정 구성](configure-precise-release-scheduling.md) |
| **기본 가격**                | 필수                                    | [앱 가격 설정 및 예약](set-and-schedule-app-pricing.md)              |
| **무료 평가판**                | 기본값: 무료 평가판 없음                      | [무료 평가판](set-app-pricing-and-availability.md#free-trial)              |
| **할인 판매 가격 책정**              | 옵션                                    | [앱 및 추가 기능 판매](put-apps-and-add-ons-on-sale.md)           |
| **조직 라이선스**    | 기본값: 조직에서 대량 구입할 수 있음 | [조직 라이선스 옵션](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>속성 페이지

| 필드 이름                    | 참고                                       | 자세한 정보                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **범주 및 하위 범주**  | 필수                                    | [범주 및 하위 범주 테이블](category-and-subcategory-table.md)       |
| **개인 정보 취급 방침 URL**            | 많은 앱에 필요. [앱 개발자 계약](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) 및 [Microsoft Store 정책](https://docs.microsoft.com/en-us/legal/windows/agreements/store-policies#105-personal-information)을 참조하세요. | [개인 정보 취급 방침 URL](enter-app-properties.md#privacy-policy-url)        |
| **웹 사이트**                   | 선택 사항                                    | [웹 사이트](enter-app-properties.md#website)                   |
| **지원 연락처 정보**      | Xbox에서 제품을 사용할 수 있는 경우에는 필수이지만, 그 외의 경우에는 선택 사항입니다(그러나 권장 사항).                                   | [지원 연락처 정보](enter-app-properties.md#support-contact-info)              |
| **게임 설정**             | 선택 사항(게임에만 해당)         | [게임 설정](enter-app-properties.md#game-settings) |
| **디스플레이 모드**             | 선택 사항                   | [디스플레이 모드](enter-app-properties.md#display-mode) |
| **제품 선언**          | 기본값: 고객이 디바이스 또는 이동식 저장소를 대체하기 위해 이 앱을 설치할 수 있음, Windows에서는 이 앱의 데이터를 OneDrive에 대한 자동 백업에 포함할 수 있음 | [제품 선언](app-declarations.md) |
| **시스템 요구 사항**      | 옵션                                    | [시스템 요구 사항](enter-app-properties.md#system-requirements)      |

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
| **디바이스 패밀리 가용성** | 기본값: 패키지에 따름       | [디바이스 패밀리 가용성](device-family-availability.md) |
| **점진적 패키지 배포**   | 옵션(업데이트에만 해당)            | [점진적 패키지 배포](gradual-package-rollout.md) |
| **필수 업데이트**          | 옵션(업데이트에만 해당)            | [필수 업데이트](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>스토어 목록

앱에서 지원하는 하나 이상의 언어에 대한 필수 정보가 모두 필요합니다. 앱에서 지원하는 모든 언어로 [Store 목록](create-app-store-listings.md)을 제공하는 것이 좋으며 [추가 언어로 Store 목록을 제공](create-app-store-listings.md#store-listing-languages)할 수도 있습니다. 동일한 제품에 대해 여러 목록을 손쉽게 관리할 수 있도록 [Store 목록 가져오기 및 내보내기](import-and-export-store-listings.md)가 가능합니다.

| 필드 이름                    | 참고                                       | 자세한 정보                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **설명**               | 필수                                    | [호소력 있는 앱 설명 작성](write-a-great-app-description.md) |
| **이 버전의 새로운 기능**   | 선택 사항                                 | [릴리스 정보](create-app-store-listings.md#whats-new-in-this-version)       |
| **앱 기능**              | 선택 사항                                    | [제품 기능](create-app-store-listings.md#product-features)         |
| **스크린샷**               | 필수 사항(최소 하나의 스크린샷, 4개 이상을 권장)          | [스크린샷](app-screenshots-and-images.md#screenshots)          |
| **Microsoft Store 로고**               | 권장 사항(일부 OS 버전에서는 필수 사항) | [Microsoft Store 로고](app-screenshots-and-images.md#store-logos)             |
| **예고편**                  | 선택 사항                                    | [예고편](app-screenshots-and-images.md#trailers)                | 
| **Windows 10 및 Xbox 이미지(16:9 수퍼 히어로 아트)**     | 권장 사항        | [Windows 10 및 Xbox 이미지 (16:9 수퍼 히어로 아트)
] (app-screenshots-and-images.md#windows-10-and-xbox-image-169-super-hero-art) |
| **Xbox 이미지**     | Xbox에 게시 하는 경우에 최적의 표시를 위해 필요 합니다.        | [Xbox 이미지
] (앱-스크린샷을-및-images.md #xbox 이미지) |
| **보조 필드**  | 선택 사항                                    | [보조 필드](create-app-store-listings.md#supplemental-fields) 
| **검색어**              | 선택 사항                                    | [검색어](create-app-store-listings.md#search-terms)         |
| **저작권 및 상표 정보** | 선택 사항                                 | [저작권 및 상표 정보](create-app-store-listings.md#copyright-and-trademark-info) |
| **추가 사용 조건**  | 선택 사항                                    | [추가 사용 조건](create-app-store-listings.md#additional-license-terms) |
| **개발자**              | 선택 사항                                    | [개발자](create-app-store-listings.md#developed-by)                   |


<span/>

### <a name="submission-options-page"></a>제출 옵션 페이지

| 필드 이름                    | 참고                                       | 자세한 정보                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **게시 고정 옵션**     | 기본값: 인증을 통과하는 즉시(또는 "일정" 섹션에서 선택한 날짜에 따라) 이 제출을 게시      | [게시 고정 옵션](manage-submission-options.md#publishing-hold-options)    
| **인증에 대한 참고 사항**     | 권장 사항          | [인증에 대한 참고 사항](notes-for-certification.md)             |
| **제한된 접근 권한 값**     | 제품 모든 [제한 된 접근 권한 값](../packaging/app-capability-declarations.md#restricted-capabilities) 을 선언 하는 경우 필요 합니다.    | [제한된 접근 권한 값](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> LOB(기간 업무) 앱을 엔터프라이즈에 직접 게시하는 방법은 [엔터프라이즈에 LOB 앱 배포](distribute-lob-apps-to-enterprises.md)를 참조하세요.
