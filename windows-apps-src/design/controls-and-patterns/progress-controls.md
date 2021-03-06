---
description: 진행률 컨트롤은 긴 작업을 진행 중인 사용자에게 피드백을 제공합니다.
title: 진행률 컨트롤에 대한 지침
ms.assetid: FD53B716-C43D-408D-8B07-522BC1F3DF9D
label: Progress controls
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: jeffarn
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 24ba67dfa51c039055cc5bc4cb31d4aff4de6765
ms.sourcegitcommit: b99fe39126fbb457c3690312641f57d22ba7c8b6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/04/2020
ms.locfileid: "96603896"
---
# <a name="progress-controls"></a>진행률 컨트롤

진행률 컨트롤은 긴 작업을 진행 중인 사용자에게 피드백을 제공합니다. 이는 진행률 표시기가 표시될 때 사용자가 앱을 조작할 수 없다는 의미이며 사용되는 표시기에 따라 대기 시간을 예측할 수도 있다는 의미입니다.

**Windows UI 라이브러리 가져오기**

:::row:::
   :::column:::
      ![WinUI 로고](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      **ProgressBar** 컨트롤은 Windows 앱용 새 컨트롤과 UI 기능을 포함하는 NuGet 패키지인 Windows UI 라이브러리의 일부로 포함되었습니다. 설치 지침을 비롯한 자세한 내용은 [Windows UI 라이브러리](/uwp/toolkits/winui/)를 참조하세요.
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **Windows UI 라이브러리 API:** [ProgressBar 클래스](/uwp/api/Microsoft.UI.Xaml.Controls.ProgressBar), [IsIndeterminate 속성](/uwp/api/Microsoft.ui.xaml.controls.progressbar.isindeterminate), [ProgressRing 클래스](/uwp/api/Microsoft.UI.Xaml.Controls.ProgressRing), [IsActive 속성](/uwp/api/Microsoft.ui.xaml.controls.progressring.isactive)
>
> **플랫폼 API:** [ProgressBar 클래스](/uwp/api/Windows.UI.Xaml.Controls.ProgressBar), [IsIndeterminate 속성](/uwp/api/windows.ui.xaml.controls.progressbar.isindeterminate), [ProgressRing 클래스](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing), [IsActive 속성](/uwp/api/windows.ui.xaml.controls.progressring.isactive)

> [!NOTE]
> ProgressBar 및 ProgressRing 컨트롤에는 두 가지 버전이 있습니다. 하나는 Windows.UI.Xaml 네임스페이스로 표현되는 플랫폼에 있고, 다른 하나는 Microsoft.UI.Xaml 네임스페이스로 표현되는 Windows UI 라이브러리에 있습니다. ProgressRing 및 ProgressBar용 API는 동일하지만 두 버전 간 컨트롤의 모양은 다릅니다. 이 문서에서는 최신 Windows UI 라이브러리 버전의 이미지가 나옵니다.
이 문서 전체에서 XAML의 **muxc** 별칭을 사용하여 프로젝트에 포함된 Windows UI 라이브러리 API를 나타냅니다. 다음을 [Page](/uwp/api/windows.ui.xaml.controls.page) 요소에 추가했습니다.

```xaml
xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
```

코드 숨김에서는 C#의 **muxc** 별칭을 사용하여 프로젝트에 포함된 Windows UI 라이브러리 API를 나타냅니다. 다음 **using** 문을 파일 맨 위에 추가했습니다.

```csharp
using muxc = Microsoft.UI.Xaml.Controls;
```

```vb
Imports muxc = Microsoft.UI.Xaml.Controls
```

## <a name="types-of-progress"></a>진행률 유형

사용자에게 작업 진행을 표시하기 위해 ProgressBar 또는 ProgressRing 두 가지 컨트롤이 있습니다. ProgressBar와 ProgressRing에는 사용자가 애플리케이션과 상호 작용할 수 있는지 여부를 통신하는 두 가지 상태가 있습니다. 

-   ProgressBar 및 ProgressRing의 *확정* 상태는 작업 완료율을 보여줍니다. 기간이 알려진 작업 중에 사용되지만 해당 진행률이 사용자의 앱 조작을 차단하지 않아야 합니다.
-   ProgressBar의 *확정되지 않음* 상태는 작업이 진행 중이며 사용자의 앱 조작을 차단하지 않고 완료 시간을 알 수 없음을 보여줍니다.
-   ProgressRing의 *확정되지 않음* 상태는 작업이 진행 중이며 사용자의 앱 조작을 차단하고 완료 시간을 알 수 없다는 것을 보여줍니다.


또한 진행률 컨트롤은 읽기 전용이므로 조작할 수 없습니다. 즉, 사용자가 이러한 컨트롤을 직접 호출하거나 사용할 수 없다는 의미입니다.

|제어|표시|
|---|---|
| 확정되지 않은 ProgressBar | ![ProgressBar 확정되지 않음](images/progressbar-indeterminate.gif) |
| 확정된 ProgressBar | ![ProgressBar 확정됨](images/progressbar-determinate.png)|
| 확정되지 않은 ProgressRing | ![확정되지 않은 ProgressRing 상태](images/progressring-indeterminate.gif)|
| 확정된 ProgressRing | ![확정된 ProgressRing 상태](images/progress_ring.jpg)|


## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 앱을 열고 실제로 작동하는 <a href="xamlcontrolsgallery:/item/ProgressBar">ProgressBar</a> 또는 <a href="xamlcontrolsgallery:/item/ProgressRing">ProgressRing</a>을 확인합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="when-to-use-each-control"></a>각 컨트롤 사용 시기

현재 상태를 표시하려고 할 때 사용할 컨트롤 또는 상태(확정 및 확정되지 않음)가 항상 명확한 것은 아닙니다. 진행률 컨트롤이 필요하지 않을 정도로 작업이 명확한 경우도 있고, 진행률 컨트롤이 사용되더라도 사용자에게 작업이 진행 중임을 설명하는 텍스트 줄이 필요한 경우도 있습니다.

### <a name="progressbar"></a>ProgressBar
-   **컨트롤에 기간 또는 예측 가능한 종료 시점이 정의되어 있나요?**

    확정 ProgressBar를 사용하고 백분율 또는 값을 적절히 업데이트합니다.

-   **사용자가 작업 진행률을 모니터링하지 않고도 계속 진행할 수 있나요?**

    ProgressBar를 사용하는 경우 조작(방식)은 모달이 아닙니다. 즉, 일반적으로 작업이 완료될 때까지 사용자가 차단되지 않으며 해당 측면이 완료되기 전에도 다른 방법으로 앱을 계속 사용할 수 있습니다.

-   **키워드**

    작업에서 다음 키워드가 사용되거나 다음 키워드와 일치하는 텍스트를 진행률 작업과 함께 표시하는 경우 ProgressBar를 사용하는 것이 좋습니다.

    - *로드 중...*
    - *검색 중*
    - *작업 중...*

### <a name="progressring"></a>ProgressRing

-   **작업으로 인해 사용자가 계속 대기 중인가요?**

    작업이 완료될 때까지 모든(또는 상당 부분) 앱 조작을 대기해야 하는 경우 확정되지 않은 ProgressRing을 사용하는 것이 더 좋습니다.

    -   **컨트롤에 기간 또는 예측 가능한 종료 시점이 정의되어 있나요?**

    시각적 개체가 막대 대신 링이 되도록 하려면 확정된 ProgressRing을 사용하고 그에 따라 백분율 또는 값을 업데이트합니다. 

-   **사용자 작업이 완료될 때까지 앱이 대기하나요?**

    이 경우 사용자에 대한 알 수 없는 대기 시간을 나타낸다는 의미이므로 확정되지 않은 ProgressRing을 사용합니다.

-   **키워드**

    작업에서 다음 키워드가 사용되거나 다음 키워드와 일치하는 텍스트를 진행률 작업과 함께 표시하는 경우 ProgressRing을 사용하는 것이 좋습니다.

    - *새로 고침 중*
    - *로그인 중...*
    - *연결 중...*

### <a name="no-progress-indication-necessary"></a>진행률 표시가 필요하지 않음
-   **어떤 일이 일어나고 있는지 사용자가 알아야 하나요?**

    예를 들어 앱이 백그라운드에서 항목을 다운로드 중이고 사용자가 다운로드를 시작하지 않았다면 사용자는 해당 작업을 몰라도 됩니다.

-   **컴퓨터 작업이 사용자 작업을 차단하지 않으며 사용자와 거의 무관한 백그라운드 작업인가요?**

    항상 보일 필요는 없지만 상태를 표시할 필요는 있는 작업을 앱이 수행하고 있을 때는 텍스트를 사용합니다.

-   **사용자에게 작업의 완료 여부만 필요한가요?**

    때로는 작업이 완료된 경우에만 알림을 표시하거나 작업이 완료된 즉시 시각적으로 표시하고 백그라운드에서 마무리 작업을 실행하는 것이 가장 좋습니다.

## <a name="progress-controls-best-practices"></a>진행률 컨트롤 모범 사례

다음과 같이 다양한 진행률 컨트롤을 사용할 시기 및 위치를 시각적으로 표시하는 것이 가장 좋은 경우도 있습니다.

**ProgressBar - 확정**

![ProgressBar 확정 예제](images/progress-bar-determinate-example.png)

첫 번째 예제는 확정 ProgressBar입니다. 작업 기간을 알고 있는 경우나 설치, 다운로드, 설정하는 경우 등에는 확정 ProgressBar가 가장 적합합니다.

**ProgressBar - 확정되지 않음**

![ProgressBar 확정되지 않음 예제](images/progress-bar-indeterminate-example.png)

작업 기간을 알 수 없는 경우 확정되지 않은 ProgressBar를 사용합니다. 확정되지 않은 ProgressBar는 가상화된 목록을 채우고 확정되지 않은 ProgressBar에서 확정 ProgressBar로의 자연스러운 시각적 전환을 만들 때도 좋습니다.

-   **가상화된 컬렉션에 작업이 있나요?**

    이 경우 각 목록 항목이 나타날 때 진행률 표시기를 배치하지 마세요. 대신 ProgressBar를 사용하고 로드 중인 항목 컬렉션 위에 배치하여 가져오는 항목을 표시합니다.

**ProgressRing - 확정되지 않음**

![ProgressRing 확정되지 않은 예제](images/PR_IndeterminateExample.png)

확정되지 않은 ProgressRing은 사용자의 앱 조작이 중단되었거나 앱이 계속 진행되기 위해 사용자 입력을 대기 중인 경우에 사용됩니다. 위의 “로그인...” 예제는 ProgressRing의 완벽한 시나리오로, 사용자는 로그인이 완료될 때까지 앱을 사용할 수 없습니다.

**ProgressRing - 확정됨**

![ProgressRing 확정 예제](images/progress_ring_determinate_example.png)

작업 기간을 알고 있는 경우와 링 비주얼이 필요한 경우, 설치, 다운로드, 설정 등에는 확정된 ProgressRing이 가장 좋습니다.

## <a name="customizing-a-progress-control"></a>진행률 컨트롤 사용자 지정

두 진행률 컨트롤은 비교적 간단하지만 컨트롤의 일부 시각적 기능은 사용자 지정하기에 명확하지 않습니다.

**ProgressRing 크기 조정**

ProgressRing은 20x20epx 이내에서 원하는 대로 크기를 조정할 수 있습니다. ProgressRing의 크기를 조정하려면 해당 높이와 너비를 설정해야 합니다. 높이 또는 너비만 설정된 경우 컨트롤이 최소 크기(20x20epx)라고 간주합니다. 반대로 높이와 너비가 서로 다른 크기로 설정되면 더 작은 크기로 간주됩니다.
ProgressRing이 요구 사항에 적합하도록 높이와 너비 모두를 동일한 값으로 설정합니다.

```XAML
<ProgressRing Height="100" Width="100"/>
```

ProgressRing을 표시하고 애니메이션 효과를 주려면 IsActive 속성을 true로 설정해야 합니다.

```XAML
<ProgressRing IsActive="True" Height="100" Width="100"/>
```

```C#
progressRing.IsActive = true;
```

**진행률 컨트롤 색 지정**

기본적으로 진행률 컨트롤의 기본 색은 시스템의 테마 컬러로 설정됩니다. 이 브러시를 재정의하려면 두 컨트롤 중 하나의 전경 속성을 변경합니다.

```XAML
<ProgressRing IsActive="True" Height="100" Width="100" Foreground="Blue"/>
<muxc:ProgressBar Width="100" Foreground="Green"/>
```

ProgressRing의 전경색을 변경하면 링의 색도 변경됩니다. ProgressBar의 전경 속성은 막대의 채우기 색을 변경합니다. 막대의 채워지지 않은 부분을 변경하려면 배경 속성을 재정의합니다.

**대기 커서 표시**

앱 또는 작업에 생각할 시간이 필요하거나, 대기 커서가 사라질 때까지 대기 커서가 표시된 앱 또는 영역을 조작할 수 없음을 사용자에게 알려야 하는 경우 등에는 간단한 대기 커서만 표시하는 것이 가장 좋을 수도 있습니다.

```C#
Window.Current.CoreWindow.PointerCursor = new Windows.UI.Core.CoreCursor(Windows.UI.Core.CoreCursorType.Wait, 10);
```

## <a name="get-the-sample-code"></a>샘플 코드 가져오기

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery) - 대화형 형식으로 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련된 문서

- [ProgressBar 클래스](/uwp/api/Windows.UI.Xaml.Controls.ProgressBar)
- [ProgressRing 클래스](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing)
