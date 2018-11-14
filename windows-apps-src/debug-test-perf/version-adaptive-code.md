---
author: jwmsft
title: 버전 적응 코드
description: ApiInformation 클래스를 사용하여 이전 버전과 호환성을 유지하면서 새로운 API 활용
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 3293e91e-6888-4cc3-bad3-61e5a7a7ab4e
ms.localizationpriority: medium
ms.openlocfilehash: e25a3bd447519ce344a95a1c335451f731552487
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6653386"
---
# <a name="version-adaptive-code"></a>버전 적응 코드

[적응 UI를 만드는](https://msdn.microsoft.com/windows/uwp/layout/layouts-with-xaml) 것은 적응 코드 작성과 유사하게 생각할 수 있습니다. 가장 작은 화면에서 실행될 기본 UI를 디자인한 다음 앱이 더 큰 화면에서 실행되고 있음을 감지하면 요소를 이동하거나 추가할 수 있습니다. 적응 코드를 사용하여 최하위 OS 버전에서 실행될 기본 코드를 작성하고, 앱이 새 기능을 사용할 수 있는 상위 버전에서 실행되고 있음을 감지하면 수동으로 선택한 기능을 추가할 수 있습니다.

ApiInformation, API 계약 및 Visual Studio 구성에 대한 중요한 배경 정보는 [버전 적응 앱](version-adaptive-apps.md)을 참조하세요.

### <a name="runtime-api-checks"></a>런타임 API 검사

코드의 조건에서 [Windows.Foundation.Metadata.ApiInformation](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.aspx) 클래스를 사용하여 호출할 API의 존재 여부를 테스트합니다. 그러면 앱이 실행되는 모든 디바이스에서 이 조건이 평가되지만, API가 있어 호출에 사용할 수 있는 디바이스에 대해서만 **true**로 평가합니다. 이렇게 하면 특정 OS 버전에서만 사용할 수 있는 API를 사용하는 앱을 만들기 위해 버전 적응 코드를 작성할 수 있습니다.

여기서는 Windows Insider Preview의 새로운 기능을 대상으로 지정하기 위한 구체적인 예를 살펴봅니다. **ApiInformation** 사용에 대한 일반적인 개요는 [장치 패밀리 개요](https://docs.microsoft.com/en-us/uwp/extension-sdks/device-families-overview#writing-code) 및 [API 계약을 사용하여 동적으로 기능 검색](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/) 블로그 게시물을 참조하세요.

> [!TIP]
> 많은 API 런타임 검사는 앱의 성능에 영향을 미칠 수 있습니다. 여기서는 예제에서 즉시 처리되는 검사를 보여 줍니다. 프로덕션 코드에서는 검사를 한 번 수행하고 결과를 캐시한 다음 캐시된 결과를 앱 전체에서 사용해야 합니다. 

### <a name="unsupported-scenarios"></a>지원되지 않는 시나리오

대부분의 경우 앱의 최소 버전을 SDK 버전 10240으로 설정된 상태로 두고 런타임 검사를 사용하여 앱이 이후 버전에서 실행될 때 새 API를 사용하도록 설정할 수 있습니다. 그러나 새로운 기능을 사용하기 위해 앱의 최소 버전을 증가시켜야 하는 경우도 있습니다.

다음을 사용하는 경우 앱의 최소 버전을 증가시켜야 합니다.
- 이전 버전에서 사용할 수 없는 기능을 필요로 하는 새로운 API. 지원되는 최소 버전을 해당 기능이 포함된 버전으로 증가시켜야 합니다. 자세한 내용은 [앱 접근 권한 값 선언](../packaging/app-capability-declarations.md)을 참조하세요.
- generic.xaml에 추가되고 이전 버전에서 사용할 수 없는 새로운 리소스 키. 런타임에 사용되는 generic.xaml의 버전은 디바이스가 실행되고 있는 OS 버전에 의해 결정됩니다. API 런타임 검사를 사용하여 XAML 리소스의 존재 여부를 확인할 수 없습니다. 따라서 앱이 지원하는 최소 버전에서 사용할 수 있는 리소스 키만 사용해야 합니다. 그러지 않으면 [XAMLParseException](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.markup.xamlparseexception.aspx)으로 인해 런타임에 앱의 작동이 중단됩니다.

### <a name="adaptive-code-options"></a>적응 코드 옵션

두 가지 방법으로 적응 코드를 만들 수 있습니다. 대부분의 경우에 최소 버전에서 실행할 앱 태그를 작성한 다음 앱 코드를 사용하여 최신 OS 기능이 있는 경우 해당 기능을 활용합니다. 그러나 시각적 상태에서 속성을 업데이트해야 하는 경우 OS 버전 간에 속성 또는 열거형 값 변경 사항만 있으면 API의 존재 여부에 따라 활성화되는 확장 가능한 상태 트리거를 만들 수 있습니다.

여기서는 이러한 옵션을 비교합니다.

**앱 코드**

사용해야 하는 경우:
- 확장 가능한 트리거에 대해 아래에 정의된 특정한 경우를 제외하고 모든 적응 코드 시나리오에 권장됩니다.

이점:
- API 차이를 태그로 연결하는 데 따른 개발자 오버헤드/복잡성을 방지합니다.

단점:
- 디자이너 지원 기능이 없습니다.

**상태 트리거**

사용해야 하는 경우:
- OS 버전 간에 논리 변경이 필요하지 않고 시각적 상태에 연결되어 있는 속성 또는 열거형 변경 사항만 있는 경우 사용합니다.

이점:
- API의 존재 여부에 따라 트리거되는 특정 시각적 상태를 만들 수 있습니다.
- 일부 디자이너 지원 기능을 사용할 수 있습니다.

단점:
- 사용자 지정 트리거의 사용은 시각적 상태로 제한되며 복잡한 적응형 레이아웃에 적합하지 않습니다.
- Setter를 사용하여 값 변경을 지정해야 하므로 간단한 변경만 가능합니다.
- 사용자 지정 상태 트리거는 설정하고 사용하기가 꽤 번거롭습니다.

## <a name="adaptive-code-examples"></a>적응 코드 예제

이 섹션에서는 Windows 10, 버전 1607(Windows Insider Preview)에서 새로 추가된 API를 사용하는 적응 코드의 몇 가지를 예제를 보여 줍니다.

### <a name="example-1-new-enum-value"></a>예제 1: 새 열거형 값

Windows 10, 버전 1607에서는 새로운 값인 **ChatWithoutEmoji**를 [InputScopeNameValue](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.inputscopenamevalue.aspx) 열거형에 추가합니다. 이 새로운 입력 범위는 **Chat** 입력 범위와 동일한 입력 동작을 포함하고 있지만(맞춤법 검사, 자동 완성, 자동 대문자 표시), 이모지 단추가 없는 터치 키보드에 매핑됩니다. 이는 고유한 이모지 선택기를 만들고 터치 키보드에서 기본 제공 이모지 단추를 사용하지 않도록 설정하려는 경우에 유용합니다. 

이 예제에서는 **ChatWithoutEmoji** 열거형 값이 있는지 확인하고 있으면 **TextBox**의 [InputScope](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox.inputscope.aspx) 속성을 설정하는 방법을 보여 줍니다. 이 값이 앱이 실행되는 시스템에 없는 경우 **InputScope**는 대신 **Chat**로 설정됩니다. 표시된 코드는 Page 생성자나 Page.Loaded 이벤트 처리기에 배치될 수 있습니다.

> [!TIP]
> API를 검사할 때 .NET 언어 기능에 의존하는 대신 정적 문자열을 사용하세요. 그러지 않으면 앱이 정의되지 않은 형식에 액세스하려고 하고 런타임에 작동이 중단될 수 있습니다.

**C#**
```csharp
// Create a TextBox control for sending messages 
// and initialize an InputScope object.
TextBox messageBox = new TextBox();
messageBox.AcceptsReturn = true;
messageBox.TextWrapping = TextWrapping.Wrap;
InputScope scope = new InputScope();
InputScopeName scopeName = new InputScopeName();

// Check that the ChatWithEmoji value is present.
// (It's present starting with Windows 10, version 1607,
//  the Target version for the app. This check returns false on earlier versions.)         
if (ApiInformation.IsEnumNamedValuePresent("Windows.UI.Xaml.Input.InputScopeNameValue", "ChatWithoutEmoji"))
{
    // Set new ChatWithoutEmoji InputScope if present.
    scopeName.NameValue = InputScopeNameValue.ChatWithoutEmoji;
}
else
{
    // Fall back to Chat InputScope.
    scopeName.NameValue = InputScopeNameValue.Chat;
}

// Set InputScope on messaging TextBox.
scope.Names.Add(scopeName);
messageBox.InputScope = scope;

// For this example, set the TextBox text to show the selected InputScope.
messageBox.Text = messageBox.InputScope.Names[0].NameValue.ToString();

// Add the TextBox to the XAML visual tree (rootGrid is defined in XAML).
rootGrid.Children.Add(messageBox);
```

이전 예제에서는 TextBox가 만들어지고 모든 속성이 코드에서 설정됩니다. 그러나 기존 XAML이 있는 경우 새 값이 지원되는 시스템에서 InputScope 속성을 변경해야 하면 여기에서 보여 주듯이 XAML을 변경하지 않고 변경할 수 있습니다. XAML에서 기본값을 **Chat**로 설정하지만 **ChatWithoutEmoji** 값이 있는 경우 코드에서 기본값을 재정의합니다.

**XAML**
```xaml
<TextBox x:Name="messageBox"
         AcceptsReturn="True" TextWrapping="Wrap"
         InputScope="Chat"
         Loaded="messageBox_Loaded"/>
```

**C#**
```csharp
private void messageBox_Loaded(object sender, RoutedEventArgs e)
{
    if (ApiInformation.IsEnumNamedValuePresent("Windows.UI.Xaml.Input.InputScopeNameValue", "ChatWithoutEmoji"))
    {
        // Check that the ChatWithEmoji value is present.
        // (It's present starting with Windows 10, version 1607,
        //  the Target version for the app. This code is skipped on earlier versions.)
        InputScope scope = new InputScope();
        InputScopeName scopeName = new InputScopeName();
        scopeName.NameValue = InputScopeNameValue.ChatWithoutEmoji;
        // Set InputScope on messaging TextBox.
        scope.Names.Add(scopeName);
        messageBox.InputScope = scope;
    }

    // For this example, set the TextBox text to show the selected InputScope.
    // This is outside of the API check, so it will happen on all OS versions.
    messageBox.Text = messageBox.InputScope.Names[0].NameValue.ToString();
}
```

이제 구체적인 예제가 있으므로 대상 및 최소 버전 설정이 어떻게 예제에 적용되는지 살펴보겠습니다.

이러한 예제에서는 XAML이나 코드에서 검사 없이 Chat 열거형 값을 사용할 수 있습니다. 이 값이 지원되는 최소 OS 버전에 있기 때문입니다. 

XAML이나 코드에서 검사 없이 ChatWithoutEmoji 값을 사용하는 경우 이 값이 대상 OS 버전에 있기 때문에 오류 없이 컴파일됩니다. 또한 대상 OS 버전을 사용하는 시스템에서 오류 없이 실행됩니다. 그러나 앱이 최소 OS 버전을 사용하는 시스템에서 실행되는 경우 ChatWithoutEmoji 열거형 값이 없기 때문에 런타임에 작동이 중단됩니다. 따라서 이 값을 코드에서만 사용하고 런타임 API 검사에 래핑하여 현재 시스템에서 지원되는 경우에만 호출되도록 해야 합니다.

### <a name="example-2-new-control"></a>예제 2: 새 컨트롤

일반적으로 새 버전의 Windows에서는 새로운 기능을 플랫폼에 제공하는 새 컨트롤을 UWP API 노출 영역에 제공합니다. 새 컨트롤을 활용하려면 [ApiInformation.IsTypePresent](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.istypepresent.aspx) 메서드를 사용합니다.

Windows 10, 버전 1607에서는 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx)라는 새로운 미디어 컨트롤을 도입했습니다. 이 컨트롤은 [MediaPlayer](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.aspx) 클래스를 기반으로 하므로 배경 오디오에 쉽게 연결하는 기능 등을 제공하며 미디어 스택의 향상된 아키텍처를 이용합니다.

그러나 앱이 버전 1607보다 이전 버전의 Windows 10에서 실행되고 있는 디바이스에서 실행되는 경우 새로운 **MediaPlayerElement** 컨트롤 대신 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaelement.aspx) 컨트롤을 사용해야 합니다. [**ApiInformation.IsTypePresent**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.istypepresent.aspx) 메서드를 사용하여 런타임에 MediaPlayerElement 컨트롤의 존재 여부를 확인할 수 있으며 앱이 실행되고 있는 시스템에 적합한 컨트롤을 로드할 수 있습니다.

이 예제에서는 MediaPlayerElement 형식의 존재 여부에 따라 새 MediaPlayerElement 또는 이전 MediaElement를 사용하는 앱을 만드는 방법을 보여 줍니다. 이 코드에서는 [UserControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol.aspx) 클래스를 사용하여 컨트롤과 관련 UI 및 코드를 구성 요소화하므로 OS 버전에 따라 이러한 항목을 내부 및 외부로 전환할 수 있습니다. 다른 방법으로, 이 간단한 예제에 필요한 것보다 많은 기능과 사용자 지정 동작을 제공하는 사용자 지정 컨트롤을 사용할 수 있습니다.
 
**MediaPlayerUserControl** 

`MediaPlayerUserControl`은 **MediaPlayerElement** 및 프레임 단위로 미디어를 건너뛰는 데 사용되는 몇 가지 단추를 캡슐화합니다. UserControl을 사용하면 이러한 컨트롤을 단일 엔터티로 처리할 수 있으며 이전 시스템의 MediaElement와 전환하기가 더 쉽습니다. 이 사용자 컨트롤은 MediaPlayerElement가 있는 시스템에서만 사용해야 하므로 이 사용자 컨트롤 내의 코드에서는 ApiInformation 검사를 사용하지 마세요.

> [!NOTE]
> 이 예제를 주제 중심으로 간단하게 유지하기 위해 프레임 단계 단추가 미디어 플레이어 외부에 배치됩니다. 사용자 환경을 개선하려면 사용자 지정 단추를 포함하도록 MediaTransportControls를 사용자 지정해야 합니다. 자세한 내용은 [사용자 지정 전송 컨트롤](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/custom-transport-controls)을 참조하세요. 

**XAML**
```xaml
<UserControl
    x:Class="MediaApp.MediaPlayerUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MediaApp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="300"
    d:DesignWidth="400">

    <Grid x:Name="MPE_grid>
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <StackPanel Orientation="Horizontal" 
                    HorizontalAlignment="Center" Grid.Row="1">
            <RepeatButton Click="StepBack_Click" Content="Step Back"/>
            <RepeatButton Click="StepForward_Click" Content="Step Forward"/>
        </StackPanel>
    </Grid>
</UserControl>
```

**C#**
```csharp
using System;
using Windows.Media.Core;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;

namespace MediaApp
{
    public sealed partial class MediaPlayerUserControl : UserControl
    {
        public MediaPlayerUserControl()
        {
            this.InitializeComponent();
            
            // The markup code compiler runs against the Minimum OS version so MediaPlayerElement must be created in app code
            MPE = new MediaPlayerElement();
            Uri videoSource = new Uri("ms-appx:///Assets/UWPDesign.mp4");
            MPE.Source = MediaSource.CreateFromUri(videoSource);
            MPE.AreTransportControlsEnabled = true;
            MPE.MediaPlayer.AutoPlay = true;

            // Add MediaPlayerElement to the Grid
            MPE_grid.Children.Add(MPE);

        }

        private void StepForward_Click(object sender, RoutedEventArgs e)
        {
            // Step forward one frame, only available using MediaPlayerElement.
            MPE.MediaPlayer.StepForwardOneFrame();
        }

        private void StepBack_Click(object sender, RoutedEventArgs e)
        {
            // Step forward one frame, only available using MediaPlayerElement.
            MPE.MediaPlayer.StepForwardOneFrame();
        }
    }
}
```

**MediaElementUserControl**
 
`MediaElementUserControl`은 **MediaElement** 컨트롤을 캡슐화합니다.

**XAML**
```xaml
<UserControl
    x:Class="MediaApp.MediaElementUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MediaApp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="300"
    d:DesignWidth="400">

    <Grid>
        <MediaElement AreTransportControlsEnabled="True" 
                      Source="Assets/UWPDesign.mp4"/>
    </Grid>
</UserControl>
```

> [!NOTE]
> `MediaElementUserControl`에 대한 코드 페이지에는 생성된 코드만 포함되어 있으므로 표시되지 않았습니다.

**IsTypePresent에 따라 컨트롤 초기화**

런타임에 **ApiInformation.IsTypePresent**를 호출하여 MediaPlayerElement가 있는지 검사합니다. 있으면 `MediaPlayerUserControl`을 로드하고, 없으면 `MediaElementUserControl`을 로드합니다.

**C#**
```csharp
public MainPage()
{
    this.InitializeComponent();

    UserControl mediaControl;

    // Check for presence of type MediaPlayerElement.
    if (ApiInformation.IsTypePresent("Windows.UI.Xaml.Controls.MediaPlayerElement"))
    {
        mediaControl = new MediaPlayerUserControl();
    }
    else
    {
        mediaControl = new MediaElementUserControl();
    }

    // Add mediaControl to XAML visual tree (rootGrid is defined in XAML).
    rootGrid.Children.Add(mediaControl);
}
```

> [!IMPORTANT]
> 이 검사는 `mediaControl` 개체를 `MediaPlayerUserControl` 또는 `MediaElementUserControl`로만 설정한다는 점에 유의하세요. MediaPlayerElement API를 사용할지, 아니면 MediaElement API를 사용할지를 결정해야 하는 코드의 어느 부분에서든 이러한 조건부 검사를 수행해야 합니다. 검사를 한 번 수행하고 결과를 캐시한 다음 캐시된 결과를 앱 전체에서 사용해야 합니다.

## <a name="state-trigger-examples"></a>상태 트리거 예제

확장 가능한 상태 트리거를 사용하면 태그와 코드를 함께 사용하여 코드에서 검사하는 조건(이 경우에는 특정 API의 존재 여부)에 따라 시각적 상태 변경을 트리거할 수 있습니다. 오버헤드가 발생하고 시각적 상태로만 제한되기 때문에 일반적인 적응 코드 시나리오에는 상태 트리거를 권장하지 않습니다. 

나머지 UI에 영향을 주지 않을 서로 다른 OS 버전 간의 작은 UI 변경(예: 컨트롤의 속성 또는 열거형 값 변경)이 있는 경우에만 적응 코드에 상태 트리거를 사용해야 합니다.

### <a name="example-1-new-property"></a>예제 1: 새 속성

확장 가능한 상태 트리거를 설정하는 첫 번째 단계는 [StateTriggerBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.statetriggerbase.aspx) 클래스를 하위 클래스화하여 API의 존재 여부에 따라 활성화될 사용자 지정 트리거를 만드는 것입니다. 이 예제에서는 속성 존재 여부가 XAML에서 설정된 `_isPresent` 변수와 일치하는 경우 활성화되는 트리거를 보여 줍니다.

**C#**
```csharp
class IsPropertyPresentTrigger : StateTriggerBase
{
    public string TypeName { get; set; }
    public string PropertyName { get; set; }

    private Boolean _isPresent;
    private bool? _isPropertyPresent = null;

    public Boolean IsPresent
    {
        get { return _isPresent; }
        set
        {
            _isPresent = value;
            if (_isPropertyPresent == null)
            {
                // Call into ApiInformation method to determine if property is present.
                _isPropertyPresent =
                ApiInformation.IsPropertyPresent(TypeName, PropertyName);
            }

            // If the property presence matches _isPresent then the trigger will be activated;
            SetActive(_isPresent == _isPropertyPresent);
        }
    }
}
```

다음 단계는 API의 존재 여부에 따라 두 가지 시각적 상태가 발생하도록 XAML에서 시각적 상태 트리거를 설정하는 것입니다. 

Windows 10 버전 1607에서는 [FrameworkElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.aspx) 클래스에 [AllowFocusOnInteraction](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.allowfocusoninteraction.aspx)이라는 새 속성을 도입했습니다. 이 속성은 사용자가 컨트롤을 사용할 때 컨트롤이 포커스를 받는지 여부를 결정합니다. 이는 사용자가 단추를 클릭하는 동안 데이터 입력을 위해 텍스트 상자에 포커스를 유지하고 터치 키보드를 계속 표시하려는 경우에 유용합니다.

이 예제에서 트리거는 속성이 있는지 여부를 검사합니다. 속성이 있으면 Button의 **AllowFocusOnInteraction** 속성을 **false**로 설정하고, 속성이 없으면 Button이 원래 상태를 유지합니다. 코드를 실행할 때 이 속성의 효과를 쉽게 확인할 수 있도록 하기 위해 TextBox가 포함되었습니다.

**XAML**
```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel>
        <TextBox Width="300" Height="36"/>
        <!-- Button to set the new property on. -->
        <Button x:Name="testButton" Content="Test" Margin="12"/>
    </StackPanel>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="propertyPresentStateGroup">
            <VisualState>
                <VisualState.StateTriggers>
                    <!--Trigger will activate if the AllowFocusOnInteraction property is present-->
                    <local:IsPropertyPresentTrigger 
                        TypeName="Windows.UI.Xaml.FrameworkElement" 
                        PropertyName="AllowFocusOnInteraction" IsPresent="True"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="testButton.AllowFocusOnInteraction" 
                            Value="False"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

### <a name="example-2-new-enum-value"></a>예제 2: 새 열거형 값

이 예제에서는 값의 존재 여부에 따라 다른 열거형 값을 설정하는 방법을 보여 줍니다. 이 예제에서는 사용자 지정 상태 트리거를 사용하여 이전 채팅 예제와 동일한 결과를 얻습니다. 이 예제에서는 디바이스에서 Windows 10, 버전 1607이 실행되면 새로운 ChatWithoutEmoji 입력 범위를 사용하고, 그렇지 않으면 **Chat** 입력 범위를 사용합니다. 이 트리거를 사용하는 시각적 상태는 입력 범위가 새 열거형 값의 존재 여부에 따라 선택되는 *if-else* 스타일에서 설정됩니다.

**C#**
```csharp
class IsEnumPresentTrigger : StateTriggerBase
{
    public string EnumTypeName { get; set; }
    public string EnumValueName { get; set; }

    private Boolean _isPresent;
    private bool? _isEnumValuePresent = null;

    public Boolean IsPresent
    {
        get { return _isPresent; }
        set
        {
            _isPresent = value;

            if (_isEnumValuePresent == null)
            {
                // Call into ApiInformation method to determine if value is present.
                _isEnumValuePresent =
                ApiInformation.IsEnumNamedValuePresent(EnumTypeName, EnumValueName);
            }

            // If the property presence matches _isPresent then the trigger will be activated;
            SetActive(_isPresent == _isEnumValuePresent);
        }
    }
}
```

**XAML**
```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <TextBox x:Name="messageBox"
     AcceptsReturn="True" TextWrapping="Wrap"/>


    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="EnumPresentStates">
            <!--if-->
            <VisualState x:Name="isPresent">
                <VisualState.StateTriggers>
                    <local:IsEnumPresentTrigger 
                        EnumTypeName="Windows.UI.Xaml.Input.InputScopeNameValue" 
                        EnumValueName="ChatWithoutEmoji" IsPresent="True"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="messageBox.InputScope" Value="ChatWithoutEmoji"/>
                </VisualState.Setters>
            </VisualState>
            <!--else-->
            <VisualState x:Name="isNotPresent">
                <VisualState.StateTriggers>
                    <local:IsEnumPresentTrigger 
                        EnumTypeName="Windows.UI.Xaml.Input.InputScopeNameValue" 
                        EnumValueName="ChatWithoutEmoji" IsPresent="False"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="messageBox.InputScope" Value="Chat"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

## <a name="related-articles"></a>관련 문서

- [장치 패밀리 개요](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview)
- [Dynamically detecting features with API contracts(API 계약을 사용하여 동적으로 기능 검색)](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)
