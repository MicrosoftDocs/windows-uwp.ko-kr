---
Description: If you want your app to support different display languages, and you have string literals in your code or XAML markup or app package manifest, then move those strings into a Resources File (.resw). You can then make a translated copy of that Resources File for each language that your app supports.
title: UI와 앱 패키지 매니페스트에 문자열 지역화
ms.assetid: E420B9BB-C0F6-4EC0-BA3A-BA2875B69722
label: Localize strings in your UI and app package manifest
template: detail.hbs
ms.date: 11/01/2017
ms.topic: article
keywords: Windows 10, uwp, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: 6740e6ce35277fa7f7f088c312f8b9ee1f5281c3
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7838531"
---
# <a name="localize-strings-in-your-ui-and-app-package-manifest"></a>UI와 앱 패키지 매니페스트에 문자열 지역화
앱 지역화의 가치 제안에 대한 자세한 내용은 [세계화 및 지역화](../design/globalizing/globalizing-portal.md)를 참조하세요.

앱에서 다른 표시 언어를 지원하길 원하고 코드 또는 XAML 태그 또는 앱 패키지 매니페스트에 문자열 리터럴이 있는 경우, 해당 문자열을 리소스 파일(.resw)로 이동합니다. 그런 다음 앱에서 지원하는 언어별로 해당 리소스 파일의 번역본을 만들 수 있습니다.

하드 코드된 문자열 리터럴은 명령적 코드 또는 XAML 태그, 예를 들어 **TextBlock**의 **Text** 속성으로 표시될 수 있습니다. 앱 패키지 매니페스트 소스 파일(`Package.appxmanifest`파일)에 표시될 수도 있습니다. 예를 들면 Visual Studio 매니페스트 디자이너의 응용 프로그램 탭에 표시 이름에 대한 값으로 표시됩니다. 이러한 문자열을 리소스 파일(.resw)로 이동하고 앱 및 매니페스트에 하드 코드된 문자열 리터럴을 리소스 식별자에 대한 참조로 바꿉니다.

이미지 리소스 파일에 하나의 이미지 리소스만 있는 이미지 리소스와는 달리 *여러* 문자열 리소스는 문자열 리소스 파일에 포함되어 있습니다. 문자열 리소스 파일은 리소스 파일(.resw)이며 일반적으로 이 유형의 리소스 파일을 프로젝트의 \Strings 폴더에 만듭니다. 리소스 파일(.resw) 이름에 한정자를 사용하는 방법에 대한 배경 지식은 [언어, 규모 및 기타 한정자에 맞게 리소스 조정](tailor-resources-lang-scale-contrast.md)을 참조하세요.

## <a name="create-a-resources-file-resw-and-put-your-strings-in-it"></a>리소스 파일(.resw)을 이에 문자열 추가
1. 앱의 기본 언어를 설정합니다.
    1. Visual Studio에서 솔루션을 연 상태로 `Package.appxmanifest`를 엽니다.
    2. 응용 프로그램 탭에서 기본 언어가 제대로 설정되어 있는지 확인합니다(예: "en" 또는 "en-US"). 나머지 단계에서는 기본 언어를 "en-US"로 설정했다고 가정합니다.
    <br>**참고**최소한이 기본 언어에 대해 지역화 된 문자열 리소스를 제공 해야 합니다. 이러한 리소스는 사용자의 기본 설정 언어 또는 표시 언어 설정에 대해 더 나은 일치를 찾을 수 없는 경우 로드됩니다.
2. 기본 언어에 대한 리소스 파일(.resw)을 만듭니다.
    1. 프로젝트 노드에서 새 폴더를 만들고 이름을 "Strings"로 지정합니다.
    2. `Strings` 아래에 새 하위 폴더를 만들고 이름을 "en-US"로 지정합니다.
    3. `en-US`아래에서 새 리소스 파일(.resw)을 만들고 이름이 "Resources.resw"인지 확인합니다.
    <br>**참고**이식 하려는.NET 리소스 파일 (.resx)가 경우 [XAML 및 UI 포팅](../porting/wpsl-to-uwp-porting-xaml-and-ui.md#localization-and-globalization)참조 하세요.
3.  `Resources.resw`를 열고 이러한 문자열 리소스를 추가합니다.

    `Strings/en-US/Resources.resw`

    ![리소스 추가, 영어](images/addresource-en-us.png)

    이 예에서 "Greeting"은 태그에서 참조할 수 있는 문자열 리소스 식별자이며, 이에 대해 알아보겠습니다. Greeting" 식별자의 경우 문자열은 Text 속성에 대해 제공되고 문자열은 Width 속성에 대해 제공됩니다. "Greeting.Text"는 UI 요소의 속성에 해당하므로 속성 식별자의 예가 됩니다. 또한 예를 들어 이름 열에 "Greeting.Foreground"를 추가하고 값을 "빨간색"으로 설정수 있습니다. "Farewell" 식별자는 단순한 문자열 리소스 식별자입니다. 하위 속성을 가지지 않으며 명령적 코드에서 로드됩니다. 이에 대해 설명할 것입니다. 설명 열은 번역가에게 특별한 지침을 전달하기 좋은 위치입니다.

    이 예에서 "Farewell"이라는 단순한 문자열 리소스 식별자 항목이 있기 때문에 동일한 식별자에 기반한 속성 식별자 *또한* 가질 수 없습니다. 따라서 "Farewell.Text"를 추가하면 `Resources.resw`를 빌드할 때 중복된 항목 오류가 발생합니다.

    리소스 식별자는 대/소문자를 구분하지 않으며 리소스 파일별로 고유해야 합니다. 의미 있는 리소스 식별자를 사용하여 번역가를 위한 추가 컨텍스트를 제공해야 합니다. 그리고 문자열 리소스를 번역에 사용하도록 보낸 이후에는 리소스 식별자를 변경하지 마세요. 지역화 팀은 리소스 식별자를 사용하여 리소스의 추가, 삭제, 업데이트를 추적합니다. 리소스 식별자를 변경하면("리소스 식별자 전환"이라고도 함) 문자열을 삭제하고 다른 문자열을 추가한 것처럼 보이므로 문자열을 다시 번역해야 합니다.

## <a name="refer-to-a-string-resource-identifier-from-xaml-markup"></a>XAML 코드에서 문자열 리소스 식별자 참조
[x:Uid 지시어](../xaml-platform/x-uid-directive.md)를 사용하여 태그의 컨트롤 또는 기타 요소를 문자열 리소스 식별자와 연결할 수 있습니다.

```xaml
<TextBlock x:Uid="Greeting"/>
```

실행 시 `\Strings\en-US\Resources.resw`가 로드됩니다(현재 이것이 프로젝트의 유일한 리소스 파일입니다). **TextBlock**의 **x:Uid** 지시어로 조회를 실행하여 문자열 리소스 식별자 "Greeting"을 포함하는 `Resources.resw` 내에 있는 속성 식별자를 찾을 수 있습니다. "Greeting.Text" 및 "Greeting.Width" 속성 식별자가 검색되고 해당 값은 **TextBlock**에 적용되며 태그에 로컬로 설정된 모든 값을 재정의합니다. "Greeting.Foreground" 값 또한 사용자가 추가한 경우 적용됩니다. 그러나 유일한 속성 식별자는 XAML 태그 요소의 속성을 설정하는 데 사용되기 때문에 이 TextBlock에서 **x:Uid**를 "Farewell"로 설정해도 영양을 미치지 않습니다. `Resources.resw` *does*에 문자열 리소스 식별자 "Farewell"가 포함되어 있지만 이에 대한 속성 식별자는 포함하지 않습니다.

XAML 요소에 문자열 리소스 식별자를 할당할 해당 식별자에 대한 *모든*이 XAML 요소에 적절한지 확인하세요. 예를 들어 **TextBlock**에서 `x:Uid="Greeting"`을 설정하면 **TextBlock** 유형에 Text 속성이 있기 때문에 "Greeting.Text"가 확인됩니다. 그러나 **Button**에 `x:Uid="Greeting"`을 설정하는 경우 **Button** 형식의 텍스트 속성이 없기 때문에 "Greeting.Text"는 런타임 오류를 발생합니다. 이 경우에 대한 한 가지 해결 방법은 "ButtonGreeting.Content"라는 속성 식별자를 작성하고 **Button**에 `x:Uid="ButtonGreeting"`을 설정하는 것입니다.

리소스 파일에서 **너비**를 설정하는 대신 콘텐츠에 동적으로 크기를 맞추는 컨트롤을 허용하도록 할 수 있습니다.

**참고** [연결 된 속성](../xaml-platform/attached-properties-overview.md)에 대 한.resw 파일의 이름 열에 특수 구문이 필요 합니다. 예를 들어 "Greeting" 식별자에 대한 [**AutomationProperties.Name**](/uwp/api/windows.ui.xaml.automation.automationproperties.NameProperty) 연결된 속성에 대한 값을 설정하려면 이름 열에서 이를 입력합니다.

```xml
Greeting.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name
```

## <a name="refer-to-a-string-resource-identifier-from-code"></a>코드에서 문자열 리소스 식별자 참조
명시적으로 단순한 문자열 리소스 식별자를 기반으로 문자열 리소스를 로드할 수 있습니다.

> [!NOTE]
> 백그라운드/작업자 스레드에서 *실행되었을 수 있는* **GetForCurrentView** 메서드를 호출하는 경우 해당 호출을 `if (Windows.UI.Core.CoreWindow.GetForCurrentThread() != null)` 테스트로 보호하세요. 백그라운드/작업자 스레드에서 **GetForCurrentView**를 호출하면 예외 "CoreWindow가 없는 스레드에서는 *&lt;typename&gt;을(를) 만들 수 없습니다.*"가 발생합니다.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
```

```cppwinrt
auto resourceLoader{ Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView() };
myXAMLTextBlockElement().Text(resourceLoader.GetString(L"Farewell"));
```

```cpp
auto resourceLoader = Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView();
this->myXAMLTextBlockElement->Text = resourceLoader->GetString("Farewell");
```

클래스 라이브러리(유니버설 Windows) 또는 [Windows 런타임 라이브러리(유니버설 Windows)](../winrt-components/index.md) 프로젝트 내에서 동일할 코드를 사용할 수 있습니다. 런타임 시 라이브러리를 호스팅하는 앱의 리소스가 로드됩니다. 앱에는 뛰어난 지역화 수준이 있을 가능성이 높으므로 라이브러리를 호스팅하는 앱에서 리소스를 로드하는 것이 좋습니다. 라이브러리는 리소스를 제공한 다음 호스팅 앱에 이러한 리소스를 입력으로 교체하는 옵션을 제공해야 합니다.

리소스 이름을 분할 하는 경우 (포함 된 "." 문자), 다음 바꾸기 점으로 슬래시 ("/")를 사용 하 여 리소스 이름에서 문자입니다. 속성 식별자 예 포함 되는 점입니다. 따라서이 substition 코드에서 그 중 하나를 로드 하기 위해 수행 해야 합니다.

```csharp
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Fare/Well"); // <data name="Fare.Well" ...> ...
```

잘 모르겠으면, 앱의 PRI 파일을 덤프 하 [MakePri.exe](makepri-exe-command-options.md) 를 사용할 수 있습니다. 각 리소스의 `uri` 덤프 파일에 표시 됩니다.

```xml
<ResourceMapSubtree name="Fare"><NamedResource name="Well" uri="ms-resource://<GUID>/Resources/Fare/Well">...
```

## <a name="refer-to-a-string-resource-identifier-from-your-app-package-manifest"></a>앱 패키지 매니페스트에서 문자열 리소스 식별자 참조
1. 기본적으로 앱의 표시 이름이 문자열 리터럴로 표시되는 앱 패키지 매니페스트 소스 파일(`Package.appxmanifest` 파일)을 엽니다.

   ![리소스 추가, 영어](images/display-name-before.png)

2. 이 문자열의 지역화된 버전을 만들려면 `Resources.resw`를 열고 이름이 "AppDisplayName"이고 값이 "Adventure Works Cycles"인 새 문자열 리소스를 추가합니다.

3. 표시 이름 문자열 리터럴을 방금 만든 문자열 리소스 식별자 ("AppDisplayName")에 대한 참조로 바꿉니다. `ms-resource`URI(Uniform Resource Identifier) 스키마를 사용하여 이 작업을 수행합니다.

   ![리소스 추가, 영어](images/display-name-after.png)

4. 지역화하고자 하는 매니페스트의 각 문자열에 대해 이 프로세스를 반복합니다. 예를 들면 앱의 짧은 이름(시작의 앱 타일에 표시되도록 구성할 수 있음)입니다. 지역화할 수 있는 앱 패키지 매니페스트의 모든 항목 목록을 보려면 [지역화할 수 있는 매니페스트 항목](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)을 참조하세요.

## <a name="localize-the-string-resources"></a>문자열 리소스 지역화
1. 다른 언어에 대한 리소스 파일(.resw)의 복사본을 만듭니다.
    1. "Strings" 아래에서 새 하위 폴더를 만들고 독일어(독일)에 대해 이름을 "de-DE"로 지정합니다.
   <br>**참고**폴더 이름에 대 한 모든 [bcp-47 언어 태그](http://go.microsoft.com/fwlink/p/?linkid=227302)를 사용할 수 있습니다. 언어 한정자 및 일반 언어 태그 목록에 대한 자세한 내용은 [언어, 규모 및 기타 한정자에 맞게 리소스 조정](tailor-resources-lang-scale-contrast.md)을 참조하세요.
   2. `Strings/de-DE` 폴더의 `Strings/en-US/Resources.resw`의 복사본을 만듭니다.
2. 문자열을 번역합니다.
    1. `Strings/de-DE/Resources.resw`를 열고 값 열에서 값을 번역합니다. 설명을 번역할 필요가 없습니다.

    `Strings/de-DE/Resources.resw`

    ![리소스 추가, 독일어](images/addresource-de-de.png)

원하는 경우 추가 언어에 대해 1단계와 2단계를 반복합니다.

`Strings/fr-FR/Resources.resw`

![리소스 추가, 프랑스어](images/addresource-fr-fr.png)

## <a name="test-your-app"></a>앱 테스트
기본 표시 언어에 대해 앱을 테스트합니다. 그런 다음 **설정** > **시간 및 언어** > **지역 및 언어** > **언어**에서 표시 언어를 변경하고 앱을 다시 테스트할 수 있습니다. UI와 셸의 문자열을 확인합니다(예를 들어, 사용자의 표시 이름이 제목 표시줄 및 타일의 짧은 이름).

**참고** 표시 언어 설정과 일치하는 폴더 이름을 찾을 수 있을 경우 해당 폴더 내의 리소스 파일이 로드됩니다. 그렇지 않은 경우 앱의 기본 언어에 대한 리소스로 대체됩니다.

## <a name="factoring-strings-into-multiple-resources-files"></a>여러 개의 리소스 파일로 문자열 팩터링
모든 문자열을 단일 리소스 파일(resw)에 두거나 여러 리소스 파일에 팩터링할 수 있습니다. 예를 들어 하나의 리소스 파일에 오류 메시지를 두고 앱 패키지 매니페스트 문자열은 다른 파일에, UI 문자열은 세 번째 파일에 두고자 할 수 있습니다. 이 경우 폴더 구조는 이러한 모습입니다.

![리소스 추가, 영어](images/manifest-resources.png)

특정 파일에 대한 문자열 리소스 식별자 참조 범위를 지정하려면 식별자 전에 `/<resources-file-name>/`을 추가하면 됩니다. 아래의 태그 예제는 `ErrorMessages.resw`가 이름이 "PasswordTooWeak.Text"고 값이 오류를 설명하는 리소스를 포함한다고 가정합니다.

```xaml
<TextBlock x:Uid="/ErrorMessages/PasswordTooWeak"/>
```

`Resources.resw`가 *아닌* 리소스 파일에 대한 문자열 리소스 식별자 전에 `/<resources-file-name>/`을 추가하기만 하면 됩니다. 이는 "Resources.resw"가 기본 파일 이름이므로 파일 이름을 생략한 경우 이것이 파일 이름으로 간주됩니다(이 항목의 이전 예제에서 설명).

아래의 코드 예제는 `ErrorMessages.resw`가 이름이 "MismatchedPasswords"고 값이 오류를 설명하는 리소스를 포함한다고 가정합니다.

> [!NOTE]
> 백그라운드/작업자 스레드에서 *실행되었을 수 있는* **GetForCurrentView** 메서드를 호출하는 경우 해당 호출을 `if (Windows.UI.Core.CoreWindow.GetForCurrentThread() != null)` 테스트로 보호하세요. 백그라운드/작업자 스레드에서 **GetForCurrentView**를 호출하면 예외 "CoreWindow가 없는 스레드에서는 *&lt;typename&gt;을(를) 만들 수 없습니다.*"가 발생합니다.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("ErrorMessages");
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("MismatchedPasswords");
```

```cppwinrt
auto resourceLoader{ Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView(L"ErrorMessages") };
myXAMLTextBlockElement().Text(resourceLoader.GetString(L"MismatchedPasswords"));
```

```cpp
auto resourceLoader = Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView("ErrorMessages");
this->myXAMLTextBlockElement->Text = resourceLoader->GetString("MismatchedPasswords");
```

"AppDisplayName" 리소스를 `Resources.resw`에서 `ManifestResources.resw`로 이동하고자 하는 경우 앱 패키지 매니페스트에서 `ms-resource:AppDisplayName`을 `ms-resource:/ManifestResources/AppDisplayName`으로 변경합니다.

리소스 파일 이름이 있는 분할 하는 경우 (포함 된 "." 문자)를 참조 하는 경우 이름에 점이 둡니다. 리소스 이름에 대 한 것 처럼 슬래시 ("/") 문자로 점을 대체 **하지 않습니다** .

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("Err.Msgs");
```

잘 모르겠으면, 앱의 PRI 파일을 덤프 하 [MakePri.exe](makepri-exe-command-options.md) 를 사용할 수 있습니다. 각 리소스의 `uri` 덤프 파일에 표시 됩니다.

```xml
<ResourceMapSubtree name="Err.Msgs"><NamedResource name="MismatchedPasswords" uri="ms-resource://<GUID>/Err.Msgs/MismatchedPasswords">...
```

## <a name="load-a-string-for-a-specific-language-or-other-context"></a>특정 언어 또는 다른 컨텍스트에 대한 문자열 로드
기본 [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)([**ResourceContext.GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView)에서 가져옴)에는 기본 런타임 컨텍스트(즉, 현재 사용자와 시스템에 대한 설정)를 나타내는 각 한정자 이름에 대한 한정자 값이 포함됩니다. 리소스 파일(.resw)은 이름의 한정자에 따라 해당 런타임 컨텍스트의 한정자 값에 대해 일치합니다.

하지만 앱에서 시스템 설정을 재정의하여 로드할 일치하는 리소스 파일을 찾을 때 사용할 언어, 배율, 또는 기타 한정자 값을 명시하고자 할 수 있습니다. 예를 들어, 사용자가 도구 설명 또는 오류 메시지에 대해 다른 언어를 선택할 수 있도록 할 수 있습니다.

이는 새 **ResourceContext**(기본값 대신)를 구성하고 해당 값을 재정의한 다음 문자열 조회에서 해당 컨텍스트 개체를 사용하여 수행할 수 있습니다.

```csharp
var resourceContext = new Windows.ApplicationModel.Resources.Core.ResourceContext(); // not using ResourceContext.GetForCurrentView
resourceContext.QualifierValues["Language"] = "de-DE";
var resourceMap = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap.GetSubtree("Resources");
this.myXAMLTextBlockElement.Text = resourceMap.GetValue("Farewell", resourceContext).ValueAsString;
```

위 예제에서와 같이 **QualifierValues**는 모든 한정자에 대해 사용할 수 있습니다. 특수한 언어의 경우 대신 이를 수행할 수 있습니다.

```csharp
resourceContext.Languages = new string[] { "de-DE" };
```

전역 수준에서 같은 효과를 적용하려면 기본 **ResourceContext**의 한정자 값을 재정의*하면 됩니다*. 그러나 이 대신 [**ResourceContext.SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)를 호출하는 것이 좋습니다. **SetGlobalQualifierValue**를 호출하는 값을 한 번 설정하면 조회를 위해 해당 값을 사용할 때마다 기본 **ResourceContext**에 적용됩니다.

```csharp
Windows.ApplicationModel.Resources.Core.ResourceContext.SetGlobalQualifierValue("Language", "de-DE");
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
```

일부 한정자에는 시스템 데이터 공급자가 있습니다. 따라서 **SetGlobalQualifierValue**를 호출하는 대신 자체 API를 통해 공급자를 조정할 수 있습니다. 예를 들어, 이 코드는 [**PrimaryLanguageOverride**](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride)를 설정하는 방법을 보여 줍니다.

```csharp
Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride = "de-DE";
```

## <a name="updating-strings-in-response-to-qualifier-value-change-events"></a>한정자 값 변경 이벤트에 대한 응답으로 문자열 업데이트
실행 중인 앱은 기본 **ResourceContext**의 한정자 값에 영향을 미치는 시스템 설정의 변경에 응답할 수 있습니다. 이러한 모든 시스템 설정은 [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)에서 [**MapChanged**](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live) 이벤트를 호출합니다.

이 이벤트에 대한 응답으로 기본 **ResourceContext**에서 문자열을 다시 로드할 수 있습니다.

```csharp
public MainPage()
{
    this.InitializeComponent();

    ...

    // Subscribe to the event that's raised when a qualifier value changes.
    var qualifierValues = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
    qualifierValues.MapChanged += new Windows.Foundation.Collections.MapChangedEventHandler<string, string>(QualifierValues_MapChanged);
}

private async void QualifierValues_MapChanged(IObservableMap<string, string> sender, IMapChangedEventArgs<string> @event)
{
    var dispatcher = this.myXAMLTextBlockElement.Dispatcher;
    if (dispatcher.HasThreadAccess)
    {
        this.RefreshUIText();
    }
    else
    {
        await dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () => this.RefreshUIText());
    }
}

private void RefreshUIText()
{
    var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
    this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
}
```

## <a name="loading-strings-from-a-class-library-or-a-windows-runtime-library"></a>클래스 라이브러리 또는 Windows 런타임 라이브러리에서 문자열 로드
참조 클래스 라이브러리(유니버설 Windows) 또는 [Windows 런타임 라이브러리(유니버설 Windows)](../winrt-components/index.md)의 문자열 리소스는 일반적으로 빌드 과정에서 포함되는 패키지의 하위 폴더에 추가됩니다. 이러한 문자열의 리소스 식별자는 대개 *LibraryName/ResourcesFileName/ResourceIdentifier*의 형식을 띱니다.

라이브러리는 자체 리소스에 대한 ResourceLoader를 가져올 수 있습니다. 예를 들어 다음 코드는 라이브러리 또는 참조 하는 앱이 라이브러리의 문자열 리소스에 대 한 ResourceLoader를 가져오는 방법을 보여 줍니다.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("ContosoControl/Resources");
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("exampleResourceName");
```

Windows 런타임 라이브러리 (유니버설 Windows), 기본 네임 스페이스를 분할 하는 경우 (포함 된 "." 문자), 리소스 맵 이름에 점이 사용 합니다.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("Contoso.Control/Resources");
```

클래스 라이브러리 (유니버설 Windows)에 대해 수행할 필요가 없습니다. 잘 모르겠으면, [MakePri.exe](makepri-exe-command-options.md) 구성 요소 또는 라이브러리의 PRI 파일을 덤프를 사용할 수 있습니다. 각 리소스의 `uri` 덤프 파일에 표시 됩니다.

```xml
<NamedResource name="exampleResourceName" uri="ms-resource://Contoso.Control/Contoso.Control/ReswFileName/exampleResourceName">...
```

## <a name="loading-strings-from-other-packages"></a>다른 패키지에서 문자열 로드
앱 패키지에 대 한 리소스를 관리 하 고 패키지를 통해 액세스 현재[**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live)에서 액세스할 수 있는 최상위[**ResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemap?branch=live) 소유 합니다. 각 패키지 내에서 다양 한 구성 요소는 해당 ownResourceMapsubtrees [**ResourceMap.GetSubtree**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getsubtree?branch=live)통해 액세스할 수 있을 수 있습니다.

프레임워크 패키지는 절대 리소스 식별자 URI로 자체 리소스에 액세스할 수 있습니다. [URI 스키마](uri-schemes.md)도 참조하세요.

## <a name="important-apis"></a>중요 API
* [ApplicationModel.Resources.ResourceLoader](https://msdn.microsoft.com/library/windows/apps/br206014)
* [ResourceContext.SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)
* [MapChanged](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live)

## <a name="related-topics"></a>관련 항목
* [XAML 및 UI 포팅](../porting/wpsl-to-uwp-porting-xaml-and-ui.md#localization-and-globalization)
* [x:Uid 지시어](../xaml-platform/x-uid-directive.md)
* [연결된 속성](../xaml-platform/attached-properties-overview.md)
* [지역화할 수 있는 매니페스트 항목](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)
* [BCP-47 언어 태그](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [언어, 규모 및 기타 한정자에 맞게 리소스 조정](tailor-resources-lang-scale-contrast.md)
* [문자열 리소스를 로드하는 방법](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)