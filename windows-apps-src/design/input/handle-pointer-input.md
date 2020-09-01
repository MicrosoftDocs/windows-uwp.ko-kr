---
Description: Windows 응용 프로그램에서 터치, 마우스, 펜/스타일러스, 터치 패드 등의 포인팅 장치에서 입력 데이터를 수신, 처리 및 관리 합니다.
title: 포인터 입력 처리
ms.assetid: BDBC9E33-4037-4671-9596-471DCF855C82
label: Handle pointer input
template: detail.hbs
keywords: 펜, 마우스, 터치 패드, 터치, 포인터, 입력, 사용자 상호 작용
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f544b73e069827f3c680db45797081605ce41b63
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173457"
---
# <a name="handle-pointer-input"></a>포인터 입력 처리

Windows 응용 프로그램의 포인팅 장치 (예: 터치, 마우스, 펜/스타일러스, 터치 패드)에서 입력 데이터를 수신, 처리 및 관리 합니다.

> [!Important]
> 명확 하 고 잘 정의 된 요구 사항이 있고 플랫폼 컨트롤에서 지 원하는 상호 작용이 시나리오를 지원 하지 않는 경우에만 사용자 지정 상호 작용을 만듭니다.  
> Windows 응용 프로그램에서 상호 작용 환경을 사용자 지정 하는 경우 사용자는 일관성 있고 직관적 이며 검색 가능 하 게 될 것으로 간주 합니다. 이러한 이유로 [플랫폼 컨트롤](../controls-and-patterns/controls-by-function.md)에서 지 원하는 사용자 지정 상호 작용을 모델링 하는 것이 좋습니다. 플랫폼 컨트롤은 표준 상호 작용, 애니메이션 된 물리학 효과, 시각적 피드백 및 내게 필요한 옵션을 비롯 한 전체 Windows 앱 사용자 상호 작용 환경을 제공 합니다. 

## <a name="important-apis"></a>중요 API
- [Windows.Devices.Input](/uwp/api/Windows.Devices.Input)
- [Windows.UI.Input](/uwp/api/Windows.UI.Core)
- [Windows.UI.Xaml.Input](/uwp/api/Windows.UI.Input)

## <a name="pointers"></a>포인터
대부분의 상호 작용 환경은 일반적으로 터치, 마우스, 펜/스타일러스, 터치 패드와 같은 입력 장치를 통해 상호 작용 하려는 개체를 식별 하는 사용자를 포함 합니다. 이러한 입력 장치에서 제공 하는 HID (raw 휴먼 인터페이스 장치) 데이터에 많은 공용 속성이 포함 되어 있기 때문에 데이터는 승격 되 고 통합 된 입력 스택으로 통합 되며 장치에 관계 없는 포인터 데이터로 노출 됩니다. 그러면 Windows 응용 프로그램에서 사용 되는 입력 장치에 대 한 걱정 없이이 데이터를 사용할 수 있습니다.

> [!NOTE]
> 또한 장치 관련 정보는 앱이 필요로 하는 원시 HID 데이터에서 승격 됩니다.

입력 스택의 각 입력 지점 (또는 연락처)은 다양 한 포인터 이벤트 처리기에서 [**PointerRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs) 매개 변수를 통해 노출 되는 [**포인터**](/uwp/api/Windows.UI.Xaml.Input.Pointer) 개체로 표시 됩니다. 다중 펜 또는 다중 터치 입력의 경우 각 연락처는 고유한 입력 포인터로 처리 됩니다.

## <a name="pointer-events"></a>포인터 이벤트

포인터 이벤트는 입력 장치 유형 및 검색 상태 (범위 내 또는 연락처)와 같은 기본 정보를 표시 하 고, 위치, 압력 및 접촉 geometry와 같은 확장 정보를 제공 합니다. 또한 사용자가 누른 마우스 단추 또는 펜 지우개 팁이 사용 되 고 있는지 여부와 같은 특정 장치 속성도 사용할 수 있습니다. 앱이 입력 장치와 해당 기능을 구분 해야 하는 경우 [입력 장치 식별](identify-input-devices.md)을 참조 하세요.

Windows 앱은 다음과 같은 포인터 이벤트를 수신할 수 있습니다.

> [!NOTE]
> 포인터 이벤트 처리기 내에서 해당 요소에 대해  [**CapturePointer**](/uwp/api/windows.ui.xaml.uielement.capturepointer) 를 호출 하 여 포인터 입력을 특정 UI 요소로 제한 합니다. 요소가 요소에 의해 캡처될 때 포인터가 개체의 경계 영역 밖으로 이동 하더라도 해당 개체만 포인터 입력 이벤트를 수신 합니다. **CapturePointer** 성공 하려면 [**IsInContact**](/uwp/api/windows.ui.xaml.input.pointer.isincontact) (마우스 단추 누름, 터치 또는 연락처의 스타일러스)이 true 여야 합니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">이벤트</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercanceled"><strong>PointerCanceled</strong></a></p></td>
<td align="left"><p>플랫폼에 의해 포인터가 취소 될 때 발생 합니다. 이 문제는 다음과 같은 경우에 발생할 수 있습니다.</p>
<ul>
<li>입력 표면의 범위 내에서 펜이 검색 되 면 터치 포인터가 취소 됩니다.</li>
<li>100 밀리초를 초과 하는 활성 연락처는 검색 되지 않습니다.</li>
<li>모니터/디스플레이 (해상도, 설정, 다중 mon 구성)가 변경 되었습니다.</li>
<li>데스크톱이 잠겨 있거나 사용자가 로그 오프 했습니다.</li>
<li>동시 연락처 수가 장치에서 지원 되는 수를 초과 했습니다.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost"><strong>PointerCaptureLost</strong></a></p></td>
<td align="left"><p>다른 UI 요소에서 포인터를 캡처하거나 포인터가 해제 되거나 프로그래밍 방식으로 다른 포인터가 캡처될 때 발생 합니다.</p>
<div class="alert">
<strong>참고</strong>    해당 포인터 캡처 이벤트가 없습니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered"><strong>PointerEntered 됨</strong></a></p></td>
<td align="left"><p>포인터가 요소의 경계 영역에 들어갈 때 발생합니다. 이는 터치, 터치 패드, 마우스 및 펜 입력에 대해 약간 다른 방법으로 발생할 수 있습니다.</p>
<ul>
<li>터치를 사용 하려면 요소에 대 한 직접 터치에서 또는 요소의 경계 영역으로 이동 하 여이 이벤트를 발생 시킵니다.</li>
<li>마우스 및 터치 패드에는 항상 표시 되는 화상 커서가 있으며 마우스 또는 터치 패드 단추를 누르지 않아도이 이벤트가 발생 합니다.</li>
<li>터치와 마찬가지로 펜은 요소에 대 한 직접 펜을 사용 하거나 요소의 경계 영역으로 이동 하 여이 이벤트를 발생 시킵니다. 그러나 펜에는 true 인 경우이 이벤트를 발생 시키는 가리키기 상태 (<a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointer.isinrange">IsInRange</a>)도 있습니다.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited"><strong>PointerExited</strong></a></p></td>
<td align="left"><p>포인터가 요소의 경계 영역을 벗어나면 발생 합니다. 이는 터치, 터치 패드, 마우스 및 펜 입력에 대해 약간 다른 방법으로 발생할 수 있습니다.</p>
<ul>
<li>터치를 사용 하려면 손가락 연락처가 필요 하며 포인터가 요소의 경계 영역 밖으로 이동할 때이 이벤트를 발생 시킵니다.</li>
<li>마우스 및 터치 패드에는 항상 표시 되는 화상 커서가 있으며 마우스 또는 터치 패드 단추를 누르지 않아도이 이벤트가 발생 합니다.</li>
<li>터치와 마찬가지로 펜은 요소의 경계 영역 밖으로 이동할 때이 이벤트를 발생 시킵니다. 그러나 상태가 true에서 false로 변경 되 면이 이벤트를 발생 시키는 가리키기 상태 (<a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointer.isinrange">IsInRange</a>)도 있습니다.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved"><strong>PointerMoved 됨</strong></a></p></td>
<td align="left"><p>요소의 경계 영역 내에서 포인터의 좌표, 단추 상태, 압력, 기울기 또는 접촉 기 하 도형 (예: 너비 및 높이)이 변경 될 때 발생 합니다. 이는 터치, 터치 패드, 마우스 및 펜 입력에 대해 약간 다른 방법으로 발생할 수 있습니다.</p>
<ul>
<li>터치를 사용 하려면 손가락 연락처가 필요 하 고 요소의 경계 영역 내에 있는 연락처에 있는 경우에만이 이벤트를 발생 시킵니다.</li>
<li>마우스 및 터치 패드에는 항상 표시 되는 화상 커서가 있으며 마우스 또는 터치 패드 단추를 누르지 않아도이 이벤트가 발생 합니다.</li>
<li>터치와 마찬가지로 펜은 요소의 경계 영역 내에서 연락할 때이 이벤트를 발생 시킵니다. 그러나 펜에는 요소에 대 한 경계 영역 내에서 true 및이 이벤트가 발생 하는 가리키기 상태 (<a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointer.isinrange">IsInRange</a>)도 있습니다.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed"><strong>PointerPressed</strong></a></p></td>
<td align="left"><p>요소가 요소의 경계 영역 내에서 누름 동작 (예: 터치 다운, 마우스 단추 누름, 펜 아래로 또는 터치 패드 단추 누르기)을 나타내는 경우에 발생 합니다.</p>
<p><a href="/uwp/api/windows.ui.xaml.uielement.capturepointer">CapturePointer</a> 는이 이벤트에 대 한 처리기에서 호출 해야 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased"><strong>PointerReleased</strong></a></p></td>
<td align="left"><p>포인터가 요소의 경계 영역 내에서 터치, 마우스 단추 위로, 펜 위, 터치 패드 단추 등의 릴리스 작업을 나타내는 경우 또는 경계 영역 외부에서 포인터가 캡처된 경우 발생 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged"><strong>PointerWheelChanged</strong></a></p></td>
<td align="left"><p>마우스 휠이 회전할 때 발생합니다.</p>
<p>마우스 입력은 마우스 입력이 처음 감지 될 때 할당 된 단일 포인터와 연결 됩니다. 마우스 단추 (왼쪽, 휠 또는 오른쪽)를 클릭 하면 <a href="/uwp/api/windows.ui.xaml.uielement.pointermoved">Pointermoved</a> 이벤트를 통해 포인터와 해당 단추 사이에 보조 연결이 생성 됩니다.</p></td>
</tr>
</tbody>
</table> 

## <a name="pointer-event-example"></a>Pointer 이벤트 예제

다음은 여러 포인터에 대 한 이벤트를 수신 하 고 처리 하는 방법 및 연결 된 포인터에 대 한 다양 한 속성을 가져오는 방법을 보여 주는 기본 포인터 추적 앱의 일부 코드 조각입니다.

![포인터 응용 프로그램 UI](images/pointers/pointers1.gif)

**포인터 입력 샘플에서이 샘플 다운로드 [(기본)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers.zip)**

### <a name="create-the-ui"></a>UI 만들기

이 예제에서는 포인터 입력을 사용 [Rectangle](/uwp/api/windows.ui.xaml.shapes.rectangle) 하 `Target` 는 개체에 사각형 ()을 사용 합니다. 포인터 상태가 변경 되 면 대상의 색이 변경 됩니다.

각 포인터에 대 한 세부 정보는 포인터가 이동할 때 포인터 뒤에 오는 부동 [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 에 표시 됩니다. 포인터 이벤트 자체는 사각형 오른쪽의 [RichTextBlock](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) 에 보고 됩니다.

이 예제에서 UI에 대 한 Extensible Application Markup Language (XAML)입니다. 

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="*" />
        <ColumnDefinition Width="250"/>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="320" />
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Canvas Name="Container" 
            Grid.Column="0"
            Grid.Row="1"
            HorizontalAlignment="Center" 
            VerticalAlignment="Center" 
            Margin="245,0" 
            Height="320"  Width="640">
        <Rectangle Name="Target" 
                    Fill="#FF0000" 
                    Stroke="Black" 
                    StrokeThickness="0"
                    Height="320" Width="640" />
    </Canvas>
    <Grid Grid.Column="1" Grid.Row="0" Grid.RowSpan="3">
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        <Button Name="buttonClear" 
                Grid.Row="0"
                Content="Clear"
                Foreground="White"
                HorizontalAlignment="Stretch" 
                VerticalAlignment="Stretch">
        </Button>
        <ScrollViewer Name="eventLogScrollViewer" Grid.Row="1" 
                        VerticalScrollMode="Auto" 
                        Background="Black">                
            <RichTextBlock Name="eventLog"  
                        TextWrapping="Wrap" 
                        Foreground="#FFFFFF" 
                        ScrollViewer.VerticalScrollBarVisibility="Visible" 
                        ScrollViewer.HorizontalScrollBarVisibility="Disabled"
                        Grid.ColumnSpan="2">
            </RichTextBlock>
        </ScrollViewer>
    </Grid>
</Grid>
```

### <a name="listen-for-pointer-events"></a>포인터 이벤트 수신 대기

대부분의 경우 이벤트 처리기의 [**PointerRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs) 을 통해 포인터 정보를 가져오는 것이 좋습니다.

이벤트 인수가 필요한 포인터 정보를 노출 하지 않는 경우 [**PointerRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs)의 [**getcurrentpoint**](/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getcurrentpoint) 및 [**GetIntermediatePoints**](/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getintermediatepoints) 메서드를 통해 노출 된 확장 [**pointerpoint**](/uwp/api/Windows.UI.Input.PointerPoint) 정보에 대 한 액세스 권한을 얻을 수 있습니다.

다음 코드에서는 각 활성 포인터를 추적 하기 위해 전역 사전 개체를 설정 하 고 대상 개체에 대 한 다양 한 포인터 이벤트 수신기를 식별 합니다.

```CSharp
// Dictionary to maintain information about each active pointer. 
// An entry is added during PointerPressed/PointerEntered events and removed 
// during PointerReleased/PointerCaptureLost/PointerCanceled/PointerExited events.
Dictionary<uint, Windows.UI.Xaml.Input.Pointer> pointers;

public MainPage()
{
    this.InitializeComponent();

    // Initialize the dictionary.
    pointers = new Dictionary<uint, Windows.UI.Xaml.Input.Pointer>();

    // Declare the pointer event handlers.
    Target.PointerPressed += 
        new PointerEventHandler(Target_PointerPressed);
    Target.PointerEntered += 
        new PointerEventHandler(Target_PointerEntered);
    Target.PointerReleased += 
        new PointerEventHandler(Target_PointerReleased);
    Target.PointerExited += 
        new PointerEventHandler(Target_PointerExited);
    Target.PointerCanceled += 
        new PointerEventHandler(Target_PointerCanceled);
    Target.PointerCaptureLost += 
        new PointerEventHandler(Target_PointerCaptureLost);
    Target.PointerMoved += 
        new PointerEventHandler(Target_PointerMoved);
    Target.PointerWheelChanged += 
        new PointerEventHandler(Target_PointerWheelChanged);

    buttonClear.Click += 
        new RoutedEventHandler(ButtonClear_Click);
}
```

### <a name="handle-pointer-events"></a>포인터 이벤트 처리

다음으로, UI 피드백을 사용 하 여 기본 포인터 이벤트 처리기를 보여 줍니다.

-   이 처리기는 [**Pointerpressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed) 이벤트를 관리 합니다. 이벤트를 이벤트 로그에 추가 하 고, 활성 포인터 사전에 포인터를 추가 하 고, 포인터 세부 정보를 표시 합니다.

    > [!NOTE]
    > [**Pointerpressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed) 및 [**pointerpressed**](/uwp/api/windows.ui.xaml.uielement.pointerreleased) 이벤트는 항상 쌍으로 발생 하지 않습니다. 앱은 포인터를 종료할 수 있는 이벤트를 수신 하 고 처리 해야 합니다 (예: [**Pointerexited**](/uwp/api/windows.ui.xaml.uielement.pointerexited), [**pointerexited**](/uwp/api/windows.ui.xaml.uielement.pointercanceled)및 [**PointerCaptureLost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost)).      

```csharp
/// <summary>
/// The pointer pressed event handler.
/// PointerPressed and PointerReleased don't always occur in pairs. 
/// Your app should listen for and handle any event that can conclude 
/// a pointer down (PointerExited, PointerCanceled, PointerCaptureLost).
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
void Target_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Down: " + ptrPt.PointerId);

    // Lock the pointer to the target.
    Target.CapturePointer(e.Pointer);

    // Update event log.
    UpdateEventLog("Pointer captured: " + ptrPt.PointerId);

    // Check if pointer exists in dictionary (ie, enter occurred prior to press).
    if (!pointers.ContainsKey(ptrPt.PointerId))
    {
        // Add contact to dictionary.
        pointers[ptrPt.PointerId] = e.Pointer;
    }

    // Change background color of target when pointer contact detected.
    Target.Fill = new SolidColorBrush(Windows.UI.Colors.Green);

    // Display pointer details.
    CreateInfoPop(ptrPt);
}
```

-   이 처리기는 [**Pointerentered**](/uwp/api/windows.ui.xaml.uielement.pointerentered) 이벤트를 관리 합니다. 이벤트를 이벤트 로그에 추가 하 고 포인터 컬렉션에 포인터를 추가 하 고 포인터 세부 정보를 표시 합니다.

```csharp
/// <summary>
/// The pointer entered event handler.
/// We do not capture the pointer on this event.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerEntered(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Entered: " + ptrPt.PointerId);

    // Check if pointer already exists (if enter occurred prior to down).
    if (!pointers.ContainsKey(ptrPt.PointerId))
    {
        // Add contact to dictionary.
        pointers[ptrPt.PointerId] = e.Pointer;
    }

    if (pointers.Count == 0)
    {
        // Change background color of target when pointer contact detected.
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
    }

    // Display pointer details.
    CreateInfoPop(ptrPt);
}
```

-   이 처리기는 [**Pointermoved**](/uwp/api/windows.ui.xaml.uielement.pointermoved) 이벤트를 관리 합니다. 이벤트를 이벤트 로그에 추가 하 고 포인터 세부 정보를 업데이트 합니다.

    > [!Important]
    > 마우스 입력은 마우스 입력이 처음 감지 될 때 할당 된 단일 포인터와 연결 됩니다. 마우스 단추 (왼쪽, 휠 또는 오른쪽)를 클릭 하면 [**Pointerpressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed) 이벤트를 통해 포인터와 해당 단추 사이에 보조 연결이 생성 됩니다. [**Pointerreleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased) 이벤트는 동일한 마우스 단추를 눌렀다 놓은 경우에만 발생 합니다 (이 이벤트가 완료 될 때까지 다른 단추는 포인터와 연결 될 수 없음). 이 배타적 연결로 인해 다른 마우스 단추 클릭은 [**Pointermoved**](/uwp/api/windows.ui.xaml.uielement.pointermoved) 이벤트를 통해 라우팅됩니다.     

```csharp
/// <summary>
/// The pointer moved event handler.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerMoved(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Multiple, simultaneous mouse button inputs are processed here.
    // Mouse input is associated with a single pointer assigned when 
    // mouse input is first detected. 
    // Clicking additional mouse buttons (left, wheel, or right) during 
    // the interaction creates secondary associations between those buttons 
    // and the pointer through the pointer pressed event. 
    // The pointer released event is fired only when the last mouse button 
    // associated with the interaction (not necessarily the initial button) 
    // is released. 
    // Because of this exclusive association, other mouse button clicks are 
    // routed through the pointer move event.          
    if (ptrPt.PointerDevice.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse)
    {
        if (ptrPt.Properties.IsLeftButtonPressed)
        {
            UpdateEventLog("Left button: " + ptrPt.PointerId);
        }
        if (ptrPt.Properties.IsMiddleButtonPressed)
        {
            UpdateEventLog("Wheel button: " + ptrPt.PointerId);
        }
        if (ptrPt.Properties.IsRightButtonPressed)
        {
            UpdateEventLog("Right button: " + ptrPt.PointerId);
        }
    }

    // Display pointer details.
    UpdateInfoPop(ptrPt);
}
```

-   이 처리기는 [**PointerWheelChanged**](/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged) 이벤트를 관리 합니다. 이벤트를 이벤트 로그에 추가 하 고 포인터 배열 (필요한 경우)에 포인터를 추가 하 고 포인터 세부 정보를 표시 합니다.

```csharp
/// <summary>
/// The pointer wheel event handler.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Mouse wheel: " + ptrPt.PointerId);

    // Check if pointer already exists (for example, enter occurred prior to wheel).
    if (!pointers.ContainsKey(ptrPt.PointerId))
    {
        // Add contact to dictionary.
        pointers[ptrPt.PointerId] = e.Pointer;
    }

    // Display pointer details.
    CreateInfoPop(ptrPt);
}
```

-   이 처리기는 디지타이저와의 연결이 종료 되는 [**Pointerreleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased) 이벤트를 관리 합니다. 이벤트를 이벤트 로그에 추가 하 고 포인터 컬렉션에서 포인터를 제거 하 고 포인터 세부 정보를 업데이트 합니다.

```csharp
/// <summary>
/// The pointer released event handler.
/// PointerPressed and PointerReleased don't always occur in pairs. 
/// Your app should listen for and handle any event that can conclude 
/// a pointer down (PointerExited, PointerCanceled, PointerCaptureLost).
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
void Target_PointerReleased(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Up: " + ptrPt.PointerId);

    // If event source is mouse or touchpad and the pointer is still 
    // over the target, retain pointer and pointer details.
    // Return without removing pointer from pointers dictionary.
    // For this example, we assume a maximum of one mouse pointer.
    if (ptrPt.PointerDevice.PointerDeviceType != Windows.Devices.Input.PointerDeviceType.Mouse)
    {
        // Update target UI.
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);

        DestroyInfoPop(ptrPt);

        // Remove contact from dictionary.
        if (pointers.ContainsKey(ptrPt.PointerId))
        {
            pointers[ptrPt.PointerId] = null;
            pointers.Remove(ptrPt.PointerId);
        }

        // Release the pointer from the target.
        Target.ReleasePointerCapture(e.Pointer);

        // Update event log.
        UpdateEventLog("Pointer released: " + ptrPt.PointerId);
    }
    else
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
    }
}
```

-   이 처리기는 디지타이저와의 연결이 유지 되는 경우 [**Pointerexited**](/uwp/api/windows.ui.xaml.uielement.pointerexited) 이벤트를 관리 합니다. 이벤트를 이벤트 로그에 추가 하 고 포인터 배열에서 포인터를 제거 하 고 포인터 세부 정보를 업데이트 합니다.

```csharp
/// <summary>
/// The pointer exited event handler.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerExited(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Pointer exited: " + ptrPt.PointerId);

    // Remove contact from dictionary.
    if (pointers.ContainsKey(ptrPt.PointerId))
    {
        pointers[ptrPt.PointerId] = null;
        pointers.Remove(ptrPt.PointerId);
    }

    if (pointers.Count == 0)
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);
    }

    // Update the UI and pointer details.
    DestroyInfoPop(ptrPt);
}
```

-   이 처리기는 [**Pointercanceled**](/uwp/api/windows.ui.xaml.uielement.pointercanceled) 이벤트를 관리 합니다. 이벤트를 이벤트 로그에 추가 하 고 포인터 배열에서 포인터를 제거 하 고 포인터 세부 정보를 업데이트 합니다.

```csharp
/// <summary>
/// The pointer canceled event handler.
/// Fires for various reasons, including: 
///    - Touch contact canceled by pen coming into range of the surface.
///    - The device doesn't report an active contact for more than 100ms.
///    - The desktop is locked or the user logged off. 
///    - The number of simultaneous contacts exceeded the number supported by the device.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerCanceled(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Pointer canceled: " + ptrPt.PointerId);

    // Remove contact from dictionary.
    if (pointers.ContainsKey(ptrPt.PointerId))
    {
        pointers[ptrPt.PointerId] = null;
        pointers.Remove(ptrPt.PointerId);
    }

    if (pointers.Count == 0)
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
    }

    DestroyInfoPop(ptrPt);
}
```

-   이 처리기는 [**PointerCaptureLost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost) 이벤트를 관리 합니다. 이벤트를 이벤트 로그에 추가 하 고 포인터 배열에서 포인터를 제거 하 고 포인터 세부 정보를 업데이트 합니다.

    > [!NOTE]
    > [**PointerCaptureLost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost) 는 [**pointerreleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased)대신 발생할 수 있습니다. 포인터 캡처는 사용자 상호 작용, 다른 포인터의 프로그래밍 방식 캡처, [**Pointerreleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased)호출 등 다양 한 이유로 손실 될 수 있습니다.     

```csharp
/// <summary>
/// The pointer capture lost event handler.
/// Fires for various reasons, including: 
///    - User interactions
///    - Programmatic capture of another pointer
///    - Captured pointer was deliberately released
// PointerCaptureLost can fire instead of PointerReleased. 
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerCaptureLost(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Pointer capture lost: " + ptrPt.PointerId);

    if (pointers.Count == 0)
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
    }

    // Remove contact from dictionary.
    if (pointers.ContainsKey(ptrPt.PointerId))
    {
        pointers[ptrPt.PointerId] = null;
        pointers.Remove(ptrPt.PointerId);
    }

    DestroyInfoPop(ptrPt);
}
```

### <a name="get-pointer-properties"></a>포인터 속성 가져오기

앞에서 설명한 것 처럼 PointerRoutedEventArgs의 [**Getcurrentpoint**](/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getcurrentpoint) 및 [**GetIntermediatePoints**](/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getintermediatepoints) 메서드를 통해 얻은 [**PointerRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs)개체에서 대부분의 확장 된 포인터 정보를 가져와야 [**합니다.**](/uwp/api/Windows.UI.Input.PointerPoint) 다음 코드 조각에서는 방법을 보여 줍니다.

-   먼저 각 포인터에 대 한 새 [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 을 만듭니다.

```csharp
/// <summary>
/// Create the pointer info popup.
/// </summary>
/// <param name="ptrPt">Reference to the input pointer.</param>
void CreateInfoPop(PointerPoint ptrPt)
{
    TextBlock pointerDetails = new TextBlock();
    pointerDetails.Name = ptrPt.PointerId.ToString();
    pointerDetails.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
    pointerDetails.Text = QueryPointer(ptrPt);

    TranslateTransform x = new TranslateTransform();
    x.X = ptrPt.Position.X + 20;
    x.Y = ptrPt.Position.Y + 20;
    pointerDetails.RenderTransform = x;

    Container.Children.Add(pointerDetails);
}
```

-   그런 다음 해당 포인터와 연결 된 기존 [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 의 포인터 정보를 업데이트 하는 방법을 제공 합니다.

```csharp
/// <summary>
/// Update the pointer info popup.
/// </summary>
/// <param name="ptrPt">Reference to the input pointer.</param>
void UpdateInfoPop(PointerPoint ptrPt)
{
    foreach (var pointerDetails in Container.Children)
    {
        if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
        {
            TextBlock textBlock = (TextBlock)pointerDetails;
            if (textBlock.Name == ptrPt.PointerId.ToString())
            {
                // To get pointer location details, we need extended pointer info.
                // We get the pointer info through the getCurrentPoint method
                // of the event argument. 
                TranslateTransform x = new TranslateTransform();
                x.X = ptrPt.Position.X + 20;
                x.Y = ptrPt.Position.Y + 20;
                pointerDetails.RenderTransform = x;
                textBlock.Text = QueryPointer(ptrPt);
            }
        }
    }
}
```

-   마지막으로 다양 한 포인터 속성을 쿼리 합니다.

```csharp
/// <summary>
/// Get pointer details.
/// </summary>
/// <param name="ptrPt">Reference to the input pointer.</param>
/// <returns>A string composed of pointer details.</returns>
String QueryPointer(PointerPoint ptrPt)
{
    String details = "";

    switch (ptrPt.PointerDevice.PointerDeviceType)
    {
        case Windows.Devices.Input.PointerDeviceType.Mouse:
            details += "\nPointer type: mouse";
            break;
        case Windows.Devices.Input.PointerDeviceType.Pen:
            details += "\nPointer type: pen";
            if (ptrPt.IsInContact)
            {
                details += "\nPressure: " + ptrPt.Properties.Pressure;
                details += "\nrotation: " + ptrPt.Properties.Orientation;
                details += "\nTilt X: " + ptrPt.Properties.XTilt;
                details += "\nTilt Y: " + ptrPt.Properties.YTilt;
                details += "\nBarrel button pressed: " + ptrPt.Properties.IsBarrelButtonPressed;
            }
            break;
        case Windows.Devices.Input.PointerDeviceType.Touch:
            details += "\nPointer type: touch";
            details += "\nrotation: " + ptrPt.Properties.Orientation;
            details += "\nTilt X: " + ptrPt.Properties.XTilt;
            details += "\nTilt Y: " + ptrPt.Properties.YTilt;
            break;
        default:
            details += "\nPointer type: n/a";
            break;
    }

    GeneralTransform gt = Target.TransformToVisual(this);
    Point screenPoint;

    screenPoint = gt.TransformPoint(new Point(ptrPt.Position.X, ptrPt.Position.Y));
    details += "\nPointer Id: " + ptrPt.PointerId.ToString() +
        "\nPointer location (target): " + Math.Round(ptrPt.Position.X) + ", " + Math.Round(ptrPt.Position.Y) +
        "\nPointer location (container): " + Math.Round(screenPoint.X) + ", " + Math.Round(screenPoint.Y);

    return details;
}
```

## <a name="primary-pointer"></a>기본 포인터
터치 디지타이저 또는 터치 패드와 같은 일부 입력 장치는 마우스 또는 펜의 일반적인 단일 포인터 보다 더 많이 지원 합니다. 대부분의 경우 Surface Hub는 두 개의 펜 입력을 지원 합니다. 

**[Pointerpointerproperties](/uwp/api/windows.ui.input.pointerpointproperties)** 클래스의 읽기 전용 **[IsPrimary](/uwp/api/windows.ui.input.pointerpointproperties.IsPrimary)** 속성을 사용 하 여 단일 기본 포인터를 식별 하 고 구분 합니다. 기본 포인터는 항상 입력 시퀀스 중에 검색 되는 첫 번째 포인터입니다. 

기본 포인터를 식별 하 여 마우스 또는 펜 입력을 에뮬레이트 하거나, 상호 작용을 사용자 지정 하거나, 기타 특정 기능 또는 UI를 제공 하는 데 사용할 수 있습니다.

> [!NOTE]
> 입력 시퀀스 중에 기본 포인터가 해제, 취소 또는 손실 된 경우 새 입력 시퀀스가 시작 될 때까지 기본 입력 포인터가 생성 되지 않습니다. 모든 포인터가 해제, 취소 또는 손실 될 때 입력 시퀀스가 종료 됩니다.

## <a name="primary-pointer-animation-example"></a>기본 포인터 애니메이션 예제

이러한 코드 조각은 사용자가 응용 프로그램의 포인터 입력을 구분 하는 데 도움이 되는 특별 한 시각적 피드백을 제공 하는 방법을 보여 줍니다.

이 특정 앱은 색과 애니메이션을 모두 사용 하 여 기본 포인터를 강조 표시 합니다.

![애니메이션 시각적 피드백을 포함 하는 포인터 응용 프로그램](images/pointers/pointers-usercontrol-animation.gif)

**[포인터 입력 샘플 (UserControl with animation)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip) 에서이 샘플 다운로드**

### <a name="visual-feedback"></a>시각적 피드백

각 포인터가 캔버스에 있는 위치를 강조 하 고 **[Storyboard](/uwp/api/windows.ui.xaml.media.animation.storyboard)** 를 사용 하 여 기본 포인터에 해당 하는 타원에 애니메이션 효과를 주는 XAML **[타원](/uwp/api/windows.ui.xaml.shapes.ellipse)** 개체를 기반으로 **[UserControl](/uwp/api/windows.ui.xaml.controls.usercontrol)** 을 정의 합니다.

**XAML은 다음과 같습니다.**

```xaml
<UserControl
    x:Class="UWP_Pointers.PointerEllipse"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:UWP_Pointers"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="100"
    d:DesignWidth="100">

    <UserControl.Resources>
        <Style x:Key="EllipseStyle" TargetType="Ellipse">
            <Setter Property="Transitions">
                <Setter.Value>
                    <TransitionCollection>
                        <ContentThemeTransition/>
                    </TransitionCollection>
                </Setter.Value>
            </Setter>
        </Style>
        
        <Storyboard x:Name="myStoryboard">
            <!-- Animates the value of a Double property between 
            two target values using linear interpolation over the 
            specified Duration. -->
            <DoubleAnimation
              Storyboard.TargetName="ellipse"
              Storyboard.TargetProperty="(RenderTransform).(ScaleTransform.ScaleY)"  
              Duration="0:0:1" 
              AutoReverse="True" 
              RepeatBehavior="Forever" From="1.0" To="1.4">
            </DoubleAnimation>

            <!-- Animates the value of a Double property between 
            two target values using linear interpolation over the 
            specified Duration. -->
            <DoubleAnimation
              Storyboard.TargetName="ellipse"
              Storyboard.TargetProperty="(RenderTransform).(ScaleTransform.ScaleX)"  
              Duration="0:0:1" 
              AutoReverse="True" 
              RepeatBehavior="Forever" From="1.0" To="1.4">
            </DoubleAnimation>

            <!-- Animates the value of a Color property between 
            two target values using linear interpolation over the 
            specified Duration. -->
            <ColorAnimation 
                Storyboard.TargetName="ellipse" 
                EnableDependentAnimation="True" 
                Storyboard.TargetProperty="(Fill).(SolidColorBrush.Color)" 
                From="White" To="Red"  Duration="0:0:1" 
                AutoReverse="True" RepeatBehavior="Forever"/>
        </Storyboard>
    </UserControl.Resources>

    <Grid x:Name="CompositionContainer">
        <Ellipse Name="ellipse" 
        StrokeThickness="2" 
        Width="{x:Bind Diameter}" 
        Height="{x:Bind Diameter}"  
        Style="{StaticResource EllipseStyle}" />
    </Grid>
</UserControl>
```

코드 숨김이 여기에 나와 있습니다.
```csharp
using Windows.Foundation;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

// The User Control item template is documented at 
// https://go.microsoft.com/fwlink/?LinkId=234236

namespace UWP_Pointers
{
    /// <summary>
    /// Pointer feedback object.
    /// </summary>
    public sealed partial class PointerEllipse : UserControl
    {
        // Reference to the application canvas.
        Canvas canvas;

        /// <summary>
        /// Ellipse UI for pointer feedback.
        /// </summary>
        /// <param name="c">The drawing canvas.</param>
        public PointerEllipse(Canvas c)
        {
            this.InitializeComponent();
            canvas = c;
        }

        /// <summary>
        /// Gets or sets the pointer Id to associate with the PointerEllipse object.
        /// </summary>
        public uint PointerId
        {
            get { return (uint)GetValue(PointerIdProperty); }
            set { SetValue(PointerIdProperty, value); }
        }
        // Using a DependencyProperty as the backing store for PointerId.  
        // This enables animation, styling, binding, etc...
        public static readonly DependencyProperty PointerIdProperty =
            DependencyProperty.Register("PointerId", typeof(uint), 
                typeof(PointerEllipse), new PropertyMetadata(null));


        /// <summary>
        /// Gets or sets whether the associated pointer is Primary.
        /// </summary>
        public bool PrimaryPointer
        {
            get { return (bool)GetValue(PrimaryPointerProperty); }
            set
            {
                SetValue(PrimaryPointerProperty, value);
            }
        }
        // Using a DependencyProperty as the backing store for PrimaryPointer.  
        // This enables animation, styling, binding, etc...
        public static readonly DependencyProperty PrimaryPointerProperty =
            DependencyProperty.Register("PrimaryPointer", typeof(bool), 
                typeof(PointerEllipse), new PropertyMetadata(false));


        /// <summary>
        /// Gets or sets the ellipse style based on whether the pointer is Primary.
        /// </summary>
        public bool PrimaryEllipse 
        {
            get { return (bool)GetValue(PrimaryEllipseProperty); }
            set
            {
                SetValue(PrimaryEllipseProperty, value);
                if (value)
                {
                    SolidColorBrush fillBrush = 
                        (SolidColorBrush)Application.Current.Resources["PrimaryFillBrush"];
                    SolidColorBrush strokeBrush = 
                        (SolidColorBrush)Application.Current.Resources["PrimaryStrokeBrush"];

                    ellipse.Fill = fillBrush;
                    ellipse.Stroke = strokeBrush;
                    ellipse.RenderTransform = new CompositeTransform();
                    ellipse.RenderTransformOrigin = new Point(.5, .5);
                    myStoryboard.Begin();
                }
                else
                {
                    SolidColorBrush fillBrush = 
                        (SolidColorBrush)Application.Current.Resources["SecondaryFillBrush"];
                    SolidColorBrush strokeBrush = 
                        (SolidColorBrush)Application.Current.Resources["SecondaryStrokeBrush"];
                    ellipse.Fill = fillBrush;
                    ellipse.Stroke = strokeBrush;
                }
            }
        }
        // Using a DependencyProperty as the backing store for PrimaryEllipse.  
        // This enables animation, styling, binding, etc...
        public static readonly DependencyProperty PrimaryEllipseProperty =
            DependencyProperty.Register("PrimaryEllipse", 
                typeof(bool), typeof(PointerEllipse), new PropertyMetadata(false));


        /// <summary>
        /// Gets or sets the diameter of the PointerEllipse object.
        /// </summary>
        public int Diameter
        {
            get { return (int)GetValue(DiameterProperty); }
            set { SetValue(DiameterProperty, value); }
        }
        // Using a DependencyProperty as the backing store for Diameter.  This enables animation, styling, binding, etc...
        public static readonly DependencyProperty DiameterProperty =
            DependencyProperty.Register("Diameter", typeof(int), 
                typeof(PointerEllipse), new PropertyMetadata(120));
    }
}
```

### <a name="create-the-ui"></a>UI 만들기
이 예제의 UI는 포인터를 추적 하 여 포인터 카운터와 기본 포인터 식별자를 포함 하는 헤더 표시줄과 함께 포인터를 추적 하 고 포인터 표시기와 기본 포인터 애니메이션 (해당 하는 경우)을 렌더링 하는 입력 **[캔버스로](/uwp/api/windows.ui.xaml.controls.canvas)** 제한 됩니다.

다음은 MainPage xaml입니다.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" 
                Orientation="Horizontal" 
                Grid.Row="0">
        <StackPanel.Transitions>
            <TransitionCollection>
                <AddDeleteThemeTransition/>
            </TransitionCollection>
        </StackPanel.Transitions>
        <TextBlock x:Name="Header" 
                    Text="Basic pointer tracking sample - IsPrimary" 
                    Style="{ThemeResource HeaderTextBlockStyle}" 
                    Margin="10,0,0,0" />
        <TextBlock x:Name="PointerCounterLabel"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="Number of pointers: " 
                    Margin="50,0,0,0"/>
        <TextBlock x:Name="PointerCounter"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="0" 
                    Margin="10,0,0,0"/>
        <TextBlock x:Name="PointerPrimaryLabel"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="Primary: " 
                    Margin="50,0,0,0"/>
        <TextBlock x:Name="PointerPrimary"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="n/a" 
                    Margin="10,0,0,0"/>
    </StackPanel>
    
    <Grid Grid.Row="1">
        <!--The canvas where we render the pointer UI.-->
        <Canvas x:Name="pointerCanvas"/>
    </Grid>
</Grid>
```

### <a name="handle-pointer-events"></a>포인터 이벤트 처리

마지막으로 MainPage.xaml.cs 코드 숨김으로 기본 포인터 이벤트 처리기를 정의 합니다. 이전 예제에서 기본 사항을 다룬 바와 같이 여기서는 코드를 재현 하지 않지만 [포인터 입력 샘플 (UserControl with animation)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip)에서 작업 샘플을 다운로드할 수 있습니다.

## <a name="related-articles"></a>관련된 문서

### <a name="topic-samples"></a>토픽 샘플

- [포인터 입력 샘플 (기본)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers.zip)
- [포인터 입력 샘플 (애니메이션 사용 UserControl)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip)

### <a name="other-samples"></a>기타 샘플

- [기본 입력 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [짧은 대기 시간 입력 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [사용자 상호 작용 모드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [포커스 화면 효과 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>보관 샘플

- [Input: XAML 사용자 입력 이벤트 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [입력: 장치 기능 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Input: 조작 및 제스처 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [입력: 터치 적중 테스트 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [XAML 스크롤, 패닝 및 확대/축소 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [입력: 간소화 된 잉크 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)