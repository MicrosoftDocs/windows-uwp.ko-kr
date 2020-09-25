---
Description: 이름을 예약하여 앱을 만들면 앱 게시 작업을 시작할 수 있습니다. 첫 번째 단계는 제출을 만드는 것입니다.
title: 앱 제출
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: 검사 목록, windows, uwp, 제출, 제출, 게임, 앱, 제출
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 99b4d7412727e5f195c32d3f3c21fe82b284e658
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219976"
---
# <a name="app-submissions"></a>앱 제출


[이름을 예약 하 여 앱을 만들었으면](create-your-app-by-reserving-a-name.md)게시 작업을 시작할 수 있습니다. 첫 번째 단계는 **제출을**만드는 것입니다.

앱이 완료 되 고 게시할 준비가 되 면 제출을 시작 하거나 한 줄의 코드를 작성 하기 전에도 정보 입력을 시작할 수 있습니다. 전송에 대 한 업데이트는 저장 되므로 준비가 되 면 언제 든 지 해당 업데이트에 대해 작업을 수행 하 여 작업할 수 있습니다.

> [!NOTE]
> Microsoft Store에 앱을 제출 하려면 [파트너 센터](https://partner.microsoft.com/dashboard) 에 활성 [개발자 계정이](https://developer.microsoft.com/store/register) 있어야 합니다.

앱이 게시 되 면 파트너 센터에서 다른 제출을 만들어 업데이트 된 버전을 게시할 수 있습니다. 새 제출을 만들면 새 패키지를 업로드 하거나 가격 또는 범주와 같은 세부 정보를 변경 하 든 상관 없이 필요한 변경 내용을 만들고 게시할 수 있습니다. 게시 된 앱에 대 한 새 제출을 만들려면 **개요** 페이지에 표시 된 최신 제출 옆의 **업데이트** 를 클릭 합니다. 이 작업을 수행 해야 하는 경우 [스토어에서 앱을 제거한](guidance-for-app-package-management.md#removing-an-app-from-the-store) 후 나중에 다시 사용할 수 있도록 설정 해야 할 수도 있습니다.

> [!NOTE]
> 설명서의이 섹션에서는 파트너 센터에서 앱 제출을 만드는 방법을 설명 합니다. 또는 [Microsoft Store 제출 API](../monetize/create-and-manage-submissions-using-windows-store-services.md) 를 사용 하 여 앱 제출을 자동화할 수 있습니다.

> [!IMPORTANT]
> Windows Phone .x SDK를 사용 하 여 빌드된 새 XAP 패키지는 더 이상 업로드할 수 없습니다. 이미 XAP 패키지를 사용 하 여 저장 된 앱은 Windows 10 Mobile 장치에서 계속 작동 합니다. 자세한 내용은이 [블로그 게시물](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)을 참조 하세요.

## <a name="app-submission-checklist"></a>앱 제출 검사 목록

자세한 정보에 대 한 링크를 사용 하 여 앱 제출을 만들 때 제공할 수 있는 세부 정보는 다음과 같습니다.

제공 하거나 지정 해야 하는 항목은 아래에 나와 있습니다. 일부 영역은 선택 사항이 며 원하는 대로 변경할 수 있는 기본값을 제공 합니다. 다음 섹션에서 여기에 나열된 순서대로 작업을 수행할 필요는 없습니다.

### <a name="pricing-and-availability-page"></a>가격 책정 및 가용성 페이지
| 필드 이름                    | 참고                                       | 추가 정보                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **시장**                   | 기본값: 모든 가능한 시장  | [가격 책정 및 시장 선택 정의](./define-market-selection.md)         |
| **대상**                | 기본값: 공용 사용자 | [대상](choose-visibility-options.md#audience) |
| **검색 기능**                | 기본값: 스토어에서이 앱을 사용 가능 하 게 설정 하 고 검색 가능 | [검색 기능](choose-visibility-options.md#discoverability) |
| **일정**                  | 기본값: 가능한 한 빨리 릴리스        | [정확한 릴리스 예약 구성](configure-precise-release-scheduling.md) |
| **기본 가격**                | 필수                                    | [앱 가격 설정 및 예약](set-and-schedule-app-pricing.md)              |
| **무료 평가판**                | 기본값: 무료 평가판 없음                      | [무료 평가판](set-app-pricing-and-availability.md#free-trial)              |
| **판매 가격**              | 선택 사항                                    | [앱 및 추가 기능 판매](put-apps-and-add-ons-on-sale.md)           |
| **조직 라이선스**    | 기본값: 조직에서 볼륨 획득 허용 | [조직 라이선스 옵션](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>속성 페이지

| 필드 이름                    | 참고                                       | 추가 정보                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **범주 및 하위 범주**  | 필수                                    | [범주 및 하위 범주 테이블](category-and-subcategory-table.md)       |
| **개인정보처리방침 URL**            | 많은 앱에 필요 합니다. [앱 개발자 계약](/legal/windows/agreements/app-developer-agreement) 및 [Microsoft Store 정책](store-policies.md#105-personal-information) 참조 | [개인정보처리방침 URL](enter-app-properties.md#privacy-policy-url)        |
| **웹 사이트**                   | 선택 사항                                    | [웹 사이트](enter-app-properties.md#website)                   |
| **지원 연락처 정보**      | Xbox에서 제품을 사용할 수 있는 경우에 필요 합니다. 그렇지 않으면 선택 사항 (권장 됨)                                   | [지원 연락처 정보](enter-app-properties.md#support-contact-info)              |
| **게임 설정**             | 선택 사항 (게임에만 적용 됨)         | [게임 설정](enter-app-properties.md#game-settings) |
| **디스플레이 모드**             | 선택 사항                   | [디스플레이 모드](enter-app-properties.md#display-mode) |
| **제품 선언**          | 기본값: 고객이 대체 드라이브나 이동식 저장소에이 앱을 설치할 수 있습니다. Windows는이 앱의 데이터를 OneDrive에 자동 백업에 포함할 수 있습니다. | [제품 선언](./product-declarations.md) |
| **시스템 요구 사항**      | 선택 사항                                    | [시스템 요구 사항](enter-app-properties.md#system-requirements)      |

<span/>

### <a name="age-ratings-page"></a>연령 등급 페이지

| 필드 이름                    | 참고                                       | 추가 정보                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **연령별 등급**               | 필수                                    | [연령별 등급](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>패키지 페이지

| 필드 이름                    | 참고                                  | 추가 정보                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **패키지 업로드 제어**    | 필수 (하나 이상의 패키지)        | [앱 패키지 업로드](upload-app-packages.md) |
| **디바이스 패밀리 가용성** | 기본값: 패키지 기반       | [디바이스 패밀리 가용성](device-family-availability.md) |
| **점진적 패키지 배포**   | 선택 사항 (업데이트에만 해당)            | [점진적 패키지 배포](gradual-package-rollout.md) |
| **필수 업데이트**          | 선택 사항 (업데이트에만 해당)            | [필수 업데이트](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>스토어 목록

앱에서 지 원하는 언어 중 하나 이상에 필요한 모든 정보가 필요 합니다. 앱에서 지 원하는 모든 언어에 [매장 목록을](create-app-store-listings.md) 제공 하는 것이 좋으며, [추가 언어에서 상점 목록을 제공할](create-app-store-listings.md#store-listing-languages)수도 있습니다. 동일한 제품에 대 한 여러 목록을 더 쉽게 관리할 수 있도록 [저장소 목록을 가져오고 내보낼](import-and-export-store-listings.md)수 있습니다.

| 필드 이름                    | 참고                                       | 추가 정보                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **설명**               | 필수                                    | [호소력 있는 앱 설명 쓰기](write-a-great-app-description.md) |
| **이 버전의 새로운 기능**   | 선택 사항                                 | [릴리스 정보](create-app-store-listings.md#whats-new-in-this-version)       |
| **앱 기능**              | 선택 사항                                    | [제품 기능](create-app-store-listings.md#product-features)         |
| **스크린샷**               | 필수 (하나 이상의 스크린샷, 4 개 이상 권장)          | [스크린샷](app-screenshots-and-images.md#screenshots)          |
| **스토어 로고**               | 바람직하지 일부 OS 버전에 필요 합니다. | [스토어 로고](app-screenshots-and-images.md#store-logos)             |
| **트레일러**                  | 선택 사항                                    | [트레일러](app-screenshots-and-images.md#trailers)                | 
| **Windows 10 및 Xbox 이미지 (16:9 슈퍼 주인공 아트)**     | 권장        | [Windows 10 및 Xbox 이미지 (16:9 슈퍼 주인공 아트)](app-screenshots-and-images.md#windows-10-and-xbox-image-169-super-hero-art) |
| **Xbox 이미지**     | Xbox에 게시 하는 경우 적절 한 디스플레이에 필요        | [Xbox 이미지](app-screenshots-and-images.md#xbox-images) |
| **추가 필드**  | 선택 사항                                    | [추가 필드](create-app-store-listings.md#supplemental-fields) 
| **검색어**              | 선택 사항                                    | [검색어](create-app-store-listings.md#search-terms)         |
| **저작권 및 상표 정보** | 선택 사항                                 | [저작권 및 상표 정보](create-app-store-listings.md#copyright-and-trademark-info) |
| **추가 사용 조건**  | 선택 사항                                    | [추가 사용 조건](create-app-store-listings.md#additional-license-terms) |
| **개발한 사람**              | 선택 사항                                    | [개발한 사람](create-app-store-listings.md#developed-by)                   |


<span/>

### <a name="submission-options-page"></a>전송 옵션 페이지

| 필드 이름                    | 참고                                       | 추가 정보                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **게시 보류 옵션**     | 기본값: 인증 (또는 일정 섹션에서 선택한 날짜에 따라)을 전달 하는 즉시이 제출을 게시 합니다.      | [게시 보류 옵션](manage-submission-options.md#publishing-hold-options)    
| **인증에 대한 참고 사항**     | 권장          | [인증에 대한 참고 사항](notes-for-certification.md)             |
| **제한된 접근 권한 값**     | 제품이 [제한 된 기능](../packaging/app-capability-declarations.md#restricted-capabilities) 을 선언 하는 경우 필수    | [제한된 접근 권한 값](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> LOB (기간 업무) 앱을 엔터프라이즈에 직접 게시 하는 방법에 대 한 자세한 내용은 [lob 앱을 엔터프라이즈에 배포](distribute-lob-apps-to-enterprises.md)를 참조 하세요.
