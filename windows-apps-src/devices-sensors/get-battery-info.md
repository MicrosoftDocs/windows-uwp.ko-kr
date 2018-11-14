---
author: muhsinking
ms.assetid: 90BB59FC-90FE-453E-A8DE-9315E29EB98C
title: 배터리 정보 가져오기
description: Windows.Devices.Power 네임스페이스에서 API를 사용하여 자세한 배터리 정보를 가져오는 방법을 알아봅니다.
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c745b99104495b4d0b3c60202c378285dbfdd7b6
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/14/2018
ms.locfileid: "6673845"
---
# <a name="get-battery-information"></a>배터리 정보 가져오기


** 중요 API **

-   [**Windows.Devices.Power**](https://msdn.microsoft.com/library/windows/apps/Dn895017)
-   [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/BR225432)

[**Windows.Devices.Power**](https://msdn.microsoft.com/library/windows/apps/Dn895017) 네임스페이스에서 API를 사용하여 자세한 배터리 정보를 가져오는 방법을 알아봅니다. *배터리 보고서*([**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005))는 배터리의 충전, 용량 및 상태 또는 배터리의 집계를 설명합니다. 이 항목에서는 앱에서 배터리 보고서를 가져오고 변경 알림을 받을 수 있는 방법을 보여 줍니다. 코드 예제는 이 항목의 끝에 나열된 기본 배터리 앱에서 발췌한 것입니다.

## <a name="get-aggregate-battery-report"></a>배터리 집계 보고서 가져오기


일부 장치에는 둘 이상의 배터리가 있으며, 각 배터리가 장치의 전체 에너지 용량에 기여하는 정도가 명확하지 않을 수 있습니다. 이때는 [**AggregateBattery**](https://msdn.microsoft.com/library/windows/apps/Dn895011) 클래스가 필요합니다. *배터리 집계*는 장치에 연결된 모든 배터리 컨트롤러를 나타내며 전체 단일 [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005) 개체를 제공할 수 있습니다.

**참고** [**배터리**](https://msdn.microsoft.com/library/windows/apps/Dn895004) 클래스는 실제로 배터리 컨트롤러에 해당 합니다. 장치에 따라 컨트롤러가 실제 배터리에 연결되거나 장치 엔클로저에 연결될 수 있습니다. 따라서 배터리가 없는 경우에도 배터리 개체를 만들 수 있습니다. 다른 경우에는 배터리 개체가 **null**일 수 있습니다.

배터리 집계 개체를 만든 후에는 [**GetReport**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.getreport)를 호출하여 해당 [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005)를 가져옵니다.

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

개별 배터리에 대한 [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005) 개체를 만들 수도 있습니다. [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/BR225432) 메서드와 함께 [**GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.getdeviceselector.aspx)를 사용하여 장치에 연결된 모든 배터리 컨트롤러를 나타내는 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 개체의 컬렉션을 가져옵니다. 그런 다음 원하는 **DeviceInformation** 개체의 **Id** 속성을 사용하여 [**FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.fromidasync.aspx) 메서드로 해당 [**Battery**](https://msdn.microsoft.com/library/windows/apps/Dn895004)를 만듭니다. 마지막으로 [**GetReport**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.getreport)를 호출하여 개별 배터리 보고서를 가져옵니다.

이 예제에서는 장치에 연결된 모든 배터리에 대한 배터리 보고서를 만드는 방법을 보여 줍니다.

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

[**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005) 개체는 많은 배터리 정보를 제공합니다. 자세한 내용은 해당 속성에 대한 API 참조를 참조하세요(**Status**([**BatteryStatus**](https://msdn.microsoft.com/library/windows/apps/Dn818458) 열거), [**ChargeRateInMilliwatts**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.batteryreport.chargerateinmilliwatts.aspx), [**DesignCapacityInMilliwattHours**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.batteryreport.designcapacityinmilliwatthours.aspx), [**FullChargeCapacityInMilliwattHours**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.batteryreport.fullchargecapacityinmilliwatthours.aspx) 및 [**RemainingCapacityInMilliwattHours**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.batteryreport.remainingcapacityinmilliwatthours)). 이 예제에서는 이 항목의 뒷부분에 나와 있는 기본 배터리 앱에서 사용되는 일부 배터리 보고서 속성을 보여 줍니다.

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

[**Battery**](https://msdn.microsoft.com/library/windows/apps/Dn895004) 개체는 배터리의 충전, 용량 또는 상태가 변경된 경우 [**ReportUpdated**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.reportupdated) 이벤트를 트리거합니다. 일반적으로 이는 상태 변경 시 즉시 발생하고, 다른 모든 변경 시 주기적으로 발생합니다. 이 예제에서는 배터리 보고서 업데이트에 등록하는 방법을 보여 줍니다.

```csharp
...
Battery.AggregateBattery.ReportUpdated += AggregateBattery_ReportUpdated;
...
```

## <a name="handle-report-updates"></a>보고서 업데이트 처리

배터리 업데이트가 발생한 경우 [**ReportUpdated**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.reportupdated) 이벤트는 해당 [**Battery**](https://msdn.microsoft.com/library/windows/apps/Dn895004) 개체를 이벤트 처리기 메서드로 전달합니다. 그러나 이 이벤트 처리기는 UI 스레드에서 호출되지 않습니다. 이 예제에 표시된 대로 [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/BR208211) 개체를 사용하여 UI 변경 내용을 호출해야 합니다.

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

## <a name="example-basic-battery-app"></a>예제: 기본 배터리 앱

Microsoft Visual Studio에서 다음 기본 배터리 앱을 빌드하여 이러한 API를 테스트합니다. Visual Studio 시작 페이지에서 **새 프로젝트**를 클릭한 다음 **Visual C# &gt; Windows &gt; Universal** 템플릿에서 **빈 앱** 템플릿을 사용하여 새 앱을 만듭니다.

그런 다음 **MainPage.xaml** 파일을 열고 다음 XML을 이 파일에 복사합니다(원래 콘텐츠 바꾸기).

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

앱 이름이 **App1**이 아닌 경우 이전 코드 조각에서 클래스 이름의 첫 번째 부분을 앱의 네임스페이스로 바꾸어야 합니다. 예를 들어 **BasicBatteryApp**이라는 프로젝트를 만든 경우 `x:Class="App1.MainPage"`을 `x:Class="BasicBatteryApp.MainPage"`로 바꿉니다. 또한 `xmlns:local="using:App1"`을 `xmlns:local="using:BasicBatteryApp"`로 바꾸어야 합니다.

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

앱 이름이 **App1**이 아닌 경우 이전 예제의 네임스페이스 이름을 프로젝트에 지정한 이름으로 바꾸어야 합니다. 예를 들어 **BasicBatteryApp**이라는 프로젝트를 만든 경우 `App1` 네임스페이스를 `BasicBatteryApp` 네임스페이스로 바꿉니다.

마지막으로 이 기본 배터리 앱을 실행하려면 **디버그** 메뉴에서 **디버깅 시작**을 클릭하여 솔루션을 테스트합니다.

**팁** [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005) 개체에서 숫자 값을 받으려면 **로컬 컴퓨터** 또는 외부 **장치** (예: Windows Phone)에서 앱을 디버그 합니다. 장치 에뮬레이터에서 디버그하는 경우 **BatteryReport** 개체는 용량 및 속도 속성에 **null**을 반환합니다.

 

