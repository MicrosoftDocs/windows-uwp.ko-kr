---
author: Karl-Bridge-Microsoft
Description: Customize the built-in handwriting view for ink to text input that is supported by UWP text controls such as the TextBox, RichEditBox (and controls like the AutoSuggestBox that provide a similar text input experience).
title: 필기 보기를 사용 하 여 텍스트 입력
label: Text input with the handwriting view
template: detail.hbs
ms.author: kbridge
ms.date: 10/13/18
ms.topic: article
keywords: windows 10, uwp
pm-contact: sewen
design-contact: minah.kim
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 889683d83f1e592e2ce94f3fce63ca0b2c8bbdcb
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5543248"
---
# <a name="text-input-with-the-handwriting-view"></a>필기 보기를 사용 하 여 텍스트 입력

![펜으로 터치할 때 텍스트 상자가 확장](images/handwritingview/handwritingview2.gif)

잉크를 [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox), [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox), 다른 컨트롤 (예: [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)) 유사한 텍스트 입력된 경험을 제공 하는 등 UWP 텍스트 컨트롤에서 지원 되는 텍스트 입력에 대 한 기본 제공 필기 보기를 사용자 지정 합니다.

## <a name="overview"></a>개요

XAML 텍스트 입력된 상자 [Windows Ink](../input/pen-and-stylus-interactions.md)를 사용한 펜 입력에 대 한 지원이 포함 되어 있습니다. 사용자가 Windows 펜을 사용 하 여 텍스트 입력된 상자에 탭 하면, 텍스트 상자는 별도 입력된 창을 열지 하는 것이 아니라 필기 표면으로 변환 합니다.

사용자가 쓰기 어디서 나 텍스트 상자와 후보 창에 인식 결과가 표시 텍스트 인식 됩니다. 사용자는 결과를 터치하여 선택하거나 쓰기를 계속하여 제안된 후보 창을 수락할 수 있습니다. 리터럴(글자별) 인식 결과가 후보 창에 포함되어 있기 때문에 인식이 사전의 단어로 제한되지 않습니다. 사용자가 쓰기를 하면 자연스럽게 필기하는 느낌을 유지할 수 있도록 수락된 텍스트 입력이 스크립트 글꼴로 변환됩니다.

> [!NOTE]
> 필기 보기는 기본적으로 사용할 수 있지만 컨트롤 별로에서 사용 하지 않도록 설정 하 고 텍스트 입력된 패널로 돌아갈 수 있습니다.

![잉크 및 제안 된 텍스트 상자](images/handwritingview/handwritingview-inksuggestion1.gif)

사용자는 표준 제스처 및 이러한 작업을 사용하여 텍스트를 편집할 수 있습니다.

- _취소선_ 또는 _지우기_ - 단어나 단어의 일부를 삭제하기 위해 관통선 그리기
- _조인_ - 단어 간 공백 삭제를 위해 호 그리기
- _삽입_ - 공백 삽입을 위해 캐럿 기호 그리기
- _덮어쓰기_ - 교체를 위해 기존 텍스트 덮어쓰기

![잉크 수정 있는 입력란](images/handwritingview/handwritingview-inkcorrection1.gif)

## <a name="disable-the-handwriting-view"></a>필기 보기를 사용 하지 않도록 설정

기본 제공 필기 보기는 기본적으로 사용 됩니다.

응용 프로그램에서 동일한 잉크-텍스트 기능을 이미 제공 또는 텍스트 입력된 환경을 일종의 형식 지정 또는 특수 문자 (예: 탭) 필기를 통해 사용할 수 없는에 의존 하는 경우 필기 보기를 사용 하지 않도록 설정 하려고 합니다.

이 예제에서는 [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) 컨트롤의 [IsHandwritingViewEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.ishandwritingviewenabled) 속성을 false로 설정 하 여 필기 보기를 비활성화 합니다. 필기 보기를 지 원하는 모든 텍스트 컨트롤 비슷한 속성을 지원 합니다.

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen" 
    IsHandwritingViewEnabled="False">
</TextBox>
```

## <a name="specify-the-alignment-of-the-handwriting-view"></a>필기 보기의 맞춤을 지정 합니다.

내부 텍스트 컨트롤 위에 있는 필기 보기 이며 사용자의 필기 기본 설정에 맞게 크기 조정 (장치-> **설정 참조 펜-> 및 Windows Ink 필기를-> 텍스트 필드에 직접 쓸 때 글꼴 크기를-> **). 텍스트 컨트롤 및 앱 내 위치를 기준으로 보기도 자동으로 맞춰집니다.

시스템을 중요 한 UI를 폐색 보기를 발생 시킬 수 있으므로 더 큰 제어를 수용 하기 위해 응용 프로그램 UI를 재배치 하지 않습니다.

여기에서는 기본 텍스트 컨트롤에는 앵커 필기 보기에 맞게 사용 되도록 지정 [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 의 [PlacementAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.placementalignment) 속성을 사용 하는 방법을 보여 줍니다.

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen">
        <TextBox.HandwritingView>
            <HandwritingView PlacementAlignment="TopLeft"/>
        </TextBox.HandwritingView>
</TextBox>
```

## <a name="disable-auto-completion-candidates"></a>자동 완성 후보를 사용 하지 않도록 설정

텍스트 제안을 팝업 인식 후보 최상위 후보 올바르지 않은 경우 사용자가 선택할 수 있는 최상위 잉크 목록을 제공 하기 위해 기본적으로 사용 됩니다.

응용 프로그램에 이미 강력한에서 제공 하는 경우 사용자 지정 인식 기능을 사용할 수 있습니다 [AreCandidatesEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.arecandidatesenabled) 속성에 기본 제공 제안을 사용 하지 않으려면 다음 예와 같이.

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen">
        <TextBox.HandwritingView>
            <HandwritingView AreCandidatesEnabled="False"/>
        </TextBox.HandwritingView>
</TextBox>
```

## <a name="use-handwriting-font-preferences"></a>필기 글꼴 기본 설정 사용

사용자가 잉크 인식에 따라 텍스트 렌더링 될 때 사용 하 여 필기 기반 글꼴의 미리 정의 된 컬렉션에서 선택할 수 있습니다 (참조 **설정-장치 > 펜-> 및 Windows Ink 필기를-> 필기를 사용 하는 경우 글꼴->**).

> [!NOTE]
> 사용자가 자신의 필기 기반 글꼴을 만들어 수도 있습니다.
> [!VIDEO https://www.youtube.com/embed/YRR4qd4HCw8]

앱이이 설정에 액세스 하 고 인식 된 텍스트 텍스트 컨트롤에 대 한 선택 된 글꼴을 사용할 수 있습니다.

이 예제에서는 [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) 의 [TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged) 이벤트 수신 하 고 텍스트 변경의 HandwritingView (또는 그렇지 않은 경우 기본 글꼴)에서 생성 된 경우 사용자의 선택 된 글꼴을 적용 합니다.

```csharp
private void SampleTextBox_TextChanged(object sender, TextChangedEventArgs e)
{
    ((TextBox)sender).FontFamily = 
        ((TextBox)sender).HandwritingView.IsOpen ?
            new FontFamily(PenAndInkSettings.GetDefault().FontFamilyName) : 
            new FontFamily("Segoe UI");
}
```

## <a name="access-the-handwritingview-in-composite-controls"></a>복합 컨트롤의 HandwritingView 액세스

또한 [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) 등 [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) 또는 [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox) 컨트롤을 사용 하는 복합 컨트롤 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)를 지원 합니다.

[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 복합 컨트롤에 액세스 하려면 [VisualTreeHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper) API 사용 합니다.

다음 XAML 코드 조각을 [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) 컨트롤을 표시합니다.

```xaml
<AutoSuggestBox Name="SampleAutoSuggestBox" 
    Height="50" Width="500" 
    PlaceholderText="Auto Suggest Example" 
    FontSize="16" FontFamily="Segoe UI"  
    Loaded="SampleAutoSuggestBox_Loaded">
</AutoSuggestBox>
```

해당 코드 뒤에서 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)를 사용 하지 않도록 설정 하는 방법을 보여 줍니다.

1. 먼저 처리 Loaded 이벤트 응용 프로그램의 시각적 트리 통과 시작 하려면 FindInnerTextBox 함수를 호출 하 합니다.

    ```csharp
    private void SampleAutoSuggestBox_Loaded(object sender, RoutedEventArgs e)
    {
        if (FindInnerTextBox((AutoSuggestBox)sender))
            autoSuggestInnerTextBox.IsHandwritingViewEnabled = false;
    }
    ```

2. 그런 다음 FindVisualChildByName 호출 하 여 FindInnerTextBox 함수에서 시각적 트리 (AutoSuggestBox부터 시작)를 반복 시작 하겠습니다.

    ```csharp
    private bool FindInnerTextBox(AutoSuggestBox autoSuggestBox)
    {
        if (autoSuggestInnerTextBox == null)
        {
            // Cache textbox to avoid multiple tree traversals. 
            autoSuggestInnerTextBox = 
                (TextBox)FindVisualChildByName<TextBox>(autoSuggestBox);
        }
        return (autoSuggestInnerTextBox != null);
    }
    ```

3. 마지막으로,이 함수 TextBox를 검색할 때까지 시각적 트리를 반복 합니다.

    ```csharp
    private FrameworkElement FindVisualChildByName<T>(DependencyObject obj)
    {
        FrameworkElement element = null;
        int childrenCount = 
            VisualTreeHelper.GetChildrenCount(obj);
        for (int i = 0; (i < childrenCount) && (element == null); i++)
        {
            FrameworkElement child = 
                (FrameworkElement)VisualTreeHelper.GetChild(obj, i);
            if ((child.GetType()).Equals(typeof(T)) || (child.GetType().GetTypeInfo().IsSubclassOf(typeof(T))))
            {
                element = child;
            }
            else
            {
                element = FindVisualChildByName<T>(child);
            }
        }
        return (element);
    }
    ```

## <a name="reposition-the-handwritingview"></a>위치 변경의 HandwritingView

[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 는 그 그렇지 않은 경우 UI 요소를 설명 있는지 확인 해야 하는 할 수 있습니다.

여기에서는 받아쓰기 (StackPanel에 TextBox와 받아쓰기 단추를 배치 하 여 구현)를 지 원하는 TextBox를 만듭니다.

![받아쓰기를 사용 하 여 TextBox](images/handwritingview/textbox-with-dictation.png)

StackPanel 이제 TextBox 보다 큰 그대로 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 모든 복합 cotnrol 안 보일 하지 않을 수 있습니다.

![받아쓰기를 사용 하 여 TextBox](images/handwritingview/textbox-with-dictation-handwritingview.png)

이 문제를 해결 하려면 것은 맞출 수 있는 UI 요소에 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 의 PlacementTarget 속성을 설정 합니다.

```xaml
<StackPanel Name="DictationBox" 
    Orientation="Horizontal" 
    VerticalAlignment="Top" 
    HorizontalAlignment="Left" 
    BorderThickness="1" BorderBrush="DarkGray" 
    Height="55" Width="500" Margin="50">
    <TextBox Name="DictationTextBox" 
        Width="450" BorderThickness="0" 
        FontSize="24" VerticalAlignment="Center">
        <TextBox.HandwritingView>
            <HandwritingView PlacementTarget="{Binding ElementName=DictationBox}"/>
        </TextBox.HandwritingView>
    </TextBox>
    <Button Name="DictationButton" 
        Height="48" Width="48" 
        FontSize="24" 
        FontFamily="Segoe MDL2 Assets" 
        Content="&#xE720;" 
        Background="White" Foreground="DarkGray"     Tapped="DictationButton_Tapped" />
</StackPanel>
```

## <a name="resize-the-handwritingview"></a>HandwritingView 크기 조정

보기에는 중요 한 UI 안 보일 하지 않도록 해야 할 때 유용할 수 있는 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview), 크기를 설정할 수도 있습니다.

이전 예제와 마찬가지로 받아쓰기 (StackPanel에 TextBox와 받아쓰기 단추를 배치 하 여 구현)를 지 원하는 TextBox를 만듭니다.

![받아쓰기를 사용 하 여 TextBox](images/handwritingview/textbox-with-dictation.png)

이 경우 받아쓰기 단추는 항상 표시 되었는지 확인 하려고 합니다.

![받아쓰기를 사용 하 여 TextBox](images/handwritingview/textbox-with-dictation-handwritingview-resize.png)

이렇게 하려면 안 보일 해야 하는 UI 요소의 너비를 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 의 MaxWidth 속성을 바인딩합니다.

```xaml
<StackPanel Name="DictationBox" 
    Orientation="Horizontal" 
    VerticalAlignment="Top" 
    HorizontalAlignment="Left" 
    BorderThickness="1" 
    BorderBrush="DarkGray" 
    Height="55" Width="500" 
    Margin="50">
    <TextBox Name="DictationTextBox" 
        Width="450" 
        BorderThickness="0" 
        FontSize="24" 
        VerticalAlignment="Center">
        <TextBox.HandwritingView>
            <HandwritingView 
                PlacementTarget="{Binding ElementName=DictationBox}“
                MaxWidth="{Binding ElementName=DictationTextBox, Path=Width"/>
        </TextBox.HandwritingView>
    </TextBox>
    <Button Name="DictationButton" 
        Height="48" Width="48" 
        FontSize="24" 
        FontFamily="Segoe MDL2 Assets" 
        Content="&#xE720;" 
        Background="White" Foreground="DarkGray" 
        Tapped="DictationButton_Tapped" />
</StackPanel>
```

## <a name="reposition-custom-ui"></a>사용자 지정 UI 위치 변경

알림 팝업 같은 텍스트 입력에 대 한 응답에 표시 되는 사용자 지정 UI를 설정한 경우 필기 보기를 안 보일 하지 되므로 해당 UI의 위치를 변경 해야 합니다.

![사용자 지정 UI 사용 하 여 TextBox](images/handwritingview/textbox-with-customui.png)

다음 예제에서는 [팝업](https://docs.microsoft.com/uwp/api/windows.ui.popups)의 위치를 설정 하려면 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 의 [Opened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.opened), [Closed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.closed
)및 [SizeChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) 이벤트를 수신 하는 방법을 보여 줍니다.

```csharp
private void Search_HandwritingViewOpened(
    HandwritingView sender, HandwritingPanelOpenedEventArgs args)
{
    UpdatePopupPositionForHandwritingView();
}

private void Search_HandwritingViewClosed(
    HandwritingView sender, HandwritingPanelClosedEventArgs args)
{
    UpdatePopupPositionForHandwritingView();
}

private void Search_HandwritingViewSizeChanged(
    object sender, SizeChangedEventArgs e)
{
    UpdatePopupPositionForHandwritingView();
}

private void UpdatePopupPositionForHandwritingView()
{
if (CustomSuggestionUI.IsOpen)
    CustomSuggestionUI.VerticalOffset = GetPopupVerticalOffset();
}

private double GetPopupVerticalOffset()
{
    if (SearchTextBox.HandwritingView.IsOpen)
        return (SearchTextBox.Margin.Top + SearchTextBox.HandwritingView.ActualHeight);
    else
        return (SearchTextBox.Margin.Top + SearchTextBox.ActualHeight);    
}
```

## <a name="retemplate-the-handwritingview-control"></a>HandwritingView 컨트롤 템플릿을 다시 작성

모든 XAML 프레임 워크 컨트롤과 마찬가지로 구체적인 요구 사항에 대 한 시각적 구조와 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 의 시각적 동작을 지정할 수 있습니다.

사용자 지정 템플릿을 [사용자 지정 전송 컨트롤을 만드는](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/custom-transport-controls) 방법 또는 [사용자 지정 편집 컨트롤 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl)체크아웃 만드는 전체 예제를 보려면.







