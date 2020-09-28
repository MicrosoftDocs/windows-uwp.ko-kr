---
description: Windows 런타임 API를 사용하여 Windows 10 사용자용 데스크톱 애플리케이션을 개선하세요.
title: 데스크톱 앱에서 Windows 런타임 API 호출
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 2b0d6bb305490e05c2670f0e0a326601c51a8373
ms.sourcegitcommit: 609441402c17d92e7bfac83a6056909bb235223c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/21/2020
ms.locfileid: "90837818"
---
# <a name="call-windows-runtime-apis-in-desktop-apps"></a>데스크톱 앱에서 Windows 런타임 API 호출

UWP(유니버설 Windows 플랫폼) API를 사용하여 Windows 10 사용자를 지원하는 최신 경험을 데스크톱 앱에 추가할 수 있습니다.

먼저 필요한 참조를 사용하여 프로젝트를 설정합니다. 그런 다음, 코드에서 Windows 런타임 API를 호출하여 데스크톱 앱에 Windows 10 환경을 추가합니다. Windows 10 사용자를 위해 별도로 빌드할 수도 있고, 사용자가 실행하는 Windows 버전에 상관없이 모든 사용자에게 동일한 바이너리를 배포할 수도 있습니다.

일부 Windows 런타임 API는 [패키지 ID](modernize-packaged-apps.md)가 있는 데스크톱 앱에서만 지원됩니다. 자세한 내용은 [사용 가능한 Windows 런타임 API](desktop-to-uwp-supported-api.md)를 참조하세요.

## <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Windows 런타임 API를 사용하도록 .NET 프로젝트 수정

.NET 프로젝트에는 다음과 같은 몇 가지 옵션이 있습니다.

* .NET 5 Preview 8부터 TFM(대상 프레임워크 모니커)을 프로젝트 파일에 추가하여 WinRT API에 액세스할 수 있습니다. 이 옵션은 Windows 10, 버전 1809 이상을 대상으로 하는 프로젝트에서 지원됩니다.
* 이전 버전 .NET의 경우 `Microsoft.Windows.SDK.Contracts` NuGet 패키지를 설치하여 프로젝트에 필요한 모든 참조를 추가할 수 있습니다. 이 옵션은 Windows 10, 버전 1803 이상을 대상으로 하는 프로젝트에서 지원됩니다.
* 프로젝트가 .NET 5 Preview 8 이상 버전을 다중 대상으로 하는 경우 두 옵션을 모두 사용하도록 프로젝트 파일을 구성할 수 있습니다.

### <a name="net-5-preview-8-and-later-use-the-target-framework-moniker-option"></a>.NET 5 Preview 8 이상: 대상 프레임워크 모니커 옵션 사용 

이 옵션은 .NET 5 Preview 8(또는 이전 릴리스) 및 대상 Windows 10, 버전 1809 이상 OS 릴리스를 사용하는 프로젝트에서만 지원됩니다. 이 시나리오 대한 자세한 배경 정보는 [이 블로그 게시물](https://blogs.windows.com/windowsdeveloper/2020/09/03/calling-windows-apis-in-net5/)을 참조하세요.

1. Visual Studio에서 프로젝트를 연 상태로 **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭하고 **프로젝트 파일 편집**을 선택합니다. 프로젝트 파일은 다음과 유사하게 나타납니다.

    ```csharp
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">
      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>net5.0</TargetFramework>
        <UseWindowsForms>true</UseWindowsForms>
      </PropertyGroup>
    </Project>
    ```

2. **TargetFramework** 요소의 값을 다음 문자열 중 하나로 바꿉니다.

    * **net5.0-windows10.0.17763.0**: 앱이 Windows 10, 버전 1809를 대상으로 하는 경우 이 값을 사용합니다.
    * **net5.0-windows10.0.18362.0**: 앱이 Windows 10, 버전 1903을 대상으로 하는 경우 이 값을 사용합니다.
    * **net5.0-windows10.0.19041.0**: 앱이 Windows 10, 버전 2004를 대상으로 하는 경우 이 값을 사용합니다.

    예를 들어, 다음 요소는 Windows 10, 버전 2004를 대상으로 하는 프로젝트를 위한 것입니다.

    ```csharp
    <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
    ```

3. 변경 내용을 저장하고 프로젝트 파일을 닫습니다.

### <a name="earlier-versions-of-net-install-the-microsoftwindowssdkcontracts-nuget-package"></a>이전 버전의 .NET: Microsoft.Windows.SDK.Contracts NuGet 패키지 설치

앱이 .NET Core 3.x, .NET 5 Preview 7 이하 또는 .NET Framework를 사용하는 경우 이 옵션을 사용합니다. 이 옵션은 Windows 10, 버전 1803 이상 OS 릴리스를 대상으로 하는 프로젝트에서 지원됩니다.

1. [패키지 참조](/nuget/consume-packages/package-references-in-project-files)를 사용하도록 설정합니다.

    1. Visual Studio에서 **도구 -> NuGet 패키지 관리자 -> 패키지 관리자 설정**을 클릭합니다.
    2. **기본 패키지 관리 형식**으로 **PackageReference**를 선택합니다.

2. Visual Studio에서 프로젝트를 연 상태로 **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭하고 **NuGet 패키지 관리**를 선택합니다.

3. **NuGet 패키지 관리자** 창에서 **찾아보기** 탭을 선택하고 `Microsoft.Windows.SDK.Contracts`를 검색합니다.

4. `Microsoft.Windows.SDK.Contracts` 패키지가 검색되면 **NuGet 패키지 관리자**의 오른쪽 창에서 대상으로 지정할 Windows 10 버전에 따라 설치하려는 패키지의 **버전**을 선택합니다.

    * **10.0.19041.xxxx**: Windows 10, 버전 2004가 대상이면 이 버전을 선택합니다.
    * **10.0.18362.xxxx**: Windows 10 버전 1903이 대상이면 이 버전을 선택합니다.
    * **10.0.17763.xxxx**: Windows 10 버전 1809가 대상이면 이 버전을 선택합니다.
    * **10.0.17134.xxxx**: Windows 10 버전 1803이 대상이면 이 버전을 선택합니다.

5. **설치**를 클릭합니다.

### <a name="configure-projects-that-multi-target-different-versions-of-net"></a>여러 버전의 .NET을 다중 대상으로 하는 프로젝트 구성

프로젝트가 .NET 5 Preview 8 이상 및 이전 버전(.NET Core 3.x 및 .NET Framework 포함)을 다중 대상으로 하는 경우, .NET 5 Preview 8 이상에 대해서는 WinRT API 참조에서 자동으로 끌어오도록 대상 프레임워크 모니커를 사용하고 이전 버전에 대해서는 `Microsoft.Windows.SDK.Contracts` NuGet 패키지를 사용하도록 프로젝트 파일을 구성할 수 있습니다.

1. Visual Studio에서 프로젝트를 연 상태로 **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭하고 **프로젝트 파일 편집**을 선택합니다. 다음 예제에서는 .NET Core 3.1을 사용하는 앱에 대한 프로젝트 파일을 보여 줍니다.

    ```csharp
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">
      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.1</TargetFramework>
        <UseWindowsForms>true</UseWindowsForms>
      </PropertyGroup>
    </Project>
    ```

2. 파일의 **TargetFramework** 요소를 **TargetFrameworks** 요소(복수형)로 바꿉니다. 이 요소에서 대상으로 지정할 .NET의 모든 버전에 대한 대상 프레임워크 모니커는 세미콜론으로 구분하여 지정합니다. 

    * .NET 5 Preview 8 이상에서는 다음 대상 프레임워크 모니커 중 하나를 사용합니다.
        * **net5.0-windows10.0.17763.0**: 앱이 Windows 10, 버전 1809를 대상으로 하는 경우 이 값을 사용합니다.
        * **net5.0-windows10.0.18362.0**: 앱이 Windows 10, 버전 1903을 대상으로 하는 경우 이 값을 사용합니다.
        * **net5.0-windows10.0.19041.0**: 앱이 Windows 10, 버전 2004를 대상으로 하는 경우 이 값을 사용합니다.
    * .NET Core 3.x의 경우 **netcoreapp3.0** 또는 **netcoreapp3.1**을 사용합니다.
    * .NET Framework의 경우 **net46**을 사용합니다.

    다음 예제에서는 .NET Core 3.1 및 .NET 5 Preview 8(Windows 10, 버전 2004)을 다중 대상으로 하는 방법을 보여 줍니다.

    ```csharp
    <TargetFrameworks>netcoreapp3.1;net5.0-windows10.0.19041.0</TargetFrameworks>
    ```

3. **PropertyGroup** 요소 뒤에 모든 버전의 .NET Core 3.x 또는 앱이 대상으로 하는 .NET Framework용 `Microsoft.Windows.SDK.Contracts` NuGet 패키지를 설치하는 조건문이 포함된 **PackageReference** 요소를 추가합니다. **PackageReference** 요소는 **ItemGroup** 요소의 하위 요소여야 합니다. 다음 예제에서는 .NET Core 3.1에 대해 이 작업을 수행하는 방법을 보여 줍니다.

    ```csharp
    <ItemGroup>
      <PackageReference Condition="'$(TargetFramework)' == 'netcoreapp3.1'"
                        Include="Microsoft.Windows.SDK.Contracts"
                        Version="10.0.19041.0" />
    </ItemGroup>
    ```

    완료되면 프로젝트 파일은 다음과 유사하게 나타납니다.

    ```csharp
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">
      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFrameworks>netcoreapp3.1;net5.0-windows10.0.19041.0</TargetFrameworks>
        <UseWPF>true</UseWPF>
      </PropertyGroup>
      <ItemGroup>
        <PackageReference Condition="'$(TargetFramework)' == 'netcoreapp3.1'"
                         Include="Microsoft.Windows.SDK.Contracts"
                         Version="10.0.19041.0" />
      </ItemGroup>
    </Project>
    ```

4. 변경 내용을 저장하고 프로젝트 파일을 닫습니다.

## <a name="modify-a-c-win32-project-to-use-windows-runtime-apis"></a>Windows 런타임 API를 사용하도록 C++ Win32 프로젝트 수정

[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/)를 사용하여 Windows 런타임 API를 사용합니다. C++/WinRT는 헤더 파일 기반 라이브러리로 구현된 WinRT(Windows 런타임) API용 최신의 완전한 표준 C++17 언어 프로젝션이며, 최신 Windows API에 최고 수준의 액세스를 제공하도록 설계되었습니다.

다음과 같이 C++/WinRT에 사용할 프로젝트를 구성합니다.

* 새 프로젝트인 경우 [C++/WinRT Visual Studio Extension(VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)을 설치하고 해당 확장에 포함된 C++/WinRT 프로젝트 템플릿 중 하나를 사용할 수 있습니다.
* 기존 프로젝트인 경우 [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet 패키지를 프로젝트에 설치할 수 있습니다.

이러한 옵션에 대한 자세한 내용은 [이 문서](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)를 참조하세요.

## <a name="add-windows-10-experiences"></a>Windows 10 환경 추가

사용자가 Windows 10에서 애플리케이션을 실행할 때 만족을 주는 최신 환경을 추가할 준비가 되었습니다. 다음과 같은 디자인 흐름을 사용합니다.

: white_check_mark: **첫째, 추가하고 싶은 환경 결정**

선택할 수 있는 환경이 많습니다. 예를 들어 [수익 창출 API](/windows/uwp/monetize)를 사용하여 구매 주문 흐름을 간소화할 수 있습니다. 또는 다른 사용자가 추가한 새 사진처럼 공유하면 재미있는 콘텐츠가 있을 때 [애플리케이션으로 직접 관심을 유도](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts)할 수 있습니다.

![알림 메시지](images/desktop-to-uwp/toast.png)

사용자가 메시지를 무시 또는 해제하는 경우에도 알림 센터에서 다시 메시지를 확인한 후 메시지를 클릭하여 앱을 열 수 있습니다. 이렇게 하면 애플리케이션 참여도를 높일 수 있으며 애플리케이션이 운영 체제와 긴밀하게 통합된 것처럼 보이게 하는 부가적인 효과가 있습니다. 이 문서의 뒷부분에서 이러한 환경의 코드를 보여드리겠습니다.

자세한 내용은 [UWP 설명서](/windows/uwp/get-started/)를 참조하세요.

: white_check_mark: **강화할 것인지 아니면 확장할 것인지 결정**

저희는 *강화*와 *확장*이라는 용어를 자주 사용합니다. 각 용어의 의미를 정확히 설명하겠습니다.

*강화*라는 용어는 데스크톱 앱에서 직접 호출할 수 있는 Windows 런타임 API를 설명할 때 사용됩니다(애플리케이션을 MSIX 패키지에 패키징하기로 선택했는지 여부에 관계 없이). Windows 10 환경을 선택할 때 환경을 만들기 위해 필요한 API를 식별하고 해당 API가 [이 목록](desktop-to-uwp-supported-api.md)에 표시되는지 확인하세요. 이 목록은 데스크톱 앱에서 직접 호출할 수 있는 API 목록입니다. API와 연결된 기능을 UWP 프로세스에서만 실행할 수 있는 경우에는 이 목록에 API가 표시되지 않습니다. UWP 지도 컨트롤이나 Windows Hello 보안 프롬프트 같은 UWP XAML을 렌더링하는 API가 여기에 자주 포함됩니다.

> [!NOTE]
> UWP XAML을 렌더링하는 API는 일반적으로 데스크톱에서 직접 호출할 수 없지만, 다른 방법을 사용할 수 있습니다. UWP XAML 컨트롤 또는 다른 사용자 지정 시각적 환경을 호스팅하려면 [XAML Islands](xaml-islands.md)(Windows 10 버전 1903부터) 및 [시각적 레이어](visual-layer-in-desktop-apps.md)(Windows 10 버전 1803부터)를 사용할 수 있습니다. 이러한 기능은 패키징 또는 패키징되지 않은 데스크톱 앱에서 사용할 수 있습니다.

데스크톱 앱을 MSIX 패키지에 패키징하기로 선택한 경우 또 다른 옵션으로 솔루션에 UWP 프로젝트를 추가하여 애플리케이션을 *확장*할 수 있습니다. 데스크톱 프로젝트는 여전히 애플리케이션의 진입점이지만, UWP 프로젝트를 통해 [이 목록](desktop-to-uwp-supported-api.md)에 표시되지 않는 모든 API에 액세스할 수 있습니다. 데스크톱 앱은 앱 서비스를 사용하여 UWP 프로세스와 통신할 수 있으며, 이렇게 설정하는 방법에 대한 여러 지침이 제공됩니다. UWP 프로젝트가 필요한 환경을 추가하려면 [UWP 구성 요소를 사용하여 확장](desktop-to-uwp-extend.md)을 참조하세요.

: white_check_mark: **참조 API 계약**

데스크톱 앱에서 직접 API를 호출할 수 있다면 브라우저를 열고 해당 API의 참조 토픽을 검색합니다.
아래의 API 요약 정보에서 해당 API의 API 계약에 대해 설명하는 표를 찾을 수 있습니다. 다음은 해당 표의 예입니다.

![API 계약 표](images/desktop-to-uwp/contract-table.png)

.NET 기반 데스크톱 앱을 갖고 있다면 API 계약에 참조를 추가한 다음, 해당 파일의 **로컬 복사** 속성을 **False**로 설정합니다. C++ 기반 프로젝트를 사용하는 경우 이 계약이 포함된 폴더의 경로인 **Additional Include Directories**에 추가합니다.

: white_check_mark: **API를 호출하여 환경 추가**

다음은 앞에서 살펴본 알림 창을 표시하는 데 사용할 코드입니다. 지금 바로 데스크톱 앱에 이 코드를 추가하고 실행할 수 있도록 다음 API가 이 [목록](desktop-to-uwp-supported-api.md)에 표시됩니다.

```csharp
using Windows.Foundation;
using Windows.System;
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;
...

private void ShowToast()
{
    string title = "featured picture of the day";
    string content = "beautiful scenery";
    string image = "https://picsum.photos/360/180?image=104";
    string logo = "https://picsum.photos/64?image=883";

    string xmlString =
    $@"<toast><visual>
       <binding template='ToastGeneric'>
       <text>{title}</text>
       <text>{content}</text>
       <image src='{image}'/>
       <image src='{logo}' placement='appLogoOverride' hint-crop='circle'/>
       </binding>
      </visual></toast>";

    XmlDocument toastXml = new XmlDocument();
    toastXml.LoadXml(xmlString);

    ToastNotification toast = new ToastNotification(toastXml);

    ToastNotificationManager.CreateToastNotifier().Show(toast);
}
```

```cppwinrt
#include <sstream>
#include <winrt/Windows.Data.Xml.Dom.h>
#include <winrt/Windows.UI.Notifications.h>

using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::System;
using namespace winrt::Windows::UI::Notifications;
using namespace winrt::Windows::Data::Xml::Dom;

void UWP::ShowToast()
{
    std::wstring const title = L"featured picture of the day";
    std::wstring const content = L"beautiful scenery";
    std::wstring const image = L"https://picsum.photos/360/180?image=104";
    std::wstring const logo = L"https://picsum.photos/64?image=883";

    std::wostringstream xmlString;
    xmlString << L"<toast><visual><binding template='ToastGeneric'>" <<
        L"<text>" << title << L"</text>" <<
        L"<text>" << content << L"</text>" <<
        L"<image src='" << image << L"'/>" <<
        L"<image src='" << logo << L"'" <<
        L" placement='appLogoOverride' hint-crop='circle'/>" <<
        L"</binding></visual></toast>";

    XmlDocument toastXml;

    toastXml.LoadXml(xmlString.str().c_str());

    ToastNotificationManager::CreateToastNotifier().Show(ToastNotification(toastXml));
}
```

```cppcx
using namespace Windows::Foundation;
using namespace Windows::System;
using namespace Windows::UI::Notifications;
using namespace Windows::Data::Xml::Dom;

void UWP::ShowToast()
{
    Platform::String ^title = "featured picture of the day";
    Platform::String ^content = "beautiful scenery";
    Platform::String ^image = "https://picsum.photos/360/180?image=104";
    Platform::String ^logo = "https://picsum.photos/64?image=883";

    Platform::String ^xmlString =
        L"<toast><visual><binding template='ToastGeneric'>" +
        L"<text>" + title + "</text>" +
        L"<text>"+ content + "</text>" +
        L"<image src='" + image + "'/>" +
        L"<image src='" + logo + "'" +
        L" placement='appLogoOverride' hint-crop='circle'/>" +
        L"</binding></visual></toast>";

    XmlDocument ^toastXml = ref new XmlDocument();

    toastXml->LoadXml(xmlString);

    ToastNotificationManager::CreateToastNotifier()->Show(ref new ToastNotification(toastXml));
}
```

알림에 대한 자세한 정보는 [적응형 및 대화형 알림 메시지](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts)를 참조하세요.

## <a name="support-windows-xp-windows-vista-and-windows-78-install-bases"></a>Windows XP, Windows Vista, Windows 7/8 설치 기반 지원

새 분기를 만들고 별도의 코드 기반을 유지할 필요 없이 Windows 10용 애플리케이션을 현대화할 수 있습니다.

Windows 10 사용자를 위한 별도의 바이너리를 빌드하려면 조건부 컴파일을 사용합니다. 모든 Windows 사용자에게 배포할 단일 바이너리 세트를 빌드하려면 런타임 검사를 사용합니다.

각 옵션을 간단하게 살펴보겠습니다.

### <a name="conditional-compilation"></a>조건부 컴파일

Windows 10 사용자만을 위한 한 가지 코드 기반을 유지하고 바이너리 세트를 컴파일할 수 있습니다.

먼저 프로젝트에 새 빌드 구성을 추가합니다.

![빌드 구성](images/desktop-to-uwp/build-config.png)

해당 빌드 구성에서, Windows 런타임 API를 호출하는 코드를 식별하는 상수를 만듭니다.  

.NET 기반 프로젝트의 상수는 **Conditional Compilation Constant**입니다.

![조건부 컴파일 상수](images/desktop-to-uwp/compilation-constants.png)

C++ 기반 프로젝트의 상수는 **Preprocessor Definition**입니다.

![전처리기 정의 상수](images/desktop-to-uwp/pre-processor.png)

UWP 코드 블록 앞에 해당 상수를 추가합니다.

```csharp
[System.Diagnostics.Conditional("_UWP")]
private void ShowToast()
{
 ...
}
```

```C++
#if _UWP
void UWP::ShowToast()
{
 ...
}
#endif
```

활성 빌드 구성에서 상수를 정의한 경우에만 컴파일러가 코드를 빌드합니다.

### <a name="runtime-checks"></a>런타임 검사

실행 중인 Windows 버전에 관계 없이 모든 Windows 사용자를 위한 바이너리 세트를 컴파일 할 수 있습니다. 사용자가 애플리케이션을 Windows 10에서 패키지된 애플리케이션으로 실행하는 경우에만 애플리케이션이 Windows 런타임 API를 호출합니다.

코드에 런타임 검사를 추가하는 가장 쉬운 방법은 Nuget 패키지: [데스크톱 브리지 도우미](https://www.nuget.org/packages/DesktopBridge.Helpers/)를 설치한 다음, ``IsRunningAsUWP()`` 메서드를 사용하여 Windows 런타임 API를 호출하는 모든 코드를 제어하는 것입니다. 자세한 내용은 블로그 게시물 [Desktop Bridge - Identify the application's context(데스크톱 브리지 - 애플리케이션 컨텍스트 식별)](/archive/blogs/appconsult/desktop-bridge-identify-the-applications-context)을 참조하세요.

## <a name="related-samples"></a>관련 샘플

* [Hello World 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [보조 타일](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Microsoft Store API 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [UWP UpdateTask를 구현하는 WinForms 애플리케이션](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [데스크톱 앱-UWP 브리지 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="find-answers-to-your-questions"></a>질문에 대한 답변 찾기

질문이 있으세요? Stack Overflow에서 질문하세요. 저희 팀은 이러한 [태그](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다. [여기](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)에서 문의할 수도 있습니다.