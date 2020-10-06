---
Description: Cortana 음성 명령, 음성 인식 및 음성 합성을 사용 하 여 앱에 음성을 통합 합니다.
title: Surface Dial 조작
label: Surface Dial interactions
template: detail.hbs
keywords: Surface Dial, Windows 휠, RadialController, 방사형 컨트롤러, 사용자 조작, 입력
ms.date: 09/24/2020
ms.topic: article
ms.assetid: e7deb1d6-feeb-471e-9a83-26386d1aaf37
ms.localizationpriority: medium
ms.openlocfilehash: 29a054299b933e523f8594419c4e954c3a0bf1e4
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91749969"
---
# <a name="surface-dial-interactions"></a>Surface Dial 조작

![Surface Studio를 사용 하 여 Surface Dial 이미지](images/windows-wheel/dial-pen-studio-600px.png)  
Surface *Studio 및 Pen을 사용한 Surface 전화 걸기* ( [Microsoft Store](https://www.microsoft.com/store/d/Surface-Dial/925R551SKTGN?icid=Surface_Accessories_ModB_Surface_Dial_103116)에서 구매할 수 있음)

## <a name="overview"></a>개요

Surface 전화 접속과 같은 windows 휠 장치는 Windows 및 Windows 앱에 대해 강력 하 고 고유한 사용자 상호 작용 환경을 호스트 하는 데 사용할 수 있는 새로운 범주의 입력 장치입니다. 

> [!IMPORTANT]
> 이 항목에서는 특히 Surface Dial 상호 작용을 참조 하지만 정보는 모든 Windows 휠 장치에 적용 됩니다. 

:::row:::
   :::column:::
      <iframe src="https://www.youtube-nocookie.com/embed/WMklcdzcNcU" width="300" height="200" allowFullScreen="true" frameBorder="0"></iframe>

      *Surface Dial 앱 파트너*
   :::column-end:::
   :::column:::
      <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Programming-the-Microsoft-Surface-Dial/player" width="300" height="200" allowFullScreen="true" frameBorder="0"></iframe>

      *개발자에 대 한 Surface 전화 걸기*
   :::column-end:::
:::row-end:::

*회전* 작업 (제스처)을 기반으로 하는 폼 팩터를 사용 하 여 Surface 다이얼은 기본 장치에서 입력을 보완 하는 보조 다중 모달 입력 장치로 사용 됩니다. 대부분의 경우 장치는 사용자의 기반이 아닌 손으로 작업을 수행 하는 동안 (예: 펜으로 잉크를 사용 하 여) 작업을 수행 하는 동안 조작 됩니다. 이는 터치, 펜, 마우스 등의 전체 자릿수 포인터 입력을 위해 디자인 되지 않았습니다. 

또한 Surface 다이얼은 *누르기 및 유지* 작업과 *클릭* 작업을 모두 지원 합니다. 길게 누르기는 단일 함수를 포함 합니다. 명령 메뉴를 표시 합니다. 메뉴가 활성화 되어 있으면 회전 및 클릭 입력이 메뉴에 의해 처리 됩니다. 그렇지 않으면 입력이 처리를 위해 앱에 전달 됩니다. 

**모든 Windows 입력 장치와 마찬가지로, 앱의 기능에 맞게 Surface Dial 상호 작용 환경을 사용자 지정 하 고 조정할 수 있습니다.**

> [!TIP]
> Surface 다이얼 및 새 Surface Studio를 함께 사용 하면 훨씬 더 독특한 사용자 환경을 제공할 수 있습니다.  
>
>설명 된 기본 누름 메뉴 환경 뿐만 아니라 surface를 Surface Studio 화면에 직접 배치할 수도 있습니다. 이렇게 하면 특별 한 "화상" 메뉴를 사용할 수 있습니다. 
>
>시스템에서는 Surface 전화 접속의 연결 위치와 범위를 모두 검색 하 여이 정보를 사용 하 여 장치에서 폐색를 처리 하 고, 다이얼 외부에서 래핑하는 더 큰 버전의 메뉴를 표시 합니다. 이 동일한 정보를 사용 하 여 장치 유무 및 사용자의 손 및 arm 배치와 같은 예상 사용에 대 한 UI를 조정 하는 앱 에서도 사용할 수 있습니다.

:::row:::
   :::column:::
      **Surface 다이얼 오프 화면 메뉴**

      ![Surface 다이얼 오프 화면 메뉴](images/windows-wheel/surface-dial-menu-offscreen.png)
   :::column-end:::
   :::column:::
      **화면 전화 접속 화면 메뉴**

      ![화면 전화 접속 화면 메뉴](images/windows-wheel/surface-dial-menu-onscreen.png)
   :::column-end:::
:::row-end:::

## <a name="system-integration"></a>시스템 통합

Surface 다이얼은 Windows와 긴밀 하 게 통합 되며 메뉴에서 시스템 볼륨, 스크롤, 확대/축소, 실행 취소/다시 실행에 대 한 기본 제공 도구 집합을 지원 합니다.

이 기본 제공 도구 컬렉션은 다음을 포함 하도록 현재 시스템 컨텍스트에 적응 합니다.
- 사용자가 Windows 바탕 화면에 있는 경우 시스템 밝기 도구
- 미디어가 재생 중인 이전/다음 트랙 도구

이러한 일반 플랫폼 지원 이외에도 Surface 전화 걸기는 Windows Ink 플랫폼 컨트롤 ([**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 및 [**inktoolbar**](/uwp/api/Windows.UI.Xaml.Controls.InkToolbar))과 긴밀 하 게 통합 됩니다.

![Surface로 전화 접속 surface 펜](images/windows-wheel/dial-and-pen-400px.png)  
*Surface로 전화 접속 surface 펜*

이러한 컨트롤을 Surface 전화 접속과 함께 사용 하면 잉크 특성을 수정 하 고 잉크 도구 모음의 눈금자 스텐실을 제어 하기 위한 추가 기능을 사용할 수 있습니다.

잉크 도구 모음을 사용 하는 잉크 응용 프로그램에서 Surface Dial 메뉴를 열면 이제 메뉴에 펜 종류와 브러시 두께를 제어 하는 도구가 포함 됩니다. 눈금자를 사용 하는 경우 장치에서 눈금자의 위치와 각도를 제어할 수 있는 해당 도구가 메뉴에 추가 됩니다.

![Windows 잉크 도구 모음에 대 한 펜 선택 도구를 사용 하는 Surface 다이얼 메뉴](images/windows-wheel/surface-dial-menu-inktoolbar-pen.png)  
*Windows 잉크 도구 모음에 대 한 펜 선택 도구를 사용 하는 Surface 다이얼 메뉴*

![Windows 잉크 도구 모음에 대 한 스트로크 크기 도구를 사용 하는 Surface 다이얼 메뉴](images/windows-wheel/surface-dial-menu-inktoolbar-strokesize.png)  
*Windows 잉크 도구 모음에 대 한 스트로크 크기 도구를 사용 하는 Surface 다이얼 메뉴*

![Windows 잉크 도구 모음에 대 한 눈금자 도구를 사용 하는 Surface 전화 접속 메뉴](images/windows-wheel/surface-dial-menu-inktoolbar-ruler.png)  
*Windows 잉크 도구 모음에 대 한 눈금자 도구를 사용 하는 Surface 전화 접속 메뉴*

## <a name="user-customization"></a>사용자 지정

사용자는 Windows 설정-기본 도구, 진동 (또는 햅 피드백) 및 쓰기 (또는 기준)를 비롯 한 **Windows 설정 > 장치 > 휠** 페이지를 통해 전화 접속 환경의 일부 측면을 사용자 지정할 수 있습니다. 

Surface 전화 접속 사용자 환경을 사용자 지정 하는 경우 항상 특정 기능 또는 동작을 사용 하 고 사용자가 사용할 수 있도록 해야 합니다.

## <a name="custom-tools"></a>사용자 지정 도구

여기서는 Surface Dial 메뉴에 노출 되는 도구를 사용자 지정 하기 위한 UX 및 개발자 지침을 모두 설명 합니다.

### <a name="ux-guidance-for-custom-tools"></a>사용자 지정 도구에 대 한 UX 지침

**현재 컨텍스트에 해당 하는 도구를 확인 합니다** . 도구에서 수행 하는 작업과 표면 다이얼 작용의 작동 원리를 명확 하 고 직관적으로 지정 하면 사용자가 신속 하 게 학습 하 고 작업에 집중 하는 데 도움이 됩니다.

**가능한 한 많은 앱 도구 수 최소화**  
Surface 전화 접속 메뉴에는 7 개 항목에 대 한 공간이 있습니다. 항목이 8 개 이상 있는 경우 사용자는 전화를 사용 하 여 오버플로 플라이 아웃에서 사용할 수 있는 도구를 확인 하 여 메뉴를 탐색 하 고 도구를 검색 하 고 선택 하기 어렵게 만듭니다.

앱 또는 앱 컨텍스트에는 단일 사용자 지정 도구를 제공 하는 것이 좋습니다. 이렇게 하면 사용자가 Surface 전화 접속 메뉴를 활성화 하 고 도구를 선택할 필요 없이 사용자가 수행 하는 작업에 따라 해당 도구를 설정할 수 있습니다. 

**도구 컬렉션을 동적으로 업데이트**  
Surface Dial 메뉴 항목은 사용 하지 않도록 설정 된 상태를 지원 하지 않으므로 사용자 컨텍스트 (현재 보기 또는 포커스가 있는 창)에 따라 도구 (기본 제공, 기본 도구 포함)를 동적으로 추가 하 고 제거 해야 합니다. 도구가 현재 작업과 관련이 없거나 중복 되는 경우 제거 합니다.

> [!IMPORTANT]
> 메뉴에 항목을 추가할 때 항목이 아직 없는지 확인 합니다.

**기본 제공 시스템 볼륨 설정 도구 제거 안 함**  
볼륨 조절은 일반적으로 사용자에게 항상 필요합니다. 앱을 사용 하는 동안 음악을 수신 하는 동안에는 항상 Surface Dial 메뉴에서 볼륨 및 다음 트랙 도구에 액세스할 수 있습니다. 미디어를 재생 하는 경우 다음 트랙 도구가 메뉴에 자동으로 추가 됩니다.

**메뉴 구성과 일치**  
이렇게 하면 사용자가 앱을 사용할 때 사용할 수 있는 도구를 검색 하 고 학습할 수 있으며 도구를 전환할 때 효율성을 향상 시킬 수 있습니다.

**기본 제공 아이콘과 일관 된 고품질 아이콘 제공**  
아이콘은 사용자의 전문성과 영감 및 신뢰를 전달할 수 있습니다.
- 고품질 64 x 64 픽셀 PNG 이미지를 제공 합니다 (44 x 44은 지원 되는 최소 크기).
- 배경이 투명 한지 확인
- 아이콘은 대부분의 이미지를 채워야 합니다.
- 고대비 모드에서는 흰색 아이콘이 검정 윤곽선으로 표시 되어야 합니다.

:::row:::
   :::column:::
      ![알파 배경을 사용 하는 아이콘](images/windows-wheel/surface-dial-menu-icon1.png)

      *알파 배경을 사용 하는 아이콘*
   :::column-end:::
   :::column:::
      ![기본 테마 아이콘을 사용하여 휠 메뉴에 표시되는 아이콘](images/windows-wheel/surface-dial-menu-icon2.png)

      *기본 테마가 있는 휠 메뉴에 표시 되는 아이콘*
   :::column-end:::
   :::column:::
      ![화면 전화 접속 화면 메뉴](images/windows-wheel/surface-dial-menu-icon3.png)

      *고대비 흰색 테마를 사용 하 여 휠 메뉴에 표시 되는 아이콘*
   :::column-end:::
:::row-end:::

**간결한 이름 및 설명이 포함 된 이름 사용**  
도구 메뉴에 도구 아이콘과 함께 도구 이름이 표시 되 고 화면 판독기 에서도 사용 됩니다. 
- 휠 메뉴의 중앙 원 안에 맞추기 위해 이름을 짧게 지정 해야 합니다.
- 이름으로 기본 작업을 명확 하 게 식별 해야 합니다 (보완 작업이 암시 될 수 있음).
  - Scroll은 두 회전 방향의 효과를 나타냅니다.
  - 실행 취소는 기본 작업을 지정 하지만 사용자가 다시 실행 (보완 작업)을 유추 하 여 쉽게 찾을 수 있습니다.

### <a name="developer-guidance"></a>개발자 지침

포괄적인 [Windows 런타임 api](/uwp/api/Windows.UI.Input.RadialController)집합을 통해 앱의 기능을 보완 하기 위해 Surface Dial 환경을 사용자 지정할 수 있습니다. 

앞에서 설명한 대로 기본 Surface 전화 접속 메뉴는 시스템에서 진행 중인 오디오나 비디오 재생을 검색할 때 다양 한 기본 시스템 기능 (시스템 볼륨, 시스템 밝기, 스크롤, 확대/축소, 실행 취소 및 미디어 제어)을 포함 하는 기본 제공 도구 집합으로 미리 채워져 있습니다. 그러나 이러한 기본 도구는 앱에 필요한 기능을 제공 하지 않을 수 있습니다. 

다음 섹션에서는 Surface Dial 메뉴에 사용자 지정 도구를 추가 하 고 노출 되는 기본 제공 도구를 지정 하는 방법을 설명 합니다.

[RadialController 사용자 지정](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-radialcontroller-customization.zip)에서이 샘플의 보다 강력한 버전을 다운로드 합니다.

**사용자 지정 도구 추가**

이 예제에서는 회전 및 클릭 이벤트의 입력 데이터를 일부 XAML UI 컨트롤에 전달 하는 기본 사용자 지정 도구를 추가 합니다.

1. 먼저 XAML에서 UI (슬라이더 및 토글 단추만)를 선언 합니다.

   ![샘플 앱 UI 이미지](images/windows-wheel/surface-dial-snippet-customtool1.png)  
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

2. 그런 다음 코드 숨김으로 사용자 지정 도구를 Surface Dial 메뉴에 추가 하 고 [**RadialController**](/uwp/api/Windows.UI.Input.RadialController) 입력 처리기를 선언 합니다. 

   [**CreateForCurrentView**](/uwp/api/windows.ui.input.radialcontroller.createforcurrentview)를 호출 하 여 [**RadialController**](/uwp/api/Windows.UI.Input.RadialController) 개체에 대 한 참조를 Surface Dial (mycontroller)에 가져옵니다.

   그런 다음 [**RadialControllerMenuItem. CreateFromIcon**](/uwp/api/windows.ui.input.radialcontrollermenuitem.createfromicon)를 호출 하 여 [**RadialControllerMenuItem**](/uwp/api/Windows.UI.Input.RadialControllerMenuItem) (myitem)의 인스턴스를 만듭니다. 

   그런 다음 해당 항목을 메뉴 항목의 컬렉션에 추가 합니다.

   [**RadialController**](/uwp/api/Windows.UI.Input.RadialController) 개체에 대 한 입력 이벤트 처리기 ([**controls.buttonclicked**](/uwp/api/windows.ui.input.radialcontroller.buttonclicked) 및 [**RotationChanged**](/uwp/api/windows.ui.input.radialcontroller.rotationchanged))를 선언 합니다.

   마지막으로 이벤트 처리기를 정의 합니다.

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

앱을 실행할 때 Surface 다이얼을 사용 하 여 상호 작용 합니다. 먼저를 길게 눌러 메뉴를 열고 사용자 지정 도구를 선택 합니다. 사용자 지정 도구를 활성화 한 후에는 전화를 돌려 슬라이더 컨트롤을 조정 하 고 전화 걸기를 클릭 하 여 스위치를 설정/해제할 수 있습니다.

![Surface Dial 사용자 지정 도구를 사용 하 여 활성화 된 샘플 앱 UI 이미지](images/windows-wheel/surface-dial-snippet-customtool2.png)  
*Surface Dial 사용자 지정 도구를 사용 하 여 활성화 된 샘플 앱 UI*

**기본 제공 도구 지정**

[**RadialControllerConfiguration**](/uwp/api/Windows.UI.Input.RadialControllerConfiguration) 클래스를 사용 하 여 앱에 대 한 기본 제공 메뉴 항목의 컬렉션을 사용자 지정할 수 있습니다.

예를 들어 앱에 스크롤 또는 확대/축소 영역이 없고 실행 취소/다시 실행 기능이 필요 하지 않은 경우 메뉴에서 이러한 도구를 제거할 수 있습니다. 그러면 메뉴의 공간이 열리며 앱에 대 한 사용자 지정 도구를 추가 합니다. 

> [!IMPORTANT] 
> Surface 전화 접속 메뉴에는 하나 이상의 메뉴 항목이 있어야 합니다. 사용자 지정 도구 중 하나를 추가 하기 전에 모든 기본 도구가 제거 되 면 기본 도구가 복원 되 고 도구가 기본 컬렉션에 추가 됩니다.

디자인 지침에 따라 사용자가 다른 작업을 수행 하는 동안 백그라운드 음악이 재생 되는 경우가 많으므로 미디어 컨트롤 도구 (볼륨 및 이전/다음 트랙)를 제거 하지 않는 것이 좋습니다.

여기서는 볼륨 및 다음/이전 트랙에 미디어 컨트롤만 포함 하도록 Surface 전화 접속 메뉴를 구성 하는 방법을 보여 줍니다.

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

## <a name="custom-interactions"></a>사용자 지정 상호 작용

앞서 언급 했 듯이 Surface 전화 걸기는 해당 하는 기본 상호 작용을 사용 하 여 세 가지 제스처 (길게 누르기, 회전, 클릭)를 지원 합니다. 

이러한 제스처를 기반으로 하는 사용자 지정 상호 작용이 선택한 작업 또는 도구에 적합 한지 확인 합니다. 

> [!NOTE]
> 상호 작용 환경은 Surface 전화 접속 메뉴의 상태에 따라 달라 집니다. 메뉴가 활성화 된 경우에는 입력을 처리 합니다. 그렇지 않으면 앱이 수행 합니다.

### <a name="press-and-hold"></a>길게 누르기

이 제스처는 화면 다이얼 메뉴를 활성화 하 고 표시 하며,이 제스처와 연결 된 앱 기능은 없습니다. 

기본적으로 메뉴는 사용자의 화면 가운데에 표시 됩니다. 그러나 사용자는이를 잡아 원하는 위치에서 이동할 수 있습니다.

> [!NOTE]
> Surface Studio 화면에 Surface 전화 걸기가 배치 되 면 메뉴는 화면 다이얼의 화면에 있는 위치를 중심으로 합니다.

### <a name="rotate"></a>회전

Surface 다이얼은 주로 아날로그 값 또는 컨트롤에 대 한 부드러운 증분 조정을 포함 하는 상호 작용에 대 한 회전을 지원 하도록 설계 되었습니다.

장치는 시계 방향으로 시계 반대 방향으로 회전 될 수 있으며, 햅 피드백을 제공 하 여 불연속 거리를 나타낼 수도 있습니다.

> [!NOTE]
> 햅 피드백은 **Windows 설정-> 장치-> 휠** 페이지에서 사용자가 사용 하지 않도록 설정할 수 있습니다.

#### <a name="ux-guidance-for-custom-interactions"></a>사용자 지정 상호 작용에 대 한 UX 지침

**연속 또는 높은 회전 민감도를 가진 도구는 햅 피드백을 사용 하지 않도록 설정 해야 합니다.**

햅 피드백은 활성 도구의 회전 민감도와 일치 합니다. 사용자 환경이 불편을 얻을 수 있으므로 연속 또는 높은 회전 민감도를 사용 하는 도구에 대 한 햅 피드백을 비활성화 하는 것이 좋습니다. 

**중심은 회전 기반 상호 작용에 영향을 주지 않습니다.**

Surface Dial에서 어떤 손을 사용 중인지 검색할 수 없지만, 사용자는 windows **설정-> 장치 > 펜 & Windows Ink**에서 쓰기 (또는 기준)를 설정할 수 있습니다.

**모든 회전 상호 작용에 대해 로캘을 고려해 야 합니다.**

Accomodating를 통해 고객 만족도를 극대화 하 고 로캘 및 오른쪽에서 왼쪽으로의 레이아웃에 대 한 상호 작용을 조정 합니다.

전화 걸기 메뉴의 기본 제공 도구 및 명령은 회전 기반 상호 작용에 대 한 다음 지침을 따릅니다.

:::row:::
   :::column:::
      왼쪽

      위로

      아웃 
   :::column-end:::
   :::column span="2":::
      ![Surface 전화 접속 이미지](images/windows-wheel/surface-dial-rotate.png)
   :::column-end:::
   :::column:::
      오른쪽

      아래로

      In(다음 안에)
   :::column-end:::
:::row-end:::

| 개념적 방향 | Surface 전화 접속에 매핑 | 시계 방향 회전 | 시계 반대 방향 회전 |
| --- | --- | --- | --- |
| 수평적 크기 조정 | 화면 전화 접속의 위쪽을 기준으로 하는 왼쪽 및 오른쪽 매핑 | 오른쪽 | 왼쪽 |
| Vertical | Surface 다이얼의 왼쪽을 기준으로 하는 위쪽 및 아래쪽 매핑 | 아래로 | 위로 |
| Z 축 | 위쪽/오른쪽으로 매핑됨 (또는 가까이)<br/>아래쪽/왼쪽으로 매핑됨 (또는 그 이상) | In(다음 안에) | 아웃 |

#### <a name="developer-guidance"></a>개발자 지침

사용자가 장치를 회전할 때 [**RadialController. RotationChanged**](/uwp/api/windows.ui.input.radialcontroller.rotationchanged) 이벤트는 회전 방향에 상대적인 델타 ([**RadialControllerRotationChangedEventArgs**](/uwp/api/windows.ui.input.radialcontrollerrotationchangedeventargs.rotationdeltaindegrees))를 기반으로 발생 합니다. [**RadialController. RotationResolutionInDegrees**](/uwp/api/windows.ui.input.radialcontroller.rotationresolutionindegrees) 속성을 사용 하 여 데이터의 민감도 또는 해상도를 설정할 수 있습니다.

> [!NOTE]
> 기본적으로 회전 입력 이벤트는 장치가 최소 10도 회전 된 경우에만 [**RadialController**](/uwp/api/Windows.UI.Input.RadialController) 개체로 전달 됩니다. 각 입력 이벤트로 인해 장치가 진동 됩니다.

일반적으로 회전 해상도가 5도 미만으로 설정 된 경우 햅 피드백을 사용 하지 않도록 설정 하는 것이 좋습니다. 이를 통해 지속적인 상호 작용에 대 한 원활한 환경을 제공 합니다. 

[**UseAutomaticHapticFeedback**](/uwp/api/windows.ui.input.radialcontroller.useautomatichapticfeedback) 속성을 설정 하 여 사용자 지정 도구에 대해 햅 피드백을 사용 하거나 사용 하지 않도록 설정할 수 있습니다.

> [!NOTE]
> 볼륨 컨트롤 등의 시스템 도구에 대 한 햅 동작을 재정의할 수 없습니다. 이러한 도구에 대 한 햅 피드백은 휠 설정 페이지의 사용자만 비활성화할 수 있습니다.

다음은 회전 데이터의 해상도를 사용자 지정 하 고 햅 피드백을 사용 하거나 사용 하지 않도록 설정 하는 방법의 예입니다.

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

### <a name="click"></a>리본 메뉴에서

Surface Dial을 클릭하는 것은 왼쪽 마우스 단추를 클릭하는 것과 같습니다(디바이스의 회전 상태가 이 작업에는 영향을 미치지 않음).

#### <a name="ux-guidance"></a>UX 지침

**사용자가 결과에서 쉽게 복구할 수 없는 경우 작업 또는 명령을이 제스처에 매핑하지 마세요.**

사용자를 기준으로 하 여 앱에서 수행 하는 모든 작업은 이동 전화를 클릭 하 여 해독 해야 합니다. 항상 사용자가 앱 백 스택을 쉽게 트래버스 하 고 이전 앱 상태를 복원할 수 있도록 합니다.

음소거/음소거 해제 또는 표시/숨기기와 같은 이진 작업은 클릭 제스처로 좋은 사용자 환경을 제공 합니다.

**표면 전화 걸기를 클릭 하 여 모달 도구를 사용 하거나 사용 하지 않도록 설정 하면 안 됩니다.**

일부 앱/도구 모드는 회전을 사용 하는 상호 작용과 충돌 하거나 사용 하지 않도록 설정할 수 있습니다. Windows 잉크 도구 모음의 눈금자와 같은 도구는 다른 UI affordances 설정 하거나 해제 해야 합니다. 잉크 도구 모음은 기본 제공 [**ToggleButton**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton) 컨트롤을 제공 합니다.

모달 도구의 경우 활성 Surface 전화 접속 메뉴 항목을 대상 도구나 이전에 선택한 메뉴 항목에 매핑합니다.

#### <a name="developer-guidance"></a>개발자 지침

Surface 전화 걸기가 클릭 되 면 [**RadialController. controls.buttonclicked**](/uwp/api/windows.ui.input.radialcontroller.buttonclicked) 이벤트가 발생 합니다. [**RadialControllerButtonClickedEventArgs**](/uwp/api/Windows.UI.Input.RadialControllerButtonClickedEventArgs) 에는 surface Studio 화면에서 Surface 전화 접속 연락처의 위치 및 경계 영역을 포함 하는 [**contact**](/uwp/api/windows.ui.input.radialcontrollerbuttonclickedeventargs.contact) 속성이 포함 되어 있습니다. Surface 전화 접속이 화면과 접촉 되지 않은 경우이 속성은 null입니다. 

### <a name="on-screen"></a>화면에

앞에서 설명한 대로 surface 다이얼을 Surface Studio와 함께 사용 하 여 화면 다이얼 메뉴를 특수 화면 모드로 표시할 수 있습니다. 

이 모드에서 응용 프로그램을 사용 하 여 전화 접속 상호 작용 환경을 더욱 통합 하 고 사용자 지정할 수 있습니다. Surface Dial 및 Surface Studio에서 유일 하 게 사용할 수 있는 고유한 환경의 예는 다음과 같습니다.

- Surface Dial의 위치에 따라 상황에 따른 도구(예: 색상표)를 표시하여 보다 쉽게 찾아서 사용하도록 지원
- Surface 다이얼이 배치 되는 UI를 기반으로 활성 도구 설정
- Surface 전화 접속의 위치를 기준으로 화면 영역 확대
- 화면 위치에 따른 고유한 게임 상호 작용

#### <a name="ux-guidance-for-on-screen-interactions"></a>화면 간 상호 작용에 대 한 UX 지침

**화면 전화 걸기가 화면에 검색 되 면 앱이 응답 해야 함**

시각적 피드백은 앱이 Surface Studio 화면에서 장치를 검색 했음을 사용자에 게 알리는 데 도움이 됩니다.

**장치 위치에 따라 Surface 전화 접속 관련 UI 조정**

사용자가 장치를 배치 하는 위치에 따라 장치 및 사용자의 본문이 중요 한 UI를 려 수 있습니다.

**사용자 상호 작용에 따라 Surface 전화 접속 관련 UI 조정**

하드웨어 폐색 외에도 장치를 사용할 때 사용자의 손을 및 arm이 화면을 려 수 있습니다.

폐색 영역은 장치와 함께 사용 되는 손을 따라 달라 집니다. 장치를 주로 사용 하지 않는 방식으로 사용 하도록 설계 되었으므로 Surface 전화 접속 관련 UI는 사용자가 지정 하는 반대 방향으로 조정 해야 합니다 (windows**설정 > 장치 > Pen & Windows 잉크 > 설정으로 작성할 손을 선택** ).

**상호 작용은 이동 대신 표면 전화 접속 위치에 응답 해야 합니다.**

장치의 피트는 전체 자릿수 포인팅 장치가 아니므로 슬라이드가 아닌 화면에 맞게 디자인 되었습니다. 따라서 사용자가 화면을 가로질러 이동 하는 대신 Surface 다이얼을 리프트 하 고 저장 하는 것이 더 일반적입니다.

**화면 위치를 사용 하 여 사용자 의도 확인**

컨트롤, 캔버스 또는 창과 근접 한 UI 컨텍스트에 따라 활성 도구를 설정 하면 작업을 수행 하는 데 필요한 단계를 줄임으로써 사용자 환경을 향상 시킬 수 있습니다.

#### <a name="developer-guidance"></a>개발자 지침

Surface Studio의 디지타이저 화면에 Surface 다이얼이 배치 되 면 [**RadialController**](/uwp/api/windows.ui.input.radialcontroller.screencontactstarted) 이벤트가 발생 하 고 연락처 정보 ([**RadialControllerScreenContactStartedEventArgs**](/uwp/api/windows.ui.input.radialcontrollerscreencontactstartedeventargs.contact))가 앱에 제공 됩니다.

마찬가지로 Surface Studio의 디지타이저 화면에 연락할 때 Surface 다이얼을 클릭 하면 [**RadialController controls.buttonclicked**](/uwp/api/windows.ui.input.radialcontroller.buttonclicked) 이벤트가 발생 하 고 연락처 정보 ([**RadialControllerButtonClickedEventArgs**](/uwp/api/windows.ui.input.radialcontrollerbuttonclickedeventargs.contact))가 앱에 제공 됩니다. 

연락처 정보 ([**RadialControllerScreenContact**](/uwp/api/Windows.UI.Input.RadialControllerScreenContact))에는 앱의 좌표 공간에서 화면 전화 접속 중심의 X/Y[**좌표 (Dip**](/uwp/api/windows.ui.input.radialcontrollerscreencontact.position)(장치 독립적 픽셀)의 경계 사각형 ([**RadialControllerScreenContact**](/uwp/api/windows.ui.input.radialcontrollerscreencontact.bounds)))가 포함 됩니다. 이 정보는 활성 도구에 컨텍스트를 제공 하 고 사용자에 게 장치 관련 시각적 피드백을 제공 하는 데 매우 유용 합니다.

다음 예제에서는 각각 하나의 슬라이더와 토글을 포함 하는 네 가지 섹션을 포함 하는 기본 앱을 만들었습니다. 그런 다음 surface 전화 접속의 화면 위치를 사용 하 여 Surface 다이얼에 의해 제어 되는 슬라이더 및 토글 집합을 받아쓰기 합니다.

1. 먼저 XAML에서 UI (각각 슬라이더와 토글 단추가 있는 네 개의 섹션)를 선언 합니다.

   ![샘플 앱 UI 이미지](images/windows-wheel/surface-dial-snippet-customtool3.png)  
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

2. 다음은 표면 전화 걸기 화면 위치에 대해 정의 된 처리기가 있는 코드를 포함 합니다.

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

앱을 실행할 때 Surface 다이얼을 사용 하 여 상호 작용 합니다. 먼저, 앱이 검색 하 여 오른쪽 아래 섹션과 연결 하는 Surface Studio 화면에 장치를 놓습니다 (이미지 참조). 그런 다음 Surface 다이얼을 길게 눌러 메뉴를 열고 사용자 지정 도구를 선택 합니다. 사용자 지정 도구를 활성화 한 후에는 Surface 전화를 회전 하 여 슬라이더 컨트롤을 조정할 수 있으며 Surface 전화 걸기를 클릭 하 여 스위치를 전환할 수 있습니다.

![Surface Dial 사용자 지정 도구를 사용 하 여 활성화 된 샘플 앱 UI 이미지](images/windows-wheel/surface-dial-snippet-customtool4.png)  
*Surface Dial 사용자 지정 도구를 사용 하 여 활성화 된 샘플 앱 UI*

## <a name="summary"></a>요약

이 항목에서는 화면 종료 시나리오에 대 한 사용자 환경을 사용자 지정 하는 방법과 Surface Studio에서 사용 하는 경우 화면 시나리오를 사용자 지정 하는 방법에 대 한 UX 및 개발자 지침을 포함 하는 Surface Dial 입력 장치에 대 한 개요를 제공 합니다.

## <a name="feedback"></a>피드백

질문, 제안 및 피드백을으로 보내 주시기 바랍니다 [radialcontroller@microsoft.com](mailto:radialcontroller@microsoft.com) .

## <a name="related-articles"></a>관련된 문서

[자습서: Windows 앱의 Surface 전화 접속 및 기타 휠 장치 지원](radialcontroller-walkthrough.md)

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

[초보자를 위한 자습서: Windows 앱의 Surface 전화 접속 및 기타 휠 장치 지원](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-RadialController)

[유니버설 Windows 플랫폼 샘플(C# 및 C++)](https://github.com/Microsoft/Windows-universal-samples/tree/b78d95134ce2d57c848e0a8dc339fc362748fb9c/Samples/RadialController)

[Windows 클래식 데스크톱 샘플](https://github.com/Microsoft/Windows-classic-samples/tree/master/Samples/RadialController)
