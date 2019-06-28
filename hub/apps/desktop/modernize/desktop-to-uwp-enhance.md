---
Description: 유니버설 Windows 플랫폼 (UWP) Api를 사용 하 여 Windows 10 사용자를 위한 데스크톱 응용 프로그램을 향상 합니다.
title: 데스크톱 앱에서 사용 하 여 UWP Api
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 0545ea525b96d3a9310f3a761fd60a644f21baeb
ms.sourcegitcommit: b8087f8b6cf8367f8adb7d6db4581d9aa47b4861
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67414076"
---
# <a name="call-uwp-apis-in-desktop-apps"></a>데스크톱 앱에서 UWP Api를 호출 합니다.

최신 환경을 실행 되는 Windows 10 사용자에 대 한 데스크톱 앱에 추가할 유니버설 Windows 플랫폼 (UWP) Api를 사용할 수 있습니다.

먼저, 필요한 참조를 사용 하 여 프로젝트를 설정 합니다. 그런 다음 Windows 10 데스크톱 앱에 환경을 추가 하려면 코드에서 UWP Api를 호출 합니다. Windows 10 사용자에 대해 개별적으로 빌드 수도 있고 실행 되는 Windows 버전에 관계 없이 모든 사용자가 동일한 이진 파일을 배포할 수 있습니다.

일부 UWP Api에 패키지 된 데스크톱 앱 에서만 지원 되는 [MSIX 패키지](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)합니다. 자세한 내용은 [사용 가능한 UWP Api](desktop-to-uwp-supported-api.md)합니다.

## <a name="set-up-your-project"></a>프로젝트 설정

프로젝트에서 몇 가지를 변경해야 UWP API를 사용할 수 있습니다.

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Windows 런타임 Api를 사용 하도록.NET 프로젝트를 수정 합니다.

가지.NET 프로젝트에 대 한 두 가지 옵션이 있습니다.

* 앱에서 Windows 10 버전 1803 이상이 대상으로 하는 경우에 필요한 모든 참조를 제공 하는 NuGet 패키지를 설치할 수 있습니다.
* 또는 수동으로 참조를 추가할 수 있습니다.

#### <a name="to-use-the-nuget-option"></a>NuGet 옵션을 사용 하려면

1. 했는지 [패키지 참조](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) 활성화 됩니다.

    1. Visual Studio에서 클릭 **도구-> NuGet 패키지 관리자-> 패키지 관리자 설정**합니다.
    2. 했는지 **PackageReference** 에 대해 선택 되었는지 **기본 패키지 관리 형식**합니다.

2. Visual Studio에서 열려 있는 프로젝트에서 프로젝트를 마우스 오른쪽 단추로 클릭 **솔루션 탐색기** 선택한 **NuGet 패키지 관리**합니다.

3. 에 **NuGet 패키지 관리자** 창에서를 **찾아보기** 탭을 검색할 `Microsoft.Windows.SDK.Contracts`.

4. 후는 `Microsoft.Windows.SDK.Contracts` 의 오른쪽 창에서 패키지를 찾은 합니다 **NuGet 패키지 관리자** 창 선택 합니다 **버전** 를 설치 하려면 대상으로 하려는 Windows 10의 버전에 따라 패키지의:

    * **10.0.18362.xxxx-preview**: Windows 10의 버전이 1903 경우이 선택 합니다.
    * **10.0.17763.xxxx-preview**: Windows 10, 버전 1809이를 선택 합니다.
    * **10.0.17134.xxxx-preview**: Windows 10, 버전 1803이를 선택 합니다.

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
    |windows.winmd|C:\Program Files (x86)\Windows Kits\10\UnionMetadata\\<*sdk version*>\Facade|
    |Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\\<*sdk version*>\Windows.Foundation.UniversalApiContract\<*version*>|
    |Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\\<*sdk version*>\Windows.Foundation.FoundationContract\<*version*>|

3. **속성** 창에서 각 *.winmd* 파일의 **로컬 복사** 필드를 **False**로 설정합니다.

    ![copy-local-field](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-project-to-use-windows-runtime-apis"></a>수정 된 C++ Windows 런타임 Api를 사용 하도록 프로젝트

사용 하 여 [ C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/) Windows 런타임 Api를 사용 하도록 합니다. C++/WinRT는 Windows 런타임(WinRT) API용 최신 표준 C++17 언어 프로젝션으로서 헤더 파일 기반 라이브러리로 구현되며, 오늘날 Windows API에 대해 최고 수준의 액세스를 제공하도록 설계되었습니다.

C +에 대 한 프로젝트를 구성 하려면 + WinRT, 참조 [추가할 C + Windows 데스크톱 응용 프로그램 프로젝트를 수정 + WinRT 지원](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support)합니다.

## <a name="add-windows-10-experiences"></a>Windows 10 환경 추가

사용자가 Windows 10에서 응용 프로그램을 실행할 때 만족을 주는 최신 환경을 추가할 준비가 되었습니다. 이 디자인 흐름을 사용합니다.

: white_check_mark: **어떤 환경을 추가 하려면 먼저 결정**

선택할 수 있는 환경이 많습니다. 예를 들어 구매 주문 흐름이 사용 하 여 단순화할 수 있습니다 [Api 수익 화](/windows/uwp/monetize), 또는 [응용 프로그램에 주목해](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts) 흥미로운 공유에 있는 경우와 같은 새 그림는 다른 사용자가 게시 됩니다.

![알림](images/desktop-to-uwp/toast.png)

사용자가 메시지를 무시 또는 해제하는 경우에도, 알림 센터에서 다시 메시지를 확인한 후 메시지를 클릭하여 앱을 열 수 있습니다. 응용 프로그램을 사용 하 여 engagement 늘어나고 운영 체제와 긴밀히 통합 표시 하는 응용 프로그램의 기회가 추가로 있습니다. 보여드리겠습니다 해당 환경에 대 한 코드를이 문서의 뒷부분 합니다.

방문 합니다 [UWP 설명서](/windows/uwp/get-started/) 자세한 아이디어에 대 한 합니다.

: white_check_mark: **강화 또는 확장 여부를 결정합니다**

조건에 사용 하 여 종종 알 수 있습니다 *향상* 하 고 *확장*이므로 평균 정확히 어떤 이러한 각 조건 설명 하기 위해 잠시 살펴보겠습니다.

이라는 용어 *향상* (여부 하기로 MSIX 패키지의 응용 프로그램 패키지) 데스크톱 앱에서 직접 호출할 수 있는 Windows 런타임 Api를 설명 합니다. Windows 10 환경에 선택한 경우에 만든 다음 해당 API에 나타나는지를 확인 해야 하는 Api를 식별 [이 목록](desktop-to-uwp-supported-api.md)합니다. 데스크톱 앱에서 직접 호출할 수 있는 Api의 목록입니다. API와 연결된 기능을 UWP 프로세스에서만 실행시킬 수 있다면, 이 목록에 API가 표시되지 않습니다. 대체로 이러한 Api는 UWP 지도 컨트롤 또는 Windows Hello 보안 프롬프트와 같은 UWP XAML을 렌더링 하는 포함 됩니다.

> [!NOTE]
> 일반적으로 UWP XAML을 렌더링 하는 Api는 바탕 화면에서 직접 호출할 수 없습니다, 하지만 대체 방법을 사용할 수 있습니다. UWP XAML 컨트롤 또는 다른 사용자 지정 시각적 개체 환경을 호스트 하려는 경우 사용할 수 있습니다 [XAML 제도](xaml-islands.md) (Windows 10 버전 1903부터) 및 [시각적 계층](visual-layer-in-desktop-apps.md) (Windows 10, 버전 1803에서에서 시작)입니다. 패키지 또는 패키지 되지 않은 데스크톱 앱에서 이러한 기능을 사용할 수 있습니다.

다른 옵션으로 MSIX 패키지에서 데스크톱 앱 패키지를 선택한 경우 *확장* UWP 프로젝트를 솔루션에 추가 하 여 응용 프로그램입니다. 데스크톱 프로젝트는 여전히 응용 프로그램의 진입점 이지만 UWP 프로젝트에서 표시 되지 않는 Api의 모든 액세스를 제공 [이 목록](desktop-to-uwp-supported-api.md)합니다. 데스크톱 응용 프로그램을 사용 하 여 UWP 프로세스와 통신할 수는 app service를 설정 하는 방법에 지침 많은 포함 합니다. UWP 프로젝트를 필요로 하는 환경을 추가 하려면 다음을 참조 하십시오 [UWP 구성 요소를 사용 하 여 확장](desktop-to-uwp-extend.md)합니다.

: white_check_mark: **API 참조 계약**

를 데스크톱 앱에서 직접 API를 호출 하는 경우 브라우저를 열고 해당 API에 대 한 참조 항목을 검색 합니다.
API 요약 아래에서 해당 API의 API 계약에 대한 설명한 표를 찾을 수 있습니다. 표의 예는 다음과 같습니다.

![API 계약 표](images/desktop-to-uwp/contract-table.png)

.NET 기반 데스크톱 앱을 갖고 있다면, API 계약에 참조를 추가한 후 해당 파일의 **로컬 복사** 속성을 **False**로 설정합니다. C++ 기반 프로젝트를 사용하는 경우, **Additional Include Directories** 경로를 이 계약이 포함된 폴더에 추가합니다.

: white_check_mark: **환경을 추가 하는 Api 호출**

앞서 보았던 알림 창을 표시하는 데 사용하는 코드입니다. 이러한 Api는이 나타나지 [목록](desktop-to-uwp-supported-api.md) 데스크톱 앱에이 코드를 추가 하 고 지금 바로 실행할 수 있도록 합니다.

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

새 분기를 만들고 별도 코드 베이스를 유지 관리 하지 않고도 Windows 10 용 응용 프로그램을 현대화 할 수 있습니다.

Windows 10 사용자를 위해 별도 바이너리를 빌드하고 싶다면 조건부 컴파일을 사용합니다. 모든 Windows 사용자에게 배포할 바이너리들을 빌드하고 싶다면 런타임 검사를 사용하세요.

간략히 각 옵션을 살펴보겠습니다.

### <a name="conditional-compilation"></a>조건부 컴파일

하나의 코드 기반을 유지, Windows 10 사용자를 위한 바이너리 세트를 컴파일할 수 있습니다.

먼저 프로젝트에 새 빌드 구성을 추가합니다.

![빌드 구성](images/desktop-to-uwp/build-config.png)

해당 빌드 구성에 대 한 만들기 상수 Windows 런타임 Api를 호출 하는 코드를 식별 하는 합니다.  

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

실행 중인 Windows 버전에 관계없이 모든 Windows 사용자를 위한 바이너리 세트를 컴파일 할 수 있습니다. 응용 프로그램 호출 Windows 런타임 Api는 사용자가 실행 하는 경우에 응용 프로그램 패키지 응용 프로그램으로 Windows 10에서 합니다.

코드에 런타임 검사를 추가 하는 가장 쉬운 방법은 다음과 같습니다.이 Nuget 패키지를 설치 하려면 [데스크톱 브리지 도우미](https://www.nuget.org/packages/DesktopBridge.Helpers/) 사용 하 여는 ``IsRunningAsUWP()`` Windows 런타임 Api를 호출 하는 모든 코드는 게이트 방법입니다. 이 블로그 게시물에 대 한 자세한 내용은 참조 하세요. [데스크톱 브리지-응용 프로그램의 컨텍스트를 식별](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/)합니다.

## <a name="related-samples"></a>관련 샘플

* [Hello World 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [보조 타일](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [저장소 API 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [UWP UpdateTask를 구현 하는 WinForms 응용 프로그램](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [UWP 샘플 데스크톱 앱 연결](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="support-and-feedback"></a>지원 및 피드백

**질문에 답변**

질문이 있으세요? Stack Overflow에서 질문해 주세요. 저희 팀은 이러한 [태그](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다. [여기](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)에서 Microsoft에 문의할 수도 있습니다.

**피드백 제공 또는 기능 제안**

[UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)를 참조하세요.
