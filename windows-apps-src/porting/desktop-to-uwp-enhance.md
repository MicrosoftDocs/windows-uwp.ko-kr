---
author: normesta
Description: Enhance your desktop application for Windows 10 users by using Universal Windows Platform (UWP) APIs.
Search.Product: eADQiWindows 10XVcnh
title: Windows 10용 데스크톱 응용 프로그램 개선
ms.author: normesta
ms.date: 08/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 392f8166e16c028a57bc9e27039a9884f1d9714a
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4267768"
---
# <a name="enhance-your-desktop-application-for-windows-10"></a>Windows 10용 데스크톱 응용 프로그램 개선

UWP API를 사용, Windows 10 사용자가 만족할 최신 환경을 추가할 수 있습니다.

먼저 프로젝트를 설정합니다. 그리고 Windows 10 환경을 추가합니다. Windows 10 사용자를 위해 별도로 빌드하거나, 실행하는 Windows 버전에 상관 없이 모든 사용자에게 동일한 바이너리를 배포할 수 있습니다.

## <a name="first-set-up-your-project"></a>먼저 프로젝트를 설정합니다.

프로젝트에서 몇 가지를 변경해야 UWP API를 사용할 수 있습니다.

### <a name="modify-a-net-project-to-use-uwp-apis"></a>UWP API를 사용하기 위해 .NET 프로젝트를 수정합니다.

**참조 관리자** 대화 상자를 연 후, **찾아보기**와 **모든 파일**을 차례대로 선택합니다.

![참조 대화 상자 추가](images/desktop-to-uwp/browse-references.png)

그리고 이러한 파일에 참조를 추가합니다.

|파일|위치|
|--|--|
|System.Runtime.WindowsRuntime|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\\.NETCore\v4.5|
|System.Runtime.WindowsRuntime.UI.Xaml|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\\.NETCore\v4.5|
|System.Runtime.InteropServices.WindowsRuntime|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\\.NETCore\v4.5|
|Windows.winmd|C:\Program Files (x86)\Windows Kits\10\UnionMetadata\Facade|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk version*>\Windows.Foundation.UniversalApiContract\<*version*>|
|Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk version*>\Windows.Foundation.FoundationContract\<*version*>|

**속성** 창에서 각 *.winmd* 파일의 **로컬 복사** 필드를 **False**로 설정합니다.

![copy-local-field](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-project-to-use-uwp-apis"></a>UWP API를 사용할 수 있도록 C++를 수정

프로젝트의 속성 페이지를 엽니다.

**C/C++** 설정 그룹의 **일반** 설정에서 **Windows 런타임 확장 사용** 필드를 **예(/ZW)** 로 설정합니다.

   ![Windows 런타임 확장 사용](images/desktop-to-uwp/consume-runtime-extensions.png)

**Additional #using Directories** 대화 상자를 열어 이러한 디렉터리를 추가합니다.

* %VSInstallDir%\Common7\IDE\VC\vcpackages
* C:\Program Files (x86)\Windows Kits\10\UnionMetadata
* C:\Program Files (x86)\Windows Kits\10\References\Windows.Foundation.UniversalApiContract\<*latest version*>
* C:\Program Files (x86)\Windows Kits\10\References\Windows.Foundation.FoundationContract\<*latest version*>

**Additional Include Directories** 대화 상자를 열어 이 디렉토리를 추가합니다. C:\Program Files (x86)\Windows Kits\10\Include\<*latest version*>\um

![디렉터리를 포함한 추가](images/desktop-to-uwp/additional-include.png)

**C/C++** 설정 그룹의 **코드 생성**에서 **최소 다시 빌드 사용**설정을 **아니요(/GM-)** 로 설정합니다.

![최소 다시 빌드 사용](images/desktop-to-uwp/disable-min-build.png)


## <a name="add-windows-10-experiences"></a>Windows 10 환경 추가

사용자가 Windows 10에서 응용 프로그램을 실행할 때 만족을 주는 최신 환경을 추가할 준비가 되었습니다. 이 디자인 흐름을 사용합니다.

:white_check_mark: **먼저 추가하고 싶은 환경을 결정합니다.**

선택할 수 있는 환경이 많습니다. 예를 들어 있는 다른 사용자가 게시 하는 새 사진 같이 공유 하면 흥미로운 것 수익 창출 Api, 또는 응용 프로그램에 직접 주의 사용 하 여 구매 주문 흐름을 단순화할 수 있습니다.

![알림](images/desktop-to-uwp/toast.png)

사용자가 메시지를 무시 또는 해제하는 경우에도, 알림 센터에서 다시 메시지를 확인한 후 메시지를 클릭하여 앱을 열 수 있습니다. 응용 프로그램 참여를 증가 하 고 더 깊이 통합 된 운영 체제를 표시 하는 응용 프로그램을 만드는 추가 효과가 있습니다. 잠시 후에 해당 환경을 위한 코드를 제시하겠습니다.

아이디어를 얻으려면 [개발자 센터](https://developer.microsoft.com/windows)를 방문합니다.

:white_check_mark: **강화 또는 확장 여부 결정**

저희는 '강화'와 '확장'이라는 용어를 자주 사용합니다. 각 용어의 의미를 정확히 설명하겠습니다.

저희는 데스크톱 응용 프로그램에서 직접 호출할 수 있는 UWP API를 설명하기 위해 '강화'라는 용어를 사용합니다. Windows 10 환경을 선택했을 때, 생성해야 할 API를 식별하고, 해당 API가 이 [목록](desktop-to-uwp-supported-api.md)에 표시되는지 확인합니다. 이 목록은 데스크톱 응용 프로그램에서 직접 호출할 수 있는 AIP들입니다. API와 연결된 기능을 UWP 프로세스에서만 실행시킬 수 있다면, 이 목록에 API가 표시되지 않습니다. 종종 여기에 UWP 지도 컨트롤이나 Windows Hello 보안 프롬프트 같은 최신 UI를 표시하는 API가 포함됩니다.

이런 환경을 응용 프로그램에 포함시키고 싶다면, UWP 프로젝트를 솔루션에 추가해 응용 프로그램을 확장하면 됩니다. 데스크톱 프로젝트가 여전히 응용 프로그램의 진입점입니다. 그러나 UWP 프로젝트는 여기 [목록](desktop-to-uwp-supported-api.md)에 표시되지 않는 API에 액세스할 수 있도록 도와줍니다. 데스크톱 응용 프로그램은 앱 서비스를 사용하여 UWP 프로세스와 통신할 수 있습니다. 저희는 이를 설정하는 방법에 대한 많은 지침을 갖고 있습니다. UWP 프로젝트가 필요한 환경을 추가하고 싶다면 [UWP로 확장](desktop-to-uwp-extend.md)을 참조하세요.

:white_check_mark: **참조 API 계약**

데스크톱 응용 프로그램에서 직접 API를 호출할 수 있다면, 브라우저를 열어 해당 API의 참조 항목을 검색합니다.
API 요약 아래에서 해당 API의 API 계약에 대한 설명한 표를 찾을 수 있습니다. 표의 예는 다음과 같습니다.

![API 계약 표](images/desktop-to-uwp/contract-table.png)

.NET 기반 데스크톱 앱을 갖고 있다면, API 계약에 참조를 추가한 후 해당 파일의 **로컬 복사** 속성을 **False**로 설정합니다. C++ 기반 프로젝트를 사용하는 경우, **Additional Include Directories** 경로를 이 계약이 포함된 폴더에 추가합니다.

:white_check_mark: **환경을 추가하는 API 호출**

앞서 보았던 알림 창을 표시하는 데 사용하는 코드입니다. 이런 API가 여기 [목록](desktop-to-uwp-supported-api.md)에 표시되어야, 데스크톱 응용 프로그램에 이 코드를 추가해 실행할 수 있습니다.

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

새 지점을 만들어 별도 코드 기반을 유지 하 고 필요 없이 Windows 10 용 응용 프로그램을 현대화 할 수 있습니다.

Windows 10 사용자를 위해 별도 바이너리를 빌드하고 싶다면 조건부 컴파일을 사용합니다. 모든 Windows 사용자에게 배포할 바이너리들을 빌드하고 싶다면 런타임 검사를 사용하세요.

간략히 각 옵션을 살펴보겠습니다.

### <a name="conditional-compilation"></a>조건부 컴파일

하나의 코드 기반을 유지, Windows 10 사용자를 위한 바이너리 세트를 컴파일할 수 있습니다.

먼저 프로젝트에 새 빌드 구성을 추가합니다.

![빌드 구성](images/desktop-to-uwp/build-config.png)

해당 빌드 구성에서 UWP API를 호출하는 코드를 식별하는 상수를 만듭니다.  

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

실행 중인 Windows 버전에 관계없이 모든 Windows 사용자를 위한 바이너리 세트를 컴파일 할 수 있습니다. 응용 프로그램 패키지 된 응용 프로그램으로 Windows 10에서 실행 하는 응용 프로그램이 UWP Api는 사용자가 있는 경우에 호출 합니다.

가장 쉽게 런타임 검사를 코드에 추가하는 방법은 Nuget 패키지: [데스크톱 브리지 도우미](https://www.nuget.org/packages/DesktopBridge.Helpers/)를 설치하고, ``IsRunningAsUWP()``모든 UWP 코드 해제 메서드를 사용하는 것입니다. 자세한 내용은 [Desktop Bridge - Identify the application's context](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/)라는 블로그를 참조하세요.

## <a name="related-video"></a>관련 비디오

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Use-UWP-APIs-in-Your-Code-3d78c6WhD_9506218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="related-samples"></a>관련 샘플

* [Hello World 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [보조 타일](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Microsoft Store API 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [UWP UpdateTask를 구현 하는 WinForms 응용 프로그램](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [데스크톱 앱-UWP 브리지 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)


## <a name="support-and-feedback"></a>지원 및 피드백

**질문에 대한 답변 찾기**

질문이 있으세요? Stack Overflow에서 질문해 주세요. 저희 팀은 이러한 [태그](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다. [여기](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)에서 Microsoft에 문의할 수도 있습니다.

**피드백 제공 또는 기능 제안**

[UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)를 참조하세요.
