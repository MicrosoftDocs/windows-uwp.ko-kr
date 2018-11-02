---
author: Karl-Bridge-Microsoft
Description: Incorporate speech into your apps using Cortana voice commands, speech recognition, and speech synthesis.
title: Surface Dial 조작
label: Surface Dial interactions
template: detail.hbs
keywords: Surface Dial, Windows 휠, RadialController, Radial controller, 사용자 조작, 입력
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.assetid: e7deb1d6-feeb-471e-9a83-26386d1aaf37
ms.localizationpriority: medium
ms.openlocfilehash: 0a360bf936843450f5646c0e4e03ad9c3bac34d2
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5981524"
---
# <a name="surface-dial-interactions"></a>Surface Dial 조작

![Surface Studio가 있는 Surface Dial 이미지](images/windows-wheel/dial-pen-studio-600px.png)  
*Surface Studio 및 Pen이 있는 Surface Dial*([Microsoft 스토어](https://aka.ms/purchasesurfacedial)에서 구매 가능)

## <a name="overview"></a>개요

Surface Dial 등의 Windows 휠 디바이스는 Windows 및 Windows 앱을 위한 유용하고 독특한 사용자 조작 환경을 가능하게 하는 새로운 범주의 입력 디바이스입니다. 

> [!IMPORTANT]
> 이 항목에서는 특별히 Surface Dial 조작에 대해 설명하지만 해당 정보는 모든 Windows 휠 디바이스에 적용됩니다. 

| 비디오 |   |
| --- | --- |
| <iframe src="https://www.youtube-nocookie.com/embed/WMklcdzcNcU" width="300" height="200" allowFullScreen="true" frameBorder="0"></iframe> | <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Programming-the-Microsoft-Surface-Dial/player" width="300" height="200" allowFullScreen="true" frameBorder="0"></iframe> |
| *Surface Dial 앱 파트너* | *개발자용 Surface Dial* |

*회전* 작업(또는 제스처) 기반의 폼 팩터를 사용하는 Surface Dial은 기본 디바이스의 입력을 보완하는 보조 다중 모달 입력 디바이스로 고안되었습니다. 대부분의 경우에서 주요 손으로 작업을 수행하면서 나머지 손으로 이 디바이스를 조작합니다(예: 펜으로 수동 입력 수행). 즉, 정밀 포인터 입력(예: 터치, 펜 또는 마우스)용으로 고안되지 않았습니다. 

또한 Surface Dial은 *길게 누르기* 작업 및 *클릭* 작업을 모두 지원합니다. 길게 누르기는 한 가지 기능을 제공합니다. 즉, 명령 메뉴가 표시됩니다. 메뉴가 활성화된 경우 메뉴를 통해 회전 및 클릭 입력이 처리됩니다. 그렇지 않은 경우 처리를 위해 앱에 입력이 전달됩니다. 

**모든 Windows 입력 디바이스와 마찬가지로 앱의 기능에 맞게 Surface Dial 조작 환경을 사용자 지정할 수 있습니다.**

> [!TIP]
> Surface Dial 및 새로운 Surface Studio를 함께 사용할 경우 훨씬 더 고유한 사용자 환경이 제공될 수 있습니다.  
>
>설명된 기본 길게 누르기 메뉴 환경 외에, Surface Dial을 Surface Studio 화면에 바로 배치할 수도 있습니다. 이를 통해 특수한 "화면 내부" 메뉴가 제공됩니다. 
>
>시스템은 Surface Dial의 연결 위치 및 경계 정보를 사용하여 디바이스에 의한 폐색을 처리하고, Dial 외부를 둘러싸는 더 큰 메뉴 버전을 표시합니다. 앱에서는 이 동일한 정보를 사용하여 앱에서 디바이스 및 그 예상된 용도, 사용자의 손 및 arm 배치 등의 존재에 대해 UI에 맞게 사용할 수도 있습니다.

| Surface Dial 화면 외부 메뉴 | | Surface Dial 화면 내부 메뉴 |
| --- | --- | --- |
| ![Surface Dial 화면 외부 메뉴](images/windows-wheel/surface-dial-menu-offscreen.png) | | ![Surface Dial 화면 내부 메뉴](images/windows-wheel/surface-dial-menu-onscreen.png) |

## <a name="system-integration"></a>시스템 통합

Surface Dial은 Windows와 밀접하게 통합되며 시스템 볼륨, 스크롤, 확대/축소 및 실행 취소/다시 실행과 같은 메뉴의 기본 제공 도구 집합을 지원합니다.

이러한 기본 제공 도구 모음은 현재 시스템 컨텍스트에 맞게 조정되어 다음을 포함합니다.
- 사용자가 Windows 바탕 화면에 있는 경우 시스템 밝기 도구
- 미디어가 재생되는 경우 이전/다음 트랙 도구

이러한 일반 플랫폼 지원 외에, Surface Dial은 Windows Ink 플랫폼 컨트롤([**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkCanvas) 및 [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbar))과도 긴밀히 통합됩니다.

![Surface Pen이 있는 Surface Dial](images/windows-wheel/dial-and-pen-400px.png)  
*Surface Pen이 있는 Surface Dial*

Surface Dial과 함께 이러한 컨트롤을 사용하면 잉크 특성을 수정하고 잉크 도구 모음의 눈금자 스텐실을 제어하기 위한 추가 기능을 사용할 수 있습니다.

잉크 도구 모음을 사용하는 수동 입력 응용 프로그램에서 Surface Dial 메뉴를 열면 메뉴에는 이제 펜 유형 및 브러시 두께를 제어하기 위한 도구가 포함됩니다. 눈금자가 사용 가능하게 설정되면 디바이스가 눈금자의 위치 및 각도를 제어할 수 있도록 해당 도구가 메뉴에 추가됩니다.

![Windows Ink 도구 모음의 펜 선택 도구가 있는 Surface Dial 메뉴](images/windows-wheel/surface-dial-menu-inktoolbar-pen.png)  
*Windows Ink 도구 모음의 펜 선택 도구가 있는 Surface Dial 메뉴*

![Windows Ink 도구 모음의 스트로크 크기 도구가 있는 Surface Dial 메뉴](images/windows-wheel/surface-dial-menu-inktoolbar-strokesize.png)  
*Windows Ink 도구 모음의 스트로크 크기 도구가 있는 Surface Dial 메뉴*

![Windows Ink 도구 모음의 눈금자 도구가 있는 Surface Dial 메뉴](images/windows-wheel/surface-dial-menu-inktoolbar-ruler.png)  
*Windows Ink 도구 모음의 눈금자 도구가 있는 Surface Dial 메뉴*

## <a name="user-customization"></a>사용자 지정

**Windows 설정 -&gt; 디바이스 -&gt; 휠** 페이지를 통해 Dial 환경의 일부 측면(기본 도구, 진동(또는 촉각 피드백), 쓰는(기본) 손 등)을 사용자 지정할 수 있습니다. 

Surface Dial 사용자 환경을 사용자 지정할 때 항상 정 기능 또는 동작을 사용할 수 있는지와 사용 가능하게 설정되어 있는지를 확인해야 합니다.

## <a name="custom-tools"></a>사용자 지정 도구

다음에서는 Surface Dial 메뉴에 노출되는 도구를 사용자 지정하기 위한 UX 및 개발자 지침을 알아봅니다.

### <a name="ux-guidance"></a>UX 지침

**도구가 현재 상황에 맞는지 확인** 도구가 수행하는 작업과 Surface Dial 조작이 작동하는 방식을 명확히 이해하면 사용자들이 더 빠르게 배우고 작업에 집중하도록 도와줄 수 있습니다.

**앱 도구 수를 최대한 작게 유지합니다.**  
Surface Dial 메뉴에는 7개 항목에 대한 공간이 있습니다. 항목 수가 8개가 넘는 경우 오버플로 플라이아웃에서 사용할 수 있는 도구를 표시하기 위해 Dial을 회전해야 하며, 메뉴를 탐색하고 도구를 찾아 선택하기가 더 어려워집니다.

앱 또는 앱 상황에 맞는 단일 사용자 지정 도구를 제공하는 것이 좋습니다. 이렇게 하면 Surface Dial 메뉴를 활성화하고 도구를 선택하지 않아도 사용자가 수행하는 작업을 기준으로 해당 도구를 설정할 수 있습니다. 

**도구 모음을 동적으로 업데이트합니다.**  
Surface Dial 메뉴 항목은 비활성된 상태를 지원하지 않기 때문에 사용자 상황(현재 보기 또는 포커스가 있는 창)에 따라 동적으로 도구를 추가 및 제거해야 합니다. 도구가 현재의 활동과 관련이 없거나 중복된 경우에는 제거합니다.

> [!IMPORTANT]
> 메뉴에 항목을 추가할 때는 항목이 아직 없는지 확인합니다.

**기본 제공 시스템 볼륨 설정 도구는 제거하지 않도록 합니다.**  
볼륨 조절은 일반적으로 사용자에게 항상 필요합니다. 앱을 사용하는 동안 음악을 들고 있을 수 잇으므로 볼륨 및 다음 트랙 도구를 항상 Surface Dial 메뉴에서 액세스할 수 있어야 합니다. (다음 트랙 도구는 미디어가 재생되는 동안 메뉴에 자동으로 추가됩니다.)

**메뉴 조직과 일관성을 유지합니다.**  
이렇게 하면 사용자가 앱을 사용하면서 사용 가능한 도구를 검색하고 배울 수 있으며 도구를 전환할 때 효율성도 높아집니다.

**기본 제공 아이콘과 일관된 고품질 아이콘을 제공합니다.**  
아이콘은 전문성과 우수성을 전달하고 사용자에게 신뢰를 줄 수 있습니다.
- 고품질 64x64 픽셀 PNG 이미지를 제공합니다(최소 지원 크기: 44x44).
- 투명 배경인지 확인합니다.
- 아이콘이 이미지 대부분을 채워야 합니다.
- 고대비 모드에서 보이도록 흰색 아이콘에 검은색 윤곽을 지정합니다.

|   |   |   |
| --- | --- | --- |
| ![알파 배경의 아이콘](images/windows-wheel/surface-dial-menu-icon1.png) | ![기본 테마 아이콘을 사용하여 휠 메뉴에 표시되는 아이콘](images/windows-wheel/surface-dial-menu-icon2.png) | ![Surface Dial 화면 내부 메뉴](images/windows-wheel/surface-dial-menu-icon3.png) |
| *알파 배경의 아이콘* | *기본 테마를 사용하여 휠 메뉴에 표시되는 아이콘* | *고대비 흰색 테마를 사용하여 휠 메뉴에 표시되는 아이콘* |

**설명을 포함하는 간결한 이름 사용**  
도구 이름이 도구 아이콘과 함께 도구 메뉴에 표시되고 화면 읽기 프로그램에서도 사용됩니다. 
- 이름은 휠 메뉴의 중앙 원 안에 맞게 줄여야 합니다.
- 이름은 기본 동작을 명확히 식별해야 합니다(보조 동작은 암시될 수 있음).
  - 스크롤은 양쪽 회전 방향의 효과를 나타냅니다.
  - 실행 취소는 기본 동작을 지정하지만 다시 실행(보조 작업)은 사용자가 유추하고 쉽게 검색할 수 있습니다.

### <a name="developer-guidance"></a>개발자 참고 자료

포괄적인 [Windows 런타임 API](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController) 집합을 통해 앱의 기능을 보완하도록 Surface Dial 환경을 사용자 지정할 수 있습니다. 

앞서 설명한 대로 기본 Surface Dial 메뉴는 광범위한 기본 시스템 기능(시스템 볼륨, 시스템 밝기, 스크롤, 확대/축소, 실행 취소, 시스템이 진행 중인 오디오 또는 비디오 재생을 감지할 때의 미디어 컨트롤)을 포함하는 기본 제공 도구 모음으로 미리 채워집니다. 그러나 이러한 기본 도구가 앱에 필요한 기능을 제공하지 않을 수도 있습니다. 

다음 섹션에서는 Surface Dial 메뉴에 사용자 지정 도구를 추가하고 노출되는 기본 제공 도구를 지정하는 방법을 설명합니다.

**사용자 지정 도구 추가**

이 예제에서는 회전 및 클릭 이벤트의 입력 데이터를 일부 XAML UI 컨트롤에 전달하는 기본 사용자 지정 도구를 추가합니다.

1. 먼저 XAML에서 UI(슬라이더 및 토글 단추)를 선언합니다.

   ![샘플 앱 UI의 이미지](images/windows-wheel/surface-dial-snippet-customtool1.png)  
   *샘플 앱 UI*

    ```Xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
      </Grid.RowDefinitions>
      <StackPanel x:Name="HeaderPanel" 
        Orientation="Horizontal" 
        Grid.Row="0">
          <TextBlock x:Name="Header"
            Text="RadialController customization sample"
            VerticalAlignment="Center"
            Style="{ThemeResource HeaderTextBlockStyle}"
            Margin="10,0,0,0" />
      </StackPanel>
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center"
        Grid.Row="1">
          <!-- Slider for rotation input -->
          <Slider x:Name="RotationSlider"
            Width="300"
            HorizontalAlignment="Left"/>
          <!-- Switch for click input -->
          <ToggleSwitch x:Name="ButtonToggle"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    ```

2. 그런 다음 코드 숨김 방식으로 Surface Dial 메뉴에 사용자 지정 도구를 추가하 고 [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController) 입력 처리기를 선언합니다. 

   [**CreateForCurrentView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController)를 호출하여 Surface Dial에 대한 [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.CreateForCurrentView) 개체(myController)에 대한 참조를 가져옵니다.

   그런 후 [**RadialControllerMenuItem.CreateFromIcon**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenuItem)을 호출하여 [**RadialControllerMenuItem**](https://msdn.microsoft.com/library/windows/apps/mt759255)(myItem)의 인스턴스를 만듭니다. 

   다음으로 메뉴 항목 컬렉션에 해당 항목을 추가합니다.

   [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ButtonClicked) 개체에 대한 입력 이벤트 처리기([**ButtonClicked**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.RotationChanged) 및 [**RotationChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController))를 선언합니다.

   마지막으로, 이벤트 처리기를 정의합니다.

    ```csharp
    public sealed partial class MainPage : Page
    {
        RadialController myController;

        public MainPage()
        {
            this.InitializeComponent();
            // Create a reference to the RadialController.
            myController = RadialController.CreateForCurrentView();

            // Create an icon for the custom tool.
            RandomAccessStreamReference icon =
              RandomAccessStreamReference.CreateFromUri(
                new Uri("ms-appx:///Assets/StoreLogo.png"));

            // Create a menu item for the custom tool.
            RadialControllerMenuItem myItem =
              RadialControllerMenuItem.CreateFromIcon("Sample", icon);

            // Add the custom tool to the RadialController menu.
            myController.Menu.Items.Add(myItem);

            // Declare input handlers for the RadialController.
            myController.ButtonClicked += MyController_ButtonClicked;
            myController.RotationChanged += MyController_RotationChanged;
        }

        // Handler for rotation input from the RadialController.
        private void MyController_RotationChanged(RadialController sender,
          RadialControllerRotationChangedEventArgs args)
        {
            if (RotationSlider.Value + args.RotationDeltaInDegrees > 100)
            {
                RotationSlider.Value = 100;
                return;
            }
            else if (RotationSlider.Value + args.RotationDeltaInDegrees < 0)
            {
                RotationSlider.Value = 0;
                return;
            }
            RotationSlider.Value += args.RotationDeltaInDegrees;
        }

        // Handler for click input from the RadialController.
        private void MyController_ButtonClicked(RadialController sender,
          RadialControllerButtonClickedEventArgs args)
        {
            ButtonToggle.IsOn = !ButtonToggle.IsOn;
        }
    }
    ```

앱을 실행할 때 Surface Dial을 사용하여 조작합니다. 먼저 길게 눌러 메뉴를 열고 사용자 지정 도구를 선택합니다. 사용자 지정 도구가 활성화되면 Dial을 회전하여 슬라이더 컨트롤을 조정하고, Dial을 클릭하여 스위치를 설정/해제할 수 있습니다.

![Surface Dial 사용자 지정 도구를 사용하여 활성화된 샘플 앱 UI 이미지](images/windows-wheel/surface-dial-snippet-customtool2.png)  
*Surface Dial 사용자 지정 도구를 사용하여 활성화된 샘플 앱 UI*

**기본 제공 도구 지정**

[**RadialControllerConfiguration**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerConfiguration) 클래스를 사용하여 앱에 대한 기본 제공 메뉴 항목의 컬렉션을 사용자 지정할 수 있습니다.

예를 들어 앱에 스크롤 또는 확대/축소 영역이 없고 실행 취소/다시 실행 기능이 필요하지 않으면 메뉴에서 이러한 도구를 제거할 수 있습니다. 이렇게 하면 메뉴에서 앱에 대한 사용자 지정 도구를 추가할 수 있는 공간이 열립니다. 

> [!IMPORTANT] 
> Surface Dial 메뉴에는 하나 이상의 메뉴 항목이 있어야 합니다. 사용자 지정 도구 중 하나를 추가하기 전에 모든 기본 도구를 제거하면 기본 도구가 복원되고 도구가 기본 컬렉션에 추가됩니다.

디자인 지침에 따르면 사용자가 다른 작업을 수행하면서 배경 음악을 재생할 때는 미디어 제어 도구(볼륨 및 이전/다음 트랙)를 제거하지 않는 것이 좋습니다.

여기에서는 볼륨 및 이전/다음 트랙에 대한 미디어 컨트롤만 포함하도록 Surface Dial 메뉴를 구성하는 방법을 보여 줍니다.

```csharp
public MainPage()
{
  ...
  //Remove a subset of the default system tools
  RadialControllerConfiguration myConfiguration = 
  RadialControllerConfiguration.GetForCurrentView();
  myConfiguration.SetDefaultMenuItems(new[] 
  {
    RadialControllerSystemMenuItemKind.Volume,
      RadialControllerSystemMenuItemKind.NextPreviousTrack
  });
}
```

## <a name="custom-interactions"></a>사용자 지정 조작

앞서 설명한 것처럼 Surface Dial은 해당하는 기본 조작이 있는 세 가지 제스처(길게 누르기, 회전, 클릭)를 지원합니다. 

이러한 제스처를 기준으로 하는 모든 사용자 지정 조작은 선택한 작업 또는 도구에 해당됩니다. 

> [!NOTE]
> 조작 환경은 Surface Dial 메뉴의 상태에 따라 달라집니다. 메뉴가 활성화된 경우 메뉴가 입력을 처리하고, 그렇지 않으면 앱이 입력을 처리합니다.

### <a name="press-and-hold"></a>길게 누르기

이 제스처는 Surface Dial 메뉴를 활성화하고 표시하며 이 제스처와 관련된 앱 기능은 없습니다. 

기본적으로 메뉴는 화면 가운데에 표시됩니다. 그러나 메뉴를 잡아서 원하는 위치로 이동할 수 있습니다.

> [!NOTE]
> Surface Studio의 화면에 Surface Dial이 배치되면 메뉴가 Surface Dial의 화면 내부 위치의 가운데에 배치됩니다.

### <a name="rotate"></a>Rotate

Surface Dial은 아날로그 값 또는 컨트롤을 점진적으로 원활하게 조정하는 조작을 위해 회전을 지원하도록 디자인되었습니다.

디바이스가 시계 방향 및 시계 반대 방향으로 회전 가능하며, 개별 거리를 나타내기 위해 촉각 피드백을 제공할 수도 있습니다.

> [!NOTE]
> 촉각 피드백은 **Windows 설정 -&gt; 디바이스 -&gt; 휠** 페이지에서 사용하지 않도록 설정할 수 있습니다.

#### <a name="ux-guidance"></a>UX 지침

**연속 또는 높은 회전 민감도를 갖는 도구의 경우 촉각 피드백을 사용하지 않도록 설정해야 합니다.**

촉각 피드백은 활성 도구의 회전 민감도와 일치합니다. 사용자가 불편함을 느낄 수 있으므로 연속 또는 높은 회전 민감도를 갖는 도구에 대해서는 촉각 피드백을 사용하지 않도록 설정하는 것이 좋습니다. 

**주요 손은 회전 기반 조작에 영향을 미치지 않아야 합니다.**

Surface Dial은 어느 쪽 손을 사용하고 있는지 감지할 수 없으나 사용자가 **Windows 설정 -&gt; 디바이스 -&gt; 펜 및 Windows Ink**에서 쓰는 손(주요 손)을 설정할 수 있습니다.

**모든 회전 조작에 대해 로캘을 고려해야 합니다.**

로캘 및 오른쪽에서 왼쪽 레이아웃에 맞게 조작을 조정하여 고객 만족도를 최대화할 수 있습니다.

전화 메뉴의 기본 제공 도구 및 명령은 회전 기반 조작에 대해 다음 지침을 따릅니다.

|   |   |   |
| --- | --- | --- |
| 왼쪽<br/>위쪽<br/>바깥쪽 | ![Surface Dial 이미지](images/windows-wheel/surface-dial-rotate.png) | 오른쪽<br/>아래쪽<br/>안쪽 |
|   |   |   |

| 개념적 방향 | Surface Dial에 매핑 | 시계 방향 회전 | 시계 반대 방향 회전 |
| --- | --- | --- | --- |
| 수평 | Surface Dial 위쪽 기준으로 왼쪽 및 오른쪽 매핑 | 오른쪽 | 왼쪽 |
| 수직 | Surface Dial 왼쪽 기준으로 위쪽 및 아래쪽 매핑 | 아래쪽 | 위쪽 |
| Z축 | 위쪽/오른쪽에 안쪽으로(또는 가깝게) 매핑<br/>아래쪽/왼쪽에 바깥쪽으로(또는 멀게) 매핑 | 안쪽 | 바깥쪽 |

#### <a name="developer-guidance"></a>개발자 참고 자료

사용자가 디바이스를 회전하면 회전 방향에 상대적으로 델타([**RadialControllerRotationChangedEventArgs.RotationDeltaInDegrees**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.RotationChanged))[**RadialController.RotationChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerRotationChangedEventArgs.RotationDeltaInDegrees)를 기준으로 발생합니다. 데이터의 민감도(또는 해상도)는 [**RadialController.RotationResolutionInDegrees**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.RotationResolutionInDegrees) 속성을 사용하여 설정할 수 있습니다.

> [!NOTE]
> 기본적으로 회전 입력 이벤트는 디바이스가 최소 10도로 회전될 때만 [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController) 개체로 전달됩니다. 각 입력 이벤트로 인해 디바이스가 진동합니다.

일반적으로 회전 해상도가 5도 미만으로 설정된 경우 촉각 피드백을 사용하지 않도록 설정하는 것이 좋습니다. 이렇게 하면 연속 조작이 좀 더 원활해집니다. 

[**RadialController.UseAutomaticHapticFeedback**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.UseAutomaticHapticFeedback) 속성을 설정하여 사용자 지정 도구에 대한 촉각 피드백을 사용하거나 사용하지 않도록 설정할 수 있습니다.

> [!NOTE]
> 볼륨 컨트롤 같은 시스템 도구에 대한 촉각 동작은 재정의할 수 없습니다. 이러한 도구의 경우 휠 설정 페이지에서만 촉각 피드백을 사용하지 않도록 설정할 수 있습니다.

회전 데이터의 해상도를 사용자 지정하고 촉각 피드백을 사용하거나 사용하지 않도록 설정하는 방법의 예는 다음과 같습니다.

```csharp
private void MyController_ButtonClicked(RadialController sender, 
  RadialControllerButtonClickedEventArgs args)
{
  ButtonToggle.IsOn = !ButtonToggle.IsOn;

  if(ButtonToggle.IsOn)
  {
    //high resolution mode
    RotationSlider.LargeChange = 1;
    myController.UseAutomaticHapticFeedback = false;
    myController.RotationResolutionInDegrees = 1;
  }
  else
  {
    //low resolution mode
    RotationSlider.LargeChange = 10;
    myController.UseAutomaticHapticFeedback = true;
    myController.RotationResolutionInDegrees = 10;
  }
}
```

### <a name="click"></a>클릭

Surface Dial을 클릭하는 것은 왼쪽 마우스 단추를 클릭하는 것과 같습니다(디바이스의 회전 상태가 이 작업에는 영향을 미치지 않음).

#### <a name="ux-guidance"></a>UX 지침

**사용자가 결과로부터 쉽게 복구할 수 없으면 작업 또는 명령을 이 제스처에 매칭하지 않도록 합니다.**

Surface Dial을 클릭하는 사용자를 기준으로 앱에 의해 수행된 작업을 되돌릴 수 있어야 합니다. 항상 사용자가 쉽게 앱 뒤로 스택을 통과하고 이전 앱 상태를 복원할 수 있도록 합니다.

음소거/음소거 해제 또는 표시/숨기기 등의 이진 작업은 클릭 제스처를 사용하기에 좋은 사용자 환경을 제공합니다.

**모달 도구는 Surface Dial을 클릭하여 사용하거나 사용하지 않도록 설정하면 안 됩니다.**

일부 앱/도구 모드는 회전에 의존하는 조작과 충돌하거나 이러한 조작을 사용하지 않도록 설정할 수 있습니다. Windows Ink 도구 모음의 눈금자와 같은 도구는 다른 UI 어포던스를 통해 설정 또는 해제되어야 합니다(Ink 도구 모음은 기본 제공 [**ToggleButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.ToggleButton) 컨트롤을 제공함).

모달 도구의 경우 활성 Surface Dial 메뉴 항목을 대상 도구 또는 이전에 선택한 메뉴 항목에 매핑합니다.

#### <a name="developer-guidance"></a>개발자 참고 자료

Surface Dial을 클릭하면 [**RadialController.ButtonClicked**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ButtonClicked) 이벤트가 발생합니다. [**RadialControllerButtonClickedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs)에는 Surface Studio 화면의 Surface Dial 연결 부분에 대한 위치 및 경계 영역을 포함하는 [**Contact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs.Contact) 속성이 포함됩니다. Surface Dial이 화면과 연결되지 않으면 이 속성은 null입니다. 

### <a name="on-screen"></a>화면 내부

앞에서 설명한 대로 Surface Dial을 Surface Studio와 함께 사용하여 Surface Dial을 특별한 화면 내부 모드로 표시할 수 있습니다. 

이 모드에서는 Dial 조작 환경을 앱 이벤트에 통합하고 추가로 사용자 지정할 수 있습니다. Surface Dial 및 Surface Studio에서만 가능한 고유한 환경 예제로는 다음이 있습니다.
- Surface Dial의 위치에 따라 상황에 따른 도구(예: 색상표)를 표시하여 보다 쉽게 찾아서 사용하도록 지원
- Surface Dial을 놓은 UI에 따라 활성 도구 설정
- Surface Dial의 위치에 따라 화면 영역 확대
- 화면 위치에 따라 고유한 게임 조작

#### <a name="ux-guidance"></a>UX 지침

**Surface Dial이 화면 내부에서 감지될 때 앱이 응답해야 합니다.**

시각적 피드백은 앱이 Surface Studio 화면에서 디바이스를 검색했음을 사용자에게 나타내는 데 도움이 됩니다.

**디바이스 위치에 따라 Surface Dial 관련 UI를 조정합니다.**

디바이스(및 사용자의 신체)가 사용자가 디바이스를 놓은 위치에 따라 중요한 UI를 폐색할 수 있습니다.

**사용자 조작에 따라 Surface Dial 관련 UI를 조정합니다.**

하드웨어 폐색 외에도 디바이스를 사용할 때 사용자의 손과 팔이 화면 일부를 폐색할 수도 있습니다. 

폐색된 영역은 디바이스에서 어떤 손을 사용하는지에 따라 다릅니다. 디바이스는 주요 손이 아닌 다른 손으로 주로 사용되도록 디자인되므로 Surface Dial 관련 UI는 사용자가 지정한 반대쪽 손에 맞게 조정되어야 합니다(**Windows 설정 &gt; 디바이스 &gt; 펜 및 Windows Ink &gt; 글을 쓸 때 사용하는 손 선택** 설정).

**조작은 이동이 아닌 Surface Dial 위치에 반응해야 합니다.**

디바이스의 바닥은 정밀 포인팅 디바이스가 아니므로 슬라이드가 아닌 화면에 고정되도록 디자인되어 있습니다. 따라서 사용자가 Surface Dial을 화면을 가로질러 끌지 않고 들어서 놓는 것이 좀 더 일반적입니다.

**화면 위치를 사용하여 사용자 의도 확인**

컨트롤, 캔버스 또는 창에 대한 근접성과 같은 UI 컨텍스트에 따라 활성 도구를 설정하면 작업을 수행하는 데 필요한 단계가 줄어들어 사용자 환경이 개선될 수 있습니다.

#### <a name="developer-guidance"></a>개발자 참고 자료

Surface Dial이 Surface Studio의 디지타이저 화면에 배치되면 [**RadialController.ScreenContactStarted**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ScreenContactStarted) 이벤트가 발생하고 연결 정보([**RadialControllerScreenContactStartedEventArgs.Contact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContactStartedEventArgs.Contact))가 앱에 제공됩니다.

마찬가지로 Surface Studio의 디지타이저 화면에 연결되어 있을 때 Surface Dial을 클릭하면 [**RadialController.ButtonClicked**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ButtonClicked) 이벤트가 발생하고 연결 정보([**RadialControllerButtonClickedEventArgs.Contact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs.Contact))가 앱에 제공됩니다. 

접촉 정보 ([**RadialControllerScreenContact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact))에는 앱의 좌표 공간에서 Surface Dial 중심의 X/Y 좌표([**RadialControllerScreenContact.Position**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact.Position))와 DIP(디바이스 독립적 픽셀) 크기의 경계 직사각형([**RadialControllerScreenContact.Bounds**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact.Bounds))이 포함됩니다. 이 정보는 활성 도구에 대한 컨텍스트를 제공하고 사용자에게 디바이스 관련 시각적 피드백을 제공하는 데 매우 유용합니다.

다음 예제에서는 각각의 하나의 슬라이더와 하나의 토글을 포함하는 4개의 다른 섹션이 있는 기본 앱을 만들었습니다. 그런 후 Surface Dial의 화면 위치를 사용하여 Surface Dial이 제어하는 슬라이더 및 토클 집합을 지정합니다.

1. 먼저 XAML에서 UI(각각 슬라이더와 토글 단추를 포함하는 4개의 섹션)를 선언합니다.

   ![샘플 앱 UI의 이미지](images/windows-wheel/surface-dial-snippet-customtool3.png)  
   *샘플 앱 UI*

   ```xaml 
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
  <Grid.RowDefinitions>
    <RowDefinition Height="Auto"/>
    <RowDefinition Height="*"/>
  </Grid.RowDefinitions>
  <StackPanel x:Name="HeaderPanel" 
        Orientation="Horizontal" 
        Grid.Row="0">
    <TextBlock x:Name="Header"
      Text="RadialController customization sample"
      VerticalAlignment="Center"
      Style="{ThemeResource HeaderTextBlockStyle}"
      Margin="10,0,0,0" />
  </StackPanel>
  <Grid Grid.Row="1" x:Name="RootGrid">
    <Grid.RowDefinitions>
      <RowDefinition Height="*"/>
      <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
      <ColumnDefinition Width="*"/>
      <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    <Grid x:Name="Grid0"
      Grid.Row="0"
      Grid.Column="0">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider0"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle0"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid1"
      Grid.Row="0"
      Grid.Column="1">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider1"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle1"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid2"
      Grid.Row="1"
      Grid.Column="0">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider2"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle2"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid3"
      Grid.Row="1"
      Grid.Column="1">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider3"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle3"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
  </Grid>
</Grid>
   ```

2. 다음은 Surface Dial 화면 위치에 대해 처리기가 정의된 코드 숨김 내용입니다.

```csharp
Slider ActiveSlider;
ToggleSwitch ActiveSwitch;
Grid ActiveGrid;

public MainPage()
{
  ...

  myController.ScreenContactStarted += 
    MyController_ScreenContactStarted;
  myController.ScreenContactContinued += 
    MyController_ScreenContactContinued;
  myController.ScreenContactEnded += 
    MyController_ScreenContactEnded;
  myController.ControlLost += MyController_ControlLost;

  //Set initial grid for Surface Dial input.
  ActiveGrid = Grid0;
  ActiveSlider = RotationSlider0;
  ActiveSwitch = ButtonToggle0;
}

private void MyController_ScreenContactStarted(RadialController sender, 
  RadialControllerScreenContactStartedEventArgs args)
{
  //find grid at contact location, update visuals, selection
  ActivateGridAtLocation(args.Contact.Position);
}

private void MyController_ScreenContactContinued(RadialController sender, 
  RadialControllerScreenContactContinuedEventArgs args)
{
  //if a new grid is under contact location, update visuals, selection
  if (!VisualTreeHelper.FindElementsInHostCoordinates(
    args.Contact.Position, RootGrid).Contains(ActiveGrid))
  {
    ActiveGrid.Background = new 
      SolidColorBrush(Windows.UI.Colors.White);
    ActivateGridAtLocation(args.Contact.Position);
  }
}

private void MyController_ScreenContactEnded(RadialController sender, object args)
{
  //return grid color to normal when contact leaves screen
  ActiveGrid.Background = new 
  SolidColorBrush(Windows.UI.Colors.White);
}

private void MyController_ControlLost(RadialController sender, object args)
{
  //return grid color to normal when focus lost
  ActiveGrid.Background = new 
    SolidColorBrush(Windows.UI.Colors.White);
}

private void ActivateGridAtLocation(Point Location)
{
  var elementsAtContactLocation = 
    VisualTreeHelper.FindElementsInHostCoordinates(Location, 
      RootGrid);

  foreach (UIElement element in elementsAtContactLocation)
  {
    if (element as Grid == Grid0)
    {
      ActiveSlider = RotationSlider0;
      ActiveSwitch = ButtonToggle0;
      ActiveGrid = Grid0;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid1)
    {
      ActiveSlider = RotationSlider1;
      ActiveSwitch = ButtonToggle1;
      ActiveGrid = Grid1;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid2)
    {
      ActiveSlider = RotationSlider2;
      ActiveSwitch = ButtonToggle2;
      ActiveGrid = Grid2;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid3)
    {
      ActiveSlider = RotationSlider3;
      ActiveSwitch = ButtonToggle3;
      ActiveGrid = Grid3;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
  }
}
```

앱을 실행할 때 Surface Dial을 사용하여 조작합니다. 먼저 Surface Studio 화면에 디바이스를 배치합니다. 그러면 앱은 오른쪽 아래 섹션을 감지하고 이 섹션에 연결됩니다(이미지 참조). 그런 후 Surface Dial을 길게 눌러 메뉴를 열고 사용자 지정 도구를 선택합니다. 사용자 지정 도구가 활성화되면 Surface Dial을 회전하여 슬라이더 컨트롤을 조정하고, Surface Dial을 클릭하여 스위치를 설정/해제할 수 있습니다.

![Surface Dial 사용자 지정 도구를 사용하여 활성화된 샘플 앱 UI 이미지](images/windows-wheel/surface-dial-snippet-customtool4.png)  
*Surface Dial 사용자 지정 도구를 사용하여 활성화된 샘플 앱 UI*

## <a name="summary"></a>요약

이 항목에서는 UX가 있는 Surface Dial 입력 디바이스의 개요와 Surface Studio를 사용할 때 화면 외부 시나리오 및 화면 내부 시나리오에 대해 사용자 환경을 사용자 지정하는 방법에 대한 개발자 지침을 제공합니다.

## <a name="feedback"></a>피드백

질문, 제안 및 피드백은 [radialcontroller@microsoft.com](mailto:radialcontroller@microsoft.com)으로 보내주세요.

## <a name="related-articles"></a>관련 문서

### <a name="api-reference"></a>API 참조

- [**RadialController** 클래스](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController)
- [**RadialControllerButtonClickedEventArgs** 클래스](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs)
- [**RadialControllerConfiguration** 클래스](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerConfiguration) 
- [**RadialControllerControlAcquiredEventArgs** 클래스](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerControlAcquiredEventArgs) 
- [**RadialControllerMenu** 클래스](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenu) 
- [**RadialControllerMenuItem** 클래스](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenuItem) 
- [**RadialControllerRotationChangedEventArgs** 클래스](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerRotationChangedEventArgs) 
- [**RadialControllerScreenContact** 클래스](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact) 
- [**RadialControllerScreenContactContinuedEventArgs** 클래스](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContactContinuedEventArgs) 
- [**RadialControllerScreenContactStartedEventArgs** 클래스](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContactStartedEventArgs)
- [**RadialControllerMenuKnownIcon** 열거형](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenuKnownIcon) 
- [**RadialControllerSystemMenuItemKind** 열거형](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerSystemMenuItemKind) 

### <a name="samples"></a>샘플

[유니버설 Windows 플랫폼 샘플(C# 및 C++)](https://go.microsoft.com/fwlink/?linkid=832713)

[Windows 클래식 데스크톱 샘플](https://aka.ms/radialcontrollerclassicsample)