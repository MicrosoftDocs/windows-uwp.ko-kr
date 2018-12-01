---
Description: View multiple parts of your app in separate windows.
title: 앱에 대한 여러 보기 표시
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 107c904dc4b89941c0f453efd830504d2d032534
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8342789"
---
# <a name="show-multiple-views-for-an-app"></a>앱에 대한 여러 보기 표시

![여러 창으로 앱을 보여 주는 와이어프레임](images/multi-view.gif)

개별 창에서 앱의 독립 부분을 볼 수 있도록 하여 사용자의 생산성을 높이는 데 도움을 줍니다. 여러 개의 앱 창을 만드는 경우 각 창이 독립적으로 동작합니다. 작업 표시줄에 각 창이 개별적으로 표시됩니다. 사용자는 앱 창을 독립적으로 이동, 크기 조정, 표시 및 숨길 수 있으며, 별도의 앱인 것처럼 앱 창 간에 전환할 수 있습니다. 각 창은 해당 스레드에서 작동합니다.

> **중요 API**: [**ApplicationViewSwitcher**](https://msdn.microsoft.com/library/windows/apps/dn281094), [**CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278)

## <a name="when-should-an-app-use-multiple-views"></a>앱에서 언제 여러 보기를 사용해야 하나요?
여러 보기를 활용할 수 있는 다양한 시나리오가 있습니다. 다음은 몇 가지 예입니다.
 - 사용자가 받은 메시지 목록을 보면서 새 이메일을 작성할 수 있는 이메일 앱
 - 사용자가 여러 사람의 연락처 정보를 나란히 비교할 수 있는 주소록 앱
 - 사용자가 현재 재생 중인 음악과 재생 가능한 음악의 목록을 동시에 볼 수 있는 음악 플레이어 앱
 - 사용자가 노트의 한 페이지에서 다른 페이지로 정보를 복사할 수 있는 메모 작성 앱
 - 사용자가 모든 상위 헤드라인을 정독한 후 나중에 읽을 수 있도록 여러 기사를 열어 둘 수 있는 읽기 앱

다수의 앱 인스턴스를 따로 만들려면 [다중 인스턴스 UWP 앱 만들기](../../launch-resume/multi-instance-uwp.md)를 참조하세요.

## <a name="what-is-a-view"></a>보기란?

앱 보기는 앱이 콘텐츠를 표시하는 데 사용하는 창과 스레드의 1:1 연결입니다. [**Windows.ApplicationModel.Core.CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) 개체로 표시됩니다.

보기는 [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016) 개체에 의해 관리됩니다. [**CoreApplication.CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278)를 호출하여 [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) 개체를 만듭니다. **CoreApplicationView**는 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 및 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)([**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br225019) 및 [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/dn433264) 속성에 저장됨)를 한 곳에서 표시합니다. **CoreApplicationView**는 Windows 런타임이 핵심 Windows 시스템과 상호 작용하는 데 사용하는 개체로 간주할 수 있습니다.

일반적으로 [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017)를 직접 사용하지는 않습니다. 대신, Windows 런타임은 [**Windows.UI.ViewManagement**](https://msdn.microsoft.com/library/windows/apps/hh701658) 네임스페이스의 [**ApplicationView**](https://msdn.microsoft.com/library/windows/apps/br242295) 클래스를 제공합니다. 이 클래스는 앱이 창 시스템과 상호 작용할 때 사용하는 속성, 메서드 및 이벤트를 제공합니다. **ApplicationView**를 사용하려면 현재 **CoreApplicationView**의 스레드에 연결된 **ApplicationView** 인스턴스를 가져오는 정적 [**ApplicationView.GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/hh701672) 메서드를 호출합니다.

마찬가지로, XAML 프레임워크는 [**Windows.UI.XAML.Window**](https://msdn.microsoft.com/library/windows/apps/br208225) 개체에 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br209041) 개체를 래핑합니다. XAML 앱에서는 일반적으로 **CoreWindow**를 직접 사용하는 대신 **Window** 개체를 조작합니다.

## <a name="show-a-new-view"></a>새 보기 표시

각 앱 레이아웃은 고유하지만, 콘텐츠의 오른쪽 위 모서리와 같은 예측 가능한 위치에 "새 창" 단추를 배치하여 새 창으로 열 수 있도록 하는 것이 좋습니다. 또한 상황에 맞는 메뉴 옵션에 "새 창에서 열기"를 넣는 것도 고려하십시오.

새로운 보기를 만드는 단계를 살펴보겠습니다. 여기서 새 보기는 단추 클릭에 대한 응답으로 시작됩니다.

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    CoreApplicationView newView = CoreApplication.CreateNewView();
    int newViewId = 0;
    await newView.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        Frame frame = new Frame();
        frame.Navigate(typeof(SecondaryPage), null);   
        Window.Current.Content = frame;
        // You have to activate the window in order to show it later.
        Window.Current.Activate();

        newViewId = ApplicationView.GetForCurrentView().Id;
    });
    bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);
}
```

**새 보기를 표시하려면**

1.  [**CoreApplication.CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297291)를 호출하여 보기 콘텐츠에 대한 새 창과 스레드를 만듭니다.

    ```csharp
    CoreApplicationView newView = CoreApplication.CreateNewView();
    ```

2.  새 보기의 [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120)를 추적합니다. 나중에 보기를 표시하는 데 사용합니다.

    만든 보기의 추적에 도움이 되도록 앱에 일부 인프라를 빌드하는 것이 좋습니다. 예제는 [MultipleViews 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620574)의 `ViewLifetimeControl` 클래스를 참조하세요.

    ```csharp
    int newViewId = 0;
    ```

3.  새 스레드에서 창을 채웁니다.

    [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 메서드를 사용하여 새 보기에 대한 UI 스레드에 작업을 예약합니다. [람다 식](http://go.microsoft.com/fwlink/p/?LinkId=389615)을 사용하여 함수를 **RunAsync** 메서드에 인수로 전달합니다. 람다 함수에서 수행하는 작업이 새 보기의 스레드에서 발생합니다.

    XAML에서는 일반적으로 [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041)의 [**Content**](https://msdn.microsoft.com/library/windows/apps/br209051) 속성에 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)을 추가한 다음 **Frame**에서 앱 콘텐츠를 정의한 XAML [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503)로 이동합니다. 자세한 내용은 [두 페이지 간의 피어 투 피어 탐색:](../basics/navigate-between-two-pages.md)을 참조하세요.

    새 [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041)가 채워진 후 나중에 **Window**를 표시하려면 **Window**의 [**Activate**](https://msdn.microsoft.com/library/windows/apps/br209046) 메서드를 호출해야 합니다. 이 작업은 새 보기의 스레드에서 발생하므로 새 **Window**가 활성화됩니다.

    마지막으로, 나중에 보기를 표시하는 데 사용할 새 보기의 [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120)를 가져옵니다. 마찬가지로 이 작업은 새 보기의 스레드에서 발생하므로 [**ApplicationView.GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/hh701672)는 새 보기의 **Id**를 가져옵니다.

    ```csharp
    await newView.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        Frame frame = new Frame();
        frame.Navigate(typeof(SecondaryPage), null);   
        Window.Current.Content = frame;
        // You have to activate the window in order to show it later.
        Window.Current.Activate();

        newViewId = ApplicationView.GetForCurrentView().Id;
    });
    ```

4.  [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://msdn.microsoft.com/library/windows/apps/dn281101)를 호출하여 새 보기를 표시합니다.

    새 보기를 만든 후 [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://msdn.microsoft.com/library/windows/apps/dn281101) 메서드를 호출하여 새 창에 표시할 수 있습니다. 이 메서드의 *viewId* 매개 변수는 앱에서 각 보기를 고유하게 식별하는 정수입니다. **ApplicationView.Id** 속성 또는 [**ApplicationView.GetApplicationViewIdForWindow**](https://msdn.microsoft.com/library/windows/apps/dn281120) 메서드를 사용하여 [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281109) 보기를 검색합니다.

    ```csharp
    bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);
    ```

## <a name="the-main-view"></a>기본 보기


앱을 시작할 때 생성되는 첫 번째 보기를 *기본 보기*라고 합니다. 이 보기는 [**CoreApplication.MainView**](https://msdn.microsoft.com/library/windows/apps/hh700465) 속성에 저장되고 해당 [**IsMain**](https://msdn.microsoft.com/library/windows/apps/hh700452) 속성은 true입니다. 이 보기는 직접 만들지 않고 앱에 의해 생성됩니다. 기본 보기의 스레드는 앱 관리자 역할을 하며 모든 앱 활성화 이벤트가 이 스레드에서 전달됩니다.

보조 보기가 열려 있으면 기본 보기의 창을 숨길 수 있지만(예: 창 제목 표시줄에서 닫기(x) 단추 클릭) 해당 스레드는 활성 상태로 유지됩니다. 기본 보기의 [**Window**](https://msdn.microsoft.com/library/windows/apps/br209049)에서 [**Close**](https://msdn.microsoft.com/library/windows/apps/br209041)를 호출하면 **InvalidOperationException**이 발생합니다. 앱을 닫으려면 [**Application.Exit**](https://msdn.microsoft.com/library/windows/apps/br242327)를 사용합니다. 기본 보기의 스레드가 종료되면 앱이 닫힙니다.

## <a name="secondary-views"></a>보조 보기


앱 코드에서 [**CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278)를 호출하여 만든 모든 보기를 비롯한 다른 보기는 보조 보기입니다. 기본 보기와 보조 보기는 둘 다 [**CoreApplication.Views**](https://msdn.microsoft.com/library/windows/apps/br205861) 컬렉션에 저장됩니다. 일반적으로 사용자 작업에 대한 응답으로 보조 보기를 만듭니다. 시스템이 앱에 대한 보조 보기를 만드는 경우도 있습니다.

> [!NOTE]
> Windows *할당된 액세스* 기능을 사용하여 [키오스크 모드](https://technet.microsoft.com/library/mt219050.aspx)로 앱을 실행할 수 있습니다. 이렇게 하면 시스템이 보조 보기를 만들어 잠금 화면 위에 앱 UI를 표시합니다. 앱에서 만든 보조 보기는 허용되지 않으므로 사용자 고유의 보조 보기를 키오스크 모드로 표시하려고 하면 예외가 발생합니다.

## <a name="switch-from-one-view-to-another"></a>보기 간에 전환

사용자가 보조 창에서 부모 창으로 다시 이동할 수 있는 방법을 제공합니다. 이렇게 하려면 [**ApplicationViewSwitcher.SwitchAsync**](https://msdn.microsoft.com/library/windows/apps/dn281097) 메서드를 사용합니다. 전환 중인 창의 스레드에서 이 메서드를 호출하고 전환할 창의 보기 ID를 전달합니다.

```csharp
await ApplicationViewSwitcher.SwitchAsync(viewIdToShow);
```

[**SwitchAsync**](https://msdn.microsoft.com/library/windows/apps/dn281097)를 사용하는 경우 [**ApplicationViewSwitchingOptions**](https://msdn.microsoft.com/library/windows/apps/dn281105) 값을 지정하여 초기 창을 닫고 작업 표시줄에서 제거할지 여부를 선택할 수 있습니다.

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항

* "새 창 열기" 기호를 활용하여 보조 보기로의 깔끔한 진입점을 제공합니다.
* 사용자에게 보조 보기 목적을 알립니다.
* 앱은 단일 보기에서 완벽하게 작동하며, 보조 보기는 사용자의 편의를 위해서만 열립니다.
* 알림이나 기타 일시적인 시각 효과를 제공하기 위해 보조 보기에 의존하지 마십시오.

## <a name="related-topics"></a>관련 항목

* [ApplicationViewSwitcher](https://msdn.microsoft.com/library/windows/apps/dn281094)
* [CreateNewView](https://msdn.microsoft.com/library/windows/apps/dn297278)
 
