---
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: Microsoft Advertising SDK는 광고로 앱 수익을 창출하는 다양한 방법을 제공합니다.
title: Microsoft Advertising SDK를 사용하여 앱에 광고 표시
ms.date: 06/20/2018
ms.topic: article
keywords: windows 10, uwp, 광고, 광고, 배너, 광고 관리, 중간 광고
ms.localizationpriority: medium
ms.openlocfilehash: 84ed7f5f1eb65f06a47e92de962777ca9d3c50c7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658498"
---
# <a name="display-ads-in-your-app-with-the-microsoft-advertising-sdk"></a>Microsoft Advertising SDK를 사용하여 앱에 광고 표시

Microsoft Advertising SDK를 사용해 Windows 10용 유니버설 Windows 플랫폼(UWP) 앱에 광고를 삽입하여 수익 기회를 늘리세요. Ad 수익 화 플랫폼은 다양 한 많은 인기 있는 광고 네트워크를 사용 하 여 앱 및 지원 중재 프로그램에 원활 하 게 통합 될 수 있는 ad 형식의 제공 합니다. 이 플랫폼은 OpenRTB, VAST 2.x, MRAID 2 및 VPAID 3 표준을 준수하며 MOAT 및 IAS와 호환됩니다. 

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
    <a href="https://aka.ms/ads-sdk-uwp">Microsoft Advertising SDK 설치</a>
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
    <a href="set-up-ad-units-in-your-app.md">설정 앱의 ad 단위</a>
    <br/>
    <a href="best-practices-for-ads-in-apps.md">모범 사례</a>
    <br/>
    <a href="https://msdn.microsoft.com/en-us/library/windows/apps/mt691884.aspx">API 참조</a>
    </td>
</tr>
</tbody>
</table>

## <a name="step-1-install-the-microsoft-advertising-sdk"></a>1단계: Microsoft Advertising SDK 설치

시작하려면 앱을 빌드하기 위해 사용하는 개발 컴퓨터에 [Microsoft Advertising SDK](https://aka.ms/ads-sdk-uwp)를 설치합니다. 설치 지침은 [이 문서](install-the-microsoft-advertising-libraries.md)를 참조하세요.

## <a name="step-2-implement-ads-in-your-app"></a>2단계: 앱에 광고 구현

Microsoft Advertising SDK는 앱에서 사용할 수 있는 여러 유형의 광고 컨트롤을 제공합니다. 시나리오에 가장 적합한 광고 유형을 선택한 후 이 광고를 표시할 코드를 앱에 추가합니다. 이 단계에 테스트 광고 단위를 사용하므로 테스트 중에 앱이 광고를 어떻게 렌더링하는지 확인할 수 있습니다.

### <a name="banner-ads"></a>배너 광고

이 광고는 앱에서 페이지의 직사각형 부분을 활용하여 홍보용 콘텐츠를 표시하는 정적 디스플레이 광고입니다. 이러한 광고는 정해진 간격에 따라 자동으로 새로 고칠 수 있습니다. 앱에 광고를 처음 게시하는 경우 시작하기 좋은 방법입니다.

지침과 코드 예제는 [이 문서](adcontrol-in-xaml-and--net.md)를 참조하세요.

![addreferences](images/banner-ad.png)

### <a name="interstitial-video-and-interstitial-banner-ads"></a>중간 비디오 및 중간 배너 광고

일반적으로 사용자가 앱 또는 게임을 계속하기 위해 강제로 비디오를 시청하거나 광고를 클릭해야 하는 전체 화면 광고입니다. 저희는 비디오와 배너라는 두 가지 중간 광고를 지원합니다.

지침과 코드 예제는 [이 문서](interstitial-ads.md)를 참조하세요.

![addreferences](images/interstitial-ad.png)

### <a name="native-ads"></a>기본 광고

구성 요소 기반 광고입니다. 자신의 글꼴, 색상, 기타 UI 구성 요소를 사용해 자신의 각 광고 크리에이티브 조각이 앱과 통합할 수 있는 개별 요소로 앱에 제공되는 구성 요소 기반 광고입니다.

지침과 코드 예제는 [이 문서](native-ads.md)를 참조하세요.

![addreferences](images/native-ad.png)

<span id="ad-mediation"/>

## <a name="step-3-create-an-ad-unit-and-configure-mediation"></a>3단계: Ad 단위 만들기 및 중재를 구성 합니다.

앱 테스트를 마친 후 스토어에 제출할 준비가 되었습니다. ad 단위를 만들기에 [앱 내 광고](../publish/in-app-ads.md) 파트너 센터의 페이지입니다. 그런 다음, 이 광고 단위를 사용하도록 앱 코드를 업데이트하면 앱에 실제 광고가 게재됩니다. 자세한 내용은 [앱에서 광고 단위 설정](set-up-ad-units-in-your-app.md#live-ad-units)을 참조하세요.

기본적으로 앱에는 유료 광고에 대한 Microsoft 네트워크의 광고가 표시됩니다. 광고 수익을 극대화하기 위해 Taboola 및 Smaato 등 추가 유료 광고 네트워크의 광고를 표시하도록 광고 단위에 대한 [광고 조정](ad-mediation-service.md)을 사용할 수 있습니다. Microsoft 앱 홍보 캠페인에 광고를 제공하여 앱 홍보 기능을 향상시킬 수 있습니다.

UWP 앱의 광고 조정을 사용하기 시작하려면, 광고 단위의 [광고 조정 설정을 구성](../publish/in-app-ads.md#mediation-settings)합니다. 개발자의 앱이 제공되는 시장에서 광고 수익을 극대화할 수 있도록 기본적으로 기계 학습 알고리즘을 사용하여 조정 설정이 자동으로 구성됩니다. 하지만 사용하려는 네트워크를 수동으로 선택할 수 있는 옵션도 제공됩니다. 어떤 방식을 선택하든 조정 설정이 서버 전체에서 구성되므로 개발자는 앱에서 코드를 변경할 필요가 없습니다.    

## <a name="step-4-submit-your-app-and-review-performance"></a>4단계: 앱을 제출하고 성능 검토

광고를 사용 하 여 앱 개발을 마친 후 있습니다 [업데이트 된 앱 제출](https://docs.microsoft.com/windows/uwp/publish/app-submissions) 저장소에 사용할 수 있도록 하려면 파트너 센터에서. 광고를 표시하는 앱은 [Microsoft Store 정책 섹션 10.10](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) 및 [앱 개발자 계약 별첨 E](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)에 규정된 추가 요구 사항을 충족해야 합니다.

앱 게시 후 저장소에 사용 가능한 검토할 수 있습니다 하 [성능 보고서를 광고](../publish/advertising-performance-report.md) 파트너 센터에서 광고의 성능을 최적화할 수 있도록 중재 설정을 변경 하려면 계속 합니다. 광고 수익은 [지급액 요약](../publish/payout-summary.md)에 포함되어 있습니다.

<span id="additional-help" />

## <a name="additional-help"></a>추가 도움말

Microsoft Advertising SDK 사용에 대한 추가 도움말은 다음 리소스를 사용하세요.

|  작업    | 리소스 |               
|----------|-------|
| 버그 보고 또는 광고 지원 받기     | [지원 페이지](https://developer.microsoft.com/en-us/windows/support)를 방문하여 **인앱 광고**를 선택합니다.        |
| 커뮤니티 지원 받기     | [포럼](https://go.microsoft.com/fwlink/p/?LinkId=401266)을 방문하세요.       |
| 배너 및 중간 광고를 앱에 추가하는 방법을 보여 주는 샘플 프로젝트를 다운로드합니다.     | [GitHub의 광고 샘플](https://aka.ms/githubads)을 참조하세요.       |
| Windows 앱용 최신 수익 창출 기회에 대한 자세한 정보     | [앱으로 수익 창출](https://developer.microsoft.com/store/monetize)을 방문하세요.        |

## <a name="windows-81-and-windows-phone-8x-apps"></a>Windows 8.1 및 Windows Phone 8.x 앱

Windows 8.1 및 Windows Phone 8.x 앱의 경우 [Microsoft Advertising SDK for Windows 및 Windows Phone 8.x](https://aka.ms/store-8-sdk)를 제공합니다. Windows 8.1 및 Windows Phone 8.x 앱에서 이 SDK를 사용해 광고를 표시하는 방법에 대한 자세한 내용은 [이 문서](https://docs.microsoft.com/en-us/previous-versions/windows/apps/dn792120(v=win.10))를 참조하세요.

## <a name="related-topics"></a>관련 항목

* [Microsoft Advertising SDK](https://aka.ms/ads-sdk-uwp)
* [광고 성과 보고서](../publish/advertising-performance-report.md)
* [Windows Premium 광고 게시자 프로그램](windows-premium-ads-publishers-program.md)
