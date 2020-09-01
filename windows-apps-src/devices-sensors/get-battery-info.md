---
ms.assetid: 90BB59FC-90FE-453E-A8DE-9315E29EB98C
title: 배터리 정보 가져오기
description: Windows.Devices.Power 네임스페이스에서 API를 사용하여 자세한 배터리 정보를 가져오는 방법을 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 383cb37e044e7e13080526d8e8270e7fc36560ff
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172237"
---
# <a name="get-battery-information"></a>배터리 정보 가져오기


* * 중요 한 Api * *

-   [**Windows. 장치. 전원**](/uwp/api/Windows.Devices.Power)
-   [**DeviceInformation. FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)

[  **Windows.Devices.Power**](/uwp/api/Windows.Devices.Power) 네임스페이스에서 API를 사용하여 자세한 배터리 정보를 가져오는 방법을 알아봅니다. *배터리 보고서* ([**BatteryReport**](/uwp/api/Windows.Devices.Power.BatteryReport))는 배터리 또는 배터리 집계의 요금, 용량 및 상태를 설명 합니다. 이 항목에서는 앱이 배터리 보고서를 받고 변경 내용에 대 한 알림 메시지를 표시 하는 방법을 보여 줍니다. 코드 예제는이 항목의 끝에 나열 된 기본 배터리 앱에서 가져온 것입니다.

## <a name="get-aggregate-battery-report"></a>집계 배터리 보고서 가져오기


일부 장치에는 두 개 이상의 배터리가 포함 되어 있으며, 각 배터리가 장치의 전체 에너지 용량에 어떻게 기여 하는 지는 항상 명확 하지는 않습니다. [**AggregateBattery**](/uwp/api/windows.devices.power.battery.aggregatebattery) 클래스가 제공 되는 위치입니다. *집계 배터리* 는 장치에 연결 된 모든 배터리 컨트롤러를 나타내며 단일 전체 [**BatteryReport**](/uwp/api/Windows.Devices.Power.BatteryReport) 개체를 제공할 수 있습니다.

**참고**    [**배터리**](/uwp/api/Windows.Devices.Power.Battery) 클래스는 실제로 배터리 컨트롤러에 해당 합니다. 장치에 따라 컨트롤러는 실제 배터리에 연결 된 경우에도 장치 엔클로저에 연결 되는 경우도 있습니다. 따라서 배터리가 없을 때에도 배터리 개체를 만들 수 있습니다. 다른 경우에는 배터리 개체가 **null**일 수 있습니다.

집계 배터리 개체가 있으면 [**Getreport**](/uwp/api/windows.devices.power.battery.getreport) 를 호출 하 여 해당 [**BatteryReport**](/uwp/api/Windows.Devices.Power.BatteryReport)를 가져옵니다.

```csharp
private void RequestAggregateBatteryReport()
{
    // Create aggregate battery object
    var aggBattery = Battery.AggregateBattery;

    // Get report
    var report = aggBattery.GetReport();

    // Update UI
    AddReportUI(BatteryReportPanel, report, aggBattery.DeviceId);
}
```

## <a name="get-individual-battery-reports"></a>개별 배터리 보고서 가져오기

개별 배터리에 대해 [**BatteryReport**](/uwp/api/Windows.Devices.Power.BatteryReport) 개체를 만들 수도 있습니다. [**FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 메서드와 함께 [**getdeviceselector**](/uwp/api/windows.devices.power.battery.getdeviceselector) 를 사용 하 여 장치에 연결 된 모든 배터리 컨트롤러를 나타내는 [**deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 개체의 컬렉션을 가져옵니다. 그런 다음 원하는 **Deviceinformation** 개체의 **Id** 속성을 사용 하 여 [**FromIdAsync**](/uwp/api/windows.devices.power.battery.fromidasync) 메서드를 사용 하 여 해당 [**배터리**](/uwp/api/Windows.Devices.Power.Battery) 를 만듭니다. 마지막으로 [**Getreport**](/uwp/api/windows.devices.power.battery.getreport) 를 호출 하 여 개별 배터리 보고서를 가져옵니다.

이 예제에서는 장치에 연결 된 모든 배터리에 대 한 배터리 보고서를 만드는 방법을 보여 줍니다.

```csharp
async private void RequestIndividualBatteryReports()
{
    // Find batteries 
    var deviceInfo = await DeviceInformation.FindAllAsync(Battery.GetDeviceSelector());
    foreach(DeviceInformation device in deviceInfo)
    {
        try
        {
        // Create battery object
        var battery = await Battery.FromIdAsync(device.Id);

        // Get report
        var report = battery.GetReport();

        // Update UI
        AddReportUI(BatteryReportPanel, report, battery.DeviceId);
        }
        catch { /* Add error handling, as applicable */ }
    }
}
```

## <a name="access-report-details"></a>보고서 세부 정보 액세스

[**BatteryReport**](/uwp/api/Windows.Devices.Power.BatteryReport) 개체는 많은 배터리 정보를 제공 합니다. 자세한 내용은 해당 속성에 대 한 API 참조, **상태** ( [**BatteryStatus**](/previous-versions/windows/dn818458(v=win.10)) 열거형), [**ChargeRateInMilliwatts**](/uwp/api/windows.devices.power.batteryreport.chargerateinmilliwatts), [**DesignCapacityInMilliwattHours**](/uwp/api/windows.devices.power.batteryreport.designcapacityinmilliwatthours), [**FullChargeCapacityInMilliwattHours**](/uwp/api/windows.devices.power.batteryreport.fullchargecapacityinmilliwatthours)및 [**RemainingCapacityInMilliwattHours**](/uwp/api/windows.devices.power.batteryreport.remainingcapacityinmilliwatthours)를 참조 하세요. 이 예제는이 항목의 뒷부분에서 제공 하는 기본 배터리 앱에서 사용 하는 일부 배터리 보고서 속성을 보여 줍니다.

```csharp
...
TextBlock txt3 = new TextBlock { Text = "Charge rate (mW): " + report.ChargeRateInMilliwatts.ToString() };
TextBlock txt4 = new TextBlock { Text = "Design energy capacity (mWh): " + report.DesignCapacityInMilliwattHours.ToString() };
TextBlock txt5 = new TextBlock { Text = "Fully-charged energy capacity (mWh): " + report.FullChargeCapacityInMilliwattHours.ToString() };
TextBlock txt6 = new TextBlock { Text = "Remaining energy capacity (mWh): " + report.RemainingCapacityInMilliwattHours.ToString() };
...
...
```

## <a name="request-report-updates"></a>보고서 업데이트 요청

배터리 [**개체는**](/uwp/api/Windows.Devices.Power.Battery) 배터리 청구, 용량 또는 상태가 변경 될 때 [**reportupdated**](/uwp/api/windows.devices.power.battery.reportupdated) 이벤트를 트리거합니다. 일반적으로 상태를 변경 하 고 다른 모든 변경 내용에 대해 주기적으로 발생 합니다. 이 예제에서는 배터리 보고서 업데이트를 등록 하는 방법을 보여 줍니다.

```csharp
...
Battery.AggregateBattery.ReportUpdated += AggregateBattery_ReportUpdated;
...
```

## <a name="handle-report-updates"></a>보고서 업데이트 처리

배터리 업데이트가 발생 하면 [**Reportupdated**](/uwp/api/windows.devices.power.battery.reportupdated) 이벤트는 해당 [**배터리**](/uwp/api/Windows.Devices.Power.Battery) 개체를 이벤트 처리기 메서드에 전달 합니다. 그러나이 이벤트 처리기는 UI 스레드에서 호출 되지 않습니다. 이 예제와 같이 [**디스패처**](/uwp/api/Windows.UI.Core.CoreDispatcher) 개체를 사용 하 여 UI 변경을 호출 해야 합니다.

```csharp
async private void AggregateBattery_ReportUpdated(Battery sender, object args)
{
    if (reportRequested)
    {

        await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            // Clear UI
            BatteryReportPanel.Children.Clear();


            if (AggregateButton.IsChecked == true)
            {
                // Request aggregate battery report
                RequestAggregateBatteryReport();
            }
            else
            {
                // Request individual battery report
                RequestIndividualBatteryReports();
            }
        });
    }
}
```

## <a name="example-basic-battery-app"></a>예: 기본 배터리 앱

Microsoft Visual Studio에서 다음과 같은 기본 배터리 앱을 빌드하여 이러한 Api를 테스트 합니다. Visual Studio 시작 페이지에서 **새 프로젝트**를 클릭 하 고 **Visual c # &gt; Windows &gt; 유니버설** 템플릿 아래에서 **비어 있는 앱** 템플릿을 사용 하 여 새 앱을 만듭니다.

다음으로, **mainpage .xaml** 파일을 열고 다음 XML을이 파일에 복사 합니다 (원래 내용 바꾸기).

```xml
<Page
    x:Class="App1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" >
        <StackPanel VerticalAlignment="Center" Margin="15,30,0,0" >
            <RadioButton x:Name="AggregateButton" Content="Aggregate results" GroupName="Type" IsChecked="True" />
            <RadioButton x:Name="IndividualButton" Content="Individual results" GroupName="Type" IsChecked="False" />
        </StackPanel>
        <StackPanel Orientation="Horizontal">
        <Button x:Name="GetBatteryReportButton" 
                Content="Get battery report" 
                Margin="15,15,0,0" 
                Click="GetBatteryReport"/>
        </StackPanel>
        <StackPanel x:Name="BatteryReportPanel" Margin="15,15,0,0"/>
    </StackPanel>
</Page>
```

앱 이름이 **App1**이 아닌 경우 이전 코드 조각에서 클래스 이름의 첫 번째 부분을 앱의 네임 스페이스로 바꾸어야 합니다. 예를 들어 **BasicBatteryApp**이라는 프로젝트를 만든 경우를 `x:Class="App1.MainPage"` 로 바꿉니다 `x:Class="BasicBatteryApp.MainPage"` . 또한를로 바꾸어야 `xmlns:local="using:App1"` 합니다 `xmlns:local="using:BasicBatteryApp"` .

그런 다음 프로젝트의 **MainPage.xaml.cs** 파일을 열고 기존 코드를 다음 코드로 바꿉니다.

```csharp
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
using Windows.Devices.Enumeration;
using Windows.Devices.Power;
using Windows.UI.Core;

namespace App1
{
    public sealed partial class MainPage : Page
    {
        bool reportRequested = false;
        public MainPage()
        {
            this.InitializeComponent();
            Battery.AggregateBattery.ReportUpdated += AggregateBattery_ReportUpdated;
        }


        private void GetBatteryReport(object sender, RoutedEventArgs e)
        {
            // Clear UI
            BatteryReportPanel.Children.Clear();


            if (AggregateButton.IsChecked == true)
            {
                // Request aggregate battery report
                RequestAggregateBatteryReport();
            }
            else
            {
                // Request individual battery report
                RequestIndividualBatteryReports();
            }

            // Note request
            reportRequested = true;
        }

        private void RequestAggregateBatteryReport()
        {
            // Create aggregate battery object
            var aggBattery = Battery.AggregateBattery;

            // Get report
            var report = aggBattery.GetReport();

            // Update UI
            AddReportUI(BatteryReportPanel, report, aggBattery.DeviceId);
        }

        async private void RequestIndividualBatteryReports()
        {
            // Find batteries 
            var deviceInfo = await DeviceInformation.FindAllAsync(Battery.GetDeviceSelector());
            foreach(DeviceInformation device in deviceInfo)
            {
                try
                {
                // Create battery object
                var battery = await Battery.FromIdAsync(device.Id);

                // Get report
                var report = battery.GetReport();

                // Update UI
                AddReportUI(BatteryReportPanel, report, battery.DeviceId);
                }
                catch { /* Add error handling, as applicable */ }
            }
        }


        private void AddReportUI(StackPanel sp, BatteryReport report, string DeviceID)
        {
            // Create battery report UI
            TextBlock txt1 = new TextBlock { Text = "Device ID: " + DeviceID };
            txt1.FontSize = 15;
            txt1.Margin = new Thickness(0, 15, 0, 0);
            txt1.TextWrapping = TextWrapping.WrapWholeWords;

            TextBlock txt2 = new TextBlock { Text = "Battery status: " + report.Status.ToString() };
            txt2.FontStyle = Windows.UI.Text.FontStyle.Italic;
            txt2.Margin = new Thickness(0, 0, 0, 15);

            TextBlock txt3 = new TextBlock { Text = "Charge rate (mW): " + report.ChargeRateInMilliwatts.ToString() };
            TextBlock txt4 = new TextBlock { Text = "Design energy capacity (mWh): " + report.DesignCapacityInMilliwattHours.ToString() };
            TextBlock txt5 = new TextBlock { Text = "Fully-charged energy capacity (mWh): " + report.FullChargeCapacityInMilliwattHours.ToString() };
            TextBlock txt6 = new TextBlock { Text = "Remaining energy capacity (mWh): " + report.RemainingCapacityInMilliwattHours.ToString() };

            // Create energy capacity progress bar & labels
            TextBlock pbLabel = new TextBlock { Text = "Percent remaining energy capacity" };
            pbLabel.Margin = new Thickness(0,10, 0, 5);
            pbLabel.FontFamily = new FontFamily("Segoe UI");
            pbLabel.FontSize = 11;

            ProgressBar pb = new ProgressBar();
            pb.Margin = new Thickness(0, 5, 0, 0);
            pb.Width = 200;
            pb.Height = 10;
            pb.IsIndeterminate = false;
            pb.HorizontalAlignment = HorizontalAlignment.Left;

            TextBlock pbPercent = new TextBlock();
            pbPercent.Margin = new Thickness(0, 5, 0, 10);
            pbPercent.FontFamily = new FontFamily("Segoe UI");
            pbLabel.FontSize = 11;

            // Disable progress bar if values are null
            if ((report.FullChargeCapacityInMilliwattHours == null)||
                (report.RemainingCapacityInMilliwattHours == null))
            {
                pb.IsEnabled = false;
                pbPercent.Text = "N/A";
            }
            else
            {
                pb.IsEnabled = true;
                pb.Maximum = Convert.ToDouble(report.FullChargeCapacityInMilliwattHours);
                pb.Value = Convert.ToDouble(report.RemainingCapacityInMilliwattHours);
                pbPercent.Text = ((pb.Value / pb.Maximum) * 100).ToString("F2") + "%";
            }

            // Add controls to stackpanel
            sp.Children.Add(txt1);
            sp.Children.Add(txt2);
            sp.Children.Add(txt3);
            sp.Children.Add(txt4);
            sp.Children.Add(txt5);
            sp.Children.Add(txt6);
            sp.Children.Add(pbLabel);
            sp.Children.Add(pb);
            sp.Children.Add(pbPercent);
        }

        async private void AggregateBattery_ReportUpdated(Battery sender, object args)
        {
            if (reportRequested)
            {

                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    // Clear UI
                    BatteryReportPanel.Children.Clear();


                    if (AggregateButton.IsChecked == true)
                    {
                        // Request aggregate battery report
                        RequestAggregateBatteryReport();
                    }
                    else
                    {
                        // Request individual battery report
                        RequestIndividualBatteryReports();
                    }
                });
            }
        }
    }
}
```

앱 이름이 **App1**이 아닌 경우에는 이전 예제에서 프로젝트에 지정한 이름으로 네임 스페이스의 이름을 변경 해야 합니다. 예를 들어 **BasicBatteryApp**이라는 프로젝트를 만든 경우 네임 스페이스를 네임 스페이스로 바꿉니다 `App1` `BasicBatteryApp` .

마지막으로,이 기본 배터리 앱을 실행 하려면 **디버그** 메뉴에서 **디버깅 시작** 을 클릭 하 여 솔루션을 테스트 합니다.

**팁**    [**BatteryReport**](/uwp/api/Windows.Devices.Power.BatteryReport) 개체에서 숫자 값을 수신 하려면 **로컬 컴퓨터** 또는 외부 **장치** (예: Windows Phone)에서 앱을 디버그 합니다. 장치 에뮬레이터에서 디버그할 때 **BatteryReport** 개체는 capacity 및 rate 속성에 **null** 을 반환 합니다.

 