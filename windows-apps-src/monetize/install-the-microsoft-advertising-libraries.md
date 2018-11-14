---
author: Xansky
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: Microsoft Advertising SDK를 설치하는 방법을 알아봅니다.
title: Microsoft Advertising SDK 설치
ms.author: mhopkins
ms.date: 08/23/2017
ms.topic: article
keywords: Windows 10, uwp, 광고, 설치, SDK, 광고 라이브러리
ms.localizationpriority: medium
ms.openlocfilehash: c08acaf3f5d4ddf59121e68782c0e8b78e8c358e
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6469698"
---
# <a name="install-the-microsoft-advertising-sdk"></a>Microsoft Advertising SDK 설치

Windows 10용 UWP 앱에 광고를 표시하려면 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)를 설치합니다. 이 SDK는 Visual Studio 2015 이상 버전의 확장입니다.

> [!NOTE]
> 개발 하는 경우 JavaScript/HTML UWP 앱을 설치한 Windows 10 SDK 버전 10.0.14393 (1 주년 업데이트) 또는 나중에, [WinJS](https://github.com/winjs/winjs) 라이브러리도 설치 해야 합니다. 이전 Windows 10 SDK 버전에는 이 라이브러리가 포함되어 있었지만, 10.0.14393(1주년 업데이트) 버전부터는 라이브러리를 별도로 설치해야 합니다.

<span id="install-msi" />

## <a name="install-via-msi"></a>MSI를 통해 설치

MSI 설치 관리자를 통해 Microsoft Advertising Services SDK를 설치:

1.  모든 Visual Studio 인스턴스를 닫습니다.

2. 이전에 Microsoft Advertising SDK, Universal Ad Client SDK, Ad Mediator 확장 또는 Microsoft Store Engagement and Monetization SDK의 이전 버전을 설치한 경우 지금 이러한 SDK 버전을 제거합니다. 또 **명령 프롬프트** 창을 열고 다음 명령을 실행하여 Visual Studio와 함께 설치되었을 수 있으나 컴퓨터에 설치된 프로그램 목록에 나타나지 않을 수 있는 이전 광고 SDK 버전을 모두 정리할 수 있습니다.
    ```
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)를 다운로드하여 설치합니다. 설치하는 데 몇 분 정도 걸릴 수 있습니다. 프로세스가 완료되었는지 확인하고 완료될 때까지 기다립니다.

4.  Visual Studio를 다시 시작합니다.

5.  이전 버전의 Microsoft Advertising SDK, Universal Ad Client SDK 또는 Microsoft Store Engagement and Monetization SDK에 있는 광고 라이브러리를 참조하는 기존 프로젝트가 있는 경우 Visual Studio에서 프로젝트를 열고 프로젝트를 정리한 후 다시 빌드합니다(**솔루션 탐색기**에서 프로젝트 노드를 마우스 오른쪽 단추로 클릭하고 **정리**를 선택한 다음 프로젝트 노드를 다시 마우스 오른쪽 단추로 클릭하고 **다시 빌드** 선택).

  기타 프로젝트에서 처음 Microsoft Advertising SDK를 사용하는 경우에도, 이제 [Microsoft Advertising SDK에 참조를 추가](#reference)할 수 있습니다.

<span id="install-nuget" />

## <a name="install-via-nuget"></a>NuGet을 통해 설치

NuGet을 통해 특정 UWP 프로젝트에서 Microsoft Advertising SDK 설치:

1.  모든 Visual Studio 인스턴스를 닫습니다.

2.  이전에 Microsoft Advertising SDK, Universal Ad Client SDK, Ad Mediator 확장 또는 Microsoft Store Engagement and Monetization SDK의 이전 버전을 설치한 경우 지금 이러한 SDK 버전을 제거합니다. 또 **명령 프롬프트** 창을 열고 다음 명령을 실행하여 Visual Studio와 함께 설치되었을 수 있으나 컴퓨터에 설치된 프로그램 목록에 나타나지 않을 수 있는 이전 광고 SDK 버전을 모두 정리할 수 있습니다.
    ```
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Visual Studio를 시작하고 Microsoft Advertising SDK 라이브러리를 사용하려는 프로젝트를 엽니다.
    > [!NOTE]
    > 프로젝트에 이 SDK의 이전 MSI 설치에 포함된 라이브러리 참조가 이미 들어 있으면 프로젝트에서 이러한 참조를 제거합니다. 이러한 참조는 참조하는 라이브러리가 이전 단계에서 제거되었으므로 옆에 경고 아이콘이 표시됩니다.

4. Visual Studio에서 **프로젝트** 및 **NuGet 패키지 관리**를 클릭합니다.

5. 검색 상자에 **Microsoft.Advertising.XAML**(XAML 프로젝트) 또는 **Microsoft.Advertising.JS**(JavaScript/HTML 프로젝트)를 입력하고, 해당 패키지를 설치합니다. 패키지 설치가 완료되면 솔루션을 저장합니다.
    > [!NOTE]
    > **출력** 창이 지정된 경로가 너무 길다는 것을 나타내는 *Install-Package* 오류를 보고하는 경우, 기본 위치보다 경로가 더 짧은 다른 위치로 패키지를 추출하도록 NuGet을 구성해야 할 수 있습니다. 이렇게 하려면 컴퓨터의 nuget.config 파일에 ```repositoryPath``` 값을 추가하고 NuGet 패키지를 추출할 수 있는 더 짧은 폴더에 할당합니다. 자세한 내용은 NuGet 설명서에서 [이 문서](http://docs.nuget.org/ndocs/consume-packages/configuring-nuget-behavior)를 참조하세요. 또는 Visual Studio 프로젝트를 더 짧은 경로의 대체 폴더로 이동하려고 할 수 있습니다.

6. 솔루션을 닫았다가 다시 엽니다.

7.  프로젝트가 NuGet를 통해 설치된 이전 버전의 Microsoft Advertising SDK에 있는 라이브러리를 이미 참조하며 프로젝트를 최신 버전의 SDK로 업데이트한 경우 프로젝트를 정리한 후 다시 빌드하는 것이 좋습니다(**솔루션 탐색기**에서 프로젝트 노드를 마우스 오른쪽 단추로 클릭하고 **정리**를 선택한 다음 프로젝트 노드를 다시 마우스 오른쪽 단추로 클릭하고 **다시 빌드** 선택).

  기타 프로젝트에서 처음 SDK를 사용하는 경우에도, 이제 [Microsoft Advertising SDK에 참조를 추가](#reference)할 수 있습니다.

<span id="reference" />

## <a name="add-a-reference-to-the-microsoft-advertising-sdk"></a>Microsoft Advertising SDK에 참조 추가

Microsoft Advertising SDK를 설치한 후에는 다음 지침에 따라 광고 API를 사용하도록 프로젝트에서 SDK를 참조시킵니다.

1. Visual Studio에서 프로젝트를 엽니다.
    > [!NOTE]
    > 프로젝트의 대상이 **모든 CPU**인 경우 아키텍처별 빌드 출력(예: **x86**)을 사용하도록 프로젝트를 업데이트합니다. 프로젝트의 대상이 **모든 CPU**인 경우 다음 단계에 따라 Microsoft Advertising SDK에 대한 참조를 성공적으로 추가하지 못할 수 있습니다. 자세한 내용은 [프로젝트에서 모든 CPU를 대상으로 할 경우 발생하는 참조 오류](known-issues-for-the-advertising-libraries.md#reference_errors)를 참조하세요.

2. **솔루션 탐색기**에서 **참조**를 마우스 오른쪽 단추로 클릭한 다음 **참조 추가...** 를 선택합니다.

3. **참조 관리자**에서 **유니버설 Windows**를 확장하고, **확장**을 클릭한 다음 **Microsoft Advertising SDK for XAML** (XAML 앱인 경우) 또는 **Microsoft Advertising SDK for JavaScript**(JavaScript 및 HTML을 사용하여 빌드되는 앱인 경우) 옆에 있는 확인란을 선택합니다.

4.  **참조 관리자**에서 확인을 클릭합니다.

광고 API 시작 방법을 보여 주는 연습은 다음 문서를 참조하세요.

* [중간 광고](interstitial-ads.md)
* [기본 광고](native-ads.md)
* [XAML 및 .NET의 AdControl](adcontrol-in-xaml-and--net.md)
* [HTML 5 및 JavaScript의 AdControl](adcontrol-in-html-5-and-javascript.md)

<span id="framework" />

## <a name="understanding-framework-packages-in-the-microsoft-advertising-sdk"></a>Microsoft Advertising SDK의 프레임워크 패키지 이해

UWP 앱용 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)의 Microsoft.Advertising.dll 라이브러리는 *프레임워크 패키지*로 구성됩니다. 이 라이브러리에는 [Microsoft.Advertising](https://docs.microsoft.com/uwp/api/microsoft.advertising) 및 [Microsoft.Advertising.WinRT.UI](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui) 네임스페이스의 광고 API가 포함되어 있습니다.

이 라이브러리는 프레임워크 패키지입니다. 다시 말해서 사용자가 이 라이브러리를 사용하는 앱 버전을 설치하면 수정 및 성능 향상이 포함된 새 버전의 라이브러리가 게시될 때마다 Windows 업데이트를 통해 사용자 디바이스의 라이브러리가 자동으로 업데이트됩니다. 따라서 고객의 디바이스에 항상 사용 가능한 최신 버전의 라이브러리가 설치됩니다.

이 라이브러리에 새로운 API 또는 기능을 도입하는 새 버전의 SDK를 릴리스하는 경우 이러한 기능을 사용하려면 최신 버전의 SDK를 설치해야 합니다. 이 시나리오에서는 업데이트된 앱도 스토어에 게시해야 합니다.
