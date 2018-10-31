---
author: Jwmsft
Description: Use a RichTextBlock with RichTextBlockOverflow elements to create advanced text layouts.
title: RichTextBlock
ms.assetid: E4BE4B1B-418E-4075-88F1-22C09DDF8E45
label: Rich text block
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 16ebc375a72984af8bbc40823121d2d94689fcf2
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5840258"
---
# <a name="rich-text-block"></a>서식 있는 텍스트 블록

 

서식 있는 텍스트 블록은 단락, 인라인 UI 요소 또는 복잡한 텍스트 레이아웃에 대한 지원이 필요한 경우 사용할 수 있는 몇 가지 고급 텍스트 레이아웃 기능을 제공합니다.

> **중요 API**: [RichTextBlock 클래스](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx), [RichTextBlockOverflow 클래스](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx), [Paragraph 클래스](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx), [Typography 클래스](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx)

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

여러 단락, 다중 열 또는 기타 복잡한 텍스트 레이아웃, 이미지와 같은 인라인 UI 요소에 대한 지원이 필요한 경우 **RichTextBlock**을 사용하세요.

앱에서 대부분의 읽기 전용 텍스트를 표시하려면 **TextBlock**을 사용합니다. 이 컨트롤을 사용하여 한 줄 또는 여러 줄 텍스트, 인라인 하이퍼링크 및 굵게, 기울임꼴 또는 밑줄 서식이 적용된 텍스트를 표시할 수 있습니다. TextBlock은 더 단순한 콘텐츠 모델을 제공하므로 일반적으로 더 사용하기 쉽고 RichTextBlock보다 더 우수한 텍스트 렌더링 성능을 제공할 수 있습니다. 대부분의 앱 UI 텍스트에 사용하는 것이 좋습니다. 텍스트에 줄 바꿈을 넣을 수는 있지만, TextBlock은 단일 단락을 표시하도록 디자인되었으며 텍스트 들여쓰기를 지원하지 않습니다.

올바른 텍스트 컨트롤을 선택하는 방법에 대한 자세한 내용은 [텍스트 컨트롤](text-controls.md) 문서를 참조하세요.

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/RichTextBlock">앱을 열고 작동 중인 RichTextBlock을 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-rich-text-block"></a>서식 있는 텍스트 블록 만들기

RichTextBlock의 콘텐츠 속성은 [Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx) 요소를 통해 단락 기반 텍스트를 지원하는 [Blocks](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.blocks.aspx) 속성입니다. 쉽게 앱에서 컨트롤의 텍스트 콘텐츠에 액세스하는 데 사용할 수 있는 **Text** 속성이 없습니다. 그러나 RichTextBlock은 TextBlock에서 제공하지 않는 여러 고유한 기능을 제공합니다. 

RichTextBlock은 다음을 지원합니다.
- 여러 단락. [TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.textindent.aspx) 속성을 설정하여 단락에 대해 들여쓰기를 설정할 수 있습니다.
- 인라인 UI 요소. [InlineUIContainer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.inlineuicontainer.aspx)를 사용하여 이미지와 같은 UI 요소를 인라인 텍스트로 표시할 수 있습니다.
- 오버플로 컨테이너. [RichTextBlockOverflow](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx) 요소를 사용하여 다중 열 텍스트 레이아웃을 만들 수 있습니다.

### <a name="paragraphs"></a>단락

[Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx) 요소를 사용하여 RichTextBlock 컨트롤 내에 표시할 텍스트 블록을 정의할 수 있습니다. 모든 RichTextBlock은 Paragraph를 하나 이상 포함해야 합니다. 

RichTextBlock.TextIndent 속성을 설정하여 [RichTextBlock](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.textindent.aspx)에서 모든 단락에 대한 들여쓰기 양을 설정할 수 있습니다. [Paragraph.TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.textindent.aspx) 속성을 다른 값으로 설정하면 RichTextBlock에서 특정 단락에 대한 이 설정을 재정의할 수 있습니다.

```xaml
<RichTextBlock TextIndent="12">
  <Paragraph TextIndent="24">First paragraph.</Paragraph>
  <Paragraph>Second paragraph.</Paragraph>
  <Paragraph>Third paragraph. <Bold>With an inline.</Bold></Paragraph>
</RichTextBlock>
```

### <a name="inline-ui-elements"></a>인라인 UI 요소

[InlineUIContainer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.inlineuicontainer.aspx) 클래스를 사용하면 UIElement 인라인 텍스트를 포함할 수 있습니다. 일반적인 시나리오는 Image 인라인 텍스트를 배치하는 것이지만, Button 또는 CheckBox와 같은 대화형 요소를 사용할 수도 있습니다.

동일한 위치에 둘 이상의 인라인 요소를 포함하려는 경우 패널을 단일 InlineUIContainer 자식으로 사용한 후 해당 패널 내에 여러 요소를 배치하는 것이 좋습니다.

이 예제는 InlineUIContainer를 사용하여 RichTextBlock에 이미지를 삽입하는 방법을 보여 줍니다. 

```xaml
<RichTextBlock>
    <Paragraph>
        <Italic>This is an inline image.</Italic>
        <InlineUIContainer>
            <Image Source="Assets/Square44x44Logo.png" Height="30" Width="30"/>
        </InlineUIContainer>
        Mauris auctor tincidunt auctor.
    </Paragraph>
</RichTextBlock>
```

## <a name="overflow-containers"></a>오버플로 컨테이너

[RichTextBlockOverflow](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx) 요소와 함께 RichTextBlock을 사용하여 다중 열 또는 기타 고급 페이지 레이아웃을 만들 수 있습니다. RichTextBlockOverflow 요소의 콘텐츠는 항상 RichTextBlock 요소에서 가져옵니다. RichTextBlock의 OverflowContentTarget 또는 다른 RichTextBlockOverflow로 설정하여 RichTextBlockOverflow 요소를 링크합니다.

다음은 2열 레이아웃을 만드는 간단한 예제입니다. 더 복잡한 예제는 예제 섹션을 참조하세요.

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <RichTextBlock Grid.Column="0" 
                   OverflowContentTarget="{Binding ElementName=overflowContainer}" >
        <Paragraph>
            Proin ac metus at quam luctus ultricies.
        </Paragraph>
    </RichTextBlock>
    <RichTextBlockOverflow x:Name="overflowContainer" Grid.Column="1"/>
</Grid>
```

## <a name="formatting-text"></a>텍스트 서식 지정

RichTextBlock이 일반 텍스트를 저장하지만, 다양한 서식 옵션을 적용하면 앱에서 텍스트를 렌더링하는 방법을 사용자 지정할 수 있습니다. FontFamily, FontSize, FontStyle, Foreground 및 CharacterSpacing과 같은 표준 컨트롤 속성을 설정하여 텍스트의 모양을 변경할 수 있습니다. 또한 인라인 텍스트 요소 및 Typography 연결 속성을 사용하여 텍스트의 서식을 지정할 수 있습니다. 이러한 옵션은 RichTextBlock이 로컬에서 텍스트를 표시하는 방식에만 영향을 줍니다. 예를 들어 서식 있는 텍스트 컨트롤에 텍스트를 복사하여 붙여넣을 경우 서식이 적용되지 않습니다.

### <a name="inline-elements"></a>인라인 요소

[Windows.UI.Xaml.Documents](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.aspx) 네임스페이스는 Bold, Italic, Run, Span 및 LineBreak와 같이 텍스트의 서식을 지정하는 데 사용할 수 있는 다양한 인라인 텍스트 요소를 제공합니다. 텍스트의 섹션에 서식을 적용하는 일반적인 방법은 Run 또는 Span 요소에 텍스트를 배치한 후 해당 요소에서 속성을 설정하는 것입니다.

다음은 첫 번째 구가 파란색의 16pt 텍스트로 굵게 표시되는 Paragraph입니다.

```xaml
<Paragraph>
    <Bold><Span Foreground="DarkSlateBlue" FontSize="16">Lorem ipsum dolor sit amet</Span></Bold>
    , consectetur adipiscing elit.
</Paragraph>
```

### <a name="typography"></a>입력 체계

[Typography](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx) 클래스의 연결된 속성을 통해 Microsoft OpenType 입력 체계 속성 집합에 액세스할 수 있습니다. 다음과 같이 RichTextBlock 또는 개별 인라인 텍스트 요소에서 이러한 연결된 속성을 설정할 수 있습니다.

```xaml
<RichTextBlock Typography.StylisticSet4="True">
    <Paragraph>
        <Span Typography.Capitals="SmallCaps">Lorem ipsum dolor sit amet</Span>
        , consectetur adipiscing elit.
    </Paragraph>
</RichTextBlock>
```

## <a name="recommendations"></a>권장 사항

글꼴에 대한 지침 및 입력 체계를 참조하세요.

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련 문서

[텍스트 컨트롤](text-controls.md)

**디자이너용**
- [맞춤법 검사에 대한 지침](text-controls.md)
- [검색 추가](https://msdn.microsoft.com/library/windows/apps/hh465231)
- [텍스트 입력에 대한 지침](text-controls.md)

**개발자용(XAML)**
- [TextBox 클래스](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Windows.UI.Xaml.Controls PasswordBox 클래스](https://msdn.microsoft.com/library/windows/apps/br227519)


**개발자용(기타)**
- [String.Length 속성](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)
