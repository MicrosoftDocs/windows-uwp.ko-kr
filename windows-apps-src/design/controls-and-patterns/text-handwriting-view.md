---
Description: 잉크를 UWP 텍스트 컨트롤의 텍스트 상자와 같은 RichEditBox (및 유사한 텍스트 입력된 환경을 제공 하는 컨트롤을 AutoSuggestBox 같은)에서 지원 되는 텍스트 입력에 대 한 기본 제공 필기 보기를 사용자 지정 합니다.
title: 필기 뷰를 사용 하 여 텍스트 입력
label: Text input with the handwriting view
template: detail.hbs
ms.date: 10/13/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: sewen
design-contact: minah.kim
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f7b31898e6a90410e4edc73ee36f71a7e4d94155
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634918"
---
# <a name="text-input-with-the-handwriting-view"></a>필기 뷰를 사용 하 여 텍스트 입력

![펜으로 터치할 때 텍스트 상자가 확장](images/handwritingview/handwritingview2.gif)

잉크와 같은 UWP 텍스트 컨트롤에서 지원 되는 텍스트 입력에 대 한 기본 제공 필기 보기를 사용자 지정 합니다 [텍스트 상자에 붙여넣습니다](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox), [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox), 제어와 같은 이러한에서 파생 된는 [ AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)합니다.

## <a name="overview"></a>개요

XAML 텍스트 입력된 상자를 사용 하 여 입력 펜에 대 한 포함 된 지원 기능 [Windows 잉크](../input/pen-and-stylus-interactions.md)합니다. 사용자는 Windows 펜을 사용 하 여 텍스트 입력된 상자에 탭을 텍스트 상자에 별도 입력된 패널을 열고 하는 것이 아니라 필기 화면을 변환 합니다.

사용자 쓰기 아무 곳 이나 텍스트 상자 및 후보 창 인식 결과 표시 하는 대로 텍스트 인식 됩니다. 사용자는 결과를 터치하여 선택하거나 쓰기를 계속하여 제안된 후보 창을 수락할 수 있습니다. 리터럴(글자별) 인식 결과가 후보 창에 포함되어 있기 때문에 인식이 사전의 단어로 제한되지 않습니다. 사용자가 쓰기를 하면 자연스럽게 필기하는 느낌을 유지할 수 있도록 수락된 텍스트 입력이 스크립트 글꼴로 변환됩니다.

> [!NOTE]
> 필기 보기 기본적으로 설정 되어 있지만 컨트롤 단위로 별로 사용 하지 않도록 설정 하는 텍스트 입력된 패널 되돌아갑니다.

![잉크 및 제안 된 텍스트 상자](images/handwritingview/handwritingview-inksuggestion1.gif)

사용자는 표준 제스처 및 이러한 작업을 사용하여 텍스트를 편집할 수 있습니다.

- _취소선_ 또는 _지우기_ - 단어나 단어의 일부를 삭제하기 위해 관통선 그리기
- _조인_ - 단어 간 공백 삭제를 위해 호 그리기
- _삽입_ - 공백 삽입을 위해 캐럿 기호 그리기
- _덮어쓰기_ - 교체를 위해 기존 텍스트 덮어쓰기

![잉크 수정 된 텍스트 상자](images/handwritingview/handwritingview-inkcorrection1.gif)

## <a name="disable-the-handwriting-view"></a>필기 뷰를 사용 하지 않도록 설정

기본 제공 필기 보기는 기본적으로 사용 됩니다.

응용 프로그램에 이미 동일한 텍스트 잉크 기능을 제공 하거나 일부 종류의 형식 지정 또는 특수 문자 (예: 탭) 필기 통해 사용할 수 없는 텍스트 입력된 환경을 사용 하는 경우에 필기 뷰를 사용 하지 않도록 좋습니다.

이 예제에서는 설정 하 여 필기 뷰 비활성화를 [IsHandwritingViewEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.ishandwritingviewenabled) 의 속성을 [텍스트 상자에 붙여넣습니다](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) false 컨트롤. 필기 보기를 지 원하는 모든 텍스트 컨트롤에는 비슷한 속성을 지원 합니다.

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen" 
    IsHandwritingViewEnabled="False">
</TextBox>
```

## <a name="specify-the-alignment-of-the-handwriting-view"></a>필기 보기의 맞춤을 지정 합니다.

필기 뷰는 기본 텍스트 컨트롤 위쪽에 있는 한 사용자의 필기 기본 설정에 맞게 크기가 조정 됩니다 (참조 **설정-> 장치 펜-> & Windows 잉크 필기->에 직접 작성 하는 경우에 글꼴 크기를-> 텍스트 필드**). 뷰는 텍스트 컨트롤 및 앱 내에서 해당 위치를 기준으로 자동으로 맞춰집니다.

응용 프로그램 UI 시스템 뷰 채워집니다 중요 한 UI를 일으킬 수 있으므로 큰 컨트롤에 맞게 재배치 되지 않습니다.

여기에서 사용 하는 방법을 보여 줍니다는 [PlacementAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.placementalignment) 의 속성을 [텍스트 상자에 붙여넣습니다](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 기본 텍스트 컨트롤에 있는 앵커에 맞게 사용 되도록 지정 합니다 필기 보기입니다.

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

텍스트 제안 팝업은 인식 후보 사용자 최상위 후보 올바르지 않은 경우 선택할 수 있는 상위 잉크의 목록을 제공 하도록 기본적으로 설정 됩니다.

응용 프로그램이 이미는 강력 하 고 사용자 지정 인식 기능을 제공 하는 경우 사용할 수 있습니다 합니다 [AreCandidatesEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.arecandidatesenabled) 속성을 다음 예와에서 같이 기본 제공 제안 된 방법을 사용 하지 않도록 설정 합니다.

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

잉크 인식에 따라 텍스트 렌더링을 사용자 필기 기반 글꼴을 사용 하는 미리 정의 된 컬렉션에서 선택할 수 있습니다 (참조 **설정-> 장치 펜-> & Windows 잉크 필기-> 필기를사용하는경우글꼴->**).

> [!NOTE]
> 사용자 자신의 필기 기반 글꼴을 만들 수 있습니다.
> [!VIDEO https://www.youtube.com/embed/YRR4qd4HCw8]

앱이이 설정에 액세스 하 고 텍스트 컨트롤에 인식된 된 텍스트에 선택한 글꼴을 사용할 수 있습니다.

이 예제에서는 수신 대기할를 [TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged) 의 이벤트를 [텍스트 상자에 붙여넣습니다](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) 텍스트 변경의 HandwritingView (또는 기본 글꼴, 그렇지 않은 경우)에서 시작 하는 경우 사용자의 선택한 글꼴을 적용 하 고 합니다.

```csharp
private void SampleTextBox_TextChanged(object sender, TextChangedEventArgs e)
{
    ((TextBox)sender).FontFamily = 
        ((TextBox)sender).HandwritingView.IsOpen ?
            new FontFamily(PenAndInkSettings.GetDefault().FontFamilyName) : 
            new FontFamily("Segoe UI");
}
```

## <a name="access-the-handwritingview-in-composite-controls"></a>복합 컨트롤에서 HandwritingView 액세스

복합 컨트롤을 사용 하는 합니다 [텍스트 상자에 붙여넣습니다](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) 또는 [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox) 같은 컨트롤 [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) 기능도 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)합니다.

액세스는 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 복합 컨트롤을 사용 합니다 [VisualTreeHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper) API.

다음 XAML 코드 조각을 표시 하는 [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) 제어 합니다.

```xaml
<AutoSuggestBox Name="SampleAutoSuggestBox" 
    Height="50" Width="500" 
    PlaceholderText="Auto Suggest Example" 
    FontSize="16" FontFamily="Segoe UI"  
    Loaded="SampleAutoSuggestBox_Loaded">
</AutoSuggestBox>
```

해당 코드 숨김에 사용 하지 않도록 설정 하는 방법을 알아보겠습니다 합니다 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 에 [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)합니다.

1. 먼저 처리 응용 프로그램의 Loaded 이벤트 시각적 트리 순회를 시작 하는 FindInnerTextBox 함수 라고 합니다.

    ```csharp
    private void SampleAutoSuggestBox_Loaded(object sender, RoutedEventArgs e)
    {
        if (FindInnerTextBox((AutoSuggestBox)sender))
            autoSuggestInnerTextBox.IsHandwritingViewEnabled = false;
    }
    ```

2. 먼저 다음 FindVisualChildByName 호출 하 여 FindInnerTextBox 함수에서 시각적 트리 (프로그램 AutoSuggestBox부터 시작)를 통해 반복을 시작 합니다.

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

3. 마지막으로,이 함수는 텍스트를 검색할 때까지 시각적 트리를 통해 반복 합니다.

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

## <a name="reposition-the-handwritingview"></a>HandwritingView 위치 변경

일부 경우에 있는지 확인 해야 합니다 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 것이 고, 그렇지 않을 수 있는 UI 요소를 설명 합니다.

여기에서는 받아쓰기 (StackPanel에 텍스트 상자와 받아쓰기 단추를 배치 하 여 구현)를 지 원하는 텍스트 상자를 만듭니다.

![받아쓰기 있는 TextBox](images/handwritingview/textbox-with-dictation.png)

StackPanel 입력란 보다 큰 이제 그대로 합니다 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 모든 복합 cotnrol 채워집니다 하지 않을 수 있습니다.

![받아쓰기 있는 TextBox](images/handwritingview/textbox-with-dictation-handwritingview.png)

이 문제를 해결 PlacementTarget 속성을 설정 합니다 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 것은 맞출 수 있는 UI 요소에 있습니다.

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

크기를 설정할 수도 있습니다는 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview), 보기를 확인 해야 할 때 유용할 수 있는 중요 한 UI 채워집니다 하지 않습니다.

이전 예제와 같이 받아쓰기 (StackPanel에 텍스트 상자와 받아쓰기 단추를 배치 하 여 구현)를 지 원하는 텍스트를 생성 합니다.

![받아쓰기 있는 TextBox](images/handwritingview/textbox-with-dictation.png)

이 경우 받아쓰기 단추가 표시 되는지 항상 확인 하려고 합니다.

![받아쓰기 있는 TextBox](images/handwritingview/textbox-with-dictation-handwritingview-resize.png)

바인딩하기만 MaxWidth 속성을이 작업을 수행 하는 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 채워집니다 해야 하는 UI 요소의 너비입니다.

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

사용자 지정 UI를 정보 팝업 등의 텍스트 입력에 대 한 응답에 표시 되는 경우 필기 보기를 채워집니다 하지 되므로 해당 UI의 위치를 변경 해야 합니다.

![사용자 지정 UI 사용 하 여 텍스트 상자](images/handwritingview/textbox-with-customui.png)

다음 예제에 대 한 수신 하는 방법을 보여 줍니다 합니다 [Opened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.opened), [닫힘](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.closed
), 및 [SizeChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) 의 이벤트를 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 설정 하는 위치를 [팝업](https://docs.microsoft.com/uwp/api/windows.ui.popups)합니다.

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

## <a name="retemplate-the-handwritingview-control"></a>Retemplate HandwritingView 컨트롤

모든 XAML framework 컨트롤을 사용 하 여 사용자 지정할 수 있습니다 시각적 구조와의 시각적 동작으로는 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 특정 요구 사항에 대 한 합니다.

사용자 지정 템플릿을 체크 아웃을 만드는 전체 예제는 [사용자 지정 전송 컨트롤을 만들](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/custom-transport-controls) 방법 또는 [편집 하는 사용자 지정 컨트롤 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl)합니다.








