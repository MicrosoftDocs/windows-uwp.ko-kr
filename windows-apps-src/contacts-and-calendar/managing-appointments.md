---
description: Windows.ApplicationModel.Appointments 네임스페이스를 통해 사용자의 일정 앱에서 약속을 만들고 관리할 수 있습니다.
title: 약속 관리
ms.assetid: 292E9249-07C3-4791-B32C-6EC153C2B538
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 약속, 일정
ms.localizationpriority: medium
ms.openlocfilehash: 8fc8bbaf16e1fc4b3372b9884e1b9bd6817a2f18
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166297"
---
# <a name="manage-appointments"></a>약속 관리



[  **Windows.ApplicationModel.Appointments**](/uwp/api/Windows.ApplicationModel.Appointments) 네임스페이스를 통해 사용자의 일정 앱에서 약속을 만들고 관리할 수 있습니다. 여기서는 약속을 만들고, 약속을 일정 앱에 추가하고, 일정 앱에서 약속을 바꾸고, 일정 앱에서 약속을 제거하는 방법을 설명하겠습니다. 또한 일정 앱의 시간 범위를 표시하고 약속 되풀이 개체를 만드는 방법도 설명합니다.

## <a name="create-an-appointment-and-apply-data-to-it"></a>약속을 만들고 데이터를 적용 합니다.

[**Windows.**](/uwp/api/Windows.ApplicationModel.Appointments.Appointment) r e s e r e. a s o. r o s 개체를 만들어 변수에 할당 합니다. 그런 다음 사용자가 UI를 통해 제공 된 약속 속성을 **약속** 에 적용 합니다.

```cs
private void Create-Click(object sender, RoutedEventArgs e)
{
    bool isAppointmentValid = true;
    var appointment = new Windows.ApplicationModel.Appointments.Appointment();

    // StartTime
    var date = StartTimeDatePicker.Date;
    var time = StartTimeTimePicker.Time;
    var timeZoneOffset = TimeZoneInfo.Local.GetUtcOffset(DateTime.Now);
    var startTime = new DateTimeOffset(date.Year, date.Month, date.Day, time.Hours, time.Minutes, 0, timeZoneOffset);
    appointment.StartTime = startTime;

    // Subject
    appointment.Subject = SubjectTextBox.Text;

    if (appointment.Subject.Length > 255)
    {
        isAppointmentValid = false;
        ResultTextBlock.Text = "The subject cannot be greater than 255 characters.";
    }

    // Location
    appointment.Location = LocationTextBox.Text;

    if (appointment.Location.Length > 32768)
    {
        isAppointmentValid = false;
        ResultTextBlock.Text = "The location cannot be greater than 32,768 characters.";
    }

    // Details
    appointment.Details = DetailsTextBox.Text;

    if (appointment.Details.Length > 1073741823)
    {
        isAppointmentValid = false;
        ResultTextBlock.Text = "The details cannot be greater than 1,073,741,823 characters.";
    }

    // Duration
    if (DurationComboBox.SelectedIndex == 0)
    {
        // 30 minute duration is selected
        appointment.Duration = TimeSpan.FromMinutes(30);
    }
    else
    {
        // 1 hour duration is selected
        appointment.Duration = TimeSpan.FromHours(1);
    }

    // All Day
    appointment.AllDay = AllDayCheckBox.IsChecked.Value;

    // Reminder
    if (ReminderCheckBox.IsChecked.Value)
    {
        switch (ReminderComboBox.SelectedIndex)
        {
            case 0:
                appointment.Reminder = TimeSpan.FromMinutes(15);
                break;
            case 1:
                appointment.Reminder = TimeSpan.FromHours(1);
                break;
            case 2:
                appointment.Reminder = TimeSpan.FromDays(1);
                break;
        }
    }

    //Busy Status
    switch (BusyStatusComboBox.SelectedIndex)
    {
        case 0:
            appointment.BusyStatus = Windows.ApplicationModel.Appointments.AppointmentBusyStatus.Busy;
            break;
        case 1:
            appointment.BusyStatus = Windows.ApplicationModel.Appointments.AppointmentBusyStatus.Tentative;
            break;
        case 2:
            appointment.BusyStatus = Windows.ApplicationModel.Appointments.AppointmentBusyStatus.Free;
            break;
        case 3:
            appointment.BusyStatus = Windows.ApplicationModel.Appointments.AppointmentBusyStatus.OutOfOffice;
            break;
        case 4:
            appointment.BusyStatus = Windows.ApplicationModel.Appointments.AppointmentBusyStatus.WorkingElsewhere;
            break;
    }

    // Sensitivity
    switch (SensitivityComboBox.SelectedIndex)
    {
        case 0:
            appointment.Sensitivity = Windows.ApplicationModel.Appointments.AppointmentSensitivity.Public;
            break;
        case 1:
            appointment.Sensitivity = Windows.ApplicationModel.Appointments.AppointmentSensitivity.Private;
            break;
    }

    // Uri
    if (UriTextBox.Text.Length > 0)
    {
        try
        {
            appointment.Uri = new System.Uri(UriTextBox.Text);
        }
        catch (Exception)
        {
            isAppointmentValid = false;
            ResultTextBlock.Text = "The Uri provided is invalid.";
        }
    }

    // Organizer
    // Note: Organizer can only be set if there are no invitees added to this appointment.
    if (OrganizerRadioButton.IsChecked.Value)
    {
        var organizer = new Windows.ApplicationModel.Appointments.AppointmentOrganizer();

        // Organizer Display Name
        organizer.DisplayName = OrganizerDisplayNameTextBox.Text;

        if (organizer.DisplayName.Length > 256)
        {
            isAppointmentValid = false;
            ResultTextBlock.Text = "The organizer display name cannot be greater than 256 characters.";
        }
        else
        {
            // Organizer Address (for example, Email Address)
            organizer.Address = OrganizerAddressTextBox.Text;

            if (organizer.Address.Length > 321)
            {
                isAppointmentValid = false;
                ResultTextBlock.Text = "The organizer address cannot be greater than 321 characters.";
            }
            else if (organizer.Address.Length == 0)
            {
                isAppointmentValid = false;
                ResultTextBlock.Text = "The organizer address must be greater than 0 characters.";
            }
            else
            {
                appointment.Organizer = organizer;
            }
        }
    }

    // Invitees
    // Note: If the size of the Invitees list is not zero, then an Organizer cannot be set.
    if (InviteeRadioButton.IsChecked.Value)
    {
        var invitee = new Windows.ApplicationModel.Appointments.AppointmentInvitee();

        // Invitee Display Name
        invitee.DisplayName = InviteeDisplayNameTextBox.Text;

        if (invitee.DisplayName.Length > 256)
        {
            isAppointmentValid = false;
            ResultTextBlock.Text = "The invitee display name cannot be greater than 256 characters.";
        }
        else
        {
            // Invitee Address (for example, Email Address)
            invitee.Address = InviteeAddressTextBox.Text;

            if (invitee.Address.Length > 321)
            {
                isAppointmentValid = false;
                ResultTextBlock.Text = "The invitee address cannot be greater than 321 characters.";
            }
            else if (invitee.Address.Length == 0)
            {
                isAppointmentValid = false;
                ResultTextBlock.Text = "The invitee address must be greater than 0 characters.";
            }
            else
            {
                // Invitee Role
                switch (RoleComboBox.SelectedIndex)
                {
                    case 0:
                        invitee.Role = Windows.ApplicationModel.Appointments.AppointmentParticipantRole.RequiredAttendee;
                        break;
                    case 1:
                        invitee.Role = Windows.ApplicationModel.Appointments.AppointmentParticipantRole.OptionalAttendee;
                        break;
                    case 2:
                        invitee.Role = Windows.ApplicationModel.Appointments.AppointmentParticipantRole.Resource;
                        break;
                }

                // Invitee Response
                switch (ResponseComboBox.SelectedIndex)
                {
                    case 0:
                        invitee.Response = Windows.ApplicationModel.Appointments.AppointmentParticipantResponse.None;
                        break;
                    case 1:
                        invitee.Response = Windows.ApplicationModel.Appointments.AppointmentParticipantResponse.Tentative;
                        break;
                    case 2:
                        invitee.Response = Windows.ApplicationModel.Appointments.AppointmentParticipantResponse.Accepted;
                        break;
                    case 3:
                        invitee.Response = Windows.ApplicationModel.Appointments.AppointmentParticipantResponse.Declined;
                        break;
                    case 4:
                        invitee.Response = Windows.ApplicationModel.Appointments.AppointmentParticipantResponse.Unknown;
                        break;
                }

                appointment.Invitees.Add(invitee);
            }
        }
    }

    if (isAppointmentValid)
    {
        ResultTextBlock.Text = "The appointment was created successfully and is valid.";
    }
}
```

## <a name="add-an-appointment-to-the-users-calendar"></a>사용자의 일정에 약속을 추가 합니다.

[**Windows.**](/uwp/api/Windows.ApplicationModel.Appointments.Appointment) r e s e r e. a s o. r o s 개체를 만들어 변수에 할당 합니다. 그런 다음 [**ShowAddAppointmentAsync (AppointmentManager (약속, Rect, Placement)**](/uwp/api/windows.applicationmodel.appointments.appointmentmanager.showaddappointmentasync) 메서드를 호출 하 여 기본 약속 공급자 추가 약속 UI를 표시 합니다. 그러면 사용자가 약속을 추가할 수 있습니다. 사용자가 **추가**를 클릭 한 경우이 샘플은 **ShowAddAppointmentAsync** 에서 반환 하는 약속 식별자를 출력 합니다.

```cs
private async void Add-Click(object sender, RoutedEventArgs e)
{
    // Create an Appointment that should be added the user's appointments provider app.
    var appointment = new Windows.ApplicationModel.Appointments.Appointment();

    // Get the selection rect of the button pressed to add this appointment
    var rect = GetElementRect(sender as FrameworkElement);

    // ShowAddAppointmentAsync returns an appointment id if the appointment given was added to the user's calendar.
    // This value should be stored in app data and roamed so that the appointment can be replaced or removed in the future.
    // An empty string return value indicates that the user canceled the operation before the appointment was added.
    String appointmentId = await Windows.ApplicationModel.Appointments.AppointmentManager.ShowAddAppointmentAsync(
                           appointment, rect, Windows.UI.Popups.Placement.Default);
    if (appointmentId != String.Empty)
    {
        ResultTextBlock.Text = "Appointment Id: " + appointmentId;
    }
    else
    {
        ResultTextBlock.Text = "Appointment not added.";
    }
}
```

**참고**    Windows Phone 스토어 앱의 경우 약속을 추가 하기 위해 표시 되는 대화 상자를 편집할 수 있는 [**ShowEditNewAppointment**](/uwp/api/windows.applicationmodel.appointments.appointmentmanager.showeditnewappointmentasync) 와 마찬가지로 [**showaddappointment**](/uwp/api/windows.applicationmodel.appointments.appointmentmanager.showaddappointmentasync) 작동 합니다.

## <a name="replace-an-appointment-in-the-users-calendar"></a>사용자의 일정에서 약속 바꾸기

[**Windows.**](/uwp/api/Windows.ApplicationModel.Appointments.Appointment) r e s e r e. a s o. r o s 개체를 만들어 변수에 할당 합니다. 그런 다음 적절 한 [**AppointmentManager ShowReplaceAppointmentAsync**](/uwp/api/windows.applicationmodel.appointments.appointmentmanager.showreplaceappointmentasync) 메서드를 호출 하 여 사용자가 약속을 바꿀 수 있도록 하는 기본 약속 공급자 대체 약속 UI를 표시 합니다. 또한 사용자는 바꾸려는 약속 식별자를 제공 합니다. 이 식별자는 AppointmentManager에서 반환 되었습니다 [**. ShowAddAppointmentAsync**](/uwp/api/windows.applicationmodel.appointments.appointmentmanager.showaddappointmentasync). 사용자가 **바꾸기**를 클릭 한 경우 샘플은 해당 약속 식별자를 업데이트 한 것으로 인쇄 합니다.

```cs
private async void Replace-Click(object sender, RoutedEventArgs e)
{
    // The appointment id argument for ReplaceAppointmentAsync is typically retrieved from AddAppointmentAsync and stored in app data.
    String appointmentIdOfAppointmentToReplace = AppointmentIdTextBox.Text;

    if (String.IsNullOrEmpty(appointmentIdOfAppointmentToReplace))
    {
        ResultTextBlock.Text = "The appointment id cannot be empty";
    }
    else
    {
        // The Appointment argument for ReplaceAppointmentAsync should contain all of the Appointment' s properties including those that may have changed.
        var appointment = new Windows.ApplicationModel.Appointments.Appointment();

        // Get the selection rect of the button pressed to replace this appointment
        var rect = GetElementRect(sender as FrameworkElement);

        // ReplaceAppointmentAsync returns an updated appointment id when the appointment was successfully replaced.
        // The updated id may or may not be the same as the original one retrieved from AddAppointmentAsync.
        // An optional instance start time can be provided to indicate that a specific instance on that date should be replaced
        // in the case of a recurring appointment.
        // If the appointment id returned is an empty string, that indicates that the appointment was not replaced.
        String updatedAppointmentId;
        if (InstanceStartDateCheckBox.IsChecked.Value)
        {
            // Replace a specific instance starting on the date provided.
            var instanceStartDate = InstanceStartDateDatePicker.Date;
            updatedAppointmentId = await Windows.ApplicationModel.Appointments.AppointmentManager.ShowReplaceAppointmentAsync(
                                   appointmentIdOfAppointmentToReplace, appointment, rect, Windows.UI.Popups.Placement.Default, instanceStartDate);
        }
        else
        {
            // Replace an appointment that occurs only once or in the case of a recurring appointment, replace the entire series.
            updatedAppointmentId = await Windows.ApplicationModel.Appointments.AppointmentManager.ShowReplaceAppointmentAsync(
                                   appointmentIdOfAppointmentToReplace, appointment, rect, Windows.UI.Popups.Placement.Default);
        }

        if (updatedAppointmentId != String.Empty)
        {
            ResultTextBlock.Text = "Updated Appointment Id: " + updatedAppointmentId;
        }
        else
        {
            ResultTextBlock.Text = "Appointment not replaced.";
        }
    }
}
```

## <a name="remove-an-appointment-from-the-users-calendar"></a>사용자의 일정에서 약속 제거

사용자가 약속을 제거할 수 있도록 하려면 적절 한 ShowRemoveAppointmentAsync 메서드를 호출 하 여 기본 약속 공급자 [**APPOINTMENTMANAGER**](/uwp/api/windows.applicationmodel.appointments.appointmentmanager.showremoveappointmentasync) UI를 표시 합니다. 또한 사용자는 제거 하고자 하는 약속 식별자를 제공 합니다. 이 식별자는 AppointmentManager에서 반환 되었습니다 [**. ShowAddAppointmentAsync**](/uwp/api/windows.applicationmodel.appointments.appointmentmanager.showaddappointmentasync). 사용자가 **삭제**를 클릭 한 경우 샘플은 해당 약속 식별자로 지정 된 약속이 제거 되었음을 출력 합니다.

```cs
private async void Remove-Click(object sender, RoutedEventArgs e)
{
    // The appointment id argument for ShowRemoveAppointmentAsync is typically retrieved from AddAppointmentAsync and stored in app data.
    String appointmentId = AppointmentIdTextBox.Text;

    // The appointment id cannot be null or empty.
    if (String.IsNullOrEmpty(appointmentId))
    {
        ResultTextBlock.Text = "The appointment id cannot be empty";
    }
    else
    {
        // Get the selection rect of the button pressed to remove this appointment
        var rect = GetElementRect(sender as FrameworkElement);

        // ShowRemoveAppointmentAsync returns a boolean indicating whether or not the appointment related to the appointment id given was removed.
        // An optional instance start time can be provided to indicate that a specific instance on that date should be removed
        // in the case of a recurring appointment.
        bool removed;
        if (InstanceStartDateCheckBox.IsChecked.Value)
        {
            // Remove a specific instance starting on the date provided.
            var instanceStartDate = InstanceStartDateDatePicker.Date;
            removed = await Windows.ApplicationModel.Appointments.AppointmentManager.ShowRemoveAppointmentAsync(
                      appointmentId, rect, Windows.UI.Popups.Placement.Default, instanceStartDate);
        }
        else
        {
            // Remove an appointment that occurs only once or in the case of a recurring appointment, replace the entire series.
            removed = await Windows.ApplicationModel.Appointments.AppointmentManager.ShowRemoveAppointmentAsync(
                      appointmentId, rect, Windows.UI.Popups.Placement.Default);
        }

        if (removed)
        {
            ResultTextBlock.Text = "Appointment removed";
        }
        else
        {
            ResultTextBlock.Text = "Appointment not removed";
        }
    }
}
```

## <a name="show-a-time-span-for-the-appointments-provider"></a>약속 공급자의 시간 범위 표시

사용자가 **표시**를 클릭 한 경우 [**AppointmentManager**](/uwp/api/windows.applicationmodel.appointments.appointmentmanager.showtimeframeasync) 메서드를 호출 하 여 기본 약속 공급자의 기본 UI에 대 한 특정 시간 범위를 표시 합니다. 이 샘플에서는 기본 약속 공급자가 화면에 표시 된 것을 인쇄 합니다.

```cs
private async void Show-Click(object sender, RoutedEventArgs e)
{
    var dateToShow = new DateTimeOffset(2015, 6, 12, 18, 32, 0, 0, TimeSpan.FromHours(-8));
    var duration = TimeSpan.FromHours(1);
    await Windows.ApplicationModel.Appointments.AppointmentManager.ShowTimeFrameAsync(dateToShow, duration);
    ResultTextBlock.Text = "The default appointments provider should have appeared on screen.";
}
```

## <a name="create-an-appointment-recurrence-object-and-apply-data-to-it"></a>약속-되풀이 개체를 만들고 데이터를 적용 합니다.

[**AppointmentRecurrence**](/uwp/api/windows.applicationmodel.appointments.appointmentrecurrence) 개체를 만들고이 개체를 변수에 할당 합니다. 그런 다음 사용자가 UI를 통해 제공 된 되풀이 속성을 **AppointmentRecurrence** 에 적용 합니다.

```cs
private void Create-Click(object sender, RoutedEventArgs e)
{
    bool isRecurrenceValid = true;
    var recurrence = new Windows.ApplicationModel.Appointments.AppointmentRecurrence();

    // Unit
    switch (UnitComboBox.SelectedIndex)
    {
        case 0:
            recurrence.Unit = Windows.ApplicationModel.Appointments.AppointmentRecurrenceUnit.Daily;
            break;
        case 1:
            recurrence.Unit = Windows.ApplicationModel.Appointments.AppointmentRecurrenceUnit.Weekly;
            break;
        case 2:
            recurrence.Unit = Windows.ApplicationModel.Appointments.AppointmentRecurrenceUnit.Monthly;
            break;
        case 3:
            recurrence.Unit = Windows.ApplicationModel.Appointments.AppointmentRecurrenceUnit.MonthlyOnDay;
            break;
        case 4:
            recurrence.Unit = Windows.ApplicationModel.Appointments.AppointmentRecurrenceUnit.Yearly;
            break;
        case 5:
            recurrence.Unit = Windows.ApplicationModel.Appointments.AppointmentRecurrenceUnit.YearlyOnDay;
            break;
    }

    // Occurrences
    // Note: Occurrences and Until properties are mutually exclusive.
    if (OccurrencesRadioButton.IsChecked.Value)
    {
        recurrence.Occurrences = (uint)OccurrencesSlider.Value;
    }

    // Until
    // Note: Until and Occurrences properties are mutually exclusive.
    if (UntilRadioButton.IsChecked.Value)
    {
        recurrence.Until = UntilDatePicker.Date;
    }

    // Interval
    recurrence.Interval = (uint)IntervalSlider.Value;

    // Week of the month
    switch (WeekOfMonthComboBox.SelectedIndex)
    {
        case 0:
            recurrence.WeekOfMonth = Windows.ApplicationModel.Appointments.AppointmentWeekOfMonth.First;
            break;
        case 1:
            recurrence.WeekOfMonth = Windows.ApplicationModel.Appointments.AppointmentWeekOfMonth.Second;
            break;
        case 2:
            recurrence.WeekOfMonth = Windows.ApplicationModel.Appointments.AppointmentWeekOfMonth.Third;
            break;
        case 3:
            recurrence.WeekOfMonth = Windows.ApplicationModel.Appointments.AppointmentWeekOfMonth.Fourth;
            break;
        case 4:
            recurrence.WeekOfMonth = Windows.ApplicationModel.Appointments.AppointmentWeekOfMonth.Last;
            break;
    }

    // Days of the Week
    // Note: For Weekly, MonthlyOnDay or YearlyOnDay recurrence unit values, at least one day must be specified.
    if (SundayCheckBox.IsChecked.Value) { recurrence.DaysOfWeek |= Windows.ApplicationModel.Appointments.AppointmentDaysOfWeek.Sunday; }
    if (MondayCheckBox.IsChecked.Value) { recurrence.DaysOfWeek |= Windows.ApplicationModel.Appointments.AppointmentDaysOfWeek.Monday; }
    if (TuesdayCheckBox.IsChecked.Value) { recurrence.DaysOfWeek |= Windows.ApplicationModel.Appointments.AppointmentDaysOfWeek.Tuesday; }
    if (WednesdayCheckBox.IsChecked.Value) { recurrence.DaysOfWeek |= Windows.ApplicationModel.Appointments.AppointmentDaysOfWeek.Wednesday; }
    if (ThursdayCheckBox.IsChecked.Value) { recurrence.DaysOfWeek |= Windows.ApplicationModel.Appointments.AppointmentDaysOfWeek.Thursday; }
    if (FridayCheckBox.IsChecked.Value) { recurrence.DaysOfWeek |= Windows.ApplicationModel.Appointments.AppointmentDaysOfWeek.Friday; }
    if (SaturdayCheckBox.IsChecked.Value) { recurrence.DaysOfWeek |= Windows.ApplicationModel.Appointments.AppointmentDaysOfWeek.Saturday; }

    if (((recurrence.Unit == Windows.ApplicationModel.Appointments.AppointmentRecurrenceUnit.Weekly) ||
         (recurrence.Unit == Windows.ApplicationModel.Appointments.AppointmentRecurrenceUnit.MonthlyOnDay) ||
         (recurrence.Unit == Windows.ApplicationModel.Appointments.AppointmentRecurrenceUnit.YearlyOnDay)) &&
        (recurrence.DaysOfWeek == Windows.ApplicationModel.Appointments.AppointmentDaysOfWeek.None))
    {
        isRecurrenceValid = false;
        ResultTextBlock.Text = "The recurrence specified is invalid. For Weekly, MonthlyOnDay or YearlyOnDay recurrence unit values, " +
                               "at least one day must be specified.";
    }

    // Month of the year
    recurrence.Month = (uint)MonthSlider.Value;

    // Day of the month
    recurrence.Day = (uint)DaySlider.Value;

    if (isRecurrenceValid)
    {
        ResultTextBlock.Text = "The recurrence specified was created successfully and is valid.";
    }
}
```

## <a name="add-a-new-editable-appointment"></a>새 편집 가능한 약속 추가

[**ShowEditNewAppointmentAsync**](/uwp/api/windows.applicationmodel.appointments.appointmentmanager.showeditnewappointmentasync) 는 사용자가 약속 데이터를 저장 하기 전에 수정할 수 있도록 약속을 추가 하는 대화 상자가 편집 가능 하다는 점을 제외 하 고는 [**ShowAddAppointmentAsync**](/uwp/api/windows.applicationmodel.appointments.appointmentmanager.showaddappointmentasync) 와 똑같이 작동 합니다.

``` cs
private async void AddAndEdit-Click(object sender, RoutedEventArgs e)
{
    // Create an Appointment that should be added the user' s appointments provider app.
    var appointment = new Windows.ApplicationModel.Appointments.Appointment();

    appointment.StartTime = DateTime.Now + TimeSpan.FromDays(1);
    appointment.Duration = TimeSpan.FromHours(1);
    appointment.Location = "Meeting location";
    appointment.Subject = "Meeting subject";
    appointment.Details = "Meeting description";
    appointment.Reminder = TimeSpan.FromMinutes(15); // Remind me 15 minutes prior


    // ShowAddAppointmentAsync returns an appointment id if the appointment given was added to the user' s calendar.
    // This value should be stored in app data and roamed so that the appointment can be replaced or removed in the future.
    // An empty string return value indicates that the user canceled the operation before the appointment was added.
    String appointmentId =
        await Windows.ApplicationModel.Appointments.AppointmentManager.ShowEditNewAppointmentAsync(appointment);

    if (appointmentId != String.Empty)
    {
        ResultTextBlock.Text = "Appointment Id: " + appointmentId;
    }
    else
    {
        ResultTextBlock.Text = "Appointment not added.";
    }
}
```

## <a name="show-appointment-details"></a>약속 정보 표시

[**ShowAppointmentDetailsAsync**](/uwp/api/windows.applicationmodel.appointments.appointmentmanager.showappointmentdetailsasync) 를 지정 하면 시스템에서 지정 된 약속에 대 한 세부 정보를 표시 합니다. 앱 달력을 구현 하는 앱은 자신이 소유 하는 일정의 약속에 대 한 세부 정보를 표시 하도록 활성화 되도록 선택할 수 있습니다. 그렇지 않으면 시스템에서 약속 세부 정보를 표시 합니다. 되풀이 약속의 인스턴스에 대 한 세부 정보를 표시 하기 위해 시작 날짜 인수를 허용 하는 메서드의 오버 로드가 제공 됩니다.

```cs
private async void ShowAppointmentDetails-Click(object sender, RoutedEventArgs e)
{

    if (instanceStartTime == null)
    {
        await Windows.ApplicationModel.Appointments.AppointmentManager.ShowAppointmentDetailsAsync(
            currentAppointment.LocalId);
    }
    else
    {
        // Specify a start time to show an instance of a recurring appointment
        await Windows.ApplicationModel.Appointments.AppointmentManager.ShowAppointmentDetailsAsync(
            currentAppointment.LocalId, instanceStartTime);
    }

}
```

## <a name="summary-and-next-steps"></a>요약 및 다음 단계

이제 약속을 관리 하는 방법에 대해 기본적으로 이해 하 고 있습니다. GitHub에서 [유니버설 Windows 앱 샘플](https://github.com/Microsoft/Windows-universal-samples) 을 다운로드 하 여 약속을 관리 하는 방법에 대 한 더 많은 예제를 확인 합니다.

## <a name="related-topics"></a>관련 항목

* [약속 API 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Appointments)
 

 