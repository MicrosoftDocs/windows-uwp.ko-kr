---
title: 게임 루프를 이식 합니다.
description: UWP (유니버설 Windows 플랫폼) 게임의 창을 구현 하는 방법 및 전체 화면 CoreWindow을 제어 하는 IFrameworkView를 작성 하는 방법을 비롯 하 여 게임 루프를 가져오는 방법을 보여 줍니다.
ms.assetid: 070dd802-cb27-4672-12ba-a7f036ff495c
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 포팅, 게임 루프, direct3d 9, directx 11
ms.localizationpriority: medium
ms.openlocfilehash: e62b7e6576ff1b39cbeba2c201f929952abd4105
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159097"
---
# <a name="port-the-game-loop"></a>게임 루프를 이식 합니다.



**요약**

-   [1 부: Direct3D 11 초기화](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)
-   [2 부: 렌더링 프레임 워크 변환](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   3 부: 게임 루프를 이식 합니다.


UWP (유니버설 Windows 플랫폼) 게임의 창을 구현 하는 방법 및 전체 화면 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)을 제어 하는 [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) 를 작성 하는 방법을 비롯 하 여 게임 루프를 가져오는 방법을 보여 줍니다. 포트의 3 부에서는 [간단한 Direct3D 9 앱을 DirectX 11 및 UWP 연습으로 만듭니다](walkthrough--simple-port-from-direct3d-9-to-11-1.md) .

## <a name="create-a-window"></a>창 만들기


Direct3D 9 뷰포트를 사용 하 여 데스크톱 창을 설정 하려면 데스크톱 앱에 대 한 기존 창 작업 프레임 워크를 구현 해야 했습니다. HWND를 만들고, 창 크기를 설정 하 고, 창 처리 콜백을 제공 하 고, 표시 하는 등의 작업을 수행 해야 했습니다.

UWP 환경의 시스템은 훨씬 더 간단 합니다. DirectX를 사용 하는 Microsoft Store 게임에서는 기존 창을 설정 하는 대신 [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView)를 구현 합니다. 이 인터페이스는 DirectX 앱 및 게임을 앱 컨테이너 내부의 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 에서 직접 실행 하는 데 사용할 수 있습니다.

> **참고**    Windows는 원본 응용 프로그램 개체 및 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)같은 리소스에 대 한 관리 되는 포인터를 제공 합니다. [**개체 연산자에 대 한 핸들 (^)을**](/cpp/extensions/handle-to-object-operator-hat-cpp-component-extensions)참조 하세요.

 

"Main" 클래스는 [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) 에서 상속 하 고 5 가지 **IFrameworkView** 메서드 (Initialize, [**setwindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow), [**Load**](/uwp/api/windows.applicationmodel.core.iframeworkview.load), [**Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run)및 [**Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize))를 구현 해야 [**합니다.**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize) **IFrameworkView**(기본적으로)를 만드는 것 외에도 **IFrameworkView**의 인스턴스를 만드는 팩터리 클래스를 구현 해야 합니다. 이 게임에는 **main ()** 메서드를 사용 하는 실행 파일이 여전히 있지만 모든 main은 팩터리를 사용 하 여 **IFrameworkView** 인스턴스를 만들 수 있습니다.

Main 함수

```cpp
//-----------------------------------------------------------------------------
// Required method for a DirectX-only app.
// The main function is only used to initialize the app's IFrameworkView class.
//-----------------------------------------------------------------------------
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}
```

IFrameworkView 팩터리

```cpp
//-----------------------------------------------------------------------------
// This class creates our IFrameworkView.
//-----------------------------------------------------------------------------
ref class Direct3DApplicationSource sealed : 
    Windows::ApplicationModel::Core::IFrameworkViewSource
{
public:
    virtual Windows::ApplicationModel::Core::IFrameworkView^ CreateView()
    {
        return ref new Cube11();
    };
};
```

## <a name="port-the-game-loop"></a>게임 루프를 이식 합니다.


Direct3D 9 구현의 게임 루프를 살펴보겠습니다. 이 코드는 응용 프로그램의 main 함수에 있습니다. 이 루프의 각 반복은 창 메시지를 처리 하거나 프레임을 렌더링 합니다.

Direct3D 9 desktop game의 게임 루프

```cpp
while(WM_QUIT != msg.message)
{
    // Process window events.
    // Use PeekMessage() so we can use idle time to render the scene. 
    bGotMsg = (PeekMessage(&msg, NULL, 0U, 0U, PM_REMOVE) != 0);

    if(bGotMsg)
    {
        // Translate and dispatch the message
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }
    else
    {
        // Render a new frame.
        // Render frames during idle time (when no messages are waiting).
        RenderFrame();
    }
}
```

게임 루프는 유사 하지만 UWP 버전의 게임에서 더 간편해 집니다.

게임 루프가 [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) 클래스 내에서 작동 하기 때문에 [**IFrameworkView:: Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) 메서드 ( **main ()** 대신)로 이동 합니다.

메시지 처리 프레임 워크를 구현 하 고 [**PeekMessage**](/windows/desktop/api/winuser/nf-winuser-peekmessagea)를 호출 하는 대신 앱 창의 [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher)에 기본 제공 되는 [**processevents**](/uwp/api/windows.ui.core.coredispatcher.processevents) 메서드를 호출할 수 있습니다. 게임 루프가 메시지를 분기 하 고 처리할 필요가 없습니다. **Processevents** 를 호출 하 고 계속 합니다.

Direct3D 11 Microsoft Store 게임의 게임 루프

```cpp
// UWP apps should not exit. Use app lifecycle events instead.
while (true)
{
    // Process window events.
    auto dispatcher = CoreWindow::GetForCurrentThread()->Dispatcher;
    dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

    // Render a new frame.
    RenderFrame();
}
```

이제 동일한 기본 그래픽 인프라를 설정 하는 UWP 앱이 있으며 DirectX 9 예제 처럼 동일한 다채로운 큐브를 렌더링 합니다.

## <a name="where-do-i-go-from-here"></a>여기에서 어떻게 하나요?


[DirectX 11 포팅 FAQ](directx-porting-faq.md)에 책갈피를 합니다.

DirectX UWP 템플릿에는 게임과 함께 사용할 준비가 된 강력한 Direct3D 장치 인프라가 포함 되어 있습니다. 올바른 템플릿을 선택 하는 방법에 대 한 지침은 [템플릿에서 DirectX 게임 프로젝트 만들기](user-interface.md) 를 참조 하세요.

다음 심층 Microsoft Store 게임 개발 문서를 참조 하세요.

-   [연습: DirectX를 사용 하는 간단한 UWP 게임](tutorial--create-your-first-uwp-directx-game.md)
-   [게임의 오디오](working-with-audio-in-your-directx-game.md)
-   [게임의 이동 컨트롤](tutorial--adding-move-look-controls-to-your-directx-game.md)

 

 