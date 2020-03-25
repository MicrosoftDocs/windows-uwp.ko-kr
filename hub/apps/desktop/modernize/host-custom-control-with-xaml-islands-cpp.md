---
description: 이 문서에서는 XAML 호스팅 API를 사용 하 여 C++ Win32 앱에서 사용자 지정 UWP 컨트롤을 호스트 하는 방법을 보여 줍니다.
title: XAML 호스팅 API를 사용 하 여 C++ Win32 앱에서 사용자 지정 UWP 컨트롤 호스팅
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp, C++, Win32, xaml 아일랜드, 사용자 지정 컨트롤, 사용자 정의 컨트롤, 호스트 컨트롤
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 2f34c9c56cf9db5dfcfd702b97f2d34273b86e6a
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226317"
---
# <a name="host-a-custom-uwp-control-in-a-c-win32-app"></a>C++ Win32 앱에서 사용자 지정 UWP 컨트롤 호스트

이 문서에서는 [UWP xaml 호스팅 API](using-the-xaml-hosting-api.md) 를 사용 하 여 새 C++ Win32 앱에서 사용자 지정 uwp xaml 컨트롤을 호스트 하는 방법을 보여 줍니다. 기존 C++ Win32 응용 프로그램 프로젝트가 있는 경우 프로젝트에 대 한 이러한 단계 및 코드 예제를 적용할 수 있습니다.

사용자 지정 UWP XAML 컨트롤을 호스트 하려면이 연습의 일부로 다음 프로젝트 및 구성 요소를 만듭니다.

* **Windows 데스크톱 응용 프로그램 프로젝트**입니다. 이 프로젝트는 네이티브 C++ Win32 데스크톱 앱을 구현 합니다. UWP XAML 호스팅 API를 사용 하 여 사용자 지정 UWP XAML 컨트롤을 호스트 하는 코드를이 프로젝트에 추가 합니다.

* **UWP 앱 프로젝트 (C++/winrt)** . 이 프로젝트는 사용자 지정 UWP XAML 컨트롤을 구현 합니다. 또한 프로젝트에서 사용자 지정 UWP XAML 형식에 대 한 메타 데이터를 로드 하기 위한 루트 메타 데이터 공급자를 구현 합니다.

## <a name="requirements"></a>요구 사항

* Visual Studio 2019 버전 16.4.3 이상
* Windows 10, 버전 1903 SDK (버전 10.0.18362) 이상.
* Visual studio와 함께 설치 되는 [/winrt VSIX (visual studio Extension). C++](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) C++/WinRT는 Windows 런타임(WinRT) API용 최신 표준 C++17 언어 프로젝션으로서 헤더 파일 기반 라이브러리로 구현되며, 오늘날 Windows API에 대해 최고 수준의 액세스를 제공하도록 설계되었습니다. 자세한 내용은 [ C++/winrt](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/)를 참조 하세요.

## <a name="create-a-desktop-application-project"></a>데스크톱 응용 프로그램 프로젝트 만들기

1. Visual Studio에서 **MyDesktopWin32App**라는 새 **Windows 데스크톱 응용 프로그램** 프로젝트를 만듭니다. 이 프로젝트 템플릿은 **C++** , **Windows**및 **데스크톱** 프로젝트 필터 아래에서 사용할 수 있습니다.

2. **솔루션 탐색기**에서 솔루션 노드를 마우스 오른쪽 단추로 클릭 하 고, **솔루션**대상 변경을 클릭 하 고, **10.0.18362.0** 또는 이후 버전의 SDK 릴리스를 선택한 다음 **확인**을 클릭 합니다.

3. 프로젝트에서 [ C++/winrt](/windows/uwp/cpp-and-winrt-apis) 를 지원할 수 있도록 [CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet 패키지를 설치 합니다.

    1. **솔루션 탐색기** 에서 **MyDesktopWin32App** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리**를 선택 합니다.
    2. **찾아보기** 탭을 선택 하 고 [CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) 패키지를 검색 한 후이 패키지의 최신 버전을 설치 합니다.

4. **Nuget 패키지 관리** 창에서 다음과 같은 추가 NuGet 패키지를 설치 합니다.

    * 6\.0.0 (버전 v-이상 [).](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) 이 패키지는 XAML 아일랜드가 앱에서 작동할 수 있도록 하는 몇 가지 빌드 및 런타임 자산을 제공 합니다.
    * 6\.0.0 (버전 v 이상)를 다시 [적용](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) 합니다.
    * [VCRTForwarders](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140).

5. 솔루션을 빌드하고 성공적으로 빌드되는지 확인 합니다.

## <a name="create-a-uwp-app-project"></a>UWP 앱 프로젝트 만들기

그런 다음 솔루션에 **UWP (C++/winrt)** 앱 프로젝트를 추가 하 고이 프로젝트에 대 한 구성을 변경 합니다. 이 연습 뒷부분에서는이 프로젝트에 코드를 추가 하 여 사용자 지정 UWP XAML 컨트롤을 구현 하 고 Windows 커뮤니티 도구 키트에서 제공 하는 [Microsoft Toolkit. Xamlhost](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) 클래스의 인스턴스를 정의 합니다. 앱은이 클래스를 사용자 지정 UWP XAML 형식에 대 한 메타 데이터를 로드 하기 위한 루트 메타 데이터 공급자로 사용 합니다.

1. **솔루션 탐색기**에서 솔루션 노드를 마우스 오른쪽 단추로 클릭 하 고 **추가** -> **새 프로젝트**를 선택 합니다.

2. 비어 있는 **앱 (C++/winrt)** 프로젝트를 솔루션에 추가 합니다. 프로젝트 이름을 **MyUWPApp** 로 지정 하 고 대상 버전 및 최소 버전이 모두 **Windows 10 버전 1903** 이상으로 설정 되어 있는지 확인 합니다.

3. **MyUWPApp** 프로젝트에 다음과 같이 [Microsoft Toolkit 응용 프로그램](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) NuGet 패키지를 설치 합니다.

    1. **MyUWPApp** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리**를 선택 합니다.
    2. **찾아보기** 탭을 선택 하 고, 6.0.0 [응용 프로그램](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) 패키지를 검색 하 고,이 패키지의 v 이상 버전을 설치 합니다.

4. **MyUWPApp** 노드를 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다. **공용 속성** ->  **C++/winrt** 페이지에서 다음 속성을 설정한 다음 **적용**을 클릭 합니다.

    * **자세한 정도** 를 **보통**으로 설정 합니다.
    * **최적화** 됨을 **아니요**로 설정 합니다.

    완료 되 면 속성 페이지가 다음과 같이 표시 됩니다.

    ![C++/WinRT 프로젝트 속성](images/xaml-islands/xaml-island-cpp-1.png)

5. 속성 창의 **구성 속성** -> **일반** 페이지에서 **구성 유형** 을 **동적 라이브러리 (.dll)** 로 설정 하 고 **확인** 을 클릭 하 여 속성 창을 닫습니다.

    ![일반 프로젝트 속성](images/xaml-islands/xaml-island-cpp-2.png)

6. **MyUWPApp** 프로젝트에 자리 표시자 실행 파일을 추가 합니다. 이 자리 표시자 실행 파일은 Visual Studio에서 필요한 프로젝트 파일을 생성 하 고 프로젝트를 제대로 빌드하는 데 필요 합니다.

    1. **솔루션 탐색기**에서 **MyUWPApp** 프로젝트 노드를 마우스 오른쪽 단추로 클릭 하 고 **추가** -> **새 항목**을 선택 합니다.
    2. **새 항목 추가** 대화 상자의 왼쪽 페이지에서 **유틸리티** 를 선택 하 고 **텍스트 파일 (.txt)** 을 선택 합니다. 이름 **자리 표시자 .exe** 를 입력 하 고 **추가**를 클릭 합니다.
      텍스트 파일을 추가 ![](images/xaml-islands/xaml-island-cpp-3.png)
    3. **솔루션 탐색기**에서 **자리 표시자 .exe** 파일을 선택 합니다. **속성** 창에서 **Content** 속성이 **True**로 설정 되어 있는지 확인 합니다.
    4. **솔루션 탐색기**에서 **MyUWPApp** 프로젝트의 **appxmanifest.xml** 파일을 마우스 오른쪽 단추로 클릭 하 고 **연결 프로그램**을 선택 하 고 **XML (텍스트) 편집기**를 선택한 다음 **확인**을 클릭 합니다.
    5. **&lt;응용 프로그램&gt;** 요소를 찾고 **실행 가능한** 특성을 `placeholder.exe`값으로 변경 합니다. 완료 되 면 **&lt;응용 프로그램&gt;** 요소가 다음과 같이 표시 됩니다.

        ```xml
        <Application Id="App" Executable="placeholder.exe" EntryPoint="MyUWPApp.App">
          <uap:VisualElements DisplayName="MyUWPApp" Description="Project for a single page C++/WinRT Universal Windows Platform (UWP) app with no predefined layout"
            Square150x150Logo="Assets\Square150x150Logo.png" Square44x44Logo="Assets\Square44x44Logo.png" BackgroundColor="transparent">
            <uap:DefaultTile Wide310x150Logo="Assets\Wide310x150Logo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
        ```

    6. **Appxmanifest.xml** 파일을 저장 하 고 닫습니다.

7. **솔루션 탐색기**에서 **MyUWPApp** 노드를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 언로드**를 선택 합니다.
8. **MyUWPApp** 노드를 마우스 오른쪽 단추로 클릭 하 고 **편집 MyUWPApp**를 선택 합니다.
9. `<Import Project="$(VCTargetsPath)\Microsoft.Cpp.Default.props" />` 요소를 찾아서 다음 XML로 바꿉니다. 이 XML은 요소 바로 앞에 여러 개의 새 속성을 추가 합니다.

    ```xml
    <PropertyGroup Label="Globals">
        <WindowsAppContainer>true</WindowsAppContainer>
        <AppxGeneratePriEnabled>true</AppxGeneratePriEnabled>
        <ProjectPriIndexName>App</ProjectPriIndexName>
        <AppxPackage>true</AppxPackage>
    </PropertyGroup>
    <Import Project="$(VCTargetsPath)\Microsoft.Cpp.Default.props" />
    ```

10. 프로젝트 파일을 저장한 후 닫습니다.
11. **솔루션 탐색기**에서 **MyUWPApp** 노드를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 다시 로드**를 선택 합니다.

## <a name="configure-the-solution"></a>솔루션 구성

이 섹션에서는 두 프로젝트를 모두 포함 하는 솔루션을 업데이트 하 여 프로젝트를 올바르게 빌드하는 데 필요한 프로젝트 종속성 및 빌드 속성을 구성 합니다.

1. **솔루션 탐색기**에서 솔루션 노드를 마우스 오른쪽 단추로 클릭 하 고 이름이 **SOLUTION**인 새 XML 파일을 추가 합니다.
2. 다음 XML을 **솔루션. props** 파일에 추가 합니다.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
      <PropertyGroup>
        <IntDir>$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(MSBuildProjectName)\</IntDir>
        <OutDir>$(SolutionDir)\bin\$(Platform)\$(Configuration)\$(MSBuildProjectName)\</OutDir>
        <GeneratedFilesDir>$(IntDir)Generated Files\</GeneratedFilesDir>
      </PropertyGroup>
    </Project>
    ```

3. **보기** 메뉴에서 **속성 관리자** 를 클릭 합니다. 구성에 따라 -> **다른 창** **보기** 에 있을 수 있습니다.
4. **속성 관리자** 창에서 **MyDesktopWin32App** 를 마우스 오른쪽 단추로 클릭 하 고 **기존 속성 시트 추가**를 선택 합니다. 방금 추가한 **Solution props** 파일로 이동 하 고 **열기**를 클릭 합니다.
5. 이전 단계를 반복 하 여 **속성 관리자** 창의 **MyUWPApp** 프로젝트에 **Solution props** 파일을 추가 합니다.
6. **속성 관리자** 창을 닫습니다.
7. 속성 시트 변경 내용이 제대로 저장 되었는지 확인 합니다. **솔루션 탐색기**에서 **MyDesktopWin32App** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다. **구성 속성** -> **gen, al**을 클릭 하 고 **출력 디렉터리** 및 **중간 디렉터리** 속성에 **솔루션. props** 파일에 추가한 값이 있는지 확인 합니다. **MyUWPApp** 프로젝트에 대해 동일한를 확인할 수도 있습니다.
    프로젝트 속성을 ![](images/xaml-islands/xaml-island-cpp-4.png)

8. **솔루션 탐색기**에서 솔루션 노드를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 종속성**을 선택 합니다. **프로젝트** 드롭다운에서 **MyDesktopWin32App** 이 선택 되어 있는지 확인 하 고 **종속** 목록에서 **MyUWPApp** 를 선택 합니다.
    프로젝트 종속성을 ![](images/xaml-islands/xaml-island-cpp-5.png)

9. **확인**을 클릭합니다.

## <a name="add-code-to-the-uwp-app-project"></a>UWP 앱 프로젝트에 코드 추가

이제 **MyUWPApp** 프로젝트에 코드를 추가 하 여 이러한 작업을 수행할 준비가 되었습니다.

* 사용자 지정 UWP XAML 컨트롤을 구현 합니다. 이 연습의 뒷부분에서는 **MyDesktopWin32App** 프로젝트에서이 컨트롤을 호스트 하는 코드를 추가 합니다.
* Windows 커뮤니티 도구 키트의 [Microsoft toolkit. Xamlhost](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) 클래스에서 파생 되는 형식을 정의 합니다. 이 클래스는 사용자 지정 UWP XAML 형식에 대 한 메타 데이터를 로드 하기 위한 루트 메타 데이터 공급자로 작동 합니다.

### <a name="define-a-custom-uwp-xaml-control"></a>사용자 지정 UWP XAML 컨트롤 정의

1. **솔루션 탐색기**에서 **MyUWPApp** 을 마우스 오른쪽 단추로 클릭 하 고 **추가** -> **새 항목**을 선택 합니다. 왼쪽 창 **에서 C++ 시각적** 노드를 선택 하 고 **비어 있는 사용자 정의 컨트롤C++(/winrt)** 을 선택 하 고 이름을 **MyUserControl**로 이름을 입력 한 다음 **추가**를 클릭 합니다.
2. XAML 편집기에서 **MyUserControl** 파일의 내용을 다음 xaml로 바꾸고 파일을 저장 합니다.

    ```xml
    <UserControl
        x:Class="MyUWPApp.MyUserControl"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyUWPApp"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <StackPanel HorizontalAlignment="Center" Spacing="10" 
                    Padding="20" VerticalAlignment="Center">
            <TextBlock HorizontalAlignment="Center" TextWrapping="Wrap" 
                           Text="Hello from XAML Islands" FontSize="30" />
            <TextBlock HorizontalAlignment="Center" Margin="15" TextWrapping="Wrap"
                           Text="😍❤💋🌹🎉😎�🐱‍👤" FontSize="16" />
            <Button HorizontalAlignment="Center" 
                    x:Name="Button" Click="ClickHandler">Click Me</Button>
        </StackPanel>
    </UserControl>
    ```

### <a name="define-a-xamlapplication-class"></a>XamlApplication 클래스 정의

다음으로 **MyUWPApp** 프로젝트의 기본 **앱** 클래스를 수정 하 여 Windows 커뮤니티 도구 키트에서 제공 하는 [Microsoft Toolkit. xamlhost](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) 클래스에서 파생 합니다. 이 연습의 뒷부분에서는 사용자 지정 UWP XAML 형식에 대 한 메타 데이터를 로드 하기 위한 루트 메타 데이터 공급자로이 클래스의 인스턴스를 만들도록 데스크톱 프로젝트를 업데이트 합니다.

  > [!NOTE]
  > XAML 아일랜드를 사용 하는 각 솔루션에는 `XamlApplication` 개체를 정의 하는 프로젝트가 하나만 포함 될 수 있습니다. 앱의 모든 사용자 지정 UWP XAML 컨트롤은 동일한 `XamlApplication` 개체를 공유 합니다. 

1. **솔루션 탐색기**에서 **MyUWPApp** 프로젝트의 **mainpage .xaml** 파일을 마우스 오른쪽 단추로 클릭 합니다. **제거** 를 클릭 한 다음 **삭제** 를 클릭 하 여 프로젝트에서이 permamently 파일을 삭제 합니다.
2. **MyUWPApp** 프로젝트에서 **app.xaml** 파일을 확장 합니다.
3. **App.xaml** **, app.xaml, app-v**및 **응용 프로그램 .idl** 파일의 내용을 다음 **App.h**코드로 바꿉니다.

    * **App.xaml**:

        ```xml
        <Toolkit:XamlApplication
            x:Class="MyUWPApp.App"
            xmlns:Toolkit="using:Microsoft.Toolkit.Win32.UI.XamlHost"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="using:MyUWPApp">
        </Toolkit:XamlApplication>
        ```

    * **응용 프로그램 idl**:

        ```IDL
        namespace MyUWPApp
        {
             [default_interface]
             runtimeclass App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
             {
                App();
             }
        }
        ```

    * **App-v**:

        ```cpp
        #pragma once
        #include "App.g.h"
        #include "App.base.h"
        namespace winrt::MyUWPApp::implementation
        {
            class App : public AppT2<App>
            {
            public:
                App();
                ~App();
            };
        }
        namespace winrt::MyUWPApp::factory_implementation
        {
            class App : public AppT<App, implementation::App>
            {
            };
        }
        ```

    * **App-v**:

        ```cpp
        #include "pch.h"
        #include "App.h"
        using namespace winrt;
        using namespace Windows::UI::Xaml;
        namespace winrt::MyUWPApp::implementation
        {
            App::App()
            {
                Initialize();
                AddRef();
                m_inner.as<::IUnknown>()->Release();
            }
            App::~App()
            {
                Close();
            }
        }
        ```

4. **MyUWPApp** 프로젝트에 새 헤더 파일을 추가 **합니다.**
5. 다음 코드를 **app.config** 파일에 추가 하 고 파일을 저장 한 다음 닫습니다.

    ```cpp
    #pragma once
    namespace winrt::MyUWPApp::implementation
    {
        template <typename D, typename... I>
        struct App_baseWithProvider : public App_base<D, ::winrt::Windows::UI::Xaml::Markup::IXamlMetadataProvider>
        {
            using IXamlType = ::winrt::Windows::UI::Xaml::Markup::IXamlType;
            IXamlType GetXamlType(::winrt::Windows::UI::Xaml::Interop::TypeName const& type)
            {
                return AppProvider()->GetXamlType(type);
            }
            IXamlType GetXamlType(::winrt::hstring const& fullName)
            {
                return AppProvider()->GetXamlType(fullName);
            }
            ::winrt::com_array<::winrt::Windows::UI::Xaml::Markup::XmlnsDefinition> GetXmlnsDefinitions()
            {
                return AppProvider()->GetXmlnsDefinitions();
            }
        private:
            bool _contentLoaded{ false };
            std::shared_ptr<XamlMetaDataProvider> _appProvider;
            std::shared_ptr<XamlMetaDataProvider> AppProvider()
            {
                if (!_appProvider)
                {
                    _appProvider = std::make_shared<XamlMetaDataProvider>();
                }
                return _appProvider;
            }
        };
        template <typename D, typename... I>
        using AppT2 = App_baseWithProvider<D, I...>;
    }
    ```

6. 솔루션을 빌드하고 성공적으로 빌드되는지 확인 합니다.

## <a name="configure-the-desktop-project-to-consume-custom-control-types"></a>사용자 지정 컨트롤 형식을 사용 하도록 데스크톱 프로젝트 구성

**MyDesktopWin32App** 앱이 xaml 아일랜드에서 사용자 지정 UWP xaml 컨트롤을 호스트 하려면 먼저 **MyUWPApp** 프로젝트의 사용자 지정 컨트롤 형식을 사용 하도록 구성 해야 합니다. 이 작업을 수행 하는 방법에는 두 가지가 있습니다 .이 연습을 완료 하면 두 가지 옵션 중 하나를 선택할 수 있습니다.

### <a name="option-1-package-the-app-using-msix"></a>옵션 1: MSIX을 사용 하 여 앱 패키지

배포를 위해 [Msix 패키지](https://docs.microsoft.com/windows/msix) 에 앱을 패키지할 수 있습니다. MSIX은 Windows 용 최신 앱 패키징 기술 이며 MSI, .appx, App-v 및 ClickOnce 설치 기술의 조합을 기반으로 합니다.

1. 새 [Windows 응용 프로그램 패키징 프로젝트](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) 를 솔루션에 추가 합니다. 프로젝트를 만들 때 이름을 **MyDesktopWin32Project** 로 표시 하 고 **Windows 10, 버전 1903 (10.0;을 선택 합니다. 빌드 18362)** 는 **대상 버전** 및 **최소 버전**모두에 해당 합니다.

2. 패키징 프로젝트에서 **응용 프로그램** 노드를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가**를 선택 합니다. 프로젝트 목록에서 **MyDesktopWin32App** 프로젝트 옆의 확인란을 선택 하 고 **확인**을 클릭 합니다.
    ![참조 프로젝트](images/xaml-islands/xaml-island-cpp-6.png)

> [!NOTE]
> 배포를 위해 [Msix 패키지](https://docs.microsoft.com/windows/msix) 에 응용 프로그램을 패키지 하지 않도록 선택 하는 경우 앱을 실행 하는 컴퓨터에 [는 C++ Visual Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) 이 설치 되어 있어야 합니다.

### <a name="option-2-create-an-application-manifest"></a>옵션 2: 응용 프로그램 매니페스트 만들기

[응용 프로그램 매니페스트](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) 를 앱에 추가할 수 있습니다.

1. **MyDesktopWin32App** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **추가** -> **새 항목**을 선택 합니다. 
2. **새 항목 추가** 대화 상자의 왼쪽 창에서 **웹** 을 클릭 하 고 **xml 파일 (.xml)** 을 선택 합니다. 
3. 새 파일의 이름을 **app.config** 로 하 고 **추가**를 클릭 합니다.
4. 새 파일의 내용을 다음 XML로 바꿉니다. 이 XML은 **MyUWPApp** 프로젝트에 사용자 지정 컨트롤 형식을 등록 합니다.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <assembly
     xmlns="urn:schemas-microsoft-com:asm.v1"
     xmlns:asmv3="urn:schemas-microsoft-com:asm.v3"
     manifestVersion="1.0">
      <asmv3:file name="MyUWPApp.dll">
        <activatableClass
            name="MyUWPApp.App"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
        <activatableClass
            name="MyUWPApp.XamlMetadataProvider"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
        <activatableClass
            name="MyUWPApp.MyUserControl"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
      </asmv3:file>
    </assembly>
    ```

## <a name="configure-additional-desktop-project-properties"></a>추가 데스크톱 프로젝트 속성 구성

그런 다음 추가 포함 디렉터리에 대 한 매크로를 정의 하 고 추가 속성을 구성 하도록 **MyDesktopWin32App** 프로젝트를 업데이트 합니다.

1. **솔루션 탐색기**에서 **MyDesktopWin32App** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 언로드**를 선택 합니다.

2. **MyDesktopWin32App (언로드 됨)** 를 마우스 오른쪽 단추로 클릭 하 고 **편집 MyDesktopWin32App**를 선택 합니다.

3. 파일의 끝에 있는 닫는 `</Project>` 태그 바로 앞에 다음 XML을 추가 합니다. 그런 다음 파일을 저장 하 고 닫습니다.

    ```xml
      <!-- Configure these for your UWP project -->
      <PropertyGroup>
        <AppProjectName>MyUWPApp</AppProjectName>
      </PropertyGroup>
      <PropertyGroup>
        <AppIncludeDirectories>$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(AppProjectName)\;$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(AppProjectName)\Generated Files\;</AppIncludeDirectories>
      </PropertyGroup>
      <ItemGroup>
        <ProjectReference Include="..\$(AppProjectName)\$(AppProjectName).vcxproj" />
      </ItemGroup>
      <!-- End Section-->
    ```

4. **솔루션 탐색기**에서 **MyDesktopWin32App (언로드 됨)** 를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 다시 로드**를 선택 합니다.

5. **MyDesktopWin32App**를 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택한 다음 왼쪽 창에서 **C/C++**  노드를 클릭 합니다. **추가 포함 디렉터리** 매크로가 이전 단계에서 변경한 프로젝트 파일에서 정의 되었는지 확인 합니다.
    ![C/C++ 프로젝트 설정](images/xaml-islands/xaml-island-cpp-7.png)

6. **속성 페이지** 대화 상자에서 **매니페스트 도구** -> **입력 및 출력**을 확장 합니다. **Dpi** 인식 속성을 **모니터 단위 높은 dpi 인식**으로 설정 합니다. 이 속성을 설정 하지 않으면 특정 높은 DPI 시나리오에서 매니페스트 구성 오류가 발생할 수 있습니다.
    ![C/C++ 프로젝트 설정](images/xaml-islands/xaml-island-cpp-8.png)

## <a name="host-the-custom-uwp-xaml-control-in-the-desktop-project"></a>데스크톱 프로젝트에서 사용자 지정 UWP XAML 컨트롤 호스트

마지막으로 **MyDesktopWin32App** 프로젝트에 코드를 추가 하 여 이전에 **MyUWPApp** 프로젝트에서 정의한 사용자 지정 UWP XAML 컨트롤을 호스트할 준비가 되었습니다.

1. **MyDesktopWin32App** 프로젝트에서 **프레임 워크 .h** 파일을 열고 다음 코드 줄을 주석으로 처리 합니다. 완료 되 면 파일을 저장 합니다.

    ```cpp
    #define WIN32_LEAN_AND_MEAN
    ```

2. **MyDesktopWin32App** 파일을 열고이 파일의 내용을 다음 코드로 바꾸어 필요한 C++/winrt 헤더 파일을 참조 합니다. 완료 되 면 파일을 저장 합니다.

    ```cpp
    #pragma once

    #include "resource.h"
    #include <winrt/Windows.Foundation.Collections.h>
    #include <winrt/Windows.system.h>
    #include <winrt/windows.ui.xaml.hosting.h>
    #include <windows.ui.xaml.hosting.desktopwindowxamlsource.h>
    #include <winrt/windows.ui.xaml.controls.h>
    #include <winrt/Windows.ui.xaml.media.h>
    #include <winrt/Windows.UI.Core.h>
    #include <winrt/MyUWPApp.h>

    using namespace winrt;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Xaml::Hosting;
    using namespace Windows::Foundation::Numerics;
    using namespace Windows::UI::Xaml::Controls;
    ```

3. **MyDesktopWin32App** 파일을 열고 `Global Variables:` 섹션에 다음 코드를 추가 합니다.

    ```cpp
    winrt::MyUWPApp::App hostApp{ nullptr };
    winrt::Windows::UI::Xaml::Hosting::DesktopWindowXamlSource _desktopWindowXamlSource{ nullptr };
    winrt::MyUWPApp::MyUserControl _myUserControl{ nullptr };
    ```

4. 동일한 파일에서 다음 코드를 `Forward declarations of functions included in this code module:` 섹션에 추가 합니다.

    ```cpp
    void AdjustLayout(HWND);
    ```

5. 동일한 파일에서 `wWinMain` 함수의 `TODO: Place code here.` 주석 바로 뒤에 다음 코드를 추가 합니다.

    ```cpp
    // TODO: Place code here.
    winrt::init_apartment(winrt::apartment_type::single_threaded);
    hostApp = winrt::MyUWPApp::App{};
    _desktopWindowXamlSource = winrt::Windows::UI::Xaml::Hosting::DesktopWindowXamlSource{};
    ```

6. 동일한 파일에서 기본 `InitInstance` 함수를 다음 코드로 바꿉니다.

    ```cpp
    BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
    {
        hInst = hInstance; // Store instance handle in our global variable

        HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
            CW_USEDEFAULT, 0, CW_USEDEFAULT, 0, nullptr, nullptr, hInstance, nullptr);

        if (!hWnd)
        {
            return FALSE;
        }

        // Begin XAML Islands walkthrough code.
        if (_desktopWindowXamlSource != nullptr)
        {
            auto interop = _desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
            check_hresult(interop->AttachToWindow(hWnd));
            HWND hWndXamlIsland = nullptr;
            interop->get_WindowHandle(&hWndXamlIsland);
            RECT windowRect;
            ::GetWindowRect(hWnd, &windowRect);
            ::SetWindowPos(hWndXamlIsland, NULL, 0, 0, windowRect.right - windowRect.left, windowRect.bottom - windowRect.top, SWP_SHOWWINDOW);
            _myUserControl = winrt::MyUWPApp::MyUserControl();
            _desktopWindowXamlSource.Content(_myUserControl);
        }
        // End XAML Islands walkthrough code.

        ShowWindow(hWnd, nCmdShow);
        UpdateWindow(hWnd);

        return TRUE;
    }
    ```

7. 동일한 파일에서 파일의 끝에 다음 새 함수를 추가 합니다.

    ```cpp
    void AdjustLayout(HWND hWnd)
    {
        if (_desktopWindowXamlSource != nullptr)
        {
            auto interop = _desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
            HWND xamlHostHwnd = NULL;
            check_hresult(interop->get_WindowHandle(&xamlHostHwnd));
            RECT windowRect;
            ::GetWindowRect(hWnd, &windowRect);
            ::SetWindowPos(xamlHostHwnd, NULL, 0, 0, windowRect.right - windowRect.left, windowRect.bottom - windowRect.top, SWP_SHOWWINDOW);
        }
    }
    ```

8. 동일한 파일에서 `WndProc` 함수를 찾습니다. Switch 문의 기본 `WM_DESTROY` 처리기를 다음 코드로 바꿉니다.

    ```cpp
    case WM_DESTROY:
        PostQuitMessage(0);
        if (_desktopWindowXamlSource != nullptr)
        {
            _desktopWindowXamlSource.Close();
            _desktopWindowXamlSource = nullptr;
        }
        break;
    case WM_SIZE:
        AdjustLayout(hWnd);
        break;
    ```

9. 파일을 저장합니다.
10. 솔루션을 빌드하고 성공적으로 빌드되는지 확인 합니다.

## <a name="test-the-app"></a>앱 테스트

솔루션을 실행 하 고 다음 창이 포함 된 **MyDesktopWin32App** 가 열리는지 확인 합니다.

![MyDesktopWin32App 앱](images/xaml-islands/xaml-island-cpp-9.png)

## <a name="next-steps"></a>다음 단계

XAML 아일랜드를 호스트 하는 많은 데스크톱 응용 프로그램은 원활한 사용자 환경을 제공 하기 위해 추가 시나리오를 처리 해야 합니다. 예를 들어 데스크톱 응용 프로그램은 XAML 아일랜드에서 키보드 입력을 처리 하 고, XAML 아일랜드와 기타 UI 요소 사이에 포커스를 탐색 하 고, 레이아웃을 변경 해야 할 수 있습니다.

이러한 시나리오 및 관련 코드 샘플에 대 한 포인터를 처리 하는 방법에 대 한 자세한 내용은 [Win32 C++ 앱의 XAML 아일랜드에 대 한 고급 시나리오](advanced-scenarios-xaml-islands-cpp.md)를 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [데스크톱 앱에서 UWP XAML 컨트롤 호스트 (XAML 제도)](xaml-islands.md)
* [C++ Win32 앱에서 UWP XAML 호스팅 API 사용](using-the-xaml-hosting-api.md)
* [C++ Win32 앱에서 표준 UWP 컨트롤 호스팅](host-standard-control-with-xaml-islands-cpp.md)
* [Win32 앱의 C++ XAML 아일랜드에 대 한 고급 시나리오](advanced-scenarios-xaml-islands-cpp.md)
* [XAML 아일랜드 코드 샘플](https://github.com/microsoft/Xaml-Islands-Samples)
