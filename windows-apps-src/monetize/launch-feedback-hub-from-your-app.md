---
Description: You can encourage your customers to leave feedback by launching Feedback Hub from your app.
title: 앱에서 피드백 허브 시작
ms.assetid: 070B9CA4-6D70-4116-9B18-FBF246716EF0
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 피드백 허브, 시작
ms.localizationpriority: medium
ms.openlocfilehash: b4e11b8dfffd7e749a31f052545bfbdfc4449126
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8469028"
---
# <a name="launch-feedback-hub-from-your-app"></a>앱에서 피드백 허브 시작

피드백 허브를 시작하는 UWP(유니버설 Windows 플랫폼) 앱에 컨트롤(예: 단추)을 추가하여 피드백을 남기도록 고객을 권유할 수 있습니다. 피드백 허브는 Windows 및 설치된 앱에 대한 피드백을 수집할 단일 위치를 제공하는 사전 설치된 앱입니다. 피드백 허브를 통해 앱에 대 한 제출 되는 모든 고객 피드백 수집 되어 문제, 제안 및 좋아요를 하나의 보고서에서 고객이 제출한 볼 수 있도록 파트너 센터에서 [피드백 보고서](../publish/feedback-report.md) 에서 귀하에 게 제공 됩니다.

앱에서 피드백 허브를 시작하려면 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)에서 제공하는 API를 사용합니다. 이 API를 사용하여 디자인 지침을 따르는 앱의 UI 요소에서 피드백 허브를 시작하는 것이 좋습니다.

> [!NOTE]
> 피드백 허브는 데스크톱과 모바일 [ 패밀리](https://msdn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide#device-families)를 기반으로 하는 Windows10 OS 버전 10.0.14271 이상을 실행하는 장치에서만 사용할 수 있습니다. 사용자 장치에서 피드백 허브를 사용할 수 있는 경우에만 앱에 피드백 컨트롤을 표시하는 것이 좋습니다. 이 항목의 코드는 이 작업을 수행하는 방법을 보여 줍니다.

## <a name="how-to-launch-feedback-hub-from-your-app"></a>앱에서 피드백 허브를 시작하는 방법

앱에서 피드백 허브를 시작하려면

1. [Microsoft Store Services SDK를 설치합니다.](microsoft-store-services-sdk.md#install-the-sdk)
2. Visual Studio에서 프로젝트를 엽니다.
3. 솔루션 탐색기에서 프로젝트의 **참조** 노드를 마우스 오른쪽 단추로 클릭하고 **참조 추가**를 클릭합니다.
4. **참조 관리자**에서 **유니버설 Windows**를 확장하고 **확장**을 클릭합니다.
5. SDK 목록에서 **Microsoft Engagement Framework**(Microsoft 참여 프레임워크) 옆의 확인란을 클릭하고 **확인**을 클릭합니다.
6. 프로젝트에서 사용자에게 표시하려는 피드백 허브를 시작하는 컨트롤(예: 단추)을 추가합니다. 컨트롤을 다음과 같이 구성하는 것이 좋습니다.
  * 컨트롤에 표시된 콘텐츠의 글꼴을 **Segoe MDL2 자산**으로 설정합니다.
  * 컨트롤의 텍스트를 16진수 유니코드 문자 코드 E939로 설정합니다. 이는 **Segoe MDL2 자산** 글꼴에서 권장되는 피드백 아이콘에 대한 문자 코드입니다.
  * 컨트롤의 표시 여부를 숨김으로 설정합니다.
    > [!NOTE]
    > 기본적으로 피드백 컨트롤을 숨기고 사용자 장치에서 피드백 허브를 사용할 수 있는 경우에만 초기화 코드에 표시하는 것이 좋습니다. 다음 단계에서는 이 작업을 수행하는 방법을 보여 줍니다.

    다음 코드는 위에서 설명한 대로 구성된 [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)의 XAML 정의를 보여 줍니다.

    ```XML
    <Button x:Name="feedbackButton" FontFamily="Segoe MDL2 Assets" Content="&#xE939;" HorizontalAlignment="Left" Margin="138,352,0,0" VerticalAlignment="Top" Visibility="Collapsed"  Click="feedbackButton_Click"/>
    ```

7. 피드백 컨트롤을 호스트하는 앱 페이지의 초기화 코드에서 [StoreServicesFeedbackLauncher](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher.issupported) 클래스의 정적 [IsSupported](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher) 메서드를 사용하여 사용자 장치에서 피드백 허브를 사용할 수 있는지 여부를 확인합니다. 피드백 허브는 데스크톱과 모바일 [ 패밀리](https://msdn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide#device-families)를 기반으로 하는 Windows10 OS 버전 10.0.14271 이상을 실행하는 장치에서만 사용할 수 있습니다.

    이 속성이 **true**를 반환하는 경우 컨트롤을 표시되도록 설정합니다. 다음 코드는 [Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)에 대해 이 작업을 수행하는 방법을 보여 줍니다.

    [!code-cs[LaunchFeedback](./code/StoreSDKSamples/cs/FeedbackPage.xaml.cs#ToggleFeedbackVisibility)]
      > [!NOTE]
      > 피드백 허브는 현재 Xbox 장치에서 지원되지 않지만 **IsSupported** 속성은 Windows 10 버전 10.0.14271 이상을 실행하는 Xbox 장치에서 현재 **true**를 반환합니다. 이것은 알려진 문제로 Microsoft Store Services SDK의 이후 릴리스에서 수정될 예정입니다.  

8. 사용자가 컨트롤을 클릭할 때 실행되는 이벤트 처리기에서 [StoreServicesFeedbackLauncher](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher) 개체를 가져오고 [LaunchAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher.launchasync) 메서드를 호출하여 피드백 허브 앱을 시작합니다. 이 메서드에 대한 두 개의 오버로드가 있습니다. 하나는 매개 변수가 없고, 다른 하나는 피드백과 연결하려는 메타데이터를 포함하는 키/값 쌍의 사전을 사용합니다. 다음 예제는 [Button](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)의 [Click](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) 이벤트 처리기에서 피드백 허브를 실행하는 방법을 보여 줍니다.

    [!code-cs[LaunchFeedback](./code/StoreSDKSamples/cs/FeedbackPage.xaml.cs#FeedbackButtonClick)]

## <a name="design-recommendations-for-your-feedback-ui"></a>피드백 UI에 대한 디자인 권장 사항

피드백 허브를 시작하려면 Segoe MDL2 자산 글꼴 및 문자 코드 E939에서 다음과 같은 표준 피드백 아이콘을 표시하는 UI 요소(예: 단추)를 앱에 추가하는 것이 좋습니다.

![피드백 아이콘](images/feedback_icon.PNG)

앱에서 피드백 허브 연결에 대해 다음 배치 옵션 중 하나 이상을 사용하는 것이 좋습니다.
* **앱 바에 직접 배치**. 구현에 따라 아이콘만 사용하거나 아래와 같이 텍스트를 추가할 수 있습니다.

  ![피드백 아이콘](images/feedback_appbar_placement.png)

* **앱의 설정에 배치**. 피드백 허브에 대한 액세스를 제공하는 보다 정교한 방법입니다. 아래 예제에서는 앱 아래의 링크 중 하나로 피드백 링크가 나타납니다.

  ![피드백 아이콘](images/feedback_settings_placement.png)

* **이벤트 구동 플라이아웃에 배치**. Windows 피드백 허브를 시작하기 전에 고객에게 특정 질문을 쿼리하려는 경우에 유용합니다. 예를 들어 앱에서 특정 기능을 사용한 후 해당 기능의 고객 만족도에 대한 특정 질문을 고객에게 표시할 수 있습니다. 고객이 응답하도록 선택하면 앱이 피드백 허브를 시작합니다.


## <a name="related-topics"></a>관련 항목

* [피드백 보고서](../publish/feedback-report.md)
