---
ms.assetid: DA562509-D893-425A-AAE6-B2AE9E9F8A19
label: 텍스트 블록
template: detail.hbs
---
# 텍스트 블록
 텍스트 블록은 앱에서 읽기 전용 텍스트를 표시하기 위한 주 컨트롤입니다. 이 컨트롤을 사용하여 한 줄 또는 여러 줄 텍스트, 인라인 하이퍼링크 및 굵게, 기울임꼴 또는 밑줄 서식이 적용된 텍스트를 표시할 수 있습니다.

<span class="sidebar_heading" style="font-weight: bold;">중요 API</span>

-   [**TextBlock 클래스**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.aspx)
-   [**Text 속성**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx)
-   [**Inlines 속성**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.inlines.aspx)

## 올바른 컨트롤인가요?

텍스트 블록은 일반적으로 서식 있는 텍스트 블록보다 사용하기 쉬우며 더 나은 텍스트 렌더링 성능을 제공하므로 대부분의 앱 UI 텍스트에서 기본으로 설정됩니다. [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx) 속성의 값을 가져와 앱에서 텍스트 블록의 텍스트에 쉽게 액세스하고 사용할 수 있습니다. 또한 텍스트가 렌더링되는 방식을 사용자 지정할 수 있도록 동일한 서식 옵션을 여러 개 제공합니다. 

텍스트에 줄 바꿈을 넣을 수는 있지만, 텍스트 블록은 단일 단락을 표시하도록 디자인되었으며 텍스트 들여쓰기를 지원하지 않습니다. 여러 단락, 다중 열 텍스트 또는 기타 복잡한 텍스트 레잉아웃, 이미지와 같은 인라인 UI 요소에 대한 지원이 필요한 경우 **RichTextBlock**을 사용하세요.

올바른 텍스트 컨트롤을 선택하는 방법에 대한 자세한 내용은 [텍스트 컨트롤](text-controls.md) 문서를 참조하세요.

## 예제


## 텍스트 블록 만들기

다음은 간단한 TextBlock 컨트롤을 정의하고 Text 속성을 문자열로 설정하는 방법입니다.

```xaml
<TextBlock Text="Hello, world!" />
```

```csharp
TextBlock textBlock1 = new TextBlock();
textBlock1.Text = "Hello, world!";
```

    <TextBlock Text="Hello, world!" />

    TextBlock textBlock1 = new TextBlock();
    textBlock1.Text = "Hello, world!";

### 콘텐츠 모델

TextBlock에 콘텐츠를 추가하는 데 사용할 수 있는 속성은 두 가지 즉, [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx) 및 [Inlines](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.inlines.aspx)가 있습니다.

텍스트를 표시하는 가장 일반적인 방법은 이전 예제에 나와 있듯이 Text 속성을 문자열 값으로 설정하는 것입니다.

또한 다음과 같이 TextBox.Inlines 속성에 인라인 흐름 콘텐츠 요소를 배치하여 콘텐츠를 추가할 수도 있습니다.
```xaml
<TextBlock><Run>Text can be <Bold>bold</Bold>, <Italic>italic</Italic>, or <Bold><Italic>both</Italic></Bold>.</Run></TextBlock>
```

Bold, Italic, Run, Span 및 LineBreak와 같이 Inline 클래스에서 파생된 요소는 텍스트의 다른 부분에 대해 다른 서식을 적용할 수 있습니다. 자세한 내용은 [텍스트 서식 지정]() 섹션을 참조하세요. 인라인 Hyperlink 요소를 사용하면 텍스트에 하이퍼링크를 추가할 수 있습니다. 그러나 Inlines를 사용하면 다음 섹션에서 설명하는 빠른 경로 텍스트 렌더링을 사용할 수 없습니다.


## 성능 고려 사항

가능한 경우 언제나 XAML에서는 레이아웃 텍스트에 대한 더 효율적인 코드 경로를 사용합니다. 이 빠른 경로는 텍스트를 측정하고 정렬하는 드는 전체 메모리 사용량을 줄이고 CPU 시간을 크게 단축합니다. 이 빠른 경로는 TextBlock에만 적용되므로 가능한 경우에는 RichTextBlock 대신 이 경로를 사용해야 합니다.

특정 조건에서는 텍스트 렌더링을 위해 TextBlock이 기능이 풍부하고 CPU를 많이 사용하는 코드 경로를 사용해야 합니다. 텍스트 렌더링을 빠른 경로에서 유지하려면 여기에 나열된 속성을 설정할 때 다음 지침에 따야 합니다.
- [
            **Text**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx): 가장 중요한 조건은 XAML 또는 코드에서(이전 예제에 나온 대로) 명시적으로 Text 속성을 설정하여 텍스트를 설정할 때만 빠른 경로를 사용한다는 점입니다. 여러 형식의 잠재적 복잡성으로 인해, TextBlock의 Inlines 컬렉션(예: `<TextBlock>Inline text</TextBlock>`)을 통해 텍스트를 설정하면 빠른 경로가 사용하지 않도록 설정됩니다.
- [
            **CharacterSpacing**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.characterspacing.aspx): 기본값 0만 빠른 경로입니다.
- [
            **Typography**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx): 다양한 Typography 속성에 대한 기본값만 빠른 경로입니다.
- [
            **TextTrimming**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.texttrimming.aspx): **None**, **CharacterEllipsis** 및 **WordEllipsis** 값만 빠른 경로입니다. **Clip** 값은 빠른 경로를 사용하지 않도록 설정합니다.
- [
            **LineStackingStrategy**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.linestackingstrategy.aspx): [LineHeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.lineheight.aspx)가 0이 아닌 경우 **BaselineToBaseline** 및 **MaxHeight** 값이 빠른 경로를 사용하지 않도록 설정합니다.
- [
            **IsTextSelectionEnabled**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.istextselectionenabled.aspx): **false**만 빠른 경로입니다. 이 속성을 **true**로 설정하면 빠른 경로가 사용하지 않도록 설정됩니다.

디버그 도중에 [DebugSettings.IsTextPerformanceVisualizationEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.debugsettings.istextperformancevisualizationenabled.aspx) 속성을 **true**로 설정하여 텍스트가 빠른 경로 렌더링을 사용하고 있는지 여부를 확인할 수 있습니다. 이 속성이 true로 설정된 경우 빠른 경로에 있는 텍스트가 밝은 녹색으로 표시됩니다. 

>**팁**&nbsp;&nbsp;이 기능에 대해서는 빌드 2015 - [XAML 성능: XAML을 사용하여 빌드한 유니버설 Windows 앱 환경을 극대화하는 기법](https://channel9.msdn.com/Events/Build/2015/3-698)의 이 세션에서 자세히 설명합니다.

 

일반적으로 App.xaml에 대한 코드 숨김 페이지에서 [OnLaunched](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.application.onlaunched.aspx) 메서드 재정의에서 다음과 같이 디버그 설정을 지정합니다.
```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
#if DEBUG
    if (System.Diagnostics.Debugger.IsAttached)
    {
        this.DebugSettings.IsTextPerformanceVisualizationEnabled = true;
    }
#endif

// ...

}
```

이 예제에서 첫 번째 TextBlock은 빠른 경로를 사용하여 렌더링되고, 두 번째는 그렇지 않습니다.
```xaml
<StackPanel>
    <TextBlock Text="This text is on the fast path."/>
    <TextBlock>This text is NOT on the fast path.</TextBlock>
<StackPanel/>
```

IsTextPerformanceVisualizationEnabled를 true로 설정하여 이 XAML을 디버그 모드에서 실행하면 결과는 다음과 같습니다.

![디버그 모드에서 렌더링된 텍스트](images/text-block-rendering-performance.png)

>**주의**&nbsp;&nbsp;빠른 경로에 있지 않은 텍스트의 색은 변경되지 않습니다. 앱에 밝은 녹색으로 지정된 텍스트가 있는 경우 텍스트가 더 느린 렌더링 경로에 있으면 계속 밝은 녹색으로 표시됩니다. 앱에서 녹색으로 설정된 텍스트와, 빠른 경로에 있으며 디버그 설정 때문에 녹색인 텍스트와 혼동하지 않도록 주의해야 합니다.

## 텍스트 서식 지정

Text 속성이 일반 텍스트를 저장하지만, TextBlock 컨트롤에 다양한 서식 옵션을 적용하여 앱에서 텍스트가 렌더링되는 방법을 사용자 지정할 수 있습니다. FontFamily, FontSize, FontStyle, Foreground 및 CharacterSpacing과 같은 표준 컨트롤 속성을 설정하여 텍스트의 모양을 변경할 수 있습니다. 또한 인라인 텍스트 요소 및 Typography 연결 속성을 사용하여 텍스트의 서식을 지정할 수 있습니다. 이러한 옵션은 TextBlock이 로컬에서 텍스트를 표시하는 방식에만 영향을 줍니다. 예를 들어 서식 있는 텍스트 컨트롤에 텍스트를 복사하여 붙여넣을 경우 서식이 적용되지 않습니다.

>**참고**&nbsp;&nbsp;이전 섹션에서 설명했듯이 인라인 텍스트 요소 및 기본값이 아닌 입력 체계 값은 빠른 경로에서 렌더링되지 않습니다.
 

### 인라인 요소

[Windows.UI.Xaml.Documents](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.aspx) 네임스페이스는 Bold, Italic, Run, Span 및 LineBreak와 같이 텍스트의 서식을 지정하는 데 사용할 수 있는 다양한 인라인 텍스트 요소를 제공합니다. 

TextBlock에서 일련의 문자열을 표시할 수 있으며, 여기서 각 문자열은 형식이 서로 다릅니다. 이렇게 하려면 Run 요소를 사용하여 각 문자열을 지정된 서식으로 표시하고 각 Run 요소를 LineBreak 요소로 분리하면 됩니다.

다음은 LineBreak로 분리된 Run 개체를 사용하여 TextBlock에서 서로 다른 서식이 지정된 여러 텍스트 문자열을 정의하는 방법입니다.
```xaml
<TextBlock FontFamily="Arial" Width="400" Text="Sample text formatting runs">
    <LineBreak/>
    <Run Foreground="Gray" FontFamily="Courier New" FontSize="24"> 
        Courier New 24 
    </Run>
    <LineBreak/>
    <Run Foreground="Teal" FontFamily="Times New Roman" FontSize="18" FontStyle="Italic"> 
        Times New Roman Italic 18 
    </Run>
    <LineBreak/>
    <Run Foreground="SteelBlue" FontFamily="Verdana" FontSize="14" FontWeight="Bold"> 
        Verdana Bold 14 
    </Run>
</TextBlock>
```

다음은 결과입니다.

![실행된 요소로 서식이 지정된 텍스트](images/text-block-run-examples.png)

### 입력 체계

[Typography](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx) 클래스의 연결된 속성을 통해 Microsoft OpenType 입력 체계 속성 집합에 액세스할 수 있습니다. TextBlock 또는 개별 인라인 텍스트 요소에서 이러한 연결된 속성을 설정할 수 있습니다. 다음 예에서는 두 가지를 모두 보여 줍니다.
```xaml
<TextBlock Text="Hello, world!"
           Typography.Capitals="SmallCaps"
           Typography.StylisticSet4="True"/>
```

```csharp
TextBlock textBlock1 = new TextBlock();
textBlock1.Text = "Hello, world!";
Windows.UI.Xaml.Documents.Typography.SetCapitals(textBlock1, FontCapitals.SmallCaps);
Windows.UI.Xaml.Documents.Typography.SetStylisticSet4(textBlock1, true);
```

```xaml
<TextBlock>12 x <Run Typography.Fraction="Slashed">1/3</Run> = 4.</TextBlock>
```

## 권장 사항



\[이 문서에는 UWP(유니버설 Windows 플랫폼) 앱 및 Windows 10과 관련된 정보가 있습니다. Windows 8.1 참고 자료는 [Windows 8.1 지침 PDF](https://go.microsoft.com/fwlink/p/?linkid=258743)를 다운로드하세요.\]

## 관련 문서

[텍스트 컨트롤](text-controls.md)

**디자이너용**
- [맞춤법 검사에 대한 지침](spell-checking-and-prediction.md)
- [검색 추가](https://msdn.microsoft.com/library/windows/apps/hh465231)
- [텍스트 입력에 대한 지침](text-controls.md)

**개발자용(XAML)**
- [**TextBox 클래스**](https://msdn.microsoft.com/library/windows/apps/br209683)
- [**Windows.UI.Xaml.Controls PasswordBox 클래스**](https://msdn.microsoft.com/library/windows/apps/br227519)


**개발자용(기타)**
- [String.Length 속성](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)


<!--HONumber=Mar16_HO1-->


