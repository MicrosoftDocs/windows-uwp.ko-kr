---
author: mcleanbyron
description: "앱을 업데이트하여 지원되는 최신 Microsoft 광고 라이브러리를 사용하고 앱이 배너 광고를 계속 받도록 하는 방법을 알아봅니다."
title: "최신 Advertising 라이브러리로 앱 업데이트"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 광고, 광고, AdControl, AdMediatorControl, 마이그레이션"
ms.assetid: f8d5b2ad-fcdb-4891-bd68-39eeabdf799c
ms.openlocfilehash: 25435ebf314327db7288ac853819c90ebba35669
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="update-your-app-to-the-latest-advertising-libraries"></a>최신 Advertising 라이브러리로 앱 업데이트

Microsoft Advertising의 배너 광고를 표시하는 앱이 2017년 4월 1일 이후 배너 광고를 계속 받으려면 다음 SDK 중 하나의 **AdControl** 또는 **AdMediatorControl**을 사용해야 합니다.

  * [Microsoft Store Services SDK](http://aka.ms/store-services-sdk)(UWP 앱의 경우)
  * [Microsoft Advertising SDK for Windows 및 Windows Phone 8.x](http://aka.ms/store-8-sdk)(Windows 8.1 및 Windows Phone 8.x 앱의 경우)

이러한 SDK가 제공되기 전에 Microsoft에서는 Windows 및 Windows Phone 앱을 위한 여러 가지 이전 광고 SDK 릴리스에서 이러한 컨트롤을 출시했습니다. 이러한 이전 광고 SDK 릴리스는 더 이상 지원되지 않습니다. 2017년 4월 1일 이후 Microsoft는 추가 경고 없이 언제든지 이전 광고 SDK 릴리스를 사용하는 앱에 대한 배너 광고 서비스를 중지할 수 있습니다.

**AdControl** 또는 **AdMediatorControl**을 사용하여 배너 광고를 표시하는 기존 앱(스토어에 이미 있거나 아직 개발 중인 앱)이 있다면 이 문서의 지침을 따라 해당 앱이 이 변경에 영향을 받는지 확인하고 필요할 경우 앱을 업데이트하는 방법을 알아보세요.

>**참고**&nbsp;&nbsp;또한 2017년 4월 1일 이후에는 둘 이상의 앱에서 사용되는 모든 광고 단위에 대한 배너 광고 서비스도 중지됩니다. 이러한 변경에 대비하려면 광고 단위가 각각 하나의 앱에서만 사용되는지 확인하세요.

## <a name="more-details-about-this-change"></a>이 변경에 대한 세부 정보

이러한 변경에 대한 추가 컨텍스트를 제공하기 위해 IAB(Interactive Advertising Bureau)의 [MRAID(Mobile Rich-media Ad Interface Definitions) 1.0 사양](http://www.iab.com/wp-content/uploads/2015/08/IAB_MRAID_VersionOne.pdf)을 통해 HTML5 리치 미디어를 제공하는 기능을 비롯한 최소 기능 집합을 지원하지 않는 이전 광고 SDK 릴리스에 대한 지원이 제거될 예정입니다. 많은 광고주가 이러한 기능을 원하고 있으므로 Microsoft는 광고주들에게 앱 에코시스템을 더욱 매력적으로 어필하고 사용자가 더 많은 수익을 보도록 이러한 변경을 진행하고 있습니다.

이 변경에 영향을 받는 앱을 대상 플랫폼의 최신 광고 SDK를 사용하도록 업데이트하지 않을 경우 Microsoft에서 지원되지 않는 광고 SDK 릴리스를 사용하는 앱에 대한 배너 광고 서비스를 중지하면 다음과 같은 동작이 나타납니다.

* 앱의 **AdControl** 또는 **AdMediatorControl** 컨트롤에 배너 광고가 더 이상 제공되지 않을 것이며 해당 컨트롤에서 광고 수익을 얻지 못합니다.

* 앱의 **AdControl** 또는 **AdMediatorControl**이 새 광고를 요청하는 경우 컨트롤의 **ErrorOccurred** 이벤트가 발생하고 이벤트 인수의 **ErrorCode** 속성이 **NoAdAvailable** 값을 갖게 됩니다.

문제가 발생하거나 지원이 필요할 경우 [고객 지원 센터에 문의](http://go.microsoft.com/fwlink/?LinkId=393643)하세요.

>**참고**&nbsp;&nbsp;앱에서 이미 [Microsoft Store Services SDK](http://aka.ms/store-services-sdk)(UWP 앱의 경우) 또는 [Microsoft Advertising SDK for Windows 및 Windows Phone 8.x](http://aka.ms/store-8-sdk)(Windows 8.1 및 Windows Phone 8.x 앱의 경우)를 사용하고 있거나 이러한 SDK 중 하나를 사용하도록 이전에 앱을 업데이트한 경우에는 이미 앱에서 최신 SDK를 사용하고 있으므로 추가로 앱을 변경할 필요가 없습니다.

## <a name="prerequisites"></a>필수 조건

* **AdControl** 또는 **AdMediatorControl**을 사용하는 앱에 대한 전체 소스 코드 및 Visual Studio 프로젝트 파일

* 앱에 대한 .appx 또는 .xap 패키지

  >**참고**&nbsp;&nbsp;앱에 대한 .appx 또는 .xap 패키지가 더 이상 없으나 Visual Studio 버전의 개발 컴퓨터가 있고 앱 빌드에 사용했던 광고 SDK가 있는 경우 Visual Studio에서 .appx 또는 .xap 패키지를 다시 생성할 수 있습니다.

<span id="part-1" />
## <a name="part-1-determine-whether-you-need-to-update-your-app"></a>1부: 앱을 업데이트해야 하는지 여부 결정

다음 섹션의 지침에 따라 앱을 업데이트해야 하는지 결정합니다.

### <a name="your-app-uses-adcontrol"></a>앱에 AdControl이 사용되는 경우

앱이 **AdControl**을 사용하여 배너 광고를 표시하는 경우 다음 지침을 따르세요.

**Windows 10용 UWP 앱**

1. 원본을 손상시키지 않기 위해 앱용 .appx 패키지의 복사본을 만들고 .zip 확장명으로 이름을 바꾸고 파일의 압축을 풉니다.

2. 앱 패키지의 압축 푼 내용을 확인합니다.

  * Microsoft.Advertising.dll 파일이 있으면 앱이 이전 SDK를 사용하는 것이므로 아래 섹션의 지침에 따라 프로젝트를 업데이트해야 합니다. [2부](update-your-app-to-the-latest-advertising-libraries.md#part-2)를 계속 진행합니다.

  * Microsoft.Advertising.dll 파일이 없으면 UWP 앱이 사용 가능한 최신 광고 SDK를 이미 사용하고 있는 것이므로 프로젝트를 변경할 필요가 없습니다.

<span/>

**Windows8.1 또는 Windows Phone 8.x 앱**

1. 원본을 손상시키지 않기 위해 앱용 .appx 또는 .xap 패키지의 복사본을 만들고 .zip 확장명으로 이름을 바꾸고 파일의 압축을 풉니다.

2. XAML 또는 JavaScript/HTML 앱의 경우 앱 패키지의 압축 푼 내용을 확인합니다.

  * Microsoft.Advertising.winmd 파일이 있으나 UniversalXamlAdControl.\*.dll 파일(XAML 앱용) 또는 UniversalSharedLibrary.Windows.dll 파일(JavaScript/HTML 앱용)은 없으면 앱은 이전 SDK를 사용하는 것이며 아래 섹션의 지침에 따라 프로젝트를 업데이트해야 합니다. [2부](update-your-app-to-the-latest-advertising-libraries.md#part-2)를 계속 진행합니다.

  * 그러지 않으면 다음 단계를 계속 진행합니다.

2. Windows PowerShell을 열고 다음 명령을 입력하고 앱 패키지의 압축 푼 내용에 대한 전체 경로에 ```-Path``` 인수를 할당합니다. 이 명령은 프로젝트에서 참조되는 모든 광고 라이브러리와 각 라이브러리의 버전을 표시합니다.

  > [!div class="tabbedCodeSnippets"]
  ```syntax
  get-childitem -Path "<path to your extracted package>" * -Recurse -include *advert*.dll,*admediator*.dll,*xamladcontrol*.dll,*universalsharedlibrary*.dll | where-object {$_.Name -notlike "*resources*" -and $_.Name -notlike "*design*" } | foreach-object { "{0}`t{1}" -f $_.FullName, [System.Diagnostics.FileVersionInfo]::GetVersionInfo($_).FileVersion }
  ```

2. 다음 표에서 앱의 대상 플랫폼에 해당하는 파일을 찾고 해당 파일 버전을 표에 나열된 버전과 비교합니다.

  <table>
    <colgroup>
      <col width="33%" />
      <col width="33%" />
      <col width="33%" />
    </colgroup>
    <thead>
      <tr class="header">
        <th align="left">대상 플랫폼</th>
        <th align="left">Files(파일)</th>
        <th align="left">버전</th>
      </tr>
    </thead>
    <tbody>
      <tr class="odd">
        <td align="left"><p>Windows8.1 XAML</p></td>
        <td align="left"><p>UniversalXamlAdControl.Windows.dll</p></td>
        <td align="left"><p>8.5.1601.07018</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>Windows Phone 8.1 XAML</p></td>
        <td align="left"><p>UniversalXamlAdControl.WindowsPhone.dll</p></td>
        <td align="left"><p>8.5.1601.07018</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>Windows8.1 JavaScript/HTML<br/>Windows Phone 8.1 JavaScript/HTML</p></td>
        <td align="left"><p>UniversalSharedLibrary.Windows.dll</p></td>
        <td align="left"><p>8.5.1601.07018</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>Windows Phone 8.1 Silverlight</p></td>
        <td align="left"><p>Microsoft.Advertising.\*.dll</p></td>
        <td align="left"><p>8.1.50112.0</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>Windows Phone 8.0 Silverlight</p></td>
        <td align="left"><p>Microsoft.Advertising.\*.dll</p></td>
        <td align="left"><p>6.2.40501.0</p></td>
      </tr>
    </tbody>
  </table>

3. 파일 버전이 이전 표에 나열된 버전 이상인 경우 프로젝트를 변경할 필요가 없습니다.

  파일 버전이 더 낮은 버전이면 아래 섹션의 지침에 따라 프로젝트를 업데이트해야 합니다. [2부](update-your-app-to-the-latest-advertising-libraries.md#part-2)를 계속 진행합니다.

<span/>

**Windows8.0 앱**

* 향후 Windows8.0을 대상으로 하는 앱에는 더 이상 배너 광고가 제공되지 않을 수 있습니다. 광고 노출 손실을 방지하려면 프로젝트를 Windows 10을 대상으로 하는 UWP 앱으로 변환하는 것이 좋습니다. 대부분의 Windows8.0 앱 트래픽은 Windows10 디바이스에서 실행됩니다.

<span/>

**Windows Phone 7.x 앱**

* WindowsPhone 7.x를 대상으로 하는 앱에는 나중에 더 이상 배너 광고가 제공되지 않습니다. 광고 노출 손실을 방지하려면 프로젝트를 Windows Phone 8.1 앱을 대상으로 하도록 변환하거나 Windows 10을 대상으로 하는 UWP 앱으로 변환하는 것이 좋습니다. 대부분의 Windows7.x 앱 트래픽은 Windows Phone 8.1 또는 Windows10 디바이스에서 실행됩니다.

<span/>

### <a name="your-app-uses-admediatorcontrol"></a>앱이 AdMediatorControl을 사용하는 경우

앱이 **AdMediatorControl**을 사용하여 배너 광고를 표시하는 경우 다음 지침에 따라 앱을 업데이트해야 하는지 여부를 결정합니다.

**Windows 10용 UWP 앱**

* **AdMediatorControl** 은 UWP 앱에서 더 이상 지원되지 않습니다. 아래 섹션의 지침에 따라 **AdControl**을 사용하는 방식으로 마이그레이션해야 합니다. [2부](update-your-app-to-the-latest-advertising-libraries.md#part-2)를 계속 진행합니다.

<span/>

**Windows8.1 또는 Windows Phone 8.1 앱**

1. 원본을 손상시키지 않기 위해 앱용 .appx 또는 .xap 패키지의 복사본을 만들고 .zip 확장명으로 이름을 바꾸고 파일의 압축을 풉니다.

2. Windows PowerShell을 열고 다음 명령을 입력하고 앱 패키지의 압축 푼 내용에 대한 전체 경로에 ```-Path``` 인수를 할당합니다. 이 명령은 프로젝트에서 참조되는 모든 광고 라이브러리와 각 라이브러리의 버전을 표시합니다.

  > [!div class="tabbedCodeSnippets"]
  ```syntax
  get-childitem -Path "<path to your extracted package>" * -Recurse -include *advert*.dll,*admediator*.dll,*xamladcontrol*.dll,*universalsharedlibrary*.dll | where-object {$_.Name -notlike "*resources*" -and $_.Name -notlike "*design*" } | foreach-object { "{0}`t{1}" -f $_.FullName, [System.Diagnostics.FileVersionInfo]::GetVersionInfo($_).FileVersion }
  ```

2. 출력에 표시된 Microsoft.AdMediator.\*.dll 파일의 버전이 2.0.1603.18005 이상이면 프로젝트를 변경할 필요가 없습니다.

  파일 버전이 더 낮은 버전이면 아래 섹션의 지침에 따라 프로젝트를 업데이트해야 합니다. [2부](update-your-app-to-the-latest-advertising-libraries.md#part-2)를 계속 진행합니다.

<span id="part-2" />
## <a name="part-2-install-the-latest-sdk"></a>2부: 최신 SDK 설치

앱이 이전 SDK 릴리스를 사용하는 경우 다음 지침에 따라 개발 컴퓨터에 최신 SDK가 있는지 확인하세요.

1. 개발 컴퓨터에 Visual Studio 2015(UWP, Windows8.1 또는 Windows Phone 8.x 프로젝트용) 또는 Visual Studio 2013(Windows8.1 또는 Windows Phone 8.x 프로젝트용)이 설치되어 있는지 확인합니다.

  >**참고**&nbsp;&nbsp;개발 컴퓨터에 Visual Studio가 열려 있으면 먼저 닫은 후 다음 단계를 수행해야 합니다.

1.    개발 컴퓨터에서 Microsoft Advertising SDK 및 Ad Mediator SDK의 모든 이전 버전을 제거합니다.

2.    **명령 프롬프트** 창을 열고 다음 명령을 실행하여 Visual Studio와 함께 설치되었을 수 있으나 컴퓨터에 설치된 프로그램 목록에 나타나지 않을 수 있는 SDK 버전을 모두 정리합니다.

  > [!div class="tabbedCodeSnippets"]
  ```syntax
  MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
  MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
  MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
  ```

3.    앱용 최신 SDK를 설치합니다.
  * Windows 10용 UWP 앱의 경우 [Microsoft Store Services SDK](http://aka.ms/store-services-sdk)를 설치합니다.
  * 이전 OS 버전을 대상으로 하는 앱의 경우 [Windows 및 Windows Phone 8.x용 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)를 설치합니다.

## <a name="part-3-update-your-project"></a>3부: 프로젝트 업데이트

다음 지침에 따라 프로젝트를 업데이트합니다.

### <a name="uwp-projects-for-windows-10"></a>Windows 10용 UWP 프로젝트

<span/>

앱이 **AdMediatorControl**을 사용하는 경우 대신 [AdControl을 사용하도록 앱을 리팩터링](migrate-from-admediatorcontrol-to-adcontrol.md)합니다. **AdMediatorControl** 은 UWP 앱에서 더 이상 지원되지 않습니다.

앱이 **AdControl**을 사용하는 경우 프로젝트에서 Microsoft Advertising 라이브러리에 대한 기존의 모든 참조를 제거하고 [XAML의 AdControl](adcontrol-in-xaml-and--net.md) 또는 [HTML의 AdControl](adcontrol-in-html-5-and-javascript.md) 지침에 따라 필요한 참조를 추가합니다. 이렇게 하면 프로젝트에서 올바른 라이브러리가 사용될 수 있습니다. 기존 XAML 태그 및 코드를 유지할 수 있습니다.

<span/>

### <a name="windows-81-or-windows-phone-81-xaml-or-javascripthtml-projects"></a>Windows8.1 또는 Windows Phone 8.1(XAML 또는 JavaScript/HTML) 프로젝트

<span/>

1. 프로젝트에서 모든 Microsoft.Advertising.\* 및 Microsoft.AdMediator.\* 참조를 제거합니다. 유니버설 프로젝트 템플릿을 사용하는 경우 두 개의 참조(Windows 및 Windows Phone 각각에 대해 하나씩)가 있을 수 있습니다.

2. 앱이 **AdMediatorControl**을 사용하는 경우 [광고 조정 컨트롤 추가 및 사용](https://msdn.microsoft.com/library/windows/apps/xaml/dn864355.aspx)의 지침에 따라 라이브러리 참조를 다시 추가합니다. 앱이 **AdControl**을 사용하는 경우 [XAML의 AdControl](adcontrol-in-xaml-and--net.md) 또는 [HTML의 AdControl](adcontrol-in-html-5-and-javascript.md) 지침에 따라 라이브러리 참조를 다시 추가합니다.

<span/>

다음 사항에 유의하세요.

* 앱이 이전에 **임의 CPU** 플랫폼으로 컴파일된 경우 프로젝트를 아키텍처별 플랫폼(x86, x64 또는 ARM)으로 다시 컴파일해야 합니다.

* 이전에 **AdControl** 클래스가 **Microsoft.Advertising.Mobile.UI** 네임스페이스에 정의된 SDK 버전을 사용했던 Windows Phone 8.x XAML 앱이 있는 경우 **Microsoft.Advertising.WinRT.UI** 네임스페이스의 **AdControl** 클래스를 참조하도록 코드를 업데이트해야 합니다(이 클래스는 좀 더 최신 SDK 릴리스의 네임스페이스로 이동됨).

* 이전 문제 이외의 경우에는 기존 XAML 태그 및 코드를 유지할 수 있습니다.

<span/>

### <a name="windows-phone-8x-silverlight-projects"></a>Windows Phone 8.x Silverlight 프로젝트

<span/>

1. 프로젝트에서 모든 Microsoft.Advertising.\* 및 Microsoft.AdMediator.\* 참조를 제거합니다.

2. 앱이 **AdMediatorControl**을 사용하는 경우 [광고 조정 컨트롤 추가 및 사용](https://msdn.microsoft.com/library/windows/apps/xaml/dn864355.aspx)의 지침에 따라 라이브러리 참조를 다시 추가합니다. 앱이 **AdControl**을 사용하는 경우 [Windows Phone Silverlight의 AdControl](adcontrol-in-windows-phone-silverlight.md) 지침에 따라 라이브러리 참조를 다시 추가합니다.

<span/>

다음 사항에 유의하세요.

* 기존 XAML 태그 및 코드를 유지할 수 있습니다.

* **솔루션 탐색기**에서 프로젝트의 **Microsoft.Advertising.Mobile.UI** 참조에 대한 속성을 확인합니다. 앱이 Windows Phone 8.0을 대상으로 하는 경우 버전 6.2.40501.0이어야 하며, 앱이 Windows Phone 8.1을 대상으로 하는 경우 버전 8.1.50112.0이어야 합니다.

* Windows Phone 8.x Silverlight 앱의 경우 에뮬레이터의 프로덕션 단위 테스트는 지원되지 않습니다. 디바이스에서 테스트를 수행하는 것이 좋습니다.

## <a name="part-4-test-and-republish-your-app"></a>4단계: 앱 테스트 및 다시 게시

앱을 테스트하여 앱이 예상대로 배너 광고를 표시하는지 확인합니다.

앱의 이전 버전을 스토어에서 이미 사용할 수 있는 경우 Windows 개발자 센터 대시보드에서 업데이트된 앱에 대한 [새 제출을 만들어](../publish/app-submissions.md) 앱을 다시 게시합니다.





 
