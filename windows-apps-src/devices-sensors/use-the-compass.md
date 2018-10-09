---
author: muhsinking
ms.assetid: 5B30E32F-27E0-4656-A834-391A559AC8BC
title: 나침반 사용
description: 나침반을 사용하여 현재 전면부를 판단하는 방법을 알아봅니다.
ms.author: mukin
ms.date: 06/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f2bddb9ae3adf8ef6cfdf1b6c078db5eb026c93d
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2018
ms.locfileid: "4467366"
---
# <a name="use-the-compass"></a>나침반 사용


**중요 API**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**나침반**](https://msdn.microsoft.com/library/windows/apps/BR225705)

**샘플**

-   더 완전한 구현은 [나침반 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Compass)을 참조하세요.

나침반을 사용하여 현재 전면부를 판단하는 방법을 알아봅니다.

앱은 자북 또는 실제 북쪽을 기준으로 현재 전면부를 검색할 수 있습니다. 탐색 앱은 나침반을 사용하여 장치가 향하고 있는 방향을 판단한 다음 그에 따라 지도의 방위치를 확정합니다.

## <a name="prerequisites"></a>사전 요구 사항

XAML(Extensible Application Markup Language), Microsoft Visual C# 및 이벤트에 대해 알고 있어야 합니다.

사용하는 장치 또는 에뮬레이터가 나침반을 지원해야 합니다.

## <a name="create-a-simple-compass-app"></a>간단한 나침반 앱 만들기

이 섹션은 두 개의 하위 섹션으로 나뉩니다. 첫 번째 하위 섹션에서는 처음부터 간단한 나침반 응용 프로그램을 만드는 데 필요한 단계를 안내합니다. 다음 하위 섹션에서는 방금 만든 앱에 대해 설명합니다.

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

    using Windows.UI.Core; // Required to access the core dispatcher object
    using Windows.Devices.Sensors; // Required to access the sensor platform and the compass


    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private Compass _compass; // Our app' s compass object

            // This event handler writes the current compass reading to
            // the textblocks on the app' s main page.

            private async void ReadingChanged(object sender, CompassReadingChangedEventArgs e)
            {
               await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    CompassReading reading = e.Reading;
                    txtMagnetic.Text = String.Format("{0,5:0.00}", reading.HeadingMagneticNorth);
                    if (reading.HeadingTrueNorth.HasValue)
                        txtNorth.Text = String.Format("{0,5:0.00}", reading.HeadingTrueNorth);
                    else
                        txtNorth.Text = "No reading.";
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
               _compass = Compass.GetDefault(); // Get the default compass object

                // Assign an event handler for the compass reading-changed event
                if (_compass != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _compass.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _compass.ReportInterval = reportInterval;
                    _compass.ReadingChanged += new TypedEventHandler<Compass, CompassReadingChangedEventArgs>(ReadingChanged);
                }
            }
        }
    }
    ```

You'll need to rename the namespace in the previous snippet with the name you gave your project. For example, if you created a project named **CompassCS**, you'd replace `namespace App1` with `namespace CompassCS`.

-   Open the file MainPage.xaml and replace the original contents with the following XML.

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
            <TextBlock HorizontalAlignment="Left" Height="22" Margin="8,18,0,0" TextWrapping="Wrap" Text="Magnetic Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFFBF9F9"/>
            <TextBlock HorizontalAlignment="Left" Height="18" Margin="8,58,0,0" TextWrapping="Wrap" Text="True North Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFF3F3F3"/>
            <TextBlock x:Name="txtMagnetic" HorizontalAlignment="Left" Height="22" Margin="130,18,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFFBF6F6"/>
            <TextBlock x:Name="txtNorth" HorizontalAlignment="Left" Height="18" Margin="130,58,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFF5F1F1"/>

         </Grid>
    </Page>
```

이전 코드 조각에서 클래스 이름의 첫 번째 부분을 앱의 네임스페이스로 바꾸어야 합니다. 예를 들어 **CompassCS**라는 프로젝트를 만든 경우 `x:Class="App1.MainPage"`을 `x:Class="CompassCS.MainPage"`로 바꿉니다. 또한 `xmlns:local="using:App1"`을 `xmlns:local="using:CompassCS"`로 바꾸어야 합니다.

-   F5 키를 누르거나 **디버그** > **디버깅 시작**을 선택하여 앱을 빌드, 배포 및 실행합니다.

앱이 실행 중이면 디바이스를 이동하거나 에뮬레이터 도구를 사용하여 나침반 값을 변경할 수 있습니다.

-   Visual Studio로 돌아가서 Shift+F5를 눌러 앱을 중지하거나 **디버그** > **디버깅 중지**를 선택하여 앱을 중지합니다.

### <a name="explanation"></a>설명

앞의 예는 앱에서 나침반 입력을 통합하기 위해 작성해야 하는 코드의 양이 얼마나 작은지를 보여줍니다.

앱은 **MainPage** 메서드에서 기본 나침반과 연결합니다.

```csharp
_compass = Compass.GetDefault(); // Get the default compass object
```

앱은 **MainPage** 메서드 내에서 보고 간격을 설정합니다. 이 코드는 디바이스에서 지원되는 최소 간격을 검색하여 요청된 16밀리초 간격(약 60Hz 새로 고침 빈도)과 비교합니다. 지원되는 최소 간격이 요청된 간격보다 큰 경우 코드는 값을 최소값으로 설정합니다. 그렇지 않으면 값을 요청된 간격으로 설정합니다.

```csharp
uint minReportInterval = _compass.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_compass.ReportInterval = reportInterval;
```

새 나침반 데이터는 **ReadingChanged** 메서드에서 캡처됩니다. 센서 드라이버는 센서에서 새 데이터를 받을 때마다 이 이벤트 처리기를 사용하여 이 값을 앱에 전달합니다. 앱은 다음 줄에서 이 이벤트 처리기를 등록합니다.

```csharp
_compass.ReadingChanged += new TypedEventHandler<Compass,
CompassReadingChangedEventArgs>(ReadingChanged);
```

이 새 값은 프로젝트의 XAML에서 찾은 TextBlock에 쓰입니다.

```xml
 <TextBlock HorizontalAlignment="Left" Height="22" Margin="8,18,0,0" TextWrapping="Wrap" Text="Magnetic Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFFBF9F9"/>
 <TextBlock HorizontalAlignment="Left" Height="18" Margin="8,58,0,0" TextWrapping="Wrap" Text="True North Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFF3F3F3"/>
 <TextBlock x:Name="txtMagnetic" HorizontalAlignment="Left" Height="22" Margin="130,18,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFFBF6F6"/>
 <TextBlock x:Name="txtNorth" HorizontalAlignment="Left" Height="18" Margin="130,58,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFF5F1F1"/>
```
 

 
