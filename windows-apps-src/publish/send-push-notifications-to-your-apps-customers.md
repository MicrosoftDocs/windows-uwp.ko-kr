---
author: JnHs
Description: "앱을 평가하거나 추가 기능을 구매하는 등의 행동을 유도하기 위해 Windows 개발자 센터에서 앱으로 대상 푸시 알림을 전송하는 방법을 알아보세요."
title: "앱의 고객에게 대상 푸시 알림 보내기"
ms.author: wdg-dev-content
ms.date: 02/28/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 16386c81-702d-47cd-9f91-67659f5dca73
ms.openlocfilehash: ca57ff45d440ebd68f7fb85b7d6a5da0a9f1995c
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="send-targeted-push-notifications-to-your-apps-customers"></a>앱의 고객에게 대상 푸시 알림 보내기

올바른 시간에 올바른 메시지로 고객의 참여를 유도하는 것이 앱 개발자로서 성공하기 위한 핵심입니다. Windows 개발자 센터에서는 푸시 알림을 모든 고객에게 또는 [고객층](create-customer-segments.md)에서 정의한 기준을 충족하는 Windows 10의 일부 고객에게만 전송하기 위해 사용할 수 있는 데이터 기반 고객 참여 플랫폼을 제공합니다.

대상 푸시 알림을 사용하면 앱 평가, 추가 기능 구매, 새 기능 확인, 다른 앱 다운로드 등 고객의 참여를 유도할 수 있습니다.

> **중요** 대상 지정 푸시 알림은 UWP 앱에만 사용할 수 있습니다.

알림의 콘텐츠를 고려할 때 다음을 유의하십시오.
- 알림의 콘텐츠는 스토어 [콘텐츠 정책](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#content_policies)을 준수해야 합니다.
- 알림 콘텐츠에는 기밀 정보나 민감한 정보가 포함되어서는 안 됩니다.
- 알림이 일정에 따라 전달되도록 최선을 다할 것이지만, 전달에 영향을 미치는 지연 문제가 간혹 있을 수 있습니다.
- 너무 자주 알림을 보내지 마십시오. 30분 간격보다 짧게 알림을 보내면 방해가 될 수 있으며, 많은 경우에 사람들은 알림을 덜 자주 받기를 선호합니다.
- 여러분의 앱을 사용하는 고객이 나중에 장치를 다른 사람에게 사용하라고 주었을 경우(또한 고객층 멤버 자격을 판단할 때 본래 사용자의 Microsoft 계정으로 로그인되어 있을 경우), 원래 목표한 대상이 아닌 다른 사람에게 알림이 전달될 수 있음을 유의하십시오. 자세한 내용은 [앱에서 대상 푸시 알림 구성](../monetize/configure-your-app-to-receive-dev-center-notifications.md#notification-customers)을 참조하세요.

## <a name="getting-started-with-push-notifications"></a>푸시 알림 시작

고객 참여를 유도하기 위한 푸시 알림을 사용하려면 상위 수준에서 세 가지를 수행해야 합니다.
1. **푸시 알림을 받기 위해 앱을 등록합니다.** 앱에서 Microsoft Store Services SDK에 대한 참조를 추가한 다음, 개발자 센터와 앱 사이의 알림 채널을 등록하는 코드 몇 줄을 추가하면 됩니다. 고객에게 푸시 알림을 전달하는 데 이 채널을 사용하게 됩니다. 자세한 내용은 [개발자 센터 알림을 받도록 앱 구성](../monetize/configure-your-app-to-receive-dev-center-notifications.md)을 참조하세요.
2. **대상으로 지정할 하나 이상의 고객층을 만듭니다.** 인구 또는 수익 기준을 기반으로 고객층을 그룹화할 수 있습니다. 자세한 내용은 [고객층 만들기](create-customer-segments.md)를 참조하세요.
3. **푸시 알림을 만들고 특정 고객층으로 보냅니다.** 예를 들면, 새 고객에게 앱을 평가해달라는 알림을 보내거나 추가 기능을 특가에 구매할 수 있다는 알림을 보낼 수 있습니다.

## <a name="to-create-and-send-a-targeted-push-notification"></a>대상 알림 만들기 및 보내기

다음 단계를 따라 대시보드에서 푸시 알림을 만들고 특정 고객층에 보냅니다.

> **참고** 앱이 개발자 센터에서 대상 푸시 알림을 받으려면, 먼저 앱에서 [RegisterNotificationChannelAsync](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx) 메서드를 호출하여 알림을 받도록 앱을 등록해야 합니다. 이 메서드는 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)에서 사용할 수 있습니다. 코드 예제와 함께 이 메서드를 호출하는 방법은 [개발자 센터 알림을 받도록 앱 구성](../monetize/configure-your-app-to-receive-dev-center-notifications.md)을 참조하세요.

1.    [Windows 개발자 센터 대시보드](https://developer.microsoft.com/dashboard/overview)에서 앱을 선택합니다.
2.    왼쪽 탐색 메뉴에서 **서비스**를 확장하고 **푸시 알림**을 선택합니다.
3.    **대상 푸시 알림** 페이지에서 **새 알림**을 선택합니다.
4.    **템플릿 선택** 섹션에서 보낼 알림 유형을 선택합니다. 자세한 내용은 [알림 템플릿 유형](#notification-template-types)을 참조하세요.
  ![알림 템플릿](images/push-notifications-template.png)
5.    **알림 설정** 섹션에서 알림의 **이름**을 선택하고, 알림을 보낼 **고객 그룹**을 선택합니다.
고객층을 아직 만들지 않은 경우 **새 고객 그룹 만들기**를 선택합니다. 알림에 새 고객층을 사용할 수 있으려면 24시간이 소요됩니다. 자세한 내용은 [고객층 만들기](create-customer-segments.md)를 참조하세요.
6.    알림을 언제 보낼지를 지정하려면 **알림 즉시 보내기** 확인란을 지우고 특정 날짜와 시간을 선택합니다.
7.    알림의 만료 시점을 지정하려면 **알림 만료 없음** 확인란을 지우고 특정 만료 날짜와 시간을 선택합니다.
8.    **알림 콘텐츠** 섹션의 **언어** 메뉴에서 알림을 표시할 언어를 선택합니다. 자세한 내용은 [알림 번역](#translate-your-notifications)을 참조하세요.
9.    **옵션** 섹션에서 텍스트를 입력하고 원하는 다른 옵션을 구성합니다. 템플릿으로 시작한 경우 몇몇 옵션은 기본적으로 제공되지만 원하는 대로 변경할 수 있습니다.
   사용 가능한 옵션은 사용하는 알림 유형에 따라 달라집니다. 몇몇 옵션은 다음과 같습니다.
   - **정품 인증 유형**(대화형 알림 유형). **포그라운드**, **백그라운드** 또는 **프로토콜**을 선택할 수 있습니다.
   - **시작**(대화형 알림 유형). 알림에서 앱 또는 웹 사이트를 열도록 선택할 수 있습니다.
   - **앱 시작 속도 추적**(대화형 알림 유형). 각 알림을 통해 고객의 참여를 얼마나 잘 유도하는지를 측정하려면 이 확인란을 선택합니다. 자세한 내용은 [알림 성과 측정](#measure-notification-performance)을 참조하세요.
   - **기간**(대화형 알림 유형). **짧게** 또는 **길게**를 선택합니다.
   - **시나리오**(대화형 알림 유형). **기본**, **알람**, **미리 알림** 또는 **수신 전화**를 선택할 수 있습니다.
   - **기본 URI**(대화형 알림 유형). 자세한 내용은 [BaseUri](https://msdn.microsoft.com/library/windows/apps/br208712)를 참조하세요.
   - **이미지 쿼리 추가**(대화형 알림 유형). 자세한 내용은 [addImageQuery](https://msdn.microsoft.com/library/windows/apps/br230847)를 참조하세요.
   - **시각 효과**. 이미지, 동영상 또는 소리. 자세한 내용은 [visual](https://msdn.microsoft.com/library/windows/apps/br230847)을 참조하세요.
   - **입력**/**작업**/**선택**(대화형 알림 유형). 사용자와 알림의 상호 작용을 허용할 수 있습니다. 자세한 내용은 [적응형 및 대화형 알림 메시지](../controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts.md#actions)를 참조하세요.
   - **바인딩**(대화형 타일 유형). 알림 템플릿. 자세한 내용은 [binding](https://msdn.microsoft.com/library/windows/apps/br230843)을 참조하세요.

   > **팁**  적응형 타일 및 대화형 알림 메시지를 디자인하고 테스트하려면 [알림 시각화 도우미](https://www.microsoft.com/store/apps/9nblggh5xsl1) 앱을 사용해 보세요.

10.    나중에 알림 작업을 계속 수행하려면 **초안으로 저장**을 선택하고, 모두 완료했으면 **보내기**를 선택합니다.

## <a name="notification-template-types"></a>알림 템플릿 유형

다양한 알림 템플릿 중에서 선택할 수 있습니다.

-    **비어 있는 템플릿(알림).** 사용자 지정 가능한 비어 있는 알림 메시지로 시작합니다. 알림 메시지란 고객이 다른 앱, 시작 화면 또는 데스크톱에 있을 때 앱과 통신하도록 허용하는, 화면에 표시되는 팝업 UI입니다.
-    **비어 있는 템플릿(타일).** 사용자 지정 가능한 비어 있는 타일 알림으로 시작합니다. 타일은 시작 화면에서 앱이 표시되는 방식입니다. 타일은 "라이브"일 수 있습니다. 즉, 표시되는 내용이 알림에 응답하여 변경될 수 있습니다.
-    **평점 요청(알림).** 고객에게 앱 평가를 요청하는 알림 메시지. 알림을 선택하면 앱에 대한 스토어 평점 페이지가 표시됩니다.
-    **피드백 요청(알림).** 고객에게 앱 피드백을 요청하는 알림 메시지. 알림을 선택하면 앱에 대한 피드백 허브 페이지가 표시됩니다.
   > **참고** **시작** 상자에서 이 템플릿 유형을 선택하는 경우 {PACKAGE_FAMILY_NAME} 자리 표시자 값을 앱의 실제 PFN(패키지 패밀리 이름)으로 교체해야 합니다. [앱 ID](view-app-identity-details.md) 페이지에서 앱의 PFN을 찾을 수 있습니다(**앱 관리** > **앱 ID**).

   ![피드백 알림 시작 상자](images/push-notifications-feedback-toast-launch-box.png)
-    **교차 홍보(알림).** 선택한 다른 앱을 홍보하기 위한 알림 메시지. 고객이 알림을 선택하면 다른 앱의 스토어 목록이 표시됩니다.
  > **참고** **시작** 상자에서 이 템플릿 유형을 선택하는 경우 **{여기에 홍보할 ProductId 입력}** 자리 표시자 값을 교차 홍보할 항목의 실제 스토어 ID로 교체해야 합니다. [앱 ID](view-app-identity-details.md) 페이지에서 스토어 ID를 찾을 수 있습니다(**앱 관리** > **앱 ID**).

  ![교차 홍보 알림 시작 상자](images/push-notifications-promote-toast-launch-box.png)
-    **판매 홍보(알림).** 앱 특가 판매를 발표하기 위해 사용할 수 있는 알림 메시지. 고객이 알림을 선택하면 앱의 스토어 목록이 표시됩니다.
- **업데이트 확인(알림).** 앱의 이전 버전을 실행 중인 고객에게 최신 버전을 설치하도록 권고하는 알림 메시지. 고객이 알림을 선택하면 스토어 앱의 **다운로드 및 업데이트** 목록이 표시됩니다. 이 템플릿을 사용하기 위해 고객층을 만들 필요는 없습니다. Microsoft에서는 이 알림을 24시간 내에 예약하고, 아직 앱의 최신 버전을 실행하지 않는 모든 사용자를 대상으로 지정하기 위해 노력할 것입니다.

## <a name="measure-notification-performance"></a>알림 성과 측정

각 알림을 통해 고객의 참여를 얼마나 잘 유도하는지를 측정할 수 있습니다.

###<a name="to-measure-notification-performance"></a>알림 성과를 측정하려면

1.    알림을 만들 때 **알림 콘텐츠** 섹션에서 **앱 시작 속도 추적** 확인란을 선택합니다.
2.    대상 지정 알림에 대한 응답으로 앱이 시작되었음을 개발자 센터에 알리려면 앱에서 [ParseArgumentsAndTrackAppLaunch](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch.aspx) 메서드를 호출합니다. 이 메서드는 Microsoft Store SDK에서 제공됩니다. 이 메서드를 호출하는 방법에 대한 자세한 내용은 [Configure your app to receive Dev Center notifications](../monetize/configure-your-app-to-receive-dev-center-notifications.md)(개발자 센터 알림을 받도록 앱 구성)를 참조하세요.

###<a name="to-view-notification-performance"></a>알림 성과 보기

위에서 설명한 대로 [알림 성과를 측정](#to-measure-notification-performance)하도록 알림과 앱을 구성한 경우 대시보드를 사용하여 알림의 성과를 확인할 수 있습니다.

1.  대시보드에서 앱 중 하나를 선택합니다.
2.  해당 앱과 연결된 알림을 보려면 왼쪽 메뉴의 **서비스** 섹션을 확장한 다음, **푸시 알림**을 선택합니다.
3.    각 알림의 전체적인 성과를 살펴보려면 **대상 푸시 알림** 페이지에서 **진행 중** 또는 **완료됨**을 선택하고, **배달 속도** 및 **앱 실행 속도**를 확인합니다.
4.  좀 더 자세한 성과 세부 정보를 보려면 알림 이름을 선택합니다. **배달 통계** 섹션이 나타나고, 다음 알림**상태** 유형에 대한 **개수** 및 **백분율** 정보를 표시합니다.
 - **실패**: 어떤 이유에서든 알림이 배달되지 않았습니다. 예를 들어 Windows 알림 서비스에서 문제가 발생하는 경우 이 오류가 발생할 수 있습니다.
 - **채널 만료 오류**: 앱과 개발자 센터 간 채널이 만료되었으므로 알림을 배달할 수 없습니다. 예를 들어 고객이 오랫동안 앱을 열지 않은 경우 이 오류가 발생할 수 있습니다.
 - **보내는 중**: 전송할 큐에 알림이 있습니다.
 - **보냄**: 알림을 보냈습니다.
 - **시작**: 알림을 보냈고 고객이 클릭했으며 그 결과 앱이 열렸습니다. 여기에서는 앱 시작만 추적합니다. 다른 작업(예: 스토어를 시작하여 평가 남기기)을 수행하도록 고객을 초대하는 알림은 이 상태에 포함되지 않습니다.
 - **알 수 없음**: 이 알림의 상태를 확인할 수 없습니다.

## <a name="translate-your-notifications"></a>알림 번역

알림의 영향을 최대화하려면 고객이 기본적으로 사용하는 언어로 알림을 번역하는 것이 좋습니다. [Microsoft Translator](https://msdn.microsoft.com/library/dd576287.aspx) 서비스를 활용하면 개발자 센터에서 알림을 원하는 언어로 자동으로 손쉽게 번역할 수 있습니다.

1.    기본 언어로 알림을 작성한 후 **언어 추가**(**알림 콘텐츠** 섹션의 **언어** 메뉴 아래)를 선택합니다.
2.    **언어 추가** 창에서 알림을 표시할 추가 언어를 선택한 다음 **업데이트**를 선택합니다.
**언어 추가** 창에서 선택한 언어로 알림이 자동으로 번역되고 해당 언어가 **언어** 메뉴에 추가됩니다.
3.    알림의 번역 내용을 보려면 **언어** 메뉴에서 방금 추가한 언어를 선택합니다.

번역에 대해 고려할 사항:
 - 해당 언어에 대한 **콘텐츠** 상자에 다른 내용을 입력하여 자동 번역을 재정의할 수 있습니다.
 - 자동 번역을 재정의한 후 또 다른 텍스트 상자를 알림의 영어 버전에 추가하는 경우 새 텍스트 상자는 번역된 알림에 추가되지 않습니다. 이 경우 번역된 각 알림에 새 텍스트 상자를 수동으로 추가해야 합니다.
 - 알림이 번역된 후 영어 텍스트를 변경하는 경우 변경 사항과 일치하도록 번역된 알림이 자동으로 업데이트됩니다. 그러나 이전에 초기 번역을 재정의하도록 선택한 경우에는 자동으로 업데이트되지 않습니다.

## <a name="related-topics"></a>관련 항목
- [UWP 앱에 대한 타일, 배지 및 알림](../controls-and-patterns/tiles-badges-notifications.md)
- [WNS(Windows 푸시 알림 서비스) 개요](../controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview.md)
- [WNS(Windows 푸시 알림 서비스) 개요(Windows 런타임 앱)](https://msdn.microsoft.com/library/windows/apps/hh913756.aspx)
- [알림 시각화 도우미 앱](https://www.microsoft.com/store/apps/9nblggh5xsl1)
- [StoreServicesEngagementManager.RegisterNotificationChannelAsync() | registerNotificationChannelAsync() 메서드](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx)
- [고객 구분 및 푸시 알림: 새로운 Windows 개발자 센터 참가자 프로그램 기능(블로그 게시물)](https://blogs.windows.com/buildingapps/2016/08/17/customer-segmentation-and-push-notifications-a-new-windows-dev-center-insider-program-feature/#XTuCqrG8G5IMgWew.97)
