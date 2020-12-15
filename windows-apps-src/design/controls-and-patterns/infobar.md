---
description: InfoBar는 필수 앱 전체 메시지에 대한 인라인 알림입니다.
title: InfoBar
template: detail.hbs
ms.date: 11/30/2020
ms.topic: article
keywords: windows 10, winui, uwp
pm-contact: gabilka
design-contact: kimsea
dev-contact: ranjeshj
ms.custom: 20H2
ms.localizationpriority: medium
ms.openlocfilehash: 422d2cb0874abe2fbe767a75d718cd1f0637ccee
ms.sourcegitcommit: b99fe39126fbb457c3690312641f57d22ba7c8b6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/04/2020
ms.locfileid: "96604960"
---
# <a name="infobar"></a>InfoBar
InfoBar 컨트롤은 사용자에게 눈에 잘 띄지만 방해가 되지 않는 앱 전체의 상태 메시지를 표시하기 위한 것입니다. 표시되는 메시지 유형뿐만 아니라 사용자 고유의 동작 호출 또는 하이퍼링크 단추를 포함하는 옵션을 쉽게 표시할 수 있는 심각도 수준이 내장되어 있습니다. InfoBar는 다른 UI 콘텐츠와 인라인되어 있기 때문에 사용자가 컨트롤을 항상 표시하거나 닫을 수 있는 옵션이 있습니다. 

**Windows UI 라이브러리 가져오기**

:::row:::
   :::column:::
      ![WinUI 로고](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      **InfoBar** 컨트롤은 Windows 앱용 새 컨트롤과 UI 기능을 포함하는 NuGet 패키지인 Windows UI 라이브러리가 필요합니다. 설치 지침을 비롯한 자세한 내용은 [Windows UI 라이브러리](/uwp/toolkits/winui/)를 참조하세요.
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **Windows UI 라이브러리 API:** [InfoBar 클래스](/uwp/api/microsoft.ui.xaml.controls.infobar)

> [!TIP]
> 이 문서 전체에서 XAML의 **muxc** 별칭을 사용하여 프로젝트에 포함된 Windows UI 라이브러리 API를 나타냅니다. 이를 [Page](/uwp/api/windows.ui.xaml.controls.page) 요소(`xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`)에 추가했습니다.
>
>코드 숨김에서는 C#의 **muxc** 별칭을 사용하여 프로젝트에 포함된 Windows UI 라이브러리 API를 나타냅니다. 이 **using** 문(`using muxc = Microsoft.UI.Xaml.Controls;`)을 파일 맨 위에 추가했습니다.

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?
사용자가 변경된 애플리케이션 상태에 대한 알림을 받거나, 확인하거나, 조치를 취해야 할 때 InfoBar 컨트롤을 사용합니다. 기본적으로 알림은 사용자가 닫을 때까지 콘텐츠 영역에 그대로 남아 있지만 사용자 흐름을 끊지는 않습니다.

InfoBar는 레이아웃에서 공간을 차지하고 다른 모든 자식 요소처럼 동작합니다. 다른 콘텐츠를 덮거나 그 위에 뜨지 않습니다.

앱의 상태를 변경하지 않거나, 시간에 민감한 경고 또는 중요하지 않은 메시지에 대해 직접 사용자 작업을 확인하거나 응답하는 데 InfoBar 컨트롤을 사용하지 마세요.

### <a name="remarks"></a>설명
앱 인식 또는 경험에 **직접** 영향을 미치는 시나리오에서 사용자가 닫거나 상태가 해결된 InfoBar를 사용합니다.


다음은 몇 가지 예입니다.
- 인터넷 연결 끊김
- 특정 사용자 작업과 관련이 없이 자동으로 트리거된 경우 문서 저장 시 오류 발생
- 녹음하려고 할 때 마이크가 연결되지 않음
- 휴대폰에 연결할 수 없음
- 애플리케이션 구독이 만료됨


앱 인식 또는 경험에 **직접** 영향을 미치는 시나리오에서 사용자가 닫은 InfoBar를 사용합니다.

다음은 몇 가지 예입니다.
- 통화 녹음이 시작됨
- 업데이트가 '릴리스 정보' 링크와 함께 적용됨
- 서비스 약관이 업데이트되었으며 승인이 필요함
- 앱 전체 백업이 비동기적으로 완료됨
- 애플리케이션 구독이 만료에 가까움

### <a name="when-should-a-different-control-be-used"></a>다른 컨트롤을 사용해야 하는 경우

[ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog), [Flyout](/uwp/api/Windows.UI.Xaml.Controls.Flyout) 또는 [TeachingTip](/uwp/api/Microsoft.UI.Xaml.Controls.TeachingTip)을 사용하는 것이 더 적합한 몇 가지 시나리오가 있습니다.

- 특정 UI 요소의 컨텍스트에 정보를 표시하는 등 지속적인 알림이 필요하지 않은 시나리오의 경우 [Flyout](/uwp/design/controls-and-patterns/dialogs-and-flyouts/flyouts)이 더 나은 옵션입니다. 
- 애플리케이션에서 사용자 작업을 확인하고 user **_must_* _read 정보를 표시하는 시나리오의 경우 [ContentDialog](/uwp/design/controls-and-patterns/dialogs-and-flyouts/dialogs)를 사용합니다.
  - 또한 앱 상태가 너무 많이 변경되어 사용자가 앱과 상호 작용하는 모든 기능을 차단해야 하는 경우 ContentDialog를 사용합니다.
- 알림이 일시적인 교육 모멘트인 시나리오의 경우 [TeachingTip](/uwp/design/controls-and-patterns/dialogs-and-flyouts/teaching-tip)이 더 나은 옵션입니다.


올바른 알림 컨트롤을 선택하는 방법에 대한 자세한 내용은 [대화 상자 및 플라이아웃](/uwp/design/controls-and-patterns/dialogs-and-flyouts/) 문서를 참조하세요.


## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/InfoBar">앱을 열고 작동 중인 InfoBar를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

### <a name="create-an-infobar"></a>InfoBar 만들기
아래 XAML에서는 정보 알림에 대한 기본 스타일링이 있는 인라인 InfoBar에 대해 설명합니다. 정보 표시줄은 다른 요소처럼 배치할 수 있으며 기본 레이아웃 동작을 따릅니다.
예를 들어 세로 StackPanel에서 InfoBar는 가로로 확장되어 사용 가능한 너비를 채웁니다. 


기본적으로 InfoBar는 표시되지 않습니다. InfoBar를 표시하려면 XAML 또는 코드 숨김에서 IsOpen 속성을 true로 설정합니다.


```xaml
<muxc:InfoBar x:Name="UpdateAvailableNotification"
    Title="Update available."
    Message="Restart the application to apply the latest update.">
</muxc:InfoBar>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    if(IsUpdateAvailable())
    {
        UpdateAvailableNotification.IsOpen = true;
    }
}
```

![닫기 단추와 메시지가 있는 기본 상태의 InfoBar 샘플](images/infobar-default-title-message.png)

### <a name="using-pre-defined-severity-levels"></a>미리 정의된 심각도 수준 사용
알림의 중요도에 따라 일관된 상태 색상, 아이콘 및 보조 기술 설정을 자동으로 설정하도록 정보 표시줄의 유형을 심각도 속성을 통해 설정할 수 있습니다. 심각도가 설정되지 않은 경우 기본 정보 스타일이 적용됩니다.


```xaml
<muxc:InfoBar x:Name="SubscriptionExpiringNotification"
    Severity="Warning"
    Title="Your subscription is expiring in 3 days."
    Message="Renew your subscription to keep all functionality" />
```
![닫기 단추와 메시지가 있는 경고 상태의 InfoBar 샘플](images/infobar-warning-title-message.png)

### <a name="programmatic-close-in-infobar"></a>InfoBar에서 프로그래밍 방식으로 닫기
사용자가 닫기 단추를 통해 또는 프로그래밍 방식으로 InfoBar를 닫을 수 있습니다. 상태가 해결될 때까지 알림이 표시되어야 하며 사용자가 정보 표시줄을 닫는 기능을 제거하려는 경우 IsClosable 속성을 false로 설정할 수 있습니다.


이 속성의 기본값은 true이며, 이 경우 닫기 단추가 존재하며 'X' 형식을 취합니다.


```xaml
<muxc:InfoBar x:Name="NoInternetNotification"
    Severity="Error"
    Title="No Internet"
    Message="Reconnect to continue working."
    IsClosable="False" />
```
![닫기 단추가 없는 오류 상태의 InfoBar 샘플](images/infobar-error-no-close.png)

### <a name="customization-background-color-and-icon"></a>사용자 지정: 배경색 및 아이콘
미리 정의된 심각도 수준을 벗어나면 Background 및 IconSource 속성을 설정하여 아이콘 및 배경색을 사용자 지정할 수 있습니다. InfoBar는 정의된 심각도의 보조 기술 설정을 유지하며, 심각도가 정의되지 않은 경우 기본값을 유지합니다.


사용자 지정 배경색은 표준 배경 속성을 통해 설정할 수 있으며 심각도에서 설정한 색상을 재정의합니다.
자신만의 색상을 설정할 때 콘텐츠 가독성과 접근성을 고려하세요.


사용자 지정 아이콘은 IconSource 속성을 통해 설정할 수 있습니다. 기본적으로 아이콘이 표시됩니다(컨트롤이 축소되지 않는다고 가정).
이 아이콘은 IsIconVisible 속성을 false로 설정하여 제거할 수 있습니다. 사용자 지정 아이콘의 경우 권장되는 아이콘 크기는 20px입니다.


```xaml
<muxc:InfoBar x:Name="CallRecordingNotification"
    Title="Recording started"
    Message="Your call has begun recording."
    Background="{StaticResource LavenderBackgroundBrush}">
    <muxc:InfoBar.IconSource>
        <muxc:SymbolIconSource Symbol="Phone" />
    </muxc:InfoBar.IconSource>
</muxc:InfoBar>
```

![사용자 지정 배경색, 사용자 지정 아이콘 및 닫기 단추가 있는 기본 상태의 InfoBar 샘플](images/infobar-custom-icon-color.png)

### <a name="add-an-action-button"></a>작업 단추 추가

[ButtonBase](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase)를 상속하는 고유의 단추를 정의하고 ActionButton 속성에서 이를 설정하여 작업 단추를 추가할 수 있습니다. 사용자 지정 스타일은 일관성과 접근성을 위해 [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) 및 [HyperlinkButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton) 유형의 작업 단추에 적용됩니다. ActionButton 속성 외에도 사용자 지정 콘텐츠를 통해 작업 단추를 추가할 수 있으며 메시지 아래에 나타납니다.


```xaml
<muxc:InfoBar x:Name="NoInternetNotification"
    Severity="Error"
    Title="No Internet"
    Message="Reconnect to continue working.">
    <muxc:InfoBar.ActionButton>
        <Button Content="Network Settings" Click="InternetInfoBarButton_Click" />
    </muxc:InfoBar.ActionButton>
</muxc:InfoBar>
```

![한 줄 메시지와 작업 단추가 있는 오류 상태의 InfoBar 샘플](images/infobar-error-action-button.png)

```xaml
<muxc:InfoBar x:Name="TermsAndConditionsUpdatedNotification"
    Title="Terms and Conditions Updated"
    Message="Dismissal of this message implies agreement to the updated Terms and Conditions. Please view the changes on our website.">
    <muxc:InfoBar.ActionButton>
        <HyperlinkButton Content="Terms and Conditions Sep 2020" 
            NavigateUri="https://www.example.com"
            Click="UpdateInfoBarHyperlinkButton_Click" />
    <muxc:InfoBar.ActionButton />
</muxc:InfoBar>
```
![여러 줄로 확장되는 메시지와 하이퍼링크가 있는 InfoBar 샘플](images/infobar-default-hyperlink.png)

### <a name="content-wrapping"></a>콘텐츠 래핑
사용자 지정 콘텐츠를 제외하고 InfoBar 콘텐츠가 하나의 가로 줄에 맞출 수 없는 경우 세로로 배치됩니다. 제목, 메시지 및 ActionButton(있는 경우)은 각각 별도의 줄에 나타납니다.


```xaml
<muxc:InfoBar x:Name="BackupCompleteNotification"
    Severity="Success"
    Title="Backup complete: Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."  
    Message="All documents were uploaded successfully. Ultrices sagittis orci a scelerisque. Aliquet risus feugiat in ante metus dictum at tempor commodo. Auctor augue mauris augue neque gravida.">
    <muxc:InfoBar.ActionButton>
        <Button Content="Action"
            Click="BackupInfoBarButton_Click" />
    </muxc:InfoBar.ActionButton>
</muxc:InfoBar>
```

![성공 상태의 여러 줄 제목과 메시지 InfoBar의 InfoBar 샘플](images/infobar-success-content-wrapping.png)


### <a name="custom-content"></a>사용자 지정 콘텐츠
Content 속성을 사용하여 XAML 콘텐츠를 InfoBar에 추가할 수 있습니다. 나머지 컨트롤 콘텐츠 아래의 자체 줄에 나타납니다. InfoBar는 정의된 콘텐츠에 맞게 확장됩니다.


```xaml
<muxc:InfoBar x:Name="BackupInProgressNotification"
    Title="Backup in progress"  
    Message="Your documents are being saved to the cloud"
    IsClosable="False">
    <muxc:InfoBar.Content>
        <ProgressBar IsIndeterminate="True" Margin="0,0,0,6"/>
    </muxc:InfoBar.Content>
</muxc:InfoBar>
```

![확정되지 않은 진행률 표시줄이 있는 기본 상태의 InfoBar 샘플](images/infobar-default-custom-content.gif)

### <a name="lightweight-styling"></a>경량 스타일 지정

기본 스타일 및 ControlTemplate를 수정하여 컨트롤에 고유한 모양을 제공할 수 있습니다. 자세한 내용은 [스타일링 컨트롤](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-styles) 문서의 [경량 스타일 지정 섹션](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-styles#lightweight-styling)을 참조하세요.

예를 들어, 다음은 페이지의 InfoBar에서 제목 표시줄 글꼴 크기가 22pt가 되도록 합니다.

```xaml
<Page.Resources>
    <x:Double x:Key="InfoBarTitleFontSize">22</x:Double>
</Page.Resources>
```
### <a name="canceling-close"></a>닫기 취소
Closing 이벤트를 사용하여 InfoBar의 닫기를 취소 및/또는 연기할 수 있습니다. 이 이벤트를 사용하여 InfoBar를 열어 두거나 사용자 지정 작업을 수행할 수 있는 시간을 허용할 수 있습니다. InfoBar 닫기가 취소되면 IsOpen은 다시 true로 돌아갑니다. 프로그래밍 방식 닫기도 취소할 수 있습니다.


```xaml
<muxc:InfoBar x:Name="UpdateAvailable"
    Title="Update Available"
    Message="Please close this tip to apply required security updates to this application"
    Closing="InfoBar_Closing">
</muxc:InfoBar>
```

```csharp
public void InfoBar_Closing(InfoBar sender, InfoBarClosingEventArgs args)
{
    if (args.Reason == InfoBarCloseReason.CloseButton) {
        if (!ApplyUpdates()) {
            // could not apply updates - do not close
            args.Cancel = true;
        }
    }   
}
```

## <a name="recommendations"></a>권장 사항

### <a name="enter-and-exit-usability"></a>유용성 입력 및 종료
#### <a name="flashing-content"></a>깜박이는 콘텐츠
InfoBar가 화면에서 깜박이는 것을 방지하기 위해 보기에서 빠르게 나타나거나 사라지면 안 됩니다. 광과민성이 있는 사람들에게 시각적으로 반짝이는 것을 방지하고 애플리케이션의 유용성을 개선하세요.


앱 상태 조건을 통해 보기를 자동으로 들어오고 나가는 InfoBar의 경우, 콘텐츠가 연속적으로 빠르게 또는 여러 번 표시되거나 사라지는 것을 방지하기 위해 애플리케이션에 논리를 포함하는 것이 좋습니다. 그러나 일반적으로 이 컨트롤은 오래 표시되는 상태 메시지에 사용해야 합니다.


#### <a name="updating-the-infobar"></a>InfoBar 업데이트
컨트롤이 열리면 메시지 또는 심각도 업데이트와 같은 다양한 속성이 변경되어도 알림 이벤트가 발생하지 않습니다. 화면 판독기를 사용하는 사용자에게 InfoBar의 업데이트된 콘텐츠를 알리려면 컨트롤을 닫고 다시 열어 이벤트를 트리거하는 것이 좋습니다.


#### <a name="inline-messages-offsetting-content"></a>콘텐츠를 오프셋하는 인라인 메시지
다른 UI 컨텐츠와 인라인되는 InfoBar의 경우, 페이지의 나머지 부분이 요소 추가에 반응하는 방식을 염두에 둡니다.


상당한 높이의 InfoBar는 페이지 상의 다른 요소의 레이아웃을 크게 변경할 수 있습니다. InfoBar가 빠르게 특히 연속적으로 나타나거나 사라지는 경우, 사용자는 변화하는 시각적 상태를 혼동할 수 있습니다.


### <a name="color-and-icon"></a>색상 및 아이콘
사전 설정된 심각도 수준을 벗어난 색상 및 아이콘을 사용자 지정할 때 표준 아이콘 및 색상 집합의 의미에 대한 사용자 기대를 염두에 둡니다.


또한 사전 설정된 심각도 색상은 테마 변경, 고대비 모드, 색상 혼동 접근성 및 전경색과의 대비를 위해 이미 디자인되었습니다. 가능한 경우 이러한 색상을 사용하고 다양한 색상 상태 및 접근성 기능에 맞게 애플리케이션에 사용자 지정 논리를 포함하는 것이 좋습니다.


메시지가 명확하게 전달되고 사용자가 액세스할 수 있도록 [표준 아이콘](/windows/win32/uxguide/vis-std-icons) 및 [색상](/windows/win32/uxguide/vis-color)에 대한 UX 지침을 확인하세요.


#### <a name="severity"></a>심각도
 제목, 메시지 또는 사용자 지정 콘텐츠에서 전달된 정보와 일치하지 않는 알림에 대해서는 심각도 속성을 설정하지 마세요.
 

 함께 제공되는 정보는 해당 심각도를 사용하기 위해 다음 사항을 전달하는 것을 목표로 해야 합니다.
 - 오류: 발생한 오류 또는 문제입니다.
 - 경고: 나중에 문제를 일으킬 수 있는 조건입니다.
 - 성공: 장기 실행 및/또는 배경 작업이 완료되었습니다.
 - 기본: 사용자의 주의가 필요한 일반 정보입니다.

아이콘과 색상은 알림의 의미를 나타내는 유일한 UI 구성 요소가 되어서는 안 됩니다. 알림의 제목 및/또는 메시지에 텍스트를 포함하여 정보를 표시해야 합니다.


### <a name="message"></a>메시지 

알림의 텍스트는 모든 언어에서 길이가 일정하지 않습니다. 제목 및 메시지 속성의 경우 이로 인해 알림이 두 번째 줄로 확장될 수도 있습니다. 메시지 길이 또는 특정 언어로 설정된 다른 UI 요소를 기준으로 위치를 지정하지 않는 것이 좋습니다.

알림은 오른쪽에서 왼쪽으로(RTL) 또는 왼쪽에서 오른쪽으로(LTR)로 지역화된 경우 표준 미러링 동작을 따릅니다. 아이콘은 방향성이 있는 경우에만 미러링됩니다.

알림의 텍스트 지역화에 대한 자세한 내용은 [레이아웃 및 글꼴 조정, RTL 지원](/uwp/design/globalizing/adjust-layout-and-fonts--and-support-rtl)을 참조하세요.

## <a name="related-articles"></a>관련 문서

_ [대화 상자 및 플라이아웃](./dialogs-and-flyouts/index.md)
