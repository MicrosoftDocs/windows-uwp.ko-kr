---
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: Microsoft Advertising SDK는 광고를 사용 하 여 앱을 수익 창출 하는 여러 가지 방법을 제공 합니다.
title: Microsoft Advertising SDK를 사용하여 앱에 광고 표시
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, 광고, 광고, 배너, ad 컨트롤, 중간
ms.localizationpriority: medium
ms.openlocfilehash: 4e9a67bb26d47d0bd9cc26c56df90efac5e2daa5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164837"
---
# <a name="display-ads-in-your-app-with-the-microsoft-advertising-sdk"></a>Microsoft Advertising SDK를 사용하여 앱에 광고 표시

>[!WARNING]
> 2020 년 6 월 1 일부 터 Windows UWP 앱 용 Microsoft Ad 수익 화 플랫폼이 종료 됩니다. [자세한 정보](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Microsoft Advertising SDK를 사용 하 여 Windows 10 용 유니버설 Windows 플랫폼 (UWP) 앱에 광고를 배치 하 여 수익 기회를 늘립니다. Ad 수익 화 플랫폼은 앱에 원활 하 게 통합 될 수 있고 널리 사용 되는 여러 ad 네트워크를 통한 중재를 지 원하는 다양 한 ad 형식을 제공 합니다. 이 플랫폼은 OpenRTB, 방대한 2.x, MRAID 2 및 VPAID 3 표준과 호환 되며 MOAT 및 IAS와 호환 됩니다. 

<br/>

<table style="border: none !important;">
<colgroup>
<col width="10%" />
<col width="23%" />
<col width="10%" />
<col width="23%" />
<col width="10%" />
<col width="23%" />
</colgroup>
<tbody>
<tr>
<td align="left"><img src="images/install-sdk.png" alt="Install SDK icon" /></td>
<td align="left"><b>시작</b><br/><br/>
    <a href="https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK">Microsoft Advertising SDK 설치</a>
</td>
<td align="left"><img src="images/write-code.png" alt="Develop icon" /></td>
<td align="left"><b>개발자 가이드</b><br/><br/>
    <a href="banner-ads.md">배너 광고</a>
    <br/>
    <a href="interstitial-ads.md">중간 광고</a>
    <br/>
    <a href="native-ads.md">네이티브 광고</a>
    </td>
<td align="left"><img src="images/api-reference.png" alt="API ref icon" /></td>
<td align="left"><b>기타 리소스</b><br/><br/>
    <a href="set-up-ad-units-in-your-app.md">앱에서 ad 단위 설정</a>
    <br/>
    <a href="best-practices-for-ads-in-apps.md">모범 사례</a>
    <br/>
    <a href="/uwp/api/overview/advertising">API 참조</a>
    </td>
</tr>
</tbody>
</table>

## <a name="step-1-install-the-microsoft-advertising-sdk"></a>1 단계: Microsoft Advertising SDK 설치

시작 하려면 앱을 빌드하는 데 사용 하는 개발 컴퓨터에 [MICROSOFT ADVERTISING SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) 를 설치 합니다. 설치 지침은 [이 문서](install-the-microsoft-advertising-libraries.md)를 참조 하세요.

## <a name="step-2-implement-ads-in-your-app"></a>2 단계: 앱에서 광고 구현

Microsoft Advertising SDK는 앱에서 사용할 수 있는 여러 가지 유형의 ad 컨트롤을 제공 합니다. 시나리오에 가장 적합 한 광고 유형을 선택 하 고 앱에 이러한 광고를 표시 하는 코드를 추가 합니다. 이 단계에서는 테스트 하는 동안 앱이 광고를 어떻게 렌더링 하는지 확인할 수 있도록 테스트 ad 단위를 사용 합니다.

### <a name="banner-ads"></a>배너 광고

프로 모션 콘텐츠를 표시 하기 위해 앱에서 페이지의 사각형 부분을 활용 하는 정적 표시 광고입니다. 이러한 광고는 일정 한 간격으로 자동으로 새로 고칠 수 있습니다. 앱에서 광고를 처음 접하는 경우이를 시작 하는 것이 좋습니다.

지침 및 코드 예제는 [이 문서](adcontrol-in-xaml-and--net.md)를 참조 하세요.

![addreferences](images/banner-ad.png)

### <a name="interstitial-video-and-interstitial-banner-ads"></a>중간 비디오 및 중간 배너 광고

이러한 광고는 일반적으로 사용자가 비디오를 시청 하거나 클릭 하 여 앱 또는 게임을 계속 하는 데 필요한 전체 화면 광고입니다. 비디오와 배너의 두 가지 중간 광고 유형을 지원 합니다.

지침 및 코드 예제는 [이 문서](interstitial-ads.md)를 참조 하세요.

![addreferences](images/interstitial-ad.png)

### <a name="native-ads"></a>기본 광고

이러한 광고는 구성 요소 기반 광고입니다. 제목, 이미지, 설명 및 동작에 대 한 호출 텍스트와 같은 ad creative의 각 부분은 사용자 고유의 글꼴, 색 및 기타 UI 구성 요소를 사용 하 여 앱에 통합할 수 있는 개별 요소로 앱에 배달 됩니다.

지침 및 코드 예제는 [이 문서](native-ads.md)를 참조 하세요.

![addreferences](images/native-ad.png)

<span id="ad-mediation"/>

## <a name="step-3-create-an-ad-unit-and-configure-mediation"></a>3 단계: ad 단위 만들기 및 중재 구성

앱 테스트를 마치고 스토어에 제출할 준비가 되 면 파트너 센터의 [앱 내 광고](../publish/in-app-ads.md) 페이지에서 ad 단위를 만듭니다. 그런 다음 앱이 라이브 광고를 받을 수 있도록이 ad 단위를 사용 하도록 앱 코드를 업데이트 합니다. 자세한 내용은 [앱에서 ad 단위 설정](set-up-ad-units-in-your-app.md#live-ad-units)을 참조 하세요.

기본적으로 앱은 유료 광고에 대해 Microsoft 네트워크의 광고를 표시 합니다. Ad 수익을 최대화 하기 위해 사용자의 ad에 대해 ad [중재](ad-mediation-service.md) 를 사용 하도록 설정 하 여 다른 유료 ad 네트워크 (예: Taboola 및 Smaato)의 광고를 표시할 수 있습니다. Microsoft 앱 판촉 캠페인에서 광고를 제공 하 여 앱 승격 기능을 늘릴 수도 있습니다.

UWP 앱에서 ad 중재 사용을 시작 하려면 ad 장치에 대 한 [ad 중재 설정을 구성](../publish/in-app-ads.md#mediation-settings) 합니다. 기본적으로 앱이 지 원하는 시장에서 광고 수익을 극대화 하는 데 도움이 되도록 기계 학습 알고리즘을 사용 하 여 중재 설정을 자동으로 구성 합니다. 그러나 사용 하려는 네트워크를 수동으로 선택 하는 옵션도 있습니다. 어느 쪽이 든, 중재 설정은 서버에서 전적으로 구성 됩니다. 앱에서 코드를 변경할 필요가 없습니다.    

## <a name="step-4-submit-your-app-and-review-performance"></a>4 단계: 앱 제출 및 성능 검토

광고를 사용 하 여 앱 개발을 마친 후에는 [업데이트 된 앱](../publish/app-submissions.md) 을 파트너 센터에서 제출 하 여 스토어에서 사용할 수 있도록 할 수 있습니다. 광고를 표시 하는 앱은 [앱 개발자 계약의](/legal/windows/agreements/app-developer-agreement)Microsoft Store 정책 및 공시물 E의 [섹션 10.10](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) 에 지정 된 추가 요구 사항을 충족 해야 합니다.

앱을 게시 하 고 스토어에서 사용할 수 있게 되 면 파트너 센터에서 [광고 성능 보고서](../publish/advertising-performance-report.md) 를 검토 하 고 중재 설정을 변경 하 여 광고의 성능을 최적화할 수 있습니다. 광고 수익은 [지급 요약](../publish/payout-summary.md)에 포함 되어 있습니다.

<span id="additional-help" />

## <a name="additional-help"></a>추가 도움말

Microsoft Advertising SDK를 사용 하 여 추가 도움말을 보려면 다음 리소스를 사용 하세요.

|  Task    | 리소스 |               
|----------|-------|
| 버그 보고 또는 광고에 대 한 보조 지원 받기     | [지원 페이지](https://developer.microsoft.com/windows/support) 를 방문 하 여 **앱 광고-앱**을 선택 합니다.        |
| 커뮤니티 지원 받기     | [포럼](https://social.msdn.microsoft.com/forums/windowsapps/en-US/home?category=windowsapps)을 방문 하세요.       |
| 앱에 배너 및 중간 광고를 추가 하는 방법을 보여 주는 샘플 프로젝트를 다운로드 합니다.     | [GitHub의 광고 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)을 참조 하세요.       |
| Windows 앱용 최신 수익 창출 기회에 대한 자세한 정보     | [앱 수익 창출을](https://developer.microsoft.com/store/monetize)방문 하세요.        |

## <a name="windows-81-and-windows-phone-8x-apps"></a>Windows 8.1 및 Windows Phone 8gb 앱

Windows 8.1 및 Windows Phone 8.x 앱의 경우 [MICROSOFT ADVERTISING SDK For Windows 및 Windows Phone](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDKforWindowsandWindowsPhone8x)4.x를 제공 합니다. 이 SDK를 사용 하 여 Windows 8.1 및 Windows Phone 8gb 앱에 광고를 표시 하는 방법에 대 한 자세한 내용은 [이 문서](/previous-versions/windows/apps/dn792120(v=win.10))를 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)
* [광고 성과 보고서](../publish/advertising-performance-report.md)
* [Windows 프리미엄 광고 게시자 프로그램](windows-premium-ads-publishers-program.md)