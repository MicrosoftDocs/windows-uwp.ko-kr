---
author: mcleanbyron
ms.assetid: c0450f7b-5c81-4d8c-92ef-2b1190d18af7
description: "AdControl 클래스를 사용하여 Windows Phone 8.1 또는 Windows Phone 8.0용 Silverlight 앱에서 배너 광고를 표시하는 방법을 알아봅니다."
title: "Windows Phone Silverlight의 AdControl"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 5a12badfb11cfd43c0833522d996da7df73b3d55


---

# Windows Phone Silverlight의 AdControl


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 연습에서는 [AdControl](https://msdn.microsoft.com/library/windows/apps/hh524191.aspx) 클래스를 사용하여 Windows Phone 8.1 또는 Windows Phone 8.0용 Silverlight 앱에서 배너 광고를 표시하는 방법을 보여 줍니다.

## 필수 조건

*  Visual Studio 2015 또는 Visual Studio 2013과 함께 [Microsoft 스토어 참여 및 수익 창출 SDK](http://aka.ms/store-em-sdk)를 설치합니다.


## 광고 어셈블리 참조 추가

Windows Phone Silverlight 프로젝트에 대한 Microsoft Advertising 어셈블리는 Microsoft 스토어 참여 및 수익 창출 SDK를 사용하여 로컬로 설치되지 않습니다. 코드 업데이트를 시작하려면 먼저 Microsoft 스토어 참여 및 수익 창출 SDK에서 광고 조정 지원과 **연결된 서비스**를 사용하여 이러한 어셈블리를 다운로드하고 프로젝트에서 참조합니다.

1.  Visual Studio에서 **프로젝트** 및 **연결된 서비스 추가**를 클릭합니다.

2.  **연결된 서비스 추가** 대화 상자에서 **광고 중재자**를 클릭한 다음 **구성**을 클릭합니다.

3.  **광고 네트워크 선택**을 클릭하고 **Microsoft Advertising**만 선택합니다.

    이제 필요한 모든 Silverlight용 Microsoft Advertising 어셈블리가 NuGet 패키지를 통해 로컬 프로젝트에 다운로드되며 이러한 어셈블리에 대한 참조가 프로젝트에 자동으로 추가됩니다. 광고 조정 어셈블리에 대한 참조도 프로젝트에 추가됩니다. 이 시나리오에는 필요 없기 때문에 이후 단계에서는 광고 조정 어셈블리 참조를 제거합니다.

4.  **광고 네트워크 선택** 대화 상자에서 **확인**을 클릭합니다. 다음 **가져오기 상태** 확인 페이지에서 **확인**을 다시 클릭하고 마지막으로 **추가**를 클릭하여 **광고 중재자** 대화 상자를 닫습니다.

5.  **솔루션 탐색기**에서 **참조** 노드를 확장합니다. 마우스 오른쪽 단추로 **Microsoft.AdMediator.WindowsPhone81SL.MicrosoftAdvertising**을 클릭하고 **제거**를 클릭합니다. 이 어셈블리 참조는 이 시나리오에 필요하지 않습니다.

## 앱 코딩


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

    ``` syntax
    xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
    ```

    페이지의 헤더에는 다음 코드가 포함됩니다.

    ``` syntax
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
        x:Class="PhoneApp1.MainPage"
    ```

6.  **Grid** 태그에 **AdControl**에 대한 다음 코드를 추가합니다. **ApplicationId** 및 **AdUnitId** 속성을 [테스트 모드 값](test-mode-values.md)에 제공된 테스트 값에 할당하고 **Height** 및 **Width** 속성을 [배너 광고에 대해 지원되는 광고 크기](supported-ad-sizes-for-banner-ads.md) 중 하나로 조정합니다.

    > **참고**  
    앱을 제출하기 전에 테스트 **ApplicationId** 및 **AdUnitId** 값을 라이브 값으로 바꿉니다.

    ``` syntax
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

## 개발자 센터를 사용하여 라이브 광고와 함께 앱 출시


1.  개발자 센터 대시보드에서 앱의 **수익 창출**&gt;**광고로 수익 창출** 페이지로 이동한 후 [독립 실행형 Microsoft 광고 단위를 만듭니다](../publish/monetize-with-ads.md). 광고 단위 유형으로 **배너**를 지정합니다. 광고 단위 ID와 응용 프로그램 ID를 적어둡니다.

2.  코드에서 테스트 광고 단위 값(**applicationId** 및 **adUnitId**)을 개발자 센터에서 생성한 라이브 값으로 바꿉니다.

3.  개발자 센터 대시보드를 사용하여 스토어에 [앱을 제출](../publish/app-submissions.md)합니다.

4.  개발자 센터 대시보드에서 [광고 성과 보고서](../publish/advertising-performance-report.md)를 검토합니다.


 



<!--HONumber=Jun16_HO4-->


