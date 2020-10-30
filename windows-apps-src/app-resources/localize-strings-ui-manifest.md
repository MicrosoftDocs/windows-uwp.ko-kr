---
description: 앱에서 다른 표시 언어를 지원하도록 하고 코드, XAML 태그 또는 앱 패키지 매니페스트에 문자열 리터럴이 있으면 해당 문자열을 리소스 파일(.resw)로 이동합니다. 그러면 앱에서 지원하는 언어별로 해당 리소스 파일의 변환된 복사본을 만들 수 있습니다.
title: UI 및 앱 패키지 매니페스트의 문자열 지역화
ms.assetid: E420B9BB-C0F6-4EC0-BA3A-BA2875B69722
label: Localize strings in your UI and app package manifest
template: detail.hbs
ms.date: 11/01/2017
ms.topic: article
keywords: Windows 10, UWP, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: 63e88322c7a85bba1ca1a4ff8ff5f2186c4da9f8
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031836"
---
# <a name="localize-strings-in-your-ui-and-app-package-manifest"></a>UI 및 앱 패키지 매니페스트의 문자열 지역화

앱 지역화의 가치 제안에 대한 자세한 내용은 [세계화 및 지역화](../design/globalizing/globalizing-portal.md)를 참조하세요.

앱에서 다른 표시 언어를 지원하도록 하고 코드, XAML 태그 또는 앱 패키지 매니페스트에 문자열 리터럴이 있으면 해당 문자열을 리소스 파일(.resw)로 이동합니다. 그러면 앱에서 지원하는 언어별로 해당 리소스 파일의 변환된 복사본을 만들 수 있습니다.

하드 코드 된 문자열 리터럴은 **TextBlock** 의 **Text** 속성과 같이 명령적 코드 또는 XAML 태그에 나타날 수 있습니다. 응용 프로그램 패키지 매니페스트 소스 파일 (파일)에도 표시 될 수 있습니다 `Package.appxmanifest` . 예를 들어 Visual Studio 매니페스트 디자이너의 응용 프로그램 탭에 표시 이름 값이 표시 됩니다. 이러한 문자열을 리소스 파일 (. resw)로 이동 하 고 앱 및 매니페스트에서 하드 코드 된 문자열 리터럴을 리소스 식별자에 대 한 참조로 바꿉니다.

이미지 리소스 파일에 이미지 리소스를 하나만 포함 하는 이미지 리소스와 달리 *여러* 문자열 리소스는 문자열 리소스 파일에 포함 됩니다. 문자열 리소스 파일은 리소스 파일 (. resw) 이며 일반적으로 프로젝트의 \Strings 폴더에 이러한 종류의 리소스 파일을 만듭니다. 리소스 파일 (. resw)의 이름에 한정자를 사용 하는 방법에 대 한 배경 지식이 있으면 [언어, 크기 조정 및 기타 한정자에 대 한 리소스를 조정](tailor-resources-lang-scale-contrast.md)합니다 .를 참조 하세요.

## <a name="store-strings-in-a-resources-file"></a>리소스 파일에 문자열 저장

1. 앱의 기본 언어를 설정 합니다.
    1. Visual Studio에서 솔루션을 열고를 엽니다 `Package.appxmanifest` .
    2. 응용 프로그램 탭에서 기본 언어가 적절 하 게 설정 되었는지 확인 합니다 (예: "en" 또는 "en-us"). 나머지 단계에서는 기본 언어를 "en-us"로 설정 했다고 가정 합니다.
    <br>**참고** 최소한이 기본 언어에 대해 지역화 된 문자열 리소스를 제공 해야 합니다. 사용자의 기본 설정 언어 또는 표시 언어 설정에 대 한 더 나은 일치 항목을 찾을 수 없는 경우 로드 되는 리소스입니다.
2. 기본 언어에 대 한 리소스 파일 (. resw)을 만듭니다.
    1. 프로젝트 노드 아래에서 새 폴더를 만들고 이름을 "Strings"로 만듭니다.
    2. 에서 `Strings` 새 하위 폴더를 만들고 이름을 "en-us"로 만듭니다.
    3. 에서 `en-US` 새 리소스 파일 (. resw)을 만들고 이름이 ".resources. resw" 인지 확인 합니다.
    <br>**참고** 이식 하려는 .NET 리소스 파일 (.resx)이 있는 경우 [XAML 및 UI 포팅](../porting/wpsl-to-uwp-porting-xaml-and-ui.md#localization-and-globalization)을 참조 하세요.
3. `Resources.resw`을 열고 이러한 문자열 리소스를 추가 합니다.

    `Strings/en-US/Resources.resw`

    ![> E N U S > 리소스에 대 한 리소스 추가 테이블의 스크린샷. resw 파일.](images/addresource-en-us.png)

    이 예제에서 "인사말"은 표시 될 때 태그에서 참조할 수 있는 문자열 리소스 식별자입니다. 식별자 "인사"의 경우 텍스트 속성에 대 한 문자열이 제공 되 고 Width 속성에 대해 문자열이 제공 됩니다. "인사말과 Text"는 UI 요소의 속성에 해당 하므로 속성 식별자의 예입니다. 예를 들어 이름 열에 "인사말과"를 추가 하 고 해당 값을 "Red"로 설정할 수도 있습니다. "인사" 식별자는 간단한 문자열 리소스 식별자입니다. 여기에는 하위 속성이 없으며 앞에서 설명한 대로 명령적 코드에서 로드 될 수 있습니다. 주석 열은 번역기에 특별 한 지침을 제공 하는 좋은 장소입니다.

    이 예제에서는 이름이 "인사" 인 간단한 문자열 리소스 식별자 항목이 있으므로 동일한 식별자를 기반으로 하 *는 속성* 식별자를 가질 수 없습니다. 따라서 "인사"를 추가 하면 빌드하는 동안 중복 된 항목 오류가 발생 `Resources.resw` 합니다.

    리소스 식별자는 대/소문자를 구분 하지 않으며 리소스 파일당 고유 해야 합니다. 번역기에 대 한 추가 컨텍스트를 제공 하려면 의미 있는 리소스 식별자를 사용 해야 합니다. 변환에 대해 문자열 리소스를 보낸 후에는 리소스 식별자를 변경 하지 않습니다. 지역화 팀은 리소스 식별자를 사용 하 여 리소스의 추가, 삭제 및 업데이트를 추적 합니다. &mdash;"리소스 식별자 이동"이 라고도 하는 리소스 식별자의 변경 내용은 문자열이 &mdash; 삭제 되 고 다른 사용자가 추가 된 것 처럼 표시 되기 때문에 문자열을 다시 변환 해야 합니다.

## <a name="refer-to-a-string-resource-identifier-from-xaml"></a>XAML에서 문자열 리소스 식별자를 참조 하세요.

[X:Uid 지시문](../xaml-platform/x-uid-directive.md) 을 사용 하 여 태그의 컨트롤이 나 다른 요소를 문자열 리소스 식별자와 연결 합니다.

```xaml
<TextBlock x:Uid="Greeting"/>
```

런타임에 `\Strings\en-US\Resources.resw` 이 로드 됩니다 (이제는 프로젝트의 유일한 리소스 파일 이기 때문에). **TextBlock** 의 **x:Uid** 지시문을 사용 하면 조회를 수행 하 여 `Resources.resw` 문자열 리소스 식별자 "인사"를 포함 하는 내의 속성 식별자를 찾을 수 있습니다. "인사말과" 및 "인사말이. Width" 속성 식별자가 발견 되 면 해당 값은 태그에서 로컬로 설정 된 값을 재정의 하 여 **TextBlock** 에 적용 됩니다. "인사말과" 값을 추가한 경우에도 적용 됩니다. 그러나 속성 식별자만 XAML 태그 요소에 속성을 설정 하는 데 사용 되므로이 TextBlock에서 **x:Uid** 를 "인사"로 설정 해도 아무런 효과가 없습니다. `Resources.resw`에 "인사" 문자열 리소스 식별자가 포함 되어 있지만 해당 식별자에 대 한 속성 식별자를 포함 하지 *않습니다* .

XAML 요소에 문자열 리소스 식별자를 할당 하는 경우 해당 식별자에 대 한 *모든* 속성 식별자는 xaml 요소에 적합 합니다. 예를 들어 Textblock에를 설정 하는 경우 `x:Uid="Greeting"` **textblock** 형식에 텍스트 **TextBlock** 속성이 있으므로 "인사말과"를 확인 합니다. 그러나 단추를 설정한 경우 `x:Uid="Greeting"` **단추** 형식에 **Button** 텍스트 속성이 없으므로 "인사말과 텍스트"를 누르면 런타임 오류가 발생 합니다. 이 경우 한 가지 해결 방법은 "ButtonGreeting. Content" 라는 속성 식별자를 작성 하 고 단추에 설정 하는 것입니다 `x:Uid="ButtonGreeting"` . **Button**

리소스 파일에서 **너비** 를 설정 하는 대신 컨트롤을 콘텐츠로 동적으로 크기를 조정 하는 것이 좋습니다.

**참고** [연결 된 속성](../xaml-platform/attached-properties-overview.md)의 경우. resw 파일의 이름 열에 특수 구문이 필요 합니다. 예를 들어 "인사" 식별자에 대 한 [**AutomationProperties.Name**](/uwp/api/windows.ui.xaml.automation.automationproperties.NameProperty) 연결 된 속성의 값을 설정 하려면 이름 열에 입력 합니다.

```xml
Greeting.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name
```

## <a name="refer-to-a-string-resource-identifier-from-code"></a>코드에서 문자열 리소스 식별자를 참조 하세요.

간단한 문자열 리소스 식별자를 기반으로 문자열 리소스를 명시적으로 로드할 수 있습니다.

> [!NOTE]
> 백그라운드/작업자 스레드에서 실행 될 *수* 있는 **GetForCurrentView** 메서드에 대 한 호출이 있는 경우 해당 호출을 테스트를 사용 하 여 보호 `if (Windows.UI.Core.CoreWindow.GetForCurrentThread() != null)` 합니다. 백그라운드/작업자 스레드에서 **GetForCurrentView** 를 호출 하면 " *&lt; &gt; CoreWindow가 없는 스레드에 대해 typename을 만들 수 없습니다.* " 라는 예외가 발생 합니다.

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

클래스 라이브러리 (유니버설 Windows) 또는 [Windows 런타임 라이브러리 (유니버설 windows)](../winrt-components/index.md) 프로젝트에서 이와 동일한 코드를 사용할 수 있습니다. 런타임에 라이브러리를 호스트 하는 응용 프로그램의 리소스가 로드 됩니다. 앱이 더 높은 수준의 지역화를 가질 가능성이 있으므로 라이브러리에서 해당 리소스를 호스팅하는 앱에서 리소스를 로드 하는 것이 좋습니다. 라이브러리가 리소스를 제공 해야 하는 경우 해당 리소스를 입력으로 대체 하는 옵션을 호스팅 앱에 제공 해야 합니다.

리소스 이름이 분할 된 경우 ("." 문자 포함) 마침표를 리소스 이름에서 슬래시 ("/") 문자로 바꿉니다. 예를 들어 속성 식별자에는 점이 포함 됩니다. 따라서 코드에서이 중 하나를 로드 하기 위해이 substition을 수행 해야 합니다.

```csharp
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Fare/Well"); // <data name="Fare.Well" ...> ...
```

확실 하지 않은 경우 [MakePri.exe](makepri-exe-command-options.md) 를 사용 하 여 앱의 PRI 파일을 덤프할 수 있습니다. 각 리소스 `uri` 는 덤프 된 파일에 표시 됩니다.

```xml
<ResourceMapSubtree name="Fare"><NamedResource name="Well" uri="ms-resource://<GUID>/Resources/Fare/Well">...
```

## <a name="refer-to-a-string-resource-identifier-from-your-app-package-manifest"></a>앱 패키지 매니페스트에서 문자열 리소스 식별자를 참조 하세요.

1. 앱 패키지 매니페스트 소스 파일 ( `Package.appxmanifest` 파일)을 엽니다 .이 파일은 기본적으로 앱의 `Display name` 이 문자열 리터럴로 표현 됩니다.

   ![표시 이름이 놀이 Works Cycles로 설정 된 응용 프로그램 탭을 보여 주는 appxmanifest.xml 파일의 스크린샷](images/display-name-before.png)

2. 이 문자열의 지역화할 수 있는 버전을 만들려면를 열고 " `Resources.resw` AppDisplayName" 이라는 이름의 새 문자열 리소스와 "어드벤처 Works Cycles" 값을 추가 합니다.

3. 표시 이름 문자열 리터럴을 방금 만든 문자열 리소스 식별자 ("AppDisplayName")에 대 한 참조로 바꿉니다. `ms-resource`URI (Uniform Resource identifier) 체계를 사용 하 여이 작업을 수행 합니다.

   ![표시 이름이 M S 리소스 앱 표시 이름으로 설정 된 응용 프로그램 탭을 보여 주는 appxmanifest.xml 파일의 스크린샷](images/display-name-after.png)

4. 지역화 하려는 매니페스트의 각 문자열에 대해이 프로세스를 반복 합니다. 예를 들어 앱의 짧은 이름 (시작 시 앱 타일에 나타나도록 구성할 수 있음)이 있습니다. 지역화할 수 있는 응용 프로그램 패키지 매니페스트의 모든 항목 목록은 [지역화 가능한 매니페스트 항목](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)을 참조 하세요.

## <a name="localize-the-string-resources"></a>문자열 리소스 지역화

1. 다른 언어에 대 한 리소스 파일 (. resw)의 복사본을 만듭니다.
    1. "문자열"에서 새 하위 폴더를 만들고 이름을 "de" (Deutschland)로 Deutsch.
   <br>**참고** 폴더 이름에는 모든 [BCP-47 언어 태그](https://tools.ietf.org/html/bcp47)를 사용할 수 있습니다. 언어 한정자 및 공용 언어 태그의 목록에 대 한 자세한 내용은 [언어, 크기 조정 및 기타 한정자에 대 한 리소스](tailor-resources-lang-scale-contrast.md) 맞춤을 참조 하세요.
   2. 폴더에의 복사본을 만듭니다 `Strings/en-US/Resources.resw` `Strings/de-DE` .
2. 문자열을 변환 합니다.
    1. `Strings/de-DE/Resources.resw`을 열고 값 열에서 값을 변환 합니다. 주석을 번역할 필요가 없습니다.

    `Strings/de-DE/Resources.resw`

    ![리소스 추가, 독일어](images/addresource-de-de.png)

원하는 경우 추가 언어에 대해 1 단계와 2 단계를 반복할 수 있습니다.

`Strings/fr-FR/Resources.resw`

![리소스 추가, 프랑스어](images/addresource-fr-fr.png)

## <a name="test-your-app"></a>앱 테스트

앱에서 기본 표시 언어를 테스트 합니다. 그런 다음 **설정**  >  **시간 & 언어**  >  **지역 & 언어** 언어로 표시 언어를 변경  >  **Languages** 하 고 앱을 다시 테스트할 수 있습니다. UI 및 셸에서 문자열을 확인 합니다 (예: &mdash; 표시 이름 &mdash; 및 타일의 짧은 이름인 제목 표시줄).

**참고** 표시 언어 설정과 일치 하는 폴더 이름을 찾을 수 있으면 해당 폴더 내의 리소스 파일이 로드 됩니다. 그렇지 않으면 응용 프로그램의 기본 언어에 대 한 리소스를 사용 하 여 대체 하는 작업이 수행 됩니다.

## <a name="factoring-strings-into-multiple-resources-files"></a>여러 리소스 파일로 문자열 팩터링

모든 문자열을 단일 리소스 파일 (resw)로 유지 하거나 여러 리소스 파일에 걸쳐 요소를 구분할 수 있습니다. 예를 들어 오류 메시지를 한 리소스 파일, 응용 프로그램 패키지 매니페스트 문자열 및 세 번째의 UI 문자열에 유지 하려고 할 수 있습니다. 이 경우 폴더 구조는 다음과 같습니다.

![독일어, U S 영어 및 프랑스어 로캘 폴더와 파일을 포함 하는 놀이 Works 주기 > 문자열 폴더를 보여 주는 솔루션 패널의 스크린샷](images/manifest-resources.png)

특정 파일에 대 한 문자열 리소스 식별자 참조의 범위를 표시 하려면 식별자 앞에를 추가 `/<resources-file-name>/` 합니다. 아래 태그 예제에서는 `ErrorMessages.resw` 이름이 "PasswordTooWeak"이 고 해당 값이 오류를 설명 하는 리소스를 포함 하는 것으로 가정 합니다.

```xaml
<TextBlock x:Uid="/ErrorMessages/PasswordTooWeak"/>
```

`/<resources-file-name>/` *이외의* 리소스 파일에 대 한 문자열 리소스 식별자 앞에를 추가 하기만 하면 `Resources.resw` 됩니다. 이는 "같습니다"가 기본 파일 이름이 기 때문입니다. 따라서이 항목의 이전 예제에서와 같이 파일 이름을 생략 하는 경우에만 해당 됩니다.

아래 코드 예제에서는 `ErrorMessages.resw` 이름이 "MismatchedPasswords"이 고 해당 값이 오류를 설명 하는 리소스를 포함 하는 것으로 가정 합니다.

> [!NOTE]
> 백그라운드/작업자 스레드에서 실행 될 *수* 있는 **GetForCurrentView** 메서드에 대 한 호출이 있는 경우 해당 호출을 테스트를 사용 하 여 보호 `if (Windows.UI.Core.CoreWindow.GetForCurrentThread() != null)` 합니다. 백그라운드/작업자 스레드에서 **GetForCurrentView** 를 호출 하면 " *&lt; &gt; CoreWindow가 없는 스레드에 대해 typename을 만들 수 없습니다.* " 라는 예외가 발생 합니다.

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

"AppDisplayName" 리소스를에서로 이동 하는 경우 `Resources.resw` `ManifestResources.resw` 앱 패키지 매니페스트에서 `ms-resource:AppDisplayName` 로 변경 `ms-resource:/ManifestResources/AppDisplayName` 합니다.

리소스 파일 이름이 분할 된 경우 ("." 문자 포함), 참조 하는 경우 이름에 점을 그대로 둡니다. 리소스 이름과 같이 마침표를 슬래시 ("/") 문자로 바꾸지 **마십시오** .

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("Err.Msgs");
```

확실 하지 않은 경우 [MakePri.exe](makepri-exe-command-options.md) 를 사용 하 여 앱의 PRI 파일을 덤프할 수 있습니다. 각 리소스 `uri` 는 덤프 된 파일에 표시 됩니다.

```xml
<ResourceMapSubtree name="Err.Msgs"><NamedResource name="MismatchedPasswords" uri="ms-resource://<GUID>/Err.Msgs/MismatchedPasswords">...
```

## <a name="load-a-string-for-a-specific-language-or-other-context"></a>특정 언어 또는 기타 컨텍스트에 대 한 문자열 로드

기본 [**resourcecontext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live) ( [**GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView)에서 가져옴)에는 각 한정자 이름에 대 한 한정자 값이 포함 되어 있으며,이 값은 기본 런타임 컨텍스트 (즉, 현재 사용자 및 컴퓨터에 대 한 설정)를 나타냅니다. 리소스 파일 (. resw)은 해당 &mdash; 런타임 컨텍스트의 한정자 값에 대해 해당 이름의 한정자를 기준으로 일치 시킵니다 &mdash; .

그러나 응용 프로그램에서 시스템 설정을 재정의 하 고 일치 하는 리소스 파일을 검색할 때 사용할 언어, 배율 또는 다른 한정자 값을 명시적으로 지정할 수 있습니다. 예를 들어 사용자가 도구 설명 또는 오류 메시지에 대 한 대체 언어를 선택할 수 있도록 할 수 있습니다.

이렇게 하려면 기본 항목을 사용 하는 대신 새 **Resourcecontext** 를 생성 하 고 해당 값을 재정의 한 다음 문자열 조회에 해당 컨텍스트 개체를 사용 합니다.

```csharp
var resourceContext = new Windows.ApplicationModel.Resources.Core.ResourceContext(); // not using ResourceContext.GetForCurrentView
resourceContext.QualifierValues["Language"] = "de-DE";
var resourceMap = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap.GetSubtree("Resources");
this.myXAMLTextBlockElement.Text = resourceMap.GetValue("Farewell", resourceContext).ValueAsString;
```

위의 코드 예제에서와 같이 **QualifierValues** 를 사용 하는 것은 모든 한정자에 대해 작동 합니다. 특수 언어의 경우에는 대신이 작업을 수행할 수 있습니다.

```csharp
resourceContext.Languages = new string[] { "de-DE" };
```

전역 수준에서 동일한 효과를 위해 기본 **Resourcecontext** 에서 한정자 값을 재정의할 *수* 있습니다. 그러나 대신 [**SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)를 호출 하는 것이 좋습니다. **SetGlobalQualifierValue** 에 대 한 호출을 사용 하 여 값을 한 번 설정 하면 해당 값이 조회에 사용할 때마다 기본 **resourcecontext** 에 적용 됩니다.

```csharp
Windows.ApplicationModel.Resources.Core.ResourceContext.SetGlobalQualifierValue("Language", "de-DE");
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
```

일부 한정자에는 시스템 데이터 공급자가 있습니다. 따라서 **SetGlobalQualifierValue** 를 호출 하는 대신 자체 API를 통해 공급자를 조정 하는 것이 좋습니다. 예를 들어이 코드는 [**PrimaryLanguageOverride**](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride)를 설정 하는 방법을 보여 줍니다.

```csharp
Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride = "de-DE";
```

## <a name="updating-strings-in-response-to-qualifier-value-change-events"></a>한정자 값 변경 이벤트에 대 한 응답으로 문자열 업데이트

실행 중인 앱은 기본 **Resourcecontext** 의 한정자 값에 영향을 주는 시스템 설정 변경에 응답할 수 있습니다. 이러한 시스템 설정은 [**QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)에서 [**mapchanged**](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live) 이벤트를 호출 합니다.

이 이벤트에 대 한 응답으로 기본 **Resourcecontext** 에서 문자열을 다시 로드할 수 있습니다.

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

## <a name="load-strings-from-a-class-library-or-a-windows-runtime-library"></a>클래스 라이브러리 또는 Windows 런타임 라이브러리에서 문자열 로드

참조 된 클래스 라이브러리 (유니버설 Windows) 또는 [Windows 런타임 라이브러리 (유니버설 windows)](../winrt-components/index.md) 의 문자열 리소스는 일반적으로 빌드 프로세스 중에 포함 된 패키지의 하위 폴더에 추가 됩니다. 이러한 문자열의 리소스 식별자는 일반적으로 *LibraryName/Sourcefilename/ResourceIdentifier* 형식을 사용 합니다.

라이브러리는 자체 리소스에 대 한 ResourceLoader를 가져올 수 있습니다. 예를 들어 다음 코드에서는 라이브러리 또는이를 참조 하는 앱이 라이브러리의 문자열 리소스에 대 한 ResourceLoader를 가져올 수 있는 방법을 보여 줍니다.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("ContosoControl/Resources");
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("exampleResourceName");
```

Windows 런타임 라이브러리 (유니버설 Windows)의 경우 기본 네임 스페이스가 분할 된 경우 ("." 문자 포함), 리소스 맵 이름에 마침표를 사용 합니다.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("Contoso.Control/Resources");
```

클래스 라이브러리 (유니버설 Windows)에 대해이 작업을 수행할 필요가 없습니다. 확실 하지 않은 경우 구성 요소 또는 라이브러리의 PRI 파일을 덤프 하는 [MakePri.exe 명령줄 옵션](makepri-exe-command-options.md) 을 지정할 수 있습니다. 각 리소스 `uri` 는 덤프 된 파일에 표시 됩니다.

```xml
<NamedResource name="exampleResourceName" uri="ms-resource://Contoso.Control/Contoso.Control/ReswFileName/exampleResourceName">...
```

## <a name="loading-strings-from-other-packages"></a>다른 패키지에서 문자열 로드

앱 패키지에 대 한 리소스는 현재 [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live)에서 액세스할 수 있는 패키지 자체의 최상위 [**resourcemap**](/uwp/api/windows.applicationmodel.resources.core.resourcemap?branch=live) 통해 관리 및 액세스 됩니다. 각 패키지 내에서 다양 한 구성 요소에 고유한 ResourceMap 사용할 수 있습니다 .이 [**하위 트리는 Resourcemap getsubtree**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getsubtree?branch=live)를 통해 액세스할 수 있습니다.

프레임 워크 패키지는 절대 리소스 식별자 URI를 사용 하 여 자체 리소스에 액세스할 수 있습니다. [URI 스키마](uri-schemes.md)도 참조 하세요.

## <a name="loading-strings-in-non-packaged-applications"></a>패키지 되지 않은 응용 프로그램에서 문자열 로드

Windows 버전 1903 (2019 업데이트 될 수 있음)에서 패키지 되지 않은 응용 프로그램은 리소스 관리 시스템을 활용할 수도 있습니다.

UWP 사용자 컨트롤/라이브러리를 만들고 [리소스 파일에 문자열을 저장](#store-strings-in-a-resources-file)하면 됩니다. 그런 다음 [XAML에서 문자열 리소스 식별자를 참조](#refer-to-a-string-resource-identifier-from-xaml)하거나, [코드에서 문자열 리소스 식별자를 참조](#refer-to-a-string-resource-identifier-from-code)하거나, [클래스 라이브러리나 Windows 런타임 라이브러리에서](#load-strings-from-a-class-library-or-a-windows-runtime-library)문자열을 로드할 수 있습니다.

패키지 되지 않은 응용 프로그램에서 리소스를 사용 하려면 다음과 같은 몇 가지 작업을 수행 해야 합니다.

1. 패키지 되지 않은 시나리오에 *현재 뷰가* 없으므로 코드에서 리소스를 확인할 때 [GetForCurrentView](/uwp/api/windows.applicationmodel.resources.resourceloader.getforcurrentview) 대신 [GetForViewIndependentUse](/uwp/api/windows.applicationmodel.resources.resourceloader.getforviewindependentuse) 를 사용 합니다. 비 패키지 시나리오에서 [GetForCurrentView](/uwp/api/windows.applicationmodel.resources.resourceloader.getforcurrentview) 를 호출 하는 경우 다음과 같은 예외가 발생 합니다. *CoreWindow가 없는 스레드에는 리소스 컨텍스트가 생성 되지 않을 수 있습니다* .
1. [MakePri.exe](./compile-resources-manually-with-makepri.md) 를 사용 하 여 앱의 리소스 .pri 파일을 수동으로 생성 합니다.
    - `makepri new /pr <PROJECTROOT> /cf <PRICONFIG> /of resources.pri`을 실행합니다.
    - &lt;Priconfig은 &gt; &lt; &gt; 모든 리소스가 단일 리소스. pri 파일에 번들 되도록 "패키징" 섹션을 생략 해야 합니다. [Createconfig](./makepri-exe-command-options.md#createconfig-command)에서 만든 기본 [MakePri.exe 구성 파일](./makepri-exe-configuration.md) 을 사용 하는 경우 " &lt; 패키징 &gt; " 섹션을 만든 후 수동으로 삭제 해야 합니다.
    - &lt;Priconfig은 &gt; 프로젝트의 모든 리소스를 단일 리소스. pri 파일에 병합 하는 데 필요한 모든 관련 인덱서를 포함 해야 합니다. [Createconfig](./makepri-exe-command-options.md#createconfig-command) 에서 만든 기본 [MakePri.exe 구성 파일](./makepri-exe-configuration.md) 은 모든 인덱서를 포함 합니다.
    - 기본 구성을 사용 하지 않는 경우에는 PRI 인덱서 (이 작업을 수행 하는 방법에 대 한 기본 구성 검토)를 사용 하도록 설정 해야 합니다 (이 작업을 수행 하는 방법에 대 한 기본 구성 검토).
        > [!NOTE]
        > 을 생략 하 `/IndexName` 고 프로젝트에서 응용 프로그램 매니페스트를 포함 하지 않는 경우 PRI 파일의 IndexName/root 네임 스페이스는 자동으로 *응용 프로그램* 으로 설정 됩니다. 그러면 런타임에서 패키지 되지 않은 앱을 인식 하 게 됩니다. 이렇게 하면 패키지 ID의 이전 하드 종속성이 제거 됩니다. 리소스 Uri를 지정 하는 경우, 루트 네임 스페이스를 생략 하는 *응용 프로그램* 을 패키지 되지 않은 앱에 대 한 루트 네임 스페이스로 유추 하는 리소스 ( *Application* 예:
1. PRI 파일을 .exe의 빌드 출력 디렉터리에 복사 합니다.
1. .Exe를 실행 합니다. 
    > [!NOTE]
    > 리소스 관리 시스템은 패키지 되지 않은 앱의 언어를 기반으로 리소스를 확인할 때 사용자 기본 언어 목록이 아닌 시스템 표시 언어를 사용 합니다. 사용자 기본 설정 언어 목록은 UWP 앱에만 사용 됩니다.

> [!Important]
> 리소스를 수정할 때마다 수동으로 PRI 파일을 다시 작성 해야 합니다. [MakePri.exe](./compile-resources-manually-with-makepri.md) 명령을 처리 하는 빌드 후 스크립트를 사용 하 고 .exe 디렉터리에 리소스 pri 출력을 복사 하는 것이 좋습니다.

## <a name="important-apis"></a>중요 API
* [ApplicationModel .Resources. ResourceLoader](/uwp/api/Windows.ApplicationModel.Resources.ResourceLoader)
* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)
* [MapChanged](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live)

## <a name="related-topics"></a>관련 항목
* [XAML 및 UI 포팅](../porting/wpsl-to-uwp-porting-xaml-and-ui.md#localization-and-globalization)
* [x:Uid 지시어](../xaml-platform/x-uid-directive.md)
* [연결 된 속성](../xaml-platform/attached-properties-overview.md)
* [지역화할 수 있는 매니페스트 항목](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)
* [BCP-47 언어 태그](https://tools.ietf.org/html/bcp47)
* [언어, 크기 조정 및 기타 한정자에 맞게 리소스를 조정 합니다.](tailor-resources-lang-scale-contrast.md)
* [문자열 리소스를 로드 하는 방법](/previous-versions/windows/apps/hh965323(v=win.10))
