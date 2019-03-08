---
Description: 이 자습서에서는 기본 응용 프로그램 사용자 인터페이스를 만드는 방법을 설명 합니다. 가장 일반적인 XAML 요소인 Grid 및 StackPanel을 사용하는 방법을 설명하고 보여 줍니다.
title: Grid 및 StackPanel을 사용하여 간단한 날씨 앱을 만듭니다.
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 9794a04d-e67f-472c-8ba8-8ebe442f6ef2
ms.localizationpriority: medium
ms.openlocfilehash: 5b221220d417df5b70927984ac65eff93fae54a4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646538"
---
# <a name="tutorial-use-grid-and-stackpanel-to-create-a-simple-weather-app"></a>자습서: 표 및 StackPanel 간단한 날씨 앱 만들기를 사용 하 여

XAML을 사용하여 **Grid** 및 **StackPanel** 요소로 간단한 날씨 앱의 레이아웃을 만들 수 있습니다. 이러한 도구를 사용하면 Windows 10을 실행하는 모든 디바이스에서 작동하는 근사한 앱을 만들 수 있습니다. 이 자습서는 10-20분 정도 걸립니다.

> **중요 한 Api**: [Grid 클래스](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.grid), [StackPanel 클래스](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.stackpanel)

## <a name="prerequisites"></a>필수 구성 요소
- Windows 10 및 Microsoft Visual Studio 2015 이상. (최신 Visual Studio 업데이트 현재 개발 및 보안에 대 한 권장) [Visual Studio를 사용 하 여 설정 하는 방법을 알아보려면 여기를 클릭 하십시오.](../../get-started/get-set-up.md)합니다.
- XAML 및 C#을 사용하여 기본 "Hello World" 앱을 만드는 방법에 대한 지식. 아직 살펴보지 않았다면 [여기를 클릭하여 "Hellow World" 앱을 만드는 방법을 알아보세요](https://msdn.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal).

## <a name="step-1-create-a-blank-app"></a>1단계: 빈 앱 만들기
1. Visual Studio 메뉴에서 **파일** > **새 프로젝트**를 선택합니다.
2. **새 프로젝트** 대화 상자의 왼쪽 창에서 **Visual C#** > **Windows** > **유니버설** 또는 **Visual C++** > **Windows** > **유니버설**을 선택합니다.
3. 가운데 창에서 **빈 앱**을 선택합니다.
4. **이름** 상자에 **WeatherPanel**을 입력하고 **확인**을 선택합니다.
5. 프로그램을 실행하려면 메뉴에서 **디버그** > **디버깅 시작**을 선택하거나 F5 키를 선택합니다.

## <a name="step-2-define-a-grid"></a>2단계: 표 정의
XAML에서 **Grid**는 일련의 행과 열로 이루어집니다. **Grid** 내에서 요소의 행과 열을 지정하면 사용자 인터페이스 내에서 다른 요소를 배치하고 간격을 지정할 수 있습니다. 행과 열은 **RowDefinition** 및 **ColumnDefinition** 요소로 정의됩니다.

레이아웃을 만들기 시작하려면 **솔루션 탐색기**를 사용하여 **MainPage.xaml**을 열고 자동으로 생성된 **Grid** 요소를 이 코드로 바꿉니다.

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="3*"/>
        <ColumnDefinition Width="5*"/>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="2*"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
</Grid>
```

새 **Grid**는 앱 인터페이스의 레이아웃을 정의하는 두 개의 행과 열 집합을 만듭니다. 첫 번째 열에는 **너비** 의 "3\*" 반면 두 번째 "5\*", 비율은 3: 5에서 두 열 간에 가로 간격을 구분 합니다. 두 개의 행에 같은 방법으로는 **높이** 의 "2\*"및"\*" 각각 하므로 **표** 두 번째의 경우 첫 번째 행에 두 배의 공간을 할당 ("\*"동일" 1\*"). 이러한 비율은 창 크기를 조정하거나 디바이스를 변경하는 경우에도 유지됩니다.

행과 열의 크기를 조정하는 다른 방법을 알아보려면 [XAML을 사용하여 레이아웃 정의](https://msdn.microsoft.com/windows/uwp/layout/layouts-with-xaml#layout-properties)를 참조하세요.

지금 응용 프로그램을 실행하면 **Grid** 영역에 콘텐츠가 없으므로 빈 페이지만 표시됩니다. **Grid**를 표시하기 위해 색을 지정하겠습니다.

## <a name="step-3-color-the-grid"></a>3단계: 색 눈금
**Grid**에 색을 지정하기 위해 각각 다른 배경색을 가진 **Border** 요소 세 개를 추가합니다. 또한 **Grid.Row** 및 **Grid.Column** 특성을 사용하여 부모 **Grid**의 행과 열에 각각 할당합니다. 이러한 특성 값은 기본적으로 0으로 설정되므로 첫 번째 **Border**에는 할당할 필요가 없습니다. **Grid** 요소에서 행과 열 정의 뒤에 다음 코드를 추가합니다.

```xml
<Border Background="#2f5cb6"/>
<Border Grid.Column ="1" Background="#1f3d7a"/>
<Border Grid.Row="1" Grid.ColumnSpan="2" Background="#152951"/>
```

세 번째 **Border**에 대해 추가 특성 **Grid.ColumnSpan**을 사용하여 이 **Border**가 아래쪽 행의 두 열에 모두 적용되도록 합니다. 동일한 방식으로 **Grid.RowSpan**을 사용할 수 있으며, 두 특성을 함께 사용하여 원하는 개수의 행과 열에 걸쳐 있는 요소를 만들 수 있습니다. 이러한 범위의 왼쪽 위 모서리는 항상 요소 특성에 지정된 **Grid.Column** 및 **Grid.Row**입니다.

앱을 실행하면 결과가 다음과 같이 표시됩니다.

![Grid에 색 지정](images/grid-weather-1.png)

## <a name="step-4-organize-content-by-using-stackpanel-elements"></a>4단계: StackPanel 요소를 사용 하 여 콘텐츠를 구성 합니다.
**StackPanel**은 날씨 앱을 만드는 데 사용할 두 번째 UI 요소입니다. **StackPanel**은 많은 기본 앱 레이아웃의 핵심 부분으로, 요소를 세로 또는 가로로 쉽게 스택할 수 있습니다.

다음 코드에서는 **StackPanel** 요소 두 개를 만들고 각 요소에 **TextBlock** 세 개를 채웁니다. **Grid**에서 3단계의 **Border** 요소 아래에 해당 **StackPanel** 요소를 추가합니다. 이렇게 하면 **TextBlock** 요소가 앞에서 만든 색이 지정된 **Grid** 위에 렌더링됩니다.

```xml
<StackPanel Grid.Column="1" Margin="40,0,0,0" VerticalAlignment="Center">
    <TextBlock Foreground="White" FontSize="25" Text="Today - 64° F"/>
    <TextBlock Foreground="White" FontSize="25" Text="Partially Cloudy"/>
    <TextBlock Foreground="White" FontSize="25" Text="Precipitation: 25%"/>
</StackPanel>
<StackPanel Grid.Row="1" Grid.ColumnSpan="2" Orientation="Horizontal"
            HorizontalAlignment="Center" VerticalAlignment="Center">
    <TextBlock Foreground="White" FontSize="25" Text="High: 66°" Margin="0,0,20,0"/>
    <TextBlock Foreground="White" FontSize="25" Text="Low: 43°" Margin="0,0,20,0"/>
    <TextBlock Foreground="White" FontSize="25" Text="Feels like: 63°"/>
</StackPanel>
```

첫 번째 **Stackpanel**에서 각 **TextBlock**이 다음 요소 아래에 세로로 스택됩니다. 이는 StackPanel의 기본 동작이므로 **Orientation** 특성을 설정할 필요는 없습니다. 두 번째 StackPanel에서는 자식 요소를 왼쪽에서 오른쪽으로 가로로 스택하려고 하므로 **Orientation** 특성을 "Horizontal"로 설정합니다. 또한 텍스트가 아래쪽 **Border** 위의 가운데에 배치되도록 **Grid.ColumnSpan** 특성을 "2"로 설정해야 합니다.

이제 앱을 실행하면 다음과 같이 표시됩니다.

![StackPanel 추가](images/grid-weather-2.png)

## <a name="step-5-add-an-image-icon"></a>5단계: 이미지 아이콘 추가

마지막으로, **Grid**의 빈 섹션에 오늘 날씨를 나타내는 이미지(예: "부분적으로 흐림")를 채우겠습니다.

아래 이미지를 다운로드하고 "partially-cloudy"라는 PNG로 저장합니다.

![부분적으로 흐림](images/partially-cloudy.PNG)

에 **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 합니다 **자산** 폴더를 선택한 **추가** -> **기존 항목...** 표시 되는 브라우저에서 부분적으로 cloudy.png를 찾아를 선택 하 고 클릭 **추가**합니다.

**MainPage.xaml**에서 4단계의 StackPanel 아래에 다음 **Image** 요소를 추가합니다.

```xml
<Image Margin="20" Source="Assets/partially-cloudy.png"/>
```

첫 번째 행과 열에 Image를 배치하려고 하므로 해당 **Grid.Row** 또는 **Grid.Column** 특성을 설정할 필요는 없으며 기본값 "0"으로 두면 됩니다.

정말 간단하죠! 간단한 날씨 응용 프로그램의 레이아웃을 만들었습니다. **F5** 키를 눌러 응용 프로그램을 실행하는 경우 다음과 같이 표시됩니다.

![날씨 창 샘플](images/grid-weather-3.PNG)

원하는 경우 위의 레이아웃을 실험해보고, 날씨 데이터를 표시할 수 있는 다양한 방법을 살펴보세요.

## <a name="related-articles"></a>관련 문서
UWP 앱 레이아웃 디자인 소개를 보려면 [UWP 앱 디자인 소개](https://msdn.microsoft.com/windows/uwp/layout/design-and-ui-intro)를 참조하세요.

다양한 화면 크기에 맞게 조정되는 반응형 레이아웃을 만드는 방법에 대한 자세한 내용은 [XAML을 사용하여 페이지 레이아웃 정의](https://msdn.microsoft.com/windows/uwp/layout/layouts-with-xaml)를 참조하세요.
