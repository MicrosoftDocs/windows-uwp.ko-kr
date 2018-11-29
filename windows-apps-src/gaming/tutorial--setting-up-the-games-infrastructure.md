---
title: 게임 프로젝트 설정
description: 게임을 어셈블하는 첫 번째 단계는 Microsoft Visual Studio에서 수행해야 하는 코드 인프라 작업의 양을 최소화하는 방식으로 프로젝트를 설정하는 것입니다.
ms.assetid: 9fde90b3-bf79-bcb3-03b6-d38ab85803f2
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 설정, directx
ms.localizationpriority: medium
ms.openlocfilehash: 252d7ccb8e50e773a19282afaf19bb18d4c5d5a6
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8198392"
---
# <a name="set-up-the-game-project"></a>게임 프로젝트 설정

이 항목에서는 Visual Studio에서 템플릿을 사용하여 간단한 UWP DirectX 게임을 설정하는 방법을 알아봅니다. 게임을 어셈블하는 첫 번째 단계는 Microsoft Visual Studio에서 수행해야 하는 코드 인프라 작업의 양을 최소화하는 방식으로 프로젝트를 설정하는 것입니다. 올바른 템플릿을 사용하고 특히 게임 개발을 위해 이 프로젝트를 구성하여 설정 시간을 줄이는 방법을 알아보세요.

## <a name="objectives"></a>목표

* Visual Studio에서 템플릿을 사용하여 Direct3D 게임 프로젝트를 설정합니다.
* **앱** 원본 파일을 검토하여 게임의 기본 진입점을 이해
* 프로젝트의 **package.appxmanifest** 파일 검토
* 이 프로젝트에 포함되어 있는 게임 개발 도구 및 지원을 확인

## <a name="how-to-set-up-the-game-project"></a>게임 프로젝트 설정 방법

UWP(유니버설 Windows 플랫폼) 개발이 처음인 경우에는 Visual Studio에서 템플릿을 사용하여 기본 코드 구조를 설정하는 것이 좋습니다.

>[!Note]
>이 문서는 게임 샘플 기반의 자습서 시리즈의 일부입니다. [Direct3D 게임 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)에서 최신 코드를 얻을 수 있습니다. 이 샘플은 UWP 기능 샘플의 큰 컬렉션의 일부입니다. 샘플을 다운로드하는 방법에 대한 지침은 [GitHub에서 UWP 샘플 가져오기](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)를 참조하세요.

### <a name="use-directx-template-to-create-a-project"></a>DirectX 템플릿을 사용하여 프로젝트 생성

Visual Studio 템플릿은 기본 언어 및 기술을 기반으로 하는 특정 앱 유형을 대상으로 하는 설정 및 코드 파일 모음입니다. Microsoft Visual Studio2017에서 다양 한 게임 및 그래픽 앱 개발을 상당히 줄일 수 있는 템플릿 찾을 수 있습니다. 템플릿을 사용하지 않을 경우 기본 그래픽 렌더링 및 디스플레이 프레임워크 대부분을 직접 개발해야 하는데, 초보 게임 개발자에게는 매우 힘들 일이 될 수 있습니다.

이 자습서에 사용된 템플릿은  **DirectX 11 앱(유니버설 Windows)** 이라는 템플릿입니다. 

Visual Studio에서 DirectX 11 게임 프로젝트를 만들기 위한 단계:
1.  **파일...** &gt; **신규**  &gt; **프로젝트...** 를 선택
2.  왼쪽 창에서 **설치 완료** &gt; **템플릿** &gt; **Visual C++** &gt; **Windows 유니버설**을 선택
3.  가운데 창에서 **DirectX 11 앱(유니버설 Windows)** 을 선택
4.  게임 프로젝트에 이름을 지정하고 **확인**을 클릭합니다.

![새 게임 프로젝트를 만들기 위해 directx11 템플릿을 선택하는 방법을 보여주는 스크린샷](images/simple-dx-game-setup-new-project.png)

이 템플릿은 C++로 작성된 DirectX를 사용하는 UWP 앱의 기본 프레임워크를 제공합니다. F5 키를 눌러 앱을 빌드 및 실행합니다. 옅은 청회색 화면을 확인합니다. 이 템플릿은 DirectX 및 C++를 사용하는 UWP 앱의 기본 기능이 포함된 여러 코드 파일을 만듭니다.

## <a name="review-the-apps-main-entry-point-by-understanding-iframeworkview"></a>IFrameworkView를 이해하여 앱의 기본 진입점 검토

**앱** 클래스는 **IFrameworkView** 클래스에서 상속됩니다.

### <a name="inspect-apph"></a>**App.h**를 검사합니다.

뷰 공급자를 정의하는 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700469) 인터페이스를 구현할 때 [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495), [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509), [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501), [**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505), [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523) 등 **App.h**의 5가지 메서드를 간략하게 살펴보겠습니다. 이러한 메서드는 게임이 시작될 때 만들어지는 앱 단일 항목에서 실행되며 앱의 모든 리소스를 로드할 뿐만 아니라 적절한 이벤트 처리기를 연결합니다.

```cpp
    // Main entry point for our app. Connects the app with the Windows shell and handle application lifecycle events.
    ref class App sealed : public Windows::ApplicationModel::Core::IFrameworkView
    {
    public:
        App();

        // IFrameworkView Methods.
        virtual void Initialize(Windows::ApplicationModel::Core::CoreApplicationView^ applicationView);
        virtual void SetWindow(Windows::UI::Core::CoreWindow^ window);
        virtual void Load(Platform::String^ entryPoint);
        virtual void Run();
        virtual void Uninitialize();

    protected:
        ...
    };
```

### <a name="inspect-appcpp"></a>**App.cpp** 검사

**App.cpp** 원본 파일의 **main** 메서드는 다음과 같습니다.

```cpp
//The main function is only used to initialize our IFrameworkView class.
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}
```

이 메서드에서 뷰 공급자 팩터리(**App.h**에 정의된**Direct3DApplicationSource**)에서 Direct3D 뷰 공급자의 인스턴스를 만들고 ([**CoreApplication::Run**](https://msdn.microsoft.com/library/windows/apps/hh700469))을 호출하여 이를 앱 단일 항목에 전달합니다. 즉, 게임의 시작 지점은 구현된 [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 메서드의 본문에 있습니다(이 경우 **App::Run**). 

스크롤하여 **App.cpp**에서 **App::Run** 메서드를 찾습니다. 코드는 다음과 같습니다.

```cpp
//This method is called after the window becomes active.
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

이 메서드는 게임 창이 닫혀 있지 않으면 모든 이벤트를 디스패치하고 타이머를 업데이트한 다음, 그래픽 파이프라인의 결과를 렌더링 및 표현합니다. 이에 대한 자세한 내용은 [UWP 앱 프레임워크 정의](tutorial--building-the-games-uwp-app-framework.md), [렌더링 프레임워크 I: 렌더링 소개](tutorial--assembling-the-rendering-pipeline.md) 및  [렌더링 프레임워크 II: 게임 렌더링](tutorial-game-rendering.md)에서 살펴보겠습니다. 지금은 UWP DirectX 게임의 기본 코드 구조를 알아보아야 합니다.

## <a name="review-and-update-the-packageappxmanifest-file"></a>package.appxmanifest 파일을 검토 및 업데이트


코드 파일이 템플릿과 관련된 전부가 아닙니다. **Package.appxmanifest** 파일에는 게임 패키징 및 론칭과 Microsoft Store로의 제출에 사용되는 프로젝트에 대한 메타데이터가 포함되어 있습니다. 또한 플레이어의 시스템에서 게임에서 실행되어야 하는 시스템 리소스에 액세스하는 데 사용하는 중요한 정보가 포함되어 있습니다.

**솔루션 탐색기**에서 **Package.appxmanifest** 파일을 두 번 클릭하여 **매니페이트 디자이너**를 시작합니다.

![package.appx 매니페스트 편집기의 스크린샷입니다.](images/simple-dx-game-setup-app-manifest.png)

**package.appxmanifest** 파일 및 패키징에 대한 자세한 내용은 [매니페스트 디자이너](https://msdn.microsoft.com/library/windows/apps/br230259.aspx)를 참조하세요. 우선, **기능** 탭과 제공되는 옵션들에 대해 살펴보겠습니다.

![Direct3D 앱의 기본 기능에 대한 스크린샷입니다.](images/simple-dx-game-setup-capabilities.png)

전역 최고 점수 보드를 위한 **인터넷** 액세스 같이 게임에서 사용되는 기능을 선택하지 않으면 해당 리소스나 기능에 액세스할 수 없습니다. 새 게임을 만들 때는 게임에서 실행되어야 하는 기능을 선택해야 합니다.

이제, **DirectX 11 앱(유니버설 Windows)** 템플릿과 함께 제공되는 나저미 파일들을 살펴보겠습니다.

## <a name="review-the-included-libraries-and-headers"></a>포함된 라이브러리 및 헤더를 검토합니다.

아직 살펴보지 않은 몇 개의 파일이 있습니다. 이 파일은 Direct3D 게임 개발 시나리오에 공통적으로 적용되는 추가 도구 및 지원을 제공합니다. 기본 DirectX 게임 프로젝트와 함께 제공되는 전체 파일 목록은 [DirectX 게임 프로젝트 템플릿](user-interface.md#template-structure)을 참조하세요.

| 템플릿 원본 파일         | 파일 폴더            | 설명 |
|------------------------------|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DeviceResources.h/.cpp       | 일반                 | 모든 DirectX [디바이스 리소스](tutorial--assembling-the-rendering-pipeline.md#resource)를 제어하는 클래스 개체를 정의합니다. 여기에는 디바이스를 분실하거나 만들 때 알림이 제공되는 DeviceResources 소유 응용 프로그램의 인터페이스도 포함됩니다.                                                |
| DirectXHelper.h              | 일반                 | **DX::ThrowIfFailed**, **ReadDataAsync** 및 **ConvertDipsToPixels 같은 메서드를 구현합니다. **DX::ThrowIfFailed**는 DirectX Win32 API에서 반환된 오류 HRESULT 값을 Windows 런타임 예외로 변환합니다. 이 메서드를 사용하여 DirectX 오류를 디버깅하기 위한 중단점을 배치합니다. 자세한 내용은 [ThrowIfFailed](https://github.com/Microsoft/DirectXTK/wiki/ThrowIfFailed)를 참조하세요. **ReadDataAsync**는 이진 파일을 비동기적으로 읽습니다. **ConvertDipsToPixels**는 장치 독립적인 픽셀(DIP)의 길이를 실제 픽셀의 길이로 변환합니다. |
| StepTimer.h                  | 일반                 | 게임 또는 대화형 렌더링 앱에 유용한 고해상도 타이머를 정의합니다.   |
| Sample3DSceneRenderer.h/.cpp | 내용                | 기본 렌더링 파이프라인을 인스턴스화하도록 클래스 개체를 정의합니다. 이렇게 하면 DirectX를 사용하여 Direct3D 스왑 체인 및 그래픽 어댑터를 UWP에 연결하는 기본적인 렌더러가 구현됩니다.   |
| SampleFPSTextRenderer.h/.cpp | 내용                | Direct2D 및 DirectWrite를 사용하여 화면 오른쪽 아래에 있는 현재 초당 프레임(FPS) 값을 렌더링하도록 클래스 개체를 정의합니다.  |
| SamplePixelShader.hlsl       | 내용                | 기본 픽셀 셰이더에 대한 HLSL(High Level Shader Language) 코드가 들어 있습니다.                                            |
| SampleVertexShader.hlsl      | 내용                | 기본 꼭짓점 셰이더에 대한 HLSL(High Level Shader Language) 코드가 들어 있습니다.                                           |
| ShaderStructures.h           | 내용                | 꼭지점 셰이더에 MVP 매트릭스 및 꼭지점별 데이터를 보내는 데 사용할 수 있는 셰이더 구조가 포함되어 있습니다.  |
| pch.h/.cpp                   | 기본                   | DirectX 11 API를 포함하여 Direct3D 앱이 사용하는 API에 대한 모든 Windows 시스템 include가 들어 있습니다.| 

### <a name="next-steps"></a>다음 단계

지금까지 **DirectX 11 App (Universal Windows)** 템플릿을 사용하여 UWP DirectX 게임 프로젝트를 생성하는 방법을 알아보고 이 프로젝트에서 제공되는 몇 가지 구성 요소 및 파일도 살펴봤습니다.

다음 섹션은 [게임의 UWP 프레임워크 정의](tutorial--building-the-games-uwp-app-framework.md)입니다. 여기서는 이 게임에서 템플릿이 제공하는 다양한 개념과 구성 요소를 어떻게 사용 및 확장할 수 있는지 살펴보겠습니다.

 

 




