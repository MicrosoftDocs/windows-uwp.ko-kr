---
Description: 이름을 예약하여 앱을 만들면 앱 게시 작업을 시작할 수 있습니다. 첫 번째 단계에서는 제출을 만듭니다.
title: 앱 제출
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: 검사 목록, Windows, UWP, 제출, 제출하다, 게임, 앱, 제출하기
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2fe5d44823821208a2384aec9d66037c4da3865f
ms.sourcegitcommit: 4aef8c01ba9321401d5729a1ec6d46452ee76faf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67468924"
---
# <a name="app-submissions"></a>앱 제출


[이름을 예약하여 앱을 만들면](create-your-app-by-reserving-a-name.md) 앱 게시 작업을 시작할 수 있습니다. 첫 번째 단계에서는 **제출**을 만듭니다.

앱이 완성되고 게시할 준비가 되면 제출을 시작하거나 한 줄 코드를 작성하기 전이라도 정보 입력을 시작할 수 있습니다. 업데이트를 다시 방문 하 고 작업할 준비가 될 때마다 수 있도록 저장 됩니다.

> [!NOTE]
> 활성 있어야 [개발자 계정](https://go.microsoft.com/fwlink/p/?LinkId=615100) 에 [파트너 센터](https://partner.microsoft.com/dashboard) Microsoft Store 앱을 제출 하기 위해.

앱을 게시 한 후에 파트너 센터에서 다른 제출을 만들어 업데이트 된 버전을 게시할 수 있습니다. 새 패키지를 업로드하든, 가격이나 범주와 같은 세부 정보를 변경하든, 새 제출을 만드는 방법으로 필요한 변경 내용을 적용하고 게시할 수 있습니다. 게시 된 앱에 대 한 새 전송을 만들려면 **업데이트** 에 표시 된 가장 최근 제출 옆에 있는 해당 **개요** 페이지입니다. 할 수도 있습니다 [스토어에서 앱을 제거](guidance-for-app-package-management.md#removing-an-app-from-the-store) 이렇게 (및 다음 사용할 수 있도록 나중에 다시 원하는 경우) 해야 합니다.

> [!NOTE]
> 설명서의이 섹션에는 파트너 센터에서는 응용 프로그램 제출을 만드는 방법을 설명 합니다. 또는 [Microsoft Store 제출 API](../monetize/create-and-manage-submissions-using-windows-store-services.md)를 사용하여 앱 제출을 자동화할 수 있습니다.

> [!IMPORTANT]
> 2018 년 10 월 31 일부 터 새로 만든 제품 Windows 8.x/Windows를 대상으로 하는 패키지를 포함할 수 없습니다 Phone 8.x 또는 이전 버전입니다. 자세한 내용은 참조 [블로그 게시물](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)합니다.

## <a name="app-submission-checklist"></a>앱 제출 검사 목록

앱 제출을 만들 때 제공할 수 있는 세부 정보 및 추가 정보 링크는 다음과 같습니다.

제공하거나 지정해야 하는 항목은 다음과 같습니다. 일부 영역은 옵션이거나 원하는 대로 변경할 수 있는 기본값이 제공되어 있습니다. 여기에 나열 된 순서 대로 이러한 섹션에 사용할 필요가 없습니다.

### <a name="pricing-and-availability-page"></a>가격 책정 및 가용성 페이지
| 필드 이름                    | 참고                                       | 자세한 정보                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **시장**                   | Default: 모든 가능한 시장  | [가격 책정 및 시장 선택 정의](define-pricing-and-market-selection.md)         |
| **대상 그룹**                | Default: 공중 대상 | [대상 그룹](choose-visibility-options.md#audience) |
| **검색 기능**                | Default: 저장소에서 사용 가능 하 고 검색 가능한이 앱 만들기 | [검색 기능](choose-visibility-options.md#discoverability) |
| **일정**                  | Default: 가능한 한 빨리 릴리스        | [정확 하 게 릴리스 일정 구성](configure-precise-release-scheduling.md) |
| **기본 가격**                | 필수                                    | [집합 및 일정 앱 가격](set-and-schedule-app-pricing.md)              |
| **무료 평가판**                | Default: 무료 평가판 없음                      | [무료 평가판](set-app-pricing-and-availability.md#free-trial)              |
| **할인 판매 가격 책정**              | 선택 사항                                    | [Put apps and add-ons on sale](put-apps-and-add-ons-on-sale.md)(앱 및 추가 기능 판매)           |
| **조직의 라이선스**    | Default: 조직에서 볼륨 취득 허용 | [조직의 라이선스 옵션](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>속성 페이지

| 필드 이름                    | 참고                                       | 자세한 정보                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **범주 및 하위 범주**  | 필수                                    | [Category 및 subcategory 테이블](category-and-subcategory-table.md)       |
| **개인정보 처리 방침 URL**            | 많은 앱에 필요. [앱 개발자 계약](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) 및 [Microsoft Store 정책](store-policies.md#105-personal-information)을 참조하세요. | [개인정보 처리 방침 URL](enter-app-properties.md#privacy-policy-url)        |
| **웹 사이트**                   | 선택 사항                                    | [웹 사이트](enter-app-properties.md#website)                   |
| **지원 연락처 정보**      | Xbox에서 제품을 사용할 수 있는 경우에는 필수이지만, 그 외의 경우에는 선택 사항입니다(그러나 권장 사항).                                   | [지원 연락처 정보](enter-app-properties.md#support-contact-info)              |
| **게임 설정**             | 선택 사항(게임에만 해당)         | [게임 설정](enter-app-properties.md#game-settings) |
| **디스플레이 모드**             | 선택 사항                   | [디스플레이 모드](enter-app-properties.md#display-mode) |
| **제품 선언**          | Default: 고객은 대체 드라이브 또는 이동식 저장소에이 앱을 설치할 수 있습니다. Windows는 OneDrive에 자동 백업에서이 앱의이 데이터를 포함할 수 있습니다. | [제품 선언](app-declarations.md) |
| **시스템 요구 사항**      | 선택 사항                                    | [시스템 요구 사항](enter-app-properties.md#system-requirements)      |

<span/>

### <a name="age-ratings-page"></a>연령별 등급 페이지

| 필드 이름                    | 참고                                       | 자세한 정보                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **연령별 등급**               | 필수                                    | [연령별 등급](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>패키지 페이지

| 필드 이름                    | 참고                                  | 자세한 정보                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **패키지 업로드 컨트롤**    | 필수(패키지 하나 이상)        | [앱 패키지 업로드](upload-app-packages.md) |
| **장치 패밀리 가용성** | 기본값: 패키지에 따름       | [장치 패밀리 가용성](device-family-availability.md) |
| **패키지를 점진적으로 롤아웃**   | 옵션(업데이트에만 해당)            | [패키지를 점진적으로 롤아웃](gradual-package-rollout.md) |
| **필수 업데이트**          | 옵션(업데이트에만 해당)            | [필수 업데이트](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>스토어 목록

앱에서 지원하는 하나 이상의 언어에 대한 필수 정보가 모두 필요합니다. 앱이 지원하는 모든 언어로 [스토어 목록](create-app-store-listings.md)을 제공하는 것이 좋으며 [추가 언어로 스토어 목록을 제공](create-app-store-listings.md#store-listing-languages)할 수도 있습니다. 동일한 제품에 대해 여러 목록을 손쉽게 관리할 수 있도록 [Store 목록 가져오기 및 내보내기](import-and-export-store-listings.md)가 가능합니다.

| 필드 이름                    | 참고                                       | 자세한 정보                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **설명**               | 필수                                    | [유용한 앱 설명 작성](write-a-great-app-description.md) |
| **이 버전의 새로운 기능**   | 선택 사항                                 | [릴리스 정보](create-app-store-listings.md#whats-new-in-this-version)       |
| **앱 기능**              | 선택 사항                                    | [제품 기능](create-app-store-listings.md#product-features)         |
| **스크린 샷**               | 필수 사항(최소 하나의 스크린샷, 4개 이상을 권장)          | [스크린 샷](app-screenshots-and-images.md#screenshots)          |
| **스토어 로고**               | 권장 사항(일부 OS 버전에서는 필수 사항) | [스토어 로고](app-screenshots-and-images.md#store-logos)             |
| **트레일러**                  | 선택 사항                                    | [트레일러](app-screenshots-and-images.md#trailers)                | 
| **Windows 10 및 Xbox 이미지 (16:9 슈퍼 hero 아트)**     | 권장        | [Windows 10 및 Xbox 이미지 (16:9 슈퍼 hero 아트)
](app-screenshots-and-images.md#windows-10-and-xbox-image-169-super-hero-art) |
| **Xbox 이미지**     | Xbox에 게시 하는 경우 적절 하 게 표시할에 필요한        | [Xbox 이미지
](app-screenshots-and-images.md#xbox-images) |
| **추가 필드**  | 선택 사항                                    | [추가 필드](create-app-store-listings.md#supplemental-fields) 
| **검색 용어**              | 선택 사항                                    | [검색 용어](create-app-store-listings.md#search-terms)         |
| **저작권 및 상표 정보** | 선택 사항                                 | [저작권 및 상표 정보](create-app-store-listings.md#copyright-and-trademark-info) |
| **추가 사용 조건**  | 선택 사항                                    | [추가 사용 조건](create-app-store-listings.md#additional-license-terms) |
| **개발**              | 선택 사항                                    | [개발](create-app-store-listings.md#developed-by)                   |


<span/>

### <a name="submission-options-page"></a>제출 옵션 페이지

| 필드 이름                    | 참고                                       | 자세한 정보                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **게시 보존 옵션**     | Default: 통과 인증 (또는 일정 섹션에서 선택한 날짜 당) 즉시이 제출 게시      | [게시 보존 옵션](manage-submission-options.md#publishing-hold-options)    
| **인증에 대 한 정보**     | 권장          | [인증에 대 한 정보](notes-for-certification.md)             |
| **제한 된 기능**     | 제품 하나를 선언 하는 경우 필요한 [기능 제한](../packaging/app-capability-declarations.md#restricted-capabilities)    | [제한 된 기능](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> LOB(기간 업무) 앱을 엔터프라이즈에 직접 게시하는 방법은 [엔터프라이즈에 LOB 앱 배포](distribute-lob-apps-to-enterprises.md)를 참조하세요.
