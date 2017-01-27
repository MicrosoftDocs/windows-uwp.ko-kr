---
author: DelfCo
Description: "UI용 문자열 리소스를 리소스 파일에 넣습니다. 그러면 코드 또는 태그로부터 해당 문자열을 참조할 수 있습니다."
title: "리소스에 UI 문자열 배치"
ms.assetid: E420B9BB-C0F6-4EC0-BA3A-BA2875B69722
label: Put UI strings into resources
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: dff1e6df480a1ffc0e2441c78b942ccd0ee2126c

---

# <a name="put-ui-strings-into-resources"></a>리소스에 UI 문자열 배치
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

UI용 문자열 리소스를 리소스 파일에 넣습니다. 그러면 코드 또는 태그로부터 해당 문자열을 참조할 수 있습니다.

<div class="important-apis" >
<b>중요 API</b><br/>
<ul>
<li>[**ApplicationModel.Resources.ResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br206014)</li>
<li>[**WinJS.Resources.processAll**](https://msdn.microsoft.com/library/windows/apps/br211864)</li>
</ul>
</div>


이 항목에서는 유니버설 Windows 앱에 여러 언어 문자열 리소스를 추가하는 단계와 이를 간단히 테스트하는 방법을 소개합니다.

## <a name="put-strings-into-resource-files-instead-of-putting-them-directly-in-code-or-markup"></a>문자열을 코드나 태그에 바로 넣는 대신 리소스 파일에 넣습니다.


1.  Visual Studio에서 솔루션을 열거나 새 솔루션을 만듭니다.

2.  Visual Studio에서 package.appxmanifest를 열고 **응용 프로그램** 탭으로 이동한 후 기본 언어를 "en-US"(이 예제의 경우)로 설정합니다. 솔루션에 package.appxmanifest 파일이 여러 개 있는 경우에는 파일마다 이 단계를 수행합니다.
    <br>**참고**  이렇게 하면 프로젝트의 기본 언어가 지정됩니다. 기본 언어 리소스는 사용자의 기본 설정 언어 또는 표시 언어가 응용 프로그램에 제공된 언어 리소스와 일치하지 않을 경우 사용됩니다.
3.  리소스 파일을 넣을 폴더를 만듭니다.
    1.  솔루션 탐색기에서 프로젝트(솔루션에 프로젝트가 여러 개인 경우에는 공유 프로젝트)를 마우스 오른쪽 단추로 클릭한 후 **추가** &gt; **새 폴더**를 선택합니다.
    2.  새 폴더의 이름을 "Strings"로 지정합니다.
    3.  새 폴더가 솔루션 탐색기에 보이지 않으면 프로젝트가 여전히 선택된 상태에서 Microsoft Visual Studio 메뉴에서 **프로젝트** &gt; **모든 파일 표시**를 선택합니다.

4.  영어(미국)용 하위 폴더와 리소스 파일을 만듭니다.
    1.  문자열 폴더를 마우스 오른쪽 단추로 클릭하고 그 아래에 새 폴더를 추가합니다. 이름을 "en-US"로 지정합니다. 리소스 파일은 [BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302) 언어 태그용으로 이름이 지정된 폴더에 배치됩니다. 언어 한정자 및 일반 언어 태그 목록에 대한 자세한 내용은 [한정자를 사용하여 리소스 이름을 지정하는 방법](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)을 참조하세요.
    2.  en-US 폴더를 마우스 오른쪽 단추로 클릭하고 **추가** &gt; **새 항목…**을 선택합니다.
    3.  "리소스 파일(.resw)"을 선택합니다.

    4.  **추가**를 클릭합니다. 기본 이름의 리소스 파일 "Resources.resw"가 추가됩니다. 이 기본 파일 이름을 사용하는 것이 좋습니다. 앱에서 리소스를 여러 파일로 분할할 수 있지만 이러한 파일을 참조할 때는 각별히 주의하여 올바르게 참조해야 합니다.[문자열 리소스를 로드하는 방법](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)을 참조하세요.
    5.  이전 .NET 프로젝트의 문자열 리소스만 포함된 .resx 파일이 있는 경우 **추가** &gt; **기존 항목…**을 선택하고 .resx 파일을 추가한 후 이름을 .resw로 바꿉니다.
    6.  이 파일을 열고 편집기를 사용하여 이 리소스를 추가합니다.


        Strings/en-US/Resources.resw ![리소스 추가, 영어](images/addresource-en-us.png) 이 예제에서 "Greeting.Text" 및 "Farewell"은 표시되어야 하는 문자열을 나타냅니다. "Greeting.Width"는 "Greeting" 문자열의 Width 속성을 나타냅니다. 주석은 문자열을 다른 언어로 지역화하는 번역사에게 특별한 지침을 제공할 수 있는 위치입니다.

## <a name="associate-controls-to-resources"></a>컨트롤을 리소스와 연결합니다.

지역화된 텍스트가 필요한 모든 컨트롤을 .resw 파일과 연결해야 합니다. 이렇게 하려면 다음과 같이 XAML 요소의 **x:Uid** 특성을 사용합니다.

```XML
<TextBlock x:Uid="Greeting" Text="" />
```

리소스 이름에는 **Uid** 특성 값을 사용해야 하며, 번역된 문자열을 가져오는 데 사용할 속성을 지정해야 합니다(이 경우 Text 속성). 다른 언어에 대해 Greeting.Width 등의 다른 속성/값을 지정할 수 있지만, 그러한 레이아웃 관련 속성은 주의해서 사용해야 합니다. 장치의 화면을 기반으로 컨트롤이 동적으로 배치될 수 있도록 고려해야 합니다.

첨부된 속성은 resw 파일에서 다르게 처리됩니다(예: AutomationPeer.Name). 네임스페이스를 이와 같이 명확하게 표시해야 합니다.

```XML
MediumButton.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name</code></pre></td>
```

## <a name="add-string-resource-identifiers-to-code-and-markup"></a>코드 및 태그에 문자열 리소스 식별자를 추가합니다.

코드에서는 문자열을 동적으로 참조할 수 있습니다.

**C#**
```CSharp
var loader = new Windows.ApplicationModel.Resources.ResourceLoader();
var str = loader.GetString("Farewell");
```

**C++**
```cpp
auto loader = ref new Windows::ApplicationModel::Resources::ResourceLoader();
auto str = loader->GetString("Farewell");
```


## <a name="add-folders-and-resource-files-for-two-additional-languages"></a>다른 두 언어에 대한 폴더와 리소스 파일을 추가합니다.


1.  독일어용으로 문자열 폴더 아래에 다른 폴더를 추가합니다. 독일어(독일)에 대해 폴더 이름을 "de-DE"로 지정합니다.
2.  de-DE 폴더에 다른 리소스 파일을 만들고 다음을 추가합니다.

    strings/de-DE/Resources.resw

    ![리소스 추가, 독일어](images/addresource-de-de.png)


3.  프랑스어(프랑스)용으로 "fr-FR"이라는 폴더를 하나 더 만듭니다. 새 리소스 파일을 만들고 다음을 추가합니다.

    strings/fr-FR/Resources.resw ![리소스 추가, 프랑스어](images/addresource-fr-fr.png)

## <a name="build-and-run-the-app"></a>앱을 빌드하여 실행합니다.


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

## <a name="related-topics"></a>관련 항목


* [한정자를 사용하여 리소스 이름을 지정하는 방법](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)
* [문자열 리소스를 로드하는 방법](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)
* [BCP-47 언어 태그](http://go.microsoft.com/fwlink/p/?linkid=227302)
 

 






<!--HONumber=Dec16_HO2-->


