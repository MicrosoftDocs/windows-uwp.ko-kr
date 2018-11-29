---
ms.assetid: 454953E1-DD8F-44B7-A614-7BAD8C683536
title: 회전계 사용
description: 회전계를 사용하여 사용자의 동작 변화를 탐지하는 방법을 알아봅니다.
ms.date: 06/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8257a8a0c6b8ca320adb04bfb64b0308a4cbefd3
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8215642"
---
# <a name="use-the-gyrometer"></a>회전계 사용


**중요 API**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**회전계**](https://msdn.microsoft.com/library/windows/apps/BR225718)

**샘플**

-   더 완전한 구현은 [회전계 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/gyrometer)을 참조하세요.

회전계를 사용하여 사용자의 동작 변화를 탐지하는 방법을 알아봅니다.

회전계는 가속도계를 게임 컨트롤러로서 보완합니다. 가속도계는 선형 동작을 측정할 수고 회전계는 각속도 또는 회전 동작을 측정합니다.

## <a name="prerequisites"></a>사전 요구 사항

응용 프로그램 언어 XAML (Extensible Markup), Microsoft VisualC # 및 이벤트에 알고 있어야 합니다.

사용하는 장치 또는 에뮬레이터가 회전계를 지원해야 합니다.

## <a name="create-a-simple-gyrometer-app"></a>간단한 회전계 앱 만들기

이 섹션은 두 개의 하위 섹션으로 나뉩니다. 첫 번째 하위 섹션에서는 처음부터 간단한 회전계 응용 프로그램을 만드는 데 필요한 단계를 안내합니다. 다음 하위 섹션에서는 방금 만든 앱에 대해 설명합니다.

###  <a name="instructions"></a>지침

-   **Visual C#** 프로젝트 템플릿에서 **빈 앱(유니버설 Windows)** 를 선택하여 새 프로젝트를 만듭니다.

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

    using Windows.UI.Core; // Required to access the core dispatcher object
    using Windows.Devices.Sensors; // Required to access the sensor platform and the gyrometer


    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private Gyrometer _gyrometer; // Our app' s gyrometer object

            // This event handler writes the current gyrometer reading to
            // the three textblocks on the app' s main page.

            private async void ReadingChanged(object sender, GyrometerReadingChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    GyrometerReading reading = e.Reading;
                    txtXAxis.Text = String.Format("{0,5:0.00}", reading.AngularVelocityX);
                    txtYAxis.Text = String.Format("{0,5:0.00}", reading.AngularVelocityY);
                    txtZAxis.Text = String.Format("{0,5:0.00}", reading.AngularVelocityZ);
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _gyrometer = Gyrometer.GetDefault(); // Get the default gyrometer sensor object

                if (_gyrometer != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _gyrometer.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _gyrometer.ReportInterval = reportInterval;

                    // Assign an event handler for the gyrometer reading-changed event
                    _gyrometer.ReadingChanged += new TypedEventHandler<Gyrometer, GyrometerReadingChangedEventArgs>(ReadingChanged);
                }

            }
        }
    }
```

이전 코드 조각의 네임스페이스 이름을 프로젝트에 지정한 이름으로 바꾸어야 합니다. 예를 들어 **GyrometerCS**라는 프로젝트를 만든 경우 `namespace App1`을 `namespace GyrometerCS`로 바꿉니다.

-   MainPage.xaml 파일을 열고 원본 콘텐츠를 다음 XML로 바꿉니다.

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
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="8,8,0,0" TextWrapping="Wrap" Text="X-Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFDFDFD"/>
            <TextBlock x:Name="txtXAxis" HorizontalAlignment="Left" Height="23" Margin="67,8,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="88" Foreground="#FFFDFAFA"/>
            <TextBlock HorizontalAlignment="Left" Height="20" Margin="8,52,0,0" TextWrapping="Wrap" Text="Y Axis:" VerticalAlignment="Top" Width="46" Foreground="White"/>
            <TextBlock x:Name="txtYAxis" HorizontalAlignment="Left" Height="24" Margin="54,48,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="80" Foreground="#FFFBFBFB"/>
            <TextBlock HorizontalAlignment="Left" Height="21" Margin="8,93,0,0" TextWrapping="Wrap" Text="Z Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFEFBFB"/>
            <TextBlock x:Name="txtZAxis" HorizontalAlignment="Left" Height="21" Margin="54,93,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="63" Foreground="#FFF8F3F3"/>

        </Grid>
    </Page>
```

이전 코드 조각에서 클래스 이름의 첫 번째 부분을 앱의 네임스페이스로 바꾸어야 합니다. 예를 들어 **GyrometerCS**라는 프로젝트를 만든 경우 `x:Class="App1.MainPage"`을 `x:Class="GyrometerCS.MainPage"`로 바꿉니다. 또한 `xmlns:local="using:App1"`을 `xmlns:local="using:GyrometerCS"`로 바꾸어야 합니다.

-   F5 키를 누르거나 **디버그** > **디버깅 시작**을 선택하여 앱을 빌드, 배포 및 실행합니다.

앱이 실행 중이면 디바이스를 이동하거나 에뮬레이터 도구를 사용하여 회전계 값을 변경할 수 있습니다.

-   Visual Studio로 돌아가서 Shift+F5를 눌러 앱을 중지하거나 **디버그** > **디버깅 중지**를 선택하여 앱을 중지합니다.

###  <a name="explanation"></a>설명

앞의 예는 앱에서 회전계 입력을 통합하기 위해 작성해야 하는 코드의 양이 얼마나 작은지를 보여줍니다.

앱은 **MainPage** 메서드에서 기본 회전계와 연결합니다.

```csharp
_gyrometer = Gyrometer.GetDefault(); // Get the default gyrometer sensor object
```

앱은 **MainPage** 메서드 내에서 보고 간격을 설정합니다. 이 코드는 디바이스에서 지원되는 최소 간격을 검색하여 요청된 16밀리초 간격(약 60Hz 새로 고침 빈도)과 비교합니다. 지원되는 최소 간격이 요청된 간격보다 큰 경우 코드는 값을 최소값으로 설정합니다. 그렇지 않으면 값을 요청된 간격으로 설정합니다.

```csharp
uint minReportInterval = _gyrometer.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_gyrometer.ReportInterval = reportInterval;
```

새 회전계 데이터는 **ReadingChanged** 메서드에서 캡처됩니다. 센서 드라이버는 센서에서 새 데이터를 받을 때마다 이 이벤트 처리기를 사용하여 이 값을 앱에 전달합니다. 앱은 다음 줄에서 이 이벤트 처리기를 등록합니다.

```csharp
_gyrometer.ReadingChanged += new TypedEventHandler<Gyrometer,
GyrometerReadingChangedEventArgs>(ReadingChanged);
```

이 새 값은 프로젝트의 XAML에서 찾은 TextBlock에 쓰입니다.

```xml
        <TextBlock HorizontalAlignment="Left" Height="23" Margin="8,8,0,0" TextWrapping="Wrap" Text="X-Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFDFDFD"/>
        <TextBlock x:Name="txtXAxis" HorizontalAlignment="Left" Height="23" Margin="67,8,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="88" Foreground="#FFFDFAFA"/>
        <TextBlock HorizontalAlignment="Left" Height="20" Margin="8,52,0,0" TextWrapping="Wrap" Text="Y Axis:" VerticalAlignment="Top" Width="46" Foreground="White"/>
        <TextBlock x:Name="txtYAxis" HorizontalAlignment="Left" Height="24" Margin="54,48,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="80" Foreground="#FFFBFBFB"/>
        <TextBlock HorizontalAlignment="Left" Height="21" Margin="8,93,0,0" TextWrapping="Wrap" Text="Z Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFEFBFB"/>
        <TextBlock x:Name="txtZAxis" HorizontalAlignment="Left" Height="21" Margin="54,93,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="63" Foreground="#FFF8F3F3"/>
```

 ## <a name="related-topics"></a>관련 항목

* [회전계 샘플](http://go.microsoft.com/fwlink/p/?linkid=241379)
