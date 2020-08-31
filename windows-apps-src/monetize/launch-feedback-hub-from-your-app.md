---
Description: 앱에서 피드백 허브를 시작 하 여 고객에 게 피드백을 남겨 두는 것이 좋습니다.
title: 앱에서 피드백 허브 시작
ms.assetid: 070B9CA4-6D70-4116-9B18-FBF246716EF0
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 피드백 허브, 시작
ms.localizationpriority: medium
ms.openlocfilehash: efdc4a4b39f71b26658e3fbaf57287098b23e4be
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158497"
---
# <a name="launch-feedback-hub-from-your-app"></a>앱에서 피드백 허브 시작

피드백 허브를 실행 하는 유니버설 Windows 플랫폼 (UWP) 앱에 컨트롤 (예: 단추)을 추가 하 여 고객에 게 피드백을 남겨 두는 것이 좋습니다. 피드백 허브는 Windows 및 설치 된 앱에 대 한 피드백을 수집할 수 있는 단일 장소를 제공 하는 사전 설치 된 앱입니다. 피드백 허브를 통해 앱에 대해 제출 되는 모든 사용자 의견은 파트너 센터의 [사용자 의견 보고서](../publish/feedback-report.md) 에 수집 되 고 사용자에 게 표시 되므로 고객이 한 보고서에서 제출한 문제, 제안 및 upvotes를 볼 수 있습니다.

앱에서 피드백 허브를 시작 하려면 [Microsoft Store SERVICES SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK)에서 제공 하는 API를 사용 합니다. 이 API를 사용 하 여 디자인 지침을 따르는 응용 프로그램의 UI 요소에서 피드백 허브를 시작 하는 것이 좋습니다.

> [!NOTE]
> 사용자 의견 허브는 데스크톱 및 모바일 [장치 제품군](../get-started/universal-application-platform-guide.md)을 기반으로 하는 WINDOWS 10 OS 버전 10.0.14271 이상을 실행 하는 장치 에서만 사용할 수 있습니다. 사용자의 장치에서 피드백 허브를 사용할 수 있는 경우에만 앱에 피드백 컨트롤을 표시 하는 것이 좋습니다. 이 항목의 코드에서는이 작업을 수행 하는 방법을 보여 줍니다.

## <a name="how-to-launch-feedback-hub-from-your-app"></a>앱에서 피드백 허브를 시작 하는 방법

앱에서 피드백 허브를 시작 하려면 다음을 수행 합니다.

1. [Microsoft Store SERVICES SDK를 설치](microsoft-store-services-sdk.md#install-the-sdk)합니다.
2. Visual Studio에서 프로젝트를 엽니다.
3. 솔루션 탐색기에서 프로젝트에 대 한 **참조** 노드를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가**를 클릭 합니다.
4. **참조 관리자**에서 **유니버설 Windows** 를 확장 하 고 **확장**을 클릭 합니다.
5. Sdk 목록에서 **Microsoft Engagement 프레임 워크** 옆의 확인란을 클릭 하 고 **확인**을 클릭 합니다.
6. 프로젝트에서 사용자에 게 표시 하려는 컨트롤을 추가 하 여 단추와 같은 피드백 허브를 시작 합니다. 다음과 같이 컨트롤을 구성 하는 것이 좋습니다.
  * 컨트롤에 표시 되는 콘텐츠의 글꼴을 **SEGOE MDL2 자산**으로 설정 합니다.
  * 컨트롤의 텍스트를 16 진수 유니코드 문자 코드 E939로 설정 합니다. 이는 **SEGOE MDL2 자산** 글꼴의 권장 되는 피드백 아이콘에 대 한 문자 코드입니다.
  * 컨트롤의 표시 유형을 숨김으로 설정 합니다.
    > [!NOTE]
    > 사용자의 장치에서 피드백 허브를 사용할 수 있는 경우에만 기본적으로 피드백 컨트롤을 숨기고 초기화 코드에 표시 하는 것이 좋습니다. 다음 단계에서는이 작업을 수행 하는 방법을 보여 줍니다.

    다음 코드에서는 위에서 설명한 대로 구성 된 [단추의](/uwp/api/Windows.UI.Xaml.Controls.Button) XAML 정의를 보여 줍니다.

    ```XML
    <Button x:Name="feedbackButton" FontFamily="Segoe MDL2 Assets" Content="&#xE939;" HorizontalAlignment="Left" Margin="138,352,0,0" VerticalAlignment="Top" Visibility="Collapsed"  Click="feedbackButton_Click"/>
    ```

7. 피드백 컨트롤을 호스트 하는 앱 페이지에 대 한 초기화 코드에서 [StoreServicesFeedbackLauncher](/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher) 클래스의 정적 [issupported](/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher.issupported) 메서드를 사용 하 여 사용자의 장치에서 피드백 허브를 사용할 수 있는지 여부를 확인 합니다. 사용자 의견 허브는 데스크톱 및 모바일 [장치 제품군](../get-started/universal-application-platform-guide.md)을 기반으로 하는 WINDOWS 10 OS 버전 10.0.14271 이상을 실행 하는 장치 에서만 사용할 수 있습니다.

    이 속성이 **true**를 반환 하는 경우 컨트롤을 표시 합니다. 다음 코드에서는 [단추](/uwp/api/windows.ui.xaml.controls.button)에 대해이 작업을 수행 하는 방법을 보여 줍니다.

    [!code-csharp[LaunchFeedback](./code/StoreSDKSamples/cs/FeedbackPage.xaml.cs#ToggleFeedbackVisibility)]
      > [!NOTE]
      > 현재는 Xbox 장치에서 피드백 허브가 지원 되지 않지만, **Issupported** 속성은 현재 Windows 10 버전 10.0.14271 이상을 실행 하는 xbox 장치에서 **true** 를 반환 합니다. 이 문제는 알려진 문제 이며 Microsoft Store Services SDK의 이후 릴리스에서 수정 될 예정입니다.  

8. 사용자가 컨트롤을 클릭할 때 실행 되는 이벤트 처리기에서 [StoreServicesFeedbackLauncher](/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher) 개체를 가져오고 [LaunchAsync](/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher.launchasync) 메서드를 호출 하 여 피드백 허브 앱을 시작 합니다. 이 메서드에는 두 개의 오버 로드가 있습니다. 하나는 매개 변수가 없는 것이 고, 다른 하나는 사용자 의견에 연결할 메타 데이터를 포함 하는 키 및 값 쌍의 사전을 허용 합니다. 다음 예제에서는 [단추](/uwp/api/Windows.UI.Xaml.Controls.Button)에 대 한 [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 이벤트 처리기에서 피드백 허브를 시작 하는 방법을 보여 줍니다.

    [!code-csharp[LaunchFeedback](./code/StoreSDKSamples/cs/FeedbackPage.xaml.cs#FeedbackButtonClick)]

## <a name="design-recommendations-for-your-feedback-ui"></a>피드백 UI에 대 한 디자인 권장 사항

피드백 허브를 시작 하려면 Segoe MDL2 자산 글꼴 및 문자 코드 E939에서 다음 표준 피드백 아이콘을 표시 하는 UI 요소를 앱에 추가 하는 것이 좋습니다 (예: 단추).

![피드백 아이콘](images/feedback_icon.PNG)

또한 앱에서 피드백 허브에 연결 하는 데 다음 배치 옵션 중 하나 이상을 사용 하는 것이 좋습니다.
* **앱 바에서 직접**. 구현에 따라 아이콘만 사용 하거나 아래와 같이 텍스트를 추가할 수 있습니다.

  ![피드백 아이콘](images/feedback_appbar_placement.png)

* **앱의 설정에서**. 이 방법은 피드백 허브에 대 한 액세스를 제공 하는 보다 미묘한 방법입니다. 아래 예제에서는 피드백 링크가 앱 아래의 링크 중 하나로 표시 됩니다.

  ![피드백 아이콘](images/feedback_settings_placement.png)

* **이벤트 기반 플라이 아웃** 이는 Windows 피드백 허브로 시작 하기 전에 특정 질문에 대해 고객을 쿼리 하려는 경우에 유용 합니다. 예를 들어 앱이 특정 기능을 사용 하는 경우 해당 기능에 대 한 특정 질문을 고객에 게 표시할 수 있습니다. 고객이 응답을 선택 하는 경우 앱에서 피드백 허브를 시작 합니다.


## <a name="related-topics"></a>관련 항목

* [피드백 보고서](../publish/feedback-report.md)