---
description: 파트너 센터에서 또는 다른 방법을 통해 Windows 앱에 대 한 자세한 분석을 받으세요.
title: 앱 성능 분석
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 분석, 보고서, 대시보드, 앱, 데이터, 메트릭
ms.localizationpriority: medium
ms.openlocfilehash: 08ee9c5ddf729ac7a9026c36bc9fbddad0af5dc5
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032986"
---
# <a name="analyze-app-performance"></a>앱 성능 분석

[파트너 센터](https://partner.microsoft.com/dashboard)에서 앱에 대 한 자세한 분석을 볼 수 있습니다. 통계 및 차트를 사용 하 여 앱을 사용 하는 방법, 앱을 사용 하는 방법 및 그에 대 한 정보를 확인 하는 방법을 알 수 있습니다. 앱 상태, 광고 사용 등에 대 한 메트릭을 찾을 수도 있습니다.

파트너 센터에서 바로 분석 보고서를 보거나 오프 라인으로 데이터를 분석 하 [는 데 필요한 보고서를 다운로드할](download-analytic-reports.md) 수 있습니다. 또한 [파트너 센터 외부의 분석 데이터에 액세스](#outside)하는 여러 가지 방법을 제공 합니다.

## <a name="view-key-analytics-for-all-your-apps"></a>모든 앱에 대 한 주요 분석 보기

가장 많이 다운로드 한 앱에 대 한 주요 분석을 보려면 **분석** 을 확장 하 고 **개요** 를 선택 합니다. 기본적으로 개요 페이지에는 가장 많은 수명 가져오기가 있는 5 개 앱에 대 한 정보가 표시 됩니다. 표시할 다른 게시 된 앱을 선택 하려면 **필터** 를 선택 합니다.

## <a name="view-individual-reports-for-each-app"></a>각 앱에 대 한 개별 보고서 보기

이 섹션에서는 다음 각 보고서에 제공 된 정보에 대 한 세부 정보를 찾을 수 있습니다.

-   [구입 보고서](acquisitions-report.md)
-   [추가 기능 구입 보고서](add-on-acquisitions-report.md)
-   [사용 보고서](usage-report.md)
-   [상태 보고서](health-report.md)
-   [평점 보고서](ratings-report.md)
-   [리뷰 보고서](reviews-report.md)
-   [피드백 보고서](feedback-report.md)
-   [Xbox 분석 보고서](xbox-analytics-report.md)
-   [인사이트 보고서](insights-report.md)
-   [광고 성과 보고서](advertising-performance-report.md)
-   [광고 캠페인 보고서](/windows/uwp/publish/ad-campaign-report)


> [!NOTE]
> 앱의 특정 기능 및 구현에 따라 이러한 모든 보고서에 데이터가 표시 되지 않을 수 있습니다.

<span id="outside"/>

## <a name="access-analytics-data-outside-of-partner-center"></a>파트너 센터 외부의 분석 데이터 액세스

파트너 센터에서 보고서를 보는 것 외에도 다른 방법으로 앱 분석에 액세스할 수 있습니다.

### <a name="microsoft-store-analytics-api"></a>Microsoft Store analytics API

[스토어 분석 API](../monetize/access-analytics-data-using-windows-store-services.md) 를 사용 하 여 앱에 대 한 분석 데이터를 프로그래밍 방식으로 검색할 수 있습니다. 이 REST API를 사용 하 여 앱 및 추가 기능, 오류, 앱 등급 및 리뷰에 대 한 데이터를 검색할 수 있습니다. 이 API는 Azure Active Directory (Azure AD)를 사용 하 여 앱 또는 서비스에서 호출을 인증 합니다.

### <a name="windows-dev-center-content-pack-for-power-bi"></a>Power BI용 Windows 개발자 센터 콘텐츠 팩

[Power BI 용 Windows 개발자 센터 콘텐츠 팩](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) 을 사용 하 여 Power BI에서 파트너 센터 분석 데이터를 탐색 하 고 모니터링할 수 있습니다. Power BI는 비즈니스 데이터의 단일 뷰를 제공 하는 클라우드 기반 비즈니스 분석 서비스입니다.

Power BI 사용 하 여 분석 데이터에 액세스 하려면 다음 리소스를 사용 하세요.

* [Power BI에 등록](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Power BI 사용 방법 알아보기](https://powerbi.microsoft.com/guided-learning/)
* [Power BI 용 Windows 개발자 센터 콘텐츠 팩을 사용 하 여 분석 데이터에 연결 하는 방법을 알아봅니다.](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> Power BI 용 Windows 개발자 센터 콘텐츠 팩에 연결 하려면 파트너 센터 계정과 연결 된 Azure AD 디렉터리에서 자격 증명을 지정 하는 것이 좋습니다. Microsoft 계정 자격 증명을 사용 하는 경우 Power BI의 분석 데이터가 자동으로 새로 고쳐지지 않으며 Power BI에 로그인 하 여 데이터를 새로 고쳐야 합니다. 조직에서 이미 Microsoft 365 또는 Microsoft의 다른 비즈니스 서비스를 사용 하는 경우 Azure AD가 이미 있는 것입니다. 그렇지 않으면 [무료로 다운로드할](https://account.azure.com/organization)수 있습니다. 연결 설정에 대 한 자세한 내용은 [파트너 센터 계정에 Azure Active Directory 연결](./associate-azure-ad-with-partner-center.md)을 참조 하세요.
