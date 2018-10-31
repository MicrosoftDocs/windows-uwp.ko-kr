---
author: muhsinking
ms.assetid: 1889AC3A-A472-4294-89B8-A642668A8A6E
title: 방향 센서 사용
description: 방향 센서를 사용하여 디바이스 방향을 확인하는 방법을 알아봅니다.
ms.author: mukin
ms.date: 06/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 49199a91f6713b3f18928eaafb6875a49deaf451
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5817902"
---
# <a name="use-the-orientation-sensor"></a>방향 센서 사용


**중요 API**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371)
-   [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/BR206399)

**샘플**

-   [방향 센서 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/OrientationSensor)
-   [단순 방향 센서 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleOrientationSensor)

방향 센서를 사용하여 디바이스 방향을 확인하는 방법을 알아봅니다.

[**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408) 네임스페이스에는 [**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371) 및 [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/BR206399)이라는 두 가지 유형의 방향 센서 API가 있습니다. 두 센서는 모두 방향 센서이지만 해당 용어가 오버로드되어 매우 다른 용도로 사용됩니다. 그러나 둘 다 방향 센서이므로 모두 이 문서에서 다뤄집니다.

[**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371) API는 3차원 앱이 쿼터니언과 회전 행렬을 가져오는 데 사용됩니다. 쿼터니언은 임의의 축을 중심으로 한 점 \[x,y,z\]의 회전이라고 이해하면 제일 쉽습니다(세 축을 중심으로 한 회전을 나타내는 회전 행렬과 대조적). 쿼터니언의 수학적 배경은 쿼터니언이 복소수의 기하학적 속성과 허수의 수학적 속성과 관련이 있으므로 매우 매력적이지만 이에 대한 작업은 간단하고 DirectX같은 프레임워크가 이를 지원합니다. 복합 3-D 앱은 방향 센서를 사용하여 사용자 시각을 조정할 수 있습니다. 이 센서는 가속도계, 회전계 및 나침반의 입력을 결합합니다.

[**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/BR206399) API는 세로 위로, 세로 아래로, 가로 왼쪽, 가로 오른쪽과 같은 정의 측면에서 현재 장치 방향을 확인하는 데 사용됩니다. 장치가 앞면 위로 또는 앞면 아래로인지도 탐지할 수 있습니다. 이 센서는 "세로 위로"나 "가로 왼쪽"같은 속성을 반환하기 보다는 "Not rotated(회전하지 않음)", "Rotated90DegreesCounterclockwise" 등과 같은 회전 값을 반환합니다. 다음 표는 일반적인 방향 속성을 해당 센서 표기에 매핑합니다.

| 방향     | 해당 센서 표기      |
|-----------------|-----------------------------------|
| Portrait Up(세로 위로)     | NotRotated                        |
| Landscape Left(가로 왼쪽)  | Rotated90DegreesCounterclockwise  |
| Portrait Down(세로 아래로)   | Rotated180DegreesCounterclockwise |
| Landscape Right(가로 오른쪽) | Rotated270DegreesCounterclockwise |

## <a name="prerequisites"></a>사전 요구 사항

응용 프로그램 언어 XAML (Extensible Markup), Microsoft VisualC # 및 이벤트 잘 알고 있어야 합니다.

사용하는 장치 또는 에뮬레이터가 방향 센서를 지원해야 합니다.

## <a name="create-an-orientationsensor-app"></a>OrientationSensor 앱 만들기

이 섹션은 두 개의 하위 섹션으로 나뉩니다. 첫 번째 하위 섹션에서는 처음부터 방향 응용 프로그램을 만드는 데 필요한 단계를 안내합니다. 다음 하위 섹션에서는 방금 만든 앱에 대해 설명합니다.

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

    using Windows.UI.Core;
    using Windows.Devices.Sensors;

    // The Blank Page item template is documented at http://go.microsoft.com/fwlink/p/?linkid=234238

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

이전 코드 조각의 네임스페이스 이름을 프로젝트에 지정한 이름으로 바꾸어야 합니다. 예를 들어 **OrientationSensorCS**라는 프로젝트를 만든 경우 `namespace App1` 을 `namespace OrientationSensorCS`로 바꿉니다.

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

이전 코드 조각에서 클래스 이름의 첫 번째 부분을 앱의 네임스페이스로 바꾸어야 합니다. 예를 들어 **OrientationSensorCS**라는 프로젝트를 만든 경우 `x:Class="App1.MainPage"` 을 `x:Class="OrientationSensorCS.MainPage"`로 바꿉니다. 또한 `xmlns:local="using:App1"`을 `xmlns:local="using:OrientationSensorCS"`로 바꾸어야 합니다.

-   F5 키를 누르거나 **디버그** > **디버깅 시작**을 선택하여 앱을 빌드, 배포 및 실행합니다.

앱이 실행 중이면 디바이스를 이동하거나 에뮬레이터 도구를 사용하여 방향을 변경할 수 있습니다.

-   Visual Studio로 돌아가서 Shift+F5를 눌러 앱을 중지하거나 **디버그** > **디버깅 중지**를 선택하여 앱을 중지합니다.

###  <a name="explanation"></a>설명

앞의 예는 앱에서 방향 센서 입력을 통합하기 위해 작성해야 하는 코드의 양이 얼마나 작은지를 보여줍니다.

앱은 **MainPage** 메서드에서 기본 방향 센서와 연결합니다.

```csharp
_sensor = OrientationSensor.GetDefault();
```

앱은 **MainPage** 메서드 내에서 보고 간격을 설정합니다. 이 코드는 디바이스에서 지원되는 최소 간격을 검색하여 요청된 16밀리초 간격(약 60Hz 새로 고침 빈도)과 비교합니다. 지원되는 최소 간격이 요청된 간격보다 큰 경우 코드는 값을 최소값으로 설정합니다. 그렇지 않으면 값을 요청된 간격으로 설정합니다.

```csharp
uint minReportInterval = _sensor.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_sensor.ReportInterval = reportInterval;
```

새 센서 데이터는 **ReadingChanged** 메서드에서 캡처됩니다. 센서 드라이버는 센서에서 새 데이터를 받을 때마다 이 이벤트 처리기를 사용하여 이 값을 앱에 전달합니다. 앱은 다음 줄에서 이 이벤트 처리기를 등록합니다.

```csharp
_sensor.ReadingChanged += new TypedEventHandler<OrientationSensor,
OrientationSensorReadingChangedEventArgs>(ReadingChanged);
```

이 새 값은 프로젝트의 XAML에서 찾은 TextBlock에 쓰입니다.

## <a name="create-a-simpleorientation-app"></a>SimpleOrientation 앱 만들기

이 섹션은 두 개의 하위 섹션으로 나뉩니다. 첫 번째 하위 섹션에서는 처음부터 간단한 방향 응용 프로그램을 만드는 데 필요한 단계를 안내합니다. 다음 하위 섹션에서는 방금 만든 앱에 대해 설명합니다.

### <a name="instructions"></a>지침

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

    using Windows.UI.Core;
    using Windows.Devices.Sensors;
    // The Blank Page item template is documented at http://go.microsoft.com/fwlink/p/?linkid=234238

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

이전 코드 조각의 네임스페이스 이름을 프로젝트에 지정한 이름으로 바꾸어야 합니다. 예를 들어 **SimpleOrientationCS**라는 프로젝트를 만든 경우 `namespace App1` 을 `namespace SimpleOrientationCS`로 바꿉니다.

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
            <TextBlock HorizontalAlignment="Left" Height="24" Margin="8,8,0,0" TextWrapping="Wrap" Text="Current Orientation:" VerticalAlignment="Top" Width="101" Foreground="#FFF8F7F7"/>
            <TextBlock x:Name="txtOrientation" HorizontalAlignment="Left" Height="24" Margin="118,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="175" Foreground="#FFFEFAFA"/>

        </Grid>
    </Page>
```

이전 코드 조각에서 클래스 이름의 첫 번째 부분을 앱의 네임스페이스로 바꾸어야 합니다. 예를 들어 **SimpleOrientationCS**라는 프로젝트를 만든 경우 `x:Class="App1.MainPage"` 을 `x:Class="SimpleOrientationCS.MainPage"`로 바꿉니다. 또한 `xmlns:local="using:App1"`을 `xmlns:local="using:SimpleOrientationCS"`로 바꾸어야 합니다.

-   F5 키를 누르거나 **디버그** > **디버깅 시작**을 선택하여 앱을 빌드, 배포 및 실행합니다.

앱이 실행 중이면 디바이스를 이동하거나 에뮬레이터 도구를 사용하여 방향을 변경할 수 있습니다.

-   Visual Studio로 돌아가서 Shift+F5를 눌러 앱을 중지하거나 **디버그** > **디버깅 중지**를 선택하여 앱을 중지합니다.

### <a name="explanation"></a>설명

앞의 예는 앱에서 단순 방향 센서 입력을 통합하기 위해 작성해야 하는 코드의 양이 얼마나 작은지를 보여줍니다.

앱은 **MainPage** 메서드에서 기본 센서와 연결합니다.

```csharp
_simpleorientation = SimpleOrientationSensor.GetDefault();
```

새 센서 데이터는 **OrientationChanged** 메서드에서 캡처됩니다. 센서 드라이버는 센서에서 새 데이터를 받을 때마다 이 이벤트 처리기를 사용하여 이 값을 앱에 전달합니다. 앱은 다음 줄에서 이 이벤트 처리기를 등록합니다.

```csharp
_simpleorientation.OrientationChanged += new TypedEventHandler<SimpleOrientationSensor,
SimpleOrientationSensorOrientationChangedEventArgs>(OrientationChanged);
```

이 새 값은 프로젝트의 XAML에서 찾은 TextBlock에 쓰입니다.

```csharp
<TextBlock HorizontalAlignment="Left" Height="24" Margin="8,8,0,0" TextWrapping="Wrap" Text="Current Orientation:" VerticalAlignment="Top" Width="101" Foreground="#FFF8F7F7"/>
 <TextBlock x:Name="txtOrientation" HorizontalAlignment="Left" Height="24" Margin="118,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="175" Foreground="#FFFEFAFA"/>
```