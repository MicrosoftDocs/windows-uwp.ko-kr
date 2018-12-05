---
description: 앱을 업데이트하여 지원되는 최신 Microsoft 광고 라이브러리를 사용하고 앱이 배너 광고를 계속 받도록 하는 방법을 알아봅니다.
title: 배너 광고용 최신 광고 라이브러리로 앱 업데이트
ms.date: 08/23/2017
ms.topic: article
keywords: windows 10, uwp, 광고, 광고, AdControl, AdMediatorControl, 마이그레이션
ms.assetid: f8d5b2ad-fcdb-4891-bd68-39eeabdf799c
ms.localizationpriority: medium
ms.openlocfilehash: adac5cfdb1b4a10674fb7173e5b84a86b509f130
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8691967"
---
# <a name="update-your-app-to-the-latest-advertising-libraries-for-banner-ads"></a>배너 광고용 최신 광고 라이브러리로 앱 업데이트

2017년 4월 1일부터는 지원되지 않는 광고 SDK 릴리스를 사용하는 앱에 더 이상 배너 광고가 제공되지 않습니다. **AdControl**을 사용하여 유니버설 Windows 플랫폼(UWP) 앱에 배너 광고를 게시하는 경우 이 문서의 정보를 사용하여 지원되지 않는 광고 SDK를 사용하고 있지 않은지 확인하고, 지원되는 SDK로 앱을 마이그레이션하세요.

## <a name="overview"></a>개요

배너 광고를 게시하는 UWP 앱은 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)에 배포된 광고 라이브러리의**AdControl**을 사용해야 합니다. 이 SDK는 IAB(Interactive Advertising Bureau)의 [MRAID(Mobile Rich-media Ad Interface Definitions) 1.0 사양](http://www.iab.com/wp-content/uploads/2015/08/IAB_MRAID_VersionOne.pdf)를 통해 HTML5 리치 미디어를 서비스하는 기능을 포함하여 최소한의 광고 기능을 지원합니다. 많은 광고주가 이러한 기능을 원하고 있으며, 이에 따라 Microsoft는 광고주들에게 앱 에코시스템을 더욱 매력적으로 어필하고 개발자에게 더 많은 수익을 안길 수 있도록 앱 개발자에게 이러한 SDK 릴리스 중 하나를 의무적으로 사용하도록 요구하고 있습니다.

이 SDK가 릴리스되기 전, 이전 버전의 광고 SDK 릴리스를 통해 **AdControl** 클래스를 여러 차례 제공했습니다. 이러한 기존 광고 SDK 릴리스는 위에서 설명한 최소 광고 기능을 지원하지 않기 때문에 더 이상 지원되지 않습니다. 2017년 4월 1일부터는 지원되지 않는 광고 SDK 릴리스를 사용하는 앱에 더 이상 배너 광고가 제공되지 않습니다. 여전히 지원되지 않는 광고 SDK 릴리스를 사용하는 앱은 다음과 같은 동작을 보입니다.

* 앱의 **AdControl**에 배너 광고가 더 이상 제공되지 않을 것이며 해당 컨트롤에서 광고 수익을 얻지 못합니다.

* 앱의 **AdControl**이 새 광고를 요청하는 경우 컨트롤의 **ErrorOccurred** 이벤트가 발생하고 이벤트 인수의 **ErrorCode** 속성이 **NoAdAvailable** 값을 갖게 됩니다.

* 해당 앱과 관련된 모든 광고 단위가 비활성화됩니다. DePartnerv 센터 계정에서 이러한 비활성화 된 광고 단위를 제거할 수 없습니다. [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)를 사용하도록 앱을 업데이트하는 경우 이러한 광고 단위를 무시하고 새 광고 단위를 만드세요.

* 두 개 이상의 앱에서 사용되는 광고 단위에도 더 이상 배너 광고가 제공되지 않습니다. 따라서 각 광고 단위를 한 앱에만 사용해야 합니다.

**AdControl**을 사용하여 배너 광고를 표시하는 기존 앱(이미 Store에 있거나 아직 개발 중인 앱)이 있고 앱에서 어떤 광고 SDK가 사용되는지 잘 모르는 경우 이 문서의 지침을 따라 해당 앱을 지원되는 SDK로 업데이트해야 하는지 확인하세요. 문제가 발생하거나 지원이 필요할 경우 [고객 지원 센터에 문의](http://go.microsoft.com/fwlink/?LinkId=393643)하세요.

> [!NOTE]
> 앱이 이미 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)(UWP 앱용)를 사용하는 경우 앱을 더 변경할 필요가 없습니다.

## <a name="prerequisites"></a>필수 구성 요소

* **AdControl**을 사용하는 앱에 대한 전체 소스 코드 및 Visual Studio 프로젝트 파일입니다.
* 앱에 대한 .appx 패키지입니다.

> [!NOTE]
> 앱에 대한 .appx 패키지가 더 이상 없으나 Visual Studio 버전의 개발 컴퓨터가 있고 앱 빌드에 사용했던 광고 SDK가 있는 경우 Visual Studio에서 .appx 또는 .xap 패키지를 다시 생성할 수 있습니다.

<span id="part-1" />

## <a name="part-1-determine-whether-you-need-to-update-your-uwp-app"></a>1부: UWP 앱을 업데이트해야 하는지 여부 결정

다음 섹션의 지침에 따라 앱을 업데이트해야 하는지 결정합니다.

1. 원본을 손상시키지 않기 위해 앱용 .appx 패키지의 복사본을 만들고 .zip 확장명으로 이름을 바꾸고 파일의 압축을 풉니다.

2. 앱 패키지의 압축 푼 내용을 확인합니다.
  * Microsoft.Advertising.dll 파일이 있으면 앱이 이전 SDK를 사용하는 것이므로 아래 섹션의 지침에 따라 프로젝트를 업데이트해야 합니다. [2부](update-your-app-to-the-latest-advertising-libraries.md#part-2)를 계속 진행합니다.
  * Microsoft.Advertising.dll 파일이 없으면 UWP 앱이 사용 가능한 최신 광고 SDK를 이미 사용하고 있는 것이므로 프로젝트를 변경할 필요가 없습니다.


<span id="part-2" />

## <a name="part-2-install-the-latest-sdk"></a>2부: 최신 SDK 설치

앱이 이전 SDK 릴리스를 사용하는 경우 다음 지침에 따라 개발 컴퓨터에 최신 SDK가 있는지 확인하세요.

1. 개발 컴퓨터에 Visual Studio 2015 이상이 설치되어 있는지 확인하세요.
    > [!NOTE]
    > 개발 컴퓨터에서 Visual Studio가 열려 있으면 닫은 후 다음 단계를 수행해야 합니다.

1.  개발 컴퓨터에서 Microsoft Advertising SDK 및 Ad Mediator SDK의 모든 이전 버전을 제거합니다.

2.  **명령 프롬프트** 창을 열고 다음 명령을 실행하여 Visual Studio와 함께 설치되었을 수 있으나 컴퓨터에 설치된 프로그램 목록에 나타나지 않을 수 있는 SDK 버전을 모두 정리합니다.
    ```syntax
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)를 설치합니다.

## <a name="part-3-update-your-project"></a>3부: 프로젝트 업데이트

프로젝트에서 Microsoft Advertising 라이브러리에 대한 기존의 모든 참조를 제거하고 [이 지침](install-the-microsoft-advertising-libraries.md#reference)에 따라 필요한 참조를 추가합니다. 이렇게 하면 프로젝트에서 올바른 라이브러리가 사용될 수 있습니다. 기존 태그 및 코드를 유지할 수 있습니다.

## <a name="part-4-test-and-republish-your-app"></a>4단계: 앱 테스트 및 다시 게시

앱을 테스트하여 앱이 예상대로 배너 광고를 표시하는지 확인합니다.

이전 버전의 앱이 앱을 다시 게시할 파트너 센터에서 업데이트 된 앱에 대 한 [새 제출을 만들고](../publish/app-submissions.md) 스토어에서 이미 사용할 수 있습니다.
