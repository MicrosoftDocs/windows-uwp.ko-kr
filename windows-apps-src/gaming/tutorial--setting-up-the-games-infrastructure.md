---
title: 게임 프로젝트 설정
description: 게임을 개발 하는 첫 번째 단계는 Microsoft Visual Studio에서 프로젝트를 설정 하는 것입니다. 게임 개발을 위해 특별히 프로젝트를 구성한 후 나중에 해당 프로젝트를 일종의 템플릿으로 다시 사용할 수 있습니다.
ms.assetid: 9fde90b3-bf79-bcb3-03b6-d38ab85803f2
ms.date: 06/24/2020
ms.topic: article
keywords: windows 10, uwp, 게임, 설정, directx
ms.localizationpriority: medium
ms.openlocfilehash: 86c7b80ba7125547c2a45dae434c40a67a758b0d
ms.sourcegitcommit: 8bface2162e091999b1cf2218340edda2389da89
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/16/2021
ms.locfileid: "103496700"
---
# <a name="set-up-the-game-project"></a>게임 프로젝트 설정

> [!NOTE]
> 이 항목은 DirectX 자습서 시리즈 [를 사용 하 여 단순 유니버설 Windows 플랫폼 (UWP) 게임 만들기](tutorial--create-your-first-uwp-directx-game.md) 의 일부입니다. 해당 링크의 항목은 계열의 컨텍스트를 설정 합니다.

게임을 개발 하는 첫 번째 단계는 Microsoft Visual Studio에서 프로젝트를 만드는 것입니다. 게임 개발을 위해 특별히 프로젝트를 구성한 후 나중에 해당 프로젝트를 일종의 템플릿으로 다시 사용할 수 있습니다.

## <a name="objectives"></a>목표

* 프로젝트 템플릿을 사용 하 여 Visual Studio에서 새 프로젝트를 만듭니다.
* **앱** 클래스에 대 한 소스 파일을 검사 하 여 게임의 진입점 및 초기화를 이해 합니다.
* 게임 루프를 확인 합니다.
* 프로젝트의 **appxmanifest.xml** 파일을 검토 합니다.

## <a name="create-a-new-project-in-visual-studio"></a>Visual Studio에서 새 프로젝트 만들기

> [!NOTE]
> &mdash;프로젝트 템플릿 및 빌드 지원을 함께 제공하는 C++/WinRT Visual Studio 확장(VSIX) 및 NuGet 패키지를 설치하고 사용하는 방법을 포함&mdash;하는 C++/WinRT용 Visual Studio 개발 설정에 대한 자세한 내용은 [Visual Studio의 C++/WinRT 지원](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)을 참조하세요.

먼저 최신 버전의 c + +/WinRT Visual Studio Extension (VSIX)을 설치 (또는 업데이트) 합니다. 위의 메모를 참조 하세요. 그런 다음 Visual Studio에서 **핵심 앱 (c + +/WinRT)** 프로젝트 템플릿을 기반으로 새 프로젝트를 만듭니다. 일반적으로 사용 가능한 최신(미리 보기 아님) 버전의 Windows SDK를 대상으로 합니다.

## <a name="review-the-app-class-to-understand-iframeworkviewsource-and-iframeworkview"></a>**App** 클래스를 검토 하 여 **IFrameworkViewSource** 및 **IFrameworkView** 이해

핵심 앱 프로젝트에서 소스 코드 파일을 엽니다 `App.cpp` . 에는 앱 및 해당 수명 주기를 나타내는 **app** 클래스가 구현 되어 있습니다. 물론이 경우 앱이 게임 임을 알 수 있습니다. 그러나 UWP (유니버설 Windows 플랫폼) 앱의 초기화 방법에 대해 보다 일반적으로 이야기 하기 위해이를 *앱* 이라고 합니다.

### <a name="the-wwinmain-function"></a>WWinMain 함수

**Wwinmain** 함수는 앱의 진입점입니다. 의 **Wwinmain** 모양은 다음과 같습니다 `App.cpp` .

```cppwinrt
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(winrt::make<App>());
}
```

**App** 클래스의 인스턴스를 만들고 (생성 된 **앱** 의 인스턴스인 경우에만), 정적 [**CoreApplication**](/uwp/api/windows.applicationmodel.core.coreapplication.run) 메서드에이를 전달 합니다. **CoreApplication** 에는 [**IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) 인터페이스가 필요 합니다. 따라서 **앱** 클래스에서 해당 인터페이스를 구현 해야 합니다.

이 항목의 다음 두 섹션에서는 [**IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) 및 [**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) 인터페이스에 대해 설명 합니다. 이러한 인터페이스 (및 **CoreApplication**)는 앱이 *보기-공급자* 를 사용 하 여 Windows를 제공 하는 방법을 나타냅니다. Windows에서는 응용 프로그램 수명 주기 이벤트를 처리할 수 있도록 해당 뷰 공급자를 사용 하 여 응용 프로그램을 Windows 셸에 연결 합니다.

### <a name="the-iframeworkviewsource-interface"></a>IFrameworkViewSource 인터페이스

**App** 클래스는 아래 목록에서 볼 수 있는 것 처럼 [**IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource)을 구현 합니다.

```cppwinrt
struct App : winrt::implements<App, IFrameworkViewSource, IFrameworkView>
{
    ...
    IFrameworkView CreateView()
    {
        return *this;
    }
    ...
}
```

**IFrameworkViewSource** 를 구현 하는 개체는 *뷰 공급자 팩터리* 개체입니다. 이 개체의 작업은 *뷰 공급자* 개체를 제조 하 고 반환 하는 것입니다.

**IFrameworkViewSource** 에는 [**IFrameworkViewSource:: createview**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource.createview)의 단일 메서드가 있습니다. Windows에서는 **CoreApplication** 에 전달 하는 개체에서 해당 함수를 호출 합니다. 위에서 볼 수 있듯이, 해당 메서드의 **App:: CreateView** 구현은을 반환 합니다 `*this` . 즉, **앱** 개체가 자신을 반환 합니다. **IFrameworkViewSource:: CreateView** 에는 [**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource)의 반환 값 형식이 있으므로 **App** 클래스에서 *해당* 인터페이스를 구현 해야 합니다. 위의 목록에 있는 것을 볼 수 있습니다.

### <a name="the-iframeworkview-interface"></a>IFrameworkView 인터페이스

[**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) 를 구현 하는 개체는 *뷰 공급자* 개체입니다. 이제 해당 뷰 공급자를 사용 하 여 Windows를 제공 했습니다. **Wwinmain** 에서 만든 것과 동일한 **앱** 개체입니다. 따라서 **앱** 클래스는 *뷰 공급자 팩터리* 및 *뷰 공급자로* 사용 됩니다.

이제 Windows에서 **IFrameworkView** 의 메서드 구현을 **앱** 클래스에서 호출할 수 있습니다. 이러한 메서드를 구현할 때 앱은 초기화와 같은 작업을 수행 하 여 필요한 리소스를 로드 하 고, 적절 한 이벤트 처리기를 연결 하 고, 앱에서 출력을 표시 하는 데 사용 하는 [**CoreWindow**](/uwp/api/windows.ui.core.corewindow) 을 받을 수 있습니다.

**IFrameworkView** 의 메서드 구현은 아래에 표시 된 순서 대로 호출 됩니다.

- [**초기화**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize)
- [**SetWindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow)
- [**로드**](/uwp/api/windows.applicationmodel.core.iframeworkview.load)
- [**CoreApplicationView:: 활성화**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) 된 이벤트가 발생 합니다. 따라서 해당 이벤트를 처리 하도록 등록 한 경우 (선택 사항) **Onactivated** 된 처리기가 현재 호출 됩니다.
- [**실행할지**](/uwp/api/windows.applicationmodel.core.iframeworkview.run)
- [**묵시적**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize)

그리고 이러한 메서드의 시그니처를 보여 주는 **App** 클래스 (의)의 기본 구조는 다음과 같습니다 `App.cpp` .

```cppwinrt
struct App : winrt::implements<App, IFrameworkViewSource, IFrameworkView>
{
    ...
    void Initialize(Windows::ApplicationModel::CoreCoreApplicationView const& applicationView) { ... }
    void SetWindow(Windows::UI::Core::CoreWindow const& window) { ... }
    void Load(winrt::hstring const& entryPoint) { ... }
    void OnActivated(
        Windows::ApplicationModel::CoreCoreApplicationView const& applicationView,
        Windows::ApplicationModel::Activation::IActivatedEventArgs const& args) { ... }
    void Run() { ... }
    void Uninitialize() { ... }
    ...
}
```

이는 **IFrameworkView** 를 소개 하기 위한 것입니다. 이러한 메서드에 대 한 자세한 내용 및 구현 방법에 대 한 자세한 내용은 [게임의 UWP 앱 프레임 워크 정의](tutorial--building-the-games-uwp-app-framework.md)를 참조 하세요.

### <a name="tidy-up-the-project"></a>프로젝트 정리

프로젝트 템플릿에서 만든 핵심 앱 프로젝트에는이 시점에서 정리 해야 하는 기능이 포함 되어 있습니다. 그런 다음 프로젝트를 사용 하 여 촬영 갤러리 게임 (**Simple3DGameDX**)을 다시 만들 수 있습니다. 에서 **App** 클래스를 다음과 같이 변경 합니다 `App.cpp` .

- 해당 데이터 멤버를 삭제 합니다.
- **Onpointerpressed**, **onpointerpressed** 및 **addvisual 개체** 삭제
- **Setwindow** 에서 코드를 삭제 합니다.

프로젝트를 빌드하고 실행 하지만 클라이언트 영역에 단색만 표시 됩니다.

## <a name="the-game-loop"></a>게임 루프

게임 루프의 모양을 파악 하려면 다운로드 한 [Simple3DGameDX](/samples/microsoft/windows-universal-samples/simple3dgamedx/) 샘플 게임의 소스 코드를 확인 합니다.

**App** 클래스에는 **GameMain** 형식의 *m_main* 이라는 데이터 멤버가 있습니다. 그리고 해당 멤버는 다음과 같이 **App:: Run** 에서 사용 됩니다.

```cppwinrt
void Run()
{
    m_main->Run();
}
```

에서 **GameMain:: Run** 을 찾을 수 있습니다 `GameMain.cpp` . 이는 게임의 주요 루프 이며, 다음은 가장 중요 한 기능을 보여 주는 대략적인 개요입니다.

```cppwinrt
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
            Update();
            m_renderer->Render();
            m_deviceResources->Present();
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

이 주요 게임 루프에서 수행 하는 작업에 대 한 간략 한 설명은 다음과 같습니다.

게임 창이 닫혀 있지 않으면 모든 이벤트를 디스패치 하 고 타이머를 업데이트 한 다음 그래픽 파이프라인의 결과를 렌더링 하 고 표시 합니다. 이러한 문제에 대해 훨씬 더 많은 정보를 제공 하 고, [게임의 UWP 앱 프레임 워크](tutorial--building-the-games-uwp-app-framework.md), [렌더링 프레임](tutorial--assembling-the-rendering-pipeline.md)워크, 렌더링 소개 및 [렌더링 프레임 워크 II: 게임 렌더링](tutorial-game-rendering.md)항목에서이 작업을 수행 합니다. 그러나 UWP DirectX 게임의 기본 코드 구조입니다.

## <a name="review-and-update-the-packageappxmanifest-file"></a>Appxmanifest.xml 파일 검토 및 업데이트

**Appxmanifest.xml** 파일에는 UWP 프로젝트에 대 한 메타 데이터가 포함 됩니다. 이러한 메타 데이터는 게임을 패키지 하 고 시작 하며 Microsoft Store에 제출할 때 사용 됩니다. 이 파일에는 플레이어 시스템에서 게임을 실행 하는 데 필요한 시스템 리소스에 대 한 액세스를 제공 하는 데 사용 하는 중요 정보도 포함 되어 있습니다.

**솔루션 탐색기** 에서 **appxmanifest.xml** 파일을 두 번 클릭 하 여 **매니페스트 디자이너** 를 시작 합니다.

![패키지 appx 매니페스트 편집기의 스크린샷](images/simple-dx-game-setup-app-manifest.png)

**Appxmanifest.xml** 파일 및 패키지에 대 한 자세한 내용은 [매니페스트 디자이너](/previous-versions/br230259(v=vs.140))를 참조 하세요. 지금은 **기능** 탭을 살펴보고 제공 된 옵션을 확인 합니다.

![direct3d 앱의 기본 기능을 사용 하는 스크린샷](images/simple-dx-game-setup-capabilities.png)

게임에서 사용 하는 기능을 선택 하지 않으면 (예: 전 세계 최고 점수 보드를 위한 **인터넷** 액세스) 해당 하는 리소스 또는 기능에 액세스할 수 없습니다. 새 게임을 만들 때 게임에서 호출 하는 Api에 필요한 기능을 선택 했는지 확인 합니다.

이제 **DirectX 11 앱 (유니버설 Windows)** 템플릿과 함께 제공 되는 파일의 나머지 부분을 살펴보겠습니다.

## <a name="review-other-important-libraries-and-source-code-files"></a>다른 중요 한 라이브러리 및 소스 코드 파일 검토

사용자를 위해 특정 게임 프로젝트 템플릿을 만들려는 경우 나중에 프로젝트를 시작 점으로 다시 사용할 수 있도록 하려면 다운로드 한 Simple3DGameDX 프로젝트를 복사 하 여 `GameMain.h` `GameMain.cpp` 외부에 추가 하 고 새 핵심 앱 프로젝트에 추가 합니다. [](/samples/microsoft/windows-universal-samples/simple3dgamedx/) 이러한 파일을 연구 하 고, 수행 하는 작업을 알아보고, **Simple3DGameDX** 관련 된 모든 항목을 제거 합니다. 또한 아직 복사 하지 않은 코드에 의존 하는 모든 항목을 주석으로 처리 합니다. 예를 들어,는 `GameMain.h` 에 따라 결정 `GameRenderer.h` 됩니다. **Simple3DGameDX** 에서 더 많은 파일을 복사 하면 주석 처리를 해제할 수 있습니다.

다음은 템플릿에 포함 하는 데 유용 하 게 사용할 수 있는 **Simple3DGameDX** 의 일부 파일에 대 한 간단한 설문 조사입니다. 어떤 경우 든 **Simple3DGameDX** 자체가 작동 하는 방식을 이해 하는 것은 동일 합니다.

|원본 파일|파일 폴더|설명|
|------------------------------|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|DeviceResources/.cpp|유틸리티|모든 DirectX [장치 리소스](tutorial--assembling-the-rendering-pipeline.md#resource)를 제어 하는 **DeviceResources** 클래스를 정의 합니다. 는 또한 그래픽 어댑터 장치를 분실 하거나 다시 만들었는지 응용 프로그램에 알리는 데 사용 되는 **IDeviceNotify** 인터페이스를 정의 합니다.|
|DirectXSample .h|유틸리티|**ConvertDipsToPixels** 와 같은 도우미 함수를 구현 합니다. **ConvertDipsToPixels** 는 dip (장치 독립적 픽셀) 길이를 물리적 픽셀 길이로 변환 합니다.|
|GameTimer/.cpp|유틸리티|게임 또는 대화형 렌더링 앱에 유용한 고해상도 타이머를 정의 합니다.|
|GameRenderer/.cpp|렌더링|기본 렌더링 파이프라인을 구현 하는 **GameRenderer** 클래스를 정의 합니다.|
|GameHud/.cpp|렌더링|Direct2D 및 DirectWrite를 사용 하 여 게임의 HUD (헤드 표시)를 렌더링 하는 클래스를 정의 합니다.|
|VertexShader hlsl 및 VertexShaderFlat hlsl|셰이더|기본 꼭 짓 점 셰이더에 대 한 HLSL (high level shader language) 코드를 포함 합니다.|
|Shadereffect hlsl 및 PixelShaderFlat hlsl|셰이더|기본 픽셀 셰이더에 대 한 HLSL (high level shader language) 코드를 포함 합니다.|
|ConstantBuffers. .hlsli|셰이더|꼭 짓 점 셰이더에 MVP (모델-뷰-프로젝션) 매트릭스 및 꼭 짓 점 데이터를 전달 하는 데 사용 되는 상수 버퍼 및 셰이더 구조에 대 한 데이터 구조 정의를 포함 합니다.|
|.pch. h/.cpp|해당 없음|일반적인 c + +/WinRT, Windows 및 DirectX 포함을 포함 합니다.| 

### <a name="next-steps"></a>다음 단계

이 시점에서 DirectX 게임에 대 한 새 UWP 프로젝트를 만들고, 일부 부분을 살펴보고, 해당 프로젝트를 게임에 사용할 수 있는 일종의 템플릿으로 전환 하는 방법을 고려 하는 방법을 살펴보았습니다. 또한 **Simple3DGameDX** 샘플 게임의 중요 한 부분을 살펴보았습니다.

다음 섹션에서는 [게임의 UWP 앱 프레임 워크를 정의](tutorial--building-the-games-uwp-app-framework.md)합니다. **Simple3DGameDX** 작동 방식에 대해 자세히 살펴보겠습니다.
