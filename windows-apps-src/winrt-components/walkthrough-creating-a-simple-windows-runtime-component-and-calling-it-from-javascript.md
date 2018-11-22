---
author: msatranjr
title: 간단한 Windows 런타임 구성 요소를 만들고 JavaScript에서 호출
description: 이 연습에서는 Visual Basic 또는 C#과 함께 .NET Framework를 사용하여 Windows 런타임 구성 요소로 패키지된 고유한 Windows 런타임 형식을 만드는 방법 및 JavaScript를 사용하여 Windows용으로 빌드된 유니버설 Windows 앱에서 구성 요소를 호출하는 방법을 보여 줍니다.
ms.assetid: 1565D86C-BF89-4EF3-81FE-35367DB8D671
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 49b9fe0833151155b11b7d7b796e395bb6a2ca7f
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7571973"
---
# <a name="walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript"></a>연습: 단순한 Windows 런타임 구성 요소를 만들고 JavaScript에서 이를 호출




이 연습에서는 Visual Basic 또는 C#과 함께 .NET Framework를 사용하여 Windows 런타임 구성 요소로 패키지된 고유한 Windows 런타임 형식을 만드는 방법 및 JavaScript를 사용하여 Windows용으로 빌드된 유니버설 Windows 앱에서 구성 요소를 호출하는 방법을 보여 줍니다.

Visual Studio에서는 쉽게 C# 또는 Visual Basic으로 작성된 Windows 런타임 구성 요소를 앱에 추가하고 JavaScript에서 호출할 수 있는 Windows 런타임 형식을 만들 수 있습니다. 내부적으로, Windows 런타임 형식에서는 유니버설 Windows 앱에 허용되는 .NET Framework 기능을 사용할 수 있습니다. (자세한 내용은 참조 [C# 및 Visual Basic에서 Windows 런타임 구성 만들기](creating-windows-runtime-components-in-csharp-and-visual-basic.md) 및 [.NET UWP 앱 개요에 대 한](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx)). 외부적으로 형식의 멤버의 매개 변수에 대 한 Windows 런타임 형식만 표시할 하 고 값을 반환할 수 있습니다. 솔루션을 빌드할 때 Visual Studio는 .NET Framework Windows 런타임 구성 요소 프로젝트를 빌드한 다음 Windows 메타데이터(.winmd) 파일을 만드는 빌드 단계를 실행합니다. 이는 Visual Studio가 앱에 포함하는 Windows 런타임 구성 요소입니다.

> **참고**.NET Framework Windows 런타임 이와 동일한에 기본 데이터 형식 및 컬렉션 형식과 같은 자주 사용 되는 일부.NET Framework 형식의 자동으로 매핑됩니다. 이러한 .NET Framework 형식은 Windows 런타임 구성 요소의 공용 인터페이스에서 사용할 수 있으며 구성 요소 사용자에게 해당 Windows 런타임 형식으로 표시됩니다. [C# 및 Visual Basic에서 Windows 런타임 구성 요소 만들기](creating-windows-runtime-components-in-csharp-and-visual-basic.md)를 참조하세요.

이 연습에서는 다음 작업을 보여 줍니다. JavaScript로 작성된 Windows 앱을 설정하는 첫 번째 섹션을 완료한 후 순서에 관계없이 나머지 섹션을 완료할 수 있습니다.

## <a name="prerequisites"></a>필수 조건:

-   Windows 10
-   Microsoft Visual Studio2015 또는 Microsoft Visual Studio Community2015

## <a name="creating-a-simple-windows-runtime-class"></a>간단한 Windows 런타임 클래스 만들기


이 섹션에서는 JavaScript를 사용하여 Windows용으로 빌드된 유니버설 Windows 앱을 만들고 Visual Basic 또는 C# Windows 런타임 구성 요소 프로젝트를 추가합니다. 관리되는 Windows 런타임 형식을 정의하고 JavaScript에서 형식의 인스턴스를 만들고 정적 및 인스턴스 멤버를 호출하는 방법을 보여 줍니다. 예제 앱의 시각적 표시는 구성 요소에 포커스를 유지하기 위해 의도적으로 흐릿하게 설정됩니다. 자유롭게 표시를 개선할 수 있습니다.

1.  Visual Studio에서 새 JavaScript 프로젝트를 만듭니다. 메뉴 모음에서 **파일, 새로 만들기, 프로젝트**를 선택합니다. **새 프로젝트** 대화 상자의 **설치된 템플릿** 섹션에서 **JavaScript**, **Windows**, **유니버설**을 차례로 선택합니다. Windows를 사용할 수 없는 경우 Windows 8 이상을 사용 중인지 확인합니다. **빈 응용 프로그램** 템플릿을 선택하고 프로젝트 이름에 SampleApp을 입력합니다.
2.  구성 요소 프로젝트를 만듭니다. 솔루션 탐색기에서 SampleApp 솔루션의 바로 가기 메뉴를 열고 **추가**, **새 프로젝트**를 차례로 선택하여 새 C# 또는 Visual Basic 프로젝트를 솔루션에 추가합니다. **새 프로젝트 추가** 대화 상자의 **설치된 템플릿** 섹션에서 **Visual Basic** 또는 **Visual C#** 을 선택한 다음 **Windows**, **유니버설**을 차례로 선택합니다. **Windows 런타임 구성 요소** 템플릿을 선택하고 프로젝트 이름에 **SampleComponent** 를 입력합니다.
3.  클래스 이름을 **Example**로 변경합니다. 기본적으로 클래스는 **public sealed**(Visual Basic의 경우 **Public NotInheritable**)로 표시됩니다. 구성 요소에서 노출하는 모든 Windows 런타임 클래스는 봉인해야 합니다.
4.  클래스에 두 개의 간단한 멤버인 **static** 메서드(Visual Basic의 경우 **Shared** 메서드) 및 인스턴스 속성을 추가합니다.

    > [!div class="tabbedCodeSnippets"]
    > ```csharp
    > namespace SampleComponent
    > {
    >     public sealed class Example
    >     {
    >         public static string GetAnswer()
    >         {
    >             return "The answer is 42.";
    >         }
    >
    >         public int SampleProperty { get; set; }
    >     }
    > }
    > ```
    > ```vb
    > Public NotInheritable Class Example
    >     Public Shared Function GetAnswer() As String
    >         Return "The answer is 42."
    >     End Function
    >
    >     Public Property SampleProperty As Integer
    > End Class
    > ```

5.  옵션: 새로 추가된 멤버에 IntelliSense를 사용하도록 설정하려면 솔루션 탐색기에서 SampleComponent 프로젝트의 바로 가기 메뉴를 열고 **빌드**를 선택합니다.
6.  솔루션 탐색기의 JavaScript 프로젝트에서 **참조**의 바로 가기 메뉴를 열고 **참조 추가**를 선택하여 **참조 관리자**를 엽니다. **프로젝트**를 선택한 다음 **솔루션**을 선택합니다. SampleComponent 프로젝트의 확인란을 선택한 다음 **확인**을 선택하여 참조를 추가합니다.

## <a name="call-the-component-from-javascript"></a>JavaScript에서 구성 요소 호출


JavaScript에서 Windows 런타임 형식을 사용하려면 Visual Studio 템플릿에서 제공하는 프로젝트 js 폴더의 default.js 파일에서 익명 함수에 다음 코드를 추가합니다. app.oncheckpoint 이벤트 처리기 뒤, app.start 호출 앞에 와야 합니다.

```javascript
var ex;

function basics1() {
   document.getElementById('output').innerHTML =
        SampleComponent.Example.getAnswer();

    ex = new SampleComponent.Example();

   document.getElementById('output').innerHTML += "<br/>" +
       ex.sampleProperty;

}

function basics2() {
    ex.sampleProperty += 1;
    document.getElementById('output').innerHTML += "<br/>" +
        ex.sampleProperty;
}
```

각 멤버 이름의 첫 글자가 대문자에서 소문자로 변경됩니다. 이 변환은 Windows 런타임을 자연스럽게 사용할 수 있도록 하기 위해 JavaScript에서 제공하는 지원의 일부입니다. 네임스페이스 및 클래스 이름은 Pascal Case됩니다. 멤버 이름은 모두 소문자인 이벤트 이름을 제외하고 Camel Case됩니다. [JavaScript에서 Windows 런타임 사용](https://msdn.microsoft.com/library/hh710230.aspx)을 참조하세요. Camel Casing 규칙은 혼동될 수 있습니다. 일련의 초기 대문자는 일반적으로 소문자로 표시되지만 대문자 3자 뒤에 소문자 하나가 오는 경우 처음 두 문자만 소문자로 표시됩니다. 예를 들어 IDStringKind라는 멤버는 idStringKind로 표시됩니다. Visual Studio에서 Windows 런타임 구성 요소 프로젝트를 빌드한 다음 JavaScript 프로젝트에 IntelliSense를 사용하여 올바른 대/소문자를 확인할 수 있습니다.

유사한 방식으로, .NET Framework는 관리 코드에서 Windows 런타임을 자연스럽게 사용할 수 있도록 지원합니다. 이 [Windows 런타임 구성 요소 만들기 C# 및 Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) 및 [UWP 앱 및 Windows 런타임에 대 한.NET Framework 지원](https://msdn.microsoft.com/library/hh694558.aspx)문서 및이 문서의 다음 섹션에서 설명 합니다.

## <a name="create-a-simple-user-interface"></a>간단한 사용자 인터페이스 만들기


JavaScript 프로젝트에서 default.html 파일을 열고 다음 코드에 표시된 대로 본문을 업데이트합니다. 이 코드는 예제 앱에 대한 전체 컨트롤 집합을 포함하며 클릭 이벤트에 대한 함수 이름을 지정합니다.

> **참고**앱을 처음 실행할 때는 Basics1 및 Basics2 단추만 지원 됩니다.

```html
<body>
            <div id="buttons">
            <button id="button1" >Basics 1</button>
            <button id="button2" >Basics 2</button>

            <button id="runtimeButton1">Runtime 1</button>
            <button id="runtimeButton2">Runtime 2</button>

            <button id="returnsButton1">Returns 1</button>
            <button id="returnsButton2">Returns 2</button>

            <button id="events1Button">Events 1</button>

            <button id="btnAsync">Async</button>
            <button id="btnCancel" disabled="disabled">Cancel Async</button>
            <progress id="primeProg" value="25" max="100" style="color: yellow;"></progress>
        </div>
        <div id="output">
        </div>
</body>
```

JavaScript 프로젝트의 css 폴더에서 default.css를 엽니다. 표시된 대로 본문 섹션을 수정하고 단추 레이아웃과 출력 텍스트의 위치를 제어하는 스타일을 추가합니다.

```css
body
{
    -ms-grid-columns: 1fr;
    -ms-grid-rows: 1fr 14fr;
    display: -ms-grid;
}

#buttons {
    -ms-grid-rows: 1fr;
    -ms-grid-columns: auto;
    -ms-grid-row-align: start;
}
#output {
    -ms-grid-row: 2;
    -ms-grid-column: 1;
}
```

이제 default.js의 app.onactivated에 있는 processAll 호출에 then 절을 추가하여 이벤트 수신기 등록 코드를 추가합니다. setPromise를 호출하는 기존 코드 줄을 바꿔 다음 코드로 변경합니다.

```javascript
args.setPromise(WinJS.UI.processAll().then(function () {
    var button1 = document.getElementById("button1");
    button1.addEventListener("click", basics1, false);
    var button2 = document.getElementById("button2");
    button2.addEventListener("click", basics2, false);
}));
```

HTML에서 직접 클릭 이벤트 처리기를 추가하는 것보다 이 방법으로 HTML 컨트롤에 이벤트를 추가하는 것이 더 낫습니다. ["Hello World" 앱 만들기(JS)](https://msdn.microsoft.com/library/windows/apps/mt280216)를 참조하세요.

## <a name="build-and-run-the-app"></a>앱 빌드 및 실행


빌드하기 전에 컴퓨터에 적절하게 모든 프로젝트의 대상 플랫폼을 ARM, x64 또는 x86으로 변경합니다.

솔루션을 빌드하고 실행하려면 F5 키를 선택합니다. SampleComponent가 정의되어 있지 없다는 런타임 오류 메시지가 표시되는 경우 클래스 라이브러리 프로젝트에 대한 참조가 없습니다.

Visual Studio는 먼저 클래스 라이브러리를 컴파일한 다음 [Winmdexp.exe(Windows 런타임 메타데이터 내보내기 도구)](https://msdn.microsoft.com/library/hh925576.aspx)를 실행하는 MSBuild 작업을 실행하여 Windows 런타임 구성 요소를 만듭니다. 구성 요소는 관리 코드와 코드를 설명하는 Windows 메타데이터 둘 다를 포함하는 .winmd 파일에 포함됩니다. WinMdExp.exe는 Windows 런타임 구성 요소에 유효하지 않은 코드를 작성하는 경우 빌드 오류 메시지를 생성하며 Visual Studio IDE에 오류 메시지가 표시됩니다. Visual Studio는 유니버설 Windows 앱에 대한 앱 패키지(.appx 파일)에 구성 요소를 추가하고 적절한 매니페스트를 생성합니다.

Basics 1 단추를 선택하여 정적 GetAnswer 메서드의 반환 값을 출력 영역에 할당하고, Example 클래스의 인스턴스를 만들고, 출력 영역에 해당 SampleProperty 속성 값을 표시합니다. 출력은 다음과 같습니다.

``` syntax
"The answer is 42."
0
```

Basics 2 단추를 선택하여 SampleProperty 속성 값을 증분하고 출력 영역에 새 값을 표시합니다. 문자열 및 숫자와 같은 기본 형식을 매개 변수 형식 및 반환 형식으로 사용할 수 있으며 관리 코드와 JavaScript 간에 전달할 수 있습니다. JavaScript의 숫자는 배정밀도 부동 소수점 형식으로 저장되므로 .NET Framework 숫자 형식으로 변환됩니다.

> **참고**기본적으로 JavaScript 코드 에서만에서 중단점을 설정할 수 있습니다. Visual Basic 또는 C# 코드를 디버그하려면 C# 및 Visual Basic에서 Windows 런타임 구성 요소 만들기를 참조하세요.

 

디버깅을 중지하고 앱을 닫으려면 앱에서 Visual Studio로 전환하고 Shift+F5를 선택합니다.

## <a name="using-the-windows-runtime-from-javascript-and-managed-code"></a>JavaScript와 관리 코드에서 Windows 런타임 사용


JavaScript 또는 관리 코드에서 Windows 런타임을 호출할 수 있습니다. 둘 간에 Windows 런타임 개체를 전달할 수 있으며, 어느 한 쪽에서 이벤트를 처리할 수 있습니다. 그러나 JavaScript 및 .NET Framework는 Windows 런타임을 다르게 지원하므로 두 환경에서 Windows 런타임 형식을 사용하는 방법이 일부 세부 정보에서 서로 다릅니다. 다음 예제에서는 [Windows.Foundation.Collections.PropertySet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.propertyset.aspx) 클래스를 사용하여 이러한 차이를 보여 줍니다. 이 예제에서는 관리 코드에서 PropertySet 컬렉션의 인스턴스를 만들고 이벤트 처리기를 등록하여 컬렉션의 변경 내용을 추적합니다. 그런 다음 컬렉션을 가져오고 자체 이벤트 처리기를 등록한 다음 컬렉션을 사용하는 JavaScript 코드를 추가합니다. 마지막으로, 관리 코드에서 컬렉션을 변경하고 관리되는 예외를 처리하는 JavaScript를 표시하는 메서드를 추가합니다.

> **중요 한**이 예제는 이벤트가 발생 하는 UI 스레드에서 합니다. 백그라운드 스레드(예: 비동기 호출)에서 이벤트가 발생하는 경우 JavaScript에서 이벤트를 처리하려면 몇 가지 추가 작업을 수행해야 합니다. 자세한 내용은 [Windows 런타임 구성 요소에서 이벤트 발생](raising-events-in-windows-runtime-components.md)을 참조하세요.

 

SampleComponent 프로젝트에서 PropertySetStats라는 새 **public sealed** 클래스(Visual Basic의 경우 **Public NotInheritable** 클래스)를 추가합니다. 클래스는 PropertySet 컬렉션을 래핑하고 해당 MapChanged 이벤트를 처리합니다. 이벤트 처리기가 발생하는 각 종류의 변경 내용 수를 추적하고, DisplayStats 메서드는 html로 형식이 지정된 보고서를 생성합니다. 추가 **using** 문(Visual Basic의 경우 **Imports** 문)을 확인합니다. 덮어쓰는 대신 기존 **using** 문에 이 문을 추가해야 합니다.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> using Windows.Foundation.Collections;
>
> namespace SampleComponent
> {
>     public sealed class PropertySetStats
>     {
>         private PropertySet _ps;
>         public PropertySetStats()
>         {
>             _ps = new PropertySet();
>             _ps.MapChanged += this.MapChangedHandler;
>         }
>
>         public PropertySet PropertySet { get { return _ps; } }
>
>         int[] counts = { 0, 0, 0, 0 };
>         private void MapChangedHandler(IObservableMap<string, object> sender,
>             IMapChangedEventArgs<string> args)
>         {
>             counts[(int)args.CollectionChange] += 1;
>         }
>
>         public string DisplayStats()
>         {
>             StringBuilder report = new StringBuilder("<br/>Number of changes:<ul>");
>             for (int i = 0; i < counts.Length; i++)
>             {
>                 report.Append("<li>" + (CollectionChange)i + ": " + counts[i] + "</li>");
>             }
>             return report.ToString() + "</ul>";
>         }
>     }
> }
> ```
> ```vb
> Imports System.Text
>
> Public NotInheritable Class PropertySetStats
>     Private _ps As PropertySet
>     Public Sub New()
>         _ps = New PropertySet()
>         AddHandler _ps.MapChanged, AddressOf Me.MapChangedHandler
>     End Sub
>
>     Public ReadOnly Property PropertySet As PropertySet
>         Get
>             Return _ps
>         End Get
>     End Property
>
>     Dim counts() As Integer = {0, 0, 0, 0}
>     Private Sub MapChangedHandler(ByVal sender As IObservableMap(Of String, Object),
>         ByVal args As IMapChangedEventArgs(Of String))
>
>         counts(CInt(args.CollectionChange)) += 1
>     End Sub
>
>     Public Function DisplayStats() As String
>         Dim report As New StringBuilder("<br/>Number of changes:<ul>")
>         For i As Integer = 0 To counts.Length - 1
>             report.Append("<li>" & CType(i, CollectionChange).ToString() &
>                           ": " & counts(i) & "</li>")
>         Next
>         Return report.ToString() & "</ul>"
>     End Function
> End Class
> ```

이벤트 처리기는 IObservableMap (이 경우 PropertySet 개체)에서 이벤트를 보낸 사람 캐스팅 된 한다는 점을 제외 하면 친숙 한.NET Framework 이벤트 패턴을 따릅니다&lt;문자열, 개체&gt; (IObservableMap (문자열, 개체)의 인터페이스 Visual Basic의 경우)는 Windows 런타임 인터페이스의 인스턴스 [IObservableMap&lt;K, V&gt;](https://msdn.microsoft.com/library/windows/apps/br226050.aspx)합니다. (캐스팅할 수 발신자의 형식에 필요한 경우.) 또한 이벤트 인수가 개체가 아닌 인터페이스로 표시 됩니다.

default.js 파일에서 Runtime1 함수를 표시된 대로 추가합니다. 이 코드는 PropertySetStats 개체를 만들고, 해당 PropertySet 컬렉션을 가져오고, 자체 이벤트 처리기인 onMapChanged 함수를 추가하여 MapChanged 이벤트를 처리합니다. 컬렉션을 변경한 후 runtime1은 DisplayStats 메서드를 호출하여 변경 유형에 대한 요약을 표시합니다.

```javascript
var propertysetstats;

function runtime1() {
    document.getElementById('output').innerHTML = "";

    propertysetstats = new SampleComponent.PropertySetStats();
    var propertyset = propertysetstats.propertySet;

    propertyset.addEventListener("mapchanged", onMapChanged);

    propertyset.insert("FirstProperty", "First property value");
    propertyset.insert("SuperfluousProperty", "Unnecessary property value");
    propertyset.insert("AnotherProperty", "A property value");

    propertyset.insert("SuperfluousProperty", "Altered property value")
    propertyset.remove("SuperfluousProperty");

    document.getElementById('output').innerHTML +=
        propertysetstats.displayStats();
}

function onMapChanged(change) {
    var result
    switch (change.collectionChange) {
        case Windows.Foundation.Collections.CollectionChange.reset:
            result = "All properties cleared";
            break;
        case Windows.Foundation.Collections.CollectionChange.itemInserted:
            result = "Inserted " + change.key + ": '" +
                change.target.lookup(change.key) + "'";
            break;
        case Windows.Foundation.Collections.CollectionChange.itemRemoved:
            result = "Removed " + change.key;
            break;
        case Windows.Foundation.Collections.CollectionChange.itemChanged:
            result = "Changed " + change.key + " to '" +
                change.target.lookup(change.key) + "'";
            break;
        default:
            break;
     }

     document.getElementById('output').innerHTML +=
         "<br/>" + result;
}
```

JavaScript에서 Windows 런타임 이벤트를 처리하는 방법은 .NET Framework 코드에서 처리하는 방법과 매우 다릅니다. JavaScript 이벤트 처리기는 하나의 인수만 사용합니다. Visual Studio 디버거에서 이 개체를 보는 경우 첫 번째 속성은 보낸 사람입니다. 이벤트 인수 인터페이스의 멤버도 이 개체에 직접 표시됩니다.

앱을 실행하려면 F5 키를 선택합니다. 클래스가 봉인되지 않은 경우 "봉인 되지 않은 형식 'SampleComponent.Example' 내보내기는 현재 지원되지 않습니다. 봉인됨으로 표시하세요."라는 오류 메시지가 표시됩니다.

**Runtime 1** 단추를 선택합니다. 이벤트 처리기는 요소를 추가하거나 변경할 때 변경 내용을 표시하고 끝에 DisplayStats 메서드가 호출되어 개수 요약을 생성합니다. 디버깅을 중지하고 앱을 닫으려면 Visual Studio로 다시 전환하고 Shift+F5를 선택합니다.

관리 코드에서 PropertySet 컬렉션에 두 항목을 더 추가하려면 PropertySetStats 클래스에 다음 코드를 추가합니다.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public void AddMore()
> {
>     _ps.Add("NewProperty", "New property value");
>     _ps.Add("AnotherProperty", "A property value");
> }
> ```
> ```vb
> Public Sub AddMore()
>     _ps.Add("NewProperty", "New property value")
>     _ps.Add("AnotherProperty", "A property value")
> End Sub
> ```

이 코드는 두 환경에서 Windows 런타임 형식을 사용하는 방법의 또 다른 차이점을 강조 표시합니다. 이 코드를 직접 입력하는 경우 IntelliSense가 JavaScript 코드에서 사용한 insert 메서드를 표시하지 않는 것을 확인할 수 있습니다. 대신 .NET Framework에서 컬렉션에 일반적으로 표시되는 Add 메서드를 표시합니다. 이는 Windows 런타임 및 .NET Framework에서 자주 사용하는 일부 컬렉션 인터페이스가 이름은 다르지만 기능이 유사하기 때문입니다. 관리 코드에서 이러한 인터페이스를 사용하면 해당 .NET Framework 인터페이스로 표시됩니다. 이 내용에 대해서는 [C# 및 Visual Basic에서 Windows 런타임 구성 요소 만들기](creating-windows-runtime-components-in-csharp-and-visual-basic.md)에서 설명합니다. JavaScript에서 동일한 인터페이스를 사용하는 경우 Windows 런타임에서 변경되는 내용은 멤버 이름 맨 앞의 대문자가 소문자로 바뀌는 것뿐입니다.

마지막으로, 예외 처리를 사용하여 AddMore 메서드를 호출하려면 default.js에 runtime2 함수를 추가합니다.

```javascript
function runtime2() {
   try {
      propertysetstats.addMore();
    }
   catch(ex) {
       document.getElementById('output').innerHTML +=
          "<br/><b>" + ex + "<br/>";
   }

   document.getElementById('output').innerHTML +=
       propertysetstats.displayStats();
}
```

이전과 동일한 방식으로 이벤트 처리기 등록 코드를 추가합니다.

```javascript
var runtimeButton1 = document.getElementById("runtimeButton1");
runtimeButton1.addEventListener("click", runtime1, false);
var runtimeButton2 = document.getElementById("runtimeButton2");
runtimeButton2.addEventListener("click", runtime2, false);
```

앱을 실행하려면 F5 키를 선택합니다. **Runtime 1**, **Runtime 2**를 차례로 선택합니다. JavaScript 이벤트 처리기가 컬렉션에 대한 첫 번째 변경 내용을 보고합니다. 그러나 두 번째 변경 내용은 중복 키를 갖습니다. .NET Framework 사전의 사용자는 Add 메서드에서 예외가 발생할 것을 예상하며 실제로 예외가 발생합니다. JavaScript는 .NET Framework 예외를 처리합니다.

> **참고**에서 JavaScript 코드에서는 예외 메시지를 표시할 수 없습니다. 메시지 텍스트가 스택 추적으로 바뀝니다. 자세한 내용은 C# 및 Visual Basic에서 Windows 런타임 구성 요소 만들기에서 "예외 발생"을 참조하세요.

반면, JavaScript에서 중복 키를 사용하여 insert 메서드를 호출한 경우 항목의 값이 변경되었습니다. 이러한 동작의 차이는 [C# 및 Visual Basic에서 Windows 런타임 구성 요소 만들기](creating-windows-runtime-components-in-csharp-and-visual-basic.md)에 설명된 대로 JavaScript 및 .NET Framework에서 Windows 런타임을 지원하는 방법이 서로 다르기 때문입니다.

## <a name="returning-managed-types-from-your-component"></a>구성 요소에서 관리되는 형식 반환


앞에서 설명한 대로 JavaScript 코드와 C# 또는 Visual Basic 코드 간에 자유롭게 네이티브 Windows 런타임 형식을 전달할 수 있습니다. 대체로 형식 이름 및 멤버 이름은 두 경우에서 동일합니다(JavaScript에서는 멤버 이름이 소문자로 시작된다는 점 제외). 그러나 이전 섹션에서 PropertySet 클래스는 관리 코드에서 다른 멤버를 갖는 것으로 표시되었습니다. 예를 들어 JavaScript에서는 insert 메서드를 호출했고 .NET Framework 코드에서는 Add 메서드를 호출했습니다. 이 섹션에서는 이러한 차이가 JavaScript에 전달되는 .NET Framework 형식에 미치는 영향을 설명합니다.

구성 요소에서 만들었거나 JavaScript에서 구성 요소에 전달한 Windows 런타임 형식을 반환하는 것 외에도 해당 Windows 런타임 형식인 것처럼 관리 코드에서 만든 관리되는 형식을 JavaScript에 반환할 수 있습니다. 런타임 클래스의 첫 번째 간단한 예제에서도 멤버의 매개 변수 및 반환 형식은 .NET Framework 형식인 Visual Basic 또는 C# 기본 형식이었습니다. 컬렉션에 대한 이 동작을 표시하려면 Example 클래스에 다음 코드를 추가하여 정수로 인덱싱된 일반 문자열 사전을 반환하는 메서드를 만듭니다.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static IDictionary<int, string> GetMapOfNames()
> {
>     Dictionary<int, string> retval = new Dictionary<int, string>();
>     retval.Add(1, "one");
>     retval.Add(2, "two");
>     retval.Add(3, "three");
>     retval.Add(42, "forty-two");
>     retval.Add(100, "one hundred");
>     return retval;
> }
> ```
> ```vb
> Public Shared Function GetMapOfNames() As IDictionary(Of Integer, String)
>     Dim retval As New Dictionary(Of Integer, String)
>     retval.Add(1, "one")
>     retval.Add(2, "two")
>     retval.Add(3, "three")
>     retval.Add(42, "forty-two")
>     retval.Add(100, "one hundred")
>     Return retval
> End Function
> ```

사전은 [Dictionary&lt;TKey, TValue&gt;](https://msdn.microsoft.com/library/xfhwa508.aspx)에 의해 구현되고 Windows 런타임 인터페이스에 매핑되는 인터페이스로 반환되어야 합니다. 이 경우 인터페이스는 IDictionary&lt;int, string&gt;(Visual Basic의 경우 IDictionary(Of Integer, String))입니다. Windows 런타임 형식 IMap&lt;int, string&gt;은 관리 코드에 전달될 때 IDictionary&lt;int, string&gt;으로 표시되며 관리 형식이 JavaScript에 전달될 때는 그 반대가 됩니다.

**중요 한**관리 되는 형식이 여러 인터페이스를 구현 하는 경우 JavaScript는 목록에서 처음 나타나는 인터페이스를 사용 합니다. 예를 들어 Dictionary&lt;int, string&gt;을 JavaScript 코드로 반환하는 경우 반환 형식으로 지정한 인터페이스에 관계없이 IDictionary&lt;int, string&gt;으로 나타납니다. 즉, 첫 번째 인터페이스가 나머지 인터페이스에 나타나는 멤버를 포함하고 있지 않은 경우 해당 멤버는 JavaScript에 표시되지 않습니다.

 

새 메서드를 테스트하고 사전을 사용하려면 default.js에 returns1 및 returns2 함수를 추가합니다.

```javascript
var names;

function returns1() {
    names = SampleComponent.Example.getMapOfNames();
    document.getElementById('output').innerHTML = showMap(names);
}

var ct = 7;

function returns2() {
    if (!names.hasKey(17)) {
        names.insert(43, "forty-three");
        names.insert(17, "seventeen");
    }
    else {
        var err = names.insert("7", ct++);
        names.insert("forty", "forty");
    }
    document.getElementById('output').innerHTML = showMap(names);
}

function showMap(map) {
    var item = map.first();
    var retval = "<ul>";

    for (var i = 0, len = map.size; i < len; i++) {
        retval += "<li>" + item.current.key + ": " + item.current.value + "</li>";
        item.moveNext();
    }
    return retval + "</ul>";
}
```

다른 이벤트 등록 코드와 동일한 then 블록에 이벤트 등록 코드를 추가합니다.

```javascript
var returnsButton1 = document.getElementById("returnsButton1");
returnsButton1.addEventListener("click", returns1, false);
var returnsButton2 = document.getElementById("returnsButton2");
returnsButton2.addEventListener("click", returns2, false);
```

이 JavaScript 코드와 관련해서 관찰할 몇 가지 흥미로운 점이 있습니다. 무엇보다, HTML에서 사전 콘텐츠를 표시하는 showMap 함수가 포함되어 있습니다. ShowMap에 대한 코드에서 반복 패턴을 확인합니다. .NET Framework에서 일반 IDictionary 인터페이스에는 First 메서드가 없으며, Size 메서드가 아니라 Count 속성에 의해 크기가 반환됩니다. JavaScript에서 IDictionary&lt;int, string&gt;은 Windows 런타임 형식 IMap&lt;int, string&gt;처럼 나타납니다. [IMap&lt;K,V&gt;](https://msdn.microsoft.com/library/windows/apps/br226042.aspx) 인터페이스를 참조하세요.

이전 예제와 같이 returns2 함수에서 JavaScript는 Insert 메서드(JavaScript의 insert)를 호출하여 사전에 항목을 추가합니다.

앱을 실행하려면 F5 키를 선택합니다. 사전의 초기 콘텐츠를 만들고 표시하려면 **Returns 1** 단추를 선택합니다. 사전에 항목을 두 개 더 추가하려면 **Returns 2** 단추를 선택합니다. Dictionary&lt;TKey, TValue&gt;에서 예상한 것처럼 항목이 삽입 순서대로 표시되는지 확인합니다. 정렬하려는 경우 GetMapOfNames에서 SortedDictionary&lt;int, string&gt;을 반환할 수 있습니다. 이전 예제에서 사용한 PropertySet 클래스는 Dictionary&lt;TKey, TValue&gt;와 다른 내부 조직을 사용합니다.

물론, JavaScript는 강력한 형식의 언어가 아니므로 강력한 형식의 제네릭 컬렉션을 사용하면 몇 가지 놀라운 결과가 나타날 수 있습니다. **Returns 2** 단추를 다시 선택합니다. JavaScript에서 "7"을 숫자 7로 강제 변환하고 ct에 저장된 숫자 7을 문자열로 강제 변환합니다. 또한 문자열 "forty"를 0으로 강제 변환합니다. 그러나 위에서 소개한 기능은 일부에 불과합니다. **Returns 2** 단추를 몇 번 더 선택합니다. 관리 코드에서 Add 메서드는 값이 올바른 형식으로 캐스팅된 경우에도 중복 키 예외를 생성합니다. 반면, Insert 메서드는 기존 키에 연결된 값을 업데이트하고 새 키가 사전에 추가되었는지 여부를 나타내는 부울 값을 반환합니다. 이 때문에 키 7과 연결된 값이 계속 변경됩니다.

다른 예기치 않은 동작이 있습니다. 할당되지 않은 JavaScript 변수를 문자열 인수로 전달하는 경우 "undefined" 문자열을 가져옵니다. 즉, JavaScript 코드에 .NET Framework 컬렉션 형식을 전달할 때는 주의하세요.

> **참고**연결할 텍스트가 많은 경우 할 수 있는 것 보다 효율적으로 코드를.NET Framework 메서드로 이동 하 고 StringBuilder 클래스를 사용 하 여 showMap 함수와 같이 합니다.

Windows 런타임 구성 요소에서 고유한 제네릭 형식을 표시할 수는 없지만 다음과 같은 코드를 사용하여 Windows 런타임 클래스에 대한 .NET Framework 제네릭 컬렉션을 반환할 수 있습니다.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static object GetListOfThis(object obj)
> {
>     Type target = obj.GetType();
>     return Activator.CreateInstance(typeof(List<>).MakeGenericType(target));
> }
> ```
> ```vb
> Public Shared Function GetListOfThis(obj As Object) As Object
>     Dim target As Type = obj.GetType()
>     Return Activator.CreateInstance(GetType(List(Of )).MakeGenericType(target))
> End Function
> ```

List&lt;T&gt;는 JavaScript에서 Windows 런타임 형식 IVector&lt;T&gt;로 표시되는 IList&lt;T&gt;를 구현합니다.

## <a name="declaring-events"></a>이벤트 선언


표준 .NET Framework 이벤트 패턴 또는 Windows 런타임에서 사용되는 기타 패턴을 사용하여 이벤트를 선언할 수 있습니다. .NET Framework는 System.EventHandler&lt;TEventArgs&gt; 대리자와 Windows 런타임 EventHandler&lt;T&gt; 대리자 간의 동등성을 지원하므로 EventHandler&lt;TEventArgs&gt;를 사용하는 것이 표준 .NET Framework 패턴을 구현하는 좋은 방법입니다. 이 작동 방식을 확인하려면 SampleComponent 프로젝트에 다음 클래스 쌍을 추가합니다.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> namespace SampleComponent
> {
>     public sealed class Eventful
>     {
>         public event EventHandler<TestEventArgs> Test;
>         public void OnTest(string msg, long number)
>         {
>             EventHandler<TestEventArgs> temp = Test;
>             if (temp != null)
>             {
>                 temp(this, new TestEventArgs()
>                 {
>                     Value1 = msg,
>                     Value2 = number
>                 });
>             }
>         }
>     }
>
>     public sealed class TestEventArgs
>     {
>         public string Value1 { get; set; }
>         public long Value2 { get; set; }
>     }
> }
> ```
> ```vb
> Public NotInheritable Class Eventful
>     Public Event Test As EventHandler(Of TestEventArgs)
>     Public Sub OnTest(ByVal msg As String, ByVal number As Long)
>         RaiseEvent Test(Me, New TestEventArgs() With {
>                             .Value1 = msg,
>                             .Value2 = number
>                             })
>     End Sub
> End Class
>
> Public NotInheritable Class TestEventArgs
>     Public Property Value1 As String
>     Public Property Value2 As Long
> End Class
> ```

Windows 런타임에서 이벤트를 노출하는 경우 이벤트 인수 클래스가 System.Object에서 상속합니다. EventArgs는 Windows 런타임 형식이 아니기 때문에 .NET Framework와 같이 System.EventArgs에서 상속되지 않습니다.

이벤트에 대해 사용자 지정 이벤트 접근자(Visual Basic의 경우 **Custom** 키워드)를 선언하는 경우 Windows 런타임 이벤트 패턴을 사용해야 합니다. [Windows 런타임 구성 요소의 사용자 지정 이벤트 및 이벤트 접근자](custom-events-and-event-accessors-in-windows-runtime-components.md)를 참조하세요.

Test 이벤트를 처리하려면 default.js에 events1 함수를 추가합니다. events1 함수는 Test 이벤트에 대한 이벤트 처리기 함수를 만들고 즉시 OnTest 메서드를 호출하여 이벤트를 발생시킵니다. 이벤트 처리기의 본문에 중단점을 배치하는 경우 단일 매개 변수에 전달된 개체가 원본 개체와 TestEventArgs의 두 멤버를 모두 포함하는 것을 확인할 수 있습니다.

```javascript
var ev;

function events1() {
   ev = new SampleComponent.Eventful();
   ev.addEventListener("test", function (e) {
       document.getElementById('output').innerHTML = e.value1;
       document.getElementById('output').innerHTML += "<br/>" + e.value2;
   });
   ev.onTest("Number of feet in a mile:", 5280);
}
```

다른 이벤트 등록 코드와 동일한 then 블록에 이벤트 등록 코드를 추가합니다.

```javascript
var events1Button = document.getElementById("events1Button");
events1Button.addEventListener("click", events1, false);
```

## <a name="exposing-asynchronous-operations"></a>비동기 작업 노출


.NET Framework에는 작업 및 제네릭 [Task&lt;TResult&gt;](https://msdn.microsoft.com/library/dd321424.aspx) 클래스에 따라 비동기 처리 및 병렬 처리를 위한 풍부한 도구 집합이 있습니다. Windows 런타임 구성 요소에 작업 기반 비동기 처리를 노출하려면 Windows 런타임 인터페이스 [IAsyncAction](https://msdn.microsoft.com/library/br205781.aspx), [IAsyncActionWithProgress&lt;TProgress&gt;](https://msdn.microsoft.com/library/br205784.aspx), [IAsyncOperation&lt;TResult&gt;](https://msdn.microsoft.com/library/br205802.aspx) 및 [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](https://msdn.microsoft.com/library/br205807.aspx)를 사용합니다. Windows 런타임에서 작업(operation)은 결과를 반환하지만 작업(action)은 반환하지 않습니다.

이 섹션에서는 진행률을 보고하고 결과를 반환하는 취소할 수 있는 비동기 작업을 보여 줍니다. GetPrimesInRangeAsync 메서드는 [AsyncInfo](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.asyncinfo.aspx) 클래스를 사용하여 작업을 생성하고 해당 취소 및 진행률 보고 기능을 WinJS.Promise 개체에 연결합니다. 먼저 Example 클래스에 GetPrimesInRangeAsync 메서드를 추가합니다.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> using System.Runtime.InteropServices.WindowsRuntime;
> using Windows.Foundation;
>
> public static IAsyncOperationWithProgress<IList<long>, double>
> GetPrimesInRangeAsync(long start, long count)
> {
>     if (start < 2 || count < 1) throw new ArgumentException();
>
>     return AsyncInfo.Run<IList<long>, double>((token, progress) =>
>
>         Task.Run<IList<long>>(() =>
>         {
>             List<long> primes = new List<long>();
>             double onePercent = count / 100;
>             long ctProgress = 0;
>             double nextProgress = onePercent;
>
>             for (long candidate = start; candidate < start + count; candidate++)
>             {
>                 ctProgress += 1;
>                 if (ctProgress >= nextProgress)
>                 {
>                     progress.Report(ctProgress / onePercent);
>                     nextProgress += onePercent;
>                 }
>                 bool isPrime = true;
>                 for (long i = 2, limit = (long)Math.Sqrt(candidate); i <= limit; i++)
>                 {
>                     if (candidate % i == 0)
>                     {
>                         isPrime = false;
>                         break;
>                     }
>                 }
>                 if (isPrime) primes.Add(candidate);
>
>                 token.ThrowIfCancellationRequested();
>             }
>             progress.Report(100.0);
>             return primes;
>         }, token)
>     );
> }
> ```
> ```vb
> Imports System.Runtime.InteropServices.WindowsRuntime
>
> Public Shared Function GetPrimesInRangeAsync(ByVal start As Long, ByVal count As Long)
> As IAsyncOperationWithProgress(Of IList(Of Long), Double)
>
>     If (start < 2 Or count < 1) Then Throw New ArgumentException()
>
>     Return AsyncInfo.Run(Of IList(Of Long), Double)( _
>         Function(token, prog)
>             Return Task.Run(Of IList(Of Long))( _
>                 Function()
>                     Dim primes As New List(Of Long)
>                     Dim onePercent As Long = count / 100
>                     Dim ctProgress As Long = 0
>                     Dim nextProgress As Long = onePercent
>
>                     For candidate As Long = start To start + count - 1
>                         ctProgress += 1
>
>                         If ctProgress >= nextProgress Then
>                             prog.Report(ctProgress / onePercent)
>                             nextProgress += onePercent
>                         End If
>
>                         Dim isPrime As Boolean = True
>                         For i As Long = 2 To CLng(Math.Sqrt(candidate))
>                             If (candidate Mod i) = 0 Then
>                                 isPrime = False
>                                 Exit For
>                             End If
>                         Next
>
>                         If isPrime Then primes.Add(candidate)
>
>                         token.ThrowIfCancellationRequested()
>                     Next
>                     prog.Report(100.0)
>                     Return primes
>                 End Function, token)
>         End Function)
> End Function
> ```

GetPrimesInRangeAsync는 매우 간단한 소수 찾기이며 이는 의도된 것입니다. 여기서는 비동기 작업 구현에 중점을 두므로 단순성이 중요하며 취소를 보여 주는 경우 느린 구현이 도움이 됩니다. GetPrimesInRangeAsync는 소수만 사용하는 대신 해당 제곱근보다 작거나 같은 모든 정수로 후보를 나누어 무차별 대입(brute force)으로 소수를 찾습니다. 이 코드를 단계별로 실행

-   비동기 작업을 시작하기 전에 매개 변수 유효성 검사 및 잘못된 입력에 대한 예외 발생과 같은 정리 작업을 수행합니다.
-   이 구현의 관건은 [AsyncInfo.Run&lt;TResult, TProgress&gt;(Func&lt;CancellationToken, IProgress&lt;TProgress&gt;, Task&lt;TResult&gt;](https://msdn.microsoft.com/library/hh779740.aspx)&gt;) 메서드와 메서드의 유일한 매개 변수인 대리자입니다. 대리자는 취소 토큰과 진행률 보고를 위한 인터페이스를 수락하고 해당 매개 변수를 사용하는 시작된 작업을 반환해야 합니다. JavaScript가 GetPrimesInRangeAsync 메서드를 호출하는 경우 다음 단계가 수행됩니다(여기에 제공된 순서와 다를 수 있음).

    -   [WinJS.Promise](https://msdn.microsoft.com/library/windows/apps/br211867.aspx) 개체는 반환된 결과를 처리하고, 취소에 대응하고, 진행률 보고서를 처리할 함수를 제공합니다.
    -   AsyncInfo.Run 메서드는 취소 원본과 IProgress&lt;T&gt; 인터페이스를 구현하는 개체를 만듭니다. 대리자에게 취소 원본의 [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) 토큰 및 [IProgress&lt;T&gt;](https://msdn.microsoft.com/library/hh138298.aspx) 인터페이스 둘 다를 전달합니다.

        > **참고**Promise 개체가 취소에 대응 하는 함수를 제공 하지 않는 asyncinfo.run은 취소할 수 있는 토큰을 전달 및 취소 취소가 발생할 수 있습니다. Promise 개체가 진행률 업데이트를 처리하는 함수를 제공하지 않는 경우에도 AsyncInfo.Run은 IProgress&lt;T&gt;를 구현하는 개체를 제공하지만 해당 보고서는 무시됩니다.

    -   대리자는 [Task.Run&lt;TResult&gt;(Func&lt;TResult&gt;, CancellationToken](https://msdn.microsoft.com/library/hh160376.aspx)) 메서드를 통해 토큰과 진행률 인터페이스를 사용하는 시작된 작업을 만듭니다. 시작된 작업에 대한 대리자는 원하는 결과를 계산하는 람다 함수에 의해 제공됩니다. 잠시 후에 자세히 설명하겠습니다.
    -   AsyncInfo.Run 메서드는 [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](https://msdn.microsoft.com/library/windows/apps/br206594.aspx) 인터페이스를 구현하는 개체를 만들고, Windows 런타임 취소 메커니즘을 토큰 원본에 연결하고, Promise 개체의 진행률 보고 함수를 &lt;T&gt; 인터페이스에 연결합니다.
    -   IAsyncOperationWithProgress&lt;TResult, TProgress&gt; 인터페이스가 JavaScript에 반환됩니다.

-   시작된 작업이 나타내는 람다 함수는 인수를 사용하지 않습니다. 람다 함수이기 때문에 토큰 및 IProgress 인터페이스에 액세스할 수 있습니다. 후보 숫자를 평가할 때마다 람다 함수는 다음을 수행합니다.

    -   진행률의 다음 백분율 지점에 도달했는지 여부를 확인합니다. 도달한 경우 람다 함수는 IProgress&lt;T&gt;.Report 메서드를 호출하며, Promise 개체가 진행률 보고를 위해 지정한 함수에 백분율이 전달됩니다.
    -   취소 토큰을 사용하여 작업이 취소된 경우 예외를 발생시킵니다. IAsyncOperationWithProgress&lt;TResult, TProgress&gt; 인터페이스가 상속하는 [IAsyncInfo.Cancel](https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncinfo.cancel.aspx) 메서드가 호출된 경우 AsyncInfo.Run 메서드가 설정하는 연결은 취소 토큰이 알림을 받도록 합니다.
-   람다 함수가 소수 목록을 반환하는 경우 WinJS.Promise 개체가 결과 처리를 위해 지정한 함수에 목록이 전달됩니다.

JavaScript promise를 만들고 취소 메커니즘을 설정하려면 default.js에 asyncRun 및 asyncCancel 함수를 추가합니다.

```javascript
var resultAsync;
function asyncRun() {
    document.getElementById('output').innerHTML = "Retrieving prime numbers.";
    btnAsync.disabled = "disabled";
    btnCancel.disabled = "";

    resultAsync = SampleComponent.Example.getPrimesInRangeAsync(10000000000001, 2500).then(
        function (primes) {
            for (i = 0; i < primes.length; i++)
                document.getElementById('output').innerHTML += " " + primes[i];

            btnCancel.disabled = "disabled";
            btnAsync.disabled = "";
        },
        function () {
            document.getElementById('output').innerHTML += " -- getPrimesInRangeAsync was canceled. -- ";

            btnCancel.disabled = "disabled";
            btnAsync.disabled = "";
        },
        function (prog) {
            document.getElementById('primeProg').value = prog;
        }
    );
}

function asyncCancel() {    
    resultAsync.cancel();
}
```

이전과 동일한 방식으로 이벤트 등록 코드를 잊지 마세요.

```javascript
var btnAsync = document.getElementById("btnAsync");
btnAsync.addEventListener("click", asyncRun, false);
var btnCancel = document.getElementById("btnCancel");
btnCancel.addEventListener("click", asyncCancel, false);
```

비동기 GetPrimesInRangeAsync 메서드를 호출하면 asyncRun 함수가 WinJS.Promise 개체를 만듭니다. 개체의 then 메서드는 반환된 결과를 처리하고, 오류에 대응하고(취소 포함), 진행률 보고서를 처리하는 세 개의 함수를 사용합니다. 이 예제에서는 반환된 결과가 출력 영역에서 인쇄됩니다. 취소 또는 완료는 작업을 시작 및 취소하는 단추를 다시 설정합니다. 진행률 보고는 진행률 컨트롤을 업데이트합니다.

asyncCancel 함수는 WinJS.Promise 개체의 cancel 메서드만 호출합니다.

앱을 실행하려면 F5 키를 선택합니다. 비동기 작업을 시작하려면 **Async** 단추를 선택합니다. 다음에 수행되는 작업은 컴퓨터 속도에 따라 달라집니다. 깜박일 시간이 있기 전에 진행률 표시줄이 완료되는 경우 GetPrimesInRangeAsync에 전달되는 시작 번호의 크기를 하나 이상의 10의 비율로 늘립니다. 테스트할 숫자 개수를 늘리거나 줄여 작업 기간을 미세 조정할 수 있지만 시작 번호 중간에 0을 추가하면 영향을 더 커집니다. 작업을 취소하려면 **Cancel Async** 단추를 선택합니다.

## <a name="related-topics"></a>관련 항목

* [.NET UWP 앱 개요](https://msdn.microsoft.com/library/windows/apps/xaml/br230302.aspx)
* [UWP 앱용 .NET](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx)
* [연습: 단순한 Windows 런타임 구성 요소를 만들고 JavaScript에서 이를 호출](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
