---
Description: UI용 문자열 리소스를 리소스 파일에 넣습니다. 그러면 코드 또는 태그로부터 해당 문자열을 참조할 수 있습니다.
title: 리소스에 UI 문자열 배치
ms.assetid: E420B9BB-C0F6-4EC0-BA3A-BA2875B69722
label: 리소스에 UI 문자열 배치
template: detail.hbs
---

# 리소스에 UI 문자열 배치


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**ApplicationModel.Resources.ResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br206014)
-   [**WinJS.Resources.processAll**](https://msdn.microsoft.com/library/windows/apps/br211864)

UI용 문자열 리소스를 리소스 파일에 넣습니다. 그러면 코드 또는 태그로부터 해당 문자열을 참조할 수 있습니다.

이 항목에서는 유니버설 Windows 앱에 여러 언어 문자열 리소스를 추가하는 단계와 이를 간단히 테스트하는 방법을 소개합니다.

## <span id="put_strings_into_resource_files__instead_of_putting_them_directly_in_code_or_markup."> </span> <span id="PUT_STRINGS_INTO_RESOURCE_FILES__INSTEAD_OF_PUTTING_THEM_DIRECTLY_IN_CODE_OR_MARKUP."> </span>문자열을 코드나 태그에 바로 넣는 대신 리소스 파일에 넣습니다.


1.  Visual Studio에서 솔루션을 열거나 새 솔루션을 만듭니다.

2.  Visual Studio에서 package.appxmanifest를 열고 **응용 프로그램** 탭으로 이동한 후 기본 언어를 "en-US"(이 예제의 경우)로 설정합니다. 솔루션에 package.appxmanifest 파일이 여러 개 있는 경우에는 파일마다 이 단계를 수행합니다.
    <br>**참고** 이렇게 하면 프로젝트의 기본 언어가 지정됩니다. 기본 언어 리소스는 사용자의 기본 설정 언어 또는 표시 언어가 응용 프로그램에 제공된 언어 리소스와 일치하지 않을 경우 사용됩니다.
3.  리소스 파일을 넣을 폴더를 만듭니다.
    1.  솔루션 탐색기에서 프로젝트(솔루션에 프로젝트가 여러 개인 경우에는 공유 프로젝트)를 마우스 오른쪽 단추로 클릭한 후 **추가** &gt; **새 폴더**를 선택합니다.
    2.  새 폴더의 이름을 "Strings"로 지정합니다.
    3.  새 폴더가 솔루션 탐색기에 보이지 않으면 프로젝트가 여전히 선택된 상태에서 Microsoft Visual Studio 메뉴에서 **프로젝트** &gt; **모든 파일 표시**를 선택합니다.

4.  영어(미국)용 하위 폴더와 리소스 파일을 만듭니다.
    1.  문자열 폴더를 마우스 오른쪽 단추로 클릭하고 그 아래에 새 폴더를 추가합니다. 이름을 "en-US"로 지정합니다. 리소스 파일은 [BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302) 언어 태그용으로 이름이 지정된 폴더에 배치됩니다. 언어 한정자 및 일반 언어 태그 목록에 대한 자세한 내용은 [한정자를 사용하여 리소스 이름을 지정하는 방법](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)을 참조하세요.
    2.  en-US 폴더를 마우스 오른쪽 단추로 클릭하고 **추가** &gt; **새 항목...**을 선택합니다.
    3.  **XAML:** "리소스 파일(.resw)"을 선택합니다.
        <br>**HTML:** "리소스 파일(.resjson)"을 선택합니다.

    4.  **추가**를 클릭합니다. 기본 이름의 리소스 파일 "Resources.resw"(**XAML**의 경우) 또는 "resources.rejson"(**HTML**의 경우)이 추가됩니다. 이 기본 파일 이름을 사용하는 것이 좋습니다. 앱에서 리소스를 여러 파일로 분할할 수 있지만 이러한 파일을 참조할 때는 각별히 주의하여 올바르게 참조해야 합니다.[문자열 리소스를 로드하는 방법](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)을 참조하세요.
    5.  **XAML만 해당:** 이전 .NET 프로젝트의 문자열 리소스만 포함된 .resx 파일이 있는 경우 **추가** &gt; **기존 항목...**을 선택하고 .resx 파일을 추가한 후 이름을 .resw로 바꿉니다.
    6.  이 파일을 열고 편집기를 사용하여 이 리소스를 추가합니다.

        **XAML:**

        Strings/en-US/Resources.resw
        ![리소스 추가, 영어](images/addresource-en-us.png)
        이 예제에서 "Greeting.Text" 및 "Farewell"은 표시되어야 하는 문자열을 나타냅니다. "Greeting.Width"는 "Greeting" 문자열의 Width 속성을 나타냅니다. 주석은 문자열을 다른 언어로 지역화하는 번역사에게 특별한 지침을 제공할 수 있는 위치입니다.

        **HTML:**

        새 파일에는 기본 콘텐츠가 포함되어 있습니다. 파일 내용을 다음으로 바꿉니다(기본값과 유사할 수 있음).

        Strings/en-US/resources.resjson

        ```        json
        {
                "greeting"              : "Hello",
                "_greeting.comment"     : "A welcome greeting",

                "farewell"              : "Goodbye",
                "_farewell.comment"     : "A goodbye"
        }
        ```

        마지막 쌍을 제외한 각 이름/값 쌍 뒤에 쉼표를 추가해야 하는 엄격한 JSON(JavaScript Object Notation) 구문입니다. 이 샘플에서 "greeting" 및 "farewell"은 표시되어야 하는 문자열을 나타냅니다. 다른 쌍("\_greeting.comment" 및 "\_farewell.comment")은 문자열을 설명하는 주석입니다. 주석은 문자열을 다른 언어로 지역화하는 번역사에게 특별한 지침을 제공할 수 있는 위치입니다.

## <span id="associate_controls_to_resources."> </span> <span id="ASSOCIATE_CONTROLS_TO_RESOURCES."> </span>컨트롤을 리소스와 연결합니다.


**XAML만 해당:**

지역화된 텍스트가 필요한 모든 컨트롤을 .resw 파일과 연결해야 합니다. 이렇게 하려면 다음과 같이 XAML 요소의 **x:Uid** 특성을 사용합니다.

```XAML
<TextBlock x:Uid="Greeting" Text="" />
```

리소스 이름에는 **Uid** 특성 값을 사용해야 하며, 번역된 문자열을 가져오는 데 사용할 속성을 지정해야 합니다(이 경우 Text 속성). 다른 언어에 대해 Greeting.Width 등의 다른 속성/값을 지정할 수 있지만, 그러한 레이아웃 관련 속성은 주의해서 사용해야 합니다. 장치의 화면을 기반으로 컨트롤이 동적으로 배치될 수 있도록 고려해야 합니다.

첨부된 속성은 resw 파일에서 다르게 처리됩니다(예: AutomationPeer.Name). 네임스페이스를 이와 같이 명확하게 표시해야 합니다.

```XAML
MediumButton.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name</code></pre></td>
```

## <span id="add_string_resource_identifiers_to_code_and_markup."> </span> <span id="ADD_STRING_RESOURCE_IDENTIFIERS_TO_CODE_AND_MARKUP."> </span>코드 및 태그에 문자열 리소스 식별자를 추가합니다.


**XAML:**

코드에서는 문자열을 동적으로 참조할 수 있습니다.

**C#**
```CSharp
var loader = new Windows.ApplicationModel.Resources.ResourceLoader();
var str = loader.GetString("Farewell");
```

**C++**
```ManagedCPlusPlus
auto loader = ref new Windows::ApplicationModel::Resources::ResourceLoader();
auto str = loader->GetString("Farewell");
```

**HTML:**

1.  JavaScript용 Windows 라이브러리에 대한 참조를 HTML 파일에 추가합니다(없는 경우).

    **참고** 다음 코드는 Visual Studio에서 새로운 **빈 앱(유니버설 Windows)** JavaScript 프로젝트를 만들 때 생성되는 Windows 프로젝트의 default.html 파일용 HTML을 보여 줍니다. 파일에는 WinJS에 대한 참조가 이미 포함되어 있습니다.

    ```    HTML
    <!-- WinJS references -->
    <link href="WinJS/css/ui-dark.css" rel="stylesheet" />
    <script src="WinJS/js/base.js"></script>
    <script src="WinJS/js/ui.js"></script>
    ```

2.  HTML 파일과 함께 제공되는 JavaScript 코드에서 HTML이 로드될 때 [**WinJS.Resources.processAll**](https://msdn.microsoft.com/library/windows/apps/br211864) 함수를 호출합니다.

    ```    JavaScript
    WinJS.Application.onloaded = function(){
        WinJS.Resources.processAll();
    }
    ```
    
    추가 HTML이 [**WinJS.UI.Pages.PageControl**](https://msdn.microsoft.com/library/windows/apps/jj126158) 개체에 로드되면, 페이지 컨트롤의 [**IPageControlMembers.ready**](https://msdn.microsoft.com/library/windows/apps/hh770590) 메서드에서 [**WinJS.Resources.processAll**](https://msdn.microsoft.com/library/windows/apps/br211864)(*element*)을 호출합니다. 여기서 *element*는 로드되는 HTML 요소(및 하위 요소)입니다. 이 예제는 [응용 프로그램 리소스 및 지역화 샘플](http://go.microsoft.com/fwlink/p/?linkid=227301)의 시나리오 6을 기반으로 합니다.

    ```    JavaScript
    var output;
    
    var page = WinJS.UI.Pages.define("/html/scenario6.html", {
        ready: function (element, options) {
            output = element.querySelector('#output');
            WinJS.Resources.processAll(output); // Refetch string resources
            WinJS.Resources.addEventListener("contextchanged", refresh, false);
        }
        unload: function () { 
            WinJS.Resources.removeEventListener("contextchanged", refresh, false); 
        } 
    });
    ```

3.  HTML에서 리소스 파일의 리소스 식별자('greeting' 및 'farewell')를 사용하여 문자열 리소스를 참조합니다.
    ```    HTML
    <h2><span data-win-res="{textContent: 'greeting';}"></span></h2>
    <h2><span data-win-res="{textContent: 'farewell'}"></span></h2>
    ```

4.  특성에 대한 문자열 리소스를 참조합니다.

    ```    HTML
    <div data-win-res="{attributes: {'aria-label'; : 'String1'}}" >
    ```

    HTML 대체를 위한 data-win-res 특성의 일반 패턴은 data-win-res="{*propertyname1*: '*resource ID*', *propertyname2*: '*resource ID2*'}"입니다.

    **참고** 문자열에 태그가 포함되어 있지 않으면, 가능한 경우 innerHTML 대신 textContent 속성에 리소스를 바인딩하세요. textContent 속성이 innerHTML보다 훨씬 교체 속도가 빠릅니다.

5.  JavaScript에서 문자열 리소스를 참조합니다.
    <span codelanguage="JavaScript"></span>
    ```    JavaScript
    var el = element.querySelector('#header');
    var res = WinJS.Resources.getString('greeting');
    el.textContent = res.value;
    el.setAttribute('lang', res.lang);
    ```

## <span id="add_folders_and_resource_files_for_two_additional_languages."> </span> <span id="ADD_FOLDERS_AND_RESOURCE_FILES_FOR_TWO_ADDITIONAL_LANGUAGES."> </span>다른 두 언어에 대한 폴더와 리소스 파일을 추가합니다.


1.  독일어용으로 문자열 폴더 아래에 다른 폴더를 추가합니다. 독일어(독일)에 대해 폴더 이름을 "de-DE"로 지정합니다.
2.  de-DE 폴더에 다른 리소스 파일을 만들고 다음을 추가합니다.

    **XAML:**

    strings/de-DE/Resources.resw

    ![리소스 추가, 독일어](images/addresource-de-de.png)

    **HTML:**

    strings/de-DE/resources.resjson

    ```    json
    {
        "greeting"              : "Hallo",
        "_greeting.comment"     : "A welcome greeting.",

        "farewell"              : "Auf Wiedersehen",
        "_farewell.comment"     : "A goodbye."
    }
    ```

3.  프랑스어(프랑스)용으로 "fr-FR"이라는 폴더를 하나 더 만듭니다. 새 리소스 파일을 만들고 다음을 추가합니다.

    **XAML:**

    strings/fr-FR/Resources.resw
    ![리소스 추가, 프랑스어](images/addresource-fr-fr.png)
    **HTML:**

    strings/fr-FR/resources.resjson

    ```    json
    {
        "greeting"              : "Bonjour",
        "_greeting.comment"     : "A welcome greeting.",

        "farewell"              : "Au revoir",
        "_farewell.comment"     : "A goodbye."
    }
    ```

## <span id="build_and_run_the_app."> </span> <span id="BUILD_AND_RUN_THE_APP."> </span>앱 빌드 및 실행


기본 표시 언어를 위한 앱 테스트

1.  F5 키를 눌러 앱을 빌드 및 실행합니다.
2.  greeting 및 farewell이 사용자의 기본 설정 언어로 표시됩니다.
3.  앱을 종료합니다.

다른 언어에 대해 앱을 테스트합니다.

1.  장치에 **설정**을 불러옵니다.
2.  **시간 및 언어**를 선택합니다.
3.  **지역 및 언어**(휴대폰 또는 휴대폰 에뮬레이터에서는 **언어**)를 선택합니다.
4.  앱 실행 시 표시된 언어는 목록에 있는 최상위 언어(영어, 독일어 또는 프랑스어)입니다. 최상위 언어가 이 세 언어 중 하나가 아니면 앱이 지원하는 목록에서 그 다음 언어가 앱에 적용됩니다.
5.  시스템에 이 세 언어가 모두 없을 경우에는 **언어 추가**를 클릭하고 목록에 추가합니다.
6.  다른 언어를 사용하여 앱을 테스트하려면 목록에서 언어를 선택하고 **기본값으로 설정**을 클릭합니다(휴대폰 또는 휴대폰 에뮬레이터에서는 목록에서 언어를 길게 누르고 **위로 이동**을 맨 위에 올 때까지 누름). 그런 다음 앱을 실행합니다.

## <span id="related_topics"> </span>관련 항목


* [한정자를 사용하여 리소스 이름을 지정하는 방법](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)
* [문자열 리소스를 로드하는 방법](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)
* [BCP-47 언어 태그](http://go.microsoft.com/fwlink/p/?linkid=227302)
 

 





<!--HONumber=Mar16_HO1-->


