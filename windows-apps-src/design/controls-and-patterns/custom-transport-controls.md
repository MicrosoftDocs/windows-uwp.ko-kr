---
Description: The media player has customizable XAML transport controls to manage control of audio and video content.
title: 사용자 지정 미디어 전송 컨트롤을 만드는 방법
ms.assetid: 6643A108-A6EB-42BC-B800-22EABD7B731B
label: Create custom media transport controls
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4960f6eb18fe4cffe34b8167a328521e9038c684
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8332014"
---
# <a name="create-custom-transport-controls"></a>사용자 지정 전송 컨트롤 만들기



MediaPlayerElement에는 UWP(유니버설 Windows 플랫폼) 앱에서 오디오와 비디오 콘텐츠의 컨트롤을 관리하기 위해 사용자 지정 가능한 XAML 전송 컨트롤이 있습니다. 여기에서 MediaTransportControls 템플릿을 사용자 지정하는 방법을 설명합니다. 오버플로 메뉴에 대한 작업을 수행하고 사용자 지정 단추를 추가하며 슬라이더를 수정하는 방법을 살펴보겠습니다.

> **중요 API**: [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx), [MediaPlayerElement.AreTransportControlsEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aretransportcontrolsenabled.aspx), [MediaTransportControls](https://msdn.microsoft.com/library/windows/apps/dn278677)

시작하기 전에 MediaPlayerElement 및 MediaTransportControls 클래스에 익숙해야 합니다. 자세한 내용은 MediaPlayerElement 컨트롤 가이드를 참조하세요.

> [!TIP]
> 이 항목의 예제는 [미디어 전송 컨트롤 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620023)에 기반을 두고 있습니다. 샘플을 다운로드하여 전체 코드를 보고 실행할 수 있습니다.

> [!NOTE]
> **MediaPlayerElement**는 Windows 10, 버전 1607 이상에서만 사용 가능합니다. 이전 버전의 Windows 10 앱을 개발하는 경우 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)를 대신 사용해야 합니다. 이 페이지의 모든 예제는 **MediaElement**에서도 작동합니다.

## <a name="when-should-you-customize-the-template"></a>템플릿을 사용자 지정해야 하는 경우

**MediaPlayerElement**는 수정하지 않고도 대부분의 비디오 및 오디오 재생 앱에서 우수하게 작동하도록 설계된 기본 제공 전송 컨트롤이 있습니다. 이 컨트롤은 [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.aspx) 클래스에서 제공하며 미디어 재생, 중지, 탐색 단추를 비롯하여, 볼륨 조정, 전체 화면으로 전환, 두 번째 디바이스로 캐스팅, 자막 사용, 오디오 트랙 전환 및 재생 속도 조정을 처리하는 단추를 포함합니다. MediaTransportControls에는 각 단추의 표시 여부와 사용 설정 여부를 제어할 수 있는 속성이 있습니다. 또한 [**IsCompact**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.iscompact.aspx) 속성을 설정하여 컨트롤을 한 행에 표시할지 아니면 두 행에 표시할지 여부를 지정할 수 있습니다.

그러나 추가로 컨트롤의 모양을 사용자 지정하거나 컨트롤 동작을 변경해야 하는 시나리오가 있을 수 있습니다. 예를 들면 다음과 같습니다.
- 아이콘, 슬라이더 동작 및 색을 변경합니다.
- 자주 사용하지 않는 명령 단추를 오버플로 메뉴로 이동합니다.
- 컨트롤 크기를 조정할 때 명령이 드롭아웃되는 순서를 변경합니다.
- 기본 집합에 없는 명령 단추를 제공합니다.

> [!NOTE]
> 화면에 표시된 단추는 화면에 충분한 공간이 없는 경우 미리 정의된 순서로 기본 제공 전송 컨트롤에서 삭제됩니다. 이 순서를 변경하거나 오버플로 메뉴에 맞지 않은 명령을 배치하려면 컨트롤을 사용자 지정해야 합니다.

기본 서식 파일을 수정하여 컨트롤의 모양을 사용자 지정할 수 있습니다. 컨트롤의 동작을 수정하거나 새 명령을 추가하려면 MediaTransportControls에서 파생된 사용자 지정 컨트롤을 만들면 됩니다.

> [!TIP]
> 사용자 지정 가능한 컨트롤 템플릿은 XAML 플랫폼의 강력한 기능이지만 이 기능의 사용에 따른 결과를 고려해야 합니다. 템플릿을 사용자 지정하면 앱의 정적 부분이 되므로 Microsoft에서 배포하는 템플릿의 플랫폼 업데이트를 받지 못합니다. Microsoft에서 템플릿을 업데이트한 경우 새 템플릿을 받아 다시 수정해야 업데이트된 템플릿의 이점을 얻을 수 있습니다.

## <a name="template-structure"></a>템플릿 구조

[**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.controltemplate.aspx)은 기본 스타일의 일부입니다. 전송 컨트롤의 기본 스타일은 [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.aspx) 클래스 참조 페이지에 나와 있습니다. 이 기본 스타일을 프로젝트에 복사하여 수정할 수 있습니다. ControlTemplate은 다른 XAML 컨트롤 템플릿과 비슷하게 여러 섹션으로 구분됩니다.
- 템플릿의 첫 번째 섹션에는 [**Style**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.aspx)의 여러 구성 요소에 대한 MediaTransportControls 정의가 포함되어 있습니다.
- 두 번째 섹션에서는 MediaTransportControls에서 사용하는 다양한 시각적 상태를 정의합니다.
- 세 번째 섹션에는 다양한 MediaTransportControls 요소를 포함하고 구성 요소가 배치되는 방식을 정의하는 [**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx)가 있습니다.

> [!NOTE]
> 템플릿을 수정하는 방법은 [컨트롤 템플릿]()을 참조하세요. 텍스트 편집기 또는 IDE의 유사한 편집기를 사용하여 \(*Program Files*)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\\(*SDK version*)\Generic에 있는 XAML 파일을 열 수 있습니다. 각 컨트롤의 기본 스타일과 템플릿은 **generic.xaml** 파일에 정의되어 있습니다. "MediaTransportControls"를 검색하여 generic.xaml에서 MediaTransportControls 템플릿을 찾을 수 있습니다.

다음 섹션에서는 전송 컨트롤의 기본 요소 중 몇 가지를 사용자 지정하는 방법에 대해 알아봅니다.
- [**Slider**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.slider.aspx): 사용자는 이 요소를 통해 미디어를 삭제할 수 있으며, 이 요소는 진행률도 표시합니다.
- [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.aspx): 모든 단추를 포함합니다.
자세한 내용은 MediaTransportControls 참조 항목의 구조 섹션을 참조하세요.

## <a name="customize-the-transport-controls"></a>전송 컨트롤 사용자 지정

MediaTransportControls의 모양만 수정하려는 경우 기본 컨트롤 스타일과 템플릿의 복사본을 작성하여 수정하면 됩니다. 그러나 컨트롤 기능에 추가하거나 해당 기능을 수정하려는 경우 MediaTransportControls에서 파생되는 새 클래스를 만들어야 합니다.

### <a name="re-template-the-control"></a>컨트롤 다시 템플릿

**MediaTransportControls 기본 스타일 및 템플릿을 사용자 지정하려면**
1. MediaTransportControls 스타일 및 템플릿의 기본 스타일을 프로젝트의 ResourceDictionary로 복사합니다.
2. Style에 x:Key 값을 지정하여 다음과 같이 식별합니다.

```xaml
<Style TargetType="MediaTransportControls" x:Key="myTransportControlsStyle">
    <!-- Style content ... -->
</Style>
```

3. MediaTransportControls와 함께 MediaPlayerElement를 UI에 추가합니다.
4. 여기에 나와 있는 것처럼 MediaTransportControls 요소의 Style 속성을 사용자 지정 Style 리소스로 설정합니다.

```xaml
<MediaPlayerElement AreTransportControlsEnabled="True">
    <MediaPlayerElement.TransportControls>
        <MediaTransportControls Style="{StaticResource myTransportControlsStyle}"/>
    </MediaPlayerElement.TransportControls>
</MediaPlayerElement>
```

스타일 및 템플릿 수정에 대한 자세한 내용은 [스타일링 컨트롤]() 및 [컨트롤 템플릿]()을 참조하세요.

### <a name="create-a-derived-control"></a>파생된 컨트롤 만들기

전송 컨트롤의 기능을 추가하거나 수정하려면 MediaTransportControls에서 파생된 새 클래스를 만들어야 합니다. `CustomMediaTransportControls`라는 파생 클래스가 [미디어 전송 컨트롤 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620023) 및 이 페이지의 나머지 예제에 나와 있습니다.

**MediaTransportControls에서 파생된 새 클래스를 만들려면**
1. 새 클래스 파일을 프로젝트에 추가합니다.
    - Visual Studio에서 프로젝트 &gt; 클래스 추가를 선택합니다. 새 항목 추가 대화 상자가 열립니다.
    - 새 항목 추가 대화 상자에서 클래스 파일의 이름을 입력한 다음 추가를 클릭합니다. 미디어 전송 컨트롤 샘플에서 클래스 이름은 `CustomMediaTransportControls`입니다.
2. MediaTransportControls 클래스에서 파생되도록 클래스 코드를 수정합니다.

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
}
```

3. [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.aspx)의 기본 스타일을 프로젝트의 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.resourcedictionary.aspx)로 복사합니다. 이는 수정하는 스타일 및 템플릿입니다.
(미디어 전송 컨트롤 샘플에서 "Themes"라는 새 폴더가 생성되고 generic.xaml이라는 ResourceDictionary 파일이 추가됩니다.
4. 스타일의 [**TargetType**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.targettype.aspx)을 새 사용자 지정 컨트롤 유형으로 변경합니다. (샘플에서 TargetType이 `local:CustomMediaTransportControls`로 변경됩니다.)

```xaml
xmlns:local="using:CustomMediaTransportControls">
...
<Style TargetType="local:CustomMediaTransportControls">
```

5. 사용자 지정 클래스의 [**DefaultStyleKey**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.defaultstylekey.aspx)를 설정합니다. 이렇게 하면 사용자 지정 클래스가 `local:CustomMediaTransportControls`의 TargetType과 함께 Style을 사용합니다.

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
    public CustomMediaTransportControls()
    {
        this.DefaultStyleKey = typeof(CustomMediaTransportControls);
    }
}
```

6. [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx)를 XAML 태그에 추가하고 사용자 지정 전송 컨트롤을 여기에 추가합니다. 한 가지 주의할 점은 기본 단추를 숨기고 표시하고, 사용하지 않도록 설정하고, 사용하도록 설정하는 API는 사용자 지정된 템플릿에서 여전히 작동한다는 것입니다.

```xaml
<MediaPlayerElement Name="MediaPlayerElement1" AreTransportControlsEnabled="True" Source="video.mp4">
    <MediaPlayerElement.TransportControls>
        <local:CustomMediaTransportControls x:Name="customMTC"
                                            IsFastForwardButtonVisible="True"
                                            IsFastForwardEnabled="True"
                                            IsFastRewindButtonVisible="True"
                                            IsFastRewindEnabled="True"
                                            IsPlaybackRateButtonVisible="True"
                                            IsPlaybackRateEnabled="True"
                                            IsCompact="False">
        </local:CustomMediaTransportControls>
    </MediaPlayerElement.TransportControls>
</MediaPlayerElement>
```

이제 컨트롤 스타일 및 템플릿을 수정하여 사용자 지정 컨트롤의 모양을 업데이트하고 제어 코드를 수정하여 해당 동작을 업데이트할 수 있습니다.

### <a name="working-with-the-overflow-menu"></a>오버플로 메뉴에 대한 작업

MediaTransportControls 명령 단추를 오버플로 메뉴로 이동하면 사용자가 필요로 할 때까지 자주 사용하지 않는 명령이 숨겨집니다.

MediaTransportControls 템플릿에서 명령 단추는 [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.aspx) 요소에 포함되어 있습니다. 명령 모음에는 기본 및 보조 명령의 개념이 있습니다. 기본 명령은 컨트롤에 기본적으로 표시되는 단추이며 항상 표시됩니다(단추를 비활성화하거나 숨기지 않은 경우 또는 충분한 공간이 있는 경우). 보조 명령은 사용자가 줄임표(...) 단추를 클릭할 때 나타나는 오버플로 메뉴에 표시됩니다. 자세한 내용은 [앱 바 및 명령 모음](app-bars.md) 문서를 참조하세요.

명령 모음 기본 명령의 요소를 오버플로 메뉴로 이동하려면 XAML 컨트롤 템플릿을 편집해야 합니다.

**오버플로 메뉴로 명령을 이동하려면**
1. 컨트롤 템플릿에서 `MediaControlsCommandBar`라는 CommandBar 요소를 찾습니다.
2. [**SecondaryCommands**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.secondarycommands.aspx) 섹션을 CommandBar에 대한 XAML에 추가합니다. 이 섹션을 [**PrimaryCommands**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.primarycommands.aspx)의 닫는 태그 뒤에 넣습니다.

```xaml
<CommandBar x:Name="MediaControlsCommandBar" ... >  
  <CommandBar.PrimaryCommands>
...
    <AppBarButton x:Name='PlaybackRateButton'
                    Style='{StaticResource AppBarButtonStyle}'
                    MediaTransportControlsHelper.DropoutOrder='4'
                    Visibility='Collapsed'>
      <AppBarButton.Icon>
        <FontIcon Glyph="&#xEC57;"/>
      </AppBarButton.Icon>
    </AppBarButton>
...
  </CommandBar.PrimaryCommands>
<!-- Add secondary commands (overflow menu) here -->
  <CommandBar.SecondaryCommands>
    ...
  </CommandBar.SecondaryCommands>
</CommandBar>
```

3. 명령으로 이 메뉴를 채우려면 원하는 [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx) 개체에 대한 XAML을 PrimaryCommands에서 잘라내어 SecondaryCommands에 붙여넣습니다. 이 예제에서 `PlaybackRateButton`를 오버플로 메뉴로 이동합니다.

4. 다음과 같이 단추에 레이블을 추가하고 스타일 지정 정보를 제거합니다.
오버플로 메뉴는 텍스트 단추로 구성되어 있기 때문에 단추에 텍스트 레이블을 추가하고 단추의 너비와 높이를 설정하는 스타일도 제거해야 합니다. 그러지 않으면 단추가 오버플로 메뉴에 제대로 나타나지 않습니다.

```xaml
<CommandBar.SecondaryCommands>
    <AppBarButton x:Name='PlaybackRateButton'
                  Label='Playback Rate'>
    </AppBarButton>
</CommandBar.SecondaryCommands>
```

> [!IMPORTANT]
> 여전히 단추를 보이게 하고 오버플로 메뉴에서 사용할 수 있도록 설정해야 합니다. 이 예제에서 IsPlaybackRateButtonVisible 속성이 true가 아니면 PlaybackRateButton 요소가 오버플로 메뉴에서 보이지 않습니다. IsPlaybackRateEnabled 속성이 true가 아니면 사용되지 않습니다. 이러한 속성 설정은 이전 섹션에 나와 있습니다.

### <a name="adding-a-custom-button"></a>사용자 지정 단추 추가

MediaTransportControls를 사용자 지정하려고 할 수 있는 한 가지 이유는 사용자 지정 명령을 컨트롤에 추가할 수 있기 때문입니다. 기본 명령으로 추가하든 아니면 보조 명령으로 추가하든 명령 단추를 만들고 해당 동작을 수정하기 위한 절차는 동일합니다. [미디어 전송 컨트롤 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620023)에서 "등급" 단추가 기본 명령에 추가됩니다.

**사용자 지정 명령 단추를 추가하려면**
1. AppBarButton 개체를 만들고 컨트롤 템플릿의 CommandBar에 추가합니다.

```xaml
<AppBarButton x:Name="LikeButton"
              Icon="Like"
              Style="{StaticResource AppBarButtonStyle}"
              MediaTransportControlsHelper.DropoutOrder="3"
              VerticalAlignment="Center" />
```

    You must add it to the CommandBar in the appropriate location. (For more info, see the Working with the overflow menu section.) How it's positioned in the UI is determined by where the button is in the markup. For example, if you want this button to appear as the last element in the primary commands, add it at the very end of the primary commands list.

    You can also customize the icon for the button. For more info, see the [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx) reference.

2. [**OnApplyTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.onapplytemplate.aspx) 재정의에서, 템플릿의 단추를 가져와서 해당 [**Click**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) 이벤트에 대한 처리기를 등록합니다. 이 코드는 `CustomMediaTransportControls` 클래스에 포함됩니다.

```csharp
public sealed class CustomMediaTransportControls :  MediaTransportControls
{
    // ...

    protected override void OnApplyTemplate()
    {
        // Find the custom button and create an event handler for its Click event.
        var likeButton = GetTemplateChild("LikeButton") as Button;
        likeButton.Click += LikeButton_Click;
        base.OnApplyTemplate();
    }

    //...
}
```

3. Click 이벤트 처리기에 단추를 클릭할 때 발생하는 작업을 수행하는 코드를 추가합니다.
다음은 해당 클래스의 전체 코드입니다.

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
    public event EventHandler< EventArgs> Liked;

    public CustomMediaTransportControls()
    {
        this.DefaultStyleKey = typeof(CustomMediaTransportControls);
    }

    protected override void OnApplyTemplate()
    {
        // Find the custom button and create an event handler for its Click event.
        var likeButton = GetTemplateChild("LikeButton") as Button;
        likeButton.Click += LikeButton_Click;
        base.OnApplyTemplate();
    }

    private void LikeButton_Click(object sender, RoutedEventArgs e)
    {
        // Raise an event on the custom control when 'like' is clicked.
        var handler = Liked;
        if (handler != null)
        {
            handler(this, EventArgs.Empty);
        }
    }
}
```

**"좋아요" 단추가 추가된 사용자 지정 미디어 전송 컨트롤**
![추가 좋아요 단추가 있는 사용자 지정 미디어 전송 컨트롤](images/controls/mtc_double_custom_inprod.png)

### <a name="modifying-the-slider"></a>슬라이더 수정

MediaTransportControls의 "검색" 컨트롤은 [**Slider**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.slider.aspx) 요소에서 제공됩니다. 이 컨트롤을 사용자 지정할 수 있는 한 가지 방법은 검색 동작의 세분성을 변경하는 것입니다.

기본 검색 슬라이더는 100개의 부분으로 나뉘어져 있으므로 검색 동작은 이 수만큼의 섹션으로 제한됩니다. [**MediaPlayerElement.MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.mediaopened.aspx)의 [**MediaOpened**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 이벤트 처리기에 있는 XAML 시각적 트리에서 Slider를 가져와서 검색 슬라이더의 세분성을 변경할 수 있습니다. 이 예제는 [**VisualTreeHelper**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.visualtreehelper.aspx)를 사용하여 Slider에 대한 참조를 가져온 다음 미디어가 120분보다 긴 경우 1%에서 0.1%까지(1000단계) 슬라이더의 기본 단계 빈도를 변경하는 방법을 보여 줍니다. MediaPlayerElement는 `MediaPlayerElement1`라고 합니다.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
  MediaPlayerElement1.MediaPlayer.MediaOpened += MediaPlayerElement_MediaPlayer_MediaOpened;
  base.OnNavigatedTo(e);
}

private void MediaPlayerElement_MediaPlayer_MediaOpened(object sender, RoutedEventArgs e)
{
  FrameworkElement transportControlsTemplateRoot = (FrameworkElement)VisualTreeHelper.GetChild(MediaPlayerElement1.TransportControls, 0);
  Slider sliderControl = (Slider)transportControlsTemplateRoot.FindName("ProgressSlider");
  if (sliderControl != null && MediaPlayerElement1.NaturalDuration.TimeSpan.TotalMinutes > 120)
  {
    // Default is 1%. Change to 0.1% for more granular seeking.
    sliderControl.StepFrequency = 0.1;
  }
}
```
## <a name="related-articles"></a>관련 문서

- [미디어 재생](media-playback.md)
