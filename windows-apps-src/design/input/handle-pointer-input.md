---
author: Karl-Bridge-Microsoft
Description: Receive, process, and manage input data from pointing devices such as touch, mouse, pen/stylus, and touchpad, in your Universal Windows Platform (UWP) applications.
title: 포인터 입력 처리
ms.assetid: BDBC9E33-4037-4671-9596-471DCF855C82
label: Handle pointer input
template: detail.hbs
keywords: 펜, 마우스, 터치 패드, 터치, 포인터, 입력, 사용자 조작
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ba685f30eb0cf94314996587073a82440cf6c951
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6975418"
---
# <a name="handle-pointer-input"></a>포인터 입력 처리

UWP(유니버설 Windows 플랫폼) 응용 프로그램에서 터치, 마우스, 펜/스타일러스, 터치 패드 등의 포인팅 장치에서 입력 데이터를 수신하고, 처리하고, 관리합니다.

> [!Important]
> 요구 사항이 명확하게 잘 정의되어 있으며 플랫폼 컨트롤에서 지원하는 제스처가 시나리오를 지원하지 않는 경우에만 사용자 지정 조작을 만드세요.  
> Windows 응용 프로그램의 조작 환경을 사용자 지정하는 경우, 사용자들을 해당 사항이 일관되고, 직관적이며, 검색 가능할 것으로 기대합니다. 이러한 이유로 [플랫폼 컨트롤](../controls-and-patterns/controls-by-function.md)에서 지원하는 항목에 대해 사용자 지정 조작을 모델링하는 것이 좋습니다. 플랫폼 컨트롤은 표준 조작, 애니메이션 효과를 준 물리적 효과, 시각적 피드백 및 접근성을 비롯하여 UWP(유니버설 Windows 플랫폼) 사용자 조작 환경 전체를 제공합니다. 

## <a name="important-apis"></a>중요 API
- [Windows.Devices.Input](https://msdn.microsoft.com/library/windows/apps/br225648)
- [Windows.UI.Input](https://msdn.microsoft.com/library/windows/apps/br208383)
- [Windows.UI.Xaml.Input](https://msdn.microsoft.com/library/windows/apps/br242084)

## <a name="pointers"></a>포인터
대부분의 조작 환경에는 일반적으로 터치, 마우스, 펜/스타일러스, 터치 패드 등의 입력 장치로 가리켜서 조작하려는 개체를 식별하는 사용자를 포함합니다. 이러한 입력 장치에서 제공하는 원시 HID(휴먼 인터페이스 장치) 데이터에는 많은 일반적인 속성이 포함되어 있으므로 데이터가 통합 입력 스택으로 올라가며 장치 독립적인 통합 포인터 데이터로 표시됩니다. 그런 다음 UWP 응용 프로그램은 사용 중인 입력 장치와 상관없이 이 데이터를 사용할 수 있습니다.

> [!NOTE]
> 앱에 장치 관련 정보가 필요한 경우 해당 정보가 원시 HID 데이터에서도 올라갑니다.
 

입력 스택의 각 입력 지점(또는 연락처)은 다양한 포인터 이벤트 처리기에서 제공하는 [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br227968) 매개 변수를 통해 표시되는 [**Pointer**](https://msdn.microsoft.com/library/windows/apps/hh943076) 개체로 표시됩니다. 멀티 펜 또는 멀티 터치 입력의 경우 각 접점이 하나의 고유한 입력 포인터로 간주됩니다.

## <a name="pointer-events"></a>포인터 이벤트


포인터 이벤트는 입력 장치 유형 및 범위 또는 접촉의 감지 상태 등의 기본 정보와 위치, 압력, 접촉 기하 등의 확장 정보를 표시합니다. 또한 사용자가 누른 마우스 단추, 펜 지우개 팁을 사용 중인지 여부 등의 특정 장치 속성도 사용할 수 있습니다. 앱에서 입력 장치와 해당 접근 권한 값을 구분해야 할 경우에는 [입력 장치 식별](identify-input-devices.md)을 참조하세요.

UWP 앱은 다음과 같은 포인터 이벤트를 수신 대기할 수 있습니다.

> [!NOTE]
> 포인터 이벤트 처리기 내의 해당 요소에 대해 [**CapturePointer**](https://msdn.microsoft.com/library/windows/apps/br208918)를 호출하여 특정 UI 요소로 포인터 입력을 제한할 수 있습니다. 포인터가 요소로 캡처될 경우 포인터가 개체의 경계 영역 외부로 이동하더라도 해당 개체만 포인터 입력 이벤트를 받습니다. [**IsInContact**](https://msdn.microsoft.com/library/windows/apps/br227976)(마우스 단추 누름, 터치 또는 스타일러스 접촉 중)이 true여야 **CapturePointer**가 성공적으로 수행됩니다.
 

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">이벤트</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208964"><strong>PointerCanceled</strong></a></p></td>
<td align="left"><p>플랫폼에서 포인터를 취소할 경우 발생합니다. 이 작업은 다음과 같은 환경에서 발생할 수 있습니다.</p>
<ul>
<li>펜이 입력 표면 범위 내에서 감지되면 터치 포인터가 취소됩니다.</li>
<li>100ms가 넘는 활성 접촉은 인식되지 않습니다.</li>
<li>모니터/디스플레이가 변경됩니다(해상도, 설정, 다중 모니터 구성).</li>
<li>데스크톱이 잠기거나 사용자가 로그오프했습니다.</li>
<li>동시 접촉 수가 장치에서 지원하는 수를 초과했습니다.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208965"><strong>PointerCaptureLost</strong></a></p></td>
<td align="left"><p>다른 UI 요소가 포인터를 캡처하거나, 포인터가 해제되거나, 다른 포인터가 프로그래밍 방식으로 캡처될 때 발생합니다.</p>
<div class="alert">
<strong>참고</strong>있으면 다음 해당 포인터 캡처 이벤트가 없습니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208968"><strong>PointerEntered</strong></a></p></td>
<td align="left"><p>포인터가 요소의 경계 영역에 들어갈 때 발생합니다. 터치, 터치 패드, 마우스 및 펜 입력의 경우에는 약간 다른 방식으로 발생할 수 있습니다.</p>
<ul>
<li>터치의 경우 이벤트를 발생시키려면 요소를 직접 누르거나 요소의 경계 영역으로 이동하여 손가락을 접촉해야 합니다.</li>
<li>마우스 및 터치 패드에는 항상 표시되는 화상 커서가 있으며 마우스 또는 터치 패드 버튼을 누르지 않은 경우에도 이 이벤트가 발생합니다.</li>
<li>터치와 마찬가지로 펜에서도 요소를 직접 누르거나 요소의 경계 영역으로 이동하여 이 이벤트를 발생시킵니다. 그러나, 펜에는 가리키기 상태([IsInRange](https://msdn.microsoft.com/library/windows/apps/br227977))도 있으며, true인 경우 이 이벤트가 발생합니다.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208969"><strong>PointerExited</strong></a></p></td>
<td align="left"><p>포인터가 요소 경계 영역을 벗어날 때 발생합니다. 터치, 터치 패드, 마우스 및 펜 입력의 경우에는 약간 다른 방식으로 발생할 수 있습니다.</p>
<ul>
<li>터치의 경우 손가락 접촉이 필요하며 포인터가 요소의 경계 영역을 벗어날 때 이 이벤트가 발생합니다.</li>
<li>마우스 및 터치 패드에는 항상 표시되는 화상 커서가 있으며 마우스 또는 터치 패드 버튼을 누르지 않은 경우에도 이 이벤트가 발생합니다.</li>
<li>터치와 마찬가지로 펜도 요소의 경계 영역을 벗어날 때 이 이벤트가 발생합니다. 그러나, 펜에는 가리키기 상태([IsInRange](https://msdn.microsoft.com/library/windows/apps/br227977))도 있으며, 상태가 True에서 False로 변경되면 이 이벤트가 발생합니다.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208970"><strong>PointerMoved</strong></a></p></td>
<td align="left"><p>요소의 경계 영역 내에서 포인터가 좌표, 버튼 상태, 압력, 기울기 또는 접촉 기하(예: 너비 및 높이)를 변경할 때 발생합니다. 터치, 터치 패드, 마우스 및 펜 입력의 경우에는 약간 다른 방식으로 발생할 수 있습니다.</p>
<ul>
<li>터치의 경우 손가락 접촉이 필요하며 요소의 경계 영역 내에서 접촉할 경우에만 이 이벤트가 발생합니다.</li>
<li>마우스 및 터치 패드에는 항상 표시되는 화상 커서가 있으며 마우스 또는 터치 패드 버튼을 누르지 않은 경우에도 이 이벤트가 발생합니다.</li>
<li>터치와 마찬가지로 펜이 요소의 경계 영역 내에서 접촉할 경우 이 이벤트가 발생합니다. 그러나 펜에는 가리키기 상태([[IsInRange](https://msdn.microsoft.com/library/windows/apps/br227977)](https://msdn.microsoft.com/library/windows/apps/br227977))도 있으며, true이고 요소의 경계 영역 내에 있는 경우 이 이벤트가 발생합니다.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208971"><strong>PointerPressed</strong></a></p></td>
<td align="left"><p>요소의 경계 영역 내에서 포인터가 누르기 동작(예: 터치 다운, 마우스 단추 누름, 펜 누름 또는 터치 패드 단추 누름)을 나타냅니다.</p>
<p>이 이벤트가 수행되려면 [CapturePointer](https://msdn.microsoft.com/library/windows/apps/br208918)가 처리기에서 호출되어야 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208972"><strong>PointerReleased</strong></a></p></td>
<td align="left"><p>포인터가 요소 경계 영역 내에서 해제 동작(예: 터치 업, 마우스 단추에서 손 떼기, 펜 업 또는 터치 패드 단추에서 손 떼기)을 나타내거나 포인터가 경계 영역 외부에서 캡처되는 경우 발생합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208973"><strong>PointerWheelChanged</strong></a></p></td>
<td align="left"><p>마우스 휠이 회전할 때 발생합니다.</p>
<p>마우스 입력이 먼저 감지되면 마우스 입력이 할당된 단일 포인터와 연결됩니다. 마우스 단추(왼쪽, 휠 또는 오른쪽)를 클릭하면 [PointerMoved](https://msdn.microsoft.com/library/windows/apps/br208970) 이벤트를 통해 포인터와 해당 단추 간의 보조 연결이 만들어집니다.</p></td>
</tr>
</tbody>
</table> 

## <a name="pointer-event-example"></a>포인터 이벤트 예제

다음은 기본 포인터 추적 앱의 코드 조각으로, 여러 포인터를 위해 이벤트를 수신 대기하고 처리하며 연결된 포인터의 다양한 속성을 가져오는 방법을 보여 줍니다.

![포인터 응용 프로그램 UI](images/pointers/pointers1.gif)

**[포인터 입력 샘플(기본)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers.zip)에서 이 샘플 다운로드**

### <a name="create-the-ui"></a>UI 만들기

다음 예에서는 [사각형](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.shapes.rectangle)(`Target`)을 포인터 입력을 사용하는 개체로 사용합니다. 대상의 색상은 포인터 상태가 변경될 때 변경됩니다.

각 포인터에 대한 세부 정보는 포인터가 이동할 때 포인터를 따라오는 부동 [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)에 표시됩니다. 포인터 이벤트 자체는 사각형의 오른쪽에 있는 [RichTextBlock](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock)에 보고됩니다.

다음은 XAML(Extensible Application Markup Language) UI 예문입니다. 

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

### <a name="listen-for-pointer-events"></a>포인터 이벤트 수신

대부분의 경우에는 이벤트 처리기의 [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076)를 통해 포인터 정보를 가져오는 것이 좋습니다.

이벤트 인수가 필요한 포인터 정보를 표시하지 않으면 [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br242038)의 [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/hh943077) 및 [**GetIntermediatePoints**](https://msdn.microsoft.com/library/windows/apps/hh943078) 메서드를 통해 표시되는 확장된 [**PointerPoint**](https://msdn.microsoft.com/library/windows/apps/hh943076) 정보에 액세스할 수 있습니다.

다음은 각 활성 포인터 추적용으로 전역 사전 개체를 설정하고 대상 개체용으로 다양한 포인터 이벤트 수신기를 식별하는 코드입니다.

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

다음으로 UI 피드백을 사용하여 기본 포인터 이벤트 처리기에 대해 살펴봅니다.

-   이 처리기는 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) 이벤트를 관리합니다. 이벤트 로그에 이벤트를 추가하고, 활성 포인터 사전에 포인터를 추가하고, 포인터 상세 정보를 표시합니다.

    > [!NOTE]
    > [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) 및 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) 이벤트는 항상 쌍으로 발생하지는 않습니다. 앱은 포인터 다운(예: [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969), [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964) 및 [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965))을 완료할 수도 있는 이벤트를 수신하고 처리해야 합니다.      

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

-   이 처리기는 [**PointerEntered**](https://msdn.microsoft.com/library/windows/apps/br208968) 이벤트를 관리합니다. 이벤트 로그에 이벤트를 추가하고, 포인터 모음에 포인터를 추가하고, 포인터 상세 정보를 표시합니다.

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

-   이 처리기는 [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970) 이벤트를 관리합니다. 이벤트 로그에 이벤트를 추가하고 포인터 상세 정보를 업데이트합니다.

    > [!Important]
    > 마우스 입력이 먼저 감지되면 마우스 입력이 할당된 단일 포인터와 연결됩니다. 마우스 단추(왼쪽, 휠 또는 오른쪽)를 클릭하면 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) 이벤트를 통해 포인터와 해당 단추 간의 보조 연결이 만들어집니다. [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) 이벤트는 해당하는 동일한 마우스 단추를 해제한 경우에만 발생합니다(이 이벤트가 완료될 때까지는 다른 단추를 이 포인터와 연결할 수 없음). 이 독점적인 연결 때문에 다른 마우스 단추 클릭은 [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970) 이벤트를 통해 라우트됩니다.     

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

-   이 처리기는 [**PointerWheelChanged**](https://msdn.microsoft.com/library/windows/apps/br208973) 이벤트를 관리합니다. 이벤트 로그에 이벤트를 추가하고, 포인터 배열에 포인터를 추가(필요한 경우)하고, 포인터 상세 정보를 표시합니다.

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

-   이 처리기는 디지타이저와의 접촉이 종료되는 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) 이벤트를 관리합니다. 이벤트 로그에 이벤트를 추가하고, 포인터 모음에서 포인터를 제거하고, 포인터 상세 정보를 업데이트합니다.

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

-   이 처리기는 디지타이저와의 접촉이 유지 관리되는 [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969) 이벤트를 관리합니다. 이벤트 로그에 이벤트를 추가하고, 포인터 배열에서 포인터를 제거하고, 포인터 상세 정보를 업데이트합니다.

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

-   이 처리기는 [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964) 이벤트를 관리합니다. 이벤트 로그에 이벤트를 추가하고, 포인터 배열에서 포인터를 제거하고, 포인터 상세 정보를 업데이트합니다.

```csharp
/// <summary>
/// The pointer canceled event handler.
/// Fires for for various reasons, including: 
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

-   이 처리기는 [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965) 이벤트를 관리합니다. 이벤트 로그에 이벤트를 추가하고, 포인터 배열에서 포인터를 제거하고, 포인터 상세 정보를 업데이트합니다.

    > [!NOTE]
    > [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965)는 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) 대신 발생할 수 있습니다. 포인터 캡처는 사용자 조작, 프로그래밍 방식의 다른 포인터 캡처, [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) 호출 등 다양한 이유로 잃게 될 수 있습니다.     

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

앞에서 설명한 대로 [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br242038)의 [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/hh943077) 및 [**GetIntermediatePoints**](https://msdn.microsoft.com/library/windows/apps/hh943078)의 메서드를 통해 [**Windows.UI.Input.PointerPoint**](https://msdn.microsoft.com/library/windows/apps/hh943076) 개체에서 가장 확장된 포인터를 가져와야 합니다. 다음은 작업 방법을 보여주는 코드 조각입니다.

-   먼저 포인터별로 새로운 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)을 만듭니다.

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

-   그런 다음 해당 포인터와 연결된 기존의 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)에서 포인터 정보를 업데이트하는 방법을 제공합니다.

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

-   마지막으로 다양한 포인터 속성을 쿼리합니다.

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
터치 디지타이저나 터치 패드와 같은 일부 입력 장치는 기존에 사용하던 하나의 마우스나 펜 포인터가 아닌 여러 개의 포인터를 지원합니다(대부분의 경우 Surface Hub는 두 개의 펜 입력 지원). 

**[PointerPointerProperties](https://docs.microsoft.com/uwp/api/windows.ui.input.pointerpointproperties)** 클래스의 읽기 전용 **[IsPrimary](https://docs.microsoft.com/uwp/api/windows.ui.input.pointerpointproperties.IsPrimary)** 속성을 사용하면 단일 기본 포인터를 식별하고 차별화할 수 있습니다(기본 포인터는 항상 입력 시퀀스 중에 탐지되는 첫 번째 포인터임). 

기본 포인터를 식별함으로써 이를 사용하여 마우스나 펜 입력을 에뮬레이트하거나, 조작을 사용자 지정하거나 일부 다른 특정 기능 또는 UI를 제공할 수 있습니다.

> [!NOTE]
> 입력 시퀀스 중에 기본 포인터가 해제되거나, 취소되거나, 손실되면, 새로운 입력 순서가 시작될 때까지 기본 입력 포인터가 생성되지 않습니다(모든 포인터가 해제, 취소 또는 손실되면 입력 시퀀스가 종료됨).

## <a name="primary-pointer-animation-example"></a>기본 포인터 애니메이션 예제

다음은 특별한 가시적 피드백을 제공하여 사용자가 응용 프로그램에 있는 포인터 입력 간을 차별화하도록 돕는 방법을 보여주는 코드 조각입니다.

이 특정 앱에서는 색상 및 애니메이션을 모두 사용하여 기본 포인터를 강조 표시합니다.

![애니메이션 시각적 피드백을 사용하는 포인터 응용 프로그램](images/pointers/pointers-usercontrol-animation.gif)

**[포인터 입력 샘플(애니메이션이 있는 UserControl)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip)에서 이 샘플 다운로드**

### <a name="visual-feedback"></a>시각적 피드백

XAML **[Ellipse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.ellipse)** 개체를 기반으로 **[UserControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol)** 을 정의합니다. 이 개체는 캔버스에서 각 개체의 위치를 강조 표시하며 **[Storyboard](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.animation.storyboard)** 를 사용하여 기본 포인터에 해당하는 타원에 애니메이션 효과를 줍니다.

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

관련 코드는 다음과 같습니다.
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
이 예제의 UI는 입력 **[캔버스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas)** 로 제한됩니다. 여기에서 포인터를 추적하고 포인터 카운터 및 기본 포인터 식별자가 들어 있는 헤더 막대를 따라 포인터 표시기 및 기본 포인터 애니메이션을 렌더링합니다.

MainPage.xaml은 다음과 같습니다.

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

마지막으로, MainPage.xaml.cs 관련 코드에 기본 포인터 이벤트 처리기를 정의합니다. 이전 예제에서 기본 사항을 설명했으므로 여기에서 코드를 재현하지는 않겠지만, 원하는 경우 [포인터 입력 샘플(애니메이션이 있는 UserControl)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip)에서 작업 샘플을 다운로드할 수 있습니다.

## <a name="related-articles"></a>관련 문서

**항목 샘플**
* [포인터 입력 샘플(기본)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers.zip)
* [포인터 입력 샘플(애니메이션이 있는 UserControl)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip)

**기타 샘플**
* [기본 입력 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [짧은 대기 시간 입력 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [사용자 조작 모드 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [포커스 화면 효과 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**보관 샘플**
* [입력: XAML 사용자 입력 이벤트 샘플](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [입력: 장치 기능 샘플](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [입력: 조작 및 제스처(C++) 샘플](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [입력: 터치 적중 횟수 테스트 샘플](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML 스크롤, 이동 및 확대/축소 샘플](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [입력: 간단한 잉크 샘플](http://go.microsoft.com/fwlink/p/?linkid=246570)