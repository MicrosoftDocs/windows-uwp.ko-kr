---
게임 프로젝트 설정
게임을 어셈블하는 첫 번째 단계는 Microsoft Visual Studio에서 수행해야 하는 코드 인프라 작업의 양을 최소화하는 방식으로 프로젝트를 설정하는 것입니다.
ms.assetid: 9fde90b3-bf79-bcb3-03b6-d38ab85803f2
---

# 게임 프로젝트 설정


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

게임을 어셈블하는 첫 번째 단계는 Microsoft Visual Studio에서 수행해야 하는 코드 인프라 작업의 양을 최소화하는 방식으로 프로젝트를 설정하는 것입니다. 적합한 템플릿을 사용하고 게임 개발에 맞게 특별히 프로젝트를 구성하면 시간을 절약하고 혼동을 줄일 수 있습니다. 여기서는 간단한 게임 프로젝트 설정 및 구성에 대한 단계별로 안내합니다.

## 목표


-   Visual Studio에서 Direct3D 게임 프로젝트를 설정하는 방법을 알아봅니다.

## 게임 프로젝트 설정


간편한 텍스트 편집기, 몇 가지 샘플 및 초보적인 수준의 지능만 있으면 처음부터 게임을 빌드할 수 있습니다. 그러나 시간 절약 측면에서는 가장 효과적인 방법이 아닐 수 있습니다. 또한 UWP(유니버설 Windows 플랫폼) 개발이 처음이라면 Visual Studio 사용이 부담스럽지 않을 리가 없습니다. 다음은 프로젝트를 활발하게 시작하기 위해 수행할 작업입니다.

## 1. 적합한 템플릿을 선택합니다.


Visual Studio 템플릿은 기본 언어 및 기술을 기반으로 하는 특정 앱 유형을 대상으로 하는 설정 및 코드 파일 모음입니다. Microsoft Visual Studio 2015에는 게임 및 그래픽 앱 개발을 상당히 쉽게 수행할 수 있는 많은 템플릿이 있습니다. 템플릿을 사용하지 않을 경우 기본 그래픽 렌더링 및 디스플레이 프레임워크 대부분을 직접 개발해야 하는데, 초보 게임 개발자에게는 매우 힘들 일이 될 수 있습니다.

이 자습서에 적합한 템플릿은 DirectX 11(유니버설 Windows)이라는 템플릿입니다. Visual Studio 2015에서 **파일...** &gt; **새 프로젝트**를 클릭하고 나서,

1.  **템플릿**에서 **Visual C++**, **Windows**, **유니버설**을 선택합니다.
2.  가운데 창에서 **DirectX 11 앱(유니버설 Windows)**을 선택합니다.
3.  게임 프로젝트에 이름을 지정하고 **확인**을 클릭합니다.

![direct3d 응용 프로그램 템플릿 선택](images/simple-dx-game-vs-new-proj.png)

이 템플릿은 C++로 작성된 DirectX를 사용하는 UWP 앱의 기본 프레임워크를 제공합니다. 계속해서 이 템플릿을 빌드하고 F5 키를 눌러 실행합니다. 옅은 청회색 화면을 확인합니다. 템플릿에서 제공하는 코드를 검토합니다. 이 템플릿은 DirectX 및 C++를 사용하는 UWP 앱의 기본 기능이 포함된 여러 코드 파일을 만듭니다. [3단계](#3-review-the-included-libraries-and-headers)에서 다른 코드 파일에 대해 추가로 설명합니다. 지금은 **App.h**를 빠르게 살펴보겠습니다.

```cpp
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
        // Application lifecycle event handlers.
        void OnActivated(Windows::ApplicationModel::Core::CoreApplicationView^ applicationView, Windows::ApplicationModel::Activation::IActivatedEventArgs^ args);
        void OnSuspending(Platform::Object^ sender, Windows::ApplicationModel::SuspendingEventArgs^ args);
        void OnResuming(Platform::Object^ sender, Platform::Object^ args);

        // Window event handlers.
        void OnWindowSizeChanged(Windows::UI::Core::CoreWindow^ sender, Windows::UI::Core::WindowSizeChangedEventArgs^ args);
        void OnVisibilityChanged(Windows::UI::Core::CoreWindow^ sender, Windows::UI::Core::VisibilityChangedEventArgs^ args);
        void OnWindowClosed(Windows::UI::Core::CoreWindow^ sender, Windows::UI::Core::CoreWindowEventArgs^ args);

        // DisplayInformation event handlers.
        void OnDpiChanged(Windows::Graphics::Display::DisplayInformation^ sender, Platform::Object^ args);
        void OnOrientationChanged(Windows::Graphics::Display::DisplayInformation^ sender, Platform::Object^ args);
        void OnDisplayContentsInvalidated(Windows::Graphics::Display::DisplayInformation^ sender, Platform::Object^ args);

    private:
        std::shared_ptr<DX::DeviceResources> m_deviceResources;
        std::unique_ptr<MyAwesomeGameMain> m_main;
        bool m_windowClosed;
        bool m_windowVisible;
    };
```

뷰 공급자를 정의하는 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700469) 인터페이스를 구현할 때 [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495), [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509), [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501), [**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 및 [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523)의 5가지 메서드를 만듭니다. 이러한 메서드는 게임이 시작될 때 만들어지는 앱 단일 항목에서 실행되며 앱의 모든 리소스를 로드할 뿐만 아니라 적절한 이벤트 처리기를 연결합니다.

**main** 메서드는 **App.cpp** 원본 파일에 있으며, 다음과 같이 표시됩니다.

```cpp
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}
```

지금은 뷰 공급자 팩터리(**App.h**에 정의된 **Direct3DApplicationSource**)에서 Direct3D 뷰 공급자의 인스턴스를 만들고 앱 단일 항목에 전달하여 실행합니다([**CoreApplication::Run**](https://msdn.microsoft.com/library/windows/apps/hh700469)). 즉, 게임의 시작 지점은 [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 메서드 구현 본문(이 경우 **App::Run**)에 있습니다. 코드는 다음과 같습니다.

```cpp
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

게임 창을 닫으면 이 메서드는 모든 이벤트를 디스패치하고 타이머를 업데이트하며 그래픽 파이프라인의 결과를 렌더링하여 제공합니다. 이 메서드는 [게임의 UWP 프레임워크 정의](tutorial--building-the-games-metro-style-app-framework.md) 및 [렌더링 파이프라인 어셈블](tutorial--assembling-the-rendering-pipeline.md)에서 자세히 살펴보겠습니다. 지금은 UWP DirectX 게임의 기본 코드 구조를 알아보아야 합니다.

## 2. package.appxmanifest 파일을 검토하고 업데이트합니다.


코드 파일이 템플릿과 관련된 전부가 아닙니다. **package.appxmanifest** 파일에는 게임 패키징 및 시작과 Windows 스토어에 제출하는 데 사용되는 프로젝트에 대한 메타데이터가 포함되어 있습니다. 또한 플레이어의 시스템에서 게임에서 실행되어야 하는 시스템 리소스에 액세스하는 데 사용하는 중요한 정보가 포함되어 있습니다.

**솔루션 탐색기**에서 **package.appxmanifest** 파일을 두 번 클릭하여 **Manifest Designer(매니페스트 디자이너)**를 시작합니다. 다음 보기가 표시됩니다.

![package.appx 매니페스트 편집기.](images/simple-dx-game-vs-app-manifest.png)

**package.appxmanifest** 파일 및 패키징에 대한 자세한 내용은 [매니페스트 디자이너](https://msdn.microsoft.com/library/windows/apps/br230259.aspx)를 참조하세요. 이제 **기능** 탭을 살펴보고 제공된 옵션을 살펴봅니다.

![direct3d 앱의 기본 기능.](images/simple-dx-game-vs-capabilities.png)

전역 최고 점수 보드를 위한 **인터넷** 액세스 같이 게임에서 사용되는 기능을 선택하지 않으면 해당 리소스나 기능에 액세스할 수 없습니다. 새 게임을 만들 때는 게임에서 실행되어야 하는 기능을 선택해야 합니다.

이제 **DirectX 11 앱(유니버설 Windows)** 템플릿과 함께 제공되는 나머지 파일을 살펴보겠습니다.

## 3. 포함된 라이브러리 및 헤더를 검토합니다.


아직 살펴보지 않은 몇 개의 파일이 있습니다. 이 파일은 Direct3D 게임 개발 시나리오에 공통적으로 적용되는 추가 도구 및 지원을 제공합니다.

| 템플릿 원본 파일         | 설명                                                                                                                                                                                                            |
|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| StepTimer.h                  | 게임 또는 대화형 렌더링 앱에 유용한 고해상도 타이머를 정의합니다.                                                                                                                                       |
| Sample3DSceneRenderer.h/.cpp | Direct3D 스왑 체인 및 그래픽 어댑터를 DirectX로 작성된 UWP 앱에 연결하는 기본 렌더러 구현을 정의합니다.                                                                                            |
| DirectXHelper.h              | DirectX API에서 반환된 오류 HRESULT 값을 Windows 런타임 예외로 변환하는 단일 메서드 **DX::ThrowIfFailed**를 구현합니다. 이 메서드를 사용하여 DirectX 오류를 디버깅하기 위한 중단점을 배치합니다. |
| pch.h/.cpp                   | DirectX 11 API를 포함하여 Direct3D 앱이 사용하는 API에 대한 모든 Windows 시스템 include가 들어 있습니다.                                                                                                           |
| SamplePixelShader.hlsl       | 기본 픽셀 셰이더에 대한 HLSL(High Level Shader Language) 코드가 들어 있습니다.                                                                                                                                     |
| SampleVertexShader.hlsl      | 기본 꼭짓점 셰이더에 대한 HLSL(High Level Shader Language) 코드가 들어 있습니다.                                                                                                                                    |

 

### 다음 단계

이제 DirectX를 사용하는 UWP를 만들고 DirectX 11 앱(유니버설 Windows) 템플릿에서 제공하는 구성 요소 및 파일을 식별할 수 있습니다.

다음 [게임의 UWP 프레임워크 정의](tutorial--building-the-games-metro-style-app-framework.md) 자습서에서는 완성된 게임을 사용하고, 템플릿에서 제공하는 여러 가지 개념과 구성 요소를 사용 및 확장하는 방법에 대해 검토합니다.

 

 






<!--HONumber=Mar16_HO1-->


