---
Description: UWP (유니버설 Windows 플랫폼) Api를 사용 하 여 Windows 10 사용자를 위한 데스크톱 응용 프로그램을 개선 합니다.
title: 데스크톱 앱에서 UWP Api 사용
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 78d9760c5ef21b29d09babaace0f4379b6a51209
ms.sourcegitcommit: cc108c791842789464c38a10e5d596c9bd878871
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/20/2019
ms.locfileid: "75302607"
---
# <a name="call-uwp-apis-in-desktop-apps"></a>데스크톱 앱에서 UWP Api 호출

UWP (유니버설 Windows 플랫폼) Api를 사용 하 여 Windows 10 사용자를 위해 최신 환경을 데스크톱 앱에 추가할 수 있습니다.

먼저 필요한 참조를 사용 하 여 프로젝트를 설정 합니다. 그런 다음 코드에서 UWP Api를 호출 하 여 데스크톱 앱에 Windows 10 환경을 추가 합니다. Windows 10 사용자를 위해 별도로 빌드 하거나 실행 하는 Windows 버전에 관계 없이 모든 사용자에 게 동일한 이진 파일을 배포할 수 있습니다.

일부 UWP Api는 [패키지 id](modernize-packaged-apps.md)가 있는 데스크톱 앱 에서만 지원 됩니다. 자세한 내용은 [사용 가능한 UWP api](desktop-to-uwp-supported-api.md)를 참조 하세요.

## <a name="set-up-your-project"></a>프로젝트 설정

프로젝트에서 몇 가지를 변경해야 UWP API를 사용할 수 있습니다.

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Windows 런타임 Api를 사용 하도록 .NET 프로젝트 수정

.NET 프로젝트에는 다음과 같은 두 가지 옵션이 있습니다.

* 앱이 Windows 10 버전 1803 이상을 대상으로 하는 경우 필요한 모든 참조를 제공 하는 NuGet 패키지를 설치할 수 있습니다.
* 또는 참조를 수동으로 추가할 수 있습니다.

#### <a name="to-use-the-nuget-option"></a>NuGet 옵션을 사용 하려면

1. [패키지 참조](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) 를 사용할 수 있는지 확인 합니다.

    1. Visual Studio에서 **도구-> NuGet 패키지 관리자-> 패키지 관리자 설정**을 클릭 합니다.
    2. **기본 패키지 관리 형식**에 대해 **PackageReference** 이 선택 되어 있는지 확인 합니다.

2. Visual Studio에서 프로젝트를 열고 **솔루션 탐색기** 에서 프로젝트를 마우스 오른쪽 단추로 클릭 한 다음 **NuGet 패키지 관리**를 선택 합니다.

3. **NuGet 패키지 관리자** 창에서 **찾아보기** 탭을 선택 하 고 `Microsoft.Windows.SDK.Contracts`를 검색 합니다.

4. `Microsoft.Windows.SDK.Contracts` 패키지를 찾은 후에는 **NuGet 패키지 관리자** 창의 오른쪽 창에서 대상으로 지정할 Windows 10 버전에 따라 설치할 패키지의 **버전** 을 선택 합니다.

    * **10.0.18362**: Windows 10 버전 1903에 대해 선택 합니다.
    * **10.0.17763**: Windows 10 버전 1809에 대해 선택 합니다.
    * **10.0.17134**: Windows 10 버전 1803에 대해 선택 합니다.

5. **설치**를 클릭합니다.

#### <a name="to-add-the-required-references-manually"></a>필요한 참조를 수동으로 추가 하려면

1. **참조 관리자** 대화 상자를 연 후, **찾아보기**와 **모든 파일**을 차례대로 선택합니다.

    ![참조 대화 상자 추가](images/desktop-to-uwp/browse-references.png)

2. 이러한 파일에 대 한 참조를 추가 합니다.

    |파일|위치|
    |--|--|
    |System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.WindowsRuntime.UI.Xaml|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.InteropServices.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |windows winmd|C:\Program Files (x86) \Windows Kits\10\UnionMetadata\\<*sdk 버전*> \facade|
    |Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86) \Windows Kits\10\References\\<*sdk 버전*> \Windows.Foundation.UniversalApiContract\<*버전*>|
    |Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86) \Windows Kits\10\References\\<*sdk 버전*> \Windows.Foundation.FoundationContract\<*버전*>|

3. **속성** 창에서 각 *.winmd* 파일의 **로컬 복사** 필드를 **False**로 설정합니다.

    ![copy-local-field](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-win32-project-to-use-windows-runtime-apis"></a>Windows 런타임 Api C++ 를 사용 하도록 Win32 프로젝트 수정

[ C++/Winrt](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/) 를 사용 하 여 Windows 런타임 api를 사용 합니다. C++/WinRT는 헤더 파일 기반 라이브러리로 구현된 WinRT(Windows 런타임) API용 최신의 완전한 표준 C++17 언어 프로젝션이며, 최신 Windows API에 최고 수준의 액세스를 제공하도록 설계되었습니다.

/Winrt에 대 한 C++프로젝트를 구성 하려면:

* 새 프로젝트의 경우 [ C++/winrt Visual Studio 확장 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) 을 설치 하 고 해당 확장에 포함 된 C++/winrt 프로젝트 템플릿 중 하나를 사용할 수 있습니다.
* 기존 프로젝트의 경우 프로젝트에 [CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet 패키지를 설치할 수 있습니다.

이러한 옵션에 대 한 자세한 내용은 [이 문서](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)를 참조 하세요.

## <a name="add-windows-10-experiences"></a>Windows 10 환경 추가

사용자가 Windows 10에서 응용 프로그램을 실행할 때 만족을 주는 최신 환경을 추가할 준비가 되었습니다. 이 디자인 흐름을 사용합니다.

:white_check_mark: **먼저 추가하고 싶은 환경을 결정합니다.**

선택할 수 있는 환경이 많습니다. 예를 들어, [수익 화 api](/windows/uwp/monetize)를 사용 하 여 구매 주문 흐름을 간소화 하거나 다른 사용자가 게시 한 새 사진과 같이 공유할 흥미로운 항목이 있는 경우 [응용 프로그램에 직접 주의를 기울일](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts) 수 있습니다.

![알림](images/desktop-to-uwp/toast.png)

사용자가 메시지를 무시 또는 해제하는 경우에도, 알림 센터에서 다시 메시지를 확인한 후 메시지를 클릭하여 앱을 열 수 있습니다. 이로 인해 응용 프로그램의 참여가 늘어나고 응용 프로그램이 운영 체제와 긴밀 하 게 통합 되도록 하는 추가 보너스로 있습니다. 해당 환경에 대 한 코드는이 문서의 뒷부분에 나와 있습니다.

자세한 내용은 [UWP 설명서](/windows/uwp/get-started/) 를 참조 하세요.

:white_check_mark: **강화 또는 확장 여부 결정**

*개선* 및 *확장*이라는 용어를 사용 하는 경우가 많으므로 이러한 각 용어의 의미를 정확 하 게 설명 하는 데 잠시 시간이 걸립니다.

확장 이라는 용어를 사용 하 여 데스크톱 앱에서 직접 호출할 수 있는 Windows 런타임 *api를 설명* 합니다 (msix 패키지에서 응용 프로그램을 패키지 하도록 선택 했는지 여부에 관계 없이). Windows 10 환경을 선택한 경우 해당 환경을 만들어야 하는 Api를 확인 한 다음 [이 목록](desktop-to-uwp-supported-api.md)에 해당 api가 나타나는지 확인 합니다. 데스크톱 앱에서 직접 호출할 수 있는 Api 목록입니다. API와 연결된 기능을 UWP 프로세스에서만 실행시킬 수 있다면, 이 목록에 API가 표시되지 않습니다. 주로 UWP 맵 컨트롤 또는 Windows Hello 보안 프롬프트와 같은 UWP XAML을 렌더링 하는 Api를 포함 하는 경우도 있습니다.

> [!NOTE]
> UWP XAML을 렌더링 하는 Api는 일반적으로 바탕 화면에서 직접 호출할 수 없지만 다른 방법을 사용할 수 있습니다. UWP XAML 컨트롤이 나 기타 사용자 지정 시각적 개체를 호스팅하려면 [Xaml 아일랜드](xaml-islands.md) (windows 10, 버전 1903부터) 및 [시각적 계층](visual-layer-in-desktop-apps.md) (windows 10 버전 1803에서 시작)을 사용할 수 있습니다. 이러한 기능은 패키지 또는 패키지 되지 않은 데스크톱 앱에서 사용할 수 있습니다.

MSIX 패키지에서 데스크톱 앱을 패키징하는 경우 다른 옵션은 솔루션에 UWP 프로젝트를 추가 하 여 응용 프로그램을 *확장* 하는 것입니다. 데스크톱 프로젝트는 여전히 응용 프로그램의 진입점 이지만 UWP 프로젝트는 [이 목록](desktop-to-uwp-supported-api.md)에 표시 되지 않은 모든 api에 대 한 액세스를 제공 합니다. 데스크톱 앱은 app service를 사용 하 여 UWP 프로세스와 통신할 수 있으며,이를 설정 하는 방법에 대 한 많은 지침이 있습니다. UWP 프로젝트가 필요한 환경을 추가 하려면 [uwp 구성 요소를 사용 하 여 확장](desktop-to-uwp-extend.md)을 참조 하세요.

:white_check_mark: **참조 API 계약**

데스크톱 앱에서 직접 API를 호출할 수 있는 경우 브라우저를 열고 해당 API에 대 한 참조 항목을 검색 합니다.
API 요약 아래에서 해당 API의 API 계약에 대한 설명한 표를 찾을 수 있습니다. 표의 예는 다음과 같습니다.

![API 계약 표](images/desktop-to-uwp/contract-table.png)

.NET 기반 데스크톱 앱을 갖고 있다면, API 계약에 참조를 추가한 후 해당 파일의 **로컬 복사** 속성을 **False**로 설정합니다. C++ 기반 프로젝트를 사용하는 경우, **Additional Include Directories** 경로를 이 계약이 포함된 폴더에 추가합니다.

:white_check_mark: **환경을 추가하는 API 호출**

앞서 보았던 알림 창을 표시하는 데 사용하는 코드입니다. 이러한 Api는이 [목록](desktop-to-uwp-supported-api.md) 에 표시 되므로이 코드를 데스크톱 앱에 추가 하 고 지금 바로 실행할 수 있습니다.

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

```C++
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

알림에 대한 자세한 내용은 [적응형 및 대화형 알림 메시지](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts)를 참조하세요.

## <a name="support-windows-xp-windows-vista-and-windows-78-install-bases"></a>Windows XP, Windows Vista, Windows 7/8 설치 기반 지원

새 분기를 만들고 별도의 코드 베이스를 유지 관리할 필요 없이 Windows 10 용 응용 프로그램을 현대화 수 있습니다.

Windows 10 사용자를 위해 별도 바이너리를 빌드하고 싶다면 조건부 컴파일을 사용합니다. 모든 Windows 사용자에게 배포할 바이너리들을 빌드하고 싶다면 런타임 검사를 사용하세요.

간략히 각 옵션을 살펴보겠습니다.

### <a name="conditional-compilation"></a>조건부 컴파일

하나의 코드 기반을 유지, Windows 10 사용자를 위한 바이너리 세트를 컴파일할 수 있습니다.

먼저 프로젝트에 새 빌드 구성을 추가합니다.

![빌드 구성](images/desktop-to-uwp/build-config.png)

해당 빌드 구성의 경우 Windows 런타임 Api를 호출 하는 코드를 식별 하는 상수를 만듭니다.  

.NET 기반 프로젝트의 상수는 **Conditional Compilation Constant**입니다.

![전처리기](images/desktop-to-uwp/compilation-constants.png)

C++ 기반 프로젝트의 상수는 **Preprocessor Definition**입니다.

![전처리기](images/desktop-to-uwp/pre-processor.png)

UWP 코드 블록 앞에 상수를 추가합니다.

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

실행 중인 Windows 버전에 관계없이 모든 Windows 사용자를 위한 바이너리 세트를 컴파일 할 수 있습니다. 응용 프로그램은 사용자가 Windows 10에서 패키지 된 응용 프로그램으로 응용 프로그램을 실행 하는 경우에만 Windows 런타임 Api를 호출 합니다.

코드에 런타임 검사를 추가 하는 가장 쉬운 방법은이 Nuget 패키지를 설치 하는 것입니다. [데스크톱 브리지 도우미](https://www.nuget.org/packages/DesktopBridge.Helpers/) 에서 ``IsRunningAsUWP()`` 메서드를 사용 하 여 Windows 런타임 api를 호출 하는 모든 코드를 성문 합니다. 자세한 내용은 [Desktop Bridge - Identify the application's context](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/)라는 블로그를 참조하세요.

## <a name="related-samples"></a>관련 샘플

* [Hello World 예제](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [보조 타일](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [저장소 API 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [UWP UpdateTask을 구현 하는 응용 프로그램](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [UWP 샘플에 대 한 데스크톱 앱 브리지](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="find-answers-to-your-questions"></a>질문에 대한 답변 찾기

질문이 있으세요? Stack Overflow에서 질문해 주세요. 저희 팀은 이러한 [태그](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다. [여기](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)에서 Microsoft에 문의할 수도 있습니다.
