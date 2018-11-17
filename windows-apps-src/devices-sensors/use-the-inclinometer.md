---
author: muhsinking
ms.assetid: 16AD53CA-1252-456C-8567-2263D3EC95F3
title: 경사계 사용
description: 경사계를 사용하여 피치, 롤 및 요를 확인하는 방법을 알아봅니다.
ms.author: mukin
ms.date: 06/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dd335d56fb2a01ed1b9255f974bcaacd47f623f5
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7144949"
---
# <a name="use-the-inclinometer"></a>경사계 사용


**중요 API**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**경사계**](https://msdn.microsoft.com/library/windows/apps/BR225766)

**샘플**

-   더 완전한 구현은 [경사계 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Inclinometer)을 참조하세요.

경사계를 사용하여 피치, 롤 및 요를 판단하는 방법을 알아봅니다.

일부 3차원 게임에는 입력 장치로서 경사계가 필요합니다. 일반적인 한 예로, 경사계의 세 축(X, Y 및 Z)을 항공기의 승강키, 보조 날개 및 방향타 입력에 매핑하는 비행 시뮬레이터가 있습니다.

 ## <a name="prerequisites"></a>사전 요구 사항

응용 프로그램 언어 XAML (Extensible Markup), Microsoft VisualC # 및 이벤트 잘 알고 있어야 합니다.

사용하는 장치 또는 에뮬레이터가 경사계를 지원해야 합니다.

 ## <a name="create-a-simple-inclinometer-app"></a>간단한 경사계 앱 만들기

이 섹션은 두 개의 하위 섹션으로 나뉩니다. 첫 번째 하위 섹션에서는 처음부터 간단한 경사계 응용 프로그램을 만드는 데 필요한 단계를 안내합니다. 다음 하위 섹션에서는 방금 만든 앱에 대해 설명합니다.

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

이전 코드 조각의 네임스페이스 이름을 프로젝트에 지정한 이름으로 바꾸어야 합니다. 예를 들어 **InclinometerCS**라는 프로젝트를 만든 경우 `namespace App1`을 `namespace InclinometerCS`로 바꿉니다.

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
            <TextBlock HorizontalAlignment="Left" Height="21" Margin="0,8,0,0" TextWrapping="Wrap" Text="Pitch: " VerticalAlignment="Top" Width="45" Foreground="#FFF9F4F4"/>
            <TextBlock x:Name="txtPitch" HorizontalAlignment="Left" Height="21" Margin="59,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="71" Foreground="#FFFDF9F9"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="0,29,0,0" TextWrapping="Wrap" Text="Roll:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F1F1"/>
            <TextBlock x:Name="txtRoll" HorizontalAlignment="Left" Height="23" Margin="59,29,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="50" Foreground="#FFFCF9F9"/>
            <TextBlock HorizontalAlignment="Left" Height="19" Margin="0,56,0,0" TextWrapping="Wrap" Text="Yaw:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F3F3"/>
            <TextBlock x:Name="txtYaw" HorizontalAlignment="Left" Height="19" Margin="55,56,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="54" Foreground="#FFF6F2F2"/>

        </Grid>
    </Page>
```

이전 코드 조각에서 클래스 이름의 첫 번째 부분을 앱의 네임스페이스로 바꾸어야 합니다. 예를 들어 **InclinometerCS**라는 프로젝트를 만든 경우 `x:Class="App1.MainPage"`을 `x:Class="InclinometerCS.MainPage"`로 바꿉니다. 또한 `xmlns:local="using:App1"`을 `xmlns:local="using:InclinometerCS"`로 바꾸어야 합니다.

-   F5 키를 누르거나 **디버그** > **디버깅 시작**을 선택하여 앱을 빌드, 배포 및 실행합니다.

앱이 실행 중이면 디바이스를 이동하거나 에뮬레이터 도구를 사용하여 경사계 값을 변경할 수 있습니다.

-   Visual Studio로 돌아가서 Shift+F5를 눌러 앱을 중지하거나 **디버그** > **디버깅 중지**를 선택하여 앱을 중지합니다.

###  <a name="explanation"></a>설명

앞의 예는 앱에서 경사계 입력을 통합하기 위해 작성해야 하는 코드의 양이 얼마나 작은지를 보여줍니다.

앱은 **MainPage** 메서드에서 기본 경사계와 연결합니다.

```csharp
_inclinometer = Inclinometer.GetDefault();
```

앱은 **MainPage** 메서드 내에서 보고 간격을 설정합니다. 이 코드는 디바이스에서 지원되는 최소 간격을 검색하여 요청된 16밀리초 간격(약 60Hz 새로 고침 빈도)과 비교합니다. 지원되는 최소 간격이 요청된 간격보다 큰 경우 코드는 값을 최소값으로 설정합니다. 그렇지 않으면 값을 요청된 간격으로 설정합니다.

```csharp
uint minReportInterval = _inclinometer.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_inclinometer.ReportInterval = reportInterval;
```

새 경사계 데이터는 **ReadingChanged** 메서드에서 캡처됩니다. 센서 드라이버는 센서에서 새 데이터를 받을 때마다 이 이벤트 처리기를 사용하여 이 값을 앱에 전달합니다. 앱은 다음 줄에서 이 이벤트 처리기를 등록합니다.

```csharp
_inclinometer.ReadingChanged += new TypedEventHandler<Inclinometer,
InclinometerReadingChangedEventArgs>(ReadingChanged);
```

이 새 값은 프로젝트의 XAML에서 찾은 TextBlock에 쓰입니다.

```xml
<TextBlock HorizontalAlignment="Left" Height="21" Margin="0,8,0,0" TextWrapping="Wrap" Text="Pitch: " VerticalAlignment="Top" Width="45" Foreground="#FFF9F4F4"/>
 <TextBlock x:Name="txtPitch" HorizontalAlignment="Left" Height="21" Margin="59,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="71" Foreground="#FFFDF9F9"/>
 <TextBlock HorizontalAlignment="Left" Height="23" Margin="0,29,0,0" TextWrapping="Wrap" Text="Roll:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F1F1"/>
 <TextBlock x:Name="txtRoll" HorizontalAlignment="Left" Height="23" Margin="59,29,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="50" Foreground="#FFFCF9F9"/>
 <TextBlock HorizontalAlignment="Left" Height="19" Margin="0,56,0,0" TextWrapping="Wrap" Text="Yaw:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F3F3"/>
 <TextBlock x:Name="txtYaw" HorizontalAlignment="Left" Height="19" Margin="55,56,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="54" Foreground="#FFF6F2F2"/>
```

