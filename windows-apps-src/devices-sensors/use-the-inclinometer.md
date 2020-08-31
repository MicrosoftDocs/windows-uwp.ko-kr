---
ms.assetid: 16AD53CA-1252-456C-8567-2263D3EC95F3
title: 경사계 사용
description: 경사 계 입력 장치를 사용 하 여 피치, 롤 및 요를 확인 하는 기본 앱을 만드는 방법에 대해 알아봅니다.
ms.date: 06/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ebfbdda8f5f7bf308ee427ab79d8dd45969e3108
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054333"
---
# <a name="use-the-inclinometer"></a>경사계 사용


**중요 API**

-   [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors)
-   [**Inclinometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Inclinometer)

**샘플**

-   보다 완전 한 구현은 [경사 계 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Inclinometer)을 참조 하세요.

경사 계를 사용 하 여 피치, 롤 및 요를 확인 하는 방법을 알아봅니다.

일부 3 차원 게임에는 입력 장치로 경사 계 필요 합니다. 일반적인 예 중 하나는 경사 계의 세 축 (X, Y, Z)을 항공기의 엘리베이터, 보조 eron 및 방향 조정기 입력에 매핑하는 비행 시뮬레이터입니다.

 ## <a name="prerequisites"></a>필수 구성 요소

Extensible Application Markup Language (XAML), Microsoft Visual c # 및 이벤트에 대해 잘 알고 있어야 합니다.

사용 중인 장치 또는 에뮬레이터에서 경사 계을 지원 해야 합니다.

 ## <a name="create-a-simple-inclinometer-app"></a>간단한 경사 계 앱 만들기

이 섹션은 두 개의 하위 섹션으로 구성 되어 있습니다. 첫 번째 하위 섹션에서는 간단한 경사 계 응용 프로그램을 처음부터 만드는 데 필요한 단계를 안내 합니다. 다음 하위 섹션에서는 방금 만든 앱에 대해 설명 합니다.

###  <a name="instructions"></a>Instructions

-   **Visual c #** 프로젝트 템플릿에서 **빈 앱 (유니버설 Windows)** 을 선택 하 여 새 프로젝트를 만듭니다.

-   프로젝트의 MainPage.xaml.cs 파일을 열고 기존 코드를 다음 코드로 바꿉니다.

```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Windows.Foundation;
    using Windows.Foundation.Collections;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Controls.Primitives;
    using Windows.UI.Xaml.Data;
    using Windows.UI.Xaml.Input;
    using Windows.UI.Xaml.Media;
    using Windows.UI.Xaml.Navigation;

    using Windows.UI.Core;
    using Windows.Devices.Sensors;


    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private Inclinometer _inclinometer;

            // This event handler writes the current inclinometer reading to
            // the three text blocks on the app' s main page.

            private async void ReadingChanged(object sender, InclinometerReadingChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    InclinometerReading reading = e.Reading;
                    txtPitch.Text = String.Format("{0,5:0.00}", reading.PitchDegrees);
                    txtRoll.Text = String.Format("{0,5:0.00}", reading.RollDegrees);
                    txtYaw.Text = String.Format("{0,5:0.00}", reading.YawDegrees);
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _inclinometer = Inclinometer.GetDefault();


                if (_inclinometer != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _inclinometer.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _inclinometer.ReportInterval = reportInterval;

                    // Establish the event handler
                    _inclinometer.ReadingChanged += new TypedEventHandler<Inclinometer, InclinometerReadingChangedEventArgs>(ReadingChanged);
                }
            }
        }
    }
```

이전 코드 조각에서 네임 스페이스의 이름을 프로젝트에 지정한 이름으로 바꿔야 합니다. 예를 들어 **InclinometerCS**이라는 프로젝트를 만든 경우를 `namespace App1` 로 바꿉니다 `namespace InclinometerCS` .

-   MainPage .xaml 파일을 열고 원래 내용을 다음 XML로 바꿉니다.

```xml
        <Page
        x:Class="App1.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:App1"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid x:Name="LayoutRoot" Background="#FF0C0C0C">
            <TextBlock HorizontalAlignment="Left" Height="21" Margin="0,8,0,0" TextWrapping="Wrap" Text="Pitch: " VerticalAlignment="Top" Width="45" Foreground="#FFF9F4F4"/>
            <TextBlock x:Name="txtPitch" HorizontalAlignment="Left" Height="21" Margin="59,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="71" Foreground="#FFFDF9F9"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="0,29,0,0" TextWrapping="Wrap" Text="Roll:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F1F1"/>
            <TextBlock x:Name="txtRoll" HorizontalAlignment="Left" Height="23" Margin="59,29,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="50" Foreground="#FFFCF9F9"/>
            <TextBlock HorizontalAlignment="Left" Height="19" Margin="0,56,0,0" TextWrapping="Wrap" Text="Yaw:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F3F3"/>
            <TextBlock x:Name="txtYaw" HorizontalAlignment="Left" Height="19" Margin="55,56,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="54" Foreground="#FFF6F2F2"/>

        </Grid>
    </Page>
```

이전 코드 조각의 클래스 이름 중 첫 번째 부분을 응용 프로그램의 네임 스페이스로 바꾸어야 합니다. 예를 들어 **InclinometerCS**이라는 프로젝트를 만든 경우를 `x:Class="App1.MainPage"` 로 바꿉니다 `x:Class="InclinometerCS.MainPage"` . 또한를로 바꾸어야 `xmlns:local="using:App1"` 합니다 `xmlns:local="using:InclinometerCS"` .

-   F5 키를 누르거나 **디버그**  >  **디버깅 시작** 을 선택 하 여 앱을 빌드, 배포 및 실행 합니다.

앱이 실행 되 고 나면 장치를 이동 하거나 에뮬레이터 도구를 사용 하 여 경사 계 값을 변경할 수 있습니다.

-   Visual Studio로 돌아가서 Shift + F5 키를 누르거나 **디버그**  >  **디버깅 중지** 를 선택 하 여 앱을 중지 함으로써 앱을 중지 합니다.

###  <a name="explanation"></a>설명

이전 예제에서는 경사 계 입력을 앱에 통합 하기 위해 작성 해야 하는 코드의 양을 보여 줍니다.

앱은 **Mainpage** 메서드의 기본 경사 계 연결을 설정 합니다.

```csharp
_inclinometer = Inclinometer.GetDefault();
```

앱은 **Mainpage** 메서드 내에서 보고서 간격을 설정 합니다. 이 코드는 장치에서 지 원하는 최소 간격을 검색 하 고 요청 된 간격을 16 밀리초 (60-Hz 새로 고침 빈도 근사치)와 비교 합니다. 지원 되는 최소 간격이 요청 된 간격 보다 클 경우 코드에서 값을 최소값으로 설정 합니다. 그렇지 않으면 값을 요청 된 간격으로 설정 합니다.

```csharp
uint minReportInterval = _inclinometer.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_inclinometer.ReportInterval = reportInterval;
```

새 경사 계 데이터는 **readingchanged 이벤트가 발생할** 메서드에서 캡처됩니다. 센서 드라이버가 센서 로부터 새 데이터를 받을 때마다이 이벤트 처리기를 사용 하 여 앱에 값을 전달 합니다. 앱은 다음 줄에이 이벤트 처리기를 등록 합니다.

```csharp
_inclinometer.ReadingChanged += new TypedEventHandler<Inclinometer,
InclinometerReadingChangedEventArgs>(ReadingChanged);
```

이러한 새 값은 프로젝트의 XAML에 있는 Textblock에 기록 됩니다.

```xml
<TextBlock HorizontalAlignment="Left" Height="21" Margin="0,8,0,0" TextWrapping="Wrap" Text="Pitch: " VerticalAlignment="Top" Width="45" Foreground="#FFF9F4F4"/>
 <TextBlock x:Name="txtPitch" HorizontalAlignment="Left" Height="21" Margin="59,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="71" Foreground="#FFFDF9F9"/>
 <TextBlock HorizontalAlignment="Left" Height="23" Margin="0,29,0,0" TextWrapping="Wrap" Text="Roll:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F1F1"/>
 <TextBlock x:Name="txtRoll" HorizontalAlignment="Left" Height="23" Margin="59,29,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="50" Foreground="#FFFCF9F9"/>
 <TextBlock HorizontalAlignment="Left" Height="19" Margin="0,56,0,0" TextWrapping="Wrap" Text="Yaw:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F3F3"/>
 <TextBlock x:Name="txtYaw" HorizontalAlignment="Left" Height="19" Margin="55,56,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="54" Foreground="#FFF6F2F2"/>
```

