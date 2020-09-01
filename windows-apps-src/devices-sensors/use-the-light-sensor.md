---
ms.assetid: 15BAB25C-DA8C-4F13-9B8F-EA9E4270BCE9
title: 광원 센서 사용
description: 주변 광원 센서를 사용 하 여 조명 변화를 검색 하는 방법을 알아봅니다.
ms.date: 06/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ae31e1dc26a5836285a438d3bff9c742f94b7177
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159497"
---
# <a name="use-the-light-sensor"></a>광원 센서 사용


**중요 API**

-   [**Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors)
-   [**LightSensor**](/uwp/api/Windows.Devices.Sensors.LightSensor)

**샘플**

-   보다 완전 한 구현은 [광원 센서 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LightSensor)을 참조 하세요.

주변 광원 센서를 사용 하 여 조명 변화를 검색 하는 방법을 알아봅니다.

주변 광원 센서는 앱이 사용자 환경의 변화에 응답하는 것을 허용하는 몇 가지 환경 센서 유형 중 하나입니다.

## <a name="prerequisites"></a>필수 구성 요소

Extensible Application Markup Language (XAML), Microsoft Visual c # 및 이벤트에 대해 잘 알고 있어야 합니다.

사용 중인 장치 또는 에뮬레이터에서 주변 광원 센서를 지원 해야 합니다.

## <a name="create-a-simple-light-sensor-app"></a>간단한 경량 센서 앱 만들기

이 섹션은 두 개의 하위 섹션으로 구성 되어 있습니다. 첫 번째 하위 섹션에서는 간단한 광원 센서 응용 프로그램을 처음부터 만드는 데 필요한 단계를 안내 합니다. 다음 하위 섹션에서는 방금 만든 앱에 대해 설명 합니다.

###  <a name="instructions"></a>Instructions

-   **Visual c #** 프로젝트 템플릿에서 **빈 앱 (유니버설 Windows)** 을 선택 하 여 새 프로젝트를 만듭니다.

-   프로젝트의 BlankPage.xaml.cs 파일을 열고 기존 코드를 다음 코드로 바꿉니다.

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
    using Windows.Devices.Sensors; // Required to access the sensor platform and the ALS

    // The Blank Page item template is documented at https://go.microsoft.com/fwlink/p/?linkid=234238

    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class BlankPage : Page
        {
            private LightSensor _lightsensor; // Our app' s lightsensor object

            // This event handler writes the current light-sensor reading to
            // the textbox named "txtLUX" on the app' s main page.

            private void ReadingChanged(object sender, LightSensorReadingChangedEventArgs e)
            {
                Dispatcher.RunAsync(CoreDispatcherPriority.Normal, (s, a) =>
                {
                    LightSensorReading reading = (a.Context as LightSensorReadingChangedEventArgs).Reading;
                    txtLuxValue.Text = String.Format("{0,5:0.00}", reading.IlluminanceInLux);
                });
            }

            public BlankPage()
            {
                InitializeComponent();
                _lightsensor = LightSensor.GetDefault(); // Get the default light sensor object

                // Assign an event handler for the ALS reading-changed event
                if (_lightsensor != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _lightsensor.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _lightsensor.ReportInterval = reportInterval;

                    // Establish the even thandler
                    _lightsensor.ReadingChanged += new TypedEventHandler<LightSensor, LightSensorReadingChangedEventArgs>(ReadingChanged);
                }

            }

        }
    }
```

이전 코드 조각에서 네임 스페이스의 이름을 프로젝트에 지정한 이름으로 바꿔야 합니다. 예를 들어 **LightingCS**이라는 프로젝트를 만든 경우를 `namespace App1` 로 바꿉니다 `namespace LightingCS` .

-   MainPage .xaml 파일을 열고 원래 내용을 다음 XML로 바꿉니다.

```xml
    <Page
        x:Class="App1.BlankPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:App1"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid x:Name="LayoutRoot" Background="Black">
            <TextBlock HorizontalAlignment="Left" Height="44" Margin="52,38,0,0" TextWrapping="Wrap" Text="LUX Reading" VerticalAlignment="Top" Width="150"/>
            <TextBlock x:Name="txtLuxValue" HorizontalAlignment="Left" Height="44" Margin="224,38,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="217"/>


        </Grid>

    </Page>
```

이전 코드 조각의 클래스 이름 중 첫 번째 부분을 응용 프로그램의 네임 스페이스로 바꾸어야 합니다. 예를 들어 **LightingCS**이라는 프로젝트를 만든 경우를 `x:Class="App1.MainPage"` 로 바꿉니다 `x:Class="LightingCS.MainPage"` . 또한를로 바꾸어야 `xmlns:local="using:App1"` 합니다 `xmlns:local="using:LightingCS"` .

-   F5 키를 누르거나 **디버그**  >  **디버깅 시작** 을 선택 하 여 앱을 빌드, 배포 및 실행 합니다.

앱이 실행 되 면 센서에 사용할 수 있는 조명을 변경 하거나 에뮬레이터 도구를 사용 하 여 광원 센서 값을 변경할 수 있습니다.

-   Visual Studio로 돌아가서 Shift + F5 키를 누르거나 **디버그**  >  **디버깅 중지** 를 선택 하 여 앱을 중지 함으로써 앱을 중지 합니다.

###  <a name="explanation"></a>설명

이전 예제에서는 앱에서 가벼운 센서 입력을 통합 하기 위해 작성 해야 하는 코드의 양을 보여 줍니다.

앱은 **BlankPage** 메서드의 기본 센서와의 연결을 설정 합니다.

```csharp
_lightsensor = LightSensor.GetDefault(); // Get the default light sensor object
```

앱은 **BlankPage** 메서드 내에서 보고서 간격을 설정 합니다. 이 코드는 장치에서 지 원하는 최소 간격을 검색 하 고 요청 된 간격을 16 밀리초 (60-Hz 새로 고침 빈도 근사치)와 비교 합니다. 지원 되는 최소 간격이 요청 된 간격 보다 클 경우 코드에서 값을 최소값으로 설정 합니다. 그렇지 않으면 값을 요청 된 간격으로 설정 합니다.

```csharp
uint minReportInterval = _lightsensor.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_lightsensor.ReportInterval = reportInterval;
```
새 가벼운 센서 데이터가 **readingchanged 이벤트가 발생할** 메서드에서 캡처됩니다. 센서 드라이버는 센서에서 새 데이터를 받을 때마다 이 이벤트 처리기를 사용하여 이 값을 앱에 전달합니다. 앱은 다음 줄에이 이벤트 처리기를 등록 합니다.

```csharp
_lightsensor.ReadingChanged += new TypedEventHandler<LightSensor,
LightSensorReadingChangedEventArgs>(ReadingChanged);
```

이러한 새 값은 프로젝트의 XAML에서 찾은 TextBlock에 기록 됩니다.

```xml
<TextBlock HorizontalAlignment="Left" Height="44" Margin="52,38,0,0" TextWrapping="Wrap" Text="LUX Reading" VerticalAlignment="Top" Width="150"/>
 <TextBlock x:Name="txtLuxValue" HorizontalAlignment="Left" Height="44" Margin="224,38,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="217"/>
```