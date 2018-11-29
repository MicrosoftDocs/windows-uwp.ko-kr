---
title: 게임 루프 포팅
description: IFrameworkView를 작성하여 전체 화면 CoreWindow를 제어하는 방법을 포함하여 UWP(유니버설 Windows 플랫폼) 게임의 창을 구현하는 방법 및 게임 루프로 가져오는 방법을 보여 줍니다.
ms.assetid: 070dd802-cb27-4672-12ba-a7f036ff495c
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 포팅, 게임 루프, direct3d 9, directx 11
ms.localizationpriority: medium
ms.openlocfilehash: 8b0cf6352d400371b54a54d71176c4d8e1dc457d
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7975618"
---
# <a name="port-the-game-loop"></a>게임 루프 포팅



**요약**

-   [1부: Direct3D 11 초기화](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)
-   [2부: 렌더링 프레임워크 변환](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   3부: 게임 루프 포팅


[**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478)를 작성하여 전체 화면 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)를 제어하는 방법을 포함하여 UWP(유니버설 Windows 플랫폼) 게임의 창을 구현하는 방법 및 게임 루프로 가져오는 방법을 보여 줍니다. [DirectX 11 및 UWP로 간단한 Direct3D 9 앱 포팅](walkthrough--simple-port-from-direct3d-9-to-11-1.md) 연습의 3부.

## <a name="create-a-window"></a>창 만들기


Direct3D 9 뷰포트를 사용하여 바탕 화면 창을 설정하려면 데스크톱 앱에 대한 기존의 앱 프레임 워크를 구현해야 했습니다. HWND를 만들고, 창 크기를 설정하고, 창 처리 콜백을 제공하고, 이 창이 표시되게 하는 등의 작업을 수행해야 했습니다.

UWP 환경에는 훨씬 간단한 시스템이 있습니다. 기존의 창을 설정하는 대신 DirectX를 사용하는 Microsoft Store 게임은 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478)를 구현합니다. DirectX 앱과 게임이 앱 컨테이너 내 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)에서 직접 실행되도록 하기 위해 이 인터페이스가 존재합니다.

> **참고**  Windows는 원본 응용 프로그램 개체 및 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)등의 리소스에 관리 되는 포인터를 제공 합니다. [**개체 연산자 (^)에 대 한 핸들**]를 참조 하세요.https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx.

 

"기본" 클래스는 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478)에서 상속하고 5개 **IFrameworkView** 메서드([**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495), [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509), [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501), [**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 및 [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523))를 구현해야 합니다. (기본적으로) 게임에 있는 위치인 **IFrameworkView**를 만들 뿐만 아니라, **IFrameworkView**의 인스턴스를 만드는 팩터리 클래스를 구현해야 합니다. 게임에는 여전히 **main()** 이라는 메서드가 있지만, 기본 클래스가 수행할 수 있는 모든 것은 팩터리를 사용하여 **IFrameworkView** 인스턴스를 만드는 것입니다.

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

## <a name="port-the-game-loop"></a>게임 루프 포팅


Direct3D 9 구현에서 게임 루프를 살펴보겠습니다. 이 코드는 앱의 main 함수에 존재합니다. 이 루프의 각 반복에서는 창 메시지를 처리하거나 프레임을 렌더링합니다.

Direct3D 9 데스크톱 게임의 게임 루프

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

게임 루프는 비슷하지만 UWP 버전의 해당 게임에서 더 간편합니다.

게임이 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) 클래스 내에서 작동하기 때문에 게임 루프가 [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 메서드(**main()** 대신)에서 이동합니다.

메시지 처리 프레임워크를 구현하고 [**PeekMessage**](https://msdn.microsoft.com/library/windows/desktop/ms644943)를 호출하는 대신 앱 창의 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)에 내장된 [**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) 메서드를 호출할 수 있습니다. 메시지를 분기 및 처리하기 위해 게임 루프가 필요하지 않습니다. **ProcessEvents**를 처리하고 진행하기만 하면 됩니다.

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

이제 같은 기본 그래픽 인프라를 설정하는 UWP 앱이 있고 DirectX 9 예제에서와 같은 다채로운 색상의 큐브를 렌더링합니다.

## <a name="where-do-i-go-from-here"></a>여기에서 이동할 위치


[DirectX 11 포팅 FAQ](directx-porting-faq.md)를 책갈피로 지정합니다.

DirectX UWP 템플릿에는 UWP 게임에 사용할 수 있는 강력한 Direct3D 장치 인프라가 포함되어 있습니다. 올바른 템플릿 선택에 대한 지침은 [템플릿에서 DirectX 게임 프로젝트 만들기](user-interface.md)를 참조하세요.

다음과 같이 심층적인 Microsoft Store 게임 개발 문서를 참조하세요.

-   [연습: DirectX로 간단한 UWP 게임 만들기](tutorial--create-your-first-uwp-directx-game.md)
-   [게임용 오디오](working-with-audio-in-your-directx-game.md)
-   [게임용 이동-보기 컨트롤](tutorial--adding-move-look-controls-to-your-directx-game.md)

 

 




