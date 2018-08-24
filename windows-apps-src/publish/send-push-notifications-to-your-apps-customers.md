---
author: JnHs
Description: Learn how to send notifications from Windows Dev Center to your app to encourage groups of customers to take an action, such as rating an app or buying an add-on.
title: 앱의 고객에게 대상 푸시 알림 보내기
ms.author: wdg-dev-content
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 대상 알림, 푸시 알림, 알림, 타일
ms.assetid: 16386c81-702d-47cd-9f91-67659f5dca73
ms.localizationpriority: medium
ms.openlocfilehash: 9d62f46ad1b55fbad3ab7c21a593625a2538b68f
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2834075"
---
# <a name="send-notifications-to-your-apps-customers"></a>앱의 고객에게 알림 보내기

올바른 시간에 올바른 메시지로 고객의 참여를 유도하는 것이 앱 개발자로서 성공하기 위한 핵심입니다. 알림을 보내 앱 평가, 추가 기능 구매, 새 기능 체험, 다른 앱 다운로드 등 고객의 참여를 유도할 수 있습니다(제공하는 [홍보 코드](generate-promotional-codes.md)를 이용해 무료로).

Windows 개발자 센터에서는 알림을 모든 앱 고객에게 또는 [고객층](create-customer-segments.md)에서 정의한 기준을 충족하는 앱의 Windows 10 일부 고객에게만 전송할 수 있도록 데이터 기반 고객 참여 플랫폼을 제공합니다. <!-- You can also send a single notification to all of the customers for multiple apps. -->

> [!IMPORTANT]
> 이러한 알림은 UWP 앱에만 사용할 수 있습니다.

알림의 콘텐츠를 고려할 때 다음을 유의하십시오.
- 알림의 콘텐츠는 Microsoft Store [콘텐츠 정책](https://docs.microsoft.com/legal/windows/agreements/store-policies#content_policies)을 준수해야 합니다.
- 알림 콘텐츠에는 기밀 정보나 민감한 정보가 포함되어서는 안 됩니다.
- 알림이 일정에 따라 전달되도록 최선을 다할 것이지만, 전달에 영향을 미치는 지연 문제가 간혹 있을 수 있습니다.
- 너무 자주 알림을 보내지 마십시오. 30분 간격보다 짧게 알림을 보내면 방해가 될 수 있으며, 많은 경우에 사람들은 알림을 덜 자주 받기를 선호합니다.
- 여러분의 앱을 사용하는 고객이 나중에 장치를 다른 사람에게 사용하라고 주었을 경우(또한 고객층 멤버 자격을 판단할 때 본래 사용자의 Microsoft 계정으로 로그인되어 있을 경우), 원래 목표한 대상이 아닌 다른 사람에게 알림이 전달될 수 있음을 유의하십시오. 자세한 내용은 [앱에서 대상 푸시 알림 구성](../monetize/configure-your-app-to-receive-dev-center-notifications.md#notification-customers)을 참조하세요.
- 여러 앱의 고객에게 동일한 알림을 보내는 경우 세그먼트를 대상으로 할 수 없습니다. 알림은 선택한 앱의 모든 고객에게 전송됩니다.


## <a name="getting-started-with-notifications"></a>알림 시작

고객 참여를 유도하기 위한 알림을 사용하려면 상위 수준에서 세 가지를 수행해야 합니다.

1. **푸시 알림을 받기 위해 앱을 등록합니다.** 앱에서 Microsoft Store Services SDK에 대한 참조를 추가한 다음, 개발자 센터와 앱 사이의 알림 채널을 등록하는 코드 몇 줄을 추가하면 됩니다. 고객에게 알림을 전달하는 데 이 채널을 사용하게 됩니다. 자세한 내용은 [앱에서 대상 푸시 알림 구성](../monetize/configure-your-app-to-receive-dev-center-notifications.md)을 참조하세요.
2. **어떤 고객을 대상으로 할지 결정합니다.** 앱의 모든 고객에게, 또는 단일 앱에 대해 생성된 알림의 경우 인구 통계 또는 수익 기준에 따라 정의할 수 있는 *세그먼트*라고 하는 고객 그룹에게 알림을 보낼 수 있습니다. 자세한 내용은 [고객층 만들기](create-customer-segments.md)를 참조하세요.
3. **알림 콘텐츠를 만들고 전송합니다.** 예를 들어 새로운 고객에게 앱을 평가하도록 권장하는 알림을 만들거나 추가 기능을 구매하도록 유도하는 특가를 홍보하는 알림을 보낼 수 있습니다.


## <a name="to-create-and-send-a-notification"></a>알림을 만들어서 보내려면

다음 단계를 따라 대시보드에서 알림을 만들고 특정 고객층에 보냅니다.

> [!NOTE]
> 앱이 개발자 센터에서 알림을 받으려면, 먼저 앱에서 [RegisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) 메서드를 호출하여 알림을 수신하도록 앱을 등록해야 합니다. 이 메서드는 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)에서 사용할 수 있습니다. 코드 예제와 함께 이 메서드를 호출하는 방법은 [앱에서 대상 푸시 알림 구성](../monetize/configure-your-app-to-receive-dev-center-notifications.md)을 참조하세요.

1. [Windows 개발자 센터 대시보드](https://partner.microsoft.com/dashboard/)에서 **참여** 섹션을 확장하고 **알림**을 선택합니다.
2. **알림** 페이지에서 **새 알림**을 선택합니다.
3. **서식 파일 선택** 섹션에서 보내고 **확인**을 클릭 한 다음 원하는 [알림 유형](#notification-template-types) 을 선택 합니다.
4. 다음 페이지에서 드롭다운 메뉴를 사용하여 알림을 생성하고자 하는 **단일 앱** 또는 **여러 앱**을 선택합니다. [Microsoft 저장소 서비스 SDK를 사용 하 여 알림을 수신 하도록 구성](../monetize/configure-your-app-to-receive-dev-center-notifications.md)된 앱만 선택할 수 있습니다.
5. **알림 설정** 섹션에서 알림의 **이름**을 선택하고, 적용 가능한 경우, 알림을 보낼 **고객 그룹**을 선택합니다. (여러 앱에 전송된 알림은 해당 앱의 모든 고객에게만 보낼 수 있습니다.) 아직 만들지 않은 세그먼트를 사용하려는 경우 **새 고객 그룹 만들기**를 선택합니다. 알림에 새 고객층을 사용할 수 있으려면 24시간이 소요됩니다. 자세한 내용은 [고객층 만들기](create-customer-segments.md)를 참조하세요.
6. 알림을 보낼 시간을 지정하고자 하는 경우 **즉시 알림 보내기** 확인란의 선택을 취소하고 특정 날짜 및 시간을 선택합니다(모든 고객의 현지 시간대를 지정하지 않는 이상 모든 고객에 대해 UTC 형식으로).
7. 알림의 만료 시점을 지정하려면 **알림 만료 없음** 확인란을 지우고 특정 만료 날짜와 시간(UTC 형식으로)을 선택합니다.
8. **단일 앱에 대한 알림의 경우:** 특정 언어 사용자, 특정 시간대의 사용자에게만 알림을 전달하기 위해 수신자를 필터링하려면 **필터 사용** 확인란을 체크합니다. 사용하고 싶은 언어 및/또는 시간대를 지정할 수 있습니다.
8. **여러 앱에 대한 알림의 경우:** (고객당)각 디바이스의 마지막 활성 앱 또는 각 디바이스의 모든 앱에 알림을 보낼 것인지 여부를 지정합니다.
10. **알림 콘텐츠** 섹션의 **언어** 메뉴에서 알림을 표시할 언어를 선택합니다. 자세한 내용은 [알림 번역](#translate-your-notifications)을 참조하세요.
11. **옵션** 섹션에서 텍스트를 입력하고 원하는 다른 옵션을 구성합니다. 템플릿으로 시작한 경우 몇몇 옵션은 기본적으로 제공되지만 원하는 대로 변경할 수 있습니다.

    사용 가능한 옵션은 사용하는 알림 유형에 따라 달라집니다. 몇몇 옵션은 다음과 같습니다.

    * **정품 인증 유형**(대화형 알림 유형). **전경**, **백그라운드** 또는 **프로토콜**을 선택할 수 있습니다.
    * **시작**(대화형 알림 유형). 알림에서 앱 또는 웹 사이트를 열도록 선택할 수 있습니다.
    * **앱 시작 속도 추적**(대화형 알림 유형). 각 알림을 통해 고객의 참여를 얼마나 잘 유도하는지를 측정하려면 이 확인란을 선택합니다. 자세한 내용은 [알림 성과 측정](#measure-notification-performance)을 참조하세요.
    * **기간**(대화형 알림 유형). **짧게** 또는 **길게**를 선택합니다.
    * **시나리오**(대화형 알림 유형). **기본**, **알람**, **미리 알림** 또는 **수신 전화**를 선택할 수 있습니다.
    * **기본 URI**(대화형 알림 유형). 자세한 내용은 [BaseUri](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.baseuri#Windows_UI_Xaml_FrameworkElement_BaseUri)를 참조하세요.
    * **이미지 쿼리 추가**(대화형 알림 유형). 자세한 내용은 [addImageQuery](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/element-visual#attributes-and-elements)를 참조하세요.
    * **시각 효과**. 이미지, 동영상 또는 소리. 자세한 내용은 [visual](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/element-visual)을 참조하세요.
    * **입력**/**작업**/**선택**(대화형 알림 유형). 사용자와 알림의 상호 작용을 허용할 수 있습니다. 자세한 내용은 [적응형 및 대화형 알림 메시지](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)를 참조하세요.
    * **바인딩**(대화형 타일 유형). 알림 템플릿. 자세한 내용은 [binding](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/element-binding)을 참조하세요.

    > [!TIP]
    > 적응형 타일 및 대화형 알림 메시지를 디자인하고 테스트하려면 [알림 시각화 도우미](https://www.microsoft.com/store/apps/9nblggh5xsl1) 앱을 사용해 보세요.

12. 나중에 알림 작업을 계속 수행하려면 **초안으로 저장**을 선택하고, 모두 완료했으면 **보내기**를 선택합니다.


## <a name="notification-template-types"></a>알림 템플릿 유형

다양한 알림 템플릿 중에서 선택할 수 있습니다.

-   **비어 있는 템플릿(알림).** 사용자 지정 가능한 비어 있는 알림 메시지로 시작합니다. 알림 메시지란 고객이 다른 앱, 시작 화면 또는 데스크톱에 있을 때 앱과 통신하도록 허용하는, 화면에 표시되는 팝업 UI입니다.
-   **비어 있는 템플릿(타일).** 사용자 지정 가능한 비어 있는 타일 알림으로 시작합니다. 타일은 시작 화면에서 앱이 표시되는 방식입니다. 타일은 "라이브"일 수 있습니다. 즉, 표시되는 내용이 알림에 응답하여 변경될 수 있습니다.
-   **평점 요청(알림).** 고객에게 앱 평가를 요청하는 알림 메시지. 알림을 선택하면 앱에 대한 Microsoft Store 평점 페이지가 표시됩니다.
-   **피드백 요청(알림).** 고객에게 앱 피드백을 요청하는 알림 메시지. 알림을 선택하면 앱에 대한 피드백 허브 페이지가 표시됩니다.
    > [!NOTE]
    > **시작** 상자에서 이 템플릿 유형을 선택하는 경우 {PACKAGE_FAMILY_NAME} 자리 표시자 값을 앱의 실제 PFN(패키지 패밀리 이름)으로 교체해야 합니다. [앱 ID](view-app-identity-details.md) 페이지에서 앱의 PFN을 찾을 수 있습니다(**앱 관리** > **앱 ID**).

    ![피드백 알림 시작 상자](images/push-notifications-feedback-toast-launch-box.png)

-   **교차 홍보(알림).** 선택한 다른 앱을 홍보하기 위한 알림 메시지. 고객이 알림을 선택하면 다른 앱의 Microsoft Store 목록이 표시됩니다.
    > [!NOTE]
    > **시작** 상자에서 이 템플릿 유형을 선택하는 경우 **{여기에 홍보할 ProductId 입력}** 자리 표시자 값을 교차 홍보할 항목의 실제 Microsoft Store ID로 교체해야 합니다. [앱 ID](view-app-identity-details.md) 페이지에서 Microsoft Store ID를 찾을 수 있습니다(**앱 관리** > **앱 ID**).

    ![교차 홍보 알림 시작 상자](images/push-notifications-promote-toast-launch-box.png)

-   **판매 홍보(알림).** 앱 특가 판매를 발표하기 위해 사용할 수 있는 알림 메시지. 고객이 알림을 선택하면 앱의 Microsoft Store 목록이 표시됩니다.
-   **업데이트 확인(알림).** 앱의 이전 버전을 실행 중인 고객에게 최신 버전을 설치하도록 권고하는 알림 메시지. 고객이 알림을 선택하면 Microsoft Store 앱이 실행되며 **다운로드 및 업데이트** 목록이 표시됩니다. 이 템플릿은 하나의 앱으로만 사용할 수 있으며 특정 고객층을 대상으로 지정하거나 이를 전송할 시간을 정의할 수 없습니다. 이 알림은 항상 24시간 내에 전송되도록 되어 있으며 아직 최신 버전의 앱을 실행하지 않는 모든 사용자를 대상으로 지정할 수 있도록 노력하고 있습니다.


## <a name="measure-notification-performance"></a>알림 성과 측정

각 알림을 통해 고객의 참여를 얼마나 잘 유도하는지를 측정할 수 있습니다.


### <a name="to-measure-notification-performance"></a>알림 성과를 측정하려면

1.  알림을 만들 때 **알림 콘텐츠** 섹션에서 **앱 시작 속도 추적** 확인란을 선택합니다.
2.  대상 지정 알림에 대한 응답으로 앱이 시작되었음을 개발자 센터에 알리려면 앱에서 [ParseArgumentsAndTrackAppLaunch](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) 메서드를 호출합니다. 이 메서드는 Microsoft Store Services SDK에서 제공됩니다. 이 메서드를 호출하는 방법에 대한 자세한 내용은 [개발자 센터 알림을 받도록 앱 구성](../monetize/configure-your-app-to-receive-dev-center-notifications.md)을 참조하세요.


### <a name="to-view-notification-performance"></a>알림 성과 보기

위에서 설명한 대로 알림 성과를 측정하도록 알림과 앱을 구성한 경우 대시보드를 사용하여 알림의 성과를 확인할 수 있습니다.

각 알림에 대 한 자세한 데이터를 검토 합니다.

1.  Windows 개발자 센터 대시보드에서 **참여** 섹션을 확장하고 **알림**을 선택합니다.
2.  기존 알림 표에 **진행** 중이거나 **완료**선택 하 고 각 알림의 높은 수준의 성능을 참조 하려면 **배달 속도** 및 **응용 프로그램 실행 속도** 열을 봅니다.
3.  좀 더 자세한 성과 세부 정보를 보려면 알림 이름을 선택합니다. **배달 통계** 섹션이 나타나고, 다음 알림**상태** 유형에 대한 **개수** 및 **백분율** 정보가 표시됩니다.
    * **실패**: 어떤 이유에서든 알림이 배달되지 않았습니다. 예를 들어 Windows 알림 서비스에서 문제가 발생하는 경우 이 오류가 발생할 수 있습니다.
    * **채널 만료 오류**: 앱과 개발자 센터 간 채널이 만료되었으므로 알림을 배달할 수 없습니다. 예를 들어 고객이 오랫동안 앱을 열지 않은 경우 이 오류가 발생할 수 있습니다.
    * **보내는 중**: 전송할 큐에 알림이 있습니다.
    * **보냄**: 알림을 보냈습니다.
    * **시작**: 알림을 보냈고 고객이 클릭했으며 그 결과 앱이 열렸습니다. 여기에서는 앱 시작만 추적합니다. 다른 작업(예: Microsoft Store를 시작하여 평가 남기기)을 수행하도록 고객을 초대하는 알림은 이 상태에 포함되지 않습니다.
    * **알 수 없음**: 이 알림의 상태를 확인할 수 없습니다.

모든 알림에 대 한 사용자 작업 데이터를 분석 합니다.

1.  Windows 개발자 센터 대시보드에서 **참여** 섹션을 확장하고 **알림**을 선택합니다.
2.  **알림** 페이지에서 **분석** 탭을 클릭 합니다. 이 탭에는 다음과 같은 데이터가 표시 됩니다.
    * 팝업 알림 및 작업 센터 알림에 대 한 다양 한 사용자 작업 상태 보기를 그래프로 표시 합니다.
    * 팝업 알림을 작업에 대 한 비율이-를 통해-클릭의 세계 지도 보기를 가운데에 알림을.
3. 페이지 위쪽에서 데이터가 표시되는 기간을 선택할 수 있습니다. 기본 설정은 30D (30일)이지만, 3개월, 6개월 또는 12개월 동안 데이터를 표시하거나 사용자가 지정한 데이터 범위에서 데이터를 표시하도록 선택할 수 있습니다. 모든 응용 프로그램 및 시장 데이터를 필터링 할 **필터** 확장할 수 있습니다.

## <a name="translate-your-notifications"></a>알림 번역

알림의 영향을 최대화하려면 고객이 기본적으로 사용하는 언어로 알림을 번역하는 것이 좋습니다. [Microsoft Translator](https://www.microsoft.com/translator/home.aspx) 서비스를 활용하면 개발자 센터에서 알림을 원하는 언어로 자동으로 손쉽게 번역할 수 있습니다.

1.  기본 언어로 알림을 작성한 후 **언어 추가**(**알림 콘텐츠** 섹션의 **언어** 메뉴 아래)를 선택합니다.
2.  **언어 추가** 창에서 알림을 표시할 추가 언어를 선택한 다음 **업데이트**를 선택합니다.
**언어 추가** 창에서 선택한 언어로 알림이 자동으로 번역되고 해당 언어가 **언어** 메뉴에 추가됩니다.
3.  알림의 번역 내용을 보려면 **언어** 메뉴에서 방금 추가한 언어를 선택합니다.

번역에 대해 고려할 사항:
 - 해당 언어에 대한 **콘텐츠** 상자에 다른 내용을 입력하여 자동 번역을 재정의할 수 있습니다.
 - 자동 번역을 재정의한 후 또 다른 텍스트 상자를 알림의 영어 버전에 추가하는 경우 새 텍스트 상자는 번역된 알림에 추가되지 않습니다. 이 경우 번역된 각 알림에 새 텍스트 상자를 수동으로 추가해야 합니다.
 - 알림이 번역된 후 영어 텍스트를 변경하는 경우 변경 사항과 일치하도록 번역된 알림이 자동으로 업데이트됩니다. 그러나 이전에 초기 번역을 재정의하도록 선택한 경우에는 자동으로 업데이트되지 않습니다.

## <a name="related-topics"></a>관련 항목
- [UWP 앱의 타일](../design/shell/tiles-and-notifications/creating-tiles.md)
- [Windows 푸시 알림 서비스 개요](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)
- [알림 시각화 도우미 앱](https://www.microsoft.com/store/apps/9nblggh5xsl1)
- [StoreServicesEngagementManager.RegisterNotificationChannelAsync() | registerNotificationChannelAsync() 메서드](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync)
