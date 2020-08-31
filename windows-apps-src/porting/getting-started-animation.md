---
title: 애니메이션 시작
ms.assetid: C1C3F5EA-B775-4700-9C45-695E78C16205
description: 이 프로젝트에서는 사각형을 이동 하 고 페이드 효과를 적용 한 다음 다시 뷰로 가져옵니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5da59c85f2b091ce21b9da5bb676a410a4b318ba
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171317"
---
# <a name="getting-started-animation"></a>시작: 애니메이션


## <a name="adding-animations"></a>애니메이션 추가

iOS에서는 애니메이션 효과를 프로그래밍 방식으로 만드는 경우가 많습니다. 예를 들어 블록 기반 **Uiview** 클래스의 **animateWithDuration** 메서드 또는 이전 비 블록 기반 메서드에서 제공 하는 애니메이션을 사용할 수 있습니다. 또는 **CALayer** 클래스를 명시적으로 사용하여 계층에 애니메이션 효과를 줄 수 있습니다. Windows 앱의 애니메이션은 프로그래밍 방식으로 만들 수 있지만 Extensible Application Markup Language (XAML)을 사용 하 여 선언적으로 정의 될 수도 있습니다. Microsoft Visual Studio를 사용 하 여 XAML 코드를 직접 편집할 수 있지만 Visual Studio에는 디자이너에서 애니메이션을 사용할 때 XAML 코드를 만드는 **Blend**라는 도구도 제공 됩니다. 실제로 Blend를 사용 하면 전체 Visual Studio 프로젝트를 그래픽으로 열고, 디자인 하 고, 빌드하고, 실행할 수 있습니다. 다음 연습을 통해이를 시도할 수 있습니다.

새 유니버설 Windows 플랫폼 (UWP) 앱을 만들고 이름을 "SimpleAnimation"와 같이 합니다. 이 프로젝트에서는 사각형을 이동 하 고 페이드 효과를 적용 한 다음 다시 뷰로 가져옵니다. XAML 애니메이션은 *storyboard* 개념을 기반으로 합니다 (iOS storyboard와 혼동 하지 않음). Storyboard는 *키 프레임* 을 사용 하 여 속성 변경 내용을 애니메이션 합니다.

프로젝트를 연 상태 **솔루션 탐색기**에서 프로젝트의 이름을 마우스 오른쪽 단추로 클릭 한 다음 **blend에서 열기** 또는 blend에서 열기를 선택 합니다. 다음 그림과 같이 **blend**에서 열기를 선택 합니다. Visual Studio는 백그라운드에서 계속 실행 됩니다.

![blend에서 열기 메뉴 명령](images/ios-to-uwp/vs-open-in-blend.png)

Blend를 시작 하면 다음 그림과 유사한 내용이 표시 됩니다.

![blend 개발 환경](images/ios-to-uwp/blend-1.png)

왼쪽의 **솔루션 탐색기** 에서 **mainpage** 을 두 번 클릭 합니다. 다음으로, 다음 그림에 표시 된 것 처럼 중앙 **디자인 보기**가장자리의 세로 도구 스트립에서 **사각형** 도구를 선택 하 고 **디자인 뷰에서**사각형을 그립니다.

![디자인 뷰에 사각형 추가](images/ios-to-uwp/blend-2.png)

사각형을 녹색으로 만들려면 **속성** 창에서 확인 하 고 **브러시** 영역에서 **단색 브러시** 단추를 클릭 한 다음 **색 스 포 이트** 아이콘을 클릭 합니다. 색조의 녹색 띠에서 아무 곳 이나 클릭 합니다.

사각형에 애니메이션 효과를 시작 하려면 다음 그림에 표시 된 것 처럼 **개체 및 타임라인** 창에서 더하기 기호 (**새로 만들기**) 단추를 탭 한 다음 **확인**을 탭 합니다.

![스토리 보드 추가](images/ios-to-uwp/blend-3.png)

스토리 보드가 **개체 및 타임라인** 창에 표시 됩니다. 제대로 보려면 보기의 크기를 조정 해야 할 수 있습니다. **디자인 뷰**가 변경되어 **Storyboard1 타임라인 기록이 켜져 있음**이라고 표시됩니다. 다음 그림에 표시 된 것 처럼, **개체 및 타임라인** 창에서 사각형의 현재 상태를 캡처하려면 노란색 화살표 바로 위에 있는 **키 프레임 기록** 단추를 누릅니다.

![키 프레임 기록](images/ios-to-uwp/blend-4.png)

이제 사각형을 이동 하 고 페이드 아웃 하겠습니다. 이렇게 하려면 주황색/노란색 화살표를 2 초 위치로 끌고 녹색 사각형을 오른쪽으로 약간 이동 합니다. 그런 다음 **속성** 창의 **모양** 영역에서 **불투명도** 속성을 **0**으로 변경 합니다 (다음 그림에 표시 된 것 처럼). 애니메이션을 미리 보려면 Storyboard 패널에서 **재생** 단추를 누릅니다.

![속성 창 및 재생 단추](images/ios-to-uwp/blend-5.png)

다음으로 사각형을 다시 보기로 만들어 보겠습니다. **개체 및 타임라인** 창에서 **Storyboard1**를 두 번 클릭 합니다. 그런 다음 **속성** 창의 **일반** 영역에서 다음 그림과 같이 **system.windows.media.animation.timeline.autoreverse**를 선택 합니다.

![스토리 보드 선택](images/ios-to-uwp/blend-6.png)

마지막으로 **재생** 단추를 클릭 하 여 어떻게 되는지 확인 합니다.

창 위쪽에서 녹색 실행 단추를 클릭 하거나 F5 키를 눌러 프로젝트를 빌드하고 실행할 수 있습니다. 이렇게 하면 프로젝트가 실제로 빌드 및 실행 되는 것을 볼 수 있지만, 녹색 사각형은 슈퍼마켓 통로의 아이 거부 사탕와 같이 완전히 stubbornly 됩니다. 애니메이션을 시작 하려면 프로젝트에 코드 줄을 추가 해야 합니다. 방법은 다음과 같습니다.

**파일** 메뉴를 열고 **mainpage 저장**을 선택 하 여 프로젝트를 저장 합니다. Visual Studio로 돌아갑니다. 수정 된 파일을 다시 로드할지 여부를 묻는 대화 상자가 표시 되 면 **예**를 선택 합니다. **Mainpage**에서 숨겨진 **MainPage.xaml.cs** 파일을 두 번 클릭 하 여 열고, public mainpage () 메서드 바로 위에 다음 코드를 추가 합니다.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    // Add the following line of code.
    Storyboard1.Begin();
}
```

프로젝트를 다시 실행 하 고 사각형에 애니메이션 효과를 봅니다. Hurrah!

MainPage .xaml 파일을 열면 **xaml** 뷰에서 디자이너에서 작업할 때 Blend가 추가 된 xaml 코드를 볼 수 있습니다. 특히 및 요소의 코드를 확인 `<Storyboard>` `<Rectangle>` 합니다. 다음은 예를 보여 주는 코드입니다. 줄임표는 간단 하 게 구분 하기 위해 관련이 없는 코드를 나타내며 코드 가독성을 위해 줄 바꿈이 추가 되었습니다.

```xml
...
<Storyboard 
        x:Name="Storyboard1" 
        AutoReverse="True">
    <DoubleAnimationUsingKeyFrames 
            Storyboard.TargetProperty="(UIElement.RenderTransform).(CompositeTransform.TranslateX)"
            Storyboard.TargetName="rectangle">
        <EasingDoubleKeyFrame 
                KeyTime="0" 
                Value="0"/>
        <EasingDoubleKeyFrame 
                KeyTime="0:0:2" 
                Value="185.075"/>
    </DoubleAnimationUsingKeyFrames>
    <DoubleAnimationUsingKeyFrames 
            Storyboard.TargetProperty="(UIElement.RenderTransform).(CompositeTransform.TranslateY)" 
            Storyboard.TargetName="rectangle">
        <EasingDoubleKeyFrame 
                KeyTime="0" 
                Value="0"/>
        <EasingDoubleKeyFrame 
                KeyTime="0:0:2" 
                Value="2.985"/>
    </DoubleAnimationUsingKeyFrames>
    <DoubleAnimationUsingKeyFrames 
            Storyboard.TargetProperty="(UIElement.Opacity)" 
            Storyboard.TargetName="rectangle">
        <EasingDoubleKeyFrame 
                KeyTime="0" 
                Value="1"/>
        <EasingDoubleKeyFrame 
                KeyTime="0:0:2"
                Value="0"/>
    </DoubleAnimationUsingKeyFrames>
</Storyboard>
...
<Rectangle 
        x:Name="rectangle" 
        Fill="#FF00FF63" 
        HorizontalAlignment="Left" 
        Height="122" 
        Margin="151,312,0,0" 
        Stroke="Black" 
        VerticalAlignment="Top" 
        Width="239" 
        RenderTransformOrigin="0.5,0.5">
    <Rectangle.RenderTransform>
        <CompositeTransform/>
    </Rectangle.RenderTransform>
</Rectangle>
...
```

이 XAML을 수동으로 편집 하거나 Blend로 돌아와서 작업을 계속할 수 있습니다. Blend를 사용 하면 흥미로운 사용자 인터페이스를 편리 하 게 만들 수 있으며 그래픽 도구를 사용 하 여 애니메이션 효과를 주는 기능을 사용 하면 개발 시간을 크게 단축할 수 있습니다. 애니메이션에 대 한 자세한 내용은 [애니메이션 개요](../design/motion/xaml-animation.md)를 참조 하세요.

**참고**    <span class="legacy-term">JavaScript 및 html을 사용 하는 UWP 앱</span>에 대 한 애니메이션 정보는 [UI에 애니메이션 적용 (html)](/previous-versions/windows/apps/hh465165(v=win.10))을 참조 하세요.

### <a name="next-step"></a>다음 단계

[시작: 그 다음에 무엇을 해야 합니까?](getting-started-what-next.md)