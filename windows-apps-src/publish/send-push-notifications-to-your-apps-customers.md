---
description: 파트너 센터에서 앱으로 알림을 보내는 방법에 대해 알아봅니다 .이를 통해 고객 그룹이 앱 등급을 평가 하거나 추가 기능을 구입 하는 등의 작업을 수행할 수 있습니다.
title: 앱의 고객에게 대상 푸시 알림 보내기
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 대상 알림, 푸시 알림, 알림, 타일
ms.assetid: 16386c81-702d-47cd-9f91-67659f5dca73
ms.localizationpriority: medium
ms.openlocfilehash: 3233dcee13b103978e2aa85181c8aa8a54e0b55c
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034596"
---
# <a name="send-notifications-to-your-apps-customers"></a>앱의 고객에게 알림 보내기

적절 한 시간에 적절 한 메시지와 함께 고객에 게 참여 하는 것은 앱 개발자로 서 성공 하는 데 중요 합니다. 알림은 앱 등급 지정, 추가 기능 구매, 새 기능 시도 또는 다른 앱 다운로드 (사용자가 제공 하는 [판촉 코드](generate-promotional-codes.md) 에서 무료로)와 같은 작업을 수행 하도록 권장 합니다.

[파트너 센터](https://partner.microsoft.com/dashboard) 는 모든 앱 고객에 게 알림을 보내거나 [고객 부문](create-customer-segments.md)에서 정의한 조건을 충족 하는 앱의 Windows 10 고객의 하위 집합을 대상으로 하는 데 사용할 수 있는 데이터 기반 고객 참여 플랫폼을 제공 합니다. 또한 둘 이상의 앱을 고객에 게 보낼 알림을 만들 수 있습니다.

> [!IMPORTANT]
> 이러한 알림은 UWP 앱 에서만 사용할 수 있습니다.

알림의 콘텐츠를 고려할 때 다음 사항에 유의 하세요.
- 알림의 콘텐츠는 저장소 [콘텐츠 정책을](/legal/windows/agreements/store-policies#content_policies)준수 해야 합니다.
- 알림 콘텐츠에 기밀 또는 잠재적으로 중요 한 정보가 포함 되어서는 안 됩니다.
- 예약 된 대로 알림을 배달 하기 위해 모든 작업을 수행 하는 동안 배달에 영향을 주는 대기 시간 문제가 있을 수 있습니다.
- 알림을 너무 자주 보내지 않아야 합니다. 30 분 마다 두 번 이상 방해가 되지 않는 것 처럼 보일 수 있습니다 (대부분의 시나리오에서 권장 되는 것 보다는 빈도가 낮음).
- 앱을 사용 하는 고객이 (세그먼트 멤버 자격이 결정 되는 시점에 해당 Microsoft 계정로 로그인 한 경우) 나중에 장치를 사용할 사람에 게 제공 하는 다른 사용자는 원래 고객의 대상에 해당 하는 알림을 볼 수 있습니다. 자세한 내용은 [대상 푸시 알림에 대 한 앱 구성](../monetize/configure-your-app-to-receive-dev-center-notifications.md#notification-customers)을 참조 하세요.
- 여러 앱의 고객에 게 동일한 알림을 보내는 경우 세그먼트를 대상으로 지정할 수 없습니다. 선택한 앱에 대 한 모든 고객에 게 알림이 전송 됩니다.


## <a name="getting-started-with-notifications"></a>알림 시작

개략적인 수준에서 고객에 게 알림을 사용 하 여 세 가지 작업을 수행 해야 합니다.

1. **푸시 알림을 수신 하도록 앱을 등록 합니다.** 앱에서 Microsoft Store Services SDK에 대 한 참조를 추가한 다음 파트너 센터와 앱 간에 알림 채널을 등록 하는 코드를 몇 줄 추가 하 여이 작업을 수행 합니다. 해당 채널을 사용 하 여 고객에 게 알림을 제공 합니다. 자세한 내용은 [대상 푸시 알림에 대 한 앱 구성](../monetize/configure-your-app-to-receive-dev-center-notifications.md)을 참조 하세요.
2. **대상으로 할 고객을 결정 합니다.** 모든 앱 고객 또는 (단일 앱에 대해 생성 된 알림의 경우)는 인구 통계 또는 수익 기준에 따라 정의할 수 있는 *세그먼트* 라는 고객 그룹에 게 알림을 보낼 수 있습니다. 자세한 내용은 [고객 세그먼트 만들기](create-customer-segments.md)를 참조 하세요.
3. **알림 콘텐츠를 만들고 보냅니다.** 예를 들어 새 고객이 앱을 평가 하도록 하는 알림을 만들거나 특별 한 거래를 홍보 하 여 추가 기능을 구매할 수 있는 알림을 보낼 수 있습니다.


## <a name="to-create-and-send-a-notification"></a>알림을 만들고 보내려면

파트너 센터에서 알림을 만들어 특정 고객 세그먼트로 보내려면 다음 단계를 수행 합니다.

> [!NOTE]
> 앱이 파트너 센터에서 알림을 받을 수 있으려면 먼저 앱에서 [Registernotificationchannelasync](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) 메서드를 호출 하 여 알림을 수신 하도록 앱을 등록 해야 합니다. 이 메서드는 [Microsoft Store SERVICES SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK)에서 사용할 수 있습니다. 코드 예제를 포함 하 여이 메서드를 호출 하는 방법에 대 한 자세한 내용은 [대상 푸시 알림에 대 한 앱 구성](../monetize/configure-your-app-to-receive-dev-center-notifications.md)을 참조 하세요.

1. [파트너 센터](https://partner.microsoft.com/dashboard)에서 **참여** 섹션을 확장 한 다음 **알림** 을 선택 합니다.
2. **알림** 페이지에서 **새 알림** 을 선택 합니다.
3. **템플릿 선택** 섹션에서 전송 하려는 [알림 유형을](#notification-template-types) 선택한 다음 **확인** 을 클릭 합니다.
4. 다음 페이지에서 드롭다운 메뉴를 사용 하 여 알림을 생성 하려는 **단일 앱** 또는 **여러 앱** 을 선택 합니다. [Microsoft Store SERVICES SDK를 사용 하 여 알림을 받도록 구성](../monetize/configure-your-app-to-receive-dev-center-notifications.md)된 앱만 선택할 수 있습니다.
5. 알림 **설정** 섹션에서 알림의 **이름을** 선택 하 고 해당 하는 경우 알림을 보낼 **고객 그룹** 을 선택 합니다. 여러 앱에 전송 된 알림은 해당 앱의 모든 고객 에게만 보낼 수 있습니다. 아직 만들지 않은 세그먼트를 사용 하려는 경우 **새 고객 그룹 만들기** 를 선택 합니다. 알림에 새 세그먼트를 사용 하려면 24 시간이 걸립니다. 자세한 내용은 [고객 세그먼트 만들기](create-customer-segments.md)를 참조 하세요.
6. 알림을 보낼 시기를 지정 하려면 **즉시 알림 보내기** 확인란의 선택을 취소 하 고 각 고객의 현지 표준 시간대를 사용 하도록 지정 하지 않으면 모든 고객의 특정 날짜 및 시간을 선택 합니다.
7. 특정 시점에 알림이 만료 되도록 하려면 **알림 만료 안 함** 확인란을 선택 취소 하 고 특정 만료 날짜 및 시간 (UTC)을 선택 합니다.
8. **단일 앱에 대 한 알림:** 특정 언어를 사용 하거나 특정 표준 시간대에 속하는 사용자 에게만 알림이 전달 되도록 받는 사람을 필터링 하려면 **필터 사용** 확인란을 선택 합니다. 그런 다음 사용할 언어 및/또는 표준 시간대 옵션을 지정할 수 있습니다.
8. **여러 앱에 대 한 알림:** 각 장치 (고객 당)의 마지막 활성 앱에만 알림을 보낼지 아니면 각 장치에 있는 모든 앱에 보낼지를 지정 합니다.
10. **알림 콘텐츠** 섹션의 **언어** 메뉴에서 알림을 표시할 언어를 선택 합니다. 자세한 내용은 [알림 번역](#translate-your-notifications)을 참조 하세요.
11. **옵션** 섹션에서 텍스트를 입력 하 고 원하는 다른 옵션을 구성 합니다. 템플릿으로 시작한 경우이 중 일부는 기본적으로 제공 되지만 원하는 대로 변경할 수 있습니다.

    사용 가능한 옵션은 사용 하는 알림 유형에 따라 달라 집니다. 몇 가지 옵션은 다음과 같습니다.

    * **활성화 유형** (대화형 알림 유형). **전경** , **배경** 또는 **프로토콜** 을 선택할 수 있습니다.
    * **시작** (대화형 알림 유형). 알림이 앱 또는 웹 사이트를 열도록 선택할 수 있습니다.
    * **앱 시작 율 추적** (대화형 알림 유형). 각 알림을 통해 고객에 게 얼마나 잘 관여 하는지 측정 하려면이 확인란을 선택 합니다. 자세한 내용은 [알림 성능 측정](#measure-notification-performance)을 참조 하세요.
    * **기간** (대화형 알림 형식). **짧거나** **긴** 를 선택할 수 있습니다.
    * **시나리오** (대화형 알림 형식). **기본** , **경보** , **미리 알림** 또는 **들어오는 통화** 를 선택할 수 있습니다.
    * **기본 URI** (대화형 알림 형식)입니다. 자세한 내용은 [BaseUri](/uwp/api/windows.ui.xaml.frameworkelement.baseuri#Windows_UI_Xaml_FrameworkElement_BaseUri)를 참조 하세요.
    * **이미지 쿼리 추가** (대화형 알림 유형). 자세한 내용은 [addImageQuery](/uwp/schemas/tiles/toastschema/element-visual#attributes-and-elements)를 참조 하세요.
    * **시각적 개체** . 이미지, 비디오 또는 사운드입니다. 자세한 내용은 [visual](/uwp/schemas/tiles/toastschema/element-visual)을 참조하세요.
    * **입력** / **작업** / **선택** (대화형 알림 유형). 사용자가 알림과 상호 작용할 수 있도록 허용 합니다. 자세한 내용은 [적응 및 대화형 알림 메시지](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)를 참조 하세요.
    * **Binding** (대화형 타일 형식). 알림 템플릿입니다. 자세한 내용은 [binding](/uwp/schemas/tiles/toastschema/element-binding)을 참조 하십시오.

    > [!TIP]
    > [알림 시각화 도우미](https://www.microsoft.com/store/apps/9nblggh5xsl1) 앱을 사용 하 여 적응 타일 및 대화형 알림 메시지를 디자인 하 고 테스트 해 보세요.

12. 나중에 알림에 대 한 작업을 계속 하려면 **초안으로 저장** 을 선택 하 고, 모두 완료 했으면 **보내기** 를 선택 합니다.


## <a name="notification-template-types"></a>알림 템플릿 유형

다양 한 알림 템플릿에서 선택할 수 있습니다.

-   **Blank (알림).** 사용자 지정할 수 있는 빈 알림 메시지를 시작 합니다. 알림 메시지는 고객이 다른 앱, 시작 화면 또는 바탕 화면에 있을 때 앱이 고객과 통신할 수 있도록 화면에 표시 되는 팝업 UI입니다.
-   **Blank (타일)** 사용자 지정할 수 있는 빈 타일 알림으로 시작 합니다. 타일은 시작 화면에 있는 앱의 표현입니다. 타일은 "라이브" 일 수 있으며,이는 표시 되는 콘텐츠가 알림에 대 한 응답으로 변경 될 수 있음을 의미 합니다.
-   **등급 (알림)을 요청 합니다.** 고객에 게 앱 평가를 요청 하는 알림 메시지입니다. 고객이 알림을 선택 하면 앱에 대 한 스토어 등급 페이지가 표시 됩니다.
-   **사용자 의견 (알림)을 요청 합니다.** 고객에 게 앱에 대 한 피드백을 제공 하도록 요청 하는 알림 메시지입니다. 고객이 알림을 선택 하면 앱에 대 한 피드백 허브 페이지가 표시 됩니다.
    > [!NOTE]
    > 이 템플릿 유형을 선택 하는 경우 **시작** 상자에서 {PACKAGE_FAMILY_NAME} 자리 표시자 값을 앱의 실제 패키지 패밀리 이름 (pfn)으로 바꾸어야 합니다. 앱 [id](view-app-identity-details.md) 페이지 ( **앱 관리**  >  **앱 id** )에서 앱의 pfn을 찾을 수 있습니다.

    ![피드백 알림 시작 상자](images/push-notifications-feedback-toast-launch-box.png)

-   **알림 (알림).** 선택한 다른 앱을 승격 하는 알림 메시지입니다. 고객이 알림을 선택 하면 다른 앱의 스토어 목록이 표시 됩니다.
    > [!NOTE]
    > 이 템플릿 유형을 선택 하는 경우 **시작** 상자에서 **{ProductId To 승격할 {ProductId}** 자리 표시자 값을 교차 승격 시킬 항목의 실제 저장소 ID로 바꾸어야 합니다. [앱 id](view-app-identity-details.md) 페이지 ( **앱 관리**  >  **앱 id** )에서 상점 ID를 찾을 수 있습니다.

    ![교차 수준 올리기 알림 시작 상자](images/push-notifications-promote-toast-launch-box.png)

-   **판매 (알림)를 홍보 합니다.** 앱에 대 한 거래를 발표 하는 데 사용할 수 있는 알림 메시지입니다. 고객이 알림을 선택 하면 앱의 스토어 목록이 표시 됩니다.
-   **업데이트 확인 (알림)** 이전 버전의 앱을 실행 하는 고객에 게 최신 버전을 설치 하는 것을 권장 하는 알림 메시지입니다. 고객이 알림을 선택 하면 스토어 앱이 시작 되 **고 다운로드 및 업데이트** 목록이 표시 됩니다. 이 템플릿은 단일 앱 에서만 사용할 수 있으며 특정 고객 세그먼트를 대상으로 지정 하거나 보낼 시간을 정의할 수 없습니다. 항상이 알림이 24 시간 이내에 전송 되도록 예약 하 고 최신 버전의 앱을 아직 실행 하지 않는 모든 사용자를 대상으로 하는 것이 가장 좋습니다.


## <a name="measure-notification-performance"></a>알림 성능 측정

각 알림을 통해 고객에 게 얼마나 잘 참여 하 고 있는지 측정할 수 있습니다.


### <a name="to-measure-notification-performance"></a>알림 성능을 측정 하려면

1.  알림을 만들 때 **알림 콘텐츠** 섹션에서 **앱 시작 요금 추적** 확인란을 선택 합니다.
2.  앱에서 [ParseArgumentsAndTrackAppLaunch](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) 메서드를 호출 하 여 대상 알림에 대 한 응답으로 앱이 시작 되었음을 파트너 센터에 알립니다. 이 메서드는 Microsoft Store Services SDK에서 제공 됩니다. 이 메서드를 호출 하는 방법에 대 한 자세한 내용은 [파트너 센터 알림을 받도록 앱 구성](../monetize/configure-your-app-to-receive-dev-center-notifications.md)을 참조 하세요.


### <a name="to-view-notification-performance"></a>알림 성능을 보려면

위에서 설명한 대로 알림 성능을 측정 하도록 알림과 앱을 구성한 경우 알림이 얼마나 잘 수행 되는지 확인할 수 있습니다.

각 알림에 대 한 자세한 데이터를 검토 하려면 다음을 수행 합니다.

1.  파트너 센터에서 **참여** 섹션을 확장 하 고 **알림** 을 선택 합니다.
2.  기존 알림 표에서 **진행** 중 또는 **완료 됨** 을 선택한 다음 **배달 속도** 와 **앱 시작 속도** 열을 확인 하 여 각 알림의 상위 수준 성능을 확인 합니다.
3.  더 세부적인 성능 세부 정보를 보려면 알림 이름을 선택 합니다. **배달 통계** 섹션에서 다음 알림 **상태** 유형에 대 한 **개수** 및 **비율** 정보를 볼 수 있습니다.
    * **실패** : 어떤 이유로 알림이 전달 되지 않았습니다. 예를 들어 Windows 알림 서비스에서 문제가 발생 하는 경우이 문제가 발생할 수 있습니다.
    * **채널 만료 오류** : 앱과 파트너 센터 간 채널이 만료 되어 알림을 배달할 수 없습니다. 이는 예를 들어 고객이 오랫동안 앱을 열지 않은 경우에 발생할 수 있습니다.
    * **전송 중: 알림을** 보낼 큐에 있습니다.
    * **Sent** : 알림을 보냈습니다.
    * **시작** : 알림이 전송 되 고, 고객이이를 클릭 하 고, 앱이 결과로 열렸습니다. 이는 앱 실행만 추적 합니다. 등급을 유지 하기 위해 스토어를 시작 하는 등의 다른 작업을 수행 하도록 고객을 초대 하는 알림은이 상태에 포함 되지 않습니다.
    * **알 수 없음** :이 알림의 상태를 확인할 수 없습니다.

모든 알림에 대 한 사용자 활동 데이터를 분석 하려면:

1.  파트너 센터에서 **참여** 섹션을 확장 하 고 **알림** 을 선택 합니다.
2.  **알림** 페이지에서 **분석** 탭을 클릭 합니다. 이 탭에는 다음 데이터가 표시 됩니다.
    * 알림을 및 알림 센터 알림에 대 한 다양 한 사용자 작업 상태의 그래프 보기입니다.
    * 알림을 및 알림 센터 알림에 대 한 클릭 속도의 세계 지도 보기입니다.
3. 페이지 맨 위 근처에서 데이터를 표시 하려는 기간을 선택할 수 있습니다. 기본 선택 항목은 30D (30 일) 이지만 3, 6 또는 12 개월에 대 한 데이터 또는 지정한 사용자 지정 데이터 범위에 대 한 데이터를 표시 하도록 선택할 수 있습니다. **필터** 를 확장 하 여 앱 및 시장의 모든 데이터를 필터링 할 수도 있습니다.

## <a name="translate-your-notifications"></a>알림 번역

알림의 영향을 최대화 하려면 고객이 선호 하는 언어로 번역 하는 것이 좋습니다. 파트너 센터를 사용 하면 [Microsoft Translator](https://www.microsoft.com/translator/home.aspx) 서비스의 기능을 활용 하 여 자동으로 알림을 쉽게 번역할 수 있습니다.

1.  기본 언어로 알림을 작성 한 후에는 **알림 콘텐츠** 섹션의 **언어** 메뉴 아래에서 **언어 추가** 를 선택 합니다.
2.  **언어 추가** 창에서 알림을 표시할 추가 언어를 선택 하 고 **업데이트** 를 선택 합니다.
사용자의 알림은 **언어 추가** 창에서 선택한 언어로 자동으로 변환 되 고 **언어** 메뉴에 추가 됩니다.
3.  알림 번역을 보려면 **언어** 메뉴에서 방금 추가한 언어를 선택 합니다.

번역에 유의 해야 할 사항은 다음과 같습니다.
 - 해당 언어에 대 한 **내용** 상자에 다른 항목을 입력 하 여 자동 번역을 재정의할 수 있습니다.
 - 자동 번역을 무시 한 후 영어 버전의 알림에 다른 텍스트 상자를 추가 하는 경우 새 텍스트 상자가 번역 알림에 추가 되지 않습니다. 이 경우 번역 된 각 알림에 새 텍스트 상자를 수동으로 추가 해야 합니다.
 - 알림이 번역 된 후 영어 텍스트를 변경 하는 경우 변경 내용에 맞게 번역 된 알림을 자동으로 업데이트 합니다. 그러나 이전에 초기 번역을 재정의하도록 선택한 경우에는 자동으로 업데이트되지 않습니다.

## <a name="related-topics"></a>관련 항목
- [UWP 앱에 대 한 타일](../design/shell/tiles-and-notifications/creating-tiles.md)
- [WNS(Windows 푸시 알림 서비스) 개요](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)
- [알림 시각화 도우미 앱](https://www.microsoft.com/store/apps/9nblggh5xsl1)
- [StoreServicesEngagementManager () | registerNotificationChannelAsync () 메서드](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync)
