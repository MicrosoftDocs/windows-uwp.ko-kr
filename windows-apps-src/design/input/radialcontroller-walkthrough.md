---
author: Karl-Bridge-Microsoft
ms.assetid: ''
title: UWP 앱에서 Surface Dial(및 기타 휠 장치) 지원
description: UWP 앱에 Surface Dial(및 기타 휠 장치)에 대한 지원을 추가하는 단계별 자습서입니다.
keywords: 다이얼, 방사형, 자습서
ms.author: kbridge
ms.date: 01/25/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: c7dc6436e1a233a6b0a74a787b5c30de47899eff
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/20/2018
ms.locfileid: "5169540"
---
# <a name="tutorial-support-the-surface-dial-and-other-wheel-devices-in-your-uwp-app"></a>자습서: UWP 앱에서 Surface Dial(및 기타 휠 장치) 지원

![Surface Studio가 있는 Surface Dial 이미지](images/radialcontroller/dial-pen-studio-600px.png)  
*Surface Studio 및 Surface 펜이 있는 Surface Dial*([Microsoft 스토어](https://aka.ms/purchasesurfacedial)에서 구매 가능)

이 자습서에서는 Surface Dial과 같은 휠 디바이스에서 지원하는 사용자 상호 작용 환경을 사용자 지정하는 방법을 단계별로 설명합니다. GitHub에서 다운로드할 수 있는 샘플 앱에서 코드 조각([샘플 코드](#sample-code) 참조)을 사용하여 다양한 기능과 연결된 [**RadialController**](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) API에 대해 설명합니다.

중점을 두고 살펴볼 내용은 다음과 같습니다.
* [**RadialController**](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) 메뉴에 표시되는 기본 제공 도구 지정
* 사용자 지정 도구를 메뉴에 추가
* 촉각 피드백 제어
* 클릭 상호 작용 사용자 지정
* 회전 상호 작용 사용자 지정

이러한 기능과 기타 기능의 구현에 대한 자세한 내용은 [UWP 앱에서 Surface Dial 상호 작용](windows-wheel-interactions.md)을 참조하세요.

## <a name="introduction"></a>소개

Surface Dial은 펜, 터치, 마우스와 같은 기본 입력 디바이스와 함께 사용했을 때 사용자의 생산성을 높이는 데 도움이 되는 보조 입력 디바이스입니다. 보조 입력 디바이스인 다이얼은 일반적으로 주로 사용하는 손이 아닌 손으로 제어하여 시스템 명령과 더 상황에 맞는, 도구 및 기능에 액세스하는 데 사용됩니다. 

다이얼은 세 가지 기본 제스처를 지원합니다. 
- 길게 눌러 기본 제공 명령 메뉴를 표시합니다.
- 메뉴가 활성화된 경우 회전시켜 메뉴 항목을 강조 표시하거나 메뉴가 활성화되지 않은 경우 앱에서 현재 작업을 수정합니다.
- 메뉴가 활성화된 경우 클릭하여 강조 표시된 메뉴 항목을 선택하거나 메뉴가 활성화되지 않은 경우 앱의 명령을 호출합니다.

## <a name="prerequisites"></a>필수 구성 요소

* Windows 10 크리에이터 업데이트 이상을 실행하는 컴퓨터(또는 가상 컴퓨터)
* [Visual Studio 2017(10.0.15063.0)](https://developer.microsoft.com/windows/downloads)
* [Windows 10 SDK(10.0.15063.0)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* 휠 디바이스(지금은 [Surface Dial](https://aka.ms/purchasesurfacedial)만)
* Visual Studio를 사용하는 UWP(유니버설 Windows 플랫폼) 앱 개발을 처음 하는 경우, 이 자습서를 시작하기 전에 이러한 항목을 살펴보십시오.  
    * [설정하기](https://docs.microsoft.com/windows/uwp/get-started/get-set-up)
    * ["Hello, World" 앱 만들기(XAML)](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)

## <a name="set-up-your-devices"></a>디바이스 설정

1. Windows 디바이스가 켜져 있는지 확인합니다.
2. **시작**으로 이동하여 **설정** > **디바이스** > **Bluetooth 및 기타 디바이스**를 선택한 다음 **Bluetooth**를 켭니다.
3. Surface Dial 아래쪽 배터리 삽입 부분을 열고 AAA 배터리 2개가 있는지 확인합니다.
4. 배터리 탭이 다이얼의 아래쪽에 있는 경우 제거합니다.
5. Bluetooth 표시등이 깜박일 때까지 배터리 옆의 작은 삽입 단추를 길게 누릅니다.
6. Windows 디바이스로 돌아가서 **Bluetooth 또는 기타 디바이스 추가**를 선택합니다.
7. **디바이스 추가** 대화 상자를 선택하고 **Bluetooth** > **Surface Dial**을 선택합니다. 이제 Surface Dial이 연결되어 **Bluetooth 및 기타 디바이스** 설정 페이지의 **마우스, 키보드 및 펜**의 디바이스 목록에 추가됩니다.
8. 다이얼을 몇 초 동안 길게 눌러 기본 제공 메뉴가 표시되는지 테스트합니다.
9. 메뉴 (Dial 해야 진동도) 화면에 표시 되지 않으면 이동 Bluetooth 설정으로 장치를 제거 하 고 장치를 다시 연결을 시도 합니다.

> [!NOTE]
> 휠 디바이스는 **휠** 설정을 통해 구성할 수 있습니다.
> 1. **시작** 메뉴에서 **설정**을 선택합니다.
> 2. **디바이스** > **휠**을 선택합니다.    
> ![휠 설정 화면](images/radialcontroller/wheel-settings.png)

이제 이 자습서를 시작할 준비가 되었습니다. 

## <a name="sample-code"></a>샘플 코드
이 자습서에서는 샘플 앱을 사용하여 개념과 기능을 설명할 것입니다.

[windows-appsample-get-started-radialcontroller 샘플](https://aka.ms/appsample-radialcontroller)의 [GitHub](https://github.com/)에서 Visual Studio 샘플과 소스 코드를 다운로드하세요.

1. 녹색 **복제 또는 다운로드** 단추를 선택합니다.  
![리포지토리 복제](images/radialcontroller/wheel-clone.png)
2. GitHub 계정이 있는 경우 **Visual Studio에서 열기**를 선택하여 리포지토리를 로컬 컴퓨터로 복제할 수 있습니다. 
3. GitHub 계정이 없거나 프로젝트의 로컬 복사본만 원한다면 **ZIP 다운로드**를 선택합니다. 최신 업데이트를 다운로드하기 위해 주기적으로 확인해야 합니다.

> [!IMPORTANT]
> 샘플의 대부분 코드는 주석으로 처리됩니다. 이 항목의 각 단계를 진행하면서 코드의 여러 섹션에 대한 주석 처리를 제거하라는 지침이 있을 것입니다. Visual Studio에서 코드 줄을 강조 표시하고 CTRL-K를 누른 다음 CTRL-U를 누릅니다.

## <a name="components-that-support-wheel-functionality"></a>휠 기능을 지원하는 구성 요소

이러한 개체는 UWP 앱을 위한 많은 휠 디바이스 환경을 제공합니다.

| 구성 요소 | 설명 |
| --- | --- |
| [**RadialController** 클래스](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController) 및 관련 항목 | 휠 입력 디바이스 또는 Surface Dial과 같은 액세서리를 나타냅니다. |
| [**IRadialControllerConfigurationInterop**](https://msdn.microsoft.com/library/windows/desktop/mt790709) / [**IRadialControllerInterop**](https://msdn.microsoft.com/library/windows/desktop/mt790711)<br/>여기서는 이 기능을 다루지 않습니다. 자세한 내용은 [Windows 클래식 데스크톱 샘플](https://aka.ms/radialcontrollerclassicsample)을 참조하세요. | UWP 앱과의 상호 운용성을 가능하게 합니다. |

## <a name="step-1-run-the-sample"></a>1단계: 샘플 실행

RadialController 샘플 앱을 다운로드한 후 실행되는지 확인합니다.
1. Visual Studio에서 샘플 프로젝트를 엽니다.
2. **솔루션 플랫폼** 드롭다운을 비-ARM 선택 항목으로 설정합니다.
3. F5를 눌러 컴파일하고, 배포하고, 실행합니다. 

> [!NOTE]
> 또는 **디버그** > **디버깅 시작** 메뉴 항목을 선택하거나 여기에 표시된 **로컬 컴퓨터** 실행 단추를 선택합니다. ![Visual Studio 프로젝트 빌드 단추](images/radialcontroller/wheel-vsrun.png)

앱 창이 열리고 몇 초 동안 시작 화면이 나타난 후 이 초기 화면이 표시됩니다.

![빈 앱](images/radialcontroller/wheel-app-step1-empty.png)

이제 이 자습서의 나머지 부분에서 사용할 기본 UWP 앱이 준비되었습니다. 다음 단계에서 **RadialController** 기능을 추가합니다.

## <a name="step-2-basic-radialcontroller-functionality"></a>2단계: 기본 RadialController 기능

앱을 실행하여 포그라운드에 있을 때, Surface Dial을 길게 눌러 **RadialController** 메뉴를 표시합니다.

아직 앱에 아무런 사용자 지정을 하지 않았으므로, 메뉴에는 상황에 맞는 도구의 기본 집합이 포함되어 있습니다. 

아래 이미지는 기본 메뉴의 두 가지 변형을 보여줍니다. (Windows 데스크톱이 활성화되어 있고 포그라운드에 앱이 없는 기본 시스템 도구, InkToolbar가 있을 때의 추가 수동 입력 도구, 지도 앱을 사용할 때의 매핑 도구를 포함하여 다른 많은 변형이 있습니다.)

| RadialController 메뉴(기본)  | RadialController 메뉴(미디어 재생 기능이 있는 기본)  |
|---|---|
| ![기본 RadialController 메뉴](images/radialcontroller/wheel-app-step2-basic-default.png) | ![음악이 있는 기본 RadialController 메뉴](images/radialcontroller/wheel-app-step2-basic-withmusic.png) |

이제 몇 가지 기본 사용자 지정을 시작하겠습니다.

## <a name="step-3-add-controls-for-wheel-input"></a>3단계: 휠 입력에 대한 컨트롤 추가

먼저 앱을 위한 UI를 추가합니다.

1. MainPage_Basic.xaml 파일을 엽니다.
2. 이 단계의 제목("\<!-- Step 3: Add controls for wheel input -->")으로 표시된 코드를 찾습니다.
3. 다음 줄의 주석 처리를 제거합니다.

    ```xaml
    <Button x:Name="InitializeSampleButton" 
            HorizontalAlignment="Center" 
            Margin="10" 
            Content="Initialize sample" />
    <ToggleButton x:Name="AddRemoveToggleButton"
                    HorizontalAlignment="Center" 
                    Margin="10" 
                    Content="Remove Item"
                    IsChecked="True" 
                    IsEnabled="False"/>
    <Button x:Name="ResetControllerButton" 
            HorizontalAlignment="Center" 
            Margin="10" 
            Content="Reset RadialController menu" 
            IsEnabled="False"/>
    <Slider x:Name="RotationSlider" Minimum="0" Maximum="10"
            Width="300"
            HorizontalAlignment="Center"/>
    <TextBlock Text="{Binding ElementName=RotationSlider, Mode=OneWay, Path=Value}"
                Margin="0,0,0,20"
                HorizontalAlignment="Center"/>
    <ToggleSwitch x:Name="ClickToggle"
                    MinWidth="0" 
                    Margin="0,0,0,20"
                    HorizontalAlignment="center"/>
    ```
이 시점에서는 **샘플 초기화** 단추, 슬라이더 및 토글 스위치만 사용할 수 있습니다. 이후 단계에서 사용되는 다른 단추는 **RadialController** 메뉴 항목을 추가 및 제거하며, 슬라이더 및 토글 스위치에 액세스하는 기능을 제공합니다.

![기본 샘플 앱 UI](images/radialcontroller/wheel-app-step3-basicui.png)

## <a name="step-4-customize-the-basic-radialcontroller-menu"></a>4단계: 기본 RadialController 메뉴 사용자 지정

이제 컨트롤에 대한 **RadialController** 액세스에 필요한 코드를 추가합니다.

1. MainPage_Basic.xaml.cs 파일을 엽니다.
2. 이 단계의 제목("// Step 4: Basic RadialController menu customization")으로 표시된 코드를 찾습니다.
3. 다음 줄의 주석 처리를 제거합니다.
    - [Windows.UI.Input](https://docs.microsoft.com/uwp/api/windows.ui.input) 및 [Windows.Storage.Streams](https://docs.microsoft.com/uwp/api/windows.storage.streams) 유형 참조는 이후 단계의 기능에 사용됩니다.  
    
        ```csharp
        // Using directives for RadialController functionality.
        using Windows.UI.Input;
        ```

    - 글로벌 개체([RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller), [RadialControllerConfiguration](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollerconfiguration), [RadialControllerMenuItem](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem))는 앱 전체에서 사용됩니다.
    
        ```csharp
        private RadialController radialController;
        private RadialControllerConfiguration radialControllerConfig;
        private RadialControllerMenuItem radialControllerMenuItem;
        ```

    - 여기서 단추에 대한 **Click** 처리기를 지정하여 컨트롤을 활성화하고 사용자 지정 **RadialController** 메뉴 항목을 초기화합니다.

        ```csharp
        InitializeSampleButton.Click += (sender, args) =>
        { InitializeSample(sender, args); };
        ``` 

    - 다음은 [RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) 개체를 초기화하고 [RotationChanged](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationChanged) 및 [ButtonClicked](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ButtonClicked) 이벤트에 대한 처리기를 설정합니다.

        ```csharp
        // Set up the app UI and RadialController.
        private void InitializeSample(object sender, RoutedEventArgs e)
        {
            ResetControllerButton.IsEnabled = true;
            AddRemoveToggleButton.IsEnabled = true;

            ResetControllerButton.Click += (resetsender, args) =>
            { ResetController(resetsender, args); };
            AddRemoveToggleButton.Click += (togglesender, args) =>
            { AddRemoveItem(togglesender, args); };

            InitializeController(sender, e);
        }
        ```

    - 여기에서 사용자 정의 RadialController 메뉴 항목을 초기화합니다. [RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) 개체로의 참조를 가져오는 [CreateForCurrentView](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.CreateForCurrentView)를 사용하고, [RotationResolutionInDegrees](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationResolutionInDegrees) 속성을 사용하여 회전 감도를 "1"로 설정하고, [CreateFromFontGlyph](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem.CreateFromFontGlyph)를 사용하여 [RadialControllerMenuItem](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem)을 만들고, 메뉴 항목을 **RadialController** 메뉴 항목 컬렉션에 추가하고, 마지막으로 [SetDefaultMenuItems](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollerconfiguration.setdefaultmenuitems)를 사용하여 기본 메뉴 항목을 지우고 사용자 정의 도구만 남깁니다. 

        ```csharp
        // Configure RadialController menu and custom tool.
        private void InitializeController(object sender, RoutedEventArgs args)
        {
            // Create a reference to the RadialController.
            radialController = RadialController.CreateForCurrentView();
            // Set rotation resolution to 1 degree of sensitivity.
            radialController.RotationResolutionInDegrees = 1;

            // Create the custom menu items.
            // Here, we use a font glyph for our custom tool.
            radialControllerMenuItem =
                RadialControllerMenuItem.CreateFromFontGlyph("SampleTool", "\xE1E3", "Segoe MDL2 Assets");

            // Add the item to the RadialController menu.
            radialController.Menu.Items.Add(radialControllerMenuItem);

            // Remove built-in tools to declutter the menu.
            // NOTE: The Surface Dial menu must have at least one menu item. 
            // If all built-in tools are removed before you add a custom 
            // tool, the default tools are restored and your tool is appended 
            // to the default collection.
            radialControllerConfig =
                RadialControllerConfiguration.GetForCurrentView();
            radialControllerConfig.SetDefaultMenuItems(
                new RadialControllerSystemMenuItemKind[] { });

            // Declare input handlers for the RadialController.
            // NOTE: These events are only fired when a custom tool is active.
            radialController.ButtonClicked += (clicksender, clickargs) =>
            { RadialController_ButtonClicked(clicksender, clickargs); };
            radialController.RotationChanged += (rotationsender, rotationargs) =>
            { RadialController_RotationChanged(rotationsender, rotationargs); };
        }

        // Connect wheel device rotation to slider control.
        private void RadialController_RotationChanged(
            object sender, RadialControllerRotationChangedEventArgs args)
        {
            if (RotationSlider.Value + args.RotationDeltaInDegrees >= RotationSlider.Maximum)
            {
                RotationSlider.Value = RotationSlider.Maximum;
            }
            else if (RotationSlider.Value + args.RotationDeltaInDegrees < RotationSlider.Minimum)
            {
                RotationSlider.Value = RotationSlider.Minimum;
            }
            else
            {
                RotationSlider.Value += args.RotationDeltaInDegrees;
            }
        }

        // Connect wheel device click to toggle switch control.
        private void RadialController_ButtonClicked(
            object sender, RadialControllerButtonClickedEventArgs args)
        {
            ClickToggle.IsOn = !ClickToggle.IsOn;
        }
        ```
4. 이제 앱을 다시 실행합니다.
5. **Initialize radial controller** 단추를 선택합니다.  
6. 앱이 포그라운드에 있을 때, Surface Dial을 길게 눌러 메뉴를 표시합니다. 모든 기본 도구가 제거(**RadialControllerConfiguration.SetDefaultMenuItems** 메서드를 사용하여)되고, 사용자 지정 도구만 남아 있습니다. 사용자 지정 도구가 있는 메뉴는 다음과 같습니다. 

| RadialController 메뉴(사용자 지정)  | 
|---|
| ![사용자 지정 RadialController 메뉴](images/radialcontroller/wheel-app-step3-custom.png) |

7. 사용자 지정 도구를 선택하고 이제 Surface Dial을 통한 상호 작용이 지원되는지 확인합니다.
    * 회전하면 슬라이더가 이동합니다. 
    * 클릭하면 토글을 켜거나 끕니다.

이제 단추를 연결해 보겠습니다.

## <a name="step-5-configure-menu-at-runtime"></a>5단계: 런타임 시 메뉴 구성

이 단계에서 **항목 추가/제거** 및 **RadialController 메뉴 초기화** 단추를 연결하여 메뉴를 동적으로 사용자 지정하는 방법을 설명하겠습니다.

1. MainPage_Basic.xaml.cs 파일을 엽니다.
2. 이 단계 제목("// Step 5: Configure menu at runtime")으로 표시된 코드를 찾습니다.
3. 다음 메서드의 코드에 대한 주석 처리를 제거하고 앱을 다시 실행합니다. 하지만 다음 단계에서 할 것이므로 아무 단추도 누르지 않습니다.  

    ``` csharp
    // Add or remove the custom tool.
    private void AddRemoveItem(object sender, RoutedEventArgs args)
    {
        if (AddRemoveToggleButton?.IsChecked == true)
        {
            AddRemoveToggleButton.Content = "Remove item";
            if (!radialController.Menu.Items.Contains(radialControllerMenuItem))
            {
                radialController.Menu.Items.Add(radialControllerMenuItem);
            }
        }
        else if (AddRemoveToggleButton?.IsChecked == false)
        {
            AddRemoveToggleButton.Content = "Add item";
            if (radialController.Menu.Items.Contains(radialControllerMenuItem))
            {
                radialController.Menu.Items.Remove(radialControllerMenuItem);
                // Attempts to select and activate the previously selected tool.
                // NOTE: Does not differentiate between built-in and custom tools.
                radialController.Menu.TrySelectPreviouslySelectedMenuItem();
            }
        }
    }

    // Reset the RadialController to initial state.
    private void ResetController(object sender, RoutedEventArgs arg)
    {
        if (!radialController.Menu.Items.Contains(radialControllerMenuItem))
        {
            radialController.Menu.Items.Add(radialControllerMenuItem);
        }
        AddRemoveToggleButton.Content = "Remove item";
        AddRemoveToggleButton.IsChecked = true;
        radialControllerConfig.SetDefaultMenuItems(
            new RadialControllerSystemMenuItemKind[] { });
    }
    ```
4. **항목 제거** 단추를 선택하고 다이얼을 길게 눌러 메뉴를 다시 표시합니다.

    이제 메뉴가 도구의 기본 모음을 포함하고 있음을 알 수 있습니다. 3단계에서 사용자 지정 메뉴를 설정하는 동안 모든 기본 도구를 제거하고 사용자 지정 도구를 추가했음을 기억할 것입니다. 메뉴가 빈 모음으로 설정되면, 현재 상황에 대한 기본 항목이 복원됩니다. (기본 도구를 제거하기 전에 사용자 지정 도구를 추가했습니다.)

5. **항목 추가** 단추를 선택하고 다이얼을 길게 누릅니다.

    이제 메뉴에 기본 도구 모음과 사용자 지정 도구가 모두 포함되어 있습니다.

6. **RadialController 메뉴 다시 설정** 단추를 선택하고 다이얼을 길게 누릅니다.

    메뉴가 원래 상태로 돌아갑니다.

## <a name="step-6-customize-the-device-haptics"></a>6단계: 디바이스 촉각 사용자 지정
Surface Dial 및 기타 휠 디바이스는 현재 상호 작용에 해당하는 촉각 피드백(클릭이나 회전에 따라)을 제공할 수 있습니다.

이 단계에서 슬라이더 및 토글 스위치 컨트롤을 연결하고 이를 사용하여 동적으로 촉각 피드백 동작을 지정함으로써 촉각 피드백을 사용자 지정하는 방법을 보겠습니다. 이 예의 경우, 피드백을 사용하기 위해 토글 스위치를 설정해야 하며, 슬라이더 값을 지정하여 클릭 피드백이 얼마나 자주 반복될지 지정합니다. 

> [!NOTE]
> 촉각 피드백은 사용자가 **설정** >  **디바이스** > **휠** 페이지에서 사용하지 않도록 설정할 수 있습니다.

1. App.xaml.cs 파일을 엽니다.
2. 이 단계 제목("Step 6: Customize the device haptics")으로 표시된 코드를 찾습니다.
3. 첫 번째와 세 번째 줄("MainPage_Basic"과 "MainPage")은 주석 처리하고 두 번째 줄("MainPage_Haptics")은 주석 처리를 제거합니다.  

    ``` csharp
    rootFrame.Navigate(typeof(MainPage_Basic), e.Arguments);
    rootFrame.Navigate(typeof(MainPage_Haptics), e.Arguments);
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
    ```
4. MainPage_Haptics.xaml 파일을 엽니다.
5. 이 단계 제목("\<!-- Step 6: Customize the device haptics -->")으로 표시된 코드를 찾습니다.
6. 다음 줄의 주석 처리를 제거합니다. (이 UI 코드는 현재 디바이스에서 어떤 촉각 기능이 지원되는지 나타냅니다.)    

    ```xaml
    <StackPanel x:Name="HapticsStack" 
                Orientation="Vertical" 
                HorizontalAlignment="Center" 
                BorderBrush="Gray" 
                BorderThickness="1">
        <TextBlock Padding="10" 
                    Text="Supported haptics properties:" />
        <CheckBox x:Name="CBDefault" 
                    Content="Default" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsChecked="True" />
        <CheckBox x:Name="CBIntensity" 
                    Content="Intensity" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPlayCount" 
                    Content="Play count" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPlayDuration" 
                    Content="Play duration" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBReplayPauseInterval" 
                    Content="Replay/pause interval" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBBuzzContinuous" 
                    Content="Buzz continuous" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBClick" 
                    Content="Click" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPress" 
                    Content="Press" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBRelease" 
                    Content="Release" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBRumbleContinuous" 
                    Content="Rumble continuous" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
    </StackPanel>
    ```
7. MainPage_Haptics.xaml.cs 파일을 엽니다.
8. 이 단계 제목("Step 6: Haptics customization")으로 표시된 코드를 찾습니다.
9. 다음 줄의 주석 처리를 제거합니다.  

    - [Windows.Devices.Haptics](https://docs.microsoft.com/uwp/api/windows.devices.haptics) 형식 참조는 이후 단계의 기능에 사용됩니다.  
    
        ```csharp
        using Windows.Devices.Haptics;
        ```

    - 여기서 사용자 지정 **RadialController** 메뉴 항목이 선택되었을 때 트리거되는 [ControlAcquired](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ControlAcquired) 이벤트에 대한 처리기를 지정합니다.

        ```csharp
        radialController.ControlAcquired += (rc_sender, args) =>
        { RadialController_ControlAcquired(rc_sender, args); };
        ``` 

    - 다음은 기본 촉각 피드백을 사용하지 않도록 하고 촉각 UI를 초기화하는 [ControlAcquired](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ControlAcquired) 처리기를 정의합니다.

        ```csharp
        private void RadialController_ControlAcquired(
            RadialController rc_sender,
            RadialControllerControlAcquiredEventArgs args)
        {
            // Turn off default haptic feedback.
            radialController.UseAutomaticHapticFeedback = false;

            SimpleHapticsController hapticsController =
                args.SimpleHapticsController;

            // Enumerate haptic support.
            IReadOnlyCollection<SimpleHapticsControllerFeedback> supportedFeedback =
                hapticsController.SupportedFeedback;

            foreach (SimpleHapticsControllerFeedback feedback in supportedFeedback)
            {
                if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.BuzzContinuous)
                {
                    CBBuzzContinuous.IsEnabled = true;
                    CBBuzzContinuous.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Click)
                {
                    CBClick.IsEnabled = true;
                    CBClick.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Press)
                {
                    CBPress.IsEnabled = true;
                    CBPress.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Release)
                {
                    CBRelease.IsEnabled = true;
                    CBRelease.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.RumbleContinuous)
                {
                    CBRumbleContinuous.IsEnabled = true;
                    CBRumbleContinuous.IsChecked = true;
                }
            }

            if (hapticsController?.IsIntensitySupported == true)
            {
                CBIntensity.IsEnabled = true;
                CBIntensity.IsChecked = true;
            }
            if (hapticsController?.IsPlayCountSupported == true)
            {
                CBPlayCount.IsEnabled = true;
                CBPlayCount.IsChecked = true;
            }
            if (hapticsController?.IsPlayDurationSupported == true)
            {
                CBPlayDuration.IsEnabled = true;
                CBPlayDuration.IsChecked = true;
            }
            if (hapticsController?.IsReplayPauseIntervalSupported == true)
            {
                CBReplayPauseInterval.IsEnabled = true;
                CBReplayPauseInterval.IsChecked = true;
            }
        }
        ```

    - [RotationChanged](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationChanged) 및 [ButtonClicked](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ButtonClicked) 이벤트 처리기에서 해당 슬라이더와 토글 단추 컨트롤을 사용자 지정 촉각 피드백에 연결합니다. 

        ```csharp
        // Connect wheel device rotation to slider control.
        private void RadialController_RotationChanged(
            object sender, RadialControllerRotationChangedEventArgs args)
        {
            ...
            if (ClickToggle.IsOn && 
                (RotationSlider.Value > RotationSlider.Minimum) && 
                (RotationSlider.Value < RotationSlider.Maximum))
            {
                SimpleHapticsControllerFeedback waveform = 
                    FindWaveform(args.SimpleHapticsController, 
                    KnownSimpleHapticsControllerWaveforms.BuzzContinuous);
                if (waveform != null)
                {
                    args.SimpleHapticsController.SendHapticFeedback(waveform);
                }
            }
        }

        private void RadialController_ButtonClicked(
            object sender, RadialControllerButtonClickedEventArgs args)
        {
            ...

            if (RotationSlider?.Value > 0)
            {
                SimpleHapticsControllerFeedback waveform = 
                    FindWaveform(args.SimpleHapticsController, 
                    KnownSimpleHapticsControllerWaveforms.Click);

                if (waveform != null)
                {
                    args.SimpleHapticsController.SendHapticFeedbackForPlayCount(
                        waveform, 1.0, 
                        (int)RotationSlider.Value, 
                        TimeSpan.Parse("1"));
                }
            }
        }
        ```
    - 마지막으로, 촉각 피드백에 대한 요청된 **[파형](https://docs.microsoft.com/uwp/api/windows.devices.haptics.simplehapticscontrollerfeedback.Waveform)**(지원되는 경우)을 가져옵니다. 

        ```csharp
        // Get the requested waveform.
        private SimpleHapticsControllerFeedback FindWaveform(
            SimpleHapticsController hapticsController,
            ushort waveform)
        {
            foreach (var hapticInfo in hapticsController.SupportedFeedback)
            {
                if (hapticInfo.Waveform == waveform)
                {
                    return hapticInfo;
                }
            }
            return null;
        }
        ```

이제 앱을 다시 실행하고 슬라이더 값을 변경하고 스위치 상태를 토글하여 사용자 지정 촉각 피드백을 사용해 봅니다.

## <a name="step-7-define-on-screen-interactions-for-surface-studio-and-similar-devices"></a>7단계: Surface Studio 및 유사 디바이스를 위한 화면 상호 작용 정의
Surface Dial을 Surface Studio와 함께 사용하면 더 고유한 사용자 환경을 만들 수 있습니다. 

설명된 기본 길게 누르기 메뉴 환경 외에, Surface Dial을 Surface Studio 화면에 바로 배치할 수도 있습니다. 이를 통해 특수한 "화면 내부" 메뉴가 제공됩니다. 

시스템은 Surface Dial의 연결 위치 및 경계 정보를 감지하여 디바이스에 의한 폐색을 처리하고, 다이얼 외부를 둘러싸는 더 큰 메뉴 버전을 표시합니다. 앱에서는 이 동일한 정보를 사용하여 앱에서 디바이스 및 그 예상된 용도, 사용자의 손 및 arm 배치 등의 존재에 대해 UI에 맞게 사용할 수도 있습니다. 

이 자습서와 함께 제공되는 샘플에는 이러한 몇 가지 기능을 설명하는 약간 더 복잡한 예가 포함되어 있습니다.

이 샘플이 작동하는 것을 보려면(Surface Studio 필요):

1. Visual Studio가 설치된 상태에서 Surface Studio 디바이스에 샘플을 다운로드합니다.
2. Visual Studio에서 샘플을 엽니다.
3. App.xaml.cs 파일을 엽니다.
4. 이 단계 제목("Step 7: Define on-screen interactions for Surface Studio and similar devices")으로 표시된 코드를 찾습니다.
5. 첫 번째와 두 번째 줄("MainPage_Basic"과 "MainPage_Haptics")은 주석 처리하고 세 번째 줄("MainPage")은 주석 처리를 제거합니다.  

    ``` csharp
    rootFrame.Navigate(typeof(MainPage_Basic), e.Arguments);
    rootFrame.Navigate(typeof(MainPage_Haptics), e.Arguments);
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
    ```
6. 앱을 실행하고 두 컨트롤 영역에서 서로 전환할 수 있도록 Surface Dial을 배치합니다.    
![화면 RadialController](images/radialcontroller/wheel-app-step5-onscreen2.png) 

    샘플이 작동하는 비디오:  

    <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Programming-the-Microsoft-Surface-Dial/player" width="600" height="400" allowFullScreen frameBorder="0"></iframe>  

## <a name="summary"></a>요약

축하합니다. *시작 자습서: UWP 앱에서 Surface Dial(및 기타 휠 디바이스) 지원*을 완료했습니다! UWP 앱에서 휠 디바이스를 지원하기 위해 필요한 기본 코드를 소개하고, **RadialController** API가 지원하는 풍부한 사용자 환경의 일부를 제공하는 방법을 알아보았습니다.
