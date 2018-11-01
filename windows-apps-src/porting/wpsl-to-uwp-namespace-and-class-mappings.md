---
author: stevewhims
description: 이 항목에서는 이와 동일한 유니버설 Windows 플랫폼 (UWP)에 대 한 포괄적인 매핑을 WindowsPhone Silverlight api를 제공 합니다.
title: WindowsPhone Silverlight를 UWP 네임 스페이스 및 클래스 매핑
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 54118b41fc1f3036dddba9a0cfb8ecd860c1e233
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5873513"
---
# <a name="windowsphone-silverlight-to-uwp-api-mappings"></a>WindowsPhone Silverlight를 UWP API로 매핑


이 항목에서는 이와 동일한 유니버설 Windows 플랫폼 (UWP)에 대 한 포괄적인 매핑을 WindowsPhone Silverlight api를 제공 합니다. 일반적으로는 기능이 일대일로 매핑되지는 않지만 각 플랫폼의 네임스페이스 또는 클래스에 비슷한 기능이 있을 수 있습니다.

매핑 테이블 UWP 프로젝트에서 작업 하 고 다시 WindowsPhone Silverlight 프로젝트의에서 소스 코드를 사용 하는 경우에 도움이 됩니다. 두 플랫폼 간에는 네임스페이스와 클래스의 이름(UI 컨트롤 포함)이 다릅니다. 대부분의 경우 간단히 네임스페이스 이름을 변경하면 코드가 컴파일됩니다. 클래스 또는 API 이름이 네임스페이스 이름과 함께 변경된 경우도 있습니다. 드물기는 하지만, 매핑이 더 복잡하여 접근 방법을 변경해야 하는 경우도 있습니다.

**테이블을 사용 하는 방법:** 먼저 사용 중인 클래스의 이름을 검색 합니다. 매핑이 네임스페이스 이름을 변경하는 것보다 더 복잡한 모든 경우에 클래스가 나열됩니다. 클래스가 나열되지 않는 경우 매핑 시 네임스페이스만 변경하면 됩니다. 따라서 클래스의 네임스페이스 이름을 찾고 해당하는 UWP 네임스페이스 이름을 찾습니다. 클래스가 해당 네임스페이스에 있습니다. 네임스페이스가 나열되지 않는 경우 이름이 변경되지 않은 것입니다.

**참고**Windows10 보다 훨씬 더.NET framework Windows Phone 스토어 앱을 많이 지원 합니다. 예를 들어 Windows10 여러 System.ServiceModel.\* 네임 스페이스 뿐만 아니라 System.Net, System.Net.NetworkInformation 및 System.Net.Sockets에 있습니다.
또한 Windows10 앱의 이점을 활용할 수에서.NET 네이티브는 MSIL을 기본적으로 실행 가능 컴퓨터 코드로 변환 하는 미리 컴파일 기술입니다. .NET 네이티브 앱은 해당 MSIL 앱보다 더 빠르게 시작되고 메모리와 배터리를 더 적게 사용합니다.

| WindowsPhone Silverlight | Windows 런타임 |
| ------------------------- | --------------- |
| 광고 | |
| **Microsoft.Advertising.Mobile.UI.AdControl** 클래스 | [AdControl](http://msdn.microsoft.com/library/advertising-windows-sdk-api-reference-adcontrol.aspx) 클래스 |
| 알람, 미리 알림 및 백그라운드 에이전트 | |
| **Microsoft.Phone.BackgroundAgent** 클래스 | [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 클래스 |
| **Microsoft.Phone.Scheduler** 네임스페이스 | [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847) 네임스페이스 |
| **Microsoft.Phone.Scheduler.Alarm** 클래스 | [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 및 [**ToastNotificationManager**](https://msdn.microsoft.com/library/windows/apps/br208642) 클래스 |
| **Microsoft.Phone.Scheduler.PeriodicTask**, **ScheduledAction**, **ScheduledActionService**, **ScheduledTask**, **ScheduledTaskAgent** 클래스 | [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 클래스 |
| **Microsoft.Phone.Scheduler.Reminder** 클래스 | [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 및 [**ToastNotificationManager**](https://msdn.microsoft.com/library/windows/apps/br208642) 클래스 |
| **Microsoft.Phone.PictureDecoder** 클래스 | [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) 클래스 |
| **Microsoft.Phone.BackgroundAudio** 네임스페이스 | [**Windows.Media.Playback**](https://msdn.microsoft.com/library/windows/apps/dn640562) 네임스페이스 |
| **Microsoft.Phone.BackgroundTransfer** 네임스페이스 | [**Windows.Networking.BackgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) 네임스페이스 |
| 앱 모델 및 환경 | |
| **System.AppDomain** 클래스 | 직접적으로 해당하는 항목이 없습니다. [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324), [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016) 클래스를 참조하세요. |
| **System.Environment** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **System.ComponentModel.Annotations** 클래스  | 직접적으로 해당하는 항목이 없습니다. |
| **System.ComponentModel.BackgroundWorker** 클래스 | [**ThreadPool**](https://msdn.microsoft.com/library/windows/apps/br229621) 클래스 |
| **System.ComponentModel.DesignerProperties** 클래스 | [**DesignMode**](https://msdn.microsoft.com/library/windows/apps/br224664) 클래스 |
| **System.Threading.Thread**, **System.Threading.ThreadPool** 클래스 | [**ThreadPool**](https://msdn.microsoft.com/library/windows/apps/br229621) 클래스 |
| (ST = **System.Threading**) <br/> **ST.Thread.MemoryBarrier** 메서드 | (ST = **System.Threading**) <br/> **ST.Interlocked.MemoryBarrier** 메서드 |
| (ST = **System.Threading**) <br/> **ST.Thread.ManagedThreadId** 속성 | (S = **System**) <br/> **S.Environment.ManagedThreadId** 속성 |
| **System.Threading.Timer** 클래스 | [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/br230587) 클래스 |
| (SWT = **System.Windows.Threading**) <br/> **SWT.Dispatcher** 클래스 | [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 클래스 |
| (SWT = **System.Windows.Threading**) <br/> **SWT.DispatcherTimer** 클래스 | [**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250) 클래스 |
| Visual Studio용 Blend | |
| (MEDC = **Microsoft.Expression.Drawing.Core**) <br/> **MEDC.GeometryHelper** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Expression.Interactivity** 네임스페이스 | [Microsoft.Xaml.Interactivity](http://go.microsoft.com/fwlink/p/?LinkId=328776) 네임스페이스 |
| **Microsoft.Expression.Interactivity.Core** 네임스페이스 | [Microsoft.Xaml.Interactions.Core](http://go.microsoft.com/fwlink/p/?LinkId=328773) 네임스페이스 |
| (MEIC = **Microsoft.Expression.Interactivity.Core**) <br/> **MEIC.ExtendedVisualStateManager** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Expression.Interactivity.Input** 네임스페이스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Expression.Interactivity.Media** 네임스페이스 | [Microsoft.Xaml.Interactions.Media](http://go.microsoft.com/fwlink/p/?LinkId=328775) 네임스페이스 |
| **Microsoft.Expression.Shapes** 네임스페이스 | 직접적으로 해당하는 항목이 없습니다. |
| (MI = **Microsoft.Internal**) <br/> **MI.IManagedFrameworkInternalHelper** 인터페이스 | 직접적으로 해당하는 항목이 없습니다. |
| 연락처 및 일정 데이터 | |
| **Microsoft.Phone.UserData** 네임스페이스 | [**Windows.ApplicationModel.Contacts**](https://msdn.microsoft.com/library/windows/apps/br225002), [**Windows.ApplicationModel.Appointments**](https://msdn.microsoft.com/library/windows/apps/dn263359) 네임스페이스 |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Account**, **ContactAddress**, **ContactCompanyInformation**, **ContactEmailAddress**, **ContactPhoneNumber** 클래스 | [**Contact**](https://msdn.microsoft.com/library/windows/apps/br224849) 클래스 |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Appointments** 클래스 | [**AppointmentCalendar**](https://msdn.microsoft.com/library/windows/apps/dn596134) 클래스 |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Contacts** 클래스 | [**ContactStore**](https://msdn.microsoft.com/library/windows/apps/dn624859) 클래스 |
| 컨트롤 및 UI 인프라 | |
| **ControlTiltEffect.TiltEffect** 클래스 | Windows 런타임 애니메이션 라이브러리의 애니메이션은 공용 컨트롤의 기본 스타일로 기본 제공됩니다. [애니메이션](wpsl-to-uwp-porting-xaml-and-ui.md)을 참조하세요. |
| **Microsoft.Phone.Controls** 네임스페이스 | [**Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227716) 네임스페이스 |
| (MPC = **Microsoft.Phone.Controls**) <br/> **MPC.ContextMenu** 클래스 | [**PopupMenu**](https://msdn.microsoft.com/library/windows/apps/br208693) 클래스 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.DatePickerPage** 클래스 | [**DatePickerFlyout**](https://msdn.microsoft.com/library/windows/apps/dn625013) 클래스 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.GestureListener** 클래스 | [**GestureRecognizer**](https://msdn.microsoft.com/library/windows/apps/br241937) 클래스 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.LongListSelector** 클래스 | [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) 클래스 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.ObscuredEventArgs** 클래스 | [**SystemProtection**](https://msdn.microsoft.com/library/windows/apps/jj585394), [**WindowActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br208377) 클래스 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.Panorama** 클래스 | [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843) 클래스 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationFrame**,<br/>(SWN = **System.Windows.Navigation**) <br/>**SWN.NavigationService** 클래스 | [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) 클래스 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationPage** 클래스 | [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) 클래스 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.TiltEffect** 클래스 | [**PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969164) 클래스 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.TimePickerPage** 클래스 | [**TimePickerFlyout**](https://msdn.microsoft.com/library/windows/apps/dn608313) 클래스 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WebBrowser** 클래스 | [**WebView**](https://msdn.microsoft.com/library/windows/apps/br227702) 클래스 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WebBrowserExtensions** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WrapPanel** 클래스 | 일반 레이아웃용으로 직접 해당하는 항목이 없습니다. 항목 컨트롤의 항목 패널 템플릿에서 [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/dn298849) 및 [**WrapGrid**](https://msdn.microsoft.com/library/windows/apps/br227717)를 사용할 수 있습니다. |
| (MPD = **Microsoft.Phone.Data**) <br/>**MPD.Linq** 네임스페이스 | 직접적으로 해당하는 항목이 없습니다. |
| (MPD = **Microsoft.Phone.Data**) <br/>**MPD.Linq.Mapping** 네임스페이스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Phone.Globalization** 네임스페이스 | 직접적으로 해당하는 항목이 없습니다. |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.DeviceExtendedProperties**, **DeviceStatus** 클래스 | [**EasClientDeviceInformation**](https://msdn.microsoft.com/library/windows/apps/hh701390), [**MemoryManager**](https://msdn.microsoft.com/library/windows/apps/dn633831) 클래스. 자세한 내용은 [디바이스 상태](wpsl-to-uwp-input-and-sensors.md)를 참조하세요. |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.MediaCapabilities** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.UserExtendedProperties** 클래스 | [**AdvertisingManager**](https://msdn.microsoft.com/library/windows/apps/dn363391) 클래스 |
| **System.Windows** 네임스페이스 | [**Windows.UI.Xaml**](https://msdn.microsoft.com/library/windows/apps/br209045) 네임스페이스 |
| **System.Windows.Automation** 네임스페이스 | [**Windows.UI.Xaml.Automation**](https://msdn.microsoft.com/library/windows/apps/br209179) 네임스페이스 |
| **System.Windows.Controls**, **System.Windows.Input** 네임스페이스 | [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383), [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084), [**Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227716) 네임스페이스 |
| **System.Windows.Controls.DrawingSurface**, **DrawingSurfaceBackgroundGrid** 클래스 | [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) 클래스 |
| **System.Windows.Controls.RichTextBox** 클래스 | [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/br227548) 클래스 |
| **System.Windows.Controls.WrapPanel** 클래스 | 일반 레이아웃용으로 직접 해당하는 항목이 없습니다. 항목 컨트롤의 항목 패널 템플릿에서 [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/dn298849) 및 [**WrapGrid**](https://msdn.microsoft.com/library/windows/apps/br227717)를 사용할 수 있습니다. |
| **System.Windows.Controls.Primitives** 네임스페이스 | [**Windows.UI.Xaml.Controls.Primitives**](https://msdn.microsoft.com/library/windows/apps/br209818) 네임스페이스 |
| **System.Windows.Controls.Shapes** 네임스페이스 | [**Windows.UI.Xaml.Controls.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) 네임스페이스 |
| **System.Windows.Data** 네임스페이스 | [**Windows.UI.Xaml.Data**](https://msdn.microsoft.com/library/windows/apps/br209917) 네임스페이스 |
| **System.Windows.Documents** 네임스페이스 | [**Windows.UI.Xaml.Documents**](https://msdn.microsoft.com/library/windows/apps/br209984) 네임스페이스 |
| **System.Windows.Ink** 네임스페이스 | 직접적으로 해당하는 항목이 없습니다. |
| **System.Windows.Markup** 네임스페이스 | [**Windows.UI.Xaml.Markup**](https://msdn.microsoft.com/library/windows/apps/br228046) 네임스페이스 | 
| **System.Windows.Navigation** 네임스페이스 | [**Windows.UI.Xaml.Navigation**](https://msdn.microsoft.com/library/windows/apps/br243300) 네임스페이스 |
| **System.Windows.UIElement.Tap** 이벤트, **EventHandler&lt;GestureEventArgs&gt;** 대리자 | [**Tapped**](https://msdn.microsoft.com/library/windows/apps/br208985) 이벤트, [**TappedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br227993) 대리자 |
| 데이터 및 서비스 |  |
| **System.Data.Linq.DataContext** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **System.Data.Linq.Mapping.ColumnAttribute** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **System.Data.Linq.SqlClient.SqlHelpers** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| 장치 | |
| **Microsoft.Devices**, **Microsoft.Devices.Sensors** 네임스페이스 | [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459), [**Windows.Devices.Enumeration.Pnp**](https://msdn.microsoft.com/library/windows/apps/br225517), [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648), [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/br206408) 네임스페이스 |
| **Microsoft.Devices.Camera**, **Microsoft.Devices.PhotoCamera** 클래스 | [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 클래스. 또한 [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 클래스(Windows 전용). |
| **Microsoft.Devices.CameraButtons** 클래스 | [**HardwareButtons**](https://msdn.microsoft.com/library/windows/apps/jj207557) 클래스 |
| **Microsoft.Devices.CameraVideoBrushExtensions** 클래스 | [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) 클래스 |
| **Microsoft.Devices.Environment** 클래스 | 직접적으로 해당하는 항목이 없습니다. 조건부 컴파일을 사용하고 사용자 지정 기호를 정의하여 문제를 해결할 수 있습니다. 또는 [IsAttached](http://msdn.microsoft.com/library/e299w87h.aspx) 속성을 사용하여 문제를 해결할 수 있습니다. |
| **Microsoft.Devices.MediaHistory** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Devices.VibrateController** 클래스 | [**VibrationDevice**](https://msdn.microsoft.com/library/windows/apps/jj207230) 클래스 |
| **Microsoft.Devices.Radio.FMRadio** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Devices.Sensors.Accelerometer**, **Compass** 클래스 | [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/br206408) 네임스페이스에서 |
| **Microsoft.Devices.Sensors.Gyroscope** 클래스 | [**Gyrometer**](https://msdn.microsoft.com/library/windows/apps/br225718) 클래스 |
| **Microsoft.Devices.Sensors.Motion** 클래스 | [**Inclinometer**](https://msdn.microsoft.com/library/windows/apps/br225766) 클래스 |
| 세계화 | |
| **System.Globalization** 네임스페이스 | [**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) 네임스페이스 |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentCulture** 속성 | (SG = **System.Globalization**) <br/> **S.CultureInfo.CurrentCulture** 속성 |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentUICulture** 속성 | (SG = **System.Globalization**) <br/> **S.CultureInfo.CurrentUICulture** 속성 |
| 그래픽 및 애니메이션 | |
| **Microsoft.Xna.Framework.\*** 네임스페이스, [XNA Framework 클래스 라이브러리](http://go.microsoft.com/fwlink/p/?LinkId=263769), [콘텐츠 파이프라인 클래스 라이브러리](http://go.microsoft.com/fwlink/p/?LinkId=263770) | 직접적으로 해당하는 항목이 없습니다. 일반적으로 C++로 작성한 [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274)를 사용합니다. [게임 개발](https://msdn.microsoft.com/library/windows/apps/hh452744) 및 [DirectX 및 XAML interop](https://msdn.microsoft.com/library/windows/apps/hh825871)을 참조하세요. |
| **Microsoft.Xna.Framework.Audio.Microphone** 클래스 | [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 클래스 |
| **Microsoft.Xna.Framework.Audio.SoundEffect** 클래스 | [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 클래스 |
| **Microsoft.Xna.Framework.GamerServices** 네임스페이스 | (WPS = **Windows.Phone.System**) <br/> [**WPS.UserProfile.GameServices.Core**](https://msdn.microsoft.com/library/windows/apps/jj207609) 네임스페이스 |
| **Microsoft.Xna.Framework.GamerServices.Guide** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Xna.Framework.Input.GamePad** 클래스 | [**HardwareButtons**](https://msdn.microsoft.com/library/windows/apps/jj207557) 클래스 |
| **Microsoft.Xna.Framework.Input.Touch.TouchPanel** 클래스 | [**GestureRecognizer**](https://msdn.microsoft.com/library/windows/apps/br241937) 클래스 |
| (MXFM = **Microsoft.Xna.Framework.Media**) <br/> **MXFM.MediaLibrary**, **MXFM.PhoneExtensions.MediaLibraryExtensions** 클래스 | [**KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151) 클래스 |
| **Microsoft.Xna.Framework.Media.MediaQueue** 클래스 | [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) 클래스 |
| **Microsoft.Xna.Framework.Media.Playlist** 클래스 | [**BackgroundMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/dn652527) 클래스 |
| **System.Windows.Media** 네임스페이스 | [**Windows.UI.Xaml.Media**](/uwp/api/Windows.UI.Xaml.Media) 네임스페이스 |
| **System.Windows.Media.RadialGradientBrush** 클래스 | 직접적으로 해당하는 항목이 없습니다. [미디어 및 그래픽](wpsl-to-uwp-porting-xaml-and-ui.md)을 참조하세요. |
| **System.Windows.Media.Animation** 네임스페이스 | [**Windows.UI.Xaml.Media.Animation**](https://msdn.microsoft.com/library/windows/apps/br243232) 네임스페이스 |
| **System.Windows.Media.Effects** 네임스페이스 | 직접적으로 해당하는 항목이 없습니다. |
| **System.Windows.Media.Imaging** 네임스페이스 | [**Windows.UI.Xaml.Media.Imaging**](https://msdn.microsoft.com/library/windows/apps/br243258) 네임스페이스 |
| **System.Windows.Media.Media3D** 네임스페이스 | [**Windows.UI.Xaml.Media.Media3D**](https://msdn.microsoft.com/library/windows/apps/br243274) 네임스페이스 |
| **System.Windows.Shapes** 네임스페이스 | [**Windows.UI.Xaml.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) 네임스페이스 |
| 시작 관리자 및 선택자 | |
| **Microsoft.Phone.Tasks.AddressChooserTask**, **EmailAddressChooserTask**, **PhoneNumberChooserTask** 클래스 | [**ContactPicker**](https://msdn.microsoft.com/library/windows/apps/br224913) 클래스 |
| **Microsoft.Phone.Tasks.AddWalletItemTask**, **AddWalletItemResult** 클래스 | [**Windows.ApplicationModel.Wallet**](https://msdn.microsoft.com/library/windows/apps/dn631399) 네임스페이스 |
| **Microsoft.Phone.Tasks.BingMapsDirectionsTask**, **BingMapsTask** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Phone.Tasks.CameraCaptureTask** 클래스 | [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 클래스. 또한 [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 클래스(Windows 전용). |
| **Microsoft.Phone.Tasks.MarketplaceDetailTask** | [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 클래스([**RequestAppPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/hh967813) 메서드) |
| **Microsoft.Phone.Tasks.ConnectionSettingsTask**, **MarketplaceHubTask**, **MarketplaceReviewTask**, **MarketplaceSearchTask**, **MediaPlayerLauncher**, **SearchTask**, **SmsComposeTask**, **WebBrowserTask** 클래스 | [**Launcher**](https://msdn.microsoft.com/library/windows/apps/br241801) 클래스 |
| **Microsoft.Phone.Tasks.EmailComposeTask** 클래스 | [**EmailMessage**](https://msdn.microsoft.com/library/windows/apps/dn631270) 클래스 |
| **Microsoft.Phone.Tasks.GameInviteTask** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Phone.Tasks.MapDownloaderTask**, **MapsDirectionsTask**, **MapsTask**, **MapUpdaterTask** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Phone.Tasks.PhoneCallTask** 클래스 | [**PhoneCallManager**](https://msdn.microsoft.com/library/windows/apps/dn624832) 클래스 |
| **Microsoft.Phone.Tasks.PhotoChooserTask** 클래스 | [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 클래스 |
| **Microsoft.Phone.Tasks.SaveAppointmentTask** 클래스 | [**AppointmentManager**](https://msdn.microsoft.com/library/windows/apps/dn297254) 클래스 |
| **Microsoft.Phone.Tasks.SaveContactTask**, **SaveEmailAddressTask**, **SavePhoneNumberTask** 클래스 | [**StoredContact**](https://msdn.microsoft.com/library/windows/apps/jj207727) 클래스(Windows Phone 전용) | 
| **Microsoft.Phone.Tasks.SaveRingtoneTask** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Phone.Tasks.ShareLinkTask**, **ShareMediaTask**, **ShareStatusTask** 클래스 | [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/br205873) 클래스 |
| 위치 | |
| **System.Device.Location** 네임스페이스 | [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603) 네임스페이스 |
| **System.Device.GeoCoordinateWatcher** 클래스 | [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 클래스 |
| 지도 | |
| **Microsoft.Phone.Maps** 네임스페이스 | [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 네임스페이스 |
| **Microsoft.Phone.Maps.Controls** 네임스페이스 | [**Windows.UI.Xaml.Controls.Maps**](https://msdn.microsoft.com/library/windows/apps/dn610751) 네임스페이스 |
| **Microsoft.Phone.Maps.Controls.Map** 클래스 | [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 클래스 |
| **Microsoft.Phone.Maps.Services** 네임스페이스 | [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 네임스페이스 |
| **Microsoft.Phone.Maps.Services.GeocodeQuery**, **ReverseGeocodeQuery** 클래스 | [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 클래스 |
| **System.Device.Location.GeoCoordinate** 클래스 | [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675) 클래스 |
| **Microsoft.Phone.Maps.Services.Route** 클래스 | [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) 클래스 |
| **Microsoft.Phone.Maps.Services.RouteQuery** 클래스 | [**MapRouteFinder**](https://msdn.microsoft.com/library/windows/apps/dn636938) 클래스 |
| 수익 창출 | |
| **Microsoft.Phone.Marketplace** 네임스페이스 | [**Windows.ApplicationModel.Store**](https://msdn.microsoft.com/library/windows/apps/br225197) 네임스페이스 |
| 미디어 | |
| **Microsoft.Phone.Media** 네임스페이스 | [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 클래스 |
| 네트워킹 | |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.DeviceNetworkInformation** 클래스 | [**Hostname**](https://msdn.microsoft.com/library/windows/apps/br207113), [**NetworkInformation**](https://msdn.microsoft.com/library/windows/apps/br207293) 클래스 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterface** 클래스 | [**NetworkInformation**](https://msdn.microsoft.com/library/windows/apps/br207293) 클래스 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterfaceInfo** 클래스 | [**ConnectionProfile**](https://msdn.microsoft.com/library/windows/apps/br207249) 클래스 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterfaceList** 클래스 | [**NetworkInformation**](https://msdn.microsoft.com/library/windows/apps/br207293) 클래스 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.SocketExtensions** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.WebRequestExtensions** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **Microsoft.Phone.Networking.Voip** 네임스페이스 | 직접적으로 해당하는 항목이 없습니다. |
| **System.Net.CookieCollection** 클래스 | 여전히 지원되지만 일부 속성은 누락되었음(예: IsReadOnly) |
| **System.Net.DownloadProgressChangedEventArgs** 클래스 및 **System.Net.WebClient**와 관련된 유사한 클래스 | [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 클래스 (또는 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). [System.Net.Http.StreamContent](https://msdn.microsoft.com/library/system.net.http.streamcontent.aspx)에서 파생되어 진행률을 측정합니다. |
| **System.Net.DnsEndPoint**, **IPAddress** 클래스 | 이러한 클래스는 여전히 지원되지만 일부 속성은 누락되었습니다. 또는 [**HostName**](https://msdn.microsoft.com/library/windows/apps/br207113) 클래스로 포팅합니다. |
| **System.Net.HttpUtility** 클래스 | [**HtmlFormatHelper**](https://msdn.microsoft.com/library/windows/apps/hh738437) 클래스 |
| **System.Net.HttpWebRequest** 클래스 | 부분적으로 지원되지만 권장되는 미래 지향적인 대안은 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 클래스 또는 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)입니다. 이러한 API는 [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx)를 사용하여 HTTP 요청을 나타냅니다. |
| **System.Net.HttpWebResponse** 클래스 | 여전히 지원되지만, Close() 대신 Dispose()를 사용합니다. 그러나 권장되는 미래 지향적인 대안은 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 클래스 또는 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)입니다. 이러한 API는 [System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx)를 사용하여 HTTP 응답을 나타냅니다. |
| (SNN = **System.Net.NetworkInformation**) <br/> **SNN.NetworkChange** 클래스 | 생성자를 제외하고 계속 지원됩니다. |
| **System.Net.OpenReadCompletedEventArgs** 클래스 및 **System.Net.WebClient**와 관련된 유사한 클래스 | [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 클래스 (또는 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| **System.Net.Sockets.Socket** 클래스 | 여전히 지원되지만, Close() 대신 Dispose()를 사용합니다. 또는 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 클래스로 포팅합니다. |
| **System.Net.Sockets.SocketException** 클래스 | 여전히 지원되지만, ErrorCode 대신 SocketErrorCode 속성을 사용합니다. |
| **System.Net.Sockets.UdpAnySourceMulticastClient**, **UdpSingleSourceMulticastClient** 클래스 | [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) 클래스 |
| **System.Net.UploadProgressChangedEventArgs** 클래스 및 **System.Net.WebClient**와 관련된 유사한 클래스 | [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 클래스 (또는 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| **System.Net.WebClient** 클래스 | [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 클래스 (또는 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| **System.Net.WebRequest** 클래스 | 부분적으로 지원되지만(다른 속성 집합), 권장되는 미래 지향적인 대안은 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 클래스 또는 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)입니다. 이러한 API는 [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx)를 사용하여 HTTP 요청을 나타냅니다. |
| **System.Net.WebResponse** 클래스 | 여전히 지원되지만, Close() 대신 Dispose()를 사용합니다. 그러나 권장되는 미래 지향적인 대안은 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 클래스 또는 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)입니다. 이러한 API는 [System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx)를 사용하여 HTTP 응답을 나타냅니다. |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventArgs** 클래스 | [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 클래스 (또는 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventHandler** 클래스 | [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 클래스 (또는 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| **System.UriFormatException** 클래스 | **System.FormatException** 클래스 |
| 알림 | |
| MPN = **Microsoft.Phone.Notification** 네임스페이스 | [**Windows.UI.Notifications**](https://msdn.microsoft.com/library/windows/apps/br208661), [**Windows.Networking.PushNotifications**](https://msdn.microsoft.com/library/windows/apps/br241307) 네임스페이스 |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotification** 클래스 | [**TileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616) 클래스 |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotificationChannel** 클래스 | [**PushNotificationChannel**](https://msdn.microsoft.com/library/windows/apps/br241283) 클래스 |
| 프로그래밍 | |
| **System** 네임스페이스 | [**Windows.Foundation**](https://msdn.microsoft.com/library/windows/apps/br226021) 네임스페이스 |
| **System.Diagnostics.StackFrame**, **StackTrace** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| **System.Diagnostics** 네임스페이스 | [**Windows.Foundation.Diagnostics**](https://msdn.microsoft.com/library/windows/apps/br206677) 네임스페이스 |
| **System.ICloneable** 인터페이스 | 적절한 형식을 반환하는 사용자 지정 메서드입니다. |
| **System.Reflection.Emit.ILGenerator** 클래스 | 직접적으로 해당하는 항목이 없습니다. |
| 반응적 확장 | |
| **Microsoft.Phone.Reactive** 네임스페이스 | 직접적으로 해당하는 항목이 없습니다. |
| 리플렉션 | |
| **System.Type** 클래스 | **System.Reflection.TypeInfo** 클래스. [UWP 앱용 .NET Framework의 리플렉션](https://msdn.microsoft.com/library/hh535795.aspx)을 참조하세요. |
| 리소스 | |
| **System.Resources.ResourceManager** 클래스 | (WA = **Windows.ApplicationModel**)<br/>[**WA.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) 및 [**WA.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022) 네임스페이스, [**ResourceManager**](https://msdn.microsoft.com/library/windows/apps/br206078) 클래스. [Windows 런타임 앱에서 리소스 만들기 및 검색](https://msdn.microsoft.com/library/windows/apps/xaml/hh694557.aspx)을 참조하세요. |
| 보안 요소 | |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> **MPS.SecureElementChannel**, **MPS.SecureElementSession** 클래스 | [**SmartCardConnection**](https://msdn.microsoft.com/library/windows/apps/dn608002) 클래스 |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> **MPS.SecureElementReader** 클래스 | [**SmartCardReader**](https://msdn.microsoft.com/library/windows/apps/dn263857) 클래스 |
| 보안 | |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.Aes**, **SSC.RSA** 클래스 | [**CryptographicEngine**](https://msdn.microsoft.com/library/windows/apps/br241490) 클래스 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.HMACSHA256**, **SSC.SHA256** 클래스 | [**HashAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241511) 클래스 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.ProtectedData** 클래스 | [**DataProtectionProvider**](https://msdn.microsoft.com/library/windows/apps/br241559) 클래스 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.RandomNumberGenerator** 클래스 | [**CryptographicBuffer**](https://msdn.microsoft.com/library/windows/apps/br227092) 클래스 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.X509Certificates.X509Certificate** 클래스 | [**CertificateEnrollmentManager**](https://msdn.microsoft.com/library/windows/apps/br212075) 클래스 |
| 셸 | |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBar** 클래스 | [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/dn279427) 클래스 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBarIconButton** 클래스 | [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244) 클래스([**PrimaryCommands**](https://msdn.microsoft.com/library/windows/apps/dn279430) 속성 내에서 사용되는 경우) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBarMenuItem** 클래스 | [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244) 클래스([**SecondaryCommands**](https://msdn.microsoft.com/library/windows/apps/dn279434) 속성 내에서 사용되는 경우) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.CycleTileData**, **MPSh.FlipTileData**, **MPSh.IconicTileData**, **MPSh.ShellTileData**, **MPSh.StandardTileData** 클래스 | [**TileTemplateType**](https://msdn.microsoft.com/library/windows/apps/br208621) 클래스 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.PhoneApplicationService** 클래스 | [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016), [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) 클래스 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ProgressIndicator** 클래스 | [**StatusBarProgressIndicator**](https://msdn.microsoft.com/library/windows/apps/dn633865) 클래스 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellTile** 클래스 | [**SecondaryTile**](https://msdn.microsoft.com/library/windows/apps/br242183) 클래스 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellTileSchedule** 클래스 | [**TileUpdater**](https://msdn.microsoft.com/library/windows/apps/br208628) 클래스 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellToast** 클래스 | [**ToastNotificationManager**](https://msdn.microsoft.com/library/windows/apps/br208642) 클래스 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.SystemTray** 클래스 | [**StatusBar**](https://msdn.microsoft.com/library/windows/apps/dn633864) 클래스 |
| 저장소 및 I/O | |
| **Microsoft.Phone.Storage.ExternalStorage**, **ExternalStorageDevice**, **ExternalStorageFile**, **ExternalStorageFolder** 클래스 | [**KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151) 클래스 |
| **System.IO** 네임스페이스 | [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346), [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/br241791) 네임스페이스 |
| **System.IO.Directory** 클래스 | [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) 클래스 |
| **System.IO.File** 클래스 | [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 및 [**PathIO**](https://msdn.microsoft.com/library/windows/apps/hh701663) 클래스
| (SII = **System.IO.IsolatedStorage**) <br/> **SII.IsolatedStorageFile** 클래스 |[**ApplicationData.LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) 속성 |
| (SII = **System.IO.IsolatedStorage**) <br/> **SII.IsolatedStorageSettings** 클래스 | [**ApplicationData.LocalSettings**](https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.localsettings.aspx) 속성 |
| **System.IO.Stream** 클래스 | 아직 지원되긴 하지만 BeginRead()/EndRead() 및 BeginWrite()/EndWrite() 대신 ReadAsync() 및 WriteAsync()를 사용하세요. |
| Wallet | |
| **Microsoft.Phone.Wallet** 네임스페이스 | [**Windows.ApplicationModel.Wallet**](https://msdn.microsoft.com/library/windows/apps/dn631399) 네임스페이스 |
| Xml | |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTime** 메서드 |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTimeOffset** 메서드 |


다음 항목은 [프로젝트 포팅](wpsl-to-uwp-porting-to-a-uwp-project.md)입니다.

