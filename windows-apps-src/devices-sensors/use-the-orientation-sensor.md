---
ms.assetid: 1889AC3A-A472-4294-89B8-A642668A8A6E
title: 방향 센서 사용
description: 방향 센서를 사용 하 여 장치 방향을 결정 하는 방법에 대해 알아봅니다.
ms.date: 06/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0217567fd2b78542b745a02fbdfa3bd816d9a2b6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159457"
---
# <a name="use-the-orientation-sensor"></a>방향 센서 사용


**중요 API**

-   [**Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors)
-   [**OrientationSensor**](/uwp/api/Windows.Devices.Sensors.OrientationSensor)
-   [**SimpleOrientation**](/uwp/api/Windows.Devices.Sensors.SimpleOrientation)

**샘플**

-   [방향 센서 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/OrientationSensor)
-   [간단한 방향 센서 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleOrientationSensor)

방향 센서를 사용 하 여 장치 방향을 결정 하는 방법에 대해 알아봅니다.

OrientationSensor 네임 스페이스에 포함 된 두 가지 유형의 방향 센서 Api ( [**OrientationSensor**](/uwp/api/Windows.Devices.Sensors.OrientationSensor) 및 [**SimpleOrientation**](/uwp/api/Windows.Devices.Sensors.SimpleOrientation))가 [**있습니다.**](/uwp/api/Windows.Devices.Sensors) 이러한 센서는 모두 방향 센서 이지만 해당 용어는 오버 로드 되며 매우 다른 용도로 사용 됩니다. 그러나 두 가지 모두 방향 센서가 기 때문에이 문서에 설명 되어 있습니다.

[**OrientationSensor**](/uwp/api/Windows.Devices.Sensors.OrientationSensor) API는 3 차원 앱에 사용 되며 4 원수와 회전 행렬을 가져옵니다. 4 원수는 \[ 임의의 축에 대 한 점 x, y, z의 회전으로 가장 쉽게 이해 될 수 있습니다 \] (3 개의 축을 중심으로 회전을 나타내는 회전 매트릭스와 대조). 복소수는 복소수의 기하학 속성과 복소수의 수학적 속성을 포함 하지만, 이러한 작업을 수행 하는 것은 간단 하며 DirectX와 같은 프레임 워크는 이러한 작업을 지원 하기 때문에 매우 exotic. 복잡 한 3 차원 앱은 방향 센서를 사용 하 여 사용자의 큐브 뷰를 조정할 수 있습니다. 이 센서는가 속도계, 회전 계 및 나침반의 입력을 결합 합니다.

[**SimpleOrientation**](/uwp/api/Windows.Devices.Sensors.SimpleOrientation) API는 세로 위쪽, 세로 아래쪽, 가로 왼쪽 및 가로 오른쪽과 같은 정의 측면에서 현재 장치 방향을 결정 하는 데 사용 됩니다. 또한 장치가 얼굴을 차지 하 고 있는지 여부를 감지할 수 있습니다. "세로 위쪽" 또는 "가로 왼쪽"과 같은 속성을 반환 하는 대신이 센서는 "회전 안 함", "Rotated90DegreesCounterclockwise" 등의 회전 값을 반환 합니다. 다음 표에서는 일반적인 방향 속성을 해당 센서 읽기에 매핑합니다.

| 방향     | 해당 센서 판독값      |
|-----------------|-----------------------------------|
| 세로 위쪽     | NotRotated                        |
| 왼쪽 가로  | Rotated90DegreesCounterclockwise  |
| 세로 아래로   | Rotated180DegreesCounterclockwise |
| 가로 오른쪽 | Rotated270DegreesCounterclockwise |

## <a name="prerequisites"></a>필수 구성 요소

Extensible Application Markup Language (XAML), Microsoft Visual c # 및 이벤트에 대해 잘 알고 있어야 합니다.

사용 중인 장치 또는 에뮬레이터에서 방향 센서를 지원 해야 합니다.

## <a name="create-an-orientationsensor-app"></a>OrientationSensor 앱 만들기

이 섹션은 두 개의 하위 섹션으로 구성 되어 있습니다. 첫 번째 하위 섹션에서는 방향 응용 프로그램을 처음부터 만드는 데 필요한 단계를 안내 합니다. 다음 하위 섹션에서는 방금 만든 앱에 대해 설명 합니다.

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

    // The Blank Page item template is documented at https://go.microsoft.com/fwlink/p/?linkid=234238

    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private OrientationSensor _sensor;

            private async void ReadingChanged(object sender, OrientationSensorReadingChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    OrientationSensorReading reading = e.Reading;

                    // Quaternion values
                    txtQuaternionX.Text = String.Format("{0,8:0.00000}", reading.Quaternion.X);
                    txtQuaternionY.Text = String.Format("{0,8:0.00000}", reading.Quaternion.Y);
                    txtQuaternionZ.Text = String.Format("{0,8:0.00000}", reading.Quaternion.Z);
                    txtQuaternionW.Text = String.Format("{0,8:0.00000}", reading.Quaternion.W);

                    // Rotation Matrix values
                    txtM11.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M11);
                    txtM12.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M12);
                    txtM13.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M13);
                    txtM21.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M21);
                    txtM22.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M22);
                    txtM23.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M23);
                    txtM31.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M31);
                    txtM32.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M32);
                    txtM33.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M33);
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _sensor = OrientationSensor.GetDefault();

                // Establish the report interval for all scenarios
                uint minReportInterval = _sensor.MinimumReportInterval;
                uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                _sensor.ReportInterval = reportInterval;

                // Establish event handler
                _sensor.ReadingChanged += new TypedEventHandler<OrientationSensor, OrientationSensorReadingChangedEventArgs>(ReadingChanged);
            }
        }
    }
```

이전 코드 조각에서 네임 스페이스의 이름을 프로젝트에 지정한 이름으로 바꿔야 합니다. 예를 들어 **OrientationSensorCS**이라는 프로젝트를 만든 경우를 `namespace App1` 로 바꿉니다 `namespace OrientationSensorCS` .

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

        <Grid x:Name="LayoutRoot" Background="Black">
            <TextBlock HorizontalAlignment="Left" Height="28" Margin="4,4,0,0" TextWrapping="Wrap" Text="M11:" VerticalAlignment="Top" Width="46"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="4,36,0,0" TextWrapping="Wrap" Text="M12:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="24" Margin="4,72,0,0" TextWrapping="Wrap" Text="M13:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="31" Margin="4,118,0,0" TextWrapping="Wrap" Text="M21:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="24" Margin="4,160,0,0" TextWrapping="Wrap" Text="M22:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="24" Margin="8,201,0,0" TextWrapping="Wrap" Text="M23:" VerticalAlignment="Top" Width="35"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="4,234,0,0" TextWrapping="Wrap" Text="M31:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="28" Margin="4,274,0,0" TextWrapping="Wrap" Text="M32:" VerticalAlignment="Top" Width="46"/>
            <TextBlock HorizontalAlignment="Left" Height="21" Margin="4,322,0,0" TextWrapping="Wrap" Text="M33:" VerticalAlignment="Top" Width="39"/>
            <TextBlock x:Name="txtM11" HorizontalAlignment="Left" Height="19" Margin="43,4,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM12" HorizontalAlignment="Left" Height="23" Margin="43,36,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM13" HorizontalAlignment="Left" Height="15" Margin="43,72,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM21" HorizontalAlignment="Left" Height="20" Margin="43,114,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM22" HorizontalAlignment="Left" Height="19" Margin="43,156,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM23" HorizontalAlignment="Left" Height="16" Margin="43,197,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM31" HorizontalAlignment="Left" Height="17" Margin="43,230,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM32" HorizontalAlignment="Left" Height="19" Margin="43,270,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM33" HorizontalAlignment="Left" Height="21" Margin="43,322,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock HorizontalAlignment="Left" Height="15" Margin="194,8,0,0" TextWrapping="Wrap" Text="Quaternion X:" VerticalAlignment="Top" Width="81"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="194,36,0,0" TextWrapping="Wrap" Text="Quaternion Y:" VerticalAlignment="Top" Width="81"/>
            <TextBlock HorizontalAlignment="Left" Height="15" Margin="194,72,0,0" TextWrapping="Wrap" Text="Quaternion Z:" VerticalAlignment="Top" Width="81"/>
            <TextBlock x:Name="txtQuaternionX" HorizontalAlignment="Left" Height="15" Margin="279,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="104"/>
            <TextBlock x:Name="txtQuaternionY" HorizontalAlignment="Left" Height="12" Margin="275,36,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="108"/>
            <TextBlock x:Name="txtQuaternionZ" HorizontalAlignment="Left" Height="19" Margin="275,68,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="89"/>
            <TextBlock HorizontalAlignment="Left" Height="21" Margin="194,96,0,0" TextWrapping="Wrap" Text="Quaternion W:" VerticalAlignment="Top" Width="81"/>
            <TextBlock x:Name="txtQuaternionW" HorizontalAlignment="Left" Height="12" Margin="279,96,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="72"/>

        </Grid>
    </Page>
```

이전 코드 조각의 클래스 이름 중 첫 번째 부분을 응용 프로그램의 네임 스페이스로 바꾸어야 합니다. 예를 들어 **OrientationSensorCS**이라는 프로젝트를 만든 경우를 `x:Class="App1.MainPage"` 로 바꿉니다 `x:Class="OrientationSensorCS.MainPage"` . 또한를로 바꾸어야 `xmlns:local="using:App1"` 합니다 `xmlns:local="using:OrientationSensorCS"` .

-   F5 키를 누르거나 **디버그**  >  **디버깅 시작** 을 선택 하 여 앱을 빌드, 배포 및 실행 합니다.

앱이 실행 되 면 장치를 이동 하거나 에뮬레이터 도구를 사용 하 여 방향을 변경할 수 있습니다.

-   Visual Studio로 돌아가서 Shift + F5 키를 누르거나 **디버그**  >  **디버깅 중지** 를 선택 하 여 앱을 중지 함으로써 앱을 중지 합니다.

###  <a name="explanation"></a>설명

이전 예제에서는 앱에서 방향 센서 입력을 통합 하기 위해 작성 해야 하는 코드의 양을 보여 줍니다.

앱은 **Mainpage** 메서드에서 기본 방향 센서와의 연결을 설정 합니다.

```csharp
_sensor = OrientationSensor.GetDefault();
```

앱은 **Mainpage** 메서드 내에서 보고서 간격을 설정 합니다. 이 코드는 장치에서 지 원하는 최소 간격을 검색 하 고 요청 된 간격을 16 밀리초 (60-Hz 새로 고침 빈도 근사치)와 비교 합니다. 지원 되는 최소 간격이 요청 된 간격 보다 클 경우 코드에서 값을 최소값으로 설정 합니다. 그렇지 않으면 값을 요청 된 간격으로 설정 합니다.

```csharp
uint minReportInterval = _sensor.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_sensor.ReportInterval = reportInterval;
```

새 센서 데이터가 **readingchanged 이벤트가 발생할** 메서드에서 캡처됩니다. 센서 드라이버가 센서 로부터 새 데이터를 받을 때마다이 이벤트 처리기를 사용 하 여 앱에 값을 전달 합니다. 앱은 다음 줄에이 이벤트 처리기를 등록 합니다.

```csharp
_sensor.ReadingChanged += new TypedEventHandler<OrientationSensor,
OrientationSensorReadingChangedEventArgs>(ReadingChanged);
```

이러한 새 값은 프로젝트의 XAML에 있는 Textblock에 기록 됩니다.

## <a name="create-a-simpleorientation-app"></a>SimpleOrientation 앱 만들기

이 섹션은 두 개의 하위 섹션으로 구성 되어 있습니다. 첫 번째 하위 섹션에서는 간단한 방향 응용 프로그램을 처음부터 만드는 데 필요한 단계를 안내 합니다. 다음 하위 섹션에서는 방금 만든 앱에 대해 설명 합니다.

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

    using Windows.UI.Core;
    using Windows.Devices.Sensors;
    // The Blank Page item template is documented at https://go.microsoft.com/fwlink/p/?linkid=234238

    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            // Sensor and dispatcher variables
            private SimpleOrientationSensor _simpleorientation;

            // This event handler writes the current sensor reading to
            // a text block on the app' s main page.

            private async void OrientationChanged(object sender, SimpleOrientationSensorOrientationChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    SimpleOrientation orientation = e.Orientation;
                    switch (orientation)
                    {
                        case SimpleOrientation.NotRotated:
                            txtOrientation.Text = "Not Rotated";
                            break;
                        case SimpleOrientation.Rotated90DegreesCounterclockwise:
                            txtOrientation.Text = "Rotated 90 Degrees Counterclockwise";
                            break;
                        case SimpleOrientation.Rotated180DegreesCounterclockwise:
                            txtOrientation.Text = "Rotated 180 Degrees Counterclockwise";
                            break;
                        case SimpleOrientation.Rotated270DegreesCounterclockwise:
                            txtOrientation.Text = "Rotated 270 Degrees Counterclockwise";
                            break;
                        case SimpleOrientation.Faceup:
                            txtOrientation.Text = "Faceup";
                            break;
                        case SimpleOrientation.Facedown:
                            txtOrientation.Text = "Facedown";
                            break;
                        default:
                            txtOrientation.Text = "Unknown orientation";
                            break;
                    }
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _simpleorientation = SimpleOrientationSensor.GetDefault();

                // Assign an event handler for the sensor orientation-changed event
                if (_simpleorientation != null)
                {
                    _simpleorientation.OrientationChanged += new TypedEventHandler<SimpleOrientationSensor, SimpleOrientationSensorOrientationChangedEventArgs>(OrientationChanged);
                }
            }
        }
    }
```

이전 코드 조각에서 네임 스페이스의 이름을 프로젝트에 지정한 이름으로 바꿔야 합니다. 예를 들어 **SimpleOrientationCS**이라는 프로젝트를 만든 경우를 `namespace App1` 로 바꿉니다 `namespace SimpleOrientationCS` .

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
            <TextBlock HorizontalAlignment="Left" Height="24" Margin="8,8,0,0" TextWrapping="Wrap" Text="Current Orientation:" VerticalAlignment="Top" Width="101" Foreground="#FFF8F7F7"/>
            <TextBlock x:Name="txtOrientation" HorizontalAlignment="Left" Height="24" Margin="118,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="175" Foreground="#FFFEFAFA"/>

        </Grid>
    </Page>
```

이전 코드 조각의 클래스 이름 중 첫 번째 부분을 응용 프로그램의 네임 스페이스로 바꾸어야 합니다. 예를 들어 **SimpleOrientationCS**이라는 프로젝트를 만든 경우를 `x:Class="App1.MainPage"` 로 바꿉니다 `x:Class="SimpleOrientationCS.MainPage"` . 또한를로 바꾸어야 `xmlns:local="using:App1"` 합니다 `xmlns:local="using:SimpleOrientationCS"` .

-   F5 키를 누르거나 **디버그**  >  **디버깅 시작** 을 선택 하 여 앱을 빌드, 배포 및 실행 합니다.

앱이 실행 되 면 장치를 이동 하거나 에뮬레이터 도구를 사용 하 여 방향을 변경할 수 있습니다.

-   Visual Studio로 돌아가서 Shift + F5 키를 누르거나 **디버그**  >  **디버깅 중지** 를 선택 하 여 앱을 중지 함으로써 앱을 중지 합니다.

### <a name="explanation"></a>설명

이전 예제에서는 간단한 방향 센서 입력을 앱에 통합 하기 위해 작성 해야 하는 코드의 양을 보여 줍니다.

앱은 **Mainpage** 메서드의 기본 센서와의 연결을 설정 합니다.

```csharp
_simpleorientation = SimpleOrientationSensor.GetDefault();
```

새 센서 데이터가 **OrientationChanged** 메서드에서 캡처됩니다. 센서 드라이버가 센서 로부터 새 데이터를 받을 때마다이 이벤트 처리기를 사용 하 여 앱에 값을 전달 합니다. 앱은 다음 줄에이 이벤트 처리기를 등록 합니다.

```csharp
_simpleorientation.OrientationChanged += new TypedEventHandler<SimpleOrientationSensor,
SimpleOrientationSensorOrientationChangedEventArgs>(OrientationChanged);
```

이러한 새 값은 프로젝트의 XAML에서 찾은 TextBlock에 기록 됩니다.

```csharp
<TextBlock HorizontalAlignment="Left" Height="24" Margin="8,8,0,0" TextWrapping="Wrap" Text="Current Orientation:" VerticalAlignment="Top" Width="101" Foreground="#FFF8F7F7"/>
 <TextBlock x:Name="txtOrientation" HorizontalAlignment="Left" Height="24" Margin="118,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="175" Foreground="#FFFEFAFA"/>
```