---
ms.assetid: cf0d2709-21a1-4d56-9341-d4897e405f5d
title: XAML/C#에서 오류 처리 연습
description: '이 연습을 수행 하 여 XAML/c # 앱에서 AdControl의 오류를 catch 하 고 처리 하는 방법을 알아봅니다.'
ms.date: 02/18/2020
ms.topic: article
keywords: 'windows 10, uwp, 광고, 광고, 오류 처리, XAML, c #'
ms.localizationpriority: medium
ms.openlocfilehash: 7ad3d7dfcbf191960fba03808270a55d3128d4f0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158587"
---
# <a name="error-handling-in-xamlc-walkthrough"></a>XAML/C#에서 오류 처리 연습

>[!WARNING]
> 2020 년 6 월 1 일부 터 Windows UWP 앱 용 Microsoft Ad 수익 화 플랫폼이 종료 됩니다. [자세한 정보](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

이 연습에서는 앱에서 광고 관련 오류를 catch 하는 방법을 보여 줍니다. 이 연습에서는 [Adcontrol](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 을 사용 하 여 배너 광고를 표시 하지만이의 일반적인 개념은 중간 광고 및 네이티브 광고에도 적용 됩니다.

이러한 예제에서는 **Adcontrol**을 포함 하는 XAML/c # 앱이 있다고 가정 합니다. 응용 프로그램에 **Adcontrol** 을 추가 하는 방법을 보여 주는 단계별 지침은 [XAML 및 .Net의 adcontrol](adcontrol-in-xaml-and--net.md)을 참조 하세요. 

1.  MainPage .xaml 파일에서 **Adcontrol**에 대 한 정의를 찾습니다. 해당 코드는 다음과 같습니다.
    ``` xml
    <UI:AdControl
      ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
      AdUnitId="test"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,10,0,0"
      VerticalAlignment="Top"
      Width="300" />
    ```

2.   **Width** 속성 뒤에 닫는 태그 앞에서 오류 이벤트 처리기의 이름을 [erroroccurred](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.erroroccurred) 이벤트에 할당 합니다. 이 연습에서 오류 이벤트 처리기의 이름은 **Onaderror**입니다.
    ``` xml
    <UI:AdControl
      ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
      AdUnitId="test"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,10,0,0"
      VerticalAlignment="Top"
      Width="300"
      ErrorOccurred="OnAdError"/>
    ```

3.  런타임에 오류를 생성 하려면 다른 응용 프로그램 ID를 사용 하 여 두 번째 **Adcontrol** 을 만듭니다. 앱의 모든 **adcontrol** 개체는 동일한 응용 프로그램 id를 사용 해야 하므로 다른 응용 프로그램 id를 사용 하 여 추가 **adcontrol** 을 만들면 오류가 발생 합니다.

    첫 번째 **adcontrol**바로 뒤의 mainpage. xaml에서 두 번째 **adcontrol** 을 정의 하 고 [ApplicationId](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) 속성을 0 ("0")으로 설정 합니다.
    ``` xml
    <UI:AdControl
        ApplicationId="0"
        AdUnitId="test"
        HorizontalAlignment="Left"
        Height="250"
        Margin="10,265,0,0"
        VerticalAlignment="Top"
        Width="300"
        ErrorOccurred="OnAdError" />
    ```

4.  MainPage.xaml.cs에서 다음 **Onaderror** 이벤트 처리기를 **mainpage** 클래스에 추가 합니다. 이 이벤트 처리기는 Visual Studio **출력** 창에 정보를 씁니다.
    ``` csharp
    private void OnAdError(object sender, AdErrorEventArgs e)
    {
        System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name +
            "): " + e.ErrorMessage + " ErrorCode: " + e.ErrorCode.ToString());
    }
    ```

4.  프로젝트를 빌드하고 실행합니다. 앱을 실행 한 후에는 Visual Studio의 **출력** 창에 아래와 비슷한 메시지가 표시 됩니다.
    ```json
    AdControl error (): MicrosoftAdvertising.Shared.AdException: all ad requests must use the same application ID within a single application (0, d25517cb-12d4-4699-8bdc-52040c712cab) ErrorCode: ClientConfiguration
    ```

## <a name="related-topics"></a>관련 항목

* [GitHub의 광고 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)