---
title: C#에서 클립보드 샘플을 C++/WinRT로 포팅(사례 연구)
description: 이 항목에서는 [UWP(유니버설 Windows 플랫폼) 앱 샘플](https://github.com/microsoft/Windows-universal-samples) 중 하나를 [C#](/visualstudio/get-started/csharp)에서 [C++/WinRT](./intro-to-using-cpp-with-winrt.md)로 포팅하는 사례 연구를 제공합니다.
ms.date: 04/13/2020
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 이식, 마이그레이션, C#, 샘플, 클립보드, 사례, 연구
ms.localizationpriority: medium
ms.openlocfilehash: 5a7ec46b28a8ddf0b4accadb37b40e786ac8c47a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170417"
---
# <a name="porting-the-clipboard-sample-tocwinrtfromcmdasha-case-study"></a>C#에서 클립보드 샘플을 C++/WinRT로 이식(사례 연구)

이 항목에서는 [UWP(유니버설 Windows 플랫폼) 앱 샘플](https://github.com/microsoft/Windows-universal-samples) 중 하나를 [C#](/visualstudio/get-started/csharp)에서 [C++/WinRT](./intro-to-using-cpp-with-winrt.md)로 포팅하는 사례 연구를 제공합니다. 연습을 진행하면서 샘플을 직접 이식하여 이식을 연습해보고 값진 경험을 얻을 수 있습니다.

C#에서 C++/WinRT로 이식하는 데 관련된 기술 세부 정보에 대한 포괄적인 카탈로그는 [C#에서 C++/WinRT로 이동](./move-to-winrt-from-csharp.md) 관련 문서를 참조하세요.

## <a name="a-brief-preface-about-c-and-c-source-code-files"></a>C# 및 C++ 소스 코드 파일에 대한 간략한 소개

C# 프로젝트에서 소스 코드 파일은 주로 `.cs` 파일입니다. C++로 이동하면 더 많은 종류의 소스 코드 파일을 사용합니다. 그 이유는 컴파일러 간의 차이점, C++ 소스 코드가 다시 사용되는 방식, 형식 및 해당 기능(메서드)에 대한 *선언* 및 *정의* 개념과 관련이 있습니다.

함수 *선언*은 함수의 *서명*(반환 형식, 이름, 매개 변수 형식 및 이름)만 설명합니다. 함수 *정의*에는 함수의 *본문*(구현)이 포함됩니다.

형식에 관해서는 약간 다릅니다. 형식은 해당 이름을 제공하고 모든 멤버 함수(및 기타 멤버)를 *선언*하여(최소한으로) *정의*합니다. 해당 멤버 함수를 정의하지 않아도 형식을 *정의*할 수 있습니다.

- 공통 C++ 소스 코드 파일은 `.h` 및 `.cpp` 파일입니다. `.h` 파일은 *헤더* 파일이며 하나 이상의 형식을 정의합니다. 헤더에서 멤버 함수 정의가 *가능*하긴 하지만, 일반적으로 `cpp` 파일이 담당합니다. 따라서 가상 형식의 C++ 형식 **MyClass**의 경우 `MyClass.h`에서 **MyClass**를 정의하고, `MyClass.cpp`에서 해당 멤버 함수를 정의합니다. 다른 개발자가 클래스를 다시 사용하기 위해 `.h` 파일 및 개체 코드만 공유합니다. 구현에서 지적 재산권을 구성하기 때문에 `.cpp` 파일은 비밀로 유지해야 합니다.
- 미리 컴파일된 헤더(`pch.h`). 일반적으로 애플리케이션에 포함되는 헤더 파일 세트가 있으며, 이는 거의 변경되지 않습니다. 따라서 컴파일할 때마다 해당 헤더 세트의 콘텐츠를 처리하는 대신, 해당 헤더를 하나로 집계하고, 한 번 컴파일한 다음, 빌드할 때마다 미리 컴파일(precompilation) 단계의 출력을 사용할 수 있습니다. *미리 컴파일된 헤더* 파일(일반적으로 `pch.h`)을 통해 이 작업을 수행합니다.
- `.idl` 파일. 이러한 파일에는 IDL(Interface Definition Language)이 포함되어 있습니다. IDL을 Windows 런타임 형식에 대한 헤더 파일로 간주할 수 있습니다. [**MainPage** 형식에 대한 IDL](#idl-for-the-mainpage-type) 섹션에서 IDL에 대해 자세히 설명합니다.

## <a name="download-and-test-the-clipboard-sample"></a>클립보드 샘플 다운로드 및 테스트

[클립보드 샘플](/samples/microsoft/windows-universal-samples/clipboard/) 웹 페이지를 방문한 후 **ZIP 다운로드**를 클릭합니다. 다운로드한 파일의 압축을 풀고 폴더 구조를 확인합니다.

- 샘플 소스 코드의 C# 버전은 `cs`라는 폴더에 포함되어 있습니다.
- 샘플 소스 코드의 C++/WinRT 버전은 `cppwinrt`라는 폴더에 포함되어 있습니다.
- &mdash;C# 버전과 C++/WinRT 버전에서 사용되는&mdash; 다른 파일은 `shared` 및 `SharedContent` 폴더에 있습니다.

이 항목의 연습에서는 C# 소스 코드에서 이식하여 Clipboard 샘플의 C++/WinRT 버전을 다시 만드는 방법을 보여 줍니다. 이를 통해 고유한 C# 프로젝트를 C++/WinRT로 이식하는 방법을 확인할 수 있습니다.

샘플에서 수행되는 작업을 이해하려면 C# 솔루션(`\Clipboard_sample\cs\Clipboard.sln`)을 열고 구성을 적절히 변경한 후(예: *x64*) 빌드하고 실행합니다. 샘플의 자체 UI(사용자 인터페이스)에서는 다양한 기능을 단계별로 안내합니다.

## <a name="create-a-blank-app-cwinrt-named-clipboard"></a>Clipboard라는 비어 있는 앱(C++/WinRT) 만들기

> [!NOTE]
> 프로젝트 템플릿 및 빌드 지원을 함께 제공하는 C++/WinRT Visual Studio 확장(VSIX) 및 NuGet 패키지를 설치하고 사용하는 방법에 대한 자세한 내용은 [Visual Studio의 C++/WinRT 지원](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)을 참조하세요.

Microsoft Visual Studio에서 새 C++/WinRT 프로젝트를 만들어 이식 프로세스를 시작합니다. **비어 있는 앱(C++/WinRT)** 프로젝트 템플릿을 사용하여 새 프로젝트를 만듭니다. 해당 이름을 *Clipboard*로 설정하고 폴더 구조가 연습과 일치하도록 **솔루션 및 프로젝트를 같은 디렉터리에 배치**를 선택 취소합니다.

기준을 얻기 위해 이 새로운 비어 있는 프로젝트를 빌드하고 실행합니다.

## <a name="packageappxmanifest-and-asset-files"></a>Package.appxmanifest 및 자산 파일

샘플의 C# 및 C++/WinRT 버전을 동일한 컴퓨터에 나란히 설치할 필요가 없는 경우 두 프로젝트의 앱 패키지 매니페스트 소스 파일(`Package.appxmanifest`)이 같아도 됩니다. 이 경우 C# 프로젝트에서 C++/WinRT 프로젝트로 `Package.appxmanifest`를 복사하면 작업이 완료됩니다.

두 버전의 샘플이 공존하려면 서로 다른 식별자가 필요합니다. 이 경우 C++/WinRT 프로젝트에서 XML 편집기를 통해 `Package.appxmanifest` 파일을 열고 다음 3개의 값을 기록해 둡니다.

- **/Package/Identity** 요소 내에서 **Name** 특성의 값을 기록해 둡니다. *패키지 이름*입니다. 새로 만든 프로젝트의 경우 프로젝트에서 고유 GUID의 초기 값을 지정합니다.
- **/Package/Applications/Application** 요소 내에서 **Id** 특성의 값을 기록해 둡니다. *애플리케이션 ID*입니다.
- **/Package/mp:PhoneIdentity** 요소 내에서 **PhoneProductId** 특성의 값을 기록해 둡니다. 마찬가지로 새로 만든 프로젝트의 경우 패키지 이름이 설정된 GUID와 동일하게 설정됩니다.

그런 다음, C# 프로젝트에서 C++/WinRT 프로젝트로 `Package.appxmanifest`를 복사합니다. 마지막으로, 기록한 3개의 값을 복원할 수 있습니다. 또는 새 프로젝트의 경우와 마찬가지로 복사한 값을 편집하여 애플리케이션 및 조직에 맞는 고유한 값으로 지정할 수 있습니다. 예를 들어 이 경우 패키지 이름 값을 복원하는 대신 복사된 값을 *Microsoft.SDKSamples.Clipboard.CS*에서 *Microsoft.SDKSamples.Clipboard.CppWinRT*로 변경하면 됩니다. 또한 애플리케이션 ID를 *App*으로 설정된 상태로 유지할 수 있습니다. 패키지 이름 또는 애플리케이션 ID가 다른 경우 두 애플리케이션의 애플리케이션 사용자 모델 ID(AUMID)가 다릅니다.

이 연습의 목적에 맞게 `Package.appxmanifest`를 추가로 몇 가지 변경하는 것이 좋습니다. *클립보드 C# 샘플* 문자열이 세 번 나옵니다. 이 문자열을 *클립보드 C++/WinRT 샘플*로 변경합니다.

C++/WinRT 프로젝트에서 `Package.appxmanifest` 파일 및 프로젝트는 현재 참조하는 자산 파일에 대해 동기화되지 않습니다. 이를 해결하려면 먼저 `Assets` 폴더(Visual Studio의 솔루션 탐색기)에서 모든 파일을 선택하고 제거하여(대화 상자에서 **삭제** 선택) C++/WinRT 프로젝트에서 자산을 제거합니다.

C# 프로젝트는 공유 폴더의 자산 파일을 참조합니다. C++/WinRT 프로젝트에서 동일한 작업을 수행하거나 이 연습에서 수행하는 것과 같은 방법으로 파일을 복사할 수 있습니다.

`\Clipboard_sample\SharedContent\media` 폴더로 이동합니다. C# 프로젝트에 포함된 7개의 파일(`windows-sdk.png`~`microsoft-sdk.png`)을 선택하고 복사한 다음, 새 프로젝트의 `\Clipboard\Clipboard\Assets` 폴더에 붙여 넣습니다.

`Assets` 폴더를 마우스 오른쪽 단추로 클릭하고(C++/WinRT 프로젝트의 솔루션 탐색기에서) > **추가** > **기존 항목...** 을 선택하고 `\Clipboard\Clipboard\Assets`로 이동합니다. 파일 선택에서 7개의 파일을 선택하고 **추가**를 클릭합니다.

`Package.appxmanifest`는 이제 프로젝트의 자산 파일과 다시 동기화됩니다.

## <a name="mainpage-including-the-functionality-that-configures-the-sample"></a>샘플 구성 기능을 포함하는 **MainPage**

모든 [UWP(유니버설 Windows 플랫폼) 앱 샘플](https://github.com/microsoft/Windows-universal-samples)과 같이 Clipboard 샘플은 사용자가 한 번에 하나씩 단계별로 실행할 수 있는 시나리오 컬렉션으로 구성되어 있습니다. 지정된 샘플의 시나리오 컬렉션은 샘플의 소스 코드에서 구성됩니다. 컬렉션의 각 시나리오는 시나리오를 구현하는 프로젝트의 클래스 형식 뿐만 아니라 제목을 저장하는 데이터 항목입니다.

C# 버전의 샘플에서 `SampleConfiguration.cs` 소스 코드 파일을 확인하는 경우 두 개의 클래스가 표시됩니다. 대부분의 구성 논리는 부분 클래스인 **MainPage** 클래스에 있습니다(이 클래스는 `MainPage.xaml`의 태그 및 `MainPage.xaml.cs`의 명령형 코드와 결합될 때 전체 클래스를 형성함). 이 소스 코드 파일의 다른 클래스는 **Title** 및 **ClassType** 속성이 있는 **Scenario**입니다.

다음 몇 가지 하위 섹션을 통해 **MainPage** 및 **Scenario**를 이식하는 방법을 살펴보겠습니다.

### <a name="idl-for-the-mainpage-type"></a>**MainPage** 형식에 대한 IDL

IDL(인터페이스 정의 언어)과 C++/WinRT를 사용하여 프로그래밍하는 경우 어떻게 유용한지 간략하게 설명하면서 이 섹션을 시작해 보겠습니다. IDL은 Windows 런타임 형식의 호출이 가능한 화면을 설명하는 일종의 소스 코드입니다. 형식에서 호출 가능한(또는 퍼블릭) 화면은 *프로젝션*되어 형식이 사용될 수 있도록 합니다. 형식에서 이 *프로젝션된* 부분은 형식의 실제 내부 구현(호출할 수 없으며 퍼블릭이 아님)과 대조됩니다. 이는 IDL에서 정의하는 프로젝션된 부분에 불과합니다.

`.idl` 파일 내에서 IDL 소스 코드를 작성한 후에는 IDL을 컴퓨터에서 읽을 수 있는 메타데이터 파일(Windows 메타데이터라고도 함)로 컴파일할 수 있습니다. 이러한 메타데이터 파일의 확장명은 `.winmd`이며, 다음과 같이 사용됩니다.

- `.winmd`는 구성 요소에서 Windows 런타임 형식을 설명할 수 있습니다. 애플리케이션 프로젝트에서 Windows 런타임 구성 요소(WRC)를 참조하는 경우 애플리케이션 프로젝트는 WRC에 속하는 Windows 메타데이터를 읽습니다(해당 메타데이터는 별도의 파일에 있을 수도 있고, WRC 자체와 동일한 파일로 패키지될 수도 있음). 따라서 애플리케이션 내에서 WRC의 형식을 사용할 수 있습니다.
- `.winmd`는 동일한 애플리케이션의 다른 부분에서 사용될 수 있도록 애플리케이션의 한 부분에서 Windows 런타임 형식을 설명할 수 있습니다. 예를 들어 동일한 앱의 XAML 페이지에서 사용하는 Windows 런타임 형식이 있습니다.
- Windows 런타임 형식(기본 제공 또는 타사)을 더 쉽게 사용할 수 있도록 C++/WinRT 빌드 시스템은 `.winmd` 파일을 사용하여 이러한 Windows 런타임 형식의 프로젝션된 부분을 나타내는 래퍼 형식을 생성합니다.
- 사용자 고유의 Windows 런타임 형식을 더 쉽게 구현할 수 있도록 하려면 C++/WinRT 빌드 시스템은 IDL을 `.winmd` 파일로 전환한 다음, 이를 사용하여 프로젝션의 래퍼를 생성하고, 구현을 기반으로 하는 스텁을 생성합니다(이러한 스텁은 이 항목의 뒷부분에서 더 자세히 설명).

C++/WinRT와 함께 사용하는 특정 버전의 IDL은 [Microsoft 인터페이스 정의 언어 3.0](/uwp/midl-3/intro)입니다. 이 섹션의 나머지 부분에서는 C# **MainPage** 형식에 대해 자세히 살펴보겠습니다. C++/WinRT **MainPage** 형식의 *프로젝션*에 있어야 하는 부분(즉, 호출 가능 또는 퍼블릭, 화면)과 해당 구현에 포함될 수 있는 부분을 결정합니다. IDL을 작성하게 되면(이후 섹션에서 수행할 예정) 그 안에서 호출 가능한 부분만 정의할 것이기 때문에 이러한 차이점은 중요합니다.

**MainPage** 형식을 함께 구현하는 C# 소스 코드 파일은 `MainPage.xaml`(복사하여 곧 이식함), `MainPage.xaml.cs` 및 `SampleConfiguration.cs`입니다.

C++/WinRT 버전에서는 비슷한 방식으로 **MainPage** 형식을 소스 코드 파일에서 고려합니다. `MainPage.xaml.cs`의 논리를 사용하고 대부분의 경우에서 `MainPage.h` 및 `MainPage.cpp`로 변환합니다. 또한 `SampleConfiguration.cs`의 논리에서는 `SampleConfiguration.h` 및 `SampleConfiguration.cpp`로 변환합니다.

C# UWP(유니버설 Windows 플랫폼) 애플리케이션의 클래스는 당연히 Windows 런타임 형식입니다. 그러나 C++/WinRT 애플리케이션에서 형식을 작성하는 경우 해당 형식이 Windows 런타임 형식인지, 아니면 일반 C++ 클래스/구조체/열거형인지 선택할 수 있습니다.

프로젝트의 모든 XAML 페이지는 Windows 런타임 형식이어야 하므로 **MainPage**는 Windows 런타임 형식이어야 합니다. C++/WinRT 프로젝트에서 **MainPage**는 이미 Windows 런타임 형식이므로 해당 측면을 변경할 필요가 없습니다. 특히 *런타임 클래스*입니다.

- 특정 형식의 런타임 클래스를 작성해야 하는지 여부에 대한 자세한 내용은 [C++/WinRT를 통한 API 작성](./author-apis.md) 항목을 참조하세요.
- C++/WinRT에서 런타임 클래스의 내부 구현과 그에 대한 프로젝션된(퍼블릭) 부분은 서로 다른 두 클래스 형식으로 존재합니다. 이러한 형식을 *구현 형식*과 *프로젝션된 형식*이라고 합니다. 위의 글머리 기호에서 언급된 항목과 [C++/WinRT를 통한 API 사용](./consume-apis.md)에서 자세히 알아볼 수 있습니다.
- 런타임 클래스와 IDL(`.idl` 파일) 간 연결에 대한 자세한 내용은 [XAML 컨트롤, C++/WinRT 속성에 바인딩](./binding-property.md) 항목을 읽고 작업을 진행합니다. 해당 항목에서는 새로운 런타임 클래스 작성 프로세스를 진행하며, 첫 번째 단계는 새 **Midl 파일(.idl)** 항목을 프로젝트에 추가하는 것입니다.

**MainPage**의 경우 C++/WinRT 프로젝트에 필요한 `MainPage.idl` 파일이 이미 있습니다. 프로젝트 템플릿에서 생성되기 때문입니다. 단, 이 연습의 뒷부분에서는 `.idl` 파일을 프로젝트에 더 추가합니다.

그러면 기존 `MainPage.idl` 파일에 추가해야 하는 IDL의 목록이 바로 표시됩니다. 그 전에는 수행할 작업과 수행하지 않을 작업에 대한 이유를 IDL에 포함해야 합니다.

`MainPage.idl`에서 선언해야 하는 **MainPage**의 멤버(**MainPage** 런타임 클래스의 일부가 됨)와 단순히 **MainPage** 구현 형식의 멤버가 될 수 있는 대상을 확인하기 위해 C# **MainPage** 클래스의 멤버 목록을 만들어 보겠습니다. `MainPage.xaml.cs` 및 `SampleConfiguration.cs`를 확인하여 이러한 멤버를 찾을 수 있습니다.

`protected` 및 `private` 필드와 메서드를 총 12개 찾습니다. 다음 `public` 멤버를 찾을 수 있습니다.

- 기본 생성자 `MainPage()`
- 정적 필드 **Current** 및 **FEATURE_NAME**
- 속성 **IsClipboardContentChangedEnabled** 및 **Scenarios**
- 메서드 **BuildClipboardFormatsOutputString**, **DisplayToast**, **EnableClipboardContentChangedNotifications** 및 **NotifyUser**

`MainPage.idl`에서 선언하기에 적합한 `public` 멤버입니다. 각 항목을 검토하고 **MainPage** 런타임 클래스에 포함해야 하는지 여부 또는 구현에만 포함될 수 있는지 여부를 확인하겠습니다.

- 기본 생성자 `MainPage()` XAML **Page**의 경우 IDL에서 기본 생성자를 선언하는 것이 일반적입니다. 이렇게 하면 XAML UI 프레임워크에서 해당 형식을 활성화할 수 있습니다.
- 정적 필드 **Current**는 개별 시나리오 XAML 페이지에서 **MainPage**의 애플리케이션 인스턴스에 액세스하는 데 사용됩니다. **Current**는 XAML 프레임워크와 상호 작용하는 데 사용되지 않고 컴파일 단위에서도 사용되지 않기 때문에 구현 형식의 멤버로만 예약할 수 있습니다. 이와 같은 경우 사용자 고유의 프로젝트를 사용하여 이 작업을 수행하도록 선택할 수 있습니다. 그러나 이 필드는 프로젝션된 형식의 인스턴스이며, IDL에서 선언하는 것이 논리적인 것 같습니다. 따라서 이 작업은 여기에서 수행하며, 이렇게 하면 코드가 약간 더 깔끔해 집니다.
- **MainPage** 형식 내에서 액세스할 수 있는 정적 **FEATURE_NAME** 필드의 경우와 비슷합니다. 또한 IDL에 선언하도록 선택하면 코드가 약간 더 간결해 집니다.
- **IsClipboardContentChangedEnabled** 속성은 **OtherScenarios** 클래스에서만 사용됩니다. 이제 이식 중에 약간의 작업을 단순화하고 **OtherScenarios** 런타임 클래스의 프라이빗 필드로 만듭니다. 따라서 IDL에 이동되지 않습니다.
- **Scenarios** 속성은 **Scenario** 형식(앞에서 언급한 형식)의 개체 컬렉션입니다. 다음 하위 섹션에서 **Scenario**에 대해 설명하겠습니다. 그때까지 **Scenario** 속성을 그대로 둡니다.
- **BuildClipboardFormatsOutputString**, **DisplayToast** 및 **EnableClipboardContentChangedNotifications** 메서드는 기본 페이지의 경우보다 샘플의 일반적인 상태에서 더 많은 작업을 수행하는 유틸리티 함수입니다. 따라서 이식 동안 이러한 세 가지 메서드를 **SampleState**라는 새 유틸리티 형식으로 리팩터링합니다(Windows 런타임 형식일 필요는 없음). 이러한 이유로 이러한 세 가지 메서드는 IDL로 이동하지 않습니다.
- **NotifyUser** 메서드는 정적 *Current* 필드에서 반환되는 **MainPage** 인스턴스의 개별 시나리오 XAML 페이지에서 호출됩니다. 이미 언급한 것처럼 **Current**는 프로젝션된 형식의 인스턴스이므로 IDL에서 **NotifyUser**를 선언해야 합니다. **NotifyUser**는 **NotifyType** 형식의 매개 변수를 사용합니다. 다음 하위 섹션에서 이에 대해 설명합니다.

`{x:Bind}` 또는 `{Binding}` 사용 여부에 관계없이 데이터 바인딩을 수행할 모든 멤버를 IDL에 선언해야 합니다. 자세한 내용은 [데이터 바인딩](../data-binding/index.md)을 참조하세요.

진행 중인 작업이 있습니다. `MainPage.idl` 파일에 추가할 멤버 목록과 추가하지 않을 멤버 목록을 개발하고 있습니다. 그러나 **Scenarios** 속성 및 **NotifyType** 형식에 대해 계속 논의해야 합니다. 그러면 다음을 수행하겠습니다.

### <a name="idl-for-the-scenario-and-notifytype-types"></a>**Scenario** 및 **NotifyType** 형식에 대한 IDL

**Scenario** 클래스는 `SampleConfiguration.cs`에 정의되어 있습니다. 해당 클래스를 C++/WinRT에 이식하는 방법을 결정해야 합니다. 기본적으로 일반적인 C++ `struct`를 만들 수 있습니다. 그러나 **Scenario**가 이진 파일에서 사용되거나 XAML 프레임워크와 상호 운용되는 경우 IDL에서 Windows 런타임 형식으로 선언해야 합니다.

C# 소스 코드를 살펴보고 이 컨텍스트에서 **Scenario**가 사용되는 것을 확인했습니다.

```xaml
<ListBox x:Name="ScenarioControl" ... >
```

```csharp
var itemCollection = new List<Scenario>();
int i = 1;
foreach (Scenario s in scenarios)
{
    itemCollection.Add(new Scenario { Title = $"{i++}) {s.Title}", ClassType = s.ClassType });
}
ScenarioControl.ItemsSource = itemCollection;
```

**Scenario** 개체의 컬렉션이 **ListBox**(항목 컨트롤)의 [**ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 속성에 할당됩니다. **Scenario**는 XAML과 상호 운용되어야 하므로 Windows 런타임 형식이어야 합니다. 따라서 IDL에 정의해야 합니다. IDL에 **Scenario** 형식을 정의하면 C++/WinRT 빌드 시스템은 백그라운드 헤더 파일에서 **Scenario**의 소스 코드 정의를 생성하게 됩니다(이 연습에서 중요하지 않은 이름 및 위치).

다시 말하지만 **MainPage.Scenarios**는 방금 IDL에 포함해야 한다고 설명한 **Scenario** 개체의 컬렉션입니다. 이러한 이유로 **MainPage.Scenarios**를 IDL에 선언해야 합니다.

**NotifyType**은 C#의 `MainPage.xaml.cs`에 선언된 `enum`입니다. **NotifyType**을 **MainPage** 런타임 클래스에 속하는 메서드에 전달하게 되므로 **NotifyType**은 Windows 런타임 형식이어야 하며 `MainPage.idl`에서 정의해야 합니다.

이제 `MainPage.idl` 파일에 IDL에서 선언하기로 결정한 **Mainpage**의 새 형식 및 새 멤버를 추가해 보겠습니다. 이와 동시에 Visual Studio 프로젝트 템플릿이 제공한 **Mainpage**의 자리 표시자 멤버를 IDL에서 제거합니다.

따라서 C++/WinRT 프로젝트에서 `MainPage.idl`을 열고 아래 목록과 같이 편집합니다. 편집 내용 중 하나는 **Clipboard**에서 **SDKTemplate**으로 네임스페이스 이름을 변경하는 것입니다. 원하는 경우 `MainPage.idl`의 전체 내용을 다음 코드로 바꾸기만 하면 됩니다. 또 다른 수정 사항은 **Scenario::ClassType**의 이름을 **Scenario::ClassName**으로 변경한다는 것입니다.

```idl
// MainPage.idl
namespace SDKTemplate
{
    struct Scenario
    {
        String Title;
        Windows.UI.Xaml.Interop.TypeName ClassName;
    };

    enum NotifyType
    {
        StatusMessage,
        ErrorMessage
    };

    [default_interface]
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();

        static MainPage Current{ get; };
        static String FEATURE_NAME{ get; };

        static Windows.Foundation.Collections.IVector<Scenario> scenarios{ get; };

        void NotifyUser(String strMessage, NotifyType type);
    };
}
```

> [!NOTE]
> C++/WinRT 프로젝트의 `.idl` 파일 내용에 대한 자세한 내용은 [Microsoft 인터페이스 정의 언어 3.0](/uwp/midl-3/)을 참조하세요.

사용자 고유의 이식 작업을 사용하는 경우 위에서 설명한 것처럼 네임스페이스 이름을 변경하려고 하지 않을 것이며 변경할 필요도 없습니다. 여기서는 이식하는 C# 프로젝트의 기본 네임스페이스가 **SDKTemplate**이지만 프로젝트 및 어셈블리의 이름은 **Clipboard**이기 때문에 이 작업을 수행합니다.

그러나 이 연습에서 이식을 계속 진행하게 되므로 소스 코드에서 나오는 모든 **Clipboard** 네임스페이스 이름을 **SDKTemplate**로 변경하게 됩니다. 또한 C++/WinRT 프로젝트 속성에도 **Clipboard** 네임스페이스 이름이 나타나므로 지금 변경할 수 있습니다.

Visual Studio에서 C++/WinRT 프로젝트에 대해 프로젝트 속성 **공용 속성** \> **C++/WinRT** \> **루트 네임스페이스**를 값 *SDKTemplate*로 설정합니다.

### <a name="save-the-idl-and-re-generate-stub-files"></a>IDL 저장 및 스텁 파일 다시 생성

[XAML 컨트롤, C++/WinRT 속성에 바인딩](./binding-property.md) 항목에서는 *스텁 파일*의 개념을 소개하고 작업에 대한 연습을 보여 줍니다. 스텁은 이 항목에서도 앞서 언급한 바 있습니다. C++/WinRT 빌드 시스템이 `.idl` 파일의 콘텐츠를 Windows 메타데이터로 변환하면 해당 메타데이터에서 `cppwinrt.exe`라는 도구가 구현 기반이 되는 스텁을 생성한다는 내용이었습니다.

IDL에서 항목을 추가, 제거 또는 변경하거나 빌드할 때마다 빌드 시스템은 이러한 스텁 파일에서 스텁 구현을 업데이트합니다. 따라서 IDL을 변경하고 빌드할 때마다 이러한 스텁 파일을 보고 변경된 서명을 복사하여 프로젝트에 붙여넣는 것이 좋습니다. 잠시 후에 이러한 작업을 수행하는 방법에 대한 자세한 내용과 예제를 살펴보겠습니다. 그러나 이에 대한 장점은 구현 형식의 모양과 해당 메서드의 서명에 대해 항상 알 수 있는 오류 없는 방법을 제공한다는 점입니다.

연습의 이 시점에서 `MainPage.idl` 파일 편집을 완료하게 되므로 지금 저장해야 합니다. 지금은 프로젝트가 완료 상태까지 빌드되지는 않지만 지금 빌드를 수행하면 **MainPage**의 스텁 파일이 다시 생성되므로 유용합니다.

이 C++/WinRT 프로젝트의 경우 스텁 파일은 `\Clipboard\Clipboard\Generated Files\sources` 폴더에 생성됩니다. 부분 빌드가 완료된 후에도 이러한 항목을 찾을 수 있습니다. 다시 말하지만 예상한 것처럼 빌드가 완전히 성공하는 것은 아닙니다. 그러나 여기서 중요한 단계는 스텁 생성이 생성할 것이라는 사실입니다. 중요한 파일은 `MainPage.h` 및 `MainPage.cpp`입니다.

이러한 두 스텁 파일에서는 IDL에 추가한 **MainPage**의 멤버(예: **Current** 및 **FEATURE_NAME**)에 대한 새로운 스텁 구현을 확인할 수 있습니다. 이러한 스텁 구현을 프로젝트에 이미 있는 `MainPage.h` 및 `MainPage.cpp` 파일에 복사합니다. 이와 동시에 IDL로 수행한 것처럼, Visual Studio 프로젝트 템플릿이 제공하는**Mainpage**의 자리 표시자 멤버(**MyProperty**라는 더미 속성 및 **ClickHandler**라는 이벤트 처리기)를 기존 파일에서 제거합니다.

실제로 유지하려는 현재 **MainPage** 버전의 유일한 멤버는 생성자입니다.

스텁 파일에서 새 멤버를 복사하고, 원치 않는 멤버를 삭제하고, 네임스페이스를 업데이트하면 프로젝트의 `MainPage.h` 및 `MainPage.cpp` 파일이 다음 코드 목록과 같이 표시됩니다. 두 가지 **MainPage** 형식이 있습니다. **implementation** 네임스페이스 형식과 **factory_implementation** 네임스페이스 형식입니다. **factory_implementation** 형식에 대한 유일한 변경 내용은 해당 네임스페이스에 **SDKTemplate**를 추가하는 것입니다.

```cppwinrt
// MainPage.h
#pragma once
#include "MainPage.g.h"

namespace winrt::SDKTemplate::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        static SDKTemplate::MainPage Current();
        static hstring FEATURE_NAME();
        static Windows::Foundation::Collections::IVector<SDKTemplate::Scenario> scenarios();
        void NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type);
    };
}
namespace winrt::SDKTemplate::factory_implementation
{
    struct MainPage : MainPageT<MainPage, implementation::MainPage>
    {
    };
}
```

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

namespace winrt::SDKTemplate::implementation
{
    MainPage::MainPage()
    {
        InitializeComponent();
    }
    SDKTemplate::MainPage MainPage::Current()
    {
        throw hresult_not_implemented();
    }
    hstring MainPage::FEATURE_NAME()
    {
        throw hresult_not_implemented();
    }
    Windows::Foundation::Collections::IVector<SDKTemplate::Scenario> MainPage::scenarios()
    {
        throw hresult_not_implemented();
    }
    void MainPage::NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type)
    {
        throw hresult_not_implemented();
    }
}
```

문자열의 경우 C#은 **System.String**을 사용합니다. 예제는 **MainPage.NotifyUser** 메서드를 참조하세요. 이 IDL에서는 **String**으로 문자열을 선언합니다. 그러면 `cppwinrt.exe` 도구는 C++/WinRT 코드를 생성할 때 [**WinRT::hstring**](/uw/cpp-ref-for-winrt/hstring) 형식을 사용합니다. C# 코드에서 문자열이 나올 때마다 **winrt::hstring**으로 이식합니다. 자세한 내용은 [C++/WinRT의 문자열 처리](./strings.md)를 참조하세요.

메서드 시그니처의 `const&` 매개 변수에 대한 설명은 [매개 변수-전달](./concurrency.md#parameter-passing)을 참조하세요.

### <a name="update-all-remaining-namespace-declarationsreferences-and-build"></a>나머지 모든 네임스페이스 선언/참조 업데이트 및 빌드

C++/WinRT 프로젝트를 빌드하기 전에 **Clipboard** 네임스페이스에 대한 선언 및 참조를 찾아 **SDKTemplate**로 변경합니다.

- `MainPage.xaml` 및 `App.xaml` 네임스페이스는 `x:Class` 및 `xmlns:local` 특성 값에 표시됩니다.
- `App.idl`을 차례로 클릭합니다.
- `App.h`을 차례로 클릭합니다.
- `App.cpp`을 차례로 클릭합니다. 두 개의 `using namespace` 지시문(부분 문자열 `using namespace Clipboard` 검색)과 두 개의 **MainPage** 형식 한정자(`Clipboard::MainPage` 검색)가 있습니다. 이러한 항목은 변경해야 합니다.

**MainPage**에서 이벤트 처리기를 제거했으므로 `MainPage.xaml`로 이동하여 태그에서 **Button** 요소를 삭제합니다.

모든 파일을 저장합니다. 솔루션을 정리하고(**빌드** > **Clipboard 정리**) 빌드합니다. 지금까지 변경한 내용이 유효한 경우 빌드가 성공적으로 수행됩니다.

### <a name="implement-the-mainpage-members-that-we-declared-in-idl"></a>IDL에 선언한 **MainPage** 멤버 구현

#### <a name="the-constructor-current-and-feature_name"></a>생성자, **Current** 및 **FEATURE_NAME**

다음은 이식해야 하는 C# 프로젝트의 관련 코드입니다.

```xaml
<!-- MainPage.xaml -->
...
<TextBlock x:Name="SampleTitle" ... />
...
```

```csharp
// MainPage.xaml.cs
...
public sealed partial class MainPage : Page
{
    public static MainPage Current;

    public MainPage()
    {
        InitializeComponent();
        Current = this;
        SampleTitle.Text = FEATURE_NAME;
    }
...
}
...

// SampleConfiguration.cs
...
public partial class MainPage : Page
{
    public const string FEATURE_NAME = "Clipboard C# sample";
...
}
...
```

곧 `MainPage.xaml`을 복사하여 전체적으로 다시 사용할 것입니다. 지금은 적절한 이름을 사용하여 **TextBlock** 요소를 C++/WinRT 프로젝트의 `MainPage.xaml`에 일시적으로 추가합니다.

**FEATURE_NAME**은 `SampleConfiguration.cs`에 정의된 **MainPage**의 정적 필드(C# `const` 필드는 기본적으로 동작이 정적임)입니다. C++/WinRT의 경우 (정적) 필드 대신, (정적) 읽기 전용 속성의 C++/WinRT 식으로 만듭니다. C++/WinRT에서 속성 getter를 표현하는 방법은 속성 값을 반환하고 매개 변수(접근자)를 사용하지 않는 함수로서 입니다. 따라서 C# **FEATURE_NAME** 정적 필드는 C++/WinRT **FEATURE_NAME** 정적 접근자 함수가 됩니다(이 경우 문자열 리터럴 반환).

그렇지만 C# 읽기 전용 속성을 이식한다면 동일한 작업을 수행하게 됩니다. C# 쓰기 가능 속성의 경우 C++/WinRT에서 속성 setter를 표현하는 방법은 속성 값을 매개 변수(변경자(mutator))로 사용하는 `void` 함수입니다. 두 경우 모두 C# 필드 또는 속성이 정적이면 C++/WinRT 접근자 및/또는 변경자(mutator)도 마찬가지입니다.

**Current**는 **MainPage**의 정적(상수 아님) 필드입니다. 다시, 읽기 전용 속성(C++/WinRT 식)으로 만들고, 다시 정적으로 만듭니다. **FEATURE_NAME**이 상수 이면 **Current**는 그렇지 않습니다. 따라서 C++/WinRT에서는 지원 필드가 필요하며, 접근자는 해당 필드를 반환합니다. 따라서 C++/WinRT 프로젝트에서는 **current**라는 프라이빗 정적 필드를 `MainPage.h`에서 선언하고, `MainPage.cpp`에서 **current**를 정의/초기화하며, **Current**라는 퍼블릭 정적 접근자 함수를 통해 액세스합니다.

생성자 자체가 몇 가지 할당을 수행하므로 이식이 간단해집니다.

C++/WinRT 프로젝트에서 이름이 `SampleConfiguration.cpp`인 새 **Visual C++**  > **코드** > **C++ 파일(.cpp)** 항목을 추가합니다.

아래 목록과 일치하도록 `MainPage.xaml`, `MainPage.h`, `MainPage.cpp` 및 `SampleConfiguration.cpp`를 편집합니다.

```xaml
<!-- MainPage.xaml -->
...
<StackPanel ...>
    <TextBlock x:Name="SampleTitle" />
</StackPanel>
...
```

```cppwinrt
// MainPage.h
...
namespace winrt::SDKTemplate::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
...
        static SDKTemplate::MainPage Current() { return current; }
...
    private:
        static SDKTemplate::MainPage current;
...
    };
...
}

// MainPage.cpp
...
namespace winrt::SDKTemplate::implementation
{
    SDKTemplate::MainPage MainPage::current{ nullptr };
...
    MainPage::MainPage()
    {
        InitializeComponent();
        MainPage::current = *this;
        SampleTitle().Text(FEATURE_NAME());
    }
...
}

// SampleConfiguration.cpp
#include "pch.h"
#include "MainPage.h"

using namespace winrt;
using namespace SDKTemplate;

hstring implementation::MainPage::FEATURE_NAME()
{
    return L"Clipboard C++/WinRT Sample";
}
```

또한 **MainPage::Current()** 및 **MainPage::FEATURE_NAME()** 메서드는 다른 위치에서 정의하므로 해당 `MainPage.cpp`에서 기존 함수 본문을 삭제해야 합니다.

여기에서 볼 수 있듯이 **MainPage::current**가 프로젝션된 형식인 **SDKTemplate::MainPage** 형식으로 선언됩니다. 구현 형식인 **SDKTemplate::implementation::MainPage** 형식이 아닙니다. 프로젝션된 형식은 XAML 상호 운용을 위해 프로젝트 내에서 또는 이진 파일 간에 사용되도록 디자인된 형식입니다. 구현 형식은 프로젝션된 형식에 노출한 기능을 구현하는 데 사용됩니다. **MainPage::current**(`MainPage.h`)의 선언이 구현 네임스페이스(**winrt::SDKTemplate::implementation**) 내에 나타나기 때문에, 정규화되지 않은 **MainPage**는 구현 형식을 참조했을 것입니다. 따라서 **MainPage::current**를 프로젝션된 형식 **winrt::SDKTemplate::MainPage**의 인스턴스로 지정하려고 한다는 사실을 명확히 하기 위해 **SDKTemplate::** 로 한정합니다.

생성자에는 설명을 나타낼 수 있는 `MainPage::current = *this;`와 관련된 몇 가지 지점이 있습니다.
- 구현 형식의 멤버 내에서 `this` 포인터를 사용하는 경우 `this` 포인터는 물론 구현 형식에 대한 포인터입니다.
- `this` 포인터를 해당하는 프로젝션된 형식으로 변환하려면 역참조합니다. 여기에 나와 있는 것처럼 IDL에서 구현 형식을 생성하는 경우 구현 형식에는 프로젝션된 형식으로 변환되는 변환 연산자가 있습니다. 할당이 여기에서 진행되는 것이 바로 이때문입니다.

해당 세부 정보에 대한 자세한 내용은 [구현 형식과 인터페이스의 인스턴스화 및 반환](./author-apis.md#instantiating-and-returning-implementation-types-and-interfaces)을 참조하세요.

또한 생성자에는 `SampleTitle().Text(FEATURE_NAME());`이 있습니다. `SampleTitle()` 파트는 XAML에 추가한 **TextBlock**을 반환하는 **SampleTitle**이라는 간단한 접근자 함수에 대한 호출입니다. XAML 요소에 대해 `x:Name`을 수행할 때마다 XAML 컴파일러는 요소에 대해 명명된 접근자를 생성합니다. `.Text(...)` 부분은 **SampleTitle** 접근자가 반환한 **TextBlock** 개체에서 **Text** 변경자(mutator) 함수를 호출합니다. 또한 `FEATURE_NAME()`은 정적 **MainPage::FEATURE_NAME** 접근자 함수를 호출하여 문자열 리터럴을 반환합니다. 해당 코드 줄은 *SampleTitle*이라는 **TextBlock**의 **Text** 속성을 설정합니다.

문자열은 Windows 런타임에서 와이드이므로 문자열 리터럴을 이식하기 위해 와이드 문자 인코딩 접두사 `L`을 접두사로 붙입니다. 따라서 예를 들어 "string literal"을 L "a string literal"로 변경합니다. 또한 [와이드 문자열 리터럴](/cpp/cpp/string-and-character-literals-cpp#wide-string-literals)을 참조하세요.

#### <a name="scenarios"></a>**시나리오**

이식해야 하는 관련 C# 코드는 다음과 같습니다.

```csharp
// MainPage.xaml.cs
...
public sealed partial class MainPage : Page
{
...
    public List<Scenario> Scenarios
    {
        get { return this.scenarios; }
    }
...
}
...

// SampleConfiguration.cs
...
public partial class MainPage : Page
{
...
    List<Scenario> scenarios = new List<Scenario>
    {
        new Scenario() { Title = "Copy and paste text", ClassType = typeof(CopyText) },
        new Scenario() { Title = "Copy and paste an image", ClassType = typeof(CopyImage) },
        new Scenario() { Title = "Copy and paste files", ClassType = typeof(CopyFiles) },
        new Scenario() { Title = "Other Clipboard operations", ClassType = typeof(OtherScenarios) }
    };
...
}
...
```

이전 조사에서 이 **Scenario** 개체의 컬렉션이 **ListBox**에 표시되는 것을 알 수 있습니다. C++/WinRT에는 항목 컨트롤의 **ItemsSource** 속성에 할당할 수 있는 컬렉션 종류가 제한됩니다. 이 컬렉션은 벡터 또는 observable 벡터여야 하고 해당 요소는 다음 중 하나여야 합니다.

- 런타임 클래스 또는
- [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)

**IInspectable**의 경우, 요소가 자체 런타임 클래스가 아닌 경우 이러한 요소는 [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) 간에 boxing 및 unboxing할 수 있는 종류여야 합니다. 즉, Windows 런타임 형식이어야 합니다([스칼라 값을 IInspectable로 boxing 및 unboxing](./boxing.md) 참조).

이 사례 연구에서는 **Scenario**를 런타임 클래스로 만들지 않았습니다. 그러나 여전히 적절한 옵션입니다. 또한 고유한 이식 작업에서 런타임 클래스가 분명히 적절하게 사용되는 경우가 있을 것입니다. 예를 들어, 요소 형식을 *observable*로 지정해야 하거나([XAML 컨트롤, C++/WinRT 속성 바인딩](./binding-property.md) 참조) 다른 이유로 요소에 메서드가 있어야 하는 경우 데이터 멤버의 세트 이상이 됩니다.

이 연습에서는 **Scenario** 형식에 대해 런타임 클래스를 사용하지 않을 것이므로  boxing을 고려해야 합니다. **Scenario**를 일반적인 C++ `struct`으로 만든 경우 boxing할 수 없습니다. 그러나 IDL에서 **Scenario**를 `struct`으로 선언했으므로 boxing할 수 *있습니다*.

**Scenario**를 미리 boxing하거나, **ItemsSource**에 할당할 때까지 대기하는 옵션 중 하나를 선택하고 Just-in-Time에 boxing합니다. 이러한 두 옵션에 대한 몇 가지 고려 사항은 다음과 같습니다.

- 미리 Boxing. 이 옵션의 경우 데이터 멤버는 UI에 할당할 준비가 완료된 **IInspectable**의 컬렉션입니다. 초기화할 때 **Scenario** 개체를 해당 데이터 멤버로 boxing합니다. 해당 컬렉션의 복사본은 하나만 필요하지만 필드를 읽어야 할 때마다 요소를 unboxing해야 합니다.
- Just-in-Time Boxing. 이 옵션의 경우 데이터 멤버는 **Scenario**의 컬렉션입니다. UI에 할당할 시간이 되면 데이터 멤버의 **Scenario** 개체를 **IInspectable**의 새 컬렉션으로 boxing합니다. Unboxing하지 않고 데이터 멤버로 요소 필드를 읽을 수 있지만 컬렉션의 복사본이 2개 필요합니다.

알 수 있는 것처럼, 이와 같은 소형 컬렉션의 경우 장단점이 있습니다. 따라서 이 사례 연구에서는 Just-in-Time 옵션을 사용합니다.

**scenarios** 멤버는 `SampleConfiguration.cs`에서 정의되고 초기화되는 **MainPage**의 필드입니다. 또한 **Scenarios**는 `MainPage.xaml.cs`에 정의되고 **scenarios** 필드를 간단히 반환하도록 구현된 **MainPage**의 읽기 전용 속성입니다. C++/WinRT 프로젝트에서도 유사한 작업을 수행하지만, 두 멤버를 정적으로 지정합니다(애플리케이션에서는 하나의 인스턴스만 필요하므로 클래스 인스턴스 없이 액세스할 수 있음). 또한 두 멤버의 이름을 각각 *scenariosInner* 및 *scenarios*로 지정합니다. `MainPage.h`에서 *scenariosInner*를 선언합니다. 스토리지 기간이 정적이므로 `.cpp` 파일(이 경우 `SampleConfiguration.cpp`)에서 정의/초기화합니다.

아래 목록과 일치하도록 `MainPage.h` 및 `SampleConfiguration.cpp`를 편집합니다.

```cppwinrt
// MainPage.h
...
struct MainPage : MainPageT<MainPage>
{
...
    static Windows::Foundation::Collections::IVector<Scenario> scenarios() { return scenariosInner; }
...
private:
    static winrt::Windows::Foundation::Collections::IVector<Scenario> scenariosInner;
...
};

// SampleConfiguration.cpp
...
using namespace Windows::Foundation::Collections;
...
IVector<Scenario> implementation::MainPage::scenariosInner = winrt::single_threaded_observable_vector<Scenario>(
{
    Scenario{ L"Copy and paste text", xaml_typename<SDKTemplate::CopyText>() },
    Scenario{ L"Copy and paste an image", xaml_typename<SDKTemplate::CopyImage>() },
    Scenario{ L"Copy and paste files", xaml_typename<SDKTemplate::CopyFiles>() },
    Scenario{ L"History and roaming", xaml_typename<SDKTemplate::HistoryAndRoaming>() },
    Scenario{ L"Other Clipboard operations", xaml_typename<SDKTemplate::OtherScenarios>() },
});
```

또한 이제 헤더 파일에 해당 메서드를 정의하기 때문에 **MainPage::scenarios()** 에 대한 `MainPage.cpp`에서 기존 함수 본문을 삭제해야 합니다.

알고 있는 것처럼 `SampleConfiguration.cpp`에서 [winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)라는 C++/WinRT 도우미 함수를 호출하여 정적 데이터 멤버 *scenariosInner*를 초기화합니다. 이 함수는 새 Windows 런타임 컬렉션 개체를 만들고 [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_) 인터페이스로 반환합니다. 이 샘플에서 컬렉션은 *observable*이 아니므로(초기화 후 요소를 추가 및 제거하지 않기 때문에 필요하지 않음), 대신 [winrt::single_threaded_vector](/uwp/cpp-ref-for-winrt/single-threaded-vector)를 호출하도록 옵트인(opt in)할 수 있었습니다. 이 함수는 컬렉션을 [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) 인터페이스로 반환합니다.

컬렉션에 대한 자세한 내용과 바인딩하는 방법은 [XAML 항목 컨트롤(C++/WinRT 컬렉션에 바인딩)](./binding-collection.md) 및 [C++/WinRT로 작성된 컬렉션](./collections.md)을 참조하세요.

방금 추가한 초기화 코드는 프로젝트에 아직 없는 형식(예: **winrt::SDKTemplate::CopyText**)을 참조합니다. 이를 해결하려면 계속 작업하면서 프로젝트에 비어 있는 5개의 XAML 페이지를 추가해 보겠습니다.

#### <a name="add-five-new-blank-xaml-pages"></a>5개의 비어 있는 새 XAML 페이지 추가

프로젝트에 새 **XAML** > **빈 페이지(C++/WinRT)**  항목을 추가합니다(**빈 페이지**가 아니라 **빈 페이지(C++/WinRT)** 항목 템플릿이어야 함). 이름을  `CopyText`로 지정합니다. 새 XAML 페이지는 우리가 원하는 **SDKTemplate** 네임스페이스 내에서 정의됩니다.

위의 프로세스를 4번 더 반복하고 XAML 페이지 이름을 `CopyImage`, `CopyFiles`, `HistoryAndRoaming` 및 `OtherScenarios`로 지정합니다.

이제 원할 경우 다시 빌드할 수 있습니다.

#### <a name="notifyuser"></a>**NotifyUser**

C# 프로젝트에서는 `MainPage.xaml.cs`에서 **MainPage.NotifyUser** 메서드의 구현을 찾을 수 있습니다. **MainPage.NotifyUser**는 **MainPage.UpdateStatus**에 대한 종속성을 가지며, 이 메서드는 아직 이식하지 않은 XAML 요소에 대해 종속성을 갖습니다. 이제 C++/WinRT 프로젝트에서 **UpdateStatus** 메서드를 제거하고 나중에 이식합니다.

이식해야 하는 관련 C# 코드는 다음과 같습니다.

```csharp
// MainPage.xaml.cs
...
public void NotifyUser(string strMessage, NotifyType type)
if (Dispatcher.HasThreadAccess)
{
    UpdateStatus(strMessage, type);
}
else
{
    var task = Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => UpdateStatus(strMessage, type));
}
private void UpdateStatus(string strMessage, NotifyType type) { ... }{
...
```

**NotifyUser**는 [**Windows.UI.Core.CoreDispatcherPriority**](/uwp/api/windows.ui.core.coredispatcherpriority) 열거형을 사용합니다. C++/WinRT에서는 Windows 네임스페이스의 형식을 사용하려고 할 때마다 해당 C++/WinRT 네임스페이스 헤더 파일을 포함해야 합니다(이에 대한 자세한 내용은 [C++/WinRT 시작](./get-started.md) 참조). 이 경우 아래 코드 목록에서 볼 수 있듯이 헤더가 `winrt/Windows.UI.Core.h`가 되며 `pch.h`에 포함할 것입니다.

**UpdateStatus**는 프라이빗입니다. 따라서 이 메서드를 **MainPage** 구현 형식에 대한 프라이빗 메서드로 만들 것입니다. **UpdateStatus**는 런타임 클래스에서 호출되지 않으므로 IDL에서 선언하지 않습니다.

**MainPage.NotifyUser**를 이식하고 **MainPage.UpdateStatus**를 제거한 후에 C++/WinRT 프로젝트에서 얻게 되는 결과입니다. 이 코드 목록 다음에 몇 가지 세부 정보를 살펴보겠습니다.

```cppwinrt
// pch.h
...
#include <winrt/Windows.UI.Core.h>
...

// MainPage.h
...
struct MainPage : MainPageT<MainPage>
{
...
    void NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type);
...
private:
    void UpdateStatus(hstring const& strMessage, SDKTemplate::NotifyType const& type);
...
};

// MainPage.cpp
...
void MainPage::NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type)
{
    if (Dispatcher().HasThreadAccess())
    {
        UpdateStatus(strMessage, type);
    }
    else
    {
        Dispatcher().RunAsync(Windows::UI::Core::CoreDispatcherPriority::Normal, [strMessage, type, this]()
            {
                UpdateStatus(strMessage, type);
            });
    }
}
void MainPage::UpdateStatus(hstring const& strMessage, SDKTemplate::NotifyType const& type)
{
    throw hresult_not_implemented();
}
...
```

C#에서는 점 표기법을 사용하여 중첩된 속성에 *dot into*할 수 있습니다. 따라서 C# **MainPage** 형식은 `Dispatcher` 구문을 사용하여 자체 **Dispatcher** 속성에 액세스할 수 있습니다. 또한 C#은 `Dispatcher.HasThreadAccess`와 같은 구문을 사용하여 해당 값을 추가로 *dot into*할 수 있습니다. C++/WinRT에서 속성은 접근자 함수로 구현되므로 구문은 각 함수 호출에 대해 괄호를 추가한다는 측면에서만 다릅니다.

|C#|C++/WinRT|
|-|-|
|`Dispatcher.HasThreadAccess`|`Dispatcher().HasThreadAccess()`|

**NotifyUser**의 C# 버전이 [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync)를 호출하면 비동기 콜백 대리자를 람다 함수로 구현합니다. C++/WinRT 버전은 동일한 작업을 수행하지만 구문은 약간 다릅니다. C++/WinRT에서는 `this` 포인터(멤버 함수를 호출하려고 하므로) 뿐만 아니라 사용하려는 두 개의 매개 변수를 캡처합니다. 대리자를 람다로 구현하는 방법에 대한 자세한 내용 및 코드 예제는 [C++/WinRT의 대리자를 사용한 이벤트 처리](./handle-events.md) 항목을 참조하세요. 또한 이 특정 사례에서 `var task =` 부분은 무시해도 됩니다. 반환된 비동기 개체를 기다리지 않을 것이므로 저장할 필요가 없습니다. 

### <a name="implement-the-remaining-mainpage-members"></a>남은 **MainPage** 멤버 구현

지금까지 이식한 멤버와 이식할 멤버를 확인할 수 있도록 **MainPage**(`MainPage.xaml.cs` 및 `SampleConfiguration.cs`에서 구현) 멤버의 전체 목록을 만들어 보겠습니다.

|구성원|액세스|상태|
|-|-|-|
|**MainPage** 생성자|`public`|이식됨|
|**Current** 속성|`public`|이식됨|
|**FEATURE_NAME** 속성|`public`|이식됨|
|**IsClipboardContentChangedEnabled** 속성|`public`|시작 안 됨|
|**Scenarios** 속성|`public`|이식됨|
|**BuildClipboardFormatsOutputString** 메서드|`public`|시작 안 됨|
|**DisplayToast** 메서드|`public`|시작 안 됨|
|**EnableClipboardContentChangedNotifications** 메서드|`public`|시작 안 됨|
|**NotifyUser** 메서드|`public`|이식됨|
|**OnNavigatedTo** 메서드|`protected`|시작 안 됨|
|**isApplicationWindowActive** 필드|`private`|시작 안 됨|
|**needToPrintClipboardFormat** 필드|`private`|시작 안 됨|
|**scenarios** 필드|`private`|이식됨|
|**Button_Click** 메서드|`private`|시작 안 됨|
|**DisplayChangedFormats** 메서드|`private`|시작 안 됨|
|**Button_Click** 메서드|`private`|시작 안 됨|
|**HandleClipboardChanged** 메서드|`private`|시작 안 됨|
|**OnClipboardChanged** 메서드|`private`|시작 안 됨|
|**OnWindowActivated 된** 메서드|`private`|시작 안 됨|
|**ScenarioControl_SelectionChanged** 메서드|`private`|시작 안 됨|
|**UpdateStatus** 메서드|`private`|제거됨|

다음 몇 개의 하위 섹션에서 아직 이식되지 않은 멤버를 살펴보겠습니다.

> [!NOTE]
> 경우에 따라 소스 코드에서 XAML 태그의 UI 요소에 대한 참조를 볼 수 있습니다(`MainPage.xaml`). 이러한 참조를 보게 되면 XAML에 간단한 자리 표시자 요소를 추가하여 일시적으로 해결할 수 있습니다. 이렇게 하면 각 하위 섹션 후에 프로젝트가 계속 빌드됩니다. 다른 방법은 지금 C# 프로젝트에서 C++/WinRT 프로젝트로 `MainPage.xaml`의 전체 콘텐츠를  복사하여 참조를 해결하는 것입니다. 그러나 이 작업을 수행하는 경우 잠시 중지했다가 다시 빌드하기 위해 한참 대기해야 할 수 있습니다. 따라서 모든 오타나 기타 오류가 발생하는 경우를 방지할 수 있습니다.
>
> **MainPage** 클래스에 대한 명령형 코드의 이식이 끝나면 XAML 파일의 내용을 복사하고, 안심하고 프로젝트를 계속 빌드할 수 있습니다.

#### <a name="isclipboardcontentchangedenabled"></a>**IsClipboardContentChangedEnabled**

이 속성은 기본값이 `false`인 get-set C# 속성입니다. **MainPage**의 멤버로, `SampleConfiguration.cs`에 정의되어 있습니다.

C++/WinRT의 경우 필드로 접근자 함수, 변경자(mutator) 함수 및 지원 데이터 멤버가 필요합니다. **IsClipboardContentChangedEnabled**는 **MainPage** 자체의 상태가 아니라 샘플의 시나리오 중 하나의 상태를 나타내므로 **SampleState**라는 새 유틸리티 형식에 새 멤버를 만듭니다. `SampleConfiguration.cpp` 소스 코드 파일에서 구현하고 멤버를 `static`으로 지정합니다(애플리케이션에서는 하나의 인스턴스만 필요하므로 클래스 인스턴스 없이 액세스할 수 있음).

C++/WinRT 프로젝트에서 `SampleConfiguration.cpp`를 함께 사용하려면 이름이 `SampleConfiguration.h`인 새 **Visual C++**  > **코드** > **헤더 파일(.h)** 항목을 추가합니다. 아래 목록과 일치하도록 `SampleConfiguration.h` 및 `SampleConfiguration.cpp`를 편집합니다.

```cppwinrt
// SampleConfiguration.h
#pragma once 
#include "pch.h"

namespace winrt::SDKTemplate
{
    struct SampleState
    {
        static bool IsClipboardContentChangedEnabled();
        static void IsClipboardContentChangedEnabled(bool checked);
    private:
        static bool isClipboardContentChangedEnabled;
    };
}

// SampleConfiguration.cpp
...
#include "SampleConfiguration.h"
...
bool SampleState::isClipboardContentChangedEnabled = false;
...
bool SampleState::IsClipboardContentChangedEnabled()
{
    return isClipboardContentChangedEnabled;
}
void SampleState::IsClipboardContentChangedEnabled(bool checked)
{
    if (isClipboardContentChangedEnabled != checked)
    {
        isClipboardContentChangedEnabled = checked;
    }
}
```

다시 말하지만 `static` 스토리지(예: **SampleState::isClipboardContentChangedEnabled**)가 있는 필드는 애플리케이션에서 한번만 정의하면 되며 `.cpp` 파일(이 경우`SampleConfiguration.cpp`)에 정의하는 것이 좋습니다.

#### <a name="buildclipboardformatsoutputstring"></a>**BuildClipboardFormatsOutputString**

이 메서드는 **MainPage**의 퍼블릭 멤버로, `SampleConfiguration.cs`에 정의되어 있습니다.

```csharp
// SampleConfiguration.cs
...
public string BuildClipboardFormatsOutputString()
{
    DataPackageView clipboardContent = Windows.ApplicationModel.DataTransfer.Clipboard.GetContent();
    StringBuilder output = new StringBuilder();

    if (clipboardContent != null && clipboardContent.AvailableFormats.Count > 0)
    {
        output.Append("Available formats in the clipboard:");
        foreach (var format in clipboardContent.AvailableFormats)
        {
            output.Append(Environment.NewLine + " * " + format);
        }
    }
    else
    {
        output.Append("The clipboard is empty");
    }
    return output.ToString();
}
...
```

C++/WinRT에서는 **BuildClipboardFormatsOutputString**을 **SampleState**의 퍼블릭 정적 메서드로 만듭니다. 인스턴스 멤버에 액세스하지 않기 때문에 `static`으로 지정할 수 있습니다.

C++/WinRT에서 **Clipboard** 및 **DataPackageView** 형식을 사용하려면 C++/WinRT Windows 네임스페이스 헤더 파일 `winrt/Windows.ApplicationModel.DataTransfer.h`를 포함해야 합니다.

C#에서 **DataPackageView.AvailableFormats** 속성은 **IReadOnlyList**이므로 해당 **Count** 속성에 액세스할 수 있습니다. C++/WinRT에서 **DataPackageView::AvailableFormats** 접근자 함수는 호출할 수 있는 **Size** 접근자 함수를 포함하는 **IVectorView**를 반환합니다.

C# **System.Text.StringBuilder** 형식의 사용을 이식하려면 표준 C++ 형식 [**std::wostringstream**](/cpp/standard-library/sstream-typedefs#wostringstream)을 사용합니다. 해당 형식은 와이드 문자열의 출력 스트림으로, 사용하려면 `sstream` 헤더 파일을 포함해야 합니다. **StringBuilder**를 사용할 때와 같이 **Append** 메서드를 사용하는 대신, **wostringstream**과 같은 출력 스트림에 [삽입 연산자](/cpp/standard-library/using-insertion-operators-and-controlling-format)(`<<`)를 사용합니다. 자세한 내용은 [iostream 프로그래밍](/cpp/standard-library/iostream-programming) 및 [ 문자열 서식 지정](./strings.md#formatting-strings)을 참조하세요.

C# 코드는 `new` 키워드를 사용하여 **StringBuilder**를 생성합니다. C#에서 개체는 기본적으로 `new`를 사용하여 힙에 선언된 참조 형식입니다. 최신 표준 C++에서 개체는 기본적으로 `new`를 사용하지 않고 스택에 선언된 값 형식입니다. 따라서 `StringBuilder output = new StringBuilder();`를 간단히 `std::wostringstream output;`으로 C++/WinRT로 이식합니다.

C# `var` 키워드는 형식을 유추하도록 컴파일러에 요청합니다. C++/WinRT에서 `var`을 `auto`로 이식할 수 있습니다. 그러나 C++/WinRT에서는 복사본을 방지하기 위해 *참조*를 유추된 형식 또는 추론된 형식으로 설정하고 `auto&`를 사용하여 추론된 형식에 대한 lvalue 참조를 표현하는 경우가 있습니다. 또한 *lvalue* 또는 *rvalue*를 사용하여 초기화되었는지 여부에 관계없이 올바르게 바인딩되는 특별한 유형의 참조를 원하는 경우도 있습니다. 그리고 이는 `auto&&`를 사용하여 표현합니다. 아래의 이식된 코드에서 `for` 루프에 사용된 형태입니다. *lvalue* 및 *rvalue*에 대한 소개는 [값 범주 및 해당 참조](./cpp-value-categories.md)에서 확인하세요.

아래 목록과 일치하도록 `pch.h`, `SampleConfiguration.h` 및 `SampleConfiguration.cpp`를 편집합니다.

```cppwinrt
// pch.h
...
#include <sstream>
#include "winrt/Windows.ApplicationModel.DataTransfer.h"
...

// SampleConfiguration.h
...
static hstring BuildClipboardFormatsOutputString();
...

// SampleConfiguration.cpp
...
using namespace Windows::ApplicationModel::DataTransfer;
...
hstring SampleState::BuildClipboardFormatsOutputString()
{
    DataPackageView clipboardContent{ Clipboard::GetContent() };
    std::wostringstream output;

    if (clipboardContent && clipboardContent.AvailableFormats().Size() > 0)
    {
        output << L"Available formats in the clipboard:";
        for (auto&& format : clipboardContent.AvailableFormats())
        {
            output << std::endl << L" * " << std::wstring_view(format);
        }
    }
    else
    {
        output << L"The clipboard is empty";
    }

    return hstring{ output.str() };
}
```

> [!NOTE]
> `DataPackageView clipboardContent{ Clipboard::GetContent() };` 코드 줄의 구문은 `=` 기호 대신 특이하게 중괄호를 사용하여 *균일 초기화*라는 최신 표준 C++의 기능을 사용합니다. 이 구문을 사용하면 할당 대신 초기화가 발생합니다. 할당과 *비슷한*(실제로는 그렇지 않은) 구문 형태를 선호하는 경우 위의 구문을 동등한 `DataPackageView clipboardContent = Clipboard::GetContent();`로 바꿀 수 있습니다. 하지만 두 가지 초기화 표현 방식이 모두 코드에서 자주 사용될 가능성이 높으므로 두 구문에 익숙해지는 것이 좋습니다.

#### <a name="displaytoast"></a>**DisplayToast**

**DisplayToast**는 C# **MainPage** 클래스의 퍼블릭 정적 메서드로, `SampleConfiguration.cs`에 정의되어 있습니다. C++/WinRT에서는 **SampleState**의 퍼블릭 정적 메서드로 만듭니다.

이 메서드를 이식하는 것과 관련된 세부 정보 및 기술 대부분은 이미 살펴보았습니다. 유의해야 할 새로운 사항은 C# 축자 문자열 리터럴(`@`)을 표준 C++ [원시 문자열 리터럴](/cpp/cpp/string-and-character-literals-cpp#raw-string-literals-c11)(`LR`)로 이식하는 것입니다.

또한 C++/WinRT에서 [**ToastNotification**](/uwp/api/windows.ui.notifications.toastnotification) and [**XmlDocument**](/uwp/api/windows.data.xml.dom.xmldocument) 형식을 참조하는 경우 네임스페이스 이름으로 한정하거나 `SampleConfiguration.cpp`를 편집하고 다음 예제와 같은 `using namespace` 지시문을 추가할 수 있습니다.

```cppwinrt
using namespace Windows::UI::Notifications;
```

[**XmlDocument**](/uwp/api/windows.data.xml.dom.xmldocument) 형식을 참조할 때와 다른 Windows 런타임 형식을 참조할 때마다 동일하게 선택합니다.

이러한 항목 외에도 이전에 수행한 것과 같은 지침에 따라 다음 단계를 수행합니다.

- `SampleConfiguration.h`에서 메서드를 선언하고 `SampleConfiguration.cpp`에서 정의합니다.
- 필요한 C++/WinRT Windows 네임스페이스 헤더 파일을 포함하도록 `pch.h`를 편집합니다.
- 힙이 아닌 스택에 C++/WinRT 개체를 생성합니다.
- 속성 get 접근자에 대한 호출을 함수 호출 구문(`()`)으로 바꿉니다.

컴파일러/링커 오류의 가장 일반적인 원인은 필요한 C++/WinRT Windows 네임스페이스 헤더 파일을 포함하지 않는 경우입니다. 한 가지 가능한 오류에 대한 자세한 내용은 [링커에서 “LNK2019: 확인되지 않은 외부 기호” 오류가 발생하는 이유는 무엇인가요?](./faq.md#why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error)를 참조하세요.

연습 과정을 진행하면서 **DisplayToast**를 직접 이식하려는 경우, 그 결과를 다운로드한 [클립보드 샘플](/samples/microsoft/windows-universal-samples/clipboard/) 소스 코드의 ZIP에서 C++/WinRT 버전의 코드와 비교할 수 있습니다.

#### <a name="enableclipboardcontentchangednotifications"></a>**EnableClipboardContentChangedNotifications**

**EnableClipboardContentChangedNotifications**는 C# **MainPage** 클래스의 퍼블릭 정적 메서드로, `SampleConfiguration.cs`에 정의되어 있습니다.

```csharp
// SampleConfiguration.cs
...
public bool EnableClipboardContentChangedNotifications(bool enable)
{
    if (IsClipboardContentChangedEnabled == enable)
    {
        return false;
    }

    IsClipboardContentChangedEnabled = enable;
    if (enable)
    {
        Clipboard.ContentChanged += OnClipboardChanged;
        Window.Current.Activated += OnWindowActivated;
    }
    else
    {
        Clipboard.ContentChanged -= OnClipboardChanged;
        Window.Current.Activated -= OnWindowActivated;
    }
    return true;
}
...
private void OnClipboardChanged(object sender, object e) { ... }
private void OnWindowActivated(object sender, WindowActivatedEventArgs e) { ... }
...
```

C++/WinRT에서는 **SampleState**의 퍼블릭 정적 메서드로 만듭니다.

C#에서는 `+=` 및 `-=` 연산자 구문을 사용하여 이벤트 처리 대리자를 등록 및 해지할 수 있습니다. C++/WinRT에는 [C++/WinRT의 대리자를 사용한 이벤트 처리](./handle-events.md)에 설명된 대로 대리자를 등록/해지할 수 있는 몇 가지 구문 옵션이 제공됩니다. 그러나 일반적으로는 이벤트에 대해 명명된 함수 쌍을 호출하여 등록 및 해지합니다. 등록하려면 대리자를 등록 함수에 전달하고 반환 결과에서 해지 토큰을 검색합니다([**winrt::event_token**](/uwp/cpp-ref-for-winrt/event-token)). 해지하려면 해당 토큰을 해지 함수에 전달합니다. 이 경우 처리기는 정적이며(다음 코드 목록에서 볼 수 있음) 함수 호출 구문은 간단합니다.

C# 내부에서는 실질적으로 유사한 토큰이 *사용됩니다*. 하지만 이 언어는 해당 세부 정보를 암시적으로 만듭니다. C++/WinRT는 이를 명시적으로 만듭니다.

C# 이벤트 처리기 시그니처에 **object** 형식이 표시됩니다. C# 언어에서 **object**는 .NET [**System.Object**](/dotnet/api/system.object) 형식에 대한 [별칭](/dotnet/csharp/language-reference/builtin-types/reference-types)입니다. C++/WinRT의 동급 항목은 [**winrt::Windows::Foundation::IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable)입니다. 따라서 C++/WinRT 이벤트 처리기에서는 **IInspectable**을 볼 수 있습니다.

아래 목록과 일치하도록 `SampleConfiguration.h` 및 `SampleConfiguration.cpp`를 편집합니다.

```cppwinrt
// SampleConfiguration.h
...
private:
    static event_token clipboardContentChangedToken;
    static event_token activatedToken;
    static void OnClipboardChanged(Windows::Foundation::IInspectable const& sender, Windows::Foundation::IInspectable const& e);
    static void OnWindowActivated(Windows::Foundation::IInspectable const& sender, Windows::UI::Core::WindowActivatedEventArgs const& e);
...

// SampleConfiguration.cpp
...
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml;
...
event_token SampleState::clipboardContentChangedToken;
event_token SampleState::activatedToken;
...
bool SampleState::EnableClipboardContentChangedNotifications(bool enable)
{
    if (isClipboardContentChangedEnabled == enable)
    {
        return false;
    }

    IsClipboardContentChangedEnabled(enable);
    if (enable)
    {
        clipboardContentChangedToken = Clipboard::ContentChanged(OnClipboardChanged);
        activatedToken = Window::Current().Activated(OnWindowActivated);
    }
    else
    {
        Clipboard::ContentChanged(clipboardContentChangedToken);
        Window::Current().Activated(activatedToken);
    }
    return true;
}
void SampleState::OnClipboardChanged(IInspectable const&, IInspectable const&){}
void SampleState::OnWindowActivated(IInspectable const&, WindowActivatedEventArgs const& e){}
```

지금은 이벤트 처리 대리자 자체(**OnClipboardChanged** 및 **OnWindowActivated**)를 스텁으로 둡니다. 이러한 대리자는 이식할 멤버 목록에 이미 있으므로 후속 하위 섹션에서 살펴보겠습니다.

#### <a name="onnavigatedto"></a>**OnNavigatedTo**

**OnNavigatedTo**는 C# **MainPage** 클래스의 보호된 메서드로, `MainPage.xaml.cs`에 정의되어 있습니다. 여기서는 참조하는 XAML **ListBox**와 함께 제공됩니다.

```xaml
<!-- MainPage.xaml -->
...
<ListBox x:Name="ScenarioControl" ... />
...
```

```csharp
// MainPage.xaml.cs
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    // Populate the scenario list from the SampleConfiguration.cs file
    var itemCollection = new List<Scenario>();
    int i = 1;
    foreach (Scenario s in scenarios)
    {
        itemCollection.Add(new Scenario { Title = $"{i++}) {s.Title}", ClassType = s.ClassType });
    }
    ScenarioControl.ItemsSource = itemCollection;

    if (Window.Current.Bounds.Width < 640)
    {
        ScenarioControl.SelectedIndex = -1;
    }
    else
    {
        ScenarioControl.SelectedIndex = 0;
    }
}
```

**Scenario** 개체 컬렉션이 UI에 할당되는 위치이므로 중요하고 흥미로운 메서드입니다. 이 C# 코드는 **Scenario** 개체의 [**System.Collections.Generic.List**](/dotnet/api/system.collections.generic.list-1)를 빌드한 후 **ListBox**(항목 컨트롤)의[**ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 속성에 할당합니다. 또한 C#에서는 [문자열 보간](/dotnet/csharp/language-reference/tokens/interpolated)을 사용하여 각 **Scenario** 개체의 제목을 작성합니다(`$` 특수 문자 사용).

C++/WinRT에서는 **OnNavigatedTo**를 **MainPage**의 퍼블릭 메서드로 만듭니다. 또한 빌드가 성공하도록 스텁 **ListBox** 요소를 XAML에 추가합니다. 코드 목록 다음에 몇 가지 세부 정보를 살펴보겠습니다.

```xaml
<!-- MainPage.xaml -->
...
<StackPanel ...>
    ...
    <ListBox x:Name="ScenarioControl" />
</StackPanel>
...
```

```cppwinrt
// MainPage.h
...
void OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
...

// MainPage.cpp
...
using namespace winrt::Windows::UI::Xaml;
using namespace winrt::Windows::UI::Xaml::Navigation;
...
void MainPage::OnNavigatedTo(NavigationEventArgs const& /* e */)
{
    auto itemCollection = winrt::single_threaded_observable_vector<IInspectable>();
    int i = 1;
    for (auto s : MainPage::scenarios())
    {
        s.Title = winrt::to_hstring(i++) + L") " + s.Title;
        itemCollection.Append(winrt::box_value(s));
    }
    ScenarioControl().ItemsSource(itemCollection);

    if (Window::Current().Bounds().Width < 640)
    {
        ScenarioControl().SelectedIndex(-1);
    }
    else
    {
        ScenarioControl().SelectedIndex(0);
    }
}
...
```

다시 [winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) 함수를 호출하지만 이번에는 [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)의 컬렉션을 만듭니다. 이 작업은 Just-in-Time 기준으로 **Scenario**의 boxing을 수행하기 위한 결정의 일부입니다.

그리고 여기에서는 C#의 [문자열 보간](/dotnet/csharp/language-reference/tokens/interpolated)을 사용하는 대신 [**to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) 함수와 **winrt::hstring**의 [연결 연산자](/uwp/cpp-ref-for-winrt/hstring#operator-concatenation-operator) 조합을 사용합니다.

#### <a name="isapplicationwindowactive"></a>**isApplicationWindowActive**

C#에서 **isApplicationWindowActive**는 **MainPage** 클래스에 속하는 간단한 프라이빗 `bool` 필드로, `SampleConfiguration.cs`에 정의되어 있습니다. 기본값은 `false`입니다. C++/WinRT에서는 동일한 기본값을 사용하여 `SampleConfiguration.h` 및 `SampleConfiguration.cpp` 파일에 **SampleState**(앞서 설명한 이유)의 퍼블릭 정적 필드로 만듭니다.

정적 필드를 선언, 정의 및 초기화하는 방법을 이미 살펴보았습니다. 리프레셔의 경우 **isClipboardContentChangedEnabled** 필드에서 수행한 작업을 다시 확인하고 **isApplicationWindowActive**로 동일한 작업을 수행합니다.

#### <a name="needtoprintclipboardformat"></a>**needToPrintClipboardFormat**

**isApplicationWindowActive**와 동일한 패턴입니다(이 항목 바로 앞의 머리글 참조).

#### <a name="button_click"></a>**Button_Click**

**Button_Click**은 C# **MainPage** 클래스의 프라이빗(이벤트 처리) 메서드이며 `MainPage.xaml.cs`에 정의되어 있습니다. 여기에는 이를 참조하는 XAML **SplitView**와 이를 등록하는 **ToggleButton**이 있습니다.

```xaml
<!-- MainPage.xaml -->
...
<SplitView x:Name="Splitter" ... />
...
<ToggleButton Click="Button_Click" .../>
...
```

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    Splitter.IsPaneOpen = !Splitter.IsPaneOpen;
}
```

또한 C++/WinRT로 이식되는 동급 항목이 있습니다. C++/WinRT 버전에서 이벤트 처리기는 `public`입니다. 여기서 볼 수 있듯이 `private:` 선언 전에 선언합니다. 왜냐하면 이와 같이 XAML 태그에 등록된 이벤트 처리기가 C++/WinRT에서 `public`이어야 XAML 태그가 액세스할 수 있기 때문입니다. 앞서 **MainPage::EnableClipboardContentChangedNotifications**에서 한 것처럼 명령형 코드에 이벤트 처리기를 등록하는 경우에는 이벤트 처리기가 `public`일 필요가 없습니다.

```xaml
<!-- MainPage.xaml -->
...
<StackPanel ...>
    ...
    <SplitView x:Name="Splitter" />
</StackPanel>
...
```

```cppwinrt
// MainPage.h
...
    void Button_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e);
private:
...

// MainPage.cpp
void MainPage::Button_Click(Windows::Foundation::IInspectable const& /* sender */, Windows::UI::Xaml::RoutedEventArgs const& /* e */)
{
    Splitter().IsPaneOpen(!Splitter().IsPaneOpen());
}
```

#### <a name="displaychangedformats"></a>**DisplayChangedFormats**

C#에서 **DisplayChangedFormats**는 **MainPage** 클래스에 속하는 프라이빗 메서드로, `SampleConfiguration.cs`에 정의되어 있습니다.

```csharp
private void DisplayChangedFormats()
{
    string output = "Clipboard content has changed!" + Environment.NewLine;
    output += BuildClipboardFormatsOutputString();
    NotifyUser(output, NotifyType.StatusMessage);
}
```

C++/WinRT에서는 `SampleConfiguration.h` 및 `SampleConfiguration.cpp` 파일에서 **SampleState**의 프라이빗 정적 필드로 만듭니다(인스턴스 멤버에 액세스하지 않음). 이 메서드에 대한 C# 코드는 **System.Text.StringBuilder**를 사용하지 않습니다. 그러나 C++/WinRT 버전에 대해 충분한 문자열 서식 지정을 수행하므로 **std::wostringstream**을 사용하는 데 좋은 또 다른 위치입니다.

C# 코드에 사용되는 정적 [**System.Environment.NewLine**](/dotnet/api/system.environment.newline) 속성 대신, 표준 C++ `std::endl`(줄 바꿈 문자)을 출력 스트림에 삽입합니다.

```cppwinrt
// SampleConfiguration.h
...
private:
    static void DisplayChangedFormats();
...

// SampleConfiguration.cpp
void SampleState::DisplayChangedFormats()
{
    std::wostringstream output;
    output << L"Clipboard content has changed!" << std::endl;
    output << BuildClipboardFormatsOutputString().c_str();
    MainPage::Current().NotifyUser(output.str(), NotifyType::StatusMessage);
}
```

위의 C++/WinRT 버전 디자인은 약간 비효율적입니다. 먼저 **std::wostringstream**을 만듭니다. 하지만 **BuildClipboardFormatsOutputString** 메서드(이전에 이식함)도 호출합니다. 해당 메서드는 자체 **std::wostringstream**을 만듭니다. 또한 해당 스트림을 **winrt::hstring**으로 전환한 후 반환합니다. [**hstring::c_str**](/uwp/cpp-ref-for-winrt/hstring#hstringc_str-function) 함수를 호출하여 반환된 **hstring**을 C 스타일 문자열로 전환한 후 스트림으로 삽입합니다. 메서드를 통해 직접 문자열을 삽입할 수 있도록 **std::wostringstream**을 하나만 만들고 주위에 참조를 전달하는 것이 더 효율적입니다.

이 작업은 [클립보드 샘플](/samples/microsoft/windows-universal-samples/clipboard/) 소스 코드의 C++/WinRT 버전(다운로드한 ZIP에 있음)에서 수행합니다. 해당 소스 코드에는 출력 스트림에 대한 참조를 사용하고 작동하는 **SampleState::AddClipboardFormatsOutputString**라는 새 프라이빗 정적 메서드가 있습니다. 그런 다음, **SampleState::DisplayChangedFormats** 및 **SampleState::BuildClipboardFormatsOutputString** 메서드가 해당 새 메서드를 호출하도록 리팩터링됩니다. 이 항목의 코드 목록과 기능적으로 동일하지만 좀 더 효율적입니다.

#### <a name="footer_click"></a>**Footer_Click**

**Footer_Click**은 C# **MainPage** 클래스에 속하는 비동기 이벤트 처리기이며 `MainPage.xaml.cs`에 정의되어 있습니다. 아래 코드 목록은 다운로드한 소스 코드의 메서드와 기능적으로 동일합니다. 하지만 여기서는 1줄에서 4줄로 압축이 해제되어 수행하는 작업을 보다 쉽게 확인할 수 있으며 결과적으로 이식 방법도 알 수 있습니다.

```csharp
async void Footer_Click(object sender, RoutedEventArgs e)
{
    var hyperlinkButton = (HyperlinkButton)sender;
    string tagUrl = hyperlinkButton.Tag.ToString();
    Uri uri = new Uri(tagUrl);
    await Windows.System.Launcher.LaunchUriAsync(uri);
}
```

기술적으로 이 메서드는 비동기적이지만 `await` 후에는 아무 작업도 수행하지 않으므로 `await`와 `async` 키워드가 둘 다 필요하지 않습니다. Visual Studio에서 IntelliSense 메시지를 방지하기 위해 이 메서드를 사용할 수 있습니다.

해당 C++/WinRT 메서드도 [**Launcher.LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync)를 호출하기 때문에 비동기입니다. 그러나 `co_await`를 수행할 필요가 없고 비동기 개체를 반환할 필요도 없습니다. `co_await`에 대한 자세한 내용과 비동기 개체에 대해서는 [C++/WinRT를 통한 동시성 및 비동기 작업](./concurrency.md)을 참조하세요.

이제 메서드가 수행하는 작업을 알아보겠습니다. 이것은 **HyperlinkButton**의 **Click** 이벤트에 대한 이벤트 처리기이므로 *sender*라는 개체는 실제로 **HyperlinkButton**입니다. 이러한 형식 변환은 안전합니다(또는 이 변환을 `sender as HyperlinkButton`으로 표현할 수도 있음). 다음으로 **Tag** 속성의 값을 검색합니다. C# 프로젝트의 XAML 태그를 살펴보면 웹 URL을 나타내는 문자열로 설정된 것을 볼 수 있습니다. **FrameworkElement.Tag** 속성(**HyperlinkButton**은 **FrameworkElement**임) **object** 형식이지만 C#에서는 [**Object.ToString**](/dotnet/api/system.object.tostring)으로 stringify를 수행할 수 있습니다. 결과 문자열에서 **Uri** 개체를 생성합니다. 마지막으로 셸을 통해 브라우저를 시작하고 URL로 이동합니다.

다음은 C++/WinRT로 이식된 메서드(명확히 나타내기 위해 확장됨)와 세부 정보에 대한 설명입니다.

```cppwinrt
// pch.h
...
#include "winrt/Windows.System.h"
...

// MainPage.h
...
    void Footer_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e);
private:
...

// MainPage.cpp
...
using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::UI::Xaml::Controls;
...
void MainPage::Footer_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const&)
{
    auto hyperlinkButton{ sender.as<HyperlinkButton>() };
    hstring tagUrl{ winrt::unbox_value<hstring>(hyperlinkButton.Tag()) };
    Uri uri{ tagUrl };
    Windows::System::Launcher::LaunchUriAsync(uri);
}
```

항상 그런 것처럼 이벤트 처리기를 `public`으로 지정합니다. *sender* 개체에서 [**as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) 함수를 사용하여 **HyperlinkButton**으로 변환합니다. C++/WinRT에서 **Tag** 속성은 [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)([**Object**](/dotnet/api/system.object)에 해당)입니다. 그러나 **IInspectable**에는 **Tostring**이 없습니다. 대신 **IInspectable**을 스칼라 값(이 경우 문자열)으로 unboxing해야 합니다. boxing 및 unboxing에 대한 자세한 내용은 [스칼라 값을 IInspectable로 boxing 및 unboxing](./boxing.md)을 참조하세요.

마지막 두 줄은 앞에서 살펴본 이식 패턴을 반복하여 C# 버전을 에코합니다.

#### <a name="handleclipboardchanged"></a>**HandleClipboardChanged**

이 메서드 이식과 관련된 새로운 항목은 없습니다. 다운로드한 [클립보드 샘플](/samples/microsoft/windows-universal-samples/clipboard/) 소스 코드의 ZIP에서 C# 및 C++/WinRT 버전을 비교할 수 있습니다.

#### <a name="onclipboardchanged-and-onwindowactivated"></a>**OnClipboardChanged** 및 **OnWindowActivated**

지금까지 이러한 두 이벤트 처리기에는 빈 스텁만 있습니다. 그러나 이러한 항목을 이식하는 것은 간단하며, 새롭게 다룰 내용은 없습니다.

#### <a name="scenariocontrol_selectionchanged"></a>**ScenarioControl_SelectionChanged**

C# **MainPage** 클래스에 속하고 `MainPage.xaml.cs`에 정의되는 또 다른 프라이빗 이벤트 처리기입니다. C++/WinRT에서는 이 항목을 퍼블릭으로 설정하고 `MainPage.h` 및 `MainPage.cpp`에서 구현합니다.

이 메서드의 경우 `false`로 초기화되는 프라이빗 부울 필드인 **MainPage::navigating**이 필요합니다. 또한 *ScenarioFrame*이라는 `MainPage.xaml`에 **Frame**이 필요합니다. 그러나 이러한 세부 정보를 제외하고 이 메서드 이식과 관련해서 새로운 기술은 없습니다.

#### <a name="updatestatus"></a>**UpdateStatus**

지금까지 **MainPage.UpdateStatus**에 대한 스텁만 있습니다. 구현을 다시 이식하는 것은 대체적으로 이전 내용을 기준으로 합니다. 한 가지 새로운 점은 C#에서는 **string**을 **String.Empty**와 비교할 수 있지만, C++/WinRT에서는 대신 [**winrt::hstring::empty**](/uwp/cpp-ref-for-winrt/hstring#hstringempty-function) 함수를 호출합니다. 또 다른 점은 표준 C++의 `nullptr`이 C#의 `null`에 해당한다는 것입니다.

이미 살펴본 기술을 사용하여 나머지 이식을 수행할 수 있습니다. 다음은 이 메서드의 이식된 버전이 컴파일되기 전에 수행해야 하는 작업의 목록입니다.

- `MainPage.xaml`에 *StatusBorder*라는 **Border**를 추가합니다.
- `MainPage.xaml`에 *StatusBlock*이라는 **TextBlock**을 추가합니다.
- `MainPage.xaml`에 *StatusPanel*이라는 **StackPanel**을 추가합니다.
- `pch.h`에 `#include "winrt/Windows.UI.Xaml.Media.h"`를 추가합니다.
- `pch.h`에 `#include "winrt/Windows.UI.Xaml.Automation.Peers.h"`를 추가합니다.
- `MainPage.cpp`에 `using namespace winrt::Windows::UI::Xaml::Media;`를 추가합니다.
- `MainPage.cpp`에 `using namespace winrt::Windows::UI::Xaml::Automation::Peers;`를 추가합니다.

### <a name="copy-the-xaml-and-styles-necessary-to-finish-up-porting-mainpage"></a>**MainPage** 이식을 완료하는 데 필요한 XAML 및 스타일을 복사합니다.

XAML의 경우에는 C# 및 C++/WinRT 프로젝트에서 *동일한*  XAML 태그를 사용할 수 있습니다. 또한 Clipboard 샘플은 이러한 경우 중 하나입니다.

`Styles.xaml` 파일에서 Clipboard 샘플에는 애플리케이션의 UI에서 단추, 메뉴 및 기타 UI 요소에 적용되는 스타일의 XAML **ResourceDictionary**가 있습니다. `Styles.xaml` 페이지가 `App.xaml`에 병합됩니다. 그러면 이미 간략히 살펴본 UI의 표준 `MainPage.xaml` 시작점이 표시됩니다. 이제 프로젝트의 C++/WinRT 버전에서 변경되지 않은 3개의 `.xaml` 파일을 다시 사용할 수 있습니다.

자산 파일과 마찬가지로 여러 버전의 애플리케이션에서 동일한 공유 XAML 파일을 참조하도록 선택할 수 있습니다. 이 연습에서는 간단하게 나타내기 위해 파일을 C++/WinRT 프로젝트에 복사하고 해당 방식으로 추가합니다.

`\Clipboard_sample\SharedContent\xaml` 폴더로 이동하고 `App.xaml` 및 `MainPage.xaml`을 선택하고 복사한 다음, 메시지가 표시될 때 파일을 바꾸도록 선택하고 C++/WinRT 프로젝트의 `\Clipboard\Clipboard` 폴더에 이 두 개의 파일을 붙여 넣습니다.

C++/WinRT 프로젝트에서 `Styles`라는 프로젝트 노드 바로 아래에 새 폴더를 추가합니다. `\Clipboard_sample\SharedContent\xaml` 폴더로 이동하고 `Styles.xaml`을 선택하고 복사한 다음, C++/WinRT 프로젝트의 `\Clipboard\Clipboard\Styles` 폴더에 붙여 넣습니다. `Styles` 폴더를 마우스 오른쪽 단추로 클릭하고(C++/WinRT 프로젝트의 솔루션 탐색기에서) > **추가** > **기존 항목...** 을 선택하고 `\Clipboard\Clipboard\Styles`로 이동합니다. 파일 선택에서 `Styles`를 선택하고 **추가**를 클릭합니다.

이제 **MainPage** 이식이 끝났으므로, 해당 단계를 진행하면 C++/WinRT 프로젝트가 빌드되고 실행됩니다.

## <a name="consolidate-your-idl-files"></a>`.idl` 파일 통합

Clipboard 샘플에는 UI의 표준 `MainPage.xaml` 시작점 외에 5가지 기타 시나리오 관련 XAML 페이지와 해당하는 코드 숨김 파일이 있습니다. 이러한 모든 페이지의 실제 XAML 태그를 C++/WinRT 버전의 프로젝트에 그대로 다시 사용할 것입니다. 그리고 다음에 나오는 몇 가지 주요 섹션에서 코드 숨김을 이식하는 방법을 살펴볼 것입니다. 하지만 그 전에 IDL에 대해 알아보겠습니다.

런타임 클래스를 단일 IDL 파일에 통합하면 가치를 얻을 수 있습니다([런타임 클래스를 Midl 파일(.idl)로 팩터링](./author-apis.md#factoring-runtime-classes-into-midl-files-idl) 참조). 그 다음에는, 이 IDL을 `Project.idl`이라는 단일 파일로 이동한 다음, 원래 파일을 삭제하여 `CopyFiles.idl`, `CopyImage.idl`, `CopyText.idl`, `HistoryAndRoaming.idl` 및 `OtherScenarios.idl`의 내용을 통합합니다.

이러한 각각의 5개 XAML 페이지 형식에서 자동으로 생성된 더미 속성(`Int32 MyProperty;` 및 구현)도 제거하겠습니다.

먼저 C++/WinRT 프로젝트에 새 **Midl 파일(.idl)** 항목을 추가합니다. 이름을 `Project.idl`이라고 지정합니다. `Project.idl`의 전체 내용을 다음 코드로 바꿉니다.

```idl
// Project.idl
namespace SDKTemplate
{
    [default_interface]
    runtimeclass CopyFiles : Windows.UI.Xaml.Controls.Page
    {
        CopyFiles();
    }

    [default_interface]
    runtimeclass CopyImage : Windows.UI.Xaml.Controls.Page
    {
        CopyImage();
    }

    [default_interface]
    runtimeclass CopyText : Windows.UI.Xaml.Controls.Page
    {
        CopyText();
    }

    [default_interface]
    runtimeclass HistoryAndRoaming : Windows.UI.Xaml.Controls.Page
    {
        HistoryAndRoaming();
    }

    [default_interface]
    runtimeclass OtherScenarios : Windows.UI.Xaml.Controls.Page
    {
        OtherScenarios();
    }
}
```

보시다시피, 이는 모두 하나의 네임스페이스 내에 있는 개별 `.idl` 파일의 내용에 대한 복사본일 뿐이며 각 런타임 클래스에서 `MyProperty`는 제거되었습니다.

Visual Studio의 솔루션 탐색기에서 모든 원래 IDL 파일(`CopyFiles.idl`, `CopyImage.idl`, `CopyText.idl`, `HistoryAndRoaming.idl` 및 `OtherScenarios.idl`)을 다중 선택하고 **편집** > **제거**를 선택합니다(대화 상자에서 **삭제** 선택).

마지막으로, 각 5개 XAML 페이지 형식의 `.h` 및 `.cpp` 파일에서 `MyProperty` 제거를 완료하기 위해 `int32_t MyProperty()` 접근자 및 `void MyProperty(int32_t)` 변경자 함수의 선언 및 정의를 삭제합니다.

그런데 XAML 파일의 이름이 해당 클래스의 이름과 일치하는 것이 항상 가장 좋습니다. 예를 들어 XAML 태그 파일에 `x:Class="MyNamespace.MyPage"`가 있는 경우 해당 파일의 이름을 `MyPage.xaml`로 지정해야 합니다. 이는 기술적 요구 사항은 아니지만 동일한 아티팩트에 여러 이름을 번갈아 사용하면 프로젝트를 더 쉽게 이해하고 유지 관리할 수 있으며, 프로젝트 작업도 더 수월해집니다.

## <a name="copyfiles"></a>**CopyFiles**

C# 프로젝트에서 **CopyFiles** XAML 페이지 형식은 `CopyFiles.xaml` 및 `CopyFiles.xaml.cs` 소스 코드 파일에 구현됩니다. **CopyFiles**의 각 구성원을 차례로 살펴보겠습니다.

### <a name="rootpage"></a>**rootPage**

프라이빗 필드입니다.

```csharp
// CopyFiles.xaml.cs
...
public sealed partial class CopyFiles : Page
{
    MainPage rootPage = MainPage.Current;
    ...
}
...
```

C++/WinRT에서는 이와 같이 정의하고 초기화할 수 있습니다.

```cppwinrt
// CopyFiles.h
...
struct CopyFiles : CopyFilesT<CopyFiles>
{
    ...
private:
    SDKTemplate::MainPage rootPage{ MainPage::Current() };
};
...
```

다시 말하지만, **MainPage::current**와 마찬가지로, **CopyFiles::rootPage**는 구현 형식이 아니라 프로젝션된 형식인 **SDKTemplate::MainPage** 형식으로 선언됩니다.

### <a name="copyfiles-the-constructor"></a>**CopyFiles**(생성자)

C++/WinRT 프로젝트에서 **CopyFiles** 형식에는 원하는 코드를 포함하는 생성자가 이미 있습니다(**InitializeComponent**만 호출).

### <a name="copybutton_click"></a>**CopyButton_Click**

C# **CopyButton_Click** 메서드는 이벤트 처리기이며, 해당 시그니처의 `async` 키워드를 통해 메서드가 비동기 작업을 수행한다는 것을 알 수 있습니다. C++/WinRT에서는 비동기 메서드를 *코루틴*으로 구현합니다. C++/WinRT의 동시성 소개와 *코루틴*에 대한 설명을 보려면 [C++/WinRT를 통한 동시성 및 비동기 작업](./concurrency.md)을 참조하세요.

코루틴이 완료된 후에 추가 작업을 예약하는 것이 일반적입니다. 이러한 경우 코루틴은 대기할 수 있는 몇 가지 비동기 개체 유형을 반환하고 필요에 따라 진행 상황을 보고합니다. 그러나 이러한 고려 사항은 일반적으로 이벤트 처리기에는 적용되지 않습니다. 따라서 비동기 작업을 수행하는 이벤트 처리기가 있는 경우 **winrt::fire_and_forget**을 반환하는 코루틴으로 구현할 수 있습니다. 자세한 내용은 [실행 후 망각](./concurrency-2.md#fire-and-forget)을 참조하세요.

실행 후 망각 코루틴의 개념은 언제 완료되든 신경쓰지 않는 것이며, 작업이 백그라운드에서 계속 실행되거나 일시 중단된 상태로 다시 시작되기를 기다립니다. C# 구현을 보면 **CopyButton_Click**은 `this` 포인터에 따라 달라지는 것을 알 수 있습니다(인스턴스 데이터 구성원 `rootPage`에 액세스함). 따라서 `this` 포인터(**CopyFiles** 개체에 대한 포인터)가 **CopyButton_Click** 코루틴보다 오래 지속되도록 해야 합니다. 사용자가 UI 페이지를 탐색하는 이 샘플 애플리케이션의 경우에는 해당 페이지의 수명을 직접 제어할 수 없습니다. **CopyButton_Click**이 아직 백그라운드 스레드에서 실행 중일 때 **CopyFiles** 페이지에서 나가서 이 페이지가 삭제될 경우 `rootPage`에 액세스하는 것은 안전하지 않습니다. 코루틴을 올바르게 만들려면 `this` 포인터에 대한 강한 참조를 가져오고 코루틴 기간 동안 해당 참조를 유지해야 합니다. 자세한 내용은 [C++/WinRT의 강한 참조 및 약한 참조](./weak-references.md)에서 확인하세요.

C++/WinRT 버전의 샘플에서 **CopyFiles::CopyButton_Click**을 보면 스택에 대한 단순한 선언과 함께 완료된 것을 볼 수 있습니다.

```cppwinrt
fire_and_forget CopyFiles::CopyButton_Click(IInspectable const&, RoutedEventArgs const&)
{
    auto lifetime{ get_strong() };
    ...
}
```

이식된 코드에서 주목할 만한 다른 요소를 살펴보겠습니다.

이 코드에서는 [**FileOpenPicker**](/uwp/api/windows.storage.pickers.fileopenpicker) 개체를 인스턴스화하고, 나중에 해당 개체의 [**FileTypeFilter**](/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter) 속성에 액세스합니다. 해당 속성의 반환 형식은 문자열의 **IVector**를 구현합니다. 그리고 여기서는 이 **IVector**에서 [IVector<T>.ReplaceAll(T[])](/uwp/api/windows.foundation.collections.ivector-1.replaceall) 메서드를 호출합니다. 흥미로운 점은 해당 메서드에 전달하는 값입니다. 여기서는 배열이 필요합니다. 코드 줄은 다음과 같습니다.

```cppwinrt
filePicker.FileTypeFilter().ReplaceAll({ L"*" });
```

여기서 전달하는 값(`{ L"*" }`)은 표준 C++ *이니셜라이저 목록*입니다. 이 경우 단일 개체를 포함하지만, 이니셜라이저 목록에는 쉼표로 구분된 여러 개체가 포함될 수 있습니다. 이와 같은 메서드에 이니셜라이저 목록을 편리하게 전달할 수 있게 해주는 C++/WinRT 요소는 [표준 이니셜라이저 목록](./std-cpp-data-types.md#standard-initializer-lists)에 설명되어 있습니다.

C++/WinRT에서 `co_await`에 C# `await` 키워드를 이식합니다. 이 코드의 예제는 다음과 같습니다.

```cppwinrt
auto storageItems{ co_await filePicker.PickMultipleFilesAsync() };
```

다음으로, 이 C# 코드 줄을 살펴보세요.

```csharp
dataPackage.SetStorageItems(storageItems);
```

C#은 *storageItems*로 표현된 **IReadOnlyList<StorageFile>** 를 [**DataPackage.SetStorageItems**](/uwp/api/windows.applicationmodel.datatransfer.datapackage.setstorageitems)에 필요한 **IEnumerable<IStorageItem>** 로 암시적으로 변환할 수 있습니다. 그러나 C++/WinRT에서는 **IVectorView<StorageFile>** 에서 **IIterable<IStorageItem>** 로 명시적으로 변환해야 합니다. 그리고 [**as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) 함수의 또 다른 예가 나와 있습니다.

```cppwinrt
dataPackage.SetStorageItems(storageItems.as<IVectorView<IStorageItem>>());
```

여기서는 C#에 `null` 키워드(예: `Clipboard.SetContentWithOptions(dataPackage, null)`)를 사용하고, C++/WinRT에 `nullptr`(예: `Clipboard::SetContentWithOptions(dataPackage, nullptr)`)을 사용합니다.

### <a name="pastebutton_click"></a>**PasteButton_Click**

이는 실행 후 망각 코루틴 형식의 또 다른 이벤트 처리기입니다. 이식된 코드에서 주목할 만한 요소를 살펴보겠습니다.

C# 버전의 샘플에서는 `catch (Exception ex)`를 사용하여 예외를 catch합니다. 이식된 C++/WinRT 코드에서 `catch (winrt::hresult_error const& ex)` 식을 볼 수 있습니다. [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)와 이를 사용하는 방법에 대한 자세한 내용은 [C++/WinRT를 통한 오류 처리](./error-handling.md)를 참조하세요.

C# 개체가 `null`인지, 아니면 `if (storageItems != null)`인지 테스트하는 예입니다. C++/WinRT에서는 `nullptr`에 대한 내부 테스트를 수행하는 `bool`에 대한 변환 연산자를 사용할 수 있습니다.

이식된 C++/WinRT 버전의 샘플에서 코드 조각을 약간 간소화한 버전은 다음과 같습니다.

```cppwinrt
std::wostringstream output;
output << std::wstring_view(ApplicationData::Current().LocalFolder().Path());
```

이와 같이 **winrt::hstring**으로 **std::wstring_view**를 구성하는 방법은 **winrt::hstring**을 C 스타일의 문자열로 변환하는 [**hstring::c_str**](/uwp/cpp-ref-for-winrt/hstring#hstringc_str-function) 함수를 호출하지 않는 대안을 보여줍니다. 이 대안은 **hstring**의 [**std::wstring_view**에 대한 변환 연산자](/uwp/cpp-ref-for-winrt/hstring#hstringoperator-stdwstring_view) 덕분에 가능합니다.

C#의 이 조각을 살펴보세요.

```csharp
var file = storageItem as StorageFile;
if (file != null)
...
```

C# `as` 키워드를 C++/WinRT로 이식하기 위해 지금까지 [**as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) 함수가 두어 번 사용된 것을 보았습니다. 유형 변환이 실패하면 이 함수가 예외를 throw합니다. 하지만 변환이 실패할 경우 코드에서 해당 상황을 처리할 수 있도록 `nullptr`이 반환되도록 하려면 [**try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function) 함수를 대신 사용합니다.

```cppwinrt
auto file{ storageItem.try_as<StorageFile>() };
if (file)
...
```

### <a name="copy-the-xaml-necessary-to-finish-up-porting-copyfiles"></a>**CopyFiles** 이식을 완료하는 데 필요한 XAML 복사

이제 C# 프로젝트에서 `CopyFiles.xaml` 파일의 전체 내용을 선택하여 C++/WinRT 프로젝트의 `CopyFiles.xaml` 파일에 붙여넣을 수 있습니다. 그러면 C++/WinRT 프로젝트에서 해당 파일의 기존 내용이 이 내용으로 바뀝니다.

마지막으로, 해당 XAML 태그를 덮어썼으므로 `CopyFiles.h` 및 `.cpp`를 편집하고 더미 **ClickHandler** 함수를 삭제합니다.

이제 **CopyFiles** 이식이 끝났으므로, 해당 단계를 진행하면 C++/WinRT 프로젝트가 빌드되고 실행되며 **CopyFiles** 시나리오가 작동합니다.

## <a name="copyimage"></a>**CopyImage**

**CopyImage** XAML 페이지 형식을 이식하려면 **CopyFiles**와 동일한 프로세스를 수행합니다. **CopyImage**를 이식할 때 [**IDisposable**](/dotnet/api/system.idisposable) 인터페이스를 구현하는 개체가 올바르게 삭제되도록 하는 C# [*using 문*](/dotnet/csharp/language-reference/keywords/using-statement)이 사용되는 것을 볼 수 있을 것입니다.

```csharp
if (imageReceived != null)
{
    using (var imageStream = await imageReceived.OpenReadAsync())
    {
        ... // Pass imageStream to other APIs, and do other work.
    }
}
```

C++/WinRT에서 이에 해당하는 인터페이스는 단일 **Close** 메서드가 포함된 [**IClosable**](/uwp/api/windows.foundation.iclosable)입니다. 위의 C# 코드에 해당하는 C++/WinRT는 다음과 같습니다.

```cppwinrt
if (imageReceived)
{
    auto imageStream{ co_await imageReceived.OpenReadAsync() };
    ... // Pass imageStream to other APIs, and do other work.
    imageStream.Close();
}
```

C++/WinRT 개체는 주로 명확한 종료가 없는 언어의 이점을 얻기 위해 **IClosable**을 구현합니다. C++/WinRT에는 결정적 종료가 있으므로 C++/WinRT를 작성할 때 **IClosable::Close**를 호출하지 않는 경우가 많습니다. 그러나 이를 호출하는 것이 유용한 경우가 있으며 이 경우가 그에 해당합니다. 여기서 *imageStream* 식별자는 기본 Windows Runtime 개체(이 경우 [**IRandomAccessStreamWithContentType**](/uwp/api/windows.storage.streams.irandomaccessstreamwithcontenttype)을 구현하는 개체)에 대한 참조 계수 래퍼입니다. *imageStream*의 종료자(소멸자)가 바깥쪽 범위(중괄호) 끝 부분에서 실행된다고 확인할 수는 있지만 종료자가 **Close**를 호출한다고 확신할 수는 없습니다. *imageStream*이 다른 API에 전달되어 해당 API가 기본 Windows Runtime 개체의 참조 수에 계속 합산될 수 있기 때문입니다. 이 경우가 바로 **Close**를 명시적으로 호출하면 좋은 경우입니다. 자세한 내용은 [사용하는 런타임 클래스에서 IClosable::Close를 호출해야 하나요?](./faq.md#do-i-need-to-call-iclosableclose-on-runtime-classes-that-i-consume)를 참조하세요.

다음으로, **OnDeferredImageRequestedHandler** 이벤트 처리기에서 볼 수 있는 C# 식 `(uint)(imageDecoder.OrientedPixelWidth * 0.5)`를 살펴보겠습니다. 이 식은 `uint`에 `double`을 곱하여 `double`을 생성합니다. 그런 다음, 이를 `uint`로 캐스팅합니다. C++/WinRT에서 비슷한 모양의 C 스타일 캐스트(`(uint32_t)(imageDecoder.OrientedPixelWidth() * 0.5)`)를 *사용할 수는 있지만* 원하는 캐스트 종류를 정확하게 지정하는 것이 좋습니다. 이 경우 `static_cast<uint32_t>(imageDecoder.OrientedPixelWidth() * 0.5)`를 사용하여 이 작업을 수행합니다.

C# 버전의 **CopyImage.OnDeferredImageRequestedHandler**에는 `catch` 절이 아닌 `finally` 절이 있습니다. C++/WinRT 버전에서 조금 더 나아가 지연된 렌더링이 성공했는지 여부를 보고할 수 있도록 `catch` 절을 구현했습니다.

이 XAML 페이지의 나머지 부분을 이식할 경우 새로운 논의 거리가 생기지 않습니다. 더미 **ClickHandler** 함수를 삭제하는 것을 잊지 마세요. 그리고 **CopyFiles**와 마찬가지로, 이식의 마지막 단계는 `CopyImage.xaml`의 전체 내용을 선택하고 C++/WinRT 프로젝트의 동일한 파일에 붙여넣는 것입니다.

## <a name="copytext"></a>**CopyText**

앞서 설명한 기술을 사용하여 `CopyText.xaml` 및 `CopyText.xaml.cs`를 이식할 수 있습니다.

## <a name="historyandroaming"></a>**HistoryAndRoaming**

**HistoryAndRoaming** XAML 페이지 형식을 이식할 때 몇 가지 흥미로운 점을 발견할 수 있습니다.

먼저 C# 소스 코드를 살펴보고 **OnNavigatedTo**부터 **OnHistoryEnabledChanged** 이벤트 처리기를 거쳐 마지막 비동기 함수 **CheckHistoryAndRoaming**까지 이어지는 제어 흐름을 확인하세요(대기가 없으므로 기본적으로 실행 후 망각 형태임). **CheckHistoryAndRoaming**은 비동기식이므로 `this` C++/WinRT에서 포인터의 수명에 주의해야 합니다. `HistoryAndRoaming.cpp` 소스 코드 파일에서 구현을 살펴보면 결과를 확인할 수 있습니다. 먼저 **Clipboard::HistoryEnabledChanged** 및 **Clipboard::RoamingEnabledChanged** 이벤트에 대리자를 연결할 때는 **HistoryAndRoaming** 페이지 개체에 대한 약한 참조만 가져옵니다. 그러려면 `this` 포인터에 대한 종속성 대신 [**winrt::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)에서 반환된 값에 대한 종속성을 가진 대리자를 만들면 됩니다. 즉, 최종적으로 비동기 코드로 호출되는 대리자 자체는 사용자가 **HistoryAndRoaming** 페이지에서 나갈 경우 이 페이지를 활성 상태로 유지하지 않습니다.

그리고 둘째로, **CheckHistoryAndRoaming**이라는 실행 후 망각 코루틴에 마침내 도달할 경우 처음으로 할 일은 이 코루틴이 최종적으로 완료될 때까지 **HistoryAndRoaming** 페이지가 최소한 활성화되어 있도록 `this`에 대한 강한 참조를 가져오는 것입니다. 앞서 설명한 두 가지 사항에 대한 자세한 내용은 [C++/WinRT의 강한 참조 및 약한 참조](./weak-references.md)에서 확인하세요.

**CheckHistoryAndRoaming**을 이식하는 동안 또 다른 흥미로운 부분을 찾았습니다. 여기에는 UI를 업데이트하는 코드가 포함되어 있으므로 기본 UI 스레드에서 이 작업을 수행해야 합니다. 처음에 이벤트 처리기로 호출되는 스레드는 주 UI 스레드입니다. 그러나 일반적으로 비동기 메서드는 임의의 스레드에서 실행되거나 다시 시작될 수 있습니다. C#에서 해결책은 [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync)를 호출하고 람다 함수 내에서 UI를 업데이트하는 것입니다. C++/WinRT에서는 [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) 함수와 `this` 포인터의 [**디스패처**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher)를 함께 사용하여 코루틴을 일시 중단하고 기본 UI 스레드에서 즉시 다시 시작할 수 있습니다.

관련 식은 `co_await winrt::resume_foreground(Dispatcher());`입니다. 또는 덜 명확하지만 `co_await Dispatcher();`로 단순하게 표현할 수 있습니다. 축약된 버전은 C++/WinRT에서 제공하는 변환 연산자로 구현할 수 있습니다.

이 XAML 페이지의 나머지 부분을 이식할 경우 새로운 논의 거리가 생기지 않습니다. 더미 **ClickHandler** 함수를 삭제하고 XAML 태그에 복사하는 것을 잊지 마세요.

## <a name="otherscenarios"></a>**OtherScenarios**

앞서 설명한 기술을 사용하여 `OtherScenarios.xaml` 및 `OtherScenarios.xaml.cs`를 이식할 수 있습니다.

## <a name="conclusion"></a>결론

이 연습을 통해 자체 C# 애플리케이션을 C++/WinRT에 이식하는 데 필요한 충분한 이식 정보 및 기술을 얻으셨기를 바랍니다. 복습을 위해 Cliboard 샘플의 이전(C#) 및 이후(C++/WinRT) 버전의 소스 코드를 다시 계속 살펴보고 둘을 나란히 비교하면서 관련성을 확인할 수 있습니다.

## <a name="related-topics"></a>관련 항목
* [C#에서 C++/WinRT로 이동](./move-to-winrt-from-csharp.md)