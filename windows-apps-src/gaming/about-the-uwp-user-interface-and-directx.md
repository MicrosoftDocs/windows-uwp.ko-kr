---
title: 앱 개체 및 DirectX
description: DirectX 게임을 사용 하는 UWP (유니버설 Windows 플랫폼)에서는 많은 Windows UI 사용자 인터페이스 요소와 개체를 사용 하지 않습니다.
ms.assetid: 46f92156-29f8-d65e-2587-7ba1de5b48a6
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, directx, app 개체
ms.localizationpriority: medium
ms.openlocfilehash: 08d2039bd7b3b8aa248acca31615d34635929aa1
ms.sourcegitcommit: 48702934676ae366fd46b7d952396c5e2fb2cbbe
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/06/2021
ms.locfileid: "97927777"
---
# <a name="the-app-object-and-directx"></a>앱 개체 및 DirectX

DirectX 게임을 사용 하는 UWP (유니버설 Windows 플랫폼)에서는 많은 Windows UI 사용자 인터페이스 요소와 개체를 사용 하지 않습니다. 대신, Windows 런타임 스택의 하위 수준에서 실행 되기 때문에 앱 개체와 직접 액세스 하 고 상호 운용 하 여 보다 기본적인 방식으로 사용자 인터페이스 프레임 워크와 상호 운용 해야 합니다. 이러한 상호 운용성이 발생 하는 시기와 방법 및 DirectX 개발자가 UWP 앱 개발 시이 모델을 효과적으로 사용할 수 있는 방법에 대해 알아봅니다.

읽는 동안 발생 하는 익숙하지 않은 그래픽 용어 또는 개념에 대 한 자세한 내용은 [Direct3D 그래픽 용어집](../graphics-concepts/index.md) 을 참조 하십시오.

## <a name="the-important-core-user-interface-namespaces"></a>중요 한 핵심 사용자 인터페이스 네임 스페이스

먼저 UWP 앱에서를 **사용** 하 여 포함 해야 하는 Windows 런타임 네임 스페이스를 살펴보겠습니다. 약간의 세부 정보를 가져옵니다.

-   [**Windows ApplicationModel. 핵심**](/uwp/api/Windows.ApplicationModel.Core)
-   [**Windows ApplicationModel. 활성화**](/uwp/api/Windows.ApplicationModel.Activation)
-   [**Windows. u i >**](/uwp/api/Windows.UI.Core)
-   [**Windows.System**](/uwp/api/Windows.System)
-   [**Windows Foundation**](/uwp/api/Windows.Foundation)

> [!NOTE]
> UWP 앱을 개발 하 고 있지 않은 경우에는 이러한 네임 스페이스에 제공 된 형식 대신 JavaScript 또는 XAML 관련 라이브러리와 네임 스페이스에 제공 된 사용자 인터페이스 구성 요소를 사용 합니다.

## <a name="the-windows-runtime-app-object"></a>Windows 런타임 app 개체

UWP 앱에서 보기를 가져오고 스왑 체인 (표시 버퍼)을 연결할 수 있는 창 및 보기 공급자를 가져오려고 합니다. 이 보기를 실행 중인 앱에 대 한 창 특정 이벤트에 연결할 수도 있습니다. [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 형식으로 정의 되는 앱 개체의 부모 창을 가져오려면 [**IFrameworkViewSource**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkViewSource)를 구현 하는 형식을 만듭니다. **IFrameworkViewSource** 을 구현 하는 방법을 보여 주는 [c + +/winrt](../cpp-and-winrt-apis/index.md) 코드 예제는 [DirectX 및 Direct2D를 사용한 컴퍼지션 네이티브 상호 운용성](../composition/composition-native-interop.md)을 참조 하세요.

핵심 사용자 인터페이스 프레임 워크를 사용 하 여 창을 가져오는 기본적인 단계는 다음과 같습니다.

1.  [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView)를 구현 하는 형식을 만듭니다. 이것이 뷰입니다.

    이 형식에서 다음을 정의 합니다.

    -   [**CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) 의 인스턴스를 매개 변수로 사용 하는 [**Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize) 메서드입니다. [**CoreApplication. CreateNewView**](/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)를 호출 하 여이 형식의 인스턴스를 가져올 수 있습니다. 앱이 시작 될 때 앱 개체가 호출 합니다.
    -   [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 의 인스턴스를 매개 변수로 사용 하는 [**setwindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow) 메서드입니다. 새 [**CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) 인스턴스의 [**CoreWindow**](/uwp/api/windows.applicationmodel.core.coreapplicationview.corewindow) 속성에 액세스 하 여이 형식의 인스턴스를 가져올 수 있습니다.
    -   진입점에 대 한 문자열을 유일한 매개 변수로 사용 하는 [**Load**](/uwp/api/windows.applicationmodel.core.iframeworkview.load) 메서드입니다. App 개체는이 메서드를 호출할 때 진입점 문자열을 제공 합니다. 여기서는 리소스를 설정 합니다. 여기에서 장치 리소스를 만듭니다. 앱이 시작 될 때 앱 개체가 호출 합니다.
    -   [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 개체를 활성화 하 고 창 이벤트 디스패처를 시작 하는 [**Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) 메서드입니다. 앱의 프로세스를 시작할 때 앱 개체가이를 호출 합니다.
    -   [**Load**](/uwp/api/windows.applicationmodel.core.iframeworkview.load)호출에서 설정 된 리소스를 정리 하는 [**초기화**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize) 취소 메서드입니다. 앱 개체는 응용 프로그램이 닫힐 때이 메서드를 호출 합니다.

2.  [**IFrameworkViewSource**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkViewSource)를 구현 하는 형식을 만듭니다. 이것은 보기 공급자입니다.

    이 형식에서 다음을 정의 합니다.

    -   1 단계에서 만든 [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) 구현의 인스턴스를 반환 하는 [**createview**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource.createview) 라는 메서드

3.  뷰 공급자의 인스턴스를 [**CoreApplication**](/uwp/api/windows.applicationmodel.core.coreapplication.run) 에 전달 **합니다.**

이러한 기본 사항을 염두에 두면이 방법을 확장 하는 데 필요한 추가 옵션을 살펴보겠습니다.

## <a name="core-user-interface-types"></a>핵심 사용자 인터페이스 형식

다음은 도움이 될 수 있는 Windows 런타임의 다른 핵심 사용자 인터페이스 형식입니다.

-   [**CoreApplicationView입니다.**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)
-   [**CoreWindow.**](/uwp/api/Windows.UI.Core.CoreWindow)
-   [**CoreDispatcher.**](/uwp/api/Windows.UI.Core.CoreDispatcher)

이러한 유형을 사용하여 앱의 보기, 특히 앱 부모 창의 콘텐츠를 작성하는 조각에 액세스하고 해당 창에 대해 발생한 이벤트를 처리할 수 있습니다. 앱 창의 프로세스는 격리 되 고 모든 콜백을 처리 하는 *응용 프로그램 단일 스레드 아파트* (ASTA)입니다.

앱의 뷰는 앱 창의 보기 공급자에 의해 생성 되며, 대부분의 경우 특정 프레임 워크 패키지나 시스템 자체에서 구현 되므로 사용자가 직접 구현할 필요가 없습니다. DirectX의 경우 앞에서 설명한 대로 씬 뷰 공급자를 구현 해야 합니다. 다음 구성 요소와 동작 사이에는 특정 일대일 관계가 있습니다.

-   [**CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) 형식으로 표현 되는 앱의 뷰가 며, 창을 업데이트 하는 메서드를 정의 합니다.
-   ASTA 앱의 스레딩 동작을 정의 하는 특성입니다. ASTA에 COM STA 특성 형식의 인스턴스를 만들 수 없습니다.
-   앱이 시스템에서 얻거나 구현 하는 보기 공급자입니다.
-   [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 형식으로 표현 되는 부모 창입니다.
-   모든 활성화 이벤트를 소싱 합니다. 뷰와 창에는 각각 별도의 활성화 이벤트가 있습니다.

요약 하자면, app 개체는 뷰 공급자 팩터리를 제공 합니다. 뷰 공급자를 만들고 앱에 대 한 부모 창을 인스턴스화합니다. 보기 공급자는 앱의 부모 창에 대 한 앱의 뷰를 정의 합니다. 이제 뷰와 부모 창의 세부 사항을 설명 하겠습니다.

## <a name="coreapplicationview-behaviors-and-properties"></a>CoreApplicationView 동작 및 속성

[**CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) 은 현재 앱 뷰를 나타냅니다. 앱 singleton은 초기화 하는 동안 앱 보기를 만들지만 활성화 될 때까지 뷰는 유휴 상태로 유지 됩니다. [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 속성에 액세스 하 여 뷰를 표시 하는 [**CoreApplicationView**](/uwp/api/windows.applicationmodel.core.coreapplicationview.corewindow) 를 가져올 수 있으며, 대리자를 [**CoreApplicationView**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) 이벤트에 등록 하 여 뷰의 활성화 및 비활성화 이벤트를 처리할 수 있습니다.

## <a name="corewindow-behaviors-and-properties"></a>CoreWindow 동작 및 속성

[**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 인스턴스인 부모 창이 만들어지고 앱 개체가 초기화 될 때 뷰 공급자에 전달 됩니다. 앱에 표시할 창이 있으면 표시 됩니다. 그렇지 않으면 뷰를 초기화 하기만 하면 됩니다.

[**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 는 입력 및 기본 창 동작에 관련 된 다양 한 이벤트를 제공 합니다. 사용자 고유의 대리자를 등록 하 여 이러한 이벤트를 처리할 수 있습니다.

[**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher)의 인스턴스를 제공 하는 [**CoreWindow**](/uwp/api/windows.ui.core.corewindow.dispatcher) 속성에 액세스 하 여 창에 대 한 창 이벤트 디스패처를 가져올 수도 있습니다.

## <a name="coredispatcher-behaviors-and-properties"></a>CoreDispatcher 동작 및 속성

[**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) 형식을 사용 하 여 창에 대 한 이벤트 디스패치의 스레딩 동작을 결정할 수 있습니다. 이 형식에는 창 이벤트 처리를 시작 하는 [**CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher.processevents) 메서드가 특히 중요 한 메서드가 있습니다. 앱에 대해 잘못 된 옵션을 사용 하 여이 메서드를 호출 하면 모든 종류의 예기치 않은 이벤트 처리 동작이 발생할 수 있습니다.

| CoreProcessEventsOption 옵션 | 설명 |
|--------------------------------|-------------|
| [**CoreProcessEventsOption 보류 중**](/uwp/api/Windows.UI.Core.CoreProcessEventsOption) | 큐에서 현재 사용할 수 있는 모든 이벤트를 디스패치합니다. 보류 중인 이벤트가 없으면 다음 새 이벤트를 기다립니다. |
| [**CoreProcessEventsOption를 표시 합니다.**](/uwp/api/Windows.UI.Core.CoreProcessEventsOption) | 큐에서 보류 중인 경우 하나의 이벤트를 디스패치합니다. 보류 중인 이벤트가 없으면 새 이벤트를 발생 시킬 때까지 기다리지 않고 즉시 반환 합니다. |
| [**CoreProcessEventsOption.ProcessUntilQuit**](/uwp/api/Windows.UI.Core.CoreProcessEventsOption) | 새 이벤트를 기다리고 사용 가능한 모든 이벤트를 디스패치합니다. 창이 닫히거나 응용 프로그램이 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 인스턴스에서 [**Close**](/uwp/api/windows.ui.core.corewindow.close) 메서드를 호출할 때까지이 동작을 계속 합니다. |
| [**CoreProcessEventsOption.ProcessAllIfPresent**](/uwp/api/Windows.UI.Core.CoreProcessEventsOption) | 큐에서 현재 사용할 수 있는 모든 이벤트를 디스패치합니다. 보류 중인 이벤트가 없으면 즉시 반환 합니다. |

DirectX를 사용 하는 UWP에서는 [**CoreProcessEventsOption. ProcessAllIfPresent**](/uwp/api/Windows.UI.Core.CoreProcessEventsOption) 옵션을 사용 하 여 그래픽 업데이트를 방해할 수 있는 차단 동작을 방지 해야 합니다.

## <a name="asta-considerations-for-directx-devs"></a>DirectX 개발자에 대 한 ASTA 고려 사항

응용 프로그램의 런타임 표현을 정의 하는 app 개체는 ASTA (Application Single-Threaded 아파트) 라는 스레딩 모델을 사용 하 여 앱의 UI 뷰를 호스팅합니다. Uwp 및 DirectX 앱을 개발 하는 경우에는 ASTA의 속성에 대해 잘 알고 있습니다. UWP 및 DirectX 앱에서 디스패치 한 스레드는 [**Windows:: System:: 스레딩**](/uwp/api/Windows.System.Threading) api를 사용 해야 합니다. 또는 [**CoreWindow:: CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher)를 사용 해야 합니다. 앱에서 [**CoreWindow:: GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread) 를 호출 하 여 ASTA에 대 한 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 개체를 가져올 수 있습니다.

UWP DirectX 앱의 개발자는 알아야 할 가장 중요 한 사항은 **main ()** 에서 **Platform:: MTAThread** 를 설정 하 여 앱 스레드가 MTA 스레드를 디스패치할 수 있도록 설정 해야 한다는 것입니다.

```cpp
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto myDXAppSource = ref new MyDXAppSource(); // your view provider factory 
    CoreApplication::Run(myDXAppSource);
    return 0;
}
```

UWP DirectX 앱에 대 한 앱 개체가 활성화 되 면 UI 보기에 사용할 ASTA가 만들어집니다. 새 ASTA 스레드는 사용자의 뷰 공급자 팩터리를 호출 하 여 앱 개체에 대 한 뷰 공급자를 만들기 때문에 해당 ASTA 스레드에서 뷰 공급자 코드가 실행 됩니다.

또한 ASTA에서 스핀 한 모든 스레드는 MTA에 있어야 합니다. 사용자가 스핀 한 모든 MTA 스레드는 계속 해 서 재진입 문제를 생성 하 여 교착 상태를 발생 시킬 수 있습니다.

기존 코드를 ASTA 스레드에서 실행되도록 이식하려는 경우 다음 고려 사항에 주의해야 합니다.

-   [**CoWaitForMultipleObjects**](/windows/win32/api/combaseapi/nf-combaseapi-cowaitformultipleobjects)와 같은 Wait 기본 형식은 STA 에서보다 다르게 동작 합니다.
-   COM 호출 모달 루프는 ASTA에서 다르게 작동 합니다. 나가는 호출이 진행 중인 동안에는 더 이상 관련 되지 않은 호출을 받을 수 없습니다. 예를 들어 다음 동작은 ASTA에서 교착 상태를 만들고 앱을 즉시 중단 합니다.
    1.  ASTA는 MTA 개체를 호출 하 고 인터페이스 포인터 P1을 전달 합니다.
    2.  ASTA는 나중에 동일한 MTA 개체를 호출 합니다. MTA 개체는 ASTA에 반환 되기 전에 p 1을 호출 합니다.
    3.  P1은 관련이 없는 호출을 수행 하는 것이 차단 되므로 ASTA에 입력할 수 없습니다. 그러나 MTA 스레드는 p 1에 대 한 호출을 시도 하는 동안 차단 됩니다.

    이 문제는 다음을 통해 해결할 수 있습니다.
    -   병렬 패턴 라이브러리 (Ppltasks.h)에 정의 된 **비동기** 패턴 사용
    -   [**CoreDispatcher:P:**](/uwp/api/windows.ui.core.coredispatcher.processevents) 를 호출 하면 임의의 호출을 허용 하기 위해 앱의 ASTA (응용 프로그램의 주 스레드)에서 rocessevents를 호출 하는 것이 가능 합니다.

    즉, 앱의 ASTA에 대 한 관련이 없는 호출을 즉시 전달 하는 데 의존할 수 없습니다. 비동기 호출에 대 한 자세한 내용은 [c + +의 비동기 프로그래밍](../threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps.md)을 참조 하세요.

전체적으로 UWP 앱을 디자인할 때 앱의 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 및 [**CoreDispatcher::P Rocessevents**](/uwp/api/windows.ui.core.coredispatcher.processevents) 에 대해 [**COREDISPATCHER**](/uwp/api/Windows.UI.Core.CoreDispatcher) 를 사용 하 여 MTA 스레드를 직접 만들고 관리 하는 대신 모든 UI 스레드를 처리 합니다. **CoreDispatcher** 를 사용 하 여 처리할 수 없는 별도의 스레드가 필요 하면 비동기 패턴을 사용 하 고 앞에서 설명한 지침에 따라 재진입 문제를 방지 합니다.