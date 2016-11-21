---
author: jnHs
Description: "Windows 개발자 센터 대시보드에서 앱에 대한 자세한 분석을 볼 수 있습니다."
title: "분석"
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
translationtype: Human Translation
ms.sourcegitcommit: fa6e5945855defc99e9f5636543ec072eb777a5a
ms.openlocfilehash: c628b1fb29601ff1d4ff3629da45409586274f6b

---

# 분석

Windows 개발자 센터 대시보드에서 앱에 대한 자세한 분석을 볼 수 있습니다. 통계와 차트를 통해 연결된 고객 수, 고객들이 앱을 사용하는 방식, 고객들의 앱 평가를 기반으로 앱의 상태를 알 수 있습니다. 앱 상태, 광고 사용 등에 대한 정보를 찾을 수도 있습니다. 대시보드에서 보고서를 보거나 [필요한 보고서를 다운로드](download-analytic-reports.md)하여 데이터를 오프라인으로 분석합니다. 또한 [대시보드를 사용하지 않고 분석 데이터에 액세스](#no-dashboard)할 수 있는 여러 가지 방법을 제공합니다.

> [!NOTE]
> 대시보드 보고서 외에도 [Windows 스토어 분석 REST API](../monetize/access-analytics-data-using-windows-store-services.md)를 사용하여 프로그래밍 방식으로 분석 데이터에 액세스할 수 있습니다.

## 모든 앱에 대한 분석

가장 많이 다운로드 한 앱에 대한 주요 분석을 보려면 위쪽 탐색 메뉴에서 **분석** > **개요**를 선택합니다. 기본적으로 **분석 개요** 페이지에는 수명을 가장 높게 획득한 앱 5개에 대한 정보가 표시됩니다. 다른 앱을 표시하도록 선택하려면 **필터 변경**을 선택합니다.

## 각 앱에 대해 사용할 수 있는 보고서

이 섹션에서는 다음 보고서 각각에 제공된 정보에 대한 세부 정보를 찾을 수 있습니다.

-   [구입 보고서](acquisitions-report.md)
-   [상태 보고서](health-report.md)
-   [평점 보고서](ratings-report.md)
-   [리뷰 보고서](reviews-report.md)
-   [피드백 보고서](feedback-report.md)
-   [사용 보고서](usage-report.md)
-   [추가 기능 구입 보고서](add-on-acquisitions-report.md)
-   [광고 조정 보고서](ad-mediation-report.md)
-   [광고 성과 보고서](advertising-performance-report.md)
-   [계열사 성과 보고서](affiliates-performance-report.md)
-   [앱 설치 광고 보고서](app-install-ads-reports.md)
-   [채널 및 변환 보고서](channels-and-conversions-report.md)

> [!NOTE]
> 앱의 특정 기능 및 구현에 따라 이러한 보고서 중 일부에 데이터가 표시되지 않을 수도 있습니다.

## 페이지 및 섹션 필터

각 보고서에는 데이터로 드릴다운하는 데 사용할 수 있는 필터가 포함됩니다. 페이지 위쪽에 **필터 적용**이 표시됩니다. 이러한 필터를 사용하여 페이지에서 모든 차트 및 정보의 범위를 제한하거나 확장할 수 있습니다.

각 특정 차트 내에서 개별 섹션 필터를 확인할 수도 있습니다. 이러한 섹션 필터는 해당 특정 차트에 대해서만 표시되는 데이터를 제한합니다.

특정 필터는 보고서별로 달라집니다. 이 섹션의 항목에서는 사용 가능한 필터를 설명하고 각 보고서 페이지의 기타 데이터도 설명합니다.

<span id="no-dashboard"/>
## 개발자 센터 대시보드를 사용하지 않고 분석 데이터에 액세스

대시보드의 분석 보고서 외에도 여러 가지 다양한 방법으로 분석 데이터에 액세스할 수 있습니다.

### Windows 스토어 분석 API

[Windows 스토어 분석 API](../monetize/access-analytics-data-using-windows-store-services.md)를 사용하면 앱에 대한 분석 데이터를 프로그래밍 방식으로 검색합니다. 이 REST API를 사용하면 앱 및 추가 기능 구입, 오류, 앱 평점 및 리뷰 데이터를 검색할 수 있습니다. 이 API는 Azure AD(Azure Active Directory)를 사용하여 앱 또는 서비스의 호출을 인증합니다.

### Power BI용 Windows 개발자 센터 콘텐츠 팩

[Power BI용 Windows 개발자 센터 콘텐츠 팩](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)을 사용하여 Power BI에서 개발자 센터 분석 데이터를 탐색하고 모니터링합니다. Power BI는 클라우드 기반의 비즈니스 분석 서비스로 비즈니스 데이터의 단일 보기를 제공합니다.

Power BI 사용을 시작하여 분석 데이터에 액세스하려면 다음 리소스를 사용하세요.

* [Power BI 등록](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Power BI를 사용하는 방법 알아보기](https://powerbi.microsoft.com/guided-learning/)
* [Power BI용 Windows 개발자 센터 콘텐츠 팩을 사용하여 분석 데이터에 연결하는 방법 알아보기](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> Power BI용 Windows 개발자 센터 콘텐츠 팩에 연결하려면 개발자 센터 계정과 연결된 Azure AD 디렉터리에서 자격 증명을 지정하는 것이 좋습니다. Microsoft 계정 자격 증명을 사용하는 경우에는 Power BI에서 분석 데이터가 자동으로 새로 고쳐지지 않습니다. 데이터를 새로 고치려면 Power BI에 로그인해야 합니다. 조직에서 이미 Office 365 또는 Microsoft의 다른 비즈니스 서비스를 사용하는 경우 이미 Azure AD가 있습니다. 그렇지 않은 경우 [무료로 얻을](http://go.microsoft.com/fwlink/p/?LinkId=703757) 수 있습니다. 개발자 센터 계정과 Azure AD를 연결하는 방법에 대한 자세한 내용은 [계정 사용자 관리](manage-account-users.md)를 참조하세요.

### 개발자 센터 앱


  [개발자 센터](https://www.microsoft.com/store/apps/dev-center/9nblggh4r5ws) 앱을 설치하면 Windows10 디바이스에서 앱의 상태와 성능에 대한 세부 정보를 빠르게 확인할 수 있습니다.

## 관련 항목
- [Windows 앱 게시](index.md)



<!--HONumber=Nov16_HO1-->


