---
title: C# 또는 Visual Basic Windows 런타임 구성 요소를 만들고 JavaScript에서 호출하는 연습
description: '이 연습에서는 Visual Basic 또는 c #에서 .NET을 사용 하 여 Windows 런타임 구성 요소에 패키지 된 고유의 Windows 런타임 형식을 만드는 방법과 JavaScript를 사용 하 여 Windows 용으로 빌드된 UWP 응용 프로그램에서 구성 요소를 호출 하는 방법을 보여 줍니다.'
ms.assetid: 1565D86C-BF89-4EF3-81FE-35367DB8D671
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e94a06d3a8a48959acaa25b57d049b2ddcc44d81
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174237"
---
# <a name="walkthrough-of-creating-a-c-or-visual-basic-windows-runtime-component-and-calling-it-from-javascript"></a>C# 또는 Visual Basic Windows 런타임 구성 요소를 만들고 JavaScript에서 호출하는 연습

이 연습에서는 Visual Basic 또는 c #에서 .NET을 사용 하 여 Windows 런타임 구성 요소에 패키지 된 고유한 Windows 런타임 형식을 만들고이 구성 요소를 UWP (JavaScript 유니버설 Windows 플랫폼) 앱에서 호출 하는 방법을 보여 줍니다.

Visual Studio를 사용 하면 c # 또는 Visual Basic를 사용 하 여 작성 된 WRC (Windows 런타임 구성 요소) 프로젝트 내에서 사용자 지정 Windows 런타임 형식을 쉽게 작성 및 배포한 다음 JavaScript 응용 프로그램 프로젝트에서 해당 WRC를 참조 하 고 해당 응용 프로그램에서 해당 사용자 지정 형식을 사용할 수 있습니다.

내부적으로 Windows 런타임 형식은 UWP 응용 프로그램에서 허용 되는 모든 .NET 기능을 사용할 수 있습니다.

> [!NOTE]
> 자세한 내용은 [c #을 사용 하 여 구성 요소 Windows 런타임 및 Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) 및 [UWP 앱 용 .net 개요](/dotnet/api/index?view=dotnet-uwp-10.0)를 참조 하세요.

외부적으로 형식의 멤버는 매개 변수 및 반환 값에 대 한 Windows 런타임 형식만 노출할 수 있습니다. 솔루션을 빌드하면 Visual Studio에서 .NET WRC 프로젝트를 빌드한 다음 Windows 메타 데이터 (winmd) 파일을 만드는 빌드 단계를 실행 합니다. Visual Studio가 앱에 포함 하는 Windows 런타임 구성 요소입니다.

> [!NOTE]
> .NET은 기본 데이터 형식 및 컬렉션 형식과 같은 일반적으로 사용 되는 일부 .NET 형식을 해당 Windows 런타임에 자동으로 매핑합니다. 이러한 .NET 형식은 Windows 런타임 구성 요소의 공용 인터페이스에서 사용 될 수 있으며 구성 요소의 사용자에 게 해당 하는 Windows 런타임 형식으로 표시 됩니다. [C # 및 Visual Basic를 사용 하 여 Windows 런타임 구성 요소](creating-windows-runtime-components-in-csharp-and-visual-basic.md)를 참조 하세요.

## <a name="prerequisites"></a>필수 조건:

- Windows 10
- [Microsoft Visual Studio](https://visualstudio.microsoft.com/downloads/)

## <a name="creating-a-simple-windows-runtime-class"></a>간단한 Windows 런타임 클래스 만들기

이 섹션에서는 JavaScript UWP 응용 프로그램을 만들고 Visual Basic 또는 c # Windows 런타임 구성 요소 프로젝트를 솔루션에 추가 합니다. Windows 런타임 형식을 정의 하 고 JavaScript에서 형식의 인스턴스를 만든 다음 정적 멤버와 인스턴스 멤버를 호출 하는 방법을 보여 줍니다. 구성 요소에 포커스를 유지 하기 위해 예제 앱의 시각적 표시는 의도적으로 낮은 키입니다.

1. Visual Studio에서 새 JavaScript 프로젝트를 만듭니다. 메뉴 모음에서 **파일, 새로 만들기, 프로젝트**를 선택 합니다. **새 프로젝트** 대화 상자의 **설치 된 템플릿** 섹션에서 **JavaScript**를 선택한 다음 **Windows**, **유니버설**을 차례로 선택 합니다. Windows를 사용할 수 없는 경우 Windows 8 이상을 사용 하 고 있는지 확인 합니다. **빈 응용 프로그램** 템플릿을 선택 하 고 프로젝트 이름으로 sampleapp.exe을 입력 합니다.
2.  구성 요소 프로젝트 만들기: 솔루션 탐색기에서 Sampleapp.exe 솔루션에 대 한 바로 가기 메뉴를 열고 **추가**를 선택한 다음 **새 프로젝트** 를 선택 하 여 새 c # 또는 Visual Basic 프로젝트를 솔루션에 추가 합니다. **새 프로젝트 추가** 대화 상자의 **설치 된 템플릿** 섹션에서 **Visual Basic** 또는 **Visual c #** 을 선택 하 고 **Windows**, **유니버설**을 차례로 선택 합니다. **Windows 런타임 구성 요소** 템플릿을 선택 하 고 프로젝트 이름으로 **SampleComponent** 을 입력 합니다.
3.  클래스의 이름을 **Example**로 변경 합니다. 기본적으로 클래스는 **public sealed** 로 표시 되어 있습니다 (Visual Basic의**public NotInheritable** ). 구성 요소에서 노출 하는 모든 Windows 런타임 클래스는 sealed 여야 합니다.
4.  클래스에 두 개의 간단한 멤버를 추가 하 고 **정적** 메서드 (Visual Basic의**공유** 메서드)와 인스턴스 속성을 추가 합니다.

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

5.  선택 사항: 새로 추가 된 멤버에 대해 IntelliSense를 사용 하도록 설정 하려면 솔루션 탐색기에서 SampleComponent 프로젝트에 대 한 바로 가기 메뉴를 열고 **빌드**를 선택 합니다.
6.  솔루션 탐색기의 JavaScript 프로젝트 **에서 참조에 대 한**바로 가기 메뉴를 열고 **참조 추가** 를 선택 하 여 **참조 관리자**를 엽니다. **프로젝트**를 선택 하 고 **솔루션**을 선택 합니다. SampleComponent 프로젝트에 대 한 확인란을 선택 하 고 **확인** 을 선택 하 여 참조를 추가 합니다.

## <a name="call-the-component-from-javascript"></a>JavaScript에서 구성 요소 호출

JavaScript에서 Windows 런타임 형식을 사용 하려면 Visual Studio 템플릿에서 제공 하는 default.js 파일 (프로젝트의 js 폴더)의 무명 함수에 다음 코드를 추가 합니다. 응용 프로그램을 호출 하기 전에 응용 프로그램. oncheckpoint 이벤트 처리기를 실행 해야 합니다.

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

각 멤버 이름의 첫 문자가 대문자에서 소문자로 변경되었습니다. 이 변환은 Windows 런타임를 자연스럽 게 사용할 수 있도록 JavaScript에서 제공 하는 지원의 일부입니다. 네임스페이스 및 클래스 이름은 파스칼식 대/소문자를 사용합니다. 멤버 이름은 모두 소문자인 이벤트 이름을 제외하고 카멜식 대/소문자를 사용합니다. [JavaScript에서 Windows 런타임 사용](/scripting/jswinrt/using-the-windows-runtime-in-javascript)을 참조 하세요. 카멜식 대/소문자 규칙은 혼동될 수 있습니다. 일련의 첫 대문자들은 일반적으로 소문자로 나타나지만 대문자 3개 뒤에 소문자 하나가 오는 경우 처음 두 문자만 소문자로 나타납니다. 예를 들어 IDStringKind라는 멤버는 idStringKind로 나타납니다. Visual Studio에서는 Windows 런타임 구성 요소 프로젝트를 빌드한 다음 JavaScript 프로젝트에서 IntelliSense를 사용 하 여 올바른 대/소문자를 볼 수 있습니다.

이와 비슷한 방식으로 .NET은 관리 코드에서 Windows 런타임를 자연스럽 게 사용할 수 있도록 지원 합니다. 이에 대해서는이 문서의 후속 섹션과 c #을 사용 하는 [구성 요소 및 Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) 및 [UWP 앱 및 Windows 런타임에 대 한 .net 지원](/dotnet/standard/cross-platform/support-for-windows-store-apps-and-windows-runtime)Windows 런타임 문서에 설명 되어 있습니다.

## <a name="create-a-simple-user-interface"></a>간단한 사용자 인터페이스 만들기

JavaScript 프로젝트에서 default.html 파일을 열고 다음 코드와 같이 body를 업데이트합니다. 이 코드는 예제 앱의 전체 컨트롤 집합을 포함하며 클릭 이벤트에 대한 함수 이름을 지정합니다.

> **참고**    앱을 처음 실행할 때 Basics1 및 Basics2 단추만 단추만 지원 됩니다.

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

JavaScript 프로젝트의 css 폴더에서 기본 .css를 엽니다. 표시 된 대로 body 섹션을 수정 하 고 단추 레이아웃과 출력 텍스트의 배치를 제어 하는 스타일을 추가 합니다.

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

이제 default.js에서 onactivated 된 app.config의 processAll 호출에 then 절을 추가 하 여 이벤트 수신기 등록 코드를 추가 합니다. SetPromise을 호출 하는 기존 코드 줄을 바꾸고 다음 코드로 변경 합니다.

```javascript
args.setPromise(WinJS.UI.processAll().then(function () {
    var button1 = document.getElementById("button1");
    button1.addEventListener("click", basics1, false);
    var button2 = document.getElementById("button2");
    button2.addEventListener("click", basics2, false);
}));
```

이 방법은 클릭 이벤트 처리기를 HTML로 직접 추가 하는 것 보다 HTML 컨트롤에 이벤트를 추가 하는 것이 더 좋은 방법입니다. ["Hello, 세계" 앱 만들기 (JS)를](../get-started/create-a-hello-world-app-js-uwp.md)참조 하세요.

## <a name="build-and-run-the-app"></a>앱 빌드 및 실행

빌드하기 전에 컴퓨터에 적절 하 게 모든 프로젝트의 대상 플랫폼을 ARM, x64 또는 x 86으로 변경 합니다.

솔루션을 빌드하고 실행하려면 F5 키를 선택합니다. SampleComponent가 정의되지 않았다는 런타임 오류 메시지가 나타나는 경우 클래스 라이브러리 프로젝트에 대한 참조가 없는 것입니다.

Visual Studio는 먼저 클래스 라이브러리를 컴파일한 다음 [Winmdexp.exe (Windows 런타임 메타 데이터 내보내기 도구)](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool) 를 실행 하는 MSBuild 작업을 실행 하 여 Windows 런타임 구성 요소를 만듭니다. 구성 요소는 관리 코드뿐만 아니라 코드를 설명하는 Windows 메타데이터를 포함하는 .winmd 파일에 포함되어 있습니다. WinMdExp.exe Windows 런타임 구성 요소에서 유효 하지 않은 코드를 작성할 때 빌드 오류 메시지를 생성 하 고 오류 메시지는 Visual Studio IDE에 표시 됩니다. Visual Studio는 UWP 응용 프로그램에 대 한 앱 패키지 (.appx 파일)에 구성 요소를 추가 하 고 적절 한 매니페스트를 생성 합니다.

기본 사항 1 단추를 선택 하 여 정적 GetAnswer 메서드의 반환 값을 출력 영역에 할당 하 고, 예제 클래스의 인스턴스를 만들고, 해당 SampleProperty 속성의 값을 출력 영역에 표시 합니다. 아래와 같은 출력이 표시됩니다.

``` syntax
"The answer is 42."
0
```

기본 사항 2 단추를 선택 하 여 SampleProperty 속성의 값을 늘리고 출력 영역에 새 값을 표시 합니다. 문자열 및 숫자와 같은 기본 형식을 매개 변수 형식 및 반환 형식으로 사용할 수 있으며 관리 코드와 JavaScript 간에 전달할 수 있습니다. JavaScript의 숫자는 배정밀도 부동 소수점 형식으로 저장되기 때문에 .NET Framework 숫자 형식으로 변환됩니다.

> **참고**    기본적으로 JavaScript 코드에만 중단점을 설정할 수 있습니다. Visual Basic 또는 c # 코드를 디버깅 하려면 c #에서 Windows 런타임 구성 요소 만들기 및 Visual Basic를 참조 하세요.

디버깅을 중지하고 앱을 닫으려면 앱에서 Visual Studio로 전환하고 Shift+F5를 선택합니다.

## <a name="using-the-windows-runtime-from-javascript-and-managed-code"></a>JavaScript 및 관리 코드에서 Windows Runtime 사용

Windows 런타임는 JavaScript 또는 관리 코드에서 호출할 수 있습니다. Windows 런타임 개체는 둘 사이를 앞뒤로 전달할 수 있으며, 이벤트는 어느 쪽에서 나 처리할 수 있습니다. 그러나 JavaScript와 .NET은 Windows 런타임를 다르게 지원 하기 때문에 두 환경에서 Windows 런타임 형식을 사용 하는 방법은 약간 다릅니다. 다음 예제에서는 [PropertySet](/uwp/api/windows.foundation.collections.propertyset) 클래스를 사용 하 여 이러한 차이점을 보여 줍니다. 이 예제에서는 관리 코드에서 PropertySet 컬렉션의 인스턴스를 만들고 이벤트 처리기를 등록 하 여 컬렉션의 변경 내용을 추적 합니다. 그런 다음 컬렉션을 가져오는 JavaScript 코드를 추가하고 고유한 이벤트 처리기를 등록한 후 컬렉션을 사용합니다. 마지막으로 관리 코드에서 컬렉션을 변경하고 관리되는 예외를 처리하는 JavaScript를 보여 주는 메서드를 추가합니다.

> **중요**    이 예제에서는 UI 스레드에서 이벤트가 발생 합니다. 예를 들어 비동기 호출의 백그라운드 스레드에서 이벤트를 발생시키는 경우 JavaScript가 이벤트를 처리할 수 있도록 몇 가지 추가 작업을 수행해야 합니다. 자세한 내용은 [Windows 런타임 구성 요소에서 이벤트 발생](raising-events-in-windows-runtime-components.md)을 참조 하세요.

SampleComponent 프로젝트에서 PropertySetStats 라는 새 **public sealed** 클래스 (Visual Basic의**public NotInheritable** 클래스)를 추가 합니다. 클래스는 PropertySet 컬렉션을 래핑하고 MapChanged 이벤트를 처리합니다. 이벤트 처리기는 발생 하는 각 종류의 변경 내용 수를 추적 하 고, DisplayStats 메서드는 HTML로 서식이 지정 된 보고서를 생성 합니다. 추가 **using** 문 (Visual Basic에서**Imports** 문)을 확인 합니다. 이를 덮어쓰는 것이 아니라 기존 **using** 문에 추가 해야 합니다.

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

이벤트 전송자 (이 경우 PropertySet 개체)가 IObservableMap &lt; string, 개체 &gt; 인터페이스 (Windows 런타임 [ &lt; &gt; ](/uwp/api/Windows.Foundation.Collections.IObservableMap_K_V_)Visual Basic의 IObservableMap (of string, object))로 캐스팅 되는 경우를 제외 하 고, 이벤트 처리기는 익숙한 .NET Framework 이벤트 패턴을 따릅니다. 필요한 경우 발신자를 해당 형식으로 캐스팅할 수 있습니다. 또한 이벤트 인수는 개체가 아닌 인터페이스로 제공 됩니다.

default.js 파일에서 표시 된 대로 Runtime1 함수를 추가 합니다. 이 코드는 PropertySetStats 개체를 만들고 해당 PropertySet 컬렉션을 가져온 후 MapChanged 이벤트를 처리 하는 자체 이벤트 처리기 인 onMapChanged 함수를 추가 합니다. 컬렉션을 변경한 후 runtime1는 DisplayStats 메서드를 호출 하 여 변경 형식에 대 한 요약을 표시 합니다.

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

JavaScript에서 Windows 런타임 이벤트를 처리 하는 방법은 .NET Framework 코드에서 처리 하는 방식과 매우 다릅니다. JavaScript 이벤트 처리기는 인수를 하나만 사용합니다. Visual Studio 디버거에서 이 개체를 볼 때 첫 번째 속성은 전송자입니다. 이벤트 인수 인터페이스의 멤버도 이 개체에 직접 나타납니다.

앱을 실행하려면 F5 키를 선택합니다. 클래스가 봉인되지 않은 경우 "'SampleComponent.Example' unsealed 형식은 현재 내보낼 수 없습니다. 이 형식을 sealed로 표시하세요."라는 오류 메시지가 표시됩니다.

**런타임 1** 단추를 선택 합니다. 이벤트 처리기는 요소가 추가 되거나 변경 될 때 변경 내용을 표시 하 고, 끝에 DisplayStats 메서드가 호출 되어 개수에 대 한 요약을 생성 합니다. 디버깅을 중지하고 앱을 닫으려면 Visual Studio로 다시 전환하고 Shift+F5를 선택합니다.

관리 코드에서 PropertySet 컬렉션에 두 항목을 더 추가 하려면 다음 코드를 PropertySetStats 클래스에 추가 합니다.

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

이 코드에서는 두 환경에서 Windows 런타임 형식을 사용 하는 방법의 다른 차이점을 보여 줍니다. 이 코드를 직접 입력 하는 경우 JavaScript 코드에서 사용한 insert 메서드가 IntelliSense에 표시 되지 않는다는 것을 알 수 있습니다. 대신 .NET의 컬렉션에 일반적으로 표시 되는 Add 메서드를 표시 합니다. 이는 일반적으로 사용 되는 일부 컬렉션 인터페이스의 이름이 다르지만 Windows 런타임 및 .NET에서 유사한 기능을 사용 하기 때문입니다. 이러한 인터페이스가 관리 코드에서 사용되는 경우에는 해당하는 .NET Framework 항목으로 나타납니다. 이에 대해서는 [c # 및 Visual Basic를 사용 하는 Windows 런타임 구성 요소](creating-windows-runtime-components-in-csharp-and-visual-basic.md)에서 설명 합니다. JavaScript에서 동일한 인터페이스를 사용 하는 경우 멤버 이름 시작 부분에 있는 대문자는 소문자가 되기 때문에 Windows 런타임만 변경 됩니다.

마지막으로 예외 처리를 사용 하 여 AddMore 메서드를 호출 하려면 default.js runtime2 함수를 추가 합니다.

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

이전에 수행한 것과 동일한 방식으로 이벤트 처리기 등록 코드를 추가 합니다.

```javascript
var runtimeButton1 = document.getElementById("runtimeButton1");
runtimeButton1.addEventListener("click", runtime1, false);
var runtimeButton2 = document.getElementById("runtimeButton2");
runtimeButton2.addEventListener("click", runtime2, false);
```

앱을 실행하려면 F5 키를 선택합니다. **런타임 1** 을 선택 하 고 **런타임 2**를 선택 합니다. JavaScript 이벤트 처리기는 컬렉션에 대한 첫 번째 변경 사항을 보고합니다. 그러나 두 번째 변경 사항에는 중복 키가 있습니다. .NET Framework 사전의 사용자는 Add 메서드가 예외를 throw 할 것으로 예상 하며이는 발생 합니다. JavaScript는 .NET 예외를 처리 합니다.

> **참고**    JavaScript 코드에서는 예외의 메시지를 표시할 수 없습니다. 메시지 텍스트는 스택 추적으로 대체됩니다. 자세한 내용은 c # 및 Visual Basic에서 Windows 런타임 구성 요소 만들기에서 "예외 Throw"를 참조 하세요.

이와 대조적으로 JavaScript에서 중복 키가 있는 insert 메서드를 호출한 경우 항목의 값이 변경 되었습니다. 이러한 동작의 차이는 [c # 및 Visual Basic를 사용 하 Windows 런타임 구성 요소](creating-windows-runtime-components-in-csharp-and-visual-basic.md)에 설명 된 대로 JavaScript 및 .net에서 Windows 런타임를 지 원하는 다양 한 방법으로 인해 발생 합니다.

## <a name="returning-managed-types-from-your-component"></a>구성 요소에서 관리되는 형식 반환

앞에서 설명한 대로 JavaScript 코드와 c # 또는 Visual Basic 코드 간에 네이티브 Windows 런타임 형식을 앞뒤로 전달할 수 있습니다. 대부분 형식 이름과 멤버 이름은 두 경우에 동일합니다(멤버 이름이 JavaScript에서 소문자로 시작하는 경우 제외). 그러나 이전 단원에서 PropertySet 클래스는 관리 코드에서 다른 멤버를 가진 것으로 나타났습니다. 예를 들어 JavaScript에서 삽입 메서드를 호출 하 고 .NET 코드에서 Add 메서드를 호출 했습니다. 이 섹션에서는 이러한 차이점이 JavaScript에 전달 되는 .NET Framework 형식에 어떤 영향을 주는지 살펴봅니다.

구성 요소에서 만들었거나 JavaScript에서 구성 요소에 전달 된 Windows 런타임 형식을 반환 하는 것 외에도 관리 코드에서 만들어진 관리 되는 형식을 해당 하는 Windows 런타임 형식인 것 처럼 JavaScript에 반환할 수 있습니다. 런타임 클래스의 첫 번째 간단한 예제에서도 멤버의 매개 변수와 반환 형식은 .NET Framework 형식인 Visual Basic 또는 C# 기본 형식입니다. 컬렉션에 대해이를 보여 주려면 다음 코드를 예제 클래스에 추가 하 여 정수로 인덱싱된 문자열의 제네릭 사전을 반환 하는 메서드를 만듭니다.

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

사전은 dictionary [ &lt; TKey, TValue &gt; ](/dotnet/api/system.collections.generic.dictionary-2)에 의해 구현 되 고 Windows 런타임 인터페이스에 매핑되는 인터페이스로 반환 되어야 합니다. 이 경우 인터페이스는 IDictionary &lt; int, 문자열 &gt; (Visual Basic의 Idictionary (정수, 문자열))입니다. Windows 런타임 IMap int를 입력 하는 경우 &lt; 문자열 &gt; 은 관리 코드에 전달 되 고, 문자열은 IDictionary &lt; int, string으로 표시 되 &gt; 고, 관리 되는 형식이 JavaScript에 전달 되는 경우에는 역방향이 true가 됩니다.

**중요**    관리 되는 형식이 여러 인터페이스를 구현 하는 경우 JavaScript는 목록에서 첫 번째로 표시 되는 인터페이스를 사용 합니다. 예를 들어, 문자열을 JavaScript 코드로 반환 하는 경우 &lt; 문자열을 &gt; &lt; &gt; 반환 형식으로 지정 하는 인터페이스에 관계 없이 IDictionary int로 표시 됩니다. 즉, 첫 번째 인터페이스가 나머지 인터페이스에 나타나는 멤버를 포함하고 있지 않은 경우 해당 멤버는 JavaScript에 표시되지 않습니다.

 

새 메서드를 테스트 하 고 사전을 사용 하려면 returns1 및 returns2 함수를 default.js에 추가 합니다.

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

다른 이벤트 등록 코드와 동일한 다음 블록에 이벤트 등록 코드를 추가 합니다.

```javascript
var returnsButton1 = document.getElementById("returnsButton1");
returnsButton1.addEventListener("click", returns1, false);
var returnsButton2 = document.getElementById("returnsButton2");
returnsButton2.addEventListener("click", returns2, false);
```

이 JavaScript 코드에서 관찰할 몇 가지 흥미로운 사실이 있습니다. 먼저,이 파일에는 사전의 내용을 HTML로 표시 하는 showMap 함수가 포함 되어 있습니다. ShowMap에 대 한 코드에서 반복 패턴을 확인 합니다. .NET에는 일반 IDictionary 인터페이스에 대 한 첫 번째 메서드가 없고 크기는 크기 메서드가 아니라 Count 속성에 의해 반환 됩니다. JavaScript로, IDictionary &lt; int, string &gt; 은 IMap int, string Windows 런타임 형식으로 표시 됩니다 &lt; &gt; . ( [IMap &lt; K, V &gt; ](/uwp/api/Windows.Foundation.Collections.IMap_K_V_) 인터페이스를 참조 하세요.)

Returns2 함수에서 JavaScript는 앞의 예제와 같이 Insert 메서드 (JavaScript에서 삽입)를 호출 하 여 사전에 항목을 추가 합니다.

앱을 실행하려면 F5 키를 선택합니다. 사전의 초기 내용을 만들고 표시 하려면 **1 반환** 단추를 선택 합니다. 사전에 두 항목을 더 추가 하려면 **2 반환** 단추를 선택 합니다. 사전 TKey, TValue에서 짐작할 수 있듯이 항목은 삽입 순서 대로 표시 됩니다 &lt; &gt; . 이러한 항목을 정렬 하려면 &lt; GetMapOfNames에서 system.collections.generic.sorteddictionary int, string을 반환 하면 &gt; 됩니다. (이전 예제에서 사용 된 PropertySet 클래스에는 사전 &lt; 에서 다른 내부 조직이 있습니다. TKey, TValue &gt; .)

물론 JavaScript는 강력한 형식의 언어가 아니므로 강력한 형식의 제네릭 컬렉션을 사용하면 놀라운 결과가 발생할 수 있습니다. **반환 2** 단추를 다시 선택 합니다. JavaScript에서는는 "7"을 숫자 7로, ct에 저장 된 숫자 7을 문자열로 강제 변환 합니다. 또한 문자열 "forty"를 0으로 강제 변환합니다. 그러나 이는 시작에 불과합니다. 두 번 **반환** 단추를 몇 번 더 선택 합니다. 관리 코드에서 Add 메서드는 값이 올바른 형식으로 캐스팅 된 경우에도 중복 키 예외를 생성 합니다. 이와 대조적으로 Insert 메서드는 기존 키와 연결 된 값을 업데이트 하 고 새 키가 사전에 추가 되었는지 여부를 나타내는 부울 값을 반환 합니다. 이 때문에 키 7과 연결된 값이 계속 변경됩니다.

또 다른 예기치 않은 동작은 할당되지 않은 JavaScript 변수를 문자열 인수로 전달하는 경우 "undefined"라는 문자열을 얻게 된다는 것입니다. 즉, .NET Framework 컬렉션 형식을 JavaScript 코드에 전달할 때는 주의해야 합니다.

> **참고**    연결할 많은 양의 텍스트가 있는 경우 showMap 함수와 같이 코드를 .NET Framework 메서드로 이동 하 고 StringBuilder 클래스를 사용 하 여 보다 효율적으로이 작업을 수행할 수 있습니다.

Windows 런타임 구성 요소에서 사용자 고유의 제네릭 형식을 노출할 수 없지만 다음과 같은 코드를 사용 하 여 Windows 런타임 클래스에 대해 .NET Framework 제네릭 컬렉션을 반환할 수 있습니다.

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

List &lt; t는 &gt; &lt; &gt; JavaScript에서 Windows 런타임 형식 IVector t로 표시 되는 IList t를 구현 &lt; &gt; 합니다.

## <a name="declaring-events"></a>이벤트 선언


표준 .NET Framework 이벤트 패턴 또는 Windows 런타임에서 사용 되는 다른 패턴을 사용 하 여 이벤트를 선언할 수 있습니다. .NET Framework는 System.object &lt; &gt; 및 Windows 런타임 eventhandler &lt; T 대리자 간의 동등성을 지원 &gt; 하므로 eventhandler teventargs를 사용 하는 것 &lt; &gt; 이 표준 .NET Framework 패턴을 구현 하는 좋은 방법입니다. 이에 대해 살펴보려면 다음 클래스 쌍을 SampleComponent 프로젝트에 추가합니다.

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

Windows 런타임에서 이벤트를 노출할 때 이벤트 인수 클래스는 System.object에서 상속 됩니다. EventArgs는 Windows 런타임 형식이 아니기 때문에 .NET의 경우와 마찬가지로 System.object에서 상속 되지 않습니다.

이벤트에 대 한 사용자 지정 이벤트 접근자를 선언 하는 경우 (Visual Basic의**사용자 지정** 키워드) Windows 런타임 이벤트 패턴을 사용 해야 합니다. [Windows 런타임 구성 요소의 사용자 지정 이벤트 및 이벤트 접근자](custom-events-and-event-accessors-in-windows-runtime-components.md)를 참조 하세요.

테스트 이벤트를 처리 하려면 default.js events1 함수를 추가 합니다. Events1 함수는 테스트 이벤트에 대 한 이벤트 처리기 함수를 만들고 즉시 OnTest 메서드를 호출 하 여 이벤트를 발생 시킵니다. 이벤트 처리기의 본문에 중단점을 추가 하는 경우 단일 매개 변수에 전달 된 개체에 소스 개체와 TestEventArgs의 두 멤버가 모두 포함 되어 있음을 알 수 있습니다.

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

다른 이벤트 등록 코드와 동일한 다음 블록에 이벤트 등록 코드를 추가 합니다.

```javascript
var events1Button = document.getElementById("events1Button");
events1Button.addEventListener("click", events1, false);
```

## <a name="exposing-asynchronous-operations"></a>비동기 작업 노출


.NET Framework에는 작업 및 일반 [작업 &lt; TResult &gt; ](/dotnet/api/system.threading.tasks.task-1) 클래스를 기반으로 하는 비동기 처리 및 병렬 처리를 위한 다양 한 도구 집합이 있습니다. Windows 런타임 구성 요소에서 작업 기반 비동기 처리를 노출 하려면 Windows 런타임 인터페이스 [iasyncaction](/windows/desktop/api/windows.foundation/nn-windows-foundation-iasyncaction), [iasyncactionwithprogress &lt; tprogress &gt; ](/previous-versions/br205784(v=vs.85)), [iasyncoperation<tresult> &lt; &gt; tresult](/previous-versions/br205802(v=vs.85))및 [IAsyncOperationWithProgress &lt; tresult, tprogress &gt; ](/previous-versions/br205807(v=vs.85))를 사용 합니다. Windows 런타임에서 작업은 결과를 반환 하지만 작업은 결과를 반환 하지 않습니다.

이 단원에서는 진행률을 보고하고 결과를 반환하는 취소 가능한 비동기 작업을 보여 줍니다. GetPrimesInRangeAsync 메서드는 [system.runtime.interopservices.windowsruntime.asyncinfo](/dotnet/api/system.runtime.interopservices.windowsruntime) 클래스를 사용 하 여 작업을 생성 하 고 취소 및 진행률 보고 기능을 WinJS 개체에 연결 합니다. 먼저 예제 클래스에 GetPrimesInRangeAsync 메서드를 추가 합니다.

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

GetPrimesInRangeAsync는 매우 간단한 소수 찾기 이며 의도적입니다. 여기에서는 비동기 작업을 구현하는 데 초점을 맞추고 있기 때문에 간결성이 중요하며, 속도가 느린 구현 성능은 취소 기능을 설명할 때 유용합니다. GetPrimesInRangeAsync는 무차별 암호 대입으로 prime를 찾습니다. 소수를 사용 하는 대신 제곱근 보다 작거나 같은 모든 정수로 후보를 나눕니다. 이 코드를 단계별로 실행하면 다음과 같습니다.

-   비동기 작업을 시작하기 전에 매개 변수의 유효성을 검사하고 잘못된 입력에 대한 예외를 throw하는 등의 준비 작업을 수행합니다.
-   이 구현의 핵심은 [system.runtime.interopservices.windowsruntime.asyncinfo &lt; TResult, tprogress &gt; (Func &lt; CancellationToken, iprogress &lt; tprogress &gt; , Task &lt; TResult &gt; ](/dotnet/api/system.runtime.interopservices.windowsruntime) &gt; ) 메서드 및 메서드의 유일한 매개 변수인 대리자입니다. 대리자는 취소 토큰과 진행률을 보고하기 위한 인터페이스를 받아들여야 하고 이러한 매개 변수를 사용하는 시작된 작업을 반환해야 합니다. JavaScript에서 GetPrimesInRangeAsync 메서드를 호출 하면 다음 단계가 발생 합니다 (여기에 제공 된 순서 대로 진행 되지 않을 수 있습니다).

    -   [WinJS](/previous-versions/windows/apps/br211867(v=win.10)) 개체는 반환 된 결과를 처리 하 고 취소에 대응 하며 진행률 보고서를 처리 하는 함수를 제공 합니다.
    -   System.runtime.interopservices.windowsruntime.asyncinfo 메서드는 취소 소스와 IProgress T 인터페이스를 구현 하는 개체를 만듭니다 &lt; &gt; . 대리자는 취소 소스의 [CancellationToken](/dotnet/api/system.threading.cancellationtoken) 토큰과 [iprogress &lt; &gt; T](/dotnet/api/system.iprogress-1) 인터페이스를 전달 합니다.

        > **참고**    약속 개체가 취소에 대응 하는 함수를 제공 하지 않는 경우 System.runtime.interopservices.windowsruntime.asyncinfo는 취소 가능한 토큰을 계속 전달 하 고 취소가 발생할 수 있습니다. 약속 개체가 진행률 업데이트를 처리 하는 함수를 제공 하지 않는 경우 System.runtime.interopservices.windowsruntime.asyncinfo는 여전히 IProgress t를 구현 하는 개체를 제공 &lt; &gt; 하지만 해당 보고서는 무시 됩니다.

    -   대리자는 Task를 사용 [합니다. &lt; TResult &gt; (Func &lt; tresult &gt; , CancellationToken](/dotnet/api/system.threading.tasks.task.run#System_Threading_Tasks_Task_Run__1_System_Func___0__System_Threading_CancellationToken_)) 메서드를 실행 하 여 토큰과 진행률 인터페이스를 사용 하는 시작 된 작업을 만듭니다. 시작된 작업의 대리자는 원하는 결과를 계산하는 람다 함수가 제공합니다. 잠시 후에 자세히 설명하겠습니다.
    -   System.runtime.interopservices.windowsruntime.asyncinfo 메서드는 [IAsyncOperationWithProgress &lt; TResult, tprogress &gt; ](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) 인터페이스를 구현 하는 개체를 만들고, 토큰 소스를 사용 하 여 Windows 런타임 취소 메커니즘을 연결 하 고, 약속 개체의 진행률 보고 함수를 iprogress T 인터페이스와 연결 합니다 &lt; &gt; .
    -   IAsyncOperationWithProgress &lt; TResult, TProgress &gt; 인터페이스가 JavaScript로 반환 됩니다.

-   시작된 작업이 나타내는 람다 함수는 인수를 사용하지 않습니다. 람다 함수 이기 때문에 토큰 및 IProgress 인터페이스에 액세스할 수 있습니다. 후보 숫자가 계산될 때마다 람다 함수는 다음을 수행합니다.

    -   진행률의 다음 백분율 지점에 도달했는지 확인합니다. 있는 경우 람다 함수는 IProgress T를 호출 합니다 &lt; &gt; . 보고서 메서드 및 백분율은 약속 개체가 진행률을 보고 하기 위해 지정한 함수에 전달 됩니다.
    -   작업이 취소된 경우 취소 토큰을 사용하여 예외를 throw합니다. IAsyncOperationWithProgress TResult, TProgress 인터페이스가 상속 된 [IAsyncInfo](/uwp/api/windows.foundation.iasyncinfo.cancel) 메서드를 호출 하는 경우 &lt; &gt; system.runtime.interopservices.windowsruntime.asyncinfo 메서드에서 설정 된 연결을 통해 취소 토큰에 알림이 표시 됩니다.
-   람다 함수가 소수의 목록을 반환할 때 목록은 WinJS.Promise 개체가 결과를 처리하기 위해 지정한 함수에 전달됩니다.

JavaScript 약속을 만들고 취소 메커니즘을 설정 하려면 default.js 하는 asyncRun 및 Asyncrun 함수를 추가 합니다.

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

이전에 했던 것과 동일한 방법으로 이벤트 등록 코드를 잊지 마세요.

```javascript
var btnAsync = document.getElementById("btnAsync");
btnAsync.addEventListener("click", asyncRun, false);
var btnCancel = document.getElementById("btnCancel");
btnCancel.addEventListener("click", asyncCancel, false);
```

비동기 GetPrimesInRangeAsync 메서드를 호출하면 asyncRun 함수가 WinJS.Promise 개체를 만듭니다. 개체의  then 메서드는 반환된 결과를 처리하는 세 함수를 사용하고 오류(취소 포함)에 대응하며 진행률 보고서를 처리합니다. 이 예제에서는 반환된 결과가 출력 영역에 인쇄됩니다. 취소하거나 완료하면 작업을 시작하고 취소하는 단추가 다시 설정됩니다. 진행률 보고는 진행률 컨트롤을 업데이트합니다.

AsyncCancel 함수는 WinJS 개체의 cancel 메서드만 호출 합니다.

앱을 실행하려면 F5 키를 선택합니다. 비동기 작업을 시작 하려면 **Async** 단추를 선택 합니다. 다음에 발생하는 상황은 컴퓨터 속도에 따라 다릅니다. Zips 때까지 진행률 표시줄이 완료로 표시 되 면 하나 이상의 요인이 GetPrimesInRangeAsync에 전달 되는 시작 숫자의 크기를 늘립니다. 테스트할 숫자의 개수를 늘리거나 줄여서 작업의 기간을 조정할 수 있지만 시작 숫자의 가운데에 0을 여러 개 추가하면 더 큰 영향을 미치게 됩니다. 작업을 취소 하려면 **비동기 취소** 단추를 선택 합니다.

## <a name="related-topics"></a>관련 항목

* [UWP 앱 용 .NET 개요](/previous-versions/windows/apps/br230302(v=vs.140))
* [UWP 앱용 .NET](/dotnet/api/index?view=dotnet-uwp-10.0)