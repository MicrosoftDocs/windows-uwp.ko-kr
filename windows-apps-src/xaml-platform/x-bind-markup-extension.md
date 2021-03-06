---
description: XBind 태그 확장은 바인딩에 대 한 고성능 대안입니다. xBind-Windows 10의 새로운 기능-바인딩과 더 나은 디버깅을 지 원하는 메모리와 메모리를 줄이고 실행 합니다.
title: xBind 태그 확장
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8f383174e0afccd9800a4fbf8e0db6c4f5ba9cf0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170787"
---
# <a name="xbind-markup-extension"></a>{x:Bind} 태그 확장

**참고**   **{X:bind}** 를 사용 하 여 앱에서 데이터 바인딩을 사용 하는 방법과 **{X:bind}** 및 **{binding}** 사이의 전체 비교를 사용 하는 방법에 대 한 일반적인 정보는 [데이터 바인딩 심층](../data-binding/data-binding-in-depth.md)분석을 참조 하세요.

{ **X:bind}** 태그 확장 (Windows 10의 경우 새로 만들기)은 **{Binding}** 의 대안입니다. **{X:bind}** 은 (는) **{Binding}** 보다 짧은 시간 내에 실행 되며 더 나은 디버깅을 지원 합니다.

XAML 컴파일 시간에 **{x:Bind}** 는 데이터 원본에 대한 속성에서 값을 가져오는 코드로 변환되고 태그에 지정된 속성에서 이를 설정합니다. 필요한 경우 데이터 원본 속성의 값 변경을 관찰하고 해당 변경 내용에 따라 자체적으로 새로 고치도록 바인딩 개체를 구성할 수 있습니다(`Mode="OneWay"`). 또한 필요한 경우 고유한 값 변경을 소스 속성에 다시 적용하도록 구성할 수도 있습니다(`Mode="TwoWay"`).

**{x:Bind}** 및 **{Binding}** 에서 만든 바인딩 개체는 기능적으로 거의 동일합니다. 그러나 **{x:Bind}** 는 컴파일 타임에 생성되는 특수 코드이며, **{Binding}** 은 범용 런타임 개체 검사를 사용합니다. 따라서 **{x:Bind}** 바인딩(컴파일된 바인딩이라고도 함)은 성능이 뛰어나고, 바인딩 식에 대한 컴파일 타임 유효성 검사를 제공하며, 페이지의 partial 클래스로 생성되는 코드 파일에 중단점을 설정할 수 있도록 하여 디버깅을 지원합니다. 이러한 파일은 `obj` 폴더(C#의 경우 이름이 `<view name>.g.cs`와 같음)에서 찾을 수 있습니다.

> [!TIP]
> **{x:Bind}** 의 기본 모드는 **OneTime**이며 **{Binding}** 의 기본 모드는 **OneWay**입니다. **OneWay**를 사용하면 연결 및 변경 감지 처리 시 더 많은 코드가 생성되기 때문에 성능을 이유로 기본 모드로 선택되었습니다. 명시적으로 OneWay 또는 TwoWay 바인딩을 사용할 모드를 지정할 수 있습니다. 또한 [x:DefaultBindMode](x-defaultbindmode-attribute.md)를 사용하여 태그 트리의 특정 세그먼트에서 **{x:Bind}** 의 기본 모드를 변경할 수 있습니다. 지정된 모드는 바인딩의 일부로 모드를 명시적으로 지정하지 않은 해당 요소와 하위 요소의 모든 **{x:Bind}** 식에 적용됩니다.

**{x:Bind}를 보여주는 샘플 앱**

-   [{x:Bind} 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind)
-   [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper)
-   [XAML 컨트롤 갤러리](https://github.com/Microsoft/Xaml-Controls-Gallery)

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object property="{x:Bind}" .../>
-or-
<object property="{x:Bind propertyPath}" .../>
-or-
<object property="{x:Bind bindingProperties}" .../>
-or-
<object property="{x:Bind propertyPath, bindingProperties}" .../>
-or-
<object property="{x:Bind pathToFunction.functionName(functionParameter1, functionParameter2, ...), bindingProperties}" .../>
```

| 용어 | Description |
|------|-------------|
| _propertyPath_ | 바인딩의 속성 경로를 지정하는 문자열. 자세한 내용은 [속성 경로](#property-path) 섹션을 참조하세요. |
| _bindingProperties_ |
| _속성 이름_ = _value_ \[ , _속성 이름_ = _값_\]* | 이름/값 쌍 구문을 사용하여 지정된 하나 이상의 바인딩 속성. |
| _propName_ | 바인딩 개체에 설정할 속성의 문자열 이름. 예: "Converter" |
| _value_ | 속성을 설정할 값. 인수 구문은 설정할 속성에 따라 다릅니다. 다음은 _propName_ = 값이 태그 확장인 인 속성_값_ 을 사용 하는 예제 `Converter={StaticResource myConverterClass}` 입니다. 자세한 내용은 아래의 [{x:Bind}로 설정할 수 있는 속성](#properties-that-you-can-set-with-xbind)을 참조하세요. |

## <a name="examples"></a>예

```XAML
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

이 예제 XAML에서는 **ListView.ItemTemplate** 속성에서 **{x:Bind}** 를 사용합니다. **x:DataType** 값의 선언에 유의하세요.

```XAML
  <DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

## <a name="property-path"></a>속성 경로

*PropertyPath*는 **{x:Bind}** 식에 대한 **Path**를 설정합니다. **Path**는 소스에 바인딩할 속성, 하위 속성, 필드 또는 메서드의 값을 지정하는 속성 경로입니다. **Path** 속성 이름을 명시적으로 지정하거나(`{x:Bind Path=...}`) 생략할 수 있습니다(`{x:Bind ...}`).

### <a name="property-path-resolution"></a>속성 경로 확인

**{x:Bind}** 는 **DataContext**를 기본 소스로 사용하지 않습니다. 대신 페이지 또는 사용자 컨트롤 자체를 사용합니다. 따라서 속성, 필드 및 메서드에 대한 페이지 또는 사용자 컨트롤의 코드 숨김으로 표시됩니다. 뷰 모델을 **{x:Bind}** 에 노출하려면 일반적으로 페이지 또는 사용자 컨트롤의 코드 숨김에 새 필드 또는 속성을 추가하면 됩니다. 속성 경로의 단계는 점(.)으로 구분되므로 연속적인 하위 속성을 트래버스하기 위해 여러 구분 기호를 포함할 수 있습니다. 바인딩할 개체를 구현하는 데 사용되는 프로그래밍 언어에 관계없이 점 구분 기호를 사용합니다.

예를 들어 페이지에서 **Text="{x:Bind Employee.FirstName}"** 은 **Employee** 멤버를 찾은 다음 **Employee**에서 반환되는 개체에서 **FirstName** 멤버를 찾습니다. 직원의 부양가족을 포함하는 속성에 항목 컨트롤을 바인딩하는 경우 속성 경로는 "Employee.Dependents"일 수 있으며 항목 컨트롤의 항목 템플릿은 "Dependents" 항목의 표시를 담당합니다.

C++/CX의 경우 **{x:Bind}** 는 페이지 또는 데이터 모델의 프라이빗 필드 및 속성에 바인딩할 수 없습니다. 바인딩하려면 공용 속성이 있어야 합니다. 관련 메타데이터를 가져올 수 있도록 바인딩 노출 영역을 CX 클래스/인터페이스로 노출해야 합니다. ** \[ 바인딩 \] ** 가능한 특성이 필요 하지 않습니다.

**x:Bind**를 사용하면 **ElementName=xxx**를 바인딩 식의 일부로 사용할 필요가 없습니다. 대신, 명명 된 요소가 루트 바인딩 소스를 나타내는 페이지 또는 사용자 정의 컨트롤 내의 필드가 되기 때문에 요소의 이름을 바인딩에 대 한 경로의 첫 번째 부분으로 사용할 수 있습니다.

### <a name="collections"></a>컬렉션

데이터 원본이 컬렉션인 경우 속성 패치가 컬렉션에서 위치 또는 색인별로 항목을 지정할 수 있습니다. 예를 들면 "팀 \[ 0" \] 입니다. 플레이어 ". 여기서 리터럴" \[ \] "은 0으로 인덱싱된 컬렉션의 첫 번째 항목을 요청 하는" 0 "을 포함 합니다.

인덱서를 사용하려면 메서드에서 인덱싱할 속성 형식에 대해 **IList&lt;T&gt;** 또는 **IVector&lt;T&gt;** 를 구현해야 합니다. (IReadOnlyList &lt; T &gt; 및 IVectorView &lt; T &gt; 는 인덱서 구문을 지원 하지 않습니다.) 인덱싱된 속성의 형식이 **INotifyCollectionChanged** 또는 **IObservableVector** 를 지원 하 고 바인딩이 OneWay 또는 TwoWay 인 경우 해당 인터페이스에 대 한 변경 알림을 등록 하 고 수신 대기 합니다. 변경 내용 검색 논리는 인덱싱된 특정 값에 영향을 주지 않는 경우에도 모든 컬렉션 변경 내용에 따라 업데이트됩니다.습니다 하는 경우에 모든 컬렉션 변경 내용에 따라 합니다. 이는 수신 대기 논리가 컬렉션의 모든 인스턴스에서 공통적이기 때문입니다.

데이터 원본이 사전 또는 맵인 경우 문자열 이름을 기준으로 속성 경로에서 컬렉션의 항목을 지정할 수 있습니다. 예: ** &lt; TextBlock Text = "{x:bind Players \[ ' John Smith ' \] }"/ &gt; ** 는 "john smith" 라는 사전의 항목을 찾습니다. 이름은 따옴표로 묶어야 하며 작은따옴표 또는 큰따옴표를 사용할 수 있습니다. 문자열에서 따옴표를 이스케이프할 때는 캐럿(^)을 사용할 수 있습니다. 일반적으로 XAML 특성에 사용 된 것과 다른 따옴표를 사용 하는 것이 가장 쉽습니다. (Ireadonlydictionary<string &lt; T &gt; 및 IMapView &lt; t &gt; 는 인덱서 구문을 지원 하지 않습니다.)

문자열 인덱서를 사용하려면 모델에서 인덱싱할 속성 형식에 대해 **IDictionary&lt;string, T&gt;** 또는 **IMap&lt;string, T&gt;** 를 구현해야 합니다. 인덱싱된 속성 형식이 **IObservableMap**을 지원하는 경우 바인딩이 OneWay 또는 TwoWay이면 해당 인터페이스에 대한 변경 알림을 등록하고 수신합니다. 변경 내용 검색 논리는 인덱싱된 특정 값에 영향을 주지 않는 경우에도 모든 컬렉션 변경 내용에 따라 업데이트됩니다.습니다 하는 경우에 모든 컬렉션 변경 내용에 따라 합니다. 이는 수신 대기 논리가 컬렉션의 모든 인스턴스에서 공통적이기 때문입니다.

### <a name="attached-properties"></a>연결된 속성

[연결 된 속성](./attached-properties-overview.md)에 바인딩하려면 클래스 및 속성 이름을 점 뒤의 괄호 안에 넣어야 합니다. 예를 들면 **Text="{x:Bind Button22.(Grid.Row)}"** 와 같습니다. Xaml 네임스페이스에서 속성이 선언되지 않은 경우 문서 헤드의 코드 네임스페이스에 매핑해야 하는 xml 네임스페이스를 접두사로 사용해야 합니다.

### <a name="casting"></a>캐스팅

컴파일된 바인딩이 강력한 형식이며 경로의 각 단계에 대한 형식을 확인합니다. 반환된 형식에 멤버가 없으면 컴파일 타임에 실패합니다. 개체의 실제 형식을 바인딩하도록 명령하는 캐스트를 지정할 수 있습니다.

다음 사례에서 **obj**는 형식 개체의 속성이지만 입력란을 포함하므로 **Text="{x:Bind ((TextBox)obj).Text}"** 또는 **Text="{x:Bind obj.(TextBox.Text)}"** 를 사용할 수 있습니다.

**groups3** **Text = "{x:bind ((data: SampleDataGroup) groups3 \[ 0 \] )의 groups3 필드 Title} "** 는 개체의 사전 이므로 **데이터를 SampleDataGroup**로 캐스팅 해야 합니다. xml **data:** 네임스페이스 접두사는 기본 XAML 네임스페이스의 일부가 아닌 코드 네임스페이스에 개체 형식을 매핑하는 데 사용됩니다.

_참고: c # 스타일 캐스트 구문은 연결 된 속성 구문 보다 더 유연 하며 앞으로 나오는 권장 구문입니다._

#### <a name="pathless-casting"></a>유효한 캐스팅

네이티브 바인딩 파서는 함수 매개 변수로 나타낼 키워드를 제공 하지 `this` 않지만 `{x:Bind (x:String)}` 함수 매개 변수로 사용 될 수 있는 유효한 캐스팅 (예:)을 지원 합니다. 따라서 `{x:Bind MethodName((namespace:TypeOfThis))}` 는 개념적으로 동일한 항목을 수행 하는 유효한 방법입니다 `{x:Bind MethodName(this)}` .

예제:

`Text="{x:Bind local:MainPage.GenerateSongTitle((local:SongItem))}"`

```xaml
<Page
    x:Class="AppSample.MainPage"
    ...
    xmlns:local="using:AppSample">

    <Grid>
        <ListView ItemsSource="{x:Bind Songs}">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:SongItem">
                    <TextBlock
                        Margin="12"
                        FontSize="40"
                        Text="{x:Bind local:MainPage.GenerateSongTitle((local:SongItem))}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

```csharp
namespace AppSample
{
    public class SongItem
    {
        public string TrackName { get; private set; }
        public string ArtistName { get; private set; }

        public SongItem(string trackName, string artistName)
        {
            ArtistName = artistName;
            TrackName = trackName;
        }
    }

    public sealed partial class MainPage : Page
    {
        public List<SongItem> Songs { get; }
        public MainPage()
        {
            Songs = new List<SongItem>()
            {
                new SongItem("Track 1", "Artist 1"),
                new SongItem("Track 2", "Artist 2"),
                new SongItem("Track 3", "Artist 3")
            };

            this.InitializeComponent();
        }

        public static string GenerateSongTitle(SongItem song)
        {
            return $"{song.TrackName} - {song.ArtistName}";
        }
    }
}
```

## <a name="functions-in-binding-paths"></a>바인딩 경로의 함수

Windows 10 버전 1607부터 **{x:Bind}** 는 함수를 바인딩 경로의 리프 단계로 사용할 수 있습니다. 이는 여러 가지 시나리오를 태그에서 사용할 수 있도록 하는 데이터 바인딩의 강력한 기능입니다. 자세한 내용은 [함수 바인딩](../data-binding/function-bindings.md) 을 참조 하세요.

## <a name="event-binding"></a>이벤트 바인딩

이벤트 바인딩은 컴파일된 바인딩에 대해 고유한 기능입니다. 이를 통해 코드 숨김에 대한 메서드 없이도 바인딩을 사용하여 이벤트 처리기를 지정할 수 있습니다. 예: **= "{x:Bind rootFrame. GoForward}"를 클릭**합니다.

이벤트의 경우 대상 메서드는 오버로드되어서는 안 되며, 다음을 충족해야 합니다.

- 이벤트의 서명과 일치해야 합니다.
- 또는 매개 변수가 없어야 합니다.
- 또는 이벤트 매개 변수 형식에서 할당 가능한 동일한 개수의 매개 변수 형식이 있어야 합니다.

생성된 코드 숨김에서 컴파일된 바인딩은 이벤트를 처리하여 모델의 메서드로 라우트합니다. 이를 통해 이벤트가 발생한 경우 바인딩 식의 경로를 평가합니다. 즉, 속성 바인딩과 달리 모델의 변경 내용을 추적하지 않습니다.

속성 경로의 문자열 구문에 대한 자세한 내용은 [Property-path 구문](property-path-syntax.md)을 참조하세요(**{x:Bind}** 에 대해 여기에 설명된 차이점에 유의).

## <a name="properties-that-you-can-set-with-xbind"></a>{x:Bind}로 설정할 수 있는 속성

태그 확장에서 설정될 수 있는 읽기/쓰기 속성이 여러 개이므로 **{x:Bind}** 은 *bindingProperties* 자리 표시자 구문을 통해 설명됩니다. 속성은 *쉼표로 구분 된 속성* = *값* 쌍을 사용 하 여 순서에 관계 없이 설정할 수 있습니다. 바인딩 식에는 줄바꿈을 포함할 수 없습니다. 일부 속성에는 형식 변환이 없는 형식이 필요하므로 해당 태그 확장이 **{x:Bind}** 에 중첩되어야 합니다.

이러한 속성은 [**Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding) 클래스의 속성과 거의 동일한 방식으로 작동합니다.

| 속성 | 설명 |
|----------|-------------|
| **경로** | 위의 [속성 경로](#property-path) 섹션을 참조하세요. |
| **변환기** | 바인딩 엔진이 호출하는 변환기 개체를 지정합니다. 변환기는 XAML에서 설정할 수 있지만 리소스 사전의 해당 개체에 대한 [{StaticResource} 태그 확장](staticresource-markup-extension.md) 참조에서 할당한 개체 인스턴스를 참조하는 경우에만 설정할 수 있습니다. |
| **ConverterLanguage** | 변환기가 사용할 문화권을 지정합니다. **ConverterLanguage** 를 설정 하는 경우 **변환기**를 설정 해야 합니다. 문화권이 표준 기반 식별자로 설정 됩니다. 자세한 내용은 [**ConverterLanguage**](/uwp/api/windows.ui.xaml.data.binding.converterlanguage)를 참조하세요. |
| **ConverterParameter** | 변환기 논리에서 사용될 수 있는 변환기 매개 변수를 지정합니다. **ConverterParameter** 를 설정 하는 경우 **변환기**를 설정 해야 합니다. 대부분의 변환기는 전달 된 값에서 변환 하는 데 필요한 모든 정보를 가져오는 간단한 논리를 사용 하며 **ConverterParameter** 값이 필요 하지 않습니다. **ConverterParameter** 매개 변수는 **ConverterParameter**에서 전달되는 내용을 수용하는 여러 논리가 있는 고급 변환기를 구현하기 위한 것입니다. 문자열이 아닌 값을 사용하는 변환기를 작성할 수도 있지만 일반적이지 않습니다. 자세한 내용은 [**ConverterParameter**](/uwp/api/windows.ui.xaml.data.binding.converterparameter)에서 설명을 참조하세요. |
| **FallbackValue** | 원본 또는 경로를 확인할 수 없을 때 표시할 값을 지정합니다. |
| **모드** | "OneTime", "OneWay" 또는 "TwoWay" 문자열 중 하나로 바인딩 모드를 지정 합니다. 기본값은 "OneTime"입니다. 이 값은 **{Binding}** 에 대한 기본값(대부분의 경우 "OneWay"임)과 다릅니다. |
| **TargetNullValue** | 원본 값이 확인되지만 명시적으로 **null**이 아닌 경우 표시할 값을 지정합니다. |
| **BindBack** | 양방향 바인딩의 반대 방향으로 사용할 함수를 지정합니다. |
| **UpdateSourceTrigger** | TwoWay 바인딩에서 컨트롤에서 모델로 변경을 다시 적용하는 시기를 지정합니다. 텍스트 상자를 제외한 모든 속성의 기본값은 PropertyChanged입니다. TextBox. Text는 LostFocus입니다.|

> [!NOTE]
> **{Binding}** 에서 **{x:Bind}** 로 태그를 변환하는 경우 **모드** 속성에 대한 기본값의 차이에 주의하세요.
> **[x:DefaultBindMode](./x-defaultbindmode-attribute.md)** X:bind 태그 트리의 특정 분야에 대 한 기본 모드를 변경 하려면 사용할 수 있습니다. 선택된 모드는 바인딩의 일부로 모드를 명시적으로 지정하지 않은 해당 요소와 하위 요소의 모든 x:Bind 식에 적용됩니다. OneWay를 사용하면 변경 검색을 연결 및 처리하기 위해 생성해야 할 코드가 더 많아지기 때문에 OneTime은 OneWay보다 성능이 우수합니다.

## <a name="remarks"></a>설명

**{x:Bind}** 는 생성된 코드를 사용하여 이점을 제공하기 때문에 컴파일 타임에 형식 정보가 필요합니다. 즉, 사전에 형식을 모르는 속성에 바인딩할 수 없습니다. 따라서 **Object** 형식이고 런타임에 변경될 수 있는 **DataContext** 속성에서 **{x:Bind}** 를 사용할 수 없습니다.

데이터 템플릿을 사용 하 여 **{X:bind}** 를 사용 하는 경우 [예제](#examples) 단원에 나와 있는 것 처럼 **x:datatype** 값을 설정 하 여 바인딩되는 유형을 지정 해야 합니다. 형식을 인터페이스 또는 기본 클래스 형식으로 설정한 다음 필요한 경우 캐스트를 사용하여 전체 식을 구성할 수 있습니다.

컴파일된 바인딩은 코드 생성에 따라 다릅니다. 따라서 리소스 사전의 **{x:Bind}** 를 사용하는 경우 리소스 사전에는 코드 숨김 클래스가 있어야 합니다. 코드 예제에 대해서는 [{x:Bind}를 사용하는 리소스 사전](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind)을 참조하세요.

컴파일된 바인딩이 포함된 페이지 및 사용자 컨트롤의 생성된 코드에는 "Bindings" 속성이 포함됩니다. 여기에는 다음 메서드가 포함됩니다.

- **Update()** - 컴파일된 모든 바인딩의 값을 업데이트합니다. 단방향/양방향 바인딩에는 변경 사항을 검색하기 위해 연결된 리스너가 있습니다.
- **Initialize()** - 바인딩이 아직 초기화되지 않은 경우 Update()를 호출하여 바인딩을 초기화합니다.
- **StopTracking()** - 단방향 및 양방향 바인딩을 위해 만들어진 모든 리스너의 연결이 해제됩니다. Update() 메서드를 사용하여 다시 초기화할 수 있습니다.

> [!NOTE]
> Windows 10 버전 1607부터 XAML 프레임워크는 기본 제공 부울-표시 변환기를 제공합니다. 변환기는 **표시 되** 는 열거형 값에 **True** 를 매핑하고 축소 되도록 **false** 를 **축소** 하므로 변환기를 만들지 않고도 표시 유형 속성을 부울에 바인딩할 수 있습니다. 이는 함수 바인딩이 아닌 속성 바인딩의 기능입니다. 기본 제공 변환기를 사용하려면 앱의 최소 대상 SDK 버전이 14393 이상이어야 합니다. 앱이 이전 버전의 Windows 10을 대상으로 하는 경우 기본 제공 변환기를 사용할 수 없습니다. 대상 버전에 대한 자세한 내용은 [버전 적응 코드](../debug-test-perf/version-adaptive-code.md)를 참조하세요.

**팁**   [**Path**](/uwp/api/windows.ui.xaml.data.binding.path) 또는 [**ConverterParameter**](/uwp/api/windows.ui.xaml.data.binding.converterparameter)와 같이 값에 대해 단일 중괄호를 지정 해야 하는 경우 백슬래시를 사용 하 여 앞에를 `\{` 붙입니다. 또는 보조 따옴표 집합에서 이스케이프해야 하는 괄호가 포함된 전체 문자열을 다음과 같이 묶습니다. `ConverterParameter='{Mix}'`.

[**Converter**](/uwp/api/windows.ui.xaml.data.binding.converter), [**ConverterLanguage**](/uwp/api/windows.ui.xaml.data.binding.converterlanguage)와 **ConverterLanguage**는 모두 바인딩 소스에서 바인딩 대상 속성과 호환 되는 형식 또는 값으로 값 또는 형식을 변환 하는 시나리오와 관련이 있습니다. 자세한 내용과 예제는 [데이터 바인딩 심층 분석](../data-binding/data-binding-in-depth.md)의 "데이터 변환" 섹션을 참조하세요.

**{x:Bind}** 는 태그 확장일 뿐이므로 이러한 바인딩을 프로그래밍 방식으로 만들거나 조작할 방법이 없습니다. 태그 확장에 대한 자세한 내용은 [XAML 개요](xaml-overview.md)를 참조하세요.