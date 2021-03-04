---
title: Xamarin.ios를 사용 하 여 간단한 Android 앱 만들기
description: Windows에서 Xamarin을 사용 하 여 Android 장치에서 작동 하는 플랫폼 간 앱을 만드는 방법에 대 한 단계별 가이드입니다.
author: hickeys
ms.author: hickeys
manager: jken
ms.topic: article
keywords: android, windows, xamarin.ios, xaml, 자습서
ms.date: 04/28/2020
ms.openlocfilehash: b1364d8ac19176ec25ee6d45664c7e765accfe1e
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101823197"
---
# <a name="get-started-developing-for-android-using-xamarinforms"></a>Xamarin.ios를 사용 하 여 Android 용 개발 시작

이 가이드는 Windows에서 Xamarin을 사용 하 여 Android 장치에서 작동 하는 플랫폼 간 앱을 시작 하는 데 도움이 됩니다.

이 문서에서는 Xamarin. Forms 및 Visual Studio 2019을 사용 하 여 간단한 Android 앱을 만듭니다.

## <a name="requirements"></a>요구 사항

이 자습서를 사용 하려면 다음이 필요 합니다.

- 윈도우 10
- [Visual Studio 2019: Community, Professional 또는 Enterprise](https://visualstudio.microsoft.com/downloads/) (참고 참조)
- Visual Studio 2019에 대 한 ".NET을 사용한 모바일 개발" 워크 로드

> [!NOTE]
> 이 가이드는 Visual Studio 2017 또는 2019와 함께 작동 합니다. Visual Studio 2017을 사용 하는 경우 두 버전의 Visual Studio 간 UI 차이로 인해 일부 지침이 올바르지 않을 수 있습니다.

앱을 실행할 Android 휴대폰 또는 구성 된 에뮬레이터를 사용할 수도 있습니다. [Android 장치 또는 에뮬레이터에서 테스트](emulator.md)를 참조 하세요.

## <a name="create-a-new-xamarinforms-project"></a>새 Xamarin Forms 프로젝트 만들기

Visual Studio를 시작합니다. 파일 > 새 > 프로젝트를 클릭 하 여 새 프로젝트를 만듭니다.

새 프로젝트 대화 상자에서 **모바일 앱 (xamarin.ios)** 템플릿을 선택 하 고 **다음** 을 클릭 합니다.

프로젝트 이름을 **TimeChangerForms** 로 선택 하 고 **만들기** 를 클릭 합니다.

새 플랫폼 간 앱 대화 상자에서 **비어 있음** 을 선택 합니다. 플랫폼 섹션에서 **Android** 를 선택 하 고 다른 모든 확인란의 선택을 취소 합니다. **확인** 을 클릭합니다.

Xamarin은 **TimeChangerForms** 및 **TimeChangerForms** 의 두 프로젝트를 사용 하 여 새 솔루션을 만듭니다.

## <a name="create-a-ui-with-xaml"></a>XAML을 사용 하 여 UI 만들기

**TimeChangerForms** 프로젝트를 확장 하 고 **mainpage** 을 엽니다. 이 파일의 XAML은 TimeChanger를 열 때 사용자에 게 표시 되는 첫 번째 화면을 정의 합니다.

TimeChanger UI는 단순 합니다. 현재 시간을 표시 하 고 1 시간 단위로 시간을 조정 하는 단추를 포함 합니다. 세로 StackLayout을 사용 하 여 단추 위에 시간을 맞추고 가로 StackLayout을 사용 하 여 단추를 나란히 정렬 합니다. 세로 StackLayout의 **HorizontalOptions** 및 **VerticalOptions** 를 **"center andexpand"** 로 설정 하 여 콘텐츠를 화면 가운데에 맞춥니다.

MainPage의 내용을 다음 코드로 바꿉니다.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:d="http://xamarin.com/schemas/2014/forms/design"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             mc:Ignorable="d"
             x:Class="TimeChangerForms.MainPage">

    <StackLayout HorizontalOptions="CenterAndExpand"
                 VerticalOptions="CenterAndExpand">
        <Label x:Name="time"
               HorizontalOptions="CenterAndExpand"
               VerticalOptions="CenterAndExpand"
               Text="At runtime, this Label will display the current time.">
        </Label>
        <StackLayout Orientation="Horizontal">
            <Button HorizontalOptions="End"
                    VerticalOptions="End"
                    Text="Up"
                    Clicked="OnUpButton_Clicked"/>
            <Button HorizontalOptions="Start"
                    VerticalOptions="End"
                    Text="Down"
                    Clicked="OnDownButton_Clicked"/>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

이 시점에서 UI가 완료 됩니다. 그러나 **UpButton_Clicked** 및 **DownButton_Clicked** 메서드가 XAML에서 참조 되지만 어디에도 정의 되어 있지 않기 때문에 TimeChangerForms는 빌드되지 않습니다. 앱이 실행 된 경우에도 현재 시간은 표시 되지 않습니다. 다음 섹션에서는 이러한 오류를 수정 하 고 UI에 기능을 추가 합니다.

## <a name="add-logic-code-with-c"></a>C를 사용 하 여 논리 코드 추가 #

솔루션 탐색기에서 MainPage을 마우스 오른쪽 단추로 클릭 하 고 **코드 보기** 를 클릭 합니다. 이 파일에는 UI에 기능을 추가 하는 코드가 포함 되어 있습니다.

### <a name="set-the-current-time"></a>현재 시간 설정

이 파일의 코드는 컨트롤의 **x:Name** 특성 값을 사용 하 여 XAML에 선언 된 컨트롤을 참조할 수 있습니다. 이 경우 현재 시간을 표시 하는 레이블이 호출 됩니다 `time` .

주 스레드에서 UI 컨트롤을 업데이트 해야 합니다. 다른 스레드에서 변경한 내용이 화면에 표시 되 면 해당 컨트롤을 제대로 업데이트 하지 못할 수 있습니다. 이 코드는 항상 주 스레드에서 실행 되는 것이 보장 되지 않기 때문에 **BeginInvokeOnMainThread** 메서드를 사용 하 여 업데이트를 올바르게 표시 하는지 확인 합니다. 전체 UpdateTimeLabel 메서드는 다음과 같습니다.

```csharp
private void UpdateTimeLabel(object state = null)
{
    Device.BeginInvokeOnMainThread(() =>
        {
            time.Text = DateTime.Now.ToLongTimeString();
        }
    );
}
```

### <a name="update-the-current-time-once-every-second"></a>1 초 마다 한 번 현재 시간 업데이트

이제 TimeChangerForms가 시작 된 후 최대 1 초 동안 현재 시간이 정확 하 게 지정 됩니다. 시간을 정확 하 게 유지 하려면 레이블을 정기적으로 업데이트 해야 합니다. **타이머** 개체는 현재 시간으로 레이블을 업데이트 하는 콜백 메서드를 주기적으로 호출 합니다.

```csharp
var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);
```

### <a name="add-houroffset"></a>HourOffset 추가

위쪽 및 아래쪽 단추는 시간을 1 시간 단위로 조정 합니다. **HourOffset** 속성을 추가 하 여 현재 조정을 추적 합니다.

```csharp
public int HourOffset { get; private set; }
```

이제 UpdateTimeLabel 메서드를 업데이트 하 여 HourOffset 속성을 인식 합니다.

```csharp
currentTime.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
```

### <a name="add-button-click-event-handlers"></a>단추 클릭 이벤트 처리기 추가

모든 위쪽 및 아래쪽 단추가 HourOffset 속성을 증가 시키거나 감소 시키고 UpdateTimeLabel를 호출 해야 합니다.

```csharp
private void UpButton_Clicked(object sender, EventArgs e)
{
    HourOffset++;
    UpdateTimeLabel();
}
```

완료 되 면 MainPage.xaml.cs는 다음과 같이 표시 됩니다.

```csharp
using System;
using System.ComponentModel;
using System.Threading;
using Xamarin.Forms;

namespace TimeChangerForms
{
    // Learn more about making custom code visible in the Xamarin.Forms previewer
    // by visiting https://aka.ms/xamarinforms-previewer
    [DesignTimeVisible(false)]
    public partial class MainPage : ContentPage
    {
        public int HourOffset { get; private set; }

        public MainPage()
        {
            InitializeComponent();
        }

        protected override void OnAppearing()
        {
            base.OnAppearing();
            var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);
        }

        private void UpdateTimeLabel(object state = null)
        {
            Device.BeginInvokeOnMainThread(() =>
                {
                    time.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
                }
            );
        }

        private void OnUpButton_Clicked(object sender, EventArgs e)
        {
            HourOffset++;
            UpdateTimeLabel();
        }

        private void OnDownButton_Clicked(object sender, EventArgs e)
        {
            HourOffset--;
            UpdateTimeLabel();
        }
    }
}
```

## <a name="run-the-app"></a>앱 실행

앱을 실행 하려면 **F5** 키를 누르거나 디버그 > 디버깅 시작을 클릭 합니다. [디버거가 구성 된](emulator.md)방식에 따라 장치 또는 에뮬레이터에서 앱이 시작 됩니다.

## <a name="related-links"></a>관련 링크

- [Android 장치 또는 에뮬레이터에서 테스트](emulator.md)합니다.

- [Xamarin Android를 사용 하 여 Android 샘플 앱 만들기](xamarin-android.md)
