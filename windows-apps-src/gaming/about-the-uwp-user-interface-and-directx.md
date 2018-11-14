---
author: mtoepke
title: 앱 개체 및 DirectX
description: DirectX로 작성된 UWP(유니버설 Windows 플랫폼) 게임은 Windows UI 사용자 인터페이스 요소 및 개체를 거의 사용하지 않습니다.
ms.assetid: 46f92156-29f8-d65e-2587-7ba1de5b48a6
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, directx, 앱 개체
ms.localizationpriority: medium
ms.openlocfilehash: 7e29a19410915836be3c54c0dc04a6d7dc29ceeb
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6665415"
---
# <a name="the-app-object-and-directx"></a>앱 개체 및 DirectX



DirectX로 작성된 UWP(유니버설 Windows 플랫폼) 게임은 Windows UI 사용자 인터페이스 요소 및 개체를 거의 사용하지 않습니다. 더 정확히 말하면 그러한 요소 및 개체는 Windows 런타임 스택의 하위 수준에서 실행되므로 앱 개체에 직접 액세스하여 상호 작용하는 보다 본질적인 방식으로 사용자 인터페이스 프레임워크와 상호 작용해야 합니다. 이러한 상호 작용이 발생하는 경우와, DirectX 개발자로서 UWP 앱 개발에 이 모델을 효율적으로 사용하는 방법에 대해 학습합니다.

낯선 그래픽 조건이 나 읽는 동안 발생 하는 개념에 대 한 정보에 대 한 [Direct3D 그래픽 용어](../graphics-concepts/index.md) 를 참조 하세요.

## <a name="the-important-core-user-interface-namespaces"></a>중요한 핵심 사용자 인터페이스 네임스페이스


먼저 (**using**을 사용하여) UWP 앱에 포함해야 할 Windows 런타임 네임스페이스에 대해 조금 자세히 살펴보겠습니다.

-   [**Windows.ApplicationModel.Core**](https://msdn.microsoft.com/library/windows/apps/br205865)
-   [**Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766)
-   [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)
-   [**Windows.System**](https://msdn.microsoft.com/library/windows/apps/br241814)
-   [**Windows.Foundation**](https://msdn.microsoft.com/library/windows/apps/br226021)

> **참고**  UWP 앱을 개발 하지 않는 경우 JavaScript 또는 XAML 특정 라이브러리 및 네임이 스페이스에 제공 된 유형 대신 네임 스페이스에 제공 된 사용자 인터페이스 구성 요소를 사용 합니다.

 

## <a name="the-windows-runtime-app-object"></a>Windows 런타임 앱 개체


UWP 앱에서는 보기를 가져오고 스왑 체인을 연결할 수 있는 창 및 보기 공급자가 필요합니다(디스플레이 버퍼). 또한 이 보기를 실행 중인 앱에 대한 창별 이벤트에 연결할 수도 있습니다. [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 유형으로 정의된 앱 개체의 부모 창을 가져오려면 앞 코드 조각에서 했던 대로 [**IFrameworkViewSource**](https://msdn.microsoft.com/library/windows/apps/hh700482)를 구현합니다.

다음은 핵심 사용자 인터페이스 프레임워크를 사용하여 창을 가져오는 기본 단계입니다.

1.  [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478)를 구현하는 유형을 만듭니다. 이는 개발자의 보기입니다.

    이 유형에서는 다음을 정의합니다.

    -   [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017)의 인스턴스를 매개 변수로 사용하는 [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) 메서드 이 유형의 인스턴스는 [**CoreApplication.CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278)를 호출하여 가져올 수 있습니다. 앱 개체는 앱이 시작되면 이 메서드를 호출합니다.
    -   [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)의 인스턴스를 매개 변수로 사용하는 [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509) 메서드 새 [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) 인스턴스의 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br225019) 속성에 액세스하여 이 유형의 인스턴스를 가져올 수 있습니다.
    -   진입점에 대한 문자열을 단독 매개 변수로 사용하는 [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501) 메서드 이 메서드를 호출하면 앱 개체는 진입점 문자열을 제공합니다. 여기가 리소스 설정 위치입니다. 여기서 장치 리소스를 만듭니다 앱 개체는 앱이 시작되면 이 메서드를 호출합니다.
    -   [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 개체를 활성화하고 창 이벤트 디스패처를 시작하는 [**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 메서드 앱 개체는 앱의 처리가 시작되면 이 메서드를 호출합니다.
    -   [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501) 호출에 설정된 리소스를 정리하는 [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523) 메서드 앱 개체는 앱이 종료되면 이 메서드를 호출합니다.

2.  [**IFrameworkViewSource**](https://msdn.microsoft.com/library/windows/apps/hh700482)를 구현하는 유형을 만듭니다. 이는 개발자의 뷰 공급자입니다.

    이 유형에서는 다음을 정의합니다.

    -   1단계에서 만든 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) 구현의 인스턴스를 반환하는 [**CreateView**](https://msdn.microsoft.com/library/windows/apps/hh700491)라는 메서드.

3.  뷰 공급자의 인스턴스를 **main**에서 [**CoreApplication.Run**](https://msdn.microsoft.com/library/windows/apps/hh700469)으로 전달합니다.

이러한 기본 개념을 기반으로 하여 접근 방법을 확장해야 하는 추가 옵션에 대해 알아보겠습니다.

## <a name="core-user-interface-types"></a>핵심 사용자 인터페이스 유형


다음은 유용하게 사용할 수 있는 Windows 런타임의 다른 핵심 사용자 인터페이스 유형입니다.

-   [**Windows.ApplicationModel.Core.CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017)
-   [**Windows.UI.Core.CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)
-   [**Windows.UI.Core.CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)

이러한 유형을 사용하여 앱의 보기, 특히 앱 부모 창의 콘텐츠를 작성하는 조각에 액세스하고 해당 창에 대해 발생한 이벤트를 처리할 수 있습니다. 앱 창의 프로세스는 격리되어 있고 모든 콜백을 처리하는 ASTA(*응용 프로그램 단일 스레드 아파트*)입니다.

앱의 보기는 앱 창의 뷰 공급자가 생성하고 대부분의 경우 특정 프레임워크 패키지나 시스템 자체에서 구현하므로 직접 구현할 필요가 없습니다. DirectX의 경우 앞에서 설명한 대로 씬 뷰 공급자를 구현해야 합니다. 다음 구성 요소와 동작 간에는 고유한 일대일 관계가 있습니다.

-   [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) 유형으로 표시되고 창을 업데이트하는 메서드를 정의하는 앱의 보기
-   앱의 스레딩 동작을 정의하는 속성인 ASTA. ASTA에서는 COM STA 특성이 지정된 유형의 인스턴스를 만들 수 없습니다.
-   앱이 시스템에서 가져오거나 사용자가 구현하는 뷰 공급자
-   [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 유형으로 표시되는 부모 창
-   모든 활성화 이벤트의 원본. 보기와 창 모두에 별도의 활성화 이벤트가 있습니다.

요약하면, 앱 개체는 뷰 공급자 팩터리를 제공합니다. 앱 개체는 뷰 공급자를 만들고 앱에 대한 부모 창을 인스턴스화합니다. 뷰 공급자는 앱의 부모 창에 대한 앱 보기를 정의합니다. 이제 보기 및 부모 창의

## <a name="coreapplicationview-behaviors-and-properties"></a>CoreApplicationView 동작 및 속성


[**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017)는 현재 앱 보기를 나타냅니다. 앱 singleton은 초기화 중에 앱 보기를 만들지만 보기는 활성화되기 전까지 유휴 상태로 유지됩니다. 그 [**CoreApplicationView.CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br225019) 속성에 액세스하면 보기를 표시하는 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)를 가질 수 있으며, [**CoreApplicationView.Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) 이벤트에 대리자를 등록하면 보기에 대한 활성화 및 비활성화를 처리할 수 있습니다.

## <a name="corewindow-behaviors-and-properties"></a>CoreWindow 동작 및 속성


앱 개체가 초기화되면 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 인스턴스인 부모 창이 만들어지고 뷰 공급자로 전달됩니다. 앱에 표시할 창이 없으면 창을 표시합니다. 그렇지 않으면 보기를 초기화하기만 합니다.

[**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)는 입력 및 기본 창 동작에 대한 다수의 이벤트를 제공합니다. 자신의 대리자를 이벤트에 등록하여 이러한 이벤트를 처리할 수 있습니다.

또한 [**CoreWindow.Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br208264) 속성에 액세스하여 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)의 인스턴스를 제공하는 창에 대한 창 이벤트 디스패처를 얻을 수 있습니다.

## <a name="coredispatcher-behaviors-and-properties"></a>CoreDispatcher 동작 및 속성


창에 대해 디스패치하는 이벤트의 스레딩 동작은 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 유형으로 확인할 수 있습니다. 이 유형에서 특히 중요한 메서드 하나는 창 이벤트 처리를 시작하는 [**CoreDispatcher.ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) 메서드입니다. 앱에 대해 이 메서드를 잘못된 옵션으로 호출하면 모든 종류의 예기치 않은 이벤트 처리 동작이 발생할 수 있습니다.

| CoreProcessEventsOption 옵션                                                           | 설명                                                                                                                                                                                                                                  |
|------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [**CoreProcessEventsOption.ProcessOneAndAllPending**](https://msdn.microsoft.com/library/windows/apps/br208217) | 큐에서 현재 사용 가능한 모든 이벤트를 디스패치합니다. 보류 중인 이벤트가 없으면 다음의 새 이벤트를 대기합니다.                                                                                                                                 |
| [**CoreProcessEventsOption.ProcessOneIfPresent**](https://msdn.microsoft.com/library/windows/apps/br208217)     | 큐에 보류 중인 이벤트 하나를 디스패치합니다. 보류 중인 이벤트가 없는 경우 새 이벤트가 발생할 때까지 대기하지 않고 대신 즉시 반환합니다.                                                                                          |
| [**CoreProcessEventsOption.ProcessUntilQuit**](https://msdn.microsoft.com/library/windows/apps/br208217)        | 새 이벤트를 대기하고 사용 가능한 모든 이벤트를 디스패치합니다. 창이 닫히거나 응용 프로그램이 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 인스턴스의 [**Close**](https://msdn.microsoft.com/library/windows/apps/br208260) 메서드를 호출할 때까지 이 동작을 계속합니다. |
| [**CoreProcessEventsOption.ProcessAllIfPresent**](https://msdn.microsoft.com/library/windows/apps/br208217)     | 큐에서 현재 사용 가능한 모든 이벤트를 디스패치합니다. 보류 중인 이벤트가 없으면 즉시 반환합니다.                                                                                                                                          |

 

그래픽 업데이트를 중단시킬 수 있는 차단 동작을 방지하려면 DirectX를 사용하는 UWP가 [**CoreProcessEventsOption.ProcessAllIfPresent**](https://msdn.microsoft.com/library/windows/apps/br208217) 옵션을 사용해야 합니다.

## <a name="asta-considerations-for-directx-devs"></a>DirectX 부분에 대한 ASTA 고려 사항


UWP 및 DirectX 앱의 런타임 표현을 정의하는 앱 개체는 ASTA(응용 프로그램 단일 스레드 아파트)라는 스레딩 모델을 사용하여 앱의 UI 보기를 호스트합니다. UWP 및 DirectX 앱을 개발하고 있는 경우 ASTA 속성에 익숙할 것입니다. UWP 및 DirectX 앱에서 디스패치하는 모든 스레드는 [**Windows::System::Threading**](https://msdn.microsoft.com/library/windows/apps/br229642) API 또는 [**CoreWindow::CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)를 사용해야 하기 때문입니다. 앱에서 [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589)를 호출하여 ASTA에 대한 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 개체를 가져올 수 있습니다.

UWP DirectX 앱 개발자로서 가장 중요하게 알고 있어야 할 사항은 **main()** 에서 **Platform::MTAThread**를 설정하여 앱 스레드에서 MTA 스레드를 디스패치하도록 해야 한다는 점입니다.

```cpp
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto myDXAppSource = ref new MyDXAppSource(); // your view provider factory 
    CoreApplication::Run(myDXAppSource);
    return 0;
}
```

UWP DirectX 앱에 대한 앱 개체가 활성화되면 UI 보기에 사용될 ASTA를 만듭니다. 새로운 ASTA 스레드는 뷰 공급자 팩터리로 호출되어 앱 개체에 대한 뷰 공급자를 만들므로 뷰 공급자 코드가 해당 ASTA 스레드에서 실행됩니다.

또한 ASTA에서 분리되는 모든 스레드는 MTA에 있어야 합니다. 분리되는 MTA 스레드는 다시 표시 문제를 생성하여 교착 상태를 발생시킬 수도 있습니다.

기존 코드를 ASTA 스레드에서 실행되도록 이식하려는 경우 다음 고려 사항에 주의해야 합니다.

-   [**CoWaitForMultipleObjects**](https://msdn.microsoft.com/library/windows/desktop/hh404144) 같은 대기 기능은 ASTA에서 STA에서와 다르게 작동합니다.
-   COM 호출 모달 루프는 ASTA에서 다르게 작동합니다. 발신 호출이 진행되는 동안 관련 없는 호출을 더 이상 받을 수 없습니다. 예를 들어 다음과 같은 동작은 ASTA에서 교착 상태를 만들므로 앱이 바로 작동 중지됩니다.
    1.  ASTA가 MTA 개체를 호출하고 인터페이스 포인터 P1을 전달합니다.
    2.  나중에 ASTA에서 동일한 MTA 개체를 호출합니다. MTA 개체는 ASTA로 반환되기 전에 P1을 호출합니다.
    3.  P1은 관련 없는 호출을 할 수 없으므로 ASTA에 표시되지 않습니다. 그러나 MTA 스레드는 P1을 호출하려고 하므로 차단됩니다.

    다음 방법으로 이 문제를 해결할 수 있습니다.
    -   병렬 패턴 라이브러리(PPLTasks.h)에 정의된 **async** 패턴 사용
    -   앱의 ASTA(앱의 메인 스레드)에서 최대한 빨리 [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215)를 호출하여 임의 호출 허용

    즉, 관련 없는 호출을 앱의 ASTA에 바로 전달할 수 없습니다. 비동기 호출에 대한 자세한 내용은 [C++의 비동기 프로그래밍](https://msdn.microsoft.com/library/windows/apps/mt187334)을 참조하세요.

전체적으로 UWP 앱을 디자인하는 경우 직접 MTA 스레드를 만들어 관리하는 대신 앱의 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 및 [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215)에 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)를 사용하여 모든 UI 스레드를 처리하세요. **CoreDispatcher**로 처리할 수 없는 별도의 스레드가 필요한 경우 비동기 패턴을 사용하고 앞에서 설명한 지침에 따라 다시 표시 문제를 방지하세요.

 

 




