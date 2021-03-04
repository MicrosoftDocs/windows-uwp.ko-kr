---
title: Xamarin Android를 사용 하 여 간단한 Android 앱 만들기
description: Android 장치에서 작동 하는 플랫폼 간 앱을 만들기 위해 Windows에서 Xamarin.ios를 사용 하 여 시작 하는 방법에 대 한 단계별 가이드입니다.
author: hickeys
ms.author: hickeys
manager: jken
ms.topic: article
keywords: android, windows, xamarin android, 자습서, xaml
ms.date: 04/28/2020
ms.openlocfilehash: 3bcecf24fe6bb90dc2b94dfa62a5768481b298e5
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101823177"
---
# <a name="get-started-developing-for-android-using-xamarinandroid"></a>Xamarin Android를 사용 하 여 Android 용 개발 시작

이 가이드는 Android 장치에서 작동 하는 플랫폼 간 앱을 만들기 위해 Windows에서 Xamarin.ios를 사용 하 여 시작 하는 데 도움이 됩니다.

이 문서에서는 Xamarin Android 및 Visual Studio 2019을 사용 하 여 간단한 Android 앱을 만듭니다.

## <a name="requirements"></a>요구 사항

이 자습서를 사용 하려면 다음이 필요 합니다.

- 윈도우 10
- [Visual Studio 2019: Community, Professional 또는 Enterprise](https://visualstudio.microsoft.com/downloads/) (참고 참조)
- Visual Studio 2019에 대 한 ".NET을 사용한 모바일 개발" 워크 로드

> [!NOTE]
> 이 가이드는 Visual Studio 2017 또는 2019와 함께 작동 합니다. Visual Studio 2017을 사용 하는 경우 두 버전의 Visual Studio 간 UI 차이로 인해 일부 지침이 올바르지 않을 수 있습니다.

앱을 실행할 Android 휴대폰 또는 구성 된 에뮬레이터를 사용할 수도 있습니다. [Android 에뮬레이터 구성](emulator.md)을 참조 하세요.

## <a name="create-a-new-xamarinandroid-project"></a>새 Xamarin.Android 프로젝트 만들기

Visual Studio를 시작합니다. 파일 > 새 > 프로젝트를 선택 하 여 새 프로젝트를 만듭니다.

새 프로젝트 대화 상자에서 **Android 앱 (Xamarin)** 템플릿을 선택 하 고 **다음** 을 클릭 합니다.

프로젝트 이름을 **TimeChangerAndroid** 로 선택 하 고 **만들기** 를 클릭 합니다.

새 플랫폼 간 앱 대화 상자에서 비어 있는 **앱** 을 선택 합니다. **최소 Android 버전** 에서 **Android 5.0 (롤리팝)** 을 선택 합니다. **확인** 을 클릭합니다.

Xamarin은 **TimeChangerAndroid** 이라는 단일 프로젝트를 사용 하 여 새 솔루션을 만듭니다.

## <a name="create-a-ui-with-xaml"></a>XAML을 사용 하 여 UI 만들기

프로젝트의 **Resources\layout** 디렉터리에서 **activity_main.xml** 를 엽니다. 이 파일의 XML은 TimeChanger를 열 때 사용자에 게 표시 되는 첫 번째 화면을 정의 합니다.

TimeChanger UI는 단순 합니다. 현재 시간을 표시 하 고 1 시간 단위로 시간을 조정 하는 단추를 포함 합니다. 세로를 사용 하 여 `LinearLayout` 단추 위에 있는 시간을 맞추고 가로를 사용 하 여 단추를 나란히 `LinearLayout` 정렬 합니다. 콘텐츠는 **android: 중력** 특성을 세로로 **가운데 맞춤** 으로 설정 하 여 화면 가운데에 배치 됩니다 `LinearLayout` .

**activity_main.xml** 내용을 다음 코드로 바꿉니다.

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="At runtime, I will display current time"
        android:id="@+id/timeDisplay"
    />
    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal">
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Up"
            android:id="@+id/upButton"/>
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Down"
            android:id="@+id/downButton"/>
    </LinearLayout>
</LinearLayout>
```

이 시점에서 **TimeChangerAndroid** 를 실행 하 고 만든 UI를 볼 수 있습니다. 다음 섹션에서는 현재 시간을 표시 하는 UI에 기능을 추가 하 고 단추를 사용 하 여 작업을 수행할 수 있도록 합니다.

## <a name="add-logic-code-with-c"></a>C를 사용 하 여 논리 코드 추가 #

**MainActivity.cs** 를 엽니다. 이 파일에는 UI에 기능을 추가 하는 코드 숨김이 포함 되어 있습니다.

### <a name="set-the-current-time"></a>현재 시간 설정

먼저 시간이 표시 되는에 대 한 참조를 가져옵니다 `TextView` . **FindViewById** 를 사용 하 여 이전 단계의 xml에서로 설정 된 올바른 **android: id** 를 사용 하 여 모든 UI 요소를 검색 합니다 `"@+id/timeDisplay"` . `TextView`현재 시간을 표시 하는입니다.

```csharp
var timeDisplay = FindViewById<TextView>(Resource.Id.timeDisplay);
```

Ui 스레드에서 ui 컨트롤을 업데이트 해야 합니다. 다른 스레드에서 변경한 내용이 화면에 표시 되 면 해당 컨트롤을 제대로 업데이트 하지 못할 수 있습니다. 이 코드는 항상 UI 스레드에서 실행 되는 것은 아닙니다. **Runonuithread** 메서드를 사용 하 여 업데이트가 제대로 표시 되는지 확인 합니다. 전체 메서드는 다음과 같습니다 `UpdateTimeLabel` .

```csharp
private void UpdateTimeLabel(object state = null)
{
    RunOnUiThread(() =>
    {
        TimeDisplay.Text = DateTime.Now.ToLongTimeString();
    });
}
```

### <a name="update-the-current-time-once-every-second"></a>1 초 마다 한 번 현재 시간 업데이트

이제 TimeChangerAndroid가 시작 된 후 최대 1 초 동안 현재 시간이 정확 하 게 지정 됩니다. 시간을 정확 하 게 유지 하려면 레이블을 정기적으로 업데이트 해야 합니다. **타이머** 개체는 현재 시간으로 레이블을 업데이트 하는 콜백 메서드를 주기적으로 호출 합니다.

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
TimeDisplay.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
```

### <a name="create-the-button-click-event-handlers"></a>단추 클릭 이벤트 처리기 만들기

모든 위쪽 및 아래쪽 단추가 HourOffset 속성을 증가 시키거나 감소 시키고 UpdateTimeLabel를 호출 해야 합니다.

```csharp
public void UpButton_Click(object sender, System.EventArgs e)
{
    HourOffset++;
    UpdateTimeLabel();
}
```

### <a name="wire-up-the-up-and-down-buttons-to-their-corresponding-event-handlers"></a>해당 이벤트 처리기에 위쪽 및 아래쪽 단추를 연결 합니다.

단추를 해당 이벤트 처리기와 연결 하려면 먼저 FindViewById를 사용 하 여 해당 id로 단추를 찾습니다. Button 개체에 대 한 참조를 만들었으면 이벤트에 이벤트 처리기를 추가할 수 있습니다 `Click` .

```csharp
Button upButton = FindViewById<Button>(Resource.Id.upButton);
upButton.Click += UpButton_Click;
```

## <a name="completed-mainactivitycs-file"></a>완료 된 MainActivity.cs 파일

완료 되 면 MainActivity.cs는 다음과 같이 표시 됩니다.

```csharp
using Android.App;
using Android.OS;
using Android.Support.V7.App;
using Android.Runtime;
using Android.Widget;
using System;
using System.Threading;

namespace TimeChangerAndroid
{
    [Activity(Label = "@string/app_name", Theme = "@style/AppTheme", MainLauncher = true)]
    public class MainActivity : AppCompatActivity
    {
        public TextView TimeDisplay { get; private set; }
        public int HourOffset { get; private set; }

        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);

            // Set the view from the "main" layout resource
            SetContentView(Resource.Layout.activity_main);

            var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);

            Button upButton = FindViewById<Button>(Resource.Id.upButton);
            upButton.Click += OnUpButton_Click;

            Button downButton = FindViewById<Button>(Resource.Id.downButton);
            downButton.Click += OnDownButton_Click;

            TimeDisplay = FindViewById<TextView>(Resource.Id.timeDisplay);
        }

        private void UpdateTimeLabel(object state = null)
        {
            // Timer callbacks run on a background thread, but UI updates must run on the UI thread.
            RunOnUiThread(() =>
            {
                TimeDisplay.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
            });
        }

        public void OnUpButton_Click(object sender, System.EventArgs e)
        {
            HourOffset++;
            UpdateTimeLabel();
        }

        public void OnDownButton_Click(object sender, System.EventArgs e)
        {
            HourOffset--;
            UpdateTimeLabel();
        }
    }
}
```

## <a name="run-your-app"></a>앱을 실행합니다.

앱을 실행 하려면 **F5** 키를 누르거나 디버그 > 디버깅 시작을 클릭 합니다. [디버거가 구성 된](emulator.md)방식에 따라 장치 또는 에뮬레이터에서 앱이 시작 됩니다.

## <a name="related-links"></a>관련 링크

- [Android 장치 또는 에뮬레이터에서 테스트](emulator.md)합니다.
- [Xamarin.ios를 사용 하 여 Android 샘플 앱 만들기](xamarin-forms.md)
