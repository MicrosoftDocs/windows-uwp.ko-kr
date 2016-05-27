---
author: QuinnRadich
Description: Windows 10 및 새 개발자 도구는 새 UWP(유니버설 Windows 플랫폼)에서 지원하는 도구, 기능 및 환경을 제공합니다.
title: Windows 10, RTM - 2015년 7월의 개발자용 새로운 기능
---

# Windows 10, RTM&#58; 2015년 7월의 개발자용 새로운 기능


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

Windows 10 및 새 개발자 도구는 새 UWP(유니버설 Windows 플랫폼)에서 지원하는 도구, 기능 및 환경을 제공합니다. Windows 10에 [도구 및 SDK를 설치](https://dev.windows.com/downloads)한 후 [새 유니버설 Windows 앱을 만들](https://msdn.microsoft.com/library/windows/apps/bg124288)거나 [Windows에서 기존 앱 코드](https://msdn.microsoft.com/library/windows/apps/mt238321)를 사용하는 방법을 살펴볼 수 있습니다.

Windows 10, RTM의 새로운 기능을 기능별로 살펴보면 다음과 같습니다.

## 적응형 레이아웃


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">맞춤형 콘텐츠에 대한 여러 보기</td>
<td align="left">XAML은 동일한 코드 파일을 공유하는 맞춤형 보기(.xaml 파일)를 정의하기 위한 새로운 지원을 제공합니다. 이 지원을 사용하면 특정 디바이스 패밀리 또는 시나리오에 맞게 조정된 여러 보기를 보다 쉽게 만들고 유지 관리할 수 있습니다. 앱에 시나리오마다 전혀 다른 고유한 UI 콘텐츠, 레이아웃 또는 탐색 모델이 있는 경우 여러 보기를 빌드합니다. 예를 들어 모바일 앱에서는 탐색이 한 손 사용에 최적화된 [<strong>Pivot</strong>](https://msdn.microsoft.com/library/windows/apps/dn608241)을 사용하지만, 데스크톱 앱에서는 탐색 메뉴가 마우스에 최적화된 [<strong>SplitView</strong>](https://msdn.microsoft.com/library/windows/apps/dn864360)를 사용할 수 있습니다.</td>
</tr>
<tr class="even">
<td align="left">StateTriggers</td>
<td align="left">새 [<strong>VisualState.StateTriggers</strong>](https://msdn.microsoft.com/library/windows/apps/dn890396) 기능을 사용하여 창 높이/너비에 따라 또는 사용자 지정 트리거에 따라 조건부로 속성을 설정할 수 있습니다. 이전에는 코드에서 창 [<strong>SizeChanged</strong>](https://msdn.microsoft.com/library/windows/apps/br209055) 이벤트를 처리하고 [<strong>VisualStateManager.GoToState</strong>](https://msdn.microsoft.com/library/windows/apps/br209025)를 호출해야 했습니다.</td>
</tr>
<tr class="odd">
<td align="left">Setter</td>
<td align="left"><p>새 [<strong>VisualState.Setters</strong>](https://msdn.microsoft.com/library/windows/apps/dn890395) 구문을 사용하면 간소화된 태그로 [<strong>VisualStateManager</strong>](https://msdn.microsoft.com/library/windows/apps/br209021)에서 속성 변경 내용을 정의할 수 있습니다. 이전에는 스토리보드를 사용하고 애니메이션을 만들어 [<strong>StackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/br209635)의 방향을 가로에서 세로로 변경하는 등의 속성 변경 내용을 적용해야 했습니다. 유니버설 Windows 앱에서는 보다 간단한 다음 Setter 구문을 사용할 수 있습니다.</p>
<p><code>&lt;setter target=&quot;stackPanel1.Orientation&quot; value=&quot;Vertical&quot; /&gt;</code></p></td>
</tr>
</tbody>
</table>

 

## XAML 기능


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">컴파일된 데이터 바인딩(x:Bind)</td>
<td align="left"><p>유니버설 Windows 앱에서는 x:Bind 속성을 통해 활성화되는 새 컴파일러 기반 바인딩 메커니즘을 사용할 수 있습니다. 컴파일러 기반 바인딩은 강력한 형식이며 컴파일 타임에 처리되므로 더 빠르고 바인딩 형식이 일치하지 않을 경우 컴파일 타임 오류를 제공합니다. 또한 바인딩이 컴파일된 앱 코드로 변환되기 때문에 이제 Visual Studio에서 코드를 단계별로 실행하여 특정 바인딩 문제를 진단함으로써 바인딩을 디버그할 수 있습니다. 다음과 같이 x:Bind를 사용하여 메서드에 바인딩할 수도 있습니다.</p>
<p><code>&lt;textblock text=&quot;{x:Bind Customer.Address.ToString()}&quot; /&gt;</code></p>
<p>일반적인 바인딩 시나리오의 경우 바인딩 대신 x:Bind를 사용하여 향상된 성능 및 유지 관리 능력을 얻을 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left">목록의 선언적 증분 렌더링(x:Phase)</td>
<td align="left"><p>유니버설 Windows 앱에서 새 x:Phase 특성을 사용하면 코드 대신 XAML을 통해 목록의 증분 또는 단계별 렌더링을 수행할 수 있습니다. 복잡한 항목이 포함된 긴 목록을 이동하는 경우 앱이 이동 속도를 따라갈 만큼 빠르게 항목을 렌더링하지 못해 좋지 않은 사용자 경험을 만들 수 있습니다. 단계별 렌더링을 사용하면 목록 항목에 포함된 개별 요소의 렌더링 우선 순위를 지정할 수 있으므로 빠르게 이동하는 시나리오에서는 목록 항목의 가장 중요한 부분만 렌더링됩니다. 따라서 사용자에게 보다 매끄러운 이동 경험을 제공할 수 있습니다.</p>
<p>Windows 8.1에서는 [<strong>ContainerContentChanging</strong>](https://msdn.microsoft.com/library/windows/apps/dn298914) 이벤트를 처리하고 목록 항목을 단계별로 렌더링하는 코드를 작성할 수 있었습니다. UWP 앱에서 x:Phase 특성을 사용하여 선언적으로 단계별 렌더링을 수행할 수 있습니다. 컴파일된 바인딩 x:Bind와 함께 x:Phase를 사용하면 데이터 템플릿의 각 바인딩된 요소에 대해 렌더링 우선 순위를 쉽게 지정할 수 있습니다. 이동 시 항목을 렌더링하는 작업은 증분 항목 렌더링을 사용하도록 설정하는 단계에 따라 시간 분할됩니다.</p></td>
</tr>
<tr class="odd">
<td align="left">UI 요소의 지연된 로드(x:DeferLoadingStrategy)</td>
<td align="left">유니버설 Windows 앱에서 새 x:DeferLoadingStrategy 지시문을 사용하면 지연 로드될 사용자 인터페이스 부분을 지정할 수 있으므로 시작 성능이 향상되고 앱의 메모리 사용이 감소합니다. 예를 들어 앱 UI에 잘못된 데이터를 입력한 경우에만 표시되는 데이터 유효성 검사 요소가 있는 경우 필요할 때까지 해당 요소의 로드를 지연할 수 있습니다. 그러면 페이지를 로드할 때 요소 개체가 생성되지 않고, 데이터 오류가 있으며 페이지의 시각적 트리에 추가해야 하는 경우에만 생성됩니다.</td>
</tr>
<tr class="even">
<td align="left">SplitView</td>
<td align="left">새 [<strong>SplitView</strong>](https://msdn.microsoft.com/library/windows/apps/dn864360) 컨트롤은 임시 콘텐츠를 쉽게 표시하고 숨기는 방법을 제공합니다. 이 컨트롤은 일반적으로 탐색 콘텐츠가 숨겨져 있다 사용자 동작으로 필요 시 표시되는 &quot;햄버거 메뉴&quot;와 같은 최상위 탐색 시나리오에 사용됩니다.</td>
</tr>
<tr class="odd">
<td align="left">RelativePanel</td>
<td align="left">[<strong>RelativePanel</strong>](https://msdn.microsoft.com/library/windows/apps/dn879546)은 서로 또는 부모 패널과 관련하여 자식 개체를 배치하고 정렬할 수 있는 새 레이아웃 패널입니다. 예를 들어 일부 텍스트는 항상 패널 왼쪽에 배치되고 단추는 항상 텍스트 아래에 정렬되도록 지정할 수 있습니다. [<strong>StackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/br209635) 또는 [<strong>Grid</strong>](https://msdn.microsoft.com/library/windows/apps/br242704)를 사용해야 하는 명확한 선형 패턴이 없는 사용자 인터페이스를 만드는 경우 <strong>RelativePanel</strong>을 사용합니다.</td>
</tr>
<tr class="even">
<td align="left">CalendarView</td>
<td align="left">[<strong>CalendarView</strong>](https://msdn.microsoft.com/library/windows/apps/dn890052) 컨트롤을 사용하면 사용자 지정 가능한 월 기반 보기를 통해 날짜 및 날짜 범위를 쉽게 보고 선택할 수 있습니다. <strong>CalendarView</strong>는 선택할 수 있는 날짜를 제한하는 최소, 최대, 블랙아웃 날짜 등의 기능을 지원합니다. 또한 특정 날짜에 일정의 일반적 &quot;예약률&quot;을 표시하는 데 사용할 수 있는 사용자 지정 밀도 막대를 설정할 수 있습니다.</td>
</tr>
<tr class="odd">
<td align="left">CalendarDatePicker</td>
<td align="left">[<strong>CalendarDatePicker</strong>](https://msdn.microsoft.com/library/windows/apps/dn950083)는 요일이나 일정의 예약률과 같이 컨텍스트 정보가 중요한 [<strong>CalendarView</strong>](https://msdn.microsoft.com/library/windows/apps/dn890052)에서 단일 날짜를 선택하는 데 최적화된 드롭다운 컨트롤입니다. [<strong>DatePicker</strong>](https://msdn.microsoft.com/library/windows/apps/dn298584) 컨트롤과 유사하지만 <strong>DatePicker</strong>는 생년월일과 같은 알려진 날짜를 선택하는 데 최적화되어 있습니다.</td>
</tr>
<tr class="even">
<td align="left">MediaTransportControls</td>
<td align="left">새 [<strong>MediaTransportControls</strong>](https://msdn.microsoft.com/library/windows/apps/dn831962) 클래스를 사용하면 [<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926)의 전송 컨트롤을 쉽게 사용자 지정할 수 있습니다. Windows 8.1에서는 <strong>MediaElement</strong>의 기본 제공 전송 컨트롤을 사용하도록 설정하거나, <strong>MediaElement</strong> 메서드를 호출하는 고유한 전송 컨트롤을 만들 수 있었습니다. 이제 <strong>MediaTransportControls</strong>의 기본 제공 기능을 사용하고 앱에 맞게 모양을 쉽게 사용자 지정할 수 있습니다.</td>
</tr>
<tr class="odd">
<td align="left">속성 변경 알림</td>
<td align="left"><p>유니버설 Windows 앱에서는 해당하는 변경 이벤트가 없는 속성의 경우에도 [<strong>DependencyObject</strong>](https://msdn.microsoft.com/library/windows/apps/br242356)의 속성 변경을 수신 대기할 수 있습니다.</p>
<p>알림은 이벤트처럼 작동하지만 실제로는 콜백으로 노출됩니다. 콜백은 이벤트 처리기처럼 발신자 인수를 사용하지만 이벤트 인수는 사용하지 않습니다. 대신 어떤 속성인지 나타내기 위해 속성 식별자만 전달됩니다. 이 정보를 사용하여 앱은 여러 속성 알림에 대한 단일 처리기를 정의할 수 있습니다. 자세한 내용은 [<strong>RegisterPropertyChangedCallback</strong>](https://msdn.microsoft.com/library/windows/apps/dn831902) 및 [<strong>UnregisterPropertyChangedCallback</strong>](https://msdn.microsoft.com/library/windows/apps/dn831910)을 참조하세요.</p></td>
</tr>
<tr class="even">
<td align="left">지도</td>
<td align="left"><p>[<strong>MapControl</strong>](https://msdn.microsoft.com/library/windows/apps/dn637004) 클래스는 위성 3D 이미지 및 거리 수준 보기를 제공하도록 업데이트되었습니다. 이 새로운 기능 및 이전의 매핑 기능을 이제 유니버설 Windows 앱에서 사용할 수 있습니다. 다음 API를 사용하여 앱에 매핑을 추가하세요.</p>
<ul>
<li>[<strong>Windows.UI.Xaml.Controls.Maps</strong>](https://msdn.microsoft.com/library/windows/apps/dn610751) 네임스페이스 - 지도를 표시합니다.</li>
<li>[<strong>Windows.Services.Maps</strong>](https://msdn.microsoft.com/library/windows/apps/dn636979) 네임스페이스 - 위치 및 경로를 찾습니다.</li>
</ul>
<p>지금 유니버설 Windows 앱에서 이러한 API 사용을 시작하려면 [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)에서 키를 요청하세요. 자세한 내용은 [지도 앱을 인증하는 방법](https://msdn.microsoft.com/library/windows/apps/xaml/dn741528)을 참조하세요. 또한 Windows 10의 새로운 기능으로, PC 및 휴대폰 사용자는 설정 앱에서 오프라인 지도를 다운로드할 수 있습니다. 사용 가능한 경우 오프라인 지도는 [<strong>MapControl</strong>](https://msdn.microsoft.com/library/windows/apps/dn637004)이 인터넷에 액세스할 수 없을 때 지도를 표시하는 데 사용됩니다.</p></td>
</tr>
<tr class="odd">
<td align="left">입력 단추 매핑</td>
<td align="left">[<strong>Windows.UI.Xaml.Input.KeyRoutedEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/hh943072) 클래스에는 해당하는 [<strong>Windows.System.VirtualKey</strong>](https://msdn.microsoft.com/library/windows/apps/br241812) 업데이트와 더불어 매핑되지 않은 원래 입력 단추를 키보드 입력 이벤트와 연결할 수 있게 해주는 새 [<strong>OriginalKey</strong>](https://msdn.microsoft.com/library/windows/apps/dn960168) 속성이 있습니다.</td>
</tr>
<tr class="even">
<td align="left">수동 입력</td>
<td align="left"><p>[<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535) 컨트롤과 기본 [<strong>InkPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn922011) 클래스로 인해 C++, C# 또는 Visual Basic을 사용하는 Windows 런타임 앱의 강력한 수동 입력 기능을 이제 간단하게 사용할 수 있습니다.</p>
<p>[<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535) 컨트롤은 잉크 스트로크를 그리고 렌더링하기 위한 오버레이 영역을 정의합니다. 이 컨트롤의 기능(입력, 처리 및 렌더링)은 [<strong>InkPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn922011), [<strong>InkStroke</strong>](https://msdn.microsoft.com/library/windows/apps/br208485), [<strong>InkRecognizer</strong>](https://msdn.microsoft.com/library/windows/apps/br208478) 및 [<strong>InkSynchronizer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903979) 클래스에서 제공됩니다.</p>
<p></p>
<div class="alert">
<strong>중요</strong> JavaScript를 사용하는 Windows 앱에서는 이러한 클래스가 지원되지 않습니다.
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

## 업데이트된 XAML 기능


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">CommandBar 및 AppBar 업데이트</td>
<td align="left"><p>[<strong>CommandBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn279427) 및 [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) 컨트롤이 다양한 디바이스 패밀리에서 UWP 앱에 대해 일관적 API, 동작, 사용자 환경을 제공하도록 업데이트되었습니다.</p>
<p>유니버설 Windows 앱의 [<strong>CommandBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn279427) 컨트롤은 [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) 기능의 상위 집합을 제공하고 앱에서 보다 유연하게 사용할 수 있도록 향상되었습니다. Windows 10의 모든 새 유니버설 Windows 앱에 <strong>CommandBar</strong>를 사용해야 합니다. Windows 8.1의 <strong>CommandBar</strong>에서는 [<strong>AppBarButton</strong>](https://msdn.microsoft.com/library/windows/apps/dn279244)과 같은 [<strong>ICommandBarElement</strong>](https://msdn.microsoft.com/library/windows/apps/dn251969)를 구현한 컨트롤만 사용할 수 있습니다. 유니버설 Windows 앱에서는 이제 <strong>AppBarButton</strong>뿐만 아니라 <strong>CommandBar</strong>에 사용자 지정 콘텐츠를 추가할 수 있습니다.</p>
<p>[<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) 컨트롤이 업데이트되어 이제 <strong>AppBar</strong>를 사용하는 Windows 8.1 앱을 유니버설 Windows 플랫폼으로 쉽게 이동할 수 있습니다. <strong>AppBar</strong>는 전체 화면 앱과 함께 사용하고 가장자리 제스처를 통해 호출하도록 설계되었습니다. 컨트롤 업데이트에서는 Windows 10의 가장자리 제스처 부족, 창 앱 등의 문제를 고려합니다.</p>
<p>이전에 Windows Phone에만 있었던 숨겨진 [<strong>AppBar.ClosedDisplayMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn633872)가 이제 모든 디바이스 패밀리에서 지원되므로 명령의 다양한 힌트 수준 사이에서 선택할 수 있습니다. [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927)는 Windows 8.1 앱을 플랫폼의 가장자리 제스처 지원에 더 이상 사용할 수 없는 유니버설 Windows 앱으로 업그레이드할 때 일관성을 제공하기 위해 기본적으로 최소한의 힌트만 표시합니다.</p>
<p><strong>새</strong> [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) <strong>API:</strong>[<strong>Closing</strong>](https://msdn.microsoft.com/library/windows/apps/dn996483), [<strong>OnClosing</strong>](https://msdn.microsoft.com/library/windows/apps/dn996484), [<strong>Opening</strong>](https://msdn.microsoft.com/library/windows/apps/dn996486), [<strong>OnOpening</strong>](https://msdn.microsoft.com/library/windows/apps/dn996485), [<strong>TemplateSettings</strong>](https://msdn.microsoft.com/library/windows/apps/dn996487).</p>
<p><strong>새</strong> [<strong>CommandBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn279427) <strong>API:</strong>[<strong>CommandBarOverflowPresenterStyle</strong>](https://msdn.microsoft.com/library/windows/apps/dn975227) 및 [<strong>CommandBarOverflowPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn975225).</p></td>
</tr>
<tr class="even">
<td align="left">GridView 업데이트</td>
<td align="left">Windows 10 이전에는 기본 [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) 레이아웃 방향이 Windows의 경우 가로였고 Windows Phone의 경우 세로였습니다. UWP 앱에서는 <strong>GridView</strong>가 기본적으로 모든 디바이스 패밀리에 대해 세로 레이아웃을 사용하여 일관된 기본 환경을 제공합니다.</td>
</tr>
<tr class="odd">
<td align="left">AreStickyGroupHeadersEnabled 속성</td>
<td align="left">[<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) 또는 [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705)에 그룹화된 데이터를 표시하는 경우 이제 목록을 스크롤할 때 그룹 헤더가 계속 표시됩니다. 헤더가 사용자가 보고 있는 데이터에 대한 컨텍스트를 제공하는 큰 데이터 집합에서는 이 기능이 중요합니다. 그러나 각 그룹에 몇 개의 요소만 있는 경우 항목과 함께 헤더가 화면 밖으로 스크롤되게 할 수 있습니다. [<strong>ItemsStackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/dn298795) 및 [<strong>ItemsWrapGrid</strong>](https://msdn.microsoft.com/library/windows/apps/dn298849)의 [<strong>AreStickyGroupHeadersEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn932028) 속성을 설정하여 이 동작을 제어할 수 있습니다.</td>
</tr>
<tr class="even">
<td align="left">GroupHeaderContainerFromItemContainer 메서드</td>
<td align="left">그룹화된 데이터를 [<strong>ItemsControl</strong>](https://msdn.microsoft.com/library/windows/apps/br242803)에 표시하는 경우 [<strong>GroupHeaderContainerFromItemContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903984) 메서드를 호출하여 그룹의 부모 헤더에 대한 참조를 가져올 수 있습니다. 예를 들어 사용자가 그룹의 마지막 항목을 삭제하는 경우 그룹 헤더에 대한 참조를 가져오고 항목과 그룹 헤더를 둘 다 제거할 수 있습니다.</td>
</tr>
<tr class="odd">
<td align="left">ChoosingGroupHeaderContainer 이벤트</td>
<td align="left">[<strong>ListViewBase</strong>](https://msdn.microsoft.com/library/windows/apps/br242879)에 대한 새 [<strong>ChoosingGroupHeaderContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn899082) 이벤트를 사용하면 [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) 또는 [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705)에서 그룹 헤더의 상태를 설정할 수 있습니다. 예를 들어 보조 기술에서 그룹을 나타내기 위해 이 이벤트를 처리하여 그룹 헤더의 [<strong>AutomationProperties.NameProperty</strong>](https://msdn.microsoft.com/library/windows/apps/br209099)를 설정할 수 있습니다.</td>
</tr>
<tr class="even">
<td align="left">ChoosingItemContainer 이벤트</td>
<td align="left">[<strong>ListViewBase</strong>](https://msdn.microsoft.com/library/windows/apps/br242879)에 대한 새 [<strong>ChoosingItemContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903989) 이벤트를 사용하면 [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) 또는 [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705)에서 UI 가상화를 보다 강력하게 제어할 수 있습니다. [<strong>ContainerContentChanging</strong>](https://msdn.microsoft.com/library/windows/apps/dn298914) 이벤트와 함께 이 이벤트를 사용하여 필요에 따라 이용할 고유한 재활용 컨테이너 큐를 유지 관리할 수 있습니다. 예를 들어 필터링으로 인해 데이터 원본이 다시 설정된 경우 최상의 성능을 얻기 위해 이미 만들어진 시각 효과 집합(ItemContainers)을 해당 데이터와 빠르게 일치시킬 수 있습니다.</td>
</tr>
<tr class="odd">
<td align="left">목록 스크롤 가상화</td>
<td align="left"><p>XAML [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) 및 [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) 컨트롤에는 데이터 컬렉션에서 변경이 발생할 때 컨트롤의 성능을 향상하는 새 [<strong>ListViewBase.ChoosingItemContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903989) 이벤트가 있습니다.</p>
<p>시스템은 이제 목록을 완전히 다시 설정하여 시작 애니메이션을 재생하는 대신, 포커스 및 선택 상태를 유지하면서 보기에서 항목을 유지합니다. 뷰포트의 새 항목과 제거된 항목이 애니메이션 효과를 통해 매끄럽게 나타나고 사라집니다. 데이터 컬렉션에서 컨테이너가 소멸되지 않는 변경이 수행되면 앱은 이전 컨테이너에서 &quot;이전&quot; 항목과 일치하는 항목을 빠르게 검색하고 컨테이너 수명 주기 재정의 메서드의 추가 처리를 건너뛸 수 있습니다. &quot;새&quot; 항목만 처리되고 재생된 컨테이너 또는 새 컨테이너와 연결됩니다.</p></td>
</tr>
<tr class="even">
<td align="left">SelectRange 메서드 및 SelectedRanges 속성</td>
<td align="left">이제 유니버설 Windows 앱에서 [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) 및 [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) 컨트롤을 사용하여 항목 개체 참조가 아닌 항목 색인 범위로 항목을 선택할 수 있습니다. 이 방법에서는 선택한 각 항목에 대해 항목 개체를 만들지 않아도 되므로 항목 선택을 설명하는 더욱 효율적인 방법입니다. 자세한 내용은 [<strong>ListViewBase.SelectedRanges</strong>](https://msdn.microsoft.com/library/windows/apps/dn904244), [<strong>ListViewBase.SelectRange</strong>](https://msdn.microsoft.com/library/windows/apps/dn904245) 및 [<strong>ListViewBase.DeselectRange</strong>](https://msdn.microsoft.com/library/windows/apps/dn904241)를 참조하세요.</td>
</tr>
<tr class="odd">
<td align="left">새 ListViewItemPresenter API</td>
<td align="left">[<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) 및 [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705)에서는 항목 프리젠터를 사용하여 선택 및 포커스에 대한 기본 화면 효과를 제공합니다. UWP 앱에서 [<strong>ListViewItemPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn298500) 및 [<strong>GridViewItemPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn279298)는 목록 항목에 대한 화면 효과를 추가로 사용자 지정할 수 있는 새 속성이 있습니다. 새 속성은 [<strong>CheckBoxBrush</strong>](https://msdn.microsoft.com/library/windows/apps/dn913905), [<strong>CheckMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn913923), [<strong>FocusSecondaryBorderBrush</strong>](https://msdn.microsoft.com/library/windows/apps/dn898370), [<strong>PointerOverForeground</strong>](https://msdn.microsoft.com/library/windows/apps/dn898372), [<strong>PressedBackground</strong>](https://msdn.microsoft.com/library/windows/apps/dn913931) 및 [<strong>SelectedPressedBackground</strong>] (https://msdn.microsoft.com/library/windows/apps/dn913937)입니다.</td>
</tr>
<tr class="even">
<td align="left">SemanticZoom 업데이트</td>
<td align="left"><p>이제 [<strong>SemanticZoom</strong>](https://msdn.microsoft.com/library/windows/apps/hh702601) 컨트롤이 모든 디바이스 패밀리의 UWP 앱에 대해 일관된 동작을 제공합니다.</p>
<p>확대 보기와 축소 보기 간에 전환하는 기본 동작은 축소 보기에서 그룹 헤더를 탭하는 것입니다. 이는 Windows Phone 8.1의 동작과 같지만, 손가락 모으기 제스처를 사용하여 확대/축소한 Windows 8.1에서 변경되었습니다. 손가락을 모아 확대/축소를 사용하여 보기를 변경하려면 [<strong>SemanticZoom</strong>](https://msdn.microsoft.com/library/windows/apps/hh702601)의 내부 [<strong>ScrollViewer</strong>](https://msdn.microsoft.com/library/windows/apps/br209527)에서 [<strong>ScrollViewer.ZoomMode</strong>](https://msdn.microsoft.com/library/windows/apps/br209601)=&quot;Enabled&quot;를 설정합니다.</p>
<p>유니버설 Windows 앱의 경우 축소 보기가 확대 보기를 대체하며 대체된 보기와 크기가 같습니다. 이는 Windows 8.1의 동작과 같지만, 축소 보기가 전체 화면 크기를 차지하고 다른 모든 콘텐츠에 위에 렌더링된 Windows Phone 8.1에서 변경되었습니다.</p></td>
</tr>
<tr class="odd">
<td align="left">DatePicker 및 TimePicker 업데이트</td>
<td align="left">이제 [<strong>DatePicker</strong>](https://msdn.microsoft.com/library/windows/apps/dn298584) 및 [<strong>TimePicker</strong>](https://msdn.microsoft.com/library/windows/apps/dn299280) 컨트롤이 모든 디바이스 패밀리의 유니버설 Windows 앱에 대해 하나의 일관된 구현을 제공합니다. 또한 Windows 10에 대해 새로운 모양을 제공합니다. 이제 팝업 부분이 모든 디바이스에서 [<strong>DatePickerFlyout</strong>](https://msdn.microsoft.com/library/windows/apps/dn625013) 및 [<strong>TimePickerFlyout</strong>](https://msdn.microsoft.com/library/windows/apps/dn608313) 컨트롤을 사용합니다. 이는 Windows Phone 8.1의 동작과 같지만, [<strong>ComboBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209348) 컨트롤을 사용한 Windows 8.1에서 변경되었습니다. 플라이아웃 컨트롤을 사용하면 사용자 지정 날짜 및 시간 선택기를 쉽게 만들 수 있습니다.</td>
</tr>
<tr class="even">
<td align="left">새 ScrollViewer API</td>
<td align="left">[<strong>ScrollViewer</strong>](https://msdn.microsoft.com/library/windows/apps/br209527)에는 터치 이동이 시작 및 중지될 때 앱에 알리기 위한 새 [<strong>DirectManipulationStarted</strong>](https://msdn.microsoft.com/library/windows/apps/dn858544) 및 [<strong>DirectManipulationCompleted</strong>](https://msdn.microsoft.com/library/windows/apps/dn858543) 이벤트가 있습니다. 이러한 이벤트를 처리하여 해당 사용자 작업으로 UI를 조정할 수 있습니다.</td>
</tr>
<tr class="odd">
<td align="left">MenuFlyout 업데이트</td>
<td align="left">유니버설 Windows 앱에는 더 나은 상황에 맞는 메뉴를 더 쉽게 빌드할 수 있는 새 API가 있습니다. 새 [<strong>MenuFlyout.ShowAt</strong>](https://msdn.microsoft.com/library/windows/apps/dn913174) 메서드를 사용하면 다른 요소와 관련해서 플라이아웃을 표시할 위치를 지정할 수 있습니다. [<strong>MenuFlyout</strong>](https://msdn.microsoft.com/library/windows/apps/dn299030)은 앱 창의 경계를 겹칠 수도 있습니다. 새 [<strong>MenuFlyoutSubItem</strong>](https://msdn.microsoft.com/library/windows/apps/dn913170) 클래스를 사용하면 계단식 메뉴를 만들 수 있습니다.</td>
</tr>
<tr class="even">
<td align="left">ContentPresenter, Grid 및 StackPanel의 새 테두리 속성</td>
<td align="left">일반적인 컨테이너 컨트롤에는 새 테두리 속성이 있으므로 이러한 속성을 사용하면 XAML에 Border 요소를 추가하지 않고 주위에 테두리를 그릴 수 있습니다. [<strong>ContentPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/br209378), [<strong>Grid</strong>](https://msdn.microsoft.com/library/windows/apps/br242704) 및 [<strong>StackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/br209635)에는 [<strong>BorderBrush</strong>](https://msdn.microsoft.com/library/windows/apps/dn960179), [<strong>BorderThickness</strong>](https://msdn.microsoft.com/library/windows/apps/dn960181), [<strong>CornerRadius</strong>](https://msdn.microsoft.com/library/windows/apps/dn960183), [<strong>Padding</strong>](https://msdn.microsoft.com/library/windows/apps/dn960187) 등의 새 속성이 있습니다.</td>
</tr>
<tr class="odd">
<td align="left">ContentPresenter의 새 텍스트 API</td>
<td align="left">[<strong>ContentPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/br209378)에는 텍스트 표시를 보다 강력하게 제어할 수 있는 [<strong>LineHeight</strong>](https://msdn.microsoft.com/library/windows/apps/dn903982), [<strong>LineStackingStrategy</strong>](https://msdn.microsoft.com/library/windows/apps/dn831933), [<strong>MaxLines</strong>](https://msdn.microsoft.com/library/windows/apps/dn831935), [<strong>TextWrapping</strong>](https://msdn.microsoft.com/library/windows/apps/dn831937) 등의 새 API가 있습니다.</td>
</tr>
<tr class="even">
<td align="left">시스템 포커스 화면 효과</td>
<td align="left">XAML 컨트롤에 대한 포커스 화면 효과가 이제 컨트롤 템플릿에서 XAML 요소로 선언되지 않고 시스템에서 만들어집니다. 포커스 화면 효과는 일반적으로 모바일 디바이스에서 필요하지 않으며, 필요에 따라 시스템에서 만들고 관리하게 하면 앱 성능이 향상됩니다. 포커스 화면 효과에 대한 제어를 강화해야 하는 경우 시스템 동작을 재정의하고 포커스 화면 효과를 정의하는 사용자 지정 컨트롤 템플릿을 제공할 수 있습니다. 자세한 내용은 [<strong>UseSystemFocusVisuals</strong>](https://msdn.microsoft.com/library/windows/apps/dn899076) 및 [<strong>IsTemplateFocusTarget</strong>](https://msdn.microsoft.com/library/windows/apps/dn899074)을 참조하세요.</td>
</tr>
<tr class="odd">
<td align="left">PasswordBox.PasswordRevealMode</td>
<td align="left"><p>유니버설 Windows 앱에서는 [<strong>PasswordRevealMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn890867) 속성이 [<strong>IsPasswordRevealButtonEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/hh702579) 속성을 대체하여 디바이스 패밀리에서 일관된 동작을 제공합니다.</p>
<p></p>
<div class="alert">
<strong>주의</strong> Windows 10 이전에는 기본적으로 암호 표시 단추가 표시되지 않았습니다. 유니버설 Windows 앱에서는 기본적으로 표시됩니다. 앱 보안 정책에 따라 암호를 항상 가려야 하는 경우 [<strong>PasswordRevealMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn890867)를 Hidden으로 설정하세요.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left">Control.IsTextScaleFactorEnabled</td>
<td align="left">Windows Phone 8.1에서 사용할 수 있었던 [<strong>IsTextScaleFactorEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn624997) 속성을 이제 모든 디바이스 패밀리의 유니버설 Windows 앱에 사용할 수 있습니다.</td>
</tr>
<tr class="odd">
<td align="left">AutoSuggestBox</td>
<td align="left">Windows Phone 8.1의 [<strong>AutoSuggestBox</strong>](https://msdn.microsoft.com/library/windows/apps/dn633874) 컨트롤을 이제 모든 디바이스 패밀리의 유니버설 Windows 앱에 사용할 수 있으며, [<strong>SearchBox</strong>](https://msdn.microsoft.com/library/windows/apps/dn252771) 대신 사용해야 합니다. <strong>AutoSuggestBox</strong>는 사용자 입력 시 제안을 제공하며, 터치, 키보드, 입력기 등 다양한 입력 유형에서 잘 작동합니다. [<strong>QueryIcon</strong>](https://msdn.microsoft.com/library/windows/apps/dn996494) 속성, [<strong>QuerySubmitted</strong>](https://msdn.microsoft.com/library/windows/apps/dn996497) 이벤트 등 검색 상자로 작동을 개선하는 몇 개의 새 멤버도 있습니다.</td>
</tr>
<tr class="even">
<td align="left">ContentDialog</td>
<td align="left">이제 Windows Phone 8.1의 [<strong>ContentDialog</strong>](https://msdn.microsoft.com/library/windows/apps/dn633972) 컨트롤을 모든 디바이스 패밀리의 유니버설 Windows 앱에 사용할 수 있습니다. <strong>ContentDialog</strong>를 사용하면 전체 디바이스에서 제대로 작동하는 사용자 지정 가능한 모달 대화 상자를 표시할 수 있습니다.</td>
</tr>
<tr class="odd">
<td align="left">Pivot</td>
<td align="left">이제 Windows Phone 8.1의 [<strong>Pivot</strong>](https://msdn.microsoft.com/library/windows/apps/dn608241) 컨트롤을 모든 디바이스 패밀리의 유니버설 Windows 앱에 사용할 수 있습니다. 이제 모바일 및 데스크톱 디바이스용 앱에 동일한 <strong>Pivot</strong> 컨트롤을 사용할 수 있습니다. <strong>Pivot</strong>은 화면 크기와 입력 형식에 따라 적응형 동작을 제공합니다. <strong>Pivot</strong> 컨트롤에 스타일을 지정하면 각 피벗 항목에 다른 정보 보기를 사용하여 탭과 유사한 동작을 제공할 수 있습니다.</td>
</tr>
</tbody>
</table>

 

## Text


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Windows 코어 텍스트 API</td>
<td align="left"><p>새 [<strong>Windows.UI.Text.Core</strong>](https://msdn.microsoft.com/library/windows/apps/dn958238) 네임스페이스는 키보드 입력을 단일 서버에서 처리하는 클라이언트-서버 시스템을 제공합니다.</p>
<p>이 네임스페이스를 사용하여 사용자 지정 텍스트 입력된 컨트롤의 편집 버퍼를 처리할 수 있습니다. 텍스트 입력 서버는 앱과 서버 간에 비동기 통신 채널을 통해 텍스트 입력 컨트롤의 내용과 고유한 편집 버퍼의 내용을 항상 동기화 상태로 유지합니다.</p></td>
</tr>
<tr class="even">
<td align="left">벡터 아이콘</td>
<td align="left">[<strong>Glyphs</strong>](https://msdn.microsoft.com/library/windows/apps/br209921) 요소에는 컬러 글꼴을 지원하는 새 [<strong>IsColorFontEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn900468) 및 [<strong>ColorFontPalleteIndex</strong>](https://msdn.microsoft.com/library/windows/apps/dn900466) 속성이 있습니다. 이제 글꼴 파일을 사용하여 글꼴 기반 아이콘을 렌더링할 수 있습니다. <strong>ColorFontPalleteIndex</strong>를 색상표 전환에 사용하는 경우 단일 아이콘을 여러 가지 색 집합으로 렌더링하여 아이콘의 사용 및 사용 안 함 버전 등을 표시할 수 있습니다.</td>
</tr>
<tr class="odd">
<td align="left">입력기 창 이벤트</td>
<td align="left">사용자는 때때로 텍스트 입력란 바로 아래의 창에 표시되는 입력기를 통해 텍스트를 입력합니다(일반적으로 동아시아 언어). [<strong>TextBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209683) 및 [<strong>RichEditBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227548)의 [<strong>CandidateWindowBoundsChanged</strong>](https://msdn.microsoft.com/library/windows/apps/dn955144) 이벤트와 [<strong>DesiredCandidateWindowAlignment</strong>](https://msdn.microsoft.com/library/windows/apps/dn955145) 속성을 사용하여 IME 창에서 UI 작동을 개선할 수 있습니다.</td>
</tr>
<tr class="even">
<td align="left">텍스트 컴퍼지션 이벤트</td>
<td align="left">[<strong>TextBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209683) 및 [<strong>RichEditBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227548)에는 입력기를 사용하여 텍스트가 작성되면 앱에 알리는 [<strong>TextCompositionStarted</strong>](https://msdn.microsoft.com/library/windows/apps/dn891458), [<strong>TextCompositionEnded</strong>](https://msdn.microsoft.com/library/windows/apps/dn891457), [<strong>TextCompositionChanged</strong>](https://msdn.microsoft.com/library/windows/apps/dn891456) 등의 새 이벤트가 있습니다. 이러한 이벤트를 처리하여 IME 텍스트 컴퍼지션 프로세스로 앱 코드를 조정할 수 있습니다. 예를 들어 동아시아 언어에 대해 인라인 자동 완성 기능을 구현할 수 있습니다.</td>
</tr>
<tr class="odd">
<td align="left">양방향 텍스트의 향상된 처리</td>
<td align="left"><p>XAML 텍스트 컨트롤에는 양방향 텍스트 처리를 개선하여 다양한 입력 언어에서 더 나은 텍스트 맞춤 및 단락 방향을 제공하는 새 API가 있습니다.</p>
<p>[<strong>TextReadingOrder</strong>](https://msdn.microsoft.com/library/windows/apps/dn949133) 속성의 기본값이 <strong>DetectFromContent</strong>로 변경되어 읽기 순서 검색 지원이 기본적으로 사용됩니다. 또한 <strong>TextReadingOrder</strong> 속성이 [<strong>PasswordBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227519), [<strong>RichEditBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227548) 및 [<strong>TextBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209683)에 추가되었습니다.</p>
<p>텍스트 컨트롤의 [<strong>TextAlignment</strong>](https://msdn.microsoft.com/library/windows/apps/br209703) 속성을 새 <strong>DetectFromContent</strong> 값으로 설정하여 콘텐츠에서 맞춤이 자동 검색되도록 옵트인(opt in)할 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left">텍스트 렌더링</td>
<td align="left">Windows 10에서는 대체로 XAML 앱의 텍스트가 이제 Windows 8.1의 속도보다 거의 두 배 빠르게 렌더링됩니다. 대부분의 경우 앱은 변경 없이 이 향상을 활용할 수 있습니다. 빠른 렌더링 외에도, 이러한 향상을 통해 XAML 앱의 일반적인 메모리 소비가 5% 감소합니다.</td>
</tr>
</tbody>
</table>

 

## 응용 프로그램 모델


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Cortana</td>
<td align="left"><p>외부 응용 프로그램을 시작하고 해당 응용 프로그램 내에서 단일 작업을 실행하는 음성 명령으로 <strong>Cortana</strong>의 기본 기능을 확장하세요.</p>
<p>앱의 기본 기능을 통합하고 사용자가 앱을 직접 열지 않고도 대부분의 작업을 수행할 수 있는 중앙 진입점을 제공하면 <strong>Cortana</strong>가 앱과 사용자 간의 연락을 담당하는 역할을 할 수 있습니다. 많은 경우 이렇게 하면 사용자의 시간과 노력을 상당히 줄일 수 있습니다.</p>
<p>[앱을 Cortana 캔버스에 통합](https://msdn.microsoft.com/library/windows/apps/xaml/dn974230)하는 방법을 알아보세요. 아이디어가 필요한 경우 [유니버설 Windows 앱에 대한 디자인 기본 사항](https://dev.windows.com/design/design-basics)에서 <strong>Cortana</strong>와 관련된 디자인 권장 사항 및 UX 지침을 참조할 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left">파일 탐색기</td>
<td align="left">새 [<strong>Windows.System.Launcher.LaunchFolderAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn889616) 메서드를 사용하면 파일 탐색기를 시작하고 지정한 폴더의 내용을 표시할 수 있습니다.</td>
</tr>
<tr class="odd">
<td align="left">공유 저장소</td>
<td align="left">새 [<strong>Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager</strong>](https://msdn.microsoft.com/library/windows/apps/dn889985) 클래스 및 해당 메서드를 사용하면 URI 활성화를 통해 다른 앱을 실행할 때 공유 토큰을 전달하여 다른 앱과 파일을 공유할 수 있습니다. 대상 앱은 토큰을 등록하여 원본 앱에서 공유한 파일을 가져옵니다.</td>
</tr>
<tr class="even">
<td align="left">설정</td>
<td align="left"><p>[<strong>LaunchUriAsync</strong>](https://msdn.microsoft.com/library/windows/apps/hh701476) 메서드와 함께 ms-settings 프로토콜을 사용하여 기본 제공 설정 페이지를 표시합니다. 예를 들어 다음 코드는 Wi-Fi 설정 페이지를 표시합니다.</p>
<p><code>bool result = await Launcher.LaunchUriAsync(new Uri(&quot;ms-settings://network/wifi&quot;));</code></p>
<p>표시할 수 있는 설정 페이지 목록에 대해서는 [ms-settings 프로토콜을 사용하여 기본 제공 설정 페이지를 표시하는 방법](https://msdn.microsoft.com/library/windows/apps/jj207014.aspx)을 참조하세요.</p></td>
</tr>
<tr class="odd">
<td align="left">앱 간 통신</td>
<td align="left"><p>Windows 10의 새로운 [앱 간 통신](https://msdn.microsoft.com/library/windows/apps/xaml/dn997827) API를 통해 Windows 웹 응용 프로그램은 물론 Windows 응용 프로그램 간에 서로 시작하고 데이터 및 파일을 교환할 수 있습니다.</p>
<p>이러한 새 API를 사용하면 이전에는 여러 응용 프로그램을 사용해야 했던 복잡한 작업도 원활하게 처리할 수 있습니다. 예를 들어 앱에서 소셜 네트워킹 앱을 실행하여 연락처를 선택하거나 체크 아웃 응용 프로그램을 실행하여 지급 프로세스를 완료할 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left">앱 서비스</td>
<td align="left">앱 서비스는 앱이 Windows 10의 다른 앱에 서비스를 제공하는 한 가지 방법입니다. 앱 서비스는 백그라운드 작업 형태를 사용합니다. 포그라운드 앱은 다른 앱의 앱 서비스를 호출하여 백그라운드에서 작업을 수행할 수 있습니다. 앱 서비스 API에 대한 참조 정보는 [<strong>Windows.ApplicationModel.AppService</strong>](https://msdn.microsoft.com/library/windows/apps/dn921731)를 참조하세요.</td>
</tr>
<tr class="odd">
<td align="left">앱 패키지 매니페스트</td>
<td align="left"><p>Windows 10의 [패키지 매니페스트 스키마](https://msdn.microsoft.com/library/windows/apps/br211474) 참조에 대한 업데이트에는 추가, 제거 및 변경된 요소가 포함됩니다.</p>
<p>스키마의 모든 요소, 특성 및 유형에 대한 참조 정보는 [요소 계층 구조](https://msdn.microsoft.com/library/windows/apps/dn934819)를 참조하세요.</p></td>
</tr>
</tbody>
</table>

 

## 장치


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Microsoft Surface Hub</td>
<td align="left"><p>Microsoft Surface Hub는 Surface Hub 또는 연결된 디바이스에서 기본적으로 실행되는 유니버설 Windows 앱을 위한 강력한 팀 공동 작업 디바이스 및 큰 화면 플랫폼입니다.</p>
<p>큰 화면, 터치 및 수동 입력, 카메라 및 센서 같은 확장 온보드 하드웨어를 활용하는, 비즈니스를 위해 특별히 설계된 고유한 앱을 빌드하세요.</p>
<p>[유니버설 Windows 앱에 대한 디자인 기본 사항](https://dev.windows.com/design/design-basics)에서 Surface Hub와 관련된 디자인 권장 사항 및 UX 지침을 살펴보세요. 이러한 문서에서는 유니버설 Windows 앱에 대한 반응형 디자인 기술을 설명합니다.</p>
<p>공동 공유 앱 지원에 대한 자세한 내용은 [<strong>SharedModeSettings</strong>](https://msdn.microsoft.com/library/windows/apps/dn949019)를 참조하세요.</p>
<p>수동 입력 및 새 [<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535) 컨트롤의 다중 지점 수동 입력 지원에 대한 자세한 내용은 [<strong>Windows.UI.Input.Inking</strong>](https://msdn.microsoft.com/library/windows/apps/br208524) 및 [<strong>Windows.UI.Input.Inking.Core</strong>](https://msdn.microsoft.com/library/windows/apps/dn958452)를 참조하세요.</p>
<p>센서 입력 처리에 대해서는 [디바이스, 프린터 및 센서 통합](https://msdn.microsoft.com/library/windows/apps/br229563)을 참조하세요.</p></td>
</tr>
<tr class="even">
<td align="left">Location</td>
<td align="left"><p>Windows 10에서는 사용자 위치에 대한 액세스 권한을 요청하는 새 메서드인 [<strong>RequestAccessAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn859152)를 제공합니다.</p>
<p>사용자는 <strong>설정</strong> 앱의 <strong>위치 개인 정보 설정</strong>에서 자신의 위치 데이터 개인 정보를 설정합니다. 앱은 다음의 경우에만 사용자 위치에 액세스할 수 있습니다.</p>
<ul>
<li><strong>이 디바이스의 위치</strong>가 켜짐 상태인 경우(휴대폰용 Windows 10에는 해당되지 않음)</li>
<li>위치 서비스 설정 "<strong>위치</strong>"가 <strong>켜짐</strong> 상태인 경우</li>
<li><strong>사용자의 위치를 사용할 수 있는 앱 선택</strong>에서 앱이 <strong>켜짐</strong> 상태로 설정된 경우</li>
</ul>
<p>사용자 위치에 액세스하기 전에 [<strong>RequestAccessAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn859152)를 호출하는 것이 중요합니다. 이때 앱이 포그라운드에 있어야 하고 <strong>RequestAccessAsync</strong>가 UI 스레드에서 호출되어야 합니다. 사용자가 자신의 위치에 대한 권한을 앱에 부여하기 전에는 앱이 위치 데이터에 액세스할 수 없습니다.</p></td>
</tr>
<tr class="odd">
<td align="left">AllJoyn</td>
<td align="left"><p>[<strong>Windows.Devices.AllJoyn</strong>](https://msdn.microsoft.com/library/windows/apps/dn894971) Windows 런타임 네임스페이스는 Microsoft에서 구현한 AllJoyn 오픈 소스 소프트웨어 프레임워크 및 서비스를 제공합니다. 이러한 API를 사용하면 유니버설 Windows 디바이스 앱이 AllJoyn 기반 IoT(사물 인터넷) 시나리오에서 다른 디바이스에 참여할 수 있습니다. AllJoyn C API에 대한 자세한 내용을 보려면 [AllSeen Alliance](https://allseenalliance.org/)에서 설명서를 다운로드하세요.</p>
<p>이 릴리스에 포함된 [AllJoynCodeGen](https://msdn.microsoft.com/library/windows/apps/dn913809) 도구를 사용하여 디바이스 앱에서 AllJoyn 시나리오를 가능하게 하기 위해 사용할 수 있는 Windows 구성 요소를 생성할 수 있습니다.</p>
<div class="alert">
<strong>참고</strong> 이제 새 클래스의 소형 디바이스에 Windows 10 IoT Core를 사용할 수 있으므로 Windows 및 Visual Studio를 사용하여 IoT("사물 인터넷") 디바이스를 만들 수 있습니다. [WindowsOnDevices.com](http://www.windowsondevices.com/)에서 Windows IoT에 대해 자세히 알아보기
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left">모바일의 인쇄 API(XAML)</td>
<td align="left">모바일 디바이스를 비롯한 디바이스 패밀리에서 XAML 기반 UWP 앱을 통해 인쇄할 수 있는 통합된 단일 API 집합이 있습니다. 이제 [<strong>Windows.Graphics.Printing</strong>](https://msdn.microsoft.com/library/windows/apps/br226489) 및 [<strong>Windows.UI.Xaml.Printing</strong>](https://msdn.microsoft.com/library/windows/apps/br243325) 네임스페이스의 친숙한 인쇄 관련 API를 사용하여 모바일 앱에 인쇄 기능을 추가할 수 있습니다.</td>
</tr>
<tr class="odd">
<td align="left">배터리</td>
<td align="left"><p>[<strong>Windows.Devices.Power</strong>](https://msdn.microsoft.com/library/windows/apps/dn895017) 네임스페이스의 배터리 API를 사용하면 앱을 실행하는 디바이스에 연결된 모든 배터리에 대해 앱이 자세히 알아볼 수 있습니다.</p>
<p>[<strong>Battery</strong>](https://msdn.microsoft.com/library/windows/apps/dn895004) 개체를 만들어 개별 배터리 컨트롤러 또는 모든 배터리 컨트롤러의 집계를 나타냅니다(각각 [<strong>FromIdAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn895013) 또는 [<strong>AggregateBattery</strong>](https://msdn.microsoft.com/library/windows/apps/dn895011)로 만든 경우).</p>
<p>[<strong>GetReport</strong>](https://msdn.microsoft.com/library/windows/apps/dn895015) 메서드를 사용하여 해당 배터리의 충전, 용량 및 상태를 나타내는 [<strong>BatteryReport</strong>](https://msdn.microsoft.com/library/windows/apps/dn895005) 개체를 반환합니다.</p></td>
</tr>
<tr class="even">
<td align="left">MIDI 디바이스</td>
<td align="left"><p>새 [<strong>Windows.Devices.Midi</strong>](https://msdn.microsoft.com/library/windows/apps/dn895002) 네임스페이스를 사용하면 다음을 만들 수 있습니다.</p>
<ul>
<li>외부 MIDI 디바이스와 통신할 수 있는 앱</li>
<li>Microsoft GS MIDI 소프트웨어 신시사이저와 직접 통신하는 앱 및 외부 디바이스</li>
<li>여러 클라이언트가 동시에 단일 MIDI 포트에 액세스하는 시나리오</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">사용자 지정 센서 지원</td>
<td align="left">[<strong>Windows.Devices.Sensors.Custom</strong>](https://msdn.microsoft.com/library/windows/apps/dn895032) 네임스페이스를 사용하면 하드웨어 개발자가 CO2 센서와 같은 새로운 사용자 지정 센서 유형을 정의할 수 있습니다.</td>
</tr>
<tr class="even">
<td align="left">HCE(호스트 기반 카드 에뮬레이션)</td>
<td align="left"><p>호스트 카드 에뮬레이션을 사용하면 OS에 호스트된 NFC 카드 에뮬레이션 서비스를 구현하고 NFC 라디오를 통해 외부 판독기 터미널과 계속 통신할 수 있습니다.</p>
<p>NFC를 통해 스마트 카드를 에뮬레이트하는 백그라운드 작업을 구현합니다. 백그라운드 작업을 트리거하려면 [<strong>SmartCardTrigger</strong>](https://msdn.microsoft.com/library/windows/apps/dn624853) 클래스를 사용합니다.</p>
<p>[<strong>SmartCardTriggerType</strong>](https://msdn.microsoft.com/library/windows/apps/dn608017) 열거형의 <strong>EmulatorHostApplicationActivated</strong> 값을 사용하여 HCE 이벤트가 발생한 것을 앱에 알릴 수 있습니다.</p></td>
</tr>
</tbody>
</table>

 

## 그래픽


|                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DirectX              | Windows 10의 DirectX 12에서는 DirectX의 핵심에 해당하는 3D 그래픽 API인 Microsoft Direct3D의 새로운 버전을 제공합니다. [Direct3D 12 그래픽](https://msdn.microsoft.com/library/windows/desktop/dn903821)은 콘솔과 유사한 저급 API의 성능과 효율성을 가능하게 합니다. Direct3D 12는 이전보다 더 빠르고 효율적이며 풍부한 장면, 더 많은 개체, 더 복잡한 효과 및 최신 GPU 하드웨어의 보다 효율적인 활용을 지원합니다.                                                                                                                                                                                                                                                                                     |
| SoftwareBitmapSource | 유니버설 Windows 앱에서 새 [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854) 형식을 XAML 이미지 원본으로 사용할 수 있습니다. 이렇게 하면 인코드되지 않은 이미지를 XAML 프레임워크에 전달하여 XAML 프레임워크에 의한 이미지 디코딩을 무시하고 화면에 즉시 표시할 수 있습니다. 카메라에서 직접 짧은 지연 사진 렌더링, 사용자 지정 이미지 디코더 사용, DirectX 화면에서 프레임 캡처 또는 메모리 내 이미지를 처음부터 만들고 짧은 대기 시간 및 낮은 메모리 오버헤드로 XAML에서 직접 모두 렌더링 등 훨씬 더 빠르게 이미지를 렌더링할 수 있습니다.                                                                                                     |
| 원근 카메라   | 유니버설 Windows 앱에서는 XAML의 새 Transform3D API를 사용하여 XAML 트리(또는 장면)에 원근 변형을 적용할 수 있습니다. 이렇게 하면 단일 장면 수준의 변형(또는 카메라)에 따라 모든 XAML 자식 요소가 변형됩니다. 이전에는 이 작업을 위해 MatrixTransform 및 복잡한 수학을 사용했지만 Transform3D를 통해 이 효과가 훨씬 간소화되었으며 효과에 애니메이션 효과를 줄 수도 있습니다. 자세한 내용은 [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/dn906919) 속성, [**Transform3D**](https://msdn.microsoft.com/library/windows/apps/dn914748), [**CompositeTransform3D**](https://msdn.microsoft.com/library/windows/apps/dn914714) 및 [**PerspectiveTransform3D**](https://msdn.microsoft.com/library/windows/apps/dn914740)를 참조하세요. |

 

## 미디어


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">HTTP 라이브 스트리밍</td>
<td align="left">새 [<strong>AdaptiveMediaSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn946912) 클래스를 사용하여 적응형 동영상 스트리밍 기능을 앱에 추가할 수 있습니다. 개체에서 스트리밍 매니페스트 파일을 가리켜 개체를 초기화합니다. 지원되는 매니페스트 형식에는 HLS(HTTP 라이브 스트리밍) 및 DASH(Dynamic Adaptive Streaming over HTTP)가 포함됩니다. 개체가 XAML 미디어 요소에 바인딩되면 적응형 재생이 시작됩니다. 사용 가능, 최소 및 최대 비트 전송률과 같은 스트림의 속성을 쿼리하고 적절한 경우 설정할 수 있습니다.</td>
</tr>
<tr class="even">
<td align="left">MFT(Media Foundation Transforms)에 대한 미디어 파운데이션 XVP(Transcode Video Processor) 지원</td>
<td align="left"><p>MFT(Media Foundation Transforms)를 사용하는 Windows 앱은 이제 <strong>미디어 파운데이션 XVP(Transcode Video Processor)</strong>를 사용하여 원시 비디오 데이터를 변환하고 크기를 조정하고 변형할 수 있습니다.</p>
<p>새 [MF_XVP_CALLER_ALLOCATES_OUTPUT](https://msdn.microsoft.com/library/windows/desktop/dn803919) 특성을 사용하면 Microsoft DXVA(DirectX 비디오 가속) 모드에서도 호출자 할당 텍스처로 출력할 수 있습니다.</p>
<p>새 [<strong>IMFVideoProcessorControl2</strong>](https://msdn.microsoft.com/library/windows/desktop/dn800741) 인터페이스를 통해 앱에서 하드웨어 효과를 사용하고, 지원되는 하드웨어 효과를 쿼리하고, 비디오 프로세서가 수행하는 회전 작업을 재정의할 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left">코드 변환</td>
<td align="left">새 [<strong>MediaProcessingTrigger</strong>](https://msdn.microsoft.com/library/windows/apps/dn806005) API를 통해 앱이 백그라운드 작업에서 미디어 코드 변환을 수행할 수 있으므로 포그라운드 앱이 종료된 경우에도 코드 변환 작업을 계속할 수 있습니다.</td>
</tr>
<tr class="even">
<td align="left">MediaElement 미디어 오류 이벤트</td>
<td align="left">유니버설 Windows 앱에서 [<strong>MediaElement</strong>] (https://msdn.microsoft.com/library/windows/apps/br242926)는 미디어 콘텐츠에 유효한 스트림이 하나 이상 포함되어 있기만 하면 스트림 중 하나를 디코드하는 동안 오류가 발생하더라도 여러 스트림이 포함된 콘텐츠를 재생합니다. 예를 들어 오디오 및 동영상 스트림이 포함된 콘텐츠의 동영상 스트림이 실패할 경우 <strong>MediaElement</strong>는 오디오 스트림을 계속 재생합니다. [<strong>PartialMediaFailureDetected</strong>](https://msdn.microsoft.com/library/windows/apps/dn889635) 이벤트는 스트림 내의 스트림 중 하나를 디코드할 수 없다고 알려줍니다. 또한 UI에 해당 정보를 반영할 수 있도록 어떤 유형의 스트림이 실패했는지도 알려줍니다. 미디어 스트림 내의 모든 스트림이 실패할 경우 [<strong>MediaFailed</strong>](https://msdn.microsoft.com/library/windows/apps/br227393) 이벤트가 발생합니다.</td>
</tr>
<tr class="odd">
<td align="left">MediaElement를 이용한 적응형 동영상 스트리밍 지원</td>
<td align="left">[<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926)에는 적응형 동영상 스트리밍을 지원하는 새 [<strong>SetPlaybackSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn899085) 메서드가 있습니다. 이 메서드를 사용하여 미디어 원본을 [<strong>AdaptiveMediaSource</strong>] (https://msdn.microsoft.com/library/windows/apps/dn946912)로 설정할 수 있습니다.</td>
</tr>
<tr class="even">
<td align="left">MediaElement 및 이미지로 캐스팅</td>
<td align="left">[<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926) 및 [<strong>Image</strong>](https://msdn.microsoft.com/library/windows/apps/br242752) 콘솔에는 새 [<strong>GetAsCastingSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn920012) 메서드가 있습니다. 이 메서드를 사용하여 모든 미디어 또는 이미지 요소의 콘텐츠를 광범위한 원격 디바이스(예: Miracast, Bluetooth 및 DLNA)에 프로그래밍 방식으로 보낼 수 있습니다.<strong>MediaElement</strong>의 [<strong>AreTransportControlsEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn298977)를 true로 설정하면 이 기능이 자동으로 사용됩니다.</td>
</tr>
<tr class="odd">
<td align="left">데스크톱 앱용 미디어 전송 컨트롤</td>
<td align="left">[<strong>ISystemMediaTransportControls</strong>](https://msdn.microsoft.com/library/windows/desktop/dn892299) 인터페이스 및 관련 API를 사용하면 데스크톱 앱이 기본 제공 시스템 미디어 전송 컨트롤을 조작할 수 있습니다. 여기에는 전송 컨트롤 단추를 사용하여 사용자 조작에 응답, 현재 재생 중인 미디어 콘텐츠에 대한 메타데이터를 표시하도록 전송 컨트롤 표시 업데이트 등이 포함됩니다.</td>
</tr>
<tr class="even">
<td align="left">임의 액세스 JPEG 인코딩 및 디코딩</td>
<td align="left">새 WIC 메서드 [<strong>IWICJpegFrameEncode</strong>](https://msdn.microsoft.com/library/windows/desktop/dn903864) 및 [<strong>IWICJpegFrameDecode</strong>](https://msdn.microsoft.com/library/windows/desktop/dn903834)를 통해 JPEG 이미지를 인코드 및 디코드할 수 있습니다. 이제 메모리 사용량은 증가하지만 큰 이미지에 대한 효율적인 임의 액세스를 제공하는 이미지 데이터 인덱싱을 사용하도록 설정할 수도 있습니다.</td>
</tr>
<tr class="odd">
<td align="left">미디어 컴퍼지션의 오버레이</td>
<td align="left">새 [<strong>MediaOverlay</strong>](https://msdn.microsoft.com/library/windows/apps/dn764793) 및 [<strong>MediaOverlayLayer</strong>](https://msdn.microsoft.com/library/windows/apps/dn764795) API를 통해 정적 또는 동적 미디어 콘텐츠의 여러 계층을 쉽게 미디어 컴퍼지션에 추가할 수 있습니다. 각 계층의 불투명도, 위치 및 타이밍을 조정할 수 있으며 입력 계층에 대한 사용자 지정 작성자를 구현할 수도 있습니다.</td>
</tr>
<tr class="even">
<td align="left">새로운 효과 프레임워크</td>
<td align="left">[<strong>Windows.Media.Effects</strong>](https://msdn.microsoft.com/library/windows/apps/dn278802) 네임스페이스는 오디오 및 동영상 스트림에 효과를 추가하는 단순하고 직관적인 프레임워크를 제공합니다. 이 프레임워크에는 사용자 지정 오디오 및 동영상 효과를 만들어 미디어 파이프라인에 삽입하도록 구현할 수 있는 기본 인터페이스가 포함되어 있습니다.</td>
</tr>
</tbody>
</table>

 

## 네트워킹


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">소켓</td>
<td align="left"><p>소켓 업데이트에는 다음이 포함됩니다.</p>
<ul>
<li><strong>소켓 브로커.</strong> 소켓 브로커는 앱 수명 주기의 모든 상태에서 앱을 대신하여 소켓 연결을 설정하고 닫을 수 있습니다. 이를 통해 제공되는 앱과 서비스를 보다 잘 검색할 수 있습니다. 예를 들어 소켓 브로커를 통해 Win32 서비스는 실행되지 않는 경우에도 들어오는 소켓 연결을 계속 수락할 수 있습니다.</li>
<li><strong>처리량 향상.</strong> 소켓 처리량은 [<strong>Windows.Networking.Sockets</strong>](https://msdn.microsoft.com/library/windows/apps/br226960) 네임스페이스를 사용하는 앱에 맞게 최적화되었습니다.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">백그라운드 전송 사후 처리 작업</td>
<td align="left">[<strong>Windows.Networking.BackgroundTransfer</strong>](https://msdn.microsoft.com/library/windows/apps/br207242) 네임스페이스의 새 API를 사용하여 사후 처리 작업 그룹을 등록할 수 있습니다. 따라서 앱은 포그라운드 상태가 아니더라도 사용자가 다음번에 앱을 다시 시작할 때까지 기다릴 필요 없이 백그라운드 전송의 성공 또는 실패에 대해 즉시 반응할 수 있습니다.</td>
</tr>
<tr class="odd">
<td align="left">Bluetooth 광고 지원</td>
<td align="left">[<strong>Windows.Devices.Bluetooth.Advertisement</strong>](https://msdn.microsoft.com/library/windows/apps/dn894325) 네임스페이스를 사용하면 앱은 Bluetooth LE 연결을 통해 광고를 보내고 받고 필터링할 수 있습니다.</td>
</tr>
<tr class="even">
<td align="left">Wi-Fi Direct API 업데이트</td>
<td align="left"><p>디바이스 브로커가 앱을 나가지 않고도 디바이스와 연결할 수 있도록 업데이트되었습니다. [<strong>Windows.Devices.WiFiDirect</strong>](https://msdn.microsoft.com/library/windows/apps/dn297687) 네임스페이스에 추가하면 디바이스가 다른 디바이스에서 검색될 수 있으며 들어오는 연결 알림을 수신 대기할 수 있습니다.</p>
<div class="alert">
<strong>참고</strong> 이 릴리스에서는 향상된 Wi-Fi Direct 기능이 UX에 제공되지 않으며 누름 단추 페어링만 지원합니다. 또한 이 릴리스에서는 하나만 활성 연결만 지원합니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left">향상된 JSON 지원</td>
<td align="left">[<strong>Windows.Data.Json</strong>](https://msdn.microsoft.com/library/windows/apps/br240639) 네임스페이스는 이제 디버그 세션 중에 JSON 개체를 변환할 때 기존 표준 정의 및 개발자 환경을 보다 잘 지원합니다.</td>
</tr>
</tbody>
</table>

 

## 보안


|                             |                                                                      |
|-----------------------------|----------------------------------------------------------------------|
| ECC 암호화              | [
            **Windows.Security.Cryptography**](https://msdn.microsoft.com/library/windows/apps/br241404) 네임스페이스의 새 API는 유한체 위의 타원형 곡선을 기반으로 하는 공개 키 암호화 구현을 나타내는 ECC(타원 곡선 암호화)를 지원합니다. ECC는 RSA보다 수학적으로 더 복잡하고, 더 작은 키 크기를 제공하며, 메모리를 적게 사용하고, 성능을 향상합니다. 또한 Microsoft 서비스 및 고객에게 RSA 키 및 NIST 승인 곡선 매개 변수를 대신할 수 있는 기능을 제공합니다. |
| Microsoft Passport          | Microsoft Passport는 암호를 비대칭 암호화 및 제스처로 바꾸는 대체 인증 방법입니다. [
            **KeyCredentialManager**](https://msdn.microsoft.com/library/windows/apps/dn973043)와 같은 [**Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089) 네임스페이스의 클래스를 통해 개발자는 복잡한 암호화 또는 생체 인식을 사용하지 않고 Microsoft Passport를 사용하여 쉽게 응용 프로그램을 만들 수 있습니다.  |
| Microsoft Passport for Work | Microsoft Passport for Work는 암호, 스마트 카드 및 가상 스마트 카드를 사용하지 않는 Azure Active Directory 계정으로 Windows에 로그인하는 대체 방법입니다. 이 정책 설정을 사용할지 또는 사용하지 않을지를 선택할 수 있습니다. |
| 토큰 브로커                | 토큰 브로커는 앱이 온라인 ID 제공자(예: Facebook)에 쉽게 연결할 수 있도록 하는 새로운 인증 프레임워크입니다. 계정 사용자 이름 및 암호 관리, 간소화된 UI 등의 기능은 사용자에게 획기적으로 개선된 인증 환경을 제공합니다. |

 

## 시스템 서비스


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">전원</td>
<td align="left"><p>이제 배터리 절약 모드를 시작하거나 이 모드가 해제될 때 Windows 데스크톱 응용 프로그램에서 알림을 받을 수 있습니다. 응용 프로그램에서 전원 상태 변화에 대응하면서 배터리 사용 시간을 연장할 수 있습니다.</p>
<p>[<strong>GUID_POWER_SAVING_STATUS</strong>](https://msdn.microsoft.com/library/windows/desktop/hh448380): 이 새 GUID와 [<strong>PowerSettingRegisterNotification</strong>](https://msdn.microsoft.com/library/windows/desktop/hh769082) 함수를 함께 사용하면 배터리 절약 모드를 시작하거나 이 모드가 해제될 때 알림을 받을 수 있습니다.</p>
<p>[<strong>SYSTEM_POWER_STATUS</strong>](https://msdn.microsoft.com/library/windows/desktop/aa373232): 이 구조체는 배터리 절약 모드를 지원하도록 업데이트되었습니다. 이제 네 번째 멤버인 <strong>SystemStatusFlag</strong>(이전 이름 Reserved1)는 배터리 절약 모드가 사용되는지 여부를 나타냅니다. [<strong>GetSystemPowerStatus</strong>](https://msdn.microsoft.com/library/windows/desktop/aa372693) 함수를 사용하여 이 구조체에 대한 포인터를 가져올 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left">버전</td>
<td align="left"><p>[버전 도우미 함수](https://msdn.microsoft.com/library/windows/desktop/dn424972)를 사용하여 운영 체제의 버전을 확인할 수 있습니다. Windows 10의 경우 이러한 도우미 함수에는 새로운 함수 [<strong>IsWindows10OrGreater</strong>](https://msdn.microsoft.com/library/windows/desktop/dn905474)가 포함됩니다. 시스템 버전을 확인하려는 경우 더 이상 사용되지 않는 [<strong>GetVersionEx</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724451) 및 [<strong>GetVersion</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724439) 함수가 아닌 도우미 함수를 사용해야 합니다. 시스템 버전을 가져오는 방법에 대한 자세한 내용은 [시스템 버전 가져오기](https://msdn.microsoft.com/library/windows/desktop/ms724429)를 참조하세요.</p>
<p>더 이상 사용되지 않는 [<strong>GetVersionEx</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724451) 또는 [<strong>GetVersion</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724439) 함수를 사용하여 [<strong>OSVERSIONINFOEX</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724833) 또는 [<strong>OSVERSIONINFO</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724834) 구조체의 버전 정보를 가져오는 경우 이러한 구조체에 포함된 버전 번호가 Windows 8.1 및 Windows Server 2012 R2를 나타내는 6.3에서 Windows 10을 나타내는 10.0으로 증가합니다. 운영 체제 버전 번호에 대한 자세한 내용은 [운영 체제 버전](https://msdn.microsoft.com/library/windows/desktop/ms724832)을 참조하세요.</p>
<p>또한 [<strong>GetVersionEx</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724451) 또는 [<strong>GetVersion</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724439) 함수를 사용하여 이러한 버전에 대한 올바른 버전 정보를 가져오려면 응용 프로그램에서 Windows 8.1 또는 Windows 10을 구체적으로 대상으로 지정해야 합니다. 이러한 Windows 버전용 응용 프로그램을 대상으로 지정하는 방법에 대한 자세한 내용은 [Windows용 응용 프로그램을 대상으로 지정](https://msdn.microsoft.com/library/windows/desktop/dn481241)을 참조하세요.</p></td>
</tr>
<tr class="odd">
<td align="left">사용자 정보</td>
<td align="left">[<strong>Windows.System</strong>](https://msdn.microsoft.com/library/windows/apps/br241814) 네임스페이스의 새로운 API를 통해 사용자 이름 및 계정 사진과 같은 사용자 관련 정보에 쉽게 액세스할 수 있습니다. 또한 로그인 및 로그아웃과 같은 사용자 이벤트에 응답하는 기능도 제공합니다.</td>
</tr>
<tr class="even">
<td align="left">메모리 관리 및 프로파일링</td>
<td align="left">[<strong>Windows.System</strong>](https://msdn.microsoft.com/library/windows/apps/br241814)의 메모리 프로파일링 API에 대한 지원이 모든 플랫폼으로 확장되었으며 전반적인 기능이 새로운 클래스 및 함수를 통해 개선되었습니다.</td>
</tr>
</tbody>
</table>

 

## 저장소


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Windows Phone에 사용할 수 있는 파일 검색 API</td>
<td align="left"><p>앱 게시자는 앱 매니페스트에 확장을 추가하여 게시하는 다른 앱과 저장소 폴더를 공유하도록 앱을 등록할 수 있습니다. 그런 다음 [<strong>Windows.Storage.ApplicationData.GetPublisherCacheFolder</strong>](https://msdn.microsoft.com/library/windows/apps/dn889607) 메서드를 호출하여 공유 저장소 위치를 가져옵니다.</p>
<p>Windows 런타임 앱의 강력한 보안 모델은 일반적으로 앱 간 데이터 공유를 방지합니다. 그러나 동일한 게시자의 앱에서는 사용자별로 파일 및 설정을 공유하는 것이 유용할 수 있습니다.</p></td>
</tr>
</tbody>
</table>

 

## 도구


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Visual Studio의 실시간 시각적 트리</td>
<td align="left">Visual Studio에 새로운 실시간 시각적 트리 기능이 추가되었습니다. 디버그 중에 이 기능을 사용하여 앱의 시각적 트리 상태를 빠르게 파악하고 요소 속성이 어떻게 설정되었는지 검색할 수 있습니다. 또한 앱이 실행되는 동안 속성 값을 변경할 수 있어 다시 실행하지 않고도 수정과 실험이 가능합니다.</td>
</tr>
<tr class="even">
<td align="left">추적 로그</td>
<td align="left"><p>[TraceLogging](https://msdn.microsoft.com/library/windows/desktop/dn904636)은 사용자 모드 앱 및 커널 모드 드라이버에 대한 새 이벤트 추적 API로, [ETW(Windows용 이벤트 추적)](https://msdn.microsoft.com/library/windows/desktop/bb968803)를 기반으로 빌드됩니다. 이 API는 별도의 계측 매니페스트 XML 파일 없이도 코드를 계측화하고 이벤트에 구조화된 데이터를 포함하는 간편한 방법을 제공합니다.</p>
<p>WinRT,.NET 및 C/C++ TraceLogging API는 각기 다른 개발자 대상 그룹에 사용될 수 있습니다.</p></td>
</tr>
</tbody>
</table>

 

## 사용자 환경


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">음성 인식</td>
<td align="left">이제 유니버설 Windows 플랫폼에서 긴 형식 받아쓰기 시나리오에 대한 연속 음성 인식이 지원됩니다. 음성 조작 설명서에서 연속 받아쓰기를 사용하도록 설정하는 방법을 참조하세요.</td>
</tr>
<tr class="even">
<td align="left">다른 응용 프로그램 플랫폼 간 끌어서 놓기 기능</td>
<td align="left"><p>새 [<strong>Windows.ApplicationModel.DataTransfer.DragDrop</strong>](https://msdn.microsoft.com/library/windows/apps/dn894216) 네임스페이스는 유니버설 Windows 앱에 끌어서 놓기 기능을 제공합니다. 이전과 같이 폴더의 문서를 Outlook 메일 메시지로 끌어와 첨부하는 데스크톱 프로그램의 일반적인 끌어서 놓기 기능이 유니버설 Windows 앱에서는 가능하지 않습니다. 이러한 새 API를 사용하여 앱 사용자는 다른 유니버설 Windows 앱과 데스크톱 간에 데이터를 쉽게 이동할 수 있습니다.</p>
<p>앱 간 끌어서 놓기 기능을 지원하기 위해 XAML에 다음과 같은 새 API가 추가되었습니다.</p>
<ul>
<li>[<strong>ListViewBase.DragItemsCompleted</strong>](https://msdn.microsoft.com/library/windows/apps/dn831959)</li>
<li>[<strong>UIElement</strong>](https://msdn.microsoft.com/library/windows/apps/br208911): [<strong>CanDrag</strong>](https://msdn.microsoft.com/library/windows/apps/dn903558), [<strong>DragStarting</strong>](https://msdn.microsoft.com/library/windows/apps/dn903560), [<strong>StartDragAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn903562), [<strong>DropCompleted</strong>](https://msdn.microsoft.com/library/windows/apps/dn903561)</li>
<li>[<strong>DragOperationDeferral</strong>](https://msdn.microsoft.com/library/windows/apps/dn831917), [<strong>DragUI</strong>](https://msdn.microsoft.com/library/windows/apps/dn985714), [<strong>DragUIOverride</strong>](https://msdn.microsoft.com/library/windows/apps/dn985715)</li>
<li>[<strong>DragEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/br242372): [<strong>AcceptedOperation</strong>](https://msdn.microsoft.com/library/windows/apps/dn831912), [<strong>DataView</strong>](https://msdn.microsoft.com/library/windows/apps/dn831913), [<strong>DragUIOverride</strong>](https://msdn.microsoft.com/library/windows/apps/dn985710), [<strong>GetDeferral</strong>](https://msdn.microsoft.com/library/windows/apps/dn831914), [<strong>Modifiers</strong>](https://msdn.microsoft.com/library/windows/apps/dn831915)</li>
<li>[<strong>DragItemsCompletedEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/dn831953), [<strong>DropCompletedEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/dn903549), [<strong>DragStartingEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/dn903540)</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">사용자 지정 창 제목 표시줄</td>
<td align="left">데스크톱 디바이스 패밀리용 UWP 앱의 경우 이제 [<strong>ApplicationViewTitleBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn906115) 클래스와 [<strong>ApplicationView.TitleBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn906131) 속성 및 [<strong>Window.SetTitleBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn965560) 메서드를 사용하여 기본 Windows 제목 표시줄 콘텐츠를 사용자 지정 XAML 콘텐츠로 바꿀 수 있습니다. XAML이 &quot;시스템 크롬&quot;으로 취급되므로 Windows는 앱이 아닌 입력 이벤트를 처리합니다. 즉, 사용자가 사용자 지정 제목 표시줄 콘텐츠를 클릭하는 경우에도 창을 끌어 놓고 크기를 조정할 수 있습니다.</td>
</tr>
</tbody>
</table>

 

## 웹


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Internet Explorer</td>
<td align="left"><p>Internet Explorer의 새로운 에지 모드: 다른 최신 브라우저 및 최신 웹 콘텐츠와의 상호 운용성을 최대화하기 위해 디자인된 새로운 &quot;활성&quot; 문서 모드입니다. 이 실험 모드는 임의로 선택된 Windows 10 사용자 집합에 점진적으로 공개하는 중입니다. 새 IE <strong>about:flags</strong> 메커니즘을 통해 에지 모드를 사용하거나 사용하지 않도록 수동으로 설정할 수 있습니다. 자세한 내용은 다음의 정보를 참조하세요.</p>
<ul>
<li>[에지 작업 – 웹 작동을 도와주는 다음 단계](http://blogs.msdn.com/b/ie/archive/2014/11/11/living-on-the-edge-our-next-step-in-interoperability.aspx)</li>
<li>[Windows 10용 Internet Explorer 개발자 가이드](https://dev.windows.com/microsoft-edge/)</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">WebView 에지 모드 검색</td>
<td align="left">[<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702) 컨트롤은 새로운 에지 브라우저와 동일한 렌더링 엔진을 사용하여 가장 정확한 표준 규격 모드의 HTML 렌더링을 제공합니다.</td>
</tr>
<tr class="odd">
<td align="left">오프-스레드 WebView</td>
<td align="left">[<strong>WebView.ExecutionMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn932034)를 지정할 경우 처리를 활성화하고 별도 백그라운드 스레드에 웹 콘텐츠를 표시하여 특정 시나리오에서 성능을 향상할 수 있습니다.</td>
</tr>
<tr class="even">
<td align="left">WebView.UnsupportedUriSchemeIdentified 이벤트</td>
<td align="left"><p>새 [<strong>WebView.UnsupportedUriSchemeIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn974400) 이벤트를 사용하면 앱에서 지원되지 않는 URI 체계를 처리할 방식을 결정할 수 있습니다. 이 이벤트를 처리하면 앱에서 지원되지 않는 URI 체계의 사용자 지정 처리를 제공할 수 있습니다.</p>
<p>HTML WebView 컨트롤에 대한 자세한 내용은 [<strong>MSWebViewUnsupportedUriSchemeIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn803906.aspx) 이벤트를 참조하세요.</p></td>
</tr>
<tr class="odd">
<td align="left">WebView.NewWindowRequested 이벤트</td>
<td align="left"><p>새 [<strong>WebView.NewWindowRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn974397) 이벤트를 사용하면 WebView의 스크립트에서 새 브라우저 창을 요청할 때 응답할 수 있습니다.</p>
<p>HTML WebView 컨트롤에 대한 자세한 내용은 [<strong>MSWebViewNewWindowRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn806030) 이벤트를 참조하세요.</p></td>
</tr>
<tr class="even">
<td align="left">WebView.PermissionRequested 이벤트</td>
<td align="left"><p>새 [<strong>WebView.PermissionRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn974398) 이벤트를 사용하면 WebView 콘텐츠가 지리적 위치와 같이 사용자의 특별 허가가 필요한 새로운 HTML5 API를 활용할 수 있습니다.</p>
<p>HTML WebView 컨트롤에 대한 자세한 내용은 [<strong>MSWebViewPermissionRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn806030.aspx) 이벤트를 참조하세요.</p></td>
</tr>
<tr class="odd">
<td align="left">WebView.UnviewableContentIdentified 이벤트</td>
<td align="left"><p>새 [<strong>WebView.UnviewableContentIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn299351) 이벤트를 사용하면 [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702)에서 PDF 파일 또는 Office 문서와 같은 웹 이외의 콘텐츠를 탐색할 때 응답할 수 있습니다.</p>
<p>HTML WebView 컨트롤에 대한 자세한 내용은 [<strong>MSWebViewUnviewableContentIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn609716) 이벤트를 참조하세요.</p></td>
</tr>
<tr class="even">
<td align="left">WebView.AddWebAllowedObject 메서드</td>
<td align="left"><p>새 [<strong>WebView.AddWebAllowedObject</strong>](https://msdn.microsoft.com/library/windows/apps/dn903993) 메서드를 호출하여 WinRT 개체를 XAML [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702)에 삽입한 다음 해당 <strong>WebView</strong>에 호스트된 신뢰할 수 있는 JavaScript에서 함수를 호출할 수 있습니다. 예를 들어 웹 콘텐츠는 부모 앱에 [<strong>ToastNotificationManager</strong>](https://msdn.microsoft.com/library/windows/apps/br208642) WinRT API를 호출하도록 요청하여 시스템 알림을 표시할 수 있습니다.</p>
<p>HTML [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702) 컨트롤에 대한 자세한 내용은 [<strong>addWebAllowedObject</strong>](https://msdn.microsoft.com/library/windows/apps/dn926632) 메서드를 참조하세요.</p></td>
</tr>
<tr class="odd">
<td align="left">WebView.ClearTemporaryWebDataAync 메서드</td>
<td align="left">사용자가 XAML [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702) 내의 웹 콘텐츠를 조작하는 경우 <strong>WebView</strong> 컨트롤은 해당 사용자의 세션을 기준으로 데이터를 캐싱합니다. 새 [<strong>ClearTemporaryWebDataAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn974394) 메서드를 호출하여 이 캐시를 지울 수 있습니다. 예를 들어 한 사용자가 앱에서 로그아웃할 때 다른 사용자가 이전 세션의 데이터에 액세스할 수 없도록 캐시를 지울 수 있습니다.</td>
</tr>
</tbody>
</table>



<!--HONumber=May16_HO2-->


