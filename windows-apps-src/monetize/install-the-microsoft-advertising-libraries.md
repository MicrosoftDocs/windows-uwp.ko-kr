---
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: Microsoft Advertising SDK를 설치 하 여 Windows 10 용 UWP (유니버설 Windows 플랫폼) 앱에 광고를 표시 하는 방법을 알아봅니다.
title: Microsoft Advertising SDK 설치
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, 광고, 광고, 설치, SDK, 광고 라이브러리
ms.localizationpriority: medium
ms.openlocfilehash: d5c5c18c41996c5d46c261f351a900fea2532a93
ms.sourcegitcommit: 45dec3dc0f14934b8ecf1ee276070b553f48074d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89094672"
---
# <a name="install-the-microsoft-advertising-sdk"></a>Microsoft Advertising SDK 설치

>[!WARNING]
> 2020 년 6 월 1 일부 터 Windows UWP 앱 용 Microsoft Ad 수익 화 플랫폼이 종료 됩니다. [자세한 정보](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Windows 10 용 UWP 앱에서 광고를 표시 하려면 [MICROSOFT ADVERTISING SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)를 설치 합니다. 이 SDK는 Visual Studio 2015 이상 버전에 대 한 확장입니다.

> [!NOTE]
> JavaScript/HTML UWP 앱을 개발 하 고 있고 Windows 10 SDK 버전 10.0.14393 (기념일 업데이트) 이상을 설치한 경우 [WinJS](https://github.com/winjs/winjs) 라이브러리도 설치 해야 합니다. 이 라이브러리는 이전 버전의 Windows 10 SDK에 포함 되어 있지만 Windows 10 SDK 버전 10.0.14393 (기념일 업데이트) 부터는 별도로 설치 해야 합니다.

<span id="install-msi" />

## <a name="install-via-msi"></a>MSI를 통해 설치

MSI 설치 관리자를 통해 Microsoft Advertising SDK를 설치 하려면 다음을 수행 합니다.

1.  Visual Studio의 모든 인스턴스를 닫습니다.

2. 이전 버전의 Microsoft Advertising SDK, 유니버설 Ad 클라이언트 SDK, Ad Mediator 확장 또는 Microsoft Store Engagement 및 수익 화 SDK를 설치한 경우 이제 이러한 SDK 버전을 제거 합니다. 필요에 따라 **명령 프롬프트** 창을 열고 다음 명령을 실행 하 여 Visual Studio와 함께 설치 되었지만 컴퓨터의 설치 된 프로그램 목록에 표시 되지 않을 수 있는 이전 광고 SDK 버전을 정리 합니다.
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  [MICROSOFT ADVERTISING SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)를 다운로드 하 여 설치 합니다. 설치 하는 데 몇 분 정도 걸릴 수 있습니다. 프로세스가 완료 될 때까지 대기 해야 합니다.

4.  Visual Studio를 다시 시작합니다.

5.  Microsoft Advertising SDK, 유니버설 Ad 클라이언트 SDK 또는 Microsoft Store Engagement 및 수익 화 SDK의 이전 버전에서 광고 라이브러리를 참조 하는 기존 프로젝트가 있는 경우 Visual Studio에서 프로젝트를 열고 프로젝트를 정리 하 고 다시 빌드해야 합니다 ( **솔루션 탐색기**에서 프로젝트 노드를 마우스 오른쪽 단추로 클릭 하 고 프로젝트 노드를 다시 마우스 오른쪽 단추로 클릭 한 다음 다시 **빌드** **를 선택**).

  그렇지 않고 프로젝트에서 처음으로 Microsoft Advertising SDK를 사용 하는 경우 [MICROSOFT ADVERTISING sdk에](#reference)대 한 참조를 추가할 준비가 된 것입니다.

<span id="install-nuget" />

## <a name="install-via-nuget"></a>NuGet을 통해 설치

NuGet을 통해 특정 UWP 프로젝트에 Microsoft Advertising SDK를 설치 하려면 다음을 수행 합니다.

1.  Visual Studio의 모든 인스턴스를 닫습니다.

2.  이전 버전의 Microsoft Advertising SDK, 유니버설 Ad 클라이언트 SDK, Ad Mediator 확장 또는 Microsoft Store Engagement 및 수익 화 SDK를 설치한 경우 이제 이러한 SDK 버전을 제거 합니다. 필요에 따라 **명령 프롬프트** 창을 열고 다음 명령을 실행 하 여 Visual Studio와 함께 설치 되었지만 컴퓨터의 설치 된 프로그램 목록에 표시 되지 않을 수 있는 이전 광고 SDK 버전을 정리 합니다.
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Visual Studio를 시작 하 고 Microsoft Advertising SDK를 사용 하려는 프로젝트를 엽니다.
    > [!NOTE]
    > 프로젝트에 이미 SDK의 이전 MSI 설치에서 라이브러리 참조가 포함 된 경우 프로젝트에서 이러한 참조를 제거 합니다. 이러한 참조는 이전 단계에서 참조 하는 라이브러리가 제거 되었으므로 옆에 경고 아이콘이 있습니다.

4. Visual Studio에서 **프로젝트** 및 **NuGet 패키지 관리**를 클릭 합니다.

5. 검색 상자에 **Microsoft** (xaml 프로젝트의 경우) 또는 **Microsoft.Advertising.JS** (JavaScript/HTML 프로젝트용)를 입력 하 고 해당 패키지를 설치 합니다. 패키지 설치가 완료 되 면 솔루션을 저장 합니다.
    > [!NOTE]
    > **출력** 창에서 지정 된 경로가 너무 긴 것을 나타내는 *설치 패키지* 오류를 보고 하는 경우 기본 위치 보다 짧은 경로를 사용 하 여 대체 위치에 패키지를 추출 하도록 NuGet을 구성 해야 할 수 있습니다. 이렇게 하려면 `repositoryPath` 컴퓨터의 nuget.config 파일에 값을 추가 하 고 NuGet 패키지를 추출할 수 있는 짧은 폴더 경로에 할당 합니다. 자세한 내용은 NuGet 설명서에서 [이 문서](https://docs.microsoft.com/nuget/consume-packages/configuring-nuget-behavior) 를 참조 하세요. 또는 더 짧은 경로를 사용 하 여 Visual Studio 프로젝트를 대체 폴더로 이동 하려고 할 수 있습니다.

6. 솔루션을 닫았다가 다시 엽니다.

7.  프로젝트에서 NuGet을 통해 설치 된 Microsoft Advertising SDK의 이전 버전에서 라이브러리를 이미 참조 하 고 프로젝트를 최신 버전의 SDK로 업데이트 한 경우 프로젝트를 정리 하 고 다시 빌드하는 것이 좋습니다. **솔루션 탐색기**에서 프로젝트 노드를 마우스 오른쪽 단추로 클릭 하 고 **정리**를 선택한 다음 프로젝트 노드를 다시 마우스 오른쪽 단추로 클릭 하 고 다시 **빌드**를 선택 합니다.

  그렇지 않고 프로젝트에서 처음으로 SDK를 사용 하는 경우 [MICROSOFT ADVERTISING sdk에](#reference)대 한 참조를 추가할 준비가 된 것입니다.

<span id="reference" />

## <a name="add-a-reference-to-the-microsoft-advertising-sdk"></a>Microsoft Advertising SDK에 대 한 참조 추가

Microsoft Advertising SDK를 설치한 후에는 이러한 지침에 따라 프로젝트에서 SDK를 참조 하 여 광고 Api를 사용할 수 있습니다.

1. Visual Studio에서 프로젝트를 엽니다.
    > [!NOTE]
    > 프로젝트가 **모든 CPU**를 대상으로 하는 경우 프로젝트를 업데이트 하 여 아키텍처 관련 빌드 출력 (예: **x86**)을 사용 합니다. 프로젝트가 **모든 CPU**를 대상으로 하는 경우에는 다음 단계에서 Microsoft Advertising SDK에 대 한 참조를 성공적으로 추가할 수 없습니다. 자세한 내용은 [프로젝트의 모든 CPU를 대상으로 하 여 발생 한 참조 오류](known-issues-for-the-advertising-libraries.md#reference_errors)를 참조 하세요.

2. **솔루션 탐색기**에서 **참조** 를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가** ...를 선택 합니다.

3. **참조 관리자**에서 **유니버설 Windows**를 확장 하 고 **확장**을 클릭 한 후 **Microsoft Advertising sdk for xaml** (xaml 앱의 경우) 또는 **Microsoft Advertising sdk** (javascript 및 HTML을 사용 하 여 빌드한 앱) 옆의 확인란을 선택 합니다.

4.  **참조 관리자**에서 확인을 클릭 합니다.

광고 Api를 사용 하 여 시작 하는 방법을 보여 주는 연습은 다음 문서를 참조 하세요.

* [중간 광고](interstitial-ads.md)
* [기본 광고](native-ads.md)
* [XAML 및 .NET의 AdControl](adcontrol-in-xaml-and--net.md)
* [HTML 5 및 Javascript의 AdControl](adcontrol-in-html-5-and-javascript.md)

<span id="framework" />

## <a name="understanding-framework-packages-in-the-microsoft-advertising-sdk"></a>Microsoft Advertising SDK의 프레임 워크 패키지 이해

[MICROSOFT ADVERTISING SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) (UWP 앱 용)의 Microsoft.Advertising.dll 라이브러리는 *프레임 워크 패키지로*구성 됩니다. 이 라이브러리에는 [microsoft](https://docs.microsoft.com/uwp/api/microsoft.advertising) 광고 및 [microsoft](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui) * 광고 네임 스페이스의 광고 api가 포함 되어 있습니다.

이 라이브러리는 프레임 워크 패키지 이므로 사용자가이 라이브러리를 사용 하는 앱 버전을 설치한 후 수정 및 성능 향상으로 라이브러리의 새 버전을 게시할 때마다 Windows 업데이트를 통해 장치에서이 라이브러리가 자동으로 업데이트 됩니다. 이렇게 하면 고객이 항상 사용 가능한 최신 버전의 라이브러리를 장치에 설치 하도록 할 수 있습니다.

이 라이브러리에서 새 Api 또는 기능을 도입 하는 새 버전의 SDK를 출시 하는 경우 해당 기능을 사용 하려면 최신 버전의 SDK를 설치 해야 합니다. 이 시나리오에서는 업데이트 된 앱을 스토어에도 게시 해야 합니다.
