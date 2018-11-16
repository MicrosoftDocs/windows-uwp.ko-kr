---
author: JnHs
Description: Get detailed analytics for your Windows apps, in Partner Center or via other methods.
title: 앱 성능 분석
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 분석, 보고서, 대시보드, 앱, 데이터, 메트릭
ms.localizationpriority: medium
ms.openlocfilehash: 22d9a4d4b66091148bbb078abfb89237ab14ea87
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6971340"
---
# <a name="analyze-app-performance"></a>앱 성능 분석

[파트너 센터](https://partner.microsoft.com/dashboard)의 앱에 대 한 자세한 분석을 볼 수 있습니다. 통계와 차트를 통해 연결된 고객 수, 고객들이 앱을 사용하는 방식, 고객들의 앱 평가를 기반으로 앱의 상태를 알 수 있습니다. 앱 상태, 광고 사용 등에 대한 메트릭을 찾을 수도 있습니다.

분석 보고서를 볼 수 파트너 센터 또는 [필요한 보고서를 다운로드](download-analytic-reports.md) 하 여 데이터를 오프 라인으로 분석할 오른쪽 합니다. 사용자에 대 한 여러 가지 방법으로 [파트너 센터 외부 분석 데이터에 액세스할](#outside)수 제공도 합니다.

## <a name="view-key-analytics-for-all-your-apps"></a>모든 앱에 대한 주요 분석 보기

가장 많이 다운로드 한 앱에 대한 주요 분석을 보려면 **분석**을 확장하고 **개요**를 선택합니다. 기본적으로 개요 페이지에는 수명을 가장 높게 획득한 앱 5개에 대한 정보가 표시됩니다. 다른 게시 앱을 표시하도록 선택하려면 **필터**를 선택합니다.

## <a name="view-individual-reports-for-each-app"></a>각 앱에 대한 개별 보고서 보기

이 섹션에서는 다음 보고서 각각에 제공된 정보에 대한 세부 정보를 찾을 수 있습니다.

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
-   [광고 캠페인 보고서](promote-your-app-report.md)


> [!NOTE]
> 앱의 특정 기능 및 구현에 따라 이러한 보고서 중 일부에 데이터가 표시되지 않을 수도 있습니다.

<span id="outside"/>

## <a name="access-analytics-data-outside-of-partner-center"></a>파트너 센터 외부 분석 데이터에 액세스

파트너 센터에서 보고서를 보는 것 외에 다양 한 가지 방법으로 앱 분석을 액세스할 수 있습니다.

### <a name="microsoft-store-analytics-api"></a>Microsoft Store 분석 API

[Store 분석 API](../monetize/access-analytics-data-using-windows-store-services.md)를 사용하면 앱에 대한 분석 데이터를 프로그래밍 방식으로 검색할 수 있습니다. 이 REST API를 사용하면 앱 및 추가 기능 구입, 오류, 앱 평점 및 리뷰 데이터를 검색할 수 있습니다. 이 API는 Azure AD(Azure Active Directory)를 사용하여 앱 또는 서비스의 호출을 인증합니다.

### <a name="windows-dev-center-content-pack-for-power-bi"></a>Power BI용 Windows 개발자 센터 콘텐츠 팩

[Power BI 용 Windows 개발자 센터 콘텐츠 팩](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) 을 사용 하 여 탐색 하 고 Power BI에서 파트너 센터 분석 데이터를 모니터링 합니다. Power BI는 클라우드 기반의 비즈니스 분석 서비스로 비즈니스 데이터의 단일 보기를 제공합니다.

Power BI 사용을 시작하여 분석 데이터에 액세스하려면 다음 리소스를 사용하세요.

* [Power BI 등록](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Power BI를 사용하는 방법 알아보기](https://powerbi.microsoft.com/guided-learning/)
* [Power BI용 Windows 개발자 센터 콘텐츠 팩을 사용하여 분석 데이터에 연결하는 방법 알아보기](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> Power BI 용 Windows 개발자 센터 콘텐츠 팩에 연결 하려면 파트너 센터 계정과 연결 된 Azure AD 디렉터리에서 자격 증명을 지정 하는 것이 좋습니다. Microsoft 계정 자격 증명을 사용하는 경우에는 Power BI에서 분석 데이터가 자동으로 새로 고쳐지지 않습니다. 데이터를 새로 고치려면 Power BI에 로그인해야 합니다. 조직에서 이미 Office 365 또는 Microsoft의 다른 비즈니스 서비스를 사용하는 경우 이미 Azure AD가 있습니다. 그렇지 않은 경우 [무료로 얻을](http://go.microsoft.com/fwlink/p/?LinkId=703757) 수 있습니다. 연결 설정에 대 한 자세한 내용은 [Azure Active Directory 연결 파트너 센터 계정에](associate-azure-ad-with-dev-center.md)를 참조 하세요.

### <a name="dev-center-app"></a>개발자 센터 앱

[개발자 센터](https://www.microsoft.com/store/apps/dev-center/9nblggh4r5ws) 앱을 설치하면 Windows10 장치에서 앱의 상태와 성능에 대한 세부 정보를 빠르게 확인할 수 있습니다.

