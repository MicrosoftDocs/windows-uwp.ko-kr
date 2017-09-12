---
author: mcleanbyron
ms.assetid: c0450f7b-5c81-4d8c-92ef-2b1190d18af7
description: "AdControl 클래스를 사용하여 Windows Phone 8.1 또는 Windows Phone 8.0용 Silverlight 앱에서 배너 광고를 표시하는 방법을 알아봅니다."
title: "Windows Phone Silverlight의 AdControl"
ms.author: mcleans
ms.date: 07/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 광고, 광고, AdControl, Silverlight, Windows Phone"
ms.openlocfilehash: f1582639757abfb6de156bf88ce8af71ba3eaacd
ms.sourcegitcommit: a9e4be98688b3a6125fd5dd126190fcfcd764f95
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/21/2017
---
# <a name="adcontrol-in-windows-phone-silverlight"></a>Windows Phone Silverlight의 AdControl

이 연습에서는 [AdControl](https://msdn.microsoft.com/library/windows/apps/hh524191.aspx) 클래스를 사용하여 Windows Phone 8.1 또는 Windows Phone 8.0용 Silverlight 앱에서 배너 광고를 표시하는 방법을 보여 줍니다.

<span id="silverlight_support"/>
## <a name="advertising-support-for-windows-phone-8x-silverlight-projects"></a>Windows Phone 8.x Silverlight 프로젝트용 광고 지원

일부 개발자 시나리오는 Windows Phone 8.x Silverlight 프로젝트에서 더 이상 지원되지 않습니다. 자세한 내용은 다음 표를 참조하세요.

|  플랫폼 버전  |  기존 프로젝트    |   새 프로젝트  |
|-----------------|----------------|--------------|
| Windows Phone 8.0 Silverlight     |  Universal Ad Client SDK 또는 Microsoft Advertising SDK의 이전 릴리스에서 **AdControl** 또는 **AdMediatorControl**을 이미 사용하는 기존 Windows Phone 8.0 Silverlight 프로젝트가 있고 이 앱이 Windows 스토어에 이미 게시된 경우, 프로젝트를 수정 및 다시 빌드할 수 있으며 디바이스의 변경 내용을 디버그 또는 테스트할 수 있습니다. 에뮬레이터에서 프로젝트 디버깅 또는 테스트는 지원되지 않습니다.  |  지원되지 않음.  |
| Windows Phone 8.1 Silverlight    |  이전 SDK의 **AdControl** 또는 **AdMediatorControl**을 사용하는 기존 Windows Phone 8.1 Silverlight 프로젝트가 있는 경우 프로젝트를 수정하고 다시 빌드할 수 있습니다. 그러나 앱을 디버그하거나 테스트하려면 에뮬레이터에서 앱을 실행하고 응용 프로그램 ID와 광고 단위 ID에 [테스트 모드 값](test-mode-values.md)을 사용해야 합니다. 디바이스에서 앱 디버깅 또는 테스트는 지원되지 않습니다.  |   새 Windows Phone 8.1 Silverlight 프로젝트에 **AdControl** 또는 **AdMediatorControl**을 추가할 수 있습니다. 그러나 앱을 디버그하거나 테스트하려면 에뮬레이터에서 앱을 실행하고 응용 프로그램 ID와 광고 단위 ID에 [테스트 모드 값](test-mode-values.md)을 사용해야 합니다. 장치에서 앱 디버깅 또는 테스트는 지원되지 않습니다. |

## <a name="add-the-advertising-assemblies-to-your-project"></a>프로젝트에 광고 어셈블리 추가

먼저 프로젝트에 Windows Phone Silverlight용 Microsoft Advertising 어셈블리가 포함된 NuGet 패키지를 다운로드하여 설치합니다.

1.  Visual Studio에서 프로젝트를 엽니다.

2.  **도구**를 클릭하고, **NuGet 패키지 관리자**를 가리키고, **패키지 관리자 콘솔**을 클릭합니다.

3.  **패키지 관리자 콘솔** 창에서 다음 명령 중 하나를 입력합니다.

  * Windows Phone 8.0 대상의 프로젝트인 경우 다음 명령을 입력합니다.

      ```syntax
      Install-Package Microsoft.Advertising.WindowsPhone.SL80 -Version 6.2.40501.1
      ```

  * Windows Phone 8.1 대상의 프로젝트인 경우 다음 명령을 입력합니다.

      ```syntax
      Install-Package Microsoft.Advertising.WindowsPhone.SL81 -Version 8.1.50112
      ```

    명령을 입력한 후 필요한 모든 Silverlight용 Microsoft Advertising 어셈블리가 NuGet 패키지를 통해 로컬 프로젝트에 다운로드되며 이러한 어셈블리에 대한 참조가 프로젝트에 자동으로 추가됩니다.

## <a name="code-your-app"></a>앱 코딩


1.  WMAppManifest.xml 파일의 **Capabilities** 노드에 다음과 같은 기능을 추가합니다.

  ``` syntax
  <Capability Name="ID_CAP_IDENTITY_USER"/>
  <Capability Name="ID_CAP_MEDIALIB_PHOTO"/>
  <Capability Name="ID_CAP_PHONEDIALER"/>
  ```

  이 예제에서는 **Capabilities** 노드가 다음과 같습니다.

  ``` syntax
  <Capabilities>
      <Capability Name="ID_CAP_NETWORKING"/>
      <Capability Name="ID_CAP_MEDIALIB_AUDIO"/>
      <Capability Name="ID_CAP_MEDIALIB_PLAYBACK"/>
      <Capability Name="ID_CAP_SENSORS"/>
      <Capability Name="ID_CAP_WEBBROWSERCOMPONENT"/>
      <Capability Name="ID_CAP_IDENTITY_USER"/>
      <Capability Name="ID_CAP_MEDIALIB_PHOTO"/>
      <Capability Name="ID_CAP_PHONEDIALER"/>
  </Capabilities>
  ```

2.  (옵션) 프로젝트를 저장합니다. **모두 저장** 아이콘을 클릭하거나 **파일** 메뉴에서 **모두 저장**을 클릭합니다.

3.  프로젝트의 Package.appxmanifest 파일에 **인터넷(클라이언트 및 서버)** 기능을 추가합니다. **솔루션 탐색기**에서 **Package.appxmanifest**를 두 번 클릭합니다.

    ![wp81silverlightmarkup\-solutionexplorer\-packageappxmanifest](images/13-b98c2a1a-69c3-4018-be0a-6ce010e703e7.jpg)

    **편집기**에서 **기능** 탭을 클릭합니다. **인터넷(클라이언트 및 서버)** 확인란을 선택합니다.

4.  (옵션) 프로젝트를 저장합니다. **모두 저장** 아이콘을 클릭하거나 **파일** 메뉴에서 **모두 저장**을 클릭합니다.

5.  **Microsoft.Advertising.Mobile.UI** 네임스페이스를 포함하도록 MainPage.xaml 파일의 Silverlight 태그를 수정합니다.

  ``` xml
  xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
  ```

  페이지의 헤더에는 다음 코드가 포함됩니다.

  ``` xml
  xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
  xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
  x:Class="PhoneApp1.MainPage"
  ```

6.  **Grid** 태그에 **AdControl**에 대한 다음 코드를 추가합니다. **ApplicationId** 및 **AdUnitId** 속성을 [테스트 모드 값](test-mode-values.md)에 제공된 테스트 값에 할당하고 **Height** 및 **Width** 속성을 [배너 광고에 대해 지원되는 광고 크기](supported-ad-sizes-for-banner-ads.md) 중 하나로 조정합니다.

  > **참고**&nbsp;&nbsp;앱을 제출하기 전에 테스트 **ApplicationId** 및 **AdUnitId** 값을 라이브 값으로 바꿉니다.

  ``` xml
  <Grid x:Name="ContentPanel" Grid.Row="1">
      <UI:AdControl
          ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
          AdUnitId="10865270"
          HorizontalAlignment="Left"
          Height="80"
          VerticalAlignment="Top"
          Width="480"/>
  </Grid>
  ```

7.  프로젝트를 빌드하고 실행합니다. 앱에 다음과 비슷한 광고가 표시되는지 확인합니다.

  ![wp81silverlight\-emulatorwithad](images/13-8db1492f-ae1d-439b-9b78-bed8e22fe996.jpg)

## <a name="release-your-app-with-live-ads-using-dev-center"></a>개발자 센터를 사용하여 라이브 광고와 함께 앱 출시

1.  개발자 센터 대시보드에서 앱의 **수익 창출** &gt; **광고로 수익 창출** 페이지로 이동한 후 [독립 실행형 광고 단위를 만듭니다](../publish/monetize-with-ads.md). 광고 단위 유형으로 **배너**를 지정합니다. 광고 단위 ID와 응용 프로그램 ID를 적어둡니다.

2.  코드에서 테스트 광고 단위 값(**applicationId** 및 **adUnitId**)을 개발자 센터에서 생성한 라이브 값으로 바꿉니다.

3.  개발자 센터 대시보드를 사용하여 스토어에 [앱을 제출](../publish/app-submissions.md)합니다.

4.  개발자 센터 대시보드에서 [광고 성과 보고서](../publish/advertising-performance-report.md)를 검토합니다.


 
