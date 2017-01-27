---
author: Karl-Bridge-Microsoft
Description: "UWP(유니버설 Windows 플랫폼) 앱에서 터치, 마우스, 펜/스타일러스, 터치 패드 등의 포인팅 장치에서 입력 데이터를 수신하고, 처리하고, 관리합니다."
title: "포인터 입력 처리"
ms.assetid: BDBC9E33-4037-4671-9596-471DCF855C82
label: Handle pointer input
template: detail.hbs
keywords: "펜, 마우스, 터치 패드, 터치, 포인터, 입력, 사용자 조작"
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: 482530931fe5764f65d2564107318c272c5c7b7f
ms.openlocfilehash: ba4288d93924d3a32a0a659dea67af28fb607987

---

# <a name="handle-pointer-input"></a>포인터 입력 처리
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

UWP(유니버설 Windows 플랫폼) 앱에서 터치, 마우스, 펜/스타일러스, 터치 패드 등의 포인팅 장치에서 입력 데이터를 수신하고, 처리하고, 관리합니다.

<div class="important-apis" >
<b>중요 API</b><br/>
<ul>
<li>[**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)</li>
<li>[**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br208383)</li>
<li>[**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)</li>
</ul>
</div>

**중요**  
고유한 조작 지원을 구현하는 경우 사용자들은 앱의 UI 요소를 직접 조작하는 직관적인 환경을 기대한다는 것에 유의하세요. [컨트롤 목록](https://msdn.microsoft.com/library/windows/apps/mt185406)에서 항목들을 일관되고 검색 가능하게 유지할 수 있도록 사용자 지정 조작을 모델링할 것을 권장합니다. 플랫폼 컨트롤은 표준 조작, 애니메이션 효과를 준 물리적 효과, 시각적 피드백 및 접근성을 비롯하여 UWP(유니버설 Windows 플랫폼) 사용자 조작 환경 전체를 제공합니다. 요구 사항이 명확하게 잘 정의되어 있으며 기본 제스처가 시나리오를 지원하지 않는 경우에만 사용자 지정 조작을 만드세요.


## <a name="pointers"></a>포인터
많은 조작 환경에는 터치, 마우스, 펜/스타일러스, 터치 패드 등의 입력 장치로 가리켜서 조작하려는 개체를 식별하는 사용자를 포함합니다. 이러한 입력 장치에서 제공하는 원시 HID(휴먼 인터페이스 장치) 데이터에는 많은 일반적인 속성이 포함되어 있으므로 정보가 통합 입력 스택으로 올라가며 장치 독립적인 통합 포인터 데이터로 표시됩니다. 그런 다음 UWP 앱은 사용 중인 입력 장치와 상관없이 이 데이터를 사용할 수 있습니다.

**참고**  앱에 장치 관련 정보가 필요한 경우 해당 정보가 원시 HID 데이터에서도 올라갑니다.

 

입력 스택의 각 입력 지점(또는 연락처)은 다양한 포인터 이벤트에서 제공하는 [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br227968) 매개 변수를 통해 표시되는 [**Pointer**](https://msdn.microsoft.com/library/windows/apps/hh943076) 개체로 표시됩니다. 멀티 펜 또는 멀티 터치 입력의 경우 각 접점이 하나의 고유한 포인터로 간주됩니다.

## <a name="pointer-events"></a>포인터 이벤트


포인터 이벤트는 범위 또는 접촉의 감지 상태, 장치 유형 등의 기본 정보와 위치, 압력, 접촉 기하 등의 확장 정보를 표시합니다. 또한 사용자가 누른 마우스 단추, 펜 지우개 팁을 사용 중인지 여부 등의 특정 장치 속성도 사용할 수 있습니다. 앱에서 입력 장치와 해당 접근 권한 값을 구분해야 할 경우에는 [입력 장치 식별](identify-input-devices.md)을 참조하세요.

UWP 앱은 다음과 같은 포인터 이벤트를 수신 대기할 수 있습니다.

**참고**  [**CapturePointer**](https://msdn.microsoft.com/library/windows/apps/br208918)를 호출하여 포인터 입력을 특정 UI 요소로 제한하세요. 포인터가 요소로 캡처될 경우 포인터가 개체의 경계 영역 외부로 이동하더라도 해당 개체만 포인터 입력 이벤트를 받습니다. **CapturePointer**가 성공하려면 [**IsInContact**](https://msdn.microsoft.com/library/windows/apps/br208971)(마우스 단추 누름, 터치 또는 스타일러스 접촉)가 true여야 하므로 일반적으로 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br227976) 이벤트 처리기 내에서 포인터를 캡처합니다.

 

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
<td align="left"><p>[<strong>PointerCanceled</strong>](https://msdn.microsoft.com/library/windows/apps/br208964)</p></td>
<td align="left"><p>플랫폼에서 포인터를 취소할 경우 발생합니다.</p>
<ul>
<li>펜이 입력 표면 범위 내에서 감지되면 터치 포인터가 취소됩니다.</li>
<li>100ms가 넘는 활성 접촉은 인식되지 않습니다.</li>
<li>모니터/디스플레이가 변경됩니다(해상도, 설정, 다중 모니터 구성).</li>
<li>데스크톱이 잠기거나 사용자가 로그오프했습니다.</li>
<li>동시 접촉 수가 장치에서 지원하는 수를 초과했습니다.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>PointerCaptureLost</strong>](https://msdn.microsoft.com/library/windows/apps/br208965)</p></td>
<td align="left"><p>다른 UI 요소가 포인터를 캡처하거나, 포인터가 해제되거나, 다른 포인터가 프로그래밍 방식으로 캡처될 때 발생합니다.</p>
<div class="alert">
<strong>참고</strong>  해당하는 포인터 캡처 이벤트가 없습니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>PointerEntered</strong>](https://msdn.microsoft.com/library/windows/apps/br208968)</p></td>
<td align="left"><p>포인터가 요소의 경계 영역에 들어갈 때 발생합니다. 터치, 터치 패드, 마우스 및 펜 입력의 경우에는 약간 다른 방식으로 발생할 수 있습니다.</p>
<ul>
<li>터치의 경우 이벤트를 발생시키려면 요소를 직접 누르거나 요소의 경계 영역으로 이동하여 손가락을 접촉해야 합니다.</li>
<li>마우스 및 터치 패드에는 항상 표시되는 화상 커서가 있으며 마우스 또는 터치 패드 버튼을 누르지 않은 경우에도 이 이벤트가 발생합니다.</li>
<li>터치와 마찬가지로 펜에서도 요소를 직접 누르거나 요소의 경계 영역으로 이동하여 이 이벤트를 발생시킵니다. 그러나 펜에는 가리키기 상태([<strong>IsInRange</strong>](https://msdn.microsoft.com/library/windows/apps/br227977))도 있으며, true인 경우 이 이벤트가 발생합니다.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>PointerExited</strong>](https://msdn.microsoft.com/library/windows/apps/br208969)</p></td>
<td align="left"><p>포인터가 요소 경계 영역을 벗어날 때 발생합니다. 터치, 터치 패드, 마우스 및 펜 입력의 경우에는 약간 다른 방식으로 발생할 수 있습니다.</p>
<ul>
<li>터치의 경우 손가락 접촉이 필요하며 포인터가 요소의 경계 영역을 벗어날 때 이 이벤트가 발생합니다.</li>
<li>마우스 및 터치 패드에는 항상 표시되는 화상 커서가 있으며 마우스 또는 터치 패드 버튼을 누르지 않은 경우에도 이 이벤트가 발생합니다.</li>
<li>터치와 마찬가지로 펜도 요소의 경계 영역을 벗어날 때 이 이벤트가 발생합니다. 그러나 펜에는 가리키기 상태([<strong>IsInRange</strong>](https://msdn.microsoft.com/library/windows/apps/br227977))도 있으며, 상태가 true에서 false로 변경되는 경우 이 이벤트가 발생합니다.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>PointerMoved</strong>](https://msdn.microsoft.com/library/windows/apps/br208970)</p></td>
<td align="left"><p>요소의 경계 영역 내에서 포인터가 좌표, 버튼 상태, 압력, 기울기 또는 접촉 기하(예: 너비 및 높이)를 변경할 때 발생합니다. 터치, 터치 패드, 마우스 및 펜 입력의 경우에는 약간 다른 방식으로 발생할 수 있습니다.</p>
<ul>
<li>터치의 경우 손가락 접촉이 필요하며 요소의 경계 영역 내에서 접촉할 경우에만 이 이벤트가 발생합니다.</li>
<li>마우스 및 터치 패드에는 항상 표시되는 화상 커서가 있으며 마우스 또는 터치 패드 버튼을 누르지 않은 경우에도 이 이벤트가 발생합니다.</li>
<li>터치와 마찬가지로 펜이 요소의 경계 영역 내에서 접촉할 경우 이 이벤트가 발생합니다. 그러나 펜에는 가리키기 상태([<strong>IsInRange</strong>](https://msdn.microsoft.com/library/windows/apps/br227977))도 있으며, true이고 요소의 경계 영역 내에 있는 경우 이 이벤트가 발생합니다.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>PointerPressed</strong>](https://msdn.microsoft.com/library/windows/apps/br208971)</p></td>
<td align="left"><p>요소의 경계 영역 내에서 포인터가 누르기 동작(예: 터치 다운, 마우스 단추 누름, 펜 누름 또는 터치 패드 단추 누름)을 나타냅니다.</p>
<p>[<strong>CapturePointer</strong>](https://msdn.microsoft.com/library/windows/apps/br208918)는 이 이벤트 처리기에서 호출되어야 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>PointerReleased</strong>] (https://msdn.microsoft.com/library/windows/apps/br208972)</p></td>
<td align="left"><p>포인터가 요소 경계 영역 내에서 해제 동작(예: 터치 업, 마우스 단추에서 손 떼기, 펜 업 또는 터치 패드 단추에서 손 떼기)을 나타내거나 포인터가 경계 영역 외부에서 캡처되는 경우 발생합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>PointerWheelChanged</strong>](https://msdn.microsoft.com/library/windows/apps/br208973)</p></td>
<td align="left"><p>마우스 휠이 회전할 때 발생합니다.</p>
<p>마우스 입력이 먼저 감지되면 마우스 입력이 할당된 단일 포인터와 연결됩니다. 마우스 단추(왼쪽, 휠 또는 오른쪽)를 클릭하면 [<strong>PointerMoved</strong>](https://msdn.microsoft.com/library/windows/apps/br208970) 이벤트를 통해 포인터와 해당 단추 간의 보조 연결이 만들어집니다.</p></td>
</tr>
</tbody>
</table>

 

## <a name="example"></a>예제


다음은 기본 포인터 추적 앱의 코드 예제로, 포인터 이벤트를 수신 대기하고 처리하며 활성 포인터의 다양한 속성을 가져오는 방법을 보여 줍니다.

### <a name="create-the-ui"></a>UI 만들기

다음 예에서는 직사각형(`targetContainer`)을 포인터 입력을 위한 대상 개체로 사용합니다. 대상의 색상은 포인터 상태가 변경될 때 변경됩니다.

각 포인터에 대한 세부 정보는 포인터와 함께 이동하는 부동 텍스트 블록에 표시됩니다. 포인터 이벤트 자체는 직사각형(이벤트 순서 보고용)의 왼쪽에 표시됩니다.

다음은 XAML(Extensible Application Markup Language) 예문입니다.

```XAML
<Page
    x:Class="PointerInput.MainPage"
    IsTabStop="false"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:PointerInput"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Name="page">

    <Grid Background="{StaticResource ApplicationForegroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="69.458" />
            <ColumnDefinition Width="80.542"/>
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
        <Button Name="buttonClear"
                Foreground="White"
                Width="100"
                Height="100">
            clear
        </Button>
        <TextBox Name="eventLog" 
                 Grid.Column="1"
                 Grid.Row="0"
                 Grid.RowSpan="3" 
                 Background="#000000" 
                 TextWrapping="Wrap" 
                 Foreground="#FFFFFF" 
                 ScrollViewer.VerticalScrollBarVisibility="Visible" 
                 BorderThickness="0" Grid.ColumnSpan="2"/>
    </Grid>
</Page>
```

### <a name="listen-for-pointer-events"></a>포인터 이벤트 수신

대부분의 경우에는 이벤트 처리기의 [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076)를 통해 포인터 정보를 가져오는 것이 좋습니다.

이벤트 인수가 필요한 포인터 정보를 표시하지 않으면 [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br242038)의 [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/hh943077) 및 [**GetIntermediatePoints**](https://msdn.microsoft.com/library/windows/apps/hh943078) 메서드를 통해 표시되는 확장된 [**PointerPoint**](https://msdn.microsoft.com/library/windows/apps/hh943076) 정보에 액세스할 수 있습니다.

다음 예에서는 직사각형(`targetContainer`)을 포인터 입력을 위한 대상 개체로 사용합니다. 대상의 색상은 포인터 상태가 변경될 때 변경됩니다.

다음 코드는 대상 개체를 설정하고, 전역 변수를 선언하고, 대상에 대해 다양한 포인터 이벤트 수신기를 식별합니다.

```CSharp
        // For this example, we track simultaneous contacts in case the 
        // number of contacts has reached the maximum supported by the device.
        // Depending on the device, additional contacts might be ignored 
        // (PointerPressed not fired). 
        uint numActiveContacts;
        Windows.Devices.Input.TouchCapabilities touchCapabilities = new Windows.Devices.Input.TouchCapabilities();

        // Dictionary to maintain information about each active contact. 
        // An entry is added during PointerPressed/PointerEntered events and removed 
        // during PointerReleased/PointerCaptureLost/PointerCanceled/PointerExited events.
        Dictionary<uint, Windows.UI.Xaml.Input.Pointer> contacts;

        public MainPage()
        {
            this.InitializeComponent();
            numActiveContacts = 0;
            // Initialize the dictionary.
            contacts = new Dictionary<uint, Windows.UI.Xaml.Input.Pointer>((int)touchCapabilities.Contacts);
            // Declare the pointer event handlers.
            Target.PointerPressed += new PointerEventHandler(Target_PointerPressed);
            Target.PointerEntered += new PointerEventHandler(Target_PointerEntered);
            Target.PointerReleased += new PointerEventHandler(Target_PointerReleased);
            Target.PointerExited += new PointerEventHandler(Target_PointerExited);
            Target.PointerCanceled += new PointerEventHandler(Target_PointerCanceled);
            Target.PointerCaptureLost += new PointerEventHandler(Target_PointerCaptureLost);
            Target.PointerMoved += new PointerEventHandler(Target_PointerMoved);
            Target.PointerWheelChanged += new PointerEventHandler(Target_PointerWheelChanged);

            buttonClear.Click += new RoutedEventHandler(ButtonClear_Click);
        }

        private void ButtonClear_Click(object sender, RoutedEventArgs e)
        {
            eventLog.Text = "";
        }

```

### <a name="handle-pointer-events"></a>포인터 이벤트 처리

다음으로 UI 피드백을 사용하여 기본 포인터 이벤트 처리기에 대해 살펴봅니다.

-   이 처리기는 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) 이벤트를 관리합니다. 이벤트 로그에 이벤트를 추가하고, 관심있는 포인터를 추적하는 데 사용하는 포인터 배열에 포인터를 추가하고, 포인터 상세 정보를 표시합니다.

    **참고**  [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) 및 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) 이벤트는 항상 쌍으로 발생하지는 않습니다. 앱은 포인터 다운 작업(예: [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969), [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964) 및 [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965))을 완료할 수도 있는 이벤트를 수신하고 처리해야 합니다.

     

```    CSharp
        // PointerPressed and PointerReleased events do not always occur in pairs. 
            // Your app should listen for and handle any event that might conclude a pointer down action 
            // (such as PointerExited, PointerCanceled, and PointerCaptureLost).
            // For this example, we track the number of contacts in case the 
            // number of contacts has reached the maximum supported by the device.
            // Depending on the device, additional contacts might be ignored 
            // (PointerPressed not fired). 
            void Target_PointerPressed(object sender, PointerRoutedEventArgs e)
            {
                Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

                // Update event sequence.
                eventLog.Text += "\nDown: " + ptr.PointerId;

                // Change background color of target when pointer contact detected.
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Green);

                // Prevent most handlers along the event route from handling the same event again.
                e.Handled = true;

                // Lock the pointer to the target.
                Target.CapturePointer(e.Pointer);

                // Update event sequence.
                eventLog.Text += "\nPointer captured: " + ptr.PointerId;

                // Check if pointer already exists (for example, enter occurred prior to press).
                if (contacts.ContainsKey(ptr.PointerId))
                {
                    return;
                }
                // Add contact to dictionary.
                contacts[ptr.PointerId] = ptr;
                ++numActiveContacts;

                // Display pointer details.
                createInfoPop(e);
            }
```

-   이 처리기는 [**PointerEntered**](https://msdn.microsoft.com/library/windows/apps/br208968) 이벤트를 관리합니다. 이벤트 로그에 이벤트를 추가하고, 포인터 모음에 포인터를 추가하고, 포인터 상세 정보를 표시합니다.

```    CSharp
        private void Target_PointerEntered(object sender, PointerRoutedEventArgs e)
            {
                Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

                // Update event sequence.
                eventLog.Text += "\nEntered: " + ptr.PointerId;

                if (contacts.Count == 0)
                {
                    // Change background color of target when pointer contact detected.
                    Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
                }

                // Check if pointer already exists (if enter occurred prior to down).
                if (contacts.ContainsKey(ptr.PointerId))
                {
                    return;
                }

                // Add contact to dictionary.
                contacts[ptr.PointerId] = ptr;
                ++numActiveContacts;

                // Prevent most handlers along the event route from handling the same event again.
                e.Handled = true;

                // Display pointer details.
                createInfoPop(e);
            }
```

-   이 처리기는 [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970) 이벤트를 관리합니다. 이벤트 로그에 이벤트를 추가하고 포인터 상세 정보를 업데이트합니다.

    **중요**  마우스 입력이 먼저 감지되면 마우스 입력이 할당된 단일 포인터와 연결됩니다. 마우스 단추(왼쪽, 휠 또는 오른쪽)를 클릭하면 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) 이벤트를 통해 포인터와 해당 단추 간의 보조 연결이 만들어집니다. [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) 이벤트는 해당하는 동일한 마우스 단추를 해제한 경우에만 발생합니다(이 이벤트가 완료될 때까지는 다른 단추를 이 포인터와 연결할 수 없음). 이 독점적인 연결 때문에 다른 마우스 단추 클릭은 [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970) 이벤트를 통해 라우트됩니다.

     

```    CSharp
private void Target_PointerMoved(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

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
        if (ptr.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse)
        {
            // To get mouse state, we need extended pointer details.
            // We get the pointer info through the getCurrentPoint method
            // of the event argument. 
            Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
            if (ptrPt.Properties.IsLeftButtonPressed)
            {
                eventLog.Text += "\nLeft button: " + ptrPt.PointerId;
            }
            if (ptrPt.Properties.IsMiddleButtonPressed)
            {
                eventLog.Text += "\nWheel button: " + ptrPt.PointerId;
            }
            if (ptrPt.Properties.IsRightButtonPressed)
            {
                eventLog.Text += "\nRight button: " + ptrPt.PointerId;
            }
        }

        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;

        // Display pointer details.
        updateInfoPop(e);
    }
```

-   이 처리기는 [**PointerWheelChanged**](https://msdn.microsoft.com/library/windows/apps/br208973) 이벤트를 관리합니다. 이벤트 로그에 이벤트를 추가하고, 포인터 배열에 포인터를 추가(필요한 경우)하고, 포인터 상세 정보를 표시합니다.

```    CSharp
private void Target_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nMouse wheel: " + ptr.PointerId;

        // Check if pointer already exists (for example, enter occurred prior to wheel).
        if (contacts.ContainsKey(ptr.PointerId))
        {
            return;
        }

        // Add contact to dictionary.
        contacts[ptr.PointerId] = ptr;
        ++numActiveContacts;

        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;

        // Display pointer details.
        createInfoPop(e);
    }
```

-   이 처리기는 디지타이저와의 접촉이 종료되는 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) 이벤트를 관리합니다. 이벤트 로그에 이벤트를 추가하고, 포인터 모음에서 포인터를 제거하고, 포인터 상세 정보를 업데이트합니다.

```    CSharp
void Target_PointerReleased(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nUp: " + ptr.PointerId;

        // If event source is mouse or touchpad and the pointer is still 
        // over the target, retain pointer and pointer details.
        // Return without removing pointer from pointers dictionary.
        // For this example, we assume a maximum of one mouse pointer.
        if (ptr.PointerDeviceType != Windows.Devices.Input.PointerDeviceType.Mouse)
        {
            // Update target UI.
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);

            destroyInfoPop(ptr);

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            // Release the pointer from the target.
            Target.ReleasePointerCapture(e.Pointer);

            // Update event sequence.
            eventLog.Text += "\nPointer released: " + ptr.PointerId;

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }
        else
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
        }

    }
```

-   이 처리기는 디지타이저와의 접촉이 유지 관리되는 [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969) 이벤트를 관리합니다. 이벤트 로그에 이벤트를 추가하고, 포인터 배열에서 포인터를 제거하고, 포인터 상세 정보를 업데이트합니다.

```    CSharp
private void Target_PointerExited(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nPointer exited: " + ptr.PointerId;

        // Remove contact from dictionary.
        if (contacts.ContainsKey(ptr.PointerId))
        {
            contacts[ptr.PointerId] = null;
            contacts.Remove(ptr.PointerId);
            --numActiveContacts;
        }

        // Update the UI and pointer details.
        destroyInfoPop(ptr);

        if (contacts.Count == 0)
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);
        }
        
        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;
    }
```

-   이 처리기는 [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964) 이벤트를 관리합니다. 이벤트 로그에 이벤트를 추가하고, 포인터 배열에서 포인터를 제거하고, 포인터 상세 정보를 업데이트합니다.

```    CSharp
// Fires for for various reasons, including: 
    //    - Touch contact canceled by pen coming into range of the surface.
    //    - The device doesn&#39;t report an active contact for more than 100ms.
    //    - The desktop is locked or the user logged off. 
    //    - The number of simultaneous contacts exceeded the number supported by the device.
    private void Target_PointerCanceled(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nPointer canceled: " + ptr.PointerId;

        // Remove contact from dictionary.
        if (contacts.ContainsKey(ptr.PointerId))
        {
            contacts[ptr.PointerId] = null;
            contacts.Remove(ptr.PointerId);
            --numActiveContacts;
        }

        destroyInfoPop(ptr);

        if (contacts.Count == 0)
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
        }
        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;
    }
```

-   이 처리기는 [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965) 이벤트를 관리합니다. 이벤트 로그에 이벤트를 추가하고, 포인터 배열에서 포인터를 제거하고, 포인터 상세 정보를 업데이트합니다.

    **참고**  [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965)는 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) 대신 발생할 수 있습니다. 다양한 이유로 포인터 캡처가 손실될 수 있습니다.

     

```    CSharp
// Fires for for various reasons, including: 
    //    - User interactions
    //    - Programmatic capture of another pointer
    //    - Captured pointer was deliberately released
    // PointerCaptureLost can fire instead of PointerReleased. 
    private void Target_PointerCaptureLost(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nPointer capture lost: " + ptr.PointerId;

        // Remove contact from dictionary.
        if (contacts.ContainsKey(ptr.PointerId))
        {
            contacts[ptr.PointerId] = null;
            contacts.Remove(ptr.PointerId);
            --numActiveContacts;
        }

        destroyInfoPop(ptr);

        if (contacts.Count == 0)
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
        }
        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;
    }
```

### <a name="get-pointer-properties"></a>포인터 속성 가져오기

앞에서 설명한 대로 [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br242038)의 [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/hh943077) 및 [**GetIntermediatePoints**](https://msdn.microsoft.com/library/windows/apps/hh943078)의 메서드를 통해 [**Windows.UI.Input.PointerPoint**](https://msdn.microsoft.com/library/windows/apps/hh943076) 개체에서 가장 확장된 포인터를 가져와야 합니다.

-   먼저 포인터별로 새로운 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)을 만듭니다.

```    CSharp
        void createInfoPop(PointerRoutedEventArgs e)
            {
                TextBlock pointerDetails = new TextBlock();
                Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                pointerDetails.Name = ptrPt.PointerId.ToString();
                pointerDetails.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
                pointerDetails.Text = queryPointer(ptrPt);

                TranslateTransform x = new TranslateTransform();
                x.X = ptrPt.Position.X + 20;
                x.Y = ptrPt.Position.Y + 20;
                pointerDetails.RenderTransform = x;

                Container.Children.Add(pointerDetails);
            }
```

-   그런 다음 해당 포인터와 연결된 기존의 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)에서 포인터 정보를 업데이트하는 방법을 제공합니다.

```    CSharp
        void updateInfoPop(PointerRoutedEventArgs e)
            {
                foreach (var pointerDetails in Container.Children)
                {
                    if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
                    {
                        TextBlock _TextBlock = (TextBlock)pointerDetails;
                        if (_TextBlock.Name == e.Pointer.PointerId.ToString())
                        {
                            // To get pointer location details, we need extended pointer info.
                            // We get the pointer info through the getCurrentPoint method
                            // of the event argument. 
                            Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                            TranslateTransform x = new TranslateTransform();
                            x.X = ptrPt.Position.X + 20;
                            x.Y = ptrPt.Position.Y + 20;
                            pointerDetails.RenderTransform = x;
                            _TextBlock.Text = queryPointer(ptrPt);
                        }
                    }
                }
            }
```

-   마지막으로 다양한 포인터 속성을 쿼리합니다.

```    CSharp
         String queryPointer(PointerPoint ptrPt)
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

                 GeneralTransform gt = Target.TransformToVisual(page);
                 Point screenPoint;

                 screenPoint = gt.TransformPoint(new Point(ptrPt.Position.X, ptrPt.Position.Y));
                 details += "\nPointer Id: " + ptrPt.PointerId.ToString() +
                     "\nPointer location (parent): " + ptrPt.Position.X + ", " + ptrPt.Position.Y +
                     "\nPointer location (screen): " + screenPoint.X + ", " + screenPoint.Y;
                 return details;
             }
```

### <a name="complete-example"></a>전체 예제

다음은 C\# 코드 예제입니다. 좀 더 복잡한 샘플 링크는 이 페이지 하단에 있는 관련 문서를 참조하세요.

```CSharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Input;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;

// The Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=234238

namespace PointerInput
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        // For this example, we track simultaneous contacts in case the 
        // number of contacts has reached the maximum supported by the device.
        // Depending on the device, additional contacts might be ignored 
        // (PointerPressed not fired). 
        uint numActiveContacts;
        Windows.Devices.Input.TouchCapabilities touchCapabilities = new Windows.Devices.Input.TouchCapabilities();

        // Dictionary to maintain information about each active contact. 
        // An entry is added during PointerPressed/PointerEntered events and removed 
        // during PointerReleased/PointerCaptureLost/PointerCanceled/PointerExited events.
        Dictionary<uint, Windows.UI.Xaml.Input.Pointer> contacts;

        public MainPage()
        {
            this.InitializeComponent();
            numActiveContacts = 0;
            // Initialize the dictionary.
            contacts = new Dictionary<uint, Windows.UI.Xaml.Input.Pointer>((int)touchCapabilities.Contacts);
            // Declare the pointer event handlers.
            Target.PointerPressed += new PointerEventHandler(Target_PointerPressed);
            Target.PointerEntered += new PointerEventHandler(Target_PointerEntered);
            Target.PointerReleased += new PointerEventHandler(Target_PointerReleased);
            Target.PointerExited += new PointerEventHandler(Target_PointerExited);
            Target.PointerCanceled += new PointerEventHandler(Target_PointerCanceled);
            Target.PointerCaptureLost += new PointerEventHandler(Target_PointerCaptureLost);
            Target.PointerMoved += new PointerEventHandler(Target_PointerMoved);
            Target.PointerWheelChanged += new PointerEventHandler(Target_PointerWheelChanged);

            buttonClear.Click += new RoutedEventHandler(ButtonClear_Click);
        }

        private void ButtonClear_Click(object sender, RoutedEventArgs e)
        {
            eventLog.Text = "";
        }


        // PointerPressed and PointerReleased events do not always occur in pairs. 
        // Your app should listen for and handle any event that might conclude a pointer down action 
        // (such as PointerExited, PointerCanceled, and PointerCaptureLost).
        // For this example, we track the number of contacts in case the 
        // number of contacts has reached the maximum supported by the device.
        // Depending on the device, additional contacts might be ignored 
        // (PointerPressed not fired). 
        void Target_PointerPressed(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nDown: " + ptr.PointerId;

            // Change background color of target when pointer contact detected.
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Green);

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Lock the pointer to the target.
            Target.CapturePointer(e.Pointer);

            // Update event sequence.
            eventLog.Text += "\nPointer captured: " + ptr.PointerId;

            // Check if pointer already exists (for example, enter occurred prior to press).
            if (contacts.ContainsKey(ptr.PointerId))
            {
                return;
            }
            // Add contact to dictionary.
            contacts[ptr.PointerId] = ptr;
            ++numActiveContacts;

            // Display pointer details.
            createInfoPop(e);
        }

        void Target_PointerReleased(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nUp: " + ptr.PointerId;

            // If event source is mouse or touchpad and the pointer is still 
            // over the target, retain pointer and pointer details.
            // Return without removing pointer from pointers dictionary.
            // For this example, we assume a maximum of one mouse pointer.
            if (ptr.PointerDeviceType != Windows.Devices.Input.PointerDeviceType.Mouse)
            {
                // Update target UI.
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);

                destroyInfoPop(ptr);

                // Remove contact from dictionary.
                if (contacts.ContainsKey(ptr.PointerId))
                {
                    contacts[ptr.PointerId] = null;
                    contacts.Remove(ptr.PointerId);
                    --numActiveContacts;
                }

                // Release the pointer from the target.
                Target.ReleasePointerCapture(e.Pointer);

                // Update event sequence.
                eventLog.Text += "\nPointer released: " + ptr.PointerId;

                // Prevent most handlers along the event route from handling the same event again.
                e.Handled = true;
            }
            else
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
            }

        }

        private void Target_PointerMoved(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

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
            if (ptr.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse)
            {
                // To get mouse state, we need extended pointer details.
                // We get the pointer info through the getCurrentPoint method
                // of the event argument. 
                Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                if (ptrPt.Properties.IsLeftButtonPressed)
                {
                    eventLog.Text += "\nLeft button: " + ptrPt.PointerId;
                }
                if (ptrPt.Properties.IsMiddleButtonPressed)
                {
                    eventLog.Text += "\nWheel button: " + ptrPt.PointerId;
                }
                if (ptrPt.Properties.IsRightButtonPressed)
                {
                    eventLog.Text += "\nRight button: " + ptrPt.PointerId;
                }
            }

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Display pointer details.
            updateInfoPop(e);
        }

        private void Target_PointerEntered(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nEntered: " + ptr.PointerId;

            if (contacts.Count == 0)
            {
                // Change background color of target when pointer contact detected.
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
            }

            // Check if pointer already exists (if enter occurred prior to down).
            if (contacts.ContainsKey(ptr.PointerId))
            {
                return;
            }

            // Add contact to dictionary.
            contacts[ptr.PointerId] = ptr;
            ++numActiveContacts;

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Display pointer details.
            createInfoPop(e);
        }

        private void Target_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nMouse wheel: " + ptr.PointerId;

            // Check if pointer already exists (for example, enter occurred prior to wheel).
            if (contacts.ContainsKey(ptr.PointerId))
            {
                return;
            }

            // Add contact to dictionary.
            contacts[ptr.PointerId] = ptr;
            ++numActiveContacts;

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Display pointer details.
            createInfoPop(e);
        }

        // Fires for for various reasons, including: 
        //    - User interactions
        //    - Programmatic capture of another pointer
        //    - Captured pointer was deliberately released
        // PointerCaptureLost can fire instead of PointerReleased. 
        private void Target_PointerCaptureLost(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nPointer capture lost: " + ptr.PointerId;

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            destroyInfoPop(ptr);

            if (contacts.Count == 0)
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
            }
            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }

        // Fires for for various reasons, including: 
        //    - Touch contact canceled by pen coming into range of the surface.
        //    - The device doesn&#39;t report an active contact for more than 100ms.
        //    - The desktop is locked or the user logged off. 
        //    - The number of simultaneous contacts exceeded the number supported by the device.
        private void Target_PointerCanceled(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nPointer canceled: " + ptr.PointerId;

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            destroyInfoPop(ptr);

            if (contacts.Count == 0)
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
            }
            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }

        private void Target_PointerExited(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nPointer exited: " + ptr.PointerId;

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            // Update the UI and pointer details.
            destroyInfoPop(ptr);

            if (contacts.Count == 0)
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);
            }
            
            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }

        void createInfoPop(PointerRoutedEventArgs e)
        {
            TextBlock pointerDetails = new TextBlock();
            Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
            pointerDetails.Name = ptrPt.PointerId.ToString();
            pointerDetails.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
            pointerDetails.Text = queryPointer(ptrPt);

            TranslateTransform x = new TranslateTransform();
            x.X = ptrPt.Position.X + 20;
            x.Y = ptrPt.Position.Y + 20;
            pointerDetails.RenderTransform = x;

            Container.Children.Add(pointerDetails);
        }

        void destroyInfoPop(Windows.UI.Xaml.Input.Pointer ptr)
        {
            foreach (var pointerDetails in Container.Children)
            {
                if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
                {
                    TextBlock _TextBlock = (TextBlock)pointerDetails;
                    if (_TextBlock.Name == ptr.PointerId.ToString())
                    {
                        Container.Children.Remove(pointerDetails);
                    }
                }
            }
        }

        void updateInfoPop(PointerRoutedEventArgs e)
        {
            foreach (var pointerDetails in Container.Children)
            {
                if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
                {
                    TextBlock _TextBlock = (TextBlock)pointerDetails;
                    if (_TextBlock.Name == e.Pointer.PointerId.ToString())
                    {
                        // To get pointer location details, we need extended pointer info.
                        // We get the pointer info through the getCurrentPoint method
                        // of the event argument. 
                        Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                        TranslateTransform x = new TranslateTransform();
                        x.X = ptrPt.Position.X + 20;
                        x.Y = ptrPt.Position.Y + 20;
                        pointerDetails.RenderTransform = x;
                        _TextBlock.Text = queryPointer(ptrPt);
                    }
                }
            }
        }

         String queryPointer(PointerPoint ptrPt)
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

             GeneralTransform gt = Target.TransformToVisual(page);
             Point screenPoint;

             screenPoint = gt.TransformPoint(new Point(ptrPt.Position.X, ptrPt.Position.Y));
             details += "\nPointer Id: " + ptrPt.PointerId.ToString() +
                 "\nPointer location (parent): " + ptrPt.Position.X + ", " + ptrPt.Position.Y +
                 "\nPointer location (screen): " + screenPoint.X + ", " + screenPoint.Y;
             return details;
         }
    }
}
```

## <a name="related-articles"></a>관련 문서


**샘플**
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
 

 







<!--HONumber=Dec16_HO3-->


