---
ms.assetid: ''
title: Windows 앱에서 Surface Dial(및 기타 휠 디바이스) 지원
description: Windows 앱에 Surface Dial (및 기타 휠 장치)에 대 한 지원을 추가 하는 단계별 자습서입니다.
keywords: 전화 걸기, 방사형, 자습서
ms.date: 03/11/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8edd7a9345f93d3cf0abe76f68c321a977ee2e50
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173377"
---
# <a name="tutorial-support-the-surface-dial-and-other-wheel-devices-in-your-windows-app"></a>자습서: Windows 앱의 Surface 전화 접속 및 기타 휠 장치 지원

![Surface Studio를 사용 하 여 Surface Dial 이미지](images/radialcontroller/dial-pen-studio-600px.png)  
Surface Studio와 surface Pen ( [Microsoft Store](https://www.microsoft.com/store/d/Surface-Dial/925R551SKTGN?icid=Surface_Accessories_ModB_Surface_Dial_103116)에서 구매할 수 있음) *을 사용 하 여 전화를 걸* 수 있습니다.

이 자습서에서는 Surface 전화 접속과 같은 휠 장치에서 지원 되는 사용자 상호 작용 환경을 사용자 지정 하는 방법을 단계별로 안내 합니다. 각 단계에서 설명 하는 다양 한 기능 및 관련 [**RadialController**](/uwp/api/windows.ui.input.radialcontroller) api를 설명 하기 위해 GitHub에서 다운로드할 수 있는 샘플 앱에서 코드 조각을 사용 합니다 ( [샘플 코드](#sample-code)참조).

다음에 중점을 둡니다.
* [**RadialController**](/uwp/api/windows.ui.input.radialcontroller) 메뉴에 표시 되는 기본 제공 도구 지정
* 메뉴에 사용자 지정 도구 추가
* 햅 피드백 제어
* 클릭 상호 작용 사용자 지정
* 회전 상호 작용 사용자 지정

이러한 기능 및 기타 기능을 구현 하는 방법에 대 한 자세한 내용은 [Windows 앱의 Surface 다이얼 작용](windows-wheel-interactions.md)을 참조 하세요.

## <a name="introduction"></a>소개

Surface 다이얼은 사용자가 펜, 터치, 마우스 등의 기본 입력 장치와 함께 사용 하는 경우 생산성을 높일 수 있도록 하는 보조 입력 장치입니다. 보조 입력 장치는 일반적으로 시스템 명령과 기타 추가 컨텍스트, 도구 및 기능에 대 한 액세스를 제공 하기 위해 일반적이 지 않은 손을 사용 하 여 전화 걸기가 사용 됩니다. 

전화 걸기는 세 가지 기본 제스처를 지원 합니다. 
- 누르고 있으면 명령의 기본 제공 메뉴를 표시 합니다.
- 메뉴 항목을 강조 표시 하거나 (메뉴가 활성화 된 경우) 앱에서 현재 작업을 수정 하려면 (메뉴가 활성화 되지 않은 경우) 회전 합니다.
- 강조 표시 된 메뉴 항목을 클릭 하 여 선택 하거나 (메뉴가 활성화 된 경우) 앱에서 명령을 호출 합니다 (메뉴가 활성화 되어 있지 않은 경우).

## <a name="prerequisites"></a>필수 구성 요소

* Windows 10 크리에이터 스 업데이트 이상을 실행 하는 컴퓨터 또는 가상 컴퓨터
* [Visual Studio 2019](https://developer.microsoft.com/windows/downloads)
* [Windows 10 SDK(10.0.15063.0)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* 휠 장치 (현재는 [Surface 전화 접속](https://www.microsoft.com/store/d/Surface-Dial/925R551SKTGN?icid=Surface_Accessories_ModB_Surface_Dial_103116) 만)
* Visual Studio를 사용 하 여 Windows 앱을 개발 하는 경우이 자습서를 시작 하기 전에 다음 항목을 참조 하세요.  
    * [설정하기](../../get-started/get-set-up.md)
    * ["Hello, 세계" 앱 만들기 (XAML)](../../get-started/create-a-hello-world-app-xaml-universal.md)

## <a name="set-up-your-devices"></a>장치 설정

1. Windows 장치가 설정 되어 있는지 확인 합니다.
2. **시작**으로 이동 하 고 **설정**  >  **장치**  >  **Bluetooth & 기타 장치**를 선택한 후 **bluetooth** 를 켜 세요.
3. 화면 전화 접속의 아래쪽을 제거 하 여 배터리 구획을 열고 안에 AAA 배터리가 두 개 있는지 확인 합니다.
4. 전화 접속의 아래쪽에 배터리 탭이 있으면 제거 합니다.
5. Bluetooth 조명이 깜박일 때까지 배터리 옆의 작은 인세트 단추를 길게 누릅니다.
6. Windows 장치로 돌아가서 **Bluetooth 또는 다른 장치 추가**를 선택 합니다.
7. **장치 추가** 대화 상자에서 **Bluetooth**  >  **Surface 전화 걸기**를 선택 합니다. 이제 **Bluetooth & 기타 장치** 설정 페이지에서 **마우스, 키보드, & 펜** 의 장치 목록에 Surface 전화 걸기가 연결 되어 추가 됩니다.
8. 몇 초 동안 눌러 전화 걸기를 테스트 하 여 기본 제공 메뉴를 표시 합니다.
9. 메뉴가 화면에 표시 되지 않는 경우 (다이얼도 진동 해야 함) Bluetooth 설정으로 돌아가서 장치를 제거 하 고 장치를 다시 연결 해 보세요.

> [!NOTE]
> 휠 장치는 **휠** 설정을 통해 구성할 수 있습니다.
> 1. **시작** 메뉴에서 **설정**을 선택 합니다.
> 2. **장치**  >  **휠**을 선택 합니다.    
> ![휠 설정 화면](images/radialcontroller/wheel-settings.png)

이제이 자습서를 시작할 준비가 되었습니다. 

## <a name="sample-code"></a>예제 코드
이 자습서에서는 샘플 앱을 사용 하 여 설명 된 개념과 기능을 보여 줍니다.

[GitHub](https://github.com/) 에서이 Visual Studio 샘플 및 소스 코드 다운로드 [-appsample-radialcontroller 샘플](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-RadialController):

1. 녹색 **복제 또는 다운로드** 단추를 선택 합니다.  
![리포지토리 복제](images/radialcontroller/wheel-clone.png)
2. GitHub 계정이 있는 경우 **Visual Studio에서 열기**를 선택 하 여 로컬 컴퓨터에 리포지토리를 복제할 수 있습니다. 
3. GitHub 계정이 없거나 프로젝트의 로컬 복사본을 원하는 경우 **ZIP 다운로드** 를 선택 합니다. 최신 업데이트를 다운로드 하려면 정기적으로 다시 확인 해야 합니다.

> [!IMPORTANT]
> 샘플에서 대부분의 코드는 주석 처리 되어 있습니다. 이 항목의 각 단계를 진행할 때 코드의 다양 한 섹션에 대 한 주석 처리를 제거 하 라는 메시지가 표시 됩니다. Visual Studio에서 코드 줄을 강조 표시 하 고 CTRL + K, CTRL + U를 차례로 누릅니다.

## <a name="components-that-support-wheel-functionality"></a>휠 기능을 지 원하는 구성 요소

이러한 개체는 Windows 앱에 대 한 대부분의 휠 장치 환경을 제공 합니다.

| 구성 요소 | Description |
| --- | --- |
| [ **RadialController** 클래스](/uwp/api/Windows.UI.Input.RadialController) 및 관련 항목 | 표면 전화 접속과 같은 휠 입력 장치 또는 액세서리를 나타냅니다. |
| [**IRadialControllerConfigurationInterop**](/previous-versions/windows/desktop/api/radialcontrollerinterop/nn-radialcontrollerinterop-iradialcontrollerconfigurationinterop)  /  [ **IRadialControllerInterop**](/previous-versions/windows/desktop/api/radialcontrollerinterop/nn-radialcontrollerinterop-iradialcontrollerinterop)<br/>여기서는이 기능에 대해 다루지 않습니다. 자세한 내용은 [Windows 클래식 데스크톱 샘플](https://github.com/Microsoft/Windows-classic-samples/tree/master/Samples/RadialController)을 참조 하세요. | Windows 앱과의 상호 운용성을 가능 하 게 합니다. |

## <a name="step-1-run-the-sample"></a>1 단계: 샘플 실행

RadialController 샘플 앱을 다운로드 한 후 실행 되는지 확인 합니다.
1. Visual Studio에서 샘플 프로젝트를 엽니다.
2. **솔루션 플랫폼** 드롭다운을 ARM이 아닌 선택 항목으로 설정 합니다.
3. F5 키를 눌러 컴파일, 배포 및 실행 합니다. 

> [!NOTE]
> 또는 **디버그**  >  **디버깅 시작** 메뉴 항목을 선택 하거나 여기에 표시 된 **로컬 컴퓨터** 실행 단추를 선택할 수 있습니다. ![ Visual Studio 빌드 프로젝트 단추](images/radialcontroller/wheel-vsrun.png)

앱 창이 열리고 몇 초 동안 시작 화면이 표시 되 면이 초기 화면이 표시 됩니다.

![빈 앱](images/radialcontroller/wheel-app-step1-empty.png)

이제이 자습서의 나머지 부분에서 사용할 기본 Windows 앱이 있습니다. 다음 단계에서는 **RadialController** 기능을 추가 합니다.

## <a name="step-2-basic-radialcontroller-functionality"></a>2 단계: 기본 RadialController 기능

응용 프로그램을 실행 하 고 있는 응용 프로그램을 사용 하 여 **RadialController** 메뉴를 표시 하려면 Surface 다이얼을 누르고 있습니다.

아직 앱에 대 한 사용자 지정을 수행 하지 않았으므로 메뉴에는 기본 상황별 도구 집합이 포함 되어 있습니다. 

이러한 이미지는 기본 메뉴의 두 가지 변형을 보여 줍니다. (Windows 데스크톱이 활성 상태이 고 응용 프로그램이 전경에 있지 않은 경우에는 기본 시스템 도구를 비롯 한 다른 여러 사용자가 있으며, InkToolbar가 있는 경우 추가 수동 도구를 비롯 하 여 맵 앱을 사용 하는 경우 매핑 도구를 포함 합니다.

| RadialController 메뉴 (기본값)  | RadialController 메뉴 (미디어 재생 시 기본값)  |
|---|---|
| ![기본 RadialController 메뉴](images/radialcontroller/wheel-app-step2-basic-default.png) | ![음악과 함께 기본 RadialController 메뉴](images/radialcontroller/wheel-app-step2-basic-withmusic.png) |

이제 몇 가지 기본 사용자 지정부터 시작 하겠습니다.

## <a name="step-3-add-controls-for-wheel-input"></a>3 단계: 휠 입력을 위한 컨트롤 추가

먼저 앱에 대 한 UI를 추가 해 보겠습니다.

1. MainPage_Basic .xaml 파일을 엽니다.
2. 이 단계의 제목 ("")으로 표시 된 코드를 찾습니다 \<!-- Step 3: Add controls for wheel input --> .
3. 다음 줄의 주석 처리를 제거 합니다.

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
이 시점에서는 **Initialize sample** 단추, slider 및 toggle 스위치만 사용 됩니다. 다른 단추는 이후 단계에서 슬라이더 및 토글 스위치에 대 한 액세스를 제공 하는 **RadialController** 메뉴 항목을 추가 및 제거 하는 데 사용 됩니다.

![기본 샘플 앱 UI](images/radialcontroller/wheel-app-step3-basicui.png)

## <a name="step-4-customize-the-basic-radialcontroller-menu"></a>4 단계: 기본 RadialController 메뉴 사용자 지정

이제 컨트롤에 대 한 **RadialController** 액세스를 사용 하도록 설정 하는 데 필요한 코드를 추가 해 보겠습니다.

1. MainPage_Basic .xaml 파일을 엽니다.
2. 이 단계의 제목으로 표시 된 코드를 찾습니다 ("//4 단계: 기본 RadialController 메뉴 사용자 지정").
3. 다음 줄의 주석 처리를 제거 합니다.
    - 다음 단계에서 사용 되는 기능에는 Windows. n e t. [입력](/uwp/api/windows.ui.input) 및 [windows](/uwp/api/windows.storage.streams) .  
    
        ```csharp
        // Using directives for RadialController functionality.
        using Windows.UI.Input;
        ```

    - 이러한 전역 개체 ([RadialController](/uwp/api/windows.ui.input.radialcontroller), [RadialControllerConfiguration](/uwp/api/windows.ui.input.radialcontrollerconfiguration), [RadialControllerMenuItem](/uwp/api/windows.ui.input.radialcontrollermenuitem))는 앱 전체에서 사용 됩니다.
    
        ```csharp
        private RadialController radialController;
        private RadialControllerConfiguration radialControllerConfig;
        private RadialControllerMenuItem radialControllerMenuItem;
        ```

    - 여기서는 컨트롤을 활성화 하 고 사용자 지정 **RadialController** 메뉴 항목을 초기화 하는 단추에 대 한 **클릭** 처리기를 지정 합니다.

        ```csharp
        InitializeSampleButton.Click += (sender, args) =>
        { InitializeSample(sender, args); };
        ``` 

    - 다음으로, [RadialController](/uwp/api/windows.ui.input.radialcontroller) 개체를 초기화 하 고 [RotationChanged](/uwp/api/windows.ui.input.radialcontroller.RotationChanged) 및 [controls.buttonclicked](/uwp/api/windows.ui.input.radialcontroller.ButtonClicked) 이벤트에 대 한 처리기를 설정 합니다.

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

    - 여기서는 사용자 지정 RadialController 메뉴 항목을 초기화 합니다. [CreateForCurrentView](/uwp/api/windows.ui.input.radialcontroller.CreateForCurrentView) 를 사용 하 여 [RadialController](/uwp/api/windows.ui.input.radialcontroller) 개체에 대 한 참조를 가져오고, RotationResolutionInDegrees 속성을 사용 하 여 회전 민감도를 "1"로 설정 하 고, [RotationResolutionInDegrees](/uwp/api/windows.ui.input.radialcontroller.RotationResolutionInDegrees) 속성을 [사용 하 여](/uwp/api/windows.ui.input.radialcontrollermenuitem.CreateFromFontGlyph) [RadialControllerMenuItem](/uwp/api/windows.ui.input.radialcontrollermenuitem) 를 만들고, **RadialController** 메뉴 항목 컬렉션에 메뉴 항목을 추가 하 고, 마지막으로 [SetDefaultMenuItems](/uwp/api/windows.ui.input.radialcontrollerconfiguration.setdefaultmenuitems) 를 사용 하 여 기본 메뉴 항목을 지우고 사용자 지정 도구를 그대로 둡니다. 

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
4. 이제 앱을 다시 실행 합니다.
5. **방사형 컨트롤러 초기화** 단추를 선택 합니다.  
6. 전경에서 앱을 사용 하 여 화면 전화 걸기를 누르고 메뉴를 표시 합니다. **RadialControllerConfiguration SetDefaultMenuItems** 메서드를 사용 하 여 모든 기본 도구가 제거 되어 사용자 지정 도구만 남게 됩니다. 사용자 지정 도구를 사용 하는 메뉴는 다음과 같습니다. 

| RadialController 메뉴 (사용자 지정)  | 
|---|
| ![사용자 지정 RadialController 메뉴](images/radialcontroller/wheel-app-step3-custom.png) |

7. 사용자 지정 도구를 선택 하 고 이제 Surface 전화 접속을 통해 지원 되는 상호 작용을 사용해 보세요.
    * 회전 동작은 슬라이더를 이동 합니다. 
    * 클릭 하면 토글 설정 또는 해제가 설정 됩니다.

해당 단추를 연결 해 보겠습니다.

## <a name="step-5-configure-menu-at-runtime"></a>5 단계: 런타임에 메뉴 구성

이 단계에서는 **항목 추가/제거** 및 **RadialController 다시 설정 메뉴** 단추를 연결 하 여 메뉴를 동적으로 사용자 지정 하는 방법을 보여 줍니다.

1. MainPage_Basic .xaml 파일을 엽니다.
2. 이 단계의 제목으로 표시 된 코드를 찾습니다 ("//5 단계: 런타임에 메뉴 구성").
3. 다음 방법으로 코드의 주석 처리를 제거 하 고 앱을 다시 실행 하 되 단추를 선택 하지 않습니다 (다음 단계를 위해 저장).  

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
4. **항목 제거** 단추를 선택 하 고 전화 걸기를 누르고 있으면 메뉴가 다시 표시 됩니다.

    이제 메뉴에 기본 도구 컬렉션이 포함 되어 있습니다. 3 단계에서 사용자 지정 메뉴를 설정 하는 동안 모든 기본 도구를 제거 하 고 사용자 지정 도구만 추가 했습니다. 또한 메뉴가 빈 컬렉션으로 설정 된 경우 현재 컨텍스트의 기본 항목이 복원 되는 것을 알 수 있습니다. (기본 도구를 제거 하기 전에 사용자 지정 도구를 추가 했습니다.)

5. **항목 추가** 단추를 선택 하 고 전화 번호를 길게 누릅니다.

    이제 메뉴에는 기본 도구 컬렉션과 사용자 지정 도구가 모두 포함 되어 있습니다.

6. **RadialController 다시 설정 메뉴** 단추를 선택 하 고 전화 번호를 길게 누릅니다.

    메뉴가 원래 상태로 돌아갑니다.

## <a name="step-6-customize-the-device-haptics"></a>6 단계: 장치 haptics 사용자 지정
Surface Dial 및 기타 휠 장치를 사용 하 여 사용자에 게 현재 상호 작용에 해당 하는 햅 피드백을 제공할 수 있습니다 (클릭 또는 회전 기준).

이 단계에서는 슬라이더를 연결 하 고 스위치 컨트롤을 전환 하 고 햅 피드백 동작을 동적으로 지정 하는 데 사용 하 여 햅 피드백을 사용자 지정할 수 있는 방법을 보여 줍니다. 이 예에서는 사용자 의견을 활성화 하기 위해 설정/해제 스위치를 on으로 설정 해야 합니다. 반면 슬라이더 값은 클릭 피드백이 반복 되는 빈도를 지정 합니다. 

> [!NOTE]
> **설정**  >   **장치**  >  **휠** 페이지에서 사용자가 햅 피드백을 사용 하지 않도록 설정할 수 있습니다.

1. App.xaml.cs 파일을 엽니다.
2. 이 단계의 제목으로 표시 된 코드를 찾습니다 ("6 단계: 장치 haptics 사용자 지정").
3. 첫 번째 및 세 번째 줄 ("MainPage_Basic" 및 "MainPage")을 주석 처리 하 고 두 번째 ("MainPage_Haptics")의 주석 처리를 제거 합니다.  

    ``` csharp
    rootFrame.Navigate(typeof(MainPage_Basic), e.Arguments);
    rootFrame.Navigate(typeof(MainPage_Haptics), e.Arguments);
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
    ```
4. MainPage_Haptics .xaml 파일을 엽니다.
5. 이 단계의 제목 ("")으로 표시 된 코드를 찾습니다 \<!-- Step 6: Customize the device haptics --> .
6. 다음 줄의 주석 처리를 제거 합니다. 이 UI 코드는 현재 장치에서 지원 되는 haptics 기능을 나타냅니다.    

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
7. MainPage_Haptics .xaml 파일을 엽니다.
8. 이 단계의 제목으로 표시 된 코드 찾기 ("6 단계: Haptics 사용자 지정")
9. 다음 줄의 주석 처리를 제거 합니다.  

    - [Haptics](/uwp/api/windows.devices.haptics) 형식 참조는 이후 단계의 기능에 사용 됩니다.  
    
        ```csharp
        using Windows.Devices.Haptics;
        ```

    - 여기서는 사용자 지정 **RadialController** 메뉴 항목을 선택할 때 트리거되는 [controlacquired](/uwp/api/windows.ui.input.radialcontroller.ControlAcquired) 이벤트에 대 한 처리기를 지정 합니다.

        ```csharp
        radialController.ControlAcquired += (rc_sender, args) =>
        { RadialController_ControlAcquired(rc_sender, args); };
        ``` 

    - 다음으로, 기본 햅 피드백을 사용 하지 않도록 설정 하 고 haptics UI를 초기화 하는 [Controlacquired](/uwp/api/windows.ui.input.radialcontroller.ControlAcquired) 한 처리기를 정의 합니다.

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

    - [RotationChanged](/uwp/api/windows.ui.input.radialcontroller.RotationChanged) 및 [controls.buttonclicked](/uwp/api/windows.ui.input.radialcontroller.ButtonClicked) 이벤트 처리기에서 해당 슬라이더와 설정/해제 단추 컨트롤을 사용자 지정 haptics에 연결 합니다. 

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
    - 마지막으로 햅 피드백에 대해 요청 된 **[파형](/uwp/api/windows.devices.haptics.simplehapticscontrollerfeedback.Waveform)** (지원 되는 경우)을 가져옵니다. 

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

이제 응용 프로그램을 다시 실행 하 여 슬라이더 값 및 토글 전환 상태를 변경 하 여 사용자 지정 haptics을 사용해 봅니다.

## <a name="step-7-define-on-screen-interactions-for-surface-studio-and-similar-devices"></a>7 단계: Surface Studio 및 비슷한 장치에 대 한 화면 조작 정의
Surface Studio와 연결 된 Surface 전화 접속은 훨씬 더 독특한 사용자 환경을 제공할 수 있습니다. 

설명 된 기본 누름 메뉴 환경 뿐만 아니라 surface를 Surface Studio 화면에 직접 배치할 수도 있습니다. 이렇게 하면 특별 한 "화상" 메뉴를 사용할 수 있습니다. 

시스템은 표면 전화 걸기의 연락처 위치와 범위를 모두 검색 하 여 장치에서 폐색을 처리 하 고, 다이얼 외부에서 래핑하는 더 큰 버전의 메뉴를 표시 합니다. 이 동일한 정보를 사용 하 여 장치 유무 및 사용자의 손 및 arm 배치와 같은 예상 사용에 대 한 UI를 조정 하는 앱 에서도 사용할 수 있습니다. 

이 자습서와 함께 제공 되는 샘플에는 이러한 기능 중 일부를 보여 주는 약간 더 복잡 한 예제가 포함 되어 있습니다.

작업에서이를 확인 하려면 (Surface Studio 필요):

1. Visual Studio가 설치 된 Surface Studio 장치에서 샘플 다운로드
2. Visual Studio에서 샘플 열기
3. App.xaml.cs 파일을 엽니다.
4. 이 단계의 제목으로 표시 된 코드 찾기 ("7 단계: Surface Studio 및 유사한 장치에 대 한 화면 조작 정의")
5. 첫 번째 및 두 번째 줄 ("MainPage_Basic" 및 "MainPage_Haptics")을 주석으로 처리 하 고 세 번째 ("MainPage")의 주석 처리를 제거 합니다.  

    ``` csharp
    rootFrame.Navigate(typeof(MainPage_Basic), e.Arguments);
    rootFrame.Navigate(typeof(MainPage_Haptics), e.Arguments);
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
    ```
6. 앱을 실행 하 고 두 개의 제어 영역에 모두 번갈아 이동 하 여 Surface 다이얼을 배치 합니다.    
![화면 RadialController](images/radialcontroller/wheel-app-step5-onscreen2.png) 

    다음은 실행 중인이 샘플의 비디오입니다.  

    <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Programming-the-Microsoft-Surface-Dial/player" width="600" height="400" allowFullScreen frameBorder="0"></iframe>  

## <a name="summary"></a>요약

축 하 합니다 *. 시작 자습서: Windows 앱의 Surface 전화 걸기 (및 기타 휠 장치) 지원*을 완료 했습니다. Windows 앱에서 휠 장치를 지 원하는 데 필요한 기본 코드와 **RadialController** api에서 지원 되는 다양 한 사용자 환경을 제공 하는 방법을 살펴보았습니다.

## <a name="related-articles"></a>관련된 문서

[Surface Dial 조작](windows-wheel-interactions.md)

### <a name="api-reference"></a>API 참조

- [**RadialController** 클래스](/uwp/api/Windows.UI.Input.RadialController)
- [**RadialControllerButtonClickedEventArgs** 클래스](/uwp/api/Windows.UI.Input.RadialControllerButtonClickedEventArgs)
- [**RadialControllerConfiguration** 클래스](/uwp/api/Windows.UI.Input.RadialControllerConfiguration) 
- [**RadialControllerControlAcquiredEventArgs** 클래스](/uwp/api/Windows.UI.Input.RadialControllerControlAcquiredEventArgs) 
- [**RadialControllerMenu** 클래스](/uwp/api/Windows.UI.Input.RadialControllerMenu) 
- [**RadialControllerMenuItem** 클래스](/uwp/api/Windows.UI.Input.RadialControllerMenuItem) 
- [**RadialControllerRotationChangedEventArgs** 클래스](/uwp/api/Windows.UI.Input.RadialControllerRotationChangedEventArgs) 
- [**RadialControllerScreenContact** 클래스](/uwp/api/Windows.UI.Input.RadialControllerScreenContact) 
- [**RadialControllerScreenContactContinuedEventArgs** 클래스](/uwp/api/Windows.UI.Input.RadialControllerScreenContactContinuedEventArgs) 
- [**RadialControllerScreenContactStartedEventArgs** 클래스](/uwp/api/Windows.UI.Input.RadialControllerScreenContactStartedEventArgs)
- [**RadialControllerMenuKnownIcon** 열거형](/uwp/api/Windows.UI.Input.RadialControllerMenuKnownIcon) 
- [**RadialControllerSystemMenuItemKind** 열거형](/uwp/api/Windows.UI.Input.RadialControllerSystemMenuItemKind) 

### <a name="samples"></a>샘플

#### <a name="topic-samples"></a>토픽 샘플

[RadialController 사용자 지정](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-radialcontroller-customization.zip)

#### <a name="other-samples"></a>기타 샘플
[서적 샘플 강조](https://github.com/Microsoft/Windows-appsample-coloringbook)

[유니버설 Windows 플랫폼 샘플(C# 및 C++)](https://github.com/Microsoft/Windows-universal-samples/tree/b78d95134ce2d57c848e0a8dc339fc362748fb9c/Samples/RadialController)

[Windows 클래식 데스크톱 샘플](https://github.com/Microsoft/Windows-classic-samples/tree/master/Samples/RadialController)