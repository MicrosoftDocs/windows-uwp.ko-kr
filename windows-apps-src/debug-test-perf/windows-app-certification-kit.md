---
ms.assetid: 78D833B9-E528-4BCA-9C48-A757F17E6C22
title: Windows 앱 인증 키트
description: 앱이 Microsoft Store에 게시되거나 Windows Certified를 획득할 수 있는 가장 좋은 기회를 제공하려면 앱을 제출하여 인증을 받기 전에 로컬로 유효성을 검사하고 테스트합니다. 이 항목에서는 Windows 앱 인증 키트를 설치하고 실행하는 방법을 보여 줍니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, app certification
ms.localizationpriority: medium
ms.openlocfilehash: 4772edb9c99426396b7fa3a8734e2f45391c3a0f
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257828"
---
# <a name="windows-app-certification-kit"></a>Windows 앱 인증 키트



To get your app [Windows Certified](https://msdn.microsoft.com/windows/desktop/jj134964.aspx) or prepare it for [publication to the Microsoft Store](https://docs.microsoft.com/windows/uwp/publish/app-submissions), you should validate and test it locally first. This topic shows you how to install and run the [Windows App Certification Kit](https://msdn.microsoft.com/en-US/windows/apps/bg127575) to ensure your app is safe and efficient.

## <a name="prerequisites"></a>필수 구성 요소

유니버설 Windows 앱을 테스트하기 위한 필수 구성 요소:

-   You must install and run Windows 10.
-   You must install [Windows App Certification Kit version 10]( https://go.microsoft.com/fwlink/p/?LinkID=309666), which is included in the Windows Software Development Kit (SDK) for Windows 10.
-   [디바이스를 개발에 사용하도록 설정](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)해야 합니다.
-   테스트할 Windows 앱을 컴퓨터에 배포해야 합니다.

**A note about in-place upgrades**

최신 [Windows 앱 인증 키트]( https://go.microsoft.com/fwlink/p/?LinkID=309666)를 설치하면 컴퓨터에 설치되어 있는 이전 버전의 키트가 대체됩니다.

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-interactively"></a>대화식으로 Windows 앱 인증 키트를 사용하여 Windows 앱의 유효성 검사

1.  **시작** 메뉴에서 **앱**을 검색하고 **Windows 키트**를 찾은 다음 **Windows 앱 인증 키트**를 클릭합니다.

2.  Windows 앱 인증 키트에서 수행할 유효성 검사 범주를 선택합니다. 예를 들어 Windows 앱의 유효성을 검사할 경우 **Windows 앱 유효성 검사**를 선택합니다.

    테스트할 앱을 직접 찾아보거나 UI의 목록에서 앱을 선택할 수 있습니다. Windows 앱 인증 키트가 처음으로 실행되면 컴퓨터에 설치된 모든 Windows 앱이 UI에 나열됩니다. 그 이후에 실행되면 UI에 유효성을 검사한 최근 Windows 앱이 표시됩니다. 테스트할 앱이 목록에 표시되지 않는 경우 **내 앱이 나열되지 않음**을 클릭하여 시스템에 설치된 모든 앱의 포괄적인 목록을 가져올 수 있습니다.

3.  테스트할 앱을 입력하거나 선택한 후 **다음**을 클릭합니다.

4.  다음 화면에서 테스트할 앱 유형에 맞게 정렬된 테스트 워크플로가 표시됩니다. 테스트가 목록에서 회색으로 표시되면 테스트가 환경에 적용되지 않습니다. 예를 들어 Windows 7에서 Windows 10 앱을 테스트할 경우 정적 테스트만 워크플로에 적용됩니다. Note that the Microsoft Store may apply all tests from this workflow. 실행할 테스트를 선택하고 **다음**을 클릭합니다.

    Windows 앱 인증 키트에서 앱 유효성 검사를 시작합니다.

5.  테스트 후의 프롬프트에 테스트 보고서를 저장할 폴더 경로를 입력합니다.

    Windows 앱 인증 키트가 XML 보고서와 함께 HTML을 만들고 이 폴더에 저장합니다.

6.  보고서 파일을 열고 테스트 결과를 검토합니다.

> [!NOTE]
> If you're using Visual Studio, you can run the Windows App Certification Kit when you create your app package. 방법을 알아보려면 [UWP 앱 패키징](/windows/msix/package/packaging-uwp-apps)을 참조하세요.

 

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-from-a-command-line"></a>명령줄에서 Windows 앱 인증 키트를 사용하여 Windows 앱의 유효성 검사

> [!IMPORTANT]
> The Windows App Certification Kit must be run within the context of an active user session.

1.  명령 창에서 Windows 앱 인증 키트가 들어 있는 디렉터리로 이동합니다.

    **Note**   The default path is C:\\Program Files\\Windows Kits\\10\\App Certification Kit\\.

2.  테스트 컴퓨터에 이미 설치한 앱을 테스트하려면 다음 명령을 순서대로 입력합니다.

    `appcert.exe reset`

    `appcert.exe test -packagefullname [package full name] -reportoutputpath [report file name]`

    또는 앱을 설치하지 않은 경우 다음 명령을 사용할 수 있습니다. Windows 앱 인증 키트는 패키지를 열고 적절한 테스트 워크플로를 적용합니다.

    `appcert.exe reset`

    `appcert.exe test -appxpackagepath [package path] -reportoutputpath [report file name]`

3.  테스트가 완료되면 `[report file name]`이라는 보고서 파일을 열고 테스트 결과를 검토합니다.

**Note**  The Windows App Certification Kit can be run from a service, but the service must initiate the kit process within an active user session and cannot be run in Session0.

**Note**   For more info about the Windows App Certification Kit command line, enter the command `appcert.exe /?`

## <a name="testing-with-a-low-power-computer"></a>절전 컴퓨터를 사용하여 테스트

Windows 앱 인증 키트의 성능 테스트 임계값은 절전 컴퓨터의 성능을 기반으로 합니다.

테스트를 수행하는 컴퓨터의 특성이 테스트 결과에 영향을 줄 수 있습니다. To determine if your app’s performance meets the [Microsoft Store Policies](https://docs.microsoft.com/legal/windows/agreements/store-policies), we recommend that you test your app on a low-power computer, such as an Intel Atom processor-based computer with a screen resolution of 1366x768 (or higher) and a rotational hard drive (as opposed to a solid-state hard drive).

저성능 컴퓨터가 발전함에 따라 시간이 지나면 성능 특성이 변할 수도 있습니다. Refer to the most current [Microsoft Store Policies](https://docs.microsoft.com/legal/windows/agreements/store-policies) and test your app with the most current version of the Windows App Certification Kit to make sure that your app complies with the latest performance requirements.

## <a name="related-topics"></a>관련 항목

* [Windows App Certification Kit tests](windows-app-certification-kit-tests.md)
* [Microsoft Store 정책](https://docs.microsoft.com/legal/windows/agreements/store-policies)
 

 




