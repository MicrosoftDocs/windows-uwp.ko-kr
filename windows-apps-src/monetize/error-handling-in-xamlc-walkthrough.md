---
author: Xansky
ms.assetid: cf0d2709-21a1-4d56-9341-d4897e405f5d
description: 앱에서 AdControl 오류를 검색하는 방법을 알아봅니다.
title: XAML/C#에서 오류 처리 연습
ms.author: mhopkins
ms.date: 05/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 광고, 광고, 오류 처리, XAML, c#
ms.localizationpriority: medium
ms.openlocfilehash: e4387ccb8ef7eb02cb0043530a4e683f7917f421
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5473493"
---
# <a name="error-handling-in-xamlc-walkthrough"></a>XAML/C#에서 오류 처리 연습

이 연습에서는 앱에서 광고 관련 오류를 검색하는 방법을 보여 줍니다. 이 연습은 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol)을 사용하여 배너를 표시하는 것을 안내하지만 일반적인 개념은 중간 광고 및 기본 광고에도 적용됩니다.

이러한 예제에서는 **AdControl**이 포함된 XAML/C# 앱이 있다고 가정합니다. 앱에 **AdControl**을 추가하는 방법을 보여 주는 단계별 지침은 [XAML 및 .NET의 AdControl](adcontrol-in-xaml-and--net.md)을 참조하세요. 

1.  MainPage.xaml 파일에서 **AdControl**에 대한 정의를 찾습니다. 코드는 다음과 같습니다.
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

2.   **Width** 속성 뒤, 닫는 태그 앞에서, 오류 이벤트 처리기의 이름을 [ErrorOccurred](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.erroroccurred) 이벤트에 할당합니다. 이 연습에서 오류 이벤트 처리기의 이름은 **OnAdError**입니다.
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

3.  런타임에 오류를 생성하려면 다른 응용 프로그램 ID를 사용하여 두 번째 **AdControl**을 만듭니다. 앱의 모든 **AdControl** 개체는 동일한 응용 프로그램 ID를 사용해야 하므로 다른 응용 프로그램 ID를 사용하여 추가 **AdControl**을 만들면 오류가 발생합니다.

    MainPage.xaml에서 첫 번째 **AdControl** 바로 다음에 두 번째 **AdControl**을 정의하고 [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) 속성을 "0"으로 설정합니다.
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

4.  MainPage.xaml.cs에서 다음 **OnAdError** 이벤트 처리기를 **MainPage** 클래스에 추가합니다. 이 이벤트 처리기는 Visual Studio **출력** 창에 정보를 씁니다.
    ``` csharp
    private void OnAdError(object sender, AdErrorEventArgs e)
    {
        System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name +
            "): " + e.ErrorMessage + " ErrorCode: " + e.ErrorCode.ToString());
    }
    ```

4.  프로젝트를 빌드하고 실행합니다. 앱이 실행되는 동안 Visual Studio의 **출력** 창에 아래와 비슷한 메시지가 표시됩니다.
    ```
    AdControl error (): MicrosoftAdvertising.Shared.AdException: all ad requests must use the same application ID within a single application (0, d25517cb-12d4-4699-8bdc-52040c712cab) ErrorCode: ClientConfiguration
    ```

## <a name="related-topics"></a>관련 항목

* [GitHub의 광고 샘플](http://aka.ms/githubads)
