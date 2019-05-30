---
Description: 텍스트 컨트롤에 다양 한 데이터를 포함 하려면 콘텐츠 링크를 사용 합니다.
title: 텍스트 컨트롤의 콘텐츠 링크
label: Content links
template: detail.hbs
ms.date: 03/07/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 3fc54662b29255b73e972bcfb0fa4b6bb2dcf968
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66363054"
---
# <a name="content-links-in-text-controls"></a>텍스트 컨트롤의 콘텐츠 링크

콘텐츠 링크는 텍스트 컨트롤에 풍부한 데이터를 포함시킬 수 있는 방법을 제공하기 때문에 사용자는 앱의 컨텍스트를 벗어나지 않고도 사람이나 장소에 대해 더 많은 정보를 찾아서 활용할 수 있습니다.

사용자가 RichEditBox에서 앰퍼샌드(@) 기호를 항목 앞에 붙이면 항목과 일치되는 사람 및 장소 목록이 표시됩니다. 그런 다음, 예를 들어 사용자가 장소를 선택하면 해당 장소에 대한 ContentLink가 텍스트에 삽입됩니다. 사용자가 RichEditBox에서 콘텐츠 링크를 호출하면 지도 및 이 장소에 대한 추가 정보와 함께 플라이아웃이 표시됩니다.

> **중요 API**: [ContentLink 클래스](/uwp/api/windows.ui.xaml.documents.contentlink)하십시오 [ContentLinkInfo 클래스](/uwp/api/windows.ui.text.contentlinkinfo), [RichEditTextRange 클래스](/uwp/api/windows.ui.text.richedittextrange)

> [!NOTE]
> 콘텐츠 링크에 대 한 Api는 다음 네임 스페이스에 분산 됩니다. Windows.UI.Xaml.Controls Windows.UI.Xaml.Documents, 하며 Windows.UI.Text 합니다.



## <a name="content-links-in-rich-edit-vs-text-block-controls"></a>서식 있는 편집 상자의 콘텐츠 링크와 텍스트 블록 컨트롤의 콘텐츠 링크 비교

콘텐츠 링크를 사용할 수 있는 두 가지 방법이 있습니다.

1. [RichEditBox](/uwp/api/windows.ui.xaml.controls.richeditbox)에서 사용자는 선택기를 열고 @ 기호를 텍스트 앞에 붙여서 콘텐츠 링크를 추가할 수 있습니다. 콘텐츠 링크는 서식 있는 텍스트 콘텐츠의 일부로 저장됩니다.
1. [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) 또는 [RichTextBlock](/uwp/api/windows.ui.xaml.controls.richtextblock)에서 콘텐츠 링크는 사용 패턴과 동작이 [하이퍼링크](/uwp/api/windows.ui.xaml.documents.hyperlink)와 상당 부분 비슷한 텍스트 요소입니다.

아래에는 RichEditBox 및 TextBlock에서 콘텐츠 링크가 기본적으로 어떻게 나타나는지 나와 있습니다.

![콘텐츠 링크에서 서식 있는 입력란](images/content-link-default-richedit.png)
![텍스트 블록에 대 한 콘텐츠 링크](images/content-link-default-textblock.png)

사용 패턴, 렌더링 및 동작의 차이점은 다음 세션에서 자세히 다루겠습니다. 이 표에는 RichEditBox의 콘텐츠 링크와 텍스트 블록의 콘텐츠 링크 간의 가장 큰 차이점이 간략하게 비교되어 있습니다.

| 기능   | RichEditBox | 텍스트 블록 |
| --------- | ----------- | ---------- |
| 사용법 | ContentLinkInfo 인스턴스 | ContentLink 텍스트 요소 |
| Cursor | 콘텐츠 링크의 유형에 따라 결정되며 변경이 불가능 | 커서 속성에 의해 결정되며 기본값은 **null** |
| ToolTip | 렌더링 되지 않음 | 보조 텍스트 표시 |

## <a name="enable-content-links-in-a-richeditbox"></a>RichEditBox에서 콘텐츠 링크 활성화

가장 일반적인 콘텐츠 링크 용도는 사용자가 텍스트에서 사람 또는 장소 이름 앞에 앰퍼샌드(@) 기호를 붙여 신속하게 정보를 추가하기 위한 것입니다. [RichEditBox](/uwp/api/windows.ui.xaml.controls.richeditbox)에서 활성화가 되면 선택기가 열리고 사용자는 활성화된 항목에 따라 연락처 목록의 사람이나 근처 장소를 삽입할 수 있습니다.

서식 있는 텍스트 콘텐츠로 콘텐츠 링크를 저장할 수 있으며, 이를 추출해서 앱의 다른 부분에서 사용할 수 있습니다. 예를 들어, 이메일 앱에서 사람 정보를 추출하고 이를 이메일 주소에서 수신자 상자를 채우는 데 사용할 수 있습니다.

> [!NOTE]
> 콘텐츠 링크 선택기는 Windows에 포함된 앱이기 때문에 앱과 별도의 프로세스에서 실행됩니다.

### <a name="content-link-providers"></a>콘텐츠 링크 공급자

[RichEditBox.ContentLinkProviders](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkProviders) 컬렉션에 하나 이상의 콘텐츠 링크 공급자를 추가하여 RichEditBox에서 콘텐츠 링크를 활성화할 수 있습니다. XAML 프레임워크에서는 2개의 콘텐츠 링크 공급자가 기본 제공됩니다.

- [ContactContentLinkProvider](/uwp/api/windows.ui.xaml.documents.contactcontentlinkprovider) – **사람** 앱을 사용해 연락처를 조회합니다.
- [PlaceContentLinkProvider](/uwp/api/windows.ui.xaml.documents.placecontentlinkprovider) – **지도** 앱을 사용해 장소를 조회합니다.

> [!IMPORTANT]
> RichEditBox.ContentLinkProviders 속성에 대 한 기본값은 빈 컬렉션이 아니라 **null**입니다. 콘텐츠 링크 공급자를 추가하기 전에 [ContentLinkProviderCollection](/uwp/api/windows.ui.xaml.documents.contentlinkprovidercollection)을 명시적으로 생성해야 합니다.

XAML에 콘텐츠 링크 공급자를 추가하는 방법은 다음과 같습니다.

```xaml
<RichEditBox>
    <RichEditBox.ContentLinkProviders>
        <ContentLinkProviderCollection>
            <ContactContentLinkProvider/>
            <PlaceContentLinkProvider/>
        </ContentLinkProviderCollection>
    </RichEditBox.ContentLinkProviders>
</RichEditBox>
```

또한 스타일에서 콘텐츠 링크 공급자를 추가하고 이를 여러 RichEditBoxe에 적용할 수 있습니다.

```xaml
<Page.Resources>
    <Style TargetType="RichEditBox" x:Key="ContentLinkStyle">
        <Setter Property="ContentLinkProviders">
            <Setter.Value>
                <ContentLinkProviderCollection>
                    <PlaceContentLinkProvider/>
                    <ContactContentLinkProvider/>
                </ContentLinkProviderCollection>
            </Setter.Value>
        </Setter>
    </Style>
</Page.Resources>

<RichEditBox x:Name="RichEditBox01" Style="{StaticResource ContentLinkStyle}" />
<RichEditBox x:Name="RichEditBox02" Style="{StaticResource ContentLinkStyle}" />
```

코드에 콘텐츠 링크 공급자를 추가하는 방법은 다음과 같습니다.

```csharp
RichEditBox editor = new RichEditBox();
editor.ContentLinkProviders = new ContentLinkProviderCollection
{
    new ContactContentLinkProvider(),
    new PlaceContentLinkProvider()
};
```

### <a name="content-link-colors"></a>콘텐츠 링크 색상

콘텐츠 링크의 모양은 전경, 배경 및 아이콘에 의해 결정됩니다. RichEditBox에서 [ContentLinkForegroundColor](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkForegroundColor) 및 [ContentLinkBackgroundColor](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkBackgroundColor) 속성을 설정하여 기본 색상을 변경할 수 있습니다. 

커서를 설정할 수 없습니다. 콘텐츠 링크의 유형에 따라 RichEditbox에서 커서가 렌더링됩니다. 사람 링크의 경우 [사람](/uwp/api/windows.ui.core.corecursortype) 커서가, 장소 링크의 경우 [핀](/uwp/api/windows.ui.core.corecursortype) 커서가 렌더링됩니다.

### <a name="the-contentlinkinfo-object"></a>ContentLinkInfo 개체

사용자가 사람 또는 장소 선택기에서 원하는 것을 선택하면 시스템은 [ContentLinkInfo](/uwp/api/windows.ui.text.contentlinkinfo) 개체를 생성하고 이를 현재 [RichEditTextRange](/uwp/api/windows.ui.text.richedittextrange)의 **ContentLinkInfo** 속성에 추가합니다.

ContentLinkInfo 개체에는 콘텐츠 링크를 표시, 호출 및 관리하는 데 사용되는 정보가 포함되어 있습니다.

- **DisplayText** – 콘텐츠 링크가 렌더링될 때 표시되는 문자열입니다. RichEditBox에서 사용자는 생성된 콘텐츠 링크의 텍스트를 편집해서 이 속성의 값을 변경할 수 있습니다.
- **SecondaryText** -이 문자열은 렌더링된 콘텐츠 링크의 ToolTip에 표시됩니다.
  - 선택기에서 생성된 장소 콘텐츠 링크에는 위치의 주소가 포함됩니다.
- **Uri** – 콘텐츠 링크의 주제에 대한 자세한 내용을 제공하는 링크입니다. 이 Uri를 통해 설치된 앱이나 웹 사이트를 열 수 있습니다.
- **Id** - RichEditBox 컨트롤로 만든 컨트롤당 읽기 전용 카운터입니다. 삭제나 편집 같은 작업을 수행하는 동안 이 ContentLinkInfo를 추적하는 데 사용됩니다. 새 id를 가져오게 됩니다을 ContentLinkInfo 잘라내기는 컨트롤에 다시 붙여를 Id 값은 증분 합니다.
- **LinkContentKind** – 콘텐츠 링크의 유형을 설명하는 문자열입니다. 기본 제공되는 콘텐츠 유형은 _장소_ 및 _연락처_입니다. 이 값은 대/소문자를 구분합니다.

#### <a name="link-content-kind"></a>링크 콘텐츠 종류

LinkContentKind가 중요한 상황이 종종 있습니다.

- 사용자가 RichEditBox에서 콘텐츠 링크를 복사해서 다른 RichEditBox에 붙여 넣을 때는 두 컨트롤 모두가 해당 콘텐츠 유형에 대해 ContentLinkProvider를 가지고 있어야 합니다. 그렇지 않으면 링크가 텍스트로 붙여 넣기가 됩니다.
- [ContentLinkChanged](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkChanged) 이벤트 처리기에서 LinkContentKind를 사용해서 앱의 다른 부분에서 사용할 때 콘텐츠 링크에서 어떤 조치가 필요한지 결정할 수 있습니다. 자세한 내용은 예제 섹션을 참조하세요.
- LinkContentKind는 링크 호출 시 시스템이 Uri를 여는 방법에 영향을 미칩니다. 다음에 Uri 시작에 대해 논의할 때 이에 대해 살펴보겠습니다.

#### <a name="uri-launching"></a>Uri 시작

Uri 속성은 하이퍼링크의 NavigateUri 속성과 상당 부분 비슷하게 작동합니다. 사용자가 클릭을 하면 기본 브라우저나 Uri 값에 지정된 특정 프로토콜에 대해 등록된 앱에서 Uri가 시작됩니다.

아래에서는 기본 제공되는 2개의 링크 콘텐츠 유형에 대한 특정 동작을 설명합니다.

##### <a name="places"></a>장소

장소 선택기는 Uri 루트가 https://maps.windows.com/인 ContentLinkInfo를 생성합니다. 다음 세 가지 방법으로 이 링크를 열 수 있습니다.

- LinkContentKind가 "장소"인 경우에는 플라이아웃에서 정보 카드가 열립니다. 플라이아웃은 콘텐츠 링크 선택기 플라이아웃과 비슷합니다. 이는 Windows의 일부로, 앱과는 별도의 프로세스에서 실행됩니다.
- LinkContentKind가 "장소"가 아니면 지정된 위치로 **지도** 앱이 열립니다. 예를 들어, ContentLinkChanged 이벤트 처리기에서 LinkContentKind를 수정한 경우에 이런 일이 발생할 수 있습니다.
- 지도 앱에서 Uri가 열리지 않으면 기본 브라우저에서 지도가 열립니다. 사용자의 _웹 사이트용 앱_ 설정에서 **지도** 앱으로 Uri를 열 수 없도록 하고 있을 때 주로 이런 일이 발생합니다.

##### <a name="people"></a>피플

사람 선택기는 **ms-people** 프로토콜을 사용하는 Uri를 통해 ContentLinkInfo를 생성합니다.

- LinkContentKind가 "사람"인 경우에는 플라이아웃에서 정보 카드가 열립니다. 플라이아웃은 콘텐츠 링크 선택기 플라이아웃과 비슷합니다. 이는 Windows의 일부로, 앱과는 별도의 프로세스에서 실행됩니다.
- LinkContentKind가 "사람"이 아니면 **사람** 앱이 열립니다. 예를 들어, ContentLinkChanged 이벤트 처리기에서 LinkContentKind를 수정한 경우에 이런 일이 발생할 수 있습니다.

> [!TIP]
> 앱에서 다른 앱 및 웹 사이트 열기에 대 한 자세한 내용은 아래의 항목을 참조 하세요 [Uri 사용 하 여 앱을 시작](/windows/uwp/launch-resume/launch-app-with-uri)합니다.

#### <a name="invoked"></a>호출됨

사용자가 콘텐츠 링크를 호출하면 URI 시작을 위한 기본 동작이 개시되기 전에 [ContentLinkInvoked](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkInvoked) 이벤트가 발생됩니다. 이 이벤트를 처리하여 기본 동작을 재정의하거나 취소할 수 있습니다.

이 예제는 장소 콘텐츠 링크에 대한 기본 시작 동작을 재정의할 수 있는 방법을 보여줍니다. 장소 정보 카드, 지도 앱 또는 기본 웹 브라우저에서 지도를 여는 대신, 이벤트를 Handled로 표시하고 앱 내 [웹 보기](/uwp/api/windows.ui.xaml.controls.webview) 컨트롤에서 지도를 엽니다.

```xaml
<RichEditBox x:Name="editor"
             ContentLinkInvoked="editor_ContentLinkInvoked">
    <RichEditBox.ContentLinkProviders>
        <ContentLinkProviderCollection>
            <PlaceContentLinkProvider/>
        </ContentLinkProviderCollection>
    </RichEditBox.ContentLinkProviders>
</RichEditBox>

<WebView x:Name="webView1"/>
```

```csharp
private void editor_ContentLinkInvoked(RichEditBox sender, ContentLinkInvokedEventArgs args)
{
    if (args.ContentLinkInfo.LinkContentKind == "Places")
    {
        args.Handled = true;
        webView1.Navigate(args.ContentLinkInfo.Uri);
    }
}
```

#### <a name="contentlinkchanged"></a>ContentLinkChanged

[ContentLinkChanged](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkChanged) 이벤트를 사용하여 콘텐츠 링크가 추가, 수정 또는 제거될 때 이를 알릴 수 있습니다. 이렇게 하면 텍스트에서 콘텐츠 링크를 추출하고 이를 앱의 다른 장소에서 사용할 수 있습니다. 이에 대한 내용은 뒤의 "예제" 섹션에 나와 있습니다.

[ContentLinkChangedEventArgs](/uwp/api/windows.ui.xaml.controls.contentlinkchangedeventargs)의 속성은 다음과 같습니다.

- **ChangedKind** - 이 ContentLinkChangeKind 열거형은 ContentLink 내의 작업입니다. 예를 들어 ContentLink가 삽입, 제거 또는 편집된 경우입니다.
- **정보** - ContentLinkInfo가 작업 대상이었습니다.

각 ContentLinkInfo 작업에서 이 이벤트가 발생합니다. 예를 들어, 사용자가 한 번에 여러 콘텐츠 링크를 복사해서 RichEditBox에 붙여 넣으면 추가된 각 항목에 대해 이 이벤트가 발생합니다. 또는 사용자가 여러 콘텐츠 링크를 동시에 선택해 삭제하면 삭제된 각 항목에서 이 이벤트가 발생합니다.

**TextChanging** 이벤트 후, **TextChanged** 이벤트 전에 이 이벤트가 RichEditBox에서 발생합니다.

#### <a name="enumerate-content-links-in-a-richeditbox"></a>RichEditBox에 콘텐츠 링크를 열거

RichEditTextRange.ContentLink 속성을 사용해 서식 있는 편집 문서에서 콘텐츠 링크를 얻을 수 있습니다. 콘텐츠 링크를 텍스트 범위를 탐색할 때 사용할 단위로 지정할 수 있도록 TextRangeUnit 열거형에 ContentLink 값이 포함되어 있습니다.

이 예제는 RichEditBox에 모든 콘텐츠 링크를 열거하고 목록으로 사람을 추출하는 방법을 보여줍니다.

```xaml
<StackPanel Width="300">
    <RichEditBox x:Name="Editor" Height="200">
        <RichEditBox.ContentLinkProviders>
            <ContentLinkProviderCollection>
                <ContactContentLinkProvider/>
                <PlaceContentLinkProvider/>
            </ContentLinkProviderCollection>
        </RichEditBox.ContentLinkProviders>
    </RichEditBox>

    <Button Content="Get people" Click="Button_Click"/>

    <ListView x:Name="PeopleList" Header="People">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="ContentLinkInfo">
                <TextBlock>
                    <ContentLink Info="{x:Bind}" Background="Transparent"/>
                </TextBlock>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackPanel>
```

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    PeopleList.Items.Clear();
    RichEditTextRange textRange = Editor.Document.GetRange(0, 0) as RichEditTextRange;

    do
    {
        // The Expand method expands the Range EndPosition 
        // until it finds a ContentLink range. 
        textRange.Expand(TextRangeUnit.ContentLink);
        if (textRange.ContentLinkInfo != null
            && textRange.ContentLinkInfo.LinkContentKind == "People")
        {
            PeopleList.Items.Add(textRange.ContentLinkInfo);
        }
    } while (textRange.MoveStart(TextRangeUnit.ContentLink, 1) > 0);
}
```

## <a name="contentlink-text-element"></a>ContentLink 텍스트 요소

TextBlock 또는 RichTextBlock 컨트롤에서 콘텐츠 링크를 사용하려면 [ContentLink](/uwp/api/windows.ui.xaml.documents.contentlink) 텍스트 요소(Windows.UI.Xaml.Documents 네임스페이스에서 나온)를 사용합니다.

텍스트 블록의 ContentLink를 위한 일반적인 소스는 다음과 같습니다.

- RichTextBlock 컨트롤에서 추출한 선택기로 생성한 콘텐츠 링크입니다.
- 코드에서 생성하는 장소를 위한 콘텐츠 링크입니다.

다른 상황에서는 하이퍼링크 텍스트 요소가 일반적으로 적합합니다.

### <a name="contentlink-appearance"></a>ContentLink 모양

콘텐츠 링크의 모양은 전경, 배경 및 커서에 의해 결정됩니다. 텍스트 블록에서 전경(TextElement에서의) 및 [배경](/uwp/api/windows.ui.xaml.documents.contentlink.background) 속성을 설정하여 기본 설정을 변경할 수 있습니다.

기본적으로 사용자가 콘텐츠 링크에 마우스를 가져가면 [손](/uwp/api/windows.ui.core.corecursortype) 커서가 표시됩니다. RichEditBlock과 달리 텍스트 블록 컨트롤은 링크 유형에 따라 커서를 자동으로 변경하지 않습니다. [커서](/uwp/api/windows.ui.xaml.documents.contentlink.Cursor) 속성을 설정하여 링크 유형이나 기타 요인에 따라 커서를 변경할 수 있습니다.

TextBlock에서 사용되는 ContentLink의 예는 다음과 같습니다. ContentLinkInfo는 코드에서 생성되어 XAML에서 생성되는 ContentLink 텍스트 요소의 정보 속성에 할당됩니다.

```xaml
<StackPanel>
    <TextBlock>
        <Span xml:space="preserve">
            <Run>This valcano erupted in 1980: </Run><ContentLink x:Name="placeContentLink" Cursor="Pin"/>
            <LineBreak/>
        </Span>
    </TextBlock>

    <Button Content="Show" Click="Button_Click"/>
</StackPanel>
```

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    var info = new Windows.UI.Text.ContentLinkInfo();
    info.DisplayText = "Mount St. Helens";
    info.SecondaryText = "Washington State";
    info.LinkContentKind = "Places";
    info.Uri = new Uri("https://maps.windows.com?cp=46.1912~-122.1944");
    placeContentLink.Info = info;
}
```

> [!TIP]
> 텍스트 컨트롤에서 XAML의 다른 텍스트 요소와 함께 ContentLink를 사용할 때는 [Span](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.span) 컨테이너에 콘텐츠를 배치하고 `xml:space="preserve"` 속성을 Span에 적용하여 ContentLink와 기타 요소 간에 공백을 유지합니다.

## <a name="examples"></a>예

이 예제에서 사용자는 사람 또는 장소 콘텐츠 링크를 RickTextBlock에 입력할 수 있습니다. ContentLinkChanged 이벤트를 처리하여 콘텐츠 링크를 추출하고 사람 목록이나 장소 목록에서 이들을 최신 상태로 유지합니다.

이 목록에 대한 항목 템플릿에서 TextBlock과 함께 ContentLink 텍스트 요소를 사용하여 콘텐츠 링크 정보를 표시합니다. ListView는 각 항목에 대한 자체 배경을 제공하므로 ContentLink 배경이 "투명"으로 설정됩니다.

```xaml
<StackPanel Orientation="Horizontal" Grid.Row="1">
    <RichEditBox x:Name="Editor"
                 ContentLinkChanged="Editor_ContentLinkChanged"
                 Margin="20" Width="300" Height="200"
                 VerticalAlignment="Top">
        <RichEditBox.ContentLinkProviders>
            <ContentLinkProviderCollection>
                <ContactContentLinkProvider/>
                <PlaceContentLinkProvider/>
            </ContentLinkProviderCollection>
        </RichEditBox.ContentLinkProviders>
    </RichEditBox>

    <ListView x:Name="PeopleList" Header="People">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="ContentLinkInfo">
                <TextBlock>
                    <ContentLink Info="{x:Bind}"
                                 Background="Transparent"
                                 Cursor="Person"/>
                </TextBlock>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>

    <ListView x:Name="PlacesList" Header="Places">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="ContentLinkInfo">
                <TextBlock>
                    <ContentLink Info="{x:Bind}"
                                 Background="Transparent"
                                 Cursor="Pin"/>
                </TextBlock>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackPanel>
```

```csharp
private void Editor_ContentLinkChanged(RichEditBox sender, ContentLinkChangedEventArgs args)
{
    var info = args.ContentLinkInfo;
    var change = args.ChangeKind;
    ListViewBase list = null;

    // Determine whether to update the people or places list.
    if (info.LinkContentKind == "People")
    {
        list = PeopleList;
    }
    else if (info.LinkContentKind == "Places")
    {
        list = PlacesList;
    }

    // Determine whether to add, delete, or edit.
    if (change == ContentLinkChangeKind.Inserted)
    {
        Add();
    }
    else if (args.ChangeKind == ContentLinkChangeKind.Removed)
    {
        Remove();
    }
    else if (change == ContentLinkChangeKind.Edited)
    {
        Remove();
        Add();
    }

    // Add content link info to the list. It's bound to the
    // Info property of a ContentLink in XAML.
    void Add()
    {
        list.Items.Add(info);
    }

    // Use ContentLinkInfo.Id to find the item,
    // then remove it from the list using its index.
    void Remove()
    {
        var items = list.Items.Where(i => ((ContentLinkInfo)i).Id == info.Id);
        var item = items.FirstOrDefault();
        var idx = list.Items.IndexOf(item);

        list.Items.RemoveAt(idx);
    }
}
```
