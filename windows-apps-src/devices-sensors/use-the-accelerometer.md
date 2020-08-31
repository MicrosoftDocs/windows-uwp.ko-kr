---
ms.assetid: F90686F5-641A-42D9-BC44-EC6CA11B8A42
title: 가속도계 사용
description: 단일 센서 인가 속도계을 사용 하 여 사용자 이동에 응답 하는 기본 앱을 만드는 방법을 알아봅니다.
ms.date: 06/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e38d64750b410369a9ff9ebf871267b03e0ad07e
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054343"
---
# <a name="use-the-accelerometer"></a>가속도계 사용


**중요 API**

-   [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors)
-   [**가속도계**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer)

**샘플**

-   보다 완전 한 구현은 [가 속도계 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Accelerometer)을 참조 하세요.

가 속도계를 사용 하 여 사용자 이동에 응답 하는 방법을 알아봅니다.

간단한 게임 앱은 단일 센서 인가 속도계를 입력 장치로 사용 합니다. 이러한 앱은 일반적으로 입력에 하나 또는 두 개의 축만 사용 합니다. 그러나 다른 입력 소스로는 흔들기 이벤트를 사용할 수도 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

Extensible Application Markup Language (XAML), Microsoft Visual c # 및 이벤트에 대해 잘 알고 있어야 합니다.

사용 중인 장치 또는 에뮬레이터에서가 속도계을 지원 해야 합니다.

## <a name="create-a-simple-accelerometer-app"></a>간단한가 속도계 앱 만들기

이 섹션은 두 개의 하위 섹션으로 구성 되어 있습니다. 첫 번째 하위 섹션에서는 간단한가 속도계 응용 프로그램을 처음부터 만드는 데 필요한 단계를 안내 합니다. 다음 하위 섹션에서는 방금 만든 앱에 대해 설명 합니다.

### <a name="instructions"></a>Instructions

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

    // Required to support the core dispatcher and the accelerometer

    using Windows.UI.Core;
    using Windows.Devices.Sensors;

    namespace App1
    {

        public sealed partial class MainPage : Page
        {
            // Sensor and dispatcher variables
            private Accelerometer _accelerometer;

            // This event handler writes the current accelerometer reading to
            // the three acceleration text blocks on the app' s main page.

            private async void ReadingChanged(object sender, AccelerometerReadingChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    AccelerometerReading reading = e.Reading;
                    txtXAxis.Text = String.Format("{0,5:0.00}", reading.AccelerationX);
                    txtYAxis.Text = String.Format("{0,5:0.00}", reading.AccelerationY);
                    txtZAxis.Text = String.Format("{0,5:0.00}", reading.AccelerationZ);

                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _accelerometer = Accelerometer.GetDefault();

                if (_accelerometer != null)
                {
                    // Establish the report interval
                    uint minReportInterval = _accelerometer.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _accelerometer.ReportInterval = reportInterval;

                    // Assign an event handler for the reading-changed event
                    _accelerometer.ReadingChanged += new TypedEventHandler<Accelerometer, AccelerometerReadingChangedEventArgs>(ReadingChanged);
                }
            }
        }
    }
```

이전 코드 조각에서 네임 스페이스의 이름을 프로젝트에 지정한 이름으로 바꿔야 합니다. 예를 들어 **AccelerometerCS**이라는 프로젝트를 만든 경우를 `namespace App1` 로 바꿉니다 `namespace AccelerometerCS` .

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
            <TextBlock HorizontalAlignment="Left" Height="25" Margin="8,20,0,0" TextWrapping="Wrap" Text="X-axis:" VerticalAlignment="Top" Width="62" Foreground="#FFEDE6E6"/>
            <TextBlock HorizontalAlignment="Left" Height="27" Margin="8,49,0,0" TextWrapping="Wrap" Text="Y-axis:" VerticalAlignment="Top" Width="62" Foreground="#FFF5F2F2"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="8,80,0,0" TextWrapping="Wrap" Text="Z-axis:" VerticalAlignment="Top" Width="62" Foreground="#FFF6F0F0"/>
            <TextBlock x:Name="txtXAxis" HorizontalAlignment="Left" Height="15" Margin="70,16,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="61" Foreground="#FFF2F2F2"/>
            <TextBlock x:Name="txtYAxis" HorizontalAlignment="Left" Height="15" Margin="70,49,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53" Foreground="#FFF2EEEE"/>
            <TextBlock x:Name="txtZAxis" HorizontalAlignment="Left" Height="15" Margin="70,80,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53" Foreground="#FFFFF8F8"/>

        </Grid>
    </Page>
```

이전 코드 조각의 클래스 이름 중 첫 번째 부분을 응용 프로그램의 네임 스페이스로 바꾸어야 합니다. 예를 들어 **AccelerometerCS**이라는 프로젝트를 만든 경우를 `x:Class="App1.MainPage"` 로 바꿉니다 `x:Class="AccelerometerCS.MainPage"` . 또한를로 바꾸어야 `xmlns:local="using:App1"` 합니다 `xmlns:local="using:AccelerometerCS"` .

-   F5 키를 누르거나 **디버그** &gt; **디버깅 시작** 을 선택 하 여 앱을 빌드, 배포 및 실행 합니다.

앱이 실행 되 고 나면 장치를 이동 하거나 에뮬레이터 도구를 사용 하 여가 속도계 값을 변경할 수 있습니다.

-   Visual Studio로 돌아가서 Shift + F5 키를 누르거나 **디버그** &gt; **디버깅 중지** 를 선택 하 여 앱을 중지 함으로써 앱을 중지 합니다.

### <a name="explanation"></a>설명

이전 예제에서는가 속도계 입력을 앱에 통합 하기 위해 작성 해야 하는 코드의 양을 보여 줍니다.

앱은 **Mainpage** 메서드의 기본가 속도계 연결을 설정 합니다.

```csharp
_accelerometer = Accelerometer.GetDefault();
```

앱은 **Mainpage** 메서드 내에서 보고서 간격을 설정 합니다. 이 코드는 장치에서 지 원하는 최소 간격을 검색 하 고 요청 된 간격을 16 밀리초 (60-Hz 새로 고침 빈도 근사치)와 비교 합니다. 지원 되는 최소 간격이 요청 된 간격 보다 클 경우 코드에서 값을 최소값으로 설정 합니다. 그렇지 않으면 값을 요청 된 간격으로 설정 합니다.

```csharp
uint minReportInterval = _accelerometer.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_accelerometer.ReportInterval = reportInterval;
```

새가 속도계 데이터는 **readingchanged 이벤트가 발생할** 메서드에서 캡처됩니다. 센서 드라이버가 센서 로부터 새 데이터를 받을 때마다이 이벤트 처리기를 사용 하 여 앱에 값을 전달 합니다. 앱은 다음 줄에이 이벤트 처리기를 등록 합니다.

```csharp
_accelerometer.ReadingChanged += new TypedEventHandler<Accelerometer,
AccelerometerReadingChangedEventArgs>(ReadingChanged);
```

이러한 새 값은 프로젝트의 XAML에 있는 Textblock에 기록 됩니다.

```xml
<TextBlock x:Name="txtXAxis" HorizontalAlignment="Left" Height="15" Margin="70,16,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="61" Foreground="#FFF2F2F2"/>
 <TextBlock x:Name="txtYAxis" HorizontalAlignment="Left" Height="15" Margin="70,49,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53" Foreground="#FFF2EEEE"/>
 <TextBlock x:Name="txtZAxis" HorizontalAlignment="Left" Height="15" Margin="70,80,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53" Foreground="#FFFFF8F8"/>
```
