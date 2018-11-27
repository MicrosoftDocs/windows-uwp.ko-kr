---
Description: A progress control provides feedback to the user that a long-running operation is underway.
title: 진행률 컨트롤에 대한 지침
ms.assetid: FD53B716-C43D-408D-8B07-522BC1F3DF9D
label: Progress controls
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, uwp
pm-contact: kisai
design-contact: jeffarn
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c0340b4b990a2397e24ad83af0d9239fe00f6f21
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7704700"
---
# <a name="progress-controls"></a>진행률 컨트롤

 

진행률 컨트롤은 긴 작업을 진행 중인 사용자에게 피드백을 제공합니다. 이는 진행률 표시기가 표시될 때 사용자가 앱을 조작할 수 없다는 의미이며 사용되는 표시기에 따라 대기 시간을 예측할 수도 있다는 의미입니다.

> **중요 API**: [ProgressBar 클래스](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.aspx), [IsIndeterminate 속성](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.isindeterminate.aspx), [ProgressRing 클래스](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.aspx), [IsActive 속성](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.isactive.aspx)

## <a name="types-of-progress"></a>진행률 유형

사용자에게 작업 진행을 표시하기 위해 ProgressBar 또는 ProgressRing 두 가지 컨트롤이 있습니다.

-   ProgressBar *확정* 상태는 작업의 완료율을 표시합니다. 기간이 알려진 작업 중 사용되지만 해당 진행률이 사용자의 앱 조작을 차단하지 않아야 합니다.
-   ProgressBar *확정되지 않음* 상태는 작업이 진행 중이며 사용자의 앱 조작을 차단하지 않고 완료 시간을 알 수 없다는 의미입니다.
-   ProgressRing은 *확정되지 않음* 상태로만 설정할 수 있으며 작업이 완료될 때까지 추가 사용자 조작이 차단될 때 사용되어야 합니다.

또한 진행률 컨트롤은 읽기 전용이므로 조작할 수 없습니다. 즉, 사용자가 이러한 컨트롤을 직접 호출하거나 사용할 수 없다는 의미입니다.

![ProgressBar 상태](images/ProgressBar_TwoStates.png)

*위쪽에서 아래쪽 - 확정되지 않음 ProgressBar 및 확정 ProgressBar*

![ProgressRing 상태](images/ProgressRing_SingleState.png)

*확정되지 않음 ProgressRing*

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 앱을 열고 작동 중인 <a href="xamlcontrolsgallery:/item/ProgressBar">ProgressBar</a> 또는 <a href="xamlcontrolsgallery:/item/ProgressRing">ProgressRing</a>을 확인합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="when-to-use-each-control"></a>각 컨트롤 사용 시기

현재 상태를 표시하려고 할 때 사용할 컨트롤 또는 상태(확정 및 확정되지 않음)가 항상 명확한 것은 아닙니다. 때로는 진행률 컨트롤이 필요하지 않을 정도로 작업이 명확하거나, 진행률 컨트롤이 사용되더라도 사용자에게 작업이 진행 중임을 설명하는 텍스트 줄이 필요할 수 있습니다.

### <a name="progressbar"></a>ProgressBar
-   **컨트롤에 기간 또는 예측 가능한 종료 시점이 정의되어 있나요?**

    확정 ProgressBar를 사용하고 백분율 또는 값을 적절히 업데이트합니다.

-   **사용자가 작업의 진행률을 모니터링하지 않고 계속 진행할 수 있나요?**

    ProgressBar를 사용 중일 때 조작은 모달이 아닙니다. 즉, 일반적으로 해당 작업이 완료될 때까지 사용자가 차단되지 않으며 해당 상태가 완료되기 전에도 다른 방법으로 앱을 계속 사용할 수 있다는 것을 의미합니다.

-   **키워드**

    작업에서 다음 키워드가 사용되거나 다음 키워드와 일치하는 진행률 작업과 함께 텍스트를 표시하는 경우 ProgressBar를 사용하는 것이 좋습니다.

    - *로드 중...*
    - *검색 중*
    - *작업 중...*

### <a name="progressring"></a>ProgressRing

-   **작업으로 인해 사용자가 계속 대기 중인가요?**

    작업이 완료될 때까지 모든(또는 상당 부분) 앱 조작을 대기해야 하는 경우 ProgressRing을 사용하는 것이 더 좋습니다. ProgressRing 컨트롤은 모달 조작에 사용되며 이는 ProgressRing이 사라질 때까지 사용자가 차단된다는 의미입니다.

-   **사용자 작업이 완료될 때까지 앱이 대기하나요?**

    이 경우 사용자에 대한 알 수 없는 대기 시간을 나타낸다는 의미이므로 ProgressRing을 사용합니다.

-   **키워드**

    작업에서 다음 키워드가 사용되거나 다음 키워드와 일치하는 진행률 작업과 함께 텍스트를 표시하는 경우 ProgressRing을 사용하는 것이 좋습니다.

    - *새로 고침 중*
    - *로그인 중...*
    - *연결하는 중...*

### <a name="no-progress-indication-necessary"></a>진행률 표시가 필요하지 않음
-   **어떤 일이 일어나고 있는지를 사용자가 알아야 하나요?**

    예를 들어 앱이 백그라운드에서 다운로드하고 있을 때 사용자가 다운로드를 시작하지 않았다면 사용자는 이를 알 필요가 없습니다.

-   **컴퓨터 작업이 사용자 작업을 차단하지 않으며 사용자와 거의 무관한 백그라운드 작업인가요?**

    항상 보일 필요는 없지만 상태를 표시할 필요는 있는 작업을 앱이 수행하고 있을 때는 텍스트를 사용합니다.

-   **사용자에게 작업의 완료 여부만 필요한가요?**

    경우에 따라 작업이 완료될 때만 알림을 표시하거나 작업이 완료된 즉시 시각적으로 표시하고 백그라운드에서 완료 작업을 실행하는 것이 좋을 때가 있습니다.

## <a name="progress-controls-best-practices"></a>진행률 컨트롤 모범 사례

다음과 같이 다양한 진행률 컨트롤을 사용할 시기 및 위치를 시각적으로 표시하는 것이 가장 좋은 경우가 있습니다.

**ProgressBar - 확정**

![ProgressBar 확정 예제](images/PB_DeterminateExample.png)

첫 번째 예제는 확정 ProgressBar입니다. 작업 기간을 알고 있는 경우나 설치, 다운로드, 설정하는 경우 등에는 확정 ProgressBar가 가장 적합합니다.

**ProgressBar 확정되지 않음**

![ProgressBar 확정되지 않음 예제](images/PB_IndeterminateExample.png)

작업 기간을 알 수 없는 경우 확정되지 않음 ProgressBar를 사용합니다. 확정되지 않음 ProgressBar는 가상화된 목록을 채우고 확정되지 않음 ProgressBar에서 확정 ProgressBar로의 자연스러운 시각적 전환을 만들 때도 좋습니다.

-   **가상화된 컬렉션에 작업이 있나요?**

    이 경우 각 목록 항목이 나타날 때 진행률 표시기를 배치하지 마세요. 대신 ProgressBar를 사용하고 로드 중인 항목 컬렉션 위에 배치하여 가져오는 항목을 표시합니다.

**ProgressRing - 확정되지 않음**

![ProgressRing 확정되지 않음 예제](images/PR_IndeterminateExample.png)

확정되지 않음 ProgressRing은 앱에 대한 사용자의 추가 조작이 중단되거나 앱이 사용자가 계속 입력하기를 기다릴 때 사용됩니다. 위의 "로그인 중...." 예제는 ProgressRing에 대한 완벽한 시나리오이며 로그인이 완료될 때까지 사용자는 앱을 사용할 수 없습니다.

## <a name="customizing-a-progress-control"></a>진행률 컨트롤 사용자 지정

두 진행률 컨트롤은 비교적 간단하지만 컨트롤의 일부 시각적 기능은 사용자 지정하기에 명확하지 않습니다.

**ProgressRing 크기 조정**

ProgressRing은 20x20epx 이내에서 원하는 대로 크기를 조정할 수 있습니다. ProgressRing의 크기를 조정하려면 해당 높이와 너비를 설정해야 합니다. 높이 또는 너비만 설정된 경우 컨트롤이 최소 크기(20x20epx)라고 간주합니다. 반대로 높이와 너비가 서로 다른 크기로 설정되면 더 작은 크기로 간주됩니다.
ProgressRing이 요구 사항에 적합하도록 하려면 높이와 너비 모두를 동일한 값으로 설정합니다.

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
<ProgressBar Width="100" Foreground="Green"/>
```

ProgressRing의 전경색을 변경하면 점의 색도 변경됩니다. ProgressBar의 전경 속성은 막대의 채우기 색을 변경합니다. 막대의 채워지지 않은 부분을 변경하려면 배경 속성을 재정의합니다.

**대기 커서 표시**

때로는 간단한 대기 커서만 표시하는 것이 좋을 수도 있습니다. 즉, 앱 또는 작업에 인지할 시간이 필요한 경우, 대기 커서가 사라질 때까지 사용자가 대기 커서가 표시되는 앱 또는 영역을 조작하면 안 된다고 알리는 경우 등에는 대기 커서를 사용하는 것이 좋습니다.

```C#
Window.Current.CoreWindow.PointerCursor = new Windows.UI.Core.CoreCursor(Windows.UI.Core.CoreCursorType.Wait, 10);
```

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련 문서

- [ProgressBar 클래스](https://msdn.microsoft.com/library/windows/apps/br227529)
- [ProgressRing 클래스](https://msdn.microsoft.com/library/windows/apps/br227538)

**개발자용(XAML)**
- [진행률 컨트롤 추가](https://msdn.microsoft.com/library/windows/apps/xaml/hh780651)
- [Windows Phone에 대한 확정되지 않은 사용자 지정 진행률 표시줄을 만드는 방법](http://go.microsoft.com/fwlink/p/?LinkID=392426)
