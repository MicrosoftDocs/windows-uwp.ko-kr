---
description: 이 항목에서는 유니버설 Windows 플랫폼 (UWP)에 해당 하는 Windows Phone Silverlight Api의 포괄적인 매핑을 제공합니다.
title: Windows Phone Silverlight를 UWP에 네임 스페이스 및 클래스 매핑
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 649062c8d1901a7b0f24a69378e13a7725d7c84c
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67322322"
---
# <a name="windowsphone-silverlight-to-uwp-api-mappings"></a>Windows Phone Silverlight를 UWP API에 매핑


이 항목에서는 유니버설 Windows 플랫폼 (UWP)에 해당 하는 Windows Phone Silverlight Api의 포괄적인 매핑을 제공합니다. 일반적으로는 기능이 일대일로 매핑되지는 않지만 각 플랫폼의 네임스페이스 또는 클래스에 비슷한 기능이 있을 수 있습니다.

매핑 테이블 UWP 프로젝트에서 작업할 때 및 Windows Phone Silverlight 프로젝트에서 소스 코드 다시 사용 하는 경우에 도움이 됩니다. 두 플랫폼 간에는 네임스페이스와 클래스의 이름(UI 컨트롤 포함)이 다릅니다. 대부분의 경우 간단히 네임스페이스 이름을 변경하면 코드가 컴파일됩니다. 클래스 또는 API 이름이 네임스페이스 이름과 함께 변경된 경우도 있습니다. 매핑이 더 복잡하여 접근 방법을 변경해야 하는 경우도 있습니다.

**테이블을 사용 하는 방법:  ** 첫 번째를 사용 하는 클래스의 이름 검색 합니다. 매핑이 네임스페이스 이름을 변경하는 것보다 더 복잡할 경우 클래스가 나열됩니다. 클래스가 나열되지 않는 경우 매핑 시 네임스페이스만 변경하면 됩니다. 따라서 클래스의 네임스페이스 이름을 찾고 해당하는 UWP 네임스페이스 이름을 찾습니다. 클래스가 해당 네임스페이스에 있습니다. 네임스페이스가 나열되지 않는 경우 이름이 변경되지 않은 것입니다.

**참고**  Windows 10은 훨씬 더 많은.NET Framework의 Windows Phone 스토어 앱을 보다 지원 합니다. 예를 들어, Windows 10에는 여러 System.ServiceModel에 있습니다. \* 네임 스페이스 System.Net.Sockets System.Net, System.Net.NetworkInformation, 및입니다.
또한 Windows 10 앱에서이 좋고.NET 네이티브 코드에서 있는 MSIL을 고유 하 게 실행 가능한 기계어 코드로 변환 하는 미리 컴파일 기술입니다. .NET 네이티브 앱은 해당 MSIL 앱보다 더 빠르게 시작되고 메모리와 배터리를 더 적게 사용합니다.

| Windows Phone Silverlight | Windows 런타임 |
| ------------------------- | --------------- |
| 광고 | |
| **Microsoft.Advertising.Mobile.UI.AdControl** 클래스 | [AdControl](https://docs.microsoft.com/windows/uwp/monetize/display-ads-using-the-microsoft-advertising-libraries) 클래스 |
| 알람, 미리 알림 및 백그라운드 에이전트 | |
| **Microsoft.Phone.BackgroundAgent** 클래스 | [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) class |
| **Microsoft.Phone.Scheduler** 네임스페이스 | [**Windows.ApplicationModel.Background**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background) namespace |
| **Microsoft.Phone.Scheduler.Alarm** 클래스 | [**BackgroundTaskBuilder** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 하 고 [ **ToastNotificationManager** ](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) 클래스 |
| **Microsoft.Phone.Scheduler.PeriodicTask**, **ScheduledAction**, **ScheduledActionService**, **ScheduledTask**, **ScheduledTaskAgent** 클래스 | [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) class |
| **Microsoft.Phone.Scheduler.Reminder** 클래스 | [**BackgroundTaskBuilder** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 하 고 [ **ToastNotificationManager** ](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) 클래스 |
| **Microsoft.Phone.PictureDecoder** 클래스 | [**Bitmapdecoder에서** ](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) 클래스 |
| **Microsoft.Phone.BackgroundAudio** 네임스페이스 | [**Windows.Media.Playback**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback) namespace |
| **Microsoft.Phone.BackgroundTransfer** 네임스페이스 | [**Windows.Networking.BackgroundTransfer**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer) namespace |
| 앱 모델 및 환경 | |
| **System.AppDomain** 클래스 | 직접적으로 해당하는 항목이 없습니다. [  **Application**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Application), [**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication) 클래스를 참조하세요. |
| **System.Environment** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **System.ComponentModel.Annotations** 클래스  | 직접적으로 해당하는 항목이 없습니다. |
| **System.ComponentModel.BackgroundWorker** 클래스 | [**ThreadPool**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool) class |
| **System.ComponentModel.DesignerProperties** 클래스 | [**서** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DesignMode) 클래스 |
| **System.Threading.Thread**, **System.Threading.ThreadPool** 클래스 | [**ThreadPool**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool) class |
| (ST = **System.Threading**) <br/> **ST.Thread.MemoryBarrier** 메서드 | (ST = **System.Threading**) <br/> **ST.Interlocked.MemoryBarrier** 메서드 |
| (ST = **System.Threading**) <br/> **ST.Thread.ManagedThreadId** 속성 | (S = **System**) <br/> **S.Environment.ManagedThreadId** 속성 |
| **System.Threading.Timer** 클래스 | [**ThreadPoolTimer**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPoolTimer) class |
| (SWT = **System.Windows.Threading**) <br/> **SWT.Dispatcher** 클래스 | [**CoreDispatcher** ](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) 클래스 |
| (SWT = **System.Windows.Threading**) <br/> **SWT.DispatcherTimer** 클래스 | [**DispatcherTimer** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DispatcherTimer) 클래스 |
| Visual Studio용 Blend | |
| (MEDC = **Microsoft.Expression.Drawing.Core**) <br/> **MEDC.GeometryHelper** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Expression.Interactivity** 네임스페이스 | [Microsoft.Xaml.Interactivity](https://go.microsoft.com/fwlink/p/?LinkId=328776) 네임스페이스 |
| **Microsoft.Expression.Interactivity.Core** 네임스페이스 | [Microsoft.Xaml.Interactions.Core](https://go.microsoft.com/fwlink/p/?LinkId=328773) 네임스페이스 |
| (MEIC = **Microsoft.Expression.Interactivity.Core**) <br/> **MEIC.ExtendedVisualStateManager** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Expression.Interactivity.Input** 네임스페이스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Expression.Interactivity.Media** 네임스페이스 | [Microsoft.Xaml.Interactions.Media](https://go.microsoft.com/fwlink/p/?LinkId=328775) 네임스페이스 |
| **Microsoft.Expression.Shapes** 네임스페이스 | 직접적으로 해당하는 항목이 없습니다. |
| (MI = **Microsoft.Internal**) <br/> **MI.IManagedFrameworkInternalHelper** 인터페이스 | 직접적으로 해당하는 항목이 없습니다. |
| 연락처 및 일정 데이터 | |
| **Microsoft.Phone.UserData** 네임스페이스 | [**Windows.ApplicationModel.Contacts**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts), [**Windows.ApplicationModel.Appointments**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments) namespaces |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Account**, **ContactAddress**, **ContactCompanyInformation**, **ContactEmailAddress**, **ContactPhoneNumber** 클래스 | [**연락처** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact) 클래스 |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Appointments** 클래스 | [**AppointmentCalendar** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments.AppointmentCalendar) 클래스 |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Contacts** 클래스 | [**ContactStore** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.ContactStore) 클래스 |
| 컨트롤 및 UI 인프라 | |
| **ControlTiltEffect.TiltEffect** 클래스 | Windows 런타임 애니메이션 라이브러리의 애니메이션은 공용 컨트롤의 기본 스타일로 기본 제공됩니다. [애니메이션](wpsl-to-uwp-porting-xaml-and-ui.md)을 참조하세요. |
| **Microsoft.Phone.Controls** 네임스페이스 | [**Windows.UI.Xaml.Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls) namespace |
| (MPC = **Microsoft.Phone.Controls**) <br/> **MPC.ContextMenu** 클래스 | [**팝업 메뉴** ](https://docs.microsoft.com/uwp/api/Windows.UI.Popups.PopupMenu) 클래스 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.DatePickerPage** 클래스 | [**DatePickerFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePickerFlyout) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.GestureListener** 클래스 | [**GestureRecognizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.GestureRecognizer) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.LongListSelector** 클래스 | [**SemanticZoom** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 클래스 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.ObscuredEventArgs** 클래스 | [**SystemProtection**](https://docs.microsoft.com/uwp/api/Windows.Phone.System.SystemProtection), [**WindowActivatedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.WindowActivatedEventArgs) classes |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.Panorama** 클래스 | [**허브** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) 클래스 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationFrame**,<br/>(SWN = **System.Windows.Navigation**) <br/>**SWN.NavigationService** 클래스 | [**프레임** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) 클래스 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationPage** 클래스 | [**페이지** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) 클래스 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.TiltEffect** 클래스 | [**PointerDownThemeAnimation** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation) 클래스 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.TimePickerPage** 클래스 | [**TimePickerFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePickerFlyout) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WebBrowser** 클래스 | [**WebView** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) 클래스 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WebBrowserExtensions** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WrapPanel** 클래스 | 일반 레이아웃용으로 직접 해당하는 항목이 없습니다. [**ItemsWrapGrid** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) 하 고 [ **WrapGrid** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) 항목 컨트롤의 항목 패널 템플릿에서 사용할 수 있습니다. |
| (MPD = **Microsoft.Phone.Data**) <br/>**MPD.Linq** 네임스페이스 | 직접적으로 해당하는 항목이 없습니다. |
| (MPD = **Microsoft.Phone.Data**) <br/>**MPD.Linq.Mapping** 네임스페이스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Phone.Globalization** 네임스페이스 | 직접적으로 해당하는 항목이 없습니다. |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.DeviceExtendedProperties**, **DeviceStatus** 클래스 | [**EasClientDeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Security.ExchangeActiveSyncProvisioning.EasClientDeviceInformation)하십시오 [ **MemoryManager** ](https://docs.microsoft.com/uwp/api/Windows.System.MemoryManager) 클래스입니다. 자세한 내용은 [디바이스 상태](wpsl-to-uwp-input-and-sensors.md)를 참조하세요. |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.MediaCapabilities** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.UserExtendedProperties** 클래스 | [**AdvertisingManager** ](https://docs.microsoft.com/uwp/api/Windows.System.UserProfile.AdvertisingManager) 클래스 |
| **System.Windows** 네임스페이스 | [**Windows.UI.Xaml**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml) namespace |
| **System.Windows.Automation** 네임스페이스 | [**Windows.UI.Xaml.Automation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation) namespace |
| **System.Windows.Controls**, **System.Windows.Input** 네임스페이스 | [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core), [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Xaml.Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls) namespaces |
| **System.Windows.Controls.DrawingSurface**, **DrawingSurfaceBackgroundGrid** 클래스 | [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) class |
| **System.Windows.Controls.RichTextBox** 클래스 | [**RichEditBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) class |
| **System.Windows.Controls.WrapPanel** 클래스 | 일반 레이아웃용으로 직접 해당하는 항목이 없습니다. [**ItemsWrapGrid** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) 하 고 [ **WrapGrid** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) 항목 컨트롤의 항목 패널 템플릿에서 사용할 수 있습니다. |
| **System.Windows.Controls.Primitives** 네임스페이스 | [**Windows.UI.Xaml.Controls.Primitives**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives) namespace |
| **System.Windows.Controls.Shapes** 네임스페이스 | [**Windows.UI.Xaml.Controls.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) namespace |
| **System.Windows.Data** 네임스페이스 | [**Windows.UI.Xaml.Data**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data) namespace |
| **System.Windows.Documents** 네임스페이스 | [**Windows.UI.Xaml.Documents**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents) namespace |
| **System.Windows.Ink** 네임스페이스 | 직접적으로 해당하는 항목이 없습니다. |
| **System.Windows.Markup** 네임스페이스 | [**Windows.UI.Xaml.Markup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Markup) namespace | 
| **System.Windows.Navigation** 네임스페이스 | [**Windows.UI.Xaml.Navigation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Navigation) namespace |
| **System.Windows.UIElement.Tap** 이벤트, **EventHandler&lt;GestureEventArgs&gt;** 대리자 | [**Tapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.tapped) event, [**TappedEventHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.tappedeventhandler) delegate |
| 데이터 및 서비스 |  |
| **System.Data.Linq.DataContext** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **System.Data.Linq.Mapping.ColumnAttribute** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **System.Data.Linq.SqlClient.SqlHelpers** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| 장치 | |
| **Microsoft.Devices**, **Microsoft.Devices.Sensors** 네임스페이스 | [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration)하십시오 [ **Windows.Devices.Enumeration.Pnp**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.Pnp)하십시오 [ **Windows.Devices.Input** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Input)하십시오 [ **Windows.Devices.Sensors** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors) 네임 스페이스 |
| **Microsoft.Devices.Camera**, **Microsoft.Devices.PhotoCamera** 클래스 | [**MediaCapture** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) 클래스입니다. 또한 [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) 클래스(Windows 전용). |
| **Microsoft.Devices.CameraButtons** 클래스 | [**HardwareButtons** ](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons) 클래스 |
| **Microsoft.Devices.CameraVideoBrushExtensions** 클래스 | [**CaptureElement** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) 클래스 |
| **Microsoft.Devices.Environment** 클래스 | 직접적으로 해당하는 항목이 없습니다. 조건부 컴파일을 사용하고 사용자 지정 기호를 정의하여 문제를 해결할 수 있습니다. 또는 [IsAttached](https://docs.microsoft.com/dotnet/api/system.diagnostics.debugger.isattached?redirectedfrom=MSDN#System_Diagnostics_Debugger_IsAttached) 속성을 사용하여 문제를 해결할 수 있습니다. |
| **Microsoft.Devices.MediaHistory** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Devices.VibrateController** 클래스 | [**VibrationDevice** ](https://docs.microsoft.com/uwp/api/Windows.Phone.Devices.Notification.VibrationDevice) 클래스 |
| **Microsoft.Devices.Radio.FMRadio** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Devices.Sensors.Accelerometer**, **Compass** 클래스 | [  **Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors) 네임스페이스에서 |
| **Microsoft.Devices.Sensors.Gyroscope** 클래스 | [**회전 계** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Gyrometer) 클래스 |
| **Microsoft.Devices.Sensors.Motion** 클래스 | [**경사 계** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Inclinometer) 클래스 |
| 전역화 | |
| **System.Globalization** 네임스페이스 | [**Windows.Globalization**](https://docs.microsoft.com/uwp/api/Windows.Globalization) namespace |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentCulture** 속성 | (SG = **System.Globalization**) <br/> **S.CultureInfo.CurrentCulture** 속성 |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentUICulture** 속성 | (SG = **System.Globalization**) <br/> **S.CultureInfo.CurrentUICulture** 속성 |
| 그래픽 및 애니메이션 | |
| **Microsoft.Xna.Framework 합니다. \***  네임 스페이스 [XNA Framework 클래스 라이브러리](https://go.microsoft.com/fwlink/p/?LinkId=263769), [콘텐츠 파이프라인 클래스 라이브러리](https://go.microsoft.com/fwlink/p/?LinkId=263770) | 직접적으로 해당하는 항목이 없습니다. 일반적으로 C++로 작성한 [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx)를 사용합니다. [게임 개발](https://docs.microsoft.com/previous-versions/windows/apps/hh452744(v=win.10)) 및 [DirectX 및 XAML interop](https://docs.microsoft.com/previous-versions/windows/apps/hh825871(v=win.10))을 참조하세요. |
| **Microsoft.Xna.Framework.Audio.Microphone** 클래스 | [**MediaCapture** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) 클래스 |
| **Microsoft.Xna.Framework.Audio.SoundEffect** 클래스 | [**MediaElement** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 클래스 |
| **Microsoft.Xna.Framework.GamerServices** 네임스페이스 | (WPS = **Windows.Phone.System**) <br/> [**WPS.UserProfile.GameServices.Core**](https://docs.microsoft.com/uwp/api/Windows.Phone.System.UserProfile.GameServices.Core) namespace |
| **Microsoft.Xna.Framework.GamerServices.Guide** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Xna.Framework.Input.GamePad** 클래스 | [**HardwareButtons** ](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons) 클래스 |
| **Microsoft.Xna.Framework.Input.Touch.TouchPanel** 클래스 | [**GestureRecognizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.GestureRecognizer) class |
| (MXFM = **Microsoft.Xna.Framework.Media**) <br/> **MXFM.MediaLibrary**, **MXFM.PhoneExtensions.MediaLibraryExtensions** 클래스 | [**KnownFolders** ](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) 클래스 |
| **Microsoft.Xna.Framework.Media.MediaQueue** 클래스 | [**SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls) class |
| **Microsoft.Xna.Framework.Media.Playlist** 클래스 | [**BackgroundMediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.BackgroundMediaPlayer) class |
| **System.Windows.Media** 네임스페이스 | [**Windows.UI.Xaml.Media**](/uwp/api/Windows.UI.Xaml.Media) namespace |
| **System.Windows.Media.RadialGradientBrush** 클래스 | 직접적으로 해당하는 항목이 없습니다. [미디어 및 그래픽](wpsl-to-uwp-porting-xaml-and-ui.md)을 참조하세요. |
| **System.Windows.Media.Animation** 네임스페이스 | [**Windows.UI.Xaml.Media.Animation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation) namespace |
| **System.Windows.Media.Effects** 네임스페이스 | 직접적으로 해당하는 항목이 없습니다. |
| **System.Windows.Media.Imaging** 네임스페이스 | [**Windows.UI.Xaml.Media.Imaging**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging) namespace |
| **System.Windows.Media.Media3D** 네임스페이스 | [**Windows.UI.Xaml.Media.Media3D**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Media3D) namespace |
| **System.Windows.Shapes** 네임스페이스 | [**Windows.UI.Xaml.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) namespace |
| 시작 관리자 및 선택자 | |
| **Microsoft.Phone.Tasks.AddressChooserTask**, **EmailAddressChooserTask**, **PhoneNumberChooserTask** 클래스 | [**ContactPicker** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.ContactPicker) 클래스 |
| **Microsoft.Phone.Tasks.AddWalletItemTask**, **AddWalletItemResult** 클래스 | [**Windows.ApplicationModel.Wallet**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Wallet) namespace |
| **Microsoft.Phone.Tasks.BingMapsDirectionsTask**, **BingMapsTask** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Phone.Tasks.CameraCaptureTask** 클래스 | [**MediaCapture** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) 클래스입니다. 또한 [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) 클래스(Windows 전용). |
| **Microsoft.Phone.Tasks.MarketplaceDetailTask** | [**CurrentApp** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 클래스 ([**RequestAppPurchaseAsync** ](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) 메서드) |
| **Microsoft.Phone.Tasks.ConnectionSettingsTask**, **MarketplaceHubTask**, **MarketplaceReviewTask**, **MarketplaceSearchTask**, **MediaPlayerLauncher**, **SearchTask**, **SmsComposeTask**, **WebBrowserTask** 클래스 | [**표시 아이콘** ](https://docs.microsoft.com/uwp/api/Windows.System.Launcher) 클래스 |
| **Microsoft.Phone.Tasks.EmailComposeTask** 클래스 | [**데는 EmailMessage** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Email.EmailMessage) 클래스 |
| **Microsoft.Phone.Tasks.GameInviteTask** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Phone.Tasks.MapDownloaderTask**, **MapsDirectionsTask**, **MapsTask**, **MapUpdaterTask** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Phone.Tasks.PhoneCallTask** 클래스 | [**PhoneCallManager** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls.PhoneCallManager) 클래스 |
| **Microsoft.Phone.Tasks.PhotoChooserTask** 클래스 | [**FileOpenPicker** ](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 클래스 |
| **Microsoft.Phone.Tasks.SaveAppointmentTask** 클래스 | [**AppointmentManager** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments.AppointmentManager) 클래스 |
| **Microsoft.Phone.Tasks.SaveContactTask**, **SaveEmailAddressTask**, **SavePhoneNumberTask** 클래스 | [**StoredContact** ](https://docs.microsoft.com/uwp/api/Windows.Phone.PersonalInformation.StoredContact) 클래스 (Windows Phone만 해당) | 
| **Microsoft.Phone.Tasks.SaveRingtoneTask** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Phone.Tasks.ShareLinkTask**, **ShareMediaTask**, **ShareStatusTask** 클래스 | [**DataPackage** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) 클래스 |
| 위치 | |
| **System.Device.Location** 네임스페이스 | [**Windows.Devices.Geolocation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation) namespace |
| **System.Device.GeoCoordinateWatcher** 클래스 | [**Geolocator** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator) 클래스 |
| 지도 | |
| **Microsoft.Phone.Maps** 네임스페이스 | [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) namespace |
| **Microsoft.Phone.Maps.Controls** 네임스페이스 | [**Windows.UI.Xaml.Controls.Maps**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps) namespace |
| **Microsoft.Phone.Maps.Controls.Map** 클래스 | [**MapControl** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 클래스 |
| **Microsoft.Phone.Maps.Services** 네임스페이스 | [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) namespace |
| **Microsoft.Phone.Maps.Services.GeocodeQuery**, **ReverseGeocodeQuery** 클래스 | [**MapLocationFinder** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) 클래스 |
| **System.Device.Location.GeoCoordinate** 클래스 | [**Geopoint** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geopoint) 클래스 |
| **Microsoft.Phone.Maps.Services.Route** 클래스 | [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) class |
| **Microsoft.Phone.Maps.Services.RouteQuery** 클래스 | [**MapRouteFinder** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinder) 클래스 |
| 수익 창출 | |
| **Microsoft.Phone.Marketplace** 네임스페이스 | [**Windows.ApplicationModel.Store**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store) namespace |
| 미디어 | |
| **Microsoft.Phone.Media** 네임스페이스 | [**MediaElement** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 클래스 |
| 네트워킹 | |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.DeviceNetworkInformation** 클래스 | [**호스트 이름**](https://docs.microsoft.com/uwp/api/Windows.Networking.HostName)하십시오 [ **NetworkInformation** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) 클래스 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterface** 클래스 | [**NetworkInformation** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) 클래스 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterfaceInfo** 클래스 | [**ConnectionProfile** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionProfile) 클래스 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterfaceList** 클래스 | [**NetworkInformation** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) 클래스 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.SocketExtensions** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.WebRequestExtensions** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Phone.Networking.Voip** 네임스페이스 | 직접적으로 해당하는 항목이 없습니다. |
| **System.Net.CookieCollection** 클래스 | 여전히 지원되지만 일부 속성은 누락되었음(예: IsReadOnly) |
| **System.Net.DownloadProgressChangedEventArgs** 클래스 및 **System.Net.WebClient**와 관련된 유사한 클래스 | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 클래스 (또는 [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). [System.Net.Http.StreamContent](https://docs.microsoft.com/previous-versions/visualstudio/hh138119(v=vs.118))에서 파생되어 진행률을 측정합니다. |
| **System.Net.DnsEndPoint**, **IPAddress** 클래스 | 이러한 클래스는 여전히 지원되지만 일부 속성은 누락되었습니다. 또는 [**HostName**](https://docs.microsoft.com/uwp/api/Windows.Networking.HostName) 클래스로 포팅합니다. |
| **System.Net.HttpUtility** 클래스 | [**HtmlFormatHelper** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.HtmlFormatHelper) 클래스 |
| **System.Net.HttpWebRequest** 클래스 | 부분적으로 지원되지만 권장되는 미래 지향적인 대안은 [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 클래스 또는 [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))입니다. 이러한 API는 [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118))를 사용하여 HTTP 요청을 나타냅니다. |
| **System.Net.HttpWebResponse** 클래스 | 여전히 지원되지만, Close() 대신 Dispose()를 사용합니다. 그러나 권장되는 미래 지향적인 대안은 [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 클래스 또는 [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))입니다. 이러한 API는 [System.Net.Http.HttpResponseMessage](https://docs.microsoft.com/dotnet/api/system.net.http.httpresponsemessage?redirectedfrom=MSDN)를 사용하여 HTTP 응답을 나타냅니다. |
| (SNN = **System.Net.NetworkInformation**) <br/> **SNN.NetworkChange** 클래스 | 생성자를 제외하고 계속 지원됩니다. |
| **System.Net.OpenReadCompletedEventArgs** 클래스 및 **System.Net.WebClient**와 관련된 유사한 클래스 | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 클래스 (또는 [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.Sockets.Socket** 클래스 | 여전히 지원되지만, Close() 대신 Dispose()를 사용합니다. 또는 [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) 클래스로 포팅합니다. |
| **System.Net.Sockets.SocketException** 클래스 | 여전히 지원되지만, ErrorCode 대신 SocketErrorCode 속성을 사용합니다. |
| **System.Net.Sockets.UdpAnySourceMulticastClient**, **UdpSingleSourceMulticastClient** 클래스 | [**DatagramSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket) class |
| **System.Net.UploadProgressChangedEventArgs** 클래스 및 **System.Net.WebClient**와 관련된 유사한 클래스 | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 클래스 (또는 [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.WebClient** 클래스 | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 클래스 (또는 [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.WebRequest** 클래스 | 부분적으로 지원되지만(다른 속성 집합), 권장되는 미래 지향적인 대안은 [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 클래스 또는 [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))입니다. 이러한 API는 [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118))를 사용하여 HTTP 요청을 나타냅니다. |
| **System.Net.WebResponse** 클래스 | 여전히 지원되지만, Close() 대신 Dispose()를 사용합니다. 그러나 권장되는 미래 지향적인 대안은 [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 클래스 또는 [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))입니다. 이러한 API는 [System.Net.Http.HttpResponseMessage](https://docs.microsoft.com/dotnet/api/system.net.http.httpresponsemessage?redirectedfrom=MSDN)를 사용하여 HTTP 응답을 나타냅니다. |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventArgs** 클래스 | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 클래스 (또는 [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventHandler** 클래스 | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 클래스 (또는 [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.UriFormatException** 클래스 | **System.FormatException** 클래스 |
| 알림 | |
| MPN = **Microsoft.Phone.Notification** 네임스페이스 | [**Windows.UI.Notifications**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications)하십시오 [ **Windows.Networking.PushNotifications** ](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications) 네임 스페이스 |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotification** 클래스 | [**TileNotification** ](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification) 클래스 |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotificationChannel** 클래스 | [**PushNotificationChannel** ](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannel) 클래스 |
| 프로그래밍 | |
| **System** 네임스페이스 | [**Windows.Foundation**](https://docs.microsoft.com/uwp/api/Windows.Foundation) namespace |
| **System.Diagnostics.StackFrame**, **StackTrace** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **System.Diagnostics** 네임스페이스 | [**Windows.Foundation.Diagnostics**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Diagnostics) namespace |
| **System.ICloneable** 인터페이스 | 적절한 형식을 반환하는 사용자 지정 메서드입니다. |
| **System.Reflection.Emit.ILGenerator** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| 반응적 확장 | |
| **Microsoft.Phone.Reactive** 네임스페이스 | 직접적으로 해당하는 항목이 없습니다. |
| 반사 | |
| **System.Type** 클래스 | **System.Reflection.TypeInfo** 클래스. [UWP 앱용 .NET Framework의 리플렉션](https://docs.microsoft.com/dotnet/framework/reflection-and-codedom/reflection-for-windows-store-apps)을 참조하세요. |
| 리소스 | |
| **System.Resources.ResourceManager** 클래스 | (WA = **Windows.ApplicationModel**)<br/>[**WA 합니다. Resources.Core** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core) 하 고 [ **WA 합니다. 리소스** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources) 네임 스페이스 [ **ResourceManager** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager) 클래스입니다. [Windows 런타임 앱에서 리소스 만들기 및 검색](https://docs.microsoft.com/previous-versions/windows/apps/hh694557(v=vs.140))을 참조하세요. |
| 보안 요소 | |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> **MPS.SecureElementChannel**, **MPS.SecureElementSession** 클래스 | [**SmartCardConnection** ](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardConnection) 클래스 |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> **MPS.SecureElementReader** 클래스 | [**스마트 카드 판독기** ](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardReader) 클래스 |
| 보안 | |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.Aes**, **SSC.RSA** 클래스 | [**CryptographicEngine** ](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.CryptographicEngine) 클래스 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.HMACSHA256**, **SSC.SHA256** 클래스 | [**HashAlgorithmProvider** ](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.HashAlgorithmProvider) 클래스 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.ProtectedData** 클래스 | [**DataProtectionProvider** ](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.DataProtection.DataProtectionProvider) 클래스 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.RandomNumberGenerator** 클래스 | [**CryptographicBuffer** ](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.CryptographicBuffer) 클래스 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.X509Certificates.X509Certificate** 클래스 | [**CertificateEnrollmentManager**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Certificates.CertificateEnrollmentManager) class |
| 셸 | |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBar** 클래스 | [**CommandBar** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) 클래스 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBarIconButton** 클래스 | [**AppBarButton** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) 클래스 (내에서 사용 하는 경우를 [ **PrimaryCommands** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands) 속성) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBarMenuItem** 클래스 | [**AppBarButton** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) 클래스 (내에서 사용 하는 경우를 [ **SecondaryCommands** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands) 속성) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.CycleTileData**, **MPSh.FlipTileData**, **MPSh.IconicTileData**, **MPSh.ShellTileData**, **MPSh.StandardTileData** 클래스 | [**TileTemplateType** ](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileTemplateType) 클래스 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.PhoneApplicationService** 클래스 | [**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication), [**DisplayRequest**](https://docs.microsoft.com/uwp/api/Windows.System.Display.DisplayRequest) classes |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ProgressIndicator** 클래스 | [**StatusBarProgressIndicator**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.StatusBarProgressIndicator) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellTile** 클래스 | [**SecondaryTile** ](https://docs.microsoft.com/uwp/api/Windows.UI.StartScreen.SecondaryTile) 클래스 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellTileSchedule** 클래스 | [**TileUpdater** ](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater) 클래스 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellToast** 클래스 | [**ToastNotificationManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.SystemTray** 클래스 | [**StatusBar** ](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.StatusBar) 클래스 |
| 저장소 및 I/O | |
| **Microsoft.Phone.Storage.ExternalStorage**, **ExternalStorageDevice**, **ExternalStorageFile**, **ExternalStorageFolder** 클래스 | [**KnownFolders** ](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) 클래스 |
| **System.IO** 네임스페이스 | [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage), [**Windows.Storage.Streams**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams) namespaces |
| **System.IO.Directory** 클래스 | [**StorageFolder** ](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) 클래스 |
| **System.IO.File** 클래스 | [**StorageFile** ](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 하 고 [ **PathIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.PathIO) 클래스
| (SII = **System.IO.IsolatedStorage**) <br/> **SII.IsolatedStorageFile** 클래스 |[**ApplicationData.LocalFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder) property |
| (SII = **System.IO.IsolatedStorage**) <br/> **SII.IsolatedStorageSettings** 클래스 | [**ApplicationData.LocalSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings) property |
| **System.IO.Stream** 클래스 | 아직 지원되긴 하지만 BeginRead()/EndRead() 및 BeginWrite()/EndWrite() 대신 ReadAsync() 및 WriteAsync()를 사용하세요. |
| 전자지갑 | |
| **Microsoft.Phone.Wallet** 네임스페이스 | [**Windows.ApplicationModel.Wallet**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Wallet) namespace |
| xml | |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTime** 메서드 |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTimeOffset** 메서드 |


다음 항목은 [프로젝트 포팅](wpsl-to-uwp-porting-to-a-uwp-project.md)입니다.

